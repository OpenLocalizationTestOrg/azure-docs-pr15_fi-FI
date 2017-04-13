<properties
    pageTitle="Voit käyttää palvelun Bus olevien Java | Microsoft Azure"
    description="Opettele käyttämään palvelua Bus olevien Azure-tietokannassa. Kirjoitettu Java MALLIKOODEJA."
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Opi käyttämään palvelua Bus olevien

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tässä artikkelissa kerrotaan, miten käyttämään palvelua Bus olevien. Mallit on kirjoitettu Java ja käytä [Azure SDK Java][]. Tilanteita, joissa kattaa ovat **luominen olevien** **viestien lähettäminen ja vastaanottaminen**ja **poistamalla olevien**.

## <a name="what-are-service-bus-queues"></a>Mitkä ovat palvelun Bus olevien?

Palvelun Bus olevien tukevat **se messaging** viestintä-malli. Kun käytät olevien, jaetun sovelluksen osia ei keskenään suoraan; sen sijaan ne vaihtaa viestejä jono, jonka välittämään (broker) kautta. Viestin tuottaja (lähettäjä) kädet jonossa viestin käytöstä ja jatkaa sitten sen käsittely.
Asynkronisesti viestin kuluttaja (vastaanottaja) hakee jonossa lähettäjä ja käsittelee sen. Tuottaja ei tarvitse odottaa kuluttaja vastauksessa, jotta voit käsitellä ja viestien lähettäminen edelleen. Olevien tarjouksen **ensimmäisen-, ensimmäinen ulos FIFO** viestin lähettämisen vähintään yksi kilpailevien kuluttajille. Viestit ovat eli yleensä vastaanotettu ja käsittelemien siinä järjestyksessä, jossa ne on lisätty jonossa ja viesti on vastaanotettu ja käsitellä vain yhden viestin kuluttaja vastaanottajia.

![QueueConcepts](./media/service-bus-java-how-to-use-queues/sb-queues-08.png)

Palvelun Bus olevien on yleinen tekniikka, joka voi käyttää erilaisia skenaariot:

- Viestintä Internetin kautta tai työntekijä roolien monitasoisten Azure sovelluksen välillä.
- Paikallisen sovellukset ja Azure välisen isännöidään sovellusten hybrid-ratkaisussa.
- Viestintä käynnissä paikallisen eri organisaatioissa tai organisaation osastot jaettu sovellus osien välillä.

Olevien avulla voit skaalata helpommin, sovellusten ja ota käyttöön oman arkkitehtuuri vikasietoisuudelle.

## <a name="create-a-service-namespace"></a>Luo palvelun nimitila

Kun haluat käyttää palvelun Bus olevien Azure, sinun on luotava nimitila. Nimitilan tarjoaa säilöön osoitteiden palvelun Bus resurssien sovelluksessa.

Voit luoda nimitilan seuraavasti:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Määritä sovelluksesi voi käyttää palvelua Bus

Varmista, että olet asentanut [Azure SDK Java][] ennen tämän mallin luomisesta. Jos käytät Pimennys, voit asentaa [Azure työkalujen Pimennys varten][] , joka sisältää Java Azure SDK-paketissa. Voit lisätä **Microsoft Azure-kirjastojen Java** sitten projektin:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Lisää seuraava `import` lauseet Java-tiedoston alkuun:

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Luo jono

Palvelun Bus olevien hallintatoiminnot voivat tehdä **ServiceBusContract** -luokan kautta. **ServiceBusContract** objektin muodostetaan sopiva kokoonpanossa, kapseloi SAS tunnuksen ja hallita sitä oikeudet ja **ServiceBusContract** -luokka on ainoa Azure tietoliikenteen.

**ServiceBusService** luokan sisältää tapoja luoda, Luetteloi ja poistaa olevien. Seuraavassa esimerkissä näkyy, miten **ServiceBusService** objektin avulla voidaan luoda nimeltä "TestQueue", "HowToSample"-niminen nimitilan jonon:

