<properties
   pageTitle="Resurssien Azure CLI ja mallin käyttöön | Microsoft Azure"
   description="Ota käyttöön resurssi Azure Azure Resurssienhallinta ja Azure CLI avulla. Resursseja on määritetty Resurssienhallinta-mallissa."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Resurssien Resurssienhallinta mallit ja Azure CLI käyttöönotto

> [AZURE.SELECTOR]
- [PowerShellin](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-group-template-deploy-rest.md)

Tässä ohjeaiheessa kerrotaan, miten voit käyttää Azure CLI Resurssienhallinta malleja resurssien käyttöön Azure.  

> [AZURE.TIP] Saat apua virheen virheenkorjaus käyttöönoton aikana:
>
> - [Näytä käyttöönottotoimintoja, joiden Azure CLI](resource-manager-troubleshoot-deployments-cli.md) käytön tiedot, joiden avulla voit opetella virheiden vianmääritys
> - Yleisiä käyttöönoton virheiden ratkaisemisesta saat [vianmääritys yleisiä virheitä, kun resursseja Azure Azure resurssien hallinnan käyttöönotto](resource-manager-common-deployment-errors.md)

Mallin voi olla paikallisen tiedoston tai ulkoiseen tiedostoon, joka on käytettävissä URI-Osoitteen kautta. Kun malli sijaitsee tallennustilan tilin, voit rajoittaa malliin ja anna jaettu käyttöoikeustietue allekirjoitus (SAS) käyttöönoton aikana.

## <a name="quick-steps-to-deployment"></a>Vaiheittaiset ohjeet käyttöönottoon

Tässä artikkelissa kuvataan kaikki vaihtoehdot käytettävissä käyttöönoton aikana. Kuitenkin usein tarvitset vain yksinkertaisia komentoja. Aloita nopeasti käyttöönoton kanssa, käytä seuraavia komentoja:

    azure group create -n ExampleResourceGroup -l "West US"
    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

Lisätietoja asetukset, jotka saattavat olla paremmin sopivia käyttämässäsi skenaariossa käyttöönottoa varten, Jatka lukemista tässä artikkelissa.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-azure-cli"></a>Azure CLI käyttöönotto

Jos et ole aiemmin käyttänyt Azure CLI resurssien hallinta, katso [Azure-CLI Mac, Linux-ja Windows Azure resurssien hallinnan](xplat-cli-azure-resource-manager.md).

1. Kirjaudu sisään Azure-tili. Kun olet antanut tunnistetiedot-komento palauttaa kirjautumisnimi.

        azure login
  
        ...
        info:    login command OK

2. Jos sinulla on useita tilauksia, antaa Tilaustunnus, jota haluat käyttää käyttöönottoa varten.

        azure account set <YourSubscriptionNameOrId>

3. Siirry Azure Resurssienhallinta moduuli. Saat vahvistusviestin siitä, uusi tila.

        azure config mode arm
   
        info:     New mode is arm

4. Jos sinulla ei ole olemassa resurssiryhmä, Luo resurssiryhmä. Anna nimi resurssiryhmä ja sijainnin, jotka tarvitset ratkaisu. Sinun on annettava resurssiryhmän sijainti, koska tietoja resursseja resurssiryhmän metatiedot. Yhteensopivuuden vuoksi haluat ehkä määrittää, että metatietojen tallennuspaikan. Suosittelemme, Määritä sijainti, johon useimmat resurssien tallennetaan. Samaan sijaintiin avulla voidaan yksinkertaistaa mallin.

        azure group create -n ExampleResourceGroup -l "West US"

     Uusi resurssiryhmä yhteenveto palautetaan.
   
        info:    Executing command group create
        + Getting resource group ExampleResourceGroup
        + Creating resource group ExampleResourceGroup
        info:    Created resource group ExampleResourceGroup
        data:    Id:                  /subscriptions/####/resourceGroups/ExampleResourceGroup
        data:    Name:                ExampleResourceGroup
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags:
        data:
        info:    group create command OK

5. Vahvista käyttöönoton ennen kuin suoritat suorittamalla **azure ryhmän malli Vahvista** -komennon. Kun käyttöönoton testaaminen määritettävä parametrit samalla tavalla kuin yksittäiselle suoritettaessa käyttöönoton (kuten seuraavassa vaiheessa).

        azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup

5. Ottaa käyttöön resursseja resurssiryhmän, suorita seuraava komento ja anna tarvittavat parametrit. Parametrit ovat käyttöönoton nimi, resurssiryhmän, polku tai URL-Osoitteen nimi mallin ja muut tarvittavat käyttämässäsi skenaariossa parametrit. 
   
     Sinulla on tarjoamiseksi parametriarvot seuraavista kolmesta vaihtoehdosta: 

     1. Parametrien käyttäminen tekstiin ja paikallisen mallina. Kunkin parametrin on muodossa: `"ParameterName": { "value": "ParameterValue" }`. Seuraavassa esimerkissä esitetään kanssa ohjausmerkkejä parametrit.

            azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     2. Parametrien käyttäminen tekstiin ja linkin lisääminen malliin.

            azure group deployment create --template-uri <LinkToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment

     3. Käytä parametri-tiedosto. Lisätietoja mallitiedosto on artikkelissa [parametri tiedosto](#parameter-file).
    
            azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

     Kun resursseja on otettu käyttöön jonkin yllä kolmella avulla, näkyviin tulee käyttöönoton yhteenveto.
  
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        ...
        info:    group deployment create command OK

     Voit suorittaa valmis käyttöönoton määrittämällä arvoksi **Valmis** **tila** . Ole varovainen, kun käytetään tässä tilassa voit epähuomiossa poistaa resursseja, joita ei ole mallin.

        azure group deployment create --mode Complete -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

6. Jos haluat lisätietoja, joiden avulla voit vianmääritys käyttöönoton virheitä, käytä **Virheenkorjaus-asetus** -parametrin käyttöönoton Kirjaudu. Voit määrittää, että sisällön pyynnön ja vastauksen sisällön on kirjautunut käyttöönoton-toimintoa.

        azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

## <a name="deploy-template-from-storage-with-sas-token"></a>SAS tunnuksen ja tallennustilaa mallin ottaminen käyttöön

Voit lisätä mallisi tallennustilan tilin ja linkittää ne SAS tunnuksen käyttöönoton aikana.

> [AZURE.IMPORTANT] Noudattamalla seuraavia ohjeita blob, jossa malli on käytettävissä vain tilin omistajan. Kuitenkin luodessasi SAS-tunnuksen, blob Blob-objektien voivat käyttää kaikki, URI. Jos toisen käyttäjän sieppaa URI-käyttäjä on käyttää mallia. SAS tunnusta on hyvä keino mallisi pääsyä, mutta älä sisällytä luottamuksellisia tietoja, kuten salasanat suoraan mallissa.

### <a name="add-private-template-to-storage-account"></a>Lisää tallennustilan tilin yksityinen-malli

Tallennustilan tilin mallien seuraavasti:

1. Luo resurssiryhmä.

        azure group create -n "ManageGroup" -l "westus"

2. Tallennustilan tilin luominen. Tallennustilan tilin nimi on oltava yksilöivä Azure, anna niin oman tilin nimi.

        azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates

3. Määritä muuttujat tallennustilan tilin ja avaimen avulla.

        export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
        export AZURE_STORAGE_ACCESS_KEY={storage_account_key}

4. Voit luoda säilön. Käyttöoikeudet on määritetty **käytöstä** toisin sanoen säilö on ainoastaan niiden omistaja.

        azure storage container create --container templates -p Off 
        
4. Mallin lisääminen säilöön.

        azure storage blob upload --container templates -f c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Anna SAS tunnuksen käyttöönoton aikana

Ottamaan yksityinen mallin tallennustilan tilillä noutaa SAS tunnuksen ja Sisällytä se mallin URI.

1. Luo SAS tunnuksen, jonka lukuoikeudet ja käyttörajoitukset voimassaolon ajan. Määritä riittävästi aikaa suorittamiseen käyttöönoton voimassaolon ajan. Noutaa mallin, mukaan lukien SAS tunnuksen koko URI.

        expiretime=$(date -I'minutes' --date "+30 minutes")
        fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")

2. Ota malli antamalla URI, joka sisältää SAS-tunnuksen.

        azure group deployment create --template-uri $fullurl -g ExampleResourceGroup

Katso esimerkki SAS tunnuksen käyttäminen linkitetyt mallit, [linkitetyn mallien Azure resurssien hallinnan avulla](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Seuraavat vaiheet
- Katso esimerkki käyttöönottoon resursseja .NET asiakkaan kirjaston kautta, [Ota käyttöön resurssien .NET-kirjastojen ja mallin avulla](virtual-machines/virtual-machines-windows-csharp-template.md).
- Parametrien määrittäminen malli-kohdassa [julkaisu-mallit](resource-group-authoring-templates.md#parameters).
- Ohjeita-ratkaisun käyttöönotto eri ympäristöissä on artikkelissa [kehitys ja testaa ympäristöissä Microsoft Azure-tietokannassa](solution-dev-test-environments.md).
- Lisätietoja KeyVault viittauksen avulla voit siirtää suojatun arvot on artikkelissa [siirtää suojatun arvot käyttöönoton aikana](resource-manager-keyvault-parameter.md).

