Tässä osassa Päivitä koodi aiemmin mobiilisovellukset Taustajärjestelmä projektin push ilmoituksen lähettäminen aina, kun uusi kohde on lisätty. Tämä on tarjoaa ilmoituksen keskittimet [mallin](../articles/notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) ominaisuus, ottaminen käyttöön Office kaikissa ympäristöissä työntää. Eri asiakkaat ovat rekisteröityjen mallien avulla push-ilmoitukset ja kaikki asiakasympäristöjen siirtyä yhden yleinen push.

Valitse alla olevien ohjeiden, joka vastaa Taustajärjestelmä projektityypin&mdash; [.NET taustatietokannan](#dotnet) tai [Node.js Taustajärjestelmä](#nodejs).

### <a name="dotnet"></a>.NET Taustajärjestelmä projektin
1. Visual Studiossa, server project hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**, Etsi `Microsoft.Azure.NotificationHubs`, valitse **Asenna**. Tämä toiminto asentaa ilmoitusten lähettäminen oman taustasta ilmoituksen keskittimet kirjasto.

3. Palvelimen Projectissa, Avaa **ohjaimet** > **TodoItemController.cs**, Lisää seuraava teksti ja lauseiden käyttämisestä:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
    

2. Lisää seuraava koodi **PostTodoItem** -menetelmä **InsertAsync**puhelu jälkeen:  

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings = 
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();
        
        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
        .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Sending the message so that all template registrations that contain "messageParam"
        // will receive the notifications. This includes APNS, GCM, WNS, and MPNS template registrations.
        Dictionary<string,string> templateParams = new Dictionary<string,string>();
        templateParams["messageParam"] = item.Text + " was added to the list.";

        try
        {
            // Send the push notification and log the results.
            var result = await hub.SendTemplateNotificationAsync(templateParams);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Tämä lähettää mallia ilmoitus, joka sisältää kohteen. Kun uusi kohde on lisätty teksti.

4. Jos haluat julkaista server project. 

### <a name="nodejs"></a>Node.js Taustajärjestelmä projektin

1. Jos sinulla ei ole vielä lisätty [Lataa pikaopas Taustajärjestelmä projektin](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) tai muuten [online editorin Azure-portaalissa](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Korvaa aiemmin luotu koodi todoitem.js seuraavasti:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');
    
        var table = azureMobileApps.table();
        
        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK, 
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');
        
        // Define the template payload.
        var payload = '{"messageParam": "' + context.item.text + '" }';  
        
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a template notification.
                    context.push.send(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;  

    Tämä lähettää malli-ilmoituksen, joka sisältää item.text, kun uusi kohde on lisätty.

2. Kun muokkaat tiedostoa paikalliseen tietokoneeseen, julkaista server project.
