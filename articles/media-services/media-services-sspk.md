<properties 
    pageTitle="Käyttöoikeuksien myöntämistä Microsoft® tasainen Streaming asiakkaan sovittaminen Kit" 
    description="Lisätietoja on artikkelissa käyttöoikeuksien Microsoft® tasainen Streaming asiakkaan sovittaminen Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Käyttöoikeuksien myöntämistä Microsoft® tasainen Streaming asiakkaan sovittaminen Kit

##<a name="overview"></a>Yleiskatsaus

Microsoft tasainen Streaming asiakkaan sovittaminen Kit (**SSPK** käyttämistä lyhyen) on tasainen Streaming asiakkaan toteutus, joka on optimoitu upotetun laitteen valmistajan, kaapeli ja matkapuhelinoperaattorit sisällön palveluntarjoajien, luuri valmistajat, (vertaiskäyttäjien) ja palveluntarjoajat luominen tuotteista ja palveluista, streaming mukautuvat streaming sisällön tasainen Streaming-muodossa. SSPK on laite ja ympäristö riippumaton toteutus tasainen Streaming asiakkaan, minkä tahansa laitteen ja ympäristö käyttöoikeuden voit siirtämisen. 

Alla on korkean tason arkkitehtuuri ja IIS tasainen Streaming sovittaminen Kit-ruutu on Microsoftin tasainen Streaming asiakkaan käyttöönoton ja sisältää kaikki core tasainen mediavirtaa toistoa logiikan. Tämä on sitten siirtämisen mukaan kumppanit laitetta tai ympäristö käyttämällä tarvittavat liittymät. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Kuvaus

SSPK käyttöoikeus ehdot, jotka tarjoavat erinomainen liiketoiminta-arvon. SSPK-käyttöoikeus sisältää teollisuuden kanssa:

- Tasainen c++ Streaming sovittaminen Kit lähde 
  - toteuttaa tasainen Streaming asiakkaan toiminnot
  - Lisää muoto jäsennyksen heuristiikkaa, puskurointi logiikan jne.
- Toisto-ohjelmaa API 
  - ohjelmoinnin liittymät vuorovaikutuksen media player-sovelluksen kanssa
- Ympäristön otetaan kerroksen (PAL)-käyttöliittymä 
  - ohjelmoinnin liittymät käsittelemisen käyttöjärjestelmän (viestiketjuja, sockets)
- Laitteiston otetaan tason (Layer)-käyttöliittymä 
  - Ohjelmointi liittymät käsittelemisen laitteiston A / V dekoodereita (koodauksen, värien)
- Digitaalisten oikeuksien hallinta (DRM)-käyttöliittymä 
  - ohjelmoinnin liittymät käsittelyyn DRM DRM otetaan kerroksen (Domain) kautta
  - Microsoft PlayReady sovittaminen Kit osana erikseen, mutta integroi liittymän kautta. Lisätietoja Microsoft PlayReady Device käyttöoikeuksista saat napsauttamalla [tätä](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Käyttöönotto-objektit 
  - Esimerkki PAL käyttöönoton Linux
  - Esimerkki HAL käyttöönoton GStreamer

##<a name="licensing-options"></a>Käyttöoikeusvaihtoehdoista

Microsoft tasainen Streaming asiakkaan sovittaminen Kit saataville asiakirjamalleja kaksi eri käyttöoikeuden sopimusten: yksi tasainen Streaming väliaikaisen-asiakasohjelman tuotteet ja toiseen jakamiseksi tasainen Streaming asiakkaan lopullinen tuotteiden käyttäjille.
 
- Piirisarjan valmistajat, järjestelmä muutetaan tai riippumattomat toimittajat jotka tarvitsevat sovittaminen kit voit tehdä väliaikaisen tuotteiden lähdekoodi on suoritettava Microsoft tasainen Streaming asiakkaan sovittaminen Kit **Väliaikaisen tuotteen käyttöoikeuden** .
- Laitteen valmistajan tai valmistajille, jotka tarvitsevat jakauman oikeuksien tasainen Streaming asiakkaan lopullinen tuotteiden käyttäjille on suoritettava Microsoft tasainen Streaming asiakkaan sovittaminen Kit **Lopullisen tuotteen käyttöoikeuden** .

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft tasainen Streaming asiakkaan sovittaminen Kit-kohdan tuotteen käyttöoikeuden

Microsoft tarjoaa käyttöoikeus, valitse tasainen Streaming asiakkaan sovittaminen paketin ja tarvittavat tekijänoikeudet kehittää ja jakaa muiden tasainen Streaming asiakkaan sovittaminen Kit-laitteen asiakirjamalleja, joka jakaa tasainen Streaming asiakkaan lopullinen tuotteiden tasainen Streaming väliaikaisen-asiakasohjelman tuotteet.

####<a name="fee-structure"></a>Maksu-rakenne

Yhdysvaltain 50 000 erikseen käyttöoikeuden maksu tarjoaa sujuvan Streaming asiakkaan sovittaminen pakettiin. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft tasainen Streaming asiakkaan sovittaminen Kit lopullinen tuotteen käyttöoikeuden

Microsoft tarjoaa käyttöoikeus, valitse kaikki tarvittavat immateriaalioikeuksia vastaanottaa muiden tasainen Streaming asiakkaan sovittaminen Kit-asiakirjamalleja tasainen Streaming väliaikaisen-asiakkaan tuotteita ja jakaa yrityksen mukautettu tasainen Streaming asiakkaan lopullinen tuotteiden käyttäjille.

####<a name="fee-structure"></a>Maksu-rakenne

Tasainen Streaming asiakkaan lopullisen tuotteen tarjotaan royalty mallin, valitse:

- 0,10 laitteen käyttöönoton toimitettu kohden
- Royalty on enimmillään 50 000 vuosittain
- Ei ole royalty ensimmäiset 10 000 laitteen käyttöotot vuosittain varten 

##<a name="licensing-procedure-and-sspk-access"></a>Käyttöoikeuksien myöntämistä toimintosarja ja SSPK käyttö

Kirjoita sähköpostin [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) kaikki käyttöoikeudet kyselyt.

[SSPK jakauman portal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) voivat käyttää rekisteröidyn väliaikaisen asiakirjamalleja.

Väliaikaisen ja lopullisen SSPK asiakirjamalleja voivat lähettää teknisiä kysymyksiä [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft tasainen Streaming asiakkaan väliaikaisen tuotteen sopimuksen asiakirjamalleja

- Adroit Business ratkaisuja, Inc.
- Lisäasetukset digitaalinen lähetyksen SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret tehoainetta mehiläistä
- Albis Technologies Ltd.
- Alticast Corporation
- Amazon digitaalinen Services, Inc.
- AVC Multimedia ohjelmiston Co., Ltd.
- Cavium Inc.
- EchoStar ostamalla Corporation
- Enseo Inc.
- Etelä-Amerikan Fluendo
- HANDAN BroadInfoCom Co., Ltd.
- Infomir GMBH
- Irdeto USA Inc.
- Liberty yleinen Services BV
- MediaTek Inc.
- MStar Mää Ltd
- Nintendo Co., Ltd.
- OpenTV Inc.
- Sahrami digitaalinen rajoitettu
- Sichuan Changhong sähköinen Co. Ltd
- SoftAtHome
- Sony Corporation
- Tatung tekniikka Inc.
- TCL Technoly Electronics (Huizhou) Co., Ltd.
- Poista Vestel Elektronik Sanayi Ticaret tehoainetta mehiläistä
- VisualOn Inc.
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft tasainen Streaming asiakkaan lopullisen tuotteen sopimuksen asiakirjamalleja

- Lisäasetukset digitaalinen lähetyksen SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret tehoainetta mehiläistä
- Albis Technologies Ltd.
- Amazon digitaalinen Services, Inc.
- AmTRAN tekniikka Co., Ltd.
- Arcadyan tekniikka Corporation
- ATMACA HAAVOITTUVUUDESTA, JOKA ELEKTRONİK SAN. POISTA TİC. A.Ş
- Brittiläinen Sky lähettämisestä rajoitettu
- CastPal tekniikka Inc. Shenzhen
- Compal Electronics Inc.
- Dongguan digitaalinen AV tekniikka Corporation, Ltd.
- EchoStar ostamalla Corporation
- Enseo Inc.
- Filmflex elokuvat rajoitettu
- Etelä-Amerikan Fluendo
- Gibson innovaatiot rajoitettu
- Haier tiedot Applicantion S.R.L
- HANDAN BroadInfoCom Co., Ltd.
- Homecast Co. Ltd
- Hon Hai tarkkuus alan Co., Ltd.
- Infomir GMBH
- Kaonmedia Co., Ltd.
- KDDI Corporation
- Nintendo Co., Ltd.
- Oranssi SA
- Sahrami digitaalinen rajoitettu
- Sagemcom laajakaista Suojaussidokset
- Shenzhen Coship Electronics CO. LTD
- Shenzhen Jiuzhou sähköinen Co. Ltd
- Shenzhen Skyworth digitaalinen tekniikka Co., Ltd
- Sichuan Changhong sähköinen Co., Ltd.
- Skardin teollisuus Corporation
- Sky Saksa Fernsehen GmbH & Co. KG
- Etelä-Amerikan SmarDTV
- SoftAtHome
- Sony Corporation
- Rajoitettu TCL merentakaisten markkinointi (Macao kaupallista Offshore)
- Technicolor toimituksen Technologies Suojaussidokset
- Tongfang yleinen Ltd.
- Toshiba elämäntapa-tuotteiden ja palvelujen Corporation
- Yleinen median Corporation /Slovakia/ s.r.o.
- VIZIO Inc.
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
