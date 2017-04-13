<properties
    pageTitle="Voit suojata verkko-Ohjelmointirajapinnan taustassa ja Azure Active Directory-Ohjelmointirajapinnan hallinta"
    description="Opettele suojaa verkko-Ohjelmointirajapinnan taustassa ja Azure Active Directory-Ohjelmointirajapinnan hallinta." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-protect-a-web-api-backend-with-azure-active-directory-and-api-management"></a>Voit suojata verkko-Ohjelmointirajapinnan taustassa ja Azure Active Directory-Ohjelmointirajapinnan hallinta

Seuraavassa videossa näytetään, miten verkko-Ohjelmointirajapinnan taustassa kokoaminen ja suojata OAuth 2.0-protokollan avulla ja Azure Active Directory-Ohjelmointirajapinnan hallinta.  Tässä artikkelissa on yleiskatsaus ja lisätietoja videon ohjeita. 24 minuuttia Tässä videossa kerrotaan, miten voit:

-   Muodosta verkko-Ohjelmointirajapinnan taustassa ja AAD - 1:30 aloittaen suojaaminen
-   Tuoda Ohjelmointirajapinnan API hallinta - aloittaen 7 / 10
-   Määritä Soita Ohjelmointirajapinnan – 9:09 aloittaen Developer-portaalissa
-   Soita API - 18:08 aloittaen työpöytäsovellus määrittäminen
-   Hyväksy ennalta pyynnöt - 20:47 aloittaen JWT kelpoisuuden käytännön määrittäminen

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## <a name="create-an-azure-ad-directory"></a>Azure AD-kansion luominen

