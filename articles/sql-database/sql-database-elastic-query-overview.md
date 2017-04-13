<properties
    pageTitle="Azure SQL-tietokantaan joustavasti tietokannan kyselyn yleiskatsaus | Microsoft Azure"
    description="Yleistä joustavasti kysely-ominaisuus"    
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
    ms.date="04/27/2016"
    ms.author="torsteng" />

# <a name="azure-sql-database-elastic-database-query-overview-preview"></a>Azure SQL-tietokantaan joustavasti tietokannan kyselyn yleiskatsaus (ennakkoversio)

(Valitse Esikatselu) joustavasti tietokannan kysely-ominaisuuden avulla voit suorittaa Transact-SQL-kysely, joka ulottuu useiden tietokantojen Azure SQL tietokanta (SQLDB). Sen avulla voit suorittaa rajat-tietokantakyselyt remote taulukoita ja muodostaa Microsoftin ja kolmansien osapuolten työkalut (Excel, PowerBI, Tableau, jne.) tietojen tasoa useiden tietokantojen kysely. Tämän ominaisuuden avulla voi skaalata, kyselyt suurien tietomäärien tasoa SQL-tietokantaan ja esittää tulokset business intelligence (BI) raporteissa.

## <a name="documentation"></a>Ohjeet

* [Usean tietokantakyselyt käytön aloittaminen](sql-database-elastic-query-getting-started-vertical.md)
* [Raportin yli skaalata ulos cloud tietokannat](sql-database-elastic-query-getting-started.md)
* [Kysely sharded cloud-tietokannat (vaakasuunnassa osioitu)](sql-database-elastic-query-horizontal-partitioning.md)
* [Kysely cloud tietokantojen kanssa eri rakenteita (pystysuunnassa osioitu)](sql-database-elastic-query-vertical-partitioning.md)
* [SP\_suorittaa \_remote](https://msdn.microsoft.com/library/mt703714)


## <a name="why-use-elastic-queries"></a>Joustavasti kyselyjen käyttötarkoitus

**Azure SQL-tietokanta**

Azure SQL-tietokantoja kokonaan T-SQL-kysely. Näin vain luku-kyselyä remote tietokannat. Tämä on vaihtoehto nykyisen paikallisen SQL Server-asiakkaat voivat siirtää sovellusten kolmen ja four osa nimiä tai SQL DB linkitetyn palvelimen avulla.

**Käytettävissä standard ylätasolla** Joustavasti kyselyn tuetaan vakio suorituskyvyn ylätasolla lisäksi Premium suorituskyvyn taso. Katso esikatselu rajoitukset alla alemman suorituskyvyn tasoa rajoituksia suorituskyvyn käyttöön.

**Push-remote tietokantoihin**

Joustavasti kyselyt voi nyt push SQL-parametreja remote tietokantoihin suorittamista varten.

**Tallennetun toimintosarjan suorittamisen**

Suorita remote tallennetun toimintosarjan puhelut tai remote funktioiden käyttäminen [sp\_suorittaa \_remote](https://msdn.microsoft.com/library/mt703714).

**Joustavuus**

Ulkoiset taulukot joustavasti Query voi nyt viitata remote taulukoita, joissa on eri rakennetta tai taulukkonimeä.

## <a name="elastic-database-query-scenarios"></a>Joustavasti tietokannan kyselyn skenaariot

Tavoitteena on helpottaa kyseltäessä skenaariot, jossa useiden tietokantojen osallistuja rivejä yhteen yleinen tulokseen. Kyselyn voi laaditut joko käyttäjän tai sovelluksen suoraan tai epäsuorasti työkaluja, joilla on yhteydessä tietokantaan kautta. Tämä on erityisen hyödyllinen luotaessa raportteja kaupallista BI tai tietojen integrointi työkalut – tai jokin sovellus, jota ei voi muuttaa. Joustavasti Queryn avulla voit tehdä kyselyn useita tietokantoja, joissa tuttuja SQL Server-yhteyksien kokemus työkalujen, kuten Excel, PowerBI, Tableau tai Cognos yli.
Joustavasti kyselyn mahdollistaa helpon pääsyn koko kokoelman tietokantojen kyselyjen SQL Server Management Studiossa tai Visual Studio kautta ja helpottaa rajat-tietokannan kyselyt kohteen Framework tai ORM muiden ympäristöjen. Kuvassa 1 on tilanne jossa aiemmin luotua cloud-sovellusta (joka käyttää [joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-database-client-library.md)) muodostaa skaalata ulos tietojen ylätasolla ja joustavasti kyselyn käytetään rajat tietokannan raportointiin.

**Kuva 1** Joustavasti tietokantakyselyn käytettyyn skaalata ulos tietojen taso

![Joustavasti tietokantakyselyn käytettyyn skaalata ulos tietojen taso][1]

Asiakkaan tilanteita, joissa joustavasti kyselyn varaavat seuraavat topologioissa:

* **Pystysuuntainen jakaminen – rajat tietokantakyselyt** (Topologian 1): tiedot on osioitu pystysuunnassa määrä tietojen taso tietokantojen välillä. Yleensä eri tiedostojoukot taulukot sijaitsevat eri tietokannat. Tämä tarkoittaa, että rakenne on erilainen eri tietokantojen. Esimerkiksi kaikki taulukot varaston ovat yhden tietokannan vaikka kaikki accounting liittyvät taulukot ovat toisen tietokannan. Yleisiä Käytä palvelupyynnöt tämän topologian edellyttävät kysely tai kääntää raporttien yli useiden tietokantojen taulukot.
* **Vaakasuuntainen jakaminen – Sharding** (Topologian 2): tiedot osioidaan vaakasuunnassa, riviä jakaa skaalatun ulos tietojen taso. Tämän menetelmän rakenne on identtiset osallistuvien piilotetusta. Tämän menetelmän kutsutaan myös "sharding". Sharding voidaan suorittaa ja hallittujen käyttäminen (1) joustavasti tietokannan Työkalut kirjastoja tai (2) itse sharding. Joustavasti kyselyn käytetään kyselyn tai kääntää raporttien monta shards yli.

> [AZURE.NOTE] Joustavasti tietokantakyselyn toimii parhaiten ajoittaiset raportointi skenaarioissa, johon useimmat käsittelyn voidaan suorittaa tietojen taso. Paksu raportoinnin työmääriä tai tietojen laajemmissa skenaariot monimutkaisia kyselyjen avulla kannattaa myös käyttää [Azure SQL-tietovarasto](https://azure.microsoft.com/services/sql-data-warehouse/).


## <a name="elastic-database-query-topologies"></a>Joustavasti tietokannan kyselyn topologioissa

### <a name="topology-1-vertical-partitioning--cross-database-queries"></a>Topologian 1: Pystysuuntainen jakaminen – rajat-tietokannan kyselyt

Aloita coding ohjeaiheessa [käytön aloittaminen rajat - tietokantakyselyn (pystysuora jakaminen)](sql-database-elastic-query-getting-started-vertical.md).

Joustavasti kyselyn avulla voidaan helpottaa tietojen käytettävissä SQLDB muita tietokantoja SQLDB tietokanta sijaitsee. Näin kyselyjen viittaamaan minkä tahansa remote SQLDB tietokannan taulukoiden tietokannasta. Ensimmäiseksi on määrittää ulkoisen tietolähteen kunkin etätietokantaan. Ulkoisen tietolähteen määritetään paikallinen tietokanta, josta haluat käyttöösi taulukot sijaitsevat etätietokantaan. Muutoksia ei tarvittavat etätietokantaan. Tyypillinen pystysuunnassa osioinnin skenaarioille joihin eri tietokantoja on eri rakenteita joustavasti kyselyjen avulla voidaan toteuttaa yleisiä Käytä tapauksissa kuten viittaus tietojen käytön ja rajat-tietokannan kyselyt.

**Viitetiedot**: topologian 1 käytetään viittaus tietojen hallinta. Alla olevassa kuvassa kahden taulukon (T1 ja T2) viittaus tietoja säilytetään varatun tietokannan. Joustavasti Queryn avulla voit nyt käyttää taulukoiden T1 ja T2 etäyhteyden muita tietokantoja kuvassa osoitetulla tavalla. Käytä topologian 1, jos viittauksen taulukot ovat pieniä tai etätietokoneen kyselyjen viittaus taulukkoon on valikoiva predikaatit.

**Kuva 2** Pystysuuntainen jakaminen - joustavasti query-kyselyn viitetiedot käyttäminen

![Pystysuuntainen jakaminen - joustavasti query-kyselyn viitetiedot käyttäminen][3]

**Usean tietokannan kyselyt**: Lisää joustavasti kyselyt Ota käyttöön tapauksissa, jotka edellyttävät kyselyt useita SQLDB tietokantoja yli. Kuva 3 näkyy neljä eri tietokantojen: CRM, varaston, henkilöstön ja tuotteet. Suorittaa yhteen tietokantojen kyselyjen on myös käyttää yhtä tai kaikki muut tietokannat. Joustavasti Queryn avulla voit määrittää tässä tapauksessa tietokannan suorittamalla muutaman yksinkertaisen DDL-lauseita kunkin neljä tietokannat. Erikseen, määritysten jälkeen remote taulukon käyttöä on pelkästään viittaaminen paikallisen taulukon T-SQL-kyselyissä tai Liiketoimintatieto-työkaluista. Tämän menetelmän suositellaan, jos remote kyselyt eivät palauta suuri tuloksia.

**Kuva 3** Pystysuuntainen jakaminen – Lisää joustavasti query-kyselyn käyttö useiden tietokantojen

![Pystysuuntainen jakaminen – Lisää joustavasti query-kyselyn käyttö useiden tietokantojen][4]

### <a name="topology-2-horizontal-partitioning--sharding"></a>Topologian 2: Vaakasuuntainen jakaminen – sharding

Raportoinnin tehtävät päälle sharded, eli vaakasuunnassa osioitu joustavasti kyselyn avulla tietojen taso edellyttää [joustavasti tietokannan shard kartan](sql-database-elastic-scale-shard-map-management.md) edustavan tietojen tason tietokannat. Yleensä vain yhden shard kartan käytetään tässä skenaariossa ja erillinen tietokanta on joustavasti Kyselyominaisuudet on aloituskohtaa kyselyt-raportointia varten. Vain tämä erillinen tietokanta on pääsy shard kartan. Kuva 4 on kuvattu tämän topologiasta ja joustavasti kysely tietokanta ja shard rakenneruudun sen asetukset. Tietojen tason tietokannoissa voi olla Azure SQL-tietokanta- versiota. Saat lisätietoja joustavasti tietokannan asiakas-kirjaston ja shard karttojen luomiseen [Shard yhdistää hallinta](sql-database-elastic-scale-shard-map-management.md).

**Kuva 4** Vaakasuuntainen jakaminen - käyttämällä joustavasti kyselyn päälle sharded tietojen tasoa raportointia varten

![Vaakasuuntainen jakaminen - käyttämällä joustavasti kyselyn päälle sharded tietojen tasoa raportointia varten][5]

> [AZURE.NOTE] Erillinen joustavasti tietokannan kysely tietokanta on oltava SQL DB Vipuventtiili v12 tietokanta. Ei ole mitään rajoituksia käyttöön shards itse.

Aloita coding ohjeaiheessa [käytön aloittaminen joustavasti tietokantakyselyn, vaakasuora jakaminen (sharding)](sql-database-elastic-query-getting-started.md).

## <a name="implementing-elastic-database-queries"></a>Yksityiskohtaiset joustavasti tietokantakyselyt

Seuraavissa osissa käsitellään toteuttamisesta vaaka- ja pystysuuntaisten osioinnin käyttötavoista joustavasti kyselyn vaiheita. Ne myös katsoa osioinnin eri skenaarioissa yksityiskohtaisempia ohjeissa.

### <a name="vertical-partitioning---cross-database-queries"></a>Pystysuuntainen jakaminen - rajat-tietokannan kyselyt

Seuraavat vaiheet määrittää joustavasti tietokantakyselyt pystysuunnassa osioinnin tilanteita, jotka tarvitsevat samaan rakenteeseen remote SQLDB tietokantojen sijaitsevat taulukkoon käyttöoikeudet:

*    [Luo AVAIMEN](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Luo tietokanta SUODATETUT TUNNISTETIEDON](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    [Luo ja PUDOTA ULKOISEEN TIETOLÄHTEESEEN](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource **RDBMS** tyyppi
*    [Luo ja PUDOTA ULKOISEN taulukon](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Kun olet suorittanut DDL-lauseita, voit käyttää remote taulukon "mytable" aivan kuin se on paikallinen taulukko. Azure SQL-tietokantaan automaattisesti avautuu etätietokanta yhteyden käsittelee etätietokanta-pyyntö ja palauttaa tuloksen.
Saat lisätietoja vaiheet, jotka tarvitaan pystysuora osioinnin skenaarion löytyvät [joustavasti kyselyn pystysuora jakaminen](sql-database-elastic-query-vertical-partitioning.md).  

### <a name="horizontal-partitioning---sharding"></a>Vaakasuuntainen jakaminen - sharding

Seuraavat vaiheet määrittää vaaka osioinnin skenaariot, jotka vaativat yhteyden taulukon joukko, jotka sijaitsevat joustavasti tietokantakyselyt (yleensä) useita remote SQLDB tietokantoja:

*    [Luo AVAIMEN](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
*    [Luo tietokanta SUODATETUT TUNNISTETIEDON](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
*    Luo [shard kartan](sql-database-elastic-scale-shard-map-management.md) edustava käyttämällä joustavasti tietokannan asiakas-kirjaston tietoja oman taso.   
*    [Luo ja PUDOTA ULKOISEEN TIETOLÄHTEESEEN](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource **SHARD_MAP_MANAGER** tyyppi
*    [Luo ja PUDOTA ULKOISEN taulukon](https://msdn.microsoft.com/library/dn935021.aspx) mytable

Kun olet tehnyt nämä vaiheet, voit käyttää vaakasuunnassa osioitua taulukon "mytable" aivan kuin se on paikallinen taulukko. Azure SQL-tietokantaan automaattisesti avautuu useita rinnakkaisia yhteyksiä remote tietokantojen taulukot fyysisesti tallennuspaikkaa käsittelee remote tietokantoja pyynnöt ja palauttaa tuloksen.
Saat lisätietoja vaiheet, jotka tarvitaan vaakasuuntainen osioinnin skenaarion löytyvät [joustavasti kyselyn vaakasuora jakaminen](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="t-sql-querying"></a>T-SQL-kysely
Kun olet määrittänyt ulkoisia tietolähteitä ja ulkoiset taulukot, voit muodostaa tietokannat, jossa määritetty ulkoiset taulukot säännöllisesti SQL Server-yhteyden merkkijonoja. Voit suorittaa T-SQL-lauseita päälle ulkoiset taulukot-yhteyden kanssa alla kuvattujen rajoitukset. Voit etsiä lisätietoja ja esimerkkejä T-SQL-kyselyiden dokumentaatio on artikkeleissa [Vaakasuora jakaminen](sql-database-elastic-query-horizontal-partitioning.md) ja [Pystysuuntainen jakaminen](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Yhteys-työkaluissa
Voit käyttää säännöllisesti SQL Server-yhteyden merkkijonot muodostaaksesi sovellukset ja BI tai tietojen integrointi-Työkalut, joiden ulkoiset taulukot. Varmista, että SQL Server on tuettu työkalu tietolähteenä. Kun yhteys on muodostettu, viitata joustavasti kyselytietokannan tai ulkoiset taulukot tietokannan aivan kuin tehdä kaikki muut SQL Server-tietokantaan, voit muodostaa yhteyden oman-työkalulla.

> [AZURE.IMPORTANT] Azure Active Directoryn avulla joustavasti kyselyjen todennusta ei tueta.

## <a name="cost"></a>Kustannukset

Joustavasti kyselyyn sisällytetään Azure SQL-tietokanta-tietokantojen kustannukset. Huomaa, että topologioissa missä remote tietokantoja ovat eri tietokeskuksen kuin joustavasti kyselyn päätepisteen ovat tuettuja, mutta tietojen juniin remote tietokantojen peritään säännöllisesti [Azure korvaukset](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Esikatselu-rajoitukset
* Ensimmäinen joustavasti-kyselyn suorittaminen voi kestää muutaman minuutin kuluttua vakio suorituskyvyn ylätasolla. Tällä hetkellä tarvitaan lataamaan joustavasti kyselytoimintoja. suorituskykyä on parannettu suurempi suorituskyvyn tasoa kanssa.
* Ulkoisten tietolähteiden tai ulkoisen taulukon SSMS tai SSDT komentosarjan ei vielä tueta.
* Tuo/Vie SQL DB ei vielä tue ulkoisia tietolähteitä ja ulkoiset taulukot. Jos haluat käyttää Tuo/Vie pudota ennen viemistä nämä objektit ja luo ne uudelleen tuomisen jälkeen.
* Joustavasti tietokantakyselyn tukee tällä hetkellä vain lukuoikeudet ulkoiset taulukot. Voit kuitenkin käyttää T-SQL-toimintojen täydet tietokannan jossa ulkoinen taulukko on määritetty. Tämä on hyvä, esimerkiksi jatkuvat käyttäminen, kuten väliaikainen tulokset, valitse < column_list > < local_table > tai määrittää tallennettujen toimintosarjojen joustavasti kysely tietokanta, joka viittaa ulkoiset taulukot.
* Nvarchar(max), lukuun ottamatta LOB tyyppejä ei tueta ulkoisen taulukon määritykset. Voit kiertää tämän ongelman voit luoda näkymän, joka muuttaa sarakemuotoa nvarchar(max) LOB kirjoittaman etätietokanta, määrittää ulkoisen taulukon sijaan perustaulukon näkymää ja liittää se sitten takaisin alkuperäiseen LOB tyyppi kyselyissä.
* Sarakkeen tilastojen päälle ulkoiset taulukot ovat tällä hetkellä tueta. Taulukoiden tilastotiedot ovat tuettuja, mutta se on luotava manuaalisesti.

## <a name="feedback"></a>Palaute
Jaa käyttökokemuksen palautetta heidän joustavasti kyselyjen kanssamme Disqus alla MSDN-keskustelupalstoilta tai Stackoverflow. Emme kiinnostunut kaikenlaisten palautetta palvelu (häiriöitä, karkea reunat-toiminnon puutteiden).

## <a name="more-information"></a>Lisätietoja

Voit etsiä lisätietoja rajat-tietokannan kyselyt ja pystysuuntainen osioinnin skenaariot seuraavat asiakirjat:

* [Usean tietokannan kysely- ja pystysuuntainen osioinnin yleiskatsaus](sql-database-elastic-query-vertical-partitioning.md)
* Kokeile sekä vaiheittaisia opetusohjelma on täydellinen työn Esimerkki käynnissä minuutteina: [käytön aloittaminen rajat - tietokantakyselyn (pystysuora jakaminen)](sql-database-elastic-query-getting-started-vertical.md).


Saat lisätietoja vaakasuora jakaminen ja sharding skenaariot on saatavilla täällä:

* [Vaakasuuntainen jakaminen ja sharding yleiskatsaus](sql-database-elastic-query-horizontal-partitioning.md)
* Yritä sekä vaiheittaisia opetusohjelma on täydellinen työn Esimerkki käynnissä minuutteina: [käytön aloittaminen joustavasti tietokantakyselyn, vaakasuora jakaminen (sharding)](sql-database-elastic-query-getting-started.md).


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
