<properties 
    pageTitle="Voit käyttää AMQP 1.0 Java palvelun Bus Ohjelmointirajapinnan | Microsoft Azure" 
    description="Voit käyttää Azure palvelun Bus ja Advanced viestin Queuing Protodol (AMQP) 1.0 Java viestin Service (JMS)." 
    services="service-bus" 
    documentationCenter="java" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-the-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Palvelun Bus ja AMQP 1.0 Java viestin Service (JMS) Ohjelmointirajapinnan käyttäminen

Laajennettu viesti Queuing Protocol (AMQP) 1.0 on tehokasta, luotettavia, wire tason tekstiviesti protokolla, joiden avulla voit luoda tehokkaat, Office kaikissa ympäristöissä tekstiviesti sovelluksia.

Tuki AMQP 1.0-palvelun Bus tarkoittaa, että voit käyttää queuing ja Julkaise ja tilaa se tekstiviesti ominaisuuksia solualueen ympäristöjen tehokas binaarinen-protokollan avulla. Voit lisäksi luoda osat, jotka on luotu käyttäen kielet, kehysten sekä käyttöjärjestelmät koostuu sovellukset.

Tässä artikkelissa kerrotaan palvelun Bus messaging toiminnot (olevien ja Julkaise ja tilaa aiheet) käyttämisestä Java-sovellusten avulla Suositut Java viestin Service (JMS) API vakio. [Avustaja-artikkeli](service-bus-dotnet-advanced-message-queuing.md) , jossa kerrotaan, miten voit tehdä saman palvelun Bus .NET-Ohjelmointirajapinnan käyttäminen on. Voit käyttää näiden kahden oppaan yhdessä Saat lisätietoja Office kaikissa ympäristöissä messaging AMQP 1.0 avulla.

## <a name="get-started-with-service-bus"></a>Palvelun Bus käytön aloittaminen

