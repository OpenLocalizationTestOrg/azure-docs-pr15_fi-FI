<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - osia" 
   description="Lue, miten voit luoda ja hallita käyttäjien tunnistaa käyttämällä Azure Mobile välitys käyttötavat osia" 
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

# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a>Voit luoda ja hallita käyttäjien tunnistaa käyttötavat osia

Tässä artikkelissa kuvataan **Mobile välitys** -portaalin **osat** -välilehti. **Mobile välitys** -portaalin avulla voit seurata ja hallita mobile-sovellukset.

Käyttöliittymän osat-osan avulla voit käsitellä segmentoinnille käyttäjien eri tavalla ja analytics, joka saa sovelluksesta ja voit myös käyttää osia Ohjelmointirajapinnan perusteella. Osia ensin lasketaan 24 tuntia, kun ne on luotu ja ne ovat recomputed 24 tunnin välein uusimman analytics-tietojen perusteella. Kun segmentin lasketaan, se näyttää "Day päivän historia" kaavion päivittäin.


>[AZURE.NOTE] Monta **Mobile välitys** portaalin Käyttöliittymän osat sisältävät **Näytä Ohje** -painiketta. Paina tätä painiketta osan tilannekohtaiset lisätietoa.

## <a name="create-segments"></a>Luo osia
Voit luoda segmentin enintään 10 ehtojen perusteella tiettynä päivänä määrittäminen 60 päivää aiemman analytics-osiosta. Voit esimerkiksi luoda henkilöt, jotka on tarkastella tiettyjä sivuja tai viimeisten 10 päivän sovelluksen tiettyyn sisältöön etsitään perusteella segmentin. Nämä tiedot ovat käytettävissä analytics-osassa. Näin on voit luoda segmentin ja määritä sitten push ilmoituksen kohdentamisen tämän osan käyttäjien ne palaa sovelluksen saa. 
 
> Huomautus: Kun segmentin on laskettu, sitä ei voi muokata. se vain voidaan monistaa (kopioitu) tai hävitetään (poistettu). Segmentin voi monistaa saman sovelluksesta (kanssa samaan sovelluksen tunnus), ja se voi monistaa myös muiden sovellusten (kanssa eri sovelluksen tunnus). 
 
 ![segments1][35] 

## <a name="examples-segments"></a>Esimerkkejä osia
 ![segments2][36]

Osia mahdollistavat määritetään sovelluksen loppukäyttäjien.
Segmentoinnille käyttäjien on tärkeää markkinoinnin strategia. Azure Mobile välitys voit saada historiatietoihin ja luo mukautettuja osia. Tämä tehokas työkalu mahdollistaa sovelluksen käyttökokemuksen asiakkaiden tietoja. Voit analysoida oman osia ja käytä omaa osia push kohteet.
Yleisiä käyttötapaus on, että haluat lähettää push edistää oman loppukäyttäjien luokittelevan kaupan sovelluksen ilmoituksen. Sen sijaan, että lähettää ilmoituksen kaikkien loppukäyttäjien, voit luoda segmentin, jotka määrittää vain käyttäjät, joilla on käyttää sovelluksen päivittäin viime kuussa ja on ollut erinomainen käyttäjän toimia. Kun lähetät vähemmän, erittäin kohdennettujen push-ilmoitukset, saat paremman ROI.
 
 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a>Voit luoda osia perusteella Azure Mobile välitys tärkeimmät osat:
- Tapahtuman: Luo segmentin kohteet yksi tietyn tapahtuman, sovellus, jossa on tapahtunut enemmän kuin kaksi kertaa viikossa. 
- Istunnon: Luo käyttäjät, jotka ovat käyttäneet sovelluksen enemmän kuin 5 luvulla viime viikolla-osioon.
- Toiminta: Luo käyttäjät, jotka ovat käyttäneet toiselle sivulle tai sisällön enemmän tai vähemmän kuin 10 aika viime kuussa-osioon.
- Työ: Luo käyttäjät, jotka olet tehnyt projektin enemmän kuin kaksi kertaa päivässä-osioon.
- Kaatumisen: Luo kaikki käyttäjät, jotka on ollut yli 10 kertaa viime viikolla kaatumisen-osioon. (Tässä osiossa apology tai jopa kupongin voitu push!)
- Virhe: Luo kaikki käyttäjät, jotka on ollut virheen yli 100 kertaa viimeisen 3 päivän ajalta-osioon.
- Sovelluksen tietoja: Luo segmentin, joka sopii mukautetun sovelluksen-tiedot, jotka on tapahtunut 25 viimeisen päivän aikana.
 
 ![segments4][38]

Osion käyttämisestä on paras tapa on on ostamaasi mukautetun integrointi SDK: n ja tunnisteiden palvelupaketti tunnisteiden "sovelluksen tiedot-sovelluksessa.
Siirry käyttöliittymän aloitussivulla, valitse sovellus ja valitse "Osia"-osassa.

1. Valitse "Osia" osa.
2. "Uuden osion" valitsemalla Luo uusi segmentti-painiketta.

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a>Todellinen aika-Esimerkki: Luoda yksinkertaisen segmentin "Istunnon" tietojen perusteella
Luo kaikki käyttäjät, jotka ovat käyttäneet sovelluksen vähintään 50 kertaa: viime viikolla-osioon. Sieltä Etsi loppukäyttäjien, joka on käytetty vähintään 30 sekuntia sovelluksen istunnossa. Tämä näyttää kaikkien loppukäyttäjien, joilla on positiivinen kokemus-sovelluksen. Sitten luotu segmentti voidaan siirtää ilmoituksen nämä loppukäyttäjien Pyydä korko-kaupan sovelluksen.
 
 ![segments5][39]

1. Anna segmentin nimi, jotta löydät sen nopeasti "Segmentin"-luettelossa.
2. Napsauta "Luo"-painiketta.
 
 ![segments6][40]

Valitse istunto.
 
 ![segments7][41]

1. Valitse aikajakso "Edellisellä viikolla".
2. Valitse Seuraava.
 
 ![segments8][42]

1. Valitse operaattori, jotka vastaavat luettelon kesken: =; ≥, ≤.
2. Kirjoita haluamasi määrä.
3. Valitse haluamasi esiintymä. 
4. Valitse Seuraava.
Tässä esimerkissä on määritetty, jotta edellisen viikon aikana, jotka vastaavat käyttäjät, jotka olet tehnyt vähintään 50 istuntoja.
 
 ![segments9][43]

Istunnon segmentointia voit valita pituus hakuperusteena istunnossa.

1. Valitse operaattori luettelosta.
2. Anna pituus istunnossa.
3. Valitse Seuraava.
Tässä esimerkissä se kertoo, että kaikissa istunnoissa päälle, joka on ollut Segmentoitu esiintymä-osan, valitse vain käyttäjät, jotka on kulunut yli 30 sekuntia istunnossa.
 
 ![segments10][44]

Nimeä oman ehdon noutamista valmis suppilo ja valitsemalla Valmis.
 
 ![segments11][45]

Kun olet tehnyt asetus määrittäminen oman ehtoa, se näkyy segmentin suppilo.
Koska segmentin perustuu analytics-tiedot, kerran päivässä laskettuja osia.
Tässä esimerkissä 47,7 yhteensä loppukäyttäjien prosenttia vastaa ehtoa. Niiden on oltava käyttäjät, joilla on ollut hyvän käyttökokemuksen ja ne ovat todennäköisesti antaa korkeampi luokitus push ne ilmoituksen pyydät heitä luokittelevan app Storessa.


## <a name="see-also"></a>Katso myös

- [Käsitteitä][Link 6]
- [Oppaan palvelun vianmääritys][Link 24]

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md
 
