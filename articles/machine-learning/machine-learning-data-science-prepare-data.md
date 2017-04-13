<properties
    pageTitle="Tehtävien tietojen valmisteleminen varten parannetun koneen learning | Microsoft Azure"
    description="Esikäsittely ja tietojen valmisteleminen koneen learning Puhdista."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016" 
    ms.author="bradsev" />


# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Tehtävien tietojen valmisteleminen varten parannetun koneen oppiminen

Esikäsittely ja puhdistus tiedot ovat tärkeitä tehtäviä, jotka yleensä on suoritettava ennen tietojoukon avulla voidaan tehokkaasti koneen learning. Tietoja on usein häiriöistä ja epäluotettavista ja voi puuttua arvot. Näitä tietoja käyttämällä mallinnus voidaan tuottaa harhaanjohtavia tulokset. Nämä tehtävät ovat osa ryhmän tietojen tiede prosessi (TDSP) ja noudata yleensä alkuperäinen tarkasteluun, tutustu ja suunnitella vanhat käsittely edellyttää tietojoukko. Katso tarkempia ohjeita TDSP prosessia, [Ryhmän tietojen tiede prosessin](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)artikkelin.

Vanhat käsittely ja puhdistus tehtäviä, kuten tietojen tarkasteluun, tehtävä voidaan suorittaa-ympäristössä, kuten SQL- tai rakenne- tai Azure koneen Learning Studio ja eri työkaluja ja kielillä, kuten R tai Python, sen mukaan, johon tiedot on tallennettu ja miten se on muotoiltu erilaisia. TDSP on iteratiivinen laatu, nämä tehtävät voidaan hallita työnkulun prosessin eri vaiheet.

Tässä artikkelissa esitellään eri tietojen käsittely käsitteitä ja tehtäviä, jotka voidaan toteuttaa ennen tai jälkeen ingesting tietojen tuominen Azure koneen Learning.

Esimerkki tietojen tarkasteluun ja valmis sisällä Azure koneen Learning studio vanhat käsittely-artikkelissa [esikäsittely tietojen Azure koneen Learning Studiossa](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) -video.


## <a name="why-pre-process-and-clean-data"></a>Miksi esikäsittely ja Puhdista tiedot?

Käytännön tiedot kerätään eri lähteistä ja prosesseja, ja se voi olla sääntöjenvastaisuuksien tai vioittunut vaarantavat dataset laadun tiedot. Tietojen laadun ongelmat, jotka aiheutuvat ovat seuraavat:

* **Incomplete**: tietojen puuttuu määritteet tai puuttuvia arvoja, joka sisältää.
* **Noisy**: tiedoissa on virheellisiä tietueita tai harha.
* **Ristiriitainen**: tiedoissa on ristiriitaiset tietueet tai ristiriidat.

Tietojen laadun on laatu ennakoivan malleille. Voit välttää "roskaa roskaa-" ja parantaa tietojen laadun ja mallin vuoksi suorituskyvyn, on ehdottoman tärkeää Järjestä tiedot kunto näyttöön havaita ongelmien ratkaiseminen alkuvaiheessa ja päättää vastaavien tietojen käsittely ja puhdistus vaiheet.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Mitkä ovat joidenkin tietojen kunto näyttöjä, jotka työskentelevät?

Olemme tarkistaa Yleiset tietojen laadun valitsemalla:

* **Tietueiden**määrän.
* Määrä **määritteet** (tai - **Ominaisuudet**).
* Määritteen **tietotyypit** (korko.vuosi, järjestysluku tai jatkuva).
* **Puuttuvien arvojen**määrän.
* **Sekä formedness** tiedot.
    * Jos tiedot ovat TSV tai CSV-Tarkista, että erottimet sarakkeen ja rivin erottimet aina oikein erota sarakkeita ja rivejä.
    * Jos tiedot ovat HTML- tai XML-muodossa, tarkista, onko tiedot on hyvin muodostettu perusteella niiden vastaaviin mukaisia.
    * Jäsennyksen myös ehkä tarpeen Pura jäsenneltyä tietoa puolirakenteinen tai rakenteeton tiedoista.
* **Epäyhtenäinen tietueita**. Valitse sallittu arvojen alueeseen. Esimerkiksi jos tiedoissa student keskiarvon, tarkista, onko keskiarvon on nimetyn alueen sano 0 ~ 4.

Kun olet löytänyt tietojen ongelmat, **käsittelyn vaiheet** tarvitaan, johon kuuluu usein puhdistus puuttuvia arvoja, tietojen normalisointi, discretization tekstin käsittely poistaa ja/tai korvata upotetun merkit, jotka voivat vaikuttaa tietojen tasausta, erilaiset tietotyypit yhteiset kentät ja muiden.

**Azure koneen Learning siinä käytetään muotoiltu taulukkomuotoiset tiedot**.  Jos tiedot ovat jo taulukkomuodossa, vanhat tietojenkäsittely voi käyttää suoraan Azure koneen Learning koneen Learning Studiossa kanssa.  Jos tiedot eivät ole taulukkomuodossa, että se on XML-jäsentäminen voidaan tarvita, jos haluat muuntaa tiedot taulukkomuotoon.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Mitkä ovat tärkeimmät tehtävät vanhat tietojenkäsittely?

