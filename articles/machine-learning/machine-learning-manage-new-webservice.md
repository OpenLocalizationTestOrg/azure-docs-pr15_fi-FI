<properties
    pageTitle="Hallitse Azure koneen Learning Web Serivces portaalissa verkkopalvelun | Microsoft Azure"
    description="Hallita niiden käyttöä Azure koneen Learning työtilojen ja käyttöön ja hallita ML API-verkkopalvelut"
    services="machine-learning"
    documentationCenter=""
    authors="vDonGlover"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="v-donglo"/>


# <a name="manage-a-web-service-using-the-azure-machine-learning-web-services-portal"></a>Hallitse verkkopalvelun Azure koneen Learning Web Services-portaalissa

Voit hallita tietokoneen Learning uusien ja perinteinen verkkopalvelut Microsoft Azure koneen Learning Web Services-portaalissa. Koska perinteinen verkkopalveluita ja uusi verkkopalvelut perustuvat eri pohjana tekniikoiden, sinulla on hieman erilainen hallintatoiminnot kullekin niistä.

Koneen Learning Web Services-portaalissa voit:

- Seurata, miten web-palvelu on käytössä.
- Määrittää kuvaus-Päivitä näppäimet Web-palveluun (vain uutta), päivittää oman tallennustilan tilin avaimen (vain uutta), Ota loki käyttöön, ja ota käyttöön tai poistaa käytöstä mallitiedot.
- Poista web-palveluun.
- Luo, poista tai päivitä Laskutus-Palvelupaketit (vain uutta).
- Lisääminen ja poistaminen päätepisteet (vain Klassinen)

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="manage-new-web-services"></a>Uusi Web-palveluiden hallinta

Voit hallita uuden sivuston palvelujen seuraavasti:

