<properties 
    pageTitle="Käynnistäminen streaming työt Stream Analytics | Microsoft Azure" 
    description="Suoritustapaa streaming työn Azure Stream Analytics | oppimiskeskuksen polku-osiossa."
    keywords="Streaming töitä"
    documentationCenter=""
    services="stream-analytics"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Azure Stream Analytics streaming projektin suorittaminen

Kun työ syöte, kyselyssä ja tulos kaikki määritettyjä voit aloittaa Stream Analytics-työ.

Työn käynnistäminen

1.  Azure perinteinen-portaalissa työn koontinäytöstä Valitse sivun alareunassa **Käynnistä-painiketta** .

    ![Aloita työ-painike](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Azure-portaalissa Valitse **Käynnistä** työ-sivun yläreunassa.

    ![Azure portaalin Käynnistä työn painike](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Määritä **Käynnistä tulosteen** arvo määrittää, milloin tämä työ alkaa Tulosta. Projektit, joita ei ole aiemmin aloitettu oletusarvo on **Työn aloitusaika**, mikä tarkoittaa, että työn alkaa heti käsittelytiedot. Voit myös määrittää **Mukautetut** aika (for muissa historiatietoja) aiemmin tai myöhemmin (Jos haluat määrittää käsittely tulevien saakka). Tapauksissa kun työ on alkanut ja keskeyttää, **Pysäytetty viimeksi** -vaihtoehto on käytettävissä jatkaminen tulosteen viimeksi työn ja tietojen menettämisen välttäminen.  

    ![Käynnistä streaming työn aika](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure portaalin Käynnistä streaming työn aika](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Vahvista valintasi. Työn tilaksi muuttuu *Aloitus* ja pian siirtää *käynnissä* kun työ on alkanut. Voit valvoa edistymistä **käynnistää** toiminnon **Ilmoitus-toiminnossa**:

    ![Projektin etenemisen-tietovirta](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![Projektin etenemisen-tietovirta Azure portal](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
