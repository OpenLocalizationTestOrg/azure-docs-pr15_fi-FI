### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Yksi tietue määrityksessä AAAA-tietueen luominen

    $rs = New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-cname-record-set-with-a-single-record"></a>Luo CNAME-tietue, joka määrittää yksittäisen tietueen kanssa

    $rs = New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-mx-record-set-with-a-single-record"></a>Yksi tietue määrityksessä MX-tietueen luominen

Tässä esimerkissä Käytämme tietuejoukon nimi "@" luominen vyöhykkeen apex MX-tietue (Tässä tapauksessa "contoso.com"). Tämä on yleisiä MX-tietueita.

    $rs = New-AzureRmDnsRecordSet -Name "@" -RecordType MX -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-ns-record-set-with-a-single-record"></a>Luo yhden tietueen määrityksessä NS-tietue

    $rs = New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Luo määrityksessä vastaavan tietueen PTR-tietue
Tässä tapauksessa "Omat-arpa-zone.com' edustaa ARPA vyöhykkeen edustava IP-alueen.  Määrittää vyöhykkeen PTR kunkin tietueen vastaa IP-alueen sisällä IP-osoite.  

    $rs = New-AzureRmDnsRecordSet -Name "10" -RecordType PTR -Ttl 3600 -ZoneName my-arpa-zone.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ptrdname "myservice.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-an-srv-record-set-with-a-single-record"></a>Luo yhden tietueen määrityksessä SRV-tietue

Jos olet luomassa SRV-tietue vyöhykkeen ylimmällä, riittää, että määrittää *_service* ja *_protocol* tietueen nimi. Ei tarvita, jos haluat sisällyttää "@" -tietueen nimi.

    $rs = New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="create-a-txt-record-set-with-a-single-record"></a>Määrittää yksittäisen tietueeseen TXT-tietueen luominen

    $rs = New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs
