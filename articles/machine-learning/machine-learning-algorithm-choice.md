<properties
    pageTitle="Valitseminen machine learning algoritmit | Microsoftin Azure"
    description="Miten valita Azure Machine Learning algoritmeja valvottua ja varoitukseksi oppimiseen, klusterointi, luokitus tai regression kokeisiin."
    services="machine-learning"
    documentationCenter=""
    authors="brohrer"
    manager="jhubbard"
    editor="cgronlun"
    tags=""/>
    
<tags
    ms.service="machine-learning"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="08/09/2016"
    ms.author="brohrer;garye" />

# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Miten valita algoritmeja Microsoftin Azure Machine Learning

Vastaus kysymykseen "Mitä oppimisen algoritmi tulisi käyttää koneen?" on aina "Joskus." Se riippuu koon, laadun ja tietojen luonteeseen. Se riippuu mitä haluat tehdä ja vastauksia. Se riippuu miten matematiikan algoritmin oli käännetty ohjeita käyttämäsi tietokone. Ja se on riippuvainen, kuinka paljon aikaa sinulla on. Jopa kaikkein kokenut tiedot tutkijoiden ei voi tarkistaa algoritmi suorittaa parhaiten, ennen kuin yrität niitä.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Machine Learning algoritmi Cheat taulukko

**Microsoftin Azure koneen Koulujen algoritmi Cheat taulukko** auttaa valitsemaan oikean koneen Koulujen algoritmi ehkäisevän analytics-ratkaisujen algoritmeja Microsoftin Azure Machine Learning-kirjastosta.
Tässä artikkelissa käydään läpi, miten sitä käytetään.

> [AZURE.NOTE] Lataa huijata arkki ja noudattamalla tämän artikkelin kanssa, siirry [Microsoftin Azure Machine Learning Studio algoritmi huijata arkki Koulujen kone](machine-learning-algorithm-cheat-sheet.md).

Tämä huijata arkki on erityinen käyttäjäryhmä mielessä: alussa tiedot Tiedemies ja undergraduate-tason machine learning yrittää valita algoritmin aluksi Azure Machine Learning Studio. Tämä tarkoittaa sitä, että se tekee jotkin yleistyksiä ja oversimplifications, mutta se osoittaa sinun turvalliseen suuntaan. Tämä tarkoittaa myös sitä, että on monia erilaisia algoritmeja ei ole lueteltu tässä. Azure-Machine Learning kasvaessa koskemaan käytettävissä olevat menetelmät yleisemmin lisäämme niitä.

Nämä suositukset ovat käännetty palautetta ja vinkkejä-erän tiedot tutkijoiden ja asiantuntijoiden machine learning. Emme ei ole sopivat kaiken, mutta olen kokeillut yhdenmukaistaa meidän lausunnot huomioon karkea yksimielisyyttä. Erimielisyyden syyt lauseita useimmat alkavat "Joskus..."

### <a name="how-to-use-the-cheat-sheet"></a>Miten huijata arkki

Kaavion otsikot polku ja algoritmi lukea "varten * &lt;polun nimi&gt; * käyttää * &lt;algoritmi&gt;*." Esimerkiksi *"nopeus* Käytä *kahden luokan logistista regressiota*." Joskus useampia sivuliikkeitä koskevat.
Joskus ne eivät ole perfect fit. Ne ovat tarkoitettu suosituksia, niin älä huolehdi että tarkka napin sääntö.
Useita tietoja tutkijoiden minä puhuin ihan kanssa mainittu, löytääkseen parhaan algoritmin vain varma tapa on kokeilla niitä kaikkia.

