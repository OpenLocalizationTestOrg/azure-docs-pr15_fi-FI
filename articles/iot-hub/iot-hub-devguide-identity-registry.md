<properties
 pageTitle="Kehittäjän guide - laitteen tunnistetietojen rekisterin | Microsoft Azure"
 description="Azure IoT keskittimeen developer guide - kuvaus laitteen identity-rekisterin ja hallitse sen avulla"
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

# <a name="manage-device-identities-in-iot-hub"></a>Laitteen IoT toiminnossa käyttäjätietojen hallinta

## <a name="overview"></a>Yleiskatsaus

Jokaisen IoT-toiminnossa on laitteen identity-rekisterin, joka sisältää tietoja, jotka voivat muodostaa yhteyden valitsemalla laitteista. Ennen kuin laitteen muodostaa keskittimeen, on oltava merkinnän laitteen toiminnossa laitteen tunnistetietojen rekisterissä. Laite on myös todentamismenetelmä keskittimeen perusteella laitteen käyttäjätiedot rekisteriin tallennettuja tunnistetietoja.

Korkean tason laitteen tunnistetietojen rekisteri on REST-yhteyttä hyödyntäviin kokoelma tunnistetietojen resurssit. Kun tekstin lisääminen rekisterin IoT keskittimeen Luo laitteen kohti resurssien joukon kuten jonossa, joka sisältää tapahtumakartoitus cloud laitteen viestit-palvelussa.

### <a name="when-to-use"></a>Käyttäminen

Käytä laitteen tunnistetietojen rekisterin, kun olet valmisteltava laitteet, Yhdistä IoT keskittimeen ja milloin haluat hallita laitteen kohti laitteen osoittava päätepisteet oman toiminnossa.

> [AZURE.NOTE] Laitteen tunnistetietojen rekisteri ei sisällä sovelluksen kielikohtaiset metatietoihin.

## <a name="device-identity-registry-operations"></a>Laitteen tunnistetietojen rekisterin toiminnot

IoT keskittimeen laitteen identity-rekisterin paljastaa seuraavat toimenpiteet:

* Laitteen käyttäjätietojen luominen
* Päivitä laitteen tunnistetiedot
* Laitteen tunnistetietojen sijaan tunnuksen hakeminen
* Poista laitteen tunnistetiedot
* Enintään 1 000 käyttäjätietojen luettelo
* Vie kaikki käyttäjätiedot Azure-blob-säiliö
* Käyttäjätietojen tuominen Azure-blob-säiliö

Näitä toimintoja voi käyttää Optimistinen samanaikainen määritetty [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Ainoa tapa noutaa kaikki käyttäjätiedot keskittimeen, joka tunnistetietojen rekisterin on käyttää [Vie] [ lnk-export] toimintoja.

IoT keskittimeen laitteen tunnistetietojen rekisteri:

- Sovelluksen metatietoihin ei sisällä.
- Niitä voi käyttää sanaston, kuten käyttämällä **deviceId** avaimeksi.
- Ilmeikäs kyselyt eivät tue.

IoT ratkaisu on yleensä erilliset ratkaisu kielikohtaiset-kaupasta, joka sisältää sovelluksen kielikohtaiset metatiedot. Esimerkiksi smart rakennuksen ratkaista ratkaisu kielikohtaiset kauppa tallentaa sen ryhmän, jossa lämpötilamittarin on otettu käyttöön.

> [AZURE.IMPORTANT] Vain sellaisia laitteen tunnistetietojen rekisterin hallinta ja toiminnot. Suuri siirtonopeuden toimintojen suorituksen aikana ei olisi riippuvainen toimintojen tekemistä laitteen identity-rekisterin. Esimerkiksi tarkistus laitteen yhteystilan ennen lähettämistä-komento ei ole tuettu kuvio. Varmista, että voit tarkistaa [rajoittimen korvaukset] [ lnk-quotas] laitteen identity-rekisterin ja [laitteen uudelleenaktivoinnin yhteydessä] [ lnk-guidance-heartbeat] kuvio.

## <a name="disable-devices"></a>Laitteiden poistaminen käytöstä

Voit poistaa laitteiden päivittämällä rekisterin jäsenyyden **tilan** -ominaisuutta. Käytät tätä ominaisuutta yleensä kaksi skenaariot:

- Valmistelu tiedonsiirron-prosessin aikana Lisätietoja on artikkelissa [Laitteen käyttöönotto][lnk-guidance-provisioning].
- Jos jostakin syystä kannattaa laitteen on tai enää ole.

## <a name="import-and-export-device-identities"></a>Tuominen ja vieminen laitteen käyttäjätietoja

Voit viedä laitteen käyttäjätietojen joukkona IoT-toiminnossa tunnistetietojen rekisteristä käyttämällä asynkronisten toimintojen [IoT keskittimeen resurssin tarjoajan päätepisteen][lnk-endpoints]. Vienti on pitkään suoritettavien työt, jotka asiakkaan toimittaman blob-säilö avulla voit tallentaa laitteen käyttäjätiedot tunnistetietojen rekisteristä.

Voit tuoda laitteen käyttäjätietojen joukkona IoT-toiminnossa tunnistetietojen rekisterin käyttämällä asynkronisten toimintojen [IoT keskittimeen resurssin tarjoajan päätepisteen][lnk-endpoints]. Tuonnin ovat asiakkaan toimittaman blob-säilö tietojen kirjoittaminen laitteen käyttäjätiedot laitteen käyttäjätiedot rekisteriin käyttävien pitkään käynnissä olevien töiden.

- Katso lisätietoja tuonti ja vienti ohjelmointirajapinnan [IoT keskittimeen resurssin tarjoajaan REST API][lnk-resource-provider-apis].
- Lisätietoja käynnissä Tuo ja vie työt on ohjeaiheessa [IoT keskittimeen laitteen käyttäjätietojen hallinta joukkona][lnk-bulk-identity].

## <a name="device-provisioning"></a>Laitteen käyttöönotto

Laitteen tiedot, jotka annetun IoT-ratkaisun tallentaa määräytyy sen, että ratkaisu. Mutta vähimmäisvaatimus ratkaisu on tallennettava laitteen käyttäjätiedot ja todentaminen avaimet. Azure IoT-toiminnossa on identity-rekisterin, voit tallentaa kunkin laitteen, kuten tunnukset, avaimet todennus ja tilakoodin arvoja. Muut Azure-palvelut, kuten Azure-taulukkotallennus Azure-blob-säiliö ja Azure DocumentDB avulla ratkaista voidaan tallentaa laitteen tiedot.

Luettelokohteiden lisääminen alkuperäisen laitteen tiedot ratkaisu myymälät *laitteen valmistelu* on. Uuden laitteen muodostaa yhteyttä keskittimeen käyttöön on lisättävä uusi Laitetunnus ja avaimet IoT keskittimeen identity-rekisterin. Osana valmistelun yhteydessä voit joutua muuttamaan alusta-ratkaisun muiden stores laitekohtaiset tiedot.

## <a name="device-heartbeat"></a>Laitteen uudelleenaktivoinnin yhteydessä

IoT keskittimeen tunnistetietojen rekisteri sisältää **connectionState**-nimisen kentän. **ConnectionState** kentän kannattaa käyttää vain kehityksen aikana ja virheenkorjaus IoT ratkaisuja olisi ei kyselyn kenttä suorituksen aikana (esimerkiksi tarkistamaan, jos laite on yhdistetty, jotta voit päättää, cloud laitteen viestin tai Tekstiviestin lähettäminen).

Jos IoT ratkaisu tarvitsee tietää, jos laite on yhdistetty (suorituksen aikana, tai Lisää tarkkuudella kuin **connectionState** -ominaisuus on), ratkaisu olisi pantava täytäntöön *uudelleenaktivoinnin yhteydessä kuvio*.

Uudelleenaktivoinnin yhteydessä, kuvio-laite lähettää laitteen cloud viestien vähintään kerran jokaisen rahassa aika (esimerkiksi vähintään kerran tunnissa). Tämä tarkoittaa, että myös silloin, kun laite ei ole mitään tietoja, voit lähettää, se lähettää edelleen laitteen cloud-viesti on tyhjä (yleensä kanssa ominaisuus, joka tunnistaa sen muodossa uudelleenaktivoinnin yhteydessä). Palvelun reunassa ratkaisun ylläpitää kartan viimeisen uudelleenaktivoinnin yhteydessä vastaanotettu kunkin laitteen kanssa ja oletetaan, että ilmenee ongelmia laitteen kanssa Jos se ei saa uudelleenaktivoinnin yhteydessä viestin odotetun ajan kuluessa.

Monimutkaisempia käyttöönotto voi sisältää tiedot [toimintojen seuranta] [ lnk-devguide-opmon] tunnistaa laitteet, joissa käytetään yhteyden tai viestiä yritetään mutta puuttuessa. Kun otat uudelleenaktivoinnin yhteydessä kuviota, varmista, että [IoT keskittimeen kiintiöiden ja ylikuormitustilan][lnk-quotas].

> [AZURE.NOTE] Jos IoT-ratkaisun tarvitsee laitteen yhteystilaa ainoastaan, jos haluat määrittää cloud laitteen viestien lähettämiseen, viestit lähetetään ei laitteiden suurten tietojoukkojen paljon yksinkertaisempi kuvion huomioitavia on käyttää lyhyt voimassaolon ajan. Tämä kertyy saman tuloksen kuin käyttämällä uudelleenaktivoinnin yhteydessä, kuvio aikana johtaa siihen merkittävästi tehokkaampaa laitteen yhteyden tilan rekisterin säilytetään. Se on mahdollista, myös pyytämällä viestin kuittaussanomien ilmoitusten IoT keskittimeen laitteet, jotka voivat vastaanottaa viestejä ja jotka eivät ole online tai ovat epäonnistui.

## <a name="reference-topics"></a>Viittaus aiheet:

Viittaus seuraavissa ohjeaiheissa on lisätietoja devcie tunnistetietojen rekisterin.

## <a name="device-identity-properties"></a>Laitteen tunnistetietojen ominaisuudet

Laitteen käyttäjätietojen esitetään JSON-asiakirjojen seuraavat ominaisuudet.

| Ominaisuus | Asetukset | Kuvaus |
| -------- | ------- | ----------- |
| deviceId | pakollinen, vain luku-päivitykset | ASCII-7-bittisten aakkosnumeerisia merkkejä kirjainkoko on merkitsevä merkkijono (enintään 128: aa merkkiä) + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | pakollinen, vain luku-tilassa | Kirjainkoon, toiminto luo merkkijono, enintään 128: aa merkkiä. Tätä käytetään erottaa laitteet, joissa on sama **deviceId**, kun ne poistetaan ja luodaan uudelleen. |
| ETag | pakollinen, vain luku-tilassa | Merkkijono, joka vastaa heikkoja etag laitteen käyttäjätietojen, kuten [RFC7232][lnk-rfc7232].|
| todennus | Valinnainen | Koosteen objektit, joiden todennus ja niiden tietoturva materiaali. |
| auth.symkey | Valinnainen | Koosteen objekti, joka sisältää ensisijaisen ja toissijaisen avaimen tallennettu base64-muodossa. |
| tila | pakollinen | Access-ilmaisin. Voi olla **käytössä** tai **ei käytössä**. Jos **käytössä**laite voi muodostaa. Jos **poistettu käytöstä**, tämä laite ei voi käyttää minkä tahansa laitteen osoittava päätepiste. |
| statusReason | Valinnainen | 128 merkin pituinen merkkijono, joka tallentaa laitteen jäsenyyden tilan syy. Voit käyttää kaikkia UTF-8-merkkejä. |
| statusUpdateTime | vain luku-tilassa | Ajallinen ilmaisin, jossa päivämäärän ja kellonajan viimeisimmän päivityksen tila. |
| connectionState | vain luku-tilassa | Kentän, joka ilmaisee yhteyden tila: **yhteys** tai **yhteys katkaistu**. Tässä kentässä edustaa IoT keskittimeen näkymän laitteen yhteyden tila. **Tärkeää**: Tätä kenttää voi käyttää vain tarkoituksiin kehittäminen ja virheenkorjaus. Yhteyden tilan päivitetään vain MQTT tai AMQP laitteet. Lisäksi se perustuu protokolla tason Testaa (MQTT Testaa eli AMQP Testaa) ja se voi olla enintään viive, vain 5 minuuttia. Seuraavien syiden takia voi olla tunnistettujen, kuten laitteiden valmiiksi yhteydessä, mutta, jotka ovat todella katkennut. |
| connectionStateUpdatedTime | vain luku-tilassa | Ajallinen ilmaisin, joka näyttää päivämäärän ja kellonajan viimeisen yhteystilaa on päivitetty. |
| lastActivityTime  | vain luku-tilassa | Ajallinen ilmaisin näyttää päivämäärän ja kellonajan viimeisen laitteen yhteydessä, vastaanotettu tai lähetetty viesti. |

> [AZURE.NOTE] Yhteyden tilan vain kuvaa yhteyden tilan IoT keskittimeen näkymän. Tässä tilassa päivitykset voidaan lykätä Verkkoehdot ja määritysten mukaan.

## <a name="additional-reference-material"></a>Lisää aineistoa

Kehittäjän opas muiden viittauksen aiheet ovat seuraavat:

- [IoT keskittimeen päätepisteet] [ lnk-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen runtime ja hallintaa varten.
- [Rajoitusten ja kiintiöiden] [ lnk-quotas] kuvataan kiintiön, jotka koskevat IoT keskittimeen-palveluun ja rajoittava toiminnan toiminta palvelua käytettäessä.
- [IoT keskittimeen laite- ja palveluluettelon SDK: T] [ lnk-sdks] näyttää eri kielellä SDK: T voit Käytä tätä, kun laite ja palvelun sovelluksista, jotka toimivat IoT keskittimeen ja kehittää.
- [Kyselyn kielen twins, menetelmistä ja töiden] [ lnk-query] kuvataan kyselykieltä, voit hakea tietoja IoT-toiminnosta laitteen twins, menetelmät ja projektit.
- [IoT keskittimeen MQTT tuki] [ lnk-devguide-mqtt] on lisätietoja IoT keskittimeen tuki MQTT-protokollaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt käyttämisestä IoT keskittimeen laitteen identity-rekisterin oppimiasi voi olla kiinnostuneita Sovelluskehittäjän opas seuraavissa ohjeaiheissa:

- [IoT pääsivuston käyttöoikeuksien hallinta][lnk-devguide-security]
- [Synkronoinnin tilan ja määritykset laitteen twins avulla][lnk-devguide-device-twins]
- [Suora menetelmän laitteessa][lnk-devguide-directmethods]
- [Ajoita työt useilla eri laitteilla][lnk-devguide-jobs]

Jos haluat kokeilla osa käsitteistä, joka on kuvattu tämän artikkelin, voit tarkastella käyttämällä seuraavia IoT keskittimeen opetusohjelma:

- [Azure IoT keskittimeen käytön aloittaminen][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md