<properties
    pageTitle="Luo ja lataa Linux Näennäiskiintolevyn | Microsoft Azure"
    description="Luo ja lataa Azure virtual kiintolevyn (Näennäiskiintolevyn), joka sisältää Linux-käyttöjärjestelmän perinteinen käyttöönotto-malli."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-the-linux-operating-system"></a>Luominen ja lataamisen Virtual kiintolevyn, joka sisältää Linux-käyttöjärjestelmä

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Voit myös [Lataa mukautetun levyn kuva Azure resurssien hallinnan avulla](virtual-machines-linux-upload-vhd.md).

Tässä artikkelissa kerrotaan, miten voit luoda ja ladata virtual kiintolevyn (Näennäiskiintolevyn), jotta voit käyttää sitä oman kuvana näennäiskoneiden luominen Azure. Lisätietoja käyttöjärjestelmän valmisteleminen siten, että sen avulla voit luoda useita näennäiskoneiden kyseiseen kuvaan perusteella. 

>  [AZURE.NOTE] Jos sinulla on hetkeksi, Auta meitä kehittämään Azure Linux AM ohjeista tehokkaasta tämän oman kokemukset [nopeasti kyselyn](https://aka.ms/linuxdocsurvey) . Jokaisen vastauksen auttaa meitä ohjeen saat tekemäsi työt.

## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että seuraavat kohteet:

- **Linux käyttöjärjestelmiä .vhd tiedostossa** - asennettuna [Azure vahvistettava Linux jakauma](virtual-machines-linux-endorsed-distros.md) (tai tarkastella [tietoja ei ole vahvistanut jaot](virtual-machines-linux-create-upload-generic.md)) virtual levylle Näennäiskiintolevyn-muodossa. Useita työkaluja AM ja Näennäiskiintolevyn luomiseen:
    - Asenna ja määritä [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) tai [KVM](http://www.linux-kvm.org/page/RunningKVM), varoen käyttämään Näennäiskiintolevyn kuva-muodossa. Tarvittaessa voit [muuntaa kuvan](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) käyttämällä `qemu-img convert`.
    - Voit myös Hyper-V [Windows 10: ssä](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) tai [Windows Server 2012/2012 R2-järjestelmästä](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Azure VHDX uudempaan tiedostomuotoon ei tueta. Kun luot AM, Määritä Näennäiskiintolevyn muotoon. Tarvittaessa voit muuntaa VHDX levyjen Näennäiskiintolevyn käyttämällä [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) tai [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet-komennon. Lisäksi Azure ei tue ladataan dynaaminen näennäiskiintolevyjen, sinun tarvitse näiden levyjen muuntaminen staattinen näennäiskiintolevyjen ennen lataamista. Voit muuntaa dynaamisiksi Azure lataamisen aikana työkaluilla, kuten [Azure Näennäiskiintolevyn apuohjelmia, siirry](https://github.com/Microsoft/azure-vhd-utils-for-go) .

- **Azure käyttöliittymä** - asenna uusin [Azure käyttöliittymä](../virtual-machines-command-line-tools.md) lataaminen Näennäiskiintolevyn.

<a id="prepimage"> </a>
## <a name="step-1-prepare-the-image-to-be-uploaded"></a>Vaihe 1: Valmisteleminen ladattava kuva

Azure tukee eri Linux jaot (Lisätietoja on [Vahvistettava jaot](virtual-machines-linux-endorsed-distros.md)). Seuraavissa artikkeleissa opastaa eri Linux jaot, joita tuetaan Azure valmistelemisesta. Kun suoritat seuraavat apuviivat ohjeita, tulevat takaisin tätä, kun Näennäiskiintolevytiedosto, joka on valmis, voit ladata Azure:

- **[CentOS perustuva jakelu](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Punainen on Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Muut --vahvistettava jakelu](virtual-machines-linux-create-upload-generic.md)**

> [AZURE.NOTE] Azure-ympäristö SLA koskee näennäiskoneiden Linux OS vain, kun jokin vahvistettu jaot käytetään tiedot 'Tuettu versioita' [Linux-Azure-Endorsed jaot](virtual-machines-linux-endorsed-distros.md)osalta. Linux jaot Azure kuva-valikoimassa on vahvistettu jaot tarvittavat määrityksissä.

Katso myös yleisiä vihjeitä Linux kuvia valmisteleminen Azure **[Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** .


<a id="connect"> </a>
## <a name="step-2-prepare-the-connection-to-azure"></a>Vaihe 2: Valmisteleminen Azure yhteys

Varmista, että käytät Azure-CLI perinteinen käyttöönottomalli (`azure config mode asm`), kirjaudu sisään tiliisi:

```
azure login
```


<a id="upload"> </a>
## <a name="step-3-upload-the-image-to-azure"></a>Vaihe 3: Lataa kuva Azure

Tarvitset tallennustilan tilin Näennäiskiintolevytiedosto ladataan. Voit valita olemassa olevan tallennustilan tilin tai [luoda uuden](../storage/storage-create-storage-account.md).

Azure-CLI avulla voit ladata kuvan käyttämällä seuraava komento:

```bash
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

Edellisessä esimerkissä:

- **BlobStorageURL** on aiot käyttää tallennustilan tilin URL-osoite
- **YourImagesFolder** on Blob-objektien tallennustilaan säilössä kohtaa, johon haluat tallentaa kuvat
- **VHDName** on otsikko, joka näkyy portaalissa tunnistavan virtual kiintolevylle.
- **PathToVHDFile** on koko polku ja käyttämääsi laitteeseen .vhd-tiedoston nimi.

Seuraavassa kuvassa näkyy valmiina Esimerkki:

```bash
azure vm image create UbuntuLTS `
    --blob-url https://teststorage.blob.core.windows.net/vhds/UbuntuLTS.vhd `
    --os Linux /home/ahmet/UbuntuLTS.vhd
```

## <a name="step-4-create-a-vm-from-the-image"></a>Vaihe 4: Luominen AM kuva
Voit luoda AM käyttämällä `azure vm create` samalla tavalla kuin tavallinen AM. Määritä annoit kuvan edellisessä vaiheessa. Seuraavassa esimerkissä Käytämme kuvan nimi **UbuntuLTS** edellisessä vaiheessa:

```bash
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "DeployedUbuntu" UbuntuLTS
```

Jos haluat luoda oman VMs, määritettävä oman käyttäjänimi + salasanan, sijainti, DNS-nimi ja kuva nimi.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on artikkelissa [Azure CLI viittauksen Azure perinteinen käyttöönotto-malliin](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload
