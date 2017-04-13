<properties
    pageTitle="Azure ilmoituksen keskittimet – usein kysyttyjä kysymyksiä"
    description="Usein kysyttyjä kysymyksiä-ilmoituksen keskittimet ratkaisujen suunnitteleminen ja toteuttaminen"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="Push-ilmoituksen, push-ilmoitukset, iOS push-ilmoitukset, android push-ilmoitukset, ios push, android push"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Push-ilmoitukset ja Azure ilmoituksen keskittimet – usein kysytyt kysymykset

##<a name="general"></a>Yleiset
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. mikä on ilmoitus keskittimien Hintamalli?
Ilmoitus keskittimet tarjotaan kolme tasoa:

* **Vapaa** - Hae enintään 1 miljoonaa työntää tilauskohtaisten kuussa.
* **Perus** - get 10 miljoonaa työntää tilauskohtaisten kuussa, kun perusaikataulu-kiintiön kasvu-vaihtoehtojen avulla.
* **Vakio** - Hae 10 miljoonaa työntää tilauskohtaisten kuussa, kun perusaikataulu, jossa kiintiön Suurenna asetukset sekä monipuolisia telemetriatietojen capabilties.

Saat viimeisimmät tiedot löytyy [Ilmoituksen keskittimet hinnoittelu] -sivulla. Hinnoittelua muodostetaan tilauksen tasolla ja perustuu push ilmoituksen tutkimus määrä, joten se ei ole väliä montako nimitilan tai ilmoitus keskittimet olet luonut Azure-tilaukseesi.

**Vapaa** taso tarjotaan kehittäminen tarkoitukseen SLA ei takaa kanssa. Vaikka tämä taso voi olla hyvä pohjana niille, jotka haluat tarkastella push-ilmoitukset – Azure ilmoituksen keskittimet ominaisuuksia, eivät välttämättä ole, paras vaihtoehto Normaali, suuri asteikko-sovellukset.

**Tavallinen** & **Vakio** tasoa tarjotaan tuotannon käyttötapa seuraavia ominaisuuksia ja otettu käyttöön *vain standardin varten taso*:

- *RTF-telemetriatietojen* - ilmoituksen keskittimet tarjous on useita ominaisuuksia, jotka telemetriatietojen tietojen vieminen sekä push-ilmoituksen rekisteröintitietoja offline-tilassa ja analysointia varten.
- *Usean vuokraajan* - ihanteellinen vaihtoehto, jos luot useita alihallinnat, jotka tukevat ilmoituksen keskittimet avulla mobiilisovelluksessa. Tämän avulla voit määrittää Push-ilmoituksen Services (PNS) tunnistetiedot ilmoituksen keskittimeen nimitilan tasolla sovellus ja sitten voit eroteltava alihallinnat, jotka tarjoavat ne yksittäisiä keskittimet tämä yleisiä nimitila-kohdassa. Näin voit helpottaa ylläpitoa samalla, kun pitäminen SAS-näppäimet, jotta voit lähettää ja vastaanottaa ilmoituksen keskittimet push-ilmoitukset eristetyt kunkin vuokraajan varmistaa, että muut rajat-vuokraajan limitys.
- *Ajoitettu Push* - voit ajoittaa push-ilmoitukset, josta tulee jonossa ja lähettää myöhemmin.
- *Bulk tuominen* - toiminnon avulla voit tuoda merkintöjen kerralla.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. mikä on ilmoitus keskittimet SLA?
**Perus** - ja **Standard** Notification keskittimet tasoa takaa, että vähintään 99,9 % ajan, oikein määritetyn sovellukset voivat lähettää push-ilmoitukset tai suorittaa rekisteröinnin hallintatoiminnot, jotka koskevat ilmoituksen keskittimeen käyttöön sisällä tuetut taso. Lisätietoja Microsoftin SLA käy [Ilmoituksen keskittimet SLA] -sivulla.

