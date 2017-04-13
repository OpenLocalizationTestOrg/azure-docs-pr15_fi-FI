<properties 
    pageTitle="Viestin kirjoittaminen kyselyjen Stream Analytics | Microsoft Azure" 
    description="Kyselyjen kirjoittaminen Stream analyysin ja kyselyn tiedot | oppimiskeskuksen polku-osiossa."
    keywords="Voit kirjoittaa kyselyt-kyselyn tiedot, Kirjoita kyselyn kirjoittamisen kyselyt"
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

# <a name="how-to-write-queries-in-stream-analytics"></a>Viestin kirjoittaminen Stream Analytics kyselyt

Kirjoittaminen kyselyjen käsittely Azure Stream Analytics funktioiden stream on toteutettu "pysyvän kysely", joka on määritetty, ennen kuin työn käynnistyy ja suorittaa tietojen ohjausobjekti työn. Tietojen muuntamista ilmaistaan kaltaisessa SQL-kyselyn kielellä, joka on suurelta alijoukkoa T-SQL-tunniste on joitakin lisätyn kielen kuin [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) käytettävä express ajallinen semantiikkaan liittyvien.

## <a name="writing-queries"></a>Kyselyjen toimimalla seuraavasti: ##

1. Virta Analytics töissä Azure hallinta-portaalissa Valitse **kysely**.

    ![Valitse kysely](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Valitse **kyselyn**Azure-portaalissa.

    ![Valitse kyselyn esikatselu](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Uusia töitä on kyselyn mallin avulla pääset alkuun. Kyselyn mallin suorittaa projektien kaikki kentät "Jos" kysely syötteen tapahtumien tulosteen kyselyjä.  

    - Jos olet määrittänyt vähintään yhden syötteen ja tuloste työlle, voit korvata paikkamerkin "[YourOutputAlias]" ja "[YourInputAlias]"-kentät, joilla on aliases syötteen ja tulosteen, jotka haluat käyttää ensin. Lisäksi voit edelleen tekijän ja kyselyn testaaminen Azure perinteinen portaalissa määrittämättä syötteiden ja tulostaa työn.
    - Jos haluat suorittaa enemmän kuin yksinkertainen läpivienti käsittely, voit muokata kyselymääritystä. Aloita kyselyn yhtä aikaa muiden kanssa, tutustu joitakin yleisiä kyselyn kuviot välitetyt [tähän](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Kyselyn tiedot-ikkuna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Kyselyn tietojen kelpoisuuden toimii: ##

Voit testata, että kysely toimii oikein, suorittamalla selaimessa, vähintään yksi paikallisen JSON tiedostot, jotka sisältävät testitiedot päälle. Tämä vaihtoehto Aloita työ tai laskutuksen vaikuttaa.

> [AZURE.NOTE] Tällä hetkellä selaimessa kyselyn testaaminen ei tue Azure-portaalissa.  

1.  Varmista, että (muussa tapauksessa painike eivät ole käytettävissä) kyselyssä on virheitä ja valitse sitten testi-painiketta.  

    ![Kyselyn tietojen testi](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Voit pyydetään määrittämään kunkin Viitattu kyselyn syötteiden tiedostot. Tässä esimerkissä mallin kyselyn jätetään nimellä-on-valintaikkunassa on tietojen kysely nimeltä "yourinputalias".  

    ![Testaa tietojen kysely](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Siirry testitiedosto. Useita mallitiedostojen ovat käytettävissä [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) ja mallitiedot voit myös noutaa Omat tiedot stream syötteiden mallitiedot funktion syötteiden-välilehden kautta.  

    ![Kyselyn syöte](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  Sulje valintaikkuna kysely suoritetaan testi tietojen päälle ja näet tulokset kysely-sivun alareunassa.  

    ![Kyselyn yhteenveto](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
