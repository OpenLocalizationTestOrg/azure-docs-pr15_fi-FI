<properties
    pageTitle="Palauttaa yhden taulukon Azure SQL-tietokannan varmuuskopiointi | Microsoft Azure"
    description="Opettele palauttamaan yhdeksi taulukoksi Azure SQL-tietokanta varmuuskopiosta."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="daleche"/>


# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Voit palauttaa yhden taulukon Azure SQL-tietokannan varmuuskopiointi

T tilanteen, jossa muokkaamasi vahingossa tietoja SQL-tietokantaan ja haluat nyt palauttaa yhteen kyseessä olevaan taulukkoon. Tässä artikkelissa käsitellään palauttaa yhden taulukon tietokannan SQL-tietokannan [automaattisen varmuuskopioinnin](sql-database-automated-backups.md).

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Valmistavia vaiheita: nimeä taulukko uudelleen ja palauttaa tietokannan kopion
1. Määritä taulukko, jonka haluat korvata palautettu kopiolla Azure SQL-tietokannassa. Nimeä taulukko uudelleen Microsoft SQL Management Studiossa avulla. Nimeä esimerkiksi taulukon &lt;taulukkonimi&gt;_old.

    **Huomautus** Jotta kansiota ei estettäisi, varmista, että olevan aktiviteettia käytössä taulukon, jonka haluat nimetä uudelleen. Jos käytössä ilmenee ongelmia, varmista, että, joka suorittaa tämän toimenpiteen aikoina.

2. Palauttaa varmuuskopion tietokannasta pisteeseen ajankohtaan, jonka haluat palauttaa käyttämään [Pisteen In_Time palauttaa](sql-database-recovery-using-backups.md#point-in-time-restore) ohjeita.

    **Huomautuksia**:
    - Palautetun tietokannan nimi on DBName + aikaleima-muodossa. esimerkiksi **Adventureworks2012_2016-01 – 01T22-12Z**. Tämä vaihe ei korvaa aiemmin luodun tietokannan nimi, palvelimessa. Tämä on turvallisuutta mitta ja se on tarkoitettu sinulle vahvistamiseksi palautettu tietokanta, ennen kuin ne niiden nykyinen tietokanta ja nimeä palautettu tietokanta tuotannon käyttöä varten.
    - Kaikki suorituskyvyn tasoa perus Premium automaattisesti varmuuskopioidaan palvelun kanssa vaihteleva varmuuskopion säilytys arvot tason mukaan:

| DB palauttaminen | Tavallinen taso | Vakio tasoa | Premium tasoa |
| :-- | :-- | :-- | :-- |
|  Ajankohtaan palauttaminen |  Mikä tahansa palauttaa pisteen seitsemän päivän ajalta|Mikä tahansa palauttaa pisteen 35 päivän aikana| Mikä tahansa palauttaa pisteen 35 päivän aikana|

## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Taulukon kopioiminen palautettu tietokanta SQL-tietokannan siirto-työkalun avulla
1. Lataa ja asenna [SQL-tietokannan siirto](https://sqlazuremw.codeplex.com).

2. Avaa SQL siirron Ohjattu tietokannan, **Valitse prosessi** -sivulla, valitse **Analysoi/artikkelista tietokannan**ja valitse sitten **Seuraava**.
![SQL-tietokannan siirto ohjattu – Valitse prosessi](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)
3. **Muodosta yhteys palvelimeen** -valintaikkunassa Käytä seuraavia asetuksia:
 - **Palvelimen nimi**: Your SQL Azure-esiintymä
 - **Todentaminen**: **SQL Server-todennusta**. Anna sitten kirjautumistietosi.
 - **Tietokanta**: **perustyyli DB (Luettele kaikki tietokannat)**.
 - **Huomautus** Oletusarvon mukaan ohjattu toiminto tallentaa kirjautumistietoja. Jos et halua sen, valitse **Unohda kirjautumistiedot**.
![SQL-tietokannan siirto ohjattu – Valitse lähde - vaihe 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. **Valitse tietolähde** -valintaikkunassa Valitse lähde **valmistavia vaiheita** osion palautettu tietokannan nimi ja valitse sitten **Seuraava**.

    ![SQL-tietokannan siirto ohjattu – Valitse lähde - vaihe 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)

5. **Valitse objektit** -valintaikkunassa **Valitse tietyn tietokantaobjektien** -vaihtoehto ja valitse sitten table(or tables), jonka haluat siirtää kohteen palvelimeen.
![SQL-tietokannan siirto ohjattu - objektien valitseminen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)

6. Kun sinulta kysytään, oletko valmis luomaan SQL-komentosarja tietoja Valitse **Komentosarja ohjatun yhteenveto** -sivulla **Kyllä** . Käytössä on myös voi tallentaa TSQL komentosarja myöhempää käyttöä varten.
![SQL-tietokannan siirto ohjattu - komentosarjan ohjatun yhteenveto](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)

7. Valitse **Tulosten yhteenveto** -sivulla **Seuraava**.
![SQL-tietokannan siirto ohjattu - tulosten yhteenveto](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)

8. **Asennuksen kohde palvelinyhteyden** -sivulla **Muodosta yhteys palvelimeen**ja kirjoita sitten tiedot seuraavasti:
    - **Palvelimen nimi**: kohde esiintymä
    - **Todentaminen**: **SQL Server-todennusta**. Anna sitten kirjautumistietosi.
    - **Tietokanta**: **perustyyli DB (Luettele kaikki tietokannat)**. Tämä asetus on lueteltu kaikki tietokannat kohde-palvelimeen.

    ![SQL-tietokannan siirto Ohjattu - asennuksen kohde palvelinyhteyden](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)

9. Valitse **Yhdistä**, valitse kohdetietokanta, jonka haluat siirtää taulukon kohtaa ja valitse sitten **Seuraava**. Tämä on valmis, aiemmin luodut komentosarja ja näkyviin tulee kopioitava kohdetietokannan juuri siirretyt taulukon.

## <a name="verification-step"></a>Tarkistus-vaihe
1. Kyselyn ja testata juuri kopioidun taulukon varmistaaksesi, että tiedot ovat ennalleen. Vahvistus, kun pudotat uudelleennimetty taulukkomuotoon **valmistavia vaiheita** -osassa. (esimerkiksi &lt;taulukkonimi&gt;_old).

## <a name="next-steps"></a>Seuraavat vaiheet

[Automaattisen varmuuskopioinnin SQL-tietokantaan](sql-database-automated-backups.md)
