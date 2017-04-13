<properties
 pageTitle="Kehittäjän guide - kiintiön ja rajoitusten | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - kiintiön, jotka koskevat IoT keskittimeen ja rajoittava toiminta kuvaus"
 services="iot-hub"
 documentationCenter=".net"
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

# <a name="reference---quotas-and-throttling"></a>Viittaus - kiintiön ja rajoittaminen

## <a name="quotas-and-throttling"></a>Kiintiöiden ja rajoittaminen

Jokaisen Azure tilauksen voi olla enintään 10 IoT keskittimet.

Kunkin IoT-toiminnossa on valmisteltu on useita tietyn SKU yksiköiden (Lisätietoja on artikkelissa [Azure IoT keskittimeen hinnat][lnk-pricing]). TUOTE- ja yksiköiden määrän, määrittävät voit lähetettävien viestien enimmäismäärä päivittäinen kiintiö.

Olevat versiot määrittää myös IoT keskittimeen tietokantanäkymä-kaikki toiminnot, joka rajoittava rajoitukset.

## <a name="operation-throttles"></a>Toiminnon ylikuormitustilan

Toiminnon ylikuormitustilan on korko rajoituksia, jotka käytetään minuutit alueet, ja ne on tarkoitettu välttää väärinkäyttö. IoT keskittimeen yrittää välttää virheet aina, kun se on mahdollista, joka palauttaa, mutta se alkaa palauttaa poikkeukset, jos hän rajoituksen rikkoo liian kauan.

Seuraavassa on pakotettu ylikuormitustilan luettelo. Arvot viittaavat yksittäisiä toiminnossa.

| Rajoituksen | Toiminto arvo |
| -------- | ------------- |
| Käyttäjätietojen rekisterin toiminnot (luominen, hakea, luettelon, päivittäminen ja poistaminen) | 5000/min/yksikkö (S3) <br/> 100/min/yksikkö (S1 ja S2). |
| Laitteen yhteydet | 6000/sec/yksikkö (S3) 120/sec/yksikkö (S2), 12/sec/yksikkö (S1). <br/>Vähintään 100/sec. <br/> Kaksi S1 resurssit ovat esimerkiksi 2\*12 = 24/sec, mutta se on vähintään 100/sec oman yksiköiden välillä. Yhdeksän S1 yksiköt on 108/sec (9\*12) että yksiköiden välillä. |
| Lähettää pilvipalveluun laite | 6000/sec/yksikkö (S3) 120/sec/yksikkö (S2), 12/sec/yksikkö (S1). <br/>Vähintään 100/sec. <br/> Kaksi S1 resurssit ovat esimerkiksi 2\*12 = 24/sec, mutta se on vähintään 100/sec oman yksiköiden välillä. Yhdeksän S1 yksiköt on 108/sec (9\*12) että yksiköiden välillä. |
| Cloud laitteen lähettää | 5000/min/yksikkö (S3) 100/min/yksikkö (S1 ja S2). |
| Cloud laite vastaanottaa | 50000/min/yksikkö (S3) 1000/min/yksikkö (S1 ja S2). |
| Lataa tiedostotoiminnot | 5000 tiedoston lataaminen ilmoitukset/min/yksikkö (S3), 100 tiedoston lataamisen ilmoitukset/min/yksikkö (S1 ja S2). <br/> 10000 SAS URI voi olla ulos Azure-tallennustilan tilin samalla kertaa.<br/> 10 SAS URI/laitteen voi olla ulos samalla kertaa. | 

On tärkeää selventää *laitteen yhteydet* rajoituksen määrittää nopeus, jolla uuden laitteen yhteyksiä voi muodostaa IoT-toiminnosta ja ei samanaikaisesti yhdistettyjen laitteiden enimmäismäärä. Rajoituksen määräytyy käyttöasteen, joka on valmisteltu valitsemalla.

Esimerkiksi jos ostat S1 yksikkönä, näyttöön tulee 100 yhteyksien sekunnissa rajoituksen. Tämä tarkoittaa, että muodostaa 100 000 laitteiden kestää vähintään 1 000 sekuntia (noin 16 minuuttia). Voi kuitenkin olla on rekisteröity laitteen tunnistetietojen rekisteriä laitteita niin monta samanaikaisesti yhdistettyjen laitteiden.

Perusteellisempaa keskustelun IoT-toiminnossa on rajoitin-toiminnon asetukset seuraavassa blogimerkinnässä [IoT keskittimeen rajoittaminen ja][lnk-throttle-blog].

>[AZURE.NOTE] Milloin tahansa annetun on mahdollista niin, että kiintiöiden tai rajoituksen rajoitukset suurentamalla IoT-toiminnossa valmistellun yksiköiden määrän.

>[AZURE.IMPORTANT] Käyttäjätietojen rekisterin toiminnot on tarkoitettu käytettäväksi suorituksenaikainen hallinta ja valmistelu skenaarioita. Luettaessa tai päivittäminen on runsaasti laitteen käyttäjätietojen tuetaan kautta [tuominen ja vieminen työt][lnk-importexport].

## <a name="next-steps"></a>Seuraavat vaiheet

Muiden viittauksen aiheita IoT keskittimeen developer oppaan ovat:

- [IoT keskittimeen päätepisteet][lnk-devguide-endpoints]
- [Kyselykielen twins, menetelmistä ja työt][lnk-devguide-query]
- [IoT keskittimeen MQTT tuki][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md