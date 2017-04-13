<properties 
    pageTitle="Geo replikoinnin määrittäminen Azure SQL-tietokanta Azure-portaalissa | Microsoft Azure" 
    description="Geo replikoinnin määrittäminen Azure SQL-tietokanta Azure-portaalissa" 
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
    ms.workload="NA"
    ms.date="10/18/2016"
    ms.author="sstein"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-the-azure-portal"></a>Geo replikoinnin määrittäminen Azure SQL-tietokanta Azure-portaalissa


> [AZURE.SELECTOR]
- [Yleiskatsaus](sql-database-geo-replication-overview.md)
- [Azure portal](sql-database-geo-replication-portal.md)
- [PowerShellin](sql-database-geo-replication-powershell.md)
- [T-SQL](sql-database-geo-replication-transact-sql.md)

Tässä artikkelissa kerrotaan aktiivinen Geo-replikoinnin määrittäminen [Azure portal](http://portal.azure.com)SQL-tietokantaan.

Aloittaa automaattisesti Azure-portaalissa, katso [aloittaa suunniteltu tai suunnittelematon automaattisesti Azure SQL-tietokantaan Azure-portaalissa](sql-database-geo-replication-failover-portal.md).

>[AZURE.NOTE] Kaikki palvelutasot kaikki tietokannat on nyt aktiivinen Geo-replikoinnin (luettavissa secondaries). Huhtikuussa 2017 ei voi lukea toissijainen tyyppi poistettu käytöstä ja ei voi lukea aiemmin luotujen tietokantojen päivitetään automaattisesti luettavissa secondaries.

Voit määrittää Geo replikoinnin Azure-portaalissa, tarvitset seuraavaan resurssiin:

- Azure SQL-tietokantaan - ensisijainen tietokanta, jonka haluat toistaa eri maantieteellinen alue.

## <a name="add-secondary-database"></a>Lisää toissijainen tietokanta

Seuraavat vaiheet toissijainen uuden tietokannan luominen Geo replikoinnin yhteistyössä.  

Jos haluat lisätä toissijainen, on oltava tilauksen omistaja tai rinnakkaisomistajan. 

Toissijaisen tietokanta on sama nimi kuin ensisijaisen tietokannan ja on oletusarvoisesti palvelun samalla tasolla. Toissijaisen tietokanta voi olla yhden tietokannan tai joustavasti tietokannan. Lisätietoja on artikkelissa [Palvelutasot](sql-database-service-tiers.md).
Kun toissijaisen on luotu ja kylvetään, tietojen alkaa ensisijainen tietokannan replikoiminen toissijainen uuteen tietokantaan. 

> [AZURE.NOTE] Jos kumppanin tietokanta on jo (esimerkiksi – tuloksena lopetetaan edellisen Geo replikoinnin yhteyden) komento epäonnistuu.

### <a name="add-secondary"></a>Lisää toissijainen

1. Siirry [Azure portal](http://portal.azure.com)tietokanta, jonka haluat Geo replikoinnin määrittäminen.
2. SQL-tietokanta-sivun Valitse **Geo replikointi**ja valitse sitten haluamasi alue on toissijainen luotu. Voit valita minkä tahansa alue kuin alue, jossa ensisijaisen tietokanta, mutta [pisteparin alueen](../best-practices-availability-paired-regions.md) suositellaan.

    ![määrittää Geo replikoinnin](./media/sql-database-geo-replication-portal/configure-geo-replication.png)


4. Valitse tai Määritä palvelin ja hinnoittelu taso toissijainen tietokannan.

    ![Toissijainen määrittäminen](./media/sql-database-geo-replication-portal/create-secondary.png)

5. Vaihtoehtoisesti voit lisätä toisen tietokannan joustavasti tietokanta-ryhmään:

 Resurssivarantoon toissijainen tietokannan luomiseen Valitse **joustavasti tietokannan resurssivarantoon** resurssivarantoon kohde-palvelimeen. Resurssivarantoon on jo olemassa kohde-palvelimeen, kun tämä työnkulku ei voi luoda resurssivarantoon.

6. Valitse Lisää toissijaisen **luominen** .
 
6. Toissijaisen tietokanta on luotu ja seeding käynnistyy. 
 
    ![Toissijainen määrittäminen](./media/sql-database-geo-replication-portal/seeding0.png)

7. Kun seeding prosessi on valmis, toissijaisen tietokanta näyttää sen tilan.

    ![valuuttamuunnosten valmis](./media/sql-database-geo-replication-portal/seeding-complete.png)


## <a name="remove-secondary-database"></a>Poista toissijaisen tietokanta

Toiminto katkaisee toissijainen tietokannan replikoiminen ja toissijaisen roolin muutoksista Normaali luku-ja kirjoitusoikeudet tietokannan pysyvästi. Jos yhteydet toissijainen tietokantaan katkeaa, komento onnistuu, mutta ei tule luku ja kirjoitus-yhteyksien jälkeen toissijainen palautetaan.  

1. Siirry [Azure portal](http://portal.azure.com)Geo replikoinnin partnership ensisijainen tietokannan.
2. Valitse SQL-tietokanta-sivulla **Geo replikoinnin**.
3. Valitse **SECONDARIES** -luettelosta haluamasi tietokanta Geo replikoinnin partnership poistaminen.
4. Valitse **Lopeta replikoinnin**.

    ![Poista toissijainen](./media/sql-database-geo-replication-portal/remove-secondary.png)

5. Valitsemalla **Lopeta replikoinnin** avautuu vahvistusikkunassa niin valitsemalla **Kyllä** tietokannan poistaminen Geo replikoinnin partnership (aseta se luku-ja kirjoitusoikeudet tietokanta ei ole mitään replikoinnin osa).


## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja Active Geo-replikoinnin, artikkelissa - [Aktiivinen Geo-replikointi](sql-database-geo-replication-overview.md)
- Liiketoiminnan jatkuvuus-yleiskatsaus ja tilanteita, joissa on artikkelissa [liiketoiminnan jatkuvuuden yhteenveto](sql-database-business-continuity.md)

