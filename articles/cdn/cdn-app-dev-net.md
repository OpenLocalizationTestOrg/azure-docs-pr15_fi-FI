<properties
    pageTitle="Aloittaminen Azure CDN kirjaston .NET | Microsoft Azure"
    description="Opettele kirjoittamaan .NET-sovellukset voivat hallita Azure CDN Visual Studiossa."
    services="cdn"
    documentationCenter=".net"
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

[.NET Azure CDN kirjastossa](https://msdn.microsoft.com/library/mt657769.aspx) voit automatisoida luomisen ja hallinnan CDN-profiileista ja päätepisteet.  Tässä opetusohjelmassa käydään läpi yksinkertainen, joka esittelee useita käytettävissä olevat toiminnot .NET console-sovelluksen luominen.  Tässä opetusohjelmassa ei ole tarkoitettu kuvaamaan .NET ‑sivun Azure CDN kirjaston yksityiskohtaiset tiedot.

Tarvitset Visual Studio 2015 suorittamiseen tässä opetusohjelmassa.  [Visual Studio yhteisön 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) on vapaasti ladattavissa.

> [AZURE.TIP] [Valmis projektin tässä opetusohjelmassa](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) on ladattavissa MSDN-sivuston.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Projektin luominen ja lisääminen Nuget-paketit

Olemme olet luonut resurssiryhmä Microsoftin CDN profiilien ja saavat Microsoftin Azure AD-sovelluksen muokkausoikeudet CDN-profiileista ja päätepisteet kunkin ryhmän, emme Aloita tämän sovelluksen luominen.

Kun olet Visual Studio 2015, napsauta uusi projekti-valintaikkunan avaaminen **Tiedosto**- **Uusi**- **projektia** .  Laajenna **Visual C#**ja valitse sitten **Windows** vasemmanpuoleisessa ruudussa.  Valitse keskimmäisessä ruudussa **Console-sovellus** .  Projektin nimi ja valitse sitten **OK**.  

![Uusi projekti](./media/cdn-app-dev-net/cdn-new-project.png)

Projektin suorittaminen käyttää joidenkin Azure kirjastojen sisältämät Nuget paketit.  Lisätään ne projektiin.

1. Valitse **Työkalut** -, **Nuget pakettien hallinta**ja valitse **Paketti hallinta-konsolin**.

    ![Nuget pakettien hallinta](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. Pakettien hallinta-konsolissa Suorita seuraava komento Asenna **Active Directory käyttöoikeuksien kirjaston (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Suorita asentaa **Azure CDN kirjaston**seuraavasti:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Direktiivejä, vakioita tai ensisijainen tapa aputoiminto menetelmät

Aloitetaan kehittämisohjelmaan kirjoitettu perusrakenteen.

1. Takaisin sisään Program.cs-välilehden korvaa `using` direktiivejä yläosassa seuraava:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Määrittää joitakin Microsoftin tavoista käyttää vakioita annettava.  Valitse `Program` luokan, mutta ennen kuin `Main` -menetelmä lisää seuraava teksti.  Varmista, että Korvaa paikkamerkit, mukaan lukien ** &lt;kulmasulkeiden&gt;**, oman arvoilla tarpeen mukaan.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Myös luokan tasolla, voit määrittää nämä kaksi muuttujat.  Olemme määrittämiseen tulee käyttää näitä myöhemmin sekä profiilin ja päätepisteen jo ole.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Korvaa `Main` menetelmä seuraavasti:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Jotkin sekä muita menetelmiä sujuvat käyttäjää "Kyllä/ei-kysymyksissä.  Lisää helpottaa, hieman seuraavasti:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Nyt kun kehittämisohjelmaan perusrakenteen on kirjoitettu, olisi luodaan kutsuu menetelmiä `Main` menetelmää.

## <a name="authentication"></a>Todennus

Emme voi käyttää Azure CDN hallinta-kirjasto, haluamme todennetaan meidän service lyhennys ja hanki todennus-tunnuksen.  Tässä menetelmässä ADAL tunnuksen hakemiseen.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Jos käytössäsi on yksittäisen käyttäjän käyttöoikeuden tarkistaminen, `GetAccessToken` tapa näyttää hieman eri tavalla.

>[AZURE.IMPORTANT] Käytä koodi tässä esimerkissä vain, jos valitset yksittäisen käyttäjän käyttöoikeuden sen sijaan, että tärkeimmät palvelu on.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Varmista, että korvaa `<redirect URI>` uudelleenohjaus URI kirjoittamasi asentaessasi Azure AD sovelluksen kanssa.

## <a name="list-cdn-profiles-and-endpoints"></a>Luettelon CDN-profiileista ja päätepisteet

Nyt sinut voidaan suorittaa CDN nyt.  Tämän menetelmän onko ensiksi on profiilit ja tutustu resurssiryhmä päätepisteet ja jos määritetty Microsoftin vakiot profiilin ja päätepisteen nimien vastine löytyy tekee, paina myöhempää käyttöä varten, jotta ei yrittää luoda kaksoiskappaleet.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Luo CDN-profiileista ja päätepisteet

Olemme seuraavaksi Luo profiili.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Kun profiili on luotu, emme Luo päätepisteen.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] Yllä olevassa esimerkissä määrittää päätepisteen niminen ja *Contoso* -isäntänimi kanssa origin `www.contoso.com`.  Sinun on muutettava tämä osoittamaan oman origin isäntänimi.

## <a name="purge-an-endpoint"></a>Päätepisteen poistaminen

Jos päätepiste on luotu, yhteisen tehtävän, että haluat ehkä suorittaa kehittämisohjelmaan on poistetaan Microsoftin päätepisteen sisältö.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] Merkkijonon yllä olevassa esimerkissä `/*` ilmaisee, että haluan poistaa kaikki pääkansiossa päätepisteen polku.  Tämä on sama kuin tarkistuksen **Tyhjennä kaikki** Azure portal "Poista"-valintaikkunassa. Valitse `CreateCdnProfile` menetelmä luotu Microsoftin profiilin kuin käyttämällä annettua **Azure CDN-Verizon** -profiilin `Sku = new Sku(SkuName.StandardVerizon)`, joten tämä onnistuu.  Kuitenkin **Azure CDN Akamai-** profiileista eivät tue **Tyhjennä kaikki**, joten Akamai profiilin käyttäminen tässä opetusohjelmassa on, jos minulla on lisättävä tiettyjä polkuja poisto.

## <a name="delete-cdn-profiles-and-endpoints"></a>Poista CDN-profiileista ja päätepisteet

Viimeinen tavoista poistaa sekä päätepisteen ja profiilin.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Ohjelman

Syy Käännä ja suorita ohjelma Visual Studio **Käynnistä** -painiketta.

![Suoritettava ohjelma](./media/cdn-app-dev-net/cdn-program-running-1.png)

Kun ohjelma yllä kehotteen, on oltava palauttaa Azure-portaalissa resurssiryhmän ja nähdä, että profiili on luotu.

![Success!](./media/cdn-app-dev-net/cdn-success.png)

Olemme Vahvista sitten Suorita ohjelma loput näyttöön.

![Ohjelma viimeisteleminen](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Voit tarkastella valmiin projektin tätä vaiheittaista [Lataa malli](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Etsi lisäohjeita Azure CDN hallinta kirjaston .NET [viittaus MSDN-sivuston](https://msdn.microsoft.com/library/mt657769.aspx)tarkastelu

Hallitse CDN resurssien [PowerShellin](./cdn-manage-powershell.md)avulla.


