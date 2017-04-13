<properties
    pageTitle="Azure Resurssienhallinta käytännön | Microsoft Azure"
    description="Tässä artikkelissa käsitellään Azure resurssien hallinta käytännön avulla voit estää osoitteessa eri alueiden, kuten tilauksen, resurssiryhmiä tai yksittäisiä resursseja."
    services="azure-resource-manager"
    documentationCenter="na"
    authors="ravbhatnagar"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="gauravbh;tomfitz"/>

# <a name="use-policy-to-manage-resources-and-control-access"></a>Resurssien ja käyttöoikeuksien hallinta käytännön avulla

Azure Resurssienhallinta avulla voit hallita kautta mukautetut käytännöt nyt. Käytäntöjä, jossa voit estää käyttäjiä organisaation katkaisemisen käytäntöjä, joita tarvitaan hallita organisaation resursseja. 

Voit luoda käytännön määritteitä, jotka kuvaavat toiminnot tai resurssit, jotka on erikseen estetty. Voit määrittää kyseiset käytännön määritykset haluamasi alueessa, kuten tilaus, resurssiryhmä tai yksittäinen resurssi. 

Tässä artikkelissa kerromme käytännön määritys kieli, jolla voit luoda käytäntöjä perusrakenteen. Tämän jälkeen on kuvaus siitä, miten voit käyttää eri käyttöalueita, kyseiset käytännöt.

## <a name="how-is-it-different-from-rbac"></a>Miten se on sama kuin RBAC?

On joitakin keskeisiä eroja käytäntö ja Roolipohjainen käyttöoikeuksien valvonta, mutta Huomaa, että tietoja on käytännöt ja RBAC toimivat yhdessä. Käyttämään käytäntöjä, sinun on todennetaan RBAC kautta. Toisin kuin RBAC-käytäntö on oletusarvoisesti salli ja eksplisiittinen estä järjestelmän. 

RBAC ohjeessa on toimintoja, **käyttäjä** voi tehdä eri alueiden etsiminen. Tietyn käyttäjän lisätään esimerkiksi osallistujan rooli resurssiryhmän haluamasi alueessa, joten käyttäjä voi tehdä muutoksia resurssin ryhmään. 

Käytännön ohjeessa on **resurssin** toiminnot eri alueiden etsiminen. Esimerkiksi käytäntöjen avulla voit hallita resurssityyppiä, joka on valmisteltu tai rajoittaa sijainnit, jossa on valmisteltu resurssit.

## <a name="common-scenarios"></a>Yleisiä tilanteita, joissa

Yksi käytetty vaihtoehto on vaadi osaston tunnisteet palautus tarkoitus. Organisaation on hyvä sallimaan toiminnot vain, jos tarvittavat kustannuspaikka liitetään; Muussa tapauksessa ne hylätä pyynnön. Tämän käytännön avulla ne veloittaa toimintoja kustannuspaikka.

Toisen käytetty vaihtoehto on organisaation kannattaa määrittää, missä resurssit luodaan sijainnit. Tai he haluavat ohjaa resurssit sallimalla vain tietyntyyppisiä valmisteltava resurssit.

Vastaavasti organisaation voit hallita palvelua luettelon tai Pakota haluamasi nimeämiskäytännöt resurssien.

Käytä käytäntöjä, näissä tilanteissa helposti onnistuu.

## <a name="policy-definition-structure"></a>Käytännön rakenne

