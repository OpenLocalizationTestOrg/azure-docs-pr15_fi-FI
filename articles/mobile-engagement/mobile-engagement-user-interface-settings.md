<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - asetukset" 
   description="Opi hallitsemaan Yleiset asetukset-sovelluksen käyttäminen Azure Mobile välitys" 
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

# <a name="how-to-manage-the-global-settings-of-your-application"></a>Sovelluksen yleisten asetusten hallinta

**Asetukset** -valikon vaihtoehdot käytettävissä sovelluksen-vaihtelevat sen mukaan, sovellus ja olet saanut-sovelluksen käyttöoikeudet ympäristö. Seuraavat asetukset: tiedot, projektien, alkuperäisen Push, Push nopeutta, tunniste (sovelluksen tiedot) ja kaupallisen paine. Tunniste (sovelluksen tiedot)-valikkovaihtoehdon asetukset-osan voi hallita sovelluksesi (joko SDK) tai oman Taustajärjestelmä (laitteen Ohjelmointirajapinnan käyttäminen). 


>[AZURE.NOTE] Monta **Mobile välitys** -portaalin Käyttöliittymän osat sisältävät **Näytä Ohje** -painiketta. Paina tätä painiketta osan tilannekohtaiset lisätietoa.

## <a name="details"></a>Tiedot

Voit muuttaa nimi ja kuvaus-sovelluksen Näytä sovelluksesi ja roolin käyttöoikeuksia omistaja. 

Analytics määritysten avulla voit tarkastella tai muuttaa päivän, viikon käynnistäminen ja säilytys aika päivää.
 
  ![settings1][46]
 
## <a name="projects"></a>Projektit

Voit valita haluat sovelluksen näkyvän kaikissa projekteissa. 

Voit etsiä projektin ja tarkastella nimi, kuvaus, omistaja ja rooli mitkä tahansa projektin sovelluksen kuuluu.

Lisätietoja on artikkelissa: [Käyttöliittymän asiakirjat – Home][Link 13]
 
  ![settings3][48]

## <a name="native-push"></a>Alkuperäisen Push

Voit rekisteröidä alkuperäisen push uutta varmennetta tai poista ja varmenne käyttöä varten. Alkuperäisen Push mahdollistaa Azure Mobile varauksia push-sovelluksen milloin tahansa, myös kun se ei ole käynnissä. 

Kun olet antanut tunnistetiedot tai varmenteet vähintään yksi alkuperäisen Push-palveluun, voit valita "Aina" luotaessa saavuttaa kampanjat ja käytä "ilmoitus"-parametrin PUSH-Ohjelmointirajapinnan.



### <a name="apple-push-notification-service-apns"></a>Apple Push-ilmoituspalvelu (APN)

Voit ottaa käyttöön alkuperäisen Push Apple Push-Ilmoituspalvelun haluat rekisteröidä sertifikaatin avulla. Sinun on voit määrittää varmenteen kehittäminen (keskihajonta) tai tuotannon (tuot). Sinun on lataa sitten varmennetta ja salasana.

Lisätietoja on artikkelissa: [SDK asiakirjat - iOS - valmistelemaan Apple Push-ilmoitukset sovelluksen][Link 5]
 
![settings4][49]
 
### <a name="windows-push-notification-service-wpns"></a>Windowsin Push-ilmoituspalvelu (WPNS)

Alkuperäisen Push käyttämällä Windows-ilmoituspalvelu käyttöön, sinun on annettava sovelluksen tunnistetiedot. Sinun on paketin suojaustunnuksen (SUOJAUSTUNNUS) ja että salausavaimen.
 
![settings5][50]
 
### <a name="google-cloud-messaging-for-android-gcm"></a>Google Cloud Messaging for Android (GCM)

Alkuperäisen Push käyttämällä GCM käyttöön haluat Google ohjeita. Valitse sinun on liitettävä palvelimen yksinkertainen Ohjelmointirajapinnan avain, määritetty ilman IP-rajoituksia. Vaatii SDK integrointi Android v1.12.0 +.

Lisätietoja on artikkelissa: 

- [SDK dokumentaatio Android miten voit integroida GCM][Link 5]
- [Google-Developer GCM opas](http://developer.android.com/guide/google/gcm/gs.html)
 
### <a name="amazon-device-messaging-for-android-adm"></a>Amazon laitteen Messaging for Android (ADM)

Alkuperäisen Push käyttämällä ADM käyttöön sinun on määritettävä Amazon <OAuth credentials> Asiakastunnus ja asiakkaan salaisuus (edellyttää SDK integrointi Android v2.1.0 +).

Lisätietoja on artikkelissa: 

- [SDK dokumentaatio Android miten voit integroida ADM][Link 5]
- [Amazon Developer ADM dokumentaatio](https://developer.amazon.com/sdk/adm/credentials.html#Getting)
 
![settings6][51]

## <a name="push-speed"></a>Push-nopeus

Näyttää nykyisen sovelluksesi push nopeuden ja voit määrittää sovelluksen push nopeuden.
 
  ![settings7][52]

## <a name="tag-app-info"></a>Tunniste (sovelluksen tiedot)

![settings11][56]
  
## <a name="commercial-pressure"></a>Kaupallinen paine


![settings12][57]


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
 
