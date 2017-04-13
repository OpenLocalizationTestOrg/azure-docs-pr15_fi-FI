<properties
    pageTitle="Käytä tietojoukkojen otosten koneen Learning Studiossa | Microsoft Azure"
    description="Käytetty otoksen malleissa ML Studio sisältyvät tietojoukot kuvauksia. Voit käyttää näitä otoksen tietojoukot oman kokeissa."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="garye"/>


# <a name="use-the-sample-data-sets-in-azure-machine-learning-studio"></a>Käytä tietojoukkojen otosten Azure koneen Learning Studiossa

[top]: #machine-learning-sample-datasets

Kun luot uuden työtilan luominen Azure koneen Learning, tietojoukkojen otosten ja kokeiden määrä kuuluvat oletusarvoisesti. Monia näistä otoksen tietojoukot käyttävät malli mallien [Azure Cortana Liiketoimintatieto-valikoima](http://gallery.cortanaintelligence.com/)ja toiset ovat esimerkkejä eri tietotyyppeihin on käytetään yleensä tietokoneen learning mainittu.

Joitain näistä tietojoukot ovat käytettävissä Azure-Blob-objektien tallennustilaan. Alla olevassa taulukossa tarjoaa nämä tietojoukot suoran linkin. Voit käyttää näitä tietojoukot oman kokeissa käyttämällä [Tietojen] [ import-data] moduuli.

Muiden tietojoukkojen nämä otosten on lueteltu kohdassa **Tallennettu tietojoukkoja** vasemmalla puolella kokeen alusta moduuli-valikoimasta kun Avaa tai Luo uusi koe ML Studiossa.
Voit käyttää jotakin näistä tietojoukot oman kokeessa vetämällä sen kokeen alusta.


<!--
For a list of sample experiments available in ML Studio, see [Machine Learning Sample Experiments][sample-experiments].

[sample-experiments]: machine-learning-sample-experiments.md
-->

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<table>

<tr>
  <th align=left>Tietojoukon nimi</th>
  <th align=left>Tietojoukon kuvaus</th>
</tr>

<tr>
  <td valign=top>Aikuisen laskenta tulot binaarinen luokitus-tietojoukko</td>
  <td valign=top>
Alijoukkoa 1994 laskenta tietokannan käytön aikuiset välityksellä 16 iän muutetut tulo-indeksi > 100.<p> </p><b>Käyttö:</b> luokitella käyttävät demografia ennustaa, onko henkilö saa yli 50K vuodessa.<p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Kohavi, r-Becker, b, (1996). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede </td>
</tr>

<tr ID=airport-codes-dataset>
  <td valign=top>Airport koodit tietojoukko</td>
  <td valign=top>
Yhdysvaltain airport koodit.<p> </p>Tämä tietojoukko sisältää kunkin US airport airport ID-tunnus ja nimi ja sijainti paikkakunnan ja alueen yhden rivin.
  </td>
</tr>

<tr>
  <td valign=top>Autojen hintatiedot (raaka)</td>
  <td valign=top>
Tietoja autot mukaan tee ja mallin hinta, mukaan lukien ominaisuuksia, kuten sylinterit ja MPG sekä vakuutus pistemäärän määrän.<p> </p>Riskin kirjainarvosanan on alun perin liitetty automaattinen hinta ja sitten huomioitu tunnetut vakuutusmatemaatikkojen kesken suorittavat kuin symboling prosessin todellinen riskit. + 3 arvo tarkoittaa, että automaattinen on riskialtis ja – 3 arvon, että se on todennäköisesti melko turvallista.<p> </p><b>Käyttö:</b> ennustaa riskin kirjainarvosanan ominaisuuksia, käyttämällä regression tai multivariate luokitus. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Schlimmer, J.C. (1987). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede </td>
</tr>

<tr ID=bike-rental-uci-dataset>
  <td valign=top>Polkupyörien vuokrausta UCI tietojoukko</td>
  <td valign=top>
UCI polkupyörien vuokrausta tietojoukko, joka perustuu pääoman Bikeshare yritys, joka säilyttää Washington Ohjauskoneen polkupyörien vuokrausta verkon reaali tiedot.<p> </p>Tietojoukon on yksi rivi kunkin päivittäin 2011 ja 2012 17,379 rivien yhteensä tunti. Kerran tunnissa polkupyörien vuokrat alue on 1 977.

  </td>
</tr>

<tr ID=bill-gates-rgb-image>
  <td valign=top>Bill Gates RGB-kuva</td>
  <td valign=top>
Yleisesti saatavilla kuvatiedosto muunnetaan CSV-tiedot.<p> </p>Kuvan muuntaminen koodi on annettu <strong>värin käyttäminen K tarkoittaa klusterointi quantization</strong> mallin tiedot-sivu.
  </td>
</tr>

<tr>
  <td valign=top>Verenpaineen sivu tiedot</td>
  <td valign=top>
Osan tiedoista Verenpaineen verensiirtoon palvelun Center Hsin Chu-kaupunki, Taiwan Verenpaineen luovuttajan tietokannasta.<p> </p>Lahjoittajan tiedot sisältävät kuukauden viimeinen sivu jälkeen), ja korkojakso tai lahjoituksia, viimeinen sivu kuluneen ajan kokonaismäärän ja Verenpaineen donated määrä.<p> </p><b>Käyttö:</b> lähinnä ennustaa luokitus kautta, onko luovuttaja donated Verenpaineen maaliskuussa 2007: ssä, jossa 1 ilmaisee luovuttajaeläimeltä aikana kohde-käyttö ja 0 ei luovuttajan. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Yeh, I.C., (2008). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede <p> </p>Yeh, I-Cheng, Varvikko, kuningas-Jang ja otetu, napauta-Ming "Knowledge etsiminen RFM mallin Bernoullin näppäinyhdistelmällä" sovellusten, 2008 avustaja järjestelmien <a href="http://dx.doi.org/10.1016/j.eswa.2008.07.018">http://dx.doi.org/10.1016/j.eswa.2008.07.018</a>
  </td>
