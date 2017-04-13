<properties
    pageTitle="Optimoida palvelun kangasta ratkaisuun Log Analytics-ympäristösi | Microsoft Azure"
    description="Palvelun kangasta-ratkaisun avulla voit arvioida riski ja palvelun kangasta sovellukset, mikro-palveluiden, solmujen ja klustereiden kunto."
    services="log-analytics"
    documentationCenter=""
    authors="niniikhena"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="nini"/>



# <a name="service-fabric-solution-in-log-analytics"></a>Palvelun kangasta ratkaisu Log Analytics

> [AZURE.SELECTOR]
- [Resurssien hallinta](log-analytics-service-fabric-azure-resource-manager.md)
- [PowerShellin](log-analytics-service-fabric.md)

Tässä artikkelissa käsitellään tunnistamista ja vianmääritys usean palvelun kangasta klusterin Log Analytics palvelun kangasta-ratkaisun avulla.

Palvelun kangasta-ratkaisun käyttää Service-kangasta VMs Azure diagnostiikka tietoja tietojen kerääminen Azure WAD-taulukosta. Valitse log Analytics lukee palvelun kangasta framework tapahtumia, kuten **Luotettavaa palvelun tapahtumia**, **Toimijan tapahtumat**, **Toiminnallisia tapahtumien**ja **Mukautettu tapahtumien seuranta tapahtumia**. Ratkaisu-koontinäyttö, jossa olet voit tarkastella huomattavat ongelmat ja tapahtumista palvelun kangasta-ympäristössä.

Aloita ratkaisuun, sinun on palvelun kangasta klusterin muodostaa Log Analytics-työtila. Seuraavassa on kuvattu kolme käyttötilannetta ottaa huomioon:

1. Jos olet ottanut ei palvelun kangasta klusterin, ohjeiden mukaisesti ***käyttöönotto palvelun kangasta klusterin yhteydessä Log Analytics työtilan*** ottaa käyttöön uuden klusterin ja sen määritetty lokin Analytics-raportin.

2. Jos tarvitset kerääminen suorituskyvyn laskureita isäntien että pystyt käyttämään palvelua kangasta-klusterin OMS-ratkaisuja, kuten suojauksen, noudata ohjeita ***palvelun kangasta klusterin yhteydessä OMS työtilan AM tunniste asennettu käyttöönotto.***

3. Jos olet jo asentanut palvelun kangasta klusterin ja haluat muodostaa Log Analytics, noudata ohjeita ***käytössä olevan tallennustilan tilin lisääminen Log Analytics.***


##<a name="deploy-a-service-fabric-cluster-connected-to-a-log-analytics-workspace"></a>Ota loki Analytics työtilan yhteydessä palvelun kangasta klusterin.
Tämän mallin tekee seuraavat toimet:


1. Ottaa käyttöön aiemmin muodostanut yhteyden Log Analytics työtilan Azure palvelun kangasta-klusterin. Voit luoda uuden työtilan otettaessa malli tai jo olemassa olevan lokin Analytics-työtilan nimi.
2. Lisää diagnostiikan tallennustilan tilin Log Analytics-työtilassa.
3. Ottaa käyttöön palvelun kangasta-ratkaisun Log Analytics-työtilassa.

