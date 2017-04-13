<properties
   pageTitle="Tarkista yhdyskäytävän yhteys | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten vahvistamiseksi yhdyskäytävän yhteys resurssien hallinnan käyttöönottomalli"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="verify-a-gateway-connection"></a>Tarkista yhdyskäytävän yhteys

Voit tarkistaa yhdyskäytävän yhteys muutamalla eri tavalla. Tässä artikkelissa kerrotaan Resurssienhallinta yhdyskäytävän yhteyden tilan tarkistaminen Azure-portaalissa ja PowerShell-toiminnolla.


## <a name="verify-using-powershell"></a>Tarkista PowerShellin avulla

Tarvitset Azure Resurssienhallinta PowerShellin cmdlet-komennot uusimman version asentaminen. Lisätietoja asentamisesta PowerShellin cmdlet-komennot Katso, [miten asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md). Lisätietoja resurssien hallinnan cmdlet-komentojen käyttämisestä on artikkelissa [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).

### <a name="step-1-log-in-to-your-azure-account"></a>Vaihe 1: Kirjaudu sisään Azure-tili

1. Avaa PowerShell-konsolin oikeuksin ja muodostaa yhteyden tiliisi.

        Login-AzureRmAccount

2. Tarkista tilaukset-tilin.

        Get-AzureRmSubscription 

3. Määritä tilaus, jota haluat käyttää.

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

### <a name="step-2-verify-your-connection"></a>Vaihe 2: Tarkista yhteys


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="verify-using-the-azure-portal"></a>Tarkista Azure-portaalissa

[AZURE.INCLUDE [verify connection portal](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)] 


## <a name="next-steps"></a>Seuraavat vaiheet

- Voit lisätä näennäiskoneiden virtual verkkoihin. Lisätietoja on kohdassa [Create Virtual Machine](../virtual-machines/virtual-machines-windows-hero-tutorial.md) ohjeita.

