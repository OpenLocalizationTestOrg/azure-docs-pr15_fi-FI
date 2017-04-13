<properties
    pageTitle="Automaattisen varmuuskopioinnin näennäiskoneiden SQL Server (perinteinen) | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan SQL Serveriä-Azuren näennäiskoneiden resurssien hallinnan automaattisen varmuuskopioinnin-ominaisuus. "
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

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-classic"></a>Automaattisen varmuuskopioinnin SQL Server Azure näennäiskoneiden (perinteinen)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-automated-backup.md)
- [Perinteinen](virtual-machines-windows-classic-sql-automated-backup.md)

Automaattisen varmuuskopioinnin määrittää automaattisesti kaikki aiemmin luoduissa että uusissa tietokannat Azure-AM, jossa on SQL Server 2014 Standard tai yrityksen [Microsoft Azure hallitun varmuuskopion](https://msdn.microsoft.com/library/dn449496.aspx) . Näin voit määrittää säännöllisesti tietokannan varmuuskopioita, jotka käyttävät kestävät Azure-Blob-objektien tallennustilaan. Automaattisen varmuuskopioinnin määräytyy [SQL Server IaaS Agent-tunniste](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Tämän artikkelin Resurssienhallinta-versio on artikkelissa [Automaattisen varmuuskopioinnin SQL Server Azure näennäiskoneiden resurssien hallinta](virtual-machines-windows-sql-automated-backup.md).

## <a name="prerequisites"></a>Edellytykset

Automaattisen varmuuskopioinnin käyttöön Ota huomioon seuraavat vaatimukset:

**Käyttöjärjestelmä**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Serverin versio/version**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

>[AZURE.NOTE] SQL Server 2016 ei vielä tueta automaattisen varmuuskopioinnin.

**Tietokannan määritykset**:

- Kohde-tietokantoja on käytettävä koko palautusmalli.

**Azure PowerShell**:

- [Asenna uusimmat Azure PowerShell-komennoilla](../powershell-install-configure.md).

**SQL Server IaaS tunniste**:

- [Asenna SQL Server IaaS-laajennus](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Asetukset

Seuraavassa taulukossa kuvataan vaihtoehdot, jotka voidaan ottaa käyttöön automaattisen varmuuskopioinnin. Perinteinen VMs, sinun on käytettävä PowerShell asetusten määrittäminen.

|Asetus|Alue (oletus)|Kuvaus|
|---|---|---|
|**Automaattisen varmuuskopioinnin**|Ottaa käyttöön/poistaa käytöstä (poissa käytöstä)|Ottaa käyttöön tai poistaa automaattisen varmuuskopioinnin, jossa on SQL Server 2014 Standard tai yrityksen Azure-AM.|
|**Säilytysaika**|1 – 30 päivää (30 päivää)|Voit säilyttää varmuuskopion päivien määrän.|
|**Tallennustilan tilin**|Azure-tallennustilan tilin (luo määritetyn AM tallennustilan-tili)|Azure-tallennustilan tilin automaattinen varmuuskopiot tallennetaan Blob-objektien tallennustilaan. Säilön luodaan tähän sijaintiin kaikki varmuuskopion tallentamista varten. Varmuuskopiotiedoston nimeämiskäytäntöä sisältää päivämäärän, kellonajan ja tietokoneen nimi.|
|**Salaus**|Ottaa käyttöön/poistaa käytöstä (poissa käytöstä)|Ottaa käyttöön tai poistaa salausta. Kun salaus on käytössä, palauta varmuuskopio sertifikaatit sijaitsevat määritetyn tallennustilan tilin käyttämällä samaa nimeämiskäytäntöä saman automaticbackup säilö. Jos salasana muuttuu, uutta varmennetta luotu salasana, mutta vanha varmenne säilyy palauttaa edellisen varmuuskopiot.|
|**Salasana**|Salasana-teksti (ei mitään)|Salausavaimet salasanan. Tämä on vain pakollinen salaus on käytössä. Jos haluat palauttaa salattuja varmuuskopion, on oltava oikeaa salasanaa ja liittyvä varmenne, jota käytettiin varmuuskopioinnin otettiin aikaan.|

## <a name="configuration-with-powershell"></a>Määritysten PowerShellin avulla

PowerShellin seuraavassa esimerkissä automaattisen varmuuskopioinnin on määritetty aiemmin luotuun SQL Server 2014 AM. **Uusi AzureVMSqlServerAutoBackupConfig** -komento määrittää automaattisen varmuuskopioinnin tallentaa varmuuskopioita $storageaccount muuttujan määrittämää Azure-tallennustilan tilin asetukset. Nämä varmuuskopiot säilytetään 10 päivää. **Määritä AzureVMSqlServerExtension** -komento päivittää määritetyn Azure AM nämä asetukset.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Saattaa kulua useita minuutteja asennetaan ja määritetään SQL Server IaaS Agent.

Salauksen käyttöön Muokkaa Edellinen komentosarja välittää EnableEncryption parametrin sekä CertificatePassword-parametrin salasanan (suojattu merkkijonon). Seuraavaa komentosarjaa otetaan käyttöön automaattisen varmuuskopioinnin asetukset edellisessä esimerkissä, ja lisää salausta.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Automaattisen varmuuskopioinnin käytöstä, suorita sama komentosarja ilman **-käyttöön** **Uusi AzureVMSqlServerAutoBackupConfig**parametri. Asennuksen kanssa voi kestää useita minuutteja automaattisen varmuuskopioinnin poistaminen käytöstä.

>[AZURE.NOTE] Poistaminen käytöstä ja SQL Server IaaS Agent asennuksen poistaminen ei poista aiemmin määritetyn hallitun varmuuskopiointi-asetukset. Poista automaattinen varmuuskopiointi käytöstä ennen virustentorjuntaohjelmiston poistamisesta käytöstä tai SQL Server IaaS Agent asennuksen poistaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Automaattisen varmuuskopioinnin määrittää hallitun varmuuskopiointi Azure VMs. Siten on tärkeää tarkistettava [hallitun varmuuskopiointi ohjeissa](https://msdn.microsoft.com/library/dn449496.aspx) ymmärtämään toiminta ja vaikutukset.

Voit etsiä lisätietoja Varmuuskopiointi ja palauttaminen seuraavassa aiheessa on SQL Server Azure VMs ohjeissa: [Varmuuskopiointi ja palauttaminen SQL Serverin Azuren näennäiskoneiden](virtual-machines-windows-sql-backup-recovery.md).

Katso tietoja käytettävissä automaatio muihin tehtäviin [SQL Server IaaS Agent-tunniste](virtual-machines-windows-classic-sql-server-agent-extension.md).

Katso lisätietoja SQL Server-käyttöjärjestelmässä Azure VMs [Yhteenvedossa Azuren näennäiskoneiden SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
