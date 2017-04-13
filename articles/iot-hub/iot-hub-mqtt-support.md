<properties
 pageTitle="IoT keskittimeen MQTT tuki | Microsoft Azure"
 description="Kuvaus MQTT tuki IoT keskittimeen tason"
 services="iot-hub"
 documentationCenter=".net"
 authors="kdotchkoff"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/24/2016"
 ms.author="kdotchko"/>

# <a name="iot-hub-mqtt-support"></a>IoT keskittimeen MQTT tuki

IoT keskittimeen mahdollistaa laitteet pitää yhteyttä IoT keskittimeen laitteen päätepisteet käyttämällä [MQTT v3.1.1] [ lnk-mqtt-org] protokolla portin 8883 tai MQTT v3.1.1 porttiin 443 WebSocket-protokollan kautta. IoT toiminto edellyttää, että kaikki laitteen tietoliikenteen suojaamista käyttämällä TLS/SSL (näin ollen IoT keskittimeen ei tue-suojattuja yhteyksiä portin 1883 kautta).

## <a name="connecting-to-iot-hub"></a>Yhteyden muodostaminen IoT-toiminnossa

Laitteen voit muodostaa yhteyden joko käyttämällä [Microsoft Azure IoT SDK: T] kirjastoihin IoT-toiminnossa MQTT-protokollan avulla[ lnk-device-sdks] tai käyttämällä MQTT protokolla suoraan.

## <a name="using-the-device-client-sdks"></a>Laitteen käyttävä SDK: T

[Laitteen asiakkaan SDK: T] [ lnk-device-sdks] tukevat MQTT protokolla ovat käytettävissä Java, Node.js, C, C# ja Python. Laitteen asiakkaan SDK: T muodostavat yhteyden IoT-toiminnossa vakio IoT keskittimeen yhteysmerkkijonon avulla. Käyttämään MQTT-protokollaa asiakkaan protokolla-parametri on määritettävä **MQTT**. Oletusarvon mukaan laitteen asiakkaan SDK: T Yhdistä IoT-keskittimeen, jossa **CleanSession** Merkitse arvoksi **0** ja käyttää **QoS 1** viestin exchange sen IoT kanssa.

Kun laite on yhdistetty IoT-keskittimeen, laitteen asiakkaan SDK: T määritettävä menetelmiä, jotka mahdollistavat laitteen viestien lähettäminen ja vastaanottaminen IoT-toiminnosta.

Seuraavassa taulukossa on linkkejä MALLIKOODEJA jokaisen tuetun kielen ja määrittää parametri avulla voit muodostaa yhteyden IoT keskittimeen MQTT-protokollan avulla.

