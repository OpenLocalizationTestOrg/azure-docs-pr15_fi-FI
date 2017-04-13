<properties 
   pageTitle="Luo ja Määritä sovelluksen yhdyskäytävän ja sisäinen kuormituksen (ILB) Virtual verkossa | Microsoft Azure"
   description="Tällä sivulla on ohjeita ja sisäinen kuormituksen saapuva päätepisteen yhdyskäytävän Azure-sovelluksen määrittäminen"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Luo sovelluksen yhdyskäytävän sisäinen kuormituksen (ILB)

> [AZURE.SELECTOR]
- [Azure perinteinen vaiheet](application-gateway-ilb.md)
- [Resurssienhallinta Powershell-ohjeita](application-gateway-ilb-arm.md)

Sovelluksen yhdyskäytävän voi määrittää selainpohjainen vastakkaisten virtual IP- tai sisäinen muuntyyppiselle eikä julkaista Internetissä, eli sisäinen kuormituksen tasauspalvelun (ILB) päätepiste. Määrittää yhdyskäytävän kanssa ILB on hyötyä sisäinen liiketoiminta-sovelluksissa eikä julkaista internet. Kannattaa myös käyttää palvelut ja tasojen monitasoisten sovelluksesta, joka sijaitsee eikä julkaista internet security reunaa, mutta edellyttävät edelleen PYÖRISTÄ-funktiota Mikko kuormituksen jakauman, istunnon tahmeutta tai SSL-tilauksen. Tässä artikkelissa käydään läpi vaiheet ja määrittää yhdyskäytävän sovelluksen ILB.

## <a name="before-you-begin"></a>Ennen aloittamista

1. Asenna uusin versio käyttäminen WWW-ympäristö asennusohjelma Azure PowerShellin cmdlet-komennot. Voit ladata ja asenna uusin versio **Windows PowerShell** -osasta [lataussivulle](https://azure.microsoft.com/downloads/).
2. Varmista, että sinulla on kelvollinen toimimasta virtual verkosta.
3. Varmista, että Taustajärjestelmä palvelinten virtual verkossa tai IP/VIP julkisen määritetty kanssa.


Voit luoda yhdyskäytävän sovelluksen, suorita seuraavat vaiheet järjestyksessä. 

1. [Sovelluksen Gatewayn luominen](#create-a-new-application-gateway)
2. [Yhdyskäytävän määrittäminen](#configure-the-gateway)
3. [Yhdyskäytävän määrittäminen](#set-the-gateway-configuration)
4. [Käynnistä yhdyskäytävän](#start-the-gateway)
4. [Tarkista yhdyskäytävän](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Luo sovelluksen yhdyskäytävän:

**Luo yhdyskäytävä**-käyttö `New-AzureApplicationGateway` cmdlet-arvojen korvaaminen omalla. Huomaa, että yhdyskäytävän laskutuksen ei käynnisty tässä vaiheessa. Laskutus alkaa myöhemmin, kun yhdyskäytävä on käynnistetty.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Vahvista** , että yhdyskäytävä on luotu, voit käyttää `Get-AzureApplicationGateway` cmdlet-komento. 

Esimerkki, *kuvaus*, *InstanceCount*ja *GatewaySize* ovat valinnaisia parametreja. *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Pienet ja suuret on muita käytettävissä olevat arvot. *VIP* ja *DnsName* ovat näkyvissä kuin tyhjä koska yhdyskäytävä ei ole vielä aloitettu. Nämä luodaan, kun yhdyskäytävä on käytössä-tilassa. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Yhdyskäytävän määrittäminen

Sovelluksen-yhdyskäytävä-määritys sisältää useita arvoja. Arvot on sidottu yhteen, kun haluat käyttää määritykset.
 
Arvot ovat:

- **Taustajärjestelmä palvelimen resurssivarantoon:** Taustajärjestelmä-palvelimien IP-osoitteiden luettelo. IP-osoitteiden luettelossa tulee kuulua joko VNet aliverkon tai pitäisi olla julkiseen IP-tai VIP. 
- **Taustajärjestelmä resurssivarantoon palvelinasetukset:** Jokaisen varanto on asetusten, kuten portin, protokolla ja evästeiden perustuva affiniteetti. Nämä asetukset on liitetty resurssivarantoon, ja niitä käytetään varannon kaikki palvelimiin.
- **Edusta-portin:** Tämä portti on avattu sovelluksen yhdyskäytävän Julkinen portti. Liikenne käynnit portin ja sitten uudelleenohjataan Taustajärjestelmä-palvelimia.
- **Listener:** Kuuntelun edusta-portin-protokolla (Http tai Https-kirjainkoko on merkitsevä), ja SSL-varmenteen nimi (Jos määrittäminen SSL purku). 
- **Säännön:** Säännön sitoo kuuntelua ja Taustajärjestelmä palvelimen resurssivarantoon ja määrittää, mitkä Taustajärjestelmä palvelimen resurssivarantoon liikenne olisi ohjautuu, kun se käynnit tietyn listener. *Tavallinen* sääntö on tällä hetkellä tueta. *Tavallinen* sääntö on pyöreän pöydän kuormituksen jakauman.

Voit luoda kokoonpanosi luomalla kokoonpano-objekti tai määritysten XML-tiedoston avulla. Muodostaa kokoonpanon määritys XML-tiedoston avulla, käytä alla otosten.



Ota seuraavat seikat huomioon:


- *FrontendIPConfigurations* osan kuvataan kannalta määrittäminen sovelluksen yhdyskäytävä ILB ILB tiedot. 

- "Yksityiseksi" on asetettava edusta-IP- *tyyppi*

- Jossa yhdyskäytävän vastaanottaa liikenne haluamasi sisäinen IP on asetettava *StaticIPAddress* . Huomaa, että *StaticIPAddress* elementti on valinnainen. Jos et neljänneksen käytettävissä sisäinen IP käyttöön aliverkosta valitaan. 

- *Nimen* osan *FrontendIPConfiguration* arvoa käytetään HTTPListener *FrontendIP* osaan nähdäksesi FrontendIPConfiguration.

 **Määritysten XML-esimerkki**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Yhdyskäytävän määrittäminen

Seuraavaksi määrität sovelluksen yhdyskäytävän. Voit käyttää `Set-AzureApplicationGatewayConfig` cmdlet-kokoonpano-objekti tai määritysten XML-tiedosto. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Käynnistä yhdyskäytävän

Kun yhdyskäytävä on määritetty, `Start-AzureApplicationGateway` cmdlet-komento, Käynnistä yhdyskäytävän. Sovelluksen yhdyskäytävän Laskutus alkaa, kun yhdyskäytävä on aloitettu. 


> [AZURE.NOTE] `Start-AzureApplicationGateway` Cmdlet-komento saattaa kestää jopa 15-20 minuuttia. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Tarkista yhdyskäytävän tila

Käytä `Get-AzureApplicationGateway` cmdlet-komento Tarkista yhdyskäytävän tila. Jos *Käynnistä AzureApplicationGateway* onnistui edellisessä vaiheessa, siinä on oltava *käynnissä*ja Vip ja DnsName pitäisi olla kelvollisia arvoja. Tämä malli näkyy cmdlet ensimmäisen rivin jäljessä tulos. Tässä esimerkissä yhdyskäytävä on käynnissä, ja se on ryhtyä kirjoittamaan liikenne. 

> [AZURE.NOTE] Sovelluksen yhdyskäytävän määritetään hyväksymään liikenteen 10.0.0.10 tässä esimerkissä määritetyn ILB päätepisteelle.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Seuraavat vaiheet


Jos haluat lisätietoja ladata yleinen vastatilin asetukset-kohdassa:

- [Azure kuormituksen](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure liikenteen hallinta](https://azure.microsoft.com/documentation/services/traffic-manager/)
