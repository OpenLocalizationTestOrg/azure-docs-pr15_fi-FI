<properties 
    pageTitle="Azure ilmoituksen keskittimet - vianmäärityksen ohjeet" 
    description="Ohjeita siitä, miten Azure ilmoituksen keskittimet yleisten ongelmien vianmääritys." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Ilmoitus-keskittimet Azure - vianmäärityksen ohjeet

##<a name="overview"></a>Yleiskatsaus

Yksi eniten yleisiin kysymyksiin, jotka on kuulla Azure ilmoituksen keskittimet asiakkaiden on kuinka voit selvittää, miksi muut eivät näe sovelluksen niiden taustasta lähetetään ilmoitus näkyvät asiakas-laitteeseen - kohtaa, johon ja miksi ilmoitukset poistettiin ja miten voit korjata ongelman. Tässä artikkelissa käydään läpi eri syistä, miksi ilmoitukset Hae pudotettu tai lopu laitteisiin. Microsoft myös selata tavalla, jossa voit analysoida ja pääkansion syynä voi selvittää. 

Ensimmäisen kerran, on tärkeää ymmärtää, kuinka Azure ilmoituksen keskittimet Vie ilmoitusta laitteet.
![][0]

Tyypillinen Lähetä ilmoitus-työnkulussa viesti lähetetään **sovelluksen Taustajärjestelmä** , jota ei puolestaan joitakin käsittely kaikki ottaen huomioon määritetyn tunnisteet ja tunnisteen lausekkeiden määrittämään "kohteiden" eli kaikki merkintöjä, joita on push-ilmoituksen rekisteröinnissä **Azure ilmoituksen keskittimeen (NH)** . Näiden merkintöjen voi olla yli jonkin tai kaikki Microsoftin tuetut käyttöympäristöt - iOS, Google, Windowsin, Windows Phone Kindle ja Baidu Kiinan Android. Kun kohteet on muodostettu, NH sitten työntää ilmoitusta, jakaa useita erän merkintöjen laitteen Platformin tietyn **Push-ilmoituksen Service (PNS)** – kuten APNS Apple, Google jne GCM. NH todentaa tunnistetiedot, ilmoitus-toiminnossa määrittäminen-sivulla Azure perinteinen-portaalissa määrittäminen perustuu vastaaviin PNS kanssa. PNS välittää sitten vastaaviin **asiakaslaitteet**ilmoitukset. Tämä on käyttöympäristön suositeltava tapa pitää push-ilmoitukset ja tarkista, että lopullinen jalan ilmoituksen toimituksen tapahtuu ympäristö PNS ja laitteen välillä. Siten on neljä pääosaa - *asiakas*, *sovelluksen taustatietokannan*, *Azure ilmoituksen keskittimet (NH)* ja *Push-ilmoituksen Services (PNS)* ja jokin seuraavista voi aiheuttaa ilmoitusten saaminen kohteet poistetaan. Lisätietoja tämän arkkitehtuuri on käytettävissä [Ilmoituksen keskittimet yleiskatsaus].

Virhe aikana ilmoitukset voi ilmetä aikana alkuperäisen testi/väliaikaisen vaiheen, jotka voivat olla merkkinä siitä määritysongelmia tai se voi johtua seuraavista tuotannon missä joko kaikki tai osan ilmoitukset saattaa voi tuottaa poistetusta ilmaisee, että jotkin tarkempaa sovelluksen tai messaging kuvion ongelma. -Osassa alla on näyttää eri poistetusta ilmoitukset tilanteissa väliltä yleisiä harvoissa tyyppi, joista osa saattaa olla ilmeisimmät ja joitakin muita ei ole paljon-palvelussa. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure ilmoitukset-toiminnossa määritettäisi 

Azure ilmoituksen keskittimet on todennusta voivan lähettää onnistuneesti ilmoitukset vastaaviin PNS sovelluksen kehittäjän kontekstissa. Tämä on mahdollista kehittäjä Kehitystyökalut-tilin luominen vastaaviin ympäristön (Google, Apple, Windowsin jne.) kanssa ja rekisteröiminen niiden sovelluksen mistä tunnistetiedot, joilla on määritettävä ilmoitus keskittimet Tietolähdemääritykset-osasta-portaalissa. Jos ilmoituksia ei ovat kautta, ensimmäiseksi on oltava varmistaa, että ne vastaavat luotu ympäristö kehittäjä niiden tilissä sovelluksessa ilmoitus-toiminnossa on määritetty tarvittavat tunnistetiedot. Löydät Microsoftin [Käytön aloittaminen opetusohjelmat] hyödyllisiä Siirry päälle prosessia vaiheittainen tavalla. Seuraavassa on muutamia yleisiä virheellisistä määritykset:

