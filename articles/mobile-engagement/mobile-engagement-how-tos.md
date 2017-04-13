<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - Reach poistamisesta"
   description="Käyttäjän käyttöliittymän yleistä Azure Mobile välitys" 
   services="mobile-engagement" 
   documentationCenter="" 
   authors="piyushjo" 
   manager="dwrede" 
   editor=""/>

<tags
   ms.service="mobile-engagement"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="mobile-multiple"
   ms.workload="mobile" 
   ms.date="08/19/2016"
   ms.author="piyushjo"/>

# <a name="how-to-get-started-using-and-managing-pushes-to-reach-out-to-your-end-users"></a>Pikaviestien käyttäminen ja hallinta vie käyttäjiesi saavuttaminen

Kun SDK on täysin integroitu sovelluksen, voit aloittaa käytön Push-ilmoitukset sovelluksesi käyttäjät Käyttöliittymän Reach-osa.  

## <a name="do-your-first-push-notification-campaign"></a>Tee ensimmäinen Push-ilmoituksen markkinointikampanja
-    Varmista, että saatavilla on integroitu sovellus SDK: N kanssa. 
-    Valitse sovellus
 
![First1][1]

-    Siirry "Reach" osa ja valitse "uusi ilmoitus"
 
![First2][2]

-    Luo uusi markkinointikampanja ja anna sille nimi
 
 ![First3][3]

-    Valitse, miten ilmoitus lähetetään, kuin sovelluskohtaisesta vain
 
![First4][4]

-    Haluat push viestin luominen
 
![First5][5]

-    Voi kirjoittaa otsikon huomautus (valinnainen) käyttöön.
-    Kirjoita push viestin sisältöä.
-    Voit ladata kuvan. Huomaa, että tiedoston koko voi olla enintään 32 768 tavun.
-    Käytössä on myös mahdollista valita muita vaihtoehtoja, mutta tässä opetusohjelmassa kohdistus, emme lukee, myöhemmin.

-    Valitse sisältötyyppi vain ilmoitus
 
![First6][6]

-    Push-markkinointikampanjan luominen ja se näkyy markkinointikampanja-luettelosta.
 
![First7][7]

## <a name="test-your-push-notification-campaign"></a>Testaa Push-ilmoituksen markkinointikampanja
![Test1][8]

-    Rekisteröi laite.
-    Valitse haluamasi laite push-valintaruutu.
-    Valitse Lähetä painallus laitteeseen "Testi"-painiketta.
 
![Test2][9]

-    Aktivoi markkinointikampanja
 
![Test3][10]

-    Nyt kun olet luonut että markkinointikampanja haluat aktivoida sen ilmoituksen sijaita käyttäjille.
 
## <a name="send-personalized-pushes"></a>Lähettää mukautetun työntää
-    Tässä esimerkissä luodaan push, jossa mukautetun hyvityksen koodi on syötetty push-ilmoitus.
 
![Personalize1][11]

Mukauttaminen works korvaamalla merkki mukaan app tiedot tunnisteesta niin, sinun on varmistaaksesi, että käyttäjällä on sovelluksen-tiedot ERISNIMI ensin. Tässä esimerkissä kohdennettujen käyttäjillä on tiedot sovelluksen tunnisteen rebate_code määritetty.
Kuten näet yllä push-ilmoituksen sisältöä on merkki ${rebate_code}, joka ilmaisee, että se on korvataan sovelluksen tiedot-tunniste asiakirjan sisältöä.

> Varoitus: Sovelluksen tiedot-tunniste on määritetty käyttäjälle, jos käyttäjä saa painallus.

-    Tulos
 
![Personalize2][12]

### <a name="you-can-further-personalize-the-text-your-notification"></a>Voit edelleen muokata tekstiä ilmoitus
![Personalize3][13]

-    Ilmoituksen otsikko
-    Ja viestin sisältö.
-    Valitse haluamasi ilmoitus (teksti- tai Web-näkymän)
 
![Personalize4][14]

### <a name="the-body-of-an-announcement-may-also-be-personalized-with"></a>Ilmoitus leipätekstiin voi mukauttaa myös kanssa:
-    Toiminnon URL-osoite, jos haluat mukauttaa aloitussivu
-    Otsikon
-    Viestin tekstiosaan.
 
 
## <a name="differentiate-your-push-notification-in-or-out-of-app"></a>Erottaa Your Push-ilmoitus (sisään tai ulos sovelluksesta)
-    Valitse haluamasi ilmoituksen push, valitse sovelluksen, siirry kohtaan "Saavuttaa", valitse tai luodaan push-markkinointikampanjan ja "Ilmoitus"-kohdassa.
 
-    Valitse haluamasi "toimituksen tila".
-    Napsauta "Rajoita toiminnot"-valintaruutu, kun haluat ilmoituksen ilmenee määritetyt toiminnot (näyttöä).

![Differentiate1][15]

### <a name="out-of-app-only-delivery-mode"></a>"Ulos sovelluksesta vain" toimituksen tila
![Differentiate2][16]

"Ulos sovelluksesta vain" Toimitustapa tarjoaa push-ilmoituksen, kun sovellus suljetaan. Tämä on vakio push-ilmoitus.
Kun valitset "ulos sovelluksesta vain", on jo annettu varmenteet-ympäristö, joka luo sovelluksen (APN tai GCM).

