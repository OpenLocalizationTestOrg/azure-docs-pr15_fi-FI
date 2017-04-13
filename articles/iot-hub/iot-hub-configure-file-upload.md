<properties
     pageTitle="Voit määrittää tiedostojen lataamisen Azure portaalin käyttämällä | Microsoft Azure"
     description="Opit määrittämään tiedostojen lataamisen Azure-portaalissa"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Määritä Azure-portaalissa tiedostojen lataaminen palvelimeen

## <a name="file-upload"></a>Tiedoston lataaminen

Voit käyttää [tiedoston lataaminen toimintoja IoT toiminnossa][lnk-upload], sinun täytyy ensin yhdistää Azure-tallennustilan tilin oman toiminnossa. Valitse **tiedoston lataaminen** -asetuksia näyttämään Lataa ominaisuuksien IoT keskittimeen, muokataan luettelo.

![][13]

**Tallennustilan säilö**: Azure portaalin avulla voit valita Azure-tallennustilan tilin IoT-toiminnossa liitettävä tilauksen nykyisen Azure-blob-säilö. Tarvittaessa voit luoda Azure-tallennustilan tilin **tallennustilan tilit** sivu ja Blob-objektien säilö- **säilöt** -sivu. IoT toiminto luo SAS URI automaattisesti blob-säilö laitteita, jota käytetään, kun ne ladata tiedostoja kirjoitusoikeudet.

![][14]

**Vastaanota ilmoituksia ladattuja tiedostoja**: ottaminen käyttöön tai poistaminen käytöstä tiedoston latauksen odotussanomat sivulla kautta.

**SAS TTL**: Tämä asetus on--elinaika IoT keskittimeen laitteeseen palauttama SAS URI-osoitteita. Määrittää tunnin oletusarvoisesti, mutta voi mukauttaa muita arvoja liukusäätimellä.

**Tiedoston ilmoitus asetukset default TTL**:--elinaika-tiedoston lataaminen ilmoitus, ennen kuin se on vanhentunut. Määrittää oletusarvon mukaan yksi päivä, mutta voi mukauttaa muita arvoja liukusäätimellä.

**Tiedoston ilmoituksen suurin toimituksen Laske**: kuinka monta kertaa IoT-toiminnossa yrittää toimittaa tiedoston lataaminen ilmoitus. Määrittää 10 oletusarvoisesti, mutta voi mukauttaa muita arvoja liukusäätimellä.

![][15]

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja IoT keskittimeen tiedoston lataaminen-ominaisuuksia [laitteesta tiedostojen lataaminen] [ lnk-upload] developer-oppaassa.

Saat lisätietoja hallinnasta Azure IoT keskittimeen seuraavista linkeistä:

- [Bulk IoT laitteiden hallinta][lnk-bulk]
- [Käyttö-arvot][lnk-metrics]
- [Toimintojen seuranta][lnk-monitor]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]
- [Suojattu IoT ratkaisu alkava määrittäminen][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md