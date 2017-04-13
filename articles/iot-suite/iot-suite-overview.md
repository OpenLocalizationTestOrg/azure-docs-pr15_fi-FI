<properties
    pageTitle="Microsoft Azure IoT Suite yleiskatsaus | Microsoft Azure"
    description="Opit Azure IoT Suite toimittaa asioita esimääritetyt ratkaisut Internetiin kerätä, analysoida, ja tallentaa tiedot, anna visualisoinnit ja muista järjestelmistä integroida."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Mikä on Azure IoT Suite?

Azure internet asioita (IoT) palvelujen tarjoavat laajan valikoiman ominaisuuksia. Yrityksen arvosana palveluista, joiden avulla voit:

- Tietojen kerääminen laitteilla
- Analysoi tietoja virtaa liikkeessä
- Tallentaa ja suurten tietojoukkojen kysely
- Reaaliaikainen- ja historiallisten tietojen visualisointi
- Integroi takaisin office-järjestelmiin

Pitää näitä ominaisuuksia, Azure IoT Suite pakettien yhdessä Azure palveluihin on mukautettu kuin *esimääritetyt ratkaisut*. Esimääritetyt ratkaisut ovat Yleiset IoT ratkaisu mallit, jotka voit vähentää ajan tallentumaan IoT ratkaisuja perusosoitteen käyttöotot. Käyttämällä [IoT software development Kit][lnk-sdks], voit mukauttaa ja laajentaa vastaamaan omia tarpeita seuraavia ratkaisuja. Voit käyttää myös seuraavia ratkaisuja esimerkkejä tai malleja kun kehittävät uusia IoT ratkaisuja.

Seuraavassa videossa esitellään Azure IoT ohjelmistopakettiin:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT Services-palvelujen Azure IoT Suite

Esimääritetyt ratkaisut käyttävät yleensä seuraavat palvelut:

- Azure IoT ohjelmistopakettiin Core on [Azure IoT keskittimeen] [ lnk-iot-hub] palvelu. Tämä palvelu sisältää laitteen cloud ja cloud laitteen sähköpostiviestinnän toimintoja ja toimii yhdyskäytävä pilveen ja muut avaimen IoT Suite-palvelut. Palvelun avulla voit vastaanottaa viestejä tasolla laitteet ja lähettää komentoja laitteet.

- [Azure Stream Analytics] [ lnk-asa] sisältää liikkeessä Tietoanalyysi. IoT Suite hyödyntää tämän palvelun käsittelemään saapuvia telemetriatietojen, yhdistämisen suorittaminen ja tunnistaa tapahtumat. Esimääritetyt ratkaisut käyttää myös stream analytics Käsittele tiedoksi viestejä, jotka sisältävät tietoja, kuten metatietojen tai komentoon vastaukset laitteilla. Ratkaisuja käyttää Stream Analytics käsitellä laitteilla viestit sekä viestit muihin palveluihin.

- [Azure-tallennustilan] [ lnk-azure-storage] ja [Azure DocumentDB] [ lnk-document-db] antaa tietoja tallennustilan ominaisuuksia. Esimääritetyt ratkaisut avulla Blob-objektien tallennustilaan tallentamiseen telemetriatietojen ja vapauta se analyysi. Ratkaisuja käyttää DocumentDB tallentamiseen laitteen metatiedot ja laitteen hallinta käyttämiseksi ratkaisuista.

- [Azure verkkosovelluksissa] [ lnk-web-apps] ja [Microsoft Power BI] [ lnk-power-bi] tarjota tietojen visualisoinnin ominaisuuksia. Power BI joustavuutta avulla voit nopeasti luoda omia vuorovaikutteiset raporttinäkymät IoT Suite tietoja käyttävät.

Yleiskuvaus tyypillinen IoT-ratkaisun arkkitehtuuri-kohdassa [Microsoft Azure ja asioita Internet (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Esimääritetyt ratkaisut

IoT Suite sisältää esimääritettyjä ratkaisuja, joiden avulla voit nopeasti alkuun ja tutustu yleisiä IoT skenaarioita, kuten *Remote seuranta* ja *ennakoivan ylläpito*, joka mahdollistaa Azure IoT Suite. Voit näitä ratkaisuja käyttöön Azure-tilauksesi ja suorita sitten IoT tilanne määritetty, lopusta loppuun.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet luonut, mitä voit tehdä IoT Suite ja mitä sen pääosaa, voit lisätietoja esimääritetyt ratkaisut IoT-ohjelmalla tehdystä tiedostosta, katso [mitä Azure IoT valmiiksi ratkaisuja?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
