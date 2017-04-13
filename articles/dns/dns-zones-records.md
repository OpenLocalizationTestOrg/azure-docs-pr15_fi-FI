<properties 
   pageTitle="DNS-vyöhykkeet ja tietueet | Microsoft Azure" 
   description="Yleistä tuki isännöinnin DNS-vyöhykkeet ja Microsoft Azure DNS-tietueet." 
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
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS-vyöhykkeet ja tietueet

Tällä sivulla kerrotaan käsitteistä toimialueet, DNS-vyöhykkeet ja DNS-tietueet ja tietueen joukot ja miten niitä voi käyttää Azure DNS.

## <a name="domain-names"></a>Toimialueiden nimet
 
Domain Name System on toimialueen hierarkian. Hierarkian alkaa '' päätoimialue, jonka nimi on yksinkertaisesti**.**.  Alapuolelle tulee ylimmän tason toimialueet, kuten "com', 'nettonykyarvon',"organisaatiokaavion","USA"tai"jp".  Nämä alla on toisen tason toimialueet, kuten "org.uk" tai "co.jp". Yleisesti toimialueen DNS-hierarkian jaetaan, ylläpitää DNS-nimipalvelimet eri puolilla maailmaa.

Toimialuerekisteröijältä on organisaatio, jonka avulla voit ostaa toimialuenimi, kuten "contoso.com".  Toimialuenimen ostaminen antaa oikeuden hallita DNS-hierarkia, että nimi-kohdassa, joilla voit ohjata nimi "www.contoso.com" yrityksen sivuston esimerkiksi. Rekisteröijästä voi olla oma nimipalvelimet puolestasi toimialueen tai Vaihtoehtoisesti voit määrittää vaihtoehtoiset nimipalvelimet.

Azure DNS on yleisesti eri aikajaksoille, suuren käytettävyyden nimi palvelimen infrastruktuuri, jonka avulla toimialueesi isännöimiseen. Sijoittamalla Azure DNS-toimialueen, voit hallita tunnistetietoja, API, työkalut, Laskutus ja tukea DNS-tietueiden Azure muiden palvelujen.

Azure DNS ei tällä hetkellä tue ostamalla toimialuenimien. Jos haluat ostaa toimialuenimi, sinun on käytettävä kolmannen osapuolen toimialuerekisteröijälle. Rekisteröijästä yleensä veloittaa pieni vuosimaksu. Toimialueet on isännöitävä sitten Azure DNS-DNS-tietueiden hallintaa varten. Katso lisätietoja [edustajan Azure DNS toimialue](dns-domain-delegation.md) .

## <a name="dns-zones"></a>DNS-vyöhykkeet 

DNS-vyöhyke käytetään isännöimiseen tietyn toimialueen DNS-tietueet. Aloita toimialueen Azure DNS-isännöintipalvelu, haluat luoda kyseisen toimialueen kohdalta DNS-vyöhyke. Kunkin toimialueen DNS-tietue luodaan sitten DNS-vyöhykkeen sisällä. 

Esimerkiksi toimialueen "contoso.com" voi olla useita DNS-tietueita, kuten mail.contoso.com (for postipalvelimeen) ja www.contoso.com (sivustolle).

Luodessasi DNS-vyöhyke Azure DNS-vyöhyke nimen on oltava yksilöivä resurssiryhmän. Alueen nimi voidaan käyttää eri resurssiryhmä tai eri Azure-tilauksen. Jos useita alueita, joilla on sama nimi, jokaiselle esiintymälle määritetään eri nimipalvelimen osoitteet. Vain yksi joukkoon osoitteita voi määrittää nimen toimialuerekisteröijän.

>[AZURE.NOTE] Sinun ei tarvitse omista luomaan DNS-vyöhyke sisältää kyseisen toimialueen kohdalta Azure DNS-toimialuenimi. Haluat kuitenkin omistat toimialueen, määrittää Azure DNS-nimipalvelimet toimialuenimeä toimialuenimen Rekisteröintipalvelun oikea nimipalvelimet.

Lisätietoja on artikkelissa [edustajan Azure DNS toimialue](dns-domain-delegation.md).

## <a name="dns-records"></a>DNS-tietueet

### <a name="record-types"></a>Tietuetyypit

Kukin DNS-tietue on nimi ja tyyppi. Tietueet on järjestetty eri ne sisältävät tietojen mukaan. Yleisin on "A" tietueeseen, joka yhdistää nimi IPv4-osoite. Yleisiä on 'MX'-tietueen, joka yhdistää postipalvelimen nimi.

Azure DNS tukee kaikkia Yleiset DNS-tietuetyypit: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV ja TXT.

### <a name="record-names"></a>Tietueiden nimet

Azure DNS-tietueet määritetään käyttämällä suhteellinen nimiä. *Täydellinen* toimialuenimi (FQDN) sisältää alueen nimi, *suhteellinen* nimi ei. Suhteellinen tietueen nimi vyöhykkeellä "contoso.com" WWW-tunnistetta antaa esimerkiksi täydellinen tietueen nimi "www.contoso.com".

*Apex* tietue on pääkansio (tai *apex*) DNS-tietue DNS-vyöhyke. Esimerkiksi DNS-vyöhykkeellä "contoso.com" apex tietue on myös täydellinen nimi "contoso.com" (tätä kutsutaan joskus *pelkistetty* toimialue).  Käytännön suhteellinen nimen mukaan '@' avulla luodaan apex tietueita.

### <a name="record-sets"></a>Tietueen joukot
Joskus sinun täytyy luoda useamman kuin yhden DNS-tietueen annettu nimi ja tyyppi. Oletetaan esimerkiksi, että 'www.contoso.com' web-sivuston isännöidään kaksi eri IP-osoitteisiin. Sivuston edellyttää kaksi eri tietuetta, yksi kullekin IP-osoite. Seuraavassa on esimerkki tietuejoukon:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS hallitsee DNS-tietueet käyttämällä *tietueen asettaa*. Tietuejoukko (tunnetaan myös nimellä *resurssin* tietuejoukon) on kokoelma DNS-tietueiden vyöhykkeessä, jotka on sama nimi ja samaa tyyppiä. Useimmat tietueen joukot sisältää vastaavan tietueen, mutta esimerkkejä katsomalla, joiden tietuejoukon on useampi kuin yksi tietue eivät ole epätavallisten.

Esimerkiksi oletetaan, että olet jo luonut A-tietue "www" alue "contoso.com", osoittamalla IP osoite "134.170.185.46" (ensimmäisen tietueen yllä).  Luo toinen tietue sinun Lisää tietue luodun tietuejoukon sijaan luoda uuden tietueen.

SOA ja CNAME-tietuetyypit on poikkeus. DNS-standardit eivät salli useita tietueita saman niminen näiden, näin tietueen joukot voivat sisältää vain yhden tietueen.

### <a name="time-to-live"></a>Time-to-live

Oletusaika tai TTL-määrittää, kuinka kauan kunkin tietueen on välimuistiin asiakkaat ennen uudelleen kyselyä. Edellä olevassa esimerkissä TTL on 3 600 sekuntia tai 1 tunti.

Azure DNS-TTL on määritetty ei kutakin tietuetta varten tietuejoukon käytettäväksi on sama arvo, tietuejoukon kaikki tietueet.  Voit määrittää minkä tahansa väliltä 1 ja 2 147 483 647 sekuntia TTL-arvoa.

### <a name="wildcard-records"></a>Yleismerkkien tietueet

Azure DNS tukee [yleismerkkien tietueita](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Yleismerkkien tietueet palautetaan vastauksena kyselyä vastaava nimellä (paitsi-yleismerkki tietuejoukon tarkempi vastinetta ei). Yleismerkkien tietueen joukot tuetaan kaikkien tietuetyyppien NS ja SOA lukuun ottamatta.  

Voit luoda yleismerkkien tietuejoukon käyttämällä tietuejoukon nimi "\*". Vaihtoehtoisesti voit myös käyttää nimi, jossa on\*"otsikkona sen vasemmanpuoleisimmasta, esimerkiksi"\*.foo ".

### <a name="cname-records"></a>CNAME-tietueet

CNAME-tietueen joukkoja ei voi olla saman niminen muita tietueen joukkoja. Esimerkiksi et voi luoda CNAME-tietue, joka määrittää suhteellisen nimellä "www" ja A-tietue suhteellinen nimellä "www" samanaikaisesti.

Koska alueen apex (nimen = ‘@’) sisältää aina NS ja SOA tietueeseen joukot, jotka on luotu vyöhykkeen luonnin yhteydessä, et voi luoda määrittää vyöhykkeen apex CNAME-tietue.

Näiden rajoitusten aiheutuvat DNS-standardit ja eivät ole Azure DNS rajoitukset.

### <a name="ns-records"></a>NS-tietueet

NS-tietuejoukon luodaan automaattisesti kunkin alueen yläreunassa (nimen = ‘@’), ja poistetaan automaattisesti, kun vyöhyke poistetaan (sitä ei voi poistaa erikseen).  Voit muokata tätä tietuejoukon TTL, mutta et voi muokata tietueita, jotka ovat valmiiksi määritetyn viittaamaan vyöhykkeeseen Azure DNS-nimipalvelimet.

Voit luoda ja poistaa vyöhykkeellä ei ole vyöhykkeen apex osoitteessa muiden NS-tietueet.  Näin voit määrittää lapsen alueet (katso [Ryhmäkäytännön alitoimialueisiin Azure DNS](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)).

### <a name="soa-records"></a>SOA tietueet

SOA tietuejoukon luodaan automaattisesti kunkin alueen yläreunassa (nimen = ‘@’), ja poistetaan automaattisesti, kun vyöhyke poistetaan.  SOA tietueita ei voi luoda tai poistaa erikseen. 

Voit muokata kaikkia ominaisuuksia SOA tietueen lukuun ottamatta "isäntä"-ominaisuutta, joka on valmiiksi viitata Azure DNS myöntämä ensisijainen nimi palvelimen nimi.

### <a name="spf-records"></a>SPF-tietueet

Sender Policy Framework (SPF) tietueiden käytetään määrittämisestä minkä sähköpostipalvelimien on oikeus lähettää sähköpostia puolesta määritetyn toimialuenimi.  Konfiguroinnin SPF-tietueet on tärkeää estä merkintä 'roskapostiksi' sähköpostin vastaanottajia.

DNS-RFC käyttöön alun perin uuden 'SPF-tietuetyypin tukemaan Tämä skenaario. Tukemaan vanhempia nimipalvelimet ne nimenomaisesti myös Käytä TXT-tietuetyypin Määritä SPF-tietueet.  Tämä moniselitteisyyden johtanut sekaannusta, jossa on poistunut [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  Tämä ilmaistu SPF-tietueet on vain luodaan käyttämällä tietuetyypiksi TXT ja poistettu SPF-tietueen tyyppi. 

**SPF-tietueet tukemat Azure DNS ja luodaanko TXT-tietuetyypin avulla.** Vanhentuneen SPF-tietuetyyppi ei tueta. Kun [tuominen DNS-vyöhykkeen tiedoston](dns-import-export.md), kaikki SPF-tietueet käyttämällä SPF-tietuetyypin muunnetaan TXT-tietuetyyppi. 

### <a name="srv-records"></a>SRV-tietueet

[SRV-tietueiden](https://en.wikipedia.org/wiki/SRV_record) käytetään eri palveluissa Määritä palvelimen sijainnit. Kun määrität Azure DNS SRV-tietue:

- *Palvelu* - ja *protokolla* on määritettävä tietuejoukon nimeen, etuliite alaviivoja.  Esimerkiksi "\_sip. \_tcp.name ".  Tietueen etsiminen vyöhykkeen apex ei ei tarvitse määrittää '@' tietueen nimi, riittää, että Käytä palvelu- ja protokolla, esimerkiksi "\_sip. \_tcp ".
- *Prioriteetti*, *leveys*, *portin*ja *kohde* määritetään kunkin tietueen tietuejoukon parametreiksi. 

## <a name="tags-and-metadata"></a>Tunnisteet ja metatiedot

### <a name="tags"></a>Tunnisteet
Tunnisteet ovat luettelon nimen ja arvon tietoparin, ja niitä käytetään Azure resurssien hallinnan avulla otsikko resurssit.  Azure Resurssienhallinta käyttää tunnisteita, jotta suodatettujen näkymien Azure lasku ja avulla voit määrittää käytännön, jolla tunnisteet tarvitaan myös. Lisätietoja tunnisteet nähdä [käyttäminen tunnisteet järjestämiseen Azure resurssit](../resource-group-using-tags.md).

Azure DNS tukee Azure Resurssienhallinta-ohjauskoodien käyttäminen DNS zone resursseja.  Se ei tue tunnisteita DNS-tietueen joukoissa.

### <a name="metadata"></a>Metatietojen

Tietuejoukon tunnisteet vaihtoehtoisena Azure DNS tukee tietueen joukot ' metatietojen ' lisätä huomautuksia.  Kuten tunnisteita, metatietojen avulla voit yhdistää nimen ja arvon tietoparin kunkin tietuejoukon.  Tämä voi olla hyödyllistä, kuten tietue kunkin tietuejoukon käyttötarkoituksen.  Toisin kuin tunnisteet metatietojen ei voi käyttää antamaan Azure laskun suodatettua näkymää ja ei voi määrittää Azure Resurssienhallinta-käytäntö.

## <a name="limits"></a>Rajoitukset

Seuraavat oletusarvon raja-arvot koskevat käytettäessä Azure DNS:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Seuraavat vaiheet

- Aloita Azure DNS: n avulla, katso, miten [DNS-vyöhyke](dns-getstarted-create-dnszone-portal.md) ja [DNS-tietueiden luominen](dns-getstarted-create-recordset-portal.md).
- Voit siirtää olemassa olevat DNS-vyöhyke, lue [tuominen](dns-import-export.md)ja DNS-Vyöhyketiedosto.
