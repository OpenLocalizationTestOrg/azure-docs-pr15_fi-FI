<properties
    pageTitle="Yksinkertainen koe-Machine Learning Studio | Microsoftin Azure"
    description="Koneen oppimisen opetusohjelma opastaa tieteen kokeen tiedot helposti. Olemme arvioida regressio-algoritmin avulla auton hinnasta."
    keywords="Kokeile, lineaarinen regressio, machine learning algoritmeja, machine learning opetusohjelma, ehkäisevän mallinnus tekniikoita tietojen tiede-koe"
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
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="garye"/>

# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Machine learning opetusohjelma: luoda ensimmäisen tiedot-tiede-koe Azure Machine Learning Studio

Koneen oppimisen opetusohjelma opastaa tieteen kokeen tiedot helposti. Luodaan lineaarisen regression-malli, joka ennustaa hinnan perusteella eri muuttujia kuten korjaamollesi tehdä ja teknisissä eritelmissä. Tällöin käytetään Azure Machine Learning Studio kehittää ja käydä ehkäisevän yksinkertainen-analytics kokeilla.

*Ehkäisevän analytics* on eräänlainen tietojen tiedettä, jonka avulla nykyiset tiedot ennustaa tulevia tuloksia. Ehkäisevän analytics hyvin yksinkertainen esimerkki, katso tietoja tieteen aloittelijoille video 4: [vastaus on yksinkertainen malli ennustaa](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (runtime: 7:42).

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Miten Machine Learning Studio auttaa?

Machine Learning Studio on helppo määrittää käyttämällä vedä ja pudota-moduulit esiohjelmoituja ehkäisevän mallinnus tekniikoita ja koe. Suorittaa oman koe ja ennakoida vastauksen, käytetään Machine Learning Studio *mallin luominen*, *mallin junan*ja *pisteet ja Testaa malli*.

Anna Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net). Jos Machine Learning Studio, ennen kuin olet kirjautunut, valitse **Kirjaudu sisään tähän**. Muussa tapauksessa **Rekisteröidy** ja valita vaihtoehdoista vapaa ja maksettu.

Saat yleisiä tietoja Machine Learning Studio on [Mikä on Machine Learning Studio?](machine-learning-what-is-ml-studio.md)

## <a name="five-steps-to-create-an-experiment"></a>Viisi vaiheet luoda koe

Koulujen opetusohjelman tässä koneessa noudatat rakentaa kokeen luominen, kouluttaminen ja pisteytys malli Machine Learning Studio viiden perusvaiheen mukaisesti:

- Mallin luominen
    - [Vaihe 1: Nouda tiedot]
    - [Vaihe 2: Esikäsittele tiedot]
    - [Vaihe 3: Määritä ominaisuudet]
- Malli junan
    - [Vaihe 4: Ja soveltaa Koulujen algoritmi]
- Sija ja Testaa malli
    - [Vaihe 5: Uusien autojen hintojen ennustetaan]

[Vaihe 1: Nouda tiedot]: #step-1-get-data
[Vaihe 2: Esikäsittele tiedot]: #step-2-preprocess-data
[Vaihe 3: Määritä ominaisuudet]: #step-3-define-features
[Vaihe 4: Ja soveltaa Koulujen algoritmi]: #step-4-choose-and-apply-a-learning-algorithm
[Vaihe 5: Uusien autojen hintojen ennustetaan]: #step-5-predict-new-automobile-prices


## <a name="step-1-get-data"></a>Vaihe 1: Nouda tiedot

On mukana Machine Learning Studio, voit valita otoksen DataSet-ryhmien määrä, ja voit tuoda tietoja useista lähteistä. Tässä esimerkissä Käytämme mukana toimitettavan dataset- **Auto hintatiedot (Raw)**.
Tämän DataSet-ryhmän sisältää useita yksittäisiä autot, kuten valmistaja, malli, teknisten eritelmien ja hinta.

1. Käynnistää uusi koe napsauttamalla **+ Uusi** Machine Learning Studio-ikkunan alaosassa, valitse **KOKEILE**ja valitse **Tyhjä kokeilla**. Valitse alustan yläreunassa kokeen nimi ja nimeä se uudelleen jotain merkitystä, esimerkiksi **Auto hinta ennakoivat**.

