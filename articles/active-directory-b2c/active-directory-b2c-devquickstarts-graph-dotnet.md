<properties
    pageTitle="Azure Active Directory-B2C: Käytä kaavio API | Microsoft Azure"
    description="Voit kutsua Graph-Ohjelmointirajapinnan B2C-vuokraajan käyttämällä voit automatisoida sovelluksen käyttäjätiedot."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-use-the-graph-api"></a>Azure AD-B2C: Käytä Graph Ohjelmointirajapinta

Azure Active Directory (Azure AD) B2C alihallinnat, jotka yleensä hyvin suuri. Tämä tarkoittaa, että useita yleisiä vuokraajan hallintatehtäviä on suoritettava ohjelmallisesti. Käyttäjien hallinta on ensisijainen esimerkiksi. Voit joutua siirtämään aiemmin luodun käyttäjän myymälän B2C vuokraajalle. Voit halutessasi isännöidä käyttäjän rekisteröinti oman sivulla ja luoda käyttäjätilejä Azure AD taustalla. Nämä tehtävätyypit edellyttävät voi luoda, lukea, päivittää ja poistaa käyttäjätilejä. Voit tehdä seuraavat tehtävät käyttämällä Azure AD-kaavio-Ohjelmointirajapinnan.

B2C-vuokraajiin on kaksi ensisijainen tilat yhteydessä Graph-Ohjelmointirajapinnan kanssa.

- Vuorovaikutteinen, kerran tehtävät B2C vuokraajan järjestelmänvalvojatilin olisi edustajana tehtäviä suoritettaessa. Tässä tilassa edellyttää järjestelmänvalvojan Kirjaudu sisään, järjestelmänvalvoja voi suorittaa Graph-ohjelmointirajapinnan puhelujen.
- Automaattinen, jatkuva tehtävien käytettävä jonkin palvelutilin, voit antaa hallintatehtävien suorittamiseen tarvittavat oikeudet. Azure AD voit tehdä tämän sovelluksen rekisteröiminen ja Azure AD todennustapa. Tämä on valmis käyttämällä **Tunnus** , jonka [myöntää OAuth 2.0 asiakkaan käyttäjätiedot](../active-directory/active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api). Tässä tapauksessa sovellus toimii itse ei käyttäjänä Soita Graph-Ohjelmointirajapinnan.

Tässä artikkelissa käsitellään tietoja automaattisesta käyttötapaus. Osoittaa, että luominen .NET 4.5 `B2CGraphClient` , joka suorittaa käyttäjän luominen, lukeminen, päivittäminen ja poistaminen (CRUD)-toimintoja. Asiakas on Windowsin käyttöliittymä (CLI), jonka avulla voit käynnistää eri tavoilla. Kuitenkin koodi kirjoitetaan toimimaan ilman vuorovaikutteisuutta, automaattisten tavalla.

## <a name="get-an-azure-ad-b2c-tenant"></a>Hae Azure AD B2C vuokraajan

Ennen kuin voit luoda sovelluksia tai käyttäjät tai käsitellä Azure AD lainkaan, sinun on Azure AD B2C alihallintaan ja alihallintaan Yleinen järjestelmänvalvoja-tilin. Jos sinulla ei ole vuokraajan jo [Azure AD B2C käytön aloittaminen](active-directory-b2c-get-started.md).

## <a name="register-a-service-application-in-your-tenant"></a>Rekisteröi palvelusovelluksen vuokraajan

