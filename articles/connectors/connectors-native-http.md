<properties
    pageTitle="HTTP-toiminnon lisääminen logiikan sovelluksissa | Microsoft Azure"
    description="HTTP-toiminto, jonka ominaisuuksien yleiskatsaus"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>HTTP-toiminnon käytön aloittaminen

HTTP-toiminnolla voit laajentaa työnkulut organisaation ja toimitettava kaikki päätepisteen HTTP-yhteyden kautta.

Voit:

- Luo logiikka sovelluksen työnkulut, joita Aktivoi (käynnistimen), kun sivustoon, jossa voit hallita siirtyy.
- Toimitettava kaikki päätepisteen HTTP laajentaa muiden palvelujen työnkulkuja.

Aloita HTTP-toiminnon avulla logiikan-sovelluksessa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>HTTP-käynnistimen käyttäminen

Käynnistimen tapahtuma, jonka avulla voidaan käynnistää työnkulun, joka on määritetty logiikan-sovelluksessa. [Lisätietoja käynnistimien](connectors-overview.md).

Näin voit määrittää logiikan sovelluksen suunnittelussa HTTP-käynnistimen Esimerkki-järjestystä.

1. Lisää HTTP-käynnistimen logiikan-sovelluksessa.
2. Täytä HTTP päätepiste, jonka haluat kyselyn parametrit.
3. Valitse, kuinka usein se olisi kyselyn toistovälin muuttaminen
4. Logiikan sovellus käynnistyy heti sisällöllä, joka palautetaan kunkin tarkistuksen aikana.

![HTTP-käynnistin](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>HTTP-käynnistimen toiminta

HTTP-käynnistimen avulla HTTP-päätepisteen puhelun toistuva väli. Oletusarvon mukaan mitään HTTP-vastaus koodin alle 300 tulokset Suorita logiikan-sovelluksessa. Voit lisätä ehdon koodinäkymän, joka arvioi jälkeen voit selvittää, jos logiikan sovelluksen pitäisi Kyselysäännön HTTP-puhelu. Esimerkki HTTP-käynnistimen, joka käynnistyy, kun Palautettu tilakoodi on suurempi tai yhtä suuri kuin `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Täydelliset HTTP käynnistimen parametrit ovat käytettävissä [MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger)-sivuston.

## <a name="use-the-http-action"></a>HTTP-toiminnolla

Toiminto on toiminto, joka suoritetaan työnkulun, joka on määritetty logiikan-sovelluksessa. [Lisätietoja toiminnot](connectors-overview.md).

1. Valitse **Uusi vaihe** -painike.
2. Valitse **Lisää toiminnon**.
3. Kirjoita toiminto-hakuruutuun **http** luettelon HTTP-toiminto.

    ![Valitse HTTP-toiminto](./media/connectors-native-http/using-action-1.png)

4. Voit lisätä parametreja, joita tarvitaan HTTP-puhelu.

    ![HTTP-toiminto suoritetaan](./media/connectors-native-http/using-action-2.png)

5. Valitse työkalurivillä Tallenna vasemmassa yläkulmassa. Logiikan sovelluksen sekä Tallenna ja julkaise (Aktivoi).

## <a name="http-trigger"></a>HTTP-käynnistin

Seuraavassa on tietoja käynnistimen, joka tukee Tämä yhdistin. HTTP-yhdistin on yksi käynnistin.

|Käynnistin|Kuvaus|
|---|---|
|HTTP|Tekee HTTP-kutsun ja palauttaa vastauksen sisältö.|

## <a name="http-action"></a>HTTP-toiminto

Seuraavassa on tietoja toiminnon, joka tukee Tämä yhdistin. HTTP-yhdistin on yksi mahdollista toiminto.

|Toiminto|Kuvaus|
|---|---|
|HTTP|Tekee HTTP-kutsun ja palauttaa vastauksen sisältö.|

## <a name="http-details"></a>HTTP-tiedot

Seuraavassa taulukossa kuvataan pakolliset ja valinnaiset syötteen kentät toiminnon ja vastaavat tulostustiedot, jotka liittyvät-toiminnolla.


#### <a name="http-request"></a>HTTP-pyyntö
Seuraavassa on toiminto, joka tekee lähtevän HTTP-pyynnön syötteen kentät.
A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Näyttönimi|Ominaisuuden nimi|Kuvaus|
|---|---|---|
|Menetelmä *|menetelmä|HTTP-toiminnon käyttäminen|
|URI *|URI|HTTP-pyyntö URI-tunnus|
|Otsikot|otsikot|JSON-objektia, jonka sisältämään HTTP-otsikot|
|Tekstissä|tekstissä|HTTP-pyyntö tekstissä|
|Todennus|todennus|[Todentaminen](#authentication) ‑osassa tiedot|
<br>

#### <a name="output-details"></a>Tulostustiedot

Seuraavassa taulukossa on tulostustiedot HTTP-vastauksen.

|Ominaisuuden nimi|Tietotyyppi|Kuvaus|
|---|---|---|
|Otsikot|objektin|Vastausten otsikot|
|Tekstissä|objektin|Vastauksen objekti|
|Tilakoodin|kokonaisluku|HTTP-tilakoodi|

## <a name="authentication"></a>Todennus

Azure App palvelun logiikan sovellukset-ominaisuuden avulla voit käyttää erityyppisiä todennus vastaan HTTP päätepisteet. Voit käyttää tätä todennusta **HTTP**, **[HTTP + Swagger](./connectors-native-http-swagger.md)**ja **[HTTP Webhook](./connectors-native-webhook.md)** yhdistimillä. Seuraavanlaisia todennus on määritettävä:

* [Perustodentamista](#basic-authentication)
* [Asiakasvarmenteen todennus](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth-todennus](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Perustodentamista

Seuraavan objektin todennus tarvitaan perustodentamista.
A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Ominaisuuden nimi|Tietotyyppi|Kuvaus|
|---|---|---|
|Kirjoita *|tyyppi|Todennustapa (on oltava `Basic` perustodentamista varten)|
|Käyttäjänimi *|käyttäjänimi|Käyttäjänimi tarkistamiseen|
|Salasanan *|salasana|Salasanan tarkistamiseen|

>[AZURE.TIP] Jos haluat käyttää salasana, jota ei voi hakea määritelmää, käytä `securestring` parametrin ja `@parameters()` [työnkulun määritys-funktiota](http://aka.ms/logicappdocs).

Niin luot objektin tältä tarkistus-kenttään:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Asiakasvarmenteen todennus

Seuraavan objektin todennus tarvitaan Asiakasvarmenteen todennus. A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Ominaisuuden nimi|Tietotyyppi|Kuvaus|
|---|---|---|
|Kirjoita *|tyyppi|Tyyppi-todennus (on oltava `ClientCertificate` asiakkaan SSL-varmenteiden käytön)|
|PFX *|PFX|Henkilökohtaisia tietoja Exchange (PFX)-tiedoston sisällön Base64-koodattu|
|Salasanan *|salasana|Access-tiedoston PFX salasana|

>[AZURE.TIP] Voit käyttää `securestring` parametrin ja `@parameters()` [työnkulun määritys-funktion](http://aka.ms/logicappdocs) käyttäminen parametri, joka ei voi lukea määrityksessä logiikan sovelluksen tallentamisen jälkeen.

Esimerkki:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD-OAuth-todennus

Seuraavan objektin todennus tarvitaan Azure AD OAuth todennusta varten. A * tarkoittaa, että se on muuttaminen pakolliseksi kentäksi.

|Ominaisuuden nimi|Tietotyyppi|Kuvaus|
|---|---|---|
|Kirjoita *|tyyppi|Tyyppi-todennus (on oltava `ActiveDirectoryOAuth` Azure AD OAuth varten)|
|Vuokraajan *|vuokraajan|Azure AD-vuokraajan vuokraajan tunnus|
|Käyttäjäryhmän *|käyttäjäryhmälle|Asettaminen`https://management.core.windows.net/`|
|Asiakkaan tunnus *|clientId|Azure AD-sovelluksen asiakas-tunniste|
|Salainen *|salainen|Asiakas, joka pyytää tunnuksen salaisuus|

>[AZURE.TIP] Voit käyttää `securestring` parametrin ja `@parameters()` [työnkulun määritys-funktion](http://aka.ms/logicappdocs) käyttäminen parametri, joka ei voi lukea määrityksessä tallentamisen jälkeen.

Esimerkki:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Kokeile nyt-ympäristön ja [logiikka sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md). Voit tutkia muita käytettävissä yhdistimet logiikan sovelluksissa katsomalla [API-luettelosta](apis-list.md).