1. **Yleiset**
 
    a) tehdä Varmista, että ilmoitus-toiminnossa nimi (ilman kirjoitusvirheet) on sama:

    - Missä ovat rekisteröiminen-asiakasohjelmassa 
    - Jos lähetät ilmoitukset taustasta  
    - Jos olet määrittänyt PNS tunnistetiedot ja 
    - Jonka olet määrittänyt asiakkaan ja taustaan SAS-tunnistetietoja. 
        
    b) Tee Varmista, että käytät oikeat SAS määritysten merkkijonot asiakkaan ja sovelluksen Taustajärjestelmä. Kuin säännön of napin sinun on käytettävä **DefaultListenSharedAccessSignature** asiakas-ja **DefaultFullSharedAccessSignature** sovelluksen taustassa (joka antaa oikeudet, voivat lähettää ilmoituksen NH)

2. **Apple Push-ilmoituksen Service (APN) määrittäminen**
 
    Sinulla on kaksi eri keskittimet - yksi tuotannon ja toiseen testikäyttöön tarkoitukseen. Tämä tarkoittaa ladataan aiot käyttää eristetyn erillisessä keskittimeen varmenne ja aiot käyttää erillisen keskittimeen tuotannon varmenne. Älä yritä ladata erilaisia todistuksia samaan keskittimeen, että se saattaa aiheuttaa ilmoituksen virheet rivin alaspäin. Jos tiedä paikassa, jossa erityyppisiä varmenteen vahingossa olet ladannut samaan keskittimeen, on suositeltavaa poistaa keskittimeen ja aloittaa ajan tasalla. Jos jostain syystä, et voi poistaa sitten on ainakin keskittimeen, sinun on poistettava kaikki olemassa olevat merkintöjen toiminnosta. 

3. **Google Cloud Messaging (GCM) määrittäminen** 

    a) tehdä Varmista, että haluat ottaa käyttöön "Google Cloud Messaging for Android-hakusanalla cloud projektin alle. 
    
    ![][2]
    
    b) Varmista että luominen "Server-näppäintä samalla, kun hankkiminen tunnistetiedot mitä NH käyttää GCM todentamismenetelmä. 
    
    ![][3]
    
    c) Tee Varmista, että olet määrittänyt "Tunnus" asiakas, joka on täysin numeerisia kokonaisuus, jonka voit saada koontinäytöstä:
    
    ![][1]

##<a name="application-issues"></a>Sovellusten ongelmia

1) **Tunnisteet / tunnisteen lausekkeet**

Jos olet määritetään yleisö tunnisteita tai tunnisteen lausekkeiden avulla, ei aina, kun haluat lähettää ilmoituksen, kohdetta ei ole löydy tunnisteet/tunnisteen lausekkeita, Lähetä kutsu määrität perusteella. Kannattaa tarkistaa oman merkintöjen varmistaa, että ovat tunnisteita, jotka vastaavat kun Lähetä ilmoitus ja tarkista sitten ilmoituksen vastaanoton vain-asiakkaiden kanssa näiden merkintöjen. Esimerkiksi Jos haluat lähettää ilmoituksen tunniste "Urheilu" kaikki oman merkintöjen NH kanssa on tehty sano "Tutkimuspolitiikasta" tunnistetta, se lähetetään ei kaikissa laitteissa. Monimutkainen tapaus voi aiheuttaa tunnisteen lausekkeiden missä vain rekisteröidyt "Tunnisteen A" tai "Tunniste B", mutta lähetettäessä ilmoituksissa, kohdennat "Tunnisteen A & & tunnisteen B". Vihjeitä diagnosointi tahdissa alla olevasta osiosta ovat tavalla, jossa haluat tarkistaa, että merkintöjen sekä heillä on tunnisteet. 

2) **Mallin ongelmat**

Jos käytät malleja varmistaa, että seuraavat ohjeet on kuvattu osoitteessa [mallin ohjeet]. 

3) **Virheellinen merkintöjen**

