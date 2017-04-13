<properties
   pageTitle="Ohje tietojen palvelun luomiseen Marketplacen | Microsoft Azure"
   description="Lisätietoja siitä, miten luominen, varmentaminen ja ottaa käyttöön tietojen palvelun hankkia Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="08/26/2016"
      ms.author="hascipio; avikova" />

# <a name="understanding-the-nodes-schema-for-mapping-an-existing-web-service-to-odata-through-csdl"></a>Rakenteen solmujen yhdistäminen aiemmin WWW-palvelun kautta CSDL OData perusteet

>[AZURE.IMPORTANT] **Tällä hetkellä onboarding olemme eivät ole enää mitään uusi tietopalvelu julkaisijat. Uusi dataservices ei Hae hyväksyä luettelo.** Jos haluat julkaista AppSource SaaS yrityssovelluksen löydät lisätietoja [tähän](https://appsource.microsoft.com/partners). Jos sinulla IaaS sovellusten tai haluat julkaista Azure Marketplace-Kehitystyökalut-palvelun löytyy lisätietoja [tästä](https://azure.microsoft.com/marketplace/programs/certified/).

Tämän asiakirjan avulla voidaan selkeyttää yhdistäminen OData-protokolla CSDL solmun rakenne. On tärkeää muistaa, että solmu rakenne on hyvin muodostettu XML. Pääkansio-, pää- ja aliluetteloista rakenne on niin sovellettavan suunniteltaessa OData-määritys.

## <a name="ignored-elements"></a>Ohitetut osat
Seuraavat kohteet korkean tason CSDL (XML-solmut), joita ei käytettävän Azure Marketplace-taustatietokannan WWW-palvelun metatietojen tuonnin aikana. Ne voivat olla käytössä, mutta ohitetaan.

| Osa | Laajuus |
|----|----|
| Elementin avulla | Solmun, alisolmut ja kaikki määritteet |
| Asiakirjat-elementti | Solmun, alisolmut ja kaikki määritteet |
| ComplexType | Solmun, alisolmut ja kaikki määritteet |
| Liitoksen elementti | Solmun, alisolmut ja kaikki määritteet |
| Lisäominaisuuden | Solmun, alisolmut ja kaikki määritteet |
| EntityContainer | Seuraavien määritteiden ohitetaan: *laajentaa* ja *AssociationSet* |
| Rakenne | Seuraavien määritteiden ohitetaan: *Namespace* |
| FunctionImport | Seuraavien määritteiden ohitetaan: *tila* (oletusarvo ln oletetaan) |
| Entiteettityypin | Vain seuraavat alisolmut ohitetaan: *avain* ja *PropertyRef* |

Seuraavassa kuvataan muutokset (lisätty ja ohittaa osat) eri CSDL XML-solmujen yksityiskohtaiset tiedot.

## <a name="functionimport-node"></a>FunctionImport solmu
FunctionImport-solmu edustaa yhtä URL (aloituskohtaa), joka paljastaa palvelu käyttäjälle. Solmun sallii kerrotaan, miten URL-osoite on osoitettu, mitkä parametrit ovat käytettävissä käyttäjälle ja miten nämä parametrit toimitetaan.

Lisätietoja tämän solmun on osoitteessa tähän [MSDNFunctionImportLink]

[MSDNFunctionImportLink]: (https://msdn.microsoft.com/library/cc716710 (v=vs.100).aspx)

Seuraavassa on muita määritteitä (tai määritteiden lisäykset), jotka ovat näyttämiä FunctionImport-solmu:

**d:BaseUri** -
URI-mallin, joka on määritetty Marketplace muiden resurssin. Marketplace käyttää mallia muodostaa kyselyitä, jotka perustuvat REST-verkkopalvelu. URI-mallissa on paikkamerkit parametrien {parameterName}-muodossa, missä parameterName parametrin nimi. Esimerkiksi apiVersion = {apiVersion}.
Parametrien sallitaan URI parametreiksi tai URI-polku osana. Ulkoasun polussa ne ovat aina pakollinen (ei voi merkitä kuin nullable). *Esimerkki:*`d:BaseUri="http://api.MyWeb.com/Site/{url}/v1/visits?start={start}&amp;end={end}&amp;ApiKey=3fadcaa&amp;Format=XML"`

**Nimi** - tuodun funktion nimi.  Ei voi olla sama kuin muut määritetyt nimet CSDL.  Esimerkiksi Nimi = "GetModelUsageFile"

**EntitySet** *(valinnainen)* - Jos funktio palauttaa kohde tiedostotyypit kokoelma **EntitySet** arvon on oltava kohde, johon kuuluu eikä kokoelman määrittäminen. Muussa tapauksessa **EntitySet** -määritettä ei saa käyttää. *Esimerkki:*`EntitySet="GetUsageStatisticsEntitySet"`

**Palautustyyppi** *(Valinnainen)* - määrittää osien palauttama URI.  Älä käytä tämän määritteen, jos funktio ei palauta arvoa. Tuetut tiedostotyypit ovat seuraavat:

 - **Sivustokokoelman (<Entity type name>)**: määrittää määritetty kohde tiedostotyypit-apuohjelmista. Nimi ei entiteettityypin solmun Name-määrite. Esimerkki on sivustokokoelman (WXC. HourlyResult).
 - **Raaka (<mime type>)**: määrittää raaka asiakirjan/blob, joka palautetaan käyttäjälle. Esimerkki on Raw(image/jpeg) muita Esimerkkejä:

  - ReturnType="Raw(text/plain)"
  - Palautustyyppi = "sivustokokoelman (sagen. DeleteAllUsageFilesEntity) "*

**d:Paging** - määrittää, miten sivutus käsitellään REST-resurssien mukaan. Parametriarvot käytetään sisällä Kaarevien braches, kuten sivun = {$page} & ItemsPerPage-arvo = {$size} käytettävissä olevat vaihtoehdot ovat:

- **Ei mitään:** ei ole sivutus ei käytettävissä
- **Ohita:** sivutus ilmaistaan looginen "Ohita" ja "Ota" (päällimmäisenä) kautta. Ohita kaikkien sellaisten viivanylitysten M elementtien yli ja ota palauttaa seuraavan N osat. Parametriarvo: $skip
- **Kestää:** Ota palauttaa seuraavan N osat. Parametriarvo: $take
- **PageSize:** sivutus ilmaistaan looginen sivu ja koko (kohteita sivulla) kautta. Sivun edustaa nykyisen sivun, joka palautetaan. Parametriarvo: $page
- **Kokoa:** koko edustaa jokaisella sivulla palautettavien kohteiden määrän. Parametriarvo: $size

**d:AllowedHttpMethods** *(Valinnainen)* - määrittää, mitkä verbin käsittelee LOPUT resurssi. Rajoittaa myös hyväksytyssä verbin määritetyllä arvolla.  Oletus = VIESTIIN.  *Esimerkki:* `d:AllowedHttpMethods="GET"` Käytettävissä olevat vaihtoehdot ovat:

- **HAE:** käytetään yleensä tietojen palauttaminen
- **Kirjaa:** käytetään yleensä Lisää uudet tiedot
- **Valitseminen:** käytetään yleensä tietojen päivittäminen
- **Poistaminen:** käytetään tietojen poistaminen

Lisää alisolmut (ei koske CSDL ohjeissa) sisällä FunctionImport solmun ovat seuraavat:

**d:RequestBody** *(Valinnainen)* - pyyntö tekstissä käytetään osoittamaan pyyntö odottaa lähetetään tekstissä. Parametrien voidaan kirjoittaa kokouspyynnön tekstiosassa. Ne on esimerkiksi Kaarevien sulkeiden sisällä ilmaistu {parameterName}. Nämä parametrit on yhdistetty-syöttötapa tekstiosaan, jotka siirretään sisältötoimittajan-palveluun. RequestBody elementti on määrite, jolla HttpMethod nimi-arvo. Määritteen avulla kaksi arvoa:

- **Kirjaa:** Jos kutsu on HTTP-VIESTIIN
- **HAE:** Jos kutsu on HTTP GET

    Esimerkki:

        `<d:RequestBody d:httpMethod="POST">
        <![CDATA[
        <req1:Request xmlns:r1="http://schemas.mysite.com//generic/requests/1" Version="1.0">
        <req1:Query>{Query}</req1:Query><req1:AppId>D453474</req1:AppId>
        <req:DestinationSchemas><req1:Schema>Generic.RequestResponse[1.0]</req1:Schema>
        </req1:DestinationSchemas>
        </req1: Request>
        ]]>
        </d:RequestBody>`

**d:Namespaces** ja **d:Namespace** - solmu kuvataan nimitilan, jotka on määritetty XML, joka palautetaan funktio tuo (URI päätepisteen). XML, joka palautetaan Taustajärjestelmä-palvelun saattaa sisältää nimitilan erottaa toisistaan sisältöön, joka palautetaan, jos minkä tahansa määrän. **Kaikki nämä nimitilan, jos d:Map tai d:Match XPath-kyselyiden on oltava lueteltuna.** D:Namespaces-solmu sisältää d:Namespace solmujen määrittäminen/luettelo. Jokainen niistä luettelo yksi nimitilan Taustajärjestelmä palvelun vastauksessa. Määrite d:Namespace solmun ovat seuraavat:

-   **d:Prefix:** Nimitilan komento XML palautettujen tulosten palvelun, kuten f:FirstName, f:LastName, jossa f on etuliite etuliite.
- **d:Uri:** Nimitilan tuloksen asiakirjan koko URI. On arvo, jonka etuliite on selvitetty suorituksen aikana.

**d:ErrorHandling** *(Valinnainen)* - solmu sisältää Virheenkäsittely ehdot. Kunkin ehdot tarkistetaan tulos, joka palautetaan sisältötoimittajan palvelu. Jos ehto on sama kuin ehdotetut HTTP-virhekoodin käyttäjä palauttaa virhesanoman.

**d:ErrorHandling** *(Valinnainen)* ja **d:Condition** *(valinnainen)* - ehto-solmu on yksi ehto, joka on kuitattu sisään sisältötoimittajan palvelun palauttama tulos. **Pakolliset** määritteet ovat seuraavat:

- **d:Match:** XPath-lauseke, joka tarkistaa, onko annetun solmun/arvo on sisältötoimittajan XML-tulosteessa. XPath-lauseke suoritetaan tulos ja tulee palauttaa arvon TOSI, jos ehto on EPÄTOSI tai vastine muulla tavoin.
- **d:HttpStatusCode:** HTTP-tilakoodin, joka on palautettava Marketplace tapauksessa ehto vastaa. Marketplace signalizes virheitä HTTP-tilakoodin käyttäjälle. HTTP-tilakoodin luettelo on osoitteessa http://en.wikipedia.org/wiki/HTTP_status_code
- **d:ErrorMessage:** Virhesanoma, joka palautetaan – HTTP-tilakoodin – käyttäjälle. Tämä on oltava virhesanoman helpossa muodossa, joka ei sisällä mitään tietoja.

**d:Title** *(Valinnainen)* - välilehdellä kuvaava otsikko-funktion. Otsikko-arvo on peräisin

- Valinnainen kartan määrite (xpath), joka määrittää löydät otsikko huoltopyyntö palauttama vastaus.
- Tai otsikko, joka on määritetty arvona solmun.

**d:Rights** *(Valinnainen)* - käyttöoikeuksiaan (kuten tekijänoikeuksien)-funktion kanssa. Oikeuksien arvo on peräisin:

- Valinnainen kartan määrite (xpath), joka määrittää löydät oikeudet huoltopyyntö palauttama vastaus.
-   Tai määritetty arvona solmu käyttöoikeudet.

**d:Description** *(Valinnainen)* - funktion lyhyt kuvaus. Kuvaus-arvo on peräisin

- Valinnainen kartan määrite (xpath), joka määrittää, johon etsiminen huoltopyyntö palauttama vastaus kuvauksen.
- - Tai – määritetty arvo solmun kuvaus.

**d:EmitSelfLink** - *on esimerkki yläpuolella "FunctionImport sivutuksen kautta palautti tietoja"*

**d:EncodeParameterValue** - valinnainen OData-laajennus

**d:QueryResourceCost** - valinnainen OData-laajennus

**d:Map** - valinnainen OData-laajennus

**d:Headers** - valinnainen OData-laajennus

**d:Headers** - valinnainen OData-laajennus

**d:Value** - valinnainen OData-laajennus

**d:HttpStatusCode** - valinnainen OData-laajennus

**d:ErrorMessage** - valinnainen OData-laajennus

## <a name="parameter-node"></a>Parametrin solmu

Tämä solmu edustaa yksi parametri, joka on määritetty osana URI-mallin / pyytää tekstissä, joka on määritetty FunctionImport-solmu.

Solmu-parametrin elementti"tietoja erittäin hyödyllinen tiedot asiakirjan sivulle löytyy [tähän](http://msdn.microsoft.com/library/ee473431.aspx) (käytä **Muu versio** avattavasta valikosta voit valita eri versiota, voit tarkastella niitä tarvittaessa)-palvelussa. *Esimerkki:*`<Parameter Name="Query" Nullable="false" Mode="In" Type="String" d:Description="Query" d:SampleValues="Rudy Duck" d:EncodeParameterValue="true" MaxLength="255" FixedLength="false" Unicode="false" annotation:StoreGeneratedPattern="Identity"/>`

| Parametrin määrite | Tarvitaan | Arvo |
|----|----|----|
| Nimi | Kyllä | Parametrin nimi. Sama kirjainkoko!  Kirjainkoon BaseUri. **Esimerkki:**`<Property Name="IsDormant" Type="Byte" />` |
| Tyyppi | Kyllä | Parametrityyppi. Arvon on oltava **EDMSimpleType** tai mallin laajuus sisältyvä monimutkaisia tyyppi. Lisätietoja on artikkelissa "6 tuetut parametrin/ominaisuuden tiedostotyypit".  (Kirjainkoko on merkitsevä! Ensimmäinen merkki on iso kirjain, muut ovat pienet kirjaimet)  Katso myös-[käsitteellinen mallin tyypit (CSDL)] [MSDNParameterLink]. **Esimerkki:**`<Property Name="LimitedPartnershipID " Type="Int32" />` |
| Tila | Ei | **In**-, tai InOut sen mukaan, onko parametrin syöte, tuloste tai i/parametri. (Vain "IN" ei käytettävissä Azure Marketplacesta.) **Esimerkki:**`<Parameter Name="StudentID" Mode="In" Type="Int32" />` |
| MaxLength | Ei | Suurin sallittu parametrin pituuden. **Esimerkki:**`<Property Name="URI" Type="String" MaxLength="100" FixedLength="false" Unicode="false" />` |
| Tarkkuus | Ei | Parametrin tarkkuus. **Esimerkki:**`<Property Name="PreviousDate" Type="DateTime" Precision="0" />` |
| Asteikko | Ei | Parametrin asteikon. **Esimerkki:**`<Property Name="SICCode" Type="Decimal" Precision="10" Scale="0" />` |

[MSDNParameterLink]: (http://msdn.microsoft.com/library/bb399548 (v=VS.100).aspx)

Attribuuteista, jotka on lisätty CSDL määrityksen ovat seuraavat:

| Parametrin määrite | Kuvaus |
|----|----|
| **d:regex** *(Valinnainen)* | Regex-lause, jota käytetään Vahvista parametrin arvo. Jos arvo ei vastaa lauseen arvo on hylätty. Näin voit määrittää myös mahdollisista arvoista, kuten ^ [0-9] +? $ päästää vain lukuja. **Esimerkki:** ' < parametrin nimi = "nimi" tilassa = "Tomaatti" tyyppi = "Merkkijonon" d: Nullable = "false" d:Regex = "^ [a-zA-Z] * $" d:Description = "A nimi, joka ei sisällä välilyöntejä tai alfa kuin englanninkielisessä merkkiä" d:SampleValues = "George|Teemu|Thomas|James "/ >" |
| **d:Enum** *(Valinnainen)* | Pystyviivaa eroteltu kelvollinen parametrin arvojen luettelo. Arvojen tyyppi on vastattava parametrin määritetyn tyypin. Esimerkki: "englanti|arvo|raaka`. Enum will display as a selectable dropdown list of parameters in the UI (service explorer). **Example:** `< parametrin nimi = "Kesto" tyyppi = "Merkkijonon" Mode = "Tomaatti" näytettäviä = "true" d:Enum = "1 vuosi|5years|10years "/ >" |
| **d: Nullable** *(Valinnainen)* | Sallii määrittäminen, voidaanko parametrin null. Oletusarvo on: TOSI. Parametrien, joka on määritetty osana URI-mallin polku et voi olla null. Kun määrite on määritetty epätosi näiden parametrien – käyttäjältä ohitetaan. **Esimerkki:**`<Parameter Name="BikeType" Type="String" Mode="In" Nullable="false"/>` |
| **d:SampleValue** *(Valinnainen)* | Esimerkki arvo esittämään muistiinpanon asiakkaan käyttöliittymässä.  Se on mahdollista lisätä useita arvoja käyttämällä putkien erotettu luettelo, eli "|b|c` **Example:** `< parametrin nimi = "BikeOwner" tyyppi = "Merkkijonon" Mode = "Tomaatti" d:SampleValues = "George|Teemu|Thomas|James "/ >" |

## <a name="entitytype-node"></a>Entiteettityypin solmu

Tämä solmu edustaa yhtä tyypit, jotka palautetaan peruskäyttäjän Marketplacesta. Se sisältää myös tulosteen, joka palauttaa arvot, jotka käyttäjä palautetaan sisältötoimittajan palvelun yhdistäminen.

Lisätietoja tämän solmun löytyy osoitteessa [tähän](http://msdn.microsoft.com/library/bb399206.aspx) (käyttö **Muu versio** avattavasta valikosta voit valita eri versiota, voit tarkastella niitä tarvittaessa)

| Määritteen nimi | Tarvitaan | Arvo |
|----|----|----|
| Nimi | Kyllä | Kohteen tyyppi nimi. **Esimerkki:**`<EntityType Name="ListOfAllEntities" d:Map="//EntityModel">` |
| Tyyppiä | Ei | Toisen kohteen tyyppi, joka on kantaluku kohteen tyyppi, joka on määritetty nimi. **Esimerkki:**`<EntityType Name="PhoneRecord" BaseType="dqs:RequestRecord">` |

Attribuuteista, jotka on lisätty CSDL määrityksen ovat seuraavat:

**d:Map** - palvelun tulosteen kohdistaa XPath-lauseke. Perustietojen tähän on, että palvelun tulos sisältää joukon, näkyvien elementtien kuin ATOM-syötteen where on joukko tapahtuma-solmut, toista. Kaikkien näiden toistuvan solmujen on yksi tietue. XPath-lauseke määritetään sitten Vie yksittäisiä toistuvan alisolmun korottaminen sisältötoimittajan palvelun tulos, joka sisältää arvot yksittäisestä tietueesta. Esimerkki tulostus-palvelusta:

        `<foo>
          <bar> … content … </bar>
          <bar> … content … </bar>
          <bar> … content … </bar>
        </foo>`

XPath-lauseke olla /foo/palkin, koska kunkin palkin solmu on tulos toistuvan solmu ja se sisältää asiakirjan sisältöä, joka palautetaan käyttäjälle.

**Avain** - määrite on ohittaa Marketplacesta. MUUT perusteella verkkopalvelut, yleinen ei Näytä perusavain.


## <a name="property-node"></a>Ominaisuuden solmu

Tämä solmu sisältää yhden ominaisuuden tietueen.

Lisätietoja tämän solmun on osoitteessa [http://msdn.microsoft.com/library/bb399546.aspx](http://msdn.microsoft.com/library/bb399546.aspx) (käyttö **Muu versio** avattavasta valikosta voit valita eri versiota, voit tarkastella niitä tarvittaessa) *Esimerkki:*
        `<EntityType Name="MetaDataEntityType" d:Map="/MyXMLPath">
        <Property Name="Name"   Type="String" Nullable="true" d:Map="./Service/Name" d:IsPrimaryKey="true" DefaultValue=”Joe Doh” MaxLength="25" FixedLength="true" />
        ...
        </EntityType>`

| AttributeName | Pakollinen | Arvo |
|----|----|----|
| Nimi | Kyllä | Ominaisuuden nimi. |
| Tyyppi | Kyllä | Ominaisuuden arvon tyyppi. Ominaisuuden arvon tyyppi on oltava **EDMSimpleType** tai monimutkaisia tyyppi (merkitty täydellinen nimi) laajuus mallin sisältyvä. Lisätietoja on artikkelissa käsitteellinen mallin tyypit (CSDL). |
| Tyypin Nullable | Ei | **Tosi** (oletusarvo) tai **False** sen mukaan, onko se voi olla null-arvoa. Huomautus: CSDL [http://schemas.microsoft.com/ado/2006/04/edm](http://schemas.microsoft.com/ado/2006/04/edm) nimitilan merkitty versiossa monimutkaisia tyypiksi on oltava näytettäviä = "False". |
| Oletusarvo | Ei | Oletusarvo-ominaisuuden. |
|MaxLength | Ei | Ominaisuuden arvo enimmäispituus. |
| FixedLength | Ei | **Tosi** tai **Epätosi** sen mukaan, onko ominaisuuden arvon tallennetaan fiexed merkkijonoksi. |
| Tarkkuus | Ei | Viittaa säilyttää numeroarvon numeroiden enimmäismäärä. |
| Asteikko | Ei | Säilyttää numeroarvon desimaalien määrä. |
| Unicode | Ei | **Tosi** tai **Epätosi** sen mukaan, onko ominaisuuden arvo on tallennettu Unicode-merkkijono. |
| Lajittelu | Ei | Merkkijono, joka määrittää, jota käytetään tietolähteeseen lajittelujärjestystä. |
| ConcurrencyMode | Ei | **Ei mitään** (oletusarvo) tai **Kiinteä**. Jos arvo on määritetty **Kiinteä**-ominaisuuden arvon käytetään optimistisen tarkistukset. |

Muita määritteitä, jotka on lisätty CSDL määrityksen ovat seuraavat:

**d:Map** - palvelun kohdistaa XPath-lauseke tulostus- ja purkaa yhden ominaisuuden tuloste. Määritettyä XPath-lauseke on toistuva solmu, joka on valittu entiteettityypin solmu XPath suhteessa. On myös mahdollista määrittää suora XPath sallimaan staattinen resurssin mukaan lukien kunkin tulosteen solmut, kuten esimerkiksi tekijänoikeuksien ilmoitus, jossa on vain kerran havaittu alkuperäisen palvelussa tulosteen vaikka sen pitäisi olla jokaiselle OData-tulostus riveillä. Esimerkki-palvelusta:

        `<foo>
          <bar>
           <baz0>… value …</baz0>
           <baz1>… value …</baz1>
           <baz2>… value …</baz2>
          </bar>
        </foo>`

XPath-lauseke tähän olisi ./bar/baz0 saat baz0 solmun sisältötoimittajan-palvelusta.

**d:CharMaxLength** - merkkijonotyypin, voit määrittää enimmäispituus. Katso DataService CSDL Esimerkki

**d:IsPrimaryKey** - ilmaisee, onko sarake perusavaimen/taulukkonäkymässä. Katso DataService CSDL esimerkkiä.

**d:isExposed** - määrittää, jos taulukko rakenne on määritetty (yleensä tosi). Katso DataService CSDL Esimerkki

**d:IsView** *(Valinnainen)* - arvon TOSI, jos tämä perustuu näkymän taulukon sijaan.  Katso DataService CSDL Esimerkki

**d:Tableschema** – Katso DataService CSDL Esimerkki

**d:ColumnName** - on taulukkonäkymään tai sarakkeen nimi.  Katso DataService CSDL Esimerkki

**d:IsReturned** - on totuusarvo, joka määrittää, jos palvelun paljastaa arvoksi asiakkaalle.  Katso DataService CSDL Esimerkki

**d:IsQueryable** - on totuusarvo, joka määrittää, jos tietokanta kyselyssä käytettävä sarake.   Katso DataService CSDL Esimerkki

**d:OrdinalPosition** - on sarakkeen numeeristen sijainnin ulkoasun, x, taulukon tai näkymän, jossa x on väliltä 1-taulukon sarakkeiden määrä.  Katso DataService CSDL Esimerkki

**d:DatabaseDataType** - on tietokannan sarakkeen tietotyypin eli SQL tietojen tyyppi. Katso DataService CSDL Esimerkki

## <a name="supported-parametersproperty-types"></a>Tuetut parametrit/ominaisuuden tyypit
Seuraavassa on Tuetut tiedostotyypit parametrit ja ominaisuudet. (Kirjainkoko on merkitsevä)

| Primitiivityyppejä | Kuvaus |
|----|----|
| Null | Vastaa arvoa puuttuminen |
| Totuusarvo | Edustaa binaaritiedosto arvon logiikan matemaattinen käsite|
| Tavu | Allekirjoittamaton 8-bittisen kokonaisluvun arvo|
|Päivämäärä ja aika| Edustaa päivämäärän ja kellonajan 12:00:00 keskiyöhön, tammikuussa 1 1753 A.D. vakiovalikoimasta arvoilla 11:59:59 P.M, joulukuuta 9999 A.D. kautta|
|Desimaaliluku | Edustaa numeeristen arvojen kiinteän ja asteikko. Tätä tyyppiä voidaan kuvata numeerinen arvo negatiivinen 10 ^ 255 + 1 positiivinen 10 ^ 255 -1|
| Kaksinkertainen | Irrallinen pisteen numero, jota käyttämällä voidaan esittää + 2.23e – 308 kautta 1, 79E + lähes solualueen arvojen 15 numeron tarkkuudella edustaa +308. **Käytä desimaalin Exel Vie ongelman vuoksi**|
| Yksittäisen | Irrallinen pisteen numero, jota käyttämällä voidaan esittää + 1.18e-38 kautta + 3, 40E lähes solualueen arvojen 7 numeroa tarkkuudella edustaa +38|
|GUID-tunnus |Edustaa 16 tavua (128-bittinen) yksilöllinen arvo |
|Int16|On allekirjoitettu 16-bittinen kokonaislukuarvo |
|Int32|On allekirjoitettu 32-bittinen kokonaislukuarvo |
|Int64|Vastaa arvoa allekirjoitetun 64-bittisen kokonaisluvun |
|Merkkijono | Kiinteä - tai vaihtuvan pituisissa merkki tietojen esittäminen |


## <a name="see-also"></a>Katso myös
- Jos olet kiinnostunut tietoja yleinen OData kartoituksen ja tarkoitus, lue artikkeli [Tietoja palvelun OData yhdistämisen](marketplace-publishing-data-service-creation-odata-mapping.md) tarkistettava määritykset, rakenteet ja ohjeita.
- Jos olet kiinnostunut tarkistaminen esimerkkejä, lue tämän artikkelin [Tiedot palvelun OData yhdistäminen esimerkkejä](marketplace-publishing-data-service-creation-odata-mapping-examples.md) otoksen koodi ja ymmärtää kenttäkoodin syntaksi ja kontekstia.
- Palaa määrätyn polun tietojen palvelun julkaiseminen Azure Marketplace-artikkeli [Tietoja palvelun julkaisun opas](marketplace-publishing-data-service-creation.md).
