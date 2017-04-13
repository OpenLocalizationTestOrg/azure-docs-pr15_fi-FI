<properties
    pageTitle="Miten Machine Learning mallin siirrytään koe-operationalized WWW-palvelun | Microsoft Azure"
    description="Yleiskatsaus, miten Azure koneen Learning malli-etenee-kehitys kokeilla operationalized WWW-palvelun suunnittelu."
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
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>


# <a name="how-a-machine-learning-model-progresses-from-an-experiment-to-an-operationalized-web-service"></a>Miten Machine Learning mallin siirrytään koe-operationalized Web-palvelu

Azure koneen Learning Studio on vuorovaikutteinen alusta, jonka avulla voit kehittää, suorittaminen, Testaa ja käytöstä ***Kokeile*** edustava ennakoivan analysis-malli. On erilaisia moduulit käytettävissä, voit:

* Oman kokeen syötteen tiedot
* Tietojen muokkaamiseen
* Mallin käyttäminen koneen learning algoritmit kouluttaminen
* Mallin pisteet
* Tulosten laskeminen
* Lopullinen arvot

Kun olet tehnyt oman koe, voit ottaa käyttöön sen ***perinteinen Azure koneen Learning WWW-palvelun*** tai ***Uusi Azure koneen Learning WWW-palvelun*** niin, että käyttäjät voivat lähettää uudet tiedot ja takaisin tulokset.

Tässä artikkelissa on antavat yleiskuvan, miten Machine Learning malli-etenee-kehitys kokeilla operationalized WWW-palvelun suunnittelu.

>[AZURE.NOTE] Useilla eri tavoilla kehittää ja otetaan käyttöön tietokoneen learning mallit, mutta tässä artikkelissa on kohdistettu koneen Koulujen Studio käyttämisestä. Lisätietoja luomisesta perinteinen ennakoivan verkkopalvelun r on seuraavassa blogimerkinnässä [Muodosta ja ota käyttöön ennakoivan Web Apps käyttämällä RStudio ja Azure ML](http://blogs.technet.com/b/machinelearning/archive/2015/09/25/build-and-deploy-a-predictive-web-app-using-rstudio-and-azure-ml.aspx).

Kun Azure koneen Learning Studio on suunniteltu helpottamaan voit kehittää ja ottaa käyttöön *ennakoivan analysis-mallin*, on mahdollista Studiolla kehittää kokeessa, jossa ei ole ennakoivan analysis-malli. Kokeen voi esimerkiksi vain syötetietoja, muokata sitä ja Näytä tulokset. Ennakoivan analyysin-koe, kuten voit ottaa tämän – ennakoivan kokeen Web-palvelu, mutta se on yksinkertaisempi prosessin, koska kokeen ei ole koulutus tai näkyvissä pistemäärä koneen learning mallin. Ei ole tyypillinen Studiolla tällä tavalla, kun Microsoft lisätä sen keskustelun niin, että voimme antaa täydellinen selitys Studio toiminnasta.

## <a name="developing-and-deploying-a-predictive-web-service"></a>Kehittämiseen ja käyttöönottoon ennakoivan Web-palvelu

Seuraavat vaiheet, jotka tyypillinen ratkaista seuraavasti, kun kehittää ja ottaa käyttöön sen koneen Learning Studiossa:

![Käyttöönotto-työnkulku](media\machine-learning-model-progression-experiment-to-web-service\model-stages-from-experiment-to-web-service.png)

*Kuva 1 - jaksojen tyypilliset ennakoivan analysis-malli*

### <a name="the-training-experiment"></a>Koulutus kokeen

WWW-palvelun koneen Learning Studiossa kehittäminen alkuvaiheen ***koulutus kokeilla*** on. Koulutus kokeen tarkoituksena on avulla voit kehittää, Testaa, käytöstä ja kouluttaminen myöhemmin konepohjaisten oppimistekniikoiden mallin sijainti. Voit voi myös kouluttaminen useita malleja samanaikaisesti, kun etsit paras mahdollinen ratkaisu, mutta kun olet valmis lataamista voit valita yhden koulutus malli ja siten poistaa kokeen kuin muiden. Esimerkki kehittäminen ennakoivan analyysi kokeilla on artikkelissa [kehittäminen luottotietojen risk Assessment-ohjelman-Azure koneen Learning ennakoivan analytics-ratkaisua](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="the-predictive-experiment"></a>Ennakoivan kokeen

Kun koulutetun mallin koulutus kokeessa, **Määrittää WWW-palvelun** ja valitse **Ennakoivan verkkopalvelun** koneen Learning Studiossa voit aloittaa koulutus kokeen muuntaminen ***ennakoivan kokeen***. Ennakoivan kokeen tarkoituksena on sija uudet tiedot, jonka tavoitteena on tulossa myöhemmin operationalized Azure-Web-palvelu koulutetun mallin avulla.

Tämä muunnos on valmiiksi seuraavien vaiheiden läpi:

-   Muuntaa moduulit koulutus yksittäisen moduuliin käytettäviä määrittäminen ja tallenna se koulutetun mallina

-   Poista ylimääräiset moduulit ei liity näkyvissä pistemäärä

-   Lisää syöttö- ja portit, joka käyttää potentiaalisen Web-palvelu

Voi olla myös muita muutoksia, jotka haluat tehdä käyttäjien ennakoivan kokeen kuin WWW-palvelun käyttöön. Jos haluat siirtää vain osaa tulokset Web-palvelu, voit lisätä output portti ennen suodattamista moduuli.

Muuntaminen tässä prosessissa koulutus koe hylätään ei. Kun prosessi on valmis, sinulla on kaksi välilehteä Studiossa: yksi koulutus kokeessa ja yksi ennakoivan kokeen osalta. Näin voit tehdä koulutusta kokeen muutoksia, ennen kuin WWW-palvelun käyttöönotto ja muodostaa ennakoivan kokeen. Tai voit tallentaa kopion koulutus kokeen voit aloittaa vuorovaikutteisuudesta toisen rivin.

>[AZURE.NOTE] Kun napsautat **Ennakoivan verkkopalvelun** käynnistät automaattista prosessia koulutus kokeen muuntaminen ennakoivan kokeen ja tämä toimii hyvin useimmissa tapauksissa. Jos koulutusta kokeen on monimutkainen (esimerkiksi sinulla on useita polkuja koulutusta, kun liityt yhdessä), kannattaa ehkä tehdä tämä muuntaminen manuaalisesti. Lisätietoja on artikkelissa [tietokoneen Learning koulutus-kokeen ennakoivan koe, muunna](machine-learning-convert-training-experiment-to-scoring-experiment.md).

### <a name="the-web-service"></a>Web-palvelu

Kun olet tyytyväinen ennakoivan kokeen on valmis, voit ottaa käyttöön joko perinteinen WWW-palvelulle palvelun tai uusi verkkopalvelun perusteella Azure resurssien hallinta. Mallin operationalize ottamalla käyttöön *perinteinen Learning Machine-verkkopalvelun*, valitse **WWW-palvelun käyttöön** ja valitse **Ota käyttöön Web-palvelu [[perinteinen]]**. Ottaa käyttöön *Uuden tietokoneen Learning-Web*-palvelu, **WWW-palvelun käyttöön** ja valitse **Ota käyttöön Web-palvelu [uusi]**. Käyttäjät voivat nyt tietojen lähettäminen Web-palveluun REST API mallin ja vastaanottaa takaisin tulokset. Jos haluat lisätietoja, katso, [miten tarjoaman, jotka on otettu käyttöön Konepohjaisten Oppimistekniikoiden kokeen Azure kone Learning Web-palvelu](machine-learning-consume-web-services.md).

## <a name="the-non-typical-case-creating-a-non-predictive-web-service"></a>– Normaali kirjainkoko: luominen – ennakoivan Web-palvelu

Jos yhteyttä kokeen ei kouluttaminen ennakoivan analysis-mallin, sinun ei tarvitse luoda koulutus kokeessa ja tulosmalli kokeen - on vain yksi koe ja otat käyttöön sen verkkopalvelun. Koneen Learning Studio tunnistaa, sisältääkö oman kokeen ennakoivan mallin mukaan analysoiminen moduulit ovat käytössä.

Kun olet iteroitava oman koe- ja sitä täyttyvät:

1.  **Määrittää WWW-palvelun** ja valitse **Uudelleenkoulutusta verkkopalvelu** - syöttö- ja solmujen lisätään automaattisesti

2.  Valitse **Suorita**

3. **WWW-palvelun käyttöön** ja valitse **Käyttöönotto verkkopalvelun [perinteinen]** tai **Ottaa käyttöön Web-palveluun [uusi]** , johon haluat ottaa käyttöön käyttöympäristösi mukaan.

Web-palvelu on nyt otettu käyttöön, ja voit käyttää ja hallita sitä tavoin ennakoivan verkkopalvelun.

## <a name="updating-your-web-service"></a>WWW-palvelun päivittäminen

Nyt kun olet käyttöön oman kokeen kuin Web-palveluun, jos haluat päivittää sen?

Sinun on päivitettävä, joka määräytyy:

**Haluat muuttaa syötteitä tai tulosteita tai haluat muuttaa, miten WWW-palvelun käsittelevää tiedot**

Jos haluat vaihtaa ei mallin, mutta vain muuttaa WWW-palvelun tapa käsitellä tietoja, voit Muokkaa ennakoivan kokeen ja sitten **käyttöönotto Web** -palvelu ja valitse **Käyttöönotto verkkopalvelun [perinteinen]** tai **Ottaa käyttöön Web-palvelu [uusi]** uudelleen. Web-palvelu on pysäytetty päivitetyt ennakoivan kokeen on otettu käyttöön ja Web-palvelu on käynnistettävä uudelleen.

Esimerkki: oletetaan, että ennakoivan kokeen palauttaa syöttötiedot budjetoidut tuloksella koko rivi. Voit päättää, jonka haluat palauttaa juuri tulos Web-palveluun. Näin voit lisätä **Projektin sarakkeiden** moduuli ennakoivan kokeessa oikealle output-portti tuloksen kuin sarakkeiden jättäminen pois ennen. Kun napsautat **WWW-palvelun käyttöön** ja valitse **Käyttöönotto verkkopalvelun [perinteisessä]** tai **Ottaa käyttöön Web-palvelu [uusi]** uudelleen, Web-palvelu on päivitetty.

**Haluat suuntaamista mallin uusilla tiedoilla**

Jos haluat säilyttää oman konepohjaisten oppimistekniikoiden mallin, mutta haluat suuntaamista uusilla tiedoilla, käytettävissä on kaksi vaihtoehtoa:

1.  **Mallin Web-palvelu on käynnissä suuntaamista** – voit halutessasi suuntaamista aikana ennakoivan mallin Web-palvelu on käynnissä, voit tehdä tämän tekemällä koulutus kokeilla, jotta se ***uudelleenkoulutuksen kokeen***on muutamia muutoksia ja valitse voi ottaa ** *uudelleenkoulutuksen web* -palveluun**. Katso ohjeet siitä, miten voit tehdä tämän [tietokoneen Learning suuntaamista mallit ohjelmallisesti](machine-learning-retrain-models-programmatically.md).

2.  **Palaa alkuperäiseen koulutus kokeessa ja käytä eri koulutustietoja kehittää mallin** - ennakoivan kokeen on linkitetty Web-palvelu, mutta koulutus kokeen ei ole linkitetty suoraan tällä tavalla. Jos teet muutoksia alkuperäisen koulutus kokeessa ja **määrittää Web**-palvelu, se luodaan *Uusi* ennakoivan kokeilla, kun käyttöön, luo *Uusi* Web-palveluun. Se ei vain Päivitä alkuperäinen Web-palveluun.

    Jos haluat muokata koulutus koe, avaa se ja valitse **Tallenna nimellä** kopioida. Tämä jättää ennalleen alkuperäisen koulutus kokeen ennakoivan kokeessa ja WWW-palvelun. Voit nyt luoda uuden verkkopalvelun tekemilläsi muutoksilla. Kun olet ottanut käyttöön uuden Web-palvelun sen jälkeen päätät, Edellinen WWW-palvelun pysäyttäminen vai säilyttää sen käynnissä uuteen rinnalla.

**Haluat eri mallia kouluttaminen**

Jos haluat tehdä muutoksia oman alkuperäisen ennakoivan koe, kuten valitsemalla toiseen tietokoneeseen learning-algoritmin kokeilujakson eri koulutus menetelmä, jne. ja valitse sinun tarvitse noudattaa toinen olevien ohjeiden uudelleenkoulutusta mallin: Avaa koulutus kokeen, valitse **Tallenna nimellä** , kopioida, ja kirjoita uusi polku kehittäminen mallin alaspäin , ennakoivan kokeen luominen ja käyttöönotto WWW-palvelun. Tämä luo uuden sivuston palvelun liittymättömiä alkuperäisen tekemääsi – voit päättää, yhtä tai molempia pitäminen käynnissä.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja kehittämisen ja kokeilla prosessin on seuraavissa artikkeleissa:

-   koe - [koneen Learning-koulutusta kokeen ennakoivan kokeen voit muuntaa](machine-learning-convert-training-experiment-to-scoring-experiment.md) muuntaminen

-   Verkkopalvelu - [käyttöönotto Azure koneen Learning WWW-palvelun](machine-learning-publish-a-machine-learning-web-service.md) käyttöönotto

-   uudelleenkoulutusta malli - [suuntaamista koneen Learning mallit ohjelmallisesti](machine-learning-retrain-models-programmatically.md)

Koko prosessin esimerkkejä on seuraavissa artikkeleissa:

-   [Koneen learning opetusohjelma: Luo ensimmäinen kokeen Azure Konepohjaisten Oppimistekniikoiden Studiossa](machine-learning-create-experiment.md)

-   [Vaiheittainen kuvaus: Kehittää luottotietojen risk Assessment-ohjelman-Azure koneen Learning ennakoivan analytics-ratkaisua](machine-learning-walkthrough-develop-predictive-solution.md)