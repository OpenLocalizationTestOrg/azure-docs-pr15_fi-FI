<properties 
    pageTitle="Diagnostiikan hakutoiminnolla | Microsoft Azure" 
    description="Etsi ja suodattaa yksittäisiä tapahtumia, pyynnöt ja kirjaudu jäljittää." 
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
    ms.date="06/09/2016" 
    ms.author="awills"/>
 
# <a name="using-diagnostic-search-in-application-insights"></a>Diagnostiikan haun avulla hakemuksen tiedot

Diagnostiikan haun toimintoa [Hakemuksen tiedot] [ start] , että käytät etsitään, tutustu telemetriatietojen yksittäisiä kohteita, kuten page views poikkeukset-tai web-pyynnöt. Ja voit tarkastella log jäljittää ja tapahtumia, jotka on koodattu.

## <a name="where-do-you-see-diagnostic-search"></a>Jos näet diagnostiikan haun?


### <a name="in-the-azure-portal"></a>Azure-portaalissa

Voit avata diagnostiikan haun erikseen:

![Avaa diagnostiikan haku](./media/app-insights-diagnostic-search/01-open-Diagnostic.png)


Se avautuu myös, kun napsautat läpi kaikki kaaviot ja ruudukon kohteet. Tässä tapauksessa sen suodattimissa on valmiiksi määritetty keskittyä valitun kohteen tyyppi. 

Jos sovellus on verkkopalvelun, yhteenveto-sivu näyttää kaavion pyyntöjen määrän. Napsauta sitä ja saat tarkempia kaavioon luettelo, jossa näkyy, kuinka monta pyynnöt on tehty kunkin URL-osoitteen kanssa. Valitse minkä tahansa rivin ja saat yksittäisten pyyntöjen luettelo kyseisen URL-osoitteen:

![Avaa diagnostiikan haku](./media/app-insights-diagnostic-search/07-open-from-filters.png)


Päärungon muodostavat diagnostiikan haku on lueteltu telemetriatietojen kohteet - palvelimen pyynnöt sivun näkymät, mukautetut tapahtumat, joka on koodattu ja niin edelleen. Luettelon yläosassa on yhteenveto-kaavio, jossa määrät tapahtumien ajan kuluessa.

Tapahtumien yleensä näkyvät diagnostiikan haun ennen kuin ne näkyvät metrisillä hallinnassa. Vaikka sivu päivitetään väliajoin, valitse Päivitä, jos odotat sisäänpääsyä tietyn tapahtuman.


### <a name="in-visual-studio"></a>Visual Studiossa

Avaa Etsi-toiminnon Visual Studiossa:

![](./media/app-insights-diagnostic-search/32.png)

Etsi-ikkunassa on samat toiminnot kuin web-portaaliin:

![](./media/app-insights-diagnostic-search/34.png)


## <a name="sampling"></a>Esimerkkejä

Jos sovelluksen Luo telemetriatietojen paljon (ja käytät ASP.NET SDK-version 2.0.0-beta3 tai uudempi), mukautuvat esimerkkejä moduulin automaattisesti vähentää asema, joka lähetetään portaalin lähettämällä vain edustava luku tapahtumat. Tapahtumat, jotka liittyvät pyynnössä kuitenkin valittuna tai valitsematta ryhmänä ja siten, että voit siirtyä niihin liittyvät tapahtumat. 

[Lue lisää esimerkkejä](app-insights-sampling.md).


## <a name="inspect-individual-items"></a>Yksittäisten kohteiden tarkistaminen

Valitse telemetriatietojen keskustelusta Nähdäksesi avainkenttien ja toisiinsa liittyvät kohteet. Jos haluat nähdä täydellisen luettelon kentät, valitse "...". 


![Valitse uusi työnimike, Muokkaa kenttiä ja valitse sitten OK.](./media/app-insights-diagnostic-search/10-detail.png)

Etsi täydellisen luettelon kentät, Käytä tavallista merkkijonoja (ilman yleismerkkejä). Käytettävissä olevat kentät määräytyvät sen mukaan, telemetriatietojen.

## <a name="create-work-item"></a>Luo työnimike

Voit luoda ohjelmavirhe Visual Studio Team Servicesissä telemetriatietojen minkä tahansa kohteen tiedot. 

![Valitse uusi työnimike, Muokkaa kenttiä ja valitse sitten OK.](./media/app-insights-diagnostic-search/42.png)

Ensimmäisen kerran teet tämän, sinua pyydetään ryhmän Services-tilin ja projektin linkki.

![Kirjoita URL-osoite ja Team Services-palvelimen projektinimi ja sitten Hyväksy](./media/app-insights-diagnostic-search/41.png)

(Voit myös siirtyä asetusten määritys-sivu > töitä.)

## <a name="filter-event-types"></a>Suodattimen tapahtumatyypit

Avaa suodatin-sivu ja valitse haluamasi tapahtumatyypit. (Jos myöhemmin, jonka haluat palauttaa suodattimia, joiden kanssa olet avannut sivu, valitse Palauta.)


![Valitse Suodata ja valitse telemetriatietojen tyypit](./media/app-insights-diagnostic-search/02-filter-req.png)


Tapahtuman ovat:

* **Jäljitys** - vianmäärityslokit, kuten TrackTrace, log4Net, NLog ja System.Diagnostic.Trace puheluja.
* **Pyydä** - HTTP-pyyntöjen server-sovelluksessa, kuten sivujen, komentosarjoja, kuvia, tyylitiedostot ja tiedot. Näitä tapahtumia käytetään pyynnön ja vastauksen yleiskatsaus kaavioiden luominen.
* **Sivun katselu** - web-asiakasohjelman lähettämä Telemetriatietojen käyttää sivulla view-raporttien luominen. 
* **Mukautettu tapahtuma** - Jos olet lisännyt TrackEvent() puhelut tilauksen [seurata käyttöä][track], voit etsiä tästä.
* **Poikkeus** - palvelin ja mitkä kirjaudut käyttäen TrackException() aiheutetaan poikkeuksia.

## <a name="filter-on-property-values"></a>Suodata arvot

Voit suodattaa tapahtumat niiden ominaisuuksien arvot. Käytettävissä olevat ominaisuudet määräytyvät tapahtuman valittu. 

Valitse esimerkiksi pyynnöt tietyn vastauksen koodilla ulos.

![Laajenna ominaisuus ja valitse arvo](./media/app-insights-diagnostic-search/03-response500.png)

Valitsemalla ei ole tietyn ominaisuuden arvoja on samoin kuin valitsemalla kaikki arvot; se siirtyy käytöstä suodattaminen kyseisestä ominaisuudesta.


### <a name="narrow-your-search"></a>Haun rajaaminen

Huomaa, että laskee oikealla puolella suodatinarvot näyttää kuinka monta esiintymien on nykyisen suodatettuun sarjaan. 

Tässä esimerkissä se on Poista, joka `Reports/Employees` pyytäminen useimpia 500 virheet tulokset:

![Laajenna ominaisuus ja valitse arvo](./media/app-insights-diagnostic-search/04-failingReq.png)

Lisäksi voit halutessasi myös Katso, mitä muita tapahtumia on näistä tänä aikana, voit tarkistaa **Sisällytä tapahtumat Määrittämätön ominaisuudet**.

## <a name="remove-bot-and-web-test-traffic"></a>Poista robotti ja web-testi liikenne

Suodatus **reaali tai synteettiset liikenteen** ja tarkista **reaaliluku**.

Voit myös suodattaa **synteettistä liikenteen lähteen**mukaan.

## <a name="inspect-individual-occurrences"></a>Tarkasta yksittäisiin

Lisää suodatin pyynnön nimi ja sitten tutkia kyseisen tapahtuman yksittäiset esiintymät.

![Valitse arvo](./media/app-insights-diagnostic-search/05-reqDetails.png)

Pyyntö tapahtumien tiedot Näytä poikkeukset pyynnön käsittelyssä on tapahtunut.

Vahvista automaattisen poikkeuksen nähdäksesi sen tiedot, mukaan lukien pinon jäljitys.

![Valitse poikkeuksen](./media/app-insights-diagnostic-search/06-callStack.png)

## <a name="find-events-with-the-same-property"></a>Tapahtumien samalla ominaisuudella etsiminen

Etsi kaikki kohteet, joilla on sama ominaisuuden arvo:

![Napsauta hiiren kakkospainikkeella ominaisuus](./media/app-insights-diagnostic-search/12-samevalue.png)

## <a name="search-by-metric-value"></a>Hakuperusteena kustannusarvo

Pyydä pyynnöt vastauksen aina > 5s.  Kertaa esitetään jakoviivat: 10 000 jakoviivat = 1ms.

!["Vastausajan":(threshold TO *)](./media/app-insights-diagnostic-search/11-responsetime.png)



## <a name="search-the-data"></a>Hae tiedot

Voit etsiä termejä kaikista ominaisuuden arvot. Tämä on erityisen hyödyllinen, jos olet kirjoittanut [Mukautetut tapahtumat] [ track] arvojen kanssa. 

Voit määrittää alueen hakujen lyhentää alueen yli nopeammin on aika. 

![Avaa diagnostiikan haku](./media/app-insights-diagnostic-search/appinsights-311search.png)

Etsi ehdot, ei alimerkkijonoja. Ehdot ovat aakkosnumeerinen merkkijonoja, kuten joitakin välimerkkejä kuten '. "ja"_". Esimerkki:

