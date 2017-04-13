<properties
pageTitle="Useiden mallien luominen yhden kokeen | Microsoft Azure"
description="PowerShellin avulla voit luoda useita tietokoneen Learning mallit ja web päätepisteiden samoja mutta erilaisia koulutusta tietojoukkoja."
services="machine-learning"
documentationCenter=""
authors="hning86"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="10/03/2016"
ms.author="garye;haining"/>

# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Useimmat tietokoneen Learning mallit ja web päätepisteiden luominen yhden kokeen PowerShellin avulla

Seuraavassa on yleinen koneen learning ongelma: haluat luoda monta tietomalleja, joissa on saman koulutus työnkulun ja käyttää samaa algoritmin, mutta on eri koulutus tietojoukkoja syötteenä. Tässä artikkelissa kerrotaan, miten toiminto käytetään Azure koneen Learning Studio käyttämällä vain yhden kokeen.

Esimerkiksi oletetaan, että omistat yleinen polkupyörien vuokrausta konsessio yrityksen. Haluat luoda regression mallin ennustaa historiatietoihin perusteella vuokrausta tarvittaessa. Sinulla on 1 000 vuokrausta sijainnit kaikkialla maailmassa ja olet kerätään tietojoukko sijaintiin, joka sisältää tärkeitä ominaisuuksia, kuten päivämäärän, kellonajan, sää ja liikenne, jotka liittyvät kuhunkin sijaintiin.

Onnistunut kouluttaminen mallin kaikki tietojoukkoja yhdistetyt version käyttö kerran kaikki sijainnit. Mutta koska kaikista sijainneista on yksilöllinen-ympäristö, vaihtoehto olisi kouluttaminen erikseen avulla dataset jokaisen sijainnin regression mallin. Sen mukaan, koulutetun malleille voi huomioon eri kokoja, äänenvoimakkuutta, geography, populaation, polkupyörien soveltuvia liikenne ympäristössä, *jne*.

Joka voi olla paras tapa, mutta et halua 1 000 koulutus kokeissa luominen Azure koneen Learning kunkin yksi edustava ainutkertainen sijainti. Sen lisäksi, että hankalaa tehtävän, se on myös vaikuttaa melko tehotonta, koska kunkin kokeen on samat osien koulutus-tietojoukko lukuun ottamatta.

Olemme lähettäjä, tehdä tämän käyttämällä [Azure koneen Learning uudelleenkoulutusta API](machine-learning-retrain-models-programmatically.md) ja automatisointi [PowerShellin Azure koneen Learning](machine-learning-powershell-module.md)tehtävään.

> [AZURE.NOTE] Microsoftin otoksen merkittävästi parantaa suorituskykyä, jotta pienennetään määrällä sijainteja 1 000-10. Mutta saman periaatteet ja menettelyt koskevat 1 000 sijainteihin. Ainoa ero on, että voit halutessasi kouluttaminen-1 000 tietojoukkoja haluat todennäköisesti ajatella PowerShell-komentosarjojen suorittaminen rinnakkain. Voit tehdä on tämän artikkelin laajemmin, mutta voit etsiä PowerShell esimerkkejä monisäikeisyys Internetissä.  

## <a name="set-up-the-training-experiment"></a>Koulutus kokeen määrittäminen

