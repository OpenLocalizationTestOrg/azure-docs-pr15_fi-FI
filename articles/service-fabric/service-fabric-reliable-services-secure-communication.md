<properties
   pageTitle="Ohje: n suojattu tietoliikenne Services-palvelun kangasta | Microsoft Azure"
   description="Yleiskatsaus siitä, kuinka voit luotettavia palveluja Azure palvelun kangasta-klusterin käynnissä olevat tietosuojaa."
   services="service-fabric"
   documentationCenter=".net"
   authors="suchiagicha"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/06/2016"
   ms.author="suchiagicha"/>

# <a name="help-secure-communication-for-services-in-azure-service-fabric"></a>Suojattu tietoliikenne Azure palvelun kangasta-palvelujen avulla

Suojaus on communication tärkeimmät ominaisuuksia. Luotettavan palveluiden sovelluksen framework on muutama valmiin viestintä pinoa ja työkaluja, joilla voit parantaa suojausta. Tässä artikkelissa keskustella siitä, miten voit parantaa suojausta, kun käytät palvelun Etäpalvelujen ja Windows Communication Foundation (WCF) viestintä pinossa.

## <a name="help-secure-a-service-when-youre-using-service-remoting"></a>Suojata palvelu, kun käytät palvelun Etäpalvelujen

Esimerkissä käytössä olemassa [Esimerkki](service-fabric-reliable-services-communication-remoting.md) , jossa kerrotaan Etäpalvelujen luotettava palvelujen määrittäminen. Voit suojata palvelu kun käytät palvelun Etäpalvelujen, toimimalla seuraavasti:

1. Luo käyttöliittymään, `IHelloWorldStateful`, joka määrittää menetelmät, jotka ovat käytettävissä etäproseduurikutsu palvelustasi. Palvelun käyttää `FabricTransportServiceRemotingListener`, joka on määritetty `Microsoft.ServiceFabric.Services.Remoting.FabricTransport.Runtime` nimitilan. Tämä on `ICommunicationListener` , joka sisältää Etäpalvelujen ominaisuuksien käyttöönotto.

    ```csharp
    public interface IHelloWorldStateful : IService
    {
        Task<string> GetHelloWorld();
    }

    internal class HelloWorldStateful : StatefulService, IHelloWorldStateful
    {
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]{
                    new ServiceReplicaListener(
                        (context) => new FabricTransportServiceRemotingListener(context,this))};
        }

        public Task<string> GetHelloWorld()
        {
            return Task.FromResult("Hello World!");
        }
    }
    ```

2. Lisää kuuntelijan asetukset ja suojausvaltuudet.

    Varmista, että varmenne, jota haluat käyttää palvelun tietoliikenne suojaamiseen on asennettu-klusterin solmut. Voit antaa kuuntelijan asetukset ja suojausvaltuudet kahdella tavalla:

    1. Anna ne suoraan palvelun koodissa:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            FabricTransportListenerSettings listenerSettings = new FabricTransportListenerSettings
            {
                MaxMessageSize = 10000000,
                SecurityCredentials = GetSecurityCredentials()
            };
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(context,this,listenerSettings))
            };
        }

        private static SecurityCredentials GetSecurityCredentials()
        {
            // Provide certificate details.
            var x509Credentials = new X509Credentials
            {
                FindType = X509FindType.FindByThumbprint,
                FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
                StoreLocation = StoreLocation.LocalMachine,
                StoreName = "My",
                ProtectionLevel = ProtectionLevel.EncryptAndSign
            };
            x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");
            return x509Credentials;
        }
        ```
    2. Anna niiden [config paketin](service-fabric-application-model.md)avulla:

        Lisää `TransportSettings` settings.xml tiedosto-osassa.

        ```xml
        <!--Section name should always end with "TransportSettings".-->
        <!--Here we are using a prefix "HelloWorldStateful".-->
        <Section Name="HelloWorldStatefulTransportSettings">
            <Parameter Name="MaxMessageSize" Value="10000000" />
            <Parameter Name="SecurityCredentialsType" Value="X509" />
            <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
            <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
            <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
            <Parameter Name="CertificateStoreName" Value="My" />
            <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
            <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
        </Section>
        ```

        Tässä tapauksessa `CreateServiceReplicaListeners` tapa näyttää tältä:

        ```csharp
        protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
        {
            return new[]
            {
                new ServiceReplicaListener(
                    (context) => new FabricTransportServiceRemotingListener(
                        context,this,FabricTransportListenerSettings.LoadFrom("HelloWorldStateful")))
            };
        }
        ```

         Jos lisäät `TransportSettings` ilman mitään etuliitettä settings.xml-tiedosto kohdassa `FabricTransportListenerSettings` Lataa kaikki asetukset tästä osasta oletusarvoisesti.

         ```xml
         <!--"TransportSettings" section without any prefix.-->
         <Section Name="TransportSettings">
             ...
         </Section>
         ```
         Tässä tapauksessa `CreateServiceReplicaListeners` tapa näyttää tältä:

         ```csharp
         protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
         {
             return new[]
             {
                 return new[]{
                         new ServiceReplicaListener(
                             (context) => new FabricTransportServiceRemotingListener(context,this))};
             };
         }
         ```

3. Kun soitat menetelmiä suojatun palveluun käyttämällä Etäpalvelujen-pino sijaan `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` luokan luominen palveluvälityspalvelimen, käytä `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxyFactory`. Välitä `FabricTransportSettings`, joka sisältää `SecurityCredentials`.

    ```csharp

    var x509Credentials = new X509Credentials
    {
        FindType = X509FindType.FindByThumbprint,
        FindValue = "4FEF3950642138446CC364A396E1E881DB76B48C",
        StoreLocation = StoreLocation.LocalMachine,
        StoreName = "My",
        ProtectionLevel = ProtectionLevel.EncryptAndSign
    };
    x509Credentials.RemoteCommonNames.Add("ServiceFabric-Test-Cert");

    FabricTransportSettings transportSettings = new FabricTransportSettings
    {
        SecurityCredentials = x509Credentials,
    };

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(transportSettings));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Jos asiakas-koodi on käynnissä osana palvelu, voit ladata `FabricTransportSettings` settings.xml-tiedostosta. Luo TransportSettings osa, joka on samanlainen palvelukoodi-esitetyllä aiemmassa versiossa. Tee seuraavat muutokset asiakas-koodi:

    ```csharp

    ServiceProxyFactory serviceProxyFactory = new ServiceProxyFactory(
        (c) => new FabricTransportServiceRemotingClientFactory(FabricTransportSettings.LoadFrom("TransportSettingsPrefix")));

    IHelloWorldStateful client = serviceProxyFactory.CreateServiceProxy<IHelloWorldStateful>(
        new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

    Jos asiakas ei ole käynnissä osana palvelu, voit luoda client_name.settings.xml tiedoston client_name.exe missä samassa sijainnissa. Valitse Luo tiedosto TransportSettings osa.

    Vastaavat uudelleen, jos haluat lisätä `TransportSettings` ilman mitään etuliitettä settings.xml/client_name.settings.xml asiakas-osassa `FabricTransportSettings` Lataa kaikki asetukset tästä osasta oletusarvoisesti.

    Tässä tapauksessa aiemmissa koodin täsmentää yksinkertaistettu:  

    ```csharp

    IHelloWorldStateful client = ServiceProxy.Create<IHelloWorldStateful>(
                 new Uri("fabric:/MyApplication/MyHelloWorldService"));

    string message = await client.GetHelloWorld();

    ```

## <a name="help-secure-a-service-when-youre-using-a-wcf-based-communication-stack"></a>Suojata palvelu, kun käytät WCF-pohjainen viestintä-pino

Esimerkissä käytössä olemassa [Esimerkki](service-fabric-reliable-services-communication-wcf.md) , jossa kerrotaan WCF-liikennettä pino luotettava palvelujen määrittäminen. Suojaamiseen palvelu, kun käytät WCF-liikennettä pino, toimi seuraavasti:

1. Palvelun, sinun on suojata WCF viestintä kuuntelua (`WcfCommunicationListener`), jotka luot. Voit tehdä tämän muokkaaminen `CreateServiceReplicaListeners` menetelmää.

    ```csharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        return new[]
        {
            new ServiceReplicaListener(
                this.CreateWcfCommunicationListener)
        };
    }

    private WcfCommunicationListener<ICalculator> CreateWcfCommunicationListener(StatefulServiceContext context)
    {
       var wcfCommunicationListener = new WcfCommunicationListener<ICalculator>(
            serviceContext:context,
            wcfServiceObject:this,
            // For this example, we will be using NetTcpBinding.
            listenerBinding: GetNetTcpBinding(),
            endpointResourceName:"WcfServiceEndpoint");

        // Add certificate details in the ServiceHost credentials.
        wcfCommunicationListener.ServiceHost.Credentials.ServiceCertificate.SetCertificate(
            StoreLocation.LocalMachine,
            StoreName.My,
            X509FindType.FindByThumbprint,
            "9DC906B169DC4FAFFD1697AC781E806790749D2F");
        return wcfCommunicationListener;
    }

    private static NetTcpBinding GetNetTcpBinding()
    {
        NetTcpBinding b = new NetTcpBinding(SecurityMode.TransportWithMessageCredential);
        b.Security.Message.ClientCredentialType = MessageCredentialType.Certificate;
        return b;
    }
    ```

2. Valitse-asiakasohjelmassa `WcfCommunicationClient` luokka, joka on luotu edellisessä [esimerkissä](service-fabric-reliable-services-communication-wcf.md) säilyy muuttumattomana. Mutta haluat ohittaa `CreateClientAsync` maksutavan `WcfCommunicationClientFactory`:

    ```csharp
    public class SecureWcfCommunicationClientFactory<TServiceContract> : WcfCommunicationClientFactory<TServiceContract> where TServiceContract : class
    {
        private readonly Binding clientBinding;
        private readonly object callbackObject;
        public SecureWcfCommunicationClientFactory(
            Binding clientBinding,
            IEnumerable<IExceptionHandler> exceptionHandlers = null,
            IServicePartitionResolver servicePartitionResolver = null,
            string traceId = null,
            object callback = null)
            : base(clientBinding, exceptionHandlers, servicePartitionResolver,traceId,callback)
        {
            this.clientBinding = clientBinding;
            this.callbackObject = callback;
        }

        protected override Task<WcfCommunicationClient<TServiceContract>> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
        {
            var endpointAddress = new EndpointAddress(new Uri(endpoint));
            ChannelFactory<TServiceContract> channelFactory;
            if (this.callbackObject != null)
            {
                channelFactory = new DuplexChannelFactory<TServiceContract>(
                this.callbackObject,
                this.clientBinding,
                endpointAddress);
            }
            else
            {
                channelFactory = new ChannelFactory<TServiceContract>(this.clientBinding, endpointAddress);
            }
            // Add certificate details to the ChannelFactory credentials.
            // These credentials will be used by the clients created by
            // SecureWcfCommunicationClientFactory.  
            channelFactory.Credentials.ClientCertificate.SetCertificate(
                StoreLocation.LocalMachine,
                StoreName.My,
                X509FindType.FindByThumbprint,
                "9DC906B169DC4FAFFD1697AC781E806790749D2F");
            var channel = channelFactory.CreateChannel();
            var clientChannel = ((IClientChannel)channel);
            clientChannel.OperationTimeout = this.clientBinding.ReceiveTimeout;
            return Task.FromResult(this.CreateWcfCommunicationClient(channel));
        }
    }
    ```

    Käytä `SecureWcfCommunicationClientFactory` Luo WCF viestintä-asiakas (`WcfCommunicationClient`). Asiakkaan avulla voit käynnistää palvelun tavoista.

    ```csharp
    IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();

    var wcfClientFactory = new SecureWcfCommunicationClientFactory<ICalculator>(clientBinding: GetNetTcpBinding(), servicePartitionResolver: partitionResolver);

    var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
        wcfClientFactory,
        ServiceUri,
        ServicePartitionKey.Singleton);

    var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
        client => client.Channel.Add(2, 3)).Result;
    ```

## <a name="next-steps"></a>Seuraavat vaiheet

* [Verkko-Ohjelmointirajapinnan kanssa OWIN luotettava Services-palveluissa](service-fabric-reliable-services-communication-webapi.md)
