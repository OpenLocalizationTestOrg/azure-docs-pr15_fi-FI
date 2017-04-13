<properties
    pageTitle="Vapauta huomautusten hakemuksen tiedot | Microsoft Azure"
    description="Lisää käyttöönoton tai korostaaksesi hakemuksen tiedot arvot explorer kaavioiden luominen."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Vapauta huomautukset-sovelluksen tiedot

Vapauta huomautuksia [Arvot Explorer](app-insights-metrics-explorer.md) palkkikaaviot näyttävät, jossa uusi versio asennettuna. Niiden avulla on helppo nähdä, onko tekemäsi muutokset ovat vaikutusta sovelluksen suorituskykyä. Ne voivat automaattisesti luoda [Visual Studio Team Services luo järjestelmän](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs)ja voit myös [luoda niitä PowerShell](#create-annotations-from-powershell).

![Esimerkki huomautusten kanssa näkyvissä korrelaatio palvelimen vastausajan kanssa](./media/app-insights-annotations/00.png)

Vapauta huomautuksia on pilvipohjainen luo ominaisuus, ja vapauta Visual Studio Team Services-palvelun. 

## <a name="install-the-annotations-extension-one-time"></a>Asenna huomautukset-tunniste (kerran)

Voivat luoda release huomautukset, sinun on asentaa jokin käytettävissä monta ryhmän palvelun laajennuksia Visual Studio Marketplacesta.

1. Kirjaudu [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projektin.
2. Visual Studio Marketplacesta [Hae Release huomautukset-tunniste](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), ja lisää se Team Services-tiliisi.

![AT parhaat ryhmän Services web-sivun, Avaa Marketplace oikealla. Valitse Visual ryhmän palvelut, ja valitse-kohdassa muodosta ja vapauta, katso lisätietoja.](./media/app-insights-annotations/10.png)

Tarvitset vain voit tehdä tämän kerran Visual Studio Team Services-tilin. Vapauta huomautukset voi määrittää nyt kaikki projektin tilissäsi. 

## <a name="get-an-api-key-from-application-insights"></a>Pyydä Ohjelmointirajapinnan avain hakemuksen tiedot

Sinun on suoritettava tämä kunkin release-malli, jonka haluat luoda release huomautukset.


1. Kirjaudu [Microsoft Azure Portal](https://portal.azure.com) -ja avaa sovelluksen tiedot-resurssi, joka valvoo sovelluksen. (Tai [luoda sen nyt](app-insights-overview.md), jos et ole antanut sitä vielä.)
2. Avaa **API Access**ja tutustu kopion **Havainnollistamisen tunnus**.

    ![Avaa sovellus tiedot resurssin portal.azure.com, ja valitse asetukset. Avaa API-käyttöoikeuksia. Kopioi Sovellustunnus](./media/app-insights-annotations/20.png)

2. Erillisessä selainikkunassa Avaa (tai luo), joka hallitsee käyttöönoton Visual Studio Team Services release-malli. 

    Tehtävän ja valitse sovelluksen tiedot Release huomautuksen tehtävän valikosta.

    Liitä **Tunnus** , jonka kopioit API-käyttöoikeuksia-sivu.

    ![Visual Studio ryhmän palveluja Avaa julkaisu, valitse versio-määritys ja valitse Muokkaa. Lisää tehtävä ja valitse sovelluksen tiedot Release huomautuksen. Liitä sovelluksen havainnollistamisen ID-tunnuksellasi.](./media/app-insights-annotations/30.png)

3. **APIKey** -kentän arvoksi muuttujan `$(ApiKey)`.

4. API-käyttöoikeuksia-sivu-kohdassa Luo uusi Ohjelmointirajapinnan avain ja ottaa sen.

    ![Valitse Luo Ohjelmointirajapinnan avain Azure-ikkunassa API-käyttöoikeuksia-sivu. Anna kommentin, kirjoita huomautukset Tarkista ja valitse Luo avain. Kopioi uusi avain.](./media/app-insights-annotations/40.png)

4. Avaa release mallin määritys-välilehti.

    Luo muuttujan määritelmä `ApiKey`.

    Liitä API key-tuotetunnuksen ApiKey muuttujan määritelmän.

    ![Valitse ryhmän palvelut-ikkuna määritys-välilehti ja valitse Lisää muuttujan. Määritetty nimi ApiKey ja arvoksi, Liitä olet juuri luonut avain.](./media/app-insights-annotations/50.png)


5. Lopuksi **Tallenna** julkaisu määritys.

## <a name="create-annotations-from-powershell"></a>Huomautuksia luominen PowerShell

Voit myös luoda minkä tahansa prosessista (käyttämättä ja ryhmän järjestelmä), kuten huomautukset. 

Hae [-GitHub Powershell-komentosarjaa](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

Käytä tältä:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Hae `applicationId` ja `apiKey` hakemuksen tiedot resurssin: Avaa asetukset, API Accessin ja kopioi sovelluksen tunnus. Valitse valitsemalla Luo Ohjelmointirajapinnan avain ja kopioi sitten-näppäintä. 

## <a name="release-annotations"></a>Vapauta huomautukset

Nyt kun release-mallin avulla voit ottaa käyttöön uudessa versiossa, huomautuksen lähetetään sovelluksen havainnollistamisen. Huomautukset näkyvät kaavioiden arvot Resurssienhallinnassa.

Napsauta huomautuksen merkinnän Avaa versio, mukaan lukien pyytäjän tietolähteen ohjausobjektin haaran tietoja, Vapauta määritys ja ympäristössä.


![Valitse Vapauta huomautuksen merkinnän.](./media/app-insights-annotations/60.png)
