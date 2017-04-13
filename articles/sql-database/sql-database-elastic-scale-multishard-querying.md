<properties 
    pageTitle="Usean shard kyselyt | Microsoft Azure" 
    description="Kyselyjen suorittaminen joustavasti tietokannan asiakas-kirjaston käytön shards yli." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Usean shard kyselyt

## <a name="overview"></a>Yleiskatsaus

Voit luoda [joustavasti Tietokantatyökalut](sql-database-elastic-scale-introduction.md)sharded tietokannan ratkaisuja. **Usean shard kyselyt** käytetään esimerkiksi tietojen kerääminen ja raportointi tehtäviä, joihin tarvitaan venyttää kyselyn suorittamisen useita shards yli. (Kontrasti tämä [tietojen riippuva reititys](sql-database-elastic-scale-data-dependent-routing.md), joka suorittaa työ yhden shard.) 

## <a name="overview"></a>Yleiskatsaus

1. Saat [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) tai [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)tai [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) -menetelmää. Katso [**luomisesta ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) ja [**RangeShardMap ja ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Luo **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** objekti.
2. Luo **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. **[CommandText-ominaisuuden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** arvoksi T-SQL-komennon.
3. Suorita komento **[ExecuteReader menetelmää](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Tarkastele tuloksia **[MultiShardDataReader luokan](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**avulla. 

## <a name="example"></a>Esimerkki

Seuraava koodi on kuvattu käyttö usean shard kyselyt tietyn **ShardMap** nimeltä *myShardMap*avulla. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Avaimen ero on usean shard yhteydet rakentaminen. Jos **SqlConnection** toimii yhteen tietokantaan, **MultiShardConnection** kestää ***shards sivustokokoelman*** sen syötteenä. Täytä shards shard kartta-sivustokokoelman. Kysely suoritetaan sitten **UNION ALL** semantiikkaan liittyvien avulla voit koota yhden yleinen tuloksen shards sivustokokoelman. Voit myös nimi, johon rivi on peräisin shard voidaan lisätä **ExecutionOptions** -ominaisuuden käyttäminen komennon tulosteen. 

Huomautus **myShardMap.GetShards()**puhelu. Tämä menetelmä hakee kaikki shards shard kartan ja tarjoaa kyselyn suorittaminen eri asiaa piilotetusta helposti. Shards kokoelma, usean shard kyselyn voidaan tarkentaa edelleen suorittamalla LINQ kyselyn päälle kokoelma palauttaa **myShardMap.GetShards()**puhelu. Yhdessä osittaisia tuloksia käytännön valittu usean shard kyselyt-ominaisuus on suunniteltu toimimaan hyvin shards satoja ylöspäin lähimpään kymmeneen.

Kanssa usean shard kyselyt rajoitus on tällä hetkellä shards ja shardlets, jossa pyydetään vahvistus puuttuminen. Kun tietojen riippuva reititys tarkistaa, että tietyn shard, shard kartan kyselyt ajankohtana, usean shard kyselyjen ei tarkisteta. Tämä voi aiheuttaa käytössä tietokannoissa, jotka on poistettu shard kartan usean shard kyselyihin.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Usean shard kyselyt ja jaa yhdistämistä

Usean shard kyselyt eivät tarkista onko shardlets kyselyn pohjana tietokanta on meneillään jatkuva Jaa-yhdistämistä. (Katso [Skaalaus joustavasti Jaa ja yhdistäminen-tietokantatyökalun avulla](sql-database-elastic-scale-overview-split-and-merge.md).) Tämä voi aiheuttaa epäyhtenäisyydet missä saman shardlet rivit näyttää useiden tietokantojen samassa usean shard kyselyssä. Ota huomioon seuraavat rajoitukset ja tarvittaessa draining meneillään oleviin Jaa ja yhdistäminen-toiminnot ja muutokset shard kartan suorittaessasi usean shard kyselyt.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Katso myös
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** luokat ja menetelmät.


Hallitse shards [joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md)käytöstä. Sisältää nimitilan, kutsutaan [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) , joka mahdollistaa kyselyn useita shards yhteen kyselyn ja tulos. Se sisältää kyseltäessä otetaan shards kokoelma päälle. Se sisältää myös vaihtoehtoinen käytäntöihin, erityisesti osittaisia tuloksia, käsitellä virheet, kun kysely suoritetaan monta shards päälle.  

 