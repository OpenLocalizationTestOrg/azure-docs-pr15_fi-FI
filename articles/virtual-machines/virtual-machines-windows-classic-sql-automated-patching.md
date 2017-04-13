<properties
    pageTitle="Automaattinen korjaaminen, SQL Server VMs (perinteinen) | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan automaattinen korjaaminen-ominaisuus, SQL Server näennäiskoneiden Azure perinteinen käyttöönotto-tilassa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automaattinen korjaaminen, SQL Server Azure-Virtuaalikoneissa (perinteinen)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-automated-patching.md)
- [Perinteinen](virtual-machines-windows-classic-sql-automated-patching.md)

Automaattinen korjaus muodostaa ylläpito-ikkuna, Azure Virtual Machine SQL Serveriä. Automaattiset päivitykset voidaan asentaa vain ylläpidon aikana. SQL Serverin Näin varmistat, että Järjestelmäpäivitykset ja kaikki liittyvät käynnistyy ilmetä tietokannan paras mahdollinen aikaan. Automaattinen korjaus määräytyy [SQL Server IaaS Agent-tunniste](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Tämän artikkelin Resurssienhallinta-versio on artikkelissa [Automaattinen korjaaminen SQL Server Azure näennäiskoneiden resurssien hallinta](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Edellytykset

Jos haluat käyttää automaattinen korjaaminen, harkitse seuraavat edellytykset:

**Käyttöjärjestelmä**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-versiota**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Asenna uusimmat Azure PowerShell-komennoilla](../powershell-install-configure.md).

**SQL Server IaaS tunniste**:

- [Asenna SQL Server IaaS-laajennus](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Asetukset

Seuraavassa taulukossa kuvataan vaihtoehdot, jotka voidaan määrittää automaattinen korjaaminen. Perinteinen VMs, sinun on käytettävä PowerShell asetusten määrittäminen.

|Asetus|Mahdolliset arvot|Kuvaus|
|---|---|---|
|**Automaattinen korjaaminen**|Ottaa käyttöön/poistaa käytöstä (poissa käytöstä)|Ottaa käyttöön tai poistaa Azure virtuaalikoneen, automaattinen korjaaminen.|
|**Ylläpitoaikataulu**|Everyday, maanantai, tiistai, keskiviikko, torstai, perjantai, lauantai, sunnuntai|Lataaminen ja asentaminen Windows ja Microsoft SQL Server päivitykset virtuaalikoneen ajoitusta.|
|**Aloituspäivämäärä ylläpito**|0 – 24|Päivitä virtuaalikoneen paikallisen alkamisaika.|
|**Ylläpito ikkunan kesto**|30 180|Viimeistele lataaminen ja asentaminen päivitysten sallittu minuuttimäärä.|
|**Korjaustiedoston luokka**|Tärkeää|Lataa ja asenna päivitykset luokka.|

## <a name="configuration-with-powershell"></a>Määritysten PowerShellin avulla

Seuraavassa esimerkissä PowerShell käytetään määrittämistä aiemmin luotuun SQL Server-AM automaattinen korjaaminen. **Uusi AzureVMSqlServerAutoPatchingConfig** -komento määrittää automaattisia päivityksiä ylläpito uudessa ikkunassa.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Seuraavassa taulukossa kuvataan käytännössä aikataulussa Azure AM tämän esimerkin mukaan:

|Parametri|Vaikutus|
|---|---|
|**: N DayOfWeek**|Korjaukset asennettu joka torstai.|
|**MaintenanceWindowStartingHour**|Aloita 11:00 Suomen päivitykset.|
|**MaintenanceWindowsDuration**|Korjaukset on oltava asennettuna enintään 120 minuuttia. Perusteella alkamisaika, ne on täytä 1:00 pm.|
|**PatchCategory**|Ainoa tämä parametri on "Tärkeää".|

Saattaa kulua useita minuutteja asennetaan ja määritetään SQL Server IaaS Agent.

Käytöstä automaattinen korjaaminen, suorita sama komentosarjan ilman uusi-AzureVMSqlServerAutoPatchingConfig Ota käyttöön-parametri. Asennuksen kanssa voi kestää useita minuutteja käytöstä automaattinen korjaaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso tietoja käytettävissä automaatio muihin tehtäviin [SQL Server IaaS Agent-tunniste](virtual-machines-windows-classic-sql-server-agent-extension.md).

Katso lisätietoja SQL Server-käyttöjärjestelmässä Azure VMs [Yhteenvedossa Azuren näennäiskoneiden SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
