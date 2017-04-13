<properties
    pageTitle="Node.js palvelimeen SDK Mobile-sovellusten käsittelemisestä | Azure sovelluksen-palvelu"
    description="Lue, miten Node.js palvelimeen SDK toimimaan Azure palvelun mobiilisovellukset."
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="how-to-use-the-azure-mobile-apps-nodejs-sdk"></a>Opi käyttämään Azure Mobile sovellusten Node.js SDK-paketissa

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Tässä artikkelissa on yksityiskohtaisia ohjeita ja esimerkkejä esittää, kuinka voit käyttää Node.js taustassa Azure App palvelun Mobile-sovelluksissa.

## <a name="Introduction"></a>Johdanto

Azure palvelun mobiilisovellukset mahdollistaa mobile optimoitu tietoja access Web-Ohjelmointirajapinnan lisääminen web-sovelluksen.  ASP.NET- ja Node.js verkkosovellusten varten on annettu Azure App palvelun Mobile Apps SDK-paketissa.  SDK sisältää seuraavat toimenpiteet:

- Tietoja access-taulukon toimintoja (luku, Lisää, Päivitä, poista)
- Mukautettu API-toiminnot

Molemmat toimintojen avulla todennus kaikki tunnistetietojen toimittajat Azure App palvelua, mukaan lukien sosiaalisten tunnistetietojen toimittajat kuten Facebookissa, Twitterissä, Google ja Microsoft sekä Azure Active Directory yrityksen jäsenyyden salliman yli.

Esimerkkejä, joiden voit etsiä kunkin käyttötapaus [GitHub näytteiden hakemistoon].

## <a name="supported-platforms"></a>Tuetut käyttöympäristöt

Azure Mobile sovellusten solmu SDK tukee l. nykyinen versio solmun ja uudemmissa versioissa.  Virkkeet, l. uusin on solmu v4.5.0.  Muut versiot solmu välttämättä toimi, mutta ei tueta.

Azure Mobile sovellusten solmu SDK tukee kahta Tietokantaohjaimet - solmu mssql-ohjain tukee SQL Azure- ja paikallisen SQL Server-esiintymät.  Sqlite3-ohjain tukee vain yhden esiintymän SQLite tietokannat.

### <a name="howto-cmdline-basicapp"></a>Toimintaohje: Luo komentoriviltä Basic Node.js taustassa

Jokaisen Azure App palvelun Mobile-sovelluksen Node.js Taustajärjestelmä käynnistyy ExpressJS sovelluksena.  ExpressJS on eniten käytetyt web palvelun kehys Node.js käytettävissä.  Voit luoda basic [Express] sovelluksen toimimalla seuraavasti:

1. Valitse komento tai PowerShell-ikkunassa Luo projektin hakemisto.

        mkdir basicapp

2. Suorita npm alusta alustaa paketin rakenne.

        cd basicapp
        npm init

    Npm alusta-komento pyytää alustaa projektin kysymyksiin.  Katso esimerkki tulos:

    ![Npm alusta tulos][0]

3. Asenna pika- ja azure-mobile-sovellukset-kirjastojen npm säilö.

        npm install --save express azure-mobile-apps

4. Luo toteuttamisesta basic mobile server app.js-tiedosto.

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

Tämän sovelluksen Luo mobile optimoitu WebAPI yhden päätepisteissä (`/tables/TodoItem`), joka sisältää Todentamattomille pohjana SQL tietosäilö, dynaamisen rakenteen avulla.  Ei sovellu asiakkaan kirjaston nopeasti käynnistää seuraavat:

- [Android asiakas-pikaopas]
- [Apache Cordova asiakas-pikaopas]
- [iOS asiakas-pikaopas]
- [Windows-kaupan asiakas-pikaopas]
- [Xamarin.iOS asiakas-pikaopas]
- [Xamarin.Android asiakas-pikaopas]
- [Xamarin.Forms asiakas-pikaopas]

Löydät koodin basic sovelluksen [basicapp GitHub-malli].

### <a name="howto-vs2015-basicapp"></a>Toimintaohje: Luo solmu taustassa Visual Studio 2015

Visual Studio 2015 edellyttää tiedostotunnistetta kehittää IDE Node.js-sovelluksissa.  Voit aloittaa, asenna [Node.js Työkalut 1.1 Visual Studio].  Kun Node.js Tools for Visual Studio on asennettu, Express 4.x-sovelluksen luominen:

1. Avaa **Uusi projekti** -valintaikkuna ( **tiedostosta** > **Uusi** > **projektin...**).

2. **Mallit** > **JavaScript** > **Node.js**.

3. Valitse **Perus Azure Node.js Express 4-sovellus**.

4. Täytä projektin nimeä.  Valitse *OK*.

    ![Visual Studio 2015 uusi projekti][1]

5. Napsauta **npm** -solmu ja valitse **Asenna uusi... pakettien npm**.

6. Voit joutua päivittämään npm luettelon ensimmäisen Node.js-sovelluksen luominen.  Valitse **Päivitä** tarvittaessa.

7. Kirjoita hakuruutuun _azure-mobile-sovellukset_ .  Valitse **azure-mobiilisovellukset - 2.0.0** pakkaus ja valitse sitten **Asenna paketti**.

    ![Asenna uusi npm-paketit][2]

8. Valitse **Sulje**.

9. Avaa Lisää tuki Azure Mobile Apps SDK _app.js_ -tiedosto.  Rivillä 6 at kirjaston alareunassa edellyttävät lauseita, Lisää seuraava koodi:

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    Lisää rivi 27 noin app.use-lauseet jälkeen, seuraava koodi:

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use(mobile);

    Tallenna tiedosto.

