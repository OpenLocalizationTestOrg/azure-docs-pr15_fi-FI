<properties
    pageTitle="Azure ilmoituksen keskittimet"
    description="Opettele käyttämään push-ilmoitukset Azure-tietokannassa. MALLIKOODEJA kirjoitettu C# .NET-Ohjelmointirajapinnan käyttäminen."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure ilmoituksen keskittimet

##<a name="overview"></a>Yleiskatsaus

Azure ilmoituksen keskittimet on helposti käytettävällä, useita, skaalata ulos push-infrastruktuuri, jonka avulla voit lähettää Mobilen push-ilmoitukset minkä tahansa taustasta (-pilvi tai paikalliseen) tahansa mobile ympäristössä.

Ilmoitus keskittimet avulla voit helposti lähettää Office kaikissa ympäristöissä, mukautetut push-ilmoitukset abstracting tietoja eri ympäristö ilmoituksen järjestelmien (PNS). Voit kohdistaa yksittäisille käyttäjille tai koko yleisön osia sisältävä käyttäjien miljoonia yli kaikissa laitteissa, yhden API-puhelun.

Voit käyttää ilmoituksen keskittimet enterprise- ja kuluttaja skenaarioita. Esimerkki:

- Lähetä jakautumisen uutisia ilmoitukset miljoonia alhainen viive (ilmoitus keskittimet tehostaa Bing-sovellusten esiasennettuna kaikissa Windows- ja Windows Phone-laitteissa) kanssa.
- Lähetä sijaintiin perustuva kuponki käyttäjän osia.
- Lähetä käyttäjille tai ryhmille urheilu/talous ja visualisointi: n sovellusten tapahtumailmoituksia.
- Ilmoita käyttäjille yrityksen tapahtumia, kuten uusien viestien/sähköpostiviestit ja myyntimahdollisuuksia.
- Lähetä yhteen-aikainen-salasanojen monimenetelmäisen todentamisen vaaditaan.



##<a name="what-are-push-notifications"></a>Mitkä ovat Push-ilmoitukset?

Älypuhelimissa ja taulutietokoneissa voit "Ilmoita" käyttäjille on ilmennyt tapahtuma. Ilmoitukset voi kestää useita lomakkeita.

