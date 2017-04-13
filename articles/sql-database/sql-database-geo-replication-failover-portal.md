<properties 
    pageTitle="Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan Azure-portaalissa | Microsoft Azure" 
    description="Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokannan Azure-portaalissa" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Aloita suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan Azure-portaalissa


> [AZURE.SELECTOR]
- [Azure portal](sql-database-geo-replication-failover-portal.md)
- [PowerShellin](sql-database-geo-replication-failover-powershell.md)
- [T-SQL](sql-database-geo-replication-failover-transact-sql.md)


Tässä artikkelissa kerrotaan aloittaminen automaattisesti [Azure portal](http://portal.azure.com)toissijainen SQL-tietokantaan. Määrittää Geo replikoinnin, on artikkelissa [Määrittäminen Geo-replikoinnin Azure SQL-tietokantaan](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Aloita automaattisesti

Toissijaisen tietokanta, voidaan vaihtaa ensisijaisen luomisella.  

1. [Azure portal](http://portal.azure.com) Selaa Geo replikoinnin partnership ensisijainen tietokannan.
2. Valitse SQL-tietokanta-sivu **kaikkia asetuksia** > **Geo replikoinnin**.
3. Valitse **SECONDARIES** -luettelosta haluamasi tietokanta muuttuvat uuden pääsivujen ja valitse **automaattisesti**.

    ![automaattisesti][2]

4. Valitse **Kyllä** , Aloita vikasietotilaa.

Komennon heti vaihtuu toissijaisen tietokanta ensisijainen rooliin. 

Tällä aikana molemmat tietokannat eivät ole käytettävissä (järjestykseen 0 25 sekuntia) samalla, kun roolit vaihdetaan lyhyen ajan. Jos ensisijainen tietokannassa on useita toissijainen tietokantoja, komento automaattisesti uudelleen muiden muodostaa uusi ensisijainen secondaries. Koko-toiminto tekee saapuville alle minuutin Normaali tilanteissa. 

>[AZURE.NOTE] Jos ensisijainen on online-tilassa ja vahvistetaan tapahtumat, kun komento on annettu kadota tietoja saattaa ilmetä.


## <a name="next-steps"></a>Seuraavat vaiheet   

- Automaattisesti, kun varmistat todennusvaatimukset palvelin ja tietokanta on määritetty uusi ensisijaisessa. Lisätietoja on artikkelissa [SQL-tietokannan suojaus palauttaminen jälkeen](sql-database-geo-replication-security-config.md).
- Lisätietoja palauttaminen käyttämällä Active Geo-replikoinnin mukaan lukien pre huono jälkeen ja lähettää palautus vaiheet ja suorittamiseen tietojen palauttaminen-sääntö on ohjeaiheessa [Tietojen palauttaminen työkaluja](sql-database-disaster-recovery.md)
- Sasha Nosov blogimerkinnän tietoja Active Geo-replikoinnin Katso [valokeila Geo replikoinnin uudet ominaisuudet](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Lisätietoja cloud-sovellusten käyttäminen aktiivinen Geo-replikoinnin suunnitteleminen on [suunnittelemisesta cloud sovellusten Liiketoiminnan jatkuvuus käyttämällä Geo toistoa varten](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Saat lisätietoja aktiivinen Geo-replikoinnin käyttämisestä joustavasti tietokannan jakavat [joustavasti resurssivarantoon varalle](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Saat yleiskatsauksen business continurity [Liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
