<properties
   pageTitle="Luominen ratkaisujen toimintojen hallinta Suite (OMS) | Microsoft Azure"
   description="Ratkaisujen laajentaa toimintoja toimintojen hallinta Suite (OMS) tarjoamalla pakattu hallinta tilanteita, joissa asiakkaat voit lisätä niiden OMS työtilaan.  Tässä artikkelissa kuvataan, miten voit luoda oman ympäristössä käytettävä ratkaisujen tai ovat käytettävissä asiakkaille."
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
   ms.date="10/27/2016"
   ms.author="bwren" />

# <a name="creating-management-solutions-in-operations-management-suite-oms-preview"></a>Ratkaisujen luominen-toimintojen hallinta Suite (OMS) (ennakkoversio)

>[AZURE.NOTE]Tämä on alustava luomiseen ratkaisujen OMS, jotka ovat tällä hetkellä esikatselu. Minkä tahansa rakenteen jäljempänä sitä saatetaan muuttaa.  

Ratkaisujen laajentaa toimintoja toimintojen hallinta Suite (OMS) antamalla pakattu hallinta tilanteita, joissa asiakkaat voit lisätä niiden OMS työtilaan.  Tässä artikkelissa on tietoja oman ratkaisujen, voit käyttää omaa ympäristössä tai saataville yhteisön kautta asiakkaat luomisesta.

## <a name="planning-your-management-solution"></a>Management-ratkaisun suunnittelu
OMS-ratkaisujen sisältää useita tukevat tietyn hallinta tilanne resursseja.  Ratkaisu suunniteltaessa kannattaa keskittyä tilanne, jossa yrität saavuttamiseksi ja kaikki tarvittavat resurssit tukee sitä.  Kunkin ratkaisu olisi voi itse sisälsi ja voit määrittää kullekin resurssille, joka edellyttää, vaikka yhden tai useamman resurssin määritellään myös muita ratkaisuja.  Kun hallintaratkaisu, joka on asennettu, kullekin resurssille luodaan, paitsi jos se on jo olemassa, ja voit määrittää, mitä tapahtuu resursseja, kun ratkaisu on poistettu.  

Esimerkiksi hallintaratkaisu, joka voi olla [Azure automaatio runbookin](../automation/automation-intro.md) , joka kerää tietoja [aikataulun](../automation/automation-schedules.md) ja [näkymän](../log-analytics/log-analytics-view-designer.md) , joka sisältää visualisoinnista kerättyjen tietojen Log Analytics säilöön.  Ratkaisu saattaa käyttää samaa aikatauluun.  Management-ratkaisun tekijä määrittää kaikkien kolmen resurssien mutta määrittää, että runbookin ja tarkastella tulee automaattisesti poistetaan, kun ratkaisu on poistettu.    Myös määrittää aikataulun mutta määrittää, että se pysyy paikoillaan Jos ratkaisu on poistaa muiden ratkaisu siltä varalta, että se on edelleen käytössä.

## <a name="management-solution-files"></a>Management-ratkaisun tiedostot
[Resurssienhallinta](../resource-manager-template-walkthrough.md)malleiksi ratkaisujen otettu käyttöön.  Tärkeimmät tehtävän opettelemalla tekijän ratkaisujen Koulujen miten [Tekijä mallia](../resource-group-authoring-templates.md).  Tässä artikkelissa on yksilöllinen tietoja käytetään ratkaisuihin ja tyypillinen ratkaisu resurssien määrittäminen.

Management-ratkaisutiedoston perusrakenteen on sama kuin [Resurssienhallinta malli](resource-group-authoring-templates.md#template-format) on seuraavasti.  Kunkin seuraavissa kohdissa kuvataan ylimmän tason elementit ja ja ratkaista niiden sisältöä.  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a>Parametrit

[Parametrit](../resource-group-authoring-templates.md#parameters) ovat arvoja, jotka tarvitset käyttäjältä management-ratkaisun asentaminen.  On vakio parametrit kaikki ratkaisut sisältävät ja voit lisätä parametreja halutulla tavalla tietyn ratkaisun.  Miten käyttäjien antaa parametriarvot asentaessaan ratkaisu riippuu tietyn parametrin ja kuinka ratkaisu asennetaan.


Kun käyttäjä asentaa management-ratkaisun [Azure Marketplacesta](operations-management-suite-solutions.md#finding-and-installing-solutions) tai [Azure pikaopas malleja](operations-management-suite-solutions.md#finding-and-installing-solutions) heitä pyydetään valitsemaan on [OMS työtilan ja automaatio-tilin](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account)kautta.  Näitä käytetään täytä vakio parametrien arvot.  Käyttäjää pyydetään ei anna suoraan vakio parametrien arvot, mutta he saavat kehotteen Anna arvot mahdolliset muut parametrit.

Kun käyttäjä on asennettu ratkaisu [muulla tavalla](operations-management-suite-solutions.md#finding-and-installing-solutions), ne on määritettävä arvo kaikki vakio parametrit ja kaikki muut parametrit.

Esimerkki-parametri on alla.

    "Daily Start Time": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

Seuraavassa taulukossa on kuvattu parametrin määritteet.

| Määrite | Kuvaus |
|:--|:--|
| tyyppi        | Parametrin tietotyyppi. Näyttää käyttäjän syöteohjausobjekti määräytyy tietotyypin.<br><br>bool - avattavassa<br>merkkijono - tekstiruutu<br>INT - tekstiruutu<br>SecureString - salasana<br> |
| luokka    | Parametrin valinnainen luokka.  Saman luokan parametrit on ryhmitelty. |
| ohjausobjektin     | Kyselyparametri lisätoimintoja.<br><br>DATETIME - Datetime-ohjausobjektin tulee näkyviin.<br>GUID-tunnus - Guid-arvon luodaan automaattisesti ja parametri ei ole näkyvissä. |
| kuvaus | Parametrin valinnainen kuvaus.  Näyttää tiedot-selitteeseen parametrin vieressä. |


### <a name="standard-parameters"></a>Vakio-parametrit
Seuraavassa taulukossa on lueteltu kaikki ratkaisujen vakio parametrit.  Nämä arvot täytetään käyttäjän sen sijaan, että kysely ne ratkaisu on asennettu Azure Marketplacesta tai pikaopas malleja.  Käyttäjän on annettava arvot niiden, jos ratkaisu on asennettu toista menetelmää.

>[AZURE.NOTE]Pikaopas mallit ja Azure Marketplacesta käyttöliittymä odottaa parametrinimet taulukossa.  Jos käytössäsi on eri parametrinimet sitten käyttäjää pyydetään niiden ja ne eivät täytetään automaattisesti.


| Parametri | Tyyppi | Kuvaus |
|:--|:--|:--|
| accountName       | merkkijono |  Azure automaatio-tilin nimi. |
| pricingTier       | merkkijono | Hinnat taso Log Analytics-työtilan ja Azure automaatio-tili. |
| regionId          | merkkijono | Alueen Azure automaatio-tili. |
| ratkaisun nimi      | merkkijono | Ratkaisun nimi. |
| workspaceName     | merkkijono | Kirjaudu Analytics työtilan nimi. |
| workspaceRegionId | merkkijono | Lokitiedoston Analytics-työtilan alue. |





### <a name="sample"></a>Esimerkki
Seuraavassa on esimerkki-parametrin kohteen ratkaisua.  Tämä sisältää kaikki vakio parametrit ja kaksi parametreja samaan luokkaan.

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
            "type": "string",
            "metadata": {
                "description": "A valid Azure Automation account name"
            }
        },
        "workspaceRegionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        },
        "jobIdGuid": {
        "type": "string",
            "metadata": {
                "description": "GUID for a runbook job",
                "control": "guid",
                "category": "Schedule"
            }
        },
        "startTime": {
            "type": "string",
            "metadata": {
                "description": "Time for starting the runbook.",
                "control": "datetime",
                "category": "Schedule"
            }
        }


Voit viitata muiden osien syntaksi **Parametrit (parametrin nimi)**-ratkaisun parametriarvot.  Esimerkiksi jos haluat käyttää työtilan nimi, vakiomittoja **parameters('workspaceName')** 

## <a name="variables"></a>Muuttujat

**Muuttujat** elementti sisältää arvot, joita käytät management-ratkaisun muille käyttäjille.  Nämä arvot eikä niitä julkaista käyttäjän ratkaisun asentamista.  Ne on tarkoitettu antaa tekijän yhteen paikkaan, jossa voit hallita arvoja, jotka voivat käyttää useita kertoja koko ratkaisu. Sijoita arvot tietyn ratkaisuun muuttujan kehystä hardcoding ne **resurssit** -osaan.  Tämä on koodin luettavuutta ja avulla voit helposti muuttaa nämä arvot uudemmissa versioissa.

Seuraavassa on esimerkki **muuttujat** osan tyypillinen parametreilla käyttää ratkaisuja.

    "variables": { 
        "SolutionVersion": "1.1", 
        "SolutionPublisher": "Contoso", 
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

Voit viitata muuttujien arvoihin ratkaisussa, jossa syntaksi **muuttujat ('muuttujan nimi")**.  Esimerkiksi jos haluat käyttää ratkaisun nimi-muuttuja, vakiomittoja **variables('solutionName')** 


## <a name="resources"></a>Resurssit

**Resurssit** -osa määrittää management-ratkaisun eri resursseja.  Tämä on mallin suuria ja monimutkaisia osa.  Resursseja on määritetty seuraavat rakenteen kanssa.  

    "resources": [
        {
            "name": "<name-of-the-resource>",           
            "apiVersion": "<api-version-of-resource>",
            "type": "<resource-provider-namespace/resource-type-name>",     
            "location": "<location-of-resource>",
            "tags": "<name-value-pairs-for-resource-tagging>",
            "comments": "<your-reference-notes>",
            "dependsOn": [
                "<array-of-related-resource-names>"
            ],
            "properties": "<unique-settings-for-the-resource>",
            "resources": [
                "<array-of-child-resources>"
            ]
        }
    ]

### <a name="dependencies"></a>Riippuvuudet
**DependsOn** elementit määrittää [riippuvuuden](../resource-group-define-dependencies.md) toiselle resurssille.  Kun ratkaisu on asennettu, resurssin ei luoda, kunnes kaikki sen tarvitsemat luonut.  Esimerkiksi ratkaisu voi [käynnistää runbookin](operations-management-suite-solutions-resources-automation.md#runbooks) on asennettu käyttämällä [projektin resurssi](operations-management-suite-solutions-resources-automation.md#automation-jobs).  Työn resurssi on riippuvainen runbookin resurssin varmistaaksesi, että: n runbookin on luotu ennen työn luodaan.

### <a name="oms-workspace-and-automation-account"></a>OMS työtilan ja automaatio-tili
Ratkaisujen edellyttävät [OMS työtilan](../log-analytics/log-analytics-manage-access.md) sisältämään näkymiä ja [automaatio-tili](../automation/automation-security-overview.md#automation-account-overview) sisältää runbooks ja siihen liittyvät resurssit.  Nämä on oltava käytettävissä, ennen kuin resursseista ratkaisun luodaan ja on määritettävä itse ratkaisussa.  Käyttäjän tulee [määrittää työtilan ja tilin](operations-management-suite-solutions.md#oms-workspace-and-automation-account) ne ottaa ratkaisuja, mutta tekijä Ota huomioon seuraavat seikat.


## <a name="solution-resource"></a>Ratkaisu resurssi
Kunkin ratkaisu edellyttää resurssi-merkinnän **resurssit** -elementin, joka määrittää ratkaisun itse.  Tämä on eräänlainen **Microsoft.OperationsManagement/solutions**.  Seuraavassa on esimerkki ratkaisu resurssin.  Seuraavissa osissa on kuvattu sen eri osat.

    "name": "[concat(variables('SolutionName'), '[ ' ,parameters('workspacename'), ' ]')]",
    "location": "[parameters('workspaceRegionId')]",
    "tags": { },
    "type": "Microsoft.OperationsManagement/solutions",
    "apiVersion": "[variables('LogAnalyticsApiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]",
        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
    ]
    "properties": {
        "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
        "referencedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/schedules/', variables('ScheduleName'))]"
        ],
        "containedResources": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('RunbookName'))]",
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
        ]
    },
    "plan": {
        "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
        "Version": "[variables('SolutionVersion')]",
        "product": "AzureSQLAnalyticSolution",
        "publisher": "[variables('SolutionPublisher')]",
        "promotionCode": ""
    }

### <a name="solution-name"></a>Ratkaisun nimi
Ratkaisu-nimen on oltava yksilöllinen Azure-tilaukseesi. Suositeltu arvon, jota käytetään on seuraava.  Tämä käyttää muuttujaan, jonka **ratkaisun nimi** pääkansion nimi ja **workspaceName** -parametrin varmistaa, että nimi on yksilöllinen.

    [concat(variables('SolutionName'), ' [' ,parameters('workspaceName'), ']')]

Tämä ratkaista nimi, kuten jompikumpi seuraavista.

    My Solution Name [MyWorkspace]
 

### <a name="dependencies"></a>Riippuvuudet
Ratkaisu resurssi on oltava [riippuvuuden](../resource-group-define-dependencies.md) jokaisen ratkaisun resurssin jälkeen, kun ne on luotu ennen Ratkaisun voidaan luoda.  Voit tehdä tämän lisäämällä merkinnän kullekin resurssille **dependsOn** -osaan.


### <a name="properties"></a>Ominaisuudet:
Ratkaisu resurssi on ominaisuudet seuraavan taulukon.  Tämä sisältää resurssien Viitattu ja sisältämien ratkaisu, joka määrittää hallintatavat resurssin ratkaisun asentamisen jälkeen.  Kullekin resurssille ratkaisun luettelossa pitäisi näkyä **referencedResources** tai **containedResources** -ominaisuudessa.

| Ominaisuus | Kuvaus |
|:--|:--|
| workspaceResourceId | Lomakkeen OMS työtilan tunnus * <Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<työtilan nimi\>*. |
| referencedResources   | Ratkaisu, joka ei poisteta, kun ratkaisu on poistettu resurssien luetteloa. |
| containedResources   | Luettelon resursseista ratkaisu, joka poistaa ratkaisun poistamisen yhteydessä. |

Yllä olevassa esimerkissä on ratkaisussa, jossa runbookin, aikataulu ja Näytä.  Aikataulun ja runbookin ovat *viitatun* **Ominaisuudet** -osan, joten niitä ei poisteta ratkaisun poistamisen yhteydessä.  Näkymä on *sisälsi* , joten se poistetaan, kun ratkaisu on poistettu.


### <a name="plan"></a>Suunnitteleminen
**Suunnitelman** ratkaisu resurssin kohteella ominaisuudet seuraavan taulukon. 

| Ominaisuus | Kuvaus |
|:--|:--|
| Nimi          | Ratkaisun nimi. |
| versio       | Ratkaisun määritetyn käyttäjän tekemät versio. |
| tuotteen       | Tunnistaa ratkaisun yksilöllinen merkkijono. |
| Publisherin     | Publisher-ratkaisun. |


## <a name="other-resources"></a>Muut resurssit
Saat tietoja ja resursseja, jotka löytyvät ratkaisujen seuraavissa artikkeleissa-objektit.

- [Näkymät ja raporttinäkymien](operations-management-suite-solutions-resources-views.md)
- [Automaatio-resurssit](operations-management-suite-solutions-resources-automation.md)

## <a name="testing-a-management-solution"></a>Management-ratkaisun testaaminen
Ennen kuin otat management-ratkaisun, on suositeltavaa testaaminen [Testi AzureRmResourceGroupDeployment](../resource-group-template-deploy.md#deploy-with-powershell)avulla.  Tämä vahvistaa ratkaisutiedosto- ja Ohje, ennen kuin yrität asentaa se tunnistaa mahdolliset ongelmat.



## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [Authoring Azure Resurssienhallinta](../resource-group-authoring-templates.md)mallia.
- Esimerkkejä eri Resurssienhallinta malleja haku [Azure pikaopas malleja](https://azure.microsoft.com/documentation/templates) .
- Näytä [näkymien management-ratkaisun lisääminen](operations-management-suite-solutions-resources-views.md)tiedot.
- Näytä tiedot lisätään [automaatio resurssien management-ratkaisun](operations-management-suite-solutions-resources-automation.md).
 