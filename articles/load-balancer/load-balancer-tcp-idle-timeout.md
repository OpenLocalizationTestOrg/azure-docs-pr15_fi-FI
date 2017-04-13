<properties
   pageTitle="Määrittää kuormituksen tasauspalvelun TCP aikakatkaisun | Microsoft Azure"
   description="Kuormituksen tasauspalvelun TCP aikakatkaisun asetusten määrittäminen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>Azure kuormituksen TCP aikakatkaisun asetusten määrittäminen

Sen alkuperäisessä kokoonpanossa Azure kuormituksen on aikakatkaisun-asetukset 4 minuuttia. Jos ajan, on pidempi kuin aikakatkaisun, ei ole takaa, että TCP- tai HTTP-istunnon määritetään asiakkaan ja pilvipalvelussa sijaitsevaan välillä.

Kun yhteys on suljettu, asiakassovellus voi tulla seuraava virhesanoma: "taustalla oleva yhteys on suljettu: yhteys, joka on tarkoitus pidettävän avoinna palvelin on suljettu."

Yleisiä kannattaa käyttää säilyttämisen TCP. Käytäntö pitää yhteyttä aktiivinen pidemmän. Lisätietoja näistä [.NET esimerkkejä](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx). Kun säilyttämisen käytössä, paketit lähetetään, käyttämättömänä yhteydessä. Nämä säilyttämisen pakettien varmistaa, että aikakatkaisuarvoa ei koskaan saavutettu yhteys määritetään pitkään.

Tämä asetus toimii vain saapuvia yhteyksiä. Menetä yhteyden säilyttämisen TCP ja pienempi kuin aikakatkaisun asetukset aikavälin määrittäminen tai käyttämättömänä aikakatkaisun arvoa. Tukemaan tällaiset skenaariot Olemme lisänneet määritettäviä aikakatkaisun tuki. Voit nyt määrittää sen kestoksi 4 – 30 minuuttia.

TCP säilyttämisen toimii hyvin skenaariot, jossa varauksen elinkaaren ei ole rajoituksen. Ei ole suositeltavaa mobile-sovellusten. TCP säilyttämisen mobile-sovelluksen avulla voit Tyhjennä laitteen varauksen nopeammin.

![TCP aikakatkaisu](./media/load-balancer-tcp-idle-timeout/image1.png)

Seuraavissa kohdissa kuvataan näennäiskoneiden aikakatkaisun asetusten muuttaminen ja cloud services.

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>TCP-aikakatkaisun määrittäminen esiintymän tason julkiseen IP 15 minuuttia

    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes`on valinnainen. Jos sitä ei määritetä, aikakatkaisun oletusarvo on 4 minuuttia. Hyväksyttävä aikakatkaisu-alue on 4 – 30 minuuttia.

## <a name="set-the-idle-timeout-when-creating-an-azure-endpoint-on-a-virtual-machine"></a>Määritä aikakatkaisun luotaessa Azure päätepisteen virtual tietokoneeseen

Jos haluat muuttaa päätepisteen aikakatkaisuasetusta, toimimalla seuraavasti:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Noutaa aikakatkaisun asetusten, käytä seuraavaa komentoa:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15

## <a name="set-the-tcp-timeout-on-a-load-balanced-endpoint-set"></a>Määrittää TCP-aikakatkaisun kuormituksen päätepisteen määrittäminen

Jos päätepisteet ovat kuormituksen päätepisteen määrittäminen, TCP-aikakatkaisun on määritettävä kuormituksen päätepiste on määritetty. Esimerkki:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## <a name="change-timeout-settings-for-cloud-services"></a>Pilvipalveluihin aikakatkaisun asetusten muuttaminen

Voit päivittää oman pilvipalvelussa Azure SDK-paketissa. Tee päätepisteen asetuksia pilvipalveluihin .csdef-tiedosto. TCP-aikakatkaisun pilvipalveluun käyttöönottoa varten päivittämiseen tarvitaan käyttöönotto-päivitys. Poikkeuksen on, jos TCP-aikakatkaisun määritetään vain julkiseen IP. Julkiseen IP-asetukset ovat .cscfg-tiedostossa, ja voit päivittää ne käyttöönoton Update-sivuston ja päivitys.

Päätepisteasetukset .csdef muutokset ovat seuraavat:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Julkiseen IP-osoitteet aikakatkaisu-asetukselle .cscfg muutokset ovat:

    <NetworkConfiguration>
      <VirtualNetworkSite name="VNet"/>
      <AddressAssignments>
        <InstanceAddress roleName="VMRolePersisted">
        <PublicIPs>
          <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
        </PublicIPs>
        </InstanceAddress>
      </AddressAssignments>
    </NetworkConfiguration>

## <a name="rest-api-example"></a>REST API-Esimerkki

Voit määrittää TCP-aikakatkaisun service API-hallinnan avulla. Varmista, että `x-ms-version` otsikko on määritetty versio `2014-06-01` tai uudempi versio. Päivitä määritetyn kuormituksen syötteen päätepisteet kaikki näennäiskoneiden käyttöönoton määritys.

### <a name="request"></a>Pyyntö

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>

### <a name="response"></a>Vastaus

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
        <LocalPort>local-port-number</LocalPort>
        <Port>external-port-number</Port>
        <LoadBalancerProbe>
          <Path>path-of-probe</Path>
          <Port>port-assigned-to-probe</Port>
          <Protocol>probe-protocol</Protocol>
          <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
          <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
        </LoadBalancerProbe>
        <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
        <Protocol>endpoint-protocol</Protocol>
        <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
        <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
        <EndpointACL>
          <Rules>
            <Rule>
              <Order>priority-of-the-rule</Order>
              <Action>permit-rule</Action>
              <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
              <Description>description-of-the-rule</Description>
            </Rule>
          </Rules>
        </EndpointACL>
      </InputEndpoint>
    </LoadBalancedEndpointList>

## <a name="next-steps"></a>Seuraavat vaiheet

[Sisäinen kuormituksen tasauspalvelun yleiskatsaus](load-balancer-internal-overview.md)

[Aloita Internetiin yhteydessä oleva-kuormituksen määrittäminen](load-balancer-get-started-internet-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)
