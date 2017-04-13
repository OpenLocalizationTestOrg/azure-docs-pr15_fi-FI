<properties
    pageTitle="Luo pyyntöjen API Management-palvelun avulla"
    description="Kutsua ulkoisia palveluja oman API API hallinta pyynnön ja vastauksen käytäntöjen avulla"
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


# <a name="using-external-services-from-the-azure-api-management-service"></a>Ulkoisten palvelujen Azure API Management-palvelun avulla

Käytettävissä Azure API hallintapalvelun käytäntöjä voi tehdä monenlaisia hyödyllistä työtä laskettuna saapuvan pyynnön, lähetettävän vastauksen ja peruskokoonpano tietojen perusteella. Kuitenkin käytäntöjä, että voit käsitellä ulkoisia palveluja API hallinnasta tulee Lisää monia mahdollisuuksia.

Olemme aiemmin tullut kuinka Microsoft käyttää [lokiin kirjaaminen, valvonnan ja analytics palvelun Azure tapahtumaa-toiminnossa](api-management-log-to-eventhub-sample.md). Tässä artikkelissa on esitetty käytäntöjä, jotta voit käsitellä kaikki ulkoisen HTTP-pohjainen palvelu. Käytännöt voidaan käynnistävä remote tapahtumia tai haetaan tietoja, joita käytetään käsitellä alkuperäisen pyynnön ja vastauksen jollakin tavalla.

## <a name="send-one-way-request"></a>Lähetä-yksi-tapa-pyyntö
Mahdollisesti helpoin ulkoisen vuorovaikutus on palomuuri ja unohdat tyylin pyyntö, jonka avulla jokin lisätoimi tärkeää tapahtumaa ilmoittaa ulkoiseen palveluun. Käytämme ohjausobjektin työnkulku käytännön `choose` esiintyvien mitä tahansa ehdon emme kiinnostunut, ja jos ehto täyttyy, emme voi Tee ulkoinen HTTP-pyyntö, [Lähetä-yksi-tapa-request](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) käytännön avulla. Tämä voi johtua sähköpostijärjestelmän, kuten Hipchat tai liukuma tai Mailia API, kuten SendGrid tai MailChimp pyyntö tai tärkeitä tuki tapaukset, tähän tapaan PagerDuty. Kaikki tekstiviesti järjestelmät on yksinkertainen HTTP-ohjelmointirajapinnan, emme voi helposti käynnistää.

### <a name="alerting-with-slack"></a>Ilmoitat ja liukuma
Seuraavassa esimerkissä kerrotaan, miten voit lähettää viestin liukuma keskusteluryhmän, jos HTTP-vastauksen tilakoodi on suurempi tai yhtä suuri 500. 500 aluevirhe kertoo Microsoftin Taustajärjestelmä API, tutustu API asiakas ei voi selvittää itse ongelma. Se vaatii yleensä jokin lisätoimi tämän osan toimia.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Liukuma on käsite saapuvien web koukut. Saapuvien web-koukku määritettäessä liukuma Luo määräten URL-osoite, jonka avulla voit tehdä yksinkertaisia POST-pyynnön ja voit siirtää viestin liukuma kanava. JSON-teksti, joka luodaan perustuu liukuma määrittämiä muodossa.

