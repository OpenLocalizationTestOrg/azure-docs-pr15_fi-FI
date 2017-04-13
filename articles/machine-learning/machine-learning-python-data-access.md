<properties 
    pageTitle="Käyttää tietokoneen Learning Python asiakas-kirjaston kanssa tietojoukkoja | Microsoft Azure" 
    description="Asenna ja määritä Python asiakkaan kirjaston hallita Azure koneen Learning tietoja suojatusti paikalliseen Python ympäristöstä ja käyttää." 
    services="machine-learning" 
    documentationCenter="python" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="huvalo;bradsev" />


#<a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a>Access-tietojoukkoja Python käyttämällä Azure koneen Learning Python asiakas-kirjaston kanssa 

Microsoft Azure koneen Learning Python asiakkaan kirjaston esikatselu voit suojattu käyttö Azure koneen Learning tietojoukkoja haluat paikallisen Python ympäristöstä ja avulla luominen ja hallinta-työtilassa tietojoukkoja.

Tämä artikkeli sisältää ohjeita siitä, miten voit:

* Asenna tietokoneen Learning Python asiakas-kirjasto 
* käyttää ja lataa tietojoukkoja, mukaan lukien käyttöoikeuden saaminen Azure koneen Learning tietojoukkoja käyttää Python paikallisen ympäristön ohjeet
*  keskitason tietojoukkoja käyttäminen kokeissa
*  Luetteloi tietojoukkoja, käyttää metatietoja, tietojoukko sisällön lukeminen, Luo uusi tietojoukkoja ja Päivitä aiemmin tietojoukkoja Python asiakas-kirjaston avulla

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

 
##<a name="prerequisites"></a>Edellytykset

Python asiakkaan kirjaston testattu seuraavat ympäristöissä-kohdassa:

 - Windows-, Mac- ja Linux
 - 2.7, 3.3 ja 3.4 Python

Se on riippuvuussuhteessa seuraavat paketit:

 - palvelupyynnöt
 - Python dateutil
 - pandas

