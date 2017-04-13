<properties
   pageTitle="Authoring Azure Resurssienhallinta malleja | Microsoft Azure"
   description="Luo syntaksin määritettäviä JSON sovellusten Azure Azure Resurssienhallinta-mallit."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Authoring Azure Resurssienhallinta-mallit

Tässä ohjeaiheessa kerrotaan Azure Resurssienhallinta-mallin rakenne. Sen tuottamat mallin ja ominaisuudet, jotka ovat käytettävissä osat eri osiin. Mallin koostuu JSON ja lausekkeita, joiden avulla voit muodostaa käyttöönoton arvot. 

Olet jo asentanut resurssien malli on artikkelissa [Azure Resurssienhallinta-mallin aiemmin resursseilta, vie](resource-manager-export-template.md). Ohjeita mallin luomisesta on artikkelissa [Resurssienhallinta mallin ongelmatilanteita](resource-manager-template-walkthrough.md). Katso suosituksista mallien luominen [parhaita käytäntöjä Azure Resurssienhallinta-mallien luomiseen](resource-manager-template-best-practices.md).

Hyvä JSON-Editorissa voit yksinkertaistaa mallien luomisesta. Saat lisätietoja Visual Studio käyttämisestä mallisi [luominen ja käyttöönotto Azure resurssiryhmät Visual Studio kautta](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). Tietoja ja koodi on artikkelissa [Azure Resurssienhallinta mallit Visual Studio Code käsitteleminen](resource-manager-vs-code.md).

Rajoittaa kokoa 1 Megatavu mallin ja kunkin parametrin tiedoston 64 kilotavua. 1 Megatavu rajoitus koskee mallin Lopputila sen jälkeen, kun se on laajennettu iteratiivinen resurssin määritykset ja muuttujien ja parametrien arvot. 

## <a name="template-format"></a>Mallimuoto

Sen helpoin rakenteen malli sisältää seuraavat osat:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Rakenteen nimeä   | Pakollinen | Kuvaus
| :------------: | :------: | :----------
| $schema        |   Kyllä    | JSON-rakennetiedoston, joka kuvaa mallin kieli-version sijainti. Käytä edellisessä esimerkissä URL-Osoitetta.
| contentVersion |   Kyllä    | Mallin (esimerkiksi 1.0.0.0) versio. Voit antaa mikä tahansa arvo tämä elementti. Kun otat mallin avulla resursseja, tämä arvo voidaan varmistaaksesi, että malli on käytössä.
| Parametrit     |   Ei     | Arvot, jotka tarjotaan, kun käyttöönoton suoritetaan resurssin käyttöönoton mukauttamiseen.
| muuttujat      |   Ei     | Arvot, joita käytetään JSON Vaillinaiset lauseet mallin yksinkertaistaa mallin lausekkeet.
| resurssit      |   Kyllä    | Resurssityypit, jotka on otettu käyttöön tai päivittää resurssiryhmä.
| tulostus        |   Ei     | Arvot, jotka palautetaan käyttöönoton jälkeen.

Olemme tutkia tarkemmin tämän artikkelin mallin osiin. Nyt on tarkistaa osa, joka muodostaa mallin syntaksia.

## <a name="expressions-and-functions"></a>Lausekkeista ja funktioista

