<properties
    pageTitle="Azure Premium tallennustila: Suunnitella suorituskyvyn | Microsoft Azure"
    description="Suunnittele tehokas sovellusten Azure Premium-tallennustilan. Premium tallennustila on tehokas, pieni viive levyn I/O-paljon työmääriä käynnissä Azuren näennäiskoneiden-tuki."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium tallennustila: Erinomainen suorituskyky rakenteen

## <a name="overview"></a>Yleiskatsaus  
Tässä artikkelissa on ohjeita erinomainen suorituskyky-sovellusten käyttäminen Azure Premium-tallennustilan. Voit yhdistää suorituskykyä koskevat sovelluksen käyttämät tekniikat parhaita käytäntöjä tässä asiakirjassa annettuja ohjeita. Esitä Profiilikuva on on käytetty SQL Serveriä Premium säilössä esimerkkinä tässä asiakirjassa.

Tallennustilan kerros tämän artikkelin suorituskyvyn tilanteita, joissa on osoite, kun haluat optimoida sovelluskerroksen. Jos isännöit SharePoint-klusterin Azure Premium säilössä, voit voit käyttää SQL Server-Esimerkkejä tästä artikkelista optimoi tietokantapalvelimen. Lisäksi voit optimoida SharePoint-klusterin verkkopalvelin ja saat useimmat suorituskyvyn sovelluspalvelin.

Tässä artikkelissa kerrotaan, miten answer seuraavan sovelluksen optimoinnista Azure Premium säilössä usein kysyttyjä kysymyksiä

-   Miten sovellusten suorituskykyä?  
-   Miksi ovat voit näe hyvin suorituskykyä?  
-   Tekijät vaikuttaa sovelluksen suorituskyvyn Premium säilössä?  
-   Miten vaikuttavista seikoista Premium säilössä sovelluksen suorituskykyä?  
-   Miten voi optimoida IOPS, kaistanleveys ja viive?  

On annettu ohjeita erityisesti Premium tallentamista varten, koska työmääriä Premium tallennustilan käytössä on erittäin luottamuksellisia suorituskykyä. On annettu esimerkkejä tarvittaessa. Voit myös käyttää joitakin ohjeita käytössä IaaS VMs vakio tallennustilan levyjen sovelluksia.