Tutustutaan käyttäminen esimerkiksi [koulutus kokeilla](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) , että olet jo luonut [Cortana Liiketoimintatieto-valikoima](http://gallery.cortanaintelligence.com). Avaa tämä kokeen [Azure koneen Learning Studio](https://studio.azureml.net) työtilassa.

>[AZURE.NOTE] Jotta voit seurata sekä tässä esimerkissä, haluat ehkä käyttää vakio työtilan vapaa työtilan sijaan. Olemme luot päätepiste kunkin asiakkaan - 10 päätepisteet - yhteensä ja, jotka edellyttävät vakio työtilan jälkeen vapaa työtilan on rajoitettu 3 päätepisteet. Jos sinulla on vain vapaa työtilan, Muokkaa alla sallimaan vain 3 sijaintien komentosarjoja.

Kokeen käyttää **Tietojen tuonti** -moduulin koulutusta tietojoukko *customer001.csv* tuominen Azure-tallennustilan tilin. Oletetaan, että olemme on kerätään koulutusta tietojoukkoja kaikki polkupyörien vuokrausta sijainneista ja tallennetaan ne blob storage samassa sijainnissa tiedostonimiä *rentalloc10.csv* *rentalloc001.csv* väliltä.

![Kuva](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

Huomaa, että **WWW-palvelun tulosteen** moduuli on lisätty **Junassa malli** -moduulin.
Kun tämän kokeen otetaan käyttöön web-palvelu, päätepisteen liittyviä, että tulostus palauttaa koulutetun mallin .ilearner tiedoston muoto.

Huomaa, että olemme määrittäminen web-palvelun parametri, joka **Tuontitiedot** -moduuli käyttää URL-osoite. Näin parametrin avulla voit määrittää yksittäisen koulutus tietojoukkoja haluat kouluttaminen mallin jokaisen sijainnin.
Useilla eri tavoilla on voitu olet tehnyt tämän, kuten SQL-kyselyn käyttäminen web-palvelun parametri tietojen käyttämistä SQL Azure-tietokantaan tai välittää DataSet WWW-palvelun yksinkertaisesti **WWW-palvelun Input** -moduulin avulla.

![Kuva](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

Nyt suorittaa oletetaan, että tämä koulutus kokeen käyttämällä oletusarvoista arvo *rental001.csv* koulutus-tietojoukko. Jos tarkastelet **Laske** -moduulin tulos (Valitse tulos ja valitse **Visualisoi**), näet olemme Hae *AUC* decent toimintakykyä = 0.91. Tässä vaiheessa olemme valmis ottamaan verkkopalvelun koulutus-kokeen ulos.

## <a name="deploy-the-training-and-scoring-web-services"></a>Koulutus ja näkyvissä pistemäärä verkkopalvelut käyttöönotto

Koulutus-verkkopalvelun ottamaan kokeen piirtoalustan alapuolella **Määrittää WWW-palvelun** -painiketta ja valitse **WWW-palvelun käyttöön**. Soita verkkosovelluksen "" polkupyörien vuokrausta koulutus".

Nyt annettava tulosmalli WWW-palvelun käyttöön.
Voit tehdä tämän on **Määritetty WWW-palvelun** napsauttamalla piirtoalustan alapuolella ja valitse **Ennakoivan WWW-palvelun**. Tämä luo tulosmalli kokeen.
Microsoft on muutama aliversiot säätäminen sen toimivat web-palveluun, kuten otsikko-sarakkeen "cnt" poistaminen syötteen tiedoista ja rajoittaminen vain tunniste ja sitä vastaava tulosteet ennustettujen arvo.

Tallenna työsi, voit avata ainoastaan [ennakoivan kokeen](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) valikoiman, joka on jo valmisteltu.

WWW-palvelun ottamaan Suorita ennakoivan kokeen ja valitse sitten **WWW-palvelun käyttöön** -painikkeen alla alusta. Tulosmalli verkkopalvelun "Polkupyörien vuokrausta näkyvissä pistemäärä" nimi ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Luo 10 samanlaiset web-Palvelupäätepisteet PowerShellin avulla

Web-palvelu sisältyy oletusarvoinen päätepiste. Mutta emme oletusarvoinen päätepiste kiinnostunut koska sitä ei voi päivittää. Mitä voit tehdä annettava on luotava 10 muita päätepisteet, yhteen kunkin sijainti. Olemme Tee tämä PowerShellin avulla.

Ensin on määrittäminen usealle PowerShell-ympäristöön:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Suorita seuraava komento PowerShell:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Nyt on tullut 10 päätepisteet ja niissä kaikki koulutus- *customer001.csv*koulutetun mallissa. Voit tarkastella niitä Azure hallinta-portaalissa.

![Kuva](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Päivitä päätepisteet käyttämään erillisessä koulutus tietojoukkoja PowerShellin avulla

Seuraavaksi Päivitä päätepisteet yksilöllisesti koulutus kunkin asiakkaan yksittäisten tietojen mallit. Mutta ensin annettava tuottaa **Polkupyörien vuokrausta koulutus** verkkopalvelusta näistä malleista. Mennään takaisin **Polkupyörien vuokrausta koulutusta** web-palveluun. Soita sen BES päätepisteen 10 kertaa 10 eri koulutus tietojoukkoja haluat tuottaa 10 eri mallien annettava. Esitellään toiminto **InovkeAmlWebServiceBESEndpoint** PowerShell-cmdlet-komennon avulla.

Sinun on myös tunnistetietoja blob storage tilin `$configContent`, eli, kenttien `AccountName`, `AccountKey` ja `RelativeLocation`. `AccountName` Voi olla jokin tili-nimistä tarkastelu **Perinteinen Azure hallinta-portaalin** (*tallennustila* -välilehdessä). Valittuasi tallennustilan-tilissä sen `AccountKey` löytyy alareunassa **Pikanäppäinten hallinta** -painikkeen ja kopioimalla *Access perusavain*. `RelativeLocation` On suhteessa tallennustilan polku, johon uusi malli tallennetaan. Esimerkiksi polku `hai/retrain/bike_rental/` alapuolella kohdeosoite-niminen säilön komentosarjan `hai`, ja `/retrain/bike_rental/` ovat alikansioita. Tällä hetkellä voi luoda alikansioita palvelun Käyttöliittymän-portaalissa, mutta on [Useita Azure-tallennustilan hallinnasta](../storage/storage-explorers.md) , jotta voit tehdä. On suositeltavaa, että luot uuden säilön tallennustilaan tallentaa uudet koulutetun mallit (.ilearner tiedostot) seuraavasti: tallennustilan-sivun alalaidassa olevaa **Lisää** -painiketta napsauttamalla ja anna sille nimi `retrain`. Yhteenvedossa, alla olevaa komentosarjaa tarvittavat muutokset koskevat `AccountName`, `AccountKey` ja `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

>[AZURE.NOTE] BES päätepiste on tämän toiminnon vain tuetut-tilassa. RRS ei voi käyttää tuottavat koulutetun mallit.

Kuten näet yllä sijaan luomisesta 10 eri BES töiden määritysten json tiedostoja, syy luoda config merkkijonon sijaan dynaamisesti ja syötteen *jobConfigString* parametrin **InvokeAmlWebServceBESEndpoint** cmdlet-komento, koska se on todella ei tarvitse säilyttää viestistä kopion levyllä.

Jos kaikki sujuu hyvin, hetken kuluttua pitäisi näkyä 10 .ilearner tiedostoja, voit *model010.ilearner* *model001.ilearner* Azure-tallennustilan tilin. On nyt päivittää Microsoftin 10 tulosmalli web päätepisteiden näitä malleja **Korjaustiedoston AmlWebServiceEndpoint** PowerShell-cmdlet-komennolla. Muista uudelleen, että olemme vain korjaustiedoston ohjelmallisesti luomaasi aiemmin muun kuin oletusarvoisen päätepisteet.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Pitäisi toimia varsin nopeasti. Suorittamisen päätyttyä Microsoft luonut 10 ennakoivan web Palvelupäätepisteet, sisältävä koulutus yksilöllisesti tietyn tietojoukko vuokrausta sijaintiin, kaikki yksittäiseen kokeen koulutetun malli. Tarkistaa, voit yrittää soittaa nämä päätepisteet **InvokeAmlWebServiceRRSEndpoint** -cmdlet-komennolla tarjoamiseksi ne samaan kenttään annettavat tiedot ja olisi oletat hakutuloksia, koska mallit ovat koulutus kanssa eri koulutus määrittää eri tekstinsyöttö.

## <a name="full-powershell-script"></a>Koko PowerShell-komentosarjaa

Näin koko lähdekoodin luettelo:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
