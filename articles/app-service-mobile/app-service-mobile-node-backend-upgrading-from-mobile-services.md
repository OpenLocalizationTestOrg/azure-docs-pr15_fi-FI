<properties
    pageTitle="Sovelluksen-palveluun Azure - Node.js Mobile-palvelujen päivittäminen"
    description="Opi päivittämään helposti Mobile-palvelut-sovellus App palvelun Mobile-sovellukseen"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-nodejs-azure-mobile-service-to-app-service"></a>Päivitä aiemmin Node.js Azure mobiilipalvelun sovelluksen-palveluun

Sovelluksen palvelun Mobile on uusi tapa käyttämällä Microsoft Azure mobiili-sovelluksia. Lisätietoja on artikkelissa [mitä mobiilisovellukset?].

Tässä artikkelissa käsitellään olemassa olevan Node.js Taustajärjestelmä sovelluksen ksi uusi sovellus-palvelun Mobile-sovellusten Azure Mobile-palvelut. Samalla, kun olet suorittanut tämän päivityksen, voit jatkaa aiemmin Mobile-palvelut-sovelluksen toimimaan.  Jos haluat päivittää Node.js Taustajärjestelmä sovelluksen viitata [päivittäminen .NET Mobile-palvelut](./app-service-mobile-net-upgrading-from-mobile-services.md).

Mobiili taustassa päivityksen Azure sovelluksen-palveluun, on käyttää sovelluksen palvelun kaikkia ominaisuuksia ja mukaan [App palvelun hinnat], ei hinnat Mobile-palveluihin laskutettu.

## <a name="migrate-vs-upgrade"></a>Siirtää ja päivitys

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] On suositeltavaa, että [siirto](app-service-mobile-migrating-from-mobile-services.md) ennen päivitystä loppuun. Näin voit sijoittaa sovelluksen molemmat versiot saman sovelluksen-palvelu-suunnittelu ja maksamaan ilman lisäkustannuksia.

### <a name="improvements-in-mobile-apps-nodejs-server-sdk"></a>Mobile-sovellusten Node.js server SDK-parannukset

