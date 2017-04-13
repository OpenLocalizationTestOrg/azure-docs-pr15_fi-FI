<properties
    pageTitle="Yleistä Windows Näennäiskiintolevyn | Microsoft Azure"
    description="Opettele Sysprep generalize Windows-AM käytettäväksi resurssien hallinnan käyttöönottomalli."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Generalize Windows virtual machine-Sysprepin avulla

Tämän osan avulla voit generalize Windows virtuaalikoneen käyttö kuvana. Sysprep poistaa kaikki henkilökohtaiset tietosi, muun muassa ja valmistelee tietokone, jota käytetään kuvana. Lisätietoja Sysprep kohdassa [Voit käyttää Sysprep: Johdanto](http://technet.microsoft.com/library/bb457073.aspx).

Varmista, että siinä tietokoneessa palvelinrooleista tuetaan tekemät. Lisätietoja on artikkelissa [Palvelinrooleista Sysprep-tuki](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Jos käytät Sysprep ennen oman Näennäiskiintolevyn lataaminen Azure ensimmäistä kertaa, varmista, että sinulla on [valmisteltu oman AM](virtual-machines-windows-prepare-for-upload-vhd-image.md) ennen Sysprepin suorittamista. 

1. Kirjaudu sisään Windows virtuaalikoneen.

2. Avaa komentokehoteikkuna järjestelmänvalvojana. Vaihda kansio **%windir%\system32\sysprep**ja suorita sitten `sysprep.exe`.

3. Valitse **Järjestelmän** -valintaikkunan **Kirjoita ruutuun esittelyyn (Tervetuloa-ohjelma)**ja varmista, että **Yleistä** -valintaruutu on valittuna.

4. Valitse **Sammuta** **Sammuta**.

5. Valitse **OK**.

    ![Käynnistä Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Kun Sysprep on valmis, se suljetaan virtuaalikoneen. 

## <a name="next-steps"></a>Seuraavat vaiheet

- Jos AM on paikallisen, voit nyt [ladata Näennäiskiintolevyn, Azure](virtual-machines-windows-upload-image.md).
- Jos AM on jo Azure, voit nyt [luoda generalized AM kuva](virtual-machines-windows-capture-image.md).