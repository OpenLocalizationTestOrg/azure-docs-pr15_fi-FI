<properties 
    pageTitle="Joustavasti tietokannan asiakkaan kirjaston tunnistetietojen hallinta | Microsoft Azure" 
    description="Voit määrittää oikean suojaustason tunnistetietoja, järjestelmänvalvojan joustavasti tietokanta-sovellusten, vain luku-tilaan" 
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

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Käyttää joustavasti tietokannan asiakkaan kirjaston tunnistetiedot

[Joustavasti tietokannan asiakkaan kirjaston](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) käyttää kolmenlaisia tunnistetiedot [shard kartan hallinnan](sql-database-elastic-scale-shard-map-management.md)käyttäminen. Tarpeen mukaan käyttäminen alin käyttöoikeustaso mahdollisia tunnistetieto.

* **Tunnistetietojen hallinta**: luomisen ja käsittelevistä shard kartan esimies. (Katso [sanasto](sql-database-elastic-scale-glossary.md).) 
* **Käyttöoikeudet**: aiemmin kartan shard-Managerin, voit hankkia tietoja shards käyttäminen.
* **Yhteyden tunnistetiedot**: Muodosta yhteys shards. 

Katso myös [tietokantojen hallinta ja kirjautumiset Azure SQL-tietokantaan](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Lisätietoja tunnistetietojen hallinta

Hallinnan käytetään [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) -objektin sovelluksissa, jotka käsitellä shard karttojen luomiseen. (Esimerkiksi näkyviin [lisäämällä shard, käyttämällä joustavasti Tietokantatyökalut](sql-database-elastic-scale-add-a-shard.md) ja [tietojen riippuvaiset reititys](sql-database-elastic-scale-data-dependent-routing.md)) Joustavasti asteikko asiakkaan kirjaston käyttäjä luo SQL-käyttäjät ja SQL-kirjautumiset ja varmistaa, että jokainen on myönnetty luku-/ kirjoitusoikeudet yleinen shard kartta-tietokannan ja kaikki shard tietokannat. Nämä käytetään pitämään yleinen shard kartta- ja paikallisen shard kartat, kun shard kartan tehdyt muutokset on tehty. Käytä esimerkiksi shard kartan hallinta-objektin luominen hallinta-tunnistetietoja (käyttäminen [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Muuttujan **smmAdminConnectionString** on yhteysmerkkijonon, joka sisältää tunnistetietojen hallinta. Käyttäjätunnus ja salasana on luku-/ kirjoitusoikeudet shard kartan tietokannan ja yksittäisiä shards. Hallinta-yhteysmerkkijonon myös palvelimen nimi ja tietokannan nimi yleinen shard kartta-tietokantaan. Näin tyypillinen yhteysmerkkijonon tarkoitusta varten:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Älä käytä arvot muodossa "username@server"—instead vain "käyttäjänimi-kentän arvoksi.  Tämä johtuu siitä tunnistetiedot on työskenneltävä shard kartan manager-tietokannan ja yksittäisiä shards, joka voi olla eri palvelimiin.

## <a name="access-credentials"></a>Käyttöoikeudet
  
Kun luot shard kartan hallinnan sovellus, joka ei hallinta shard kartat, käytä tunnistetiedot, jotka on vain luku-oikeudet yleinen shard kartassa. Nämä tunnistetiedot-kohdassa yleinen shard kartan hakee tietoja käytetään [tietojen riippuva](sql-database-elastic-scale-data-dependent-routing.md) reitittämiseen ja täytä asiakkaan shard kartta-välimuistin. Tunnistetietojen toimitetaan samoissa puhelun kautta **GetSqlShardMapManager** yllä esitetyllä tavalla: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Huomaa, **smmReadOnlyConnectionString** vastaamaan eri tunnistetietoja käytön tämän access puolesta **muille kuin** järjestelmänvalvojille: näitä tunnistetietoja ei pitäisi antaa kirjoitusoikeudet yleinen shard kartassa. 

## <a name="connection-credentials"></a>Yhteyden tunnistetiedot 

Lisää tunnistetiedot tarvitaan, kun käyttää shard, sharding avaimeen liittyvät [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) -menetelmällä. Nämä tunnistetiedot on annettava paikallisen shard kartta-taulukoihin, shard käyttöön vain luku-käyttöoikeudet. Tämä on tarpeen suorittaa tietojen riippuva reititys shard yhteyden vahvistus. Tämä koodikatkelman mahdollistaa tietojen käyttö tietojen riippuvaiset reitityksestä kontekstissa: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

Tässä esimerkissä **smmUserConnectionString** pitää käyttäjän tunnistetietoja yhteysmerkkijono. Azure SQL-tietokannan näin tyypillinen yhteysmerkkijonon käyttäjän tunnistetietoja: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Järjestelmänvalvojan tunnistetiedot, jossa ei ole arvot muodossa "username@server". Käytä sen sijaan "käyttäjänimi".  Huomaa, että yhteysmerkkijono ei sisällä palvelimen nimi ja tietokannan nimi. Tämä johtuu **OpenConnectionForKey** puhelun automaattisesti ohjaa avaimen perusteella oikean shard yhteys. Näin ollen palvelimen nimi ja tietokannan nimi ei anneta. 

## <a name="see-also"></a>Katso myös
[Tietokantojen ja Azure SQL-tietokantaan kirjautumiset hallinta](sql-database-manage-logins.md)

[SQL-tietokannan suojaaminen](sql-database-security.md)

[Töiden joustavasti tietokannan käytön aloittaminen](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 