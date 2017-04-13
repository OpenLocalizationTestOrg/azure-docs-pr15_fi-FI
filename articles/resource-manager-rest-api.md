<properties
   pageTitle="Resurssienhallinta REST API | Microsoft Azure"
   description="Resurssienhallinta REST API-todennusta ja käyttö esimerkkejä yleiskatsaus"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Resurssienhallinta REST API

> [AZURE.SELECTOR]
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Portal](./azure-portal/resource-group-portal.md) 
- [REST-OHJELMOINTIRAJAPINNALLA](resource-manager-rest-api.md)

Jokaisen kutsun, Azure Resurssienhallinta, jokainen käyttöön malli takana takana takana määritetyn tallennustilan jokaiselle tilille on yhden tai usean Azure Resurssienhallinta RESTful-Ohjelmointirajapinta puhelut. Tässä aiheessa on varattu näiden API ja kuinka voit kutsua niitä käyttämättä mitään SDK. Tämä voi olla erittäin hyödyllistä, jos haluat, että kaikki palvelupyynnöt Azure tai jos ensisijainen kieli SDK-paketissa ei ole käytettävissä tai ei tue toiminnot haluat suorittaa täydet oikeudet.

Tässä artikkelissa ei siirry jokaisen API, joka on määritetty Azure kautta, mutta mieluummin käyttää joitakin esimerkkejä siitä, miten Siirry eteenpäin ja muodostaa yhteyden niihin. Jos tiedät, että voit siirtyä eteenpäin ja lukea yksityiskohtaisia tietoja käyttämisestä API loput [Azure Resurssienhallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn790568.aspx) perusteet.

## <a name="authentication"></a>Todennus
ARM-todennus käsitellään mukaan Azure Active Directory (AD). Jotta voidaan yhdistää kaikki API, sinun on ensin todentamismenetelmä Azure AD vastaanottamaan todennus-tunnuksen, joita voit välittää jokaisen pyynnön. Kun Microsoft on kuvaava täysin puhelun suoraan REST API-on myös olettaa, että et halua Normaali käyttäjänimi salasanalla, jossa ponnahdusikkunat-korotus-näyttöön voi pyytää käyttäjänimeä ja salasanaa ja jopa ehkä muiden käyttöoikeuksien kaksi kerroin todennus tilanteissa menetelmät todennusta. Olemme luodaan vuoksi, mitä kutsutaan Azure AD-sovelluksen ja palvelun lyhennys, jota käytetään, kirjaudu sisään. Mutta muista, että Azure AD tukevat useita todennus-ohjeita ja kaikki ne voidaan hakea, todennus-tunnuksen, seuraavien Ohjelmointirajapinnan pyyntöjen annettava.
Noudata [luominen Azure AD-sovelluksen ja palvelun tarpeen](./resource-group-create-service-principal-portal.md) vaiheittaiset ohjeet.

### <a name="generating-an-access-token"></a>Access-tunnuksen luominen 
Todennus vastaan Azure AD tehdään Azure AD-soittamista login.microsoftonline.com sijaitsee. Jotta todentaa sinut on oltava seuraavat tiedot:

* Azure AD-vuokraajan tunnus (, että käytössäsi on kirjautuminen, usein sama kuin yrityksen, mutta sitä ei tarvita Azure AD nimi)
* Tunnus (tehdyt vaiheessa Azure AD-sovelluksen luominen)
* Salasana (jonka olet valinnut luotaessa Azure AD-sovellus)

Valitse alla HTTP-pyyntö Varmista, että korvaa "Azure AD-vuokraajan tunnus", "Tunnus" ja "Salasana" oikeat arvot.

**Yleinen HTTP-pyyntö:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... näkyy (Jos todennus onnistuu) johtaa samalla vastauksen tähän:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Edellä vastauksessa access_token on lyhennetty, voit parantaa luettavuutta)

**Luodaan käyttämällä Bash käyttöoikeustietue:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Luodaan käyttöoikeustietue PowerShellin avulla:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Vastaus sisältää Access-tunnuksen, tietoja siitä, kuinka kauan, että tunnus on kelvollinen ja tietoja siitä, mitä resurssin voit määrittää, että tunnus.
Olet saanut edellisen HTTP-kutsun käyttöoikeustietue on siirrettävä kaikki pyynnön ARM-ohjelmointirajapinnan otsikoksi nimeltä "Lupa" arvona "Haltijan YOUR_ACCESS_TOKEN". Huomaa "Haltijan" ja Access-tunnuksen välisen etäisyyden.

Kuten näet yllä tuloksesta HTTP-tunnus on voimassa tietyn ajanjakson aikana olisi välimuistin ja käyttää uudelleen, että sama tunnuksen. Vaikka ei todennetaan kunkin API-kutsu Azure Active Directory-olisi erittäin tehotonta.

## <a name="calling-arm-rest-apis"></a>Soittavan KÄDESSÄ REST API

[Azure Resurssienhallinta REST API on kuvattu seuraavassa](https://msdn.microsoft.com/library/azure/dn790568.aspx) ja se on tämä opetusohjelma, jotta asiakirjan jokaiselle ja jokaisen käyttö alueen ulkopuolella. Näitä ohjeita vain käyttää muutaman ohjelmointirajapinnan kertoaksesi basic käyttö API ja sen jälkeen voit uutta virallinen ohjeissa.

### <a name="list-all-subscriptions"></a>Luettele kaikki tilaukset

Yksi helpoin toimintoja, tee on luettelo käytettävissä tilaukset, jotka voivat käyttää. Valitse näet, miten Access-tunnuksen on välitetty otsikkona pyynnön alla.

(Korvaa YOUR_ACCESS_TOKEN oman todellinen Access-tunnuksen.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

..., näyttöön tulee tilaukset, jotka tämä palvelu lyhennys on oikeus käyttää luettelo

(Tilauksen tunnukset alla on lyhennetty, luettavuuden)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Luettele kaikki tietyn tilauksen resurssiryhmät

Kaikki resurssit, jotka ovat käytettävissä ARM-ohjelmointirajapinnan on ylemmän tason sisällä resurssiryhmä. Tässä tapauksessa kyselyn KÄDESSÄ aiemmin resurssin ryhmien Microsoftin tilauksen käyttämisen alla HTTP GET-pyyntö. Huomaa, miten tilauksen tunnus on välitetty URL-Osoitteen osana tällä hetkellä.

(Korvaa YOUR_ACCESS_TOKEN ja Tilaustunnus, todellinen Access tunnuksen ja Tilaustunnus)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Saat vastauksen sen mukaan, onko resurssi-ryhmistä määritetty ja jos näin on, kuinka monta.

(Tilauksen tunnukset alla on lyhennetty, luettavuuden)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Resurssiryhmän luominen

Tähän mennessä on olet vain ole kyselyt lisätietoja ARM-ohjelmointirajapinnan, on aika on seuraavia resursseja luominen sijaan ja aloitetaan helpoin ne kaikki resurssiryhmän mukaan. HTTP-pyyntö luo uusi resurssiryhmä valittua alue/sijainti ja Lisää vähintään yksi tunnisteet (esimerkki alla itse asiassa ainoastaan Lisää yhden tunnisteen).

(Korvaa YOUR_ACCESS_TOKEN, Tilaustunnus, RESOURCE_GROUP_NAME, todellinen Access-tunnuksen, Tilaustunnus ja haluat luoda resurssiryhmän nimi)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Jos onnistuu, näyttöön tulee tämä samalla vastauksen

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Olet luonut resurssiryhmä Azure-tietokannassa. Onnittelen!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Ota käyttöön resursseja resurssiryhmän ARM-mallin avulla

ARM-ja resurssien ARM-mallien avulla voit ottaa käyttöön. ARM-malli määrittää useita resursseja ja niiden riippuvuuksia. Tässä osassa vain oletetaan ovat sinulle tuttuja ARM-mallit ja vain esitellään, miten API puhelun Aloita jokin käyttöönotto. Löytyy yksityiskohtaiset ohjeet ARM-malleista.

ARM-mallin käyttöönoton ei vaihtelevat paljon kuinka muut ohjelmointirajapinnan Soita. Yksi projektinhallintaa on mallin käyttöönotto voi kestää jonkin aikaa, sen mukaan, mitä sisältyy mallin ja API-kutsu palauttaa vain ja valinta on sinulle kehittäjän kyselyn käyttöönoton tilan haluat selvittää, kun asennus on valmis.

Tässä esimerkissä käytetään käytettävissä julkisesti näkyviä ARM mallin käyttöön [GitHub](https://github.com/Azure/azure-quickstart-templates). Tarkastellaan käyttämään malli otetaan käyttöön Linux AM Länsi US-alueelle. Vaikka tämä malli on käytettävissä mallin julkisen säilöön, kuten GitHub, voit valita myös välittää koko mallin pyynnön osana. Huomaa, että annamme parametriarvot pyynnön, jota käytetään sisällä käytetty mallin osana.

(Korvaa Tilaustunnus, RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD ja DNS_NAME_FOR_PUBLIC_IP arvoja, jotka sopivat pyyntö)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Jotta voit parantaa luettavuutta näissä ohjeissa on jätetty pois aivan pitkä JSON vastaus pyyntö. Vastauksen sisältää juuri luomaasi malliin perustuvan käyttöönoton tietoja.

