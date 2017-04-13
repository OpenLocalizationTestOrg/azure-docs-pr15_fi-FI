<properties
    pageTitle="Windowsin näennäiskoneiden ohjeet | Microsoft Azure"
    description="Lisätietoja keskeisiä suunnittelu ja käyttöönotto ohjeita käyttöönoton näennäiskoneiden windows Azure tuominen"
    documentationCenter=""
    services="virtual-machines-windows"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="virtual-machines-guidelines"></a>Näennäiskoneiden ohjeet

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)] 

Tässä artikkelissa keskitytään tietoja tarvittavat suunnittelun vaiheet luomisen ja ylläpidon näennäiskoneiden (VMs) Azure-ympäristöön.

## <a name="implementation-guidelines-for-vms"></a>Toteutuksen jälkeisen VMs ohjeet
Päätökset:

- Kuinka monta VMs asetat eri sovelluksen tasoa ja infrastruktuurin osat?
- Suorittimen ja muistin resursseista kunkin AM on ja mitä tallennustilan vaatimuksia?

Tehtävät:

- Määritä sovellus ja VMs edellyttävät resurssien työmääriä.
- Tasaa resurssi vaatimukset varten tarvittavat AM koko ja tallennustilaa tyypin kunkin AM.
- Määritä resurssiryhmien eri tasojen ja infrastruktuurin osat.
- Määritä AM nimeämiskäytäntöä.
- Voit luoda oman VMs PowerShellin Azure-web-portaalin, tai Resurssienhallinta-mallien avulla.

## <a name="virtual-machines"></a>Näennäiskoneiden

Yksi pääosaa Azure-ympäristöön on todennäköisesti VMs. Tämä on, jossa suoritat sovellukset, tietokannoista, todennuspalvelujen, jne.

On tärkeää ymmärtää [AM erikokoisia](virtual-machines-windows-sizes.md) kokoa oikein ympäristösi suorituskyky ja kustannukset näkökulmasta. Jos yhteyttä VMs ei ole tarpeeksi suorittimen sydämiä tai muistia, riippumatta siitä, miten se on suunniteltu ja kehitetty kärsii sovelluksen suorituskykyä. Tarkista kunkin AM sarjan lähtökohtana ehdotettu työmääriä kuin päätät missä koossa AM käyttämään infrastruktuurin kullekin osalle. Voit [muuttaa kokoa AM](https://azure.microsoft.com/blog/resize-virtual-machines/) käyttöönoton jälkeen.

Tallennustilan toistetaan roolissa AM suorituskykyä. Voit käyttää vakio tallennustilaa, joka käyttää säännöllisesti pyörivä levyjä, tai Premium tallennustilan hyvin i/o-toiminnoista ja huippu suorituskykyä, joka käyttää Suoritettaessa levyjen. Kuin AM koon siellä on kustannusten tapoja, joilla voit valita tallennusväline. [Tallennustilan infrastruktuurin ohjeita artikkelissa](virtual-machines-windows-infrastructure-storage-solutions-guidelines.md) suunnitella tarvittavat tallennustilan lisääminen VMs parhaan suorituskyvyn, kuinka voit lukea.


## <a name="resource-groups"></a>Resurssiryhmät
Osia, kuten VMs ryhmitellään loogisesti yhteen helpottaa hallinta ja ylläpito [Azure resurssiryhmät](../azure-resource-manager/resource-group-overview.md). Resurssiryhmiä käyttämällä voit luoda, hallita ja seurata, jotka muodostavat sovelluksen resurssit. Voit myös ottaa käyttöön [Roolipohjainen käyttöoikeuksien](../active-directory/role-based-access-control-what-is.md) avulla myöntää käyttöoikeuksia muille ryhmän vain ne edellyttävät resursseja. Vie jonkin aikaa, resurssiryhmät ja roolimäärityksiä suunnitteleminen. On todella suunnitella ja toteuttaa resurssiryhmät, joten muista [resurssin ryhmät ohjeita artikkelissa](virtual-machines-windows-infrastructure-resource-groups-guidelines.md) selvittääksesi, miten parhaiten muodostetaan yhteyttä VMs eri tavalla.


## <a name="templates"></a>Mallit 
Voit luoda malleja, määritettäviä JSON-tiedostoja, voit luoda oman VMs määrittämiä. Mallien yleensä myös luoda pakollinen tallennustilan, verkko, verkkoliittymät, IP-osoitteiden jne sekä VMs itse. Mallien avulla voit luoda yhdenmukaisia, toistettavissa ympäristöissä kehittäminen ja testaus tarkoituksiin replikointia helposti tuotannon ympäristöissä ja päinvastoin. Voit lukea lisää [luomiseen ja mallien avulla](../azure-resource-manager/resource-group-overview.md#template-deployment) selvittääksesi, miten voit käyttää niitä luominen ja käyttöönotto oman VMs.


## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)] 