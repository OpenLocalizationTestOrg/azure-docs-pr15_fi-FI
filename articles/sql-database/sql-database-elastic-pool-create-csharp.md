<properties
    pageTitle="Luo joustavasti tietokannan resurssivarantoon C# | Microsoft Azure"
    description="C# tietokannan kehittäminen tekniikoita avulla voit luoda skaalattava joustavasti tietokannan varannon Azure SQL-tietokantaan, jotta voit jakaa resursseja useita tietokantoja."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="create-an-elastic-database-pool-with-cx23"></a>Luo joustavasti tietokannan resurssivarantoon C & #x 23.

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-pool-create-portal.md)
- [PowerShellin](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Tässä artikkelissa kerrotaan, miten käyttää C# luomiseen Azure SQL-tietokannan joustavasti resurssivarantoon [Azure SQL-tietokannan kirjasto .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Erillinen SQL-tietokannan luominen-kohdassa [Käytä C# .NET SQL-tietokannan kirjaston kanssa SQL-tietokannan luominen](sql-database-get-started-csharp.md).

.NET Azure SQL tietokanta-kirjasto sisältää [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md)-Ohjelmointirajapinta, joka rivittyy [Resurssienhallinta-pohjainen SQL tietokanta REST API](https://msdn.microsoft.com/library/azure/mt163571.aspx)perusteella.

>[AZURE.NOTE] SQL-tietokannan monia uusia ominaisuuksia tuetaan vain, kun käytät [Azure resurssien hallinnan käyttöönottomalli](../azure-resource-manager/resource-group-overview.md), joten käytä aina uusimmat **Azure SQL tietokanta hallinnan kirjasto .NET ([docs](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [NuGet Package](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Vanhat [perinteinen käyttöönoton mallin pohjainen kirjastoihin](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) tuetaan vain yhteensopivuus niin Suosittelemme käyttämään uudempaan mukaan Resurssienhallinta-kirjastot.

Tämän artikkelin seuraavien vaiheiden suorittamiseen tarvitset seuraavat:

- Azure tilaus. Jos tarvitset Azure tilauksen yksinkertaisesti **ILMAISEN tilin** tämän sivun yläosassa ja valitse palaa valmis tässä artikkelissa.
- Visual Studio. Katso Visual Studio maksutta, [Visual Studio lataukset](https://www.visualstudio.com/downloads/download-visual-studio-vs) -sivu.


## <a name="create-a-console-app-and-install-the-required-libraries"></a>Konsolin sovelluksen luominen ja asenna tarvittavat kirjastot

1. Käynnistä Visual Studio.
2. Valitse **Tiedosto** > **uuden** > **projektin**.
3. C#- **Konsolisovelluksen** luominen ja anna sille nimi: *SqlElasticPoolConsoleApp*


SQL-tietokannan luominen ja C#, ladata tarvittavat hallinta-kirjastoja, (joko [paketin hallinta-konsolin](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Valitse **Työkalut** > **NuGet pakettien hallinta** > **Paketin hallinta-konsolin**.
2. Kirjoita `Install-Package Microsoft.Azure.Management.Sql –Pre` asentaa [Microsoft Azure SQL kirjaston](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Kirjoita `Install-Package Microsoft.Azure.Management.ResourceManager –Pre` asentaa [Microsoft Azure Resurssienhallinta kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Kirjoita `Install-Package Microsoft.Azure.Common.Authentication –Pre` asentaa [Microsoft Azure yleisiä todennus kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] Tämän artikkelin esimerkkejä lomakkeella API sivupyynnön painikkeen ja estää ennen kuin muiden päättymisen kutsuvat pohjana-palvelusta. Käytettävissä on asynkroninen tapoja.


## <a name="create-a-sql-elastic-database-pool---c-example"></a>Luo SQL joustavasti tietokannan resurssivarantoon - C#-Esimerkki

Seuraava esimerkki luo resurssiryhmä, server, palomuurin ja joustavasti resurssivarantoon, ja sitten SQL-tietokanta-ryhmän. Näkyvissä, saat [Luo pääasiallista resursseihin palvelu](#create-a-service-principal-to-access-resources) `_subscriptionId, _tenantId, _applicationId, and _applicationSecret` muuttujat.

Korvaa **Program.cs** sisällön seuraavaan ja Päivitä `{variables}` sovelluksen arvoilla (Älä sisällytä `{}`).


```
using Microsoft.Azure;
using Microsoft.Azure.Management.ResourceManager;
using Microsoft.Azure.Management.ResourceManager.Models;
using Microsoft.Azure.Management.Sql;
using Microsoft.Azure.Management.Sql.Models;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace SqlElasticPoolConsoleApp
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

        static string _poolName = "{pool-name}";
        static string _poolEdition = "{Standard}";
        static int _poolDtus = {100};
        static int _databaseMinDtus = {10};
        static int _databaseMaxDtus = {100};

        static string _databaseName = "{elasticdbfromcsarticle}";



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

            Console.WriteLine("Elastic pool...");
            ElasticPoolCreateOrUpdateResponse epr = CreateOrUpdateElasticDatabasePool(_sqlMgmtClient, _resourceGroupName, _serverName, _poolName, _poolEdition, _poolDtus, _databaseMinDtus, _databaseMaxDtus);
            Console.WriteLine("Elastic pool: " + epr.ElasticPool.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _poolName);
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



        static ElasticPoolCreateOrUpdateResponse CreateOrUpdateElasticDatabasePool(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string poolName, string poolEdition, int poolDtus, int databaseMinDtus, int databaseMaxDtus)
        {
            // Retrieve the server that will host this elastic pool
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus,
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlMgmtClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, poolName, newPoolParameters);
            return newPoolResponse;
        }




        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string poolName)
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
                    ElasticPoolName = poolName
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
```





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

- [Oman ryhmän hallinta](sql-database-elastic-pool-manage-csharp.md)
- [Luo joustavasti työt](sql-database-elastic-jobs-overview.md): joustavasti työt avulla voit suorittaa T-SQL-komentosarjat resurssivarantoon tietokantojen määrää.
- [Azure SQL-tietokannan laajentaminen](sql-database-elastic-scale-introduction.md): skaalata ulos joustavasti Tietokantatyökalut avulla.

## <a name="additional-resources"></a>Lisäresursseja

- [SQL-tietokantaan](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure resurssien hallinnan API](https://msdn.microsoft.com/library/azure/dn948464.aspx)
