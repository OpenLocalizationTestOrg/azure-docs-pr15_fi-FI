<properties
    pageTitle="Määritä ilmoitukset kyselyjen Stream Analytics | Microsoft Azure"
    description="Tietoja Stream Analytics ilmoitat"
    keywords="ilmoitusten määrittäminen"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics työt ilmoitusten määrittäminen

## <a name="introduction-monitor-page"></a>Johdanto: Näyttö-sivu

Voit määrittää ilmoitusten käynnistettävän ilmoituksen, kun mittarin saavuttaa ehto, joka määritetään.

Esimerkiksi "tulosteen tapahtumien viimeinen 15 minuuttia, onko < 100, Lähetä sähköposti-ilmoituksen sähköpostitunnus: xyz@company.com”.

Säännöt voidaan määrittää arvot portaalin kautta, tai se voi olla määritetty [ohjelmallisesti](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) toimintojen lokit tietojen päälle.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Määritä ilmoitukset palvelun perinteinen Azure-portaalissa

Voit määrittää ilmoitusten Azure perinteinen portaalissa kahdella tavalla:  

1.  Virta Analytics työtäsi **Näyttö** -välilehti  
2.  Toimintojen lokin Management Services-palveluissa  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>Ilmoitus portaalissa työn näyttö-välilehdessä määrittäminen

1.  Valitse näyttö-välilehdessä lisätiedot Raporttinäkymät-ikkunan alareunassa **Lisää sääntö** -painiketta ja asennuksen säännöt.  

    ![Raporttinäkymät-ikkunan](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  Määritä nimi ja kuvaus ilmoitus  

    ![Säännön luominen](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Raja-arvot, ilmoitusten laskenta-ikkuna ja ilmoituksen toiminnot  

    ![Määritä ehdot](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>Määritä ilmoitukset – toimintojen lokitiedot

1.  Siirry [Azure perinteinen Portal](https://manage.windowsazure.com)hallintapalvelut **ilmoitukset** -välilehti.  
2.  Valitse **Lisää sääntö**  

    ![Ehdot](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Määritä nimi ja kuvaus ilmoitus. Valitse "Stream Analytics-palvelutyyppi ja palvelun nimi projektin nimi.  

    ![Ilmoituksen määrittäminen](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Määritä ilmoitukset Azure-portaalissa ##

Azure-portaalissa kiinnostavan Ilmoita Stream Analytics työn selaamalla ja valitse **Seuranta** -osa.  **Arvo** -sivu, joka avautuu Valitse **Lisää ilmoitus** -komento.

  ![Azure portal-asetukset](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Voit ilmoitusten säännön nimi ja valitse kuvaus, joka näkyy sähköposti-ilmoitus.

Kun valitset arvot Valitse ehto ja arvo raja arvo.

  ![Azure portaalin Valitse metrijärjestelmä](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Katso tarkemmin hälytysten määrittämisen Azure-portaalissa [Vastaanota ilmoitukset](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
