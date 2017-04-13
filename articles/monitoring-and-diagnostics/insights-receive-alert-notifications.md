<properties
    pageTitle="Azure-palveluiden ilmoitusten ilmoitusten vastaanottaminen | Microsoft Azure"
    description="Ilmoituksen, kun ilmoitusten sääntöjen ehdot täyttyvät."
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

# <a name="receive-alert-notifications"></a>Ilmoitusten vastaanottaminen

Voit vastaanottaa ilmoituksen seurantaa mittaukset tai Azure services-tapahtumat.

Kun tietyn mittarin arvo ylittää raja-arvo on määritetty, metrisillä arvon ilmoitusten sääntöä, hälytyksen aktivoituu ja lähettää ilmoituksen. Tapahtumien ilmoitusten säännön säännön lähettää ilmoituksen *jokaisen* tapahtuman tai, vain silloin, kun tietty määrä tapahtumien tapahtuu.

Kun luot ilmoitusten sääntö, voit valita Lähetä sähköposti-ilmoituksen, palvelun järjestelmänvalvoja ja apuyhteyshenkilöiden tai toinen järjestelmänvalvoja, voit määrittää asetukset. Sähköposti-ilmoitus lähetetään, kun säännön aktivoituu ja ilmoitusten ehto ratkeaa.

Voit käyttää [REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx) tai [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) ja saada ilmoituksen säännöt tietoja ohjelmallisesti.

## <a name="create-an-alert-rule"></a>Ilmoitusten säännön luominen

1. [Portaalin](https://portal.azure.com/)valitsemalla **Selaa**ja valitse sitten resurssi käyttämällä seuranta.

2. Valitse **Toiminnot** -linssin **ilmoitusten säännöt** -ruutu.

3. Napsauta **Lisää ilmoitus** -komentoa.

    ![Lisää ilmoitus](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. Voit ilmoitusten säännön nimi ja valitse kuvaus, joka näkyy sähköposti-ilmoitus.

5. Kun valitset **arvot** Valitse ehto ja arvo raja arvo. Tämä on aikaa, jonka avulla Azure näyttö ja piirron ilmoitusten tehtävän.

    ![Ehto ja kynnysarvo](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. Voit myös valita **tapahtumien**ja saat ilmoituksen, kun tiettyjen tapahtumien tapahtuu.

    ![Tapahtumat](./media/insights-receive-alert-notifications/Insights_Events.png)

7. Lopuksi voit lähettää sähköposti-ilmoituksen vastaava järjestelmänvalvojat.

Kun valitset **Tallenna**, muutaman minuutin kuluessa voi ajan tasalla aina, kun valitset metrijärjestelmä kynnysarvo.

## <a name="managing-your-alert-rules"></a>Ilmoitusten sääntöjen hallinta

Kun olet luonut ilmoitusten sääntöä, voit tarkastella ilmoituksen esikatselu kynnysarvo verrattuna edellisen päivän arvo.

![Tapahtumat](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


Voit muokata kurssin tämän ilmoituksen sääntö ja **Uutissyötetoimintojen** **ottaminen käyttöön** tai sen halutessasi tilapäisesti Pysäytä se ilmoitusten vastaanottaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Määritä webhooks, jolla ilmoitukset toimitetaan](insights-webhooks-alerts.md) eri kanavien reitti-ilmoitukset
* [Näytön palvelun mittarit](insights-how-to-customize-monitoring.md) , varmista, että palvelu on käytettävissä ja vastaa.
* Kerätä yksityiskohtaisia hyvin usein arvot palvelun [käyttöön seuranta- ja diagnostiikka](insights-how-to-use-diagnostics.md) .
* [Näytön käytettävyys- ja minkä tahansa verkkosivun vasteaikaa](../application-insights/app-insights-monitor-web-app-availability.md) sovelluksen tiedot, jotta voit tarkistaa, jos sivu on poissa käytöstä.
* Voit halutessasi selvittääksesi, miten pilveen osassa koodisi suorittaman [sovelluksen suorituskyvyn seuranta](../application-insights/app-insights-azure-web-apps.md) .
* [Tapahtumien tarkasteleminen ja valvontalokit](insights-debugging-with-events.md) saat kaikki, joka on tapahtunut palvelussa.
* Voit selvittää, milloin Azure on ilmennyt suorituskyvyn heikkeneminen tai palvelun keskeytyksiä [seuraa palvelun kuntoa](insights-service-health.md) .