Käytännön määritys luodaan JSON. Se koostuu ehdot ja loogiset operaattorit, jotka määrittävät toiminnot ja tehosteen, joka kertoo, mitä tapahtuu, kun ehdot täyttyvät. Rakenne on julkaistu [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

Lähinnä-käytännön luominen sisältää seuraavat osat:

**Ehto ja loogiset operaattorit:** tietyt ehdot, jonka avulla voidaan läpi loogiset operaattorit.

**Vaikutus:** mitä tapahtuu, kun ehto täyttyy – estää tai valvonta. Valvonta-tehosteen tietokoneesta kuuluu äänimerkki varoitus palvelun tapahtumaloki. Järjestelmänvalvoja voi esimerkiksi luoda käytännön, joka aiheuttaa valvonta-tapahtuman, jos kuka tahansa Luo suuria AM. Järjestelmänvalvoja voi tarkastella lokit myöhemmin.

    {
      "if" : {
          <condition> | <logical operator>
      },
      "then" : {
          "effect" : "deny | audit | append"
      }
    }
    
## <a name="policy-evaluation"></a>Käytännön arviointi

Käytännöt arvioidaan, kun resursseja on luotu. Jos mallin käyttöönottoa käytännöt arvioidaan kullekin resurssille mallin luonnin aikana. 

> [AZURE.NOTE] Käytäntöä ei tällä hetkellä arvioi resurssityypit, jotka eivät tue tunnisteita, tyyppi ja sijainti, kuten Microsoft.Resources/deployments resurssin laji. Tämä tuki lisätään myöhemmin. Yhteensopivuus aiempien versioiden kanssa ongelmien välttämiseksi tulee määrittää tyyppi eksplisiittisesti laatimisen käytännöt. Esimerkiksi tunnisteen käytännön, joka ei ole määritetty tyypit otetaan käyttöön kaikissa. Siinä tapauksessa mallin käyttöönottoa saattaa epäonnistua, onko sisäkkäisiä resurssi, joka ei tue tunnisteet ja käyttöönoton resurssin laji on lisätty käytännön arviointiin. 

## <a name="logical-operators"></a>Loogiset operaattorit

Tuetut loogisilla operaattoreilla sekä sen syntaksi on:

| Operaattorin nimi     | Syntaksi         |
| :------------- | :------------- |
| Ei            | "ei": {&lt;ehto tai operaattori &gt;}             |
| Ja           | "allOf": [{&lt;ehto tai operaattori &gt;}, {&lt;ehto tai operaattori &gt;}] |
| Tai                         | "käyttöoikeuttaan": [{&lt;ehto tai operaattori &gt;}, {&lt;ehto tai operaattori &gt;}] |

Resurssien hallinnan avulla voit määrittää monimutkaisia logiikan sisäkkäisiä operaattoreita – käytännön. Voit esimerkiksi estää resurssin luomista varten määritettyä resurssityyppiä tietyssä paikassa. Esimerkki sisäkkäisiä operaattorit näkyy alla.

## <a name="conditions"></a>Ehdot

Ehto, **kenttä** tai **tietolähteen** tietyt ehdot. Tuetut ehto nimien ja syntaksi on:

| Ehdon nimi | Syntaksi                |
| :------------- | :------------- |
| Yhtä suuri kuin             | "on": "&lt;arvo&gt;"               |
| Kuten                  | "tykätä": "&lt;arvo&gt;"                   |
| Sisältää          | "sisältää": "&lt;arvo&gt;"|
| Valitse                        | "in": ["&lt;arvo1&gt;";"&lt;arvo2&gt;"]|
| ContainsKey    | "containsKey": "&lt;avain&gt;" |
| On olemassa     | "on": "&lt;bool&gt;" |

### <a name="fields"></a>Kentät

Ehdot on muotoiltu käyttämällä kentät ja lähteistä. Kenttä vastaa resurssien pyynnön-paketti, jota käytetään kuvaus resurssin tilan ominaisuudet. Lähteen edustaa ominaisuudet pyynnön itse. 

Seuraavat kentät ja tietolähteiden tuetaan:

Kentät: **nimi**, **tyyppi**, **tyyppi**, **sijainti**, **tunnisteet** **tunnisteita.** *, and * *ominaisuuden alias**. 

### <a name="property-aliases"></a>Ominaisuuden tunnukset 
Ominaisuuden tunnus on nimi, jonka avulla voidaan käyttää resurssin laji ominaisuudet, kuten asetukset ja tuotteissa käytännön määritys. Se toimii kaikissa kaikki API-versiot, kun ominaisuus on olemassa. ALIASES voi hakea käyttämällä REST-Ohjelmointirajapinnalla (Powershell tuki lisätään myöhemmin) alla:

    GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
    
Alias määritelmä on alla. Kuten näet, sähköpostitunnus määrittää polut API versiosta, vaikka ominaisuuden nimi muuttuu. 

    "aliases": [
        {
          "name": "Microsoft.Storage/storageAccounts/sku.name",
          "paths": [
            {
              "path": "properties.accountType",
              "apiVersions": [
                "2015-06-15",
                "2015-05-01-preview"
              ]
            },
            {
              "path": "sku.name",
              "apiVersions": [
                "2016-01-01"
              ]
            }
          ]
        }
    ]

Tällä hetkellä tuetut aliakset ovat seuraavat:

| Aliaksen nimi | Kuvaus |
| ---------- | ----------- |
| {resourceType}/sku.name | Tuetut resurssityypit ovat: Microsoft.Compute/virtualMachines,<br />Microsoft.Storage/storageAccounts,<br />Microsoft.Web/serverFarms,<br /> Microsoft.Scheduler/jobcollections,<br />Microsoft.DocumentDB/databaseAccounts,<br />Microsoft.Cache/Redis,<br />Microsoft.CDN/profiles |
| {resourceType}/sku.family | Tuetut resurssilaji on Microsoft.Cache/Redis |
| {resourceType}/sku.capacity | Tuetut resurssilaji on Microsoft.Cache/Redis |
| Microsoft.Compute/virtualMachines/imagePublisher |  |
| Microsoft.Compute/virtualMachines/imageOffer  |  |
| Microsoft.Compute/virtualMachines/imageSku  |  |
| Microsoft.Compute/virtualMachines/imageVersion  |  |
| Microsoft.Cache/Redis/enableNonSslPort |  |
| Microsoft.Cache/Redis/shardCount |  |
| Microsoft.SQL/servers/version |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveId |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveName |  |
| Microsoft.SQL/servers/databases/edition |  |
| Microsoft.SQL/servers/databases/elasticPoolName |  |
| Microsoft.SQL/servers/elasticPools/dtu |  |
| Microsoft.SQL/servers/elasticPools/edition |  |

Käytännön toimii tällä hetkellä vain HYLLYTETTY pyynnöt. 

## <a name="effect"></a>Vaikutus
Käytännön tukee tehoste - kolmen erityyppisen **Estä**, **valvoa**ja **Liitä**. 

- Estä luo tapahtuman valvontalokin ja kutsu epäonnistuu
- Valvonta luo tapahtuman valvontaloki, mutta ei onnistu pyyntö
- Lisää Lisää kentät määritettyjä pyyntö 

**Liittäminen**sinun on määritettävä seuraavat asiat:

    ....
    "effect": "append",
    "details": [
      {
        "field": "field name",
        "value": "value of the field"
      }
    ]

Arvo voi olla merkkijono tai JSON-objektin muotoileminen. 

## <a name="policy-definition-examples"></a>Käytännön määritys-esimerkkejä

Nyt katsotaan siitä, miten Microsoft Määritä käytäntö saavuttamiseksi edellä kuvattujen tilanteiden.

### <a name="chargeback-require-departmental-tags"></a>Palautus: Edellyttävät osaston tunnisteet

Seuraavat käytännön hylkää pyynnöt, joka ei ole tunniste sisältää "costCenter"-näppäintä.

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

Seuraavat käytännön Lisää costCenter-tunniste, jonka ennalta määritetyn arvon tunnisteita ei ole käytettävissä. 

    {
      "if": {
        "field": "tags",
        "exists": "false"
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags",
            "value": {"costCenter":"myDepartment" }
          }
        ]
      }
    }
    
