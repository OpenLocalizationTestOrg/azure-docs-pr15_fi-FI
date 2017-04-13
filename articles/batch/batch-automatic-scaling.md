<properties
    pageTitle="Automaattisesti asteikko Laske Azure erä resurssivarantoon solmujen | Microsoft Azure"
    description="Ota käyttöön Automaattinen skaalaus cloud resurssivarantoon Säädä dynaamisesti ryhmän Laske näkyvien solmujen määrän."
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="batch"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="multiple"
    ms.date="10/14/2016"
    ms.author="marsma"/>

# <a name="automatically-scale-compute-nodes-in-an-azure-batch-pool"></a>Skaalaa automaattisesti Azure erä resurssivarantoon solmujen suorittaminen

Kanssa Automaattinen skaalaus Azure erä palvelun voit dynaamisesti lisääminen tai poistaminen Laske solmujen resurssivarantoon, joka määritetään parametrien perusteella. Voit tallentaa aikaa ja rahaa mahdollisesti säätämällä automaattisesti summan käyttää sovelluksen--Laske tehon Lisää solmujen oman projektin tehtävien vaatimukset kasvu ja poistaa niitä, kun ne pienentää.

Voit ottaa käyttöön Laske solmujen resurssivarantoon Automaattinen skaalaus liittämällä sitä *Automaattinen skaalaus-kaava* , jonka olet määrittänyt, kuten [PoolOperations.EnableAutoScale] kanssa[ net_enableautoscale] menetelmä [Erä .NET](batch-dotnet-get-started.md) -kirjastossa. Erän palvelun käyttää seuraavaa kaavaa määrittämiseen tarvittavat suorittaa havainnollistamiseen Laske solmujen määrän. Erän reagoi palvelun arvot, jotka kerätään säännöllisesti tietomallia ja muuttuu kaavan perusteella määritettäviä määritetyn resurssivarantoon Laske näkyvien solmujen määrän.

Voit ottaa automaattisen skaalauksen resurssivarantoon luotaessa tai olemassa olevan ryhmän. Voit myös muuttaa Valitse varanto on "Automaattinen skaalaus" käytössä olevan kaavan. Erän mahdollistaa kaavojen laskeminen ennen määritteleminen jakavat sekä automaattisen skaalauksen suoritetaan tilaa.

## <a name="automatic-scaling-formulas"></a>Automaattinen skaalauksen kaavat

Automaattisen skaalauksen kaava on merkkijonoarvo määrittämiisi arvoihin, joka sisältää vähintään yhden lauseita ja määritetään resurssivarantoon [autoScaleFormula] [ rest_autoscaleformula] elementti (erä REST) tai [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] ominaisuuden (erä .NET). Kun määritetty resurssivarantoon, erä-palvelu käyttää kaavan määrittää ryhmän Laske solmujen kohde määrän käsittely (Lisätietoja myöhemmin välien) seuraavan välin. Kaavan merkkijono ei voi ylittää 8 Kilotavun kokoinen, voi olla enintään 100 lausekkeita, jotka on erotettu toisistaan puolipisteillä ja voivat sisältää rivin- ja kommentit.

