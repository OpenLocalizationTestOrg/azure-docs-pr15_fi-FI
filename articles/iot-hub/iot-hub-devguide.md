<properties
 pageTitle="Sovelluskehittäjän opas ohjeaiheissa on IoT keskittimeen | Microsoft Azure"
 description="Azure IoT keskittimeen developer opas, joka sisältää IoT keskittimeen päätepisteet, suojaus, laite tunnistetietojen rekisterin, hallinta ja viestintä"
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

# <a name="azure-iot-hub-developer-guide"></a>Azure IoT keskittimeen Sovelluskehittäjän opas

Azure IoT-toiminnossa on täysin hallittujen palvelu, joka auttaa käyttöön luotettavan ja turvallisen kaksisuuntaisen tietoliikenteen miljoonia IoT laitteiden välillä ja sovelluksen takaisin lopettaa.

Azure IoT-toiminnossa on kanssa:

* Suojattu tietoliikenne käyttämällä laitteen kohti suojausvaltuudet sekä käyttää ohjausobjektin.
* Luotettavan laitteen pilven ja pilven laitteen hyper-akseli viesti.
* Suosituimmat kielet ja ympäristöjen laitteen kirjastoja helposti laitteen yhteys.

IoT keskittimeen Sovelluskehittäjän opas sisältää on seuraavissa artikkeleissa:

- [Lähetä ja vastaanota viestit kanssa IoT keskittimeen] [ devguide-messaging] käsitellään tekstiviesti ominaisuuksia (laitteen pilven ja pilven laite), joka paljastaa IoT toiminnossa. Artikkelissa on myös tietoja aiheista viestimuotoa, kuten ja tuettujen viestintä-protokollat ja käyttävät porttinumerot.
- [Lataa tiedostot laitteesta] [ devguide-upload] kuvataan, miten voit ladata tiedostoja laitteesta. Tässä artikkelissa on myös tietoja aiheista, kuten ilmoitusten uplaod prosessi lähettää.
- [Laitteen IoT toiminnossa jäsenyyksiä hallitaan] [ devguide-identities] kuvataan, mitä tietoja kunkin IoT-toiminnossa laitteen tunnistetietojen rekisterin tallentaa ja miten voit käyttää ja muokata sitä.
- [Hallita IoT keskittimeen] [ devguide-security] kuvataan suojausmalli IoT keskittimeen toimintoja käyttöoikeus sekä laitteita ja cloud osat. Artikkelissa on lisätietoja tunnusten ja X.509-varmenteita ja myöntää käyttöoikeuksia tiedot.
- [Synkronoinnin tilan ja määritykset laitteen twins avulla] [ devguide-device-twins] kuvataan *laitteen sekä* käsitteestä ja toimintoja, kuten synkronointi laitteen kanssa sen sekä sen paljastaa. Artikkelissa on tietoja laitteen sekä tallennettuja tietoja.
- [Suora menetelmän laitteessa] [ devguide-directmethods] kuvataan suora menetelmä tietoja siitä, miten ongelma menetelmiä laitteessa taustatietokantaan-sovellukset ja käsitellä suora menetelmä laitteen lifecycle.
- [Ajoita työt useilla eri laitteilla] [ devguide-jobs] kuvataan, miten voit ajoittaa työt useilla eri laitteilla. Artikkelissa kerrotaan, miten voit lähettää työt, jotka suorittavat tehtäviä suoritettaessa suoraan menetelmää, devcie, käyttämällä devcie sekä päivittäminen. Se myös käsitellään kyselyn projektin tila.
- [Viittaus - toiminnossa IoT päätepisteet] [ devguide-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen runtime ja hallintaa varten. Artikkelissa kerrotaan myös käyttämisestä kentän yhdyskäytävän käyttöön joitakin laitteita muodostaa IoT keskittimeen päätepisteet.
- [Viittaus - kyselykieltä twins, menetelmät ja töiden] [ devguide-query] kuvaa, jonka avulla voit hakea tietoja oman keskittimeen laitteen twins ja töiden kyselyn kyseisellä kielellä.
- [Viittaus - kiintiön ja rajoitusten] [ devguide-quotas] kiintiöiden määrittäminen IoT keskittimeen-palveluun ja rajoittava toiminnan, kun kiintiön ole odotat on yhteenveto.
- [Viittaus - laite- ja palveluluettelon SDK: T] [ devguide-sdks] luetteloiden SDK: T avulla voit kehittää laitteen ja palvelun sovelluksia, jotka käsitellä IoT-toiminnossa. Artikkelissa on linkkejä API ohjeessa.
- [Viittaus - IoT keskittimeen MQTT tuki] [ devguide-mqtt] yksityiskohtaisia tietoja siitä, miten IoT toiminto tukee MQTT-protokollaa. Artikkelin kuvataan valmiita SDK: T MQTT-protokollan tuki ja tietoja suoraan MQTT-protokollan avulla.
- [Sanasto] [ devguide-glossary] Yleiset IoT keskittimeen liittyvät termit luettelo.



[devguide-messaging]: iot-hub-devguide-messaging.md
[devguide-upload]: iot-hub-devguide-file-upload.md
[devguide-identities]: iot-hub-devguide-identity-registry.md
[devguide-security]: iot-hub-devguide-security.md
[devguide-device-twins]: iot-hub-devguide-device-twins.md
[devguide-directmethods]: iot-hub-devguide-direct-methods.md
[devguide-jobs]: iot-hub-devguide-jobs.md
[devguide-endpoints]: iot-hub-devguide-endpoints.md
[devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[devguide-query]: iot-hub-devguide-query-language.md
[devguide-sdks]: iot-hub-devguide-sdks.md
[devguide-mqtt]: iot-hub-mqtt-support.md
[devguide-glossary]: iot-hub-devguide-glossary.md

