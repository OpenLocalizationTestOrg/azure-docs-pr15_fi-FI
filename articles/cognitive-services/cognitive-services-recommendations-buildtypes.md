<properties
    pageTitle="Muodosta tyypit ja mallin laatu: suosituksia API | Microsoft Azure"
    description="Azure koneen--Learning suositukset-pikaopas"
    services="cognitive-services"
    documentationCenter=""
    authors="luiscabrer"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="luisca"/>

#  <a name="build-types-and-model-quality"></a>Muodosta tyypit ja mallin laatu #

<a name="TypeofBuilds"></a>
## <a name="supported-build-types"></a>Tuetut muodosta tyypit ##

Suositukset-Ohjelmointirajapinnan tukee tällä hetkellä muodosta kahdenlaisia: *suositus* ja *muuttaa FBT-Maksuerien*. Kunkin on luotu käyttämällä eri algoritmit ja jokaisella on eri vahvuuksien. Tämän asiakirjan kuvattu nämä versiot ja tekniikat vertailu mallit luodaan laatuun.

Jos et ole vielä niin, on suositeltavaa, että suoritat [pikaopas](cognitive-services-recommendations-quick-start.md).

<a name="RecommendationBuild"></a>
### <a name="recommendation-build-type"></a>Suositus muodosta tyyppi ###