Windows-kaupan ja Windows Phone-sovelluksissa ilmoitus voi olla _ilmoitukseen_muodossa: irrallisia ikkuna tulee näkyviin, ja äänen, signaalin uusi ilmoitus. Muuntyyppisten ilmoitus, joita tuetaan sisältää _-ruutu_, _raaka_ja _merkin_ ilmoitukset. Saat lisätietoja ilmoitusten tuetut Windows-laitteissa tyypit [ruudut, merkkejä, ja ilmoituksia](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Apple iOS-laitteiden painallus ilmoittaa vastaavasti käyttäjälle pyytää käyttäjä voi tarkastella tai Sulje ilmoitus-valintaikkunassa. **Näkymän** valitseminen avaa sovellus, joka vastaanottaa viestiä. Saat lisätietoja iOS ilmoitukset [iOS ilmoitukset](http://go.microsoft.com/fwlink/?LinkId=615245).

Push-ilmoitusten avulla Näyttää ajan tasalla tietoja, vaikka energiajärjestelmien säästävän mobiililaitteet. Ilmoitukset voidaan lähettää Taustajärjestelmä järjestelmien mobiililaitteiden myös silloin, kun laitteessa vastaavan sovellukset eivät ole käytössä. Push-ilmoitukset ovat tärkeä osa kuluttaja-sovellusten, joissa niitä käytetään niin, että sovelluksen välitys ja käyttö. Ilmoitukset on myös hyötyä yrityksille, ajantasaiset tiedot kasvattaa työntekijän vastausajan business tapahtumia.

Esimerkkejä tietyn mobile välitys tilanteita, joissa on:

1.  Päivitetään Windows 8: ssa tai Windows Phone-ruudun nykyinen taloudellisia tietoja.
2.  Ilmoitat käyttäjä, jolla ilmoitukseen, jotkin työnimike on määritetty käyttäjälle, että työnkulku-pohjaista enterprise-sovelluksessa.
3.  Näyttää merkin, jossa nykyisen myynnin määrä ohjaa CRM-sovelluksessa (kuten Microsoft Dynamics CRM).

##<a name="how-push-notifications-work"></a>Miten Push-ilmoitukset työmäärä

Push-ilmoitukset toimitetaan käyttöjärjestelmäkohtaiset infrastruktuurin kutsutaan _Ympäristö ilmoituksen järjestelmien_ (PNS) kautta. PNS tarjoaa barebones Funktiot (eli ei tue lähetettäväksi mukauttaminen) ja olet yleisiä liittymää. Voit lähettää ilmoituksen Windows-kaupan sovellus, kehittäjä on esimerkiksi yhteyttä WNS (Windows-ilmoituspalvelu). Jos haluat lähettää ilmoituksen iOS-laitteessa, saman Kehitystyökalut on yhteyttä APN (Apple Push-ilmoituspalvelu) ja Lähetä viesti toisen kerran. Azure ilmoituksen keskittimet auttaa antamalla Yleiset-käyttöliittymä, sekä muita ominaisuuksia, jotka tukevat yli eri ympäristöissä push-ilmoitukset.

Korkean tason kaikki ympäristö ilmoituksen järjestelmät noudattamalla kuitenkin samoissa:

1.  Asiakkaan ottaa yhteyttä noutaa sen _käsitellä_PNS. Kahvan määräytyy järjestelmässä. WNS, se on URI-osoite tai "ilmoituksen kanavan." Se on APN-tunnus.
2.  Asiakas-sovellus tallentaa tätä kahvaa app _taustatietokantaan_ myöhempää käyttöä varten. WNS sitten taustatietokantatiedosto on yleensä pilvipalveluun. Apple-järjestelmän kutsutaan _Toimittaja_.
3.  Push ilmoituksen lähettäminen app taustatietokantaan yhteystietojen PNS kohteen tietyn asiakas-sovelluksen esiintymää kahvan avulla.
4.  PNS välittää ilmoituksen kahvan määritettyyn laitteeseen.

![][0]

##<a name="the-challenges-of-push-notifications"></a>Push-ilmoitukset haasteiden

Vaikka järjestelmät ovat hyvin tehokkaita, ne edelleen jätä jäljellä olevan työmäärän sovelluksen kehittäjä jotta Toteuta vaikka yleisiä push ilmoituksen tilanteita, joissa, kuten lähetystä tai push-ilmoitukset lähetetään Segmentoitu käyttäjät.

Push-ilmoitukset ovat yksi mobiilisovellukset pilvipalveluihin pyydetty eniten toimintoja. Syy siitä, että tarvitaan, jotta ne toimivat infrastruktuuri on varsin monimutkainen ja enimmäkseen toisiinsa liittymättömiä tärkeimmät liiketoimintalogiikan sovelluksen. Joitakin muodostaminen tarvittaessa push-infrastruktuurin haasteita ovat seuraavat:

- **Riippuvuus-ympäristössä.** Voidaksesi lähettää ilmoitukset laitteiden eri ympäristöissä, useita liittymät on koodattu-sitten taustatietokantatiedosto. Paitsi eroavat perustason tietoja, mutta (ruutu, ilmoitukseen tai merkin) ilmoitus, kun esitys on myös ympäristö riippuva. Nämä erot voivat johtaa monimutkaisia ja Kova ylläpitää taustatietokantaan koodi.

- **Asteikko.** Tämä infrastruktuurin skaalaus on kaksi ominaisuuksia:
    + PNS ohjeiden mukaan laitteen tunnusten on päivitettävä aina, kun sovellus käynnistetään. Tämä johtaa suuria määriä liikenne (ja seurauksena tietokannan sisäänkäyntien) vain, jos haluat säilyttää laitteen tunnusten ajan tasalla. Kun laitteiden määrä kasvaa (mahdollisesti, miljoonia), luominen ja ylläpito tämän infrastruktuurin kustannus on nonnegligible.

    + Useimmat PNSs eivät tue yleislähetystä useilla eri laitteilla. Seuraa lähetyksen laitteiden miljoonia johtaa puhelut PNSs miljoonia. Ei voi skaalata nämä pyynnöt on vähäpätöinen ongelma, koska yleensä sovellusten kehittäjille haluat säilyttää yhteensä viive. Esimerkiksi viimeisen laitteen sanoma tulee ei saa ilmoituksen 30 minuuttia sen jälkeen, kun olet lähettänyt ilmoituksia, usein kuin se päihität tarkoituksena on push-ilmoitukset.
- **Reititys.** PNSs lisäämistapaa viestin lähettäminen laitteeseen. Kuitenkin useimpien sovellusten ilmoitukset suunnattuja käyttäjien tai ryhmien (esimerkiksi kaikki työntekijöitä tiettyjä asiakastilin). Reitittää ilmoitukset oikea laitteet, jotta sovelluksen taustatietokannan on sellaisenaan ylläpidettävä rekisterin, joka yhdistää ryhmien laitteen tunnusten. Tämä katseltavan Lisää kokonaisaika sovelluksen market ja ylläpito kustannuksiin.

##<a name="why-use-notification-hubs"></a>Ilmoitus keskittimet käyttötarkoitus

Ilmoitus keskittimet poistamista monimutkaisuus: sinun ei tarvitse push-ilmoitukset haasteiden hallinta. Voit käyttää sen sijaan, ilmoitus-toiminnossa. Ilmoituksen keskittimet käyttämällä täydellistä useita, skaalata ulos push-ilmoituksen infrastruktuuri ja vähentää huomattavasti push-kohtaisia koodi, jota käytetään sovelluksen Taustajärjestelmä. Ilmoitus keskittimet Ota käyttöön push-infrastruktuuria kaikki toiminnot. Laitteet ovat vain vastuussa rekisteröiminen PNS kahvoista ja taustaan on vastuussa ympäristöstä riippumaton lähettää käyttäjille tai ryhmille korko seuraavassa kuvassa osoitetulla tavalla:

![][1]


Ilmoitus keskittimet on valmis avulla push-ilmoituksen infrastruktuurin kanssa seuraavat edut:

- **Useiden ympäristöjen.**
    +  Tuen pää mobile kaikissa ympäristöissä. Ilmoitus keskittimet lähettää push-ilmoitukset Windows-kaupan-, iOS-, Android- ja Windows Phone-sovelluksiin.

    +  Ilmoitus keskittimet tarjoaa yleisiä käyttöliittymän voi lähettää ilmoituksia kaikki tuetut käyttöympäristöt. Käyttöjärjestelmäkohtaiset protokollat eivät ole pakollisia. Sovelluksen taustatietokantaan voi lähettää ilmoituksia käyttöjärjestelmäkohtaiset tai ympäristöstä riippumaton muodossa. Sovelluksen yhteydessä vain ilmoituksen keskittimet.

    +  Kahvan hallinta. Ilmoitus keskittimet ylläpitää kahvaa rekisterin ja palautteen PNSs.

- **Minkä tahansa taustatietokantaan toimii**: Cloud tai paikalliseen, .NET, PHP, Java-solmu, jne.

- **Asteikko.** Ilmoitus keskittimet laitteiden ilman tarvitse arkkitehti uudelleen tai shard miljoonia Skaalaa.


- **RTF useat toimituksen kuvioita**:

    - *Lähetä*: sallii-samanaikaisesti lähetyksen miljoonia laitteet, joissa on yksittäinen API-kutsu.

    - *Unicast/moni*: Push yksittäisiä käyttäjiä, myös kaikki laitteensa, joka edustaa tunnisteet tai useita ryhmän; esimerkiksi erillisessä lomakkeessa tekijät (tablet ja puhelinnumero).

    - *Segmentointi*: Push-monimutkaisia segmenttiin määrittämiä tunnisteen lausekkeet (esimerkiksi New Yorkissa seuraavan Yankees-laitteet).

    Kukin laite, kun lähetät sen kahvaa ilmoitus-toiminnossa voit määrittää yhden tai useamman _tunnisteet_. Lisätietoja [tunnisteet]. Tunnisteiden ei ole valmiiksi valmistelun yhteydessä tai poistettu. Tunnisteiden lisäämistapaa yksinkertainen voi lähettää ilmoituksia käyttäjien tai ryhmien. Tunnisteita voi olla minkä tahansa sovelluksen kielikohtaiset tunniste (esimerkiksi käyttäjän tai ryhmän tunnukset), koska niiden käyttöä vapauttaa app taustatietokantaan-esittää tarvitse tallentaa ja hallita laitteen kahvoja.

- **Mukauttaminen**: kunkin laitteen voi olla vähintään yksi mallit-saavuttaa laitteen sovellus on lokalisoitu ja mukauttaminen vaikuttamatta taustatietokantaan koodi.

- **Suojaus**: jaetun Access salaisuus (SAS) tai liitetyt todennusta.

- **RTF-telemetriatietojen**: käytettävissä portaalissa ja ohjelmallisesti.


##<a name="integration-with-app-service-mobile-apps"></a>Sovelluksen palvelun Mobiilisovellusten integrointi

Helpottaa saumattoman ja verkkoympäristöjen käyttökokemuksen Azure palveluita yli [Palvelun mobiilisovellukset] on sisäinen ilmoitus keskittimet käyttämällä push-ilmoitusten tuki. [Palvelun mobiilisovellukset] on erittäin skaalattava, yleisesti saatavilla mobiilisovelluksen kehitysympäristö yrityksen kehittäjille ja järjestelmän muutetaan, joka yhdistää mobile kehittäjille on monenlaisia ominaisuuksia.

Mobile-sovellusten kehittäjille voit hyödyntää ilmoituksen keskittimet seuraavat työnkulussa:

1. Noutaa laitteen PNS kahva
2. Rekisteröi laite ja [malleja] ilmoituksen keskittimet helposti Mobile-sovellusten asiakkaan SDK Rekisteröi API-Liittymän kautta
    + Huomaa, että Mobile-sovellusten Sahalaita pois kaikki tunnisteet rekisteröinnissä tietoturvasyistä. Käsitellä ilmoituksen keskittimet taustasta yhteyttä suoraan, jos haluat liittää tunnisteet laitteiden kanssa.
3. Ilmoitukset lähetetään sovelluksen taustasta ilmoituksen keskittimet kanssa

Seuraavassa on joitakin conveniences toi tämän integrointi kehittäjät:

- **Sovellusten mobiilisovelluksen SDK: T.** Nämä usean platform SDK: T antaa yksinkertainen ohjelmointirajapinnan rekisteröinti ja jutella linkitetty-Mobiilisovelluksella automaattisesti ilmoitus-toiminnossa. Kehittäjät ei tarvitse ilmoituksen keskittimet tunnistetietojen avulla tarkastella ja käsitellä Lisää palvelu.
    + SDK: T tunnisteen automaattisesti tietyn laitetta, jonka mobiilisovellukset todennettu Käyttäjätunnus käyttöön push Käyttäjäskenaario.
    + SDK: T Määritä automaattisesti GUID Mobile-sovellukset asennuksen tunnus rekisteröitymään ilmoituksen keskittimet, tallentaminen kehittäjät säilyttää useita palvelun GUID ongelmia.
    
- **Asennus-malli.** Mobile-sovellusten toimii ilmoituksen keskittimet uusimman push mallin edustavan kaikki liittyvät laitteen JSON-asennuksen, Tasaa Push ilmoituksen Services ja on helppokäyttöinen push-ominaisuudet.

- **Joustavuutta.** Kehittäjät aina valita toimimaan ilmoituksen keskittimet suoraan vaikka integrointi on lisätty.

- **Integroidun käyttöliittymän [Azure]-portaalissa.** Push-ominaisuus esitetään visuaalisesti Mobile-sovellusten ja kehittäjät voivat käsitellä helposti liittyvän ilmoituksen keskittimeen Mobile-sovellusten kautta.



##<a name="next-steps"></a>Seuraavat vaiheet

Voit etsiä lisätietoja ilmoituksen keskittimet Näissä ohjeaiheissa:

+ **[Miten asiakkaat käyttävät ilmoituksen keskittimet]**

+ **[Ilmoitus keskittimet opetusohjelmat ja apuviivat]**

+ **Ilmoitus keskittimet aloittaminen opetusohjelmat** ([iOS]- [Android] [Windows Universal] [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Asianmukaisten .NET hallitun Ohjelmointirajapinnan viittaukset push-ilmoitukset ovat sijaitsee seuraavassa:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Miten asiakkaat käyttävät ilmoituksen keskittimet]: http://azure.microsoft.com/services/notification-hubs
  [Ilmoitus keskittimet opetusohjelmat ja apuviivat]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android-laitteeseen]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Windowsin Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Sovelluksen palvelun Mobiilisovellusten]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [mallit]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure portal]: https://portal.azure.com
  [tunnisteet]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