Tässä oppaassa oletetaan, että sinulla on jo palvelun Bus nimitilan sisältävä nimeltä **queue1**jonon. Jos et valitse voit [luoda nimitilan ja jonon](service-bus-create-namespace-portal.md) [Azure portal](https://portal.azure.com). Lisätietoja palvelun Bus nimitilan ja olevien luomisesta on artikkelissa [palvelun Bus olevien käyttämisestä](service-bus-dotnet-get-started-with-queues.md).

> [AZURE.NOTE] Osioitu olevien ja ohjeita tukea myös AMQP. Lisätietoja on artikkelissa [Partitioned pikaviestinnän yksiköt](service-bus-partitioning.md) ja [palvelun Bus AMQP 1.0 tuki osioitu olevien ja aiheet](service-bus-partitioned-queues-and-topics-amqp-overview.md).

## <a name="downloading-the-amqp-10-jms-client-library"></a>AMQP 1.0 JMS asiakkaan kirjaston lataaminen

Tietoja siitä, mistä voit ladata uusimman version Apache Qpid JMS AMQP 1.0 asiakkaan kirjastoon, käy [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

On lisättävä seuraavat neljä PURKKI tiedostot Apache Qpid JMS AMQP 1.0 jakauman arkistosta Java-SELAIMESI kun sekä JMS-sovellusten käytön palvelun Bus:

- geronimo jms\_1.1\_määritys 1.0.jar
- qpid-amqp-1-0-Client-[version].JAR
- qpid-amqp-1-0-Client-jms-[version].JAR
- qpid-amqp-1-0-Common-[version].JAR

## <a name="coding-java-applications"></a>Java-sovellusten koodaus

### <a name="java-naming-and-directory-interface-jndi"></a>Java nimeämisestä ja käyttöliittymän Directory (JNDI)

JMS käytetään luomaan väliä looginen ja fyysinen nimet Java nimeämisestä ja hakemisto-liittymän (JNDI). Kahdentyyppisiä JMS objektien selvitetään käyttämällä JNDI: ConnectionFactory ja kohde. JNDI käyttää tarjoajan malli, johon voi kiinnittää eri hakemistopalvelujen käsittelemään nimi tarkkuus tehtävät. Apache Qpid JMS AMQP 1.0 kirjasto sisältää yksinkertaisen ominaisuudet tiedostopohjaisia JNDI-palvelu, joka on määritetty seuraavat ominaisuudet-tiedoston muotoileminen:

```
# servicebus.properties - sample JNDI configuration
    
# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
    
# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-the-connectionfactory"></a>Määritä ConnectionFactory

Voit määrittää **ConnectionFactory** Qpid ominaisuudet tiedoston JNDI palvelussa tapahtuma on seuraava kaava:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Jos **[jndi_name]** ja **[ConnectionURL]** on seuraavia asioita:

- **[jndi_name]**: ConnectionFactory looginen nimi. Tämä on nimi, joka korjataan JNDI IntialContext.lookup() menetelmällä Java-sovelluksessa.
- **[ConnectionURL]**: A URL-Osoitetta, joka tarjoaa JMS kirjaston AMQP broker tarvittavat tiedot.

**ConnectionURL** muoto on seuraavanlainen:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Jos **[nimitilan]**, **[SASPolicyName]** ja **[SASPolicyKey]** on seuraavia asioita:

- **[nimitilan]**: palvelun Bus nimitilan.
- **[SASPolicyName]**: jonon jaettu Access allekirjoituksen käytännön nimi.
- **[SASPolicyKey]**: jonon jaettu Access allekirjoituksen käytäntö-näppäintä.

> [AZURE.NOTE] Sinun täytyy URL-Osoitteen koodata salasanan manuaalisesti. Hyödyllisiä URL-koodausta apuohjelma on saatavilla kohdassa [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

#### <a name="configure-destinations"></a>Määritä kohteisiin

Kohde määritetään Qpid ominaisuudet tiedoston JNDI palvelussa tapahtuma on seuraavanlainen:

```
queue.[jndi_name] = [physical_name]
```

tai

```
topic.[jndi_name] = [physical_name]
```

Jossa **[jndi\_nimi]** ja **[fyysinen\_nimi]** on seuraavia asioita:

- **[jndi_name]**: kohde looginen nimi. Tämä on nimi, joka korjataan JNDI IntialContext.lookup() menetelmällä Java-sovelluksessa.
- **[physical_name]**: palvelun Bus kohde, johon sovellus lähettää tai vastaanottaa viestit nimi.

> [AZURE.NOTE] Palvelun Bus aiheen tilauksesta vastaanotettaessa määritetyn JNDI fyysinen nimen tulisi olla aiheen nimi. Tilauksen nimi on annettu luotaessa kestävät tilauksen JMS sovelluksen koodissa. [Palvelun Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) on lisätietoja palvelun Bus aiheen tilaukset-JMS käyttämiseen.

### <a name="write-the-jms-application"></a>Kirjoita JMS-sovellus

Ei ole erityistä ohjelmointirajapinnan tai kun JMS käyttäminen palvelun Bus asetukset. On kuitenkin joitakin rajoituksia, jotka myöhemmin kattaa. On JMS kaikissa sovelluksissa ensin thing pakollinen on JNDI ympäristön **ConnectionFactory** ja kohteet voivat määrittäminen.

#### <a name="configure-the-jndi-initialcontext"></a>Määritä JNDI InitialContext

JNDI-ympäristö on määritetty välittämällä hajautustaulukko kokoonpanotietoja javax.naming.InitialContext luokan konstruktori kyselyjä. Kahden edellyttää, hajautustaulukko osat ovat luokkanimi ja alkuperäinen kontekstin Factory-palveluntarjoajan URL-osoite. Seuraava koodi näkyy määrittäminen käyttämään Qpid ominaisuudet-tiedostoa JNDI-ympäristön pohjalta JNDI tarjoajan **servicebus.properties**tiedoston ominaisuudet.

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Yksinkertainen JMS-sovelluksesta valitsemalla palvelun Bus jonossa

Seuraavassa esimerkissä ohjelma palvelun Bus jonon JNDI looginen nimellä jonon JMS TextMessages lähettää ja vastaanottaa viestit takaisin.

```
// SimpleSenderReceiver.java
    
import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;
    
public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();
    
    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);
    
        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");
    
        // Create Connection
        connection = cf.createConnection();
    
        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);
    
        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }
    
    public static void main(String[] args) {
        try {
    
            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }
    
            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] to send a message. Type 'exit' + [enter] to quit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));
    
            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }
    
    public void close() throws JMSException {
        connection.close();
    }
    
    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}   
```

### <a name="run-the-application"></a>Suorita sovellus

Sovelluksen suorittamisen tuottaa lomakkeen tulokset:

```
> java SimpleSenderReceiver
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318
    
Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483
    
Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Ympäristöstä messaging JMS ja .NET-

