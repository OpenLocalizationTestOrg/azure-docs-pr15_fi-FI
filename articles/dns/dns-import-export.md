<properties
   pageTitle="Tuominen ja vieminen toimialueen Vyöhyketiedosto Azure DNS käyttämällä CLI | Microsoft Azure"
   description="Lue, miten voit tuoda ja viedä DNS-Vyöhyketiedosto Azure DNS Azure CLI avulla"
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

# <a name="import-and-export-a-dns-zone-file-using-the-azure-cli"></a>Tuominen ja vieminen DNS-Vyöhyketiedosto Azure-CLI käyttäminen


Tässä artikkelissa opastaa tuominen ja vieminen DNS-vyöhyke-tiedostojen käyttäminen Azure-CLI Azure DNS.

## <a name="introduction-to-dns-zone-migration"></a>DNS-vyöhyke siirron esittely

DNS-Vyöhyketiedosto on tekstitiedosto, joka sisältää vyöhykkeellä jokaisen (DNS, Domain Name System)-tietueen tiedot. Seuraa vakiotiedostomuoto, että soveltuvat siirtäminen DNS-tietueet DNS-järjestelmiä välillä. Vyöhykkeen tiedoston on pika-luotettavia ja helposti tapa siirtää DNS-vyöhyke sisään tai ulos Azure DNS.

