<properties 
    pageTitle="SQL Server Azure-mallitiedot | Microsoft Azure" 
    description="SQL Server Azure-mallitiedot" 
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
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>SQL Server Azure-mallitiedot


Tämä asiakirja näyttää, miten malli SQL Server Azure tallennettuja tietoja SQL-tai Python ohjelmointikieli. Se näyttää myös siitä, miten näytteeksi tietojen siirtämiseen Azure koneen Learning tallentamalla sen tiedostoon, lataaminen Azuren Blob-objektien ja lukemalla sen Azure koneen Learning Studio.

Esimerkkejä Python käyttää [pyodbc](https://code.google.com/p/pyodbc/) ODBC-kirjaston muodostaa yhteyden SQL Server Azure ja [Pandas](http://pandas.pydata.org/) -kirjastossa, voit tehdä esimerkkejä.

>[AZURE.NOTE] Tämän asiakirjan malli SQL-koodin oletetaan, että tiedot ovat SQL Server Azure. Jos se ei ole, katso ohjeet tietojen siirtäminen SQL Server Azure- [Siirrä tiedot SQL Server Azure-](machine-learning-data-science-move-sql-server-virtual-machine.md) artikkelissa.

**Esimerkki siitä, miksi tiedot?**
Jos haluat analysoida tietojoukko on suuri, se on yleensä hyvä alas otoksen voit pienentää koko on pienempi, mutta edustava ja helpommin hallittaviin tiedot. Tämä helpottaa tietojen tietoja, tarkasteluun ja ominaisuus tekniikka. [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) sen rooli on nopea prototyyppiä tietojen käsittely-toimintojen ja konepohjaisten oppimistekniikoiden mallit käyttöön.

**Valikon** alla linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit eri tallennustilan ympäristöissä mallitiedot. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Esimerkkejä tehtävä on vaiheessa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>SQL käyttäminen

Tässä osassa kuvataan SQL avulla voit suorittaa yksinkertaisen satunnaisia esimerkkejä tietojen perusteella tietokannan eri vaihtoehtoja. Valitse perustuva tiedot koon ja sen jakelua.

Kaksi alla olevien kohteiden näyttäminen newid käyttämisestä SQL Server esimerkkejä suorittamiseen. Tapa, voit valita riippuu siitä, miten satunnaisia haluat olevan otoksen (pk_id otoksen koodissa alla oletetaan automaattisesti luodut perusavaimen).

1. Vähemmän tarkka satunnaisia malli

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Lisää satunnaisesti 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample voivat olla kun otos, jota käytetään sekä osoittaa alapuolella. Tämä voi johtua vaihtoehto, jos arvopisteiden koko on suuri (olettaen, että tiedot eri sivujen ei Korreloidun) ja viimeistele ajoissa kyselyn.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Voit tarkastella ja luoda ominaisuuksia näytteeksi näistä tiedoista tallentamalla ne uuteen taulukkoon


###<a name="sql-aml"></a>Yhteyden muodostaminen Azure koneen oppiminen

Voit käyttää suoraan yläpuolella otoksen kyselyt Azure koneen Learning [Tietojen tuominen] [ import-data] alaspäin otoksen tiedot nopeasti ja tuo Azure koneen Learning kokeen moduuli. Näyttökuva avulla lukija-moduulin näytteeksi tietojen lukeminen on alla:
   
![Lukija sql][1]

##<a name="python"></a>Ohjelmointikielellä Python käyttäminen 

Tässä osassa kuvataan [pyodbc kirjaston](https://code.google.com/p/pyodbc/) ODBC muodostaa yhteyden Python SQL server-tietokantaan. Tietokannan yhteysmerkkijono on seuraavanlainen: (korvaa palvelimen nimi, dbname, käyttäjänimi ja salasana kokoonpanosi):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python [Pandas](http://pandas.pydata.org/) -kirjastossa on monenlaisia tietorakenteet ja tietojen analysointityökaluista tietojen käsittelemistä varten Python ohjelmoinnin. Seuraava koodi lukee tiedot 0,1 näyte Azure SQL-tietokannan taulukkoon Pandas tietoihin:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Voit nyt käsitellä näytteeksi tiedot Pandas tietojen kehyksessä. 

###<a name="python-aml"></a>Yhteyden muodostaminen Azure koneen oppiminen

Seuraava esimerkki koodi voit tallentaa alas näyte tiedot tiedostoon ja lataa se Azure-blob. Blob-tiedot voi suoraan lukea kyselyjä Azure koneen Learning kokeen [Tietojen] [ import-data] moduuli. Vaiheet ovat seuraavat: 

1. Kirjoita pandas tietojen kehyksen paikallisen tiedoston

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Azure-blob paikallisen tiedoston lataaminen

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Tietojen lukeminen Azure-blob Azure koneen Learning [Tietojen] [ import-data] moduulin alla näytön kaapata esitetyllä tavalla:
 
![Lukija-blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Toiminnon esimerkissä ryhmän tietojen tiede-prosessi

Lopusta loppuun ongelmatilanteita Esimerkki ryhmän tietojen tiede-prosessin avulla julkisen tietojoukko kohdassa [ryhmän tietojen tiede prosessi-toiminto: käyttäen SQL Server](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
