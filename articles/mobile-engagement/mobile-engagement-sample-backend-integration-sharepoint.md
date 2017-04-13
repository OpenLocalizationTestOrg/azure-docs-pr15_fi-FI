<properties 
    pageTitle="Azure Mobile välitys - taustatietokannan integrointi" 
    description="Azure Mobile välitys yhteyttä SharePoint-taustatietokannan Kampanjat luominen SharePoint" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

# <a name="azure-mobile-engagement---api-integration"></a>Azure Mobile välitys - Ohjelmointirajapinnan integrointi

Automaattinen markkinoinnin järjestelmän luominen ja aktivoiminen markkinointikampanjoita ilmetä myös automaattisesti. Tähän tarkoitukseen - Azure Mobile välitys avulla luodaan automaattinen markkinointikampanjoita, kuten ohjelmointirajapinnan käyttäminen sekä. 

Yleensä asiakkaat käyttää Mobile välitys edusta-liittymän luomiseen ilmoitukset/kyselyjä muille niiden markkinointikampanjan osana. Kuitenkin kun markkinointikampanjoita kehittynyt, on tarpeen hyödyntää tiedot lukittu Taustajärjestelmä järjestelmien (kuten CRM-järjestelmän tai CMS-järjestelmään, kuten SharePoint), niin, että automaattista myyntijakso voidaan luoda joka luo Kampanjat Mobile välitys dynaamisesti Taustajärjestelmä järjestelmistä juoksutus tietojen perusteella. 

![][5]

Tässä opetusohjelmassa käy läpi skenaarion mihin SharePoint-yrityskäyttäjä täyttää markkinoinnin tietoja SharePoint-luetteloon ja automatisoidun prosessin vastataan luettelon kohteita ja yhdistää Mobile välitys järjestelmän käyttämällä käytettävissä REST API markkinointikampanjan luominen SharePoint-tietoja. 
 
> [AZURE.IMPORTANT] Yleinen voit käyttää tässä esimerkissä lähtökohtana ymmärtämään, kuinka kutsu Mobile-välitys REST-Ohjelmointirajapinta, koska se tiedot kaksi avaimen näkökohdat kutsumista API - todentamalla ja kulkeva parametrit. 

## <a name="sharepoint-integration"></a>SharePoint-integrointi
1. Tässä on esimerkki SharePoint-luettelo näyttää tältä. Ilmoituksen luominen käytetään **otsikko**, **luokka**, **NotificationTitle**, **viestin** ja **URL-osoite** . On sarake nimeltä **IsProcessed** koon muuttamiseen tarkoitettu console-ohjelman lomakkeeseen otoksen automaatio-prosessin. Voit joko Suorita tämä konsoli ohjelman Azure-WebJob niin, että voit ajoittaa tai voi käyttää suoraan SharePoint-työnkulun luominen ja aktivoiminen ilmoituksen, kun kohde on lisätty SharePoint-luettelon-ohjelmaan. Tässä esimerkissä Käytämme console-ohjelman, joka menee läpi SharePoint-luettelon kohteita ja ilmoituksen luominen Azure Mobile välitys kullekin niistä ja merkitsee lopuksi **IsProcessed** merkintä täytyttävä onnistuneen ilmoituksen luominen.

    ![][1]

2. Esimerkissä on käytössä *Remote todennus SharePointin verkossa käyttämällä asiakkaan objektimalli* otosten koodin [tähän](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) todentamismenetelmä SharePoint-luetteloon.
 
3. Kun todennettu, emme käydään läpi luettelokohteet kieliominaisuuksien juuri luomasi kohteet (joka on **IsProcessed** EPÄTOSI). 

        static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);

                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();

                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;

                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;

                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";

                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a>Mobile välitys-integrointi
1.  Kun olemme Etsi kohde, joka edellyttää käsittely - olemme Pura luettelokohteen ja puhelun ilmoituksen luominen tarvittavat tiedot `CreateAzMECampaign` luomiseen ja sen jälkeen `ActivateAzMECampaign` Aktivoi. Nämä ovat olennaisesti REST API puhelut soittavan Mobile välitys Taustajärjestelmä. 

2.  Mobile välitys REST API edellyttävät **Basic auth värimallin luvan HTTP-otsikon** joka koostuu `ApplicationId` ja `ApiKey` sinulla on Azure-portaalista. Varmista, että käytät avain **api näppäimet** -osasta ja *ei* **sdk näppäimet** -osiosta. 

    ![][2]

        static string CreateAuthZHeader()
        {
            string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
            return BasicAuthzHeaderString;
        }

        static public string EncodeTo64(string toEncode)
        {
            byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
            string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
            return returnValue;
        }  

3. Ilmoituksen luomisen type markkinointikampanja - [ohjeista](https://msdn.microsoft.com/library/azure/mt683750.aspx). Varmista, että määrität markkinointikampanja joudut `kind` *ilmoitus* ja [paketti](https://msdn.microsoft.com/library/azure/mt683751.aspx) ja kulkeva FormUrlEncodedContent nimellä. 

        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";

            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
                
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
                
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });

                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }

4. Kun olet luonut ilmoituksen, näet seuraavanlaiselta Mobile välitys-portaalissa (Huomaa, että tila = luonnos ja aktivoitu = puuttuu)

    ![][3]

5. `CreateAzMECampaign`Luo ilmoitus markkinointikampanjan ja palauttaa sen tunnuksen soittajalle. `ActivateAzMECampaign`edellyttää tätä tunnusta parametrina aktivoimaan markkinointikampanja. 

        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());

                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });

                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }

6. Kun olet aktivoinut ilmoitus, näet seuraavanlaiselta Mobile välitys-portaalissa:

    ![][4]

7. Kun markkinointikampanja saa aktivoitu, laitteita, jotka täyttävät ehdon-markkinointikampanja alkaa näe ilmoitukset. 

8. Huomaa myös, että luettelokohteen merkitty IsProcessed = false on määritetty tosi ilmoitus markkinointikampanjan luomisen jälkeen.  

Tässä esimerkissä luodaan yksinkertainen ilmoitus-markkinointikampanjan määrittäminen enimmäkseen tarvittavat ominaisuudet. Voit mukauttaa mahdollisimman paljon edellä mainittujen tietojen avulla voit-portaalista [tähän](https://msdn.microsoft.com/library/azure/mt683751.aspx). 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png


 