10. Suorita sovellus paikallisesti (Ohjelmointirajapinnan on tiedoksi http://localhost:3000) tai Azure julkaiseminen.

### <a name="create-node-backend-portal"></a>Toimintaohje: Luo Node.js taustassa Azure-portaalissa

Voit luoda Mobile-sovelluksen Taustajärjestelmä oikeus [Azure portal]. Voit noudattamalla seuraavia ohjeita tai luoda asiakkaan ja palvelimen yhdessä [luominen mobiilisovelluksessa](app-service-mobile-ios-get-started.md) -opetusohjelma. Opetusohjelman on yksinkertaistettu versiota, nämä ohjeet ja parhaiten kuitti käsite projektit.

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

_Aloittaminen_ -sivu **taulukon luominen API**-kohdassa Valitse **Node.js** **Taustajärjestelmä kieli**. "**voin hyväksyy, että tämä korvaa kaikki sivuston sisältö.**" valintaruutu ja valitse sitten **Luo TodoItem taulukon**.

### <a name="download-quickstart"></a>Toimintaohje: lataa Node.js Taustajärjestelmä pikaopas koodiprojektin Git käyttäminen

Kun luot Node.js Mobile-sovelluksen taustassa **pikaopas** sivu-portaalissa, Node.js projektin puolestasi ja ottaa käyttöön sivustossa. Voit lisätä taulukoita ja ohjelmointirajapinnan ja muokata Node.js Taustajärjestelmä portaalissa kooditiedostoja. Voit myös ladata Taustajärjestelmä projektin, jotta voit lisätä tai muokata taulukoita ja ohjelmointirajapinnan sitten julkaista projektin eri käyttöönottotyökaluja. Lisätietoja on artikkelissa [Azure App palvelun käyttöönotto-oppaassa]. Seuraavassa käyttää Git säilön Lataa pikaopas project-koodia.

1. Asenna Git, jos et ole vielä tehnyt niin. Asennettava Git etenee käyttöjärjestelmien välillä. Katso [Asentaminen Git](http://git-scm.com/book/en/Getting-Started-Installing-Git) käyttöjärjestelmäkohtaisia jakelu ja asennuksen ohjeita.

2. [Ottaa käyttöön sovelluksen palvelun sovelluksen säilöön](../app-service-web/app-service-deploy-local-git.md#Step3) käyttöön Git säilö taustaan sivuston, että muistiin käyttöönoton käyttäjänimi ja salasana noudattamalla.

3. For Mobile-sovelluksen Taustajärjestelmä sivu-asetus **Git Kloonaa URL-osoite** mieleen.

4. Suorita `git clone` komento käyttäen Git Kloonaa URL-osoite, kirjoittamalla salasana, kun määrittäminen pakolliseksi, kuten seuraavassa esimerkissä:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Selaa paikallisessa kansiossa, joka edellisessä esimerkissä on /todolist, niin huomaat projektitiedostoja ladataan. Etsi `todoitem.json` -tiedoston `/tables` hakemisto.  Tämä tiedosto määrittää käyttöoikeudet taulukon.  Myös `todoitem.js` taulukon komentosarjojen tiedostoa samaan kansioon, joka määrittää, että CRUD-toimintoa.

6. Kun olet tehnyt muutoksia projektitiedostoihin, suorita seuraavat komennot Lisää, Vahvista ja tee muutokset lataaminen sivustoon:

        $ git commit -m "updated the table script"
        $ git push origin master

    Kun lisäät uusia tiedostoja projektiin, sinun on ensin suorittamaan `git add .` komento.

Sivuston uudelleen aina, kun uusi joukko tapahtumasarjan vahvistukset on miten sivustoon.

### <a name="howto-publish-to-azure"></a>Toimintaohje: Node.js Taustajärjestelmä julkaiseminen Azure

Microsoft Azure tarjoaa useita järjestelmiä julkaisemisen Azure App palvelun Mobile-sovellusten Node.js Taustajärjestelmä Azure-palveluun.  Näitä ovat käyttävien käyttöönottotyökaluja integroitu Visual Studiossa, komentorivin työkalut ja tietolähteen ohjausobjektin perusteella jatkuva asennusvaihtoehdot.  Lisätietoja aiheesta on artikkelissa [Azure App palvelun käyttöönotto-oppaassa].

Azure sovelluksen-palvelulla erityisiä neuvoja Node.js-sovelluksen, joihin kannattaa tutustua ennen kuin otat:

- [Solmu-Version] määrittäminen
- Opi käyttämään [solmu moduulit]

### <a name="howto-enable-homepage"></a>Toimintaohje: ottaa käyttöön sovelluksen kotisivu

Monet sovellukset ovat yhdistelmän web- ja mobile-sovellukset ja ExpressJS framework avulla voit yhdistää kaksi rakenteita.  Joskus kuitenkin haluta Toteuta vain mobile liittymää.  Kannattaa antaa aloitussivu varmistamiseksi sovelluksen-palvelu on hyvin alkuun.  Voit säätää aloitus-sivulla tai käyttöön tilapäinen aloitussivulle.  Tilapäinen kotisivun käyttöön käyttämällä vahvistamiseen Azure-mobiilisovellukset seuraavaa:

    var mobile = azureMobileApps({ homePage: true });

Jos haluat tämä asetus on käytettävissä vain, kun kehittäminen paikallisesti, voit lisätä tämän asetuksen, että `azureMobile.js` tiedosto.

## <a name="TableOperations"></a>Taulukkotoiminnot 

Azure--mobiilisovellukset Node.js Server SDK sisältää järjestelmiä voit näyttää arvotaulukot Azure SQL-tietokanta WebAPI kuin tallennetaan.  Viisi toiminnot ovat käytettävissä.

| Toiminto | Kuvaus |
| --------- | ----------- |
| HAE /tables/_taulukon nimi_ | Hae kaikki tietueet taulukossa |
| HAE /tables/_taulukon nimi_/:id | Hae tiettyyn tietueeseen taulukossa |
| Kirjaa /tables/_taulukon nimi_ | Luo tietue taulukossa |
| KORJAUSTIEDOSTON /tables/_taulukon nimi_/:id | Päivitä tietue taulukossa |
| Poista /tables/_taulukon nimi_/:id | Taulukon tietueen poistaminen |

Tämä WebAPI tukee [OData] ja laajentaa Taulukkorakenteen tukemaan [offline-tietojen synkronointi].

### <a name="howto-dynamicschema"></a>Toimintaohje: määrittää taulukoiden dynaamisen rakenteen avulla

Ennen kuin taulukon voidaan käyttää, se on vielä määritettävä.  Taulukoita voi määrittää staattinen rakenne (jossa kehittäjä määrittää rakenteen sarakkeita) tai dynaamisesti (jossa SDK ohjaa pyynnöt perusteella rakenteen). Lisäksi kehittäjä, voit määrittää WebAPI tiettyjä ominaisuuksia lisäämällä Javascript-koodia määritelmän.

Paras käytäntö tulee jokaisessa taulukossa määritetään taulukoiden hakemistossa Javascript-tiedoston sitten Tuo taulukot tables.import()-menetelmällä.  Laajentaminen basic-sovelluksen app.js tiedoston mukautettava:

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

Määritä juuri. / tables/TodoItem.js:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

Taulukoiden käyttää dynaamisen rakenteen oletusarvoisesti.  Dynaamisen rakenteen käytöstä yleisesti, arvoksi false Azure portaalin asetus **MS_DynamicSchema** .

Löydät täydellinen esimerkki [todo otoksen GitHub käyttöön].

### <a name="howto-staticschema"></a>Toimintaohje: Määritä luodaan staattinen rakenne

Voit määrittää erikseen sarakkeet näyttää WebAPI kautta.  Azure--mobiilisovellukset Node.js SDK lisää automaattisesti kaikki tarvittavat offline-tietojen synkronointi luetteloon, joka saadaan sarakkeita.  Esimerkiksi pikaopas-asiakassovellusten edellyttää taulukko, jossa on kaksi saraketta: teksti (merkkijono) ja (totuusarvo).  
Taulukko voidaan määrittää taulukon JavaScript määritystiedoston (joka sijaitsee siinä hakemistossa taulukot) seuraavasti:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

Jos määrität taulukoiden nimipalvelimet, valitse on myös soitat tables.initialize() menetelmä luo tietokantarakenteen käynnistyksen yhteydessä.  Tables.initialize()-menetelmä palauttaa [Promise] niin, että web-palvelu ei käsittele pyyntöjä valmistellaan tietokanta ennen.

### <a name="howto-sqlexpress-setup"></a>Toimintaohje: Käytä SQL ilmaiseminen kehittäminen tietosäilö paikallisessa tietokoneessa

Azure Mobile sovellukset AzureMobile sovellukset solmu SDK sisältää tietoja käsittelevä ulos ruutuun kolmella eri: kolme vaihtoehtoa SDK että tietojen tarjoillaan ulos ruutuun:

- Jatkuva Esimerkki kaupan **muistin** ohjaimen avulla
- SQL Express tietosäilö kehittämiseen **mssql** ohjaimen avulla
- Azure SQL-tietokanta-tietovarasto tuotannon **mssql** ohjaimen avulla

Azure Mobile sovellusten Node.js SDK käyttää [mssql Node.js paketin] ja muodostaa yhteyden SQL Express ja SQL-tietokantaan.  Tämä paketti edellyttää, että otat TCP-yhteydet SQL Express-esiintymässä.

> [AZURE.TIP]Muisti-ohjain ei tarjoa tilat testikäyttöön täydelliset.  Jos haluat testata Taustajärjestelmä paikallisesti, on suositeltavaa SQL Express tietosäilö ja mssql ohjain.

1. Lataa ja asenna [Microsoft SQL Server 2014 Express].  Varmista, että voit asentaa SQL Server 2014 Expressin Työkalut-version mukana.  Jos sinun ei tarvitse erikseen 64-bittinen tuki, 32-bittisen version siinä käytetään vähemmän muistia, kun käynnissä.

2. Suorittamalla SQL Serverin 2014 määritysten hallinta.

  1. Laajenna vasemmanpuoleisessa Puuvalikko **SQL Serverin verkon määritys** -solmu.
  2. Valitse **SQLEXPRESS protokollat**.
  3. Napsauta hiiren kakkospainikkeella **TCP/IP** , ja valitse **Ota käyttöön**.  Valitse avautuvassa valintaikkunassa **OK** .
  4. Napsauta hiiren kakkospainikkeella **TCP/IP** , ja valitse **Ominaisuudet**.
  5. Valitse **IP-osoitteet** -välilehti.
  6. Etsi solmu **IPAll** .  Lisää **1433** **Porttinumeroa** -kenttään.

         ![Configure SQL Express for TCP/IP][3]

  7. Valitse **OK**.  Valitse avautuvassa valintaikkunassa **OK** .
  8. Valitse vasemmanpuoleisessa Puuvalikko **SQL Server Services** .
  9. **SQL Server (SQLEXPRESS)** hiiren kakkospainikkeella ja valitse **uudelleen**
  10. Sulje SQL Serverin 2014 määritysten hallinta.

3. Suorita SQL Server 2014 Management Studiossa ja muodosta yhteys paikalliseen SQL Express-esiintymä

  1. Oman esiintymän Object Explorerissa hiiren kakkospainikkeella ja valitse **Ominaisuudet**
  2. Valitse **Suojaus** -välilehti.
  3. Varmista **SQL Server- ja Windows-todennustila** on valittuna
  4. Valitse **OK**

        ![SQL Express-todennuksen määrittäminen][4]

  5. Laajenna **Suojaus** > **Kirjautumiset** Object Explorerissa
  6. Napsauta **Kirjautumiset** hiiren kakkospainikkeella ja valitse **Uusi kirjautuminen...**
  7. Kirjoita Login nimi.  Valitse **SQL Server-todennus**.  Kirjoita salasana ja valitse sama salasana Kirjoita **Vahvista salasana**.  Salasana on täytettävä Windows monimutkaisuusvaatimukset.
  8. Valitse **OK**

        ![Lisää uusi käyttäjä SQL Expressiin][5]

  9. Napsauta uuden kirjautumisen ja valitse **Ominaisuudet**
  10. Valitse **Roolit** -sivu
  11. Valitse haluamasi **dbcreator** rooli-valintaruutu
  12. Valitse **OK**
  13. Sulje SQL Server 2015 Management Studiossa

Varmista, voit tallentaa käyttäjänimi ja salasana, jotka olet valinnut.  Voit joutua määrittämään palvelimen roolit ja oikeudet tietokannan avaamiseksi tarpeen mukaan.

Node.js-sovellus lukee tietokannan yhteysmerkkijono **SQLCONNSTR_MS_TableConnectionString** -ympäristömuuttuja.  Voit määrittää tämän muuttujan käyttöympäristössä.  Voit esimerkiksi määrittää tämä ympäristömuuttuja PowerShellin avulla:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

Tietokannan TCP/IP-yhteyden kautta ja anna yhteyden käyttäjänimeä ja salasanaa.

### <a name="howto-config-localdev"></a>Toimintaohje: paikallisen kehittämiseen projektin määrittäminen

Azure Mobile-sovellusten lukee kutsua _azureMobile.js_ paikallisesta tiedostojärjestelmästä JavaScript-tiedoston.  Älä käytä tätä tiedostoa määrittäminen tuotannon Azure Mobile-sovellukset-SDK-sovelluksen asetusten [Azure portal] Käytä sen sijaan.  _AzureMobile.js_ tiedoston Vie kokoonpano-objekti.  Yleisimmät asetukset ovat seuraavat:

- Asetusten määrittäminen
- Diagnostiikan kirjaus
- Vaihtoehtoinen CORS asetukset

Esimerkki _azureMobile.js_ äänitiedoston käyttöönoton edellisen tietokanta-asetuksia seuraavasti:

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

On suositeltavaa _azureMobile.js_ lisääminen _.gitignore_ -tiedosto (tai muita lähdekoodin hallinta ohittaa tiedoston) pilveen tallennettujen salasanojen estäminen.  Tuotannon asetusten määrittäminen aina [Azure portal]App-asetukset.

### <a name="howto-appsettings"></a>Miten: Mobile-sovellus App asetusten määrittäminen

Useimmat asetukset _azureMobile.js_ tiedostossa on vastaavan sovelluksen-asetuksen [Azure portal].  Seuraavan luettelon avulla voit määrittää sovelluksen asetukset-sovelluksen:

| Asetus                 | _azureMobile.js_ -asetus  | Kuvaus                               | Kelvollisia arvoja                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | Nimi                      | Sovelluksen nimeä                       | merkkijono                                      |
| **MS_MobileLoggingLevel**   | Logging.level             | Pienin lokin sanomien taso      | Korjaa virhe, Varoitus-tiedot, yksityiskohtainen, Hassuja |
| **MS_DebugMode**            | korjaaminen                     | Ota käyttöön tai virheenkorjaus-tilan poistaminen käytöstä              | arvo on TOSI, EPÄTOSI                                 |
| **MS_TableSchema**          | Data.Schema               | Rakenteen oletusnimi SQL-taulukot        | merkkijono (oletus: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | Ota käyttöön tai virheenkorjaus-tilan poistaminen käytöstä              | arvo on TOSI, EPÄTOSI                                 |
| **MS_DisableVersionHeader** | version (määritetty, määrittämätön)| Poistaa käytöstä X-ZUMO-Server-Version-otsikko | arvo on TOSI, EPÄTOSI                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | Poistaa käytöstä asiakasohjelman API-version tarkistaminen     | arvo on TOSI, EPÄTOSI                                 |

Sovelluksen-asetuksen määrittäminen:

1. Kirjaudu sisään [Azure portal].
2. Valitse **kaikki resurssit** tai **Sovelluksen palvelujen** valitsemalla Mobile-sovelluksen nimi.
3. Asetukset-sivu avautuu oletusarvoisesti. Jos näin ei ole, valitse **asetukset**.
4. Valitse Yleiset-valikossa **asetukset** .
5. Siirry sovelluksen asetukset-osassa.
6. Jos sovelluksen määrittäminen jo olemassa, valitse Muokkaa arvoa sovelluksen-asetuksen arvoa.
7. Jos sovellus-asetus ei ole, kirjoita asetus avain-ruutuun ja arvo-ruudussa.
8. Kun olet valmis, valitse **Tallenna**.

Useimmat app-asetusten muuttaminen vaatii uudelleenkäynnistyksen palvelu.

### <a name="howto-use-sqlazure"></a>Toimintaohje: Käytä SQL-tietokanta nimellä tuotannon tietosäilö

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Käytä tietosäilö Azure SQL-tietokanta on sama kuin kaikkien Azure App palvelun sovelluksen välillä. Jos et ole vielä niin, Luo Mobile-sovelluksen taustassa seuraavasti.

1. Kirjaudu sisään [Azure portal].

2. Ylä-vasemmalle ikkunan, valitse **+ Uusi** -painike > **Web + Mobile** > **Mobile-sovelluksen**, Anna nimi Mobile-sovelluksen Taustajärjestelmä.

3. Kirjoita **Resurssiryhmä** -ruudussa on sama nimi kuin sovelluksen.

4. Oletusarvoinen sovelluksen palvelusopimus on valittuna.  Jos haluat muuttaa sovelluksen palvelusopimus, voit tehdä napsauttamalla sovelluksen palvelusopimus > **+ luoda uusia**.  Anna uusi sovellus-palvelusopimus nimi ja valitse haluamiisi kohtiin.  Hinnoittelu-taso ja valitse haluamasi hinnoittelu taso-palvelun. Valitse Näytä Lisää hinnoittelu asetukset, kuten **vapaa** ja **jaettujen** **Näytä kaikki** .  Kun olet valinnut hinnoittelu tason, napsauta **Valitse** -painiketta.  Edellinen- **sovelluksen palvelusopimus** -sivu valitsemalla **OK**.

5. Valitse **Luo**. Mobile-sovelluksen taustassa valmistelu voi kestää muutaman minuutin.  Kun Mobile-sovelluksen Taustajärjestelmä on valmisteltu-portaalin avautuu for Mobile-sovelluksen Taustajärjestelmä **asetukset** -sivu.

Kun Mobile-sovelluksen Taustajärjestelmä on luotu, voit valita aiemmin luodun SQL-tietokannan yhdistäminen Mobile-sovelluksen taustatietokannan tai Luo uusi SQL-tietokanta.  Tässä osassa on SQL-tietokannan luominen.

> [AZURE.NOTE]Jos sinulla on jo tietokannan eri sijaintiin kuin mobiilisovelluksen taustatietokannan, voit valita sen sijaan, **Käytä aiemmin luotua tietokantaa** ja valitse sitten tietokannan. Käytä tietokannan eri sijainnissa ei suositella suurempi viiveitä vuoksi.

6. Valitse uusi Mobile-sovelluksen, Taustajärjestelmä **asetukset** > **Mobile-sovelluksen** > **tietojen** > **+ Lisää**.

7. Valitse **Lisää tietoyhteys** -sivu **SQL-tietokanta - tarvittavat asetusten määrittäminen** > **Luo uusi tietokanta**.  Kirjoita uuden tietokannan nimi **nimi** -kenttään.

8. Valitse **Server**.  Kirjoita **Uusi palvelin** -sivu- **palvelimen nimi** -kenttään yksilöivä nimi ja sopivan **palvelimen järjestelmänvalvojan kirjautuminen** ja **salasanaa**.  Varmista, että **Salli azure services palvelinta** on valittuna.  Valitse **OK**.

    ![Azure SQL-tietokannan luominen][6]

9. Valitse **Uusi tietokanta** -sivu valitsemalla **OK**.

10. **Lisää tietoyhteys** -sivu uudelleen käyttöön **yhteysmerkkijonon**, kirjoittamalla sisäänkirjautuminen- ja salasana, jonka ilmoitit tietokannan luomiseen.  Jos käytät aiemmin luotuun tietokantaan, antaa tietokannan kirjautumisen tunnistetiedot.  Kun syötetty, valitse **OK**.

11. Takaisin **Lisää tietoyhteys** -sivu, valitse **OK** tietokannan luomiseen.

<!--- END OF ALTERNATE INCLUDE -->

Tietokannan luominen voi kestää muutaman minuutin kuluttua.  **Ilmoitusalueen** avulla voit valvoa edistymistä käyttöönoton.  Ennen kuin tietokanta on otettu käyttöön onnistuneesti ei edistyminen.  Kun otettu, yhteysmerkkijonon luodaan Mobile Taustajärjestelmä sovelluksen asetusten SQL-tietokanta-esiintymässä.  Näet sovelluksen-asetus **asetukset** > **Sovellusasetukset** > **yhteyden merkkijonoja**.

### <a name="howto-tables-auth"></a>Toimintaohje: Vaadi todennus käyttämään taulukot

Jos haluat käyttää sovelluksen palvelun todennusta ja taulukoiden päätepisteen, sinun on määritettävä App todentaminen [Azure portal] ensin.  Lisätietoja käyttöoikeuksien määrittämisestä Azure-sovelluksen palvelussa tarkistaa tunnistetietojen toimittaja, joita aiot käyttää määritys-opas:

- [Azure Active Directory käyttöoikeuksien määrittämisestä]
- [Facebook-käyttöoikeuksien määrittämisestä]
- [Google-käyttöoikeuksien määrittämisestä]
- [Microsoft Authentication määrittäminen]
- [Twitter-käyttöoikeuksien määrittämisestä]

Jokaisessa taulukossa on access-ominaisuuden, jonka avulla voidaan hallita taulukkoon.  Seuraavassa esimerkissä näkyy nimipalvelimet määritetyn taulukon todennus vaaditaan kanssa.

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Access-ominaisuudella voi olla jokin kolmesta vaihtoehdosta

  - *Anonyymi* ilmaisee, että asiakassovellus voi lukea tietoja ilman todennus
  - *todennettu* tarkoittaa, että asiakassovellus on lähettää kelvollinen todennus-tunnuksen, jonka pyyntö
  - *käytöstä* ilmaisee, että tässä taulukossa on poistettu käytöstä

Jos access-ominaisuus ei ole määritetty, Todentamattomille käyttö on sallittu.

### <a name="howto-tables-getidentity"></a>Toimintaohje: todennus saatavat käyttäminen taulukoissa

Voit määrittää eri saatavat, jossa pyydetään, kun käyttöoikeuksien määrittämisen jälkeen.  Nämä vaateita, jotka eivät ole yleensä käytettävissä kautta `context.user` objekti.  Kuitenkin ne voi hakea käyttämällä `context.user.getIdentity()` menetelmää.  `getIdentity()` Menetelmä palauttaa Promise, joka korjaa objektiin.  Objekti on liittyvät haluttuihin todentamismenetelmän (facebook, google, Twitterissä, microsoftaccount käytettävissä tai aad) mukaan.

Esimerkiksi jos määrität Microsoft Account todennus ja pyydä sähköpostiosoitteet väittää, voit lisätä sähköpostiosoite tietueeseen seuraavat taulukon ohjaimella:

    var azureMobileApps = require('azure-mobile-apps');

    // Create a new table definition
    var table = azureMobileApps.table();

    table.columns = {
        "emailAddress": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;
    table.access = 'authenticated';

    /**
    * Limit the context query to those records with the authenticated user email address
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function queryContextForEmail(context) {
        return context.user.getIdentity().then((data) => {
            context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
            return context.execute();
        });
    }

    /**
    * Adds the email address from the claims to the context item - used for
    * insert operations
    * @param {Context} context the operation context
    * @returns {Promise} context execution Promise
    */
    function addEmailToContext(context) {
        return context.user.getIdentity().then((data) => {
            context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
            return context.execute();
        });
    }

    // Configure specific code when the client does a request
    // READ - only return records belonging to the authenticated user
    table.read(queryContextForEmail);

    // CREATE - add or overwrite the userId based on the authenticated user
    table.insert(addEmailToContext);

    // UPDATE - only allow updating of record belong to the authenticated user
    table.update(queryContextForEmail);

    // DELETE - only allow deletion of records belong to the authenticated uer
    table.delete(queryContextForEmail);

    module.exports = table;

Jos haluat nähdä, mitä vaateita, jotka ovat käytettävissä, käytä selaimen tarkastelemiseen `/.auth/me` sivuston päätepiste.

### <a name="howto-tables-disabled"></a>Toimintaohje: poistaminen käytöstä käyttöoikeuksien taulukon tiettyjen toimintojen

Lisäksi näy taulukon, access-ominaisuuden avulla voit hallita yksittäisten toimintojen.  On neljä toiminnot:

  - *luku* on taulukon RESTful HAE-toiminto
  - *Lisää* on taulukon RESTful POST-toiminto
  - *Päivitä* on taulukon RESTful KORJAUSTIEDOSTON-toiminto
  - *Poista* on taulukon RESTful Poista-toiminto

Jos esimerkiksi haluat on vain luku-Todentamattomille taulukon:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>Toimintaohje: Muokkaa kyselyä, jota käytetään taulukkotoiminnot

Yleinen vaatimus taulukon toiminnoissa on rajoitettu Näytä tiedot.  Voit esimerkiksi tarjota taulukko, joka on merkitty todennetun käyttäjän siten, että voit lukea tai päivittää oman tietueita vain.  Seuraavassa taulukossa määritelmä on toiminto:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    });

    module.exports = table;

Toiminnot, jotka yleensä kyselyn suorittaminen on Kyselyominaisuus, joka voit säätää kanssa where lause. Kyselyominaisuus on [QueryJS] -objekti, jota käytetään OData-kyselyn muuntaminen jotakin, mitä tietoja Taustajärjestelmä voit käsitellä.  Yksinkertainen tasa tapauksissa (kuten edellisessä yhteen) voidaan kartta. Voit lisätä tiettyyn SQL-lauseet:

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>Toimintaohje: määrittämistä Pehmeä Poista taulukko

Pehmeä poistaminen ei poista tietueet itse asiassa.  Sen sijaan se merkitsee ne poistetaan tietokannasta määrittämällä sarakkeen poistettu tosi.  Azure Mobile Apps SDK poistetaan Pehmeä poistaa tietueita automaattisesti tulokset, ellei Mobile asiakkaan SDK IncludeDeleted().  Jos haluat määrittää taulukon Pehmeä poistettavaksi, Määritä `softDelete` taulukon määritystiedosto-ominaisuus:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Sinun tulee luoda poistetaan tietueiden – joko asiakassovellus, WebJob Azure-funktiota tai mukautetun Ohjelmointirajapinnan kautta.

### <a name="howto-tables-seeding"></a>Toimintaohje: Määritä tietokantasi tiedoilla

Kun luot uuden sovelluksen, haluat ehkä Määritä taulukon tiedoilla.  Voit tehdä tämän taulukon määritys JavaScript-tiedostoon seuraavasti:

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

Tietojen valuuttamuunnosten vain valmis, kun taulukko luodaan Azure Mobile Apps SDK-paketissa.  Jos taulukko on jo olemassa tietokannassa, ei ole tietoja syötetään taulukon.  Jos dynaaminen rakenne on otettu käyttöön, rakenne johdetaan seeded tiedoista.

On suositeltavaa, että olet kutsussa `tables.initialize()` menetelmä luo taulukko, kun palvelu käynnistyy.

### <a name="Swagger"></a>Toimintaohje: Swagger tuen ottaminen käyttöön

Azure palvelun mobiilisovellukset sisältää valmiit [Swagger] tuki.  Swagger tuen ottaminen käyttöön, asenna swagger-käyttöliittymän ensin riippuvuus:

    npm install --save swagger-ui

Kun asennettu, voit ottaa Azure-mobiilisovellukset konstruktoria Swagger-tuki:

    var mobile = azureMobileApps({ swagger: true });

Voit ehkä vain haluat kehittäminen versioissa Swagger tuen ottaminen käyttöön.  Voit tehdä tämän välityksellä `NODE_ENV` asetus:

    var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });

Http://_yoursite_.azurewebsites.net/swagger swagger päätepisteen sijaitsee.  Voit käyttää Swagger-Käyttöliittymän välityksellä `/swagger/ui` päätepiste.  Jos valitset edellyttää todennusta koko-sovelluksen kautta, Swagger tuottaa virheen.  Saat parhaan tuloksen, valitse Salli Todentamattomille pyynnöt kautta Azure-sovelluksen todentaminen / luvan asetukset, sitten ohjaavat todennus `table.access` ominaisuus.

Voit myös lisätä Swagger-vaihtoehto, jos haluat oman `azureMobile.js` tiedoston, jos haluat Swagger tuki vain, kun kehittäminen paikallisesti.

## <a name="a-namepushpush-notifications"></a><a name="push">Push-ilmoitukset

Mobile-sovellusten integroitu Azure ilmoituksen keskittimet, jotta voit lähettää kohdennettujen push-ilmoitukset laitteiden miljoonia kaikissa pää ympäristöissä. Ilmoitus keskittimet avulla voit lähettää push-ilmoitukset iOS-laitteiden Android- ja Windows. Saat lisätietoja kaikkea voit tehdä ilmoituksen keskittimet artikkelissa [Ilmoituksen keskittimet yleiskatsaus](../notification-hubs/notification-hubs-push-notification-overview.md).

### </a><a name="send-push"></a>Toimintaohje: Lähetä push-ilmoitukset

Seuraava koodi esitetään, kuinka voit lähettää lähetyksen push-ilmoitus rekisteröity iOS-laitteita push-objektin avulla:

    // Create an APNS payload.
    var payload = '{"aps": {"alert": "This is an APNS payload."}}';

    // Only do the push if configured
    if (context.push) {
        // Send a push notification using APNS.
        context.push.apns.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Luomalla mallin push-rekisteröintiä asiakkaasta lähettää viestin mallin push-laitteisiin kaikki tuetut käyttöympäristöt sen sijaan. Seuraava koodi näytetään, miten malli ilmoituksen lähettäminen:

    // Define the template payload.
    var payload = '{"messageParam": "This is a template payload."}';

    // Only do the push if configured
    if (context.push) {
        // Send a template notification.
        context.push.send(null, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }


###<a name="push-user"></a>Toimintaohje: Lähetä push-ilmoitukset todennetut käyttäjälle ohjauskoodien käyttäminen

Todennetun käyttäjän Rekisteröi push-ilmoituksia, kun käyttäjä tunnus tunnisteen lisätään automaattisesti rekisteröinnin. Käyttämällä tätä tunnistetta, voit lähettää push-ilmoitukset kaikki tietyn käyttäjän rekisteröityjen laitteet. Seuraava koodi saa pyynnön käyttäjän SUOJAUSTUNNUS ja lähettää mallin push ilmoituksen jokaisen laitteen rekisteröinti hänen käyttöönsä:

    // Only do the push if configured
    if (context.push) {
        // Send a notification to the current user.
        context.push.send(context.user.id, payload, function (error) {
            if (error) {
                // Do something or log the error.
            }
        });
    }

Kun rekisteröiminen todennetut asiakaskoneelta push-ilmoitukset, varmista, että tarkistus on valmis ennen kuin yrität rekisteröinti.

## <a name="CustomAPI"></a>Mukautetun API

###  <a name="howto-customapi-basic"></a>Toimintaohje: määrittää mukautetun Ohjelmointirajapinta


Lisäksi tietojen käytön API /tables päätepisteen kautta Azure-mobiilisovellukset voit tarkistustyökalua mukautetun API.  Mukautettu ohjelmointirajapinnan on määritetty samalla tavalla kuin taulukon määritelmien ja voivat käyttää kaikki samat toiminnot, kuten todennusta.

Jos haluat käyttää sovelluksen palvelun todennusta ja mukautettu-Ohjelmointirajapinta, sinun on määritettävä sovelluksen todentaminen [Azure portal] ensin.  Lisätietoja käyttöoikeuksien määrittämisestä Azure-sovelluksen palvelussa tarkistaa tunnistetietojen toimittaja, joita aiot käyttää määritys-opas:

- [Azure Active Directory käyttöoikeuksien määrittämisestä]
- [Facebook-käyttöoikeuksien määrittämisestä]
- [Google-käyttöoikeuksien määrittämisestä]
- [Microsoft Authentication määrittäminen]
- [Twitter-käyttöoikeuksien määrittämisestä]

Mukautetun ohjelmointirajapinnan määritetään samalla tavoin kuin taulukoiden Ohjelmointirajapinnan.

1. Luo **api** -hakemisto
2. Luo API määritelmä JavaScript-tiedoston **api** -kansio.
3. Tuo menetelmän avulla voit tuoda **api** -kansio.

Seuraavassa on prototyypin api määritelmä on käytetty aiemmin basic-sovelluksen otoksen perusteella.

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Voit esimerkiksi API, joka palauttaa palvelimen päivämäärän _Date.now()_ -menetelmällä.  Näin api/date.js-tiedosto:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

Kunkin parametrin on vakio RESTful verbien - GET, viestin, korjaus tai poista.  Menetelmä on vakio [ExpressJS Middleware] -funktio, joka lähettää tarvittavat tulosteen.

### <a name="howto-customapi-auth"></a>Toimintaohje: Vaadi todennus käyttämään mukautettua API

Azure Mobile-sovellusten SDK toteuttaa todennus taulukoiden päätepiste ja mukautetun ohjelmointirajapinnan samalla tavalla.  Lisää todennus kehittänyt edellisessä osassa ohjelmointirajapinnan lisäämällä **access** -ominaisuus:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

Voit myös määrittää käyttöoikeuden tietyn toimintoja:

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        }
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

Mukautetun ohjelmointirajapinnan todennusta on käytettävä samaa tunnuksen, jota käytetään taulukoiden päätepisteen.

### <a name="howto-customapi-auth"></a>Toimintaohje: käsittele suurten tiedostojen lataaminen palvelimeen

Azure Mobile-sovellusten SDK käyttää [tekstissä jäsentimen middleware](https://github.com/expressjs/body-parser) Hyväksy ja toistaa lähettämäsi sisällön tekstissä.  Voit määrittää valmiiksi leipäteksti-jäsentimen Hyväksy suuri tiedosto latauksia:

    var express = require('express'),
        bodyParser = require('body-parser'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Set up large body content handling
    app.use(bodyParser.json({ limit: '50mb' }));
    app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

Tiedosto on base-64-koodattu ennen lähettämistä.  Tämä suurentaa todellinen lataus (ja näin ollen koon sinun on otettava huomioon).

### <a name="howto-customapi-sql"></a>Toimintaohje: Suorita mukautetut SQL-lauseet

Azure Mobile Apps SDK pääsee käyttämään koko yhteydessä – pyyntö kohdetta, joilla voit suorittaa Parametroitu SQL-lauseet määritetyn tietopalvelu helposti:

    var api = {
        get: function (request, response, next) {
            // Check for parameters - if not there, pass on to a later API call
            if (typeof request.params.completed === 'undefined')
                return next();

            // Define the query - anything that can be handled by the mssql
            // driver is allowed.
            var query = {
                sql: 'UPDATE TodoItem SET complete=@completed',
                parameters: [{
                    completed: request.params.completed
                }]
            };

            // Execute the query.  The context for Azure Mobile Apps is available through
            // request.azureMobile - the data object contains the configured data provider.
            request.azureMobile.data.execute(query)
            .then(function (results) {
                response.json(results);
            });
        }
    };

    api.get.access = 'authenticated';
    module.exports = api;

## <a name="Debugging"></a>Virheenkorjaus, helposti taulukoita ja helposti API

### <a name="howto-diagnostic-logs"></a>Toimintaohje: virheenkorjaus, diagnosointi ja Azure-mobiilisovellukset vianmääritys

Sovelluksen Azure-palvelu sisältää useita virheenkorjaus ja tekniikoita Node.js sovellusten vianmääritys.
Lisätietoja on seuraavissa artikkeleissa Aloita Node.js Mobile Taustajärjestelmä vianmäärityksessä:

- [Seurannan Azure sovelluksen-palvelu]
- [Ota Diagnostiikan kirjaus Azure sovelluksen-palvelussa]
- [Azure sovelluksen-palvelun Visual Studiossa vianmääritys]

Node.js sovellusten käyttää monenlaisia vianmäärityslokeihin työkalut.  Sisäisesti Azure Mobile sovellusten Node.js SDK käyttää Diagnostiikan kirjaus [Winston] .  Kirjaaminen otetaan automaattisesti käyttöön virheenkorjaustilan ottaminen käyttöön tai määrittämällä **MS_DebugMode** sovelluksen asetus [Azure portal]tosi. Luotu lokit näkyvät [Azure portal]vianmäärityslokit.

### <a name="in-portal-editing"></a><a name="work-easy-tables"></a>Toimintaohje: Azure-portaalissa helposti taulukoiden käsitteleminen

Helposti portaalin taulukoiden avulla voit luoda ja portaalissa oikealle taulukoiden käsitteleminen. Voit myös muokata taulukon toimintoja käyttämällä sovelluksen Service-editorin.

Kun napsautat **helposti taulukoiden** taustassa sivuston asetusten, voit lisätä, muokata tai taulukon poistaminen. Näet myös taulukon tiedot.

![Helppo taulukoiden käsitteleminen](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

Seuraavat komennot ovat käytettävissä taulukon komentopalkki:

+ **Muuta käyttöoikeuksia** - Muokkaa käyttöoikeuksia luku, lisätä, päivittää ja poistaa taulukon toimintoja. 
  Vaihtoehdot ovat anonyymin käytön edellyttää todennusta tai poistaa käytöstä kaikki toiminto. 
+ **Muokkaa komentosarja** - komentosarjatiedosto taulukolle on avattu sovelluksen Service editoria.
+ **Rakenteen hallinta** - Lisää tai poista sarakkeita tai muuta taulukkoindeksin.
+ **Poista taulukko** - katkaisee aiemmin luotuun taulukkoon on kaikkien tietorivien poistaminen mutta jätä rakenteen muuttumattomana.
+ **Poista rivejä** - Poista yksittäisiä tietorivejä.
+ **Näytä streaming lokit** - yhdistää streaming log-palvelu sivustossasi.

###<a name="work-easy-apis"></a>Toimintaohje: helposti ohjelmointirajapinnan käyttäminen Azure-portaalissa

Helposti ohjelmointirajapinnan portaalissa avulla voit luoda ja käyttää mukautettuja ohjelmointirajapinnan suoraan portaalin. Voit muokata editorilla App Service API komentosarjoja.

Napsautettaessa **Helposti ohjelmointirajapinnan** taustassa sivuston asetusten voit lisätä, muokata tai poistaa mukautetun API-päätepisteen.

![Helppo ohjelmointirajapinnan käyttäminen](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

Portaalissa voit muuttaa tietyn HTTP-toiminnon käyttöoikeudet, Muokkaa API-komentosarjatiedosto App Service editoria tai tarkastella streaming lokeja.

###<a name="online-editor"></a>Toimintaohje: Muokkaa koodia-sovelluksen palvelun editorissa

Azure portaalin avulla voit muokata Node.js Taustajärjestelmä komentosarjan tiedostojen App Service editoria eikä sinun tarvitse projektin lataaminen paikalliseen tietokoneeseen. Voit muokata komentosarjatiedostoja online-editorissa seuraavasti:

1. Valitse Mobile-sovelluksen-taustatietokannan sivu **kaikki asetukset** > **helposti taulukoita** tai **Helposti API**-taulukon tai API ja valitse sitten **Muokkaa komentosarjaa**. Komentosarjatiedosto avataan-sovelluksen palvelun editorissa.

    ![Sovelluksen Service editoria](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. Tee haluamasi muutokset koodi-tiedostoa online-editorissa. Muutokset tallentuvat automaattisesti, kun kirjoitat.


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android asiakas-pikaopas]: app-service-mobile-android-get-started.md
[Apache Cordova asiakas-pikaopas]: app-service-mobile-cordova-get-started.md
[iOS asiakas-pikaopas]: app-service-mobile-ios-get-started.md
[Xamarin.iOS asiakas-pikaopas]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android asiakas-pikaopas]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms asiakas-pikaopas]: app-service-mobile-xamarin-forms-get-started.md
[Windows-kaupan asiakas-pikaopas]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[Offline-tietojen synkronointi]: app-service-mobile-offline-data-sync.md
[Azure Active Directory käyttöoikeuksien määrittämisestä]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook-käyttöoikeuksien määrittämisestä]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google-käyttöoikeuksien määrittämisestä]: app-service-mobile-how-to-configure-google-authentication.md
[Microsoft Authentication määrittäminen]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter-käyttöoikeuksien määrittämisestä]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App palvelun käyttöönotto-opas]: ../app-service-web/web-sites-deploy.md
[Seurannan Azure sovelluksen-palvelu]: ../app-service-web/web-sites-monitor.md
[Ota Diagnostiikan kirjaus Azure sovelluksen-palvelussa]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Azure sovelluksen-palvelun Visual Studiossa vianmääritys]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[Määritä solmu-versio]: ../nodejs-specify-node-version-azure-apps.md
[Käytä solmu moduulit]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/
[Swagger]: http://swagger.io/

[Azure portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[Valitse GitHub basicapp Esimerkki]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[Valitse GitHub TODO malli]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[Esimerkkejä, joiden hakemistoon GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Visual Studio työkalut node.js 1.1]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[MSSQL Node.js paketti]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
