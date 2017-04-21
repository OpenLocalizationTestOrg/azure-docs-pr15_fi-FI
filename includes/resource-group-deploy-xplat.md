## <a name="how-to-deploy-with-azure-cli"></a>Azure CLI käyttöönotosta

1. Kirjaudu Azure-tili.

        azure login

  Kun olet antanut tunnistetiedot-komento palauttaa kirjautumisnimi.

        ...
        info:    login command OK

2. Jos sinulla on useita tilauksia, antaa Tilaustunnus, jota haluat käyttää käyttöönottoa varten.

        azure account set <YourSubscriptionNameOrId>

3. Siirry Azure Resurssienhallinta-moduuli

        azure config mode arm

   Saat vahvistus uusi tila.

        info:     New mode is arm

4. Jos sinulla ei ole olemassa resurssiryhmä, Luo uusi resurssiryhmä. Anna nimi resurssiryhmä ja sijainnin, jotka tarvitset ratkaisu.

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

5. Jos haluat luoda uuden käyttöympäristön resurssiryhmän, suorita seuraava komento ja anna tarvittavat parametrit. Parametrien sisällytetään käyttöönoton nimi, luomaasi mallia resurssiryhmän, polku tai URL-Osoitteen nimi ja muut tarvittavat käyttämässäsi skenaariossa parametrit.

   Sinulla on antamisen parametriarvot seuraavista vaihtoehdoista:

   - Parametrien käyttäminen tekstiin ja paikallisen mallina.

             azure group deployment create -f <PathToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Parametrien käyttäminen tekstiin ja linkin lisääminen malliin.

             azure group deployment create --template-uri <LinkToTemplate> {"ParameterName":"ParameterValue"} -g ExampleResourceGroup -n ExampleDeployment

   - Käytä parametri-tiedosto.

             azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g ExampleResourceGroup -n ExampleDeployment

  Kun resurssiryhmän on otettu käyttöön, näet käyttöönoton yhteenveto.

         info:    Executing command group deployment create
         + Initializing template configurations and parameters
         + Creating a deployment
         ...
         info:    group deployment create command OK


6. Saat lisätietoja uusimman käyttöönotto.

         azure group log show -l ExampleResourceGroup

7. Jos haluat lisätiedot käyttöönoton virheet.

         azure group log show -l -v ExampleResourceGroup
