<properties
    pageTitle="Luo AM perinteinen portaalissa | Microsoft Azure"
    description="Luo Windows-virtual machine Azure perinteinen-portaalissa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="cynthn"/>

# <a name="create-a-virtual-machine-running-windows-in-the-azure-classic-portal"></a>Windows Azure perinteinen portaalissa on virtual koneen luominen

> [AZURE.SELECTOR]
- [Azure perinteinen portal](virtual-machines-windows-classic-tutorial.md)
- [PowerShellin: Perinteinen käyttöönotto](virtual-machines-windows-classic-create-powershell.md)

<br>

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Lue [Resurssienhallinta käyttöönoton mallin seuraavasti](virtual-machines-windows-hero-tutorial.md) **uudessa Azure-portaalissa**. 

Tässä opetusohjelmassa näytetään luomisesta Azure virtual machine (AM) Windows Azure perinteinen portaalissa on. Käytämme esimerkkinä Windows Server-kuva, mutta joka on vain yksi Azure tarjoaa useita kuvia. Huomaa, että kuva käytettävissä olevat vaihtoehdot määräytyvät tilauksen. Esimerkiksi Windows desktop-kuvat ole käytettävissä MSDN-tilaajille.

Tämän osan avulla voit luoda virtuaalikoneen Azure perinteinen portal **-Valikoima** -vaihtoehdon avulla. Tämä vaihtoehto sisältää enemmän kuin **Nopea luominen** -vaihtoehto määritysten valinnat. Esimerkiksi jos haluat liittyä virtual machine virtual verkkoon, sinun on **-Valikoima** -vaihtoehtoa.

Voit myös luoda [omia kuvia](virtual-machines-windows-classic-createupload-vhd.md)käyttämällä VMs. Lisätietoja tämän ja muiden kautta on artikkelissa [eri tapoja luoda Windows-virtual machine](virtual-machines-windows-creation-choices.md).



## <a name="video-walkthrough"></a>Videon vaiheittainen kuvaus

Seuraavassa on tässä opetusohjelmassa vaiheista.

[AZURE.VIDEO creating-a-windows-vm-on-microsoft-azure-classic-portal]

## <a id="createvirtualmachine"> </a>Virtuaalikoneen luominen

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja uuden Azure-portaalissa [luominen AM, käyttämällä resurssien hallinnan käyttöönottomalli](virtual-machines-windows-hero-tutorial.md) . 

- Kirjaudu virtuaalikoneen. Katso ohjeet [käytössä Windows Server virtual koneen kirjautua](virtual-machines-windows-classic-connect-logon.md).

- Liitä DVD-levyllä tietojen tallentamiseen. Voit liittää tyhjä levyille ja levyjä, jotka sisältävät tietoja. Katso ohjeet [Liitä Windows virtual machine-perinteinen käyttöönotto-mallin avulla luotu tietojen levylle](virtual-machines-windows-classic-attach-disk.md).