Verkko-Ohjelmointirajapinnan suojaamiseen palautettua täytyy ensin Azure Active Directoryn avulla AAD vuokraajan. Tässä videossa käytetään nimeltä **APIMDemo** palvelutili. Luo AAD vuokraajan kirjautuminen [Azure perinteinen Portal](https://manage.windowsazure.com) ja valitse **Uusi**->**Sovelluksen Services**->**Active Directory**->**Directory**->**Mukautettu luominen**. 

![Azure Active Directory][api-management-create-aad-menu]

Tässä esimerkissä **APIMDemo** hakemiston luodaan oletustoimialueen, nimeltä **DemoAPIM.onmicrosoft.com**. Tähän kansioon käytetään videon koko.

![Azure Active Directory][api-management-create-aad]

## <a name="create-a-web-api-service-secured-by-azure-active-directory"></a>Luo Azure Active Directory-suojattua verkko-Ohjelmointirajapinnan palvelu

Tässä vaiheessa verkko-Ohjelmointirajapinnan taustassa luodaan Visual Studio 2013: n avulla. Tässä vaiheessa videon alkaa 1:30. Voit luoda verkko-Ohjelmointirajapinnan Taustajärjestelmä projektin Visual Studiossa valitsemalla **Tiedosto**->**Uusi**->**projektin**, ja valitse **ASP.NET Web-sovelluksen** **Web** -mallit-luettelosta. Tässä videossa projektin nimi on **APIMAADDemo**. Valitse **OK** , jos haluat luoda projektin. 

![Visual Studio][api-management-new-web-app]

Valitse **Valitse malli-luettelosta** Luo verkko-Ohjelmointirajapinnan projekti- **Verkko-Ohjelmointirajapinnan** . Määritä Azure kansion käyttöoikeuksien valitsemalla **Muuta todennusta**.

![Uusi projekti][api-management-new-project]

Valitse **Organisaation tili**ja määrittää **toimialueen** AAD vuokraajan. Tässä esimerkissä toimialue on **DemoAPIM.onmicrosoft.com**. Hakemiston toimialueen saadaan hakemistossa **Domains** -välilehteä.

![Toimialueet][api-management-aad-domains]

Määritä haluamasi asetukset **Käyttöoikeuksien muuttaminen** -valintaikkunassa ja valitse **OK**.

![Käyttöoikeuksien muuttaminen][api-management-change-authentication]

Kun valitset **OK** Visual Studio yrittää rekisteröidä sovelluksen Azure Active Directory-hakemistosta ja sinua voidaan pyytää kirjautumaan Visual Studio mukaan. Kirjaudu sisään käyttämällä järjestelmänvalvojan tilin hakemistossa.

![Visual Studio kirjautuminen][api-management-sign-in-vidual-studio]

Jos haluat määrittää tämän projektin isännän **pilveen** Azure verkko-Ohjelmointirajapinnan-Valitse valintaruutu ja valitse sitten **OK**.

![Uusi projekti][api-management-new-project-cloud]

Sinua voidaan pyytää kirjautumaan Azure ja määritä sitten Web Appissa.

![Asetusten määrittäminen][api-management-configure-web-app]

Tässä esimerkissä nimeltä **APIMAADDemo** uuden **sovelluksen palvelusopimus** on määritetty.

Valitse **OK** Web App-sovelluksen määrittäminen ja luo sitten projekti.

## <a name="add-the-code-to-the-web-api-project"></a>Lisää verkko-Ohjelmointirajapinnan projektin koodi

Videon seuraavaan vaiheeseen Lisää verkko-Ohjelmointirajapinnan projektin koodi. Tämä vaihe 4:35 alkaa.

Tässä esimerkissä verkko-Ohjelmointirajapinnan toteuttaa basic Laskimen palvelun mallia ja ohjain. Voit lisätä mallin palvelun **Ratkaisunhallinnassa** **Mallit** kakkospainikkeella ja valitse **Lisää**- **luokka**. Luokan nimeäminen `CalcInput` ja sitten **Lisää**.

Lisää seuraava `using` lauseen yläreunaan `CalcInput.cs` tiedosto.

    using Newtonsoft.Json;

 Korvaa luodun luokan seuraava koodi.

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

Napsauta **Ratkaisunhallinnassa** **ohjaimet** hiiren kakkospainikkeella ja valitse **Lisää**->**ohjauskoneen**. Valitse **Verkko-Ohjelmointirajapinnan 2-ohjain - Tyhjennä** ja sitten **Lisää**. Kirjoita nimi ohjauskoneen **CalcController** ja sitten **Lisää**.

![Lisää ohjain][api-management-add-controller]

Lisää seuraava `using` lauseen yläreunaan `CalcController.cs` tiedosto.

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

Korvaa seuraava koodi luotu controller-luokka. Tämä koodi toteuttaa `Add`, `Subtract`, `Multiply`, ja `Divide` Basic Laskimen Ohjelmointirajapinnan toimintoja.

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }
    }

Paina **F6** voivat laatia ja tarkista ratkaisun.

## <a name="publish-the-project-to-azure"></a>Projektin julkaiseminen Azure

Tässä vaiheessa Visual Studio projekti julkaistaan Azure. Videon tämä vaihe 5:45 alkaa.

Projektin julkaiseminen Azure- **APIMAADDemo** projektia Visual Studiossa hiiren kakkospainikkeella ja valitse **Julkaise**. Pidä oletusasetukset **Julkaise** -valintaikkunassa ja valitse sitten **Julkaise**.

![Web-julkaisu][api-management-web-publish]

## <a name="grant-permissions-to-the-azure-ad-backend-service-application"></a>Azure AD-taustatietokannan palvelusovelluksen käyttöoikeuksia

Uuden sovelluksen Taustajärjestelmä palvelun luodaan verkko-Ohjelmointirajapinnan projektin määrittäminen ja julkaisun yhteydessä Azure AD-kansiossa. Tässä vaiheessa 6:13, aloittaen videon käyttöoikeudet myönnetään verkko-Ohjelmointirajapinnan Taustajärjestelmä.

![Sovelluksen][api-management-aad-backend-app]

