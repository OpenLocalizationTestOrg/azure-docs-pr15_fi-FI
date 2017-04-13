<properties 
    pageTitle="Palvelun Bus välitys opetusohjelma | Microsoft Azure"
    description="Muodosta palvelun Bus asiakkaan sovelluksen ja palvelun Bus välitys-palveluun."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/27/2016"
    ms.author="sethm" />

# <a name="service-bus-relay-tutorial"></a>Palvelun Bus välitys-opetusohjelma

Tässä opetusohjelmassa kuvataan, miten voit luoda yksinkertaisen palvelun Bus asiakassovellus ja palvelu palvelun Bus "välitys"-ominaisuuksia. Vastaavat opetusohjelmaan, joka käyttää palvelun Bus [se viestintä](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)on artikkelissa [Palvelun Bus se Messaging .NET opetusohjelma](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Tässä opetusohjelmassa – työskentelyä antaa käsityksen ohjeet, joita tarvitaan palvelun Bus asiakkaan ja sovelluksen luominen. WCF niiden vastineiden palvelu on rakennetta, joka sisältää vähintään yhden päätepisteet, joista kukin paljastaa yhden tai usean palvelutoimintoja. Palvelun päätepisteelle määrittää sidonta, joka sisältää tiedot, jotka asiakas on oltava yhteydessä palvelu ja sopimuksen, joka määrittää palvelun toimintoja asiakkaille josta löytyy palvelu-osoite. Tärkein WCF- ja palvelun Bus palvelun välinen ero on, että päätepiste on määritetty pilvessä sijaan paikallisesti tietokoneeseen.

Sen jälkeen, kun työskentelet läpi aiheet sarjan Tässä opetusohjelmassa, sinun on käynnissä palvelu ja asiakas, voit käynnistää palvelun toimintoja. Ensimmäinen aihe käsitellään Määritä ensin tili. Seuraavat vaiheet kuvaavat määrittäminen palvelu, joka käyttää sopimuksen, palvelun ottamisesta käyttöön ja määrittäminen palvelun koodi. Ne kuvaavat myös isännöidä ja palvelun suorittamisesta. Palvelun, joka on luotu on itse isännöityä ja asiakkaan ja palvelun suorittaminen samassa tietokoneessa. Voit määrittää palvelun koodin tai määritystiedostoa.

Lopullinen kolme vaihetta kerrotaan, kuinka voit luoda asiakassovellus, Määritä asiakassovellus, ja luoda ja käyttää asiakas, voit käyttää isännän toimintoja.

Kaikki tässä osassa on artikkeleissa oletetaan, että käytät Visual Studiossa kehitysympäristö.

## <a name="sign-up-for-an-account"></a>Tilin rekisteröityminen

Ensimmäiseksi on, voit luoda nimitilan ja jakaa Access allekirjoitus (SAS) Key-tunnuksen. Nimitilan on sovelluksen rajan kunkin sovelluksen tarjoamia palvelun Bus kautta. Suojaussidosten avaimen luodaan automaattisesti järjestelmä palvelun nimitila luomisen yhteydessä. Palvelun nimitila ja Suojaussidosten avaimen yhdistelmä on tunnistetietojen palvelun Bus tarkistamiseen access-sovellukseen.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="define-a-wcf-service-contract-to-use-with-service-bus"></a>Määritä WCF sopimus palvelun Bus käyttäminen

Service-palvelusopimus määrittää, mitkä toiminnot (Web opettelu menetelmistä tai toiminnot)-palvelun tukee. Sopimuksia luodaan määrittämällä C++, C# tai Visual Basic-käyttöliittymän. Kummassakin menetelmässä käyttöliittymässä vastaa tietyn palvelun-toiminto. Kunkin liittymän on oltava käytössä [ServiceContractAttribute-kohdetta](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) määrite ja kunkin toiminnon on oltava käytössä [ServiceOperationInfo-kohteessa määritetystä](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) määrite. Menetelmän käyttöliittymä, jossa on [ServiceContractAttribute-kohdetta](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) -määrite ei ole [ServiceOperationInfo-kohteessa määritetystä](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) -määrite, jos tämä menetelmä ei näkyvät. Näiden tehtävien koodi on annettu esimerkin ohjeiden mukaisesti. Katso suurempi keskustelu sopimuksia ja palvelujen, [suunnittelemisesta ja toteuttaminen palvelut](https://msdn.microsoft.com/library/ms729746.aspx) WCF asiakirjoissa.

### <a name="to-create-a-service-bus-contract-with-an-interface"></a>Voit luoda liittymällä Service Bus-palvelusopimus

1. Avaa Visual Studio järjestelmänvalvojana hiiren kakkospainikkeella **Käynnistä** -valikon ohjelma ja valitse **Suorita järjestelmänvalvojana**.

2. Luo uusi konsoli sovelluksen projekti. Valitse **Tiedosto** -valikko ja valitsemalla **Uusi**ja valitse sitten **Projekti**. **Uusi projekti** -valintaikkunassa valitse **Visual C#** (Jos **Visual C#** ei näy, katso kohdan **Muut kielet**). Valitse **Console-sovelluksen** malli ja anna sille nimi **EchoService**. Valitse **OK** , jos haluat luoda projektin.

    ![][2]

3. Asenna Service Bus NuGet-paketti. Tämän paketin lisää automaattisesti palvelun Bus kirjastoista sekä WCF **System.ServiceModel**viittauksia. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) on nimitilan, jonka avulla voit käyttää ohjelmallisesti WCF perusominaisuudet. Palvelun Bus määrittää monia objektien ja määritteiden WCF avulla palvelun sopimuksia.

    Napsauta ratkaisunhallinnassa-ratkaisun hiiren kakkospainikkeella ja valitse sitten **Ratkaisu NuGet pakettien hallinta**. **Selaa** -välilehti ja valitse Hae `Microsoft Azure Service Bus`. Varmista, että projektin nimeä valittu **versiot** -ruutuun. Valitse **Asenna**ja hyväksy käyttöehdot.

    ![][3]

