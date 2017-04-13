<properties 
    pageTitle="Web app, jossa taulukkotallennus (Node.js) | Microsoft Azure" 
    description="Opetusohjelma, jota käytetään lisäämällä Azuren tallennustilaan services ja Azure moduulin Express-opetusohjelman Web-sovellukseen." 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robmcm"/>

# <a name="nodejs-web-application-using-storage"></a>Node.js Web-sovelluksen tallennustila

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa ulottuu sovelluksen luotu [Node.js Web-sovelluksen Express] -opetusohjelmassa Node.js Microsoft Azure asiakas-kirjastoissa data management Services-palvelun käyttöä varten. Ulottuu sovelluksen, voit ottaa käyttöön Azure verkkopohjaisia-tehtäväluettelo-sovelluksen luominen. Tehtäväluettelon avulla käyttäjä voi hakea tehtävät, uusien tehtävien lisääminen ja merkitä tehtävän valmiiksi.

Tehtävä-kohteet tallennetaan Azure-tallennustilan. Azuren tallennustila on rakenteeton tietosäilö vikasietoinen ja hyvin käytettävissä oleva. Azure-tallennustilan sisältää useita tietojen rakenteet kohtaa, johon voit tallentaa ja käyttää tietoja ja voidaan hyödyntää sisältyvät Azure SDK Node.js tai kautta REST API ohjelmointirajapinnan tallennustilan palveluista. Lisätietoja on artikkelissa [tallentaminen ja käyttäminen tietojen Azure-tietokannassa].

Tässä opetusohjelmassa oletetaan, että olet suorittanut [Node.js Web-sovelluksen] ja [Node.js Expressin][Node.js Web-sovelluksen käyttäminen Express] Opetusohjelmissa.

Opit seuraavat asiat:

-   Jade malli-ohjelma käsittelemisestä
-   Azure Data Management Services-palveluiden käyttäminen

Näyttökuva valmiin sovelluksen on alla:

