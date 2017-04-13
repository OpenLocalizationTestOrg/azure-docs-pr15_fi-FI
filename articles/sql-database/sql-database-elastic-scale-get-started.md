<properties 
    pageTitle="Joustavasti Tietokantatyökalut käytön aloittaminen" 
    description="SELITYS Basic joustavasti tietokannan työkalut-toiminnon, Azure SQL-tietokanta, mukaan lukien helppo otoksen sovelluksen suorittamiseen." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Joustavasti Tietokantatyökalut käytön aloittaminen

Tämän asiakirjan esitellään Kehitystyökalut-ratkaisun mukaan malli-sovellusta. Näyte Luo yksinkertaisen sharded sovelluksen ja käsitellään joustavasti Tietokantatyökalut n tärkeimpiin ominaisuuksiin. Esimerkissä funktioita [joustavasti tietokannan asiakkaan kirjastoon](sql-database-elastic-database-client-library.md)

Asenna kirjastoon, siirry [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Huomaa, että kirjasto on asennettu otoksen sovelluksen seuraavalla tavalla.

## <a name="prerequisites"></a>Edellytykset

1. Jossa C# Visual Studio 2012 tai uudempi versio vaaditaan. Lataa maksuton versio osoitteessa [Visual Studio Lataa](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. Nuget 2.7 tai uudempi versio. Uusimman version asentamisesta [Asentaminen NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Lataa ja suorita malli-sovellus

**Joustavasti Azure SQL-tietokannan – aloittaminen** sovelluksen malli on kuvattu sharded Azure SQL-joustavasti Tietokantatyökalut sovellusten kehittämisen käyttökokemusta tärkeimmät ominaisuuksia. Se keskitytään avaimen käytön laatikkomäärät [shard hallinta](sql-database-elastic-scale-shard-map-management.md), [tietojen riippuvaiset reititys](sql-database-elastic-scale-data-dependent-routing.md) ja [usean shard kyselyt](sql-database-elastic-scale-multishard-querying.md). Lataamisen ja suorittamisen otosten, toimi seuraavasti: 

1. Avaa Visual Studio ja valitse **-Tiedosto > uusi projekti ->**.
2. Valitse **Online**-valintaikkunassa.

    ![Uusi projekti > online-tilassa][2]
3. Valitse **Visual C#** **objektit**-kohdassa.

    ![Valitse Visual C#][3]
4. Kirjoita hakuruutuun **joustavasti db** otosten etsimiseen. Otsikon **Joustavasti Azure SQL - aloittaminen DB Työkalut** tulevat näkyviin.

    ![Hakuruutu][1]
 
5. Valitse malli, valitsemalla nimen ja sijainnin projektin ja paina **OK** voit luoda projektin.
6. **App.config** -tiedoston avaaminen otoksen projektin ratkaisu ja noudata ohjeita voit lisätä oman Azure SQL-tietokanta palvelimen nimi ja kirjautumistietoja (käyttäjänimi ja salasana)-tiedoston.
7. Muodosta ja suorita sovellus. Kun ohjelma kysyy, sallia Visual Studio palauttaa ratkaisun NuGet-paketit. Uusimman version joustavasti tietokannan asiakkaan kirjastoon ladataan NuGet.
8. Lisätietoja asiakkaan kirjaston ominaisuuksia eri vaihtoehtoja soittaminen Huomautus sovelluksen kestää konsolissa vaiheet tulostus- ja vapaasti tutkiminen koodin taustalla.

    ![käynnissä][4]

Onnittelut – on luotu ja suorita ensimmäisen sharded sovelluksen joustavasti Tietokantatyökalut käyttäminen Azure SQL-tietokantaan. Tutustu nopeasti shards, jotka otosten luotu muodostamalla Visual Studio tai SQL Server Management Studiossa Azure DB-palvelimeen. Huomaat, että uusi malli shard tietokantojen ja shard kartan hallinnan tietokannassa, jossa malli on luotu.

> [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Tärkeimmät osat koodi-Esimerkki

1. **Hallinta Shards ja Shard kartat**: koodi kuvataan, miten voit käyttää shards, alueita, ja yhdistämismääritykset-tiedosto **ShardMapManagerSample.cs**. Voit etsiä lisätietoja tämän ohjeaiheen: [Shard kartan hallinta](http://go.microsoft.com/?linkid=9862595).  
2. **Tietoja riippuvaiset reititys**: reititys tapahtumien oikean shard näkyy **DataDependentRoutingSample.cs**. Lisätietoja on artikkelissa [Tietojen riippuvaiset reititys](http://go.microsoft.com/?linkid=9862596). 
3. **Kysely päälle useita Shards**: yli shards kysely on esitelty **MultiShardQuerySample.cs**-tiedostossa. Lisätietoja on artikkelissa [Usean Shard kyselyt](http://go.microsoft.com/?linkid=9862597).
4. **Lisääminen tyhjä shards**: uusi tyhjä shards iteratiivinen lisääminen suorittaa koodin **AddNewShardsSample.cs**-tiedostossa. Tietoja tässä artikkelissa käydään läpi tähän: [Shard kartan hallinta](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Joustavasti asteikko muihin toimintoihin

1. **Jakaminen aiemmin shard**: voidaan jakaa shards on annettu **Jaa ja yhdistäminen-työkalu**. Voit etsiä lisätietoja tämä työkalu: [Jaa Yhdistä työkalun yleiskatsaus](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Yhdistäminen aiemmin shards**: Shard yhdistää suoritetaan myös **Jaa ja yhdistäminen-työkalun**avulla. Saat lisätietoja viitata: [Jaa Yhdistä työkalun yleiskatsaus](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Kustannukset

Joustavasti Tietokantatyökalut ovat maksutta. Joustavasti Tietokantatyökalut asettamatta muita kulujen Azure käyttö kustannukset päälle. 

Esimerkiksi sovelluksen malli luo uusien tietokantojen. Kustannus määräytyy Azure SQL-DB tietokanta-version, voit valita ja sovelluksen Azure käyttö.

Alla on tietoja artikkelissa [SQL tietokanta hinnat tiedot](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Seuraavat vaiheet
Saat lisätietoja joustavasti Tietokantatyökalut:

* [Joustavasti tietokannan Työkalut dokumentaatio](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    MALLIKOODEJA: 
    -    [Joustavasti DB Azure SQL - käytön aloittaminen](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Azure SQL - integraation kohteen Framework joustavasti DB](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Valitse komentosarjat shard joustavuus](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blogi: [joustavasti asteikko-ilmoitus](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Kanavan 9: [joustavasti asteikko yleiskatsaus Video](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Keskustelun keskustelupalsta: [Azure SQL-tietokanta-keskustelupalsta](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Mitata suorituskyky: [suorituskyvyn laskureita shard kartan hallinta](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
