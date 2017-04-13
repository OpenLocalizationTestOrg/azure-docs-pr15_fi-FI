<properties
   pageTitle="Tarkastele käyttöönottotoimintoja, joiden Azure CLI | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Resurssienhallinta käyttöönotosta virheiden havaitsemista Azure-CLI avulla."
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
   ms.date="08/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-cli"></a>Näytä käyttöönottotoimintoja, joiden Azure CLI

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShellin](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-manager-troubleshoot-deployments-rest.md)

Jos olet saanut Virhe otettaessa resurssien Azure, haluat ehkä lisätietoja käyttöönoton toiminnoista, joita on suoritettu. Azure CLI on komentoja, joiden avulla voit etsiä virheitä ja määrittää mahdolliset korjaukset.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Voit välttää virheitä vahvistamalla malli ja infrastruktuurin ennen käyttöönottoa. Voit myös kirjautua muita pyynnön ja vastauksen tiedot, joista voi olla hyötyä myöhemmin vianmääritys käyttöönoton aikana. Tietoja vahvistaminen ja pyynnön ja vastauksen kirjaustiedot-kohdassa [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy-cli.md).

## <a name="use-audit-logs-to-troubleshoot"></a>Käytä valvontalokien vianmääritys

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Virheet käyttöönottoa näkyviin noudattamalla seuraavia ohjeita:

1. Voit tarkastella valvontalokien suorittamalla **azure ryhmän log Näytä** -komento. Voit lisätä **--viimeksi käyttöönoton** vaihtoehto, jos haluat hakea vain viimeisimmän käyttöönoton log.

        azure group log show ExampleGroup --last-deployment

2. **Azure ryhmän log Näytä** -komento palauttaa tietoja. Vianmäärityksestä on yleensä haluat keskittyä toiminnoista, joita epäonnistui. Seuraavaa komentosarjaa käyttää Etsi käyttöönoton virheet lokista **--json** -asetus ja [jq](https://stedolan.github.io/jq/) JSON-apuohjelma.

        azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'
        
        {
        "claims": {
          "aud": "https://management.core.windows.net/",
          "iss": "https://sts.windows.net/<guid>/",
          "iat": "1442510510",
          "nbf": "1442510510",
          "exp": "1442514410",
          "ver": "1.0",
          "http://schemas.microsoft.com/identity/claims/tenantid": "{guid}",
          "http://schemas.microsoft.com/identity/claims/objectidentifier": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
          "puid": "XXXXXXXXXXXXXXXX",
          "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",
          "altsecid": "1:example.com:XXXXXXXXXXX",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string="">",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
          "name": "Tom FitzMacken",
          "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
          "groups": "{guid}",
          "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
          "wids": "{guid}",
          "appid": "{guid}",
          "appidacr": "0",
          "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
          "http://schemas.microsoft.com/claims/authnclassreference": "1",
          "ipaddr": "000.000.000.000"
        },
        "properties": {
          "statusCode": "Conflict",
          "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
            mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
            \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
        },
        ...

    Voit tarkastella **ominaisuuksien** tietoja json tietoja epäonnistunut toiminto.

    Voit käyttää **--yksityiskohtainen** ja **- vv** asetukset, saat lisätietoja lokitiedot.  Käytä **--yksityiskohtainen** asetus, kun haluat toimia käymällä läpi vaiheet `stdout`. Käytä koko pyyntö, historia **- vv** -vaihtoehto. Viestit on usein tärkeä kertoa kaikki virheet syy.

3. Keskittyminen epäonnistui merkintöjen tila-sanoma, käytä seuraavaa komentoa:

        azure group log show ExampleGroup --json | jq -r ".[] | select(.status.value == \"Failed\") | .properties.statusMessage"


## <a name="use-deployment-operations-to-troubleshoot"></a>Vianmääritys käyttöönoton toimintojen avulla

1. Hae-ympäristö, jonka **azure ryhmän käyttöönoton Näytä** -komento yleistä tilaa. Alla olevassa esimerkissä käyttöönotto epäonnistui.

        azure group deployment show --resource-group ExampleGroup --name ExampleDeployment
        
        info:    Executing command group deployment show
        + Getting deployments
        data:    DeploymentName     : ExampleDeployment
        data:    ResourceGroupName  : ExampleGroup
        data:    ProvisioningState  : Failed
        data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
        data:    Mode               : Incremental
        data:    Name             Type    Value
        data:    ---------------  ------  ------------
        data:    siteName         String  ExampleSite
        data:    hostingPlanName  String  ExamplePlan
        data:    siteLocation     String  West US
        data:    sku              String  Free
        data:    workerSize       String  0
        info:    group deployment show command OK

2. Jos näkyviin tulee sanoma, kun epäonnistuneita toimintoja käyttöönottoa varten, käytä:

        azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json  | jq ".[] | select(.properties.provisioningState == \"Failed\") | .properties.statusMessage.Message"


## <a name="next-steps"></a>Seuraavat vaiheet

- Ohjeita tietyn käyttöönoton virheiden ratkaisemisesta on artikkelissa [ratkaista yleisiä virheitä, kun resursseja Azure Azure resurssien hallinnan käyttöönotto](resource-manager-common-deployment-errors.md).
- Lisätietoja seurannassa muuntyyppisten toiminnot valvontalokien käyttämisestä on artikkelissa [valvonta toimintojen resurssien hallinta](resource-group-audit.md).
- Vahvista ennen sen suorittamista käyttöönoton, on artikkelissa [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy.md).
