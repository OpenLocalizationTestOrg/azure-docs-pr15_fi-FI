<properties 
    pageTitle="Opi käyttämään SendGrid sähköpostipalvelu (.NET) | Microsoft Azure" 
    description="Lue, miten lähettää sähköpostia SendGrid sähköpostipalvelu Azure. Esimerkkejä, joiden kirjoitettu C#-koodiin ja käytä .NET-Ohjelmointirajapinnan." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Voit lähettää sähköpostia SendGrid käyttäminen Azure


## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa kerrotaan, miten Azure SendGrid sähköpostipalvelu yleisten ohjelmoinnin tehtävien suorittamiseen. Mallit on kirjoitettu C\#
ja .NET-Ohjelmointirajapinnan käyttäminen. Kattaa skenaariot ovat **luomisesta sähköpostia**, **Sähköpostin lähettäminen**, **liitteiden lisääminen**ja **suodattimien avulla**. Lisätietoja SendGrid ja sähköpostin lähettämistä on kohdassa [vaiheisiin][] .

## <a name="what-is-the-sendgrid-email-service"></a>Mikä on SendGrid sähköpostipalvelu?

SendGrid on [pilvipohjainen sähköpostipalvelu] , joka sisältää luotettava [tapahtumien sähköpostin toimittamista], skaalattavuus ja reaaliaikainen analytics sekä joustava API, jotka helpottavat mukautetun integrointi. Yleisiä SendGrid käyttö tilanteita, joissa ovat seuraavat:

-   Lähettäminen automaattisesti kuittaukset asiakkaille.
-   Kuukausittainen e-fliers ja erikoistarjoukset lähettämisestä asiakkaiden hallinta jakauman luetteloista.
-   Kerääminen reaaliaikainen estetyn sähköpostin ja asiakkaan vastausajan esimerkiksi mittaukset.
-   Voit tunnistaa suuntaukset raporttien luominen.
-   Välittäminen asiakkaan kyselyt.
-   Käsitellään saapuvat sähköpostit.

