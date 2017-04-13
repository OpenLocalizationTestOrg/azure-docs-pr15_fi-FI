<properties 
   pageTitle="Azure Mobile välitys käyttöliittymä - Reach sisältö" 
   description="Opi hallitsemaan yksilöllistä sisältöä erityyppisiä push ilmoituksen Kampanjat-Azure Mobile välitys" 
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

# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a>Push-ilmoituksen Kampanjat erityyppiset yksilöllinen sisällön hallinta
 
Uusi reach markkinointikampanjan sisältö-osassa voit muokata sisältöä ilmoitukset, kyselyiden, Vie tiedot, ja ruudut (vain Windows Phone). Push-Kampanjat sisällön asetus on markkinointikampanjan tyyppi. 
 
### <a name="content-types"></a>Sisältötyypit:
- Ilmoitukset
- Kyselyiden
- Tietoja työntää
- Ruutujen (vain Windows Phone)
 
## <a name="content-of-announcements"></a>Sisällön ilmoitukset
 ![Saatavilla Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a>Valitse ilmoituksessa tyyppi:
-    Vain ilmoituksen: on yksinkertainen vakio ilmoituksen. Tarkoittaa, että, jos käyttäjä valitsee sen, ei ole lisänäkymät näkyvät, mutta vain siihen liittyvä toiminto suoritetaan.
-    Tekstin ilmoitus: ilmoitus, joka käyttää käyttäjä voi tarkastella tekstin näkymän on.
-    Web-ilmoitus: ilmoitus, joka käyttää käyttäjä voi tarkastella web-näkymä on.

### <a name="see-also"></a>Katso myös
- [Yhteys - miten ohjeet - ilmoitukset][Link 3] 

### <a name="about-web-view-announcements"></a>Tietoja Web Näytä ilmoitukset:
HTML-koodia tai JavaScript-koodia antaisit seuraavassa kuviossa "{deviceid}" esiintymät korvataan automaattisesti laite näyttää ilmoituksen tunnus. Tämä on helposti hakea Azure Mobile välitys laitteen tunnisteista ylläpitämä takaisin Office ulkoisen web-palvelu.
Jos haluat luoda koko näytön web-näkymä (ilman oletusarvo-toiminto ja Lopeta painikkeet annamme) voit käyttää web näkymän ilmoituksessa 's JavaScript-koodia-seuraavat toiminnot: 

-    ilmoitus-toiminto: ReachContent.actionContent()
-    Lopeta ilmoitus: ReachContent.exitContent()
 
### <a name="choose-your-action"></a>Valitse haluamasi toiminto:

### <a name="about-action-urls"></a>Tietoja toiminnon URL-osoitteet:
URL-Osoitteen, joka voidaan tulkita kohdennettujen laitteen käyttöjärjestelmä voidaan käyttää toiminnon URL-osoite.
Jokin omistautunut sovelluksen ehkä tue URL-osoite (esimerkiksi haluat siirtyä tiettyyn näyttöön) voi käyttää myös toiminnon URL-osoite.
{Deviceid} kuvion jokaiselle esiintymälle korvataan automaattisesti sen toiminnon laitteen tunnus. Tämä voidaan hakea helposti Azure Mobile välitys laitteen tunnisteet isännöimät takaisin office ulkoisen verkkopalvelun kautta.

- **Android + iOS toiminnot**
    - Avaa verkkosivu
    - http://\[web-sivuston-toimialue\] 
    - Esimerkki: http://www.azure.com
    - Sähköpostin lähettäminen
    - mailto:\[riskille-sähköpostin vastaanottajan\]?subject =\[aihe\]& leipätekstin =\[viesti\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Tekstiviestien
    - SMS:\[puhelinnumero\] 
    - Esimerkki: sms:2125551212
    - Puhelinnumero
    - TEL:\[puhelinnumero\] 
    - Esimerkki: tel:2125551212
- **Android vain Toiminnot**
    - Lataa sovellus Napsauta Play-kaupasta
    - Market://details?id=\[sovelluspaketin\] 
    - Esimerkki: market://details?id=com.microsoft.office.word
    - Aloittaa geo lokalisoitu
    - GEO:0, 0?q =\[hakukyselyn\] 
    - Esimerkki: geo:0, 0? q = starbucks Pariisi
- **iOS vain Toiminnot**
    - Lataa App Store-sovellukseen
    - http://iTunes.Apple.com/ [Maa] /app/ [sovelluksen nimi] /id [sovellustunnus]? mt = 8 
    - Esimerkki: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8
    - Windows-toiminnot
    - Avaa verkkosivu
    - http://\[web-sivuston-toimialue\] 
    - Esimerkki: http://www.azure.com
    - Sähköpostin lähettäminen
    - mailto:\[riskille-sähköpostin vastaanottajan\]?subject =\[aihe\]& leipätekstin =\[viesti\] 
    - Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!
    - Tekstiviestien (Skype-kaupan sovellus pakollinen)
    - SMS:\[puhelinnumero\] 
    - Esimerkki: sms:2125551212
    - Valitse puhelinnumero (Skype-kaupan sovellus pakollinen)
    - TEL:\[puhelinnumero\] 
    - Esimerkki: tel:2125551212
    - Lataa sovellus Napsauta Play-kaupasta
    - MS-windows-kaupan: PDP?PFN =\[paketin Sovellustunnus\] 
    - Esimerkki: ms-windows-kaupan: PDP? PFN = 4d91298a 07cb-40fb-aecc-4cb5615d53c1
    - Aloita bingmaps haku
    - bingmaps:?q =\[hakukyselyn\] 
    - Esimerkki: bingmaps:? q = starbucks Pariisi
    - Oman mallin avulla
    - \[mukautetun värimallin\]://\[mukautetun värimallin parametrit\] 
    - Esimerkki: myCustomProtocol://myCustomParams
    - Käytä paketin tiedot (kaupan sovellus tunnisteen luku pakollinen)
    - \[kansion\]\[tietojen\]. \[tunniste\] 
    - Example:myfolderdata.txt
 
### <a name="build-a-tracking-url"></a>Laadi seuranta URL-osoite:
-    "Asetukset" osiosta, <UI Documentation> varten oikeista rakentaminen seuranta URL-Osoitetta, joka Salli käyttäjien ladata yhtä muista sovelluksista.
 
### <a name="define-the-texts-of-your-announcement"></a>Määritä ilmoituksessa tekstit
Täytä otsikko, sisältö ja painikkeen tekstit ilmoituksessa. Voit kohdistaa käyttäjäryhmän tulevien markkinointikampanjan, joka on saatavilla, kuinka käyttäjät vastannut tämän markkinointikampanjan palautteen perusteella. Käyttäjäryhmälle kohdistamisen avulla voi perustua, onko tämä markkinointikampanjan vain ansiosta, Vastattujen, actioned tai poistuneet palautetta.

### <a name="see-also"></a>Katso myös
- [Käyttöliittymän asiakirjat - Reach - uusi Push vertailuarvo][Link 28]

## <a name="content-of-polls"></a>Kyselyiden sisällöstä
![Saatavilla Content2][31] täytä otsikko, kuvaus ja painikkeen tekstit ilmoituksessa. Lisää sitten kysymyksiä ja vastauksia kysymyksiisi valinnat.
Voit kohdistaa käyttäjäryhmän tulevien markkinointikampanjan, joka on saatavilla, kuinka käyttäjät vastannut tämän markkinointikampanjan palautteen perusteella. Onko tämä markkinointikampanjan vain ansiosta, Vastattujen, actioned tai poistuneet voi perustua Käyttäjäryhmälle kohdistamisen avulla. Käyttäjäryhmälle kohdistamisen avulla voit myös perusteella kyselyn vastaus palautetta, kysymyksiä ja vastauksia valinta käytettyjen ehtoina.

### <a name="see-also"></a>Katso myös
- [Käyttöliittymän asiakirjat - Reach - uusi Push vertailuarvo][Link 28]
 
## <a name="content-of-data-pushes"></a>Vie tiedot sisällöstä
![Saatavilla Content3][32] 

### <a name="choose-the-type-of-your-data"></a>Valitse haluamasi tiedot:
- Teksti
- Binaaritietoja
- Base64 tietoja

### <a name="define-the-content-of-your-data"></a>Määritä tietojen sisältö
- Jos olet valinnut push tekstitietoja, kopioi ja liitä teksti "sisältö"-kenttään.
- Jos olet valinnut push binaarinen tai base64 tietoja, Lataa tiedosto "Lataa tiedosto"-painikkeen avulla.
- Voit kohdistaa käyttäjäryhmän tulevien markkinointikampanjan, joka on saatavilla, kuinka käyttäjät vastannut tämän markkinointikampanjan palautteen perusteella. Onko tämä markkinointikampanjan vain ansiosta, Vastattujen, actioned tai poistuneet voi perustua Käyttäjäryhmälle kohdistamisen avulla.

### <a name="see-also"></a>Katso myös
- [Käyttöliittymän asiakirjat - Reach - uusi Push vertailuarvo][Link 28]

## <a name="content-of-tiles-windows-phone-only"></a>Sisällön ruutujen (vain Windows Phone)
![Saatavilla Content4][33]

### <a name="define-the-content-of-your-tile"></a>Määrittää, että ruutu sisällön
Ruutu-sisältö on Windows Phone-laitteissa sovelluksen ruudun näytettävä teksti.
Ruutu-push on alkuperäisen push for Windows Phone Microsoftin Push ilmoituksen Service (MPNS) versio. Push ruututyyppi on push ainoa, joka ei ole vastauksen ja siten tulevia markkinointikampanjoita yleisön ei voi muodostaa ruutu push-markkinointikampanjan tuloksiin. 

### <a name="see-also"></a>Katso myös
- [API asiakirjat - Reach API - alkuperäisen Push][Link 4]

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
 
