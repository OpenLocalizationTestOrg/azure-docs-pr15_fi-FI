<properties
   pageTitle="Luo tietuejoukon ja tietueet DNS-vyöhyke PowerShellin avulla | Microsoft Azure"
   description="Miten Azure DNS-isännän tietueiden luomiseen. Tietue on määritetty asettaa ja tietueiden PowerShellin avulla"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Luo DNS-tietueen joukot ja tietueiden PowerShell-toiminnolla


> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-recordset-portal.md)
- [PowerShellin](dns-getstarted-create-recordset.md)
- [Azure CLI](dns-getstarted-create-recordset-cli.md)

Tässä artikkelissa käydään läpi tietueita ja tietueiden joukkojen luominen Windows PowerShellin avulla. Kun olet luonut DNS-vyöhyke, lisäät toimialueen DNS-tietueet. Tällöin sinun on DNS-tietueet ja tietueen joukot.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Varmista, että PowerShell uusin versio

Varmista, että olet asentanut uusimman version Azure Resurssienhallinta PowerShell cmdlet-komentoja. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.

## <a name="create-a-record-set-and-record"></a>Tietuejoukon ja tietueen luominen

Tässä osassa kerrotaan, miten voit luoda tietueen ja tietuejoukon.


### <a name="1-connect-to-your-subscription"></a>1. tilauksen yhdistäminen

Avaa PowerShell-konsolin ja muodostaa yhteyden tiliisi. Seuraavassa esimerkissä avulla voit tarkastella:

    Login-AzureRmAccount

Tarkista tilaukset-tilin.

    Get-AzureRmSubscription

Määritä tilaus, jota haluat käyttää.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Lisätietoja PowerShellin artikkelissa [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Luo tietuejoukon

Voit luoda tietueen joukkoja käyttämällä `New-AzureRmDnsRecordSet` cmdlet-komento. Luodessasi tietuejoukon, sinun täytyy määrittää tietueen nimi, vyöhykkeen, time to live (TTL) ja tietuetyypin määrittäminen.

Määrittää vyöhykkeen yläreunassa tietueen luomiseen (Tässä tapauksessa "contoso.com"), käytä tietueen nimi "@", lainausmerkit mukaan lukien. Tämä on Yleiset DNS-konferenssi.

Seuraavassa esimerkissä luodaan määrittää DNS-vyöhyke "contoso.com" suhteellinen nimellä "www-tietueen. Tietuejoukon täydellinen nimi on "www.contoso.com". Tietuetyyppi on "A", ja TTL on 60 sekuntia. Kun olet suorittanut tämän vaiheen, sinun on tyhjä "www-tietueen määrittäminen, joka on määritetty muuttujan *$rs*.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Jos tietue määrittäminen jo olemassa

Jos on jo määritetty tietue-komento epäonnistuu, paitsi *-Korvaa* valitsimen. *-Korvaa* vaihtoehto käynnistää vahvistusviesti, jonka avulla voidaan estää *-voimassa* vaihtaminen.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


Tässä esimerkissä määrittää vyöhykkeen käyttämällä alueen nimi ja resurssiryhmän nimi. Vaihtoehtoisesti voit määrittää vyöhykkeen lisääminen objektiin palauttamat `Get-AzureRmDnsZone` tai `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Palauttaa paikallisen objekti, joka esittää, joka on luotu Azure DNS-tietueen määrittäminen.

### <a name="3-add-a-record"></a>3. tietueen lisääminen

Haluat käyttää juuri luomasi "www" tietuejoukon tietueiden lisääminen. Voit lisätä IPv4 *A* tietueet "www-tietueeseen Määritä käyttämällä seuraavassa esimerkissä. Tässä esimerkissä on riippuvainen muuttujan *$rs* , voit määrittää edellisessä vaiheessa.

Tietueiden lisääminen tietueen Määritä käyttämällä `Add-AzureRmDnsRecordConfig` on offline-toiminto. Vain paikallisen muuttujan *$rs* päivitetään.


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. Vahvista muutokset

Vahvista muutokset tietuejoukon. Käytä `Set-AzureRmDnsRecordSet` voit ladata muutokset tietuejoukon Azure DNS.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. noutaa tietuejoukon

Voit hakea käyttämällä Azure DNS-tietuejoukon `Get-AzureRmDnsRecordSet` seuraavan esimerkin mukaisesti.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Voit käyttää myös nslookup-työkalun tai muita DNS-työkaluja uuden tietuejoukon kysely.

Ei ole vielä delegoida Azure DNS-nimipalvelimet toimialue, jos haluat määrittää erikseen nimi, palvelimen ja vyöhykkeen osoite.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Yksi tietue kunkin tietueen joukon luominen


Seuraavissa esimerkeissä, miten voit luoda tietueen kunkin tietuetyypin. Tietueen kukin ehtojoukko sisältää yhden tietueen.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Seuraavat vaiheet

[Voit hallita DNS-vyöhykkeet PowerShellin avulla](dns-operations-dnszones.md)

[DNS-tietueet ja tietueen joukkojen hallinta PowerShellin avulla](dns-operations-recordsets.md)

[Azure toimintoja, joiden .NET SDK automatisointi](dns-sdk.md)
