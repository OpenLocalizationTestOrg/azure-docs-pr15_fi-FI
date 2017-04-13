<properties
    pageTitle="Varmuuskopiointi ja palauttaminen App palvelun sovellukset PowerShellin avulla"
    description="Lue, miten voit varmuuskopioida ja palauttaa Azure App Service-sovelluksen PowerShellin avulla"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Varmuuskopiointi ja palauttaminen App palvelun sovellukset PowerShellin avulla

> [AZURE.SELECTOR]
- [PowerShellin](app-service-powershell-backup.md)
- [REST-OHJELMOINTIRAJAPINNALLA](../app-service-web/websites-csm-backup.md)

Opettele käyttämään PowerShellin Azure varmuuskopioiminen ja palauttaminen [App palvelun sovellukset](https://azure.microsoft.com/services/app-service/web/). Web app varmuuskopiointia, vaatimukset ja rajoituksia, lue lisätietoja artikkelissa [varmuuskopioida Azure-sovelluksen palvelun verkkosovellukseen](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Edellytykset
PowerShellin avulla voit hallita sovelluksen varmuuskopiot, tarvitset seuraavat:

- **A SAS URL-Osoitetta** , joka sallii luku- ja kirjoitusoikeudet Azure-tallennustilan säilö. Kohdassa [tietoa SAS mallin](../storage/storage-dotnet-shared-access-signature-part-1.md) selitystä SAS URL-osoitteet. Artikkelissa [Azure PowerShellin Azure-tallennustilan kanssa](../storage/storage-powershell-guide-full.md) esimerkkejä PowerShellin Azure-tallennustilan hallinta.
- Jos haluat sekä koodiin tietokannan varmuuskopioiminen **A yhteysmerkkijonon** .

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Voit luoda käyttäminen web app varmuuskopion Cmdlet-komentoja SAS URL-osoite
PowerShellin avulla voidaan luoda SAS URL-osoite. Tässä on esimerkki siitä, miten luo sellainen, joka voidaan käyttää tässä artikkelissa kuvatut Cmdlet-komentoja.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Asenna PowerShellin Azure 1.3.2 tai uudempi

Artikkelissa [Azure PowerShellin Azure resurssien hallinnan](../powershell-install-configure.md) asentaminen ja käyttäminen PowerShellin Azure ohjeita.

## <a name="create-a-backup"></a>Luo varmuuskopio

Uusi AzureRmWebAppBackup cmdlet-komennon avulla voit luoda varmuuskopion verkkosovellukseen.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Tämä luo varmuuskopion automaattisesti nimellä. Jos haluat nimetä varmuuskopion, käytä BackupName valinnaisten parametrien.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

Voit lisätä tietokannan varmuuskopiointi-ensin Luo uusi AzureRmWebAppDatabaseBackupSetting cmdlet-komennolla tietokannan varmuuskopiointi asetus ja valitse Anna uusi AzureRmWebAppBackup cmdlet-komento tietokantojen-parametrissa asetusta. Tietokantojen parametrin hyväksyy matriisin tietokanta-asetukset, joilla voit varmuuskopioida useampaa kuin yhtä tietokantaa.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Hae varmuuskopiointi

Get-AzureRmWebAppBackupList cmdlet-komento palauttaa matriisin web-sovelluksen kaikki varmuuskopiot. Sinun on annettava nimi web-sovelluksen ja sen resurssiryhmä.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Jos haluat tietyn varmuuskopion, käytä Get-AzureRmWebAppBackup cmdlet-komento.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Voit myös pipe web app-objekti mihin tahansa asiasta varmuuskopion hallinnan cmdlet-komennot.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Automaattisen varmuuskopioinnin

Voit ajoittaa varmuuskopioinnin tapahtuvan automaattisesti tietyin väliajoin. Voit määrittää varmuuskopioinnin aikataulu, käyttämällä Muokkaa AzureRmWebAppBackupConfiguration cmdlet-komento. Tämä cmdlet-komento käyttää useita parametreja:

- **Nimi** - web-sovelluksen nimeä.
- **ResourceGroupName** - resurssiryhmä, joka sisältää web-sovelluksen nimi.
- **Paikka** - valinnainen. Web app-saapumisaikojen nimi.
- **StorageAccountUrl** - URL-Suojaussidosten voidaan tallentaa varmuuskopioista Azure-tallennustilan säilö varten.
- **FrequencyInterval** - numeerinen arvo, kuinka usein varmuuskopioista hyödynnettäviksi. On oltava positiivinen kokonaisluku.
- **FrequencyUnit** -, kuinka usein varmuuskopioista hyödynnettäviksi aikayksikkö. Vaihtoehdot ovat tunti ja päivä.
- **RetentionPeriodInDays** - kuinka monta päivää automaattisen varmuuskopioinnin on tallennettava ennen poistetaan automaattisesti.
- **Aloitusajan** - valinnainen. Aika, jolloin automaattisen varmuuskopioinnin alkaa. Varmuuskopioiden alkaa heti, jos kyseessä on null. DateTime on oltava.
- **Tietokantojen** - valinnainen. DatabaseBackupSettings matriisi varmuuskopiointi tietokantoja varten.
- **KeepAtLeastOneBackup** - valinnainen vaihtanut parametria. Anna tämä, jos yksi varmuuskopio aina tulisi säilyttää tallennustilan tilin, riippumatta siitä, miten kauan se.

Seuraavassa on esimerkki siitä, miten tämä cmdlet-komennolla.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Saat nykyisen varmuuskopioinnin aikataulu-Käytä komentosovelmaa Get-AzureRmWebAppBackupConfiguration. Tämä on hyvä muokkaamisen aikataulun, joka on jo määritetty.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Verkkosovellukseen palauttaminen varmuuskopiosta

Palauttaminen varmuuskopiosta verkkosovellukseen, käytä palauttaminen AzureRmWebAppBackup cmdlet-komento. Helpoin tapa käyttää cmdlet on varmuuskopion objektin putkien hakee Get-AzureRmWebAppBackup cmdlet-komennon tai Hae AzureRmWebAppBackupList cmdlet-komento.

Kun varmuuskopioinnin objektin, voit pipe se Palauta AzureRmWebAppBackup cmdlet-komento. Määritä ilmaisemaan, että haluat korvata koodiin sisällön varmuuskopion sisältöä korvaa valitsin-parametrin. Jos varmuuskopioinnin sisältää tietokantoja, kyseiset tietokannat palautetaan paikan päällä.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Seuraavassa on esimerkki käyttämisestä palauttaminen-AzureRmWebAppBackup määrittämällä kaikki parametrit.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Poista varmuuskopiointi

Jos haluat poistaa varmuuskopion, käytä Poista AzureRmWebAppBackup cmdlet-komento. Tämä poistaa varmuuskopioinnin tallennustilan-tililtä. Määritä sovelluksen nimen, sen resurssiryhmä ja haluat poistaa varmuuskopion tunnus.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Voit myös pipe varmuuskopion objektin siirtäminen Poista AzureRmWebAppBackup cmdlet-komento, poista se.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
