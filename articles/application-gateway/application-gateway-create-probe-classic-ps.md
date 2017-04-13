<properties
   pageTitle="Luo mukautettu näytteenottimen sovelluksen Gatewayn PowerShell-toiminnolla perinteinen käyttöönoton mallin | Microsoft Azure"
   description="Mukautetun näytteenottimen luominen sovelluksen Gatewayn PowerShell-toiminnolla perinteinen käyttöönotto-mallissa"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Luo mukautettu näytteenottimen Azure sovelluksen Gateway (perinteinen) PowerShell-toiminnolla

> [AZURE.SELECTOR]
- [Azure portal](application-gateway-create-probe-portal.md)
- [Azure Resurssienhallinta PowerShell](application-gateway-create-probe-ps.md)
- [Azure perinteinen PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Sovelluksen Gatewayn luominen

Voit luoda yhdyskäytävän sovelluksen seuraavasti:

1. Luo sovelluksen yhdyskäytävän yritysresurssi.
2. Luo XML-kokoonpanotiedosto tai kokoonpano-objekti.
3. Vahvista juuri luomasi sovelluksen yhdyskäytävän resurssin määritykset.

### <a name="create-an-application-gateway-resource"></a>Luo sovellus-yhdyskäytävän resurssi

Voit luoda yhdyskäytävän käyttämällä **Uutta AzureApplicationGateway** -cmdlet-arvojen korvaaminen omalla. Laskutuksen yhdyskäytävä ei käynnisty tässä vaiheessa. Laskutus alkaa myöhemmin, kun yhdyskäytävä on käynnistetty.

Seuraavassa esimerkissä luodaan sovelluksen yhdyskäytävän nimeltä "testvnet1" ja "aliverkon 1"-niminen aliverkon virtual verkkoa käyttäen.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Vahvista, että yhdyskäytävä on luotu, voit **Saada AzureApplicationGateway** cmdlet-komento.

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  *InstanceCount* oletusarvo on 2 suurin arvo on 10. *GatewaySize* oletusarvo on Normaali. Voit valita pieni, Normaali tai suuri.

 *VirtualIPs* ja *DnsName* ovat näkyvissä kuin tyhjä koska yhdyskäytävä ei ole vielä aloitettu. Nämä luodaan, kun yhdyskäytävä on käytössä-tilassa.

## <a name="configure-an-application-gateway"></a>Sovelluksen yhdyskäytävän asetusten määrittäminen

Voit määrittää sovelluksen yhdyskäytävän käyttämällä XML- tai kokoonpano-objekti.

## <a name="configure-an-application-gateway-by-using-xml"></a>Määrittää sovelluksen yhdyskäytävän käyttämällä XML

Seuraavassa esimerkissä käytetään XML-tiedostoon kaikki sovelluksen yhdyskäytävän asetusten määrittäminen ja vahvista sovelluksen yhdyskäytävän resurssi.  

### <a name="step-1"></a>Vaihe 1

Kopioi seuraava teksti Muistioon.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Muokkaa sulkumerkkien määritysten kohteiden arvoja. Tallenna tiedosto tunniste .xml.

Seuraavassa esimerkissä esitetään käyttämisestä määritystiedoston määrittämään sovelluksen yhdyskäytävää, lataamisen saldo HTTP-liikenteen julkiseen porttiin 80 ja lähettää verkkoliikennettä taustatietokantaan portti 80 kaksi IP-osoitetta käyttämällä mukautettuja näytteenottimen välillä.

>[AZURE.IMPORTANT] Protokollan kohteen Http tai Https kirjainkoko on merkitsevä.

Uuden kohteen määritys \<tutkia\> lisätään määrittää mukautetun keräysputkien.

Määritysten parametrit ovat seuraavat:

- **Nimi** - mukautettu näytteenottimen viitteen nimi.
- **Protocol (protokolla)** -protokolla (mahdolliset arvot ovat HTTP ja HTTPS).
- **Host (isäntä)** ja **polku** - täydellinen URL-polku, joka suoritetaan sovelluksen yhdyskäytävän esiintymän terveydentilasta mukaan. Esimerkiksi jos sinulla on sivuston http://contoso.com/, valitse mukautettu keräysputken voidaan määrittää "http://contoso.com/path/custompath.htm", näytteenottimen tarkistuksien on onnistunut HTTP-vastaus.
- **Aikaväli** - määrittää näytteenottimen aikavälin tarkistukset sekunnin kuluttua.
- **Aikakatkaisu** - määrittää HTTP-vastauksen valintaruudun näytteenottimen aikakatkaisun.
- **UnhealthyThreshold** - epäonnistui HTTP-vastaukset tarvitaan taustatietokantaan esiintymän kuin merkitsevän numeron *perusasemassa*.

Näytteenottimen nimi viitataan <BackendHttpSettings> kokoonpanon määrittäminen mitä taustatietokantaan resurssivarantoon käyttää mukautettuja näytteenottimen asetuksia.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Lisää mukautettu näytteenottimen-määrityksen aiemmin sovelluksen yhdyskäytävän

Nykyiset määritykset sovelluksen yhdyskäytävän muuttaminen edellyttää kolme vaihetta: Hae nykyisessä XML-määritystiedoston, muokata on mukautettu näytteenottimen ja määrittää sovelluksen yhdyskäytävän uusia XML-asetuksia.

### <a name="step-1"></a>Vaihe 1

XML-tiedoston avulla get-AzureApplicationGatewayConfig hakeminen Tämä vie määritykset XML voidaan muokata Lisää näytteenottimen-asetus.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Vaihe 2

Avaa XML-tiedostoa tekstieditorissa. Lisää `<probe>` osan jälkeen `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

XML backendHttpSettings-osassa Lisää näytteenottimen nimi, kuten seuraavassa esimerkissä:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

Tallenna XML-tiedosto.

### <a name="step-3"></a>Vaihe 3

Päivitä sovelluksen yhdyskäytävän määritys uuden XML-tiedoston avulla **Määrittäminen AzureApplicationGatewayConfig**. Tämä päivittää sovelluksen yhdyskäytävän uudet määritykset.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat määrittää purku Secure Sockets Layer (SSL)-kohdassa [Configure sovelluksen yhdyskäytävän SSL purku](application-gateway-ssl.md).

Jos haluat määrittää sovelluksen yhdyskäytävän sisäinen kuormituksen käytettäväksi, katso [luominen sovelluksen yhdyskäytävän sisäinen kuormituksen (ILB) kanssa](application-gateway-ilb.md).
