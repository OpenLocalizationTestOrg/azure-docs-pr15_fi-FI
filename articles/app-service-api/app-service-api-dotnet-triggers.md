<properties 
    pageTitle="Sovelluksen Service API app käynnistimien | Microsoft Azure" 
    description="Käynnistimien ottamisesta käyttöön Azure App Service API-sovelluksessa" 
    services="logic-apps" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="rachelap"/>

# <a name="azure-app-service-api-app-triggers"></a>Azure App Service API app Käynnistimet

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee API sovellusten 2014 – 12-01 – esikatselu rakenteen versio.


## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten voit toteuttaa API app käynnistimien ja käyttää niitä logiikan-sovelluksen.

Kaikki tämän artikkelin koodikatkelmat kopioidaan [FileWatcher API App koodin malli](http://go.microsoft.com/fwlink/?LinkId=534802). 

Huomaa, että sinun on ladattava seuraavat nuget-paketti, voit luoda ja suorittaa tämän artikkelin koodi: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/).

## <a name="what-are-api-app-triggers"></a>Mitkä ovat API app käynnistimien?

On käytetty vaihtoehto API-sovelluksen Kyselysäännön tapahtuman niin, että asiakkaiden API-sovelluksen voi kestää sopiva vaihtoehto vastauksena tapahtuma. REST API-pohjainen järjestelmä, joka tukee tässä skenaariossa kutsutaan API-sovelluksen käynnistintä. 

Esimerkiksi oletetaan asiakas-koodi on käytössä [yhdistimen API Twitter-sovelluksen](../app-service-logic/app-service-logic-connector-twitter.md) ja koodi on suoritettava toiminnon uusi tweets, jotka sisältävät tiettyjä sanoja perusteella. Tässä tapauksessa voit määrittää kyselyä tai push käynnistimen tähän tarpeeseen helpottamiseksi.

## <a name="poll-trigger-versus-push-trigger"></a>Kyselyn käynnistin ja push käynnistin

Tällä hetkellä tueta kahdentyyppisiä käynnistimet:

- Kyselyn käynnistimen - asiakasohjelman tekee kyselyn ilmoituksen tapahtuman ottaa käynnistyy API-sovellukseen 
- Push-käynnistimen - asiakas on ilmoituksen API-sovellus, kun tapahtuman käynnistyy 

### <a name="poll-trigger"></a>Kyselyn käynnistin

Kyselyn käynnistin on toteutettu säännöllisesti REST-Ohjelmointirajapinnalla ja odottaa sen asiakkaiden (kuten logiikan-sovellus), jotta saat ilmoituksen äänestyksen järjestäminen. Asiakas voi säilyttää tilan, kun kysely käynnistin itse on tilattomien. 

Seuraavat pyynnön ja vastauksen pakettien koskevat tiedot havainnollistetaan keskeisiä tietyiltä kyselyn käynnistimen sopimuksen:

- Pyyntö
    - HTTP-menetelmää: hankkiminen
    - Parametrit
        - triggerState - tämän valinnaisten parametrien avulla voit määrittää niiden tilaan niin, että kysely käynnistin oikein päättää, palaa ilmoitus vai ei määritetty tilaan perustuvia käyttävät.
        - API-kohtaisia parametrit
- Vastaus
    - Tilan koodi **200** - pyyntö on voimassa ja on käynnistin ilmoitus. Ilmoituksen sisältö on vastauksen teksti. Vastauksessa "Uudelleen-jälkeen" ylätunnisteen ilmaisee, että muut ilmoituksen tietojen noutaa myöhemmin pyynnön puhelun.
    - Tilan koodi **202** - pyyntö on kelvollinen, mutta ei ole käynnistin uusi ilmoitus.
    - Tilan koodi **4xx** – pyyntö ei kelpaa. Asiakas ei olisi uudelleen pyynnön.
    - Tilan koodi **5xx** – pyyntö on tullut sisäinen palvelinvirhe ja/tai väliaikainen ongelma. Asiakkaan tulee uudelleen pyynnön.

Seuraavat koodikatkelman on esimerkki kyselyn käynnistimen ottamisesta käyttöön.

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