![Liukuman Web koukku](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>On palomuuri ja unohdat tarpeeksi hyvä?
On tiettyjä kompromissien fire ja unohdat tyylin pyynnön käytettäessä. Jos jostain syystä, kutsu epäonnistuu, ja valitse virheen ei ilmoiteta. Tässä tilanteessa monimutkaisuutta on toissijainen epäonnistui järjestelmän ja vastausta odotetaan suorituskykyä kustannusten ei käyttöön. Tilanteessa, jossa on tarkistaminen vastauksen [Lähetä pyyntö](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) käytäntö on sitten parempi vaihtoehto.

## <a name="send-request"></a>Lähetä pyyntö
`send-request` Käytäntö ottaa ulkoiseen palveluun käyttämällä suorittaa monimutkaisessa käsittelytoimintoihin ja tietojen hallinnan API-palvelu, joka voidaan myöhempää käsittelyä varten.

### <a name="authorizing-reference-tokens"></a>Viittaus tunnusten sallimisesta
Tärkeimmät funktion API hallinnan suojaa Taustajärjestelmä resurssit. Jos yhteyttä API käyttämä luvan palvelimen Luo [JWT tunnusten](http://jwt.io/) OAuth2 sen työnkulku osana kuin [Azure Active Directory](../active-directory/active-directory-aadconnect.md) - jälkeen voit käyttää `validate-jwt` käytännön tarkastaa tunnuksen. Jotkin todennus-palvelimet, luoda mitä kutsutaan [viittaustunnuksia](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , joka ei voi vahvistaa ilman, että puhelu luvan palvelimeen.

### <a name="standardized-introspection"></a>Standardoitu introspection
Aiemman on ollut standardoitu lainkaan tarkistetaan viittaus-tunnuksen todennus-palvelimen kanssa. Kuitenkin viimeksi ehdotettu Vakio [RFC 7662](https://tools.ietf.org/html/rfc7662) julkaisija IETF, joka määrittää, miten resurssin palvelimen tarkistaa tunnusta voimassaolo.

### <a name="extracting-the-token"></a>Poimii tunnus
Ensimmäiseksi on tunnuksen noutamiseen Authorization-otsikko. Muotoiltu otsikko-arvo `Bearer` luvan värimallin, yksi välilyönti ja luvan tunnuksen mukaan [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Valitettavasti on tapauksia, joissa todennus-värimallin jätetään pois. Asiakas tämän jäsennyksen, emme Jaa välilyönnillä otsikon-arvo ja valitse merkkijonon merkkijonot palautettavaa matriisia. Vaihtoehtoinen menetelmä säädetään muotoiltua luvan otsikot.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Pyynnön vahvistus
Kun on lupa tunnuksen, voimme kehittää pyynnön vahvistamiseen tunnuksen. RFC 7662 soittaa tämän prosessin introspection ja edellyttää, että olet `POST` introspection resurssin HTML-lomake. HTML-lomake on oltava avain/arvo-pari vähintään avaimella `token`. Pyyntö luvan palvelimeen myös voi todentaa varmistaa, että haittaohjelmien asiakkaiden ei voi siirtyä pyytää troolilla kelvollinen tunnusten varten.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Vastauksen tarkistaminen
`response-variable-name` Määritettä käytetään voi antaa käyttöoikeuden palautetut vastaus. Tämän ominaisuuden määritetyn nimen voidaan avaimeksi `context.Variables` sanaston käyttämään `IResponse` objekti.

Vastauksen objektista emme voi hakea tekstiin ja RFC 7622 kertoo us vastaus on oltava JSON-objekti ja on oltava vähintään yhden ominaisuuden `active` , joka on looginen arvo. Kun `active` on TOSI, ja valitse tunnus on voimassa.

### <a name="reporting-failure"></a>Virhe raportointi
Käytämme `<choose>` käytännön esiintyvien Jos tunnus on virheellinen, ja siinä tapauksessa palauttaa 401 vastaus.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

[RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) , jossa kuvataan mukaan kuinka `bearer` tunnusten käytetään myös palautettavien `WWW-Authenticate` 401 vastauksen otsikon. WWW-todennus on tarkoitus opasta oikein valtuutettujen pyyntö muodostamisesta asiakas. Tavoista mahdollisuuksiin OAuth2 framework erilaisia vuoksi on vaikeaa viestintä kaikki tarvittavat tiedot. Lähettäjä on tehokkuutta alettua jotta [asiakkaiden selvää, miten voit sallia oikein pyynnöt resurssin palvelimeen](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Lopullinen ratkaisu
Sovittaminen kaikki yhteen, emme tulee seuraavat käytäntö:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Tämä on vain yksi useita esimerkkejä siitä, miten `send-request` käytäntöä voidaan käyttää ulkoisia palveluita integroida prosessi, jossa vastaukset juoksutus API Management-palvelun kautta.

## <a name="response-composition"></a>Vastauksen yhdistelmä
`send-request` Käytännön voi käyttää parantaminen Taustajärjestelmä, ensisijainen pyyntö on tuli edellisessä esimerkissä, tai se voidaan käyttää valmiiksi korvaa Taustajärjestelmä puhelun varten. Avulla tällä menetelmällä voit helposti luoda koosteen resurssit, jotka yhdistetään useista eri järjestelmistä.

### <a name="building-a-dashboard"></a>Raporttinäkymien luominen   
Haluat Joskus voi näyttää tiedot, jotka sisältyvät esimerkiksi useiden Taustajärjestelmä järjestelmien, vaikuttavat Raporttinäkymät-ikkunan. Suorituskykyilmaisimet peräisin kaikki eri Edellinen-päättyy, mutta ei, jos haluat säätää suora pääsy ja se voisi olla hyvä, jos kaikki tiedot voitu hakea yksittäisen pyynnön. Joitakin Taustajärjestelmä tietoja tarvitsee ehkä joitakin viipaloiminen ja paloittelua ja ensin hieman sanitizing! Ei voi välimuistiin koosteen resurssin olla hyötyä vähentää Taustajärjestelmä kuormituksen tiedät, että käyttäjillä on vaan Opettele käyttämään toistuvien yritysten eston F5-näppäintä, jotta voit nähdä, jos niiden underperforming arvot voivat muuttua.    

### <a name="faking-the-resource"></a>Faking resurssi
Raporttinäkymät-ikkunan tämän resurssin rakentamiseen ensimmäiseksi on uusi toiminto paikallaan API hallinta publisher-portaalin. Tämä on käytettävä Microsoftin yhdistelmä käytännön muodostaa dynaamisen tämän resurssin paikkamerkki-toimintoa.

![Dashboard-toiminto](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Puhelujen pyynnöt
Kerran `dashboard` toiminto on luotu Microsoft voit määrittää käytännön, toimintoa varten. 

![Dashboard-toiminto](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Ensimmäiseksi on poimia minkä tahansa kyselyparametrit saapuvan pyynnön niin, että emme voi toimittaa Microsoftin Taustajärjestelmä. Tässä esimerkissä Microsoftin Raporttinäkymät-ikkunan on näkyvissä tiedot tietyn ajan mukaan on tämän vuoksi `fromDate` ja `toDate` parametrin. Microsoft voi käyttää `set-variable` käytännön tiedon noutamiseen pyyntö URL-osoite.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Kun tiedot on voimme kehittää pyynnöt Taustajärjestelmä järjestelmiin. Sivupyynnön muodostaa uusi URL-osoite parametrin tiedot ja soittaa sen vastaaviin palvelimen ja tallentaa vastauksen kontekstin muuttujana.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Nämä pyynnöt suorittaa järjestyksessä, jossa ei ole paras mahdollinen. Tulevien versiossa on voidaan esittely kutsutaan uuden käytännön `wait` , joka mahdollistaa kaikki nämä pyynnöt suorittaminen rinnakkain.

### <a name="responding"></a>Vastaa

Muodostaa koosteen vastauksen Käytämme [return-vastauksen](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) käytännön. `set-body` Elementin lausekkeen avulla voit luoda uuden `JObject` kaikki upotettu ominaisuuksina osan esitykset kanssa.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Koko käytännön näyttää seuraavasti:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

Paikkamerkin määrityksessä toiminto on määrittäminen Raporttinäkymät-ikkunan resurssin välimuistiin vähintään tuntia, koska Ymmärrämme tiedot laatu tarkoittaa, että myös, jos se on tunnin ajan tasalla, se on riittävän tehokkaita esittää tärkeitä tietoja käyttäjille.

## <a name="summary"></a>Yhteenveto
Azure API hallintapalvelun tarjoaa joustavia käytäntöjä, joilla voidaan valikoivasti HTTP-liikenne ja mahdollistaa Taustajärjestelmä palvelujen yhdistelmä. Haluatko parantaa API-yhdyskäytävän kanssa ilmoitat Funktiot, vahvistusta, vahvistus ominaisuuksia tai luoda uusia koosteen resursseja Taustajärjestelmä palveluihin, perusteella `send-request` ja liittyvät käytännöt Avaa maailman pikavalikon.

## <a name="watch-a-video-overview-of-these-policies"></a>Katso video yleiskatsaus käytännöt
Lisätietoja [Lähetä-yksi-tapa-pyyntö](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [Lähetä pyyntö](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)ja [Palaa vastauksen](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) käytännöt tämän artikkelin Katso seuraava video.

> [AZURE.VIDEO send-request-and-return-response-policies]
