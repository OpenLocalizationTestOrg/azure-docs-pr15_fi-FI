<properties
   pageTitle="Odottaa tietoja tiede tiedot? Tietojen arviointi | Microsoft Azure"
   description="Lue tietoja on valmis tietojen tiede 4 ehdot. Tietoja tiede aloittelijoille video 2 on betonin esimerkkejä, jotka auttavat perustiedot arvioinnin kanssa."
   keywords="tarvittavat tiedot, arvioi tietoja, tiedot, tietojen ehtoja, valmis tietojen valmisteleminen"
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


# <a name="is-your-data-ready-for-data-science"></a>Odottaa tietoja tiede tiedot?

## <a name="video-2-data-science-for-beginners-series"></a>Video 2: Tietojen tiede aloittelijoille sarjan

Opettele varmistaaksesi, että se on valmis tietojen tiede basic ehtoja vastaavien tietojen laskeminen.

Pääset tehokkaasti sarjan katsoa ne kaikki. [Siirry videoiden luettelo](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-is-your-data-ready-for-data-science]

## <a name="other-videos-in-this-series"></a>Muita videoita sarjassa

*Tietoja tiede aloittelijoille* on viisi lyhyiden videoiden tiede tietojen lyhyt esittely.

  * Video 1: [5 kysymyksiä tietojen tiede vastauksia](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 minuuttia 14 sekuntia)*
  * Video 2: On tiedot valmiina tiedot tiede?
  * Video 3: [Kirjoita kysymys voi vastata tiedoilla](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 s)*
  * Video 4: [Ennustaa vastaus yksinkertainen malli](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 s)*
  * Video 5: [Muiden käyttäjien työn alla tietojen tiede kopioiminen](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 minuuttia 18 sekuntia)*

## <a name="transcript-is-your-data-ready-for-data-science"></a>Tallenne: On tiedot valmiina tiedot tiede?

Tervetuloa käyttämään "Odottaa tietoja tietojen tiede?" sarjan *Tietojen tiede aloittelijoille*toinen video.  

Ennen tietojen tiede voi antaa sinulle haluamasi vastaukset, sinun on annettava joitakin laadukkaita raaka-käyttöä varten. Aivan kuten tekeminen pizza sitä paremmin ainesosat, voit aloittaa kanssa, sitä parempia lopullisen tuotteen.

## <a name="criteria-for-data"></a>Tietoja perusteet

Tietoja tiede kyseessä ongelmaan on joitakin ainesosat, joita tarvitaan erotettu yhdessä.

Tarvitsemme tiedot, jotka ovat:

  * Asiaa
  * Yhdistetty
  * Tarkka
  * Riittävän käyttöä varten

## <a name="is-your-data-relevant"></a>Sopii tiedot?

Jotta ensimmäinen ainesosan - annettava liittyviä tietoja.

![Aiheeseen liittyviä tietoja ole merkitystä tiedot – ja arvioida tietoja](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-relevant-and-irrelevant-data.png)

Tarkista taulukon vasemmalla puolella. Olemme seitsemän Boston palkit ulkopuolisiin henkilöihin täyty, mitattuna niiden Verenpaineen alkoholin taso, niiden viimeisen Ottelu punainen Sox batting-keskiarvo ja hinnan osuutta lähimpään helppokäyttöisyys kauppa.

Tämä on täysin oikeutetut tiedot. Se on vain vika on ei asiaa. Näytetyt määrät välillä ei ole ilmeisimmät yhteyttä. Jos voin sait nykyinen hinta maidon ja punainen Sox batting keskiarvon, ei ei voitu arvata Omat Verenpaineen olut voi.

Nyt Tarkista taulukon oikealla. Tällä hetkellä on mitattu kunkin henkilön massa teksti ja laskea juomien heillä on ollut määrä.  Kunkin rivin numeroiden nyt liittyvät toisiinsa. Jos voin antanut Omat leipätekstin massa ja voin joku toinen on käynyt Margaritas määrä, voit tehdä arvaus on oma Verenpaineen alkoholin sisällön.

## <a name="do-you-have-connected-data"></a>Yhdistettyjä tietoja?

Seuraava ainesosa on yhdistetyt tiedot.

![Yhdistettyjen tietojen ja katkaistut tiedot – tietojen perusteiden tiedot valmiina](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-connected-vs-disconnected-data.png)

