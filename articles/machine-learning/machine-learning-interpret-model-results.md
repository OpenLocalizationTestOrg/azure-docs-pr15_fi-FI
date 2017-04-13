<properties
    pageTitle="Tulkita mallin tulokset koneen Learning | Microsoft Azure"
    description="Kokousajan valitseminen optimaalisen parametrin määrittäminen algoritmin avulla ja kesäolympialaisten visualisointi pistemäärän mallin tulostaa."
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
    ms.author="bradsev" />


# <a name="interpret-model-results-in-azure-machine-learning"></a>Mallin tulokset Azure koneen Learning tulkitseminen

Tässä ohjeaiheessa kerrotaan, miten voit visualisoida ja tulkita tekstinsyöttö tulokset Azure koneen Learning Studiossa. Kun olet koulutus mallin ja valmis ennusteiden selvästi ("tulos mallin"), sinun on ymmärtää ja tulkita ennustamisominaisuudet-tulos.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

On neljä konepohjaisten oppimistekniikoiden mallien Azure Konepohjaisten Oppimistekniikoiden pää tiedostotyyppejä:

* Luokitus
* Klusterointi
* LOGREGR-funktio
* Suosittelija järjestelmät

Käytetään tekstinsyöttö päälle näitä malleja moduulit ovat:

* [Mallin sija] [ score-model] luokittelu ja regression-moduuli
* [Määritä klustereihin] [ assign-to-clusters] klusterointi-moduuli
* [Pisteet Matchbox suosittelija] [ score-matchbox-recommender] suositus Systemsin

Tässä asiakirjassa kerrotaan, miten merkitys tekstinsyöttö tulokset kullekin näitä moduuleja. Yleiskatsaus näitä moduuleja Katso, [miten voit valita parametrit optimoi oman Azure koneen Learning algoritmit](machine-learning-algorithm-parameters-optimize.md).

Tässä ohjeaiheessa käsitellään tekstinsyöttö tulkinnassa, mutta ei mallin arviointi. Lisätietoja siitä, miten arvioimaan mallin, katso, [miten voit arvioida mallin suorituskyvyn Azure koneen Learning](machine-learning-evaluate-model-performance.md).

Jos ole aiemmin käyttänyt Azure koneen oppiminen ja luoda yksinkertaisen kokeen aloittamaan, katso [Luo yksinkertaisia kokeen Azure koneen Learning Studiossa](machine-learning-create-experiment.md) Azure koneen Learning Studiossa.

## <a name="classification"></a>Luokitus ##
Luokitus ongelmia kaksi alaluokkia:

* Vain kahteen luokkaan (kaksi luokan tai binaarinen luokitus) liittyviä ongelmia
* Ongelmia enemmän kuin kaksi luokat (usean luokan luokitus)

Azure koneen Learning on eri moduuleissa käsitellä eri luokittelu, mutta menetelmät tulkinnassa tekstinsyöttö kenttien tulosten muistuttavat.

### <a name="two-class-classification"></a>Kahden luokan luokittelu###
**Esimerkki kokeen**

