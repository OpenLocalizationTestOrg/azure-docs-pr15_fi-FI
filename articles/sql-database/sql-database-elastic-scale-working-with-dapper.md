<properties 
    pageTitle="Joustavasti tietokannan asiakkaan kirjaston käyttäminen Dapper | Microsoft Azure" 
    description="Joustavasti tietokannan asiakkaan kirjaston käyttäminen Dapper." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Joustavasti tietokannan asiakkaan kirjaston käyttäminen Dapper 

Tämä asiakirja on sovelluskehittäjille, jotka perustuvat Dapper, voit luoda sovelluksia, mutta haluat myös kehityshankkeissa [joustavasti tietokannan tooling](sql-database-elastic-scale-introduction.md) voit luoda sovelluksia, jotka toteuttavat sharding skaalaus-kohtaa niiden tietojen taso.  Tässä asiakirjassa on kuvattu Dapper-sovelluksissa, joita tarvitaan joustavasti Tietokantatyökalut integroida muutokset. Tutustu kohdistus on käyttöön sähköpostiviestiä joustavasti tietokannan shard hallinta ja tietojen riippuvaiset reititys Dapper kanssa. 

**Esimerkki koodi**: [Azure SQL-tietokanta - Dapper integrointi joustavasti Tietokantatyökalut](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Integrointi **Dapper** ja **DapperExtensions** Azure SQL-tietokanta joustavasti tietokannan asiakas-kirjaston kanssa on helppoa. Sovellusten voit käyttää tietojen riippuvaiset reititys muuttaminen luominen ja avaamalla uusien [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektien [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) puhelun [asiakkaan kirjasto](http://msdn.microsoft.com/library/azure/dn765902.aspx). Tämä rajoittaa muutokset sovelluksessa vain, jos uudet yhteydet luodaan ja avataan. 

## <a name="dapper-overview"></a>Dapper yleiskatsaus
**Dapper** on objektin relaatio mapper. Se yhdistää .NET objektien sovelluksestasi relaatiotietokannasta (tai päinvastoin). Sample code ensimmäinen osa on kuvattu, miten joustavasti tietokannan asiakkaan kirjastossa voit integroida Dapper-sovelluksissa. Sample code toisen osan havainnollistaa, miten voit integroida käytettäessä Dapper ja DapperExtensions.  

Dapper mapper-toiminto tarjoaa tunniste menetelmiä tietokantayhteyksiä, jotka helpottavat suorittamisen tai kyselyt tietokannan lähettävä T-SQL-lauseita. Esimerkiksi Dapper on helppo yhdistämään .NET-objektit ja SQL-lauseita **suoritus** puheluihin parametrit välillä tai SQL-kyselyiden tulokset tarjoaman .NET objekteiksi Dapper kutsujen **kyselyn** avulla. 

Kun käytät DapperExtensions, et enää tarvitse antaa SQL-lauseita. Tunnisteet menetelmien esimerkiksi **GetList** tai **Lisää** tietokantayhteys Luo taustatietoja SQL-lauseita.
 
Toinen etu Dapper ja DapperExtensions on sovelluksen ohjaa tietokantayhteys luomista. Näin voit käsitellä joustavasti tietokannan asiakkaan kirjastoon joka vakuutuksenvälittäjän tietokannan yhteydet shardlets tietokantoihin yhdistämisen perusteella.

Dapper kokoonpanon asentamisesta [Dapper piste nettonykyarvon](http://www.nuget.org/packages/Dapper/). Katso [DapperExtensions](http://www.nuget.org/packages/DapperExtensions)Dapper tunnisteet.

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Lyhyesti joustavasti tietokannan asiakas-kirjasto

Joustavasti tietokannan asiakas-kirjaston kanssa määrität osioita kutsutaan *shardlets* sovelluksen tietojen, yhdistäminen tietokantojen ja tunnistaminen *sharding avaimet*. Voit määrittää niin monta tietokantaa kuin tarvitset ja jakaa oman shardlets tietokannat. Tietokantojen avainarvot sharding määritystä tallennetaan kirjaston ohjelmointirajapinnan myöntämä shard kartan mukaan. Tätä ominaisuutta kutsutaan **shard yhdistää hallinta**. Shard kartan toimii myös pyynnöt, jotka sisältävät sharding näppäintä tietokantayhteyksiä broker. Tätä ominaisuutta kutsutaan nimellä **tietojen riippuvaiset reititys**.

![Shard karttojen ja tietojen riippuvaiset reititys][1]

Shard kartan hallinta suojaa käyttäjien epäyhtenäinen näkymien shardlet tietoihin, jotka voivat ilmetä samanaikainen shardlet hallintatoiminnot ovat näistä-tietokannat. Voit tehdä shard kartat broker tietokantayhteyksiä sovelluksen luotu kirjastoon. Kun shard hallintatoiminnot voi olla vaikutusta shardlet, näin lopettaa automaattisesti tietokantayhteys shard kartta-toimintoja. 

Sijaan voit luoda yhteyksiä Dapper perinteinen asennustapa annettava [OpenConnectionForKey menetelmää](http://msdn.microsoft.com/library/azure/dn824099.aspx). Näin varmistat, että kaikki vahvistus tapahtuu ja yhteydet hallitaan oikein, kun kaikki tiedot siirtyvät shards välillä.

### <a name="requirements-for-dapper-integration"></a>Dapper integrointi vaatimukset

Kun käsittelet joustavasti tietokannan asiakkaan kirjastoon ja Dapper API, haluamme säilyttää seuraavat ominaisuudet:

* **Scaleout**: haluamme Lisää tai poista tietokannat sharded sovelluksen kapasiteettivaatimukset tarvittaessa sovelluksen tiedot-taso. 

-    **Yhdenmukaisuuden**: tutustu sovelluksen on skaalattu, käytössä sharding, koska annettava suorittaa tietojen riippuvaiset reititys. Haluat tietoja riippuvaiset reititys ominaisuuksia kirjaston avulla. Erityisesti haluat säilyttää vahvistus ja yhdenmukaisuuden takaa myöntämä yhteydet, jotka ovat se shard kartan hallinta – siten vältät vioittumisen tai väärä kyselyn tuloksiin. Näin varmistat, että yhteydet annetun shardlet hylätty tai pysäyttää, jos (esimerkiksi) shardlet tällä hetkellä siirretään eri shard, Jaa ja Yhdistä-ohjelmointirajapinnan käyttäminen.

-    **Objektin yhdistäminen**: haluamme säilyttää Dapper käännettävä luokat-sovelluksessa ja pohjana oleva tietokanta-rakenteet myöntämä yhdistämistä helppokäyttöisyys. 

Seuraavassa osassa on ohjeita näitä vaatimuksia sovellusten **Dapper** ja **DapperExtensions**perusteella.

## <a name="technical-guidance"></a>Tekniset ohjeet
### <a name="data-dependent-routing-with-dapper"></a>Tietoja riippuvaiset reititys Dapper kanssa 

Dapper-sovellus on yleensä vastuussa luomalla ja avaamalla olevan tietokannan yhteyksiä. Kirjoita T sovellus, Dapper palauttaa kyselytulokset kuin .NET sivustokokoelmat tyypin T. Dapper suorittaa yhdistämistä T-SQL-tulos rivien objektit tyypin T. Vastaavasti Dapper yhdistää .NET objektit SQL-arvoja tai tietojen käsittelemisessä language (Enimmäiskuolleisuusrajaa) lauseet parametrit. Dapper on tämän toiminnon ADO .NET SQL Clientin kirjastojen säännöllisesti [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektin menetelmiä tunniste kautta. SQL-yhteys palauttama joustavasti asteikko-ohjelmointirajapinnan DDR, ovat myös säännöllisesti [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekteja. Näin käyttämään Dapper extensions asiakirjakirjaston DDR API palauttama tyyppi suoraan, sillä se on myös yksinkertaista SQL Clientin yhteyttä.

Nämä huomautukset Varmista yksinkertaista käyttämään liittymillä joustavasti tietokannan asiakas-kirjastoa varten Dapper yhteydet.

(Valitse mukana näyte) tämä koodi-esimerkissä havainnollistetaan lähestymistapa, jossa sovelluksen kirjastoon broker oikean shard yhteys antama sharding-avain.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

[OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API puhelu korvaa oletusarvoisen luominen ja avaaminen SQL Clientin yhteyttä. [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kutsu on tietoja riippuvaiset reititys varten tarvittavat argumenttia: 

-    Voit käyttää tietojen riippuvaiset reititys liittymät shard-kartta
-    Tunnistaa shardlet sharding-avain
-    Tunnistetietojen (käyttäjänimi ja salasana) shard yhdistäminen

Shard map-objekti Luo shard, joka sisältää tietyn sharding-näppäimen shardlet yhteyden. Joustavasti tietokannan asiakkaan ohjelmointirajapinnan tunnisteen myös yhteyden toteuttamisesta sen yhdenmukaisuuden oikeudet. Koska [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) puhelu palauttaa tavallisen SQL Clientin yhteyden objektin, **Suorita** tunniste-menetelmän myöhemmin soittaja Dapper seuraa Dapper tapana.

Kyselyjen toimivat hyvin samalla tavalla – Avaa ensin yhteys [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) asiakaskoneesta API. Valitse säännöllisesti Dapper tunniste-menetelmillä yhdistämään .NET objekteiksi SQL-kyselyn tulokset:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Huomaa, että **käyttämällä** lohko, jonka DDR yhteyden vaikutusalueista yhden shard, jossa on käytettävissä tenantId1 Estä kaikki tietokannan toiminnot. Kysely palauttaa vain nykyisen shard tallennettuna blogit, mutta ei muiden shards tallennetun tiedoston. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Riippuvaiset reititys Dapper ja DapperExtensions tiedot

Dapper sisältyy tunnisteita, joka voi toimittaa edelleen helppokäyttöisyys ja otetaan tietokannasta, kun tietokanta kehityssovellusten ekosysteemiin. DapperExtensions on esimerkki. 

DapperExtensions käyttäminen sovelluksen eivät muutu, miten tietokannan yhteydet luodaan ja hallitaan. Se on edelleen sovelluksen vastuu Avaa yhteydet ja säännöllisesti SQL Clientin yhteyden objektien odotetaan tunniste menetelmillä. Emme voi luottaa [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) edellä kuvatulla. Näyttää seuraavat MALLIKOODEJA, ainoa muutos on, että Microsoft ei enää tarvitse kirjoittaa T-SQL-lauseita:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

Ja tässä on kyselyn koodi-Esimerkki: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Käsittely lyhytkestoisia virheitä

Microsoft Patterns ja käytännöt ryhmän julkaista auttaa pienentämään yhteisiä lyhytkestoisia vika edellytyksiä havaitsi pilveen suoritettaessa sovelluskehittäjät [Lyhytkestoisia vika käsittely sovelluksen estä](http://msdn.microsoft.com/library/hh680934.aspx) . Lisätietoja on artikkelissa [Perseverance, kaikki saavutuksensa salaisuus: lyhytkestoisia vika käsittely sovelluksen-eston avulla](http://msdn.microsoft.com/library/dn440719.aspx).

Koodi-malli on riippuvainen lyhytkestoisia vika kirjaston suojautua lyhytkestoisia virheitä. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

Yllä koodissa **SqlDatabaseUtils.SqlRetryPolicy** on määritetty **SqlDatabaseTransientErrorDetectionStrategy** ja uudelleen laskeminen 10 ja 5 sekuntia odotusaika uudelleenyritykset välillä. Jos käytössäsi on tapahtumia, varmista, että, yritä laajuus siirtyy takaisin kyseessä lyhytkestoisia vika tapahtuman alkua.

## <a name="limitations"></a>Rajoitukset

Tässä asiakirjassa kuvatut tavoista aiheuttaa pari rajoitukset:

* Tämän asiakirjan mallikoodia ei näytetään, miten rakenteen hallitsemaan shards.
* Valita pyyntö, oletetaan, että kaikki sen tietokannan käsittelyn sisältyy yksittäisen shard yksilöityä pyynnön myöntämä sharding-näppäintä. Kuitenkin tämän olettaen ei aina pidä esimerkiksi kun ei voida saataville sharding-näppäintä. Osoitteen tämä joustavasti tietokannan asiakkaan kirjasto sisältää [MultiShardQuery luokan](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Luokan toteuttaa yhteyden-otetaan päälle useita shards kyselyitä varten. MultiShardQuery käyttäminen yhdessä Dapper on tämän artikkelin piiriin.

## <a name="conclusion"></a>Tekemistä

Sovellusten Dapper ja DapperExtensions avulla voit helposti hyötyä joustavasti Tietokantatyökalut Azure SQL-tietokantaan. Tässä asiakirjassa kuvattujen vaiheiden näiden sovellusten käyttää työkalun ominaisuuksien tietojen riippuvaiset reititys muuttaminen luominen ja avaamalla uusien [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektien [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) puhelun joustavasti tietokannan asiakkaan kirjaston. Tämä rajoittaa sovelluksen muutokset tarvitaan näiden paikkaa, johon uudet yhteydet luodaan ja avataan. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 