* **Tiedot**: täyttää tai puuttuvia arvoja, haku- ja häiriöistä tietojen ja harha.
* **Muunnetaan**: normalisointi tietojen vähentää mitat ja vähentämiseksi.
* **Tietoja vähentämistä**: esimerkki tiedot tai määritteet helpottaa tietojen käsittelyä.
* **Tietojen discretization**: jatkuva määritteet muuntaminen helppous categorical määritteet tiettyjen tietokoneen learning menetelmillä.
* **Tekstin puhdistus**: Poista upotettu merkkejä, joita saattaa aiheuttaa tietojen kohdistusvirhe, esimerkiksi upotetun välilehtien sarkaineroteltuun datatiedoston upotettujen uudet rivit, jotka voivat katkaista tietueet jne.

Alla olevassa osissa kuvataan tietojen käsittely seuraavasti.

## <a name="how-to-deal-with-missing-values"></a>Voit käsitellä puuttuvat arvot?

Käsitellä puuttuvia arvoja, on parasta tunnistaa ensin paremmin kahvaa puuttuvat arvot syy ongelma. Tyypillinen puuttuu arvo käsittely menetelmiä ovat seuraavat:

* **Poisto**: puuttuvat arvot sisältävien tietueiden poistaminen
* **Nuken korvaaminen**: korvaa puuttuvat arvot tyhjä arvo: esimerkiksi, _Tuntematon_ categorical tai 0, numeeriset arvot.
* **Tarkoittaa korvaaminen**: Jos puuttuvat tiedot on numeerinen, korvaa puuttuvat arvot keskiarvosta.
* **Usein käytetyt korvaaminen**: Jos puuttuvat tiedot on categorical, korvaa puuttuvat arvot eniten kohde
* **Regressiosuoran korvaaminen**: korvaa puuttuvat arvot regressed arvoilla regression menetelmän avulla.  

## <a name="how-to-normalize-data"></a>Miten tietojen normalisointi?

Tietojen normalisointi Skaalaa uudelleen tietyn alueen numeeriset arvot. Suositut tietojen normalisointi menetelmiä ovat seuraavat:

* **Min-Maks normalisointi**: lineaarisesti Muunna alueeksi tiedot, sano väliltä 0 – 1, jossa pienin arvo on sovitettu 0 ja suurin arvo 1.
* **Z-tulos normalisointi**: skaalata tiedot keskiarvo ja keskihajonta perusteella: tiedot ja keskiarvosta jaetaan keskihajonnan.
* **Desimaali skaalaus**: skaalata tiedot siirtämällä määritteen desimaalipilkun.  

## <a name="how-to-discretize-data"></a>Miten discretize tiedot?

Tietoja voi discretized muuntamalla jatkuva arvot korko.vuosi määritteet tai väliajoin. Operaattoria tekoa ovat seuraavat:

* **Yhtä suuri kuin leveys Binning**: jakaa kaikista mahdollisista arvoista määritteen alueen N ryhmiin samankokoisia ja määrittää arvot, jotka kuuluvat roskakorissa olevat bin numeron.
* **Yhtä suuri kuin korkeus Binning**: jakaa kaikista mahdollisista arvoista määritteen alueen N ryhmiin esiintymät sama määrä kussakin ja määrittää arvot, jotka kuuluvat roskakorissa olevat bin numeron.  

## <a name="how-to-reduce-data"></a>Voit vähentää tietoja?

Voit pienentää tiedot koon helpottaa tietojen käsittely eri tavoilla. Eri tietojen koko ja toimialueen voi suojata seuraavista tavoista:

* **Tietueen esimerkkejä**: esimerkki tietueet ja valita vain edustavan osajoukko tiedot.
* **Esimerkkejä määrite**: Valitse tärkeimmät määritteiden osajoukko tiedoista.  
* **Kooste**: jakaa tietoja ryhmiin ja tallentaa kunkin ryhmän lukuja. Esimerkiksi ravintolaan yhdistettyjen 20 vuoden aikana Päivittäinen tuotto numerot on koostettava kuukausittain tuloja tiedot koon pienentämiseksi.  

## <a name="how-to-clean-text-data"></a>Miten Puhdista tekstitietoja?

**Tekstikentät ne** voivat sisältää merkkejä, jotka vaikuttavat sarakkeiden tasaus-ja/tai tietueen rajat. Esimerkiksi välilehdet upotettuna sarkaineroteltuun tiedostoon syy sarakkeen kohdistusvirhe ja upotettu rivinvaihtomerkit katkaista tietueen rivit. Virheellisestä tekstin koodauksen käsittely luettaessa kirjoittaminen ja tekstin johtaa tietojen menettämistä tahattoman johdanto voi lukea merkkiä, esimerkiksi Null-arvot ja voi myös vaikuttaa tekstin jäsennys. Varovainen jäsennyksen tarkasteleminen ja muokkaaminen voidaan tarvita, jotta Puhdista tekstikenttiä, oikea tasaus ja/tai Pura Jäsennettyjen tietojen rakenteeton tai puolirakenteinen tekstin tiedoista.

**Tietojen etsintä** -toiminto tarjoaa aikainen näkymän tietoa. Ongelmien ratkaiseminen määrä voi olla peittämättömästä tässä vaiheessa ja vastaavia menetelmiä voi suojata näiden ongelmien korjaamiseen.  On tärkeää esittää kysymyksiä, kuten mikä ongelman aiheuttaja on ja miten ongelma saattaa on otettu käyttöön. Tämä myös avulla voit päättää tietojen käsittelyvaiheet, jotka on otettava ratkaisemiseen. Millaisia tietoja jokin aikoo johdettu tiedot voidaan myös priorisoida tietojen käsittely työmäärään.

## <a name="references"></a>Viittaukset

>*Mahdollisista: ja tekniikoiden*, kolmas Edition Morgan Kaufmann 2011 Jiawei Han Micheline Kamber ja Jian Pei
