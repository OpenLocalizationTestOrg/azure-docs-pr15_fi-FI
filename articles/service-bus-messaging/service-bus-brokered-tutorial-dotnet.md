<properties 
    pageTitle="Palvelun Bus se tekstiviesti .NET-opetusohjelma | Microsoft Azure"
    description="Se tekstiviesti .NET-opetusohjelma."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-brokered-messaging-net-tutorial"></a>Palvelun Bus se tekstiviesti .NET-opetusohjelma

Azure palvelun Bus sisältää kaksi täydellinen tekstiviesti ratkaisuja – yksi kautta keskitetystä "välitys" palvelua pilvipalvelussa, joka tukee useita eri protokollat ja Web services standardeja, mukaan lukien SOAP-WS-*, ja Aseta. Asiakas ei tarvita paikallisen palvelun suoran yhteyden eikä se täytyy tietää, johon palvelu sijaitsee, ja paikallisen palvelun ei tarvitse mitään saapuvien porttien Avaa palomuurin.

Toinen tekstiviesti-ratkaisun avulla "se" sähköpostiviestinnän toimintoja. Nämä voidaan pitää mahdollisimman asynkronisen tai erillisen tekstiviesti ominaisuudet, joita tuki Julkaise-tilaa, ajallinen irtautus ja kuormituksen skenaarioiden palvelun Bus tekstiviesti-infrastruktuuria. Erilliset viestintä on monia etuja; esimerkiksi asiakkaiden ja palvelinten muodostaa tarpeen mukaan ja suorittaa niiden asynkroninen tavalla.

Tässä opetusohjelmassa on tarkoitettu antamaan sinulle yleiskatsaus ja olevien käytännön kokemusta-palvelun Bus core-komponentteja se messaging. Kun työskentelet läpi aiheet sarjan Tässä opetusohjelmassa, sinun on sovellus, joka täyttää viestiluetteloa, Luo jonon ja lähettää viestejä, jonon. Lopuksi sovellus vastaanottaa ja näyttää jonossa viestit sitten tyhjentää sen resurssit ja sulkee. Katso vastaavan opetusohjelma siitä, miten voit luoda palvelun Bus välitys käyttävä sovellus- [palvelun Bus välittäminen tekstiviesti opetusohjelma](../service-bus-relay/service-bus-relay-tutorial.md).

## <a name="introduction-and-prerequisites"></a>Johdanto ja edellytykset

Olevien tarjouksen ensimmäisen-First Out (FIFO) viestin lähettämisen vähintään yksi kilpailevien kuluttajille. FIFO tarkoittaa, että viestit odotetaan yleensä vastaanotettu ja käsittelee ajallinen järjestyksessä, jossa siirretyt enqueued ja jokaisen viestin vastaanotettu ja käsitellä vain yhden viestin kuluttaja vastaanottajia. Tärkein etu käyttämällä olevien on saavuttamiseksi sovelluksen osia *ajallinen irtautus* : toisin sanoen tuottajat ja kuluttajille ei tarvitse Lähetä ja vastaanota viestit yhtä aikaa, koska viestit tallennetaan kuuluminen jonossa. Aiheeseen liittyvät etu on *kuormituksen tasaus*, joka mahdollistaa tuottajat ja käyttäjät voivat lähettää ja vastaanottaa viestejä on erilainen..

Seuraavassa on joitakin järjestelmänvalvojan ja valmistelevat toimenpiteet olisi ennen opetusohjelman aloittamista. Ensimmäinen on palvelun nimitila ja hankkia jaettua käyttöä allekirjoitus (SAS)-näppäintä. Nimitilan on sovelluksen rajan kunkin sovelluksen tarjoamia palvelun Bus kautta. Suojaussidosten avaimen luodaan automaattisesti järjestelmä palvelun nimitila luomisen yhteydessä. Palvelun nimitila ja Suojaussidosten avaimen yhdistelmä on tunnistetiedon, jolla palvelun Bus todentaa access-sovellukseen.

### <a name="create-a-service-namespace-and-obtain-a-sas-key"></a>Luo palvelun nimitila ja Suojaussidosten avaimen hankkiminen