Voit ajatella automaattisen skaalauksen kaavojen käyttämistä erän Automaattinen skaalaus "kieli". Kaava-lauseet ovat vapaa muotoiltu lausekkeita, jotka voivat sisältää palvelun määrittämät muuttujat (erä palvelun määrittämät muuttujat) ja käyttäjän määrittämät muuttujat (muuttujat, jotka määrität). He voivat tehdä nämä arvot eri toimintoja sisäiset tyypit, operaattoreita ja funktioiden avulla. Esimerkiksi ilmoitus voi seuraavanlainen:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Kaavoja sisältävät yleensä useita lausekkeita, jotka suorittavat toimintoja arvoja, jotka saadaan edellisen lauseita. Esimerkiksi on saatava arvo `variable1`, siirtää täytä funktion `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Kaavassa lauseiden kanssa lähinnä, suorittaminen, jotka altaan pitäisi olla skaalattu--solmujen määrän vuoroon **kohde** **erillinen solmujen**määrän. Numero voi olla suurempi, pienet- tai sama kuin altaan solmujen määrä. Erän arvioi resurssivarantoon Automaattinen skaalaus kaavan tietyn ajan ([Automaattinen skaalauksen väliajoin](#automatic-scaling-interval) käsitellään alla). Valitse se säätää kohde, Automaattinen skaalaus-kaava määrittää, milloin arvioinnin lukuun varannon solmujen määrän.

Lyhyt Esimerkki kahden rivin Automaattinen skaalaus tämä kaava määrittää, että näkyvien solmujen määrän muuttaa aktiiviset tehtävät, enintään 10 Laske solmujen määrän mukaan:

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

Tämän artikkelin seuraavaan muutamia osissa käsitellään eri kohteet, jotka muodostavat Automaattinen skaalaus-kaavoista, kuten muuttujat, operaattorit, toiminnot ja funktioita. Löydät, miten voit hankkia eri Laske resurssi ja tehtävä arvot erä kuluessa. Voit käyttää näitä arvot Säädä älykkäästi lisääminen resurssivarantoon solmu Laske resurssien käyttö- ja tehtävän tilan perusteella. Opit sitten siitä, miten voit käyttää kaavassa ja ottaa resurssivarantoon Automaattinen skaalaus käyttämällä erä REST- ja .NET-ohjelmointirajapinnan. Microsoft valmis muutama Esimerkki kaavoja.

> [AZURE.IMPORTANT] Azure erä tileille on rajoitettu enimmäismäärä sydämiä (ja näin ollen Laske solmujen), jonka avulla voidaan käsittelyä varten. Erän palvelun luo solmujen vain core rajoituksen ylöspäin. Sen vuoksi se saattaa ulotu kohde Laske näkyvien solmujen määrän, joka on määritetty kaavan avulla. Lisätietoja tarkastelemista ja lisää tili-kiintiön on [kiintiöiden ja Azure erä palvelun rajoitukset](batch-quota-limit.md) .

## <a name="variables"></a>Muuttujat

Voit käyttää sekä **palvelun määrittämiä** ja **käyttäjän määrittämät** muuttujat Automaattinen skaalaus-kaavoissa. Palvelun määrittämät muuttujat ovat erä – palvelu: n valmista osa on luku-ja kirjoitusoikeudet ja jotkin ovat vain luku-tilassa. Käyttäjän määrittämät muuttujat ovat muuttujat, *Voit* määrittää. Kahden rivin Esimerkki kaavan edellä `$TargetDedicated` on palvelun määrittämä muuttuvat, kun `$averageActiveTaskCount` on Käyttäjäkohtainen muuttuja.

Seuraavissa taulukoissa Näytä sekä luku-ja kirjoitusoikeudet ja vain luku-muuttujat, jotka on määritetty erä-palvelu.

Voit **saada** ja **Määritä** palvelun määrittämiä muuttujia hallittavan resurssivarantoon Laske näkyvien solmujen määrän arvoja:

| Luku-ja kirjoitusoikeudet palvelun määrittämät muuttujat | Kuvaus |
| --- | --- |
| $TargetDedicated | Altaan varten **varattu Laske solmujen** **kohde** numero. Tämä on, jotka altaan pitäisi olla skaalattu Laske solmujen määrän. Se on "kohde" luku, koska se on mahdollista resurssivarantoon ei saavuttamiseksi kohde näkyvien solmujen määrän. Näin voi käydä, jos näkyvien solmujen määrän kohde on muokannut uudelleen myöhemmin Automaattinen skaalaus-arviointi ennen altaan on muutettu ensimmäisen kohteen. Se voi esiintyä myös, jos erä tilin solmu tai core kiintiön on saavutettu ennen näkyvien solmujen määrän kohde on saavutettu. |
| $NodeDeallocationOption | Toiminto, joka seuraa, kun Laske solmujen poistetaan resurssivarantoon. Mahdolliset arvot ovat:<ul><li>**requeue**--päättyy tehtävät välittömästi ja sijoittaa ne uudelleen käyttöön työn jonossa niin, että ne uudelleen.<li>**Lopeta**--päättyy tehtävät välittömästi ja poistaa ne työn jonossa.<li>**taskcompletion**--odottaa käynnissä olevat tehtävät loppuun ja poistaa sitten olevilla solmu.<li>**retaineddata**--odottaa kaikki paikalliset tehtävän säilyttää tiedot solmun puhdistettava ennen kuin poistat solmu olevilla.</ul> |

Voit **saada** arvon palvelun määrittämiä muuttujia voi tehdä muutoksia, jotka perustuvat arvot erä-palvelusta.

| Vain luku-palvelun määrittämät muuttujat | Kuvaus |
| --- | --- |
| $CPUPercent | Suorittimen käyttö prosentteina. |
| $WallClockSeconds | Kulutettu sekuntien määrän. |
| $MemoryBytes | Käyttää megatavua keskimääräinen määrä. |
| $DiskBytes | Keskimääräinen määrän gigatavuina käytetään paikallisen levyille. |
| $DiskReadBytes | Lue tavujen määrän. |
| $DiskWriteBytes | Kirjoitettu tavujen määrän. |
| $DiskReadOps | Lue levyn toiminnot suoritetaan määrä. |
| $DiskWriteOps | Kirjoita levyn toiminnot suoritetaan määrä. |
| $NetworkInBytes | Saapuvien tavujen määrän. |
| $NetworkOutBytes | Lähtevien tavujen määrän. |
| $SampleNodeCount | Laske solmujen määrän. |
| $ActiveTasks | Aktiivinen tehtävien määrä. |
| $RunningTasks | Monta tehtävää käynnissä. |
| $PendingTasks | $ActiveTasks ja $RunningTasks summa. |
| $SucceededTasks | Monta tehtävää, että suoritettu. |
| $FailedTasks | Monta tehtävää, joka epäonnistui. |
| $CurrentDedicated | Erillinen nykyisen määrän laskemiseen solmujen. |

> [AZURE.TIP] Vain luku-palvelun määrittämät muuttujat, jotka ovat edellä ovat *objektit* , joiden avulla voidaan käyttää liittyvät tiedot eri tavalla. Lisätietoja on kohdassa [Hae mallitiedot](#getsampledata) alapuolella.

## <a name="types"></a>Tyypit

Nämä **tyypit** tuetaan kaavassa.

- Kaksinkertainen
- doubleVec
- doubleVecList
- merkkijono
- aikaleima--aikaleima on koostetun rakennetta, joka sisältää seuraavat jäsenet:

    - vuoden
    - kuukausi (1 – 12)
    - päivä (1 – 31)
    - Viikonpäivä (numero, esimerkiksi 1 maanantain muodossa)
    - Tunnit (24 tunnin lukumuotoilun, kuten 13 tarkoittaa, että 1 PM)
    - minuutit (00 – 59)
    - Second (00 – 59)
- timeinterval

    - TimeInterval_Zero
    - TimeInterval_100ns
    - TimeInterval_Microsecond
    - TimeInterval_Millisecond
    - TimeInterval_Second
    - TimeInterval_Minute
    - TimeInterval_Hour
    - TimeInterval_Day
    - TimeInterval_Week
    - TimeInterval_Year

## <a name="operations"></a>Toiminnot

Näitä **toimintoja** sallita tyypit, jotka on lueteltu yläpuolella.

| Toiminto                             | Tuetut operaattorit   | Tulostyyppi   |
| ------------------------------------- | --------------------- | ------------- |
| Kaksinkertainen *operaattori* kaksinkertainen              | +, -, *, /            | Kaksinkertainen            |
| Kaksinkertainen *operaattori* timeinterval        | *                     | timeinterval      |
| doubleVec *operaattori* kaksinkertainen           | +, -, *, /            | doubleVec         |
| doubleVec *operaattori* doubleVec        | +, -, *, /            | doubleVec         |
| timeinterval *operaattori* kaksinkertainen        | *, /                  | timeinterval      |
| timeinterval *toimijan* timeinterval  | +, -                  | timeinterval      |
| timeinterval *operaattori* aikaleima     | +                     | aikaleima         |
| aikaleima *operaattori* timeinterval     | +                     | aikaleima         |
| aikaleima *operaattori* aikaleima        | -                     | timeinterval      |
| Kaksinkertainen *operaattori*                      | -, !                  | Kaksinkertainen            |
| *operaattorien*timeinterval                | -                     | timeinterval      |
| Kaksinkertainen *operaattori* kaksinkertainen              | < < =, ==, > =, >,! =  | Kaksinkertainen            |
| merkkijono *operaattorin* merkkijono              | < < =, ==, > =, >,! =  | Kaksinkertainen            |
| aikaleima *operaattori* aikaleima        | < < =, ==, > =, >,! =  | Kaksinkertainen            |
| timeinterval *toimijan* timeinterval  | < < =, ==, > =, >,! =  | Kaksinkertainen            |
| Kaksinkertainen *operaattori* kaksinkertainen              | & & & #124; & #124;      | Kaksinkertainen            |

Kun testataan double Kolmiarvoinen operaattorilla (`double ? statement1 : statement2`), ei ole nolla on **Tosi**ja nolla on **Epätosi**.

## <a name="functions"></a>Funktiot

Näitä ennalta määritetyt **Toiminnot** ovat käytettävissä voi käyttää automaattista skaalauksen kaavan määrittäminen.

| Funktio                          | Palautustyyppi   | Kuvaus
| --------------------------------- | ------------- | --------- |
| AVG(doubleVecList)                | Kaksinkertainen        | Palauttaa kaikkien arvojen keskiarvo doubleVecList.
| Len(doubleVecList)                | Kaksinkertainen        | Palauttaa vektorin, jotka on luotu doubleVecList pituuden.
| LG(Double)                        | Kaksinkertainen        | Palauttaa log double kantaluku 2.
| LG(doubleVecList)                 | doubleVec     | Palauttaa componentwise log doubleVecList kantaluku 2. Parametrin vec(double) erikseen on siirrettävä. Muussa tapauksessa oletetaan kaksinkertainen lg(double)-versio.
| ln(Double)                        | Kaksinkertainen        | Palauttaa double luonnollinen log.
| ln(doubleVecList)                 | doubleVec     | Palauttaa componentwise log doubleVecList kantaluku 2. Parametrin vec(double) erikseen on siirrettävä. Muussa tapauksessa oletetaan kaksinkertainen lg(double)-versio.
| log(Double)                       | Kaksinkertainen        | Palauttaa log double kantaluku 10.
| log(doubleVecList)                | doubleVec     | Palauttaa componentwise log doubleVecList kantaluku 10. Yksittäinen kaksinkertainen parametri vec(double) erikseen on siirrettävä. Muussa tapauksessa oletetaan kaksinkertainen log(double)-versio.
| Max(doubleVecList)                | Kaksinkertainen        | Palauttaa suurimman arvon doubleVecList.
| MIN(doubleVecList)                | Kaksinkertainen        | Palauttaa pienimmän arvon doubleVecList.
| NORM(doubleVecList)               | Kaksinkertainen        | Palauttaa kahden Normaali, jotka on luotu doubleVecList vektorin.
| prosenttipiste (doubleVec-v kaksinkertainen p) | Kaksinkertainen        | Palauttaa vektorin v prosenttipiste-elementtiä.
| Funktiot RAND()                            | Kaksinkertainen        | Palauttaa satunnaisen 0.0 ja 1.0 välillä.
| Range(doubleVecList)              | Kaksinkertainen        | Palauttaa min- ja enimmäisarvot välinen ero doubleVecList.
| STD(doubleVecList)                | Kaksinkertainen        | Palauttaa doubleVecList arvojen otoksen keskihajonta.
| Stop()                            |               | Lopettaa automaattisen skaalauksen poistaminen lausekkeen arvioinnista.
| SUM(doubleVecList)                | Kaksinkertainen        | Palauttaa kaikki doubleVecList osat.
| aika (dateTime merkkijono = "")          | aikaleima     | Palauttaa nykyisen kellonajan, jos ei ole parametreja välitetään aikaleiman tai aikaleiman dateTime-merkkijonon, jos se siirretään. Tuetut dateTime-muodot ovat W3C DTF ja RFC 1123.
| Val (doubleVec-v kaksinkertainen i)        | Kaksinkertainen        | Palauttaa elementti, joka on tehty i vektorista v aloitus indeksi on nolla-arvon.

Jotkin funktiot, jotka edellä olevassa taulukossa on kuvattu hyväksyä luettelon argumenttina. Pilkuilla erotettu luettelo on *Kaksinkertainen* ja *doubleVec*. Esimerkki:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

*DoubleVecList* arvo muunnetaan yhden *doubleVec* arviointi ennen. Esimerkiksi jos `v = [1,2,3]`, soittaminen `avg(v)` on sama kuin soitat `avg(1,2,3)`. Soittavan `avg(v, 7)` on sama kuin soitat `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Hanki mallitiedot

