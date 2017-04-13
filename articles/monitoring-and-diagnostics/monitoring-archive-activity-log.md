<properties
    pageTitle="Azure tehtävän lokin | Microsoft Azure"
    description="Lue, miten voit arkistoida Azure toimintojen kirjautumistiedot, pitkään säilytys-tallennustilan tilin."
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
    ms.date="08/23/2016"
    ms.author="johnkem"/>

# <a name="archive-the-azure-activity-log"></a>Azure tehtävän lokin
Tässä artikkelissa on Näytä käyttämisestä Azure portal PowerShellin cmdlet-komennot tai Office kaikissa ympäristöissä CLI arkistoon tallennustilan tilin [**Azure tehtävän loki**](monitoring-overview-activity-logs.md) . Tämä asetus on hyödyllinen, jos haluat säilyttää toimintojen kirjautumistiedot yli 90 päivää valvontalokin, staattinen analyysin tai varmuuskopiointi (kanssa säilytyskäytäntö täydet oikeudet). Jos haluat säilyttää tapahtumien 90 päivää tai vähemmän sinun ei tarvitse määrittää arkistointia tallennustilan tilille, koska tehtävän tapahtumien säilyvät Azure ympäristö 90 päivää käyttöön arkistointia.

## <a name="prerequisites"></a>Edellytykset
Ennen kuin aloitat, sinun täytyy [luoda tallennustilan tilin](../storage/storage-create-storage-account.md#create-a-storage-account) , johon voit arkistoida toimintojen kirjautumistiedot. On suositeltavaa, että et käytä käytössä olevan tallennustilan tilin, joka on muiden, seurannan niin, että voit määrittää access-tietojen valvominen paremmin tallennetut tiedot. Jos olet tallentamassa myös vianmäärityslokit ja arvot tallennustilan tilin, se voi esittää kannattaa tehtävän lokin sekä tallennustilan tilin avulla voit pitää kaikki seurantatiedot keskitetyssä paikassa. Voit käyttää tallennustilan tilin on oltava yleisiä tarkoitus tallennustilan tilin ei blob-tallennustilan tilin.

## <a name="log-profile"></a>Log-profiili
Voit arkistoida tehtävän lokin jollakin seuraavista tavoista, voit määrittää tilauksen **Loki profiilin** . Log-profiili määrittää tyyppi tulostaa ja tapahtumia, jotka on tallennettu tai virtauttaa – tallennustilan tilin ja/tai tapahtumaa-toiminnossa. Säilytyskäytännön (päivinä, jos haluat säilyttää) määrittää myös tapahtumat tallennetaan tallennustilan tilin. Jos säilytyskäytäntö on määritetty nolla, tapahtumat tallennetaan jatkuvasti. Muussa tapauksessa tämän voi määrittää, mikä tahansa arvo väliltä 1 – 2147483647. [Voit lukea lisää tietoja log profiilit tähän](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles).

## <a name="archive-the-activity-log-using-the-portal"></a>Tehtävän lokin-portaalissa
1. Valitse vasemman reunan siirtymisosassa **Tehtävän Log** -linkki-portaalissa. Jos et näe tehtävän lokin linkin, valitse **Lisää palveluita** -linkki.

    ![Siirry tehtävän Log-sivu](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Valitse sivu yläreunassa **Vie**.

    ![Valitse Vie-painike](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Sivu, joka tulee näkyviin **Vie tallennustilan tilin** valintaruutu ja valitse tallennustilan tilin.

    ![Tallennustilan tilin määrittämisestä](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Käytä liukusäädintä tai tekstiruutuja, Määritä päivinä, jonka tallennustila-tilisi tulisi säilyttää tapahtumien tehtävän poistaminen. Jos haluat käyttää tietojen samanlainen tallennustilan tilin jatkuvasti, tämän numeron arvoksi nolla.
5. Valitse **Tallenna**.

## <a name="archive-the-activity-log-via-powershell"></a>Tehtävän lokin PowerShellin kautta
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Ominaisuus         | Pakollinen | Kuvaus                                                                                                                                                                                                                                                                                       |
|------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| StorageAccountId | Ei       | Resurssitunnus-tallennustilan tilin, johon lokeja tallennetaan.                                                                                                                                                                                                                        |
| Sijainnit        | Kyllä      | CSV-luettelo, jonka haluat kerätä tehtävän tapahtumien alueet. Voit tarkastella luetteloa, kaikki alueet [ohjesisältöä tämän sivun](https://azure.microsoft.com/en-us/regions) tai käyttämällä [Azure hallinta REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| RetentionInDays  | Kyllä      | Tapahtumat on säilytettävä, väliltä 1 – 2147483647 päivien määrä. Nolla-arvo sisältää lokit toistaiseksi (jatkuvasti).                                                                                                                                                                                             |
| Luokat       | Kyllä      | CSV-luettelo, joka olisi kannettava luokat. Mahdolliset arvot ovat kirjoittaminen ja poista toiminto.                                                                                                                                                                                 |
## <a name="archive-the-activity-log-via-cli"></a>Tehtävän lokin CLI kautta
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Ominaisuus        | Pakollinen | Kuvaus                                                                                                                                                                                                                                                                                       |
|-----------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Nimi            | Kyllä      | Log-profiilin nimi.                                                                                                                                                                                                                                                                         |
| storageId       | Ei       | Resurssitunnus-tallennustilan tilin, johon lokeja tallennetaan.                                                                                                                                                                                                                        |
| sijainnit       | Kyllä      | CSV-luettelo, jonka haluat kerätä tehtävän tapahtumien alueet. Voit tarkastella luetteloa, kaikki alueet [ohjesisältöä tämän sivun](https://azure.microsoft.com/en-us/regions) tai käyttämällä [Azure hallinta REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/gg441293.aspx). |
| retentionInDays | Kyllä      | Tapahtumat on säilytettävä, väliltä 1 – 2147483647 päivien määrä. Nolla-arvo tallennetaan lokit toistaiseksi (jatkuvasti).                                                                                                                                                                                             |
| Luokat      | Kyllä      | CSV-luettelo, joka olisi kannettava luokat. Mahdolliset arvot ovat kirjoittaminen ja poista toiminto.                                                                                                                                                                                 |

## <a name="storage-schema-of-the-activity-log"></a>Tallennustilan rakenteen tehtävän lokin
Kun olet määrittänyt arkistointia-tallennustilan säilö luodaan tallennustilan tilin heti, kun tehtävä lokitapahtuman tapahtuu. BLOB säilöön noudattamalla samassa muodossa yli tehtävän Log ja vianmäärityslokit. Nämä BLOB rakenne on:

> tiedot-toiminnallisia-lokit ja name = oletusarvon/resourceId = / tilaukset / {Tilaustunnus} / y = {numeerinen nelinumeroista} / m = {kaksinumeroinen Numeerinen kuukausi} / d = {kaksinumeroinen numeerinen päivän} / h = {kaksinumeroinen 24 tunnin kelloa hour}/m=00/PT1H.json

Blob-objektien nimi voi olla esimerkiksi:

> insights-Operational-Logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.JSON

Kunkin PT1H.json Blob-objektien sisältää, joka on tapahtunut Blob-objektien URL-osoitteessa määritettyä tunnin kuluessa JSON Blob-objektien (kuten h = 12). Esitä tunnin aikana tapahtumat liitetään PT1H.json tiedoston ilmenee. Arvo minuutit (m = 00) on aina 00, koska tehtävän tapahtumien on jaettava yksittäisiä BLOB tunnissa.

PT1H.json-tiedostossa kuhunkin tapahtumaan on tallennettu "tietueet" matriisissa seuraavan tässä muodossa:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
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
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
            }
        }
    ]
}
```


| Rakenteen nimeä    | Kuvaus                                                                                                    |
|-----------------|----------------------------------------------------------------------------------------------------------------|
| aika            | Kellonaika, jolloin tapahtuma on luotu Azure-palvelu vastaavan tapahtuman pyynnön.    |
| resourceId      | Resurssitunnus sellaisiin resurssin.                                                                          |
| operationName   | Toiminnon nimi.                                                                                         |
| luokka        | Luokka-toiminnon, esimerkiksi. Kirjoita luku-, toiminto.                                                               |
| resultType      | Tulos on tyypin esimerkki. Onnistui-virhe, Käynnistä                                                            |
| resultSignature | Määräytyy resurssin laji.                                                                                  |
| durationMs      | Millisekunteina toiminnon toistaminen                                                                      |
| callerIpAddress | Käyttäjä, joka on suorittanut toiminnon, UPN varaaminen tai SPN varaa käytettävyyden perusteella IP-osoite.         |
| correlationId   | Yleensä GUID-tunnus merkkijono-muodossa. Tapahtumia, joiden correlationId kuuluvat saman uber-toiminnon.         |
| käyttäjätiedot        | JSON blob kuvaava todennus ja saatavat.                                                             |
| todennus   | Tapahtuman ominaisuudet RBAC BLOB. Sisältää yleensä ominaisuudet: "toiminto", "roolin" ja "alue".            |
| taso           | Tapahtuman taso. Jokin seuraavista arvoista: "Kriittinen", "Virhe", "Varoitus", "Tiedottavat" ja "Yksityiskohtainen" |
| sijainti        | Alue, jossa sijainti tapahtui (tai yleinen).                                                             |
| Ominaisuudet:      | Useat `<Key, Value>` paria (eli sanaston) kuvaava tapahtuman tiedot.                             |

> [AZURE.NOTE] Ominaisuudet ja niiden ominaisuuksien käyttö voi vaihdella sen mukaan, resurssi.

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lataa BLOB analysointia varten](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
- [Virta tehtävän lokiin tapahtuman porttiin](monitoring-stream-activity-logs-event-hubs.md)
- [Lisätietoja on artikkelissa tehtävän lokiin](monitoring-overview-activity-logs.md)