Tässä on esimerkki [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/) -koe, joka yrittää vastaan samoja tietoja useita algoritmeja ja vertaa tuloksia: [verrata usean luokan valitsimet: kirje tunnustamista](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

>[AZURE.TIP] Voit ladata ja tulostaa kaavion, joka antaa yleiskuvan Machine Learning Studio ominaisuuksia, katso [Yleiskatsaus kaavio Azure Machine Learning Studio ominaisuuksia](machine-learning-studio-overview-diagram.md).

## <a name="flavors-of-machine-learning"></a>Flavors, machine learning

### <a name="supervised"></a>Valvovat

Valvotut Koulujen algoritmien tehdä ennusteita asiakkaillenne perustuu joukko esimerkkejä. Esimerkiksi historialliset pörssikursseja voidaan tulevaisuudessa hinnoilla vaaran arvaus. Edun arvo on merkitty kunkin esimerkin käytetyt koulutukselle – tässä tapauksessa osakekurssi. Valvotut Koulujen algoritmi etsii arvon otsikoita. Se voi käyttää tietoja, jotka saattavat olla merkityksellisiä — päivän, viikon, aikaan, taloudelliset tiedot yrityksen, alan tyyppi, häiritsevä geopolicitical tapahtumien esiintyminen – ja jokainen algoritmi etsii erilaisia kuvioita. Algoritmi on löytänyt parhaan kuvion jälkeen se voi, se käyttää hyväksikäytön tehdä ennusteita asiakkaillenne Nimetön tietoihin testauksen – huomisen hinnat.

Tämä on suosittu ja hyödyllinen machine learning. Yhtä poikkeusta lukuun ottamatta kaikki moduulit Azure Machine Learning niitä valvotaan Koulujen algoritmeja. Tietyt määritetyypit valvotut opiskelun, joita edustaa sisällä Azure Machine Learning: luokittelu, regression ja oudosta tunnistus.

* **Luokitus**. Kun tietoja käytetään ennustaa luokka, valvotut Koulujen kutsutaan myös luokitus. Näin menetellään, kun kuvan liittäminen kuvana "kissa" tai "dog". Kun on vain kaksi vaihtoehtoa, tätä kutsutaan **kahden luokan** tai **binomijakauman luokitus**. Kun on enemmän luokkia, sellaisena kuin se on kun maaliskuussa Madness NCAA turnauksen voittaja ennakoimaan, ongelma kutsutaan **usean luokan luokitus**.

* **Regression**. Arvo on parhaillaan ennustaa kanssa pörssikursseja, valvotut Koulujen kutsutaan regression.

* **Oudosta tunnistus**. Joskus tavoitteena on tunnistaa, jotka ovat yksinkertaisesti epätavallista. Petosten paljastamista harvinaista luottokortin tahansa kaupankäynnin kulujen kuviot ovat esimerkiksi epäilystä. Mahdollisiin muutoksiin on niin paljon ja niin vähän, ettei se ole mahdollista oppia mitä petollisista toimista koulutus esimerkkejä näköinen. Lähestymistapa, joka ottaa oudosta tunnistus on yksinkertaisesti oppia mitä normaali toiminta muistuttaa (käyttäen historia-vilpillisten liiketoimien) ja tunnistaa mitään, joka on merkittävästi erilainen.

### <a name="unsupervised"></a>Varoitukseksi

Arvopisteitä on varoitukseksi oppiminen ei ole niihin liittyviä otsikoita. Sen sijaan algoritmi varoitukseksi Koulujen tavoitteena on järjestää tiedot jollakin tavalla, tai kirjoittaa sen rakennetta. Tämä voi tarkoittaa niin, että se näkyy yksinkertaisempi tai järjestää monimutkaisia tietoja tarkastelemalla eri tapoja löytää tai ryhmittämällä sen klustereihin.

### <a name="reinforcement-learning"></a>Raudoitustöiden oppiminen

Koulujen raudoitustöiden algoritmi saa valita toiminnon vastauksen jokaiseen arvopisteeseen. Koulujen algoritmi saa myös toisen signaalin hetki myöhemmin, joka ilmaisee, miten hyvä päätös.
Tämän perusteella algoritmi Muokkaa strategia sen saavuttamiseksi suurin saat pisteitä. Tällä hetkellä on ei raudoitustöiden Koulujen algoritmi moduulien Azure Machine Learning. Raudoitustöiden oppiminen on yhteinen robotics, jossa ajoin yhdessä vaiheessa anturin lukemat on arvopiste ja algoritmi robotti seuraava toiminto on valittava. On myös luonnollista henkilöä mahtuu asioiden Internet applications.

## <a name="considerations-when-choosing-an-algorithm"></a>Näkökohdat valittaessa algoritmi

### <a name="accuracy"></a>Tarkkuus

Haetaan mahdollista tarkin vastaus ei ole aina tarpeen.
Joskus arvion riittää, sen mukaan, mitä haluat käyttää sitä. Jos näin on, saatat pystyä leikata että käsittelyaika huomattavasti enemmän likimääräisiä menetelmiä pistämistä mukaan. Lisää suunnilleen tavoista toinen etu on, että ne luonnollisesti ne pyrkivät välttämään [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Aika koulutukseen

Minuutteina tai tunteina tarvitse opettaa mallin määrä vaihtelee paljon välillä algoritmeja. Aikaa koulutukseen liittyy usein läheisesti tarkkuus — yksi yleensä mukana toiseen. Lisäksi jotkin algoritmit ovat arvopisteiden määrä herkempiä kuin toiset.
Kun aika on rajoitettu sen aseman valinta algoritmi, erityisesti silloin, kun tiedot on suuri.

### <a name="linearity"></a>Lineaarisuus

Tehdä paljon machine learning algoritmit käyttävät lineaarisuus. Algoritmeja lineaarisen luokitus oletetaan, että luokkia voidaan erottaa toisistaan suoran viivan (tai sen suurempaa kolmiulotteisen analoginen). Näitä ovat logistista regressiota ja tukevat vector koneet (toteutettu Azure Machine Learning).
Algoritmeja lineaarisen regression olettaa, että tietojen trendien seurata suora viiva. Nämä olettamukset eivät ole huono ongelmia, mutta muille ne tuo tarkkuutta.

![Ei-luokan bounday][1]

***Ei-lineaarisissa luokan rajan*** *-että lineaarisen luokitus algoritmi johtaa alhainen tarkkuus*

![Epälineaarinen kehitys tiedot][2]

***Epälineaarinen kehitys tiedot*** *-Luo lineaarisen regression menetelmällä paljon enemmän virheitä kuin on tarpeen*

Niiden vaaroista huolimatta algoritmeja lineaarisen ovat hyvin suosittuja kuin ensimmäisen rivin hyökkäyksen. Ne ovat usein algorithmically helppo ja nopea junaan.

### <a name="number-of-parameters"></a>Parametrien määrä on

Parametrit ovat tiedot Tiedemies saa ottaa algoritmi määritettäessä nupit. Ne ovat numeroita, jotka vaikuttavat algoritmin toimintaa, kuten Virhetoleranssi tai määrä toistoja tai variantteja, miten algoritmi toimii välillä asetukset. Koulutukseen tarvittava aika ja tarkkuuden algoritmi Joskus voi olla melko herkkä saaminen oikeat asetukset. Parametreilla suurten lukujen algoritmit vaativat yleensä löytää hyvä yhdistelmä useimmat kokeiluversio ja-virhe.

Vaihtoehtoisesti on [parametri sweeping](machine-learning-algorithm-parameters-optimize.md) moduuli estää Azure Machine Learning, joka yrittää automaattisesti kaikki parametrin yhdistelmät, voit valita mitä tahansa rakeisuus. Vaikka tämä on hyvä tapa Varmista, että olet Levitetyt parametrin tilaa, joka kuluu junan mallin kasvaa eksponenttimuodossa parametrien määrä.

Upside on että ottaa monta parametria yleensä tarkoittaa, että algoritmi on joustavampi. Usein se voi saavuttaa tarkkuus on erittäin hyvä. Jos löydät oikean yhdistelmän parametriasetukset.

### <a name="number-of-features"></a>Ominaisuuksien määrä

Tiettyjä tietotyyppejä, useita ominaisuuksia voi olla hyvin suuri arvopisteiden määrä verrattuna. Näin menetellään usein genetiikan tai tekstimuotoisia tietoja. Paljon ominaisuuksia voit bog alas joitakin Koulujen algoritmeja tekemistä koulutuksen unfeasibly pitkän aikaa. Tuki Vector-koneet ovat erityisen hyvin soveltuvien tässä tapauksessa (ks. jäljempänä).

### <a name="special-cases"></a>Erityistapauksissa

Learning joitakin algoritmeja tehdä tietyn rakenteen tiedot tai haluttujen tulosten oletuksia. Jos löydät yhden, joka sopii tarpeisiisi, se voi antaa sinulle enemmän hyödyllisiä tuloksia tai nopeampi koulutus kertaa tarkempia ennusteita asiakkaillenne.

|**Algoritmi**|**Tarkkuus**|**Aika koulutukseen**|**Lineaarisuus**|**Parametrit**|**Huomautukset**|
|---|:---:|:---:|:---:|:---:|---|
|**2-luokan luokitus**| | | | | |
|[logistista regressiota](https://msdn.microsoft.com/library/azure/dn905994.aspx)                    | |●|●|5| |
|[päätöksessä toimialuepuuryhmässä.](https://msdn.microsoft.com/library/azure/dn906008.aspx)|●|○| |6| |
|[päätös Viidakko](https://msdn.microsoft.com/library/azure/dn905976.aspx)|●|○| |6|Vähäisen muistin|
|[Päätöksentekopuu akuista](https://msdn.microsoft.com/library/azure/dn906025.aspx)|●|○| |6|Suuren muistin|
|[neural-verkko](https://msdn.microsoft.com/library/azure/dn905947.aspx)|●| | |9|[Lisätietoja mukauttamista on mahdollista](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[pisteiden perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx)|○|○|●|4| |
|[vector tietokoneen tuki](https://msdn.microsoft.com/library/azure/dn905835.aspx)| |○|●|5|Hyvä ominaisuus suuria joukkoja varten|
|[paikallisesti laaja tuki vector koneen](https://msdn.microsoft.com/library/azure/dn913070.aspx)|○| | |8|Hyvä ominaisuus suuria joukkoja varten|
|[Bayes-' kohta kone](https://msdn.microsoft.com/library/azure/dn905930.aspx)| |○|●|3| |
|**Usean luokan luokitus**| | | | | |
|[logistista regressiota](https://msdn.microsoft.com/library/azure/dn905853.aspx)| |●|●|5| |
|[päätöksessä toimialuepuuryhmässä.](https://msdn.microsoft.com/library/azure/dn906015.aspx)|●|○| |6| |
|[päätös Viidakko](https://msdn.microsoft.com/library/azure/dn905963.aspx)|●|○| |6|Vähäisen muistin|
|[neural-verkko](https://msdn.microsoft.com/library/azure/dn906030.aspx)|●| | |9|[Lisätietoja mukauttamista on mahdollista](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[yksi v all-](https://msdn.microsoft.com/library/azure/dn905887.aspx)|-|-|-|-|Katso ominaisuuksia valitun menetelmän kaksi-luokan|
|**Regressio**| | | | | |
|[Lineaarinen](https://msdn.microsoft.com/library/azure/dn905978.aspx)| |●|●|4| |
|[Lineaarinen Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx)| |○|●|2| |
|[päätöksessä toimialuepuuryhmässä.](https://msdn.microsoft.com/library/azure/dn905862.aspx)|●|○| |6| |
|[Päätöksentekopuu akuista](https://msdn.microsoft.com/library/azure/dn905801.aspx)|●|○| |5|Suuren muistin|
|[Nopea toimialuepuuryhmän kvantiili](https://msdn.microsoft.com/library/azure/dn913093.aspx)|●|○| |9|Kohta laskentamenetelmää sijaan jaot|
|[neural-verkko](https://msdn.microsoft.com/library/azure/dn905924.aspx)|●| | |9|[Lisätietoja mukauttamista on mahdollista](http://go.microsoft.com/fwlink/?LinkId=402867)|
|[POISSON](https://msdn.microsoft.com/library/azure/dn905988.aspx)| | |●|5|Teknisesti log-lineaarisen. Toimistorakennusten inventointeja varten|
|[järjestysnumero](https://msdn.microsoft.com/library/azure/dn906029.aspx)| | | |0|Toimistorakennusten arvon järjestämisen osalta|
|**Oudosta tunnistus**| | | | | |
|[vector tietokoneen tuki](https://msdn.microsoft.com/library/azure/dn913103.aspx)|○|○| |2|Erityisen hyvin suuri ominaisuus asettaa|
|[Oudosta KYS-pohjainen tunnistus](https://msdn.microsoft.com/library/azure/dn913102.aspx)| |○|●|3| |
|[K-keinot](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)| |○|●|4|Klusteroinnin algoritmi|


**Ominaisuudet:**

**●** - näyttää erinomainen tarkkuus ja nopea koulutus kertaa lineaarisuus käyttö

**○** - näyttää tarkkuus on hyvä ja joustava koulutus kertaa

## <a name="algorithm-notes"></a>Algoritmi huomautukset

### <a name="linear-regression"></a>Lineaarinen regressio

Kuten aiemmin mainittiin, [lineaarisen regression](https://msdn.microsoft.com/library/azure/dn905978.aspx) sovittaa tietojoukon viivan (tai taso- tai hyperplane). On workhorse, helppo ja nopea, mutta se voi olla liian simplistic joitakin ongelmia.
Tarkista tähän [opetusohjelma lineaarisen regression](machine-learning-linear-regression-in-azure.md).

![Lineaarisen trendin tiedot][3]

***Lineaarisen trendin tiedot***

### <a name="logistic-regression"></a>Logistista regressiota

Vaikka se sisältää aiheuttavalla 'regressio' nimi, logistista regressiota on todella tehokas työkalu [2-luokan](https://msdn.microsoft.com/library/azure/dn905994.aspx) ja [multiclass](https://msdn.microsoft.com/library/azure/dn905853.aspx) luokitus. Se on nopea ja yksinkertainen. Se seikka, että se käyttää, 'S'-sijaan suoran viivan muotoisen käyrän avulla luonnollinen sovitus, tietojen jakaminen ryhmiin. Logistista regressiota antaa lineaarinen luokkien rajat, niin kun käytät sitä, varmista, että lineaarinen lähentämisestä on jotain asut kanssa.

![2-luokan tiedot vain yhden ominaisuuden logistista regressiota][4]

***Logistista regressiota yhden ominaisuuden tietoja kaksi-luokan*** *-luokan raja on piste, jossa logistinen käyrä on juuri lähelle molempien luokkien*

### <a name="trees-forests-and-jungles"></a>Puiden ja metsien jungles

Päätöksen metsien ([Regressio](https://msdn.microsoft.com/library/azure/dn905862.aspx), [kahden luokan](https://msdn.microsoft.com/library/azure/dn906008.aspx)ja [multiclass](https://msdn.microsoft.com/library/azure/dn906015.aspx)), päätös jungles ([2-luokan](https://msdn.microsoft.com/library/azure/dn905976.aspx) ja [multiclass](https://msdn.microsoft.com/library/azure/dn905963.aspx)) ja tehosti päätöspuiden ([regression](https://msdn.microsoft.com/library/azure/dn905801.aspx) ja [2-luokan](https://msdn.microsoft.com/library/azure/dn906025.aspx)) perustuvat päätöspuiden, oppimisen käsite foundational kone. On monia päätöspuiden variantteja, mutta ne kaikki toimivat samalla tavoin — ominaisuus tilaa jakaa alueisiin on pääosin sama otsikko. Nämä voivat olla alueita yhtenäinen luokka tai vakioarvoa mukaan ovat tekemässä luokitus tai regression.

![Päätöksentekopuu jakaa ominaisuus tilaa][5]

***Päätöspuu jakaa ominaisuus tilaa suunnilleen yhdenmukaiset arvot alueisiin***

Toiminnon tila voidaan jakaa satunnaisesti pieniä alueita, koska on helppo kuvitella sen riittävän hienoksi jakaminen on yksi arvopiste alueittain – äärimmäinen Esimerkki overfitting. Välttämiseksi tämä suuri joukko puita on rakennettu erityisiä matemaattisia huolellisesti tehty, että puut ole vastaavuussuhteet. Tämä "päätöksessä toimialuepuuryhmässä" keskiarvo on puun, joka estää overfitting. Päätöksen metsien käyttää paljon muistia. Päätös jungles on muuttuja, joka kuluttaa vähemmän muistia kustannuksella koulutus hieman pidempi aika.

Tehosti päätöspuiden välttää rajoittamalla kuinka monta kertaa he voivat jakaa ja kuinka vähän arvopisteitä sallitaan kunkin alueen overfitting. Algoritmi muodostaa järjestyksen puita, joista jokainen oppii korvaamaan jättämän puun ennen virhe. Tulos on erittäin tarkka oppija, joka yleensä käyttää paljon muistia. Täydellinen tekninen kuvaus Tutustu [Alkuperäinen paperi Friedman's](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Nopea toimialuepuuryhmän kvantiili regressiota](https://msdn.microsoft.com/library/azure/dn913093.aspx) on tavallaan päätöspuiden erikoistapaus, jossa haluat tietää ei vain tavallinen (mediaani) arvon alueella, mutta sen jakautuminen quantiles muodossa olevia tietoja varten.

### <a name="neural-networks-and-perceptrons"></a>Neural networks ja perceptrons

Neural-verkot ovat aivot-tyylisellä Koulujen algoritmeja, joka kattaa [multiclass](https://msdn.microsoft.com/library/azure/dn906030.aspx), [2-luokan](https://msdn.microsoft.com/library/azure/dn905947.aspx)ja [Regressio](https://msdn.microsoft.com/library/azure/dn905924.aspx) -ongelmia. He tulevat äärettömän monia, mutta neural networks Azure Machine Learning sisällä ovat kaikki lomakkeessa ohjattu asykliset kaavioista. Tämä tarkoittaa sitä, että syöttötavan toiminnot välitetään eteenpäin (ei koskaan taaksepäin) järjestys kerrosten läpi ennen lähtöjen tiedot poistettu. Kunkin tason tuotantopanoksia on painotettu eri, lasketaan yhteen ja siirretään seuraavaan kerrokseen. Tämä yhdistelmä yksinkertaisia laskutoimituksia johtaa kyky oppia kehittyneitä luokan rajat ja tietojen trendit, näennäisesti magic mukaan. Tällaiset verkostojen monta kerrostetun suorittaa "syvä oppimista", polttoaineita paljon tech raportointi- ja science fiction.

Tämän korkean suorituskyvyn ei tule ilmaiseksi, vaikka. Neural networks kestää kauan kouluttaminen on erityisen suuria tietojoukkoja, jossa on paljon ominaisuuksia. Niillä on myös useita parametreja kuin useimmat algoritmit, mikä tarkoittaa, että parametrin sweeping laajentaa koulutusta aika paljon.
Ja ne overachievers, jotka haluavat [määrittää oman verkon rakenne](http://go.microsoft.com/fwlink/?LinkId=402867), mahdollisuudet ovat inexhaustible.

<a name="boundaries-learned-by-neural-networks6"></a>![Opitut asiat mukaan neural networks rajat][6]
---------------------------

***Opitut asiat neural networks mukaan rajat voivat olla hyvin monimutkaisia epäsäännöllinen***

[2-luokan keskiarvo perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) on skyrocketing koulutus kertaa vastaus neural networks. Se käyttää verkon rakenne, jonka avulla lineaarinen luokkien rajat. On lähes Alkukantainen tämän päivän standardien mukaan, mutta se on pitkä historia työskennellä robustly ja on tarpeeksi pieni, jotta saat nopeasti.

### <a name="svms"></a>SVMs

Tuki vector koneet (SVMs) Etsi rajan, joka erottaa mahdollisimman laaja marginaali kuin luokkien mukaan. Kun kahteen luokkaan ei voida selvästi erottaa, algoritmit löytää paras he voivat rajan. Kuten kirjoitettu Azure Machine Learning, [kaksi-luokan SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) tekee tämän vain suora viiva. (SVM puhua, se käyttää lineaarinen ytimen.) Koska se tekee lineaarisen lähentämiseen, se pystyy suorittamaan varsin nopeasti. Jos se todella shines on erottuva ominaisuus tietoja, kuten tekstiä tai genomien. Näissä tapauksissa SVMs on erottaa luokkien nopeammin ja vähemmän overfitting kuin useimmat muut algoritmeja, lisäksi vaaditaan vain vaatimaton määrä muistia.

![Tuki vector koneen luokan rajan][7]

***Tavalliset vector koneen luokan rajan suurentaa erottaminen kahteen reunus***

Tuotteen Microsoft Research, [kaksi-luokan paikallisesti syvä SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) on epälineaarisia, SVM, joka säilyttää suurimman osan lineaarinen versio nopeus ja muistin tehokkuutta. Se soveltuu erinomaisesti tapauksissa, joissa lineaarista lähestymistapaa ei tuota tarpeeksi tarkkoja vastauksia. Kehittäjät pidetään nopeasti hajottamalla ongelman huomioon useita pieniä lineaarinen SVM ongelmia. Lue lisätietoja siitä, miten ne vedetään pois tämä [täydellinen kuvaus](http://research.microsoft.com/um/people/manik/pubs/Jose13.pdf) .

Tiedostopäätteellä Epälineaaristen SVMs kekseliäs, [yksi-luokan SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) piirtää raja, joka esittelee tiiviisti koko joukon. Se on hyödyllinen oudosta tunnistamiseen. Uusi arvopisteitä, jotka eivät kuulu siltä osin, että rajan ovat epätavallisia ovat huomionarvoisia.

### <a name="bayesian-methods"></a>Bayesian menetelmät

Bayesian menetelmillä on erittäin toivottavaa laatua: he välttää overfitting. Ne tehdä tämän tekemällä joitakin oletuksia etukäteen todennäköinen leviäminen vastauksen. Toinen byproduct, tämä lähestymistapa on, että heillä on hyvin vähän parametreja. Azure Machine Learning on molemmat Bayesian algoritmeja sekä luokitukseen ([2-luokan Bayes kohta kone](https://msdn.microsoft.com/library/azure/dn905930.aspx)) ja regressio ([Lineaarinen regressio Bayesian](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Huomaa, että nämä oletetaan, että tiedot voidaan jakaa tai sovittaa suoran viivan kanssa.

Historiallinen muistiinpanon Bayes' kohta koneet kehitettiin Microsoft Research. Heillä on joitakin poikkeuksellisen kauniita teoreettisen työn taakse. Kiinnostunut opiskelija ohjataan [alkuperäisen artikkelin JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) ja [Kirsi Virtanen, kyetäkseen blogi](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Erikoistunut algoritmit

Jos sinulla on erityinen tavoite ehkä tykö. Azure-Machine Learning-kokoelmassa on algoritmeja, jotka ovat erikoistuneet arvovalon tekstinsyöttö ([järjestysnumero regression](https://msdn.microsoft.com/library/azure/dn906029.aspx)), määrä tekstinsyöttö ([Poisson-regressiota](https://msdn.microsoft.com/library/azure/dn905988.aspx)) ja oudosta detection (yksi [keskeisestä analyysi](https://msdn.microsoft.com/library/azure/dn913102.aspx) ja [tukee vector koneen](https://msdn.microsoft.com/library/azure/dn913103.aspx)s perustuu perusteella).
Eikä mitään parittoman klusteroinnin algoritmia myös ([K-keinoja](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/)).

![Oudosta KYS-pohjainen tunnistus][8]

***Oudosta KYS-pohjainen tunnistus*** *-Suurin osa tiedoista kuuluu stereotypical jakelu, hinnanvaihteluista huomattavasti siitä, että jakelu ovat Vaillinaiset pistettä*

![Tiedot on ryhmitelty K keinoilla][9]

***Sitä on ryhmitelty käyttäen K 5 klustereihin***

Myös ei ensemble [yksi v all - valitsin multiclass](https://msdn.microsoft.com/library/azure/dn905887.aspx), joka jakaa kaksi-luokan luokituksen ongelmia N-1 N-luokan luokittelun ongelma. Tarkkuutta, aikaa ja lineaarisuus ominaisuudet määräytyvät käytössä kaksi-luokan esittelyjä.

![2-luokan valitsimet yhdistellä 3-luokan valitsin][10]

***Muodostavat kolme-luokan valitsin yhdistää kaksi 2-luokan valitsimet***

Azure Machine Learning sisältyy myös pääsy tehokas kone oppimistyökalu [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf)otsikon alla.
VW defies luokittelu, sillä se oppii luokitus- ja regressio-ongelmia ja saat jopa osittain Nimetön tiedoista. Voit määrittää sen jollakin useita algoritmeja, tappio-funktioiden ja optimointi algoritmit. Se on suunniteltu maanpinnasta ylöspäin rinnakkain, tehokas ja erittäin nopea. Se käsittelee ridiculously suuri ominaisuus määrittää näennäisen pienellä vaivalla.
Aloitettiin ja johti mukaan Microsoft Researchin 's oma John Langford, VW on kaluston autojen algoritmeja kentän kaavan yksi merkintä. Sopii VW ei jokaista ongelmaa, mutta ei voi olla että aikaa climb oppimiskäyrä liittymässä sen arvoinen. Se on saatavana myös [erillisenä avoimen lähdekoodin](https://github.com/JohnLangford/vowpal_wabbit) useilla eri kielillä.


<!-- Media -->

[1]: ./media/machine-learning-algorithm-choice/image1.png
[2]: ./media/machine-learning-algorithm-choice/image2.png
[3]: ./media/machine-learning-algorithm-choice/image3.png
[4]: ./media/machine-learning-algorithm-choice/image4.png
[5]: ./media/machine-learning-algorithm-choice/image5.png
[6]: ./media/machine-learning-algorithm-choice/image6.png
[7]: ./media/machine-learning-algorithm-choice/image7.png
[8]: ./media/machine-learning-algorithm-choice/image8.png
[9]: ./media/machine-learning-algorithm-choice/image9.png
[10]: ./media/machine-learning-algorithm-choice/image10.png
