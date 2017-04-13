### <a name="prerequisites"></a>Edellytykset
- Azure tilin; Voit luoda [ilmaisen tilin](https://azure.microsoft.com/free)
- - [Azure SQL-tietokannan](../articles/sql-database/sql-database-get-started.md) yhteys-tiedot, mukaan lukien palvelimen nimen, tietokannan nimi ja käyttäjänimellä ja salasanalla. SQL-tietokanta-yhteysmerkkijonon sisältyy nämä tiedot:
  
    Palvelimen = tcp:*yoursqlservername*. database.windows.net,1433;Initial luettelon =*yourqldbname*; Pysyvä suojaustiedot = False; Käyttäjätunnus = {your_username}; Salasanan = {your_password}; MultipleActiveResultSet-sarjoja = False; Salaa = tosi. TrustServerCertificate = False; Yhteyden aikakatkaisu = 30;

    Lisätietoja on artikkelissa [Azure SQL-tietokannat](https://azure.microsoft.com/services/sql-database).

> [AZURE.NOTE] Kun luot Azure SQL-tietokantaan, voit myös luoda kuuluva SQL otoksen tietokannat. 



Ennen kuin käytät Azure SQL-tietokanta logiikan-sovelluksessa, muodosta yhteys SQL-tietokantaan. Voit tehdä tämän helposti sisällä logiikan sovelluksen Azure-portaalissa.  

Muodosta yhteys Azure SQL-tietokantaan, seuraavien ohjeiden mukaisesti:  

1. Logiikan sovelluksen luominen. Logiikan sovellusten suunnittelussa Lisää käynnistin ja lisää sitten toiminnon. Valitse avattavassa luettelossa **Näytä Microsoft hallitun ohjelmointirajapinnan** ja Kirjoita hakukenttään "sql". Valitse jokin seuraavista:  

    ![SQL Azure-yhteyden luominen vaihe](./media/connectors-create-api-sqlazure/sql-actions.png)

2. Jos et ole aiemmin luonut mahdolliset yhteydet SQL-tietokantaan, sinua kehotetaan yhteystiedot:  

    ![SQL Azure-yhteyden luominen vaihe](./media/connectors-create-api-sqlazure/connection-details.png) 

3. Kirjoita SQL-tietokannan tiedot. Tarvittavat ominaisuudet tähdellä.

    | Ominaisuus | Tiedot |
|---|---|
| Yhdyskäytävän välityksellä | Jätä tämä ole valittuna. Tätä käytetään muodostettaessa yhteyttä paikalliseen SQL Server. |
| Yhteyden nimi * | Kirjoita mitään yhteyttä. | 
| SQL-palvelimen nimi * | Kirjoita palvelimen nimi; Mikä on suunnilleen *servername.database.windows.net*. Palvelimen nimi on näkyvät Azure-portaalissa SQL-tietokanta-ominaisuudet ja yhteysmerkkijonon myös näkyvissä. | 
| SQL-tietokannan nimi * | Kirjoita annoit SQL-tietokantaan. Tämä on kerrottu yhteysmerkkijonon SQL-tietokanta-ominaisuudet: alkuperäisen luettelon =*yoursqldbname*. | 
| Käyttäjänimi * | Kirjoita käyttäjänimesi, voit luoda, jos SQL-tietokanta on luotu. Tämä näkyy Azure portaalissa SQL-tietokanta-ominaisuudet. | 
| Salasanan * | Voit luoda, jos SQL-tietokanta on luotu salasana. | 

    Nämä käytetään sallivat logiikan sovelluksen yhteyden ja SQL-tietojen käyttämistä varten. Kun valmiina, yhteyden tiedot näyttää seuraavankaltaiselta:  

    ![SQL Azure-yhteyden luominen vaihe](./media/connectors-create-api-sqlazure/sample-connection.png) 

4. Valitse **Luo**. 

5. Huomaa, yhteys on luotu. Nyt jatkaa logiikan sovelluksen muita ohjeita: 

    ![SQL Azure-yhteyden luominen vaihe](./media/connectors-create-api-sqlazure/table.png)