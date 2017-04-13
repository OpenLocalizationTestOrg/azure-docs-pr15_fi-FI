<properties
   pageTitle="Resurssien REST API ja mallin käyttöön | Microsoft Azure"
   description="Ota käyttöön resurssi Azure Azure Resurssienhallinta ja Resurssienhallinta REST API avulla. Resursseja on määritetty Resurssienhallinta-mallissa."
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
   ms.date="07/11/2016"
   ms.author="tomfitz"/>

# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Ottaa käyttöön resursseja Resurssienhallinta mallit ja Resurssienhallinta REST-Ohjelmointirajapinnalla

> [AZURE.SELECTOR]
- [PowerShellin](resource-group-template-deploy.md)
- [Azure CLI](resource-group-template-deploy-cli.md)
- [Portal](resource-group-template-deploy-portal.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-group-template-deploy-rest.md)

Tässä artikkelissa kerrotaan, miten voit asentaa resurssien Azure Resurssienhallinta malleja Resurssienhallinta REST-Ohjelmointirajapinnalla avulla.  

> [AZURE.TIP] Saat apua virheen virheenkorjaus käyttöönoton aikana:
>
> - [Näytä käyttöönoton toiminnot REST-ohjelmointirajapinnalla](resource-manager-troubleshoot-deployments-rest.md) käytön tiedot, joiden avulla voit opetella virheiden vianmääritys
> - Yleisiä käyttöönoton virheiden ratkaisemisesta saat [vianmääritys yleisiä virheitä, kun resursseja Azure Azure resurssien hallinnan käyttöönotto](resource-manager-common-deployment-errors.md)

Mallin voi olla paikallisen tiedoston tai ulkoiseen tiedostoon, joka on käytettävissä URI-Osoitteen kautta. Kun malli sijaitsee tallennustilan tilin, voit rajoittaa malliin ja anna jaettu käyttöoikeustietue allekirjoitus (SAS) käyttöönoton aikana.

[AZURE.INCLUDE [resource-manager-deployments](../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Ottaa käyttöön REST-ohjelmointirajapinnalla
1. Määritä [yleisiä parametreja ja otsikot](https://msdn.microsoft.com/library/azure/8d088ecc-26eb-42e9-8acc-fe929ed33563#bk_common), mukaan lukien todennus tunnusten.
2. Jos sinulla ei ole olemassa resurssiryhmä, Luo resurssiryhmä. Anna tilauksen tunnus, nimi uusi resurssiryhmä ja sijainnin, jotka tarvitset ratkaisu. Lisätietoja on artikkelissa [luominen resurssiryhmä](https://msdn.microsoft.com/library/azure/dn790525.aspx).

        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
   
3. Vahvista käyttöönoton ennen kuin suoritat suorittamalla [Vahvista mallin käyttöönotto](https://msdn.microsoft.com/library/azure/dn790547.aspx) -toimintoa. Kun käyttöönoton testaaminen määritettävä parametrit samalla tavalla kuin yksittäiselle suoritettaessa käyttöönoton (kuten seuraavassa vaiheessa).

3. Luo käyttöönoton. Anna tilauksen tunnus resurssiryhmä käyttöön, käyttöönoton ja linkin nimiä mallin nimi. Lisätietoja mallitiedosto on artikkelissa [parametri tiedosto](#parameter-file). Lisätietoja luominen resurssiryhmä REST-Ohjelmointirajapinnalla artikkelissa [luominen mallin käyttöönottoa](https://msdn.microsoft.com/library/azure/dn790564.aspx). Ilmoitus **tila** on määritetty **vaiheittainen**. Voit suorittaa valmis käyttöönoton määrittämällä arvoksi **Valmis** **tila** . Ole varovainen, kun käytössä on valmis tila kuin voit poistaa vahingossa resurssit, jotka eivät ole mallin.
    
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0",
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0",
              }
            }
          }
   
      Jos haluat kirjautua vastauksen sisältöä, pyyntö sisältöä tai molempia, Sisällytä **debugSetting** pyynnön.

        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }

      Voit määrittää tilisi tallennustilan käyttämään jaettua käyttöä allekirjoituksen (SAS)-tunnuksen. Lisätietoja on artikkelissa [Delegoiminen käyttää ja jakaa Access-allekirjoitus](https://msdn.microsoft.com/library/ee395415.aspx).

4. Näyttää mallin käyttöönoton tilan. Lisätietoja on artikkelissa [mallin käyttöönottoa tietoja](https://msdn.microsoft.com/library/azure/dn790565.aspx).

          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

[AZURE.INCLUDE [resource-manager-parameter-file](../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Seuraavat vaiheet
- Katso esimerkki käyttöönottoon resursseja .NET asiakkaan kirjaston kautta, [Ota käyttöön resurssien .NET-kirjastojen ja mallin avulla](virtual-machines/virtual-machines-windows-csharp-template.md).
- Parametrien määrittäminen malli-kohdassa [julkaisu-mallit](resource-group-authoring-templates.md#parameters).
- Ohjeita-ratkaisun käyttöönotto eri ympäristöissä on artikkelissa [kehitys ja testaa ympäristöissä Microsoft Azure-tietokannassa](solution-dev-test-environments.md).
- Lisätietoja KeyVault viittauksen avulla voit siirtää suojatun arvot on artikkelissa [siirtää suojatun arvot käyttöönoton aikana](resource-manager-keyvault-parameter.md).