> [AZURE.NOTE] Mikään ei ole SLA jalan Platform-ilmoituspalvelu ja laitteen välillä, koska ilmoituksen keskittimet määräytyvät ulkoisen ympäristö tarjoajia push-ilmoituksen laitteeseen.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. asiakkaat käyttävät ilmoituksen keskittimet?
Asiakkaiden ilmoituksen keskittimet käyttäminen muutaman huomattavat tiedoston alla suuri määrä on:

* Sochi 2014 – 100s ryhmien, 3 + miljoonaa laitteita, 150 + miljoonaa ilmoitus lähetetään kahden viikon aikana. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Seattle kertoja – [CaseStudy - Seattle kertaa]
* Mural.LY - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Bing Apps – laitteita, miljoonia 10s lähettäminen 3 miljoonaa ilmoitukset/päivä.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. kuinka päivittää tai alentaa Omat ilmoituksen keskittimet muuttamaan service-taso?
Siirry [Azure perinteinen Portal], valitse palvelu Bus ja valitse sitten, että nimitilan ilmoitus-toiminnossa. Valitse mittakaava-välilehti osaat muuttaminen ilmoituksen keskittimet-palvelutaso.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Rakenne- ja kehitystyö
###<a name="1---which-server-side-platforms-do-you-support"></a>1. palvelinpuolen missä ympäristöissä tukee?
Annamme SDK: T ja [Valmis esimerkkejä] .NET, Java, PHP, Python, Node.js siten, että sovelluksen Taustajärjestelmä on asennuksen vaihtamaan ilmoituksen porttiin näissä ympäristöissä avulla. Ilmoitus keskittimet ohjelmointirajapinnan perustuvat muiden liittymät, niin voit suoraan jutella ne sen sijaan, jos et halua lisätä ylimääräisen riippuvuuden. Lisätietoja löytyy [NH - REST API] -sivulla.

