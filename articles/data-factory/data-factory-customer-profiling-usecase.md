<properties 
    pageTitle="Käyttötapaus - asiakkaan profilointi" 
    description="Lue, miten Azure Data Factory käytti tietoihin perustuvien työnkulun luominen (myyntijakso), profiilin gaming asiakkaille." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>Käyttötapaus - asiakkaan profilointi

Azure Data Factory on monia ratkaisu pikatoimintojen Cortana Liiketoimintatieto-ohjelmistopaketin käyttää palveluita.  Lisätietoja Cortanan liiketoimintatietojen käy [Cortanan liiketoimintatietojen Suite](http://www.microsoft.com/cortanaanalytics). Tässä asiakirjassa on kuvaavat yksinkertainen Käyttötapaus auttaa ymmärtämään, kuinka Azure Data Factory ongelmat voidaan ratkaista yleisiä analytics käytön aloittaminen.

Käyttö-ja kokeile tämän yksinkertaisen käyttötapaus on on [Azure tilauksen](https://azure.microsoft.com/pricing/free-trial/).  Voit ottaa käyttöön otoksen, joka sisältää tässä Käyttötapaus noudattamalla [näytteiden](data-factory-samples.md) artikkelissa kuvatulla tavalla.

## <a name="scenario"></a>Skenaario

Contoso on gaming yritys, joka luo visualisointi n useita ympäristöjen: pelien konsoliin, kannettava laitteiden ja tietokoneiden (tietokoneet). Kun pelaajat toistaa nämä visualisointi n, erittäin paljon tietoja on valmistettu, joka seuraa käyttötavat, gaming tyyli ja käyttäjän asetukset.  Kun yhdistetty demografiset, Aluekohtaiset- ja tuotetietojen, Contoso suorittaa analytics opas niitä siitä, miten voit parantaa pelaajat kokemus ja kohde ne päivityksistä ja Ottelu-ostaa. 

Contoson tavoite on tunnistaa sen pelaajat gaming historiatiedot perusteella ylös-myynti ja rajat-Tilausasiakas mahdollisuudet ja näyttävien toimintojen lisääminen asema business kasvu ja tarjota paremman käyttökokemuksen asiakkaille. Käytä tässä tapauksessa Käytämme gaming yrityksen esimerkkinä liiketoiminnan. Yrityksen haluaa optimoida sen visualisointi n pelaajat toimintatapojesi perusteella. Nämä periaatteet koskevat yritysten, jotka haluaa osallistuminen ympärille sen tuotteiden ja palvelujen asiakkaiden ja niiden asiakkaiden käyttökokemuksen parantamiseksi.

## <a name="challenges"></a>Haasteita


## <a name="solution-overview"></a>Ratkaisun yleiskatsaus

Esimerkki siitä, miten voit käyttää Azure Data Factory ingest, valmisteleminen, muunna, analysoida ja julkaista tietoja voidaan käyttää tämän yksinkertaisen käyttötapaus.

![Lopusta loppuun-työnkulku](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

Seuraavassa kuvassa tietojen putkistot näkymisen Azure-portaalissa sen jälkeen, kun ne on otettu käyttöön.

1.  **PartitionGameLogsPipeline** lukee raaka Ottelu tapahtumat-blob-säiliö ja luo osiot vuoden, kuukauden ja päivän perusteella.
2.  **EnrichGameLogsPipeline** yhdistää osioitua geo koodin viitetiedot Ottelu tapahtumat ja rikastaa tiedot määrittämällä IP-osoitteiden vastaavan geo sijainteihin.
3.  **AnalyzeMarketingCampaignPipeline** putkijohto käyttää lisätyt tiedot, ja käsittelee mainonta luomiseen, joka sisältää markkinoinnin markkinointikampanjan tehokkuuden lopputulos tiedoilla.

Tässä esimerkissä Data Factory käytetään orchestrate toimintoja, jotka syöttötiedot, muunna ja prosessien tietojen kopioiminen ja lopullinen tiedot Azure SQL-tietokantaan.  Voit myös Visualisoi tietoja putkistot verkon, hallita ja valvoa tilansa käyttöliittymässä.

## <a name="benefits"></a>Edut

Optimointi niiden käyttäjän profiiliin analytics ja tasaaminen ja tavoitteet, gaming yrityksen on nopeasti käyttötavat kerää ja analysoi sen markkinointikampanjan tehokkuuden.




