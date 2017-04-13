<properties
   pageTitle="Web-sovelluksen palomuurin määrittämistä uuteen tai aiemmin luotuun sovelluksen yhdyskäytävän | Microsoft Azure"
   description="Tässä artikkelissa on ohjeita siitä, miten voit käyttää web-sovelluksen palomuurin olemassa oleva vai uusi sovellus yhdyskäytävässä."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Web-sovelluksen palomuurin määrittämistä uuteen tai aiemmin luotuun sovelluksen yhdyskäytävän

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-web-application-firewall-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-web-application-firewall-powershell.md)

Web application palomuuri (WAF) Azure sovelluksen yhdyskäytävän suojaa verkkosovellusten yleisiä verkkopohjaisia kalastelu, kuten SQL lisäämisen ja komentosarjoja sivustojen-istunnon hijacks.

Azure sovelluksen yhdyskäytävä on kerroksen 7 kuormituksen. Se sisältää automaattisesti, suorituskyvyn reititys-pyyntöjen eri palvelinten välillä, olivatpa ne sitten cloud tai paikallisen. Sovelluksen sisältää useita sovelluksen toimituksen ohjauskoneen (ADC) mukaan lukien HTTP kuormituksen tasaamisen, eväste perustuva istunnon affiniteetti, Secure Sockets Layer (SSL) purku mukautetun kunto keräysputkien usean sivuston tuki ja monista muista. Luettelo kaikista tuetut ominaisuudet etsimällä käy sovelluksen yhdyskäytävän yleiskatsaus

Seuraava artikkeli näyttää [lisäämisestä aiemmin sovelluksen yhdyskäytävän web application palomuurin](#add-web-application-firewall-to-an-existing-application-gateway) ja [Luo sovelluksen yhdyskäytävän, joka käyttää web-sovelluksen palomuuri](#create-an-application-gateway-with-web-application-firewall).

![Skenaario kuva][scenario]

## <a name="waf-configuration-differences"></a>WAF määritys erot

Jos olet lukenut [Luo sovelluksen yhdyskäytävän PowerShellin](application-gateway-create-gateway-arm.md), ymmärrät tuote-asetusten määrittäminen sovelluksen yhdyskäytävän luotaessa. WAF on lisäasetuksia, voit määrittää määritettäessä olevat versiot sovelluksen yhdyskäytävän. Jakautuvat ei ole itse sovelluksen yhdyskäytävän tekemäsi muutokset.

**Tuote** - Normaali sovelluksen yhdyskäytävän ilman WAF tukee **Vakio\_pieni**, **Vakio\_Normaali**, ja **Vakio\_suuri** koot. WAF esittely, jossa on kaksi muita tuotteissa **WAF\_Normaali** ja **WAF\_suuri**. Pieni sovellus yhdyskäytävien WAF ei tueta.

**Taso** - käytettävissä olevat arvot ovat **Vakio** - tai **WAF**. Web-sovelluksen palomuurin käytettäessä **WAF** on valittava.

**Tila** - asetus on WAF tilassa. sallittu arvo on **tunnistaminen** ja **estäminen**. Kun WAF on määritetty tunnistamisen tilassa, kaikki uhkien tallennetaan lokitiedostoon. Tapahtumat kirjataan edelleen estämisen, mutta hän saa 403 luvattoman vastauksen sovelluksen Yhdyskäytävästä.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Web-sovelluksen palomuurin lisääminen olemassa olevan sovelluksen yhdyskäytävän

Varmista, että käytät PowerShellin Azure uusimman version. Lisätietoja on saatavana [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Vaihe 1

Kirjaudu sisään Azure-tili.

    Login-AzureRmAccount

### <a name="step-2"></a>Vaihe 2

Valitse käytettävä tässä skenaariossa tilaus.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Vaihe 3

Noutaa yhdyskäytävän, jotka olet lisäämässä Web-sovelluksen palomuuri.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Vaihe 4

Määritä web-sovelluksen palomuuri-tuote. Käytettävissä olevat koot ovat **WAF\_suuri** ja **WAF\_Normaali**. Web-sovelluksen palomuurin käytettäessä tason on oltava **WAF**, kapasiteetti on vahvistettava, kun määrität olevat versiot.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Vaihe 5

WAF asetusten määritysten seuraavan esimerkin mukaisesti:

**WafMode** -asetus, käytettävissä olevat arvot on ehkäisemistä ja havaitsemista.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Vaihe 6

Päivitä sovellus yhdyskäytävän määritettyjen ehtojen edellisessä vaiheessa.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

Tämä komento päivittää sovelluksen yhdyskäytävän Web-sovelluksen palomuuri. On suositeltavaa tarkastella, kuinka voit tarkastella lokit sovelluksen Gatewayn [Sovelluksen yhdyskäytävän diagnostiikka](application-gateway-diagnostics.md) . WAF suojaus, laatu vuoksi lokit on tarkistettava säännöllisesti ymmärtää web-sovellusten suojaus-reagoimisessa.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Luo sovelluksen yhdyskäytävän Web-sovelluksen palomuuri

Seuraavat vaiheet kestää koko prosessin alusta loppuun luomiseen sovelluksen yhdyskäytävän Web-sovelluksen palomuurin läpi.

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

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Vaihe 4

Resurssiryhmä (ohita tämä vaihe, jos käytät aiemmin resurssiryhmä) luominen

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tämä sijainti käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Varmista, että kaikki komennot, voit luoda yhdyskäytävän sovellus käyttää samaa resurssiryhmä.

Edellisessä esimerkissä luomaamme nimeltä "appgw RG" ja "Länsi US" sijainnin resurssiryhmä.

>[AZURE.NOTE] Jos haluat määrittää mukautetun näytteenottimen sovelluksen Gateway-kohdassa [Luo mukautettu keräysputkien PowerShell-toiminnolla sovelluksen-Gatewaylle](application-gateway-create-probe-ps.md). Tutustu [mukautetun keräysputkien ja kunnon seuranta](application-gateway-probe-overview.md) .

### <a name="step-5"></a>Vaihe 5

Liitä osoitealueen aliverkon voidaan käyttää sovelluksen yhdyskäytävän itse.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Sovelluksen aliverkon pitäisi olla vähintään 28 syöttörajoitteen bittien määrän. Tämä arvo ei 10 käytettävissä osoitteet sovelluksen Yhdyskäytäväesiintymästä koostuva aliverkon. Pienempi aliverkon, jossa voit ei ehkä voi lisätä sovelluksen yhdyskäytävän Lisää esiintymä tulevaisuudessa.

### <a name="step-6"></a>Vaihe 6

Liitä osoitealue, jota käytetään Taustajärjestelmä osoite resurssivarantoon.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Vaihe 7: ssä

Luo vaiheessa luotu edellisen aliverkosta resurssiryhmän virtual verkosta: [resurssi-ryhmän luominen](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Vaihe 8

Noutaa virtual verkkoresurssin ja aliverkon resursseja voi käyttää seuraavasti:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Vaihe 9

Luo julkinen IP-resurssi, jota käytetään sovelluksen yhdyskäytävän. Tätä yleisen IP-osoitetta käytetään jokin seuraavista toimista:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Sovelluksen yhdyskäytävä ei tue julkinen IP-osoite, joka on luotu käyttämällä toimialueen selite käyttö. Tuetaan vain julkinen IP-osoite on luotu dynaamisesti toimialueen otsikko. Jos asetat sovelluksen yhdyskäytävän helpossa muodossa dns-nimen, on suositeltavaa käyttää cname-tietueen alias.

### <a name="step-10"></a>Vaihe 10

Ennen kuin luot sovelluksen yhdyskäytävä on määritettävä kaikki määritettävät kohteet. Seuraavat vaiheet Luo määritettävät kohteet, joita tarvitaan sovelluksen yhdyskäytävän resurssin.

Luo sovelluksen yhdyskäytävän IP-määritykset, tämä määrittää, mitä aliverkon sovelluksen yhdyskäytävä käyttää. Sovelluksen yhdyskäytävän käynnistyessä vastataan aliverkosta, joka on määritetty IP-osoite ja reitittää verkkoliikenteen taustatietokantaan IP-ryhmän IP-osoitteisiin. Ota huomioon, että jokaiselle esiintymälle on yksi IP-osoite.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Vaihe 11

Määritä taustatietokantaan IP-osoite-ryhmän IP-osoitteiden Taustajärjestelmä verkko-palvelimiin. Nämä IP-osoitteet on IP-osoitteet, joka vastaanottaa joka edusta IP-päätepisteen tulee ‑sovelluksen verkkoliikenteen tarpeisiin. Voit korvata seuraavia IP-osoitteet, voit lisätä oman sovelluksen IP-osoite päätepisteet.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Vaihe 12

Lataa sertifikaatti, jota käytetään ssl käytössä Taustajärjestelmä resurssivarannon resursseja.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Vaihe 13

Sovelluksen yhdyskäytävän taustatietokantaan http-asetusten määrittäminen. Määrittää varmenteen ladattu edellisessä vaiheessa http-asetukset.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Vaihe 14

Määritä edusta IP-portin julkiseen IP-päätepisteelle. Tämä portti on portti, johon käyttäjät muodostaa yhteyden.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Vaihe 15

Luo edusta IP-määritys-asetusta yhdistää yksityisen tai julkisen ip-osoite edusta sovelluksen yhdyskäytävän. Seuraavassa vaiheessa liittää julkisen IP-osoitteen edellisessä vaiheessa edusta IP-määritys.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Vaihe 16

Sovelluksen Gateway-varmenteen määrittäminen. Tämän todistuksen käytetään salaus ja salata uudelleen sovelluksen yhdyskäytävän liikenne.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Vaihe 17

Luo sovelluksen yhdyskäytävän HTTP-kuuntelutoiminnon. Määritä käytettävä edusta ip määritysten porteista ja ssl sertifikaatti.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Vaihe 18

Luo kuormituksen tasauspalvelun reititys sääntö, joka määrittää kuormituksen tasauspalvelun toiminnan. Tässä esimerkissä basic PYÖRISTÄ-funktiota Mikko sääntö on luotu.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Vaihe 19

Määritä sovelluksen yhdyskäytävän esiintymän kokoa.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Voit valita **WAF\_Normaali** ja **WAF\_suuri**, kun käyttäminen WAF on aina **WAF**taso. Kapasiteetti on jokin muu luku väliltä 1 – 10.

### <a name="step-20"></a>Vaihe 20

Tilan määrittäminen WAF, kelvollisia arvoja on **ehkäisemistä** ja **havaitsemista**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Vaihe 21

Voit luoda application Gatewayn määritysten kaikki kohteet edellä kuvatut toimet. Tässä esimerkissä sovelluksen yhdyskäytävän nimi on "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Opettele määrittämään Diagnostiikan kirjaus lokiin tapahtumat, jotka ovat havaita tai esti Web-sovelluksen palomuuri ohjesisältöä [Sovelluksen yhdyskäytävän diagnostiikka](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png