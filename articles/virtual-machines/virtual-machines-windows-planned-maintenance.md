<properties
    pageTitle="Suunniteltu ylläpito Windows VMs | Microsoft Azure"
    description="Mitä Azure suunniteltu ylläpito on ja miten se vaikuttaa oman Windows näennäiskoneiden Azure"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="drewm"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="04/26/2016"
    ms.author="drewm"/>

# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Suunniteltu ylläpito näennäiskoneiden Azure-tietokannassa


Ymmärtää, mitä Azure suunniteltu ylläpito on ja miten se voi vaikuttaa Windows näennäiskoneiden käytettävyyttä. Tässä artikkelissa on myös [Linux näennäiskoneiden](virtual-machines-linux-planned-maintenance.md)käytettävissä. 

Tässä artikkelissa on taustan siten, että Azure suunniteltu ylläpito-prosessin. Jos kenties selvittää, miksi oman AM käynnistettävä, voit [lukea tämän blogin viestiin, joka käsittelee tarkasteleminen AM uudelleen lokitiedot](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="why-azure-performs-planned-maintenance"></a>Miksi Azure suorittaa suunniteltu ylläpito

Microsoft Azure suorittaa päivitykset säännöllisesti yli maapallo luotettavasti, suorituskykyä ja näennäiskoneiden host toimiva infrastruktuuri suojauksen parantamiseksi. Monia nämä päivitykset, jotka suoritetaan ilman mitään vaikutusta näennäiskoneiden tai pilvipalveluihin, mukaan lukien muistin säilyttäminen päivitykset.

Jotkin päivitykset edellyttävät kuitenkin, että näennäiskoneiden koskee infrastruktuuri tarvittavat päivitykset uudelleen. Näennäiskoneiden Sammuta aikana olemme korjaustiedoston infrastruktuuri ja sitten näennäiskoneiden käynnistetään uudelleen.

Huomaa, että on kahdenlaisia, jotka vaikuttavat oman näennäiskoneiden käytettävyyttä ylläpito: suunnitellut ja suunnittelemattomat. Tällä sivulla kerrotaan, miten Microsoft Azure suorittaa suunniteltu ylläpito. Saat lisätietoja suunnittelematon ylläpito [ymmärtäminen suunniteltu ja suunnittelematon ylläpito](virtual-machines-windows-manage-availability.md).

[AZURE.INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]
