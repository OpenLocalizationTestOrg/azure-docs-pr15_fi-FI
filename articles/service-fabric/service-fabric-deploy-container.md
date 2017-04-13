<properties
   pageTitle="Palvelun kangasta ja käyttöönottaminen säilöjen | Microsoft Azure"
   description="Palvelun kangasta ja käytä säilöjä microservice sovellusten. Tässä artikkelissa kuvataan ominaisuuksista, joita palvelun kangasta tarjoaa säilöt ja Windows-säilö kuva ottamisesta klusteriin"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Esikatselu: Käyttöönottaminen Windows säilön palvelun kangasta

> [AZURE.SELECTOR]
- [Windows-säilön käyttöönotto](service-fabric-deploy-container.md)
- [Ottaa käyttöön Docker säilö](service-fabric-deploy-container-linux.md)

Tässä artikkelissa käydään läpi luominen Windowsin säilöjen tai kontteihin pakattuja palvelut. 

>[AZURE.NOTE] Tämä ominaisuus on Linux ja ei ole tällä hetkellä käytettävissä Windows Server 2016 Preview'n. Tämä on käytettävissä Windows Server 2016 esikatselussa palvelun kangasta seuraavaa versiota ja tuettujen myöhemmin versiossa sen jälkeen.

Palvelun kangasta on useita säilö-ominaisuuksia, jotka auttavat sovellusten, jotka koostuvat, jotka ovat tai kontteihin pakattuja microservices. Näitä kutsutaan tai kontteihin pakattuja palvelut. 

Ominaisuuksia ovat;

- Säilön kuva käyttöönotto- ja aktivointi
- Resurssin hallinnon
- Tietovaraston todennus
- Säilön portin host portin määritys
- Säilön säilö etsiminen ja viestintä
- Mahdollisuus määrittää ja määrittää ympäristömuuttujat

Voit näyttää kussakin ominaisuuksia puolestaan, kun pakkaamista sisällytettävien sovellukseen tai kontteihin pakattuja palvelu.

## <a name="packaging-a-windows-container"></a>Pakkaamista Windows-säilö

