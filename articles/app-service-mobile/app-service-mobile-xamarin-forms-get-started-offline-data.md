<properties
    pageTitle="Ota offline-synkronoinnin, kun Azure Mobile-sovellus (Xamarin.Forms) | Microsoft Azure"
    description="Opettele käyttämään App palvelun Mobile-sovelluksen välimuisti ja synkronoi offline-tilassa tietojen Xamarin.Forms-sovelluksessa"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Xamarin.Forms-mobiilisovelluksen offline-synkronoinnin ottaminen käyttöön

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Yleiskatsaus
Tässä opetusohjelmassa esitellään Azure-mobiilisovellukset offline-synkronointi-ominaisuuden Xamarin.Forms varten. Offline-synkronoinnin avulla käyttäjät voivat käsitellä mobiilisovelluksessa--tarkasteleminen, lisääminen tai muokkaaminen tiedot – myös silloin, kun yhteyttä ei ole verkossa. Muutokset on tallennettu paikalliseen tietokantaan. Kun laite on online-tilaan, muutokset synkronoidaan remote-palveluun.

Tässä opetusohjelmassa perustuu Xamarin.Forms pikaopas-ratkaisun Mobile-sovellukset, jotka voit luoda, kun olet suorittanut opetusohjelman [Xamarin iOS-sovelluksen luominen]. Xamarin.Forms pikaopas ratkaisu on tukemaan offline-synkronoinnin, jossa on vain otettava käyttöön. Tässä opetusohjelmassa Päivitä pikaopas-ratkaisun käyttöön offline-tilassa Azure Mobile-sovellusten ominaisuuksia. Olemme Korosta myös offline-kohtaisia koodi-sovelluksessa. Jos et käytä ladatut pikaopas-ratkaisun, tiedot access-tunniste-paketti on lisättävä projektin. Saat lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen][1].

Lisätietoja offline-synkronointi-toiminto on aiheessa [Offline-tilassa Azure Mobile-sovelluksissa tietojen synkronointi][2].

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Pikaopas-ratkaisun toimintoja offline-synkronoinnin ottaminen käyttöön

Offline-synkronoinnin koodin sisältyy projektin C# esikäsittelydirektiivejä avulla. Kun **offline-TILASSA\_SYNKRONOI\_käytössä** symboli on määritetty, Luo sisältyvät koodin polut. Windows-sovellusten sinun on asennettava myös SQLite-ympäristössä.

1. Visual Studiossa, napsauta hiiren kakkospainikkeella ratkaisun > **Ratkaisu... NuGet pakettien hallinta**ja valitse Etsi ja asenna **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet pakkaaminen ratkaisun kaikissa projekteissa.

2. Napsauta ratkaisunhallinnassa TodoItemManager.cs-tiedoston avaaminen ja **kannettavat** projektin nimi, joka on kannettava luokkakirjasto projekti, valitse kommentointi seuraavat esikäsittelydirektiivi:

        #define OFFLINE_SYNC_ENABLED

4. (Valinnainen) Asenna jokin SQLite runtime-pakettien tukemaan Windows-laitteet:

    * **Windows 8.1 Runtimen:** Asenna [Windows 8.1 SQLite][3].
    * **Windows Phone 8.1:** Asenna [Windows Phone 8.1 SQLite][4].
    * **Yleinen Windows-ympäristössä** Asenna [Yleinen Windows-Universal SQLite][5].

    Vaikka pikaopas ei sisällä yleinen Windows projektin, yleinen Windows-ympäristön tuetaan Xamarin lomakkeilla.

5. (Valinnainen) Jokaisen projektin Windows-sovellus, napsauta hiiren kakkospainikkeella **viittaukset** > **Lisää viittaus...**, laajenna **Windows** -kansioon > **tunnisteet**.
    Ota käyttöön haluamasi **SQLite Windows** SDK sekä **Visual C++ 2013 Runtimen Windows** SDK.
    SQLite SDK nimet vaihtelevat hieman kussakin Windows-ympäristössä.