###<a name="2---which-client-platforms-do-you-support"></a>2. mitä asiakasympäristöjen tukee?
Sovellus tukee lähettämisen push-ilmoitukset [Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Yleinen Windowsin](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone-](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Kiinan (joko Baidu)](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin ([iOS](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Android](xamarin-notification-hubs-push-notifications-android-gcm.md))- [Sovellusten Chrome](notification-hubs-chrome-push-notifications-get-started.md) ja [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) -käyttöjärjestelmien työpöytäsovelluksiin. Täydellinen luettelo käytön aloittaminen opetusohjelmat tasa lähettämisen push-ilmoitukset näissä ympäristöissä Tutustu [NH - käytön aloittaminen opetusohjelmat] ‑sivustossa.

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. eivät tue SMS/sähköpostin tai web-ilmoitukset?
Ilmoitus keskittimet on suunniteltu ensisijaisesti voi lähettää ilmoituksia käyttämällä yllä ympäristöjen mobiilisovellukset. Ei ole vielä annamme voidaan lähettää sähköpostin tai Tekstiviesti-ilmoitukset kuitenkin kolmannen osapuolen ympäristöissä, jotka tarjoavat seuraavia ominaisuuksia voi integroida sekä ilmoituksen keskittimet alkuperäisen push-ilmoitukset lähetetään [Azure Mobile-sovellusten]avulla.

Ilmoitus keskittimet ei anna selaimen-push ilmoituksen toimituksen palvelun ulos,-valmiilla. Asiakkaat voivat valita toteuttamisesta tämä SignalR käyttämällä palvelinpuolen tuetut käyttöympäristöt päälle. Jos etsit voi lähettää ilmoituksia Chrome-eristetyn sovelluksia selaimessa, tutustu [Chrome sovellusten opetusohjelma].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. mikä on Azure Mobile-sovellusten ja Azure ilmoituksen keskittimet välisen suhteen, ja kun mitä käytetään?
Jos sinulla on aiemmin mobiilisovelluksen-taustatietokannan ja haluat lisätä lähettämiseen push-ilmoitukset voit käyttää Azure ilmoituksen keskittimet. Jos haluat määrittää mobiilisovelluksen-taustatietokannan alusta alkaen sitten kannattaa Azure Mobile-sovellusten käyttämisestä. Azure Mobile-sovelluksen valmistelee automaattisesti ilmoitus-toiminnossa voit voi lähettää helposti mobiilisovelluksen taustasta push-ilmoitukset. Azure-mobiilisovellukset hinnoittelu sisältää perus ovat ilmoitus-toiminnosta ja maksat vain siirtyessäsi mukana työntää lisäksi. Lisätietoja kustannukset ovat käytettävissä [Sovelluksen palvelun hinnat] -sivulla.

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. kuinka monta laitteet voin tukevat Jos lähetän ilmoituksen keskittimet kautta push-ilmoitukset?
Lue lisätietoja Tuetut laitteet määrän [Ilmoituksen keskittimet hinnoittelu] -sivulle.

Tiettyjen käyttötavoista, jos tarvitset tukea on enemmän kuin 10,000,000 rekisteröity laite, ota [yhteyttä](https://azure.microsoft.com/overview/contact-us/) suoraan ja Microsoft avulla voit skaalata ratkaisu.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. kuinka monta push-ilmoitukset voi lähettää?
Valitun tason mukaan Azure automaattisesti skaalautuvat ilmoitukset juoksutus järjestelmän kautta määrän mukaan.

>[AZURE.NOTE] Yleistä käyttökustannukset siirtyä perustuvat push-ilmoitukset parhaillaan served määrän. Varmista, että voidaan ottaa huomioon aiemmin taso rajoitukset jäsennetty [Ilmoituksen keskittimet hinnoittelu] -sivulla.

Olemassa olevan asiakkaidensa käyttävät ilmoituksen keskittimet miljoonia push-ilmoitukset lähetetään päivittäin. Sinun ei tarvitse tehdä mitään erityistä skaalata oman push ilmoitukset saavuttaa, kun käytät Azure ilmoituksen keskittimet.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. kuinka paljon kuluu aikaa, lähetetty push-ilmoitukset saavuttamiseksi laitteeseeni?
Azure ilmoituksen keskittimet ei voi käsitellä vähintään **1 miljoona push-ilmoituksen lähettää minuuteiksi** tavallinen käyttää skenaario, jossa Saapuvan kuormituksen on melko yhdenmukaisia ja ei ole spikey laatu. Tämä kurssi saattavat vaihdella sen mukaan montako tunnisteita, saapuvat lähettää laatu ja muut Ulkoiset tekijät.

Arvioitu toimitusaikaan aikana palvelu ei voi laskea kohti ympäristön ja reitin viestien rekisteröity tunnisteet/tunniste-lausekkeisiin perustuvien vastaaviin push ilmoituksen toimituksen palveluiden kohteet. Tässä näkymässä on vastuussa Push-ilmoitukset-palvelujen (PNS) ilmoituksen lähettäminen laitteeseen.

PNS takaa minkä tahansa SLA välittää ilmoituksista. kuitenkin yleensä suuria useimpia push-ilmoitukset toimitetaan laitteiden (yleensä 10 minuutin rajoissa) muutaman minuutin kuluessa lähetetään Microsoftin platform ajasta. Voi olla muutaman harha, joka voi kestää kauemmin.

>[AZURE.NOTE] Azure ilmoituksen keskittimet on käytäntö on lisätty, voit poistaa minkä tahansa push-ilmoitukset, jotka eivät voi toimittaa PNS puolessa tunnissa. Tämän viiveen voi johtua useista syistä, saat eniten yleisesti PNS on rajoitin sovelluksen.

###<a name="8---is-there-any-latency-guarantee"></a>8. on minkä tahansa viive on voimassa?
Push-ilmoitukset (ne toimittaa ulkoinen, käyttöjärjestelmäkohtaiset Push-ilmoituspalvelu) laatu, koska ei ei viiveen takaa. Yleensä push-ilmoitukset useimpia perille muutaman minuutin kuluessa.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. Mitä minun täytyy ottaa huomioon ratkaisua nimitilan ja ilmoituksen keskittimet suunniteltaessa huomioitavista?

####<a name="mobile-appenvironment"></a>Mobiili App/ympäristössä

* Pitäisi olla yksi ilmoitus-toiminnossa mobiilisovelluksen kohti ympäristön kohden.
* Usean vuokraajan tilanteessa kunkin vuokraajan pitäisi olla erilliset toiminnossa.
* Jaettava koskaan samaan ilmoituksen keskittimeen, numero- ja ympäristöjen välillä, kun tämä saattaa aiheuttaa ongelmia rivi alaspäin lähetettäessä ilmoitukset. kuten Apple on eristetyn ja tuotannon Push päätepisteet kunkin ottaa erillisiä käyttöoikeuksia.
* Oletusarvon mukaan voit lähettää testi ilmoitukset rekisteröity laitteet Azure-portaalin tai Visual Studiossa Azure integroitu osan kautta. Raja-arvo on määritetty 10-laitteet, joka valitaan satunnaisesti rekisteröinti käytetään varannon.

>[AZURE.NOTE] Jos valitsemalla Apple eristetyn varmenteella alun perin määritetty ja se sitten uudelleen käyttämään Apple-tuotannon varmennetta, vanha laitteen tunnusten tapauksessa saattavat muuttua virheellisiksi uusi sertifikaatin ja aiheuttaa työntää epäonnistuu. On parasta erottaa oman tuotannon ja testaa ympäristöissä ja käyttää eri ympäristöissä eri keskittimet.

####<a name="pns-credentials"></a>PNS tunnistetiedot

Kun mobile-sovellus rekisteröidään on ympäristö developer-portaalissa (kuten Apple tai Google jne.) Valitse saat sovelluksen tunnus ja suojaus tunnuksia, jossa app Taustajärjestelmä tarvitsee ympäristö Push-ilmoituksen palveluihin voivan lähettää push-ilmoitukset laitteet. Nämä suojauksen tunnuksia, joka voi olla varmenteet (kuten Apple iOS- tai Windows Phone) tai suojauksen näppäimet (Google Android, Windows jne) on määritettävä ilmoitus keskittimet. Tämä tapahtuu yleensä ilmoituksen keskittimeen tasolla, mutta voi tehdä myös usean vuokraajan tilanne nimitilan tasolla.

####<a name="namespaces"></a>Nimitilan

Nimitilan voidaan käyttöönoton ryhmittelyyn.  Se voidaan myös edustavan kaikki ilmoituksen keskittimet, kaikki alihallinnat saman sovelluksen usean Vuokraajan skenaariossa.

####<a name="geo-distribution"></a>GEO-jakauman.

GEO-jakauman ei aina kriittinen push ilmoituksen tilanteissa. On huomattava eri Push ilmoituksen Services (kuten APN, GCM jne.), joka push-ilmoitukset toimitetaan kädessä laitteet, ei tasaisesti jaettu.

Jos sinulla on jokin sovellus, jota käytetään kaikkialla maailmassa, voit luoda useita keskittimet eri nimitilan hyödyntämistä ilmoituksen keskittimet palvelun käytettävyys eri Azure alueiden maapallon ympärille.

>[AZURE.NOTE] Tämä lisää hallinta kustannukset - erityisen ympärille merkintöjä, jotta tämä todella ei kannata ja vain täytyy tehdä, jos eksplisiittinen tarvita.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. kannattaa tehdä merkintöjen app taustasta tai suoraan asiakkaan laitteiden?
Merkintöjen app taustasta on hyötyä, kun sinun on tehtävä ennen kuin luot rekisteröinnin asiakkaan todennus tai sinulla on tunnisteita, jotka on luotu tai muokannut sovelluksen logiikkaa perusteella app Taustajärjestelmä. Lisätietoja, Lue lisää [Taustajärjestelmä rekisteröinti ohjeet] ja [Taustajärjestelmä rekisteröinti ohjeet - 2] -sivuilla.

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. push-ilmoituksen toimituksen suojausmalli mikä on?
Azure ilmoituksen keskittimet käyttää [Jaettu Access allekirjoituksen (SAS)](../storage/storage-dotnet-shared-access-signature-part-1.md)-pohjainen suojausmalli. Voit käyttää SAS tunnusten nimitilan päätasolle tai hajautetun ilmoituksen keskittimet tasolla. SAS näiden tunnusten voidaan määrittää kanssa eri käyttöoikeuksien myöntämissääntöjä kuten Lähetä viesti käyttöoikeudet, kuunnella ilmoituksen käyttöoikeudet jne. Lisätietoja ovat käytettävissä [NH suojausmalli] asiakirjassa.

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. miten käsittelee luottamuksellisten tietojen push-ilmoitukset?
Kaikki ilmoitukset toimitetaan laitteiden mukaan ympäristö Push-ilmoituksen Services (PNS). Kun lähettäjän lähettää ilmoituksen Azure ilmoituksen porttiin syy käsitellä, ja välittää ilmoituksen vastaaviin PNS.

Kaikki yhteydet lähettäjältä Azure ilmoitukset porttiin ja PNS käyttää HTTPS.

>[AZURE.NOTE] Azure ilmoitukset keskittimet ei kirjaa sanoman paketti minkäänlaista.

Lähettää paketin luottamuksellisten tietojen mutta suosittelemme suojatun Push kuvion johon lähettäjän toimittaa ping-' ilmoituksen viestin tunnuksella laitteeseen ilman luottamukselliset tiedot ja sovelluksen laitteeseen saa tämän paketin, se on voivat soittaa suojatun API suoraan hakeaksesi viestin tiedot. Yllä olevien ohjeiden kuvion ottamisesta käyttöön koskeva opas on käytettävissä [NH - opetusohjelma suojatun Push] -sivulla.

##<a name="operations"></a>Toiminnot
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. mitä on tietojen palauttaminen (DR) Tarinan?
Olemme tarkistustyökalua metatietojen palauttaminen Microsoftin lopussa (ilmoitus-toiminnossa nimi, yhteysmerkkijonon ja muita tärkeitä tietoja). Kun DR tilanne käynnistyy, merkintöjen tiedot ovat **vain segmentin** ilmoituksen keskittimet infrastruktuuria, joka menetetään. Tarvitset toteuttamisesta ratkaista täytä uudelleen uuden keskittimeen jälkeinen palautus kyselyjä tiedoista.

- *Vaihe1* - ilmoituksen toissijaiseen keskittimeen luominen eri palvelinkeskukseen. Voit luoda tämän suoraan selaimessa DR tapahtuman aikaan tai voit luoda vaihtoehto Hae Siirry. Se ei tee paljon ero minkä vaihtoehdon valitset koska ilmoituksen keskittimeen valmistelu on suhteellisen nopea prosessi hetkinen järjestyksessä. Yhteen alusta suojata voit DR tapahtuman vaikuttavat hallintaominaisuuksia, joten se on erittäin suositeltavaa-vaihtoehto.

- *Vaiheen2 ohjeiden mukaisesti* - hydraatti toissijaiseen ilmoituksen keskittimeen, ensisijainen ilmoitus-toiminnosta merkintöjen kanssa. On ei ole suositeltavaa säilyttää vertaisyhteyksistä Valitse molemmat keskittimet ja yritä voit synkronoida ne suoraan selaimessa, kun merkintöjen on tulossa-yleensä, joka ei toimi hyvin luontaisen taipumusta merkintöjen olevaksi vuoksi PNS reunassa. Ilmoitus keskittimet Puhdista ne saadaan vanhentunut tai ei kelpaa merkintöjen PNS palautetta.  

Suositus on käyttää sovelluksen Taustajärjestelmä kumpi tahansa:

- Ylläpitää merkintöjen sen päässä määritettyjen siten, että se voi tehdä joukkona Lisää toissijainen ilmoitus-keskittimeen, jos DR

**TAI**

- Hakee tavalliseen vedosta merkintöjen varmuuskopioina ensisijainen-toiminnosta, ja sitten syötteessä lisätä Toissijainen NH.

>[AZURE.NOTE] Merkintöjen Vie/tuo-toiminnon käyttöön vakio taso on kuvattu [Merkintöjen Vie/tuo] -asiakirja.

Jos sinulla ei ole taustassa, sitten, kun sovellus käynnistyy millä tahansa laitteella kohde, uusi rekisteröinti suoritettava toissijainen ilmoitus-toiminnossa ja tekstin toissijaisen ilmoitus-toiminnossa on kaikkien aktiivisten laitteiden rekisteröity.

Entä huonot puolet on, että kun laitteet, jossa sovellusten ole avannut ei saa ilmoituksia ajanjakson ole.

###<a name="2---is-there-any-audit-log-capability"></a>2. on minkä tahansa valvonta log ominaisuuksien?
Kaikki ilmoituksen keskittimet hallintatoiminnot Siirry toimintojen lokit joka niitä julkaista [Azure perinteinen Portal].

##<a name="monitoring--troubleshooting"></a>Seuranta ja vianmääritys
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. mitä vianmäärityksen ominaisuuksia on käytettävissä?
Azure ilmoituksen keskittimet on useita ominaisuuksia, voit tehdä yleisiä vianmääritysohjeita on erityisesti useimmin käytetty vaihtoehto poistetusta ilmoitukset ympärille. Lisätietoja on Microsoftin [NH - vianmääritys] julkaisu.

###<a name="2---what-telemetry-features-are-available"></a>2. telemetriatietojen mitä ominaisuuksia on käytettävissä?
Azure ilmoituksen keskittimet ottaa telemetriatietojen tietojen tarkasteleminen [Azure perinteinen Portal]. Käytettävissä olevat arvot tiedot ovat käytettävissä [NH - arvot] -sivu.

>[AZURE.NOTE] Onnistuneiden ilmoitukset tarkoittavat vain push-ilmoitukset on toimitettu ulkoisen Push-ilmoituspalvelu (kuten APNS Apple, Google jne GCM). Kannattaa pitää ilmoituksen laitteiden PNS ylöspäin. Yleensä PNS ei näy toimituksen arvot kolmansille osapuolille.  

Annamme myös voidaan viedä telemetriatietoja tiedot ohjelmallisesti ( **Vakio** taso). Katso lisätietoja [NH - arvot-malli] .

[Azure perinteinen Portal]: https://manage.windowsazure.com
[Ilmoitus keskittimet hinnat]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Ilmoitus keskittimet SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle ajat]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - REST API]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH - käytön aloittamisen opetusohjelmat]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome-sovellukset-opetusohjelma]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Taustajärjestelmä rekisteröinti ohjeet]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Taustajärjestelmä rekisteröinti ohjeet - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[NH suojausmalli]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH - suojatun Push-opetusohjelma]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH - vianmääritys]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - arvot]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - arvot-Esimerkki]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Merkintöjen tuonti tai vienti]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[Valmis objektit]: https://github.com/Azure/azure-notificationhubs-samples
[Azure-mobiilisovellukset]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[Sovelluksen palvelun hinnat]: https://azure.microsoft.com/en-us/pricing/details/app-service/