Seuraavilla Lisää costCenter-tunniste, jonka ennalta määritetyn arvon, kun costCenter tunnistetta ei ole, mutta muita tunnisteita ovat näkyvissä. 

    {
      "if": {
        "allOf": [
          {
            "field": "tags",
            "exists": "true"
          },
          {
            "field": "tags.costCenter",
            "exists": "false"
          }
        ]
    
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags.costCenter",
            "value": "myDepartment"
          }
        ]
      }
    }


### <a name="geo-compliance-ensure-resource-locations"></a>GEO yhteensopivuus: Varmista Resurssiensijainnit

Seuraavassa esimerkissä käytännön, joka estää pyynnöt, jossa sijainti ei ole Pohjois Euroopassa tai Länsi Europe.

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="service-curation-select-the-service-catalog"></a>Palvelun Curation: Valitse palvelu-luettelo

Seuraavassa esimerkissä käytännön, joka sallii toiminnot vain Valitse palvelut-tyypin Microsoft.Resources/\*, Microsoft.Compute/\*, Microsoft.Storage/\*, Microsoft.Network/\* sallitaan. Muu on estetty.

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "field" : "type",
              "like" : "Microsoft.Resources/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Compute/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Storage/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="use-approved-skus"></a>Käytä hyväksytty tuotteissa

