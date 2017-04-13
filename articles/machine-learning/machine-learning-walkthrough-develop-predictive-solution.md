<properties
    pageTitle="Luottotietojen riskin koneen Learning ennakoivan ratkaisua | Microsoft Azure"
    description="Yksityiskohtainen ongelmatilanteita luominen Azure koneen Learning Studiossa ennakoivan analytics ratkaisua luottotietojen risk Assessment-sovelluksesta."
    keywords="luottokortti riski, ennakoivan analytics-ratkaisun ja risk Assessment-sovelluksesta"
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
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Vaiheittainen kuvaus: Kehittää luottotietojen risk Assessment-ohjelman-Azure koneen Learning ennakoivan analytics-ratkaisua

Oletetaan, että haluat ennustaa henkilön luottotietojen riskin ne Anna luottotietojen sovelluksen tietojen perusteella.  

Luottotietojen risk Assessment-sovelluksesta on monimutkaisia ongelman, mutta yksinkertaistaa seuraavaksi kysymyksen parametrit hieman. Olemme sitten käyttää sitä esimerkki siitä, miten voit käyttää Microsoft Azure koneen Learning koneen Learning Studio ja tietokoneen Learning WWW-palvelun näiden ennakoivan analytics-ratkaisun.  

Yksityiskohtainen tätä vaiheittaista noudattamalla kehittäminen ennakoivan analytics mallin koneen Learning Studiossa ja sen käyttöönottamisesta kuin Azure koneen Learning WWW-palvelun. Microsoft yleisesti saatavilla luottotietojen riskitiedot alkavat, kehittää kouluttaminen ennakoivan mallin tietojen perusteella ja ota malli käyttöön web-palvelu, jonka avulla voidaan muut luottotietojen risk Assessment-sovelluksesta.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

>[AZURE.TIP] Voit ladata ja tulostaa kaavion, jossa on yleiskuvaus koneen Learning Studio ominaisuuksia, katso [yleiskuvauskaavio Azure koneen Learning Studio ominaisuuksia](machine-learning-studio-overview-diagram.md).

Luottotietojen riskien arviointi ratkaista luomiseen on toimimalla seuraavasti:  

1.  [Koneen Learning työtilan luominen](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Ladata olemassa olevia tietoja](machine-learning-walkthrough-2-upload-data.md)
3.  [Luo uusi koe](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Kouluttaminen ja arvioi mallit](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [WWW-palvelun käyttöön](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Access web-palvelu](machine-learning-walkthrough-6-access-web-service.md)

Tätä vaiheittaista perustuu yksinkertaistettu versio [binaarinen Classfication: luottokortti riskin tekstinsyöttö](http://go.microsoft.com/fwlink/?LinkID=525270) otoksen kokeen [Cortana Liiketoimintatieto-valikoima](http://gallery.cortanaintelligence.com/).
