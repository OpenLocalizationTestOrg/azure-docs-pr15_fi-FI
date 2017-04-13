<properties
   pageTitle="Luo, Käynnistä tai poistaminen sovelluksen yhdyskäytävän Azure resurssien hallinnan avulla | Microsoft Azure"
   description="Tällä sivulla on ohjeita luominen, määrittäminen, aloittaminen ja poistaa Azure sovelluksen yhdyskäytävän Azure resurssien hallinnan avulla"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Luo, Käynnistä tai poistaminen sovelluksen yhdyskäytävän Azure resurssien hallinnan avulla

> [AZURE.SELECTOR]
- [Azure Portal](application-gateway-create-gateway-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-gateway-arm.md)
- [Azure perinteinen PowerShell](application-gateway-create-gateway.md)
- [Azure Resurssienhallinta-malli](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure sovelluksen yhdyskäytävä on kerroksen 7 kuormituksen. Se sisältää automaattisesti, suorituskyvyn reititys-pyyntöjen eri palvelinten välillä, olivatpa ne sitten cloud tai paikallisen. Sovelluksen yhdyskäytävän sisältää useita sovelluksen toimituksen ohjauskoneen (ADC) mukaan lukien HTTP kuormituksen tasaamisen, eväste perustuva istunnon affiniteetti, Secure Sockets Layer (SSL) purku mukautetun kunto keräysputkien usean sivuston tuki ja monista muista. Luettelo kaikista tuetut ominaisuudet etsimällä käy [Sovelluksen Gateway yleiskatsaus](application-gateway-introduction.md)

Tässä artikkelissa käydään läpi vaiheet luominen, määrittäminen, aloittaminen ja poista sovellus-yhdyskäytävä.

>[AZURE.IMPORTANT] Ennen kuin voit käsitellä Azure resursseja, on tärkeää ymmärtää, että Azure on tällä hetkellä kaksi käyttöönoton mallit: Resurssienhallinta ja perinteinen. Varmista, että ymmärrät [käyttöönotto-malleja ja työkalut](../azure-classic-rm.md) ennen minkä tahansa Azure resurssien käsitteleminen. Voit tarkastella työkaluja ohjeissa napsauttamalla ohjeaiheen yläreunassa olevia välilehtiä. Tässä asiakirjassa kerrotaan sovelluksen yhdyskäytävän luominen Azure Resurssienhallinta. Siirry käyttämään perinteistä versiota [Luo sovellusten käyttöönoton yhdyskäytävän PowerShell-toiminnolla perinteinen](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Ennen aloittamista

1. WWW-ympäristö asennusohjelma Azure PowerShellin cmdlet-komennot uusimman version asentamalla. Voit ladata ja asenna uusin versio **Windows PowerShell** -osiosta, [Lataa sivu](https://azure.microsoft.com/downloads/).
2. Jos sinulla on aiemmin virtual verkkoon, valitse aiemmin luotu tyhjä aliverkon tai aliverkon luominen olemassa olevan virtual verkoston ainoastaan sovelluksen yhdyskäytävän. Eri virtual verkkoon kuin aiot ottaa käyttöön sovelluksen yhdyskäytävän takana resurssien sovelluksen yhdyskäytävä ei voi ottaa käyttöön.
3. Palvelimet, joilla voit määrittää sovelluksen yhdyskäytävän käyttämään on oltava tai määrittänyt niiden päätepisteet luotu virtual verkossa tai julkiseen IP-tai VIP kanssa.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mikä on luotava sovelluksen gateway?

- **Taustatietokantaan palvelimen resurssivarantoon:** Taustatietokannan palvelimien IP-osoitteiden luettelo. IP-osoitteiden luettelossa tulee kuulua joko VPN-aliverkon tai pitäisi olla julkiseen IP-tai VIP.
- **Taustatietokantaan resurssivarantoon palvelinasetukset:** Jokaisen varanto on asetusten, kuten portin, protokolla ja evästeiden perustuva affiniteetti. Nämä asetukset on liitetty resurssivarantoon, ja niitä käytetään varannon kaikki palvelimiin.
- **Edusta portti:** Tämä portti on Julkinen portti, joka on avattu sovelluksen yhdyskäytävän. Liikenne käynnit portin ja sitten uudelleenohjataan taustatietokantaan-palvelimia.
- **Listener:** Kuuntelun edusta portti-protokolla (Http tai Https-arvot ovat kirjainkoko on merkitsevä), ja SSL-varmenteen nimi (Jos määrittäminen SSL purku).
- **Säännön:** Säännön sitoo listener taustatietokantaan server-ryhmän ja määrittää, mitkä taustatietokantaan palvelimen resurssivarantoon liikenne olisi ohjautuu, kun se käynnit tietyn listener.

## <a name="create-an-application-gateway"></a>Sovelluksen Gatewayn luominen

Azure perinteinen ja Azure Resurssienhallinta välinen ero on siinä järjestyksessä, jossa luot sovelluksen yhdyskäytävän ja kohteista, jotka on määritettävä.

Resurssien hallinnan kaikki kohteet, jotka helpottavat sovelluksen gateway on määritetty erikseen ja ladata yhteen, kun haluat luoda sovelluksen gateway resurssia.

Seuraavassa on ohjeita, joita tarvitaan sovelluksen Gatewayn luominen.

## <a name="create-a-resource-group-for-resource-manager"></a>Luoda resurssiryhmän resurssin Manageria varten

Varmista, että käytät PowerShellin Azure uusimman version. Lisätietoja on saatavana [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Vaihe 1

Azure kirjautuminen

    Login-AzureRmAccount

Sinua kehotetaan todentamismenetelmä tunnistetiedot.

### <a name="step-2"></a>Vaihe 2

Tarkista tilaukset-tilin.

    Get-AzureRmSubscription

### <a name="step-3"></a>Vaihe 3

Valitse, mitä Azure tilauksistasi käyttämään.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Vaihe 4

Resurssiryhmä (ohita tämä vaihe, jos käytät aiemmin resurssiryhmä) luominen

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tämä sijainti käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Varmista, että kaikki komennot, voit luoda yhdyskäytävän sovellus käyttää samaa resurssiryhmä.

Yllä olevassa esimerkissä luomaasi nimeltä "appgw RG" ja "Länsi US" sijainnin resurssiryhmä.

>[AZURE.NOTE] Jos haluat määrittää mukautetun näytteenottimen sovelluksen Gateway-kohdassa [Luo mukautettu keräysputkien PowerShell-toiminnolla sovelluksen-Gatewaylle](application-gateway-create-probe-ps.md). Tutustu [mukautetun keräysputkien ja kunnon seuranta](application-gateway-probe-overview.md) .

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Luo virtuaalisia verkko- ja sovelluksen yhdyskäytävän aliverkon

Seuraavassa esimerkissä esitetään luomisesta virtual verkon resurssien hallinnan avulla.

### <a name="step-1"></a>Vaihe 1

Määritä osoite alueen 10.0.0.0/24 aliverkon muuttujan voidaan luoda virtuaalisia verkkoon.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Vaihe 2

Luo virtuaalisia verkko nimeltä "appgwvnet" resurssin ryhmän "appgw-rg" etuliite 10.0.0.0/16 käyttäminen aliverkon 10.0.0.0/24 Länsi US alue.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Vaihe 3

Määritä aliverkon muuttuja, seuraavat vaiheet, joiden sovelluksen Gatewayn luominen.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luo edusta määritysten julkiseen IP-osoite

Luo julkinen IP-resurssin resurssin ryhmän "appgw-rg" Länsi US alue "publicIP01".

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Luo objekti sovelluksen gateway-määritys

Ennen kuin luot sovelluksen yhdyskäytävä on määritettävä kaikki määritettävät kohteet. Seuraavat vaiheet Luo määritettävät kohteet, joita tarvitaan sovelluksen yhdyskäytävän resurssin.

### <a name="step-1"></a>Vaihe 1

Luo sovelluksen yhdyskäytävän IP määritys nimeltä "gatewayIP01". Sovelluksen Gateway käynnistyessä vastataan aliverkosta, joka on määritetty IP-osoite ja reitittää verkkoliikenteen taustatietokantaan IP-ryhmän IP-osoitteisiin. Ota huomioon, että jokaiselle esiintymälle on yksi IP-osoite.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Vaihe 2

Määritä taustatietokantaan IP-osoiteryhmän nimeltä "pool01" IP-osoitteita "134.170.185.46, 134.170.188.221,134.170.185.50". Nämä IP-osoitteet on IP-osoitteet, joka vastaanottaa joka edusta IP-päätepisteen tulee ‑sovelluksen verkkoliikenteen tarpeisiin. Voit korvata edellisen IP-osoitteet, voit lisätä oman sovelluksen IP-osoite päätepisteet.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Vaihe 3

Määritä sovelluksen yhdyskäytävän asetus "poolsetting01" taustatietokantaan resurssivarantoon kuormituksen-verkkoliikenteelle.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Vaihe 4

Määritä edusta IP-portin nimeltä "frontendport01" julkiseen IP-päätepisteelle.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Vaihe 5

Luo nimeltä "fipconfig01" edusta IP-määritys ja julkiseen IP-osoitteeseen liittäminen edusta IP-määritys.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Vaihe 6

Luo kuuntelua nimi "listener01" ja liittää edusta portin edusta IP-määritys.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Vaihe 7: ssä

Luo kuormituksen tasauspalvelun reititys säännön nimeltä "rule01", joka määrittää kuormituksen tasauspalvelun toiminnan.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Vaihe 8

Määritä sovelluksen yhdyskäytävän esiintymä kokoa.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Voit valita Standard_Small, Standard_Medium ja Standard_Large välillä.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Yhdyskäytävän sovelluksen luomalla uusi AzureRmApplicationGateway

Voit luoda application Gatewayn määritysten kaikki kohteet edellä kuvatut toimet. Tässä esimerkissä sovelluksen yhdyskäytävän nimi on "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Vaihe 9

Noutaa julkiseen IP resurssi liitetty sovelluksen yhdyskäytävän DNS- ja VIP sovelluksen yhdyskäytävän tiedot.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Poista sovellus-yhdyskäytävä

Voit poistaa sovelluksen yhdyskäytävän, toimimalla seuraavasti:

### <a name="step-1"></a>Vaihe 1

Hae yhdyskäytävän sovellusobjektin ja liittää sen muuttujan "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Vaihe 2

**Lopeta AzureRmApplicationGateway** avulla voit lopettaa sovelluksen yhdyskäytävän.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Kun sovellus yhdyskäytävä on pysäytetty-tilaan, **Poista AzureRmApplicationGateway** cmdlet-komennon avulla voit poistaa palvelun.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] **-Pakota** valitsimen avulla voidaan estää vahvistusviestissä Poista.

Jos haluat varmistaa, että palvelu on poistettu, voit käyttää **Get-AzureRmApplicationGateway** cmdlet-komento. Tämä vaihe ei tarvita.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

## <a name="get-application-gateway-dns-name"></a>Hanki sovellus yhdyskäytävän DNS-nimi

Kun yhdyskäytävä on luotu, seuraava vaihe on viestintään edusta määrittämiseen. Käytettäessä julkiseen IP-sovelluksen yhdyskäytävän edellyttää dynaamisesti määritetty DNS-nimi, joka ei ole helpossa muodossa. Jotta loppukäyttäjät näppäintä sovelluksen yhdyskäytävän CNAME-tietueen avulla voidaan osoittamaan julkisen päätepisteen sovelluksen yhdyskäytävän. [Mukautetun toimialuenimi Azure-tietokannassa määrittäminen](../cloud-services/cloud-services-custom-domain-name-portal.md). Voit tehdä tämän noutaa tiedot sovelluksen yhdyskäytävän ja sen liittyvät IP/DNS-nimen PublicIPAddress-elementin, joka on liitetty sovelluksen yhdyskäytävän avulla. Sovelluksen yhdyskäytävän DNS-nimeä käytetään luomaan CNAME-tietue, joka osoittaa kahden verkkosovellusten DNS-nimeä. A-tietueiden käyttö ei suositella, koska VIP voivat muuttua uudelleenkäynnistyksen sovelluksen yhdyskäytävän.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat määrittää SSL purku-kohdassa [Configure sovelluksen yhdyskäytävän SSL purku](application-gateway-ssl.md).

Jos haluat määrittää sovelluksen yhdyskäytävän sisäinen kuormituksen käytettäväksi, katso [luominen sovelluksen yhdyskäytävän sisäinen kuormituksen (ILB) kanssa](application-gateway-ilb.md).

Jos haluat lisätietoja ladata yleinen vastatilin asetukset-kohdassa:

- [Azure kuormituksen](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure liikenteen hallinta](https://azure.microsoft.com/documentation/services/traffic-manager/)
