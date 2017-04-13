<properties
    pageTitle="Luo ja lataa AM kuva PowerShellin avulla | Microsoft Azure"
    description="Opettele luomaan ja lataa generalized Windows Server kuva (Näennäiskiintolevyn) perinteinen käyttöönoton mallin ja PowerShellin Azure avulla."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/21/2016"
    ms.author="cynthn"/>

# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Luo ja lataa Windows Server Näennäiskiintolevyn Azure

Tässä artikkelissa kerrotaan lataaminen generalized AM-kuva virtual kiintolevyn (Näennäiskiintolevyn), jotta voit käyttää sitä Luo näennäiskoneiden. Saat lisätietoja levyjen ja Microsoft Azure näennäiskiintolevyjen [tietoja levyjen ja näennäiskiintolevyjen näennäiskoneiden](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Voit myös [ladata](virtual-machines-windows-upload-image.md) virtual kone Resurssienhallinta-mallin. 

## <a name="prerequisites"></a>Edellytykset

Tässä artikkelissa oletetaan, että:

- **Azure tilauksen** – Jos sinulla ei ole yhtä voit [avata Azure-tili maksutta](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** - sinulla on Microsoft Azure PowerShell-moduulin asennettu ja määritetty käyttämään tilauksen. 

- **A . Näennäiskiintolevytiedosto** - tuettujen Windows-käyttöjärjestelmän .vhd-tiedosto tallennetaan ja virtual machine liitetään. Tarkista, jos käynnissä ylläpito palvelinrooleista tuetaan tekemät. Lisätietoja on artikkelissa [Palvelinrooleista Sysprep-tuki](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

> [AZURE.IMPORTANT] Microsoft Azure ei tueta VHDX-muodossa. Voit muuntaa levyn Näennäiskiintolevyn muotoon Hyper-V Manager tai [Muunna Näennäiskiintolevyn cmdlet-komennon](http://technet.microsoft.com/library/hh848454.aspx)avulla. Lisätietoja on artikkelissa tämän [blogpost](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Vaihe 1: Valmistelu Näennäiskiintolevyn 

Näennäiskiintolevyn lataaminen Azure, se on generalized Sysprep-työkalun avulla. Tämä valmistelee Näennäiskiintolevyn käytettävän kuvana. Lisätietoja Sysprep kohdassa [Voit käyttää Sysprep: Johdanto](http://technet.microsoft.com/library/bb457073.aspx). Varmuuskopioi AM ennen Sysprepin suorittamista.

Virtual tietokoneesta, johon käyttöjärjestelmä on asennettu toimimalla seuraavasti:

1. Kirjaudu käyttöjärjestelmä.

2. Avaa komentokehoteikkuna järjestelmänvalvojana. Vaihda kansio **%windir%\system32\sysprep**ja suorita sitten `sysprep.exe`.

    ![Avaa komentorivi-ikkuna](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.  **Järjestelmän** -valintaikkuna.

    ![Käynnistä Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  **Järjestelmän** **Anna järjestelmän poissa ruutuun (Experience)** ja varmista, että **Yleistä** on valittuna.

5.  Valitse **Sammuta** **Sammuta**.

6.  Valitse **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Vaihe 2: Luo tallennustilan tilin ja säilö

Tarvitset Azure-tallennustilan tilin, eli tarjolla on nyt ladata .vhd-tiedoston sijainti. Tämän vaiheen avulla voit luoda tili, tai pyydä tiedot, sinun on aiemmin luotu tili. Korvaa muuttujien &lsaquo; hakasulkeita &rsaquo; omilla tiedoillasi.

1. Kirjautuminen

        Add-AzureAccount

1. Määritä Azure tilauksen.

        Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Luo uusi tallennustilan-tili. Tallennustilan tilin nimi on oltava yksilöivä otsikko, 3 – 24 merkkiä. Nimi voi olla kirjaimia ja numeroita. Sinun on myös määritettävä sijainti, kuten "Itä US"
        
        New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Määritä uusi tallennustilan tilin oletusprofiiliksi.
        
        Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Luo uusi säilö.

        New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## <a name="step-3-upload-the-vhd-file"></a>Vaihe 3: Lataa .vhd-tiedosto

[Lisää AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) avulla voit ladata Näennäiskiintolevyn.

Käytetään edellisessä vaiheessa PowerShellin Azure-ikkunasta, kirjoita seuraava komento ja korvaa muuttujien &lsaquo; hakasulkeita &rsaquo; omilla tiedoillasi.

        Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Vaihe 4: Lisää kuva mukautetun kuvien luetteloon

[Lisää AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) cmdlet-komennon avulla voit lisätä kuvan mukautetun kuvien luetteloon.

        Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt [luoda mukautettuja AM](virtual-machines-windows-classic-createportal.md) käyttäminen ladattujen kuva.

