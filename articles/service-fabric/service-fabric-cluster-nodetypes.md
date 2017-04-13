<properties
   pageTitle="Palvelun kangasta solmutyypit ja AM asteikko joukot | Microsoft Azure"
   description="Kuvataan, miten palvelun kangasta solmutyypit liittyvät AM asteikko joukot ja miten remote muodostaa AM asteikko määrittää esiintymän tai klusterisolmu."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# <a name="the-relationship-between-service-fabric-node-types-and-virtual-machine-scale-sets"></a>Palvelun kangasta solmutyypit ja virtuaalikoneen asteikko joukot välinen suhde

Virtuaalikoneen asteikko ovat Azure Laske resurssin, voit ottaa käyttöön ja hallita näennäiskoneiden kokoelma joukkona. Solmun verkkosivustoasi, joka on määritetty palvelun kangasta klusterissa on määritetty joukkona erillisessä AM asteikko. Solmun kunkin tyyppiselle voi skaalata sitten tai alas itsenäisesti on erilaisia arvojoukkoja avoimet portit ja voi olla eri kapasiteetin arvot.

Seuraavassa näyttökuvassa näkyy klusteriin, joka on kaksi solmu: edusta- ja Taustajärjestelmä.  Solmun kullakin virhelajilla on viisi solmujen.

![Näyttökuva klusteriin, joka on kaksi solmu][NodeTypes]

## <a name="mapping-vm-scale-set-instances-to-nodes"></a>Yhdistäminen AM asteikko määrittäminen esiintymät solmujen

Kuten näet yllä, AM asteikko määrittäminen esiintymät Käynnistä esiintymän 0 ja siirtyy ylöspäin. Numerointi näkyy nimet. Esimerkiksi BackEnd_0 on Taustajärjestelmä AM asteikko joukon 0 esiintymä. Tämän tietyn AM asteikko joukon on viisi esiintymää, BackEnd_0, BackEnd_1, BackEnd_2, BackEnd_3 ja BackEnd_4.

Voit skaalata määrittäminen AM-asteikko-määrittää uuden esiintymän luodaan. Uusi AM asteikko määrittää esiintymän nimi tulee yleensä AM asteikko määritetty nimi + seuraavan esiintymän numeron. Tässä esimerkissä on BackEnd_5.


## <a name="mapping-vm-scale-set-load-balancers-to-each-node-typevm-scale-set"></a>Yhdistettyjen AM asteikko määrittää kuormituksen tasoitusmääritykset kunkin solmun tyyppi/AM asteikko määrittäminen

Jos olet ottanut yhteyttä klusterin-portaalista tai olet käyttänyt otoksen Resurssienhallinta-malli, joka toimittaa, sitten kun saat kaikki resursseja resurssiryhmän luettelo tulee näkyviin kuormituksen tasoitusmääritykset AM asteikko määrittäminen tai solmu mistäkin.

Nimi kuin suunnilleen: **kg -&lt;NodeType nimi&gt;**. Esimerkiksi kg-sfcluster4doc-0, kuten tässä näyttökuvassa:


![Resurssit][Resources]


## <a name="remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node"></a>Etäyhteyksien yhdistäminen AM asteikko määrittää esiintymän tai Klusterisolmu
Solmun verkkosivustoasi, joka on määritetty klusterissa on määritetty joukkona erillisessä AM asteikko.  Solmutyypit skaalata tarkoittaa ylös tai alas itsenäisesti ja voit tehdä eri AM tuotteissa. Toisin kuin yhden esiintymän VMs AM asteikko määrittäminen esiintymät saa omaa virtual IP-osoite. Näin se voi olla hieman hankalaa kun etsit IP-osoite ja portti, jota voit käyttää remote yhdistäminen tiettyä esiintymää.

Näin voit seurata tuttuihin ne.

### <a name="step-1-find-out-the-virtual-ip-address-for-the-node-type-and-then-inbound-nat-rules-for-rdp"></a>Vaihe 1: Selvitä, solmun tyyppiä virtual IP-osoite ja valitse sitten RDP NAT saapuvan liikenteen säännöt

Jotta saat, jotka haluat saapuvan NAT säännöt arvot, jotka on määritetty osana **Microsoft.Network/loadBalancers**resurssin määritelmä.

Siirry-portaalissa kuormituksen tasauspalvelun sivu ja valitse sitten **asetukset**.

![LBBlade][LBBlade]


Napsauta **asetukset**- **NAT saapuvan liikenteen säännöt**. Tämä on nyt saat IP-osoite ja portti, jota voit käyttää remote Yhdistä AM asteikko määrittäminen käynnissä. Alla olevassa näyttökuvassa on **104.42.106.156** ja **3389**

