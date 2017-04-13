<properties 
    pageTitle=".NET SDK käyttäminen Azure Mobile välitys Service API" 
    description="Kerrotaan, miten voit käyttää Mobile välitys .NET SDK Azure Mobile välitys Service API"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>.NET SDK käyttäminen Azure Mobile välitys Service API

Azure Mobile välitys paljastaa ohjelmointirajapinnan voit hallita laitteita, joukko Reach/Push Kampanjat jne. Voit käsitellä nämä API myös annamme voit [Swagger tiedoston](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , voit käyttää työkaluja haluat luoda SDK: T ensisijainen kieli. On suositeltavaa luoda oman SDK Swagger Microsoftin tiedostosta [AutoRest](https://github.com/Azure/AutoRest) -työkalun avulla. 

On luonut .NET SDK samalla tavalla, jonka avulla voit käsitellä nämä ohjelmointirajapinnan käyttäminen C#-paketti, ja sinun ei tarvitse tehdä todennusta suojaustunnuksen neuvottelun ja Päivitä itse.  

Tässä esimerkissä käy läpi vaiheet käyttämällä .NET SDK määrittäminen:

1. Kaikkien, sinun on ensin määrittäminen oman ohjelmointirajapinnan käyttäen Azure Active Directory kuvattu [seuraavassa](mobile-engagement-api-authentication.md#authentication)todennus. Nämä vaiheet lopussa on kelvollinen **SubscriptionId**, **TenantId**, **ApplicationId** ja **salaisuus**. 

2. Käytämme yksinkertainen Windows Console-sovelluksen osoittamaan .NET SDK ja luoda ilmoituksen markkinointikampanjan skenaario käsitteleminen. Avaa Visual Studio ja **Console-sovelluksen**luominen.   

3. Seuraavaksi sinun täytyy ladata .NET-SDK, joka on käytettävissä **Microsoft Azure välitys kirjaston** Nuget valikoiman [tähän](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Jos asennat Nuget Visual Studiossa, sinun on varmistaa, että sinulla on merkitty tehtäessä hakuja paketille **Sisällytä prerelease** -vaihtoehdon valinta:

    ![][1]

4. Valitse `Program.cs` tiedoston, Lisää seuraavat nimitilan:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Seuraavaksi sinun on määritettävä seuraavat vakiot, Käytämme todennus-ja Mobile välitys-sovelluksessa, jossa luot ilmoitus markkinointikampanjan käyttäminen:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Määritä `EngagementManagementClient` muuttuja, joka Käytämme Soita Mobile välitys SDK tavoista:

        static EngagementManagementClient engagementClient; 

7. Lisää seuraavat oman `Main` menetelmää:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Määritä seuraavat menetelmä, jota kestää varoen alustaminen `EngagementManagementClient` ensin todennustapa ja liittämisestä itse Mobile välitys-sovelluksessa, jonka aiot luoda ilmoituksen markkinointikampanjan:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Huomaa, että sinun on käytettävä **Sovelluksen resurssinimi** AppName parametrin Azure hallinta-portaalin määritysten mukaisesti. 

9. Määritä lopuksi CreateCampaign menetelmä, jota käytetään aiemmin valmisteltu EngagementClient luominen Yksinkertainen **milloin tahansa**huolehtia & **vain ilmoituksen** otsikko ja viestin markkinointikampanjan: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Suorita console-sovellus ja näet, markkinointikampanja luomisen onnistumisen seuraavasti:

    **Markkinointikampanja-tunnuksen '159' on luotu ja se on "luonnostilassa**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
