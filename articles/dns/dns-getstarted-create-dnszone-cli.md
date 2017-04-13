<properties
   pageTitle="Luo DNS-vyöhyke käyttämällä CLI | Microsoft Azure"
   description="DNS-vyöhykkeet luominen Azure DNS-vaiheittaiset Aloita toimialueen DNS-CLI käytön isännöintipalvelu"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Luo käyttämällä CLI Azure DNS-vyöhyke


> [AZURE.SELECTOR]
- [Azure Portal](dns-getstarted-create-dnszone-portal.md)
- [PowerShellin](dns-getstarted-create-dnszone.md)
- [Azure CLI](dns-getstarted-create-dnszone-cli.md)


Tässä artikkelissa opastaa vaihe vaiheelta, miten DNS-vyöhyke luominen käyttämällä CLI. Voit myös luoda PowerShell tai Azure portaalin DNS-vyöhyke.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Ennen aloittamista

Näiden ohjeiden avulla Microsoft Azure CLI. Muista päivittää uusimmat Azure CLI (0.9.8 tai uudempi) voit käyttää Azure DNS-komentoja. Kirjoita `azure -v` tarkistamaan Azure CLI version on asennettu tietokoneeseen.

## <a name="step-1---set-up-azure-cli"></a>Vaihe 1 – Azure CLI määrittäminen

### <a name="1-install-azure-cli"></a>1. Asenna Azure CLI

Voit asentaa Azure CLI Windows, Linux tai Mac-tietokoneeseen. Seuraavat vaiheet on suoritettava, ennen kuin voit hallita Azure DNS Azure CLI avulla. Lisätietoja on osoitteessa [asentaa Azure-CLI](../xplat-cli-install.md). DNS-komennot vaativat Azure CLI versio 0.9.8 tai uudempi versio.

Kaikki verkon tarjoajan komentoja CLI voi olla löytämät seuraava komento:

    azure network

### <a name="2-switch-cli-mode"></a>2. Vaihda CLI tila

Azure DNS käyttää Azure Resurssienhallinta. Varmista, että voit siirtyä CLI-tilassa ja ARM-komennoilla.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. kirjautuminen Azure-tili

Voit pyydetään todentamismenetelmä tunnistetiedot. Ota huomioon, että voit käyttää vain ORGID tilit.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Valitse tilaus

Valitse, mitä Azure tilauksistasi käyttämään.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. Resurssiryhmän luominen

Azure Resurssienhallinta edellyttää, että kaikki resurssiryhmät, valitse sijainti. Tämä käytetään oletussijainti resurssien resurssin kyseisen ryhmän. Koska kaikki DNS-resurssit ovat yleistä, ei alueellisen, resurssin ryhmän sijainti valinta ei ole vaikutusta Azure DNS.

Voit ohittaa tämän vaiheen, jos käytössäsi on aiemmin resurssiryhmä.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. Rekisteröi

Azure DNS-palvelun hallitsee Microsoft.Network resurssin toimittaja. Azure-tilauksesi on oltava rekisteröity käyttämään tämän resurssin tarjoajaan, ennen kuin voit käyttää Azure DNS. Tämä on jokaisen tilauksen erikseen toiminto.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Vaihe 2: Luo DNS-vyöhyke

DNS-vyöhyke on luotu `azure network dns zone create` komento. Voit myös luoda DNS-vyöhyke sekä tunnisteet. Tunnisteet ovat luettelon nimen ja arvon tietoparin, ja niitä käytetään Azure resurssien hallinnan avulla otsikko resurssien laskutuksen tai ryhmittely. Lisätietoja tunnisteet nähdä [käyttäminen tunnisteet järjestämiseen Azure resurssit](../resource-group-using-tags.md).

Azure DNS-vyöhyke nimet on määritettävä ilman lopetetaan **".'**. Esimerkiksi "**contoso.com**-muodossa sijaan '**contoso.com.**".


### <a name="to-create-a-dns-zone"></a>Luo DNS-vyöhyke

Alla olevassa esimerkissä Luo DNS-vyöhyke, eli *contoso.com* kutsutaan *MyResourceGroup*resurssiryhmä.

Luo DNS-vyöhyke, korvaaminen oman arvot esimerkin avulla.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Luo DNS-vyöhyke ja tunnisteet.

Azure DNS CLI tukee DNS-vyöhykkeet määritetty käyttämällä valinnainen tunnisteet *-tunniste* parametria. Seuraavassa esimerkissä on DNS-vyöhyke luomisesta ja kaksi tunnisteita on projektin = esittely ja kirjekuori = testi.

Alla olevassa esimerkissä avulla voit luoda DNS-vyöhyke ja tunnisteita, korvaaminen oman arvot.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Näytä tietueet

DNS-vyöhyke luominen luo seuraavat DNS-tietueet:

- 'Aloita sekä myöntäjä"(SOA)-tietue. Tämä ei sisällä tietoja jokaisen DNS-vyöhyke ylimmällä tasolla.

- Tärkeimpien (NS) nimipalvelintietueita. Nämä Näytä mitä nimipalvelimet isännöit vyöhykkeen. Azure DNS käyttää nimipalvelimet resurssivarantoon ja hieman eri nimipalvelimet voi määrittää vyöhykkeitä Azure DNS-palvelimesta. Lisätietoja on kohdassa [edustajan Azure DNS toimialue](dns-domain-delegation.md) .

Jos haluat tarkastella tietueista, käytä `azure network dns-record-set show`.<BR>
*Käyttö: verkon dns-tietueen määrittäminen Näytä < resurssi-ryhmän >< dns-vyöhyke-name > <name><type>*


Alla olevassa esimerkissä Jos suoritat komennon resurssin ryhmän *myresourcegroup*, tietueen Määritä nimi *"@"* (for pääkansion tietueen), ja kirjoita *SOA*, se tuottaa seuraavat tulokset:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Voit tarkastella luotuja vyöhykkeen NS-tietueita, käytä seuraavaa komentoa:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Tietueen asettaa pääkansio (tai *apex*) DNS-vyöhyke käytön **@** tietueen määrittää nimi.

## <a name="test"></a>Testi

Voit testata DNS-vyöhyke DNS-työkaluja, kuten nslookup-tarkastella, tai `Resolve-DnsName` PowerShell cmdlet-komennon.

Jos et ole vielä valtuutetun toimialueen käyttämään uutta aluetta Azure DNS-, haluat ohjata suoraan hiirellä yhtä suorakulmion nimipalvelimet vyöhykkeen DNS-kysely. Vyöhykkeen nimipalvelimet esitetään NS-tietueet, kun "azure verkko dns-tietueen määrittäminen Näytä" yllä olevassa luettelossa. Voidaan varmistaa, että korvaava-komennon alla vyöhykkeen oikeat arvot.

Seuraavassa esimerkissä tarkastella kyselyn käyttäminen määritetyt DNS-vyöhyke nimipalvelimia toimialueen contoso.com. Kyselyssä on osoittamaan nimipalvelin, jossa on käytetty * @ * ja käyttämällä tarkastella alueen nimi.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet luonut DNS-vyöhyke, luo [tietue joukot ja tietueiden](dns-getstarted-create-recordset-cli.md) voit käynnistää Internet-toimialueen nimet selvitetään.
