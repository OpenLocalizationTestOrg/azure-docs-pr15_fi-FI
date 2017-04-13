<properties
   pageTitle="Ottaa käyttöön Windows HPC klusterin PowerShell-komentosarjaa | Microsoft Azure"
   description="Suorita Windows HPC Pack-klusterin Azuren näennäiskoneiden ottamaan PowerShell-komentosarja"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-windows-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Luo tehokas tietojenkäsittely (HPC) klusterin Windows HPC Pack IaaS käyttöönoton komentosarja

Suorita HPC Pack IaaS käyttöönoton PowerShell-komentosarjaa ottamaan valmis HPC klusteri Windows Azuren näennäiskoneiden työmääriä varten. Klusterin koostuu Active Directory liittyneet pään solmun käytössä Windows Server- tai Microsoft HPC Pack ja muita Windows Laske määrittämäsi resurssit. Jos haluat ottaa käyttöön HPC Pack klusteri Azure, Linux työmääriä varten, katso [luominen Linux HPC klusterin HPC Pack IaaS käyttöönoton komentosarjan](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Voit myös ottaa HPC Pack-klusterin Azure Resurssienhallinta-malli. Katso esimerkkejä [HPC-klusterin luominen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/) ja [Luo HPC-klusterin, jossa on mukautettu Laske solmu kuva](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-files"></a>Esimerkki tiedostojen

Seuraavissa esimerkeissä korvaa oman arvot tilauksen tunnus tai nimi ja tilin ja palvelun nimet.

### <a name="example-1"></a>Esimerkki 1

Seuraavat määritystiedosto otetaan käyttöön HPC Pack-klusteriin, joka on paikallinen tietokantoja pään solmu ja viisi Laske solmujen Windows Server 2012 R2-käyttöjärjestelmää. Kaikki pilvipalveluihin luodaan suoraan Länsi US sijainti. Toimialueen ohjauskoneen, toimialueen metsää toimii pään solmu.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2012R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### <a name="example-2"></a>Esimerkki 2

Seuraavat määritystiedosto otetaan käyttöön aiemmin toimialue-metsää HPC Pack klusterin. Klusterin on 1 paikalliset tietokannat pää-solmu ja 12 Laske solmujen käytetty BGInfo AM tunnisteella.
Windows-päivitysten automaattinen asennus ei ole käytettävissä kaikissa toimialueen metsää VMs. Kaikki pilvipalveluihin luodaan suoraan Itä-Aasian sijainti. Laske-solmut luodaan kolme pilvipalveluiden ja kolme tallennustilan tiliä: _MyHPCCN 0001_ , _MyHPCCN 0005_ _MyHPCCNService01_ ja _mycnstorage01_; _MyHPCCN 0006_ , _MyHPCCN0010_ _MyHPCCNService02_ ja _mycnstorage02_; ja _MyHPCCN 0011_ _MyHPCCN 0012_ _MyHPCCNService03_ ja _mycnstorage03_avulla). Laske-solmut luodaan olemassa olevan yksityisen kuvasta oteta talteen, Laske-solmun. Automaattinen suurennus ja pienennys-palvelu on käytössä oletustausta suurennus ja pienennys väliajoin.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
      </DomainController>
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### <a name="example-3"></a>Esimerkki 3

Seuraavat määritystiedosto otetaan käyttöön aiemmin toimialue-metsää HPC Pack klusterin. Klusterin on yksi pään solmu, yksi tietokantapalvelimen kanssa 500 gt tietoja DVD-levyllä, 2 broker solmujen Windows Server 2012 R2-käyttöjärjestelmää ja viisi Laske solmujen Windows Server 2012 R2-käyttöjärjestelmää. Pilvipalvelussa MyHPCCNService luodaan affiniteetti ryhmän *MyIBAffinityGroup*ja muut pilvipalveluihin luodaan affiniteetti ryhmän *MyAffinityGroup*. HPC työn ajoituksen REST API ja HPC web-portaalin otetaan käyttöön pään solmu.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```



### <a name="example-4"></a>Esimerkki 4

Seuraavat määritystiedosto otetaan käyttöön aiemmin toimialue-metsää HPC Pack klusterin. Klusterin on kaksi pää-solmu paikalliset tietokannat, kaksi Azure solmu mallit luodaan ja kolme kokoa Normaali Azure solmujen luodaan mallin Azure solmu _AzureTemplate1_. Komentosarjatiedosto suorittaa pään solmun, kun pään solmu on määritetty.

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```

## <a name="troubleshooting"></a>Vianmääritys


* **"VNet ei ole" Virhe** -, jos suoritat komentosarjan ottaa käyttöön useita klustereiden Azure-tietokannassa samanaikaisesti yksi tilaus-kohdassa vähintään yksi käyttöönotoissa voi epäonnistua virheen "VNet *VNet\_nimi* ei ole".
Jos tämä virhe ilmenee, Suorita komentosarja uudelleen epäonnistui käyttöönottoa varten.

* **Ongelma Azure virtual verkosta Internet-yhteyden muodostaminen** – Jos luot uuden toimialueen ohjauskoneen kanssa klusterin avulla käyttöönoton komentosarja tai voit manuaalisesti korottaminen pään AM toimialueen ohjauskoneen, saattaa ilmetä ongelmia VMs Internet-yhteyden. Tämä ongelma voi ilmetä, jos forwarder DNS-palvelin on automaattisesti määritetty toimialueen ohjauskoneen ja forwarder DNS-palvelin ei vastaa oikein.

    Voit kiertää tämän ongelman, Kirjaudu toimialueen ohjauskoneen ja joko Poista forwarder hakumäärityksen asetus tai määrittää kelvollinen forwarder DNS-palvelimeen. Jos haluat määrittää tämän asetuksen, palvelimen hallinnan valitsemalla **Työkalut** >
  avata DNS-hallintaan ja kaksoisnapsauta sitten **välittäjiä**  **DNS** .

* **Ongelma käyttäminen RDMA verkosta Laske paljon VMs** – Jos lisäät Windows Server Laske tai broker solmu VMs käyttämällä RDMA-yhteyttä hyödyntäviin koon, kuten A8 tai A9, saattaa ilmetä ongelmia näiden VMs RDMA sovelluksen verkkoon. Yksi tämän ongelman syy on, jos HpcVmDrivers-laajennus ei ole asennettu oikein, kun VMs lisätty klusterin. Esimerkiksi tiedostotunnisteen voi jäädä asennuksen tilassa.

    Voit kiertää tämän ongelman, tarkista ensin VMs laajennuksen tila. Jos laajennus ei ole asennettu oikein, projektistasi solmut HPC-klusterin ja lisää solmut uudelleen. Voit esimerkiksi lisätä Laske solmu VMs suorittamalla Lisää HpcIaaSNode.ps1 komentosarja pään solmun.
    
## <a name="next-steps"></a>Seuraavat vaiheet

* Suorita testi työmäärää klusterin. Esimerkki on artikkelissa HPC Pack [Aloitusopas](https://technet.microsoft.com/library/jj884144).

* Katso opetusohjelma, jotta klusterikäyttöönoton komentosarja ja suorita HPC työmäärää, [HPC Pack-klusterin Azure-tietokannassa, suorittamaan Excelin ja SOA työmääriä käytön aloittaminen](virtual-machines-windows-excel-cluster-hpcpack.md).

* Kokeile HPC Pack-työkaluja, joilla voi aloittaa, lopettaa, Lisää ja Poista Laske solmujen luot klusterin. Katso [hallinta Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md).

* Voit määrittää voit lähettää työt klusterin paikallisesta tietokoneesta, katso [Lähetä HPC työt paikallisen tietokoneesta HPC Pack-klusterin Azure-tietokannassa](virtual-machines-windows-hpcpack-cluster-submit-jobs.md).
