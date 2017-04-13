<properties
    pageTitle="Työnkulku, logiikan sovellukset, toiminnot ja WebJobs väliltä valitseminen | Microsoft Azure"
    description="Vertaile ja kontrasti-cloud integration services Microsoftin ja päättää, mitä sinun on käytettävä palveluihin."
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="Microsoft työnkulku, työnkulku, logiikan sovellusten azure Funktiot, funktioiden azure webjobs, webjobs, tapahtuman käsittely-dynaaminen Laske serverless arkkitehtuuri"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>Työnkulku, logiikan sovellukset, toiminnot ja WebJobs väliltä valitseminen

Tässä artikkelissa verrataan ja kontrastin Microsoftin pilvipalvelussa, jossa voit kaikki ratkaista integroinnin ongelmia ja liiketoimintaprosessien automaatio seuraavat palvelut:

- [Microsoft-työnkulku](https://flow.microsoft.com/)
- [Azure logiikan sovellukset](https://azure.microsoft.com/services/logic-apps/)
- [Azure-Funktiot](https://azure.microsoft.com/services/functions/)
- [Azure App palvelun WebJobs](../app-service-web/web-sites-create-web-jobs.md)

Kaikki Yhteisöpalvelut on hyötyä "liimaaminen" yhdessä erillisiä systems. Ne kaikki määrittävät, syöte, toiminnot, ehdot ja tulos. Voit suorittaa kullekin niistä aikataulun tai käynnistintä. Kuitenkin kunkin palvelun Lisää arvo ainutlaatuisia ja verrata niitä ei ole kysymyksiä "mikä palvelu on paras?" mutta yhden "mikä palvelu parhaiten sopii tilanne?" Usein näiden palvelujen yhdistelmä on paras tapa luoda nopeasti skaalattava, koko esitellyt integrointi ratkaista.

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>Työnkulku ja logiikka sovellukset

Voit Käsittelemme Microsoft Flow ja Azure logiikan sovelluksia yhdessä koska ne ovat molemmat *määritysten ensimmäisen* integrointipalveluiden, joka on helppo luoda prosesseja ja työnkulkuja ja integrointi SaaS ja yrityksen eri sovellusten. 

- Työnkulku rakentuu logiikan sovellukset
- Heillä on saman työnkulun suunnittelussa
- [Yhdistimiä](../connectors/apis-list.md) , jotka toimivat yhdessä toimii myös muihin

Työnkulkujen avulla minkä tahansa office-työntekijän yksinkertainen Integrointien suorittaminen (kuten hankkiminen SMS tärkeitä sähköpostiviestejä) avaamatta kehittäjät tai IT. Toisaalta logiikan Apps ottaa Lisäasetukset tai kriittisten Integrointien (kuten B2B prosessit) sijainti yritystason DevOps ja suojaus käytännöt. On tyypillinen liiketoiminnan kasvattamista monimutkaisuus ylityömäärä työnkulun. Voit vastaavasti ensimmäinen työnkulku alkaa ja valitse Muunna logiikan app tarpeen mukaan.

Seuraavan taulukon avulla voit selvittää, onko vuota tai logiikan sovellusten parhaat annetun integrointia varten.

|               | Työnkulku                                                                             | Logiikan sovellukset                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Käyttäjäryhmälle      | Office-sovellusta, yrityskäyttäjät                                                   | IT-ammattilaisille-kehittäjille                                                                                 |
| Skenaariot     | Itsepalvelu                                                                     | Kriittisten                                                                                    |
| Suunnittelutyökalu   | Selaimessa, vain Käyttöliittymä                                                              | Selain- ja [Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md), [Koodinäkymän](../app-service-logic/app-service-logic-author-definitions.md) käytettävissä |
| DevOps        | Itsenäisen, kehittää tuotannon                                                    | tietolähteen ohjausobjektin, testaaminen, tuki- ja automaatio- [Azure Resurssienhallinta](../app-service-logic/app-service-logic-arm-provision.md) hallinta|
| Järjestelmänvalvoja-toiminto| [https://Flow.microsoft.com](https://flow.microsoft.com)                       | [https://Portal.Azure.com](https://portal.azure.com)                                                |
| Tietoturva      | Tavallisia: [tietojen paikallisen tietosuojan](https://wikipedia.org/wiki/Technological_Sovereignty), [salauksen rest-palvelussa](https://wikipedia.org/wiki/Data_at_rest#Encryption) luottamuksellisia tietoja, jne. | Suojauksen oikeellisuuden Azure: [Azure suojaus](https://www.microsoft.com/trustcenter/Security/AzureSecurity), [Tietoturvakeskus](https://azure.microsoft.com/services/security-center/) [valvontalokit](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)tai Lisää. |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>Funktioiden ja WebJobs

Olemme voivat keskustella Azure-Funktiot ja Azure App palvelun WebJobs yhdessä koska ne ovat molemmat *koodin ensimmäisen* integrointipalveluiden ja suunniteltu kehittäjille. Niiden avulla voit suorittaa komentosarjan tai koodin eri tapahtumia, kuten [Uusi tallennustilan BLOB](functions-bindings-storage.md) tai [WebHook, pyydä](functions-bindings-http-webhook.md). Seuraavassa on niiden yhteiset ominaisuudet: 

- Molemmat on suunniteltu [Azure](../app-service/app-service-value-prop-what-is.md) -sovelluksen palveluun ja Nauti ominaisuudet [ja tietolähteen ohjausobjektin](../app-service-web/app-service-continuous-deployment.md), [todennus](../app-service/app-service-authentication-overview.md)ja [valvonta](../app-service-web/web-sites-monitor.md).
- Molemmat ovat developer koskivat palveluita.
- Sekä tukevat komentosarjan ja ohjelmointi kieliä.
- Molemmat on NuGet ja NPM tukevat.

Funktiot on WebJobs luonnollinen kehitys, se tulee WebJobs parasta ominaisuutta ja parantaa heille. Parannukset ovat seuraavat: 

- Virtaviivaistettu keskihajonta, Testaa ja suorittaa koodin suoraan selaimessa.
- Valmiin integrointi Lisää Azure ja 3rd osapuolen palvelut, kuten [GitHub WebHooks](https://developer.github.com/webhooks/creating/).
- Maksu-käyttökertakustannuksia, ei tarvitse maksaa [Sovelluksen-palvelun suunnittelu](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
- Automaattisen [dynaaminen skaalaus](functions-scale.md).
- Asiakkaista App palvelun sovelluksen-palvelun käytön suunnitteleminen (voi hyödyntää käyttää resursseja).
- Logiikan sovellusten integrointi.

Seuraavassa taulukossa on yhteenveto Funktiot ja WebJobs erot:

|                        | Funktiot                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| Skaalaus                | Configurationless skaalaus                                                                                                                                                | Skaalaa ja sovelluksen palvelusopimus        |
| Hinnat                | Maksun käyttäminen tai sen osien App palvelusopimus                                                                                                                                  | Sovelluksen palvelusopimus osa           |
| Suorita-tyyppi               | saatu, ajoittaa (avulla ajastin käynnistimen)                                                                                                                                  | käynnistettävät, jatkuva, ajoitettu   |
| Käynnistimen tapahtumat         | [ajastimen](functions-bindings-timer.md)ja [Azure DocumentDB](functions-bindings-documentdb.md), [Azure tapahtuman keskittimet](functions-bindings-event-hubs), [HTTP/WebHook (GitHub, liukuma)](functions-bindings-http-webhook.md), [Azure palvelun mobiilisovellukset](functions-bindings-mobile-apps.md), [Azure ilmoituksen keskittimet](functions-bindings-notification-hubs.md), [Azure palvelun Bus](functions-bindings-service-bus.md), [Azuren tallennustilaan](articles/functions-bindings-storage.md) | [Azure-tallennustilan](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), [Azure palvelun Bus](websites-dotnet-webjobs-sdk-service-bus.md)         |
| Selain-kehittäminen | x                                                                                                                                                                        |                                    |
| Ikkunan komentosarjan       | koe                                                                                                                                                             | x                                  |
| PowerShellin             | koe                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| Bash                   | koe                                                                                                                                                             | x                                  |
| PHP                    | koe                                                                                                                                                             | x                                  |
| Python                 | koe                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | koe                                                                                                                                                             | x                                  |

Haluatko käyttää funktioita tai WebJobs kädessä riippuu siitä, mitä jo teet sovelluksen-palvelun kanssa. Jos sinulla on sovelluksen Service-sovellus, jonka haluat suorittaa koodikatkelmat ja haluat hallita niitä yhdessä samalla DevOps-ympäristössä, sinun on käytettävä WebJobs. Jos haluat suorittaa koodikatkelmat Azure services tai jopa 3rd osapuolten sovelluksissa, tai jos haluat hallita integrointi-koodikatkelmat erikseen App Service-sovelluksista tai jos haluat soittaa koodikatkelmat logiikan-sovelluksesta, olisi voit hyödyntää kaikki parannukset funktioissa.  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>Työnkulku, logiikan sovellukset ja funktioita yhdessä

Kuten edellä mainittiin mikä palvelu on sinulle parhaiten riippuen tilannettasi. 

- Yksinkertainen business optimointi käyttää työnkulku.
- Jos integrointi skenaarion liian advanced kulkuun tai tarvitset DevOps ominaisuuksia ja suojauksen compliances, käytä logiikan sovellukset.
- Jos vaihe integrointi käyttämässäsi skenaariossa vaatii erittäin mukautetun muunnoksen tai erityinen koodi, kirjoittaa funktion-sovelluksen ja käynnistää sitten toiminnoksi logiikan sovelluksen funktion.

Voit osallistua logiikan app vuo. Voit kutsua funktion logiikan-sovelluksen ja logiikka app myös funktion. Työnkulku, logiikan sovellukset ja funktioiden välinen integrointi edelleen parantaa ylityökustannukset. Voit luoda jotain yksi palvelu ja käyttää sitä muissa palveluissa. Kaikki nämä kolme technologies tekemäsi sijoitus on sopia.

## <a name="next-steps"></a>Seuraavat vaiheet

Aloittaminen palvelut luomalla oman ensimmäinen työnkulku, logiikan app, funktio sovelluksen tai WebJob. Valitse jokin seuraavista linkeistä:

- [Microsoft Flow käytön aloittaminen](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Luo ensimmäinen Azure-funktio](../azure-functions/functions-create-first-azure-function.md)
- [Ottaa käyttöön Visual Studiossa WebJobs](../app-service-web/websites-dotnet-deploy-webjobs.md)

Tai saada lisätietoja näistä integrointipalveluiden sisältää seuraavat linkit:

- [Azure-funktioita & Azure App palvelun hyödyntäminen integrointi skenaarioissa saattanut Anderson mukaan](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Yksinkertainen tekemät Charles Lamanna integroinnit](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [Logiikan sovellusten Live Web-lähetystä](http://aka.ms/logicappslive)
- [Microsoft Flow usein kysytyt kysymykset](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure WebJobs asiakirjat-resurssit](../app-service-web/websites-webjobs-resources.md)
