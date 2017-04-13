<properties
    pageTitle="Vie Azure Resurssienhallinta malli | Microsoft Azure"
    description="Käytä mallin tuominen aiemmin resurssiryhmä Azure resurssien hallinta."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/20/2016"
    ms.author="tomfitz"/>

# <a name="export-an-azure-resource-manager-template-from-existing-resources"></a>Azure Resurssienhallinta-mallin vieminen aiemmin resurssit

Resurssien hallinnan avulla voit Resurssienhallinta-mallin vieminen olemassa olevan tilauksen resurssit. Voit käyttää luotu mallin avulla näet, kuinka mallin syntaksi tai automatisoida ratkaisu lukea tarpeen mukaan.

On tärkeää huomaat, että on vietävä malli kahdella eri tavalla:

- Voit viedä todellinen mallin, jota käytit käyttöönottoa. Viedyt malli sisältää kaikki parametrit ja muuttujat tarkalleen siinä muodossa kuin ne näkyi alkuperäinen malli. Tämä vaihtoehto on hyötyä, kun olet ottanut resursseja portaalin kautta. Haluat nyt, katso, miten voit käyttää Luo resursseja mallia.
- Voit viedä mallin, joka vastaa resurssiryhmä nykyisen tilan. Viedyt malli ei perustu mallia, jota käytit käyttöönottoa varten. Sen sijaan ohjelma luo mallin, joka on resurssiryhmän. Viedyt mallissa on koodattu arvojen ja ei ehkä ole niin monta parametrit sellaisena yleensä määritetään. Tämä vaihtoehto on hyödyllinen, kun olet muokannut resurssiryhmä portal tai komentosarjojen kautta. Seuraavaksi sinun täytyy siepata resurssiryhmä mallina.

Tässä ohjeaiheessa esitellään kummassakin.

Tässä opetusohjelmassa Azure-portaaliin sisäänkirjautuminen tallennustilan tilin luominen ja vie tallennustilan tilin malli. Voit lisätä virtual verkon muokata resurssiryhmän. Lopuksi voit viedä mallina, joka edustaa nykyisestä tilasta. Tässä artikkelissa keskitytään yksinkertaistettu infrastruktuuri, vaikka voi viedä monimutkaisempia ratkaista mallin saman näiden ohjeiden avulla.

## <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

1. Valitse [Azure portal](https://portal.azure.com) **Uusi** > **tallennustilan** > **tallennustilan tilin**.

      ![tallennuspaikan luominen](./media/resource-manager-export-template/create-storage.png)

2. Luoda tallennustilan tilin nimi- **tallennustilan**, nimikirjaimesi ja päivämäärä. Tallennustilan tilin nimi on oltava yksilöllinen Azure. Jos nimi on jo käytössä, näkyvissä virhe, joka ilmaisee nimi on käytössä. Kokeile muunnelma. Resurssiryhmä-Luo uusi resurssiryhmä ja anna sille nimi **ExportGroup**. Voit käyttää muita ominaisuuksia oletusarvot. Valitse **Luo**.

      ![Anna arvot tallennustila](./media/resource-manager-export-template/provide-storage-values.png)

Käyttöönotto voi kestää jonkin aikaa. Kun asennus on valmis, tilauksesi sisältää tallennustilan-tili.

## <a name="view-a-template-from-deployment-history"></a>Näytä mallin käyttöönoton historia

1. Siirry Uusi resurssiryhmä resurssi-ryhmän sivu. Huomaa, että sivu näyttää edellisen käyttöönoton tuloksen. Valitse tämä linkki.

      ![resurssien ryhmä-sivu](./media/resource-manager-export-template/resource-group-blade.png)

2. Näet ominaisuuksissa ryhmän historiatiedot. Tässä tapauksessa sivu näyttää luultavasti vain yhden käyttöönotto. Valitse käyttöönottoon.

     ![viimeinen käyttöönotto](./media/resource-manager-export-template/last-deployment.png)

3. Sivu näyttää yhteenvedon käyttöönotto. Yhteenveto sisältää käyttöönottoa ja toimintojaan ja, jonka ilmoitit parametrien arvot tilasta. Voit avata mallin, jota käytit käyttöönotto valitsemalla **Näytä malli**.

     ![Näytä käyttöönoton yhteenveto](./media/resource-manager-export-template/deployment-summary.png)

4. Resurssienhallinta hakee seuraavasti kuusi tiedostot puolestasi:

   1. **Malli** - malli, joka määrittää ratkaisu infrastruktuuri. Tallennustilan tilin kautta portaalin luotaessa Resurssienhallinta mallin avulla voit ottaa sen ja tallentaa mallin myöhempää käyttöä varten.
   2. **Parametrit** - parametri-tiedosto, joiden avulla voit välittää arvot käyttöönoton aikana. Se sisältää arvot, jonka ilmoitit ensimmäisen käyttöönoton aikana, mutta voit muuttaa arvoja, kun mallin käyttöön.
   3. **CLI** - Azure command-line-liittymän (CLI) komentosarjatiedosto, joiden avulla voit ottaa mallin käyttöön.
   4. **PowerShell** - PowerShellin Azure-komentosarjatiedosto, joiden avulla voit ottaa mallin käyttöön.
   5. **.NET** - A .NET-luokka, joiden avulla voit ottaa mallin käyttöön.
   6. **Ruby** - A Ruby-luokka, joiden avulla voit ottaa mallin käyttöön.

     Tiedostot ovat käytettävissä linkkien kautta koko sivu. Oletusarvon mukaan sivu näyttää malli.

       ![Näytä malli](./media/resource-manager-export-template/view-template.png)

     Oletetaan, että kiinnitettävä erityistä huomiota malliin. Mallin pitäisi muistuttaa seuraavaa:

        {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parametrit": {"nimi": {"tyyppi": "Merkkijonon"}, "accountType": {"tyyppi": "Merkkijonon"}, "sijainnin": {"tyyppi": "Merkkijonon"}, "encryptionEnabled": {"oletusarvo": arvo on EPÄTOSI, "tyyppi": "Bool"}}, "resurssien": [{"tyyppi": "Microsoft.Storage/storageAccounts", "tuote": {"nimi": "[parameters('accountType')]"}, "lajin": "Tallennustilan", "nimi": "[parameters('name')]", "apiVersion": "2016-01-01", "sijainnin": "[parameters('location')]", "ominaisuudet": {"salaus": {"palvelut": {"blob-: {"käytössä":"[parameters('encryptionEnabled')]"}},"keySource":"Microsoft.Storage"}}}]}
 
Tämä malli on todellinen malli tallennustilan tilin luominen. Huomaa, että se sisältää parametreja, joiden avulla voit ottaa erityyppisiä tallennustilan tilit. Lisätietoja mallin rakenne on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md). Katso Funktiot, voit käyttää mallin kattavaan luetteloon, [Azure Resurssienhallinta malli-Funktiot](resource-group-template-functions.md).


## <a name="add-a-virtual-network"></a>Lisää virtual verkko

Edellisessä osassa lataamaasi mallia edustaa infrastruktuuri, alkuperäinen käyttöönottoa varten. Se kuitenkin huomioon käyttöönoton jälkeen tekemäsi muutokset.
Esitä ongelman Muokkaa resurssiryhmän japanin lisäämällä virtual verkon kautta portaalin.

1. Valitse **Lisää**resurssien ryhmä-sivu.

      ![Lisää resurssi](./media/resource-manager-export-template/add-resource.png)

2. Valitse käytettävissä olevat resurssit **Virtual verkkoon** .

      ![Valitse VPN](./media/resource-manager-export-template/select-vnet.png)

2. Nimeä virtual verkoston **VNET**ja muita ominaisuuksia oletusarvojen käyttö. Valitse **Luo**.

      ![ilmoituksen määrittäminen](./media/resource-manager-export-template/create-vnet.png)

