<properties
   pageTitle="Toimijoiden diagnostiikka- ja seuranta | Microsoft Azure"
   description="Tässä artikkelissa kuvataan diagnostiikka- ja suorituskyvyn palvelun kangasta luotettava toimijoiden suorituksenaikainen, mukaan lukien tapahtuma- ja suorituskyvyn laskureita lähettämän sen ominaisuuksia."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnostiikka- ja suorituskyvyn seurantaa varten luotettava toimijat
Luotettavan toimijoiden runtime tietokoneesta kuuluu äänimerkki [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) tapahtumien ja [suorituskyvyn laskureita](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Nämä Anna kyselyjä miten suorituksen toimii tiedot ja vianmääritys ja suorituskyvyn seurantaa.

## <a name="eventsource-events"></a>EventSource tapahtumat
Luotettavan toimijoiden runtime EventSource tarjoajan nimi on "Microsoft-ServiceFabric-toimijat". Tapahtuman tästä lähteestä tapahtumat näkyvät [Diagnostiikka tapahtumat](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) -ikkunassa, kun toimija-sovellus on [aloittaessasi Visual Studiossa](service-fabric-debugging-your-application.md).

Esimerkkejä työkalut ja toiminnot, jotka auttavat kerääminen ja/tai EventSource tapahtumien tarkasteleminen ovat [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure diagnostiikka](../cloud-services/cloud-services-dotnet-diagnostics.md), [Semanttinen kirjaaminen](https://msdn.microsoft.com/library/dn774980.aspx)ja [Microsoft TraceEvent kirjaston](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Avainsanat
Kaikki tapahtumat, jotka kuuluvat luotettava toimijoiden EventSource on yhdistetty vähintään yksi hakusana. Näin suodattamisen tapahtumia, jotka kerätään. Seuraavat avainsana bittien on määritetty.

|Bit|Kuvaus|
|---|---|
|0x1|Määritä tärkeitä tapahtumia, jotka sisältävät kangasta toimijoiden suoritettavan toiminnon.|
|0x2|Tapahtumia, jotka kuvaavat toimija menetelmäkutsujen joukko. Lisätietoja on artikkelissa [Johdanto-ohjeaihe-toimijat](service-fabric-reliable-actors-introduction.md#actors).|
|0x4|Toimija-tilaan liittyvät tapahtumat joukko. Lisätietoja on ohjeaiheessa [toimija-tilan hallinta](service-fabric-reliable-actors-state-management.md).|
|0x8|Ota perustuva samanaikainen toimija-liittyvät tapahtumat joukko. Lisätietoja on ohjeaiheessa [samanaikainen](service-fabric-reliable-actors-introduction.md#concurrency).|

## <a name="performance-counters"></a>Suorituskyvyn laskureita
Luotettavan toimijoiden runtime määrittää suorituskyvyn laskuri-luokat.

|Luokka|Kuvaus|
|---|---|
|Palvelun kangasta toimija|Yksityiskohtaiset laskurit Azure palvelun kangasta toimijoiden, kuten aika keskimäärin Tallenna toimija tila|
|Palvelun kangasta toimija menetelmä|Laskureita tietyn palvelun kangasta toimijoiden menetelmiä, esimerkiksi kuinka usein toimija-menetelmä käynnistetään|

Luokista on vähintään yksi laskureita.

[Windowsin suorituskyvyn valvonta](https://technet.microsoft.com/library/cc749249.aspx) -sovellus, joka on oletusarvoisesti Windows-käyttöjärjestelmän avulla voidaan kerätä ja tarkastella laskuri suorituskykytietoja. [Azure diagnostiikka](../cloud-services/cloud-services-dotnet-diagnostics.md) on toinen tapa kerätä suorituskykytietoja laskuri ja lataaminen Azure taulukot.

### <a name="performance-counter-instance-names"></a>Suorituskyvyn laskuri esiintymien nimet
Klusterin, jossa on paljon toimija services tai toimija osiot on suuri määrä toimija suorituskyvyn laskuriesiintymät. Suorituskyvyn laskuri esiintymien nimet auttaa tunnistamisessa [osio](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) ja toimija menetelmä (jos saatavilla) esiintymä suorituskyvyn laskuri on liitetty.

#### <a name="service-fabric-actor-category"></a>Palvelun kangasta toimija-luokka
Luokan `Service Fabric Actor`, laskuri-esiintymän nimi on seuraavanlainen:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* on palvelun kangasta osion tunnuksen, johon on liitetty esiintymä suorituskyvyn laskuri string-esitys. Osion tunnus on GUID-tunnus, ja sen string-esitys luodaan kautta [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) menetelmä, jolla muoto määrite "D".

*ActorRuntimeInternalID* on 64-bittisen kokonaisluvun, joka on luonut kangasta toimijoiden runtime sen sisäiseen käyttöön string-esitys. Tämä sisältyy suorituskyvyn laskuri esiintymän nimi varmistaa sen yksilöllisyyden ja olla ristiriidassa muiden suorituskyvyn laskuri esiintymän nimet. Käyttäjien ei tule yrittää tulkita tämän osan suorituskykyyn laskuri esiintymän nimi.

Seuraavassa on esimerkki laskuri esiintymänimi, joka kuuluu laskuri `Service Fabric Actor` luokkaan:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Yllä olevassa esimerkissä `2740af29-78aa-44bc-a20b-7e60fb783264` on palvelun kangasta osion ID-tunnus, string-esitys ja `635650083799324046` on 64-bittinen ID, jolla luodaan suorituksen on sisäinen avulla.

#### <a name="service-fabric-actor-method-category"></a>Palvelun kangasta toimija menetelmä luokka
Luokan `Service Fabric Actor Method`, laskuri-esiintymän nimi on seuraavanlainen:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* on toimija menetelmän, jotka liittyvät suorituskyvyn laskuri-esiintymän nimi. Menetelmän nimen muoto määräytyy kangasta toimijoiden suorituksen aikana, joka jakaa rajoituksia suorituskyvyn laskuri esiintymien nimet enimmäispituus Windows nimi luettavuutta logiikkaa perusteella.

*ActorsRuntimeMethodId* on 32-bittisen kokonaisluvun, joka on luonut kangasta toimijoiden runtime sen sisäiseen käyttöön string-esitys. Tämä sisältyy suorituskyvyn laskuri esiintymän nimi varmistaa sen yksilöllisyyden ja olla ristiriidassa muiden suorituskyvyn laskuri esiintymän nimet. Käyttäjien ei tule yrittää tulkita tämän osan suorituskykyyn laskuri esiintymän nimi.

*ServiceFabricPartitionID* on palvelun kangasta osion tunnuksen, johon on liitetty esiintymä suorituskyvyn laskuri string-esitys. Osion tunnus on GUID-tunnus, ja sen string-esitys luodaan kautta [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) menetelmä, jolla muoto määrite "D".

*ActorRuntimeInternalID* on 64-bittisen kokonaisluvun, joka on luonut kangasta toimijoiden runtime sen sisäiseen käyttöön string-esitys. Tämä sisältyy suorituskyvyn laskuri esiintymän nimi varmistaa sen yksilöllisyyden ja olla ristiriidassa muiden suorituskyvyn laskuri esiintymän nimet. Käyttäjien ei tule yrittää tulkita tämän osan suorituskykyyn laskuri esiintymän nimi.

Seuraavassa on esimerkki laskuri esiintymänimi, joka kuuluu laskuri `Service Fabric Actor Method` luokkaan:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Yllä olevassa esimerkissä `ivoicemailboxactor.leavemessageasync` menetelmän, nimi on `2` on luotu runtime sisäiseen käyttöön, 32-bittinen tunnus `89383d32-e57e-4a9b-a6ad-57c6792aa521` on palvelun kangasta osion ID-tunnus, string-esitys ja `635650083804480486` on luotu, suorituksen on sisäinen 64-bittinen tunnus käyttää.

## <a name="list-of-events-and-performance-counters"></a>Tapahtumien ja suorituskyvyn laskureita luettelo

### <a name="actor-method-events-and-performance-counters"></a>Toimija menetelmä tapahtumien ja suorituskyvyn laskureita
Luotettavan toimijoiden runtime tietokoneesta kuuluu äänimerkki liittyvät [toimija menetelmät](service-fabric-reliable-actors-introduction.md#actors)seuraava tapahtumat.

|Tapahtuman nimi|Tapahtumatunnus|Taso|Avainsana|Kuvaus|
|---|---|---|---|---|
|ActorMethodStart|7|Yksityiskohtainen|0x2|Toimijoiden suorituksenaikainen on kutsua toimija-menetelmää.|
|ActorMethodStop|8|Yksityiskohtainen|0x2|Toimija-menetelmä on suoritettu loppuun. Ei runtime toimija-menetelmä asynkroninen kutsu on palauttanut ja toimija-menetelmän palauttama tehtävä on valmis.|
|ActorMethodThrewException|9|Varoitus|0x3|Poikkeus toimija-menetelmä suorituksen aikana suorituksenaikainen toimija-menetelmän asynkroninen puhelun aikana tai tehtävän suorituksen aikana palauttama toimija-menetelmää. Tapahtuman ilmaisee syyn toimija-tunnus, jolla on tutkimuksen tarkoitus.|

Luotettavan toimijoiden runtime julkaisee seuraavia suorituskyvyn laskureita liittyvien toimija tavoista.

|Luokkanimi|Laskuri-nimi|Kuvaus|
|---|---|---|
|Palvelun kangasta toimija menetelmä|Ohjelmarakennekaaviossa/s|Kuinka monta kertaa toimija service-menetelmää kutsutaan sekunnissa|
|Palvelun kangasta toimija menetelmä|Keskimääräinen millisekuntia kohti käynnistäminen|Toimija-palvelumenetelmä suorittamiseen millisekunteina aika|
|Palvelun kangasta toimija menetelmä|Virhe ilmenee poikkeukset/s|Kuinka monta kertaa toimija palvelun menetelmä poikkeuksen sekunnissa|

### <a name="concurrency-events-and-performance-counters"></a>Samanaikainen tapahtumien ja suorituskyvyn laskureita
Luotettavan toimijoiden runtime tietokoneesta kuuluu äänimerkki [samanaikainen](service-fabric-reliable-actors-introduction.md#concurrency)liittyvät tapahtumat.

|Tapahtuman nimi|Tapahtumatunnus|Taso|Avainsana|Kuvaus|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Yksityiskohtainen|0x8|Tapahtuman kirjoitetaan kunkin uuden Palauta toimija alkuun. Se sisältää odottavien toimija puhelut, jotka odottavat hankkia toimija lukituksen, joka edellyttää Ota perustuva samanaikainen määrä.|

Luotettavan toimijoiden runtime julkaisee seuraavia samanaikainen liittyvät suorituskyvyn laskureita.

|Luokkanimi|Laskuri-nimi|Kuvaus|
|---|---|---|
|Palvelun kangasta toimija|# toimija puhelujen odotetaan toimija lukitseminen|Toimija odottavien kutsujen odottaa hankkia toimija lukituksen, joka edellyttää Ota perustuva samanaikainen|
|Palvelun kangasta toimija|Keskimääräinen millisekuntia kohti Lukitse Odota|Aika (millisekunteina) hankkia toimija lukituksen, joka edellyttää Ota perustuva samanaikainen|
|Palvelun kangasta toimija|Keskimääräinen millisekuntia toimija lukituksen|Ajan (millisekunteina), jonka toimija Lukitse säilytetään|

### <a name="actor-state-management-events-and-performance-counters"></a>Toimija tilan hallinta-tapahtumien ja suorituskyvyn laskureita
Luotettavan toimijoiden suorituksenaikainen tietokoneesta kuuluu äänimerkki seuraavat tapahtumat, jotka liittyvät [toimija-tilan hallinta](service-fabric-reliable-actors-state-management.md).

|Tapahtuman nimi|Tapahtumatunnus|Taso|Avainsana|Kuvaus|
|---|---|---|---|---|
|ActorSaveStateStart|10|Yksityiskohtainen|0x4|Toimijoiden runtime on tallentaminen toimija-tilaan.|
|ActorSaveStateStop|11|Yksityiskohtainen|0x4|Toimijoiden runtime on lopettanut tallennetaan toimija-tila.|

Luotettavan toimijoiden runtime julkaisee seuraavia suorituskyvyn laskureita liittyvät toimija-tilan hallinta.

|Luokkanimi|Laskuri-nimi|Kuvaus|
|---|---|---|
|Palvelun kangasta toimija|Keskimääräinen millisekuntia kohti Tallenna toiminnon|Tallenna toimija tilan millisekunteina aika|
|Palvelun kangasta toimija|Keskimääräinen millisekuntia kuormituksen toiminnon kohden|Lataa toimija tilan millisekunteina aika|

### <a name="events-related-to-actor-replicas"></a>Toimija replikoiden liittyvät tapahtumat
Luotettavan toimijoiden suorituksenaikainen tietokoneesta kuuluu äänimerkki [toimija replikoiden](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors)liittyvät tapahtumat.

|Tapahtuman nimi|Tapahtumatunnus|Taso|Avainsana|Kuvaus|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Tiedoksi|0x1|Toimija replikan muuttaa roolin ensisijainen. Tämä tarkoittaa sitä tässä osiossa toimijat luodaan tämän replikan sisällä.|
|ReplicaChangeRoleFromPrimary|2|Tiedoksi|0x1|Toimija replikan muuttaa roolin muun ensisijaisen. Tämä tarkoittaa sitä, että tässä osiossa toimijat enää luodaan tämän replikan sisällä. Ei uusia pyyntöjä lähetetään toimijoiden tämän replikan luotu. Toimijat tuhoutuvat keskeneräiset pyynnöt jälkeen.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Toimija aktivointi- ja käytöstä poistamisen tapahtumien ja suorituskyvyn laskureita
Luotettavan toimijoiden suorituksenaikainen tietokoneesta kuuluu äänimerkki [toimija aktivointi ja käytöstä poistamisen](service-fabric-reliable-actors-lifecycle.md)liittyvät tapahtumat.

|Tapahtuman nimi|Tapahtumatunnus|Taso|Avainsana|Kuvaus|
|---|---|---|---|---|
|ActorActivated|5|Tiedoksi|0x1|Toimija on aktivoitu.|
|ActorDeactivated|6|Tiedoksi|0x1|Toimija on poistettu käytöstä.|

Luotettavan toimijoiden runtime julkaisee seuraavia toimija aktivointi ja käytöstä poistamisen liittyvät suorituskyvyn laskureita.

|Luokkanimi|Laskuri-nimi|Kuvaus|
|---|---|---|
|Palvelun kangasta toimija|Keskimääräinen OnActivateAsync millisekunteina|Suorittamiseen OnActivateAsync menetelmä millisekunteina aika|

### <a name="actor-request-processing-performance-counters"></a>Toimija kokouspyynnön käsittely suorituskyvyn laskureita
Kun asiakas käynnistää menetelmän objektin toimija välityspalvelimen kautta, se tuottaa pyynnön-viesti on lähetetty verkossa toimija-palveluun. Palvelu käsittelee pyynnön viestin ja lähettää vastauksen takaisin asiakkaalle. Luotettavan toimijoiden runtime julkaisee seuraavia toimija kokouspyynnön käsittely liittyvät suorituskyvyn laskureita.

|Luokkanimi|Laskuri-nimi|Kuvaus|
|---|---|---|
|Palvelun kangasta toimija|# Avoin pyyntöjen|Palvelun käsittelemien pyyntöjen määrä|
|Palvelun kangasta toimija|Keskimääräinen millisekuntia pyynnön kohden|Aika (millisekunteina)-palvelun käsittelemään pyytäminen|
|Palvelun kangasta toimija|Pyynnön sarjoituksen keskimääräinen millisekunteina|Aika (millisekunteina) poistaa toimija viesti, kun se vastaanotetaan palvelu|
|Palvelun kangasta toimija|Keskimääräinen millisekuntia vastauksen Sarjatoiminto|Aika (millisekunteina) onnistu toimija vastausviesti on palvelu, ennen kuin vastaus lähetetään asiakkaalle|

## <a name="next-steps"></a>Seuraavat vaiheet
 - [Luotettavan toimijoiden käyttämisestä palvelun kangasta-ympäristössä](service-fabric-reliable-actors-platform.md)
 - [Toimija API oppaat](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Esimerkki koodi](https://github.com/Azure/servicefabric-samples)
