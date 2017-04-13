<properties
    pageTitle="SSL-käytäntö ja pääty SSL määrittäminen Application Gatewayn | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Määritä pääty SSL Application Gatewayn Azure resurssien hallinta PowerShellin avulla"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>SSL-käytäntö ja pääty SSL määrittäminen Application Gatewayn PowerShellin avulla

## <a name="overview"></a>Yleiskatsaus

Sovelluksen Gateway tukee liikenteen lopusta loppuun-salausta. Sovelluksen yhdyskäytävän tekee tämän sovelluksen yhdyskäytävässä SSL-yhteyden. Yhdyskäytävän koskee Reitityssääntöjen liikenne, salaa uudelleen paketin ja välittää paketin perusteella määritetty Reitityssääntöjen tarvittavat Taustajärjestelmä. Jokainen vastaus verkkopalvelin käy läpi samoja ohjeita takaisin peruskäyttäjän.

Toiseen ominaisuus kyseisen sovelluksen gateway tukee on käytöstä tiettyjen SSL-protokollan versiot. Sovelluksen Gateway tukee seuraavia protokollan version; poistaminen käytöstä TLSv1.0 TLSv1.1 ja TLSv1.2.

> [AZURE.NOTE] SSL 2.0- ja SSL 3.0 on oletusarvoisesti poistettu käytöstä ja ei voi ottaa käyttöön. Ne pidetään suojaamaton ja eivät voi käyttää sovelluksen yhdyskäytävän

![Skenaario kuva][scenario]

## <a name="scenario"></a>Skenaario

Tässä skenaariossa voit tietokantakyselyjen luominen sovelluksen-yhdyskäytävän pääty SSL PowerShellin avulla.

Tässä skenaariossa on:

- Luoda resurssiryhmän nimeltä "appgw rg"
- Luo virtuaalisia verkko nimeltä "appgwvnet" varattu CIDR tekstialueen 10.0.0.0/16 kanssa.
- Luo kaksi aliverkosta, jonka nimi on "appgwsubnet" ja "appsubnet".
- Lopusta loppuun SSL-salausta, joka estää tiettyjä SSL-protokollaa tukevat pieni sovellus Gatewayn luominen.

## <a name="before-you-begin"></a>Ennen aloittamista

Määrittää sovelluksen yhdyskäytävän pääty SSL-varmenteen tarvitaan yhdyskäytävän ja varmenteet tarvitaan Taustajärjestelmä palvelimia. Voit salata ja salauksen liikenne lähettää sen SSL-yhteys käytetään yhdyskäytävän varmenne. Yhdyskäytävän varmenne on oltava henkilökohtaisten tietojen vaihtaminen (pfx)-muodossa. Tässä tiedostomuodossa avulla voidaan viedä yksityinen avain joka vaaditaan suorittamaan salausta ja salauksen liikenteen sovelluksen yhdyskäytävän.

Lopusta loppuun ssl-salausta taustaan on oltava whitelisted application Gatewayn. Tämä tapahtuu lataamalla backends julkisten varmenteen sovelluksen yhdyskäytävän. Näin varmistat, että sovelluksen yhdyskäytävän yhteydessä vain tunnetut Taustajärjestelmä esiintymät. Tämä suojaa edelleen lopusta loppuun-tietoliikenne.

Tätä prosessia on kuvattu seuraavasti:

## <a name="create-the-resource-group"></a>Resurssiryhmän luominen

Tässä osassa esitellään resurssiryhmä, joka sisältää sovelluksen yhdyskäytävän luominen.

### <a name="step-1"></a>Vaihe 1

Kirjaudu sisään Azure-tili.

    Login-AzureRmAccount

### <a name="step-2"></a>Vaihe 2

Valitse käytettävä tässä skenaariossa tilaus.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Vaihe 3

Resurssiryhmä (ohita tämä vaihe, jos käytät aiemmin resurssiryhmä) luominen

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Luo virtuaalisia verkko- ja sovelluksen yhdyskäytävän aliverkon

Seuraavassa esimerkissä luodaan virtual verkko- ja kahden aliverkon. Yhden aliverkon on käyttää sovelluksen yhdyskäytävän. Muut aliverkon käytetään isännöinnin verkkosovelluksen backends.

### <a name="step-1"></a>Vaihe 1

Liitä osoitealueen aliverkon voidaan käyttää sovelluksen yhdyskäytävän itse.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Sovelluksen yhdyskäytävän määritetty aliverkosta olisi esitystapa oikein. Sovelluksen yhdyskäytävän voidaan määrittää enintään 10 esiintymät. Jokaiselle esiintymälle on 1 aliverkosta IP-osoite. Liian pieni aliverkon voit heikentää laajentaminen sovelluksen-yhdyskäytävä.