### <a name="see-also"></a>Katso myös
-  [Apple Push-ilmoituspalvelu – varmenteet](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9), Google Cloud Messaging – varmenteen] (http://developer.android.com/google/gcm/index.html) 

### <a name="in-app-only-delivery-mode"></a>"Sovelluskohtaisesta vain" toimitustapa
![Differentiate3][17]

"Sovelluskohtaisesta vain" toimituksen tila on push-ilmoituksen, kun sovellus on käynnissä.
Tämä ilmoitus varten ei tarvitse käydä läpi APN ja GCM järjestelmä.
Voit käyttää sovelluskohtaisesta toimituksen järjestelmä saavuttaa oman loppukäyttäjien.
Voit mukauttaa ilmoituksen täysin ja päättää, johon tehtävä (näyttö) ilmoitus tulee näkyviin.

### <a name="anytime-delivery-mode"></a>"Milloin tahansa" toimitustapa
Voit valita "Milloin tahansa" toimitustapa, varmistaa, että voit muodostaa yhteys käyttäjälle, onko sovellus vai ei.
Kun valitset "Milloin tahansa", on jo annettu varmenteet-ympäristö, joka sovelluksesi on perustuvat (APN tai GCM). 
 
## <a name="schedule-a-push-campaign"></a>Aikataulun Push-markkinointikampanja
### <a name="plan-to-start-a-campaign"></a>Aloita markkinointikampanjan suunnitelma
![Shedule1][18]

On 21, maaliskuussa ja on Yammerissa, jotta ja höylätty varten 22, maaliskuussa keskiyön-palvelussa. Ei ole pysyy edessä tekemään push käyttöliittymän! Voit suunnitella etukäteen tarkka minuutit ilmoitukset lähetetään.
-    Poistaa "Ei" valintaruutu ja valitse alkamisaika 
-    Valitse päivämäärä ja kellonaika, aloitustapa push-markkinointikampanjan.

### <a name="plan-to-end-a-campaign"></a>Voit lopettaa markkinointikampanjan suunnitelma
![Shedule2][19]

Haluat, että markkinointikampanja-25, maaliskuussa pysähtyvän 3.00 pm, mutta tiedät, että et voi on käsiteltävä.
Ei ole pysyy edessä push-liittymän! Voit suunnitella etukäteen tarkka minuutit että markkinointikampanja lopetetaan.
-    Valitse "Ei"-valintaruutu tai valitse päättymisaika
-    Valitse päivämäärä ja kellonaika, push-markkinointikampanjan käsittelyä varten.

### <a name="end-a-campaign-manually"></a>Lopeta markkinointikampanjan manuaalisesti
![Shedule3][20]

Oletusarvon mukaan "Ei" valintaruudut ovat valittuina.
Tämä tarkoittaa, markkinointikampanja käynnistyy heti, kun aktivoida sen reach-osassa ja päättyy, kun haluat lopettaa se on saatavilla-osassa.
 
> Huomautus: Kampanjat ilman päättymispäivän luotu painallus tallentaa paikallisesti laitteeseen ja näyttää seuraavan kerran, sovellus avataan, vaikka markkinointikampanja lopetetaan manuaalisesti.

## <a name="enhance-a-push-notification-with-a-text-view"></a>Paranna Push ilmoituksen teksti-näkymässä
### <a name="what-is-a-text-view"></a>Tekstin näkymän ominaisuudet
![TextView1][21]

Tekstinäkymä on ponnahdusikkunan tekstin sisältöä. Tämä ponnahdusikkuna tulee näkyviin, kun käyttäjä on napsautettu push-ilmoituksen.
Teksti-näkymässä voit esittää käyttäjälle enemmän sisältöä. Tämä on myös esittää toiminnon, kuten sivun sovelluksesi uudelleenohjausta myymälän avaaminen web-sivun sähköpostitse, geo lokalisoitu haun hyppääminen kutsun mahdollisuuden muille...

### <a name="example-text-view"></a>Esimerkki: Tekstinäkymä
-    Push-ilmoituksen markkinointikampanjan luominen "Reach"-kohta ja nimeä että markkinointikampanja
 
![TextView2][22]

-    Kirjoita viesti, joka tulee näkyviin ilmoitus.
-    Ilmoitus "teksti" sisällön tyypin valitseminen
 
![TextView3][23]

> Huomautus: kun painat tekstin näkymän, se aina sisältyy ilmoituksen ensin. 

- Määritä teksti (sen jälkeen, kun valittu ilmoitus tekstisisältöön aliraportti osa tulee näkyviin, avulla voit määrittää tekstin haluat näkyvän.)
 
![TextView4][24]

-    Kirjoita otsikko, joka tulee näkyviin viestin yläosassa.
-    Kirjoita teksti-näkymän pääsisältöön.
-    Kirjoita sisältöön, joka tulee näkyviin toiminto-painiketta (toimintopainikkeen avulla, sovellus tietyn toiminnon, kuten avaamalla sovellus App Storesta tai mitä tahansa lähteiden, voit antaa uudelleenohjausta sivu).
-    Kirjoita sisältö, jonka haluat näkyvän exit-painikkeen (valitsemalla Lopeta-painikkeen tekstin näkymän häviävät.)
 
-    Push-ilmoituksen markkinointikampanjan luominen ja se näkyy markkinointikampanja-luettelossa.
 
![TextView5][25]

-    Ota käyttöön push ilmoitus-markkinointikampanjan lähettäminen Tekstinäkymä käyttäjille.
 
![TextView6][26]

-    Tulos
 
![TextView7][27]

-    Käyttäjä saa ilmoituksen ja napsauttamalla sitä.
-    Tekstinäkymä näkyy salliminen vuorovaikutteisesti ponnahdusikkuna.

## <a name="enhance-a-push-notification-with-a-web-view"></a>Paranna Push ilmoituksen Web-näkymässä
### <a name="what-is-a-web-view"></a>Web-näkymän ominaisuudet
![WebView1][28]

Web-näkymä on ponnahdusikkunan web-sisältöä. Tämä ponnahdusikkuna tulee näkyviin, kun käyttäjä on napsautettu push-ilmoituksen.
Web-näkymässä voit on käyttäjä lisää käsittelemisen.
Tämä on myös esittää App Store-toiminto, kuten uudelleenohjaus puhelun mahdollisuuden avaaminen web-sivun sähköpostitse, geo lokalisoitu haun, yms...

### <a name="example-web-view"></a>Esimerkki: Web-näkymä
-    Push-markkinointikampanjan luominen "Reach"-kohta ja kirjoita että markkinointikampanja nimi.
 
![WebView2][29]

-    Kirjoita viesti, joka tulee näkyviin ilmoitus.
-    Valitse ilmoitus sisältötyypin "WWW"
 
![WebView3][30]

### <a name="about-announcement-types"></a>Tietoja ilmoitus tyypit:
- Vain ilmoituksen: on yksinkertainen vakio ilmoituksen. Tarkoittaa, että, jos käyttäjä valitsee sen, ei ole lisänäkymät näkyvät, mutta vain siihen liittyvä toiminto suoritetaan.
- Tekstin ilmoitus: on ilmoitus, joka käyttää tekstin näkymän katsaus käyttäjälle.
- Web-ilmoitus: ilmoitus, joka käyttää käyttäjä voi tarkastella web-näkymä on.
Valitse sisältö-Web-ilmoitus".

> Huomautus: Kun painat web-näkymässä, se aina sisältyy ilmoituksen ensin.

- Määritä verkkosisällön (sen jälkeen, kun valittu ilmoitus verkkosisällön tästä tulee näkyviin, avulla voit määrittää näkymän verkkosisällön haluat näytettävän.)

 
![WebView4][31]

-    Kirjoita otsikko, joka tulee näkyviin (valinnainen) viestin yläreunassa.
-    Kirjoita HTML-koodi tähän.
-    Valitse lähteen muokkaaminen painike Vaihda edition ja näet, miten se näyttää.
-    Kirjoita sisältöön, joka tulee näkyviin toiminto-painiketta (toimintopainikkeen mahdollistaa tietyn toiminnon, kuten avaamalla sivun sovelluksen uudelleenohjausta säilön tai mitä tahansa lähteiden, voit antaa sovellus).
-    Kirjoita sisältö, jonka haluat näkyvän Lopeta-painikkeen (valitsemalla Lopeta-painikkeen web-näkymän häviävät).
 
-    Tulos
 
![WebView5][32]

-    Käyttäjä saa ilmoituksen ja napsauttamalla sitä.
-    Tekstinäkymä näkyy salliminen vuorovaikutteisesti ponnahdusikkuna.

<!--Image references-->
[1]: ./media/mobile-engagement-how-tos/First1.png
[2]: ./media/mobile-engagement-how-tos/First2.png
[3]: ./media/mobile-engagement-how-tos/First3.png
[4]: ./media/mobile-engagement-how-tos/First4.png
[5]: ./media/mobile-engagement-how-tos/First5.png
[6]: ./media/mobile-engagement-how-tos/First6.png
[7]: ./media/mobile-engagement-how-tos/First7.png
[8]: ./media/mobile-engagement-how-tos/Test1.png
[9]: ./media/mobile-engagement-how-tos/Test2.png
[10]: ./media/mobile-engagement-how-tos/Test3.png
[11]: ./media/mobile-engagement-how-tos/Personalize1.png
[12]: ./media/mobile-engagement-how-tos/Personalize2.png
[13]: ./media/mobile-engagement-how-tos/Personalize3.png
[14]: ./media/mobile-engagement-how-tos/Personalize4.png
[15]: ./media/mobile-engagement-how-tos/Differentiate1.png
[16]: ./media/mobile-engagement-how-tos/Differentiate2.png
[17]: ./media/mobile-engagement-how-tos/Differentiate3.png
[18]: ./media/mobile-engagement-how-tos/Schedule1.png
[19]: ./media/mobile-engagement-how-tos/Schedule2.png
[20]: ./media/mobile-engagement-how-tos/Schedule3.png
[21]: ./media/mobile-engagement-how-tos/TextView1.png
[22]: ./media/mobile-engagement-how-tos/TextView2.png
[23]: ./media/mobile-engagement-how-tos/TextView3.png
[24]: ./media/mobile-engagement-how-tos/TextView4.png
[25]: ./media/mobile-engagement-how-tos/TextView5.png
[26]: ./media/mobile-engagement-how-tos/TextView6.png
[27]: ./media/mobile-engagement-how-tos/TextView7.png
[28]: ./media/mobile-engagement-how-tos/WebView1.png
[29]: ./media/mobile-engagement-how-tos/WebView2.png
[30]: ./media/mobile-engagement-how-tos/WebView3.png
[31]: ./media/mobile-engagement-how-tos/WebView4.png
[32]: ./media/mobile-engagement-how-tos/WebView5.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