</tr>

<tr ID=book-reviews-from-amazon>
  <td valign=top>Amazon-kirjan tarkistukset</td>
  <td valign=top>
Tehtävien tarkastusten kirjojen Amazon, tekemä University Pennsylvania tutkijoiden (<a href="http://www.cs.jhu.edu/~mdredze/datasets/sentiment/">markkinatunnelma</a>) amazon.com-sivustosta. Katso Oheistiedot-raportti "perustajajäsenten tiedot, Bollywood, ylävaunua-ruutuihin ja Blenders: toimialueen mukauttamista Markkinatunnelma luokitus" John Blitzer, merkitse Dredze ja Fernando Pereira; Liitos on laskennallinen Lingvistinen (Käyttöoikeusluettelon) 2007.<p> </p>Alkuperäinen tietojoukko on 975K tarkistukset kanssa luokittelut 1, 2, 3, 4 ja 5. Tarkistukset on kirjoitettu englanniksi ja ovat ajanjakson 1997 2007. Tämä tietojoukko on alaspäin näyte, 10K tarkistukset.
  </td>
</tr>

<tr>
  <td valign=top>Rinnassa syövän tiedot</td>
  <td valign=top>
Yksi kolme syövän liittyvät tietojoukkoja myöntämä Oncology Projektinhallintainstituutin, joka näkyy usein tietokoneen learning julkaisuista. Yhdistää vianmääritystiedot noin 300 kudoksen näytteiden koe Analysis ominaisuuksilla.<p> </p><b>Käyttö:</b> luokitella syövän tyypin, 9 määritteiden perusteella osa, jossa on lineaarinen ja jotkin ovat categorical. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Wohlberg, W.H. ja katu, W.N. sekä Mangasarian, O.L. (1995). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede </td>
</tr>

<tr ID=breast-cancer-features>
  <td valign=top>Rinnassa syövän ominaisuudet <td valign=top>
