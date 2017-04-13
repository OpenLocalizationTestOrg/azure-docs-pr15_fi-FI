### <a name="app-service-plan"></a>Sovelluksen palvelusopimus

Luo palvelusopimus isännöimiseen web Appissa. Voit nimetä suunnitelman **hostingPlanName** -parametrin kautta. Suunnitelman sijainti on käytettävä resurssiryhmän samaan sijaintiin. Hinnoittelu taso ja työntekijä koko on määritetty **tuote** - ja **workerSize** -parametrit

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

