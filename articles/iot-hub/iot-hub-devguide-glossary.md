<properties
 pageTitle="Sovelluskehittäjän opas - sanasto | Microsoft Azure"
 description="Sanaston IoT keskittimeen koskevat yleiset ehdot"
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

# <a name="glossary-of-iot-hub-terms"></a>IoT keskittimeen termien sanasto

Tässä artikkelissa on lueteltu joitakin IoT keskittimeen liittyvät yleiset ehdot.

## <a name="advanced-message-queueing-protocol-amqp"></a>Lisäasetukset viestin asetetaan jonoon Protocol (AMQP)

[AMQP](https://www.amqp.org/) on tekstiviesti-protokollia, joiden IoT toiminto tukee laitteiden yhteydessä. Saat lisätietoja tekstiviesti-protokollia, joiden IoT toiminto tukee [Lähetä ja vastaanota viestit, joiden IoT toiminnossa](iot-hub-devguide-messaging.md).

## <a name="cloud-to-device-c2d"></a>Cloud laitteen (C2D)

Käytetään yleensä viitata yhdistetyn laitteen IoT-toiminnosta lähetettyihin viesteihin. Nämä viestit ovat usein, komennot, jotka opasta laitteen joitakin toimia. Lisätietoja on artikkelissa [Lähetä ja vastaanota viestit, joiden IoT toiminnossa](iot-hub-devguide-messaging.md).

## <a name="condition"></a>Ehto

Viittaa laitteen tilatiedot, kuten tällä hetkellä käytössä connectivity-menetelmä ilmoittaa laite-sovellus. Laitteiden lähettää raportin myös niiden ominaisuuksista. Voit tehdä kyselyn ehto ja ominaisuuksien käyttäminen laitteen sekä.

## <a name="data-point-message"></a>Piste viesti

Piste viestin on cloud laitteen viesti, joka sisältää telemetriatietojen tietoja, kuten Tuulen keskinopeus tai lämpötilan.

## <a name="desired-properties"></a>Haluamasi ominaisuudet

Laitteen twins konteksti-haluamasi ominaisuudet käytetään yhdessä *raportoitu ominaisuudet* haluat synkronoida laitteen kokoonpanotietoja tai ehto. Haluamasi ominaisuudet voidaan määrittää vain sovelluksen takaisin loppuun ja havaitaan laite-sovelluksessa. 

## <a name="device-to-cloud-d2c"></a>Laitteen cloud (D2C)

Käytetään yleensä viitata IoT keskittimeen yhdistetyn laitteesta lähetettyihin viesteihin. Näiden viestien ehkä [arvopisteen](#data-point-message) ja [vuorovaikutteisia](#interactive-message) viestejä. Lisätietoja on artikkelissa [Lähetä ja vastaanota viestit, joiden IoT toiminnossa](iot-hub-devguide-messaging.md).

## <a name="device-identity-registry"></a>Laitteen tunnistetietojen rekisterin

[Laitteen tunnistetietojen rekisterin](iot-hub-devguide-identity-registry.md) on IoT-keskittimeen, joka tallentaa tiedot voivat muodostaa yhteyden keskittimeen yksittäisten laitteiden valmiin osa.

## <a name="device"></a>Laite

IoT kontekstissa laite on yleensä pieniä, erillinen tietojenkäsittely laite, jotka voivat kerätä tietoja tai hallita muissa laitteissa. Esimerkiksi laite voi olla ympäristön seurantaa laitteen tai ohjauskoneen kasvihuonekaasujen juottamisen ja tuuletus-järjestelmiin.

## <a name="device-twin"></a>Laitteen sekä

[Laitteen sekä](iot-hub-devguide-device-twins.md) on kopio IoT toiminnossa fyysinen laitteen ehtoon ja määritys-asetuksia. Voit käyttää laitteen sekä fyysinen laite määritysten hallintaan.

## <a name="direct-method"></a>Suora menetelmä

[Suora menetelmä](iot-hub-devguide-direct-methods.md) on voit käynnistää menetelmän voi suorittaa kutsumalla Ohjelmointirajapinnan IoT-toiminnossa-laitteessa.

## <a name="event-hub-compatible-endpoint"></a>Tapahtuman keskittimeen yhteensopivan päätepiste

Lukemaan laitteen cloud IoT-toiminnossa lähetetyt viestit voit muodostaa yhteyden oman keskittimeen päätepisteen ja käyttää mitä tahansa tapahtumaa-toiminnossa yhteensopivan menetelmää näiden viestien lukeminen. Tapahtuman keskittimeen yhteensopivan menetelmiä ovat tapahtuman keskittimet SDK: T ja Azure Stream analysoinnin avulla.

## <a name="field-gateway"></a>Kentän yhdyskäytävän

Kentän yhdyskäytävän mahdollistaa connectivity laitteita, joita ei voi muodostaa suoraan IoT keskittimeen ja yleensä käyttöön paikallisesti laitteiden kanssa. Lisätietoja on artikkelissa [Azure IoT keskittimeen ominaisuudet?](iot-hub-what-is-iot-hub.md).

## <a name="job"></a>Työn

Ratkaisu takaisin päässä avulla töiden ajoittaminen ja seurata aktiviteetteja-laitteiden rekisteröityjä IoT-toiminnossa. Toiminnot ovat päivitetään laitteen toivottuja sekä ominaisuuksia, laitteen sekä tunnisteiden päivittäminen ja käynnistettäessä suoraan menetelmiä.

## <a name="protocol-gateway"></a>Protokollan yhdyskäytävän

Protokollan yhdyskäytävän yleensä on otettu käyttöön pilvessä ja sisältää protokolla käännöspalveluita yhteyden IoT keskittimeen laitteita. Lisätietoja on artikkelissa [Azure IoT keskittimeen ominaisuudet?](iot-hub-what-is-iot-hub.md).

## <a name="interactive-message"></a>Vuorovaikutteinen viesti

Vuorovaikutteinen viesti on pilven laitteen viestiin, joka käynnistää heti toiminnon sovelluksen takaisin loppuun. Esimerkiksi laite voi lähettää hälytyksen epäonnistumisesta, joka on kirjautunut automaattisesti CRM-järjestelmään.

## <a name="iot-hub"></a>IoT-toiminnossa

IoT toiminnosta on täysin hallitun Azure-palvelu, joka mahdollistaa miljoonia IoT laitteet ja ratkaisu taustatietokannaksi välisten yhteyksien luotettavan ja turvallisen kaksisuuntainen. Lisätietoja on artikkelissa [Azure IoT keskittimeen ominaisuudet?](iot-hub-what-is-iot-hub.md).

## <a name="iot-suite"></a>IoT Suite

Azure IoT Suite pakkaa yhdessä Azure palveluihin esimääritettyjä ratkaisuja. Nämä esimääritetyt ratkaisut avulla pääset nopeasti alkuun IoT yleiset skenaariot käyttöotot lopusta loppuun. Lisätietoja on artikkelissa [Azure IoT Suite ominaisuudet?](../iot-suite/iot-suite-overview.md).

## <a name="job"></a>Työn

[Työn](iot-hub-devguide-jobs.md) IoT toiminnossa avulla voit suorittaa toimintoja, kuten laitteisto-ohjelmiston päivityksen yli yhdistetty oman keskittimeen useilla eri laitteilla.

## <a name="mqtt"></a>MQTT

[MQTT](http://mqtt.org/) on tekstiviesti-protokollia, joiden IoT toiminto tukee laitteiden yhteydessä. Saat lisätietoja tekstiviesti-protokollia, joiden IoT toiminto tukee [Lähetä ja vastaanota viestit, joiden IoT toiminnossa](iot-hub-devguide-messaging.md).

## <a name="reported-properties"></a>Raportoidun ominaisuudet

Laitteen twins konteksti-raportoidun ominaisuuksia käytetään yhdessä *haluamasi ominaisuudet* haluat synkronoida laitteen kokoonpanotietoja tai ehto. Raportoidun ominaisuudet voidaan määrittää vain laite-sovelluksessa, ja voit voi lukea ja kysely sovelluksen takaisin loppuun mennessä.

## <a name="tags"></a>Tunnisteet

Devcie twins kontekstissa tunnisteet ovat laitteen metatiedon tallennettu ja noutamien sovelluksen takaisin loppuun JSON asiakirjan muodossa. Tunnisteet eivät näy sovelluksiin laitteessa.

## <a name="system-properties"></a>Järjestelmän ominaisuudet

Laitteen twins yhteydessä järjestelmän ominaisuudet ovat vain luku- ja seuraavat tiedot koskevat laitteen-käyttö, kuten viimeisten tehtävän aika- ja yhteyden tila.