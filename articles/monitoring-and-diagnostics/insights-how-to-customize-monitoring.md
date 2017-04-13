<properties
    pageTitle="Yleistä Microsoft Azure arvot | Microsoft Azure"
    description="Opettele mukauttamaan seurantaa kaavioiden Azure-tietokannassa."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>Yleistä Microsoft Azure arvot

Azure palvelun seurata avaimen arvot, jotta voit seurata kunto, suorituskykyä, käytettävyys ja palvelujen käyttö. Näet nämä arvot Azure-portaalissa, ja voit myös käyttää [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) tai [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) mittarit kaikkia käytettävissä käyttäminen ohjelmallisesti.

Joidenkin palvelujen joudut ehkä ottaminen käyttöön Diagnostiikka, jotta voit nähdä kaikki arvot. Muiden näennäiskoneiden, kuten näet arvot basic joukkoa, mutta on käyttöön täydellinen määrittää hyvin usein arvot. Katso lisätietoja [käyttöön seuranta ja vianmääritys](insights-how-to-use-diagnostics.md) .

## <a name="using-monitoring-charts"></a>Seuranta-kaavioiden käyttäminen

Kaavio, jokin määritetty ne kaikki ajanjaksolla, voit valita.

1. [Azure-portaali](https://portal.azure.com/)valitsemalla **Selaa**ja valitse sitten resurssi käyttämällä seuranta.

2. **Seuranta** -osio sisältää tärkeimmät arvot Azure kullekin resurssille. Esimerkiksi web App-sovelluksesta on **pyynnöt ja virheet**, jossa kuin virtual machine on **suorittimen prosentteina** ja **levyn lukeminen ja kirjoittaminen**:  ![linssin seuranta](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. Valitsemalla minkä tahansa kaavio näyttää **metrijärjestelmä** -sivu. Sivu lisäksi kaavio, valitse on taulukko, joka sisältää arvot (kuten keskiarvo, vähimmäis- ja enimmäisarvot päällä olevan aikavälin, jonka valitsit) koosteet. Alla on resurssin ilmoitusten sääntöjä.
    ![Metrijärjestelmän sivu](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. Voit mukauttaa rivit, jotka tulevat näkyviin valitsemalla kaavion **Muokkaa** -painiketta, tai metrisillä sivu **Kaavion Muokkaa** -komentoa.

5. Tee Muokkaa kyselyä-sivu kolme asiaa:
    - Olevan aikavälin muuttaminen
    - Vaihda välillä ulkoasun palkki ja viiva
    - Valitse eri metics ![Muokkaa kyselyä](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. Olevan aikavälin muuttaminen on yhtä helppoa kuin valitsemalla eri tietoalueen (kuten **Kuluneen tunnin**) ja valitsemalla sivu alareunassa **Tallenna** . Voit myös valita **Mukautettu**, jonka avulla voit valita minkä tahansa ajanjakson kahden viikon aikana. Kuten näet koko kahden viikon aikana, tai, vain 1 tunti eilisen. Kirjoita teksti-ruutuun eri tunti.
    ![Mukautetun aikavälin](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. Alla olevan aikavälin voit kanava, valitse haluamasi arvot, jos haluat näyttää kaaviossa määrän.

8. Kun valitset Tallenna tekemäsi muutokset tallennetaan kyseiseen resurssiin. Esimerkiksi jos sinulla on kaksi näennäiskoneiden, yhden kaavion muuttaminen ei vaikuttaa muihin.

## <a name="creating-side-by-side-charts"></a>Rinnakkain kaavioiden luominen

Tehokas mukautus portaalissa voit lisätä niin monta kaavioiden sellaisena kuin haluat.

1. **...** -Valikon yläreunaan sivu napsauttamalla **ruutujen lisääminen**:  
    ![Lisää-valikko](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Tämän jälkeen voit Valitse valita kaavion **valikoima** näytön oikeassa reunassa:  ![valikoima](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Jos et näe haluamasi lisätiedot, voit aina lisätä jokin valmiiksi määritetyt arvot, ja **Muokkaa** metrijärjestelmä, jotka on kaaviossa.

## <a name="monitoring-usage-quotas"></a>Käyttökiintiön seuranta

Useimmat arvot kerrotaan trendejä ajan kuluessa, mutta tiettyjä tietoja, kuten käyttökiintiön, ovat ajankohta tietojen kanssa raja-arvon.

Näet käyttökiintiön myös resursseja, joille kiintiöiden sivu:

![Käyttö](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Kuten mittarit, jossa voit tehdä [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) tai [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) käyttää kaikkia käytettävissä olevia käyttökiintiön ohjelmallisesti.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Ilmoitusten vastaanottaminen](insights-receive-alert-notifications.md) , kun mittarin leikkauspiste raja-arvon.
* Kerää yksityiskohtaisia hyvin usein arvot palvelustasi [käyttöön seuranta- ja diagnostiikka](insights-how-to-use-diagnostics.md) .
* Varmista, että palvelu on käytettävissä ja vastaa [skaalata esiintymän Laske automaattisesti](insights-how-to-scale.md) .
* Voit halutessasi selvittääksesi, miten pilveen osassa koodisi toimii [näytön sovelluksen suorituskykyä](../application-insights/app-insights-azure-web-apps.md) .
* Hae asiakkaan tietoja selaimissa, jotka verkkosivulla analytics [Sovelluksen tietoa JavaScript-sovellukset ja web-sivujen](../application-insights/app-insights-web-track-usage.md) avulla.
* [Näytön käytettävyys- ja minkä tahansa verkkosivun vasteaikaa](../application-insights/app-insights-monitor-web-app-availability.md) sovelluksen tiedot, jotta voit tarkistaa, jos sivu on poissa käytöstä.