Mallin basic syntaksi on JSON. Kuitenkin lausekkeista ja funktioista laajentaa JSON, joka on käytettävissä mallissa. Lausekkeita voit luoda arvoja, jotka eivät ole tarkka literaaliarvot. Lausekkeiden ympärillä on Hakasulkeet [ja], ja arvioidaan, kun malli otetaan käyttöön. Lausekkeiden voit näkyvät JSON merkkijonoarvo ja palauttaa aina toiseen JSON-arvon. Jos haluat käyttää täsmällisen merkkijonon, joka alkaa hakasulje [, sinun on käytettävä hakasulkeiden [[.

Yleensä avulla lausekkeiden Funktiot suorittaa määrittämistä käyttöönottoa varten. Aivan kuten JavaScript--funktiokutsut muotoillut **functionName(arg1,arg2,arg3)**. Piste- ja [Indeksi]-operaattorien ominaisuudet viitata.

Seuraavassa esimerkissä esitetään useita funktioiden käyttämisestä luodessaan arvot:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

Mallin Funktiot koko luetteloon Tutustu [Azure Resurssienhallinta malli-Funktiot](resource-group-template-functions.md). 


## <a name="parameters"></a>Parametrit

Mallin parametrit-kohdassa voit määrittää arvot olet kirjoittanut otettaessa resurssit. Parametriarvot, joiden avulla voit mukauttaa käyttöönotossa antamalla arvot, jotka on suunniteltu erityisesti-ympäristössä (esimerkiksi keskihajonta, Testaa ja tuotannon). Sinulla ei ole antamaan parametrien mallin, mutta ilman parametreja mallin aina käyttöön samoja resursseja nimet, sijainnit ja ominaisuudet.

Voit määrittää käyttöön resurssien parametriarvot koko mallin. Vain parametrit, jotka on määritetty Parametrit-osassa voidaan muiden osien mallin.

Seuraava rakenne parametrien määrittäminen:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Rakenteen nimeä   | Pakollinen | Kuvaus
| :------------: | :------: | :----------
| parameterName  |   Kyllä    | Parametrin nimi. On oltava kelvollinen JavaScript-tunniste.
| tyyppi           |   Kyllä    | Parametrin arvon tyyppi. Näet luettelon alapuolella sallitut sisältötyypit.
| Oletusarvo   |   Ei     | Parametri, jos arvoa ei ole annettu parametrin oletusarvo.
| allowedValues  |   Ei     | Matriisi sallittu arvo parametrille, jotta voidaan varmistaa, että oikea arvo on annettu.
| minValue       |   Ei     | Int-tyypin parametrien pienimmän arvon, tämä arvo on mukaan lukien.
| maxValue       |   Ei     | Int-tyypin parametrien suurimman arvon, tämä arvo on mukaan lukien.
| minLength      |   Ei     | Merkkijono, secureString ja matriisi tyyppi parametrien vähimmäispituus, tämä arvo on mukaan lukien.
| maxLength      |   Ei     | Merkkijono, secureString ja matriisi tyyppi parametrien enimmäispituus, tämä arvo on mukaan lukien.
| kuvaus    |   Ei     | Parametri, joka näkyy käyttäjille mukautettuja julkaisuportaalimalliin-liittymän kautta mallin kuvaus.

Sallitun tiedostotyypit ja -arvot ovat seuraavat:

- **merkkijono**
- **secureString**
- **kokonaisluku**
- **bool**
- **objektin** 
- **secureObject**
- **matriisi**

Määritä valinnainen parametri, on oletusarvo, (voi olla tyhjä merkkijono). 

Jos määrität parametrin nimi on sama jokin mallin käyttöönotto-komento, ohjelma pyytää antamaan parametrin kanssa postfix **FromTemplate**arvon. Esimerkiksi jos lisäät **ResourceGroupName** malliin parametrin, joka on sama kuin [Uusi AzureRmResourceGroupDeployment] **ResourceGroupName** -parametrin[ deployment2cmdlet] cmdlet-komento, sinua kehotetaan arvo antaa **ResourceGroupNameFromTemplate**. Yleensä kannattaa välttää tämän sekaannusta nimeämällä parametrit ei ole samannimistä parametreiksi käytettävien toimintojen käyttöönotto.

>[AZURE.NOTE] Kaikki salasanoja, avaimet ja muita tietoja käytettävä **secureString** tyyppi. Malliparametrit secureString tyyppiä ei voi lukea resurssin käyttöönoton jälkeen. 

Seuraavassa esimerkissä esitetään, miten parametrien määrittäminen:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Lisätietoja syötteen parametriarvot käyttöönoton aikana, katso lisätietoja artikkelista [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](resource-group-template-deploy.md#parameter-file). 

## <a name="variables"></a>Muuttujat

Muuttujat-osassa muodosta arvot, joita voidaan käyttää kaikkialla mallin. Yleensä muuttujat perustuvat parametreista annettua arvoa. Sinun ei tarvitse määrittää, mutta ne usein yksinkertaistaa mallin vähentämällä monimutkaisissa lausekkeissa.

Voit määrittää seuraavia rakenteen kanssa:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

Seuraavassa esimerkissä esitetään määrittäminen muuttuja, jolla muodostetaan kaksi parametria arvoista:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

Seuraavassa esimerkissä on muuttuja, jolla on monimutkaisia JSON ja muuttujat, jotka muut muuttujat-rakennettava:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Resurssit

Resurssit-osassa, jotka on otettu käyttöön tai päivittää resurssien määrittäminen. Tässä osassa voit hakea monimutkaisia, koska sinun on tiedettävä otat käyttöön oikean arvot tyypit. 

Voit määrittää resursseja seuraavaa rakennetta:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Rakenteen nimeä             | Pakollinen | Kuvaus
| :----------------------: | :------: | :----------
| apiVersion               |   Kyllä    | REST API, jonka avulla voi luoda resurssin versio.
| tyyppi                     |   Kyllä    | Resurssin laji. Tämä arvo on yhdistelmä nimitilaa resurssin tarjoajaan ja resurssin laji (kuten **Microsoft.Storage/storageAccounts**).
| Nimi                     |   Kyllä    | Resurssin nimi. Noudata RFC3986 määritelty URI osan rajoitukset. Lisäksi Azure services, joka näyttää resurssin nimen ulkopuolella osapuolet Vahvista nimeä, varmista, että se ei ole yritettiin väärentää käyttäjätiedot. Katso [tarkistaminen resurssinimi](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| sijainti                 |   Vaihtelee  | Tuetut annettu resurssin geo-sijainti. Voit valita minkä tahansa käytettävissä olevista sijainneista, mutta yleensä kannattaa jokin, joka on lähellä käyttäjille. Yleensä myös kannattaa sijoittaa resurssit, jotka vaikuttaa toisiinsa samalla alueella. Useimmat resurssityypit vaadi sijainti, mutta jotkin tiedostotyypit (kuten roolimääritys) eivät edellytä sijainti.
| tunnisteet                     |   Ei     | Tunnisteita, jotka liittyvät resurssi.
| kommentit                 |   Ei     | Muistiinpanojen dokumentoida mallin resurssien osalta
| dependsOn                |   Ei     | Resurssien avulla määritettävän resurssin määräytyy. Resurssien väliset riippuvuudet arvioidaan ja resursseja on otettu käyttöön riippuvaiset järjestykseen. Resurssit eivät ole riippuvainen toisiinsa, kun ne on otettu käyttöön rinnakkain. Pilkuilla erotettu luettelo resurssi voi olla nimiä tai resurssin yksilöivää tunnistetta.
| Ominaisuudet:               |   Ei     | Resurssin kielikohtaiset asetukset. Ominaisuuksien arvot ovat samat kuin annat pyynnön tekstissä REST API-toiminnon (HYLLYTETTY menetelmä), voit luoda resurssin arvot. Saat linkkejä resurssin rakenteen asiakirjat tai REST API- [Resurssienhallinta tarjoajien, alueet, API-versiot ja rakenteet](resource-manager-supported-services.md).
| Kopioi                     |   Ei     | Useamman kuin yhden esiintymän tarvittaessa luoda resurssien määrä. Lisätietoja on artikkelissa [luominen Azure resurssien hallinnan resurssit useita kertoja](resource-group-create-multiple.md). |
| resurssit                |   Ei     | Lapsen resurssit, jotka riippuvat määritettävän resurssi. Voit kirjoittaa vain resurssityypit, ylemmän tason resurssin rakenteen tallentaminen on sallittua. Lapsen resurssityyppi täydellinen nimi sisältää ylemmän tason resurssin laji, kuten **Microsoft.Web/sites/extensions**. Ylemmän tason resurssin riippuvuus ilmaista; Sinun on määritettävä jäsenyys erikseen. 

Tietää, mitä arvoja voit määrittää **apiVersion**, **tyyppi**ja **sijainti** ei ole heti selvää. Voit määrittää lähettäjä-PowerShellin Azure-tai Azure CLI nämä arvot.

Jos haluat kaikkien resurssien palveluntarjoajien **PowerShellin**avulla, käytä:

    Get-AzureRmResourceProvider -ListAvailable

Palautetut luettelosta Etsi kiinnostavan resurssi-palveluita. Jos haluat resurssityyppejä resurssi-palvelun (kuten tallennus), käytä:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Hanki API-versioiden resurssilaji (esimerkiksi tallennustilan tilit), käytä:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Saat tuetut sijainnit resurssilaji-avulla:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Jos haluat kaikkien resurssien palveluntarjoajien **Azure CLI**kanssa, käytä:

    azure provider list

Palautetut luettelosta Etsi kiinnostavan resurssi-palveluita. Jos haluat resurssityyppejä resurssi-palvelun (kuten tallennus), käytä:

    azure provider show Microsoft.Storage

Jos haluat tuetut sijainnit ja API-versiot, käytä:

    azure provider show Microsoft.Storage --details --json

Lisätietoja resurssin tarjoajien on artikkelissa [Resurssienhallinta tarjoajien, alueet, API-versiot ja rakenteet](resource-manager-supported-services.md).

Resurssit-osio sisältää matriisin ottamaan resurssit. Kullekin resurssille sisällä voit myös määrittää matriisin lapsen resurssit. Tämän vuoksi resurssit-osassa voi olla rakenne, kuten:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


Seuraavassa esimerkissä **Microsoft.Web/serverfarms** resurssi ja **Microsoft.Web/sites** resurssin lapsi **tunnisteet** resurssi. Huomaa, että sivusto on merkitty määräytyy palvelinklusterin koska palvelinklusterin on oltava olemassa ennen sivuston voidaan ottaa käyttöön. Huomaa myös, että **laajennukset** resurssi on sivuston.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Tulostus

Tulostus-kohdassa voit määrittää arvot, jotka palautetaan käyttöönotto. Voit esimerkiksi palauttaa käyttöön resurssin URI.

Seuraavassa esimerkissä esitetään rakenteen tulostus-määritys:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Rakenteen nimeä   | Pakollinen | Kuvaus
| :------------: | :------: | :----------
| outputName     |   Kyllä    | Tuloksena saadun arvon nimi. On oltava kelvollinen JavaScript-tunniste.
| tyyppi           |   Kyllä    | Tuloksena saadun arvon tyyppi. Arvot tukevat samaan tapaan kuin mallin syöteparametrit.
| arvo          |   Kyllä    | Mallin kielen lauseke, joka arvioidaan ja palauttaa tuloksena saadun arvon.


Seuraavassa esimerkissä on arvo, joka palautetaan tulostus-osassa.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Lisätietoja tulosteen käyttämisestä on kohdassa [jakaminen state Azure Resurssienhallinta mallit](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Seuraavat vaiheet
- Valmis monista erilaisista ratkaisuja malleja on artikkelissa [Azure pikaopas malleja](https://azure.microsoft.com/documentation/templates/).
- Lisätietoja tietoja voidaan käyttää mallin funktioista on artikkelissa [Azure Resurssienhallinta malli-Funktiot](resource-group-template-functions.md).
- Jos haluat yhdistää useita malleja käyttöönoton aikana, artikkelissa [linkitetyt mallit Azure resurssien hallinta](resource-group-linked-templates.md).
- Joudut ehkä käyttää resursseja, jotka sisältyvät eri resurssiryhmä. Tässä skenaariossa yhteistä, kun käsittelet tallennustilan asiakkaat- tai virtual verkkoja, jotka on jaettu useiden resurssin ryhmien. Lisätietoja on artikkelissa [resourceId-funktiota](resource-group-template-functions.md#resourceid).


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
