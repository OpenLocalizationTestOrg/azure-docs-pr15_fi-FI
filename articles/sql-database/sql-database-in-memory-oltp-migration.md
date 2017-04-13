<properties
    pageTitle="Ladatun OLTP parantaa SQL txn perf | Microsoft Azure"
    description="Ottaa käyttöön ladatun OLTP tapahtumien suorituskyvyssä SQL-tietokannassa."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor="MightyPen"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="jodebrui"/>


# <a name="use-in-memory-oltp-preview-to-improve-your-application-performance-in-sql-database"></a>SQL-tietokantaan suorituskyvyssä oman sovelluksen käyttöä ladatun OLTP (ennakkoversio)

[Ladatun OLTP](sql-database-in-memory.md) voidaan OLTP työmäärää- [Premium](sql-database-service-tiers.md) Azure SQL-tietokantoja suorituskyvyssä ilman lisääntyvien suorituskyky.

Antaa ladatun OLTP aiemmin luotu tietokanta seuraavasti.

## <a name="step-1-ensure-your-premium-database-supports-in-memory-oltp"></a>Vaihe 1: Varmista Premium tietokannan tukee ladatun OLTP

Luotu marraskuun 2015 tai uudempi Premium tietokantojen tue olevan muistin. Voit varmistaa, tukeeko Premium-tietokannassa olevan muistin-toiminnon suorittamalla seuraava Transact-SQL-lause. Ladatun tuetaan, jos palautettu tulos on 1 (ei 0):

```
SELECT DatabasePropertyEx(Db_Name(), 'IsXTPSupported');
```

*XTP* lyhenne *Erittäin tapahtumienkäsittely*

Jos aiemmin luotu tietokanta on siirretty uuteen Vipuventtiili V12 Premium tietokantaan, voit seuraavia tekniikoita voi viedä ja tuoda sitten tiedot.

#### <a name="export-steps"></a>Vie vaiheet

Vie tuotannon tietokannan bacpac käyttämällä joko:

