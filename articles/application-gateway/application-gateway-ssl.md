<properties
   pageTitle="Määrittää SSL purku sovelluksen-yhdyskäytävän käyttämällä perinteinen käyttöönoton | Microsoft Azure"
   description="Tässä artikkelissa on ohjeita voit luoda yhdyskäytävän sovelluksen SSL purku Azure perinteinen käyttöönotto-mallin avulla."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Määritä SSL purku sovelluksen-yhdyskäytävän perinteinen käyttöönotto-mallin avulla

> [AZURE.SELECTOR]
-[Azure portal](application-gateway-ssl-portal.md)
-[Resurssienhallinta PowerShellin Azure](application-gateway-ssl-arm.md)
-[Perinteinen PowerShellin Azure](application-gateway-ssl.md)

Azure sovelluksen yhdyskäytävän voidaan määrittää Lopeta välttämiseksi kallista SSL salauksen tehtävät ilmenee, WWW-klusterin yhdyskäytävässä Secure Sockets Layer (SSL)-istunto. SSL-purku yksinkertaistaa myös edusta palvelinasetukset ja web-sovelluksen hallinta.

## <a name="before-you-begin"></a>Ennen aloittamista

1. WWW-ympäristö asennusohjelma Azure PowerShellin cmdlet-komennot uusimman version asentamalla. Voit ladata ja asenna uusin versio **Windows PowerShell** -osiosta, [Lataa sivu](https://azure.microsoft.com/downloads/).
2. Varmista, että sinulla on kelvollinen aliverkon toimimasta virtual verkosta. Varmista, että cloud ominaisuuksissa eikä näennäiskoneiden on käytössä aliverkon. Sovelluksen gateway on oltava erillisenä VPN-aliverkon.
3. Palvelimet, joilla voit määrittää sovelluksen yhdyskäytävän käyttämään on oltava tai määrittänyt niiden päätepisteet luotu virtual verkossa tai julkiseen IP-tai VIP kanssa.

Määrittämiseksi sovelluksen yhdyskäytävän SSL purku Tee luetellussa järjestyksessä seuraavasti:

1. [Sovelluksen Gatewayn luominen](#create-an-application-gateway)
2. [SSL-varmenteita lataaminen](#upload-ssl-certificates)
3. [Yhdyskäytävän määrittäminen](#configure-the-gateway)
4. [Yhdyskäytävän määrittäminen](#set-the-gateway-configuration)
5. [Käynnistä yhdyskäytävän](#start-the-gateway)
6. [Tarkista yhdyskäytävän tila](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Sovelluksen Gatewayn luominen

Voit luoda yhdyskäytävän käyttämällä **Uutta AzureApplicationGateway** -cmdlet-arvojen korvaaminen omalla. Laskutuksen yhdyskäytävä ei käynnisty tässä vaiheessa. Laskutus alkaa myöhemmin, kun yhdyskäytävä on käynnistetty.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Vahvista, että yhdyskäytävä on luotu, voit **Saada AzureApplicationGateway** cmdlet-komento.

Esimerkki, *kuvaus*, *InstanceCount*ja *GatewaySize* ovat valinnaisia parametreja. *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Pienet ja suuret on muita käytettävissä olevat arvot. *VirtualIPs* ja *DnsName* ovat näkyvissä kuin tyhjä koska yhdyskäytävä ei ole vielä aloitettu. Nämä arvot luodaan, kun yhdyskäytävä on käytössä-tilassa.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>SSL-varmenteita lataaminen

**Lisää AzureApplicationGatewaySslCertificate** avulla voit ladata sovelluksen yhdyskäytävän palvelinvarmennetta *pfx* -muodossa. Varmenteen nimi on käyttäjän valitsemia nimi, ja se on oltava yksilöllinen sovelluksen yhdyskäytävän. Tämän todistuksen on tarkoitettu kaikkien varmenteen hallintatoiminnot sovelluksen yhdyskäytävän nimeä.

Seuraavassa esimerkissä kuvataan-cmdlet-komennolla korvaa arvot otoksessa omalla.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Vahvista seuraavaksi varmenteen lataus. Käytä komentosovelmaa **Get-AzureApplicationGatewayCertificate** .

Tämä malli näkyy cmdlet ensimmäisen rivin jäljessä tulos.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Sertifikaatin salasana on oltava 4-12 merkkiä, kirjaimia tai numeroita. Erikoismerkkejä ei hyväksytä.

## <a name="configure-the-gateway"></a>Yhdyskäytävän määrittäminen

Sovelluksen-yhdyskäytävä-määritys sisältää useita arvoja. Arvot on sidottu yhteen, kun haluat käyttää määritykset.

Arvot ovat:

- **Taustatietokantaan palvelimen resurssivarantoon:** Taustatietokannan palvelimien IP-osoitteiden luettelo. IP-osoitteiden luettelossa tulee kuulua joko VPN-aliverkon tai pitäisi olla julkiseen IP-tai VIP.
- **Taustatietokantaan resurssivarantoon palvelinasetukset:** Jokaisen varanto on asetusten, kuten portin, protokolla ja evästeiden perustuva affiniteetti. Nämä asetukset on liitetty resurssivarantoon, ja niitä käytetään varannon kaikki palvelimiin.
- **Edusta portti:** Tämä portti on Julkinen portti, joka on avattu sovelluksen yhdyskäytävän. Liikenne käynnit portin ja sitten uudelleenohjataan taustatietokantaan-palvelimia.
- **Listener:** Kuuntelun edusta portti-protokolla (Http tai Https-arvot ovat kirjainkoko on merkitsevä), ja SSL-varmenteen nimi (Jos määrittäminen SSL purku).
- **Säännön:** Säännön sitoo kuuntelua ja taustatietokantaan server-ryhmän ja määrittää, mitkä taustatietokantaan palvelimen resurssivarantoon liikenne olisi ohjautuu, kun se käynnit tietyn listener. *Tavallinen* sääntö on tällä hetkellä tueta. *Tavallinen* sääntö on pyöreän pöydän kuormituksen jakauman.

**Lisää määritykset**

SSL-varmenteita kokoonpanossa **HttpListener** protokollan vaihdetaan *Https* (kirjainkoko on merkitsevä). **SslCert** -osa lisätään **HttpListener** arvoksi on sama nimi kuin käytetty lataaminen edeltävän SSL-varmenteita osan kanssa. Edustatietokannan portin päivitetäänkö 443.

**Käyttöön eväste perustuva affiniteetti**: yhdyskäytävän sovellus voi määrittää varmistaa, että asiakas istunnosta pyyntö ohjataan aina sama AM WWW-klusterin. Tässä skenaariossa tehdään istunnon eväste, joka sallii liikenne voidaan ohjata asianmukaisesti yhdyskäytävän lisäämisen. Evästeiden perustuva affiniteetti käyttöön määrittää **CookieBasedAffinity** *käytössä* **BackendHttpSettings** elementti.



Voit luoda kokoonpanosi luomalla kokoonpano-objekti tai määritysten XML-tiedoston avulla.
Muodostaa kokoonpanon määritys XML-tiedoston avulla, käytä seuraavassa esimerkissä:

**Määritysten XML-esimerkki**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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

Seuraavaksi voit määrittää sovelluksen yhdyskäytävän. Voit käyttää **Set-AzureApplicationGatewayConfig** cmdlet-komento joko kokoonpano-objekti tai määritysten XML-tiedosto.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Käynnistä yhdyskäytävän

Kun yhdyskäytävä on määritetty, Käynnistä yhdyskäytävän **Käynnistä AzureApplicationGateway** cmdlet-komennon avulla. Sovelluksen yhdyskäytävän Laskutus alkaa, kun yhdyskäytävä on aloitettu.


**Huomautus:** **Käynnistä AzureApplicationGateway** cmdlet-komento saattaa kestää jopa 15-20 minuutin loppuun.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Tarkista yhdyskäytävän tila

Tarkista yhdyskäytävän tila **Get-AzureApplicationGateway** cmdlet-komennon avulla. Jos **Käynnistä AzureApplicationGateway** onnistui edellisessä vaiheessa, *tilan* on oltava käynnissä ja *VirtualIPs* ja *DnsName* pitäisi olla kelvollisia arvoja.

Tässä esimerkissä yhdyskäytävän sovellus, joka on käynnissä, ylös- ja ryhtyä kirjoittamaan liikenne.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Seuraavat vaiheet


Jos haluat lisätietoja ladata yleinen vastatilin asetukset-kohdassa:

- [Azure kuormituksen](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure liikenteen hallinta](https://azure.microsoft.com/documentation/services/traffic-manager/)