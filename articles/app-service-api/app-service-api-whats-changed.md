<properties
    pageTitle="Sovelluksen Service API Apps – mikä on muuttunut | Microsoft Azure"
    description="Tutustu uusiin Azure App Service API varten."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>Sovelluksen Service API Apps – mikä on muuttunut

Yhteyden muodostaminen tapahtuman marraskuun 2015 parannuksia Azure-sovelluksen palveluun on [ilmoitettiin](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Parannuksia ovat pohjana muutokset API sovelluksiin paremmin tasassa Mobile ja online-sovellusten, vähentää käsite määrä ja käyttöönoton ja runtime suorituskykyä. Käynnistämisestä 30 marraskuun 2015, uudessa API-sovelluksessa voit luoda Azure hallinta-portaalissa ja uusimmat sillä nämä muutokset vaikuttavat. Tässä artikkelissa kuvataan muutokset sekä ottamisesta käyttöön aiemmin sovellusten hyödyntää ominaisuuksia.

## <a name="feature-changes"></a>Ominaisuuksien muutokset
Suoraan App palvelun siirtänyt API Apps – todennuksen, CORS ja API metatiedot – ominaisuuksia. Tämän muutoksen ominaisuudet ovat käytettävissä Web, Mobile ja API-sovellukset. Itse asiassa kolme jakaa saman **Microsoft.Web/sites** resurssin laji-Resurssienhallinta. API sovellusten yhdyskäytävä ei enää tarvita tai tarjota API-sovelluksilla. Tämä helpottaa myös käyttää Azure API hallinta, koska hakutuloksissa näkyy vain yksi API Management Gatewayn.

![API-sovellusten yleiskatsaus](./media/app-service-api-whats-changed/api-apps-overview.png)

Tärkeimmät tarpeen, API sovellusten päivityksessä on, jotta voit tuoda oman Ohjelmointirajapinnan on värisinä kielellä.  Jos yhteyttä API Oletko jo ottanut käyttöön Web App-sovelluksen tai Mobile-sovelluksen, voit hyödyntää uusia ominaisuuksia sovelluksen käyttöön ei ole. Jos käytät parhaillaan API sovellusten esikatselu-siirron ohjeet on kuvattu alla.

### <a name="authentication"></a>Todennus
Aiemmin toimitettuihin API-sovellukset, Mobile: Services-sovellusten ja Web Apps-sovellusten käyttöoikeuksien ominaisuuksia on yhdistetty ja ovat käytettävissä yhden Azure App Service-todennusta-sivu hallinta-portaalin. Katso esittely todennuspalvelujen sovelluksen-palvelussa, [laajentaminen App palvelun todennus / luvan](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

API skenaarioille on useita asiaa uusia ominaisuuksia:

- **Tuki suoraan Azure Active Directoryn avulla**, ottaa istunnon tunnuksen AAD tunnus Exchange client koodia sisältämättömien: asiakkaan vain sisällyttää AAD tunnusten Authorization-otsikko haltijan suojaustunnuksen määrityksen mukaan. Tämä tarkoittaa sitä ei ole sovelluksen palvelukohtaisia SDK tarvitaan asiakas- tai reunassa. 
- **Palvelu tai "Sisäinen" access**: Jos sinulla on suodatinohjelman prosessi tai toisen asiakkaan hänen nimeään tarvitse käytön API ilman käyttöliittymään, voit pyytää käyttämällä AAD-palvelun lyhennys tunnusta ja välittää sen sovelluksen palvelun todennustavaksi sovelluksen.
- **Lykätty luvan**: useita sovelluksia on eri osia sovelluksen erilaisten käyttörajoitukset. Haluat ehkä joitakin ohjelmointirajapinnan on yleisesti saatavilla muiden edellyttää sisäänkirjautumista. Alkuperäinen todennus/todennus-ominaisuus on all-or-nothing, koko sivustoon kirjautumisen edellyttävän. Tämä asetus on edelleen olemassa, mutta voit myös antaa sovelluksen koodin hahmontamiseen access päätöksiä, kun sovellus-palvelu on todennetun käyttäjän.
 
Lisätietoja käyttöoikeuksien uusista ominaisuuksista on artikkelissa [todennus- ja Azure App Service API varten](app-service-api-authentication.md). Lisätietoja siirtää aiemmin API-sovellusten edellisen API sovellusten mallista uuteen-kohdassa [siirtämistä aiemmin API](#migrating-existing-api-apps) tämän artikkelin.
 
### <a name="cors"></a>CORS
Sen sijaan, että pilkuilla erotettu **MS_CrossDomainOrigins** sovelluksen tämä asetus on nyt sivu Azure hallinta-portaalin CORS määrittämistä varten. Voit myös voi määrittää tooling kuten PowerShellin Azure, CLI tai [Resurssin Explorer](https://resources.azure.com/)resurssien hallinnan avulla. Määritä **cors** -ominaisuuden **Microsoft.Web/sites/config** resurssityyppiä oman ** &lt;sivustonimi&gt;/web** resurssi. Esimerkki:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>API metatiedot
API määritys-sivu on nyt saatavilla Web, Mobile ja API-sovellukset. Valitse hallinta-portaalissa voit määrittää suhteellinen URL-osoite tai absoluuttinen URL-osoite päätepisteen osoittamassa, että API isännät Swagger 2.0-esitys. Voit myös voi määrittää käyttämällä Resurssienhallinta sillä. Määritä **apiDefinition** -ominaisuuden **Microsoft.Web/sites/config** resurssityyppiä oman ** &lt;sivustonimi&gt;/web** resurssi. Esimerkki:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

Tällä hetkellä metatietojen päätepisteen on oltava kaikkien käytettävissä ilman todennusta monta edeltävät asiakkaiden (kuten Visual Studio REST API asiakkaan luonti ja PowerApps "Lisää API" tiedonkulun) tarjoaman sitä. Tämä tarkoittaa, jos käytät App todentaminen ja haluat näyttää Ohjelmointirajapinnan määritelmän mistä sovelluksen itse, tarvitset käyttämään edempänä niin, että Swagger metatietojen reititys on julkinen myöhemmäksi todennusta-asetus.

## <a name="management-portal"></a>Hallinta-portaalissa
Valitsemalla **Uusi > Web + Mobile > API sovelluksen** portaalissa Luo API-sovellukset, jotka vastaavat uusia ominaisuuksia, jotka on artikkelissa kuvatulla tavalla. **Selaa > API sovellusten** näyttää vain sovellukset API. Kun selaat API-sovellukseen, sivu jakaa saman asettelu ja ominaisuudet ovat samat kuin Web- ja Mobile-sovellusten. Vain erot ovat pikaopas sisältö ja asetukset järjestystä.

Olemassa olevan API-sovellukset (tai Marketplace API-sovellukset, jotka on luotu logiikan sovelluksista) ja Edellinen esikatselu ominaisuuksia näkyy yhä logiikan sovellusten suunnittelussa ja kaikkia resursseja resurssiryhmän selattaessa.

## <a name="visual-studio"></a>Visual Studio

Useimmat tooling verkkosovelluksissa toimii uudessa API-sovelluksessa, koska ne jakaa saman pohjana **Microsoft.Web/sites** -resurssin laji. Azure Visual Studio tooling, olisi kuitenkin päivitetyn version 2.8.1 tai uudempi versio, koska se usealla eri ominaisuuksia tietyn API. Lataa SDK [Azure lataussivulle](https://azure.microsoft.com/downloads/).

Sovelluksen palvelun tietuetyypeistä järkeistämisestä julkaista myös yhdistetyn kohdassa **Julkaise > Microsoft Azure-sovelluksen palvelun**:

![API sovellusten julkaiseminen](./media/app-service-api-whats-changed/api-apps-publish.png)

Lue lisätietoja SDK 2.8.1 ilmoitus [blogimerkintä](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Voit vaihtoehtoisesti manuaalisesti tuoda Julkaise-profiilin hallinta-portaalista käyttöön Julkaise. Kuitenkaan Cloud Explorerissa, koodin luominen ja API app valinnan/luominen edellyttää SDK 2.8.1 tai uudempi versio.

## <a name="migrating-existing-api-apps"></a>Siirtyminen aiemmin API-sovellukset
Jos mukautetun API otetaan käyttöön aiemmasta Preview-versiosta API-sovelluksia, emme pyytää siirtää uuteen malliin API-sovellusten avulla 2015 31 joulukuun. Koska sekä vanha ja uusi malliin perustuvat verkko-ohjelmointirajapinnan ylläpidettävä App palvelu, aiemmin luotua koodia useimpia voi käyttää uudelleen.

### <a name="hosting-and-redeployment"></a>Isännöinti ja lukea
Mallirakenteeseen vaiheet ovat samat kuin käyttöönotto aiemmin minkä tahansa verkko-Ohjelmointirajapinnan sovelluksen-palveluun. Ohjeita:

1. Luo tyhjä API-sovellus. Tämä voidaan tehdä uusi portaaliin > API-sovelluksessa Visual Studio Julkaise tai Resurssienhallinta sillä. Jos Resurssienhallinta esitetyllä tai malleja, määrittää **laji** -arvon **API** **Microsoft.Web/sites** resurssin laji on quickstarts ja asetusten hallinta-portaalin aloittaminen kohti API skenaarioita.
2. Muodosta ja ota käyttöön projektin tyhjä API-sovellus App palvelun tukemat käyttöönotto-järjestelmiä avulla. Lue lisätietoja [Azure App palvelun käyttöönoton ohjeet](../app-service-web/web-sites-deploy.md) . 
  
### <a name="authentication"></a>Todennus
Sovelluksen Service-todennuspalvelujen tuki samat ominaisuudet, jotka olivat käytettävissä edellistä API sovellukset-mallia. Jos käytät istunnon tunnusten ja vaatia SDK: T, käytä seuraavia asiakkaan ja palvelimen SDK: T:

- Asiakas: [Azure mobiilisovelluksen SDK-paketissa](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Palvelin: [Microsoft Azure mobiilisovelluksen .NET todennus-tunniste](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Jos käytössäsi on sen sijaan App palvelun alfa SDK: T, ne ovat nyt poistettu:

- Asiakkaan: [Microsoft Azure AppService SDK-paketissa](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Palvelin: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Erityisesti Azure Active Directory-hakemistosta, kuitenkin ei ole sovelluksen palvelukohtaisia vaaditaan, jos käytät AAD tunnuksen suoraan.

### <a name="internal-access"></a>Sisäiseen käyttöön
Edellisen API sovellukset-mallin mukana valmiin sisäinen käyttöoikeustaso. Tämä edellyttää SDK käyttäminen allekirjoittamiseen pyynnöt. Ohjeiden aiempaa versiota, joka uusi API sovellukset-mallin AAD palvelun ansaitun käytetään vaihtoehtoisen palvelun todennuksessa tarvitsematta-sovelluksen palvelukohtaisia SDK. Lue lisää tekstimuodossa [palvelun pääasiallista todennus Azure App Service API-sovellusten](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Etsiminen
Edellisen API sovellusten mallin oli API, tutustu suorituksen saman resurssiryhmän takana samassa yhdyskäytävässä API muista sovelluksista. Tämä on erityisen hyödyllinen arkkitehtuureihin, jotka toteuttavat microservice kuviot. Tämä ei ole tuettu suoraan, useita asetuksia on käytettävissä:

1. Käytä Azure Resurssienhallinta API etsiminen.
2. Sijoita Azure API hallinnan sovellus palvelun isännöimä-ohjelmointirajapinnan eteen. Azure API hallinta ulkoseinä on, ja se voi tarjota vakaata Ulkoinen aukeaman url, vaikka olet sisäinen topologian muuttuu.
3. Muodosta etsiminen API-sovelluksen omat ja muiden rekisteröidä etsiminen sovelluksen käynnistys API-sovellukset on.
4. Käyttöönoton aikana lisätä päätepisteet API-sovellukset ja kaikki API-sovellukset (asiakkaat) app-asetukset. Tämä on elinkelpoinen mallin käyttöönotoissa ja koska API-sovellusten avulla voit nyt ohjausobjektin URL-osoitteen.

## <a name="using-api-apps-with-logic-apps"></a>Logiikan sovelluksilla API-sovellusten käyttäminen

Uuden API sovellukset-mallin toimii hyvin [Logiikan sovellusten rakenteen versio 2015-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja, lue artikkelit [API sovellusten asiakirjat-osassa](https://azure.microsoft.com/documentation/services/app-service/api/). Ne on päivitetty vastaamaan API-sovellusten uusi malli. Lisäksi saavuttaminen foorumeilla lisätiedot tai siirron ohjeita:

- [MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Pinon ylivuoto](http://stackoverflow.com/questions/tagged/azure-api-apps)
