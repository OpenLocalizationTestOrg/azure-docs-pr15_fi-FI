<properties 
   pageTitle="Azure Vianmääritysoppaan - SDK Mobile välitys" 
   description="Vianmääritys SDK integrointi ongelmat Azure Mobile välitys" 
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

# <a name="troubleshooting-guide-for-sdk-integration-issues"></a>Oppaan SDK integrointi ongelmien vianmääritys

Seuraavassa on ja miten Azure Mobile välitys integroituu sovellukseen viestejä mahdolliset ongelmat.

## <a name="sdk-issues-discovered-by-a-failure-in-another-area-of-your-application"></a>SDK ongelmat havaitsi virheen sovelluksesi toisen alueen mukaan

### <a name="issue"></a>Ongelma
- Käyttöliittymän tietojen keräämisen virhe (Analytics, seuranta, Segmentointi tai koontinäytön).
- Push-virheet (työntää eivät toimi app-sovelluksen tai molemmat).
- Mukautettu toiminto virheet (seuranta,: n maantieteellinen sijainti tai tietyn työntää eivät toimi ympäristö).
- API virheet (API epäonnistuvat usein äänettömästi ilman virhesanomia).
- Palveluvirheet (ei mitään Azure Mobile välitys toimii sovelluksen).

### <a name="causes"></a>Syitä

- Useimmat ongelmat, jotka on ratkaistava Azure Mobile välitys SDK: ssa ja voi löytää virheen (kuten Käyttöliittymän tietojen kerääminen epäonnistui, push-virhe, lisäominaisuudet-virhe, API-virhe, sovellus kaatuu tai näennäinen palvelun käyttökatkosta)-sovelluksessa.  
- Jos Azure Mobile välitys tietyn ominaisuus on aina ennen-sovelluksen, sinun on suoritettava integrointi. 
- Jos käsiteltävänä Azure Mobile välitys tietyn ominaisuus pysäytetty, joudut ehkä päivittämään viimeiseen Azure Mobile välitys SDK versio. Muista, että on eri versiota Azure Mobile välitys SDK eri ympäristöissä tukemat Azure Mobile välitys (Android, iOS, Windows ja Windows Phone).

#### <a name="sdk-integration"></a>SDK-integrointi

- Azure Mobile välitys integroitu ei ole oikein SDK (Analytics).
- Yhteyttä ei ole oikein integroitu SDK (sovellusten ja poissa App Vie).
- Varmenteen vanhentuneen tai virheelliset tuot ja keskihajonta (vain iOS).
- GCM tai ADM integroitu ei ole oikein SDK (Android vain - palvelun tietyn-Vie).
- Seuranta ei ole oikein integroitu SDK (asennuksen store seuranta).
- Laiskan sijainti tai GPS sijainti ei ole oikein integroitu SDK (Targeting geo sijainnin mukaan).


**Katso myös:**

- [SDK asiakirjat - integroinnin apuviivat][Link 5] 
- [Vianmääritysoppaan - Push][Link 23]

#### <a name="sdk-upgrade"></a>SDK-päivitys

- On päivitettävä SDK ongelmien ratkaisemiseen SDK (usein liittyvät laitteen OS uudempia versioita) aiempien versioiden kanssa.
- Poista kaikki aiemmat versiot sovelluksen laitteestasi ja asenna uusin versio sovelluksesi uudelleen Rekisteröi laite-Tunnuksesi Azure Mobile välitys käyttöliittymässä vahvistamaan, että laitteesi käyttää sovelluksen uusimmassa versiossa.

**Katso myös:**

