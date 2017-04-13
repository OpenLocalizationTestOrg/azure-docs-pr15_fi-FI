<properties
    pageTitle="Kirjaudu haut Log Analytics | Microsoft Azure"
    description="Lokitiedoston hakujen avulla voit yhdistää ja yhdistää tietokoneen tietoja useista lähteistä käyttöympäristössä."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="log-searches-in-log-analytics"></a>Lokitiedoston Analytics log rajaamalla

Log Analytics core on lokin hakutoimintoa, joiden avulla voit yhdistää ja yhdistää tietokoneen tietoja useista lähteistä käyttöympäristössä. Ratkaisujen myös tarjoaa log Etsi tuotava arvot poimia ympärillä on tietty ongelma-alue.

Haku-sivulla voit luoda kyselyn ja sitten kun haet, voit suodattaa tulokset käyttämällä pinta-ohjausobjektit. Voit myös luoda tarkennettuja kyselyjä, muunto, suodattimen ja raportin tulokset.

Yleisiä log hakukyselyt näkyvät useimmissa ratkaisu sivuilla. Koko OMS-konsolin Valitse ruudut tai muita kohteita, voit tarkastella kohteen tiedot log hakutoiminnolla siirtyä.

Tässä opetusohjelmassa Käymme tässä läpi esimerkkejä, jotka kattavat perustiedot, kun lokiin hakutoiminnolla.

Microsoft yksinkertainen ja käytännön esimerkkejä aloittaminen ja sitten kootaan niitä niin, että voit siirtyä hallitsemista käytännön tapauksissa syntaksi käyttämisestä haluamasi tiedot Poimi tiedot.

Sen jälkeen, kun olet tutustunut haun avulla voit tarkastella [Log Analytics Kirjaudu haun viittaus](log-analytics-search-reference.md).

## <a name="use-basic-filters"></a>Tavallinen suodattimilla

Huomaa, että tiedät on, että haun alkuosa kysely ennen jokin "|" pystysuora pystyviivan on aina *Suodatin*. Voit ajatella se TSQL--WHERE-lauseen kuin se määrittää *hakemaan ulos OMS tietovaraston tietojen alijoukon* . Tietovaraston haku on suurelta tiedot, jotka haluat purkaa, joten voi olla luonnollinen kyselyn alkaisi ja WHERE-lauseen ominaisuuksien määrittämisestä.

Voit käyttää yleisin suodattimet ovat *avainsanoja*, kuten "Virhe" tai 'aikakatkaisu' tai tietokonenimi. Yksinkertaisia seuraavanlaisiin palauttaa yleensä monipuolisen muodot saman tulosjoukon tiedot. Tämä johtuu siitä Log Analytics on eri *tyypit* järjestelmässä.


### <a name="to-conduct-a-simple-search"></a>Voit suorittaa yksinkertaisen haku
1. Valitse **Log haun**OMS-portaalissa.  
    ![Etsi-ruutu](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Kirjoita kyselyn-kenttään `error` ja valitse sitten **Etsi**.  
    ![Etsintävirhe](./media/log-analytics-log-searches/oms-search-error.png)  
    Jos esimerkiksi kyselyn `error` seuraavan kuvan mukaisesti palautti **100 000 tapahtumatiedot (keräämiä hallinta)** , 18 **ConfigurationAlert** tietueiden (luomien kokoonpanon arviointi) ja 12 **ConfigurationChange** tietueiden (siepatun muutosten jäljityksen).   
    ![Etsinnän tulokset](./media/log-analytics-log-searches/oms-search-results01.png)  

Nämä suodattimet eivät ole todella objektien tyypit ja luokat. *Tyyppi* on vain tunniste-ominaisuus tai merkkijono/nimi ja luokka, joka on liitetty tiedosta. Jotkin tiedostojen järjestelmässä on merkitty kuin **Tyyppi: ConfigurationAlert** ja jotkin **Tyyppi: Perf**tai **Tyyppi: tapahtuman**merkityt ja niin edelleen. Hakutulos, asiakirjan, tietueen tai tapahtuman näyttää raaka ominaisuudet ja niiden arvojen kunkin näiden tietojen laitteita ja kenttänimet avulla voidaan määrittää suodattimen, kun haluat hakea vain tietueet, joiden kentässä on sama arvo.

*Tyyppi* on todella vain kaikki tietueet sisältävät kentän, se ei ole sama kuin minkä tahansa muun kentän. Tämä määritettiin Laji-kentän arvon perusteella. Tietue on eri muotoa tai lomakkeen. - **Tyyppi = Perf**, tai **tyyppi = tapahtuman** on myös syntaksi, jotka haluat kyselyn suorituskykytietoja tai tapahtumien tietoja.

Voit käyttää kaksoispiste (:) tai yhtäläisyysmerkki (=) jälkeen kenttänimen ja arvon eteen. **Tyyppi: tapahtuman** ja **tyyppi = tapahtuman** on sama merkitys, voit valita haluamasi laite.

Näin on, jos tyyppi = Perf tietueella on "CounterName-nimisen kentän ja sitten voit kirjoittaa kyselyn yksinkertaisia `Type=Perf CounterName="% Processor Time"`.

Näin aloitat vain suorituskykytietoja suorituskyvyn laskuri nimi on "% suoritin Time".

### <a name="to-search-for-processor-time-performance-data"></a>Jos haluat etsiä suoritin aika suorituskyvyn tietoja
- Kirjoita kyselyn hakukentän`Type=Perf CounterName="% Processor Time"`

Voit tarkentaa ja käyttää **InstanceName = "Yhteensä" _** kyselyssä, joka on Windowsin suorituskyky laskuri. Voit myös valita pinta ja toinen **kenttä: arvo**. Suodattimen lisätään automaattisesti suodattimessa kyselyn kaavarivillä. Näet tämän seuraavan kuvan mukaisesti. Kertoo, mihin napsauttamalla **EsiintymänNimi: '_Kokonais'** kirjoittamatta mitään kyselyyn.

![Etsi pinta](./media/log-analytics-log-searches/oms-search-facet.png)

Kyselyn muuttuu`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

Tässä esimerkissä sinun ei tarvitse määrittää **tyyppi = Perf** saat tuloksen. Koska CounterName ja EsiintymänNimi kentät ovat vain tietueiden = Perf, kysely on tietyn palauttaa on samanlainen kuin pidempi, Edellinen-vaihtoehto:
```
CounterName="% Processor Time" InstanceName="_Total"
```

Tämä johtuu siitä kyselyn suodattimien arvioidaan saattoi *ja* keskenään. Tehokkaasti, lisää kentät lisäät ehdot, saat pienempi tietyn ja Hienostunut tulokset.

Jos esimerkiksi kyselyn `Type=Event EventLog="Windows PowerShell"` on sama kuin `Type=Event AND EventLog="Windows PowerShell"`. Palauttaa kaikki tapahtumat, jotka on kirjautunut sisään ja kerätty Windows PowerShell-tapahtumaloki. Jos lisäät suodattimen useita kertoja valitsemalla toistuvasti saman pinta ongelma on pelkästään kosmeettisten--se saattaa tarpeettomat etsintäpalkin, mutta se edelleen palauttaa saman tuloksen, koska implisiittinen AND-operaattorin on aina.

Voit peruuttaa implisiittinen AND-operaattorin helposti käyttämällä NOT-operaattorin erikseen. Esimerkki:

`Type:Event NOT(EventLog:"Windows PowerShell")`tai sitä vastaava `Type=Event EventLog!="Windows PowerShell"` kaikki tapahtumat tuotto kaikki muut lokit, jotka eivät ole Windows PowerShell-loki.

Voit käyttää muita totuusarvo-operaattoria kuten tai-arvon. Seuraava kysely palauttaa tietueet, jossa tapahtumaloki on joko sovelluksen tai järjestelmän.

```
EventLog=Application OR EventLog=System
```

Käytä edellisessä kyselyssä, palautetaan virhe tapahtumat tuloksen samat molemmissa lokitiedot.

Kuitenkin Jos poistat tai jättämällä implisiittinen ja paikassa, valitse seuraava kysely Valintakyselyt eivät tuota tuloksia ei tapahtumaloki merkintä, joka kuuluu sekä lokit. Tapahtumaloki tekstien on kirjoitettu vain yhteen kaksi lokitiedot.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Lisätietoja suodattimien käyttäminen

Seuraava kysely palauttaa 2 tapahtumalokien kaikissa tietokoneissa, joissa tiedot ovat lähettäneet merkintöjä.

```
EventLog=Application OR EventLog=System
```

![Etsinnän tulokset](./media/log-analytics-log-searches/oms-search-results03.png)

Valitsemalla jokin kenttiä tai suodattimet rajata tietyn tietokoneeseen, lukuun ottamatta kaikkien muiden niistä kysely. Kysely näyttää seuraavankaltaiselta.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Jossa on sama kuin seuraavassa implisiittinen ja vuoksi

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Jokainen kyselyn arvioidaan eksplisiittinen seuraavassa järjestyksessä. Huomaa, kaarisulje.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Tavoin tapahtumaloki-kentässä voit hakea tietoja vain tietyille tietokoneille joukolle lisäämällä tai. Esimerkki:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Vastaavasti tämä seuraava kysely palauttaa vain valittujen kaksi tietokoneiden **% suorittimen ajan** .

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Totuusarvo-operaattorit
Kun datetime- ja numerokentät, voit etsiä arvoa *suurempi kuin*- *suppeampaan kuin*, ja *pienempi tai yhtä*. Voit käyttää yksinkertaisen operaattoreita, kuten >, <>, =, < =,! = kyselyn etsintäpalkin.


Voit tehdä kyselyn tietyn tapahtumalokin tietyn ajanjakson aikana. Esimerkiksi seuraava lauseke Ohje ilmaistaan viimeisen 24 tuntia.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Jos haluat etsiä totuusarvo-operaattori
- Kirjoita kyselyn hakukentän`EventLog=System TimeGenerated>NOW-24HOURS"`  
    ![haun totuusarvo](./media/log-analytics-log-searches/oms-search-boolean.png)

Vaikka voit hallita aikaväli graafisesti ja useimmiten voit tehdä, on aika-suodatin myös suoraan kyselyn etuja. Esimerkiksi Tämä toimii erinomaisesti raporttinäkymiä, voit ohittaa kunkin ruudun riippumatta *Yleinen* kellonajan valitsin, dashboard-sivun ajan. Lisätietoja on artikkelissa [Ajan asioissa raporttinäkymät-ikkunassa](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).

Suodatus ajan mukaan, pidä mielessä tulokset hankkiminen kahden *Leikkaus* aika: määritetty OMS-portaalissa (S1) ja määritetty kyselyssä (S2).

![leikkauskohta](./media/log-analytics-log-searches/oms-search-intersection.png)

Tämä tarkoittaa sitä, jos aikajaksot eivät leikkaa, kuten OMS-portaalissa, jossa voit valita **tällä viikolla** kyselyn, jossa voit määrittää **viime viikolla**, sitten ei ole leikkauskohtaa ja et saa tuloksia.

Vertailuoperaattorit TimeGenerated kentän on hyötyä myös muissa tilanteissa. Esimerkiksi ja numeeriset kentät.

Esimerkiksi ilmoitetaan, että kokoonpanon arviointi ilmoitusten vakavuus seuraavat arvot:

- 0 = tiedot
- 1 = varoitus
- 2 = kriittinen

Voit kyselyn varoitus- ja kriittinen ilmoitusten ja jättää tiedoksi niistä seuraavan kyselyn kanssa:

```
Type=ConfigurationAlert  Severity>=1
```


Voit käyttää myös alueen kyselyt. Tämä tarkoittaa, että voit antaa arvojen jakson alku ja loppu alueeseen. Esimerkiksi jos haluat Operations Manager tapahtumaloki, jossa EventID on suurempi tai yhtä suuri 2100, mutta ei ole suurempi kuin 2199 tapahtumia, valitse seuraava kysely palauttavat ne.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


>[AZURE.NOTE] Sinun on käytettävä alueen syntaksi on kaksoispiste (:)-kentän arvo: erotin ja *eivät* yhtäläisyysmerkin (=). Kirjoita alueen ylä- ja alarajan loppu hakasulkeisiin ja erota ne toisistaan kaksi pisteet (.).

## <a name="manipulate-search-results"></a>Hakutulosten muokkaaminen

Kun etsit tietoja, kannattaa tarkentaa haun ja on hyvä taso päättää, tulokset. Kun tulokset hakemista, voit käyttää komentoja, joilla voit muuntaa ne.

Log Analytics haut *on* komentoja, noudata jälkeen pystysuora pystyviivan (|). Kyselymerkkijonon ensimmäinen osa on aina oltava suodattimen. Se määrittää toimialueille tiedot ja sitten "piiput" tulokset komennon kyselyjä. Voit sitten lisätä lisäkomentoja putkien. Tämä muistuttaa löyhästi Windows PowerShell-myyntijakso.

Yleensä Log Analytics haun kieli yrittää PowerShell tyyli ja tee samalla IT-ammattilaisille ja helpottamiseksi learning käyrän ohjeita noudattamalla.

Komentojen on verbien nimet, jotta näet helposti, mitä ne.  

### <a name="sort"></a>Lajittelu

Lajittele-komennon avulla voit määrittää lajittelujärjestyksen yhden tai usean kentän mukaan. Vaikka et käytä sitä oletusarvon mukaan, laskevassa järjestyksessä aika otetaan käyttöön. Viimeisin tulokset ovat aina hakutulosten yläreunassa. Tämä tarkoittaa, että kun suoritat haun `Type=Event EventID=1234` mitä todellisuudessa suoritetaan, on:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Tämä johtuu siitä, että olet tutustunut lokit kokemus tyyppi on. Valitse esimerkiksi Windows-Tapahtumienvalvonta.

Voit lajitella tulokset palautetaan näyttötavan muuttaminen. Seuraavissa esimerkeissä, miten tämä toimii.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Yllä yksinkertainen esimerkeissä näytetään voit komennot toiminnasta – ne että suodatin palauttaa tulokset muodon muuttaminen.

### <a name="limit-and-top"></a>Raja- ja top
Toisen vähemmän tunnetut komennon ovat. Rajoitus on PowerShell kaltaisessa verbin. Rajoitus on toiminnallisesti samanlaiset YLÄREUNAN-komennon avulla. Seuraavat kyselyt palauttavat saman tuloksen.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Jos haluat etsiä alkuun
- Kirjoita kyselyn hakukentän`Type=Event EventID=600 | Top 1`   
    ![Etsi ylös](./media/log-analytics-log-searches/oms-search-top.png)

Yllä oleva kuva on 358 tuhat-tietueet, joiden EventID = 600. Kentät, rakenteita ja suodattimet vasemmalla Näytä aina tulokset tietoja palauttaa *mukaan suodatin-osan* kyselyn, joka on osa ennen minkä tahansa pystyviivan. **Tulosruudussa** palauttaa vain viimeisimmän 1 tulos, koska esimerkkikomento muotoinen ja muuntaa tulokset.

### <a name="select"></a>Valitse

Valitse komento-funktio vastaa powershellissä Select-objekti. Se palauttaa suodatettuihin tuloksiin, joka ei ole kaikki alkuperäisen ominaisuuksiensa. Sen sijaan valitsee ominaisuuksia, jotka määrität.

#### <a name="to-run-a-search-using-the-select-command"></a>Jos haluat suorittaa haun select-komennon avulla

1. Kirjoita Etsi- `Type=Event` ja valitse sitten **Etsi**.
2. Valitse **Näytä Lisää +** jossakin tulokset tulokset sisältävät ominaisuuksien tarkasteleminen.
3. Valitse osa ne nimenomaisesti ja kyselyn muutoksista `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Etsi Valitse](./media/log-analytics-log-searches/oms-search-select.png)

Tämä on erityisen hyödyllinen komento, kun haluat hallita haun tulostus ja valitse tiedot, jotka todella merkitystä, että tarkasteluun, joka ei usein täydelliset osiin. Tästä on hyötyä myös silloin, kun erityyppisiä tietueella on *joitakin* yhteisiä ominaisuuksia, mutta ei *kaikkien* niiden ominaisuudet ovat yleisiä. Voit luoda tulosteen, joka näyttää enemmän luonnollisesti taulukon tai toimivat hyvin, kun viedään CSV-tiedostoon ja sitten massaged Excelissä.



## <a name="use-the-measure-command"></a>Mitta-komennolla

TOIMENPIDE on eniten monipuolisia komentoja Log Analytics hauissa. Sen avulla voit käyttää tietoja ja tuloksia tietyn kentän mukaan ryhmiteltyinä tilastolliset *Funktiot* . On useita tilastolliset funktiot, joka tukee mitta.

### <a name="measure-count"></a>Mittaa count()

Ensimmäinen tilastollinen funktio käsitteleminen ja jokin helpoin ymmärtää on *count()* -funktio.

Etsi kyselyn tuloksena kuten `Type=Event`, Näytä suodattimet, kutsutaan myös muita hakutulosten vasemmassa reunassa. Suodattimien näyttäminen jakauman arvojen tietyn kentän tulosten mukaan suorittaa haun.

![Etsi mittayksikön määrä](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Esimerkiksi **tietokone** -kentässä näkyy kuva ja se osoittaa, että lähes 739 tuhat tapahtumat tuloksista sisällä 68 ainutlaatuisen ja eri arvoja näiden tietueiden **tietokoneen** -kentässä on. -Ruutu vain näkyy, ylin 5, mitkä ovat yleisimmät 5 arvot, jotka on kirjoitettu **tietokoneen** kentät), lajiteltu tietyn arvon kentän sisältävät tiedostojen määrä. Kuva näet, että – kesken lähes 369 tuhat – tapahtumat 90 tuhat peräisin OpsInsights04.contoso.com tietokoneen 83 tuhat DB03.contoso.com tietokoneesta ja niin edelleen.


Jos haluat nähdä kaikki arvot, koska se näkyy vain vain alusta 5?

Mitä mitta-komennon voit tehdä count()-funktio on. Tämä toiminto ei käytä parametreja. Voit myös määrittää vain kenttä, jonka haluat ryhmitellä **tietokoneen** kentän mukaan – tässä tapauksessa:

`Type=Event | Measure count() by Computer`

![Etsi mittayksikön määrä](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

**Tietokone** on kuitenkin vain käytettävä kenttä *-* jokainen tieto – on ei relaatiotietokannat ja ei ole erillinen **tietokone** -objekti missä tahansa. Vain-arvot *-* tiedot voi kuvaus minkä kohteen luotu ne ja muut ominaisuudet ja tiedot – näin ollen ominaisuuksia termin *pinta*. Voit kuitenkin ryhmitellä vain myös muita kenttiä. Koska alkuperäinen tulokset lähes 739 tuhat tapahtumia, jotka ovat piped kyselyjä mitta-komento on myös **EventID**-nimisen kentän, voit käyttää samaa menetelmää kentän Ryhmittelyperuste ja laskeminen tapahtumia EventID mukaan:

```
Type=Event | Measure count() by EventID
```

Jos et ole kiinnostunut, jotka sisältävät tietyn arvon todellisen tietuemäärä, mutta sen sijaan Jos haluat vain itse arvojen luettelon, voit lisätä *Select* -komennon lopussa sen ja vain Valitse ensimmäisessä sarakkeessa:

```
Type=Event | Measure count() by EventID | Select EventID
```

Voit saada lisää monimutkaisia ja lajitella valmiiksi kyselyn tuloksia, tai voit napsauttaa ruudukon sarakkeet liian.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Jos haluat etsiä mittayksikön määrä

- Kirjoita kyselyn hakukentän`Type=Event | Measure count() by EventID`
- Liitä `| Select EventID` kyselyn loppuun.
- Lopuksi liittäminen `| Sort EventID asc` kyselyn loppuun.


Liittyy muutama huomata ja korostaa tärkeät seikat:

Ensin tulokset eivät ole alkuperäisen raaka tulokset enää. Sen sijaan, ne ovat koostetun tulokset – pääosin tulosten ryhmät. Tämä ongelma ei ole, mutta pitäisi ymmärtää, että olet käsitellyt hyvin toiseen muotoon, joka eroaa tietojen alkuperäinen raaka muodosta, jotka luodaan suoraan selaimessa tuloksena kooste/tilastollinen funktio.

Seuraavaksi **mittayksikön Laske** palauttaa tällä hetkellä vain 100 suurinta eri tulokset. Tämä rajoitus ei koske muiden tilastolliset funktiot. Näin on tarvitset yleensä tarkemmin suodattimien käyttäminen ensimmäisen kerran, ennen kuin asennat mittayksikön count() tiettyjen kohteiden etsiminen.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Mitta-komennon käyttäminen suurin ja pienin-Funktiot

On eri tilanteista, joissa **Mitataan Max()** ja **Mittayksikön Min()** on hyötyä. Kuitenkin jokaisen funktion ollessa vastakkainen toisistaan, emme havainnollistaa Max() ja voit kokeilla Min() Omat muistiinpanot.

Jos testaat suojauksen tapahtumille, heillä on **taso** -ominaisuus, joka voi vaihdella. Esimerkki:

```
Type=SecurityEvent
```

![Etsi mittayksikön Laske alku](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Jos haluat tarkastella kaikkien suojaus suurimman arvon tapahtumien annetut yhteiset tietokoneen group by-kenttä, voit käyttää

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![Etsi mittayksikön max-tietokone](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Se, että tietokoneissa, joissa **taso** -tietuetta, useimmilla ne on näytön taso vähintään 8, monet olivat 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![Etsi mittayksikön max aika luotu tietokoneeseen](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Tämä funktio toimii hyvin numeroita, mutta se toimii myös DateTime-kenttien kanssa. Kannattaa tarkistaa kaikki tieto indeksoitu kussakin tietokoneessa, viimeiseen tai viimeksi katsottuun aikaleiman. Esimerkki: kun viimeisimmän suojauksen tapahtuman ilmoitettiin kunkin tietokoneeseen?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>KESKIARVO-funktiota käytetään mitta-komento

Avg() tilastollinen funktio käyttää mitalla avulla voit laskea keskiarvon joitakin kentän ja Ryhmätulokset samassa tai toisessa kentän mukaan. Tämä on käteviä tapaukset, kuten suorituskykytietoja.

Asetetaan ensin suorituskykytietoja. Huomaa, että OMS kerää tällä hetkellä suorituskyvyn laskureita Windows-ja Linux.

Jos haluat etsiä *kaikki* suorituskykytietoja, yleisin kysely on:

```
Type=Perf
```

![Etsi avg alku](./media/log-analytics-log-searches/oms-search-avg01.png)

Huomaa, huomaat ei että loki Analytics näkyy kolme perspektiivit: luettelo, joka näyttää, jossa näkyy todellinen tietueet takana kaavioiden; Taulukko, joka näyttää taulukkonäkymän laskuri resurssitietojen; ja arvot, jossa näkyvät kaavioiden suorituskyvyn laskureita.

Yllä oleva kuva on kaksi joukkoa merkityt kentät, jotka ilmaisevat seuraavasti:

- Ensimmäisen joukon tunnistaa Windowsin suorituskyky laskuri nimen objektinimi ja esiintymänimi kyselyn suodattimeen. Todennäköisesti yleisimmin käytät rakenteita/suodattimina kentät
- **CounterValue** on todellinen arvo laskuri. Tässä esimerkissä arvo on *75*.
- **TimeGenerated** on 12:51 24-tuntinen aika-muodossa.

Seuraavassa on näyttää kaavion arvot.

![Etsi avg alku](./media/log-analytics-log-searches/oms-search-avg02.png)

Lukutilassa Perf tietueen muodon tietoja ja ottaa lisätietoja Etsi muita menetelmiä, kun koostaa tällaista numeerisia tietoja mittayksikön Avg() avulla.

Tässä on esimerkki:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![Etsi avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

Valitse tässä esimerkissä suorittimen kokonaisaika suorituskyvyn laskuri- ja keskiarvo tietokoneella. Jos haluat tarkentaa hakutulokset vain viimeisen 6 tuntia, voit määrittää aika-suodatin tai Määritä kyselyn seuraavasti:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Voit etsiä mitta-komennon käyttäminen keskiarvo-funktio
- Kirjoita kyselyn hakukenttään `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.


Voit koota ja yhdistää tietoja *eri* tietokoneissa. Oletetaan esimerkiksi, että sinulla on tarkoitus klusterin, jossa kukin solmu on yhtä kuin, jotta muut ne vain samat Kirjoita työn ja kuormituksen tulee jakaa karkeasti isännät joukko. Voi hakea kaikkia yhdessä Siirry seuraavien kyselyn ja tarkastella keskiarvojen koko palvelinfarmin niiden laskureita. Voit aloittaa valitsemalla tietokoneissa, joissa seuraavassa esimerkissä:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Nyt kun olet luonut tietokoneet, myös vain haluat valita kaksi suorituskykyilmaisimia (KPI): suorittimen käyttö ja prosentin vapaa levytila. Kyselyn osa tulee

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Voit nyt lisätä tietokoneita ja laskureita kanssa seuraavassa esimerkissä:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Koska sinulla on hyvin tietyn valinnan, **mittayksikön Avg()** -komento palauttaa keskiarvo tietokoneen mukaan, mutta palvelinfarmin, riittää, että CounterName mukaan ryhmittely. Esimerkki:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Kyseinen rivimäärä on hyötyä suppea näkymä, pari ympäristö valmistellaan KPI: T.

![Etsi avg ryhmittely](./media/log-analytics-log-searches/oms-search-avg04.png)


Voit helposti hakukyselyn raporttinäkymät-ikkunassa. Voit esimerkiksi tallentaa hakukyselyn ja raporttinäkymien luominen se nimeltä *Web klusterin KPI: T*. Lisätietoja raporttinäkymien käyttäminen, katso [luominen mukautetun raporttinäkymän Log Analytics](log-analytics-dashboards.md).

![Etsi avg Raporttinäkymät-ikkunan](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Summa-funktiota käytetään mitta-komento

Summafunktio on samankaltainen kuin muut Funktiot mitta-komennon. Näet esimerkin summa-funktion käyttämisestä osoitteessa [W3C IIS lokit hakeminen Microsoft Azure toiminnallisia tiedot](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).

Voit käyttää Max() ja Min() numeroita, päivämäärä ajat ja tekstimerkkijonoja. Tekstimerkkijonoja se on lajiteltu aakkosjärjestykseen ja saat ensimmäinen ja viimeinen.

Voi kuitenkin käyttää Sum() kanssa mitään kuin numeeriset kentät. Tämä koskee myös Avg().

### <a name="use-the-percentile-function-with-the-measure-command"></a>Mitta-komennon käyttäminen prosenttipiste-funktio

Prosenttipiste-funktio on samanlainen Avg() ja Sum() siten, että voit käyttää vain sellaisia, numeeriset kentät. Voit käyttää mitä tahansa prosenttipiste 1-99 numeerisen kentän. Voit käyttää myös **prosenttipiste** ja **pros.** komentoja. Seuraavassa on muutamia esimerkkejä:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Käytä missä komento

Jos komento toimii samalla tavalla kuin suodattimen, mutta sitä voidaan käyttää suodatukseen koostetun että mittayksikön – komennon tuottamat tulokset myyntijakso verrattuna raaka tuloksia, jotka on suodatettu kyselyn alussa.

Esimerkki:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Voit lisätä toisen putkien "|" merkin ja mistä komennon saat vain tietokoneissa, jonka keskimääräinen suorittimen on yli 80 % kanssa seuraavassa esimerkissä:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Jos olet tottunut käyttämään Microsoft System Center - Operations Manager, voit ajatella missä komennon management pack ehdot. Jos esimerkin säännön, kyselyn ensimmäinen osa on tietolähteen ja missä komento olisi ehto tunnistamista.

Voit tehdä kyselyn ruuduksi **Oma Raporttinäkymät-ikkunan**näyttölaitteena lajitteluja ja tarkistaa, milloin tietokoneen suorittimien ovat ylikuormitettu. Lisätietoja raporttinäkymien artikkelissa [luominen mukautetun raporttinäkymän Log Analytics](log-analytics-dashboards.md). Voit myös luoda ja käyttää raporttinäkymät-Mobiilisovelluksella. Lisätietoja on artikkelissa [OMS Mobile-sovelluksesta ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). Seuraava kuva ala kaksi ruudut, näet näytössä näkyy luettelo ja numero. Aina sarakenumeron haluat nolla ja luettelo on tyhjä. Muussa tapauksessa osoittaa ilmoitusten ehto. Tarvittaessa voit käyttää sitä, katso, mitä koneet ovat paineenalaisena.

![Mobiili-koontinäyttö](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>In-operaattorin käyttäminen

*Valitse* operaattori, sekä *NOT IN* avulla voit käyttää subsearches, jotka ovat haut, jotka sisältävät etsintä argumenttina. Ne sijaitsevat aaltosulkeet {} * *ensi* - tai* etsintä kuluessa. Subsearch usein eri hakutulosten luettelo saatu käytetään ensisijainen etsinnän argumenttina.

Voit käyttää subsearches vastaamaan, ei kuvaa suoraan-hakulauseke, mutta jotka voidaan luoda haun tietojen alijoukot. Esimerkiksi jos olet kiinnostunut yhden haun avulla voit etsiä kaikki tapahtumat *puuttuu päivitykset*tietokoneesta, valitse haluat suunnitella subsearch, joka määrittää ensin, että *puuttuu päivitykset tietokoneeseen* ennen löytymistä tapahtumia, jotka kuuluvat näiden isännät.

Siis voi express seuraava kysely ja *tietokoneiden puuttuu tällä hetkellä tarvittavat päivitykset* :

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![Etsi esimerkissä](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Kun luettelossa, voit tehdä haun kuin sisemmän haun syötteen tietokoneiden luetteloa, jotka näyttävät näistä tietokoneista tapahtumien uloimman (perus) haku. Voit tehdä tämän kirjoittamalla sisemmän haun aaltosulkeet ja käyttöä sen tulokset kuin on ulompi search IN-operaattorin käyttäminen Suodatin/kentän mahdollisia arvoja. Kyselyn olla esimerkiksi:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![Etsi esimerkissä](./media/log-analytics-log-searches/oms-search-in02-revised.png)


Myös ilmoitus aika-suodattimesta käyttää sisemmän hakutoiminnossa, koska järjestelmässä päivitys arvioinnin kestää tilannevedoksen kaikkiin tietokoneisiin 24 tunnin välein. Voit tehdä sisemmän kyselyn Lisää kevyt ja tarkat hakemalla vain päivässä. Ulomman haku käyttää sen sijaan aika-valinnan käyttöliittymässä, noutaminen viimeisen seitsemän päivän tapahtumat. Saat lisätietoja aika operaattorit [Totuusarvo-operaattorit](#boolean-operators) .

Koska olet todella vain suodattimena sisemmän etsinnän tulokset value ulompi kohdalla, voit edelleen käyttää ulompi haku-komennot. Voit esimerkiksi ryhmitellä edelleen yllä tapahtumat toiseen mitta-komennolla:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![Etsi esimerkissä](./media/log-analytics-log-searches/oms-search-in03-revised.png)


Yleensä haluat suorittaa nopeasti, koska loki Analytics on palvelun Include aikakatkaisu sen ja palauttamaan tulokset vähän sisemmän kyselyn. Jos sisempi kysely palauttaa enemmän tuloksia, tulosluettelon saa katkaistu, joka saattaa aiheuttaa ulompi haku palauttaa vääriä tuloksia.

Toinen sääntö on, että sisemmän haku on tällä hetkellä antamaan *koostetun* tulokset. Toisin sanoen se on oltava *Mitta* -komento; ei voi tällä hetkellä syötteen raaka tulokset ulompi haku.

Myös voi olla vain yksi IN-operaattorin ja sen on oltava kyselyn viimeinen suodatin. Useiden IN operaattorien ei voida tai tapaan – tämä olennaisesti estää käynnissä useita subsearches: tärkeitä piste on vain yksi, että sub/sisä-haku on mahdollista kunkin ulompi haun.

Silloinkin, kun nämä raja-arvot IN mahdollistaa monenlaisia Korreloidun hakuja ja voit määrittää kaltaisen ryhmät tietokoneissa, kuten käyttäjien tai tiedostojen – jostakin kenttien tietojen sisältää. Seuraavassa on muita Esimerkkejä:

**Kaikki päivitykset puuttuu tietokoneista, joissa automaattisen päivityksen asetus on poistettu käytöstä**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Kaikkien virhe tapahtumien tietokoneista, SQL Server (=, jossa on suorittaa SQL-arviointi)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Kaikki suojauksen tapahtumat tietokoneista, jotka ovat toimialueen ohjaimet (=, jossa AD arviointi on suorittaa)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Jossa saman tietokoneissa, johon tilin BACONLAND\jochan on kirjautunut olet kirjautunut muiden tilien kautta?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Distinct-komennolla

Kuten nimi ilmaisee, tämä komento on eri arvojen luettelo kentän. On käyttäjälomakkeen yksinkertaisen mutta varsin hyödyllinen. Sama voitaisiin saavuttaa mittayksikön count()-komennolla sekä, alla kuvatulla tavalla.

```
Type=Event | Measure count() by Computer
```

![ERI haun komento Esimerkki](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Jos kaikki et ole kiinnostunut on kuitenkin pelkkä lista eri arvoja ja ei asiakirjoja, joissa on, arvojen ja sitten DISTINCT tarjoavat määrää selkeämpi ja helppolukuisempia tulostus ja lyhyempää oletussyntaksia alla olevan kuvan mukaisesti.

```
Type=Event | Distinct Computer
```
![ERI haun komento Esimerkki](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Mitta-komennon countdistinct-funktion käyttäminen
Countdistinct-funktio laskee kunkin ryhmän eri arvojen määrä. Esimerkiksi sitä voi käyttää laskeminen mistäkin raportoinnin yksilöllinen tietokoneiden määrä:

```
* | measure countdistinct(Computer) by Type
```

![OMS countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Mittayksikön väli-komennolla
Kanssa lähellä reaaliaikaisia suorituskykyyn liittyvä tietojen kerääminen, voit kerätä ja visualisointi minkä tahansa suorituskyvyn laskuri Log Analytics. Profiilinasetuksensa antamalla **Tyyppi: Perf** palauttavat metrisillä kaavioiden tuhansia kyselyn perusteella laskureita ja palvelimien Log Analytics-ympäristössä. Tarvittaessa metrisillä yhdistäminen, jossa voi tarkastella yleinen arvot ympäristössäsi korkean tason ja sinun on eritellympiä tiedot perinpohjaisesti käsittelevään artikkeliin.

Oletetaan, että haluat tietää, mitkä asiat suorittimen keskiarvo eri kaikissa tietokoneissa. Jokainen tietokone keskiarvon suorittimen katsoo eivät välttämättä ole hyödyllisiä koska tulokset Hae tasoitetun. Tarkista kyselyjä enemmän tietoja, voit yhdistää tuloksen pienempi aika ikkunassa näkyvissä ja Etsi aikasarjan tuominen eri mitat yli. Esimerkiksi voit suorittaa suorittimen käyttö tunnin välein keskiarvon eri kaikissa tietokoneissa seuraavasti:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![mittayksikön keskimääräinen väli](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Oletusarvoisesti tulokset näkyvät useita sarjoja vuorovaikutteinen viiva.  Tässä kaaviossa tukee sarjan VAIHTO (ja y-akselin rescaling), zoomaus ja hiiren osoitin.  Näytä taulukko-kohta on edelleen käytettävissä tarkasteluun perustietoja tarvittaessa.

Voit ryhmitellä muita kenttiä. Tässä esimerkissä kaikki tietyn tietokoneiden %-laskureita katsoo ja haluan tietää, mitkä asiat jokaisen laskuri hourly 70-arvona:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Huomautus on nämä kyselyt eivät ole rajoitettu suorituskyvyn laskureita. Voit käyttää niitä mikä tahansa arvo. Tässä esimerkissä etsimääni W3C IIS-lokit-palvelussa. Haluan tietää, mitkä asiat enimmäisajan, se on ensisijainen 5 minuutin väli, sivupyynnön käsittelyä varten:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Käytä useita koosteet toisessa kyselyssä
Voit määrittää useita kooste lausekkeita mitta-komennolla.  Kukin voi olla viittaama erikseen.  Jos se ei ole annettu tunnus tuloksena olevan kenttänimi on kooste-funktio, joka on käytetty (eli "avg(CounterValue)" avg(CounterValue)) varten.

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Toinen esimerkki näin:
 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja lokin haut:

- [Mukautetut kentät Log Analytics](log-analytics-custom-fields.md) avulla voit laajentaa log haut.
- Tarkista [loki Analytics Kirjaudu haun viittaus](log-analytics-search-reference.md) Tarkastele kaikkia haun kenttien ja muita käytettävissä Log Analytics.
