<properties
    pageTitle="Ota offline-synkronoinnin, kun Azure Mobile-sovellus (Cordova) | Microsoft Azure"
    description="Opettele käyttämään App palvelun Mobile-sovelluksen välimuisti ja synkronoi offline-tilassa tietojen Cordova-sovelluksessa"
    documentationCenter="cordova"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-cordova-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Cordova-mobiilisovelluksen offline-synkronoinnin ottaminen käyttöön

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

Tässä opetusohjelmassa esitellään Azure-mobiilisovellukset offline-synkronointi-ominaisuuden Cordova varten. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa. Muutokset tallennetaan paikallisessa tietokannassa. Kun laite on online-tilaan, muutokset synkronoidaan remote-palveluun.

Tässä opetusohjelmassa perustuu Cordova pikaopas-ratkaisun Mobile-sovellukset, jotka voit luoda, kun suoritat opetusohjelman [Apache Cordova nopeasti Käynnistä-painiketta]. Tässä opetusohjelmassa päivitetään lisäämään offline-tilassa Azure Mobile-sovellusten ominaisuuksien pikaopas-ratkaisun. Olemme korostaa myös offline-kohtaisia koodi-sovelluksessa.

Saat lisätietoja offline-synkronointi-toiminto on aiheessa [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa]. Lisätietoja Ohjelmointirajapinnan käyttö-kohdassa laajennus [Lueminut] -tiedosto.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Lisää offline-synkronoinnin pikaopas-ratkaisuun

Offline-synkronointi-koodi on lisättävä sovellus. Offline-synkronoinnin edellyttää cordova sqlite-tallennustilan laajennus, jonka saa lisätään automaattisesti sovelluksen kun Azure-mobiilisovellukset laajennus sisältyy projektin. Pikaopas project sisältää sekä näiden laajennukset.

1. Napsauta ratkaisunhallinnassa Visual Studiossa Avaa index.js ja korvaa seuraava koodi

        var client,            // Connection to the Azure Mobile App backend
          todoItemTable;      // Reference to a table endpoint on backend

    koodilla:

        var client,             // Connection to the Azure Mobile App backend
          todoItemTable, syncContext; // Reference to table and sync context

2. Seuraavaksi korvaa seuraava koodi:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    koodilla:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

        // Note: Requires at least version 2.0.0-beta6 of the Azure Mobile Apps plugin
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              deleted: 'boolean',
              complete: 'boolean'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Edellisen koodin lisäykset alusta paikallista ja voit määrittää paikallisen taulukon, jossa käytetään sarakkeiden arvoja vastaavia arvoja, että Azure-tietokannassa takaisin end-näppäinyhdistelmää. (Sinun ei tarvitse sisällyttää kaikki sarakkeen arvot koodi.)

    Saat soittamalla **getSyncContext**viittauksen synkronoinnin yhteydessä. Synkronoinnin yhteydessä auttaa säilyttää taulukon yhteyksien seuranta ja valitseminen muutokset asiakas-sovellus on muokannut **push** kutsutaan kaikki taulukot.

3. Päivitä sovelluksen URL-Osoitteen Mobile-sovelluksen sovelluksen URL-osoite.