Valitse sovelluksen määrittämiseen tarvittavat oikeudet nimi. Siirry **Määritä** -välilehti ja vieritä **muiden sovellusten käyttöoikeudet** -osaan. Valitse **Sovelluksen käyttöoikeudet** avattavan luettelon vieressä **Windows** **Azure Active Directory**, **directory tietojen**valintaruutu ja valitse **Tallenna**.

![Lisää käyttöoikeuksia][api-management-aad-add-permissions]

>[AZURE.NOTE] Jos **Windows** **Azure Active Directory** ei näy luettelossa muiden sovellusten käyttöoikeudet, valitse **Lisää sovellus** ja lisää se luettelosta.

Pane merkille **Sovelluksen tunnus URI** käytettäväksi seuraavassa vaiheessa, kun Azure AD-sovellus on määritetty Ohjelmointirajapinta hallinta developer-portaalissa.

![Sovellustunnus URI][api-management-aad-sso-uri]

## <a name="import-the-web-api-into-api-management"></a>Verkko-Ohjelmointirajapinnan tuominen API hallinta

Ohjelmointirajapinnan on määritetty API publisher-portaalista, jossa Azure perinteinen portaalin kautta. Publisher-portaalin saavuttamiseksi Valitse Azure perinteinen portaalissa API Management-palvelun **hallinta** . Jos et ole luonut erillisen service API hallinta-kohdassa [Hallitse ensimmäisen API][] -opas [Luo erillisen service API hallinta][] .

![Publisher-portaalissa][api-management-management-console]

Toimintoja voidaan [lisätä API manuaalisesti](api-management-howto-add-operations.md)tai ne on tuotu. Tässä videossa toimintojen tuodaan aloittaen 6:40 Swagger-muodossa.

Luo tiedosto nimeltä `calcapi.json` seuraavia sisältö ja tallenna se tietokoneeseen. Varmistaa, että `host` määrite pisteiden verkko-Ohjelmointirajapinnan Taustajärjestelmä. Tässä esimerkissä `"host": "apimaaddemo.azurewebsites.net"` käytetään.

{"swagger": "2.0", "tiedot": {"otsikko": "Laskimen", "kuvaus": "Arithmetics http!", "version": "1.0"}, "isäntä": "apimaaddemo.azurewebsites.net", "peruspolku": "/ api", "kalastelulta": ["http"] "polut": {"/ lisääminen? = {a} ja b = {b}": {"Hanki-: {"kuvaus":"Vastaa kanssa summan kertoman kahden numerot.","toimintotunnukseksi":"Lisää kahden kokonaisluvut","parametrit": [{"nimi":"a","tomaatti":"kyselyn","kuvaus":"ensimmäinen operandi. Oletusarvo on <code>51</code>. ","tarvittavat": TOSI,"oletus":"51","luettelo": ["51"]}, {"nimi":"b";"tomaatti":"kyselyn","kuvaus":"toisen operandi. Oletusarvo on <code>49</code>. ","tarvittavat": TOSI,"oletus":"49","luettelo": ["49"]}],"vastaukset": {}}}," / sub?a = {a} & b = {b} ": {"Hanki-: {"kuvaus": "Vastaa kanssa esimerkiksi ero kaksi lukujen.", "toimintotunnukseksi": "Erota kaksi kokonaisluvut", "parametrit": [{"nimi": "a", "tomaatti": "kyselyn", "kuvaus": "ensimmäinen operandi. Oletusarvo on <code>100</code>. ","pakollisen": TOSI,"oletus":"100","luettelo": ["100"]}, {"nimi":"b";"tomaatti":"kyselyn","kuvaus":"toisen operandi. Oletusarvo on <code>50</code>. ","tarvittavat": TOSI,"oletus":"50","luettelo": ["50"]}],"vastaukset": {}}}," / div?a = {a} & b = {b} ": {"Hanki-: {"kuvaus": "Vastaa ja osamäärä kahden numerot.", "toimintotunnukseksi": "Jakaa kahden kokonaisluvun", "parametrit": [{"nimi": "a", "tomaatti": "kyselyn", "kuvaus": "ensimmäinen operandi. Oletusarvo on <code>100</code>. ","pakollisen": TOSI,"oletus":"100","luettelo": ["100"]}, {"nimi":"b";"tomaatti":"kyselyn","kuvaus":"toisen operandi. Oletusarvo on <code>20</code>. ","tarvittavat": TOSI,"oletus":"20","luettelo": ["20"]}],"vastaukset": {}}}," / mul?a = {a} & b = {b} ": {"Hanki-: {"kuvaus": "Vastaa tuotteeseen, kahden numerot.", "toimintotunnukseksi": "Kerro kahden kokonaisluvun", "parametrit": [{"nimi": "a", "tomaatti": "kyselyn", "kuvaus": "ensimmäinen operandi. Oletusarvo on <code>20</code>. ","tarvittavat": TOSI,"oletus":"20","luettelo": ["20"]}, {"nimi":"b";"tomaatti":"kyselyn","kuvaus":"toisen operandi. Oletusarvo on <code>5</code>. ","tarvittavat": TOSI,"oletus":"5","luettelo": ["5"]}],"vastaukset": {}}}}}

