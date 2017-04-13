<properties 
    pageTitle="Azure WebJobs asiakirjat-resurssit" 
    description="Suositeltava Azure WebJobs ja Azure WebJobs SDK käyttäminen koulutusresursseja." 
    services="app-service" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="tdykstra"/>

# <a name="azure-webjobs-documentation-resources"></a>Azure WebJobs asiakirjat-resurssit

## <a name="overview"></a>Yleiskatsaus

Tässä aiheessa on linkkejä Azure WebJobs ja Azure WebJobs SDK käyttäminen asiakirjat-resursseja. Azure WebJobs on helppo tapa suoritetaan komentosarjoja tai ohjelmia taustalla kontekstissa [App palvelun web Appissa, API-sovelluksessa tai mobiilisovelluksessa](../app-service/app-service-value-prop-what-is.md). Voit ladata ja suorittaa suoritettava tiedosto, kuten cmd kuuma exe (.NET) ps1, Näytä, php edellisen vuoden, js ja purkki. Näissä ohjelmissa suoritetaan WebJobs aikataulun (cron) tai jatkuvasti.

[WebJobs SDK](websites-webjobs-resources.md) tarkoituksena on yksinkertaistettava kirjoittamasi usein käytettyjen tehtävien WebJob voi suorittaa, kuten kuvankäsittely, jonon käsittely koodin, RSS kooste tiedoston ylläpito tai lähettäminen sähköpostitse. WebJobs SDK on valmiita ominaisuuksia käyttöä Azuren tallennustilaan ja palvelun Bus, tehtävien ajoittaminen ja virheiden ja monia muita yleisiä tilanteita, joissa. Lisäksi se on suunniteltu extensible ja ei [Avaa lähde säilö tunnisteet](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). [Azure-Funktiot](../azure-functions/functions-overview.md) (tällä hetkellä esikatselu) perustuu WebJobs SDK-paketissa, joka toimii C# komentosarjan, Node.js ja muilla kielillä versio. 

Luominen ja käyttöönotto WebJobs hallinnasta on saumaton integroidut sillä Visual Studiossa kanssa. Voit luoda WebJobs malleista, julkaiseminen ja hallinta (Suorita/Lopeta/näyttö/virheenkorjaus) ne. 

WebJobs Raporttinäkymät-ikkunan Azure-portaalissa on tehokas hallintaominaisuuksia, jotka antavat WebJobs, mukaan lukien pätevyys käynnistää WebJobs yksittäisiä funktioita suorittamisen täydet oikeudet. Koontinäytön näkyy myös funktion CRT Runtime ja kirjaaminen tulos. 

##<a name="getstarted"></a>WebJobs ja WebJobs SDK: N käytön aloittaminen

