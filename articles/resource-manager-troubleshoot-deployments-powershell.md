<properties
   pageTitle="Näytä käyttöönoton toiminnot PowerShellin | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Resurssienhallinta käyttöönotosta virheiden havaitsemista Azure-PowerShellin avulla."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/14/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-powershell"></a>Näytä käyttöönoton toiminnot Azure PowerShellin avulla

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShellin](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure CLI](resource-manager-troubleshoot-deployments-cli.md)
- [REST-OHJELMOINTIRAJAPINNALLA](resource-manager-troubleshoot-deployments-rest.md)

Voit tarkastella toimintojen käyttöönottoa Azure-PowerShellin kautta. Voit tarkastella eniten tarkasteltaessa toimintoja, kun olet saanut virheen käyttöönoton aikana, jotta tässä artikkelissa kerrotaan toiminnoista, joita ei ole onnistunut tarkasteleminen. PowerShell tarjoaa cmdlet-komennot, joiden avulla voit helposti etsiä virheitä ja määrittää mahdolliset korjaukset.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

Voit välttää virheitä vahvistamalla malli ja infrastruktuurin ennen käyttöönottoa. Voit myös kirjautua muita pyynnön ja vastauksen tiedot, joista voi olla hyötyä myöhemmin vianmääritys käyttöönoton aikana. Tietoja vahvistaminen ja pyynnön ja vastauksen kirjaustiedot-kohdassa [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy.md).

## <a name="use-deployment-operations-to-troubleshoot"></a>Vianmääritys käyttöönoton toimintojen avulla

1. Saat käyttöönoton yleistä tilaa-komennolla **Get-AzureRmResourceGroupDeployment** . Voit suodattaa tulokset ainoastaan käyttöönotoissa, joka on epäonnistui.

        Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
        
    Joka palauttaa epäonnistui ominaisuuksissa seuraavanlainen:
        
        DeploymentName          : Microsoft.Template
        ResourceGroupName       : ExampleGroup
        ProvisioningState       : Failed
        Timestamp               : 6/14/2016 9:55:21 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
                    Name                Type                 Value
                    ===============     ===================  ==========
                    storageAccountName  String               tfstorage9855
                    adminUsername       String               tfadmin
                    adminPassword       SecureString
                    dnsNameforLBIP      String               dns
                    vmNamePrefix        String               myVM
                    imagePublisher      String               MicrosoftWindowsServer
                    imageOffer          String               WindowsServer
                    imageSKU            String               2012-R2-Datacenter
                    lbName              String               myLB
                    nicNamePrefix       String               nic
                    publicIPAddressName String               myPublicIP
                    vnetName            String               myVNET
                    vmSize              String               Standard_D1

        Outputs                 :
        DeploymentDebugLogLevel :

2. Kunkin käyttöönoton koostuu yleensä useita toimintoja, kunkin vaiheen käyttöönottoprosessin, joka edustaa toimintoa. Saat tietää, mikä meni vikaan ympäristössä, yleensä haluat nähdä lisätietoja käyttöönottotoimintoja. Näet tilan toimintoja, joiden **Get-AzureRmResourceGroupDeploymentOperation**.

        Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName Microsoft.Template
        
    Joka palauttaa useita toimintoja, joiden yksitellen seuraavanlainen:
        
        Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
        OperationId    : A3EB2DA598E0A780
        Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                         duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                         serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
        PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                         serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}

3. Saat lisätietoja epäonnistuneita toimintoja-noutaa ominaisuuksien toimintoja ja **epäonnistui** .

        (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
        
    Joka palauttaa kaikki epäonnistui toimintoja, joiden yksitellen seuraavanlainen:
        
        provisioningOperation : Create
        provisioningState     : Failed
        timestamp             : 2016-06-14T21:54:55.1468068Z
        duration              : PT3.1449887S
        trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
        serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
        statusCode            : BadRequest
        statusMessage         : @{error=}
        targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                                Microsoft.Network/publicIPAddresses/myPublicIP;
                                resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}

    Tarkista seuranta toimintoa varten. Käytetään, seuraavassa vaiheessa keskittyä tietyn toiminnon.

4. Jos haluat tietyn epäonnistui toiminnon tila-sanoma, käytä seuraava komento:

        ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
        
    Joka palauttaa:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}

## <a name="use-audit-logs-to-troubleshoot"></a>Käytä valvontalokien vianmääritys

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Virheet käyttöönottoa näkyviin noudattamalla seuraavia ohjeita:

1. Voit hakea lokimerkintöjä, **Hae AzureRmLog** -komennon suorittaminen. Voit palauttaa vain tapahtumia, jotka yhden resurssiryhmä epäonnistui **ResourceGroup** ja **tila** -parametrit. Jos et määritä alkamis- ja aika-merkintöjen edellisen tunnin palautetaan.
Jos esimerkiksi haluat hakea edellisen tunnin suorittaminen epäonnistui toimintoja:

        Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed

    Voit määrittää tietyn aikajakson. Seuraavassa esimerkissä näet epäonnistui toimintojen viimeisen päivän. 

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -Status Failed
      
    Tai voit määrittää tarkan alkamis- ja päättymisaika epäonnistui toiminnot:

        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00 -Status Failed

2. Jos tämä komento palauttaa liikaa tapahtumia ja ominaisuudet, voit sovittaa valvonnan tehokkuutta hakemalla **Ominaisuudet** -ominaisuus. Näet virhesanomat **DetailedOutput** -parametri sisältää myös.

        (Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-1) -DetailedOutput).Properties
        
    Joka palauttaa ominaisuudet lokimerkintöjä seuraavanlainen:
        
        Content
        -------
        {} 
        {[statusCode, BadRequest], [statusMessage, {"error":{"code":"DnsRecordInUse","message":"DNS record dns.westus.clouda...
        {[statusCode, BadRequest], [serviceRequestId, a426f689-5d5a-448d-a2f0-9784d14c900a], [statusMessage, {"error":{"code...

3. Toinen osa japanin näiden tulosten perusteella keskittyä. Voit tarkentaa tulokset katsomalla, merkinnän tila-sanoma.

        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput -StartTime (Get-Date).AddDays(-1)).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
        
    Joka palauttaa:
        
        code           message                                                                        details
        ----           -------                                                                        -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}



## <a name="next-steps"></a>Seuraavat vaiheet

- Ohjeita tietyn käyttöönoton virheiden ratkaisemisesta on artikkelissa [ratkaista yleisiä virheitä, kun resursseja Azure Azure resurssien hallinnan käyttöönotto](resource-manager-common-deployment-errors.md).
- Lisätietoja seurannassa muuntyyppisten toiminnot valvontalokien käyttämisestä on artikkelissa [valvonta toimintojen resurssien hallinta](resource-group-audit.md).
- Tarkista ennen sen suorittamista käyttöönoton, on artikkelissa [käyttöönotto resurssiryhmä Azure Resurssienhallinta-malli](resource-group-template-deploy.md).