API-Laskin tuotava valitsemalla **ohjelmointirajapinnan** **API hallinta** vasemmalla puolella ja valitse sitten **Tuo API**.

![Tuo API-painike][api-management-import-api]

Seuraavien toimien Laskimen API määrittämiseen.

1. Valitse **tiedostosta**, valitse `calculator.json` tallennettu tiedosto ja valitse **Swagger** -valintanappi.
2. Kirjoita **Laskenta** **API WWW-URL-osoite liite** -ruutu.
3. **Tuotteet (valinnainen)** -ruutuun ja valitse **Starter**.
4. Valitse Tuo Ohjelmointirajapinnan **Tallenna** .

![Lisää uusi Ohjelmointirajapinta][api-management-import-new-api]

Kun Ohjelmointirajapinnan on tuotu, Ohjelmointirajapinnan Yhteenveto-sivulla näkyy publisher-portaalissa.

## <a name="call-the-api-unsuccessfully-from-the-developer-portal"></a>Puhelun Ohjelmointirajapinnan mutta epäonnistuu developer-portaalissa

Tässä vaiheessa Ohjelmointirajapinnan on tuotu Ohjelmointirajapinta hallinta, mutta se ei ole vielä voi kutsua onnistuneesti developer-portaalissa koska Taustajärjestelmä-palvelu on suojattu Azure AD-todennuksen. Tämä on selitetty videon aloittaen 7:40 seuraavien ohjeiden mukaisesti.

Valitse oikeassa reunasta publisher-portaalin **Developer-portaalissa** .

![Developer-portaalissa][api-management-developer-portal-menu]

**API** ja valitse sitten **Laskimen** API.

![Developer-portaalissa][api-management-dev-portal-apis]

Valitse **Kokeile**.

![Kokeile][api-management-dev-portal-try-it]

Valitse **Lähetä** , ja tarkista **401 ei oikeuksia**vastauksen tila.

![Lähetä][api-management-dev-portal-send-401]

Pyyntö ei ole, koska Taustajärjestelmä Ohjelmointirajapinta on suojattu Azure Active Directory. Ennen kutsumista onnistuneesti Ohjelmointirajapinnan kehittäjä portaalissa on oltava määritettynä sallivat ohjelmistokehittäjille, jotka käyttävät OAuth 2.0. Tätä prosessia on kuvattu seuraavissa osissa.

## <a name="register-the-developer-portal-as-an-aad-application"></a>Rekisteröi AAD sovelluksen developer-portaalissa