Testaa kyselyä käynnistin, toimi seuraavasti:

1. Ota API-sovellus julkisia **Anonyymi**todentaminen-asetus käyttöön.
2. Soita kosketa tiedoston **kosketus** -toimintoa. Seuraavassa kuvassa on esimerkki pyyntö Postman kautta.
   ![Puhelun Touch toiminnon Postman kautta](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. Soita kyselyn käynnistin **triggerState** -parametrin arvoksi aikaleiman ennen vaiheeseen 2. Seuraavassa kuvassa on esimerkki pyynnön Postman kautta.
   ![Puhelun kyselyn käynnistimen Postman kautta](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### <a name="push-trigger"></a>Push-käynnistin

Push-käynnistin on toteutettu säännöllisesti REST-Ohjelmointirajapinnalla, joka vie ilmoitukset asiakkaille, joka on rekisteröity saada ilmoituksen tietyn tapahtumat.

Seuraavat pyynnön ja vastauksen pakettien koskevat tiedot osoittavat avaimen tietyiltä push käynnistimen sopimuksen.

- Pyyntö
    - HTTP-menetelmää: valitseminen
    - Parametrit
        - triggerId: tarvitaan - peittävä merkkijono (kuten GUID-tunnus), joka edustaa push käynnistimen rekisteröinti.
        - callbackUrl: tarvitaan - URL-osoite takaisinkutsu ongelma, kun tapahtuma käynnistyy. Kutsu on yksinkertainen kirjaa HTTP-puhelu.
        - API-kohtaisia parametrit
- Vastaus
    - Tilan koodi **200** - pyyntö rekisteröidä asiakkaan onnistuu.
    - Tilan koodi **4xx** – pyyntö ei kelpaa. Asiakas ei olisi uudelleen pyynnön.
    - Tilan koodi **5xx** – pyyntö on tullut sisäinen palvelinvirhe ja/tai väliaikainen ongelma. Asiakkaan tulee uudelleen pyynnön.
- Takaisinsoitto
    - HTTP-menetelmää: VIESTIIN
    - Pyydä leipäteksti: ilmoituksen sisältöä.

Seuraavat koodikatkelman on esimerkki ottamisesta käyttöön push-käynnistäminen:

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

Testaa kyselyä käynnistin, toimi seuraavasti:

1. Ota API-sovellus julkisia **Anonyymi**todentaminen-asetus käyttöön.
2. Siirry [http://requestb.in/](http://requestb.in/) luominen RequestBin, jotka toimivat takaisinsoitto URL-osoitteena.
3. Ota push-käynnistimen kuin **triggerId** GUID-tunnus ja **callbackUrl**kuin RequestBin URL-osoite.
   ![Puhelun Push käynnistimen Postman kautta](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. Soita kosketa tiedoston **kosketus** -toimintoa. Seuraavassa kuvassa on esimerkki pyyntö Postman kautta.
   ![Puhelun kosketustoimintojen toimintoa Postman kautta](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. Valitse Vahvista, että push käynnistimen takaisinkutsu on käynnistää ominaisuuden tulosteen RequestBin.
   ![Puhelun kyselyn käynnistimen Postman kautta](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### <a name="describe-triggers-in-api-definition"></a>Kuvaile käynnistimien API määritys

Jälkeen käyttöönoton Käynnistimet ja käyttöönotossa Azure API-sovelluksen, siirry Azure esikatselu-portaalissa **API määritys** -sivu ja näet, että käynnistimet tunnistetaan automaattisesti käyttöliittymä, jossa Swagger 2.0-Ohjelmointirajapinnan määritelmän API-sovelluksen avulla.

![API määritys-sivu](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

Jos **Swagger Lataa** -painiketta ja avaa JSON-tiedosto, näet tulokset seuraavankaltaiselta:

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

Tunniste-ominaisuus **x-ms-schedular-käynnistin** on miten käynnistimet on kuvattu API määritys ja on automaattisesti lisäämiä API app yhdyskäytävän kun pyydät API määritelmän yhdyskäytävän kautta, jos jokin seuraavista ehdoista pyynnön. (Voit myös lisätä tämän ominaisuuden manuaalisesti.)

- Kyselyn käynnistin
    - Jos HTTP-menetelmä ei **HAE**.
    - Jos **toimintotunnukseksi** -ominaisuus sisältää merkkijonon **käynnistintä**.
    - Jos **parameters** -ominaisuus sisältyy parametrin **nimi** -ominaisuuden arvoksi **triggerState**.
- Push-käynnistin
    - Jos HTTP-menetelmän **valitseminen**.
    - Jos **toimintotunnukseksi** -ominaisuus sisältää merkkijonon **käynnistintä**.
    - Jos **parameters** -ominaisuus sisältyy parametrin **nimi** -ominaisuuden arvoksi **triggerId**.

## <a name="use-api-app-triggers-in-logic-apps"></a>API-sovelluksen käynnistimillä logiikan-sovelluksissa

### <a name="list-and-configure-api-app-triggers-in-the-logic-apps-designer"></a>Luettelo ja määritä API app käynnistimien logiikan sovellusten suunnittelussa

Jos luot logiikan app samaan resurssiryhmään API-sovellus, osaat lisääminen suunnittelutyökalun alusta kätevästi napsauttamalla sitä. Seuraavat kuvat kuvaavat näin:

![Käynnistimien logiikan App Designerissa](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![Määritä kyselyn käynnistimen logiikan sovelluksen suunnittelussa](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![Määritä Push käynnistimen logiikan sovelluksen suunnittelussa](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## <a name="optimize-api-app-triggers-for-logic-apps"></a>Optimoi logiikan sovellusten API app Käynnistimet

Kun olet lisännyt käynnistimien API-sovellukseen, on muutama seikka voit parantaa käyttökokemusta API-sovelluksen käytöstä logiikan-sovelluksessa.

Esimerkiksi seuraava lauseke logiikan-sovelluksessa on asetettava **triggerState** parametri kyselyn käynnistimien. Tämä lauseke kannattaa arvioi logiikan sovelluksen käynnistimen viimeisen kutsu ja palauttaa arvon.  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

Huomautus: Selitystä yllä olevan lausekkeen käytettävistä funktioista viitata [Logiikan App työnkulun Definition Language](https://msdn.microsoft.com/library/azure/dn948512.aspx)ohjeet.

Logiikan sovelluksen käyttäjät on antamaan yläpuolella lausekkeen **triggerState** -parametrin käynnistin käytettäessä. On mahdollista, että arvoksi ennalta määritetty logiikan app suunnittelija tunniste-ominaisuus **x-ms-ajoitus-suositus**kautta.  **X-ms-näkyvyyden** tunniste-ominaisuus voi määrittää arvon *sisäinen* niin, että itse parametri ei tule näkyviin suunnittelija.  Seuraavat koodikatkelman havainnollistaa.

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

Push käynnistimien **triggerId** -parametri on yksilöivät logiikan-sovellus. Suositeltu paras käytäntö on tämän ominaisuuden määrittäminen työnkulun nimen käyttämällä seuraavaa lauseketta:

    @workflow().name

**X-ms-ajoitus-suositus** ja **x-ms-näkyvyys** tunniste-ominaisuuksien käyttäminen sen API definiton, API-sovelluksen esille logiikan app Designerin lauseketta käyttäjän määrittämään automaattisesti.

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### <a name="add-extension-properties-in-api-defintion"></a>Lisää tunniste-ominaisuus-Ohjelmointirajapinnan määritystaulukon

Metatietoja – kuten tunnisteen ominaisuudet **x-ms-ajoitus-suositus** ja **x-ms-näkyvyys** - voi lisätä API-määritystaulukon jollakin seuraavista tavoista: staattinen tai dynaaminen.

Staattinen metatietojen voit suoraan */metadata/apiDefinition.swagger.json* -tiedoston muokkaaminen projektin ja lisää ominaisuuksia manuaalisesti.

API sovellusten dynaaminen metatietojen avulla voit muokata Lisää toiminto-suodatin, jonka voit lisätä laajennusten SwaggerConfig.cs-tiedosto.

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


Seuraavassa on esimerkki siitä, miten tähän luokkaan voidaan toteuttaa helpottamiseksi dynaaminen metatiedot-skenaario.

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 
