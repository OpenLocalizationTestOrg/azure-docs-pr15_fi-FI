<properties
    pageTitle="SQL-tietokantaan: Mikä DTU? | Microsoft Azure"
    description="Mitä Azure SQL-tietokannan tietoja tapahtuman yksikkö on."
    keywords="tietokanta-asetukset, tietokannan suorituskykyä"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Kerrotaan tietokannan tapahtuman yksiköt (DTUs) ja joustavasti tietokannan tapahtuman yksiköt (eDTUs)

Tässä artikkelissa kerrotaan tietokannan tapahtuman yksiköt (DTUs) ja joustavasti tietokannan tapahtuman yksiköt (eDTUs) ja mitä tapahtuu, kun valitset suurin DTUs tai eDTUs.  

## <a name="what-are-database-transaction-units-dtus"></a>Mitkä ovat tietokannan tapahtuman yksiköt (DTUs)

DTU on mittayksikkö resursseista, jotka ovat taata voi käyttää erillisen Azure SQL-tietokantaan, suorituskykyä tasolla sisällä [erillinen tietokannan palvelutaso](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels). DTU mittaa sekoitetun suorittimen ja muistin i/o ja tapahtuman log i/o OLTP ensisijainen työmäärää suunniteltu tyypillisiä tosielämän OLTP työmääriä määräytyy suhde. Kiertämiskoneet DTUs suurentamalla tietokannan suorituskykytaso vastaa tehtävässä laimennossarja resurssin käytettävyyden tietokannan joukko. Esimerkiksi Premium P11 tietokanta on 1750 DTUs sisältää Lisää DTU x 350 Laske power kuin tavallinen tietokanta on 5 DTUs. Käytetään DTU sekoitus OLTP ensisijainen työmäärää takana kehitysmenetelmistä on artikkelissa [SQL-tietokantaan ensisijainen yleiskatsaus](sql-database-benchmark-overview.md).

