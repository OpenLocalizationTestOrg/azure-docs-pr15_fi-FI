
<properties
    pageTitle="Luettelo portit ja URL-osoitteet, whitelist Azure RemoteApp käyttöön asiakkaan virtual verkossa | Microsoft Azure"
    description="Katso, mitä portit ja URL-osoitteet tarvitset määrittämiseksi viestintä Azure RemoteApp kautta."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Portit ja URL-osoitteet sallia access Azure RemoteApp käyttöön asiakkaan VPN-luettelo 

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Seuraavat koskee Azure RemoteApp cloud tai hybrid sivustokokoelman Jos käyttöönoton virtual verkon (VNET). Lisätietoja virtual verkkojen lukea [Virtual yleistä](../virtual-network/virtual-networks-overview.md). Jos olet luonut rajoittaminen liikenne VPN resursseille, jotka olet valinnut Azure RemoteApp verkon käyttöoikeusryhmän (NSG), varmista, että seuraavat ovat helppokäyttöisiä ja sallittujen suojauskäytäntöjä virtual verkon kautta. Lisätietoja verkko-käyttöoikeusryhmät Lue [verkon käyttöoikeusryhmän ominaisuudet? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp aliverkon on oikeus käyttää näitä päätepisteet ja URL-osoitteet: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.NET  
*    Lähtevän: TCP: 443, TCP: 10101 10175 
*    Valinnainen – UDP: 10201 10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp asiakkaiden on oikeus käyttää näitä päätepisteet ja URL-osoitteet: 

Asiakkaiden voin tarkoittaa työpöytiä laitteiden jne ihmiset avulla muodostaa yhteyden käyttöön Azure RemoteApp-sivustokokoelman sovellukset.

-  https://telemetry.RemoteApp.windowsazure.com  
-  (valinnainen UDP-portit ovat tämän osoitteen) https://*.RemoteApp.windowsazure.com 
-  https://Login.Windows.NET  
-  https://Login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.NET  
-  Lähtevän: TCP: 443  
-  Valinnainen - UDP: 3391 