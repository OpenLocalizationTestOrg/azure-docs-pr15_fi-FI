<properties
   pageTitle="Internet-perinteinen käyttöönoton mallin käyttäminen pilvipalveluihin kuormituksen vastakkaisten luomisesta | Microsoft Azure"
   description="Selainpohjainen vastakkaisten kuormituksen pilvipalveluihin perinteinen käyttöönotto-mallin luominen"
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
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Selainpohjainen vastakkaisten pilvipalveluihin kuormituksen luomisesta

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [luominen selainpohjainen vastakkaisten kuormituksen Azure resurssien hallinnan avulla](load-balancer-get-started-internet-arm-cli.md).

Pilvipalveluihin kuormituksen tasauspalvelun määritetään automaattisesti, ja se voidaan mukauttaa palvelumallin kautta.

## <a name="create-a-load-balancer-using-the-service-definition-file"></a>Kuormituksen, palvelun määritelmä-tiedoston luominen

.NET 2,5 päivittämään pilvipalvelussa sijaitsevaan Azure SDK avulla voidaan hyödyntää. Päätepisteen asetusten pilvipalveluihin tehdään .csdef [palvelun](https://msdn.microsoft.com/library/azure/gg557553.aspx)määritystiedostoa.

Seuraavassa esimerkissä servicedefinition.csdef tiedoston cloud käyttöönottoa määrittämistavasta:

Tarkistuksen snipet luoma cloud käyttöönoton .csdef tiedoston, voit tarkastella käyttämään portit HTTP porttiin 10000, 10001 ja 10002 ulkoinen päätepiste.


    <ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
    <Endpoints>
        <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
        <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
        <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

        <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

        <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
           <AllocatePublicPortFrom>
              <FixedPortRange min=“10110” max=“10120“  />
           </AllocatePublicPortFrom>
        </InstanceInputEndpoint>
        <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
    </Endpoints>
    </WorkerRole>
    </ServiceDefinition>




## <a name="check-load-balancer-health-status-for-cloud-services"></a>Tarkista kuormituksen tasauspalvelun kunto tila pilvipalveluihin


Seuraavassa on esimerkki kunto näytteenottimen:

        <LoadBalancerProbes>
        <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
        </LoadBalancerProbes>

Kuormituksen yhdistää päätepisteen tiedot ja URL-Osoitteen luominen VM}:80/Probe.aspx, joilla voidaan suorittaa kyselyn palvelun kunto http://{DIP lomakkeen näytteenottimen tiedot.

Palvelu tunnistaa säännöllisiä keräysputkien-sama IP-osoite. Tämä on tulossa isännältä solmun, jossa virtuaalikoneen on käynnissä kunto näytteenottimen pyynnön.
Palvelun on vastata HTTP 200 tilakoodi varten kuormituksen olettaa, että palvelu on kunnossa. Kaikki muut HTTP-tilakoodin (esimerkiksi 503) kestää suoraan virtuaalikoneen kierto ulos.

Näytteenottimen määritelmän ohjaa myös näytteenottimen taajuus. Tässä tapauksessa edellä kuormituksen on tutkitaan päätepisteen jokaisen 5 ja osat. Jos positiivinen vastausta vastaanotetaan 10 sekuntia (kaksi näytteenottimen välein), näytteenottimen oletetaan alaspäin ja virtuaalikoneen otetaan kierto. Vastaavasti jos palvelu on kierto ja positiivinen vastaus saapuu, palvelu on takaisin kierto saman tien. Jos palvelu on vaihteleva luku kunnossa ja perusasemassa välillä, kuormituksen voit päättää luetelmakohtien palvelun kierto uudelleen tilalle, ennen kuin se on kunnossa keräysputkien määrä.

Lisätietoja [kunto näytteenottimen](https://msdn.microsoft.com/library/azure/jj151530.aspx) palvelun määritelmä rakenteen tarkistaminen

## <a name="next-steps"></a>Seuraavat vaiheet

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)

