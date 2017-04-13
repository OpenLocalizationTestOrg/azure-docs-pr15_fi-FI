<properties
   pageTitle="Resurssien PowerShellistä ja mallin käyttöön | Microsoft Azure"
   description="Resurssi käyttöön Azure Azure Resurssienhallinta ja Azure PowerShellin avulla. Resursseja on määritetty Resurssienhallinta-mallissa."
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

# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Resurssien Resurssienhallinta mallit ja PowerShellin Azure ottaminen käyttöön

> [AZURE.SELECTOR]
- [PowerShellin](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-group-template-deploy-rest.md)

Tässä ohjeaiheessa kerrotaan, miten voit ottaa resurssien Azure Resurssienhallinta malleja Azure PowerShellin avulla.  

> [AZURE.TIP] Saat apua virheen virheenkorjaus käyttöönoton aikana:
>
> - [Näytä käyttöönottotoimintoja, joiden PowerShellin Azure](resource-manager-troubleshoot-deployments-powershell.md) käytön tiedot, joiden avulla voit opetella virheiden vianmääritys
> - Yleisiä käyttöönoton virheiden ratkaisemisesta saat [vianmääritys yleisiä virheitä, kun resursseja Azure Azure resurssien hallinnan käyttöönotto](resource-manager-common-deployment-errors.md)

Mallin voi olla paikallisen tiedoston tai ulkoiseen tiedostoon, joka on käytettävissä URI-Osoitteen kautta. Kun malli sijaitsee tallennustilan tilin, voit rajoittaa malliin ja anna jaettu käyttöoikeustietue allekirjoitus (SAS) käyttöönoton aikana.

## <a name="quick-steps-to-deployment"></a>Vaiheittaiset ohjeet käyttöönottoon

Tässä artikkelissa kuvataan kaikki vaihtoehdot käytettävissä käyttöönoton aikana. Kuitenkin usein tarvitset vain yksinkertaisia komentoja. Aloita nopeasti käyttöönoton kanssa, käytä seuraavia komentoja:

    New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
    New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

Lisätietoja asetukset, jotka saattavat olla paremmin sopivia käyttämässäsi skenaariossa käyttöönottoa varten, Jatka lukemista tässä artikkelissa.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-powershell"></a>Ottaa käyttöön PowerShellin avulla

1. Kirjaudu sisään Azure-tili.

        Add-AzureRmAccount

     Tilin yhteenveto palautetaan.

        Environment : AzureCloud
        Account     : someone@example.com
        ...

2. Jos sinulla on useita tilauksia, antaa Tilaustunnus, jota haluat käyttää käyttöönoton **Määrittäminen AzureRmContext** -komennolla. 

        Set-AzureRmContext -SubscriptionID <YourSubscriptionId>

3. Yleensä käyttöönotto mallina, kun haluat luoda sisältämään resursseja resurssiryhmän. Jos sinulla on aiemmin resurssiryhmä, johon haluat ottaa käyttöön, voit ohittaa tämän vaiheen ja käyttää kyseisen resurssiryhmä. 

     Luodaksesi resurssiryhmä, Anna nimi ja sijainti resurssiryhmän. Sinun on annettava resurssiryhmän sijainti, koska tietoja resursseja resurssiryhmän metatiedot. Yhteensopivuuden vuoksi haluat ehkä määrittää, että metatietojen tallennuspaikan. Suosittelemme, Määritä sijainti, johon useimmat resurssien tallennetaan. Samaan sijaintiin avulla voidaan yksinkertaistaa mallin.

        New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   
     Uusi resurssiryhmä yhteenveto palautetaan.
   
        ResourceGroupName : ExampleResourceGroup
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
             Actions  NotActions
             =======  ==========
             *
        ResourceId        : /subscriptions/######/resourceGroups/ExampleResourceGroup

4. Ennen kuin suoritat käyttöönoton, voit tarkistaa Käyttöönottoasetukset. **Testaa AzureRmResourceGroupDeployment** cmdlet-komennon avulla voit ongelmien etsiminen ennen kuin luot todellisilla resursseilla. Seuraavassa esimerkissä esitetään, miten voit tarkistaa käyttöönoton.

        Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

5. Ottamaan resursseja resurssiryhmän **Uusi AzureRmResourceGroupDeployment** -komennon suorittaminen ja anna sitten tarvittavat parametrit. Parametrit ovat käyttöönoton nimi, luomaasi mallia resurssiryhmän, polku tai URL-Osoitteen nimi ja muut tarvittavat käyttämässäsi skenaariossa parametrit. Jos **tila** -parametria ei ole määritetty, käytetään **vaiheittainen** oletusarvon. Voit suorittaa valmis käyttöönoton määrittämällä arvoksi **Valmis** **tila** . Ole varovainen, kun käytössä on valmis tila kuin voit poistaa vahingossa resurssit, jotka eivät ole mallin.

     Paikallinen mallin ottamaan Käytä **TemplateFile** -parametri:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>

     Ottaa käyttöön ulkoisen mallin, käytä **TemplateUri** parametri:

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate>
   
     Sinulla on antamisen parametriarvot seuraavista vaihtoehdoista: 
   
     1. Tekstiin sidottu parametreilla.

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"

     2. Käytä parametrin objektia.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters

     3. Käytä paikallisen parametri-tiedosto. Lisätietoja mallitiedosto on artikkelissa [parametri tiedosto](#parameter-file).

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile <PathToParameterFile>

     4. Käytä ulkoisia parametri-tiedosto. Lisätietoja mallitiedosto on artikkelissa [parametri tiedosto](#parameter-file). 

            New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri <LinkToParameterFile>

        Kun käytät ulkoisia parametri-tiedosto, ei voi välittää muita arvoja joko tekstiin tai paikallinen tiedosto. Lisätietoja on artikkelissa [parametrin järjestys](#parameter-precendence).

     Kun resursseja on otettu käyttöön, näet käyttöönoton yhteenveto.

        DeploymentName    : ExampleDeployment
        ResourceGroupName : ExampleResourceGroup
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2015 7:00:27 PM
        Mode              : Incremental
        ...

     Jos mallin on sama nimi kuin jokin parametri PowerShell-komentoa, ohjelma pyytää antamaan parametrin arvon. Parametrin mallilla sisällytetään postfix **FromTemplate**. Parametrin nimi esimerkiksi **ResourceGroupName** malli-ristiriitoja [Uusi AzureRmResourceGroupDeployment](https://msdn.microsoft.com/library/azure/mt679003.aspx) cmdlet-komento **ResourceGroupName** -parametrin. Sinua kehotetaan arvo antaa **ResourceGroupNameFromTemplate**. Yleensä kannattaa välttää tämän sekaannusta nimeämällä parametrit ei ole samannimistä parametreiksi käytettävien toimintojen käyttöönotto.

6. Jos haluat lisätietoja, jotka voivat auttaa sinua vianmääritys käyttöönoton virheitä, käytä **DeploymentDebugLogLevel** -parametrin käyttöönoton Kirjaudu. Voit määrittää, että sisällön pyynnön ja vastauksen sisällön on kirjautunut käyttöönoton-toimintoa.

        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
        
     Saat lisätietoja käyttäminen muistin sisällön käyttöönoton ongelmien [vianmääritys resurssin ryhmän ominaisuuksissa PowerShellin Azure](resource-manager-troubleshoot-deployments-powershell.md).

## <a name="deploy-template-from-storage-with-sas-token"></a>SAS tunnuksen ja tallennustilaa mallin ottaminen käyttöön

Voit lisätä mallisi tallennustilan tilin ja linkittää ne SAS tunnuksen käyttöönoton aikana.

> [AZURE.IMPORTANT] Noudattamalla seuraavia ohjeita blob, jossa malli on käytettävissä vain tilin omistajan. Kuitenkin luodessasi SAS-tunnuksen, blob Blob-objektien voivat käyttää kaikki, URI. Jos toisen käyttäjän sieppaa URI-käyttäjä on käyttää mallia. SAS tunnusta on hyvä keino mallisi pääsyä, mutta älä sisällytä luottamuksellisia tietoja, kuten salasanat suoraan mallissa.

### <a name="add-private-template-to-storage-account"></a>Lisää tallennustilan tilin yksityinen-malli

Tallennustilan tilin mallien seuraavasti:

1. Luo resurssiryhmä.

        New-AzureRmResourceGroup -Name ManageGroup -Location "West US"

2. Tallennustilan tilin luominen. Tallennustilan tilin nimi on oltava yksilöivä Azure, anna niin oman tilin nimi.

        New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"

3. Määritä nykyinen tallennustilan-tili.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

4. Voit luoda säilön. Käyttöoikeudet on määritetty **käytöstä** toisin sanoen säilö on ainoastaan niiden omistaja.

        New-AzureStorageContainer -Name templates -Permission Off
        
5. Mallin lisääminen säilöön.

        Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
        
### <a name="provide-sas-token-during-deployment"></a>Anna SAS tunnuksen käyttöönoton aikana

Ottamaan yksityinen mallin tallennustilan tilillä noutaa SAS tunnuksen ja Sisällytä se mallin URI.

1. Jos olet muuttanut nykyistä tallennustilan tilin, Määritä nykyinen tallennustilan-tili, joka sisältää malleja.

        Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates

2. Luo SAS tunnuksen, jonka lukuoikeudet ja käyttörajoitukset voimassaolon ajan. Noutaa mallin, mukaan lukien SAS tunnuksen koko URI.

        $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

3. Ota malli antamalla URI, joka sisältää SAS-tunnuksen.

        New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri

Katso esimerkki SAS tunnuksen käyttäminen linkitetyt mallit, [linkitetyn mallien Azure resurssien hallinnan avulla](resource-group-linked-templates.md).

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="parameter-precedence"></a>Parametrin järjestys

Voit käyttää sisäisiä parametrit ja paikallinen parametri tiedoston samassa käyttöönoton-toiminnossa. Voit esimerkiksi määrittää paikallisen parametri tiedosto joitakin arvoja ja lisätä muita arvoja tekstiin käyttöönoton aikana. Jos annat paikallisen parametri tiedosto ja riveillä parametrin arvot, on tekstiin oletusarvoon nähden.

Ei voi kuitenkin käyttää sisäisiä parametrit ulkoisen parametri-tiedoston. Jos määrität parametrin tiedoston **TemplateParameterUri** -parametri, kaikki tekstiin parametrit ohitetaan. Sinun on määritettävä ulkoisen tiedoston kaikkien parametriarvot. Jos malli sisältää arkaluonteisia arvon, joka ei voi sisällyttää parametri-tiedosto, lisää kyseisen arvon avaimen säilö ja viitata avaimen säilö ulkoisen parametri-tiedostossa, tai antaa dynaamisesti kaikki parametrin arvot riveillä.

Lisätietoja KeyVault viittauksen avulla voit siirtää suojatun arvot on artikkelissa [siirtää suojatun arvot käyttöönoton aikana](resource-manager-keyvault-parameter.md).

## <a name="next-steps"></a>Seuraavat vaiheet
- Katso esimerkki käyttöönottoon resursseja .NET asiakkaan kirjaston kautta, [Ota käyttöön resurssien .NET-kirjastojen ja mallin avulla](virtual-machines/virtual-machines-windows-csharp-template.md).
- Parametrien määrittäminen malli-kohdassa [julkaisu-mallit](resource-group-authoring-templates.md#parameters).
- Ohjeita-ratkaisun käyttöönotto eri ympäristöissä on artikkelissa [kehitys ja testaa ympäristöissä Microsoft Azure-tietokannassa](solution-dev-test-environments.md).

