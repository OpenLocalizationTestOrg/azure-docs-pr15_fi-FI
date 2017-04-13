<properties
    pageTitle="Ottaa käyttöön AM, C# ja Resurssienhallinta-mallin avulla | Microsoft Azure"
    description="Opi ottamaan Azure-AM C# ja Resurssienhallinta mallin käyttäminen."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="davidmu"/>

# <a name="deploy-an-azure-virtual-machine-using-c-and-a-resource-manager-template"></a>Ottaa käyttöön Azure Virtual Machine C#- ja Resurssienhallinta mallia käyttämällä

Resurssiryhmiä ja malleja käyttämällä olet hallitsevan kaikkien resurssien ryhmitteleminen, jotka tukevat sovelluksen. Tässä artikkelissa kerrotaan, miten voit käyttää Visual Studio ja C# todentamisen määrittäminen, mallin luominen ja käyttöönotto Azure resurssien käyttämällä luomaasi mallia.

Sinun on ensin varmistaaksesi, että olet tehnyt asennuksen seuraavasti:

- Asenna [Visual Studio](http://msdn.microsoft.com/library/dd831853.aspx)
- [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) - tai [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855) asennuksen tarkistaminen
- Hae [todennus tunnus](../resource-group-authenticate-service-principal.md)
- Luo [PowerShellin Azure](../resource-group-template-deploy.md)sekä [Azure CLI](../resource-group-template-deploy-cli.md)ja [Azure portal](../resource-group-template-deploy-portal.md)resurssiryhmä.

Kestää noin 30 minuuttia, toimi seuraavien ohjeiden mukaisesti.
    
## <a name="step-1-create-the-visual-studio-project-the-template-file-and-the-parameters-file"></a>Vaihe 1: Visual Studio-projekti, mallitiedoston ja parametrit-tiedoston luominen

### <a name="create-the-template-file"></a>Mallitiedoston luominen

Azure Resurssienhallinta-malli mahdollistaa sellaisten ottaminen käyttöön ja hallita Azure resursseja yhdessä. Malli on JSON kuvaus resurssit ja liittyvän käyttöönoton parametrit.

Visual Studiossa toimi seuraavasti:

1. Valitse **Tiedosto** > **uuden** > **projektin**.

2. **Mallien** > **Visual C#**- **Console**-sovellus, nimen ja sijainnin projektin ja valitse sitten **OK**.

3. Napsauta ratkaisunhallinnassa projektin nimeä hiiren kakkospainikkeella, valitse **Lisää** > **Uusi kohde**.

4. Valitse sivusto, valitse JSON-tiedosto, kirjoita *VirtualMachineTemplate.json* nimi ja valitse sitten **Lisää**.

5. Avaaminen ja hakasulkeet VirtualMachineTemplate.json tiedoston Lisää tarvittavat rakenneosan ja tarvittavat contentVersion elementti:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
        }

