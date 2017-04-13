<properties 
    pageTitle="Tietoja tiede-Linux tietojen tiede virtuaalikoneen | Microsoft Azure" 
    description="Voit suorittaa useita yleisiä tietoja tiede tehtävien Linux tietojen tiede AM." 
    services="machine-learning"
    documentationCenter="" 
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
    ms.author="bradsev;paulsh" />


# <a name="data-science-on-the-linux-data-science-virtual-machine"></a>Tietoja tiede-Linux tietojen tiede virtuaalikoneen

Näiden vaiheiden avulla voit suorittaa useita yleisiä tietoja tiede tehtävien Linux tietojen tiede AM. Linux tietojen tiede virtuaalikoneen (DSVM) on käytettävissä, joka on asennettu valmiiksi työkaluja käytettyjä tietoja analytics ja tietokoneen learning Azure virtuaalikoneen kuva. Avaimen ohjelmiston osat ovat eritelty [säännöstä Linux tiede virtuaalikoneen](machine-learning-data-science-linux-dsvm-intro.md) artikkelissa. AM-kuva on helppo Aloita suorittamalla tietojen tiede minuutteina, eikä sinun tarvitse asentaa ja määrittää työkalujen yksitellen. Voit helposti laajentaa AM, tarvittaessa ja Pysäytä se, jos ne eivät ole käytössä. Resurssi on niin sekä kustannukset säästävän joustavasti. 