3. Kun virtual verkko on otettu resurssiryhmän, tarkista uudelleen käyttöönoton historia. Näet nyt kaksi asennuksia. Jos toinen käyttöönoton ei ole näkyvissä, joudut ehkä suljettava resurssien ryhmä-sivu ja avaat sen uudelleen. Valitse uudempaan käyttöönotto.

      ![käyttöönoton historia](./media/resource-manager-export-template/deployment-history.png)

4. Näytä mallin kyseisen käyttöönottoa varten. Huomaa, että se määrittää virtual verkkoon. Voit ottaa käyttöön aiemmin tallennustilan tilin ei sisällä. Sinulla ei ole enää mallina, joka vastaa resurssiryhmä kaikki resurssit.

## <a name="export-the-template-from-resource-group"></a>Resurssiryhmä mallin vieminen

Saat nykyisen tilan resurssiryhmä-Vie malli, jossa näkyy tilannevedoksen resurssiryhmän.  

> [AZURE.NOTE] Et voi viedä, jossa on yli 200 resursseja resurssiryhmän mallin.

1. Voit tarkastella resurssiryhmä malli valitsemalla **automaatio-komentosarjan**.

      ![Vie resurssiryhmä](./media/resource-manager-export-template/export-resource-group.png)

     Kaikki resurssityypit tue Vie malli-funktiota. Jos Resurssiryhmä sisältää vain tallennustilan tili ja virtual suorittaa tässä artikkelissa verkon, virheen ei näy. Jos olet luonut muuntyyppisiä resurssin, voit nähdä virhesanoma, jossa kerrotaan, että on ongelma, jossa Vie. Opit käsittelemisestä kyseiset ongelmat [ongelmien Vie](#fix-export-issues) -kohdassa.

      

2. Näet kuusi tiedostot, jotka voit tehdä ratkaisuun käyttöön uudelleen, mutta tällä hetkellä malli on hieman erilainen. Tämä malli on vain kaksi parametria: yksi tallennustilan tilin nimi ja VPN-Nimisarakkeen.

        "parameters": {
          "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
          },
          "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
          }
        },

     Resurssienhallinta ei hakea malleja, jota käytit käyttöönoton aikana. Sen sijaan Luo uusi malli, joka perustuu resurssit nykyiset määritykset. Esimerkiksi mallin asettaa tallennustilan tilin sijainti ja replikoinnin arvon:

        "location": "northeurope",
        "tags": {},
        "properties": {
            "accountType": "Standard_RAGRS"
        },

3. Sinulla on muutama asetusten työskentelet tämän mallin avulla. Voit ladata mallin ja käyttää sitä paikallisesti JSON-editorissa. Vaihtoehtoisesti voit tallentaa mallin kirjastoon ja käyttää sitä portaalin kautta.

     Jos olet tottunut JSON-editorin, kuten [Ja-koodin](resource-manager-vs-code.md) tai [Visual Studiossa](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md), haluat ehkä muuttaa paikallisesti mallin lataaminen ja kyseisen editorilla. Jos sinulla ei ole määrittänyt JSON-editorissa, haluat ehkä muuttaa portaalin kautta mallin muokkaaminen. Tässä ohjeaiheessa loput oletetaan, että portaalissa kirjastoon tallennettuja mallin. Kuitenkin teet syntaksi muutokset malliin, JSON-editorin tai portaalin kautta paikallisesti käsittelemiseen.

     Jos haluat tutustua paikallisesti, valitse **Lataa**.

      ![Mallin lataaminen](./media/resource-manager-export-template/download-template.png)

     Toimii portaalissa kautta, valitse **Lisää kirjastoon**.

      ![Lisää kirjastoon](./media/resource-manager-export-template/add-to-library.png)

     Kun lisäät kirjastoon mallin, Anna mallille nimi ja kuvaus. Valitse sitten **Tallenna**.

     ![mallin arvot](./media/resource-manager-export-template/set-template-values.png)

