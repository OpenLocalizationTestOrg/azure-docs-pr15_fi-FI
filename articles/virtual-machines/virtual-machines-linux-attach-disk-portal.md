<properties
    pageTitle="Liitä tiedot DVD-levyllä Linux AM | Microsoft Azure"
    description="Voit liittää tiedot uuteen tai aiemmin luotuun levyn Linux-AM Resurssienhallinta käyttöönoton mallin Azure-portaalissa."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="cynthn"/>

# <a name="how-to-attach-a-data-disk-to-a-linux-vm-in-the-azure-portal"></a>Tietoja DVD-levyllä liittämisestä Linux-AM Azure-portaalissa

Tässä artikkelissa kerrotaan sekä uusille ja nykyisille levyjen liittämisestä Linux virtual koneen palvelun Azure-portaalissa. Voit myös [liittää tietoja levyn Windows-AM Azure-portaalissa](virtual-machines-windows-attach-disk-portal.md). Ennen tämän tekemistä, tarkista seuraavia ohjeita:

- Virtuaalikoneen kokoa komponentteja näkyvien tietojen levyjä, voit liittää. Lisätietoja on artikkelissa [näennäiskoneiden koot](virtual-machines-linux-sizes.md).
- Käyttää Premium tallennustilaa, tarvitset DS-sarjan tai GS sarjan virtual machine. Voit käyttää näiden näennäiskoneiden levyjen Premium ja vakio tallennustilan tileistä. Premium tallennustila on tiettyjä alueita. Lisätietoja on artikkelissa [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](../storage/storage-premium-storage.md).
- Yhdistetty näennäiskoneiden levyjä ovat todella Azure-tallennustilan tilin .vhd-tiedostoja. Lisätietoja on artikkelissa [levyille ja näennäiskiintolevyjen näennäiskoneiden](virtual-machines-linux-about-disks-vhds.md).
- Uuden levyn ei tarvitse luoda ensin koska Azure luo sen, kun liität sen.
- Aiemmin luodun levyn .vhd-tiedosto on oltava käytettävissä Azure-tallennustilan tilin. Voit käyttää .vhd, joka on jo olemassa, jos se ei ole liitetty toiseen virtuaalikoneen tai lataa omia .vhd tiedostojen tallennustila-tilille.


[AZURE.INCLUDE [virtual-machines-common-attach-disk-portal](../../includes/virtual-machines-common-attach-disk-portal.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Kun levyn lisätään, sinun on valmistella käyttöä varten. Katso virtual machine-käyttöjärjestelmässä: "miten: alusta uudet tiedot-levy-Linux" tässä [artikkelissa](virtual-machines-linux-classic-attach-disk.md#how-to-initialize-a-new-data-disk-in-linux).