On suositeltavaa käyttää Python jakauman, kuten yllä olevassa luettelossa [Anaconda](http://continuum.io/downloads#all) tai [kupua](https://store.enthought.com/downloads/), jotka sisältyvät Python IPython ja kolme pakettien asennettuna. Mutta IPython ei ole täysin pakollinen, se on hyvä käyttöympäristön käsittelemisestä ja kesäolympialaisten visualisointi tietoja vuorovaikutteisesti.


###<a name="installation"></a>Asennusohjeet Azure koneen Learning Python asiakas-kirjasto

Tässä artikkelissa kuvattujen toimien suorittamiseen on oltava asennettuna Azure koneen Learning Python asiakas-kirjasto. Ei käytettävissä [Python paketin indeksi](https://pypi.python.org/pypi/azureml). Voit asentaa sen Python ympäristön pitäminen Python paikallisen ympäristön seuraava komento:

    pip install azureml

Vaihtoehtoisesti voit ladata ja asentaa [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python)-lähteistä.

    python setup.py install

Jos sinulla on asennettu tietokoneeseesi git, voit asentaa suoraan git säilö pip:

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


##<a name="datasetAccess"></a>Studio koodi katkelmat avulla voit käyttää tietojoukkoja

Python asiakas-kirjaston avulla voit ohjelmallisesti aiemmin tietojoukkoja-tutkimuksiin, jotka on suoritettu.

Liittymästä Studio web voit luoda koodikatkelmat, jotka sisältävät kaikki tarvittavat tiedot, voit ladata ja poistaa tietojoukkoja Pandas DataFrame objekteina sijainnin tietokoneessasi.

### <a name="security"></a>Tietojen käyttö suojaus

Koodikatkelmat myöntämä Studio käyttäminen Python asiakkaan kirjasto sisältää työtilan tunnuksen ja luvan tunnuksen. Nämä on täydet oikeudet työtilassa ja suojattava, kuten salasanan.

Tietoturvasyistä koodin koodikatkelman-toiminto on käytettävissä vain käyttäjille, jotka on määritelty **omistaja** työtilan rooli. Tehtäväsi näkyy Azure koneen Learning Studiossa **käyttäjät** -sivulla **asetukset**-kohdassa.

![Tietoturva][security]

Jos tehtäväsi ei ole määritetty **omistaja**, voit joko pyyntö kutsutuksi omistajaksi tai pyydä omistajaa työtilan tarjota koodikatkelman.

Todennus-tunnuksen hankkiminen jokin seuraavista toimista:



- Pyydä omistaja-tunnusta. Omistajat voivat tarkastella niiden luvan tunnusten niiden työtilan Studiossa asetukset-sivulla. Valitse vasemmanpuoleisesta ruudusta **asetukset** ja valitse **Luvan TUNNUSTEN** Nähdäksesi ensisijainen ja toissijainen tunnusten.  Ensisijaisen tai toissijaisen luvan tunnusten avulla voidaan koodikatkelman, on suositeltavaa, että omistajat jakaa vain toissijainen luvan tunnusten.

![](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

- Saada ylennyksen roolin omistajan pyytää.  Voit tehdä tämän työtilan nykyistä omistajaa on ensin poistaminen työtilan sitten uudelleen kutsua siihen omistajaksi.

Kun kehittäjät hankkinut työtila-tunnus ja luvan käyttöoikeustietue, he voivat käyttää työtilaan käyttämällä koodikatkelman rooli riippumatta.

Luvan tunnusten hallitaan **Luvan TUNNUSTEN** sivun **asetukset**-kohdassa. Voit luoda ne uudelleen, mutta tämä toiminto poistaa edellisen tunnuksen käyttöoikeus.

### <a name="accessingDatasets"></a>Access-tietojoukkoja paikallisen Python-sovelluksesta

1. Valitse tietokoneen Learning Studio vasemman reunan siirtymispalkissa **TIETOJOUKKOJA** .

2. Valitse tietojoukko haluat käyttää. Voit valita tietojoukkoja **Oma TIETOJOUKKOJA** -luettelosta tai **Mallit** -luettelosta.

3. Napsauta **Luo tietojen valintanumero**alareunan-työkalurivi. Jos tiedot ovat yhteensopivia Python asiakkaan kirjaston muodossa, tämä painike ei ole käytettävissä.

    ![Tietojoukkoja][datasets]

4. Valitse avautuvassa ikkunassa näkyy ja kopioi se Leikepöydän koodikatkelman.

    ![Access-koodi][dataset-access-code]

5. Liitä koodi paikallisen Python-sovelluksen muistikirjaan.

    ![Muistikirjan][ipython-dataset]

## <a name="accessingIntermediateDatasets"></a>Access-koneen Learning kokeissa keskitason tietojoukkoja

Kun kokeessa suoritetaan tietokoneen Learning Studiossa, on mahdollista käyttää keskitason tietojoukkoja moduulit tulosteen solmut. Keskitason tietojoukkoja ovat tietoja, jotka on luotu ja käyttää keskitason vaiheet, kun malli-työkalu on suoritettu.

Keskitason tietojoukkoja niitä voi käyttää, kun tietojen muoto on yhteensopiva Python asiakas-kirjaston kanssa.

Seuraavat muodot ovat tuettuja (nämä vakiot ovat `azureml.DataTypeIds` luokan):

 - Vain teksti
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader

Voit määrittää muoto osoittamalla moduulin tulosteen solmu. Se näkyy sekä Solmunimi, työkaluvihjeessä.

Jotkin moduulit, kuten [Jaa] [ split] moduuli-tulosteen nimeltä muotoon `Dataset`, joka ei tue Python asiakas-kirjasto.

![Tietojoukon muoto][dataset-format]

Sinun on käytettävä muuntaminen moduulin, kuten [CSV muuntaminen][convert-to-csv], saat tulos tuetut muotoon.

![GenericCSV muoto][csv-format]

Seuraavissa vaiheissa kuvataan esimerkki, joka luo koe, suorittaa sen ja käyttää keskitason tietojoukko.

1. Luo uusi koe.

2. Lisää **aikuisen laskenta tulot binaarinen luokitus tietojoukko** -moduuli.

3. Lisää [Jaa] [ split] moduuli, ja liittää sen syöttö tietojoukko moduulin tulostukseen.

4. Lisää [muuntaminen CSV] [ convert-to-csv] moduulin ja muodostaa sen syöttö johonkin [Jaa] [ split] moduulin tulostaa.

5. Tallenna kokeilla, suorittaa ja odota sen suoritettu loppuun.

6. Valitse [CSV muuntaminen] tulostus-solmu[ convert-to-csv] moduuli.

7. Kun pikavalikon tulee näkyviin, valitse **Luo tietojen valintanumero**.

    ![Pikavalikko][experiment]

8. Valitse koodikatkelman ja kopioi se leikepöytääsi-ikkunasta, joka tulee näkyviin.

    ![Access-koodi][intermediate-dataset-access-code]

9. Liitä koodi muistikirjan.

    ![Muistikirjan][ipython-intermediate-dataset]

10. Voit esittää tiedot käyttämällä matplotlib. Tämä näyttää Histogrammi ikä-sarakkeen:

    ![Histogrammi][ipython-histogram]


##<a name="clientApis"></a>Koneen Learning Python asiakas-kirjaston avulla voit käyttää, lukea, luoda ja hallita tietojoukkoja

### <a name="workspace"></a>Työtilan

Työtila on Python asiakkaan kirjaston aloituskohdan. Anna `Workspace` luokan työtila-tunnus ja luvan tunnussanoma luominen:

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a>Luetteloi tietojoukkoja

Luettelointi kaikki tietojoukkoja annetun työtilassa:

    for ds in ws.datasets:
        print(ds.name)

Luettelointi vain käyttäjän luoma tietojoukkoja:

    for ds in ws.user_datasets:
        print(ds.name)

Luettelointi Esimerkki tietojoukkoja:

    for ds in ws.example_datasets:
        print(ds.name)

Voit käyttää tietojoukon nimi (jossa kirjainkoko on merkitsevä):

    ds = ws.datasets['my dataset name']

Tai voit avata sen indeksin mukaan:

    ds = ws.datasets[0]


### <a name="metadata"></a>Metatietojen

Tietojoukkoja on metatietojen sisällön lisäksi. (Keskitason tietojoukkoja ovat poikkeus tähän sääntöön ja ei ole mitään metatietojen.)

Jotkin metatietoarvojen on liitetty käyttäjän luomisen:

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

Toiset ovat Azure ML määrittämät arvot:

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

Katso `SourceDataset` luokan Saat lisätietoja saatavilla metatiedot.


### <a name="read-contents"></a>Sisällön lukeminen

Myöntämä koneen Learning Studio automaattisesti koodikatkelmat Lataa ja poistaa tietojoukon Pandas DataFrame objektiin. Tämä `to_dataframe` menetelmää:

    frame = ds.to_dataframe()

Jos haluat ladata perustietoja ja suorittaa sarjoituksen, joka on haluamasi vaihtoehto. Tällä hetkellä tämä on ainoa tapa muodoissa, kuten "ARFF", jonka Python asiakas-kirjastoa ei voi poistaa.

Voit lukea sisältöä tekstinä seuraavasti:

    text_data = ds.read_as_text()

Voit lukea sisällön binaarimuodossa seuraavasti:

    binary_data = ds.read_as_binary()

Voit avata yksinkertaisesti stream sisältöön:

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a>Luo uusi tietojoukko

Python asiakkaan kirjastossa voit ladata tietojoukkoja Python-ohjelmasta. Nämä tietojoukkoja ovat käytettävissä työtilassa.

Jos tiedot ovat Pandas DataFrame, käytä seuraava koodi:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Jos tiedoissa on jo esittää sarjana, voit käyttää:

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

Python asiakkaan kirjastoa ei voi muuntaa sarjaksi Pandas-DataFrame seuraaviin muotoihin (nämä vakiot ovat `azureml.DataTypeIds` luokan):

 - Vain teksti
 - GenericCSV
 - GenericTSV
 - GenericCSVNoHeader
 - GenericTSVNoHeader


### <a name="update-an-existing-dataset"></a>Päivitä aiemmin tietojoukko

Jos yrität ladata uuden tietojoukon, jonka nimi on sama olemassa olevan tietojoukon, hanki ristiriita-virheen.

Jos haluat päivittää aiemmin tietojoukko, sinun on viittaaminen aiemmin tietojoukko:

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Käytä `update_from_dataframe` muuntaa sarjaksi ja korvaa-Azure tietojoukko sisältö:

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Jos haluat muuntaa sarjaksi tiedot toiseen muotoon, määritä arvo on valinnainen `data_type_id` parametria.

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

Voit määrittää uusi kuvaus määrittämällä arvo `description` parametria.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

Voit määrittää uuden nimen määrittämällä arvo `name` parametria. Tämän jälkeen noutaa tietojoukko käyttämällä uusi nimi. Seuraava koodi päivittää tiedot, nimi ja kuvaus.

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

`data_type_id`, `name` Ja `description` parametrit ovat valinnaisia ja niiden edellisen arvon oletus. `dataframe` Parametri on aina pakollinen.

Jos tiedoissa on jo esittää sarjana, käytä `update_from_raw_data` sijaan `update_from_dataframe`. Jos olet juuri välitä `raw_data` sijaan `dataframe`, se toimii samalla tavalla kuin.



<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
 
