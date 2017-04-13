<properties
    pageTitle="Web-sovelluksen käyttäminen MSDeploy isäntänimi ja ssl-varmenteen käyttöönotto"
    description="Web-sovelluksen käyttäminen MSDeploy ja mukautetun isäntänimi ja SSL-varmenteen määrittäminen käyttöönotto Azure Resurssienhallinta-mallin avulla"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Web-sovelluksen MSDeploy, mukautetut isäntänimi ja SSL-varmenteen käyttöönotto

Tämän oppaan käydään läpi luominen lopusta loppuun-käyttöönoton Azure Web App-sovelluksessa, hyödyntäminen MSDeploy sekä mukautetut isäntänimi ja SSL-varmenteen lisääminen ARM-malli.

Lisätietoja mallien luomisesta on artikkelissa [Yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Luo sovelluksen malli

Sisällytät ASP.NET web-sovelluksen. Ensimmäiseksi on luotava yksinkertainen web-sovelluksen (tai voi valita, haluatko käyttää aiemmin luodun - tällöin Ohita tämä vaihe).

Avaa Visual Studio 2015 ja valitse Tiedosto > uusi projekti. Valitse näkyviin tulevan valintaikkunan Web > ASP.NET Web-sovelluksen. Mallit-kohdassa Valitse verkko ja valitse MVC-malli. Valitse _Muuta todennustyyppi_ _Ei_todentamiseen. Tämä on vain, jos haluat tehdä sovelluksen malli mahdollisimman yksinkertaisena.

Tässä vaiheessa sinun on basic ASP.Net web App-sovelluksesta käyttövalmis käyttöönoton yhteydessä.

###<a name="create-msdeploy-package"></a>MSDeploy paketin luominen

Seuraavaksi voit luoda web-sovelluksen käyttöönotto Azure-paketti. Tällöin voit tallentaa projektin ja suorita sitten komentoriviltä seuraavasti:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Tämä luo pakatun paketin PackageLocation-ominaisuudessa-kansiossa. Sovellus on nyt valmis käyttöön, johon voit nyt luoda ulos Azure Resurssienhallinta-malli voit tehdä.

###<a name="create-arm-template"></a>ARM-mallin luominen
Ensin aloitetaan KÄDESSÄ perusmallin, joka luo verkkosovellus ja isännöintipalvelu sopimus (Huomaa, että parametrit ja muuttujat ei näy niitä).

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Seuraavaksi sinun on muokattava web app resurssin sisäkkäisiä MSDeploy resurssi. Näin voit viitata paketin aiemmin luotu ja kerro Azure Resurssienhallinta MSDeploy avulla voit asentaa pakkauksen Azure Web App-sovelluksen. Seuraavassa kuvassa näkyy Microsoft.Web/sites resurssin sisäkkäisiä MSDeploy resurssi:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Nyt huomaat, että MSDeploy resurssin kestää **packageUri** -ominaisuutta, jolla määritetään seuraavasti:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Tämä **packageUri** kestää tallennustilan tilin uri joka viittaa tallennustilan-tili, johon ladata paketti-zip avulla. Azure-Resurssienhallinta hyödyntää avattava paketin paikallisesti tallennustilan tililtä kun otat mallin [Jaettu Access allekirjoitukset](../storage/storage-dotnet-shared-access-signature-part-1.md) . Tämä toimenpide on automaattinen PowerShell komentosarja, joka paketin lataaminen ja luo näppäimet Azure hallinta-Ohjelmointirajapinnan kutsu kohteet ja välittää niitä malliin parametreiksi (*_artifactsLocation* ja *_artifactsLocationSasToken*) kautta. Sinun on kansion parametrien määrittäminen ja tiedostonimi paketti on ladattu kohdasta tallennustilan säilö.

Seuraava, sinun on lisättävä toinen sisäkkäisiä resurssi hostname sidontojen hyödyntää mukautetun toimialueen määrittäminen. Sinun on ensin varmistaaksesi, että omistat isäntänimi ja määrittämällä vahvistama Azure omistajuuden se - kohdassa [Configure Azure-sovelluksen palvelun mukautettua toimialuenimeä](web-sites-custom-domain-name.md). Sen jälkeen voit lisätä mallin Microsoft.Web/sites resurssi-osassa seuraavasti:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Lopuksi sinun on lisättävä uusi ylimmän tason resurssi, Microsoft.Web/certificates. Tämä sisältää SSL-varmenne, ja ne säilyvät samalla tasolla kuin online ja isännöintipalvelu suunnitelma.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

On pystyttävä muodostamaan kelvollinen SSL-varmenne, jotta voit määrittää tämän resurssin. Kun sinulla on kelvollinen sertifikaatin sinun on Pura pfx tavua base64 merkkijonona. Voit purkaa tätä yksi vaihtoehto on käyttää PowerShell-komentoa:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Onnistunut sitten välitetään tämä parametrina KÄDESSÄ käyttöönotto-malli.

Tässä vaiheessa ARM-malli on valmis.

###<a name="deploy-template"></a>Mallin ottaminen käyttöön

Lopullinen vaiheet ovat laitetta tämä ollenkaan koko lopusta loppuun-käyttöönoton kyselyjä. Voit helpottaa käyttöönoton voidaan hyödyntää **Käyttöönotto AzureResourceGroup.ps1** PowerShell-komentosarja, joka on lisätty, kun luot Azure resurssiryhmä-projektin Visual Studiossa, jossa on kaikki tarvittava mallin lataaminen. Se edellyttää, että olet luonut tallennustilan-tili, jota haluat käyttää etuajassa. Tässä esimerkissä luotu ladattava package.zip jaettua tallennustilan-tili. Komentosarja hyödyntää AzCopy paketin lataaminen tallennustilan tilin. Voit siirtää Palvelutietojen-kansiosijainti ja komentosarja automaattinen lataaminen kaikkiin tiedostoihin kansion nimetty tallennustilan säilö. Kutsumista käyttöönotto AzureResourceGroup.ps1 jälkeen sinulla Päivitä SSL-sidontojen yhdistämään mukautetun hostname kanssa SSL-varmenne.

Seuraavat PowerShell näkyy kutsumista Ota käyttöön koko käyttöönotto-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

Tässä vaiheessa sovelluksen pitäisi on otettu käyttöön ja pitäisi onnistua sen Siirry https://www.yourcustomdomain.com kautta
