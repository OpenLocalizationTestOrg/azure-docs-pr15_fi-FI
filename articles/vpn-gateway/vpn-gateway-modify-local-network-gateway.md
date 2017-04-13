<properties
   pageTitle="Muokkaa lähiverkon yhdyskäytävän IP-osoitteiden etuliitteiden ja yhdyskäytävän IP | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi IP-osoitteiden etuliitteiden paikalliseen verkkoon kirjauduttaessa Gatewayn muuttaminen"
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
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>Muokkaa lähiverkon yhdyskäytävän asetusten PowerShellin avulla

Joskus asetukset paikalliseen verkkoon kirjauduttaessa yhdyskäytävän AddressPrefix tai GatewayIPAddress muutoksen. Alla olevien ohjeiden avulla voit muokata paikalliseen verkkoon kirjauduttaessa yhdyskäytävän asetuksia. Voit myös muokata näitä asetuksia Azure-portaalissa.

## <a name="before-you-begin"></a>Ennen aloittamista
    
Tarvitset Azure Resurssienhallinta PowerShellin cmdlet-komennot uusimman version asentaminen. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.

## <a name="to-modify-ip-address-prefixes"></a>Jos haluat muokata IP-osoitteiden etuliitteiden

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Jos haluat muokata yhdyskäytävän IP-osoite

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Voit tarkistaa yhdyskäytävän yhteys. Katso [Tarkista yhdyskäytävän yhteys](vpn-gateway-verify-connection-resource-manager.md).

