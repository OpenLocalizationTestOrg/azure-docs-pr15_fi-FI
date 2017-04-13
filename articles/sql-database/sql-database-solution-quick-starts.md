<properties
   pageTitle="Azure SQL-tietokannan ratkaisu pika käynnistyy | Microsoft Azure"
   description="Lisätietoja Azure SQL-tietokannan ratkaisut"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# <a name="explore-azure-sql-database-solution-quick-starts"></a>Tutustu Azure SQL-tietokannan ratkaisu pika käynnistyy

Tässä artikkelissa on yleiskatsaus Azure SQL tietokanta ratkaisu nopeasti käynnistyy. Nämä nopeasti käynnistää sijaitsevat GitHub SQL Server-näytteiden säilöön ja kuvaavat todellisen skenaariot perusteella täydellinen ratkaisu-SQL-tietokantaan. Katso yksinkertainen vaiheittaisia ohjeita, jotka kuvaavat tietyn SQL-tietokanta-ominaisuuden, [Tutustu Azure SQL-tietokanta-opetusohjelmat](sql-database-explore-tutorials.md).

## <a name="try-the-wingtiptickets-demo-and-hands-on-lab"></a>Kokeile WingTipTickets esittely ja käytännön testiympäristössä

[Azure SQL-tietokannan WingTipTickets](https://github.com/microsoft/wingtiptickets) esittely ja käytännön kurssin osoitettava Azure SQL-tietokanta ja sovelluksen Azure Hakupohjaisten malli, jota käytetään myydä uudelleenkäytettäviksi liput.


## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools"></a>Kerää ja seurata käyttö resurssitietojen useita jakavat yli

[Ratkaisu Pika-aloitus: joustavasti resurssivarantoon telemetriatietojen PowerShellin](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) ratkaisu ja seuranta SQL-tietokantaan resurssien käyttö-tilauksen useita jakavat yli. Kun sinulla on runsaasti tietokantojen tilauksen, on hankalaa seurannassa kunkin joustavasti resurssivarantoon erikseen.

Voit ratkaista ongelman, voit yhdistää SQL-tietokannan PowerShellin cmdlet-komennot ja resurssien käyttötietojen keräämiseen useita jakavat ja tietokantansa T-SQL-kyselyjä. Näin voit valvoa ja analysoida resurssien käyttö entistä tehokkaammin.

Tässä pikaoppaassa on joukko PowerShell-komentosarjojen ja T-SQL-kyselyjä, sekä ohjeet ratkaisun mitä ja kuinka se.

## <a name="get-started-with-elastic-database-in-an-saas-scenario"></a>Aloittaminen joustavasti tietokannan SaaS-skenaario

 [Ratkaisu Pika-aloitus: joustavasti resurssivarantoon mukautetun raporttinäkymä SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) tarjoaa ratkaisun ohjelmisto-muodossa--ratkaisun (SaaS) tilanne, jossa SQL-tietokantaan antamaan edullinen ja skaalattava tietokannan taustassa SaaS sovelluksen joustavasti tietokanta-toiminnon avulla.

Tämä ratkaisu kertovat käyttöönoton verkkosovellukseen. Web-sovelluksen avulla voit havainnollistaa, joka on luotu joustavasti tietokannan kuormituksen luontitoiminto, joka käyttää mukautettuja Raporttinäkymät-ikkunan, jotka täydentävät Azure portaalin kuormituksen.

Tässä pikaoppaassa on kuormituksen luontitoiminnon ja seurantaa web App-sovelluksen asiakirjat sovelluksen mitä ja miten sitä käytetään.

## <a name="create-an-azure-sql-database-by-using-code-first-development-and-the-entity-framework"></a>Luoda Azure SQL-tietokantaan koodin ensimmäisen kehittämisen ja kohteen Framework

Video- ja näyte [Koodin ensimmäisen uuteen tietokantaan](https://msdn.microsoft.com/data/jj193542.aspx) esitellään koodin ensimmäisen kehitystä, joka sopii uuteen tietokantaan. Tässä skenaariossa jotakin tietokannan, joka ei ole, mutta jotka luodaan koodin ensimmäisen mukaan. Voit myös skenaarion luo tyhjän tietokannan, johon koodi ensimmäisen Lisää uudet taulukot.

Koodin mahdollistaa ensin mallin määrittäminen käyttämällä C#- tai Visual Basic .NET luokkien mukaan. Voit suorittaa valinnainen lisämäärityksiä käyttämällä määritteitä luokat ja ominaisuudet tai fluent Ohjelmointirajapinnan käyttäminen.

## <a name="integrate-elastic-database-tools-into-an-entity-framework-application"></a>Integroi joustavasti Tietokantatyökalut kohteen Framework-sovellukseen

[Kohteen Framework joustavasti tietokannan asiakkaan kirjastosta](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) otosten näyttää muutokset, jotka haluat integroida [joustavasti Tietokantatyökalut](sql-database-elastic-scale-get-started.md)kohteen Framework sovellukseen. Keskitytään sähköpostiviestiä [shard Yhdistä hallinta](sql-database-elastic-scale-shard-map-management.md) ja [tietojen riippuva reititys](sql-database-elastic-scale-data-dependent-routing.md) kohteen Framework koodin ensimmäisestä tavasta kanssa.

[Uusi tietokanta-otoksen EF ensimmäisen koodi](http://msdn.microsoft.com/data/jj193542.aspx) on käynnissä olevassa esimerkissä koko tässä esimerkissä. Esimerkki tunnus, jolla tämän asiakirjan mukana on näytteiden Visual Studio MALLIKOODEJA joustavasti tietokannan Työkalut-ryhmän osa.

## <a name="integrate-elastic-database-tools-with-row-level-security"></a>Integroi joustavasti Tietokantatyökalut rivin käyttäjätason suojauksen

[Multitenant sovellusten joustavasti Tietokantatyökalut ja rivin käyttäjätason suojauksen](sql-database-elastic-tools-multi-tenant-row-level-security.md) näyttää muutokset, sinun täytyy tehdä kohteen Framework sovelluksen [joustavasti Tietokantatyökalut](sql-database-elastic-scale-get-started.md) integroida [rivin käyttäjätason suojauksen](https://msdn.microsoft.com/library/dn765131). Tässä esimerkissä kuvataan tekniikalla käyttäminen yhdessä luomaan sovelluksen kanssa, joka tukee multitenant shards erittäin skaalattava tietojen taso.

Voit tehdä tämän käyttämällä ADO.NET SqlClient tai kohteen Framework. Tässä esimerkissä laajentaa [kohteen Framework joustavasti tietokannan asiakkaan kirjastosta](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) lisäämällä multitenant shard tietokantojen tuki.
Se muodostaa yksinkertaisen console-sovelluksen luomiseen blogit ja viestit, neljä alihallinnat ja multitenant shard tietokannoista.

## <a name="create-online-surveys-with-the-tailspin-surveys-application"></a>Luo online kyselyt Tailspin kyselyt-sovelluksen kanssa

Tämä [malli Tailspin kyselyt-sovelluksen](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) on multitenant web-sovelluksen, kyselyt, jonka nimi, jonka avulla käyttäjät voivat luoda online kyselyt. Otosten korjaa joitakin keskeisiä epävarma multitenant-sovelluksessa, kuten kirjautumisen, todennus, todennus ja sovelluksen roolit käyttäjätietojen hallinta.

## <a name="learn-about-the-latest-security-features-of-sql-database-with-the-contoso-clinic-demo-application"></a>Lisätietoja uusimman suojausominaisuudet SQL-tietokantaan Contoso kurssi esittely-sovelluksen kanssa

Tämä [kurssi esittely Contoso-sovelluksen](https://github.com/Microsoft/azure-sql-security-sample) esitellään uusimman suojausominaisuudet SQL-tietokantaan.

## <a name="next-steps"></a>Seuraavat vaiheet

[Tutustu Azure SQL-tietokanta-opetusohjelmat](sql-database-explore-tutorials.md)
