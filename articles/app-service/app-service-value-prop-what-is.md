<properties
    pageTitle="Azure web, mobile ja API sovellusten App palvelun | Microsoft Azure"
    description="Katso, miten Azure sovelluksen-palvelun avulla voit kehittää, ottaminen käyttöön ja hallita web- ja mobile-sovellukset."
    keywords="sovelluksen palvelun, azure app service, sovelluksen palvelu kustannus, skaalaus-skaalattava, sovellusten käyttöönoton, azure sovellusten käyttöönoton, paas, ympäristö nimellä--palvelun, sivuston, sivuston, web-azure mobile"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Azure-sovelluksen palvelun kuvaus

*Sovelluksen palvelu* on [platform nimellä--palvelun](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS), Microsoft Azure ojentamassa. Luo Internetin kautta tai mobiilisovellusten ympäristön ja laite. Integroi sovelluksia SaaS ratkaisuja, paikallisen sovellusten yhteydessä ja automatisoi liiketoimintaprosesseja. Azure suoritetaan sovelluksia täysin hallitun näennäiskoneiden (VMs) AM resursseja tai Oma VMs valittua kanssa.

Sovelluksen-palvelu sisältää web- ja mobile ominaisuuksia, jotka on aiemmin toimitettu erikseen Azure sivustot ja Azure Mobile-palvelut. Se on myös uusia ominaisuuksia liiketoimintaprosessien automatisointiin ja isännöinnissä cloud API. Yksittäisen integroitu palveluna App Servicen avulla voit luoda eri osien--sivustot, mobiilisovelluksen takaisin päättyy, RESTful ohjelmointirajapinnan ja liiketoimintaprosessien--yksittäiseksi ratkaisuksi.

Seuraavassa 4 minuutin videossa on lyhyt kuvaus siitä, miten sovelluksen palvelun liittyvät aiemmissa Azure tarjouksia ja uudet ei.

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>Sovelluksen palvelun käyttötarkoitus

Seuraavassa on joitakin ominaisuuksia ja -sovelluksen palvelun toiminnot:

- **Useita kieliä ja kehysten** - sovellus on ASP.NET, Node.js, Java, PHP ja Python pääosin tuki. Voit suorittaa [Windows PowerShellin ja komentosarjoja tai suoritettavat](../app-service-web/web-sites-create-web-jobs.md) -sovelluksen palvelun VMs.

- **DevOps optimointi** - määrittää [Jatkuva integrointi ja käyttöönotto](../app-service-web/app-service-continuous-deployment.md) Visual Studio Team Services, GitHub tai BitBucket. Voit viedä päivitykset [numero- ja väliaikaisen ympäristöissä](../app-service-web/web-sites-staged-publishing.md)kautta. Suorittaa [A / B testaaminen](../app-service-web/app-service-web-test-in-production-get-start.md). Hallinta-sovelluksen palvelun sovelluksia [PowerShellin Azure](../powershell-install-configure.md) - tai [Office kaikissa ympäristöissä käyttöliittymä (CLI)](../xplat-cli-install.md)avulla.

- **Yleinen skaalata ja suuri käytettävyys** - asteikon [ylös](../app-service-web/web-sites-scale.md) - tai [ulos](../monitoring-and-diagnostics/insights-how-to-scale.md) manuaalisesti tai automaattisesti. Isännöidä sovelluksia missä tahansa Microsoftin yleinen palvelinkeskuksen infrastruktuuri- ja sovelluksen palvelun [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) lupaa suuren käytettävyyden.

- **Yhteyksiä SaaS alustojen ja paikalliset tiedot** - valittavana yli 50 [yhdistimet](../connectors/apis-list.md) yrityksen Systemsin (kuten SAP, Siebel ja Oracle) SaaS palvelujen (kuten Salesforce- ja Office 365: ssä) ja internet-palvelujen (kuten Facebook- ja Twitter). Accessin paikalliset tiedot [Hybrid yhteydet](../biztalk-services/integration-hybrid-connection-overview.md) ja [Azure Virtual verkot](../app-service-web/web-sites-integrate-with-vnet.md).

