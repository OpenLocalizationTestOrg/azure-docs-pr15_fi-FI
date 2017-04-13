<properties
   pageTitle="Mutiple VIPs pilvipalveluun varten"
   description="Yleisiä tietoja multiVIP ja voit määrittää useita VIPs pilvipalveluun"
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

# <a name="configure-multiple-vips-for-a-cloud-service"></a>Useita VIPs cloud-palvelun määrittäminen

Voit käyttää Azure pilvipalveluihin julkisen Internetin Azure myöntämä IP-osoitteen avulla. Tätä yleisen IP-osoitetta käytetään nimitystä VIP (virtual IP) jälkeen se on linkitetty Azure kuormituksen ja cloud-palvelun esiintymät ei virtuaalikoneen (AM). Voit käyttää mitä tahansa pilvipalveluun AM esiintymä yksittäisen VIP avulla.

On kuitenkin tilanteissa, joissa voi on useampi kuin yksi VIP tekstiksi osoittamalla saman pilvipalvelussa. Esimerkiksi pilvipalvelussa sijaitsevaan voivat ylläpitää useita sivustoja, jotka edellyttävät SSL-yhteyden avulla oletusportti 443, kun kukin nykyisessä eri asiakkaan tai vuokraaja. Tässä skenaariossa tarvitset eri julkisen aukeaman IP-osoite jokaiselle sivustolle. Seuraavassa kuvassa on kuvattu tyypillinen usean vuokraajan sivuston isännöidä samaa julkisen porttia useita SSL-varmenteita tarvitaan.

![Usean VIP SSL-skenaario](./media/load-balancer-multivip/Figure1.png)

Yllä olevassa esimerkissä kaikki VIPs käyttää samaa julkisen porttia (443) ja liikenne uudelleenohjataan yhteen tai Lisää kuormituksen saapuva VMs, valitse Yksityinen portin isännöinnin kaikkien sivustojen pilvipalvelussa sisäinen IP-osoite.

>[AZURE.NOTE] Toinen tilanne voi käyttää useita VIPs isännöi useita SQL AlwaysOn käytettävyys ryhmän kuuntelijoita näennäiskoneiden samoja.

VIPs ovat dynaamisia oletusarvoisesti, mikä tarkoittaa, että todellinen pilvipalveluun, määritetty IP-osoite voivat muuttua ajan kuluessa. Jos haluat estää, asetuksen, voit varata VIP palvelun. Saat lisätietoja varattu VIPs on [Varattu julkiseen IP](../virtual-network/virtual-networks-reserved-public-ip.md).

