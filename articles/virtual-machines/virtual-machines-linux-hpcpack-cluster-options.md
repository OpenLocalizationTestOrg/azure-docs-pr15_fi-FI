<properties
 pageTitle="Linux HPC Pack klusterin asetukset pilveen | Microsoft Azure"
 description="Lisätietoja asetuksista ja Microsoft HPC Pack voit luoda ja hallita Linux-tehokas tietojenkäsittely (HPC) klusterin Azure pilveen"
 services="virtual-machines-linux,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/26/2016"
 ms.author="danlep"/>

# <a name="options-with-hpc-pack-to-create-and-manage-an-hpc-cluster-in-azure-for-linux-workloads"></a>Voit luoda ja hallita HPC Pack HPC asetukset klusterin Azure Linux toiminnoista

[AZURE.INCLUDE [virtual-machines-common-hpcpack-cluster-options](../../includes/virtual-machines-common-hpcpack-cluster-options.md)]

Tässä artikkelissa kerrotaan asetukset käyttää HPC Pack Linux toiminnoista. Saatavilla on myös käytössä [Windows HPC työmääriä HPC Pack](virtual-machines-windows-hpcpack-cluster-options.md)-asetukset.

## <a name="run-an-hpc-pack-cluster-in-azure-vms"></a>Suorita HPC Pack-klusterin Azure VMs

### <a name="azure-templates"></a>Azure-mallit


* (Marketplace) [HPC Pack klusteri Linux työmääriä varten](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/)

* (Pikaopas) [Luo HPC-klusterin kanssa Linux Laske solmujen](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster-linux-cn)


### <a name="powershell-deployment-script"></a>PowerShellin käyttöönoton komentosarja

* [Luo Linux HPC-klusterin HPC Pack IaaS käyttöönoton komentosarja](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)

### <a name="tutorials"></a>Opetusohjelmat

* [Opetusohjelma: Käytön aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md)

* [Opetusohjelma: Suorita NAMD Microsoft HPC Pack Linux Laske solmujen Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Opetusohjelma: Suorita OpenFOAM Microsoft HPC Pack Linux RDMA klusterissa Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Opetusohjelma: Suorita TÄHTI-magnesiitin + Microsoft HPC Pack Linux RDMA-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster-starccm.md)

### <a name="cluster-management"></a>Klusterin hallinta

* [Lähettää työt HPC Pack-klusterin Azure-tietokannassa](virtual-machines-windows-hpcpack-cluster-submit-jobs.md)

* [HPC Pack töiden hallinta](https://technet.microsoft.com/library/jj899585.aspx)


## <a name="create-rdma-clusters-for-mpi-workloads"></a>Luo RDMA klustereiden MPI työmääriä varten

* [Opetusohjelma: Suorita OpenFOAM Microsoft HPC Pack Linux RDMA klusterissa Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)

* [Linux RDMA klusterin MPI sovelluksia määrittäminen](virtual-machines-linux-classic-rdma-cluster.md)