## <a name="review-the-client-sync-code"></a>Tarkista asiakkaan Synkronoi-koodi

Seuraavassa on lyhyt yleiskatsaus, mitkä tuotteet sisältyvät jo opetusohjelman koodin `#if OFFLINE_SYNC_ENABLED` direktiivejä. Offline-synkronointi-toiminto on kannettava luokkakirjasto projektin TodoItemManager.cs project-tiedostoon. Käsitteellinen yleiskatsaus ominaisuus on [Offline-tilassa tietojen synkronointi Azure-mobiilisovellukset][2].

* Ennen kuin mikä tahansa taulukon toimintoja voi käyttää, paikallista alustaa. Paikallista tietokantaa on alustaa **TodoItemManager** luokan konstruktorissa käyttämällä seuraava koodi:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Tämä koodi luo uusi paikallinen SQLite tietokanta **MobileServiceSQLiteStore** -luokan avulla.

    **DefineTable** -menetelmä luo taulukon paikalliseen säilöön, joka vastaa annettua haluamasi kentät.  Voit sisällyttää kaikki sarakkeet, jotka ovat Etätietokanta ei ole tyyppi. Voi tallentaa alijoukkoa sarakkeet on.

* **TodoItemManager** **todoTable** -kentässä on **IMobileServiceSyncTable** **IMobileServiceTable**sijaan. Tämä luokan käyttää paikallisen tietokannan kaikki luoda, lukea, päivittää ja poistaa (CRUD)-taulukkotoiminnot. Voit päättää, muutokset on valittuna Mobile-sovelluksen Taustajärjestelmä kutsumalla **PushAsync** **IMobileServiceSyncContext**. Synkronoinnin yhteydessä auttaa säilyttää taulukon yhteyksien seuranta ja valitseminen muutokset asiakas-sovellus on muokannut, kun **PushAsync** kutsutaan kaikki taulukot.

    **SyncAsync** -menetelmää kutsutaan synkronoitava Taustajärjestelmä Mobile-sovelluksen kanssa:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Tässä esimerkissä käytetään yksinkertainen Virheenkäsittely oletusarvon Synkronoinnin käsittelijä kanssa. Todellinen sovelluksen käsitellä eri virheitä, kuten verkon ja palvelimen ristiriidat käyttämällä mukautettu **IMobileServiceSyncHandler** toteutus.

## <a name="offline-sync-considerations"></a>Offline-synkronoinnin huomioon otettavia seikkoja

Otoksen **SyncAsync** -menetelmää kutsutaan vain pysäyttämisestä ja synkronoinnin pyydettäessä.  Aloita synkronointi Android- ja iOS-sovelluksessa, avattava kohteet-luettelossa. (Windows) Käytä **Synkronoi** -painiketta. Tosielämän-sovelluksessa voit myös tehdä synkronoinnin käynnistin verkon tilan muuttuessa.

Kun taulukko, jossa on odottaa on kohdistaa erotettu paikallisen päivitykset jäljittää tilanteen, jotka hakevat toiminnon automaattisesti käynnistimien edellisen konteksti-push. Kun päivittäminen, lisääminen ja olet suorittanut tämän esimerkin kohteita, voit jättää eksplisiittinen **PushAsync** kutsu.

Annettu koodi pyydetään remote TodoItem taulukon kaikki tietueet, mutta on myös mahdollista tietueiden suodattaminen siirtämällä kyselyn tunnuksen ja kyselyn **PushAsync**. Lisätietoja on kohdassa [Offline-tilassa tietojen synkronointi Azure-mobiilisovellukset] *Lisäävän synkronoinnin* [2].

## <a name="run-the-client-app"></a>Suorita asiakas-sovellus

Offline-synkronoinnin nyt käytössä Suorita asiakassovellus vähintään kerran kullekin ympäristössä täytä paikallista tietokantaa. Myöhemmin simuloida offline-ominaisuudet ja muokkaa tietoja paikallista sovelluksen ollessa offline-tilassa.