Tämän oppaan ilmeni voit lähettää ja vastaanottaa viestejä ja palvelun Bus JMS avulla. Yksi tärkeimmistä eduista AMQP 1.0 on kuitenkin, että se antaa sovellusten laadittuihin kirjoitettu eri kielillä luotettavasti ja koko tarkkuuden vaihdettujen viestien komponenteilta.

Yllä olevien ohjeiden sovelluksen malli JMS ja samalla .NET-sovellus otetaan avustaja-opas [käyttämisestä AMQP 1.0 .NET palvelun Bus .NET-Ohjelmointirajapinnan kanssa](service-bus-dotnet-advanced-message-queuing.md), voit vaihtaa viestejä .NET ja Java välillä. 

Saat lisätietoja ympäristöstä messaging palvelun Bus ja AMQP 1.0-tietoja [palvelun Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md).

### <a name="jms-to-net"></a>JMS .NET

.NET messaging osoittaa JMS:

* Aloita sovelluksen .NET-malli ilman komentorivin argumentit.
* Aloita sovelluksen Java malli "sendonly" komentorivin argumentit. Tässä tilassa sovellus ei tule sanomia jonossa, se vain lähetetään.
* Paina **Enter** -näppäintä muutaman kerran Java-sovelluksen-ikkuna, mikä aiheuttaa viestejä voidaan lähettää.
* .NET-sovelluksen saapuvat viestit.

#### <a name="output-from-jms-application"></a>Tulosteen JMS sovelluksesta

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Tulosteen .NET-sovellus

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-to-jms"></a>.NET-JMS

Osoittaa .NET JMS messaging:

* Aloita sovelluksen .NET malli "sendonly" komentorivin argumentit. Tässä tilassa sovellus ei tule sanomia jonossa, se vain lähetetään.
* Aloita sovelluksen Java-malli ilman komentorivin argumentit.
* Paina **Enter** -näppäintä muutaman kerran .NET-sovelluksen konsolissa, mikä aiheuttaa viestejä voidaan lähettää.
* Java-sovelluksen saapuvat viestit.

#### <a name="output-from-net-application"></a>Tulosteen .NET-sovellus

```
> SimpleSenderReceiver.exe sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3  
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Tulosteen JMS sovelluksesta

```
> java SimpleSenderReceiver 
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Ei-tuettuja ominaisuuksia ja rajoitukset

Seuraavat rajoitukset olemassa, kun käyttäminen JMS eli AMQP 1.0 Bus palvelun kautta:

* Vain yksi **MessageProducer** tai **MessageConsumer** sallitaan **istunnossa**. Jos haluat luoda useita **MessageProducers** tai **MessageConsumers** -sovelluksessa, Luo kullekin niistä oma **istunto** .
* Muuttuvat aiheen tilaukset eivät ole tuettuja.
* **MessageSelectors** eivät ole tuettuja.
* Tilapäinen kohteita, kuten **TemporaryQueue**, **TemporaryTopic** eivät ole tuettuja, sekä **QueueRequestor** ja **TopicRequestor** API, joka käyttää niitä.
* Tapahtuma istunnot ja Jaetut tapahtumat ei tueta.

## <a name="summary"></a>Yhteenveto

Tämä käyttöä koskevia artikkeleita ilmeni käyttämisestä palvelun Bus se tekstiviesti-toimintoja (olevien ja Julkaise ja tilaa aiheet) Java Suositut JMS API ja AMQP 1.0 avulla.

Voit käyttää myös palvelun Bus AMQP 1.0 muiden kielten, kuten .NET, C, Python ja PHP. Osat, jotka on luotu käyttäen näiden kielten voit vaihtaa viestejä luotettavasti ja etsiminen käyttämällä AMQP 1.0 luotettavaa tuki palvelun Bus. Lisätietoja on artikkelissa [palvelun Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Seuraavat vaiheet

* [Azure-palvelu Bus AMQP 1.0-tuki](service-bus-amqp-overview.md)
* [Voit käyttää AMQP 1.0 palvelun Bus .NET-Ohjelmointirajapinta](service-bus-dotnet-advanced-message-queuing.md)
* [Palvelun Bus AMQP 1.0 kehittäjän opas](service-bus-amqp-dotnet.md)
* [Opi käyttämään palvelua Bus olevien](service-bus-dotnet-get-started-with-queues.md)
* [Java Developer Center](/develop/java/).



 
