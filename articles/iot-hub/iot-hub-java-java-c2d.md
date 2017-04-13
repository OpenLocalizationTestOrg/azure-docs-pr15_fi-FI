<properties
    pageTitle="Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen | Microsoft Azure"
    description="Katso tämä opetusohjelma lisätietoja Azure IoT keskittimeen käyttäminen Java cloud laitteen viestien lähettämiseen."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-java"></a>Opetusohjelma: Miten cloud laitteen IoT keskittimeen ja Java sisältävien viestien lähettäminen

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on täysin hallittujen palvelu, joka auttaa käyttöön luotettavan ja turvallisen kaksisuuntaisen tietoliikenteen miljoonia IoT laitteiden välillä ja sovelluksen takaisin lopettaa. [IoT keskittimeen käytön aloittaminen] -opetusohjelma näytetään, miten voit luoda IoT-toiminnosta ja valmistella laitteen tunnistetiedot, ei koodin Simuloitu laitteella, joka lähettää laitteen pilveen.

Tässä opetusohjelmassa muodostaa [IoT toiminnosta on]aloittaminen. Se näyttää, miten voit:

- Lähettää cloud laitteen viestien oman sovelluksen cloud taustatietokantaan, yhteen laitteeseen IoT keskittimeen kautta.
- Cloud laitteen virhesanomia laitteessa.
- Sovelluksen cloud takaisin loppuun, pyytää kuittausta toimituksen (*palaute*) lähetetyt laitteeseen IoT-toiminnosta.

Voit etsiä lisätietoja cloud laitteen viesteille [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

Tässä opetusohjelmassa lopussa suorittaa kaksi Java-konsolin sovellusten:

* **Simuloitu laitteen**, luotu [aloittaminen IoT keskittimeen], joka muodostaa yhteyden IoT-toiminnosta ja vastaanottaa cloud laite muokattu sovellusversio.
* **Lähetä-c2d-viestit**, joiden IoT keskittimeen Simuloitu laitteesi cloud laitteen lähettää ja vastaanottaa sen toimituksen kuittaus.

> [AZURE.NOTE] IoT toiminnosta on monta laitteen alustojen ja eri kielten (mukaan lukien C ja Javascript Java) – Azure IoT laitteen SDK: T SDK tuki. Vaiheittaiset ohjeet laitteen yhdistäminen Tässä opetusohjelmassa koodi ja yleensä Azure IoT-toiminnossa on artikkelissa [Azure IoT Developer Center].

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Java Tu 8. <br/> [Paikallinen ympäristö valmistellaan kehittäminen] [ lnk-dev-setup] kerrotaan, miten voit asentaa Javan opetusohjelmassa Windows-tai Linux.

+ Maven-testi 3.  <br/> [Paikallinen ympäristö valmistellaan kehittäminen] [ lnk-dev-setup] maven-testi asentamisesta Windows-tai Linux Tässä opetusohjelmassa kuvataan.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

## <a name="receive-messages-on-the-simulated-device"></a>Vastaanottaa viestejä Simuloitu laitteeseen

Tässä osassa voit muokata Simuloitu laite-sovellus on luotu cloud laitteen virhesanomia IoT-toiminnosta [IoT keskittimeen käytön aloittaminen] .

1. Simulated-device\src\main\java\com\mycompany\app\App.java tiedostoa tekstieditorissa, Avaa.

2. Lisää seuraavat **MessageCallback** luokan sisäkkäisiä luokan **App** luokan sisällä. **Suorita** -menetelmä käynnistetään, kun laite vastaanottaa viestin IoT-toiminnosta. Tässä esimerkissä laitteen aina ilmoittaa valitsemalla sen valmistumista viestin:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Muokkaa **tärkeimmät** menetelmä, jolla **MessageCallback** esiintymän luominen ja kutsua **setMessageCallback** -menetelmää, ennen kuin se avautuu asiakkaan seuraavasti:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Jos käytät HTTP MQTT tai AMQP sijaan kuljetus, **DeviceClient** esiintymän tarkistaa viestien harvoin IoT toiminnosta (enintään 25 minuutin välein). Lisätietoja MQTT, AMQP ja HTTP-tuki ja IoT keskittimeen rajoittimen erot on [IoT keskittimeen Sovelluskehittäjän opas][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Cloud laitteen viestin lähettäminen

Tässä osassa voit luoda Java console-sovellus, joka lähettää cloud laitteen Simuloitu laite-sovellukseen. Tarvitset laite Valitse [IoT keskittimeen käytön aloittaminen] -opetusohjelmassa lisäämääsi laitteen tunnus. Lisäksi tarvitset yhteysmerkkijonon oman IoT-toiminnossa voit etsiä [Azure portal].

1. Luo maven-testi projektin nimi **Lähetä-c2d-viestit** käyttämällä komentokehotteeseen seuraava komento. Huomautus: Tämä on yksi, pitkä komento:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komentorivi-ikkuna, siirry Lähetä-c2d-viestit uusi kansio.

3. Tekstieditorissa, Avaa pom.xml tiedoston Lähetä-c2d-viestit-kansion ja lisää seuraavat riippuvuuden **riippuvuudet** -solmu. Lisäämällä riippuvuuden avulla voit käyttää **iothub-java-palvelun-asiakasohjelmaa** paketin sovelluksen IoT keskittimeen-palvelun yhteydessä:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Tallenna ja sulje pom.xml-tiedosto.

5. Send-c2d-messages\src\main\java\com\mycompany\app\App.java tiedostoa tekstieditorissa, Avaa.

6. Lisää seuraavat **Tuo** lauseet tiedosto:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Lisää seuraavat luokan tason muuttujat **App** luokan tilalle **{yourhubconnectionstring}** ja **{yourdeviceid}** arvot oman merkille aiemmin:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQP;
    ```
    
8. **Tärkeimmät** menetelmä korvaa seuraava koodi, joka muodostaa yhteyden IoT-toiminnosta ja lähettää viestin laitteen odottaa, että laitteen vastaanotettu ja käsitellyt viestin kuittauksen:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Tässä opetusohjelmassa Toteuta uudelleen kaikki käytännöt yksinkertaisuuden 's sake. Tuotannon koodi olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten eksponentiaalisen backoff), yritä käytännöt.

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteeseen-Simuloitu laitteen kansioon Suorita seuraava komento lähettämisen telemetriatietojen IoT-toiminnosta ja kuunnella cloud laitteen lähetetyt oman toiminnosta:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Suorita Simuloitu laite-sovellus][img-simulated-device]

2. Komentokehotteeseen – Lähetä-c2d-viestit kansioon Suorita seuraava komento cloud laitteen sähköpostin ja odota palautetta acknowledgment:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Suorita-komento cloud laitteen viestin lähettäminen][img-send-command]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit, miten cloud laitteen viestien lähettämiseen ja vastaanottamiseen. 

Esimerkkejä valmis lopusta loppuun-ratkaisuja, jotka käyttävät IoT keskittimeen, on artikkelissa [Azure IoT Suite].

Saat lisätietoja kanssa IoT keskittimeen, ratkaisujen kehittämiseen on [IoT keskittimeen Sovelluskehittäjän opas].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[IoT keskittimeen käytön aloittaminen]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[IoT keskittimeen Sovelluskehittäjän opas]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/