Jos ilmoitus-toiminnossa on määritetty oikein ja tunnisteita tai tunnisteen kaikki lausekkeet on käytetty oikein tuloksena on kelvollinen, johon ilmoitukset lähetetään on kohteiden etsiminen, NH käynnistymiseen useita käsittely erissä rinnakkain - kunkin erän viestien lähettäminen joukko merkintöjen poistaminen käytöstä. 

> [AZURE.NOTE] Koska rinnakkain käsittely tapahtuu, emme eivät ole taata järjestyksessä, jossa ilmoitukset toimitetaan. 

Nyt Azure ilmoitukset-toiminnossa on tarkoitettu "useimmat-palvelussa kerran" viestin toimitus-malli. Tämä tarkoittaa, että olemme yrittää jään monistaminen niin, että ei ilmoitukset toimitetaan useammin kuin kerran laitteeseen. Jotta tämä on selata merkintöjä ja varmista, että vain yhden viestin per laitteen tunnus ennen kuin lähetät viestin todella PNS. Kun kunkin erän lähetetään PNS, joka puolestaan hyväksyminen ja vahvistaminen merkintöjä, on mahdollista PNS havaitsee on virhe vähintään yksi seuraavista merkintöjen osalta, palauttaa virheen Azure NH ja lopettaa käsittelyn pudottaminen siten, että erä kokonaan. Tämä on varsinkin APN, joka käyttää TCP-muodossa-protokollan kanssa. Vaikka emme on optimoitu useimpien, kun toimitus-tässä tapauksessa Läsnäolevan rekisteröinnin poistaminen Microsoftin tietokannan ja yritä ilmoituksen toimituksen muiden laitteiden kyseisen osalta.

