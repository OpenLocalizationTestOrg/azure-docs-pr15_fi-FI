<properties
   pageTitle="Luo ja Määritä sovelluksen yhdyskäytävän sisäinen kuormituksen (ILB) ja Azure resurssien hallinnan avulla | Microsoft Azure"
   description="Tällä sivulla on ohjeita luominen, määrittäminen, aloittaminen ja poista sisäinen kuormituksen (ILB) Azure sovelluksen-Gatewaylle Azure Resurssienhallinta"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Luo sisäinen kuormituksen (ILB) sovelluksen-Gatewaylle Azure resurssien hallinnan avulla

> [AZURE.SELECTOR]
- [Azure perinteinen vaiheet](application-gateway-ilb.md)
- [Resurssienhallinta PowerShell-ohjeita](application-gateway-ilb-arm.md)

Azure sovelluksen yhdyskäytävän voi määrittää Internetiin yhteydessä oleva VIP tai sisäinen päätepisteen, joka on määritetty ei yhteyden Internetiin, eli sisäinen kuormituksen tasauspalvelun (ILB) päätepisteen kanssa. Yhdyskäytävän määrittäminen ILB on hyötyä sisäinen liiketoiminta-sovellukset, eikä niitä julkaista Internetissä. On myös hyödyntää-palveluissa ja tasojen monitasoisten-sovelluksesta, joka istut suojauksen reunaa, joka on määritetty ei Internet mutta edellyttävät edelleen pyöreän pöydän Lataa jakauman, istunnon tahmeutta tai Secure Sockets Layer (SSL)-tilauksen.

Tässä artikkelissa käydään läpi vaiheet ja määrittää yhdyskäytävän sovelluksen ILB.

## <a name="before-you-begin"></a>Ennen aloittamista

