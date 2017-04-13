<properties
   pageTitle="Ennustaa yksinkertainen malli - regression mallin vastaus | Microsoft Azure"
   description="Miten voit luoda regressio mallin ennustetaan tietoja tiede aloittelijoille video 4 hinta. Sisältää lineaarisen regressiosuoran kohde tiedoilla."                                  
   keywords="luoda mallin, yksinkertainen malli, hinta tekstinsyöttö, regressio malli"
   services="machine-learning"
   documentationCenter="na"
   authors="cjgronlund"
   manager="jhubbard"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="cgronlun;garye"/>

# <a name="predict-an-answer-with-a-simple-model"></a>Ennustaa vastaus yksinkertainen malli

## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Tietojen tiede aloittelijoille sarjan

Opettele luomaan regressio mallin ennustetaan hinnan vinoneliö-tietojen tiede aloittelijoille video 4. Olemme Piirrä regressio malli, joka sisältää kohteen tietoja.

Pääset tehokkaasti sarjan katsoa ne kaikki. [Siirry videoiden luettelo](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-predict-an-answer-with-a-simple-model]

## <a name="other-videos-in-this-series"></a>Muita videoita sarjassa

*Tietoja tiede aloittelijoille* on viisi lyhyiden videoiden tiede tietojen lyhyt esittely.

  * Video 1: [5 kysymyksiä tietojen tiede vastauksia](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 minuuttia 14 sekuntia)*
  * Video 2: [tiedoissa on valmiina tietojen tiede?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 minuuttia 56 sekuntia)*
  * Video 3: [Kirjoita kysymys voi vastata tiedoilla](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 s)*
  * Video 4: Ennustaa vastaus yksinkertainen malli
  * Video 5: [Muiden käyttäjien työn alla tietojen tiede kopioiminen](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 minuuttia 18 sekuntia)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Tallenne: Ennustaa vastaus yksinkertainen malli

Tervetuloa-"tietojen tiede varten aloittelijoille" neljäs videoon sarja. Tämä vaihtoehto on yksinkertainen mallin luominen ja tee tekstinsyöttö.

*Malli* on yksinkertaistettu Tarinan Microsoftin tiedoista. Voin esitellään voin tarkoittaa.

## <a name="collect-relevant-accurate-connected-enough-data"></a>Kerää asiaa, tarkkoja, yhdistetty, riittävästi tietoja

Entä jos haluat ostaa vinoneliön varten. Minulla on soi, kuulunut Omat NO@LOC 1.35 carat vinoneliö asettamisesta kanssa ja haluan nyt saat käsityksen siitä, miten paljon se maksaa. Korujen säilöön kestää Muistio ja kynä, ja voin muistiin kaikki ruudut tapaukset, ja kuinka paljon ne arvioida carats hinta. Alkavat ensimmäisen vinoneliö - sen 1.01 carats ja $7,366.

Nyt voin käydä läpi ja toimi samoin säilön muut ruudut.

