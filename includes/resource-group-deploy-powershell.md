## <a name="how-to-deploy-with-powershell"></a>Voit ottaa käyttöön PowerShellin avulla

1. Kirjaudu Azure-tili.

          Add-AzureAccount

   Kun olet antanut tunnistetiedot-komento palauttaa tietoa tilistäsi.

          Id                             Type       ...
          --                             ----    
          example@contoso.com            User       ...   

2. Jos sinulla on useita tilauksia, antaa Tilaustunnus, jota haluat käyttää käyttöönottoa varten. 

          Select-AzureSubscription -SubscriptionID <YourSubscriptionId>

3. Siirry Azure Resurssienhallinta-moduuli.

          Switch-AzureMode AzureResourceManager

4. Jos sinulla ei ole olemassa resurssiryhmä, Luo uusi resurssiryhmä. Anna nimi resurssiryhmä ja sijainnin, jotka tarvitset ratkaisu.

        New-AzureResourceGroup -Name ExampleResourceGroup -Location "West US"

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

5. Jos haluat luoda uuden käyttöympäristön resurssiryhmän, **Uusi AzureResourceGroupDeployment** -komennon suorittaminen ja anna sitten tarvittavat parametrit. Parametrien sisällytetään käyttöönoton nimi, luomaasi mallia resurssiryhmän, polku tai URL-Osoitteen nimi ja muut tarvittavat käyttämässäsi skenaariossa parametrit. 
   
   Sinulla on antamisen parametriarvot seuraavista vaihtoehdoista: 
   
   - Tekstiin sidottu parametreilla.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -myParameterName "parameterValue"

   - Käytä parametrin objektia.

            $parameters = @{"<ParameterName>"="<Parameter Value>"}
            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterObject $parameters

   - Parametrin-tiedoston avulla.

            New-AzureResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate> -TemplateParameterFile <PathOrLinkToParameterFile>

  Kun resurssiryhmän on otettu käyttöön, näet käyttöönoton yhteenveto.

             DeploymentName    : ExampleDeployment
             ResourceGroupName : ExampleResourceGroup
             ProvisioningState : Succeeded
             Timestamp         : 4/14/2015 7:00:27 PM
             Mode              : Incremental
             ...

6. Saat lisätietoja käyttöönoton virheet.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed

7. Jos haluat lisätiedot käyttöönoton virheet.

        Get-AzureResourceGroupLog -ResourceGroup ExampleResourceGroup -Status Failed -DetailedOutput
