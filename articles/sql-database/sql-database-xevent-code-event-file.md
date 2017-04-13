<properties 
    pageTitle="SQL-tietokannan koodi XEvent tapahtumatiedoston | Microsoft Azure" 
    description="Sisältää PowerShell ja Transact-SQL kaksivaiheinen koodi-esimerkki, joka näyttää tapahtumatiedoston kohteen laajennettu tapahtuman Azure SQL-tietokantaan. Azuren tallennustila on pakollinen osa Tämä skenaario." 
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jhubbard" 
    editor="" 
    tags=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/23/2016" 
    ms.author="genemi"/>


# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Tapahtuman tiedostojen kohde koodi laajennettu tapahtumien SQL-tietokantaan

[AZURE.INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Haluat valmis koodin otoksen tehokkaat tavan sieppaus ja raportin laajennettu tapahtuman tietoja.


Microsoft SQL Server- [tapahtumatiedoston kohde](http://msdn.microsoft.com/library/ff878115.aspx) käytetään tallentamiseen tapahtuman tulostaa paikalliselle kiintolevylle tiedostoon. Mutta tällaisen tiedostot eivät ole käytettävissä Azure SQL-tietokantaan. Sen sijaan Käytämme Azure tallennuspalvelu tukemaan tapahtumatiedoston kohde.


Tässä ohjeartikkelissa esitellään kaksivaiheinen koodin otoksen:


- PowerShellin Azure-tallennustilan säilö luominen pilveen.

- Transact-SQL:
 - Jos haluat määrittää Azure-tallennustilan säiliön tapahtumatiedoston kohde.
 - Luo tapahtuma-istunnon aloittaminen ja niin edelleen.


## <a name="prerequisites"></a>Edellytykset


- Azure-tili ja tilauksen. Voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).


- Mikä tahansa tietokanta, voit luoda taulukon.
 - Halutessasi voit [luoda **AdventureWorksLT** esittelyn tietokannan](sql-database-get-started.md) minuutteina.


- SQL Server Management Studiossa (ssms.exe) Ihannetapauksessa sen kuukausittainen päivitys uusimmat. Voit ladata uusimman ssms.exe kohteesta:
 - [Lataa SQL Server Management Studiossa](http://msdn.microsoft.com/library/mt238290.aspx)aiheessa.
 - [Esityksen linkin Valitse Lataa.](http://go.microsoft.com/fwlink/?linkid=616025)


- Sinulla on oltava asennettuna [PowerShellin Azure moduulit](http://go.microsoft.com/?linkid=9811175) .
 - Moduulit sisältävät tehtäväkohtaisia komentoja, kuten - **Uusi AzureStorageAccount**.


## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Vaihe 1: PowerShell koodi Azure-tallennustilan säilö


Tämä PowerShell on kaksivaiheinen koodin otosten vaiheen 1.

Komentosarjan ja komennot, kun edellisen sillä voi suorittaa ja on rerunnable siistimiseen.



1. Liitä PowerShell-komentosarjaa tavallista tekstieditoria, kuten Notepad.exe ja Tallenna tiedosto, jonka tunniste **.ps1**komentosarja.

2. Aloita PowerShell ise: järjestelmänvalvojana.

3. Kirjoita komentokehotteeseen<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>ja paina sitten Enter-näppäintä.

4. Avaa PowerShell ise: **.ps1** -tiedosto. Komentosarjan suorittaminen

5. Komentosarjan ensin uusi ikkuna, jossa voit kirjautua Azure.
 - Jos ilman toiminnan istunnon suorittamalla komentosarja, sinun on kätevä voi kommentoinnista **Lisää AzureAccount** -komennon.


![PowerShell ise:, Azure-moduulia asennettu, valmis suorittamaan komentosarja.][30_powershell_ise]


&nbsp;


```
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


&nbsp;


Ota seuraavat seikat huomioon muutamia nimetty arvoja, jotka PowerShell-komentosarjaa tulostaa, kun se päättyy. Muokattava kyseiset arvot Transact-SQL-komentosarja, joka seuraa vaiheeseen 2.


## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Vaihe 2: Transact-SQL-koodi, joka käyttää Azure-tallennustilan säilö


- Tässä esimerkissä koodi vaiheen 1 suoritit PowerShell-komentosarjaa luominen Azure-tallennustilan säilö.
- Seuraavaksi vaiheessa 2 seuraava Transact-SQL-komentosarja on käytettävä säilö.


Komentosarjan ja komennot, kun edellisen sillä voi suorittaa ja on rerunnable siistimiseen.


PowerShell-komentosarjaa tulostaa muutaman nimettyjen arvojen, kun se on päättynyt. Muokkaa Transact-SQL-komentosarjan käyttäminen kyseiset arvot. Etsi **TODO** Transact-SQL-komentosarja Etsi Muokkaa pisteitä.


1. Avaa SQL Server Management Studiossa (ssms.exe).

2. Muodosta yhteys Azure SQL-tietokanta-tietokantaan.

3. Avaa uusi kyselyruutu napsauttamalla.

4. Liitä seuraava Transact-SQL-komentosarja kysely-ruudussa.

5. Etsi kaikki **TODO** komentosarja ja tee tarvittavat muokkaukset.

6. Tallenna ja suorita sitten komentosarja.


&nbsp;


> [AZURE.WARNING] Edellisen PowerShell-komentosarjaa luoma SAS avainarvon voi aloittaa "?" (kysymysmerkki). Kun käytät Suojaussidosten avaimen seuraava T-SQL-komentosarja, sinun täytyy *poistaa alussa "?"*. Muussa tapauksessa vaivaa voi estää suojaus.


&nbsp;


```
---- TODO: First, run the PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


&nbsp;


Jos kohde ei liitä, kun suoritat, Lopeta ja käynnistä istunto tapahtuma uudelleen:


```
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


&nbsp;


## <a name="output"></a>Tulos


Kun Transact-SQL-komentosarja on valmis, valitse solu **event_data_XML** sarakkeen otsikon alapuolella. Yksi **<event>** osa näkyy, jossa näkyy yksi UPDATE-komento.

Tässä on yksi **<event>** elementti, joka on luotu testauksen aikana:


&nbsp;


```
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```

&nbsp;


Edellinen Transact-SQL-komentosarja lukea event_file järjestelmän-toiminto avulla:

- [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)


SELITYS laajennettu tapahtumien tiedot tarkastelu lisäasetusten on saatavilla kohdassa:

- [Laajennettu kohteen tiedot laajennettu tapahtumien tarkasteleminen](http://msdn.microsoft.com/library/mt752502.aspx)

&nbsp;


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Muuntaminen voidaan suorittaa SQL Serverin koodi-Esimerkki


Oletetaan, että haluat suorittaa edellisen Transact-SQL-Esimerkki Microsoft SQL Server.


- Yksinkertaisuuden haluat korvata kokonaan Azure-tallennustilan säiliön käyttäminen yksinkertaisen tiedoston, kuten **C:\myeventdata.xel**. Tiedoston kirjoitettavat tietokone, jossa SQL Server paikalliselle kiintolevylle.


- Sinun on kaikenlaista Transact-SQL-lauseita ei **luoda PERUSMUODON avain** ja **Luoda TUNNISTETIEDON**.


- **Luoda tapahtuman istunnon** -lauseessa **Lisää kohde** -lauseen, korvaat Http määritetty arvo tehdyt **Tiedostonimi =** koko polku merkkijonolla, kuten **C:\myfile.xel**.
 - Azuren tallennustilaan tiliä ei tarvitse osallistua.


## <a name="more-information"></a>Lisätietoja


Saat lisätietoja asiakkaat ja Azure tallennuspalvelu säilöjä:

- [Opi käyttämään .NET-Blob-objektien tallennustilaan](../storage/storage-dotnet-how-to-use-blobs.md)
- [Nimeämisestä ja viittaava säilöjen BLOB-objektit ja metatiedot](http://msdn.microsoft.com/library/azure/dd135715.aspx)
- [Pääkansio säilö käsitteleminen](http://msdn.microsoft.com/library/azure/ee395424.aspx)
- [Harjoitus 1: Luominen Azure säilö tallennettu käyttöoikeuskäytäntö ja jaetun access-allekirjoitus](http://msdn.microsoft.com/library/dn466430.aspx)
    - [Harjoitus 2: Luo jaettu käyttö allekirjoituksella SQL Server-tunnistetiedot](http://msdn.microsoft.com/library/dn466435.aspx)




<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

