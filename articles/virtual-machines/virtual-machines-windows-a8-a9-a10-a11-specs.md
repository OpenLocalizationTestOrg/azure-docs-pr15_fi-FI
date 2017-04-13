<properties
 pageTitle="Tietoja Laske paljon VMs Windows | Microsoft Azure"
 description="Taustatietoja ja Azure H-sarjan ja A8, A9, A10 ja A11 Laske paljon koot Huomioitavaa Windows VMs ja cloud Services hankkiminen"
 services="virtual-machines-windows, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Tietoja H-sarjan ja Laske paljon A sarjan VMs

Seuraavassa on taustatietoja ja seikkoja uudempaan Azure H-sarjan ja aiempi A8, A9 tai A10 A11 esiintymät, eli *Laske paljon* esiintymät. Tässä artikkelissa keskitytään käyttämällä Windows VMs nämä esiintymät. Tässä artikkelissa on myös [Linux VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md)käytettävissä.


[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>RDMA verkon käyttäminen

Voit luoda klustereiden RDMA-yhteyttä hyödyntäviin Windows Server esiintymien ja ota käyttöön jokin tuettu MPI käyttöotot, voit hyödyntää Azure RDMA verkossa. Pieni viive, suuri siirtonopeuden verkoston on varattu ainoastaan MPI liikennettä.

* **Käyttöjärjestelmä**
    * **Näennäiskoneiden** - Windows Server 2012 R2, Windows Server 2012: ssa
    * **Cloud services** - Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 Vieras OS tuoteperhe

* **MPI** - Microsoft MPI (MS-MPI) 2012 R2: n tai sitä uudempi versio, Intel MPI kirjaston 5.x

Tuetut MPI käyttöotot väliseen esiintymät verkon suoraan käyttöliittymän avulla. Saat asennusvaihtoehdot ja otoksen määritysvaiheet [HPC Pack MPI-sovelluksia Windows RDMA klusterin määrittäminen](virtual-machines-windows-classic-hpcpack-rdma-cluster.md) ja [käyttäminen monen esiintymän tehtäviä viestin kulkeva Interface (MPI)-sovelluksia Azure osalta](../batch/batch-mpi.md) .


>[AZURE.NOTE]Valitse RDMA-yhteyttä hyödyntäviin Laske paljon VMs HpcVmDrivers-tunniste on lisättävä VMs asentaminen Windows verkon laiteohjaimet, joita tarvitaan RDMA yhteyksiä varten. Useimmissa käyttöönotoissa HpcVmDrivers-tunniste lisätään automaattisesti. Jos haluat lisätä tunnisteen, katso [hallinta AM tunnisteet](virtual-machines-windows-classic-manage-extensions.md).

## <a name="considerations-for-hpc-pack-and-windows"></a>HPC Pack sekä Windows

[Microsoft HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoftin vapaa HPC klusterin ja työn hallintaratkaisu, ei tarvita, voit käyttää Laske paljon esiintymät Windows Server. On kuitenkin yksi vaihtoehto, voit luoda laskentaklusteriin Azure Windows-pohjaisesta MPI sovellukset ja muut HPC toiminnoista. HPC Pack 2012 R2: n tai sitä uudempi versio sisältävät runtime-ympäristössä, joka käyttää Azure RDMA verkkoon, kun käyttöön RDMA-yhteyttä hyödyntäviin VMs MS-MPI.

Lisätietoja ja tarkistusluetteloiden Laske paljon esiintymät käytettäväksi HPC-paketti, Windows Server-kohdassa [määrittäminen Windows RDMA klusterin HPC Pack MPI sovelluksia](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).




## <a name="next-steps"></a>Seuraavat vaiheet

* Saat lisätietoja saatavuudesta ja Laske paljon kokoisille hinnoittelua [näennäiskoneiden hinnat](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows) ja [pilvipalveluihin hinnoittelua](https://azure.microsoft.com/pricing/details/cloud-services/).

* Tallennustilan valmiuksia ja levyn tiedot on artikkelissa [näennäiskoneiden koot](virtual-machines-linux-sizes.md).

* Aloita käyttöönotto ja Laske paljon esiintymät käyttäminen HPC Pack Windows-kohdasta [määrittäminen Windows RDMA klusterin HPC Pack MPI sovelluksia](virtual-machines-windows-classic-hpcpack-rdma-cluster.md).

* Tietoja käyttämällä A8 ja A9 esiintymät MPI sovellukset toimimaan Azure erä artikkelissa [Käytä usean esiintymän tehtäviä viestin kulkeva Interface (MPI)-sovelluksia Azure osalta](../batch/batch-mpi.md).
