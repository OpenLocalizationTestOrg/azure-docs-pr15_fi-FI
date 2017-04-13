<properties
   pageTitle="Luominen, Käynnistä tai poista sovellus-yhdyskäytävän | Microsoft Azure"
   description="Tällä sivulla on ohjeita luominen, määrittäminen, aloittaminen ja Azure sovelluksen yhdyskäytävän poistaminen"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Luominen, Käynnistä tai poista sovellus-yhdyskäytävä

> [AZURE.SELECTOR]
- [Azure Portal](application-gateway-create-gateway-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-gateway-arm.md)
- [Azure perinteinen PowerShell](application-gateway-create-gateway.md)
- [Azure Resurssienhallinta-malli](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure sovelluksen yhdyskäytävä on kerroksen 7 kuormituksen. Se sisältää automaattisesti, suorituskyvyn reititys-pyyntöjen eri palvelinten välillä, olivatpa ne sitten cloud tai paikallisen. Sovelluksen yhdyskäytävän sisältää useita sovelluksen toimituksen ohjauskoneen (ADC) mukaan lukien HTTP kuormituksen tasaamisen, eväste perustuva istunnon affiniteetti, Secure Sockets Layer (SSL) purku mukautetun kunto keräysputkien usean sivuston tuki ja monista muista. Luettelo kaikista tuetut ominaisuudet etsimällä käy [Sovelluksen yhdyskäytävän yleiskatsaus](application-gateway-introduction.md)

Tässä artikkelissa käydään läpi vaiheet ja luo, määrittäminen, aloittaminen ja poista sovellus-yhdyskäytävä.

## <a name="before-you-begin"></a>Ennen aloittamista

1. Asentaa uusimman version Azure PowerShell cmdlet-komentojen avulla WWW-ympäristö asennusohjelma. Voit ladata ja asenna uusin versio **Windows PowerShell** -osiosta, [Lataa sivu](https://azure.microsoft.com/downloads/).
2. Jos sinulla on aiemmin virtual verkkoon, valitse aiemmin luotu tyhjä aliverkon tai uusi aliverkon luominen olemassa olevan virtual verkoston ainoastaan sovelluksen yhdyskäytävän. Eri virtual verkkoon aiot ottaa käyttöön sovelluksen yhdyskäytävän takana, ellei vnet peering käytetään resursseja kuin sovelluksen yhdyskäytävä ei voi ottaa käyttöön. Saat lisätietoja seuraavasta [Vnet Peering](../virtual-network/virtual-network-peering-overview.md)
3. Varmista, että sinulla on kelvollinen aliverkon toimimasta virtual verkosta. Varmista, että cloud ominaisuuksissa eikä näennäiskoneiden on käytössä aliverkon. VPN-osoitteiden on oltava sovelluksen yhdyskäytävän yksinään.
3. Palvelimet, joilla voit määrittää sovelluksen yhdyskäytävän käyttämään on oltava tai määrittänyt niiden päätepisteet luotu virtual verkossa tai julkiseen IP-tai VIP kanssa.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mikä on luotava sovelluksen yhdyskäytävän?

Kun **Uusi AzureApplicationGateway** -komennon avulla voit luoda sovelluksen yhdyskäytävän-määrityksiä on määritetty tässä vaiheessa ja juuri luomasi resurssi on määritetty joko XML- tai kokoonpano-objekti.

Arvot ovat:

- **Taustatietokantaan palvelimen resurssivarantoon:** Taustatietokannan-palvelimien IP-osoitteiden luettelo. IP-osoitteiden luettelossa tulee kuulua joko VPN-aliverkon tai pitäisi olla julkiseen IP-tai VIP.
- **Taustatietokantaan resurssivarantoon palvelinasetukset:** Jokaisen varanto on asetusten, kuten portin, protokolla ja evästeiden perustuva affiniteetti. Nämä asetukset on liitetty resurssivarantoon, ja niitä käytetään varannon kaikki palvelimiin.
- **Edusta portti:** Tämä portti on Julkinen portti, joka on avattu sovelluksen yhdyskäytävän. Liikenne käynnit portin ja sitten uudelleenohjataan taustatietokantaan-palvelimia.
- **Listener:** Kuuntelun edusta portti-protokolla (Http tai Https-arvot ovat kirjainkoko on merkitsevä), ja SSL-varmenteen nimi (Jos määrittäminen SSL purku).
- **Säännön:** Säännön sitoo kuuntelua ja taustatietokantaan server-ryhmän ja määrittää, mitkä taustatietokantaan palvelimen resurssivarantoon liikenne olisi ohjautuu, kun se käynnit tietyn listener.

## <a name="create-an-application-gateway"></a>Sovelluksen Gatewayn luominen

Voit luoda yhdyskäytävän sovelluksen seuraavasti:

1. Luo sovelluksen yhdyskäytävän yritysresurssi.
2. Luo XML-kokoonpanotiedosto tai kokoonpano-objekti.
3. Vahvista juuri luomasi sovelluksen yhdyskäytävän resurssin määritykset.

>[AZURE.NOTE] Jos haluat määrittää mukautetun näytteenottimen sovelluksen Gateway-kohdassa [Luo mukautettu keräysputkien PowerShell-toiminnolla sovelluksen-Gatewaylle](application-gateway-create-probe-classic-ps.md). Tutustu [mukautetun keräysputkien ja kunnon seuranta](application-gateway-probe-overview.md) .

![Skenaario-Esimerkki][scenario]

### <a name="create-an-application-gateway-resource"></a>Luo sovellus-yhdyskäytävän resurssi

Voit luoda yhdyskäytävän käyttämällä **Uutta AzureApplicationGateway** -cmdlet-arvojen korvaaminen omalla. Laskutuksen yhdyskäytävä ei käynnisty tässä vaiheessa. Laskutus alkaa myöhemmin, kun yhdyskäytävä on käynnistetty.

Seuraavassa esimerkissä luodaan sovelluksen yhdyskäytävän nimeltä "testvnet1" ja "aliverkon 1"-niminen aliverkon virtual verkkoa käyttäen.


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Kuvaus*, *InstanceCount*ja *GatewaySize* ovat valinnaisia parametreja.

Vahvista, että yhdyskäytävä on luotu, voit **Saada AzureApplicationGateway** cmdlet-komento.

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Voit valita pieni, Normaali tai suuri.

*VirtualIPs* ja *DnsName* ovat näkyvissä kuin tyhjä koska yhdyskäytävä ei ole vielä aloitettu. Nämä luodaan, kun yhdyskäytävä on käytössä-tilassa.

## <a name="configure-the-application-gateway"></a>Sovelluksen yhdyskäytävän asetusten määrittäminen

Voit määrittää sovelluksen yhdyskäytävän käyttämällä XML- tai kokoonpano-objekti.

## <a name="configure-the-application-gateway-by-using-xml"></a>Määrittää sovelluksen yhdyskäytävän käyttämällä XML

Seuraavassa esimerkissä käytetään XML-tiedostoon kaikki sovelluksen yhdyskäytävän asetusten määrittäminen ja vahvista sovelluksen yhdyskäytävän resurssi.  

### <a name="step-1"></a>Vaihe 1  

Kopioi seuraava teksti Muistioon.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Muokkaa sulkumerkkien määritysten kohteiden arvoja. Tallenna tiedosto tunniste .xml.

>[AZURE.IMPORTANT] Protokollan kohteen Http tai Https kirjainkoko on merkitsevä.

Seuraavassa esimerkissä esitetään sovelluksen yhdyskäytävän määrittäminen määritystiedoston avulla. Esimerkki kuormituksen saldojen HTTP-liikenteen julkiseen porttiin 80 ja lähettää verkkoliikennettä taustatietokantaan portti 80 kaksi IP-osoitteiden välillä.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Vaihe 2

Määritä seuraavaksi sovelluksen yhdyskäytävän. **Määrittäminen AzureApplicationGatewayConfig** cmdlet-komennon käyttäminen määritysten XML-tiedosto.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Määrittää sovelluksen yhdyskäytävän käyttämällä kokoonpano-objekti

Seuraavassa esimerkissä esitetään määrittäminen application Gatewayn määritysten objektien avulla. Kaikki määritettävät kohteet on määritetty erikseen ja sitten lisätä sovelluksen yhdyskäytävän kokoonpano-objekti. Kun olet luonut kokoonpano-objekti, käytä **Määrittäminen AzureApplicationGateway** -komento vahvistusta aiemmin luodun sovelluksen yhdyskäytävän resurssin määritykset.

>[AZURE.NOTE] Ennen kuin määrität arvon kunkin kokoonpano-objekti, sinun täytyy määritellä, millaisia objektin PowerShell käyttää tallennustilan. Yksittäisten kohteiden luomiseen ensimmäisen rivin määrittää mitä Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (nimi) käytetään.

### <a name="step-1"></a>Vaihe 1

Luo kaikki yksittäisiä määritettävät kohteet.

Luo edusta IP, kuten seuraavassa esimerkissä.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Luo edusta portti, kuten seuraavassa esimerkissä.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Taustatietokannan server-ryhmän luominen

 Määritä IP-osoitteet, jotka lisätään taustatietokantaan palvelimen resurssivarantoon seuraavan esimerkin mukaisesti.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Arvojen lisääminen taustatietokantaan ryhmän objektin ($pool) $server objektin avulla.

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Luo taustatietokantaan palvelimen resurssivarantoon-asetus.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Luo kuuntelua.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Luo sääntö.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Vaihe 2

Kaikkien yksittäisten määritysten kohteiden liittäminen yhdyskäytävän määritysten sovellusobjektin ($appgwconfig).

Lisää edusta IP määritys.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Lisää edusta-portti määritykset.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Lisää taustatietokantaan palvelimen resurssivarantoon työnkulkuun.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Lisää määritykset taustatietokantaan resurssivarantoon-asetus.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Lisää kuuntelua työnkulkuun.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Lisää sääntö työnkulkuun.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Vaihe 3

Vahvista kokoonpano-objekti sovelluksen yhdyskäytävän resurssien **Määrittäminen AzureApplicationGatewayConfig**avulla.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Käynnistä yhdyskäytävän

Kun yhdyskäytävä on määritetty, Käynnistä yhdyskäytävän **Käynnistä AzureApplicationGateway** cmdlet-komennon avulla. Sovelluksen yhdyskäytävän Laskutus alkaa, kun yhdyskäytävä on aloitettu.

> [AZURE.NOTE] **Käynnistä AzureApplicationGateway** cmdlet-komento saattaa kestää jopa 15-20 minuutin loppuun.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Tarkista yhdyskäytävän tila

Tarkista yhdyskäytävän tila **Get-AzureApplicationGateway** cmdlet-komennon avulla. Jos **Käynnistä AzureApplicationGateway** onnistui edellisessä vaiheessa, *tilan* on oltava käynnissä ja *Vip* ja *DnsName* pitäisi olla kelvollisia arvoja.

Seuraavassa esimerkissä yhdyskäytävän sovellus, joka on enintään, käyttöön, ja valmis ottamaan liikenne tarkoitettu `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Poista sovellus-yhdyskäytävä

Voit poistaa sovelluksen yhdyskäytävän seuraavasti:

1. **Lopeta AzureApplicationGateway** cmdlet-komennon avulla voit lopettaa yhdyskäytävän.
2. **Poista AzureApplicationGateway** cmdlet-komennon avulla voit poistaa yhdyskäytävän.
3. Varmista, että yhdyskäytävä on poistettu **Get-AzureApplicationGateway** cmdlet-komennolla.

Seuraavassa esimerkissä **Stop AzureApplicationGateway** cmdlet-komento ensimmäisellä rivillä tulosteen perään.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Kun sovellus yhdyskäytävä on pysäytetty-tilaan, **Poista AzureApplicationGateway** cmdlet-komennon avulla voit poistaa palvelun.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Jos haluat varmistaa, että palvelu on poistettu, voit käyttää **Get-AzureApplicationGateway** cmdlet-komento. Tämä vaihe ei tarvita.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat määrittää SSL purku-kohdassa [Configure sovelluksen yhdyskäytävän SSL purku](application-gateway-ssl.md).

Jos haluat määrittää sovelluksen yhdyskäytävän sisäinen kuormituksen käytettäväksi, katso [luominen sovelluksen yhdyskäytävän sisäinen kuormituksen (ILB) kanssa](application-gateway-ilb.md).

Jos haluat lisätietoja ladata yleinen vastatilin asetukset-kohdassa:

- [Azure kuormituksen](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure liikenteen hallinta](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png