Saat vastaan käyttämällä Azure ilmoituksen keskittimet REST API rekisteröinti epäonnistui toimituksen yritys virhetietojen: [kohti viestin Telemetriatietojen: Saat ilmoituksen viestin Telemetriatietojen](https://msdn.microsoft.com/library/azure/mt608135.aspx) ja [PNS palaute](https://msdn.microsoft.com/library/azure/mt705560.aspx). Katso [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) esimerkiksi koodi.

##<a name="pns-issues"></a>PNS ongelmat

Kun Ilmoitusviesti on vastaanottanut vastaaviin PNS on sen vastuu aikana ilmoituksen laitteeseen. Azure ilmoituksen keskittimet on lisättävä kuva ja on ohjausobjektia tai jos ilmoituksen siirtymällä toimitetaan laitteeseen. Koska ympäristö ilmoitus-palvelut on melko tehokkaat, yleensä saavuttaa laitteen muutaman sekunnin kuluttua PNS-ilmoitukset. Jos PNS kuitenkin-asetuksen on rajoitin Azure ilmoituksen keskittimet painikkeeseen eksponentiaalisen takaisin, strategia käytöstä ja jos PNS pysyy saavuttamattomissa 30 minuuttia sitten on käytännön, joka päättyy ja pudota viestit pysyvästi. 

Jos PNS yrittää toimittaa ilmoituksen, mutta laite on offline-tilassa, ilmoitus PNS tallentamat rajoitetun ajanjakson ajan ja toimitettu laitteeseen, kun se on käytettävissä. Vain yksi kunkin sovelluksen viimeisimmät ilmoitus on tallennettu. Jos useita ilmoitukset lähetetään, kun laite on offline-tilassa, kunkin uuden ilmoituksen aiheuttaa ennakolta haluat hylätä. Toiminta on säilyttämällä uusimmat ilmoituksen kutsutaan coalescing APN-ilmoitukset ja tiivistäminen GCM (joka käyttää collapsing näppäintä). Jos laite on offline-tilassa pitkään, ilmoituksia, jotka tallennettiin se ohitetaan. Lähde - [APN ohjeet] & [GCM ohjeet]

Azure ilmoituksen keskittimet - kanssa voit välittää coalescing näppäintä kautta käyttämällä yleinen HTTP-otsikon `SendNotification` API (esimerkiksi .NET SDK – `SendNotificationAsync`) joka kestää HTTP-otsikot, jotka siirretään kuin on vastaaviin PNS. 

##<a name="self-diagnose-tips"></a>Diagnosointi tahdissa vihjeitä

Seuraavassa on tutkii eri muutoksenhakukeinoihin ja pääkansio aiheuttaa ilmoituksen keskittimeen ongelmat:

###<a name="verify-credentials"></a>Tarkista tunnistetiedot

1. **PNS developer-portaalissa**

    Vahvistavat niitä vastaaviin PNS developer portaalin (APN-GCM, WNS jne) käyttämällä Microsoftin [Käytön aloittaminen opetusohjelmat].

2. **Azure Classic-portaalissa**

    Siirry tarkistetaan sekä käytetään vastaa tunnistetietojen verrattuna kaavaan PNS developer-portaalissa saatu määrittäminen-välilehti. 

    ![][4]

###<a name="verify-registrations"></a>Tarkista merkintöjä

1. **Visual Studio**

    Jos käytät Visual Studio kehittämiseen voit yhteyden muodostaminen Microsoft Azure ja tarkastella ja hallita useita Azure palveluista, kuten "Server Explorerista" ilmoitukset-toiminnossa. Tämä on hyödyllinen lähinnä keskihajonta-/ testiympäristössä. 

    ![][9]

    Voit tarkastella ja hallita kaikki oman keskittimeen merkintöjen joka luokitellaan hyvin alustan, alkuperäiset tai mallin rekisteröinnin, tunnisteita, PNS tunnus, tunnuksella ja vanhentumispäivä. Voit myös muokata suoraan selaimessa - joka on hyödyllinen, vaikkapa, jos haluat muokata mahdolliset tunnisteet rekisteröintiä. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio toimintoja Muokkaa merkintöjen vain käytettyä keskihajonta/testi merkintöjen rajoitettu määrä. Jos syntyy on vahvistettava oman merkintöjen kerralla, harkitse Vie/tuo rekisteröinti-toimintoja käyttämällä kuvatut - [Vie/tuo merkintöjä] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Palvelun Bus explorer**

    Monet asiakkaat käyttää ServiceBus explorer - Tässä kuvattuja [ServiceBus Explorer] tarkasteleminen ja hallinta niiden ilmoitus-toiminnossa. Avaa lähde-projekti, jossa on käytettävissä code.microsoft.com - [ServiceBus Explorer-koodi] on

###<a name="verify-message-notifications"></a>Tarkista ilmoituksia

1. **Azure perinteinen Portal**

    Voit siirtyä testi ilmoitusten lähettämisen asiakkaiden ilman hänen nimeään tarvitse mitään palvelun Taustajärjestelmä ylöspäin ja käynnissä "Virheenkorjaus"-välilehteen. 

    ![][7]

2. **Visual Studio**

    Voit myös lähettää testi ilmoitukset-Visual Studio comforts:

    ![][10]

    Voit lukea lisää tässä - Visual Studio ilmoituksen keskittimeen Azure-explorer toimintoja 
    
    - [JA Server Explorer yleiskatsaus]
    - [JA palvelimen Explorer blogimerkintä - 1]
    - [JA palvelimen Explorer blogimerkintä - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Virheenkorjaus epäonnistui ilmoitukset / Tarkista ilmoituksen tulos

**EnableTestSend-ominaisuus**

Ilmoituksen ilmoituksen keskittimet kautta lähetettäviin alun perin se vain saa jonossa NH tekemään käsittelyn selvittää, kaikki kohteet, ja sitten tekstin NH lähettää sen PNS. Tämä tarkoittaa, että kun käytät REST API tai mitä tahansa asiakkaan SDK, Lähetä kutsu onnistuu palauttaa vain tarkoittaa, että viesti on on onnistuneesti jonossa ilmoituksen keskittimeen kanssa. Se ei tarjoa mitä on tapahtunut kun NH myöhemmin jonkin viestin lähettäminen PNS tietoja. Jos asiakas-laite ei saapua ilmoitus, on mahdollinen NH ei voi toimittaa viestiä PNS, esimerkiksi tietojen kooksi ylittänyt suurimman sallitun mukaan PNS virhe tai NH määritetyt tunnistetiedot ovat virheellisiä jne. Pääset PNS virheet tietoja on ovat ottaneet [EnableTestSend ominaisuus]-ominaisuuden käyttöön. Tämä ominaisuus on automaattisesti käytössä, kun lähetät sanomia portal- tai Visual Studio asiakasohjelmaa ja näin ollen mahdollistaa Saat lisätietoja muistin. Voit käyttää tätä ottaen .NET SDK, johon se on käytettävissä nyt Esimerkki ohjelmointirajapinnan kautta ja lisätään myöhemmin kaikki asiakkaan SDK: T. Jos haluat käyttää tätä REST-puhelun, vain liittäminen querystring parametrin nimeltä "testaaminen" Lähetä kutsu lopussa esimerkiksi 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Esimerkki (.NET SDK)*
 
Oletetaan, että käytössäsi on .NET SDK lähettää alkuperäisen ilmoitusruudun:

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`näkyy ainoastaan tilan `Enqueued` ilman mitään tietoja mitä on tapahtunut oman push suorittamisen lopussa. Nyt voit käyttää `EnableTestSend` ominaisuuden valmisteltaessa `NotificationHubClient` ja saat yksityiskohtaiset tilan lähetettäessä ilmoituksen PNS virheet. Lähetä kutsu tähän kestää ylimääräisen kerran, sillä se palauttaa vain sen jälkeen, kun NH on toimitettu ilmoituksessa, joka määrittää tuloksen PNS palauttaa. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Esimerkki tulostus*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
Tämä sanoma ilmaisee joko virheellinen tunnistetiedot on määritetty ilmoitus-toiminnossa tai valitsemalla Valitse merkintöjen ongelmasta ja suositellut kurssin olisi rekisteröinnin ja anna asiakkaan sen uudelleen ennen viestin lähettämistä. 
 
> [AZURE.NOTE] Huomaa, että tämä ominaisuus on käytettävä raskaasti rajoittanut niin käytössä on oltava vain tämä keskihajonta/testiympäristössä kanssa rajattu merkintöjen määrä. Lähetämme virheenkorjaus ilmoituksia vain 10-laitteet. On myös käsittelyn virheenkorjaus lähettää olevan 10 minuutissa rajoitukset. 

###<a name="review-telemetry"></a>Tarkista telemetriatietojen 

1. **Perinteinen Azure-portaalin käyttäminen**

    Portaalin avulla voit saada kaikki tehtävän pikaisesti ilmoitus-toiminnossa. 
    
    a) Valitse "dashboard-välilehdessä voit tarkastella koostettu näkymä merkintöjä, ilmoitusten sekä virheiden ympäristö. 
    
    ![][5]
    
    b) voit lisätä myös monia muita ympäristö tiettyjä tietoja "Näytön"-välilehdestä Tutustu tarkempaa erityisen palauttaa NH yrittää lähettää ilmoituksen PNS PNS tietyn virheitä. 
    
    ![][6]
    
    (c) voit olisi aloittaminen **Saapuviin viesteihin**- **Rekisteröinnin toimintojen** **Onnistuneen ilmoitukset** ja siirry sitten yhdessä platform-välilehdessä voit tarkastella PNS määrätyistä virheistä. 
    
    d) Jos sinulla on määritetty virheellisesti todennusasetukset kanssa, valitse PNS todennus-virhe tulee näkyviin ilmoitus-toiminnossa. Tämä on hyvä maininta voit tarkistaa PNS tunnistetiedot. 

2) **Ohjelmallisesti**

Lisätietoja tästä- 

- [Ohjelmallisesti Telemetriatietojen käyttö]
- [Telemetriatietojen Access, API-Esimerkki] 

> [AZURE.NOTE] Useita telemetriatietojen liittyvät ominaisuudet **Vie/tuo merkintöjä**, kuten **Telemetriatietojen Access, ohjelmointirajapinnan** jne ovat käytettävissä vain vakio taso. Jos yrität käyttää näitä ominaisuuksia, jos olet vapaa tai Basic taso sitten saat Poikkeusviesti tätä käytettäessä SDK ja HTTP-403 (kielletty), kun käytät ne suoraan REST API. Varmista, että olet siirtänyt ylöspäin Standard taso Azure perinteinen Portal kautta.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Ilmoitus keskittimet yleiskatsaus]: notification-hubs-push-notification-overview.md
[Käytön aloittamisen opetusohjelmat]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Mallin ohjeet]: https://msdn.microsoft.com/library/dn530748.aspx 
[APN-ohjeet]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM ohjeet]: http://developer.android.com/google/gcm/adv.html
[Vie/tuo merkintöjä]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Explorer-koodi]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[JA Server Explorer yleiskatsaus]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[JA palvelimen Explorer blogimerkintä - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[JA palvelimen Explorer blogimerkintä - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[EnableTestSend-ominaisuus]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Ohjelmallisesti Telemetriatietojen käyttö]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemetriatietojen Access, API-Esimerkki]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 