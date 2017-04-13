<properties 
    pageTitle="Koneen Learning mallin suorituskyvyn arvioiminen | Microsoft Azure" 
    description="Kerrotaan, miten voit arvioida Azure koneen Learning mallin suorituskykyä." 
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
    ms.date="08/19/2016" 
    ms.author="bradsev;garye" />


# <a name="how-to-evaluate-model-performance-in-azure-machine-learning"></a>Miten Azure koneen Learning mallin suorituskyvyn arvioiminen

Tässä ohjeaiheessa kerrotaan, miten voit arvioida Azure koneen Learning Studiossa mallin sekä annetaan lyhyt kuvaus käytettävissä olevat arvot tämän tehtävän. Kolme yleisiä valvonnan alaisena learning tilanteita, joissa esitetään: 

* LOGREGR-funktio
* Binaarinen luokittelu 
* multiclass luokittelu

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


Mallin suorituskyvyn arvioiminen on yksi core tietojen tiede prosessin vaiheita. Kertoo, miten onnistuu, tietojoukko tulosmalli (ennusteiden) on ollut koulutetun mallin mukaan. 

Azure Konepohjaisten Oppimistekniikoiden tukee mallin Evaluation (arviointi) – kaksi sen tärkeimmät konepohjaisten oppimistekniikoiden moduulit: [Arvioi mallin] [ evaluate-model] ja [Rajat Vahvista mallin][cross-validate-model]. Näitä moduuleja avulla näet, miten mallin suorittaa kannalta arvot koneen oppiminen ja tilastot yleisimmin käytettyjä määrä.

##<a name="evaluation-vs-cross-validation"></a>Arviointi ja rajat vahvistus##
Arviointi ja cross vahvistus on vakio tapaa mallin suorituskykyä. Molemmat Luo arvioinnin mittarit, jonka voit tarkastaa tai verrata niitä muiden mallien.

[Mallin arvioiminen] [ evaluate-model] odottaa scored tietojoukko syötteen (tai haluat vertailla 2 eri mallien tapauksessa 2). Tämä tarkoittaa, että haluat kouluttaminen [Kouluttaminen mallin] mallin[ train-model] joitakin dataset [Pistemäärän malli] -moduulin ja tee ennusteiden[ score-model] moduuli, ennen kuin voit arvioida tuloksia. Laskennan on scored otsikot/mahdollisuuden todennäköisyyden sekä tosi otsikot, jotka kaikki ovat tuloste [Pistemäärän mallin] pohjalta[ score-model] moduuli.

Vaihtoehtoisesti voit tehdä cross vahvistus tekemällä hakuun liittyviä pistemäärän junassa arvioi toiminnot (10 taitosten tekeminen) automaattisesti-kenttään annettavat tiedot eri alijoukot. Kenttään annettavat tiedot jaetaan 10 osaa, joissa yksi on varattu testikäyttöön, ja muut 9 koulutusta. Tämä toimenpide toistetaan 10 kertaa ja laskenta-arvot ovat keskiarvo. Näin määrittämiseen, miten hyvin mallin generalize uusi tietojoukkoja. [Usean Vahvista mallin] [ cross-validate-model] moduuli tulee auttamaan kouluttamattomia mallin ja jotkin nimetty tietojoukko ja tulostaa arvioinnin tulokset kaikkien 10 taitosten tekeminen lisäksi pisteiden tulokset.

Seuraavissa osissa on yksinkertainen regression ja luokittelu mallien luominen ja arvioi niiden suorituskyvyn sekä [Arvioimaan mallin] [ evaluate-model] ja [Rajat Vahvista mallin] [ cross-validate-model] moduulit.

##<a name="evaluating-a-regression-model"></a>Regressiosuoran mallin arvioiminen##
Oletetaan, että haluat ennustaa auton hinta käyttämällä joitakin toimintoja, kuten dimensiot, tehoa, ohjelma Etkö ja niin edelleen. Tämä on tyypillinen regression ongelman kohde muuttujan (*Hinta*) missä jatkuva numeerisen arvon. Olemme mahtuu yhden muuttujan lineaarinen regressio mallin, että tiettyjä Auto-ominaisuus-arvoja voi ennustaa kyseisen auton hinta. Regressiosuoran tämän mallin avulla voidaan sija on koulutus, sama tietojoukko. Kun on kaikkien autojen budjetoidut hinnat, on arvioi mallin suorituskyvyn katsomalla paljonko ennusteiden poiketa tosiasialliset hinnat keskimäärin. Esitä tämä Käytämme *Auto hinta tiedot (raaka) tietojoukko* käytettävissä Azure koneen Learning Studiossa **Tallennettu tietojoukkoja** -osassa.
 
###<a name="creating-the-experiment"></a>Kokeen luominen###
Lisää seuraavat moduulit työtilan Azure koneen Learning Studiossa:

- Autojen hintatiedot (raaka)
- [Muuttujan lineaarinen regressio][linear-regression]
- [Junassa malli][train-model]
- [Tulos-malli][score-model]
- [Mallin arvioiminen][evaluate-model]


Yhdistä portit, kuten alla olevassa kuvassa 1 ja määritä [Junassa mallin] otsikko-sarakkeessa[ train-model] *Hinta*moduuli.
 
![Regressiosuoran mallin arvioiminen](media/machine-learning-evaluate-model-performance/1.png)

Kuva 1. Arvioidaan Regression malli.

###<a name="inspecting-the-evaluation-results"></a>Tarkistetaan arvioinnin tulokset###
Kun olet suorittanut koe, voit valita [Arvioi mallin] tulostus-porttiin[ evaluate-model] moduuli ja valitse *Visualisoi* arvioinnin tulokset. Käytettävissä regression mallien laskenta-arvot ovat: *Suora virhe tarkoittaa*, *Pääkansion suorien virhe tarkoittaa*, *Suhteellinen suora virhe*, *Suhteellinen neliö virhe*ja *Määrittäminen kertoimen*.

Termin "Virhe" tässä erotusta ennustettu arvo ja tosi arvo. Absoluuttinen arvo tai neliön tämä on yleensä lasketaan kannattaa tallentaa kaikki esiintymät koko yhteensä virheen suuruus budjetoidut ja tosi-arvon välinen ero voi olla negatiivinen joissakin tapauksissa. Virhe-arvot mittaa ennakoivan suorituskyvyn regression-mallin, myös sen ennusteiden tosi arvojen keskiarvon keskihajonta. Pienet virhearvo tarkoittaa malli on tarkempaa ennusteiden tekemistä. Yleinen virheen itseisarvon on 0, että mallin sopii täydellisesti tiedot.

Korrelaatiokerroin, jossa käytetään myös nimitystä R neliö, on myös vakio tapa mittaaminen, miten hyvin mallin sopii tietoihin. Se voidaan tulkita variaation malli on esitetty, kuinka suuri osa. On korkeampi osuus paremmalta tässä tapauksessa jossa 1 merkitsee sopivaa Sovita.
 
![Lineaarisen regressiosuoran arvioinnin arvot](media/machine-learning-evaluate-model-performance/2.png)

Kuva 2. Lineaarisen regressiosuoran arvioinnin arvot.

###<a name="using-cross-validation"></a>Ristiviittauksen vahvistus käyttäminen###
Kuten edellä mainittiin, voit suorittaa toistuvien koulutusta, näkyvissä pistemäärä ja arvioinnit automaattisesti käyttämällä [Rajat Vahvista mallin] [ cross-validate-model] moduuli. Tässä tapauksessa on tietojoukko, auttamaan kouluttamattomia mallin ja [Rajat Vahvista mallin] [ cross-validate-model] moduuli (Katso alla olevassa kuvassa). Huomaa, että haluat määrittää otsikko-sarakkeessa *Hinta* [Rajat Vahvista mallin] [ cross-validate-model] moduulin ominaisuudet.