Automaattinen skaalaus-Kaavat toimivat arvot tiedoille (esimerkit), jotka on annettu erä-palvelu. Kaavan suurenee tai pienenee ryhmän koko se hakee palvelusta arvojen perusteella. Palvelun määrittämät muuttujat yllä kuvattujen ovat objektit, joissa on eri tavat käyttämään tietoja, jotka on liitetty objekti. Esimerkiksi seuraava lauseke näyttää pyynnön, saat suorittimen käyttö viimeisen viisi minuuttia:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Menetelmä | Kuvaus |
| --- | --- |
| GetSample() | `GetSample()` Menetelmä palauttaa vektorista tietomallia.<br/><br/>Esimerkki on 30 sekuntia nykyarvo arvot tiedoista. Toisin sanoen näytteiden saadaan 30 sekunnin välein. Mutta, kuten alla on viive kun otoksen kerätään ja kun se on käytettävissä kaavan välillä. Siten tietyn ajanjakson kaikki objektit eivät ole käytettävissä arvioitavaksi kaavan.<ul><li>`doubleVec GetSample(double count)`<br/>Määrittää näytteiden Hanki uusin mallit, jotka on kerätty määrä.<br/><br/>`GetSample(1)`Palauttaa viimeisen käytettävissä otosten. Varten arvot, kuten `$CPUPercent`, mutta tämä ei voi käyttää koska on mahdotonta, *Kun* otosten allekirjoitukseen. Sinun on ehkä viimeisimmät tai järjestelmän ongelmien vuoksi voi olla paljon vanhempia. On parempi tällaisissa tapauksissa käytettävä aikaväli alla kuvatulla tavalla.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Määrittää ajanjakson tietojen keräämistä varten. Vaihtoehtoisesti myös määrittää esimerkkejä, joiden on oltava käytettävissä pyydetty päivämäärävälin prosentteina.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`20 esimerkkejä, joiden palauttaa, jos kaikki esimerkit viimeisen kymmenen minuuttia ovat näkyvissä CPUPercent historiatietoihin. Viime hetken historian ei ole käytettävissä, jos kuitenkin vain 18 näytteiden palautetaan. Tässä tapauksessa:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`virhetilanne, koska vain 90 prosenttia mallit ovat käytettävissä.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`toiminto onnistuu.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Määrittää tietyn kellonaikavälin keräämällä tietoja alkamisaika ja päättymisaika.<br/><br/>Kuten edellä mainittiin, ei kun otoksen kerätään ja kun se on käytettävissä kaavan viive. Tämä on otettava huomioon käytettäessä `GetSample` menetelmää. Katso `GetSamplePercent` alapuolella.|
| GetSamplePeriod() | Palauttaa kauden näytteiden, jotka on otettu historiallisten otoksen tietojoukon. |
| Count() | Palauttaa näytteiden kokonaismäärän metrisillä historia. |
| HistoryBeginTime() | Palauttaa vanhimmasta lisätiedot käytettävissä näyte aikaleimaa. |
| GetSamplePercent() |Palauttaa objektit, jotka ovat käytettävissä tietyllä aikavälillä prosentteina. Esimerkki:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Koska `GetSample` menetelmä epäonnistuu, jos prosentti näytteiden palautetaan on vähemmän kuin `samplePercent` määritetty, voit käyttää `GetSamplePercent` menetelmä, tarkista ensin. Sitten voit suorittaa toisen toiminnon, jos riittämätön mallit ovat näkyvissä, ilman pysäyttäminen skaalauksen automaattisen laskennan.|

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Mallit, otoksen prosentti ja *GetSample()* -menetelmällä

Automaattinen skaalaus-kaavan core-toiminto on tehtävän ja resurssin metrisillä tietojen saamiseksi ja säädä ryhmän koko tietojen perusteella. Näin ollen on tärkeää on tekevän, miten Automaattinen skaalaus-kaavat voivat käsitellä arvot tietoja tai "näytteiden."

**Esimerkkejä, joiden**

Erä-palvelun säännöllisesti tulee *näytteiden* tehtävän ja resurssin arvot, ja tekee niistä käytettävissä Automaattinen skaalaus-kaavat. Näitä esimerkkejä tallennetaan 30 sekunnin välein erä-palvelu. On kuitenkin yleensä joitakin viive, joka aiheuttaa viiveen välillä kun näytteiden on tallennettu, ja kun ne ovat saatavilla, (ja voi lukea) Automaattinen skaalaus-kaavoja. Lisäksi vuoksi eri tekijöistä, kuten verkossa tai infrastruktuurin muita ongelmia, näytteiden saattaa ei tallennettu tietyn ajan. Tämä aiheuttaa "puuttuu" esimerkit.

**Esimerkki prosentti**

Kun `samplePercent` siirretään `GetSample()` menetelmää tai `GetSamplePercent()` menetelmää kutsutaan, vertailun objektit, jotka on tallennettu erä-palvelun yhteensä *mahdollista* määrän ja määrän, jotka ovat todella *käytettävissä* Automaattinen skaalaus-kaava viittaa "prosenttia".

Katsotaan 10 minuutin aikajakson esimerkkinä. Koska mallit tallennetaan 30 sekunnin välein, 10 minuutin-aikajakson sisällä enimmäismäärän objektit, jotka on tallennettu mukaan erä olisi 20 näytteiden (2 minuutissa). Kuitenkin vuoksi raportoinnin järjestelmä tai muita ongelman Azure infrastruktuurin luontaisen viive voi olla vain 15 objektit, jotka ovat käytettävissä Automaattinen skaalaus-kaavan lukemista varten. Tämä tarkoittaa, että 10 minuutin ajanjakson vain **75 prosenttia** objektit, jotka on tallennettu kokonaismäärän ovat todella käytettävissä kaavaan.

**GetSample() ja Esimerkki**

Automaattinen skaalaus-kaavat ovat siirtymällä kasvava ja pienentäminen oman jakavat--solmujen lisäämällä tai poistamalla solmujen. Solmujen kustannus rahaa, koska haluat varmistaa, että kaavojen käyttää älykkäät menetelmä analyysin, joka perustuu riittävästi tietoja. Tästä syystä suosittelemme tykkäysten tyyppi-analyysin käyttäminen kaavoissa. Tämä suurennus ja pienennys oman jakavat kerättyjen näytteiden *alueen* perusteella.

Voit tehdä `GetSample(interval look-back start, interval look-back end)` palauttaa **vector** näytteiden:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Kun yllä arvioi Siirtoerän, se palauttaa näytteiden solualueen arvojen siirtyy. Esimerkki:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Kun olet kerännyt näytteiden vektorista, voit käyttää funktioita, kuten `min()`, `max()`, ja `avg()` saada kuvaava arvot kerättyjen alueelta.

Lisäsuojauksen voit pakottaa kaavan arviointi *epäonnistuu* , jos pienempi kuin otoksen prosenttimäärä on tietyn ajanjakson. Voit pakottaa kaavan arviointi voi epäonnistua, kun pyydät erä lakkaa edelleen arvioinnin kaavan, jos tietty prosenttimäärä näytteiden ei ole käytettävissä--ja ryhmän koko ei muutu tehdään. Määritä tarvittavat prosentteina näytteiden arvioitavaksi onnistuu, määritä se kolmas parametri `GetSample()`. Tässä on määritetty 75 prosenttia näytteiden vaatimus:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

On myös tärkeää Esimerkki käytettävyyden, Määritä aina kanssa ulkoasun Edellinen aloitusaika, joka on vanhempi kuin minuutin ajan mainittiin viive vuoksi. Tämä johtuu siitä, että kestää noin minuutin näytteiden leviäminen – järjestelmä-alueen niin esimerkit `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` usein ei ole käytettävissä. Uudelleen, voit käyttää prosentti-parametri `GetSample()` pakottaa tietyn otoksen prosentti vaatimus.

> [AZURE.IMPORTANT] On **suositeltavaa** , että voit * *välttää käyttäisit *vain* - `GetSample(1)` -Automaattinen skaalaus-kaavat **. Tämä johtuu siitä, `GetSample(1)` lukee olennaisesti erä-palveluun "Anna minulle on, riippumatta siitä, kuinka kauan sitten olet saanut edellisen näytteen." Koska se sisältää vain yhden ja lähettää vanhoja otoksen, edustaja viimeisimmät tehtävän tai resurssin tilan suurempi kuva ei ehkä. Jos käytät `GetSample(1)`, varmista, että se on osa suurempi lauseen ja ei vain tiedot-kohdan, joka perustuu kaavaan.

## <a name="metrics"></a>Arvot

Voit käyttää **resurssi** ja **tehtävä** mittarit, kun määrität kaavan. Voit säätää erillinen solmujen arvot tiedot, jotka hankkiminen ja arvioi perusteella varannon kohde määrän. Katso [muuttujat](#variables) yläpuolella lisätietoja kunkin arvo.

<table>
  <tr>
    <th>Arvo</th>
    <th>Kuvaus</th>
  </tr>
  <tr>
    <td><b>Resurssi</b></td>
    <td><p><b>Resurssin arvot</b> perustuvat suorittimen ja muistin kaistanleveyden käytön Laske solmujen sekä näkyvien solmujen määrän.</p>
        <p> Palvelun määrittämiä muuttujia on hyötyä muutosten tekeminen solmu määrän perusteella:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Palvelun määrittämiä muuttujia on hyötyä muutosten tekeminen solmu resurssien käyttö perusteella:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Tehtävä</b></td>
    <td><p><b>Tehtävän arvot</b> tehtävien aktiivinen, kuten tilan perusteella Odottava, ja valmis. Seuraavat palvelun määrittämät muuttujat on hyötyä ryhmän koko muutosten mukaan tehtävän arvot:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Kirjoita Automaattinen skaalaus-kaava

Automaattinen skaalaus-kaavan luominen muodostaen raportit, joissa käytetään yllä osat ja sitten yhdistää kyseiset lauseet valmis kaavaan. Tässä osassa Luo esimerkiksi Automaattinen skaalaus-kaava, joka suorittaa joitakin tosielämän skaalauksen päätöksiä.

Ensin määrittää japanin Microsoftin uusi Automaattinen skaalaus-kaava vaatimukset. Kaava on:

1. **Lisää** kohde Laske solmujen resurssivarantoon Jos suorittimen käyttö on suuri määrä.
2. **Pienennä** kohde Laske solmujen sarjassa, kun suorittimen käyttö on pieni.
3. Rajoita aina **solmujen enimmäismäärä** 400.

*Suurenna* suuren suorittimen käytön aikana solmujen määrän, joka täyttää käyttäjäkohtainen muuttuja lauseen on määrittäminen (`$totalNodes`) arvo, joka on 110 prosenttia nykyisen kohteen numero solmujen, mutta ainoastaan, jos vähintään keskimääräinen suorittimen käyttö on viimeisten 10 minuutin oli 70 prosenttia. Muussa tapauksessa Käytämme erillinen nykyarvo.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

Haluat *pienentää* suorittimen käytön aikana solmujen määrän, tutustu kaavassa seuraavan lauseen määrittää sama `$totalNodes` muuttujan 90 prosenttia solmujen Jos keskimääräinen suorittimen käyttö, viimeiset 60 minuutin on kohdassa 20 prosenttia nykyisen kohteen määrä. Muussa tapauksessa Käytä nykyistä arvoa `$totalNodes` , joka on täydennetty yllä-lause.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Erillinen Laske solmut **enintään** 400 kohde määrän rajoittaminen nyt:

```
$TargetDedicated = min(400, $totalNodes)
```

Tässä on valmis kaava:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Automaattinen skaalaus käyttävä resurssivarannon luominen

Jos haluat luoda uuden ryhmän automaattisen skaalauksen poistaminen käytössä, jollakin seuraavista tavoista:

**Erän .NET**

1. Voit luoda altaan [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
1. [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx) -ominaisuuden arvoksi `true`.
1. Määritä Automaattinen skaalaus-kaavan [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx) ominaisuus.
1. (Valinnainen) Ominaisuuden [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx) (oletusarvo on 15 minuuttia).
1. Vahvista [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) tai [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx)resurssivarantoon.

**Erän REST-Ohjelmointirajapinnalla**

* [Lisää resurssivarantoon tiliin](https://msdn.microsoft.com/library/azure/dn820174.aspx): Määritä `enableAutoScale` ja `autoScaleFormula` elementtien REST API-pyynnössä määrittäminen automaattisen skaalausta resurssivarantoon luomisen yhteydessä.

Seuraavat koodikatkelman Luo Automaattinen skaalaus käyttävä resurssivarantoon käyttämällä [Erä .NET] [ net_api] kirjastoon. Ryhmän Automaattinen skaalaus kaavan asettaa solmujen aloituspäivä maanantai näkyvissä 5 kohde määrän ja 1 joka päivä viikon. [Automaattisen skaalauksen väli](#automatic-scaling-interval) on määritetty 30 minuuttia. Tässä ja muut C# katkelmat tämän artikkelin, "myBatchClient" on oikein valmisteltu esiintymän [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Erän REST API ja .NET SDK lisäksi voit kaikkia muita [Erä SDK: T](batch-technical-overview.md#batch-development-apis), [erä PowerShellin cmdlet-komennot](batch-powershell-cmdlets-get-started.md)ja [Erä CLI](batch-cli-get-started.md) automaattisen skaalauksen poistaminen käyttöä varten.

> [AZURE.IMPORTANT] Kun luot Automaattinen skaalaus käyttävä resurssivarantoon, sinun täytyy **ei** Määritä `targetDedicated` parametria. Myös, jos haluat manuaalisesti koon Automaattinen skaalaus käyttävä resurssivarantoon (esimerkiksi [BatchClient.PoolOperations.ResizePool]kanssa[net_poolops_resizepool]), valitse sinun täytyy ensimmäisen **käytöstä** Automaattinen skaalaus-ryhmän, muuttaa sen kokoa.

### <a name="automatic-scaling-interval"></a>Automaattisen skaalauksen väli

Oletusarvon mukaan erä palvelun säätää ryhmän koko kaavan Automaattinen skaalaus mukaan **15 minuutin**välein. Tämä väli on määritettävä, kuitenkin resurssivarantoon seuraavien ominaisuuksien avulla:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (erä .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST API)

Pienin väli on päättymässä ja enimmäismäärä on 168 tuntia. Jos määritetty aikaväli tämän alueen ulkopuolella, erä-palvelu palauttaa pyytäminen väärä (400)-virheen.

> [AZURE.NOTE] Automaattisen skaalauksen poistaminen ei ole tällä hetkellä tarkoitettu vastata muutokset alle minuutin, mutta sen sijaan, että on tarkoitettu lisääminen resurssivarantoon vähitellen kuin työmäärä suoritat kokoa.

## <a name="enable-autoscaling-on-an-existing-pool"></a>Ota käyttöön aiemmin resurssivarantoon automaattisen skaalauksen poistaminen

Jos olet jo luonut resurssivarantoon Laske solmujen tietyn ajan kanssa käyttämällä *targetDedicated* -parametria, voit ottaa automaattisen skaalauksen poistaminen-ryhmään. Kunkin erän SDK on "Ota käyttöön Automaattinen skaalaus-toiminnon, esimerkiksi:

* [BatchClient.PoolOperations.EnableAutoScale] [ net_enableautoscale] (erä .NET)
*  [Ota käyttöön Automaattinen skaalaus resurssivarantoon] [ rest_enableautoscale] (REST API)

Kun otat automaattisen skaalauksen poistaminen käyttöön aiemmin resurssivarantoon, koskee seuraavasti:

* Jos Automaattinen skaalaus on tällä hetkellä altaan **käytöstä** silloin, kun lähetät "Ota käyttöön Automaattinen skaalaus-pyynnön, sinun *täytyy* määrittää kelvollinen Automaattinen skaalaus kaavan silloin, kun lähetät pyynnön. Voit *halutessasi* määrittää Automaattinen skaalaus arvioinnin välin. Jos et määritä aikaväli, käytetään oletusarvon 15 minuuttia.

* Jos Automaattinen skaalaus on tällä hetkellä **käytössä** ryhmän, voit määrittää Automaattinen skaalaus-kaava ja/tai arviointi väli. Molempia ominaisuuksia ei voi jättää pois.

  * Jos määrität uusi Automaattinen skaalaus-arviointi väli, arvioinnin aikataulua pysäytetään ja uuden aikataulun aloitetaan. Uuden aikataulun alkamisaika on aika, jolla "Ota käyttöön Automaattinen skaalaus-pyyntö lähetettiin.

  * Jos jätät Automaattinen skaalaus-kaavan tai arvioinnin väli, erä-palvelun edelleen käyttää tätä asetusta nykyisen arvon.

> [AZURE.NOTE] Jos arvo on määritetty *targetDedicated* -parametrin altaan luonnin yhteydessä, se ohitetaan, jos automaattisen skaalauksen kaavan arvioidaan.

Tämä C# koodikatkelman käyttää [Erä .NET] [ net_api] kirjaston ottaminen käyttöön aiemmin resurssivarantoon automaattisen skaalauksen poistaminen:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Päivitä Automaattinen skaalaus-kaava

"Ota käyttöön Automaattinen skaalaus-pyynnössä *päivittää* kaavan käyttäminen aiemmin Automaattinen skaalaus käyttävä resurssivarantoon (esimerkiksi [EnableAutoScale] kanssa[ net_enableautoscale] erä .NET-). Ei mitään erityistä "Päivitä Automaattinen skaalaus-toimintoa. Esimerkiksi jos automaattisen skaalauksen poistaminen jo käytössä "myexistingpool", kun seuraava koodi suoritetaan, kaavan Automaattinen skaalaus on korvattu sisällön `myNewFormula`.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Automaattinen skaalaus päivitysväli

Päivitettäessä Automaattinen skaalaus-kaava käyttää samaa [EnableAutoScale] [ net_enableautoscale] tavan aiemmin Automaattinen skaalaus käyttävä resurssivarantoon Automaattinen skaalaus arvioinnin välien muuttaminen. Jos esimerkiksi haluat määrittää Automaattinen skaalaus arvioinnin ajan 60 minuuttia resurssivarantoon, joka on jo Automaattinen skaalaus käytössä:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Automaattinen skaalaus-kaavan laskeminen

Voit laskea kaavan ennen sen resurssivarantoon. Näin voit suorittaa "testin suorittaminen" nähdäksesi, kuinka sen lauseet arvioi ennen kuin voit sijoittaa kaavan tuotannon kaavan.

Automaattinen skaalaus-kaavan laskeminen, sinun täytyy ensimmäisen **ottaa käyttöön automaattisen skaalauksen poistaminen** käyttöön resurssivarantoon, jossa on **Virheellinen kaava**. Jos haluat testata kaavaa, joka ei ole vielä otettu käyttöön automaattisen skaalauksen poistaminen resurssivarantoon, voit käyttää yksirivinen kaavan `$TargetDedicated = 0` kun otat automaattisen skaalauksen poistaminen ensin. Käytä jompikumpi seuraavista jälkeen arvioida kaavaa haluat testata:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) tai [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Erän .NET näistä tavoista edellyttävät aiemmin resurssivaranto ja merkkijonon, joka sisältää kaavan Automaattinen skaalaus tunnus arvioida. Arvioinnin tulokset sisältyvät palautettu [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx) esiintymän.

* [Automaattisen skaalauksen kaavan laskeminen](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Määritä tämä REST API-pyyntö resurssivarantoon URI-tunnus ja pyynnön tekstiin *autoScaleFormula* osan Automaattinen skaalaus-kaava. Toiminnon vastaus sisältää virheen tietoja, jotka voi liittyä kaava.

Tämä [Erä .NET] -[ net_api] koodikatkelman, emme laskeminen kaavan ennen soveltaminen [CloudPool][net_cloudpool]. Jos altaan ei ole otettu käyttöön automaattisen skaalauksen poistaminen, emme käyttöön ensin.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Tämä koodikatkelman kaavassa onnistuneen arviointi johtaa tulosteen seuraavankaltaiselta:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Automaattinen skaalaus suorittamista koskevien tietojen hakeminen

Voit varmistaa, kaava toimii oikein, on suositeltavaa Tarkista automaattisen skaalauksen poistaminen "suoritetaan" erä suorittaa lisääminen resurssivarantoon tulokset säännöllisesti. , Joten (tai päivitä) viittaus ryhmään, ja sen automaattinen suorittaminen viimeisen skaalaus-ominaisuuksia.

Erän .NET [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx) -ominaisuus on tietoja uusimman automaattisen skaalausta suorittaa suoritetaan altaan erä-palvelun tietojen useita ominaisuuksia.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

REST-Ohjelmointirajapinnalla [resurssivarantoon tietoja](https://msdn.microsoft.com/library/dn820165.aspx) pyynnön palauttaa resurssivarantoon, joka sisältää uusimmat automaattinen skaalausta suorittaa tietojen [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun)tietoja.

Seuraavat C#-koodikatkelman käyttää erä .NET-kirjaston viimeisen automaattisen skaalauksen poistaminen resurssivarantoon "myPool" suorittaa tietojen tulostaminen:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Esimerkki tulosteen edellisen koodikatkelman:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Esimerkki Automaattinen skaalaus-kaavoja

Seuraavaksi tutustumme muutama kaavoja, jotka Näytä voi säätää varannon resursseja Laske eri tavoilla.

### <a name="example-1-time-based-adjustment"></a>Esimerkki 1: Aikapohjaisten muutos

Haluat ehkä resurssivarantoon koon viikonpäivä- ja kellonaika, voit suurentaa tai pienentää vastaavasti altaan näkyvien solmujen määrän perusteella.

Tämä kaava hakee ensin nykyisen kellonajan. Jos se on viikonpäivä (1-5) ja työaika (8 AM 6-PM), kohde-ryhmän koko on määritetty 20 solmujen. Muussa tapauksessa se on määritetty 10 solmujen.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Esimerkki 2: Tehtävien muutos

Tässä esimerkissä ryhmän koko sovitetaan jonossa tehtävien määrän mukaan. Huomaa, että sekä kommentit että rivinvaihdot ovat hyväksyttävät kaavan merkkijonojen.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Esimerkki 3: Rinnakkainen tehtävien laskenta

Tämä on toinen tässä esimerkissä tehtävät montako ryhmän koko muuttuu. Tämä kaava on myös huomioon [MaxTasksPerComputeNode] [ net_maxtasks] arvo, joka on määritetty ryhmään. Tämä on erityisen hyödyllinen tilanteissa, jossa [Rinnakkainen tehtävän suorittaminen](batch-parallel-node-tasks.md) on otettu käyttöön oman resurssivarantoon.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Esimerkki 4: Alkuperäinen resurssivarantoon-koon määrittäminen

Tässä esimerkissä näytetään C# koodikatkelman Automaattinen skaalaus-kaava, joka määrittää ryhmän koko tietyn solmujen määrän alkuperäinen aikajaksolla. Valitse se muuttuu käytön määrän perusteella ja aktiivisen ryhmän koko tehtävät, kun alkuperäinen aika on kulunut.

Seuraavat koodikatkelman kaava:

- Määrittää ensimmäisen ryhmän koko neljä solmujen.
- Ei muuta ryhmän koko ryhmän elinkaari ensimmäisen 10 minuutin kuluessa.
- 10 minuutin kuluttua hakee määrän suurin arvo käynnissä ja aktiiviset tehtävät edellisten 60 minuutin kuluessa.
  - Jos kumpikin arvo on 0 (ilmaisee tehtäviä ei ole käynnissä tai aktiivisena viimeisen 60 minuuttia)-ryhmän koko arvoksi 0.
  - Jos joko arvo on suurempi kuin nolla, ei tehdä muutoksia.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Seuraavat vaiheet

* [Suurenna Azure erä Laske samanaikainen solmu tehtävien resurssien käyttö](batch-parallel-node-tasks.md) on tietoja siitä, miten voit suorittaa useita tehtäviä samanaikaisesti lisääminen resurssivarantoon Laske-solmuissa. Lisäksi automaattisen skaalauksen poistaminen tämä ominaisuus voi auttaa Pienennä työn keston joitakin toiminnoista, tallentaminen rahaa.

* Varmista toiseen tehokkuuden Jarrutehostimen erä sovelluksen kyselyt erä palvelun eniten optimaalisen tavalla. [Kyselyn Azure erä palvelun tehokkaasti](batch-efficient-list-queries.md)käydään rajoittamisesta tiedot, jotka ylittää koneisiin, kun testaat mahdollisesti Laske solmujen tai tehtävien tuhansia tilan määrää.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx
