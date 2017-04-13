<properties
    pageTitle="Yleistä Azure tehtävän lokiin | Microsoft Azure"
    description="Tutustu Azure tehtävän lokiin on ja miten voit käyttää sitä ymmärtää Azure-tilauksen piiriin kuuluvien tapahtumien."
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
    ms.date="10/25/2016"
    ms.author="johnkem"/>

# <a name="overview-of-the-azure-activity-log"></a>Azure tehtävän lokin yleiskatsaus
**Azure tehtävän loki** on lokin, joka sisältää huomioon toiminnot, jotka olivat suorittaa resurssit-tilaukseesi. Tehtävän lokiin aiemmin nimeltään "Valvontalokien" tai "Toiminnallisia lokit", koska se raportoi ohjausobjektin tasossa tapahtumien tilauksistasi. Käytä tehtävän lokiin, voit määrittää "mitä, joka ja milloin ' varten jokin kirjoitus-toimintoja (HYLLYTETTY, Kirjaa poistaminen) otetut resurssit-tilaukseesi. Voit myös ymmärtää toiminto ja muut haluamasi ominaisuudet. Tehtävän lokin ei sisällä luku (HAE).

Tehtävän lokin eroaa [Vianmäärityslokit](./monitoring-overview-of-diagnostic-logs.md), jotka ovat kaikki resurssin lähettämän lokit. Lokitiedostot sisältävät tietoja kyseiselle resurssille toiminnon sijaan resurssin toimintoja.

Voit noutaa tapahtumien Azure CLI PowerShell-cmdlet-portaalissa toimintojen kirjautumistiedot ja Azure näytön REST API.

Näytä tämän [videon esittely tehtävän loki](https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz).  

## <a name="what-you-can-do-with-the-activity-log"></a>Miten tehtävän log
Seuraavassa on muutamia esimerkkejä, voit hyödyntää tehtävän lokiin:

- Kysely ja **Azure portal**tarkasteleminen.
- Kyselyn REST API, PowerShell cmdlet-komennon tai CLI kautta.
- [Luo sähköposti- tai webhook ilmoituksen, joka käynnistää tehtävän lokitapahtuman käytöstä.](./insights-auditlog-to-webhook-email.md)
- [Tallentaa sen **Tallennustilan tilin** manuaalisesti tai arkistointia nähtäville](./monitoring-archive-activity-log.md). Voit määrittää säilytys-aika (päivinä), käyttämällä **Log-profiileista**.
- Analysoi-PowerBI käyttämällä [**PowerBI sisällön pack**](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/).
- Kolmannen osapuolen palveluun tai mukautetun analytics-ratkaisun, kuten PowerBI erilaisille [virtauttaa se **Tapahtumaa-toiminnossa** ](./monitoring-stream-activity-logs-event-hubs.md) .

## <a name="export-the-activity-log-with-log-profiles"></a>Tehtävän lokitiedostoon Log-profiileista ja vieminen
**Log-profiilin** ohjaa, miten toimintojen kirjautumistiedot viedään. Log-profiilin avulla, voit määrittää:

- Jos tehtävän lokiin lähetetty (tallennustilan tilin tai tapahtuman keskittimet)
- Tapahtuman luokat (Kirjoita, poista-toiminto) lähetetään
- Mitkä alueet (sijainti) voi viedä
- Kuinka kauan tehtävän lokin säilytettävä tallennustilan tili – nolla päivää säilyttäminen tarkoittaa lokit säilytetään jatkuvasti. Muussa tapauksessa arvo voi olla jokin muu luku 1- 2147483647 välisten päivien määrän. Jos säilytyskäytäntöjä on määritetty, mutta lokien tallentaminen tallennustilan tilillä ei ole käytettävissä (esimerkiksi jos vain tapahtuman keskittimet tai OMS asetukset on valittuna.), säilytyskäytäntöjä ei ole vaikutusta.

Nämä asetukset voi määrittää tehtävän Log-sivu-portaalissa "Vienti"-vaihtoehto kautta. Ne voidaan myös määritetyn ohjelmallisesti [käyttämällä Azure näytön REST-Ohjelmointirajapinta](https://msdn.microsoft.com/library/azure/dn931927.aspx), PowerShellin cmdlet-komennot tai CLI. Tilauksen voi olla vain yksi loki profiili.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Määritä log-profiileista Azure-portaalissa
Voi lähettää tehtävän kirjaa tapahtumaa-toiminnossa ja tallentaa ne tallennustilan tilin Azure-portaalissa "Vienti"-asennustoiminnolla.

1. Siirry-valikon käyttäminen portaalin vasemmalla puolella **Toimintaa Log** -sivu.

    ![Siirry tehtävän lokin-portaalissa](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Valitse **Vie** -painiketta sivu yläreunassa.

    ![Vie-painike-portaalissa](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Sivu, joka tulee näkyviin voit valita:  
    - alueiden, jonka haluat viedä tapahtumat
    - Tallennustilan-tili, johon haluat tallentaa tapahtumat
    - näitä tapahtumia tallennustilaan säilytettävien päivien määrän. Arvo on 0 päivää säilyttää lokit jatkuvasti.
    - missä haluat tapahtumaa-toiminnossa luodaan streaming näitä tapahtumia Service-Bus Namespace.

    ![Vie tehtävän Log-sivu](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Valitse Tallenna asetukset **Tallenna** . Asetuksia käytetään heti tilaukseen.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Määritä log-profiileista Azure PowerShell-cmdlet-komentojen käyttäminen
#### <a name="get-existing-log-profile"></a>Hae loki profiilista
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Lisää log-profiili
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Ominaisuus         | Pakollinen | Kuvaus   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi             | Kyllä      | Log-profiilin nimi.                                 |
| StorageAccountId | Ei       | Resurssitunnus-tallennustilan tilin, johon tehtävä lokiin tallennetaan.                         |
| serviceBusRuleId | Ei       | Palvelun Bus Sääntötunnus palvelun Bus nimitilan haluat on luotu tapahtuma keskittimet. On merkkijono, joka sisältää tässä muodossa: `{service bus resource ID}/authorizationrules/{key name}`. |
| Sijainnit        | Kyllä      | CSV-luettelo, jonka haluat kerätä tehtävän tapahtumien alueet.              |
| RetentionInDays  | Kyllä      | Tapahtumat on säilytettävä, väliltä 1 – 2147483647 päivien määrä. Nolla-arvo sisältää lokit toistaiseksi (jatkuvasti). |
| Luokat       | Ei       | CSV-luettelo, joka olisi kannettava luokat. Mahdolliset arvot ovat kirjoittaminen ja poista toiminto.                                 |

#### <a name="remove-a-log-profile"></a>Poista log-profiili
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Log-profiileista käyttämällä Azure Office kaikissa ympäristöissä CLI määrittäminen
#### <a name="get-existing-log-profile"></a>Hae loki profiilista
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
`name` Ominaisuuden pitäisi olla log-profiilin nimi.

#### <a name="add-a-log-profile"></a>Lisää log-profiili
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Ominaisuus         | Pakollinen | Kuvaus   |
|------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi             | Kyllä      | Log-profiilin nimi.                                 |
| storageId        | Ei       | Resurssitunnus-tallennustilan tilin, johon tehtävä lokiin tallennetaan.                         |
| serviceBusRuleId | Ei       | Palvelun Bus Sääntötunnus palvelun Bus nimitilan haluat on luotu tapahtuma keskittimet. On merkkijono, joka sisältää tässä muodossa: `{service bus resource ID}/authorizationrules/{key name}`. |
| sijainnit        | Kyllä      | CSV-luettelo, jonka haluat kerätä tehtävän tapahtumien alueet.              |
| retentionInDays  | Kyllä      | Tapahtumat on säilytettävä, väliltä 1 – 2147483647 päivien määrä. Nolla-arvo sisältää lokit toistaiseksi (jatkuvasti).     |
| Luokat       | Ei       | CSV-luettelo, joka olisi kannettava luokat. Mahdolliset arvot ovat kirjoittaminen ja poista toiminto.                                 |

#### <a name="remove-a-log-profile"></a>Poista log-profiili
```
azure insights logprofile delete --name my_log_profile
```

## <a name="event-schema"></a>Tapahtuman rakenne
Tehtävän lokiin kuhunkin tapahtumaan on JSON-blob samalla tavalla kuin tässä esimerkissä:

```
{
  "value": [ {
    "authorization": {
      "action": "microsoft.support/supporttickets/write",
      "role": "Subscription Admin",
      "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
    },
    "caller": "admin@contoso.com",
    "channels": "Operation",
    "claims": {
      "aud": "https://management.core.windows.net/",
      "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
      "iat": "1421876371",
      "nbf": "1421876371",
      "exp": "1421880271",
      "ver": "1.0",
      "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
      "puid": "20030000801A118C",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
      "name": "John Smith",
      "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
      "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.microsoft.com/claims/authnclassreference": "1"
    },
    "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "description": "",
    "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
    "eventName": {
      "value": "EndRequest",
      "localizedValue": "End request"
    },
    "eventSource": {
      "value": "Microsoft.Resources",
      "localizedValue": "Microsoft Resources"
    },
    "httpRequest": {
      "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
      "clientIpAddress": "192.168.35.115",
      "method": "PUT"
    },
    "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
    "level": "Informational",
    "resourceGroupName": "MSSupportGroup",
    "resourceProviderName": {
      "value": "microsoft.support",
      "localizedValue": "microsoft.support"
    },
    "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
    "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
    "operationName": {
      "value": "microsoft.support/supporttickets/write",
      "localizedValue": "microsoft.support/supporttickets/write"
    },
    "properties": {
      "statusCode": "Created"
    },
    "status": {
      "value": "Succeeded",
      "localizedValue": "Succeeded"
    },
    "subStatus": {
      "value": "Created",
      "localizedValue": "Created (HTTP Status Code: 201)"
    },
    "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
    "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
    "subscriptionId": "s1"
  } ],
"nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```

| Rakenteen nimeä         | Kuvaus             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| todennus        | Tapahtuman ominaisuudet RBAC BLOB. Sisältää yleensä ominaisuudet: "toiminto", "rooli" ja "alue".|
| soittajan               | Käyttäjä, joka on suorittanut toiminnon, UPN varaaminen tai SPN varaa käytettävyyden perusteella sähköpostiosoite.|
| kanavien             | Jokin seuraavista arvoista: "Järjestelmänvalvoja", "Toiminto"|
| correlationId        | Yleensä GUID-tunnus merkkijono-muodossa. Tapahtumia, joiden correlationId kuuluvat saman uber-toiminnon.   |
| kuvaus          | Tapahtuman kuvaus staattiseksi tekstiksi.                              |
| eventDataId          | Tapahtuman yksilöllinen.    |
| eventSource          | Azure-palvelu tai infrastruktuuri, jotka on luotu tapahtuman nimi.    |
| httpRequest          | Blob-objektien kuvaava Http-pyynnön. Sisältää yleensä "clientRequestId", "clientIpAddress" ja "menetelmä" (HTTP-menetelmää. SIJOITA http://servername,).                            |
| taso                | Tapahtuman taso. Jokin seuraavista arvoista: "Kriittinen", "Virhe", "Varoitus", "Tiedottavat" ja "Yksityiskohtainen"  |
| resourceGroupName    | Sellaisiin resurssin resurssiryhmän nimi.               |
| resourceProviderName | Resurssin tarjoajan sellaisiin resurssin nimi             |
| resourceUri          | Resurssitunnus sellaisiin resurssin.                               |
| toimintotunnukseksi          | GUID-tunnus jakaa tapahtumat, jotka vastaavat yhdellä kertaa.         |
| operationName        | Toiminnon nimi.  |
| Ominaisuudet:           | Useat `<Key, Value>` paria (eli sanaston) kuvaava tapahtuman tiedot.                                |
| tila               | Merkkijono, joka kuvaa toiminnon tilan. Joitakin yleisiä arvot ovat: aloittaminen edistymistä, onnistui, epäonnistui, aktiivinen, ratkaistu.                                |
| alitila            | Yleensä vastaavan muiden HTTP tilakoodi Soita, mutta voi sisältää myös muut merkkijonot kuvaava alitila, kuten yleiset arvot: OK (HTTP Status Code: 200), luotuja (HTTP-tilakoodin: 201), hyväksytyssä (HTTP-tilakoodi: 202), sisältö ei (HTTP Status Code: 204), virheelliset pyytää (HTTP-tilakoodin: 400), ei löydy (HTTP Status Code: 404), ristiriita (HTTP-tilakoodi : 409)-sisäinen palvelinvirhe (HTTP-tilakoodi: 500), palvelu ei ole käytettävissä (HTTP-tilakoodi: 503), yhdyskäytävän aikakatkaisu (HTTP-tilakoodi: 504). |
| eventTimestamp       | Kellonaika, jolloin tapahtuma on luotu Azure-palvelu vastaavan tapahtuman pyynnön.     |
| submissionTimestamp  | Kellonaika, jolloin tapahtuma on ollut käytettävissä kyselyä varten.             |
| subscriptionId       | Azure tilauksen tunnus.  |
| nextLink             | Jatkaminen tunnuksen hakeaksesi tuloksia seuraavan joukko, kun ne on jaettu useita vastauksia. Yleensä tarvittavat on yli 200 tietueita.     |

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja tehtävän Log (aiemmalta nimeltään valvontalokien)](../resource-group-audit.md)
- [Virta Azure tehtävän lokiin tapahtuman porttiin](./monitoring-stream-activity-logs-event-hubs.md)
