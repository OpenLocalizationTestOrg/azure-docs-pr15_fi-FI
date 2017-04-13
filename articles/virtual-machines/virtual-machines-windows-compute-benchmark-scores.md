<properties
 pageTitle="Laske ensisijainen tulosten for Windowsin VMs | Microsoft Azure"
 description="Windows Server Azure virtuaalilaitteiksi for SPECint Laske ensisijainen tulosten vertailu"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="cynthn"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/22/2016"
 ms.author="cynthn"/>

# <a name="compute-benchmark-scores-for-windows-vms"></a>Laske ensisijainen tulosten Windows VMs varten

SPECInt ensisijaisen-pisteet näyttää käytössä Windows Server Azure on tehokas AM allekkain suorittaminen. Laske ensisijainen tulosten ovat käytettävissä [Linux VMs](virtual-machines-linux-compute-benchmark-scores.md)myös.



## <a name="a-series---compute-intensive"></a>A-sarjat-paljon suorittaminen


Kokoa | vCPUs | NUMA-solmut | SUORITIN | Suorittaa | Avg perus korko | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_A8 | 8 | 1 | Intel Xeon suorittimen E5-2670 0 @ 2.6 GHz | 10 | 236.1 | 1.1
Standard_A9 | 16 | 2 | Intel Xeon suorittimen E5-2670 0 @ 2.6 GHz | 10 | 450.3 | 7.0
Standard_A10 | 8 | 1 | Intel Xeon suorittimen E5-2670 0 @ 2.6 GHz | 5 | 235.6 | 0,9
Standard_A11 | 16 | 2 | Intel Xeon suorittimen E5-2670 0 @ 2.6 GHz |7 | 454.7 | 4.8

## <a name="dv2-series"></a>Dv2 sarja


Kokoa | vCPUs | NUMA-solmut | SUORITIN | Suorittaa | Avg perus korko | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_D1_v2 | 1 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 83 | 36.6 | 2.6
Standard_D2_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 27 | 70.0 | 3,7
Standard_D3_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 19 | 130.5 | 4.4
Standard_D4_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 19 | 238.1 | 5.2
Standard_D5_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 14 | 460.9 | 15.4
Standard_D11_v2 | 2 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 19 | 70.1 | 3,7
Standard_D12_v2 | 4 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 2 | 132.0 | 1.4
Standard_D13_v2 | 8 | 1 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 17 | 235.8 | 3.8
Standard_D14_v2 | 16 | 2 | Intel Xeon E5 2673 v3 @ 2,4 GHz: N | 15 | 460.8 | 6.5


## <a name="g-series-gs-series"></a>G-sarjan, GS sarja


Kokoa | vCPUs | NUMA-solmut | SUORITIN | Suorittaa | Avg perus korko | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_G1, Standard_GS1  | 2 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 31 | 71.8 | 6.5
Standard_G2, Standard_GS2 | 4 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 5 | 133.4 | 13.0
Standard_G3, Standard_GS3 | 8 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 6 | 242.3 | 6.0
Standard_G4, Standard_GS4 | 16 | 1 | Intel Xeon E5-2698B v3 @ 2 GHz | 15 | 398.9 | 6.0
Standard_G5, Standard_GS5 | 32 | 2 | Intel Xeon E5-2698B v3 @ 2 GHz | 22 | 762.8 | 3,7

## <a name="h-series"></a>H-sarjan

Kokoa | vCPUs | NUMA-solmut | SUORITIN | Suorittaa | Iteraatioita/s | StdDev
------- | ------ | ---- | -------| ---- | ---- | -----
Standard_H8 | 8 | 1 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 5 | 297.4 | 0,9
Standard_H16 | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 5 | 575.8 | 6.8
Standard_H8m | 8 | 1 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 5 | 297.0 | 1.2
Standard_H16m | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 5 | 572.2 | 3.9
Standard_H16r | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 5 | 573.2 | 2.9
Standard_H16mr | 16 | 2 | Intel Xeon E5 2667 v3 @ 3.2 GHz | 7 | 569.6 | 2,8

## <a name="about-specint"></a>Tietoja SPECint

Windows-numerot on laskettu suorittamalla [SPECint 2006](https://www.spec.org/cpu2006/results/rint2006.html) Windows Server. SPECint suoritettiin perus korko-vaihtoehto (SPECint_rate2006) käyttäminen kappale core kohden. SPECint koostuu 12 erilliset testit suorittaa kolme kertaa, ottaen kunkin testin mediaanin ja painotus ne muodostamiseksi koosteen tulos. Näiden testi suoritettiin sitten useita VMs antamaan näkyy keskiarvo tulosten välillä.


## <a name="next-steps"></a>Seuraavat vaiheet

* Tallennustilan kapasiteettia, levyn tiedot ja muita huomioon otettavia seikkoja kesken AM koot valitsemisesta on artikkelissa [näennäiskoneiden koot](virtual-machines-windows-sizes.md).
