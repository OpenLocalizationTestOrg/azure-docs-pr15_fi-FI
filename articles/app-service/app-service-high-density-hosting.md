<properties
    pageTitle="HD isännöinnin Azure App palvelun | Microsoft Azure"
    description="HD isännöinnin Azure sovelluksen-palvelusta"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>HD isännöinnin Azure sovelluksen-palvelusta

Sovelluksen palvelua käytettäessä sovelluksen erillisen kaksi antaman kapasiteettiin:

- **Sovelluksen:** Edustaa sovellus ja runtime sen asetukset. Esimerkiksi se sisältää version, .NET, jossa suorituksen ladataan-sovelluksen asetusten jne.

- **Sovelluksen palvelusopimus:** Määrittää kapasiteettia, käytettävissä olevat ominaisuudet ja sijainti-sovelluksen ominaisuudet. Esimerkiksi ominaisuuksia voi olla suuri (neljä sydämiä) tietokoneeseen, neljä esiintymät, Yhdysvaltojen Itä Premium-ominaisuuksia.

Sovelluksen-palvelusopimus on aina linkitetty sovelluksen, mutta App palvelusopimus voidaan lisätä vähintään yksi sovellus resursseja.

Tuloksena platform tarjoaa joustavuutta eristää yhden sovelluksen tai useita sovellukset resurssien jakaminen jakamalla sovelluksen-palvelusopimus on.

Kun useita sovelluksia jakaa sovelluksen-palvelusopimus, kyseisen sovelluksen esiintymää suoritetaan kaikki esiintymät, sovelluksen palvelusopimus.

## <a name="per-app-scaling"></a>Sovelluksen skaalaus kohden
*Kausi app skaalaus* on ominaisuus, joka voidaan käytössä App palvelun suunnitelman tasolla ja sitten käyttää sovellusta kohden.

Sovelluksen kohti skaalaus Skaalaa sovelluksen itsenäisesti App palvelusopimus, jossa se sijaitsee. Tällä tavalla App palvelusopimus voidaan määrittää tarjoamaan 10 esiintymät, mutta sovelluksen voidaan määrittää skaalata vain ne 5.

Seuraavat Azure Resurssienhallinta-malli luo sovelluksen Service-palvelupaketti, joka on skaalattu 10 esiintymät ja sovelluksen, joka on määritetty kohti app skaalaus ja Skaalaa vain 5 esiintymät.

Sovelluksen palvelusopimus on **sivuston kohti skaalaus** -ominaisuuden määrittäminen tosi ( `"perSiteScaling": true`). Sovellus on määrittäminen 5 **työntekijöiden määrä** (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Suositellut määritykset HD isännöinnin

Kausi app skaalaus on ominaisuus, joka on käytössä julkisen Azure alueiden ja sovelluksen Service-ympäristössä. Suositeltu toimintaperiaate on kuitenkin käyttää sovelluksen palvelun ympäristöissä hyödyntää niiden lisäominaisuudet ja kapasiteetin suurempi jakavat.  

Määritä isännöinnin sovelluksia varten HD seuraavasti:

1. Määritettävä sovelluksen Service-ympäristö, ja valitse työntekijä resurssivarantoon, joka on varattu isännöintipalvelu HD-skenaario.

1. Luo yksittäisen sovelluksen palvelusopimus ja skaalata käytettävissä kapasiteetin käyttämisestä työntekijä resurssivarantoon.

1. Määrittää sivuston kohti skaalauksen merkinnän tosi App palvelusopimus.

1. Uusien sivustojen luotu ja määritetty kyseisen sovelluksen palvelusopimus **numberOfWorkers** -ominaisuuden arvoksi **1**. Suurin tiheyden mahdollista työntekijä kannan-määritysten käyttäminen tuottaa.

1. Työntekijöiden määrä voi määrittää itsenäisesti lisäresursseja myöntäminen tarpeen mukaan-sivustoa kohden. Suuri käyttäminen sivuston voi esimerkiksi määrittää **numberOfWorkers** **3** on kyseisen sovelluksen käsittely kapasiteetin, kun vähäisen käytön sivustojen asetetaan **numberOfWorkers** **1**.
