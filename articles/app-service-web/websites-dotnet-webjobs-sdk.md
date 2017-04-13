<properties 
    pageTitle="Mikä on Azure WebJobs SDK" 
    description="Azure WebJobs SDK esittely. Tässä artikkelissa kerrotaan, mitä SDK on tavallista mahdollisuutta se on hyödyllinen ja näytteiden koodi." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="what-is-the-azure-webjobs-sdk"></a>Mikä on Azure WebJobs SDK

## <a id="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, mitä WebJobs SDK on, tarkistaa havainnollistetaan Yleisiä tilanteita, se on hyödyllinen ja on yleiskuvaus siitä, miten voit käyttää sitä koodissa.

[WebJobs](websites-webjobs-resources.md) toimintoa Azure App palvelu, jonka avulla voit suorittaa ohjelman tai komentosarjan web Appissa, API-sovelluksen tai mobiilisovelluksessa kontekstissa. [WebJobs SDK](websites-webjobs-resources.md) tarkoituksena on yksinkertaistettava kirjoittamasi usein käytettyjen tehtävien WebJob voi suorittaa, kuten kuvankäsittely, jonon käsittely koodin, RSS kooste tiedoston ylläpito tai lähettäminen sähköpostitse. WebJobs SDK on valmiita ominaisuuksia käyttöä Azuren tallennustilaan ja palvelun Bus, tehtävien ajoittaminen ja virheiden ja monia muita yleisiä tilanteita, joissa. Lisäksi se on suunniteltu extensible. [WebJobs SDK on Avaa lähde](https://github.com/Azure/azure-webjobs-sdk/)ja ei [Avaa lähde säilö tunnisteet](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview).

WebJobs SDK sisältää seuraavat osat:

* **NuGet paketit**. NuGet-paketteja, jotka on lisätty Visual Studio konsolisovelluksen projektin on kehys, jonka koodia käyttämien sisustuksessa oman menetelmiä WebJobs SDK-ominaisuuksilla.
  
* **Raporttinäkymät-ikkunan**. Osa WebJobs SDK App Azure-palvelu sisältyy sekä annetaan monipuolisia seuranta- ja diagnostiikka NuGet paketit käyttävät ohjelmat. Sinun ei tarvitse kirjoittaa koodia nämä seuranta- ja diagnostiikka-ominaisuuksia.

## <a id="scenarios"></a>Skenaariot

Tässä on muutamia tyypillinen skenaarioita, voit käsitellä välillä Azure WebJobs SDK kanssa:

* Kuva käsittely tai Suoritinta kuormittavaa työtä. Sivustojen yleinen ominaisuus on mahdollisuus ladata kuvia tai videoita. Usein haluat käsitellä sisältöä, kun se on ladattu palvelimeen, mutta et halua tehdä käyttäjän, odota, kun teet näin.

* Käsittely. Web-edusta Taustajärjestelmä-palvelun kanssa kommunikoimiseen yleisiä tapa, jolla on käyttää olevien. Kun sivusto on työskentelyyn, vie viestin jonon sivulle. Taustajärjestelmä palvelu hakee jonossa sähköpostiviestejä ja onko työhön. Voit käyttää olevien kuvankäsittely: esimerkiksi sen jälkeen, kun käyttäjä Lataa useita tiedostoja, sijoita tiedostonimet jonon viestin kerätään mukaan Taustajärjestelmä käsittelyä varten. Tai voi käyttää olevien sivuston vastausajan parantamiseksi. Esimerkiksi sijaan kirjoittaminen suoraan SQL-tietokantaan, kirjoittaa jono, onko käyttäjälle, olet valmis, ja anna palvelun kahvaa viive relaatio taustatietokanta toimi. Esimerkki jonon kuva prosessin käsittely-kohdassa [WebJobs SDK Aloita opetusohjelma](websites-dotnet-webjobs-sdk-get-started.md).

* RSS-kooste. Jos sinulla on sivuston, joka säilyttää RSS-syötteiden luettelon, voi tuoda kaikista taustalla syötteiden artikkeleja.

* Tiedoston ylläpito, kuten yhdistäminen tai Siivotaan lokitiedostot.  Voit joutua lokitiedostojen luomisen useita sivustoja tai erillisen aikavälit, johon haluat yhdistää, jotta voit suorittaa analyysia työt. Tai saatat haluta ajoittaa tehtävän suorittamiseen viikoittain vanha lokitiedostojen Puhdista.

* Tunkeutumisen Azure taulukoiksi. Saattaa tallennettuja tiedostoja ja BLOB-objektit ja jäsentää niitä ja tiedot tallennetaan taulukoihin. Tunkeutumisen-funktio voi kirjoittaa paljon rivejä (miljoonia joissakin tapauksissa) ja WebJobs SDK tekee toteuttaa tämä toiminnallisuus helposti. SDK sisältää myös reaaliaikainen seuranta tilanneilmaisimet, kuten kirjoitettujen taulukon rivien määrä.

* Muut pitkään suoritettavien tehtävät, jotka haluat suorittaa taustalla viestiketjun, kuten [lähettäminen sähköpostitse](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure). 

* Tehtävät, jotka haluat suorittaa aikataulussa, kuten varmuuskopioida toiminnon jokaisen yöllä.

Monissa näissä tilanteissa haluat ehkä skaalata verkkosovellukseen voidaan suorittaa useita VMs, jonka suorittaa useita WebJobs samanaikaisesti. Joissakin tilanteissa Tämä saattaa johtaa samat tiedot käsitellä useita kertoja, mutta tämä ei ole ongelma käytettäessä valmiin jonossa, Blob-objektien ja palvelun Bus käynnistimien WebJobs SDK: n. SDK varmistaa, että kunkin viestin ja Blob-objektien käsitellään vain kerran oman funktiot.

WebJobs SDK myös on helppo käsittelee yleiset skenaariot Virheenkäsittely. Voit määrittää ilmoituksia, ilmoitukset lähetetään, kun funktio epäonnistuu, ja voit määrittää aikakatkaisu niin, että funktion automaattisesti peruutetaan, jos se ei täytä määritetyn ajan kuluessa.

## <a id="code"></a>MALLIKOODEJA

Tyypillinen tehtäviä, jotka toimivat Azuren tallennustilaan käsittelyyn koodi on yksinkertainen. Console-sovelluksessa `Main` menetelmää voit luoda `JobHost` objektia, jonka koordinoi kirjoitat menetelmiä puhelut. WebJobs SDK framework tietää, milloin Soita oman menetelmät ja mitä parametriarvot käyttämään perusteella voit käyttää niitä WebJobs SDK määritteet. SDK sisältää *käynnistimien* , jotka määrittävät, mitä ehdot saada funktion nimi on ja *sidonta* , joka määrittää, kuinka tietoja sisään ja ulos menetelmäparametrit.

Esimerkiksi [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) määrite aiheuttaa toiminto, jota kutsutaan, kun viesti on vastaanotettu jonossa ja jos viesti on JSON DBCS-taulukko-tai mukautetun tyypin, viesti on automaattisesti sarjoitus. [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) määrite käynnistää prosessin aina, kun uusi Blob-objektien luodaan Azure-tallennustilan tilin.

Tässä on yksinkertainen ohjelma, joka tekee kyselyn jonossa ja luo Blob-objektien jokainen vastaanotettu jonon viesti:

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

`JobHost` Objekti on joukko taustan Funktiot säilö. `JobHost` Objektin valvoo Funktiot, tapahtumia, jotka käynnistävän, niiden avulla ja käynnistää Funktiot käynnistimen tapahtumien yhteydessä. Voit soittaa `JobHost` menetelmä, haluatko säilö-prosessi, jos haluat suorittaa nykyisen säikeen tai taustan viestiketjun. Esimerkissä `RunAndBlock` menetelmä toimii prosessin jatkuvasti nykyisen säikeen.

Koska `ProcessQueueMessage` tässä esimerkissä menetelmässä on `QueueTrigger` määrite käynnistin, jotka toimivat on uuden viestin jonon vastaanotto. `JobHost` Objektin seuraa jonossa uusia viestejä määritetyn jonossa ("webjobsqueue" tässä esimerkissä) ja kun jokin löytyy, kutsuu `ProcessQueueMessage`. 

`QueueTrigger` Määrite sidonnat `inputText` parametrin arvoksi jonon viestin. Ja `Blob` määrite sidonnat `TextWriter` nimeltä "blobname" nimi "containername" säilön blob-objekti.  

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

Funktio käyttää näitä parametreja sitten jonon viestin arvon kirjoittaminen blob:

        writer.WriteLine(inputText);

WebJobs SDK käynnistin ja kansion ominaisuudet yksinkertaistamaan koodia, kun haluat kirjoittaa. Alatason koodi olevien BLOB-objektit ja tiedostojen käsittelemiseen tai aloittaa Ajoitetut tehtävät tehdään WebJobs SDK framework puolestasi. Esimerkiksi puitteissa Luo jonossa olevien, joita ei ole vielä, Avaa, lukee jono viestejä, poistaa jonon viestejä, kun käsittely on valmis, Luo blob säilöjen, joita ei ole vielä, kirjoittaa BLOB-objektit ja niin edelleen.

Seuraava koodiesimerkki näyttää erilaisia käynnistimien yhden WebJob: `QueueTrigger`, `FileTrigger`, `WebHookTrigger`, ja `ErrorTrigger`. 

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

        public static void ErrorMonitor(
        [ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
        [SendGrid(
            To = "admin@emailaddress.com",
            Subject = "Error!")]
         SendGridMessage message)
        {
            // log last 5 detailed errors to the Dashboard
            log.WriteLine(filter.GetDetailedMessage(5));
            message.Text = filter.GetDetailedMessage(1);
        }
    }
```

## <a id="schedule"></a>Ajoitus

`TimerTrigger` Määritteen avulla voidaan suorittaa aikataulun käynnistimen funktiot. Voit ajoittaa WebJob kokonaisuutena kautta Azure tai aikataulun WebJob, WebJobs SDK: N avulla yksittäiset toiminnot `TimerTrigger`. Tässä on koodiesimerkki.

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

Lisää otoksen koodin artikkelissa azure-webjobs-sdk-laajennukset-säilössä [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) GitHub.com.

## <a name="extensibility"></a>Laajennettavuus

Et ole rajoitettu sisäistä toimintoa--WebJobs SDK avulla voit kirjoittaa mukautetun käynnistimien ja sidonta.  Voit esimerkiksi kirjoittaa käynnistimet välimuistin tapahtuma- ja jonka aikatauluja. [Avaa lähde säilöön](https://github.com/Azure/azure-webjobs-sdk-extensions) on [yksityiskohtainen opas WebJobs SDK laajennettavuus](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) ja otoksen koodi avulla pääset alkuun oman käynnistimien ja sidonta kirjoittamiseen.

## <a id="workerrole"></a>WebJobs SDK: N ulkopuolella WebJobs avulla

Ohjelma käyttää WebJobs SDK on vakio Console-sovellusta ja suorita tahansa--siinä ei ole WebJob suorittamista. Voit testata ohjelma paikallisesti kehittäminen tietokoneesta ja tuotannon voit suorittaa sitä pilvipalvelussa työntekijän rooli tai Windows-palvelun halutessasi jokin näistä ympäristössä. 

Kuitenkin koontinäyttö on käytettävissä vain laajennuksen Azure App palvelun web-sovelluksen. Jos haluat suorittaa ulkopuolella WebJob ja edelleen käyttää koontinäyttö, voit määrittää saman tallennustilan-tiliä, joka viittaa WebJobs SDK Raporttinäkymät-ikkunan yhteysmerkkijono ja että web Appin WebJobs Raporttinäkymät-ikkunan sitten näkyy funktion suorittamisen ohjelmasta, jossa on johonkin muuhun tietoja verkkosovellukseen. Saat koontinäyttö URL-osoite https://*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions avulla. Lisätietoja on artikkelissa [paikallisen kehitys WebJobs SDK Raporttinäkymät-ikkunan käytön](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), mutta Huomaa, että blogimerkintä näkyy vanha yhteys-merkkijono nimi. 

## <a id="nostorage"></a>Koontinäytön ominaisuudet

WebJobs SDK on monia etuja, vaikka et käytä WebJobs SDK käynnistimet tai sidonta:

* Voit kutsua Funktiot-koontinäytössä.
* Voit toistaa Funktiot-koontinäytössä.
* Voit tarkastella lokit koontinäytön liitetty tiettyyn WebJob (sovelluksen lokit, kirjoitettu käyttämällä Console.Out, Console.Error, seuranta, jne.) tai tietty funktio kutsun, joka on luotu ne linkitetään (lokit kirjoitettu käyttämällä `TextWriter` objektia, jonka SDK välittää funktiolle parametrina). 

Katso lisätietoja, [miten voit käynnistää manuaalisesti funktioon](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) ja [kirjoittaa lokit](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs) 

## <a id="nextsteps"></a>Seuraavat vaiheet

Saat lisätietoja WebJobs SDK [Azure WebJobs suositellaan resurssit](http://go.microsoft.com/fwlink/?linkid=390226).

Lisätietoja uusimman parannukset WebJobs SDK on artikkelissa [Julkaisutiedot](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes).
 
