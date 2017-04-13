<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - Reach markkinointikampanja" 
   description="Laern tapaa, jolla voit luoda ja hallita push-ilmoituksen kampanjoihin liittyviä käyttämällä Azure Mobile välitys" 
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


# <a name="how-to-create-and-manage-push-notification-campaigns"></a>Voit luoda ja hallita push ilmoituksen kampanjat
Käyttöliittymän Reach-osassa voit luoda uuden Push-markkinointikampanjan monimutkaisen kaavan antamalla kaikki tiedot, jotka haluat lähettää push-ilmoitus. Push-markkinointikampanjan vaihtoehdot määräytyvät tapahtuu hieman eri tavalla sen mukaan, neljä markkinointikampanjan tyypit: ilmoitukset, kyselyiden, Vie tiedot ja ruudut (vain Windows Phone).

### <a name="option-applies-to"></a>Asetus koskee:
- Kielet: Kaikki (ilmoituksia, kyselyiden, tietojen työntää-ruutujen)
- Markkinointikampanjan: Kaikki (ilmoituksia, kyselyiden, tietojen työntää-ruutujen)
- Ilmoitus: Ilmoitukset, kyselyiden
- Sisältö: Markkinointikampanjan mistäkin yksilöllinen
- Käyttäjäryhmä: Kaikki (ilmoituksia, kyselyiden, tietojen työntää-ruutujen)
- Ajanjakson: ilmoitukset, kyselyiden-ruudut
- Testi: Kaikki (ilmoituksia, kyselyiden, tietojen työntää-ruutujen)
 
![Saatavilla Campaign1][20]

## <a name="languages"></a>Kielet
Voit lähettää oman Push eri version laitteita, joissa on määritetty käyttämään eri kielillä kielten avattava valikko. Oletusarvoisesti kaikissa laitteissa saavat saman Push riippumatta siitä, mitä ne on määritetty käyttämään kieli. Eri kieli laitteen mukana käyttäjät saavat painallus oletuskieli-versio. Monia push markkinointikampanjan asetukset avulla voit määrittää vaihtoehtoiset sisällön kunkin valitset lisää kieliä. 
 
![Saatavilla Campaign2][21]

### <a name="language-differences-apply-to"></a>Kielen erot koskevat:
- Kielet: Yksilöllisten kieliä voi olla valittuna oletuskielen lisäksi
- Markkinointikampanjan: Kaikkien kielten sama
- Ilmoitus: Yksilöllisten lisäksi oletuskielen kullekin kielelle
- Sisältö: Yksilöllisten lisäksi oletuskielen kullekin kielelle
- Käyttäjäryhmä: Voi suodattaa eri kielellä ehdon mukaan
- Ajanjakson: saman kaikkien kielten
- Testi: Voidaan lähettää kunkin kielen kerrallaan.
 
### <a name="supported-languages"></a>Tukemilla kielillä:
- Arabia (ar) 
- Bulgaria (bg) 
- Katalaani (ca) 
- Kiina (zh) 
- Kroaatti (hr) 
- Czech (CS) 
- Tanska (da) 
- Hollanti (noudattavat) 
- Englanti (en) 
- Suomi (fi) 
- Ranska (f) 
- Saksa (de) 
- Kreikka (el) 
- Heprea (hän) 
- Hindi (suuren) 
- Unkari (hu) 
- Indonesia (tunnus) 
- Italia (se) 
- Japani (ja) 
- Korea (ko) 
- Latvia (lv) 
- Liettua (lt) 
- Malaiji (macrolanguage) (ms) 
- Norja (bokmål) (Huomautus) 
- Puola (pl) 
- Portugali (pt) 
- Romania (ro) 
- Venäjä (ru) 
- Serbia (sr) 
- Slovakki (sk) 
- Sloveeni (sl) 
- Espanja (es) 
- Ruotsi (sv) 
- Tagalog (tl) 
- Thai (th) 
- Turkki (tr) 
- Ukraina (Iso-Britannia) 
- Vietnam (vi) 
 
## <a name="campaign"></a>Markkinointikampanja
Markkinointikampanja-osan avulla voit määrittää nimen ja että markkinointikampanja luokan kuin, jos haluat ohittaa Push-markkinointikampanjan käyttäjäryhmä-osan ja lähettää sen sijaan tässä markkinointikampanjan saavuttaa Ohjelmointirajapinnan (ja joitakin osia, joiden vähäistä Push API) kautta. Luokat voidaan käyttää ennalta määritettyjä asetuksia perustuvat ohjausobjektin sovelluskohtaisesta ilmoitukset mukautettu ilmoitus käyttämällä mallia. Saat saavuttaa Ohjelmointirajapinnan kautta aiemmin "luokat" luettelo.

