<properties
 pageTitle="Azure IoT keskittimeen skaalaus | Microsoft Azure"
 description="Tässä artikkelissa käsitellään skaalata Azure IoT toiminnossa."
 services="iot-hub"
 documentationCenter=""
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/19/2016"
 ms.author="elioda"/>

# <a name="scaling-iot-hub"></a>Skaalauksen IoT-toiminnossa

Azure IoT toiminto tukee jopa miljoonaa samanaikaisesti yhdistettyjen laitteiden. Lisätietoja on artikkelissa [IoT keskittimeen hinnat][lnk-pricing]. Kunkin IoT keskittimeen yksikön avulla päivittäinen viestien tietty määrä.

Oikein skaalata ratkaisu, harkitse IoT keskittimeen tietyn käyttöehdot. Huomioi seuraavat luokat-toimintoa varten tarvittavat huippu-siirtonopeuden erityisesti:

* Laitteen cloud viestit
* Cloud laitteen viestit
* Käyttäjätietojen rekisterin toiminnot

Lisäksi siirtonopeuden nämä tiedot [IoT keskittimeen kiintiön][] ja ylikuormitustilan ja suunnitella ratkaisu vastaavasti.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Laitteen cloud ja cloud laitteen viestin siirtonopeuden

Paras tapa kokoa IoT keskittimeen ratkaisu on arvioida yksikössä välein liikenne.

Laitteen cloud viestien näiden ohjeiden kestävä siirtonopeuden.

| Taso | Kestävä siirtonopeuden | Kestävä Lähetä korko |
| ---- | -------------------- | ------------------- |
| S1 | Enintään 1 111 kt/minuutti kohti<br/>(1,5 Gigatavua/päivä/yksikkö) | Keskiarvon 278 viestien/minuutti kohti.<br/>(400,000 viestien/päivä kohti) |
| S2 | 16 Mt/minuutti kohti ylöspäin<br/>(22.8 gt/päivä/yksikkö) | Keskiarvon 4167 viestien/minuutti kohti.<br/>(6 miljoonaa viestien/päivä kohti) |
| S3 | Ylöspäin 814 Mt/minuutti kohti<br/>(1144.4 gt/päivä/yksikkö) | Keskiarvon 208,333 viestien/minuutti kohti.<br/>(300 miljoonaa viestien/päivä kohti) |

## <a name="identity-registry-operation-throughput"></a>Käyttäjätietojen rekisterin toiminnon siirtonopeuden

IoT keskittimeen tunnistetietojen rekisterin toiminnot ovat ei pitäisi olla runtime toimintoja, koska ne liittyvät etupäässä laitteen käyttöönotto.

Katso tietyn purskeen suorituskyvyn numerot [IoT keskittimeen kiintiöiden ja ylikuormitustilan][].

## <a name="sharding"></a>Sharding

Kun yksi IoT-toiminnossa voi skaalata miljoonia laitteita, joskus ratkaisu edellyttää suorituskykyä ominaisuuksia, jotka yhden IoT-toiminnossa voi taata. Tässä tapauksessa on suositeltavaa osion laitteet useita IoT keskittimet kyselyjä. Useita IoT keskittimet tasaiset liikenne bursts ja hanki tarvittavat siirtonopeuden tai toiminnon korvaukset, joita tarvitaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT keskittimeen kiintiöiden ja ylikuormitustilan]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
