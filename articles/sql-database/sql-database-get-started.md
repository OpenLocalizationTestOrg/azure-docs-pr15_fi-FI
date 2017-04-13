<properties
    pageTitle="SQL-tietokanta-opetusohjelma: SQL-tietokannan luominen | Microsoft Azure"
    description="Opi määrittämään looginen SQL-tietokantapalvelin, johon, palvelimen palomuurisäännön, SQL-tietokantaan ja mallitiedot. Lisäksi opit asiakastyökalut yhteydessä, määrittää käyttäjiä ja tietokantaan palomuurin siirtosäännön määrittäminen."
    keywords="SQL-tietokannan opetusohjelmassa sql-tietokannan luominen"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>SQL-tietokanta-opetusohjelma: SQL-tietokannan luominen minuuttia Azure-portaalissa

> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShellin](sql-database-get-started-powershell.md)

Tässä opetusohjelmassa kerrotaan Azure portaalin käyttämisestä:

- Luo mallitiedot Azure SQL-tietokantaan.
- Luo yksittäinen IP-osoite tai alueen IP-osoitteiden palvelintason palomuurisääntö.

Voit suorittaa samoja tehtäviä [C#](sql-database-get-started-csharp.md) tai [PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Ensimmäisen Azure SQL-tietokannan luominen 

1. Jos et ole yhteydessä, muodosta yhteys [Azure portal](http://portal.azure.com).
2. Valitse **Uusi**, valitse **tietoja + tallennustilan**ja Etsi **SQL-tietokantaan**.

    ![Uuden sql-tietokannan 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Valitse **SQL-tietokannan** SQL-tietokanta-sivu avautuu. Tämä sivu sisällön vaihtelee sen mukaan montako tilauksistasi ja olemassa olevat objektit (kuten aiemmin palvelimet).

    ![Uuden sql-tietokannan 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. **Tietokannan nimi** -tekstiruutuun anna sille nimi ensimmäisen tietokannan – esimerkiksi "Omat tietokannan". Vihreä valintamerkki osoittaa, että olet määrittänyt kelvollinen nimi.

    ![Uuden sql-tietokannan 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Jos sinulla on useita tilauksia, valitse tilaus.
6. **Resurssiryhmä**valitsemalla **Luo uusi** ja anna ensimmäinen resurssiryhmän – esimerkiksi "Omat--resurssiryhmä". Vihreä valintamerkki osoittaa, että olet määrittänyt kelvollinen nimi.

    ![Uuden sql-tietokannan 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. **Valitse lähde**-kohdassa **malli** ja valitse **Valitse malli** -kohdassa **AdventureWorksLT [Vipuventtiili V12]**.

    ![Uuden sql-tietokannan 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. Valitse **palvelin**-kohdassa **Määritä tarvittavat asetukset**.

    ![Uuden sql-tietokannan 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Valitse **Luo uusi palvelin**Server-sivu. Azure SQL-tietokanta on luotu palvelimen objektiin, joka voi olla uuteen palvelimeen tai aiemmin palvelimeen.

    ![Uuden sql-tietokannan 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Tarkista selvittääksesi, sinun on annettava uusi palvelimen tiedot **uuteen palvelimeen** -sivu.

    ![Uuden sql-tietokannan 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. **Palvelimen nimi** -tekstiruutuun on ensimmäinen palvelimellesi – esimerkiksi "Omat-uusi-palvelin-objekti" nimi. Vihreä valintamerkki osoittaa, että olet määrittänyt kelvollinen nimi.

    ![Uuden sql-tietokannan 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. Anna **palvelimen järjestelmänvalvojan kirjautuminen**kohdassa käyttäjänimi järjestelmänvalvoja-kirjautuminen tälle palvelimelle – esimerkiksi "-järjestelmänvalvoja-tilini". Tämän kirjautumisen kutsutaan server principal kirjautuminen. Vihreä valintamerkki osoittaa, että olet määrittänyt kelvollinen nimi.

    ![Uuden sql-tietokannan 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. Valitse **salasana** ja **Vahvista salasana**-salasanaa palvelimen principal kirjautumistiliä – kuten "p@ssw0rd1". Vihreä valintamerkki osoittaa, että olet määrittänyt kelvollinen salasana.

    ![Uuden sql-tietokannan 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. Valitse **sijainti**-kohdasta haluamasi kohtaan – esimerkiksi "Australia Itä" määrittäminen palvelinkeskuksessa.

    ![Uuden sql-tietokannan 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. Valitse ** luominen Vipuventtiili V12 server (uusimman päivityksen), Huomaa sisältää vain nykyisen Azure SQL server-version luontivaihtoehto.

    ![Uuden sql-tietokannan 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Huomaa, että oletusarvoisesti **Salli azure** -palveluiden palvelinta valintaruutu on valittuna eikä niitä voi muuttaa tähän. Tämä on lisäasetusta. Voit muuttaa palvelimen objektin palomuurin palvelinasetukset tätä asetusta, mutta useimmissa skenaarioissa tämä ei ole tarpeen.

    ![Uuden sql-tietokannan 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Uusi palvelin-sivu, valitse Tarkista valinnat ja valitse sitten **Valitse** Valitse tämä uuteen palvelimeen uuden tietokannan.

    ![Uuden sql-tietokannan 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Valitse SQL-tietokanta-sivu **hinnoittelu taso**, valitse **S2 vakio** ja valitse sitten **Basic** valitsemaan vähintään kallista hinnoittelu tason ensimmäisen tietokannan. Voit muuttaa hinnoittelu taso myöhemmin.

    ![Uuden sql-tietokannan 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. SQL-tietokanta-sivu, valitse Tarkista valinnat ja valitse sitten **Luo** luomaan ensimmäinen palvelin ja tietokanta. Arvot, jotka olet tarkistetaan ja käyttöönotto alkaa.

    ![Uuden sql-tietokannan 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Valitse portaalin työkaluriviltä käyttöönoton tilan tarkistaminen **ilmoitukset** -kohteita.

    ![Uuden sql-tietokannan 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Kun käyttöönotto on valmis, uusi Azure SQL-palvelin ja tietokanta luodaan Azure-tietokannassa. Sinulla voi muodostaa uusi palvelimeen tai työkaluilla, kunnes olet luonut palvelimen palomuurisääntö, joka avaa yhteydet ulkopuolella Azure SQL-tietokantaan palomuurin SQL Serverin tietokantaan.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet valmis SQL-tietokantaan Tässä opetusohjelmassa ja mallitietojen avulla luotu tietokanta, olet valmis käyttämällä tuttuja työkaluja näytöllä.

- Jos olet tutustunut Transact-SQL- ja SQL Server Management Studiossa (SSMS), lisätietoja [Yhdistä](sql-database-connect-query-ssms.md)ja kyselyn SQL-tietokanta on SSMS.

- Jos tiedät, Excel, katso, miten [Yhdistä Excel Azure SQL-tietokantaan](sql-database-connect-excel.md).

- Jos olet valmis aloittamaan coding, valitse ohjelmoinnin kieli [kirjastot SQL-tietokantaan ja SQL Server](sql-database-libraries.md)-palvelussa.

- Jos haluat paikallisen SQL Server-tietokannat Siirry Azure, katso lisätietoja [tietokannan SQL-tietokantaan siirtyminen](sql-database-cloud-migrate.md) .

- Jos haluat ladata tietoja uuteen taulukkoon CSV-tiedoston komentorivin BCP-työkalun avulla, katso [ladataan tietojen tuominen CSV-tiedoston BCP SQL-tietokannan](sql-database-load-from-csv-with-bcp.md).

- Jos haluat aloittaa tutustuminen Azure SQL-tietokanta suojaus-kohdassa [Suojaus käytön aloittaminen](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Lisäresursseja

[Mikä on SQL-tietokantaan?](sql-database-technical-overview.md)
