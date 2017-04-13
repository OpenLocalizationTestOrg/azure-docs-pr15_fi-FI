<properties
    pageTitle="Ominaisuus tekniikka ja Azure koneen Learning valinnan | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan ominaisuudet ja ominaisuus tekniikka ja on esimerkkejä tietojen laajentamisesta prosessin koneen opiskelun rooli."
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
    ms.date="09/12/2016"
    ms.author="zhangya;bradsev" />


# <a name="feature-engineering-and-selection-in-azure-machine-learning"></a>Ominaisuuden tekniikka ja Azure koneen Learning valinta

Tässä ohjeaiheessa kerrotaan ominaisuus tekniikka ja tietojen laajentamisesta prosessin koneen opiskelun ominaisuudet. Se on kuvattu nämä prosessit liittyä käyttämällä esimerkkejä Azure koneen Learning Studio myöntämä.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Koneen learning käytetyt koulutus tiedot voidaan parantaa usein valinnan tai ominaisuuksia purkaminen raaka kerättyjä tietoja. Esimerkki rakenne-toiminnon opettelemalla luokitella käsin merkkien kuvia kontekstissa on rakennettava raaka bittinen jakauman tiedoista bittinen tiheyden kartta. Muuntomallin auttaa Etsi merkit reunoja tehokkaammin raaka jakauman.

Rakenne-ja valitun tehokkuuden koulutusprosessi, jossa yrittää Poimi tiedot avaimen tietoja. Ne parantavat myös näiden mallien power luokitella syöttötiedot tarkasti ja halutut tulokset ennustetaan Lisää robustly. Jotta oppimista Lisää laskennallisesti tractable voit myös yhdistää ominaisuus tekniikka ja valinta. Se tekee parantaminen ja vähentää ominaisuuksien tarvitaan kalibroida tai kouluttaminen mallin. Matemaattisesti puhua valittuna mallia kouluttaminen ominaisuudet ovat vähimmäismäärää riippumattoman muuttujan, joka kerrotaan tietojen kuvioiden ja ennustaa tulosten onnistuneesti.

Tekniset Funktiot ja ominaisuudet valinta on yksi osa suurempi prosessi, joka sisältää tavallisesti neljä vaihetta:

* Tietojen kerääminen
* Tietojen laajentamisesta
* Mallin rakentaminen
* Portugalin käsittely

Tekniset Funktiot ja valinta muodostavat koneen opiskelun tietojen laajentamisesta-vaiheessa. Kolme ominaisuuksia tämä prosessi voidaan erottaa saat parhaiten tarkoituksiimme:

* **Vanhat tietojenkäsittely**: Tämä toimenpide yrittää kerättyjen tietojen ovat puhtaasti ja johdonmukaisia. Se sisältää tehtäviä, kuten integroinnissa useita arvojoukon, käsittely puuttuvat tiedot, käsittely ristiriitaisia tietoja ja tietotyyppien.
* **Tekniikka ominaisuus**: Tämä toimenpide yrittää asiaa lisäominaisuuksia luominen aiemmin raaka toimintoja tiedot ja oppiminen-algoritmin ennakoivan esitysmuotoa lisäämiseksi.
* **Ominaisuudet**: Tämä prosessi valitsee alkuperäisen tietoihin liittyviä toimintoja vähentää koulutus ongelman dimensionaalisuus avaimen osajoukko.

Tässä ohjeaiheessa käsitellään vain ominaisuuden tekniikka ja tietojen laajentamisesta prosessin ominaisuus valinnan ominaisuuksia. Saat lisätietoja tiedot valmiiksi käsittely vaiheessa [esikäsittely Azure koneen Learning Studiossa tiedot](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/).


## <a name="creating-features-from-your-data--feature-engineering"></a>Ominaisuuksien luominen tiedoista--ominaisuus tekniikka

Koulutus tiedot koostuu matriisin koostuva esimerkkejä (tietueet tai tallennettu riveillä huomioita), joista jokaisella on erilaisia ominaisuuksia (muuttujat tai kentät tallennetaan sarakkeet). Vapausasteiden tietojen kuviot odotetaan koe rakenne-parametrissa ominaisuuksia. Vaikka monet tietokentät voidaan suoraan sisällyttää käytettävä kouluttaminen mallin valittuun toiminnon määrittäminen, lisäominaisuuksia rakenne on usein rakennettava-raaka tiedot, joista parannettu koulutus-tietojoukon toimintoja.

Mitä ominaisuuksia luodaanko parantaa tiedoille, kun koulutus mallin? Rakenne ominaisuuksista, jotka parantavat koulutus on tietoja, eristää paremmin tietojen kuviot. Odotat lisätiedot, jotka eivät selvästi kirjataan uusia ominaisuuksia tai helposti näennäinen alkuperäisen tai aiemmin luotuun ominaisuusjoukko, mutta tämä toimenpide on jotain kuva. Ääni- ja tehokkaasti päätökset edellyttävät usein toimialueen joitakin osaamisalueet.

