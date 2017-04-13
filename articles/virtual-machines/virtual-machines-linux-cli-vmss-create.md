<properties
    pageTitle="Mitä ovat AM asteikko määrittää? | Microsoft Azure"
    description="Lisätietoja AM asteikko joukot."
    keywords="Linux virtuaalikoneen virtuaalikoneen asteikko määrittää" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/24/2016"
    ms.author="gatneil"/>

# <a name="what-are-virtual-machine-scale-sets"></a>Mitä ovat virtuaalikoneen asteikko määrittää?

Virtuaalikoneen asteikko joukot avulla voit hallita useita VMs joukkona. Korkean tason asteikko joukkoon on seuraavia etuja sekä haittoja:

Ammattilaisille:

1. Suuren käytettävyyden. Skaalaa kukin ehtojoukko siirtää sen VMs käytettävyys määrittää 5 vika Domains (FDs) ja 5 päivityksen Domains (UDs), jos haluat varmistaa käytettävyys (Lisätietoja on FDs ja UDs, [AM käytettävyys](./virtual-machines-linux-manage-availability.md)). 
2. Azure kuormituksen ja sovelluksen yhdyskäytävän helposti integrointi.
3. Azure Automaattinen skaalaus helposti integrointi.
4. Yksinkertaisen käyttöönoton hallinta- ja VMs puhdistaminen.
5. Tue yleisten Windowsin ja Linux flavors sekä mukautetut kuvia.

Kuvakkeet:

1. Tietoja levyjen ei voi liittää asteikko joukon AM esiintymät. Sen sijaan on käytettävä Blob-objektien tallennustilaan, Azure tiedostoja, Azure-taulukoiden ja muiden tallennustilan ratkaisu.

## <a name="quick-create-using-azure-cli"></a>Nopea luominen käyttämällä Azure CLI

[AZURE.INCLUDE [cli-vmss-quick-create](../../includes/virtual-machines-linux-cli-vmss-quick-create-include.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Yleisiä tietoja Tutustu [asteikko joukkojen tärkeimmät aloitussivu](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

Lisää ohjeita Tutustu [asteikko tärkeimmät asiakirjat-sivu-vaihtoehdon avulla](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

Esimerkiksi Resurssienhallinta malleja käyttämällä asteikko joukot, Etsi [Azure pikaopas malleja github repo](https://github.com/Azure/azure-quickstart-templates)"vmss".

