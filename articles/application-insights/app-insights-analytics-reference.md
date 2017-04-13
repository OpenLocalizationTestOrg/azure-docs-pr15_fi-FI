<properties 
    pageTitle="Viittaus Analytics-sovelluksen tiedot | Microsoft Azure" 
    description="Viittauksen lauseet Analytics-sovelluksen tiedot tehokas haku-työkalu. " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="awills"/>

# <a name="reference-for-analytics"></a>Analytics-viittaus

[Analyysin](app-insights-analytics.md) on tehokas haku-toiminnon [Hakemuksen tiedot](app-insights-overview.md). Nämä sivut kuvaavat Analytics-kyselykieltä.

> [AZURE.NOTE] Jos sovellus ei ole lähettää tiedot sovelluksen havainnollistamisen vielä [Kokeile Analytics-Simuloitu tietojamme](https://analytics.applicationinsights.io/demo) .

## <a name="index"></a>Indeksi


**Anna** [Anna](#let-clause)


**Kyselyjen ja operaattorit.** [Laske](#count-operator)  |  [arvioi](#evaluate-operator) | [laajentaa](#extend-operator) | [Liity](#join-operator) | [raja](#limit-operator) | [mvexpand](#mvexpand-operator) | [jäsentää](#parse-operator) | [projektin](#project-operator) | [projektin poissa](#project-away-operator) | [alueen](#range-operator) | [vähentää](#reduce-operator) | [hahmonnetaan direktiivin](#render-directive) | [rajoittaa lauseen](#restrict-clause) | [Lajittele](#sort-operator) | [yhteenvedon](#summarize-operator) | [kestää](#take-operator) | [ylä](#top-operator) | [top sisäkkäinen](#top-nested-operator) | [unionin](#union-operator) | [kohtaa, johon](#where-operator) | [where-](#where-in-operator)

**Koosteet** [Mikä tahansa](#any)  |  [argmax](#argmax) | [argmin](#argmin) | [keskiarvo](#avg) | [buildschema](#buildschema) | [Laske](#count) | [Laske.Jos](#countif) | [TLaske](#dcount) | [dcountif](#dcountif) | [makelist](#makelist) | [makeset](#makeset) | [max](#max) | [min](#min) | [prosenttipisteen](#percentile) | [arvona](#percentiles) | [percentilesw](#percentilesw) | [percentilew](#percentilew) | [keskihajonta](#stdev) | [Summa](#sum) | [varianssi](#variance)

**Skalaareja** [Totuusarvo literaalien](#boolean-literals)  |  [Totuusarvo-operaattorit](#boolean-operators) | [esimerkiksi lähetyksiin](#casts) | [Skalaarinen vertailuja](#scalar-comparisons) | [gettype](#gettype) | [hash](#hash) | [iff](#iff) | [isnotnull](#isnotnull) | [isnull](#isnull) | [notnull](#notnull) | [toscalar](#toscalar)

**Luvut** [Aritmeettiset operaattorit](#arithmetic-operators)  |  [Numeerinen literaalien](#numeric-literals) | [itseisarvo](#abs) | [bin](#bin) | [eksponentti](#exp) | [kerroksen](#floor) | [gamma](#gamma) | [lokin](#log) | [SATUNNAISLUKU](#rand) | [neliöjuuri](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

**Päivämäärä ja aika** [Päivämäärä ja kellonaika-lausekkeiden](#date-and-time-expressions)  |  [Päivämäärän ja kellonajan literaalien](#date-and-time-literals) | [sitten](#ago) | [datepart](#datepart) | [dayofmonth](#dayofmonth) | [: n dayofweek](#dayofweek) | [dayofyear](#dayofyear) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth) | [getyear](#getyear) | [nyt](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

**Merkkijono** [GUID](#guids)  |  [Salatut merkkijonon literaalien](#obfuscated-string-literals) | [Merkkijonon literaalien](#string-literals) | [merkkijonojen vertailussa](#string-comparisons) | [countof](#countof) | [Pura](#extract) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty)| [parseurl](#parseurl) | [Korvaa](#replace) | [jakaa](#split) | [strcat](#strcat) | [strlen](#strlen) | [alimerkkijono](#substring) | [tolower](#tolower) | [toupper](#toupper)

**Matriisien, objektien ja dynaaminen** [Matriisi ja objektien literaalien](#array-and-object-literals)  |  [Dynamic objekti-funktioiden](#dynamic-object-functions) | [lähettäville lauseet Dynamic objektien](#dynamic-objects-in-let-clauses) | [JSON polku lausekkeiden](#json-path-expressions) | [nimet](#names) | [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [alueen](#range) | [todynamic](#todynamic) | [treepath](#treepath)



## <a name="let"></a>Anna

### <a name="let-clause"></a>Anna lause

**Taulukko, anna - taulukon nimeäminen**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skalaari antaa - arvon nimeäminen**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda antaa - funktion nimeäminen**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

    let us_date = (t:datetime) { strcat(getmonth(t),'/',dayofmonth(t),'/',getyear(t)) }; 
    requests | summarize count() by bin(timestamp, 1d) | project count_, day=us_date(timestamp)

Anna lause sitoo taulukkomuotoinen tuloksen, skalaariarvon tai funktion [nimi](#names) . Lauseessa on etuliite kyselyyn ja sidonta alue on kysely. (Salli ei ole mahdollisuutta nimi asioita, joita käytetään myöhemmin istunnon.)

**Syntaksi**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

    let name = (parameterName : type [, ...]) { scalar_expression }; query

* *tyyppi:* `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`,[`dynamic`](#dynamic-type)
* *plain_query:* Kysely, joka ei salli-lauseen etuliite.

**Esimerkkejä**

    let rows = (n:long) { range steps from 1 to n step 1 };
    rows(10) | ...


Liity itse:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
  	| project start = timestamp, session_id
  	| join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
  	| extend duration = stop - start 


## <a name="queries-and-operators"></a>Kyselyjen ja operaattorit.

Kyselyn päälle oman telemetriatietojen koostuu viittauksen lähde-muodossa, suodattimien putkijohto perään. Esimerkki:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Kukin suodatin etuliite putkien merkin `|` on *operaattori*, jotkin parametreilla esiintymä. Operaattori syöte on taulukko, joka on saatu edellisen putkijohto. Useimmissa tapauksissa parametreja ole [skalaarinen lausekkeiden](#scalars) syötteen sarakkeet. Joissakin tapauksissa parametrit ovat syötteen sarakkeiden nimet ja joissakin tapauksissa parametri on toisessa taulukossa. Kyselyn tulos on aina taulukon, vaikka se on vain yksi sarake ja yksi rivi.

Kyselyt voi olla yksirivinen sivunvaihtoja, mutta tyhjän rivin päättyi. Ne voivat sisältää kommentteja välillä `//` ja rivin loppuun.

Kyselyn voi etuliite mukaan vähintään yksi [antaa lausekkeita](#let-clause), jotka määrittävät Skalaareja, taulukoita tai funktioita, joita voidaan käyttää kyselyä.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T`käytetään kyselyn esimerkeissä osoittamaan edellisessä myyntijakso tai lähde-taulukossa.
> 

### <a name="count-operator"></a>Laske-operaattori

`count` Operaattori palauttaa tietueita (rivejä) määrän syötteen tietuejoukon.

**Syntaksi**

    T | count

**Argumentit**

* *T*: taulukkomuotoinen tiedot, joiden tietueet ovat laskettavat.

**Palauttaa**

Tämä funktio palauttaa taulukon, jossa on yksi tietue ja tyyppisellä `long`. Vain solun arvo on *T*tietueiden määrän. 

**Esimerkki**

```AIQL
requests | count
```

### <a name="evaluate-operator"></a>Arvioi operaattori

`evaluate`on tunniste-järjestelmä, joka sallii tietynlaista algoritmeista, joka lisätään kyselyihin.

`evaluate`on oltava viimeisen operaattorin käyttäminen kyselyn putkijohto (lukuun ottamatta mahdollista `render`). Se on tule funktiota tekstissä.

[Arvioi autocluster](#evaluate-autocluster) | [arvioi kori](#evaluate-basket) | [arvioi diffpatterns](#evaluate-diffpatterns) | [extractcolumns arvioiminen](#evaluate-extractcolumns)

#### <a name="evaluate-autocluster"></a>Arvioi autocluster

     T | evaluate autocluster()

AutoCluster etsii Yleiset mallit erillinen määritteiden (mitat) tiedot ja vähentää (onko 100 tai 100 k rivejä) alkuperäisen kyselyn tulosten määrä on pieni, kuvioita. AutoCluster on suunniteltu helpottamaan analysoida virheitä (kuten poikkeukset, kaatuu), mutta kaikki suodatetut tietojoukon mahdollisesti käsitellä. 

**Syntaksi**

    T | evaluate autocluster( arguments )

**Palauttaa**

AutoCluster palauttaa kuviot, jotka siepata osiin jaettu yleisiä arvoilla tiedot yli useiden erillisten määritteet (yleensä pieni). Jokaisessa kuviossa edustaa rivi tuloksissa. 

Kahden ensimmäisen sarakkeen on Laske- ja ulos alkuperäisen kyselyn rivit, jotka ovat siepatun kuvion prosentteina. Jäljellä olevat sarakkeet ovat alkuperäisen kyselystä ja niiden arvo on joko tietyn arvon sarakkeesta tai "*" eli muuttujien arvoihin. 

Huomaa, että kuviot eivät ole erillisten: voivat näkyä päällekkäin ja ei yleensä kattaa kaikki alkuperäisen rivit. Joitakin rivejä ei saa olla jokin kuvio.

**Vihjeitä**

* Käytä `where` ja `project` -syötteen putkien vähentää tiedot vain mitä haluat muuttaa.
* Kun olet löytänyt kiinnostavat rivi, voit siirtyä siihen tarkemmin lisäämällä sen tiettyyn arvot oman `where` suodatin.

**Argumentit (kaikki valinnainen)**

* `output=all | values | minimal` 

    Muoto, tulokset. Tulokset näkyvät aina sarakkeiden määrä ja prosentti. 

 * `all`-syöte-sarakkeet ovat tulostus
 * `values`– suodattaa vain sarakkeiden "*" tulokset
 * `minimal`– myös suodattaa sarakkeet, jotka ovat samat alkuperäisen kyselyn kaikki rivit. 


* `min_percent=`*Kaksinkertainen* (oletus: 1)

    Prosenttia soveltamisala luodut rivit.

    Esimerkki:`T | evaluate autocluster("min_percent=5.5")`


* `num_seeds=`*kokonaisluku* (oletus: 25) 

    Siementen lukumäärä määrittää ensimmäisen paikallinen haku algoritmin pisteiden lukumäärä. Joissakin tapauksissa tiedot rakenteen mukaan lisääntyvien siemenet määrä kasvaa tulokset läpi Parannettu haku välilyöntiä hitaammin kyselyn tarjoa numero (tai laatu). Num_seeds-argumentti on supistumisena tulokset molempiin suuntiin, jotta pienenevillä alla 5 saavuttaa vähäisiä suorituskykyparannukset ja lisääntyvien yli 50 harvoin Luo muita kuviot.

    Esimerkki:`T | evaluate autocluster("num_seeds=50")`


* `size_weight=`*0 < kaksinkertainen < 1*+ (oletus: 0,5)

    Tutustutaan joitakin hallintaoikeutta tasapaino Yleinen (suuri kattavuus) ja kuvaava (monta jaetun arvot). Kasvava size_weight yleensä vähentää kuvioiden ja jokaisessa kuviossa yleensä peittää koko prosentteina. Pienentää size_weight yleensä tuottaa tiettyihin kuvioita jaettuja arvoja ja pienempi prosenttiluku kattavuuden. Näytä lisäasetukset kaava on kuvaava pisteet size_weight ja 1 size_weight nimellä paksuuksia ja normitettu yleinen tulos välillä painotettu Geometrisen keskiarvon. 

    Esimerkki:`T | evaluate autocluster("size_weight=0.8")`


* `weight_column=`*column_name*

    Arvioi syötteen määritetyn leveyden mukaan kullekin riville (oletusarvoisesti kullakin rivillä on '1' paksuus)-sarakkeen leveyden yleisiä käyttö on otettava huomioon tilin esimerkkejä tai bucketing/kooste tiedoista, jotka on jo upotettu kullekin riville.

    Esimerkki:`T | evaluate autocluster("weight_column=sample_Count")` 



#### <a name="evaluate-basket"></a>Arvioi kori

     T | evaluate basket()

Kori etsii kaikki usein käytetyt kuviot erillinen määritteiden (mitat) tiedot ja palauttaa usein käytetyt kuviot, korkojakso kynnysarvo välitetty alkuperäinen kysely. Kori Etsi kaikki usein käytetyt kuviot tietojen taata, mutta ei välttämättä on polynominen suorituksen aikana. Kyselyn suorituksen on lineaarinen rivien määrän, mutta joskus voi olla eksponentiaalisen (mitat) sarakkeiden määrä. Kori perustuu alun perin kehittämän kori analyysi mahdollisista Apriori algoritmin. 

**Palauttaa**

Yli määritetyn nimittäjä (oletus 0,05) tapahtumat näkyvät kaikki kuviot.

**Argumentit (kaikki valinnainen)**


* `threshold=`*0.015 < kaksinkertainen < 1* (oletus: 0,05) 

    Määrittää pidetään toistuvat rivit mahdollisimman vähän suhde (pienempi suhde kuvioita ei palautetaan).

    Esimerkki:`T | evaluate basket("threshold=0.02")`


* `weight_column=`*column_name*

    Arvioi syötteen määritetyn leveyden mukaan kullekin riville (oletusarvoisesti kullakin rivillä on '1' paksuus)-sarakkeen leveyden yleisiä käyttö on otettava huomioon tilin esimerkkejä tai bucketing/kooste tiedoista, jotka on jo upotettu kullekin riville.

    Esimerkki: T | Arvioi basket("weight_column=sample_Count")


* `max_dims=`*1 < int* (oletusarvo: 5)

    Määrittää kori, voit pienentää kyselyn suorituksen oletusarvoisesti rajoitettu epäkorreloivaa mitat suurin numero.


* `output=minimize` | `all` 

    Muoto, tulokset. Tulokset näkyvät aina sarakkeiden määrä ja prosentti.

 * `minimize`– suodattaa vain sarakkeiden "*" tuloksissa.
 * `all`-syöte-sarakkeet ovat tulos.




#### <a name="evaluate-diffpatterns"></a>Arvioi diffpatterns

     requests | evaluate diffpatterns("split=success")

Diffpatterns vertaa kahden tietosarjan, on sama rakenne ja löytää kuviot erillinen attribuuteista (mitat), jotka vapausasteiden kaksi tietojoukkojen väliset erot. Diffpatterns kehitetty avulla analysoida virheitä (kuten tekijä vertaaminen siitä tietyn ajanjakson ei-virheiden), mutta voit etsiä mahdollisesti minkä tahansa kahden tietosarjan saman rakenteen erot. 

**Syntaksi**

`T | evaluate diffpatterns("split=`*BinaryColumn*`" [, arguments] )`

**Palauttaa**

Diffpatterns palauttaa (yleensä pieni) kuviot, joka tallentaa tiedot kahden eri osiin (eli sieppaus ensimmäisen tietojoukon rivien suuri prosentteina ja pienen prosenttiosuuden rivit toisen määrittäminen kuvion). Jokaisessa kuviossa edustaa rivi tuloksissa.

Neljä ensimmäistä saraketta ovat Laske- ja ulos alkuperäisen kyselyn rivit, jotka ovat siepatun kussakin joukossa kuvion prosentteina, viidennen sarakkeen eroavat (suora prosenttiyksiköt) kahden välillä. Jäljellä olevat sarakkeet ovat alkuperäisen kyselystä ja niiden arvo on joko tietyn arvon sarakkeesta tai * eli muuttujien arvoihin. 

Huomaa, että kuviot eivät ole eri: voivat näkyä päällekkäin ja yleensä kata kaikki alkuperäisen rivit. Joitakin rivejä ei saa olla jokin kuvio.

**Vihjeitä**

* Käytä where ja projektin syötteen putkien vähentää tiedot vain mitä haluat muuttaa.

* Kun olet löytänyt kiinnostavat rivin, voit siirtyä edelleen sen lisäämällä sen tiettyjen arvojen lisääminen mihin suodattaa.

**Argumentit**

* `split=`*sarakkeen nimi* (pakollinen)

    Sarakkeen on oltava täsmälleen kaksi arvoa. Tarvittaessa näiden sarakkeen luominen:

    `requests | extend fault = toint(resultCode) >= 500` <br/>
    `| evaluate diffpatterns("split=fault")`

* `target=`*merkkijono*

    Kertoo, joka on suurempi prosenttiluku kohde tietojoukon voit etsiä vain algoritmin kuviot, kohde on oltava jokin näistä kahdesta arvosta Jaa-sarakkeen.

    `requests | evaluate diffpatterns("split=success", "target=false")`

* `threshold=`*0.015 < kaksinkertainen < 1* (oletus: 0,05) 

    Määrittää kahden mahdollisimman vähän kuvio (suhde) erotus.

    `requests | evaluate diffpatterns("split=success", "threshold=0.04")`

* `output=minimize | all`

    Muoto, tulokset. Tulokset näkyvät aina sarakkeiden määrä ja prosentti. 

 * `minimize`– suodattaa vain sarakkeiden "*" tulokset
 * `all`-syöte-sarakkeet ovat tulostus

* `weight_column=`*column_name*

    Arvioi syötteen määritetyn leveyden mukaan kullekin riville (oletusarvoisesti jokaisella rivillä on '1' paksuus). Yleisen käytön sarakkeen leveys on otettava huomioon esimerkkejä tai bucketing/yhteenlaskeminen tiedot, jotka on jo upotettu kullekin riville.

    `requests | evaluate autocluster("weight_column=itemCount")`






#### <a name="evaluate-extractcolumns"></a>Arvioi extractcolumns

     exceptions | take 1000 | evaluate extractcolumns("details=json") 

Extractcolumns käytetään täydentää taulukko, jossa on useita yksinkertaisia sarakkeet, jotka ovat dynaamisesti poimitun ulos (Läpikuultava) rakenteellisia sarakkeista niiden lajin perusteella. Tukee tällä hetkellä vain json sarakkeita sekä dynaamisesti ja jsons Sarjatoiminto merkkijono.


* `max_columns=`*kokonaisluku* (oletus: 10) 

    Uusi lisätty sarakkeiden määrä on dynaaminen ja se voi olla erittäin suuri (todella on eri näppäimet kaikkien json tietueiden määrän) niin, että on rajoittaa. Uudet sarakkeet on lajiteltu laskevaan järjestykseen perusteella niiden korkojakso ja ylöspäin max_columns on lisätty taulukkoon.

    `T | evaluate extractcolumns("json_column_name=json", "max_columns=30")`


* `min_percent=`*Kaksinkertainen* (oletus: 10.0) 

    Toinen tapa rajoittaa uusien sarakkeiden ohittaa sarakkeet, joiden korkojakso on pienempi kuin min_percent.

    `T | evaluate extractcolumns("json_column_name=json", "min_percent=60")`


* `add_prefix=`*bool* (oletus: TOSI) 

    Jos arvo on tosi monimutkaisia sarakkeen nimi lisätään etuliitteenä poimitun sarakkeiden nimet.


* `prefix_delimiter=`*merkkijono* (oletus: "_") 

    Jos add_prefix = true tämä parametri määrittää erotin, jota käytetään käyttäisit uusien sarakkeiden nimet.

    `T | evaluate extractcolumns("json_column_name=json",` <br/>
    `"add_prefix=true", "prefix_delimiter=@")`


* `keep_original=`*bool* (oletus: EPÄTOSI) 

    Jos arvo on tosi (json) alkuperäiset sarakkeet säilytetään tulostusalue.


* `output=query | table` 

    Muoto, tulokset. 

 * `table`-Tulos on saman taulukon siten miinus syötteen määritettyjen sarakkeiden plus uutta saraketta, jotka olivat poimittujen syötteen sarakkeet.
 * `query`-Tulos on merkkijono, joka teet saat tuloksen taulukoksi kyselyn. 




### <a name="extend-operator"></a>Laajenna operaattori

     T | extend duration = stopTime - startTime

Liitä yhden tai usean lasketun sarakkeen taulukkoon. 


**Syntaksi**

    T | extend ColumnName = Expression [, ...]

**Argumentit**

* *T:* Syötetaulukko.
* *ColumnName:* Voit lisätä sarakkeita nimi. [Nimien](#names) kirjainkoko on merkitsevä ja voi olla kirjaimia, numeroita tai "_"-merkkejä. Käytä `['...']` tai `["..."]` , tarjouksen avainsanat tai nimien ja muiden näppäimistöstä puuttuvien merkkien.
* *Lauseke:* Laskenta aiemmin sarakkeiden yli.

**Palauttaa**

Syötteen taulukko, jonka määritetyn sarakkeita kopio.

**Vihjeitä**

* Käytä [`project`](#project-operator) , jos haluat pudota tai nimetä uudelleen joitakin sarakkeita.
* Älä käytä `extend` vain, jos haluat saada lyhyempi nimi, jota käytetään pitkän lausekkeessa. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Alkuperäisen taulukon sarakkeet on indeksoitu; uusi nimi määrittää toisen sarakkeen, joka ei ole indeksoitu, jotta kysely on todennäköisesti hitaammin.

**Esimerkki**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### <a name="join-operator"></a>JOIN-operaattori

    Table1 | join (Table2) on CommonColumn

Yhdistää kahden taulukon rivien vastaavat arvot sarakkeen mukaan.


**Syntaksi**

    Table1 | join [kind=Kind] (Table2) on CommonColumn [, ...]

**Argumentit**

* *Taulukko1* - "vasen puoli' liitoksen.
* *Taulukko2* - liitoksen oikealla puolella. Voi olla sisäkkäisiä kyselylauseke, joka siirtää taulukon.
* *CommonColumn* - sarake, jossa on saman niminen kummassakin taulukossa.
* *Laji* - määrittää, miten kahden taulukon rivit ovat tietoriveihin.

**Palauttaa**

Taulukko, jossa on:

* Sarake, kaikki taulukot, mukaan lukien vastaavat näppäimet jokaisessa sarakkeessa. Sarakkeiden oikealla puolella nimetään automaattisesti, jos nimi ristiriidassa.
* Rivin kaikki ilmentymät syötteen taulukoiden välille. Vastaa yhdestä taulukosta, jossa on sama arvo kaikki valitun rivin `on` kenttiä toisesta taulukosta rivi. 

* `Kind`Määrittämätön

    Jokaisen arvon vastaa vain yhden rivin vasemmalla puolella `on` avain. Tulos on rivin kunkin vastineen tätä riviä, jonka rivit oikealta lähtien.

* `Kind=inner`
 
     Ei rivin vastaavat rivit jokaisen yhdistelmän tulosteen vasemmalta ja oikealta.

* `kind=leftouter`(or `kind=rightouter` or `kind=fullouter`)

     Sisemmän vastineita on rivin kunkin rivin vasemmalle (ja/tai oikealle), vaikka se se ei vastaa. Tässä tapauksessa Vastineettomien tietueiden tulosteen solut sisältävät Null-arvoja.

* `kind=leftanti`

     Palauttaa kaikki tietueet, jotka eivät vastaa oikealta vasemmalla puolella. Tulostaulukko on vain vasemmalla puolella taulukkoon lisättävät sarakkeet. 
 
Jos määritettynä on useita rivejä, jossa on samat arvot kyseisille kentille, saat rivien kaikki yhdistelmät.

**Vihjeitä**

Saat parhaan suorituskyvyn:

* Käytä `where` ja `project` vähentää haluamasi määrä rivejä ja sarakkeita syötteen taulukoita, ennen kuin `join`. 
* Jos taulukolla on aina pienempi kuin toisen, määritä se liitoksen vasemmalla puolella (siirtoa putkissa).
* Liity vastaavuuden sarakkeissa on oltava sama nimi. Käytä project-operaattoria tarvittaessa voit nimetä sarakkeen jollakin taulukot.

**Esimerkki**

Hae laajennetut toimintoja lokista, mitkä joissakin tapahtumat Merkitse alku ja tehtävän loppuun. 

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
  	| where Name == "Start"
  	| project Name, City, ActivityId, StartTime=timestamp
  	| join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
  	| project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```


### <a name="limit-operator"></a>raja-operaattori

     T | limit 5

Palauttaa ylöspäin määritetty rivimäärän input-taulukosta. Ei ei takaa, mitkä tietueet palautetaan. (Palauttaa lisättävät tietueet [`top`](#top-operator).)

**Alias**`take`

**Syntaksi**

    T | limit NumberOfRows


**Vihjeitä**

`Take`on yksinkertainen ja tehokas tapa näyttää tulokset mallitekstin, kun työskentelet vuorovaikutteisesti. Huomaa, että se ei takaa tuota tietyn rivejä tai tuottamaan niitä satunnaisessa järjestyksessä.

On implisiittisesti raja, asiakkaan palauttamien rivien määrän, vaikka et käyttäisi `take`. Nosta rajoitusta, käytä `notruncation` asiakkaan pyynnön vaihtoehto.



### <a name="mvexpand-operator"></a>mvexpand operaattori

    T | mvexpand listColumn 

Laajentaa luodun luettelon solusta dynamic kirjoitettu (JSON), niin, että jokainen tapahtuma on rivillään. Laajennettu rivin muihin soluihin on kaksoiskappale. 

(Katso myös [`summarize makelist`](#summarize-operator) joka suorittaa vastakkainen funktion.)

**Esimerkki**

Oletetaan, että syötetaulukko on:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hei"|{"avain": "arvon"}|
|2|"maailman"|[0,1, "k", "a"]|

    mvexpand D

Tulos on seuraava:

|A:int|B:String|D:Dynamic|
|---|---|---|
|1|"Hei"|{"avain": "arvon"}|
|2|"maailman"|0|
|2|"maailman"|1|
|2|"maailman"|"k"|
|2|"maailman"|"a"|


**Syntaksi**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumentit**

* *ColumnName:* Tulos on matriisin nimetty-sarakkeessa on laajennettu usealle riville. 
* *ArrayExpression:* Lauseke, joka pöytäviinin matriisin. Jos käytössä on tämän lomakkeen, uusi sarake lisätään ja aiemmin luotuun säilytetään.
* *Nimi:* Uuden sarakkeen nimi.
* *Typename:* Muuttaa tietyn laajennettu lauseke sarakemuotoa
* *RowLimit:* Luotu alkuperäinen kunkin rivin rivien enimmäismäärä. Oletusarvo on 128.

**Palauttaa**

Useita rivejä kullekin kaikki matriisin nimetty sarakkeen tai taulukon lausekkeen arvoa.

Laajennettu-sarakkeessa on aina dynaaminen tyyppi. Käytä cast kuten `todatetime()` tai `toint()` Jos haluat laskea tai koostefunktioiden.

Ominaisuuden kantolaukku expansions jompaakumpaa tuetaan:

* `bagexpansion=bag`: Ominaisuus pakkaaminen on laajennettu yksittäisen tiedon ominaisuuden pakkaaminen kyselyjä. Tämä on oletusarvo-laajennus.
* `bagexpansion=array`: Ominaisuus pakkaaminen laajennetaan kaksi elementtiin `[` *avaimen*`,`*arvo* `]` matriisin rakenteiden sallitaan yhtenäisen avaimet ja arvot (sekä, esimerkiksi käynnissä erillisten määrän koonti ominaisuuden nimen yllä). 

**Esimerkkejä**


    exceptions | take 1 
  	| mvexpand details[0]

Jakaa poikkeus-tietueen riveihin kunkin kohteen tiedot-kenttään.



### <a name="parse-operator"></a>Jäsennä-operaattori

    T | parse "I got 2 socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long *


    T | parse kind=relaxed
          "I got no socks for my birthday when I was 63 years old" 
    with * "got" counter:long " " present "for" * "was" year:long * 

    T |  parse kind=regex "I got socks for my 63rd birthday" 
    with "(I|She) got" present "for .*?" year:long * 

Poimii merkkijonosta arvot. Voit käyttää yksinkertaisia tai säännöllisen lausekkeen vastaavat.

**Syntaksi**

    T | parse [kind=regex|relaxed] SourceText 
        with [Match | Column [: Type [*]] ]  ...

**Argumentit**

* `T`:-Syötetaulukko.
* `kind`: 
 * `simple`(oletus): `Match` merkkijonot on tavallista merkkijonoja.
 * `relaxed`: Jos teksti ei jäsentää kuin sarakkeen tyyppi-sarakkeessa on määritetty null ja jäsennä jatkuu 
 * `regex`: `Match` merkkijonot on säännöllisiä lausekkeita.
* `Text`: Sarakkeen tai muita arvo tai voidaan muuntaa tekstimerkkijonon lauseke.
* *Vastine:* Vastaa merkkijonon seuraavaan osaan ja hylätä sen.
* *Sarake:* Määritä tämä sarake merkkijonon seuraavaan osaan. Sarakkeen luodaan, jos sitä ei ole.
* *Tyyppi:* Seuraavassa osassa merkkijonon jäsentää määritetyn Tallennusmuoto-int, kuten päivämäärä, kaksinkertainen. 


**Palauttaa**

Syötetaulukko laajennettu sarakeluettelossa mukaan.

Elementtien `with` lauseen täyttyvät puolestaan vastaan Lähdeteksti. Muut elementit chews lohkon lähde tekstin käytöstä: 

* Merkkijono tai säännöllisen lausekkeen siirtää vastaavia kohdistimen vastine pituuden mukaan.
* Säännöllinen lauseke voit käyttää regex-jäsennä vesistöihin operaattori "?" Siirry seuraavat vastine mahdollisimman pian.
* Sarakkeen nimi, jonka tyyppi jäsentää tekstin määritetyn tyyppi. Ellei tyyppi = kevennetty, epäonnistunut jäsennä kumoavat vastaavat koko mallia.
* Sarakkeen nimi ilman tyyppi, tai kirjoita merkkijono, kopioi seuraavat vastine siirtyäksesi pienin merkkimäärän.
* ' *' Ohittaa merkit, jotka seuraavat vastine siirtyä vähimmäismäärä. Voit käyttää '*' alussa ja lopussa kuvio- tai sen jälkeen tyyppi kuin merkkijono tai merkkijono vastaa välillä.

Jäsennä kuvion osissa on vastattava oikein. Muussa tapauksessa valmistettu tuloksia. Poikkeus tähän sääntöön on tilanne takuita = kevennetty, jos jäsennyksen kirjoitetun muuttujan epäonnistuu, jäsennä muiden jatkaa.

**Esimerkkejä**

*Yksinkertainen:*

```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse "I got 2 socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```

x | Laskuri | Esitä | Vuoden
---|---|---|---
1 | 2 | SOCKS | 63

*Katsottu:*

Syöte on kirjoitettu jokaisessa sarakkeessa oikein vastinetta, kun kevennetty jäsennä on samanlainen kuin yksinkertainen jäsennys. Mutta kirjoitetun sarakkeen ei jäsentää oikein, jos kevennetty jäsennä jatkaa käsittelee loput kohdalla, olisi yksinkertainen jäsennä pysähtyy ja lakkaa Luo minkä tahansa tuloksen.


```AIQL

// Test without reading a table:
 range x from 1 to 1 step 1 
 | parse kind="relaxed"
        "I got several socks for my birthday when I was 63 years old" 
    with 
     *   // skip until next match
     "got" 
     counter: long // read a number
     " " // separate fields
     present // copy string up to next match
     "for" 
     *  // skip until next match
     "was" 
     year:long // parse number
     *  // skip rest of string
```


x  | Esitä | Vuoden
---|---|---
1 |  SOCKS | 63


*Regex:*

```AIQL

// Run a test without reading a table:
range x from 1 to 1 step 1 
// Test string:
| extend s = "Event: NotifySliceRelease (resourceName=Scheduler, totalSlices=27, sliceNumber=16, lockTime=02/17/2016 08:41, releaseTime=02/17/2016 08:41:00, previousLockTime=02/17/2016 08:40:00)" 
// Parse it:
| parse kind=regex s 
  with ".*?[a-zA-Z]*=" resource 
       ", total.*?sliceNumber=" slice:long *
       "lockTime=" lock
       ",.*?releaseTime=" release 
       ",.*?previousLockTime=" previous:date 
       ".*\\)"
| project-away x, s
```

resurssi | sektori | Lukitse | Vapauta | Edellinen
---|---|---|---|---
Ajoitus | 16 | 02/17/2016 08:41:00 | 02/17/2016 08:41 | 2016-02-17T08:40:00Z

### <a name="project-operator"></a>Project-operaattori

    T | project cost=price*quantity, price

Voit lisätä, nimetä uudelleen tai Poista sarakkeet ja lisää uusia laskettuja sarakkeita. Tuloksessa sarakkeiden järjestys on määritetty argumenttien järjestyksen mukaan. Vain sarakkeet, jotka on määritetty argumenttien sisältyvät tulos: kaikki muut Input olevat kohteet poistetaan.  (Katso myös `extend`.)


**Syntaksi**

    T | project ColumnName [= Expression] [, ...]

**Argumentit**

* *T:* Syötetaulukko.
* *ColumnName:* Tulosteessa näkyvän sarakkeen nimi. Jos ei ole *lauseke*-sarakkeen nimi on näkyvät syöte. [Nimien](#names) kirjainkoko on merkitsevä ja voi olla kirjaimia, numeroita tai "_"-merkkejä. Käytä `['...']` tai `["..."]` , tarjouksen avainsanoja tai muiden näppäimistöstä puuttuvien merkkien nimet.
* *Lauseke:* Valinnainen skalaarilauseke, joka syötteen sarakkeisiin viittaaminen. 

    On oikeudellinen palauttaa uusi laskettu sarake, jossa on sama nimi kuin aiemmin luodun sarakkeen syöte.

**Palauttaa**

Taulukko, jossa on sarakkeet argumentteina ja niin monta riviä kuin syötetaulukko.

**Esimerkki**

Seuraavassa esimerkissä yhdistämismäärityksiä, joihin voi käyttää erilaisia avulla `project` operaattori. Syötetaulukko `T` on kolme saraketta, jonka tyyppi `int`: `A`, `B`, ja `C`. 

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### <a name="project-away-operator"></a>projektin poistunut operaattori

    T | project-away column1, column2, ...

Jätä määritettyihin sarakkeisiin. Tulos on syötteen sarakkeita lukuun ottamatta nimi.

### <a name="range-operator"></a>Alueoperaattori

    range LastWeek from ago(7d) to now() step 1d

Luo arvojen yksisarakkeisen taulukon. Huomaa, että se ei ole syötetty putkijohto. 

|LastWeek|
|---|
|2015-12-05 09:10:04.627|
|2015 12-06 09:10:04.627|
|...|
|2015-12-12 09:10:04.627|



**Syntaksi**

    range ColumnName from Start to Stop step Step

**Argumentit**

* *ColumnName:* Yhden sarakkeen tulostus-taulukon nimi.
* *Aloittaminen:* Tulosteen pienin arvo.
* *Lopettaminen:* Tulos (tai suurimman arvon, jos *Vaihe* vaiheet päälle arvoksi-sidottu) luodaan suurin arvo.
* *Vaihe:* Kaksi peräkkäistä arvon eron. 

Argumenttien on oltava lukuja, päivämääriä tai aikajakson arvot. Hän ei voi viitata taulukon sarakkeita. (Jos haluat laskea alueen syötteen taulukon perusteella, käytä [alue- *funktio*](#range)on ehkä [mvexpand operaattori](#mvexpand-operator).) 

**Palauttaa**

Taulukko, jossa on yksi sarake nimeltä *ColumnName*, jonka arvot on *Käynnistä-painiketta*, *Käynnistä* + *Vaihe*... ja joka sisältää *Lopeta*.

**Esimerkki**  

```AIQL
range Steps from 1 to 8 step 3
```

Taulukko, jossa on vain yksi sarake nimeltä `Steps` jonka tyyppi on `long` ja joiden arvot ovat `1`, `4`, ja `7`.

**Esimerkki**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Milloin viimeisten seitsemän päivän keskiyöhön sisällysluettelon. Bin (PYÖRISTÄ.KERR.alas)-toiminto vähentää aina päivän alkuun.

**Esimerkki**  

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Näyttää siitä, kuinka `range` operaattoria voidaan luoda pieni, itsenäisen, dimensio-taulukosta, jossa käytetään ottamaan nollia missä lähdetietojen ei ole arvoja.

### <a name="reduce-operator"></a>Pienennä operaattori

    exceptions | reduce by outerMessage

Yrittää ryhmitellä yhteen vastaavat tietueet. Kunkin ryhmän operaattori tulostaa `Pattern` se saattavat mukaan parhaiten kyseisen ryhmän ja `Count` tietueiden kyseisen ryhmän.


![](./media/app-insights-analytics-reference/reduce.png)

**Syntaksi**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumentit**

* *ColumnName:* Tutkia sarake. Sen on oltava merkkijonotyyppi.
* *Kynnysarvo:* {0-– 1 raja}-alueella olevan arvon. Oletusarvo on muutos iteraatiokertojen 0,001. Suuri syötteiden raja-arvon pitäisi olla pieni. 

**Palauttaa**

Kahden sarakkeen `Pattern` ja `Count`. Monissa tapauksissa kaava on valmis sarakkeen arvosta. Joissakin tapauksissa se tunnistaa Yleiset termit ja korvaa muuttujan osaa niiden "*".

Esimerkiksi saatu `reduce by city` voivat kuulua muun muassa: 

|Kuvion | Laske |
|---|---|
| San * | 5182 |
| San * | 2846 |
| Moskova | 3726 |
| \*-on-\* | 2730 |
| Pariisi | 27163 |


### <a name="render-directive"></a>direktiivin hahmontaminen

    T | render [ table | timechart  | barchart | piechart ]

Hahmontaminen ohjaa esityksen kerros kuinka taulukko. Olisi pystyviivaa viimeisen osan. Se on kätevä vaihtoehto näytössä, jonka avulla voi tallentaa kyselyn tiettyyn esityksen menetelmän ohjausobjekteilla.

### <a name="restrict-clause"></a>Rajoita lause 

Määrittää taulukoiden nimet on käytettävissä, jotka noudattavat operaattorit. Esimerkki:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### <a name="sort-operator"></a>Lajittele-operaattori 

    T | sort by country asc, price desc

Lajittele rivit syötteen taulukon järjestykseen yhden tai usean sarakkeen mukaan.

**Alias**`order`

**Syntaksi**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumentit**

* *T:* Taulukon lajitteleminen syöte.
* *Sarake:* *T* sarakkeen mukaan, jonka mukaan haluat lajitella. Arvojen tyyppi on oltava numeerinen, päivämäärän, kellonajan tai merkkijono.
* `asc`Lajittele nousevassa järjestyksessä, pieni suuri kyselyjä. Oletusarvo on `desc`, laskeva suuri pienimpään.

**Esimerkki**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Kaikki rivit, joilla on tietyn taulukon jäljittää `ActivityId`, lajiteltuina niiden aikaleima mukaan.

### <a name="summarize-operator"></a>yhteenvedon operaattori

Luo taulukko, joka kokoaa syötetaulukko sisällöstä.
 
    requests
  	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Taulukko, jossa näkyy kunkin maan numero, keskimääräinen pyynnön kesto ja määritä kaupunkia. Niiden eri maiden tulos on rivin. Tulostus-sarakkeissa näkyy laskeminen, keskiarvo kesto, kaupunkien ja maa. Kaikki muut syötteen sarakkeet ohitetaan.


    T | summarize count() by price_range=bin(price, 10.0)

Taulukko, joka näkyy, kuinka monta kohdetta on hinnat kussakin [0,10.0], [10.0,20.0] ja niin edelleen. Tässä esimerkissä sarakkeen määrä ja hinta-alue on. Kaikki muut syötteen sarakkeet ohitetaan.


**Syntaksi**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumentit**

* *Sarake:* Valinnainen tuloksen sarakkeen nimi. Oletusarvo on nimi johdetaan lauseke. [Nimien](#names) kirjainkoko on merkitsevä ja voi olla kirjaimia, numeroita tai "_"-merkkejä. Käytä `['...']` tai `["..."]` , tarjouksen avainsanoja tai muiden näppäimistöstä puuttuvien merkkien nimet.
* *Kooste:* Koostefunktio, kuten puhelun `count()` tai `avg()`-sarakkeen nimet argumentteina. Katso [koosteet](#aggregations).
* *GroupExpression:* Sarakkeiden yli lauseke, joka sisältää eri arvojoukon. On yleensä joko sarakkeen nimi, joka on jo rajoitettu sarja-arvojen tai `bin()` numeerisen lausekkeen tai aika-sarakkeen argumenttina. 

Jos annat numeerisen lausekkeen tai aika-lauseke käyttämättä `bin()`, Analytics muotoilee sen välein `1h` silloin, tai `1.0` luvut.

Jos et anna *GroupExpression,* koko taulukko on koottu tulosteen yhden rivin.



**Palauttaa**

Syötteen rivit on järjestetty ryhmiin samat arvot, joilla `by` lausekkeita. Valitse määritetyn koostefunktiot lasketaan kunkin ryhmän tuottaminen kullekin riville päälle. Tulos on `by` sarakkeet ja myös vähintään yhden sarakkeen kunkin luvut kooste. (Jotkin koostefunktiot palauttavat useita sarakkeita).

Tulos on yhtä monta riviä kuin eri yhdistelmät `by` arvot. Jos haluat tehdä yhteenvedon päälle numeerisia arvoja, käytä `bin()` vähentää alueiden erilliset arvot.

**Huomautus**

Voit antaa haluamaansa lausekkeiden kooste ja ryhmittelylausekkeita, mutta se on tehokkaampaa olevilla yksinkertainen sarakkeiden nimet tai käyttää `bin()` numeerinen sarakkeeseen.



### <a name="take-operator"></a>Ota operaattori

[Raja](#limit-operator) alias


### <a name="top-operator"></a>Ylimmät operaattori

    T | top 5 by Name desc nulls first

Palauttaa ensimmäisen *N* tietueet lajiteltu määritetyn sarakkeen perusteella.


**Syntaksi**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [`nulls first`|`nulls last`] [, ... ]

**Argumentit**

* *NumberOfRows:* Palauttaa *T* rivien määrä.
* *Sort_expression:* Lauseke, jonka mukaan lajittelu rivit. Se on yleensä vain sarakkeen nimi. Voit määrittää useita sort_expression.
* `asc`tai `desc` (oletusasetus) saattaa näkyä ohjausobjektin, onko valinta on todella "ala" tai "alkuun" alue.
* `nulls first`tai `nulls last` ohjausobjektit, jossa null-arvot tulevat näkyviin. `First`Oletusarvo on `asc`, `last` on oletusarvo `desc`.


**Vihjeitä**

`top 5 by name`vastaa sovelluksen `sort by name | take 5`. Suoritetaan nopeammin ja aina palauttaa lajitella tulokset olisi kuitenkin `take` ei takaa tällaisten.

### <a name="top-nested-operator"></a>ensimmäiset sisäkkäinen operaattori

    requests 
  	| top-nested 5 of name by count()  
    , top-nested 3 of performanceBucket by count() 
    , top-nested 3 of client_CountryOrRegion by count()
  	| render barchart 

Tuottaa hierarkkisia Tulosta, missä kunkin tason alirakenteen edellisen tasolla. Se on hyödyllinen vastauksia kysymyksiin, jotka ääni, kuten "mitä Suosituimmat 5 pyynnöt ja kullekin niistä mitä yläreunan 3 suorituskyvyn uudelleenkäytettävät ja kullekin niistä on ylhäällä 3 maat pyynnöt tulevien?"

**Syntaksi**

   T | ensimmäiset sisäkkäinen N, SARAKKEEN KOOSTAMINEN [,...]

**Argumentit**

* N:int - palauttaa tai välittää edistynyt rivien määrä. Kysely, jossa on kolme tasoa, jossa N on 5, 3 ja 3-rivien kokonaismäärästä ovat 45.
* SARAKE - sarake koostamisen Ryhmittele. 
* KOOSTE - [koostefunktio](#aggregations) koskee kunkin ryhmän rivien määrä. Nämä koosteet tulosten määrää yläreunan ryhmien näyttäminen.


### <a name="union-operator"></a>UNION-operaattorilla

     Table1 | union Table2, Table3

Valitsee vähintään kaksi taulukkoa ja palauttaa ne kaikki rivit. 

**Syntaksi**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumentit**

* *Taulukko1*, *taulukko2* ...
 *  Taulukon nimi, kuten `requests`, tai taulukko, joka on määritetty [Anna lause](#let-clause); tai
 *  A kyselyn lauseke`(requests | where success=="True")`
 *  Taulukoiden yleismerkkiä määritetty joukko. Esimerkiksi `e*` lomakkeen kaikki edellisen nimi alkamisen "e" ja "poikkeukset"-taulukko ja anna lauseet taulu unionin.
* `kind`: 
 * `inner`-Tulos on alijoukon, sarakkeet, jotka löytyvät kaikki syötteen taulukot.
 * `outer`-Tulos on kaikki sarakkeet, jotka ilmenevät kaikista syötteiden. Solut, jotka on määritetty syötteen riveittäin määritetään `null`.
* `withsource=`*ColumnName:* Jos määritetty, tulos sisältää sarake nimeltä *ColumnName* , jonka arvo osoittaa, mihin lähdetaulukkoa on lähettänyt kullekin riville.

**Palauttaa**

Taulukko, jossa on yhtä monta riviä muotoilussa ovat syötteen taulukoita, ja muotoilussa yhtä monta saraketta yksilöllisen sarakkeen syötteiden nimiä.

**Esimerkki**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Kaikki taulukot, joiden nimet alkavat "tt" UNION.


**Esimerkki**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Eri käyttäjille, että on valmistettu määrä `exceptions` tapahtuma tai `traces` tapahtuman edellisen päivän päälle. Tulos on "Lähdetaulukko"-sarakkeen ilmoittaa "Kyselyn" tai "Komennolla".

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Tehokkaampi versiossa tuottaa saman tuloksen. Se suodattaa kunkin taulukon ennen kuin luot unionin.

### <a name="where-operator"></a>Jos operaattori

     requests | where resultCode==200

Suodattaa taulukon rivit, jotka täyttävät lause alijoukkoa.

**Alias**`filter`

**Syntaksi**

    T | where Predicate

**Argumentit**

* *T:* Taulukkomuotoinen syöte, joiden tietueet ovat voi suodattaa.
* *Predikaatin:* A `boolean` [lausekkeen](#boolean) *T*sarakkeiden yli. Se arvioidaan kullekin riville *T*.

**Palauttaa**

*T* , jossa *predikaatin* on rivien `true`.

**Vihjeitä**

Saat nopeimmin suorituskyvyn:

* **Käytä yksinkertaista vertailuja** välisten sarakkeiden nimet ja vakioita. (Vakio"tarkoittaa vakio - taulukon päälle niin `now()` ja `ago()` ovat OK, ja siten skalaarinen arvot määritetään käyttämällä [ `let` lauseen](#let-clause).)

    Esimerkiksi mieluummin `where Timestamp >= ago(1d)` , `where floor(Timestamp, 1d) == ago(1d)`.

* **Simplest ehdot ensimmäisen**: Jos käytössäsi on useita lausekkeita conjoined kanssa `and`, sijoita ensin lausekkeita, jotka sisältävät vain yhden sarakkeen. Niin `Timestamp > ago(1d) and OpId == EventId` on parempi kuin haluamasi.


**Esimerkki**

```AIQL
traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Tietueet, jotka ovat vanhempia ei 1 tunti ja nimeltä "Kuskus" lähteestä on sama arvo kaksi saraketta. 

Huomaa, että olemme valitseminen kahden sarakkeen väliin vertailu viimeiseksi sellaisena kuin se et voi hyödyntää indeksi ja pakottaa tarkistus.


### <a name="where-in-operator"></a>WHERE-operaattori

    requests | where resultCode !in (200, 201)

    requests | where resultCode in (403, 404)

**Syntaksi**

    T | where col in (expr1, expr2, ...)
    T | where col !in (expr1, expr2, ...)

**Argumentit**

* `col`: Taulukon sarakkeeseen.
* `expr1`...: Skalaarinen lausekeluettelon.

Käytä `in` sisältämään vain rivit, joissa käytetään `col` on yhtä suuri kuin yksi lausekkeet `expr1...`.

Käytä `!in` sisältämään vain rivit, joissa `col` ei ole sama kuin jokin lausekkeista `expr1...`.  


## <a name="aggregations"></a>Koosteet

Koosteet ovat arvot ryhmien luotu [yhteenvedon toiminto](#summarize-operator)yhdistää käytettävistä funktioista. Tässä kyselyssä THaku on koostefunktio:

    requests | summarize dcount(name) by success

### <a name="any"></a>mikä tahansa 

    any(Expression)

Satunnaisesti valitsee yhden rivin ryhmän ja palauttaa määritetyn lausekkeen arvo.

Tästä on hyötyä, esimerkiksi kun joitakin sarake on suuri määrä vastaavat arvot (esimerkiksi "virheteksti"-sarake) ja haluat Esimerkki sarakkeen kerran koostetun ryhmätunnus yksilöllinen arvo. 

**Esimerkki**  

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a>
<a name="argmax"></a>
### <a name="argmin-argmax"></a>argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Etsii rivin ryhmä, johon pienentää/maximises *ExprToMaximize*ja palauttaa arvon *ExprToReturn* (tai `*` palauttaa koko rivi).

**Vihje**: välitetty kautta sarakkeet automaattisesti nimetään uudelleen. Varmista, että käytät oikeaa nimiä, Tarkasta tulokset `take 5` ennen toisen toimijan kyselyjä pipe tulokset.

**Esimerkkejä**

Näytä pyynnön nimet, kun pisimmän pyynnön tapahtui:

    requests | summarize argmax(duration, timestamp) by name

Näytä kaikki pisimmän pyynnön, ei pelkästään aikaleima tiedot:

    requests | summarize argmax(duration, *) by name


Etsi kunkin metrijärjestelmä sivustoa sen aikaleima ja muiden tietojen pienimmän arvon:

    metrics 
  	| summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-reference/argmin.png)
 


### <a name="avg"></a>keskiarvo

    avg(Expression)

Laskee *lauseke* ryhmän yli keskiarvon.

### <a name="buildschema"></a>buildschema

    buildschema(DynamicExpression)

Palauttaa mahdollisimman vähän rakenne, joka voi osallistua *DynamicExpression*kaikki arvot. 

Parametrin Saraketyyppi pitäisi olla `dynamic` -matriisin tai ominaisuuden kantolaukku. 

**Esimerkki**

    exceptions | summarize buildschema(details)

Tulos:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Huomaa, että `indexer` merkitään, joissa kannattaa käyttää numeerinen. Tämän rakenteen joitakin kelvollisia polkuja olla (olettaen että alueella on esimerkki näiden indeksien):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Esimerkki**

Oletetaan, että input-sarakkeessa on kolme dynaamisia arvoja:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Tuloksena rakenne on seuraava:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Rakenteen kertoo meille:

* Pääobjektin on säilö, jossa on neljä ominaisuudet nimeltä x ja y-z t.
* Ominaisuus nimeltä "x", joka voi olla tyypin "int" tai "merkkijonon" tyyppi.
* Ominaisuus nimeltä "a", jotka saattavat joko "double-tyyppiä tai säilö ominaisuus nimeltä"w"tyyppi"merkkijonon".
* ``indexer`` Avainsana osoittaa, että "z" ja "t" ovat matriiseja.
* Kunkin kohteen "z"-matriisissa on kokonaisluku tai merkkijono.
* "t" on matriisin merkkijonoja.
* Jokaisen ominaisuus on implisiittisesti valinnainen ja mitä tahansa taulukon saattaa olla tyhjä.

##### <a name="schema-model"></a>Rakenne-malli

Palautetut rakenteen syntaksi on seuraava:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
    Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Ne vastaavat alijoukkoa kirjoituskoneella tyyppi-huomautukset-koodattu dynaamisen arvon. Kirjoituskoneella esimerkiksi rakenne on seuraava:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### <a name="count"></a>Laske

    count([ Predicate ])

Palauttaa, jonka *predikaatin* arvoksi rivien määrän, `true`. Jos ei ole *predikaatin* ei määritetä, palauttaa tietueiden kokonaismäärän ryhmän. 

**Perf Vihje**: Käytä `summarize count(filter)` sijaan`where filter | summarize count()`

> [AZURE.NOTE] Vältä count() etsiminen pyynnöt, poikkeukset tai muita tapahtumia asennuksen aikana tapahtuneista virheistä. Kun toiminto on [Esimerkkejä](app-insights-sampling.md) , sovelluksen tiedot säilyvät arvopisteiden määrä on pienempi kuin alkuperäinen tapahtumien määrä. Käytä `summarize sum(itemCount)...`. ItemCount-ominaisuuden kuvastaa alkuperäisen tapahtumia, jotka ilmaistaan säilytetä kunkin arvopisteen määrä.

### <a name="countif"></a>Laske.Jos

    countif(Predicate)

Palauttaa, jonka *predikaatin* arvoksi rivien määrän, `true`.

**Perf Vihje**: Käytä `summarize countif(filter)` sijaan`where filter | summarize count()`

> [AZURE.NOTE] Vältä countif() etsiminen pyynnöt, poikkeukset tai muita tapahtumia asennuksen aikana tapahtuneista virheistä. Kun toiminto on [Esimerkkejä](app-insights-sampling.md) , arvopisteiden määrä on pienempi kuin todellinen tapahtumien määrä. Käytä `summarize sum(itemCount)...`. ItemCount-ominaisuuden kuvastaa alkuperäisen tapahtumia, jotka ilmaistaan säilytetä kunkin arvopisteen määrä.

### <a name="dcount"></a>TLASKE

    dcount( Expression [ ,  Accuracy ])

Palauttaa ryhmän arvio *lauseke* eri arvojen määrä. (Jos luettelon eri arvoja, käytä [`makeset`](#makeset).)

*Tarkkuutta*, jos määritetty, määrittää nopeus ja tarkkuus tasapaino.

 * `0`= vähintään ovat yhdenmukaisia ja nopein laskentaan.
 * `1`Oletusarvon mukaisen saldojen tarkkuutta ja laskennan kerran. Lisätietoja 0,8 %-virhe.
 * `2`= eniten ovat yhdenmukaisia ja mahdollisimman pieneksi laskenta; Lisätietoja 0,4 ilmoitettuna olevien %-virhe.

**Esimerkki**

    pageViews 
  	| summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/dcount.png)


### <a name="dcountif"></a>dcountif

    dcountif( Expression, Predicate [ ,  Accuracy ])

Palauttaa arvio *lauseke* rivien eri arvojen määrä, jonka *predikaatin* on tosi-ryhmässä. (Jos luettelon eri arvoja, käytä [`makeset`](#makeset).)

*Tarkkuutta*, jos määritetty, määrittää nopeus ja tarkkuus tasapaino.

 * `0`= vähintään ovat yhdenmukaisia ja nopein laskentaan.
 * `1`Oletusarvon mukaisen saldojen tarkkuutta ja laskennan kerran. Lisätietoja 0,8 %-virhe.
 * `2`= eniten ovat yhdenmukaisia ja mahdollisimman pieneksi laskenta; Lisätietoja 0,4 ilmoitettuna olevien %-virhe.

**Esimerkki**

    pageViews 
  	| summarize cities=dcountif(client_City, client_City startswith "St") 
      by client_CountryOrRegion


### <a name="makelist"></a>makelist

    makelist(Expr [ ,  MaxListSize ] )

Palauttaa `dynamic` (JSON)-taulukko, jonka *lauseke* -ryhmän kaikki arvot. 

* *MaxListSize* on valinnainen kokonaisluku-raja-funktio palauttaa elementtien enimmäismäärä (oletusarvo on *128*).

### <a name="makeset"></a>makeset

    makeset(Expression [ , MaxSetSize ] )

Palauttaa `dynamic` eri arvoja, jotka *lauseke* Vie-ryhmässä olevan matriisin (JSON). (Vihje: Jos haluat laskea vain yksilölliset arvot, käytä [`dcount`](#dcount).)
  
*  *MaxSetSize* on valinnainen kokonaisluku-raja-funktio palauttaa elementtien enimmäismäärä (oletusarvo on *128*).

**Esimerkki**

    pageViews 
  	| summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-reference/makeset.png)

Katso myös [ `mvexpand` operaattori](#mvexpand-operator) vastakkainen funktion.


### <a name="max-min"></a>Max, min

    max(Expr)

Laskee *lauseke*enimmäismäärän.
    
    min(Expr)

Laskee *lauseke*pienin arvo.

**Vihje**: Tämä antaa min tai Maks oma – esimerkiksi suurimman tai pienimmän hinta. Mutta voit halutessasi rivi - esimerkiksi alimman hinnan toimittaja nimi - muihin sarakkeisiin käyttämällä [argmin tai argmax](#argmin-argmax).


<a name="percentile"></a>
<a name="percentiles"></a>
<a name="percentilew"></a>
<a name="percentilesw"></a>
### <a name="percentile-percentiles-percentilew-percentilesw"></a>prosenttipisteen arvona percentilew, percentilesw

    percentile(Expression, Percentile)

Palauttaa määritetyn prosenttipisteen ryhmän *lausekkeen* arvio. Tarkkuutta määräytyy populaation prosenttipiste alueen avulla.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ...] )

Kuten `percentile()`, mutta laskee prosenttipiste arvojen määrän (joka on nopeampaa kuin lasketaan kullekin prosenttipiste erikseen).

    percentilew(Expression, WeightExpression, Percentile)

Painotetun prosenttipisteen. Käytä tätä valmiiksi kootut tiedot.  `WeightExpression`on kokonaisluku, joka osoittaa alkuperäisen rivien määrän ilmaistaan koostetun kullekin riville.

    percentilesw(Expression, WeightExpression, Percentile1, [, Percentile2 ...])

Kuten `percentilew()`, mutta laskee prosenttipiste arvojen määrän.

**Esimerkkejä**


Arvon `duration` , joka on suurempi kuin 95 % näytteen määritetty ja pienempi kuin 5 prosenttia mallitietoihin laskettu pyynnön kukin nimi:

    request 
  	| summarize percentile(duration, 95)
      by name

Jätä pois "mukaan..." laskemiseen koko taulukon.

Laske useita eri pyynnön nimien arvona samanaikaisesti:

    
    requests 
  	| summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-reference/percentiles.png)

Tulokset osoittavat, pyynnön /Events/Index, 5 prosenttia pyynnöt ovat vastanneet pienempi kuin-2.44s, puolet niiden 3.52s, ja 5 % olet hitaammin 6.85s.

Laskeminen useiden tilastotiedot:

    requests 
  	| summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

#### <a name="weighted-percentiles"></a>Painotetun prosenttipiste

Painotetun prosenttipiste-funktioilla tapauksissa, jossa tiedot on jo valmiiksi koostetun. 

Oletetaan, että sovelluksesi suorittaa toimintoja sekunnissa monta tuhansia ja haluat tietää niiden viive. Yksinkertainen ratkaisu olisi sovelluksen tiedot-pyynnön tai mukautetun tapahtuman jokaiselle luomiseen. Tämä loisi paljon liikennettä, vaikka mukautuvat esimerkkejä otetaan käyttöön, voit pienentää sen. Mutta päätät toteuttamisesta näkyisivät paremmin ratkaisu: Voit kirjoittaa lisäkoodin voit koota tietoja ennen sen lähettämistä sovelluksen tiedot-sovelluksen. Koostetun yhteenveto lähetetään vähentää tietojen määrä esimerkiksi muutama minuutti pistemäärän säännöllisin väliajoin.

Koodi on stream viive millisekunteina mittoja. Esimerkki:
    
     { 15, 12, 2, 21, 2, 5, 35, 7, 12, 22, 1, 15, 18, 12, 26, 7 }

Funktio laskee seuraavat palkkien mitat:`{ 10, 20, 30, 40, 50, 100 }`

Määräajoin se on TrackEvent soittamiseen, yhteen kunkin aikajakson kanssa mukautettuja mitat kunkin kutsussa sarja: 

    foreach (var latency in bins.Keys)
    { telemetry.TrackEvent("latency", null, 
         new Dictionary<string, double>
         ({"latency", latency}, {"opCount", bins[latency]}}); }

Analytics näet tällaisen ryhmä tapahtumien tältä:

`opCount` | `latency`| merkitys
---|---|---
8 | 10 | = 8 10ms roskakorissa olevat toiminnot
6 | 20 | = 6 20ms roskakorissa olevat toiminnot
3 | 30 | = 3 30 MS-roskakorissa olevat toiminnot
1 | 40 | = 1 toiminnot 40ms Roskakori

Saat tarkkoja kuvan, tapahtuman viiveitä alkuperäisen jakauman-Käytämme `percentilesw`:

    customEvents | summarize percentilesw(latency, opCount, 20, 50, 80)

Tulokset ovat samat kuin, jos on ollut käytetty vain `percentiles` -alkuperäiset mitat.

> [AZURE.NOTE] Painotetun arvona eivät koske [näyte tiedot](app-insights-sampling.md)-, jossa näytteeksi kunkin rivin otos satunnaisia alkuperäisen rivejä bin sijaan. Tavallinen prosenttipiste-funktiot soveltuvat näytteeksi tiedot.

#### <a name="estimation-error-in-percentiles"></a>Prosenttipiste virhe arviointi

Prosenttipiste-kooste on arvio käyttämällä [T käsittely](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf). 

Joitakin tärkeitä seikkoja: 

* Arvio virheen rajojen vaihtelevat pyydetty prosenttipisteen arvo. Paras tarkkuutta on [0..100] päissä skaalata, PROSENTTIPISTE 0 ja 100 ovat täsmälleen vähimmäis- ja enimmäisarvot jakauman. Tarkkuutta vähenee vähitellen kohti asteikon keskellä. Se on huonoin, Mediaani ja 1 % enimmillään. 
* Virhe rajojen havaitaan järjestyksen mukaan arvojen käyttöön. Oletetaan, että prosenttipiste (X 50) palauttaa arvon Xm. Arvio takaa 49 % ja enintään 51 % arvoista x on pienempi kuin Xm. Ei ole teoreettinen rajoitettu Xm ja X todellinen mediaanin välinen ero.

### <a name="stdev"></a>keskihajonta-funktio

     stdev(Expr)

Palauttaa ryhmän *lauseke* KESKIHAJONTAA.

### <a name="variance"></a>varianssi

    variance(Expr)

Palauttaa ryhmän *lauseke* varianssin.

### <a name="sum"></a>summa

    sum(Expr)

Palauttaa ryhmän *lauseke* .                      


## <a name="scalars"></a>Skalaareja

[esimerkiksi lähetyksiin](#casts) | [vertailu](#scalar-comparisons)
<br/>
[GetType](#gettype) | [hash](#hash) | [iff](#iff) |  [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull) | [toscalar](#toscalar)

Tuetut tiedostotyypit ovat:

| Tyyppi      | Lisää nimet   | Vastaavat .NET-tyyppi |
| --------- | -------------------- | -------------------- |
| `bool`    | `boolean`            | `System.Boolean`     |
| `datetime`| `date`               | `System.DateTime`    |
| `dynamic` |                      | `System.Object`      |
| `guid`    | `uuid`, `uniqueid`   | `System.Guid`        |
| `int`     |                      | `System.Int32`       |
| `long`    |                      | `System.Int64`       |
| `double`  | `real`               | `System.Double`      |
| `string`  |                      | `System.String`      |
| `timespan`| `time`               | `System.TimeSpan`    |

### <a name="casts"></a>Esimerkiksi lähetyksiin

Voit pakottaa julkaisusta toiseen. Yleensä Jos muuntaminen on järkevää, se toimi:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
    toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

Tarkista, onko merkkijonon voidaan muuntaa tietyntyyppinen:

    iff(notnull(todouble(customDimensions.myValue)),
       ..., ...)

### <a name="scalar-comparisons"></a>Skalaaripäivämäärän vertailu

||
---|---
`<` |Pienempi
`<=`|Pienempi tai yhtä suuri kuin
`>` |Suurempi
`>=`|Suurempi tai yhtä suuri kuin
`<>`|Eri suuri kuin
`!=`|Eri suuri kuin 
`in`| Oikeanpuoleinen operandi on (dynaamisen) taulukko ja vasen operandi on yhtä tai sen osat.
`!in`| Oikeanpuoleinen operandi on (dynaamisen) taulukko ja vasen operandi ei ole sama kuin sen elementtejä.




### <a name="gettype"></a>GetType

**Palauttaa**

Yhden argumentin totuusarvon pohjana tallennustyyppi esittävä merkkijono. Tästä on hyötyä, kun on suoritettava arvoja `dynamic`: Tässä tapauksessa `gettype()` osoittavat, miten arvo on koodattu.

**Esimerkkejä**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8"`
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`
`gettype(1.2)==real` | `true`

### <a name="hash"></a>Hash

**Syntaksi**

    hash(source [, mod])

**Argumentit**

* *tietolähteen*: skalaarinen lähteen hajautuksen lasketaan.
* *Mod*: arvo, jota käytetään hash-tulos modulo.

**Palauttaa**

Xxhash (long) arvon annetun skalaari modulo (Jos määritetty) mod annetusta arvosta.

**Esimerkkejä**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### <a name="iff"></a>IFF

`iff()` Funktio käsittelee ensimmäisen argumentin (lause) ja palauttaa sen mukaan, onko lause toisen tai kolmannen argumentit arvosta `true` tai `false`. Toisessa ja kolmannessa argumenttien on oltava samaa tyyppiä.

**Syntaksi**

    iff(predicate, ifTrue, ifFalse)


**Argumentit**

* *lause:* Lauseke, joka määrittää `boolean` arvo.
* *ifTrue:* Lauseke, joka saa lasketaan ja sen arvojen palauttama funktion Jos *lause* tulos `true`.
* *ifFalse:* Lauseke, joka saa lasketaan ja sen arvojen palauttama funktion Jos *lause* tulos `false`.

**Palauttaa**

Tämä funktio palauttaa arvon *ifTrue* , jos *lause* `true`, tai muuten *ifFalse* arvon.

**Esimerkki**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a>
<a name="isnotnull"/></a>
<a name="notnull"/></a>
### <a name="isnull-isnotnull-notnull"></a>IsNull-isnotnull, notnull

    isnull(parsejson("")) == true

Yhden argumentin tulee ja ilmoittaa, onko null.

**Syntaksi**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Palauttaa**

TOSI tai EPÄTOSI sen mukaan onko arvo on null tai ole null.


|x|IsNull(x)
|---|---
| "" | EPÄTOSI
|"x" | EPÄTOSI
|parsejson("")|TOSI
|parsejson("[]")|EPÄTOSI
|parsejson("{}")|EPÄTOSI

**Esimerkki**

    T | where isnotnull(PossiblyNull) | count

Huomaa, että useilla eri tavoilla saavuttaa asiaa:

    T | summarize count(PossiblyNull)

### <a name="toscalar"></a>toscalar

Arvioi kyselyn tai lausekkeen, ja palauttaa tuloksen kuin yksittäisen arvon. Tämä funktio on hyödyllinen vaiheistettu laskelmat; esimerkiksi laskeminen tapahtuma-ja käyttämällä sitten, kun perusaikataulu kokonaismäärä.

**Syntaksi**

    toscalar(query)
    toscalar(scalar)

**Palauttaa**

Arvioitu-argumentti. Jos argumentti on taulukko, palauttaa ensimmäisen sarakkeen ensimmäisen rivin. (Hyvien on järjestämiseen argumentti on vain yksi sarake ja rivi).

**Esimerkki**

```AIQL

    // Get the count of requests 5 days ago:
    let baseline = toscalar(requests  
        | where floor(timestamp, 1d) == floor(ago(5d),1d) | count);
    // List the counts relative to that baseline:
    requests | summarize daycount = count() by floor(timestamp, 1d)  
  	| extend relative = daycount - baseline
```




### <a name="boolean-literals"></a>Totuusarvo literaalien

    true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### <a name="boolean-operators"></a>Totuusarvo-operaattorit

    and 
    or 

    

## <a name="numbers"></a>Luvut

[abs](#abs) | [bin](#bin) | [exp](#exp) | [floor](#floor) | [gamma](#gamma) |[log](#log) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) 
| [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### <a name="numeric-literals"></a>Numeerinen literaalien

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### <a name="arithmetic-operators"></a>Aritmeettiset operaattorit

|| |
|---|-------------|
| + | Lisää         |
| - | Vähentäminen    |
| * | Kertominen    |
| / | Jaa      |
| % | Modulo      |
||
|`<` |Pienempi
|`<=`|Pienempi tai yhtä suuri kuin
|`>` |Suurempi
|`>=`|Suurempi tai yhtä suuri kuin
|`<>`|Eri suuri kuin
|`!=`|Eri suuri kuin 


### <a name="abs"></a>itseisarvo

**Syntaksi**

    abs(x)

**Argumentit**

* x - kokonaisluku, reaaliluku tai aikajakson

**Palauttaa**

    iff(x>0, x, -x)

<a name="bin"></a><a name="floor"></a>
### <a name="bin-floor"></a>luokka, kerros

Pyöristää alaspäin kokonaisluvuksi annetun bin koon kerrannaiseen. Käytetään usein [`summarize by`](#summarize-operator) kyselyn. Jos sinulla on ajoittaisia arvojoukon, ne ryhmitellään alikansioiksi tietyn pienempi arvojoukon.

Alias `floor`.

**Syntaksi**

     bin(value, roundTo)
     floor(value, roundTo)

**Argumentit**

* *arvo:* Luku, päivämäärä tai aikajakson. 
* *roundTo:* "Bin koon". Luku, päivämäärä tai aika, joka jakaa *arvon*. 

**Palauttaa**

*RoundTo* alle *arvon*lähimpään kerrannaiseen.  
 
    (toint((value/roundTo)-0.5)) * roundTo

**Esimerkkejä**

Lauseke | Tulos
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`|  `datetime(1953-04-15)`


Seuraava lauseke laskee histogrammin keston, sekunnin aikajakson muuttaminen:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```

### <a name="exp"></a>eksponentti

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v


### <a name="floor"></a>kerroksen

Tunnusnimen [`bin()`](#bin).

### <a name="gamma"></a>gamma

[Gamma-funktio](https://en.wikipedia.org/wiki/Gamma_function)

**Syntaksi**

    gamma(x)

**Argumentit**

* *x:* Reaaliluku

Saat positiivisen kokonaisluvun `gamma(x) == (x-1)!` esimerkiksi `gamma(5) == 4 * 3 * 2 * 1`.

Katso myös [loggamma](#loggamma).


### <a name="log"></a>lokitiedoston

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v`on reaaliluku > 0. Muussa tapauksessa null palautetaan.

### <a name="loggamma"></a>loggamma


Itseisarvo [gammafunktion](#gamma)luonnollisen logaritmin.

**Syntaksi**

    loggamma(x)

**Argumentit**

* *x:* Reaaliluku


### <a name="rand"></a>SATUNNAISLUKU

Satunnaisen numeron luontitoiminnon.

* `rand()`-Reaaliluku 0.0 ja 1.0
* `rand(n)`-kokonaisluku väliltä 0 – n-1




### <a name="sqrt"></a>neliöjuuri

NELIÖJUURI-funktio.  

**Syntaksi**

    sqrt(x)

**Argumentit**

* *x:* Reaaliluku > = 0.

**Palauttaa**

* Positiivinen luku siten, että`sqrt(x) * sqrt(x) == x`
* `null`Jos argumentti on negatiivinen, tai sitä ei voi muuntaa `real` arvo. 




### <a name="toint"></a>toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### <a name="tolong"></a>tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### <a name="todouble"></a>todouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## <a name="date-and-time"></a>Päivämäärä ja aika


[sitten](#ago) | [dayofmonth](#dayofmonth) | [: n dayofweek](#dayofweek) |  [dayofyear](#dayofyear) |[datepart](#datepart) | [endofday](#endofday) | [endofmonth](#endofmonth) | [endofweek](#endofweek) | [endofyear](#endofyear) | [getmonth](#getmonth)|  [getyear](#getyear) | [nyt](#now) | [startofday](#startofday) | [startofmonth](#startofmonth) | [startofweek](#startofweek) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan) | [weekofyear](#weekofyear)

### <a name="date-and-time-literals"></a>Päivämäärän ja kellonajan literaalien

|||
---|---
**Päivämäärä ja aika**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Ajat ovat aina UTC-aika. Pois jättäminen päivämäärän antaa aika tänään.
`now()`|Nykyisen kellonajan.
`now(`-*aikajakson*`)`|`now()-`*aikajakson*
`ago(`*aikajakson*`)`|`now()-`*aikajakson*
**aikajakson**|
`2d`|kaksi päivää
`1.5h`|1,5 tunti 
`30m`|puolessa tunnissa
`10s`|10 sekuntia
`0.1s`|0,1 toisen
`100ms`| 100 millisekunnin
`10microsecond`|
`1tick`|100 ns
`time("15 seconds")`|
`time("2")`| kaksi päivää
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### <a name="date-and-time-expressions"></a>Päivämäärä ja kellonaika-lausekkeiden

Lauseke |Tulos
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Pienempi
`<=`|Pienempi tai yhtä suuri kuin
`>` |Suurempi
`>=`|Suurempi tai yhtä suuri kuin
`<>`|Eri suuri kuin
`!=`|Eri suuri kuin 




### <a name="ago"></a>sitten

Vähentää tietyn aikajakson-nykyisen kellonajan UTC-aika. Kuten `now()`, tätä toimintoa voi käyttää useita kertoja, ilmoitus- ja Viitattu UTC-aika aika päivitetään samalla tavalla kaikissa toteutuksiin.

**Syntaksi**

    ago(a_timespan)

**Argumentit**

* *a_timespan*: aikavälin vähentää UTC-aika kellonajan (`now()`).

**Palauttaa**

    now() - a_timespan

**Esimerkki**

Kaikki rivit, joilla on kuluneen tunnin aikaleima:

```AIQL

    T | where timestamp > ago(1h)
```

### <a name="datepart"></a>DatePart

    datepart("Day", datetime(2015-12-14)) == 14

Hakee kokonaisluku päivämäärän määritetyn osan.

**Syntaksi**

    datepart(part, datetime)

**Argumentit**

* `part:String`-{"Vuosi", "Kuukausi", "Päivämäärä", "Tunti", "Minuutit", "Toinen", "Millisekunnin", "Microsecond", "Nanosecond"}
* `datetime`

**Palauttaa**

Long, joka edustaa määritetyn osan.


### <a name="dayofmonth"></a>dayofmonth

    dayofmonth(datetime("2016-05-15")) == 15 

Kuukauden päivän järjestysluku.

**Syntaksi**

    dayofmonth(a_date)

**Argumentit**

* `a_date`: A `datetime`.


### <a name="dayofweek"></a>: n DayOfWeek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Kokonaisluku päivinä jälkeen edeltävän sunnuntai, kuin `timespan`.

**Syntaksi**

    dayofweek(a_date)

**Argumentit**

* `a_date`: A `datetime`.

**Palauttaa**

`timespan` Jälkeen keskiyön alussa edeltävän sunnuntai, pyöristetään alaspäin kokonaislukuun päivinä.

**Esimerkkejä**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### <a name="dayofyear"></a>DAYOFYEAR

    dayofyear(datetime("2016-05-31")) == 152 
    dayofyear(datetime("2016-01-01")) == 1 

Vuoden päivän järjestysluku.

**Syntaksi**

    dayofyear(a_date)

**Argumentit**

* `a_date`: A `datetime`.

<a name="endofday"></a><a name="endofweek"></a><a name="endofmonth"></a><a name="endofyear"></a>
### <a name="endofday-endofweek-endofmonth-endofyear"></a>endofday endofweek, endofmonth, endofyear

    dt = datetime("2016-05-23 12:34")

    endofday(dt) == 2016-05-23T23:59:59.999
    endofweek(dt) == 2016-05-28T23:59:59.999 // Saturday
    endofmonth(dt) == 2016-05-31T23:59:59.999 
    endofyear(dt) == 2016-12-31T23:59:59.999 


### <a name="getmonth"></a>getmonth

Pyydä datetime kuukauden numero (1 – 12).

**Esimerkki**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### <a name="getyear"></a>getyear

Pyydä vuoden datetime.

**Esimerkki**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### <a name="now"></a>nyt

    now()
    now(-2d)

Nykyistä UTC-aika aikaa, voit myös siirtymä tietyn aikajakson mukaan. Tätä funktiota voi käyttää useita kertoja, ilmoitus- ja Viitattu kellonaika on sama kaikkien esiintymien.

**Syntaksi**

    now([offset])

**Argumentit**

* *Siirtymä:* A `timespan`, lisätä nykyisen kellonajan UTC-aika. Oletus: 0.

**Palauttaa**

Nykyinen UTC-kellonaika muodossa `datetime`.

    now() + offset

**Esimerkki**

Määrittää välin merkittyä lause tapahtuman jälkeen:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

<a name="startofday"></a><a name="startofweek"></a><a name="startofmonth"></a><a name="startofyear"></a>
### <a name="startofday-startofweek-startofmonth-startofyear"></a>startofday startofweek, startofmonth, startofyear

    date=datetime("2016-05-23 12:34:56")

    startofday(date) == datetime("2016-05-23")
    startofweek(date) == datetime("2016-05-22") // Sunday
    startofmonth(date) == datetime("2016-05-01")
    startofyear(date) == datetime("2016-01-01")



### <a name="todatetime"></a>ToDateTime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34:00")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0]) 
     todatetime(b.c) 

Tarkista, onko merkkijonon kelvollinen päivämäärä:

     iff(notnull(todatetime(customDimensions.myDate)),
         ..., ...)


### <a name="totimespan"></a>ToTimeSpan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)

### <a name="weekofyear"></a>weekofyear

    weekofyear(datetime("2016-05-14")) == 21
    weekofyear(datetime("2016-01-03")) == 1
    weekofyear(datetime("2016-12-31")) == 53

Kokonaisluku-kiertoa mukaan vakio ISO 8601 viikon numeron. Viikon ensimmäinen päivä on sunnuntai ja vuoden ensimmäinen viikko on viikko, joka sisältää kyseisen vuoden ensimmäisen torstai. (Vuoden viimeisen päivän vuoksi voi sisältää jotkin seuraavan vuoden 1 viikko päivän tai ensimmäisen päivän voi sisältää jotkin 52 tai 53 edellisen vuoden viikko.)


## <a name="string"></a>Merkkijono

[countof](#countof) | [Pura](#extract) | [extractjson](#extractjson)  | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [parseurl](#parseurl) | [Korvaa](#replace) | [jakaa](#split) | [strcat](#strcat) | [strlen](#strlen) | [alimerkkijono](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### <a name="string-literals"></a>Merkkijono literaalien

Säännöt ovat samoja kuin JavaScript.

Merkkijonojen on oltava lainausmerkkien, joko yhden tai Lisää lainaus-merkkiä. 

Kenoviiva (`\`) käytetään pääse merkkejä, kuten `\t` (välilehti) `\n` (rivi) ja kirjoittamalla lainausmerkki esiintymät.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double \" quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### <a name="obfuscated-string-literals"></a>Salatut merkkijonon literaalien

Salatut merkkijonon literaalien ovat merkkijonoja, joita Analytics peittää siirrettäessä merkkijono (esimerkiksi silloin, kun jäljitys). Obfuscation prosessin korvaa kaikki obfuscated merkkiä alkamis mukaan (`*`) merkki.

Lomake obfuscated merkkijonoliteraali koskevan `h` tai "H". Esimerkki:

```
h'hello'
h@'world' 
h"hello"
```

### <a name="string-comparisons"></a>Merkkijonojen vertailussa

Operaattori|Kuvaus|Kirjainkoon huomioon|TOSI-Esimerkki
---|---|---|---
`==`|Yhtä suuri kuin |Kyllä| `"aBc" == "aBc"`
`<>` `!=`|Eri suuri kuin|Kyllä| `"abc" <> "ABC"`
`=~`|Yhtä suuri kuin |Ei| `"abc" =~ "ABC"`
`!~`|Eri suuri kuin |Ei| `"aBc" !~ "xyz"`
`has`|Oikea-moottorimuunnokset Include (RHS) on koko termin vasen-moottorimuunnokset Include (LHS)|Ei| `"North America" has "america"`
`!has`|RHS ei ole täydellinen termin LHS|Ei|`"North America" !has "amer"` 
`hasprefix`|RHS on LHS termin etuliite|Ei|`"North America" hasprefix "ame"`
`!hasprefix`|RHS ei ole mitään LHS termin etuliite|Ei|`"North America" !hasprefix "mer"`
`hassuffix`|RHS on loppuliite LHS termin|Ei|`"North America" hassuffix "rth"`
`!hassuffix`|RHS ei ole loppuliite LHS minkä tahansa termi|Ei|`"North America" !hassuffix "mer"`
`contains` | RHS esiintyy LHS alimerkkijonon|Ei| `"FabriKam" contains "BRik"`
`!contains`| RHS ilmene LHS|Ei| `"Fabrikam" !contains "xyz"`
`containscs` | RHS esiintyy LHS alimerkkijonon|Kyllä| `"FabriKam" contains "Kam"`
`!containscs`| RHS ilmene LHS|Kyllä| `"Fabrikam" !contains "Kam"`
`startswith`|RHS on alkuperäinen alimerkkijono, LHS.|Ei|`"Fabrikam" startswith "fab"`
`!startswith`|RHS ei ole alkuperäinen alimerkkijono, LHS.|Ei|`"Fabrikam" !startswith "abr"`
`endswith`|RHS on pääte alimerkkijonon LHS.|Ei|`"Fabrikam" endswith "kam"`
`!endswith`|RHS ei ole LHS pääte alimerkkijonon.|Ei|`"Fabrikam" !endswith "ka"`
`matches regex`|LHS sisältää RHS vastinetta|Kyllä| `"Fabrikam" matches regex "b.*k"`
`in`|Yhtä suuri kuin mihin tahansa elementit|Kyllä|`"abc" in ("123", "345", "abc")`
`!in`|Eri suuri kuin jokin elementit|Kyllä|`"bc" !in ("123", "345", "abc")`

Käytä `has` tai `in` Jos olet koko Sanastollinen - termin tavoitettavuustietojen eli symboli tai aakkosnumeerinen word rajoittamalla muita kuin aakkosnumeerisia merkkejä tai alku tai loppu-kentän. `has`suorittaa nopeampia `contains`, `startswith` tai `endswith`. Nämä kyselyt ensimmäinen suoritetaan nopeammin:

    EventLog | where continent has "North" | count;
    EventLog | where continent contains "nor" | count





### <a name="countof"></a>countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Laskee alimerkkijono merkkijonon esiintymät. Tavallinen merkkijonon hakutuloksia voi peittää; Älä regex vastineita.

**Syntaksi**

    countof(text, search [, kind])

**Argumentit**

* *teksti:* Merkkijono.
* *haku:* Tavallinen merkkijono tai säännöllisen lausekkeen vastaamaan sisällä *tekstiä*.
* *tyyppi:* `"normal"|"regex"` Oletus `normal`. 

**Palauttaa**

Kuinka monta kertaa, etsintämerkkijonon voi yhdistää säilössä. Tavallinen merkkijonon hakutuloksia voi peittää; Älä regex vastineita.

**Esimerkkejä**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (ei 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### <a name="extract"></a>Pura

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Pyydä [säännöllisen lausekkeen](#regular-expressions) vastaavuuden tekstimerkkijonon. Voit myös sen sitten muuntaa poimitun alimerkkijono määritettyihin tyyppi.

**Syntaksi**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumentit**

* *regex:* [Säännöllinen lauseke](#regular-expressions).
* *captureGroup:* Positiivinen `int` vakion, joka ilmaisee sieppaus-ryhmä, johon haluat purkaa. 0 lyhenne koko vastine sääntötunnus säännöllisesti lausekkeessa, 2 tai enemmän myöhemmin sulkeita ensimmäisten "(" Sulje")"-arvoksi 1.
* *teksti:* A `string` etsimiseen.
* *typeLiteral:* Valinnainen tyyppi-literaalimerkit (esimerkiksi `typeof(long)`). Jos, poimitun alimerkkijono muunnetaan tällaista. 

**Palauttaa**

Jos *regex* etsii vastaava *teksti*: alimerkkijono verrataan määritettyihin sieppaus ryhmän *captureGroup*, voit myös muuntaa *typeLiteral*.

Jos ei ole sama tai tietotyyppien muuntamisesta epäonnistuu: `null`. 

**Esimerkkejä**

Esimerkki merkkijonon `Trace` määritelmä etsitään `Duration`. Vastine muunnetaan `real`, valitse kerrottuna aika Vakio (`1s`) niin, että `Duration` tyyppi on `timespan`. Tässä esimerkissä on yhtä kuin 123.45 sekuntia:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

Tässä esimerkissä on sama kuin `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a>
<a name="isnotempty"></a>
<a name="isempty"></a>
### <a name="isempty-isnotempty-notempty"></a>IsEmpty-isnotempty, notempty

    isempty("") == true

TOSI, jos argumentti on tyhjä merkkijono tai null.
Katso myös [isnull](#isnull).


**Syntaksi**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Palauttaa**

Ilmaisee, onko argumentti on tyhjä merkkijono tai isnull.

|x|IsEmpty(x)
|---|---
| "" | TOSI
|"x" | EPÄTOSI
|parsejson("")|TOSI
|parsejson("[]")|EPÄTOSI
|parsejson("{}")|EPÄTOSI


**Esimerkki**


    T | where isempty(fieldName) | count


### <a name="parseurl"></a>parseurl

Jaa URL-Osoitteen osiin.

**Syntaksi**

    parseurl(urlstring)

**Argumentit**

* *urlstring:* URL-OSOITE.

**Palauttaa**

Objektin, joka sisältää osat merkkijonoina.

**Esimerkki**

    parseurl("http://user:pass@contoso.com/icecream/buy.aspx?a=1&b=2#tag")

    {
    "Scheme" : "http",
    "Host" : "contoso.com",
    "Port" : "80",
    "Path" : "/icecream/buy.aspx",
    "Username" : "user",
    "Password" : "pass",
    "Query Parameters" : {"a":"1","b":"2"},
    "Fragment" : "tag"
    }

### <a name="replace"></a>Korvaa

Korvaa kaikki regex vastaa toisen merkkijonon.

**Syntaksi**

    replace(regex, rewrite, text)

**Argumentit**

* *regex:* [Säännöllinen lauseke](https://github.com/google/re2/wiki/Syntax) Etsittävä *teksti*. Se voi olla sieppaaminen ryhmät sulkeissa"(" ")". 
* *uuden esimerkkitekstin:* Minkä tahansa vastaavuus on tehnyt muokkauksia *matchingRegex*korvaava-regex. Käytä `\0` voit viitata koko vastine `\1` sieppaus ensimmäisen ryhmän `\2` ja niin edelleen myöhemmin sieppaus ryhmiä varten.
* *teksti:* Merkkijono.

**Palauttaa**

*tekstin* jälkeen tilalle arvioinnit, *kirjoittaa* *regex* kaikki kohteet. Vastaa päällekkäin.

**Esimerkki**

Tämä lause:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

On seuraavat tulokset:

| x    | Str | Korvataan|
|---|---|---|
| 1    | Luku on 1.000000  | Luku on: 1.000000|
| 2    | Luku on 2.000000  | Luku on: 2.000000|
| 3    | Luku on 3.000000  | Luku on: 3.000000|
| 4    | Luku on 4.000000  | Luku on: 4.000000|
| 5    | Luku on 5.000000  | Luku on: 5.000000|
 



### <a name="split"></a>Jaa

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Jakaa tietyn erottimen mukaan merkkijonosta ja palauttaa merkkijonon matriisin conatined alimerkkijonoja kanssa. Voit myös tiettyjä alimerkkijono voi olla palautetut Jos on olemassa.

**Syntaksi**

    split(source, delimiter [, requestedIndex])

**Argumentit**

* *tietolähteen*: lähde-merkkijono, joka on jaetun annetun erottimen mukaan.
* *erottimen*: erotin, jota käytetään, jotta voit jakaa lähde-merkkijono.
* *requestedIndex*: valinnainen nollasta indeksin `int`. Jos, palautetun merkkijonon matriisi sisältää pyydetty alimerkkijono, jos on olemassa. 

**Palauttaa**

Merkkijonotaulukko, joka sisältää alimerkkijonoja tietyn tietolähteen merkkijonon, joka on erotettu toisistaan annetun erottimen.

**Esimerkkejä**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### <a name="strcat"></a>strcat

    strcat("hello", " ", "world")

Yhdistää välillä 1 – 16 argumentit, jotka on oltava merkkijonoja.

### <a name="strlen"></a>strlen

    strlen("hello") == 5

Merkkijonon pituuden.

### <a name="substring"></a>alimerkkijono

    substring("abcdefg", 1, 2) == "bc"

Pura alimerkkijonon tietyn tietolähteen merkkijonosta alkaen annetusta indeksi. Voit myös pyydetty alimerkkijonon pituus voidaan määrittää.

**Syntaksi**

    substring(source, startingIndex [, length])

**Argumentit**

* *lähde:* Lähdemerkkijono, joka alimerkkijonon otetaan.
* *startingIndex:* Nollasta merkin aloituskohdan pyydetty alimerkkijono.
* *pituus:* Valinnainen parametri, jonka avulla voidaan määrittää alimerkkijonon pyydetty merkkien määrän. 

**Palauttaa**

Alimerkkijono määritetystä merkkijonosta. Alimerkkijonon alkaa startingIndex (nollasta) merkin sijainnin ja jatkaa loppuun merkkijono tai pituus-merkkiä, jos määritetty.

**Esimerkkejä**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### <a name="tolower"></a>tolower

    tolower("HELLO") == "hello"

Muuntaa tekstimerkkijonon pienet kirjaimet.

### <a name="toupper"></a>toupper

    toupper("hello") == "HELLO"

Muuntaa tekstimerkkijonon isot kirjaimet.



### <a name="guids"></a>GUID

    guid(00000000-1111-2222-3333-055567f333de)


## <a name="arrays-objects-and-dynamic"></a>Matriisien, objektien ja dynaaminen

[literaalien](#dynamic-literals) | [liittyvät](#casting-dynamic-objects) | [operaattorit](#operators) | [antaa lauseet](#dynamic-objects-in-let-clauses)
<br/>
[arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [alueen](#range) | [treepath](#treepath) | [todynamic](#todynamic) | [zip](#zip)


Näin sovelluksen tiedot-poikkeuksen kyselyn tuloksen. Arvo `details` on matriisikaava.

![](./media/app-insights-analytics-reference/310.png)

**Indeksoinnin:** Indeksi matriisien ja objekteja, kuten JavaScript:

    exceptions | take 1
  	| extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Mutta `arraylength` ja muut Analytics-Funktiot (ei ".length"!)

**Luovan** Joissakin tapauksissa on joka elementtiä, joka purkaa objektista, koska sen tyyppiä voi vaihdella. Esimerkiksi `summarize...to` on tietyntyyppinen:

    exceptions 
  	| summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
  	| summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literaalien** Luo eksplisiittinen matriisi tai ominaisuuden kantolaukku objekti, kirjoita se JSON merkkijonon ja cast:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand:** Jos haluat noutaa toisistaan objektin ominaisuudet eri riveille, käytä mvexpand:

    exceptions | take 1 
  	| mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-reference/410.png)


**treepath:** Jos haluat etsiä kaikki polut monimutkaisia objekti:

    exceptions | take 1 | project timestamp, details 
  	| extend path = treepath(details) 
  	| mvexpand path


![](./media/app-insights-analytics-reference/420.png)

**buildschema:** Voit etsiä pienin rakenne, joka voi osallistua lausekkeen taulukon kaikki arvot seuraavasti:

    exceptions | summarize buildschema(details)

Tulos:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Huomaa, että `indexer` merkitään, joissa kannattaa käyttää numeerinen. Tämän rakenteen joitakin kelvollisia polkuja olla (olettaen että alueella on esimerkki näiden indeksien):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### <a name="array-and-object-literals"></a>Matriisi ja objektien literaalien

Voit luoda dynaamisia literaalimerkit käyttämällä `parsejson` (alias `todynamic`) kanssa JSON merkkijono argumentti:

* `parsejson('[43, 21, 65]')`-lukumatriisia
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`-Dynaaminen tyyppi, joka sisältää luvun yksittäisen arvon
* `parsejson('"21"')`-Dynaaminen tyyppi, joka sisältää merkkijonon arvon

Huomaa, että toisin kuin JavaScript-JSON valtuuttaa lainausmerkkejä käyttö (`"`) merkkijonojen ympärille. Tämän vuoksi on yleensä helpompi tarjous JSON-koodatun merkkijonon käyttäminen yhden tarjoukset literaalien (`'`).

Tässä esimerkissä Luo dynaamisen arvon ja käyttää sitten sen kentät:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


### <a name="dynamic-object-functions"></a>Dynaaminen objekti-Funktiot

|||
|---|---|
| *arvo* `in` *matriisi*| TRUE, jos *matriisin* elementti, == *arvo*<br/>`where City in ('London', 'Paris', 'Rome')`
| *arvo* `!in` *matriisi*| TOSI, jos ei ole *matriisin* elementti, == *arvo*
|[`arraylength(`matriisi`)`](#arraylength)| Tyhjä, jos se ei ole matriisi
|[`extractjson(`PATH-, objekti`)`](#extractjson)|Siirry objektiksi käyttää polkua.
|[`parsejson(`lähde`)`](#parsejson)| Muuttuu JSON merkkijonon dynamic objekti.
|[`range(`Lähettäjä vastaanottaja-vaihe`)`](#range)| Matriisin arvoista
|[`mvexpand`listColumn](#mvexpand-operator) | Kopioi rivi jokaisen luettelon määritetyn solun arvon.
|[`summarize buildschema(`sarakkeen`)`](#buildschema) |Johtaa sarakkeen sisällön tyyppi-rakenne
|[`summarize makelist(`sarakkeen`)`](#makelist)| Objektiksi rivien ryhmiä ja siirtää arvot sarakkeen matriisi.
|[`summarize makeset(`sarakkeen`)`](#makeset) | Objektiksi rivien ryhmiä ja siirtää arvot sarakkeen matriisin ilman monistaminen.

### <a name="dynamic-objects-in-let-clauses"></a>Anna lauseet dynaaminen objektit


[Anna lauseet](#let-clause) kaupan dynaamisia arvoja merkkijonoina, joten nämä kaksi lauseet vastaavat, ja kumpikin on `parsejson` (tai `todynamic`) ennen kuin niitä käytetään:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### <a name="arraylength"></a>arraylength

Dynaamisen matriisin elementtien määrä.

**Syntaksi**

    arraylength(array)

**Argumentit**

* *matriisi:* A `dynamic` arvo.

**Palauttaa**

*Matriisin*elementtien määrä tai `null` Jos *taulukko* ei ole taulukko.

**Esimerkkejä**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### <a name="extractjson"></a>extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Määritetyn elementin ulos polku-lausekkeen avulla JSON-tekstin hakeminen Voit myös muuntaa tietyntyyppinen poimitun merkkijonon.


**Syntaksi**

```

    string extractjson(jsonPath, dataSource) 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))
```


**Palauttaa**

Tämä funktio suorittaa JsonPath kyselyn tietolähde, jossa on kelvollinen JSON-merkkijono, voit myös kyseisen arvon muuntaminen toiseen tyypin mukaan kolmas argumentti kyselyjä.



**Esimerkki**

[Hakasulje]-merkintätapaa ja piste-merkintätapaa vastaavat:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Vihjeitä tehokkuuden parantamiseksi**

* Lisää where-lauseet ennen käyttäminen`extractjson()`
* Harkitse käyttämistä säännöllisen lausekkeen vastine kanssa [Pura](#extract) . Tämä toimii hyvin nopeammin ja on voimassa, jos JSON on valmistettu mallista.
* Käytä `parsejson()` Jos haluat poimia JSON useamman kuin yhden arvon.
* Harkitse dynaamisen jäsentää osoitteessa nieltynä ilmoittamalla sarakkeen tyypin JSON.

### <a name="json-path-expressions"></a>JSON polku lausekkeet

|||
|---|---|
|`$`|Pääobjektin|
|`@`|Nykyinen objekti|
|`[0]`|Taulukon alaindeksi|
|`.`tai`[0]` | Lapsen|

*(Microsoft ei tällä hetkellä Toteuta yleismerkkejä, rekursio, unionin tai sektoreiden.)*




### <a name="parsejson"></a>parsejson

Tulkitsee `string` [JSON-arvo](http://json.org/)) ja palauttaa arvon kuin `dynamic`. Se on käyttämällä `extractjson()` milloin haluat purkaa useamman kuin yhden elementin JSON koostetun objektin.

**Syntaksi**

    parsejson(json)

**Argumentit**

* *json:* JSON-asiakirja.

**Palauttaa**

Objektin tyyppi `dynamic` *json*määrittämää.

**Esimerkki**

Seuraavassa esimerkissä, kun `context_custom_metrics` on `string` , että näyttää tältä: 

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

ja valitse Seuraava osa noutaa arvon `duration` paikka objektin ja että se hakee kaksi paikkojen `duration.value` ja  `duration.min` (`118.0` ja `110.0`,).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### <a name="range"></a>alue

`range()` Funktion (halua sekoittaa `range` operaattori) luo dynaamisen matriisin pitämällä arvosarjojen tasaisesti välein.

**Syntaksi**

    range(start, stop, step)

**Argumentit**

* *aloittaminen:* Tuloksena olevan taulukon ensimmäinen osa arvo. 
* *lopettaminen:* Tuloksena olevan taulukon viimeinen osa arvo tai pienimmän arvon, joka on suurempi kuin viimeinen osa tuloksena olevan matriisin ja kokonaisluku- *Aloita* *Vaihe* kerrannaiseen.
* *vaihe:* Kahden matriisin peräkkäisten osien välinen ero.

**Esimerkkejä**

Seuraava esimerkki palauttaa `[1, 4, 7]`:

```AIQL
range(1, 8, 3)
```

Seuraava esimerkki palauttaa vuoden 2015 kaikki päivien pitämällä:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### <a name="todynamic"></a>todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Muuntaa tekstimerkkijonon dynaamisen arvon.

### <a name="treepath"></a>treepath

    treepath(dynamic_object)

Luetteloi kaikki polku lausekkeita, jotka tunnistaa lehtiä dynaamisia objekteja. 

**Palauttaa**

Matriisin polku lausekkeita.

**Esimerkkejä**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Huomaa, että "[0]" ilmaisee matriisin, mutta ei määritä tiettyyn polkuun käyttämä indeksi.

### <a name="zip"></a>Postinumero

    zip(list1, list2, ...)

Yhdistää tietyt luettelot monikot luettelo.

* `list1...`: Arvoluettelon

**Esimerkkejä**

    zip(parsejson('[1,3,5]'), parsejson('[2,4,6]'))
    => [ [1,2], [3,4], [5,6] ]

    
    zip(parsejson('[1,3,5]'), parsejson('[2,4]'))
    => [ [1,2], [3,4], [5,null] ]


### <a name="names"></a>Nimet

Nimet voivat olla enintään 1 024 merkin pituinen. Ne salasanoissa eri merkeiksi ja voi olla kirjaimia, numeroita ja alaviivoja (`_`). 

Tarjouksen nimi käyttämällä ["... '] or [" ... "] muiden näppäimistöstä puuttuvien merkkien tai käytä avainsanaa nimen. Esimerkki:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|["polku\\file\n\'x\'"] | Käytä \ pääse merkit|
|["d-e.=/f#\n"] | |
|[@'path\file'] | Ei ole kokonaiseen - \ on literal|
|[@"\now& sitten\"] | |
|[Jos] | Kielen avainsana nimen|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


