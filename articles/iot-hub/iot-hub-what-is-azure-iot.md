<properties
 pageTitle="Azure ratkaisuja tavalla Internet | Microsoft Azure"
 description="Valitse malli-ratkaisuarkkitehtuuri ja miten se liittyy Azure IoT keskittimeen, laite SDK: T ja esimääritetyt ratkaisut mukaan lukien Azure IoT yleiskatsaus"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

[AZURE.INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Azure IoT-toiminnossa on Azure-palvelu, joka mahdollistaa suojatun ja sovelluksen välinen luotettava kaksisuuntaisen tietoliikenne takaisin lopetus- ja laitteiden miljoonia. Ottaa käyttöön sovelluksen takaisin päässä:

- Saat telemetriatietojen tasolla laitteilla.
- Reitittää tiedot stream tapahtuma-suoritin laitteilla.
- Tiedostojen lataaminen palvelimeen saavat laitteilla.
- Lähetä cloud laitteen komennot tiettyjä laitteita.

Voit käyttää IoT keskittimeen toteuttamisesta oman ratkaisu takaisin loppuun. Lisäksi IoT-toiminnossa on käytettävä valmistella laitteet ja niiden suojausvaltuudet voivat muodostaa yhteyden valitsemalla laitteen identity-rekisterin. Lisätietoja IoT toiminnosta on artikkelissa [IoT keskittimeen ominaisuudet?] [lnk-iot-hub].

Miten Azure IoT keskittimeen mahdollistaa standardien-pohjaisten IoT Laitehallinta etäyhteyden hallintaa, määrittää ja päivittää laitteet on artikkelissa [Azure IoT keskittimeen yleiskatsaus Laitehallinnan][lnk-device-management].

Toteuttamaan asiakassovelluksissa laitteen laitteiston ympäristöjen ja käyttöjärjestelmien erilaisia voit IoT laitteen SDK: T. IoT laitteen SDK: T sisältää kirjastoja, jotka helpottavat lähettämisen telemetriatietojen IoT-toiminnosta ja vastaanota cloud laitteen komentoja. Kun SDK: T, voit valita useita verkkoprotokollien pitää yhteyttä IoT toiminnossa. Lisätietoja on artikkelissa [tietoja laitteen SDK: T][lnk-device-sdks].

Aloita kirjoittaminen lisäkoodin ja muutamia esimerkkejä käynnissä-kohdasta [IoT keskittimeen käytön aloittaminen] [ lnk-getstarted] opetusohjelma.

Voit myös tarkastella [Azure IoT]ohjelmistopakettini[lnk-iot-suite], joka on kokoelma esimääritetyt ratkaisut. IoT Suite mahdollistaa pääset nopeasti alkuun ja Skaalaa IoT projektien yleiset IoT skenaariot – kuten remote valvonta, hallintaa ja ennakoivan ylläpito sähköpostiosoite.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md