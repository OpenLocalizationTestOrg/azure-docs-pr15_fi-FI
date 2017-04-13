<properties 
    pageTitle="PowerShellin Azure SQL-tietokantaan palvelun taso ja suorituskyvyn tason muuttaminen | Microsoft Azure" 
    description="Muuta palvelutaso ja suorituskyvyn tasoon Azure SQL-tietokantaan esitetään, kuinka voit skaalata SQL-tietokantaan ylös tai alas PowerShellin avulla. Muuttaminen PowerShellin Azure SQL-tietokantaan hinnoittelu taso." 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="change-the-service-tier-and-performance-level-pricing-tier-of-a-sql-database-with-powershell"></a>Palvelun taso ja suorituskyvyn tason muuttaminen (hinnoittelu taso) PowerShellin avulla on SQL-tietokanta


> [AZURE.SELECTOR]
- [Azure portal](sql-database-scale-up.md)
- [**PowerShellin**](sql-database-scale-up-powershell.md)


Palvelutasot ja suorituskyvyn tasoon kuvaavat ominaisuuksia ja resursseista SQL-tietokantaan ja päivittää sovelluksen muutoksen tarpeisiin. Lisätietoja on artikkelissa [Palvelutasot](sql-database-service-tiers.md).

Huomaa, että palvelun tason muuttaminen ja/tai tietokannan suorituskykytaso Luo alkuperäisen tietokannan tasolla suorituskykyä ja vaihtaa sitten se yhteyksien. Tietoja ei häviä vaihdon mutta aikana lyhyt hetken, kun olemme siirtää se, tietokannan yhteydet on poistettu käytöstä, jotta joidenkin tapahtumien lennon saattaa peruutettava. Tässä ikkunassa vaihtelee, mutta on keskimäärin noin neljän sekunnin ja 99 % tapauksista on alle 30 sekuntia. Erittäin harvoin etenkin jos on suuri lento on tällä hetkellä yhteyksiä tapahtumien määrä on poistettu käytöstä, tässä ikkunassa voi olla pidempi.  

Koko asteikon ylöspäin prosessin kestoa määräytyy tietokannan koon ja palvelun taso ennen ja sen jälkeen muuta. Esimerkiksi 250 gt tietokannassa, jossa voit, kohteesta tai kuluessa vakiorivejä, taso muuttamista täyttävän 6 tunnin kuluessa. Tietokannalle samankokoinen, joka muuttuu suorituskyvyn tasojen Premium-palvelutaso se valmistuu 3 tunnin kuluessa.


- Tietokannan muuntaminen, tietokanta on oltava pienempi kuin kohde-palvelutaso enimmäiskoko. 
- Päivitystä tietokannan kanssa [Geo replikoinnin](sql-database-geo-replication-portal.md) käytössä, ennen ensisijainen luodun tietokannan on päivitettävä ensin sen toissijainen tietokantojen haluamasi suorituskyvyn taso.
- Kun siirtyminen Premium palvelutaso, kaikki Geo replikoinnin yhteydet on ensin Lopeta. Voit noudattaa lopettaa välillä on ensisijainen ja toissijainen active tietokantojen replikointiprosessi [käyttökatkosta palauttaminen](sql-database-disaster-recovery.md) artikkelissa kuvatut vaiheet.
- Palauta-palveluja määräytyvät eri palvelutasot. Jos sinulla on päivitysprosessin saattavat menettää mahdollisuuden palauttaa pisteeseen senhetkinen tai jos käytössäsi on alemman varmuuskopion säilytysaika. Lisätietoja on artikkelissa [Azure SQL-tietokannan varmuuskopiointi ja palauttaminen](sql-database-business-continuity.md).
- Tietokannan uusia ominaisuuksia ei käytetä, ennen kuin muutokset ovat valmiit.



**Tässä artikkelissa suorittamiseen tarvitset seuraavat:**

- Azure tilaus. Jos tarvitset Azure tilauksen yksinkertaisesti **ILMAISEN tilin** tämän sivun yläosassa ja valitse palaa valmis tässä artikkelissa.
- Azure SQL-tietokantaan. Jos sinulla ei ole SQL-tietokantaan, luo yhden noudattamalla tämän artikkelin: [ensimmäisen Azure SQL-tietokannan luominen](sql-database-get-started.md).
- Azure PowerShell.


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]



## <a name="change-the-service-tier-and-performance-level-of-your-sql-database"></a>SQL-tietokantaan palvelun taso ja suorituskyvyn tason muuttaminen

Suorita [määrittäminen AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619433(v=azure.300\).aspx) cmdlet-komennosta ja set **-RequestedServiceObjectiveName** suorituskyvyn tasoa haluamasi hinnoittelu tason; esimerkiksi *S0*, *S1*, *S2*, *S3*, *P1*, *P2*,...

```
$ResourceGroupName = "resourceGroupName"
    
$ServerName = "serverName"
$DatabaseName = "databaseName"

$NewEdition = "Standard"
$NewPricingTier = "S2"

Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```

  

   


## <a name="sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database"></a>Esimerkki PowerShell-komentosarjaa SQL-tietokantaan palvelun taso ja suorituskyvyn tason muuttaminen

Korvaa ```{variables}``` arvot (Älä sisällytä aaltosulkeet) kanssa.

```
$SubscriptionId = "{4cac86b0-1e56-bbbb-aaaa-000000000000}"
    
$ResourceGroupName = "{resourceGroup}"
$Location = "{AzureRegion}"
    
$ServerName = "{server}"
$DatabaseName = "{database}"
    
$NewEdition = "{Standard}"
$NewPricingTier = "{S2}"
    
Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId
    
Set-AzureRmSqlDatabase -DatabaseName $DatabaseName -ServerName $ServerName -ResourceGroupName $ResourceGroupName -Edition $NewEdition -RequestedServiceObjectiveName $NewPricingTier
```
        


## <a name="next-steps"></a>Seuraavat vaiheet

- [Skaalaa, ja valitse](sql-database-elastic-scale-get-started.md)
- [Yhdistä ja kyselyjen kanssa SSMS SQL-tietokanta](sql-database-connect-query-ssms.md)
- [Vie Azure SQL-tietokanta](sql-database-export-powershell.md)

## <a name="additional-resources"></a>Lisäresursseja

- [Liiketoiminnan jatkuvuus yhteenveto](sql-database-business-continuity.md)
- [SQL-tietokanta-asiakirjat](http://azure.microsoft.com/documentation/services/sql-database/)
- [Azure SQL-tietokannan cmdlet] (http://msdn.microsoft.com/library/azure/mt574084https :/ / msdn.microsoft.com/kirjasto/azure/mt619433(v=azure.300\).aspx.aspx)