Ensimmäinen vaihe määrittäminen sallivat ohjelmistokehittäjille, jotka käyttävät OAuth 2.0 developer-portaalissa on rekisteröidä developer-portaalissa AAD sovelluksena. Tämä on selitetty 8:27 videon aloittaen.

Siirry Azure AD-vuokraajan Tämä video, tässä esimerkissä **APIMDemo** ensimmäiseksi ja siirry **sovellukset** -välilehti.

![Uusi sovellus][api-management-aad-new-application-devportal]

Luo uusi Azure Active Directory-sovellus valitsemalla **Lisää** ja valitse **Lisää sovellus kehittää oman organisaation ulkopuolelta**.

![Uusi sovellus][api-management-new-aad-application-menu]

Valitse **Web-sovelluksen ja/tai verkko-Ohjelmointirajapinnan**, nimi ja valitse Seuraava-nuoli. Tässä esimerkissä käytetään **APIMDeveloperPortal** .

![Uusi sovellus][api-management-aad-new-application-devportal-1]

**Kertakirjauksen** URL API hallintapalvelun URL-osoite ja liitä `/signin`. Tässä esimerkissä käytetään **https://contoso5.portal.azure-api.net/signin **.

**Sovelluksen tunnus** URL-osoitteen API hallintapalvelun URL-osoite ja liitä yksilöllisiä merkkejä. Tässä esimerkissä **https://contoso5.portal.azure-api.net/dp** käytetään ja ne voivat olla kaikki haluamasi merkit. Kun haluamasi **sovellus ominaisuudet** on määritetty, valitse Luo sovelluksen valintamerkki.

![Uusi sovellus][api-management-aad-new-application-devportal-2]

## <a name="configure-an-api-management-oauth-20-authorization-server"></a>API Management OAuth 2.0-todennus-palvelimen määrittäminen

Seuraavaksi OAuth 2.0-todennus-palvelimen määrittäminen API hallinta. Tämä vaihe on osoitettu 9:43 aloittaen video.

Valitsemalla **Suojaus** vasemmalla API hallinta-valikosta ja valitse **OAuth 2.0** **Lisää luvan** palvelimeen.

![Lisää luvan-palvelin][api-management-add-authorization-server]

Kirjoita **nimi** ja **kuvaus** -kenttiin nimi ja halutessasi kuvaus. Näitä kenttiä käytetään tunnistavan sisällä API Management-palvelun esiintymän OAuth 2.0 todennus-palvelimessa. Tässä esimerkissä käytetään **luvan palvelimen esittely** . Myöhemmin kun määrität OAuth 2.0-palvelimeen, jota käytetään todennus Ohjelmointirajapinta, valitse tämä nimi.

Anna **asiakkaan rekisteröinti kirjautumissivun URL-osoite** paikkamerkin arvon kuten `http://localhost`.  **Asiakkaan rekisteröinti kirjautumissivun URL-osoite** osoittaa sivu, jossa käyttäjät voivat käyttää voit luoda ja määrittää omia tilit OAuth 2.0-palvelut, jotka tukevat Käyttäjähallinta tilien osalta. Tässä esimerkissä käyttäjät eivät luo ja määritä omat tilit paikkamerkin käytetään.

![Lisää todennus-palvelin][api-management-add-authorization-server-1]

Määritä seuraavaksi **luvan päätepisteen URL-osoite** ja **tunnussanoma päätepisteen URL-osoite**.

![Todennus-palvelin][api-management-add-authorization-server-1a]

Voit noutaa nämä arvot AAD sovelluksen developer-portaalissa loit **Sovelluksen päätepisteet** -sivulla. Accessin päätepisteet Siirry AAD-sovelluksen **määrittäminen** -välilehti ja valitse **Näytä päätepisteet**.

![Sovelluksen][api-management-aad-devportal-application]

![Näytä päätepisteet][api-management-aad-view-endpoints]

**OAuth 2.0 luvan päätepisteen** kopioi ja liitä se **luvan päätepisteen URL** -tekstiruutuun.

![Lisää luvan-palvelin][api-management-add-authorization-server-2]

Kopioi **vahvistustunnuksen päätepisteen OAuth 2.0** ja liitä se **tunnuksen päätepisteen URL** -tekstiruutuun.

![Lisää todennus-palvelin][api-management-add-authorization-server-2a]

Lisäksi-tunnuksen päätepisteen liittämiseen, lisätä **muita leipäteksti-parametrin nimi** ja arvo käytettäväksi **Sovelluksen tunnus URI** AAD sovelluksesta Taustajärjestelmä-palvelun, joka on luotu milloin Visual Studio-projekti on julkaistu.

![Sovellustunnus URI][api-management-aad-sso-uri]

Määritä seuraavaksi asiakkaan käyttäjätiedot. Nämä ovat haluat käyttää, tässä tapauksessa Taustajärjestelmä palvelun resurssin tunnistetiedot.

![Asiakkaan käyttäjätiedot][api-management-client-credentials]

Saat **Asiakastunnus**-Siirry Taustajärjestelmä palvelun AAD-sovelluksen **määrittäminen** -välilehti ja kopioi **Asiakastunnus**.

Voit hakea **Asiakkaan salaisuus** napsauttamalla **Valitse kesto** avattavasta **näppäimet** -osassa ja määritä aikaväli. Tässä esimerkissä käytetään vuoden.

![Asiakastunnus][api-management-aad-client-id]

Valitse **Tallenna** Tallenna kokoonpano ja Näytä sitten-näppäintä. 

>[AZURE.IMPORTANT] Pane merkille avain. Kun suljet ikkunan Azure Active Directory-määritykset, avainta ei voi näyttää uudelleen.

Avaimen Kopioi Leikepöydälle, palaa publisher-portaalissa, liitä avaimen **Asiakkaan salaisuus** tekstiruutu ja valitse **Tallenna**.

![Lisää luvan-palvelin][api-management-add-authorization-server-3]

Todennus-koodin myönnä on heti seuraavat asiakkaan käyttäjätiedot. Kopioi luvan koodi ja siirry Azure AD-developer portaalin sovelluksesi määrittäminen sivu ja liitä luvan myöntää **Vastaa URL-osoite** -kenttään ja valitse **Tallenna** .

![Vastaa URL-osoite][api-management-aad-reply-url]

Seuraavaksi developer-portaalissa AAD sovelluksen käyttöoikeuksien määrittäminen. Valitse **Sovelluksen käyttöoikeudet** ja **directory tietojen**valintaruutu. **Tallenna** tallentaa muutoksen, ja valitse sitten **Lisää sovellus**.

![Lisää käyttöoikeuksia][api-management-add-devportal-permissions]

Napsauta hakukuvaketta, kirjoita **APIM** alkaa-ruutuun, valitse **APIMAADDemo**ja sitten Tallenna valintamerkki.

![Lisää käyttöoikeuksia][api-management-aad-add-app-permissions]

Valitse **Valtuutetut käyttöoikeudet** **APIMAADDemo** ja **Accessin APIMAADDemo**valintaruutu ja valitse **Tallenna**. Näin kehittäjä portaalin sovelluksen Taustajärjestelmä palvelun käyttämiseen.

![Lisää käyttöoikeuksia][api-management-aad-add-delegated-permissions]

## <a name="enable-oauth-20-user-authorization-for-the-calculator-api"></a>Laskimen API OAuth 2.0 käyttäjän käyttöoikeuksien ottaminen käyttöön

Nyt kun OAuth 2.0-palvelin on määritetty, voit määrittää oman Ohjelmointirajapinta suojausasetusten. Tämä vaihe on osoitettu videon aloittaen 14:30.

