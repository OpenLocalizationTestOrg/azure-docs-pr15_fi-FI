<properties
    pageTitle="Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon PowerShell-toiminnolla"
    description="Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon PowerShell-toiminnolla"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="08/15/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="archive-an-azure-sql-database-to-a-bacpac-file-by-using-powershell"></a>Arkistoida Azure SQL-tietokantaan BACPAC tiedostoon PowerShell-toiminnolla

> [AZURE.SELECTOR]
- [Azure portal](sql-database-export.md)
- [PowerShellin](sql-database-export-powershell.md)


Tässä artikkelissa on ohjeita arkistointiin Azure SQL-tietokantaan (Azure-Blob-objektien tallennustilaan tallennettuja) [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedostoon PowerShellin avulla.

Kun haluat luoda arkiston Azure SQL-tietokantaan, voit viedä tietokannan rakenteen ja tiedot BACPAC tiedostoon. BACPAC-tiedosto on vain jonka tunniste on .bacpac ZIP-tiedoston. BACPAC tiedoston myöhemmin voidaan tallentaa Azure-Blob-säiliö tai paikallinen tallennus paikallisen sijainnissa. Se myös voidaan tuoda takaisin Azure SQL-tietokanta tai SQL Server-asennuksen paikallisen.

**Huomioon otettavia seikkoja**

- Arkiston yhtenäistämistä muulla tavoin on varmistettava, joka ei ole kirjoittaa tehtävän tapahtuu viennin aikana tai [muulla tavoin yhdenmukaisia kopioi](sql-database-copy.md) Azure SQL-tietokantaan vietävän.
- Azure-Blob-säiliö arkistoida BACPAC tiedoston enimmäiskoko on 200 gt. Voit arkistoida suurempi BACPAC tiedoston paikalliseen tallennussijaintiin, käyttämällä [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) komentorivi-apuohjelmaa. Tämän apuohjelman mukana Visual Studio ja SQL Server. Voit myös [ladata](https://msdn.microsoft.com/library/mt204009.aspx) SQL Server Data Tools saat tämän apuohjelman uusin versio.
- Arkistoinnin Azure premium säilöön BACPAC-tiedoston avulla, ei tueta.
- Jos vientitoiminnon ylittää 20 tuntia, se voidaan peruuttaa. Voit parantaa suorituskykyä viennin aikana seuraavasti:
 - Tilapäisesti suurentaa service-taso.
 - Lakkaa kaikki lukeminen ja kirjoittaminen tehtävän viennin aikana.
 - Muut kuin null-arvojen etsiminen kaikki suurten taulukoiden käyttäminen [ryhmitellyn indeksin](https://msdn.microsoft.com/library/ms190457.aspx) . Liitetty indeksit ilman vienti saattaa epäonnistua, jos kestää yli 6 – 12 tuntia. Tämä johtuu siitä Vie-palvelu on tehtävä taulukon tarkistus yrittää vie koko taulukko. Suorita **DBCC SHOW_STATISTICS** ja varmista, että *RANGE_HI_KEY* ei ole null ja sen arvo on hyvä jakauma on hyvä keino selvittää, jos taulukoissa on optimoitu Vie. Lisätietoja on artikkelissa [DBCC SHOW_STATISTICS](https://msdn.microsoft.com/library/ms174384.aspx).

> [AZURE.NOTE] BACPACs ei ole tarkoitettu voidaan käyttää varmuuskopiointi ja palauttaminen toimintoja. Azure SQL-tietokanta luo automaattisesti jokaisen käyttäjän tietokannan varmuuskopiot. Lisätietoja on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md).

Tässä artikkelissa suorittamiseen tarvitset seuraavat:

- Azure tilaus.
- Azure SQL-tietokantaan.
- [Vakio Azure-tallennustilan tilin](../storage/storage-create-storage-account.md), voit tallentaa BACPAC vakio tallennustilan blob säilön kanssa.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]




## <a name="export-your-database"></a>Vie tietokantaan

[Uusi AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx) cmdlet-komento lähettää Vie tietokantaan-pyynnön-palveluun. Tietokannan koon mukaan vientitoiminnon voi kestää kauan.

> [AZURE.IMPORTANT] Voit varmistaa muulla tavoin yhdenmukaisia BACPAC tiedoston olisi ensimmäisen [tietokannan kopion luominen](sql-database-copy-powershell.md)ja vie tiedot sitten tietokannan kopion.


     $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password


## <a name="monitor-the-progress-of-the-export-operation"></a>Vientitoiminnon etenemisen seuranta

Kun olet suorittanut [uusi AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt603644(v=azure.300\).aspx), voit tarkistaa pyynnön tilaa suorittamalla [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx). Käytössä tämän heti, kun pyynnön yleensä palauttaa **tila: aktiivisuustila**. Kun näet **tila: onnistui** vienti on valmis.


    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="export-sql-database-example"></a>Vie SQL-tietokanta-Esimerkki

Seuraavassa esimerkissä Vie aiemmin luotuun SQL-tietokantaan BACPAC ja näyttää vientitoiminnon tilan tarkistaminen.

Esimerkki suorittamiseen on muutama muuttujat, sinun täytyy tietokanta ja tallennustilaa tilissäsi tiettyjen arvojen korvaaminen. Siirry [Azure portal](https://portal.azure.com)-tallennustilan tilin tallennustilan tilin nimi, blob-säilö nimen ja arvon. Löydät avain valitsemalla **pikanäppäinten** tallennustilan tili-sivu.

Korvaa seuraavat `VARIABLE-VALUES` tietyn Azure-resursseissa arvoilla. Tietokannan nimi on aiemmin luotu tietokanta haluat viedä.



    $subscriptionId = "YOUR AZURE SUBSCRIPTION ID"

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    # Database to export
    $DatabaseName = "DATABASE-NAME"
    $ResourceGroupName = "RESOURCE-GROUP-NAME"
    $ServerName = "SERVER-NAME"
    $serverAdmin = "ADMIN-NAME"
    $serverPassword = "ADMIN-PASSWORD" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $creds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

    # Generate a unique filename for the BACPAC
    $bacpacFilename = $DatabaseName + (Get-Date).ToString("yyyyMMddHHmm") + ".bacpac"

    # Storage account info for the BACPAC
    $BaseStorageUri = "https://STORAGE-NAME.blob.core.windows.net/BLOB-CONTAINER-NAME/"
    $BacpacUri = $BaseStorageUri + $bacpacFilename
    $StorageKeytype = "StorageAccessKey"
    $StorageKey = "YOUR STORAGE KEY"

    $exportRequest = New-AzureRmSqlDatabaseExport –ResourceGroupName $ResourceGroupName –ServerName $ServerName `
       –DatabaseName $DatabaseName –StorageKeytype $StorageKeytype –StorageKey $StorageKey -StorageUri $BacpacUri `
       –AdministratorLogin $creds.UserName –AdministratorLoginPassword $creds.Password
    $exportRequest

    # Check status of the export
    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $exportRequest.OperationStatusLink



## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Azure SQL-tietokannan tuominen PowerShellin avulla on artikkelissa [tuoda BACPAC, PowerShellin avulla](sql-database-import-powershell.md).


## <a name="additional-resources"></a>Lisäresursseja

- [Uusi AzureRmSqlDatabaseExport] (https://msdn.microsoft.com/library/azure/mt707796(v=azure.300\).aspx)
- [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx)