<properties
    pageTitle="Valitse parametrit optimoida oman Azure Machine Learning algoritmit | Microsoftin Azure"
    description="Kerrotaan, miten voit valita optimaalinen parametri määrittää Azure Machine Learning-algoritmia."
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
    ms.author="bradsev" />


# <a name="choose-parameters-to-optimize-your-algorithms-in-azure-machine-learning"></a>Valitse parametrit, voit optimoida oman algoritmit Azure Machine Learning

Tässä aiheessa kuvataan, miten Azure Machine Learning-algoritmi määrittää oikean hyperparameter valitseminen. Useimmat machine learning algoritmeja on määrittämällä parametrit. Kun olet malli junan, sinun on annettava näiden parametrien arvot. Koulutetut mallin tehokkuus riippuu mallin parametrit, jotka voit valita. Prosessi, jossa etsitään optimaalinen joukko parametreja käytetään nimitystä *mallin valinta*.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

On monia tapoja voidaan mallintaa valinta. Cross-vahvistus on yksi laajimmin käytetyistä tavoista mallin valinnan machine learning, ja se on oletus mallin valinta-mekanismi, Azure Machine Learning. Azure Machine Learning tukee sekä R Python, koska voit aina toteuttaa oman mallin valintamekanismit R tai Python.

Löytää paras parametrijoukko neljä vaihetta:

1.  **Määritä parametrin tilaa**: algoritmin, päätä ensin haluat tarkan parametriarvot.
2.  **Määritä rajat-vahvistuksen asetukset**: päättää, miten valita rajat vahvistusta taitosten tekeminen DataSet-ryhmän osalta.
3.  **Määritä lisätiedot**: root-keskiarvon neliön virhe, tarkkuus, peruuttaminen tai f-score, päättää, mitä mittarin käyttäminen määritettäessä paras joukko parametreja, kuten tarkkuus.
4.  **Juna-, arvioida ja verrata**: jokaisen yksilöllisen yhdistelmän parametriarvot rajat vahvistusta toteuttamia ja määrität virhe-mittarin perusteella. Arvioinnin ja vertailun jälkeen valita tehokkaimpia pisteisiin parhaiten sopivan mallin.

Seuraava kuva havainnollistaa näyttää, miten tämä voidaan saada aikaan Azure Machine Learning.

![Löytää paras parametri](./media/machine-learning-algorithm-parameters-optimize/fig1.png)

## <a name="define-the-parameter-space"></a>Parametrin tilan määrittäminen
Voit määrittää parametrin määrittäminen mallin valmistelu-vaiheessa. Parametri-ruudusta kaikki machine learning algoritmeista on kaksi kouluttaja tilaa: *Yksi parametri* ja *Arvoalueen*. Valitse parametrin alueen tila. Arvoalueen-tilassa voit syöttää useita arvoja kullekin parametrille. Pilkuilla erotetut arvot voidaan syöttää teksti-ruutuun.

![2-luokan tehosti Päätöksentekopuu, yksittäinen parametri](./media/machine-learning-algorithm-parameters-optimize/fig2.png)

 Vaihtoehtoisesti voit määrittää ruudukon ja lukumäärä viittaa **Käyttöä alueen Builder**Luo enimmäis- ja pisteitä. Oletusarvon mukaan parametriarvot muodostetaan lineaarisen asteikon. Mutta jos **Log-asteikko** on valittu, arvot muodostetaan log-asteikko (vierekkäisten pisteiden suhde on vakio eikä niiden ero). Kokonaisluku parametrit voit määrittää alueen yhdysmerkin avulla. Esimerkiksi "1-10" tarkoittaa, että kaikki kokonaisluvut 1-10 (molemmat päätepisteet mukaan lukien) muodostavat parametrien määrittäminen. Sekatila myös tuettu. Parametri määrittää esimerkiksi "1-10, 20, 50" käsittäisi arvoja ovat kokonaisluvut 1-10, 20 ja 50.

![2-luokan tehosti päätös puun arvoalueen](./media/machine-learning-algorithm-parameters-optimize/fig3.png)

## <a name="define-cross-validation-folds"></a>Määritä rajat vahvistusta taitosten tekeminen
- [Osio ja näyte] [ partition-and-sample] moduulin avulla voidaan määrittää tiedoille taitosten tekeminen satunnaisesti. Seuraavassa näyte siten, että moduuli viisi taitosten tekeminen määrittää ja liittää numeron taitto satunnaisesti näyte esiintymiä.

![Osio ja malli](./media/machine-learning-algorithm-parameters-optimize/fig4.png)


## <a name="define-the-metric"></a>Määritä mittausarvo
[Hienosäätö Model Hyperparameters] [ tune-model-hyperparameters] moduuli tukee kokeellisesti valitsemalla paras joukko parametreja tietyn algoritmin ja dataset. Muita tietoja liittyen koulutusta mallin **Ominaisuudet** -ruudun tämän moduulin sisältää mittarin paras parametrijoukko määrittämiseksi. Se on kaksi eri avattavan luettelon valintaruudut luokitus- ja regressio-algoritmeja. Jos luokitus algoritmi on algoritmi tarkasteltavana, regression metric-arvo ohitetaan ja päinvastoin. Metric-arvo on tietty tässä esimerkissä **tarkkuutta**.   

![Sweep parametrit](./media/machine-learning-algorithm-parameters-optimize/fig5.png)

## <a name="train-evaluate-and-compare"></a>Junan, arvioida ja vertailla  
Saman [Mallin Hyperparameters virittäminen] [ tune-model-hyperparameters] moduuli kaikki mallit, jotka vastaavat parametrijoukko harjoittaa arvioi eri mittareita ja luo sitten valitset mittarin mukaan koulutetut pisteisiin parhaiten sopivan mallin. Tässä moduulissa on kaksi pakolliset panokset:

* Auttamaan kouluttamattomia oppija
* DataSet-ryhmän

Moduuli on myös input valinnainen dataset. Internetin dataset taitto pakollinen dataset-syötteen tiedot. Dataset ei ole määritetty taitto mitään tietoja, jos sitten 10-fold cross-tarkistus suoritetaan automaattisesti oletusarvon mukaan. Jos valinnainen dataset-satamassa annetaan vahvistus dataset ei taitto määritys tehdään, junan testin tila on valittu ja ensimmäisen tietojoukon käytetään junan mallin parametri kullekin yhdistelmälle.

![Päätös tehosti puun valitsin](./media/machine-learning-algorithm-parameters-optimize/fig6a.png)

Mallin arvioidaan sitten vahvistus dataset. Moduuli vasemmalla tulostusportin näkyy eri mittareita funktiot on parametriarvo. Oikea lähtöportti antaa koulutetut malli, joka vastaa valitun mittausarvon (Tässä tapauksessa**tarkkuus** ) mukaan tehokkaimpia pisteisiin parhaiten sopivan mallin.  

![DataSet-ryhmän tarkistus](./media/machine-learning-algorithm-parameters-optimize/fig6b.png)

Näet oikean lähtöportti visualisoimalla valitsema tarkka parametrit. Tämä malli voidaan käyttää pisteytys sarja testi tai operationalized web-palvelun koulutetun mallina tallentamisen jälkeen.

<!-- Module References -->
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[tune-model-hyperparameters]: https://msdn.microsoft.com/library/azure/038d91b6-c2f2-42a1-9215-1f2c20ed1b40/
