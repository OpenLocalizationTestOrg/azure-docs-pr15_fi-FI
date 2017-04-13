<properties
    pageTitle="Web-sovellusten yleiskatsaus | Microsoft Azure"
    description="Katso, miten Azure sovelluksen-palvelun avulla voit kehittää ja host verkkosovellusten"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web-sovellusten yleiskatsaus

*Palvelun Web sovellukset* on täysin hallitun Laske-ympäristö, joka on optimoitu isännöinnin sivustot ja web-sovellukset. Tämä [ympäristö nimellä--palvelun](https://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) tarjoaminen Microsoft Azure avulla voit keskittyä organisaatiosi liiketoimintalogiikan samalla, kun Azure kestää varoen skaalata sovelluksia ja suorita-infrastruktuuria.

Seuraavat 5 minuutin videossa käsitellään Azure palvelun Web sovellukset.

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>Online-sovelluksen palvelun kuvaus

Sovelluksen palvelun *web App-sovelluksessa* on Laske resurssit, joiden Azure tarjoaa isännöinnin sivuston tai web-sovelluksen.  

Laske-resursseja voi olla jaettujen tai Oma näennäiskoneiden (VMs) hinnoittelu taso, joka on valittava mukaan. Sovelluksen koodin suoritetaan hallitun AM, joka on erillään muista asiakkaista.

Koodin voi olla minkä tahansa kielen tai kehys, jota tuetaan [Azure App palvelun](../app-service/app-service-value-prop-what-is.md), kuten ASP.NET, Node.js, Java, PHP tai Python. Voit suorittaa online-sovelluksessa [PowerShellistä ja komentosarjojen muunkielisiä](web-sites-create-web-jobs.md#acceptablefiles) käyttäviä komentosarjoja.

Katso esimerkkejä tyypillinen sovelluksen tilanteita, joissa voit käyttää Web-sovelluksia varten [Web app-skenaariot](https://azure.microsoft.com/documentation/scenarios/web-app/) ja [Azure App palvelu, näennäiskoneiden, palvelun kangasta ja pilvipalveluihin vertailu](choose-web-site-cloud-service-vm.md#scenarios) **skenaariot ja suositukset** -osa.

## <a name="why-use-web-apps"></a>Web Apps käyttötarkoitus

Seuraavassa on palvelun sovelluksen keskeisiä ominaisuuksia, jotka koskevat Web Apps-sovelluksissa:

- **Useita kieliä ja kehysten** - sovellus on ASP.NET, Node.js, Java, PHP ja Python pääosin tuki. Voit suorittaa [PowerShell ja komentosarjoja tai suoritettavat](../app-service-web/web-sites-create-web-jobs.md) -sovelluksen palvelun VMs.

- **DevOps optimointi** - määrittää [Jatkuva integrointi ja käyttöönotto](../app-service-web/app-service-continuous-deployment.md) Visual Studio Team Services, GitHub tai BitBucket. Voit viedä päivitykset [numero- ja väliaikaisen ympäristöissä](../app-service-web/web-sites-staged-publishing.md)kautta. Suorittaa [A / B testaaminen](../app-service-web/app-service-web-test-in-production-get-start.md). Hallinta-sovelluksen palvelun sovelluksia [PowerShellin Azure](../powershell-install-configure.md) - tai [Office kaikissa ympäristöissä käyttöliittymä (CLI)](../xplat-cli-install.md)avulla.

- **Yleinen skaalata ja suuri käytettävyys** - asteikon [ylös](../app-service-web/web-sites-scale.md) - tai [ulos](../monitoring-and-diagnostics/insights-how-to-scale.md) manuaalisesti tai automaattisesti. Isännöidä sovelluksia missä tahansa Microsoftin yleinen palvelinkeskuksen infrastruktuuri- ja sovelluksen palvelun [SLA](https://azure.microsoft.com/support/legal/sla/app-service/) lupaa suuren käytettävyyden.

- **Yhteyksiä SaaS alustojen ja paikalliset tiedot** - valittavana yli 50 [yhdistimet](../connectors/apis-list.md) yrityksen Systemsin (kuten SAP, Siebel ja Oracle) SaaS palvelujen (kuten Salesforce- ja Office 365: ssä) ja internet-palvelujen (kuten Facebook- ja Twitter). Accessin paikalliset tiedot [Hybrid yhteydet](../biztalk-services/integration-hybrid-connection-overview.md) ja [Azure Virtual verkot](../app-service-web/web-sites-integrate-with-vnet.md).

- **Tietosuojaan ja vaatimustenmukaisuuteen** - sovellus on [ISO, SOC, ja PCI yhteensopiva](https://www.microsoft.com/TrustCenter/).

- **Sovellusmallit** - valittavana laajasta luettelosta sovellusmallit [Azure Marketplacesta](https://azure.microsoft.com/marketplace/) , joiden avulla voit asentaa Suositut Avaa lähde-ohjelmistoa, kuten WordPress, Joomla ja Drupal ohjatun toiminnon avulla.

- **Visual Studio integrointi** - erillinen Työkalut Visual Studiossa nopeuttaa luominen ja käyttöönotto virheenkorjaus työmäärä.

Lisäksi web App-sovelluksessa voit hyödyntää tarjoamia [API sovelluksia](../app-service-api/app-service-api-apps-why-best-platform.md) (kuten CORS tukevat) ominaisuudet ja [Mobile-sovellusten](../app-service-mobile/app-service-mobile-value-prop.md) (kuten push-ilmoitukset). Saat lisätietoja sovelluksen tyypit-sovelluksen palvelun artikkelissa [Azure App palvelun yleiskatsaus](../app-service/app-service-value-prop-what-is.md).

Lisäksi Web Apps-sovelluksen palvelun Azure on palvelut, joita voi käyttää isännöinnin sivustot ja web-sovellukset. Useimmissa skenaarioissa Web Apps-sovelluksista on paras valinta.  Harkitse [Palvelun kangasta](https://azure.microsoft.com/documentation/services/service-fabric)microservice arkkitehtuurista, ja jos tarvitset VMs, joka koodisi voidaan määrittää tarkemmin, kannattaa harkita [Azuren näennäiskoneiden](https://azure.microsoft.com/documentation/services/virtual-machines/). Lisätietoja siitä, miten voit valita Azure palveluista on artikkelissa [Azure App palvelu, näennäiskoneiden, palvelun kangasta ja pilvipalveluihin vertailu](choose-web-site-cloud-service-vm.md).

## <a name="getting-started"></a>Käytön aloittaminen

Aloita ottamalla otoksen koodin uusi web app-sovelluksen palvelussa noudattamalla [käyttöönotto ensimmäisen koodiin, 5 minuuttia Azure](app-service-web-get-started.md) -opetusohjelman. Sinun on ilmainen Azure-tili.

Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu](http://go.microsoft.com/fwlink/?LinkId=523751), jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.