4. Haluat tarkastella kirjaston tallennetaan mallina, valitse **Lisää palveluja**, kirjoita **malleja** voit suodattaa tulokset valitsemalla **Mallit**.

      ![mallien hakeminen](./media/resource-manager-export-template/find-templates.png)

5. Valitse malli tallennettuja tiedostoja nimisen.

      ![Valitse malli](./media/resource-manager-export-template/select-library-template.png)

## <a name="customize-the-template"></a>Mallin mukauttaminen

Viedyt mallin toimii hyvin, jos haluat luoda saman tallennustilan tilin ja virtual verkon jokaisen käyttöönottoa varten. Resurssienhallinta on kuitenkin asetukset niin, että voit ottaa malleja usein lisää joustavuutta. Esimerkiksi käyttöönoton aikana haluat ehkä määrittää tallennustilan tilin luominen tai virtual verkko-osoitteen etuliite ja aliverkon etuliite arvon.

Tässä osassa Lisää parametreja viedyn malliin niin, että voit käyttää mallia, kun otat käyttöön näiden resurssien muiden ympäristöjen. Jotkin ominaisuudet lisääminen myös pienentää todennäköisyys encountering virhe, kun otat mallin mallin. Ei enää ole arvata tallennustilan tilin yksilöllinen nimi. Sen sijaan mallin Luo yksilöllinen nimi. Voit rajata vain kelvollisia asetukset tallennustilan tilityypin määritetyt arvot.

1. Valitse **Muokkaa** , jos haluat muokata mallia.

     ![Näytä mallin](./media/resource-manager-export-template/show-template.png)

1. Valitse malli.

     ![mallin muokkaaminen](./media/resource-manager-export-template/edit-template.png)

1. Voivat siirtää arvot, jonka haluat määrittää käyttöönoton aikana, tilalle uudet parametrin määritykset **Parametrit** -osassa. Huomaa, **storageAccount_accountType** **allowedValues** arvot. Jos annat vahingossa virheellisen arvon, kyseisen virheen tunnistetaan ennen käyttöönottoa käynnistyy. Huomaa myös, että kirjoitit vain etuliite tallennustilan tilin nimi ja etuliite on 11 merkkiä. Voit rajoittaa etuliite 11 merkkiä, voit varmistaa, että koko nimi enintään tallennustilan tilin merkkien enimmäismäärä. Etuliitteen avulla voit käyttää nimeämiskäytäntöä tallennustilan tilit. Näkyviin tulee yksilöllinen nimi luominen seuraavaan vaiheeseen.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Mallin **muuttujat** -osa on tällä hetkellä tyhjä. **Muuttujat** -osassa voit luoda arvoja, jotka helpottavat mallin muiden syntaksia. Tässä osassa tilalle uuden muuttujan määrityksen. **StorageAccount_name** muuttujan yhdistää etuliite-parametri on yksilöllinen merkkijono, joka on luotu resurssiryhmän tunnuksen perusteella. Ei enää ole arvata yksilöivä nimi, kun se tarjoaa parametrin arvon.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Parametrit ja muuttujan käyttäminen resurssin määritelmiä, tilalle uusi resurssi määritelmät **resurssit** -osa. Huomaa, että vähän on muuttunut resurssin määritelmiä kuin arvo, joka on liitetty resurssin-ominaisuus. Ominaisuudet ovat samat kuin ominaisuudet viedyn mallista. Ominaisuudet ovat vain liittäminen parametriarvot koodattu arvojen sijaan. Resurssien sijainti on määritetty käyttämään samaan sijaintiin kuin resurssiryhmä **resourceGroup () .location** lausekkeen avulla. Muuttuja, jolla voit luoda tallennustilan tilin nimi viitataan lauseke **muuttujat** kautta.

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

4. Valitse **OK** , kun olet valmis mallin muokkaaminen.

