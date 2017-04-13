<properties 
    pageTitle="Azure-blob-säiliö ja Pandas tietojen tutkiminen | Microsoft Azure" 
    description="Voit tutkia tietoja, jotka on tallennettu käyttämällä Pandas Azure-blob-säilö." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Azure-blob-säiliö ja Pandas tietojen tutkiminen

Tässä asiakirjassa kerrotaan, miten voit tarkastella tietoja, jotka on tallennettu käyttämällä [Pandas](http://pandas.pydata.org/) Python paketin Azure-blob-säilö.

**Valikon** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit käyttää työkaluja tietojen tutkiminen tallennustilan eri ympäristöissä. Tämä onkin [Tietojen tiede prosessin]()vaiheessa.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että sinulla on:

* Luoda Azure-tallennustilan tilin. Jos tarvitset ohjeita, katso [Azure-tallennustilan tilin luominen](../storage/storage-create-storage-account.md#create-a-storage-account)
* Tallennettu Azure-blob-tallennustilan tilin tiedot. Jos tarvitset ohjeita, katso [Siirtyminen tiedot ja sieltä pois Azuren tallennustilaan](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Lataa tiedot Pandas DataFrame
Jotta voit tarkastella ja käsitellä tietojoukko, se on ensin ladattava blob lähteestä paikallisen tiedostoon, joka voidaan ladata sitten Pandas DataFrame. Näin voit seurata toiminto:

1. Lataa tiedot Azure blob-ja Seuraava Python koodin malli blob-palvelun avulla. Korvaa seuraava koodi muuttujan määritetyistä arvoista: 

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


2. Lue tietoja Pandas tiedot-kehys ladatusta tiedostosta.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nyt olet valmis, voit tarkastella tietoja ja luoda tämän dataset-ominaisuuksia.

##<a name="blob-dataexploration"></a>Esimerkkejä tietojen tarkasteluun Pandas käyttäminen

Seuraavassa on muutamia esimerkkejä tietojen Pandas käyttäminen:

1. Tarkasta **rivien ja sarakkeiden määrä** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Tarkasta** ensimmäisen tai viimeisen muutamia **rivien** seuraavat tietojoukko:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Tarkista kunkin sarakkeen on tuotu käyttämällä seuraavaa **tietotyyppi**
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Valitse **Perus tilasto** tietojoukon sarakkeissa seuraavasti
 
        dataframe_blobdata.describe()
    
5. Tarkista kunkin sarakkeen arvon tapahtumien määrä seuraavasti

        dataframe_blobdata['<column_name>'].value_counts()

6. **Laske puuttuvat arvot** ja tosiasiallinen määrä on kunkin sarakkeeseen käyttämällä seuraavaa tapahtumat

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Jos sinulla on tiedot tietyn sarakkeen **puuttuvia arvoja** , voit poistaa ne seuraavasti:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Toinen tapa korvaa puuttuvat arvot moodi-funktio on:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Luo **Histogrammi** -piirron piirtää muuttujan jakautumisen muuttujan palkkien määrä avulla 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Tarkista **korrelaatioita** välillä muuttujien käyttäminen scatterplot tai valmiin korrelaatio-funktion käyttäminen

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
