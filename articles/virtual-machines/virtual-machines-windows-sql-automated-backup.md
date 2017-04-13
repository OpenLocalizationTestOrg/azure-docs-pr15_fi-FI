<properties
    pageTitle="Automaattisen varmuuskopioinnin näennäiskoneiden SQL Server (Resurssienhallinta) | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan SQL Serveriä-Azuren näennäiskoneiden resurssien hallinnan automaattisen varmuuskopioinnin-ominaisuus. "
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
    ms.date="07/14/2016"
    ms.author="jroth" />

# <a name="automated-backup-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automaattinen varmuuskopiointi SQL Server Azure-Virtuaalikoneissa (resurssien hallinta)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-automated-backup.md)
- [Perinteinen](virtual-machines-windows-classic-sql-automated-backup.md)

Automaattisen varmuuskopioinnin määrittää automaattisesti kaikki aiemmin luoduissa että uusissa tietokannat Azure-AM, jossa on SQL Server 2014 Standard tai yrityksen [Microsoft Azure hallitun varmuuskopion](https://msdn.microsoft.com/library/dn449496.aspx) . Näin voit määrittää säännöllisesti tietokannan varmuuskopioita, jotka käyttävät kestävät Azure-Blob-objektien tallennustilaan. Automaattisen varmuuskopioinnin määräytyy [SQL Server IaaS Agent-tunniste](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönoton mallia. Tämän artikkelin perinteinen versio on artikkelissa [Automaattisen varmuuskopioinnin SQL Server Azure näennäiskoneiden Classic-ohjelmassa](virtual-machines-windows-classic-sql-automated-backup.md).

## <a name="prerequisites"></a>Edellytykset

Automaattisen varmuuskopioinnin käyttöön Ota huomioon seuraavat vaatimukset:

**Käyttöjärjestelmä**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL Serverin versio/version**:

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

**Tietokannan määritykset**:

- Kohde-tietokantoja on käytettävä koko palautusmalli

**Azure PowerShell**:

- [Asenna uusimmat Azure PowerShell-komennot](../powershell-install-configure.md) , jos aiot määrittää automaattisen varmuuskopioinnin PowerShellin avulla.

>[AZURE.NOTE] Automaattisen varmuuskopioinnin on riippuvainen SQL Server IaaS Agent-tunniste. Nykyinen SQL virtuaalikoneen valikoiman kuvia Lisää tähän alanumeroon oletusarvoisesti. Lisätietoja on artikkelissa [SQL Server IaaS Agent-tunniste](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Asetukset

Seuraavassa taulukossa kuvataan vaihtoehdot, jotka voidaan ottaa käyttöön automaattisen varmuuskopioinnin. Todellinen määritysvaiheet vaihtelevat sen mukaan, käytätkö Azure-portaalista tai Windows PowerShellin Azure-komentoja.

|Asetus|Alue (oletus)|Kuvaus|
|---|---|---|
|**Automaattisen varmuuskopioinnin**|Ottaa käyttöön/poistaa käytöstä (poissa käytöstä)|Ottaa käyttöön tai poistaa automaattisen varmuuskopioinnin, jossa on SQL Server 2014 Standard tai yrityksen Azure-AM.|
|**Säilytysaika**|1 – 30 päivää (30 päivää)|Voit säilyttää varmuuskopion päivien määrän.|
|**Tallennustilan tilin**|Azure-tallennustilan tilin (luo määritetyn AM tallennustilan-tili)|Azure-tallennustilan tilin automaattinen varmuuskopiot tallennetaan Blob-objektien tallennustilaan. Säilön luodaan tähän sijaintiin kaikki varmuuskopion tallentamista varten. Varmuuskopiotiedoston nimeämiskäytäntöä sisältää päivämäärän, kellonajan ja tietokoneen nimi.|
|**Salaus**|Ottaa käyttöön/poistaa käytöstä (poissa käytöstä)|Ottaa käyttöön tai poistaa salausta. Kun salaus on käytössä, palauta varmuuskopio sertifikaatit sijaitsevat määritetyn tallennustilan tilin käyttämällä samaa nimeämiskäytäntöä saman automaticbackup säilö. Jos salasana muuttuu, uutta varmennetta luotu salasana, mutta vanha varmenne säilyy palauttaa edellisen varmuuskopiot.|
|**Salasana**|Salasana-teksti (ei mitään)|Salausavaimet salasanan. Tämä on vain pakollinen salaus on käytössä. Jos haluat palauttaa salattuja varmuuskopion, on oltava oikeaa salasanaa ja liittyvä varmenne, jota käytettiin varmuuskopioinnin otettiin aikaan.|

## <a name="configuration-in-the-portal"></a>Portaalin määrittäminen
Voit määrittää automaattisen varmuuskopioinnin aikana valmistelu tai aiemmin VMs Azure-portaalissa.

### <a name="new-vms"></a>Uusi VMs
Azure-portaalin avulla voit määrittää automaattisen varmuuskopioinnin, kun luot uuden SQL Server 2014 Virtual koneen resurssien hallinnan käyttöönottomalli.

Valitse **automaattisen varmuuskopioinnin** **SQL Serverin asetukset** -sivu. Seuraavat Azure portaalin näyttökuvassa näkyy **SQL automaattisen varmuuskopioinnin** -sivu.

![Azure-portaalissa SQL automaattisen varmuuskopioinnin määrittäminen](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Kontekstin on valmis ohjeaiheessa [valmistelu SQL Server-virtual tietokoneeseen Azure-tietokannassa](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Olemassa olevan VMs
Valitse aiemmin luotu SQL Server-näennäiskoneiden SQL Server-virtuaalikoneen. Valitse **asetukset** -sivu **SQL Server-määritys** -osa.

![Olemassa olevan VMs SQL automaattinen varmuuskopiointi](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Valitse **SQL Server-määritys** -sivu automaattisen varmuuskopioinnin-osassa **Muokkaa** -painiketta.

![Olemassa olevan VMs SQL automaattisen varmuuskopioinnin määrittäminen](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Kun olet valmis, valitse Tallenna muutokset **SQL Server-määritys** -sivu alaosassa **OK** -painiketta.

Jos haluat ottaa käyttöön automaattisen varmuuskopioinnin ensimmäistä kertaa, Azure määrittää SQL Server IaaS Agent taustalla. Tänä aikana Azure portaalin ei ehkä näy automaattisen varmuuskopioinnin on määritetty. Odota muutama minuutti edustaja, joka on asennettu, määritetty varten. Tämän jälkeen Azure portaalin vaikuttavat käytetään uusia asetuksia.

>[AZURE.NOTE] Voit myös määrittää automaattisen varmuuskopioinnin mallin avulla. Lisätietoja on artikkelissa [Azure pikaopas automaattinen varmuuskopiointi mallia](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update).

## <a name="configuration-with-powershell"></a>Määritysten PowerShellin avulla

Valmistelun SQL-AM jälkeen automaattisen varmuuskopioinnin määrittäminen PowerShellin avulla.

PowerShellin seuraavassa esimerkissä automaattisen varmuuskopioinnin on määritetty aiemmin luotuun SQL Server 2014 AM. **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** -komento määrittää automaattisen varmuuskopioinnin tallentaa varmuuskopioita virtuaalikoneen liitetyn Azure-tallennustilan tilin asetukset. Nämä varmuuskopiot säilytetään 10 päivää. **Määritä AzureRmVMSqlServerExtension** -komento päivittää määritetyn Azure AM nämä asetukset.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Saattaa kulua useita minuutteja asennetaan ja määritetään SQL Server IaaS Agent.

Salauksen käyttöön Muokkaa Edellinen komentosarja välittää **EnableEncryption** parametrin sekä **CertificatePassword** -parametrin salasanan (suojattu merkkijonon). Seuraavaa komentosarjaa otetaan käyttöön automaattisen varmuuskopioinnin asetukset edellisessä esimerkissä, ja lisää salausta.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Automaattisen varmuuskopioinnin käytöstä, suorita sama komentosarja ilman **-käyttöön** parametrin **AzureRM.Compute\New AzureVMSqlServerAutoBackupConfig** -komento. Puuttuminen **– Ota käyttöön** parametrin signaalit komennon poistaminen käytöstä. Asennuksen kanssa voi kestää useita minuutteja automaattisen varmuuskopioinnin poistaminen käytöstä.

>[AZURE.NOTE] SQL Server IaaS Agent poistaminen ei poista aiemmin määritetyn automaattisen varmuuskopioinnin asetuksia. Poista automaattinen varmuuskopiointi käytöstä ennen virustentorjuntaohjelmiston poistamisesta käytöstä tai SQL Server IaaS Agent asennuksen poistaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Automaattisen varmuuskopioinnin määrittää hallitun varmuuskopiointi Azure VMs. Siten on tärkeää tarkistettava [hallitun varmuuskopiointi ohjeissa](https://msdn.microsoft.com/library/dn449496.aspx) ymmärtämään toiminta ja vaikutukset.

Voit etsiä lisätietoja Varmuuskopiointi ja palauttaminen seuraavassa aiheessa on SQL Server Azure VMs ohjeissa: [Varmuuskopiointi ja palauttaminen SQL Serverin Azuren näennäiskoneiden](virtual-machines-windows-sql-backup-recovery.md).

Katso tietoja käytettävissä automaatio muihin tehtäviin [SQL Server IaaS Agent-tunniste](virtual-machines-windows-sql-server-agent-extension.md).

Katso lisätietoja SQL Server-käyttöjärjestelmässä Azure VMs [Yhteenvedossa Azuren näennäiskoneiden SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).