- [Vie](sql-database-export.md) toimintoja [portal](https://portal.azure.com/).

- **Vie tiedot tason sovelluksen** toimintoja [ajan tasalla SSMS.exe](http://msdn.microsoft.com/library/mt238290.aspx) (SQL Server Management Studiossa).
 1. Laajenna **Object Explorerissa** **Databases** -solmu.
 2. Napsauta hiiren kakkospainikkeella tietokannan solmu.
 3. Valitse **tehtävät** > **Vie tiedot taso-sovellus**.
 4. Toimi ohjatun toiminnon-ikkunasta, joka tulee näkyviin.


#### <a name="import-steps"></a>Tuontivaiheet

Tuo bacpac Premium-tietokantaan.

1. Azure [portal](https://portal.azure.com/)
 - Siirry palvelimeen.
 - Valitse [Tuo tietokanta](sql-database-import.md) .
 - Valitse Premium, hinnat taso.

2. Tuo bacpac SSMS avulla:
 - Napsauta hiiren kakkospainikkeella **Object Explorer** **tietokantojen** solmun.
 - Valitse **Tuo tiedot tason sovellus**.
 - Toimi ohjatun toiminnon-ikkunasta, joka tulee näkyviin.


## <a name="step-2-identify-objects-to-migrate-to-in-memory-oltp"></a>Vaihe 2: Määritä objektien siirtäminen ladatun OLTP

SSMS sisältää **Tapahtuman suorituskyvyn analysointi yleiskatsaus** -raportissa, joita voit suorittaa vastaan tietokanta on aktiivinen työmäärää. Raportin tunnistaa taulukot ja tallennetut toiminnot, jotka ovat hakevien ladatun OLTP siirto.

Valitse SSMS, voit luoda raportin:
- Napsauta hiiren kakkospainikkeella **Object Explorer**tietokanta-solmu.
- **Raportit** > **vakioraporttien** > **tapahtuman suorituskyvyn analysointi yleiskatsaus**.

Lisätietoja on artikkelissa [määrittäminen Jos taulukko tai tallennettu toimintosarja olisi siirtämisen ladatun OLTP](http://msdn.microsoft.com/library/dn205133.aspx).


## <a name="step-3-create-a-comparable-test-database"></a>Vaihe 3: Luo vastaa testitietokanta

Oletetaan, että raportti ilmaisee tietokannassa on taulukko, joka hyötyä muistin optimoitu taulukoksi muuntamisen. On suositeltavaa, että voit testata ensin Vahvista merkintä testaamalla.

Tarvitset tuotannon tietokannan testi kopio. Testitietokanta on oltava samalla tasolla palvelun taso tuotannon tietokantana.

Helpottamiseksi testaamista, säätää testitietokanta-tehosteasetuksilla seuraavasti:

1. Muodosta yhteys tietokantaan käyttämällä SSMS.

2. Voit välttää hänen nimeään tarvitse kyselyissä kanssa (TILANNEVEDOKSEN)-vaihtoehto määrittämällä tietokanta ‑vaihtoehto seuraava T-SQL-lause esitetyllä tavalla:
```
ALTER DATABASE CURRENT
    SET
        MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = ON;
```


## <a name="step-4-migrate-tables"></a>Vaihe 4: Siirtää taulukoita

Luo ja täytä testattava taulukon muistin optimoitu kopio. Voit luoda sen käyttämällä joko:

- SSMS kätevää optimointi muistin ohjatussa.
- Manuaalinen T-SQL.


#### <a name="memory-optimization-wizard-in-ssms"></a>Muistin optimointi ohjatun SSMS

Jos haluat käyttää siirto-asetus:

1. Muodosta yhteys testitietokanta SSMS kanssa.

2. **Objektin Explorer**Napsauta taulukkoa hiiren kakkospainikkeella ja valitse sitten **Muistin optimointi Advisor**.
 - Ohjattu **Taulukon muistin optimointitoiminnon Advisor** tulee näkyviin.

3. Valitse ohjatun toiminnon **siirron vahvistus** (tai **Seuraava** -painiketta), onko taulukon ei-tuettuja ominaisuuksia, joita ei tueta muistin optimoitu taulukot. Lisätietoja on artikkelissa:
 - *Muistin optimointi tarkistusluettelon* - [Muistia optimointi Advisor](http://msdn.microsoft.com/library/dn284308.aspx).
 - [Transact-SQL-rakenteita ei tue ladatun OLTP](http://msdn.microsoft.com/library/dn246937.aspx).
 - [Ladatun OLTP siirtämisestä](http://msdn.microsoft.com/library/dn247639.aspx).

4. Jos taulukossa on ominaisuuksia ei tueta, advisor voi suorittaa todellinen rakenteen ja tietojen siirtäminen puolestasi.


#### <a name="manual-t-sql"></a>Manuaalinen T-SQL

Jos haluat käyttää siirto-asetus:

1. Muodosta yhteys testitietokanta käyttämällä SSMS (tai samanlaisia apuohjelma).

2. Taulukon ja sen indeksit valmis T-SQL-komentosarjan hankkiminen.
 - Napsauta hiiren kakkospainikkeella SSMS-taulukko-solmu.
 - Valitse **komentosarja taulukko nimellä** > **Luo** > **Uusi kyselyikkuna**.

3. Lisää komentosarja-ikkunassa kanssa (MEMORY_OPTIMIZED = ON) CREATE TABLE-lause.

4. Jos näkyvissä on LIITETTY indeksin, muuta se NONCLUSTERED.

5. Aiemmin luodun taulukon nimeä SP_RENAME avulla.

6. Luo uusi muistin optimoitu kopio taulukon suorittamalla muokatun CREATE TABLE-komentosarjan.

7. Kopioi tiedot muistin optimoitu taulukon käyttämällä Lisää... VALITSE * HUOMIOON:
    
```
INSERT INTO <new_memory_optimized_table>
        SELECT * FROM <old_disk_based_table>;
```


## <a name="step-5-optional-migrate-stored-procedures"></a>Vaihe 5 (valinnainen): tallennettujen toimintosarjojen siirtäminen

Parannettu suorituskyky tallennetun toimintosarjan voit myös muokata olevan muistin-ominaisuus.


### <a name="considerations-with-natively-compiled-stored-procedures"></a>Huomioon otettavia seikkoja grafiikkatiedostomuotoja käännetty tallennettujen toimintosarjojen kanssa

Grafiikkatiedostomuotoja käännetty tallennetun toimintosarjan on oltava sen kanssa T-SQL-lause Valitse seuraavista vaihtoehdoista:

- NATIVE_COMPILATION

- SCHEMABINDING: eli sisältävien taulukoiden tallennetun toimintosarjan ei voi muuttaa minkäänlaista, jotka voivat vaikuttaa tallennetun toimintosarjan, ellet pudotat tallennetun toimintosarjan sarakkeen määritelmät.


Alkuperäisen moduuli on käytettävä suuri [ATOMISIA lohkot](http://msdn.microsoft.com/library/dn452281.aspx) tapahtumienhallintaa varten. Ei ole roolin eksplisiittinen ALOITTAA tapahtuman tai tapahtuma. Jos koodi havaitsee liiketoimintasäännön vastoin, se Lopeta atomisia lohko ja [palauttaa](http://msdn.microsoft.com/library/ee677615.aspx) -lause.


### <a name="typical-create-procedure-for-natively-compiled"></a>Tyypillinen CREATE PROCEDURE grafiikkatiedostomuotoja käännetty

T-SQL grafiikkatiedostomuotoja käännetty tallennetun toimintosarjan luominen on yleensä muistuttaa seuraavaa mallia:

```
CREATE PROCEDURE schemaname.procedurename
    @param1 type1, …
    WITH NATIVE_COMPILATION, SCHEMABINDING
    AS
        BEGIN ATOMIC WITH
            (TRANSACTION ISOLATION LEVEL = SNAPSHOT,
            LANGUAGE = N'your_language__see_sys.languages'
            )
        …
        END;
```

- TRANSACTION_ISOLATION_LEVEL TILANNEVEDOS on käännetty grafiikkatiedostomuotoja tallennetun toimintosarjan useimmin esiintyvän arvon. Muut arvot alijoukkoa tuetaan kuitenkin myös:
 - TOISTETTAVA LUKU
 - VOI SARJOITTAA


- KIELI-arvon on oltava sys.languages-näkymässä.


### <a name="how-to-migrate-a-stored-procedure"></a>Tallennetun toimintosarjan siirtäminen

Siirron vaiheet ovat seuraavat:


1. Tavallisen tulkittava tallennetun toimintosarjan CREATE PROCEDURE-komentosarjan hankkiminen.

2. Uuden esimerkkitekstin vastaamaan edellisen mallin sen otsikkoa.

3. Varmistaa, että käyttääkö tallennetun toimintosarjan T-SQL-koodi ominaisuudet, joita ei tueta grafiikkatiedostomuotoja käännetty tallennettujen toimintosarjojen. Toteuta menetelmää tarvittaessa.
 - Lisätietoja on [Ongelmia siirron grafiikkatiedostomuotoja käännetty tallennettujen toimintosarjojen](http://msdn.microsoft.com/library/dn296678.aspx).

4. Nimeä vanha tallennetun toimintosarjan SP_RENAME avulla. Ja PUDOTA se.

5. Suorita muokata luoda TOIMINTOSARJAN T-SQL-komentosarja.


## <a name="step-6-run-your-workload-in-test"></a>Vaihe 6: Suorita havainnollistamiseen testi

Suorita testi tietokannan, joka muistuttaa työmäärää, joka suoritetaan tuotannon tietokannan työmäärä. Tämä on paljastaminen olevan muistin-toiminnon käytön taulukoiden ja tallennettujen toimintosarjojen saavuttaa suorituskyvyn voitto.

Työmäärää pää määritteet ovat seuraavat:

- Samanaikainen yhteyksien määrä.

- Luku-/ kirjoitusoikeudet suhde.


Voit mukauttaa ja Suorita testi työmäärää, kannattaa käyttää kätevää ostress.exe-työkalun, joka on kuvattu [seuraavassa](sql-database-in-memory.md).


Pienennä verkon latenssin, tee testauksessa saman Azure maantieteellisen alueen kohtaa, johon tietokanta on olemassa.


## <a name="step-7-post-implementation-monitoring"></a>Vaihe 7: Käyttöönoton jälkeen seuranta

Harkitse seuranta ladatun-käyttöotot tuotannon suorituskyvyn tehosteita:

- [Näytön ladatun tallennustilan](sql-database-in-memory-oltp-monitoring.md).

- [Seurannan Azure SQL-tietokanta dynaaminen hallinta-näkymien käyttäminen](sql-database-monitoring-with-dmvs.md)


## <a name="related-links"></a>Aiheeseen liittyvät linkit

- [Muistin OLTP (ladatun optimointi)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Johdanto grafiikkatiedostomuotoja käännetty tallennettujen toimintosarjojen](http://msdn.microsoft.com/library/dn133184.aspx)

- [Muistin optimointi tukipalvelu](http://msdn.microsoft.com/library/dn284308.aspx)

