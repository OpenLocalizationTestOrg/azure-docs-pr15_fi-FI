<properties
    pageTitle="MySQL asentaminen OpenSUSE AM | Microsoft Azure"
    description="Lue MySQL asennetaan OpenSUSE Linux VMirtual-koneen Azure-tietokannassa."
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
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Asenna MySQL käynnissä OpenSUSE Linux Azure virtual tietokoneeseen

[MySQL] [ MySQL] on suosituimpia ja Avaa lähde SQL-tietokantaan. Tässä opetusohjelmassa näytetään, miten voit luoda virtual koneeseen, jossa käytetään OpenSUSE Linux ja asenna MySQL.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>Luo virtual koneeseen, jossa käytetään OpenSUSE Linux

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Asentaminen ja suorittaminen MySQL virtuaalikoneen

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Seuraavat vaiheet
Katso lisätietoja MySQL [MySQL ohjeissa][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com

