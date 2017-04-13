<properties
   pageTitle="DNS-tietueen joukot ja käyttämällä Azure CLI Azure DNS-tietueiden hallinta | Microsoft Azure"
   description="DNS-tietueen joukot ja Azure DNS-tietueiden hallinta kun toimialueen Azure DNS-isännöintipalvelu. Tietueen joukot ja tietueiden toimenpiteet kaikki CLI komennot."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>DNS-tietueet ja tietueen joukkojen hallinta CLI avulla


> [AZURE.SELECTOR]
- [Azure Portal](dns-operations-recordsets-portal.md)
- [Azure CLI](dns-operations-recordsets-cli.md)
- [PowerShellin](dns-operations-recordsets.md)


Tämän artikkelin avulla voit hallita tietueen joukot ja tietueet DNS-vyöhyke Office kaikissa ympäristöissä Azure komentorivivalitsimet-käyttöliittymän (CLI) avulla.

On tärkeää ymmärtää DNS-tietueen joukot ja yksittäiset DNS-tietueet. Tietuejoukon on kokoelma vyöhykkeen tietueet, jotka on sama nimi ja tyyppi on sama. Lisätietoja on artikkelissa [tietoja tietueen joukot ja tietueet](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Office kaikissa ympäristöissä Azure CLI määrittäminen

Azure DNS on Azure vain Resurssienhallinta-palvelu. Se ei ole Azure Service Management-Ohjelmointirajapinnan. Varmista, että Azure-CLI on määritetty käyttämään Resurssienhallinta-tilaa käyttämällä `azure config mode arm` komento.

Jos näet **Virhe: "dns" ei ole azure komento**, se on todennäköisesti, koska käytössäsi on Azure CLI Azure hallinta-tilassa, ei Resurssienhallinta-tilassa.

## <a name="create-a-new-record-set-and-record"></a>Uuden tietuejoukon ja tietueen luominen

Määritä Azure-portaalissa uuden tietueen luomisesta on artikkelissa [Voit luoda tietueen määrittäminen ja tietueita](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Noutaa tietuejoukon

Voit noutaa aiemmin luodun tietuejoukon `azure network dns record-set show`. Määritä resurssiryhmä alueen nimi suhteellinen nimen ja tietuetyypin määrittäminen tietueen. Käytä alla olevassa esimerkissä-arvojen korvaaminen omalla.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Luettelon tietueen joukot

Voit näyttää kaikki tietueet DNS-vyöhyke avulla `azure network dns record-set list` komento. Sinun täytyy määrittää resurssin ryhmänimi ja alueen nimi.

### <a name="to-list-all-record-sets"></a>Luettelon kaikki tietueen joukot

Tässä esimerkissä palauttaa kaikki tietueen joukot, riippumatta siitä, nimi tai tietueen tyyppi:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Luettelon tietueen tietojoukkojen tietyntyyppinen

Tässä esimerkissä palauttaa tietueen joukot, jotka vastaavat annetun tietuetyyppiä (Tässä tapauksessa "A" tietueiden):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Tietueen lisääminen tietuejoukon

Tietueiden lisääminen tietueiden joukot avulla `azure network dns record-set add-record`komento. Tietueiden lisääminen tietuejoukon parametrit vaihtelevat tietue, joka on määritetty tyypin mukaan. Esimerkiksi kun tyyppi "A" tietuejoukko, voit määrittää vain tietueiden parametria `-a <IPv4 address>`.

Voit luoda tietuejoukon käyttämällä `azure network dns record-set create`komento. Määritä resurssiryhmä alueen nimi suhteellinen nimen, tietuetyypin ja aikaa live (TTL) määrittäminen tietueen. Jos `--ttl` parametria ei ole määritetty, arvo (sekunteina) neljä oletusarvo.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Kun olet luonut "A" tietuejoukon, Lisää IPv4-osoitteen avulla `azure network dns record-set add-record`komento.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


Seuraavissa esimerkeissä, miten voit luoda tietueen kunkin tietuetyypin. Tietueen kukin ehtojoukko sisältää yhden tietueen.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Päivitä tietue tietuejoukon

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Jos haluat lisätä toisen IP-osoite (1.2.3.4) aiemmin luotuun "A" tietueen Aseta ("www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Jos haluat poistaa olemassa olevaan arvoon tietuejoukon
Käytä `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Tietueen poistaminen tietuejoukon

Tietueita voidaan poistaa järjestäminen tietueen `azure network dns record-set delete-record`. Tietue, joka on poistettu on oltava täsmälleen aiemmin luotuun tietueeseen kaikissa kaikki parametrit.

Tietuejoukon viimeisen tietueen poistaminen ei poista tietuejoukon. Lisätietoja on kohdassa on tämän artikkelin [Poista tietue-joukon](#delete)nimi.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>AAAA-tietueen poistaminen tietuejoukon

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>CNAME-tietueen poistaminen tietuejoukon

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>MX-tietueen poistaminen tietuejoukon

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Poista tietuejoukon NS-tietue

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Poista tietuejoukon PTR-tietue
Tässä tapauksessa "Omat-arpa-zone.com' edustaa ARPA vyöhykkeen edustava IP-alueen.  Määrittää vyöhykkeen PTR kunkin tietueen vastaa IP-alueen sisällä IP-osoite.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>SRV-tietueen poistaminen tietuejoukon

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>TXT-tietueen poistaminen tietuejoukon

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Poista tietuejoukon

Käyttämällä voi poistaa tietueen joukot `Remove-AzureRmDnsRecordSet` cmdlet-komento. Et voi poistaa SOA ja NS-tietue määrittää vyöhykkeen apex osoitteessa (nimen = "@") , jotka on luotu automaattisesti vyöhykkeen luonnin yhteydessä. Ne poistetaan automaattisesti Jos vyöhykkeen poistetaan.

Seuraavassa esimerkissä "," "testi-a" tietuejoukon poistetaan "contoso.com" DNS-vyöhyke:

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Valinnainen *-q* -valitsinta voidaan estää vahvistusviesti tulee näkyviin.


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure DNS on artikkelissa [Azure DNS yleiskatsaus](dns-overview.md). Saat lisätietoja automatisointi DNS- [luominen DNS-vyöhykkeet ja tietueen asettaa käyttämällä .NET SDK](dns-sdk.md).

Jos haluat käyttää käänteinen DNS-tietueet, katso, [miten voit hallita käänteinen DNS-tietueet käyttämällä Azure-CLI palvelujen](dns-reverse-dns-record-operations-cli.md).