2. Kangas on vasemmalla puolella kokeeseen valikoima DataSet-esiintymiä ja moduulit. Hakukenttään yläreunassa tähän valikoimaan etsimiseen DataSet-ryhmän tyyppi **Auto** nimeltään **Auto hintatiedot (Raw)**.

    ![Etsi valikoimasta][screen1a]

3. Vedä dataset kokeen alusta.

    ![DataSet-ryhmän][screen1]

Jos haluat nähdä nämä tiedot näyttävät, lähtöportti autoteollisuus DataSet-ryhmän alaosassa ja valitse sitten **Visualisoi**.

![Moduuli output-portti][screen1c]

Dataset-muuttujat näkyvät sarakkeina ja korjaamollesi esiintymä näkyy rivinä. Oikeassa sarakkeessa (sarake 26 ja nimetty "Hinta") on yrittää ennustaa tutustutaan target-muuttuja.

![DataSet-visualisointi][screen1b]

Sulje visualisoinnin ikkuna klikkaamalla "**x**" oikeassa yläkulmassa.

## <a name="step-2-preprocess-data"></a>Vaihe 2: Esikäsittele tiedot

DataSet-ryhmän yleensä vaatii esikäsittelyn, ennen kuin se voidaan analysoida. Olet ehkä huomannut puuttuvia arvoja ole sarakkeita eri riveillä. Nämä puuttuvat arvot täytyy puhdistaa niin malli voi analysoida tietoja oikein. Tässä tapauksessa olemme poistaa kaikki rivit, joilla on puuttuvia arvoja. **Normalisoitu tappiot** sarakkeen on myös suuri osuus puuttuvia arvoja, joten olemme jättää kyseisen sarakkeen mallin kokonaan.

> [AZURE.TIP] Useimpien moduulien edellytys on syöttötiedot puuttuvat arvot puhdistamista.

Microsoft ensin poistaa **normalisoida tappiot** sarakkeen ja sitten olemme poistaa kaikki puuttuvat tiedot sisältävä rivi.

1. Kirjoita hakukenttään yläreunassa [Valitse sarakkeet Dataset-] moduulin valikoimasta **valita sarakkeita** [ select-columns] moduuli ja sitten vetämällä sen kokeen Kuvapohjan ja kytke se **Auto hintatiedot (raaka)** dataset tulostusporttiin. Tämän moduulin avulla voimme valita, minkä sarakkeiden tiedot haluamme sisällyttää tai jättää pois mallissa.

2. Valitse [Valitse sarakkeet Dataset-] [ select-columns] moduuli ja valitse **Ominaisuudet** -ruudussa **Käynnistä sarakkeenvalitsinta** .

    - Vasemmalla Valitse **säännöt**
    - Valitse **Aloita kanssa**, **kaikki sarakkeet**. Tämä Ohjaa [Dataset-sarakkeiden valitseminen] [ select-columns] läpi kaikki sarakkeet (lukuun ottamatta olemme noin jättämään).
    - Avattavat Valitse **Jätä pois** ja **sarakkeiden nimet**ja valitse tekstikehyksen sisällä. Sarakkeiden luettelo tulee näkyviin. Valitse **normalisoida tappioita**ja se lisätään tekstikehykseen.
    - Sulje sarakkeenvalitsinta valintamerkki (OK)-painiketta.

    ![Sarakkeiden valitseminen][screen3]

    Ominaisuudet-ruudussa, **Valitse Dataset-** sarakkeille osoittaa, että läpäisee kaikki sarakkeet kautta paitsi **normalisoida tappiot**dataset.

    ![Valitse sarakkeet Dataset-ominaisuudet][screen4]

    > [AZURE.TIP] Voit lisätä kommentin moduulin moduuli kaksoisnapsauttamalla ja kirjoittamalla tekstin. Tämän ansiosta voit nähdä yhdellä silmäyksellä, mitä moduuli tekee oman kokeessa. Kaksoisnapsauta [Valitse sarakkeet Dataset-] tässä tapauksessa[ select-columns] moduuli ja Kirjoita kommentti "sulje pois normalisoida tappiot."

