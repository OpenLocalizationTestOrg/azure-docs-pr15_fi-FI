<properties 
    pageTitle="Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan Transact-SQL | Microsoft Azure" 
    description="Aloita suunniteltu tai suunnittelematon automaattisesti, käyttämällä Transact-SQL Azure SQL-tietokanta" 
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
    ms.workload="data-management"
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Aloita suunniteltu tai suunnittelematon automaattisesti, jossa Transact-SQL Azure SQL-tietokanta


> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [PowerShellin](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Tässä artikkelissa kerrotaan, miten aloittaminen automaattisesti toissijaisen SQL-tietokanta käyttämällä Transact-SQL. Määrittää Geo replikoinnin, on artikkelissa [Määrittäminen Geo-replikoinnin Azure SQL-tietokantaan](sql-database-geo-replication-transact-sql.md).



Voit aloittaa automaattisesti edellyttää seuraavia:

- Kirjautuminen, joka on määritetty ensisijaiseksi DBManager on se geo-toisinto paikallisen tietokannan db_ownership ja olla DBManager, johon määrität Geo replikoinnin kumppanin palvelimesta.
- SQL Server Management Studiossa (SSMS)


> [AZURE.IMPORTANT] On suositeltavaa, että käytät Management Studiossa uusimman version aina pysyvän synkronoituja Microsoft Azure ja SQL-tietokantaan. [Päivitä SQL Server Management Studiossa](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Aloita suunnitellun automaattisesti, toissijainen tietokannan muuttuvat uuden ensisijainen edistäminen

Voit **Muuttaa DATABASE** -lause edistää toissijainen tietokannan muuttuvat uuden ensisijaisen tietokannan suunnitellun tavalla luodun ensisijainen luomisella toissijainen alemmalle tasolle. Tämä lause on suoritettu jossa geo replikoida toissijaisen tietokanta, joka on ylemmälle tasolle sijaitsee Azure SQL-tietokanta looginen palvelimen master-tietokantaan. Tämä toiminto on suunniteltu suunnitellun automaattisesti, kuten aikana DR-työkaluja ja edellyttää, että ensisijainen tietokanta on käytettävissä.

Komento suorittaa seuraavat työnkulun:

1. Siirtyy replikoinnin tilapäisesti synkronoitu tila vuoksi kaikki avoimet tapahtumat on tyhjennetty toissijaisen ja estäminen uusille tapahtumille;

2. Siirtyy Geo replikoinnin yhteistyössä tietokannoista roolit.  

Tässä järjestyksessä takaa, että tietokannoista on synkronoitu ennen roolit Vaihda ja näin ollen ei tietojen menetystä tapahtuu. Tällä aikana molemmat tietokannat eivät ole käytettävissä (järjestykseen 0 25 sekuntia) samalla, kun roolit vaihdetaan lyhyen ajan. Jos ensisijainen tietokannassa on useita toissijainen tietokantoja, komento automaattisesti uudelleen muiden muodostaa uusi ensisijainen secondaries.  Koko-toiminto tekee saapuville alle minuutin Normaali tilanteissa. Lisätietoja on artikkelissa [Muuta TIETOKANTAA (Transact-SQL)](https://msdn.microsoft.com/library/mt574871.aspx) ja [Palvelutasot](sql-database-service-tiers.md).


Seuraavien vaiheiden avulla voit aloittaa suunnitellun automaattisesti.

1. Muodostaa Management Studiossa, jossa geo replikoida toissijaisen tietokanta sijaitsee Azure SQL-tietokanta looginen palvelimeen.

2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. **Muuta TIETOKANTAA** seuraavan lauseen avulla voit siirtyä toissijaisen tietokanta ensisijaisuus.

        ALTER DATABASE <MyDB> FAILOVER;

4. Valitse **Suorita** kysely.

>[AZURE.NOTE] Vain harvoin on mahdollista, että toiminto ei voi suorittaa ja saattaa näkyä jumissa. Tässä tapauksessa käyttäjä voi voimassa automaattisesti-komento ja hyväksy tietojen menettämisen.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Aloita suunnittelematon automaattisesti ensisijainen tietokannasta toissijaisen tietokanta

Voit **Muuttaa DATABASE** -lause edistää toissijainen tietokannan muuttuvat uuden ensisijaisen tietokannan suunnittelematon tavalla pakottaminen alennus aiemmin ensisijaisen luomisella toissijainen kerrallaan, kun ensisijainen databse ei ole enää käytettävissä. Tämä lause on suoritettu jossa geo replikoida toissijaisen tietokanta, joka on ylemmälle tasolle sijaitsee Azure SQL-tietokanta looginen palvelimen master-tietokantaan.

Tämä toiminto on suunniteltu palauttaminen, kun käytettävyys tietokannan palauttaminen on ehdottoman ja kadota tietoja ovat oikein. Pakotetun automaattisesti käynnistyy, kun määritetyn toissijainen tietokannan tulee ensisijaisen tietokannan välittömästi ja aloittaa hyväksymällä kirjoittaminen tapahtumat. Alkuperäinen ensisijainen tietokanta on muodostaa uusi ensisijainen tietokanta kanssa, kun lisäävän varmuuskopioinnin otetaan alkuperäisen ensisijainen tietokannan ja vanha ensisijaisen tietokannan tehdään toissijainen tietokantaan uuden ensisijaisen tietokannan; Tämän jälkeen se on pelkästään synkronoiminen uuden ensisijaisen.

Koska piste-aika palauttaminen ei tue toissijaisen tietokantoja, jos käyttäjä haluaa vahvistetun vanha ensisijainen tietokantaan, joka oli ole kopioitu uusi ensisijainen tietokanta ennen pakotettu vikasietotilaa tietojen palauttaminen, käyttäjän on osallistua tuki palauttaa tämän tiedot ovat hävinneet.

Jos ensisijainen tietokannassa on useita toissijainen tietokantoja, komento automaattisesti uudelleen muiden muodostaa uusi ensisijainen secondaries.

Seuraavien vaiheiden avulla voit aloittaa suunnittelematon automaattisesti.

1. Muodostaa Management Studiossa, jossa geo replikoida toissijaisen tietokanta sijaitsee Azure SQL-tietokanta looginen palvelimeen.

2. Tietokantojen-kansion avaaminen, **Järjestelmän tietokantojen** -kansion laajentaminen, napsauta **perusmuodon**hiiren kakkospainikkeella ja valitse sitten **Uusi kysely**.

3. **Muuta TIETOKANTAA** seuraavan lauseen avulla voit siirtyä toissijaisen tietokanta ensisijaisuus.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Valitse **Suorita** kysely.

>[AZURE.NOTE] Jos komento on annettu, kun ensisijainen ja toissijainen on verkossa vanha ensisijainen tulee heti ilman tietojen synkronoiminen uuden toissijaisen. Vahvistetaan tapahtumat, kun komento on annettu kadota tietoja voi ilmetä, jos ensisijainen.



## <a name="next-steps"></a>Seuraavat vaiheet   

- Automaattisesti, kun varmistat todennusvaatimukset palvelin ja tietokanta on määritetty uusi ensisijaisessa. Lisätietoja on artikkelissa [SQL-tietokannan suojaus palauttaminen jälkeen](sql-database-geo-replication-security-config.md).
- Lue palauttaminen käyttämällä Active Geo-replikoinnin mukaan lukien pre huono jälkeen ja lähettää palautus vaiheet ja suorittamiseen tietojen palauttaminen-sääntö on ohjeaiheessa [Tietojen palauttaminen](sql-database-disaster-recovery.md)
- Sasha Nosov blogimerkinnän tietoja Active Geo-replikoinnin Katso [valokeila Geo replikoinnin uudet ominaisuudet](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Lisätietoja cloud-sovellusten käyttäminen aktiivinen Geo-replikoinnin suunnitteleminen on [suunnittelemisesta cloud sovellusten Liiketoiminnan jatkuvuus käyttämällä Geo toistoa varten](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Saat lisätietoja aktiivinen Geo-replikoinnin käyttämisestä joustavasti tietokannan jakavat [joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Saat yleiskatsauksen business continurity [Liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)
