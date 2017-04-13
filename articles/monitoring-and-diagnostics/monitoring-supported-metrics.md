<properties
    pageTitle="Valvonta-arvot Azure - tuetut arvot resurssilaji per | Microsoft Azure"
    description="Resurssin mistäkin Azure näytön kanssa käytettävissä olevat arvot luettelo."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="supported-metrics-with-azure-monitor"></a>Tuetut arvot Azure näytön kanssa

Azure näytön tarjoaa useita tapoja käsitellä arvot, kuten kaavioiden ne portaalissa ja käyttää niitä REST API-Liittymän kautta sekä kyselyt ne PowerShell tai CLI avulla. Alla on luettelo kaikista kaikki arvot Azure näytön metrisillä myyntijakso tällä hetkellä käytettävissä.

> [AZURE.NOTE] Muut arvot ole käytettävissä portaalin tai käyttämällä vanha API. Tässä luettelossa on vain julkinen esikatselu arvot käytettävissä kootut Azure näytön metrisillä myyntijakso julkisen esikatselun käyttäminen.

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|CoreCount|Core määrä|Laske|Kokonaissumma|Kokonaismäärän sydämiä erä-tilille|
|TotalNodeCount|Solmun määrä|Laske|Kokonaissumma|Erän tilin solmujen määrä|
|CreatingNodeCount|Solmun Laske luominen|Laske|Kokonaissumma|Luotavan solmujen määrän|
|StartingNodeCount|Solmun Laske käynnistäminen|Laske|Kokonaissumma|Aloitetaan solmujen määrän|
|WaitingForStartTaskNodeCount|Odottaa, Käynnistä tehtävien solmu määrä|Laske|Kokonaissumma|Aloita tehtävä suorittamiseen odotetaan solmujen määrän|
|StartTaskFailedNodeCount|Käynnistä tehtävän epäonnistui solmu määrä|Laske|Kokonaissumma|Jos Aloita tehtävä on epäonnistunut solmujen määrän|
|IdleNodeCount|Käyttämättömänä solmu määrä|Laske|Kokonaissumma|Käyttämättömänä solmujen määrän|
|OfflineNodeCount|Offline-tilassa solmu määrä|Laske|Kokonaissumma|Offline-tilassa solmujen määrän|
|RebootingNodeCount|Uudelleenkäynnistyksen solmu määrä|Laske|Kokonaissumma|Uudelleenkäynnistyksen solmujen määrän|
|ReimagingNodeCount|Reimaging solmu määrä|Laske|Kokonaissumma|Reimaging solmujen määrän|
|RunningNodeCount|Käynnissä solmu määrä|Laske|Kokonaissumma|Käynnissä solmujen määrän|
|LeavingPoolNodeCount|Jätä resurssivarantoon solmu määrä|Laske|Kokonaissumma|Jätä altaan solmujen määrän|
|UnusableNodeCount|Ei käytettävissä solmu määrä|Laske|Kokonaissumma|Ei käytettävissä solmujen määrän|
|TaskStartEvent|Tehtävän alkamis-tapahtumat|Laske|Kokonaissumma|Tehtävät, jotka on aloitettu kokonaismäärä|
|TaskCompleteEvent|Tehtävä valmis tapahtumat|Laske|Kokonaissumma|Suoritettujen tehtävien määrä|
|TaskFailEvent|Tehtävän virheiden tapahtumat|Laske|Kokonaissumma|Epäonnistuneiden tilaan suoritettujen tehtävien määrä|
|PoolCreateEvent|Resurssivarannon luominen tapahtumat|Laske|Kokonaissumma|Kokonaismäärän jakavat, jotka on luotu|
|PoolResizeStartEvent|Resurssivarannon koon Käynnistä tapahtumat|Laske|Kokonaissumma|Resurssivarannon koko muuttuu, jotka on aloitettu kokonaismäärä|
|PoolResizeCompleteEvent|Resurssivarannon koon valmis tapahtumat|Laske|Kokonaissumma|Kokonaismäärän resurssivarantoon koko muuttuu, joka on valmis|
|PoolDeleteStartEvent|Ryhmän poistaminen Käynnistä tapahtumat|Laske|Kokonaissumma|Kokonaismäärän resurssivarantoon poistaa, jotka on aloitettu|
|PoolDeleteCompleteEvent|Ryhmän poistaminen valmis tapahtumat|Laske|Kokonaissumma|Kokonaismäärän resurssivarantoon poistaa, joka on valmis|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|connectedclients|Asiakkaiden|Laske|Enimmäismäärä||
|totalcommandsprocessed|Toimintojen summa|Laske|Kokonaissumma||
|cachehits|Osumia|Laske|Kokonaissumma||
|cachemisses|Välimuistin epäonnistumisia|Laske|Kokonaissumma||
|getcommands|Saa|Laske|Kokonaissumma||
|setcommands|Joukot|Laske|Kokonaissumma||
|evictedkeys|Poistaa näppäimet|Laske|Kokonaissumma||
|totalkeys|Yhteensä näppäimet|Laske|Enimmäismäärä||
|expiredkeys|Vanhentuneen näppäimet|Laske|Kokonaissumma||
|usedmemory|Käytetty muistin|Tavua|Enimmäismäärä||
|usedmemoryRss|Käytetty muistin RSS|Tavua|Enimmäismäärä||
|serverLoad|Lataa palvelimeen|Prosenttia|Enimmäismäärä||
|cacheWrite|Välimuistin kirjoittaminen|BytesPerSecond|Enimmäismäärä||
|cacheRead|Välimuistin luku|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime|SUORITIN|Prosenttia|Enimmäismäärä||
|connectedclients0|Asiakkaiden (Shard 0)|Laske|Enimmäismäärä||
|totalcommandsprocessed0|Toimintojen summa (Shard 0)|Laske|Kokonaissumma||
|cachehits0|Osumia (Shard 0)|Laske|Kokonaissumma||
|cachemisses0|Välimuistin epäonnistumisia (Shard 0)|Laske|Kokonaissumma||
|getcommands0|Saa (Shard 0)|Laske|Kokonaissumma||
|setcommands0|Määrittää (Shard 0)|Laske|Kokonaissumma||
|evictedkeys0|Poistaa avaimet (Shard 0)|Laske|Kokonaissumma||
|totalkeys0|Yhteensä näppäimet (solmu 0)|Laske|Enimmäismäärä||
|expiredkeys0|Vanhentuneen näppäimet (Shard 0)|Laske|Kokonaissumma||
|usedmemory0|Käytetty muisti (Shard 0)|Tavua|Enimmäismäärä||
|usedmemoryRss0|Käytetty muistin RSS (Shard 0)|Tavua|Enimmäismäärä||
|serverLoad0|Lataa palvelimeen (Shard 0)|Prosenttia|Enimmäismäärä||
|cacheWrite0|Kirjoita välimuistin (Shard 0)|BytesPerSecond|Enimmäismäärä||
|cacheRead0|Välimuistin luku (Shard 0)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime0|Suoritin (Shard 0)|Prosenttia|Enimmäismäärä||
|connectedclients1|Asiakkaiden (Shard 1)|Laske|Enimmäismäärä||
|totalcommandsprocessed1|Toimintojen summa (Shard 1)|Laske|Kokonaissumma||
|cachehits1|Osumia (Shard 1)|Laske|Kokonaissumma||
|cachemisses1|Välimuistin epäonnistumisia (Shard 1)|Laske|Kokonaissumma||
|getcommands1|Saa (Shard 1)|Laske|Kokonaissumma||
|setcommands1|Määrittää (Shard 1)|Laske|Kokonaissumma||
|evictedkeys1|Poistaa avaimet (Shard 1)|Laske|Kokonaissumma||
|totalkeys1|Yhteensä näppäimet (solmu 1)|Laske|Enimmäismäärä||
|expiredkeys1|Vanhentuneen näppäimet (Shard 1)|Laske|Kokonaissumma||
|usedmemory1|Käytetty muisti (Shard 1)|Tavua|Enimmäismäärä||
|usedmemoryRss1|Käytetty muistin RSS (Shard 1)|Tavua|Enimmäismäärä||
|serverLoad1|Lataa palvelimeen (Shard 1)|Prosenttia|Enimmäismäärä||
|cacheWrite1|Kirjoita välimuistin (Shard 1)|BytesPerSecond|Enimmäismäärä||
|cacheRead1|Välimuistin luku (Shard 1)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime1|Suoritin (Shard 1)|Prosenttia|Enimmäismäärä||
|connectedclients2|Asiakkaiden (Shard 2)|Laske|Enimmäismäärä||
|totalcommandsprocessed2|Toimintojen summa (Shard 2)|Laske|Kokonaissumma||
|cachehits2|Osumia (Shard 2)|Laske|Kokonaissumma||
|cachemisses2|Välimuistin epäonnistumisia (Shard 2)|Laske|Kokonaissumma||
|getcommands2|Saa (Shard 2)|Laske|Kokonaissumma||
|setcommands2|Määrittää (Shard 2)|Laske|Kokonaissumma||
|evictedkeys2|Poistaa avaimet (Shard 2)|Laske|Kokonaissumma||
|totalkeys2|Yhteensä näppäimet (solmu 2)|Laske|Enimmäismäärä||
|expiredkeys2|Vanhentuneen näppäimet (Shard 2)|Laske|Kokonaissumma||
|usedmemory2|Käytetty muisti (Shard 2)|Tavua|Enimmäismäärä||
|usedmemoryRss2|Käytetty muistin RSS (Shard 2)|Tavua|Enimmäismäärä||
|serverLoad2|Lataa palvelimeen (Shard 2)|Prosenttia|Enimmäismäärä||
|cacheWrite2|Kirjoita välimuistin (Shard 2)|BytesPerSecond|Enimmäismäärä||
|cacheRead2|Välimuistin luku (Shard 2)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime2|Suoritin (Shard 2)|Prosenttia|Enimmäismäärä||
|connectedclients3|Asiakkaiden (Shard 3)|Laske|Enimmäismäärä||
|totalcommandsprocessed3|Toimintojen summa (Shard 3)|Laske|Kokonaissumma||
|cachehits3|Osumia (Shard 3)|Laske|Kokonaissumma||
|cachemisses3|Välimuistin epäonnistumisia (Shard 3)|Laske|Kokonaissumma||
|getcommands3|Saa (Shard 3)|Laske|Kokonaissumma||
|setcommands3|Määrittää (Shard 3)|Laske|Kokonaissumma||
|evictedkeys3|Poistaa avaimet (Shard 3)|Laske|Kokonaissumma||
|totalkeys3|Yhteensä näppäimet (solmu 3)|Laske|Enimmäismäärä||
|expiredkeys3|Vanhentuneen näppäimet (Shard 3)|Laske|Kokonaissumma||
|usedmemory3|Käytetty muisti (Shard 3)|Tavua|Enimmäismäärä||
|usedmemoryRss3|Käytetty muistin RSS (Shard 3)|Tavua|Enimmäismäärä||
|serverLoad3|Lataa palvelimeen (Shard 3)|Prosenttia|Enimmäismäärä||
|cacheWrite3|Kirjoita välimuistin (Shard 3)|BytesPerSecond|Enimmäismäärä||
|cacheRead3|Välimuistin luku (Shard 3)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime3|Suoritin (Shard 3)|Prosenttia|Enimmäismäärä||
|connectedclients4|Asiakkaiden (Shard 4)|Laske|Enimmäismäärä||
|totalcommandsprocessed4|Toimintojen summa (Shard 4)|Laske|Kokonaissumma||
|cachehits4|Osumia (Shard 4)|Laske|Kokonaissumma||
|cachemisses4|Välimuistin epäonnistumisia (Shard 4)|Laske|Kokonaissumma||
|getcommands4|Saa (Shard 4)|Laske|Kokonaissumma||
|setcommands4|Määrittää (Shard 4)|Laske|Kokonaissumma||
|evictedkeys4|Poistaa avaimet (Shard 4)|Laske|Kokonaissumma||
|totalkeys4|Yhteensä näppäimet (solmu 4)|Laske|Enimmäismäärä||
|expiredkeys4|Vanhentuneen näppäimet (Shard 4)|Laske|Kokonaissumma||
|usedmemory4|Käytetty muisti (Shard 4)|Tavua|Enimmäismäärä||
|usedmemoryRss4|Käytetty muistin RSS (Shard 4)|Tavua|Enimmäismäärä||
|serverLoad4|Lataa palvelimeen (Shard 4)|Prosenttia|Enimmäismäärä||
|cacheWrite4|Kirjoita välimuistin (Shard 4)|BytesPerSecond|Enimmäismäärä||
|cacheRead4|Välimuistin luku (Shard 4)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime4|Suoritin (Shard 4)|Prosenttia|Enimmäismäärä||
|connectedclients5|Asiakkaiden (Shard 5)|Laske|Enimmäismäärä||
|totalcommandsprocessed5|Toimintojen summa (Shard 5)|Laske|Kokonaissumma||
|cachehits5|Osumia (Shard 5)|Laske|Kokonaissumma||
|cachemisses5|Välimuistin epäonnistumisia (Shard 5)|Laske|Kokonaissumma||
|getcommands5|Saa (Shard 5)|Laske|Kokonaissumma||
|setcommands5|Määrittää (Shard 5)|Laske|Kokonaissumma||
|evictedkeys5|Poistaa avaimet (Shard 5)|Laske|Kokonaissumma||
|totalkeys5|Yhteensä näppäimet (solmu 5)|Laske|Enimmäismäärä||
|expiredkeys5|Vanhentuneen näppäimet (Shard 5)|Laske|Kokonaissumma||
|usedmemory5|Käytetty muisti (Shard 5)|Tavua|Enimmäismäärä||
|usedmemoryRss5|Käytetty muistin RSS (Shard 5)|Tavua|Enimmäismäärä||
|serverLoad5|Lataa palvelimeen (Shard 5)|Prosenttia|Enimmäismäärä||
|cacheWrite5|Kirjoita välimuistin (Shard 5)|BytesPerSecond|Enimmäismäärä||
|cacheRead5|Välimuistin luku (Shard 5)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime5|Suoritin (Shard 5)|Prosenttia|Enimmäismäärä||
|connectedclients6|Asiakkaiden (Shard 6)|Laske|Enimmäismäärä||
|totalcommandsprocessed6|Toimintojen summa (Shard 6)|Laske|Kokonaissumma||
|cachehits6|Osumia (Shard 6)|Laske|Kokonaissumma||
|cachemisses6|Välimuistin epäonnistumisia (Shard 6)|Laske|Kokonaissumma||
|getcommands6|Saa (Shard 6)|Laske|Kokonaissumma||
|setcommands6|Määrittää (Shard 6)|Laske|Kokonaissumma||
|evictedkeys6|Poistaa avaimet (Shard 6)|Laske|Kokonaissumma||
|totalkeys6|Yhteensä näppäimet (6 solmu)|Laske|Enimmäismäärä||
|expiredkeys6|Vanhentuneen näppäimet (Shard 6)|Laske|Kokonaissumma||
|usedmemory6|Käytetty muisti (Shard 6)|Tavua|Enimmäismäärä||
|usedmemoryRss6|Käytetty muistin RSS (Shard 6)|Tavua|Enimmäismäärä||
|serverLoad6|Lataa palvelimeen (Shard 6)|Prosenttia|Enimmäismäärä||
|cacheWrite6|Kirjoita välimuistin (Shard 6)|BytesPerSecond|Enimmäismäärä||
|cacheRead6|Välimuistin luku (Shard 6)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime6|Suoritin (Shard 6)|Prosenttia|Enimmäismäärä||
|connectedclients7|Asiakkaiden (Shard 7)|Laske|Enimmäismäärä||
|totalcommandsprocessed7|Toimintojen summa (Shard 7)|Laske|Kokonaissumma||
|cachehits7|Osumia (Shard 7)|Laske|Kokonaissumma||
|cachemisses7|Välimuistin epäonnistumisia (Shard 7)|Laske|Kokonaissumma||
|getcommands7|Saa (Shard 7)|Laske|Kokonaissumma||
|setcommands7|Määrittää (Shard 7)|Laske|Kokonaissumma||
|evictedkeys7|Poistaa avaimet (Shard 7)|Laske|Kokonaissumma||
|totalkeys7|Yhteensä näppäimet (solmu 7)|Laske|Enimmäismäärä||
|expiredkeys7|Vanhentuneen näppäimet (Shard 7)|Laske|Kokonaissumma||
|usedmemory7|Käytetty muisti (Shard 7)|Tavua|Enimmäismäärä||
|usedmemoryRss7|Käytetty muistin RSS (Shard 7)|Tavua|Enimmäismäärä||
|serverLoad7|Lataa palvelimeen (Shard 7)|Prosenttia|Enimmäismäärä||
|cacheWrite7|Kirjoita välimuistin (Shard 7)|BytesPerSecond|Enimmäismäärä||
|cacheRead7|Välimuistin luku (Shard 7)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime7|Suoritin (Shard 7)|Prosenttia|Enimmäismäärä||
|connectedclients8|Asiakkaiden (Shard 8)|Laske|Enimmäismäärä||
|totalcommandsprocessed8|Toimintojen summa (Shard 8)|Laske|Kokonaissumma||
|cachehits8|Osumia (Shard 8)|Laske|Kokonaissumma||
|cachemisses8|Välimuistin epäonnistumisia (Shard 8)|Laske|Kokonaissumma||
|getcommands8|Saa (Shard 8)|Laske|Kokonaissumma||
|setcommands8|Määrittää (Shard 8)|Laske|Kokonaissumma||
|evictedkeys8|Poistaa avaimet (Shard 8)|Laske|Kokonaissumma||
|totalkeys8|Yhteensä näppäimet (solmu 8)|Laske|Enimmäismäärä||
|expiredkeys8|Vanhentuneen näppäimet (Shard 8)|Laske|Kokonaissumma||
|usedmemory8|Käytetty muisti (Shard 8)|Tavua|Enimmäismäärä||
|usedmemoryRss8|Käytetty muistin RSS (Shard 8)|Tavua|Enimmäismäärä||
|serverLoad8|Lataa palvelimeen (Shard 8)|Prosenttia|Enimmäismäärä||
|cacheWrite8|Kirjoita välimuistin (Shard 8)|BytesPerSecond|Enimmäismäärä||
|cacheRead8|Välimuistin luku (Shard 8)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime8|Suoritin (Shard 8)|Prosenttia|Enimmäismäärä||
|connectedclients9|Asiakkaiden (Shard 9)|Laske|Enimmäismäärä||
|totalcommandsprocessed9|Toimintojen summa (Shard 9)|Laske|Kokonaissumma||
|cachehits9|Osumia (Shard 9)|Laske|Kokonaissumma||
|cachemisses9|Välimuistin epäonnistumisia (Shard 9)|Laske|Kokonaissumma||
|getcommands9|Saa (Shard 9)|Laske|Kokonaissumma||
|setcommands9|Määrittää (Shard 9)|Laske|Kokonaissumma||
|evictedkeys9|Poistaa avaimet (Shard 9)|Laske|Kokonaissumma||
|totalkeys9|Yhteensä näppäimet (solmu 9)|Laske|Enimmäismäärä||
|expiredkeys9|Vanhentuneen näppäimet (Shard 9)|Laske|Kokonaissumma||
|usedmemory9|Käytetty muisti (Shard 9)|Tavua|Enimmäismäärä||
|usedmemoryRss9|Käytetty muistin RSS (Shard 9)|Tavua|Enimmäismäärä||
|serverLoad9|Lataa palvelimeen (Shard 9)|Prosenttia|Enimmäismäärä||
|cacheWrite9|Kirjoita välimuistin (Shard 9)|BytesPerSecond|Enimmäismäärä||
|cacheRead9|Välimuistin luku (Shard 9)|BytesPerSecond|Enimmäismäärä||
|percentProcessorTime9|Suoritin (Shard 9)|Prosenttia|Enimmäismäärä||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|NumberOfCalls|API kutsujen määrä|Laske|Kokonaissumma|API-kutsujen määrä.|
|NumberOfFailedCalls|Epäonnistuneiden API kutsujen määrä|Laske|Kokonaissumma|Epäonnistuneiden API-kutsujen määrä.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|Prosentin Suoritin|Prosentin Suoritin|Prosenttia|Keskiarvo|Kohdistettu Laske yksiköt Virtual Machine(s) käytössä olevia prosentteina|
|Verkko-|Verkko-|Tavua|Kokonaissumma|Valitse kaikki verkkoliittymät vastaanottanut Virtual Machine(s) (saapuvan liikenteen) tavujen määrän|
|Verkon ulos|Verkon ulos|Tavua|Kokonaissumma|Loitonna kaikki verkkoliittymät Virtual Machine(s) (lähtevän tietoliikenteen) mukaan tavujen määrä|
|Levyn luku tavua|Levyn luku tavua|Tavua|Kokonaissumma|Tavujen lukea levyä seuranta ajanjakson aikana|
|Levyn kirjoittaminen tavua|Levyn kirjoittaminen tavua|Tavua|Kokonaissumma|Tavujen kirjoittaa levylle seuranta ajanjakson aikana|
|Levyn toiminnot/s|Levyn toiminnot/s|CountPerSecond|Keskiarvo|Levyn luku IOPS|
|Levyn toiminnot/s|Levyn toiminnot/s|CountPerSecond|Keskiarvo|Levyn kirjoittamisessa IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|Prosentin Suoritin|Prosentin Suoritin|Prosenttia|Keskiarvo|Kohdistettu Laske yksiköt Virtual Machine(s) käytössä olevia prosentteina|
|Verkko-|Verkko-|Tavua|Kokonaissumma|Valitse kaikki verkkoliittymät vastaanottanut Virtual Machine(s) (saapuvan liikenteen) tavujen määrän|
|Verkon ulos|Verkon ulos|Tavua|Kokonaissumma|Loitonna kaikki verkkoliittymät Virtual Machine(s) (lähtevän tietoliikenteen) mukaan tavujen määrä|
|Levyn luku tavua|Levyn luku tavua|Tavua|Kokonaissumma|Tavujen lukea levyä seuranta ajanjakson aikana|
|Levyn kirjoittaminen tavua|Levyn kirjoittaminen tavua|Tavua|Kokonaissumma|Tavujen kirjoittaa levylle seuranta ajanjakson aikana|
|Levyn toiminnot/s|Levyn toiminnot/s|CountPerSecond|Keskiarvo|Levyn luku IOPS|
|Levyn toiminnot/s|Levyn toiminnot/s|CountPerSecond|Keskiarvo|Levyn kirjoittamisessa IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|Prosentin Suoritin|Prosentin Suoritin|Prosenttia|Keskiarvo|Kohdistettu Laske yksiköt Virtual Machine(s) käytössä olevia prosentteina|
|Verkko-|Verkko-|Tavua|Kokonaissumma|Valitse kaikki verkkoliittymät vastaanottanut Virtual Machine(s) (saapuvan liikenteen) tavujen määrän|
|Verkon ulos|Verkon ulos|Tavua|Kokonaissumma|Loitonna kaikki verkkoliittymät Virtual Machine(s) (lähtevän tietoliikenteen) mukaan tavujen määrä|
|Levyn luku tavua|Levyn luku tavua|Tavua|Kokonaissumma|Tavujen lukea levyä seuranta ajanjakson aikana|
|Levyn kirjoittaminen tavua|Levyn kirjoittaminen tavua|Tavua|Kokonaissumma|Tavujen kirjoittaa levylle seuranta ajanjakson aikana|
|Levyn toiminnot/s|Levyn toiminnot/s|CountPerSecond|Keskiarvo|Levyn luku IOPS|
|Levyn toiminnot/s|Levyn toiminnot/s|CountPerSecond|Keskiarvo|Levyn kirjoittamisessa IOPS|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Telemetriatietojen viestin lähetysyritykset|Laske|Kokonaissumma|Laitteen cloud telemetriatietojen viestien määrä yritti lähetetään IoT-toiminnossa|
|d2c.telemetry.ingress.Success|Telemetriatietojen lähetetyt viestit|Laske|Kokonaissumma|Laitteen Cloud telemetriatietojen sähköposteihin vastattaessa onnistuneesti IoT-toiminnossa määrä|
|c2d.Commands.egress.Complete.Success|Valmis komennot|Laske|Kokonaissumma|Cloud määrä laitteen komentoja onnistui-laite|
|c2d.Commands.egress.Abandon.Success|Ilman ylläpitäjää komennot|Laske|Kokonaissumma|Cloud ilman ylläpitäjää laitteen mukaan laitteen komentoja määrä|
|c2d.Commands.egress.Reject.Success|Komennot, jotka on hylätty|Laske|Kokonaissumma|Cloud laitteen komentoja laitteen hylännyt määrä|
|devices.totalDevices|Yhteensä laitteet|Laske|Kokonaissumma|Useita laitteita rekisteröity IoT-toiminnossa|
|devices.connectedDevices.allProtocol|Yhdistettyjen laitteiden|Laske|Kokonaissumma|IoT keskittimeen liitettyjen laitteiden määrä|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|INREQS|Pyynnöt|Laske|Kokonaissumma|Tapahtuman toiminnossa saapuvan viestin siirtonopeuden nimitilan varten|
|SUCCREQ|Onnistuneiden pyyntöjen|Laske|Kokonaissumma|Nimitilan onnistuneen pyyntöjen kokonaismäärä|
|FAILREQ|Epäonnistuneiden pyyntöjen|Laske|Kokonaissumma|Nimitilan epäonnistui pyyntöjen kokonaismäärä|
|SVRBSY|Palvelimen varattu virheet|Laske|Kokonaissumma|Yhteensä palvelimen varattu virheet nimitila|
|INTERR|Sisäisen palvelimen virheet|Laske|Kokonaissumma|Yhteensä sisäisen palvelimen virheet nimitila|
|MISCERR|Muut virheet|Laske|Kokonaissumma|Nimitilan epäonnistui pyyntöjen kokonaismäärä|
|INMSGS|Saapuviin viesteihin-kohta|Laske|Kokonaissumma|Yhteensä saapuvien viestien nimitilan varten|
|OUTMSGS|Lähtevien viestien|Laske|Kokonaissumma|Lähtevien viestien nimitilan yhteensä|
|EHINMBS|Saapuvan postin tavua sekunnissa|BytesPerSecond|Kokonaissumma|Tapahtuman toiminnossa saapuvan viestin siirtonopeuden nimitilan varten|
|EHOUTMBS|Lähtevän postin tavua sekunnissa|BytesPerSecond|Kokonaissumma|Lähtevien viestien nimitilan yhteensä|
|EHABL|Arkisto keskeneräisen viestit|Laske|Kokonaissumma|Nimitilan keskeneräisen tapahtuman keskittimeen arkisto viestit|
|EHAMSGS|Arkisto-viestit|Laske|Kokonaissumma|Tapahtuma-toiminnossa arkistoidut viestit nimitila|
|EHAMBS|Arkisto viestin siirtonopeuden|BytesPerSecond|Kokonaissumma|Tapahtuman keskittimeen arkistoidut viestin nopeus-nimitila|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|RunsStarted|Suorittaa käytön aloittaminen|Laske|Kokonaissumma|Työnkulun suorittamista aloittaminen.|
|RunsCompleted|Valmis suoritetaan|Laske|Kokonaissumma|Työnkulun suorittamista valmis.|
|RunsSucceeded|Suorittaa onnistui|Laske|Kokonaissumma|Työnkulun suorittamista onnistui.|
|RunsFailed|Suorittaa epäonnistui|Laske|Kokonaissumma|Työnkulun suorittamista epäonnistui.|
|RunsCancelled|Peruuttaa suoritetaan|Laske|Kokonaissumma|Työnkulun suorittamista peruutettu.|
|RunLatency|Suorita viive|Sekunnin ajan|Keskiarvo|Valmiit työnkulun viive suoritetaan.|
|RunSuccessLatency|Suorita Success viive|Sekunnin ajan|Keskiarvo|Viive onnistui työnkulku suoritetaan.|
|RunThrottledEvents|Suorita kyselyn tapahtumat|Laske|Kokonaissumma|Työnkulkutoiminnon tai käynnistimen rajoittanut tapahtumat.|
|RunFailurePercentage|Suorita virheen prosentti|Prosenttia|Kokonaissumma|Prosenttiosuus työnkulku suoritetaan epäonnistui.|
|ActionsStarted|Toimintojen aloittaminen |Laske|Kokonaissumma|Työnkulkutoiminnot numero käynnistää.|
|ActionsCompleted|Valmis toiminnot |Laske|Kokonaissumma|Työnkulkutoiminnot valmis määrä.|
|ActionsSucceeded|Toiminnot onnistui |Laske|Kokonaissumma|Työnkulkutoiminnot määrä onnistui.|
|ActionsFailed|Toiminnot epäonnistui|Laske|Kokonaissumma|Työnkulkutoiminnot määrä epäonnistui.|
|ActionsSkipped|Toiminnot, jotka on ohitettu |Laske|Kokonaissumma|Työnkulkutoiminnot määrä ohitetaan.|
|ActionLatency|Toiminnon viive |Sekunnin ajan|Keskiarvo|Valmiit työnkulkutoiminnot viive.|
|ActionSuccessLatency|Toiminto onnistui viive |Sekunnin ajan|Keskiarvo|Viive onnistui työnkulkutoiminnot.|
|ActionThrottledEvents|Toiminnon rajoittanut tapahtumat|Laske|Kokonaissumma|Työnkulun käynnistystoimintoa määrä rajoittanut tapahtumat...|
|TriggersStarted|Käynnistimien käytön aloittaminen |Laske|Kokonaissumma|Työnkulun käynnistimien numero käynnistää.|
|TriggersCompleted|Käynnistimien valmis |Laske|Kokonaissumma|Työnkulun käynnistimien valmis määrä.|
|TriggersSucceeded|Käynnistimien onnistui |Laske|Kokonaissumma|Työnkulun käynnistimien määrän onnistui.|
|TriggersFailed|Käynnistimien epäonnistui |Laske|Kokonaissumma|Työnkulun käynnistimet määrä epäonnistui.|
|TriggersSkipped|Käynnistimien ohitettu|Laske|Kokonaissumma|Työnkulun käynnistimien määrän ohitetaan.|
|TriggersFired|Käynnistimien käynnistyy |Laske|Kokonaissumma|Työnkulun käynnistimet määrä käynnistyy.|
|TriggerLatency|Käynnistimen viive |Sekunnin ajan|Keskiarvo|Valmiit työnkulun käynnistimien viive.|
|TriggerFireLatency|Käynnistimen Fire viive |Sekunnin ajan|Keskiarvo|Viive käynnistyy työnkulun Käynnistimet.|
|TriggerSuccessLatency|Käynnistimen Success viive |Sekunnin ajan|Keskiarvo|Viive onnistui työnkulun Käynnistimet.|
|TriggerThrottledEvents|Käynnistimen rajoittanut tapahtumat|Laske|Kokonaissumma|Työnkulun käynnistäminen määrää rajoitettu tapahtumat.|
|BillableActionExecutions|Laskutettavat toiminnon suorituskerran|Laske|Kokonaissumma|Työnkulun toiminto suorituskerran käytön Laskutettu määrä.|
|BillableTriggerExecutions|Laskutettavat käynnistimen suorituskerran|Laske|Kokonaissumma|Työnkulun käynnistäminen suorituskerran käytön Laskutettu määrä.|
|TotalBillableExecutions|Yhteensä laskutettavan suorituskerran|Laske|Kokonaissumma|Työnkulun suorituskerran käytön Laskutettu määrä.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|Siirtonopeuden|Siirtonopeuden|BytesPerSecond|Keskiarvo||

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|SearchLatency|Etsi viive|Sekunnin ajan|Keskiarvo|Keskimääräinen haun viive hakupalvelun|
|SearchQueriesPerSecond|Hakukyselyt sekunnissa|CountPerSecond|Keskiarvo|Hakukyselyt sekunnissa hakupalvelu|
|ThrottledSearchQueriesPercentage|Kyselyn haun kyselyiden prosentti|Prosenttia|Keskiarvo|Hakukyselyt, jotka on rajoittanut hakupalvelun prosentteina|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|CPUXNS|Suorittimen käyttö nimitilan kohden|Prosenttia|Enimmäismäärä|Palvelun bus premium nimitilan suorittimen käyttö metrijärjestelmä|
|WSXNS|Muistin koon nimitilan kohden|Prosenttia|Enimmäismäärä|Palvelun bus premium nimitilan muistin käyttö metrijärjestelmä|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|cpu_percent|Suorittimen prosentti|Prosenttia|Keskiarvo|Suorittimen prosentti|
|physical_data_read_percent|Tietoja IO prosentti|Prosenttia|Keskiarvo|Tietoja IO prosentti|
|log_write_percent|Lokitiedoston IO prosentti|Prosenttia|Keskiarvo|Lokitiedoston IO prosentti|
|dtu_consumption_percent|DTU prosentteina|Prosenttia|Keskiarvo|DTU prosentteina|
|tallennustilan|Tietokannan koon|Tavua|Enimmäismäärä|Tietokannan koon|
|connection_successful|Onnistuneiden yhteydet|Laske|Kokonaissumma|Onnistuneiden yhteydet|
|connection_failed|Epäonnistuneiden yhteydet|Laske|Kokonaissumma|Epäonnistuneiden yhteydet|
|blocked_by_firewall|Palomuuri estää|Laske|Kokonaissumma|Palomuuri estää|
|lukituksen|Lukkiutuu|Laske|Kokonaissumma|Lukkiutuu|
|storage_percent|Tietokannan koko prosentteina|Prosenttia|Enimmäismäärä|Tietokannan koko prosentteina|
|xtp_storage_percent|Ladatun OLTP tallennustilan percent(Preview)|Prosenttia|Keskiarvo|Ladatun OLTP tallennustilan percent(Preview)|
|workers_percent|Työntekijöiden prosentti|Prosenttia|Keskiarvo|Työntekijöiden prosenttia|
|sessions_percent|Istunnot prosenttia|Prosenttia|Keskiarvo|Istunnot prosenttia|
|dtu_limit|DTU rajoitus|Laske|Keskiarvo|DTU rajoitus|
|dtu_used|Käyttää DTU|Laske|Keskiarvo|Käyttää DTU|
|service_level_objective|Palvelun tason tavoitteena tietokanta|Laske|Kokonaissumma|Palvelun tason tavoitteena tietokanta|
|dwu_limit|dwu rajoitus|Laske|Enimmäismäärä|dwu rajoitus|
|dwu_consumption_percent|DWU prosentteina|Prosenttia|Keskiarvo|DWU prosentteina|
|dwu_used|Käyttää DWU|Laske|Keskiarvo|Käyttää DWU|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|cpu_percent|Suorittimen prosentti|Prosenttia|Keskiarvo|Suorittimen prosentti|
|physical_data_read_percent|Tietoja IO prosentti|Prosenttia|Keskiarvo|Tietoja IO prosentti|
|log_write_percent|Lokitiedoston IO prosentti|Prosenttia|Keskiarvo|Lokitiedoston IO prosentti|
|dtu_consumption_percent|DTU prosentteina|Prosenttia|Keskiarvo|DTU prosentteina|
|storage_percent|Tallennustilan prosentti|Prosenttia|Keskiarvo|Tallennustilan prosentti|
|workers_percent|Työntekijöiden prosenttia|Prosenttia|Keskiarvo|Työntekijöiden prosenttia|
|sessions_percent|Istunnot prosenttia|Prosenttia|Keskiarvo|Istunnot prosenttia|
|eDTU_limit|eDTU rajoitus|Laske|Keskiarvo|eDTU rajoitus|
|storage_limit|Tallennustilan enimmäismäärä|Tavua|Keskiarvo|Tallennustilan enimmäismäärä|
|eDTU_used|käyttää eDTU|Laske|Keskiarvo|käyttää eDTU|
|storage_used|Käytetty tallennustila|Tavua|Keskiarvo|Käytetty tallennustila|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|ResourceUtilization|SU % käyttö|Prosenttia|Enimmäismäärä|SU % käyttö|
|InputEvents|Syötteen tapahtumat|Laske|Kokonaissumma|Syötteen tapahtumat|
|InputEventBytes|Kirjoita tapahtuman tavua|Tavua|Kokonaissumma|Kirjoita tapahtuman tavua|
|LateInputEvents|Myöhässä olevat syötteen tapahtumat|Laske|Kokonaissumma|Myöhässä olevat syötteen tapahtumat|
|OutputEvents|Tulosteen tapahtumat|Laske|Kokonaissumma|Tulosteen tapahtumat|
|ConversionErrors|Muuntovirheitä|Laske|Kokonaissumma|Muuntovirheitä|
|Virheet|Runtimen virheet|Laske|Kokonaissumma|Runtimen virheet|
|DroppedOrAdjustedEvents|Epäjärjestyksessä tapahtumat|Laske|Kokonaissumma|Epäjärjestyksessä tapahtumat|
|AMLCalloutRequests|Funktion pyynnöt|Laske|Kokonaissumma|Funktion pyynnöt|
|AMLCalloutFailedRequests|Epäonnistuneiden funktion pyynnöt|Laske|Kokonaissumma|Epäonnistuneiden funktion pyynnöt|
|AMLCalloutInputEvents|Funktion tapahtumat|Laske|Kokonaissumma|Funktion tapahtumat|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|CpuPercentage|Suorittimen prosentti|Prosenttia|Keskiarvo|Suorittimen prosentti|
|MemoryPercentage|Muistin prosentti|Prosenttia|Keskiarvo|Muistin prosentti|
|DiskQueueLength|Keskiarvo|Laske|Kokonaissumma|Keskiarvo|
|HttpQueueLength|HTTP-jono|Laske|Kokonaissumma|HTTP-jono|
|BytesReceived|Tietoja|Tavua|Kokonaissumma|Tietoja|
|BytesSent|Tietojen|Tavua|Kokonaissumma|Tietojen|

## <a name="microsoftwebsites"></a>Microsoft.Web/sites

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|CpuTime|Suorittimen ajan|Sekunnin ajan|Kokonaissumma|Suorittimen ajan|
|Palvelupyynnöt|Palvelupyynnöt|Laske|Kokonaissumma|Palvelupyynnöt|
|BytesReceived|Tietoja|Tavua|Kokonaissumma|Tietoja|
|BytesSent|Tietojen|Tavua|Kokonaissumma|Tietojen|
|Http2xx|HTTP-2xx|Laske|Kokonaissumma|HTTP-2xx|
|Http3xx|HTTP-3xx|Laske|Kokonaissumma|HTTP-3xx|
|Http401|HTTP 401|Laske|Kokonaissumma|HTTP 401|
|Http403|HTTP 403|Laske|Kokonaissumma|HTTP 403|
|Http404|HTTP 404|Laske|Kokonaissumma|HTTP 404|
|Http406|HTTP 406|Laske|Kokonaissumma|HTTP 406|
|Http4xx|HTTP-4xx|Laske|Kokonaissumma|HTTP-4xx|
|Http5xx|HTTP-palvelin-virheet|Laske|Kokonaissumma|HTTP-palvelin-virheet|
|MemoryWorkingSet|Muistin Työsarja|Tavua|Kokonaissumma|Muistin Työsarja|
|AverageMemoryWorkingSet|Keskimääräinen muistin toimi määrittäminen|Tavua|Keskiarvo|Keskimääräinen muistin toimi määrittäminen|
|AverageResponseTime|Keskimääräinen vastausajan|Sekunnin ajan|Keskiarvo|Keskimääräinen vastausajan|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Arvo|Metrijärjestelmän näyttönimi|Yksikkö|Yhdistämistapa|Kuvaus|
|---|---|---|---|---|
|CpuTime|Suorittimen ajan|Sekunnin ajan|Kokonaissumma|Suorittimen ajan|
|Palvelupyynnöt|Palvelupyynnöt|Laske|Kokonaissumma|Palvelupyynnöt|
|BytesReceived|Tietoja|Tavua|Kokonaissumma|Tietoja|
|BytesSent|Tietojen|Tavua|Kokonaissumma|Tietojen|
|Http2xx|HTTP-2xx|Laske|Kokonaissumma|HTTP-2xx|
|Http3xx|HTTP-3xx|Laske|Kokonaissumma|HTTP-3xx|
|Http401|HTTP 401|Laske|Kokonaissumma|HTTP 401|
|Http403|HTTP 403|Laske|Kokonaissumma|HTTP 403|
|Http404|HTTP 404|Laske|Kokonaissumma|HTTP 404|
|Http406|HTTP 406|Laske|Kokonaissumma|HTTP 406|
|Http4xx|HTTP-4xx|Laske|Kokonaissumma|HTTP-4xx|
|Http5xx|HTTP-palvelin-virheet|Laske|Kokonaissumma|HTTP-palvelin-virheet|
|MemoryWorkingSet|Muistin Työsarja|Tavua|Kokonaissumma|Muistin Työsarja|
|AverageMemoryWorkingSet|Keskimääräinen muistin toimi määrittäminen|Tavua|Keskiarvo|Keskimääräinen muistin toimi määrittäminen|
|AverageResponseTime|Keskimääräinen vastausajan|Sekunnin ajan|Keskiarvo|Keskimääräinen vastausajan|

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lue lisätietoja Azure näytön arvot](./monitoring-overview.md#monitoring-sources)
- [Luo ilmoituksia arvojen mukaisesti](./insights-receive-alert-notifications.md)
- [Vie arvot tallennustilaa, tapahtuma-toiminnossa tai lokin Analytics](./monitoring-overview-of-diagnostic-logs.md)
