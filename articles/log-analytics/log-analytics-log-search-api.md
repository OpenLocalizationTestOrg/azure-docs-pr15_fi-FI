<properties
    pageTitle="Lokitiedoston Analytics Kirjaudu haun REST API | Microsoft Azure"
    description="Tässä oppaassa kerrotaan, miten voit käyttää lokiin Analytics-haun REST API toimintojen hallinta Suite (OMS) ja se sisältää esimerkkejä, kuinka voit käyttää komentoja basic opetusohjelman."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="log-analytics-log-search-rest-api"></a>Lokitiedoston Analytics Kirjaudu haun REST-Ohjelmointirajapinnalla

Tässä oppaassa kerrotaan, miten voit käyttää lokiin Analytics haun REST-Ohjelmointirajapinnalla toimintojen hallinta Suite (OMS) ja se sisältää esimerkkejä, kuinka voit käyttää komentoja basic opetusohjelman. Osa tämän artikkelin esimerkkejä viitata toiminnallisia havainnollistamisen Log Analytics aiemman version nimi.

## <a name="overview-of-the-log-search-rest-api"></a>Log-haun REST API yleiskatsaus

Log Analytics haun REST-Ohjelmointirajapinta on RESTful ja käyttää Azure Resurssienhallinta-Ohjelmointirajapinnan kautta. Tässä asiakirjassa löydät esimerkkejä mistä Ohjelmointirajapinnan käytetään [ARMClient](https://github.com/projectkudu/ARMClient), Avaa lähde komentoriviltä työkalua, joka helpottaa käynnistettäessä Azure Resurssienhallinta-Ohjelmointirajapinnan kautta. ARMClient ja PowerShell on useita vaihtoehtoja, voit käyttää lokiin Analytics haun Ohjelmointirajapinta. Toinen vaihtoehto on käyttää Azure PowerShell-moduulin OperationalInsights, joka sisältää cmdlet-komennot käyttäminen haun. Näillä työkaluilla voit hyödyntää RESTful Azure Resurssienhallinta-Ohjelmointirajapinta soittaa OMS työtilojen ja tee haku-komennot, niiden välillä. Ohjelmointirajapinnan siirtää hakutulosten sinulle JSON-muodossa, jolloin voit käyttää hakutulosten ohjelmallisesti monella tavalla.

Azure-Resurssienhallinta voidaan [.NET kirjaston](https://msdn.microsoft.com/library/azure/dn910477.aspx) sekä [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx)kautta. Lue lisätietoja linkitetyn verkkosivujen.

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Basic Log Analytics haun REST API-opas

### <a name="to-use-the-arm-client"></a>ARM-asiakasohjelman käyttäminen

1. Asenna [Chocolatey](https://chocolatey.org/), joka on Avaa lähde-paketin hallinta Windowsin. Avaa komentokehoteikkuna järjestelmänvalvojana ja suorittamalla seuraavan komennon:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```

2. Asenna ARMClient suorittamalla seuraavan komennon:

    ```
    choco install armclient
    ```

### <a name="to-perform-a-simple-search-using-the-armclient"></a>Yksinkertainen haku ARMClient käyttäminen

1. Kirjaudu sisään Microsoft- tai OrgID tiliisi:

    ```
    armclient login
    ```

    Onnistumisen kirjautumisen näyttää kaikki tietyn tiliin liitetty tilaukset:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```

2. Hae toimintojen hallinta Suite työtilat:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Onnistuneen Get-puhelun tulostaa kaikki työtilat, jotka on liitetty tilaukseen:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. Etsi muuttujan luominen

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. Etsi uusi haku-muuttuja käyttämällä:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Kirjaudu Analytics haun REST-Ohjelmointirajapinta viittaus-esimerkkejä
Seuraavissa esimerkeissä Näytä haun Ohjelmointirajapinta käyttämisestä.

### <a name="search---actionread"></a>Haku - toiminnon/luettu

**Esimerkki URL-osoite:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**Pyyntö:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
Seuraavassa taulukossa on kuvattu käytettävissä olevat ominaisuudet.

|**Ominaisuus**|**Kuvaus**|
|---|---|
|alkuun|Tulosten enimmäismäärä.|
|korostaminen|Sisältää vastaavat kentät korostaminen käytetään yleensä pre ja kirjaa parametrit|
|muotoon|Lisää parilliset kenttiin merkkijonosta.|
|Kirjaa|Lisää merkkijonosta niitä vastaavat kentät.|
|kyselyn|Hakukyselyn avulla pystyit keräämään ja palauttaa tuloksia.|
|aloittaminen|Haluat tulosten löydy aikaikkunan alkuun.|
|Lopeta|Haluat tulosten löydy aikaikkunan loppuun.|


**Vastaus:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Etsi / {tunnus} - toiminto/luettu

**Pyydä tallennettu haun sisältö:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

>[AZURE.NOTE] Jos haku palauttaa Odottaa-tilassa, kyselyt päivitetyt tulokset voidaan toteuttaa tämän API-Liittymän kautta. 6 min jälkeen haun tulos poistetaan välimuistista ja HTTP suoriutunut palautetaan. Jos alkuperäisen hakupyyntöä palautti "Onnistunut" tilan heti, sitä ei lisätä välimuistiin aiheuttaa tämän API palauttaa HTTP suoriutunut, jos kysely. HTTP-200 tuloksen sisällöt on samassa muodossa kuin alkuperäinen hakupyyntöä vain päivitetty arvoilla.

### <a name="saved-searches---rest-only"></a>Tallennetut haut - vain muille käyttäjille

**Pyydä tallennetun hakujen luettelo:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Tuetut menetelmät: GET valitseminen poistaminen

Tuetut sivustokokoelman menetelmät: hankkiminen

Seuraavassa taulukossa on kuvattu käytettävissä olevat ominaisuudet.

|Ominaisuus|Kuvaus|
|---|---|
|Tunnus|Yksilöllinen tunnus.|
|ETag|**Korjaustiedoston pakollinen**. Päivittää kunkin kirjoitus-palvelin. Arvon on oltava sama nykyisen tallennetun arvon tai "*" Päivitä. 409 palautettiin vanha tai virheellisiä arvoja.|
|Properties.Query|**Pakollinen**. Hakukyselyn.|
|properties.displayName|**Pakollinen**. Kyselyn määritetyn näytön käyttäjänimi. Jos mallintaa Azure resurssiksi, tämä olisi tunnisteen.|
|Properties.category|**Pakollinen**. Käyttäjän määrittämät kyselyn luokka. Jos mallintaa Azure resurssiksi olisi tunnisteen.|

>[AZURE.NOTE] Lokitiedoston Analytics haun Ohjelmointirajapinta tällä hetkellä palauttaa käyttäjän luoma tallennetut haut, kun kohdistaa pyyntöjä työtilan tallennetun rajaamalla. Ohjelmointirajapinnan ei palauta tallennettuja hakuja antama ratkaisuja tällä hetkellä. Tämä toiminto lisätään myöhemmin.

### <a name="create-saved-searches"></a>Luo tallennetut haut

**Pyyntö:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="delete-saved-searches"></a>Poista tallennettuja haut

**Pyyntö:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Päivityksen tallennettu haut

 **Pyyntö:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metatieto - JSON vain

Näin voi tarkastella työtilan kerättyjä tietoja lokin kaikentyyppisillä kenttiä. Esimerkiksi jos haluat, että jos tapahtumatyyppi on tietokoneen nimi-kenttä, valitse tämä on yksi tapa ja Vahvista.

**Pyydä kentät:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Vastaus:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

Seuraavassa taulukossa on kuvattu käytettävissä olevat ominaisuudet.

|**Ominaisuus**|**Kuvaus**|
|---|---|
|Nimi|Kenttänimi.|
|Näyttönimi|Kentän nimen.|
|tyyppi|Kentän arvon tyyppi.|
|facetable|Yhdistelmä nykyinen "indeksoitu", "tallennettu ' ja 'pinta-ominaisuudet.|
|Näyttää|Nykyinen näyttää' ominaisuus. TOSI, jos kenttä on näkyvissä hakutoiminnolla.|
|ownerType|Pienentää vain tyypit, jotka kuuluvat onboarded IP.|


## <a name="optional-parameters"></a>Valinnaisten parametrien
Seuraavassa kuvataan valinnaisten parametrien käytettävissä.

### <a name="highlighting"></a>Korostaminen

"Korostus-parametri on valinnainen parametri voit pyytää haku-alirakenne sisällyttäminen joukko merkit sen vastaus.

Seuraavat merkit Määritä alkamis- ja loppumiskohdat korostetun tekstin, joka vastaa hakukysely ehtoja.
Voit määrittää alkamis- ja merkintöjä, jota käytetään haku rivitys korostetun termi.

**Esimerkki hakukyselyn**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**Esimerkki tulos:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Huomaa, että edellä tulos sisältää virhesanoma, jonka etuliite ja joka on lisätty.

## <a name="computer-groups"></a>Tietokoneryhmät

Tietokoneryhmät ovat erityisiä tallennetut haut, joka palauttaa joukon tietokoneita.  Voit muiden kyselyiden tietokoneen ryhmän rajoita hakutuloksia ryhmän tietokoneisiin.  Tietokoneen ryhmän on toteutettu ryhmä-tunniste tietokoneen arvona tallennetun haun.

Seuraavassa on esimerkki vastauksen tietokoneen ryhmän.

    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
        }],
    "Version": 1
    }

### <a name="retrieving-computer-groups"></a>Tietokoneen ryhmien hakeminen

Ryhmä-tunnuksella Get-menetelmän avulla voit noutaa tietokoneen ryhmän.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Luominen tai päivittäminen tietokoneryhmä

Voit tehdä menetelmän avulla yksilöllisen tallennettu haun tunnuksen tietokoneen uuden ryhmän luominen. Jos käytät aiemmin tietokoneen Ryhmätunnus, sitä muokataan. Tietokoneen ryhmän luomisen OMS konsolissa, tunnus luodaan ryhmän ja nimen.

Ryhmämääritys kyselyä on palautettava joukon tietokoneita ryhmän toimii oikein.  On suositeltavaa lopettaa kyselyn kanssa *| Eri tietokoneen* varmistaa oikeat tiedot palautetaan.

Määritys on tallennettu haku on oltava ryhmän nimeltä tietokoneen haun luokitellaan tietokoneen ryhmänä arvona tunnisteen.

    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson

### <a name="deleting-computer-groups"></a>Tietokoneen ryhmien poistaminen

Tietokoneen ryhmän poistaminen Ryhmätunnus Delete-menetelmä avulla.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [lokin haut](log-analytics-log-searches.md) luonnissa kyselyjen hakuehtona mukautettuja kenttiä.
