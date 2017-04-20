<properties 
    pageTitle="Käsitellä Azure blob-tietojen kanssa kehittynyt analytics | Microsoftin Azure" 
    description="Azure-Blob-etäsäilöpalvelun prosessin tiedot." 
    services="machine-learning,storage" 
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

#<a name="heading"></a>Kanssa analytics kokeneet prosessin Azure blob-tietojen

Tässä asiakirjassa käsitellään interaktiiviseen tietojen ja kehittävän ominaisuuksia Azure Blob storage tallennetut tiedot. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Lataa tiedot Pandas tiedot kehykseen
Tutkia ja käsitellä DataSet-osaa, se on ladattava blob-lähteestä paikalliseen tiedostoon, joka voidaan ladata sitten Pandas tiedot kehykseen. Seuraavat toimet, joiden osalta tämän menettelyn:

1. Lataa tiedot Azure blob-koodilla seuraavan näytteen Python blob-palvelun avulla. Korvaa tiettyjä arvoja muuttujan koodi: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. Lue tietoja kehykseksi Pandas data-ladatusta tiedostosta.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nyt olet valmis Luo tämän DataSet-ryhmän ominaisuuksia ja tietoja.


##<a name="blob-dataexploration"></a>Tietoja talteenoton

Seuraavassa on muutamia esimerkkejä tutkia tietoja Pandas avulla:

1. Tarkasta rivien ja sarakkeiden määrä 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Tarkista alla kuin dataset-ryhmän ensimmäinen tai viimeinen vähän rivejä:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Tarkista kussakin sarakkeessa on tuotu Seuraava mallikoodi käyttämällä tietotyyppi
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Tarkista perustiedot stats tietojoukon sarakkeissa seuraavasti
 
        dataframe_blobdata.describe()
    
5. Katso kunkin sarakkeen arvon tapahtumien määrä seuraavasti

        dataframe_blobdata['<column_name>'].value_counts()

6. Määrä todellisen määrän ja kunkin sarakkeen avulla Seuraava mallikoodi tapahtumat puuttuvat arvot

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Jos olet puuttuu arvoja tietyn sarakkeen tiedot, pudota ne seuraavasti:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Moodi-funktio on toinen tapa korvata puuttuvia arvoja:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Luo avulla vaihteleva määrä varastopaikkoja muuttujan jakauman piirretään Histogrammi koealan 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Katso korrelaatioita muuttujat scatterplot tai sisäinen correlation-funktio

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>Toiminnon luominen
    
Emme voi luoda ominaisuuksia käyttämällä Python seuraavasti:

###<a name="blob-countfeature"></a>Ilmaisimen arvon perusteella toiminnon luonti

Categorical toimintoja voidaan luoda seuraavasti:

1. Tarkasta categorical sarakkeen jakautuminen:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Luo ilmaisin arvot kunkin sarakkeen arvot

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Liity ilmaisin-sarakkeeseen alkuperäisen kehyksen tietojen kanssa 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Poista alkuperäinen muuttuja itse:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Binning toiminnon luonti

Luonnin binned ominaisuuksia, on toimittava seuraavasti:

1. Lisää numeerinen sarake lokeroon sarakkeiden järjestystä
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Muunna binning järjestyksen boolean-muuttujat

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Liity lopuksi takaisin tietoja alkuperäisessä kehyksessä nuken muuttujat

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Kirjoittaminen takaisin Azure blob ja kuluttaa Azure Machine Learning-

Jälkeen luonut tarvittavat ominaisuudet, voit ladata tiedot ja tiedot on selvitetty (näyte tai featurized), Azure blob ja käyttää sen Azure Machine Learning seuraavasti: Huomaa, että Azure Machine Learning Studiossa sekä luoda lisäominaisuuksia. 
1. Tiedot kehys kirjoittaa paikalliseen tiedostoon

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Lataa Azure blob tiedot seuraavasti:

        from azure.storage.blob import BlobService
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

3. Nyt tiedot voidaan lukea Azure Machine Learning [Tuontitiedot] blob[ import-data] moduulin näytön alla esitetyllä tavalla:
 
![Reader-blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
