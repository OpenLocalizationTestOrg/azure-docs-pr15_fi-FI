<properties
    pageTitle="Automaattinen korjaaminen, SQL Server VMs (Resurssienhallinta) | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan automaattinen korjaaminen-ominaisuus, SQL Server näennäiskoneiden Azure resurssien hallinnan avulla."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automaattinen korjaaminen, SQL Server Azure-Virtuaalikoneissa (resurssien hallinta)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-automated-patching.md)
- [Perinteinen](virtual-machines-windows-classic-sql-automated-patching.md)

Automaattinen korjaus muodostaa ylläpito-ikkuna, Azure Virtual Machine SQL Serveriä. Automaattiset päivitykset voidaan asentaa vain ylläpidon aikana. SQL Serverin tämän rescriction varmistaa, että Järjestelmäpäivitykset ja kaikki liittyvät käynnistyy ilmetä tietokannan paras mahdollinen aikaan. Automaattinen korjaus määräytyy [SQL Server IaaS Agent-tunniste](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönottomalli. Tämän artikkelin perinteinen versio on artikkelissa [Automaattinen korjaaminen SQL Server Azure näennäiskoneiden Classic-ohjelmassa](virtual-machines-windows-classic-sql-automated-patching.md).

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

- Jos aiot määrittää PowerShellin automaattinen korjaaminen, [Asenna uusin Azure PowerShell-komennoilla](../powershell-install-configure.md) .

>[AZURE.NOTE] Automaattinen korjaus on riippuvainen SQL Server IaaS Agent-tunniste. Nykyinen SQL virtuaalikoneen valikoiman kuvia Lisää tähän alanumeroon oletusarvoisesti. Lisätietoja on artikkelissa [SQL Server IaaS Agent-tunniste](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Asetukset

Seuraavassa taulukossa kuvataan vaihtoehdot, jotka voidaan määrittää automaattinen korjaaminen. Todellinen määritysvaiheet vaihtelevat sen mukaan, käytätkö Azure-portaalista tai Windows PowerShellin Azure-komentoja.

|Asetus|Mahdolliset arvot|Kuvaus|
|---|---|---|
|**Automaattinen korjaaminen**|Ottaa käyttöön/poistaa käytöstä (poissa käytöstä)|Ottaa käyttöön tai poistaa Azure virtuaalikoneen, automaattinen korjaaminen.|
|**Ylläpitoaikataulu**|Everyday, maanantai, tiistai, keskiviikko, torstai, perjantai, lauantai, sunnuntai|Lataaminen ja asentaminen Windows ja Microsoft SQL Server päivitykset virtuaalikoneen ajoitusta.|
|**Aloituspäivämäärä ylläpito**|0 – 24|Päivitä virtuaalikoneen paikallisen alkamisaika.|
|**Ylläpito ikkunan kesto**|30 180|Viimeistele lataaminen ja asentaminen päivitysten sallittu minuuttimäärä.|
|**Korjaustiedoston luokka**|Tärkeää|Lataa ja asenna päivitykset luokka.|

## <a name="configuration-in-the-portal"></a>Portaalin määrittäminen
Voit käyttää Azure portaalin määrittäminen automaattinen korjaaminen valmistelun aikana tai aiemmin VMs.

### <a name="new-vms"></a>Uusi VMs
Azure portaalin avulla voit määrittää automaattinen korjaaminen, kun luot uuden SQL Server Virtual Machine resurssien hallinnan käyttöönottomalli.

Valitse **SQL Server-asetukset** -sivu **päivitetään automaattisen**. Seuraavat Azure portaalin näyttökuvassa näkyy **SQL automaattinen korjaaminen** -sivu.

![SQL Azure-portaalissa automaattinen korjaaminen](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Kontekstin on valmis ohjeaiheessa [valmistelu SQL Server-virtual tietokoneeseen Azure-tietokannassa](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Olemassa olevan VMs
Valitse aiemmin luotu SQL Server-näennäiskoneiden SQL Server-virtuaalikoneen. Valitse **asetukset** -sivu **SQL Server-määritys** -osa.

![SQL automaattista korjausta aiemmin VMs varten](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Valitse **SQL Server-määritys** -sivu päivitetään osan automaattisen **Muokkaa** -painiketta.

![Määrittää olemassa olevan VMs SQL automaattinen korjaaminen](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Kun olet valmis, valitse Tallenna muutokset **SQL Server-määritys** -sivu alaosassa **OK** -painiketta.

Jos otat automaattinen korjaaminen ensimmäistä kertaa, Azure määrittää SQL Server IaaS Agent taustalla. Tänä aikana Azure portaalin eivät välttämättä tule näkyviin, että automaattinen korjaus on määritetty. Odota muutama minuutti edustaja, joka on asennettu, määritetty varten. Tämän jälkeen Azure portaalin kuvastaa käytetään uusia asetuksia.

>[AZURE.NOTE] Voit myös määrittää automaattinen korjaaminen mallin avulla. Lisätietoja on artikkelissa [Azure pikaopas mallin automaattinen korjaaminen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update).

## <a name="configuration-with-powershell"></a>Määritysten PowerShellin avulla

Valmistelun SQL-AM jälkeen voit määrittää automaattinen korjaaminen PowerShellin avulla.

Seuraavassa esimerkissä PowerShell käytetään määrittämistä aiemmin luotuun SQL Server-AM automaattinen korjaaminen. **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig** -komento määrittää automaattisia päivityksiä ylläpito uudessa ikkunassa.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

Seuraavassa taulukossa kuvataan käytännössä aikataulussa Azure AM tämän esimerkin mukaan:

|Parametri|Vaikutus|
|---|---|
|**: N DayOfWeek**|Korjaukset asennettu joka torstai.|
|**MaintenanceWindowStartingHour**|Aloita 11:00 Suomen päivitykset.|
|**MaintenanceWindowsDuration**|Korjaukset on oltava asennettuna enintään 120 minuuttia. Perusteella alkamisaika, ne on täytä 1:00 pm.|
|**PatchCategory**|Ainoa tämä parametri on **tärkeä**.|

Saattaa kulua useita minuutteja asennetaan ja määritetään SQL Server IaaS Agent.

Käytöstä automaattinen korjaaminen, suorita ilman saman komentosarja **-käyttöön** **AzureRM.Compute\New AzureVMSqlServerAutoPatchingConfig**parametri. Puuttuminen **– Ota käyttöön** parametrin signaalit komennon poistaminen käytöstä.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso tietoja käytettävissä automaatio muihin tehtäviin [SQL Server IaaS Agent-tunniste](virtual-machines-windows-sql-server-agent-extension.md).

Katso lisätietoja SQL Server-käyttöjärjestelmässä Azure VMs [Yhteenvedossa Azuren näennäiskoneiden SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
