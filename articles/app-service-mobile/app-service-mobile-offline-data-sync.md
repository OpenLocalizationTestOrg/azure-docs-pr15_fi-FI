<properties
    pageTitle="Offline-tietojen synkronointi Azure mobiilisovellukset | Microsoft Azure"
    description="Käsitteellinen viite- ja offline-tietojen synkronointi ominaisuutta Azure Mobile-sovellusten yleiskatsaus"
    documentationCenter="windows"
    authors="adrianhall"
    manager="dwrede"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="offline-data-sync-in-azure-mobile-apps"></a>Offline-tietojen synkronointi Azure Mobile-sovellukset

## <a name="what-is-offline-data-sync"></a>Mikä on offline-tietojen synkronointi?

Offline-tietojen synkronointi on asiakkaan ja palvelimen SDK-ominaisuuden Azure Mobile-sovellukset, jotka on helppo kehittäjät voivat luoda sovellukset, jotka toimivat ilman verkkoyhteyttä.

Kun sovellus on offline-tilassa, käyttäjät voivat luoda edelleen ja muokata tietoja, jotka tallennetaan paikallisesta liikkeestä. Kun sovellus on online-tilaan, se voidaan synkronoida paikalliset muutokset Azure Mobile-sovelluksen Taustajärjestelmä. Ominaisuus on myös tuki tunnistaminen ristiriitoja, kun samaa tietuetta muutetaan asiakkaan ja taustaan. Ristiriitojen voidaan käsitellä sitten joko palvelimessa tai asiakkaan.

Offline-synkronoinnin on hyötyä:

* Parantaa sovelluksen vastausajan palvelimen paikallisesti laitteen tietojen tallentaminen välimuistiin
* Luo tehokkaat sovellukset, jotka jäävät hyötyä, kun verkko-ongelmia
* Salli loppukäyttäjien voit luoda ja muokata tietoja, vaikka ei ole verkkokäyttö tukevat skenaariot vähän tai ei ole yhteyttä
* Tietojen synkronointi usean useilla eri laitteilla ja tunnistaa ristiriitoja, kun samaa tietuetta muokataan kahdella laitteella
* Rajoita viive tai käytön mukaan laskutettavan verkon käyttöön

Opetusohjelmassa Näytä offline-synkronoinnin lisääminen mobile asiakkaiden Azure Mobile-sovellusten käyttämisestä:

* [Android: Ota käyttöön offline-synkronointi]
* [iOS: offline-synkronoinnin ottaminen käyttöön]
* [Xamarin iOS: offline-synkronoinnin ottaminen käyttöön]
* [Xamarin Android: Ota käyttöön offline-synkronointi]
* [Xamarin.Forms: Ota käyttöön offline-synkronointi](app-service-mobile-xamarin-forms-get-started-offline-data.md)
* [Yleinen Windows-ympäristössä: Ota käyttöön offline-synkronointi]

## <a name="what-is-a-sync-table"></a>Synkronoi taulukon ominaisuudet

"/ Taulukot" päätepisteen käyttämään Azure Mobile-asiakasohjelma SDK: T antaa liittymät kuten `IMobileServiceTable` (.NET asiakkaan SDK) tai `MSTable` (iOS-asiakas). Nämä API yhdistäminen Azure Mobile-sovelluksen Taustajärjestelmä ja epäonnistuu, jos asiakas-laite ei ole yhteydessä.

Tukee offline-käyttöä, sovelluksen sen sijaan käytettävä *synkronointi taulukon* API, kuten `IMobileServiceSyncTable` (.NET asiakkaan SDK) tai `MSSyncTable` (iOS-asiakas). Kaikki samat CRUD-toimintoja (luominen, luku-, päivitys, poistaminen) toimi vastaan synkronointi taulukon API, paitsi nyt ne sisällytetään lukea tai kirjoittaa *paikallista*. Ennen kuin kaikki synkronointi taulukon toimintoja voi käyttää, paikallista alustaa.

## <a name="what-is-a-local-store"></a>Mikä on paikallinen säilö?

Paikallinen säilö on tietoja pysyvyyttä kerros asiakas-laitteessa. Azure-mobiilisovellukset asiakkaan SDK: T on oletusarvoinen paikallisen säilön-käyttöönotto. Windows-, Xamarin-ja Android-se perustuu SQLite; iOS-se perustuu Core tiedot.

Pystyt käyttämään SQLite perustuva käyttöönoton Windows Phone-tai Windows-kaupan 8.1, sinun täytyy asentaa SQLite-tunniste. Lisätietoja on artikkelissa [Yleinen Windows-ympäristössä: Ota käyttöön offline-synkronoinnin]. Android- ja iOS mukana SQLite version laite-käyttöjärjestelmässä, jotta ei tarvitse viitata version SQLite.

Kehittäjät myös ottaa käyttöön oman Paikallinen säilö. Jos haluat tallentaa tiedot-mobiilisovelluksen salattuja muodossa, voit määrittää Paikallinen säilö, joka käyttää SQLCipher salausta.

## <a name="what-is-a-sync-context"></a>Mikä on synkronoinnin yhteydessä?

*Synkronoinnin yhteydessä* on liitetty mobiilisovelluksen objektin (esimerkiksi `IMobileServiceClient` tai `MSClient`) ja seuraa synkronointi taulukoita tehdyt muutokset. Synkronoinnin yhteydessä ylläpitää *toiminnon jonon* joka säilyttää järjestetyn luettelon CUD toimintoja (luominen, päivittäminen, poistaminen), joka lähetetään myöhemmin palvelimeen.

Paikallinen säilö on liitetty käyttää alustaa menetelmää, kuten synkronoinnin yhteydessä `IMobileServicesSyncContext.InitializeAsync(localstore)` [.NET asiakkaan SDK].

