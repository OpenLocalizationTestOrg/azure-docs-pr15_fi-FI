<properties 
   pageTitle="Virta Analytics rajoitukset taulukko"
   description="Tässä artikkelissa kuvataan järjestelmän rajoitukset ja suositellut koot Stream Analytics-komponenttien ja yhteydet."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Raja-tunniste | Raja       | Kommentit |
|----------------- | ------------|--------- |
| Tilauskohtaisten alueittain Streaming yksiköiden enimmäismäärä | 50 | Pyyntö niin, että streaming yksiköt tilauksen 50 lisäksi voidaan luoda ottamalla yhteyttä [Microsoftin tuotetukeen](https://support.microsoft.com/en-us). |
| Suurin nopeus Streaming yksikön | 1 Megatavu / s * | Suurin nopeus kohti SU määräytyy skenaariota. Todellinen nopeus voi heiketä ja riippuu kyselyn monimutkaisuutta ja jakaminen. Lisätietoja löytyy [asteikko Azure Stream Analytics työt niin, että nopeus](../articles/stream-analytics/stream-analytics-scale-jobs.md) -artikkelissa. |
| Syötteiden projektia kohti enimmäismäärä | 60 | On kiinteä rajoitus on 60 syötteiden Stream Analytics projektia kohti. |
| Tulostaa projektia kohti enimmäismäärä | 60 | On kiinteä rajoitus on 60 tulostaa Stream Analytics projektia kohti. |
| Enimmäismäärä Funktiot projektia kohti | 60 | On kiinteä rajoitus 60 funktioiden Stream Analytics projektia kohti. |
| Töiden alueittain enimmäismäärä | 1500 | Jokaisen tilauksen voi olla enintään 1500 työt maantieteellinen alue. |