- **Tietosuojaan ja vaatimustenmukaisuuteen** - sovellus on [ISO, SOC, ja PCI yhteensopiva](https://www.microsoft.com/TrustCenter/).

- **Sovellusmallit** - valittavana laajasta luettelosta sovellusmallit [Azure Marketplacesta](https://azure.microsoft.com/marketplace/) , joiden avulla voit asentaa Suositut Avaa lähde-ohjelmistoa, kuten WordPress, Joomla ja Drupal ohjatun toiminnon avulla.

- **Visual Studio integrointi** - erillinen Työkalut Visual Studiossa nopeuttaa luominen ja käyttöönotto virheenkorjaus työmäärä.

## <a name="app-types-in-app-service"></a>Sovelluksen tyypit sovelluksen-palvelussa

Sovelluksen palvelussa on käytettävissä useita *sovelluksen tyypit*, joista jokaisella on tarkoitettu isännöimiseen tietyntyyppisen työmäärää:

- [**Web Apps**](../app-service-web/app-service-web-overview.md) - isännöinnin sivustot ja web-sovellukset.

- [**Mobile-sovellukset**](../app-service-mobile/app-service-mobile-value-prop.md) Mobiilisovelluksen isännöimiseen takaisin päättyy.

- [**API sovellukset**](../app-service-api/app-service-api-apps-why-best-platform.md) - isännöinnin RESTful API.

- [**Logiikan sovellukset**](../app-service-logic/app-service-logic-what-are-logic-apps.md) - liiketoimintaprosessien automatisointiin ja integrointi järjestelmien ja tietoja kirjoittamatta koodia paveikslėlis yli.

Word- *sovelluksen* tähän viittaa omistautunut käynnissä työmäärä isännöintipalvelu resurssit. Ottaen "verkkosovellus" esimerkkinä, olet ehkä tottunut muistitte verkkosovellukseen Laske resurssit ja sovelluksen tunnus, jolla pitää yhdessä toimintoja selaimessa. Mutta App palvelun *web App-sovelluksessa* on Laske resurssit, joiden Azure tarjoaa isännöinnin sovelluksen-koodin. Jos sovelluksen koostuu sivuston edusta- ja RESTful-Ohjelmointirajapinta takaisin loppuun, voi ottaa sekä web App-sovellukseen tai voi ottaa käyttöön edusta koodia verkkosovellukseen ja taustatietokantaan koodin API-sovellukseen. Sovelluksen saattaa koostua useita erilaisia App palvelun sovellukset.

## <a name="app-service-plans"></a>Sovelluksen palvelusopimusten vaihtoehdot

[Sovelluksen palvelun suunnitelmien](azure-web-sites-web-hosting-plans-in-depth-overview.md) määrittää, millaisia Laske resurssien sovelluksia käynnissä olevat. Jos arvelet Vaalea liikenne latautuu, voit käyttää jaettua VMs (**vapaa** ja **jaettujen** hinnat tasoa). Korkeampi latautuu, voit valita useita koot erillinen VMs (**Basic**, **Vakio**ja **Premium** tasoa). Useita sovelluksen Service-sovellukset voivat jakaa vastaavan palvelupaketin ja ne skaalata ylä-ja hinnoittelu tasoa yhdessä palvelupakettiin.

Jos tarvitset lisää skaalattavuus ja verkon eristyksen, voit suorittaa sovellukset [App Service-ympäristössä](../app-service-web/app-service-app-service-environment-intro.md).

## <a name="pricing"></a>Hinnat

Lisätietoja siitä, kuinka paljon App kustannus on artikkelissa [Sovelluksen palvelun hinnat](https://azure.microsoft.com/pricing/details/app-service/).

## <a name="test-drive-app-service"></a>Kokeile sovelluksen-palvelu

[Esimerkki web-sovelluksen luominen, mobiilisovelluksessa tai logiikan sovelluksen](http://go.microsoft.com/fwlink/?LinkId=523751) ja toista sitä 1 tunti, ei ole luottokortin käyttämistä varten tarvitaan, ei ole sitoumukset ei ole ongelmia.

Tai avaa [vapaa Azure-tili](https://azure.microsoft.com/pricing/free-trial/)ja Kokeile jotain Microsoftin aloittaminen opetusohjelmat:

* [Opetusohjelma: Web-sovelluksen luominen](../app-service-web/app-service-web-get-started.md)
* [Opetusohjelma: Luo mobiilisovelluksessa](../app-service-mobile/app-service-mobile-android-get-started.md)
* [Opetusohjelma: API-sovelluksen luominen](../app-service-api/app-service-api-dotnet-get-started.md)
* [Opetusohjelma: Logiikan sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md)