[![Ottaa käyttöön Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-oms%2F%2Fazuredeploy.json)


Kun olet valinnut yläpuolella olevaa käyttöönotto-painiketta, voit saapuvat parametreilla, jossa voit muokata Azure-portaalissa. Luo uusi resurssiryhmä, jos olet kirjoittanut uusi loki Analytics työtilan nimi: ![palvelun kangasta](./media/log-analytics-service-fabric/2.png)

![Palvelun kangasta](./media/log-analytics-service-fabric/3.png)

Hyväksy juridiset ehdot ja valitse sitten Aloita käyttöönoton "Luo". Kun asennus on valmis, pitäisi näkyä uusi työtila ja luoda klusterin ja WADServiceFabric * lisäämäsi tapahtuman, WADWindowsEventLogs ja WADETWEvent taulukot:

![Palvelun kangasta](./media/log-analytics-service-fabric/4.png)

##<a name="deploy-a-service-fabric-cluster-connected-to-an-oms-workspace-with-vm-extension-installed"></a>Ota käyttöön palvelun kangasta klusterin yhteydessä OMS työtilan AM tunniste on asennettu.
Tämän mallin tekee seuraavat toimet:

1. Ottaa käyttöön aiemmin muodostanut yhteyden Log Analytics työtilan Azure-palvelu kangasta-klusterin. Voit luoda uuden työtilan tai Käytä aiemmin luotua.
2. Lisää diagnostiikan tallennustilan tilit Log Analytics-työtilassa.
3. Ottaa käyttöön palvelun kangasta-ratkaisun Log Analytics-työtilassa.
4. Asentaa MMA agent-tunniste kunkin AM mittakaava, Määritä palvelun kangasta-klusterin. MMA-agentti on asennettu, jonka olet voit tarkastella tietoja oman solmujen suorituskyvyn mittarit.


[![Ottaa käyttöön Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fazure-quickstart-templates%2Fmaster%2Fservice-fabric-vmss-oms%2F%2Fazuredeploy.json)


Yllä samat vaiheet kirjoita sitten tarvittavat parametrit ja käyttöönoton aloituskokouksen. Uudelleen uuden työtilan, klusterin ja WAD taulukoiden kaikki luodut tulee näkyviin:

![Palvelun kangasta](./media/log-analytics-service-fabric/5.png)

###<a name="viewing-performance-data"></a>Suorituskyvyn tietojen tarkasteleminen

Tarkasteltava oman solmujen Perf tiedot:
</br>
- Käynnistä loki Analytics-työtilan Azure-portaalista.

![Palvelun kangasta](./media/log-analytics-service-fabric/6.png)

- Valitse vasemmanpuoleisessa ruudussa asetukset ja tiedot >> Windowsin suorituskykylaskureita >> "Lisää valitun suorituskyvyn laskureita": ![palvelun kangasta](./media/log-analytics-service-fabric/7.png)

- Lokitiedoston hakutoiminnossa delve siitä, että solmujen avaimen arvot yhdeksi seuraavat kyselyjen avulla:
</br>

    a. Vertaa keskimääräinen suorittimen käyttö välillä kaikki solmut edellisen tunnin solmut on vaikeuksia ja mitä aikaväliä solmu oli Piikkiin:

    ``` Type=Perf ObjectName=Processor CounterName="% Processor Time"|measure avg(CounterValue) by Computer Interval 1HOUR. ```

    ![Palvelun kangasta](./media/log-analytics-service-fabric/10.png)


    b. Tarkastella käytettävissä oleva muisti samalla viivakaaviot kunkin tämä kysely-solmu:

    ```Type=Perf ObjectName=Memory CounterName="Available MBytes Memory" | measure avg(CounterValue) by Computer Interval 1HOUR.```

    Jos haluat tarkastella kaikkien solmujen, jossa näkyy kunkin solmun megatavuja käytettävissä tarkka keskiarvo luettelo Käytä tämä kysely:

    ```Type=Perf (ObjectName=Memory) (CounterName="Available MBytes") | measure avg(CounterValue) by Computer ```

    ![Palvelun kangasta](./media/log-analytics-service-fabric/11.png)


    c-näppäinyhdistelmää. Sinulla on voi käyttää tätä kyselyä (korvaa tietokoneen kenttä), jonka haluat tunnin välein keskiarvo, vähintään, enintään ja 75 prosenttipiste suorittimen käyttö tarkastelemalla tietyn solmun siirtyä alirakenteeseen tapauksessa:

    ```Type=Perf CounterName="% Processor Time" InstanceName=_Total Computer="BaconDC01.BaconLand.com"| measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR```

    ![Palvelun kangasta](./media/log-analytics-service-fabric/12.png)

    Lue lisätietoja suorituskyvyn mittarit Log Analytics [tähän]. (https://blogs.technet.microsoft.com/msoms/tag/metrics/)


##<a name="adding-an-existing-storage-account-to-log-analytics"></a>Käytössä olevan tallennustilan tilin lisääminen Log Analytics

Tämän mallin Lisää olemassa olevan tallennustilan asiakkaiden riittää, että uusi tai aiemmin luotu loki Analytics-työtila.
</br>

[![Ottaa käyttöön Azure](./media/log-analytics-service-fabric/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Foms-existing-storage-account%2Fazuredeploy.json)

>[AZURE.NOTE] Valitsemalla resurssiryhmä, jos käsittelet aiemmin luodusta Log Analytics-työtila, valitse "Käytä aiemmin" ja etsiä sisältävä OMS työtilan resurssiryhmä. Luo uusi yhden Jos muulla tavoin.
![Palvelun kangasta](./media/log-analytics-service-fabric/8.png)

Tämä malli on otettu käyttöön, kun osaat Log Analytics-työtilan yhteydessä näkyy tallennustilan-tili. Tämä esiintymä voin lisätä lisää tallennustilaa tilien edellä luotu Exchange-työtilaan.
![Palvelun kangasta](./media/log-analytics-service-fabric/9.png)

## <a name="view-service-fabric-events"></a>Palvelun kangasta tapahtumien tarkasteleminen

Kun käyttöönotoissa on suoritettu ja palvelun kangasta ratkaisu on otettu käyttöön työtilassa, valitse Käynnistä palvelun kangasta Raporttinäkymät-ikkunan Log Analytics-portaalissa **Palvelun kangasta** -ruutu. Koontinäytön sisältää sarakkeet seuraavassa taulukossa. Kunkin sarakkeessa kymmenen ylimmän tapahtumien määrän, jotka vastaavat kyseisen sarakkeen ehdot määritetyn aikavälin mukaan. Voit suorittaa haun lokitiedoston, joka sisältää koko luettelon napsauttamalla kunkin sarakkeen oikeassa alareunassa **näet kaikki** tai napsauttamalla sarakkeen otsikkoa.

| **Palvelun kangasta tapahtuma** | **kuvaus** |
| --- | --- |
| Huomattavat ongelmat | Näyttö, kuten RunAsyncFailures RunAsynCancellations ja solmu Niemi ongelmista. |
| Toiminnallisia tapahtumat | Huomattavat toiminnallisia tapahtumia, kuten sovelluksen päivitys- ja ominaisuuksissa. |
| Luotettavan palvelun tapahtumat | Huomattavat luotettavaa palvelun tapahtumat tällaisten Runasyncinvocations. |
| Toimija tapahtumat | Huomattavat toimija tapahtumat luoma mikro-palvelujen, kuten poikkeukset toimija-menetelmää, toimija aktivointia ja deactivations ja niin edelleen. |
| Sovelluksen tapahtumat | Kaikki mukautetut tapahtumien seuranta tapahtumat luomat sovellukset. |

![Palvelun kangasta Raporttinäkymät-ikkunan](./media/log-analytics-service-fabric/sf3.png)

![Palvelun kangasta Raporttinäkymät-ikkunan](./media/log-analytics-service-fabric/sf4.png)


Seuraavassa taulukossa on tietoja sivustokokoelman menetelmät ja muita tietoja siitä, miten palvelun kangasta kerätä tietoja.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows|![Ei](./media/log-analytics-malware/oms-bullet-red.png)|![Ei](./media/log-analytics-malware/oms-bullet-red.png)| ![Kyllä](./media/log-analytics-malware/oms-bullet-green.png)|            ![Ei](./media/log-analytics-malware/oms-bullet-red.png)|![Ei](./media/log-analytics-malware/oms-bullet-red.png)|10 minuutin |


>[AZURE.NOTE] Voit muuttaa näitä tapahtumia palvelun kangasta ratkaisun laajuus valitsemalla Raporttinäkymät-ikkunan yläreunassa **tietojen perusteella viimeisen 7 päivän aikana** . Voit myös näyttää 1 päivä tai kuusi tuntia viimeisen seitsemän päivän ajalta tapahtumat. Vaihtoehtoisesti voit valita **mukautetun** Määritä mukautettu päivämääräväli.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaisia palvelun kangasta tapahtumatietoja.
