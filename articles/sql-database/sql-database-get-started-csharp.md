<properties
    pageTitle="Kokeile SQL-tietokantaan: Käytä C# SQL-tietokannan luominen | Microsoft Azure"
    description="Kokeile SQL-tietokannan SQL- ja C#-sovellusten kehittämiseen ja luo Azure SQL-tietokantaan ja C#-SQL-tietokanta-kirjaston käytön .NET."
    keywords="Kokeile sql-sql-c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="10/04/2016"
   ms.author="sstein"/>

# <a name="try-sql-database-use-c-to-create-a-sql-database-with-the-sql-database-library-for-net"></a>Kokeile SQL-tietokantaan: Käytä C# SQL-tietokannan luominen SQL-tietokanta-kirjastoa varten .NET


> [AZURE.SELECTOR]
- [Azure portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShellin](sql-database-get-started-powershell.md)

Opi käyttämään C# Azure SQL-tietokannan luominen [Microsoft Azure SQL hallinnan kirjasto .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Tässä artikkelissa kerrotaan, miten voit luoda yhden tietokannan SQL-ja C#. Joustavasti tietokannan jakavat-kohdassa [joustavasti tietokannan resurssivarannon luominen](sql-database-elastic-pool-create-csharp.md).

.NET Azure SQL tietokanta hallinnan kirjasto sisältää [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md)-Ohjelmointirajapinta, joka rivittyy [Resurssienhallinta-pohjainen SQL tietokanta REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx)perusteella.

>[AZURE.NOTE] SQL-tietokannan monia uusia ominaisuuksia tuetaan vain, kun käytät [Azure resurssien hallinnan käyttöönottomalli](../azure-resource-manager/resource-group-overview.md), joten käytä aina uusimmat **Azure SQL tietokanta hallinnan kirjasto .NET ([docs](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet Package](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Vanhat [perinteinen käyttöönoton mallin pohjainen kirjastoihin](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) tuetaan vain yhteensopivuus niin Suosittelemme käyttämään uudempaan mukaan Resurssienhallinta-kirjastot.

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

- Azure tilaus. Jos tarvitset Azure tilauksen yksinkertaisesti **ILMAISEN tilin** tämän sivun yläosassa ja valitse palaa valmis tässä artikkelissa.
- Visual Studio. Katso Visual Studio maksutta, [Visual Studio lataukset](https://www.visualstudio.com/downloads/download-visual-studio-vs) -sivu.

>[AZURE.NOTE] Tässä artikkelissa Luo uusi, tyhjä SQL-tietokantaan. Muokkaa *CreateOrUpdateDatabase(...)* menetelmä seuraavassa esimerkissä kopioida tietokantoja, skaalata tietokantoja, tietokannan luominen resurssivarantoon, jne. Lisätietoja on artikkelissa [DatabaseCreateMode](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreatemode.aspx) ja [DatabaseProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databaseproperties.aspx) luokkia.



## <a name="create-a-console-app-and-install-the-required-libraries"></a>Konsolin sovelluksen luominen ja asenna tarvittavat kirjastot

1. Käynnistä Visual Studio.
2. Valitse **Tiedosto** > **uuden** > **projektin**.
3. C#- **Konsolisovelluksen** luominen ja anna sille nimi: *SqlDbConsoleApp*


SQL-tietokannan luominen ja C#, ladata tarvittavat hallinta-kirjastoja, (joko [paketin hallinta-konsolin](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Valitse **Työkalut** > **NuGet pakettien hallinta** > **Paketin hallinta-konsolin**.
2. Kirjoita `Install-Package Microsoft.Azure.Management.Sql –Pre` asenna uusin [Microsoft Azure SQL kirjaston](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Kirjoita `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` asentaa [Microsoft Azure Resurssienhallinta kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Kirjoita `Install-Package Microsoft.Azure.Common.Authentication –Pre` asentaa [Microsoft Azure yleisiä todennus kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Tämän artikkelin esimerkkejä lomakkeella API sivupyynnön painikkeen ja estää ennen kuin muiden päättymisen kutsuvat pohjana-palvelusta. Käytettävissä on asynkroninen tapoja.


## <a name="create-a-sql-database-server-firewall-rule-and-sql-database---c-example"></a>SQL-tietokanta, palomuurin ja SQL-tietokanta - C# Esimerkki luominen

Seuraava esimerkki luo resurssiryhmä, palvelimen, palomuurin ja SQL-tietokantaan. Näkyvissä, saat [Luo pääasiallista resursseihin palvelu](#create-a-service-principal-to-access-resources) `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` muuttujat.

Korvaa **Program.cs** sisällön seuraavaan ja Päivitä `{variables}` sovelluksen arvoilla (Älä sisällytä `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## <a name="create-a-service-principal-to-access-resources"></a>Luo palvelu pääasiallista Accessin resurssit

Seuraavaa PowerShell-komentosarjaa luo Active Directory (AD)-sovelluksen ja palvelun lyhennys, joka todentaa Microsoftin C#-sovelluksen annettava. Komentosarja tulostaa edellisen C#-otoksen annettava arvot. Lisätietoja on kohdassa [Azure PowerShellin Luo pääasiallista resursseihin palvelu](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet yrittänyt SQL-tietokantaan ja määrittää tietokannan ja C#, olet valmis on seuraavissa artikkeleissa:

- [Yhteyden muodostaminen SQL-tietokantaa, jossa SQL Server Management Studiossa ja suorittaa otoksen T-SQL-kysely](sql-database-connect-query-ssms.md)

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokantaan](https://azure.microsoft.com/documentation/services/sql-database/)
- [Tietokanta-luokka](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png