## <a name="update-the-sync-behavior-of-the-client-app"></a>Synkronoi toiminnan asiakas-sovelluksen päivittäminen

Tässä osassa muokata asiakas-projekti, jonka simuloida offline-skenaarion käyttämällä omaa Taustajärjestelmä Virheellinen sovelluksen URL-osoite. Vaihtoehtoisesti voit poistaa käytöstä verkkoyhteyksien siirtämällä laitteen "Tilaan lentokone."  Kun lisäät tai muutat tieto-osat, muutokset paikallista säilytetään, mutta ei ole synkronoitu Taustajärjestelmä tietojen säilöön, kunnes yhteys on muodostettu uudelleen.

1. Napsauta ratkaisunhallinnassa, Constants.cs projektitiedoston avaaminen **kannettavat** projektin ja muuttaa arvon `ApplicationURL` osoittamaan virheellinen URL-osoite:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";

2. TodoItemManager.cs-tiedoston avaaminen **kannettavat** projektin ja sitten lisätä **Yritä... todellisen** alueen **SyncAsync** **Todellinen** peruste **poikkeuksen** luokan. Tämän **todellisen** lohkon kirjoittaa konsolin poikkeuksen sanoma seuraavasti:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }

3. Muodosta ja suorita asiakas-sovellus.  Lisää uusi kohteita. Huomaa, että poikkeuksen on kirjautunut konsolin jokaisen yrityksen mobiililaitteesi kanssa taustaan. Nämä uudet kohteet ovat vain paikallista ennen kuin he voivat sijaita mobile Taustajärjestelmä. Asiakas-sovellus toimii aivan kuin se on kytketty taustatietokannan, tukevat kaikki luoda, lukea, päivitys, poistotoimintoja (CRUD)-toimintoja.

4. Sulje sovellus ja Käynnistä uudelleen ja varmista, että luomasi uudet kohteet ovat pysyviä paikallista.

5. (Valinnainen) Visual Studio avulla voit tarkastella Azure SQL-tietokanta-taulukosta, että taustatietokanta tietoja ei muutettu.

    Avaa Visual Studion **Palvelimen Explorer**. Siirry **Azure**-tietokannan->**SQL-tietokannat**. Tietokannan hiiren kakkospainikkeella ja valitse **Avaa SQL Server objektin Resurssienhallinnassa**. Nyt voit selata SQL-tietokannan taulukkoa ja sen sisältöä.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Päivitä uudelleen mobile Taustajärjestelmä asiakas-sovellus

Tässä osassa yhdistää sovelluksen mobile taustatietokannan, jossa varmistaminen sovelluksen palaa online-tilaan. Päivitä ele, kun tiedot on synkronoitu mobile Taustajärjestelmä.

1. Avaa Constants.cs uudelleen. Korjaa `applicationURL` siten, että oikea URL-osoite.

2. Muodosta uudelleen ja suorita asiakas-sovellus. Sovellus yrittää synkronoida kanssa mobiilisovelluksessa Taustajärjestelmä jälkeen avaamista. Varmista, että poikkeuksia kirjautunut virheenkorjaus-konsolissa.

3. (Valinnainen) Tarkastella ja päivittää tiedot, SQL Server Object Explorerissa tai REST-työkalua, kuten Fiddler tai [Postman][6]. Huomaa, että tiedot on synkronoitu taustatietokanta ja paikallista välillä.

    Ilmoitus tiedot on synkronoitu tietokannan ja paikallista välillä, ja se sisältää samalla, kun sovellus on katkennut lisätyt kohteet.

## <a name="additional-resources"></a>Lisäresursseja

* [Offline-tietojen synkronointi Azure Mobile-sovellukset][2]
* [Azure mobiilisovellukset .NET SDK ohjeet][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: http://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: http://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: http://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: http://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md