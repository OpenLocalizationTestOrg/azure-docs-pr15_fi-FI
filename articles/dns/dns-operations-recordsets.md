<properties
   pageTitle="DNS-tietueen joukot ja tietueiden hallinta Azure-portaalissa | Microsoft Azure"
   description="DNS-tietueen joukot ja Azure DNS-tietueiden hallinta kun toimialueen Azure DNS-isännöintipalvelu. Tietueen joukot ja tietueiden toimenpiteet kaikki PowerShell-komennoilla."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>DNS-tietueet ja tietueen joukkojen hallinta PowerShellin avulla



> [AZURE.SELECTOR]
- [Azure Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShellin](dns-operations-recordsets.md)



Tässä artikkelissa kerrotaan tietueen joukot ja tietueet DNS-vyöhyke hallinta Windows PowerShellin avulla.

On tärkeää ymmärtää DNS-tietueen joukot ja yksittäiset DNS-tietueet. Tietuejoukon on kokoelma tietueiden vyöhykkeessä, jotka on sama nimi ja tyyppi on sama. Lisätietoja on artikkelissa [Luo DNS-tietueen joukot ja tietueiden Azure-portaalissa](dns-getstarted-create-recordset-portal.md).

Tietueen joukot ja tietueiden hallintaan, sinun on Azure Resurssienhallinta PowerShellin cmdlet-komennot uusimman version. Katso lisätietoja, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md). Saat lisätietoja PowerShellin [Azure PowerShellin Azure resurssien hallinta](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Uuden tietuejoukon ja tietueen luominen

Luomisesta tietuejoukon PowerShellin avulla on artikkelissa [Luo DNS-tietueen joukot ja tietueiden PowerShell-toiminnolla](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Hae tietuejoukon

Voit noutaa aiemmin luodun tietuejoukon `Get-AzureRmDnsRecordSet`. Määritä tietueen suhteellinen nimen, tietuetyyppiä ja vyöhykkeen määrittäminen.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Kuten `New-AzureRmDnsRecordSet`, tietueen nimi on oltava suhteellinen nimi, mikä tarkoittaa, että se on poistettava alueen nimi.

Voit määrittää vyöhykkeen alueen nimi ja resurssiryhmän nimi tai käyttämällä vyöhykkeen objekti:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Palauttaa paikallisen objekti, joka esittää, joka on luotu Azure DNS-tietueen määrittäminen.

## <a name="list-record-sets"></a>Luettelon tietueen joukot

Voit myös käyttää`Get-AzureRmDnsRecordSet` tietueen tietojoukkojen luettelon, jos jätät *– nimi* ja/tai *– RecordType* parametrit.

### <a name="to-list-all-record-sets"></a>Luettelon kaikki tietueen joukot

Tässä esimerkissä palauttaa kaikki tietueen joukot, riippumatta siitä, nimi tai tietueen tyyppi:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Luettelon tietueen tiedostojoukot tietyn tietueen tyyppi

Tässä esimerkissä palauttaa tietueen joukot, joka vastaa tietueessa. Tässä tapauksessa tietuejoukon, joka on palautettu on "A" tietueet:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Vyöhykkeen voidaan määrittää käyttämällä joko *– ZoneName* ja *– ResourceGroupName* parametrien (kuten) tai määrittämällä zone objekti:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Tietueen lisääminen tietuejoukon

Tietueiden lisääminen tietueiden joukot avulla `Add-AzureRmDnsRecordConfig` cmdlet-komento. Tämä on offline-toiminto. Vain paikallinen objekti, joka esittää tietuejoukon muutetaan.

Tietueiden lisääminen tietuejoukon parametrit vaihtelee riippuen tietuejoukon. Esimerkiksi kun käyttämällä tietueen tyyppi "A", voit määrittää vain tietueiden parametria *-IPv4-osoite*.

Lisää tietueita voidaan lisätä kunkin tietueen, Lisää puhelut määrittämiä `Add-AzureRmDnsRecordConfig`. Voit lisätä minkä tahansa tietuejoukon enintään 20 tietuetta. Kuitenkin tyyppi "CNAME-tietueen määrä voi olla enintään yhden tietueen ja tietuejoukon ei voi olla kaksi päällekkäisiä tietueita. Tyhjä tietueen joukot (ja nolla-tietueet)) voidaan luoda, mutta ne eivät näy Azure DNS-nimipalvelimet.

Kun tietueen joukko sisältää haluamasi tietueet kokoelmaa, sinun on Vahvista avulla `Set-AzureRmDnsRecordSet` cmdlet-komento. Kun tietuejoukon on vahvistettu, se korvaa olemassa olevaan tietueeseen määrittäminen Azure DNS.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Yksi tietue määrityksessä A-tietueen luominen

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Luo tietue toimintojen järjestyksen voidaan myös *piped*, eli tietuejoukon objektin välitystapa pystyviivaa kulkeva parametrina sijaan. Esimerkki:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Lisää tietueen tyyppi-esimerkkejä

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Muokkaa olemassa olevan tietueen joukot

Aiemmin luodun tietuejoukon muokkaamisen tehdään samalla kun luot tietueita vaiheita. Toimintojen sarjan on seuraavanlainen:

1.  Noutaa järjestäminen olemassa olevaan tietueeseen `Get-AzureRmDnsRecordSet`.

2.  Muokkaa tietuejoukon joko lisäämällä, poistamalla tietueet, tietueen parametrien muuttaminen tai vaihtaminen tietueen asettaa elinaika (TTL). Tämä on offline-toiminto. Vain paikallinen objekti, joka esittää tietuejoukon muutetaan.

3.  Vahvista muutokset käyttämällä `Set-AzureRmDnsRecordSet` cmdlet-komento. Tämä korvaa olemassa olevaan tietueeseen määrittäminen Azure DNS.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Aiemmin luodun tietuejoukon tietueen päivitys

Tässä esimerkissä muutamme IP-osoitteen aiemmin luodun "A-tietue:

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

`Set-AzureRmDnsRecordSet` Cmdlet-komento käyttää etag tarkistukset varmistaa, että samanaikaiset muutokset ei korvata. Käytä *-Korvaa* lippu estää tarkistukset. Lisätietoja on artikkelissa [etags ja tunnisteet](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Jos haluat muokata SOA-tietue

Voi lisätä tai poistaa tietueita automaattisesti luotu-SOA tietueesta määrittää vyöhykkeen apex (nimen = "@"). Voit muokata kaikkia SOA (lukuun ottamatta "isäntä") tietueesta parametrit ja tietueen set TTL kuitenkin.

Seuraavassa esimerkissä esitetään SOA tietueen *Sähköposti* -ominaisuuden muuttaminen:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Voit muokata NS-tietueiden etsiminen vyöhykkeen apex

Ei voi lisätä, poistaa tai muokata tietueet automaattisesti luotu NS tietueen asetetaan vyöhykkeen apex (nimen = "@"). Vain muutoksen, joka on sallittu on muokattava tietuejoukon TTL.

Seuraavassa esimerkissä esitetään NS tietuejoukon TTL-ominaisuuden muuttaminen:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Tietueiden lisääminen aiemmin luodun tietuejoukon

Tässä esimerkissä on kaksi muita MX-tietuetta Lisää luodun tietuejoukon:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Aiemmin luodun tietuejoukon tietueen poistaminen

Tietueita voidaan poistaa järjestäminen tietueen `Remove-AzureRmDnsRecordConfig`. Tietue, joka on poistettu on oltava täsmälleen aiemmin luotuun tietueeseen kaikissa kaikki parametrit. Muutokset on vahvistettu käyttämällä `Set-AzureRmDnsRecordSet`.

Tietuejoukon viimeisen tietueen poistaminen ei poista tietuejoukon. Lisätietoja on alla [Poista tietue-joukon](#delete-a-record-set) .


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Tietueen poistaminen tietuejoukon toimintojen sarjan voi myös olla piped, eli tietuejoukon objektin välitystapa pystyviivaa kulkeva parametrina sijaan. Esimerkki:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>AAAA-tietueen poistaminen tietuejoukon

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>CNAME-tietueen poistaminen tietuejoukon

Koska CNAME-tietueen määrittäminen voi olla enintään yhden tietueen, tietueen poistaminen ei tyhjä tietuejoukon.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>MX-tietueen poistaminen tietuejoukon

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Poista tietuejoukon NS-tietue

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>SRV-tietueen poistaminen tietuejoukon

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT-tietueen poistaminen tietuejoukon

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Poista tietuejoukon

Käyttämällä voi poistaa tietueen joukot `Remove-AzureRmDnsRecordSet` cmdlet-komento. Et voi poistaa SOA ja NS-tietue määrittää vyöhykkeen apex osoitteessa (nimen = "@") , jotka on luotu automaattisesti vyöhykkeen luonnin yhteydessä. Ne poistetaan automaattisesti Jos vyöhykkeen poistetaan.

Poista tietuejoukon jollakin seuraavista tavoista:

### <a name="specify-all-the-parameters-by-name"></a>Määritä kaikki parametrit nimelläsi

Valinnainen *-voimassa* valitsimen avulla voidaan estää vahvistusviesti tulee näkyviin.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Määritä tietuejoukon nimi ja tyyppi, ja määrittää vyöhykkeen objekti

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Määritä tietuejoukon objekti

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Kun määrität tietuejoukon käyttämällä objektia, mahdollistaa etag tarkistaa, varmista, että samanaikaiset muutokset ei poisteta. Valinnainen *-Korvaa* merkinnän vaimentaa tarkistukset. Lisätietoja on kohdassa [Etags ja tunnisteet](dns-getstarted-create-dnszone.md#tagetag) .

Tietuejoukon objektin voi myös piped sijaan Parametrina välitetty:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure DNS on artikkelissa [Azure DNS yleiskatsaus](dns-overview.md). Saat lisätietoja automatisointi DNS- [luominen DNS-vyöhykkeet ja tietueen asettaa käyttämällä .NET SDK](dns-sdk.md).

Saat lisätietoja käänteinen DNS-tietueiden [hallinta PowerShellin avulla palvelujen käänteinen DNS-tietueet](dns-reverse-dns-record-operations-ps.md).
