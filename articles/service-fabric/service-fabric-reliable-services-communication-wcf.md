<properties
   pageTitle="Luotettava Services WCF viestintä pinon | Microsoft Azure"
   description="Valmiin WCF-viestintä pino palvelun kangasta sisältää Asiakaspalvelu WCF viestintä luotettavia palveluja varten."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="bharatn"/>

# <a name="wcf-based-communication-stack-for-reliable-services"></a>WCF-liikennettä pino luotettavia palveluja
Luotettavia palveluja framework avulla palvelun käyttäjät voivat valita viestintä pino, joita he haluavat niiden palveluun on oma. Ne voivat Liitä viestintä Pinotut haluamaansa palauttama [CreateServiceReplicaListeners tai CreateServiceInstanceListeners](service-fabric-reliable-services-communication.md) menetelmiä **ICommunicationListener** kautta. Puitteissa on toteutus viestintä-pino perusteella Windows Communication Foundation (WCF)-palvelun tekijät, jotka haluavat käyttää WCF-liikennettä varten.

## <a name="wcf-communication-listener"></a>WCF viestintä Listener
WCF-kohtaisia käyttöönoton **ICommunicationListener** tarjoaa **Microsoft.ServiceFabric.Services.Communication.Wcf.Runtime.WcfCommunicationListener** -luokka.

Lest vaikkapa sopimus tyyppi on`ICalculator`

```csharp
[ServiceContract]
public interface ICalculator
{
    [OperationContract]
    Task<int> Add(int value1, int value2);
}
```

Voit luoda WCF viestintä kuuntelija palvelussa seuraavalla tavalla.

```csharp

protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[] { new ServiceReplicaListener((context) =>
        new WcfCommunicationListener<ICalculator>(
            wcfServiceObject:this,
            serviceContext:context,
            //
            // The name of the endpoint configured in the ServiceManifest under the Endpoints section
            // that identifies the endpoint that the WCF ServiceHost should listen on.
            //
            endpointResourceName: "WcfServiceEndpoint",

            //
            // Populate the binding information that you want the service to use.
            //
            listenerBinding: WcfUtility.CreateTcpListenerBinding()
        )
    )};
}

```

## <a name="writing-clients-for-the-wcf-communication-stack"></a>WCF-viestintä pinon asiakkaat kirjoittaminen
Kirjoittamisen yhteydessä palveluihin käyttämällä WCF asiakkaiden puitteissa tarjoaa **WcfClientCommunicationFactory**, joka on [ClientCommunicationFactoryBase](service-fabric-reliable-services-communication.md)WCF-kohtaisia käyttöönoton.

```csharp

public WcfCommunicationClientFactory(
    Binding clientBinding = null,
    IEnumerable<IExceptionHandler> exceptionHandlers = null,
    IServicePartitionResolver servicePartitionResolver = null,
    string traceId = null,
    object callback = null);
```

WCF-tietoliikenneyhteyden voidaan käyttää **WcfCommunicationClient** **WcfCommunicationClientFactory**luoma.

```csharp

public class WcfCommunicationClient : ServicePartitionClient<WcfCommunicationClient<ICalculator>>
   {
       public WcfCommunicationClient(ICommunicationClientFactory<WcfCommunicationClient<ICalculator>> communicationClientFactory, Uri serviceUri, ServicePartitionKey partitionKey = null, TargetReplicaSelector targetReplicaSelector = TargetReplicaSelector.Default, string listenerName = null, OperationRetrySettings retrySettings = null)
           : base(communicationClientFactory, serviceUri, partitionKey, targetReplicaSelector, listenerName, retrySettings)
       {
       }
   }

```

Asiakkaan koodi käyttää **WcfCommunicationClientFactory** sekä **WcfCommunicationClient** joka toteuttaa **ServicePartitionClient** Palvelupäätepisteen ja viestintä-palvelussa.

```csharp
// Create binding
Binding binding = WcfUtility.CreateTcpClientBinding();
// Create a partition resolver
IServicePartitionResolver partitionResolver = ServicePartitionResolver.GetDefault();
// create a  WcfCommunicationClientFactory object.
var wcfClientFactory = new WcfCommunicationClientFactory<ICalculator>
    (clientBinding: binding, servicePartitionResolver: partitionResolver);

//
// Create a client for communicating with the ICalculator service that has been created with the
// Singleton partition scheme.
//
var calculatorServiceCommunicationClient =  new WcfCommunicationClient(
                wcfClientFactory,
                ServiceUri,
                ServicePartitionKey.Singleton);

//
// Call the service to perform the operation.
//
var result = calculatorServiceCommunicationClient.InvokeWithRetryAsync(
                client => client.Channel.Add(2, 3)).Result;

```
>[AZURE.NOTE] Oletusarvo ServicePartitionResolver oletetaan, että asiakkaan käyttöjärjestelmä on sama kuin palvelussa klusterin. Jos, joka on muussa tapauksessa ServicePartitionResolver objektin luominen ja välittää klusterin yhteyden päätepisteet.

## <a name="next-steps"></a>Seuraavat vaiheet
* [Etäproseduurikutsu luotettavia palveluja Etäpalvelujen](service-fabric-reliable-services-communication-remoting.md)

* [Verkko-Ohjelmointirajapinnan kanssa OWIN luotettava Services-palveluissa](service-fabric-reliable-services-communication-webapi.md)

* [Viestintä luotettava palveluiden suojaaminen](service-fabric-reliable-services-secure-communication.md)
