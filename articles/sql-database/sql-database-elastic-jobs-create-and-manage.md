<properties
    pageTitle="Voit luoda ja hallita skaalatun ulos Azure SQL-tietokantoja joustavasti työt | Microsoftin Azure"
    description="Käy läpi luomisen ja hallinnan joustavasti tietokannan työn."
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
    ms.date="07/27/2016"
    ms.author="ddove"/>

# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Voit luoda ja hallita skaalatun ulos Azure SQL-tietokantoja joustavasti töitä (ennakkoversio)

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShellin](sql-database-elastic-jobs-powershell.md)


**Joustavasti tietokannan työt** yksinkertaistaa ryhmien tietokantojen hallinta suoritetaan järjestelmänvalvojan toimintoja, kuten rakenteen muutokset, tunnistetietojen hallinta, viittaus tietojen päivittämisen, tietojen kerääminen suorituskyvyn tai vuokraajan (asiakas) telemetriatietojen sivustokokoelman. Joustavasti tietokannan työt on tällä hetkellä käytettävissä kautta Azure portaalin ja PowerShellin cmdlet-komennot. Kuitenkin Azure portaalin Näyttömalli käyttää rajoitettuja toimintoja rajoitettu suorittamisen kaikki tietokannat [joustavasti tietokannan resurssivarantoon (ennakkoversio)](sql-database-elastic-pool.md)kautta. Käyttää lisätoimintoja ja komentosarjojen suorittamisen koko ryhmälle tietokannoista, mukaan lukien mukautettu määrittämä kokoelma tai shard tiedostojoukon (luodaan käyttämällä [joustavasti tietokannan asiakkaan kirjaston](sql-database-elastic-scale-introduction.md)), artikkelissa [luominen ja hallinta PowerShellin avulla työt](sql-database-elastic-jobs-powershell.md). Saat lisätietoja työt [joustavasti tietokannan töiden yleiskatsaus](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Edellytykset

* Azure tilaus. Katso maksuttoman kokeiluversion [yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).
* Joustavasti tietokanta-ryhmän. Katso [tietoja joustavasti tietokannan jakavat](sql-database-elastic-pool.md)
* Joustavasti tietokannan työn palvelun osien asennuksen. Katso [joustavasti tietokannan työ-palvelun asentaminen](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Töiden luominen

1. Käyttämällä [Azure portal](https://portal.azure.com)varannon aiemmin luodun joustavasti tietokannan työ, valitse **Luo projekti**.
2. Kirjoita käyttäjänimi ja salasana (luotu töiden asennuksen aikana) tietokannan järjestelmänvalvojan töiden hallinta tietokannan (metatietojen tallennus projekteille).

    ![Projektin nimeä, kirjoita tai liitä koodi ja valitse Suorita][1]
2. Kirjoita projektin nimi **Luo työ** -sivu.
3. Kirjoita käyttäjänimi ja salasana kohdetietokantojen komentosarjojen suorittamisen onnistuu tarvittavia oikeuksia.
4. Liitä tai kirjoita T-SQL-komentosarja.
5. Valitse **Tallenna** ja valitse sitten **Suorita**.

    ![Projektien luominen ja suorittaminen][5]

## <a name="run-idempotent-jobs"></a>Suorita idempotent työt

Kun suoritat komentosarjan vastaan tietokantoja, sinun on oltava että komentosarja on idempotent. Toisin sanoen komentosarja on oltava voit suorittaa useita kertoja, vaikka se on epäonnistunut ennen keskeneräiseen tilaan. Esimerkiksi kun komentosarjan epäonnistuu, työn automaattisesti yritetään, kunnes se onnistuu (rajoituksin yritä uudelleen nimellä logiikan myöhemmin lakkaavat uudelleen). Voit tehdä tämän tapa on käyttää lauseen "Jos on olemassa" ja poistaa kaikki löytyneen esiintymän ennen kuin luot uuden objektin. Esimerkki on mukana:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Voit myös käyttää "Jos ei ole olemassa"-lause ennen kuin luot uuden esiintymän:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Tämä komentosarja sitten päivittää aiemmin luotu taulukko.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Työn tilan tarkistaminen

Kun työ on alkanut, voit tarkistaa sen edistymisestä.

1. Valitse **hallinta-projekteille**joustavasti tietokannan resurssivarantoon-sivulla.

    ![Valitse "Hallinta-projekteille"][2]

2. Napsauta nimeä (a) työn. **Tila** voi olla "Valmis" tai "Epäonnistui." Projektin tiedot näkyvät (b) sen päivämäärän ja kellonajan luominen ja suorittaminen. (C) alapuolelle, jotka näkyvät luettelossa kunkin tietokannalle komentosarja etenemisen resurssivarantoon, antamalla sen päivämäärän ja kellonajan tiedot.

    ![Valmiin projektin tarkistaminen][3]


## <a name="checking-failed-jobs"></a>Tarkistaminen on epäonnistunut töitä

Jos projektin epäonnistuu, sen suorittamisen loki löytyy. Valitse tiedot näkyviin epäonnistunut työ nimi.

![Tarkista epäonnistunut työ][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png

 
