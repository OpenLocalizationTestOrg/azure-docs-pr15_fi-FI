<properties
   pageTitle="Logiikan app tilanne: Azure Funktiot palvelun Bus käynnistimen | Microsoft Azure"
   description="Palvelun Bus logiikan sovelluksen käynnistimen luominen Azure-funktioiden avulla"
   services="logic-apps,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>

# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Logiikan app tilanne: Luo Azure palvelun Bus käynnistimen Azure-funktioiden avulla

Azure-funktioiden avulla voit luoda logiikan sovelluksen käynnistimen, kun haluat ottaa käyttöön pitkään suoritettavien listener tai tehtävän. Voit esimerkiksi luoda funktiota, joka jonon Kuuntele ja Kyselysäännön logiikan-sovelluksen push käynnistämisen heti.

## <a name="build-the-logic-app"></a>Logiikan-sovelluksen luominen

Tässä esimerkissä on käynnissä kunkin logiikan sovelluksen käynnistettäväksi korjattava funktiota. Luo logiikan-sovellusta, joka on HTTP-pyyntö käynnistintä. Funktion soittaa kyseiseen päätepisteen aina, kun jonossa viesti vastaanotetaan.  

1. Uuden logiikan; sovelluksen luominen Valitse **Manuaalinen – kun HTTP-pyyntö on vastaanotettu** käynnistintä.  
   Vaihtoehtoisesti voit määrittää JSON rakenteen käyttäminen jonon viestin kuten [jsonschema.net](http://jsonschema.net)-työkalun avulla. Liitä rakenteen käynnistintä. Tämä auttaa ymmärtämään tietojen muodon suunnittelu ja lisää helposti siirtyvät ominaisuudet työnkulussa.
1. Lisää minkä tahansa lisätoimia, jonka haluat ilmetä, kun jonossa viesti vastaanotetaan. Esimerkiksi lähettää sähköpostia Office 365: n kautta.  
1. Voit tallentaa logiikan sovelluksen luomiseen logiikan sovelluksen käynnistimen takaisinsoitto URL-osoite. URL-osoite näkyy käynnistimen kortin.

![Takaisinkutsu URL-osoite näkyy käynnistimen kortti][1]

## <a name="build-the-function"></a>Luo funktio

Seuraavaksi haluat luoda funktion, joka toimii käynnistin ja kuunnella jonossa.

1. [Azure-Funktiot-portaali](https://functions.azure.com/signin)valitsemalla **Uusi funktio**ja valitse sitten **ServiceBusQueueTrigger - C#** -malli.

    ![Azure Funktiot-portaalissa][2]

2. Määritä yhteys palvelun Bus jonossa (joka käyttää Azure palvelun Bus SDK `OnMessageReceive()` listener).
3. Kirjoittaa yksinkertaisen funktion Soita käyttämällä jonossa viestin käynnistimen logiikan app päätepisteen (aiemmin luotu). Seuraavassa on koko Esimerkki funktiota. Esimerkissä `application/json` viestin sisältötyypin, mutta voi muuttaa tarvittaessa.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";

   public static void Run(string myQueueItem, TraceWriter log)
   {

       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Testaa, Lisää jonon viesti, kuten [Palvelun Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer)työkalun kautta. Artikkelissa logiikan sovelluksen Kyselysäännön heti, kun funktio on vastaanottanut viestin.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG
