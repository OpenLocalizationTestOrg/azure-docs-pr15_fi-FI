<properties
   pageTitle="Aloita Azure DNS | Microsoft Azure"
   description="Opettele luomaan DNS-vyöhykkeet Azure DNS. Tämä on ensimmäinen DNS-vyöhyke luotu Aloita isännöinnin toimialueen DNS-PowerShellin avulla saat vaiheen vaihe."
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

# <a name="create-a-dns-zone-using-powershell"></a>Luo DNS-vyöhyke PowerShellin avulla

> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShellin](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)

Tässä artikkelissa opastaa luomisessa DNS-vyöhyke PowerShell-toiminnolla. Voit myös luoda CLI tai Azure portaalin DNS-vyöhyke.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>Tietoja Etags ja tunnisteet

### <a name="etags"></a>Etags

Oletetaan, että kaksi henkilöä tai kaksi yrittää DNS-tietueen muokkaaminen yhtä aikaa. Kumpi voittaa? Ja eniten tietää, että ne on korvattu vain jonkun muun luomaa muutokset?

Azure DNS käyttää Etags käsittelemään samanaikaiset muutokset samalle resurssille turvallisesti. Kukin DNS resurssi (vyöhyke tai tietuejoukon) on Etag liittyy. Aina, kun resurssin haetaan, sen Etag haetaan. Kun päivität resurssin, voit halutessasi välittämiseen takaisin Etag niin Azure DNS voit varmistaa, Etag palvelimen vastineita. Koska jokaisen päivityksen resurssille johtaa uudelleen Etag, Etag ristiriita osoittaa samanaikainen muutos on tapahtunut. Etags käytetään myös luotaessa uusi resurssi varmistaa, että resurssi ei ole jo olemassa.

DNS-PowerShellin Azure käyttää oletusarvon mukaan Etags estäminen vyöhykkeisiin samanaikaiset muutokset ja Tallenna joukot. Valinnainen *-Korvaa* valitsimen avulla voidaan estää Etag tarkistukset, jolloin asennuksen aikana tapahtuneista virheistä samanaikaiset muutokset korvataan.

Azure DNS REST-Ohjelmointirajapinnalla tasolla Etags määritetään käyttämällä HTTP-otsikot.  Seuraavassa taulukossa on esitetty toimintaa:

|Ylätunniste|Toiminta|
|------|--------|
|Ei mitään|Voit tehdä aina onnistuu (ei Etag tarkistukset)|
|Jos vastine<etag>|Voit tehdä onnistuu vain, jos resurssi on olemassa ja Etag vastaa|
|Jos vastine *     | Voit tehdä onnistuu vain, jos resurssi on olemassa|
|IF-ei mitään-vastine * |  Voit tehdä onnistuu vain, jos resurssia ei ole|

### <a name="tags"></a>Tunnisteet

Tunnisteiden poikkeavat Etags. Tunnisteet ovat luettelon nimen ja arvon tietoparin, ja niitä käytetään Azure resurssien hallinnan avulla otsikko resurssien laskutuksen tai ryhmittely. Lisätietoja tunnisteet nähdä [käyttäminen tunnisteet järjestämiseen Azure resurssit](../resource-group-using-tags.md).

Azure DNS-PowerShell tukee tunnisteet vyöhykkeet ja määritetty asetuksilla tietueen joukot `-Tag` parametria.


## <a name="before-you-begin"></a>Ennen aloittamista

Tarkista, pidä seuraavat asiat ennen aloittamista kokoonpanosi.

- Azure tilaus. Jos sinulla ei vielä ole Azure tilaus, voit ottaa [MSDN tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) tai sisäänkirjautumisen määrittäminen [ilmainen tili](https://azure.microsoft.com/pricing/free-trial/).

- Tarvitset asenna uusin versio Azure Resurssienhallinta PowerShellin cmdlet-komennot (1.0 tai uudempi). Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) lisätietoja asentaminen PowerShellin cmdlet-komennot.

## <a name="step-1---sign-in"></a>Vaihe 1 - kirjautuminen

