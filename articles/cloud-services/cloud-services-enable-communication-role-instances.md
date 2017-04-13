<properties 
pageTitle="Viestintä pilvipalveluihin roolien | Microsoft Azure" 
description="Rooli esiintymät Cloud Services-palveluissa voi olla päätepisteet (http, https, tcp, udp) varten, joka ilmoittaa ulkopuolelle tai muiden roolin esiintymien välillä." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Tietoliikenne roolin esiintymien Azure-tietokannassa

Cloud palvelun roolit tiedonvälitys sisäisten ja ulkoisten yhteydet. Ulkoiset yhteydet kutsutaan **syötteen päätepisteet** , kun sisäinen yhteyksiä kutsutaan **sisäinen päätepisteet**. Tässä ohjeaiheessa kerrotaan, miten voit muokata luoda päätepisteet [palvelun määritys](cloud-services-model-and-package.md#csdef) .


## <a name="input-endpoint"></a>Kirjoita päätepisteen
Syötteen päätepisteen käytetään, kun haluat näyttää portin ulkopuolelle. Määritä protocol-tyypin ja portin päätepisteen, jotka lisäävät päätepisteen sekä sisäisiä ja ulkoisia portit. Jos haluat, voit määrittää eri sisäinen portti päätepisteen [Paikallisportti](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) -määrite.

Syötteen päätepisteen voit käyttää seuraavia protokollia: **http, https, tcp, udp**.

Luo syötteen päätepisteen lisäämällä **InputEndpoint** alielementti verkossa tai Työntekijä-roolin **päätepisteet** -osaan.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Esiintymän syötteen päätepiste
Esiintymän syötteen päätepisteet muistuttavat syötteen päätepisteet mutta avulla voit yhdistää tietyt julkisen portit yksittäisiä roolin jokaiselle esiintymälle käyttämällä portin edelleenlähetys kuormituksen. Voit määrittää yhden julkisen portin tai solualueen portit.

Esiintymän syötteen päätepisteen käyttää vain **tcp** tai **udp** -protokolla.

Luo esiintymän syötteen päätepisteen lisäämällä **InstanceInputEndpoint** alielementti verkossa tai Työntekijä-roolin **päätepisteet** -osaan.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Sisäinen päätepiste
Sisäinen päätepisteet ovat käytettävissä esiintymän esiintymän viestintään. Portti on valinnainen ja jos jätetään pois, dynaaminen portti määritetään päätepiste. Port ()-porttialueen avulla voidaan. Viisi sisäinen päätepisteet kohti roolin enimmäismäärä on.

Sisäinen päätepisteen voit käyttää seuraavia protokollia: **http, tcp, udp, minkä tahansa**.

Luo sisäinen syötteen päätepisteen lisäämällä **InternalEndpoint** alielementti verkossa tai Työntekijä-roolin **päätepisteet** -osaan.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Voit käyttää myös porttialue.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Työntekijän roolit ja Web roolit

On yksi vähäisiä ero kanssa päätepisteet käsittelyyn liittyvistä työntekijä ja web roolit. Web-rooli on oltava vähintään yhden syötteen loppupisteen **HTTP** -protokollan avulla.


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>.NET SDK käyttäminen päätepisteen
Azure hallitun kirjasto sisältää tapaa roolin esiintymien vaihtamaan suorituksen aikana. Koodin suorittamisen sisällä roolin esiintymän voit hakea tietoja siitä, onko rooli muut esiintymät ja niiden päätepisteet ja roolin nykyisen esiintymän tietoja.

> [AZURE.NOTE] Voit hakea vain roolin tapauksia, joissa oman pilvipalvelussa, mutta, joka määrittää vähintään yksi sisäinen päätepiste tietoja. Ei voi hankkia tietoja roolin esiintymiä eri palvelu käytössä.

[Esiintymät](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) -ominaisuuden avulla voit hakea rooli esiintymät. Palauttaa viittauksen roolin nykyisen esiintymän ensin [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) avulla ja palauttaa viittauksen roolin itse [rooli](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) -ominaisuuden avulla.

Kun muodostat yhteyden rooli-esiintymässä ohjelmallisesti .NET SDK kautta, on melko helppoa päätepisteen tietoja. Kun olet yhdistänyt jo rooli-ympäristössä, saat tämän koodin tietyn päätepisteen portti:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

**Esiintymissä** ominaisuus palauttaa joukko **RoleInstance** objekteja. Sivustokokoelman aina sisältää nykyisen esiintymän. Jos rooli ei sisäinen päätepisteen, eikä kokoelman sisältää nykyisen esiintymän, mutta ei muita esiintymiä. Sivustokokoelman roolin kerrat on aina 1 siinä tapauksessa, jossa sisäinen päätepistettä on määritetty oikeuksia. Jos rooli määrittää sisäinen päätepisteen, sen esiintymät on löydettävissä suorituksen ja kokoelman kerrat vastaavat määritettyä palvelun määritystiedostossa roolin kerrat.

> [AZURE.NOTE] Kirjaston Azure hallitun ei tarjoa tarkoittaa roolin muiden esiintymien terveyden määrittämiseksi, mutta voit ottaa käyttöön kunto näiden arvioiden itse Jos palvelussa on tämän toiminnon. Voit hankkia tietoja roolin kopioiden [Azure diagnostiikka](cloud-services-dotnet-diagnostics.md) .

Voit selvittää sisäinen päätepisteen rooli-esiintymässä porttinumero, [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) -ominaisuuden avulla voidaan palauttaa sanasto-objekti, joka sisältää päätepisteen nimet ja vastaavan IP-osoitteet ja portit. [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) -ominaisuus palauttaa IP-osoite ja portin määritetyn päätepisteelle. **PublicIPEndpoint** -ominaisuus palauttaa kuormitus tasataan päätepisteen portti. IP-osoitteen osa **PublicIPEndpoint** -ominaisuus ei ole käytössä.

Tässä on esimerkki, iteroivaa roolin esiintymät.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Tässä on esimerkki Työntekijä-roolin saa tarjoamia päätepisteen kautta service-määritys ja käynnistää Kuuntele yhteydet.

> [AZURE.WARNING] Tämä koodi toimii vain käyttöön palvelun. Kun emulaattorin Azure Laske-palvelun määritys elementtejä, jotka luominen suoraan portin päätepisteet (**InstanceInputEndpoint** elementit) ohitetaan.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Verkon liikenteen säännöt voit määrittää roolin viestintä
Kun olet määrittänyt sisäinen päätepisteet, voit lisätä verkon liikenteen säännöt (perusteella, jonka loit päätepisteet) kuinka roolin esiintymät viestiä keskenään ohjausobjektiin. Seuraavassa kaaviossa on esitetty havainnollistetaan Yleisiä tilanteita, ohjaukseen roolin viestintä:

![Verkon liikenteen säännöt-skenaariot] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Verkon liikenteen säännöt-skenaariot")

Seuraava koodi-esimerkissä edellisen kaavion roolit roolimäärityksiä. Kunkin roolimääritys sisältää vähintään yhden sisäinen päätepisteen määritetty:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Roolien välisen rajoittaminen voi ilmetä sisäinen päätepisteet sekä kiinteä määritetyt automaattisesti portit.

Oletusarvon mukaan jälkeen sisäinen päätepisteen on määritetty, viestintä voi flow minkä tahansa roolista roolin ilman mitään rajoituksia sisäinen päätepisteelle. Rajoittaa viestintä-palvelun määritystiedostoa **ServiceDefinition** elementti on lisättävä **NetworkTrafficRules** elementti.

### <a name="scenario-1"></a>Käyttötilanne 1
Salli vain verkkoliikennettä **WebRole1** **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Käyttötilanne 2
Sallii verkkoliikennettä vain **WebRole1** **WorkerRole1** ja **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Tapaus 3
Sallii verkkoliikennettä vain **WebRole1** **WorkerRole1**ja **WorkerRole1** **WorkerRole2**avulla.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Skenaario 4
Sallii verkkoliikennettä vain **WebRole1** **WorkerRole1**, **WebRole1** , **WorkerRole2**ja **WorkerRole1** **WorkerRole2**avulla.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

XML-rakenteen viittaus elementit, joita käytetään edellä löytyy [tähän](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja pilvipalvelussa [malli](cloud-services-model-and-package.md).