- [SDK asiakirjat - julkaisutiedot](http://go.microsoft.com/fwlink/?LinkId= 525554) 
- [SDK asiakirjat - päivityksen apuviivat](http://go.microsoft.com/fwlink/?LinkId= 525554)

#### <a name="sdk-other"></a>Muut SDK

- Sovelluksen luettelon "AndroidManifest.xml" virheitä saattaa aiheuttaa Azure Mobile välitys ei, jos haluat tutustua (vain Android).
- SDK-integrointi ja Ohjelmointirajapinnan käyttö yleisiä ongelma on sekoittaa SDK-näppäintä ja Ohjelmointirajapinnan avain.

**Katso myös:**

- [Käsitteitä - sanasto][Link 6]

## <a name="advanced-coding-issues"></a>Lisäasetukset-koodaus ongelmat

### <a name="issue"></a>Ongelma
-  Ympäristön koodin eivät suoranaisesti liity Azure Mobile välitys voi aiheuttaa ongelmia iOS-, Android-ja Windows Phone.

### <a name="causes"></a>Syitä

- Monet advanced Azure Mobile välitys coding ongelmat virheet väärin kirjoitettujen ympäristö koodin eivät suoranaisesti liity Azure Mobile välitys. Tarvitset käyttöohjeista tietyn Platformin ovat kehittäminen lisäksi Azure Mobile välitys asiakirjat (Android, iOS, Web, Windows ja Windows Phone).
- "Luokat" määrittäminen ei ole oikein, estää linkittämisestä ilmoituksen toiseen sijaintiin sisällä tai ulkopuolella app (vain Android). 
- Ei ole tätä asetusta "UIKit.framework" "valinnainen" iOS-koodi näkyy "Symbol ei löydy virhe"-ja/tai kaatuu vanhempia iOS-laitteisiin (vain iOS).
- Varmenteiden vanhentunut tai ei ole oikein käytettäessä varmenteen syitä keskihajonta tai tuot version push ongelmat (vain iOS).
- Luontaisen alustaksi, joka Azure Mobile välitys voi hallita (esimerkiksi miten ulos sovelluksesta toimii system center Vie Android-ja iOS) on joitakin rajoituksia.
- Azure Mobile välitys julkaisee luettelon kaikista Azure Mobile välitys Live v2.0 viittaus käyttää sisäistä paketit. Ota huomioon, että Azure Mobile välitys ominaisuuksia ovat ympäristö (Android, iOS, Web, Windows ja Windows Phone).

### <a name="see-also"></a>Katso myös

 - [Vianmääritysoppaan - Push][Link 23] 
 - [SDK asiakirjat - julkaisutiedot][Link 5]
 - [SDK asiakirjat - päivityksen apuviivat][Link 5]

## <a name="application-crashes"></a>Sovellus kaatuu

### <a name="issue"></a>Ongelma
- Sovellus kaatuu loppukäyttäjän laitteessa.

### <a name="causes"></a>Syitä

- Kaatumisen tietoja voidaan tarkastella *Analytics-Käyttöliittymän* tai *Analytics-Ohjelmointirajapinta*
- Voit etsiä testilaitteen laite-tunnus ja saman toiminnon, joka aiheuttaa sovelluksen kaatumisen, käyttäjä voi auttaa tunnistamaan oman kaatumisen syy.
- Tunnetut ongelmat on Azure Mobile välitys SDK-paketissa, joka aiheuttaa sovellukset voivat kaatua selvitetään joskus päivittämällä SDK uusimman version. Varmista, että tietoja omaa ympäristöäsi julkaisutiedot perittävän kaatuu.

### <a name="see-also"></a>Katso myös

- [SDK asiakirjat - julkaisutiedot][Link 5]
- [SDK asiakirjat - päivityksen apuviivat][Link 5]

## <a name="app-store-upload-failures"></a>App store lataaminen epäonnistuu

### <a name="issue"></a>Ongelma
- Sovelluksen uusimman version lataaminen Apple, Google tai Windows-sovelluskaupasta liittyvien virheiden.

### <a name="causes"></a>Syitä

- App tallentaa joskus estä sovellusten tiettyjä ominaisuuksia, jotka on otettu käyttöön (kuten Apple-kaupasta estää IDFV käyttöä kaupan sovelluksia ja GooglePlay kaupan estää hakemuksen tiedot sovellusten jakaminen). 
- Varmista, että tarkistat ympäristö ja SDK nykyisen version julkaisutiedot, jos sinulla on vaikeuksia sovelluksen lataaminen kauppa.

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/en-us/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/en-us/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/en-us/pricing/details/mobile-engagement/
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
 