Avaa PowerShell-konsolin ja muodostaa yhteyden tiliisi. Lisätietoja on artikkelissa [Windows PowerShellin resurssien hallinta](../powershell-azure-resource-manager.md).

Seuraavassa esimerkissä avulla voit tarkastella:

    Login-AzureRmAccount

Tarkista tilaukset-tilin.

    Get-AzureRmSubscription

Määritä tilaus, jota haluat käyttää.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Vaihe 2: Luo resurssiryhmä

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tämä käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Koska kaikki DNS-resurssit ovat yleistä, ei alueellisen, resurssin ryhmän sijainti valinta ei ole vaikutusta Azure DNS.

Voit ohittaa tämän vaiheen, jos käytössäsi on aiemmin resurssiryhmä.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Vaihe 3: Rekisteröi

Azure DNS-palvelun hallitsee Microsoft.Network resurssin toimittaja. Azure-tilauksesi on oltava rekisteröity käyttämään tämän resurssin tarjoajaan, ennen kuin voit käyttää Azure DNS. Tämä on jokaisen tilauksen erikseen toiminto.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Vaihe 4 – Luo DNS-vyöhyke

DNS-vyöhyke luodaan käyttäen `New-AzureRmDnsZone` cmdlet-komento. On esimerkkejä alla luominen DNS-vyöhyke kanssa tai ilman tunnisteet. Lisätietoja tunnisteet-kohdassa tämän artikkelin [tunnisteet](#tags) .

>[AZURE.NOTE] Azure DNS-vyöhyke nimet on määritettävä ilman lopetetaan **".'**. Esimerkiksi "**contoso.com**-muodossa sijaan '**contoso.com.**".

### <a name="to-create-a-dns-zone"></a>Luo DNS-vyöhyke

Alla olevassa esimerkissä Luo DNS-vyöhyke, eli *contoso.com* kutsutaan *MyResourceGroup*resurssiryhmä. Luo DNS-vyöhyke, korvaaminen oman arvot esimerkin avulla.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Luo DNS-vyöhyke tunnisteet

Seuraavassa esimerkissä esitetään, kuinka luomaan DNS-vyöhyke sisältää kaksi tunnisteita, *projektin = esittely* ja *kirjekuori = testin*. Luo DNS-vyöhyke, korvaaminen oman arvot esimerkin avulla.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Näytä tietueet

DNS-vyöhyke luominen luo seuraavat DNS-tietueet:

- *Myöntäjä Käynnistä* (SOA)-tietue. Tämä ei sisällä tietoja jokaisen DNS-vyöhyke ylimmällä tasolla.

- Tärkeimpien (NS) nimipalvelintietueita. Nämä Näytä mitä nimipalvelimet isännöit vyöhykkeen. Azure DNS käyttää nimipalvelimet resurssivarantoon ja hieman eri nimipalvelimet voidaan määrittää vyöhykkeitä Azure DNS-palvelimesta. Katso lisätietoja [delegoida Azure DNS toimialue](dns-domain-delegation.md) .

Jos haluat tarkastella tietueista, käytä `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Tietueen asettaa pääkansio (tai *apex*) DNS-vyöhyke käytön **@** tietueen määrittää nimi.


## <a name="test"></a>Testi

Voit testata DNS-vyöhyke DNS-työkaluja, kuten nslookup, tarkastella ja [Ratkaise DnsName PowerShell cmdlet-komennon](https://technet.microsoft.com/library/jj590781.aspx)avulla.

Jos et ole vielä valtuutetun toimialueen käyttämään uutta aluetta Azure DNS-, sinun on suora suoraan hiirellä yhtä suorakulmion nimipalvelimet vyöhykkeen DNS-kysely. Vyöhykkeen nimipalvelimet on esitetty NS-tietueet mukaan lueteltuna `Get-AzureRmDnsRecordSet` yläpuolella. Voidaan varmistaa, että korvaava oikeat arvot yhdeksi alla oleva komento vyöhykkeen.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut DNS-vyöhyke, luo [tietue joukot ja tietueiden](dns-getstarted-create-recordset.md) voit käynnistää Internet-toimialueen nimet selvitetään.

