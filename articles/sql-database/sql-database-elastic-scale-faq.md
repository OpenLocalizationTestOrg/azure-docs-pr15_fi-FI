<properties 
    pageTitle="Azure SQL-joustavasti asteikko usein kysytyt kysymykset | Microsoft Azure" 
    description="Usein kysyttyjä kysymyksiä Azure SQL-tietokannan joustavasti asteikko." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Joustavasti Tietokantatyökalut usein kysytyt kysymykset 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Jos käytössä on yksi-vuokraajan shard ja sharding ei näppäintä kohden, kuinka voin täyttää sharding näppäintä rakenteen tiedot?

Rakenteen info-objekti käytetään vain Jaa yhdistämisen skenaarioita. Jos sovellus on myös single-vuokraajan, se ei edellytä Jaa Yhdistä työkalun ja näin ei ole tarpeen täytä rakenteen info-objekti.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Minulla on valmisteltu tietokannan ja Shard kartan esimies on jo, kuinka kuin shard uusi tietokanta rekisteröidä?

Katso **[lisääminen sovellukseen joustavasti tietokannan asiakas-kirjaston käytön shard](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Kuinka paljon joustavasti Tietokantatyökalut maksaa?

Joustavasti tietokannan asiakas-kirjaston käytön ei maksamaan kaikki kustannukset. Kustannukset kertyvät vain Azure SQL-tietokantoja, jota käytetään shards ja Shard kartan Managerin sekä Jaa Yhdistä työkalun valmistella web/työntekijä roolit varten.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Miksi Omat tunnistetiedot eivät toimi kun lisään shard eri palvelimesta?
Älä käytä tunnistetiedot muodossa "käyttäjän ID=username@servername”, riittää, että Käytä" Käyttäjätunnus = käyttäjänimi ".  Varmista, että "käyttäjänimi" kirjautuminen on oikeudet shard.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Pitääkö Shard kartan Managerin luominen ja täytä shards aina, kun käynnistän Omat sovellukset?

Ei – Shard kartan Manager (esimerkiksi **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) luominen on erikseen toiminto.  Sovelluksen pitäisi käyttää puhelu **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** sovelluksen pysäyttämisestä aikaan.  Olisi vain yksi näiden sovellusten toimialuetta kohti kutsu.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Minulla on joustavasti Tietokantatyökalut kysymyksiin, miten voin hankkia vastauksia? 

Ota saavuttaminen us [Azure SQL-tietokanta-keskustelupalstalla,](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)Valitse.

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Kun tietokantayhteys sharding avaimen avulla, voin voi silti kyselyn tietoja muiden samaa shard sharding-näppäimiä.  Tämä on suunniteltu ominaisuus?

Joustavasti asteikko-ohjelmointirajapinnan antaa yhteyden oikea tietokantaan sharding-näppäimen, mutta et anna sharding avaimen suodattaminen.  Tarvittaessa lisätä kyselyyn laajuuden rajoittaminen annettu sharding-näppäintä, **mihin** lausekkeita.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Käytettävät eri Azure-tietokanta-versioon, kunkin shard shard-määrittäminen

Kyllä, shard on yksittäisiä tietokannan ja näin ollen yhden shard voi olla Premium edition toiseen olla Standard edition. Lisäksi shard edition skaalata ylös tai alas useita kertoja shard elinkaaren aikana.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Jaa Yhdistä työkalun säännöstä ei tietokannan jakaminen ja yhdistäminen-toiminnon aikana (tai poistaa)? 

Ei. **Jaa** toimille kohdetietokannan tarvittavat rakenteen käyttäminen on oltava ja rekisteröitävä Shard kartan hallinnan kanssa.  **Yhdistä** toimintoja shard poistaminen shard kartan hallinta ja Poista tietokanta.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 