> Varoitus: Jos käytät "Ohita yleisön push lähetetään käyttäjien Ohjelmointirajapinnan kautta"-vaihtoehto Reach-markkinointikampanjan "Markkinointikampanjan"-osassa markkinointikampanja ei Lähetä automaattisesti, sinun on lähettää manuaalisesti saavuttaa Ohjelmointirajapinnan kautta.
 
![Saatavilla Campaign3][22]
 
### <a name="option-applies-to"></a>Asetus koskee:
- Nimi: kaikki
- Luokka: Ilmoitukset, kyselyiden
- Ohita käyttäjäryhmälle push lähetetään käyttäjien Ohjelmointirajapinnan kautta: kaikki
 
## <a name="notification"></a>Ilmoitus
Ilmoitus-osan avulla voit määrittää perusasetukset push myös: otsikko Push, viestin, sovellus-kuva, tai jos se on yrittämästä. Monta ilmoitusasetusten määrittäminen ovat laitteen ympäristössä. Voit valita, että push lähetetään "sovelluksessa" tai "ulos sovelluksesta" tai molemmat. (Muista, että käyttäjät voivat "sisältyy" tai "osallistumiseen liittyvät", "ulos sovellus" Vie osoitteessa käyttöjärjestelmän tason niiden laitteissa ja Azure Mobile välitys ei voi ohittaa tämän asetuksen. Huomaa, että yhteys Ohjelmointirajapinnan käsittelee "sovelluksessa" ja "poissa-sovelluksen" Vie. Push-Ohjelmointirajapinnan voidaan käsitellä "ulos sovelluksesta" työntää liian.) Työntää voi mukauttaa kuvia tai HTML-sisältöä, kuten laaja linkit linkittämisen sovelluksen ulkopuolella tai johonkin muuhun sijaintiin (Android SDK 2.1.0 tai uudempi versio vaaditaan käyttötarkoitusta luokat)-sovelluksen kanssa. Voit muuttaa kuvaketta tai iOS-merkin ja lähettää teksti tai web sisällön (html sisällön URL-Osoitteen linkin toiseen sijaintiin sisällä tai ulkopuolella sovelluksen valikko). Voit tehdä Android-laitteille soi tai värinätoiminnot painallus kanssa. (Muista, että sinun on oikea SDK käyttöoikeudet Android luettelon soi tai laitteen värinätoiminnot.) Ei tällä hetkellä ei alan vakio Android "laajemmin" kokoja, koska Näyttökoot ovat eri kaikkiin laitteisiin, mutta 400 x 100 kuvien käsitteleminen lähes minkä tahansa näytön koko.

### <a name="delivery-types"></a>Toimituksen tyypit:
-    Ulos sovelluksesta vain: ilmoituksen toimitetaan, kun käyttäjä ei käytä sovelluksen.
- Poissa-sovelluksen vain ilmoituksen edellyttää varmennetta Apple tai Google (APN tai GCM varmenne).
- Sovellusten vain: ilmoitus näkyy vain, kun sovellus on käynnissä.
- Huomautus käyttää Capptain toimituksen käyttäjän saavuttamiseksi. Voit mukauttaa kokonaan oman push visuaalisen asettelun/näyttämisen.
- Milloin tahansa: Tämä vaihtoehto takaa lähettää ilmoituksen joko sovellus on käynnissä vai ei.

 
![Saatavilla Campaign4][23]

### <a name="option-applies-to"></a>Asetus koskee:
- Ilmoitus: Ilmoitukset, kyselyiden
 
## <a name="content"></a>Sisältö
Sisältö-osassa voit muokata sisältöä ilmoitukset, kyselyiden, Vie tiedot, ja ruudut (vain Windows Phone). Push-Kampanjat sisällön asetus on markkinointikampanjan tyyppi. 

### <a name="see-also"></a>Katso myös
- [Käyttöliittymän asiakirjat - saavuttaa - Push-sisältö][Link 29]
 
![Saatavilla Campaign5][24]

## <a name="audience"></a>Käyttäjäryhmälle
Käyttäjäryhmä-osan avulla voit määrittää vakio luettelon kohteiden rajoittaminen markkinointikampanjan tai rajoitukset, että markkinointikampanja mukautetun ehtojen perusteella. Vakio-asetukset, voit rajoittaa yleisö voit push uusi tai vanha tai vain alkuperäisen push-käyttäjille. Voit myös määrittää kiintiön rajoita painallus saavat käyttäjät. Voit manuaalisesti muokata lausekkeen miten että markkinointikampanja suodatetaan sisältää vähintään yhden ehdon kohde käyttäjille. Voit kirjoittaa lausekkeen yleisön manuaalisesti. Näiden lausekkeen erikseen on määritettävä ehdot välinen suhde. Ehtona kuvaaman tunnus, joka on alettava kirjaimella pääoman ja eivät saa sisältää välilyöntejä. Ehdot välinen suhde on kuvattu käyttäen "ja", "tai", "ei" operaattoreita sekä "(",")". Esimerkki: "Criterion1 tai (Criterion1 ja ei-Criterion2)".

