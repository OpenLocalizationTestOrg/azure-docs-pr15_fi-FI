<properties 
    pageTitle="Yleinen Windows-sovellusten SDK sisältö" 
    description="Lisätietoja Windows yleinen Apps SDK sisältöä Azure Mobile välitys"                    
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="windows-universal-apps-sdk-content"></a>Yleinen Windows-sovellusten SDK sisältö

Tämän asiakirjan luettelo ja kuvaukset SDK-sovelluksen käyttöön sisältöä.

##<a name="the-resources-folder"></a>`/Resources` Kansio

Tämä kansio sisältää kaikki Mobile välitys tarvitsemat resurssit. Voit mukauttaa ne sovelluksen sopivaksi.

- `EngagementConfiguration.xml`Matkapuhelin: välitys kokoonpanotiedosto tällä, jossa voit mukauttaa Mobile välitys asetukset (Mobile välitys yhteysmerkkijonon, raportin kaatumisen...).

### <a name="html-folder"></a>/HTML-kansio

- `EngagementNotification.html`: Mitä `Notification` web-sovelluksen nauhan Näytä html rakenne.

- `EngagementAnnouncement.html`: Mitä `Announcement` web-näkymän html-rakenne-app kudosten näkymissä.

### <a name="images-folder"></a>/Images-kansio

- `EngagementIconNotification.png`Vasemmalla puolella ilmoituksen: brändiä-kuvake korvaa tämän yksi merkki-kuvakkeen mukaan.

- `EngagementIconOk.png`: Mitä `Ok` reach sisältösivuja toiminnon tai vahvistus-painikkeen kuvake.

- `EngagementIconNOK.png`: Mitä `NOK` kuvake käytetään, kun reach sisältösivuja vahvistus-painike ei ole käytössä.
 
- `EngagementIconClose.png`: Mitä `Close` reach ilmoitukset ja Hylkää-painikkeen sisällysluettelo-kuvake.

### <a name="overlay-folder"></a>/overlay-kansio

- `EngagementPageOverlay.cs`:-Sivu vastuussa lisääminen välitys saavuttaa alatason sovelluskohtaisesta-Käyttöliittymä.
  
