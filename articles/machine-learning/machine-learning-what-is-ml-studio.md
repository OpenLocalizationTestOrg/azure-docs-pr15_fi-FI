<properties 
    pageTitle="Mikä on Azure koneen Learning Studio? | Microsoft Azure"
    description="Vedä ja pudota-työkalun etsimisen nopeasti mallien avulla valmis kirjastossa algoritmit ja moduulit Azure ML Studion yleiskatsaus."
    keywords="Azure konepohjaisten oppimistekniikoiden, azure ml ml studio"
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
    ms.topic="get-started-article"
    ms.date="09/09/2016"
    ms.author="garye"/>

# <a name="what-is-azure-machine-learning-studio"></a>Mikä on Azure koneen Learning Studio?

Microsoft Azure koneen Learning Studio on voit tehdä muodosta, Testaa ja ota käyttöön ennakoivan analytics ratkaisujen tietojen yhteiskäytön, vedä ja pudota-työkalu. Koneen Learning Studio julkaisee mallit, voit helposti kulutettu mukautettujen sovellusten tai Liiketoimintatieto-työkaluista, kuten Excel web-palveluiden.

Koneen Learning Studio on joka tietojen tiede, ennakoivan analytics, cloud resurssit ja tietojen leikkauspiste.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Koneen Learning Studio vuorovaikutteinen työtila

Kehittää ennakoivan analysis-mallin, voit tavallisesti käyttää tietoja vähintään yksi lähteistä, muunna analysoida tietoja eri tietojen käsittelemistä ja tilastolliset funktiot ja Luo joukko tulokset. Kehittäminen tältä malli on toistuva prosessi. Kun muokkaat haluamasi Funktiot ja niiden parametrit-tulokset todennäköisyyden, kunnes olet tyytyväinen että koulutettuja ja tehokas malli.

**Azure koneen Learning Studio** tutustutaan interaktiivisia ja visuaalisia työtilan helposti muodosta, Testaa ja käytöstä ennakoivan analysis-malli. Voit vetäminen ja pudottaminen ***tietojoukkoja*** ja analyysi ***Moduulit*** vuorovaikutteinen piirtoalusta, niiden yhdessä muodostamiseksi ***kokeilla***, jonka voit käynnistää tietokoneen Learning Studiossa yhdistäminen sivulle. Käytöstä mallin rakenteeseen, voit muokata koe, voit tallentaa kopion halutessasi ja suorita se uudelleen. Kun olet valmis, että ***koulutus kokeilla*** muuntaminen ***ennakoivan kokeen***ja julkaista sen ***Verkkopalvelu*** niin, että muut niitä voi käyttää mallissa.

>[AZURE.TIP] Voit ladata ja tulostaa kaavion, jossa on yleiskuvaus koneen Learning Studio ominaisuuksia, katso [yleiskuvauskaavio Azure koneen Learning Studio ominaisuuksia](machine-learning-studio-overview-diagram.md).

Ei ole ohjelmoinnin määrittäminen pakolliseksi, yhteyden vain visuaalisesti tietojoukkoja ja moduulit muodostaa ennakoivan analysis-malli.

![Azure ML Studio kaavion: Luo kokeet, Lue tietoja useista lähteistä, tulos tietojen kirjoittaminen, kirjoita mallit.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Koneen Learning Studio käytön aloittaminen

Kun kirjoitat [Koneen Learning Studio](https://studio.azureml.net) näet **kotisivulla** . Tässä Näytä asiakirjat, videoita, verkkoseminaarit ja etsi muita tärkeitä resursseja.

Yläreunassa on kolme välilehteä: **Home** (jossa käynnistät), **Studio**ja **valikoima**.

### <a name="studio"></a>Studio

Valitse **Studio** -välilehti ja sinua pyydetään kirjautumaan Microsoft-tili tai työpaikan tai oppilaitoksen tilillä. Kun olet kirjautunut sisään, näkyviin tulee seuraavat välilehdet vasemman reunan:

- **PROJEKTIT** - tutkimuksiin, tietojoukkoja, muistikirjat ja muita resursseja, joka edustaa yhden projektin
- **Kokeissa** - kokeissa luodun, suorittaa ja tallennettuja luonnoksia
- **VERKKOPALVELUT** - verkkopalvelut, jotka olet ottanut oman kokeissa
- **MUISTIKIRJAT** - Jupyter muistikirjat, jotka olet luonut
- **TIETOJOUKKOJA** - tietojoukkoja, Studio lataamasi
- **KOULUTETUN mallit** - malleja, jotka olet koulutus kokeissa ja tallentanut Studiossa
- **Asetukset** - ryhmä, jonka avulla voit määrittää tilin ja resursseja.

### <a name="gallery"></a>Valikoima

Valitse **valikoima** -välilehti ja sinun otettava Cortana Liiketoimintatieto-valikoimaan. Valikoimassa on paikka, johon yhteisön tietojen tutkijoiden ja kehittäjät jakaa ratkaisuja, jotka on luotu käyttämällä osia Cortana liiketoimintatietojen.

Saat lisätietoja valikoiman [Jaa ja tutustu ratkaisuihin Cortana Liiketoimintatieto-valikoiman](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Kokeen osat

Kokeen koostuu tietojoukkoja tietoja tuovien analytical moduulit, joka yhdistää muodostaa ennakoivan analysis-malli. Tarkemmin sanottuna kelvollinen kokeen on seuraavia ominaisuuksia:

- Kokeen on vähintään yksi tietojoukko ja yksi moduuli
- Tietojoukkoja voidaan yhdistää vain moduulit
- Moduulit voidaan yhdistää tietojoukkoja tai muut moduulit
- Kaikki syötteen portit moduuleissa on oltava joitakin tiedonkulun yhteys
- Kaikki tarvittavat kunkin moduulin parametrit on määritettävä

Voit luoda kokeen tyhjästä tai voit käyttää aiemmin otoksen kokeen mallina. Lisätietoja on artikkelissa [Käytä otoksen kokeissa voit luoda uuden kokeissa](machine-learning-sample-experiments.md).

Esimerkki yksinkertainen kokeen luomisesta on artikkelissa [luominen Yksinkertainen kokeen Azure koneen Learning Studiossa](machine-learning-create-experiment.md).

Katso lisätietoja vaiheista ennakoivan analytics-ratkaisun luominen, [kehittäminen ennakoivan ratkaisussa, jossa Azure koneen Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Tietojoukkoja

Tietojoukko on tietoja, jotka on ladattu tietokoneen Learning Studio niin, että se voidaan mallinnus prosessin. Esimerkki tietojoukkoja useita kuuluvat koneen Learning Studio voit kokeilla ja voit ladata useita tietojoukkoja, kun tarvitset niitä. Seuraavassa on joitakin esimerkkejä mukana tietojoukkoja:

- **Eri autot MPG tiedot** – Mailia kohti gallona (MPG) arvot autot merkittyä osaa sylinteriä, tehoa jne.
- **Rinnassa syövän tiedot** – rinnassa syövän vianmäärityksen tiedot.
- **Metsää käynnistyy tiedot** – metsää fire koot koillisen osaston Portugalin.

Kun muodostat kokeessa, voit valita tietojoukkoja käytettävissä olevista vasemmalle puolelle alusta.

Esimerkki tietojoukkoja koneen Learning Studio sisältyy luetteloon Tutustu [Azure koneen Learning Studiossa tietojoukkojen otosten](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Moduulit

Moduuli on algoritmin, joita voit suorittaa tietojen. Koneen Learning Studio on väliltä tietojen tunkeutumisen Funktiot koulutus, näkyvissä pistemäärä ja kelpoisuuden prosesseja moduulit määrä. Seuraavassa on esimerkkejä mukana moduulit:

- [Muuntaa ARFF] [ convert-to-arff] -muuntaa muuntaa sarjaksi .NET-tietojoukko määrite suhde tiedoston muoto (ARFF) avulla.
- [Laske peruskoulun tilasto] [ elementary-statistics] -laskee peruskoulun tilastotiedot, kuten keskiarvo, keskihajonta jne.
- [Lineaarisen regressiosuoran] [ linear-regression] – Luo online liukuvärin laskeutumisnopeus perustuva muuttujan lineaarinen regressio-malli.
- [Mallin sija] [ score-model] -pisteyttää koulutetun luokitus tai regression malli.

Kun muodostat kokeessa, voit valita luettelo moduuleista käytettävissä vasemmalla puolella alusta.  

Moduulin voi olla joukko parametreja, joiden avulla voit määrittää moduulin sisäinen algoritmeista. Kun valitset kangasta moduuli-moduulin parametrit näkyvät **Ominaisuudet** -ruudun oikealla puolella alusta. Voit muokata ruudun säätämällä mallin parametrit.

Jotkin ohjeen muotoisessa suuri kirjasto tietokoneen learning algoritmeista käytettävissä, artikkelissa [valitseminen käyttöön Microsoft Azure koneen liittyviä algoritmit](machine-learning-algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Ennakoivan analytics verkkopalvelun käyttöönotto

Kun ennakoivan analytics-malli on valmis, voit asentaa sen koneen Learning Studio suoraan web-palvelu. Lisätietoja Tämä toimenpide on artikkelissa [Azure koneen Learning WWW-palvelun käyttöönotto](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
