<properties
    pageTitle="Ryhmän tietojen tiede prosessi-ominaisuudet  | Microsoft Azure"
    description="Ryhmän tietojen tiede-prosessi on järjestelmällinen tapa hyödyntää kehittyneen analyysin älykkäät sovellusten kehittämiseen."
    keywords="tietoja tiede prosessin tietojen tiede ryhmät"
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


# <a name="what-is-the-team-data-science-process-tdsp"></a>Mikä on ryhmän tietojen tiede prosessi (TDSP)?

Ryhmän tietojen tiede prosessi (TDSP) on järjestelmällinen lähestymistapa älykkäät sovellusten rakentamiseen, joka mahdollistaa tietojen tutkijoiden voidakseen tehdä yhteistyötä tehokkaasti koko elinkaarta, ota näiden sovellusten tuotteisiin tarvittavat toimenpiteet välityksellä ryhmiä.

Tarkemmin sanottuna TDSP on tällä hetkellä tietojen tiede ryhmät kanssa:

- **Menetelmät**: sen ympärille, joka määrittää tarjoaa ohjeet ongelman määrittäminen, asianmukaisten tietojen analysointia, luominen ja arvioi ennakoivan mallien development Lifecycle-prosessi ja käyttöön kyseiset mallit yrityksen vaiheet järjestyksessä.
- **Resurssien**: työkalut ja -tekniikoiden esimerkiksi tietojen tiede AM yksinkertaistaa ympäristöissä tiede toimille ja lennolle käyttöön uuden teknologian käytännön lisäohjeita on määritetty.

Näin development Lifecycle-prosessi ryhmän tietojen tiede yhteydessä:

![Kaavio: Tietojen tiede prosessi, jossa ryhmät ](./media/data-science-process-overview/data-science-process-for-teams-diagram.png)


Prosessi on **iteratiivinen**: ymmärtämistä, uusille ja nykyisille tai tarkennukset mallin kehitystyö ja edellyttää reworking vaiheet järjestyksessä aiemmin suoritettu. Aiemmin organisaation kehittäminen ja projektin suunnittelemisesta prosessien ovat **helposti mukautettava** toimimaan TDSP määrittämät vaiheet järjestyksessä.

Prosessin vaiheita graafinen esitys ja linkitetyt [TDSP oppimispolku](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) ja seuraavalla tavalla.  


## <a name="planning-and-preparation-steps"></a>Suunnittelu ja valmistelu vaiheet

## <a name="p1-business-and-technology-planning"></a>P1. Liiketoiminnan ja tekniikka suunnittelu

Aloita analytics-projektin tavoitteet ja ongelmia määrittämällä. Ne on määritetty **liiketoiminnan**kannalta. Tässä vaiheessa tavoitteena on tunnistaa tärkeisiin muuttujat (myynnin ennuste tai todennäköisyys tilauksen parhaillaan vilpilliseen, esimerkiksi) analyysi on ennustaa täytä näitä vaatimuksia. Lisää suunnittelu on sitten yleensä olennaiset kehittää ymmärtämistä **tietolähteiden** tarvitaan sähköpostiosoite projektin tavoitteista analytical näkökulmasta. Monimutkaisessa, esimerkiksi löydät, että nykyisten järjestelmien haluat kerätä ja kirjaudu myös muunlaisia tietoja projektin tavoitteet ja korjaa ongelma. Ohjeita on artikkelissa [suunnitteleminen ryhmän tietojen tiede prosessin ympäristön](machine-learning-data-science-plan-your-environment.md) ja [kehittyneen analyysin-Azure koneen Learning tilanteita, joissa](machine-learning-data-science-plan-sample-scenarios.md).  


## <a name="p2-plan-and-prepare-infrastructure"></a>P2. Suunnittelu ja infrastruktuurin valmisteleminen

Ryhmän tietojen tiede prosessin analytics-ympäristössä on useita osia:

