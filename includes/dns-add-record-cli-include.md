#### <a name="create-an-a-record-set-with-single-record"></a>Luo yhden tietueen määrityksessä A-tietue

Voit luoda tietuejoukon käyttämällä `azure network dns record-set create`. Määritä resurssiryhmä alueen nimi suhteellinen nimi, tietuetyypin ja ajan Live (TTL) määrittäminen tietueen.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Luotuasi A tietueeseen määrittäminen, IPv4-osoitteen lisääminen kanssa tietuejoukon `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Yksi tietue määrityksessä AAAA-tietueen luominen

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Luo CNAME-tietue, joka määrittää yksittäisen tietueen kanssa

CNAME-tietueet Salli vain yhden yksittäisen merkkijonoarvon.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Yksi tietue määrityksessä MX-tietueen luominen

Tässä esimerkissä Käytämme tietuejoukon nimi "@" MX-tietueen luominen vyöhykkeen apex (Tässä tapauksessa "contoso.com"). Tämä on yleisiä MX-tietueita.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Luo yhden tietueen määrityksessä NS-tietue

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Luo määrityksessä vastaavan tietueen PTR-tietue  
Tässä tapauksessa "Omat-arpa-zone.com' edustaa ARPA vyöhykkeen edustava IP-alueen.  Määrittää vyöhykkeen PTR kunkin tietueen vastaa IP-alueen sisällä IP-osoite.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Luo yhden tietueen määrityksessä SRV-tietue

Jos olet luomassa SRV-tietue vyöhykkeen ylimmällä, voit määrittää "_service" ja "_protocol" tietueen nimi. Ei tarvita, jos haluat sisällyttää "@" -tietueen nimi.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Määrittää yhden tietueen TXT-tietueen luominen

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
