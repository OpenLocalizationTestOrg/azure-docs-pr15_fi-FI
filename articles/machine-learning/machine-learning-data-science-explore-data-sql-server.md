<properties 
    pageTitle="Tutkia tietoja SQL Server Virtual Machine Azure | Microsoft Azure" 
    description="Voit tutkia tietoja, jotka on tallennettu SQL Server-AM Azure." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Tutkia tietoja SQL Server Virtual Machine Azure


Tässä asiakirjassa kerrotaan, miten voit tarkastella tietoja, jotka on tallennettu SQL Server-AM Azure. Tämä voidaan tehdä tietojen wrangling käyttämällä SQL tai käyttämällä ohjelmointikieli, kuten Python.

**Valikon** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit käyttää työkaluja tietojen tutkiminen tallennustilan eri ympäristöissä. Tämä onkin vaiheen Cortanan Analytics prosessin (pää).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Tämän asiakirjan malli SQL-lauseet oletetaan, että tiedot ovat SQL Server. Jos se ei ole, viitata cloud tietojen tiede Prosessikartan lisätietoja tietojen siirtäminen SQL Server.



## <a name="sql-dataexploration"></a>SQL-tietojen kanssa SQL-komentosarjat

Seuraavassa on muutama Esimerkki SQL-komentosarjat, jonka avulla voit tutkia tietoja myymälät SQL Server.

1. Hae päivässä havaintojen määrä

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Hae categorical sarakkeen tasot

    `select  distinct <column_name> from <databasename>`

3. Hae haluamasi tasojen määrä kaksi categorical sarakkeen yhdistelmä 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Numeeristen sarakkeiden jakauman hankkiminen

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Käytännön esimerkiksi voit käyttää [Kokousesitelmän taksin tietojoukko](http://www.andresmh.com/nyctaxitrips/) ja viitata IPNB liittyvä, lopusta loppuun-hallintapaketteihin [Kokousesitelmän tietojen wrangling IPython muistikirjan ja SQL Serverin avulla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) .

##<a name="python"></a>SQL-tietojen kanssa Python

Python avulla voit tutkia tietoja ja luo ominaisuuksia, kun tiedot on SQL Server on samanlainen kuin Azuren Blob-objektien käyttäminen Python, tietojen käsittelystä suorittimessa [prosessin Azure-Blob](machine-learning-data-science-process-data-blob.md)-tietojen tietojen tiede-ympäristössä. Tiedot voidaan ladata tietokannasta pandas DataFrame on, ja valitse voi käsitellä edelleen. Tietokantayhteyden muodostamisessa ja tiedot ladataan DataFrame tässä osassa on kuvattu.

Seuraavat yhteysmerkkijonon muoto avulla voidaan muodostaa yhteyden SQL Server-tietokantaan Python käyttämällä pyodbc (korvaa palvelimen nimi, dbname, käyttäjänimi ja salasana tiettyjen arvojen kanssa):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python [Pandas kirjasto](http://pandas.pydata.org/) on monenlaisia tietorakenteet ja tietojen analysointityökaluista tietojen käsittelemistä varten Python ohjelmoinnin. Seuraava koodi lukee tulokset palauttama SQL Server-tietokantaan Pandas tietojen kehys:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nyt voit käsitellä Pandas DataFrame kuin artikkelissa [prosessin Azure Blob-objektien tietoja tietojen tiede-ympäristössä](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Toiminnon esimerkissä Cortana Analytics-prosessi

Lopusta loppuun ongelmatilanteita Esimerkki käyttämällä julkisia tietojoukko Cortana Analytics-prosessi, saat [ryhmän-tiede prosessin käytössä: SQL Serverin avulla](machine-learning-data-science-process-sql-walkthrough.md).

 
