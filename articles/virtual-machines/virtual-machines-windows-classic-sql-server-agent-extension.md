<properties
    pageTitle="SQL Server-agentti laajennus SQL Server VMs (perinteinen) | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan SQL Server agent-tunniste, joka automatisoi tietyn SQL Server-hallintatehtäviä hallinta. Näitä ovat automaattisen varmuuskopioinnin, automaattinen korjaaminen ja Azure avaimen säilö integrointi. Tässä ohjeaiheessa käyttää perinteinen käyttöönotto-tilaa."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/27/2016"
    ms.author="jroth"/>

# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Server-agentti laajennus SQL Server VMs (perinteinen)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-server-agent-extension.md)
- [Perinteinen](virtual-machines-windows-classic-sql-server-agent-extension.md)

SQL Server IaaS agentti tunniste (SQLIaaSAgent) suoritetaan Azuren näennäiskoneiden hallintatehtävien automatisointiin. Tämä artikkeli sisältää yleiskatsaus services tukemat tunniste sekä ohjeet asennuksen, tila ja poistamiseen.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Tämän artikkelin Resurssienhallinta-versio on artikkelissa [SQL Server Agent Extension for SQL Serverin VMs resurssien hallinta](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Tuetut palvelut

SQL Server IaaS Agent-tunniste tukee seuraavat hallintatehtävät:

| Hallinta-ominaisuus | Kuvaus |
|---------------------|-------------------------------|
| **SQL automaattinen varmuuskopiointi** | Automatisoi kaikkien tietokantojen varmuuskopioiden ajoituksessa SQL Server AM oletusesiintymää. Lisätietoja on artikkelissa [automaattisen varmuuskopioinnin SQL Server-Azuren näennäiskoneiden (perinteinen)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL automaattinen korjaaminen** | Määrittää aikana oman AM päivitykset voidaan hallita, jotta vältyt päivitykset havainnollistamiseen piikin esiintymistä ylläpito-ikkuna. Lisätietoja on artikkelissa [automaattinen korjaaminen SQL Server-Azuren näennäiskoneiden (perinteinen)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Azure avaimen säilöön-integrointi** | Voit asentaa ja Azure avaimen säilö määrittämistä SQL Server-AM automaattisesti. Lisätietoja on artikkelissa [Määrittäminen Azure avaimen säilö integrointi SQL Server-Azure VMs (perinteinen)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## <a name="prerequisites"></a>Edellytykset

SQL Server IaaS Agent-tunniste käyttämisestä oman AM vaatimukset:

### <a name="operating-system"></a>Käyttöjärjestelmä:

- Windows Server 2012
- Windows Server 2012 R2

### <a name="sql-server-versions"></a>SQL Server-versiot:

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:

[Lataa ja määritä uusimman Azure PowerShell-komennoilla](../powershell-install-configure.md).

Käynnistä Windows PowerShell ja yhdistä se Azure tilauksen **Lisää AzureAccount** -komennolla.

    Add-AzureAccount

Jos sinulla on useita tilauksia, joka sisältää kohde-tilaus **Valitse AzureSubscription** avulla perinteinen AM.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Tässä vaiheessa saat luettelo perinteinen näennäiskoneiden ja niiden liitetyn Palvelunimet **Get-AzureVM** -komennolla.

    Get-AzureVM

## <a name="installation"></a>Asennus

Perinteinen VMs, sinun on käytettävä PowerShell SQL Server IaaS Agent-laajennuksen asentaminen ja määrittäminen siihen liittyviä palveluita. Asenna laajennus **Määrittäminen AzureVMSqlServerExtension** PowerShell-cmdlet-komennon avulla. Esimerkiksi seuraava komento asentaa tunnisteen Windows Server AM (perinteinen) ja nimen "SQLIaaSExtension".

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Jos päivität SQL IaaS Agent-tunniste uusimman version, sinun on käynnistettävä virtuaalikoneen tunnisteen päivityksen jälkeen.

>[AZURE.NOTE] Perinteinen näennäiskoneiden ei ole vaihtoehto asennetaan ja määritetään portaalin kautta SQL IaaS Agent-tunniste.

## <a name="status"></a>Tila

Varmista, että laajennus on asennettu yksi tapa on agent-tilan tarkasteleminen Azure-portaalissa. Valitse **kaikki asetukset** virtuaalikoneen sivu ja valitse **tunnisteet**. Raportissa pitäisi näkyä luettelossa **SQLIaaSAgent** tunniste.

![SQL Server IaaS agentti tunniste Azure-portaalissa](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Voit käyttää myös **Get-AzureVMSqlServerExtension** Azure Powershell-cmdlet-komennon.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Poisto   

Azure-portaalissa voit poistaa tunnisteen napsauttamalla kolmea pistettä virtuaalikoneen ominaisuuksien **tunnisteet** -sivu. Valitse **Poista**.

![Poista SQL Server IaaS Agent-tunniste Azure-portaalissa](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Voit käyttää myös **Poista AzureVMSqlServerExtension** Powershell-cmdlet-komennon.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Seuraavat vaiheet

Aloita jollakin tunnisteen tukemat palvelut. Lisätietoja on ohjeaiheissa on tämän artikkelin [Tuetut palvelut](#supported-services) -osassa.

Saat lisätietoja suorittamisesta SQL Server-Azuren näennäiskoneiden [SQL Server-Azuren näennäiskoneiden yleiskatsaus](virtual-machines-windows-sql-server-iaas-overview.md).
