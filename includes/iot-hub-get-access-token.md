## <a name="obtain-an-azure-resource-manager-token"></a>Henkilöstöpäällikkö Azure-tunnuksen hankkiminen

Azure Active Directory on todennettava resursseja Azure resurssien hallinnan avulla suoritettavat tehtävät. Tässä esimerkissä käytetään salasanan todennus muut lähestymistavat [pyyntöjen todennuksen Azure resurssinhallinta]on[lnk-authenticate-arm].

1. Lisää seuraava koodi Program.cs hakemaan tunnusta tunnusta ja salasanaa käyttäen Azure AD- **Main** -menetelmäksi.

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. Luo **ResourceManagementClient** -objekti, joka käyttää tunnuksen lisäämällä seuraavan koodin loppuun **Main** -menetelmää:

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. Luoda tai saada viittaus resurssiryhmä, jota käytät:

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx