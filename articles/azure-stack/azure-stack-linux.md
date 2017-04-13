<properties
    pageTitle="Linux vieraiden Azure pinossa | Microsoft Azure"
    description="Lue, miten luoda Linux-pohjaiset näennäiskoneiden Azure pinossa."
    services="azure-stack"
    documentationCenter=""
    authors="anjayajodha"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anajod"/>
    
# <a name="deploy-linux-virtual-machines-on-azure-stack"></a>Ottaa käyttöön Linux näennäiskoneiden Azure pinossa

Voit ottaa Linux näennäiskoneiden Azure pinon Käsitteiden, lisäämällä Linux-pohjaiset kuva kyselyjä Azure pinon Marketplacesta. Useat Linux toimittajat annettu kuvia, jotka voidaan lisätä Azure-pino Käsitteiden tai muodosta oma.

## <a name="download-an-image"></a>Lataa kuva

 1. Voit ladata ja Azure pinon yhteensopivan kuvan poimiminen seuraavista linkeistä tai valmisteleminen oman:
  - [Bitnami](https://bitnami.com/azure-stack)
  - [CentOS](http://olstacks.cloudapp.net/latest/)
  - [CoreOS](https://stable.release.core-os.net/amd64-usr/current/coreos_production_azure_image.vhd.bz2)
  - [SuSE](https://download.suse.com/Download?buildid=VCFi7y7MsFQ~)
  - [Ubuntu 14.04 l.](https://partner-images.canonical.com/azure/azure_stack/) / [Ubuntu 16.04 l.](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)
  
 2. Pura kuva Näennäiskiintolevyn tarvittaessa ja [Lisää kuva Marketplace](azure-stack-add-vm-image.md). Varmista, että `OSType` parametrin arvoksi on asetettu `Linux`.
 
 3. Kun olet lisännyt kuvan Marketplace-Marketplace kohde luodaan ja Linux virtual koneen voit ottaa käyttöön.
  
## <a name="prepare-your-own-image"></a>Oman kuvan valmisteleminen

1. Valmistele oman Linux-kuva, käytä jotakin seuraavista ohjeista:
 - [CentOS perustuva jakelu](../virtual-machines/virtual-machines-linux-create-upload-centos.md)
 - [Debian Linux](../virtual-machines/virtual-machines-linux-debian-create-upload-vhd.md)
 - [Oracle Linux](../virtual-machines/virtual-machines-linux-oracle-create-upload-vhd.md)
 - [Punainen on Enterprise Linux](../virtual-machines/virtual-machines-linux-redhat-create-upload-vhd.md)
 - [SLES & openSUSE](../virtual-machines/virtual-machines-linux-suse-create-upload-vhd.md)
 - [Ubuntu](../virtual-machines/virtual-machines-linux-create-upload-ubuntu.md)

2. Lataa ja asenna [Azure Linux-agentti](https://github.com/Azure/WALinuxAgent/)

    Azure Linux agentti versio 2.1.3 tai uudempi versio vaaditaan valmistella Linux-AM Azure pinossa. Tämä versio on agentti pakettina sisällyttäminen yllä jo jaot monia niiden säilöjen tietoihin (jonka nimi on yleensä `WALinuxAgent` tai `walinuxagent`). Kuitenkin jos Azure agent versio on pienempi kuin 2.1.3 (eli 2.0.18 tai pienempi), valitse sinun on asennettava agentti manuaalisesti. Asennettu versio voidaan määrittää pakettinimi tai suorittamalla `/usr/sbin/waagent -version` -AM.

    Noudattamalla seuraavia ohjeita asentaminen manuaalisesti - Azure Linux-agentti

 - Lataa uusimmat Azure Linux-agentti ensin [Github](https://github.com/Azure/WALinuxAgent/releases), esimerkiksi:

            # wget https://github.com/Azure/WALinuxAgent/archive/v2.2.0.tar.gz

 - Pura Azure agentti:

            # tar -vzxf v2.2.0.tar.gz

 - Asenna python setuptools

        **Debian / Ubuntu**

            # sudo apt-get update
            # sudo apt-get install python-setuptools

        **Ubuntu 16.04+**

            # sudo apt-get install python3-setuptools

        **RHEL / CentOS / Oracle Linux**

            # sudo yum install python-setuptools

 - Asenna Azure agentti:

            # cd WALinuxAgent-2.2.0
            # sudo python setup.py install --register-service

    Järjestelmien kanssa Python 2.x ja Python asennettu 3.x-rinnakkais joutua varten suorittamalla seuraavan komennon:

        # sudo python3 setup.py install --register-service

    Lisätietoja on artikkelissa Azure Linux agentti [Lueminut-tiedosto](https://github.com/Azure/WALinuxAgent/blob/master/README.md).

3. [Lisää kuva Marketplace](azure-stack-add-vm-image.md). Varmista, että `OSType` parametrin arvoksi on asetettu `Linux`.

4. Kun olet lisännyt kuvan Marketplace-Marketplace kohde luodaan ja Linux virtual koneen voit ottaa käyttöön.

## <a name="next-steps"></a>Seuraavat vaiheet

[Usein kysytyt kysymykset Azure pino](azure-stack-faq.md)