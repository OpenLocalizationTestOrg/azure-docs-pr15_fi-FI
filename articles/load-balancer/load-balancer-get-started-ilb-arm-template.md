<properties
   pageTitle="Luo sisäinen kuormituksen, mallin resurssien hallinnan avulla | Microsoft Azure"
   description="Opettele luomaan sisäinen kuormituksen, mallin resurssien hallinnan avulla"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-using-a-template"></a>Luo sisäinen kuormituksen, mallin avulla

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]
<BR>
[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönottomalli](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Asentavat malli napsauttamalla käyttöönotto

Käytettävissä otoksen malli, julkisen säilössä käyttää parametrin tiedosto, joka sisältää arvot, joita käytetään edellä kuvatun skenaarion luomiseen oletusarvo. Voit ottaa tämän mallin avulla käyttöönotto, [tätä](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-internal-load-balancer)linkkiä napsauttamalla, valitse **käyttöönotto Azure**, korvaa parametrien oletusarvot tarvittaessa ja portaalin ohjeiden avulla.

## <a name="deploy-the-template-by-using-powershell"></a>Ottaa mallin käyttöön PowerShell-toiminnolla

Ottaa käyttöön PowerShell-toiminnolla lataamasi malli, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../../articles/powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.
2. Lataa parametrit-tiedosto paikalliselle kiintolevylle.
3. Muokkaa tiedostoa ja tallenna se.
4. Suorita **Uusi AzureRmResourceGroupDeployment** cmdlet-komento luominen mallin avulla resurssiryhmä.

        New-AzureRmResourceGroupDeployment -Name TestRG -Location westus `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json' `
            -TemplateParameterFile 'C:\temp\azuredeploy.parameters.json'

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Mallin asentavat Azure-CLI

Otetaan käyttöön mallin avulla Azure-CLI, noudata seuraavia ohjeita.

1. Jos et ole aikaisemmin käyttänyt Azure CLI-kohdassa [Asenna ja Määritä Azure-CLI](../../articles/xplat-cli-install.md) ja noudata ohjeita kohtaan, jossa valitaan Azure-tili ja tilauksen.
2. Resurssienhallinta-tilassa, siirry **azure config tila** -komennon suorittaminen alla kuvatulla tavalla.

        azure config mode arm

    Näin Odotettu tulos yllä-komennon:

        info:    New mode is arm

3. Parametrin-tiedoston avaaminen, valitse sen sisältö ja tallenna se tietokoneeseen tiedoston. Tässä esimerkissä on tallennettu parametrit-tiedoston *parameters.json*.

4. Suorittamalla **azure ryhmän käyttöönoton luominen** -komento uusi sisäinen kuormituksen asentavat mallin ja parametri tiedostoja ladataan ja muokata yläpuolella. Luettelon, kun tulos kerrotaan käytetyt parametrit.

        azure group create --name TestRG --location westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-internal-load-balancer/azuredeploy.json --parameters-file parameters.json

## <a name="next-steps"></a>Seuraavat vaiheet

[Lähde-IP-affiniteetti käyttämällä kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)



