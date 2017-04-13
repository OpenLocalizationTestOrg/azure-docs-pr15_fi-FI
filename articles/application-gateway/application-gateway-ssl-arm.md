<properties
   pageTitle="Määrittää SSL purku sovelluksen-yhdyskäytävän käyttämällä Azure Resurssienhallinta | Microsoft Azure"
   description="Tällä sivulla on ohjeita voit luoda yhdyskäytävän sovelluksen SSL purku Azure resurssien hallinnan avulla"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Määritä SSL purku sovelluksen-yhdyskäytävän Azure resurssien hallinnan avulla

> [AZURE.SELECTOR]
-[Azure Portal](application-gateway-ssl-portal.md)
-[Azure Resurssienhallinta PowerShell](application-gateway-ssl-arm.md)
-[Azure perinteinen PowerShell](application-gateway-ssl.md)

 Azure sovelluksen yhdyskäytävän voidaan määrittää Lopeta välttämiseksi kallista SSL salauksen tehtävät ilmenee, WWW-klusterin yhdyskäytävässä Secure Sockets Layer (SSL)-istunto. SSL-purku yksinkertaistaa myös edusta palvelinasetukset ja web-sovelluksen hallinta.

## <a name="before-you-begin"></a>Ennen aloittamista

1. Asentaa uusimman version Azure PowerShell cmdlet-komentojen avulla WWW-ympäristö asennusohjelma. Voit ladata ja asenna uusin versio **Windows PowerShell** -osiosta, [Lataa sivu](https://azure.microsoft.com/downloads/).
2. Voit luoda virtuaalisen verkko- ja sovelluksen yhdyskäytävän aliverkon. Varmista, että cloud ominaisuuksissa eikä näennäiskoneiden on käytössä aliverkon. VPN-osoitteiden on oltava sovelluksen yhdyskäytävän yksinään.
3. Voit määrittää sovelluksen yhdyskäytävän käyttämään palvelimet on oltava tai määrittänyt niiden päätepisteet luotu virtual verkossa tai julkiseen IP-tai VIP kanssa.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mikä on luotava sovelluksen yhdyskäytävän?


- **Taustatietokantaan palvelimen resurssivarantoon:** Taustatietokannan-palvelimien IP-osoitteiden luettelo. IP-osoitteiden luettelossa tulee kuulua joko VPN-aliverkon tai pitäisi olla julkiseen IP-tai VIP.
- **Taustatietokantaan resurssivarantoon palvelinasetukset:** Jokaisen varanto on asetusten, kuten portin, protokolla ja evästeiden perustuva affiniteetti. Nämä asetukset on liitetty resurssivarantoon, ja niitä käytetään varannon kaikki palvelimiin.
- **Edusta portti:** Tämä portti on Julkinen portti, joka on avattu sovelluksen yhdyskäytävän. Liikenne käynnit portin ja sitten uudelleenohjataan taustatietokantaan-palvelimia.
- **Listener:** Kuuntelun edusta portti-protokolla (Http tai Https, nämä asetukset ovat kirjainkoko on merkitsevä), ja SSL-varmenteen nimi (Jos määrittäminen SSL purku).
- **Säännön:** Säännön sitoo kuuntelua ja taustatietokantaan server-ryhmän ja määrittää, mitkä taustatietokantaan palvelimen resurssivarantoon liikenne olisi ohjautuu, kun se käynnit tietyn listener. *Tavallinen* sääntö on tällä hetkellä tueta. *Tavallinen* sääntö on pyöreän pöydän kuormituksen jakauman.

**Lisää määritykset**

SSL-varmenteita kokoonpanossa **HttpListener** protokollan vaihdetaan *Https* (kirjainkoko on merkitsevä). **SslCertificate** -osa lisätään **HttpListener** määritetty SSL-varmenteen muuttujan arvolla. Edustatietokannan portin päivitetäänkö 443.

**Käyttöön eväste perustuva affiniteetti**: sovelluksen yhdyskäytävän voi määrittää varmistaa, että asiakas istunnosta pyyntö ohjataan aina sama AM WWW-klusterin. Tässä skenaariossa tehdään istunnon eväste, joka sallii liikenne voidaan ohjata asianmukaisesti yhdyskäytävän lisäämisen. Evästeiden perustuva affiniteetti käyttöön määrittää **CookieBasedAffinity** *käytössä* **BackendHttpSettings** elementti.


## <a name="create-an-application-gateway"></a>Sovelluksen Gatewayn luominen

Azure perinteinen käyttöönoton mallin ja Azure Resurssienhallinta välinen ero on järjestyksessä, jossa voit luoda yhdyskäytävän sovelluksen ja kohteista, jotka on määritettävä.

Resurssien hallinnan sovelluksen yhdyskäytävän kaikki osat ovat määritetty erikseen ja ladata yhteen, kun haluat luoda sovelluksen yhdyskäytävän yritysresurssi.


Seuraavassa on sovelluksen yhdyskäytävän luonnissa tarvittavat vaiheet:

1. Luoda Resurssiryhmän resurssien hallinta
2. VPN, aliverkon ja julkiseen IP sovelluksen yhdyskäytävän luominen
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

Resurssiryhmä (ohita tämä vaihe, jos käytät aiemmin resurssiryhmä) luominen

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tätä asetusta käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Varmista, että kaikki komennot, voit luoda yhdyskäytävän sovellus käyttää samaa resurssiryhmä.

Yllä olevassa esimerkissä luomaasi nimeltä "appgw RG" ja "Länsi US" sijainnin resurssiryhmä.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Luo virtuaalisia verkko- ja sovelluksen yhdyskäytävän aliverkon

Seuraavassa esimerkissä, miten voit luoda virtuaalisen verkon Resurssienhallinta:

### <a name="step-1"></a>Vaihe 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Tässä esimerkissä määrittää osoite alueen 10.0.0.0/24 aliverkon muuttujan voidaan luoda virtuaalisia verkkoon.

### <a name="step-2"></a>Vaihe 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

Tässä esimerkissä Luo virtuaalisia verkon nimeltä "appgwvnet" resurssin ryhmän "appgw-rg" etuliite 10.0.0.0/16 käyttäminen aliverkon 10.0.0.0/24 Länsi US alue.

### <a name="step-3"></a>Vaihe 3

    $subnet = $vnet.Subnets[0]

Tässä esimerkissä määrittää aliverkon objektin muuttujan $subnet seuraaviin vaiheisiin.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luo edusta määritysten julkiseen IP-osoite

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

Tässä esimerkissä Luo julkinen IP-resurssin resurssin ryhmän "appgw-rg" Länsi US alue "publicIP01".


## <a name="create-an-application-gateway-configuration-object"></a>Luo sovelluksen yhdyskäytävän kokoonpano-objekti

### <a name="step-1"></a>Vaihe 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Tässä esimerkissä Luo sovelluksen yhdyskäytävän IP määritys nimeltä "gatewayIP01". Sovelluksen yhdyskäytävän käynnistyessä vastataan aliverkosta, joka on määritetty IP-osoite ja reitittää verkkoliikenteen taustatietokantaan IP-ryhmän IP-osoitteisiin. Ota huomioon, että jokaiselle esiintymälle on yksi IP-osoite.

### <a name="step-2"></a>Vaihe 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Tässä esimerkissä määrittää taustatietokantaan IP-osoiteryhmän nimeltä "pool01" IP-osoitteita "134.170.185.46, 134.170.188.221,134.170.185.50." Nämä arvot ovat IP-osoitteet, joka vastaanottaa joka edusta IP-päätepisteen tulee ‑sovelluksen verkkoliikenteen tarpeisiin. Korvaa edellisen esimerkin IP-osoitteiden web-sovelluksen päätepisteet IP-osoitteet.

### <a name="step-3"></a>Vaihe 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Tässä esimerkissä määrittää sovelluksen yhdyskäytävän asetus "poolsetting01" kuormituksen verkkoliikennettä taustatietokantaan sarjassa.

### <a name="step-4"></a>Vaihe 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

Tässä esimerkissä määrittää edusta IP-portin nimeltä "frontendport01" julkiseen IP-päätepisteelle.

### <a name="step-5"></a>Vaihe 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

Tässä esimerkissä määrittää SSL-yhteyden käytettyä varmennetta. Varmenne on oltava .pfx-muodossa ja salasana on oltava 4-12 merkkiä.

### <a name="step-6"></a>Vaihe 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

Tässä esimerkissä Luo nimeltä "fipconfig01" edusta IP-määritys ja liittää julkiseen IP-osoitteeseen edusta IP-määritys.

### <a name="step-7"></a>Vaihe 7: ssä

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


Tässä esimerkissä Luo listener nimi "listener01" ja liittää edusta portin edusta-IP-määritys ja varmennetta.

### <a name="step-8"></a>Vaihe 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

Tässä esimerkissä Luo kuormituksen tasauspalvelun reititys säännön nimeltä "rule01", joka määrittää kuormituksen tasauspalvelun toiminnan.

### <a name="step-9"></a>Vaihe 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Tässä esimerkissä määrittää sovelluksen yhdyskäytävän esiintymän kokoa.

>[AZURE.NOTE]  *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Voit valita Standard_Small, Standard_Medium ja Standard_Large välillä.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Yhdyskäytävän sovelluksen luomalla uusi AzureApplicationGateway

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

Tässä esimerkissä Luo yhdyskäytävän sovellus, jossa kaikki määritysten kohteet edellä kuvatut toimet. Esimerkissä sovelluksen yhdyskäytävän nimi on "appgwtest".

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

Jos haluat määrittää sovelluksen yhdyskäytävän käytettävät sisäiset kuormituksen (ILB), katso [luominen sovelluksen yhdyskäytävän sisäinen kuormituksen (ILB) kanssa](application-gateway-ilb.md).

Jos haluat lisätietoja ladata yleinen vastatilin asetukset-kohdassa:

- [Azure kuormituksen](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure liikenteen hallinta](https://azure.microsoft.com/documentation/services/traffic-manager/)
