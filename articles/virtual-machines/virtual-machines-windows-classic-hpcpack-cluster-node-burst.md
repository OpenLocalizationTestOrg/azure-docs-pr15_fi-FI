<properties
 pageTitle="Lisää purskeen solmujen HPC Pack-klusterin | Microsoft Azure"
 description="Lue, miten voit laajentaa HPC Pack-klusterin-Azure tarvittaessa lisäämällä työntekijän rooli esiintymät käynnissä pilvipalvelussa"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Lisää tarvittaessa "purskeen" solmujen HPC Pack-klusterin Azure-tietokannassa



Jos määrität [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) -klusterin Azure-tietokannassa, haluat ehkä voi nopeasti skaalata klusterin kapasiteetti ylös tai alas säilyttämättä esimääritettyjä Laske solmu VMs joukko. Tässä artikkelissa näytetään, miten voit lisätä tarvittaessa "purskeen" solmujen (työntekijän rooli esiintymät käynnissä pilvipalvelussa) Laske resursseina pään solmu Azure-tietokannassa. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Purskeen solmujen][burst]

Tämän artikkelin ohjeiden avulla voit lisätä Azure solmujen nopeasti pilvipohjainen HPC Pack pään solmu AM testi tai käsitteiden, käyttöönotto. Ylätason vaiheet ovat samalla tavalla "kehittäessään Azure," Lisää cloud Laske paikallisen-HPC Pack klusterin resursseja. Katso opetusohjelma, [Määritä hybrid Laske klusterin Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Yksityiskohtaiset ohjeet ja tuotannon käyttöönottoa huomioon otettavia seikkoja on artikkelissa [Azure Microsoft HPC Pack Burst](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Edellytykset

* **Azure-AM otettu käyttöön HPC Pack pään solmu** – voit käyttää erillisen pään solmu AM tai sellainen, joka on suurempi klusterin osa. Erillinen pään solmu luomisesta on artikkelissa [Azure-AM HPC Pack pää-solmu käyttöönotto](virtual-machines-windows-hpcpack-cluster-headnode.md). Saat automaattisen HPC Pack klusterin Käyttöönottoasetukset [Voit luoda ja hallita Windows HPC Azure-tietokannassa ja Microsoft HPC Pack klusterin asetukset](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Jos käytät [HPC Pack IaaS käyttöönoton komentosarja](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) klusterin luominen Azure-Azure purskeen solmujen voi sisällyttää automaattinen käyttöönotto. Katso artikkelin esimerkkejä.

* **Azure tilauksen** – Lisää Azure solmujen, voit valita käyttöönotossa pään solmu AM, käytetty saman tilauksen tai eri tilauksen (tai tilauksia).

* **Sydämiä kiintiön** – voit joutua muuttamaan sydämiä, kiintiön Suurenna erityisesti, jos haluat ottaa käyttöön useita Azure solmujen multicore koot. Kun haluat lisätä kiintiön, [Avaa asiakkaan online-tukipyynnön](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) maksutta.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Vaihe 1: Luo pilvipalveluun ja tallennustilaa tilin Azure solmujen

Määritä on seuraavissa resursseissa, joita tarvitaan käyttöönotto Azure solmujen Azure perinteinen portal tai vastaavia Työkalut avulla:

* Uusi Azure pilvipalvelussa
* Uuden Azure-tallennustilan tilin

>[AZURE.NOTE] Aiemmin luodun pilvipalvelussa tilaukseesi ei uudelleen. 

**Huomioon otettavia seikkoja**

* Määritä kunkin Azure solmu-mallin, jota aiot luoda erilliset pilvipalvelussa. Voit kuitenkin käyttää saman tallennustilan tilin useita solmu malleja.

* On suositeltavaa, että löydät pilvipalvelussa ja käyttöönoton tallennustilan tiliin samassa Azure alueella.




## <a name="step-2-configure-an-azure-management-certificate"></a>Vaihe 2: Azure hallinta-varmenteen määrittäminen

Voit lisätä Azure solmujen Laske resursseina, tarvitset hallinta varmenteen pään solmu ja lataa vastaava varmenteen käyttöönoton käytettäviä Azure-tilaukseen.

Tässä skenaariossa voit **Oletus HPC Azure hallinta varmenne** , jota HPC Pack asentaa ja määrittää automaattisesti pään solmun. Tämä varmenne on hyötyä testikäyttöön tarkoituksiin ja käsitteiden, ominaisuuksissa. Jos haluat käyttää tätä todistusta, Lataa tiedosto C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer pään solmun AM tilaukseen. Voit ladata [Azure perinteinen portal](https://manage.windowsazure.com)varmenteen, valitsemalla **asetukset** > **Hallinta varmenteet**.

Katso lisäasetusten hallinta-varmenteen määrittäminen [tilanteita, joissa Azure Burst käyttöönotoissa Azure hallinta-varmenteen määrittäminen](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Vaihe 3: Käyttöönottaminen Azure solmujen klusterin



Voit lisätä ja Käynnistä Azure solmujen tässä skenaariossa vaiheet ovat yleensä samalla tavalla pään paikallisen-solmu kanssa. Lisätietoja on seuraavissa vaiheissa [käyttöönotto Azure solmujen Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx):

* Azure solmu mallin luominen

* Azure solmujen lisääminen Windows HPC klusterin

* Käynnistä (valmisteleminen) Azure solmujen

Kun olet lisännyt ja Käynnistä solmut, ne ovat voit käyttää klusterin työt.

Jos Azure solmujen käyttöönoton yhteydessä ilmenee ongelmia, katso [Vianmääritys käyttöönotoissa, Azure solmujen Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet

* Laske paljon esiintymän koon purskeen solmujen on artikkelissa esitettyjen [A-sarjan VMs H-sarjan ja Laske paljon tietoja](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Jos haluat automaattisesti Suurenna tai Pienennä Azure tietojenkäsittely resurssien mukaan klusterin työmäärää, katso [automaattisesti suurennus ja pienennys Azure Laske resurssien HPC Pack-klusterin](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