Kun Azure koneen Learning alkaen on helpointa vaikealta prosessia concretely käyttämällä koneen Learning Studiossa annettu. Kaksi esimerkkiä on esitetty seuraavassa:

* Valvonnan alaisena kokeessa, jossa tavoitearvojen kutsutaan regressio-Esimerkki ([tekstinsyöttö luvun polkupyörien vuokrat.](http://gallery.cortanaintelligence.com/Experiment/Regression-Demand-estimation-4))
* Käyttämällä [Ominaisuus hajautuksessa] tekstin louhintaa luokitus-Esimerkki[feature-hashing]

### <a name="example-1-adding-temporal-features-for-a-regression-model"></a>Esimerkki 1: Regression mallin ajallinen ominaisuuksien lisääminen ###

Osoittamaan miten selvittänyt regressio tehtävän ominaisuudet käyttää kokeen "Demand arviointia Bikes" Azure koneen Learning Studio. Tämän kokeen tarkoituksena on ennustaa bikes eli polkupyörien vuokrat tietyn kuukauden, päivän tai tunnin kuluessa määrän tarve. Tietojoukon **polkupyörien vuokrausta UCI tietojoukon** käytetään raaka syöttötiedot.

Seuraavan tietojoukon perustuu pääoman Bikeshare yritys, joka säilyttää Washington Ohjauskoneen Yhdysvalloissa polkupyörien vuokrausta verkon reaali tiedot. Tietojoukon edustaa polkupyörien vuokrat määrän 2011 2012 tunti päivän sisällä, ja se sisältää 17379 rivien ja sarakkeiden 17. Raaka ominaisuusjoukko sisältää sääolojen (lämpötila, kosteus, Tuulen keskinopeus) ja kirjoita päivän (juhla- tai viikonpäivä). Ennustaa kenttä on **cnt**, määrä, joka edustaa polkupyörien vuokrat määritetyn tunnin kuluessa ja joka alueet 1 977.

Muodostaa tehokkaita ominaisuuksia koulutus tietojen neljä regression mallit on suunniteltu saman algoritmin avulla, mutta neljä eri koulutus tietojoukot. Neljä tietojoukot edustavat saman raaka syötteen tietoja, mutta kasvava numerolla ominaisuuksien määrittäminen. Nämä ominaisuudet on ryhmitelty neljä luokkiin:

1. A = sää + vapaapäivä viikonpäivä + viikonloppu budjetoidut päivän ominaisuudet
2. B = bikes, jotka olivat muuttuja kussakin edellisen 12 tuntia määrä
3. C = bikes, jotka olivat muuttuja kussakin sama tunnin edellisen 12 päivää määrä
4. D = bikes, jotka olivat muuttuja kussakin samaan tunti ja samana 12 edellisen viikon aikana määrä

Ominaisuuden määrittäminen A, joka on jo raaka alkuperäiset tiedot, lisäksi muita ominaisuuksia kolme joukkoja luodaan tekniikka-prosessi-ominaisuuden avulla. Toiminnon määrittäminen B ottaa bikes viimeisimmät tarve. Toiminto määrittää C kaappaa bikes tarve tietyn tunnissa. Ominaisuusjoukko D ottaa tarve bikes tietyn tunti ja viikon tiettynä päivänä. Kukin neljä koulutus tietojoukot sisältää toiminnon joukot A, A + B, A + B + C ja A + B + C + D-tarpeen mukaan.

Azure koneen Learning, kokeile näitä neljää koulutus tietojoukot on muodostettu neljä haaroja valmiiksi käsitellyt syötteen tietojoukon kautta. Lukuun ottamatta vasemmanpuoleisin haaran kunkin nämä haaran sisältää [R-komentosarjan suorittaminen] [ execute-r-script] moduulia, johon joukko johdetun ominaisuuksia (ominaisuus asettaa B, C ja D) on rakennettava ja tuodun tietojoukon liitetään tarpeen mukaan. Seuraavassa kuvassa näytetään, R-komentosarjan avulla luodaan ominaisuusjoukko B toisen vasemman haaran.

![Ominaisuuden joukon luominen](./media/machine-learning-feature-selection-and-engineering/addFeature-Rscripts.png)

Seuraavassa taulukossa on yhteenveto neljä mallien suorituskyvyn tulosten vertailu. Parhaat tulokset näkyvät hallintatoimintojen A + B + C. Huomaa, että virhe korko suurenee, kun lisäominaisuus joukot sisältyvät koulutus tiedot. Tämä tarkistaa tämän ominaisuuden joukot B ja C antaa Lisätietoja regression tehtävän oletusta. Lisäämällä D ominaisuusjoukko ei vaikuta antamaan virhe kaikki muut alennus.

![Vertaa suoritustulokset](./media/machine-learning-feature-selection-and-engineering/result1.png)

### <a name="example2"></a>Esimerkki 2: Luominen tekstin kaivos ominaisuudet  

Ominaisuuden tekniikka käytetään laajasti tekstin kaivos, kuten asiakirjan luokitus- ja markkinatunnelma analyysi liittyviä tehtäviä. Esimerkiksi kun haluat luokitella asiakirjoja useisiin luokkiin, tyypillinen olettaen on sanat tai ilmaisut sisällyttää asiakirjan-luokassa on epätodennäköistä esiintyvän toiseen asiakirjan luokkaan. Toisin sanoen sana tai lause jakauman taajuus ei voi enää vapausasteiden toiseen tiedostoluokat. Tekstin kaivos sovellusten suunnittelu prosessin ominaisuus tarvitaan henkilöihin liittyvät sana tai lause tiheydet, koska yksittäiset osat sisällön tekstissä käytetään yleensä syöttötiedot toimintoja.

Tavoitteet tämä tehtävä-tekniikkaa, jota kutsutaan *ominaisuus hajautuksessa* otetaan käyttöön haluamaansa tekstin muotoiluominaisuuksien muuttuvat indeksien tehokkaasti. Sen sijaan, että liitetään tekstiä ominaisuuksista (sanoja tai lauseita) tietyn indeksin, tämä menetelmä Funktiot lisäämällä hash-toiminnon ominaisuudet ja käyttämällä hash-arvojen indeksien suoraan.

Azure koneen Learning on [Ominaisuus hajautuksessa] [ feature-hashing] moduuli, jolla Luo sana tai lause ominaisuuksista. Seuraavassa kuvassa on esimerkki tämän moduulin. Kirjoita tietojoukon on kaksi saraketta: kirjan luokitus väliltä 1-5 ja todellisen sisällön tarkastelu. Tämä [Ominaisuus hajautuksessa] tavoitteena[ feature-hashing] moduuli on uusia ominaisuuksia, jotka vastaavat sanat tai ilmaisut sisällä tiettyä kirjan Tarkista esiintymistaajuus Näytä hakemiseen. Voit käyttää tätä moduulia, on tehtävä seuraavat toimet:

1. Valitse sarake, joka sisältää Lisää teksti (**Sarake2** tässä esimerkissä).
2. Määritä *Hashing bitsize* 8, mikä tarkoittaa, että 2 ^ 8 = 256 ominaisuudet luodaan. Sanan tai lauseen teksti sitten hajauttaa 256 kohtia. Parametrin *Hashing bitsize* alueita väliltä 1 – 31. Jos parametri määritetään on useita, sanoja tai lauseita on epätodennäköistä hajautusalgoritmilla kyselyjä samassa hakemistossa.
3. *N g* -parametrin arvoksi 2. Unigrams (jokaisen yksittäisen sanan ominaisuuden) ja bigrams (tuloista jokaisen vierekkäisten sanojen ominaisuus) esiintymistaajuus hakee Lisää teksti. Parametrin *N g* alueita 0 10, joka ilmaisee peräkkäisiä sanat ominaisuus sisällytettävät enimmäismäärä.  

![Ominaisuuden hajautusfunktiota moduuli](./media/machine-learning-feature-selection-and-engineering/feature-Hashing1.png)

Seuraavassa kuvassa on seuraavia uusia ominaisuuksia millaiseksi.

![Ominaisuuden hajautusfunktiota Esimerkki](./media/machine-learning-feature-selection-and-engineering/feature-Hashing2.png)

## <a name="filtering-features-from-your-data--feature-selection"></a>Suodatusominaisuudet tiedoista--ominaisuudet  ##

*Ominaisuudet* on prosessi, jota käytetään usein koulutus tietojoukot ennakoivan mallinnus tehtävien luokittelu tai regression tehtäviä, kuten rakentaminen. Tavoitteena on osajoukkoa ominaisuuksia alkuperäisen tietojoukon, joka mittasuhteet pienentää vähimmäismäärää ominaisuuksien avulla edustavan varianssin tietojen enimmäismäärän. Tämän osan ominaisuuksia on vain ominaisuudet tulevat kouluttaminen malli. Ominaisuudet on kaksi tärkeimmät tarkoituksiin:

* Ominaisuudet kasvaa usein poistamalla ole merkitystä ja tarpeettomat luokitus tarkkuutta tai Korreloidun erittäin ominaisuuksia.
* Ominaisuudet vähenee ominaisuuksista, määrän, joka tekee mallin koulutus-prosessin tehokkaamman. Tämä on erityisen tärkeää oppijat, jotka ovat esimerkiksi tuki vector koneet kouluttaminen.

Vaikka ominaisuudet pyritään Vähennä tietojoukon käytettävä kouluttaminen mallin ominaisuuksista, sitä ei yleensä viitataan termi *dimensionaalisuus vähentäminen.* Ominaisuuden valinnan menetelmät Pura alkuperäisen ominaisuuksia tietojen alijoukkoa muuttamatta niitä.  Dimensionaalisuus vähentäminen menetelmät lomakkeen rakenne ominaisuudet, joita voit Muunna alkuperäisen ominaisuudet ja muokkaa niitä. Dimensionaalisuus vähentäminen menetelmiä esimerkiksi lainan osa analyysi, canonical korrelaatio analysointia ja yksittäinen arvo Hajotus.

Yksi yleisesti käytetty luokka ominaisuus valinnan tavoista valvonnan alaisena kontekstissa on suodatin-pohjainen ominaisuudet. Arvioidaan kullekin ominaisuudelle ja kohdemääritettä korrelaatio näistä tavoista Käytä pistemäärän liittäminen kullekin ominaisuudelle tilastollinen mitta. Ominaisuuksien luokittelu sitten pisteet, jonka avulla voit määrittää pitäminen tai poistamisesta tietyn ominaisuuden raja-arvon mukaan. Esimerkkejä näistä tavoista käyttää kokonaisarvoja ovat Pearsonin tulomomenttikorrelaatiokertoimen keskinäinen ja chi-neliön testi.

Azure koneen Learning Studio tarjoaa moduulit ominaisuudet. Seuraavassa kuvassa esitetyllä näitä moduuleja ovat [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] ja [Fisher lineaarinen Discriminant analyysi][fisher-linear-discriminant-analysis].

![Ominaisuuden valinnan Esimerkki](./media/machine-learning-feature-selection-and-engineering/feature-Selection.png)


Käytä esimerkiksi [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] moduulin aiemmin jäsennetty teksti kaivos esimerkkiä. Oletetaan, että haluat regressiosuoran mallin luominen – [Ominaisuus hajautuksessa] 256 ominaisuusjoukko luomisen jälkeen[ feature-hashing] moduulin ja että vastauksen muuttuja on **Sar1** ja kirjan Tarkista luokitus väliltä 1-5. Määritä **ominaisuus näkyvissä pistemäärä menetelmä** **Pearsonin tulomomenttikorrelaatiokertoimen**, **Sar1** **kohdesarakkeessa** ja **50** **haluamasi ominaisuudet** . Moduulin [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] tuottaa tietojoukon sisältävä 50 ominaisuudet ja kohdemääritteen **Sar1**. Seuraavassa kuvassa on tämän kokeen ja syöttöparametrien kulun.

![Ominaisuuden valinnan Esimerkki](./media/machine-learning-feature-selection-and-engineering/feature-Selection1.png)

Seuraavassa kuvassa on syntynyt tietojoukot. Kullekin ominaisuudelle on tulos, Pearson korrelaatio itse ja kohdemääritettä **Sar1**perusteella. Ominaisuudet, joita yläreunan tulosten säilytetään.

![Suodatin-ominaisuutta valinnan tietojoukot](./media/machine-learning-feature-selection-and-engineering/feature-Selection2.png)

Seuraavassa kuvassa näkyy valittujen ominaisuuksien vastaavan tulosten.

![Valitun toiminnon tulosten](./media/machine-learning-feature-selection-and-engineering/feature-Selection3.png)

Käyttämällä [Suodatin-pohjainen ominaisuudet] [ filter-based-feature-selection] moduulissa 50 poissa 256 ominaisuudet on valittu, koska ne on eniten kanssa muuttujan **Sar1** perusteella tulosmalli menetelmä **Pearsonin tulomomenttikorrelaatiokertoimen**kohteen ominaisuudet.

## <a name="conclusion"></a>Tekemistä
Ominaisuuden tekniset Funktiot ja ominaisuudet on kaksi vaihetta yleisimpiin koulutus-tietojen valmisteleminen koneen learning mallin luotaessa. Tavallisesti ominaisuus tekniikka käytetään ensin luomiseen lisäominaisuudet ja suorittaa toiminnon valinta vaiheessa poistamiseksi ole merkitystä, ylimääräisiä tai erittäin Korreloidun ominaisuuksia.

Aina ei ole välttämättä suorittamiseen tekniikka tai ominaisuus ominaisuudet. Onko se tarvitaan määräytyy on tai kerätä tietoja, voit valita algoritmin ja kokeilla tavoitteena.


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[fisher-linear-discriminant-analysis]: https://msdn.microsoft.com/library/azure/dcaab0b2-59ca-4bec-bb66-79fd23540080/
