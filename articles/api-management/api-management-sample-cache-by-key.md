<properties
    pageTitle="Mukautetun välimuistiin Azure API hallinta"
    description="Opettele välimuistiin kohteiden avaimella Azure API hallinta"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="custom-caching-in-azure-api-management"></a>Mukautetun välimuistin Azure API hallinta
Azure API hallintapalvelun on tukee [HTTP-vastauksen välimuistiin](api-management-howto-cache.md) käyttämällä resurssin URL-Osoitetta avaimeksi. Pyyntö ylätunnisteiden voi muokata avainta `vary-by` ominaisuudet. Tästä on hyötyä välimuistin koko HTTP-vastauksiin (eli esityksiä), mutta joskus kannattaa vain välimuistin esitys osa. Uuden [välimuistin-haku -](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) ja [välimuisti-kaupan-arvon](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) käytännöt tarjoavat voi tallentaa ja hakea tietoja käytäntöjen määritelmät haluamaansa laitteita. Tämä ominaisuus lisää myös arvon aiemmin käyttöön [Lähetä pyyntö](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) käytäntö sillä voi nyt välimuistiin ulkoisten palvelujen vastaukset.

## <a name="architecture"></a>Arkkitehtuuri  
API hallintapalvelun käyttää jaettua vuokraajan kohti tietovälimuistin niin, että kun skaalata enintään useita yksiköt näet edelleen käyttää samaan välimuistin tiedot. Kun käsittelet monille käyttöönoton välillä on kuitenkin riippumaton tallentaa kunkin alueet. Tämän vuoksi on tärkeää ei käsittele välimuistin tietosäilö, missä vain lähteen muistiinpanosivujesi tiedot. Jos teit ja päättäneet myöhemmin hyödyntää monille käyttöönoton, valitse asiakkaille, joilla käyttäjät, jotka siirtyvät ehkä voi enää käyttää välimuistiin tallennettuja tietoja.

## <a name="fragment-caching"></a>Fragmentti välimuistiin tallentaminen
Tietyissä tapauksissa, jossa vastaukset palautetaan sisältävät tietoja, jotka on kallista määrittämiseen ja vielä pysyy ajan tasalla paljon aikaa osa on. Esimerkkinä harkitse palvelun muodostama lentoyhtiön, joka sisältää tiedot lento varauksia, lento tila jne. Jos käyttäjä kuuluu lentoyhtiöiden points-ohjelma, ne on myös niiden nykyinen tila ja kumulatiivisen matka koskevat tiedot. Käyttäjiin liittyvät tiedot on ehkä tallennettu eri järjestelmään, mutta se voi olla olisi sisällytettävät vastaukset palautetaan lento tila ja varaukset. Tämän voi tehdä kutsutaan fragmentti välimuistiin menetelmällä. Ensisijainen esitys voidaan palauttaa alkuperäisestä palvelimesta osoittavat, missä käyttäjiin liittyvät tiedot on lisättävä jokin lisätoimi tunnuksen avulla. 

Ota huomioon seuraavat taustassa API JSON vastaus.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

Ja toissijainen resurssin `/userprofile/{userid}` , että näyttää seuraavalta,

     { "username" : "Bob Smith", "Status" : "Gold" }

Määrittää käyttäjätietoja annettava nimettävä, kuka käyttäjä. Se on riippuvainen toteutus. Esimerkkinä käytän `Subject` väittää `JWT` suojaustunnuksen. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Tämä tallennetaan `enduserid` arvo kontekstissa muuttujaan myöhempää käyttöä varten. Seuraavaksi voit selvittää, jos edelliseen pyyntö jo noudettu käyttäjätietoja ja välimuistiin tallennettujen. Tämän ominaisuuden Käytämme `cache-lookup-value` käytännön.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Jos ei ole, joka vastaa avainarvon ja sitten ei välimuistin `userprofile` kontekstin muuttujan luodaan. Haku käyttämällä onnistuu tarkistetaan `choose` hallita työnkulku käytännön.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Jos `userprofile` kontekstin muuttujan ei ole ja valitse HTTP-pyyntö, voit hakea ne tarvitse tarkastellaan.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Käytämme `enduserid` muodostaa käyttäjän profiiliin resurssin URL-osoite. Kun olemme vastauksen, on erotettu leipäteksti vastauksen ulos ja tallenna se takaisin konteksti-muuttuja.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Voit tehdä tämän HTTP-pyyntö uudelleen, kun sama käyttäjä muokkaa toisen pyynnön ottaa meihin välttämiseksi voidaan tallentaa käyttäjäprofiilin välimuistin.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Arvo tallennetaan välimuistin, joka on alun perin yritetään noutaa sen täsmälleen saman avaimen avulla. Kesto, joka on valittava tallentaa arvon perusteella siitä, miten usein muuttuvat ja miten varmatoimisempia käyttäjät ovat olevaan ajan tasalla tiedot. 

On tärkeää huomaat, että haetaan välimuistista on edelleen, prosessi, verkko-pyynnön ja lähimpään kymmeneen millisekuntia voit lisätä mahdollisesti edelleen pyynnön. Mitä etuja ovat peräisin määritettäessä käyttäjäprofiilin tiedot on huomattavasti pidempi kuin vuoksi hänen nimeään tarvitse lisätä tietokannan kyselyt tai useita Edellinen-päät kooste tiedot.