Kun pakkaaminen säilön, voit joko käyttää Visual Studio projektin mallia tai [luoda sovelluspaketin manuaalisesti](#manually). Visual Studiossa, sovelluksen paketin rakenne ja tiedostojen tiedostot luodaan uusi projekti ohjatulla puolestasi (tämä on tulossa seuraavassa versiossa).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Aiemmin luodun säilö kuvan pakkaaminen Visual Studion avulla

>[AZURE.NOTE] Visual Studion tooling SDK tulevissa versioissa osaat säilön lisääminen sovelluksen samalla tavalla kuin, voi lisätä suoritettavan Vieras tänään. [Ota Vieras, joka on suoritettava palvelun kangasta](service-fabric-deploy-existing-app.md) aiheessa. Tällä hetkellä sinun on tehtävä manuaalinen pakkaus seuraavalla tavalla.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Pakkaamista manuaalisesti ja säilön käyttöönotto
Prosessi, jossa pakkaamista manuaalisesti tai kontteihin pakattuja palvelu perustuu seuraavasti:

1. Julkaista säilöjen lisääminen säilöön.
2. Luo paketti kansiorakenne.
3. Muokkaa luettelon palvelutiedosto.
4. Muokkaa sovelluksen tiedostojen tiedostoa.

## <a name="container-image-deployment-and-activation"></a>Säilön kuva käyttöönotto- ja aktivoinnin.
Palvelun kangasta [sovelluksen mallin](service-fabric-application-model.md)säilön edustaa sovelluksen-isäntä, jossa useita palvelun replikoita ovat käytettävissä. Ottaa käyttöön ja aktivoi säilön, sijoita säilö kuvan nimi `ContainerHost` osan service-luettelo.

Lisää palvelu-luettelo `ContainerHost` tapahtuman osoittamalla ja määritä `ImageName` säilö säilöön ja kuvan nimeä. Seuraavat osittainen luettelo näkyy esimerkki kutsutaan *myimage:v1* kutsutaan *myrepo* säilöstä säilön käyttöönotto

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Voit antaa komennot säilö kuva määrittämällä valinnainen `Commands` elementin pilkuilla erotettu komentojen suorittamiseen säilön sisällä. 

## <a name="resource-governance"></a>Resurssin hallinnon
Resurssi on säilön ominaisuus ja rajoittaa resurssit, joiden säilö käyttää isännän. `ResourceGovernancePolicy`, Sovellus-luettelossa määritettyä, työryhmät voivat määritellä resurssien rajoitukset koodin palvelupakettiin. Resurssien rajoitukset voidaan asettaa;

- Muisti
- MemorySwap
- CpuShares (suorittimen suhteellinen paino)
- MemoryReservationInMB  
- BlkioWeight (BlockIO suhteellinen paino). 

>[AZURE.NOTE] Uuteen versioon tuki, joka määrittää tietyn tekstialueen IO rajoitukset, kuten IOPs, luku-/ kirjoitusoikeudet-BITTIÄ ja muita on mahdollista.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Tietovaraston todennus
Voit ladata säilön on ehkä kirjautumisen tunnistetietoja säilö säilöön. Kirjautumistiedot *sovellusluettelo* -parametrissa käytetään määrittämään kirjautumistiedot tai SSH-näppäintä, ladata säilö kuva kuvan säilöstä.  Seuraavassa esimerkissä on tili nimeltä *TestUser* sekä salasana pelkkänä tekstinä. Tämä ei **ole** suositeltavaa.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Salasanan voi ja salata sertifikaatilla tietokoneen käyttöön.

Seuraavassa esimerkissä on tili nimeltä *TestUser* varmenne, jota kutsutaan *MyCert*salattu salasana. Voit käyttää `Invoke-ServiceFabricEncryptText` Powershell-komentoa, luomaan salasana salainen salaus teksti. Katso tämän artikkelin [tietoja palvelun kangasta sovellusten hallinta](service-fabric-application-secret-management.md) . Salasanan salauksen sertifikaatin yksityinen avain on otettava käyttöön paikallinen kone-nauha-menetelmän (Valitse Azure tämä on ARM kautta). Sitten, kun palvelun kangasta käyttää tietokoneen palvelupakettiin, on salauksen toiminta ja sekä sen tilinimen todentamismenetelmä säilö säilö nämä tunnistetiedoilla.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Säilön portin host portin määritys
Voit määrittää käytettävä yhteydessä säilö määrittämällä isäntä-portti `PortBinding` -sovellusluettelo. Sidonta porttiin yhdistää portti, johon palvelu Kuuntele porttiin isännän säilön sisällä.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Säilön säilö etsiminen ja viestintä
Käyttämällä `PortBinding` käytännön voit yhdistää säilö portin `Endpoint` palvelun luettelossa seuraavan esimerkin mukaisesti. Päätepisteen `Endpoint1` voit määrittää kiinteän portin, kuten portti 80 tai määrittää porttia ei ole lainkaan, jolloin satunnaisen portin varausyksiköiden sovelluksen portin alueelta valitaan puolestasi.

Varten Vieras säilöjen, joka määrittää `Endpoint` kuin tämän palvelun luettelossa avulla palvelun kangasta julkaista tämän päätepisteen automaattisesti Naming-palveluun, jotta muut klusterin palveluita voit tutustua tähän säilöön Ratkaise palvelun REST-kyselyjen käyttäminen. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Rekisteröidään Naming-palvelussa, voit tehdä säilö säilö viestintä koodissa helposti yhteyttä säilön [käänteisen välityspalvelimen](service-fabric-reverseproxy.md). Kaikki sinun on suoritettava on annettava käänteisen välityspalvelimen http kumpaan ja palvelut, jotka haluat pitää yhteyttä määrittämällä ne ympäristömuuttujat nimi. Katso seuraavaan osaan seuraavasti.  

## <a name="configure-and-set-environment-variables"></a>Määritä ja määritä ympäristömuuttujat
Ympäristömuuttujia voi olla määritettyä koodia pakkauksesta palvelussa luettelon sekä palveluiden käyttöön säilöjen tai prosessit/Vieras suoritettavat foe. Ympäristön muuttujan arvot voidaan ohittaa erityisesti sovelluksen luettelo tai määritetty sovelluksen parametreiksi käyttöönoton aikana.

Seuraavat palvelun luettelo XML koodikatkelman näyttää, miten voit määrittää ympäristömuuttujat koodi-paketti. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Ympäristömuuttujia voi ohittaa sovelluksen tiedostojen tasolla:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

Edellä olevassa esimerkissä on määrittänyt eksplisiittinen arvo `HttpGateway` ympäristömuuttuja (19000) samalla, kun arvo `BackendServiceName` parametrin arvoksi on asetettu kautta `[BackendSvc]` sovelluksen parametria. Tämän avulla voit määrittää arvo `BackendServiceName`arvo, kun sovelluksen käyttöönotto ja kiinteä arvo ei ole luettelossa. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Esimerkkejä sovelluksen ja palvelun luettelon loppuun
Seuraavassa on esimerkki-sovellusluettelo, joka osoittaa säilö-ominaisuuksiin.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Seuraavassa on esimerkki (määritetty edellisen sovellusluettelo) palvelun luettelo, jossa näkyy säilö-ominaisuudet

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
