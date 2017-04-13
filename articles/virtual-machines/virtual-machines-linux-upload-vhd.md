<properties
    pageTitle="Luo ja lataa mukautetun Linux kuvan | Microsoft Azure"
    description="Luo ja lataa virtual kiintolevyn (Näennäiskiintolevyn) Azure mukautetun Linux kuvan käyttämällä resurssien hallinnan käyttöönottomalli."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

# <a name="upload-and-create-a-linux-vm-from-custom-disk-image"></a>Lataa ja Linux AM luominen mukautetun levyn kuva

Tässä artikkelissa kerrotaan, miten virtual kiintolevyn (Näennäiskiintolevyn) lataaminen Azure Resurssienhallinta käyttöönoton mallin ja luoda Linux VMs tämän mukautetun kuvan. Tämän toiminnon avulla voit asentaa ja määrittää Linux-distro tarpeisiisi ja luoda nopeasti Azure-virtuaalikoneissa (VMs) kyseisen Näennäiskiintolevyn avulla.

## <a name="quick-commands"></a>Pikakomennot
Jos haluat tehdä nopeasti tehtävän osan seuraavat asiat kantaluku AM lataaminen Azure komennot. Tarkempia tietoja ja kontekstia kunkin vaiheen löytyy loput [käynnistäminen tässä](#requirements)asiakirjassa.

Varmista, että sinulla on [Azure-CLI](../xplat-cli-install.md) kirjautuneena palveluun ja Resurssienhallinta-tilassa:

```bash
azure config mode arm
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ohjelmistopakettia `myResourceGroup`, `mystorageaccount`, ja `myimages`.

Luo resurssiryhmä. Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `WestUs` sijainti:

```bash
azure group create myResourceGroup --location "WestUS"
```

Tallennustilan tilin pitoon virtual levyjen luominen. Seuraavassa esimerkissä luodaan tallennustilan-tili nimeltä `mystorageaccount`:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Luettele tallennustilan tilin pikanäppäimet. Pane merkille `key1`:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Voit luoda säilön tallennustilan-tilisi on saatu tallennustilan avaimen avulla. Seuraavassa esimerkissä luodaan nimeltä säilön `myimages` käyttämällä tallennustilan avaimen `key1`:

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Lataa-että Näennäiskiintolevyn luomasi säilö. Määrittää paikallisen polun oman Näennäiskiintolevyn `/path/to/disk/mydisk.vhd`:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Voit nyt luoda AM oman ladatut virtual levyn [Resurssienhallinta mallin avulla](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Voit käyttää CLI määrittämällä levylle URI (`--image-urn`). Seuraavassa esimerkissä luodaan AM, jonka nimi on `myVM` käyttämällä virtual levyä äsken:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Kohde-tallennustilan tilin on oltava sama kuin kohtaa, johon olet ladannut virtual levylle. Sinun on myös määritettävä tai vastauksen kysyy, kaikki muut parametrit vaatii `azure vm create` komento, kuten VPN, julkiseen IP-osoite, käyttäjänimi ja SSH avaimet. Voit lukea lisää [käytettäviin CLI Resurssienhallinta](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Vaatimukset
Suorita seuraavat vaiheet sinun on:

- **Linux-käyttöjärjestelmä asentaa .vhd-tiedosto** - Asenna [Azure vahvistettava Linux jakauman](virtual-machines-linux-endorsed-distros.md) (tai tarkastella [tietoja ei ole vahvistanut jaot](virtual-machines-linux-create-upload-generic.md)) virtual levylle Näennäiskiintolevyn-muodossa. Useita työkaluja AM ja Näennäiskiintolevyn luomiseen:
    - Asenna ja määritä [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) tai [KVM](http://www.linux-kvm.org/page/RunningKVM), varoen käyttämään Näennäiskiintolevyn kuva-muodossa. Tarvittaessa voit [muuntaa kuvan](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) käyttämällä `qemu-img convert`.
    - Voit myös Hyper-V [Windows 10: ssä](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) tai [Windows Server 2012/2012 R2-järjestelmästä](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Azure VHDX uudempaan tiedostomuotoon ei tueta. Kun luot AM, Määritä Näennäiskiintolevyn muotoon. Tarvittaessa voit muuntaa VHDX levyjen Näennäiskiintolevyn käyttämällä [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) tai [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet-komennon. Lisäksi Azure ei tue ladataan dynaaminen näennäiskiintolevyjen, sinun tarvitse näiden levyjen muuntaminen staattinen näennäiskiintolevyjen ennen lataamista. Voit muuntaa dynaamisiksi Azure lataamisen aikana työkaluilla, kuten [Azure Näennäiskiintolevyn apuohjelmia, siirry](https://github.com/Microsoft/azure-vhd-utils-for-go) .

- Mukautetun kuvan luotu VMs on sijaittava saman tallennustilan tilin itse kuvana
    - Tallennustilan tilin ja säilön ja pidä mukautetun kuvan ja luotu VMs luominen
    - Kun olet luonut kaikki VMs, voit poistaa kuvan turvallisesti

Varmista, että sinulla on [Azure-CLI](../xplat-cli-install.md) kirjautuneena palveluun ja Resurssienhallinta-tilassa:

```bash
azure config mode arm
```

Seuraavissa esimerkeissä korvaa Esimerkki parametrinimet oman arvoilla. Esimerkki parametrinimet ohjelmistopakettia `myResourceGroup`, `mystorageaccount`, ja `myimages`.


<a id="prepimage"> </a>
## <a name="prepare-the-image-to-be-uploaded"></a>Valmistele ladattava kuva

Azure tukee eri Linux jaot (Lisätietoja on [Vahvistettava jaot](virtual-machines-linux-endorsed-distros.md)). Seuraavissa artikkeleissa opastaa valmistelemisesta eri Linux jaot, joita tuetaan Azure:

- **[CentOS perustuva jakelu](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Punainen on Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES & openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Muut --vahvistettava jakelu](virtual-machines-linux-create-upload-generic.md)**

Katso myös yleisiä vihjeitä Linux kuvia valmisteleminen Azure **[Linux asennusohjeet](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** .

> [AZURE.NOTE] [Azure-ympäristö SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) koskee virtuaalilaitteiksi Linux, vain, kun jokin vahvistettu jaot käytetään tiedot 'Tuettu versioita' [Linux-Azure-Endorsed jaot](virtual-machines-linux-endorsed-distros.md)osalta.


## <a name="create-a-resource-group"></a>Resurssiryhmän luominen
Resurssiryhmät liittävät toisiinsa loogisesti kaikki näennäiskoneiden, kuten virtual verkko- ja tallennustilaa tukemaan Azure resurssit. Lisätietoja on artikkelissa [Azure resurssiryhmät tähän](../azure-resource-manager/resource-group-overview.md). Ennen mukautetun levyn näköistiedoston lataus ja luot VMs, sinun on luoda resurssiryhmän. 

Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `WestUS` sijainti:

```bash
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen
VMs tallennetaan sivun BLOB storage-tilin. Lisätietoja on artikkelissa [Azure-blob-säiliö tähän](../storage/storage-introduction.md#blob-storage). Voit luoda mukautetun levyn näköistiedoston ja VMs tallennustilan tilin. Mikä tahansa VMs, voit luoda mukautetun levyn näköistiedoston on oltava saman tallennustilan tilin, että kuva.

Seuraavassa esimerkissä luodaan tallennustilan-tili nimeltä `mystorageaccount` resurssi-ryhmässä aiemmin luotu:

```bash
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Luettelon tallennustilan tilin näppäimet
Azure Luo kaksi 512-bittinen-pikanäppäinten tallennustilan jokaiselle tilille. Näitä pikanäppäimiä käytetään todennuksessa tallennustilan-tilille, kuten kirjoitustoimintojen suorittamiseen. Lue lisätietoja [tallennustilan tähän käyttöoikeuksien hallinta](../storage/storage-create-storage-account.md#manage-your-storage-account). Voit tarkastella pikanäppäimet, jossa `azure storage account keys list` komento.

Näytä tallennustilan-tili, jonka loit pikanäppäimet:

```bash
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Tulos on:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Pane merkille `key1` , kun sitä käytetään käsitellä tallennustilan-tilisi seuraaviin vaiheisiin.

## <a name="create-a-storage-container"></a>Luo tallennustilan säilö
Samalla tavalla, voit luoda eri kansioiden järjestäminen loogisesti paikallisen tiedostojärjestelmän voit luoda säilöjen tallennustilan tilin, voit järjestää virtual levyille ja -kuvat. Tallennustilan tilin voi olla jokin muu luku säilöjä. 

Seuraavassa esimerkissä luodaan nimeltä säilön `myimages`, joka määrittää pikanäppäimen saatu edellisessä vaiheessa (`key1`):

```bash
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Lataa Näennäiskiintolevyn
Voit nyt itse asiassa ladata mukautetun levyn näköistiedoston. Kaikkien virtual levyjen käyttämä VMs, jossa voit ladata ja tallentaa mukautetun levyn näköistiedoston sivun Blob-objektien.

Määritä access-näppäintä, loit edellisessä vaiheessa ja valitse sitten mukautettu levyn näköistiedoston polku paikallisen tietokoneen säilö:

```bash
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Voit luoda mukautetun kuvan AM
Kun luot mukautetun levyn näköistiedoston VMs, määritä levyn näköistiedoston URI. Varmista, että mukautettu levyn näköistiedoston tallennuspaikan kohde-tallennustilan tilin vastineita. Voit luoda oman AM Azure CLI tai Resurssienhallinta JSON-mallin avulla.


### <a name="create-a-vm-using-the-azure-cli"></a>Luo AM, Azure-CLI käyttäminen
Voit määrittää `--image-urn` parametrin `azure vm create` komento osoittavan mukautetun levyn näköistiedoston. Varmistaa, että `--storage-account-name` vastaa tallennustilan tilin mukautettujen levyn näköistiedoston tallennuspaikan. Ei tarvitse tallentaa oman VMs mukautetun levyn kuvana saman säilön avulla. Varmista, että muut säilöjen luominen samalla tavalla kuin edellä kuvatut toimet ennen mukautetun levyn kuvat ladataan.

Seuraavassa esimerkissä luodaan AM, jonka nimi on `myVM` mukautetun levyn kuvasta:

```bash
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Haluat määrittää tai vastauksen kysyy, kaikki muut parametrit vaatii `azure vm create` komento, kuten VPN, julkiseen IP-osoite, käyttäjänimi ja SSH avaimet. Lisätietoja [käytettäviin CLI Resurssienhallinta](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Luo AM, JSON mallin avulla
Azure Resurssienhallinta mallit ovat JavaScript Object Notation (JSON)-tiedostot, jotka haluat luoda ympäristössä. Mallit ovat eritellä eri resurssin tarjoajien, kuten Laske tai verkossa. Voit käyttää aiemmin luotuja malleja tai kirjoittaa omia. Lisätietoja [Resurssienhallinta ja mallien avulla](../azure-resource-manager/resource-group-overview.md).

Sisällä `Microsoft.Compute/virtualMachines` tarjoajan mallin, sinulla `storageProfile` solmu, joka sisältää tiedot, että AM. Voit muokata kaksi tärkeimmät parametrit on `image` ja `vhd` , joka osoittaa mukautetun levyn näköistiedoston ja uusi AM virtual levyn URI. Seuraavassa kuvassa näkyy esimerkki käyttäminen mukautettujen levyn näköistiedoston JSON:

```bash
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Voit käyttää [tätä mallia, voit luoda mukautetun kuvasta AM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) tai Lue lisätietoja [Azure Resurssienhallinta-mallien luomisesta](../resource-group-authoring-templates.md). 

Kun olet määrittänyt mallin, voit luoda oman VMs avulla `azure group deployment create` komento. Määritä JSON mallin URI `--template-uri` parametri:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Jos sinulla on JSON-tiedosto tallennetaan paikallisesti tietokoneeseen, voit käyttää `--template-file` parametrin sijaan:

```bash
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Seuraavat vaiheet
Kun olet valmistellut ja ladata mukautetun virtual levytilaa, voit lukea lisää [Resurssienhallinta ja mallien](../azure-resource-manager/resource-group-overview.md)käyttämisestä. Voit myös kannattaa lisäämään [tietoja DVD-levyllä](virtual-machines-linux-add-disk.md) uuden VMs. Jos sinulla on kohdassa VMs, joita haluat käyttää sovellusten, muista [Avaa portit ja päätepisteet](virtual-machines-linux-nsg-quickstart.md).