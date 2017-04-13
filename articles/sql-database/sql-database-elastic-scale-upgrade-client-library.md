< ominaisuudet
    
    pageTitle="Upgrade to the latest elastic database client library | Microsoft Azure" 
    description="Upgrade apps and libraries using Nuget" 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove" />

# <a name="upgrade-an-app-to-use-the-latest-elastic-database-client-library"></a>Uusimman joustavasti tietokannan asiakas-kirjasto-sovelluksen päivittäminen

[Joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md) uusia versioita ovat käytettävissä NuGetand Visual Studiossa NuGetPackage hallinta-liittymän kautta. Päivitykset sisältävät virheenkorjauksia ja tukea asiakkaan kirjaston uusia ominaisuuksia.

**Uusimman version:** Siirry [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Rakenna uudelleen uuteen kirjastoon sovelluksen sekä muuttaa aiemmin Shard kartan hallinta metatietojen tallennettu Azure SQL-tietokantoja tukemaan uusia ominaisuuksia.

Seuraavien vaiheiden järjestyksessä varmistaa, että vanhat asiakkaan kirjaston eivät enää ole ympäristössäsi metatieto-objektit päivitysajankohdan mikä tarkoittaa, että vanha versio metatietojen objekteja ei luoda päivityksen jälkeen.   

## <a name="upgrade-steps"></a>Päivitysvaiheet

**1. päivittämällä sovellukset.** Visual Studiossa Lataa ja viitata asiakkaan kirjaston uusimman kyselyjä kaikissa projekteissa, jotka käyttävät kirjastossa; kehittäminen Valitse Muodosta uudelleen ja ota käyttöön. 

 * Valitse Visual Studio-ratkaisun **Työkalut** --> **NuGet pakettien hallinta** -->  **Ratkaisu NuGet pakettien hallinta**. 
 * (Visual Studio 2013) Vasemmassa ruudussa Valitse **päivitykset**ja valitse sitten package **Azure SQL tietokanta joustavasti asteikko asiakkaan kirjastoon** , joka näkyy ikkunassa **Päivitä** -painiketta.
 * (Visual Studio 2015) Määritä suodatin-kenttään päivittäminen **käytettävissä**. Valitse paketti, Päivitä ja **Päivitä** -painiketta.
    
 
 * Muodosta ja ota käyttöön. 

**2. päivittämällä komentosarjojesi.** Jos käytössäsi on **PowerShell** -komentosarjojen, voit hallita shards, [Lataa uusi kirjasto](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) ja kopioi kansioon, josta voit suorittaa komentosarjoja. 

**3. Päivitä Jaa ja yhdistäminen-palvelussa.** Jos joustavasti tietokantatyökalu Jaa yhdistämisen avulla voit järjestää sharded tietoja, [Lataa ja asentaa työkalun uusimman version](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/). Yksityiskohtaiset päivitysvaiheet palvelun löytyy [tähän](sql-database-elastic-scale-overview-split-and-merge.md). 

**4. päivittäminen Shard kartan Manager-tietokantoja**. Päivitä metatiedot Shard kartat tukemaan Azure SQL-tietokantaan.  Voit tehdä tämän, PowerShell-tai C# kahdella tavalla. Molemmat vaihtoehdot näkyvät jäljempänä.

***Vaihtoehto 1: Päivitä metatietojen PowerShellin avulla***

1. Lataa komentorivin apuohjelma NuGet [täältä](http://nuget.org/nuget.exe) ja tallentaa kansioon. 

2. Avaa komentorivi-ikkuna, siirry samaan kansioon ja antamalla komennon:`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Client`

3. Siirry alikansio, joka sisältää uuden asiakkaan DLL version olet juuri ladannut, esimerkiksi:`cd .\Microsoft.Azure.SqlDatabase.ElasticScale.Client.1.0.0\lib\net45`

4. Lataa joustavasti tietokannan asiakkaan päivityksen scriptlet [Komentosarjat](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-Elastic-6442e6a9)ja tallenna se samaan kansioon, joka sisältää DLL.

5. Kansiosta Suorita "PowerShell.\upgrade.ps1" komentokehotteen ja noudata ohjeita.
 
***Vaihtoehto 2: Päivitä käyttämällä C# metatiedot***

Voit myös luoda Visual Studio-sovelluksen, joka avautuu oman ShardMapManager iteroivaa kaikki shards päälle ja suorittaa metatietojen päivitys soittamalla menetelmistä [UpgradeLocalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradelocalstore.aspx) ja [UpgradeGlobalStore](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.upgradeglobalstore.aspx) esimerkin osoittamalla tavalla: 

    ShardMapManager smm =
       ShardMapManagerFactory.GetSqlShardMapManager
       (connStr, ShardMapManagerLoadPolicy.Lazy); 
    smm.UpgradeGlobalStore(); 
    
    foreach (ShardLocation loc in
     smm.GetDistinctShardLocations()) 
    {   
       smm.UpgradeLocalStore(loc); 
    } 

Metatietojen päivityksistä näistä menetelmistä voi käyttää useita kertoja ilman haittaa. Esimerkiksi jos asiakkaan vanhempaa Luo shard vahingossa, kun olet jo päivittänyt, voit suorittaa päivitys uudelleen kaikki shards varmistaa, että uusin metatietojen esitä koko infrastruktuurin yli. 

**Huomautus:**  Asiakirjakirjaston uusia versioita julkaistaan alusta alkaen jatkaa Shard kartan hallinta metatietojen aiemmissa versioissa Azure SQL-DB ja päinvastoin.   Kuitenkin hyödyntämään uusimman asiakkaan joistain uusia ominaisuuksia, on päivitettävä metatiedot.   Huomaa, että metatietojen päivityksiä ei vaikuta käyttäjän tiedot tai sovelluksen kielikohtaiset tiedot vain objektit luodaan ja käytetään Shard kartan hallinta.  Ja sovellusten jatkaa toimimaan päivitystoimintoa yllä olevien ohjeiden avulla. 

## <a name="elastic-database-client-version-history"></a>Joustavasti tietokannan asiakkaan versiohistoria 

Versiohistoria Siirry [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]  


<!--Image references-->
[1]:./media/sql-database-elastic-scale-upgrade-client-library/nuget-upgrade.png
 