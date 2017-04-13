<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - tilini" 
   description="Opi hallitsemaan käyttämällä Azure Mobile välitys tilin profiilin ja testaa laitteet" 
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

# <a name="how-to-manage-your-account-profile-and-test-devices"></a>Miten Hallitse tilin profiilin ja testi
 
Tässä artikkelissa kuvataan ** **Mobile välitys** -portaalin kotisivulla** . **Mobile välitys** -portaalin avulla voit seurata ja hallita mobile-sovellukset. 
 
Voit siirtyä **Oma tili** -sivulle, valitse tili-sivulla.

Käyttöliittymän Oma tili-osa on, missä voit tarkastella ja tilisi, mukaan lukien profiiliasetusten muuttaminen ja testaa laitteen tunnukset. Nämä asetukset sisältävät kohteet, joita voit käyttää myös laitteen Ohjelmointirajapinnan kautta.

![MyAccount1][7]  

## <a name="profile"></a>Profiili:
Voit tarkastella tai muuttaa tiliasetuksia alla. Voit myös tarkastella toisen käyttäjän oikeutta käyttää sovelluksesi- [Home](mobile-engagement-user-interface-home.md)niiden sähköpostiosoitteen perusteella.

![MyAccount2][8]  

## <a name="devices"></a>Laitteet:
Voit tarkastella, lisätä tai poistaa testata laitteen ID's testi-laitteet, jotka voit tehdä testaaminen **tavoittaa** tai **push** kampanjat. Tilannekohtaiset ohjeiden etsiminen eri ympäristöissä laitteiden laitteen tunnus (iOS-, Android-, Windows Phone jne.) näkyvät, kun napsautat "Uuden laitteen". 
 
![MyAccount3][9]  
 
Push-Ohjelmointirajapinnan tai laitteen Ohjelmointirajapinnan on tiedettävä käyttäjien laitteen yksilöllinen tunnus (deviceid-parametri). Voit hakea ne useilla tavoilla:
 
1. Oman taustasta laitteen tunnisteet täydellinen luettelo laitteen Ohjelmointirajapinnan "Get"-toiminnon avulla.
2. Sovelluksen voit käyttää SDK hankkiminen. (Android-kutsua getDeviceID()-funktiota Agent-luokan ja iOS-deviceid-ominaisuus Agent-luokan.)
3. Saatavilla-ilmoitus-ilmoitukseen liittyvän toiminnon URL-osoite sisältää {deviceid} kohdalla, jos se automaattisesti korvataan käynnistävä toiminnon laitteen tunnus.
http://<example>.com/registeruser?deviceid = {deviceid} & otherparam = myparamdata korvataan: http://<example>.com/registeruser?deviceid = XXXXXXXXXXXXXXXX & otherparam = myparamdata 
4. -Reach web-ilmoituksen, jos ilmoituksen HTML-koodi on {deviceid}, kuvio sen automaattisesti korvataan näyttäminen web-ilmoitus laitteen tunnus.
Näin laite-tunniste: {deviceid} korvataan: näin laite-tunniste: XXXXXXXXXXXXXXXX
5.  Avaa sovellus laitteessasi ja suorittaa tapahtuman sovelluksesi, jotka on merkitty.
"Käyttöliittymä - sovelluksen - näyttö - tapahtuma - tiedot", valitse Etsi luettelosta suorittaa tapahtuma.
Valitse näytön tätä tapahtumaa.
Löydät olisi laitteen tunnus-laitteet, jotka olet suorittanut tämän tapahtuman-luettelossa.
Sitten voit kopioida tämä laite-tunnus ja rekisteröi-Käyttöliittymän - Oma tili - laitteet - uusi laite - SELECT laitteen omaa ympäristöäsi".
>(Otettava huomioon, kun IDFA ei ole käytettävissä iOS-laitteen tunnus voi muuttua ajan Jos asennuksen poistaminen ja uudelleenasentaminen sovelluksesi.)

##<a name="troubleshooting-guide"></a>Vianmääritysoppaan käyttäminen
-  [Vianmääritysoppaan - palvelu][Link 24]

## <a name="see-also"></a>Katso myös
-  [Käyttöliittymän asiakirjat - kotisivu][Link 13]


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


 
 