Azure DNS tukee tuominen ja vieminen vyöhykkeen tiedostot Azure komentorivivalitsimet käyttöliittymän (CLI) avulla. Azure-CLI on käytettävä Azure palveluiden hallinta Office kaikissa ympäristöissä komentorivin työkalu. Se on käytettävissä Windows, Mac tai Linux-ympäristöjen [Azure lataussivulle](https://azure.microsoft.com/downloads/). Office kaikissa ympäristöissä tuki on erityisen tärkeää tuominen ja vieminen vyöhykkeen tiedostoja, koska yleisimmät nimi Palvelinohjelmisto, [SITOA](https://www.isc.org/downloads/bind/), toimii yleensä Linux.

## <a name="obtain-your-existing-dns-zone-file"></a>Hanki aiemmin DNS-Vyöhyketiedosto

Ennen kuin tuot DNS-Vyöhyketiedosto Azure DNS, sinun on hankkiminen zone-tiedoston kopio. Jos DNS-vyöhyke tällä hetkellä nykyisessä riippuu tiedoston lähde.

- Jos DNS-vyöhyke nykyisessä kumppanin-palvelu (esimerkiksi toimialuerekisteröijän, erillinen DNS-isännöintipalvelu tai vaihtoehtoinen cloud-palvelu)-palvelun kannattaa antaa pystyy lataamaan DNS-Vyöhyketiedosto.

- Jos DNS-vyöhyke isännöidään Windows DNS-vyöhyke-tiedostojen oletuskansio on **%systemroot%\system32\dns**. DNS-palvelun hallintakonsoli **Yleiset** -välilehdessä näkyy myös kunkin alueen tiedoston koko polku.

- Jos DNS-vyöhyke nykyisessä käyttämällä sidonta-vyöhykkeelle zone-tiedoston sijainti on määritetty SITOMINEN määritysten tiedoston **named.conf**.

**GoDaddy-vyöhyke-tiedostojen käsitteleminen**<BR>
Hieman vakiokoosta muoto on vyöhykkeen GoDaddy ladatut tiedostot. Haluat korjata ongelman, ennen kuin tuotavien Azure DNS zone nämä tiedostot. RData kunkin DNS-tietue DNS-nimet ovat määritettyä nimien, mutta niitä ei ole lopetetaan-. " Tämä tarkoittaa niitä tulkitaan muiden DNS-järjestelmien suhteellinen niminä. Sinun on muokattava Vyöhyketiedosto liittäminen lopetetaan ".", ennen kuin voit tuoda ne Azure DNS heidän nimensä.

## <a name="import-a-dns-zone-file-into-azure-dns"></a>DNS-Vyöhyketiedosto tuominen Azure DNS


Vyöhykkeen tiedoston tuominen luo uusi vyöhyke Azure DNS Jos sellaista ei ole jo. Jos alue on jo olemassa-tietueen joukkojen-vyöhykkeen tiedoston on yhdistetty olemassa olevan tietueen joukot.

### <a name="merge-behavior"></a>Yhdistä toiminta

- Oletusarvon mukaan yhdistetään aiemmin luoduissa että uusissa tietueen joukot. Yhdistetyt tietuejoukon samanlaiset tietueet ovat Poista kaksoiskappaleet.

- Voit myös määrittämällä `--force` vaihtoehdon tuontiprosessin korvaa nykyisen tietueen määrittää uusien tietueiden joukkojen kanssa. Olemassa olevan tietueen joukot, joka ei ole tuodut Vyöhyketiedosto vastaavan tietuejoukon ei poisteta.

- Yhdistettäessä tietueen joukot time to live (TTL) aiemmin luotuja tietueen siirtämistä käytetään. Kun `--force` on käyttänyt, uusi tietuejoukon TTL käytetään.

- Myöntäjä (SOA) parametrit alkuun (lukuun ottamatta `host`) otetaan aina tuodut Vyöhyketiedosto riippumatta siitä, onko `--force` käytetään. Vastaavasti nimi server määrittää vyöhykkeen apex-tietueen TTL aina haetaan tuodut Vyöhyketiedosto.

- Tuodun CNAME-tietueen ei korvaa olemassa olevan CNAME-tietueen nimi paitsi `--force` -parametri on määritetty.

- Kun syntyy CNAME-tietue ja toisen tietueen, sama nimi mutta erilaisia tyyppi (riippumatta siitä, jotka on aiemmin luotu tai uusi) välillä, säilyttää olemassa olevaan tietueeseen. Tämä on erillinen käytön `--force`.

### <a name="additional-information-about-importing"></a>Lisätietoja tietojen tuomisesta

Seuraavat huomautukset on teknisiä lisätietoja vyöhykkeen tuonti.

- `$TTL` Direktiivin on valinnainen, ja se on tuettu. Ei `$TTL` direktiivin annetaan, tietueet ilman eksplisiittinen TTL on tuotu arvoksi oletusarvoista TTL-arvoksi 3 600 sekuntia. Kaksi tietuetta samaan tietuejoukon määrittäessäsi eri TTLs alempaa arvoa käytetään.

- `$ORIGIN` Direktiivin on valinnainen, ja se on tuettu. Ei `$ORIGIN` on määritetty, käytetään oletusarvo on määritetty komentorivillä vyöhykkeen nimi (plus lopetetaan-. ").

- `$INCLUDE` Ja `$GENERATE` direktiivejä ei tueta.

- Näiden tietuetyyppien tuetaan: A, AAAA, CNAME, MX, NS, SOA, SRV ja TXT.

- SOA tietue luodaan automaattisesti Azure DNS: n perusteella vyöhykkeen luomisen yhteydessä. Kun tuot vyöhykkeen tiedoston, kaikki SOA parametrit otetaan-vyöhykkeen tiedoston *lukuun ottamatta* `host` parametria. Tämä parametri käyttää myöntämä Azure DNS-arvoa. Tämä johtuu siitä tämä parametri on viitattava Azure DNS myöntämä ensisijainen nimipalvelin.

- Määrittää vyöhykkeen apex nimi palvelimen tietue luodaan myös automaattisesti Azure DNS: n perusteella vyöhykkeen luomisen yhteydessä. Vain tämän tietuejoukon TTL on tuotu. Nämä tietueet sisältävät Azure DNS myöntämä nimi palvelinten nimet. Tuodut Vyöhyketiedosto sisältämiä arvoja ei korvata tietueen tiedot.

- Julkinen esikatselun aikana Azure DNS tukee vain yhden merkkijonon TXT-tietueet. Multistring TXT-tietueet yhdistetään ja katkaistaan 255 merkkiä.

### <a name="cli-format-and-values"></a>CLI muoto ja arvot


Voit tuoda DNS-vyöhyke Azure CLI-komennon muoto on:<BR>`azure network dns zone import [options] <resource group> <zone name> <zone file name>`

Arvot:

- `<resource group>`on Azure DNS-vyöhyke resurssiryhmän nimi.
- `<zone name>`on alueen nimi.
- `<zone file name>`on tuomista zone-tiedoston polku ja nimi.

Jos alue, jonka nimi ei ole resurssiryhmän, se luodaan puolestasi. Jos alue on jo, tuodun tietueen joukot yhdistetään aiemmin tietueen joukot. Voit korvata olemassa olevan tietueen joukot `--force` vaihtoehto.

Voit tarkistaa vyöhykkeen-tiedoston muoto tuomatta todella `--parse-only` vaihtoehto.

### <a name="step-1-import-a-zone-file"></a>Vaihe 1. Alueen tiedoston tuonnin

Voit tuoda vyöhykkeen tiedoston vyöhykkeen **contoso.com**.

1. Kirjaudu Azure tilauksen Azure-CLI avulla.

        azure login

2. Valitse tilaus, johon haluat luoda uuden DNS-vyöhyke.

        azure account set <subscription name>

3. Azure DNS on Azure vain Resurssienhallinta-palvelu, jotta Azure-CLI on vaihdettu Resurssienhallinta-tilaan.

        azure config mode arm

4. Ennen kuin käytät Azure DNS-palvelun, sinun on rekisteröitävä tilauksen Microsoft.Network resurssi-palveluntarjoajan käyttäminen. (Tämä on jokaisen tilauksen erikseen toiminto).

        azure provider register Microsoft.Network

5. Jos sitä ei ole vielä ole, haluat myös luoda Resurssienhallinta resurssiryhmän.

        azure group create myresourcegroup westeurope

6. Tuo vyöhykkeen **contoso.com** tiedoston **contoso.com.txt** uusi DNS-vyöhyke, valitse resurssi-ryhmän **myresourcegroup**-komennon suorittaminen `azure network dns zone import`.<BR>Tämä komento vyöhykkeen-tiedoston lataaminen ja jäsentämään sen. Komento suorittaa sarjan komentoja vyöhykkeen luominen Azure DNS-palvelun ja kaikki tietueen määrittää vyöhykkeen. Komento myös raportoi edistymistä sekä mahdolliset virheet tai varoitukset konsoli-ikkunassa. Koska tietueen joukot luodaan sarjan, voi kestää muutaman minuutin kuluttua suuren alueen tiedoston tuonnin.

        azure network dns zone import myresourcegroup contoso.com contoso.com.txt



### <a name="step-2-verify-the-zone"></a>Vaihe 2. Tarkista vyöhykkeen

Voit tarkistaa DNS-vyöhyke, kun olet tuonut tiedoston, voit käyttää jollakin seuraavista tavoista:

- Voit lisätä tietueita Azure CLI-komennon avulla.

        azure network dns record-set list myresourcegroup contoso.com

- Voit lisätä tietueita PowerShell cmdlet-komennolla `Get-AzureRmDnsRecordSet`.

- Voit käyttää `nslookup` vahvistamiseksi nimenselvitys tietueista. Koska vyöhykkeen ei ole vielä valtuutetun, sinun on Määritä oikea Azure DNS-nimipalvelimet erikseen. Alla olevassa esimerkissä noutamisesta vyöhykkeeseen nimi palvelinten nimet. SE näyttää myös, miten kyselyn "www-tietue käyttämällä `nslookup`.

        C:\>azure network dns record-set show myresourcegroup contoso.com @ NS
        info:Executing command network dns record-set show
        + Looking up the DNS Record Set "@" of type "NS"
        data:Id: /subscriptions/…/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
        data:Name: @
        data:Type: Microsoft.Network/dnszones/NS
        data:Location: global
        data:TTL : 3600
        data:NS records
        data:Name server domain name : ns1-01.azure-dns.com
        data:Name server domain name : ns2-01.azure-dns.net
        data:Name server domain name : ns3-01.azure-dns.org
        data:Name server domain name : ns4-01.azure-dns.info
        data:
        info:network dns record-set show command OK

        C:\> nslookup www.contoso.com ns1-01.azure-dns.com

        Server: ns1-01.azure-dns.com
        Address:  40.90.4.1

        Name:www.contoso.com
        Addresses:  134.170.185.46
        134.170.188.221

### <a name="step-3-update-dns-delegation"></a>Vaihe 3. Päivitä DNS-delegointi

Kun olet varmistanut, että vyöhykkeen on tuotu oikein, sinun on päivitettävä Azure DNS-nimipalvelimet osoittamaan DNS-delegointi. Lisätietoja on artikkelissa [Päivitä DNS-delegointi](dns-domain-delegation.md).

## <a name="export-a-dns-zone-file-from-azure-dns"></a>DNS-vyöhyke-tiedoston vieminen Azure DNS

Voit tuoda DNS-vyöhyke Azure CLI-komennon muoto on:<BR>`azure network dns zone export [options] <resource group> <zone name> <zone file name>`

Arvot:

- `<resource group>`on Azure DNS-vyöhyke resurssiryhmän nimi.
- `<zone name>`on alueen nimi.
- `<zone file name>`on viedään zone-tiedoston polku ja nimi.

Kuin vyöhykkeen, tuontia voit ensin on kirjautua sisään, valitse tilauksesi ja Määritä Azure-CLI Resurssienhallinta-tilaa.

### <a name="to-export-a-zone-file"></a>Vyöhykkeen tiedoston vieminen


1. Kirjaudu Azure tilauksen Azure-CLI avulla.

        azure login

2. Valitse tilaus, johon haluat luoda uuden DNS-vyöhyke.

        azure account set <subscription name>

3. Azure DNS on Azure vain Resurssienhallinta-palvelu. Azure-CLI on vaihdettu Resurssienhallinta-tilaan.

        azure config mode arm

4. Voit viedä aiemmin Azure DNS zone **contoso.com** resurssin ryhmän **myresourcegroup** -tiedoston **contoso.com.txt** (-nykyisen kansion), suorittamalla `azure network dns zone export`. Tämä komento soittaa tietueen joukkojen-vyöhykkeen numerointi ja viedä tulokset sidonta-yhteensopiva Vyöhyketiedosto Azure DNS-palvelun.

        azure network dns zone export myresourcegroup contoso.com contoso.com.txt

