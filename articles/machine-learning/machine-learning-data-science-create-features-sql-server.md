<properties
    pageTitle="Tietojen ominaisuudet luominen SQL Server-tietokantaan SQL: n ja Python | Microsoft Azure"
    description="SQL Azure tietojen"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;fashah;garye" />


# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Luo SQL Server-tietokantaan SQL: n ja Python tietojen ominaisuudet


Tämän asiakirjan näytetään, miten voit luoda SQL Server-AM Azure-tallennettuja tietoja, jotka auttavat algoritmit tietoja tehokkaammin tietojen ominaisuudet. Tämä onnistuu käyttämällä SQL tai käyttämällä ohjelmointikieli, kuten Python, molemmat on osoittanut tähän.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Tässä **valikossa** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit luoda tietojen ominaisuudet eri ympäristöissä. Tämä onkin vaiheessa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [AZURE.NOTE] Käytännön esimerkiksi kuulla [Kokousesitelmän taksin tietojoukko](http://www.andresmh.com/nyctaxitrips/) ja viitata IPNB liittyvä, lopusta loppuun-hallintapaketteihin [Kokousesitelmän tietojen wrangling IPython muistikirjan ja SQL Serverin avulla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) .


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että sinulla on:

* Luoda Azure-tallennustilan tilin. Jos tarvitset ohjeita, katso [Azure-tallennustilan tilin luominen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Tiedot tallennetaan SQL Server. Jos sinulla ei ole, katso ohjeita siitä, miten voit siirtää tietoja [siirtää tietoja varten Azure koneen Learning Azure SQL-tietokantaan](machine-learning-data-science-move-sql-azure.md) .


## <a name="sql-featuregen"></a>Ominaisuuden luominen SQL

Tässä osassa on kuvataan tapoja luodaan SQL-ominaisuudet:  

1. [Laske perusteella ominaisuuden luominen](#sql-countfeature)
2. [Binning ominaisuuden luominen](#sql-binningfeature)
3. [Liikkuva yhden sarakkeen ominaisuudet](#sql-featurerollout)


> [AZURE.NOTE] Kun olet luonut lisäominaisuuksia, voit lisätä ne sarakkeiksi aiemmin luotuun taulukkoon tai Luo uusi taulukko, jossa lisäominaisuudet ja perusavaimen, joka on yhdistetty alkuperäisen taulukon.

### <a name="sql-countfeature"></a>Laske perusteella ominaisuuden luominen

Tämän asiakirjan esitellään kahdella tavalla luodaan Laske ominaisuuksia. Ensimmäinen menetelmä käyttää Ehdollinen summa ja toinen tapa käyttää 'where-lauseessa. Nämä voit sitten liitettävä alkuperäisen taulukon (joko perusavaimen sarakkeet) on Laske-ominaisuuksia, alkuperäiset tiedot rinnalla.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Binning ominaisuuden luominen

Seuraavassa esimerkissä esitetään, miten Luo binned ominaisuuksia (joko 5 palkkien) binning numeerinen sarake, joka voidaan käyttää ominaisuutta sen sijaan:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Liikkuva yhden sarakkeen ominaisuudet

Tässä osassa on näytetään, miten lisensointi yhden sarakkeen taulukon luomiseen lisäominaisuuksia. Esimerkki oletetaan, että taulukossa, josta yrität luoda ominaisuuksia on sarakkeen leveyttä tai pituutta.

Seuraavassa on lyhyt askeleet leveys-/ pituusasteet paikkatietojen (niille-stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Tästä on hyötyä ymmärtää ennen featurizing sijainti-kenttään:

- Kirjaudu siitä siitä meille, emme Pohjoinen tai Etelä, Itä Länsi maapalloa.
- Erisuuri kuin nolla satoja numeron kertoo us esimerkissä on käytössä pituutta, ei leveyttä!
- Kymmenlukuun numeron antaa noin 1 000 kilometreiksi sijainti. Antaa us mitä Manner tai meren olemme on hyödyllisiä tietoja.
- Yksiköt-numeron (yksi desimaali astetta) antaa toimen enintään 111 kilometreiksi (60 meripeninkulman, noin 69 Mailia). Se kertoa meille suurin piirtein mitä suuri tila tai maa-emme.
- Desimaalin on enintään 11.1 km: se erottaa yhtä suuria kaupunkia vierekkäisten suuri kaupungin paikalle.
- Toinen desimaalierottimen on enintään 1.1 km: se erottaminen seuraavaan yhden village.
- Kolmas desimaalin tarkkuuteen on enintään 110 m se tunnistaa suuri maatalouden kentän tai institutionaalisten campus.
- Neljäs desimaali on enintään 11 m se tunnistaa pakkausta maa. On vastaa korjaamaton GPS-yksikkö tyypillinen tarkkuudella ilman häiriöitä.
- Viidennen desimaalierottimen enintään 1.1 m se on puita erottaa toisistaan. Kaupallisen GPS yksiköt tason tarkkuutta voidaan saavuttaa vain eroavuus korjaus.
- Kuudennesta desimaalierottimen on enintään 0.11 m voit käyttää tätä asetteluvaihtoehdot rakenteita suunnittelun maisemien-yksityiskohtaiset tiedot teiden luomisesta. Sen pitäisi olla yli tarpeeksi hyvä siirtojen glaciers ja joet seuranta. Tämä onnistuu tehokkaasta GPS, kuten differentially korjattu GPS painstaking toimenpiteitä.

Sijaintitietojen voit voidaan featurized seuraavasti, erotat alue, sijainti ja kaupungin. Huomaa, että voit myös kerran kutsua muut lopuksi, kuten Bing Maps-Ohjelmointirajapinnan osoitteessa `https://msdn.microsoft.com/library/ff701710.aspx` alue/alueen tiedot.

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Luo muita Laske ominaisuuksia kuvatulla voidaan edelleen yllä sijainnin mukaan-ominaisuuksia.


> [AZURE.TIP] Voit lisätä ohjelmallisesti tietueet käyttämällä värisinä kieli. Joudut ehkä lisätä tiedot joukkojen kirjoittaminen tehokkuuden [uloskuittaus esimerkki siitä, miten voit käyttää tätä pyodbc](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)parantamiseksi.
Toinen vaihtoehto on lisää tiedot tietokantaan [BCP-apuohjelman](https://msdn.microsoft.com/library/ms162802.aspx) avulla

### <a name="sql-aml"></a>Yhteyden muodostaminen Azure koneen oppiminen

Äskettäin luodun ominaisuus voidaan lisätä sarakkeena aiemmin luotuun taulukkoon tai tallennetaan uuteen taulukkoon ja tietokoneen learning alkuperäisen taulukon liitetään. Ominaisuuksien voit luoda tai käyttää Jos luotu, [Tuontitiedot](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) -moduulin Azure millilitroina alla kuvatulla tavalla:

![azureml lukijat](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Ohjelmointikieli, kuten Python käyttäminen

Luo ominaisuuksia, kun tiedot on SQL Server Python avulla eroaa Azuren Blob-objektien käyttäminen Python [prosessi Azure Blob-tietojen tietojen tiede ympäristössä](machine-learning-data-science-process-data-blob.md)suorittimessa tietojen käsittelemistä. Tiedot on ladattava tietokannasta pandas tietojen kehys ja sitten voi käsitellä edelleen. Tietokantayhteyden muodostamisessa ja tiedot ladataan tietojen kehyksen tässä osassa on kuvattu.

Seuraavat yhteysmerkkijonon muoto avulla voidaan muodostaa yhteyden SQL Server-tietokantaan Python käyttämällä pyodbc (korvaa palvelimen nimi, dbname, käyttäjänimi ja salasana tiettyjen arvojen kanssa):

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python [Pandas kirjasto](http://pandas.pydata.org/) on monenlaisia tietorakenteet ja tietojen analysointityökaluista tietojen käsittelemistä varten Python ohjelmoinnin. Seuraava koodi lukee tulokset SQL Server-tietokannasta Pandas tietojen kehys:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nyt voit käsitellä Pandas tietojen kehyksen kuin ohjeaiheissa [Azure blob storage tietoja käyttämällä Panda toimintoja](machine-learning-data-science-create-features-blob.md).