3. Vedä [Puhdas puuttuvien tietojen] [ clean-missing-data] moduulin kokeeseen Kuvapohjan ja [Dataset-sarakkeiden valitseminen] muodostaa[ select-columns] moduuli. Valitse **Ominaisuudet** -ruudussa, **poistaa koko rivin** **tilaa siivous** puhdistaa tiedot poistamalla rivit, joista puuttuu arvoja. Kaksoisnapsauta moduuli ja Kirjoita kommentti "Poista arvo puuttuu rivejä."

    ![Puhdas puuttuvien tietojen ominaisuudet][screen4a]

4. Kokeeseen suorittaa valitsemalla **Suorita** koe kangas.

Kokeeseen on valmis, kun kaikki moduulit on vihreä valintamerkki osoittamaan, että ne onnistui. Huomaa myös oikeassa yläkulmassa **Valmis käynnissä** -tila.

![Ensimmäinen koe suoritetaan][screen5]

Kaikki olemme ole tehnyt tähän mennessä koe on puhdistaa tiedot. Jos haluat tarkastella puhdistetut dataset-napsauttamalla vasemmalla lähtöportti [Puhdas puuttuvien tietojen] [ clean-missing-data] ("Cleaned DataSet-ryhmän")-moduulin ja valitsemalla **Visualisoi**. Huomaa, että **normalisoida tappiot** sarake ei enää sisälly ja puuttuvia arvoja ei ole.

Nyt kun tiedot on puhdas, sinut voidaan määrittää, mitä ominaisuuksia seuraavaksi Käytä ennakoivia mallin nyt.

## <a name="step-3-define-features"></a>Vaihe 3: Määritä ominaisuudet

Machine learning- *Ominaisuudet* ovat yksittäisiä mitattavia ominaisuuksia olet kiinnostunut jostakin. Kukin rivi edustaa yhden Auto meidän dataset- ja kussakin sarakkeessa on ominaisuus, että Auto.

Etsiminen hyvä joukko ominaisuuksia, joiden ehkäisevän malli edellyttää kokeet ja tietämys haluat ratkaista ongelman. Jotkin ominaisuudet ovat paremmin ennakoimaan kohde kuin toiset. Lisäksi jotkin ominaisuudet on vahva korrelaatio muita ominaisuuksia (esimerkiksi Kaupunki-mpg maantiellä-mpg verrattuna), joten ne eivät lisää mallin paljon uutta tietoa, ja ne voidaan poistaa.

Nyt rakentaa malli, joka käyttää meidän dataset-toimintojen alijoukon. Voit jatkaa ja valita eri ominaisuuksia, suorittaa kokeen uudelleen ja nähdä, jos voit saada parempia tuloksia. Mutta Ensiksikin kokeeksi seuraavat ominaisuudet:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Toiseen [Dataset-Valitse sarakkeet] vetämällä[ select-columns] moduulin kokeeseen Kuvapohjan ja muodostaa vasemmassa lähtöportti [Puhdas puuttuvien tietojen] [ clean-missing-data] moduuli. Kaksoisnapsauta moduuli ja kirjoita "Valitse ominaisuudet Ennakoinnille."

2. Valitse **Ominaisuudet** -ruudussa **Käynnistä sarakkeenvalitsinta** .

3. Valitse **sääntöjen kanssa**.

4. Kohdan **Kanssa alkavat** **sarakkeita**ja valitse suodatinrivi ja **Sisällytä** **sarakkeen nimi** . Anna meidän sarakenimien luettelo. Tämä ohjaa läpi vain ne sarakkeet, joka määritetään moduulin.

    > [AZURE.TIP] Suorittamalla kokeen, olemme olet varmistanut, että meidän tiedot sarakemääritykset kulkevat dataset- [Puhdas puuttuvien tietojen] [ clean-missing-data] moduuli. Tämä tarkoittaa yhteyden muiden moduulien on myös tietoja tietojen joukosta.

5. Valintamerkki (OK)-painiketta.

![Sarakkeiden valitseminen][screen6]