4. Seuraavaksi korvaa koodi:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    koodilla:

        // todoItemTable = client.getTable('todoitem');

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (serverRecord, clientRecord, pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Yllä olevaa koodia alustaa synkronoinnin yhteydessä, ja sitten viittaaminen paikallisen taulukon kutsuja getSyncTable (sijaan getTable).

    Tämä koodi käyttää paikallisen tietokannan kaikki luoda, lukea, päivittää ja poistaa (CRUD)-taulukkotoiminnot.

    Tässä esimerkissä suorittaa yksinkertaisen Virheenkäsittely synkronointi ristiriidoista. Todellinen sovelluksen käsitellä eri virheitä, kuten verkon ja palvelimen ristiriidat. Katso koodiesimerkkejä [offline-synkronoinnin malli].

5. Lisää tämä funktio suorittaa todellinen Synkronoi.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    Voit päättää, milloin kannattaa siirtää muutokset Mobile-sovelluksen Taustajärjestelmä kutsumalla **push** **syncContext** -objekti, jota käytetään asiakkaan. Esimerkiksi painike tapahtumakäsittelijä, kuten uusien Synkronoi-painike-sovelluksessa voi lisätä **syncBackend** puhelun tai voit lisätä **addItemHandler** -funktion kutsu synkronoimaan aina, kun uusi kohde on lisätty.

##<a name="offline-sync-considerations"></a>Offline-synkronoinnin huomioon otettavia seikkoja

Näytteen **syncContext** **push** -menetelmää kutsutaan vain sovelluksen käynnistys kirjautumisen takaisinsoitto-funktiossa.  Tosielämän-sovelluksessa voit myös tehdä synkronointi-toiminto käynnistetään manuaalisesti tai verkon tilan muuttuessa.

Kun taulukko, joka on seurattava paikallisen odottavia kohdistaa erotettu kontekstin mukaan, salaus puretaan toiminnon käynnistäminen automaattisesti edellisen konteksti-push. Kun päivität, lisääminen ja suorittamisesta saa tämän esimerkin lisäksi kohteiden voit jättää eksplisiittinen **push** -puhelun, koska voi olla tarpeettomat (ensimmäinen Tarkista nykyisen toiminnon tilan [Lueminut-tiedosto] ).

Annettu koodi pyydetään remote todoItem taulukon kaikki tietueet, mutta on myös mahdollista tietueiden suodattaminen siirtämällä kyselyn tunnuksen ja kyselyn **push**. Lisätietoja on kohdassa *Vaiheittainen Synkronoi* [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

## <a name="optional-disable-authentication"></a>(Valinnainen) Käyttöoikeuksien poistaminen käytöstä

Jos et jo määrittänyt todennus etkä halua ennen testauksen offline-synkronoinnin todentamisen määrittäminen, ulos kirjautuminen takaisinsoitto-funktio kommentin, mutta jättää sisällä koodi takaisinkutsutoiminto uncommented.

Koodin pitäisi näyttää tältä jälkeen kommentoinnista kirjautuminen rivit.

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Nyt sovellus Synkronoi Azure Taustajärjestelmä kanssa, kun käynnistät sovelluksen sijaan, kun kirjaudut sisään.

## <a name="run-the-client-app"></a>Suorita asiakas-sovellus

Offline-synkronoinnin nyt käytössä voit nyt suorittaa asiakassovellus vähintään kerran kullekin ympäristössä täytä paikallista tietokantaa. Myöhemmin jota simuloida offline-ominaisuudet ja paikallista tietojen muokkaamiseen, kun sovellus on offline-tilassa.

## <a name="optional-test-the-sync-behavior"></a>(Valinnainen) Testaa synkronoinnin toiminta

Tässä osassa muokataan asiakkaan projektin simuloida offline-skenaarion käyttämällä omaa Taustajärjestelmä Virheellinen sovelluksen URL-osoite. Kun lisäät tai muutat tieto-osat, muutokset paikallista säilytetään, mutta ei ole synkronoitu Taustajärjestelmä tietojen säilöön, kunnes yhteys on muodostettu uudelleen.

1. Napsauta ratkaisunhallinnassa index.js projektitiedoston avaaminen ja muuta sovelluksen URL-Osoitteen osoittamaan virheellinen URL-osoite, kuten jompikumpi seuraavista:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. Päivitä index.html, CSP `<meta>` elementtiä, jossa on virheellinen URL-Osoitetta.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. Muodosta ja suorita asiakas-sovellus ja huomaat, että poikkeuksen kirjataan konsolissa, kun sovellus yrittää synkronoida taustaan kirjautumisen jälkeen. Kaikki uudet kohteet, voit lisätä on olemassa vain paikallisen säilössä, kunnes ne voivat sijaita mobile Taustajärjestelmä. Asiakas-sovellus toimii aivan kuin on yhdistetty taustatietokannan, tukevat kaikki luoda, lukea, päivitys, poistotoimintoja (CRUD)-toimintoja.

4. Sulje sovellus ja Käynnistä uudelleen ja varmista, että luomasi uudet kohteet ovat pysyviä paikallista.

5. (Valinnainen) Visual Studio avulla voit tarkastella Azure SQL-tietokanta-taulukosta, että taustatietokanta tietoja ei muutettu.

    Avaa Visual Studion **Palvelimen Explorer**. Siirry **Azure**-tietokannan->**SQL-tietokannat**. Tietokannan hiiren kakkospainikkeella ja valitse **Avaa SQL Server objektin Resurssienhallinnassa**. Nyt voit selata SQL-tietokannan taulukkoa ja sen sisältöä.


## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(Valinnainen) Mobiili taustaan haluat uuden yhteyden testaaminen

Tässä osassa muodostat sovelluksen mobile taustatietokannan, jossa varmistaminen sovelluksen palaa online-tilaan. Kun kirjaudut sisään, tiedot synkronoituvat mobile Taustajärjestelmä.

1. Avaa index.js ja korjaa osoittamaan blogisivun URL-Osoitteen sovelluksen URL-osoite.

2. Avaa index.html ja korjata CSP sovelluksen URL-osoite `<meta>` elementti.

3. Muodosta uudelleen ja suorita asiakas-sovellus. Sovellus yrittää synkronoida kanssa mobiilisovelluksessa Taustajärjestelmä kirjautumisen jälkeen. Varmista, että poikkeuksia kirjautunut virheenkorjaus-konsolissa.

4. (Valinnainen) Tarkastella ja päivittää tiedot tai SQL Server Object Explorerissa REST-työkalulla, kuten Fiddler avulla. Huomaa, että tiedot on synkronoitu taustatietokanta ja paikallista välillä.

    Ilmoitus tiedot on synkronoitu tietokannan ja paikallista välillä, ja se sisältää samalla, kun sovellus on katkennut lisätyt kohteet.

## <a name="additional-resources"></a>Lisäresursseja

* [Offline-tietojen synkronointi Azure Mobile-sovellukset]

* [Cloud kansi: Offline-synkronoinnin Azure Mobile Services] \(Huomautus: Mobile-palvelut on videon, mutta offline-synkronointi toimii samalla tavalla kuin Azure Mobile-sovelluksissa\)

* [Visual Studio Tools for Apache Cordova]

## <a name="next-steps"></a>Seuraavat vaiheet

* Tarkastella edistynyt offline-synkronointi-ominaisuudet, kuten [offline-synkronoinnin otoksen] ristiriitojen ratkaisu
* Tarkista offline-synkronoinnin API viittaus [Lueminut-tiedosto]

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova pika-aloitusopas]: app-service-mobile-cordova-get-started.md
[LUEMINUT-TIEDOSTO]: https://github.com/Azure/azure-mobile-apps-js-client#offline-data-sync-preview
[offline-synkronoinnin malli]: https://github.com/shrishrirang/azure-mobile-apps-quickstarts/tree/samples/client/cordova/ZUMOAPPNAME
[Offline-tietojen synkronointi Azure Mobile-sovellukset]: app-service-mobile-offline-data-sync.md
[Cloud kansi: Offline-synkronoinnin Azure Mobile Services-palveluissa]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md
