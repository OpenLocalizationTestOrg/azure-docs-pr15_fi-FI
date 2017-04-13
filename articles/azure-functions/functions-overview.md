<properties
   pageTitle="Azure toimii yleiskatsaus | Microsoft Azure"
   description="Tietoja Azure-funktioiden käyttämisestä optimoi asynkroninen työmääriä minuutteina."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure Funktiot, funktioiden, käsittely, webhooks, dynaaminen suorittaminen, serverless arkkitehtuuri"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Yleisiä tietoja Azure funktioista

Azure funktiot on käynnissä helposti pystysuuntaisena koodi tai-funktioiden,"ratkaisu pilveen. Voit kirjoittaa tarvitset ongelman mukaan, tarvitsematta koko sovelluksen tai infrastruktuurin suorittaa koodin. Tämä Tee kehittäminen vieläkin tehokkaasti ja voit käyttää värisinä, kuten C#, F #, Node.js, Python tai PHP kehittäminen kieli. Maksaa vain, kun suorittaa koodin ja valvonta Azure skaalata tarpeen mukaan.

Tämä artikkeli sisältää yleiskatsaus Azure-funktiot. Jos haluat siirtyä oikealle ja Azure-funktioiden käytön aloittaminen-aloittaa [Luo ensimmäinen Azure-funktio](functions-create-first-azure-function.md). Jos etsit Tarkempia teknisiä tietoja funktioista, Hae [Sovelluskehittäjän opas](functions-reference.md).

## <a name="features"></a>Ominaisuudet

Seuraavassa on joitakin ominaisuuksia Azure-funktioita:
    
