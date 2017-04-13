<properties
   pageTitle="Luo mukautettu näytteenottimen sovelluksen Gatewayn resurssien hallinta PowerShellin avulla | Microsoft Azure"
   description="Opi luomaan mukautettuja näytteenottimen sovelluksen Gatewayn resurssien hallinta PowerShellin avulla"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Luo mukautettu näytteenottimen Azure sovelluksen Gatewayn Azure resurssien hallinta PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-probe-ps.md)
- [Azure perinteinen PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][perinteinen käyttöönoton mallia](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Vaihe 1

Kirjaudu sisään AzureRmAccount avulla voit todentaa.

    Login-AzureRmAccount

### <a name="step-2"></a>Vaihe 2

Tarkista tilaukset-tilin.

    Get-AzureRmSubscription

### <a name="step-3"></a>Vaihe 3

Valitse, mitä Azure tilauksistasi käyttämään. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Vaihe 4

Resurssiryhmä (ohita tämä vaihe, jos käytät aiemmin resurssiryhmä) luominen

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tämä sijainti käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Varmista, että kaikki komennot, voit luoda yhdyskäytävän sovelluksen saman resurssiryhmä.

Yllä olevassa esimerkissä luomaasi nimeltä "appgw RG" ja "Länsi US" sijainnin resurssiryhmä.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Luo virtuaalisia verkko- ja sovelluksen yhdyskäytävän aliverkon

Seuraavat vaiheet Luo virtual verkko- ja sovelluksen yhdyskäytävän aliverkon.

### <a name="step-1"></a>Vaihe 1


Määritä osoite alueen 10.0.0.0/24 aliverkon muuttujan voidaan luoda virtuaalisia verkkoon.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Vaihe 2

Luo virtuaalisia verkko nimeltä "appgwvnet" resurssin ryhmän "appgw-rg" etuliite 10.0.0.0/16 käyttäminen aliverkon 10.0.0.0/24 Länsi US alue.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Vaihe 3

Määritä aliverkon muuttuja, seuraavat vaiheet, joiden sovelluksen Gatewayn luominen.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luo edusta määritysten julkiseen IP-osoite


Luo julkinen IP-resurssin resurssin ryhmän "appgw-rg" Länsi US alue "publicIP01".

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Luoda mukautetun näytteenottimen sovelluksen yhdyskäytävän kokoonpano-objekti

Voit määrittää kaikki määritettävät kohteet ennen kuin luot sovelluksen yhdyskäytävän. Seuraavat vaiheet Luo määritettävät kohteet, joita tarvitaan sovelluksen yhdyskäytävän resurssin.

### <a name="step-1"></a>Vaihe 1

Luo sovelluksen yhdyskäytävän IP määritys nimeltä "gatewayIP01". Sovelluksen yhdyskäytävän käynnistyessä vastataan aliverkosta, joka on määritetty IP-osoite ja reitittää verkkoliikenteen taustatietokantaan IP-ryhmän IP-osoitteisiin. Ota huomioon, että jokaiselle esiintymälle on yksi IP-osoite.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Vaihe 2


Määritä taustatietokantaan IP-osoiteryhmän nimeltä "pool01" IP-osoitteita "134.170.185.46, 134.170.188.221,134.170.185.50". Nämä arvot ovat IP-osoitteet, joka vastaanottaa joka edusta IP-päätepisteen tulee ‑sovelluksen verkkoliikenteen tarpeisiin. Voit korvata edellä voit lisätä oman sovelluksen IP-osoite päätepisteet IP-osoitteet.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Vaihe 3


Mukautetun Keräysputken on määritetty tässä vaiheessa.

Käyttää parametrit ovat seuraavat:

- **Aikaväli** - määrittää näytteenottimen aikavälin tarkistukset sekunnin kuluttua.
- **Aikakatkaisu** - määrittää HTTP-vastauksen valintaruudun näytteenottimen aikakatkaisun.
- **-Isäntänimi ja polku** - täydellinen URL-polku, joka suoritetaan sovelluksen yhdyskäytävän esiintymän terveydentilasta mukaan. Esimerkiksi jos sinulla on sivuston http://contoso.com/, valitse mukautettu keräysputken voidaan määrittää "http://contoso.com/path/custompath.htm", näytteenottimen tarkistuksien on onnistunut HTTP-vastaus.
- **UnhealthyThreshold** - epäonnistui HTTP-vastaukset tarvitaan taustatietokantaan esiintymän kuin merkitsevän numeron *perusasemassa*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Vaihe 4

Määrittää sovelluksen yhdyskäytävän asetus "poolsetting01" tietoliikenteen taustatietokantaan resurssivarantoon. Tämä vaihe on myös aikakatkaisun määrityksistä, jotka on tarkoitettu sovelluksen yhdyskäytävän pyynnön taustatietokantaan resurssivarantoon vastauksen. Kun taustatietokantaan vastauksen käynnit aikarajan-sovelluksen yhdyskäytävän peruuttaa pyynnön. Tämä arvo poikkeaa näytteenottimen aikakatkaisu, joka on tarkoitettu vain taustatietokantaan vastauksen tutkia tarkistukset.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Vaihe 5

Määritä edusta IP-portin nimeltä "frontendport01" julkiseen IP-päätepisteelle.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Vaihe 6

Luo nimeltä "fipconfig01" edusta IP-määritys ja julkiseen IP-osoitteeseen liittäminen edusta IP-määritys.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Vaihe 7: ssä

Luo kuuntelua nimi "listener01" ja liittää edusta portin edusta IP-määritys.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Vaihe 8

Luo kuormituksen tasauspalvelun reititys säännön nimeltä "rule01", joka määrittää kuormituksen tasauspalvelun toiminnan.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Vaihe 9

Määritä sovelluksen yhdyskäytävän esiintymän kokoa.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Voit valita Standard_Small, Standard_Medium ja Standard_Large välillä.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Yhdyskäytävän sovelluksen luomalla uusi AzureRmApplicationGateway

Voit luoda yhdyskäytävän sovelluksen kokoonpanon kaikki kohteet edellä kuvatut vaiheet. Tässä esimerkissä sovelluksen yhdyskäytävän nimi on "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Olemassa olevan sovelluksen yhdyskäytävän näytteenottimen lisääminen

Sinulla on neljä vaihetta mukautetun näytteenottimen lisääminen olemassa olevan sovelluksen-yhdyskäytävä.

### <a name="step-1"></a>Vaihe 1

Lataa sovelluksen yhdyskäytävän resurssin PowerShell muuttujaksi käyttämällä **Hae AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Vaihe 2

Lisää näytteenottimen yhdyskäytävän määritys.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


Esimerkissä mukautetun Keräysputken on määritetty tarkistamaan URL-polku contoso.com/path/custompath.htm 30 sekunnin välein. 120 sekuntia aikakatkaisun raja-arvo on määritetty 8 epäonnistui tutkimuspyyntöjä enimmäismäärä.

### <a name="step-3"></a>Vaihe 3

Lisää näytteenottimen taustatietokantaan resurssivarantoon asetusten määritys ja aikakatkaisun käyttämällä **-määrittäminen-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Vaihe 4

Tallenna määritykset sovelluksen yhdyskäytävän **Määrittäminen AzureRmApplicationGateway**avulla.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Olemassa olevan sovelluksen yhdyskäytävän näytteenottimen poistaminen

Näin voit poistaa mukautetun näytteenottimen olemassa olevan sovelluksen yhdyskäytävän.

### <a name="step-1"></a>Vaihe 1

Lataa sovelluksen yhdyskäytävän resurssin PowerShell muuttujaksi käyttämällä **Hae AzureRmApplicationGateway**.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Vaihe 2

Poista näytteenottimen määritykset sovelluksen yhdyskäytävän **Poista AzureRmApplicationGatewayProbeConfig**avulla.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Vaihe 3

Päivitä taustatietokantaan resurssivarantoon asetus poistaa näytteenottimen ja aikakatkaisun arvo käyttämällä **-määrittäminen-AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Vaihe 4

Tallenna määritykset sovelluksen yhdyskäytävän **Määrittäminen AzureRmApplicationGateway**avulla. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

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

Opettele määrittämään SSL purkaminen ohjesisältöä [Määrittäminen SSL purku](application-gateway-ssl-arm.md)