Seuraavassa esimerkissä ominaisuuden alias tuotteissa rajoittaa käyttöä. Esimerkki vain Standard_LRS ja Standard_GRS hyväksytään tallennustilan tilit.

    {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "not": {
              "allof": [
                {
                  "field": "Microsoft.Storage/storageAccounts/sku.name",
                  "in": ["Standard_LRS", "Standard_GRS"]
                }
              ]
            }
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
    

### <a name="naming-convention"></a>Nimeämiskäytäntö

Seuraavassa esimerkissä esitetään yleismerkkejä, jota tue ehto "like" käyttö. Ehto ilmoittaa Jos nimi vastaa mainitut kaavaa (namePrefix\*nameSuffix) hylätä pyynnön.

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }
    
### <a name="tag-requirement-just-for-storage-resources"></a>Tunnisteen vaatimus vain tallennustilan resurssit

Seuraavassa esimerkissä esitetään sijoittamisesta loogisilla operaattoreilla edellyttää sovelluksen tunnisteen vain tallennustilan resurssit.

    {
        "if": {
            "allOf": [
              {
                "not": {
                  "field": "tags",
                  "containsKey": "application"
                }
              },
              {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
              }
            ]
        },
        "then": {
            "effect": "audit"
        }
    }

## <a name="policy-assignment"></a>Käytännön määritys

Eri alueiden, kuten tilauksen resurssiryhmien ja yksittäisten resurssien käytetään käytännöt. Käytännöt periytyvät kaikki lapsen resurssit. Näin on, jos käytäntö otetaan käyttöön resurssiryhmä-on koskevat kaikki kyseisen resurssiryhmä resurssit.

## <a name="creating-a-policy"></a>Käytännön luominen

Tässä osassa on tiedot siitä, miten käytännön voidaan luoda REST API avulla.

### <a name="create-policy-definition-with-rest-api"></a>Luo käytännön määritys REST-ohjelmointirajapinnalla

Voit luoda käytäntöä [Käytäntöjen määritelmät REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/mt588471.aspx). REST API mahdollistaa luominen ja poistaminen käytäntöjen määritelmät ja määritelmät tietoja.

Suorita-käytännön luominen:

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

Jonka pyynnön leipäteksti samalla tavalla kuin seuraavassa esimerkissä:

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "name":"testdefinition"
    }


