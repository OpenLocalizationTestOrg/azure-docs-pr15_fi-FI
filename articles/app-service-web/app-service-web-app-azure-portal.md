<properties
    pageTitle="Siirtyminen Azure portal-viittaus"
    description="Lisätietoja toisen käyttäjän kokemukset Web App-palvelun hallinta-portaalin ja Azure-portaalin välillä"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>Siirtyminen Azure portal-viittaus

Azure sivustojen kutsutaan nyt [Palvelun Web sovellukset](http://go.microsoft.com/fwlink/?LinkId=529714). Kaikki tämän nimen muuttaminen vastaamaan ja antaa ohjeet Azure-portaalin Microsoftin asiakirjat päivitetään. Ennen kuin prosessi on valmis, voit käyttää tätä tiedostoa ohjeena käsittelyyn verkkosovelluksissa Azure-portaalissa.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>Perinteinen Azure-portaalin myöhemmin

Kun huomaat yrityksen tunnuksen muutokset perinteinen Azure-portaalissa, jota portaalin parhaillaan on korvattu Azure-portaalissa. Perinteinen portaalissa on poistumassa, kun kohdistus uusi kehittämiseen eteenpäin Azure-portaaliin. Web-sovellusten kaikki tulevat uudet ominaisuudet tulevat Azure-portaalissa. Jotta voit hyödyntää uudet ja että verkkosovelluksissa tarjota Azure-portaalin käytön aloittaminen.

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>Asettelun erot Azure perinteinen portaalin ja Azure-portaalissa

Perinteinen-portaalissa vasemmalla puolella näkyvät kaikki Azure-palvelut. Siirtyminen perinteinen portaalissa seuraa puun rakenne, jossa palvelun käynnistäminen ja siirtyy jokaisen osan. Tämä rakenne toimii hyvin, kun riippumaton osien hallinta. Azure-sovellusten on joukko toisiinsa liittyviä palveluja, ja puun-rakennetta ei ole ihanteellinen sivustokokoelmat palvelujen käyttöä. 

Azure portaalin on helppo luoda sovellukset-kattavan osat-palveluihin. Portaalissa on järjestetty *matkat*nimellä. *Matkan* on *terät*, jotka ovat eri osien säilöjä sarjaa. Määrittämällä esimerkiksi Automaattinen skaalaaminen verkkosovellukseen on *matkan* , jossa kerrotaan useita terät, kuten seuraavassa esimerkissä: **sivusto** -sivu (, että sivu otsikko ei vielä ole päivitetty käyttämään uusia termejä)- **asetukset** -sivu ja **ulos asteikko** -sivu. Esimerkissä Automaattinen skaalaus on parhaillaan määritetty määräytyvät suorittimen käyttö on myös **Suorittimen prosentti** -sivu. *Lavat* osia kutsutaan *osat*, jotka näyttävät ruudut. 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>Siirtyminen Esimerkki: web-sovelluksen luominen

Uusi web Apps-sovellusten luominen on edelleen yhtä helppoa kuin 1-2-3. Seuraava kuva esittää perinteinen portaalin ja portaalin-rinnakkais osoittamaan, että ei ole paljon on muuttunut tarvitsee halutun web App-sovelluksen ja suorittamalla vaiheet määrä. 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

Portaalissa voit valita yleisimpien web Apps-sovelluksista, mukaan lukien Suositut valikoima-sovelluksia, kuten WordPress. Täydellisen luettelon käytettävissä olevista sovelluksista Siirry [Azure Marketplacesta].

Kun luot web-sovelluksen, voit määrittää URL-osoite, sovelluksen palvelusopimus ja sijainti-portaalissa samalla tavalla kuin teet perinteinen-portaalissa. 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

Lisäksi portaalin avulla voit määrittää muita yleisiä asetuksia. Esimerkiksi [resurssiryhmät](../azure-resource-manager/resource-group-overview.md) tehdä yksinkertaisia voit tarkastella ja hallita Azure liittyvät resurssit. 

## <a name="navigation-example-settings-and-features"></a>Siirtyminen Esimerkki: asetukset ja toiminnot

Asetukset ja toiminnot on nyt loogisesti ryhmitelty yksi sivu, josta voit siirtyä.

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

Voit esimerkiksi luoda mukautettuja toimialueita valitsemalla **Mukautetut toimialueet ja SSL** - **asetukset** -sivu.

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

Voit määrittää seurantaa ilmoitus, valitsemalla **pyynnöt ja virheet** ja valitse sitten **Lisää ilmoitus**.

![](./media/app-service-web-app-azure-portal/Monitoring.png)

Valitse **Diagnostiikka lokit** diagnostiikka käyttöön **asetukset** -sivu.

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
Voit määrittää sovelluksen asetukset valitsemalla **Sovellusasetukset** **asetukset** -sivu. 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

Kuvitteellinen nimi, kuin muutama seikka portaalissa on nimetty uudelleen tai tiedostojasi on helpompi löytää ne ryhmitelty eri tavalla. Kuten alla on vastaavaan sivuun sovelluksen näyttökuva perinteinen portaalissa (**Määritä**) asetukset.

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>Lisää resursseja

[Azure Portal]: https://portal.azure.com
[Azure Marketplacesta]: /marketplace/

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="whats-changed"></a>Mikä on muuttunut
* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut](http://go.microsoft.com/fwlink/?LinkId=529714)
 
