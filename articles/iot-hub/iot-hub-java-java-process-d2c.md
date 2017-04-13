<properties
    pageTitle="Käsittele IoT keskittimeen laitteen cloud viestejä (Java) | Microsoft Azure"
    description="Katso tämä Java-opetusohjelma, saat hyödyllisiä kuvioiden avulla ja käsittele IoT keskittimeen laitteen cloud viestejä."
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
     ms.date="09/01/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-java"></a>Opetusohjelma: Voit käsitellä IoT keskittimeen laitteen cloud viestiä, joissa Java

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on täysin hallitun palvelu, joka mahdollistaa luotettava ja suojatun kaksisuuntaisen tietoliikenteen miljoonia IoT laitteet ja sovelluksen takaisin end-näppäinyhdistelmää. Muut opetusohjelmat ([IoT keskittimeen käytön aloittaminen] ja [cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen][lnk-c2d]), kuinka voit käyttää pilvipalveluun laitteen ja cloud laitteen tekstiviesti perustoimintoja IoT toiminnossa.

Tässä opetusohjelmassa muodostaa [IoT keskittimeen käytön aloittaminen] -opetusohjelmassa esitetyn koodin, ja se näkyy kaksi skaalattava kuvioita, joiden avulla voit käsitellä laitteen cloud viestit:

- Laitteen cloud viestien [Azure-blob-säiliö]luotettava tallennustilaan. Käytetty vaihtoehto on *kylmän polku* analytics, johon tallennat telemetriatietojen tietojen BLOB käytettävä analytics prosessit-syötteestä. Nämä prosessit voivat perustuva varten työkaluilla, kuten [Azure Data Factory] tai [HDInsight (Hadoop)] -pino.

- *Vuorovaikutteinen* laitteen cloud viestien luotettava käsittely. Laitteen cloud viestit ovat vuorovaikutteisia, kun ne ovat heti käynnistimet sovelluksen takaisin loppuun toimintojoukon. Esimerkiksi laitteen voi lähettää hälytys-viestin, joka käynnistää lippu lisäämisestä CRM-järjestelmään. Sen sijaan *arvopiste* viestien syötteen yksinkertaisesti analytics-ohjelma. Lämpötilan telemetriatietojen laitteesta, joka on tallennettu myöhempää käyttöä varten on esimerkiksi arvopiste viesti.

Koska IoT keskittimeen paljastaa [Tapahtumaa-toiminnossa][lnk-event-hubs]-yhteensopiva päätepiste vastaanottaa laitteen cloud viestejä, tämän opetusohjelman käyttää [EventProcessorHost] esiintymä. Tämä esiintymä:

* Tallentaa luotettavasti Azure-blob-säiliö *arvopiste* viestit.
* Välittää *Vuorovaikutteinen* laitteen cloud viestien Azure [palvelun Bus jonon] heti käsittelyä varten.

Palvelun Bus avulla voit varmistaa luotettava käsittelyn vuorovaikutteinen viestejä, kuten se sisältää viestin kohti tarkistuspisteet ja aikaa ikkunan perustuva jään monistaminen.

> [AZURE.NOTE] Erillisen **EventProcessorHost** on vain yksi tapa käsitellä vuorovaikutteinen viestit. Muut vaihtoehdot ovat [Azure palvelun kangasta] [ lnk-service-fabric] ja [Azure Stream analysoinnin][lnk-stream-analytics].

Tässä opetusohjelmassa lopussa suoritat kolme Java console-sovellusta:

* **Simuloitu laitteen**, loit opetusohjelmassa [aloittaminen IoT toiminnosta] muokattu sovellusversio lähettää arvopiste laitteen cloud viestien jokaisen toisen ja vuorovaikutteinen laitteen cloud viestit 10 sekunnin välein. Sovellus käyttää AMQP-protokollaa pitää yhteyttä IoT toiminnossa.
* **prosessi-d2c-viestit** käyttää [EventProcessorHost] luokan viestien noutaminen tapahtumaa-toiminnossa yhteensopivan päätepiste. Se sitten luotettavasti arvopiste viestit tallennetaan Azure-blob-säiliö ja välittää palvelun Bus jonon vuorovaikutteinen viestejä.
* **prosessi-vuorovaikutteinen-viestit** jonot varaustiedoista palvelun Bus jonossa vuorovaikutteinen viestejä.