Tietojoukon sisältää tietoja 102K epäilyttävistä alueiden (ehdokkaiden) läpivalaisulaitteilla kuvia, kunkin kuvaaman 117 ominaisuuksia. Ominaisuudet ovat omistusoikeuksia ja niiden merkitys tulee näkyviin ei tietojoukko laatijat (ohjelmaa terveydenhoito) mukaan. 
  </td>
</tr>

<tr ID=breast-cancer-info>
  <td valign=top>Rinnassa syövän tiedot</td>
  <td valign=top>
Tietojoukon on lisätietoja läpivalaisulaitteilla kuva epäilyttävistä alueittain. Esimerkin tietoja (esimerkiksi otsikko, potilaiden ID-tunnus, korjaustiedoston suhteessa koko kuvan koordinaatit) vastaavan rivinumeron rinnassa syövän ominaisuuksia tietojoukossa tietoja. Kunkin potilaan on esimerkkejä määrä. Esimerkkejä ovat positiivisia ja osa on negatiivinen cancer joiden potilaille. Potilaille, joilla ei ole syövän kaikki esimerkit on negatiivinen. Tietojoukon on esimerkkejä 102K. Tietojoukon on yksipuolinen, 0,6 prosenttia asioista on positiivinen, muut on negatiivinen. Tietojoukon tuli markkinoille ohjelmaa terveydenhoito mukaan.
  </td>
</tr>

<tr ID=crm-appetency-labels-shared>
  <td valign=top>Jaettu CRM Appetency otsikot</td>
  <td valign=top>
