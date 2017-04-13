<properties
    pageTitle="Palauta tila käyttöön Azure Linux VMs käyttämällä VMAccess-tunniste | Microsoft Azure"
    description="Palauta tila käyttöön Azure Linux VMs käyttämällä VMAccess-tunniste."
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
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="manage-users-ssh-and-check-or-repair-disks-on-azure-linux-vms-using-the-vmaccess-extension"></a>Hallitse käyttäjien SSH ja Tarkista tai korjaa levyille Azure Linux VMs käyttämällä VMAccess-tunniste

Tässä artikkelissa kerrotaan käyttämisestä Azure VMAcesss tunniste Tarkista tai korjaa DVD-levyllä, Palauta käyttöoikeudet, hallita käyttäjätilejä tai palauta Linux SSHD-määritys. Artikkelin edellyttää:

- Azure tili ([maksuton kokeiluversio](https://azure.microsoft.com/pricing/free-trial/)).

- sisään kirjautunut [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure CLI _on oltava_ Azure Resurssienhallinta tilassa `azure config mode arm`.

## <a name="quick-commands"></a>Pikakomennot

VMAccess käyttämisestä Linux VMs kahdella tavalla:

- Azure-CLI ja tarvittavat parametrit käyttäminen.
- Voit käyttää VMAccess käsittelee ja suorittaa sitten raaka JSON-tiedostot.

Pika-komento-osan tarkastellaan käyttämään Azure-CLI `azure vm reset-access` menetelmää. Korvaa arvot, jotka sisältävät "Esimerkki" arvot itse-ympäristöstä komento seuraavasti.

## <a name="create-a-resource-group-and-linux-vm"></a>Resurssiryhmä ja Linux AM luominen

```bash
azure group create myResourceGroup westus
```

## <a name="create-a-debian-vm"></a>Luo Debian AM

```bash
azure vm quick-create \
-M ~/.ssh/id_rsa.pub \
-u myAdminUser \
-g myResourceGroup \
-l westus \
-y Linux \
-n myVM \
-Q Debian
```

## <a name="reset-root-password"></a>Pääkansio salasanan vaihtaminen

Pääkansio salasanan:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u root \
-p myNewPassword
```

## <a name="ssh-key-reset"></a>SSH avaimen palauttaminen

Jos haluat palauttaa pääkansio käyttäjän SSH-näppäintä:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub
```

## <a name="create-a-user"></a>Käyttäjän luominen

Luo käyttäjä:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-u myAdminUser \
-p myAdminUserPassword
```

## <a name="remove-a-user"></a>Käyttäjän poistaminen

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM \
-R myRemovedUser
```

## <a name="reset-sshd"></a>Palauta SSHD

Jos haluat palauttaa SSHD määritykset:

```bash
azure vm reset-access \
-g myResourceGroup \
-n myVM
-r
```


## <a name="detailed-walkthrough"></a>Laskun

### <a name="vmaccess-defined"></a>Määritetty VMAccess:

Linux AM levy on näkyvissä virheitä. Aiheuttaa salasanan pääkansion Linux AM tai vahingossa SSH-yksityinen avain. Jos, joka on tapahtunut takaisin palvelinkeskuksen päivinä, tarvitset asema ja avaa sitten KVM saat palvelimen konsolissa. Ajattele Azure VMAccess-tunniste kyseiseen KVM valitsin, jonka avulla voit käyttää Palauta access Linux tai suorittaa levyn tason ylläpito konsolin.

Saat yksityiskohtaiset ongelmatilanteita tarkastellaan VMAccess, joka käyttää raaka JSON tiedostot pitkä lomake.  Näitä VMAccess JSON-tiedostoja voidaan kutsua myös Azure malleista.

### <a name="using-vmaccess-to-check-or-repair-the-disk-of-a-linux-vm"></a>Tarkista tai korjaa Linux AM levy VMAccess avulla

VMAccess avulla voit tehdä fsck Suorita kohdassa Linux AM levylle.  Voit tehdä myös levyn tarkistaminen ja levyn korjaus VMAccess.

Voit tarkistaa ja korjaa levyn käyttää tätä VMAccess-komentosarjaa:

`disk_check_repair.json`

```json
{
  "check_disk": "true",
  "repair_disk": "true, user-disk-name"
}
```

Suorita VMAccess komentosarja kanssa:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path disk_check_repair.json
```

### <a name="using-vmaccess-to-reset-user-access-to-linux"></a>Palauta käyttöoikeudet Linux VMAccess avulla

Jos access on katkennut varmenteiden Linux AM käyttöön, voit käynnistää VMAccess komentosarjan pääkansion salasanan.

Pääkansio salasanan, käytä VMAccess-komentosarja:

`reset_root_password.json`

```json
{
  "username":"root",
  "password":"myNewPassword",   
}
```

Suorita VMAccess komentosarja kanssa:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_root_password.json
```

Vaihtamaan SSH avain pääkansio käyttäjän käyttää tätä VMAccess-komentosarjaa:

`reset_ssh_key.json`

```json
{
  "username":"myAdminUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myAdminUser@myVM",   
}
```

Suorita VMAccess komentosarja kanssa:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_ssh_key.json
```

### <a name="using-vmaccess-to-manage-user-accounts-on-linux"></a>VMAccess avulla voit hallita käyttäjätilejä Linux

VMAccess on Python komentosarjan, joka voidaan hallita Linux AM käyttäjille ilman kirjautumisesta ja sudo tai pääkansion-tilillä.

Luo käyttäjä käyttää tätä VMAccess-komentosarjaa:

`create_new_user.json`

```json
{
"username":"myNewUser",
"ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
"password":"myNewUserPassword",
}
```

Suorita VMAccess komentosarja kanssa:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path create_new_user.json
```

Käyttäjän poistaminen Käytä tätä VMAccess komentosarjaa:

`remove_user.json`

```json
{
"remove_user":"myDeletedUser",
}
```

Suorita VMAccess komentosarja kanssa:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path remove_user.json
```

### <a name="using-vmaccess-to-reset-the-sshd-configuration"></a>Palauttaa SSHD määritykset VMAccess avulla

Jos tehdä muutoksia työnkulkuun Linux VMs SSHD ja sulje SSH-yhteyden, ennen kuin muutokset tarkistetaan, voit ehkä estettävä SSH'ing takaisin sisään.  VMAccess avulla voidaan palauttaa SSHD määritykset takaisin hyvä kokoonpanon ilman lokiin SSH päälle.

Palauta SSHD määritysten käyttää tätä VMAccess-komentosarjaa:

`reset_sshd.json`

```json
{
  "reset_ssh": true
}
```

Suorita VMAccess komentosarja kanssa:

```bash
azure vm extension set \
myResourceGroup \
myVM \
VMAccessForLinux \
Microsoft.OSTCExtensions * \
--private-config-path reset_sshd.json
```

## <a name="next-steps"></a>Seuraavat vaiheet

Päivittäminen Linux käyttämällä Azure VMAccess laajennukset on yksi tapa tehdä muutoksia, valitse käynnissä Linux AM.  Voit myös muokata Linux-AM käynnistyksessä työkaluja, kuten cloud alusta ja Azure malleja.

[Tietoja virtuaalikoneen tunnisteet ja toiminnot](virtual-machines-linux-extensions-features.md)

[Azure Resurssienhallinta malleja Linux AM tunniste on yhtä aikaa](virtual-machines-linux-extensions-authoring-templates.md)

[Cloud alusta avulla voit mukauttaa Linux AM luonnin aikana](virtual-machines-linux-using-cloud-init.md)