![Valmiit verkkosivu internet Explorerissa](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="setting-storage-credentials-in-webconfig"></a>Tallennustilan tunnistetietojen asetus Web.config

Azuren tallennustilaan käyttöön, sinun täytyy välittää tallennustilan tunnistetiedot. Voit tehdä tämän käyttämiseen web.config asetukset.
Nämä asetukset siirretään kuin ympäristömuuttujia solmu, joka luetaan sitten Azure SDK-paketissa.

> [AZURE.NOTE] Tallennustilan tunnistetiedot käytetään vain, kun sovellus otetaan käyttöön Azure. Kun emulaattori, sovellus käyttää tallennustilan emulaattori.

Seuraavien toimien noutaa tallennustilan tilin tunnistetiedot ja niiden lisääminen web.config-asetukset:

1.  Jos sitä ei ole vielä Avaa, Käynnistä PowerShellin Azure **Käynnistä** -valikosta **Kaikki ohjelmat, valitse Azure**laajentamalla, **PowerShellin Azure**hiiren kakkospainikkeella ja valitse sitten **Suorita järjestelmänvalvojana**.

2.  Vaihda kansio, jossa sovellus kansioita. Esimerkiksi C:\\solmu\\tasklist\\WebRole1.

3.  PowerShellin Azure-ikkunasta Lisää tallennustilan tilin tietojen hakemiseen seuraavan cmdlet-komennon:

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    Tämä noutaa tallennustilan tilit ja näppäimet liittyvät isännöityä palvelun tili.

    > [AZURE.NOTE] Koska Azure SDK Luo tallennustilan tilin, kun otat käyttöön palvelu-tallennustilan tilin olisi olemassa-edellisen apuviivat sovelluksen käyttöönotto.

4.  Avaa sisältävä ympäristöasetuksia, joita käytetään, kun sovellus otetaan käyttöön Azure **ServiceDefinition.csdef** tiedosto:

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  Lisää **ympäristön** elementti, korvaaminen {TALLENNUSTILAN tilin} ja {TALLENNUSTILAN PIKANÄPPÄIN} seuraava lohko, jonka tilin nimi ja tallennustilaa tilin perusavaimen, jota haluat käyttää käyttöönottoa varten:

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![Web.cloud.config Tiedostosisältö](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  Tallenna tiedosto ja sulje Muistio.

### <a name="install-additional-modules"></a>Asenna moduuleja

2. Käytä seuraavaa komentoa asentaminen [azure], [solmu-uuid], [nconf] ja [asynkroninen] moduulit sekä paikallisesti että Tallenna ne merkinnän **package.json** -tiedosto:

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    Tämä komento pitäisi näyttää seuraavankaltaiselta:

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##<a name="using-the-table-service-in-a-node-application"></a>Taulukon solmu-sovelluksen avulla

Tässä osassa ulottuu basic sovelluksen **Pika** -komennolla luotujen lisäämällä **task.js** tiedosto, jossa on mallin tehtäviin. Myös muokata aiemmin luotuja **app.js** ja luo uusi **tasklist.js** mallia käyttävä tiedosto.

### <a name="create-the-model"></a>Luo malli

1. **WebRole1** kansioon Luo uusi kansio nimeltä **Mallit**.

2. **Mallit** -kansioon Luo uusi tiedosto nimeltä **task.js**. Tämä tiedosto sisältää sovelluksen luomat tehtävät mallista.

3. **Task.js** tiedoston alussa Lisää tarvittavat kirjastojen viittaamaan seuraava koodi:

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Lisää seuraavaksi määrittäminen ja viedä tehtävän objektin koodi. Tämä objekti on vastuussa yhteyden taulukkoon.

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Lisää seuraava koodi määrittää muita tapoja tehtävän kohdetta, jotka mahdollistavat aktiviteettien taulukon tallennettuja tietoja:

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };

            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. Tallenna ja sulje **task.js** -tiedosto.

### <a name="create-the-controller"></a>Luo ohjain

1. **WebRole1/tiet** kansioon Luo uusi tiedosto nimeltä **tasklist.js** ja avaa se tekstieditorissa.

2. Lisää seuraava koodi **tasklist.js**. Tämä Lataa azure ja asynkroninen moduulit, jotka käyttävät **tasklist.js**. Tämä asetus määrittää myös **TaskList** -funktiota, joka on välitetty määritimme aiemmin **tehtävän** objektin esiintymä:

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. Jatka **tasklist.js** -tiedoston lisääminen lisäämällä **showTasks**, **addTask**ja **completeTasks**menetelmiä:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this      
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }

3. Tallenna tiedosto **tasklist.js** .

### <a name="modify-appjs"></a>Muokkaa app.js

1. **WebRole1** kansioon Avaa tekstieditorissa **app.js** -tiedosto. 

2. Tiedoston alkuun, Lisää seuraava lataamisen azure makrokoodi ja määrittää taulukon nimi ja osion käyttäjäavainten:

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. App.js-tiedostossa Vieritä on näkyvissä seuraava komento:

        app.use('/', routes);
        app.use('/users', users);

    Korvaa yllä rivien alapuolella näkyvää koodia. Tämä alusta <strong>tehtävän</strong> esiintymä yhteyden tallennustilan-tilisi avulla. Tämä on välitetty <strong>TaskList</strong>, joka käyttää sitä viestintä taulukko-palvelun kanssa:

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. Tallenna tiedosto **app.js** .

### <a name="modify-the-index-view"></a>Indeksi-näkymän muokkaaminen

1. Muuta kansioiden **näkymät** -kansio ja Avaa **index.jade** tiedostoa tekstieditorissa.

2. Korvaa **index.jade** -tiedoston sisällön alla olevan koodin. Tämä määrittää näkymän näyttäminen olemassa olevien tehtävien sekä lomakkeen uusien tehtävien lisääminen ja aiemmin luotuja merkitseminen valmiiksi.

        extends layout

        block content
          h1= title
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if tasks != []
                tr
                  td 
              else
                each task in tasks
                  tr
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name: 
            input(name="item[name]", type="textbox")
            label Item Category: 
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. Tallenna ja sulje **index.jade** tiedosto.

