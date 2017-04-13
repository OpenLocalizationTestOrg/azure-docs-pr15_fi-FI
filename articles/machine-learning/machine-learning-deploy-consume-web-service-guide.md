<properties
    pageTitle="Azure koneen Learning Verkkopalvelut: Käyttöönotto- ja kulutus | Microsoft Azure"
    description="Ottaa käyttöön ja muissa verkkopalvelut resursseja."
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="raymondl"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-donglo"/>

# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure koneen Learning Verkkopalvelut: Käyttöönotto- ja kulutus

Azure Konepohjaisten Oppimistekniikoiden avulla voit koneen opiskelun työnkulut ja mallien web-palveluiden käyttöönotto. Nämä verkkopalvelut voidaan Soita koneen opiskelun mallien sovelluksista, voit tehdä ennusteiden reaaliajassa tai erä tilassa Internetin välityksellä. Koska WWW-palvelut ovat RESTful, voit soittaa niitä eri ohjelmoinnin kielet ja ympäristöjen, kuten .NET ja Java- ja sovellusten, kuten Excel.

Seuraavien osien linkeissä vaihe vaiheelta, koodi ja ohjeiden avulla pääset alkuun.

## <a name="deploy-a-web-service"></a>Ottaa käyttöön web-palvelu

### <a name="with-azure-machine-learning-studio"></a>Azure koneen Learning Studiossa

Koneen Learning Studio ja Microsoft Azure koneen Learning Web Services-portaalin avulla voit ottaa käyttöön ja hallita verkkopalvelun kirjoittamatta koodia.

Seuraavissa linkeissä on yleisiä tietoja ottamisesta käyttöön uusi web-palvelu:

* Yleistietoa ottamisesta käyttöön uuden verkkopalveluun, joka perustuu Azure resurssien hallinta-kohdassa [Ota uusi web-palvelu](machine-learning-webservice-deploy-a-web-service.md).
* Saat WWW-palvelun ottamisesta vaiheittainen [käyttöönotto Azure koneen Learning web-palveluun](machine-learning-publish-a-machine-learning-web-service.md).
* Katso täysi vaiheittainen kuvaus siitä, miten voit luoda ja ottaa käyttöön verkkopalvelun [ongelmatilanteita vaihe 1: tietokoneen Learning työtilan luominen](machine-learning-walkthrough-1-create-ml-workspace.md).
* Tietyn esimerkkejä, jotka verkkopalvelun käyttöönotto-kohdassa:

    * [Vaiheittainen kuvaus vaihe 5: Käyttöönotto Azure koneen Learning web-palvelu](machine-learning-walkthrough-5-publish-web-service.md)
    * [WWW-palvelun ottamisesta käyttöön useita alueita](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web services resurssin palvelussa API (Azure Resurssienhallinta API)

Verkkopalvelut Azure koneen Learning resurssi-palvelu mahdollistaa verkkosivuston käyttöönotto- ja WWW-palveluista REST API-kutsujen avulla. Lisätietoja on MSDN-sivuston [Koneen Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) viittaus kohdassa.

### <a name="with-powershell-cmdlets"></a>Suorittamalla PowerShell cmdlet-komennot

Web-palveluiden Azure koneen Learning resurssin palvelu mahdollistaa verkkosivuston käyttöönotto- ja WWW-palveluista käyttämällä PowerShell cmdlet-komennot.

Voit käyttää Cmdlet-komentoja, voit on kirjautumalla ensin sisään Azure tiliisi PowerShell-ympäristöön [Lisää AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-komennolla. Jos et tiedä, kuinka voit soittaa PowerShell-komennot, jotka perustuvat Resurssienhallinta-artikkelissa [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md#login-to-your-azure-account).

Voit viedä ennakoivan kokeen käyttämällä [otoksen koodi](https://github.com/ritwik20/AzureML-WebServices). Kun luot koodin .exe-tiedoston, voit kirjoittaa:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Sovelluksen Luo web-palvelu JSON-mallin. Mallin avulla voit ottaa käyttöön verkkopalvelun, sinun on lisättävä seuraavat tiedot:

* Tallennustilan tilin nimi ja avaimen avulla

    Saat tallennustilan tilin nimi ja avaimen [Azure portal](https://portal.azure.com/) tai [Azure perinteinen portal](http://manage.windowsazure.com/).
* Sitoumus suunnitelman tunnus

    Saat suunnitelman tunnus [Azure koneen Learning Web Services](https://services.azureml.net) -portaalista kirjautua sisään ja valitsemalla tilauksen nimen.

Lisää ne JSON-malliin lasten *Ominaisuudet* solmun *MachineLearningWorkspace* solmu samalla tasolla.

Tässä on esimerkki:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

On seuraavissa artikkeleissa ja lisätiedot otoksen koodi:

* MSDN-sivuston viittaus [Azure koneen Learning cmdlet-komennot]( https://msdn.microsoft.com/library/azure/mt767952.aspx)
* Valitse GitHub otoksen [vaiheittainen kuvaus](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt)

## <a name="consume-the-web-services"></a>Käyttää Internet-palveluita

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Azure koneen Learning Web Services Käyttöliittymä (testaaminen)

Voit testata web-palvelu Azure koneen Learning Web Services-portaalista. Tämä vaihtoehto sisältää testaaminen vastaus palvelun (RRS) ja liittymät eräkäsittely service (BES).

* [Uusi web-palvelu käyttöönotto](machine-learning-webservice-deploy-a-web-service.md)
* [Ottaa käyttöön Azure koneen Learning web-palvelu](machine-learning-publish-a-machine-learning-web-service.md)
* [Vaiheittainen kuvaus vaihe 5: Käyttöönotto Azure koneen Learning web-palvelu](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Excelistä

Voit ladata Excel-malli, joka käyttää web-palvelu:

* [Muissa Azure koneen Learning WWW-palvelun Excelistä](machine-learning-consuming-from-excel.md)
* [Excel-apuohjelma Azure koneen Learning verkkopalvelut](machine-learning-excel-add-in-for-web-services.md)


### <a name="from-a-rest-based-client"></a>REST-asiakasohjelmassa

Azure koneen Learning Web Services ovat RESTful API. Voit käyttää näitä API-eri ympäristöissä, kuten .NET, Python, R, Java. WWW-palvelun [Microsoft Azure koneen Learning Web Services portal](https://services.azureml.net) **Kulutettava** -sivulla on esimerkki koodi, joiden avulla pääset alkuun. Jos haluat lisätietoja, katso, [miten tarjoaman web Azure koneen Learning-palvelu, joka on otettu käyttöön Konepohjaisten Oppimistekniikoiden kokeen](machine-learning-consume-web-services.md).

