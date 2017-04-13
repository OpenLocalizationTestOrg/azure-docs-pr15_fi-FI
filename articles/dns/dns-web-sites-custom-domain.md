<properties
   pageTitle="Luo mukautettu DNS-tietueet verkkosovellukseen | Microsoft Azure  "
   description="Voit luoda mukautetun toimialueen DNS-tietueet web Appin Azure DNS: n avulla."
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Mukautetun toimialueen DNS-tietueiden web-sovelluksen luominen

Voit käyttää Azure DNS isännöimiseen mukautetun toimialueen web Apps-sovellukset. Esimerkiksi luot Azure web app-sovelluksessa ja haluat käyttää joko käyttäjien käyttäen contoso.com tai www.contoso.com täydellinen toimialuenimi.

Tällöin sinun on luotava kaksi tietuetta:

- Pääkansio "A" tietueen osoittamalla contoso.com
- Www-nimi, joka osoittaa tietueessa "CNAME-tietue

Ota huomioon, että jos A-tietue web-sovelluksen luominen Azure-tietueessa on oltava manuaalisesti päivittää Jos pohjana IP-osoite web app-muutokset.

## <a name="before-you-begin"></a>Ennen aloittamista

Ennen kuin aloitat, sinun on ensin Luo DNS-vyöhyke Azure DNS- ja delegoida rekisteröintipalvelustasi Azure DNS-vyöhyke.

1. Luo DNS-vyöhyke-ohjeiden [Luo DNS-vyöhyke](dns-getstarted-create-dnszone.md).
2. Siirtää DNS-Azure DNS noudattamalla [toimialueen DNS-delegointi](dns-domain-delegation.md).

Kun olet luonut vyöhykkeen ja delegoiminen Azure DNS-voit luoda tietueita sitten mukautetun toimialueen.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Luo mukautetun toimialueen A-tietue

A-tietue käytetään nimi yhdistäminen IP-osoitetta. Seuraavassa esimerkissä on tarkoitus määrittää @ kuin IPv4-osoitteeseen A-tietue:

### <a name="step-1"></a>Vaihe 1

Luo A-tietue ja muuttujan $rs lisääminen

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Vaihe 2

IPv4-arvon lisääminen aiemmin luodun tietuejoukon "@" määritetty $rs muuttujan avulla. Määritetty IPv4-arvo on koodiin IP-osoite.

Voit etsiä verkkosovellukseen IP-osoite-ohjeiden [määrittäminen käyttämään mukautettua toimialuenimeä Azure sovelluksen-palvelussa](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Vaihe 3

Vahvista muutokset tietuejoukon. Käytä `Set-AzureRMDnsRecordSet` voit ladata muutokset tietuejoukon Azure DNS:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Luo mukautetun toimialueen CNAME-tietue

Jos toimialueen hallitsee jo Azure DNS ( [DNS-toimialueen delegointi](dns-domain-delegation.md)-kohdassa voit käyttää seuraavaa esimerkin luominen contoso.azurewebsites.net CNAME-tietue.

### <a name="step-1"></a>Vaihe 1

Avaa PowerShell- ja luo uusi CNAME-tietuejoukon ja Määritä muuttujan $rs. Tässä esimerkissä luo tietuejoukon-tyypin CNAME "Live aika" 600 sekuntia nimeltä "contoso.com" DNS-vyöhyke.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Vaihe 2

Kun tietuejoukon CNAME-TIETUE on luotu, sinun täytyy luoda alias-arvo, joka osoittaa web App-sovellukseen.

Käyttämällä aiemmin määritetyt muuttujan "$rs" Voit PowerShell-komennolla voit luoda web app-contoso.azurewebsites.net alias.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Vaihe 3

Vahvista muutokset käyttämällä `Set-AzureRMDnsRecordSet` cmdlet-komennon:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Voit tarkistaa tietue on luotu oikein "www.contoso.com" käyttämällä nslookup-kyselyt alla kuvatulla tavalla:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Web-sovellusten "awverify"-tietueen luominen


Jos päätät käyttää A-tietue web App-on kerrotaan vahvistusprosessissa, jotta mukautettu toimialue kuuluu sinulle. Vahvistus tämä vaihe on valmis luomalla erityisen nimeltä "awverify" CNAME-tietue. Tässä osassa koskee vain tietueet.


### <a name="step-1"></a>Vaihe 1

"Awverify"-tietueen luominen Seuraavassa esimerkissä on luodaan contoso.com mukautetun toimialueen omistajuuden vahvistamiseksi "aweverify"-tietue.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Vaihe 2

Kun tietuejoukon "awverify" on luotu, määrittää CNAME-tietueen alias määrittäminen. Seuraavassa esimerkissä on määrittää CNAMe-tietueen alias asettaminen awverify.contoso.azurewebsites.net.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Vaihe 3

Vahvista muutokset käyttämällä `Set-AzureRMDnsRecordSet cmdlet`-komennon alla esitetyllä tavalla.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Seuraavat vaiheet

Noudattamalla voit määrittää mukautetun toimialueen käyttäminen koodiin [määrittäminen sovelluksen palvelun mukautettua toimialuenimeä](../app-service-web/web-sites-custom-domain-name.md) .








