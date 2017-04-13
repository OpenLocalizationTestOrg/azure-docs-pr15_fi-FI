<properties
    pageTitle="Perinteinen Windows-AM päätepisteet määrittäminen | Microsoft Azure"
    description="Salli viestintä kanssa Windows-virtual machine Azure Windows-AM Azure perinteinen portaalissa päätepisteiden määrittäminen tietoja."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="how-to-set-up-endpoints-on-a-classic-windows-virtual-machine-in-azure"></a>Voit määrittää päätepisteet perinteinen Windows virtual tietokoneeseen Azure-tietokannassa


Kaikkien ikkunoiden näennäiskoneiden, jonka luot Azure perinteinen käyttöönotto-mallin avulla voit automaattisesti viestiä yksityisen päälle ja muiden näennäiskoneiden saman pilvipalveluun tai VPN-verkon kanavan. Tietokoneiden Internetissä tai muiden virtual käyttäminen edellyttää kuitenkin päätepisteet ohjaamaan virtual tietokoneeseen saapuvaa verkkoliikennettä. Tässä artikkelissa on myös [Linux näennäiskoneiden](virtual-machines-linux-classic-setup-endpoints.md)käytettävissä.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]**Resurssien hallinnan** käyttöönotto-mallissa päätepisteet on määritetty **Verkon suojausryhmien (NSGs)**avulla. Lisätietoja on artikkelissa [Salli ulkoisen käytön, että AM Azure-portaalissa] (virtual-machines-windows-nsg-quickstart-portal.md).

Luodessasi virtual kone Windows Azure perinteinen portaalissa yleisiä päätepisteet muistuttavat Etätyöpöytä ja Windows PowerShellin Etäpalvelujen yleensä luodaan sinulle automaattisesti. Voit määrittää muita päätepisteet virtuaalikoneen luotaessa tai jälkeenpäin esitystapa tarpeen mukaan.



[AZURE.INCLUDE [virtual-machines-common-classic-setup-endpoints](../../includes/virtual-machines-common-classic-setup-endpoints.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

* Määritä AM päätepiste Azure PowerShell cmdlet-komennon avulla, katso [Lisää AzureEndpoint](https://msdn.microsoft.com/library/azure/dn495300.aspx).

* Azure PowerShell cmdlet-komennon avulla voit hallita Käyttöoikeusluettelon päätepisteen-kohdassa [hallinta käyttöoikeusluettelot (käyttöoikeusluettelot) päätepisteiden PowerShell-toiminnolla](../virtual-network/virtual-networks-acl-powershell.md).

* Jos olet luonut virtual machine resurssien hallinnan käyttöönottomalli, voit [luoda verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-create-nsg-arm-ps.md) AM liikenteen hallinta PowerShellin Azure.
