<properties
   pageTitle="Näytä käyttöönoton toiminnot REST-ohjelmointirajapinnalla | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Resurssienhallinta käyttöönotosta virheiden havaitsemista Azure Resurssienhallinta REST-Ohjelmointirajapinnalla avulla."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/13/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-resource-manager-rest-api"></a>Näytä käyttöönoton toiminnot Azure Resurssienhallinta REST-ohjelmointirajapinnalla

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShellin](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-manager-troubleshoot-deployments-rest.md)

Jos olet saanut Virhe otettaessa resurssien Azure, haluat ehkä lisätietoja käyttöönoton toiminnoista, joita on suoritettu. REST API on toimintoja, joiden avulla voit etsiä virheitä ja määrittää mahdolliset korjaukset.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Voit välttää virheitä vahvistamalla malli ja infrastruktuurin ennen käyttöönottoa. Voit myös kirjautua muita pyynnön ja vastauksen tiedot, joista voi olla hyötyä myöhemmin vianmääritys käyttöönoton aikana. Tietoja vahvistaminen ja pyynnön ja vastauksen kirjaustiedot-kohdassa [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy-rest.md).

## <a name="troubleshoot-with-rest-api"></a>REST-ohjelmointirajapinnalla vianmääritys

1. Ota käyttöön resurssien [mallin käyttöönottoa luominen](https://msdn.microsoft.com/library/azure/dn790564.aspx) -toiminnossa. Jos haluat säilyttää tiedot, joista voi olla hyötyä virheenkorjaus, JSON pyyntö **requestContent** ja/tai **responseContent** **debugSetting** -ominaisuus asetetaan. 

        PUT https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
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
              },
              "debugSetting": {
                "detailLevel": "requestContent, responseContent"
              }
            }
          }

    Oletusarvon mukaan **debugSetting** -arvo on määritetty **ei mitään**. Kun **debugSetting** -arvo, joka määrittää, mieti tietotyyppi on kulkeva käyttöönoton yhteydessä. Kirjaaminen tietoja pyynnön ja vastauksen, joita voi mahdollisesti Näytä luottamuksellisia tietoja, jotka noudetaan käyttöönotto-toimintojen avulla. 

2. Tietoja-ympäristö, jonka [mallin käyttöönottoa tietoja](https://msdn.microsoft.com/library/azure/dn790565.aspx) -toiminto.

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}

    Vastauksessa Huomaa erityisesti **provisioningState** , **correlationId** ja **Virhe** -osat. **CorrelationId** käytetään liittyvät tapahtumia ja voi olla hyötyä, kun käsittelet tekniseen tukeen, voit tehdä ongelman vianmäärityksen.
    
        { 
          ...
          "properties": {
            "provisioningState":"Failed",
            "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
            ...
            "error":{
              "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
              "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
            }  
          }
        }

3. Tietoja käyttöönoton toimintojen [luettelo kaikki mallin käyttöönoton toiminnot](https://msdn.microsoft.com/library/azure/dn790518.aspx) -toimintoa. 

        GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}

    Vastauksen sisällytetään pyynnön ja vastauksen tietojen perusteella määritetty **debugSetting** -ominaisuuden käyttöönoton aikana.
    
        {
          ...
          "properties": 
          {
            ...
            "request":{
              "content":{
                "location":"West US",
                "properties":{
                  "accountType": "Standard_LRS"
                }
              }
            },
            "response":{
              "content":{
                "error":{
                  "message":"Conflict","code":"Conflict"
                }
              }
            }
          }
        }

4. Pyydä tapahtumat valvontalokien [luettelon tilauksen hallinta-tapahtumat](https://msdn.microsoft.com/library/azure/dn931934.aspx) -toiminnossa käyttöönottoa varten.

        GET https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version={api-version}&$filter={filter-expression}&$select={comma-separated-property-names}


## <a name="next-steps"></a>Seuraavat vaiheet

- Ohjeita tietyn käyttöönoton virheiden ratkaisemisesta on artikkelissa [ratkaista yleisiä virheitä, kun resursseja Azure Azure resurssien hallinnan käyttöönotto](resource-manager-common-deployment-errors.md).
- Lisätietoja seurannassa muuntyyppisten toiminnot valvontalokien käyttämisestä on artikkelissa [valvonta toimintojen resurssien hallinta](resource-group-audit.md).
- Vahvista ennen sen suorittamista käyttöönoton, on artikkelissa [Ota resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy.md).
