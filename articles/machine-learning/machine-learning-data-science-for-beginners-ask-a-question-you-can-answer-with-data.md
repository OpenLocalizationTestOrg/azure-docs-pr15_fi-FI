<properties
   pageTitle="Kirjoita kysymys voi vastata ja tiedot – muodostetaan kysymyksiä | Microsoft Azure"
   description="Lue, miten muodostetaan tietojen tiede kysymys tietojen tiede aloittelijoille video 3. Sisältää luokittelu ja regression kysymyksiä vertailu."
   keywords="tietoja tiede kysymyksiä muodostetaan kysymyksiä, regression kysymysten ja luokittelukysymyksiä, tarkkoja kysymys"
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

# <a name="ask-a-question-you-can-answer-with-data"></a>Kirjoita kysymys voi vastata tiedoilla

## <a name="video-3-data-science-for-beginners-series"></a>Video 3: Tietojen tiede aloittelijoille sarjan

Lue, miten muodostetaan tietojen tiede kysymys tietojen tiede aloittelijoille video 3. Tässä videossa on luokittelu ja regression algoritmeista kysymykset vertailu.

Pääset tehokkaasti sarjan katsoa ne kaikki. [Siirry videoiden luettelo](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-ask-a-question-you-can-answer-with-data]

## <a name="other-videos-in-this-series"></a>Muita videoita sarjassa

*Tietoja tiede aloittelijoille* on viisi lyhyiden videoiden tiede tietojen lyhyt esittely.

  * Video 1: [5 kysymyksiä tietojen tiede vastauksia](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 minuuttia 14 sekuntia)*
  * Video 2: [tiedoissa on valmiina tietojen tiede?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 minuuttia 56 sekuntia)*
  * Video 3: Voit vastata tiedoilla kysymyksen
  * Video 4: [Ennustaa vastaus yksinkertainen malli](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 s)*
  * Video 5: [Muiden käyttäjien työn alla tietojen tiede kopioiminen](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 minuuttia 18 sekuntia)*

## <a name="transcript-ask-a-question-you-can-answer-with-data"></a>Tallenne: Voit vastata tiedoilla kysymyksen

Tervetuloa käyttämään sarjan kolmas videon "Tietojen tiede aloittelijoille".  

Valitse tämä vaihtoehto Saat vihjeitä, laatia tietoja voi vastata kysymykseen.

Voit saada lisää Tässä videossa ulos Jos katselet sarjassa kaksi aiemmissa videot: "5 kysymyksiä tietojen tiede voi vastata" ja "On tiedot ovat valmiita tietojen tiede?"

## <a name="ask-a-sharp-question"></a>On terävä kysymys

Puhuimme olet miten tiedot tiede on käyttäminen (kutsutaan myös luokkien tai otsikot) nimet ja numerot ennustaa vastaus kysymykseen. Mutta se ei voi olla vain kaikki kysymykset; sen on oltava *terävä kysymys.*

Monimerkityksinen kysymys ei ole vastattu, nimi tai numero. Terävä kysymys on.

Kuvitellaan löytämäsi Taikalamppu genie kuka tahansa koskevaa kysymystä truthfully vastaa kanssa. Mutta se on mischievous genie ja hän yrität tehdä hänen answer monimerkityksinen ja sekava, kun hän saa heti. Haluat kiinnittää hänelle ilmatiivis niin, että hän ei voi auttaa mutta kertovat, mitä haluat tietää kysymys.

Jos kysymyksen monimerkityksinen, kuten "Mistä on kyse tapahdu Omat stock?" genie voi vastata, "hinnan muuttuu". Joka on ilmoittamat vastausta, mutta ei on erittäin hyödyllinen.

Mutta jos se on terävä kysymyksen, kuten "mitä Omat stock myyntihinnan ensi viikolla?", genie et voi auttaa mutta avulla voit määrittää tietyn vastaukset ja ennustaa myyntihinnan.

## <a name="examples-of-your-answer-target-data"></a>Esimerkkejä vastausta: kohdesovellus

Kun voit käyttää kysymys, tarkista, onko esimerkkejä vastaus toimialueella tiedoissa.

Jos Microsoftin kysymys on "mikä oma stock myyntihinnan ensi viikolla?" Valitse on että tietojamme sisällyttää osakkeen hinnan historia.

Jos Microsoftin kysymys on "Omat laivaston mitä auton suorittaminen epäonnistuu ensin?" Valitse on että tietojamme sisällyttää edellisen virheiden tietoja.

![Kohteen tiedot – esimerkkejä vastausta. Antaa tietoja tiede kysymys.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-target-data.png)

Näissä esimerkeissä vastausten kutsutaan kohde. Kohde on, mikä on rooliin ennustetaan tietoja tietojen pisteet, onko luokka tai numero.

Jos sinulla ei ole kohteen tietoja, tarvitset saat joitakin. Et voi vastata kysymykseen ilman sitä.

## <a name="reformulate-your-question"></a>Reformulate kysymys

Joskus voi sanamuodon saat enemmän hyötyä vastausta kysymykseesi.

Kysymys "Tarkoittavat tämän tietojen pisteen A ja B?" ennustaa luokan tai nimi tai otsikko jotain. Jos haluat vastata siihen, Käytämme *luokitus algoritmin*.

Kysymyksen "Mitä?" tai "Kuinka monta?" ennustaa summan. Vastaa Käytämme *regression algoritmin*.

Jos haluat nähdä, miten Microsoft voit muuttaa näitä, katsotaan kysymykseen "mitä selitykseen on kiinnostavimmat tällä lukuohjelmalla?" Se pyytää tekstinsyöttö monia mahdollisuuksia - yksittäisen valitsemaan, toisin sanoen "On tässä A tai B tai C ja D?" - ja käyttäisi luokitus-menetelmällä.

Mutta kysymys voi olla helpompaa vastaa Jos sen sanamuodon "miten mielenkiintoista on tämän luettelon tällä lukuohjelmalla kunkin juttu?" Nyt voit tarkastella kunkin artikkelin numeerisen tuloksen ja sitten on helppo tunnistaa suurin näkyvissä pistemäärä-artikkelissa. Tämä on Muotoile luokitus kysymyksen kyselyjä regression kysymys tai kuinka paljon?

![Reformulate kysymyksesi. Regressiosuoran kysymys ja luokittelu kysymys.](./media/machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data/machine-learning-data-science-classification-question-vs-regression-question.png)

Miten voidaan esittää kysymyksen on clue, mitkä algoritmin voi antaa vastaukseksi.

Huomaat, että tiettyjä algoritmit - Tarinan uutisia - esimerkissä ilmestyä perheille läheisesti liittyvät. Voit reformulate käyttämään algoritmin, jonka avulla voit eniten hyötyä vastauksen kysymykseesi.

Mutta, tärkeimmät Pyydä terävä kysymyksen - kyselyn, joka voi vastata tiedoilla. Ja varmista, että sinulla on oikeat tiedot vastata siihen.

Puhuimme on joitakin perusperiaatteet kysymyksen voi vastata tiedoilla.

Muista kuitata ulos "Tietojen tiede varten aloittelijoille" Microsoft Azure koneen Learning muiden videot.


## <a name="next-steps"></a>Seuraavat vaiheet

  * [Kokeile ensimmäisessä tietojen tiede kokeessa koneen Learning Studiossa](machine-learning-create-experiment.md)
  * [Hanki Microsoft Azure koneen Learning esittely](machine-learning-what-is-machine-learning.md)