Tässä on joitakin olennaiset tiedot hamburgers laatua: grilli lämpötilan, patty paksuus ja aikakauslehden paikallisen ruoka luokitus. Mutta huomata vasemmalla olevasta taulukosta aukkoja.

Useimmat tietojoukot puuttuu joitakin arvoja. Yhteistä aukkoja tältä ja tavalla voit kiertää niitä. Mutta jos näkyvissä on liikaa puuttuu, tietojen alkaa Sveitsin juusto muistuttavan.

Jos tarkastelet taulukon vasemmalla, on niin paljon puuttuu tietoja voi olla vaikea tulee kaikenlaista yhteyden SÄLEIKÖN lämpötilan ja patty paino välillä. Tämä on esimerkki katkaistut tiedot.

Taulukon oikealla on täynnä, ja Suorita - esimerkki yhdistetyt tiedot.

## <a name="is-your-data-accurate"></a>Ovatko tietosi ovat?

Seuraava ainesosa annettava on tarkkuutta. Seuraavassa on neljä kohteet, jotka haluamme osumien nuolia.

![Tarkkoja tietoja ja virheellisiä tietoja - tietojen ehdot](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-inaccurate-vs-accurate-data.png)

Tarkista kohteen oikeassa yläkulmassa. Microsoft on käytössä tiiviisti ryhmittely oikealle maalitaulu ympärille. Tämän kurssin, paikkansa. Väärin tietojen tiede kielellä, tutustu suorituskyvyn kohde sen alapuolella oikealla myös pidetään tarkkoja.

Jos kartoittaa näitä nuolia keskelle, voit nähdä, että se on erittäin lähellä maalitaulu. Nuolia leviävät kaikki ympärille kohde, jotta ne on pidettäviä epätarkkoja, mutta ne on keskitetty ympärille maalitaulu, jotta ne on pidettäviä tarkkoja.

Tarkista nyt vasemmassa kohde. Tässä Microsoftin nuolet osumien erittäin lähellä toisiaan tiiviisti ryhmittely. Ne ovat tarkat, mutta ne ovat virheellisiä, koska keskellä on tapa maalitaulu käytöstä. Ja, kohde-vasemmassa alareunassa olevia nuolia ovat virheellisiä sekä epätarkkoja. Tämä archer on useita käytännössä.

## <a name="do-you-have-enough-data-to-work-with"></a>Sinulla on riittävästi tietoja?

Lopuksi ainesosan #4 - annettava on riittävästi tietoja.

![Sinulla on tarpeeksi tietojen analysointia varten? Tietojen arviointi](./media/machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science/machine-learning-data-science-barely-enough-data.png)

Ajattele kunkin arvopisteen siten, että sivellin viiva, maalaus-taulukossa. Jos sinulla on vain ne joidenkin, maalaus voi olla melko epäselvältä - on vaikea hahmottaa, mitä.

Jos lisäät joitakin muita sivellin piirtojen, että maalaus käynnistyy Hae hieman terävän.

Jos sinulla on hankala edes erottaa tarpeeksi piirtojen, näet juuri sen verran joitakin laaja päätösten. On jotakin minun pitää käy? Vaikuttaa siltä, Värikäs-, joka näyttää SIIVOA veden – Kyllä, eli kun valitsen lomalla.

Kun lisäät tietoa, kuva tulee selkeä ja voit tehdä tarkempia päätöksiä. Nyt näyttää kolme hotelleja vasemman pankki-palvelussa. Tiedät, pidän todella etualalla yksi arkkitehtuuri ominaisuuksia. Minulla Jatka siellä kolmannen kerroksen.

Tiedot, jotka ovat asiaa, yhdistetyt, tarkkoja ja niin, että on kaikkien annettava valmistusaineiden tehdä joitakin laadukkaita tietojen tiede.

Muista kuitata ulos muita *Tietoja tiede aloittelijoille* Microsoft Azure koneen Learning neljä videoita.




## <a name="next-steps"></a>Seuraavat vaiheet

  * [Kokeile ensimmäisessä tietojen tiede kokeessa koneen Learning Studiossa](machine-learning-create-experiment.md)
  * [Hanki Microsoft Azure koneen Learning esittely](machine-learning-what-is-machine-learning.md)
