<properties
    pageTitle="Ota käyttöön offline-synkronoinnin yleinen Windows Platform (UWP)-sovelluksen kanssa Mobile-sovellusten | Azure sovelluksen-palvelu"
    description="Opettele käyttämään offline-tilassa tietojen välimuistin ja synkronoi Azure Mobile-sovelluksen yleinen Windows Platform (UWP)-sovelluksessa."
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="enable-offline-sync-for-your-windows-app"></a>Windows-sovellus offline-synkronoinnin ottaminen käyttöön

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään, miten voit lisätä offline-tuki Azure Mobile-sovelluksen Taustajärjestelmä-yleinen Windows Platform (UWP)-sovelluksessa. Offline-synkronoinnin avulla käyttäjät voivat käsitellä mobiilisovelluksessa--tarkasteleminen, lisääminen tai muokkaaminen tiedot – myös silloin, kun yhteyttä ei ole verkossa. Muutokset on tallennettu paikalliseen tietokantaan. Kun laite on online-tilaan, muutokset synkronoidaan remote Taustajärjestelmä.

Tässä opetusohjelmassa Päivitä UWP sovelluksen projektin sovelluksen [Windows-sovelluksen luominen] tukemaan Azure-mobiilisovellukset offline-tilassa ominaisuuksia. Jos et käytä ladatut pikaopas palvelimen project-tietojen käytön tunniste-paketit on lisääminen projektiin. Katso lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Saat lisätietoja offline-synkronointi-toiminto on aiheessa [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

## <a name="requirements"></a>Vaatimukset

Tässä opetusohjelmassa on seuraavat vaatimukset:

* Visual Studio 2013, jossa on Windows 8.1 tai uudempia.
* [Luo Windows-sovellus][windows-sovelluksen luominen]suorittamiseen.
* [Azure Mobile SQLite säilön][sqlite store nuget]
* [Yleinen Windows-ympäristön kehittämisen SQLite](http://www.sqlite.org/downloads)

## <a name="update-the-client-app-to-support-offline-features"></a>Päivitä asiakas-sovellus tukee offline-ominaisuudet

Azure Mobile-sovelluksen offline-tilassa ominaisuuksien avulla voit käsitellä paikalliseen tietokantaan, kun olet offline-ominaisuudet. Nämä ominaisuudet-sovelluksen käyttöön alusta [SyncContext] [ synccontext] paikalliseen säilöön. Tämän jälkeen voit viitata taulukon [IMobileServiceSyncTable][IMobileServiceSyncTable] -liittymän kautta. SQLite käytetään Paikallinen säilö laitteeseen.

1. Asenna [SQLite runtime yleinen Windows-ympäristön](http://sqlite.org/2016/sqlite-uwp-3120200.vsix).

2. Visual Studiossa Avaa UWP sovelluksen projektin, joka [Luo Windows-sovellus] opetusohjelmassa valmis NuGet paketti-hallinta.
    Etsi ja asenna **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet-paketti.

4. Napsauta ratkaisunhallinnassa, napsauta hiiren kakkospainikkeella **viittaukset** > **Lisää viittaus...**  >  **Yleinen Windows** > **tunnisteet**, valitse Ota käyttöön **SQLite yleinen Windows-ympäristön** ja **Visual C++-2015 ympäristön yleinen Windows-ympäristön sovellusten**.

    ![Lisää SQLite UWP viite][1]

5. Avaa MainPage.xaml.cs-tiedosto ja kommentointi `#define OFFLINE_SYNC_ENABLED` määritys.

6. Visual Studiossa painamalla **F5** -näppäintä uudelleen ja suorita asiakas-sovellus. Sovellus toimii samalla samalta kuin ennen offline-synkronointi käytössä. Paikallinen tietokanta lisätään kuitenkin nyt tiedot, joita voi käyttää offline-ominaisuudet.

## <a name="update-sync"></a>Päivitä sovellus taustaan yhteyden katkaiseminen

Tässä osassa Katkaise yhteys Mobile-sovelluksen-taustatietokannan voit simuloida offline-tilassa tilanne. Kun olet lisännyt tiedot, poikkeuksen käsittelytoiminnon kertoo, että sovellus on offline-tilassa. Tässä tilassa uudet kohteet lisätään paikallista ja synkronoidaan mobiilisovelluksen taustatietokannan, kun push suoritetaan seuraavan yhdistetyn tilaan.

1. Muokkaa App.xaml.cs jaetun projektin. **MobileServiceClient** alustus, kommentoi ja lisää seuraava rivi, jossa on virheellinen mobiilisovelluksen URL-osoite:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Voit myös osoittaa offline-tilassa ongelman poistamalla wifi ja matkapuhelinverkon verkot laitteeseen tai lentokone tilan käyttäminen.

2. Painamalla **F5** voivat laatia ja suorita sovellus. Huomaa, että synkronointi ei onnistunut päivityksen, kun sovellus käynnistetään.

3. Lisää uudet kohteet ja huomaat, että push epäonnistuu [CancelledByNetworkError] tilojen aina, kun valitset **Tallenna**. Kuitenkin todo uudet kohteet ovat paikallista ennen kuin he voivat sijaita mobiilisovelluksen Taustajärjestelmä.  Tuotannon-sovelluksessa Jos poikkeukset estää asiakas-sovellus toimii niin kuin se on edelleen yhteydessä mobiilisovelluksen Taustajärjestelmä.

4. Sulje sovellus ja Käynnistä uudelleen ja varmista, että luomasi uudet kohteet ovat pysyviä paikallista.

5. (Valinnainen) Avaa Visual Studion **Palvelimen Explorer**. Siirry **Azure**-tietokannan->**SQL-tietokannat**. Tietokannan hiiren kakkospainikkeella ja valitse **Avaa SQL Server objektin Resurssienhallinnassa**. Nyt voit selata SQL-tietokannan taulukkoa ja sen sisältöä. Varmista, että taustatietokanta tietoja ei muutettu.

6. (Valinnainen) Kyselyn mobile taustatietokannan, HAE kyselyn käyttäminen lomakkeen REST-työkalua, kuten Fiddler tai Postman avulla `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`.

## <a name="update-online-app"></a>Päivitä sovellus uudelleen Mobile-sovelluksen taustaan

Tässä osassa muodostat mobiilisovelluksen Taustajärjestelmä sovelluksen. Nämä muutokset simuloida verkon uuden yhteyden sovellukseen.

Kun käynnistät sovelluksen ensimmäisen `OnNavigatedTo` tapahtuman käsittelijä puhelut `InitLocalStoreAsync`. Tämä menetelmä kutsuu `SyncAsync` synkronointi paikalliseen säilöön taustatietokanta kanssa. Sovellus yrittää synkronoida käynnistyksen yhteydessä.

1. Avaa App.xaml.cs jaetun projektin- ja että edellisen alustus kommentointi `MobileServiceClient` käyttämään oikeaa mobiilisovelluksen URL-Osoitetta.

2. Painamalla **F5** -näppäintä uudelleen ja suorita sovellus. Sovellus Synkronoi paikalliset muutokset ja käyttämällä push-ja salaus puretaan Azure Mobile-sovelluksen Taustajärjestelmä kun `OnNavigatedTo` tapahtumakäsittely suoritetaan.

3. (Valinnainen) Tarkastella ja päivittää tiedot tai SQL Server Object Explorerissa REST-työkalulla, kuten Fiddler avulla. Ilmoitus tiedot on synkronoitu Azure Mobile-sovelluksen taustatietokanta ja paikallista välillä.

4. Valitse sovellus, täyttävän ne paikallista muutaman kohteen vieressä oleva valintaruutu.

  `UpdateCheckedTodoItem`puhelut `SyncAsync` synkronointi kunkin valmis kohteeseen Taustajärjestelmä Mobile-sovelluksen kanssa. `SyncAsync`kutsuu push ja salaus puretaan. Kuitenkin **aina, kun suoritat erotettu taulukossa, jossa asiakkaan on tehnyt muutoksia, push aina suoritetaan automaattisesti**. Näin varmistat, kaikki taulukot yhteydet sekä paikallisen kaupan ovat yhdenmukaisia. Tämä ongelma voi aiheuttaa odottamattomia push.  Katso lisätietoja ongelman [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].


##<a name="api-summary"></a>API yhteenveto

Tukemaan mobile-palvelujen offline-tilassa ominaisuuksia on käytetty [IMobileServiceSyncTable] -liittymän ja alustaa [MobileServiceClient.SyncContext] [ synccontext] kanssa SQLite paikalliseen tietokantaan. Offline-tilassa, Normaali CRUD toimintoja mobiilisovellukset toimivat samalla tavalla kuin sovellus on edelleen yhteydessä, kun taas toimintojen toteutuvat paikallista vastaan. Synkronoi paikallista palvelimen kanssa voi käyttää seuraavilla tavoilla:

*  **[PushAsync]** Koska tätä menetelmää kuuluu [IMobileServicesSyncContext], muutokset kaikkien taulukoiden välillä siirretty taustaan. Tietueet, joiden paikalliset muutokset lähetetään palvelimeen.

* **[PullAsync] ** 
   erotettu aloitetaan [IMobileServiceSyncTable]. Kun taulukossa on jäljitetyt muutokset, varmista, että kaikki taulukot yhteydet sekä paikallisen kaupan ovat yhdenmukaisia suoritetaan implisiittinen push. *PushOtherTables* -parametri ohjaa muiden taulukoiden kontekstissa siirretty implisiittinen push. *Kyselyn* parametri on [IMobileServiceTableQuery<T> ] [ IMobileServiceTableQuery] 
   tai OData kyselymerkkijonon palautettujen tietojen suodattamiseen. Määritä lisäävän synkronoinnin käyttää *queryId* -parametria. Lisätietoja on artikkelissa  [Offline-tilassa tietojen synkronoiminen Azure Mobile-sovelluksissa](app-service-mobile-offline-data-sync.md#how-sync-works).

* **[PurgeAsync]** Sovelluksen tulee kutsua tällä menetelmällä voit tyhjentää vanhentuneita tietoja paikallisesta säännöllisin väliajoin. Käytä *pakottaa* -parametria, kun haluat poistaa kaikki muutokset, jotka ei ole vielä synkronoitu.

Saat lisätietoja käsitteistä [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa](app-service-mobile-offline-data-sync.md#how-sync-works).

## <a name="more-info"></a>Lisätietoja

Seuraavissa ohjeaiheissa on lisätietoja taustatietoja Mobile-sovellusten käyttäminen offline-synkronointi:

* [Offline-tietojen synkronointi Azure Mobile-sovellukset]
* [Azure mobiilisovellukset .NET SDK ohjeet][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Offline-tietojen synkronointi Azure Mobile-sovellukset]: app-service-mobile-offline-data-sync.md
[windows-sovelluksen luominen]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: http://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: http://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: http://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[IMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[IMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[CancelledByNetworkError]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