* [Azure WebJobs esittely](http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx)
* [Azure WebJobs ovat, ja Käynnistä käyttämällä juuri nyt!](http://www.troyhunt.com/2015/01/azure-webjobs-are-awesome-and-you.html) (Blogimerkinnässä Troy Hunt.)
* [Azure WebJobs ominaisuudet](/blog/2014/10/22/webjobs-goes-into-full-production/)
* [Mikä on WebJobs SDK: ssa](websites-dotnet-webjobs-sdk.md)
* [Taustan työt ohjeita Microsoft kuvioiden ja käytännöistä](/documentation/articles/best-practices-background-jobs/)
* [Julkaisu 1.1.0 Microsoft Azure WebJobs SDK RTM](/blog/azure-webjobs-sdk-1-1-0-rtm/)
* [Azure WebJobs SDK: N käytön aloittaminen](websites-dotnet-webjobs-sdk-get-started.md)
* [Voit käyttää Azure jonon tallennustilan WebJobs SDK-paketissa](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)
* [Voit käyttää WebJobs SDK Azure-blob-säiliö](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)
* [Voit käyttää WebJobs SDK Azure-taulukkotallennus](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)
* [Voit käyttää Azure palvelun Bus WebJobs SDK-paketissa](websites-dotnet-webjobs-sdk-service-bus.md)
* [Voit käyttää WebHooks WebJobs-SDK esimerkkien GitHub, IFTTT ja HTTP](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/WebHooks-Walkthrough)
* [Azure WebJobs SDK Quick Reference (Lataa PDF)](http://go.microsoft.com/fwlink/?LinkID=524028&clcid=0x409)
* [GitHub WebJobs asetukset ohjeet](https://github.com/projectkudu/kudu/wiki/Web-jobs).
* Videot
    * [WebJobs ja WebJobs SDK-paketissa](http://channel9.msdn.com/Shows/Cloud+Cover/Episode-153-WebJobs-with-Pranav-Rastogi?utm_source=dlvr.it&utm_medium=twitter)
    * [Azure WebJobs-videosarja kanavan 9: ssä](http://channel9.msdn.com/Tags/azurefridaywebjobs)
    * [Visual Studio Tooling WebJobs esittely](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs sillä ja Remote virheenkorjaus](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster)
    * [Azure WebJobs päivityksen Pranav Rastogi - version 1.1 laajennettavuus](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-183-Azure-WebJobs-Update-with-Pranav-Rastogi)

Katso myös seuraavissa osissa [Käyttöönotto WebJobs](#deploy) ja [Testing ja virheiden WebJobs](#debug).

##<a name="deploy"></a>WebJobs käyttöönotto

* [Voit ottaa käyttöön Azure WebJobs Visual Studiossa](websites-dotnet-deploy-webjobs.md)
* [Ottamisesta käyttöön WebJobs Azure-portaalissa](web-sites-create-web-jobs.md)
* [Komentorivin tai jatkuva Azure WebJobs lähettämisen ottaminen käyttöön](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/)
* [.NET console-sovelluksen ottaminen käyttöön, käyttämällä WebJobs Azure Git](http://blog.amitapple.com/post/73574681678/git-deploy-console-app/)
* [Azure F #-WebJob käyttöönotossa](http://blogs.msdn.com/b/dave_crooks_dev_blog/archive/2015/02/18/deploying-f-web-job-to-azure.aspx)
* [Käyttöönotto mukautettuja palveluita kuin Azure Webjobs](http://withouttheloop.com/articles/2015-06-23-deploying-custom-services-as-azure-webjobs/)
* Videot
    * [Visual Studio Tooling WebJobs esittely](http://channel9.msdn.com/Shows/Web+Camps+TV/Introducing-WebJobs-Tooling-for-Visual-Studio-with-Brady-Gaster) 
    * [WebJobs sillä ja Remote virheenkorjaus](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="schedule"></a>Ajoituksen WebJobs

* [Lisää Azure WebJob-valintaikkuna](websites-dotnet-deploy-webjobs.md#configure)
* [Luo ajoitetun WebJob Azure-portaalissa](web-sites-create-web-jobs.md#CreateScheduled)
* [WebJob ajoituksen työn liitettäessä](http://blog.davidebbo.com/2015/05/scheduled-webjob.html)
* [Ajoituksen Azure WebJobs cron lausekkeiden kanssa](http://blog.amitapple.com/post/2015/06/scheduling-azure-webjobs/)
* [Ajoituksen käyttämällä WebJobs SDK TimerTrigger yksittäisiä WebJob-Funktiot](websites-dotnet-webjobs-sdk.md#schedule)

##<a name="debug"></a>Testaus- ja WebJobs

* [Uusi Kehitystyökalut ja Azure WebJobs Visual Studiossa virheenkorjaus ominaisuudet](http://blogs.msdn.com/b/webdev/archive/2014/11/12/new-developer-and-debugging-features-for-azure-webjobs-in-visual-studio.aspx)
* [Tarkastele WebJobs Raporttinäkymät-ikkunan](websites-dotnet-webjobs-sdk-get-started.md#view-the-webjobs-sdk-dashboard)
* [Voit kirjoittaa lokit WebJobs SDK: N avulla ja tarkastella niitä koontinäyttö](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)
* [Etä muistin WebJobs](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebugwj)
* [Kuka kirjoittamasi kyseisen blob?](http://blogs.msdn.com/b/jmstall/archive/2014/02/19/who-wrote-that-blob.aspx) 
* [Isännöinnin vuorovaikutteinen koodin pilveen](http://blogs.msdn.com/b/jmstall/archive/2014/04/26/hosting-interactive-code-in-the-cloud.aspx)
* [Jäljitä lisääminen Azure WebJobs](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx)
* [Valvonta, diagnosointi ja vianmääritys Microsoft Azuren tallennustilaan](../storage/storage-monitoring-diagnosing-troubleshooting.md)
* Videot
    * [WebJobs sillä ja Remote virheenkorjaus](http://channel9.msdn.com/Shows/Web+Camps+TV/WebJobs-GA-Series-Episode-1-WebJobs-Tooling-with-Brady-Gaster) 

##<a name="scale"></a>WebJobs skaalaus

* [Skaalaus Web-sovelluksen Azure-sivuston käyttö](http://msdn.microsoft.com/magazine/dn786914.aspx)
* [Azure App palvelun: suunnittelee valtaviin asteikko valmis For Business Web App-sovellusten](https://channel9.msdn.com/Events/Build/2014/3-626). Kattaa skaalauksen web Apps-sovellusten kanssa WebJobs, mukaan lukien WebJobs SDK-paketissa.
* Videot
    * [WebJobs laajentaminen](http://channel9.msdn.com/Shows/Azure-Friday/Azure-WebJobs-105-Scaling-out-Web-Jobs)

##<a name="additional"></a>Lisäresursseja WebJobs

* [Azure Magnus Mårtensson WebJobs GA blogimerkinnässä](http://magnusmartensson.com/azure-webjobs-ga)
* [PowerShellin Web töitä Azure sovelluksen-palvelusta](http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx)
* [Ilmoituksen saaminen, kun yhteyttä Azure WebJobs on valmis](http://blog.amitapple.com/post/2014/03/webjobs-notification/)
* [Yksinkertainen Web App varmuuskopiointi säilytyskäytäntö WebJobs kanssa](https://azure.microsoft.com/blog/2014/04/28/simple-web-site-backup-retention-policy-with-webjobs/)
* [Azure Web Apps-sovellusten ja pilvipalveluihin hidas ensimmäisen pyynnön](http://wp.sjkp.dk/windows-azure-websites-and-cloud-services-slow-on-first-request/). Voit simuloida AlwaysOn-ominaisuus, joka on käytettävissä Standard hinnoittelu taso vain WebJobs avulla.
* [WebJobs kaksivaiheista Sammuta](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.U72Il_5OWUl). WebJobs SDK kaksivaiheista Sammuta, katso [kaksivaiheista Sammuta](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#graceful).)
* [Varmuuskopioiden Azure WebJobs & AzCopy automatisointi](http://markjbrown.com/azure-webjobs-azcopy/)
* Videot
    * [Azure WebJobs videot Magnus Mårtensson mukaan](https://www.youtube.com/playlist?list=PLqp1ZOYYUSd81yEzMYLTw8cz91wx_LU9r)
    * [Azure WebJobs-videosarja kanavan 9: ssä](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="additionalsdk"></a>Lisäresursseja WebJobs SDK-paketissa

* [WebJobs SDK julkaisutiedot](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)
* [WebJobs SDK-lähdekoodi](https://github.com/Azure/azure-webjobs-sdk)
* [WebJobs SDK-laajennukset lähdekoodin](https://github.com/Azure/azure-webjobs-sdk-extensions), [yksityiskohtaiset](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview)apuviivaan laajennettavuus-malliin.  
* [WebJobs SDK komentosarjan lähdekoodi](https://github.com/Azure/azure-webjobs-sdk-script/) (käytetään [Azure-funktio](../azure-functions/functions-overview.md))
* [WebJob FREB tiedostojen lataaminen Azure-tallennustilan WebJobs SDK: N avulla](http://thenextdoorgeek.com/post/WAWS-WebJob-to-upload-FREB-files-to-Azure-Storage-using-the-WebJobs-SDK)
* [Isännöinnin Azure webjobs ulkopuolella Azure isännöidään webjob liittyvät azuren kirjaaminen-edut](http://bypassion.dk/?p=510)
* [Tietojen tuonti-työkalu, jonka Azure WebJobs luominen](http://www.freshconsulting.com/building-data-import-tool-azure-webjobs/)
* [Yleisiä tietoja Azure funktioista](../azure-functions/functions-overview.md)
* Videot
    * [Azure WebJobs-videosarja kanavan 9: ssä](http://channel9.msdn.com/Tags/azurefridaywebjobs)

##<a name="samples"></a>Esimerkki WebJob sovellukset

* [Esimerkkisovellusten myöntämä GitHub WebJobs-ryhmä](https://github.com/azure/azure-webjobs-sdk-samples)
* [Yksinkertainen Azure Web App, jossa WebJobs Taustajärjestelmä WebJobs SDK: N avulla](http://code.msdn.microsoft.com/Simple-Azure-Website-with-b4391eeb)
* [SiteMonitR](http://code.msdn.microsoft.com/SiteMonitR-dd4fcf77). Kuvataan ajoitetun ja tapahtumaohjattu WebJobs. Seuraavassa blogimerkinnässä, [joiden avulla Azure WebJobs SDK SiteMonitR](http://www.bradygaster.com/post/rebuilding-the-sitemonitr-using-windows-azure-webjobs).

##<a name="blogs"></a>Blogeja

* [Azure-blogi](/blog)
* [Amit Apple-blogi](http://blog.amitapple.com/). Valitse WebJobs (ei SDK) kohdistukset.
* [Magnus Mårtensson-blogi](http://magnusmartensson.com/)

##<a name="gethelp"></a>WebJobs saaminen

* [WebJobs StackOverflow](http://stackoverflow.com/questions/tagged/azure-webjobs)
* [WebJobs SDK StackOverflow](http://stackoverflow.com/questions/tagged/azure-webjobssdk)
* [Azure-funktioiden StackOverflow](http://stackoverflow.com/questions/tagged/azure-functions)
* [Azure- ja ASP.NET-keskustelupalsta](http://forums.asp.net/1247.aspx)
* [Azure App palvelun Web Apps-keskustelupalsta](http://social.msdn.microsoft.com/Forums/azure/home?forum=windowsazurewebsitespreview)
* [Azure Web Apps käyttäjän ääni-sivusto](https://feedback.azure.com/forums/169385-websites/)
* [Twitter](http://twitter.com/). Käytä hashtag #AzureWebJobs.
* [Raportin WebJobs ohjelmavirhe tai ongelma](https://github.com/projectkudu/kudu/wiki/Reporting-WebJobs-issues)

