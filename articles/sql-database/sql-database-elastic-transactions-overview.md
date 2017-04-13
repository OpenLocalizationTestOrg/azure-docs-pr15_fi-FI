<properties
   pageTitle="Jaetut tapahtumat yli cloud tietokannat"
   description="Yleistä joustavasti tietokannan tapahtumat Azure SQL-tietokanta"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Jaetut tapahtumat yli cloud tietokannat

Joustavasti tietokannan tapahtumia Azure SQL-tietokanta (SQL DB) avulla voit suorittaa tapahtumia, jotka ulottuvat useiden tietokantojen SQL DB. Joustavasti tietokannan tapahtumia SQL-tietokannan käyttäminen ADO .NET .NET ‑sovelluksissa on käytettävissä ja integroida tuttuja ohjelmoinnin kokemus [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) luokkien avulla. Kirjaston asentamisesta [.NET Framework 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

Paikallinen kuten tilanne yleensä pakollinen käynnissä Microsoft Distributed tapahtuman koordinaattien (MSDTC). Koska MSDTC ei ole käytettävissä Platform-muodossa--palvelusovelluksen Azure-, mahdollisuus hajautettu tapahtumien järjestämiseen on nyt suoraan integroitu SQL-tietokantaan. Sovellusten voit muodostaa yhteyden Käynnistä hajautettu tapahtumat minkä tahansa SQL-tietokantaan ja tietokantojen läpinäkyvä koordinoi hajautettu tapahtuman seuraavassa kuvassa esitetyllä tavalla. 

  ![Jaetut tapahtumat Azure SQL-tietokanta joustavasti tietokannan tapahtumien kanssa ][1]

## <a name="common-scenarios"></a>Yleisiä tilanteita, joissa

SQL-tietokannan joustavasti tietokannan tapahtumia mahdollistavat sovellusten atomisia muutosten eri SQL-tietokantoja tallennettuja tietoja. Esikatselu keskitytään asiakkaan kehittäminen kokemukset C# ja .NET. T-SQL käyttämällä palvelinpuolen-toiminto on suunniteltu myöhemmin uudelleen.  
Joustavasti tietokannan tapahtumia kohteena seuraavissa tilanteissa:

* Usean tietokantasovelluksia Azure-tietokannassa: Tämä skenaario tiedot pystysuunnassa osioidaan yli useiden tietokantojen SQL DB siten, että erilaisia tiedot ovat eri tietokannat. Jotkin toiminnot vaativat muutokset tietoja, jotka on käytettävissä kaksi tai useampi tietokanta. Sovellus käyttää joustavasti tietokannan tapahtumien järjestämiseen muutokset yli tietokantojen ja varmista, että atomisuuden.

* Azure sharded tietokantasovelluksia: Tämä skenaario tietojen tason käyttää [joustavasti tietokannan asiakkaan kirjasto](sql-database-elastic-database-client-library.md) tai itse sharding osion vaakasuunnassa yli useita tietokantoja SQL DB-tietoihin. Ääniä käyttötapauksen on suorittaa atomisia muutoksia sharded usean vuokraajan-sovelluksen, kun muutoksia kattavat alihallinnat tarvitsee. Ajattele esimerkiksi siirtäminen yhdestä vuokraajasta toiseen, sekä eri tietokantojen asu. Toisessa tapauksessa on tarkasti rajattuja sharding sopimaan kapasiteetin tarpeita suuri vuokraajan, joka puolestaan yleensä tarkoittaa, että jotkin atomisia toiminnot on venyttämällä useita tietokantoja käytetään samassa alihallinnassa. Kolmas tapaus on viittaamaan tiedoissa on yli tietokantojen replikoida atomisia päivitykset. Atomisia, tapahtuma-toimintojen pitkin viivat voi nyt koordinoi yli useita tietokantoja, joissa esikatselu.
Joustavasti tietokannan tapahtumia käytät kaksivaiheinen vahvistus tapahtuman atomisuuden tietokantojen välillä. On hyvin ratkaistavaan tapahtumia, joihin sisältyy alle 100 tietokantojen kerrallaan, yksi tapahtumasta. Nämä raja-arvot eivät ole voimassa, mutta jokin toiminta suorituskyky ja success korvaukset joustavasti tietokannan tapahtumia heikkenee, kun nämä raja-arvot yli.


## <a name="installation-and-migration"></a>Asennus- ja siirtäminen

Joustavasti tietokannan tapahtumien SQL DB-toiminnot ovat käytettävissä .NET-kirjastojen System.Data.dll ja System.Transactions.dll päivitykset. DLL-tiedostot varmistaa, että kaksivaiheinen vahvistus on käytössä tarpeelliset atomisuuden. Aloita käyttäminen joustavasti tietokannan tapahtumia sovellusten kehittämiseen, asenna [.NET Framework 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) tai sitä uudempi versio. Kun suoritat aiemmassa versiossa .NET framework-tapahtumat epäonnistuu edistää tapahtumaan ja poikkeuksen korotetaan.

Asennuksen jälkeen voit käyttää jaettua tapahtuman ohjelmointirajapinnan System.Transactions SQL DB-yhteyttä. Jos sinulla on nämä ohjelmointirajapinnan käyttäminen MSDTC sovellukset, riittää, että uudelleen .NET 4.6 aiemmin sovellustesi asennettuasi 4.6.1 Framework. Jos projektisi kohteen .NET 4.6, niissä käytetään automaattisesti versiosta Framework päivitetyt dll-kirjastoista ja jaettu Ohjelmointirajapinnan soittaa SQL DB-yhteyksien yhdessä tapahtuman onnistuu nyt.

Muista, että joustavasti tietokannan tapahtumia ei tarvita MSDTC asentaminen. Sen sijaan joustavasti tietokannan tapahtumia hallitaan suoraan mukaan ja SQL DB. Tämä merkittävästi yksinkertaistaa cloud skenaariot, koska MSDTC käyttöönoton ei tarvitse lisätä Jaetut tapahtumat käyttäminen SQL DB. Osa 4 kerrotaan tarkemmin, käyttöönottamisesta joustavasti tietokannan tapahtumia ja edellyttää .NET framework, ja Azure cloud sovellukset.

## <a name="development-experience"></a>Kehittäminen kokemus

### <a name="multi-database-applications"></a>Usean tietokantasovelluksia

Seuraava näyte koodi käyttää tuttuja ohjelmoinnin kokemus .NET System.Transactions. TransactionScope luokan muodostaa .NET ympäristön tapahtuma. ("Ympäröivän tapahtuman" on sellainen, joka sijaitsee nykyisen säikeen.) Kaikki yhteydet, jotka on avattu TransactionScope sisällä osallistu tapahtuma. Jos eri tietokantojen osallistua ylemmän tason tapahtuman automaattisesti hajautettu tapahtumaan. Tapahtuman tulos ohjataan suorittamiseen osoittamaan vahvistamista laajuuden määrittämisestä.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Sharded tietokantasovelluksia
 
SQL-tietokannan joustavasti tietokannan tapahtumia tukea myös yhteensovittamisesta Jaetut tapahtumat, jossa käytät joustavasti tietokannan asiakkaan kirjaston OpenConnectionForKey-menetelmää avaa skaalatun ulos tietojen yhteydet taso. Harkitse tapauksissa tarvittaessa takaa tapahtumien yhdenmukaisuuden muutosten yli useita eri sharding arvoja. Yhteyksien isännöinnin eri sharding avainarvot shards ovat se käyttämällä OpenConnectionForKey. Yleiset tapauksessa yhteyksiä voi olla eri shards siten, että varmistaa, että tapahtumien oikeudet edellyttää tapahtumaan. Seuraava koodi malli on kuvattu tämän menetelmän. Vaiheissa oletetaan, että muuttujaan, jonka shardmap käytetään esittämään shard kartan joustavasti tietokannan asiakkaan kirjastosta:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>.NET asennuksen Azure pilvipalveluihin

Azure tarjoaa useita tarjouksia host .NET-sovelluksiin. Eri versioiden vertailu on käytettävissä [sovelluksen Azure-palvelu, Cloud Services ja näennäiskoneiden vertailu](../app-service-web/choose-web-site-cloud-service-vm.md). Tarjoamiseksi Vieras OS on pienempi kuin .NET 4.6.1 pakollinen joustavasti tapahtumille, sinun on päivitettävä käyttöösi 4.6.1 Vieras OS. 

Azure App palveluille Vieras OS päivityksiä ei tällä hetkellä tueta. Saat Azuren näennäiskoneiden, kirjaudu yksinkertaisesti AM ja suorita uusin .NET framework asennusohjelma. Azure pilvipalveluihin sinun on sisältämään käyttöönoton käynnistys-tehtäviksi uudempaan .NET-version asennus. Käsitteitä ja vaiheet on kuvattu [Asentaa .NET Cloud palvelun rooliin](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Huomaa, että .NET 4.6.1 asennusohjelma saattaa edellyttää enemmän väliaikaisten automaattisesti käynnistyvää aikana Azure cloud Services kuin asennusohjelma .NET 4.6 varten. Jotta asennus onnistuu, tarvitset niin, että Azure pilvipalvelussa ServiceDefinition.csdef tiedostossa LocalResources osan ja ympäristöasetuksia käynnistys-tehtävän väliaikaisten seuraava näyte esitetyllä tavalla:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Useiden palvelinten tapahtumat

Joustavasti tietokannan tapahtumia tuetaan eri looginen palvelinten Azure SQL-tietokantaan. Tapahtumat toimintojen looginen palvelimen rajat, kun osallistuvien palvelimet on kirjoitettava keskinäinen viestintä yhteyden. Kun viestintä-yhteys on muodostettu, minkä tahansa tietokannan kaikista kaksi palvelimet voivat osallistua joustavasti tapahtumat tietokantojen toisen palvelimen kanssa. Tapahtumia, jonka kesto on enemmän kuin kaksi loogista palvelinta viestintä yhteyden on oltava tuloista kaikki loogiset palvelinten paikassa.

Palvelin-yhteyksien joustavasti tietokannan tapahtumia yhteyksien hallinta PowerShellin cmdlet-komennot avulla:

* **Uusi AzureRmSqlServerCommunicationLink**: cmdlet avulla voit luoda uuden viestintä suhteen kaksi looginen Azure SQL-DB-palvelimet. Yhteys on symmetrisen eli molempien palvelinten voivat aloittaa tapahtumat-palvelimen kanssa.
* **Hae AzureRmSqlServerCommunicationLink**: cmdlet avulla voit hakea viestintätapahtumien yhteyksien ja niiden ominaisuudet.
* **Poista AzureRmSqlServerCommunicationLink**: cmdlet avulla voit poistaa aiemmin luotua viestintä-yhteyttä. 


## <a name="monitoring-transaction-status"></a>Tapahtuman tilan seuranta

Dynaaminen hallinta näkymien avulla (DMVs) SQL DB-näytön tilan ja edistymisen tapahtumien jatkuvan joustavasti tietokannan. Kaikki liittyvät tapahtumat DMVs liittyvät SQL DB hajautettu tapahtumia varten. Voit etsiä vastaavan luettelo DMVs tähän: [tapahtuman liittyvät dynaaminen hallinta näkymät ja toiminnot (Transact-SQL)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Nämä DMVs ovat erityisen hyödyllisiä:

* **sys.dm\_tran\_active\_tapahtumat**: Näyttää aktiivisena tapahtumia ja niiden tiloista. UOW (työmäärän yksikkö)-sarake voidaan tunnistaa eri lapsen tapahtumat, jotka kuuluvat samaan jaettuun tapahtumaan. Kaikki tapahtumat saman hajautettu tapahtumasta toteuttaa saman UOW arvon. On lisätietoja [DMV ohjeissa](https://msdn.microsoft.com/library/ms174302.aspx) .
* **sys.dm\_tran\_tietokannan\_tapahtumat**: on lisätietoja tapahtumia, kuten tapahtuman lokiin sijaintia. On lisätietoja [DMV ohjeissa](https://msdn.microsoft.com/library/ms186957.aspx) .
* **sys.dm\_tran\_lukitukset**: tietoja siitä, meneillään oleviin tapahtumat varatuista lukituksia. On lisätietoja [DMV ohjeissa](https://msdn.microsoft.com/library/ms190345.aspx) .

## <a name="limitations"></a>Rajoitukset 

Seuraavat rajoitukset koskevat tällä hetkellä joustavasti tietokannan tapahtumia SQL DB:

* Vain tapahtumat yli SQL DB ominaisuudet ovat tuettuja. Muut [X / Avaa XA](https://en.wikipedia.org/wiki/X/Open_XA) resurssin palveluntarjoajat ja tietokantojen ulkopuolella SQL DB et voi osallistua joustavasti tietokannan tapahtumia. Tämä tarkoittaa, että joustavasti tietokannan tapahtumia et voi venyttää yli paikallinen ja SQL Server Azure SQL-tietokannat. Hajautettu tapahtumien paikallinen jatkaa MSDTC. 
* Tuetaan vain asiakas koordinoi tapahtumat .NET-sovelluksesta. T-SQL esimerkiksi ALOITTAA DISTRIBUTED tapahtuman palvelinpuolen tuki on suunniteltu, mutta ei ole vielä käytettävissä. 
* Tuetaan vain Azure SQL DB Vipuventtiili V12 tietokannat.
* Tapahtumien WCF-palveluiden välillä ei tueta. Sinulla on esimerkiksi WCF-palvelumenetelmä suorittaa tapahtuman. Kehystäminen tapahtuman alueella kutsu epäonnistuu [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception)nimellä.

## <a name="additional-resources"></a>Lisäresursseja

Käytä joustavasti tietokannan ominaisuuksia ei ole vielä Azure-sovellusten? Tutustu Microsoftin [Dokumentaatio](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Kysymyksiä Ota saavuttaminen us [SQL-tietokanta-keskustelupalstalla](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ja ehdottaa ominaisuuksia, lisää ne [SQL-tietokantaan palautetta keskustelupalstalle](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