### <a name="step-2"></a>Vaihe 2

Liitä osoitealue, jota käytetään Taustajärjestelmä osoite resurssivarantoon.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Vaihe 3

Luo virtuaalisia verkon aliverkosta määritysten edellisten vaiheiden mukaisesti.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Vaihe 4

Noutaa virtual verkkoresurssin ja aliverkon resursseja voi käyttää seuraavasti:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luo edusta määritysten julkiseen IP-osoite

Luo julkinen IP-resurssi, jota käytetään sovelluksen yhdyskäytävän. Tätä yleisen IP-osoitetta käytetään seuraavassa vaiheessa.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Sovelluksen yhdyskäytävä ei tue julkinen IP-osoite, joka on luotu käyttämällä toimialueen selite käyttö. Tuetaan vain julkinen IP-osoite on luotu dynaamisesti toimialueen otsikko. Jos asetat sovelluksen yhdyskäytävän helpossa muodossa dns-nimen, on suositeltavaa käyttää cname-tietueen alias.

## <a name="create-an-application-gateway-configuration-object"></a>Luo sovelluksen yhdyskäytävän kokoonpano-objekti

Ennen kuin luot sovelluksen yhdyskäytävä on määritettävä kaikki määritettävät kohteet. Seuraavat vaiheet Luo määritettävät kohteet, joita tarvitaan sovelluksen yhdyskäytävän resurssin.

### <a name="step-1"></a>Vaihe 1

Luo sovelluksen yhdyskäytävän IP-määritykset, tämä asetus määrittää mitä aliverkon sovelluksen yhdyskäytävä käyttää. Sovelluksen yhdyskäytävän käynnistyessä vastataan aliverkosta, joka on määritetty IP-osoite ja reitittää verkkoliikenteen taustatietokantaan IP-ryhmän IP-osoitteisiin. Ota huomioon, että jokaiselle esiintymälle on yksi IP-osoite.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Vaihe 2

Luo edusta IP-määritys-asetusta yhdistää yksityisen tai julkisen ip-osoite edusta sovelluksen yhdyskäytävän. Seuraavassa vaiheessa liittää julkisen IP-osoitteen edellisessä vaiheessa edusta IP-määritys.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Vaihe 3

Määritä taustatietokantaan IP-osoite-ryhmän IP-osoitteiden Taustajärjestelmä verkko-palvelimiin. Nämä IP-osoitteet on IP-osoitteet, joka vastaanottaa joka edusta IP-päätepisteen tulee ‑sovelluksen verkkoliikenteen tarpeisiin. Voit korvata seuraavia IP-osoitteet, voit lisätä oman sovelluksen IP-osoite päätepisteet.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Täydellinen toimialuenimi (FQDN) on myös kelvollinen arvo tekstin näyttäminen Taustajärjestelmä palvelimia ip-osoite - BackendFqdns valitsimen avulla.

### <a name="step-4"></a>Vaihe 4

Määritä edusta IP-portin julkiseen IP-päätepisteelle. Tämä portti on portti, johon käyttäjät muodostaa yhteyden.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Vaihe 5

Sovelluksen Gateway-varmenteen määrittäminen. Tämän todistuksen käytetään salaus ja salata uudelleen sovelluksen yhdyskäytävän liikenne.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] Tässä esimerkissä määrittää SSL-yhteyden käytettyä varmennetta. Varmenne on oltava .pfx-muodossa ja salasana on oltava 4-12 merkkiä.

### <a name="step-6"></a>Vaihe 6

Luo sovelluksen yhdyskäytävän HTTP-kuuntelutoiminnon. Määritä käytettävä edusta ip määritysten porteista ja ssl sertifikaatti.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Vaihe 7: ssä

Lataa sertifikaatti, jota käytetään ssl käytössä Taustajärjestelmä resurssivarannon resursseja.

> [AZURE.NOTE] Oletusarvon näytteenottimen julkinen avain hakee **oletusarvon** SSL-sidonta takaisin + end IP-osoite ja vertaa se saa antaisit tätä yleisen avainarvon julkisen avaimen arvo. Haetut julkinen avain ei välttämättä ole ehkä tarkoitettuun sivustoon, johon liikenne flow **Jos** käytät host ylä- ja SNI sitten taustatietokantatiedosto. Jos olet epävarma, valitse Edellinen päät vahvistamaan, mitä sertifikaattia käytetään **oletusarvon** SSL-sidonta-sivustossa https://127.0.0.1/. Tämän osan julkinen avain-pyynnön avulla. Jos et saa vastauksen ja varmennetta manuaalinen selaimen pyynnöstä https://127.0.0.1/ Edellinen-päätepisteissä, käytät host-ylä- ja SNI HTTPS sidontojen, sinun on määritettävä oletusarvon SSL-sidonta, Edellinen-päätepisteissä. Jos et tee niin, keräysputkien epäonnistuu ja sitten taustatietokantatiedosto voidaan eivät whitelisted.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Tässä vaiheessa annettu varmenne on oltava esitä taustassa pfx varmenteen julkinen avain. Vie varmenne (ei pääkansion varmenne), asennettuihin palvelimeen. CER Muotoile ja käyttää sitä tässä vaiheessa. Vaihe whitelists application Gatewayn Taustajärjestelmä.

### <a name="step-8"></a>Vaihe 8

Sovelluksen yhdyskäytävän taustatietokantaan http-asetusten määrittäminen. Määrittää varmenteen ladattu edellisessä vaiheessa http-asetukset.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Vaihe 9

Luo kuormituksen tasauspalvelun reititys sääntö, joka määrittää kuormituksen tasauspalvelun toiminnan. Tässä esimerkissä basic PYÖRISTÄ-funktiota Mikko sääntö on luotu.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Vaihe 10

Määritä sovelluksen yhdyskäytävän esiintymän kokoa.  Käytettävissä olevat koot ovat **Vakio\_pieni**, **Vakio\_Normaali**, ja **Vakio\_suuri**.  Kapasiteetin käytettävissä olevat arvot ovat 1 – 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] 1 esiintymä-määrä on valittava testausta varten. On tärkeää tietää, minkä tahansa esiintymän määrä-kohdassa kaksi esiintymää ei koske SLA ja näin ollen ei suositella. Pieni yhdyskäytäville on käytettävä keskihajonta-ehtoa ja ei tuotannon varten.

### <a name="step-11"></a>Vaihe 11

Määritä, jota käytetään sovelluksen yhdyskäytävän SSL-käytäntö. Sovelluksen Gateway tukee tiettyjä SSL-protokollan versiot poistetaan käytöstä.

Seuraavat arvot ovat luettelo protokolla-versioista, jotka voidaan poistaa käytöstä.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

Seuraavassa esimerkissä poistetaan käytöstä TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Sovelluksen yhdyskäytävän luominen

Sovelluksen yhdyskäytävän Luo edellisten vaiheiden avulla. Yhdyskäytävän luominen on pitkä käynnissä oleva prosessi.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>Olemassa olevan sovelluksen yhdyskäytävän SSL protokolla versiot poistaminen käytöstä

Edellisten vaiheiden siirtää sinut pääty SSL sovelluksen luominen ja poistaminen käytöstä tiettyjen SSL-protokollan versiot. Seuraavassa esimerkissä poistetaan käytöstä tiettyjen olemassa olevan sovelluksen yhdyskäytävän SSL hallintakäytäntöjä.

### <a name="step-1"></a>Vaihe 1

Noutaa sovelluksen yhdyskäytävän päivittäminen.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Vaihe 2

Määritä SSL-käytäntö. Seuraavassa esimerkissä TLSv1.0 ja TLSv1.1 poistetaan käytöstä.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Vaihe 3

Päivitä-yhdyskäytävä. On tärkeää muistaa, että tämä viimeinen vaihe on pitkä käynnissä olevan tehtävän. Kun se on valmis, lopusta loppuun ssl on määritetty sovelluksen yhdyskäytävän.

    $gw | Set-AzureRmApplicationGateway

## <a name="get-application-gateway-dns-name"></a>Hanki sovellus yhdyskäytävän DNS-nimi

Kun yhdyskäytävä on luotu, seuraava vaihe on viestintään edusta määrittämiseen. Käytettäessä julkiseen IP-sovelluksen yhdyskäytävän edellyttää dynaamisesti määritetty DNS-nimi, joka ei ole helpossa muodossa. Jotta loppukäyttäjät näppäintä sovelluksen yhdyskäytävän CNAME-tietueen avulla voidaan osoittamaan julkisen päätepisteen sovelluksen yhdyskäytävän. [Mukautetun toimialuenimi Azure-tietokannassa määrittäminen](../cloud-services/cloud-services-custom-domain-name-portal.md). Voit tehdä tämän noutaa tiedot sovelluksen yhdyskäytävän ja sen liittyvät IP/DNS-nimen PublicIPAddress-elementin, joka on liitetty sovelluksen yhdyskäytävän avulla. Sovelluksen yhdyskäytävän DNS-nimeä käytetään luomaan CNAME-tietue, joka osoittaa kahden verkkosovellusten DNS-nimeä. A-tietueiden käyttö ei suositella VIP voivat muuttua sovelluksen yhdyskäytävän uudelleenkäynnistyksen jälkeen.
    
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

Lisätietoja hardening Web-sovelluksen palomuuri sovelluksen yhdyskäytävän kautta web-sovellusten suojaus ohjesisältöä [Web Application palomuurin yleiskatsaus](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