```
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Valitse **QueueInfo** eri tavalla, joiden avulla voidaan virittää jonon ominaisuuksia (esimerkiksi: Määritä time-to-live (TTL) oletusarvo jonossa lähetettyihin viesteihin käytetään). Seuraavassa esimerkissä esitetään, voit luoda nimeltä jonon `TestQueue` koko on enintään 5 gt: N kanssa:

````
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Huomaa, että voit käyttää **listQueues** menetelmä **ServiceBusContract** objektien tarkistamaan, jos määritetty nimi on jo jono palvelun nimitilan.

## <a name="send-messages-to-a-queue"></a>Viestien lähettäminen jonossa

Jos haluat lähettää viestin palvelun Bus jonon, sovelluksen hakee **ServiceBusContract** objekti. Seuraava koodi kerrotaan, miten voit lähettää viestin `TestQueue` jonossa aiemmin luotu `HowToSample` nimitilan:

```
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Lähetettyjä viestejä ja poistaa palvelun Bus olevien ovat [BrokeredMessage][] luokan esiintymät. [BrokeredMessage][] objekteissa on joukko vakio-ominaisuuksia (esimerkiksi [otsikon](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) sanasto, jota käytetään pidä mukautetun sovelluksen tiettyjen ominaisuuksien ja haluamaansa hakemuksen tiedot. Sovelluksen voit määrittää viestin tekstiosaan välittämällä voi sarjoittaa objektin [BrokeredMessage][]konstruktori kyselyjä ja sopiva Sarjatoiminto käytetään onnistu objekti. Voit vaihtoehtoisesti kirjoittaa **java. IO. InputStream** objekti.

Seuraavassa esimerkissä viisi testaaminen viestien lähettäminen `TestQueue` **MessageSender** on saatu edellisen koodikatkelman:

```
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Palvelun Bus olevien tue viestin koko on [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu pidetään jonossa viestien määrän, mutta ole pää kokonaiskoko saamisesta jonon viestit. Tämä jono koko on määritetty luominen aikaa, yläraja 5 gigatavua.

## <a name="receive-messages-from-a-queue"></a>Vastaanottaa viestejä jonossa

Ensisijainen tapa vastaanottaa viestejä jonossa on käyttää **ServiceBusContract** objektia. Vastaanotettujen viestien työskennellä kahta eri tilat: **ReceiveAndDelete** ja **PeekLock**.

Kun **ReceiveAndDelete** -tilassa on yksittäinen kuva - toiminto, kun palvelua Bus vastaanottaa viestin lukukuittauksen jonossa, se merkitsee viestin parhaillaan kulutettu ja palauttaa sen sovelluksen. **ReceiveAndDelete** tila (joka on oletustila) on helpoin malli, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään.
Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

**PeekLock** tilassa vastaanottaa tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen soittamalla vastaanotetun viestin **poistaminen** . Palvelun Bus näkee **Poista** kutsu, kun se Merkitse viesti kulutettu käytössä-tilassa ja poista se jonossa.

Seuraavassa esimerkissä näytetään, miten viestit voi vastaanottaa ja käsitellä **PeekLock** -tilassa (ei oletustila). Alla olevassa esimerkissä ei äärettömän silmukan ja käsittelee viestit niiden saapuessa kyselyjä Microsoftin "TestQueue":

```
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua **unlockMessage** -menetelmää vastaanotetun viestin (sijaan **deleteMessage** -menetelmä). Tämä aiheuttaa palvelun Bus jonossa viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu jonossa viestin liittyvät aikakatkaisu ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu), valitse palvelun Bus viestin lukituksen automaattisesti ja lisätään uudelleen Vastaanotettava.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen **deleteMessage** -pyyntö annetaan, valitse viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kun käsittely**-, viesti käsitellään vähintään kerran mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **getMessageId** -menetelmällä.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus olevien perusteet, katso lisätietoja [olevien, aiheet, ja tilaukset][] .

Lisätietoja on artikkelissa [Java Developer Center](/develop/java/).


  [Java Azure SDK]: http://azure.microsoft.com/develop/java/
  [Azure työkalujen Pimennys varten]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

