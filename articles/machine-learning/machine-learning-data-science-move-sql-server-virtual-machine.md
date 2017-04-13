<properties 
    pageTitle="Tietojen siirtäminen SQL Server Azure virtual-laitteeseen | Azure" 
    description="Siirrä tiedot tasainen tiedostoista tai paikallisen SQL Serveristä SQL Server Azure AM." 
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
    ms.date="09/14/2016" 
    ms.author="bradsev" /> 

# <a name="move-data-to-sql-server-on-an-azure-virtual-machine"></a>Tietojen siirtäminen SQL Server Azure virtual-laitteeseen

Tässä ohjeaiheessa kuvataan tietojen siirtämistä tasainen-tiedostoista (CSV- tai TSV muotoilut) tai paikallinen SQL Serveristä SQL Server Azure virtuaalikoneen-asetukset. Näiden tietojen siirtämistä pilveen tehtävät ovat ryhmän tietojen tiede yhteydessä.

Katso artikkelista, johon ympärille asetukset tietojen siirtäminen tietokoneeseen Learning Azure SQL-tietokantaan, [Siirrä tiedot varten Azure koneen Learning Azure SQL-tietokantaan](machine-learning-data-science-move-sql-azure.md).

**Valikon** alla linkkejä ohjeaiheisiin, joissa kerrotaan ingest tietojen tuominen kohde muiden ympäristöjen, jossa tiedot voidaan tallentaa ja käsitellä aikana-työryhmän tiedot tiede prosessi (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


Seuraavassa taulukossa on yhteenveto tietojen siirtäminen SQL Server Azure virtuaalikoneen-asetukset.

<b>LÄHDE</b> |<b>KOHDE: SQL Server-Azure AM</b> |
------------------ |-------------------- |
<b>Tietuetiedostoon</b> |1. <a href="#insert-tables-bcp">komentorivin joukkona Kopioi apuohjelman (BCP)</a><br> 2. <a href="#insert-tables-bulkquery">joukkosähköposti Lisää SQL-kysely</a><br> 3. <a href="#sql-builtin-utilities">Graafinen valmiita apuohjelmia, SQL Server</a>
<b>Paikallisen SQL Serverin kanssa</b> | 1. <a href="#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard">Ota käyttöön Microsoft Azure AM ohjattu SQL Server-tietokantaan</a><br> 2. <a href="#export-flat-file">Tietuetiedostoon vieminen</a><br> 3. <a href="#sql-migration">siirto ohjattu SQL-tietokanta</a> <br> 4. <a href="#sql-backup">tietokannan takaisin ylös ja palauttaminen</a><br>

Huomaa, että tämä asiakirja oletetaan, että SQL-komentoja suoritetaan SQL Server Management Studiossa tai Visual Studio tietokannan Resurssienhallinnasta.

> [AZURE.TIP] Vaihtoehtoisesti voit [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) luominen ja ajoittaminen, jotka siirtyvät tiedot SQL Server-AM Azure-putkijohto. Lisätietoja on artikkelissa [kopioida tiedot ja Azure Data Factory (kopioi tehtävä)](../data-factory/data-factory-data-movement-activities.md).


## <a name="prereqs"></a>Edellytykset
Tässä opetusohjelmassa oletetaan, että:

* On **Azure tilauksen**. Jos sinulla ei ole tilaus, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
* **Azure-tallennustilan tilin**. Voit käyttää Azure-tallennustilan tilin tietojen tallentamista tässä opetusohjelmassa. Jos sinulla ei ole Azure-tallennustilan tilin, on artikkelissa [Luo tallennustilan tili](../storage/storage-create-storage-account.md#create-a-storage-account) . Sen jälkeen, kun olet luonut tallennustilan tilin, sinun on käyttää tallennustilaan tilin Key-tunnuksen. Katso [näkymässä, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* Valmistellun **SQL Server Azure AM**. Ohjeita on artikkelissa [Azure SQL Serverin virtual-koneen IPython muistikirjan palvelimeksi varten kehittyneen analyysin määrittäminen](machine-learning-data-science-setup-sql-server-virtual-machine.md).
* Asennettu ja määritetty **PowerShellin Azure** paikallisesti. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).


## <a name="filesource_to_sqlonazurevm"></a>Tietojen siirtämistä tietuetiedostoon lähteestä SQL Server Azure-AM käyttöön

Jos tiedot ovat tietuetiedostoon (järjestettyinä rivi tai sarake-muodossa), se voidaan siirtää SQL Server AM Azure-kautta seuraavista tavoista:

1. [Komentorivin joukkona Kopioi apuohjelman (BCP)](#insert-tables-bcp) 
2. [Bulk Lisää SQL-kysely](#insert-tables-bulkquery)
3. [SQL Server (Tuo/Vie, SSIS) graafinen valmiin apuohjelmat](#sql-builtin-utilities)


### <a name="insert-tables-bcp"></a>Komentorivin joukkona Kopioi apuohjelman (BCP)

BCP on asennettu SQL Serverin kanssa komentorivin apuohjelma, ja se on nopein tapa, jos haluat siirtää tietoja. Se toimii kaikissa kaikki kolme SQL Serverin muunnelmat (paikallisen SQL Serverin ja SQL Azure SQL Server-AM Azure-). 

> [AZURE.NOTE]**Missä tietoni olisi varten BCP?**  
> Samalla, kun sitä ei tarvita, tiedostot, jotka sisältävät lähdetiedot sijaitsevat samaan tietokoneeseen kuin kohde SQL server on sallii nopeampaa siirrot (verkon nopeuden ja paikalliseen levyasemaan IO nopeus). Voit siirtää tasainen tiedostot, jotka sisältävät tietoja koneen johon SQL Server on asennettu eri-tiedoston kopioiminen työkaluilla, kuten [AZCopy](../storage/storage-use-azcopy.md), [Azure-tallennustilan Explorerin](http://storageexplorer.com/) tai Windowsin kopioi ja liitä kautta RDP Remote Desktop Protocol ().

1. Varmista, että tietokanta ja taulukot luodaan kohde SQL Server-tietokantaan. Esimerkki siitä, miten haluat, että käytät `Create Database` ja `Create Table` komentoja:

        CREATE DATABASE <database_name>
    
        CREATE TABLE <tablename>
        (
            <columnname1> <datatype> <constraint>,
            <columnname2> <datatype> <constraint>,
            <columnname3> <datatype> <constraint>
        ) 
    
2. Luo muoto-tiedostoa, joka kuvaa taulukon rakenteen kirjoittamalla seuraava komento komentorivin koneen johon bcp on asennettu.

    `bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n` 

3. Lisää tiedot tietokantaan bcp-komennon avulla seuraavasti. Tämä on Työskentele missä komentorivillä oletetaan, että SQL Server on asennettuna samaan tietokoneeseen:

    `bcp dbname..tablename in datafilename.tsv -f exportformatfilename.xml -S servername\sqlinstancename -U username -P password -b block_size_to_move_in_single_attemp -t \t -r \n`

> **Lisää BCP optimointi** Tutustu seuraavan artikkelin [ohjeita optimointi joukkona tuo](https://technet.microsoft.com/library/ms177445%28v=sql.105%29.aspx) optimoi tällaisten Lisää.


### <a name="insert-tables-bulkquery-parallel"></a>Lisää parallelizing nopeuttaa tietojen siirtoa varten

Jos siirrät tiedot on suuri, voit nopeuttaa toimintoja suoritetaan samanaikaisesti useita BCP komentoja PowerShell-komentosarjaa rinnakkain.

> [AZURE.NOTE]**Big datasta nieltynä** Optimoi tietojen lataaminen suuria ja hyvin suurten tietojoukkojen osion useita FileGroup-ryhmien ja osion taulukoiden loogisten ja fyysisten tietokannan taulukkoon. Lisätietoja luomisesta ja osion taulukoiden tietojen lataaminen artikkelissa [Rinnakkain kuormituksen SQL-osion taulukot](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).


Alla otoksen PowerShell-komentosarjaa osoittaa rinnakkain Lisää bcp avulla:
    
    $NO_OF_PARALLEL_JOBS=2

     Set-ExecutionPolicy RemoteSigned #set execution policy for the script to execute
     # Define what each job does
       $ScriptBlock = {
           param($partitionnumber)

           #Explictly using SQL username password
           bcp database..tablename in datafile_path.csv -F 2 -f format_file_path.xml -U username@servername -S tcp:servername -P password -b block_size_to_move_in_single_attempt -t "," -r \n -o path_to_outputfile.$partitionnumber.txt

            #Trusted connection w.o username password (if you are using windows auth and are signed in with that credentials)
            #bcp database..tablename in datafile_path.csv -o path_to_outputfile.$partitionnumber.txt -h "TABLOCK" -F 2 -f format_file_path.xml  -T -b block_size_to_move_in_single_attempt -t "," -r \n 
      }
    

    # Background processing of all partitions
    for ($i=1; $i -le $NO_OF_PARALLEL_JOBS; $i++)
    {
      Write-Debug "Submit loading partition # $i"
      Start-Job $ScriptBlock -Arg $i      
    }
    

    # Wait for it all to complete
    While (Get-Job -State "Running")
    {
      Start-Sleep 10
      Get-Job
    }
    
    # Getting the information back from the jobs
    Get-Job | Receive-Job
    Set-ExecutionPolicy Restricted #reset the execution policy


### <a name="insert-tables-bulkquery"></a>Bulk Lisää SQL-kysely

[Bulk Lisää SQL-kyselyn](https://msdn.microsoft.com/library/ms188365) avulla voidaan tuoda tietojen perusteella rivi tai sarake-tiedostoista (Tuetut tiedostotyypit käydään[Valmisteleminen tiedot joukkona vieminen ja tuominen (SQL Server)](https://msdn.microsoft.com/library/ms188609))-tietokantaan aihe. 

Seuraavassa on joitakin otoksen komennot joukkona Lisää kuin alla ovat:  

1. Tietojen analysointi ja määritä haluamasi mukautetut asetukset ennen tuontia, varmista, että SQL Server-tietokantaan oletetaan, että mitään erityistä kenttien, kuten päivämäärien samassa muodossa. Tässä on esimerkki siitä, miten voit määrittää päivämäärämuodon vuoden päivää (Jos tiedoissa on päivämäärän vuoden päivää-muodossa):

        SET DATEFORMAT ymd; 
    
2. Tuo tiedot joukkona tuo lauseiden käyttämisestä:

        BULK INSERT <tablename>
        FROM    
        '<datafilename>'
        WITH 
        (
        FirstRow=2,
        FIELDTERMINATOR =',', --this should be column separator in your data
        ROWTERMINATOR ='\n'   --this should be the row separator in your data
        )
      

### <a name="sql-builtin-utilities"></a>SQL Server valmiin apuohjelmat

Voit käyttää SQL Server integroinnit Services (SSIS) tietojen tuominen SQL Server-AM-Azure tasainen tiedostosta. SSIS sisältyy studio ympäristöissä. Lisätietoja on artikkelissa [Integration Services (SSIS) ja Studio ympäristöissä](https://technet.microsoft.com/library/ms140028.aspx):

- Lisätietoja SQL Server Data Tools on artikkelissa [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/tools.aspx)  
- Lisätietoja Ohjattu tuominen ja vieminen on artikkelissa [SQL Server tuominen ja vieminen-toiminnossa](https://msdn.microsoft.com/library/ms141209.aspx)


## <a name="sqlonprem_to_sqlonazurevm"></a>Tietojen siirtämistä paikallisen SQL Serveristä SQL Server-Azure AM

Voit käyttää myös seuraavat siirron strategiat:

1. [Ottaa käyttöön Microsoft Azure AM ohjattu SQL Server-tietokantaan](#deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard)
2. [Vie Tietuetiedostoon](#export-flat-file) 
3. [Siirron ohjattu SQL-tietokanta](#sql-migration)
4. [Tietokannan takaisin ylös ja palauttaminen](#sql-backup)

Olemme kuvataan kaikkien näiden alla:

### <a name="deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Ottaa käyttöön Microsoft Azure AM ohjattu SQL Server-tietokantaan

**Ota käyttöön Microsoft Azure AM ohjattu SQL Server-tietokantaan** on yksinkertainen ja suositellut tavan siirtyä tietoja paikallisen SQL Server-esiintymän Azure-AM SQL Server. Katso lisätietoja kohdasta sekä keskustelun muita vaihtoehtoja, [artikkelista Azure-AM SQL Server-tietokannan](../virtual-machines/virtual-machines-windows-migrate-sql.md).

### <a name="export-flat-file"></a>Vie Tietuetiedostoon

Eri tavat avulla voidaan edelleen paikallisen SQL Serveristä tiedot viedään joukkosähköposti suorittimessa [joukkotuonti ja vienti, tietojen (SQL Server)](https://msdn.microsoft.com/library/ms175937.aspx) -artikkelissa. Tämän asiakirjan kattavat joukkona kopioi ohjelma (BCP) esimerkkinä. Kun tiedot on tuotu tietuetiedostoon, se voidaan tuoda toiseen SQL server-tietokantaan joukkona Tuo. 

1. Tietojen vieminen tiedostoon bcp apuohjelmalla seuraavasti paikallisen SQL Serverin kanssa

    `bcp dbname..tablename out datafile.tsv -S  servername\sqlinstancename -T -t \t -t \n -c`

2. Luo tietokanta ja taulukko SQL Server AM Azure käyttämisestä `create database` ja `create table` taulukon rakenteen viedä vaiheessa 1.

3. Luo muoto-tiedosto, joka kuvaa taulukon rakenne on viedä ja tuoda tiedot. [Luo tiedostomuodossa (SQL Server)](https://msdn.microsoft.com/library/ms191516.aspx)on artikkelissa tietoja tiedostomuodossa.

    Muotoile tiedoston luonti BCP, kun suoritat SQL Server-tietokoneesta 

        bcp dbname..tablename format nul -c -x -f exportformatfilename.xml -S servername\sqlinstance -T -t \t -r \n 

    Muotoile tiedoston luonti suoritettaessa BCP etäyhteyden SQL-palvelimelle 

        bcp dbname..tablename format nul -c -x -f  exportformatfilename.xml  -U username@servername.database.windows.net -S tcp:servername -P password  --t \t -r \n
        
    
4. Siirry tiedot tasainen tiedostojen SQL Serveriä käyttämällä jonkin osan [Siirtäminen tietoja tiedoston lähteestä](#filesource_to_sqlonazurevm) kuvatuilla tavoilla.

### <a name="sql-migration"></a>Siirron ohjattu SQL-tietokanta

[SQL Server-tietokannan siirron luominen](http://sqlazuremw.codeplex.com/) on käyttäjäystävällinen tapa tietojen siirtäminen SQL server kahdesti. Sen avulla käyttäjä voi yhdistää tiedot rakenteen lähteet ja kohdetaulukoiden välille, valitse saraketyypit ja eri toimintoja. Tiedostossa käytetään kattaa kopiota samalla kertaa (BCP). SQL-tietokannan siirto ohjatun toiminnon aloitusnäyttö näyttökuva näkyy alla.  

![Ohjattu SQL Server-siirto][2]

### <a name="sql-backup"></a>Tietokannan takaisin ylös ja palauttaminen

SQL Server tukee: 

1. [Tietokannan takaisin ylös ja palauttaa toiminnot](https://msdn.microsoft.com/library/ms187048.aspx) (molemmat paikallisen tiedoston tai bacpac vieminen blob) ja [Taso sovelluksia](https://msdn.microsoft.com/library/ee210546.aspx) (joko bacpac). 
2. Mahdollisuus luoda suoraan SQL Server VMs Azure, joka sisältää kopioidun tietokannan tai kopioi aiemmin luotuun SQL Azure-tietokantaan. Lisätietoja on artikkelissa [Ohjattu tietokannan kopion](https://msdn.microsoft.com/library/ms188664.aspx). 

Näyttökuva tietokannan takaisin ylös/Palauta SQL Server Management Studiossa-vaihtoehto näkyy alla.

![SQL Server tuo-työkalu][1]

## <a name="resources"></a>Resurssit

[Tietokannan siirtäminen SQL Server Azure AM](../virtual-machines/virtual-machines-windows-migrate-sql.md)

[SQL Server-Azuren näennäiskoneiden yleiskatsaus](../virtual-machines/virtual-machines-windows-sql-server-iaas-overview.md)

[1]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/sqlserver_builtin_utilities.png
[2]: ./media/machine-learning-data-science-move-sql-server-virtual-machine/database_migration_wizard.png
