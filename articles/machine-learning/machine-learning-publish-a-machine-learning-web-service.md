<properties
    pageTitle="Machine Learning verkkopalvelun käyttöönotto | Microsoftin Azure"
    description="Miten muuntaa koulutuksen kokeen ehkäisevän kokeen, valmistella sen käyttöönottoa ja sitten käyttäjäpohjaista Azure Machine Learning web-palveluun."
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

# <a name="deploy-an-azure-machine-learning-web-service"></a>Azure-Machine Learning-web-palvelun käyttöönotto

Azure Machine Learning avulla voit rakentaa, testata ja ottaa käyttöön ennakoivia analyyttiset ratkaisut.

-Korkean tason point-of-view tämä tapahtuu kolmessa vaiheessa:

- **[Luo koulutus koe]** - Azure Machine Learning Studio on yhteistyössä visual kehitysympäristö, jonka avulla junan ja testata ehkäisevän analytics-mallissa koulutuksen tiedot, jotka annat.
- **[Muunna ehkäisevän koe]** - kerran malli on ole harjoitettu vanhoilla tiedoilla ja olet valmis avulla sija uutta tietoa, valmistella ja tehostaa oman koe laskentamenetelmää.
- **Sen web-palvelun käyttöönotto** - voit ottaa yhteyttä ehkäisevän koe [uuden] tai [perinteisen] Azure WWW-palvelulle. Käyttäjät voivat lähettää tietoja malliin ja saada oman malleilla.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Luo koulutus-koe

Junaan ehkäisevän analytics-mallin avulla Azure Machine Learning Studio Luo koulutus kokeen johon sisällytät eri moduulien lataaminen koulutus, valmistella tarvittavat tiedot, machine learning algoritmeja ja arvioida tuloksia. Voit käydä koe- ja kokeile eri machine learning algoritmit vertailla ja arvioida tuloksia.

Prosessi luoda ja hallita koulutuksen kokeiden käsitellään tarkemmin toisaalla. Lisätietoja on seuraavissa artikkeleissa:

