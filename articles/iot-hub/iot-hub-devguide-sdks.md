<properties
 pageTitle="Sovelluskehittäjän opas - IoT keskittimeen SDK: T | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - tietoja ja linkkejä eri Azure IoT keskittimeen laite- ja palveluluettelon SDK: T."
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016"
 ms.author="dobett"/>

# <a name="iot-hub-sdks"></a>IoT keskittimeen SDK: T

## <a name="iot-hub-device-sdks"></a>Laitteen IoT keskittimeen SDK: T

Microsoft Azure IoT laitteen SDK: T sisältää koodia, joka helpottaa rakennuksen laitteita ja sovelluksia, jotka yhdistäminen ja hallitaan Azure IoT keskittimeen services.

Seuraavan IoT laitteen SDK: T ovat käytettävissä GitHub ladataan:

- [Azure IoT laitteen SDK c] [ lnk-c-device-sdk] kirjoitettu siirrettävyys- ja suurin piirtein ympäristö yhteensopivuuden ANSI c (C99).
- [Azure IoT laitteen SDK .NET][lnk-dotnet-device-sdk]
- [Azure IoT laitteen SDK Java][lnk-java-device-sdk]
- [Azure IoT laitteen SDK Node.js][lnk-node-device-sdk]
- [Microsoft Azure IoT laitteen SDK Python 2.7 varten][lnk-python-device-sdk]

> [AZURE.NOTE] Katso lisätietoja käyttämisestä kieli- ja käyttöjärjestelmäkohtaiset paketin valvojat asentamalla binaaritiedostot ja riippuvuuksien kehittäminen käyttämääsi laitteeseen GitHub säilöjen tietoihin Lueminut-tiedostot.

## <a name="os-platforms-and-hardware-compatibility"></a>OS alustojen ja laitteiston yhteensopivuus

Saat lisätietoja SDK yhteensopivuus-laitteiden [Azure sertifioitu IoT laitteen luettelon][lnk-certified].

## <a name="iot-hub-service-sdks"></a>IoT keskittimeen palvelun SDK: T

Microsoft Azure IoT palvelun SDK: T sisältää koodia, joka helpottaa rakennuksen sovelluksista, jotka toimivat kanssa suoraan IoT keskittimeen, laitteet ja turvallisuuden hallintaan.

Seuraavan IoT palvelun SDK: T ovat käytettävissä GitHub ladataan:

- [Azure IoT palvelu SDK .NET][lnk-dotnet-service-sdk]
- [Azure IoT palvelu SDK Node.js][lnk-node-service-sdk]
- [Azure IoT palvelu SDK Java][lnk-java-service-sdk]

> [AZURE.NOTE] Katso lisätietoja käyttämisestä kieli- ja käyttöjärjestelmäkohtaiset paketin valvojat asentamalla binaaritiedostot ja riippuvuuksien kehittäminen käyttämääsi laitteeseen GitHub säilöjen tietoihin Lueminut-tiedostot.

## <a name="azure-iot-gateway-sdk"></a>Azure IoT yhdyskäytävän SDK-paketissa

Azure IoT yhdyskäytävän SDK sisältää infrastruktuuri ja moduulit Luo IoT yhdyskäytävän ratkaisuja. Voit laajentaa SDK räätälöityjä minkä tahansa lopusta loppuun-skenaario yhdyskäytävien luominen.

Voit ladata [Azure IoT yhdyskäytävän SDK] [ lnk-gateway-sdk] -GitHub.

## <a name="online-api-reference-documentation"></a>Online-Ohjelmointirajapinnan oppaat

Seuraavassa on luettelo linkeistä online Azure IoT laitteen, palvelun ja yhdyskäytävän kirjastojen API ohjeet:

- [Internet-kohteita (IoT) .NET][lnk-dotnet-ref]
- [IoT keskittimeen muille käyttäjille][lnk-rest-ref]
- [Azure IoT laitteen SDK c][lnk-c-ref]
- [Azure IoT laitteen SDK Java][lnk-java-ref]
- [Azure IoT palvelu SDK Java][lnk-java-service-ref]
- [Azure IoT laitteen SDK Node.js][lnk-node-ref]
- [Azure IoT palvelu SDK Node.js][lnk-node-service-ref]
- [Azure IoT yhdyskäytävän SDK-paketissa][lnk-gateway-ref]

## <a name="next-steps"></a>Seuraavat vaiheet

Muiden viittauksen aiheita IoT keskittimeen developer oppaan ovat:

- [IoT keskittimeen päätepisteet][lnk-devguide-endpoints]
- [Kyselykielen twins, menetelmistä ja työt][lnk-devguide-query]
- [Kiintiöiden ja rajoittaminen][lnk-devguide-quotas]
- [IoT keskittimeen MQTT tuki][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md