- **tietoja työtilat** kohtaa, johon tiedot vaiheistettu ja muokkausta,
- **käsittelyn infrastruktuurin** esikäsittely, tutustuminen ja tietojen mallinnus
- **Runtimen infrastruktuurin** operationalize analytical mallit ja suorita älykkäät asiakassovellukset, tarjoaman mallit.  

Analytics-infrastruktuurin on määritettävä on usein ympäristössä, joka on erillinen core toiminnallisten järjestelmät osa. Mutta hyödyntää yleensä tietoja useista järjestelmistä yrityksen sisällä ja yrityksen ulkopuolisten lähteistä. Analytics-infrastruktuurin voi olla pelkästään pilvipohjainen, tai paikallisen asennuksen tai hybrid kahden. Saat asetukset- [tietojen tiede ympäristöissä käytettäviksi ryhmän tietojen tiede-prosessin määrittäminen](machine-learning-data-science-environment-setup.md).


## <a name="analytics-steps"></a>Analyysin vaiheet:  

## <a name="1-ingest-the-data-into-the-data-platform"></a>1. ingest tietojen tuominen tietojen-ympäristössä

Ensimmäiseksi on tuoda tarvittavat tiedot eri lähteistä, joko sisältä tai yrityksen ulkopuolisten analytics-ympäristössä, jossa tiedot käsitellään. Tietolähteen tiedoissa **muoto** voi vaihdella kohde edellyttämään muotoon. Jotkin muuntamiseen myös niin joutua vaatii nieltynä sillä. Asetukset-kohdassa [tallennustilan ympäristöissä analysoinnissa tietojen lataaminen](machine-learning-data-science-ingest-data.md)

