<properties
    pageTitle="Tekstin analytics-mallien luominen Azure koneen Learning Studio | Microsoft Azure"
    description="Luominen tekstin analytics-mallien Azure koneen Learning Studiossa käyttämällä moduulit tekstiä esikäsittely, N g tai ominaisuus hajautuksessa"
    services="machine-learning"
    documentationCenter=""
    authors="rastala"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="roastala" />


#<a name="create-text-analytics-models-in-azure-machine-learning-studio"></a>Tekstin analytics-mallien luominen Azure koneen Learning Studio

Voit käyttää Azure koneen Learning voivat laatia ja operationalize tekstin analytics mallit. Näitä malleja voi auttaa ratkaiseminen, esimerkiksi tiedoston luokitus tai markkinatunnelma analyysi.

Tekstin analytics-kokeessa olisi yleensä:

 1. Puhdista ja Esikäsittele tekstin tietojoukko
 2. Numeerinen ominaisuus vektorit poimiminen valmiiksi käsitellyt teksti
 3. Junassa luokitus tai regressio malli
 4. Pisteet ja mallin vahvistaminen
 5. Ota malli käyttöön tuotannon

Tässä opetusohjelmassa kerrotaan seuraavasti, kun selkeät markkinatunnelma analyysi mallin avulla Amazon kirjan tarkistukset tietojoukko (katso tämän Oheistiedot-raportti "perustajajäsenten tiedot, Bollywood, ylävaunua-ruutuihin ja Blenders: toimialueen mukauttamista Markkinatunnelma luokitus" John Blitzer, merkitse Dredze ja Fernando Pereira; Liitto laskennallinen Lingvistinen (Käyttöoikeusluettelon) 2007: n.) Tämä tietojoukko koostuu tarkistuksen tulosten (1 – 2 tai 4-5) ja vapaa-teksti. Tarkista tulokset ennustetaan on: pieni (1 - 2) tai suuri (4-5).

Voit tarkistaa tarkoitettujen Tässä opetusohjelmassa osoitteessa Cortana liiketoimintatietojen valikoima kokeiden:

[Ennustaa kirjan tarkistukset] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[Ennustaa kirjan tarkistukset - ennakoivan kokeen] (https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Vaihe 1: Puhdistaminen ja Esikäsittele tekstin tietojoukko

Olemme alkaa kokeen jakamalla tarkistuksen tulosten yhdeksi categorical alin ja ylin uudelleenkäytettävät muodostetaan ongelma kuin kaksi luokan luokitus. Käytämme [Muokkaa metatietojen] (https://msdn.microsoft.com/library/azure/dn905986.aspx) ja moduulit [ryhmän Categorical Values] (https://msdn.microsoft.com/library/azure/dn906014.aspx).

![Otsikon luominen](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

Olemme sitten Puhdista [Esikäsittele teksti] (https://msdn.microsoft.com/library/azure/mt762915.aspx)-moduulin teksti. Vähentämiseksi tietojoukossa ohjeen Etsi tärkeimmistä ominaisuuksista ja lopullinen mallin tarkkuuden parantamiseksi puhdistus pienentää. Microsoft poistaa stopwords - yleisiä sanoja, kuten "," tai "a" - ja numerot, erikoismerkkejä, kaksoiskappaleet merkkejä, sähköpostiosoitteet ja URL-osoitteet. On myös muuntaa tekstin pienet, lemmatize sanat ja tunnistaa virkkeen rajat, jotka on merkitty sitten "|||" valmiiksi käsitellyt tekstin symboli.

![Esikäsittele teksti](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

Jos haluat käyttää stopwords mukautettu luettelo? Voit siirtää sen valinnainen syötteenä. Voit myös mukautettuja C# syntaksi säännöllisen lausekkeen avulla voit korvata alimerkkijonoja ja poistaa sanoja sanaluokka: substantiivit tai käyttö adjektiivien.

Esikäsittely päätyttyä, emme tiedot jaetaan junassa ja testaa joukot.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Vaihe 2: Poimi numeerinen ominaisuus vektorit valmiiksi käsitellyt teksti

Voit luoda mallin tekstitiedot, yleensä on vapaamuotoiset tekstin muuntaminen numeerinen ominaisuus vektorit. Tässä esimerkissä Käytämme [Pura N g ominaisuudet tekstistä] (https://msdn.microsoft.com/library/azure/mt762916.aspx)-moduulissa, voit muuntaa tekstitiedot muodossa. Tämä moduuli käsittelee sarakkeen tekstin erotettu sanojen ja laskee sanaston sanojen tai sanat, jotka näkyvät oman tietojoukko N g. Tämän jälkeen se laskee laskee, kuinka monta kertaa kukin sana tai N g kunkin tietueen tulee näkyviin, ja luo ominaisuus vektorit niistä. Tässä opetusohjelmassa on Määritä N g koko 2, joten ominaisuus Microsoftin vektorit yksittäistä sanaa ja myöhempiä sanat yhdistelmät.

![Pura N g](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

Emme Käytä TF * IDF (termin korkojakso käänteisen asiakirjan taajuus) painotus N g laskee. Tämän menetelmän Lisää sanat, joita vastaavan tietueen usein näkyvät ei harvinaisissa yli koko tietojoukko leveyden. Muut vaihtoehdot ovat binaarinen, TF- ja kaavion painavat.

Tekstin näiden ominaisuuksien on yleensä hyvin dimensionaalisuus. Esimerkiksi jos että corpus on 100 000 yksilöllinen sanat, space-ominaisuus on 100 000 mitat tai Lisää N g käytettäessä. Pura N g ominaisuudet-moduulin avulla voit vähentää dimensionaalisuus vaihtoehdoista. Voit jättää sanoja, joilla on lyhyt tai pitkä, tai liian usein tai liian usein on ennakoivan lisäarvoa. Tässä opetusohjelmassa on Jätä N-g, jotka näkyvät vähemmän kuin 5 tietueet tai yli 80 % tietueita.

Voit myös valita vain ominaisuuksia, jotka eniten Korreloidun tekstinsyöttö kohteelle ominaisuudet. Käytämme chi-neliön ominaisuudet valitsemalla 1000 ominaisuuksia. Voit tarkastella valitun sanojen tai N g sanasto valitsemalla Pura N-g-moduulin oikea tulos.

Vaihtoehtoinen menetelmä voit poimia N g-ominaisuuksilla voit käyttää ominaisuutta hajautuksessa moduuli. Huomaa, vaikka kyseistä [ominaisuus hajautuksessa] (https://msdn.microsoft.com/library/azure/dn906018.aspx) ei ole valmiiden ominaisuus valinnan ominaisuuksia tai TF * IDF painavat.

## <a name="step-3-train-classification-or-regression-model"></a>Vaihe 3: Kouluttaminen luokitus tai regression malli

Teksti on nyt ole muunnettu numeerinen ominaisuus sarakkeet. Tietojoukon on edelleen edellisessä vaiheessa merkkijonon sarakkeita, joten Käytämme Valitse sarakkeet-tietojoukko poista ne.

Käytämme [kahden luokan logistista regressiota] (https://msdn.microsoft.com/library/azure/dn905994.aspx) ennustetaan Microsoftin kohde: suureksi tai pieneksi Tarkista tulos. Tässä vaiheessa tekstin analytics-ongelma on ohjelmassa säännöllisesti luokitus ongelma. Voit parantaa mallin Azure koneen Learning käytettävät työkalut. Voit esimerkiksi kokeilla eri valitsimen nähdäksesi, kuinka tarkkoja ne Anna tuloksia tai käyttää hyperparameter säätäminen tarkkuuden parantamiseksi.

![Junassa ja pisteet](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Vaihe 4: Pisteet ja mallin vahvistaminen

Miten koulutetun mallin tarkistaa? Olemme sija vastaan testi-tietojoukko ja arvioi tarkkuutta. Kuitenkin mallin asiat N g ja niiden leveydet koulutus dataset-sanasto. Vuoksi on pitäisi käyttää kyseisen sanaston ja näiden paksuuksia ominaisuuksia purkaminen testitiedot verrattuna luominen sanasto uuden. Vuoksi olemme Pura N g ominaisuudet-moduulin lisääminen tulosmalli haaran kokeen, Yhdistä tulosteen sanaston koulutus päätasolta ja määritä sanasto-tilassa vain luku-tilaan. On myös poistaa suodattamisen N g mukaan, kuinka usein määrittämällä vähintään 1 esiintymän ja enintään 100 %, ja ominaisuudet käytöstä.

Kun testitiedot teksti-sarakkeessa on ole muunnettu numeerinen ominaisuus sarakkeet, emme jättäminen pois edellisen vaiheet, koulutus haaran merkkijono-sarakkeet. Käytämme sitten pistemäärän mallin moduulin ennusteiden tekemiseen ja arvioi mallin moduulin arvioida tarkkuutta.

## <a name="step-5-deploy-the-model-to-production"></a>Vaihe 5: Ottaa mallin käyttöön tuotannon

Malli on melkein valmis tuotannon käyttöön. Kun käyttöön web-palvelu, se tulee vapaamuotoiset tekstimerkkijonon syötteeksi, ja palauttaa tekstinsyöttö "suuri" tai "pieni." Tiedostossa käytetään kokemuksia N g sanaston muuntaa tekstin ominaisuudet ja koulutettua logistista regressiota, jota haluat tehdä tekstinsyöttö-ominaisuuksia. 

Määrittämään ennakoivan kokeilla Microsoft tallentamista kuin tietojoukko N g-sanasto ja koulutettua logistista regressiota malli koulutus kokeen päätasolta. Emme Tallenna sitten kokeen Tallenna nimellä-toiminnolla "ennakoivan kokeen koe kaavion luomiseen. Olemme Poista jaetut tiedot-moduulin ja koulutus-haara kokeen. Valitse muodostetaan aiemmin tallennetut N g sanasto ja mallin Pura N g-ominaisuuksien ja tulos mallin moduulit tarpeen mukaan. On myös poistaa arvioi malli-moduuli.

Olemme Lisää sarakkeiden valitseminen tietojoukko-moduulissa, ennen kuin voit poistaa otsikko-sarakkeessa moduulin Esikäsittele teksti ja valitsee pistemäärän moduulissa "Liitä pistemäärän sarake DataSet"-vaihtoehto. Sen mukaan, web-palvelu ei pyytää, se yrittää ennustaa otsikko ja käytettäessä ei päivitä syötteen ominaisuudet vastaus.

![Ennakoivan kokeen](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

Nyt on koe, joka voidaan julkaista web-palvelu ja kutsua pyynnön vastauksen tai erä suorittamisen API.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja tekstin analytics moduuleja [MSDN dokumentaatio] (https://msdn.microsoft.com/library/azure/dn905886.aspx).