Otsikoiden päivittäminen KDD Cup 2009 asiakkaan yhteyden tekstinsyöttö todennus (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_appetency.labels">orange_small_train_appetency.labels</a>).
  </td>
</tr>

<tr ID=crm-churn-labels-shared>
  <td valign=top>Jaettu CRM Churn otsikot</td>
  <td valign=top>
Otsikoiden päivittäminen KDD Cup 2009 asiakkaan yhteyden tekstinsyöttö todennus (<a href="http://www.sigkdd.org/site/2009/files/orange_small_train_churn.labels">orange_small_train_churn.labels</a>).
  </td>
</tr>

<tr ID=crm-dataset-shared>
  <td valign=top>Jaettu CRM-tietojoukko</td>
  <td valign=top>
Tämä tieto tulee KDD Cup 2009 asiakkaan yhteyden tekstinsyöttö todennus (<a href="http://www.sigkdd.org/kdd-cup-2009-customer-relationship-prediction - orange_small_train.data.zip">orange_small_train.data.zip</a>).<p> </p>Tietojoukon sisältää 50K asiakkaiden Ranskan Telecom yrityksestä oranssi. Kukin asiakas on 230 anonymized ominaisuuksia, 190, jotka ovat numeroita ja 40 ovat categorical. Ominaisuudet ovat hyvin lyhyet.
  </td>
</tr>

<tr ID=crm-upselling-labels-shared>
  <td valign=top>Jaettu CRM Upselling otsikot</td>
  <td valign=top>
Otsikoiden päivittäminen KDD Cup 2009 asiakkaan yhteyden tekstinsyöttö todennus (<a href="http://www.sigkdd.org/site/2009/files/orange_large_train_upselling.labels">orange_large_train_upselling.labels</a>).
  </td>
</tr>

<tr>
  <td valign=top>Energiajärjestelmien tehokkuuden Regression tiedot</td>
  <td valign=top>
Kokoelma Simuloitu energiajärjestelmien profiilit-12 eri rakennuksen muotojen perusteella. Rakennuksia vaihtelevat suhteessa perusteella 8 ominaisuuksia, kuten lasit alue, lasi alueen jakelu ja suunta.<p> </p><b>Käyttö:</b> ennakoida energiajärjestelmien tehokkuuden arviointi mukaan yhtenä kaksi todellisia tärkeät vastaukset regression tai luokitus avulla. Usean luokan luokitus on pyöreä vastauksen muuttujan lähimpään kokonaislukuun. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Xifara, A. & Tsanas, A. (2012). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede </td>
</tr>

<tr ID=flight-delays-data>
  <td valign=top>Lennon viivyttää tiedot</td>
  <td valign=top>
Matkustajan lento aikatauluissa suorituskykytietoja otetun TranStats tietojen kokoelma Yhdysvaltain osaston, kuljetus (<a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">Aikatauluissa</a>).<p> </p>Tietojoukon kattaa ajanjakson huhtikuussa-lokakuussa 2013. Ennen kuin lataaminen Azure ML Studio dataset käsiteltiin seuraavasti:<ul><li>Tietojoukon on suodatettu koskemaan vain 70 busiest airports Manner USA</li><li>Peruutettuja lentojen on nimetty kuin lykätä enintään 15 minuutin välein</li><li>Kiertoteiden käytöstä kertynyt lentojen on suodatettu</li><li>Seuraavat sarakkeet valittiin: vuosi, kuukausi, DayofMonth,: n DayOfWeek, Carrier, OriginAirportID, DestAirportID, CRSDepTime, DepDelay, DepDel15, CRSArrTime, ArrDelay, ArrDel15, peruutettu</li></ul>
</td>
</tr>

<tr>
  <td valign=top>Lennon aikatauluissa suorituskyvyn (raaka)</td>
  <td valign=top>
Tietueiden lentokone lento saapuvat ja lähtevät sisällä Yhdysvallat lokakuu 2011.<p> </p><b>Käyttö:</b> ennustaa lennot. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> -Yhdysvaltain osaston, kuljetus <a href="http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time">http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time</a>.
  </td>
</tr>

<tr>
  <td valign=top>Metsää käynnistymiseen tiedot</td>
  <td valign=top>
Sisältää tiedot, kuten lämpötilan ja kosteuden indeksien ja tuulen nopeus-alueelta koillisen osaston Portugalin, metsää käynnistyy tietueet yhdistetään.<p> </p><b>Käyttö:</b> tämä on vaikeaa regression, tehtävä, jos tarkoituksena on ennustaa tallennetuista alueen metsää käynnistyy. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Cortez, P., ja Morais, A. (2008). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede <p> </p>[Cortez ja Morais 2007] P. Cortez ja A. Morais. Tietoja kaivos hallintatavan käyttämällä säätiedot ennustaa metsää käynnistyy. Valitse J. Pekka Posti, M.f. Santosin ja J. Machado Eds. uuden tietokoneen liiketoimintatietojen 13th EPIA 2007 - Portugalin kokouksen tietokoneen Liiketoimintatieto-joulukuun 523-Guimarães, portugali, s. 512-2007-menettelyn suuntausten. APPIA, ISBN 13 978-989-95618-0-9. Osoitteessa: <a href="http://www.dsi.uminho.pt/~pcortez/fires.pdf">http://www.dsi.uminho.pt/~pcortez/fires.pdf</a>.
  </td>
</tr>

<tr ID=german-credit-card-uci-dataset>
  <td valign=top>Saksa luottokortin UCI tietojoukko</td>
  <td valign=top>
UCI Statlog (Saksa luottokortin) tietojoukko (<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">Statlog + Saksa + luottotietojen + tietoja</a>), german.data-tiedoston avulla.<p> </p>Tietojoukon luokittelee henkilöt-kuvaaman joukko määritteitä, riskien pieni tai suuri kuin. Esimerkin edustaa henkilön. On 20 toimintoja, sekä numeroita että categorical, ja binaarinen otsikko (luottotietojen riski-arvo). Suuri luottotietojen riskin tapahtumat on otsikko 2, = pienen luottotietojen riskin tapahtumat on otsikko = 1. Kustannusten misclassifying vähäinen-Esimerkki mahdollisimman suurella on 1, misclassifying riskejä-Esimerkki mahdollisimman alhainen kustannus on 5.
  </td>
</tr>

<tr ID=imdb-movie-titles>
  <td valign=top>IMDB elokuvan otsikot</td>
  <td valign=top>
Tietojoukon sisältää tiedot, jotka on luokiteltu Twitter tweets elokuvat: IMDB elokuvan tunnus-filmin nimi ja Tyylilaji, vuosittain. Tietojoukon ei 17K elokuvat. Tietojoukon otettiin käyttöön paperin "s-kirjainta. Dooms, T. De Pessemier ja L. Martens. MovieTweetings: arviointi tietojoukko elokuvan kerätty Twitter. Workshop Crowdsourcing ja ihmisten laskenta suosittelija Systemsin CrowdRec RecSys 2013,."
  </td>
</tr>

<tr>
  <td valign=top>Tiina kahden luokan tiedot</td>
  <td valign=top>
Tämä on ehkä tunnetaan parhaiten tietokannan löydy kuvion tunnistuksen julkaisuista. Tietojoukon on suhteellisen pieni sisältävä 50 esimerkkejä kunkin Terälehti mitat kolme Tiina lajikkeista.<p> </p><b>Käyttö:</b> ennustaa mitat Tiina tyypin.  <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Fisher-R.A. (1988). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede </td>
</tr>

<tr ID=movie-tweets>
  <td valign=top>Filmin Tweets</td>
  <td valign=top>
Tietojoukon on elokuvan Tweetings tietojoukko laajennettu versio. Tietojoukon on elokuvia, Twitter-hyvin tweets poimittujen 170K luokitukset. Jokaiselle esiintymälle edustaa tweetin, ja se on monikon: Käyttäjätunnus, IMDB elokuvan tunnus, luokitus, aikaleima, mainokset Suosikit tälle tweetin ja tämän tweetin retweets määrä. Tietojoukon tuli markkinoille A. Said, s Dooms, B. Loni ja D. Tikk suosittelija järjestelmien todennus 2014 julkaistut.
  </td>
</tr>

<tr>
  <td valign=top>MPG tiedot eri autot</td>
  <td valign=top>
Tämä tietojoukko on Carnegie Mellon Universityn StatLib valikoiman myöntämä tietojoukko toimintoja on muutettu versio. Tietojoukon käytetty 1983 American tilastollinen liitos epäsuorasti.<p> </p>Tiedot on lueteltu polttoaineenkulutus eri autot-Mailia gallona sekä tiedot luku, ohjelma siirtymää, tehoa, kokonaispaino, ja acceleration kohden.<p> </p><b>Käyttö:</b> ennustaa polttoaineen taloudessa 3 moniarvoisen erillinen määritteet ja 5 jatkuva määritteiden perusteella. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> StatLib, Carnegie Mellon Universityn (1993). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede </td>
</tr>

<tr>
  <td valign=top>Pima Indians Diabetes binaarinen luokitus-tietojoukko</td>
  <td valign=top>
Alijoukkoa Diabetes kansallisia Projektinhallintainstituutin ja ruoansulatuskanavan ja munuaiset palstassa tietokannan tietoja. Tietojoukon on suodatettu keskittyä Pima Intia perintöä nainen potilaille. Tiedot sisältävät lääketieteellinen tietoja, kuten glukoosin ja inuliinisiirapin tasot sekä elämäntavan.<p> </p><b>Käyttö:</b> ennustaa onko aihe diabetes (binaarinen luokitus). <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Sigillito vastaan (1990). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml "</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede </td>
</tr>

<tr>
  <td valign=top>Ravintolaan asiakastietoja</td>
  <td valign=top>
Joukko metatietojen asiakkaita, mukaan lukien demografia ja asetukset.<p> </p><b>Käyttö:</b> käyttää yhdessä muiden kaksi ravintolaan tietojoukkojen, tämä tietojoukko kouluttaminen ja testaa suosittelija järjestelmän. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Bache, K. ja Lichman, M. (2013). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede.
  </td>
</tr>

<tr>
  <td valign=top>Ravintolaan ominaisuuksien tiedot</td>
  <td valign=top>
Joukko metatietoa ravintolat ja niiden ominaisuudet, kuten ruoka tyyppi, syöminen tyylin ja sijainnin.<p> </p><b>Käyttö:</b> käyttää yhdessä muiden kaksi ravintolaan tietojoukkojen, tämä tietojoukko kouluttaminen ja suosittelija järjestelmän testaaminen. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Bache, K. ja Lichman, M. (2013). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede.
  </td>
</tr>

<tr>
  <td valign=top>Ravintolaan luokitukset</td>
  <td valign=top>
Sisältää luokitukset antamien ravintolat käyttäjien asteikolla 0 2.<p> </p><b>Käyttö:</b> käyttää yhdessä muiden kaksi ravintolaan tietojoukkojen, tämä tietojoukko kouluttaminen ja testaa suosittelija järjestelmän. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Bache, K. ja Lichman, M. (2013). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede.
  </td>
</tr>

<tr>
  <td valign=top>Teräksen Annealing usean luokan tietojoukko</td>
  <td valign=top>
Tämä tietojoukko sisältää sarjan tietueiden terästä annealing yritykset fyysinen määritteet (leveys, paksuutta, tyyppi (kaapeli, taulukon jne.), jotka johtuvat teräksen tyyppejä.<p> </p><b>Käyttö:</b> ennustaa jokin kaksi numeerinen luokan määritteet; kovuus tai voimakkuus. Voi myös analysoida korrelaatioita kesken määritteet.<p> </p>Teräslajit noudattamalla set-standardia, SAE ja muiden organisaatioiden määrittämän. Etsimäsi tietyn "luokka" (luokka-muuttuja) ja haluat ymmärtää tarvittavat arvot. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Britannian punta, D. & Buntine W. (NA). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: Kalifornian University, tiedot koulun ja tietokoneen tiede <p> </p>Täältä löytyvät teräslajit hyödyllisiä opas: <a href="http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf">http://www.outokumpu.com/SiteCollectionDocuments/Outokumpu-steel-grades-properties-global-standards.pdf</a>
  </td>
</tr>

<tr>
  <td valign=top>Kuvan tiedot</td>
  <td valign=top>
Suuri energiajärjestelmien gamma osan bursts sekä taustaäänet tietueita, sekä Simuloitu Monte Carlo menetelmällä.<p> </p>Sen tarkoituksena on maassa ilman Cherenkov gamma eivätkä, tilastollisten menetelmien avulla voit erotella haluamasi signaali (Cherenkov säteilyn suihkut) ja taustaäänet (hadronic suihkut cosmic säteet yläkulmassa ilmakehässä Mahdollisuuden löytäjä) tarkkuuden parantamiseksi.<p> </p>Tiedot on jo valmiiksi käsitellyt luomalla lopettamiseen klusterin long-akseli on pystysuuntaisen kohti kameran keskelle. Tämä ellipsin (jota kutsutaan usein Hillas parametrit) ominaisuudet ovat kuva parametrit, joita voi käyttää syrjintää.<p> </p><b>Käyttö:</b> ennustaa onko Tervetuloa kuva edustaa signaali tai taustan vähentämiseksi.<p> </p><b>Huomautus:</b> yksinkertainen luokitus tarkkuutta ei ole kuvaava tämä tiedoille jälkeen luokittelussa taustan tapahtuman signaalin sellaisenaan huonommat kuin luokittelussa signaalin tapahtumasta taustana. Eri valitsimen vertailu, OHJETIEDOSTO kaavio on tarkoitus käyttää. Todennäköisyys, että hyväksymällä taustan tapahtuman kuin signaali on oltava jokin seuraavista kynnysarvoista alla: 0,01, 0.02, 0,05, 0,1 tai 0,2.<p> </p>Huomaa, että taustan tapahtumien (h, hadronic suihkut) määrä on aliarvioituja, olisi reaali mitat h tai ääni-luokan edustaa tapahtumien useimpia. <p> </p><b>Aiheeseen liittyvät Oheistiedot:</b> Bock, R.K. (1995). UCI Konepohjaisten Oppimistekniikoiden säilöön <a href="http://archive.ics.uci.edu/ml">http://archive.ics.uci.edu/ml</a>. Irvine, CA: University, Kalifornian, tiedot koulun </td>
</tr>

<tr ID=weather-dataset>
  <td valign=top>Sää-tietojoukko</td>
  <td valign=top>
Tunnin välein maa-pohjainen sää huomautukset-NOAA (<a href="http://cdo.ncdc.noaa.gov/qclcd_ascii/, merged data from 201304 to 201310">201304 201310 yhdistetyt tiedot</a>).<p> </p>Tiedot kattaa huomioita sähköpostit airport sää asemat, aika-asteikon huhtikuun-lokakuussa 2013 sen päällä. Ennen kuin lataaminen Azure ML Studio dataset käsiteltiin seuraavasti:<ul><li>Sää aseman tunnukset on nyt yhdistetty vastaavan airport tunnukset</li><li>Ei ole liitetty 70 busiest airports sää asemat on suodatettu</li><li>Date-sarake on jaettu eri vuoden, kuukauden ja päivän sarakkeeseen</li><li>Seuraavat sarakkeet valittiin: AirportID, vuosi, kuukausi, päivä, ajan, aikavyöhyke, SkyCondition, näkyvyyden, WeatherType, DryBulbFarenheit, DryBulbCelsius, WetBulbFarenheit, WetBulbCelsius, DewPointFarenheit, DewPointCelsius, RelativeHumidity, tuulen nopeus, WindDirection, ValueForWindCharacter, StationPressure, PressureTendency, PressureChange, SeaLevelPressure, RecordType, HourlyPrecip, Altimeter</li></ul>
  </td>
</tr>

<tr ID=wikipedia-sp-500-dataset>
  <td valign=top>Wikipedia SP 500 tietojoukko</td>
  <td valign=top>
Tietoja johdetaan Wikipedia (<a href="http://www.wikipedia.org/">http://www.wikipedia.org/</a>) artikkeleita jokaisen S & P 500 yrityksen, tallennettu XML-tietojen perusteella.<p> </p>Ennen kuin lataaminen Azure ML Studio dataset käsiteltiin seuraavasti:<ul><li>Pura tekstisisältöön jokaiselle yritykselle</li><li>Wiki-muotoilun poistaminen</li><li>Poista muita kuin aakkosnumeerisia merkkejä</li><li>Muuttaa kaikki tekstin pieniksi kirjaimiksi</li><li>Tunnetut yrityksen luokat on lisätty</li></ul><p> </p>Huomaa, että jotkin yritykset artikkeliin ei löytynyt, jotta tietueiden määrä on pienempi kuin 500.
  </td>
</tr>





<tr ID=direct-marketing>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/direct_marketing.csv">direct_marketing.csv</a></td>
  <td valign=top>
Tietojoukon sisältää asiakastietoja ja merkintöjen vastauksensa takaisin suoraan Postitusosoite markkinointikampanjan tietoja. Kullakin rivillä on asiakkaalle. Tietojoukon sisältää 9 ominaisuuksia käyttäjän demografia ja aiempia toiminta ja otsikko 3 saraketta (käy, muunto, ja käyttävät).  Siirry on binaarisarakkeen, joka ilmaisee, että markkinointikampanja jälkeen avoimelle asiakas, muuntaminen ilmoittaa asiakkaan ostettu jotakin, jota käyttävät on summa, joka kului.  Tietojoukon tuli markkinoille mukaan Kevin Hillstrom MineThatData sähköpostin Analytics ja tietojen kaivos todennus varten.
  </td>
</tr>

<tr ID=lyrl2004-tokens-test>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_test.csv">lyrl2004_tokens_test.csv</a></td>
  <td valign=top>
Testaa esimerkkejä RCV1 V2 Reuters uutisia tietojoukossa ominaisuudet. Tietojoukon on 781K uusia artikkeleita sekä niiden tunnukset (ensimmäisen sarakkeen tietojoukko). Kunkin artikkelin tokenized stopworded, ja stemmed. Tietojoukon tuli markkinoille David mukaan. D. Lewis.
  </td>
</tr>

<tr ID=lyrl2004-tokens-train>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/lyrl2004_tokens_train.csv">lyrl2004_tokens_train.csv</a></td>
  <td valign=top>
Esimerkkejä RCV1 V2 Reuters uutisia tietojoukossa ominaisuudet. Tietojoukon on 23K uusia artikkeleita sekä niiden tunnukset (ensimmäisen sarakkeen tietojoukko). Kunkin artikkelin tokenized stopworded, ja stemmed. Tietojoukon tuli markkinoille David mukaan. D. Lewis.
  </td>
</tr>

<tr ID=intrusion-detection>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a><br></td>
  <td valign=top>
Tietojoukko: KDD Cup 1999 Knowledge Discovery and tiedonlouhinnalla Työkalut kilpailua (<a href="http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html">kddcup99.html</a>).<p> </p>Tietojoukon on ladattu ja tallennettu Azure-Blob-säiliö (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/network_intrusion_detection.csv">network_intrusion_detection.csv</a>) ja sisältää sekä koulutus testaaminen tietojoukkoja. Koulutus-tietojoukko on noin 126K rivi- ja sarakeotsikot, mukaan lukien 43 sarakemuodossa 3 sarakkeet ovat osa Tarratiedot ja numeerinen ja merkkijono/categorical ominaisuuksien koostuva 40 sarakkeet ovat käytettävissä koulutus malli. Testi-tiedoissa on noin esimerkit samoja 43 sarakkeita kuin koulutus tiedot ja testaa 22.5K.

  </td>
</tr>

<tr ID=rcv1-v2-topics-qrels>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/rcv1-v2.topics.qrels.csv">rcv1 v2.topics.qrels.csv</a></td>
  <td valign=top>
Aiheen varaukset uutisia artikkeleita RCV1 V2 Reuters uutisia tietojoukko. Uutisia artikkelissa voi määrittää useita ohjeaiheita. Kunkin rivin muoto on "<topic name>  <document id> 1". Tietojoukon sisältää 2.6M aiheen tehtävät. Tietojoukon tuli markkinoille David mukaan. D. Lewis.
  </td>
</tr>

<tr ID=student-performance>
  <td valign=top><a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a></td>
  <td valign=top>
Tämä tieto tulee KDD Cup 2010 Student suorituskyvyn arviointi todennus (<a href="http://www.kdd.org/kdd-cup-2010-student-performance-evaluation">opiskelijoiden suorituskyvyn arviointi</a>). Käytettävät tiedot on joukko Algebra_2008_2009, koulutus (Stamper, J., Niculescu-Mizil, A., Ritter, s Gordon, G.J. ja Koedinger, K.R. (2010). Algebran voin 2008 2009. Todennus KDD Cup 2010 oppilaitoksille tietojen kaivos todennus tietojoukkoon. Etsi <a href="http://pslcdatashop.web.cmu.edu/KDDCup/downloads.jsp">downloads.jsp</a> tai <a href="http://www.kdd.org/sites/default/files/kddcup/site/2010/files/algebra_2008_2009.zip">algebra_2008_2009.zip</a>.<p> </p>Tietojoukon on ladattu ja tallennettu Azure-Blob-säiliö (<a href="https://azuremlsampleexperiments.blob.core.windows.net/datasets/student_performance.txt">student_performance.txt</a>) ja sisältää tutoring järjestelmän opiskelija lokitiedostojen. Annettu ominaisuuksiin kuuluvat ongelma tunnuksen ja lyhyt kuvaus, opiskelija, aikaleima ja kuinka monta yritykset käänteisen ennen ongelman ratkaisemisessa oikein. Alkuperäinen tietojoukko on 8.9M tietueet, tämä tietojoukko on jo alas näyte 100K ensimmäistä riviä. Tietojoukon on erilaisia sarkaineroteltuun sarakkeet 23: numeerinen, categorical, ja aikaleiman.

  </td>
</tr>




</table>


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