Datatehtävien tiede esitellään tämä vaihe vaiheelta seuraa artikkelin [Ryhmän tietojen tiede prosessi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Tämä toimenpide on järjestelmällinen lähestymistapa, jonka tiedot tiede, joka mahdollistaa tietojen tutkijoiden tehdä yhteistyötä tehokkaasti elinkaari älykkäät sovellusten kautta ryhmiä. Tietoja tiede prosessin sisältää myös tietojen tiede, jossa esitetään katsottava iteratiivinen framework.

Olemme analysoida [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) dataset tämän vaiheittaisen kuvauksen suorittamiseksi. Tämä on joukko sähköpostit, jotka on merkitty roskapostia tai pekoni (eli ne eivät ole roskapostia), ja sisältää joitakin tilastoja sähköpostit sisällöstä. Sisällyttää tilastotiedot käsitellään seuraavan, mutta osa. 


## <a name="prerequisites"></a>Edellytykset

Ennen kuin voit käyttää Linux tietojen tiede Virtual Machine, sinulla on oltava seuraavasti:

- On **Azure tilauksen**. Jos sinulla ei ole yhtä, katso [tänään ilmaisen Azure tilin luominen](https://azure.microsoft.com/free/).
- [**Linux tietojen tiede AM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Lisätietoja tämän AM valmistelu on artikkelissa [säännöstä Linux tiede virtuaalikoneen](machine-learning-data-science-linux-dsvm-intro.md). 
- [X2Go](http://wiki.x2go.org/doku.php) tietokoneessasi, ja avata XFCE-istunto. Tietoja asennuksesta ja määrityksestä **X2Go asiakas**on artikkelissa [asentaminen ja määrittäminen X2Go asiakas](machine-learning-data-science-linux-dsvm-intro.md#Installing-and-configuring-X2Go-client). 
- **AzureML tili**. Jos sinulla ei ole vielä yksi uusi osoitteessa [AzureML kotisivun](https://studio.azureml.net/)rekisteröityminen. Tällä vapaa käyttö taso, joiden avulla pääset alkuun.


## <a name="download-the-spambase-dataset"></a>Lataa spambase-tietojoukko

[Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) tietojoukko on suhteellisen pieni joukko, joka sisältää vain 4601 esimerkkejä. Tämä on kätevä koko käyttää osoittaa, että joitakin ominaisuuksia tietojen tiede AM sellaisena kuin se säilyttää resurssivaatimusten ohjelmaa.

>[AZURE.NOTE] Tätä vaiheittaista on luotu, D2 v2 kokoisen Linux tietojen tiede Virtual koneen. Tämä koko DSVM pystyy käsittely tätä vaiheittaista ohjeita.

Jos tarvitset lisää tallennustilaa, voit luoda uusia levyjä ja liitä ne oman AM. Näiden levyjen Käytä pysyvän Azure säilön, jotta sen tiedot säilyvät myös silloin, kun palvelin on valmistella uudelleen vuoksi koon tai suljetaan. Levyn ja liittää sen oman AM, noudata [Lisää Linux AM levylle](../virtual-machines/virtual-machines-linux-add-disk.md). Näiden ohjeiden avulla Azure komentorivikäyttöliittymä (Azure CLI), joka on jo asennettu DSVM. Niin näitä toimintosarjoja voidaan toteuttaa suoraan AM itse. Lisää tallennustilaa toinen vaihtoehto on käyttää [Azure-tiedostoja](../storage/storage-how-to-use-files-linux.md).

Voit ladata tietoja, Avaa pääte-ikkuna ja suorita tämä komento:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Ladattu tiedosto ei ole otsikkorivi, niin japanin luominen toiseen tiedostoon, joka on otsikko. Suorita tämä komento luo tiedosto, jossa haluamasi otsikot:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Sitten yhdistää kaksi tiedostoja yhdessä komennon:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Tietojoukon on erilaisia tilastotiedot kunkin sähköpostitunnuksen: 

- Sarakkeet, kuten ***word\_taajuus\_WORD*** ilmaista uusien sähköpostien sanat, jotka vastaavat *WORD*prosentteina. Esimerkiksi jos *word\_taajuus\_tehdä* on 1, valitse kaikki sanat sähköpostien 1 % on *tehdä*. 
- Sarakkeet, kuten ***merkki\_taajuus\_merkki*** ilmaista uusien sähköpostien kaikki merkit, jotka olivat *merkki*prosentteina. 
- ***pääoman\_suorittaa\_pituus\_pisimmän*** on pisimmän pituus isoilla kirjaimilla järjestyksessä. 
- ***pääoman\_suorittaa\_pituus\_keskimääräinen*** on keskipituuden isoilla kirjaimilla kaikki sarjaa. 
- ***pääoman\_suorittaa\_pituus\_yhteensä*** on isoilla kirjaimilla kaikki järjestyksiä pituinen. 
- ***roskapostin*** ilmaisee, onko sähköposti on tulkitaan roskapostiksi tai ei (1 = Roskaposti, 0 = ei roskapostia).


## <a name="explore-the-dataset-with-microsoft-r-open"></a>Tutustu kanssa R Avaa tietojoukko

Oletetaan, että tutkia tietoja ja tehdä joitakin basic konepohjaisten oppimistekniikoiden avulla R. Tietoja tiede AM sisältyy [R Avaa](https://mran.revolutionanalytics.com/open/) valmiiksi asennettuna. Tässä versiossa R monisäikeisten matemaattiset-kirjastojen tarjouksen kuin yhden threaded eri versioissa suorituskyvyn parantamiseksi. R Avaa myös tarjoaa uusittavissa käyttämällä tilannevedoksen CRAN paketin säilö.

Saat käyttää tätä vaiheittaista MALLIKOODEJA kopiot-Kloonaa käyttämällä git, joka on asennettu valmiiksi AM **Azure-Machine-Learning-tiedot-tiede** säilö. Git komentoriviltä suorittaa:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Avaa pääte-ikkuna ja R uuden istunnon aloittaminen R vuorovaikutteinen konsolin.

>[AZURE.NOTE] Voit käyttää myös RStudio seuraavia ohjeita. Voit asentaa RStudio-komentoa osoitteessa päätteeseen suorittaa:`./Desktop/DSVM\ tools/installRStudio.sh`

Jos haluat tuoda tiedot ja ympäristön määrittäminen, suorita:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Tarkista kunkin sarakkeen yhteenveto tilastotietoja seuraavasti:

    summary(data)

Eri näkymän tiedot:

    str(data)

Tämä näyttää kunkin muuttujan ja ensimmäisen joitakin arvoja dataset. 

*Roskaposti* -sarakkeessa on luettu kokonaislukuna, mutta se on todella categorical muuttujan (tai kerroin). Voit määrittää sen tyyppiä:

    data$spam <- as.factor(data$spam)

Jotkin kokeilevaa analyysi, käytä [ggplot2](http://ggplot2.org/) -paketti, joka on jo asennettu AM R Suositut graphing kirjasto. Huomautus yhteenvetotietoja näkyviin aiempaa versiota, että, kuinka usein huutomerkki merkin on yhteenvetotiedot. Oletetaan, että piirtää näiden taajuuksien tähän seuraavia komentoja:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Koska nolla palkin on muuttaa piirto-oletetaan, että siitä eroon:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Ei-trivial arvon 1, joka näyttää, kiinnostavia yläpuolella. Katsotaan vain tiedot:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Jaa se sitten Roskaposti ja pekoni mukaan:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Näissä esimerkeissä olisi avulla voit tehdä samalla joudutaan muut sarakkeet, voit tarkastella sisältämiä tietoja.


## <a name="train-and-test-an-ml-model"></a>Kouluttaminen ja testaa ML-malli

Nyt kouluttaminen oletetaan, että koneen learning mallien luokitella kuin sisältävä tekstialue tai pekoni tietojoukossa sähköpostit pari. Olemme kouluttaminen päätös puun mallin ja satunnaisia metsää mallin sisältö ja testaa sitten niiden niiden ennusteiden tarkkuutta. 

>[AZURE.NOTE] Seuraava koodi käyttää rpart (rekursiivinen jakaminen ja Regression puita)-paketti on jo asennettu tietojen tiede AM.


Ensimmäinen, Jaa seuraavaksi dataset koulutus- ja määrittää:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

Ja luo sitten Päätöspuun luokitella sähköpostitse.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Näin tulos:

![1](./media/machine-learning-data-science-linux-dsvm-walkthrough/decision-tree.png)

Voit selvittää, kuinka hyvin suorittaa koulutus on määritetty, seuraava koodi:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Voit määrittää, kuinka hyvin se suorittaa testin määrittäminen:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Yritetään satunnaisia metsää mallin myös. Satunnainen metsien kouluttaminen päätös puut monipuolisia ja tulosteen luokka, joka on kaikkien yksittäiset valintapäätöksen puut luokitukset tila. Ne sisältävät Koulujen toimintatapa sen vuoksi, kun ne korjaaminen, overfit koulutus tietojoukko päätös puun mallin taipumusta tehokkaampia koneeseen. 

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Ota malli käyttöön Azure ML

[Azure Konepohjaisten Oppimistekniikoiden Studio](https://studio.azureml.net/) (AzureML) on pilvipalvelussa, joka on helppo muodosta ja ota käyttöön ennakoivan analytics-malleista. Yksi AzureML nice ominaisuuksista on mahdollisuus julkaista R WWW-palvelulle funktiota. AzureML R-paketin tekee käyttöönoton helposti tehdä suoraan Microsoftin R-istuntoon DSVM. 

Ottaa käyttöön linkitys edellisestä osasta päätös puun-koodia, tarvitset Azure koneen Learning Studio kirjautuminen. Tarvitset sigh työtila-Tunnuksesi ja todennus-tunnuksen. Etsi seuraavat arvot ja valmistelu AzureML muuttujat heidän kanssaan:

Valitse vasemmanpuoleisessa valikossa **asetukset** . Huomaa, että **TYÖTILAN tunnus**. ![2](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-id.png)

Valitse Yleiset-valikosta **Luvan tunnusten** ja Huomaa, että **Ensisijainen luvan tunnuksen**. ![3](./media/machine-learning-data-science-linux-dsvm-walkthrough/workspace-token.png)

**AzureML** -paketin lataaminen ja määritä sitten DSVM tunnuksen ja työtilan tunnuksella muuttujan arvot R-istunnossa:


    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Oletetaan, että yksinkertaistaa mallin tässä esittelyssä Toteuta helpompaa. Valitse kolme muuttujaa lähinnä pääkansio Päätöspuun ja luoda uuden puun käyttämällä vain kolme muuttujaa:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Tarvitsemme tekstinsyöttö-funktio, joka otetaan syötteeksi ominaisuuksia ja palauttaa ennustetut arvot:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

Julkaise predictSpam-funktion AzureML **publishWebService** -funktiolla: 

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Tämä funktio on **predictSpam** -funktio, Luo nimeltä **spamWebService** määritetyn syötteiden ja tulostaa verkkopalvelun ja palauttaa uuden päätepisteen tietoja.

Näytä julkaistun web-palveluun, mukaan lukien sen API päätepisteen tiedot ja valintanäppäimet-komennolla:

    spamWebService[[2]]

Voit kokeilla ensimmäisen 10 riviä testin määrittäminen:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Käyttää muita työkaluja, jotka ovat käytettävissä

Jäljellä olevat Näytä käyttämisestä Linux tietojen tiede AM asennettuihin työkaluista. Seuraavassa on luettelo käsitellyt työkalut:

- XGBoost
- Python
- Jupyterhub
- Rattle
- PostgreSQL & Squirrel SQL
- SQL Server Data Warehouse


## <a name="xgboost"></a>XGBoost

[XGBoost](https://xgboost.readthedocs.org/en/latest/) on työkalu, jonka avulla nopeasti ja tarkasti tehosti puun käyttöönotto.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost myös osallistua python tai komentoriviltä.

## <a name="python"></a>Python

Käyttämällä Python kehittämiseen Anaconda Python jaot 2.7 ja 3.5 on asennettu DSVM. 

>[AZURE.NOTE] Anaconda jakauman sisältää [Condas](http://conda.pydata.org/docs/index.html), joka voidaan luoda mukautettuja ympäristöissä Python, joissa on eri versioita ja/tai asentaa ne paketit.

Oletetaan, että osa spambase tietojoukko lukea ja luokita sähköpostit tuki vector koneet-scikit kanssa-tietoja:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Voit tehdä ennusteiden seuraavasti:

    clf.predict(X.ix[0:20, :])

Julkaiseminen AzureML päätepisteen japanin varmistamalla, yksinkertaisempi mallin kolme muuttujaa on samoin kun on aiemmin julkaistu R-malli. 

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Jos haluat julkaista mallin AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

>[AZURE.NOTE] Tämä on käytettävissä python 2.7 vain ja ei vielä tueta 3.5. Suorita **/anaconda/bin/python2.7**.


## <a name="jupyterhub"></a>Jupyterhub

DSVM Anaconda-jakauman sisältyy Jupyter muistikirjan, Office kaikissa ympäristöissä ympäristössä, jossa voit jakaa Python, R tai Julia koodi ja analyysi. Jupyter muistikirjan käytetään JupyterHub kautta. Kirjaudut sisään käyttämällä paikallisen Linux-käyttäjänimen ja salasanan ***https://\<AM DNS-nimen tai IP-osoite\>: 8000 /***. Kaikki JupyterHub määritysten tiedostot löytyvät directory **/etc/jupyterhub**.

Esimerkki useita muistikirjoja on jo asennettu AM:

- Katso [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) otoksen Python muistikirjan.
- Katso [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) otoksen **T** -muistikirja.
- Katso [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) toinen esimerkki **Python** muistikirjan.

>[AZURE.NOTE] Julia kieli on saatavilla myös Linux tietojen tiede AM-komentoriviltä.


## <a name="rattle"></a>Rattle

[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (R Analytical työkalua, katso helposti) on mahdollisista graafinen R-työkalu. Siinä on intuitiivisen käyttöliittymän, joka on helppo lataamisen, Selaa, ja muuntaa tiedot ja luoda ja arvioi mallit.  Artikkelin [Rattle: A tietojen kaivos Graafisen r](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) tarjoaa vaiheittainen opas, joka esittelee sen ominaisuuksia.

Asenna ja Rattle aloittaminen seuraavia komentoja:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

>[AZURE.NOTE] Asennus ei edellytetä DSVM. Mutta Rattle voi kehottaa asentamaan muita pakettien lataamisen yhteydessä.

Rattle käyttää välilehti-pohjaisen käyttöliittymän. Useimmat välilehtien vastaavat vaiheet [Tietojen tiede prosessi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), kuten tietojen lataaminen tai sitä tutustuminen. Tietoja tiede prosessin kirjoitetaan vasemmalta oikealle välilehdissä. Mutta viimeisen-välilehti sisältää R-komentoja, suorita Rattle lokiin. 


Voit ladata ja määrittää tietojoukko:

- Jos haluat ladata tiedoston, valitse **tiedot** -välilehden sitten 
- Valitse **tiedostonimen** vieressä valitsin ja valitse **spambaseHeaders.data**. 
- Lataa tiedosto. Valitse **Suorita** ylärivin painikkeista. Raportissa pitäisi näkyä yhteenveto kunkin sarakkeen, mukaan lukien tunnistettujen tietotyyppiä, onko syöte, kohde tai muu muuttujan ja yksilöllisten arvojen määrän.
- Rattle on oikein tunnisti **Roskaposti** -sarakkeen kohde. Valitse Roskaposti-sarake ja valitse Määritä **Kohdetietotyyppiä** **Categoric**.

Voit tarkastella tietoja: 

- Valitse **Selaa** -välilehti. 
- Valitse **Yhteenveto**ja valitse **Suorita**, voit tarkastella tietoja muuttujan tyypit ja joitakin yhteenveto tilastoja. 
- Muuntyyppisten muuttujaa tilastotietoja, valitsemalla muita asetuksia, kuten **käsittelevistä** tai **perusteet**.

**Selaa** -välilehden avulla voit luoda useita kyetäkseen joudutaan. Jos haluat piirtää histogrammin tiedot:


- Valitse **jaot**.
- Tarkista **histogrammin** **word_freq_remove** ja **word_freq_you**.
- Valitse **Suorita**. Raportissa pitäisi näkyä sekä tiheyden joudutaan yhden kaavion ikkunassa, jossa on selvää, että sana "Voit" tulee paljon useammin kuin "Poista" sähköpostitse.

Korrelaatio-joudutaan ovat myös kiinnostavat. Voit luoda yhden seuraavasti:


- Valitse sitten **korrelaatio** **tyyppi** 
- Valitse **Suorita**. 
- Rattle ilmoittaa, että se suosittelee enimmäismäärä on 40 muuttujat. Valitse **Kyllä** piirron tarkastelemiseen. 

On joitakin kiinnostavat korrelaatioita esiin tulevat tehtävät: "tekniikka" on erittäin vaikutus "HP" ja "harjoituksia", kuten. Se on myös erittäin vaikutus "650", koska tietojoukko tutkimus suuntanumero on 650.

Sanojen korrelaatioita numeeriset arvot ovat Resurssienhallinta-ikkunassa. On kiinnostavat Huomautus, esimerkiksi, että "tekniikka" heikentää Korreloidun kanssa "että" ja "rahaa".

Rattle voivat muuttaa tietojoukon käsitellään joitakin yleisiä ongelmia. Esimerkiksi sen avulla voit Skaalaa uudelleen ominaisuuksia, hankintapäivän sijaan puuttuvat arvot, käsitellä harha ja poistaa muuttujat tai huomioita puuttuu tiedoilla. Rattle voit myös määrittää liitoksen säännöt huomioita ja/tai muuttujat välillä. Nämä välilehdet ovat johdanto tätä vaiheittaista alueen ulkopuolella.

Rattle voi tehdä myös klusterin analyysi. Oletetaan, että haluat jättää joitakin ominaisuuksia, jotta tulos on helpompi lukea. Valitse **tiedot** -välilehden vieressä kymmenen kohteita paitsi muuttujat **Ohita** :

- word_freq_hp
- word_freq_technology
- word_freq_george
- word_freq_remove
- word_freq_your
- word_freq_dollar
- word_freq_money
- capital_run_length_longest
- word_freq_business
- roskapostin

Siirry takaisin **klusterin** -välilehdessä Valitse **KMeans**ja määritä *klustereiden numero* 4. Valitse **Suorita**. Tulokset näkyvät kohde-ikkunassa. Yksi klusterin on suuri taajuus "george" ja "hp" ja on todennäköisesti oikeutetut yrityksen sähköpostiosoite.

Voit luoda yksinkertaisen päätös puun koneen learning mallin: 

- Valitse **malli** -välilehti 
- Valitse **puunäkymän** **tyyppi**. 
- Valitse **Suorita** näyttämään puun tekstimuodossa kohde-ikkunassa. 
- Näytä graafinen versio valitsemalla **Piirrä** . Tämä näyttää aivan kuin on saatu käyttämällä aiemmin *rpart*puun.

Jokin Rattle nice toimintoja ei voi suorittaa joitakin tietokoneen learning tapoja ja vertaamaan mittayksikön. Näin kuvatulla tavalla:

- Valitse **kaikki** **tyyppi**. 
- Valitse **Suorita**. 
- Sen jälkeen, kun se päättyy voit minkä tahansa yksittäisen **tyyppi**, kuten **SVM**, ja Tarkastele tuloksia. 
- Voit myös vertailla määrittäminen käyttämällä **Laske** -välilehden vahvistus malleissa suorituskykyä. Esimerkiksi **Virhe matriisi** -valinta näkyy sekaannusta matriisi, Yleinen virhe ja pisteiden luokan virhe kunkin mallin määrittäminen kelpoisuustarkistuksen. 
- Voit myös piirtää OHJETIEDOSTO kaaria, suorittaa herkkyysanalyysia ja tee muuntyyppisten mallin arvioinnit.

Kun olet valmis luoda malleja, valitse Tuo R-koodi, suorita Rattle istunnon aikana **Log** -välilehti. Voit valita tallennus **Vie** -painiketta. 

>[AZURE.NOTE] Rattle nykyisessä versiossa on ohjelmavirhe. Muokkaa komentosarjaa tai käyttää sitä toistamalla vaiheet sen myöhemmin, sinun on lisättävä #-merkki eteen *lokitiedostoon... vieminen* lokin teksti. 


## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL

DSVM sisältyy PostgreSQL asennettuna. PostgreSQL on kehittyneitä, Avaa lähde relaatiotietokannasta. Tässä osassa esitellään Lataa Microsoftin roskapostin tietojoukko PostgreSQL ja tehdä kyselyjä.

Ennen kuin voit ladata tietoja, sinun on sallittava salasanan todennus-localhost. Komentokehotteeseen:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Määritystiedoston alareunassa on useita rivejä, jotka sallittujen yhteyksien tiedot:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Voit käyttää md5 sijaan ident, niin että Kirjaudu sisään käyttäjänimellä ja salasanalla Vaihda "Paikallisen IPv4-yhteyksien":

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Ja postgres-palvelun käynnistäminen uudelleen:

    sudo systemctl restart postgresql

Käynnistää psql vuorovaikutteinen-päätteen PostgreSQL valmiin postgres käyttäjänä varten suorittamalla seuraavan komennon kehote:

    sudo -u postgres psql

Luo uusi käyttäjätili-käyttämällä samaa käyttäjänimeä Linux-tiliksi kuin olet kirjautuneena ja anna sille salasanan:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Valitse Kirjaudu sisään psql kuin käyttäjän:

    psql

Ja tuoda tiedot uuteen tietokantaan:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Seuraavaksi tarkastellaan tiedot ja suorita joitakin kyselyjen **Squirrel SQL**-graafinen työkalu, jolla voit käsitellä tietokantojen JDBC ohjaimen kautta.

Aloita Käynnistä Squirrel SQL-sovellukset-valikossa. Voit määrittää ohjaimen seuraavasti:

- Valitse **Windows**, valitse **Näytä-ohjaimet**. 
- Napsauta **PostgreSQL** hiiren kakkospainikkeella ja valitse **Muokkaa-ohjain**. 
- Valitse **ylimääräisiä luokan polku**ja sitten **Lisää**. 
- Kirjoita ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** **Tiedostonimi** ja 
- Valitse **Avaa**.
- Luettelon ohjaimet, valitse sitten Valitse **org.postgresql.Driver** **Luokkanimi**ja valitse **OK**.

Voit määrittää paikallisen palvelimen yhteyttä:
 
- Valitse **Windows**- **tarkastella aliakset.** 
- Valitse **+** painikkeen uuden tunnuksen. 
- Anna sille nimi *roskapostin tietokannan*, valitse **PostgreSQL** - **ohjain** avattavasta.
- Määritä URL-osoite *jdbc:postgresql://localhost/spam*. 
- Kirjoita *käyttäjänimi* ja *salasana*. 
- Valitse **OK**. 
- Avaa **yhteyden** -ikkuna kaksoisnapsauttamalla ***roskapostin tietokanta*** -alias. 
- Valitse **Muodosta yhteys**.

Voit suorittaa joitakin kyselyitä seuraavasti:

- Valitse **SQL** -välilehti.
- Kirjoita yksinkertaisen kyselyn `SELECT * from data;` yläreunaan SQL-välilehden kysely-tekstiruutuun. 
- Paina **Ctrl + Enter** suorittaa. Oletusarvoisesti Squirrel SQL palauttaa kyselyn ensimmäiset 100 riviä. 

On useita kyselyitä voit suorittaa kannattaa tutustua tiedoista. Esimerkiksi kuinka taajuus word, *Tee* eroavat toisistaan roskapostia ja pekoni?

    SELECT avg(word_freq_make), spam from data group by spam;

Tai mitä sähköpostin ominaisuudet, jotka sisältävät usein *3d*?

    SELECT * from data order by word_freq_3d desc;

Useimmat sähköposteja, joissa on hyvin esiintymän *3d* ovat mahdollisia roskapostin, jotta se voi olla hyödyllinen toiminto etsimisen ennakoivan mallin luokitella sähköpostit.

Jos haluat suorittaa tietokoneen learning PostgreSQL-tietokantaan tallennettuja tietoja, kannattaa käyttää [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server Data Warehouse

Azure SQL-tietovarasto on voi käsittelyn valtaviin tietomäärien, Relaatio- ja relaatio tietokanta pilvipohjainen, asteikko-kohtaa. Lisätietoja on artikkelissa [Azure SQL-tietovarasto ominaisuudet?](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Muodosta yhteys tietovarasto ja Luo taulukko, suorita seuraava komento komentoriviltä:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Valitse kehotteessa sqlcmd:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Kopioi tiedot ja bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

>[AZURE.NOTE] Ladatun tiedoston rivi-päätteet ovat Windows-tyylin, mutta bcp odottaa UNIX-tyyliä, jotta onko annettava bcp, - r merkin.

Ja kysely, joka sisältää sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

Voi myös kyselyn, jossa Squirrel SQL. Noudata vastaavat vaiheet PostgreSQL-käyttämällä Microsoft MSSQL palvelimen JDBC ohjainta, jotka löytyvät ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***osalta.

## <a name="next-steps"></a>Seuraavat vaiheet

Katso yleiskuvaus aiheisiin, joissa opastusta tehtävät, jotka muodostavat Azure tietojen tiede-prosessi- [Ryhmän tietojen tiede prosessi](http://aka.ms/datascienceprocess).

Muiden lopusta loppuun-vaihe vaiheelta, jotka osoittavat tietyissä skenaarioissa ryhmän tietojen tiede-vaiheet kuvaus-kohdassa [ryhmän tietojen tiede prosessin vaihe vaiheelta](data-science-process-walkthroughs.md). Vaiheittaiset ohjeet osoittavat myös voit yhdistää cloud ja paikallisen työkalujen ja palvelujen työnkulun tai myyntijakso älykkäät-sovelluksen luominen.