![NATRules][NATRules]

### <a name="step-2-find-out-the-port-that-you-can-use-to-remote-connect-to-the-specific-vm-scale-set-instancenode"></a>Vaihe 2: Etsi portti, johon kaukosäätimen avulla voit muodostaa yhteyden tietyn AM asteikko määrittää esiintymän/solmun ulos

Aiemmin tässä asiakirjassa-voin puhuin ihan siitä, miten AM asteikko määrittäminen esiintymät yhdistäminen solmut. Käytämme, tarkka portin laskemisesta.

Portit varataan nousevaan järjestykseen AM asteikko määrittää esiintymän. Omat esimerkissä edusta-solmu tyypin portit kunkin viisi esiintymät, joiden seuraavasti. Sinun on nyt voit tehdä saman yhdistäminen AM asteikko määrittäminen-esiintymän.

|**AM asteikko määrittäminen esiintymä**|**Port (portti)**|
|-----------------------|--------------------------|
|FrontEnd_0|3389|
|FrontEnd_1|3390|
|FrontEnd_2|3391|
|FrontEnd_3|3392|
|FrontEnd_4|3393|
|FrontEnd_5|3394|


### <a name="step-3-remote-connect-to-the-specific-vm-scale-set-instance"></a>Vaihe 3: Etäyhteyksien yhdistäminen tietyn AM asteikko määrittäminen esiintymä

Alla olevassa näyttökuvassa käytän muodostamaan yhteys FrontEnd_1 Etätyöpöytäyhteys:

![RDP][RDP]

## <a name="how-to-change-the-rdp-port-range-values"></a>Voit muuttaa RDP portin alueen arvot

### <a name="before-cluster-deployment"></a>Ennen käyttöönottoa klusterin

Kun olet määrittämässä klusterin Resurssienhallinta-mallin avulla voit määrittää alueen **inboundNatPools**.

Siirry **Microsoft.Network/loadBalancers**resurssin määritelmä. Valitse, löydät **inboundNatPools**kuvaus.  Korvaa *frontendPortRangeStart* ja *frontendPortRangeEnd* arvot.

![InboundNatPools][InboundNatPools]


### <a name="after-cluster-deployment"></a>Klusterikäyttöönoton jälkeen
Tämä on hieman enemmän aikaa ja voi aiheuttaa käytön kierrätetään VMs. Sinun on nyt uusi arvojen Azure PowerShellin avulla. Varmista, että tietokoneeseen on asennettu Azure PowerShell 1.0 tai uudempi. Jos et ole tehnyt tämän ennen, voin erittäin Ehdota noudattamalla artikkelin [asentaminen ja määrittäminen PowerShellin Azure.](../powershell-install-configure.md)

Kirjaudu Azure-tili. Jos jostain syystä epäonnistuu tämä PowerShell-komentoa, kannattaa tarkistaa, onko sinulla on asennettu oikein PowerShellin Azure.

```
Login-AzureRmAccount
```

Suorita seuraavat tiedot käyttämiseksi kuormituksen tasauspalvelun ja näet **inboundNatPools**kuvaus arvot:

```
Get-AzureRmResource -ResourceGroupName <RGname> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load balancer name>
```

Määritä haluamasi arvot nyt *frontendPortRangeEnd* ja *frontendPortRangeStart* .

```
$PropertiesObject = @{
    #Property = value;
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName <RG name> -ResourceType Microsoft.Network/loadBalancers -ResourceName <load Balancer name> -ApiVersion <use the API version that get returned> -Force
```


## <a name="next-steps"></a>Seuraavat vaiheet

- [Yleisiä tietoja "Käyttöön missä tahansa"-ominaisuutta ja Azure hallitun klustereiden vertailu](service-fabric-deploy-anywhere.md)
- [Klusterin suojaus](service-fabric-cluster-security.md)
- [Palvelun kangasta SDK ja käytön aloittaminen](service-fabric-get-started.md)


<!--Image references-->
[NodeTypes]: ./media/service-fabric-cluster-nodetypes/NodeTypes.png
[Resources]: ./media/service-fabric-cluster-nodetypes/Resources.png
[InboundNatPools]: ./media/service-fabric-cluster-nodetypes/InboundNatPools.png
[LBBlade]: ./media/service-fabric-cluster-nodetypes/LBBlade.png
[NATRules]: ./media/service-fabric-cluster-nodetypes/NATRules.png
[RDP]: ./media/service-fabric-cluster-nodetypes/RDP.png
