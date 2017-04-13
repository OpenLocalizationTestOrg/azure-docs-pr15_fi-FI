<properties 
   pageTitle="Tietoja ExpressRoute VPN yhdyskäytävien | Microsoft Azure"
   description="Lisätietoja VPN yhdyskäytävien ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Tietoja VPN IP-osoitteiden ExpressRoute


Virtuaalinen yhdyskäytävien käytetään lähettämään verkkoliikennettä Azure virtual verkkojen välillä ja paikallisen sijainnit. Kun määrität ExpressRoute-yhteyden, luominen ja määrittäminen virtual yhdyskäytävien ja virtual verkkoyhteyden yhdyskäytävä.

Kun luot virtual yhdyskäytävien, Määritä useita asetuksia. Pakollinen asetus määrittää, käytetäänkö yhdyskäytävän ExpressRoute tai sivuston sivuston VPN tietoliikenteen. Resurssien hallinnan käyttöönotto-mallissa asetus on "-GatewayType".

Kun verkkoliikennettä lähetetään oma yksityinen-yhteyden, voit käyttää yhdyskäytävän tyyppi "ExpressRoute". Tätä kutsutaan myös ExpressRoute-yhdyskäytävä. Kun verkkoliikennettä lähetetään salattuja julkisen Internetissä, voit käyttää yhdyskäytävän tyyppi "Vpn". Tätä kutsutaan nimellä VPN-yhdyskäytävä. Sivuston sivusto-kohdan sivuston ja VNet VNet yhteydet kaikki käyttävät VPN-yhdyskäytävän. 

Kunkin virtual verkon voi olla vain yksi VPN-yhdyskäytävän yhdyskäytävän tyyppiä kohden. Esimerkiksi voi olla yksi virtual yhdyskäytävien, joka käyttää - GatewayType Vpn ja, joka käyttää - GatewayType ExpressRoute. Tässä artikkelissa keskitytään ExpressRoute VPN-yhdyskäytävä.

## <a name="gwsku"></a>Yhdyskäytävän tuotteissa

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Jos haluat päivittää käyttämäsi yhdyskäytävän tehokkaampia yhdyskäytävän tuote, voit useimmissa tapauksissa 'AzureRmVirtualNetworkGateway koon' PowerShell cmdlet-komennon. Tämä sovellu päivityksiä vakio- ja korkean tuotteissa. Kuitenkin päivittämiseksi UltraPerformance SKU tarvitset luovan yhdyskäytävän.

###  <a name="aggthroughput"></a>Arvioitu kooste siirtonopeuden yhdyskäytävän SKU mukaan


Seuraavassa taulukossa on yhdyskäytävän tyypit ja arvioidut kooste siirtonopeuden. Tässä taulukossa koskee Resurssienhallinta ja perinteinen käyttöönoton mallit.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>REST API ja PowerShell cmdlet-komennot

Tekniset lisäresursseja ja tietyn syntaksivaatimukset käytettäessä REST API ja PowerShellin cmdlet-komennot VPN-yhdyskäytävä-määrityksiä varten on artikkelissa seuraavilla sivuilla:

|**Perinteinen** | **Resurssien hallinta**|
|-----|----|
|[PowerShellin](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShellin](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST-OHJELMOINTIRAJAPINNALLA](https://msdn.microsoft.com/library/jj154113.aspx)|[REST-OHJELMOINTIRAJAPINNALLA](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja saatavilla yhteyden määrityksiä [ExpressRoute yleiskatsaus](expressroute-introduction.md) . 







 