termi|*ei* vastaa mukaan|mutta nämä vastaavat
---|---|---
HomeController.About|tietoja<br/>Aloitus|h\*tietoja<br/>Aloitus\*
IsLocal|Paikallinen<br/>on<br/>\*Paikallinen|ISL\*<br/>islocal<br/>i\*l\*
Uusi viive|w d|Uusi<br/>viive<br/>n\* ja d\*


Seuraavassa on haku-lausekkeiden avulla voit:

Esimerkkikyselyn | Vaikutus 
---|---
hidas|Etsi kaikki tapahtumat päivämääräalue, jonka kentät sisältävät termi "hidastaa"
tietokannan?|Vastaa database01, databaseAB...<br/>? ei voi suorittaa hakusana alkuun.
tietokannan * |Vastaa tietokannan, database01, databaseNNNN<br/> * ei sallita hakusana alkuun
Apple ja Banaani|Etsi tapahtumia, jotka sisältävät molemmat ehdot. Käytä pääoman "ja" ei "ja".
Apple tai Banaani<br/>Apple Banaani|Etsi tapahtumia, jotka sisältävät joko termi. Käytä "Tai", ei "tai". < /br/ > lyhyt lomake.
Apple ei Banaani<br/>Apple-Banaani|Etsi tapahtumia, joissa yksi termi, mutta et toiseen.<br/>Lyhyessä muodossa.
sovelluksen * ja Banaani-(grape pear)|Loogiset operaattorit ja ole sijoitettu oikein sulkeisiin.
"Metrijärjestelmä": 0-500<br/>"Metrijärjestelmä": 500 vastaanottaja * | Etsi tapahtumia, jotka sisältävät arvo alueelta nimettyä mitta.


## <a name="save-your-search"></a>Haun tallentaminen

Kun olet määrittänyt kaikki haluamasi suodattimet, voit tallentaa haun suosikkeihin. Jos työskentelet organisaation tilille, voit valita, haluatko jakaa sen ryhmän muiden jäsenten kanssa.

![Valitse suosikki, nimi ja valitse Tallenna](./media/app-insights-diagnostic-search/08-favorite-save.png)


Jos haluat nähdä haun uudelleen, **Siirry yhteenveto-sivu** ja Avaa Suosikit:

![Suosikit-ruutu](./media/app-insights-diagnostic-search/09-favorite-get.png)

Jos olet tallentanut suhteellinen aikaväli, avata uudelleen sivu on uusimmat tiedot. Jos olet tallentanut suora aikaväli, näet samat tiedot aina.


## <a name="send-more-telemetry-to-application-insights"></a>Lisää telemetriatietojen lähettäminen hakemuksen tiedot

Lisäksi ulos-valmiin telemetriatietojen, sovelluksen havainnollistamisen SDK lähettämä voit tehdä seuraavaa:

* Sieppaaminen log jäljittää-suosikki-kirjaaminen-framework [.NET] -[ netlogs] tai [Java][javalogs]. Tämä tarkoittaa, voit etsiä log jäljittää ja yhdistää ne sivun näkymät, poikkeukset ja muita tapahtumia. 
* [Koodin kirjoittaminen] [ track] lähetetään mukautetut tapahtumat, sivun näkymien ja poikkeukset. 

[Lue, miten voit lähettää lokit ja mukautetun telemetriatietojen sovelluksen havainnollistamisen][trace].


## <a name="questions"></a>Q & A

### <a name="limits"></a>Kuinka paljon tietoja säilytetään?

Enintään 500 tapahtumat sekunnissa kunkin sovelluksesta. Tapahtumien säilyvät seitsemän päivän ajan.

### <a name="how-can-i-see-post-data-in-my-server-requests"></a>Miten näen kirjaa tietoja palvelin-pyyntöjä?

Olemme kirjautumatta kirjaa tiedot automaattisesti, mutta voit käyttää [TrackTrace tai log puheluita][trace]. Sijoittaa tiedot viestin viesti-parametrin. Et voi suodattaa viestin ominaisuudet, kuten, mutta kokorajoitus on pidempi.

## <a name="add"></a>Seuraavat vaiheet

* [Lähetä lokit ja mukautetun telemetriatietojen sovelluksen havainnollistamisen][trace]
* [Käytettävyys ja vastausajan testien määrittäminen][availability]
* [Vianmääritys][qna]



<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[javalogs]: app-insights-java-trace-logs.md
[netlogs]: app-insights-asp-net-trace-logs.md
[qna]: app-insights-troubleshoot-faq.md
[start]: app-insights-overview.md
[trace]: app-insights-search-diagnostic-logs.md
[track]: app-insights-api-custom-events-metrics.md

 