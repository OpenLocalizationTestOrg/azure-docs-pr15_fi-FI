<properties
    pageTitle="Azure IoT-toiminnossa Java aloittaminen | Microsoft Azure"
    description="Azure IoT keskittimeen kanssa Java käytön aloittaminen opetusohjelma. Toteuta asioita Internet-ratkaisun avulla Azure IoT keskittimeen ja Java Microsoft Azure IoT SDK: T."
    services="iot-hub"
    documentationCenter="java"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/11/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-java"></a>Azure IoT keskittimeen Java käytön aloittaminen

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Tässä opetusohjelmassa lopussa on kolme Java console-sovellusta:

* **Luo-laite-tunnistetiedot**, joka luo laitteen käyttäjätieto- ja liittyvät suojaustunnuksen yhteyden muodostamiseksi Simuloitu laitteen.
* **luku-d2c-viestit**, jotka näkyvät Simuloitu laitteen lähettämä telemetriatietojen.
* **Simuloitu laite**, joka muodostaa yhteyden IoT keskittimeen aiemmin luotu laitteen käyttäjätietoja ja lähettää telemetriatietojen viestin, joka toinen käyttämällä AMQP-protokollaa.

> [AZURE.NOTE] Artikkelin [IoT keskittimeen SDK: T] [ lnk-hub-sdks] tietoja siitä, eri SDK: T, joiden avulla voit luoda molemmat sovellukset toimimaan laitteet ja ratkaisu takaisin päässä.

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Java Tu 8. <br/> [Paikallinen ympäristö valmistellaan development] [ lnk-dev-setup] kerrotaan, miten voit asentaa Javan opetusohjelmassa Windows-tai Linux.

+ Maven-testi 3.  <br/> [Paikallinen ympäristö valmistellaan kehittäminen] [ lnk-dev-setup] maven-testi asentamisesta Windows-tai Linux Tässä opetusohjelmassa kuvataan.

+ Azure active tili. (Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin] [ lnk-free-trial] -muutamaan minuuttiin.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Viimeisessä vaiheessa kuin muistiin **perusavaimen** arvoa, ja valitse sitten **viestit**. **Viestit** , sivu- **tapahtumaa-toiminnossa yhteensopivan nimi** ja **tapahtumaa-toiminnossa yhteensopivan päätepisteen**muistiin. Tarvitset nämä kolme arvoa, kun luot **luku-d2c-viestit** -sovelluksen.

![Azure portaalin IoT keskittimeen viestintä-sivu][6]

Olet nyt luonut IoT-toiminnosta ja sinulla on IoT keskittimeen isäntänimi, IoT keskittimeen yhteysmerkkijonon, IoT keskittimeen perusavaimen, tapahtumaa-toiminnossa yhteensopivan nimi ja tapahtumaa-toiminnossa yhteensopivan päätepisteen, sinun on suoritettava tässä opetusohjelmassa.

## <a name="create-a-device-identity"></a>Laitteen käyttäjätietojen luominen

Tässä osassa voit luoda Java console-sovellus, joka luo laitteen uusien käyttäjätietojen IoT-toiminnossa identity-rekisterissä. Laite ei voi muodostaa yhteyttä IoT keskittimeen, paitsi jos siinä on merkintä laitteen tunnistetietojen rekisterissä. **Laitteen tunnistetietojen rekisterin** -osassa [IoT keskittimeen Sovelluskehittäjän opas] [ lnk-devguide-identity] lisätietoja. Kun suoritat tämä konsolisovellus, se luo laitteen yksilöivä tunnus ja -laitteen avulla voit tunnistaa itse, kun se lähettää laitteen cloud viestien IoT keskittimeen avaimen.

1. Luo uusi tyhjä kansio nimeltä iot java-get-käynnistettävät. Iot java-get-käynnistettävät-kansioon Luo uusi maven-testi projekti nimeltä **luominen-laitteella – identity** -komentokehotteeseen seuraava komento käyttämällä. Huomautus: Tämä on yksi, pitkä komento:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komentorivi-ikkuna, siirry uuden kansion luominen-laite-tunnistetiedot.

3. Tekstieditorissa, Avaa pom.xml-tiedoston luominen-laite-tunnistetiedot-kansion ja lisää seuraavat riippuvuuden **riippuvuudet** -solmu. Voit käyttää iothub-palvelu-sdk-paketti-sovelluksessa:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.9</version>
    </dependency>
    ```
    
4. Tallenna ja sulje pom.xml-tiedosto.

5. Create-device-identity\src\main\java\com\mycompany\app\App.java tiedostoa tekstieditorissa, Avaa.

6. Lisää seuraavat **Tuo** lauseet tiedosto:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Lisää seuraavat luokan tason muuttujat **App** luokan korvaa **{yourhubconnectionstring}** arvolla-että merkille aiemmin:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    
    ```
    
8. Muokkaa allekirjoitusta **tärkeimmät** menetelmän poikkeuksien seuraavasti:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Lisää seuraava koodi **ensisijainen** tapa tekstinä. Koodi luo kutsutaan *javadevice* IoT keskittimeen tunnistetietojen rekisteriä, jos laitetta ei ole vielä käytössä. Se näyttää sitten laitetunnus ja avainta, joka on uudempi:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Tallenna ja sulje App.java-tiedosto.

11. **Luo laitteen – identity** -sovelluksesta valitsemalla maven-testi luonnissa Suorita seuraava komento komentokehotteeseen-Luo laitteen – identity-kansiossa:

    ```
    mvn clean package -DskipTests
    ```

12. Käyttämällä maven-testi **Luo laitteen – identity** -sovelluksen käyttämiseen, suorita seuraava komento komentokehotteeseen – Luo laitteen – identity-kansiossa:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Pane merkille **laitteen tunnus** ja **laitteen avainta**. Tarvitset luodessasi sovellus, joka muodostaa yhteyden IoT keskittimeen laite-nämä myöhemmin.

> [AZURE.NOTE] IoT keskittimeen tunnistetietojen rekisterin tallentaa vain laitteen käyttäjätietojen käyttöön valitsemalla suojattu käyttö. Tallennetaan laitteen tunnukset ja näppäimet, jotta voit käyttää suojausvaltuudet ja käytössä/poissa käytöstä-merkinnän, voit poistaa käytöstä yksittäisiä laitteen. Jos sovellus täytyy tallentaa laitekohtaiset muita metatietoja, se kannattaa käyttää sovelluksen kielikohtaiset-kaupan. [IoT keskittimeen Developer] oppaassa[ lnk-devguide-identity] lisätietoja.

## <a name="receive-device-to-cloud-messages"></a>Laitteen cloud virhesanomia

Tässä osassa voit luoda Java console-sovellus, jossa lukee laitteen cloud viestien IoT-toiminnosta. IoT-toiminnossa paljastaa [Tapahtumaa-toiminnossa][lnk-event-hubs-overview]-yhteensopiva päätepisteen, jotta voit laitteen cloud viestien lukeminen. Pidä asiat yksinkertainen Tässä opetusohjelmassa Luo basic lukuohjelmaa, joka ei sovi hyvin siirtonopeuden käyttöönottoa varten. [Prosessin laitteen cloud viestien] [ lnk-process-d2c-tutorial] opetusohjelmassa näytetään, miten voit käsitellä laitteen cloud viestit tasolla. [Tapahtuman keskittimet aloittaminen] [ lnk-eventhubs-tutorial] opetusohjelma tietoja edelleen prosessin viesteihin keskittimet tapahtuma- ja soveltuu IoT keskittimeen tapahtuman keskittimeen yhteensopivan päätepisteet.

> [AZURE.NOTE] Tapahtuman keskittimeen yhteensopivan päätepisteen laitteen cloud viestien lukeminen aina käyttää AMQP-protokollaa.

1. Luo uusi maven-testi projektin nimi-komentokehotteeseen seuraava komento käyttämällä **luku-d2c-viestit** iot java-get-käynnistettävät kansio, jonka loit *Luo laitteen käyttäjätiedot* -kohdassa. Huomautus: Tämä on yksi, pitkä komento:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komentorivi-ikkuna, siirry luku-d2c-viestit uusi kansio.

3. Tekstieditorissa, Avaa pom.xml tiedoston luku-d2c-viestit-kansion ja lisää seuraavat riippuvuuden **riippuvuudet** -solmu. Näin voit lukea tapahtumaa-toiminnossa yhteensopivan päätepisteen sovelluksen eventhubs asiakas-paketin avulla:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.8.2</version> 
    </dependency>
    ```

4. Tallenna ja sulje pom.xml-tiedosto.

5. Read-d2c-messages\src\main\java\com\mycompany\app\App.java tiedostoa tekstieditorissa, Avaa.

6. Lisää seuraavat **Tuo** lauseet tiedosto:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Lisää seuraavat luokan tason muuttujat **sovelluksen** -luokka. Korvaa aiemmin merkitsemäsi arvot **{youriothubkey}**, **{youreventhubcompatibleendpoint}**ja **{youreventhubcompatiblename}** :

    ```
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Lisää **sovellus** -luokan **receiveMessages** seuraavasti. Tämä menetelmä luo **EventHubClient** esiintymä muodostaa tapahtumaa-toiminnossa yhteensopivan päätepisteen ja sitten asynkronisesti **PartitionReceiver** -esiintymän lukemaan tapahtumaa-toiminnossa-osiosta. Se silmukat jatkuvasti ja tulostaa viestin tiedot, ennen kuin se päättyy.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive(100).get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Tämä menetelmä käyttää suodattimen luodessaan vastaanotin niin, että vastaanottaja lukee vain vastaanotin käynnistyttyä käynnissä IoT keskittimeen lähetettyihin viesteihin. Tästä on hyötyä testiympäristössä, jotta näet nykyisen joukon sanomia. Tuotantoympäristössä koodin Varmista, että se käsittelee kaikki viestit - kohdassa [IoT keskittimeen laitteen cloud viestien käsitteleminen] [ lnk-process-d2c-tutorial] opetusohjelman lisätietoja.

9. Muokkaa allekirjoitusta **tärkeimmät** menetelmän sisältämään poikkeuksen seuraavasti:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Lisää seuraava koodi **sovelluksen** luokan **tärkeimmät** menetelmää. Tämä luo kaksi **EventHubClient** ja **PartitionReceiver** esiintymät ja avulla voit sulkea sovelluksen, kun olet tehnyt käsitellään viestejä:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] Tämä koodi oletetaan, että luomasi IoT-toiminnossa F1 (maksuton) taso. Vapaa IoT-toiminnossa on kaksi osiota nimeltä "0" ja "1".

11. Tallenna ja sulje App.java-tiedosto.

12. Jos haluat luoda **luku-d2c-viestit** -sovelluksesta valitsemalla maven-testi, suorita seuraava komento komentokehotteeseen-luku-d2c-viestit-kansion:

    ```
    mvn clean package -DskipTests
    ```

## <a name="create-a-simulated-device-app"></a>Simuloitu laitteen sovelluksen luominen

Tässä osassa Java konsolin sovelluksen laitteella, joka lähettää laitteen cloud IoT keskittimeen tulokset vastaavat luominen.

1. Luo uusi maven-testi projektin nimi **Simuloitu laitteen** käyttämällä komentokehotteeseen seuraava komento iot java-get-käynnistettävät kansio, jonka loit *laitteen käyttäjätietojen* luominen. Huomautus: Tämä on yksi, pitkä komento:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komentorivi-ikkuna, siirry Simuloitu laitteen uusi kansio.

3. Tekstieditorissa, pom.xml-tiedoston avaaminen Simuloitu laitteen-kansioon ja lisää seuraavat riippuvuudet **riippuvuudet** -solmu. Voit käyttää iothub java-asiakas-paketin sovelluksen pitää yhteyttä IoT-toiminnosta ja onnistu Java-objektien JSON:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.14</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Tallenna ja sulje pom.xml-tiedosto.

5. Simulated-device\src\main\java\com\mycompany\app\App.java tiedostoa tekstieditorissa, Avaa.

