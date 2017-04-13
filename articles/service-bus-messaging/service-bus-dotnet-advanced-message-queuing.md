<properties 
    pageTitle="Voit käyttää AMQP 1.0 .NET palvelun Bus Ohjelmointirajapinnan | Microsoft Azure" 
    description="Opettele käyttämään Advanced viestin Queuing Protodol (AMQP) 1.0 Azure .NET palvelun Bus Ohjelmointirajapinnan kanssa." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-amqp-10-with-the-service-bus-net-api"></a>Voit käyttää AMQP 1.0 palvelun Bus .NET-Ohjelmointirajapinta

Laajennettu viesti Queuing Protocol (AMQP) 1.0 on tehokasta, luotettavia, wire tason tekstiviesti protokolla, jonka avulla voit luoda tehokkaat, Office kaikissa ympäristöissä, messaging sovellukset.

Tuki AMQP 1.0-palvelun Bus tarkoittaa, että voit käyttää queuing ja Julkaise ja tilaa se tekstiviesti ominaisuuksia solualueen ympäristöjen tehokas binaarinen-protokollan avulla. Voit lisäksi luoda osat, jotka on luotu käyttäen kielet, kehysten sekä käyttöjärjestelmät koostuu sovellukset.

Tässä artikkelissa kerrotaan, miten voit käyttää palvelun Bus se tekstiviesti-ominaisuuksia (olevien ja Julkaise ja tilaa aiheet) palvelun Bus .NET-Ohjelmointirajapinnan käyttäminen .NET-sovelluksista. [Avustaja-artikkeli](service-bus-java-how-to-use-jms-api-amqp.md) , jossa kerrotaan, kuinka voit tehdä saman käyttämällä vakio Java viestin Service (JMS)-Ohjelmointirajapinnan on. Voit käyttää näiden kahden oppaan yhdessä Saat lisätietoja Office kaikissa ympäristöissä messaging AMQP 1.0 avulla.

## <a name="get-started-with-service-bus"></a>Palvelun Bus käytön aloittaminen