Ennen kuin aloitat, jos ole aiemmin käyttänyt Premium tallennuskiintiön, lue ensin [Premium tallennustilan: tehokas tallennustila Azure virtuaalikoneen työmääriä](storage-premium-storage.md) artikkelin ja [Azure Premium tallennustilan skaalattavuus ja suorituskyvyn kohteet](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Sovelluksen suorituskykyilmaisimia  
Olemme arvioida, onko sovellus toimii hyvin tai ei käytössä suorituskyvyn ilmaisimet, kuten, kuinka nopeasti sovelluksen käsittelee käyttäjäpyyntö, kuinka paljon tietoja sovelluksen käsittelee pyynnön kohti, kuinka monta pyytää on tietyn ajanjakson aikana, kuinka kauan odottamaan vastauksen saaminen jälkeen pyynnön käyttäjällä sovelluksen käsittely. Nämä suorituskykyilmaisimia tekniset ehdot ovat IOPS, siirtonopeuden tai kaistanleveys ja viive.

Tässä osassa käsittelemme yleisiä suorituskykyilmaisimia Premium tallennustilan kontekstissa. Seuraavassa osassa kerätään sovelluksen vaatimukset kerrotaan, miten näitä suorituskykyilmaisimia sovelluksen. Myöhemmin-sovelluksen suorituskyvyn optimointi Lisätietoja näiden KPI-ilmaisimien ja suositukset optimoi ne vaikuttavat tekijät.

## <a name="iops"></a>IOPS  
IOPS on luku, sovelluksen on lähettäminen tallennustilan levyille sekunnin pyyntöihin. I/toiminto voi lukea tai kirjoittaa peräkkäisiä tai satunnaisluku. OLTP-sovelluksia, kuten verkkokauppa-sivustossa on käyttänyt monta samanaikaiset pyynnöt heti. Käyttäjien pyyntöjen on Lisää, ja Päivitä tehostettu tietokannan tapahtumat, jotka sovellus on käsiteltävä nopeasti. Tämän vuoksi OLTP-sovellukset edellyttävät erittäin suuri IOPS. Näiden sovellusten käsitellä miljoonia pienten ja satunnaisia IO pyynnöt. Jos sinulla on jokin sovellus, suunnittelet sovelluksen infrastruktuurin optimointi IOPS. Myöhemmin *Optimointi sovelluksen suorituskyky*-osassa käsiteltävät aiheet tiedot, jotka on otettava huomioon pääset hyvin IOPS tekijät.

Kun liität premium tallennustilan DVD-levyllä hyvin asteikko AM, Azure säännösten puolestasi taattua IOPS määrä levyn määrityksen mukaan. Esimerkiksi P30 DVD-levyllä valmistelee 5000 IOPS. Kukin hyvin asteikko AM kokoa on myös tiettyjen IOPS rajoitukset, se voidaan ylläpitää. Oletetaan esimerkiksi vakio GS5 AM on 80,000 IOPS rajoittaa.

## <a name="throughput"></a>Siirtonopeuden  
Siirtonopeuden tai kaistanleveys on sovelluksesi lähettää tallennustilan levyille määritetyn aikavälin tietojen määrään. Jos sovelluksen suorittaa i/toimintoja, joiden suuri IO yksikön koot, edellyttää hyvin siirtonopeuden. Varaston sovelluksia yleensä ongelma tarkistuksen tehostettu toiminnoista, joita käyttää suuri osiin tiedot kerrallaan ja suorittaa yleisesti samalla kertaa. Toisin sanoen sovellukset edellyttävät suurempi siirtonopeuden. Jos sinulla on jokin sovellus, suunnittelet sen infrastruktuurin optimointi siirtonopeuden. Seuraavan osion käsiteltävät aiheet tiedot on paranna tätä tekijät.

Kun liität premium tallennustilan DVD-levyllä hyvin asteikolla AM, Azure säännösten siirtonopeuden kyseisen levyn määrityksen mukaan. Esimerkiksi P30 DVD-levyllä valmistelee 200 Mt toinen levy siirtonopeuden kohden. Kukin hyvin asteikko AM kokoa on myös tiettyjä nopeus-rajoitus, se voidaan ylläpitää. Oletetaan esimerkiksi vakio GS5 AM on suurin nopeus 2 000 megatavun sekunnissa.

Tällä siirtonopeuden ja IOPS alla kaavan mukaisesti välisen suhteen.

![](media/storage-premium-storage-performance/image1.png)

Tämän vuoksi on tärkeää optimaalisen siirtonopeuden ja IOPS arvojen, sovellus edellyttää. Kun yrität optimoida jokin toinen saa vaikuttaa myös. Myöhemmin-osassa *Optimointi sovelluksen suorituskyvyn*käsittelemme-lisätietoja optimoiminen IOPS ja siirtonopeuden.

## <a name="latency"></a>Viive  
Viive on sovelluksen yksittäisen pyynnön, lähettäminen tallennustilan-levyille ja lähetä vastaus asiakkaalle kuluvaa aikaa. Tämä on sovelluksen suorituskyvyn lisäksi IOPS ja siirtonopeuden kriittinen mitta. Premium tallennustilan DVD-levyllä viive on noutaa tiedot pyynnön ja toimittaa sen sovelluksen kuluvaa aikaa. Premium tallennustila on yhtenäinen pienen viiveet suurempia. Jos otat vain luku-isännän välimuistiin premium tallennustilan levyille, saat paljon pienempi luku viive. Käsittelemme levyn välimuistiin tarkemmin myöhemmin osan *optimointi sovelluksen*suorituskykyyn.

Kun optimointi sovelluksesi suurempi IOPS ja siirtonopeuden, se vaikuttaa sovelluksen viive. Jälkeen säätäminen sovelluksen suorituskyvyn arvioiminen aina sovelluksen välttää odottamattomat odotusaika viive.

## <a name="gather-application-performance-requirements"></a>Kerää sovelluksen suorituskyvyn vaatimukset  
Erinomainen suorituskyky sovellusten Azure Premium tallennustilan suunnittelussa ensimmäiseksi on ymmärtää sovelluksen vaatimukset. Kun sinulla on tarvitsemasi suorituskykyä koskevat, voit optimoida sovelluksesi eniten parhaan mahdollisen suorituskyvyn saavuttamiseksi.

Edellisessä osassa on selitetty yleisiä suorituskykyilmaisimia, IOPS, liikenteen ja viive. Sinun on tunnistettava joka nämä suorituskykyilmaisimia ovat kriittisiä sovelluksen haluamasi käyttäjäkokemuksen aikana. Esimerkiksi suuren IOPS tarvittavia OLTP sovellusten käsittely tapahtumat miljoonia sekunneiksi. Suuri siirtonopeuden on kriittinen tietovarasto sovellusten käsittely suuria tietomääriä sekunneiksi. Erittäin pienen viive on tärkeää reaaliaikainen sovelluksia, kuten videokuvaa streaming sivustot.

Seuraavaksi mittaa parhaan mahdollisen suorituskyvyn vaatimukset sovelluksesi koko aikana. Määritä alkamis otoksen tarkistuslista. Tallentaa parhaan mahdollisen suorituskyvyn vaatimukset aikana Normaali, huippu ja off-hours kuormituksen pisteitä. Tunnistamalla työmääriä tasojen vaatimukset osaat määrittää sovelluksen yleinen suorituskyvyn vaatimus. Esimerkiksi e commerce-sivuston normaalin työmäärän ovat tapahtumia, se on useimmissa päivää vuodessa. Huippu-työmäärää sivuston ovat tapahtumia, se toimii aikana tai erityinen myyntiä tapahtumien aikana. Huippu-työmäärää on yleensä kokeneilta rajoitetun ajan, mutta edellyttää, että sovelluksesi skaalata vähintään kaksi kertaa normaalisti. Selvitä 50 prosenttipiste, 90 prosenttipiste ja 99 prosenttipiste vaatimuksia. Näin suorituskyky minkä tahansa harha suodattamaan ja vaivaa voit keskittyä optimoiminen oikeita arvoja.

**Sovelluksen suorituskyvyn vaatimusten tarkistusluettelo**

| **Suorituskyvyn vaatimukset** | **50 prosenttipiste** | **90 prosenttipiste** | **99 prosenttipiste** |
|---|---|---|---|
| Maks. Tapahtumia sekunnissa | | | |
| % Luku toiminnot            | | | |
| Kirjoita %-toiminnot           | | | |
| % Satunnaisia toiminnot          | | | |
| % Peräkkäiset toiminnot      | | | |
| IO-pyynnön koko              | | | |
| Keskimääräinen nopeus           | | | |
| Maks. Siirtonopeuden              | | | |
| Min. Viive                 | | | |
| Keskimäärin              | | | |
| Maks. SUORITIN                     | | | |
| Suorittimen keskiarvo                  | | | |
| Maks. Muisti                  | | | |
| Keskimääräinen muistia               | | | |
| Jonon pituus                  | | | |

>**Tärkeä huomautus:**  
>Ota huomioon skaalaus näytetyt määrät perusteella arvioidut tulevaisuudessa kasvu-sovelluksen. On hyvä suunnitteleminen kasvu etuajassa, koska se voi olla vaikea infrastruktuuri myöhemmin suorituskyvyn parantaminen muuttamiseen.

Jos olet aiemmin luodun sovelluksen ja haluat siirtää Premium-tallennustilan, muodosta ensin edellä tarkistusluettelo aiemmin luotua sovellusta. Tämän jälkeen muodosta prototyypin sovelluksesi Premium säilössä ja rakenne-sovelluksen ohjeet on kuvattu tämän artikkelin myöhemmin osio *Optimointi sovelluksen suorituskyvyn* perusteella. Seuraavassa osassa kerrotaan työkalujen avulla voit kerätä suorituskyvyn mitat.

Luo tarkistusluettelon kaltainen prototyypin aiemmin sovelluksesi. Benchmarking työkaluilla voit simuloida työtaakkaa ja prototyypin sovelluksen suorituskykyä. Katso lisätietoja [Benchmarking](#benchmarking) . Seuraavilla tavoilla, jotta voit tarkistaa, onko Premium tallennustilan vastaamaan tai ylittäisivät sovelluksen suorituskyvyn tarpeen. Sitten voit toteuttaa tuotannon sovelluksen samoja ohjeita.

### <a name="counters-to-measure-application-performance-requirements"></a>Laskureita mitata sovelluksen suorituskyvyn vaatimukset  
Paras tapa mitata-sovelluksen suorituskykyvaatimukset on antama palvelimen käyttöjärjestelmän suorituskyvyn seurantaa työkaluilla. Voit käyttää PerfMon Windows ja iostat Linux. Kyseisten työkalujen siepata laskureita vastaavat kunkin toimenpiteen selitetty edellä olevassa osassa. Nämä laskureita arvot on siepata, kun sovellus on käynnissä, sen Normaali, huippu ja off-hours toiminnoista.

Laskurien käytettävyyteen ovat käytettävissä suorittimen, muistin ja kunkin loogisen levyn ja fyysinen levy Serverin. Käytettäessä premium tallennustilan levyjä AM-levyltä lukemalla koskevat kunkin premium tallennustilan levyn ja loogiset ovat jokaisen aseman luotu premium tallennustilan levyille. Sinun on käännettävä levyjä, jotka isännöivät sovelluksen työmäärän arvot. Jos näkyvissä on looginen ja fyysinen levyjen yksi yhteen yhdistämisen, voit katsoa levyltä lukemalla; Muussa tapauksessa viitata loogisen levyn laskureita. Linux-iostat-komento luo suorittimen ja levyn käyttö-raportin. Levyn käyttö-raportin on fyysistä laitetta tai osion kohden. Jos sinulla on tietokantapalvelimen kanssa data- ja lokitiedostojen erillisessä levyille, kerää molemmille levyille tiedot. Seuraavassa taulukossa on kuvattu laskureita levyjä, suorittimen ja muistin:

| Laskuri | Kuvaus | PerfMon | Iostat |
|---|---|---|---|
| **IOPS tai tapahtumia sekunnissa** | Tallennustilan levylle sekunnissa myöntänyt i/o-pyyntöjen määrä. | Levyn lukuja/s <br> Levyn kirjoituksia/s | TP merkinnät <br> r/s <br> w/s |
| **Levyn lukee ja kirjoittaa** | prosenttia lukee ja kirjoittaa levyn suorittaa toimintoja. | Levyn luku prosentteina <br> Levyn kirjoittaminen prosentteina | r/s <br> w/s |
| **Siirtonopeuden** | Tietojen lukeminen tai kirjoitettu levylle sekunnissa määrää. | Levyn tavua/s <br> Levyn tavua/s | kB_read/s <br> kB_wrtn/s |
| **Viive** | Kokonaisaika levyn IO pyyntöä. | Keskimääräinen levyn s/luettu <br> Keskiarvoista s/kirjoittaminen | odotettava <br> svctm |
| **IO-koko** | I/o koon pyytää tallennustilan levyille ongelmat. | Keskiarvoista tavua/luettu <br> Keskiarvoista tavua/kirjoittaminen | avgrq sz |
| **Jonon pituus** | Lue lomakkeen tai tallennustilan levylle kirjoittamista odottaa pyyntöjen avoin i/o määrä. | Nykyinen levyn jono | avgqu sz |
| **Maks. Muisti** | Suoritettava sovellus sujuvasti muistin | % Vahvistetun tavua käytössä | Käytä vmstat |
| **Maks. SUORITIN** | Suoritettava sovellus sujuvasti suorittimen summa | % Suoritinaika | % util |

Lisätietoja [iostat](http://linuxcommand.org/man_pages/iostat1.html) ja [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Sovelluksen optimoinnista  
Keskeiset tekijät, jotka vaikuttavat suorituskyvyn sovelluksen Premium säilössä ovat laatu, IO pyynnöt AM koko, levyn koko levyjen levyn välimuistiin, Multithreading ja jonon pituus määrä. Voit hallita joitakin seikoista nupit myöntämä järjestelmän kanssa. Useimpien sovellusten saattaa ei avulla voit muuttaa IO kokoa ja jonon pituus suoraan vaihtoehto. Esimerkiksi jos käytössäsi on SQL Server, et voi valita IO koko ja jonon syvyys. SQL Server valitsee optimaalisen IO koko ja jonon syvyys arvot saat useimmat suorituskyvyn. On tärkeää ymmärtää tekijät molempien tietotyyppien vaikutukset sovelluksen suorituskyvyn, niin, että voit valmistella suorituskyvyn tarpeiden tarvittavat resurssit.

Tässä osassa koko viitata sovelluksen vaatimukset tarkistusluettelon, jonka loit tunnistamiseen, kuinka paljon haluat sovelluksen suorituskyvyn parantamiseksi. Perusteella, osaat määrittämään tekijät tästä osasta tarvitset paranna. Todistajaa kunkin kerroin vaikutukset sovelluksen suorituskyvyn suorittamalla esikuva Työkalut-sovelluksen asetukset. Lisätietoja [Benchmarking](#Benchmarking) -osassa on yleisiä esikuva työkaluja Suorita Windows ja Linux VMs ohjeita noudattamalla tämän artikkelin lopussa.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimointi IOPS, liikenteen ja viive pikakatsaus  
Seuraavassa taulukossa on yhteenveto kaikki suorituskykytekijät ja vaihe vaiheelta, miten optimointi IOPS, liikenteen ja viive. Seuraavan yhteenveto osissa kuvataan kutakin tekijä on paljon syvyys.

| | **IOPS** | **Siirtonopeuden** | **Viive** |
|---|---|---|---|
| **Esimerkkitapaus** | Yrityksen OLTP sovelluksen edellyttävän erittäin suuri tapahtumien toisen korko.                                                                                                                                 | Yrityksen tiedot warehousing sovelluksen käsittely suuria määriä tietoa. | Lähellä reaaliaikaisten sovellusten edellyttävän pyynnöt, kuten online-pelaaminen pikaviestien vastaukset. |
| Suorituskykytekijät  | | | |
| **IO-koko** | IO pienemmäksi tuottaa suurempi IOPS.                                                                                                                                                                           | IO suurentaminen voi tuottaa suurempi siirtonopeuden. | |
| **AM kokoa** | Käytä AM koko, joka on suurempi kuin sovelluksen vaatimus IOPS. Katso AM koot ja niiden IOPS rajoitukset tähän. | Käyttää siirtonopeuden raja on suurempi kuin sovelluksen vaatimus AM kokoa. Katso AM koot ja niiden siirtonopeuden rajoitukset tähän. | Käyttää tarjouksia skaalata rajoitukset on suurempi kuin sovelluksen vaatimus AM kokoa. Katso AM kokojen ja niiden rajoitukset. |
| **Levyn koko** | Käytä levyn koko, joka on suurempi kuin sovelluksen vaatimus IOPS. Katso levyn koot ja niiden IOPS rajoitukset tähän. | Käyttää levyn koko siirtonopeuden raja on suurempi kuin sovelluksen vaatimus. Katso levyn koot ja niiden siirtonopeuden rajoitukset tähän. | Käyttää levyn kokoa tarjouksia skaalata suurempi kuin sovelluksen vaatimus rajoitukset. Katso levyn kokojen ja niiden rajoitukset. |
| **AM ja levyn asteikko rajoitukset** | Valittu AM koko IOPS enimmäismäärä on oltava suurempi kuin yhteensä IOPS premium tallennustilan levyjen liitetty avulla. | Valittu AM koko siirtonopeuden enimmäismäärä on oltava suurempi kuin yhteensä siirtonopeuden premium tallennustilan levyjen liitetty avulla. | Valittu AM koko asteikon rajat on oltava suurempi kuin liitetty premium tallennustilan levyjen yhteensä asteikko rajoissa. |
| **Levyn välimuistiin tallentaminen** | Käyttöön vain luku-välimuistin premium tallennustilan levyille luku suurempi luku IOPS saat paljon toimintojen kanssa. | | Käyttöön vain luku-välimuistin premium tallennustilan levyille valmis paksu työvaiheita saat erittäin pienen luku viiveet suurempia. |
| **Levyn raitasarjoittamista** | Käytä useita levyjä ja stripe ne yhteen, kun haluat saada yhdistetyn suurempi IOPS ja siirtonopeuden rajoitukset. Huomaa, että AM yhdistetyn yläraja ylittävät yhdistetyn liitetty premium levyjen. | |
| **Raita kokoa** | Raita pienemmäksi satunnaisia pieni IO viivakuvion nähdä OLTP-sovelluksissa. Käytä esimerkiksi 64 kilotavun kokoinen raita SQL Server OLTP-sovelluksen. | Raita suurentaminen peräkkäisiä suuri IO viivakuvion nähdä tietovarasto-sovelluksissa. Käytä esimerkiksi 256 kt raita kokoa SQL Server Data warehouse sovelluksen. | |
| **Monisäikeisyys** | Käytä monisäikeisyys push-pyyntöjen suurempi määrä, joka johtaa suurempiin IOPS ja siirtonopeuden Premium-tallennustilan. SQL Server määrittää suuri MAXDOP arvo varata lisää suorittimessa SQL Server-tietokantaan. | |
| **Jonon pituus** | Suurempi jonon pituus tuottaa suurempi IOPS. | Suurempi jonon pituus tuottaa suurempi siirtonopeuden. | Pienempi jonon pituus tuottaa alemman viiveet suurempia. |

## <a name="nature-of-io-requests"></a>IO pyynnöt laatu  
IO-pyyntö on i/toiminto, joka suorittaa sovelluksen yksikköä. Tunnistaminen laatu IO-pyyntöjen satunnaisia tai peräkkäisiä, luku- tai kirjoitusoikeudet, pieni tai suuri, avulla voit määrittää sovelluksen vaatimukset. On tärkeää ymmärtää IO pyynnöt oikean päätösten, suunnitellessasi sovellusta infrastruktuurin laatu.

IO-koko on tärkeämpää tekijät. IO-koko on haluamasi kokoinen sovelluksen luomat i/toiminto pyynnön. IO-koko on merkittäviä vaikutus suorituskykyyn IOPS ja sovellus on saavuttaa kaistan käyttöön. Seuraava kaava näyttää IOPS, välisen yhteyden IO koko ja kaistanleveys ja siirtonopeuden.  
    ![](media/storage-premium-storage-performance/image1.png)

Jotkin sovellukset avulla voit muuttaa IO niiden kokoa, kun sovelluksia ei ole. Esimerkiksi SQL Server määrittää optimaalisen IO koon itse ja anna käyttäjien muuttaa minkä tahansa nupit. Toisaalta, Oracle sisältää parametrin nimeltä [DB\_estää\_koon](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) avulla, jossa voit määrittää tietokannan i/o-pyyntö koon.

Jos käytössäsi on jokin sovellus, joka ei salli IO koon muuttaminen, suorituskyvyn KPI, joka on tärkeimpiä sovelluksen tämän artikkelin ohjeiden avulla. Esimerkiksi

-   OLTP-sovellus luo miljoonia pienten ja satunnaisia IO pyynnöt. Käsittelemään tällaisen IO pyynnöt täytyy suunnitella sovelluksen infrastruktuurin saat suurempi IOPS.  
-   Tietovaraston sovelluksen Luo suuria ja peräkkäisiä IO pyynnöt. Käsittelemään tällaisen IO pyynnöt täytyy suunnitella sovelluksen infrastruktuurin suurempi kaistanleveys ja siirtonopeuden.

Jos käytössäsi on jokin sovellus, jonka avulla voit muuttaa IO kokoa käyttää peukalosääntöä IO-koko lisäksi suorituskyvyn muita ohjeita

-   Saat suurempi IOPS pienempi IO koko. Esimerkki 8 KB OLTP-sovellukselle.  
-   Saat suurempi kaistanleveys ja siirtonopeuden suurempi IO koko. Esimerkiksi 1 024 Kilotavua varaston sovellukselle.

Seuraavassa on esimerkki siitä, miten voit laskea IOPS ja siirtonopeuden/kaistanleveyden sovelluksen. Harkitse P30 DVD-levyllä-sovellus. Enimmäismäärä IOPS ja siirtonopeuden/kaistanleveyden P30 DVD-levyllä toteuttaa on 5000 IOPS ja 200 Mt sekunnissa tarpeen mukaan. Nyt, jos sovellus edellyttää suurin IOPS P30 levyltä ja käytät IO-pienemmäksi, kuten 8 Knowledge Base-osaat saat tuloksen kaistanleveys on 40 Megatavua sekunnissa. Jos sovellus edellyttää suurin nopeus/kaistanleveyden P30 levyltä ja käytät IO suurentaminen kuten 1 024 Kilotavua, tuloksena olevat IOPS on vähemmän, 200 IOPS. Paranna IO koon vuoksi siten, että se täyttää sekä sovelluksen IOPS ja siirtonopeuden/kaistanleveyden vaatimus. Taulukossa on yhteenveto IO erikokoisia ja niiden vastaavan IOPS ja siirtonopeuden P30 levylle.

| **Sovelluksen vaatimus** | **I/o kokoa** | **IOPS** | **Siirtonopeuden/kaistanleveyden** |
|-----------------------------|--------------|----------|--------------------------|
| Maks-IOPS                    | 8 KT         | 5 000    | 40 Mt sekunnissa         |
| Suurin nopeus              | 1 024 KT      | 200      | 200 Mt sekunnissa        |
| Suurin nopeus + hyvin IOPS  | 64 KILOTAVUA        | 3,200    | 200 Mt sekunnissa        |
| Maks-IOPS + Suuri nopeus  | 32 KT        | 5 000    | 160 Mt sekunnissa        |

Jos haluat IOPS ja kaistanleveys on suurempi kuin yhden premium tallennustilan DVD-levyllä suurin arvo, käytä useita premium-levyjä Raidallinen yhdessä. Esimerkiksi raita kaksi P30 levyä pääset yhdistetty IOPS on 10 000 IOPS tai 400 Mt yhdistetyn siirtonopeuden sekunnissa. Seuraavan osion kuvatulla sinun on käytettävä AM koko, joka tukee yhdistetty levyn IOPS ja siirtonopeuden.

>**Huomautus:**  
>Kun lisäät IOPS tai toinen myös kasvattaa siirtonopeuden, varmista, että ei valitset siirtonopeuden tai IOPS rajat levyn tai AM kun lisääntyvien jompikumpi.

Voit todistajaa sovelluksen suorituskykyyn IO-koko tehosteita, voit suorittaa esikuva Työkalut AM ja levyjen. Luo useita testi suoritetaan ja vaikutus jokaisen IO erikokoinen avulla. Lue lisätietoja tämän artikkelin lopussa [Benchmarking](#Benchmarking) -kohdasta.

## <a name="high-scale-vm-sizes"></a>Suuri asteikko AM koot  
Kun käynnistät sovelluksen suunnitteleminen, voit tehdä ensimmäisen asiaan on Valitse AM isännöimiseen sovelluksen. Premium tallennustilaa sisältää hyvin asteikko AM koot, voit suorittaa suurempi Laske power ja suuri paikallisen levyn i/o suorituskyvyn sovellukset. Nämä VMs on nopeampaa suorittimien ja muistin core suhde Solid-State vaikuttavat Suoritettaessa Paikallinen levy. Suuri asteikko VMs tukevat Premium tallennustilan ovat DS, DSv2 ja GS sarjan VMs.

Suuri asteikko VMs ovat käytettävissä erikokoisia, jossa on eri määrä suorittimen sydämiä, muistin, OS ja tilapäinen levyn kokoa. Jokaisen AM koon on myös tietojen levyjä, voit liittää AM enimmäismäärä. Tämän vuoksi valitsemasi AM koon vaikuttaa olevan käsittelyn muistin ja tallennustilaa on käytettävissä sovelluksen. Se vaikuttaa Laske ja tallennustilaa kustannusten. Alla on suurin AM koko, DS sarjan, DSv2 sarjan ja GS sarjan määritykset:

| AM kokoa | Suorittimen sydämiä | Muisti | AM levyn koot | Maks. tietoja levyjen | Välimuistin kokoa | IOPS | Kaistanleveyden välimuistin IO rajoitukset |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GT | OS = 1023 GT <br> Paikallinen Suoritettaessa = 224 gt | 32 | 576 GT | 50 000 IOPS <br> 512 Mt sekunnissa | 4 000 IOPS ja 33 Mt sekunnissa |
| Standard_GS5 | 32 | 448 GT | OS = 1023 GT <br> Paikallinen Suoritettaessa = 896 gt | 64 | 4224 GT | 80,000 IOPS <br> 2 000 Mt sekunnissa | 5 000 IOPS ja 50 Mt sekunnissa |

Saat kaikki käytettävissä olevat Azure AM koot kattavaan luetteloon-viitata [Windows AM koot](../virtual-machines/virtual-machines-windows-sizes.md) tai [Linux AM kokoa](../virtual-machines/virtual-machines-linux-sizes.md). Valitse AM-koko, jotka täyttävät ja Skaalaa haluamasi sovellus suorituskyvyn tarpeen. Ota huomioon seuraavat tärkeät seikat AM koot valittaessa.


*Skaalaa-rajoitukset*  
Suurin IOPS rajoituksia AM ja kohti levyn ovat eri ja toisistaan riippumatta. Varmista, että sovellus riippuvan sisennyksen IOPS AM sekä liitetty premium-levyjä rajoissa. Sovelluksen suorituskyvyn kohdata muutoin rajoitusta.

Esimerkkinä oletetaan sovelluksen vaatimus on enintään 4 000 IOPS. Tätä valmistella P30 DVD-levyllä, DS1 AM. P30 levyn voit näyttää enintään 5 000 IOPS. DS1 AM on kuitenkin rajoitettu 3,200 IOPS. Näin ollen sovelluksen suorituskyvyn rajoitettu AM rajoitus on 3,200 IOPS mukaan ja ole eivät toimi oikein suorituskykyä. Jos haluat estää tilanteen, valitse AM ja levytilan koko, joka on sekä sovelluksen vaatimukset.

*Toiminnon kustannukset*  
Monissa tapauksissa on mahdollista, että oman kokonaiskustannukset avulla Premium tallennustila on pienempi kuin käyttämällä vakio-tallennustilan.

Esimerkkinä edellyttävän 16,000 IOPS sovellus. Tämä suorituskyvyn saavuttamiseksi tarvitset standardi\_D14 Azure IaaS AM, jonka suurin IOPS 16 000 käyttämällä 32 vakio tallennustilan 1 TT levyjä, voit antaa. Kunkin 1 TT vakio tallennustilan levyn voit tehostaa enintään 500 IOPS. Tämä AM kuukaudessa arvioitu kustannus on $1,570. 32 vakio tallennustilan levylle kuukausihinta on $1,638. Arvioitu kuukausittain kokonaiskustannukset ovat $3,208.

Kuitenkin jos Premium tallennustilan saman ohjelman hallinnoida, sinun on AM pienemmäksi ja vähemmän premium tallennustilan levyjä, mikä pienentää kokonaiskustannukset. Standardi\_DS13 AM voi vastata 16,000 IOPS vaatimus neljä P30 levyjen avulla. DS13 AM on suurin IOPS 25,600, ja jokaisen P30 levyn on suurin IOPS, 5 000. Yleinen-määritysten toteuttaa 5 000 x 4 = 20 000 IOPS. Tämä AM kuukaudessa arvioitu kustannus on $1,003. Neljä P30 premium tallennustilan levyjen kuukausihinta on $544.34. Arvioitu kuukausittain kokonaiskustannukset ovat $1,544.

Taulukossa on yhteenveto tässä skenaariossa kustannukset hajautuksen Standard-ja Premium-tallennustilan.

| | **Vakio** | **Premium** |
|---|---|---|
| **Kustannusten AM kuukaudessa** | $1,570.58 (vakio\_D14)   | $1,003.66 (vakio\_DS13) |
| **Kustannusten levyjen kuukaudessa** | $1,638.40 (32 x 1 TT levyä) | $544.34 (4 x P30 levyä) |
| **Kokonaiskustannukset kuukaudessa**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

Azure Premium tallennustilan saat käytössä Windows- tai Linux VMs suorituskyvyn samalla tasolla. Sovellus tukee monia flavors Linux distros, ja näet täydellisen luettelon [tähän](../virtual-machines/virtual-machines-linux-endorsed-distros.md). On tärkeää muistaa, että eri distros soveltuvat paremmin erityyppisten toiminnoista. Näet eri tasoilla suorituskyvyn havainnollistamiseen suoritetaan distro mukaan. Testaa sovelluksen Linux-distros ja valitse haluamasi vaihtoehto, joka toimii parhaiten.


Suoritettaessa Linux Premium tallennustilan ja Tarkista uusimmat päivitykset pakolliset ohjaimet, jotta erinomainen suorituskyky tietoja.

## <a name="premium-storage-disk-sizes"></a>Premium tallennustilan levyn koko  
Azure Premium tallennustila on kolme levyn kokoa tällä hetkellä. Jokaisen levyn koon on eri asteikkoa rajoitus IOPS, kaistanleveys ja tallennustilaa. Valitse oikea reuna Premium tallennustilan levyn koko sovelluksen vaatimukset ja suuri asteikon AM koon mukaan. Alla olevassa taulukossa näkyy kolme levyä koot ja niiden ominaisuudet.

| **Levyn tyyppi**       | **P10**           | **Ñ20 =**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Levyn koko           | 128 giB           | 512 giB           | Vähintään 1 024 giB (1 TT)   |
| IOPS levyä kohden       | 500               | 2300              | 5000              |
| Siirtonopeuden levyä kohden | 100 Mt sekunnissa | 150 Megatavua sekunnissa | 200 Mt sekunnissa |

Kuinka monta levyjen levyn määrittyy kokoa valittu. Voit käyttää yhden P30 DVD-levyllä tai useita P10 levyjä sovelluksen vaatimukset täyttävän. Huomioon tilin huomioon otettavia seikkoja alla vaihtoehto.

*(IOPS ja siirtonopeuden) asteikon rajoitukset*  
IOPS ja siirtonopeuden jokaisen Premium levyn koon rajoissa on eri ja riippumaton AM asteikko rajoitukset. Varmista, että yhteensä IOPS ja siirtonopeuden-levyjä ovat valitsemasi AM kokoa skaalaus rajoissa.

Jos esimerkiksi sovelluksen vaatimus on enintään 250 Mt/sec siirtonopeuden ja käytät DS4 AM yksittäisen P30 DVD-levyllä kanssa. DS4 AM antaa enintään 256 Mt/sec siirtonopeuden. Yksittäisen P30 DVD-levyllä on kuitenkin 200 Mt/sec siirtonopeuden enimmäismäärä. Näin ollen sovellus on rajoitettu 200 Mt/sec vuoksi levyn raja-palvelussa. Voit ratkaista tämän rajan valmistella enemmän kuin yksi tietojen levyjä AM.

>**Huomautus:**  
>Levyn IOPS ja siirtonopeuden näin ollen aihe levyn rajoitukset, ei sisällytetä lukee served välimuistin mukaan. Välimuistin on erillinen IOPS ja siirtonopeuden raja AM kohden.
>
>Esimerkiksi alun perin lukee ja kirjoittaa ovat 60 Mt/s ja 40 Mt/s. Ajan myötä välimuistin warms ja lisää ja lisää lukee toimii välimuistista. Tämän jälkeen saat levyltä suurempi kirjoittaminen siirtonopeuden.

*Levyjen määrä*  
Määritä mukaan arvioidaan sovelluksen vaatimukset tarvitset levyjen määrä. Jokaisen AM koon on myös levyjä, voit liittää AM määrän rajoitukset. Tämä on yleensä kahdesti sydämiä määrä. Varmista, että valitset AM koon tukee tarvitaan levyjen määrä.

Muista, että Premium tallennustilan levyjen on suurempi vakio tallennustilan levyjen verrattuna suorituskykyä. Sen vuoksi, jos sähköpostit siirretään Azure IaaS AM käyttämällä vakio tallennustilan Premium tallennustilan sovelluksesi, todennäköisesti tarvitset vähemmän premium levyjen saavuttaminen yhtä sovelluksen.

## <a name="disk-caching"></a>Levyn välimuistiin tallentaminen  
Suuri asteikko-VMs, joka hyödyntää Azure Premium tallennustilan on nimeltään BlobCache monitasoisten välimuistiin tekniikka. BlobCache käyttää virtuaalikoneen RAM-Muistia ja paikallisen Suoritettaessa yhdistelmän välimuistiin. Välimuistin on käytettävissä Premium tallennustilan pysyvä levyille ja AM paikallisen levyjä. Oletusarvon mukaan välimuisti-asetus on määritetty luku-/ kirjoitusoikeudet OS levyjä ja vain luku-tilassa tietojen levyjen isännöimät Premium-tallennustilan. Kanssa levyn välimuistiin Premium tallennustilan levyille käytössä, suuri asteikon VMs voit tehostaa erittäin suuri suorituskyvyn tasoja, jotka ylittävät pohjana levyn suorituskykyä.

>[AZURE.WARNING] Välimuistin-asetuksen muuttaminen Azure levyn irrottaa ja liittää uudelleen levy. Käyttöjärjestelmän levy on AM käynnistetään uudelleen. Lopeta kaikki sovellukset ja palvelut, jotka tämä häiriöt ennen levyn välimuisti-asetuksen muuttaminen vaikuttaa.

Saat lisätietoja BlobCache toiminta viitata sisällä [Azure Premium tallennustilan](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogimerkintä.

On tärkeää levyjen oikean joukko välimuisti käyttöön. Onko kannattaa ottaa käyttöön levyn välimuistiin premium-levyllä tai ei riippuu työmäärää kuvion levyn voidaan käsitteleminen. Alla olevassa taulukossa näkyy oletusarvo OS ja tietojen levyjen välimuistiasetukset.

| **Levyn tyyppi** | **Välimuistin oletusasetus** |
|---|---|
| OS levy | ReadWrite |
| Tietoja levyn | Ei mitään |

Seuraavassa on tietoja levyjen suositellaan välimuistiasetukset

| **Levyn välimuistiin asetus** | **Suositus siitä, milloin kannattaa käyttää tätä asetusta** |
|---|---|
| Ei mitään | Määritä Host (isäntä)-välimuistin vain kirjoitus- ja kirjoitusoikeudet näkyvä levyjen ei mitään. |
| Vain luku-tilassa | Määrittää host välimuistin vain luku-tilassa vain luku- ja luku-ja kirjoitusoikeudet levyille. |
| ReadWrite | Määritä Host (isäntä)-välimuistin ReadWrite vain, jos sovellus käsittelee oikein kirjoittaminen välimuistiin tallennetut tiedot pysyvät levyjen tarvittaessa. |

*Vain luku-tilassa*  
Määrittämällä vain luku-tilassa välimuistiin Premium tallennustilan tietojen levyjen saavuttamiseksi pienen luku viive ja hanki erittäin suuri luku IOPS ja siirtonopeuden sovelluksen. Tämä on kahdesta syystä määräpäivä

1.  Lukee suorittaa välimuistista, joka on AM muistia ja paikallisen Suoritettaessa, on paljon aiempaa nopeammin kuin lukee tiedot levyltä, joka on Azure-blob-säiliö.  
2.  Premium tallennustilan Laske served välimuisti-levyn IOPS kohti ja siirtonopeuden lukuja. Sovellus on voit tehostaa suurempi yhteensä IOPS ja siirtonopeuden.

*ReadWrite*  
OS levyjen on oletusarvoisesti ReadWrite välimuisti käytössä. Olemme lisänneet viimeksi ReadWrite välimuistin tietoja sekä levyjen tuki. Jos käytössäsi on ReadWrite tallentamisesta välimuistiin, on oltava asianmukaisesti voi kirjoittaa tiedot pysyvät levyjen välimuistista. Esimerkiksi SQL Server käsittelee kirjoittaminen välimuistiin tallennetut tiedot pysyvät tallennustilan levyjen omalla. ReadWrite välimuistin käyttäminen sovellus, joka ei käsittele toteaa tarvittavat tiedot voi aiheuttaa tietojen menetyksiltä, jos AM kaatuu.

Esimerkkinä, voit käyttää seuraavia ohjeita SQL Serveriä Premium säilössä toimimalla seuraavasti

1.  Määritä "vain luku-välimuistin premium tallennustilan levyille isännöinnin datatiedostot.  
    a.  Nopea lukee välimuistista alemman SQL Serverin ajan jälkeen tietosivujen noudetaan paljon nopeammin välimuistista verrattuna suoraan tiedot levyjä.  
    b.  Käsittelevä lukee välimuistista, tarkoittaa muita siirtonopeuden on käytettävissä premium tietojen levyjä. SQL Server voit käyttää tätä muita siirtonopeuden kohti haetaan tietojen sivuja ja muita toimintoja, kuten varmuuskopiointi ja palauttaminen ja lataa erän ja indeksi muodostaa uudelleen.  
2.  Määritä "Ei" välimuistiin premium tallennustilan levyille isännöinnin lokitiedostot.  
    a.  Lokitiedostojen on ensisijaisesti kirjoittaminen näkyvä toimintoja. Tämän vuoksi ei voi hyötyä vain luku-välimuistista.

## <a name="disk-striping"></a>Levyn raitasarjoittamista  
Kun AM on liitetty useita premium tallennustilan pysyvä levyjen kanssa hyvin skaalaus-levyjä voidaan Raidallinen yhdessä kootaan IOPs, kaistanleveys ja tallennustilaa.

Windows voit käyttää tallennustilan välilyöntejä raita-levyjen yhdessä. Sinun on määritettävä yksi sarake kunkin levyn ryhmään. Muussa tapauksessa mustat aseman suorituskykyyn voi olla pienempi kuin odotettu liikenteen levyille erimittainen jakautuminen vuoksi.

Tärkeää: Käytä palvelimen hallinnan Käyttöliittymän, voit määrittää enintään 8 mustat aseman sarakkeiden määrä. Kun liität yli 8 levyjä, luoda aseman PowerShellin avulla. PowerShellin avulla voit määrittää sarakkeiden määrä yhtä levyjen määrä. Jos määritettynä on 16 levyjen yksittäisen raita; joukon esimerkiksi Määritä *Uusi VirtualDisk* PowerShell-cmdlet-komennon *NumberOfColumns* -parametri 16 saraketta.


Linux-apuohjelmalla MDADM raita levyille yhdessä. Yksityiskohtaisia ohjeita raitasarjoittamista levyille Linux viitata [Määrittäminen ohjelmiston RAID Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Raita kokoa*  
Tärkeää määrittäminen levyn raitasarjoittamista on raita suuruus. Raitakoon tai lohkokoko on tietoja, jotka sovelluksen kohteeksi mustat asemassa pienin lohkon. Voit määrittää raitakoon määräytyy sovelluksen ja sen pyynnön kuvion tyyppi. Jos valitset väärän raita kokoa, se saattaa johtaa IO kohdistusvirhe, mikä heikentää sovelluksen suorituskykyä johtaa.

Jos sovelluksen luoma IO-pyyntö on suurempi kuin levyn raita kokoa, tallennustilan-järjestelmä kirjoittaa esimerkiksi se raita yksikön rajojen useamman kuin yhden levyn. Kun on aika käyttää tietoja, se on useampi kuin yksi raita yksiköt pyynnön suorittamiseen yli haku. Tämä käyttäytyminen kumulatiivinen vaikutus voi aiheuttaa merkittäviä kansiohierarkiassa. Toisaalta IO-pyynnön koko on pienempi kuin raitakoon ja jos se on satunnaisia, IO pyynnöt voivat lisätä saman levyn aiheuttaa pullonkaulan ja heikennä kädessä IO suorituskykyä.


Sovellus on käynnissä työmäärää tyypin mukaan haluamasi raitakoon valitseminen. Käytä satunnaisia pieniä IO pyyntöjä raita pienemmäksi. Tämän vuoksi suuri peräkkäisiä IO pyynnöt käyttäminen raita suurentaminen. Selvitä raita koon suositukset-sovelluksen käytössä Premium-tallennustilan. SQL Server määrittää raita kokoa 64 Kilotavua, ja OLTP toiminnoista ja 256 Kilotavua, ja tietojen laajemmissa toiminnoista. Katso lisätietoja [suorituskyvyn SQL Server Azure VMs parhaat käytännöt](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) .


>**Huomautus:**  
>Voit voit stripe yhdessä enintään 32 premium tallennustilan levyille DS sarjan AM ja 64 premium tallennustilan levyille GS sarjan AM.

## <a name="multi-threading"></a>Monisäikeisyys  
Azure on suunniteltu Premium tallennustilan-ympäristössä, on erittäin rinnakkain. Tämän vuoksi Salli muut samanaikaiset sovelluksen kertyy huomattavasti paremman suorituskyvyn kuin yhden threaded sovellus. Salli muut samanaikaiset sovelluksen useita viestiketjuissa siirtyminen ylöspäin tehtäviään jakautuu ja kasvattaa tehokkuuden sen suorittamisen välityksellä suurin AM ja levytilan resurssit.

Esimerkiksi jos sovellus on käynnissä yksittäisen core käyttämällä kahta viestiketjuissa siirtyminen AM-Suoritin välillä voi siirtyä kaksi viestiketjuissa siirtyminen tehokkuuden saavuttamiseksi. Kun yksi viestiketjun odottaa levyn IO suorittamiseen, Suoritin voi siirtyä muiden viestiketjun. Tällä tavalla kaksi viestiketjuissa siirtyminen tehdä useamman kuin yhden viestiketjun. Jos AM on useampi kuin yksi core, se edelleen vähenee käynnissä ajan jälkeen kunkin core voit suorittaa tehtäviä rinnakkain.

Saattaa ei voi muuttaa tapaa, jolla joltain sovelluksen toteuttaa yksittäisen säikeistysmallin tai monisäikeisyys. Esimerkiksi SQL Server pystyy usean suorittimen ja usean core käsitteleminen. Kuitenkin SQL Server päättää, mitä olosuhteissa se hyödyntää vähintään yksi viestiketjuissa siirtyminen kyselyn suorittamiseen. Voit suorittaa kyselyjä ja muodostaa indeksejä käyttämällä monisäikeisyys. Kyselyn, joka liittyy suurten taulukoiden liittämisestä ja tietojen lajittelemisesta ennen käyttäjälle, joka palauttaa SQL Server todennäköisesti käyttää useita viestiketjuissa siirtyminen. Käyttäjä voi kuitenkin määrittää onko SQL Server suorittaa yhteen viestiketjun tai useita viestiketjuissa siirtyminen kyselyn.

On asetukset, jotka voivat muuttaa tämä vaikuttaa monisäikeisyys tai rinnakkain sovelluksen käsittely. Esimerkiksi jos SQL Server on suurin rinnakkaisuus asteen määritykset. Tämä asetus on nimeltään MAXDOP, voit määrittää SQL Server käyttää, kun rinnakkain suorittimien enimmäismäärä käsitellään. Voit määrittää MAXDOP yksittäisten kyselyiden tai indeksi-toimintoja. Tämä on hyötyä, kun haluat saldo suorituskyvyn kriittinen sovelluksen järjestelmän resursseja.

Oletetaan, että sovelluksesi SQL Serveriä suoritetaan suurelle kyselyn ja indeksi toiminnon samanaikaisesti. Uutiskirjeistä oletetaan, että haluat määrittää hakemiston-toiminto on yksi performant verrattuna suurelle kyselyn. Tässä tapauksessa voit määrittää MAXDOP arvo on suurempi kuin MAXDOP kyselyn indeksi-toimintoa. Tällä tavalla SQL Server on useita, se voidaan hyödyntää indeksi-toimintoa, se voi tälle suurelle kyselyn suorittimien määrän verrattuna suorittimien määrän. Muista, että ohjaa viestiketjuissa siirtyminen SQL Server käyttää jokaiselle määrä. Voit määrittää enimmäismäärä on varattu suorittimien monisäikeisyys.

Lisätietoja SQL Serverin [Rinnakkaisuus astetta](https://technet.microsoft.com/library/ms188611.aspx) . Tällaisia asetuksia, jotka vaikuttavat Selvitä monisäikeisyys sovelluksesi ja niiden käyttömahdollisuudet suorituskyvyn parantamiseksi.

## <a name="queue-depth"></a>Jonon pituus  
Jonon pituus tai jonon pituuden tai jonon koko on järjestelmän IO pyyntöihin määrä. Jonon pituus arvo määrittää, kuinka monta sovelluksen tasata, jossa tallennustila-levyjä käsittely IO-toimintoja. Se vaikuttaa kaikkiin kolme sovellusta suorituskyvyn mittarit, joka on kerrottu tämän artikkelin kuten esimerkiksi, IOPS, liikenteen ja viive.

Jonon syvyyttä ja monisäikeisyys ovat läheisesti liittyvät. Jonon pituus-arvo osoittaa, kuinka paljon monisäikeisyys can saavuttaa sovellus. Jos jonon pituus on suuri, sovelluksen voit suorittaa toimintoja samanaikaisesti, toisin sanoen Lisää monisäikeisyys. Jos jonon pituus on pieni, vaikka sovellus ei salli muut samanaikaiset, tarpeeksi pyynnöt samanaikainen suorittamista varten alaiset ei ole.

Yleensä käytöstä shelf sovellusten ei salli jonon syvyyttä, koska jos määritetty virheellisesti se tehdään enemmän kuin hyvä haittaa. Sovellusten määrittää jonon pituus saat parhaan mahdollisen suorituskyvyn oikean arvon. Kuitenkin on tärkeää ymmärtää tämän käsitteen, jotta voit tehdä suorituskykyongelmia sovelluksen. Voit huomata jonon pituus tehosteita myös suorittamalla esikuva Työkalut-järjestelmään.

Jotkin sovellukset on vaikuttaa jonon pituus-asetukset. Esimerkiksi SQL Serverin MAXDOP (enintään asteen rinnakkaisuuden)-asetus selitetty edellisessä osassa. MAXDOP on tapa vaikuttaa jonon pituus ja monisäikeisyys, vaikka se ei suoraan Muuta SQL Serverin jonon pituus-arvo.

*Suuri jonon pituus*  
Suuri jonon pituus rivit toimintoja levyn ylöspäin. Levyn näkee sen jonossa etuajassa seuraavan pyynnön. Näin ollen levyn voit ajoittaa etuajassa toiminnot ja käsitellä niitä optimaalisen järjestyksessä. Koska sovelluksen lähettää enemmän pyyntöjä levylle, levyn voit käsitellä useita rinnakkaisia IOs. Kädessä sovelluksen voivat saada suurempi IOPS. Koska sovelluksen käsittelee enemmän pyyntöjä, yhteensä siirtonopeuden sovelluksen kasvaa.

Yleensä sovelluksen toteuttaa suurin nopeus kanssa 8-16 + avoin IOs liitetty levyä kohden. Jos jonon pituus on yksi, sovellus ei on valitseminen tarpeeksi IOs-järjestelmään ja se käsittelee vähemmän määrän annetulle. Toisin sanoen vähemmän siirtonopeuden.

Esimerkiksi SQL Server-arvo kyselyn "4" MAXDOP ilmoittaa SQL Server, että se käyttää enimmillään neljä sydämiä kyselyn suorittaminen. SQL Server määrittää parhaat jonon syvyys arvo ja määrä sydämiä kyselyn suorittamista varten.

*Paras mahdollinen jonon pituus*  
Erittäin suuri jonon syvyys arvo on myös sen haitoista. Jos jonon syvyys arvo on liian suuri, sovellus yrittää vaikuttavat erittäin suuri IOPS. Jos sovellus on jatkuva levyjä riittävästi valmisteltu IOPS kanssa, tämä heikentää sovelluksen viiveet suurempia. Seuraavan kaavan näkyy IOPS, viiveen ja jonon pituus välinen suhde.  
    ![](media/storage-premium-storage-performance/image6.png)

Määritä jonon pituus minkä tahansa suuren arvon, mutta optimaalisen arvon, joka voi toimittaa tarpeeksi IOPS sovelluksen vaikuttamatta viiveet suurempia. Esimerkiksi jos sovelluksen viive on oltava 1 millisekunnin, jonon pituus, 5 000 IOPS saavuttamiseksi pakollinen on QD = 5000 x muutos iteraatiokertojen 0,001 = 5.

*Mustat aseman jonon pituus*  
Mustat aseman ylläpitää riittävän jonon pituus siten, että jokaisen levy on piikin jonon syvyys yksitellen. Voit esimerkiksi sovellus, joka vie jonon pituus, 2 ja on 4 levyjä raita. Kaksi IO pyynnöt siirtyvät kaksi levyä ja jäljellä olevat kaksi levyä päivitetään käyttämättömänä. Määritä jonon pituus vuoksi siten, että kaikki levyjä voi olla tietoja. Kaavan alla näkyy tietoja sen selvittämisestä, mustat tietomääristä jonon syvyys.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Rajoittaminen  
Azure Premium tallennustilan säännösten määritetty IOPS ja siirtonopeuden määrä AM koot ja valitset levyn koon mukaan. Milloin tahansa sovellus yrittää vaikuttavat IOPS tai siirtonopeuden nämä raja-arvot, mitä AM tai levy Voit käsitellä yläpuolella, Premium tallennustilan rajoita sitä. Tämä luettelot eivät toimi oikein suorituskyvyn sovelluksen lomakkeessa. Tämä voi tarkoittaa suurempi viive, pienennä siirtonopeuden tai alemmalle tasolle IOPS. Jos Premium tallennustilan eivät rajoita, sovelluksen kokonaan saattaa epäonnistua mukaan suurempi kuin mitä sen resurssit ovat voidaan saavuttaa. Näin on voit välttää suorituskykyongelmat rajoitusten vuoksi, aina varaa riittävät resurssit, sovelluksen. Ota huomioon, mitä kerrottu AM koot ja levyn koot osien yläpuolella. Simuloinnissa on paras tapa selvittää, mitä tarvitset isännöimiseen sovelluksesi resursseja.

## <a name="benchmarking"></a>Esikuva  
Prosessi, jossa simuloimalla eri toiminnoista, valitse sovelluksen ja sovelluksen suorituskyvyn kunkin kuormituksen mittaaminen esikuva on. Aiemmassa osassa kuvatut vaiheet olet kerännyt sovelluksen vaatimukset. Suorittamalla esikuva Työkalut isännöinnin sovelluksen VMs voit määrittää sovelluksen toteuttaa kanssa Premium tallennustilan suorituskyvyn haluamasi tasot. Tässä osassa on antaa sinulle esimerkkejä esikuva Standard DS14-AM valmisteltu Azure Premium tallennustilan levyjen kanssa.

On on käytetty yleisiä esikuva työkaluja Iometer ja FIO, Windowsin ja Linux tarpeen mukaan. Näiden työkalujen spawn useita viestiketjuissa siirtyminen simuloimalla tuotannon, kuten työmäärän ja järjestelmän suorituskykyä. Työkaluilla voit myös määrittää parametrien, kuten estäminen koko ja jonon syvyys, joka tavallisesti ei voi muuttaa sovelluksen. Näin voit joustavammin levyasemassa parhaan mahdollisen suorituskyvyn hyvin asteikolla AM kanssa premium levyjen erityyppisiä sovelluksen työmääriä valmistelun yhteydessä. Saat lisätietoja kunkin benchmarking työkalun seuraavasta [Iometer](http://www.iometer.org/) ja [FIO](http://freecode.com/projects/fio).

Voit seurata seuraavissa esimerkeissä Luo vakio DS14 AM ja liitä 11 Premium tallennustilan levyjä AM. 11 levyjen 10 levyjen määrittäminen välimuistiin tallentaminen nimellä "Ei" isännällä ja stripe niitä kutsutaan NoCacheWrites aseman kyselyjä. Määritä Host (isäntä) välimuistiin tallentaminen nimellä "vain luku-asemassa ja nimeltä CacheReads levyä aseman luominen. Tämän asetuksen avulla osaat näet suurin luku- ja kirjoitusoikeudet suorituskyky-standardin DS14 AM. Yksityiskohtaisia ohjeita luomisesta DS14 AM premium levyjen kanssa Siirry [luominen ja käyttäminen Premium-tallennustilan tilin virtuaalikoneen tietojen levylle](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

*Välimuistin lämpenee*  
Levyn välimuistiin tallentaminen vain luku-isännällä voi antaa suurempi kuin levyn raja IOPS. Saat tämän suurin luku suorituskyvyn host välimuistista ensin sinun täytyy kiinnostunut levyä välimuisti. Näin varmistat, että mikä benchmarking työkalu vaikuttavat CacheReads asemassa luku IOs todella käynnit välimuistin ja ei levyä suoraan. Lisää IOPS yhden välimuistista välimuistin osumia tuloksen käytössä levy.

>**Tärkeää:**  
>On kiinnostunut välimuistin ennen kuin suoritat esikuva-aina, kun AM käynnistetään.

#### <a name="iometer"></a>Iometer   
[Lataa Iometer-työkalu](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) AM.

*Testitiedosto*  
Iometer käyttää testitiedosto, joka on tallennettu, benchmarking testin suorittaminen äänenvoimakkuutta. Se ohjaa lukee ja kirjoittaa tämän testin tiedoston mitata levyn IOPS ja siirtonopeuden. Iometer Luo tämän testitiedosto, jos et ole antanut jokin. 200 gt testitiedosto kutsua iobw.tst CacheReads ja NoCacheWrites asemat luominen

*Access-määritykset*  
Määritykset, pyydä IO koon % luku-/ kirjoitusoikeudet, % satunnaisia tai peräkkäisiä määritetään käyttämällä Iometer "Access-määritykset"-välilehteä. Luo access-määrityksen kullekin tilanteita, joissa seuraavalla tavalla. Luo access-määritykset ja "Tallenna" kanssa sopivaa nimeä kuten – RandomWrites\_8K, RandomReads\_8K. Valitse vastaavan määrityksen, kun käynnissä Testiskenaario.

Esimerkki access-määritykset suurin kirjoittaa IOPS skenaarion on alla,  
    ![](media/storage-premium-storage-performance/image8.png)

*Suurin IOPS Testaa määritykset*  
Suurin IOPs osoittamaan käyttää pyynnön pienemmäksi. Käytä 8K pyynnön kokoa ja luo satunnaisen kirjoittaa ja lukee määrityksiä.

| Access-määritys | Pyydä kokoa | Satunnainen % | Lue % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8K     | 8K           | 100      | 0      |
| RandomReads\_8K      | 8K           | 100      | 100    |

*Suurin nopeus Testaa määritykset*  
Suurin nopeus osoittamaan käyttää pyynnön suurempia. Käytä 64K pyynnön kokoa ja luo satunnaisen kirjoittaa ja lukee määrityksiä.

| Access-määritys | Pyydä kokoa | Satunnainen % | Lue % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64K    | 64K          | 100      | 0      |
| RandomReads\_64K     | 64K          | 100      | 100    |

*Iometer-testin suorittaminen*  
Suorittaa seuraavia ohjeita kiinnostunut välimuisti

1.  Luo kaksi access-määritykset alla, arvoilla

  	| Nimi              | Pyydä kokoa | Satunnainen % | Lue % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1 Megatavu | 1 MEGATAVU          | 100      | 0      |
  	| RandomReads\_1 Megatavu  | 1 MEGATAVU          | 100      | 100    |

2.  Valmistellaan seuraavat parametrit sisältävän levyn välimuistin Iometer testin suorittaminen Käytä kolme työntekijä viestiketjuissa siirtyminen kohdeasema ja jonon syvyys 128. Testi "Suoritusaika" kestoa arvoksi 2hrs "Testaa asetukset-välilehden.

  	| Skenaario              | Target Volume | Nimi              | Kesto |
  	|-----------------------|---------------|-------------------|----------|
  	| Alusta välimuistin levy | CacheReads    | RandomWrites\_1 Megatavu | 2hrs     |

3.  Suorita seuraavat parametrit sisältävän levyn välimuistin lämmetä Iometer-testi. Käytä kolme työsäikeitä kohdeasema ja jonon syvyys 128. Testi "Suoritusaika" kestoa arvoksi 2hrs "Testaa asetukset-välilehden.

  	| Skenaario           | Target Volume | Nimi             | Kesto |
  	|--------------------|---------------|------------------|----------|
  	| Lämpimän välimuistin levyn määrittäminen | CacheReads    | RandomReads\_1 Megatavu | 2hrs     |

Kun välimuistin levy on lämmitettävä, jatka alla testi käyttötavoista. Voit suorittaa Iometer testi käyttämällä **jokaisen** kohteen aseman vähintään kolme työntekijä viestiketjuissa siirtyminen. Kunkin työntekijän säikeen Valitse kohdeasema, Määritä jonon pituus ja valitse jokin tallennetun testi-määritykset suorittaa vastaavat Testiskenaario seuraavassa taulukossa kuvatulla. Taulukossa näkyvät odotetut tulokset myös IOPS ja siirtonopeuden testit suoritettaessa. Kaikissa tilanteissa 8 KB pieni IO-koko ja suuri jonon syvyys 128 käytetään.

| Testiskenaario      | Target Volume | Nimi              | Tulos       |
|--------------------|---------------|-------------------|--------------|
| Maks. Lue IOPS     | CacheReads    | RandomWrites\_8K  | 50 000 IOPS  |
| Maks. Kirjoita IOPS    | NoCacheWrites | RandomReads\_8K   | 64 000 IOPS  |
| Maks. Yhdistetyn IOPS | CacheReads    | RandomWrites\_8K  | 100 000 IOPS |
|                    | NoCacheWrites | RandomReads\_8K   |              |
| Maks. Mt/s   | CacheReads    | RandomWrites\_64K | 524 Mt/s   |
| Maks. Mt/s  | NoCacheWrites | RandomReads\_64K  | 524 Mt/s   |
| Yhdistetyn Mt/s    | CacheReads    | RandomWrites\_64K | 1 000 Mt/s  |
|                    | NoCacheWrites | RandomReads\_64K  |              |

Seuraavassa on yhdistetty IOPS ja siirtonopeuden skenaariot näyttökuvat siitä, Iometer testitulokset.

*Yhdistetyn lukee ja kirjoittaa enintään IOPS*  
![](media/storage-premium-storage-performance/image9.png)

*Yhdistetyn lukee ja kirjoittaa suurin nopeus*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO on ensisijainen tallennustilan Linux VMs Valitse Suositut työkalu. Se on joustavuutta, voit valita IO erikokoisia, peräkkäisiä tai satunnaisia lukuja ja kirjoittaa. Se etäasemassa työsäikeitä tai prosessit voivat suorittaa määritetyn i/o-toimintoja. Voit määrittää haluamasi kunkin työntekijän viestiketjun on suoritettava työn tiedostojen i/o-toimintoa. Luomaamme skenaarion esitelty alla olevissa esimerkeissä työn tiedostoon. Voit muuttaa näitä työn tiedostoja ensisijainen eri toiminnoista, jotka on käytössä Premium tallennustilan määritysten mukaisesti. Olevassa esimerkissä on käytössä vakio DS 14 AM käynnissä **Ubuntu**. Käytä samoja asetuksia kuvattu [Benchmarking osan](#Benchmarking) alkuun ja lämpimän välimuistin määrittäminen ennen benchmarking testien suorittaminen.

Ennen kuin voit aloittaa [FIO Lataa](https://github.com/axboe/fio) ja asenna se virtuaalikoneen.

Suorita seuraava komento Ubuntu,

        apt-get install fio

Käytämme työntekijä olevien varten ajo kirjoitus-toimintojen ja neljä työntekijä viestiketjuissa siirtyminen ajo levyille luku toimintoja varten. Kirjoita työntekijöiden ajo liikenne "nocache" asemassa, jossa on 10 levyjen välimuistin "ei mitään". Luku-sovellusta ajo liikenne asemassa "readcache", "vain luku-välimuistin on määritetty 1 levy on.

*Kirjoita suurin IOPS*  
Työ-tiedoston luominen saat suurin kirjoittaa IOPS seuraavat määritykset. Nimeä "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Huomautus seuraa tärkeimmät seikat, jotka vastaavat edellisten kohtien kuvatut rakenne-ohjeet. Nämä määritykset on olennainen vaikuttavat suurin IOPS  
-   Suuri jonon syvyys 256.  
-   Pieni estäminen koko on 8 Kilotavua.  
-   Useita viestiketjuissa siirtyminen suorittamiseen satunnaisia kirjoittaa.

Suorita seuraava komento käyttö FIO testi 30 sekunnin ajan  

    sudo fio --runtime 30 fiowrite.ini

Testin suorittamisen aikana voi nähdä määrän kirjoittaminen IOPS AM ja Premium levyjen toimittaa. Alla olevassa esimerkissä esitetyllä DS14 AM toimittaa sen enintään 50 000 IOPS IOPS enimmäismäärä kirjoittaminen.  
    ![](media/storage-premium-storage-performance/image11.png)

*Suurin luku IOPS*  
Työ-tiedoston luominen saat suurin luku IOPS seuraavat määritykset. Nimeä "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Huomautus seuraa tärkeimmät seikat, jotka vastaavat edellisten kohtien kuvatut rakenne-ohjeet. Nämä määritykset on olennainen vaikuttavat suurin IOPS

-   Suuri jonon syvyys 256.  
-   Pieni estäminen koko on 8 Kilotavua.  
-   Useita viestiketjuissa siirtyminen suorittamiseen satunnaisia kirjoittaa.

Suorita seuraava komento käyttö FIO testi 30 sekunnin ajan

    sudo fio --runtime 30 fioread.ini

Testi suoritetaan, kun osaat määrän lukea IOPS AM ja Premium levyjen toimittaa. Alla olevassa esimerkissä esitetyllä DS14 AM tarjoaa yli 64 000 luku IOPS. Tämä on yhdistelmä levyn ja välimuistin suorituskykyä.  
    ![](media/storage-premium-storage-performance/image12.png)

*Suurin luku- ja kirjoitusoikeudet IOPS*  
Luo työtiedosto, jonka seuraavat määritykset saat suurin yhdistetyn lukeminen ja kirjoittaminen IOPS. Nimeä "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Huomautus seuraa tärkeimmät seikat, jotka vastaavat edellisten kohtien kuvatut rakenne-ohjeet. Nämä määritykset on olennainen vaikuttavat suurin IOPS

-   Suuri jonon syvyys 128.  
-   Pieni estäminen koko on 4 Kilotavua.  
-   Useita viestiketjuissa siirtyminen suorittamiseen satunnaisia lukee ja kirjoittaa.

Suorita seuraava komento käyttö FIO testi 30 sekunnin ajan

    sudo fio --runtime 30 fioreadwrite.ini

Testi suoritetaan, kun osaat Katso yhdistetyn luku-ja kirjoitusoikeudet IOPS AM ja Premium levyjen toimittaa. Alla olevassa esimerkissä esitetyllä DS14 AM tarjoaa yli 100 000 yhdistetyn lukeminen ja kirjoittaminen IOPS. Tämä on yhdistelmä levyn ja välimuistin suorituskykyä.  
    ![](media/storage-premium-storage-performance/image13.png)

*Suurin nopeus yhdistetty*  
Saat suurin yhdistetyn lukeminen ja kirjoittaminen siirtonopeuden, käytä suuremmaksi lohko ja suuri jonon pituus useita viestiketjuissa siirtyminen suorittamiseen lukee ja kirjoittaa. Voit käyttää 64 Kilotavua estä koosta ja jonon pituus 128.

## <a name="next-steps"></a>Seuraavat vaiheet  

Lisätietoja Azure Premium tallennustilan:

- [Premium tallennustila: Azure virtuaalikoneen työmääriä tehokas tallennustila](storage-premium-storage.md)  

SQL Server-käyttäjille, lue artikkelit suorituskykyyn liittyviä parhaita käytäntöjä, SQL Server:

- [Suorituskyvyn Azuren näennäiskoneiden SQL Server parhaat käytännöt](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure Premium tallennustila on SQL Server Azure AM suurin suorituskyky](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
