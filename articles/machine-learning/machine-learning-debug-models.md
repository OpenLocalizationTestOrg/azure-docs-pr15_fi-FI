<properties 
    pageTitle="Virheenkorjaus-Azure koneen Learning mallin | Microsoft Azure" 
    description="Tässä artikkelissa kerrotaan, miten voit korjata Azure koneen Learning-mallin." 
    services="machine-learning"
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/09/2016" 
    ms.author="bradsev;garye" />

# <a name="debug-your-model-in-azure-machine-learning"></a>Virheenkorjaus Azure koneen Learning-malli

Tässä artikkelissa kerrotaan, miten korjata Microsoft Azure koneen Learning-mallit. Tarkemmin sanottuna käsitellään mahdollisia syitä, miksi joko kaksi virheen tilanteista saattaa ilmetä suoritettaessa mallin:

* [Junassa mallin] [ train-model] moduulin ilmoittaa virheestä 
* [Tulos mallin] [ score-model] moduulin tuottaa vääriä tuloksia 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-throws-an-error"></a>Kouluttaminen mallin moduulin aiheuttaa virheen

![image1](./media/machine-learning-debug-models/train_model-1.png)

[Junassa mallin] [ train-model] moduulin odottaa seuraavat 2 syötteiden:

1. Luokitus/Regression mallin tyypin Azure koneen Learning myöntämä mallit-valikoimasta
2. Koulutus tiedot on määritetty Otsikko-sarake. Otsikko-sarakkeessa määrittää ennustaa muuttuja. Muiden sarakkeiden sisällyttää oletetaan ominaisuuksia.

Tämä moduuli ilmoittaa virheestä seuraavissa tapauksissa:

1. Otsikko-sarakkeessa on määritetty virheellisesti, koska enemmän kuin yksi sarake on valittuna otsikkona tai virheellinen sarakkeen indeksi on valittuna. Toisessa tapauksessa sovelletaan esimerkiksi jos syötteen tietojoukko, joka oli vain 25 sarakkeiden kanssa käytettävä sarakeindeksi on 30.

2. Dataset ei sisällä ominaisuus sarakkeita. Esimerkiksi jos syötteen tietojoukko on vain 1 sarake, joka on merkitty otsikko-sarakkeessa, ei olisi ei ominaisuuksia, johon voit luoda mallin. Tässä tapauksessa [Junassa mallin] [ train-model] moduuli palauttaa virheen.

3. Kirjoita tietojoukon (ominaisuuksia tai otsikko) sisältää ääretön arvona.


## <a name="score-model-module-does-not-produce-correct-results"></a>Tulos mallin moduuli ei tuota oikeita tuloksia

![image2](./media/machine-learning-debug-models/train_test-2.png)

Tyypillinen koulutus/testaaminen Graphissa valvottava Koulujen [Jaetut tiedot] [ split] moduulin jakaa alkuperäisen tietojoukko kaksiosainen: osa, jota käytetään kouluttaminen mallin ja osa, joka on varattu pisteet, miten hyvin koulutetun mallin suorittaa tietojen se ei kouluttaminen käyttöön. Koulutetun mallin käytetään sitten sija testitiedot, minkä jälkeen tulokset arvioidaan määrittämään mallin tarkkuudella.

[Tulos mallin] [ score-model] moduulin edellyttää kahta syötettä:

1. [Junassa mallin] koulutetun mallin tulosteen[ train-model] moduuli
2. Tulosmalli tietojoukko ei, malli ei ollut koulutus-

Se voi ilmetä, vaikka kokeen onnistuu, [Pisteet mallin] [ score-model] moduulin tuottaa vääriä tuloksia. Useita skenaarioita saattaa aiheuttaa tätä:

1. Jos määritetty nimi on categorical ja regression malli on koulutus tietoihin, virheellinen tulos yhdistämällä luotuja [Pistemäärän mallin] [ score-model] moduuli. Tämä johtuu siitä regressio edellyttää jatkuva vastauksen muuttuja. Tässä tapauksessa on oltava Lisää sopiva käyttämään luokitus-malli. 
2. Vastaavasti jos luokitus-malli on koulutus-tietojoukko ottaa irrallinen lukujen otsikko-sarakkeessa, se voi tuottaa ei-toivottujen tuloksia. Tämä johtuu siitä luokitus edellyttää erillinen vastauksen muuttuja, joka sallii arvot vain alueen luokat rajallinen ja yleensä hieman pieni joukko päälle.
3. Jos tulosmalli tietojoukko ei sisällä kaikkia ominaisuuksia, jotka on käytettävä kouluttaminen-malli, [Pisteet mallin] [ score-model] tuottaa virheen.
4. [Tulos mallin] [ score-model] tuota tulosmalli tietojoukko, joka sisältää arvo puuttuu tai äärettömän arvo sen ominaisuuksien riviä vastaavan tuloksen.
5. [Tulos mallin] [ score-model] voi tuottaa samat tulostaa kaikkien rivien tulosmalli tietojoukossa. Näin voi käydä, esimerkiksi siirtymisruudussa, kun yrität perusyksikkönä päätös metsien, jos näytteiden kohti lehtisolmu vähimmäismäärä valitaan on enemmän kuin esimerkkejä käytettävissä määrä.


<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
 
