<properties 
    pageTitle="Vaiheittainen kuvaus: Valvoa Microsoft Dynamics CRM kanssa hakemuksen tiedot" 
    description="Pyydä telemetriatietojen Microsoft Dynamics CRM Online-sovelluksen tiedot käyttämällä. Ongelmatilanteita asetusten määritysten aikana käytön tiedot, visualisointi ja vie." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Vaiheittainen kuvaus: Ottaminen käyttöön Telemetriatietojen Microsoft Dynamics CRM Online-sovelluksen tiedot käyttämällä

Tässä artikkelissa kerrotaan, miten telemetriatietojen tietojen noutaminen [Microsoft Dynamics CRM Online](https://www.dynamics.com/) käyttämällä [Visual Studio sovelluksen tiedot](https://azure.microsoft.com/services/application-insights/). Käymme tässä läpi koko luettelokohteiden lisääminen sovelluksen tiedot-komentosarja-sovelluksen sieppaaminen tiedot ja tietojen visualisointi.

>[AZURE.NOTE] [Selaa näyte](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Lisää sovellus tiedot uuteen tai aiemmin luotuun CRM Online-esiintymässä 

Voit valvoa sovellusta, sovelluksen havainnollistamisen SDK-paketissa lisääminen sovelluksen. SDK lähettää telemetriatietojen [sovelluksen tiedot-portaalissa](https://portal.azure.com), jossa voit käyttää sekä tehokkaat Analysointityökalut ja vianmääritystyökalut tai tietojen vieminen tallennustilan.

### <a name="create-an-application-insights-resource-in-azure"></a>Luo sovelluksen tiedot-resurssi Azure-tietokannassa

1. Hanki [Microsoft Azure-tiliä](http://azure.com/pricing). 
2. Kirjaudu sisään [Azure portal](https://portal.azure.com) ja lisätä uuden sovelluksen tiedot resurssin. Tämä on missä tietojen käsittelemistä ja näkyviin.

    ![Valitse +, Developer palvelut-sovelluksen tiedot.](./media/app-insights-sample-mscrm/01.png)

    Valitse ASP.NET sovelluksen tyyppi.

3. Avaa pika-Aloitus-välilehden ja koodi-komentosarjan.

    ![](./media/app-insights-sample-mscrm/03.png)

**Jätä koodisivu auki** , kun teet seuraavan vaiheen toisessa selainikkunassa. Tarvitset koodin pian. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>JavaScript-WWW-resurssin luominen Microsoft Dynamics CRM:

1. Avaa CRM Online esiintymän ja kirjaudu sisään järjestelmänvalvojan oikeuksilla.
2. Avaa Microsoft Dynamics CRM-asetuksia, mukautukset, Mukauta järjestelmä

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Luo JavaScript-resurssi.

    ![](./media/app-insights-sample-mscrm/07.png)

    Anna sille nimi, valitse **komentosarja (JScript)** ja Avaa tekstieditori.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Kopioi koodi sovelluksen tiedot. Kopioitaessa muista Ohita komentosarjan tunnisteet. Lisätietoja on ohjeaiheessa näyttökuva alla:

    ![](./media/app-insights-sample-mscrm/09.png)

    Koodi on instrumentation avainta, joka määrittää sovelluksen tiedot-resurssi.

5. Tallenna ja Julkaise.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Välineen lomakkeet

1. Microsoft CRM Online Avaa Asiakas-lomakkeeseen

    ![](./media/app-insights-sample-mscrm/11.png)

2. Avaa ominaisuudet-lomake

    ![](./media/app-insights-sample-mscrm/12.png)

3. Lisää JavaScript WWW-resurssi, jonka loit

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Tallenna ja julkaise lomakkeen mukautukset.


## <a name="metrics-captured"></a>Arvot tallennetaan

Voit nyt määrittää telemetriatietojen sieppaus lomakkeen. Aina, kun sitä käytetään tiedot lähetetään sovelluksen tiedot-resurssi.

Seuraavassa on esimerkkejä tiedot, jotka näet.

#### <a name="application-health"></a>Sovelluksen kunto

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Selaimen poikkeukset:

![](./media/app-insights-sample-mscrm/17.png)

Napsauta kaaviota, saat tarkempia tietoja:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Käyttö

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Selaimet

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>: N maantieteellinen sijainti

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Keltaiset sivupyynnön näkymä

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Esimerkki koodi

[Selaa sample code](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI

Voit tehdä myös myyntilukujen, jos et [Vie tietoja Microsoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Esimerkki Microsoft Dynamics CRM-ratkaisun

[Tässä on esimerkki-ratkaisun Microsoft Dynamics CRM toteutettu] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Opi lisää

* [Mikä on hakemuksen tiedot?](app-insights-overview.md)
* [Verkkosivujen sovelluksen tietoa](app-insights-javascript.md)
* [Lisää näytteiden ja vaihe vaiheelta](app-insights-code-samples.md)

 
