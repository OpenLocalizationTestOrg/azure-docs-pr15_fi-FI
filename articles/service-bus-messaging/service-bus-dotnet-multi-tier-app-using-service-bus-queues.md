<properties
    pageTitle=".NET monitasoisten-sovellus | Microsoft Azure"
    description=".NET-opetusohjelma, jonka avulla voit kehittää monitasoisten-sovellus, joka käyttää palvelun Bus olevien kommunikoimiseen tasojen välissä Azure-tietokannassa."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="sethm"/>

# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>.NET monitasoisten-sovelluksesta valitsemalla Azure palvelun Bus olevien

## <a name="introduction"></a>Johdanto

Microsoft Azure kehittäminen on helppoa for .NET Visual Studio ja Azure vapaaseen SDK avulla. Tässä opetusohjelmassa esitellään useita Azure resursseja paikallisessa ympäristössä käyttävän sovelluksen luomisessa. Vaiheissa oletetaan, että sinulla ei ole edellisen kokemus käyttämällä Azure.

Opit seuraavat:

-   Voit määrittää tietokoneen Azure kehittämiseen latauksella ja asenna.
-   Miten Azure kehitä Visual Studio avulla.
-   Miten monitasoisten-sovelluksen luominen Azure Internetin kautta tai työntekijä roolien avulla.
-   Miten vaihtamaan käyttäen palvelua Bus olevien tasojen välissä.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