1.  Kirjaudu sisään käyttämällä Microsoft Azure-tiliä [Microsoft Azure koneen Learning Web Services](https://services.azureml.net/quickstart) -portaaliin - tiliä, joka on liitetty Azure-tilaukseesi.
2.  Valitse-valikosta **Verkkopalveluihin**.

Tämä näyttää luettelon käyttöön verkkopalvelut tilauksen. 

Voit hallita verkkopalvelun valitsemalla verkkopalveluihin. Verkkopalvelut-sivulla voit tehdä seuraavaa:

- Napsauta WWW-palvelun avulla hallita sitä.
- Valitse Laskutus suunnitteleminen web-palvelu päivittää sen.
- Poista verkkopalvelun.
- Kopioi verkkopalvelun ja ota se käyttöön toisen alueen.

Kun napsautat verkkopalvelun, web palvelun pikaopas-sivu avautuu. Palvelun pikaopas verkkosivun on kaksi valikkovaihtoehdot, jotta voit hallita web-palvelu:

- **DASHBOARD** - avulla voit tarkastella Web-palvelujen käyttöä.
- **Määritä** - avulla voit lisätä kuvaavan tekstin, Päivitä Web-palveluun liitetyn tallennustilan tilin avain ja ottaminen käyttöön tai poistaa käytöstä mallitiedot.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Miten WWW-palvelun käytetään seuranta ###

Valitse **KOONTINÄYTTÖ** -välilehti.

Raporttinäkymät-ikkunan voit tarkastella WWW-palvelun yleinen käyttö ajan kuluessa. Voit valita, jolta haluat nähdä kauden pudotusvalikosta käyttö kaavion oikeassa yläkulmassa. Koontinäytön näkyvät seuraavat tiedot:

- **Pyynnöt tietyllä ajanjaksolla** näkyy pyyntöjen määrä vaiheen päälle valitulle aikajaksolle. Se tunnistaa Jos kohtaat käyttö piikkarit.
- **Pyynnön vastaus pyynnöt** näyttää palvelu on vastaanottanut valitulle aikajaksolle ja kuinka monta ne epäonnistui-vastaus kutsujen määrä.
- **Keskiarvo-vastaus Laske aika** näkyy keskiarvo vastaanotettu pyynnöt suorittamiseen tarvittava aika.
- **Erän pyynnöt** näyttää erä pyyntöjen palvelu on vastaanottanut valitulle aikajaksolle ja kuinka monta ne epäonnistui kokonaismäärä.
- **Keskimääräinen työn odotusaika** näyttää keskimäärin vastaanotettujen pyyntöjen suorittamiseen tarvittava aika.
- **Virheet** näkyvät asennuksen aikana tapahtuneista virheistä kokonaismäärä kutsuja web-palveluun.
- **Palvelujen kustannukset** näyttää kulujen-palveluun liitetyn Laskutus palvelupaketin.

### <a name="configuring-the-web-service"></a>WWW-palvelun määrittäminen ###

Valitse **Määritä** valikkovaihtoehto.

Voit päivittää seuraavat ominaisuudet:

* **Kuvaus** antaa WWW-palvelun kuvaus.
* **Otsikon** voit määrittää WWW-palvelun otsikko
* Voit kiertää ensisijainen ja toissijainen API näppäimet **näppäimet** .
* **Tallennustilan tilin avaimen** avulla voit päivittää Web-palvelumuutoksista liittyvää tallennustilan tiliä näppäintä. 
* **Ota käyttöön mallitiedot** ansiosta voit lähettää mallitiedot, jonka avulla voit testata vastaus palvelua. Jos olet luonut WWW-palvelun koneen Learning Studiossa, esimerkkitiedot haetaan tiedot kouluttaminen mallin oman käytetään. Jos olet luonut palvelun ohjelmallisesti, tiedot haetaan antamasi JSON-ohjelmistopaketin osana esimerkkitietoja.

### <a name="managing-billing-plans"></a>Laskutus palvelupaketin hallinta ###

Valitse **suunnitelmien** -valikkovaihtoehto web services pikaopas-sivulta. Voit myös napsauttaa hallittavan suunnitelman tietyn Web-palveluun liitetyn suunnitelma.

* **Uusi** avulla voit luoda uuden suunnitelman.
* **Lisää tai poista suunnitelman esiintymän** voit "skaalata" Lisää kapasiteetin suunnitelman.
* **Päivityksen/artikkelissa** voit "skaalata" Lisää kapasiteetin suunnitelman.
* **Poista** voit poistaa suunnitelma.

Valitse suunnitelma, voit tarkastella sen Raporttinäkymät-ikkunan. Koontinäytön saat tilannevedoksen tai suunnitelma käyttö valitun ajan kuluessa. Valitse aikajakso tarkastelemiseen, valitsemalla **kauden** avattavasta valikosta osoitteessa Raporttinäkymät-ikkunan oikeassa yläkulmassa. 

Suunnitelman Raporttinäkymät-ikkunan sisältää seuraavat tiedot:

* **Suunnitelman kuvaus** näyttää tietoja kustannuksia ja järjestelyyn liittyvät kapasiteetti.
* **Suunnitelman käyttö** näyttää tapahtumia ja Laske työtunnit kirjoitetaan suunnitelman kuuluvan määrän.
* **Verkkopalvelut** Näyttää numeron WWW-palveluista, jotka käyttävät tätä palvelupakettia.
* **Ensimmäiset Web palvelun mukaan puhelut** näyttää neljän suosituimman verkkopalvelut, jotka ovat puheluiden soittamiseen, jotka veloitetaan vastaan suunnitelma.
* **Ylä-verkkopalvelut mukaan Laske h** näyttää neljän suosituimman verkkopalvelut, jotka käyttävät Laske resurssit, jotka veloitetaan vastaan suunnitelma.

## <a name="manage-classic-web-services"></a>Perinteinen Web-palveluiden hallinta

> [AZURE.NOTE] Tämän osan ohjeita liittyvät hallinta perinteinen verkkopalvelut palvelun Azure koneen Learning Web Services-portaalissa. Saat lisätietoja perinteinen verkkopalvelut kautta koneen Learning Studio ja Azure perinteinen-portaalin hallinta-kohdassa [Manage Azure koneen Learning työtila](machine-learning-manage-workspace.md).

Voit hallita perinteinen verkkopalvelut seuraavasti:

1.  Kirjaudu sisään käyttämällä Microsoft Azure-tiliä [Microsoft Azure koneen Learning Web Services](https://services.azureml.net/quickstart) -portaaliin - tiliä, joka on liitetty Azure-tilaukseesi.
2.  Valitse-valikosta **Perinteinen verkkopalveluihin**.

Voit hallita perinteinen verkkopalvelun valitsemalla **Perinteinen verkkopalveluihin**. Perinteinen verkkopalvelut-sivulla voit tehdä seuraavaa:

- Valitse liittyvä päätepisteet tarkastelemaan web-palvelu.
- Poista verkkopalvelun.

Kun hallitset perinteinen verkkopalvelun, voit hallita kunkin päätepisteet erikseen. Kun napsautat verkkopalvelut-sivun web-palvelu, päätepisteet-palveluun liitetyn luettelo avautuu. 

Perinteinen verkkopalvelun päätepiste-sivulla voit lisätä ja poistaa päätepisteet-palvelusta. Lisätietoja päätepisteet lisäämisestä on artikkelissa [Luominen päätepisteet](machine-learning-create-endpoint.md).

Valitse jokin päätepisteet Avaa palvelun pikaopas sivu. Pikaopas-sivulla on kaksi valikkovaihtoehdot, jotta voit hallita web-palvelu:

- **DASHBOARD** - avulla voit tarkastella Web-palvelujen käyttöä.
- **Määritä** - avulla voit lisätä kuvaileva teksti, virhe kirjaamisen ottaminen käyttöön ja poistaminen käytöstä, tallennustilaan näppäintä tilin Web-palveluun liitetyn ja käyttöön ja poistaminen käytöstä tietojen päivitys.

### <a name="monitoring-how-the-web-service-is-being-used"></a>Miten WWW-palvelun käytetään seuranta ###

Valitse **KOONTINÄYTTÖ** -välilehti.

Raporttinäkymät-ikkunan voit tarkastella WWW-palvelun yleinen käyttö ajan kuluessa. Voit valita, jolta haluat nähdä kauden pudotusvalikosta käyttö kaavion oikeassa yläkulmassa. Koontinäytön näkyvät seuraavat tiedot:

- **Pyynnöt tietyllä ajanjaksolla** näkyy pyyntöjen määrä vaiheen päälle valitulle aikajaksolle. Se tunnistaa Jos kohtaat käyttö piikkarit.
- **Pyynnön vastaus pyynnöt** näyttää palvelu on vastaanottanut valitulle aikajaksolle ja kuinka monta ne epäonnistui-vastaus kutsujen määrä.
- **Keskiarvo-vastaus Laske aika** näkyy keskiarvo vastaanotettu pyynnöt suorittamiseen tarvittava aika.
- **Erän pyynnöt** näyttää erä pyyntöjen palvelu on vastaanottanut valitulle aikajaksolle ja kuinka monta ne epäonnistui kokonaismäärä.
- **Keskimääräinen työn odotusaika** näyttää keskimäärin vastaanotettujen pyyntöjen suorittamiseen tarvittava aika.
- **Virheet** näkyvät asennuksen aikana tapahtuneista virheistä kokonaismäärä kutsuja web-palveluun.
- **Palvelujen kustannukset** näyttää kulujen-palveluun liitetyn Laskutus palvelupaketin.

### <a name="configuring-the-web-service"></a>WWW-palvelun määrittäminen ###

Valitse **Määritä** valikkovaihtoehto.

Voit päivittää seuraavat ominaisuudet:

* **Kuvaus** antaa WWW-palvelun kuvaus. Kuvaus on muuttaminen pakolliseksi kentäksi.
* **Lokiin kirjaaminen** avulla voit ottaa käyttöön tai poistaa käytöstä päätepisteen virheloki. Lisätietoja kirjaaminen on artikkelissa Ota [lokiin kirjaaminen koneen Learning web Services](machine-learning-web-services-logging.md).
* **Ota käyttöön mallitiedot** ansiosta voit lähettää mallitiedot, jonka avulla voit testata vastaus palvelua. Jos olet luonut WWW-palvelun koneen Learning Studiossa, esimerkkitiedot haetaan tiedot kouluttaminen mallin oman käytetään. Jos olet luonut palvelun ohjelmallisesti, tiedot haetaan antamasi JSON-ohjelmistopaketin osana esimerkkitietoja.

## <a name="grant-or-suspend-access-to-web-services-for-users-in-the-portal"></a>Myönnä tai keskeyttää verkkopalvelut käyttäjien pääsyn-portaalissa

Käytä Azure perinteinen portaalin, voit sallia tai kieltää tietyille käyttäjille.

### <a name="access-for-users-of-new-web-services"></a>Uusi Web-palvelujen käyttäjien

Jotta muut käyttäjät voivat käsitellä Web palvelujen Azure koneen Learning Web Services-portaalissa on lisättävä ne kuin Mää järjestelmänvalvojia Azure-tilauksessa.

Kirjaudu sisään käyttämällä Microsoft Azure-tiliä [Azure perinteinen portal](https://manage.windowsazure.com/) - tiliä, joka on liitetty Azure-tilaukseesi.

1. Valitse siirtymisruudussa **asetukset**ja valitse **Järjestelmänvalvojat**.
2. Valitse **Lisää**-ikkunan alareunassa. 
3. Kirjoita Lisää A muiden järjestelmänvalvoja-valintaikkunassa voit lisätä muiden järjestelmänvalvojana ja valitse sitten Tilaus, jonka haluat muiden järjestelmänvalvojat voivat käyttää kutsuttavan henkilön sähköpostiosoite.
4. Valitse **Tallenna**.

### <a name="access-for-users-of-classic-web-services"></a>Käyttäjien perinteinen WWW-palveluista

Voit hallita työtilan seuraavasti:

Kirjaudu Microsoft Azure-tilin käyttö [Azure perinteinen portal](https://manage.windowsazure.com/) - tiliä, joka on liitetty Azure-tilaukseesi.

1. Napsauta Microsoft Azure palvelut-paneelin **Koneen LEARNING**.
1. Napsauta työtilaa haluat hallita.
1. Valitse **määritys** -välilehti.

Määritys-välilehdessä voit keskeyttää tietokoneen Learning työtilan käyttöoikeus valitsemalla **HYLKÄÄ**. Käyttäjät eivät enää voi avata työtilan koneen Learning Studiossa. Voit palauttaa access valitsemalla **Salli**.

Tietyille käyttäjille:

Voit hallita tilejä, joka käyttää tietokoneen Learning Studiossa työtilaan valitsemalla **Kirjautuminen ML Studio** - **KOONTINÄYTTÖ** -välilehti. Tämä avaa työtilan koneen Learning Studiossa. Tässä näkymässä napsauttamalla **asetukset** -välilehti ja sitten **käyttäjät**. Voit valita portaalisivuston käyttöoikeuksien myöntäminen työtilassa tai valitse käyttäjä ja valitse **Poista** **Kutsu Lisää käyttäjiä** .

> [AZURE.NOTE] **Kirjautuminen ML Studio** -linkki avaa tietokoneen Learning Studio olet kirjautunut sisään Microsoft-Account. Kirjautuminen Azure perinteinen portaaliin työtilan luonnin yhteydessä käyttämäsi Microsoft-Account ei ole oikeutta avata työtilan automaattisesti. Työtilan avaaminen sinun on kirjauduttava Microsoft-Account, joka on määritetty työtilan omistaja tai haluat vastaanottaa kutsun liittymään työtilan omistaja.
