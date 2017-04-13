<properties 
    pageTitle="Tietojen määrittäminen tulostaa Stream Analytics töiden | Microsoft Azure" 
    description="Määritä tulostus Stream Analytics töiden | oppimiskeskuksen polku-osiossa."
    keywords="tietoja siirretään, tietojen siirto"
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

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Virta Analytics töiden tulostaa tietojen määrittäminen

Azure Stream Analytics työt voidaan yhdistää yhden tai useamman tietojen tulostus, jotka määrittävät yhteyden aiemmin tiedot-käsittelytoiminto. Stream Analytics-työ käsittelee ja muuntaa saapuvat tiedot, stream tietojen tulosteen tapahtumat kirjoitetaan oman työn tulos.

Virta Analytics-tietojen avulla voidaan tulostaa tietolähteen reaaliaikainen raporttinäkymien tai ilmoitukset-tietojen siirto-työnkulkujen käynnistys tai arkistoida yksinkertaisesti erä käsittelyä myöhemmin koskevia tietoja. Virta Analytics on ensimmäisen luokan integrointi useita Azure services, joka on kuvattu seuraavassa yksityiskohtaiset tiedot.

Voit lisätä tulos Stream Analytics työtäsi seuraavasti:

1. Azure perinteinen-portaalissa Valitse **tulostus** ja valitse sitten **Lisää tulosteen** Stream Analytics-työ.

    ![Lisää tulostus](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Valitse Azure portaalissa Stream Analytics työtäsi **tulostus** -ruutu.

    ![Azure Porta Lisää tulostus](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Määritä tulos:

    ![Valitse siirto-tietotyyppi](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure portal Valitse siirto-tietotyyppi](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Anna kutsumanimi tämän tulosteen **Tulosteen Alias** -kenttään. Tämä nimi voidaan oman työn kyselyn myöhemmin viittaamaan tulos.  
    
    Täytä loput jaettavaan muodostaa yhteyden ominaisuudet.  Nämä kentät vaihtelevat tulosteen tyypin mukaan, ja se on määritetty tässä yksityiskohtaisesti.  

    ![Lisää tietoja tulostus-ominaisuus](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. Voit joutua Määritä, kuinka tiedot muuntaa sarjaksi tai muotoiltu tulostus-tyypin mukaan. Tietyn Sarjatoiminto asetukset tulosteen mistäkin on kuvattu seuraavassa.

    Täytä loput muodostaminen tietolähteen yhteyden ominaisuudet. Nämä kentät vaihdella lisätiedot ja lähteen tyyppi, ja se on määritetty yksityiskohtaisesti [tähän](stream-analytics-create-a-job.md).  

    ![Lisää tietoja tulosteen tapahtumaa-toiminnossa](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Azure portaalin tietojen tulosteen tapahtumaa-toiminnossa](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Työn lisätään tulosteen elementtinä on luotava ennen työ on käynnissä ja tapahtumien Käynnistä etenee. Esimerkiksi jos käytät Blob-objektien tallennustilaan tulos, työn ei luo tallennustilan tilin automaattisesti. Se on luotava käyttäjän, ennen kuin ASA työ on alkanut.

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