> [AZURE.NOTE] IoT toiminnosta on monta laitteen alustojen ja eri kielellä, esimerkiksi C Java ja JavaScript SDK tuki. Ohjeita siitä, miten voit korvata Simuloitu laitteen fyysinen laite ja laitteiden yhdistämisestä IoT-toiminnossa Tässä opetusohjelmassa artikkelissa [Azure IoT Developer Center].

Tässä opetusohjelmassa sovelletaan suoraan muita tapoja käyttää tapahtumaa-toiminnossa yhteensopivan viestejä, kuten [HDInsight (Hadoop)] -projektit. Lisätietoja [Azure IoT keskittimeen developer guide - laitteen pilveen].

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

+ Täydellinen versio toimii [IoT keskittimeen käytön aloittaminen] -opetusohjelman.

+ Java Tu 8. <br/> [Paikallinen ympäristö valmistellaan development] [ lnk-dev-setup] kerrotaan, miten voit asentaa Javan opetusohjelmassa Windows-tai Linux.

+ Maven-testi 3.  <br/> [Paikallinen ympäristö valmistellaan kehittäminen] [ lnk-dev-setup] maven-testi asentamisesta Windows-tai Linux Tässä opetusohjelmassa kuvataan.

+ Azure active tili. <br/>Jos sinulla ei ole tiliä, voit luoda [ilmaisen tilin](https://azure.microsoft.com/free/) muutamaan minuuttiin.

Jotkin basic kokemusta [Azuren tallennustilaan] ja [Azure palvelun Bus]pitäisi olla.


## <a name="send-interactive-messages-from-a-simulated-device"></a>Vuorovaikutteinen lähettämiseen Simuloitu laitteesta

Tässä osassa voit muokata Simuloitu laitteen sovelluksen luomasi vuorovaikutteinen laitteen cloud viestien lähettämiseen IoT keskittimeen [IoT keskittimeen käytön aloittaminen] -opetusohjelmassa.

1. Käytä tekstieditorissa simulated-device\src\main\java\com\mycompany\app\App.java tiedoston avaamiseen. Tiedoston nimi sisältää **Simuloitu laitteen** sovelluksen [IoT keskittimeen käytön aloittaminen] -opetusohjelma on luotu koodi.

2. Lisää seuraavat sisäkkäisiä luokan **App** luokan:

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Tähän luokkaan muistuttaa **Simuloitu laitteen** projektin **MessageSender** -luokka. Vain erot on, että olet määrittänyt nyt **MessageId** järjestelmä-ominaisuutta ja mukautetun ominaisuuden nimi **messageType**.
    Koodi määrittää yksilöllinen tunnus (UUID) **MessageId** -ominaisuutta. Palvelun Bus avulla tunnus Poista monista se vastaanottaa viestejä. Otosten erottaa vuorovaikutteinen tietojen pisteen viesteistä **messageType** -ominaisuuden avulla. Sovelluksen määritysvaiheisiin viestin ominaisuudet, tämä tieto sen sijaan, että viestin tekstiosaan niin, että tapahtuma-suoritin ei tarvitse poistaa viestin suorittaa viestin reititys.

    > [AZURE.NOTE] On tärkeää luoda **MessageId** kaksoiskappaleet varaustiedoista vuorovaikutteinen viestien laite-koodin avulla. Katkonainen tietoliikenne tai muita epäonnistuu saattaa johtaa useita uudelleenlähetysten saman viestin kyseisen laitteesta. Voit käyttää myös semanttinen viestin tunnuksen, kuten hash haluamasi viesti-tietokenttiä UUID sijaan.

3. Muokkaa **tärkeimmät** menetelmä vuorovaikutteinen sekä viestien lähettäminen ja tiedot-kohdan viestien seuraavat koodikatkelman esitetyllä tavalla:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Tallenna ja sulje simulated-device\src\main\java\com\mycompany\app\App.java-tiedosto.

    > [AZURE.NOTE] Tässä opetusohjelmassa Toteuta yksinkertaisuuden, yritä kaikki käytännöt. Tuotannon koodi olisi Toteuta uudelleen käytäntö, kuten eksponentiaalisen backoff kuin ehdotettua [Lyhytkestoisia vika käsittely]MSDN-artikkelista.

5. Luoda **Simuloitu laitteen** sovelluksen maven-testi, suorita seuraava komento komentokehotteen-Simuloitu laitteen kansioon:

    ```
    mvn clean package -DskipTests
    ```

## <a name="process-device-to-cloud-messages"></a>Käsittele laitteen cloud viestit

Tässä osassa voit luoda Java console-sovellus, joka käsittelee laitteen cloud viestien IoT-toiminnosta. IOT keskittimeen paljastaa [tapahtumaa-toiminnossa]-yhteensopiva päätepisteen ottaa käyttöön sovelluksen laitteen cloud viestien lukeminen. Tässä opetusohjelmassa käytetään [EventProcessorHost] luokan Käsittele kyseisiä viestejä console-sovelluksessa. Lisätietoja siitä, miten prosessi viesteihin-tapahtuman keskittimet on artikkelissa [Tapahtuman keskittimet käytön aloittaminen] -opetusohjelma.

Tärkeimmät todennus, kun Toteuta luotettava tallennustilan arvopiste viestejä tai vuorovaikutteinen viestien välittäminen on käsittely riippuvainen viestin kuluttaja tarkistuspisteet antamaan etenemisen. Lisäksi saavuttamiseksi suuren siirtonopeuden, kun luetaan siis tapahtuman keskittimet pitäisi sisältää tarkistuspisteet suuri erissä. Tämä menetelmä luo vahinkojen kaksoiskappaleet käsitellään saat paljon viestejä, jos on ilmennyt virhe ja voit palata edelliseen tarkistuspiste. Tässä opetusohjelmassa näet, miten Azuren tallennustilaan kirjoituksia ja palvelun Bus jään monistaminen windows synkronointiin **EventProcessorHost** tarkistuspisteet.

Kirjoittaa viestejä Azuren tallennustilaan luotettavasti, otosten käyttää yksittäisiä estä Vahvista-ominaisuus, [Estä BLOB][Azure Block Blobs]. Tapahtuman käsittelijä kasvaa viestien muistiin, kunnes se on jatkuva tarkistuspisteen. Viestien kertynyt puskurin saavuttaa 4 Mt suurimman lohkon koon, tai jälkeen-palvelun Bus jään monistaminen aikaikkunan esimerkiksi kuluu. Valitse ennen tarkistuspiste-koodin tallentaa uuden kohteen blob.

Tapahtuman käsittelijä käyttää tapahtuman keskittimet viestin siirtymät palkkina tunnukset. Tämä järjestelmä mahdollistaa tapahtuma-suoritin suorittaa jään monistaminen tarkistuksen, ennen kuin se tallentaa uuden kohteen tallennustilaa, on mahdollista kaatumisen välillä vahvistetaan lohko ja tarkistuspiste varoen.

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään yksittäisen Azure-tallennustilan tilin kirjoittaa kaikki viestit, jotka on haettu IoT-toiminnosta. Päätä, jos haluat käyttää useita Azuren tallennustilaan tilejä ratkaisu, on artikkelissa [Azure-tallennustilan skaalattavuus ohjeita].

Sovellus käyttää palvelun Bus jään monistaminen ominaisuus välttämiseksi kaksoiskappaleet, kun se käsittelee vuorovaikutteinen viestejä. Simuloitu laitteen aikaleimaa vuorovaikutteinen viesti yksilöllisiä **MessageId**kanssa. Tämä tunnus palvelun Bus varmistaa, että määritetyn jään monistaminen aika-ikkunassa ei ole kaksi viestit, joissa on sama **MessageId** toimitetaan vastaanottajat. Jään kopiot, ja viestin kohti valmistumista semantiikkaan-liittyvien palvelun Bus olevien myöntämä toteuttamisesta vuorovaikutteinen viestien luotettava käsittely on helppoa.

Koodin synkronointi **EventProcessorHost** tarkistuspiste järjestelmä voit varmistaa, että ei viesti on lähetetty uudelleen jään monistaminen ikkunan ulkopuolella, palvelun Bus jono jään monistaminen-ikkuna. Tämä synkronointi tapahtuu pakottamalla tarkistuspisteen vähintään kerran, joka kerta, kun jään monistaminen ikkunan kuluu (Tässä opetusohjelmassa ikkuna on tunnin).

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään yksittäisen osioitua palvelun Bus-jonossa prosessin vuorovaikutteinen viestit, jotka on haettu IoT-toiminnosta. Lisätietoja opit käyttämään skaalattavuus ehtojen mukaan, että palvelun Bus olevien [Azure palvelun Bus] ohjeissa.

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>Azure-tallennustilan tilin ja palvelun Bus jonon valmistelu

Käyttämään [EventProcessorHost] -luokka on oltava Azure-tallennustilan tilin käyttöön **EventProcessorHost** tarkistuspisteen tietoja. Voit käyttää aiemmin luodun Azure-tallennustilan tilin tai noudata [Azuren tallennustilaan] voit luoda uuden. Pane merkille Azure-tallennustilan tilin yhteysmerkkijonon.

> [AZURE.NOTE] Kun kopioit ja liität Azure-tallennustilan tilin yhteysmerkkijonon, varmista, että osoitteessa ei ole levyä välilyöntejä.

Sinun on myös palvelun Bus jono käyttöön luotettava käsittelyn vuorovaikutteinen viestejä. Voit luoda jonon ohjelmallisesti tunnin jään monistaminen-ikkunassa [palvelun Bus olevien käyttämisestä][palvelun Bus jonon]artikkelissa kuvatulla tavalla. Vaihtoehtoisesti voit käyttää [Azure perinteinen portal][lnk-classic-portal], toimimalla seuraavasti:

1. Valitse **Uusi** vasemmassa alakulmassa. Valitse **Sovelluksen palvelut** > **Palvelun Bus** > **jonossa** > **Mukautettu luominen**. Kirjoita nimi- **d2ctutorial**, alueen, valitse ja käytä aiemmin nimitilan tai luoda uuden. Nimitilan nimi muistiin, sinulla on oltava se jäljempänä tässä opetusohjelmassa. Valitse seuraavalla sivulla **käyttöön kaksoiskappaleiden**ja asettaminen tunnin **kaksoiskappaleiden tunnistus historia aika-ikkuna** . Valitse oikeassa alakulmassa Tallenna jonon kokoonpanosi valintamerkki.

    ![Luo jono Azure-portaalissa][30]

2. Palvelun Bus olevien ‑luettelosta **d2ctutorial**ja valitse sitten **Määritä**. Luo kaksi jaettuun käyttöön käytännöt, yksi kutsuttu **Lähetä** **Lähetä** oikeuksilla ja yksi kutsuttu **kuunnella** **kuunnella** käyttöoikeuksilla. Huomioi **perusavaimen** arvot sekä käytäntöjä, tarvitset niitä myöhemmin: Tässä opetusohjelmassa. Kun olet valmis, valitse **Tallenna** alaosassa.

    ![Määritä jonossa Azure-portaalissa][31]

### <a name="create-the-event-processor"></a>Luo tapahtuma-suoritin

Tässä osiossa luot Java sovelluksen prosessin viesteihin tapahtumaa-toiminnossa yhteensopivan päätepisteestä.

Ensimmäiseksi on lisättävä maven-testi projektin nimi, joka vastaanottaa laitteen cloud viestien IoT keskittimeen tapahtuman keskittimeen yhteensopivan päätepisteestä ja reitittää sanomia taustatietokantaan muihin palveluihin **prosessi-d2c-viestit** .

1. Luo iot java-get-käynnistettävät kansio, jonka loit [IoT keskittimeen käytön aloittaminen] -opetusohjelma, maven-testi projektin nimi **prosessi-d2c-viestit** käyttämällä komentokehotteeseen seuraava komento. Huomautus: Tämä on yksi, pitkä komento:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komentorivi-ikkuna, siirry kansioon prosessi-d2c-viestit.

3. Tekstieditorissa, Avaa pom.xml tiedoston prosessi-d2c-viestit-kansion ja lisää seuraavat riippuvuudet **riippuvuudet** -solmu. Riippuvuussuhteita avulla voit käyttää azure eventhubs, azure-eventhubs-eph ja azure servicebus pakettien sovelluksen vuorovaikutuksessa IoT keskittimeen ja palvelun Bus jonossa:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Tallenna ja sulje pom.xml-tiedosto.

Seuraava tehtävä on **ErrorNotificationHandler** -luokan lisääminen projektiin.

1. Luo process-d2c-messages\src\main\java\com\mycompany\app\ErrorNotificationHandler.java tiedostoa tekstieditorissa avulla. Lisää virhesanomat **EventProcesssorHost** esiintymästä tiedosto seuraava koodi:

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Tallenna ja sulje ErrorNotificationHandler.java-tiedosto.

Voit nyt lisätä luokka, joka toteuttaa **IEventProcessor** -liittymän. **EventProcessorHost** luokan soittaa tähän luokkaan käsittelemään laitteen cloud-viestejä, jotka on vastaanotettu IoT-toiminnosta. Tähän luokkaan koodin toteuttaa logiikan viestien tallentaminen luotettavasti blob-säilö ja palvelun Bus jonossa vuorovaikutteinen viestien edelleenlähetys.

**OnEvents** -menetelmä määrittää **latestEventData** -muuttuja, joka seuraa siirtymä- ja vuorovaikutuksen lukea tapahtuman suorittimen uusimman viestin. Muista, että suoritin vastaa osio. **OnEvents** sitten vastaanottaa viestejä erän IoT-toiminnosta ja käsittelee ne seuraavasti: se vuorovaikutteinen viestit lähetetään palvelun Bus jonossa, ja liittää **toAppend** muistin puskuri tietojen pisteen viestejä. Jos muistin puskuri saavuttaa 4 megatavua Estä tai jään monistaminen aina Windowsin kuluu (tunnin kuluttua viimeisen tarkistuspiste Tässä opetusohjelmassa), menetelmä käynnistää tarkistuspisteen.

**AppendAndCheckPoint** menetelmä luo ensin **blockId** liittäminen blob estä. Azure-tallennustilan edellyttää, että kaikki estää tunnuksista on yhtä pitkiksi, joten menetelmä pads etunollat siirtymä. Tämän jälkeen Jos Estä tämän tunnuksen on jo blob-menetelmä korvaa se nykyisen puskurin sisältö.

> [AZURE.NOTE] Tässä opetusohjelmassa käyttää helpottaa koodi yksittäisen blob kohti osion viestejä. Todellinen ratkaisu Toteuta tiedoston juoksevan luomalla uusia tiedostoja jälkeen tietyn ajan tai kun ne saavuttaa tietyn koon summan. Muista, että estä Azure-blob sisältää enintään 195 gt tietoja.

Seuraava tehtävä on Toteuta **IEventProcessor** liittymää:

1. Luo process-d2c-messages\src\main\java\com\mycompany\app\EventProcessor.java tiedostoa tekstieditorissa avulla.

2. Lisää seuraavat tuonnin ja luokan määritys EventProcessor.java-tiedostoon. **EventProcessor** luokan toteuttaa **IEventProcessor** -liittymän, joka määrittää tapahtuman keskittimet asiakkaan:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Lisää **EventProcessor** luokan Toteuta **IEventProcessor** liittymää seuraavista tavoista:

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Lisää seuraavat luokan tason muuttujat **EventProcessor** luokkaan:

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Lisää **AppendAndCheckPoint** -menetelmä, jolla seuraava allekirjoitus **EventProcessor** luokkaan:

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Lisää seuraava koodi hakea nykyisen viestin siirtymä- ja sarjan numero-osion **AppendAndCheckPoint** -menetelmää:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. Luo **BlockEntry** esiintymä, saat seuraavan aikalohkon tallentamaan blob **AppendAndCheckPoint** -menetelmä siirtymä nykyarvo avulla:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. Lataa uusimmat viestit joukko estäminen Blob-objektien **AppendAndCheckPoint** -menetelmä ja noutaa lohkot nykyinen luettelo:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. **AppendAndCheckPoint** tapaa luoda alkuperäisen Estä uusi Blob-objektien tai lohkon liittää aiemmin Blob-objektien:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Lopuksi **AppendAndCheckPoint** tapaa Luo tarkistuspisteen osioon, ja Tallenna viestit seuraavan aikalohkon valmisteleminen:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. Lisää seuraava koodi vastaanottamaan viestejä IoT keskittimeen päätepisteen ja vuorovaikutteinen viestien edelleenlähetys palvelun Bus jonossa **onEvents** -menetelmää. Valitse puhelun **AppendAndCheckPoint** menetelmä kun esto on täynnä tai aikakatkaisun vanhenee:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Lisää lopuksi Soita **AppendAndCheckPoint** , jos aikakatkaisun päättyy, kun viestejä ei ole tulossa IoT-toiminnosta 'Muuten jos-lauseen **onEvents** -menetelmä:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Tallenna muutokset EventProcessor.java-tiedostoon.

Viimeisessä vaiheessa **prosessi-d2c-viestit** projekti on koodin lisääminen **tärkeimmät** menetelmää, joka muodostaa **EventProcessorHost** esiintymä.

1. Käytä tekstieditorissa process-d2c-messages\src\main\java\com\mycompany\app\App.java tiedoston avaamiseen.

2. Lisää seuraavat **Tuo** lauseet tiedosto:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Lisää seuraavan tason luokan muuttujan **sovelluksen** -luokka. Korvaa **{yourstorageaccountconnectionstring}** Azure-tallennustilan tilin yhteysmerkkijonon tekemäsi merkille aiemmin [valmistella Azure-tallennustilan tilin ja palvelun Bus jono](#provision-an-azure-storage-account-and-a-service-bus-queue) -osassa:

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Seuraavat luokan tason muuttujat lisääminen **sovelluksen** luokan ja korvaa palvelun Bus nimitilan ja **{yourservicebussendkey}** **{yourservicebusnamespace}** oman jonon **Lähetä** -näppäintä. Tekemäsi muistiinpanon nimitila ja **Kuuntele** avainarvot aiemmin [valmistella Azure-tallennustilan tilin ja palvelun Bus jono](#provision-an-azure-storage-account-and-a-service-bus-queue) -osassa:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Lisää seuraavat luokan tason muuttujat **sovelluksen** -luokka. Korvaa **{youreventhubcompatibleendpoint}** tapahtumaa-toiminnossa yhteensopivan päätepisteen arvo. Päätepisteen arvo näyttää **hänen... nimitilan** , joten sinun on poistettava **sb: / /** etuliite ja jälkiliite **.servicebus.windows.net/** . Vaihda **{youreventhubcompatiblename}** tapahtumaa-toiminnossa yhteensopivan nimi. Korvaa **{youriothubkey}** **iothubowner** -näppäintä. [Luo IoT-toiminnossa] tekemäsi muistiinpanon näistä arvoista[ lnk-create-an-iot-hub] *Azure IoT keskittimeen Java käytön aloittaminen* -opetusohjelma-osassa:

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Muokkaa **ensisijainen** tapa allekirjoituksen seuraavasti:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. Lisää seuraava koodi **tärkeimmät** menetelmä, jolla haluat viitata blob-säilö, joka tallentaa viestit:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. Lisää seuraava koodi **tärkeimmät** menetelmä, jolla haluat viitata palvelun Bus-palveluun:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. **Tärkeimmät** tapaa määrittäminen ja luo **EventProcessorHost** esiintymä. **SetInvokeProcessorAfterReceiveTimeout** -vaihtoehto varmistaa, että **EventProcessorHost** esiintymän käynnistää **onEvents** menetelmän **IEventProcessor** käyttöliittymässä myös silloin, kun viestejä ei ole käyttänyt. **OnEvents** menetelmä aina käynnistää sitten **AppendAndCheckPoint** menetelmä kun aikakatkaisun vanhenee.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. **Tärkeimmät** tapaa rekisteröidä **IEventProcessor** käyttöönoton **EventProcessorHost** esiintymä:

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Logiikan lisääminen lopuksi sulkeutumaan **EventProcessorHost** esiintymän **tärkeimmät** menetelmää:

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Tallenna ja sulje process-d2c-messages\src\main\java\com\mycompany\app\App.java-tiedosto.

13. Voit luoda **prosessi-d2c-viestit** -sovelluksesta valitsemalla maven-testi, suorita seuraava komento komentokehotteen-prosessi-d2c-viestit-kansion:

    ```
    mvn clean package -DskipTests
    ```

## <a name="receive-interactive-messages"></a>Vuorovaikutteinen virhesanomia

Tässä osassa voit kirjoittaa Java console-sovellus, joka vastaanottaa vuorovaikutteinen viestit palvelun Bus jonossa.

Ensimmäiseksi on lisättävä maven-testi projektin nimi, joka vastaanottaa **EventProcessor** esiintymät palvelun Bus jonossa lähettämät viestit **prosessi-vuorovaikutteinen-viestit** .

1. Luo iot java-get-käynnistettävät kansio, jonka loit [IoT keskittimeen käytön aloittaminen] -opetusohjelma, maven-testi projektin nimi-komentokehotteeseen seuraava komento käyttämällä **prosessi-vuorovaikutteinen-viestit** . Huomautus: Tämä on yksi, pitkä komento:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Komentorivi-ikkuna, siirry kansioon prosessi-vuorovaikutteinen-viestit.

3. Tekstieditorissa, Avaa pom.xml tiedoston prosessi-vuorovaikutteinen-viestit-kansion ja lisää seuraavat riippuvuuden **riippuvuudet** -solmu. Tämä riippuvuus avulla voit käsitellä palvelun Bus jonon sovelluksen azure servicebus-paketin avulla:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Tallenna ja sulje pom.xml-tiedosto.

Seuraava tehtävä on lisätä koodin viestien noutaminen palvelun Bus jonossa.

1. Käytä tekstieditorissa process-interactive-messages\src\main\java\com\mycompany\app\App.java tiedoston avaamiseen.

2. Lisää seuraava `import` lauseet tiedoston:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Seuraavat luokan tason muuttujat lisääminen **sovelluksen** luokan ja korvaa palvelun Bus nimitilan ja **{yourservicebuslistenkey}** **{yourservicebusnamespace}** oman jonon **kuunnella** avain. Tekemäsi muistiinpanon nimitila ja **Kuuntele** avainarvot aiemmin [valmistella Azure-tallennustilan tilin ja palvelun Bus jono](#provision-an-azure-storage-account-and-a-service-bus-queue) -osassa:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Lisää seuraavat sisäkkäisiä luokan **App** luokan vastaanottamaan viestejä jonossa:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Muokkaa **ensisijainen** tapa allekirjoituksen seuraavasti:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. **Tärkeimmät** tavan Lisää seuraava koodi Aloita Kuuntele uudet viestit:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Tallenna ja sulje process-interactive-messages\src\main\java\com\mycompany\app\App.java-tiedosto.

8. Voit luoda **prosessi-vuorovaikutteinen-viestit** -sovelluksesta valitsemalla maven-testi, suorita seuraava komento komentokehotteen-prosessi-vuorovaikutteinen-viestit-kansion:

    ```
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Suorita sovellukset

Nyt olet valmis suorittamaan kolme sovellusta.

1.  **Prosessi-vuorovaikutteinen-viestit** -sovelluksen käyttämiseen komentokehote tai shell Selaa kansioon, prosessi-vuorovaikutteinen-viestit ja suorita seuraava komento:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Suorita prosessi-vuorovaikutteinen-viestit][processinteractive]

2.  **Prosessi-d2c-viestit** -sovelluksen käyttämiseen komentokehote tai shell Selaa kansioon, prosessi-d2c-viestit ja suorita seuraava komento:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Suorita prosessi-d2c-viestit][processd2c]

3.  **Simuloitu laite** -sovelluksen käyttämiseen Avaa komentokehote tai shell Simuloitu laitteen-kansioon ja suorita seuraava komento:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Suorita Simuloitu laite][simulateddevice]

> [AZURE.NOTE] Näet oman Blob-objektien päivitykset, joudut ehkä vähentää **MAX_BLOCK_SIZE** vakion **StoreEventProcessor** luokan pienempi arvoa, kuten **1 024**. Tämä muutos on hyötyä, koska kestää jonkin aikaa saavuttamiseksi estä kokorajoitus Simuloitu laitteen lähettämien tiedoilla. Estä pienemmäksi, jossa ei tarvitse odottaa niin kauan Nähdäksesi Blob-objektien on luotu ja päivitetään. Kuitenkin suuremmaksi lohko helpottavat sovelluksen paremmin skaalattavissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit käsitteleminen luotettavasti piste ja vuorovaikutteinen laitteen cloud viestit käyttämällä [EventProcessorHost] -luokka.

[Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen] [ lnk-c2d] avulla voit lähettää viestejä laitteet takaisin päästä.

Esimerkkejä valmis lopusta loppuun-ratkaisuja, jotka käyttävät IoT keskittimeen, on artikkelissa [Azure IoT Suite][lnk-suite].

Saat lisätietoja kanssa IoT keskittimeen, ratkaisujen kehittämiseen on [IoT keskittimeen Sovelluskehittäjän opas].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

<!-- Links -->

[Azure-blob-säiliö]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Palvelun Bus jonossa]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT keskittimeen developer guide - laitteen pilveen]: iot-hub-devguide-messaging.md

[Azure-tallennustilan]: https://azure.microsoft.com/documentation/services/storage/
[Azure palvelun Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT keskittimeen Sovelluskehittäjän opas]: iot-hub-devguide.md
[IoT keskittimeen käytön aloittaminen]: iot-hub-java-java-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Azure-tallennustilan tietoja]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Tapahtuman keskittimet käytön aloittaminen]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Azure tallennustilan skaalattavuus ohjeet]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Lyhytkestoisia virheen käsittely]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub