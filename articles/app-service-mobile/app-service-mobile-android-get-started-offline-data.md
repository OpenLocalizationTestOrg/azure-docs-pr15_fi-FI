<properties
    pageTitle="Ota offline-synkronoinnin, kun Azure Mobile-sovellus (Android)"
    description="Opettele käyttämään palvelua mobiilisovellukset tietojen välimuistin ja synkronoi offline-tilassa Android-sovelluksessa"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Mobile Android-sovelluksen offline-synkronoinnin ottaminen käyttöön

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan Azure-mobiilisovellukset offline-synkronointi-ominaisuuden androidille. Offline-synkronoinnin avulla käyttäjät voivat käsitellä mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa. Muutokset on tallennettu paikalliseen tietokantaan. Kun laite on online-tilaan, muutokset synkronoidaan remote Taustajärjestelmä.

Jos tämä on ensimmäinen käyttökokemusta Azure Mobile-sovellusten, [Android-sovelluksen luominen]opetusohjelman ensin on suoritettava. Jos et käytä ladatut pikaopas palvelimen project-tietojen käytön tunniste-paketit on lisääminen projektiin. Katso lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Saat lisätietoja offline-synkronointi-toiminto on aiheessa [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

## <a name="update-the-app-to-support-offline-sync"></a>Päivitä sovellus tukee offline-synkronointi

Offline-synkronoinnin luku- ja kirjoittaa *synkronointi taulukon* (joko *IMobileServiceSyncTable* interface)-osan **SQLite** tietokannan laitteessasi.

Push-ja tuoda muutokset Azure Mobile-palveluihin ja laitteen välillä, voit käyttää *synkronoinnin yhteydessä* (*MobileServiceClient.SyncContext*), jonka paikallisesti tietojen tallentamiseen paikallisen tietokannan alusta.

1. Valitse `TodoActivity.java`, kommentti olemassa olevan määrityksen, `mToDoTable` ja kommentointi synkronointi taulukon versio:

        private MobileServiceSyncTable<ToDoItem> mToDoTable;

2. : `onCreate` Menetelmä kommentti ulos aiemmin alustus `mToDoTable` ja kommentointi Tämä määritelmä:

        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);

3. Valitse `refreshItemsFromTable` kommentti määritelmää, `results` ja kommentointi Tämä määritelmä:

        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();

4. Kommentin ulos määritelmän `refreshItemsFromMobileServiceTable`.

5. Kommentointi määritelmän `refreshItemsFromMobileServiceTableSyncTable`:

        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }

6. Kommentointi määritelmän `sync`:

        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Sovelluksen testaaminen

Tässä osassa toiminnan kanssa WiFi testaamista ja poista käytöstä WiFi, luo offline-ominaisuudet.

Kun olet lisännyt tiedot, ne ovat paikallinen SQLite säilö säilytetään, mutta ei ole synkronoitu mobiili-palveluun, ennen kuin painat **Päivitä** -painiketta. Muista sovelluksista voi olla erilaiset vaatimukset, kun tiedot on synkronoitava koskeva, mutta esittelyssä Tässä opetusohjelmassa on käyttäjä pyytää niiden erikseen.

Kun painat painike, uusi tausta-tehtävä alkaa. Se ensin Vie kaikki käyttämällä synkronoinnin yhteydessä ja sitten muuttaa vedetään kaikki tiedot Azure paikallisen taulukon paikalliseen säilöön tehdyt muutokset.

### <a name="offline-testing"></a>Offline-tilassa testaaminen

1. Aseta laitteen tai simulator *Lentokone*-tilassa. Tämä luo offline-ominaisuudet.

2. Lisää *ToDo* kohteita tai Merkitse valmiiksi jotkin kohteet. Sulje laitteen tai simulator (tai sulje asiakasistuntojen sovelluksen) ja käynnistä se uudelleen. Varmista, että tekemäsi muutokset on on samanlainen laitteeseen, koska ne säilytetään paikallisen SQLite Storessa.

3. Näytä joko SQL korjaustyökalulla, kuten *SQL Server Management Studiossa*tai REST-asiakas, kuten *Fiddler* tai *Postman*Azure *TodoItem* taulukon sisältöä. Varmista, että uusia kohteita _ei_ ole synkronoitu palvelimen

    + Node.js taustassa, siirry [Azure portal](https://portal.azure.com/)-ja Mobile-sovelluksesta Taustajärjestelmä saat **Helposti taulukoiden** > **TodoItem** voi tarkastella sisältöä `TodoItem` taulukon.
    + Voit tarkastella taulukon sisältö SQL-työkalua, kuten *SQL Server Management Studiossa*tai REST-asiakas, kuten *Fiddler* tai *Postman*.NET-Taustajärjestelmä.

4. Ottaa käyttöön WiFi laitetta tai simulator. Seuraavaksi Napsauta **Päivitä** -painiketta.

5. Voit tarkastella TodoItem tiedot uudelleen Azure-portaalissa. Uudet ja muuttuneet TodoItems pitäisi nyt näkyä.

## <a name="additional-resources"></a>Lisäresursseja

* [Offline-tietojen synkronointi Azure Mobile-sovellukset]

* [Cloud kansi: Offline-synkronoinnin Azure Mobile Services] \(Huomautus: Mobile-palvelut on videon, mutta offline-synkronointi toimii samalla tavalla kuin Azure Mobile-sovelluksissa\)


<!-- URLs. -->

[Offline-tietojen synkronointi Azure Mobile-sovellukset]: app-service-mobile-offline-data-sync.md

[Android-sovelluksen luominen]: app-service-mobile-android-get-started.md

[Cloud kansi: Offline-synkronoinnin Azure Mobile Services-palveluissa]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