Ensimmäiseksi on, voit luoda palvelun nimitila ja [Jakaa Access allekirjoitus](service-bus-sas-overview.md) (SAS) Key-tunnuksen. Nimitilan on sovelluksen rajan kunkin sovelluksen tarjoamia palvelun Bus kautta. Suojaussidosten avaimen luodaan automaattisesti järjestelmä palvelun nimitila luomisen yhteydessä. Palvelun nimitila ja Suojaussidosten avaimen yhdistelmä sisältää palvelun Bus tarkistamiseen access-sovellukseen tunnistetiedot.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Seuraava vaihe on Visual Studio projektin luominen ja kirjoittaminen kaksi avustaja-toiminnot, jotka Lataa viestien pilkuilla erotettu luettelo erittäin kirjoitettu [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) .NET [luettelo](https://msdn.microsoft.com/library/6sh2ey19.aspx) -objekti.

### <a name="create-a-visual-studio-project"></a>Visual Studio projektin luominen

1. Avaa Visual Studio järjestelmänvalvojana Käynnistä-valikkoon ohjelmaa hiiren kakkospainikkeella ja valitsemalla **Suorita järjestelmänvalvojana**.

1. Luo uusi konsoli sovelluksen projekti. Valitse **Tiedosto** -valikko ja valitsemalla **Uusi**ja valitse sitten **Projekti**. Valitse **Uusi projekti** -valintaikkunan **Visual C#** (Jos **Visual C#** ei näy, katso kohdan **Muut kielet**), valitse **Console-sovelluksen** malli ja anna sille nimi **QueueSample**. Käytä oletusarvoista **sijainti**. Valitse **OK** , jos haluat luoda projektin.

1. Palvelun Bus kirjastojen lisääminen projektiin NuGet pakettien hallinta avulla:
    1. Napsauta ratkaisunhallinnassa **QueueSample** projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**.
    2. **Nuget pakettien hallinta** -valintaikkunassa **Selaa** -välilehti ja Etsi **Azure palvelun Bus**-ja valitse sitten **Asenna**.
<br />
1. Napsauta ratkaisunhallinnassa Kaksoisnapsauta Program.cs-tiedoston avaaminen Visual Studio-editori. Muuttaa sen oletusnimi nimitilan `QueueSample` , `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Muokkaa `using` lauseet seuraava koodi esitetyllä tavalla.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Luo nimetty tekstitiedosto Data.csv ja kopioi seuraavat pilkuilla erotettu teksti.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Tallenna ja sulje Data.csv tiedostoa, muista sijaintiin, johon tallensit sen.

1. Napsauta ratkaisunhallinnassa, kakkospainikkeella projektin (Tässä esimerkissä **QueueSample**), valitse **Lisää**ja valitse sitten **Olemassa olevan kohteen**.

1. Siirry Data.csv-tiedosto, jonka loit vaiheessa 6. Valitse tiedosto ja valitse sitten **Lisää**. Varmistaa, että * *kaikki tiedostot (*.*) ** luettelo tiedostotyypeistä on valittuna.

### <a name="create-a-method-that-parses-a-list-of-messages"></a>Jos maksutapa jäsentää viestiluetteloa luominen

1. - `Program` Luokan ennen `Main()` -menetelmä määritellä kahden muuttujan: jokin laji **arvotaulukko**, sisältävät Data.csv viestiluettelosta. Toinen on oltava tyypin luettelon kohdetta, kirjoitettu erittäin [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx)avulla. Tämä on se opetusohjelman seuraavat vaiheet käyttävät viestit luettelo.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Ulkopuolella `Main()`, Määritä `ParseCSV()` menetelmän, joka jäsentää Data.csv viestiluettelon ja lataa viestit [arvotaulukko](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx) taulukoksi, kuten kuvassa. Menetelmä palauttaa **arvotaulukko** -objekti.

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. : `Main()` -Menetelmä lisää lause, joka kutsuu `ParseCSVFile()` menetelmää:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### <a name="create-a-method-that-loads-the-list-of-messages"></a>Menetelmä, jota ladataan viestiluettelon luominen

1. Ulkopuolella `Main()`, Määritä `GenerateMessages()` menetelmä, joka kestää palauttama **arvotaulukko** objekti `ParseCSVFile()` ja lataa se viestien erittäin kirjoitettu luetteloa taulukon. Menetelmä palauttaa **luettelon** kohdetta, kuten seuraavassa esimerkissä. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. - `Main()`, Suoraan puhelu jälkeen `ParseCSVFile()`, Lisää ilmaisee, että puhelut `GenerateMessages()` menetelmä, jolla palautusarvo `ParseCSVFile()` argumenttina:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### <a name="obtain-user-credentials"></a>Hanki käyttäjän tunnistetietoja

1. Luo kolme yleinen merkkijonon muuttujaa nämä arvot. Määritellä muuttujia suoraan edellisen muuttujan määrittelyjä; jälkeen Esimerkki:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Seuraavaksi luodaan funktion, joka voidaan käyttää ja tallentaa palvelun nimitila ja Suojaussidosten avaimen avulla. Lisää tämä menetelmä ulkopuolella `Main()`. Esimerkki: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. - `Main()`, Suoraan puhelu jälkeen `GenerateMessages()`, Lisää ilmaisee, että puhelut `CollectUserInput()` menetelmää:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### <a name="build-the-solution"></a>Rakenna ratkaisu

Visual Studio **luominen** -valikosta **Muodosta** arvoja tai painamalla **Ctrl + Vaihto + B** vahvistat työsi tähän mennessä.

## <a name="create-management-credentials"></a>Tunnistetietojen hallinta

Tässä vaiheessa voit määrittää hallintatoiminnot, voit luoda jaettu käyttö allekirjoitus (SAS) tunnistetiedot, johon sovelluksesi valtuutettuja.

1. Tässä opetusohjelmassa sijoittaa selkeyttä, kaikki jonossa toiminnot erillisessä tavan. Luo asynkroninen `Queue()` menetelmä `Program` luokan jälkeen `Main()` menetelmää. Esimerkki:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Seuraavaksi [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) objektin SAS-tunnistetietojen luomiseen. Luontimenetelmä tulee Suojaussidosten avaimen nimen ja arvon saadut `CollectUserInput()` menetelmää. Lisää seuraava koodi `Queue()` menetelmää:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Luo uusi nimitilan hallinnan objekti URI, joka sisältää nimitilan nimi ja saada edellisessä vaiheessa argumentteina hallinta tunnistetiedot. Lisää tämä koodi suoraan edellisessä vaiheessa lisännyt koodin jälkeen. Varmista, että korvaa `<yourNamespace>` palvelun nimitila nimellä:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### <a name="example"></a>Esimerkki

Tässä vaiheessa koodisi pitäisi näyttää seuraavankaltaiselta:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## <a name="send-messages-to-the-queue"></a>Viestien lähettäminen jonossa

Tässä vaiheessa jonon luominen ja valitse luettelossa se, että jonon viestien lähettämiseen.

### <a name="create-queue-and-send-messages-to-the-queue"></a>Luo jono ja lähettää viestejä jonossa

1. Luo jonossa. Anna sille esimerkiksi `myQueue`, ja määritellä suoraan lisätyn hallintatoiminnot jälkeen `Queue()` menetelmä viimeisessä vaiheessa:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. Valitse `Queue()` -menetelmä luo tekstiviesti factory-objekti luomasi palvelun Bus URI argumenttina. Lisää seuraava koodi suoraan lisäämäsi viimeisessä vaiheessa hallintatoiminnot jälkeen. Varmista, että korvaa `<yourNamespace>` palvelun nimitila nimellä:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Seuraavaksi luodaan [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) -luokan avulla jono-objekti. Lisää seuraava koodi suoraan lisäämäsi viimeisessä vaiheessa koodin jälkeen:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Lisää seuraavaksi tunnus, jolla silmukat se viestien aiemmin luomasi lähettäminen kunkin jonossa luettelo. Lisää seuraava koodi suoraan jälkeen `CreateQueueClient()` lauseen edellisessä vaiheessa:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## <a name="receive-messages-from-the-queue"></a>Vastaanottaa viestejä jonossa

Tässä vaiheessa voit hankkia viestiluettelon edellisessä vaiheessa luomasi jonossa.

### <a name="create-a-receiver-and-receive-messages-from-the-queue"></a>Luo vastaanotin ja vastaanottaa viestejä jonossa

Valitse `Queue()` menetelmä käydä läpi jonossa ja vastaanotat viestejä, [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) -menetelmällä tulostamisen kustakin viestistä konsoliin ulos. Lisää seuraava koodi suoraan lisäämäsi edellisessä vaiheessa koodin jälkeen:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Huomaa, että `Thread.Sleep` käytetään vain simuloida viestin käsittely ja olet todennäköisesti ei tarvitse sitä todellisia tekstiviesti-sovelluksessa.

### <a name="end-the-queue-method-and-clean-up-resources"></a>Voit lopettaa jonon menetelmä ja Puhdista resurssit

Suoraan jälkeen Edellinen koodi Lisää seuraava koodi tyhjentääksesi viestin factory ja jonon resurssit:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### <a name="call-the-queue-method"></a>Jonon menetelmää

Viimeinen vaihe on lisättävä ilmaisee, että puhelut `Queue()` menetelmää `Main()`. Lisää seuraava koodi korostetun rivi Main() lopussa:
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### <a name="example"></a>Esimerkki

Seuraava koodi on valmis **QueueSample** -sovellus.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## <a name="build-and-run-the-queuesample-application"></a>Muodosta ja suorita QueueSample-sovellus

Nyt kun olet suorittanut edellä kuvatut toimet, voit luoda ja **QueueSample** -sovelluksen käyttämiseen.

### <a name="build-the-queuesample-application"></a>QueueSample-sovelluksen luominen

Visual Studiossa, valitse **Luo** -valikosta **Ratkaisun luominen**tai painamalla **Ctrl + Vaihto + B**. Jos käytössä ilmenee virheitä, varmista, että koodi on oikea valmis esimerkissä esitetään edellisessä vaiheessa lopussa perusteella.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa ilmeni muodostaminen palvelun Bus asiakkaan sähköpostisovelluksen ja -palvelu palvelun Bus se sähköpostiviestinnän toimintoja. Katso samalla opetusohjelma, joka käyttää palvelun Bus [välitys](service-bus-messaging-overview.md#Relayed-messaging)- [palvelun Bus välittäminen tekstiviesti opetusohjelma](../service-bus-relay/service-bus-relay-tutorial.md).

Lisätietoja [Palvelun Bus](https://azure.microsoft.com/services/service-bus/)on seuraavissa artikkeleissa.

- [Palvelun Bus tekstiviesti yleiskatsaus](service-bus-messaging-overview.md)
- [Palvelun Bus perusteet](service-bus-fundamentals-hybrid-solutions.md)
- [Palvelun Bus-arkkitehtuuri](service-bus-architecture.md)