- [Luoda yksinkertaisen kokeen Azure Machine Learning Studio](machine-learning-create-experiment.md)
- [Kehittää ehkäisevän ratkaisua, jossa Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)
- [Koulutuksen tiedot tuodaan Azure Machine Learning Studio](machine-learning-data-science-import-data.md)
- [Koe toistoja Azure Machine Learning Studio hallinta](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Koulutuksen kokeen muuntaminen ehkäisevän koe

Kun olet harjoitettu malliin, olet valmis muuntaa koulutus-kokeen ehkäisevän kokeen yhteinäinen uusia tietoja.

Muuntamalla ehkäisevän kokeen valmistaudut koulutetun mallin käyttöönottoa tulosmalli WWW-palvelulle. Web-palvelun käyttäjät voivat lähettää mallin syöttötietojen ja malli lähetetään takaisin ennakoinnin tuloksia. Kun muunnat ehkäisevän koe, muista, miten oletat muiden käyttämä malli.

Muunnetaan ehkäisevän kokeen koulutus-kokeeseen **suorittaa** kokeen piirtoalustan alaosasta, **määrittää Web**-palvelu ja valitse **Ehkäisevän Web-palveluun**.

![Pisteytys kokeen muuntaminen](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Saat lisätietoja muuntamiseen, [Machine Learning koulutus-kokeeseen, ehkäisevän kokeeseen voi muuntaa](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Seuraavissa vaiheissa kuvataan, otetaan käyttöön ennakoivia kokeen WWW-palveluna. Voit myös ottaa kokeen kuin perinteistä web-palveluun.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Ottaa käyttöön ennakoivia kokeen WWW-palveluna

Nyt kun ehkäisevän kokeeseen on valmisteltu, voit ottaa Azure WWW-palveluna. Web-palvelun avulla käyttäjät voivat lähettää tiedot malliin ja mallin palauttavat sen laskentamenetelmää.

Valitse ottamaan oman ehkäisevän kokeen **suorittaa** kokeen piirtoalustan alaosasta. Kun koe on suoritettu, **Web** -palvelu käyttöön ja **Web-palvelun käyttöön** [Uusi].  Portaalin Machine Learning verkkopalvelun käyttöönotto-sivu avautuu.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Koneen oppiminen Web Service portal käyttöönotto koe sivu

Kokeen käyttöönotto sivulla Anna Internet-palvelun nimi.
Valitse hinnoittelu suunnitelma. Jos sinulla on olemassa hinnoittelu suunnitelma voit valita sen, muuten on luotava palvelun hinta uuden suunnitelman.

1.  **Suunnitelman hinta** avattava, valitse suunnitelman tai **uuden suunnitelman valitsemalla** haluamasi vaihtoehto.
2.  Kirjoita **Suunnitelman nimi**, nimi, joka tunnistaa laskussa suunnitelman.
3.  Valitse **Kuukausittain suunnitelma tasoa**. Suunnitelman tasoa oletusarvon oletus-alue ja web-palvelun suunnitelmat otetaan käyttöön kyseisellä alueella.

Valitse **Ota käyttöön** ja **pikaopas** -sivulta, että web-palvelu avautuu.

Palvelun pikaopas verkkosivun avulla access ja ohjeita, kun olet luonut web-palvelu suorittaa yleisimpiä tehtäviä. Täältä voit käyttää helposti testisivu ja Kulutettava sivulla.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>Web-palvelun testaaminen

Voit testata uuden web-palveluun valitsemalla Tavalliset tehtävät **testata web-palveluun** . Testisivu voit testata web-palvelun muodossa pyyntö-vastaus Service (RRS) tai Batch Execution-palvelu (puurakenteessa painikkeen VIERESSÄ).

Panosten, tuotoksen ja yleiset parametrit, jotka olet määrittänyt kokeen näyttää RRS testisivu. Voit testata web-palveluun, voit manuaalisesti syöttää asianmukaiset arvot tuotantopanosten tai pilkuilla erotettu arvo (CSV) muotoiltu tiedosto, joka sisältää testiarvot antaa.

Jos haluat kokeilla RRS-luettelon Näytä-tilassa, kirjoita asianmukaiset arvot tuotantopanosten ja valitse **Testaa pyyntö-vastaus**. Ennakoinnin tulosten Näytä-sarakkeen vasemmalle puolelle.

![Web-palvelun käyttöönotto](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Voit testata oman puurakenteessa painikkeen VIERESSÄ, valitse **erä**. Erän testin sivulla valitse Selaa-kohdassa kirjoittamasi tiedot ja valitse sopiva otoksen arvot sisältävän CSV-tiedoston. Jos CSV-tiedosto ei ole ja että ehkäisevän kokeen Machine Learning Studiossa on luotu, voit ladata tietojoukon ennakoivia että kokeen osalta ja käyttää sitä.

Voit ladata nämä tiedot, Avaa Machine Learning Studio. Avaa ennakoivia että kokeeseen ja että koe syötteen napsauttamalla hiiren kakkospainikkeella. Valitse **dataset** context menu ja valitse sitten **Lataa**.

![Web-palvelun käyttöönotto](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Valitse **testi**. **Testin erätyöt**oikeus näyttää Batch Execution-työn tilan.

![Web-palvelun käyttöönotto](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

**Määritys** -sivulla voit muuttaa otsikko, kuvaus, päivittää tilin tallennustilan avain ja mallitiedot käyttöön web-palveluun.

![Määritä Internet-palvelu](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Kun olet ottanut käyttöön web-palveluun, voit tehdä seuraavaa:

- **Käyttää** sen web-palvelun API-Liittymän kautta.
- **Hallitse** sitä Azure Machine Learning web services-portaalin tai Azure classic-portaalin kautta.
- **Päivitä** se Jos malli muuttuu.

### <a name="access-the-web-service"></a>Web-palvelun käyttö

Kun otat käyttöön web-palvelun Machine Learning Studio, voit lähettää tiedot palvelun ja vastauksia ohjelmallisesti.

**Kuluta** sivulla on kaikki tiedot, sinun täytyy käyttää web-palvelua. Esimerkiksi API avain toimitetaan valtuutettu käyttämään palvelua.

Saat lisätietoja Machine Learning-web-palvelun käyttämisestä, [miten kuluttaa Azure Machine Learning käyttöönotettu web-palveluun](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Uuden web-palvelun hallinta

Voit hallita palveluita Machine Learning verkkopalveluiden perinteinen web-portaalin. Valitse [portaalin pääsivu](https://services.azureml-test.net/) **Web-palvelut**. Voit poistaa tai kopioida palvelun web services-sivulta. Jos haluat valvoa tietyn palvelun, palvelu ja valitse sitten **Dashboard**. Voit valvoa web-palveluihin liittyvät eräajot, valitse **Erä pyytää loki**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Ottaa käyttöön ehkäisevän kokeen kuin perinteistä web-palveluun

Nyt kun ehkäisevän kokeeseen on valmisteltu riittävästi, voit ottaa Azure WWW-palveluna. Web-palvelun avulla käyttäjät voivat lähettää tiedot malliin ja mallin palauttavat sen laskentamenetelmää.

Ottaa käyttöön ennakoivia että kokeeseen, **suorittaa** kokeen piirtoalustan alaosasta ja valitse sitten **Web-palvelun käyttöön**. Määrittää web-palvelun ja web service-raporttinäkymät on sijoitettu.

![Web-palvelun käyttöönotto](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

Voit testata web-palvelun Machine Learning verkkopalvelujen portaali tai Machine Learning Studio.

Voit testata web-palvelu pyytää vastausta, **Testaa** web service koontinäytön valitsemalla. Valintaikkuna tulee näyttöön, kysy palvelun syöttötiedot. Nämä ovat tulosmalli kokeen odottamaa sarakkeita. Syötä tietoja ja valitse sitten **OK**. Web-palvelun tuottamat tulokset näkyvät Raporttinäkymät-ikkunan alareunassa.

Jos valitset **Testaa** esikatselu-linkkiä voit testata palvelua Azure Machine Learning verkkopalvelujen portaali, uuden web-palvelun osan aikaisemmin esitetyllä tavalla.

Testaa palvelun eräajon suorittamisen valitsemalla **Testaa** esikatselu-linkki. Erän testin sivulla valitse Selaa-kohdassa kirjoittamasi tiedot ja valitse sopiva otoksen arvot sisältävän CSV-tiedoston. Jos CSV-tiedosto ei ole ja että ehkäisevän kokeen Machine Learning Studiossa on luotu, voit ladata tietojoukon ennakoivia että kokeen osalta ja käyttää sitä.

![Voit testata web-palvelu](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

**Määritys** -sivulla voit muuttaa palvelun nimen ja antaa kuvauksen. Nimi ja kuvaus näkyvät [Azure klassinen portal](http://manage.windowsazure.com/) , jolla voit hallita web-palvelut.

Voit antaa kuvauksen syöttötiedot ja siirtää tietoja ja web-palveluparametrit kirjoittamalla kunkin sarakkeen **SYÖTTEEN rakennetta**, **TULOSRAKENNE**ja **Web-palvelun PARAMETRI**merkkijono. Nämä kuvaukset ovat käytössä Internet-palvelun malli-koodin ohjeissa.

Mahdollisia virheitä Toimintokeskuksessa, kun web-palvelua käytetään diagnosoida lokikirjaus otetaan käyttöön. Lisätietoja [ottaa lokiin kirjaamisen käyttöön Machine Learning-web-palvelut](machine-learning-web-services-logging.md).

![Määritä Internet-palvelu](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Voit myös määrittää web-palvelun päätepisteet Azure Machine Learning verkkopalveluiden portaalin osoitetulla uuden web-palvelun osan ohjeiden tapaan. Eri vaihtoehtoja, voit lisätä tai muuttaa palvelun kuvaus, ota kirjaaminen ja ota käyttöön mallitietojen testausta varten.

### <a name="access-the-web-service"></a>Web-palvelun käyttö

Kun otat käyttöön web-palvelun Machine Learning Studio, voit lähettää tiedot palvelun ja vastauksia ohjelmallisesti.

Koontinäytön tarjoaa kaikki tiedot, sinun täytyy käyttää web-palvelua. Esimerkiksi valtuutettu käyttämään palvelua tarjotaan API avain ja API ohjesivujen ovat tarkoitettu auttamaan koodin kirjoittamisen alkuun.

Saat lisätietoja Machine Learning-web-palvelun käyttämisestä, [miten kuluttaa Azure Machine Learning käyttöönotettu web-palveluun](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>Web-palvelun hallinta

On erilaisia toimintoja voit valvoa web-palveluun. Voit päivittää se ja poista se. Voit myös lisätä muita päätepisteet oletusarvoinen päätepiste, joka luodaan, kun otat sen lisäksi perinteinen web-palveluun.

Lisätietoja [Azure Machine Learning-työtilan hallinta](machine-learning-manage-workspace.md) ja [Hallitse web-palvelun Azure Machine Learning verkkopalvelujen portaali](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Web-palvelun päivitys

Voit tehdä muutoksia web-palveluun, kuten lisäkoulutusta tiedot päivitetään mallin ja asentaa uudelleen, korvata alkuperäisen web-palvelun.

Jos haluat päivittää web-palveluun, avaa web-palvelun käyttöön ja tehdä muokattavan kopion valitsemalla **Tallenna kuin**alkuperäinen ehkäisevän kokeeseen. Tee haluamasi muutokset ja valitse sitten **Web-palvelun käyttöön**.

Koska olet otettu käyttöön ennen tämän kokeen, sinulta kysytään, haluatko korvata (Classic verkkopalvelu) tai Päivitä (uuden web-palvelun) olemassa olevia. Valitsemalla **Kyllä** tai **päivitys** lopettaa web-palvelun ja ottaa käyttöön uuden sen tilalle otetaan käyttöön ennakoivia kokeeseen.

> [AZURE.NOTE] Tekemäsi kokoonpanomuutokset alkuperäisen web-palveluun, jos esimerkiksi kirjoittaa uusi näyttönimi ja kuvaus, täytyy syöttää arvot uudelleen.

Yksi tapa päivittää web-palveluun on mallin suuntaamista ohjelmallisesti. Lisätietoja [suuntaamista Machine Learning mallit ohjelmallisesti](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Luo koulutus-koe]: #create-a-training-experiment
[Se muuntaa ehkäisevän koe]: #convert-the-training-experiment-to-a-predictive-experiment
[Uusi]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Classic]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
