<properties
    pageTitle="Tekniikka Cortana Analytics prosessin ominaisuus | Microsoft Azure" 
    description="Ominaisuus tekniikka tarkoitetaan kerrotaan ja on esimerkkejä sen koneen opiskelun tietojen laajentamisesta-prosessin rooli."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-in-the-cortana-analytics-process"></a>Tekniikka Cortana Analytics-prosessin ominaisuus 

Tässä aiheessa kerrotaan ominaisuus tekniikka tarkoitetaan ja on esimerkkejä sen koneen opiskelun tietojen laajentamisesta-prosessin rooli. Tätä prosessia kuvataan esimerkkejä piirretään Azure koneen Learning Studio. 

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Tässä **valikossa** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit luoda tietojen ominaisuudet eri ympäristöissä. Tämä onkin vaiheessa [Ryhmän tietojen tiede prosessi (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

Ominaisuuden suunnitteluryhmät yritykset niin, että ennakoivan potenssiin Koulujen algoritmit luomalla ominaisuuksien tietoja, jotka auttavat helpottamiseksi learning prosessi. Tekniset Funktiot ja ominaisuudet valinta on yksi osa kuvatut TDSP [ryhmän tietojen tiede prosessin ominaisuudet?](data-science-process-overview.md) Ominaisuuden tekniikka ja valinnan ovat osat TDSP **kehittäminen ominaisuudet** -vaihe. 

* **ominaisuuden tekniikka**: Tämä toimenpide yrittää asiaa lisäominaisuuksia luominen aiemmin raaka toimintoja tiedot ja lisäämiseksi learning algoritmin ennakoivan potenssiin.

* **Ominaisuudet**: Tämä prosessi valitsee alkuperäisen tietoihin liittyviä toimintoja avaimen alijoukko yrittää pienentää koulutus ongelman dimensionaalisuus.

Tavallisesti **ominaisuus tekniikka** käytetään ensin Luo lisäominaisuuksia ja valitse **Ominaisuudet** -vaihe suoritetaan taustaäänten ole merkitystä, ylimääräisiä tai erittäin Korreloidun ominaisuuksia.

Koneen learning käytetyt koulutus tiedot voidaan parantaa usein ominaisuuksia purkaminen raaka kerättyjen tietojen mukaan. Esimerkki rakenne-toiminnon opettelemalla luokitella käsin merkkien kuvia kontekstissa on hieman tiheyden kartan rakennettava raaka bittinen jakauman tiedoista luomista. Etsi merkit reunoja avulla yksinkertaisesti raaka jakauman suoraan muuntomallin avulla.


[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## <a name="creating-features-from-your-data---feature-engineering"></a>Ominaisuuksien luominen tiedoista - ominaisuus tekniikka

Koulutus tiedot koostuu matriisin koostuva esimerkkejä (tietueet tai tallennettu riveillä huomioita), joista jokaisella on erilaisia ominaisuuksia (muuttujat tai kentät tallennetaan sarakkeet). Vapausasteiden tietojen kuviot odotetaan koe rakenne-parametrissa ominaisuuksia. Vaikka monet tietokentät voidaan suoraan sisällyttää käytettävä kouluttaminen mallin valittuun toiminnon määrittäminen, on usein että (rakenne) ominaisuuksia on rakennettava raaka tiedot, joista parannettu koulutus-tietojoukko toimintoja-tapaus.

Mitä ominaisuuksia luodaanko parantaa tietojoukon, kun koulutus mallin? Rakenne ominaisuuksista, jotka parantavat koulutus on tietoja, eristää paremmin tietojen kuviot. Odotamme lisätiedot, joka ei ole otettu selkeästi tai helposti näennäinen alkuperäisen tai aiemmin luotuun ominaisuusjoukko: n uusiin ominaisuuksiin. Mutta prosessia ei kuva. Ääni- ja tehokkaasti päätökset edellyttävät usein toimialueen joitakin osaamisalueet.

Kun Azure koneen Learning alkaen on helpointa vaikealta käyttämällä concretely annettu Studiossa näytteiden prosessia. Kaksi esimerkkiä on esitetty seuraavassa:

* Valvonnan alaisena kokeessa, jossa tavoitearvojen kutsutaan regression-esimerkki [tekstinsyöttö luvun polkupyörien vuokrat.](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4)
* Tekstin kaivos luokitus esimerkki käyttää [Ominaisuutta hajautuksessa](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/)

### <a name="example-1-adding-temporal-features-for-regression-model"></a>Esimerkki 1: Regression mallin ajallinen ominaisuuksien lisääminen ###

Käyttää kokeen "Demand arviointia Bikes" Azure koneen Learning Studio miten selvittänyt regression tehtävän ominaisuudet. Tämän kokeen tarkoituksena on ennustaa bikes eli polkupyörien vuokrat määritetyn kuukausi/päivä/tunnin kuluessa määrän tarve. Tietojoukon "polkupyörien vuokrausta UCI tietojoukko" käytetään raaka syöttötiedot. Tämä tietojoukko perustuu pääoman Bikeshare yritys, joka säilyttää Washington Ohjauskoneen Yhdysvalloissa polkupyörien vuokrausta verkon reaali tiedot. Tietojoukon edustaa polkupyörien vuokrat tunti vuoden 2011: n ja vuoden 2012 päivän sisällä, ja se sisältää 17379 rivien ja sarakkeiden 17. Raaka ominaisuusjoukko sisältää sääolojen (lämpötila ja kosteus/tuulen nopeus) ja kirjoita päivän (vapaapäivä/viikonpäivä). Ennustaa kenttä on "cnt", määrän, joka edustaa polkupyörien vuokrat määritetyn tunnin kuluessa ja alueet, jotka arvoalueita 977 1.

Kun tavoitteena on tehokas ominaisuudet koulutus tiedot neljä regression mallit on suunniteltu saman algoritmin avulla, mutta on neljä eri koulutus tietojoukkoja luomisesta. Neljä tietojoukkoja edustavat saman raaka syötteen tietoja, mutta kasvava numerolla ominaisuuksien määrittäminen. Nämä ominaisuudet on ryhmitelty neljä luokkiin:

1. A = sää + vapaapäivä viikonpäivä + viikonloppu budjetoidut päivän ominaisuudet
2. B = bikes, jotka olivat muuttuja kussakin edellisen 12 tuntia määrä
3. C = bikes, jotka olivat muuttuja kussakin sama tunnin edellisen 12 päivää määrä
4. D = bikes, jotka olivat muuttuja kussakin samaan tunti ja samana 12 edellisen viikon aikana määrä

Ominaisuuden määrittäminen A, joka on jo olevia alkuperäisiä raaka tietoja, lisäksi muita ominaisuuksia kolme joukkoja luodaan tekniikka-prosessi-ominaisuuden avulla. Toiminnon määrittäminen B kaappaa hyvin viimeisimmät tarve polkupyörät. Toiminto määrittää C kaappaa bikes tarve tietyn tunnissa. Ominaisuusjoukko D ottaa tarve bikes tietyn tunti ja viikon tiettynä päivänä. Neljä koulutus tietojoukkoja kunkin sisältää ominaisuuden määrittäminen A, A + B, A + B + C ja A + B + C + D.

Azure koneen Learning, kokeile näitä neljää koulutus tietojoukkoja on muodostettu neljä haaroja valmiiksi käsitellyt syötteen dataset-kautta. Useimmat haaran, nämä haaran sisältää vasemmalle lukuun ottamatta [suoritus R komentosarja](https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/) -moduulissa, jossa joukko johdetun ominaisuuksia (ominaisuusjoukko B, C ja D) ovat tarpeen mukaan rakennettava ja tuodun tietojoukon liitetään. Seuraavassa kuvassa näytetään, R-komentosarjan avulla luodaan ominaisuusjoukko B toisen vasemman haaran.

![Luo ominaisuudet](./media/machine-learning-data-science-create-features/addFeature-Rscripts.png)

Neljä mallien suorituskyvyn tulosten vertailu on koottu seuraavaan taulukkoon. Parhaat tulokset näkyvät hallintatoimintojen A + B + C. Huomautus virhe korko pienenee milloin koulutus tiedot sisältyvät muut ominaisuudet. Microsoftin oletetaan, että ominaisuusjoukko B, C on Lisätietoja regression tehtävän on vahvistettu. Mutta lisääminen D-ominaisuus ei vaikuta antamaan virhe kaikki muut alennus.

![vertailun tulokset](./media/machine-learning-data-science-create-features/result1.png)

### <a name="example2"></a>Esimerkki 2: Luominen tekstin kaivos ominaisuudet  

Ominaisuuden tekniikka käytetään laajasti tekstin kaivos, kuten asiakirjan luokitus- ja markkinatunnelma analyysi liittyviä tehtäviä. Esimerkiksi kun haluamme luokitella asiakirjoja useisiin luokkiin, tyypilliset olettaen on, että sisällyttää doc-luokassa word/lausekkeet ovat epätodennäköistä esiintyvän toiseen doc-luokkaan. Toisen sanoja sanoja tai lauseita jakauman taajuus ei voi enää vapausasteiden toiseen tiedostoluokat. Tekstin kaivos sovelluksissa, koska yksittäiset osat sisällön tekstissä käytetään yleensä kenttään annettavat tiedot suunnittelu prosessin ominaisuus tarvitaan toimintoja, joissa sanan tai lauseen tiheydet.

Tavoitteet tämä tehtävä-tekniikkaa, jota kutsutaan **ominaisuus hajautuksessa** otetaan käyttöön haluamaansa tekstin muotoiluominaisuuksien muuttuvat indeksien tehokkaasti. Sen sijaan, että liittämisestä tekstin kullekin ominaisuudelle (sanoja tai lauseita) tietyn indeksin hash-funktioon soveltaminen ominaisuuksia ja hash-arvojen käyttäminen indeksien suoraan Tämä menetelmä-funktiot.

Azure koneen Learning on [Ominaisuus hajautuksessa](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) moduuli, joka luo nämä word tai lauseen ominaisuudet helposti. Seuraavassa kuvassa on esimerkki tämän moduulin. Syötteen tietojoukko on kaksi saraketta: kirjan luokitus väliltä 1-5, ja todellinen Tarkista sisältö. Tämä [Ominaisuus hajautuksessa](https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/) moduuli tavoitteena on hakemiseen useita uusia ominaisuuksia, joissa näkyvät vastaavan sana esiintymistaajuus / vaaralauseke (vaaralausekkeet) tietyn kirjan Tarkista. Voit käyttää tätä moduulia, annettava seuraavien vaiheiden mukaisesti:

* Valitse sarake, joka sisältää Lisää teksti ("Sarake2" tässä esimerkissä).
* Seuraavaksi arvoksi "Hashing bitsize" 8, mikä tarkoittaa 2 ^ 8 = 256 ominaisuudet luodaan. Wordin/vaiheen tekstistä hajauttaa 256 kohtia. Parametrin "Hashing bitsize" alueita väliltä 1 – 31. Sana tai sanat / vaaralauseke (vaaralausekkeet) on epätodennäköistä hajautusalgoritmilla sama indeksi kyselyjä, jos se on suurempi arvo.
* Kolmas parametrin arvoksi "G N" 2. Unigrams (jokaisen yksittäisen sanan ominaisuuden) ja bigrams (tuloista jokaisen vierekkäisten sanojen ominaisuus) esiintymistaajuus hakee Lisää teksti. Parametrin "G N" alueita 0 10, joka ilmaisee peräkkäisiä sanat ominaisuus sisällytettävät enimmäismäärä.  

!["Ominaisuus Hashing-moduuli](./media/machine-learning-data-science-create-features/feature-Hashing1.png)

Seuraavassa kuvassa näkyy, mitä seuraavista uusi ominaisuus ulkoasun kuten.

!["Ominaisuus Hashing-Esimerkki](./media/machine-learning-data-science-create-features/feature-Hashing2.png)


## <a name="conclusion"></a>Tekemistä

Rakenne-ja valitun tehokkuuden koulutus käsittelyn, jota yrittää Poimi tiedot avaimen tietoja. Ne parantavat myös näiden mallien power luokitella syöttötiedot tarkasti ja halutut tulokset ennustetaan Lisää robustly. Jotta oppimista Lisää laskennallisesti tractable voit myös yhdistää ominaisuus tekniikka ja valinta. Se tekee parantaminen ja vähentää ominaisuuksien tarvitaan kalibroida tai kouluttaminen mallin. Matemaattisesti puhua valittuna mallia kouluttaminen ominaisuudet ovat vähimmäismäärää riippumattoman muuttujan, joka kerrotaan tietojen kuvioiden ja ennustaa tulosten onnistuneesti.

Huomaa, että aina ei ole välttämättä suorittamiseen tekniikka tai ominaisuus ominaisuudet. Onko tarvitaan vai ei määräytyy emme ole tai kerätä tietoja, emme Valitse algoritmin ja kokeilla tavoitteena.
 