Lisätietoja on artikkelissa [https://sendgrid.com](https://sendgrid.com) tai tutustu [C# kirjaston][sendgrid csharp]

## <a name="create-a-sendgrid-account"></a>SendGrid-tilin luominen

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Viittaus SendGrid .NET-luokkakirjasto

[SendGrid NuGet paketti](https://www.nuget.org/packages/Sendgrid) on helpoin tapa saada SendGrid Ohjelmointirajapinnan ja määrittää sovelluksen kaikki riippuvuudet. NuGet on Visual Studio-tunniste on mukana Microsoft Visual Studio 2015, joka on helppo Asenna ja Päivitä kirjastoihin ja työkalut. 

> [AZURE.NOTE] Asenna NuGet, jos käytössäsi on aiempi Visual Studio 2015 Visual Studio versio, käy [http://www.nuget.org](http://www.nuget.org)ja **Asenna NuGet** -painiketta.

Voit asentaa sovelluksen SendGrid NuGet-paketti, toimi seuraavasti:

1.  Luo uusi projekti.

    ![Uuden projektin luominen][create-new-project]

2.  Valitse malli.

    ![Valitse malli][select-a-template]

3.  **Ratkaisunhallinnassa** **viittaukset**hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.

4.  Etsi **SendGrid** ja valitse tulokset-luettelosta **SendGrid** -vaihtoehto.

    ![SendGrid NuGet-paketti][SendGrid-NuGet-package]

5.  Valitse Viimeistele asennus valitsemalla **Asenna** ja sulje sitten tässä valintaikkunassa.

SendGrid's .NET luokkakirjasto kutsutaan **SendGridMail**. Se sisältää seuraavat nimitilan:

-   **SendGridMail** luominen ja käyttäminen viestejä.
-   **SendGridMail.Transport** **Web ja muiden**kanssa **SMTP** -protokollaa tai HTTP 1.1-protokollaa sähköpostin lähettämiseen.

Lisää koodi nimitilan-määrityksiä minkä tahansa C alkuun\# tiedosto, jossa haluat käyttää ohjelmallisesti SendGrid sähköpostipalvelu.
**System.Net** ja **System.Net.Mail** ovat .NET Framework-nimitilan, jota sisällytetään, koska ne sisältävät tyypit yleisesti käytät SendGrid-ohjelmointirajapinnan kanssa.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Toimintaohje: sähköpostiviestin luominen

**SendGridMessage** -objektin avulla voit luoda sähköpostiviestin. Kun viestiobjekti on luotu, voit määrittää ominaisuuksia ja kautta, kuten sähköpostin lähettäjä, sähköpostin vastaanottaja ja aihe ja sähköpostiviestin teksti.

Seuraavassa esimerkissä kerrotaan, miten voit luoda täysin täyttää sähköpostin objektin:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Lisätietoja kaikkia ominaisuuksia ja menetelmiä tukemat **SendGrid** tyyppi näy GitHub [sendgrid csharp][] .

## <a name="how-to-send-an-email"></a>Toimintaohje: Lähetä sähköpostiviesti

Kun olet luonut sähköpostiviestin, voit lähettää sen tarjoamia SendGrid verkko-Ohjelmointirajapinnan käyttäminen. Vaihtoehtoisesti voit [käyttöä. VERKON on muodostettu kirjaston](https://sendgrid.com/docs/Code_Examples/csharp.html).

Sähköpostin lähettäminen edellyttää, että annat SendGrid tilin tunnistetiedot (käyttäjänimi ja salasana) tai SendGrid Ohjelmointirajapinnan avain. Ohjelmointirajapinnan avain on suositeltavampaa. Jos tarvitset lisätietoja määrittäminen API näppäimet, käy Microsoftin [dokumentaatio](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Voit tallentaa nämä tunnistetiedot Azure-portaalin kautta valitsemalla Määritä ja lisäämällä "sovelluksen asetukset-avain/arvo-pareina.

 ![Azure app-asetukset][azure_app_settings]

 Tämän jälkeen voit käyttää niitä seuraavasti: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Käyttämällä tunnistetietoja:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

API avaimen avulla:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Seuraavissa esimerkeissä näyttää, miten voit lähettää viestin verkko-Ohjelmointirajapinnan käyttäminen.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Toimintaohje: Lisää liitetiedosto

Liitteiden voidaan lisätä viestiin **AddAttachment** kutsumista ja määrittämällä haluat liittää tiedoston polku ja nimi.
Voit lisätä useita liitteitä soittamalla tätä menetelmää, kun jokaiselle tiedostolle, jonka haluat liittää. Seuraavassa esimerkissä liitteen viestin lisääminen:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Voit lisätä liitteitä myös tietojen **muodossa**. Se on mahdollista soittamalla samalla tavalla kuin edellä **AddAttachment**, mutta tiedot Stream kulkee ja tiedostonimi, jonka haluat näkyvän, kuten viestin. Tässä tapauksessa tarvitset lisää System.IO-kirjasto.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Toimintaohje: käyttöön alatunnisteita, seuranta ja analytics-sovelluksen avulla

SendGrid on muita sähköpostin toimintoja käyttämällä sovellukset. Nämä ovat asetukset, jotka voidaan lisätä sähköpostiviestiin, jos haluat ottaa käyttöön tiettyjä toimintoja, kuten napsauttamalla seuranta, Google analytics, tilauksen seuranta-ja niin edelleen. Katso täydellinen luettelo sovelluksista [App-asetukset][].

Sovellusten voi suojata **SendGrid** sähköpostiviestejä menetelmiä osana **SendGrid** -luokka.

Seuraavissa esimerkeissä näytetään alatunnisteen, valitse Kommentit suodattimet:

### <a name="footer"></a>Alatunniste

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Seuranta

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Toimintaohje: Käytä SendGrid lisäpalveluja

SendGrid on verkkopohjainen ohjelmointirajapinnan ja webhooks, joiden avulla voit hyödyntää SendGrid lisätoimintoja Azure-sovelluksesta. Katso tarkat tiedot [SendGrid API-ohjeissa][].

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut SendGrid sähköpostipalvelu perusteet, noudata näitä linkkejä lisätietoja.

*   SendGrid C\# kirjaston repo: [sendgrid csharp][]
*   SendGrid API-ohjeissa: <https://sendgrid.com/docs>
*   Azure asiakkaiden SendGrid erikoistarjous: [https://sendgrid.com](https://sendgrid.com)

  [Seuraavat vaiheet]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [Sovelluksen asetukset]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API dokumentaatio]: https://sendgrid.com/docs
  
  [cloud-pohjainen sähköpostipalvelu]: https://sendgrid.com/email-solutions
  [tapahtumien sähköpostin lähettämisestä]: https://sendgrid.com/transactional-email
 