Viimeinen vaihe on palautettu vastauksen päivitetään Microsoftin käyttäjäprofiilin tietoja.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Minulla on jättää lainausmerkkejä tunnuksen niin, että myös silloin, kun korvaava ei toteudu, vastaus on edelleen voimassa JSON. Tämä on ensisijaisesti tekemään virheenkorjaus helpottuu.

Kun yhdistät nämä vaiheet yhdessä, lopputulos on käytännön, joka näyttää yksi.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Tämän välimuistiin menetelmän käytetään ensisijaisesti sivustojen kohtaa, johon HTML koostuu palvelinpuolen, jotta se voi muodostaa yhdelle sivulle. Se myös voi kuitenkin hyötyä API, jossa asiakkaat eivät asiakkaan puoli HTTP välimuistin tai kannattaa ei, jos haluat sijoittaa kyseisen vastuu asiakkaan.

Tämä samanlaisia fragmentti välimuistiin voidaan tehdä myös käyttämällä Redis.txt, välimuistiin palvelimen Taustajärjestelmä web-palvelimissa, kuitenkin toimisivat API Management-palvelun avulla on hyödyllinen, kun välimuistissa osat ovat peräisin eri Edellinen-päät kuin ensisijaisen vastaukset.

## <a name="transparent-versioning"></a>Läpinäkyvä versiotiedot
Se on yleisin tapa useita eri käyttöönoton versioiden Ohjelmointirajapinnan aiotaan lisätä tiettynä hetkenä. Tämä on ehkä tukemaan eri ympäristöissä, kuten keskihajonta, Testaa, tuotannon, tai se voi olla tukemaan Ohjelmointirajapinnan antaa aika Ohjelmointirajapinta kuluttajille siirtämään uudempia versioita. 

Yksi tapa käsittely tämä sijaan edellyttävän asiakkaan kehittäjät voivat muuttaa URL-osoitteet- `/v1/customers` , `/v2/customers` on tallennuspaikoista kuluttaja profiilitiedot version Ohjelmointirajapinnan ne tällä hetkellä haluat käyttää sekä kutsua haluamasi Taustajärjestelmä URL-osoite. Jos haluat määrittää tietyn asiakkaan kutsua oikean Taustajärjestelmä URL-Osoitteen, on kyselyn määritystietoja. Määritysten nämä tiedot välimuistiin, on pienentää suorituskykyä-huomattavasti siitä valinta.

Ensimmäiseksi on määrittää tunnus, jolla voit määrittää haluamasi versio. Tässä esimerkissä minulla on liittää version product key-tilaus. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Olemme Tee välimuistin hakua, voit tarkastella, jos on jo on haettu haluamasi asiakasohjelmaversion.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Valitse Microsoft Tarkista Jos ei löytynyt se välimuistin.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Jos emme ole on go ja hakea sitä.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Pura vastauksen teksti vastaus.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Tallentaa sen myöhempää käyttöä varten välimuistin takaisin sisään.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

Ja Päivitä lopuksi voit valita haluamasi asiakkaan palvelun version taustatietokannan URL-osoite.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Kokonaan-käytäntö määritetään seuraavalla tavalla.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Tyylikäs ratkaisu, joka korjaa useita API versiotietojen koskee on API kuluttajille ohjaamaan läpinäkyvä taustatietokannan version käyttää asiakkaat eikä sinun tarvitse Päivitä ja ota uudelleen asiakkaiden ottaminen käyttöön.

## <a name="tenant-isolation"></a>Vuokraajan eristystaso

Jotkin yritykset Luo suurempi, usean vuokraajan käyttöönotoissa erillisten ryhmien omistajien eri versioiden Taustajärjestelmä laite. Tämä pienentää asiakkaat, jotka ovat vaikuttaneet laitteisto-ongelma taustassa määrä. Ottaa käyttöön myös vaiheittain esiin ohjelmistoversiot. Tämä taustatietokannan arkkitehtuuri pitäisi olla läpinäkyvä API kuluttajille. Tämä onnistuu samalla tavalla kuin läpinäkyvä versiotietojen, koska se perustuu samaa menetelmää vaihdetaan-osan käyttäminen määritysten tilaa kohden Ohjelmointirajapinnan avain Taustajärjestelmä URL-osoite.  

Sen sijaan, että kunkin tilauksen avaimen Ohjelmointirajapinnan Ensisijainen versio, joka palauttaa, palauttavat tunnus, joka liittyy palvelutili määritetyt laitteisto-ryhmään. Tunnuksen voidaan muodostaa tarvittavat Taustajärjestelmä URL-osoite.

## <a name="summary"></a>Yhteenveto
Luvun käyttämään Azure API hallinta välimuistin projektiin kaikenlaisia tietoja mahdollistaa tehokas käytön määritystietoja, jotka voivat vaikuttaa saapuva pyyntö käsitellään. Se voidaan myös tietojen osat, jotka voivat verkkotunnistetietojen vastaukset palautetaan taustasta API tallentamiseen.

## <a name="next-steps"></a>Seuraavat vaiheet
Kirjoita Anna palautetta Disqus keskusteluketjun tämän aiheen Jos argumentissa on muissa tilanteissa, kyseiset käytännöt on otettu käyttöön, tai jos mahdollista, jotka haluat saavuttaa, mutta älä koe ovat tällä hetkellä mahdollista.
