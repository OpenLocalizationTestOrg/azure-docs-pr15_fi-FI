<properties
   pageTitle="Lisääminen VNet yhdyskäytävän virtual verkkoon ExpressRoute Resurssienhallinta ja PowerShellin avulla | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi lisääminen luotu Resurssienhallinta-VNet ExpressRoute Vnet yhdyskäytävän"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Virtual yhdyskäytävien määrittäminen ExpressRoute Resurssienhallinta ja PowerShellin avulla


> [AZURE.SELECTOR]
- [PowerShell - resurssien hallinta](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell – perinteinen](expressroute-howto-add-gateway-classic.md)


Tässä artikkelissa opastaa vaiheet, kokoa ja Poista olemassa VNet virtual yhdyskäytävien (VNet). Ohjeita määritysten ovat varten VNets, jotka on luotu käyttämällä **resurssien hallinnan käyttöönottomalli** ja joka on käytettävä ExpressRoute määrityksessä. 

**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Ennen kuin aloitat

Tarkista, että olet asentanut tarvittavat määritysten Azure PowerShellin cmdlet-komennot (1.0.2 tai uudempi). Jos et ole vielä asentanut Cmdlet-komentoja, tarvitset kiellä ennen aloittamista määritysvaiheet. Lisätietoja PowerShellin Azure-asennuksen Katso, [miten asennetaan ja määritetään PowerShellin Azure](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut VNet yhdyskäytävän, voit linkittää oman VNet ExpressRoute piiri. Katso [linkittäminen ExpressRoute piiri Virtual verkon](expressroute-howto-linkvnet-arm.md).
