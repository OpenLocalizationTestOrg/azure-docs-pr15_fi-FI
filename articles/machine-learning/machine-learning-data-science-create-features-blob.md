<properties
    pageTitle="Azure-blob storage tietoja käyttämällä Panda toimintoja | Microsoft Azure"
    description="Miten voit luoda tietoihin, jotka on tallennettu Azure-blob-säilö Panda Python pakkaaminen ominaisuuksia."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Luo Azure blob storage tietoja käyttämällä Panda ominaisuudet

Tässä asiakirjassa näkyy luomisesta tietoihin, jotka on tallennettu Azure-blob-säilö [Pandas](http://pandas.pydata.org/) Python pakkaaminen ominaisuuksia. Jälkeen jäsennystä tietojen lataaminen Panda tietojen kehys, se näyttää, miten Luo categorical ominaisuuksista Python komentosarjojen käyttäminen ilmaisin arvot ja binning ominaisuuksia.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Tässä **valikossa** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit luoda tietojen ominaisuudet eri ympäristöissä. Tämä onkin vaiheessa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="prerequisites"></a>Edellytykset

Tässä artikkelissa oletetaan, että olet luonut Azure-blob-tallennustilan tilin ja tallennettuja tietoja. Jos tarvitset ohjeita, määritä ensin tili, katso [Azure-tallennustilan tilin luominen](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Lataa tiedot Pandas tietojen kehys
Jotta voit tarkastella ja käsitellä tietojoukko, jos se ladataan blob lähteestä paikallisen tiedostoon, joka voi ladata sitten Pandas tietojen kehyksessä. Näin voit seurata toiminto:

1. Lataa tiedot azuren blob-koodilla seuraava näyte Python blob-palvelun avulla. Korvaa koodissa muuttujan määritetyistä arvoista:

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

##<a name="blob-featuregen"></a>Ominaisuuden luominen

Seuraavissa kahdessa osiossa näyttää, miten Luo categorical ominaisuudet, joita ilmaisin arvot ja binning Python komentosarjat-ominaisuudet.

###<a name="blob-countfeature"></a>Ilmaisimen arvon perusteella ominaisuuden luominen

Categorical ominaisuudet voidaan luoda seuraavasti:

1. Tarkasta categorical sarakkeen jakautumisen:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Luo ilmaisin arvot kunkin sarakkeen arvot

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Alkuperäiset tiedot kehyksen ilmaisinsarakkeen liittyminen

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Poistaa alkuperäisen muuttujan itse:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>Binning ominaisuuden luominen

Luonnissa binned ominaisuuksia, on toimittava seuraavasti:

1. Lisää sarakkeita palkin numerosarake järjestyksessä

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Muuntaa binning järjestyksen totuusarvo muuttujat

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Liittyminen lopuksi takaisin alkuperäiseen tietojen kehyksen tyhjä muuttujat

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Tietojen kirjoittaminen takaisin Azure-blob ja muissa Azure koneen oppiminen

Kun tiedot on tarkasteltavia ja luonut tarvittavat ominaisuudet, voit ladata tiedot (näyte tai featurized), Azure blob ja käyttää sen Azure koneen Learning seuraavalla tavalla: Huomaa, että Azure koneen Learning Studiossa sekä luoda lisäominaisuuksia.
1. Kirjoita tiedot kehyksen paikallisen tiedoston

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Tietojen lataaminen Azure-blob seuraavasti:

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

3. Nyt tiedot voidaan lukea Blob-objektien käyttäminen Azure koneen Learning [Tietojen tuominen](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) -moduulin alla olevassa kuvassa esitetyllä tavalla:

![Lukija-blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
