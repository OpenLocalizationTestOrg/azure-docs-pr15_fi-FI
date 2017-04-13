<properties
    pageTitle="Aloittaminen Azure CDN SDK Node.js | Microsoft Azure"
    description="Opettele kirjoittamaan Node.js sovellusten hallinta Azure CDN."
    services="cdn"
    documentationCenter="nodejs"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Azure CDN kehittäminen käytön aloittaminen

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

Voit automatisoida luomisen ja hallinnan CDN-profiileista ja päätepisteet [Node.js Azure CDN SDK-paketissa](https://www.npmjs.com/package/azure-arm-cdn) .  Tässä opetusohjelmassa käydään läpi yksinkertainen, joka esittelee useita käytettävissä olevat toiminnot Node.js console-sovelluksen luominen.  Tässä opetusohjelmassa ei ole tarkoitettu kuvaamaan Node.js kaikkia ominaisuuksia Azure CDN SDK yksityiskohtaiset tiedot.

Viimeistele Tässä opetusohjelmassa, sinun kannattaa jo [Node.js](http://www.nodejs.org) **4.x.x** tai suurempi asentanut ja määrittänyt.  Voit käyttää tahansa tekstieditorilla, kannattaa luoda Node.js-sovellus.  Tässä opetusohjelmassa kirjoittamaan voin käyttää [Visual Studio-koodin](https://code.visualstudio.com).  

> [AZURE.TIP] [Valmis projektin tässä opetusohjelmassa](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) on ladattavissa MSDN-sivuston.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Projektin luominen ja lisääminen NPM riippuvuudet

Olemme olet luonut resurssiryhmä Microsoftin CDN profiilien ja saavat Microsoftin Azure AD-sovelluksen muokkausoikeudet CDN-profiileista ja päätepisteet kunkin ryhmän, emme Aloita tämän sovelluksen luominen.

Voit tallentaa sovelluksen kansion luominen  Konsolin nykyisen polun Node.js-työkaluja määrittää nykyisen sijaintisi uuteen kansioon ja alusta projektin suoritetaan:
    
    npm init
    
Valitse näyttöön tulee alustaa projektiin liittyviä kysymyksiä.  Tässä opetusohjelmassa **aloituskohta**, käyttää *app.js*.  Näet omat vaihtoehtoja seuraavan esimerkin mukaisesti.

![NPM alusta tulostus](./media/cdn-app-dev-node/cdn-npm-init.png)

Tutustu project on nyt alustaa *packages.json* tiedoston.  Projektin suorittaminen käyttää joidenkin Azure kirjastojen sisältämät NPM paketit.  Käytetään Node.js (ms-rest-azure) Azure-Client Runtimen ja Azure CDN asiakkaan kirjaston Node.js (azure-arm-cd).  Lisää oletetaan, että ne projektiin riippuvuudet.
 
    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Kun paketit ovat asennus on valmis, *package.json* tiedostojen pitäisi nyt muistuttaa tässä esimerkissä (versio numerot voivat vaihdella):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Lopuksi teksti-editorin avulla, Luo tyhjä tekstitiedosto ja Tallenna asiakirja nimellä *app.js*Tutustu project-kansion nuolta.  Microsoft on nyt valmis aloittamaan koodi.

## <a name="requires-constants-authentication-and-structure"></a>Vaatii, vakioita, todennus ja rakenteen

*App.js* Avaa Microsoftin editori, jossa aloitetaan kehittämisohjelmaan kirjoitettu perusrakenteen.

1. Lisää "vaatii" Microsoftin NPM pakettien yläosassa seuraavasti:

    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```

2. Määrittää joitakin Microsoftin tavoista käyttää vakioita annettava.  Lisää seuraava.  Varmista, että Korvaa paikkamerkit, mukaan lukien ** &lt;kulmasulkeiden&gt;**, oman arvoilla tarpeen mukaan.

    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";

    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Seuraavaksi on vahvistaa CDN management-asiakas ja anna sille Microsoftin tunnistetiedot.

    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
    
    Jos käytössäsi on yksittäisen käyttäjän käyttöoikeuden, nämä kaksi riviä näyttää erilaiselta.

    >[AZURE.IMPORTANT] Käytä koodi tässä esimerkissä vain, jos valitset yksittäisen käyttäjän käyttöoikeuden sen sijaan, että tärkeimmät palvelu on.  Varmista, ettet suojaa yksittäisen käyttäjän tunnistetiedot ja pitämään ne salainen.

    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```

    Varmista, että korvaa kohteiden ** &lt;kulmasulkeiden&gt; ** oikeat tiedot.  Saat `<redirect URI>`, käytä uudelleenohjaus asentaessasi sovelluksen Azure AD määritetty URI.
    

4.  Tutustu Node.js console-sovelluksen suorittaminen kestää joitakin komentoriviparametreja.  Vahvista seuraavaksi, että vähintään välitetty parametri.

    ```javascript
    //Collect command-line parameters
    var parms = process.argv.slice(2);

    //Do we have parameters?
    if(parms == null || parms.length == 0)
    {
        console.log("Not enough parameters!");
        console.log("Valid commands are list, delete, create, and purge.");
        process.exit(1);
    }
    ```

5. Joka näyttää us pääaluetta kehittämisohjelmaan, jossa on haarautuvan muut Funktiot, mitä parametrit on välitetty perusteella.

    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;

        case "create":
            cdnCreate();
            break;
        
        case "delete":
            cdnDelete();
            break;

        case "purge":
            cdnPurge();
            break;

        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```

6.  Useita paikassa-kehittämisohjelmaan emme on Varmista, että oikea määrä parametrit on välitetty ja näyttää apua, jos ne eivät näy oikein.  Luo, funktioita.

    ```javascript
    function requireParms(parmCount) {
        if(parms.length < parmCount) {
            usageHelp(parms[0].toLowerCase());
            process.exit(1);
        }
    }

    function usageHelp(cmd) {
        console.log("Usage for " + cmd + ":");
        switch(cmd)
        {
            case "list":
                console.log("list profiles");
                console.log("list endpoints <profile name>");
                break;

            case "create":
                console.log("create profile <profile name>");
                console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
                break;
            
            case "delete":
                console.log("delete profile <profile name>");
                console.log("delete endpoint <profile name> <endpoint name>");
                break;

            case "purge":
                console.log("purge <profile name> <endpoint name> <path>");
                break;

            default:
                console.log("Invalid command.");
        }
    }
    ```

7. Microsoft käyttää asiakkaan CDN hallinta Funktiot ovat lopuksi asynkroninen, joten he tarvitsevat menetelmän Soita takaisin, kun ne on valmis.  Tee jokin, joka näyttää tulokset CDN hallinta-asiakasohjelma (jos saatavilla) ja sulje ohjelma tilanteen japanin.

    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Kun kehittämisohjelmaan perusrakenteen kirjoitetaan, on kannattaa luoda kutsuttuja Microsoftin parametrien perusteella funktioita.

## <a name="list-cdn-profiles-and-endpoints"></a>Luettelon CDN-profiileista ja päätepisteet

Aloitetaan koodi luettelo sekä aiemmin-profiileista ja päätepisteet.  Koodi-kommentit antaa odotettu syntaksi, jolla on tietää, johon kunkin parametrin sijoitetaan.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Luo CDN-profiileista ja päätepisteet

Seuraavaksi on kirjoittaa funktioita profiilit ja päätepisteet luomiseen.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Päätepisteen poistaminen

Jos päätepiste on luotu, yhteisen tehtävän, että haluat ehkä suorittaa kehittämisohjelmaan on poistetaan Microsoftin päätepisteen sisällön.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>Poista CDN-profiileista ja päätepisteet

Olemme sisällytetään viimeisen funktio poistaa päätepisteet ja profiilit.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Ohjelman

Emme voi suorittaa nyt Node.js kehittämisohjelmaan käyttämällä Microsoftin tuttuja virheenkorjaus tai konsolissa.

> [AZURE.TIP] Jos käytät Visual Studio-koodin lisääminen virheenkorjaus, tarvitset ympäristön määritys välittää komentoriviparametreja.  Visual Studio koodi tekee **lanuch.json** -tiedosto.  Ominaisuutta nimeltä **argumentit** Etsi ja lisää parametrit-merkkijonoarvoa matriisin niin, että se näyttää samalta tähän: `"args": ["list", "profiles"]`.

Aloitetaan tekemällä luettelo Microsoftin profiilit.

![Luettelo-profiileista](./media/cdn-app-dev-node/cdn-list-profiles.png)

Olemme käytössä palauttaa tyhjän taulukon.  Vaikka emme ole yhtään profiilia Microsoftin resurssiryhmä, joka odotetaan.  Luodaan nyt profiili.

![Profiilin luominen](./media/cdn-app-dev-node/cdn-create-profile.png)

Nyt lisääminen päätepisteen.

![Päätepisteen luominen](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Oletetaan, että poistaa lopuksi Microsoftin profiilia.

![Profiilin poistaminen](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Voit tarkastella valmiin projektin tätä vaiheittaista [Lataa malli](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Jos haluat tarkastella viittaus Azure CDN SDK Node.js varten, tarkastele [viittaus](http://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/).

Etsi lisäohjeita Azure SDK: Node.js, tarkastele [koko viittaus](http://azure.github.io/azure-sdk-for-node/).

Hallitse CDN resurssien [PowerShellin](./cdn-manage-powershell.md)avulla.