Uusi [Mobile-sovellusten SDK](https://www.npmjs.com/package/azure-mobile-apps) -päivityksen tarjoaa useita parannuksia, mukaan lukien:

- [Express framework](http://expressjs.com/en/index.html)mukaan uusi solmu SDK on himmennetty ja niiden tarkoitus mukaa kuin niitä ilmenee seurata uusia solmu-versioiden kanssa. Voit mukauttaa sovelluksen toimintaa Express middleware kanssa.

- Merkittäviä parannuksia verrattuna on Mobile SDK: ssa.

- Nyt voit isännöidä sivusto ja että mobile Taustajärjestelmä; vastaavasti on helppo Azure Mobile SDK lisääminen aiemmin luotuun express.v4-sovellukseen.

- Luotu Office kaikissa ympäristöissä ja paikallisen kehitystä, Mobile-sovellusten SDK voidaan kehittää ja suorita paikallisesti Windows Linux ja OS x-käyttöjärjestelmien työpöytäsovelluksiin. Se on nyt helppokäyttöisiä yleisiä solmu kehittäminen tekniikoita kuten käynnissä [Mocha](https://mochajs.org/) testejä ennen käyttöönottoa.

## <a name="overview"></a>Perustiedot Päivitä yleiskatsaus

Helpota Node.js taustassa päivittäminen-sovelluksen Azure-palvelu on tarjonnut yhteensopivuus-paketti.  Päivityksen jälkeen on niew-sivusto, joka voidaan ottaa käyttöön sovelluksen Service-sivustoon.

Mobile Services client SDK: T ovat **ei ole** yhteensopiva uuden mobiilisovellukset palvelimen SDK kanssa. Anna palvelun, kun sovellus jatkuvuuden, jotta voit tarjota tällä hetkellä julkaistun asiakkaiden sivuston olisi ei julkaista muutokset. Sen sijaan sinun on luotava uusi mobiilisovelluksen, joka on jo olemassa. Voit sijoittaa tämän sovelluksen saman sovelluksen palvelusopimus välttämiseksi sille taloudellisen tilanteen lisäkustannuksia.

Sen jälkeen sovellus kaksi versiota: sähköpostiosoitteen loppuosa pysyy muuttumattomana ja on julkaistu sovellusten luonnosta, ja toisen, jossa voit päivittää ja kohde uusi asiakas-versiossa. Voit siirtää ja Testaa yhteyttä tahdissa koodi, mutta varmista, että kaikki virheenkorjauksia teet Hae sovelletaan molempia. Kun huomaat, että haluamasi määrä luonnosta sovellukset on päivitetty uusimpaan versioon asiakas, voit poistaa alkuperäisen siirretyt sovelluksen kysymykset. Se ei maksullisia minkä tahansa raha, jos ylläpidettävä saman sovelluksen palvelusopimus Mobile-sovelluksen nimellä.

Koko jäsennyksen päivitysprosessin on seuraavanlainen:

1. Lataa aiemmin luotu (siirretyt) Azure mobiilipalvelun.
2. Projektin muuntaminen Mobile Azure-sovelluksen yhteensopivuuden pakkaaminen.
3. Korjaa mahdolliset erot (kuten todennusasetukset).
4. Ota käyttöön muunnettu Azure Mobile-sovelluksen projektin uusi sovellus-palveluun.
4. Vapauta asiakassovellus, jotka käyttävät uusia Mobile-sovelluksen uuden version.
5. (Valinnainen) Poistaa alkuperäisen siirretyt mobiilipalvelu sovelluksen.

Poistaminen voi tapahtua, jos et näe liikenteen oman alkuperäisen siirretyt mobiilipalvelu.

## <a name="install-npm-package"></a>Asenna vaatimukset

Asenna [solmu] paikallisessa tietokoneessa.  Sinun täytyy asentaa myös yhteensopivuus-paketti.  Kun solmu on asennettu, voit suorittaa seuraavan komennon uusi cmd tai PowerShell-kehote:

```npm i -g azure-mobile-apps-compatibility```

## <a name="obtain-ams-scripts"></a>Hanki Azure Mobile-palvelujen komentosarjat

- Kirjaudu sisään [Azure Portal].
- **Kaikki resurssit** tai **Sovelluksen palveluiden**avulla, Etsi Mobile-palvelut-sivustoon.
- Sivuston, valitse **Työkalut** -> **Kudu** -> Kudu-sivuston avaaminen**Siirry** .
- Valitse **Virheenkorjaus konsolin** -> **PowerShell** virheenkorjaus-konsolin avaaminen.
- Siirry `site/wwwroot/App_Data/config` napsauttamalla kunkin hakemiston näkyvät vuorotellen.
- Valitse Lataa-kuvakkeen vieressä `scripts` directory.

Tämä Lataa komentosarjat ZIP-muodossa.  Luo uusi kansio paikallisessa tietokoneessa ja purkaminen `scripts.ZIP` tiedoston sisältä kansio.  Tämä vaihtoehto luo `scripts` hakemisto.

## <a name="scaffold-app"></a>Scaffold uuden Azure-mobiilisovellukset taustaan

Valitse komentosarjat-kansion sisältävä kansio varten suorittamalla seuraavan komennon:

```scaffold-mobile-app scripts out```

Tämä vaihtoehto Luo scaffolded Azure-mobiilisovellukset taustassa- `out` hakemisto.  Mutta niitä ei tarvita, se on suositeltavaa tarkistaa `out` kansion lähde-koodin säilöön valittua kyselyjä.

## <a name="deploy-ama-app"></a>Azure-mobiilisovellukset Taustajärjestelmä käyttöönotto

Tarvitset käyttöönoton aikana, toimi seuraavasti:

1. Luo uusi Mobile-sovelluksen [Azure-portaalissa].
2. Suorita `createViews.sql` komentosarjan yhdistetty tietokanta.
3. Linkitä kyseistä henkilöä Mobile-palvelussa uusi sovellus-palveluun.
4. Muut resurssit (esimerkiksi ilmoitus keskittimet) linkittäminen uusi sovellus-palvelu.
5. Ota käyttöön luotu koodi uuteen sivustoon.

### <a name="create-a-new-mobile-app"></a>Uuden Mobile-sovelluksen luominen

1. Kirjaudu sisään osoitteessa [Azure Portal].

2. Valitse **+ Uusi** > **Web + Mobile** > **Mobile-sovelluksen**, Anna nimi Mobile-sovelluksen Taustajärjestelmä.

3. **Resurssiryhmä**-aiemmin resurssiryhmä tai luoda uuden (joko on sama nimi kuin sovelluksesi.) 
 
    Voit valita toisen sovelluksen palvelusopimus tai luoda uuden. Saat lisätietoja sovelluksen Services suunnitelmien ja eri hinnat uuden suunnitelman luominen taso ja nähdä haluamasi sijaintisi [Azure App palvelun suunnitelmien perusteellisempaa yleiskatsaus](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).

4. **Sovelluksen palvelusopimus**(-) [Vakio taso](https://azure.microsoft.com/pricing/details/app-service/)oletusarvo-palvelupaketti on valittuna. Voit valita myös vaihtaa palvelupakettiani tai [luoda uuden](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Sovelluksen palvelun suunnitelman asetukset määrittävät [sijainti-ominaisuuksia, kustannus- ja Laske resurssit](https://azure.microsoft.com/pricing/details/app-service/) -sovellukseen kytketty. 

    Kun päätät osalta, valitse **Luo**. Tämä luo Mobile-sovelluksen Taustajärjestelmä. 


### <a name="run-createviewssql"></a>Suorita CreateViews.SQL

Scaffolded sovelluksen sisältää tiedoston `createViews.sql`.  Tämä komentosarja on voidaan kohdistaa kohdetietokanta.  Kohdetietokannan yhteysmerkkijonon saadaan siirretyt-mobiilipalvelu,-kohdasta **Yhteysmerkkijonon** **asetukset** -sivu.  Nimi on `MS_TableConnectionString`.

Voit suorittaa tämän komentosarjan SQL Server Management Studiossa tai Visual Studio.

### <a name="link-the-database-to-your-app-service"></a>Tietokannan linkittäminen sovelluksen-palvelussa

Linkittäminen aiemmin luotuun tietokantaan sovelluksen-palvelu:

- Avaa sovellus-palvelun [Azure-portaalissa].
- Valitse **kaikki asetukset** -> **tietoyhteyksiä**.
- Valitse **+ Lisää**.
- Valitse avattavan luettelon **SQL-tietokanta**
- Valitse **SQL-tietokantaan**Valitse aiemmin luotu tietokanta ja valitse sitten, **Valitse**.
- **Yhteysmerkkijonon**, valitse käyttäjänimi ja salasana Kirjoita tietokannan ja valitse sitten **OK**.
- Valitse **tietoyhteyksien lisääminen** -sivu valitsemalla **OK**.

Tarkastelemalla kohdetietokannan yhteysmerkkijonon siirretyt mobiilipalvelun löytyy käyttäjänimi ja salasana.


### <a name="set-up-authentication"></a>Käyttöoikeuksien määrittäminen

Azure Mobile-sovellusten avulla voit Azure Active Directory-, Facebook-, Google-, Microsoft- ja Twitter-todennuksen määrittäminen palvelun.  Mukautettu tarkistus on kehittänyt erikseen.  Lisätietoja [Käyttöoikeuksien käsitteitä] asiakirjat ja [Todennus pikaopas] ohjeissa.  

## <a name="updating-clients"></a>Update-mobiilisovellukset

Kun olet toiminnallisia Mobile-sovelluksen-taustatietokannan, voit käsitellä uudella versiolla, joka käyttää sitä asiakassovellus. Asiakkaan SDK: T uusi versio sisältää myös Mobile-sovellusten ja samoin kuin server-päivitystä, poista kaikki viittaukset Mobile Services SDK: T ennen asennusta Mobile-sovellusten versiot tarvitset.

Tärkeimmät muutokset versioiden välillä on konstruktoreja eivät enää tarvitse sovellusnäppäimen. Voit nyt vain välittää Mobile-sovelluksen URL-osoite. Esimerkiksi .NET-asiakkaat- `MobileServiceClient` konstruktori on nyt:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Voit lukea asennuksesta uusi SDK: T ja uusi rakennetta alla olevia linkkejä kautta:

- [Android 2.2 tai uudempi versio](app-service-mobile-android-how-to-use-client-library.md)
- [iOS versio 3.0.0 tai uudempi versio](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) versio 2.0.0 tai uudempi versio](app-service-mobile-dotnet-how-to-use-client-library.md)
- [Apache Cordova 2.0 tai uudempi versio](app-service-mobile-cordova-how-to-use-client-library.md)

Jos sovellus on käyttää push-ilmoitukset, muistiin eri ympäristöissä rekisteröinnin ohjeita, niin on joitakin muutoksia myös.

Kun olet valmis uuden asiakasohjelmaversion, kokeile päivitetyn palvelimen projektin vastaan. Voit vapauttaa asiakkaille sovelluksen uuden version tarkistettuaan, että se toimii. Myöhemmin, kun asiakkaille, sinulla on ollut mahdollisuus saada nämä päivitykset, voit poistaa sovelluksen Mobile-palvelut-versiossa. Tässä vaiheessa päivitit kokonaan Mobile-sovellusten uusimmat palvelinta SDK App palvelun Mobile ohjelmaan.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[Mitkä ovat mobiilisovellukset?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobile App Server SDK]: https://www.npmjs.com/package/azure-mobile-apps
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure Scheduler]: /en-us/documentation/services/scheduler/
[Web Job]: ../app-service-web/websites-webjobs-resources.md
[How to use the .NET server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Sovelluksen palvelun hinnat]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Todennus käsitteitä]: ../app-service/app-service-authentication-overview.md
[Todennus-pikaopas]: app-service-mobile-auth.md

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
