<properties
   pageTitle="PowerShell-komentosarjaa ottamaan Linux HPC klusterin | Microsoft Azure"
   description="Suorita PowerShell-komentosarjaa Linux HPC Pack-klusterin Azuren näennäiskoneiden käyttöönotto"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="big-compute"
   ms.date="07/07/2016"
   ms.author="danlep"/>

# <a name="create-a-linux-high-performance-computing-hpc-cluster-with-the-hpc-pack-iaas-deployment-script"></a>Luo Linux-tehokas tietojenkäsittely (HPC) klusteri ja HPC Pack IaaS käyttöönoton komentosarja

Suorita HPC Pack IaaS käyttöönoton PowerShell-komentosarjaa ottamaan valmis HPC klusteri Linux työmääriä Azuren näennäiskoneiden varten. Klusterin koostuu Active Directory liittyneet pää solmu käytössä Windows Server- tai Microsoft HPC Pack ja Laske-solmut, jotka suoritetaan jokin HPC Pack tukemat Linux jaot. Jos haluat ottaa käyttöön HPC Pack-klusterin-Azure Windows-toiminnoista, katso [luominen Windows HPC klusterin HPC Pack IaaS käyttöönoton komentosarja kanssa](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md). Voit käyttää myös Azure Resurssienhallinta-mallin käyttöön HPC Pack-klusterin. Esimerkki on kohdassa [Create HPC-klusterin kanssa Linux Laske solmujen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

[AZURE.INCLUDE [virtual-machines-common-classic-hpcpack-cluster-powershell-script](../../includes/virtual-machines-common-classic-hpcpack-cluster-powershell-script.md)]

## <a name="example-configuration-file"></a>Esimerkki kokoonpanotiedosto

Seuraavat kokoonpanotiedosto tunnistus HPC Pack-klusteriin on 1 paikalliset tietokannat pää-solmu ja 10 Linux Laske solmujen ja luo uusi toimialueen ohjauskoneen ja toimialueen metsää. Kaikki pilvipalveluihin luodaan suoraan Itä-Aasian sijainti. Linux Laske solmut luodaan 2 pilvipalveluihin ja 2 tallennustilan tilit (eli _MyLnxCN 0001_ , _MyLnxCN 0005_ _MyLnxCNService01_ ja _mylnxstorage01_) ja _MyLnxCN 0006_ _MyLnxCN 0010_ _MyLnxCNService02_ ja _mylnxstorage02_avulla. Laske-solmut luodaan OpenLogic CentOS v7.0 Linux kuva. 

Korvaa oman tilauksen nimi ja että tili ja palvelun nimet.

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
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>10</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```
## <a name="troubleshooting"></a>Vianmääritys

* **"VNet ei ole" Virhe** - Jos suoritat HPC Pack IaaS käyttöönoton komentosarja ottaa käyttöön useita klustereiden Azure-tietokannassa samanaikaisesti yksi tilaus-kohdassa vähintään yksi käyttöönotoissa voi epäonnistua virheen "VNet *VNet\_nimi* ei ole".
Jos tämä virhe ilmenee, uudelleen suorittamalla komentosarja epäonnistui käyttöönottoa varten.

* **Ongelma Azure virtual verkosta Internet-yhteyden muodostaminen** – Jos luot uuden toimialueen ohjauskoneen kanssa HPC Pack-klusterin avulla käyttöönoton komentosarja tai voit manuaalisesti korottaminen pään AM toimialueen ohjauskoneen, saattaa ilmetä ongelmia Internet-yhteyden VMs Azure virtual verkossa. Näin voi käydä, jos forwarder DNS-palvelin on automaattisesti määritetty toimialueen ohjauskoneen ja forwarder DNS-palvelin ei vastaa oikein.

    Voit kiertää tämän ongelman, Kirjaudu toimialueen ohjauskoneen ja joko Poista forwarder hakumäärityksen asetus tai määrittää kelvollinen forwarder DNS-palvelimeen. Voit tehdä tämän palvelimen hallinnan valitsemalla **Työkalut** >
  avata DNS-hallintaan ja kaksoisnapsauta sitten **välittäjiä**  **DNS** .
    
## <a name="next-steps"></a>Seuraavat vaiheet

* Katso [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md) tietoja tuetuista Linux jaot tietojen siirtäminen ja lähettää töitä HPC Pack-klusterin kanssa Linux Laske solmujen.
* Opetusohjelmia, jotka käyttävät komentosarja klusterin luominen ja suorittaminen Linux HPC työmäärää seuraavissa artikkeleissa:
    * [Suorita NAMD Microsoft HPC Pack Azure Linux Laske solmuissa](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
    * [Suorita OpenFOAM Microsoft HPC Pack Azure Linux Laske solmuissa](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)
    * [Suorita TÄHTI-magnesiitin + Microsoft HPC Pack Linux Laske solmujen Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)
