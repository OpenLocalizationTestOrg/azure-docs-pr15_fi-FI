<properties
    pageTitle="Vaihe 2: Lataa tietojen tuominen koneen Learning kokeen | Microsoft Azure"
    description="Vaihe 2 / kehittäminen vaiheittainen ennakoivan ratkaisu: lataa tallennettu julkisten tietojen tuominen Azure koneen Learning Studio."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016" 
    ms.author="garye"/>


# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Vaiheittainen kuvaus vaihe 2: Ladata olemassa olevia tietoja Azure koneen Learning kokeen esittely

Tämä on toinen vaihe vaiheittaista [kehittäminen ennakoivan analytics-ratkaisun Azure koneen oppiminen](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Koneen Learning työtilan luominen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  **Ladata olemassa olevia tietoja**
3.  [Luo uusi koe](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Kouluttaminen ja arvioi mallit](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Access web-palvelu](machine-learning-walkthrough-6-access-web-service.md)

----------

Kehittää ennakoivan mallin luottotietojen riskin annettava tiedot, jotka Käytämme kouluttaminen ja testaa sitten malli. Näiden vaiheiden käytetään "UCI Statlog (Saksa luottokortin tietoja) tietojoukon" UCI Machine Learning säilöstä. Löydät sen seuraavassa:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ics.uci.edu/ml/DataSets/Statlog+(German+Credit+Data)</a>

Käytetään tiedosto nimeltä **german.data**. Lataa tämä tiedosto paikalliselle kiintolevylle.  

Tämä tietojoukko on 20 muuttujat rivien 1000 aiemman hakijan luottoa varten. 20 muuttujia edustavat tietojoukko ominaisuus vektorista, joka tarjoaa kunkin luottotietojen hakijan tunnisteen ominaisuudet. Toisen sarakkeen kunkin rivin edustaa hakijan laskettu luottotietojen riskin tunnisti pienen luottotietojen riskin ja 300 suuri riski 700 hakijan kanssa.

UCI-sivustossa on kuvaus ominaisuus vektorin attribuuteista, jotka sisältävät taloudellisia tietoja, luottokortin historia, tila ja henkilökohtaiset tiedot. Kunkin hakijan binaarinen luokitus on ollut annetun osoittaa, ovatko ne pieni tai suuri luottokortti riski.  

Olemme kouluttaminen ennakoivan analytics-mallin näitä tietoja käytetään. Kun Microsoft on valmis, tutustu mallin pitäisi hyväksyä tiedot uuteen henkilöille ja ennustaa, ovatko ne pieni tai suuri luottotietojen riskin.  

Seuraavassa on yksi kiinnostavat kierre. Tietojoukon kuvaus selitetään, misclassifying henkilön pienen luottotietojen riski, kun ne ovat itse asiassa hyvin luottotietojen riskin on taloudellisia oppilaitos Lisää 5 luvulla kallista kuin misclassifying sellaisena kuin se on hyvin pieni luottotietojen riskin. Yksi helppo tapa Tämä ottaa huomioon Microsoftin kokeessa on kopioimalla (5 kertaa) merkintöjä, jotka edustavat henkilö, jolla on suuri luottotietojen riskin. Valitse mallin misclassifies hyvin luottotietojen riskin mahdollisimman alhainen, jos se toimi kyseisen luokitteluvirhe 5 luvulla kerran kukin kopio. Tämä lisää koulutusta tulokset tämän virheen kustannukset.  

##<a name="convert-the-dataset-format"></a>Muuntaa tietojoukko-muoto
Alkuperäinen tietojoukko käyttää tyhjä erotettu muotoa. Koneen Learning Studio toimii paremmin CSV (CSV)-tiedoston, joten emme muuntaa dataset korvaamalla pilkuilla välilyöntejä.  

Voit muuntaa tiedoista monella tavalla. Yksi tapa on Windows PowerShell-komennon avulla:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

Toinen tapa on Unix sed-komennon avulla:  

    sed 's/ /,/g' german.data > german.csv  

Kummassakin tapauksessa emme luonut CSV-tiedosto nimeltä **german.csv** , tutustu kokeen käytetään tietojen versio.

##<a name="upload-the-dataset-to-machine-learning-studio"></a>Tietojoukon lataaminen tietokoneeseen Learning Studio

Kun tiedot on muunnettu CSV-muoto, annettava ladata sen koneen Learning Studio kyselyjä. Saat lisätietoja tietokoneen Learning Studio käytön aloittaminen [Microsoft Azure koneen Learning Studio aloitus](https://studio.azureml.net/).

1.  Avaa Konepohjaisten Oppimistekniikoiden Studio ([https://studio.azureml.net](https://studio.azureml.net)). Kun sinua pyydetään kirjautumaan, käytä työtilan omistajaksi määritetyn tilin.
1.  Valitse ikkunan yläosassa **Studio** -välilehti.
1.  Valitse **+ Uusi** ikkunan alareunassa.
1.  Valitse **TIETOJOUKKO**.
1.  Valitse **FROM paikallinen tiedosto**.
1.  **Lataa uusi tietojoukko** -valintaikkunassa valitsemalla **Selaa** ja Etsi luomasi **german.csv** -tiedosto.
1.  Kirjoita tietojoukon nimi. Näiden vaiheiden olemme kutsu sitä "UCI Saksa luottokortin tiedot".
1.  Valitse tietotyyppi- **Yleinen CSV-tiedoston, jossa ei ole otsikkoa (. nh.csv)**.
1.  Lisää halutessasi kuvaus.
1.  Valitse **OK**.  

![Lataa tietojoukko][1]  


Tämä lataa tiedot tietojoukko moduuliin, Käytämme kokeessa.

> [AZURE.TIP] Voit hallita itse lisättyjen Studio tietojoukkoja valitsemalla **TIETOJOUKKOJA** -välilehden vasemmalla puolella Studio-ikkuna.

Saat lisätietoja erityyppisten tietojen tuomisesta kokeen [koulutus tietojen tuominen Azure koneen Learning Studio](machine-learning-data-science-import-data.md).

**Seuraavaksi: [Luo uusi koe](machine-learning-walkthrough-3-create-new-experiment.md)**

[1]: ./media/machine-learning-walkthrough-2-upload-data/upload1.png
