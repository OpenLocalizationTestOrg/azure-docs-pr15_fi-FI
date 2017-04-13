<properties
    pageTitle="Lokitiedoston Analytics Etsi viittaus | Microsoft Azure"
    description="Log-Analytics haun viittaus kuvaa haun kieli ja sisältää yleiset kyselysyntaksia vaihtoehdot voidaan käyttää silloin, kun tietojen hakemisesta ja suodattamisesta lausekkeiden avulla tarkentaa hakua."
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
    ms.date="10/25/2016"
    ms.author="banders"/>


# <a name="log-analytics-search-reference"></a>Kirjaudu Analytics haun viittaus

Seuraavassa viittaus tietoja haun kieli kerrotaan Yleiset kyselyn syntaksi-asetusten avulla voit tietojen hakemisesta ja suodattamisesta lausekkeiden avulla tarkentaa hakua. Tässä artikkelissa myös komentoja, joiden avulla voit hakea tietoja toimenpiteistä.

Voit lukea tietoja määrää hakuja ja rakenteita, joiden avulla voit dill kentistä-samalla luokkiin tietojen [hakukenttään ja pinta-kohdassa](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Yleiset kyselyn syntaksi

Syntaksi:

```
filterExpression | command1 | command2 …
```

Suodatin-lausekkeen (`filterExpression`) määrittää kyselyn "where"-ehto. Kysely palauttaa tuloksia koskevat komennot. Useita komentoja on erotettu pystyviivan (|).

### <a name="general-syntax-examples"></a>Yleinen syntaksi

Esimerkkejä:

```
system
```

Tämä kysely palauttaa tuloksia, jotka sisältävät sanan "järjestelmän" kentän, joka on indeksoitu koko tekstin tai haun ehdot.

>[AZURE.NOTE] Kaikki kentät on indeksoitu tällä tavalla, mutta yleensä yleisimmät tekstimuotoinen kentät (kuten kuvaukset ja nimet) olisi.

```
system error
```

Tämä kysely palauttaa tuloksia, jotka sisältävät sanat *Järjestelmä* - ja *virhetietoja*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Tämä kysely palauttaa tuloksia, jotka sisältävät sanat *Järjestelmä* - ja *virhetietoja*. Valitse lajittelee tulokset *ManagementGroupName* -kenttä (nousevassa järjestyksessä) ja napsauta *TimeGenerated* (laskevaan järjestykseen). Kestää vain ensimmäiset 10 tulokset.

>[AZURE.IMPORTANT] Kaikkien kenttien nimien ja merkkijono ja teksti-kenttien arvot ovat kirjainkoko on merkitsevä.

## <a name="filter-expression"></a>Suodatin-lausekkeen

Seuraavissa jaksoissa kerrotaan suodattimen lausekkeita.

### <a name="string-literals"></a>Merkkijono literaalien

Merkkijonoliteraalia on merkkijono, joka ei tunnista jäsennin avainsana tai valmiin tietotyyppi (esimerkiksi numero tai päivämäärä).

Esimerkkejä:

```
These all are string literals
```

Tämä kysely etsii tuloksia, jotka sisältävät kaikki viisi sanat esiintymät. Monimutkainen merkkijonon haku kehystä merkkijonoliteraali lainausmerkkeihin, esimerkiksi:

```
" Windows Server"
```

Tämä palauttaa vain tuloksia tarkkojen vastineiden "Windows Server".

### <a name="numbers"></a>Luvut

Jäsennin tukee kymmenjärjestelmän ja perus numero syntaksi numeeriset kentät.

Esimerkkejä:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="datetime"></a>Päivämäärä ja aika

Jokaisen tieto järjestelmässä on *TimeGenerated* -ominaisuutta, joka edustaa alkuperäisen päivämäärän ja kellonajan tietueen. Jotkin tietotyyppiä lisäksi voi olla kenttiä (esimerkiksi *LastModified*)-päivämäärä ja kellonaika.

Log Analytics aikajanan kaavion/aika-Valitsin näyttää tulokset jakautumisen ajan kuluessa (mukaan nykyisen kyselyn suoritettavan), *TimeGenerated* -kentän perusteella. Päivämäärä ja kellonaika kentällä on tiettyjä merkkijonon-muoto, jonka avulla voidaan rajoittaa kyselyn tietyn aikajakson kyselyt. Voit käyttää syntaksi myös viittaamaan suhteellinen (esimerkiksi "välisten aikavälien kolme päivää ja 2 tuntia aiemman").

Syntaksi:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Esimerkki:

```
TimeGenerated:2013-10-01T12:20
```

Edellisen komennon palauttaa tietueet, joiden *TimeGenerated* arvo tarkasti 12:20 1. lokakuussa 2013. On epätodennäköistä, että se antaa minkä tahansa tuloksen, mutta tiedät idea.

Jäsennin tukee Ohje päivämäärä ja kellonaika-arvon myös nyt.


On epätodennäköistä, että tämä tuotto minkä tahansa tuloksen, sillä tietoja ei tee siitä, että nopea järjestelmän kautta.

Näissä esimerkeissä ovat rakenneosien käyttämään suhteelliset ja suorat päivämäärille. Seuraavat kolme jaksoissa kerromme käyttämisestä monimutkaisempia esimerkkejä, joissa suhteellinen päivämäärävälejä suodattimien.

### <a name="datetime-math"></a>Päivämäärä ja kellonaika matemaattiset

Siirtymä- tai päivämäärä ja kellonaika-arvosta pyöristää päivämäärä ja kellonaika yksinkertaisia laskutoimituksia käyttämällä päivämäärä ja kellonaika matemaattisia operaattoreita avulla.

Syntaksi:

```
datetime/unit
```

```
datetime[+|-]count unit
```

|Operaattori|Kuvaus|
|---|---|
|/|Pyöristää määritettyä päivämäärä ja kellonaika. Esimerkki: Nyt / päivä pyöristää nykyinen päivämäärä ja kellonaika nykyisen päivän keskiyöhön.|
|+ tai –|Siirtymät päivämäärä ja kellonaika määritetyn yksiköiden määrän mukaan. Esimerkkejä: nyt + 1 tunti siirtymät mukaan tunnin ahead.2013 nykyinen päivämäärä ja kellonaika-10-01T12:00-10 päivää siirtymät päivämääräarvon takaisin 10 päivällä.|



Päivämäärä ja kellonaika matemaattisia operaattoreita voit esimerkiksi ketjut yhdessä:

```
NOW+1HOUR-10MONTHS/MINUTE
```

Seuraavassa taulukossa on lueteltu tuetut pvm. / klo-yksiköt.

Päivämäärä ja kellonaika yksikkö|Kuvaus
---|---
VUOSI-, VUOSI|Pyöristää luvun kuluvan vuoden tai siirtymät määritetty vuosien määrän mukaan.
KUUKAUDEN KUUKAUTTA|Pyöristää luvun nykyinen kuukausi tai siirtymät määritetyn kuukausien määrän mukaan.
PÄIVÄN, PÄIVÄN PÄIVÄMÄÄRÄ|Pyöristää luvun kuukauden nykyisen päivän tai siirtymät määritetyn päivien määrän mukaan.
TUNTI-TUNNIT|Pyöristää luvun nykyisen tunnin tai siirtymät tuntia määritetyn määrän mukaan.
MINUUTTI MINUUTTIA|Pyöristää luvun nykyisen minuutin tai siirtymät määritetyn minuutteina.
SEURAAVAKSI SEKUNTIA|Pyöristää nykyiseen toisen tai siirtymät sekunteina määritetyn määrän mukaan.
MILLISEKUNNIN MILLISEKUNTIA, MILLIEKVIVALENTTIA MILLIS|Pyöristää luvun nykyisen millisekunnin tai siirtymät mukaan, montako millisekuntia.


### <a name="field-facets"></a>Kentän rakenteita

Kentän rakenteita käyttämällä voit määrittää tiettyjen kenttien ja tarkka arvot eikä kirjoittaminen "tekstimuotoisen" kyselyjen eri ehtojen koko hakemiston hakuehto. On jo käytetty seuraavalla syntaksilla esimerkkien edellisen kohdan. Tässä kohdassa annamme monimutkaisempia esimerkkejä.

**Syntaksi**

```
field:value
```

```
field=value
```

**Kuvaus**

Hakee tietyn arvon kentän. Arvo voi olla merkkijono literaalimerkit, numero tai päivämäärä ja kellonaika.

Esimerkki:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntaksi**

*kentän > arvo*

*kentän < arvo*

*kentän > = arvo*

*kentän < = arvo*

*kentän! = arvo*

**Kuvaus**

Sisältää vertailuja.

Esimerkki:

```
TimeGenerated>NOW+2HOURS
```

**Syntaksi**

```
field:[from..to]
```

**Kuvaus**

Sisältää alueen faceting.

Esimerkki:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="logical-operators"></a>Loogiset operaattorit

Kyselyn kielet tukevat loogisten operaattorien (*ja* *tai*ja *ei*) ja niiden C-tyyppiseksi aliases (*&&*, *||*, ja *!*) tarpeen mukaan. Voit ryhmitellä operaattoreista sulkeita.

Esimerkkejä:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Voit jättää looginen operaattori ylimmän tason suodatin-argumenteille. Tässä tapauksessa AND-operaattorin olettaa.


Suodatin-lausekkeen|Vastaa
---|---
Virhe|järjestelmän ja virhe
järjestelmän "Windows Server-tai vakavuus-: 1|Järjestelmä- ja ("Windows Server-tai vakavuus: 1)

### <a name="wildcarding"></a>Wildcarding

Kyselykieli tukee (*\*) edustaa yhtä tai useampaa merkkiä kyselyn arvon merkki.

Esimerkkejä:

 Etsi kaikki tietokoneet "SQL-nimi, esimerkiksi"Redmond SQL".

```
Type=Event Computer=*SQL*
```

>[AZURE.NOTE] Yleismerkkejä kuluessa tarjouksia ei voi käyttää tänään. Viestin =`"*This text*"` tarkastelee (\*) käytetään literaalimerkit (\*) merkki.
## <a name="commands"></a>Komennot

Komentojen käyttäminen että kysely palauttaa tulokset. Lisätä komennon haetut tulokset käyttämällä pystyviivan (|). Useita komentoja on erotettava putkien-merkin.

>[AZURE.NOTE] Komentojen nimet voidaan kirjoittaa isoiksi tai pieniksi kirjaimiksi, toisin kuin kenttien nimet ja tiedot.

### <a name="sort"></a>Lajittelu

Syntaksi:

    sort field1 asc|desc, field2 asc|desc, …

Lajittelee tuloksia tietyn kentän mukaan. Nouseva tai Laskeva etuliite on valinnainen. Jos ne jätetään pois, oletetaan *asc* lajittelujärjestystä. Jos kysely ei käytä *Lajittele* -komento erikseen, Lajittele **TimeGenerated** desc ovat oletusasetukset ja se palauttaa aina uusimmat tuloksia ensin.

### <a name="toplimit"></a>Ylös/raja

Syntaksi:

    top number


    limit number
Rajaa ylimmät N tulokset vastauksen.

Esimerkki:

    Type:Alert errors detected | top 10

Palauttaa 10 vastaavia tuloksia.

### <a name="skip"></a>Ohita

Syntaksi:

    skip number

Siirtyy luettelossa tulosten määrää.

Esimerkki:

    Type:Alert errors detected | top 10 | skip 200

Palauttaa tuloksen 200 aloittaen Ylin 10 hakutuloksia.

### <a name="select"></a>Valitse

Syntaksi:

    select field1, field2, ...

Valitse kentät rajoitukset tulokset.

Esimerkki:

    Type:Alert errors detected | select Name, Severity

Rajoittaa palautettujen tulosten kentät *nimi* ja *vakavuus*.

### <a name="measure"></a>Toimenpide

*Mitta* -komentoa käytetään tilastofunktiot koskee raaka hakutulokset. Tämä on erittäin hyödyllinen saat *Ryhmittelyperuste* näkymien tietojen. Käytettäessä *Mitta* -komentoa Log Analytics haun näyttää taulukon, jossa on koostetun tulokset.

Syntaksi:

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Koostaa tulosten mukaan *ryhmäkenttä* ja laskee koostetun mitta-arvot käyttämällä *aggregatedField*.


|Mittayksikön tilastollinen funktio|Kuvaus|
|---|---|
|*aggregateFunction*|(Kirjainkokoa) koostefunktio nimi. Koostefunktioista tuetaan: Laske Maks MIN summa AVG STDDEV COUNTDISTINCT PROSENTTIPISTEEN ## tai PROS. ## (## on numero väliltä 1 – 99)|
|*aggregatedField*|Kentän, joka on koostetaan. Tämä kenttä on valinnainen määrä kooste-funktio, mutta se on oltava aiemmin luodun numeerisen kentän summa, MAX, MIN, AVG STDDEV prosenttipiste ## tai PROS. ## (## on numero väliltä 1 – 99). AggregatedField myös voi olla mikä tahansa tuettu Laajenna-funktioita.|
|*fieldAlias*|Laskettu koostetun arvon (valinnainen) alias. Jos ei määritetä, sen kenttänimi on AggregatedValue.|
|*ryhmäkenttä*|Kentän, joka tulosjoukon nimi on ryhmitelty.|
|*Väli*|Aikaväli-muodossa:**nnnNAME** Where: nnn on positiivinen kokonaisluku. **Nimi** on aikaväli. Tuetut aikavälin nimet sisältävät (kirjainkoko on merkitsevä): MILLISEKUNNIN [S] toisen [S] minuutit [S] päivänä TUNTIEN [S] [S] kuukauden [S] vuoden [S]|


Aikaväli-vaihtoehtoa voi käyttää vain päivämäärä ja kellonaika ryhmäkentät (kuten *TimeGenerated* ja *TimeCreated*). Tällä hetkellä tämän ei säilytetä palvelu, mutta kentän ilman päivämäärä ja aika, joka siirretään taustaan aiheuttaa Suorituksenaikainen virhe. Kun rakenteen vahvistus on otettu käyttöön, service API hylkää aikavälin kooste kenttiä ilman päivämäärä ja kellonaika käyttävien kyselyjen. *Mittayksikön* käyttöönottoon tukee aikavälin ryhmittely kooste-funktio.

Jos BY-lause jätetään pois, mutta väli on määritetty (toinen syntaksi), *TimeGenerated* -kentän oletusarvona on oletusarvon mukaan.

Esimerkkejä:

**Esimerkki 1**

    Type:Alert | measure count() as Count by ObjectId

*SELITYS*

Ryhmittelee mukaan *objektitunnus* ilmoituksia ja laskee, kuinka monta kunkin ryhmän ilmoitusta. Palauttaa koostetun arvon *laskeminen* -kenttä (alias).

**Esimerkki 2**

    Type:Alert | measure count() interval 1HOUR

*SELITYS*

Ryhmittelee ilmoituksia 1 tunnin välein käyttämällä *TimeGenerated* -kenttää ja palauttaa ilmoitusten määrä kussakin.

**Esimerkki 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

*SELITYS*

Sama kuin edellisessä esimerkissä, mutta koostekenttä alias (*AlertsPerHour*).

**Esimerkki 4**

    * | mittaa count() TimeCreated aikavälin 5DAYS mukaan

*SELITYS*

Ryhmittelee 5 päivän välein tulokset käyttämällä *TimeCreated* -kenttää ja palauttaa tulosten määrä kussakin.

**Esimerkki 5**

    Type:Alert | measure max(Severity) by WorkflowName

*SELITYS*

Ryhmittelee ilmoituksia työmäärää nimelläsi ja palauttaa ilmoitusten tärkeys jokaiselle työnkululle oma historia.

**Esimerkki 6**

    Type:Alert | measure min(Severity) by WorkflowName

*SELITYS*

Sama kuin edellisessä esimerkissä, mutta *Min* koostetun funktion.

**Esimerkki 7**

    Type:Perf | measure avg(CounterValue) by Computer

*SELITYS*

Ryhmittelee Perf tietokoneen ja laskee keskiarvo (keskiarvo).

**Esimerkki 8**

    Type:Perf | measure sum(CounterValue) by Computer

*SELITYS*

Sama kuin edellisessä esimerkissä, mutta käytetään *Summa-funktiota*.

**Esimerkki 9**

    Type:Perf | measure stddev(CounterValue) by Computer

*SELITYS*

Sama kuin edellisessä esimerkissä, mutta käyttää *STDDEV*.

**Esimerkki 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

*SELITYS*

Sama kuin edellisessä esimerkissä, mutta käyttää *PERCENTILE70*.

**Esimerkki 11**

    Type:Perf | measure pct70(CounterValue) by Computer

*SELITYS*

Sama kuin edellisessä esimerkissä, mutta käyttää *PCT70*. Huomaa, että *PROS. ##* vain tunnusnimen *prosenttipiste ##* -funktiota.

**Esimerkki 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

*SELITYS*

Ryhmittelee Perf ensin tietokoneeseen ja sitten CounterName ja laskee keskiarvo (keskiarvo).

**Esimerkki 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

*SELITYS*

Saa yläreunan viisi työnkulut, joiden ilmoitusten enimmäismäärä.

**Esimerkki 14**

    * | mittaa countdistinct(Computer) tyypin mukaan

*SELITYS*

Laskee raportoinnin mistäkin yksilöllinen tietokoneiden määrä.

**Esimerkki 15**

    * | mittaa countdistinct(Computer) väli 1 tunti

*SELITYS*

Laskee yksilöllinen tietokoneiden raportointi tunnissa.

**Esimerkki 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

*SELITYS*

Ryhmittelee %: n suoritinaikaa tietokoneen ja palauttaa keskiarvo 1 tunnissa.

**Esimerkki 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

*SELITYS*

Ryhmittelee W3CIISLog menetelmällä ja palauttaa suurimman 5 minuutin välein.

**Esimerkki 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

*SELITYS*

Ryhmittelee % suoritinaikaa tietokoneen ja palauttaa pienin-, keskiarvo, 75 prosenttipiste ja enintään 1 tunnissa.

**Esimerkki 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

*SELITYS*

Ryhmittelee % ajan ensin tietokoneeseen ja napsauta esiintymänimi ja palauttaa pienin-, keskiarvo, 75 prosenttipiste ja enintään 1 tunnissa

**Esimerkki 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

*SELITYS*

Laskee levyn enintään kirjoittaa minuutissa jokaisen levyn tietokoneeseen

### <a name="where"></a>Jos

Syntaksi:

```
**where** AggregatedValue>20
```

Vain voidaan *Mitta* -komennon jälkeen suodatukseen että *mittayksikön* koostefunktio on valmistettu koostetun tulokset.

Esimerkkejä:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)

### <a name="in"></a>IN

Syntaksi:

```
(Outer Query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Kuvaus: Seuraavalla syntaksilla avulla voit luoda kooste ja syötteen arvoluetteloon arvoja, kooste toisen ulompi (perus) haun, jotka näyttävät näiden arvona tapahtumien kyselyjä. Voit tehdä tämän kirjoittamalla sisemmän haun aaltosulkeet ja käyttöä sen tulokset kuin on ulompi search IN-operaattorin käyttäminen kentän mahdollisia arvoja.

Sisemmän kyselyn Esimerkki: *tällä hetkellä puuttuu turvapäivitykset tietokoneita* seuraavat kooste kyselyn kanssa:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Lopulliset kyselyn, joka etsii *kaikki Windows-tapahtumien puuttuu tällä hetkellä päivitykset tietokoneeseen* muistuttaa:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="dedup"></a>Dedup

**Syntaksi**

    Dedup FieldName

**Kuvaus** Palauttaa ensimmäisen asiakirjan löydetty kaikki tietyn kentän yksilölliset arvot.

**Esimerkki**

    Type=Event | sort TimeGenerated DESC | Dedup EventID

Edellä olevassa esimerkissä palauttaa yhden tapahtuman (uusimmat jälkeen Microsoft käyttää DESC TimeGenerated) EventID kohti


### <a name="extend"></a>Laajenna

**Kuvaus** Voit luoda suorituksenaikainen kenttiä kyselyt. Voit käyttää myös mitta-komennon jälkeen Laajenna Jos haluat suorittaa kooste.

**Esimerkki 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Näytä painotettu suositus pisteet SQL arviointi suositukset

**Esimerkki 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Laskuri-arvon näyttäminen työstettävänä sijaan tavua

**Esimerkki 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Skaalaa WireData TotalBytes arvo siten, että kaikki tulokset ovat väliltä 0 – 100.

**Esimerkki 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Perf laskuri arvot on pienempi kuin 50 % las pieni ja muiden merkitseminen suuri

**Esimerkki 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Laskee levyn enintään kirjoittaa minuutissa jokaisen levyn tietokoneeseen

**Tuetut toiminnot**


| Funktio | Kuvaus | Esimerkkejä syntaksi |
|---------|---------|---------|
| itseisarvo | Palauttaa määritetyn arvon tai funktion. | `abs(x)` <br> `abs(-5)` |
| ACOS | Palauttaa arvon tai funktioon arkuskosinin | `acos(x)` |
| ja | Palauttaa arvon TOSI, vain, jos kaikki sen muunnokset arvo on TOSI. | `and(not(exists(popularity)),exists(price))` |
| ASIN | Palauttaa arkussinin arvon tai funktioon | `asin(x)` |
| ATAN | Palauttaa arkustangentin arvon tai funktioon | `atan(x)` |
| ATAN2 |  Palauttaa kulman suorakulmion koordinaatteja muuntamisen tuloksena x-, y-akselin napa koordinaatit | `atan2(x,y)` |
| cbrt | Kuution pääkansio |  `cbrt(x)` |
| ceil | Pyöristää luvun ylöspäin lähimpään kokonaislukuun | `ceil(x)`  <br> `ceil(5.6)`-Funktio palauttaa arvon 6 |
| COS | Palauttaa annetun kulman kosinin | `cos(x)` |
| COSH | Palauttaa kulman hyperbolisen kosinin | `cosh(x)` |
| DEF | DEF on lyhyt oletusarvo. Palauttaa arvon kentät"", tai jos kentässä ei ole määritetty oletusarvo palauttaa ja tuottaa ensimmäisen arvon missä: `exists()==true`. | `def(rating,5)`– Def() tämä funktio palauttaa luokituksen tai jos ei ole määritetty asiakirjassa luokitusta palauttaa 5 <br> `def(myfield, 1.0)`-vastaavat`if(exists(myfield),myfield,1.0)` |
| astetta | Muuntaa radiaanit asteiksi |  `deg(x)` |
| jako | `div(x,y)`jakaa luvun x y mukaan. | `div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| JAKAUMA | Palauttaa kahden vektorit, (kohdeosoite): n kolmiulotteisen tilaa etäisyys. Hakee potenssiin korotettuna kahden tai useamman, ValueSource esiintymät ja laskee kahden vektorit välillä etäisyydet. Kunkin ValueSource on oltava luku. On oltava parillinen välitetty ValueSource esiintymien ja menetelmä oletetaan, että ensimmäisen vuosipuoliskon edustavat ensimmäisestä vektorista ja toinen puoli toisen vektorin.  | `dist(2, x, y, 0, 0)`-Laskee Euclidean etäisyys between,(0,0) ja (x, y) kunkin asiakirjan. <br> `dist(1, x, y, 0, 0)`-Laskee Manhattanin (taksiin ja samalla), etäisyys (0,0) ja (x y) kunkin asiakirjan. <br> `dist(2,,x,y,z,0,0,0)`-Euclidean etäisyys (0,0,0) ja (x, y, z) kunkin asiakirjan.<br>`dist(1,x,y,z,e,f,g)`-Manhattanin etäisyys (x y, z) ja (e, f ja g) missä kukin kirjain kenttänimi. |
| on olemassa | Palauttaa totuusarvon TOSI, jos jäsen kentän on olemassa. | `exists(author)`-Palauttaa arvon TOSI, minkä tahansa tiedoston on "author"-kentän arvo.<br>`exists(query(price:5.00))`-Funktio palauttaa arvon TOSI, jos vastaa "Hinta", "5,00". |
| eksponentti | Palauttaa Eulerin 's luvun potenssiin x | `exp(x)` |
| kerroksen | Pyöristää luvun alaspäin lähimpään kokonaislukuun | `floor(x)`  <br> `floor(5.6)`-Funktio palauttaa arvon 5 |
| hypo | Palauttaa sqrt(sum(pow(x,2),pow(y,2))) ilman keskitason ylivuoto- ja alivuotopalkit | `hypo(x,y)`  <br> ` |
| Jos | Ottaa käyttöön ehdollisen funktion kyselyt. Valitse `if(test,value1,value2)` -testi tai viittaa looginen arvo tai lauseke, joka palauttaa totuusarvon (tosi tai EPÄTOSI).  `value1`on arvo, palautetaan-toiminto, jos testi antaa tulokseksi arvon TOSI. `value2`on arvo, palautetaan-toiminto, jos testi tuottaa EPÄTOSI. Lauseke voi olla funktiota, joka siirtää totuusarvoja tai jopa funktioita, jotka palauttavat numeerisia arvoja, jossa kirjainkoko arvo 0 tulkitaan EPÄTOSI tai merkkijonoja, jolloin tyhjä merkkijono tulkitaan EPÄTOSI. | `if(termfreq(cat,'electronics'),popularity,42)`– Tämä funktio etsii kunkin asiakirjasta sisältääkö termi "electronics" kate-kentässä. Jos näin on, valitse palautetaan suosion-kenttään, muuten 42 arvon. |
| Lineaarinen | Ottaa käyttöön `m*x+c` jossa m ja c ovat vakioita ja x on haluamaansa funktio. Tämä on sama kuin `sum(product(m,x),c)`, mutta tehokkaan hieman enemmän kuin yhden funktion on toteutettu. | `linear(x,m,c) linear(x,2,4)`Palauttaa`2*x+4` |
| LUONNLOG| Palauttaa määritetyn funktion luonnollinen lokiin |  `ln(x)` |
| lokitiedoston | Palauttaa lokin kantaluku 10 määritetyn funktion. | `log(x)   log(sum(x,100))` |
| kartta | Yhdistää kaikki arvot input-funktio x, jotka kuuluvat min- ja max, mukaan lukien määritetty kohde. Argumentit min- ja max on oltava vakioita. Argumentit kohde- ja oletusarvo voi olla vakioita tai funktioita. Jos x arvo ei kuulu välillä min- ja max-sitten potenssisarjan arvon tai oletusarvon palautetaan, jos määritetty 5. argumenttina. |  `map(x,min,max,target) map(x,0,0,1)`– Muuttaa kaikki arvot 0 – 1. Tämä on hyvä käsittelyn oletusarvot 0.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`– Muuttaa kaikki arvot väliltä 0 – 100-1 ja kaikki muut arvot, 1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`-Arvot väliltä 0 ja 100 x + 599-kaikki muut arvot arvoksi taajuus termi solr, jos teksti-kenttään. |
| enimmäismäärä | Palauttaa suurimman lukuarvon useita sisäkkäisiä funktioita tai vakioita, jotka on määritetty argumentteina: `max(x,y,...)`. Max-funktio voi olla myös hyötyä "bottoming" jokin muu tai kentän joitakin määritetty vakio.  Käytä `field(myfield,max)` syntaksi valitsemalla yhden moniarvoisen kentän suurimman arvon.  | `max(myfield,myotherfield,0)` |
| min | Palauttaa pienimmän numeerisen arvon useita sisäkkäisiä funktioita vakioista, jotka on määritetty argumentteina: `min(x,y,...)`. Min-funktiota voi olla myös hyötyä "ylärajan" käyttämällä vakio-funktiosta. Käytä `field(myfield,min)` syntaksi valitsemalla yhden moniarvoisen kentän pienimmän arvon. | `min(myfield,myotherfield,0)` |
| Mod | Laskee funktion y x-funktion moduluksen. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))`   |
| MS | Palauttaa argumenttien välisen eron millisekuntia. Päivämäärät ovat suhteessa Unix- tai POSIX aika kausi, keskiyöhön, tammikuussa 1, 1970 UTC-aika. Argumentit voi olla indeksoidut TrieDateField tai päivämäärä matemaattisen vakion päivämäärän perusteella nimen tai nyt. `ms()`vastaa `ms(NOW)`, numero millisekunnin viiveen jälkeen kausi. `ms(a)`Palauttaa luvun millisekunnin viiveen jälkeen, joka edustaa argumentin kausi. `ms(a,b)`Palauttaa millisekunteina, b toteutuu ennen, joka on `a - b`.| `ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| ei | Rivitetty funktion loogisesti käänteisen arvo. | `not(exists(author))`-TOSI vain, kun `exists(author)` on EPÄTOSI. |
| tai | Loogisen disjunktio-toiminnon. | `or(value1,value2)`-TOSI, jos arvo1 tai arvo2 on TOSI. |
| Pow | Korottaa kantalukua määritetyn potenssiin. `pow(x,y)`korottaa x / y potenssiin. |  `pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`-Sama kuin neliöjuuri. |
| tuotteen | Palauttaa tulon useita arvoja tai Funktiot, jotka on määritetty CSV-luettelossa. `mul(...)`voidaan myös aliaksena tämän funktion. | `product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip | Suorittaa vastavuoroiset-funktio ja `recip(x,m,a,b)` käyttöönoton `a/(m*x+b)` jossa m, a, b ovat vakioita ja x on minkä tahansa satunnaisesti KOMPLEKSI-funktio. Kun ja b ovat yhtä suuri kuin ja x > = 0, tämä funktio on suurin arvo 1, joka laskee kasvaa x. Arvon kasvattaminen ja b yhdessä liike käyrän flatter osan koko-funktion tulokset. Nämä ominaisuudet tehdä tallennustilasta Lisää viimeisimmät tiedostot, kun x on ihanteellinen koostefunktiolla `rord(datefield)`. | `recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| RAD | Muuntaa asteet radiaaneiksi | `rad(x)` |
| Tulosta| Pyöristää luvun lähimpään kokonaislukuun | `rint(x)`  <br> `rint(5.6)`-Funktio palauttaa arvon 6 |
| sin | Palauttaa annetun kulman sinin | `sin(x)` |
| SINH | Palauttaa kulman hyperbolisen sinin. | `sinh(x)` |
| asteikko | Skaalaa arvot x-funktion siten, että ne aikavälillä määritetyn minTarget ja maxTarget mukaan lukien. Käyttöönottoon traverses kaikki funktion arvot hankkiminen min- ja max, joista se valita oikean mittakaavan. Käyttöönottoon ei pysty erottamaan, kun tiedostot on poistettu tai tiedostot, joilla ei ole arvoa. Se käyttää tällaisissa tapauksissa 0.0 arvoja. Tämä tarkoittaa, että arvot ovat yleensä kaikki suurempi kuin 0,0, jos jokin voit edelleen tulokseksi 0,0 josta yhdistetään min-arvoksi. Tällaisissa tapauksissa sopivaa `map()` funktion avulla voi kiertää tämän ongelman muuttaminen 0,0 reaali alueella olevan arvon tässä esitetyllä tavalla:`scale(map(x,0,0,5),1,2)` | `scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`-Skaalaa x arvot siten, että kaikki arvot ovat väliltä 1 – 2, mukaan lukien. |
| neliöjuuri | Palauttaa määritetyn arvon tai funktion neliöjuuren. | `sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist | Voit laskea kahden merkkijonon välistä etäisyyttä. Käyttää Lucene oikeinkirjoituksen tarkistuksen StringDistance käyttöliittymän ja tukee kaikkia käytettävissä paketin käyttöotot sekä sallii sovellusten laajennus ladataan Omat kautta Solr's resurssin ominaisuuksia. strdist kestää `(string1, string2, distance measure)`. Etäisyys mittayksikön mahdolliset arvot ovat: <br>jw: Jaro Winkler<br>Muokkaa: Levenstein tai Muokkaa etäisyys<br>ngram: NGramDistance, jos määritetty, voit myös siirtää ngram kokoinen liian. Oletusarvo on 2.<br>FQN: On täydellinen luokan toteutus StringDistance-liittymän nimi. Ei-argumentti konstruktorin on oltava.|`strdist("SOLR",id,edit)` |
| Sub | Palauttaa x-ja y- `sub(x,y)`. | `sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| summa | Palauttaa useita arvoja tai Funktiot, jotka on määritetty CSV-luettelossa. `add(...)`voidaan käyttää aliaksena tämän funktion. | `sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)`|
|termfreq | Palauttaa kuinka monta kertaa termi näkyy asiakirjan-kentässä. | termfreq(Text,'memory')|
| tan | Palauttaa annetun kulman tangentin | `tan(x)` |
| TANH | Palauttaa kulman hyperbolisen tangentin. | `tanh(x)` |



## <a name="search-field-and-facet-reference"></a>Haku-kenttään ja pinta viittaus

Kun lokiin haun avulla voit etsiä tietoja, tulokset näyttää eri kentän ja muita. Kuitenkin osa tiedoista, näkyviin tulee eivät ehkä näy hyvin kuvaava. Voit tehdä seuraavat tiedot auttavat ymmärtämään tulokset.



|Kenttä|Hakutyyppi|Kuvaus|
|---|---|---|
|TenantId|Kaikki|Osion tietojen avulla|
|TimeGenerated|Kaikki|Käytettävä asema aikajanan timeselectors (Etsi ja muiden näyttöjen). Se edustaa, kun tieto luotu (yleensä-agentti). Aika ilmaistaan ISO-muodossa, ja se on aina UTC-aika. Kyseessä 'tyypit', jotka perustuvat aiemmin instrumentation (eli lokin tapahtumat), joka log merkintä/viiva/tietue on kirjautunut osoitteessa; reaaliaikainen tämä on yleensä muiden tyypit, jotka on valmistettu kautta management Pack-paketit tai pilvipohjaisia - joillekin eli suosituksia/ilmoitukset/updateagent/muille, kun tämä uusi tieto kanssa tilannevedoksen määritysten tarkoitus allekirjoitukseen tai on suositus/ilmoitus on valmistettu perustuu sitä.|
|EventID|Tapahtuman|Windowsin tapahtumalokiin EventID|
|Tapahtumaloki|Tapahtuman|Jos tapahtuma on kirjaamat Windows tapahtumaloki|
|EventLevelName|Tapahtuman|Kriittinen / varoitus / tiedot / onnistui|
|EventLevel|Tapahtuman|Kriittinen / varoitus numeroarvon / tiedot / success (Käytä EventLevelName sijaan helpompaa/helpommin luettavan kyselyille)|
|SourceSystem|Kaikki|Mistä tiedot tulevat (myös liittää' tilaa palvelu - eli Operations Manager OMS (= tiedot luodaan pilvipalvelussa), Azuren tallennustilaan (WAD tulevat tiedot) ja niin edelleen|
|Objektin nimi|PerfHourly|Windowsin suorituskyky objektinimi|
|EsiintymänNimi|PerfHourly|Windowsin suorituskyky laskuri esiintymänimi|
|CounteName|PerfHourly|Windowsin suorituskyky laskuri nimi|
|ObjectDisplayName|PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty|Näyttää suorituskyvyn sivustokokoelman sääntöä Operations Manager tai, joka löytyy toiminnallisia tiedot tai vastaan, joka on luotu ilmoituksen objektin kohteena objektin nimi|
|RootObjectName|PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty|Ylemmän tason ylätason näyttönimi (double isännöintipalvelu yhteydessä: eli ylläpitämä ylläpitämä Windows-tietokoneessa SqlInstance SqlDatabase) suorituskyvyn sivustokokoelman sääntöä Operations Manager tai, joka löytyy toiminnallisia tiedot tai vastaan, joka on luotu ilmoituksen objektin kohteena objektin|
|Tietokoneen|Useimpien|Tietokonenimi, johon kuuluu tietojen|
|Laitenimi|ProtectionStatus|Tietokonenimi tiedot kuuluu (sama kuin "Tietokone")|
|DetectionId|ProtectionStatus| |
|ThreatStatusRank|ProtectionStatus|Uhkien tila sija on numeerinen esittäminen uhkien tila ja samalla HTTP-vastauksen koodit, että olet poistunut puutteiden lukujen väliltä. (joka on miksi viruksia ei on 150 eikä 100 tai 0) niin, että olemme on käytössä, voit lisätä uuden hyötyä tilaa. Yhteydessä rollup uhkien tilan ja tilaa haluat näyttää huonoin tilaan, jossa tietokone on ollut valitun ajanjakson aikana. Käytämme numerot järjestetään eri tilat, niin että etsiä tietueen suurimman luvun.|
|ThreatStatus|ProtectionStatus|Kuvaus ThreatStatus, karttoja ja ThreatStatusRank 1:1|
|TypeofProtection|ProtectionStatus|Haittaohjelmien tuote, joka on havaitsi tietokoneessa: ei mitään, Microsoft haittaohjelmien poisto työkalu ja Forefront|
|ScanDate|ProtectionStatus| |
|SourceHealthServiceId|ProtectionStatus, RequiredUpdate|Tämän tietokoneen agentti HealthService tunnus|
|HealthServiceId|Useimpien|Tämän tietokoneen agentti HealthService tunnus|
|ManagementGroupName|Useimpien|Operations Manager liitetty tekijöiden hallinta ryhmän nimi Muussa tapauksessa teksti on null tai tyhjä|
|Objektilaji|ConfigurationObject|Tyyppi (kuten Operations Manager management pack "tyyppi" / luokan) tämän objektin löytää Log Analytics kokoonpanon arviointi|
|UpdateTitle|RequiredUpdate|Päivityksen, joka löytynyt ei ole asennettu nimi|
|PublishDate|RequiredUpdate|Kun päivitys on julkaistu Microsoft Updaten?|
|Palvelin|RequiredUpdate|Tietokonenimi tiedot kuuluu (sama kuin "Tietokone")|
|Tuotteen|RequiredUpdate|Tuote, joka koskee päivitys|
|UpdateClassification|RequiredUpdate|Tyyppi-päivityksen (päivityskokoelma, service Pack-paketin jne.)|
|KBID|RequiredUpdate|Knowledge Base-artikkelin tunnus, joka kuvaa tämän parhaiden käytäntöjen tai päivitys|
|WorkflowName|ConfigurationAlert|Säännön tai näyttö, joka tuottaa ilmoituksen nimi|
|Vakavuus|ConfigurationAlert|Ilmoituksen vakavuus|
|Priority (prioriteetti)|ConfigurationAlert|Prioriteetti ilmoitus|
|IsMonitorAlert|ConfigurationAlert|Tämä ilmoitus on luotu näyttö (tosi) tai säännön (EPÄTOSI)|
|AlertParameters|ConfigurationAlert|XML-loki Analytics-ilmoituksen parametrit|
|Konteksti|ConfigurationAlert|Lokitiedoston Analytics-ilmoitus "kontekstin' XML|
|Kuormituksen|ConfigurationAlert|Tekniikka tai "työmäärää", joka viittaa hälytys|
|AdvisorWorkload|Suositus|Tekniikka tai "työmäärää", joka viittaa suositus|
|Kuvaus|ConfigurationAlert|Ilmoituksen kuvaus (lyhyt)|
|DaysSinceLastUpdate|UpdateAgent|Kuinka monta päivää (suhteessa 'TimeGenerated' tietueen) Tämä agentti asentaa päivitys WSUS/Microsoft Update-sivustosta?|
|DaysSinceLastUpdateBucket|UpdateAgent|Perusteella DaysSinceLastUpdate luokittelu-ja kuinka kauan sitten on tietokoneen "aika uudelleenkäytettävät viimeksi asennettu päivitys WSUS/Microsoft Update-sivustosta|
|AutomaticUpdateEnabled|UpdateAgent|On automaattinen päivitys tarkistuksen käyttöön tai pois käytöstä Tämä agentti?
|AutomaticUpdateValue|UpdateAgent|Tarkistaa automaattisten päivitysten automaattinen lataaminen ja asentaminen, lataa vain ja Tarkista vain määrittäminen|
|WindowsUpdateAgentVersion|UpdateAgent|Microsoft Update-agentti versionumero|
|WSUSServer|UpdateAgent|WSUS palvelimen update-agentti on kohdistamisen?|
|OSVersion|UpdateAgent|Tämä update-agentti ole käytössä käyttöjärjestelmän versio|
|Nimi|Suositus, ConfigurationObjectProperty|Suositus tai lokin Analytics kokoonpanon arviointi ominaisuuden nimi nimi ja otsikko|
|Arvo|ConfigurationObjectProperty|Lokitiedoston Analytics kokoonpanon arviointi-ominaisuuden arvon|
|KBLink|Suositus|Knowledge Base-artikkeli, joka kuvaa tämän parhaiden käytäntöjen tai päivitys URL-osoite|
|RecommendationStatus|Suositus|Suosituksia ovat joidenkin tiedostotyyppien, joiden tietueet "päivittää', eikä vain lisätään hakuindeksiin. Tämä tila muuttuu, onko suositus aktiivinen tai avaa tai, jos lokiin Analytics havaitsee, että se on ratkaistu.|
|RenderedDescription|Tapahtuman|Windowsin tapahtumalokiin hahmontamisen kuvaus (täyttää parametreilla uudelleenkäytetty teksti)|
|ParameterXml|Tapahtuman|Windowsin tapahtumalokiin (joka näkyy Tapahtumienvalvonta) "tiedot"-osassa parametreilla XML|
|EventData|Tapahtuman|XML Windowsin tapahtumalokiin (joka näkyy Tapahtumienvalvonta) koko tiedot-osa|
|Lähde|Tapahtuman|Tapahtuman luoneen tapahtumalokin lähde|
|EventCategory|Tapahtuman|Luokan tapahtuman suoraan Windowsin tapahtumalokista|
|Käyttäjänimi|Tapahtuman|Käyttäjänimi Windowsin tapahtumalokiin (yleensä NT AUTHORITY\LOCALSYSTEM)|
|SampleValue|PerfHourly|Suorituskyvyn laskuri tunnin välein yhteenlaskeminen keskiarvo|
|Min|PerfHourly|Tunnin välein suorituskyvyn laskuri-kooste tunnin välein aikavälin pienin arvo|
|Enimmäismäärä|PerfHourly|Tunnin välein suorituskyvyn laskuri-kooste tunnin välein aikavälin suurimman arvon|
|Percentile95|PerfHourly|Tunnin välein suorituskyvyn laskuri-kooste tunnin välein aikavälin 95. prosenttipisteen arvo|
|SampleCount|PerfHourly|Kuinka monta raaka' suorituskyvyn laskuri näytteiden käytettiin tuottamaan tunnin välein kooste tietuetta|
|Uhkien|ProtectionStatus|Löytyi haittaohjelman nimi|
|StorageAccount|W3CIISLog|Azure-tallennustilan tilin lokin lukeminen|
|AzureDeploymentID|W3CIISLog|Azure käyttöönoton pilvipalvelussa lokin tunnus kuuluu.|
|Rooli|W3CIISLog|Kirjaudu kuuluu Azure-pilvipalvelussa rooli|
|RoleInstance|W3CIISLog|Azure-roolin, joka kuuluu lokin RoleInstance|
|sSiteName|W3CIISLog|IIS-sivusto, johon kuuluu lokiin (metakannan merkintätapaa); alkuperäisen lokin s nimi-kenttä|
|sComputerName|W3CIISLog|Alkuperäisen lokin s-tietokoneen nimi-kenttä|
|sIP|W3CIISLog|Palvelimen IP-osoite HTTP pyyntö on osoitettu. Alkuperäisen lokin s-ip-kenttä|
|csMethod|W3CIISLog|HTTP-menetelmä (GET-kirjaa-jne) avulla HTTP-pyynnössä asiakas. Alkuperäisen lokin cs-menetelmä|
|cIP|W3CIISLog|Asiakkaan IP-osoite HTTP-pyynnön peräisin. Alkuperäisen lokin c-ip-kenttä|
|csUserAgent|W3CIISLog|HTTP-käyttäjäagentti tietueeksi asiakkaan (selaimessa tai muussa). Cs-käyttäjäagentti alkuperäisen lokiin|
|scStatus|W3CIISLog|HTTP Status code (200/403/500/jne) palvelimen palauttama asiakkaalle. Alkuperäisen lokin cs-tila|
|TimeTaken|W3CIISLog|Kuinka pitkään (millisekunteina), joka suorittamiseen kuluneiden pyynnön. Alkuperäisen lokin timetaken-kenttä|
|csUriStem|W3CIISLog|Suhteellinen Uri (ilman isännöidä osoite, kuten "/ Etsi"), joka on pyydetty. Alkuperäisen lokin cs uristem-kenttä|
|csUriQuery|W3CIISLog|URI-kysely. URI-kyselyt ovat tarpeen vain dynaamisia sivuja, kuten ASP-sivut, joten tässä kentässä on yleensä yhdysmerkin staattinen sivuille.|
|Urheilu|W3CIISLog|Palvelimen portti, HTTP-pyyntö lähetettiin (ja IIS seuraa, koska se noudettu sitä)|
|csUserName|W3CIISLog|Todennetun käyttäjänimi, jos pyyntö on todennettu ja eivät nimettömänä|
|csVersion|W3CIISLog|HTTP-protokolla-versio, jolla pyynnössä (eli "HTTP / 1.1')|
|csCookie|W3CIISLog|Evästeiden tiedot|
|csReferer|W3CIISLog|Sivustoon, jossa käyttäjä viimeksi käynyt. Tämän sivuston linkki nykyiseen sivustoon.|
|csHost|W3CIISLog|Toimialuenimi (kuten www.mysite.com), joka on pyydetty|
|scSubStatus|W3CIISLog|Alitila virhekoodi|
|scWin32Status|W3CIISLog|Windows-tilakoodi|
|csBytes|W3CIISLog|Lähetetään pyyntö asiakaskoneesta palvelimeen tavua|
|scBytes|W3CIISLog|Palautetaan takaisin palvelimen vastausta asiakkaan tavua|
|ConfigChangeType|ConfigurationChange|Muuta tyyppi (WindowsServices / ohjelmiston / tms)|
|ChangeCategory|ConfigurationChange|Luokan muutoksen (muokattu / lisätty tai poistettu)|
|SoftwareType|ConfigurationChange|Tyyppi-ohjelmiston (päivityksen / sovellus)|
|SoftwareName|ConfigurationChange|Nimi (koskee vain ohjelmiston muutokset)-ohjelmiston|
|Publisherin|ConfigurationChange|Myyjä, joka julkaisee ohjelmiston (koskee vain ohjelmiston muutokset)|
|SvcChangeType|ConfigurationChange|Muutoksen, jota on käytetty Windows-palvelun (osavaltio / StartupType / polun / ServiceAccount) - Windows-palvelun muutokset koskevat vain|
|SvcDisplayName|ConfigurationChange|Näyttönimi palvelu, joka on muutettu.|
|SvcName|ConfigurationChange|Palvelu, on muutettu|
|SvcState|ConfigurationChange|Uusi-palvelun (nykyisen) tilan|
|SvcPreviousState|ConfigurationChange|Edellinen tunnetut (käytettävissä vain, jos palvelun tilan muuttanut)-palvelun tilan|
|SvcStartupType|ConfigurationChange|Palvelun käynnistystapa|
|SvcPreviousStartupType|ConfigurationChange|Edellisen palvelun käynnistystapa (käytettävissä vain, jos palvelun käynnistystapa muuttanut)|
|SvcAccount|ConfigurationChange|Palvelutili|
|SvcPreviousAccount|ConfigurationChange|Edellisen palvelutilin (käytettävissä vain, jos palvelutilin muuttanut)|
|SvcPath|ConfigurationChange|Windows-palvelun tiedoston polku|
|SvcPreviousPath|ConfigurationChange|Edellinen (käytettävissä vain, jos se on muutettu) Windows-palvelun suoritettavan tiedoston polku|
|SvcDescription|ConfigurationChange|Palvelun kuvaus|
|Edellinen|ConfigurationChange|Tilaan tämän ohjelmiston (asennettu / ei ole asennettu / edellinen versio)|
|Nykyinen|ConfigurationChange|Uusimman tilan tämän ohjelmiston (asennettu / ei ole asennettu / nykyinen versio)|

## <a name="next-steps"></a>Seuraavat vaiheet
Saat lisätietoja lokin haut:

- Tutustu [log haut](log-analytics-log-searches.md) haluat tarkastella yksityiskohtaisia tietoja keräämien ratkaisuja.
- [Mukautetut kentät Log Analytics](log-analytics-custom-fields.md) avulla voit laajentaa log haut.
