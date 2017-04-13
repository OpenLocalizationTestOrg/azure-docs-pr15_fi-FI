<properties 
    pageTitle="Lisäämällä shard, käyttämällä joustavasti Tietokantatyökalut | Microsoft Azure" 
    description="Määritä joustavasti asteikko-ohjelmointirajapinnan käyttäminen lisäämään uusia shards shard." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Shard, käyttämällä joustavasti Tietokantatyökalut lisääminen

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Jos haluat lisätä uuden tai avain shard  

Sovellukset on usein Lisää uusi shards käsittelemään tietoja, jotka on odotettavissa uudet avaimet tai avaimen tulostusalueet shard kartan, jossa on jo olemassa. Esimerkiksi sovelluksen sharded vuokraajan tunnuksen mukaan saatat joutua valmistella uusi shard luo uutta vuokraajakohdetta tai tietojen sharded kuukausittain joutua uusi shard, ennen aloittamista kunkin uuden kuukauden valmistelun yhteydessä. 

Jos uusi solualue, avainarvot ei ole jo aiemmin yhdistämisen osana, on erittäin yksinkertaisia, Lisää uusi shard ja liitä uuden tunnuksen tai kyseisen shard alue. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Esimerkki: lisääminen shard ja sen alueen aiemmin shard-kartta
Tässä esimerkissä käytetään [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx) [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) menetelmiä ja luo [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) luokan esiintymän. Alla olevassa esimerkissä **sample_shard_2** ja kaikki tarvittavat rakenteen objektit sisältyy se tietokanta on luotu pitoon alueen [300-400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Vaihtoehtona Luo uusi Shard kartan hallinta PowerShellin avulla. Esimerkki on saatavilla [tähän](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Jos haluat lisätä tyhjän osan aiemmin luodun alueen shard  

Joissakin tapauksissa sinulla on jo yhdistetty alue shard ja osittain täytetty sen tietoja, mutta haluat nyt ohjautuu eri shard tulevat tiedot. Esimerkiksi voit shard päivä-alueen mukaan ja on jo kohdistettu 50 päivää shard, mutta päivänä 24 haluat purkaa eri shard tietojen. Joustavasti tietokannan [Jaa Yhdistä työkalun](sql-database-elastic-scale-overview-split-and-merge.md) voi suorittaa tämän toiminnon, mutta jos tietojen siirto ei ole välttämätöntä (esimerkiksi tietojen solualue, päivät, [25, 50), eli, päivä 25 50 yksityisesti, mukaan lukien ei ole vielä) voit tehdä tämän käyttämällä kokonaan Shard kartan hallinnan API suoraan.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Esimerkki: jakaminen alueen ja tyhjä osa määritteleminen lisäämäsi shard

Tietokanta nimeltä "sample_shard_2" ja kaikki tarvittavat rakenteen objektit sisältyy se on luotu.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Tärkeää**: käyttää tätä tapaa, jos ole varma, että alueen päivitetyt yhdistäminen on tyhjä.  Edellisistä menetelmistä Älä tarkista tietojen alueen siirretään, joten kannattaa tarkistaa sisällyttäminen koodisi.  Jos rivit siirretään alueen, todellisten tietojen jakautumisen eivät vastaa päivitetyt shard kartan. [Jaa ja yhdistäminen-työkalun](sql-database-elastic-scale-overview-split-and-merge.md) avulla voit suorittaa toiminnon sijaan tällaisissa tapauksissa.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