5. Valitse **Tallenna** tallentamaan muutoksia malliin.

     ![mallin tallentaminen](./media/resource-manager-export-template/save-template.png)

6. Päivitetyn mallin käyttöön, valitse **Ota käyttöön**.

     ![mallin ottaminen käyttöön](./media/resource-manager-export-template/deploy-template.png)

7. Anna parametriarvot ja valitse uusi resurssiryhmä ottamaan resurssien avulla.

## <a name="update-the-downloaded-parameters-file"></a>Päivityksen ladatun parametrit-tiedosto

Jos käsittelet ladatut tiedostot (portaalin kirjaston sijaan), on päivitettävä ladatut parametri-tiedosto. Se ei enää vastaa mallin parametrit. Sinulla ei ole käyttämään Parametritiedostoa, mutta se yksinkertaistamaan prosessia, kun ympäristössä käyttöön. Voit käyttää oletusarvot, jotka on määritetty mallin monia parametrit niin, että parametri-tiedosto on vain kaksi arvoa.

Korvaa parameters.json tiedoston sisältö:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Päivitetty parametri-tiedosto sisältää arvot vain parametrit, joita ei ole oletusarvon. Voit antaa arvot muut parametrit silloin, kun arvo, joka ei ole sama kuin oletusarvoa.

## <a name="fix-export-issues"></a>Vie korjaaminen

Kaikki resurssityypit tue Vie malli-funktiota. Resurssienhallinta erityisesti Vie joitakin resurssityypit estää paljastaa luottamuksellisia tietoja. Esimerkiksi jos sinulla on sivuston config yhteysmerkkijonon, todennäköisesti ei haluat sen näkyvän erikseen viedyn malli. Saat tämän ongelman lisäämällä puuttuvat resurssien takaisin lomakemalliin manuaalisesti.

> [AZURE.NOTE] Vie ongelmia ilmetä vain, kun vieminen resurssiryhmä sijaan käyttöönoton historia. Jos edellisen käyttöönoton edustaa tarkasti resurssiryhmän nykyisen tilan, vie mallin käyttöönoton History-osiosta eikä resurssi ryhmästä. Vieminen vain resurssiryhmä, kun olet tehnyt muutoksia, joka ei ole määritetty yhden mallin resurssiryhmä.

Esimerkiksi jos resurssiryhmä, joka sisältää online, SQL-tietokantaan ja yhteysmerkkijonon sivuston config mallin, näet seuraavan sanoman:

![Näytä virhe](./media/resource-manager-export-template/show-error.png)

Valitsemalla viesti näkyy täsmälleen mitkä resurssityypit ei viety. 
     
![Näytä virhe](./media/resource-manager-export-template/show-error-details.png)

Tässä ohjeaiheessa esitellään yleisiä ratkaisuja.

### <a name="connection-string"></a>Yhteysmerkkijono

Lisää verkkosivustoista resurssia yhteysmerkkijonon määritelmä-tietokantaan:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "apiVersion": "2015-08-01",
      "type": "config",
      "name": "connectionstrings",
      "dependsOn": [
          "[concat('Microsoft.Web/Sites/', parameters('<site-name>'))]"
      ],
      "properties": {
          "DefaultConnection": {
            "value": "[concat('Data Source=tcp:', reference(concat('Microsoft.Sql/servers/', parameters('<database-server-name>'))).fullyQualifiedDomainName, ',1433;Initial Catalog=', parameters('<database-name>'), ';User Id=', parameters('<admin-login>'), '@', parameters('<database-server-name>'), ';Password=', parameters('<admin-password>'), ';')]",
              "type": "SQLServer"
          }
      }
    }
  ]
}
```    

### <a name="web-site-extension"></a>Web-sivuston tunniste

Lisää sivuston resurssin määritelmän koodin asentaminen:

```
{
  "type": "Microsoft.Web/sites",
  ...
  "resources": [
    {
      "name": "MSDeploy",
      "type": "extensions",
      "location": "[resourceGroup().location]",
      "apiVersion": "2015-08-01",
      "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('<site-name>'))]"
      ],
      "properties": {
        "packageUri": "[concat(parameters('<artifacts-location>'), '/', parameters('<package-folder>'), '/', parameters('<package-file-name>'), parameters('<sas-token>'))]",
        "dbType": "None",
        "connectionString": "",
        "setParameters": {
          "IIS Web Application Name": "[parameters('<site-name>')]"
        }
      }
    }
  ]
}
```

### <a name="virtual-machine-extension"></a>Virtuaalikoneen tunniste

Esimerkkejä virtuaalikoneen laajennukset on [Azure Windows AM tunniste määritysten esimerkkejä](./virtual-machines/virtual-machines-windows-extensions-configuration-samples.md).

### <a name="virtual-network-gateway"></a>VPN-yhdyskäytävän

Lisää VPN-yhdyskäytävän resurssityyppi.

```
{
  "type": "Microsoft.Network/virtualNetworkGateways",
  "name": "[parameters('<gateway-name>')]",
  "apiVersion": "2015-06-15",
  "location": "[resourceGroup().location]",
  "properties": {
    "gatewayType": "[parameters('<gateway-type>')]",
    "ipConfigurations": [
      {
        "name": "default",
        "properties": {
          "privateIPAllocationMethod": "Dynamic",
          "subnet": {
            "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', parameters('<vnet-name>'), parameters('<new-subnet-name>'))]"
          },
          "publicIpAddress": {
            "id": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('<new-public-ip-address-Name>'))]"
          }
        }
      }
    ],
    "enableBgp": false,
    "vpnType": "[parameters('<vpn-type>')]"
  },
  "dependsOn": [
    "Microsoft.Network/virtualNetworks/codegroup4/subnets/GatewaySubnet",
    "[concat('Microsoft.Network/publicIPAddresses/', parameters('<new-public-ip-address-Name>'))]"
  ]
},
```

### <a name="local-network-gateway"></a>Paikallinen yhdyskäytävien

Lisää paikalliseen verkkoon kirjauduttaessa yhdyskäytävän resurssityyppi.

```
{
    "type": "Microsoft.Network/localNetworkGateways",
    "name": "[parameters('<local-network-gateway-name>')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "localNetworkAddressSpace": {
        "addressPrefixes": "[parameters('<address-prefixes>')]"
      }
    }
}
```

### <a name="connection"></a>Yhteys

Lisää yhteys resurssityyppi.

```
{
    "apiVersion": "2015-06-15",
    "name": "[parameters('<connection-name>')]",
    "type": "Microsoft.Network/connections",
    "location": "[resourceGroup().location]",
    "properties": {
        "virtualNetworkGateway1": {
        "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', parameters('<gateway-name>'))]"
      },
      "localNetworkGateway2": {
        "id": "[resourceId('Microsoft.Network/localNetworkGateways', parameters('<local-gateway-name>'))]"
      },
      "connectionType": "IPsec",
      "routingWeight": 10,
      "sharedKey": "[parameters('<shared-key>')]"
    }
},
```


## <a name="next-steps"></a>Seuraavat vaiheet

Onnittelen! Kun olet oppinut mallin tuominen resursseja, jonka loit portaali.

- Voit ottaa mallin [PowerShellin](resource-group-template-deploy.md) [Azure CLI](resource-group-template-deploy-cli.md)ja [REST API](resource-group-template-deploy-rest.md)kautta.
- Katso, miten voit viedä mallin PowerShellin kautta, artikkelissa [Azure PowerShellin Azure resurssien hallinta](powershell-azure-resource-manager.md).
- Katso, miten voit viedä läpi Azure CLI mallin, artikkelissa [Azure-CLI Mac, Linux-ja Windows Azure resurssien hallinta](xplat-cli-azure-resource-manager.md).