| Kieli                   | Protokolla-parametri        |
| -------------------------- | ------------------------- |
| [Node.js][lnk-sample-node] | Azure-iot-laite-mqtt     |
| [Java][lnk-sample-java]    | IotHubClientProtocol.MQTT |
| [C][lnk-sample-c]          | MQTT_Protocol             |
| [C#][lnk-sample-csharp]    | TransportType.Mqtt        |
| [Python][lnk-sample-python] | IoTHubTransportProvider.MQTT |

### <a name="migrating-a-device-app-from-amqp-to-mqtt"></a>Laitteen sovelluksen siirtämisestä MQTT AMQP
Jos käytät [laitetta asiakkaan SDK: T][lnk-device-sdks]-siirtyminen AMQP MQTT käyttäminen edellyttää muuttaminen asiakkaan alustus protokolla-parametrin ilmoitetulla yläpuolella.

Kun teet näin, varmista, että Tarkista seuraavat seikat:

* AMQP palauttaa useita ehtoja, virheitä, kun taas MQTT katkaisee yhteyden. Tuloksena oman poikkeuksen käsittely logiikan saattaa edellyttää muutoksia.
* MQTT ei tue *Hylkää* -toimintoja, kun vastaanotat [viestejä C2D][lnk-messaging]. Jos takaisin pää tarvitsee lähettää vastauksen laite-sovelluksen, kannattaa käyttää [suoraan menetelmiä][lnk-methods].

## <a name="using-the-mqtt-protocol-directly"></a>MQTT-protokollan avulla suoraan

Jos laitetta ei voi käyttää laitteen asiakkaan SDK: T, se voi silti muodostaa yhteyden julkisen laitteen päätepisteet MQTT-protokollan avulla. **Yhdistä** -paketin laitteen kannattaa käyttää seuraavat arvot:

- Käytä **deviceId** **ClientId** -kenttään. 
- **Username** -kenttä käyttää `{iothubhostname}/{device_id}`, jossa {iothubhostname} on IoT-toiminnossa koko CNAME-tietue.

    Esimerkiksi jos IoT-toiminnossa on **contoso.azure devices.net** ja laite on **MyDevice01**, koko **Username** -kenttä pitäisi näkyä `contoso.azure-devices.net/MyDevice01`.

- Käytä SAS tunnuksen **salasana** -kenttään. SAS tunnuksen muoto on sama kuin HTTP- ja AMQP protokollia:<br/>`SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}`.

    Saat lisätietoja Luo SAS tunnusten [käyttämällä IoT keskittimeen suojauksen tunnusten]laite-osassa[lnk-sas-tokens].
    
    Kun testataan, voit käyttää myös [Laitteen Explorer] [ lnk-device-explorer] voivat luoda nopeasti SAS-tunnuksen, voit kopioida ja liittää oman koodin työkalu:
    
    1. Siirry laitteen Explorerin **hallinta** -välilehti.
    2. Valitse **SAS suojaustunnuksen** (oikeasta yläkulmasta).
    3. Valitse **SASTokenForm**, valitse avattava **DeviceID** laitteen. Määritä oman **TTL**.
    4. Valitse **Luo** voit luoda oman tunnuksen.
    
    Tämä rakenne on SAS tunnuksen, joka on luotu:   `HostName={your hub name}.azure-devices.net;DeviceId=javadevice;SharedAccessSignature=SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fMyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802`.

    Tämän tunnuksen avulla voit muodostaa yhteyden MQTT **salasana** -kenttään on osa:   `SharedAccessSignature sr={your hub name}.azure-devices.net%2fdevices%2fyDevice01&sig=vSgHBMUG.....Ntg%3d&se=1456481802g%3d&se=1456481802`.

MQTT yhteyden ja Katkaise paketteja, IoT keskittimeen liittyvät ongelmat tapahtuman **Toimintojen seuranta** -kanavalla lisätiedot, jotka voit tehdä yhteysongelmien vianmääritys.

### <a name="sending-messages-to-iot-hub"></a>Viestien lähettäminen IoT keskittimeen

Kun olet tehnyt yhteyden muodostamiseen, laite lähettää viestejä IoT keskittimeen käyttämällä `devices/{device_id}/messages/events/` tai `devices/{device_id}/messages/events/{property_bag}` **Aiheen nimi**. `{property_bag}` Elementin mahdollistaa laitteen lähettää viestit, joissa on lisäominaisuuksia url-koodatun-muodossa. Esimerkki:

```
RFC 2396-encoded(<PropertyName1>)=RFC 2396-encoded(<PropertyValue1>)&RFC 2396-encoded(<PropertyName2>)=RFC 2396-encoded(<PropertyValue2>)…
```

> [AZURE.NOTE] Tämä `{property_bag}` elementin käyttää samaa koodausta kuin kyselymerkkijonot HTTP-protokolla.

Laitteen asiakassovellus käyttää myös `devices/{device_id}/messages/events/{property_bag}` muodossa **on ohjeaiheessa nimen** määrittämiseen *tulevat viestit* siirtyvät telemetriatietojen viestinä.

IoT keskittimeen ei tue QoS 2 viestejä. Jos laitteen asiakkaan julkaisee **QoS 2**sisältävän viestin, IoT keskittimeen sulkee verkkoyhteyttä.
IoT keskittimeen ei säily Säilytä viestit. Jos laite lähettää viestin **Säilytä** -merkin 1, Lisää IoT keskittimeen **x-osallistua-säilyttää** sovelluksen ominaisuus viestiin. Tässä tapauksessa sijaan toteaa Säilytä viestin IoT keskittimeen välittää sen Taustajärjestelmä-sovellukseen.

### <a name="receiving-messages"></a>Viestien vastaanottaminen

Vastaanottamaan viestejä IoT-toiminnosta laitteen olisi tilaa käyttämällä `devices/{device_id}/messages/devicebound/#` **Aiheen suodatin**. Monitasoisten yleismerkin **#** aiheen suodatinta käytetään vain, jos haluat sallia laitteelle vastaanottaminen lisäominaisuuksia aiheen nimi. IoT keskittimeen ei salli käyttö **#** tai **?** yleismerkkejä aliraportti aiheista suodattamista varten. Koska IoT-toiminto ei ole tekstiviesti yleisiä tarkoitus kirja sub-broker, se tukee vain kuvata aiheiden nimet ja aiheen suodattimet.

Ota Huomautus, että laitteen saa viestejä IoT-toiminnosta, ennen kuin se on onnistuneesti tilannut sen laitteen tietyn päätepisteen, vastaavan `devices/{device_id}/messages/devicebound/#` aiheen suodatin. Kun onnistuneen tilaus on vahvistettu, laite alkavat vastaanottaminen cloud laitteen vain viestit, jotka on lähetetty sen jälkeen, kun tilaus. Jos laitteen yhdistää **CleanSession** merkin **0**ja tilauksen voi käyttää eri istuntojen välillä. Tässä tapauksessa se yhdistää **CleanSession 0** laitteen seuraavan kerran saa avoin viestejä, jotka on lähetetty siihen, kun se on katkennut. Jos laite on käytössä **CleanSession** Merkitse arvoksi **1** , mutta se saa viestejä IoT-toiminnosta, kunnes se tilaa laitteen sen päätepisteen.

IoT keskittimeen lähettää viestit **Aiheen nimi** `devices/{device_id}/messages/devicebound/`, tai `devices/{device_id}/messages/devicebound/{property_bag}` tilanne viestin ominaisuuksia. `{property_bag}`sisältää viestin ominaisuudet url-koodatun avain/arvo-pareina. Ominaisuuden kantolaukku sisältyvät vain sovelluksen ominaisuuksien ja käyttäjän määritettävissä järjestelmän ominaisuudet (esimerkiksi **messageId** tai **correlationId**). Järjestelmän ominaisuuksien nimiä on etuliite **$**-sovelluksen ominaisuuksien käyttäminen ei etuliitettä alkuperäisen ominaisuuden nimi.

Kun laite on jokin muu sähköpostiohjelma tilaa aiheen **QoS**2, IoT keskittimeen antaa suurin QoS tason 1 **SUBACK** -paketissa. Tämän jälkeen IoT keskittimeen toimittaa viestien käyttämän laitteen QoS 1.

### <a name="additional-considerations"></a>Muita huomioon otettavia seikkoja

Kuin lopullinen vastikkeen, jos haluat mukauttaa cloud-puolen MQTT protokolla-toimintoja Tarkista [Azure IoT protokolla yhdyskäytävän] [ lnk-azure-protocol-gateway] , jonka avulla voit ottaa käyttöön, sillä suoraan IoT toiminnosta on tehokas mukautettu protokolla yhdyskäytävän. Azure IoT protokolla yhdyskäytävän avulla voit mukauttaa sopimaan brownfield MQTT ominaisuuksissa laite-protokollan tai muita mukautettuja protokollia. Tämän menetelmän edellyttävät, suorittaa ja toimivat mukautettu protokolla-yhdyskäytävä.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [muistiinpanojen MQTT tukevat] [ lnk-mqtt-devguide] Azure IoT keskittimeen Kehitystyökalut-oppaassa.

Saat lisätietoja MQTT protokolla on [MQTT ohjeissa][lnk-mqtt-docs].

Lisätietoja IoT keskittimeen käyttöönoton suunnittelun on seuraavissa artikkeleissa:

- [Azure hyväksytty IoT laitteen luettelon][lnk-devices]
- [Tuen uusia protokollia][lnk-protocols]
- [Tapahtuman keskittimet vertailu][lnk-compare]
- [Skaalaus, HA ja DR][lnk-scaling]

Tutustu tarkemmin IoT keskittimeen ominaisuuksia, katso:

- [Sovelluskehittäjän opas][lnk-devguide]
- [Simuloimalla laitetta, jonka IoT yhdyskäytävän SDK-paketissa][lnk-gateway]

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-mqtt-org]: http://mqtt.org/
[lnk-mqtt-docs]: http://mqtt.org/documentation
[lnk-sample-node]: https://github.com/Azure/azure-iot-sdks/blob/develop/node/device/samples/simple_sample_device.js
[lnk-sample-java]: https://github.com/Azure/azure-iot-sdks/blob/develop/java/device/samples/send-receive-sample/src/main/java/samples/com/microsoft/azure/iothub/SendReceive.java
[lnk-sample-c]: https://github.com/Azure/azure-iot-sdks/tree/master/c/iothub_client/samples/iothub_client_sample_mqtt
[lnk-sample-csharp]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device/samples
[lnk-sample-python]: https://github.com/Azure/azure-iot-sdks/tree/master/python/device/samples
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/readme.md
[lnk-sas-tokens]: iot-hub-devguide-security.md#using-sas-tokens-as-a-device
[lnk-mqtt-devguide]: iot-hub-devguide-messaging.md#notes-on-mqtt-support
[lnk-azure-protocol-gateway]: iot-hub-protocol-gateway.md

[lnk-devices]: https://catalog.azureiotsuite.com/
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-messaging]: iot-hub-devguide-messaging.md