Tässä artikkelissa oletetaan, että sinulla on jo palvelun Bus nimitilan sisältävä jonon nimeltä "queue1." Jos et valitse voit luoda nimitila ja [Azure portal][]-jonossa. Lisätietoja palvelun Bus nimitilan ja olevien luomisesta on artikkelissa [palvelun Bus olevien käytön aloittaminen](service-bus-dotnet-get-started-with-queues.md#1-create-a-namespace-using-the-Azure-portal).

## <a name="download-the-service-bus-sdk"></a>Lataa palvelun Bus SDK-paketissa

AMQP 1.0 tuki on käytettävissä palvelun Bus SDK 2.1 tai uudempi versio. Voit ladata uusimman bittien NuGet osoitteessa [http://nuget.org/packages/WindowsAzure.ServiceBus/](http://nuget.org/packages/WindowsAzure.ServiceBus/).

## <a name="code-net-applications"></a>Koodin .NET-sovellukset

Oletusarvon mukaan palvelun Bus .NET asiakkaan kirjaston kommunikoi erillinen SOAP-pohjainen-protokollan avulla palvelun Bus-palvelussa. AMQP 1.0 käyttää sen sijaan, että oletusarvo-protokolla edellyttää eksplisiittinen configuration-palvelun Bus yhteysmerkkijonon seuraavan osion ohjeiden mukaisesti. Tämä muutos kuin sovelluksen koodin säilyy muuttumattomana lähinnä AMQP 1.0 käytettäessä.

Nykyisessä versiossa on joitakin API-ominaisuudet, joita ei tueta, kun käytät AMQP. Tuettuja ominaisuuksia näkyvät jäljempänä kohdassa [ominaisuuksia ja rajoitukset](#unsupported-features-and-restrictions). Osa lisämäärityksiä asetuksista on eri merkitys myös AMQP käytettäessä. Tässä artikkelissa käytetyt ei mitään näistä asetuksista ovat, mutta lisätietoja ovat käytettävissä [palvelun Bus AMQP yleiskatsaus](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

### <a name="configure-via-appconfig"></a>Määritä App.config kautta

Sovellusten käyttää App.config kokoonpanotiedosto tallentamaan asetukset Suosituksena on. Palvelun Bus sovellusten App.config avulla voidaan tallentaa palvelun Bus **ConnectionString**. Tämän esimerkkisovelluksen myös käyttää App.config messaging yritys, joka käyttää palvelun Bus nimi.

App.config mallitiedosto on mukana:

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
            <add key="EntityName" value="queue1" />
    </appSettings>
</configuration>
```

### <a name="configure-the-service-bus-connection-string"></a>Määritä Service Bus yhteysmerkkijono

**Microsoft.ServiceBus.ConnectionString** -asetuksen arvo on palvelun Bus yhteysmerkkijonon, joka määrittää palvelun Bus yhteys käytetään. Muoto on seuraavanlainen:

```
Endpoint=sb://[namespace].servicebus.windows.net;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp
```

Jos `[namespace]` ja `[SAS key]` saadaan [Azure portal][]. Lisätietoja on artikkelissa [Opi käyttämään palvelua Bus olevien] [].

AMQP käytettäessä yhteysmerkkijonon näkyy kanssa `;TransportType=Amqp`, joka kertoo asiakas-kirjastossa voit tehdä sen yhteys palvelun Bus AMQP 1.0 avulla.

### <a name="configure-the-entity-name"></a>Määritä kohteen nimi

Tämä malli-sovellus `EntityName` määrittäminen App.config tiedoston **appSettings** -kohtaan nimi, jonka kanssa viestit sovelluksen vaihtaa jonossa määrittämiseen.

### <a name="a-simple-net-application-using-a-service-bus-queue"></a>Yksinkertainen .NET-sovelluksesta valitsemalla palvelun Bus jonossa

Seuraavassa esimerkissä lähettää ja vastaanottaa viestejä ja palvelun Bus jonon.

```
// SimpleSenderReceiver.cs
    
using System;
using System.Configuration;
using System.Threading;
using Microsoft.ServiceBus.Messaging;
    
namespace SimpleSenderReceiver
{
    class SimpleSenderReceiver
    {
        private static string connectionString;
        private static string entityName;
        private static Boolean runReceiver = true;
        private MessagingFactory factory;
        private MessageSender sender;
        private MessageReceiver receiver;
        private MessageListener messageListener;
        private Thread listenerThread;
    
        static void Main(string[] args)
        {
            try
            {
                if ((args.Length > 0) && args[0].ToLower().Equals("sendonly"))
                    runReceiver = false;
                    
                string ConnectionStringKey = "Microsoft.ServiceBus.ConnectionString";
                string entityNameKey = "EntityName";
                entityName = ConfigurationManager.AppSettings[entityNameKey];
                connectionString = ConfigurationManager.AppSettings[ConnectionStringKey];
                SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
    
                Console.WriteLine("Press [enter] to send a message. " +
                    "Type 'exit' + [enter] to quit.");
    
                while (true)
                {
                    string s = Console.ReadLine();
                    if (s.ToLower().Equals("exit"))
                    {
                        simpleSenderReceiver.Close();
                        System.Environment.Exit(0);
                    }
                    else
                        simpleSenderReceiver.SendMessage();
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Caught exception: " + e.Message);
            }
        }
    
        public SimpleSenderReceiver()
        {
            factory = MessagingFactory.CreateFromConnectionString(connectionString);
            sender = factory.CreateMessageSender(entityName);
    
            if (runReceiver)
            {
                receiver = factory.CreateMessageReceiver(entityName);
                messageListener = new MessageListener(receiver);
                listenerThread = new Thread(messageListener.Listen);
                listenerThread.Start();
            }
        }
    
        public void Close()
        {
            messageListener.RequestStop();
            listenerThread.Join();
            factory.Close();
        }
    
        private void SendMessage()
        {
            BrokeredMessage message = new BrokeredMessage("Test AMQP message from .NET");
            sender.Send(message);
            Console.WriteLine("Sent message with MessageID = " 
                + message.MessageId);
        }

    }
    
    public class MessageListener
    {
        private MessageReceiver messageReceiver;
        public MessageListener(MessageReceiver receiver)
        {
            messageReceiver = receiver;
        }
    
        public void Listen()
        {
            while (!_shouldStop)
            {
                try
                {
                    BrokeredMessage message = 
                        messageReceiver.Receive(new TimeSpan(0, 0, 10));
                    if (message != null)
                    {
                        Console.WriteLine("Received message with MessageID = " + 
                            message.MessageId);
                        message.Complete();
                    }
                }
                catch (Exception e)
                {
                    Console.WriteLine("Caught exception: " + e.Message);
                    break;
                }
            }
        }
    
        public void RequestStop()
        {
            _shouldStop = true;
        }
    
        private volatile bool _shouldStop;
    }
}
```

### <a name="run-the-application"></a>Suorita sovellus

Sovelluksen suorittamisen tuottaa lomakkeen tulokset:

```
> SimpleSenderReceiver.exe
Press [enter] to send a message. Type 'exit' + [enter] to quit.
    
Sent message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
Received message with MessageID = fb7f5d3733704e4ba4bd55f759d9d7cf
    
Sent message with MessageID = d00e2e088f06416da7956b58310f7a06
Received message with MessageID = d00e2e088f06416da7956b58310f7a06
    
Received message with MessageID = f27f79ec124548c196fd0db8544bca49
Sent message with MessageID = f27f79ec124548c196fd0db8544bca49
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Ympäristöstä messaging JMS ja .NET-

Tässä aiheessa on osoittanut, miten voit lähettää viestejä palvelun Bus käyttämällä .NET ja myös .NET näiden viestien vastaanottamiseen. Yksi tärkeimmistä eduista AMQP 1.0 on kuitenkin, että se antaa sovellusten laadittuihin kirjoitettu eri kielillä luotettavasti ja koko tarkkuuden vaihdettujen viestien komponenteilta.

Yllä olevien ohjeiden otoksen .NET-sovellus ja samalla Java-sovellus otetaan avustaja-opas [Java viestin Service (JMS) palvelun Bus & AMQP 1.0 Ohjelmointirajapinnan käyttäminen](service-bus-java-how-to-use-jms-api-amqp.md)on mahdollista vaihtaa .NET ja Java väliset viestit. 

Saat lisätietoja ympäristöstä messaging palvelun Bus ja AMQP 1.0-tietoja [palvelun Bus AMQP 1.0 yleiskatsaus](service-bus-amqp-overview.md).

### <a name="jms-to-net"></a>JMS .NET

.NET messaging osoittaa JMS:

* Aloita sovelluksen .NET-malli ilman komentorivin argumentit.
* Aloita sovelluksen Java malli "sendonly" komentorivin argumentit. Tässä tilassa sovellus ei tule sanomia jonossa, se vain lähetetään.
* Paina **Enter** -näppäintä muutaman kerran Java-sovelluksen-ikkuna, mikä aiheuttaa viestejä voidaan lähettää.
* .NET-sovelluksen saapuvat viestit.

### <a name="output-from-jms-application"></a>Tulosteen JMS sovelluksesta

```
> java SimpleSenderReceiver sendonly
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

### <a name="output-from-net-application"></a>Tulosteen .NET-sovellus

```
> SimpleSenderReceiver.exe  
Press [enter] to send a message. Type 'exit' + [enter] to quit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

## <a name="net-to-jms"></a>.NET-JMS

Osoittaa .NET JMS messaging:

* Aloita sovelluksen .NET malli "sendonly" komentorivin argumentit. Tässä tilassa sovellus ei tule sanomia jonossa, se vain lähetetään.
* Aloita sovelluksen Java-malli ilman komentoriviargumentit.
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

.NET-palvelun Bus API seuraavia ominaisuuksia ei tueta tällä hetkellä, kun AMQP avulla:

 * Tapahtumat
 * Lähetä siirron kohde

Lisätietoja [ei-tuettuja ominaisuuksia, rajoitukset, ja käytöspiirteen eroja](service-bus-amqp-dotnet.md#unsupported-features-restrictions-and-behavioral-differences).

## <a name="summary"></a>Yhteenveto

Tässä artikkelissa on osoittanut voit käyttää palvelun Bus se viestinnän toimintoja (olevien ja Julkaise ja tilaa aiheet) .NET AMQP 1.0 ja palvelun Bus .NET-Ohjelmointirajapinnan käyttäminen.

Voit käyttää myös palvelun Bus AMQP 1.0 muiden kielten, kuten Java, C, Python ja PHP. Osat, jotka on luotu käyttäen näiden kielten vaihtoon viestien luotettavasti ja AMQP 1.0 käyttäminen palvelun Bus luotettavaa. Lisätietoja on artikkelissa [palvelun Bus AMQP yleiskatsaus](service-bus-amqp-dotnet.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet lukenut, palvelun Bus ja AMQP .NET kanssa, katso lisätietoja seuraavista linkeistä.

* [Azure-palvelu Bus AMQP 1.0-tuki](service-bus-amqp-overview.md)
* [Palvelun Bus & AMQP 1.0 Java viestin Service (JMS) Ohjelmointirajapinnan käyttäminen](service-bus-java-how-to-use-jms-api-amqp.md)
* [Opi käyttämään palvelua Bus olevien](service-bus-dotnet-get-started-with-queues.md)
 
[Azure portal]: https://portal.azure.com