Tämä tuottaa tietojoukko, jota käytetään algoritmin Koulujen seuraavia vaiheita. Voit myöhemmin palata ja uudelleen useita eri ominaisuuksia.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Vaihe 4: Ja soveltaa Koulujen algoritmi

Nyt kun tiedot on valmis, haitalliseksi ehkäisevän malli koostuu koulutus ja testaus. Malli junan ja testaa sitten mallia miten lähellä on voitava ennakoida hintojen meidän tietojen avulla. Nyt älä huolehdi siitä, miksi meidän on opettaa ja testata sitten malli.

*Luokitus* ja *Regressio* on kaksi valvottua koneen oppimisen tekniikoita. Luokittelu ennustaa vastaukseksi määritetyn joukon luokkia, kuten väri (punainen, vihreä tai sininen). Arvioida kuinka käytetään regression.

Koska haluamme ennustaa hinnan, joka on luku, Käytämme regressiomallia. Tässä esimerkissä olemme opettaa Yksinkertainen *Lineaarinen regressio* -mallissa ja seuraavassa vaiheessa olemme testataan sitä.

1. Käytämme koulutukseen ja jakamalla se eri koulutus ja testaus joukot testaus meidän tietoja. Valitse ja vedä [Tiedoilla] [ split] moduulin kokeeseen Kuvapohjan ja muodostaa [Dataset-Valitse sarakkeet] viimeisen tulosteen[ select-columns] moduuli. **Murto-osa ensimmäinen tuloste tietojoukon rivien** arvoksi 0,75. Tällä tavoin olemme malli junan 75 prosenttia tietojen avulla, ja pidä takaisin 25 prosenttia testausta varten.

    > [AZURE.TIP] Muuttamalla **satunnainen siementen** parametri voidaan tuottaa eri pistokokeita koulutukseen ja testaamiseen. Tämä parametri määrittää Pseudo random number generator alustuksia.

2. Kokeen suorittaminen [Valitse sarakkeet Dataset-] näin[ select-columns] ja [Jaa tietoja] [ split] moduulit välittämään sarakemääritykset lisäämme seuraavaksi moduulit.  

3. Valitse Koulujen algoritmi, laajenna moduulin valikoimasta **Machine Learning** -luokan alustan vasemmalle ja laajenna sitten **Alusta-malli**. Tämä näyttää useita moduuleja, joiden avulla voidaan valmistella machine learning algoritmeja luokkia.

    Tämän kokeen, valitse [Muuttujan lineaarinen regressio] [ linear-regression] **Regressio** -luokassa (myös löydät moduuli kirjoittamalla hakuruutuun valikoima "lineaarinen regressio"), moduuli ja vedä se koe kangas.

4. Etsi ja vedä [Malli juna] [ train-model] moduulin koe kangas. [Lineaarisen Regression] tulosteen vasen tuloliitäntä muodostaa[ linear-regression] moduuli. Oikealla tuloliitäntä muodostaa koulutus tietoja tuotoksen (vasen portti) [Jaa tietoja] [ split] moduuli.

5. [Malli juna] [ train-model] moduuli, **käynnistää sarakkeenvalitsinta** **Ominaisuudet** -ruudussa, ja valitse sitten **Hinta** -sarakkeen. Tämä on arvo, joka meidän malli aiotaan ennustaa.

    ![Valitse sarake "Hinta"][screen7]

6. Kokeen suorittaminen

Tuloksena on koulutettuja regressiomallia, jota voidaan käyttää uusien näytteiden tehdä ennusteita asiakkaillenne. sija.

![Käyttäminen machine learning-algoritmi][screen8]

## <a name="step-5-predict-new-automobile-prices"></a>Vaihe 5: Uusien autojen hintojen ennustetaan

Nyt kun olemme olet harjoitettu malli, 75 prosenttia meidän tietojen avulla, voimme sen avulla sija 25 prosenttia tiedoista näet, miten hyvin malli meidän funktiot.

1. Etsi ja vedä [Tulos mallin] [ score-model] moduulin kokeeseen Kuvapohjan ja [Junan mallin] tulosteen vasen tuloliitäntä muodostaa[ train-model] moduuli. Oikealla tuloliitäntä muodostaa testin tiedot tuotoksen (oikea portti) [Jaa tietoja] [ split] moduuli.  

    ![Tulos mallin moduuli][screen8a]

2. Voit suorittaa kokeen ja tarkastella [Tulos mallin] tuotosta[ score-model] moduuli, valitse output-portti ja valitse sitten **Visualisoi**. Tuloste näyttää laskettuja arvoja, hinta ja tunnettuja arvoja koetus.  

3. Lopuksi voit testata tuloksia laadun, valitse ja vedä [Arvioida Model] [ evaluate-model] moduulin kokeeseen Kuvapohjan ja [Tulos mallin] tulosteen vasen tuloliitäntä muodostaa[ score-model] moduuli. (On kaksi tuloliitäntöihin, koska [Arvioi Model] [ evaluate-model] moduulin avulla voidaan verrata kahden malleja.)

4. Kokeen suorittaminen

Jos haluat tarkastella tulos [Mallin arvioiminen] [ evaluate-model] moduuli, valitse output-portti ja valitse sitten **Visualisoi**. Meidän malli esitetään seuraavat tilastot:

- **Absoluuttinen virhe tarkoittaa** (MAE): absoluuttinen virheitä ( *Virhe* on ennustetun arvon ja todellisen arvon välinen erotus) keskiarvo.
- **Root-keskiarvon neliön virhe** (RMSE): neliön virheitä on tehty testi dataset-laskentamenetelmää keskiarvon neliöjuuri.
- **Suhteellinen absoluuttinen virhe**: absoluuttinen virheitä suhteessa todellisten arvojen ja todellisten arvojen keskiarvon välinen absoluuttinen ero keskiarvosta.
- **Neliön suhteellinen virhe**: keskiarvon neliön virheitä suhteessa todellisten arvojen ja todellisten arvojen keskiarvon neliön erotus.
- **Määritettäessä kerroin**: tunnetaan myös nimellä **korrelaatiokerroin arvo**, tämä on tilastollinen metric-arvo, joka osoittaa, miten hyvin malli sopii tietoihin.

Kunkin virheen tilastot, pienempi on parempi. Pienempi arvo ilmaisee laskentamenetelmää tarkemmin vastaa todellisia arvoja. **Määritettäessä kerroin**, mitä lähempänä sen arvo on yksi (1.0), sitä paremmin laskentamenetelmää.

![Arvioinnin tulokset][screen9]

Lopullinen koe pitäisi näyttää seuraavalta:

![Machine learning opetusohjelma: suorittaa lineaarisen regression-kokeeseen, jota käytetään ennakoivia mallinnus tekniikoita.][screen10]

## <a name="next-steps"></a>Seuraavat vaiheet

Ensimmäinen kone-oppimisen opetusohjelma on valmis ja määrittää oman koe on, voit käydä yrittää parantaa malli. Voit esimerkiksi muuttaa ennakoivat, että käyttämäsi toiminnot. Tai voit muokata ominaisuuksia, [Lineaarinen regressio] [ linear-regression] algoritmia tai kokeile eri algoritmi kokonaan. Vaikka voit lisätä Koulujen että kokeen algoritmeja kerralla useita tietokoneen ja verrata kahta [Arvioida mallin] avulla[ evaluate-model] moduuli.

> [AZURE.TIP] Painikkeella **Tallenna kuin** koe kangas-kohdassa voit kopioida minkä tahansa iteraation että kokeen. Näet kaikki oman kokeen iteraatioiden valitsemalla sen **Näytä Suorita HISTORIA** . Katso [Manage kokeilla toistoja Azure Machine Learning Studio] [ runhistory] lisätietoja.

[runhistory]: machine-learning-manage-experiment-iterations.md

Kun olet tyytyväinen malliin, voit ottaa WWW-palvelulle, jota käytetään autojen hintojen ennustetaan käyttämällä uusia tietoja. [Azure-Machine Learning-web-palvelun käyttöönotto] on[ publish] lisätietoja.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Saat tietoja entistä laaja ja yksityiskohtainen ehkäisevän mallinnus tekniikoita luomiseen, koulutusta, pisteytys ja käyttöönoton malli, katso [kehittäminen ehkäisevän ratkaisun avulla Azure Machine Learning][walkthrough].

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
