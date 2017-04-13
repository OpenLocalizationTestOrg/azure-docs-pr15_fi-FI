<properties 
    pageTitle="Mallitiedot Azure blob storage | Microsoft Azure" 
    description="Mallitiedot Azure-Blob-objektien tallennustilaan" 
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

#<a name="heading"></a>Mallitiedot Azure blob storage


Tässä asiakirjassa kerrotaan esimerkkejä tietojen Azuren Blob-objektien tallennustilaan tallennettuja lataamalla se ohjelmallisesti ja näytteiden se kirjoitettu Python tavoilla.

**Esimerkki siitä, miksi tiedot?**
Jos haluat analysoida tietojoukko on suuri, se on yleensä hyvä alas otoksen voit pienentää koko on pienempi, mutta edustava ja helpommin hallittaviin tiedot. Tämä helpottaa tietojen tietoja, tarkasteluun ja ominaisuus tekniikka. Cortana Analytics prosessin sen rooli on nopea prototyyppiä tietojen käsittely-toimintojen ja konepohjaisten oppimistekniikoiden mallit käyttöön.

**Valikon** alla linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit eri tallennustilan ympäristöissä mallitiedot. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Esimerkkejä tehtävä on vaiheessa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Lataa ja alas-mallitiedot
1. Lataa tiedot Azure-blob-säiliö otoksen Python-koodista blob-palvelun avulla: 

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

2. Tietojen lukeminen Pandas tiedot-kehys-tiedostosta ladataan yläpuolella.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Alas otoksen tietojen avulla `numpy`'s `random.choice` seuraavasti:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Voit nyt käsitellä yllä tietojen kehys lisätietoja ja ominaisuus luonti 1 prosentti-malli.

##<a name="heading"></a>Lataa tiedot ja lukea Azure koneen oppiminen

Voit käyttää seuraavaa alas otoksen tietoja ja käyttää sitä Azure millilitroina:

1. Kirjoita tiedot kehyksen paikallisen tiedoston

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Lataa paikallinen tiedosto Azure-blob käyttämällä seuraavaa:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Tietojen lukeminen Azuren Blob-objektien käyttäminen Azure ML [Tuontitiedot](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) alla olevassa kuvassa esitetyllä tavalla:
 
![Lukija-blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
