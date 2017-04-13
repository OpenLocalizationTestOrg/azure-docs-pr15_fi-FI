<properties
    pageTitle="Voit luoda Azure SQL-tietokantaan käyttämällä PowerShell BACPAC-tiedoston tuominen | Microsoft Azure"
    description="Voit luoda Azure SQL-tietokantaan käyttämällä PowerShell BACPAC-tiedoston tuominen"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/31/2016"
    ms.author="sstein"/>

# <a name="import-a-bacpac-file-to-create-an-azure-sql-database-by-using-powershell"></a>Voit luoda Azure SQL-tietokantaan käyttämällä PowerShell BACPAC-tiedoston tuominen

**Yhteen-tietokantaan**

> [AZURE.SELECTOR]
- [Azure portal](sql-database-import.md)
- [PowerShellin](sql-database-import-powershell.md)
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)

Tässä artikkelissa on ohjeet luominen tuomalla [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) tiedoston PowerShellin Azure SQL-tietokantaan.

Tietokanta on luotu BACPAC-tiedostosta (.bacpac) tuoduista Azuren tallennustilaan-blob-säilö. Jos sinulla ei ole BACPAC tiedoston Azuren tallennustilaan, katso [arkisto Azure SQL-tietokantaan BACPAC tiedostoon PowerShell-toiminnolla](sql-database-export-powershell.md). Jos sinulla on jo BACPAC tiedosto, joka ei ole Azuren tallennustilaan, [Käytä AzCopy helposti ladata sen Azure-tallennustilan tilin](../storage/storage-use-azcopy.md#blob-upload).

> [AZURE.NOTE] Azure SQL-tietokanta luo automaattisesti ja ylläpitää jokaisen käyttäjän tietokantaan, voit palauttaa varmuuskopiot. Lisätietoja on artikkelissa [SQL-tietokantaan automaattisen varmuuskopioinnin](sql-database-automated-backups.md).


Tuo SQL-tietokantaan, tarvitset seuraavat:

- Azure tilaus. Jos tarvitset Azure tilauksen yksinkertaisesti Valitse **Maksuttoman kokeiluversion** tämän sivun yläosassa ja valitse palaa valmis tässä artikkelissa.
- Tietokannan BACPAC tiedosto, jonka haluat tuoda. BACPAC on oltava [Azure-tallennustilan tilin](../storage/storage-create-storage-account.md) -blob-säilö.



[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="set-up-the-variables-for-your-environment"></a>Muuttujat-ympäristön määrittäminen

Muutama muuttujia on tarvittaessa annetaan esimerkkiarvoja korvaaminen tietokantasi ja tallennustilaa tilisi määritetyistä arvoista.

Palvelimen nimi on oltava palvelimeen, joka on tällä hetkellä valitun edellisessä vaiheessa tilaus. Olisi haluat luoda tietokannan palvelimeen. Tietokannan tuominen suoraan joustavasti resurssivarantoon ei tueta. Mutta tuomaan ensin yksittäiseen tietokantaan ja siirrä tietokanta resurssivarantoon.

Tietokannan nimi on haluamasi uuden tietokannan nimi.

    $ResourceGroupName = "resource group name"
    $ServerName = "server name"
    $DatabaseName = "database name"


Seuraavat muuttujat ovat tallennustilan tililtä oman BACPAC sijainti. Siirry [Azure portal](https://portal.azure.com)-tallennustilan tilin nämä arvot. Löydät ensisijainen pikanäppäin valitsemalla **kaikki asetukset** ja sitten **näppäimiä** tallennustilan-tili-sivu.

Blob on aiemmin luotu BACPAC-tiedosto, jonka haluat luoda tietokannan nimi. Haluat lisätä .bacpac-tunniste.

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"


Käynnissä [Get-tunnistetieto] (https://msdn.microsoft.com/library/azure/hh849815(v=azure.300\).aspx) cmdlet-komento, näkyviin tulee ikkuna, jossa kysytään käyttäjänimeä ja salasanaa. Kirjoita SQL-tietokantapalvelinta ($ServerName edellä), ja ei käyttäjänimen ja salasanan Azure-tilin järjestelmänvalvoja-käyttäjätunnuksella ja -salasanalla.

    $credential = Get-Credential


## <a name="import-the-database"></a>Tuo tietokannan

Tämä komento lähettää tuo tietokannan pyynnön, palveluun. Tietokannan koon mukaan tuontitoiminnon voi kestää kauan.

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000


## <a name="monitor-the-progress-of-the-operation"></a>Näytön toiminnon edistyminen

Kun olet suorittanut [uusi AzureRmSqlDatabaseImport] (https://msdn.microsoft.com/library/azure/mt707793(v=azure.300\).aspx), voit tarkistaa pyynnön tilaa suorittamalla [Get-AzureRmSqlDatabaseImportExportStatus] (https://msdn.microsoft.com/library/azure/mt707794(v=azure.300\).aspx).

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="sql-database-powershell-import-script"></a>SQL-tietokannan PowerShell tuo komentosarja


    $ResourceGroupName = "resourceGroupName"
    $ServerName = "servername"
    $DatabaseName = "databasename"

    $StorageName = "storageaccountname"
    $StorageKeyType = "StorageAccessKey"
    $StorageUri = "http://$StorageName.blob.core.windows.net/containerName/filename.bacpac"
    $StorageKey = "primaryaccesskey"

    $credential = Get-Credential

    $importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $ResourceGroupName –ServerName $ServerName –DatabaseName $DatabaseName –StorageKeytype $StorageKeyType –StorageKey $StorageKey -StorageUri $StorageUri –AdministratorLogin $credential.UserName –AdministratorLoginPassword $credential.Password –Edition Standard –ServiceObjectiveName S0 -DatabaseMaxSizeBytes 50000

    Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink



## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja yhdistäminen ja kyselyjen tuodut SQL-tietokanta on artikkelissa [yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja otoksen T-SQL-kyselyn](sql-database-connect-query-ssms.md)
