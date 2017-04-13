<properties
   pageTitle="Palvelun kangasta päätepisteiden määrittäminen | Microsoft Azure"
   description="Miten kuvaamaan päätepisteen resurssien service-luettelo, mukaan lukien HTTPS päätepisteet määrittäminen"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Määritä resurssien service-luettelo

## <a name="overview"></a>Yleiskatsaus

Service-luettelo sallii resursseja, joita käytetään-palvelu on määritetty ja muuttaa muuttamatta käännetty koodi. Azure palvelun kangasta tukee päätepisteen resurssien määrittäminen palvelulle. Resurssit, jotka on määritelty service-luettelon käyttöoikeuksia voidaan hallita sovelluksen luettelossa SecurityGroup kautta. Ilmoitus resurssien avulla muutetaan käyttöönoton aikana, eli palvelu ei tarvitse ottaa käyttöön uuden määritysten järjestelmä nämä resurssit. Rakenteen määritys ServiceManifest.xml tiedoston on asennettu palvelun kangasta SDK ja *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*-työkaluja.

## <a name="endpoints"></a>Päätepisteet

Kun päätepisteen yritysresurssi on määritetty service-luettelo, palvelun kangasta määrittää portit varatut sovelluksen Port (portti)-alueen portti ei ole määritetty erikseen. Tarkista esimerkiksi päätepisteen *ServiceEndpoint1* määritetyn luettelon koodikatkelman annettu tämän kappaleen jälkeen. Lisäksi services voi pyytää resurssin tietyn portin. Palvelun replikoiden eri klusterin käytössä voi määrittää eri porttinumerot samalla, kun palvelua saman solmun replikoita jakaminen portti. Palvelun replikat voit käyttää porttien tarvittaessa replikoinnin ja kuuntele asiakkaan pyyntöihin.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Viittaavat [määrittäminen tilallisen luotettavia palveluja](service-fabric-reliable-services-configuration.md) lukemaan lisätietoja viittaava päätepisteet config pakettiasetukset-tiedosto (settings.xml).

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Esimerkki: HTTP-päätepisteen palvelun määrittäminen

Seuraavat palvelun luettelo määrittelee TCP päätepisteen resurssin ja kaksi HTTP päätepisteen resurssit &lt;resurssien&gt; elementti.

HTTP-päätepisteet ovat automaattisesti Käyttöoikeusluettelon olisi palvelun kangasta mukaan.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Esimerkki: HTTPS-päätepisteen palvelun määrittäminen

HTTPS-protokollan tarjoaa palvelimen todennuksen ja käytetään myös asiakkaan ja palvelimen tietoliikenteen salaamiseen. HTTPS-palvelun kangasta-palvelun käyttöön Määritä protokollan *resurssit -> Päätepisteet -> päätepisteen* service-luettelo, kuten aiemmin päätepisteen *ServiceEndpoint3*-osassa.

>[AZURE.NOTE] Palvelun-protokollaa ei voi muuttaa sovelluksen päivityksen aikana, ilman, että se jakautumisen muodostavan muuta.


Tässä on esimerkki ApplicationManifest, jotka haluat määrittää HTTPS. Sertifikaatin allekirjoitus on annettava. EndpointRef on ServiceManifest, jolle määritetään HTTPS-protokollan EndpointResource viittaus. Voit lisätä useita EndpointCertificate.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
