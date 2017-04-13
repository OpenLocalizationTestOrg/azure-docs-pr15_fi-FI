<properties 
    pageTitle="Reaaliaikainen tapahtuman käsittely Stream Analytics käsittely | Microsoft Azure" 
    description="Katso, miten joukko Azure palveluita voit yhteiskäyttö käyttöönoton reaaliaikainen käsittely ja analysoinnin." 
    keywords="Reaaliaikainen käsittely, käsittely, viittaus-arkkitehtuuri"
    services="stream-analytics,event-hubs,storage,sql-database" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="stream-analytics" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Viittaus arkkitehtuuri: Microsoft Azure Stream Analytics käsittely reaaliaikainen tapahtuma

Reaaliaikainen tapahtuman käsittely Azure Stream Analytics viittaus-arkkitehtuuri on tarkoitettu tarjoamaan yleinen Sinikopio reaaliaikainen ympäristön käyttöönottoon service (PaaS)-muodossa käsittely-ratkaisussa, jossa on Microsoft Azure nimellä.

## <a name="summary"></a>Yhteenveto

Analytics-ratkaisuista on perinteisesti perustana ominaisuudet, kuten ETL (Pura, muunto, kuormituksen) ja Tietovarastointi-analyysin ennen tietojen tallennussijainti. Muuttaminen vaatimukset, kuten nopeasti saapuville lisätietojen ovat valitseminen aiemmin tämän mallin rajoitus. Yksi ratkaisu on siirtäminen virtaa tallennustilan ennen tietojen analysoimisen ja samalla, kun se ei ole uusi ominaisuus, lähestymistapa ei ole yleisesti vahvistettu kaikki alan kohdennettuina yli. 

Microsoft Azure on analytics-toiminnot, jotka tukevat matriisin eri ratkaisu skenaariot ja vaatimukset pystyvät laaja luettelo. Lopusta loppuun-ratkaisun käyttöönotto Azure palveluiden valitseminen voi olla hankalaa annettu tarjouksia ulkopuolella. Tämä raportti on suunniteltu kuvaamaan ominaisuuksia ja erittäin luotettavaa eri Azure palveluja, jotka tukevat tapahtuman streaming ratkaisu. Se myös kerrotaan joistakin skenaariot, jossa asiakkaat hyötyvät tällaista lähestymistapa.

## <a name="contents"></a>Sisältö

- Johdon yhteenveto
- Reaaliaikainen Analytics esittely
- Arvo-ehdotus Azure reaaliaikaiset tiedot
- Yleisiä tilanteita, joissa reaaliaikainen Analytics
- Arkkitehtuuri ja osat
    - Tietolähteet
    - Tietojen integrointi kerros
    - Reaaliaikainen Analytics kerros
    - Tietoja tallennustilan kerros
    - Esityksen / kulutus kerros
- Tekemistä

**Tekijä:** Charles Feddersen-ratkaisun Architect tietojen havainnollistamisen keskelle huippuosaamisen, Microsoft Corporation

**Julkaistu:** Tammikuussa 2015

**Tarkistaminen:** 1.0

**Lataa:** [Reaaliaikainen tapahtuman Microsoft Azure Stream Analytics käsittely](http://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)


## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)

 
