<properties 
   pageTitle="Vianmäärityslokit tarkasteleminen Azure tietojen järvi Analytics | Microsoft Azure" 
   description="Osaavat käyttää Azure tietojen järvi analytics vianmäärityslokit ja asetukset " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Vianmäärityslokit käyttäminen Azure tietojen järvi analysoinnissa

Tietoja Diagnostiikan kirjaus tietojen järvi Analytics-tilin ottaminen käyttöön ja miten voit tarkastella lokien kerääminen tilissäsi.

Organisaatioiden ottaa Diagnostiikan kirjaus voi kerätä tietoja access-kirjausketjut niiden Azure tietojen järvi Analytics-tilille. Lokitiedostot Anna tiedot

* Luettelo käyttäjistä, joka käyttää tietoja.
* Kuinka usein tietoja käytetään.
* Kuinka paljon tietoja on tallennettu tili.

## <a name="prerequisites"></a>Edellytykset

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).
- Tietojen järvi Analytics julkisen esikatselu **käyttöön Azure tilauksen** . Katso [ohjeet](data-lake-analytics-get-started-portal.md#signup).
- **Azure tietojen järvi Analytics-tili**. Ohjeiden etsiminen [Azure tietojen järvi Analytics Azure-portaalissa käytön aloittaminen](data-lake-analytics-get-started-portal.md).

## <a name="enable-logging"></a>Kirjaamisen ottaminen käyttöön

1. Kirjaudu uudessa [Azure portal](https://portal.azure.com).

2. Avata tietojen järvi Analytics-tilisi ja valitse tietoja järvi Analytics-tilin sivu **asetukset**ja valitse sitten **Diagnostiikka-asetusten muuttaminen**.

3. Tee seuraavat muutokset diagnostiikan kirjauksen määrittäminen **Diagnostiikan** sivu.

    ![Ota Diagnostiikan kirjaus] (./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Vianmäärityslokit ottaminen käyttöön")

    * Määritä **tilaksi** **-** Diagnostiikan kirjaus käyttöön.
    * Voit valita säilön/prosessin tiedot kahdella eri tavalla.
        * Valitse **tapahtuma-toiminnossa Vie** muodossa tietoja Azure-tapahtumaa-toiminnossa. Käytä tätä vaihtoehtoa, jos sinulla on edeltävät käsittely putkijohto analysointiin saapuvien lokit reaaliajassa. Jos valitset tämän vaihtoehdon, sinun on annettava tiedot Azure tapahtumaa-toiminnossa, jota haluat käyttää.
        * Valitse Tallenna lokitiedot Azure-tallennustilan tilin **tallennustilan tilin vieminen** . Käytä tätä vaihtoehtoa, jos haluat arkistoida tiedot. Jos valitset tämän vaihtoehdon, sinun on määritettävä Azure-tallennustilan tilin, lokien tallentaminen.
    * Määrittää, haluatko saada valvontalokien tai pyynnön lokit tai molempia.
    * Määritä, jonka tiedot on säilytettävä päivien määrän.
    * Valitse **Tallenna**.

Kun olet ottanut diagnostiikan asetuksia, voit katsoa **Vianmäärityslokit** -välilehden lokit.

## <a name="view-logs"></a>Näytä lokit

Voit tarkastella tietoja järvi Analytics-tilisi lokitiedot kahdella eri tavalla.

* Tietoja järvi Analytics-tilin asetuksista
* Azuren tallennustilaan-tililtä, jossa tiedot on tallennettu

### <a name="using-the-data-lake-analytics-settings-view"></a>Tietojen järvi Analytics-asetukset-näkymässä

1. Valitse oman tietojen järvi Analytics tilin **asetukset** sivu **Vianmäärityslokit**.

    ![Näytä Diagnostiikan kirjaus] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Näytä vianmäärityslokit") 

2. **Vianmäärityslokit** -sivu pitäisi näkyä luokiteltu **Valvontalokien** ja **Pyytää lokit**lokitiedot.
    * Pyyntö lokit sieppaa jokaisen API pyynnön tietojen järvi Analytics-tilissä.
    * Valvontalokien muistuttavat pyytää lokit mutta ole paljon erittelyn tietojen järvi Analytics-tilissä parhaillaan suoritettavia toimintoja. Esimerkiksi yksittäisen lataamisen API-kutsu pyynnön lokien saattaa aiheuttaa useita "Lisää" toimintojen valvontalokien.

3. Napsauta lokiin tiedon lataamaan lokit **Lataa** -linkkiä.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Azure-tallennustilan tilin, joka sisältää lokitiedot

1. Avaa liittyvät tiedot järvi Analytics kirjaaminen, Azuren tallennustilaan tili-sivu ja valitse sitten BLOB-objektit. **Blob-objektien service** -sivu näyttää kaksi säilöä.

    ![Näytä Diagnostiikan kirjaus] (./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Näytä vianmäärityslokit")

    * Säilön **tiedot-lokit-valvonta** sisältää valvontalokien.
    * Säilön **tiedot-lokit-pyyntöjen** sisältää pyynnön lokit.

2. Näiden säilöjen sisällä lokit tallennetaan seuraava rakenne-kohdassa.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] `##` Polku merkinnät sisältävät vuoden, kuukauden, päivän ja tunti, jossa loki luotiin. Tietoja järvi Analytics Luo tunnin välein niin yhden tiedoston `m=` on aina arvo `00`.

    Valvontalokin täydellinen polku voi olla esimerkiksi:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Vastaavasti pyynnön lokia täydellinen polku voi olla:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Log-rakenne

Valvonta- ja pyydä lokit ovat JSON-muodossa. Tässä osassa Tarkista JSON rakenteen pyynnön ja valvontalokit.

### <a name="request-logs"></a>Pyydä lokit

Seuraavassa on otoksen merkintä JSON-muotoiset pyynnön lokiin. Kunkin blob on yksi Pääobjektin kutsutaan **tietueet** , joka sisältää matriisin log objekteja.

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Pyydä log rakenne

| Nimi            | Tyyppi   | Kuvaus                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| aika            | Merkkijono | Kirjaudu aikaleimaa (Valitse UTC-aika)                                              |
| resourceId      | Merkkijono | Sijoita, toiminto suoritettiin resurssin tunnus                            |
| luokka        | Merkkijono | Log-luokka. Esimerkiksi **pyynnöt**.                                   |
| operationName   | Merkkijono | Toiminto, joka on kirjautunut nimi. Esimerkiksi GetAggregatedJobHistory.              |
| resultType      | Merkkijono | Toiminto, kuten 200 tila.                                 |
| callerIpAddress | Merkkijono | Pyynnön asiakkaan IP-osoite                                |
| correlationId   | Merkkijono | Kirjaudu tunnus. Tämän arvon avulla voidaan ryhmitellä joukko liittyvät tapahtumat |
| käyttäjätiedot        | Objektin | Tunnistetiedot, jotka on luotu lokiin                                            |
| Ominaisuudet:      | JSON   | Lisätietoja on seuraavassa osassa (pyyntö lokin ominaisuudet rakenne) |

#### <a name="request-log-properties-schema"></a>Pyydä lokin ominaisuudet rakenne

| Nimi                 | Tyyppi   | Kuvaus                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod-arvo           | Merkkijono | HTTP-menetelmä käyttää toiminnossa. Jos esimerkiksi haluat. |
| Polku                 | Merkkijono | Path toiminto on suoritettu                   |
| RequestContentLength | kokonaisluku    | HTTP-pyyntö sisällön pituus                    |
| ClientRequestId      | Merkkijono | Tunnus, joka yksilöi pyyntö              |
| Aloitusajan            | Merkkijono | Palvelin vastaanotti, jossa pyynnön aika         |
| Lopetusaika              | Merkkijono | Aika, jolla palvelin lähettää vastauksen              |

### <a name="audit-logs"></a>Valvontalokien

Tässä on esimerkki merkintä JSON-muotoiset valvontalokin. Kunkin blob on yksi Pääobjektin kutsutaan **tietueet** , joka sisältää matriisin log objektit

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Valvontalokien log rakenne

| Nimi            | Tyyppi   | Kuvaus                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| aika            | Merkkijono | Kirjaudu aikaleimaa (Valitse UTC-aika)                                              |
| resourceId      | Merkkijono | Sijoita, toiminto suoritettiin resurssin tunnus                            |
| luokka        | Merkkijono | Log-luokka. Esimerkiksi **valvonta**.                                      |
| operationName   | Merkkijono | Toiminto, joka on kirjautunut nimi. Esimerkiksi JobSubmitted.              |
| resultType | Merkkijono | Alitila projektin tila (operationName). |
| resultSignature | Merkkijono | Lisätietoja työn tila (operationName). |
| käyttäjätiedot      | Merkkijono | Käyttäjä, joka pyytää toiminto. Esimerkiksi susan@contoso.com.                                 |
| Ominaisuudet:      | JSON   | Lisätietoja on seuraavassa osassa (valvonta lokin ominaisuudet rakenne) |

> [AZURE.NOTE] __resultType__ ja __resultSignature__ tietoa toiminnon tulos ja sisältävät arvon vain, jos toiminto on suoritettu. Esimerkiksi ne sisältävät arvon, kun __operationName__ sisältää __JobStarted__ tai __JobEnded__arvon.

#### <a name="audit-log-properties-schema"></a>Valvontalokien lokin ominaisuudet rakenne

| Nimi       | Tyyppi   | Kuvaus                              |
|------------|--------|------------------------------------------|
| Työn tunnus | Merkkijono | Projektille tunnus  |
| Työ | Merkkijono | Nimi, joka projektille määritettyä |
| JobRunTime | Merkkijono | Käytettävä käsittelee työn runtime |
| SubmitTime | Merkkijono | Aika (UTC-ajan), työ on lähetetty |
| Aloitusajan | Merkkijono | Aloitusaika projektin suorittaminen (Valitse UTC-ajan) lähettämisen jälkeen. |
| Lopetusaika | Merkkijono | Työn aika. |
| Rinnakkaisuus | Merkkijono | Tietoja järvi Analytics yksiköiden pyydetty työlle lähetyksen aikana määrä. |

> [AZURE.NOTE] __SubmitTime__, __Aloitusaika__, __Lopetusaika__ ja __rinnakkaisuus__ tietoa toiminnon ja sisältävät arvon vain, jos toiminto on käynnissä tai valmis. Esimerkiksi __SubmitTime__ sisältää arvon, kun __operationName__ osoittaa __JobSubmitted__.

## <a name="process-the-log-data"></a>Prosessin lokitiedot

Azure tietojen järvi Analytics on esimerkki siitä, miten voit käsitellä ja analysoida lokitiedot. Löydät otosten [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md)