Tässä opetusohjelmassa luominen ja suorittaminen monitasoisten sovelluksen Azure pilvipalvelussa. Edustan on ASP.NET MVC web-rooli ja uudelleen päivitetään Työntekijä-roolin, joka käyttää palvelun Bus jonossa. Voit luoda saman monitasoisten-sovelluksen kanssa edustatietokanta web projektina, jotka on otettu käyttöön Azure sivustoon Pilvipalvelun sijaan. Ohjeita siitä, mitä voit tehdä eri tavalla Azure sivuston edusta-kohdassa [seuraavat vaiheet](#nextsteps) . Voit myös kokeilla [.NET--paikallisen/cloud hybrid sovelluksen](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) opetusohjelman.

Seuraavassa näyttökuvassa näkyy valmiin sovelluksen.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Skenaarion yhteenveto: välisten roolin viestintä

Lähetä tilauksen käsittelyä varten, edusta Käyttöliittymä-osan verkko-roolin käynnissä on käsitellä Keskitaso logiikan Työntekijä-roolin käynnissä. Tässä esimerkissä käytetään palvelun Bus se-että tasojen väliseen viestintään messaging.

Se messaging web ja Keskimmäinen tasoa välillä käyttämällä decouples kaksi osaa. Toisin kuin suora messaging (eli TCP tai HTTP)-web-tason muodostaa Keskitaso suoraan. sen sijaan se vie työn, yksiköt viesteinä-palvelun Bus, joka säilyttää ne luotettavasti, kunnes Keskitaso on valmis tarjoaman ja käsitellä niitä kyselyjä.

Palvelun Bus sisältää kaksi, jotka tukevat se messaging: olevien ja aiheet. Olevien viesti lähetetään jonossa on kulutettu yksittäisen vastaanottajan mukaan. Ohjeita tukea Julkaise ja tilaa kuviota, jossa julkaistun viesti otetaan käyttöön tilaukseen, joka on rekisteröity aihetta. Jokaisen tilauksen ylläpitää loogisesti Omat jonossa olevien viestien. Tilaukset voidaan määrittää myös suodatusehdot, joka rajoittaa tilauksen jonossa niihin, jotka vastaavat suodattimen välitetään viestejä joukkoa. Seuraavassa esimerkissä palvelun Bus olevien.

![][1]

Viestintä-järjestelmä on suora messaging useita etuja:

-   **Ajallinen irtautus.** Asynkroninen tekstiviesti kuvion tuottajat ja kuluttajille ei tarvitse olla online samanaikaisesti. Palvelun Bus tallentaa viestit luotettavasti, kunnes vievää osapuoli on valmiina vastaanottamaan ne. Näin jaetun sovelluksen katkaistaan, joko vapaaehtoisesti, esimerkiksi ylläpito tai osan, kaatumisen vuoksi ilman vaikuttavat kokonaisuutena järjestelmän osat. Lisäksi vievää sovelluksen ehkä vain tulee online aikoina päivän aikana.

-   **Kuormituksen tasaus.** Monissa sovelluksissa järjestelmän kuormituksen vaihtelee ajan myötä samalla, kun kukin työmäärän yksikkö käsittelyaika on yleensä vakio. Intermediating viestin tuottajat ja kuluttajille jonon kanssa tarkoittaa, että vievää sovelluksen (työntekijän) vain on valmisteltu sopimaan keskimääräinen kuormituksen piikin kuormituksen sijaan. Jonossa syvyys kasvaa ja sopimuksia kuin saapuvan kuormituksen vaihtelee. Toiminto tallentaa suoraan rahaa infrastruktuurin vastaamisessa sovelluksen lataa tarvittavan määrän perusteella.

-   **Kuormituksen tasaamisen.** Lataa kasvaa, kun Lisää työntekijä prosessit voidaan lisätä lukea jonon. Kukin viesti käsitellään vain yksi työntekijä prosesseista. Lisäksi tässä salaus puretaan perustuva kuormituksen avulla työntekijä koneet optimaalisen käytön vaikka työntekijä koneet eroavat kannalta käsittely power kuin ne otetaan viestien etsiminen omia enimmäisnopeus. Tämä kaava on usein termillä *pikaluistelukilpailuissa kuluttaja* -kuvion.

    ![][2]

Seuraavissa osissa käsitellään koodi, joka sisältää tässä arkkitehtuuri.

## <a name="set-up-the-development-environment"></a>Määritä kehitysympäristö

Ennen kuin aloitat Azure sovellusten kehittämiseen, Hanki työkalut ja kehitysympäristön määritys.

1.  Asenna Azure SDK .NET [Työkalut ja SDK][]-palvelussa.

2.  Valitse **Asenna SDK** -versiota käytät Visual Studio. Tässä opetusohjelmassa käytetään Visual Studio 2015.

4.  Kun sinua pyydetään suorittamaan tai tallentamaan asennusohjelma, valitse **Suorita**.

5.  **WWW-ympäristö asennusohjelma**valitsemalla **Asenna** ja jatkaa asennusta.

6.  Kun asennus on valmis, sinulla on kaikki tarvittavat voit kehittää sovellusta. SDK sisältää työkaluja, joiden avulla voit helposti kehittää Azure sovellusten Visual Studiossa. Jos sinulla ei ole asennettu Visual Studiossa, SDK asentaa myös ilmainen Visual Studio Express.

## <a name="create-a-namespace"></a>Luo nimitila

Seuraavaksi luodaan palvelun nimitila ja hankkia jaettu Access allekirjoitus (SAS)-näppäintä. Nimitilan on sovelluksen rajan kunkin sovelluksen tarjoamia palvelun Bus kautta. Järjestelmä luo Suojaussidosten avaimen nimitilan luomisen yhteydessä. Nimitilan ja Suojaussidosten avaimen yhdistelmä on tunnistetietojen palvelun Bus tarkistamiseen access-sovellukseen.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Web-roolin luominen

Tässä osassa voit luoda edusta-sovelluksen. Luo ensin sivut, jotka näytetään.
Tämän jälkeen lisää koodi, joka lähettää palvelun Bus jonon kohteet ja näyttää jonossa tilatietoja.

### <a name="create-the-project"></a>Projektin luominen

1.  Käytä järjestelmänvalvojan oikeuksia, Käynnistä Microsoft Visual Studio. Käynnistä Visual Studio ja järjestelmänvalvojan oikeudet, **Visual Studio** -ohjelmakuvaketta hiiren kakkospainikkeella ja valitse sitten **Suorita järjestelmänvalvojana**. Azure Laske emulaattori, jäljempänä tässä artikkelissa käsiteltävät edellyttää, että Visual Studio alkamaan järjestelmänvalvojan oikeuksilla.

    Visual Studiossa, valitse **Tiedosto** -valikosta **Uusi**ja valitse sitten **Projekti**.

2.  **Asennetut mallit**, **Visual C#**Valitse **Cloud** ja valitse sitten **Azure pilvipalvelussa**. Projektin **MultiTierApp**nimi. Valitse **OK**.

    ![][9]

3.  **.NET Framework 4.5** roolit Kaksoisnapsauta **ASP.NET Web rooli**.

    ![][10]

4.  Valitse **Azure pilvipalvelussa ratkaisu** **WebRole1** pitämällä hiiren osoitinta, valitse kynä-kuvake ja web-roolin nimeksi **FrontendWebRole**. Valitse **OK**. (Varmista, että kirjoitat "edusta-pienet"e,"ei"FrontEnd"kanssa.)

    ![][11]

5.  Valitse **Uusi ASP.NET-projekti** -valintaikkunassa **Valitse malli** -luettelosta **MVC**.

    ![][12]

6. Valitse edelleen **Uusi ASP.NET-projekti** -valintaikkunassa valitse **Muuta** Authentication. **Muuta todennus** -valintaikkunassa **Ei todennus**ja valitse sitten **OK**. Tässä opetusohjelmassa, olet käyttöönotto sovelluksen käyttäjän kirjautumistunnus silloin.

    ![][16]

7. Takaisin sisään **Uusi ASP.NET-projekti** -valintaikkuna valitsemalla **OK** voit luoda projektin.

6.  **Ratkaisunhallinnassa** **FrontendWebRole** projektin **viittaukset**hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

7.  **Selaa** -välilehti ja valitse Hae `Microsoft Azure Service Bus`. Valitse **Asenna**ja hyväksy käyttöehdot.

    ![][13]

    Huomaa, että tarvittavat asiakkaan kokoonpanon viitataan kooditiedostoja on lisätty.

9.  **Ratkaisunhallinnassa** **mallien** hiiren kakkospainikkeella ja valitse **Lisää**ja valitse sitten **luokka**. Kirjoita **nimi** -ruutuun nimi **OnlineOrder.cs**. Valitse **Lisää**.

### <a name="write-the-code-for-your-web-role"></a>Web-roolin koodin kirjoittaminen

Tässä osassa voit luoda eri sivut, jotka näytetään.

1.  Korvaa olemassa olevan nimitilan määrityksen OnlineOrder.cs tiedoston Visual Studiossa, seuraava koodi:

    ```
    namespace FrontendWebRole.Models
    {
        public class OnlineOrder
        {
            public string Customer { get; set; }
            public string Product { get; set; }
        }
    }
    ```

2.  Kaksoisnapsauta **Ratkaisunhallinnassa** **Controllers\HomeController.cs**. Lisää seuraavat **käyttämällä** lauseet tiedosto, joka sisältää nimitilan mallille luomaasi, sekä palvelun Bus yläreunassa.

    ```
    using FrontendWebRole.Models;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    ```

3.  Myös Visual Studiossa HomeController.cs-tiedostossa, korvaa seuraava koodi olemassa olevan nimitilan määrityksen. Tämä koodi sisältää menetelmiä käsittely kohteiden esittäminen jonossa.

    ```
    namespace FrontendWebRole.Controllers
    {
        public class HomeController : Controller
        {
            public ActionResult Index()
            {
                // Simply redirect to Submit, since Submit will serve as the
                // front page of this application.
                return RedirectToAction("Submit");
            }
    
            public ActionResult About()
            {
                return View();
            }
    
            // GET: /Home/Submit.
            // Controller method for a view you will create for the submission
            // form.
            public ActionResult Submit()
            {
                // Will put code for displaying queue message count here.
    
                return View();
            }
    
            // POST: /Home/Submit.
            // Controller method for handling submissions from the submission
            // form.
            [HttpPost]
            // Attribute to help prevent cross-site scripting attacks and
            // cross-site request forgery.  
            [ValidateAntiForgeryToken]
            public ActionResult Submit(OnlineOrder order)
            {
                if (ModelState.IsValid)
                {
                    // Will put code for submitting to queue here.
    
                    return RedirectToAction("Submit");
                }
                else
                {
                    return View(order);
                }
            }
        }
    }
    ```

4.  Valitse **Muodosta** **Luominen ratkaisu** Testaa työsi tarkkuudella tähän mennessä.

5.  Seuraavaksi luodaan näkymän `Submit()` aiemmin luomasi menetelmää. Napsauta hiiren kakkospainikkeella `Submit()` menetelmä (on liikaa `Submit()` , joka käyttää parametreja ei ole), ja valitse sitten **Lisää näkymä**.

    ![][14]

6.  Näkymän luomisen valintaikkuna. Valitse **malli** -luettelosta valitsemalla **Luo**. Valitse **mallin luokka** -luettelosta **OnlineOrder** -luokka.

    ![][15]

7.  Valitse **Lisää**.

8.  Nyt sovellus näkyvissä nimen muuttaminen. Kaksoisnapsauta **Ratkaisunhallinnassa** **Views\Shared\\_Layout.cshtml** tiedoston avaaminen Visual Studio-editorin.

9.  Korvaa kaikki esiintymät **Oma ASP.NET-sovelluksen** **käyttäjän LITWARE-tuotteiden**kanssa.

10. Poista **Aloitus**, **tietoja**ja **yhteyshenkilön** linkit. Poista korostettu koodi:

    ![][28]

11. Lopuksi Muokkaa lisättävä jonossa tietoja lähetys-sivu. Kaksoisnapsauta **Ratkaisunhallinnassa** **Views\Home\Submit.cshtml** -tiedoston avaaminen Visual Studio-editorin. Lisää seuraava rivi jälkeen `<h2>Submit</h2>`. Nyt `ViewBag.MessageCount` on tyhjä. Voit täyttää sen myöhemmin.

    ```
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```

12. Oman Käyttöliittymän on nyt käytössä. Painamalla **F5** suorittamaan sovelluksesi ja varmista, että se on oikein.

    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Kirjoita koodi palvelun Bus jonon kohteiden lähettämistä varten

Lisää seuraavaksi koodin jonon kohteiden lähettämistä varten. Luo ensin luokka, joka sisältää palvelun Bus-jonossa yhteystietoja. Valitse alusta Global.aspx.cs-yhteyden. Päivitä lopuksi aiemmin luomasi tekstimuodossa lähetyksen koodi HomeController.cs lähettää todella palvelun Bus jonon kohteet.

1.  Napsauta **Ratkaisunhallinnassa**hiiren kakkospainikkeella **FrontendWebRole** (hiiren kakkospainikkeen projektin ei rooli). Valitse **Lisää**ja valitse sitten **luokka**.

2.  Luokan **QueueConnector.cs**nimi. Valitse **Lisää** uuden luokan.

3.  Lisää seuraavaksi tunnus, jolla kapseloi yhteystiedot ja alustaa palvelun Bus jonon yhteys. Korvaa seuraava koodi QueueConnector.cs koko sisällön ja kirjoita arvot `your Service Bus namespace` (nimitilan nimi) ja `yourKey`, joka on saatu Azure portaalin aiemmin **perusavain** .

    ```
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.ServiceBus;
    
    namespace FrontendWebRole
    {
        public static class QueueConnector
        {
            // Thread-safe. Recommended that you cache rather than recreating it
            // on every request.
            public static QueueClient OrdersQueueClient;
    
            // Obtain these values from the portal.
            public const string Namespace = "your Service Bus namespace";
    
            // The name of your queue.
            public const string QueueName = "OrdersQueue";
    
            public static NamespaceManager CreateNamespaceManager()
            {
                // Create the namespace manager which gives you access to
                // management operations.
                var uri = ServiceBusEnvironment.CreateServiceUri(
                    "sb", Namespace, String.Empty);
                var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                    "RootManageSharedAccessKey", "yourKey");
                return new NamespaceManager(uri, tP);
            }
    
            public static void Initialize()
            {
                // Using Http to be friendly with outbound firewalls.
                ServiceBusEnvironment.SystemConnectivity.Mode =
                    ConnectivityMode.Http;
    
                // Create the namespace manager which gives you access to
                // management operations.
                var namespaceManager = CreateNamespaceManager();
    
                // Create the queue if it does not exist already.
                if (!namespaceManager.QueueExists(QueueName))
                {
                    namespaceManager.CreateQueue(QueueName);
                }
    
                // Get a client to the queue.
                var messagingFactory = MessagingFactory.Create(
                    namespaceManager.Address,
                    namespaceManager.Settings.TokenProvider);
                OrdersQueueClient = messagingFactory.CreateQueueClient(
                    "OrdersQueue");
            }
        }
    }
    ```

4.  Varmista seuraavaksi **alusta** -menetelmä kutsutaan. Kaksoisnapsauta **Ratkaisunhallinnassa** **Global.asax\Global.asax.cs**.

5.  Lisää seuraava rivi koodin **Application_Start** menetelmä lopussa.

    ```
    FrontendWebRole.QueueConnector.Initialize();
    ```

6.  Päivitä-web-koodi, aiemmin luodussa karttavisualisoinnissa lähettää jonossa kohteet. Kaksoisnapsauta **Ratkaisunhallinnassa** **Controllers\HomeController.cs**.

7.  Update `Submit()` menetelmä (liikaa, joka ei ole parametrit) seuraavasti, jos haluat saada viestin Laske jonon.

    ```
    public ActionResult Submit()
    {
        // Get a NamespaceManager which allows you to perform management and
        // diagnostic operations on your Service Bus queues.
        var namespaceManager = QueueConnector.CreateNamespaceManager();
    
        // Get the queue, and obtain the message count.
        var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
        ViewBag.MessageCount = queue.MessageCount;
    
        return View();
    }
    ```

8.  Update `Submit(OnlineOrder order)` menetelmä (liikaa, yksi parametri) seuraavasti, jos haluat lähettää jonossa tilauksen tiedot.

    ```
    public ActionResult Submit(OnlineOrder order)
    {
        if (ModelState.IsValid)
        {
            // Create a message from the order.
            var message = new BrokeredMessage(order);
    
            // Submit the order.
            QueueConnector.OrdersQueueClient.Send(message);
            return RedirectToAction("Submit");
        }
        else
        {
            return View(order);
        }
    }
    ```

9.  Voit nyt suorittaa sovellus uudelleen. Aina, kun olet lähettänyt tilauksen sanoman määrä kasvaa.

    ![][18]

## <a name="create-the-worker-role"></a>Työntekijä-roolin luominen

Luo nyt työntekijän rooli, joka käsittelee tilauksen lähetysten. Tässä esimerkissä käytetään **Työntekijä rooleihin palvelun Bus jonon** Visual Studio project-malli. Jo saatu tarvittavat tunnistetiedot-portaalista.

1. Varmista, että Visual Studio yhdistettyjä Azure-tili.

2.  Hiiren kakkospainikkeella Visual Studiossa, napsauta **Ratkaisunhallinnassa** **MultiTierApp** projektin **roolit** -kansio.

3.  Valitse **Lisää**ja valitse sitten **Uuden työntekijän rooli projektin**. **Lisää uusi rooli projekti** -valintaikkuna.

    ![][26]

4.  Valitse **Lisää uusi projekti-roolin** -valintaikkunan **Työntekijän rooli palvelun Bus jonon kanssa**.

    ![][23]

5.  Kirjoita **nimi** -ruutuun nimi projektin **OrderProcessingRole**. Valitse **Lisää**.

6.  Kopioi yhteysmerkkijono, joka on saatu "Luominen palvelun Bus nimitilan"-osan vaiheessa 9 Leikepöydälle.

7.  Napsauta **Ratkaisunhallinnassa**hiiren kakkospainikkeella **OrderProcessingRole** , jonka loit vaiheessa 5 (Varmista, että napsautat hiiren kakkospainikkeella **OrderProcessingRole** **roolit**ja ei luokan). Valitse **Ominaisuudet**.

8.  **Ominaisuudet** -valintaikkunan **asetukset** -välilehdessä Valitse **arvo** -ruutuun **Microsoft.ServiceBus.ConnectionString**ja Liitä kopioimasi vaiheessa 6 päätepisteen arvon.

    ![][25]

9.  Luo **OnlineOrder** -luokan edustavan tilaukset, kuten voit käsitellä niitä jonossa. Voit käyttää olet jo luonut luokan. Napsauta **Ratkaisunhallinnassa**hiiren kakkospainikkeella **OrderProcessingRole** luokan (hiiren kakkospainikkeen luokka-kuvake ei rooli). Valitse **Lisää**ja valitse sitten **Olemassa olevan kohteen**.

10. **FrontendWebRole\Models**alikansion selaamalla ja kaksoisnapsauta sitten **OnlineOrder.cs** lisääminen tähän projektiin.

11. Muuta **WorkerRole.cs** **QueueName** muuttuja arvon `"ProcessingQueue"` , `"OrdersQueue"` seuraava koodi esitetyllä tavalla.

    ```
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```

12. Lisää seuraava lauseella WorkerRole.cs tiedoston yläosassa.

    ```
    using FrontendWebRole.Models;
    ```

13. - `Run()` Toimi sisäpuolella `OnMessage()` Soita, korvaa sisällön `try` lauseen seuraava koodi kanssa.

    ```
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```

14. Sovellus on suoritettu. Voit testata koko sovelluksen ratkaisunhallinnassa MultiTierApp projektin hiiren kakkospainikkeella, valitsemalla **Määritä projektin aloitus**ja painamalla F5-näppäintä. Huomaa, että viestin määrä ei kasvattaa, koska Työntekijä-roolin käsittelee jonossa kohteet ja merkitsee ne valmiiksi. Näet Työntekijä-roolin jäljitystiedot tarkastelemalla Azure Laske emulaattorin UI. Voit tehdä tämän tehtäväpalkin ilmaisinalueella emulaattorin-kuvaketta hiiren kakkospainikkeella ja valitsemalla **Näytä Laske emulaattorin-Käyttöliittymä**.

    ![][19]

    ![][20]

## <a name="next-steps"></a>Seuraavat vaiheet  

Lisätietoja palvelun Bus on seuraavissa resursseissa:  

* [Azure palvelun Bus][sbmsdn]  
* [Palvelun Bus service-sivu][sbwacom]  
* [Opi käyttämään palvelua Bus olevien][sbwacomqhowto]  

Lisätietoja monitasoisten tilanteita, joissa on seuraavissa artikkeleissa:  

* [.NET monitasoisten sovelluksen tallennustilan taulukot, olevien ja BLOB-objektit][mutitierstorage]  

  [0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-01.png
  [1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
  [2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
  [Työkalut ja SDK-paketissa]: http://go.microsoft.com/fwlink/?LinkId=271920


  [GetSetting]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.getsetting.aspx
  [Microsoft.WindowsAzure.Configuration.CloudConfigurationManager]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx
  [NamespaceMananger]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx

  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx

  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx

  [EventHubClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventhubclient.aspx

  [9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
  [10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
  [11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
  [12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
  [13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
  [14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
  [15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
  [16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
  [17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-36.png
  [18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-37.png

  [19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
  [20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
  [23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
  [25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
  [26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
  [28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

  [sbmsdn]: http://msdn.microsoft.com/library/azure/ee732537.aspx  
  [sbwacom]: /documentation/services/service-bus/  
  [sbwacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
  [mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
  