>[AZURE.NOTE] Katso [IP-osoite hinnat](https://azure.microsoft.com/pricing/details/ip-addresses/) lisätietoja hinnoittelu VIPs ja varattu IP-osoitteet.

Voit tarkistaa cloud-palveluissa käytetyt VIPs PowerShellin avulla sekä Lisää ja poista VIPs, Liitä VIP päätepisteen ja määrittää kuormituksen tietyn VIP.

## <a name="limitations"></a>Rajoitukset

Tällä hetkellä usean VIP-toiminto on rajoitettu seuraavissa tilanteissa:

- **Vain IaaS**. Voit ottaa usean VIP vain pilvipalveluihin, jotka sisältävät VMs varten. Et voi käyttää usean VIP PaaS tilanteissa roolin esiintymät.
- **Vain PowerShell**. Voit hallita usean VIP vain PowerShell-toiminnolla.

Näiden rajoitusten ovat väliaikaisia ja voi muuttaa milloin tahansa. Varmista, että voit palata tähän määritykseen tämän sivun tulevien muutosten tarkistaminen.


## <a name="how-to-add-a-vip-to-a-cloud-service"></a>Voit lisätä VIP pilvipalveluun

Voit lisätä VIP palvelun, suorita PowerShell-komentoa:

    Add-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

Tämä komento näkyy tulos, joka on samankaltainen kuin seuraavassa esimerkissä:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Add-AzureVirtualIP   4bd7b638-d2e7-216f-ba38-5221233d70ce Succeeded

## <a name="how-to-remove-a-vip-from-a-cloud-service"></a>VIP poistamisesta pilvipalveluun

Jos haluat poistaa palvelun yllä olevassa esimerkissä lisätään VIP, suorita PowerShell-komentoa:

    Remove-AzureVirtualIP -VirtualIPName Vip3 -ServiceName myService

>[AZURE.IMPORTANT] Voit poistaa VIP vain, jos se on siihen liittyvä päätepisteitä.

## <a name="how-to-retrieve-vip-information-from-a-cloud-service"></a>Voit hakea tietoja VIP pilvipalveluun

Cloud-palveluun liitetyn VIPs hakemiseen suorita seuraavaa PowerShell-komentosarjaa:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Komentosarjan näkyy tulos, joka on samankaltainen kuin seuraavassa esimerkissä:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

Tässä esimerkissä käytössäsi on 3 VIPs:

- **Vip1** on oletusarvoinen VIP, tiedät, koska IsDnsProgrammedName on arvo TOSI.
- **Vip2** ja **Vip3** ei käytetä, sillä niitä ei ole IP-osoitteita. Ne käytetään vain, jos liität VIP päätepistettä.

>[AZURE.NOTE] Ylimääräisen VIPs vain Peri tilauksen, kun ne liittyvät päätepisteen. Saat lisätietoja hinnat [hinnat IP-osoite](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="how-to-associate-a-vip-to-an-endpoint"></a>Voit liittää VIP päätepisteen

Liitä VIP pilvipalvelussa päätepisteen, valitse Suorita PowerShell-komentoa:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -Protocol tcp -LocalPort 8080 -PublicPort 80 -VirtualIPName Vip2 `
  	| Update-AzureVM

Komento luo linkitetty kutsua *Vip2* porttiin *80*ja linkkejä, se AM nimeltä *myVM1* nimeltä *myService* *TCP* käyttäminen portin *8080*pilvipalvelussa VIP päätepisteen.

Voit tarkistaa määritykset, suorita PowerShell-komentoa:

    $deployment = Get-AzureDeployment -ServiceName myService
    $deployment.VirtualIPs

Tulos näyttää seuraavankaltaiselta seuraavassa esimerkissä:

    Address         : 191.238.74.148
    IsDnsProgrammed : True
    Name            : Vip1
    ReservedIPName  :
    ExtensionData   :

    Address         : 191.238.74.13
    IsDnsProgrammed :
    Name            : Vip2
    ReservedIPName  :
    ExtensionData   :

    Address         :
    IsDnsProgrammed :
    Name            : Vip3
    ReservedIPName  :
    ExtensionData   :

## <a name="how-to-enable-load-balancing-on-a-specific-vip"></a>Valitse tietyn VIP kuormituksen ottaminen käyttöön

Voit yhdistää yhden VIP kuormituksen tarkoituksiin useita näennäiskoneiden. Sinulla on esimerkiksi nimeltä *myService*pilvipalveluun ja kaksi näennäiskoneiden *myVM1* ja *myVM2*. Ja pilvipalvelussa sijaitsevaan on useita VIPs jonkin niistä nimeltä *Vip2*. Jos haluat varmistaa, että kaikki liikenne paikalliseen käyttöön *Vip2* portin *81* täsmätään *myVM1* ja *myVM2* -portin *8181*, suorita seuraavaa PowerShell-komentosarjaa:

    Get-AzureVM -ServiceName myService -Name myVM1 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

    Get-AzureVM -ServiceName myService -Name myVM2 `
  	| Add-AzureEndpoint -Name myEndpoint -LoadBalancedEndpointSetName myLBSet `
        -Protocol tcp -LocalPort 8181 -PublicPort 81 -VirtualIPName Vip2  -DefaultProbe `
  	| Update-AzureVM

Voit myös päivittää oman kuormituksen, jos haluat käyttää eri VIP. Jos suoritat PowerShell-komentoa, voit esimerkiksi muuttaa määrittäminen käyttämään nimeltä Vip1 VIP kuormituksen:

    Set-AzureLoadBalancedEndpoint -ServiceName myService -LBSetName myLBSet -VirtualIPName Vip1

## <a name="next-steps"></a>Seuraavat vaiheet

[Lokitiedoston analysointitietoja Azure kuormituksen tasaus](load-balancer-monitor-log.md)

[Internet aukeaman kuormituksen tasauspalvelun yleiskatsaus](load-balancer-internet-overview.md)

[Aloita vastakkaisten kuormituksen Internetissä](load-balancer-get-started-internet-arm-ps.md)

[VPN-yleiskatsaus](../virtual-network/virtual-networks-overview.md)

[Varattu IP REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx)