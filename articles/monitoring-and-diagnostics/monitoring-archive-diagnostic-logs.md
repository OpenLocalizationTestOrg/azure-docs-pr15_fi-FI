<properties
    pageTitle="Arkistoida Azure vianmäärityslokit | Microsoft Azure"
    description="Opettele arkistoida Azure vianmäärityslokit pitkään säilytys-tallennustilan tilin varten."
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
    ms.date="08/26/2016"
    ms.author="johnkem"/>

# <a name="archive-azure-diagnostic-logs"></a>Azure vianmäärityslokit arkistoiminen
Tässä artikkelissa on Näytä käyttämisestä Azure portal PowerShellin cmdlet-komennot, CLI tai REST API arkistoon [Azure vianmäärityslokit](monitoring-overview-of-diagnostic-logs.md) tallennustilan tilillä. Tämä asetus on hyödyllinen, jos haluat säilyttää oman vianmäärityslokit valinnainen säilytyskäytäntö valvonta, staattinen analyysin tai varmuuskopion kanssa.

## <a name="prerequisites"></a>Edellytykset
Ennen kuin aloitat, sinun täytyy [luoda tallennustilan tilin](../storage/storage-create-storage-account.md#create-a-storage-account) , johon voit arkistoida oman vianmäärityslokit. On suositeltavaa, että et käytä käytössä olevan tallennustilan tilin, joka on muiden, seurannan niin, että voit määrittää access-tietojen valvominen paremmin tallennetut tiedot. Jos olet tallentamassa myös tehtävän Log ja diagnostiikan arvot tallennustilan tilin, se voi esittää kannattaa pitää kaikki seurantatiedot keskitetysti diagnostiikan lokitiedot sekä tallennustilan tilin avulla. Voit käyttää tallennustilan tilin on oltava yleisiä tarkoitus tallennustilan tilin ei blob-tallennustilan tilin.

## <a name="diagnostic-settings"></a>Diagnostiikka-asetusten muuttaminen
Voit arkistoida oman vianmäärityslokit jollakin seuraavista tavoista tietyn resurssin **Diagnostiikan asetusten** määrittäminen. Diagnostiikan asetus varten resurssin määrittää luokkia kirjautuu, että ne, jotka on tallennettu tai virtauttaa ja tulostaa – tallennustilan tilin ja/tai tapahtumaa-toiminnossa. Säilytyskäytännön (päivinä, jos haluat säilyttää) määrittää myös tapahtumat tallennetaan tallennustilan tilin log kunkin luokan. Jos säilytyskäytännön on määritetty nolla, loki luokkaan tapahtumien tallennetaan toistaiseksi (joka on sanoa jatkuvasti). Säilytyskäytännön muuten voi olla jokin muu luku 1- 2147483647 välisten päivien määrän. [Voit lukea lisää tässä diagnostiikan asetuksista](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).

## <a name="archive-diagnostic-logs-using-the-portal"></a>Arkisto-portaalissa vianmäärityslokit

1. Valitse-portaalissa huomioon resurssin, jossa haluat ottaa käyttöön arkistointia vianmäärityslokit, resurssi-sivu.
2. Valitse resurssi-asetukset-valikossa **valvonta** -osiossa **Diagnostiikka**.

    ![Resurssi-valikon seuranta](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-sec.png)
3. **Tallennustilan tilin vieminen**valintaruutu ja valitse sitten tallennustilan tilin. Määrittää halutessasi säilyttää lokitiedostot käyttämällä päivien määrä **säilytys (päiviä)** liukusäätimiä. Nolla päivää säilyttäminen tallentaa lokit jatkuvasti.

    ![Diagnostiikan lokit-sivu](media/monitoring-archive-diagnostic-logs/diag-log-monitoring-blade.png)
4. Valitse **Tallenna**.

Vianmäärityslokit arkistoidaan tallennustilan tiliin, kun uuden tapahtumatiedot luodaan.

## <a name="archive-diagnostic-logs-via-the-powershell-cmdlets"></a>Arkisto vianmäärityslokit kautta PowerShellin cmdlet-komennot

```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Ominaisuus         | Pakollinen | Kuvaus                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| ResourceId       | Kyllä      | Resurssitunnus resurssin, johon haluat määrittää diagnostiikan asetusten.                            |
| StorageAccountId | Ei       | Resurssitunnus-tallennustilan tilin, johon vianmäärityslokit tallennetaan.                          |
| Luokat       | Ei       | CSV-Ota loki aiheluokkien luetteloa.                                                     |
| Käytössä          | Kyllä      | Totuusarvo, joka ilmaisee, onko diagnostiikka käytössä vai poissa käytöstä, valitse resurssi.                  |
| RetentionEnabled | Ei       | Totuusarvo, joka ilmaisee, jos säilytyskäytännön ovat käytössä tämän resurssin.                            |
| RetentionInDays  | Ei       | Jonka tapahtumat on säilytettävä väliltä 1 – 2147483647 päivien määrä. Nolla-arvo sisältää lokit jatkuvasti. |

## <a name="archive-the-activity-log-via-the-cross-platform-cli"></a>Tehtävän lokin Office kaikissa ympäristöissä CLI kautta

```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Ominaisuus         | Pakollinen | Kuvaus                                                                                           |
|------------------|----------|-------------------------------------------------------------------------------------------------------|
| resourceId       | Kyllä      | Resurssitunnus resurssin, johon haluat määrittää diagnostiikan asetusten.                            |
| storageId        | Ei       | Resurssitunnus-tallennustilan tilin, johon vianmäärityslokit tallennetaan.                          |
| Luokat       | Ei       | CSV-Ota loki aiheluokkien luetteloa.                                                     |
| käytössä          | Kyllä      | Totuusarvo, joka ilmaisee, onko diagnostiikka käytössä vai poissa käytöstä, valitse resurssi.                  |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Arkisto vianmäärityslokit kautta REST-Ohjelmointirajapinnalla
[Katso tämän asiakirjan](https://msdn.microsoft.com/library/azure/dn931931.aspx) on tietoja siitä, miten Azure näytön REST-Ohjelmointirajapinnan käyttäminen diagnostiikan asetusten määrittäminen.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Rakenne-tallennustilan tilin vianmäärityslokit
Kun olet määrittänyt arkistointia, tallennustilan säilö luodaan tallennustilan tilin heti, kun tapahtuman ilmenee jokin log-luokka on otettu käyttöön. BLOB säilöön noudattamalla samassa muodossa vianmäärityslokit ja tehtävä-loki. Nämä BLOB rakenne on:

> tiedot - lokit-{kirjaa luokan nimen} / resourceId = / tilaukset / {Tilaustunnus} /RESOURCEGROUPS/ {resurssiryhmän nimi} /PROVIDERS/ {resurssin tarjoajan nimi} / {resurssin tyypin} / {resurssinimi} / y = {numeerinen nelinumeroista} / m = {kaksinumeroinen Numeerinen kuukausi} / d = {kaksinumeroinen numeerinen päivän} / h = {kaksinumeroinen 24 tunnin kelloa hour}/m=00/PT1H.json

Tai Lisää yksinkertaisesti

> tiedot - lokit-{kirjaa luokan nimen} / resourceId = / {resurssin tunnus} / y = {numeerinen nelinumeroista} / m = {kaksinumeroinen Numeerinen kuukausi} / d = {kaksinumeroinen numeerinen päivän} / h = {kaksinumeroinen 24 tunnin kelloa hour}/m=00/PT1H.json

Blob-objektien nimi voi olla esimerkiksi:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json

Kunkin PT1H.json Blob-objektien sisältää JSON-blob, joka on tapahtunut Blob-objektien URL-osoitteessa määritettyä tunnin kuluessa (esimerkiksi h = 12). Esitä tunnin aikana tapahtumat liitetään PT1H.json tiedoston ilmenee. Arvo minuutit (m = 00) on aina 00, koska diagnostiikan tapahtumien poistaminen on jaettava yksittäisiä BLOB tunnissa.

PT1H.json-tiedostossa kuhunkin tapahtumaan on tallennettu "tietueet" matriisissa seuraavan tässä muodossa:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Rakenteen nimeä  | Kuvaus                                                                                                 |
|---------------|-------------------------------------------------------------------------------------------------------------|
| aika          | Kellonaika, jolloin tapahtuma on luotu Azure-palvelu vastaavan tapahtuman pyynnön. |
| resourceId    | Resurssitunnus sellaisiin resurssin.                                                                       |
| operationName | Toiminnon nimi.                                                                                      |
| luokka      | Tapahtuman log-luokka.                                                                                  |
| Ominaisuudet:    | Useat `<Key, Value>` paria (eli sanaston) kuvaava tapahtuman tiedot.                            |

> [AZURE.NOTE] Ominaisuudet ja niiden ominaisuuksien käyttö voi vaihdella sen mukaan, resurssi.

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lataa BLOB analysointia varten](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Virta vianmäärityslokit tapahtuman porttiin](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Lisätietoja on artikkelissa vianmäärityslokit](monitoring-overview-of-diagnostic-logs.md)
