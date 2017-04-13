<properties
    pageTitle="Eri tapoja luoda Linux AM | Microsoft Azure"
    description="Tutustu erilaisiin tapoihin luomiseen Linux virtual koneen Azure, sekä linkit työkalujen ja opetusohjelmat palauttamista varten."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Eri tapoja luoda Linux virtual koneen Azure-tietokannassa

Voit halutessasi luoda työkaluilla ja työnkulut perehtynyt sinulle Linux virtual kone (AM) Azure-tietokannassa. Tässä artikkelissa on yhteenveto nämä erot ja esimerkkejä Linux VMs luomiseen.


## <a name="azure-cli"></a>Azure CLI 

Azure-CLI on käytettävissä kaikissa ympäristöissä npm-paketti, antamalla distro pakettien tai Docker säilö kautta. Lue lisää siitä, [miten asennetaan ja määritetään Azure-CLI](../xplat-cli-install.md). Seuraavat Opetusohjelmissa on esimerkkejä Azure-CLI käyttämiseen. Lue lisätietoja CLI quick start-komennot näkyvät kunkin artikkelin:

- [Voit luoda Linux AM Azure-CLI keskihajonta ja testi](virtual-machines-linux-quick-create-cli.md)
    - Seuraavassa esimerkissä luodaan käyttämällä julkinen avain nimeltä CoreOS-AM `azure_id_rsa.pub`:

    ```bash
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
        --image-urn CoreOS
    ```

- [Luo suojatun Linux-AM Azure mallin avulla](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
    - Seuraavassa esimerkissä luodaan AM, GitHub tallennetun mallin avulla:

    ```bash
    azure group create --name TestRG --location WestUS 
        --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```

- [Luo valmis käyttämällä Azure-CLI Linux-ympäristössä](virtual-machines-linux-create-cli-complete.md)
    - Sisältää kuormituksen tasauspalvelun ja useita VMs luominen käytettävyys määrittäminen.

- [Lisää Linux AM DVD-levyllä](virtual-machines-linux-add-disk.md)
    - Seuraava esimerkki Lisää 5 gt DVD-levyllä aiemmin luotuun AM nimeltä `TestVM`:

    ```bash
    azure vm disk attach-new --resource-group TestRG --vm-name TestVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure portal

[Azure portaalissa](https://portal.azure.com) voit luoda nopeasti AM, koska ei ole mitään järjestelmän asentaminen. Azure portaalin avulla voit luoda AM:

- [Luo Linux-AM Azure-portaalissa](virtual-machines-linux-quick-create-portal.md) 
- [Liitä DVD-levyllä Azure-portaalissa](virtual-machines-linux-attach-disk-portal.md)


## <a name="operating-system-and-image-choices"></a>Käyttöjärjestelmän ja kuva valinnat
Luotaessa AM voit valita kuvan käyttöjärjestelmä, jonka haluat suorittaa perusteella. Azure ja sen kumppaneiden tarjoavat useita kuvia, jotka muun muassa seuraavat sovellusten ja työkalujen valmiiksi asennettuna. Tai Lataa jokin omia kuvia (katso [Seuraava osa](#use-your-own-image)).

### <a name="azure-images"></a>Azure kuvat
Käytä `azure vm image` CLI komentoja, mikä on käytettävissä, publisher, distro release ja versiot.

Luettelo käytettävissä julkaisijat seuraavasti:

```bash
azure vm image list-publishers --location WestUS
```

Luettelon käytettävissä olevat tuotteet (tarjouksia) annetun Publisherin seuraavasti:

```bash
azure vm image list-offers --location WestUS --publisher Canonical
```

Luettelon annetun tarjouksen käytettävissä tuotteissa (distro versioiden) seuraavasti:

```bash
azure vm image list-skus --location WestUS --publisher Canonical --offer UbuntuServer
```

Luettelon kaikki käytettävissä olevat kuvat, annetun release seuraavasti:

```bash
azure vm image list --location WestUS --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Katso Lisää esimerkkejä selaaminen ja käytettävissä olevat kuvat, [Siirry ja valitse Azure virtuaalikoneen kuvien Azure-CLI](virtual-machines-linux-cli-ps-findimage.md).

`azure vm quick-create` Ja `azure vm create` komennot ovat aliases avulla voit nopeasti tavallisimmista distros ja niiden uusimmat versiot. Tunnusten on usein nopeampaa kuin Publisherin, tarjous, tuote ja versio aina, kun luot AM määrittäminen:

| Alias     | Publisherin | Tarjouksen        | TUOTE         | Versio |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | Centos       | 7.2         | uusimman  |
| CoreOS    | CoreOS    | CoreOS       | Vakaa      | uusimman  |
| Debian    | credativ  | Debian       | 8           | uusimman  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | uusimman  |
| RHEL      | Redhat    | RHEL         | 7.2         | uusimman  |
| SLES      | SLES      | SLES         | 12 SP1      | uusimman  |
| UbuntuLTS | Kanoninen | UbuntuServer | 14.04.4-LTS | uusimman  |

### <a name="use-your-own-image"></a>Oman kuvan käyttäminen

Jos asetat mukautuksia, voit käyttää kuvan perusteella aiemmin Azure-AM *määrittänyt* , että AM. Voit ladata luodun paikallisen kuvan. Lisätietoja tuetuista distros ja Opi käyttämään omia kuvia on seuraavissa artikkeleissa:

- [Azure vahvistettava jakelu](virtual-machines-linux-endorsed-distros.md)

- [Tietoja ei ole vahvistanut jakelu](virtual-machines-linux-create-upload-generic.md)

- [Voit siepata Linux virtual koneen Resurssienhallinta-mallina](virtual-machines-linux-capture-image.md).
    - Pika-aloitusopas Esimerkki komennoista kannattaa tallentaa aiemmin AM:

    ```bash
    azure vm deallocate --resource-group TestRG --vm-name TestVM
    azure vm generalize --resource-group TestRG --vm-name TestVM
    azure vm capture --resource-group TestRG --vm-name TestVM --vhd-name-prefix CapturedVM
    ```

## <a name="next-steps"></a>Seuraavat vaiheet

- Luo Linux AM [portal](virtual-machines-linux-quick-create-portal.md)ja [CLI](virtual-machines-linux-quick-create-cli.md)tai [Azure Resurssienhallinta-malli](virtual-machines-linux-cli-deploy-templates.md).

- Kun olet luonut Linux AM, [Lisää tiedot DVD-levyllä](virtual-machines-linux-add-disk.md).

- Vaiheittaiset ohjeet, jos haluat [palauttaa salasanan tai SSH näppäimet ja käyttäjien hallinta](virtual-machines-linux-using-vmaccess-extension.md)
