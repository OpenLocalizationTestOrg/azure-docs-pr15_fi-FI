<properties 
    pageTitle="Joukkotuonnin tietojen avulla SQL osiotaulukoita rinnakkainen | Microsoftin Azure" 
    description="Parallel Data joukkotuonnin avulla SQL osiotaulukoita" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016" 
    ms.author="bradsev" /> 

# <a name="parallel-bulk-data-import-using-sql-partition-tables"></a>Parallel Data joukkotuonnin avulla SQL osiotaulukoita

Tässä asiakirjassa kuvataan, miten rakentaa osioitu taulukoita nopea yhdensuuntainen irtotavarana tuotaessa tietoja SQL Server-tietokantaan. Suuri lastaus/siirtää tietoja SQL-tietokantaan tuominen SQL DB ja seuraavat kyselyt voidaan parantaa käyttämällä _osioitu taulukoita ja näkymiä_. 


## <a name="create-a-new-database-and-a-set-of-filegroups"></a>Luo uusi tietokanta ja joukko FileGroup-ryhmien

- [Luo uusi tietokanta](https://technet.microsoft.com/library/ms176061.aspx) (Jos ei ole)
- Lisää tietokanta, johon mahtuu osioitu fyysiset tiedostot tietokantaan FileGroup-ryhmien

  Huomautus: Voit tehdä tämän kanssa [Luoda tietokanta](https://technet.microsoft.com/library/ms176061.aspx) , jos uusi tai [Muuta TIETOKANTAA](https://msdn.microsoft.com/library/bb522682.aspx) , jos tietokanta on jo olemassa

- Lisää yhden tai useita tiedostoja kunkin tietokannan filegroup (tarvittaessa)

 > [AZURE.NOTE] Määrittää tiedostoryhmän kohde, joka sisältää tämän osion tiedot ja jossa filegroup-tietoja säilytetään fyysisen tietokannan tiedoston nimet.
 
Seuraava esimerkki luo uuden tietokannan ja kolme FileGroup-ryhmien kuin ensisijainen loki, ryhmiä, jotka sisältävät kussakin yhteen fyysiseen tiedostoon. Tietokannan tiedostot luodaan SQL Server Data oletuskansio määritykset SQL Server-esiintymään. Saat lisätietoja tiedoston oletussijainteja [Oletus ja nimeltä esiintymät SQL Server-tiedostojen sijainnit](https://msdn.microsoft.com/library/ms143547.aspx).

    DECLARE @data_path nvarchar(256);
    SET @data_path = (SELECT SUBSTRING(physical_name, 1, CHARINDEX(N'master.mdf', LOWER(physical_name)) - 1)
      FROM master.sys.master_files
      WHERE database_id = 1 AND file_id = 1);
    
    EXECUTE ('
        CREATE DATABASE <database_name>
        ON  PRIMARY 
        ( NAME = ''Primary'', FILENAME = ''' + @data_path + '<primary_file_name>.mdf'', SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_1] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_1>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_2] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name_2>.ndf'' , SIZE = 4096KB , FILEGROWTH = 1024KB ), 
        FILEGROUP [filegroup_3] 
        ( NAME = ''FileGroup1'', FILENAME = ''' + @data_path + '<file_name>.ndf'' , SIZE = 102400KB , FILEGROWTH = 10240KB ), 
        LOG ON 
        ( NAME = ''LogFileGroup'', FILENAME = ''' + @data_path + '<log_file_name>.ldf'' , SIZE = 1024KB , FILEGROWTH = 10%)
    ')
    
## <a name="create-a-partitioned-table"></a>Osioitu taulukon luominen

Luo osioitu taulukoita tietojen rakenteen yhdistetty edellisessä vaiheessa luodun tietokannan FileGroup-ryhmien perusteella. Kun tiedot on tuotu osioitu taulukoita irtotavarana, tietueet jaetaan kaikkien kesken FileGroup-ryhmien mukaan osamalli-, jäljempänä kuvatulla tavalla.

**Luo osiotaulukko täytyy:**

- [Create partition-funktio](https://msdn.microsoft.com/library/ms187802.aspx) , joka määrittää arvot tai rajat sisällytetään kunkin yksittäisen osiotaulukko esim, rajoittaa osioiden kuukausittain (joissakin\_päivämäärä ja aika\_kenttä) vuonna 2013:

        CREATE PARTITION FUNCTION <DatetimeFieldPFN>(<datetime_field>)  
        AS RANGE RIGHT FOR VALUES (
            '20130201', '20130301', '20130401',
            '20130501', '20130601', '20130701', '20130801',
            '20130901', '20131001', '20131101', '20131201' )

- [Luo osio järjestelmän](https://msdn.microsoft.com/library/ms179854.aspx) , joka yhdistää jokaisen osion partition-funktiota alueen fyysisen tiedostoryhmän, esim:

        CREATE PARTITION SCHEME <DatetimeFieldPScheme> AS  
        PARTITION <DatetimeFieldPFN> TO (
        <filegroup_1>, <filegroup_2>, <filegroup_3>, <filegroup_4>,
        <filegroup_5>, <filegroup_6>, <filegroup_7>, <filegroup_8>,
        <filegroup_9>, <filegroup_10>, <filegroup_11>, <filegroup_12> )

  Vihje: Tarkista voimassa kunkin osion funktio/suunnitelman mukaan alueet, suorittaa seuraavan kyselyn:

        SELECT psch.name as PartitionScheme,
            prng.value AS ParitionValue,
            prng.boundary_id AS BoundaryID
        FROM sys.partition_functions AS pfun
        INNER JOIN sys.partition_schemes psch ON pfun.function_id = psch.function_id
        INNER JOIN sys.partition_range_values prng ON prng.function_id=pfun.function_id
        WHERE pfun.name = <DatetimeFieldPFN>

- [Luo taulukko osioitu](https://msdn.microsoft.com/library/ms174979.aspx) (s) arviointiperusteeksi tietorakennetta ja määritä osio järjestelmän ja rajoitus voidaan osioida taulun, esim:

        CREATE TABLE <table_name> ( [include schema definition here] )
        ON <TablePScheme>(<partition_field>)

Lisätietoja on kohdassa [Luo osioida tauluja ja -indeksejä](https://msdn.microsoft.com/library/ms188730.aspx).


## <a name="bulk-import-the-data-for-each-individual-partition-table"></a>Joukkotuonti kunkin yksittäisen osiotaulukon tiedot

- Asiakas saa käyttää BCP, Lisää IRTOTAVARANA tai muita menetelmiä, kuten [SQL Serverin ohjattu siirtotoiminto](http://sqlazuremw.codeplex.com/). Annetussa esimerkissä käyttää BCP-menetelmää.

- Voit muuttaa tapahtuman kirjaamisen malli vähentää kuormitusta Logging esim BULK_LOGGED [Muuta tietokanta](https://msdn.microsoft.com/library/bb522682.aspx) :

        ALTER DATABASE <database_name> SET RECOVERY BULK_LOGGED

- Käynnistä tuo joukkotoiminnot rinnakkain tietojen latauksen suorittamiseen. Saat vinkkejä nopeuttaa hallintotasolla irtotavarana tuodaan suuri tietoja SQL Server-tietokantoihin, [1 TB alle 1 tunnin ladata](http://blogs.msdn.com/b/sqlcat/archive/2006/05/19/602142.aspx).

Seuraava PowerShell-komentosarja on esimerkki rinnakkainen tietojen latauksen avulla BCP.

    # Set database name, input data directory, and output log directory
    # This example loads comma-separated input data files
    # The example assumes the partitioned data files are named as <base_file_name>_<partition_number>.csv
    # Assumes the input data files include a header line. Loading starts at line number 2.

    $dbname = "<database_name>"
    $indir  = "<path_to_data_files>"
    $logdir = "<path_to_log_directory>"

    # Select authentication mode
    $sqlauth = 0
    
    # For SQL authentication, set the server and user credentials
    $sqlusr = "<user@server>"
    $server = "<tcp:serverdns>"
    $pass   = "<password>"

    # Set number of partitions per table - Should match the number of input data files per table
    $numofparts = <number_of_partitions>
       
    # Set table name to be loaded, basename of input data files, input format file, and number of partitions
    $tbname = "<table_name>"
    $basename = "<base_input_data_filename_no_extension>"
    $fmtfile = "<full_path_to_format_file>"
   
    # Create log directory if it does not exist
    New-Item -ErrorAction Ignore -ItemType directory -Path $logdir
      
    # BCP example using Windows authentication
    $ScriptBlock1 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -T -b 2500 -t "," -r \n
    }
    
    # BCP example using SQL authentication
    $ScriptBlock2 = {
       param($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $num, $sqlusr, $server, $pass)
       bcp ($dbname + ".." + $tbname) in ($indir + "\" + $basename + "_" + $num + ".csv") -o ($logdir + "\" + $tbname + "_" + $num + ".txt") -h "TABLOCK" -F 2 -C "RAW" -f ($fmtfile) -U $sqlusr -S $server -P $pass -b 2500 -t "," -r \n
    }
    
    # Background processing of all partitions
    for ($i=1; $i -le $numofparts; $i++)
    {
       Write-Output "Submit loading trip and fare partitions # $i"
       if ($sqlauth -eq 0) {
          # Use Windows authentication
          Start-Job -ScriptBlock $ScriptBlock1 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i)
       } 
       else {
          # Use SQL authentication
          Start-Job -ScriptBlock $ScriptBlock2 -Arg ($dbname, $tbname, $basename, $fmtfile, $indir, $logdir, $i, $sqlusr, $server, $pass)
       }
    }
    
    Get-Job
    
    # Optional - Wait till all jobs complete and report date and time
    date
    While (Get-Job -State "Running") { Start-Sleep 10 }
    date


## <a name="create-indexes-to-optimize-joins-and-query-performance"></a>Luo indeksejä liitokset ja kyselyn suorituskyvyn optimoiminen

- Jos purat mallinnus tiedot useista taulukoista, luo indeksit liitoksen avaimet liitoksen suorituskyvyn parantamiseksi.

- [Indeksien luominen](https://technet.microsoft.com/library/ms188783.aspx) (klusteroitu tai klusteroimattomiksi) kohdistus saman tiedostoryhmän kunkin osion esim:

        CREATE CLUSTERED INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)
tai,

        CREATE INDEX <table_idx> ON <table_name>( [include index columns here] )
        ON <TablePScheme>(<partition)field>)

 > [AZURE.NOTE] Voit luoda indeksejä ennen tietojen tuomista irtotavarana. Indeksin luominen, ennen kuin tuot irtotavarana hidastuvat tietojen lataamista.


## <a name="advanced-analytics-process-and-technology-in-action-example"></a>Analytics kokeneet prosessin ja teknologian toiminnon esimerkissä

Päästä päähän vaihe vaiheelta Esimerkki Cortana Analytics-prosessin avulla julkinen dataset-kohdassa [Cortana Analytics prosessi-toiminto: avulla SQL Server](machine-learning-data-science-process-sql-walkthrough.md).
 