![Johdanto SQL-tietokantaan: yksi tietokannan DTUs taso ja taso](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Voit [muuttaa palvelutasot](sql-database-scale-up.md) milloin tahansa mahdollisimman vähän käyttökatkot (yleensä aluetoimistojen neljä sekuntia)-sovellukseen. Monissa yritykset ja sovelluksia ei voi luoda tietokantoja ja soita yhden tietokannan suorituskykyä ylös tai alas pyydettäessä on tarpeeksi, erityisesti jos käyttötavat ovat suhteellisen ennakoitavissa. Mutta jos käytössäsi on odottamattomia käyttötavat, voi olla vaikea hallinta kustannukset ja business-malli. Tässä skenaariossa joustavasti resurssivarannon käyttäminen eDTUs tietty määrä.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Mitkä ovat joustavasti tietokannan tapahtuman yksiköt (eDTUs)

EDTU on mittayksikkö resursseista (DTUs), jotka voidaan jakaa joukko tietokantojen kutsutaan [joustavasti resurssivarantoon](sql-database-elastic-pool.png)Azure SQL - palvelimella olevien. Joustavasti jakavat on yksinkertainen edullinen ratkaisu, voit hallita useita tietokantoja, joissa on useita laajasti ja odottamatonta käyttötavat suorituskyvyn tavoitteet. Katso [joustavasti jakavat ja palvelutasot](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) lisätietoja.

![Johdanto SQL-tietokantaan: eDTUs taso ja taso](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Resurssivarantoon annetaan eDTUs hintaan tietyn ajan. Sisällä resurssivarantoon yksittäiset tietokannat annetaan joustavuutta automaattinen-asteikon asettaa parametreja. Kuormitettu tietokannan voit käyttää Lisää eDTUs täyttävän tarvittaessa. Light Lataa tietokannat tarjoaman pienempi ja tietokantoja ei ole kuormitus tarjoaman ei ole eDTUs. Resurssien koko resurssivarantoon sijaan yksittäisen tietokantojen valmistelu yksinkertaistaa hallinnan tehtävät. Sekä sinulla altaan ennakoitavissa budjetin.

Lisää eDTUs voidaan lisätä aiemmin ryhmään, tietokanta ei ole käyttökatkot tai joustavasti varannon tietokantoja ei ole vaikutusta. Vastaavasti jos ylimääräinen eDTUs et enää tarvitse ne voidaan poistaa aiemmin varannon milloin tahansa samanaikaisesti. Voit lisätä tai vähentää tietokantojen ryhmään. Jos tietokanta on laadukkaampi kohdassa-käyttävien resursseja, siirrä se.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Miten voit määrittää omat työmäärää tarvitsemia DTUs määrä?

Jos etsit tapoja siirtää aiemmin luodun paikallisen tai SQL Server virtuaalikoneen työmäärää Azure SQL-tietokantaan, voit käyttää [DTU Laskimen](http://dtucalculator.azurewebsites.net/) lähentää tarvitaan DTUs määrä. Aiemmin Azure SQL-tietokanta-kuormituksen voit käyttää [SQL-tietokannan kyselyn suorituskykyä tietoja](sql-database-query-performance.md) ymmärtää tietokannan resurssin kulutus (DTUs) saat optimoiminen havainnollistamiseen tarkempaa tietoja. Voit käyttää myös [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV resurssin kulutus tiedot edellisen tunnin. Voit myös luettelon näkymän [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) voidaan myös suorittaa kysely samat tiedot hakeminen viimeisten 14 päivän aikana, vaikka osoitteessa alemman tarkkuuden viiden minuutin keskiarvojen.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Mistä tiedän, jos minulla voi hyödyntää joustavasti varannon resursseja?

Jakavat ovat sopiviksi tietokantojen tietyn käyttö kuvioilla suuri määrä. Tietyn tietokannan tämä kaava on argumenttien kuvaaman pienen keskimääräinen käyttö suhteellisen epäsäännölliset käyttö piikkarit kanssa. SQL-tietokantaan käsittelee tietokantojen aiemmin luotuun SQL-tietokantapalvelinta historiallisten resurssien käyttö ja suosittelee tarvittavat resurssivarantoon määritykset Azure-portaalissa automaattisesti. Lisätietoja on artikkelissa [Kun joustavasti tietokannan resurssivarantoon käytetään?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Mitä tapahtuu, kun voin osumien oma suurin DTUs

Suorituskyvyn tasoon kalibroitu ja määräytyvät suorittaa valitun palvelun taso ja suorituskyky-tason sallittu enimmäismäärä rajoitusten tietokannan havainnollistamiseen tarvitaan resursseja. Jos havainnollistamiseen on pallolla jossakin suorittimen ja tietojen IO/Log IO rajoitukset rajoitukset, saat jatkossakin resurssit tasolla suurin sallittu taso, mutta näet todennäköisesti ohjeaiheessa parannettu viiveitä kyselyjen. Nämä raja-arvot eivät aiheuta virheitä, mutta sen sijaan, että heikentyneen työmäärää, ellei Hidastamista tulee vakavia ja kyselyjen Käynnistä ajoitus. Jos ovat pallolla rajoitukset suurin sallittu Rinnakkaisten käyttäjien istuntojen/pyyntöjen (työntekijän viestiketjuissa siirtyminen), näet eksplisiittinen virheitä. Lue lisätietoja raja resurssien kuin suorittimen, muistin, tiedot i/o [Azure SQL-tietokanta resurssien rajoitukset](sql-database-resource-limits.md) ja tapahtuman log i/o.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja DTUs ja eDTUs käytettävissä erillinen tietokantojen ja joustavasti jakavat on [palvelutaso](sql-database-service-tiers.md) .
- Lue lisätietoja raja resurssien kuin suorittimen, muistin, tiedot i/o [Azure SQL-tietokanta resurssien rajoitukset](sql-database-resource-limits.md) ja tapahtuman log i/o.
- Katso [SQL-tietokannan kyselyn suorituskykyä tietoja](sql-database-query-performance.md) ymmärtää (DTUs) kulutus.
- Artikkelissa [SQL-tietokantaan ensisijainen yleiskatsaus](sql-database-benchmark-overview.md) ymmärtää kehitysmenetelmistä takana OLTP ensisijainen työmäärää DTU sekoitus määrittämiseen.