Suositus muodosta tyyppi käyttää matriisin factorization suosituksia. Luo [piilevän ominaisuus](https://en.wikipedia.org/wiki/Latent_variable) vektorit liiketapahtumiasi kuvataan kunkin kohteen perusteella ja vertailla kohteita, jotka muistuttavat näiden piilevän vektorit avulla.

Jos electronics säilöön tehdyt ostot perustuvan mallin kouluttaminen ja anna Lumia 650 puhelimella syötteeksi mallin, mallin palauttaa joukon kohteet, jotka yleensä ovat maksullisia henkilöt, jotka todennäköisesti ostaa Lumia 650 puhelimella. Kohteet eivät välttämättä ole komplementtivirhefunktion. Tässä esimerkissä on mahdollista, että mallin palauttavat toisiin puhelimiin, koska henkilöt, jotka, kuten Lumia 650 saattaa toisiin puhelimiin, kuten.

Suositus Luo on kaksi ominaisuuksia, jotka helpottavat kiinnostavassa:

* *Suositus Luo tukee *kylmän kohteen* sijainnin **

Kohteet, joita ei ole merkittäviä käyttö kutsutaan kylmät kohteet. Esimerkiksi jos saat toimituksen koskaan myyty ennen puhelimen, järjestelmä ei voi johtaa tämän tuotteen, kun yksinään tapahtumien suosituksia. Tämä tarkoittaa, että järjestelmä kannattaa tutustua tietojen itse tuote.

Jos haluat käyttää kylmät kohteen sijaintia, tarvitset ominaisuuksia tietoja kunkin luettelon kohteita. Seuraavassa on luettelo muutaman ensimmäisen rivin saattaa millaiseksi (Huomautus avain = arvon muoto-ominaisuudet).

>Pro2 6CX 00001-, pinta-, pinta, kirjoita = laitteisto-tallennustilan = 128 Gigatavun muistia = 4G, valmistajan = Microsoft

>73H-00013, aktivointi Xbox 360 Gaming,, tyyppi =-ohjelmiston kieltä = englanti, arviointi = kehittynyt

>WAH 0F05, Minecraft Xbox 360, Gaming, * tyyppi =-ohjelmiston kieltä = espanja, arviointi = nuoriso

On myös seuraavat muodosta parametrien määrittäminen:

| Luo parametri         | Huomautuksia
|------------------     |-----------
|*useFeaturesInModel*     | Aseta arvoksi **true**.  Ilmaisee, jos ominaisuuksien avulla voidaan parantaa suositus mallia.
|*allowColdItemPlacement*   | Aseta arvoksi **true**. Ilmaisee, jos suositus pitäisi myös push kylmän kohteiden ominaisuus samanlaiset kautta.
| *modelingFeatureList*   | CSV-ominaisuuden nimiä, jotka voidaan parantaa suositus suositus Luo luettelo. Esimerkiksi "kieli, tallennustilan" edellisessä esimerkissä varten.

**Suositus Luo tukee käyttäjän suositukset**

Suositus muodosta tukee [käyttäjän suosituksia](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/56f30d77eda5650db055a3dd). Tämä tarkoittaa, että antaa käyttäjille heidän tapahtumahistorioilla perusteella mukautettuja suosituksia. Käyttäjän suositukset voit antaa käyttäjän tai tapahtumien kyseisen käyttäjän historiatietojen.

Yksi perinteinen esimerkki, jossa saatat haluta käyttää käyttäjän suosituksia on kirjauduttaessa sisään aloitussivulla. Voit muuntaa olemassa sisältöä, joka koskee tietyn käyttäjän.

Voit myös haluta käyttää suosituksia muodosta tyyppi, kun käyttäjä on Kuittaa ulos. Tässä vaiheessa sinulla on käyttäjä on hankkiminen kohteiden luettelo ja voit antaa suosituksia nykyisen market kori perusteella.

#### <a name="recommendations-build-parameters"></a>Suosituksia luominen parametrit

| Nimi  |   Kuvaus |    Kirjoita, <br>  Kelvollisia arvoja <br> (oletusarvo)
|-------|-------------------|------------------
| *NumberOfModelIterations* |   Mallin suorittaa Iteraatioita numero näkyy Laske aikaa ja mallin tarkkuutta. Mitä suurempi luku, sitä tarkempi mallin, mutta siihen suorittaminen kestää kauemmin.  |   Kokonaisluku, <br>  10 – 50 <br>(40)
| *NumberOfModelDimensions* |   Dimensioiden määrää liittyvät ominaisuudet mallin yrittää etsiä tietojen määrä. Lisääntyvien dimensioiden määrää Salli paremmin tarkka määrittäminen tulosten pienempiä klustereihin. Kuitenkin liikaa mitat estää mallin etsiminen korrelaatioita kohteet. |  Kokonaisluku, <br> 10-40 <br>(20) |
| *ItemCutOffLowerBound* |  Määrittää vähimmäismäärä käyttö asioista kohteen pitäisi olla sen pidetään osa mallia. |        Kokonaisluku, <br> vähintään 2 <br> (2) |
| *ItemCutOffUpperBound* |  Määrittää käyttö pisteiden kohteen pitäisi olla sen pidetään osa mallia. |  Kokonaisluku, <br>vähintään 2<br> (2147483647) |
|*UserCutOffLowerBound* |   Määrittää käyttäjä on tehnyt pidetään mallin osa tapahtumien vähimmäismäärä. | Kokonaisluku, <br> vähintään 2 <br> (2)
| *UserCutOffUpperBound* |  Määrittää käyttäjän on suoritettava pidetään mallin osa tapahtumien enimmäismäärä. | Kokonaisluku, <br>vähintään 2 <br> (2147483647)|
| *UseFeaturesInModel* |    Ilmaisee, jos ominaisuuksien avulla voidaan parantaa suositus mallia. |     Totuusarvo<br> Oletus: TOSI
|*ModelingFeatureList* |    CSV-ominaisuuden nimiä, jotka voidaan parantaa suositus suositus Luo luettelo. Odotusaika riippuu, jotka ovat tärkeitä ominaisuuksia. |    Merkkijono, enintään 512 merkkiä
| *AllowColdItemPlacement* |    Ilmaisee, jos suositus pitäisi myös push kylmät kohteiden ominaisuus samanlaiset kautta. | Totuusarvo <br> Oletus: EPÄTOSI
| *EnableFeatureCorrelation*    | Ilmaisee, jos ominaisuuksia voi käyttää päättelypeli. | Totuusarvo <br> Oletus: EPÄTOSI
| *ReasoningFeatureList* |  Pilkuilla erotettu luettelo nimien, jota käytetään perustelut lauseiden, kuten suositus selitykset. Odotusaika riippuu ominaisuuksia, jotka ovat tärkeitä asiakkaille. | Merkkijono, enintään 512 merkkiä
| *EnableU2I* | Ottaa käyttöön mukautetun suositukset, kutsutaan myös käyttäjän kohde (U2I) suosituksia. | Totuusarvo <br>Oletus: TOSI
|*EnableModelingInsights* | Määrittää, onko offline-tilassa arvioinnin suorittaa kerätä tietoja mallinnus (eli tarkkuutta ja moninaisuuden arvot). Jos arvo on tosi-osan tiedoista ei käytetä, koulutus, koska se on varataan testaus mallia. Lisätietoja [offline-tilassa arvioinnit](#OfflineEvaluation). | Totuusarvo <br> Oletus: EPÄTOSI
| *SplitterStrategy* | Ota mallinnus tiedot on määritetty *Tosi*, jos tämä on kuinka tietoja voidaan jakaa arviointia varten.  | Merkkijono, *RandomSplitter* tai *LastEventSplitter* <br>Oletus: RandomSplitter


<a name="FBTBuild"></a>
### <a name="fbt-build-type"></a>Muuttaa FBT-Maksuerien muodosta tyyppi ###

Usein ostetut yhdessä (muuttaa FBT-Maksuerien) luo tekee analyysi, joka laskee, kuinka monta kertaa kahden tai kolmen eri tuotteiden yhtä ilmetä yhdessä. Valitse lajittelee joukot samanlaiset-funktioon (**työtovereiden esiintymät**, **Jaccard**, **Nosta**) perusteella.

Ajattele **Jaccard** ja **Nosta** tapoja normalisointi työtovereiden esiintymät.  Tämä tarkoittaa, että kohteet palautetaan vain, jos ne Jos hankittu lähde-kohteen kanssa.

Tässä esimerkissä Lumia 650-puhelin puhelin X palautetaan vain, jos puhelin X on ostettu kuin Lumia 650 puhelimen samassa istunnossa. Koska tämä saattaa olla epätodennäköistä, on todennäköisesti odottanut kohteiden komplementtivirhefunktion Lumia 650 palautetaan; esimerkiksi näytön suojaus tai Lumia 650 sovittimen.

Tällä hetkellä kaksi kohdetta oletetaan voi ostaa saman istunnon aikana, jos ne sijaitsevat tapahtuman saman Käyttäjätunnuksellasi ja aikaleiman.

Muuttaa FBT-Maksuerien versiot eivät tue kylmän kohteita, koska määritelmän odotuksia kaksi kohteet ovat maksullisia saman tapahtuman. Kun muuttaa FBT-Maksuerien versiot voi palauttaa kohdejoukkojen (kolmikkoja), ne eivät tue mukautettuja suositukset, koska ne Hyväksy yhteen lähde-kohteen syötteenä.


#### <a name="fbt-build-parameters"></a>Muuttaa FBT-Maksuerien muodosta parametrit

| Nimi  |   Kuvaus |       Kirjoita,  <br> Kelvollisia arvoja <br> (oletusarvo)
|-------|---------------|-----------------------
| *FbtSupportThreshold* | Miten perinteinen malli on. Muiden kohteiden pidetään mallinnus esiintymien lukumäärän. |  Kokonaisluku, <br> 3 – 50 <br> (6)
| *FbtMaxItemSetSize* | Bounds usein käytetyt joukon kohteiden määrän.| Kokonaisluku  <br> 2-3 <br> (2)
| *FbtMinimalScore* | Mahdollisimman vähän pisteet, jotka sisällytetään palautettujen tulosten pitäisi olla usein käytetyt määrittäminen. Mitä suurempi parempi. | Kaksinkertainen <br> 0 ja yllä <br> (0)
| *FbtSimilarityFunction* | Määrittää käytettävän Luo samanlaiset-funktiota. **Nosta** favors serendipity **muiden esiintymä** favors ennustettavuutta ja **Jaccard** on kompromissi välillä. | Merkkijono,  <br>  <i>cooccurrence, hissin jaccard</i><br> Oletusarvo: <i>jaccard</i>

<a name="SelectBuild"></a>
## <a name="build-evaluation-and-selection"></a>Muodosta arviointi ja valinta ##

Nämä ohjeet ehkä avulla voit määrittää, onko käytettävä suosituksia muodosta tai muuttaa FBT-Maksuerien muodosta, mutta se ei tarjoa lopullinen vastaus tilanteissa, joissa voi käyttää jommankumman. Myös, vaikka tietäisit, jota haluat käyttää muuttaa FBT-Maksuerien-muodosta tyyppi, voi edelleen haluat valita **Jaccard** tai beetajakaumafunktiona samanlaiset **Nosta** .

Paras tapa valitsemalla kaksi eri versiot välillä on Testaa ne käytännön (online arviointi) ja seurata muuntaminen korvaus varten eri versiot. Muuntaminen korko voidaan mitata mukaan suositus napsautuksella, Todellinen numero ostaa näkyy suositukset, tai jopa todellinen summat eri suositukset on osoitettu. Voit valita oman muuntaminen korko metrijärjestelmä business tavoitteen perusteella.

Haluat ehkä arvioi mallin offline-tilassa, ennen kuin lisäät sen tuotannon. Kun offline-tilassa arviointi ei korvaa online arvioitavaksi, se voi toimia mittarin.

<a name="OfflineEvaluation"></a>
## <a name="offline-evaluation"></a>Offline-tilassa arviointi  ##

Offline-tilassa arvioinnin lähinnä ennustaa tarkkuus (käyttäjät, jotka ostaa sen suositellut kohteiden määrä) ja moninaisuuden suosituksia (kohteita, jotka on suositeltavaa määrä).
Osana tarkkuutta ja moninaisuuden arvot laskennan järjestelmä etsii käyttäjien otoksen ja jakaa tapahtumat kyseisille käyttäjille kahteen ryhmään: koulutus-tietojoukko ja testaa tietojoukko.

> [AZURE.NOTE]Käyttämään offline-tilassa arvot käyttötietojen on oltava aikaleimat.
> Voit jakaa käyttö oikein koulutus ja testaa tietojoukkojen väliset tarvitaan aikatietoja.

> Myös offline-tilassa arvioinnin saattaa ei tuota tuloksia pieni käyttö tiedostoille. Laskennan on perusteellisesti on oltava vähintään 1 000 testi-tietojoukko käyttö asioista.

<a name="Precision"></a>
### <a name="precision-at-k"></a>Tarkkuus on k ###
Seuraavassa taulukossa on tarkkuus on k offline-tilassa laskennan tulos.

| K | 1 | 2 | 3 |   4 |     5
|---|---|---|---|---|---|
|Prosentti |   13,75 | 18.04   | 21 |  24.31 | 26.61
|Testi-käyttäjät |    10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Katsoa käyttäjiä | 10 000 |    10 000 |    10 000 |    10 000 |    10 000
|Käyttäjien ei oteta huomioon | 0 | 0 | 0 | 0 | 0

#### <a name="k"></a>K
Edellisessä taulukossa *k* edustaa suositukset, kuten asiakkaan määrä. Taulukon seuraavasti: "Jos vain yksi suositus näkyy aikana testi-asiakkaille, vain 13,75 käyttäjien ostanut tämä suositus." Tämä lause perustuu olettaen, että malli on koulutus hankkiminen tiedoilla. Toinen tapa komento siitä, että 1 tarkkuus on 13,75.

Huomaat, että kuin enemmän kohteita näkyvät asiakkaalle, ostamalla suositeltuja kohteen asiakkaan todennäköisyys siirtyy. Edellisessä kokeen osalta todennäköisyys lähes kaksinkertaistuu 26.61 prosenttiin kun 5 kohteet on suositeltavaa.

#### <a name="percentage"></a>Prosentti
Käyttäjät, joilla on vähintään yksi *k* suositukset projektitietoja prosentteina on näkyvissä. Jakamalla käyttäjät, joilla on vähintään yksi suositus projektitietoja katsoa käyttäjiä kokonaismäärän mukaan määrä lasketaan prosentti. Katso lisätietoja katsoa käyttäjiä.

#### <a name="users-in-test"></a>Testi-käyttäjät
Tietojen rivi edustaa testi-tietojoukko käyttäjien määrä.

#### <a name="users-considered"></a>Katsoa käyttäjiä
Käyttäjä katsotaan vain, jos järjestelmä suositellaan vähintään *k* kohteiden perusteella luotu koulutus-tietojoukko mallia.

#### <a name="users-not-considered"></a>Käyttäjien ei oteta huomioon
Tietojen rivi edustaa sellaisten käyttäjien ei oteta huomioon. Käyttäjät, joita ei mennyt vähintään *k* suositellut kohteet.

Käyttäjä ei oteta huomioon = käyttäjät testin – käyttäjät pidettäviä

<a name="Diversity"></a>
### <a name="diversity"></a>Moninaisuuden ###
Moninaisuuden arvot mittaa suositellut kohteiden lajin. Seuraavassa taulukossa on moninaisuuden offline-tilassa laskennan tulos.

|Prosenttipiste aikajakson |    0 90|  90 – 99| 99-100
|------------------|--------|-------|---------
|Prosentti        | 34.258 | 55.127| 10.615


Yhteensä suositellut kohteet: 100,000

Suositellut yksilöllistä kohdetta: 954

#### <a name="percentile-buckets"></a>Prosenttipiste uudelleenkäytettävät
Prosenttipiste kunkin aikajakson edustaa aikaväli (vähimmäis- ja arvot, jotka väliltä 0 – 100 alue). Sulje 100 kohteet ovat Suosituimmat kohteet ja 0 lähellä kohteet ovat vähintään Suositut. Esimerkiksi jos 99 100 prosenttipiste aikajakson prosenttiarvo on 10.6, se tarkoittaa, että 10.6 prosenttia suosituksia palauttaa vain yhden ylimmät prosenttia Suosituimmat kohteet. Prosenttipiste aikajakson pienimmän arvon sisältyy ja suurin arvo on yksityinen, lukuun ottamatta 100.
#### <a name="unique-items-recommended"></a>Suositellut yksilöllistä kohdetta
Suositellut metrijärjestelmä yksilöllistä kohdetta näkyy eri, jotka on palautettu arvioitavaksi kohteiden määrän.
#### <a name="total-items-recommended"></a>Suositellut kohteiden määrä
Kohteiden määrä suositella lisätiedot näyttää suositellut kohteiden määrän. Jotkin ehkä kaksoiskappaleiden.

<a name="ImplementingEvaluation"></a>
### <a name="offline-evaluation-metrics"></a>Offline-tilassa arvioinnin arvot ###
Tarkkuus ja moninaisuuden offline-tilassa arvot voi olla hyödyllinen, voit valita mitkä muodosta käyttämään. Muodosta milloin osana vastaaviin muuttaa FBT-Maksuerien tai suositus muodostaa parametrit seuraavasti:

-   Määritä *enableModelingInsights* muodosta-parametrin arvoksi **true**.
-   Vaihtoehtoisesti voit valita *splitterStrategy* (joko *RandomSplitter* tai *LastEventSplitter*).
*RandomSplitter* jakaa junassa käyttötietojen ja testaa määrittää tietyn *randomSplitterParameters* testi prosentin mukaan ja satunnainen arvojen Määritä.
*LastEventSplitter* jakaa junassa käyttötietojen ja testaa määrittää kullekin käyttäjälle viimeisen tapahtuman perusteella.

Tämä käynnistää muodosta, jotka käyttävät vain osan tiedoista koulutus ja muiden tietojen laskemiseen arvioinnin arvot.  Luo jälkeen saat arvioinnin tulosteen haluat kutsua [Get muodosta arvot API](https://westus.dev.cognitive.microsoft.com/docs/services/Recommendations.V4.0/operations/577eaa75eda565095421666f)-kulkeva vastaaviin *modelId* ja *buildId*.

 Seuraavassa on esimerkki arvioitavaksi JSON-tuloste.


    {
     "Result": {
     "precisionItemRecommend": null,
     "precisionUserRecommend": {
      "precisionMetrics": [
        {
          "k": 1,
          "percentage": 13.75,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 2,
          "percentage": 18.04,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 3,
          "percentage": 21.0,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 4,
          "percentage": 24.31,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        },
        {
          "k": 5,
          "percentage": 26.61,
          "usersInTest": 10000,
          "usersConsidered": 10000,
          "usersNotConsidered": 0
        }
      ],
      "error": null
    },
    "diversityItemRecommend": null,
    "diversityUserRecommend": {
      "percentileBuckets": [
        {
          "min": 0,
          "max": 90,
          "percentage": 34.258
        },
        {
          "min": 90,
          "max": 99,
          "percentage": 55.127
        },
        {
          "min": 99,
          "max": 100,
          "percentage": 10.615
        }
      ],
      "totalItemsRecommended": 100000,
      "uniqueItemsRecommended": 954,
      "uniqueItemsInTrainSet": null,
      "error": null
      }
     },
    "Id": 1,
    "Exception": null,
    "Status": 5,
    "IsCanceled": false,
    "IsCompleted": true,
    "CreationOptions": 0,
    "AsyncState": null,
    "IsFaulted": false
    }
