<properties 
    pageTitle="Ryhmän tietojen tiede prosessi-ominaisuudet  | Microsoft Azure" 
    description="Ryhmän tietojen tiede-prosessi on järjestelmällinen tapa hyödyntää kehittyneen analyysin älykkäät sovellusten kehittämiseen." 
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

[Ryhmän tietojen tiede prosessi (TDSP)](data-science-process-overview.md) on järjestelmällinen lähestymistapa älykkäät sovellusten rakentamiseen, joka mahdollistaa tietojen tutkijoiden voidakseen tehdä yhteistyötä tehokkaasti koko elinkaarta, ota näiden sovellusten tuotteisiin tarvittavat toimenpiteet välityksellä ryhmiä. TDSP erittelee vaiheet, jotka tarjoavat **ohjeita** siitä, miten voit määrittää ongelman työkalujen määrittäminen ja ympäristön tarvitaan asianmukaisten tietojen analysointia, luominen ja arvioi ennakoivan mallit ja käyttöön kyseiset mallit yrityksen järjestyksessä. 

Tässä on **Ryhmän tietojen tiede prosessin**vaiheita:  

![PÄÄ-työnkulku](./media/machine-learning-data-science-the-cortana-analytics-process/CAP-workflow.png)

Prosessi on **iteratiivinen**: ymmärtämistä, uusille ja nykyisille tai tarkennukset mallin kehitystyö ja edellyttää reworking vaiheet järjestyksessä aiemmin suoritettu. Aiemmin organisaation kehittäminen ja projektin suunnittelemisesta prosessien ovat **helposti mukautettava** toimimaan TDSP määrittämät vaiheet järjestyksessä. 

Prosessin vaiheita graafinen esitys ja linkitetyt [TDSP oppimispolku](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) ja seuraavalla tavalla.  

## <a name="preparation-steps"></a>Valmistavia vaiheita 

## <a name="p1-plan-the-analytics-project"></a>P1. Analytics projektin suunnittelu 

Aloita analytics-projektin tavoitteet ja ongelmia määrittämällä. Ne on määritetty **liiketoiminnan**kannalta. Tässä vaiheessa tavoitteena on tunnistaa tärkeisiin muuttujat (myynnin ennuste tai todennäköisyys tilauksen parhaillaan vilpilliseen, esimerkiksi) analyysi on ennustetaan täytä näitä vaatimuksia. Lisää suunnittelu on sitten tavallisesti olennaiset kehittää ymmärtämistä **tietolähteiden** tarvitaan sähköpostiosoite projektin tavoitteista analytical näkökulmasta. Monimutkaisessa, esimerkiksi löydät, että nykyisten järjestelmien haluat kerätä ja kirjaudu myös muunlaisia tietoja projektin tavoitteet ja korjaa ongelma. Ohjeita on artikkelissa [suunnitteleminen ryhmän tietojen tiede prosessin ympäristön](machine-learning-data-science-plan-your-environment.md) ja [kehittyneen analyysin-Azure koneen Learning tilanteita, joissa](machine-learning-data-science-plan-sample-scenarios.md).  

## <a name="p2-setup-analytics-environment"></a>P2. Analytics-ympäristön määritys 

Ryhmän tietojen tiede prosessin analytics-ympäristössä on useita osia: 

- **tietoja työtilat** kohtaa, johon tiedot vaiheistettu ja muokkausta, 
- **käsittelyn infrastruktuurin** esikäsittely, tutustuminen ja tietojen mallinnus
- **Runtimen infrastruktuurin** operationalize analytical mallit ja suorita älykkäät asiakassovellukset, tarjoaman mallit.  

Analytics-infrastruktuuria, joka on oltava määritys on usein ympäristössä, joka on erillinen core toiminnallisten järjestelmät osa. Mutta hyödyntää yleensä tietoja useista järjestelmistä yrityksen sisällä ja yrityksen ulkopuolisten lähteistä. Analytics-infrastruktuurin voi olla pelkästään pilvipohjainen, tai paikallisen asennuksen tai hybrid kahden. Saat asetukset- [tietojen tiede ympäristöissä käytettäviksi ryhmän tietojen tiede-prosessin määrittäminen](machine-learning-data-science-environment-setup.md).

## <a name="analytics-steps"></a>Analyysin vaiheet:  

## <a name="1-ingest-data-into-the-analytical-environment"></a>1. ingest tietojen analytical-ympäristöön 

Ensimmäiseksi on tuoda tarvittavat tiedot kohtaa, johon tiedot voidaan käsitellä analyyttisten ympäristöissä eri lähteistä, joko sisältä tai yrityksen ulkopuolella. **Muotoile** lähde tiedoissa voi vaihdella kohde edellyttämään muotoon. Jotkin muuntamiseen myös niin joutua vaatii nieltynä sillä. Asetukset-kohdassa [tallennustilan ympäristöissä analysoinnissa tietojen lataaminen](machine-learning-data-science-ingest-data.md)

Tietojen alkuperäinen nieltynä tarvitaan monet älykkäät sovellukset tiedot päivitetään säännöllisesti jatkuvaa learning yhteydessä. Tämä voidaan tehdä määrittämällä **tietojen myyntijakso** tai työnkulun. Tämä muodostaa osan prosessi, joka sisältää muodostetaan uudelleen ja arvioimisen uudelleen analytical mallit ratkaisun käyttöönotto älykkäät sovellus käyttää toistuva osa. Katso esimerkiksi [siirtää tietoja SQL Azure Azure Data Factory kanssa, paikallinen SQL-palvelimesta](machine-learning-data-science-move-sql-azure-adf.md).


## <a name="2-explore-and-pre-process-data"></a>2. tarkastella ja käsitellä valmiiksi tiedot 

Seuraavaksi voit hankkia tarkempaa tietoja tietojen tutkiminen sen **yhteenvetotiedot** , yhteydet ja tekniikoita käyttämällä esimerkiksi **visualisointi**. Tämä on myös missä **tietojen laadun** ja eheys, kuten puuttuvat arvot tietojen tyypin lähdeluetteloiden tai ristiriitaisia tietosuhteita käsitellään. Valmiiksi käsittely muunnokset käytetään raaka tiedot edelleen analytics ennen Järjestä ja mallinnus voidaan hallita. Kuvaus-kohdassa [tehtävät parannettu tietokoneen tietojen valmisteleminen Koulujen](machine-learning-data-science-prepare-data.md).


## <a name="3-develop-features"></a>3. kehittää ominaisuudet 

Tietoja tutkijoiden toimialueen asiantuntijoiden yhteistyössä on tunnistettava ominaisuuksista, jotka siepata tietojoukon tärkeitä ominaisuuksia ja, joka voidaan käyttää parhaiten ennustetaan tärkeisiin muuttujat tunnistaa suunnittelun aikana. Seuraavia uusia ominaisuuksia voi johtaa annetuista tiedoista tai saattaa edellyttää kerätään tiedot. Tämä toimenpide on nimeltään **ominaisuus tekniikka** ja on yksi tärkeimmistä vaiheista muodostamisessa tehokkaan ennakoivan analytics-järjestelmän. Tämä vaihe edellyttää toimialueen osaamisalueet creative yhdistelmän ja tietojen tarkasteluun vaiheessa saatuja tietoja. Ohjeita on artikkelissa [ominaisuus ryhmän tietojen tiede prosessin tekniikka](machine-learning-data-science-create-features.md).


## <a name="4-create-predictive-models"></a>4. ennakoivan mallien luominen 

Tietoja tutkijoiden luominen ennustaa merkittyä määritetyt suunnittelun vaihe tiedoista, jotka on puhdistettava ja featurized business vaatimukset tärkeiden muuttujien analytical mallit. Koneen learning järjestelmät tukevat useita **Mallinnus algoritmeista** , jotka koskevat tapauksissa erilaisia. Ohjeita on artikkelissa [valitseminen ryhmän Azure koneen Learning algoritmeista](machine-learning-algorithm-choice.md).

Tietoja tutkijoiden on valittava tekstinsyöttö hoitamiseksi sopivimman mallin, jota ei ole epätavallisten että tulokset useita malleja on voidaan yhdistää Saat parhaat tulokset. Syöttötiedot mallinnus yleensä on jaettu satunnaisesti kolme osaa:

- koulutus-tietojoukko 
- vahvistus tietojoukon 
- testauksessa tietojoukon 

Mallit on suunniteltu **koulutus tietojoukon**käyttäminen. Paras mahdollinen yhdistelmä (virittää parametreilla) on valittuna suorittamalla mallit ja mittaaminen **vahvistus tietojoukon**tekstinsyöttö virheet. Lopuksi **testata tietojoukon** käytetään haluat laskea valitun mallin riippumaton tietoihin, jota käytettiin ei kouluttaminen tai Vahvista mallin suorituskykyä.  Lisätietoja on artikkelissa [Azure koneen Learning mallin suorituskyvyn arvioimiseen](machine-learning-evaluate-model-performance.md).


## <a name="5-deploy-and-consume-models"></a>5. ottaminen käyttöön ja käyttää malleja 

Kun on joukko malleja, jotka suorittavat hyvin, ne voidaan muiden sovellusten tarjoaman **operationalized** . Liiketoiminta-vaatimukset mukaan ennusteiden tehdään **reaaliajassa** tai **erä** välein. Operationalized-mallien on tarjoamia kanssa **Avaa API-käyttöliittymä** , joka on helppo kulutettu eri sovelluksista tällaisten online-sivuston, laskentataulukoita, raporttinäkymien tai business ja Taustajärjestelmä sovellusten rivi. Katso [käyttöönotto Azure koneen Learning web-palveluun](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="summary-and-next-steps"></a>Yhteenveto- ja seuraavat vaiheet

[Ryhmän tietojen tiede prosessi](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) on mallintaa iteroitava vaiheet järjestyksessä, **esitellään** tehtävien suorittamiseen kehittyneen analyysin avulla voit luoda älykkäät sovelluksia. Jokaisen vaiheen sekä tiedot siitä, miten voit käyttää eri Microsoft-tekniikoiden kuvataan tehtävien suorittamiseen. 

Kun TDSP määrätä tietyntyyppiset **dokumentaatio** palvelutiedot, on paras käytäntö asiakirjan tietojen tarkasteluun, mallinnus ja arvioinnin tulokset ja tallentaa esityksen koodia, siten, että analyysin voivat iteroitava tarvittaessa. Näin uudelleenkäyttöä analytics-työstä myös, kun muut sovellukset samanlaisia tietoja ja ennakoiva tekstinsyöttö tehtävien parissa.

Täysi lopusta loppuun vaihe vaiheelta, jotka osoittavat vaiheiden suorittamisesta prosessi, jossa **tietyissä skenaarioissa** myös toimitetaan. Katso, esimerkiksi:

- [Ryhmän tietojen tiede prosessin käytössä: SQL Serverin avulla](machine-learning-data-science-process-sql-walkthrough.md)
- [Ryhmän-tiede prosessin käytössä: käyttämällä HDInsight Hadoop klustereiden](machine-learning-data-science-process-hive-walkthrough.md).
- [Tietoja tiede ohjattu käyttäminen Azure HD.mdnsight](machine-learning-data-science-spark-overview.md)
- [Skaalattavissa tietojen tiede-Azure tietojen Lake: lopusta loppuun-ongelmatilanteita](machine-learning-data-science-process-data-lake-walkthrough.md)

 
