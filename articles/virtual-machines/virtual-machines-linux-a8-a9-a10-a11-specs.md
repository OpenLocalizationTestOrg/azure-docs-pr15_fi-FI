<properties
 pageTitle="Tietoja Laske paljon VMs kanssa Linux | Microsoft Azure"
 description="Taustatietoja ja H-sarjan ja A8, A9, A10 ja A11 Laske paljon koot Huomioitavaa Linux VMs"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>Tietoja H-sarjan ja Laske paljon A sarjan VMs 

Seuraavassa on taustatietoja ja seikkoja uudempaan Azure H-sarjan ja aiempi A8, A9 tai A10 A11 koot, eli *Laske paljon* esiintymät. Tässä artikkelissa keskitytään käyttämällä näitä kokoja Linux VMs. Tässä artikkelissa on myös [Windows VMs](virtual-machines-windows-a8-a9-a10-a11-specs.md)käytettävissä.




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>RDMA verkon käyttäminen

Voit luoda RDMA-yhteyttä hyödyntäviin Linux VMs Suorita jompikumpi seuraavista tuettu Linux HPC jakelu ja tuettujen MPI käyttöönoton hyödyntää Azure RDMA verkon klustereiden. Katso asennusvaihtoehdot ja otoksen määritysvaiheet [määrittäminen Linux RDMA klusterin MPI sovelluksia](virtual-machines-linux-classic-rdma-cluster.md) .

* **Jaot** - VMs on otettava käyttöön RDMA-yhteyttä hyödyntäviin SUSE Linux Enterprise Server (SLES) tai OpenLogic CentOS perustuva HPC kuvista Azure Marketplacesta. Vain seuraavat Marketplace kuvat tukevat tarvittavat Linux RDMA ohjaimet:

    * SLES 12 SP1 käsittelevät SLES 12 SP1 käsittelevät (Premium)
    
    * SLES 12 käsittelevät SLES 12 käsittelevät (Premium)
    
    * CentOS perustuva 7.1 HPC
    
    * CentOS perustuva 6.5 HPC
    
    >[AZURE.NOTE]H-sarjan VMs Suosittelemme joko HPC kuva SLES 12 SP1 tai CentOS-pohjainen 7.1 HPC kuva.
    >
    >CentOS perustuva HPC-kuvista ydin päivitykset eivät ole käytettävissä **yum** määritystiedostoa. Tämä johtuu siitä Linux RDMA ohjaimet jaetaan JÄLLEENMYYNTIHINNAN-pakettina ja päivitysten ei ehkä toimi, jos ydin päivitetään.

* **MPI** - Intel MPI kirjaston 5.x

    Marketplace-kuva riippuen voit valita, erilliset käyttöoikeudet, asennus, tai Intel MPI määritykset voivat olla tarpeen, seuraavasti: 
    
    * **HPC kuva SLES 12 SP1** - asennuksen Intel MPI-pakettien jakaa AM suorittamalla seuraavan komennon:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **HPC kuva SLES 12** - sinun on rekisteröitävä erikseen ladattava ja asennettava Intel MPI. Saat [Intel MPI kirjaston oppaaseen](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **CentOS perustuva HPC kuvat** - Intel MPI 5.1 on jo asennettu.  

    Järjestelmäkokoonpano tarvitaan toimimaan liitetty VMs MPI työt. Esimerkiksi klusterissa VMs haluat vahvistaa luota Laske solmujen. Tyypillinen asetuksista on artikkeleissa [määrittäminen Linux RDMA klusterin MPI sovelluksia](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>HPC Pack sekä Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsoftin vapaa HPC klusterin ja työn hallintaratkaisu on yksi vaihtoehto, voit käyttää Laske paljon esiintymät Linux. Uusimmat julkaisut HPC Pack 2012 R2: n tuen useita Linux jaot voidaan suorittaa Laske solmujen Azure VMs, hallinnassa on käytössä Windows Server pään solmu otettu käyttöön. RDMA-yhteyttä hyödyntäviin Linux Laske solmujen käynnissä Intel MPI kanssa HPC Pack voit ajoittaminen ja pitäminen Linux MPI sovelluksia, jotka käyttävät RDMA verkkoon. Aloita-kohdasta [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Verkon topologian huomioon otettavia seikkoja

* Valitse RDMA käyttävä Linux VMs Azure-tietokannassa Eth1 on varattu RDMA verkkoliikennettä. Eth1 asetuksia tai verkon viittaaminen määritystiedostossa tiedot eivät muutu. Eth0 on varattu säännöllisesti Azure verkkoliikennettä.

* Azure-IP päälle InfiniBand (i) ei tueta. Tuetaan vain RDMA i päälle.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA päivitysten SLES 12

Kun olet luonut AM, SLES 12 HPC kuva perusteella, voit joutua päivittämään RDMA verkkoyhteydessä VMs RDMA-ohjaimet. 

>[AZURE.IMPORTANT]Tämä vaihe on **pakollinen** SLES 12: n HPC AM käyttöönotoissa Azure kaikilla alueilla. 
>Tämä vaihe ei ole tarvita, jos otat SLES 12-SP1 HPC, CentOS perustuva 7.1 HPC tai CentOS perustuva 6.5 HPC AM. 

Ennen kuin päivität ohjaimet, Lopeta **zypper** prosessien tai prosessit, Lukitse SUSE repo tietokantoja AM. Muussa tapauksessa ohjaimia ei voi päivittää oikein.  

Voit päivittää kunkin AM Linux RDMA ohjaimet suorittamalla seuraavat siirtämistä Azure CLI komentojen asiakastietokoneesta.

**SLES 12 HPC AM valmisteltu perinteinen käyttöönottomalli**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**SLES 12 HPC AM valmisteltu resurssien hallinnan käyttöönottomalli**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]Voi kestää jonkin aikaa, asenna ohjaimet ja komento palauttaa ilman tulos. Päivityksen jälkeen oman AM käynnistyy ja pitäisi olla valmis käytettäväksi muutaman minuutin.

### <a name="sample-script-for-driver-updates"></a>Esimerkki komentosarja ohjaimen päivitykset

Jos käytössäsi on klusterin SLES 12 HPC VMs, sinun komentosarjan ohjaimen päivitys yli kaikki solmut yhteyttä klusterin. Esimerkiksi seuraava komentosarja päivittää 8-solmu-klusterin ohjaimet.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Seuraavat vaiheet

* Katso tietoja käytettävyys ja Laske paljon kokoisille hinnat [näennäiskoneiden hinnoittelusta](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Tallennustilan valmiuksia ja levyn tiedot on artikkelissa [näennäiskoneiden koot](virtual-machines-linux-sizes.md).

* Aloita käyttöönotto ja Laske paljon kokojen käyttäminen RDMA Linux-kohdasta [määrittäminen Linux RDMA klusterin MPI sovelluksia](virtual-machines-linux-classic-rdma-cluster.md).