Valitse vasemmanpuoleisessa valikossa **ohjelmointirajapinnan** ja sitten **Laskimen** tarkasteleminen ja sen asetusten määrittäminen.

![Laskimen Ohjelmointirajapinta][api-management-calc-api]

Siirry **Suojaus** -välilehti, **OAuth 2.0** -valintaruutu, valitse haluamasi luvan palvelin **luvan palvelimen** avattavasta luettelosta ja valitse **Tallenna**.

![Laskimen Ohjelmointirajapinta][api-management-enable-aad-calculator]

## <a name="successfully-call-the-calculator-api-from-the-developer-portal"></a>Laskuri-Ohjelmointirajapinnan kutsua onnistuneesti developer-portaalissa

Nyt kun OAuth 2.0-todennus on määritetty Ohjelmointirajapinnan, toimintojaan voit kutsuttava onnistuneesti developer center. Tämä vaihe on osoitettu 15:00 aloittaen video.

Siirry takaisin developer-portaalissa Laskimen-palvelun **lisääminen kahden kokonaisluvun** -toiminto ja valitse **Kokeile**. Huomautus **todennus** -kohdassa vastaavat juuri lisäämäsi luvan palvelimeen uuden kohteen.

![Laskimen Ohjelmointirajapinta][api-management-calc-authorization-server]

Valitse **todennus koodi** luvan avattavasta luettelosta ja kirjoita käytettävän tilin tunnistetiedot. Jos olet jo kirjautunut sisään tilille sinua ei voi pyytää.

![Laskimen Ohjelmointirajapinta][api-management-devportal-authorization-code]

Valitse **Lähetä** , ja tarkista **vastauksen tila** **200 OK** ja vastaus sisällön toiminnon tulokset.

![Laskimen Ohjelmointirajapinta][api-management-devportal-response]

## <a name="configure-a-desktop-application-to-call-the-api"></a>Soita Ohjelmointirajapinnan työpöytäsovellus määrittäminen

Seuraavassa videossa 16:30 alkaa, ja määrittää yksinkertaisen työpöytäsovellus, voit soittaa Ohjelmointirajapinnan. Ensimmäiseksi on Rekisteröi työpöytäsovelluksesta Azure AD ja antamaan sille access-kansioon ja Taustajärjestelmä-palveluun. 18:25 on osoittamista kutsumista toiminnon Laskimen API työpöytäsovellus.

## <a name="configure-a-jwt-validation-policy-to-pre-authorize-requests"></a>Hyväksy ennalta pyynnöt JWT kelpoisuuden käytännön määrittäminen

Videon lopullinen kuvattua 20:48 alkaa, ja näytetään, miten voit sallia valmiiksi vahvistamalla saapuvien sivupyynnön access-tunnusten pyynnöt [Vahvista JWT](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) käytännön avulla. Jos pyyntö ei ole vahvistettu Vahvista JWT käytäntö, on estänyt API hallinnan sekä on ei ole välitetty taustaan pitkin.

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

Katso toisen tehon määrittäminen ja käyttäminen tämän käytännön [Cloud kattavat jakson 177: Lisää API hallintaominaisuudet](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ja eteenpäin siirtyminen 13:50. Siirry eteenpäin 15:00 Nähdäksesi käytäntöjä, joka on määritetty käytäntö-editorissa ja sitten 18:50 osoittamista toiminnon soitettaessa developer-portaalissa sekä ilman tarvittavat todennus-tunnuksen.

## <a name="next-steps"></a>Seuraavat vaiheet
-   Tutustu Lisää [videoita](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API hallinnasta.
-   Muita tapoja suojata Taustajärjestelmä-palvelussa artikkelissa [molemminpuolinen todennus](api-management-howto-mutual-certificates.md) ja [yhteyden, VPN- tai ExpressRoute kautta](api-management-howto-setup-vpn.md).

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
[Ensimmäinen API hallinta]: api-management-get-started.md