> Huomautus: Kampanjat sisältyvät suurelle Käyttäjäryhmälle kohdistamisen tarkistuksen palvelinpuolen voi olla hidas, etenkin silloin, kun yrität käynnistää Kampanjat samanaikaisesti.

- Jos mahdollista käynnistää vain yhden markkinointikampanjan kerrallaan.
- Enintään käynnistää vain neljä Kampanjat kerrallaan.
- Siirtää vain aktiiviset käyttäjät (valintaruutu "osallistuminen vain käyttäjät, joilla voi siirtyä alkuperäisen Push" ja "Osallistuminen vain aktiiviset käyttäjät") niin, että tarkistettavan on vain oman käyttäjät, jotka yhä sovellus on asennettuna ja käyttää sitä.
Kun yleisön on määritetty, voit selvittää, kuinka monta käyttäjää saavat tämän Push simulate-painiketta. Tämä laskee mahdollisesti kohteena (tämä on käyttäjien satunnaisia otoksen perusteella arvion) käyttäjäryhmän tunnetut käyttäjien lukumäärä. Huomaa, että käyttäjät, jotka olet poistanut sovelluksen ovat myös osa käyttäjäryhmän, mutta ei voi siirtyä.

### <a name="see-also"></a>Katso myös
- [Käyttöliittymän asiakirjat - Reach - uusi Push vertailuarvo][Link 28]

![Saatavilla Campaign6][25]

### <a name="edit-expression"></a>Lausekkeen muokkaaminen
![Saatavilla Campaign7][26]
 
### <a name="limit-your-audience-option-applies-to"></a>Raja osallistujat-asetus on käytettävissä:
- Vain osaa käyttäjistä uutissyötteesi: kaikki (ilmoituksia, kyselyiden, Vie tiedot, ruudut)
- Vain vanhat käyttäjät osallistuminen: kaikki (ilmoituksia, kyselyiden, Vie tiedot, ruudut)
- Osallistuminen vain uudet käyttäjät: kaikki (ilmoituksia, kyselyiden, Vie tiedot, ruudut)
- Osallistuminen vain käyttämättömänä käyttäjät: ilmoitukset, kyselyiden-ruudut
- Osallistuminen vain aktiiviset käyttäjät: kaikki (ilmoituksia, kyselyiden, Vie tiedot, ruudut)
- Vain käyttäjät, joilla voi siirtyä käyttämällä alkuperäisen Push osallistuminen: ilmoitukset, kyselyiden
 
## <a name="time-frame"></a>Ajanjakson
Aikaväli-osassa voit määrittää, kun painallus lähetetään tai aikavälin voit jättää tyhjäksi, jotta voit aloittaa markkinointikampanja heti. Muista, että avulla loppukäyttäjien aikavyöhykkeen voi aloittaa markkinointikampanja päivää ennen kuin voit odottaa, että loppukäyttäjät Aasiassa ja lähettää työntää kerrallaan, kunnes kaikki aikavyöhykkeet maailmanlaajuisesti vastaa aikavälin määrittäminen että markkinointikampanja pienissä erissä. Loppukäyttäjän aikavyöhykkeen avulla voi johtua viiveitä Kampanjat-koska se on aika pyytämisestä puhelimen ennen aloittamista push.

> Huomautus: Kampanjoihin liittyviä ilman päättymispäivän voit tallentaa työntää paikallisesti ja edelleen näyttää ne sen jälkeen, kun manuaalisesti valmis kampanjat. Voit välttää tämän ongelman tietyt Kampanjat päättymisaikaa.

### <a name="see-also"></a>Katso myös
- [Yhteys - miten palvelutyyppi – ajoitus][Link 3] 
 
![Saatavilla Campaign8][27]

### <a name="settings-apply-to"></a>Koskevat asetukset:
- Ajanjakson: ilmoitukset, kyselyiden-ruudut
 
## <a name="test"></a>Testi
Testi-osan avulla voit lähettää tämän push testi-laite ennen tiedoston tallentamista markkinointikampanja. Jos olet määrittänyt kaikki tämän markkinointikampanjan mukautetun kieliä, voit halutessasi testata painallus kullekin kielelle. Voit asennuksen poistaa "Oma tili-testilaitteen.
> Huomautus: Ei tietoja kirjataan, kun "testaaminen"-painikkeen avulla palvelinpuolen Vie, tiedot kirjataan vain reaali push kampanjat.

### <a name="see-also"></a>Katso myös
- [Käyttöliittymän asiakirjat - tilini][Link 14]
 
![Saatavilla Campaign9][28]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

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
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md
 
