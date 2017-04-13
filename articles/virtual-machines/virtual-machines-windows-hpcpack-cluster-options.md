<properties
 pageTitle="Windows HPC Pack klusterin asetukset pilveen | Microsoft Azure"
 description="Lisätietoja asetuksista ja Microsoft HPC Pack voit luoda ja hallita Windows-tehokas tietojenkäsittely (HPC) klusterin Azure pilveen"
 services="virtual-machines-windows,cloud-services,batch"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-a-windows-hpc-cluster-in-azure"></a>Voit luoda ja hallita Windows HPC Azure-klusterin Pack HPC asetukset

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Tässä artikkelissa keskitytään voi luoda HPC Pack klustereiden Suorita Windows toiminnoista. Saatavilla on myös suorittaa [Linux HPC työmääriä HPC Pack](virtual-machines-linux-hpcpack-cluster-options.md)luomisen klustereiden asetukset.


## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Suorita HPC Pack-klusterin Azure VMs

### <a name="azure-templates"></a>Azure-mallit

* (Marketplace) [HPC Pack klusteri Windows työmääriä varten](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterwindowscn/)

* (Marketplace) [HPC Pack klusteri Excel työmääriä varten](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterexcelcn/)

* (Pikaopas) [HPC-klusterin luominen](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster)

* (Pikaopas) [Luo HPC-klusterin ja mukautetun suorittaminen solmu kuva](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-custom-image)

### <a name="azure-vm-images"></a>Azure AM kuvat

* [Windows Server 2012 R2 HPC Pack](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [Windows Server 2012 R2 HPC Pack Laske solmun](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack Laske Excelin Windows Server 2012 R2-solmu](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)



### <a name="powershell-deployment-script"></a>PowerShellin käyttöönoton komentosarja

* [Luo HPC-klusterin HPC Pack IaaS käyttöönoton komentosarja](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Opetusohjelmat

* [Opetusohjelma: HPC Pack-klusterin Azure-tietokannassa, suorittamaan Excelin ja SOA työmääriä käytön aloittaminen](virtual-machines-windows-excel-cluster-hpcpack.md)



### <a name="manual-deployment-with-the-azure-portal"></a>Manuaalisen käyttöönoton Azure-portaalissa

* [Azure-AM HPC Pack klusterin pään solmu määrittäminen](virtual-machines-windows-hpcpack-cluster-headnode.md)

### <a name="cluster-management"></a>Klusterin hallinta

* [Laske solmujen HPC Pack-klusterin Azure-tietokannassa hallinta](virtual-machines-windows-classic-hpcpack-cluster-node-manage.md)

* [Suurennus ja pienennys HPC Pack-klusterin Azure Laske-resurssit](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md)

* [Lähettää työt HPC Pack-klusterin Azure-tietokannassa](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [HPC Pack töiden hallinta](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="add-worker-role-nodes-to-an-hpc-pack-cluster"></a>Työntekijän roolin solmujen lisääminen HPC Pack-klusterin


* [Azure työntekijä esiintymät ja HPC Pack Burst](https://technet.microsoft.com/library/gg481749.aspx)

* [Opetusohjelma: Määrittäminen hybrid klusterin HPC Pack Azure-tietokannassa](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Azure-purskeen"solmujen lisääminen HPC Pack Azure pään solmu](virtual-machines-windows-classic-hpcpack-cluster-node-burst.md)


## <a name="integrate-with-azure-batch"></a>Integroi Azure erä 

* [Azure erä HPC Pack Burst](https://technet.microsoft.com/library/mt612877.aspx)

## <a name="create-rdma-clusters-for-mpi-workloads"></a>Luo RDMA klustereiden MPI työmääriä varten

* [Windowsin RDMA klusterin HPC Pack MPI sovelluksia määrittäminen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md)