### <a name="modify-the-global-layout"></a>Yleisen asettelun

**Näkymien** hakemiston **layout.jade** tiedoston käytetään yleisen mallin **.jade** muita tiedostoja. Tässä vaiheessa muokkaat käyttämään [Twitter-automaattinen](https://github.com/twbs/bootstrap), joka on Työkalut, joka on helppo nice näköisen verkkosivuston suunnitteluun.

1. Lataa ja Pura tiedostot [Twitter-automaattista käynnistystä](http://getbootstrap.com/)varten. Kopioi **bootstrap.min.css** -tiedostosta **käynnistyksen\\jakauma\\css** kansio **julkinen\\tyylisivuja** hakemiston tasklist sovelluksesi.

2. **Näkymät** -kansioon ja Avaa **layout.jade** teksti-editorissa ja korvata sisällön seuraavasti:

        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body.app
            nav.navbar.navbar-default
              div.navbar-header
                a.navbar-brand(href='/') My Tasks
            block content

3. Tallenna tiedosto **layout.jade** .

### <a name="running-the-application-in-the-emulator"></a>Sovelluksen suorittamisen emulaattori

Seuraavalla komennolla Käynnistä sovellus emulaattori.

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

Selain avautuu ja näyttää seuraavan sivun:

![Sivuston sivutettu liittyvä omaan tehtäväluetteloon taulukko, joka sisältää tehtävät ja Lisää uusi tehtävä-kenttien kanssa.](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

Lomakkeen avulla voit lisätä kohteita tai muiden kohteiden poistaminen merkitsemällä valmiiksi.

## <a name="publishing-the-application-to-azure"></a>Azure sovelluksen julkaiseminen


Windows PowerShell-ikkunassa Soita seuraavat cmdlet-komennolla ja ota uudelleen isännöityä Azure-palvelussa.

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

Korvaa **myuniquename** tämän sovelluksen yksilöllinen nimi. Korvaa **datacentername** Azure tietokeskuksen, kuten **Länsi US**nimi.

Kun asennus on valmis, pitäisi näkyä vastausta seuraavankaltaiselta:

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

Kuin aiemmin, koska olet määrittänyt **-Käynnistä** vaihtoehto, selain aukeaa ja näyttää sovelluksesi käynnissä Azure-tietokannassa, kun julkaiseminen on valmis.

![Oma tehtäväluettelo-sivun näyttämistä selainikkunassa. URL-osoite näkyy sivun isännöidään nyt Azure.](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## <a name="stopping-and-deleting-your-application"></a>Lopettaminen ja sovelluksen poistaminen

Jälkeen käyttöönotto sovelluksen, voit poistaa sen käytöstä, jotta voit välttää kustannukset tai luoda ja ottaa käyttöön muissa sovelluksissa maksuttoman kokeiluversion aikajakson sisällä.

Azure laskut web-roolin esiintymät palvelimen ajan tunnissa.
Palvelimen kellon käytetään, kun sovellus otetaan käyttöön, vaikka esiintymät ei ole käynnissä ja pysäytetty tilassa.

Seuraavia ohjeita noudattamalla voit Peruuta ja poista sovellus.

1.  Windows PowerShell-ikkunassa estää palvelun käyttöönoton edellisessä osassa seuraavan cmdlet-komennon avulla luotu:

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    Palvelun pysäyttäminen voi kestää useita minuutteja. Kun palvelu on pysäytetty, näyttöön tulee sanoma, että se on lopetettu.

3.  Jos haluat poistaa palvelun, Soita seuraavan cmdlet-komennon:

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    Kirjoita pyydettäessä **Y** palvelun poistaminen.

    Palvelun poistaminen saattaa kestää useita minuutteja. Kun palvelu on poistettu näyttöön tulee sanoma, joka ilmaisee, että palvelu on poistettu.

  [Node.js Web-sovelluksen Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Tallentamisesta ja Azure tietojen käyttäminen]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js Web-sovelluksen]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 
