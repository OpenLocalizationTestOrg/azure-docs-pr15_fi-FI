<properties
    pageTitle="Lisää tietosyöte Stream Analytics-projekteille | Microsoft Azure"
    description="Lisätietoja tietolähteen Stream Analytics työsi kuin streaming blogin tallennustilan tietojen tapahtuman keskittimet tai viittaus tietosyöte yhdistää."
    keywords="tietoja syötteenä, streaming tiedot"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a>Streaming tietojen syöttö- tai tietojen lisääminen Stream Analytics-työ

Lisätietoja tietolähteen Stream Analytics työsi kuin streaming Blob-objektien tallennustilaan tietojen tapahtuman keskittimet tai viittaus tietosyöte yhdistää.

Azure Stream Analytics työt voidaan yhdistää tietosyöte yhden tai usean kukin ottavat määrittää yhteyden aiemmin luotuun tietolähteeseen. Kun tiedot on lähetetty kyseiseen tietolähteeseen, se on kulutettu Stream Analytics projektin ja reaaliajassa kuin streaming tietojen käsitteleminen. Virta Analytics on ensimmäisen luokan integrointi [Azure tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/) ja [Azure-Blob-säiliö](../storage/storage-dotnet-how-to-use-blobs.md) kuluessa sekä projektin tilauksen ulkopuolella.

Tässä artikkelissa on [Stream Analytics oppimispolku](/documentation/learning-paths/stream-analytics/)vaihe.

## <a name="data-input-streaming-data-and-reference-data"></a>Tiedot tarkistavan: Streaming tiedot ja viittaus

On eri kahdentyyppisiä syötteiden Stream Analytics: tietojen virtaa ja viitetiedot.

- **Tietoja virtaa**: Stream Analytics työt on sisällettävä vähintään yksi stream tietosyöte kulutettu ja muuntaa projektin. Azure-Blob-säiliö ja Azure tapahtuman keskittimet tuetaan stream syötteen tietolähteet. Azure tapahtuman keskittimet käytetään tapahtuman virtaa kerätään yhdistettyjen laitteiden ja palvelujen sovellukset. Azure-Blob-säiliö voi käyttää syötteen tietolähteeksi ingesting stream tiedot joukkona.  
- **Viitetiedot**: Stream Analytics tukee Aux syötteen kutsuttu viitetiedot toisen tyypin.  Tiedot ovat liikerata, eikä tiedoista on staattinen tai hidastaa muuttaminen.  Se yleensä käytetään look-ups ja korrelaatioita kanssa tietojen virtaa tehokkaan tietojoukon luomiseen.  Azure-Blob-säiliö ei tällä hetkellä vain tuetut syöttölähde viitetiedot.  

Voit lisätä syötteen Stream Analytics-työtä seuraavasti:

1. Azure-portaalissa **syötteiden** napsauttamalla ja valitse sitten **Lisää syöte** Stream Analytics töissä.

    ![Azure perinteinen portal - syötteen lisääminen.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  

    Valitse Azure portaalissa Stream Analytics työtäsi **syötteiden** -ruutu.  

    ![Azure portal - Lisää tiedot tarkistavan.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  

2. Määritä syötteen tyyppi: **tietovirta** tai **viitetiedot**.

    ![Lisää tiedot ovat oikein syöte, virtauttaa tai viittaus](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  

    ![Lisää tiedot ovat oikein syöte, virtauttaa tai viittaus](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  

3. Jos luot tietovirta syöte, Määritä syötteen lähteen tyyppi.  Tässä vaiheessa voit ohitetaan kuin vain Blob storage tukee tällä hetkellä viitetiedot luonnin aikana.

    ![Lisää stream tietosyöte](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  

    ![Lisää stream tietosyöte](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  

4. Anna kutsumanimi tämän syötteen syötteen Alias-kenttään.  Tätä nimeä käytetään oman työn kyselyn myöhemmin syötteen viitata.

    Täytä loput muodostaminen tietolähteen yhteyden ominaisuudet. Nämä kentät vaihdella lisätiedot ja lähteen tyyppi, ja se on määritetty yksityiskohtaisesti [tähän](stream-analytics-create-a-job.md).  

    ![Lisää tapahtuman keskittimeen tietosyöte](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  

5. Määritä syöttötiedot Sarjatoiminto asetukset:
    - Varmista, että kyselyissä tavalliseen tapaan odotuksiasi, Määritä saapuvat tiedot **Tapahtuman Sarjatoiminto muoto** .  Tuetut Sarjatoiminto muodot ovat JSON, CSV ja Avro.
    - Tarkista tietojen **koodaus** .  UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä.

    ![Sarjatoiminto Tietoasetukset syötteen tiedot](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  

    ![Sarjatoiminto Tietoasetukset syötteen tiedot](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  

6. Kun olet suorittanut syötteen luominen, Stream Analytics tarkistamaan, että se muodostaa yhteyden lähde.  Voit tarkastella Testiyhteys toiminnon tilan ilmoitus-toiminnossa.

    ![Testaa yhteys syötteen streaming tiedot](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  

    ![Testaa yhteys syötteen streaming tiedot](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a>Ohjeita streaming tietojen syötteiden
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