3. Napsauta ratkaisunhallinnassa Kaksoisnapsauta-editorin avaaminen Program.cs-tiedostoa, jos se ei ole valmiiksi avoinna.

4. Lisää seuraava lauseiden käyttämisestä tiedoston yläosassa:

    ```
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```

1. Muuttaa nimitilan nimi **EchoService** sen oletusnimi **Microsoft.ServiceBus.Samples**.

    >[AZURE.IMPORTANT] Tässä opetusohjelmassa käyttää C#-nimitilan **Microsoft.ServiceBus.Samples**, hallita Sopimustyyppi, jota käytetään määritystiedostossa [määrittäminen WCF-asiakas](#configure-the-wcf-client) -vaiheessa nimitilan eli. Voit määrittää minkä tahansa nimitilan, kun luot tässä esimerkissä; kuitenkin opetusohjelman ei toimi, jos sitten Muokkaa sopimuksen nimitilan ja palvelun näin ollen sovelluksen. Nimitilan, määritetty App.config tiedostossa on oltava sama kuin tiedostojen C# määritettyä nimitilaa.

1. Suoraan jälkeen `Microsoft.ServiceBus.Samples` nimitilan määritys mutta nimitilan, määrittää uuden käyttöliittymän nimeltä `IEchoContract` ja ottaa käyttöön `ServiceContractAttribute` määrite liittymään **http://samples.microsoft.com/ServiceModel/Relay/**nimitilan arvot. Nimitilan poikkeaa nimitilan, jota käytetään koko koodi aluetta. Sen sijaan nimitilan arvoa käytetään yksilöllinen kuin tässä sopimuksessa. Nimitilan, joka määrittää erikseen estää nimitilan oletusarvo lisätään sopimuksen nimeä.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

    >[AZURE.NOTE] Yleensä palvelusopimuksen nimitila on nimeämiskäytäntö malli, joka sisältää versiotiedot. Versiotiedot sisällyttäminen service-palvelusopimus nimitilan ottaa käyttöön palvelut eristää määrittämällä uusi service-palvelusopimus uusi nimitila ja paljastaa uuden päätepisteen suurimpiin muutoksiin. Tällä tavalla asiakkaat edelleen käyttää vanha service-palvelusopimus eikä sinun tarvitse päivittää. Versiotiedot voi olla päivämäärä tai muodosta numero. Lisätietoja on artikkelissa [Palvelun versiotiedot](http://go.microsoft.com/fwlink/?LinkID=180498). Tässä opetusohjelmassa tarkoitetaan service-palvelusopimus nimitilan nimeämiskäytäntö malli ei sisällä versiotiedot.

1. Sisällä `IEchoContract` liittymän, määritellä laskentatapaa yhdellä kertaa `IEchoContract` sopimuksen paljastaa käyttöliittymässä ja käytä `OperationContractAttribute` määrite menetelmä, jonka haluat näyttää julkisen palvelun Bus sopimuksen osana.

    ```
    [OperationContract]
    string Echo(string text);
    ```

1. Suoraan jälkeen `IEchoContract` liittymän määritys, määritellä on kanavalla, molemmat perii `IEchoContract` ja myös `IClientChannel` liitäntä, kuten kuvassa:

    ```
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    Kanavan on hyödyntämällä isännän ja asiakkaan välittää tiedot toisiinsa WCF-objekti. Kirjoitetaan myöhemmin, voit tuoda tietoja sovellusten välisen kanavan koodia.

1. **Muodosta** -valikosta **Muodosta ratkaisu** tai paina **Ctrl + Vaihto + B** Vahvista työsi tarkkuudella tähän mennessä.

### <a name="example"></a>Esimerkki

Seuraava koodi näkyy basic käyttöliittymä, joka määrittää Service Bus-palvelusopimus.

```
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Käyttöliittymän on luotu, voit toteuttaa käyttöliittymässä.

## <a name="implement-the-wcf-contract-to-use-service-bus"></a>Toteuta käyttämään palvelua Bus WCF-palvelusopimus

Palvelun Bus välitys luominen edellyttää, että luot ensin sopimuksen, jonka avulla määritetään. Lisätietoja liittymän luomisesta on artikkelissa edellisessä vaiheessa. Seuraavaksi toteuta liittymää. Tämä koskee nimeltä luokan luominen `EchoService` , joka toteuttaa käyttäjän määrittämä `IEchoContract` käyttöliittymän. Sen jälkeen, kun otat käyttöliittymä, valitse Käytä App.config määritystiedostoa käyttöliittymän määrittäminen. Määritystiedosto sisältää tarvittavat tiedot sovelluksen, kuten palvelun nimi ja sopimus ja protokolla, jota käytetään pitää yhteyttä palvelun Bus nimiä. Näiden tehtävien käytettävä koodi on annettu esimerkin ohjeiden mukaisesti. Kohdassa [Käyttöönoton palvelun sopimuksia](https://msdn.microsoft.com/library/ms733764.aspx) yleisiä keskustelu ottamisesta käyttöön sopimus WCF-ohjeista.

1. Luo uusi luokka-niminen `EchoService` suoraan määritelmään jälkeen `IEchoContract` käyttöliittymän. `EchoService` Luokan toteuttaa `IEchoContract` käyttöliittymän. 

    ```
    class EchoService : IEchoContract
    {
    }
    ```
    
    Käyttöliittymän muiden käyttöotot tapaan määritelmä voit toteuttaa toiseen. Kuitenkin tässä opetusohjelmassa, käyttöönoton sijaitsee liitännän määrityksen samaa tiedostoa ja `Main` menetelmää.

1. Käytä [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) -määrite `IEchoContract` käyttöliittymän. Määrite määrittää palvelunimi ja nimitila. Kun teet näin, `EchoService` luokan tulee näkyviin seuraavasti:

    ```
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```

1. Ota käyttöön `Echo` määritelty menetelmä `IEchoContract` liittymän `EchoService` luokan. 

    ```
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```

1. Valitse **Muodosta**ja valitse sitten **Muodosta ratkaisu** vahvistat työsi.

### <a name="to-define-the-configuration-for-the-service-host"></a>Määritä service-isännän kokoonpano

1. Määritystiedoston muistuttaa WCF määritystiedostoa. Se sisältää palvelunimi, päätepisteen (eli palvelun Bus paljastaa asiakkaat ja isännät keskenään sijainti) ja sidonta (protokolla, jota käytetään vaihtamaan tyyppi). Tärkein ero on määritetty palvelun tämä päätepiste viittaa [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) sidonta, joka ei kuulu .NET Framework. [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) on palvelun Bus määrittämiä sidontojen.

1. Kaksoisnapsauta **Ratkaisunhallinnassa**Visual Studio-editorin avaaminen App.config-tiedostoa.

2. Valitse `<appSettings>` elementti, nimi palvelun nimitila ja Suojaussidokset paikkamerkit avaimen aiemmassa vaiheessa kopioimasi korvaa. 

1. Sisällä `<system.serviceModel>` tunnisteita, lisätä `<services>` elementti. Voit määrittää palvelun Bus lisäsovellukset yksittäisen määritystiedostossa. Tässä opetusohjelmassa määrittää kuitenkin vain yksi.
 
    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```

1. Sisällä `<services>` elementti, lisätä `<service>` osa, kun haluat määrittää palvelun nimi.

    ```
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```

1. Sisällä `<service>` elementti, Määritä sijainti päätepisteen palvelusopimuksen ja sidonta päätepisteen tyyppi.

    ```
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    Päätepisteen määrittää, jossa asiakkaan näyttää Host (isäntä)-sovelluksessa. Myöhemmin opetusohjelman käytetään luomaan URI, joka paljastaa täysin palvelun Bus isäntä tämän vaiheen. Sidonta ilmoittaa, että käytät TCP-protokolla pitää yhteyttä palvelun Bus.

1. Napsauta **Muodosta** -valikossa **Luominen ratkaisu** vahvistat työsi.

### <a name="example"></a>Esimerkki

Seuraava koodi näkyy service-palvelusopimus soveltaminen.

```
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

Seuraava koodi näkyy perusmuotoilu liittyvän palvelun host App.config-tiedostoa.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-to-register-with-service-bus"></a>Isännöidä ja suorita rekisteröityä palveluun Bus basic Web-palvelu

Tässä vaiheessa käsitellään basic palvelun Bus-palvelun.

### <a name="to-create-the-service-bus-credentials"></a>Voit luoda palvelun Bus tunnistetiedot

1. Valitse `Main()`, Luo kahden muuttujan, johon haluat tallentaa nimitilan ja Suojaussidokset avainta, joka luetaan console-ikkunasta.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    Suojaussidosten avaimen käytetään myöhemmin käyttää palvelun Bus projektin. Nimitilan välitetään parametrina `CreateServiceUri` palvelun URI luomiseen.

4. Käytä [TransportClientEndpointBehavior](https://msdn.microsoft.com/library/microsoft.servicebus.transportclientendpointbehavior.aspx) -objekti, määritellä, että käytät Suojaussidosten avaimen tunnistetiedon tyyppi. Lisää seuraava koodi suoraan viimeisessä vaiheessa lisännyt koodin jälkeen.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="to-create-a-base-address-for-the-service"></a>Perusosoitteen palvelun luominen

1. Seuraavan lisäämäsi viimeisessä vaiheessa koodin luominen `Uri` palvelun perusosoitteen esiintymän. Tämä URI määrittää palvelun Bus-malli, nimitila ja palvelun-liittymän polku.

    ```
    Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

    "sb" on lyhenne sanoista palvelun Bus-malli ja ilmaisee, että käytät TCP-protokolla. Tämä on myös aiemmin merkitty määritystiedostossa, kun [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) on määritetty sidonta.
    
    Tässä opetusohjelmassa URI on `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="to-create-and-configure-the-service-host"></a>Voit luoda ja määrittää isännöinti

1. Yhteyden tilan asettaminen `AutoDetect`.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    Yhteyden tilan kuvataan palvelu käyttää pitää yhteyttä palvelun Bus; protokolla joko HTTP tai TCP. Oletusasetuksia `AutoDetect`-palvelu yrittää muodostaa yhteyden palvelun Bus TCP, jos se on käytettävissä ja HTTP, jos TCP ei ole käytettävissä. Huomaa, että tämä eroaa protokolla palvelun määrittää asiakkaan liikenne. Että protokolla määräytyy sidonta, jota käytetään. Palvelu voi käyttää esimerkiksi [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) sidonta, joka määrittää, että sen päätepisteen (tarjoamia-palvelun Bus) HTTP kommunikoi asiakkaiden kanssa. Samaa palvelua määrittää **ConnectivityMode.AutoDetect** niin, että palvelu kommunikoi kanssa palvelun Bus TCP-protokollaa.

1. Luo käyttämällä aiemmin luotu sisältö URI service-isännässä.

    ```
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    Service-isäntä WCF-objekti, joka muodostaa palvelu. Tässä kohdassa voit välittää ne luotavan palvelutyypin ( `EchoService` tyyppi), ja myös osoite, jossa haluat alkuperän.

1. Lisää viitteet [System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) ja [Microsoft.ServiceBus.Description](https://msdn.microsoft.com/library/microsoft.servicebus.description.aspx)Program.cs tiedoston alkuun.

    ```
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```

1. Siirry takaisin `Main()`, jotta yleisön päätepisteen määrittäminen.

    ```
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Tässä vaiheessa ilmoittaa, että sovelluksesi löytyy julkisesti tarkastelemalla palvelun Bus ATOM palvelun Bus syötteen projektin. Jos määrität **DiscoveryType** **yksityiseksi**, on jokin muu sähköpostiohjelma edelleen voi käyttää palvelua. Palvelun kuitenkin näy, kun se hakee palvelun Bus nimitilan. Sen sijaan asiakas on tiedettävä päätepisteen polku etukäteen.

1. Koskevat palvelun käyttäjätiedot App.config tiedostossa määritettyjä Palvelupäätepisteet:

    ```
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Ilmoitetulla edellisessä vaiheessa voit voitu ovat ilmoittaneet useita palvelut ja päätepisteet määritystiedostossa. Jos haluat käyttää, koodi käy läpi kokoonpanotiedosto ja Etsi jokaisen päätepisteen, johon se on voimassa tunnistetiedot. Kuitenkin tässä opetusohjelmassa määritystiedosto on vain yksi päätepiste.

### <a name="to-open-the-service-host"></a>Avaa palvelu isäntä

1. Avaa palvelu.

    ```
    host.Open();
    ```

1. Ilmoittaa, että palvelu on käynnissä ja kerrotaan, kuinka voit sulkea palvelun käyttäjälle.

    ```
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] to exit");
    Console.ReadLine();
    ```

1. Kun olet valmis, sulje palvelun isäntä.

    ```
    host.Close();
    ```

1. Paina **Ctrl + Vaihto + B** projektin luonnissa.

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä sisältää opetusohjelman sopimus ja toteutuksen edelliset vaiheet ja isännöi palvelua console-sovelluksessa. Käännä suoritettavan nimeltä EchoService.exe seuraavassa.

```
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         
          
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create the credentials object for the endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create the service URI based on the service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create the service host reading the configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create the ServiceRegistrySettings behavior for the endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add the Service Bus credentials to all endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }
            
            // Open the service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] to exit");
            Console.ReadLine();

            // Close the service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-the-service-contract"></a>Luo WCF työasemaohjelmista service-palvelusopimus

Seuraavaksi voit luoda basic palvelun Bus asiakassovellus ja määrittää otat myöhemmissä vaiheissa service-palvelusopimus. Huomaa, että monia näistä vaiheista muistuttavat palvelun luonnissa käytetyt vaiheet: määrittäminen sopimuksen, muokkaaminen App.config tiedoston muodostaminen palvelun Bus tunnistetietojen avulla ja niin edelleen. Näiden tehtävien käytettävä koodi on annettu esimerkin ohjeiden mukaisesti.

1. Luo uuden projektin nykyisen Visual Studio-ratkaisun asiakkaan toimimalla seuraavasti:
    1. Napsauta ratkaisunhallinnassa samaan ratkaisuun, joka sisältää palvelun nykyiseen ratkaisuun (ei projekti) hiiren kakkospainikkeella ja valitse **Lisää**. Valitse **Uusi projekti**.
    2. Valitse **Lisää uusi projekti** -valintaikkunan **Visual C#** (Jos **Visual C#** ei näy, katso kohdan **Muut kielet**), valitse **Console-sovelluksen** malli ja anna sille nimi **EchoClient**.
    3. Valitse **OK**.
<br />

1. Napsauta ratkaisunhallinnassa Kaksoisnapsauta **EchoClient** -editorin avaaminen Projectissa Program.cs-tiedostoa, jos se ei ole valmiiksi avoinna.

1. Muuttaa sen oletusnimi nimitilan `EchoClient` , `Microsoft.ServiceBus.Samples`.

1. Asenna [Service Bus NuGet paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Napsauta ratkaisunhallinnassa **EchoClient** projektin hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**. **Selaa** -välilehti ja valitse Hae `Microsoft Azure Service Bus`. Valitse **Asenna**ja hyväksy käyttöehdot.

    ![][3]

1. Lisää `using` tietosuojatiedot [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) nimitilan Program.cs-tiedostossa. 

    ```
    using System.ServiceModel;
    ```

1. Lisää palvelusopimus-määritys nimitilan, kuten seuraavassa esimerkissä. Huomaa, että tämä määritelmä on sama kuin määritys, jota käytetään **palvelun** Projectissa. Lisää tämä koodi yläreunassa `Microsoft.ServiceBus.Samples` nimitilan.

    ```
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

1. Paina **Ctrl + Vaihto + B** luonnissa asiakas.

### <a name="example"></a>Esimerkki

Seuraava koodi näkyy Program.cs tiedoston nykyisen tilan EchoClient Projectissa.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-the-wcf-client"></a>Määritä WCF-asiakas

Tässä vaiheessa voit luoda basic asiakassovellus, joka käyttää loit aiemmin tässä opetusohjelmassa palvelun App.config-tiedostoksi. App.config tiedoston määrittää sopimuksen, sidonta ja päätepisteen nimi. Näiden tehtävien käytettävä koodi on annettu esimerkin ohjeiden mukaisesti.

1. Kaksoisnapsauta ratkaisunhallinnassa, **EchoClient** Projectissa, **App.config** tiedoston avaamiseen Visual Studio-editorissa.

2. Valitse `<appSettings>` elementti, nimi palvelun nimitila ja Suojaussidokset paikkamerkit avaimen aiemmassa vaiheessa kopioimasi korvaa.

1. System.serviceModel elementissä Lisää `<client>` elementti.

    ```
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Tässä vaiheessa ilmoittaa, että määrität asiakassovellus WCF-tyyppiseksi.

1. Sisällä `client` elementti, Määritä nimi, sopimus ja päätepisteen sidontatyyppi.

    ```
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Tässä vaiheessa määrittää päätepisteen määritetty palvelu ja asiakassovellus käyttää TCP pitää yhteyttä palvelun Bus kertoma sopimuksen nimi. Päätepisteen nimi käytetään seuraavaksi linkitettävän päätepisteen määritysten URI-palvelun kanssa.

1. Valitse **Tiedosto**ja valitse **Tallenna kaikki**.

## <a name="example"></a>Esimerkki

Seuraava koodi näkyy Kaiku asiakkaan App.config-tiedostoa.

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-the-wcf-client-to-call-service-bus"></a>WCF-asiakas, voit soittaa palvelun Bus Toteuta

Tässä vaiheessa voit toteuttaa basic asiakassovellus, joka käyttää palvelua, jonka loit aiemmin tässä opetusohjelmassa. Palvelun tapaan asiakkaan suorittaa monia käyttämään palvelua Bus samat toiminnot:

1. Asettaa yhteyden tilan.
1. Luo, joka määrittää isäntäpalvelu URI.
1. Määrittää suojausvaltuudet.
1. Koskee yhteyden tunnistetiedot.
1. Avaa yhteyden.
1. Suorittaa-sovelluksen toimenpiteet.
1. Sulkee yhteyden.

Yksi tärkein ero on kuitenkin, että asiakassovellus muodostaa kanavan palvelun Bus olisi palvelu käyttää **ServiceHost**kutsu. Näiden tehtävien käytettävä koodi on annettu esimerkin ohjeiden mukaisesti.

### <a name="to-implement-a-client-application"></a>Toteuttamisesta asiakassovellus

1. Määrittää yhteyden tilan **automaattinen tunnistaminen**. Lisää seuraava koodi sisällä `Main()` **EchoClient** käyttötapa.

    ```
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

1. Määritä muuttujat pitoon, joka luetaan konsolin palvelun nimitila ja Suojaussidosten avaimen arvot.

    ```
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```

1. Luo, joka määrittää isännän sijainnin palvelun Bus projektin URI.

    ```
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```

1. Luo service-nimitilan päätepisteen tunnistetiedon-objekti.

    ```
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

1. Luo kanavan factory, lataa määritys-App.config-tiedostossa.

    ```
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    Kanavan factory on WCF-objekti, joka luo kanavalle, jonka kautta palvelu ja asiakas-sovellusten viestiä.

1. Käytä palvelun Bus tunnistetiedot.

    ```
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```

1. Luo ja avaa kanava-palveluun.

    ```
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```

1. Kirjoita basic käyttöliittymän ja kaikua toimintoja.

    ```
    Console.WriteLine("Enter text to echo (or [Enter] to exit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Huomaa, että koodin kanavan objektin esiintymän kuin välityspalvelimen palvelulle.

1. Sulje kanavaa, ja sulje valmiiksi.

    ```
    channel.Close();
    channelFactory.Close();
    ```

## <a name="to-run-the-applications"></a>Jos haluat suorittaa sovellukset

1. Paina **Ctrl + Vaihto + B** Rakenna ratkaisu. Tämä muodostaa asiakas-projekti ja palvelun projekti, jonka loit edellisessä vaiheessa.

2. Varmista ennen kuin suoritat asiakassovellus, että palvelusovelluksen on käynnissä. Napsauta ratkaisunhallinnassa Visual Studiossa **EchoService** -ratkaisun hiiren kakkospainikkeella ja valitse **Ominaisuudet**.

3. Valitse ratkaisun ominaisuudet-valintaikkunassa **Projektin aloitus**ja valitse **useita käynnistys projektit** -painiketta. Varmista, että **EchoService** näkyy luettelossa ensimmäisenä. 

4. Määritä **EchoService** ja **EchoClient** projektien **Action** -ruutuun **alkavan**.

    ![][5]

5. Valitse **Projektiriippuvuudet**. Valitse **Projektit** -ruudussa **EchoClient**. **Riippuvuus** -ruutuun Varmista, että **EchoService** on valittuna.

    ![][6]

6. Valitse Hylkää **Ominaisuudet** -valintaikkunassa **OK** .

7. Painamalla **F5-näppäintä** voit suorittaa molemmat projektit.

8. Konsolin molemmat Avaa ja pyydä nimitilan nimi. Palvelun on suorittaa ensimmäisen, kirjoita niin nimitilan **EchoService** -konsolipuussa ja paina sitten **Enter**.

9. Sinua kehotetaan seuraavaksi SAS-näppäintä. SAS-näppäintä ja painamalla ENTER.

    Tässä on esimerkki tulosteen konsoli-ikkuna. Huomaa, että arvot tässä on esimerkki ilmoitusta.

    `Your Service Namespace: myNamespace`
    `Your SAS Key: <SAS key value>`

    Konsoli-ikkunan palvelusovelluksen tulostaa osoite, johon se on kuunteleminen, joka näkyy seuraavassa esimerkissä.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/`
    `Press [Enter] to exit`
    
10. Kirjoita **EchoClient** -konsolipuussa palvelusovelluksen aiemmin kirjoittamasi tiedot. Noudata edellä kuvatut vaiheet Kirjoita saman palvelun nimitila ja Suojaussidosten avainarvot asiakassovellus.

11. Kun olet lisännyt nämä arvot, Avaa kanavan palveluun ja pyytää sinua kirjoittamaan tekstiä konsolin tulostus-esimerkissä tarkastelu.

    `Enter text to echo (or [Enter] to exit):` 

    Kirjoita tekstiä palvelusovelluksen lähettäminen ja paina Enter-näppäintä. Tämä teksti lähetetään palvelun kautta Kaiku palvelun-toiminto ja kuten seuraavassa esimerkissä tulos palvelun console-ikkunassa.

    `Echoing: My sample text`

    Asiakassovellus vastaanottaa palautusarvo `Echo` toiminto, joka on alkuperäisen tekstin, ja tulostaa sen console-ikkunaan. Seuraavassa on esimerkki tulosteen asiakkaan konsoli-ikkuna.

    `Server echoed: My sample text`

12. Voit jatkaa tekstiviestien lähettäminen asiakkaan palvelun tällä tavalla. Kun olet valmis, Lopeta molemmat sovellukset asiakkaan ja palvelun konsoli-ikkunoiden Enter-näppäintä.

## <a name="example"></a>Esimerkki

Seuraavassa esimerkissä luomisesta asiakassovellus, miten kutsu palvelun toimintoja ja sulkemisesta asiakkaan toiminto puhelun päätyttyä.

```
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;

            
            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text to echo (or [Enter] to exit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa ilmeni muodostaminen palvelun Bus asiakkaan sähköpostisovelluksen ja -palvelu palvelun Bus "välitys"-ominaisuuksia. Samalla opetusohjelmaan käyttävän palvelun Bus [viestit](../service-bus-messaging/service-bus-messaging-overview.md#Brokered-messaging)-kohdassa [Palvelu Bus se Messaging .NET opetusohjelma](../service-bus-messaging/service-bus-brokered-tutorial-dotnet.md).

Lisätietoja palvelun Bus on seuraavissa artikkeleissa.

- [Palvelun Bus tekstiviesti yleiskatsaus](../service-bus-messaging/service-bus-messaging-overview.md)
- [Palvelun Bus perusteet](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- [Palvelun Bus-arkkitehtuuri](../service-bus-messaging/service-bus-architecture.md)

[Azure classic portal]: http://manage.windowsazure.com

[1]: ./media/service-bus-relay-tutorial/service-bus-policies.png
[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[4]: ./media/service-bus-relay-tutorial/create-ns.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