![Vinoneliö tietosaraketta](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Huomaa, että luettelossamme on kaksi saraketta. Kunkin sarakkeen on eri määrite - carats ja hinta - ja kullakin rivillä on yhteen arvopisteeseen, joka edustaa yhden vinoneliön.

Olet itse asiassa luomaasi pieni tietojen Määritä tähän - taulukkoon. Huomaa, että se täyttää laatu Microsoftin ehdot:

* Tiedot ovat **asiaa** - paino ehdottomasti liittyy hinta
* On **tarkkoja** - olemme double-checked hintoja, jotka on muistiin
* Se on **yhdistetty** - osoitteessa ei ole tyhjä välilyöntejä joko näiden sarakkeiden
* Ja tarkastellaan, se on **riittävän** tietoja, tutustu kysymykseen

## <a name="ask-a-sharp-question"></a>On terävä kysymys

Nyt on aiheuttaa Microsoftin kysymys terävä tavalla: "kuinka paljon se maksaa ostaminen 1.35 carat vinoneliön?"

Luettelossamme ei ole 1.35 carat vinoneliön uudelleen, niin olemme on käyttäminen tietojamme muiden vastaus kysymykseen.

## <a name="plot-the-existing-data"></a>Piirrä olemassa olevia tietoja

Huomaa, että Tee ei piirtää numero vaakaviivan, kutsutaan akselin, paksuuksia kaaviossa. Paksuuksia alue on 0, 2, joten viivan, joka kattaa, alue ja siirtää kunkin puolet carat jakoviivat piirtäminen.

Seuraavaksi on Piirrä pystyakseli hinnan ja yhdistä se vaaka leveys-akselin. Tämä on dollareina mittayksikkö. Nyt on joukko coordinate akselit.

![Paino ja hinta-akselit](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Tutustutaan ottaa tiedoista nyt ja *piirtää piste*muuttuvat. Tämä on erinomainen tapa visualisointi numeerisia tietojoukot.

Ensimmäisen arvopisteen olemme eyeball osoitteessa 1.01 carats pystysuoran viivan. Tämän jälkeen on eyeball vaakaviivalla $7,366-palvelussa. Jos ne vastaavat olemme Piirrä piste. Tämä on Microsoftin ensimmäisen vinoneliö.

Nyt on käydä läpi kunkin vinoneliö tämän luettelon ja samojen. Harjoitteluistonto kautta, kun kyseessä on Hae: bunch pisteistä, yksi kullekin vinoneliö.

![Pistekaavio piirto](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Piirrä mallin arvopisteiden kautta

Jos tarkastelet pisteet ja squint kokoelman näyttää nyt kuin fat, epäselvältä rivi. Syy voidaan toteuttaa Microsoftin merkki ja Piirrä suora viiva.

Piirtämällä viiva luomaasi *mallia*. Ajatella tätä muodossa ottaen todellisuudessa ja tehdä siihen simplistic piirretty versio. Nyt piirretty on väärä - rivi ei siirry kaikki arvopisteiden kautta. Mutta se on hyödyllinen yksinkertaistaminen.

![Lineaarisen regressiosuoran kulmakerroin](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Kertoma on pistettä ei täsmälleen läpi menevien rivi on OK. Tietoja tutkijoiden kerrotaan tämä on malli -, joka on viiva -, ja valitse kukin piste on joitakin *Ääni* - tai *varianssi* liittyy sanomalla. On pohjana sopivaa yhteys ja valitse ei, joka Lisää ääni- ja epävarmuutta seuraavaksi siirrytään, reaali maailman.

Koska olet yritetään kysymykseen *paljonko?* eli *regression*. Ja esimerkissä on käytössä suoran viivan, koska se on *lineaarisen regressiosuoran*.

## <a name="use-the-model-to-find-the-answer"></a>Etsi vastaus mallin avulla

Nyt on mallin ja pyydämme se Microsoftin kysymys: kuinka paljon 1.35 carat vinoneliön maksaa?

Tutustu kysymykseen syy eyeball 1.35 carats ja piirtää pystyviivan. Kun se ylittää mallirivi, emme eyeball vaakaviivan valuutta-akseli. Se käynnit oikealle on 10 000. Ylävaunua! Tämä on vastaus: 1.35 carat vinoneliön kustannuksia noin 10 000 euroa.

![Etsi vastaus mallin](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Luo LUOTTAMUSVÄLI

On luonnollinen ihmetellä, kuinka tarkkoja tämän tekstinsyöttö eivät ole. Se on hyvä tietää, onko 1.35 carat timantti on hyvin lähellä 10 000, tai usein ylemmäs tai alemmas. Kuva tämä ulos Piirrä oletetaan, että Kirjekuori, joka sisältää useimmat pisteiden regressiosuoran ympärille. Tämä kirjekuori kutsutaan Microsoftin *luottamusvälin*: harjoitteluistonto melko varma, että hinnat ovat määritetyllä välillä tämän Kirjekuori, koska edellisten useimmissa ne on. Olemme piirtää kaksi Lisää vaakaviivat jossa 1.35 carat suora Leikkaa ylä- ja kirjekuorta alareunaan.

![Luottamusväli](./media/machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Nyt on lukee jotakin tietoja Microsoftin luottamusvälin: emme voi sano eri neljänneksien 1.35 carat vinoneliön hinta on noin $ 10 000 – mutta voi olla mahdollisimman alhainen 8 000 $ ja voi olla yhtä suuret kuin 12 000 euron.

## <a name="were-done-with-no-math-or-computers"></a>Olemme olet valmis, ilman matemaattiset tai tietokoneissa

On ollut mitä tietoja tutkijoiden maksetaan tekemään ja on ollut se vain piirustuksen mukaan:

* Olemme kysytyt kysymyksen, emme voi vastata tiedoilla
* Olemme valmista *mallia* käyttämällä *muuttujan lineaarinen regressio*
* Emme tehneet *tekstinsyöttö*ja *LUOTTAMUSVÄLI*

Ja Microsoft ei käytä matemaattisten merkkien tai tietokoneiden käsiteltävä.

Nyt, jos on ollut lisätietoja, kuten...

* timantti Leikkaa
* värin variaatiot (miten lähellä timantti on parhaillaan valkoinen)
* timantti sisällytykset määrä

.. .Lisää sitten Meillä on ollut Lisää palstoja. Tässä tapauksessa matemaattiset tulee hyödyllisiä. Jos sinulla on enemmän kuin kaksi saraketta, voi olla vaikea Piirrä pistettä paperille. Matemaattiset Voit sovittaa kyseisen rivin tai kyseisen tasossa tietoihin erittäin hyvin.

Myös, jos sijaan vain joidenkin ruudut, on ollut kahden tuhat tai kaksi miljoonaa ja valitse Tee tukevat paljon nopeammin tietokoneen kanssa.

Nykyään puhuimme olet lineaarisen regressiosuoran hallinnasta ja on tehty tekstinsyöttö, kuinka tietoja.

Muista kuitata ulos "Tietojen tiede varten aloittelijoille" Microsoft Azure koneen Learning muiden videot.



## <a name="next-steps"></a>Seuraavat vaiheet

  * [Kokeile ensimmäisessä tietojen tiede kokeessa koneen Learning Studiossa](machine-learning-create-experiment.md)
  * [Hanki Microsoft Azure koneen Learning esittely](machine-learning-what-is-machine-learning.md)
