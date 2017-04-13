<properties
    pageTitle="Viittaus tiedot ja haku taulukoiden käyttäminen Stream Analytics | Microsoft Azure"
    description="Käyttämällä viitetiedot Stream Analytics-kyselyssä"
    keywords="hakutaulukko, viitetiedot"
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

# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Virta-analyysin syöttö stream viittaus tietoja tai hakukentän taulukoiden käyttäminen

Viitetiedot (tunnetaan myös nimellä hakutaulukko) rajallinen tietojoukon, joka on staattinen tai hidastaa muuttaminen laatu, jota suorittaa haun tai yhdistää tietoja stream kanssa. Voit käyttää viitetiedot Azure Stream Analytics töissä, voit yleensä käyttää [Viittaus tietojen liittyä](https://msdn.microsoft.com/library/azure/dn949258.aspx) kyselyssä. Virta Analytics käyttää Azure-Blob-säiliö tallennustilan kerros viitetiedot ja Azure Data Factory viittauksen sisältävä tiedot voidaan muuntaa tai kopioida Azure-Blob-säiliö viitetiedot, käyttää [minkä tahansa cloud määrän perusteella](../data-factory/data-factory-data-movement-activities.md)ja paikallisen Microsoft Data. Viitetiedot mallintaa on nousevaan järjestykseen on määritetty Blob-objektien nimissä päivämäärä ja kellonaika (määritetty syötteen määritys) BLOB sarjana. Se tukee **vain** lisääminen jaksossa loppuun käyttämällä päivämäärä ja kellonaika **suurempi** kuin määritetty sarjan viimeinen Blob-objektien mukaan.

## <a name="configuring-reference-data"></a>Viitetiedot määrittäminen

Määrittäminen viittaus tiedot, sinun on syötteen, joka on tyyppiä **Viitetiedot**luomiseen. Seuraavassa taulukossa kerrotaan ominaisuuksista, joita tarvitset antamaan luotaessa viitetiedot, kirjoita sen kuvaus kanssa:

<table>
<tbody>
<tr>
<td>Ominaisuuden nimi</td>
<td>Kuvaus</td>
</tr>
<tr>
<td>Syötteen Alias</td>
<td>Kutsumanimi, jota käytetään viittaamaan tämän syötteen työn kysely.</td>
</tr>
<tr>
<td>Tallennustilan tilin</td>
<td>Jossa blob-tiedostot sijaitsevat tallennustilan tilin nimi. Jos se on sama työtäsi Stream Analytics-tilauksen, voit valita avattavasta alaspäin.</td>
</tr>
<tr>
<td>Tallennustilan tilin avain</td>
<td>Tallennustilan-tiliin liittyvät salausavaimen. Tämä saa tiedot automaattisesti, jos tallennustila-tili on sama työtäsi Stream Analytics-tilauksen.</td>
</tr>
<tr>
<td>Tallennustilan säilö</td>
<td>Säilöjen on tallennettu Microsoft Azure-Blob-palveluun BLOB looginen ryhmittely. Kun lataat blob Blob-palveluun, sinun on määritettävä kyseisen blob säilö.</td>
</tr>
<tr>
<td>Polku malli</td>
<td>Käytetään etsimiseen oman BLOB määritetyn säilöön tiedostopolku. PATH voit määrittää yhden tai useamman esiintymät seuraavat 2 muuttujat:<BR>{date}, {time}<BR>Esimerkki 1: products/{date}/{time}/product-list.csv<BR>Esimerkki 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Päivämäärämuoto [valinnainen]</td>
<td>Jos olet käyttänyt {date}, joka on määritetty polku-mallissa, voit valita päivämäärämuodon jossa tiedostojen järjestetään tuetut tiedostomuodot avattavasta luettelosta. Esimerkki: VVVV/KK/PP</td>
</tr>
<tr>
<td>Aikamuoto [valinnainen]</td>
<td>Jos olet käyttänyt {time}, joka on määritetty polku-mallissa, voit valita Aikamuoto, johon tiedostot on järjestetty tuetut tiedostomuodot avattavasta luettelosta. Esimerkki: HH</td>
</tr>
<tr>
<td>Tapahtuman Sarjatoiminto muoto</td>
<td>Varmista, että kyselyissä toimi odotetulla tavalla, Stream Analytics tarvitsee tietää, mitä Sarjatoiminto muotoa käytät saapuvien tietojen virtaa varten. Tuetut tiedostomuodot ovat viitetiedot, CSV ja JSON.</td>
</tr>
<tr>
<td>Koodaus</td>
<td>UTF-8 on ainoa tuettu koodauksen tiedostomuoto tällä hetkellä</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Viitetiedot aikataulun luominen

Jos viittaus tiedot ovat hitaasti muuttaminen tietojoukon-tuki viittaus määrittämällä syötteen konfigurointi käyttämällä {date} polku-malli on käytössä tietojen päivittäminen ja {aikaa} korvaaminen tunnusten. Virta Analytics noutaa päivitetyn viittaus tietojen määritelmien perusteella polku-malli. Esimerkiksi rakenteessa `sample/{date}/{time}/products.csv` päivämäärä-muodossa **"YYYY-MM-DD-** ja aika muodossa **"Hh"** ohjaa Stream Analytics päivitetyt Blob-objektien päiväkodista `sample/2015-04-16/17:30/products.csv` kello 5:30 huhtikuussa 2015 16 UTC-ajan vyöhykkeeseen.

> [AZURE.NOTE] Tällä hetkellä Stream Analytics työt Etsi blob-päivitykseen vain silloin, kun tietokoneen kellonaika siirrytään koodattu Blob-objektien nimissä aika. Esimerkiksi projektin etsii `sample/2015-04-16/17:30/products.csv` heti, kun mahdollista, mutta ei aiemmissa kuin 5:30 PM huhtikuussa 2015 16 UTC-ajan vyöhykkeeseen. Se *ei koskaan* Etsi tiedosto, jonka koodattu aikaa ennen viimeistä, joka on määritetty.
> 
> Esimerkiksi Kun työ löytää blob `sample/2015-04-16/17:30/products.csv` se ohittaa tiedostot on koodattu päivämäärän aiempi 5:30 PM huhtikuussa 16 2015 joten jos myöhässä saapuvat `sample/2015-04-16/17:25/products.csv` blob luodaan samaan erään työ ei käytä sitä.
> 
> Samoin jos `sample/2015-04-16/17:30/products.csv` on valmistettu vain 10:03 PM huhtikuussa 16 2015, mutta ei ole Blob-objektien kanssa aiemman päivämäärän ei sisällä tietoja säilössä, työn käyttää 10:03 PM huhtikuussa 16 2015 aloittaen tiedoston ja käyttää edellisen viitetiedot vasta sitten.
> 
> Poikkeus kun työ on uudelleen käsitellä tietoja taaksepäin tai kun työ on ensimmäinen aloitetaan. Käynnistyksen työn etsii valmistettu ennen työn viimeisimmän Blob-objektien alkamisaika määritetyn ajan. Voit varmistaa, etteivät **Tyhjä** viittaus tietojoukon työn käynnistyessä tehdään. Jos jokin ei löydy, työn näyttää seuraavat diagnostiikan: `Initializing input without a valid reference data blob for UTC time <start time>`.


[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) avulla voidaan orchestrate luomisesta vaatii Stream Analytics päivittämään viittaus määritykset päivitetyt BLOB-objektit. Tietoja Factory on pilvipohjainen tietojen integrointi-palvelu, orchestrates ja automatisoi siirto ja tietojen muunnos. Data Factory tukee [muodostettaessa paljon pilvipohjainen ja paikallisen tietojen stores](../data-factory/data-factory-data-movement-activities.md) ja siirtäminen tietojen helposti, jotka määrität säännöllisin väliajoin. Lisätietoja ja voit määrittää tietojen Factory putkijohto luomiseen viitetiedot Stream analysoinnissa, joka päivittää ennalta määritetyt aikataulun vaiheittainen ohjeita tutustu tämän [GitHub malli](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Vihjeitä viittaus-tietojen päivittäminen ##

1. Viittaus tietojen BLOB korvaaminen ei aiheuta Stream Analytics Lataa blob ja joissakin tapauksissa se voi aiheuttaa työ epäonnistuu. Suositeltu tapaa muuttaa viitetiedot on Lisää uusi Blob-objektien käyttäminen samassa säilö ja polku aiemmin määritetty työ-syöte ja päivämäärä ja kellonaika **suurempi** kuin määritetty mukaan sarjan viimeinen Blob-objektien käyttäminen.
2.  Viitetiedot blob ei ole vyöhykkeen Blob-objektien "Viimeksi muokattu" aikaa, mutta vain blob määritetyn päivämäärän ja kellonajan mukaan nimeä, käyttämällä {date} ja {aika} korvaus.
3.  Työ on palata aika muutaman kerran vuoksi viittaus tietojen BLOB on ei muutettu tai poistettu.

## <a name="get-help"></a>Ohjeiden saaminen
Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet
Sinun on otettu käyttöön Stream Analytics hallitun palvelun streaming analytics asioita Internet-tietojen perusteella. Lisätietoja tämän palvelun on seuraavissa artikkeleissa:

- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
