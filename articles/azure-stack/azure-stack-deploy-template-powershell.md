<properties
    pageTitle="PowerShellin Azure Pinotut mallit käyttöön | Microsoft Azure"
    description="Opi ottamaan virtual kone Resurssienhallinta mallin ja PowerShellin avulla."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Ottaa käyttöön mallien Azure Pinotut PowerShellin avulla

Azure Resurssienhallinta-mallit käyttöön Azure pinon Käsitteiden PowerShellin avulla.  Resurssienhallinta-mallit käyttöön ja varaa kaikki resurssit, sovelluksen yhteen, koordinoidun toiminnossa.

## <a name="run-azurerm-powershell-cmdlets"></a>Suorita AzureRM PowerShell cmdlet-komennot

Tässä esimerkissä suoritat komentosarjan virtual machine käyttöön Azure pinon Käsitteiden Resurssienhallinta mallin avulla.  Varmista ennen jatkamista, sinulla on [asentanut ja määrittänyt PowerShell](azure-stack-connect-powershell.md)  

Tässä esimerkissä mallissa käytetään Näennäiskiintolevyn on marketplace oletuskuvan (WindowsServer-2012-R2-palvelinkeskuksen).

1.  Siirry etsimällä <http://aka.ms/AzureStackGitHub> **101-yksinkertainen-windows-AM** malliin ja tallenna se seuraavaan sijaintiin: c:\\mallien\\azuredeploy 101-yksinkertainen-windows-vm.json.

2.  PowerShellin Suorita seuraava käyttöönoton komentosarja. Korvaa *käyttäjänimi* ja *salasana* käyttäjänimi ja salasana. Seuraavien käyttötarkoituksia kasvattaa estää korvaaminen käyttöönoton *$myNum* -parametrin.

    ```PowerShell
        # Set Deployment Variables
        $myNum = "001" #Modify this per deployment
        $RGName = "myRG$myNum"
        $myLocation = "local"

        # Create Resource Group for Template Deployment
        New-AzureRmResourceGroup -Name $RGName -Location $myLocation

        # Deploy Simple IaaS Template
        New-AzureRmResourceGroupDeployment `
            -Name myDeployment$myNum `
            -ResourceGroupName $RGName `
            -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
            -NewStorageAccountName mystorage$myNum `
            -DnsNameForPublicIP mydns$myNum `
            -AdminUsername <username> `
            -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
            -VmName myVM$myNum `
            -WindowsOSVersion 2012-R2-Datacenter
    ```

3.  Avaa Azure pino-portaali valitsemalla **Selaa**, valitse **näennäiskoneiden**ja Etsi uusi virtuaalikoneen (*myDeployment001*).

## <a name="video-example-hybrid-virtual-machine-deployment"></a>Videon Esimerkki: virtuaalikoneen yhdistelmäympäristö

[AZURE.VIDEO microsoft-azure-stack-tp1-poc-hybrid-vm-deployment]

## <a name="next-steps"></a>Seuraavat vaiheet

[Ottaa käyttöön Visual Studiossa mallit](azure-stack-deploy-template-visual-studio.md)
