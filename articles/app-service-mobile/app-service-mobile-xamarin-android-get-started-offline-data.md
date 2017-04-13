<properties
    pageTitle="Ota offline-synkronoinnin, kun Azure Mobile-sovellus (Xamarin Android)"
    description="Opettele käyttämään App palvelun Mobile-sovelluksen välimuisti ja synkronoi offline-tilassa tietojen Xamarin Android-sovelluksessa"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Xamarin.Android-mobiilisovelluksen offline-synkronoinnin ottaminen käyttöön

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa esitellään Azure-mobiilisovellukset offline-synkronointi-ominaisuuden Xamarin.Android varten. Offline-synkronoinnin avulla käyttäjät voivat käsitellä mobiilisovelluksessa--tarkasteleminen, lisääminen tai muokkaaminen tiedot – myös silloin, kun yhteyttä ei ole verkossa. Muutokset on tallennettu paikalliseen tietokantaan.
Kun laite on online-tilaan, muutokset synkronoidaan remote-palveluun.

Tässä opetusohjelmassa Päivitä asiakkaan projektin opetusohjelman [Xamarin Android-sovelluksen luominen] tukemaan Azure-mobiilisovellukset offline-tilassa ominaisuuksia. Jos et käytä ladattuja pikaopas palvelimen project-tietojen käytön tunniste-paketit on lisääminen projektiin. Katso lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Saat lisätietoja offline-synkronointi-toiminto on aiheessa [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

## <a name="update-the-client-app-to-support-offline-features"></a>Päivitä asiakas-sovellus tukee offline-ominaisuudet

Azure Mobile-sovelluksen offline-tilassa ominaisuuksien avulla voit käsitellä paikalliseen tietokantaan, kun olet offline-ominaisuudet. Nämä ominaisuudet-sovelluksen käyttöön alusta [SyncContext] paikallisesta liikkeestä. Tämän jälkeen voit viitata taulukon IMobileServiceSyncTable [IMobileServiceSyncTable]-liittymän kautta. SQLite käytetään Paikallinen säilö laitteeseen.

1. Avaa Visual Studiossa, projekti, jota olet suorittanut [Xamarin Android-sovelluksen luominen] opetusohjelmassa NuGet paketti-hallinta.  Etsi ja asenna **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paketti.

2. Avaa ToDoActivity.cs-tiedosto ja kommentointi `#define OFFLINE_SYNC_ENABLED` määritys.

3. Visual Studiossa painamalla **F5** -näppäintä uudelleen ja suorita asiakas-sovellus. Sovellus toimii samalla samalta kuin ennen offline-synkronointi käytössä. Paikallinen tietokanta lisätään kuitenkin nyt tiedot, joita voi käyttää offline-ominaisuudet.

## <a name="update-sync"></a>Päivitä sovellus taustaan yhteyden katkaiseminen

Tässä osassa Katkaise yhteys Mobile-sovelluksen-taustatietokannan voit simuloida offline-tilassa tilanne. Kun olet lisännyt tiedot, poikkeuksen käsittelytoiminnon kertoo, että sovellus on offline-tilassa. Tässä tilassa uudet kohteet lisätään paikallista ja synkronoidaan mobiilisovelluksen taustatietokannan, kun push suoritetaan yhdistetyn tilaan.

1. Muokkaa ToDoActivity.cs jaetun projektin. Muuta **applicationURL** osoittamaan virheellinen URL-osoite:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Voit myös osoittaa offline-tilassa ongelman poistamalla käytöstä wifi ja matkapuhelinverkon verkot laitteeseen tai lentokone syöttötilan.

2. Painamalla **F5** voivat laatia ja suorita sovellus. Huomaa, että synkronointi ei onnistunut päivityksen, kun sovellus käynnistetään.

3. Lisää uudet kohteet ja huomaat, että push epäonnistuu [CancelledByNetworkError]-tilojen aina, kun valitset **Tallenna**. Kuitenkin todo uudet kohteet ovat paikallista ennen kuin he voivat sijaita mobiilisovelluksen Taustajärjestelmä.  Tuotannon-sovelluksessa Jos poikkeukset estää asiakas-sovellus toimii niin kuin se on edelleen yhteydessä mobiilisovelluksen Taustajärjestelmä.

4. Sulje sovellus ja Käynnistä uudelleen ja varmista, että luomasi uudet kohteet ovat pysyviä paikallista.

5. (Valinnainen) Avaa Visual Studion **Palvelimen Explorer**. Siirry **Azure**-tietokannan->**SQL-tietokannat**. Tietokannan hiiren kakkospainikkeella ja valitse **Avaa SQL Server objektin Resurssienhallinnassa**. Nyt voit selata SQL-tietokannan taulukkoa ja sen sisältöä. Varmista, että taustatietokanta tiedot ei muutettu.

6. (Valinnainen) Kyselyn mobile taustatietokannan, HAE kyselyn käyttäminen lomakkeen REST-työkalua, kuten Fiddler tai Postman avulla `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Päivitä sovellus uudelleen Mobile-sovelluksen taustaan

Tässä osassa yhdistää mobiilisovelluksen Taustajärjestelmä sovelluksen. Kun käynnistät sovelluksen ensimmäisen `OnCreate` tapahtuman käsittelijä puhelut `OnRefreshItemsSelected`. Tämä menetelmä soittaa `SyncAsync` synkronointi paikalliseen säilöön taustatietokanta kanssa.

1. Avaa ToDoActivity.cs jaetun projektin ja palata muutoksen **applicationURL** -ominaisuuden.

2. Painamalla **F5** -näppäintä uudelleen ja suorita sovellus. Sovellus Synkronoi paikalliset muutokset ja käyttämällä push-ja salaus puretaan Azure Mobile-sovelluksen Taustajärjestelmä kun `OnRefreshItemsSelected` menetelmä suoritetaan.

3. (Valinnainen) Tarkastella ja päivittää tiedot tai SQL Server Object Explorerissa REST-työkalulla, kuten Fiddler avulla. Ilmoitus tiedot on synkronoitu Azure Mobile-sovelluksen taustatietokanta ja paikallista välillä.

4. Valitse sovellus, täyttävän ne paikallista muutaman kohteen vieressä oleva valintaruutu.

  `CheckItem`puhelut `SyncAsync` synkronointi kunkin valmis kohteeseen Taustajärjestelmä Mobile-sovelluksen kanssa. `SyncAsync`kutsuu push ja salaus puretaan. **Aina, kun suoritat erotettu taulukossa, jossa asiakkaan on tehnyt muutoksia, push aina suoritetaan automaattisesti**. Tällä varmistetaan, että kaikki taulukot yhteydet sekä paikallisen kaupan ovat yhdenmukaisia. Tämä ongelma voi aiheuttaa odottamattomia push. Katso lisätietoja ongelman [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

## <a name="review-the-client-sync-code"></a>Tarkista asiakkaan Synkronoi-koodi

Xamarin asiakkaan projekti, jota olet ladannut, kun olet suorittanut [Xamarin Android-sovelluksen luominen] opetusohjelman jo sisältää koodia tukevat offline-synkronoinnin käyttäminen SQLite paikalliseen tietokantaan. Seuraavassa on lyhyt yleiskatsaus, mitkä tuotteet sisältyvät jo opetusohjelman c solmun. Katso Käsitteellinen yleiskatsaus ominaisuus [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

* Ennen kuin mikä tahansa taulukon toimintoja voi käyttää, paikallista alustaa. Paikallista tietokantaa alustaa kun `ToDoActivity.OnCreate()` suorittaa `ToDoActivity.InitLocalStoreAsync()`. Tämä menetelmä luo paikalliset SQLite-tietokanta-käyttäen `MobileServiceSQLiteStore` luokan Azure-mobiilisovellukset asiakkaan SDK myöntämä.

    `DefineTable` Menetelmä luo taulukon paikallisen säilössä annettu-tyypin kenttiä vastaavaa `ToDoItem` tällöin. Voit sisällyttää kaikki sarakkeet, jotka ovat Etätietokanta ei ole tyyppi. Voi tallentaa vain osan sarakkeet on.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see http://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }


* `toDoTable` Jäsen `ToDoActivity` on `IMobileServiceSyncTable` kirjoittamalla sen sijaan, että `IMobileServiceTable`. IMobileServiceSyncTable ohjaa kaikki luominen, lukea, päivittää ja poistaa (CRUD) taulukon toimintojen paikallista tietokantaa.

    Voit päättää, kun muutokset ovat miten Azure Mobile-sovelluksen taustaan haluat soittamalla `IMobileServiceSyncContext.PushAsync()`. Synkronoinnin yhteydessä auttaa säilyttää taulukon yhteyksien seuranta ja valitseminen muutokset kaikki taulukot asiakas-sovellus on muokannut milloin `PushAsync` kutsutaan.

    Annettu koodi-puhelut `ToDoActivity.SyncAsync()` synkronoitava aina, kun todoitem-luettelo päivitetään tai todoitem lisätään tai valmis. Koodi-Synkronoi paikallisen jokaisen muutoksen jälkeen.

    Annettu koodissa kaikki tietueet kaukosäätimen `TodoItem` taulukon pyydetään, mutta se on myös mahdollista tietueiden suodattaminen välittämällä kyselyn-tunnus ja kyselyn ja `PushAsync`. Lisätietoja on kohdassa *Vaiheittainen Synkronoi* [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Lisäresursseja

* [Offline-tietojen synkronointi Azure Mobile-sovellukset]
* [Azure mobiilisovellukset .NET SDK ohjeet][8]

<!-- URLs. -->
[Xamarin Android-sovelluksen luominen]: ../app-service-mobile-xamarin-android-get-started.md
[Offline-tietojen synkronointi Azure Mobile-sovellukset]: ../app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Android-sovelluksen luominen]: app-service-mobile-xamarin-android-get-started.md
[Offline-tietojen synkronointi Azure Mobile-sovellukset]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: http://xamarin.com/download
[Xamarin extension]: http://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
