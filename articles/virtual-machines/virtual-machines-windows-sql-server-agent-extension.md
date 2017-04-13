<properties
    pageTitle="SQL Server Agent-laajennus SQL Server VMs (Resurssienhallinta) | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan SQL Server agent-tunniste, joka automatisoi tietyn SQL Server-hallintatehtäviä hallinta. Näitä ovat automaattisen varmuuskopioinnin, automaattinen korjaaminen ja Azure avaimen säilö integrointi. Tässä ohjeaiheessa käyttää resurssien hallinnan käyttöönotto-tila."
    services="virtual-machines-windows"
    documentationCenter=""
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
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Server-agentti laajennus SQL Server VMs (resurssien hallinta)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-server-agent-extension.md)
- [Perinteinen](virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS agentti tunniste (SQLIaaSExtension) suoritetaan Azuren näennäiskoneiden hallintatehtävien automatisointiin. Tämä artikkeli sisältää yleiskatsaus services tukemat tunniste sekä ohjeet asennuksen, tila ja poistamiseen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönottomalli. Tämän artikkelin perinteinen versio on artikkelissa [SQL Server Agent Extension SQL Server VMs Classic](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Tuetut palvelut

SQL Server IaaS Agent-tunniste tukee seuraavat hallintatehtävät:

| Hallinta-ominaisuus | Kuvaus |
|---------------------|-------------------------------|
| **SQL automaattinen varmuuskopiointi** | Automatisoi kaikkien tietokantojen varmuuskopioiden ajoituksessa SQL Server AM oletusesiintymää. Lisätietoja on artikkelissa [automaattisen varmuuskopioinnin SQL Server-Azuren näennäiskoneiden (resurssien hallinta)](virtual-machines-windows-sql-automated-backup.md).|
| **SQL automaattinen korjaaminen** | Määrittää aikana oman AM päivitykset voidaan hallita, jotta vältyt päivitykset havainnollistamiseen piikin esiintymistä ylläpito-ikkuna. Lisätietoja on artikkelissa [automaattisen korjataan SQL Server-Azuren näennäiskoneiden (resurssien hallinta)](virtual-machines-windows-sql-automated-patching.md).|
| **Azure avaimen säilöön-integrointi** | Voit asentaa ja Azure avaimen säilö määrittämistä SQL Server-AM automaattisesti. Lisätietoja on artikkelissa [Määrittäminen Azure avaimen säilö integrointi SQL Server-Azure VMs (resurssien hallinta)](virtual-machines-windows-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Edellytykset

SQL Server IaaS Agent-tunniste käyttämisestä oman AM vaatimukset:

**Käyttöjärjestelmä**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-versiot**:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell**:

- [Lataa ja määritä uusimman Azure PowerShell-komennot](../powershell-install-configure.md)

## <a name="installation"></a>Asennus

SQL Server IaaS Agent-tunniste asennetaan automaattisesti, kun jokin SQL Serverin virtuaalikoneen valikoiman kuvia valmistelu.

Jos luot vain OS Windows Server-virtual machine, voit asentaa laajentaminen manuaalisesti **Määrittäminen AzureVMSqlServerExtension** PowerShell-cmdlet-komennolla. Esimerkiksi seuraava komento asentaa tiedostotunniste vain OS Windows Server AM ja nimet "SQLIaaSExtension".

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

Jos päivität SQL IaaS Agent-tunniste uusimman version, sinun on käynnistettävä virtuaalikoneen tunnisteen päivityksen jälkeen.

>[AZURE.NOTE] Jos asennat SQL Server IaaS Agent-tunniste Windows Server-AM manuaalisesti, käytä ja hallitse sen ominaisuuksia PowerShell-komennoilla. Portaalin käyttöliittymän on käytettävissä vain SQL Server-valikoiman kuvia.

## <a name="status"></a>Tila

Varmista, että laajennus on asennettu yksi tapa on agent-tilan tarkasteleminen Azure-portaalissa. Valitse **kaikki asetukset** virtuaalikoneen sivu ja valitse **tunnisteet**. Raportissa pitäisi näkyä luettelossa **SQLIaaSExtension** tunniste.

![SQL Server IaaS agentti tunniste Azure-portaalissa](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Voit käyttää myös **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet-komennon.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Edellisen komennon vahvistaa agentti on asennettu ja yleisiä tilan tietoja. Saat myös tilansa tietojen automaattinen varmuuskopiointi- ja korjaus ja seuraavat komennot.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Poisto   

Azure-portaalissa voit poistaa tunnisteen napsauttamalla kolmea pistettä virtuaalikoneen ominaisuuksien **tunnisteet** -sivu. Valitse **Poista**.

![Poista SQL Server IaaS Agent-tunniste Azure-portaalissa](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Voit käyttää myös **Poista AzureRmVMSqlServerExtension** Powershell-cmdlet-komennon.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Seuraavat vaiheet

Aloita jollakin tunnisteen tukemat palvelut. Lisätietoja on ohjeaiheissa on tämän artikkelin [Tuetut palvelut](#supported-services) -osassa.

Saat lisätietoja suorittamisesta SQL Server-Azuren näennäiskoneiden [SQL Server-Azuren näennäiskoneiden yleiskatsaus](virtual-machines-windows-sql-server-iaas-overview.md).