6. Lisää seuraavat **Tuo** lauseet tiedosto:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import com.google.gson.Gson;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Lisää seuraavat luokan tason muuttujat **App** luokan IoT keskittimeen nimesi ja *laitteen käyttäjätietojen luominen* -kohdassa Luo laitteen avaimen arvo **{yourdevicekey}** **{youriothubname}** tilalle:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQP;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```

    Tämän sovelluksen malli käyttää **protokolla** -muuttuja, kun se muodostaa **DeviceClient** objekti. Voit pitää yhteyttä IoT keskittimeen tai AMQP HTTP-protokolla.

8. Lisää seuraavat sisäkkäinen **TelemetryDataPoint** luokan, voit määrittää laitteen lähettää IoT keskittimeen telemetriatietojen tiedot **sovelluksen** luokan sisällä:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Lisää seuraavat sisäkkäisiä **EventCallback** -luokka, kuittaus tilan, joka palauttaa IoT-toiminnossa, kun käsittelee viestin Simuloitu laitteesta näkyviin **sovelluksen** luokan sisällä. Tämä menetelmä ilmoittaa sovelluksen tärkeimmät viestiketjun myös, kun viesti on käsitelty:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Lisää seuraavat sisäkkäisiä **MessageSender** -luokka, **sovelluksen** luokan sisällä. Tähän luokkaan **Suorita** -menetelmä luo IoT-toiminnossa lähettäminen telemetriatietojen mallitiedot ja odottaa vastausta ennen lähettämistä seuraavan sanoman:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;
      
      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.windSpeed = currentWindSpeed;
            
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println("Sending: " + msgStr);
            
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
            
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Tämä menetelmä lähettää viestiin laitteen cloud sekunnin jälkeen IoT-toiminnossa hyväksyy sen edellisen viestin. Viesti sisältää JSON muuntaa sarjaksi objektia, jonka deviceId ja satunnaisluvun voit simuloida tuulen nopeus tunnistin.

11. Korvaa seuraava koodi, joka luo laitteen cloud viestien lähettämiseen IoT keskittimeen säikeen **tärkeimmät** menetelmää:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();

      MessageSender sender = new MessageSender();

      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);

      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.close();
    }
    ```

12. Tallenna ja sulje App.java-tiedosto.

13. Luoda **Simuloitu laitteen** sovelluksen maven-testi, suorita seuraava komento komentokehotteen-Simuloitu laitteen kansioon:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Tässä opetusohjelmassa avulla voit jatkaa samaa yksinkertainen, Toteuta uudelleen kaikki käytännöt. Tuotannon koodissa olisi Toteuta kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-sivuston artikkelissa (kuten Eksponentiaalinen backoff), yritä käytännöt[lnk-transient-faults].

## <a name="run-the-applications"></a>Suorita sovellukset

Olet nyt valmiina sovelluksia.

1. Komentokehotteeseen-luku-d2c-kansiossa Suorita seuraava komento aloittaa seurannan ensimmäinen osio IoT-toiminnossa:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT keskittimeen palvelun asiakassovellus laitteen cloud viestien seuranta][7]

2. Komentokehotteeseen-Simuloitu laitteen kansioon Suorita seuraava komento aloittaa telemetriatietoja lähettäminen IoT-toiminnossa:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT keskittimeen laitteen asiakassovellus laitteen cloud viestien lähettäminen][8]

3. **Käyttö** -ruudun [Azure portal] [ lnk-portal] näkyy keskittimeen viestien lukumäärä:

    ![Azure portaalin käyttö ruutu näkyy viestien lukumäärä IoT keskittimeen][43]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa määritetty keskittimeen IoT Azure-portaalissa, ja luo laitteen identity toiminnossa tunnistetietojen rekisterissä. Tämä laite tunnistetietojen avulla laitteen cloud viestien lähettämiseen keskittimeen Simuloitu laite-sovelluksen käyttöön. Voit luoda myös sovellus, joka näyttää keskittimeen vastaanotettujen viestien. 

Voit jatkaa IoT keskittimeen aloittaminen ja tutustu IoT muissa tilanteissa seuraavissa artikkeleissa:

- [Laitteen yhteyden muodostamisessa][lnk-connect-device]
- [Mobiililaitteiden hallinnan käytön aloittaminen][lnk-device-management]
- [IoT yhdyskäytävän SDK: N käytön aloittaminen][lnk-gateway-SDK]

Voit laajentaa IoT ratkaisu ja käsittele laitteen cloud viestejä tasolla on artikkelissa [käsitellä laitteen cloud viestejä] [ lnk-process-d2c-tutorial] opetusohjelma.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/