<properties
   pageTitle="Hae tarvittavat arvot todennustapa koodista SQL-tietokantaan sovellus | Microsoft Azure"
   description="Luo palvelun lyhennys SQL-tietokannan avaaminen koodi."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>Hae tarvittavat arvot todennustapa sovelluksen voit käyttää SQL-tietokanta-koodista

Voit luoda ja hallita SQL-tietokantaan, sinun on rekisteröitävä sovelluksen Azure Active Directory (AAD)-toimialueeseen-tilaus, johon Azure resursseja on luotu koodi.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Luo palvelu pääasiallista access resurssien sovelluksesta

Haluat uusimmat [PowerShellin Azure](https://msdn.microsoft.com/library/mt619274.aspx) asennettu ja käytössä on. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

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




## <a name="see-also"></a>Katso myös

- [SQL-tietokannan luominen ja C#](sql-database-get-started-csharp.md)
- [Yhteyden muodostaminen SQL-tietokantaan Azure Active Directory käyttöoikeuksien avulla](sql-database-aad-authentication.md)