![Regressiosuoran mallin rajat vahvistaminen](media/machine-learning-evaluate-model-performance/3.png)

Kuva 3. Rajat-Regression mallin vahvistaminen.

Kun olet suorittanut koe, voit tarkastaa arvioinnin tulokset napsauttamalla oikealle output porttiin [Rajat Vahvista mallin] [ cross-validate-model] moduuli. Yksityiskohtaisen näkymän määritetty antaa iteraation (taitettava) ja pisteiden vaikutukset kaikkien arvot (kuva 4).
 
![Regressiosuoran mallin rajat vahvistus tulokset](media/machine-learning-evaluate-model-performance/4.png)

Kuva 4. Regressiosuoran mallin rajat vahvistus tulokset.

##<a name="evaluating-a-binary-classification-model"></a>Arvioimisen binaarinen luokitus-malli##
Binaarinen luokitus-tilanne kohde-muuttuja on vain kaksi mahdollisten tulosten esimerkiksi: {0, 1} tai {TOSI, EPÄTOSI}, {negatiivinen, positiivinen}. Oletetaan, että annetaan aikuisen työntekijöiden osittain tietojoukko demografiset ja työsuhteen muuttujat ja että sinulta kysytään, tuotto-tason binaarinen muuttujan arvot ennustetaan {"< = 50K", "> 50K"}. Toisin sanoen negatiivinen luokka on työntekijät, jotka tehdä pienempi tai yhtä suuri kuin 50K vuodessa ja positiivinen luokan kaikki muut työntekijät. Regression skenaariota, kuten Microsoft kouluttaminen mallin, pisteet tietoja ja arvioida tuloksia. Tärkein ero on Azure koneen Learning laskee arvot ja tulostaa valinta. Esitä tulot tason tekstinsyöttö skenaarion Käytämme [aikuisen](http://archive.ics.uci.edu/ml/datasets/Adult) tietojoukon luominen Azure koneen Learning kokeessa ja arvioida kahden luokan logistista regressiota mallin tavallisimmat binaarinen valitsin.

###<a name="creating-the-experiment"></a>Kokeen luominen###
Lisää seuraavat moduulit työtilan Azure koneen Learning Studiossa:

- Aikuisen laskenta tulot binaarinen luokitus-tietojoukko
- [Kahden luokan logistista regressiota][two-class-logistic-regression]
- [Junassa malli][train-model]
- [Tulos-malli][score-model]
- [Mallin arvioiminen][evaluate-model]

Yhdistä portit, kuten alla olevassa kuvassa 5 ja määritä [Junassa mallin] otsikko-sarakkeessa[ train-model] *tulo*-moduuli.

![Arvioimisen binaarinen luokitus-malli](media/machine-learning-evaluate-model-performance/5.png)

Kuva 5. Arvioidaan binaarinen luokitus-malli.

###<a name="inspecting-the-evaluation-results"></a>Tarkistetaan arvioinnin tulokset###
Kun olet suorittanut koe, voit valita [Arvioi mallin] tulostus-porttiin[ evaluate-model] moduuli ja valitse *Visualisoi* arvioinnin tulokset (kuva 7). Käytettävissä binaarinen luokitus-mallien laskenta-arvot ovat: *tarkkuutta*, *tarkkuus*, *peruuttaa*, *F1 pisteet*ja *AUC*. Lisäksi moduulin tulostaa sekaannusta matriisin näyttäminen positiivisten TOSI, EPÄTOSI negatiiviset, tunnistettujen, ja true negatiiviset sekä *OHJETIEDOSTO*, *Tarkkuus/peruuttaminen*ja *Nosta* kaaria määrän.

Tarkkuus on yksinkertaisesti osuus oikein luokitellaan esiintymät. Se on yleensä ensimmäinen arvo, voit tarkastella valitsimen laskettaessa. Kuitenkin, kun testitiedot ovat epätasapainoisia (johon useimmat esiintymien kuuluvat johonkin luokat) tai kiinnostavan Lisää jompikumpi luokkien suorituskyvyn tarkkuutta ei todella siepata valitsimen tehokkuutta. Tulo tason luokitus tilanteessa oletetaan testaat tietoja, jossa 99 % esiintymien vastaavat henkilöt, jotka ansaita pienempi tai yhtä suuri kuin 50K vuodessa. Se on mahdollista saada 0.99 tarkkuutta ennakoimista luokan mukaan "< = 50K" kaikkien esiintymien. Valitsimen näkyy tällöin tekeminen voidaan yleinen hyvin, mutta todellisuudessa epäonnistuu luokitella jokin high-income henkilöille (1 %) oikein.

Tästä syystä on hyödyllinen laskemiseen muut arvot, jotka siepata laskennan tiettyihin ominaisuuksia. Ennen siirtymistä tällaisten arvot yksityiskohdat, on tärkeää ymmärtää binaarinen luokitus-arviointi sekaannusta matriisissa. Luokan tarrojen koulutusta määrittäminen voi suorittaa vain 2 mahdollisia arvoja, jotka on yleensä viittaavat positiivinen tai negatiivinen. Positiivisten ja negatiivisten esiintymät, jotka valitsimen ennustaa oikein kutsutaan tosi positiivisten (TP) ja true negatiiviset (TN) tarpeen mukaan. Väärin luokitellaan esiintymät kutsutaan vastaavasti tunnistettujen (FP) ja EPÄTOSI negatiiveja (FN). Sekaannusta matriisissa on yksinkertaisesti taulukon, jossa näkyvät esiintymät, jotka kuuluvat kunkin 4 luokan määrää. Azure koneen Learning päättää automaattisesti joka kahteen luokkaan tietojoukossa on positiivinen luokka. Luokan otsikot ovat totuusarvo tai kokonaisluvut, jos "tosi" tai "1" nimetty esiintymät määritetään positiivinen luokka. Jos otsikot ovat merkkijonoja, kuten tulo-tietojoukko tapauksessa otsikot lajitellaan aakkosjärjestykseen ja ensimmäisen tason valitaan on negatiivinen luokan, kun toisen tason on positiivinen luokan.

![Binaarinen luokitus sekaannusta matriisi](media/machine-learning-evaluate-model-performance/6a.png)

Kuva 6. Binaarinen luokitus sekaannusta matriisi.

Siirtymällä tulot luokitus-ongelma, haluat pyytää useisiin arviointikysymyksiin, jotka Auta meitä ymmärtää käytetään valitsimen suorituskykyä. Hyvin luonnollinen kysymys on: "henkilöiden kanssa, jotta voi ennustaa mallin ulos > 50 K (TP + FP), kuinka monta on luokiteltu oikein (TP)?" Tämä kysymys voi vastata katsomalla **tarkkuus** -malli, joka on positiivisten, jotka on luokiteltu oikein osuus: TP/(TP+FP). Toisen yleisiä kysymys on "ulos suuri, jotta työntekijät, joiden tulo > 50 k (TP + FN), kuinka monta valitsimen luokitella oikein (TP)". Tämä on todella **peruuttaa**tai Tosi positiivinen korko: TP/(TP+FN) valitsimen. Voit havaita olevan ilmeisimmät trade-off, tarkkuutta ja peruuttaminen välillä. Esimerkiksi valita suhteellisen tasapainoinen tietojoukko, valitsin, joka ennustaa enimmäkseen positiivinen esiintymät on hyvin peruuttaminen, mutta mieluummin pieni tarkkuus negatiivinen esiintymien määrän misclassified tuloksena on suuri määrä tunnistettujen. Jos haluat nähdä, kuinka nämä kaksi arvot vaihtelevat piirto, voit valita ' Tarkkuus/peruuttaminen' käyrän arvioinnin tuloksen tulostus-sivulla (päällimmäisenä Vasen osa kuva 7).

![Binaarinen luokitus arvioinnin tuloksen](media/machine-learning-evaluate-model-performance/7.png) kuva 7. Binaarinen luokitus arvioinnin tulokset.

Toinen Aiheeseen liittyvät arvo, jota käytetään usein on **F1 tulos**, joka kestää tarkkuutta ja peruuttaminen huomioon. Se on 2 nämä arvot harmonisen ja sellaisenaan laskettu: F1 = 2 (tarkkuus x peruuttaminen) / (tarkkuus + peruuttaminen). F1 tulos on hyvä tapa tehdä yhteenvedon laskennan yhden numeron, mutta se on aina hyvä tarkkuutta ja peruuttaminen yhdessä ja ymmärtää helpommin, miten valitsimen toimii.

Lisäksi jokin tutkia ja EPÄTOSI positiivinen korko **Vastaanotin toimivien säännössä käytettävän ominaisuuden (OHJETIEDOSTO)** -käyrän ja vastaavan **Alueen kohdassa käyrän (AUC)** -arvon true positiivinen korko. Mitä lähempänä tämän käyrän on vasemmassa yläkulmassa, sitä parempia valitsin suorituskyky on (, joka on suurentaminen tosi positiivinen korko aikana pienentäminen EPÄTOSI positiivinen korko). Kaaria, jotka ovat lähellä vinosti piirron valitsimet, jotka ennusteiden, joka on lähellä satunnaisia arvata tapana tuloksen.

###<a name="using-cross-validation"></a>Ristiviittauksen vahvistus käyttäminen###
Regressiosuoran esimerkin emme voi suorittaa cross vahvistus toistuvasti kouluttaminen, pisteet ja arvioida eri tietojen alijoukot automaattisesti. Vastaavasti Käytämme [Rajat Vahvista mallin] [ cross-validate-model] moduuli, auttamaan kouluttamattomia logistista regressiota-mallin ja tietojoukko. Otsikko-sarakkeessa on määritettävä *tulot* [Rajat Vahvista mallin] [ cross-validate-model] moduulin ominaisuudet. Käynnissä kokeessa ja valitsemalla [Rajat Vahvista mallin] oikealle output porttiin jälkeen[ cross-validate-model] moduuli-näkymässä näkyy kunkin taitettava binaarinen luokitus metrisillä arvot lisäksi keskiarvo ja keskihajonta kunkin. 
 
![Binaarinen luokitus mallin rajat vahvistaminen](media/machine-learning-evaluate-model-performance/8.png)

Kuva 8. Rajat-binaarinen luokitus mallin vahvistaminen.

![Usean vahvistus tulosten binaarinen valitsin](media/machine-learning-evaluate-model-performance/9.png)

Kuva 9. Binaarinen valitsimen rajat vahvistus tulokset.

##<a name="evaluating-a-multiclass-classification-model"></a>Arvioimisen Multiclass luokitus-malli##
Tämän kokeen Käytämme Suositut [Tiina](http://archive.ics.uci.edu/ml/datasets/Iris "Tiina") tietojoukko jossa on 3 erilaista (luokat) Tiina kasvista esiintymät. 4 ominaisuuden arvot (sepal pituus ja leveys- ja Terälehti pituuden ja leveyden) jokaiselle esiintymälle on. Edellisen kokeissa koulutus ja testataan käyttämällä samaa tietojoukkoja mallit. Tässä kohdassa Käytämme [Jaetut tiedot] [ split] luoda 2 tietojen alijoukot, kouluttaminen, ensimmäisenä ja pisteet ja arvioi toisessa moduuli. Tiina tietojoukko on julkisesti saatavilla [UCI Machine Learning säilössä](http://archive.ics.uci.edu/ml/index.html), ja sen voi ladata [Tietojen tuominen] [ import-data] moduuli.

###<a name="creating-the-experiment"></a>Kokeen luominen###
Lisää seuraavat moduulit työtilan Azure koneen Learning Studiossa:

- [Tietojen tuominen][import-data]
- [Multiclass päätös metsää][multiclass-decision-forest]
- [Tiedon jakaminen][split]
- [Junassa malli][train-model]
- [Tulos-malli][score-model]
- [Mallin arvioiminen][evaluate-model]

Yhdistä portit, kuten alla olevassa kuvassa 10.

Määritä selitteen sarakkeen indeksiä [Junassa mallin] [ train-model] moduulin 5. Tietojoukon on ei ole otsikkoriviä, mutta olemme varma, että luokan otsikot ovat viidennelle-sarakkeessa.

Valitse [Tuontitiedot] [ import-data] moduulin ja *tietolähde* -ominaisuuden arvoksi *Http sivuston URL-osoite*ja http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data *URL-Osoitteen* määrittäminen.

Määritä nimittäjä esiintymien koulutus [Jaetut tiedot] käytettävän[ split] moduuli (esimerkiksi 0.7).
 
![Arvioimisen Multiclass valitsin](media/machine-learning-evaluate-model-performance/10.png)

Kuva 10. Arvioimisen Multiclass valitsin

###<a name="inspecting-the-evaluation-results"></a>Tarkistetaan arvioinnin tulokset###
Suorita kokeen ja valitse tulosteen porttiin [Arvioi mallin][evaluate-model]. Arvioinnin tulokset esitetään sekaannusta matriisin lomakkeen tällöin. -Matriisissa on todellinen budjetoidut esiintymät 3 kaikissa luokissa ja.
 
![Multiclass luokitus arvioinnin tulokset](media/machine-learning-evaluate-model-performance/11.png)

Kuva 11. Multiclass luokitus arvioinnin tulokset.

###<a name="using-cross-validation"></a>Ristiviittauksen vahvistus käyttäminen###
Kuten edellä mainittiin, voit suorittaa toistuvien koulutusta, näkyvissä pistemäärä ja arvioinnit automaattisesti käyttämällä [Rajat Vahvista mallin] [ cross-validate-model] moduuli. Tarvitset tietojoukko, auttamaan kouluttamattomia mallin ja [Rajat Vahvista mallin] [ cross-validate-model] moduuli (Katso alla olevassa kuvassa). Uudelleen sinun on määritettävä [Rajat Vahvista mallin] otsikko-sarakkeessa[ cross-validate-model] moduuli (sarakkeen indeksiä 5 tässä tapauksessa). Kun käynnissä kokeessa ja valitsemalla oikealla tulosteen portin [Rajat Vahvista mallin][cross-validate-model], voit tutkia metrisillä arvot kunkin taitettava sekä keskiarvo ja keskihajonta. Tässä näkyvät arvot ovat samanlaiset kuin käsitellyt binaarinen luokitus tapauksessa niistä. Huomaa, että kuitenkin multiclass luokittelu, TOSI positiivisten ja negatiiveja ja EPÄTOSI positiivisten ja negatiiviset tietojenkäsittely on valmis laskemalla luokan kohti, välein kuin ei ole yleistä positiivinen tai negatiivinen luokan. Esimerkiksi laskettaessa tarkkuus- tai "Tiina setosa"-luokan peruuttaminen, sen oletetaan, että tämä on positiivinen luokan ja muut kuin negatiivinen.
 
![Multiclass luokitus mallin rajat vahvistaminen](media/machine-learning-evaluate-model-performance/12.png)

Kuva 12. Rajat-Multiclass luokitus mallin vahvistaminen.


![Usean vahvistus tulosten Multiclass luokitus-malli](media/machine-learning-evaluate-model-performance/13.png)

Kuva 13. Rajat-vahvistuksen tulokset Multiclass luokitus-mallin.


<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
 