Käytä *2016-04-01*api-versio. Esimerkkejä ja Lisätietoja on artikkelissa [REST API käytännön määritykset](https://msdn.microsoft.com/library/azure/mt588471.aspx).

### <a name="create-policy-definition-using-powershell"></a>Luo käytännön määritys PowerShellin avulla

Voit luoda käytännön määritys uusi AzureRmPolicyDefinition cmdlet-komennolla. Seuraavassa esimerkissä luodaan, jolla sallitaan resurssien vain Pohjois-Eurooppa ja Länsi Europe käytännön.

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{  
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'          

Suorittamisen tulos on tallennettu $policy objekti ja voidaan myöhemmin käytännön määrityksen aikana. Käytännön-parametrin käytännön sisältävä .json-tiedoston polku voidaan antaa myös sen sijaan, että sidottua käytännön määrittäminen.

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain    regions" -Policy "path-to-policy-json-on-disk"

### <a name="create-policy-definition-using-azure-cli"></a>Käyttämällä Azure CLI käytännön määrityksen luominen

Voit luoda käytännön määritys-komennon alla kuvatulla tavalla azure CLI käyttäminen käytännön määritys. Seuraavassa esimerkissä luodaan, jolla sallitaan resurssien vain Pohjois-Eurooppa ja Länsi Europe käytännön.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{   
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'    
    

Se on mahdollista määrittää sen sijaan, että käytännön sidottua alla kuvatulla tavalla, joka määrittää käytännön sisältävän .json-tiedoston polku.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"


## <a name="applying-a-policy"></a>Käytäntö otetaan käyttöön

### <a name="policy-assignment-with-rest-api"></a>Käytännön määritys REST-ohjelmointirajapinnalla

Voit käyttää käytännön määritys haluamasi alueessa [REST API käytännön varausten](https://msdn.microsoft.com/library/azure/mt588466.aspx)kautta. REST-Ohjelmointirajapinnalla avulla voit luoda ja poistaa käytännön tehtävät ja tietoja aiemmin luoduista varauksista.

Voit luoda käytännön määrityksen suorittamalla:

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

{Käytännön varauksen} on käytännön tehtävän nimi. Käytä *2016-04-01*api-versio. 

Jonka pyynnön leipäteksti samalla tavalla kuin seuraavassa esimerkissä:

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "name":"VMPolicyAssignment"
    }

Esimerkkejä ja Lisätietoja on artikkelissa [REST API käytännön varausten](https://msdn.microsoft.com/library/azure/mt588466.aspx).

### <a name="policy-assignment-using-powershell"></a>Käytännön määritys PowerShellin avulla

Voit käyttää käytännön edellä luotu PowerShellin kautta halutun alueen uusi AzureRmPolicyAssignment cmdlet-komennolla:

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Näin $policy ryhmäkäytäntöobjekti, joka palautettiin tuloksena suoritetaan uusi AzureRmPolicyDefinition cmdlet yllä esitetyllä tavalla. Tähän alue on resurssiryhmän määrittämäsi nimi.

Jos haluat poistaa yllä käytännön varauksen, voit tehdä sen seuraavasti:

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Voit saada, muuttaminen tai poistaminen käytäntöjen määritelmät Get-AzureRmPolicyDefinition, Määritä AzureRmPolicyDefinition ja poista AzureRmPolicyDefinition cmdlet-komennot tarpeen mukaan.

Voit vastaavasti Hae, muuttaminen tai poistaminen käytännön tehtävät – Hae AzureRmPolicyAssignment, Määritä AzureRmPolicyAssignment ja poista AzureRmPolicyAssignment cmdlet-komennot tarpeen mukaan.

### <a name="policy-assignment-using-azure-cli"></a>Käytännön kuormituksen Azure CLI

Voit käyttää käytännön edellä luotu kautta Azure CLI halutun alueen käytännön määritys-komennon avulla:

    azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/<policy-name> --scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Tähän alue on resurssiryhmän määrittämäsi nimi. Jos parametrin käytännön-määritys-tunnus-arvo on tuntematon, on mahdollista saada Azure-CLI kautta, alla kuvatulla tavalla: 

    azure policy definition show <policy-name>

Jos haluat poistaa yllä käytännön varauksen, voit tehdä sen seuraavasti:

    azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Voit saada, muuttaminen tai poistaminen käytännön määritelmät – käytännön määritys näyttää, määrittää, ja poistaa komentoja tarpeen mukaan.

Voit vastaavasti Hae, muuttaminen tai poistaminen käytännön tehtävät – käytännön varauksen näyttäminen ja poistaa komentoja tarpeen mukaan.

##<a name="policy-audit-events"></a>Käytännön valvontalokin tapahtumien

Kun olet lisännyt käytäntöjen, voit aloittaa Nähdäksesi käytännön liittyvät tapahtumat. Voit joko Siirry-portaaliin, käytä PowerShell tai Azure-CLI saat tämän tiedot. 

### <a name="policy-audit-events-using-powershell"></a>Käytännön valvontalokin tapahtumien PowerShellin avulla

Voit tarkastella kaikki tapahtumat, jotka liittyvät estä tehosteen, käyttämällä PowerShell-komentoa:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/deny/action"} 

Jos haluat tarkastella kaikki tapahtumat, jotka liittyvät valvoa tehosteen, voit käyttää seuraava komento:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/audit/action"} 

### <a name="policy-audit-events-using-azure-cli"></a>Käytännön valvontalokin tapahtumien Azure CLI käyttäminen

Voit tarkastella kaikkia tapahtumia resurssin ryhmitellä liittyvää estä tehosteen, voit käyttää CLI seuraava komento:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/deny/action\")"

Jos haluat tarkastella kaikki tapahtumat, jotka liittyvät valvoa tehosteen, voit käyttää CLI seuraava komento:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/audit/action\")"