1. Asentaa uusimman version Azure PowerShell cmdlet-komentojen avulla WWW-ympäristö asennusohjelma. Voit ladata ja asenna uusin versio **Windows PowerShell** -osiosta, [Lataa sivu](https://azure.microsoft.com/downloads/).
2. Voit luoda virtuaalisen verkko- ja aliverkon sovelluksen Gatewayn. Varmista, että cloud ominaisuuksissa eikä näennäiskoneiden on käytössä aliverkon. VPN-osoitteiden on oltava sovelluksen yhdyskäytävän yksinään.
3. Palvelimet, joilla voit määrittää sovelluksen yhdyskäytävän käyttämään on oltava tai määrittänyt niiden päätepisteet luotu virtual verkossa tai julkiseen IP-tai VIP kanssa.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mikä on luotava sovelluksen yhdyskäytävän?


- **Taustatietokantaan palvelimen resurssivarantoon:** Taustatietokannan-palvelimien IP-osoitteiden luettelo. IP-osoitteiden luettelossa on kuuluttava virtual verkkoon, mutta erilaisia aliverkon sovelluksen yhdyskäytävän tai pitäisi olla julkiseen IP-tai VIP.
- **Taustatietokantaan resurssivarantoon palvelinasetukset:** Jokaisen varanto on asetusten, kuten portin, protokolla ja evästeiden perustuva affiniteetti. Nämä asetukset on liitetty resurssivarantoon, ja niitä käytetään varannon kaikki palvelimiin.
- **Edusta portti:** Tämä portti on Julkinen portti, joka on avattu sovelluksen yhdyskäytävän. Liikenne käynnit portin ja sitten uudelleenohjataan taustatietokantaan-palvelimia.
- **Listener:** Kuuntelun edusta portti-protokolla (Http tai Https-kirjainkoko on merkitsevä), ja SSL-varmenteen nimi (Jos määrittäminen SSL purku).
- **Säännön:** Säännön sitoo kuuntelua ja taustatietokantaan server-ryhmän ja määrittää, mitkä taustatietokantaan palvelimen resurssivarantoon liikenne olisi ohjautuu, kun se käynnit tietyn listener. *Tavallinen* sääntö on tällä hetkellä tueta. *Tavallinen* sääntö on pyöreän pöydän kuormituksen jakauman.



## <a name="create-an-application-gateway"></a>Sovelluksen Gatewayn luominen

Azure perinteinen ja Azure Resurssienhallinta välinen ero on siinä järjestyksessä, jossa luot sovelluksen yhdyskäytävän ja kohteista, jotka on määritettävä.
Resurssien hallinnan kaikki kohteet, jotka helpottavat sovelluksen yhdyskäytävä on määritetty erikseen ja ladata yhteen, kun haluat luoda sovelluksen yhdyskäytävän resurssia.


Seuraavassa on ohjeet, joita tarvitaan sovelluksen Gatewayn luominen:

1. Luoda Resurssiryhmän resurssien hallinta
2. Luo virtuaalisia verkko- ja sovelluksen yhdyskäytävän aliverkon
3. Luo sovelluksen yhdyskäytävän kokoonpano-objekti
4. Luo sovellus-yhdyskäytävän resurssi


## <a name="create-a-resource-group-for-resource-manager"></a>Luoda Resurssiryhmän resurssien hallinta

Varmista, että Siirry Azure resurssien hallinnan cmdlet-komentojen käyttäminen PowerShell-tilassa. Lisätietoja on saatavana [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Vaihe 1

    Login-AzureRmAccount

### <a name="step-2"></a>Vaihe 2

Tarkista tilaukset-tilin.

    Get-AzureRmSubscription

Sinua kehotetaan todentamismenetelmä tunnistetiedot.<BR>

### <a name="step-3"></a>Vaihe 3

Valitse, mitä Azure tilauksistasi käyttämään. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Vaihe 4

Luo uusi resurssiryhmä (ohita tämä vaihe, jos käytät aiemmin resurssiryhmä).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tämä käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Varmista, että kaikki komennot, voit luoda yhdyskäytävän sovellus käyttää samaa resurssiryhmä.

Yllä olevassa esimerkissä luomaasi nimeltä "appgw rg" ja "Länsi US" sijainnin resurssiryhmä.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Luo virtuaalisia verkko- ja sovelluksen yhdyskäytävän aliverkon

Seuraavassa esimerkissä, miten voit luoda virtuaalisen verkon Resurssienhallinta:

### <a name="step-1"></a>Vaihe 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Osoite-alueen 10.0.0.0/24 antaa aliverkon muuttujan voidaan luoda virtuaalisia verkkoon.

### <a name="step-2"></a>Vaihe 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

Tämä luo virtuaalisia verkon nimeltä "appgwvnet" resurssin ryhmän "appgw-rg" etuliite 10.0.0.0/16 käyttäminen aliverkon 10.0.0.0/24 Länsi US alue.

### <a name="step-3"></a>Vaihe 3

    $subnet = $vnet.subnets[0]

Tämä määrittää aliverkon objektin muuttujan $subnet seuraaviin vaiheisiin.

## <a name="create-an-application-gateway-configuration-object"></a>Luo sovelluksen yhdyskäytävän kokoonpano-objekti

### <a name="step-1"></a>Vaihe 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Tämä luo sovelluksen yhdyskäytävän IP määritys nimeltä "gatewayIP01". Sovelluksen yhdyskäytävän käynnistyessä vastataan aliverkosta, joka on määritetty IP-osoite ja reitittää verkkoliikenteen taustatietokantaan IP-ryhmän IP-osoitteisiin. Ota huomioon, että jokaiselle esiintymälle on yksi IP-osoite.


### <a name="step-2"></a>Vaihe 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Tämä määrittää taustatietokantaan IP-osoiteryhmän nimeltä "pool01" IP-osoitteita "134.170.185.46, 134.170.188.221,134.170.185.50". Siinä ovat IP-osoitteet, joka vastaanottaa joka edusta IP-päätepisteen tulee ‑sovelluksen verkkoliikenteen tarpeisiin. Voit korvata edellä voit lisätä oman sovelluksen IP-osoite päätepisteet IP-osoitteet.

### <a name="step-3"></a>Vaihe 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

Tämä määrittää sovelluksen yhdyskäytävän asetus "poolsetting01" kuormituksesta saapuva verkkoliikennettä taustatietokantaan sarjassa.

### <a name="step-4"></a>Vaihe 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

Tämä määrittää edusta ILB "frontendport01"-niminen IP-portin.

### <a name="step-5"></a>Vaihe 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

Tämä luo nimeltä "fipconfig01" edusta IP-määritys ja liittää sen yksityinen-IP nykyisen VPN-aliverkosta.

### <a name="step-6"></a>Vaihe 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

Tämä luo nimeltä "listener01" listener ja liittää edusta portin edusta IP-määritys.

### <a name="step-7"></a>Vaihe 7: ssä

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Tämä luo kuormituksen tasauspalvelun reititys säännön nimeltä "rule01", joka määrittää kuormituksen tasauspalvelun toiminnan.

### <a name="step-8"></a>Vaihe 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Tämä määrittää sovelluksen yhdyskäytävän esiintymän kokoa.

>[AZURE.NOTE]  *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Voit valita Standard_Small, Standard_Medium ja Standard_Large välillä.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Yhdyskäytävän sovelluksen luomalla uusi AzureApplicationGateway

Luo sovelluksen yhdyskäytävän määritysten kaikki kohteet edellä kuvatut vaiheet. Tässä esimerkissä sovelluksen yhdyskäytävän nimi on "appgwtest".


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

Tämä valinta luo sovelluksen yhdyskäytävän sisältää kaikki määritysten kohteet edellä kuvatut vaiheet. Esimerkissä sovelluksen yhdyskäytävän nimi on "appgwtest".


## <a name="delete-an-application-gateway"></a>Poista sovellus-yhdyskäytävä

Voit poistaa sovelluksen yhdyskäytävän, sinun on järjestyksessä seuraavasti:

1. **Lopeta AzureRmApplicationGateway** cmdlet-komennon avulla voit lopettaa yhdyskäytävän.
2. **Poista AzureRmApplicationGateway** cmdlet-komennon avulla voit poistaa yhdyskäytävän.
3. Varmista, että yhdyskäytävä on poistettu **Get-AzureApplicationGateway** cmdlet-komennolla.


### <a name="step-1"></a>Vaihe 1

Hae yhdyskäytävän sovellusobjektin ja liittää sen muuttujan "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Vaihe 2

**Lopeta AzureRmApplicationGateway** avulla voit lopettaa sovelluksen yhdyskäytävän. Tämä malli näkyy **Pysäytä AzureRmApplicationGateway** cmdlet-komento ensimmäisen rivin jäljessä tulos.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Kun sovellus yhdyskäytävä on pysäytetty-tilaan, **Poista AzureRmApplicationGateway** cmdlet-komennon avulla voit poistaa palvelun.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] **-Pakota** valitsimen avulla voidaan estää vahvistusviestissä Poista.


Jos haluat varmistaa, että palvelu on poistettu, voit käyttää **Get-AzureRmApplicationGateway** cmdlet-komento. Tämä vaihe ei tarvita.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat määrittää SSL purku-kohdassa [Configure sovelluksen yhdyskäytävän SSL purku](application-gateway-ssl.md).

Jos haluat määrittää sovelluksen-yhdyskäytävä ILB käytettäväksi, katso [luominen sovelluksen yhdyskäytävän sisäinen kuormituksen (ILB) kanssa](application-gateway-ilb.md).

Jos haluat lisätietoja ladata yleinen vastatilin asetukset-kohdassa:

- [Azure kuormituksen](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure liikenteen hallinta](https://azure.microsoft.com/documentation/services/traffic-manager/)
