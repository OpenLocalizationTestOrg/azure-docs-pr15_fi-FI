<properties
    pageTitle="Cloud alusta avulla voit mukauttaa Linux AM luonnin aikana | Microsoft Azure"
    description="Cloud alusta avulla voit mukauttaa Linux AM luonnin aikana."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="v-livech"
/>

# <a name="using-cloud-init-to-customize-a-linux-vm-during-creation"></a>Cloud alusta avulla voit mukauttaa Linux AM luonnin aikana

Tässä artikkelissa kerrotaan, miten tekemään Määritä isäntänimi-, päivitys-paketit ja hallita käyttäjätilejä cloud alusta komentosarjan.  Cloud alusta-komentosarjoja kutsutaan from Azure CLI AM luonnin aikana.  Artikkelin edellyttää:

- Azure tili ([maksuton kokeiluversio](https://azure.microsoft.com/pricing/free-trial/)).

- sisään kirjautunut [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure CLI _on oltava_ Azure Resurssienhallinta tilassa `azure config mode arm`.

## <a name="quick-commands"></a>Pikakomennot

Luo cloud init.txt komentosarjan, joka määrittää isäntänimi ja päivittää kaikki paketit sudo käyttäjä lisää Linux.

```bash
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Luo resurssiryhmä käynnistää VMs kyselyjä.

```bash
azure group create myResourceGroup westus
```

Luo Linux-AM cloud alusta avulla voit määrittää sen käynnistyksen aikana.

```bash
azure vm create \
-g myResourceGroup \
-n myVM \
-l westus \
-y Linux \
-f myVMnic \
-F myVNet \
-P 10.0.0.0/22 \
-j mySubnet \
-k 10.0.0.0/24 \
-Q canonical:ubuntuserver:14.04.2-LTS:latest \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Laskun

### <a name="introduction"></a>Johdanto

Kun käynnistät uuden Linux AM, näyttöön tulee standard Linux AM, jolla ei ole mitään mukautetut tai valmis tarpeisiin. [Cloud alusta](https://cloudinit.readthedocs.org) on vakio tapa lisätä komentosarjan tai määritysten asetukset huomioon, että Linux AM, kun se käynnistetään ensimmäistä kertaa käyttäjäksi.

Azure-on kolmella eri tavalla muokkaamiseen Linux AM sivulle, kun se otetaan käyttöön tai käynnistetty.

- Lisää komentosarjoja cloud alusta.
- Lisää komentosarjoja Azure [VMAccess tunniste](virtual-machines-linux-using-vmaccess-extension.md).
- Azuren mallin avulla cloud alusta.
- Azuren mallin avulla [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md).

Lisätäkseen komentosarjojen milloin tahansa käynnistyksen jälkeen:

- Suorita komennot suoraan SSH
- Lisää komentosarjoja Azure [VMAccess tunniste](virtual-machines-linux-using-vmaccess-extension.md)imperatively tai Azure-mallissa
- Määritysten hallintatyökalut, kuten Ansible, suola, kokki ja Puppet.

>[AZURE.NOTE]: VMAccess Extension executes a script as root in the same way using SSH can.  However, using the VM extension enables several features that Azure offers that can be useful depending upon your scenario.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Cloud alusta saatavuudesta Azure AM pika-kuva tunnusten luominen:

| Alias     | Publisherin | Tarjouksen        | TUOTE         | Versio | cloud alusta |
|:----------|:----------|:-------------|:------------|:--------|:-----------|
| CentOS    | OpenLogic | Centos       | 7.2         | uusimman  | Ei         |
| CoreOS    | CoreOS    | CoreOS       | Vakaa      | uusimman  | Kyllä        |
| Debian    | credativ  | Debian       | 8           | uusimman  | Ei         |
| openSUSE  | SUSE      | openSUSE     | 13.2        | uusimman  | Ei         |
| RHEL      | Redhat    | RHEL         | 7.2         | uusimman  | Ei         |
| UbuntuLTS | Kanoninen | UbuntuServer | 14.04.4-LTS | uusimman  | Kyllä        |

Microsoft tekee yhteistyötä ja kumppaneita pääset cloud-alusta sisältää ja kuvia, jonka avulla he voivat Azure työskentelyä.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Cloud alusta komentosarjan lisääminen Azure-CLI AM luominen

Käynnistää cloud alusta komentosarjan luotaessa AM Azure, Määritä cloud alusta-tiedoston Azure-CLI `--custom-data` vaihtaminen.

Luo resurssiryhmä käynnistää VMs kyselyjä.

```bash
azure group create myResourceGroup westus
```

Luo Linux-AM cloud alusta avulla voit määrittää sen käynnistyksen aikana.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubnet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Aseta Linux AM isäntänimi cloud alusta komentosarjan luominen

Yksi minkä tahansa Linux AM helpoin ja tärkeimmät asetusten olisi isäntänimi. Olemme helposti tehdä tämän käyttämällä tämän komentosarjan cloud alusta.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Esimerkki cloud alusta komentosarjan nimeltä `cloud_config_hostname.txt`.

``` bash
#cloud-config
hostname: myservername
```

Cloud alusta-komentosarja asettaa isäntänimi AM ensimmäisen käynnistyksen aikana `myservername`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_hostname.txt
```

Kirjaudu sisään ja vahvista uusi AM isäntänimi.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Päivitä Linux cloud alusta komentosarjan luomisesta

Tietoturvasyistä haluat Ubuntu-AM päivittämään ensimmäisen käynnistyksen yhteydessä.  Voit käyttää cloud alusta emme voi tehdä seuraa-komentosarjan sen mukaan, käytössäsi on Linux-jakauman.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Esimerkki cloud alusta komentosarjan `cloud_config_apt_upgrade.txt` Debian perheen käyttöön

```bash
#cloud-config
apt_upgrade: true
```

Kun Linux on käynnistetty, kaikki asennetut paketit päivitetään kautta `apt-get`.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_apt_upgrade.txt
```

Kirjaudu sisään ja varmista, että kaikki paketit päivitetään.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Käyttäjän lisääminen Linux cloud alusta komentosarjan luominen

Yksi kaikki uudet Linux AM ensimmäisen tehtäviin on kunkin käyttäjän lisäämiseen itsellesi tai Vältä `root`. SSH avaimet parhaita käytäntöjä, suojaus ja käytettävyyttä ja ne lisätään `~/.ssh/authorized_keys` tiedoston cloud alusta-komentosarjan.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Esimerkki cloud alusta komentosarjan `cloud_config_add_users.txt` Debian perheen käyttöön

```bash
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Kun Linux on käynnistetty-luettelossa käyttäjät luodaan ja sudo-ryhmään.

```bash
azure vm create \
--resource-group myResourceGroup \
--name myVM \
--location westus \
--os-type Linux \
--nic-name myVMnic \
--vnet-name myVNet \
--vnet-address-prefix 10.0.0.0/22 \
--vnet-subnet-name mySubNet \
--vnet-subnet-address-prefix 10.0.0.0/24 \
--image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--admin-username myAdminUser \
--custom-data cloud_config_add_users.txt
```

Kirjaudu sisään ja tarkista juuri luomasi käyttäjä.

```bash
ssh myVM
cat /etc/group
```

Tulos

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Seuraavat vaiheet

Cloud alusta on jatkossa vakio yksisuuntainen, voit muokata Linux-AM käynnistyksen yhteydessä. Azure on myös AM tunnisteet, joiden avulla voit muokata oman LinuxVM käynnistyksen yhteydessä tai sen ollessa käynnissä. Azure-VMAccessExtension avulla voit palauttaa SSH tai käyttäjän tietoja AM on käynnissä. Cloud-alusta, jossa tarvitset salasanan uudelleen.

[Tietoja virtuaalikoneen tunnisteet ja toiminnot](virtual-machines-linux-extensions-features.md)

[Hallitse käyttäjien SSH ja Tarkista tai korjaa levyille Azure Linux VMs käyttämällä VMAccess-tunniste](virtual-machines-linux-using-vmaccess-extension.md)
