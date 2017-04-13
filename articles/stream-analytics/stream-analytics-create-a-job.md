<properties 
    pageTitle="Luomisesta Stream analysoinnissa tietojen analytics käsittely työn | Microsoft Azure" 
    description="Luo Stream analysoinnissa tietojen analytics käsittely työ | oppimiskeskuksen polku-osiossa."
    keywords="tietojen analyysin käsitteleminen"
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

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Tietoja analytics käsittely työn luomisesta Stream analysoinnissa

Azure Stream Analytics ylimmän tason resurssin on Stream Analytics työ.  Se koostuu vähintään yhden syötteen tietolähteitä, kyselyn ilmoittaminen muuntamiseen ja tulostus-kohteet, jotka tulokset kirjoitetaan. Yhdessä nämä avulla käyttäjä voi suorittaa tietojen analytics streaming tietojen käsittelyyn.

Voit käyttää muodossa Analytics Aloita luomalla uusi Stream Analytics-työ.  Huomioi, että tämä toiminto ei ole laskutuksen vaikutukset, kunnes työ on alkanut.

1.  Kirjaudu sisään Onlinessa [Azure perinteinen portal](http://manage.windowsazure.com) tai [Azure portal](https://portal.azure.com/).
2.  Portaalissa: **Valitse uusi**, valitse **Tietopalvelut** tai **Tietojen Analytics** -portaalin mukaan ja valitse sitten **Azure Stream Analytics** tai **Stream Analytics** ja sitten **Nopea luominen**.

    ![Ohjattu työn tietojen analyysin käsitteleminen](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Luo tietojen analytics projektin käsitteleminen](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Määritä uudet määritykset Stream Analytics projektille.
    - Kirjoita **Projektin nimi** -ruutuun nimi, joka yksilöi Stream Analytics-työ. Kun **Projektin nimi** on tarkistettu, vihreä valintamerkki tulee näkyviin projektin nimi-ruutuun. **Työn nimi** voi olla vain aakkosnumeerisia merkkejä ja '-' merkki ja on oltava 3-63 merkkiä.
    - **Alueen** Azure portal tai **sijainti** Azure-portaalissa Määritä käyttämällä maantieteellisen sijainnin, jonka haluat suorittaa työn.
    - Jos Azure portaalin, valitse tai luo tallennustilan-tili, jota käytetään **Alueellisen seuranta tallennustilan tilin**. Tallennustilan tilin voidaan tallentaa kaikki tämän alueen töitä Stream Analytics seurantatiedot.
    - Jos käytät Azure-portaaliin, Määritä uusi tai aiemmin luotu **Resurssiryhmä** pitoon sovelluksen liittyvät resurssit.

4.  Kun uusi Stream Analytics työn asetukset on määritetty, valitse **Luo Stream Analytics työn**. Voi kestää muutaman minuutin kuluttua Stream Analytics luominen projektille. Tarkista tila, voit valvoa edistymistä ilmoitukset-toiminnossa.

    ![Tietoja analytics käsittely työn notfications-toiminnossa](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure portaalin tietojen analytics käsittelyn projektin työn luominen](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Uusi projekti näytetään tila on **luotu**. Huomaa, että **Käynnistä** -painike ei ole käytössä. Työn syöte, kyselyjen ja tulos on määritettävä, ennen kuin aloitat työn.

    ![Tietojen analyysin käsitteleminen työn tila](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure portaalin tietojen analytics käsittelyn työn projektin tila](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
