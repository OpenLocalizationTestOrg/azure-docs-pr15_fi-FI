<properties
    pageTitle="Suorituskyvyn laskureita shard kartan hallinta"
    description="ShardMapManager luokan ja tietojen riippuvaiset reititys suorituskyvyn laskureita"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="SilviaDoomra"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="SilviaDoomra"/>

# <a name="performance-counters-for-shard-map-manager"></a>Suorituskyvyn laskureita shard kartan hallinta

Voit siepata [shard kartan hallinta](sql-database-elastic-scale-shard-map-management.md)suorituskykyä erityisesti silloin, kun käytät [tietojen riippuvaiset reititys](sql-database-elastic-scale-data-dependent-routing.md). Laskureita luodaan maksutapojen Microsoft.Azure.SqlDatabase.ElasticScale.Client-luokka.  

Seuraa yrityksen [tietojen riippuvaiset reititys](sql-database-elastic-scale-data-dependent-routing.md) -toimintoa käytetään laskureita. Nämä laskureita voi käyttää suorituskyvyn valvonnassa "Joustavasti tietokannan: Shard hallinta"-luokassa.

**Uusimman version:** Siirry [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Katso myös [uusimmat joustavasti tietokannan asiakas-kirjasto-sovelluksen päivittäminen](sql-database-elastic-scale-upgrade-client-library.md).

## <a name="prerequisites"></a>Edellytykset

* Suorituskyvyn luokka ja laskureita luomiseen käyttäjän on oltava **sovelluksen isännöivän tietokoneen paikalliseen järjestelmänvalvojaryhmään** osa.  

* Suorituskyvyn laskuri-esiintymän luominen ja päivittäminen laskureita, käyttäjän on oltava **järjestelmänvalvojien** tai **Suorituskyvyn valvonnan käyttäjät** -ryhmän jäsen. 

## <a name="create-performance-category-and-counters"></a>Luo suorituskyvyn luokka ja laskureita 

Jos haluat luoda laskureita, kutsua [ShardMapManagmentFactory luokan](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.aspx)CreatePeformanceCategoryAndCounters-menetelmää. Vain järjestelmänvalvoja voi suorittaa menetelmää: 

    ShardMapManagerFactory.CreatePerformanceCategoryAndCounters()  

Voit käyttää myös [tämän](https://gallery.technet.microsoft.com/scriptcenter/Elastic-DB-Tools-for-Azure-17e3d283) PowerShell-komentosarjaa suorittaa menetelmää. Menetelmä luo seuraavia suorituskyvyn laskureita:  

* **Välimuisti yhdistämismääritykset**: yhdistämismääritykset välimuistiin shard kartan määrä.
*  **DDR toiminnot/sec**: tietojen riippuvaiset reititys toiminnot shard kartan korkokannan. Laskuri päivitetään, kun puhelun kohde shard onnistuneen yhteyden [OpenConnectionForKey()](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) tuloksiin. 
*  **Yhdistettyjen haku osumia sekunnissa**: korko yhdistämismääritykset shard rakenneruudussa onnistuu välimuistin haku-toimintoa. 
*  **Yhdistettyjen haku epäonnistumisia sekunnissa**: korko yhdistämismääritykset shard määrityksen epäonnistui välimuistin haku-toimintoa.
*  **Yhdistämismääritysten lisätty tai päivitetty välimuistin/sek**: nopeus, mitkä yhdistämismääritykset lisätään tai päivitetty välimuistin shard kartan. 
*  **Yhdistäminen poistettu välimuistin/s**: korko, jolla yhdistämismääritykset poistetaan välimuistista shard kartan. 

Suorituskyvyn laskureita luodaan kunkin välimuistiin shard Map prosessia kohti.  


## <a name="notes"></a>Huomautuksia
Seuraavista tilanteista Käynnistä suorituskyvyn laskureita luominen:  

* Alustus [ShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) [kirjoitusasennon lataamisen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerloadpolicy.aspx), jos ShardMapManager sisältää kaikki shard kartat kanssa. Näihin kuuluvat [GetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx?f=255&MSPPError=-2147217396#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerFactory.GetSqlShardMapManager%28System.String,Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardMapManagerLoadPolicy%29) ja [TryGetSqlShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx) menetelmiä.
* Onnistuneiden haku shard kartan (joko [GetShardMap()](https://msdn.microsoft.com/library/azure/dn824215.aspx), [GetListShardMap()](https://msdn.microsoft.com/library/azure/dn824212.aspx) tai [GetRangeShardMap()](https://msdn.microsoft.com/library/azure/dn824173.aspx)). 

* Shard kartan avulla CreateShardMap() luonti onnistui.

Suorituskyvyn laskureita päivitetään kaikki välimuistin toiminnot suorittaa shard kartta- ja määritykset. Shard kartan avulla DeleteShardMap () reults esiintymä suorituskyvyn laskureita poistaminen onnistuu poistamista.  

## <a name="best-practices"></a>Parhaat käytännöt

* Suorituskyvyn luokka ja laskureita luomista on tehtävä vain kerran ennen ShardMapManager objektin luomista. Jokaisen komennon suorittamisen CreatePerformanceCategoryAndCounters() (kaikki esiintymät ilmoittaa menetä) edellisen laskureita tyhjentää ja luo uusia.  

* Prosessia kohti luodaan suorituskyvyn laskuriesiintymät. Mikä tahansa sovellus kaatuu tai shard kartan välimuistista poistaminen johtaa suorituskyvyn laskureita esiintymien poistaminen.  

### <a name="see-also"></a>Katso myös

[Joustavasti tietokannan ominaisuuksien yleiskatsaus](sql-database-elastic-scale-introduction.md)  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->

