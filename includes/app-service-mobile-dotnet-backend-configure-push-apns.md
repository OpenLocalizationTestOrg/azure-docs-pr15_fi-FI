
+ **.NET Taustajärjestelmä (C#)**:    
    1. Visual Studiossa, server project hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**, Etsi `Microsoft.Azure.NotificationHubs`, valitse **Asenna**. Tämä toiminto asentaa ilmoitusten lähettäminen oman taustasta ilmoituksen keskittimet kirjasto.

    2. Avaa taustatietokannan Visual Studio projektin **ohjaimet** > **TodoItemController.cs**. Tiedoston yläosassa Lisää seuraava `using` lause:

            using Microsoft.Azure.Mobile.Server.Config;
            using Microsoft.Azure.NotificationHubs;


    3. Korvaa `PostTodoItem` menetelmä, jolla on seuraava koodi:  
      
            public async Task<IHttpActionResult> PostTodoItem(TodoItem item)
            {
                TodoItem current = await InsertAsync(item);
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
    
                // iOS payload
                var appleNotificationPayload = "{\"aps\":{\"alert\":\"" + item.Text + "\"}}";
    
                try
                {
                    // Send the push notification and log the results.
                    var result = await hub.SendAppleNativeNotificationAsync(appleNotificationPayload);
    
                    // Write the success result to the logs.
                    config.Services.GetTraceWriter().Info(result.State.ToString());
                }
                catch (System.Exception ex)
                {
                    // Write the failure result to the logs.
                    config.Services.GetTraceWriter()
                        .Error(ex.Message, null, "Push.SendAsync Error");
                }
                return CreatedAtRoute("Tables", new { id = current.Id }, current);
            }

    4. Jos haluat julkaista server project.

+ **Node.js Taustajärjestelmä** : 
   
    1. Jos sinulla ei ole vielä lisätty [Lataa pikaopas projektin](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) tai muuten [online editorin Azure-portaalissa](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor). 
    
    2. Korvaa todoitem.js taulukon komentosarja seuraava koodi:


            var azureMobileApps = require('azure-mobile-apps'),
                promises = require('azure-mobile-apps/src/utilities/promises'),
                logger = require('azure-mobile-apps/src/logger');
            
            var table = azureMobileApps.table();
            
            // When adding record, send a push notification via APNS
            table.insert(function (context) {
                // For details of the Notification Hubs JavaScript SDK, 
                // see http://aka.ms/nodejshubs
                logger.info('Running TodoItem.insert');
                
                // Create a payload that contains the new item Text.
                var payload = "{\"aps\":{\"alert\":\"" + context.item.text + "\"}}";
                
                // Execute the insert; Push as a post-execute action when results are returned as a Promise.
                return context.execute()
                    .then(function (results) {
                        // Only do the push if configured
                        if (context.push) {
                            context.push.apns.send(null, payload, function (error) {
                                if (error) {
                                    logger.error('Error while sending push notification: ', error);
                                } else {
                                    logger.info('Push notification sent successfully!');
                                }
                            });
                        }
                        return results;
                    })
                    .catch(function (error) {
                        logger.error('Error while running context.execute: ', error);
                    });
            });
            
            module.exports = table;

    2. Kun muokkaat tiedostoa paikallisessa tietokoneessa, julkaista server project.