## <a name="how-sync-works"></a>Miten offline-tilassa synkronointi toimii?

Synkronoi taulukoita käytettäessä asiakas-koodin ohjausobjektit, kun paikalliset muutokset synkronoidaan Azure Mobile-sovelluksen Taustajärjestelmä. Mitään lähetetään taustaan pistekokoa *push* paikalliset muutokset kutsu. Vastaavasti paikallista lisätään uusia tietoja vain, kun kyseessä on puhelun *salaus puretaan* .

* **Push**: Push toiminnon synkronoinnin yhteydessä ja lähettää kaikki CUD muutokset viimeisen push jälkeen. Huomaa, että ei voida lähettää vain yksittäisen taulukon muutokset, koska muuten toimintojen voitu lähettää väärässä järjestyksessä. Push suorittaa sarjan muiden kutsujen Azure Mobile-sovelluksen taustatietokannan, joka puolestaan Muokkaa server-tietokantaan.

* **Vedä**: salaus puretaan suoritetaan taulukon luovan välein ja kysely, joka noutaa palvelimen tietojen alijoukko voi mukauttaa. Azure Mobile client SDK: T lisääminen tuloksena olevat tiedot sitten paikallista.

* **Implisiittinen Vie**: erotettu on taulukko, jossa on paikallinen odottavia kohdistaa, jos erotettu suoritetaan ensin push-synkronoinnin yhteydessä. Näin voit pienentää ristiriitojen muutokset, jotka ovat jo jonossa ja uudet tiedot palvelimesta.

* **Vaiheittainen synkronointi**: salaus puretaan-toiminnon ensimmäinen parametri on *kyselyn nimeä* , jota käytetään vain työasemassa. Jos käytät muut kuin null-kyselyn nimi, Azure Mobile SDK suorittaa *vaiheittainen Synkronoi*.
  Aina, kun salaus puretaan toiminto palauttaa joukon tulokset viimeisimmän `updatedAt` SDK paikallinen järjestelmä taulukkoihin tallennetuista kyseisen tulosjoukon aikaleima. Myöhemmin erotettu toimintojen noutaa tietueet vain kyseisen aikaleima jälkeen.

  Vaiheittainen synkronoinnin käyttöön palvelimellesi on palautettava kuvaava `updatedAt` arvot ja tuettava myös tämän kentän mukaan lajittelu. Koska SDK Lisää oma Lajittele updatedAt-kentän, et voi kuitenkaan käyttää salaus puretaan kysely, jossa on oma `$orderBy$` lause.

  Kyselyn nimi voi olla mikä tahansa merkkijono, voit valita, mutta sen on oltava yksilöllinen kunkin sovelluksen looginen kyselylle.
  Muussa tapauksessa eri erotettu toiminnot voi korvata saman lisäävän synkronoinnin aikaleima ja kyselyt voivat palauttaa vääriä tuloksia.

  Jos kyselyssä on parametri, voit luoda yksilölliset kyselyn nimi on toteuttamaan parametriarvo.
  Esimerkiksi Jos suodatat käyttäjätunnus-kyselyn nimi voi olla seuraavasti (C#):

        await todoTable.PullAsync("todoItems" + userid,
            syncTable.Where(u => u.UserId == userid));

  Jos haluat valita ei vaiheittainen, välitä `null` kuin kyselyn tunnus. Tässä tapauksessa kaikki tietueet noudetaan jokaisen kutsun `PullAsync`, joka on mahdollisesti tehotonta.

* **Purging**: Voit poistaa sisältöä paikallista käyttämällä `IMobileServiceSyncTable.PurgeAsync`.
  Tämä voi olla tarpeen, jos sinulla on vanhentuneita tietoja asiakkaan tietokannan tai jos haluat hylätä kaikki odottavia muutoksia.

  Tyhjennä tyhjentää paikallisesta taulukon. Jos määritettynä on server-tietokannan kanssa synkronointia odottavia toimintoja, tyhjennä palauttaa poikkeuksen paitsi *voimassa Tyhjennä* -parametri on määritetty.

  Esimerkkinä vanhentuneita tietoja asiakkaan oletetaan, että "todo luettelon"-esimerkissä Device1 hakee vain kohteet, jotka ovat kesken. Tämän jälkeen todoitem "Ostaa maitoa" on merkitty suorittaa palvelimessa jonkin toisen laitteen. Kuitenkin Device1 edelleen on "Osta maito" todoitem paikallisen säilössä, koska se on vain tietojen kohteet, joita ei ole merkitty valmiiksi. Tyhjennä tyhjentää vanhentuneiden kohdetta.

## <a name="next-steps"></a>Seuraavat vaiheet

* [iOS: offline-synkronoinnin ottaminen käyttöön]
* [Xamarin iOS: offline-synkronoinnin ottaminen käyttöön]
* [Xamarin Android: Ota käyttöön offline-synkronointi]
* [Yleinen Windows-ympäristössä: Ota käyttöön offline-synkronointi]

<!-- Links -->
[.NET asiakkaan SDK]: app-service-mobile-dotnet-how-to-use-client-library.md
[Android: Ota käyttöön offline-synkronointi]: app-service-mobile-android-get-started-offline-data.md
[iOS: offline-synkronoinnin ottaminen käyttöön]: app-service-mobile-ios-get-started-offline-data.md
[Xamarin iOS: offline-synkronoinnin ottaminen käyttöön]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Xamarin Android: Ota käyttöön offline-synkronointi]: app-service-mobile-xamarin-ios-get-started-offline-data.md
[Yleinen Windows-ympäristössä: Ota käyttöön offline-synkronointi]: app-service-mobile-windows-store-dotnet-get-started-offline-data.md
