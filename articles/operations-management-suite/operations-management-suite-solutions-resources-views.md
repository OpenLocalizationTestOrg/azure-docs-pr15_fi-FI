<properties
   pageTitle="Näkymien toimintojen hallinta Suite (OMS) ratkaisujen | Microsoft Azure"
   description="Ratkaisujen toimintojen hallinta Suite (OMS) sisältää yleensä tietojen visualisoimiseen yksi tai useita näkymiä.  Tässä artikkelissa käsitellään näkymän suunnittelun luoma näkymän vieminen ja Sisällytä se management-ratkaisun. "
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Näkymien toimintojen hallinta Suite (OMS) ratkaisujen (ennakkoversio)

>[AZURE.NOTE]Tämä on alustava luomiseen ratkaisujen OMS, jotka ovat tällä hetkellä esikatselu. Minkä tahansa rakenteen jäljempänä sitä saatetaan muuttaa.    

[Ratkaisujen toimintojen hallinta Suite (OMS)](operations-management-suite-solutions.md) sisältää yleensä tietojen visualisoimiseen yksi tai useita näkymiä.  Tässä artikkelissa käsitellään [Näkymän suunnittelu](../log-analytics/log-analytics-view-designer.md) luoma näkymän vieminen ja Sisällytä se management-ratkaisun.  

>[AZURE.NOTE]Tämän artikkelin esimerkkejä käyttää parametrit ja muuttujat, jotka ovat tarvittavat tai yhteiset ratkaisujen ja kuvattu [luominen ratkaisujen toimintojen hallinta Suite (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että olet jo tuttuja siitä, miten voit [luoda management-ratkaisun](operations-management-suite-solutions-creating.md) ja ratkaisutiedoston rakenne.


## <a name="overview"></a>Yleiskatsaus

Sisällytettävät management-ratkaisun näkymän luot **resurssin** sen [ratkaisutiedosto](operations-management-suite-solutions-creating.md).  JSON, joka kuvaa näkymän määrityksistä on yleensä monimutkainen vaikka ja ei julkaiseminen tyypilliset ratkaisu tekijä olisi voivat luoda manuaalisesti.  [Näkymän suunnittelu](../log-analytics/log-analytics-view-designer.md)näkymän luominen, viedä tiedot ja lisää sen määrityksistä ratkaisu on yleisin. 

Näkymän lisääminen ratkaista perusvaiheet ovat seuraavat:  Jokaisen vaiheen kuvataan tarkemmin seuraavissa osissa tarkemmin.

1. Vie näkymän tiedostoon.
2. Luo näkymän resurssin ratkaisu.
3. Lisää tietojen tarkasteleminen.

## <a name="export-the-view-to-a-file"></a>Näkymän vieminen tiedostoon
Noudattamalla ohjeita osoitteessa [Log Analytics näkymän suunnittelu](../log-analytics/log-analytics-view-designer.md) näkymän vieminen tiedostoon.  Vietävän tiedoston on samat [osat ratkaisu-tiedostona](operations-management-suite-solutions-creating.md#management-solution-files)JSON-muodossa.  

Näytä tiedoston **resurssit** -elementti on resurssin **Microsoft.OperationalInsights/workspaces** , joka edustaa OMS työtilan tyyppi.  Tämä elementti on alielementti **näkymät** , joka edustaa näkymä ja sisältää sen määrityksistä sisältötyyppi.  Voit kopioida tämän elementin tiedot ja kopioimalla sen sitten ratkaisu.


## <a name="create-the-view-resource-in-the-solution"></a>Näytä resurssin luominen ratkaisu
Lisää seuraavat Näytä resurssin ratkaisun tiedoston **resurssit** -elementti.  Tämä käyttää muuttujat, jotka on kuvattu, sinun on myös lisättävä.  Huomaa, että **raporttinäkymät-ikkunan** ja **OverviewTile** -ominaisuudet ovat paikkamerkkejä, jotka vastaavat ominaisuuksien viedyn näkymän tiedostosta korvaa.
 
    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile": 
        }
    }

Lisää seuraavat muuttujat [muuttujat](operations-management-suite-solutions-creating.md#variables) osan ratkaisu-tiedoston ja korvaa arvot, jotka ovat ratkaisu.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Huomaa, että koko näkymän resurssi voi kopioiminen viedyn näkymän-tiedostosta, mutta voit tehdä seuraavat muutokset sen toimimaan ratkaisu on.  

- Näytä resurssin **laji** on vaihdettava **Microsoft.OperationalInsights/workspaces** **näkymistä** .
- Näytä resurssin **nimi** -ominaisuus on vaihdettava työtilan nimi.
- Työtilan jäsenyys on poistettu, koska työtilan resurssin ei ole määritetty ratkaisun.
- **Näyttönimi** -ominaisuus on lisätty näkymään.  Kaikki **tunnus**, **nimi**ja **Näyttönimi** on vastattava.
- Parametrinimet on muutettava vastattava tarvittavat parametrit.
- Muuttujat on määritettävä ratkaisun ja käyttää haluamasi ominaisuudet.

## <a name="add-the-view-details"></a>Lisää tietojen tarkasteleminen
Näytä resurssin viedyn näkymän-tiedosto sisältää **Ominaisuudet** -osa **raporttinäkymät-ikkunan** ja **OverviewTile** kahden elementin joka sisältää yksityiskohtaiset määritysten näkymän.  Kopioi nämä kaksi osat ja niiden sisällön näkymän resurssin ratkaisun tiedoston **Ominaisuudet** -osan. 

## <a name="example"></a>Esimerkki
Seuraavassa esimerkissä näkyy esimerkiksi yksinkertaisen ratkaisutiedosto, jonka näkymän.  Kolme pistettä (...) näkyvät **raporttinäkymät-ikkunan** ja **OverviewTile** sisällön tilaa syistä.


    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
        ]
    }




## <a name="next-steps"></a>Seuraavat vaiheet

- Katso kaikki tiedot [ratkaisujen](operations-management-suite-solutions-creating.md)luominen.
- Sisällytä [automaatio runbooks management-ratkaisun](operations-management-suite-solutions-resources-automation.md).