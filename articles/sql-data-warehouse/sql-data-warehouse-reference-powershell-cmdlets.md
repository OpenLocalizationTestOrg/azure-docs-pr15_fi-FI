<properties
   pageTitle="Azure SQL-tietovarasto PowerShell cmdlet-komennot"
   description="Etsi Azure SQL-tietovarasto sekä keskeyttäminen ja jatkaminen tietokannan yläreunan PowerShellin cmdlet-komennot."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShellin cmdlet-komennot ja REST API SQL-tietovarasto varten

Useita SQL-tietovarasto hallintatehtäviä voi hallita Azure PowerShellin cmdlet-komennot tai REST API.  Alla on joitakin esimerkkejä käyttämisestä PowerShell-komennoilla voit automatisoida SQL-tietovarasto Yleiset toiminnot.  Jotkin hyvä REST-esimerkkejä on artikkelissa [Hallitse skaalattavuus muiden kanssa][].

> [AZURE.NOTE]  Jotta voit käyttää PowerShellin Azure SQL-tietovarasto, sinun on PowerShellin Azure versio 1.0.3 tai suurempi.  Voit tarkistaa versiosi suorittamalla **Get-moduuli - ListAvailable-nimen Azure**.  [Microsoftin WWW-ympäristö asennusohjelma][]voidaan asentaa uusin versio.  Katso lisätietoja uusimman version asentamisesta, [miten voit asentaa ja määrittää PowerShellin Azure][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Aloita Azure PowerShellin cmdlet-komennot

1. Avaa Windows PowerShell. 
2. PowerShell-kehotteessa Suorita nämä komennot kirjautua sisään Azure Resurssienhallinta ja valitse tilauksen.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Keskeytä SQL Data Warehouse Esimerkki

Siirrä osoitin tietokanta nimeltä "Database02" palvelimessa nimeltä "Palvelin01."  Palvelin on Azure resurssiryhmä, nimeltä "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Muunnelma, tässä esimerkissä piiput [Keskeytys AzureRmSqlDatabase][]haetut objekti.  Tämän vuoksi tietokanta on keskeytetty. Lopulliseksi-komennolla näyttää tulokset.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Käynnistä SQL Data Warehouse Esimerkki

Jatkaa tietokannan nimeltä "Database02" palvelimessa nimeltä "Palvelin01." Palvelimen sisältyy resurssiryhmän nimeltä "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Muunnelma, tässä esimerkissä hakee tietokanta nimeltä "Database02" "Palvelin01", joka sisältyy nimeltä "ResourceGroup1." resurssiryhmä-palvelimesta Se piiput [Ansioluettelon AzureRmSqlDatabase][]haetut objekti.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Huomaa, että jos palvelin on foo.database.windows.net, Määritä "tapahtuman nimenä" PowerShell-cmdlet--palvelin.

## <a name="frequently-used-powershell-cmdlets"></a>Usein käytetyt PowerShellin cmdlet-komennot

Nämä PowerShellin cmdlet-komennot käytetään usein Azure SQL-tietovarasto.

- [Hae AzureRmSqlDatabase][]
- [Hae AzureRmSqlDeletedDatabaseBackup][]
- [Hae AzureRmSqlDatabaseRestorePoints][]
- [Uusi AzureRmSqlDatabase][]
- [Poista AzureRmSqlDatabase][]
- [Palauta AzureRmSqlDatabase][] 
- [Jatka AzureRmSqlDatabase][]
- [Valitse AzureRmSubscription][]
- [Määritä AzureRmSqlDatabase][]
- [Keskeyttää AzureRmSqlDatabase][]

## <a name="next-steps"></a>Seuraavat vaiheet
Lisää PowerShell-esimerkkejä on seuraavissa artikkeleissa:

- [Luo SQL-tietovarasto PowerShellin avulla][]
- [Tietokannan palauttaminen][]

Luettelo kaikki tehtävät, joita voidaan automatisoida PowerShellin avulla on artikkelissa [Azure SQL-tietokannan cmdlet-komennot][].  Tehtävät, joita voidaan automatisoida muiden kanssa luettelo on artikkelissa [Azure SQL-tietokantoja toimintoja][].

<!--Image references-->

<!--Article references-->
[Asentaminen ja määrittäminen PowerShellin Azure]: ./powershell-install-configure.md
[Luo SQL-tietovarasto PowerShellin avulla]: ./sql-data-warehouse-get-started-provision-powershell.md
[Tietokannan palauttaminen]: ./sql-data-warehouse-restore-database-powershell.md
[Hallitse skaalattavuus muiden kanssa]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL-tietokannan cmdlet-komennot]: https://msdn.microsoft.com/library/mt574084.aspx
[Azure SQL-tietokantoja toiminnot]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Hae AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Hae AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Hae AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Uusi AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Poista AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Palauta AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Jatka AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Valitse AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Määritä AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Keskeyttää AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoftin WWW-ympäristö asennusohjelma]: https://aka.ms/webpi-azps
