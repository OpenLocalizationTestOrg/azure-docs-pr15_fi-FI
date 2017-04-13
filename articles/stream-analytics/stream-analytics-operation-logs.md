<properties 
    pageTitle="Korjaa-toiminnon ja palvelun kirjautuu Stream Analytics | Microsoft Azure" 
    description="Toimintaohjeet käytä Stream Analytics toimintojen lokit" 
    keywords="lokit"
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

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Virheenkorjaus Stream Analytics työt ja käytön tapahtumalokien avulla

Azure palvelun antaa käyttäjien hallinta toimintoihin liittyvät tietuetietoja toiminnallisia kirjaaminen viestejä. Azure Stream Analytics näitä tietoja voidaan käyttää vianmääritystä varten, kuten tarkasteleminen tilan, työn edistymistä ja sanomia seuraamaan työn edistymistä ajan myötä Aloita käsittelyyn, jos haluat siirtää.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Etsi toiminnon kirjautuu Azure hallinta-portaalissa

Toimintojen lokit niitä voi käyttää kahdella tavalla:  

- Raporttinäkymät-ikkunan Stream Analytics työn  
- Hallintapalvelut Azure perinteinen-portaalissa  

## <a name="dashboard-of-the-stream-analytics-job"></a>Raporttinäkymät-ikkunan Stream Analytics työn

Virta Analytics työn vastaavan lokit linkki tulee näkyviin projektin koontinäyttö-välilehti. Jos napsautat linkkiä, se asettaa suodattimet siten, että se näyttää, että tietty työ uusimman lokit.

  ![Valitse hallinta-palvelulokit](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>Hallintapalvelut

Siirry manuaalisesti toimintojen lokit Stream Analytics ja muita palveluja Azure perinteinen portaalissa:

1.  Valitse **Hallintapalvelut** [Azure perinteinen portal](https://manage.windowsazure.com).
2.  Valitse **Stream Analytics** **Palvelunimi**projektin nimi ja **tyyppi** .  

  ![Valitse Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Etsi valvontalokien Azure-portaalissa ##

Etsi toiminnallisia lokit Stream Analytics-työn Azure-portaalissa valitsemalla **Selaa** ja valitse sitten **valvontalokit**.

  ![Azure-portaali valitsemalla Stream Analytics](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Tämä avaa sivu, kaikkien resurssien viimeisen seitsemän päivän tapahtumat näkyvät tilauksen.  Voit suodattaa Nähdäksesi tapahtumat Määritä tyypin tai päivämäärävälin valitsemalla **Suodata** -komento.

  ![Azure-portaali valitsemalla Stream Analytics](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Hae lokitiedot

Voit suodattaa aikavälin ja tila, kun haluat tarkastella lokeja työtä varten.

Napsauta **tiedot** -painiketta voit tarkastella tarkempia tietoja valitun tapahtuman ikkunan alareunassa Azure hallinta-portaalin. 

  ![Valitse tiedot](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Napsauta Azure-portaalissa voit tarkastella yksityiskohtaisia tapahtumat sisällä tapahtuma.

  ![Azure-portaali valitsemalla tiedot](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Sieltä voit avata valitsemalla tapahtuman **tiedot** -sivu.

  ![Azure-portaali valitsemalla tiedot](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Epäonnistuneen työn korjaaminen

Valitse Azure hallinta-portaalin haku-kuvake ja kirjoita "epäonnistui". Näin ja tuloksena on kaikki lokit kanssa virheet. 

  ![Virheenkorjaus epäonnistunut työ](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Azure-portaalissa voit suodattaa viestin **Kriittinen** tapahtumien tarkasteleminen tason mukaan.

  ![Azure portaalin virheenkorjaus](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Voit valita minkä tahansa virheet ja sitten **tiedot** lisätietoja virheestä.  Virheviestejä tarjoavat myös tietoja siitä, miten pienentämään ongelman. 

  ![Toiminnon tiedot](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Tarvitset toimittaa ryhmän kautta [MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)tiedot ja ota yhteyttä [tukeen](https://azure.microsoft.com/support/options/) , Huomaa toiminnon tiedot, erityisesti **Korrelaatiotunnus**. 

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
