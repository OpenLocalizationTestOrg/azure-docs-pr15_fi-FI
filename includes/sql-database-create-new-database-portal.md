
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Luo uusi Azure SQL-tietokanta

Azure-portaalissa seuraavien vaiheiden avulla voit luoda uuden Azure SQL-tietokantaan palvelimeen uuteen tai aiemmin luotuun Azure SQL-tietokanta looginen.

1. Jos et ole yhteydessä, muodosta yhteys [Azure portal](http://portal.azure.com).
2. Valitse **Uusi**, kirjoita **SQL-tietokanta**ja valitse sitten **SQL-tietokanta (uusi tietokanta)**.

     ![Uuden tietokannan](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Valitse **SQL-tietokanta (uusi tietokanta)**.

     ![Uuden tietokannan](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Valitse **Luo** uuden tietokannan luominen SQL-tietokanta-palvelussa.

     ![Uuden tietokannan](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Sisältävät arvot seuraavat ominaisuudet:

 - Tietokannan nimi
 - Tilaus: Tämä koskee vain, jos sinulla on useita tilauksia.
 - Resurssiryhmä: Jos olet olet vasta, käytä loogisten palvelimen resurssiryhmä.
 - Valitse lähde: Voit valita tyhjän tietokannan ja Azure varmuuskopion mallitiedot. Voit siirtää paikallisen SQL Server-tietokantaan tai lataat tiedot komentorivin BCP-työkalun avulla, on tämän artikkelin lopussa olevassa linkkien tarkasteleminen.
 - Palvelin: Uuteen tai aiemmin luotuun loogisen palvelin.
 - Palvelimen järjestelmänvalvoja kirjautuminen
 - Salasana
 - Hinnoittelu taso: Jos olet olet vasta, käytä oletusarvoa S0.
 - Lajittelu: Tämä koskee vain, jos tyhjän tietokannan valittiin.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Valitse **Luo**. Ilmaisinalueella näet käyttöönoton on alkanut.

     ![Uuden tietokannan](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Odota loppuun ennen seuraavaan vaiheeseen jatkamista käyttöönottoa varten.

     ![Uuden tietokannan](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
