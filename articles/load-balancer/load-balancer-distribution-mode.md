<properties
   pageTitle="Määrittää kuormituksen jakauman tilassa | Microsoft Azure"
   description="Azure kuormituksen tasauspalvelun jakauman tilassa tukemaan lähde-IP-affiniteetti määrittäminen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="configure-the-distribution-mode-for-load-balancer"></a>Kuormituksen jakauman tilan määrittäminen

## <a name="hash-based-distribution-mode"></a>Hash-pohjainen jakauman tila

Oletus-jakauman algoritmin on 5-monikon (lähde-IP-Lähdeportti, kohde-IP-Kohdeportti-protokolla tyyppi) hajautuksen yhdistäminen käytettävissä palvelimien liikenteen. Se on tahmeutta vain transport istunnosta. Pakettien samassa istunnossa ohjataan samassa palvelinkeskuksen IP (DIP)-esiintymässä kuormitus tasataan päätepisteen takana. Kun asiakkaan aloittaa uuden istunnon saman lähde-IP-lähdeportin muuttuu, ja voit siirtyä eri DIP endpoint liikenne aiheuttaa.

![kuormituksen Hash perusteella](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Kuva 1-5-monikon jakauman.

## <a name="source-ip-affinity-mode"></a>Lähteen IP affiniteetti tila

On nimeltään lähde-IP affiniteetti (tunnetaan myös nimellä istunnon affiniteetti tai asiakkaan IP-affiniteetti) toisessa jakauman-tilassa. Azure kuormituksen voi määrittää yhdistäminen käytettävissä palvelimien liikenteen 2-monikon (lähde-IP, kohde-IP) tai 3-monikon (lähde-IP-kohde-IP-Protocol) avulla. Lähde-IP-affiniteetti käyttämällä saman asiakkaan tietokoneelta yhteydet siirtyy saman DIP päätepiste.

Seuraavassa kaaviossa on kuvattu 2 monikon määritys. Huomaa, 2-monikon suorittamistavan kuormituksen tietokoneella näennäismuistin 1 (VM1), joka on sitten varmuuskopioida VM2 ja VM3 kautta.

![istunnon affiniteetti](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Kuva 2-2-monikon jakauman.

Lähde-IP-affiniteetti ratkaisee yhteensopivuusongelman Azure kuormituksen ja Remote Desktop (RD) yhdyskäytävän välillä. Voit nyt luoda RD yhdyskäytävä-klusterin yksittäisen pilvipalvelussa.

Toisen Käytä tapaus-skenaario on media Lataa, jossa tietojen siirto tapahtuu UDP kautta, mutta ohjausobjektin tasossa saavutetaan TCP:

- Asiakkaan ensin aloittaa TCP-istunnon osoittamaan kuormitus tasataan osoitteeseen, saa ohjataan tietyn DIP kanavan jätetään aktiivisen yhteyden kunnon valvonta
- Uuden UDP-istunnon saman asiakastietokone on aloitettu saman kuormitus tasataan julkisen päätepisteen, oletuksena tähän on, että tässä yhteydessä myös ohjataan saman DIP päätepisteen kuin edellinen TCP-yhteyden niin, että media Lataa voidaan suorittaa on hyvin siirtonopeuden säilyttäen myös ohjausobjektin kanavan kautta TCP.

>[AZURE.NOTE] Kun kuormituksen määrittäminen muuttuu (poistaminen tai lisääminen virtual machine)-asiakasohjelman pyynnöt jakautumisen on recomputed. Ei määräytyvät uudet yhteydet aiemmin asiakkaiden lopetus samaan palvelimeen. Lisäksi lähde-IP käyttämällä affiniteetti jakauman tila-saattaa aiheuttaa liikenteen varianssit jakautuminen. Välityspalvelimet takana asiakkaat voi tarkastella yksilöllinen asiakas-sovelluksina.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Kuormituksen määrittäminen lähde-IP affiniteetti asetukset

Näennäiskoneiden voit käyttää PowerShell aikakatkaisun asetusten muuttaminen:

Azure päätepisteen Virtual Machine ja kuormituksen tasauspalvelun jakauman tilan määrittäminen

    Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM

LoadBalancerDistribution voidaan määrittää sourceIP 2-monikon (lähde-IP, kohde-IP) kuormituksen tasaamisen, sourceIPProtocol kuormituksen tasaamisen 3-monikon (lähde-IP-kohde-IP-protokollan) tai ei mitään, voit halutessasi 5-monikon kuormituksen tasaamisen oletustoiminnon.

Käytä noutaa päätepisteen kuormituksen tasauspalvelun jakauman tila-asetuksia seuraavasti:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

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
    LoadBalancerDistribution : sourceIP

Jos LoadBalancerDistribution-elementtiä ei ole Azure kuormituksen käyttää oletusarvon 5-monikon algoritmin.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Määritä kuormitus tasataan päätepisteen tietylle jakauman tila

Jos päätepisteet ovat kuormitus tasataan päätepisteen joukon, jakauman tila on määritettävä kuormitus tasataan päätepisteen määrittäminen:

    Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Cloud palvelun määritykset muuttamassa jakauman.

Päivitä pilvipalvelussa sijaitsevaan avulla voidaan hyödyntää Azure-SDK .NET 2,5 (vapautettavan marraskuun). Päätepisteen asetusten pilvipalveluihin tehdään .csdef. Jotta voit päivittää pilvipalveluihin käyttöönottoa kuormituksen tasauspalvelun jakauman-tilassa, käyttöönoton päivitys vaaditaan.
Tässä on esimerkki .csdef muutokset päätepisteen asetukset:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
      </Endpoints>
    </WorkerRole>
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

## <a name="api-example"></a>API-Esimerkki

Voit määrittää kuormituksen tasauspalvelun jakauman service API-hallinnan avulla. Varmista, että voit lisätä `x-ms-version` otsikko on määritetty versio `2014-09-01` tai uudempi versio.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Päivitä käyttöönoton määritetyn kuormituksen joukon määritys

#### <a name="request-example"></a>Esimerkki pyytäminen

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet    x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

LoadBalancerDistribution arvo voi olla sourceIP 2 monikon affiniteetti, 3 monikon affiniteetti sourceIPProtocol tai ei mitään (ei affiniteetti. Toisin sanoen 5-monikon)

#### <a name="response"></a>Vastaus

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Seuraavat vaiheet

[Sisäinen kuormituksen tasauspalvelun yleiskatsaus](load-balancer-internal-overview.md)

[Aloita selainpohjainen vastakkaisten kuormituksen tasauspalvelun asetusten määrittäminen](load-balancer-get-started-internet-arm-ps.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
