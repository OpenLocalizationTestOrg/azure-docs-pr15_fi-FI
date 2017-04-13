<properties 
   pageTitle="Tarkasteleminen Azure järvi tietovaraston vianmäärityslokit | Microsoft Azure" 
   description="Kuinka asetukset ja käyttää Azure järvi tietovaraston vianmäärityslokit " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Azure järvi tietovaraston vianmäärityslokit käyttäminen

Tietoja Diagnostiikan kirjaus järvi tietovaraston tilin ottaminen käyttöön ja miten voit tarkastella lokien kerääminen tilissäsi.

Organisaatioiden ottaa Diagnostiikan kirjaus niiden Azure järvi tietovaraston tilin voi kerätä tietoja access-kirjausketjut, joka sisältää tietoja, kuten luettelon käyttäjien käytettäessä tietoja, kuinka usein tietoja käytetään, kuinka paljon tietoja on tallennettu tilin jne.

## <a name="prerequisites"></a>Edellytykset

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure järvi tietovaraston tili**. Ohjeiden etsiminen [Azure Lake Tietosäilölle Azure-portaalissa käytön aloittaminen](data-lake-store-get-started-portal.md).

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>Diagnostiikan kirjaus järvi tietovaraston tilin ottaminen käyttöön

1. Kirjaudu uudessa [Azure-portaalissa](https://portal.azure.com).

2. Avata järvi tietosäilö-tilisi ja valitse järvi tietosäilö-tilin sivu **asetukset**ja valitse sitten **Diagnostiikka-asetusten muuttaminen**.

3. Tee seuraavat muutokset diagnostiikan kirjauksen määrittäminen **Diagnostiikan** sivu.

    ![Ota Diagnostiikan kirjaus] (./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Vianmäärityslokit ottaminen käyttöön")

    * Määritä **tilaksi** **-** Diagnostiikan kirjaus käyttöön.
    * Voit valita säilön/prosessin tiedot kahdella eri tavalla.
        * Valitse vieminen **tapahtumaa-toiminnossa** voit stream tietoja Azure-tapahtumaa-toiminnossa. Todennäköisesti Käytä tätä vaihtoehtoa, jos sinulla on edeltävät käsittely putkijohto analysointiin saapuvien lokit reaaliajassa. Jos valitset tämän vaihtoehdon, sinun on annettava tiedot Azure tapahtumaa-toiminnossa, jota haluat käyttää.
        * Valitse vaihtoehto **tallennustilan tilin** vieminen Azure-tallennustilan tilin virhelokit tallentamiseen. Käytä tätä vaihtoehtoa, jos haluat arkistoida tiedot, jotka ovat erä käsitteleminen myöhemmin. Jos valitset tämän vaihtoehdon, sinun on määritettävä Azure-tallennustilan tilin, lokien tallentaminen.
    * Määrittää, haluatko saada valvontalokien tai pyynnön lokit tai molempia.
    * Määritä, jonka tiedot on säilytettävä päivien määrän.
    * Valitse **Tallenna**.

Kun olet ottanut diagnostiikan asetuksia, voit katsoa **Vianmäärityslokit** -välilehden lokit.

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>Näytä vianmäärityslokit järvi tietovaraston tilin

Voit tarkastella lokitiedot järvi tietovaraston tilin kahdella eri tavalla.

* Tietosäilö järvi tilin asetukset-näkymästä
* Azuren tallennustilaan-tililtä, jossa tiedot on tallennettu

### <a name="using-the-data-lake-store-settings-view"></a>Tietojen järvi kaupan asetukset-näkymässä

1. Valitse oman järvi tietovaraston tilin **asetukset** sivu **Vianmäärityslokit**.

    ![Näytä Diagnostiikan kirjaus] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Näytä vianmäärityslokit") 

2. **Vianmäärityslokit** -sivu pitäisi näkyä luokiteltu **Valvontalokien** ja **Pyytää lokit**lokitiedot.
    * Pyyntö lokit sieppaa jokaisen API pyynnön järvi tietosäilö-tilissä.
    * Valvontalokien muistuttavat pyytää lokit mutta ole paljon erittelyn järvi tietovaraston tilin parhaillaan suoritettavia toimintoja. Esimerkiksi yksittäisen lataamisen API-kutsu pyynnön lokien saattaa aiheuttaa useita "Lisää" toimintojen valvontalokien.

3. Napsauta kunkin lokista Lataa lokit vastaan **Lataa** -linkkiä.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Azure-tallennustilan tilin, joka sisältää lokitiedot

1. Avaa liittyvää tietojen järvi kaupan kirjaaminen Azuren tallennustilaan tili-sivu ja valitse sitten BLOB-objektit. **Blob-objektien service** -sivu näyttää kaksi säilöä.

    ![Näytä Diagnostiikan kirjaus] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Näytä vianmäärityslokit")

    * Säilön **tiedot-lokit-valvonta** sisältää valvontalokien.
    * Säilön **tiedot-lokit-pyyntöjen** sisältää pyynnön lokit.

2. Näiden säilöjen sisällä lokit tallennetaan seuraava rakenne-kohdassa.

    ![Näytä Diagnostiikan kirjaus] (./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Näytä vianmäärityslokit")

    Esimerkkinä valvontalokin täydellinen polku voi olla`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary-pyyntö lokia täydellinen polku voi olla`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>Tietoja lokitiedot rakenne

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
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
| operationName   | Merkkijono | Toiminto, joka on kirjautunut nimi. Esimerkiksi getfilestatus.              |
| resultType      | Merkkijono | Toiminto, kuten 200 tila.                                 |
| callerIpAddress | Merkkijono | Pyynnön asiakkaan IP-osoite                                |
| correlationId   | Merkkijono | Lokitiedoston, joka voi tunnus käytetään ryhmittelyyn joukko liittyvät tapahtumat |
| käyttäjätiedot        | Objektin | Tunnistetiedot, jotka on luotu lokiin                                            |
| Ominaisuudet:      | JSON   | Alla on tietoja                                                          |

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
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
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
| operationName   | Merkkijono | Toiminto, joka on kirjautunut nimi. Esimerkiksi getfilestatus.              |
| resultType      | Merkkijono | Toiminto, kuten 200 tila.                                 |
| correlationId   | Merkkijono | Lokitiedoston, joka voi tunnus käytetään ryhmittelyyn joukko liittyvät tapahtumat |
| käyttäjätiedot        | Objektin | Tunnistetiedot, jotka on luotu lokiin                                            |
| Ominaisuudet:      | JSON   | Alla on tietoja                                                          |

#### <a name="audit-log-properties-schema"></a>Valvontalokien lokin ominaisuudet rakenne

| Nimi       | Tyyppi   | Kuvaus                              |
|------------|--------|------------------------------------------|
| StreamName | Merkkijono | Path toiminto on suoritettu  |


## <a name="samples-to-process-the-log-data"></a>Esimerkkejä, joiden käsittelemään lokitiedot

Azure järvi tietosäilö on esimerkki siitä, miten voit käsitellä ja analysoida lokitiedot. Löydät otosten [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample). 


## <a name="see-also"></a>Katso myös

- [Yleistä Azure järvi tietosäilö](data-lake-store-overview.md)
- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)

