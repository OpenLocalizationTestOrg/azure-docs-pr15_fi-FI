<properties
   pageTitle="Luo sisäinen kuormituksen cloud Services perinteinen käyttöönoton mallin | Microsoft Azure"
   description="Opettele luomaan PowerShellin perinteinen käyttöönoton mallin sisäinen kuormituksen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Luomisesta sisäinen kuormituksen (perinteinen) pilvipalveluihin

[AZURE.INCLUDE [load-balancer-get-started-ilb-classic-selectors-include.md](../../includes/load-balancer-get-started-ilb-classic-selectors-include.md)]

<BR>

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Lue, miten [Resurssienhallinta-mallin seuraavasti](load-balancer-get-started-ilb-arm-ps.md).


## <a name="configure-internal-load-balancer-for-cloud-services"></a>Määritä sisäisen kuormituksen pilvipalveluihin

Sisäinen kuormituksen tuetaan näennäiskoneiden ja pilvipalveluihin. Sisäinen kuormituksen tasauspalvelun päätepisteen luotu pilvipalvelussa aluekohtaiset virtual verkon ulkopuolella ovat käytettävissä vain pilvipalvelussa sisällä.

Sisäinen kuormituksen tasauspalvelun määritys on määritettävä ensimmäisen käyttöönoton pilvipalvelussa, luonnin aikana otoksessa alla kuvatulla.

>[AZURE.IMPORTANT] Suorittaa seuraavia ohjeita edellytyksenä on on jo luotu cloud käyttöönottoa varten virtual verkon. Tarvitset virtual verkkonimi ja Luo sisäinen kuormituksen tasaamisen aliverkon nimi.

### <a name="step-1"></a>Vaihe 1

Avaa Visual Studiossa cloud käyttöönoton palvelun määritystiedosto (.cscfg) ja lisää seuraavan osan luomiseen sisäinen kuormituksen tasaamisen edellisen kohdan "`</Role>`" verkon määritysten kohde.




    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="name of the load balancer">
          <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>


Lisääminen verkko-kokoonpanotiedosto Jos haluat näyttää, miltä asiakirja näyttää arvot. Esimerkissä oletetaan luomasi nimeltä "test_vnet" test_subnet ja staattinen IP 10.0.0.4 aliverkon-10.0.0.0/24 aliverkon. Kuormituksen nimetään testLB.

    <NetworkConfiguration>
      <LoadBalancers>
        <LoadBalancer name="testLB">
          <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
        </LoadBalancer>
      </LoadBalancers>
    </NetworkConfiguration>

Saat lisätietoja kuormituksen tasauspalvelun rakennetta [Lisää kuormituksen](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Vaihe 2


Muuta sisäinen kuormituksen tasaamisen päätepisteet lisääminen palvelun määritys (.csdef)-tiedosto. Rooli esiintymää on luotu, tällä hetkellä palvelun määritystiedoston Lisää rooli esiintymät sisäinen kuormituksen tasaamisen.


    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
      </Endpoints>
    </WorkerRole>

Seuraavat samat arvot kuin edellä olevassa esimerkissä lisääminen arvot palvelun määritystiedoston.

    <WorkerRole name=WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
      <Endpoints>
        <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
      </Endpoints>
    </WorkerRole>

Verkkoliikenne on kuormitus tasataan portti 80 käytön pyynnöt työntekijän rooli esiintymät myös porttiin 80 lähettäminen testLB kuormituksen avulla.


## <a name="next-steps"></a>Seuraavat vaiheet

[Lähde-IP-affiniteetti käyttämällä kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)