<properties
    pageTitle="Päivitä Azure Linux-agentti-GitHub | Microsoft Azure"
    description="Lue, miten Linux-AM lateset versioon Azure-Github-päivitys Azure Linux-agentti"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="SuperScottz"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="mingzhan"/>


# <a name="how-to-update-the-azure-linux-agent-on-a-vm-to-the-latest-version-from-github"></a>Valitse AM Azure Linux-agentti päivittämisestä uusimpaan versioon GitHub

Jos haluat päivittää oman [Azure Linux Agent](https://github.com/Azure/WALinuxAgent) -Linux-AM Azure-tietokannassa, täytyy:

1. Azure käynnissä Linux-AM.
2. Kyseisen Linux AM käyttämällä SSH yhteys.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

<br>

> [AZURE.NOTE] Jos suoritat tämän tehtävän poistaminen Windows-tietokoneessa, voit käyttää painovärit, muste SSH, Linux-koneen kyselyjä. Katso lisätietoja, [miten virtuaalikoneen käynnissä Linux Kirjaudu](virtual-machines-linux-mac-create-ssh-keys.md).

Azure vahvistettava Linux distros on otettu käyttöön Azure Linux Agent-paketin niiden säilöjen tietoihin, tarkista ja asenna uusin versio Distro kyseisen säilöstä ensin Jos mahdollista.  

Saat Ubuntu vain kirjoittamalla:

    #sudo apt-get install walinuxagent

Ja CentOS, kirjoita:

    #sudo yum install waagent


Oracle Linux, varmista, `Addons` säilö on otettu käyttöön. Valitse Muokkaa tiedostoa `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux-6) tai `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), ja muuttaa viivan `enabled=0` , `enabled=1` **[ol6_addons]** ja **[ol7_addons]** tässä tiedostossa-kohdassa.

Asenna uusin versio Azure Linux-agentti, kirjoita:


    #sudo yum install WALinuxAgent

Jos et löydä lisäosan säilö voit vain lisätä viivat .repo tiedoston lopussa Oracle Linux-version mukaan:

Oracle Linux 6 näennäiskoneiden:

    [ol6_addons]
    name=Add-Ons for Oracle Linux $releasever ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
    gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
    gpgcheck=1
    enabled=1

Oracle Linux 7 näennäiskoneiden:

    [ol7_addons]
    name=Oracle Linux $releasever Add ons ($basearch)
    baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
    gpgcheck=1
    enabled=0

Kirjoita:

    #sudo yum update WALinuxAgent

Tavallisesti tämä on kaikki välttämätön, mutta jos jostain syystä sinun on asennettava se https://github.com suoraan noudattamalla seuraavia ohjeita.


## <a name="install-wget"></a>Asenna wget

Kirjaudu oman AM SSH avulla.

Asenna wget (on joitakin distros, älä asenna se oletusarvoisesti, kuten Redhat, CentOS ja Oracle Linux versioissa 6.4 ja 6.5) kirjoittamalla `#sudo yum install wget` komentorivillä.


## <a name="download-the-latest-version"></a>Lataa uusin versio

Avaa [Azure Linux-agentti GitHub-versiossa](https://github.com/Azure/WALinuxAgent/releases) verkkosivun ja Lue uusimmat versionumero. (Löydät nykyisen version kirjoittamalla `#waagent --version`.)

### <a name="for-version-20x-type"></a>Version 2.0.x:stä, tyyppi:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-[version]/waagent  

   Seuraava rivi käyttää versio 2.0.14 esimerkkinä:

    #wget https://raw.githubusercontent.com/Azure/WALinuxAgent/WALinuxAgent-2.0.14/waagent  

### <a name="for-version-21x-or-later-type"></a>Version 2.1.x tai sitä uudempi versio, kirjoita:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-[version].zip
    #unzip WALinuxAgent-[version].zip
    #cd WALinuxAgent-[version]

   Seuraava rivi käyttää versio 2.1.0 esimerkkinä:

    #wget https://github.com/Azure/WALinuxAgent/archive/WALinuxAgent-2.1.0.zip
    #unzip WALinuxAgent-2.1.0.zip  
    #cd WALinuxAgent-2.1.0

## <a name="install-the-azure-linux-agent"></a>Asenna Azure Linux-agentti

### <a name="for-version-20x-use"></a>Version 2.0.x:stä, käytä:

 Tee waagent suoritettava:

    #chmod +x waagent

 Kopioi uusi suoritettavan tiedoston /usr/sbin /.

  Useimmat Linux voit käyttää:

    #sudo cp waagent /usr/sbin

  CoreOS voit käyttää:

    #sudo cp waagent /usr/share/oem/bin/

  Jos kyseessä on uusi asennus Azure Linux edustajan, suorittaa:
 
    #sudo /usr/sbin/waagent -install -verbose

### <a name="for-version-21x-use"></a>Version 2.1.x, käytä:

Voit joutua Asenna paketti `setuptools` nähdä ensin-- [tähän](https://pypi.python.org/pypi/setuptools). Suorita:

    #sudo python setup.py install

## <a name="restart-the-waagent-service"></a>Waagent-palvelun käynnistäminen uudelleen

Useimpien linux Distros:

    #sudo service waagent restart

Ubuntu voit käyttää:

    #sudo service walinuxagent restart

CoreOS voit käyttää:

    #sudo systemctl restart waagent

## <a name="confirm-the-azure-linux-agent-version"></a>Vahvista Azure Linux Agent-versio

    #waagent -version

Varten CoreOS-komento ei välttämättä toimi.

Näet, että Azure Linux Agent-versio on päivitetty uuteen versioon.

Saat lisätietoja Azure Linux-agentti, [Azure Linux agentti Lueminut-tiedosto](https://github.com/Azure/WALinuxAgent).