Kun sinulla on B2C palvelutili, sinun täytyy luoda palvelusovelluksen käyttämällä Azure AD-PowerShellin cmdlet-komennot.
Tarkista ensin Lataa ja asenna- [Microsoft Online Services-kirjautuminen avustajan](http://go.microsoft.com/fwlink/?LinkID=286152). Lataa ja asenna [64-bittisen Windows PowerShellin Azure Active Directory-moduuli](http://go.microsoft.com/fwlink/p/?linkid=236297).

> [AZURE.IMPORTANT]
Graph-Ohjelmointirajapinnan käyttäminen B2C vuokraajan, sinun on erillinen sovelluksen rekisteröinti PowerShell-toiminnolla. Tehdä tämän artikkelin noudattamalla ohjeita. Et voi käyttää jo olemassa B2C sovellukset, jotka olet rekisteröitynyt Azure-portaalissa.

Kun olet asentanut PowerShell-moduulin, Avaa PowerShell- ja muodosta yhteys B2C alihallintaan. Kun olet suorittanut `Get-Credential`, näyttöön tulee kehotus varten käyttäjänimi ja salasana, kirjoita käyttäjänimi ja B2C vuokraajan järjestelmänvalvojatilin salasana.

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

Ennen kuin luot sovelluksen, sinun täytyy luoda uuden **asiakas salaisuus**.  Sovellus käyttää asiakkaan salaisuus todentaa Azure AD- ja hankkia Accessin tunnusten. Voit luoda kelvollinen salaisuus powershellissä:

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

Uusi asiakas-salaisuus tulostaa lopulliseksi-komennolla. Kopioi se turvallisessa. Tarvitset niitä myöhemmin. Voit nyt luoda sovelluksen antamalla uusi asiakas salaisuus nimellä olevan tunnistetiedon sovelluksen:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

Jos luot sovelluksen onnistuneesti, se kannattaa tulostaa yllä ilmestyä-sovelluksen ominaisuudet. Sinun on sekä `ObjectId` ja `AppPrincipalId`, kopioi niin liian kyseisistä arvoista.

Kun olet luonut sovelluksen B2C vuokraajan, sinun täytyy määrittää sen käyttöoikeudet, se täytyy suorittaa käyttäjän CRUD. Määrittää sovelluksen kolme roolit: hakemiston lukijat (lukemaan käyttäjät), hakemiston kirjoittajien (Luo ja Päivitä käyttäjät), ja käyttäjä-tilin järjestelmänvalvojaan (Jos haluat poistaa käyttäjät). Rooli on tunnettu tunnukset, jotta voit korvata `-RoleMemberObjectId` parametrin `ObjectId` edellä ja suorittamalla seuraavat komennot. Jos haluat nähdä kaikki kansion roolien yrittää luettelon `Get-MsolRole`.

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

Sinun on nyt sovellus, joka on oikeus luoda, lukea, päivittää, ja poista käyttäjiä B2C alihallintaan.

## <a name="download-configure-and-build-the-sample-code"></a>Lataa, määrittäminen ja mallikoodin luominen

Ensin Lataa mallikoodin ja hoida käynnissä. Valitse Microsoft kestää tarkempi katsaus.  Voit [ladata sample code .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip). Voit myös Kloonaa se valittua kansioon:

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

Avaa `B2CGraphClient\B2CGraphClient.sln` Visual Studio-ratkaisun Visual Studiossa. Valitse `B2CGraphClient` projekti, Avaa tiedosto `App.config`. Korvaa kolme sovelluksen asetusten oman arvot:

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Seuraavaksi, napsauta hiiren kakkospainikkeella `B2CGraphClient` ratkaisu ja muodosta malli. Jos ole onnistunut, pitäisi nyt olla `B2C.exe` suoritettava tiedosto sijaitsee `B2CGraphClient\bin\Debug`.

## <a name="build-user-crud-operations-by-using-the-graph-api"></a>Voit muodostaa käyttäjän CRUD toimintojen Graph-Ohjelmointirajapinta

Käyttämään B2CGraphClient Avaa `cmd` Windows-komennon kehotteeseen ja muuttaa hakemistossa, `Debug` hakemisto. Suorita `B2C Help` komento.

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

Tässä näkyvät jokaisen komennon lyhyt kuvaus. Aina, kun suoritat näitä komentoja `B2CGraphClient` tekee pyyntö Azure AD-kaavio-Ohjelmointirajapinnan.

### <a name="get-an-access-token"></a>Access-tunnuksen hankkiminen

Graph-ohjelmointirajapinnan pyyntö edellyttää todennusta varten access-tunnuksen. `B2CGraphClient`käyttää auttamaan hankkia Accessin tunnuksia Avaa lähde Active Directory käyttöoikeuksien kirjaston (ADAL). ADAL helpottaa tunnuksen hankkiminen tarjoaa yksinkertaisen API ja tärkeät tiedot, kuten välimuistin access tunnusten huolehtivat. Sinun ei tarvitse ADAL avulla voit hakea tunnuksia, vaikka. Saat myös tunnusten työstämistä pyyntöjen perusteella.

> [AZURE.NOTE]
    Koodin tässä esimerkissä käytetään ADAL v2 jotta Graph-Ohjelmointirajapinnan yhteydessä.  Sinun on käytettävä ADAL v2 tai v3, jotta saat access tunnuksia, joita voidaan käyttää Azure AD-kaavio-Ohjelmointirajapinnan kanssa.

Kun `B2CGraphClient` suoritetaan, se luo esiintymän `B2CGraphClient` luokka. Tähän luokkaan konstruktorin määrittää ADAL todennus-rakennustelineet:

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are
    // provided to Azure AD in order to receive an access_token by using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

Käytämme `B2C Get-User` komennon esimerkkinä. Kun `B2C Get-User` käynnistetään ilman mitään muita syötteiden CLI puhelut `B2CGraphClient.GetAllUsers(...)` menetelmää. Tämä menetelmä soittaa `B2CGraphClient.SendGraphGetRequest(...)`, joka lähettää HTTP GET-pyyntö Graph-ohjelmointirajapinnan. Ennen `B2CGraphClient.SendGraphGetRequest(...)` lähettää GET-pyyntö, se ensin saa access suojaustunnuksen ADAL avulla:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token by using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ...

```

Voit avata suojaustunnuksen Graph API soittamalla ADAL `AuthenticationContext.AcquireToken(...)` menetelmää. Palauttaa ADAL `access_token` , joka edustaa sovelluksen tunnistetiedot.

### <a name="read-users"></a>Luku-käyttäjät

Kun haluat käyttäjien luettelo tai saada tietyn käyttäjän Graph-Ohjelmointirajapinta, voit lähettää HTTP `GET` pyytää `/users` päätepiste. Kaikkien käyttäjien palvelutili pyyntö näyttää seuraavanlaiselta:

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Voit tarkastella pyyntö suorittamalla:

 ```
 > B2C Get-User
 ```

On kaksi tärkeää asiaa huomioita:

- ADAL kautta hankittu käyttöoikeustietue lisätään `Authorization` käyttämällä ylä `Bearer` värimallin.
- B2C alihallinnoista, sinun on käytettävä kyselyn parametri `api-version=1.6`.

Molemmat nämä tiedot käsitellään `B2CGraphClient.SendGraphGetRequest(...)` menetelmää:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user management, be sure to use the 1.6 Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=1.6";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request by using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ...
```

### <a name="create-consumer-user-accounts"></a>Kuluttaja Käyttäjätilien luominen

Kun luot käyttäjätilit B2C vuokraajan, voit lähettää HTTP `POST` pyytää `/users` päätepisteen:

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // All of these properties are required to create consumer users.

    "accountEnabled": true,
    "signInNames": [                            // controls which identifier the user uses to sign in to the account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "LocalAccount",            // always set to 'LocalAccount'
    "displayName": "Joe Consumer",              // a value that can be used for displaying to the end user
    "mailNickname": "joec",                     // an email alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

Kuluttaja käyttäjien luomiseen tarvitaan useimpia pyyntö ominaisuuksia. Saat lisätietoja napsauttamalla [tätä](https://msdn.microsoft.com/library/azure/ad/graph/api/users-operations#CreateLocalAccountUser). Huomaa, että `//` kommentit on lisätty kuva. Älä sisällytä ne varsinaisen pyynnön.

Voit tarkastella pyynnön suorittamalla jompikumpi seuraavista komennoista:

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

`Create-User` Komennolla syöteparametria .json tiedoston. Sisältää käyttäjäobjektin JSON esittäminen. Esimerkki-koodi on kaksi .json mallitiedostojen: `usertemplate-email.json` ja `usertemplate-username.json`. Voit muokata näitä tiedostoja haluamallasi tavalla. Pakolliset kentät on yllä lisäksi valinnaisia kenttiä, joita voit käyttää sisältyvät nämä tiedostot. Valinnainen kenttien tiedot löytyvät [Azure AD Graph API kohteesta](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity).

Näet, miten POST-pyynnössä on rakennettava `B2CGraphClient.SendGraphPostRequest(...)`.

- Se kiinnittyy access-tunnistetta `Authorization` pyynnön otsikko.
- Määrittää `api-version=1.6`.
- Se sisältää JSON käyttäjäobjektin pyynnön tekstiosaan.

> [AZURE.NOTE]
Jos tilit, jotka haluat siirtää käyttäjän-Storesta on pienempi kuin [vahva salasana voimakkuus Azure AD B2C toimialuerekisteröintipalvelun](https://msdn.microsoft.com/library/azure/jj943764.aspx)salasanan voimakkuus, voit poistaa vahva salasana vaatimus avulla `DisableStrongPassword` arvo `passwordPolicies` ominaisuus. Esimerkiksi muokkaat Luo käyttäjäpyynnön annettu yläpuolella seuraavasti: `"passwordPolicies": "DisablePasswordExpiration, DisableStrongPassword"`.

### <a name="update-consumer-user-accounts"></a>Päivitä kuluttaja-käyttäjätilit

Kun päivität käyttäjäobjekteja, prosessi on samankaltainen kuin käytät käyttäjän objektien luomiseen. Mutta tämä prosessi käyttää HTTP `PATCH` menetelmää:

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request updates only the user's displayName
}
```

Yritä päivittää käyttäjän päivittämällä JSON tiedostojen uusilla tiedoilla. Voit käyttää `B2CGraphClient` suorittaa näitä komentoja:

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

Tarkasta `B2CGraphClient.SendGraphPatchRequest(...)` menetelmä lisätietoja Lähetä pyyntö.

### <a name="search-users"></a>Etsi-käyttäjät

Voit etsiä käyttäjiä B2C vuokraajan usealla eri tavalla. Yhden käyttäjän käyttämällä objektin tunnus tai toisen käyttäjän tunnukseen Kirjaudu sisään käyttämällä (eli `signInNames` ominaisuutta).

Suorita jompikumpi seuraavista komennoista, voit etsiä tietyn käyttäjän:

```
> B2C Get-User <user-object-id>
> B2C Get-User <filter-query-expression>
```

Seuraavassa on muutamia esimerkkejä:

```
> B2C Get-User 2bcf1067-90b6-4253-9991-7f16449c2d91
> B2C Get-User $filter=signInNames/any(x:x/value%20eq%20%27joeconsumer@gmail.com%27)
```

### <a name="delete-users"></a>Käyttäjän poistaminen

Käyttäjän poistaminen on helppoa. Käytä HTTP `DELETE` menetelmä ja objektin tunnus rakennetta URL-Osoitteen oikein:

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=1.6
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

Esimerkki, kirjoita Tämä komento ja tarkastele Poista pyynnön, joka tulostuu konsoliin:

```
> B2C Delete-User <object-id-of-user>
```

Tarkasta `B2CGraphClient.SendGraphDeleteRequest(...)` menetelmä lisätietoja Lähetä pyyntö.

Voit suorittaa useita muita toimintoja lisäksi Käyttäjähallinta Azure AD-kaavio-Ohjelmointirajapinnan kanssa. [Azure AD Graph API viittaus](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) sisältää tiedot kunkin toiminnon sekä malli-pyynnöt.

## <a name="use-custom-attributes"></a>Käytä mukautetut määritteet

Useimmat kuluttaja-sovellukset on tallennettava jonkin mukautetun käyttäjäprofiilin tietoja. Voit tehdä tämän yksi tapa on ehkä määrittää mukautetun määritteen B2C alihallintaan. Voit sitten Käsittele määritteen samalla tavalla kuin muiden ominaisuuden Käsittele käyttäjän aluetta. Voit päivittää määritteen, poista määrite, kysely määrite, Lähetä määrite varaaminen ja kirjaudu sisään tunnusten.

Jos haluat määrittää mukautetun määritteen B2C vuokraajan, Hae [B2C mukautetun määritteen viittaus](active-directory-b2c-reference-custom-attr.md).

Voit tarkastella mukautettuja määritteitä, jotka on määritetty B2C vuokraajan käyttämällä `B2CGraphClient`:

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

Näiden funktioiden tulosteen paljastaa tietoja kunkin mukautettu määrite

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

Voit käyttää koko nimeä, kuten `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number`, käyttäjän objektien ominaisuutena.  Uusi ominaisuus ja ominaisuuden arvon .json-tiedoston päivittämistä ja suorita sitten:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

Käyttämällä `B2CGraphClient`, sinulla on palvelusovelluksen, jotka voivat hallita B2C vuokraajan käyttäjien ohjelmallisesti. `B2CGraphClient`käyttää omaa sovelluksen käyttäjätiedot Azure AD-kaavio-ohjelmointirajapinnan tarkistamiseen. Se myös hankkii tunnusten asiakkaan salaisuus avulla. Kun tämä toiminto yhdistää sovelluksen, muista muutaman avainkohtia B2C sovellusten:

- Haluat myöntää sovelluksen vuokraajan tarvittavat käyttöoikeudet.
- Nyt tarvitset access tunnusten ADAL v2 avulla. (Voit myös lähettää protokolla viestejä suoraan käyttämättä kirjaston.)
- Kun soitat Graph-Ohjelmointirajapinta, käytä `api-version=1.6`.
- Kun olet luonut ja Päivitä kuluttaja-käyttäjät, joitakin ominaisuuksia ovat tarvittaessa yllä olevien ohjeiden mukaisesti.

Jos sinulla on kysymyksiä tai tarjouspyyntöjen toiminnot, jotka haluat suorittaa käyttämällä B2C vuokraajan Graph-Ohjelmointirajapinta, jätä kommentin artikkelista tai tiedoston ongelma GitHub koodin otoksen säilössä.