Esimerkki kahden luokan luokitus ongelma on Tiina kukat luokitus. Tehtävä on luokitella Tiina kukat niiden ominaisuuksien perusteella. Tiina tietojoukon annettu Azure koneen Learning on Suositut [Tiina tietojoukon](http://en.wikipedia.org/wiki/Iris_flower_data_set) alijoukkoa, joka sisältää sisältävän esiintymät vain kaksi kukka lajin (luokat 0 – 1). On neljä ominaisuudet kunkin kukka (sepal pituus, sepal leveys, Terälehti pituus ja Terälehti leveys).

![Näyttökuva Tiina kokeen](./media/machine-learning-interpret-model-results/1.png)

Kuva 1. Tiina kahden luokan luokitus ongelma kokeen

Kokeen on suoritettu voit ratkaista tämän ongelman, kuten kuvassa 1. Kahden luokan tehosti päätös puun malli on koulutus ja tulos. Nyt voit havainnollistaa tekstinsyöttö tulokset [Pistemäärän mallin] [ score-model] moduulin output portti [Pistemäärän malli] napsauttamalla[ score-model] moduulin ja valitsemalla sitten **Visualisoi**.

![Tulos mallin moduuli](./media/machine-learning-interpret-model-results/1_1.png)

Tämä näyttää tulosmalli tulokset 2 kuvassa esitetyllä tavalla.

![Tiina kahden luokan luokitus kokeen tulokset](./media/machine-learning-interpret-model-results/2.png)

Kuva 2. Tulos mallin tuloksen kahden luokan luokitukseen visualisointi

**Tulos tulkitseminen**

Tulosten taulukkoa on kuusi saraketta. Vasen neljä saraketta on neljä ominaisuuksia. Oikea kaksi saraketta, tulos otsikoiden ja tulos todennäköisyys liittyy ovat tekstinsyöttö tulokset. Tulos todennäköisyys liittyy-sarakkeessa näkyy todennäköisyys kukkia kuuluu positiivinen luokan (luokka-1). Esimerkiksi sarake (0.028571) tarkoittaa on ensimmäinen numero on 0.028571 todennäköisyys, että ensimmäinen kukka kuuluu luokan 1. Tulos otsikot-sarakkeessa kunkin kukka budjetoidut luokka. Tämä perustuu tulos todennäköisyys liittyy-sarake. Kukkia scored todennäköisyys on suurempi kuin 0,5, jos se on ennustettujen luokan 1. Muussa tapauksessa se on ennustettujen luokaksi 0.

**Web-palvelun julkaisu**

Kun tekstinsyöttö tulokset on ymmärretty ja kolmannekselle ääni, kokeilla voidaan julkaista web-palvelu niin, että voit ottaa sen eri sovelluksiin ja anna sille hankkiminen luokan ennusteiden, valitse kaikki uudet Tiina kukka. Muuta koulutusta kokeen tulosmalli kokeen ja julkaista sen verkkopalvelun-kohdassa [Julkaise Azure koneen Learning WWW-palvelun](machine-learning-walkthrough-5-publish-web-service.md). Tämän toiminnon avulla voit tulosmalli kokeen 3 kuvassa esitetyllä tavalla.

![Näyttökuva näkyvissä kokeen pistemäärä](./media/machine-learning-interpret-model-results/3.png)

Kuva 3. Näkyvissä Tiina kahden luokan luokitus ongelma kokeen pistemäärä

Nyt tarvitset syötteen ja tulosteen WWW-palvelun määrittäminen. Syöte on [Tulos mallin]oikean syötteen portti[score-model], joka on Tiina kukka ominaisuudet syöte. Tulosteen valinta määräytyy sen mukaan, onko olet kiinnostunut budjetoidut luokan (tulos otsikko) ja/tai scored todennäköisyys. Tässä esimerkissä oletetaan, että olet kiinnostunut molemmat. Voit valita haluamasi näytettävät sarakkeet käyttämällä [Tietojoukon Valitse sarakkeet] [ select-columns] moduuli. Valitse [Valitse sarakkeet tietojoukon][select-columns], valitse **Käynnistä sarakevalitsin**ja valitse **Tulos otsikot** ja **Tulos todennäköisyys liittyy**. [Valitse tietojoukon sarakkeet] tulostusportin määritettyäsi[ select-columns] ja käynnistää uudelleen, sinulla on oltava valmis julkaisemaan tulosmalli kokeen verkkopalvelun nimellä valitsemalla **JULKAISE WEB-palvelu**. Lopullinen kokeen näyttää kuva 4.

![Kokeen Tiina kahden luokan luokittelu](./media/machine-learning-interpret-model-results/4.png)

Kuva 4. Lopullinen tulosmalli kokeen Tiina kahden luokan luokitus liittyvän ongelman esimerkkitulokset

Kun suoritat web-palvelu ja jotkin ominaisuuden arvot testi esiintymän, tulos palauttaa kahden luvun. Ensimmäinen numero on scored otsikko ja toinen on scored todennäköisyys. Tämä kukka ennustetaan ylittyvän luokan 1 0.9655 todennäköisyys kanssa.

![Testaa siihen tulos-malli](./media/machine-learning-interpret-model-results/4_1.png)

![Näkyvissä testitulokset pistemäärä](./media/machine-learning-interpret-model-results/5.png)

Kuva 5. Web-palvelu tulos Tiina kahden luokan luokittelu

### <a name="multi-class-classification"></a>Usean luokan luokittelu
**Esimerkki kokeen**

Tämä kokeessa suoritetaan kirjain tunnistuksen tehtävän esimerkkinä multiclass luokitus. Valitsimen yrittää ennustetaan tiettyjä kirje (luokka) käsinkirjoitetun kuvat poimittujen käsinkirjoitetun joitakin määritearvojen perusteella.

![Kirjain tunnistuksen Esimerkki](./media/machine-learning-interpret-model-results/5_1.png)

Koulutus, tiedot on 16 käsinkirjoitetun kirjain kuvia poimittujen ominaisuuksia. 26 kirjaimen lomakkeen Microsoftin 26 luokat. Kuva 6 näkyy koe, joka multiclass luokitus mallin kirjain tunnistukseen kouluttaminen ja ennustaa määritetään testin tietojoukon samaa toimintoa.

![Kirjain tunnistuksen multiclass luokitus kokeen](./media/machine-learning-interpret-model-results/6.png)

Kuva 6. Kirjain tunnistuksen multiclass luokitus ongelma kokeen

Kesäolympialaisten visualisointi tulokset [Pistemäärän mallin] [ score-model] moduulin valitsemalla [Pisteet] mallin tulostusportin[ score-model] moduulin ja valitsemalla sitten **Visualisoi**, näkyviin tulee sisältöä, kuten kuva 7.

![Tulos mallin tulokset](./media/machine-learning-interpret-model-results/7.png)

Kuva 7. Usean luokan luokitus pistemäärän mallin tulokset visualisointi

**Tulos tulkitseminen**

Vasen 16 sarakkeet edustavat testi Määritä ominaisuuden arvot. Nimet, kuten tulos todennäköisyys liittyy luokan "XX" on vain sarakkeisiin, kuten kahden luokan tapauksessa tulos todennäköisyys liittyy-sarake. Ne näyttävät todennäköisyyden sille, vastaava kohta kuuluu tiettyyn luokkaan. Esimerkiksi on ensimmäisellä rivillä on 0.003571 todennäköisyys, että se on "A" 0.000451 todennäköisyys, että se on "B" ja niin edelleen. Viimeinen sarake (tulos otsikot) on sama kuin tulos otsikoiden kahden luokan tapauksessa. Se valitsee vastaava budjetoidut luokan luokan suurimman scored todennäköisyys kanssa. Jos esimerkiksi ensimmäisen tietueen scored otsikko on "F" koska se on suurin todennäköisyys on "F" (0.916995).

**Web-palvelun julkaisu**

Saat myös scored otsikko tekstien ja scored otsikko todennäköisyyden. Tavallinen logiikan on löytää suurin todennäköisyys kaikki scored mahdollisuuden todennäköisyyden kesken. Tällöin sinun on käytettävä [R-komentosarjan suorittaminen] [ execute-r-script] moduuli. Kuva 8 näkyy R-koodin ja kokeen tuloksen näkyy kuva 9.

![Esimerkki R-koodista](./media/machine-learning-interpret-model-results/8.png)

Kuva 8. R-koodi, joka poimii tulos otsikot ja otsikot todennäköisyys

![Kokeen tuloksen](./media/machine-learning-interpret-model-results/9.png)

Kuva 9. Lopullinen tulosmalli kokeen kirjain tunnistuksen multiclass luokitus ongelman

Kun julkaiseminen ja suorita web-palvelu ja kirjoita syöttötapa-toiminnon joitakin arvoja palautettu tulos-alkoholijuomat näyttävät kuva 10. "T" kanssa 0.9715 todennäköisyys on ennustetaan ylittyvän tämä käsinkirjoitetun kirje sen poimitun 16 ominaisuuksia.

![Testaa siihen tulos-moduuli](./media/machine-learning-interpret-model-results/9_1.png)

![Testitulos](./media/machine-learning-interpret-model-results/10.png)

Kuva 10. Web-palvelu tulos multiclass luokittelu

## <a name="regression"></a>LOGREGR-funktio

Regressiosuoran ongelmia poikkeavat luokitus ongelmia. Luokitus-ongelma yrität ennustaa erillinen luokat, jotka esimerkiksi luokan Tiina kukka kuuluu. Mutta kuten näet seuraavassa esimerkissä regression ongelmasta, yrität ennustaa jatkuva muuttuja, kuten Auto hinta.

**Esimerkki kokeen**

Käyttää autojen hinta tekstinsyöttö oman esimerkkinä regression. Yrität ennustaa hinnan auton sen ominaisuuksia, kuten merkki, polttoainetyyppi, perusrakenne ja aseman kiekkopainiketta perusteella. Kokeen näkyy kuva 11.

![Autojen hinta regression kokeen](./media/machine-learning-interpret-model-results/11.png)

Kuva 11. Autojen hinta regression ongelma kokeen

Kesäolympialaisten visualisointi [Pistemäärän mallin] [ score-model] moduuli, tulos muistuttaa kuva 12.

![Näkyvissä tulokset autojen hinta tekstinsyöttö ongelman pistemäärä](./media/machine-learning-interpret-model-results/12.png)

Kuva 12. Tulos näkyvissä pistemäärä autojen hinta tekstinsyöttö ongelman

**Tulos tulkitseminen**

Tulos otsikot on tulosmalli tuloksen tulos-sarake. Luvut ovat kunkin auton ennustettu arvo.

**Web-palvelun julkaisu**

Voit julkaista regression kokeen verkkopalvelun kyselyjä ja anna sille autojen hinta tekstinsyöttö samalla tavalla kuin kaksi luokan luokitus-käyttötapaus.

![Näkyvissä kokeen autojen hinta regression ongelman pistemäärä](./media/machine-learning-interpret-model-results/13.png)

Kuva 13. Näkyvissä pistemäärä kokeen autojen hinta regression liittyvän ongelman esimerkkitulokset

Käynnissä web-palvelu palautettu tulos muistuttaa kuva 14. Tämä auton ennustettu arvo on $15,085.52.

![Testaa siihen tulosmalli moduuli](./media/machine-learning-interpret-model-results/13_1.png)

![Moduulin tulokset näkyvissä pistemäärä](./media/machine-learning-interpret-model-results/14.png)

Kuva 14. Web-palvelu tulos autojen hinta regression liittyvän ongelman esimerkkitulokset

## <a name="clustering"></a>Klusterointi

**Esimerkki kokeen**

Oletetaan, että Tiina tietojoukon uudelleen luonnissa käytettävien klusteroinnin kokeen. Tässä voit suodattaa luokan otsikot tietojoukon, niin, että vain on ominaisuuksia ja voi käyttää klusterointi. Käytä tätä Tiina palvelupyynnön, määrittää minuuttimäärän, jonka klustereiden on kaksi koulutusta aikana, mikä tarkoittaa, että klusterin kukkien kahteen luokkaan. Kokeen näkyy kuva 15.

![Kokeile Tiina klusteroinnin ongelma](./media/machine-learning-interpret-model-results/15.png)

Kuva 15. Kokeile Tiina klusteroinnin ongelma

Klusterointi eroaa luokittelu, siten, että koulutus tietojoukon ei ole maasta ainoan otsikot yksinään. Klusteroinnin ryhmät koulutus tietojoukon esiintymiä eri klustereihin. Koulutus aikana mallin nimet tapahtumat mukaan Koulujen niiden ominaisuuksien eroista. Tämän jälkeen koulutetun malli voidaan luokitella edelleen tulevia tapahtumia. On kaksi osaa emme sisällä klusteroinnin ongelma kiinnostunut tuloksen. Ensimmäinen osa on otsikoita koulutusta tietojoukon ja toinen on luokittelussa uuden tietojoukon koulutetun mallia.

Tulos alkuosa voidaan näyttää valitsemalla vasemmasta tulosteen portti [junassa klusterointi] mallin[ train-clustering-model] ja valitsemalla sitten **Visualisoi**. Visualisoinnin näkyy kuva 16.

![Klusterointi tulos](./media/machine-learning-interpret-model-results/16.png)

Kuva 16. Visualisoi klusterointi koulutus tietojoukon tulos

Tulos toisen osan klusterointi uusia merkintöjä koulutetun klusteroinnin malli näkyy kuva 17.

![Visualisoi klusterointi tulos](./media/machine-learning-interpret-model-results/17.png)

Kuva 17. Visualisoi klusterointi uuden tietojoukon, tulos

**Tulos tulkitseminen**

Vaikka tulokset kahdesta osasta runko-eri kokeen vaiheet, näy samanlaisina ja tulkitaan samalla tavalla. Neljä ensimmäistä sarakkeet ovat ominaisuuksia. Viimeinen sarake varausten on tekstinsyöttö tulos. Määritetty yhtä monta tapahtumat ovat ennustettujen halutaan olevan samassa klusterissa, eli ne jakaa yhtäläisyydet jollakin tavalla (tämän kokeen käyttää oletusarvon Euclidean etäisyys metrijärjestelmä). Koska olet määrittänyt 2 varausyksiköiden määrän, varaukset merkinnät merkitty 0 tai 1.

**Web-palvelun julkaisu**

Voit julkaista klusteroinnin kokeen verkkopalvelun kyselyjä ja anna sille klusterointia ennusteiden samalla tavalla kuin kaksi luokan luokitus Käyttötapaus varten.

![Näkyvissä kokeen Tiina klusteroinnin ongelman pistemäärä](./media/machine-learning-interpret-model-results/18.png)

Kuva 18. Näkyvissä klusteroinnin Tiina-ongelma kokeen pistemäärä

Kun olet suorittanut web-palvelu, palautettu tulos muistuttaa kuva 19. Tämä kukka on ennustaa olevan klusterin 0.

![Testaa tulkita tulosmalli moduuli](./media/machine-learning-interpret-model-results/18_1.png)

![Moduulin tulos näkyvissä pistemäärä](./media/machine-learning-interpret-model-results/19.png)

Kuva 19. Web-palvelu tulos Tiina kahden luokan luokittelu

## <a name="recommender-system"></a>Suosittelija järjestelmän
**Esimerkki kokeen**

Suosittelija Systemsin, voit käyttää ravintolaan suositus ongelma esimerkkinä: voi suositella ravintolat asiakkaiden luokittelu puheluhistoriaansa perusteella. Syöttötiedot koostuu kolmesta osasta:

* Asiakkaista luokituksen ravintolaan
* Asiakastietojen-ominaisuus
* Ravintolaan ominaisuuksien tiedot

Emme voi käyttää [Junassa Matchbox suosittelija] asioita[ train-matchbox-recommender] Azure koneen Learning moduulissa:

- Ennustaa luokitukset tietyn käyttäjän ja kohde
- Suosittele tietyn käyttäjän kohteet
- Etsi käyttäjiä, jotka liittyvät käyttäjän
- Tietyn kohteen liittyvien kohteiden etsiminen

Voit valita, mitä haluat tehdä valitsemalla neljä **suosittelija tekstinsyöttö laji** -valikon asetuksista. Tässä voit käy läpi kaikki neljä skenaariot.

![Matchbox suosittelija](./media/machine-learning-interpret-model-results/19_1.png)

Tyypillinen Azure koneen Learning kokeen suosittelija järjestelmän näyttää kuvassa 20. Lisätietoja näiden suosittelija järjestelmän moduulit käyttämisestä on artikkelissa [junassa matchbox suosittelija] [ train-matchbox-recommender] ja [tulos matchbox suosittelija][score-matchbox-recommender].

![Suosittelija järjestelmän kokeen](./media/machine-learning-interpret-model-results/20.png)

Kuva 20. Suosittelija järjestelmän kokeen

**Tulos tulkitseminen**

**Ennustaa luokitukset tietyn käyttäjän ja kohde**

Valitsemalla **Luokitus tekstinsyöttö** **suosittelija tekstinsyöttö tyyppi**-kohdassa pyydät suosittelija järjestelmän ennustaa tietyn käyttäjän ja kohteen luokitus. [Tulos Matchbox suosittelija] visualisoinnin[ score-matchbox-recommender] tulos näyttää kuva 21.

![Pisteet tuloksen suosittelija järjestelmän--tekstinsyöttö arviointi](./media/machine-learning-interpret-model-results/21.png)

Kuva 21. Visualisoi suosittelija järjestelmän--arviointi tekstinsyöttö pistemäärän tulos

Kahden ensimmäisen sarakkeen ovat syöttötiedot myöntämä käyttäjän kohteen parit. Kolmas sarake on tiettyjä kohteen käyttäjän budjetoidut luokittelu. Esimerkiksi ensimmäisen rivin asiakkaan U1048 on ennustaa korko ravintolaan 135026, 2.

**Suosittele tietyn käyttäjän kohteet**

Valitsemalla **Kohteen suositus** **suosittelija tekstinsyöttö tyyppi**-kohdassa pyydät suosittelija järjestelmän suositella tietyn käyttäjän kohteet. Valitse tässä skenaariossa viimeinen parametri on *suositus kohteen valinnan*. **Vaihtoehto-luokitellut kohteet (mallin arvioitavaksi)** on ensisijaisesti mallin arvioitavaksi koulutus aikana. Ennakoiva tekstinsyöttö tämä vaihe on valita **Kaikki kohteet**. [Tulos Matchbox suosittelija] visualisoinnin[ score-matchbox-recommender] tulos näyttää kuva 22.

![Pisteet suosittelija järjestelmän--kohteen suositus tulos](./media/machine-learning-interpret-model-results/22.png)

Kuva 22. Tulos tuloksen suosittelija järjestelmän--kohteen suositus visualisointi

Kuusi saraketta ensimmäinen edustaa annetun käyttäjätunnukset suositella kohteita, syöttötiedot mukainen. Viisi saraketta vastaavat kohteet suositeltavia käyttäjän asiayhteyden laskevassa järjestyksessä. Ensimmäisen rivin eniten suositellut ravintolan asiakkaan U1048 on 134986, 135018, 134975, 135021 ja 132862 perään.

**Etsi käyttäjiä, jotka liittyvät käyttäjän**

Valitsemalla **Liittyvät käyttäjät** **suosittelija tekstinsyöttö tyyppi**-kohdassa pyydät suosittelija järjestelmän voit etsiä tietyn käyttäjän liittyvät käyttäjät. Liittyvät käyttäjät ovat käyttäjät, joilla on samanlainen asetukset. Valitse tässä skenaariossa viimeinen parametri on *liittyvät Käyttäjävalinta*. Vaihtoehto **-Käyttäjille, että luokitellut kohteet (mallin arvioitavaksi)** on ensisijaisesti mallin arvioitavaksi koulutus aikana. Valitse **Kaikki käyttäjät** tekstinsyöttö tämä vaihe. [Tulos Matchbox suosittelija] visualisoinnin[ score-matchbox-recommender] tulos näyttää kuva 23.

![Tulos tuloksen suosittelija järjestelmän--liittyvät käyttäjät.](./media/machine-learning-interpret-model-results/23.png)

Kuva 23. Tulos tulokset suosittelija järjestelmän--liittyvät käyttäjät visualisointi

Kuusi saraketta ensimmäinen näyttää tietyn käyttäjän tunnukset tarvitaan etsiminen liittyvät käyttäjät syöttötiedot mukainen. Muut viisi saraketta tallentaa budjetoidut liittyvät käyttäjät käyttäjän laskevassa järjestyksessä osuvuuden mukaan. Ensimmäisen rivin tärkeimpiä asiakkaan asiakkaan U1048 on U1051, U1066, U1044, U1017 ja U1072 perään.

**Tietyn kohteen liittyvien kohteiden etsiminen**

**Liittyvät kohteet** **suosittelija tekstinsyöttö tyyppi**-kohdassa valitsemalla pyydät suosittelija järjestelmän voit etsiä tietyn kohteen Aiheeseen liittyvät kohteet. Liittyvät kohteet ovat todennäköisesti tykännyt sama käyttäjä kohteita. Valitse tässä skenaariossa viimeinen parametri on *liittyvä kohde valinta*. **Vaihtoehto-luokitellut kohteet (mallin arvioitavaksi)** on ensisijaisesti mallin arvioitavaksi koulutusta aikana. On valita **Kaikki kohteet** tekstinsyöttö tämä vaihe. [Tulos Matchbox suosittelija] visualisoinnin[ score-matchbox-recommender] tulos näyttää kuvan 24.

![Tulos tuloksen suosittelija järjestelmän--liittyvät kohteet.](./media/machine-learning-interpret-model-results/24.png)

Kuva 24. Visualisoi tulos tulokset suosittelija järjestelmän--liittyvät kohteet

Kuusi saraketta ensimmäinen edustaa tietyn kohteen tarvitaan liittyvien kohteiden etsiminen syöttötiedot mukainen tunnukset. Muut viisi saraketta tallentaa kohteen budjetoidut liittyvät kohteet laskevaan järjestykseen asiayhteyden kannalta. Ensimmäisen rivin kohteen 135026 tärkeimpiä kohde on esimerkiksi 135074, 135035, 132875, 135055 ja 134992 perään.

**Web-palvelun julkaisu**

Prosessi, jossa nämä kokeissa julkaisemisesta web services hankkiminen ennusteiden eroaa kunkin neljä skenaarioita. Otamme toinen skenaario (tietyn käyttäjän kohteet suositellaan) tässä esimerkkinä. Voit noudattaa muiden kolme samalla tavalla.

Tallentaminen koulutetun suosittelija järjestelmän koulutetun mallin ja yksittäisen käyttäjän tunnussarake syötteen tietojen suodattaminen, kun pyydetty, voit kokeilla, kuten kuvassa 25 yhdistää ja julkaista sen verkkopalvelun.

![Näkyvissä kokeen ravintolaan suositus ongelman pistemäärä](./media/machine-learning-interpret-model-results/25.png)

Kuva 25. Näkyvissä kokeen ravintolaan suositus ongelman pistemäärä

Käynnissä web-palvelu palautettu tulos muistuttaa kuva 26. Viisi suositellut ravintolat käyttäjän U1048 ovat 134986, 135018, 134975, 135021 ja 132862.

![Esimerkki suosittelija järjestelmäpalvelulla](./media/machine-learning-interpret-model-results/25_1.png)

![Kokeen esimerkkitulokset](./media/machine-learning-interpret-model-results/26.png)

Kuva 26. Web-palvelun tulos ravintolaan suositus liittyvän ongelman esimerkkitulokset


<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
