<properties
    pageTitle="Perinteinen Linux AM päätepisteet määrittäminen | Microsoft Azure"
    description="Opi määrittämään Linux-AM Azure perinteinen portaalissa päätepisteet Salli viestintä Linux virtual koneen kanssa Azure"
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/13/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-linux-classic-virtual-machine-in-azure"></a>Voit määrittää päätepisteet Linux perinteinen virtual tietokoneeseen Azure-tietokannassa

Kaikki Linux näennäiskoneiden, jonka luot Azure perinteinen käyttöönoton mallin automaattisesti viestiä yksityisverkon kanavassa muiden näennäiskoneiden saman pilvipalvelussa tai VPN kanssa. Tietokoneiden Internetissä tai muiden virtual käyttäminen edellyttää kuitenkin päätepisteet ohjaamaan virtual tietokoneeseen saapuvaa verkkoliikennettä. Tässä artikkelissa on myös [Windows-näennäiskoneiden](virtual-machines-windows-classic-setup-endpoints.md)käytettävissä.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Resurssien hallinnan** käyttöönotto-mallissa päätepisteet on määritetty **Verkon suojausryhmien (NSGs)**avulla. Lisätietoja on artikkelissa [porttien avaaminen ja päätepisteet] (virtual-koneet-linux-nsg-quickstart.md).

Kun luot Linux virtual koneen Azure perinteinen-portaalissa, päätepisteen, suojattu runko (SSH) tavallisesti luodaan automaattisesti. Voit määrittää muita päätepisteet virtuaalikoneen luotaessa tai jälkeenpäin esitystapa tarpeen mukaan.
 

[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

* Voit myös luoda AM päätepisteen [Azure käyttöliittymä](../virtual-machines-command-line-tools.md)avulla. Suorita **azure AM päätepisteen luominen** -komento.

* Jos olet luonut virtual machine resurssien hallinnan käyttöönottomalli, voit käyttää Azure-CLI Resurssienhallinta-tilassa ja [luoda verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-create-nsg-arm-cli.md) AM liikenteen hallinta.