6. [Parametrit](../resource-group-authoring-templates.md#parameters) eivät ole aina pakollisia, mutta niissä voi kirjoittaa arvoja, kun malli otetaan käyttöön. Lisää parametrit-elementin ja sen alielementit contentVersion elementin jälkeen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

7. [Muuttujia](../resource-group-authoring-templates.md#variables) voi käyttää mallissa, voit määrittää arvot, jotka saattavat muuttua usein tai arvoja, jotka on luotava parametriarvojen yhdistelmän. Lisää muuttujat osan jälkeen parametrit-osassa:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }

8. [Resurssit](../resource-group-authoring-templates.md#resources) , kuten virtuaalikoneen, virtual verkon ja tallennustilaa tili on määritetty seuraava mallissa. Lisää resursseja-osassa muuttujat-osan jälkeen:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvnet1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
      
9. Tallenna mallitiedosto, jonka loit.

### <a name="create-the-parameters-file"></a>Parametrit-tiedoston luominen

Jos haluat määrittää arvot, jotka on määritetty malli resurssin parametreille, voit luoda parametrit-tiedosto, joka sisältää arvot, joita käytetään, kun malli otetaan käyttöön. Visual Studiossa toimi seuraavasti:

1. Napsauta ratkaisunhallinnassa projektin nimeä hiiren kakkospainikkeella, valitse **Lisää** > **Uusi kohde**.

2. Valitse sivusto, valitse JSON-tiedosto, kirjoita *Parameters.json* nimi ja valitse sitten **Lisää**.

3. Avaa parameters.json-tiedosto ja sitten JSON sisältö:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Tässä artikkelissa Luo virtual kone Windows Server-käyttöjärjestelmän versio. Lisätietoja valitsemalla muut kuvat-kohdassa [Siirry ja valitse Azure virtuaalikoneen kuvat ja Windows PowerShellin Azure-CLI](virtual-machines-linux-cli-ps-findimage.md).

4. Tallenna parametrit-tiedosto, jonka loit.

## <a name="step-2-install-the-libraries"></a>Vaihe 2: Asenna kirjastot

NuGet paketit ovat helpoin tapa asentaa kirjastot, jotka haluat Tässä opetusohjelmassa loppuun. Tarvitset Azure resurssikirjasto hallinta ja Azure Active Directory käyttöoikeuksien kirjaston Luo resursseja. Jos haluat saada nämä kirjastot Visual Studiossa, toimi seuraavasti:

1. Projektin nimeä Solution Explorerissa hiiren kakkospainikkeella, valitse **NuGet pakettien hallinta**ja valitse sitten Selaa.

2. *Active Directory* -hakuruutuun valitsemalla **Asenna** Active Directory käyttöoikeuksien kirjaston paketin ja noudata asennusohjeita paketin.

4. Valitse **Sisällytä Prerelease**sivun yläreunassa. Haku-ruutuun tyyppi *Microsoft.Azure.Management.ResourceManager* valitsemalla **Asenna** Microsoft Azure resurssien hallinta kirjastojen ja noudata asennusohjeita paketin.

Nyt voit ryhtyä toimeen kirjastojen avulla voit luoda sovelluksen.

## <a name="step-3-create-the-credentials-that-are-used-to-authenticate-requests"></a>Vaihe 3: Luo tarkistamiseen pyynnöt käytettävät tunnistetiedot

Azure Active Directory-sovellus on luotu ja todennus-kirjasto on asennettuna. Nyt voit muotoilla tunnistetiedot, jotka pyynnöt Azure Resurssienhallinta tarkistamiseen käytettävän sovelluksen tiedot.

1. Avaa projekti, jonka loit Program.cs-tiedosto ja lisää ne lauseet tiedoston ylhäältä:

        using Microsoft.Azure;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.Azure.Management.ResourceManager.Models;
        using Microsoft.Rest;
        using System.IO;

2.  Lisää tämä menetelmä ohjelma luokan Saat tunnuksen, joka on tarvittavat tunnistetiedot luominen:

        private static async Task<AuthenticationResult> GetAccessTokenAsync()
        {
          var cc = new ClientCredential("{client-id}", "{client-secret}");
          var context = new AuthenticationContext("https://login.windows.net/{tenant-id}");
          var token = await context.AcquireTokenAsync("https://management.azure.com/", cc);
          if (token == null)
          {
            throw new InvalidOperationException("Could not get the token.");
          }
          return token;
        }

    {Asiakastunnus} korvaaminen Azure Active Directory tunnus sovelluksen, {asiakas-salaisuus} AD-sovelluksen ja {vuokraajan id} access-näppäimen kanssa tilauksen vuokraajan tunnuksessa on virhe. Löydät vuokraajan tunnus suorittamalla Get-AzureRmSubscription. Pikanäppäin voit etsiä Azure-portaalissa.

3. Luo tunnistetiedot, lisää koodi Program.cs tiedoston Main-menetelmää:

        var token = GetAccessTokenAsync();
        var credential = new TokenCredentials(token.Result.AccessToken);

4. Tallenna tiedosto Program.cs.

## <a name="step-4-deploy-the-template"></a>Vaihe 4: Käyttöönotto mallia

Tässä vaiheessa voit käyttää aiemmin luomasi resurssiryhmä, mutta voit myös luoda resurssiryhmä [ResourceGroup](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.models.resourcegroup.aspx) ja [ResourceManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx) luokkien avulla.

1. Lisää muuttujat aiemmin luomasi resurssien nimet, käyttöönoton nimi ja tilauksen tunnus-ohjelma-luokan Main-menetelmää:

        var groupName = "resource group name";
        var subscriptionId = "subsciption id";
        var deploymentName = "deployment name";

    Korvaa Ryhmän_nimi arvon resurssiryhmän nimi. Korvaa deploymentName arvon nimi, jota haluat käyttää käyttöönottoa varten. Löydät tilauksen tunnus suorittamalla Get-AzureRmSubscription.

2. Lisää ohjelma luokan ottamaan resursseja resurssiryhmän mallia, joka on määritetty käyttämällä tätä menetelmää:

        public static async Task<DeploymentExtended> CreateTemplateDeploymentAsync(
          TokenCredentials credential,
          string groupName,
          string deploymentName,
          string subscriptionId)
        {
          Console.WriteLine("Creating the template deployment...");
          var deployment = new Deployment();
          deployment.Properties = new DeploymentProperties
          {
            Mode = DeploymentMode.Incremental,
            Template = File.ReadAllText("..\\..\\VirtualMachineTemplate.json"),
            Parameters = File.ReadAllText("..\\..\\Parameters.json")
          };
          var resourceManagementClient = new ResourceManagementClient(credential) 
            { SubscriptionId = subscriptionId };
          return await resourceManagementClient.Deployments.CreateOrUpdateAsync(
            groupName,
            deploymentName,
            deployment);
        }

    Jos haluat ottaa tallennustilan tililtä mallia, voit korvata mallin ominaisuus TemplateLink-ominaisuus.

3. Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        var dpResult = CreateTemplateDeploymentAsync(
          credential,
          groupName,
          deploymentName,
          subscriptionId);
        Console.WriteLine(dpResult.Result.Properties.ProvisioningState);
        Console.ReadLine();

## <a name="step-5-delete-the-resources"></a>Vaihe 5: Poista resurssit

Perittävän Azure käytettävien resurssien, koska se on aina hyvä Poista resursseja, joita ei enää tarvita. Ei tarvitse poistaa kullekin resurssille erikseen resurssiryhmä. Poista resurssiryhmän ja kaikki sen resursseja poistetaan automaattisesti.

1.  Poista resurssiryhmän lisääminen ohjelman luokan tätä menetelmää:

        public static async void DeleteResourceGroupAsync(
          TokenCredentials credential,
          string groupName,
          string subscriptionId)
        {
          Console.WriteLine("Deleting resource group...");
          var resourceManagementClient = new ResourceManagementClient(credential)
            { SubscriptionId = subscriptionId };
          await resourceManagementClient.ResourceGroups.DeleteAsync(groupName);
        }

2.  Soita juuri lisäämäsi menetelmä lisää koodi Main-menetelmää:

        DeleteResourceGroupAsync(
          credential,
          groupName,
          subscriptionId);
        Console.ReadLine();

##<a name="step-6-run-the-console-application"></a>Vaihe 6: Console-sovelluksen käyttämiseen

1.  Console-sovelluksen käyttämiseen, valitse **Käynnistä** Visual Studiossa ja kirjaudu sitten sisään Azure AD samoilla tunnistetiedoilla, jota käytetään tilaus.

2.  Kun hyväksytty-tila näkyy, paina **Enter** -näppäintä.

    Olisi kestää noin viisi minuuttia konsolin sovelluksen suorittamiseen täysin alusta loppuun. Ennen kuin painat Enter aloittaa poistaminen resursseja, voi kestää muutaman minuutin kuluttua vahvistamiseksi luomisen resursseista Azure-portaalissa, ennen kuin poistat ne.

3. Resurssien tilan tarkasteleminen Siirry Azure-portaalissa valvontalokien:

    ![Selaa valvontalokien Azure-portaalissa](./media/virtual-machines-windows-csharp-template/crpportal.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Jos käyttöönoton ongelmat, seuraava vaihe on tarkistettava [vianmääritys resurssin ryhmän ominaisuuksissa Azure-portaalissa](../resource-manager-troubleshoot-deployments-portal.md)osoitteessa.
- Opi hallitsemaan virtuaalikoneen, jonka loit tarkastelemalla [hallinta näennäiskoneiden Azure Resurssienhallinta ja PowerShellin avulla](virtual-machines-windows-csharp-manage.md).