Tietojen alkuperäinen nieltynä tarvitaan monet älykkäät sovellukset tiedot päivitetään säännöllisesti jatkuvaa learning yhteydessä. Tämä voidaan tehdä määrittämällä **tietojen myyntijakso** tai työnkulun. Tämä muodostaa osan prosessi, joka sisältää muodostetaan uudelleen ja arvioimisen uudelleen analytical mallit ratkaisun käyttöönotto älykkäät sovellus käyttää toistuva osa. Katso esimerkiksi [siirtää tietoja SQL Azure Azure Data Factory kanssa, paikallinen SQL-palvelimesta](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-visualize-the-data"></a>2. Tutki ja Visualisoi tietoja

Seuraavaksi voit hankkia tarkempaa tietoja tietojen tutkiminen sen **yhteenvetotiedot**, yhteydet ja tekniikoita käyttämällä esimerkiksi **visualisointi**. Tämä on myös missä **tietojen laadun** ja eheys, kuten puuttuvat arvot tietojen tyypin lähdeluetteloiden tai ristiriitaisia tietosuhteita käsitellään. Valmiiksi käsittely muunnokset käytetään raaka tiedot edelleen analytics ennen Järjestä ja mallinnus voidaan hallita. Kuvaus-kohdassa [tehtävät parannettu tietokoneen tietojen valmisteleminen Koulujen](machine-learning-data-science-prepare-data.md).


## <a name="3-generate-and-select-features"></a>3. Luo ja valitse Ominaisuudet

Tietoja tutkijoiden toimialueen asiantuntijoiden yhteistyössä on tunnistettava ominaisuuksista, jotka siepata tietojoukon tärkeitä ominaisuuksia ja, joka voidaan käyttää parhaiten ennustaa tärkeisiin muuttujat tunnistaa suunnittelun aikana. Seuraavia uusia ominaisuuksia voi johtaa annetuista tiedoista tai saattaa edellyttää kerätään tiedot. Tämä toimenpide on nimeltään **ominaisuus tekniikka** ja on yksi tärkeimmistä vaiheista muodostaminen tehokkaan ennakoivan analytics-järjestelmän. Tämä vaihe edellyttää toimialueen osaamisalueet creative yhdistelmän ja tietojen tarkasteluun vaiheessa saatuja tietoja. Ohjeita on artikkelissa [ominaisuus ryhmän tietojen tiede prosessin tekniikka](machine-learning-data-science-create-features.md).


## <a name="4-create-and-train-machine-learning-models"></a>4. luominen ja kouluttaminen koneen Learning mallit

Tietoja tutkijoiden luominen ennustaa merkittyä määritetyt suunnittelun vaihe tiedoista, jotka on puhdistettava ja featurized business vaatimukset tärkeiden muuttujien analytical mallit. Koneen learning järjestelmät tukevat useita **Mallinnus algoritmeista** , jotka koskevat tapauksissa erilaisia. Ohjeita on artikkelissa [Azure koneen Learning algoritmit valitsemisesta](machine-learning-algorithm-choice.md).

Tietoja tutkijoiden on valittava tekstinsyöttö hoitamiseksi sopivimman mallin, jota ei ole epätavallisten että tulokset useita malleja on voidaan yhdistää Saat parhaat tulokset. Syöttötiedot mallinnus on yleensä jaettu satunnaisesti kolme osaa:

- koulutus-tietojoukko
- vahvistus tietojoukon
- testauksessa tietojoukon

Mallit on suunniteltu **koulutus tietojoukon**käyttäminen. Paras mahdollinen yhdistelmä (virittää parametreilla) on valittuna suorittamalla mallit ja mittaaminen **vahvistus tietojoukon**tekstinsyöttö virheet. Lopuksi **testata tietojoukon** käytetään haluat laskea valitun mallin riippumaton tietoihin, jota käytettiin ei kouluttaminen tai Vahvista mallin suorituskykyä.  Lisätietoja on artikkelissa [Azure koneen Learning mallin suorituskyvyn arvioimiseen](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-the-models-in-the-product"></a>5. käyttöönotto ja tarjoaman tuotteen malleista

Kun on joukko malleja, jotka suorittavat hyvin, ne voidaan muiden sovellusten tarjoaman **operationalized** . Liiketoiminta-vaatimukset mukaan ennusteiden tehdään **reaaliaikaisesti** tai **erä** välein. Operationalized-mallien on tarjoamia kanssa **Avaa API-käyttöliittymä** , joka on helppo kulutettu eri sovelluksista tällaisten online-sivuston, laskentataulukoita, raporttinäkymien tai business ja Taustajärjestelmä sovellusten rivi. Katso [käyttöönotto Azure koneen Learning web-palveluun](machine-learning-publish-a-machine-learning-web-service.md).


## <a name="summary-and-next-steps"></a>Yhteenveto- ja seuraavat vaiheet

[Ryhmän tietojen tiede prosessi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/) on mallintaa iteroitava vaiheet järjestyksessä, **esitellään** tehtävien suorittamiseen kehittyneen analyysin avulla voit muodostaa älykkäät sovelluksen. Jokaisen vaiheen sekä tiedot siitä, miten voit käyttää eri Microsoft-tekniikoiden kuvataan tehtävien suorittamiseen.

Vaikka TDSP määrätä tietyntyyppiset **dokumentaatio** palvelutiedot, on parasta asiakirjaan tietojen tarkasteluun, mallinnus ja arvioinnin tulokset ja tallentaa esityksen koodia, siten, että analyysi on iteroitava tarvittaessa. Näin uudelleenkäyttöä analytics-työstä myös, kun muut sovellukset samanlaisia tietoja ja ennakoiva tekstinsyöttö tehtävien parissa.

Täysi lopusta loppuun vaihe vaiheelta, jotka osoittavat vaiheiden suorittamisesta prosessi, jossa **tietyissä skenaarioissa** myös toimitetaan. Ne on lueteltu [ryhmän tietojen tiede prosessin vaiheittaiset ohjeet](data-science-process-walkthroughs.md) artikkelissa pikkukuvan kuvaukset.