* **Kielen valinta** - C#, F #, Node.js, Python, PHP, erä, bash, Java tai minkä tahansa suoritettavat kirjoitus-funktiot.
* **Maksamisen Käytä hinnoittelu malli** - maksu vain koodin suorittamisen aikaa. Katso [hinnoittelua osan](#pricing) alapuolella dynaaminen sovelluksen-palvelun suunnittelu-vaihtoehto.  
* **Tuo oman riippuvuudet** - Funktiot tukee NuGet ja NPM, jotta voit käyttää tuttuja kirjastoihin.  
* **Integroitu suojaus** - suojaa HTTP saatu Funktiot, joiden OAuth tarjoajien, kuten Azure Active Directory, Facebook, Google, Twitter ja Microsoft Account.  
* **Yksinkertaistetun integrointi** - hyödyntää helposti Azure palvelujen ja ohjelmistojen nimellä--palvelun (SaaS) palveluja. Katso esimerkkejä alla [integroinnit-osassa](#integrations) .  
* **Joustava kehittäminen** - Funktiot oikealla portaalissa koodi tai määrittää jatkuva integroinnin ja ota käyttöön koodisi GitHub, Visual Studio Team Services ja muut [Tuetut Kehitystyökalut](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Avaa lähde** - Funktiot runtime on Avaa lähde ja [GitHub käytettävissä](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Mitä voin tehdä funktioiden kanssa?

Azure funktiot on erinomainen ratkaisu käsittely tietojen integroiminen järjestelmiin, internet-ja-asioita (IoT) käyttämisestä ja luomisesta yksinkertainen ohjelmointirajapinnan ja microservices. Harkitse Funktiot, kuten kuvan tai tilauksen käsittely, tiedoston huollosta, pitkään suoritettavien tehtäviin, jotka haluat suorittaa taustalla viestiketjun tai viittaamaan tehtäviin, jotka haluat suorittaa aikataulun. 

Funktiot on mallien avulla pääset alkuun skenaariota, mukaan lukien seuraavasti:

* **BlobTrigger** - prosessin Azuren tallennustilaan BLOB säilöjen lisättäessä. Voit käyttää tätä varten kuvakoon.
* **EventHubTrigger** - tapahtumien vastata toimitettu Azure-tapahtumaa-toiminnossa. Erityisen hyödyllinen sovelluksen instrumentation, käyttäjän kokemus tai työnkulun käsittelyn ja asioita Internet (IoT) skenaarioita.
* **Yleinen webhook** - prosessin webhook HTTP pyyntöihin palvelu, joka tukee webhooks.
* **GitHub webhook** - vastata tapahtumat, jotka esiintyvät GitHub säilöjen-tietoihin. Esimerkki on artikkelissa [Create webhook tai API-funktiota](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - käynnistimen avulla HTTP-pyynnön koodin suorittaminen.
* **QueueTrigger** - vastata viestit niiden saapuessa Azuren tallennustilaan jonossa. Esimerkki on artikkelissa [luominen Azure-funktiota, joka sitoo Azure-palveluun](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - koodisi yhdistäminen Azure muiden tai paikallinen services mukaan kuunnella viestin olevien. 
* **ServiceBusTopicTrigger** - koodisi yhdistäminen Azure muiden tai paikallinen services tilaamalla aiheisiin. 
* **TimerTrigger** - uudelleenjärjestäminen tai muiden Erätehtävien suorittaminen ennalta määritetyn aikataulun. Esimerkki on artikkelissa [käsitellään funktion tapahtuman luominen](functions-create-an-event-processing-function.md).

Azure Funktiot tukee *käynnistimien*, jotka ovat aloittamistapoja koodin suorittaminen ja *sidontojen*, joka tapoja coding syöttö- ja tietojen yksinkertaistaminen. Yksityiskohtainen kuvaus käynnistimien ja sidontojen, johon Azure funktiot on artikkelissa [Azure Funktiot käynnistimien ja sidontojen Sovelluskehittäjän opas](functions-triggers-bindings.md).


## <a name="integrations"></a>Integroinnit

Azure funktiot on integroitu erilaisten Azure ja 3rd osapuolen palvelujen kanssa. Voit tehdä nämä funktion käynnistäminen ja aloita suorittaminen tai yhteyshenkilönä syötteenä ja tulos on koodisi. Seuraavat palvelun integroinnit tuetaan Azure-funktioissa. 

* Azure DocumentDB
* Azure tapahtuman keskittimet 
* Azure Mobile-sovellusten (taulukot)
* Azure ilmoituksen keskittimet
* Azure palvelun Bus (olevien ja aiheet)
* Azure Storage (blob, olevien ja taulukot) 
* GitHub (webhooks)
* Paikallisen (käyttäen palvelua Bus)

## <a name="pricing"></a>Kuinka paljon toimii kustannukset?

Azure funktiot on kahdenlaisia hinnoittelua suunnitelmat, valitse jokin tarpeitasi parhaiten sopiva: 

* **Dynaaminen isännöinnin suunnitelma** - funktion suorittamisen, Azure on kaikki tarvittavat laskennallinen resurssit. Sinun ei tarvitse huolehtia Resurssienhallinta ja maksat vain kerran, joka suoritetaan, koodisi. Koko hinnoittelutiedot ovat käytettävissä [Funktiot hinnoittelua sivun](/pricing/details/functions). 

* **Sovelluksen palvelusopimus** - Suorita oman toimintoja aivan kuten web, mobile ja API-sovellukset. Kun käytät jo App palvelun muiden sovellusten, että funktioita voit suorittaa vastaavan palvelupaketin ilman lisäkustannuksia. Tarkat tiedot löytyvät [App palvelun hinnat sivun](/pricing/details/app-service/).

Lisätietoja skaalaus oman Funktiot, katso, [miten skaalata Azure-Funktiot](functions-scale.md).

##<a name="next-steps"></a>Seuraavat vaiheet

+ [Luo ensimmäinen Azure-funktio](functions-create-first-azure-function.md)  
Siirry oikealle ja luo ensimmäinen toimintoa Azure Funktiot pikaopas. 
+ [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md)  
Tarkempia teknisiä tietoja Azure Funktiot runtime ja viittaus on coding Funktiot ja käynnistimien ja sidontojen määrittäminen.
+ [Azure Funktiot testaaminen](functions-test-a-function.md)  
Tässä artikkelissa kuvataan eri työkaluja ja menetelmiä oman Funktiot testikäyttöön.
+ [Miten Azure Funktiot](functions-scale.md)  
Tässä artikkelissa käsitellään palvelusopimusten vaihtoehdot käytettävissä Azure-toimintoja, kuten dynaaminen palvelusopimus ja voit valita oikean suunnitelma. 
+ [Lisätietoja Azure sovelluksen-palvelusta](../app-service/app-service-value-prop-what-is.md)  
Azure Funktiot hyödyntää perustoiminnot, kuten ominaisuuksissa ympäristömuuttujat ja diagnostiikka Azure App Service-ympäristön. 