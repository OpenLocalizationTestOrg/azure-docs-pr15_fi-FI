<properties
   pageTitle="Luoda Linux AM Azure CLI käyttämällä | Microsoft Azure"
   description="Luo Linux AM Azure CLI avulla."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="v-livech"/>


# <a name="create-a-linux-vm-on-azure-by-using-the-cli"></a>Luoda Linux AM Azure CLI käyttämällä

Tässä artikkelissa kerrotaan käyttöönottamisesta nopeasti käyttämällä Linux virtual tietokoneen (AM) Azure `azure vm quick-create` komento Azure komentorivivalitsimet interface (CLI). `quick-create` Komento ottaa käyttöön AM sisällä basic, suojattu infrastruktuuri, voit käyttää prototyypin tai testata joka kertoo nopeasti. Artikkelin edellyttää:

- Azure tili ([maksuton kokeiluversio](https://azure.microsoft.com/pricing/free-trial/)).

- sisään kirjautunut [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure CLI _on oltava_ Azure Resurssienhallinta tilassa `azure config mode arm`.

Linux AM käyttöönottoa nopeasti myös [Azure portal](virtual-machines-linux-quick-create-portal.md).

## <a name="quick-commands"></a>Pikakomennot

Seuraavassa esimerkissä esitetään asentamisesta CoreOS AM ja liittää suojattu runko (SSH) key-tuotetunnuksen (että argumentit voivat olla erilaisia):

```bash
azure vm quick-create -M ~/.ssh/id_rsa.pub -Q CoreOS
```

## <a name="detailed-walkthrough"></a>Laskun

Seuraavat vaiheittainen kuvaus sisältää on otettu käyttöön, vaihe vaiheelta selitykset jokaisen vaiheen tekee kanssa UbuntuLTS-AM.

## <a name="vm-quick-create-aliases"></a>AM pika-tunnusten luominen

Valitse jakauman nopea tapa on käyttää yhdistetty yleisimmät OS jaot Azure CLI tunnuksia. Seuraavassa taulukossa on lueteltu aliases (vuodesta Azure CLI versio 0,10). Kaikki asennuksia, jotka käyttävät `quick-create` VMs, jota SSD asemaa Suoritettaessa tallennustilan, joka tarjoaa nopeammin valmistelu ja tehokas levyn käyttöä varmuuskopioidaan oletusarvo. (Näiden tunnusten edustavat pieni käytettävissä jaot Azure-osa. Etsi lisää kuvia Azure Marketplacesta [PowerShellin kuvan etsiminen](virtual-machines-linux-cli-ps-findimage.md), [verkossa](https://azure.microsoft.com/marketplace/virtual-machines/)tai [oman mukautetun kuvan lataaminen](virtual-machines-linux-create-upload-generic.md).)

| Alias     | Publisherin | Tarjouksen        | TUOTE         | Versio |
|:----------|:----------|:-------------|:------------|:--------|
| CentOS    | OpenLogic | CentOS       | 7.2         | uusimman  |
| CoreOS    | CoreOS    | CoreOS       | Vakaa      | uusimman  |
| Debian    | credativ  | Debian       | 8           | uusimman  |
| openSUSE  | SUSE      | openSUSE     | 13.2        | uusimman  |
| RHEL      | Punainen on    | RHEL         | 7.2         | uusimman  |
| UbuntuLTS | Kanoninen | Ubuntu Server | 14.04.4-LTS | uusimman  |

Seuraavat osat käyttöä `UbuntuLTS` alias **ImageURN** -vaihtoehto (`-Q`) ottamaan Ubuntu 14.04.4-l. palvelimeen.

Edellisen `quick-create` Esimerkki korostettuina vain `-M` merkinnän tunnistavan SSH julkinen avain lataaminen poistettaessa käytöstä SSH salasanat, niin pyytää sinua antamaan seuraavat argumentit:

- resurssiryhmän nimi (mikä tahansa merkkijono on yleensä hyvin ensimmäisen Azure resurssiryhmä)
- AM nimi
- sijainti (`westus` tai `westeurope` on hyvä oletukset)
- Linux (Jos haluat sallia tietää, mitä haluat OS Azure)
- käyttäjänimi

Seuraava esimerkki määrittää kaikki arvot niin, että ei muita kehotteita tarvitaan. Kun olet valinnut `~/.ssh/id_rsa.pub` ssh-rsa format julkisen avaimen-tiedostona, se toimii on:

```bash
azure vm quick-create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--admin-username myAdminUser \
--ssh-public-file ~/.ssh/id_rsa.pub \
--image-urn UbuntuLTS
```

Tulosteen pitäisi näyttää samalta kuin seuraava tulos lohko:

```bash
info:    Executing command vm quick-create
+ Listing virtual machine sizes available in the location "westus"
+ Looking up the VM "myVM"
info:    Verifying the public key SSH file: /Users/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account cli16330708391032639673
+ Looking up the NIC "examp-westu-1633070839-nic"
info:    An nic with given name "examp-westu-1633070839-nic" not found, creating a new one
+ Looking up the virtual network "examp-westu-1633070839-vnet"
info:    Preparing to create new virtual network and subnet
/ Creating a new virtual network "examp-westu-1633070839-vnet" [address prefix: "10.0.0.0/16"] with subnet "examp-westu-1633070839-snet" [address prefix: "10.+.1.0/24"]
+ Looking up the virtual network "examp-westu-1633070839-vnet"
+ Looking up the subnet "examp-westu-1633070839-snet" under the virtual network "examp-westu-1633070839-vnet"
info:    Found public ip parameters, trying to setup PublicIP profile
+ Looking up the public ip "examp-westu-1633070839-pip"
info:    PublicIP with given name "examp-westu-1633070839-pip" not found, creating a new one
+ Creating public ip "examp-westu-1633070839-pip"
+ Looking up the public ip "examp-westu-1633070839-pip"
+ Creating NIC "examp-westu-1633070839-nic"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the storage account clisto1710997031examplev
+ Creating VM "myVM"
+ Looking up the VM "myVM"
+ Looking up the NIC "examp-westu-1633070839-nic"
+ Looking up the public ip "examp-westu-1633070839-pip"
data:    Id                              :/subscriptions/2<--snip-->d/resourceGroups/exampleResourceGroup/providers/Microsoft.Compute/virtualMachines/exampleVMName
data:    ProvisioningState               :Succeeded
data:    Name                            :exampleVMName
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :Canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :14.04.4-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clic7fadb847357e9cf-os-1473374894359
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli16330708391032639673.blob.core.windows.net/vhds/clic7fadb847357e9cf-os-1473374894359.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM
data:      User Name                     :myAdminUser
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-42-FB
data:          Provisioning State        :Succeeded
data:          Name                      :examp-westu-1633070839-nic
data:          Location                  :westus
data:            Public IP address       :138.91.247.29
data:            FQDN                    :examp-westu-1633070839-pip.westus.cloudapp.azure.com
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://clisto1710997031examplev.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm quick-create command OK
```

## <a name="log-in-to-the-new-vm"></a>Uusi AM kirjautuminen

Kirjaudu oman AM tulosteen luetellut julkiseen IP-osoitteen avulla. Voit käyttää myös täydellinen toimialuenimi (FQDN), joka näkyy:

```bash
ssh -i ~/.ssh/id_rsa.pub ahmet@138.91.247.29
```

Kirjautumisen pitäisi näyttää suunnilleen seuraava tulos lohko:

```bash
Warning: Permanently added '138.91.247.29' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 14.04.4 LTS (GNU/Linux 3.19.0-65-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Thu Sep  8 22:50:57 UTC 2016

  System load: 0.63              Memory usage: 2%   Processes:       81
  Usage of /:  39.6% of 1.94GB   Swap usage:   0%   Users logged in: 0

  Graph this data and manage this system at:
    https://landscape.canonical.com/

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

myAdminUser@myVM:~$
```

## <a name="next-steps"></a>Seuraavat vaiheet

`azure vm quick-create` Komento on nopeasti käyttöön AM, jotta voit kirjautua bash shell ja toimiminen. Kuitenkin `vm quick-create` laaja ohjausobjektin anneta eikä myöskään, joiden avulla voit luoda monimutkaisia ympäristössä.  Jos haluat ottaa Linux AM, joka on mukautettu, yrityksen infrastruktuuri, noudata on seuraavissa artikkeleissa:

- [Luo tietyn käyttöönoton Azure Resurssienhallinta-mallin avulla](virtual-machines-linux-cli-deploy-templates.md)
- [Luo mukautettu ympäristön Linux-AM Azure CLI komentojen käyttäminen suoraan](virtual-machines-linux-create-cli-complete.md)
- [Luo SSH suojattu Linux AM Azure mallien käyttäminen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

Voit myös [avulla `docker-machine` Azure ohjain, jossa voit luoda nopeasti Linux-AM kuin docker isäntä komentoja](virtual-machines-linux-docker-machine.md).
