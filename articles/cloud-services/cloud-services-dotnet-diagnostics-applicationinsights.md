<properties
   pageTitle="Vianmääritys sovelluksen tiedot käyttämällä pilvipalveluihin | Microsoft Azure"
   description="Opi pilveen-palvelun vianmääritys sovelluksen havainnollistamisen avulla tietojen Azure Diagnostiikka."
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>Vianmääritys sovelluksen tiedot käyttämällä pilvipalveluihin

[Azure SDK 2,8](https://azure.microsoft.com/downloads/) ja Azure diagnostiikka-tunniste 1,5 voit nyt lähettää Azure diagnostiikka tietojen Cloud-palvelun suoraan hakemuksen tiedot. Tapahtumien seuranta kirjautuu lokit Azure vianmääritys sovelluksen lokit, Windowsin tapahtumalokien, mukaan lukien keräämiä erityyppisiä ja suorituskykylaskureita nyt sovelluksen havainnollistamisen lähettämisen ja sovelluksen tiedot-portaalissa Käyttöliittymä näyttää. Hakemuksen tiedot SDK sekä käytettäessä saat nyt arvot ja lokit sovelluksen sekä järjestelmän ja infrastruktuurin tason tulevat tiedot Azure diagnostiikka tulevat tiedot.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Määritä Azure diagnostiikka tietojen lähettäminen hakemuksen tiedot

Cloud palvelun projektin Azure diagnostiikka tietojen lähettäminen hakemuksen tiedot asetukset seuraavasti.

1) Visual Studio ratkaisunhallinnassa rooliin hiiren kakkospainikkeella ja valitse Avaa rooli suunnittelutyökalu **Ominaisuudet**

![Ratkaisun Explorer roolin ominaisuudet][1]

2) Rooli suunnittelussa diagnostiikka-kohdassa valintaruutu lähettämään **Diagnostiikka tiedot sovelluksen havainnollistamisen**

![Rooli designer diagnostiikka tietojen lähettäminen hakemuksen tiedot][2]

3) Valitse näkyviin tulevassa valintaikkunassa sovelluksen havainnollistamisen resurssi, jotka haluat lähettää Azure diagnostiikka tiedot. Valintaikkunassa voit aiemman hakemuksen tiedot resurssin tilauksesta tai määritä manuaalisesti instrumentation-näppäintä, sovellus tiedot resurssin. Jos sinulla ei ole olemassa olevan sovelluksen tiedot-resurssin sitten voit luoda myös valitsemalla **Luo uusi resurssi** -linkkiä, joka avautuu selainikkunassa Azure perinteinen portaaliin kohtaa, johon voit luoda yritysresurssi hakemuksen tiedot. Katso lisätietoja sovelluksen tiedot-resurssin luominen [Luo uusi sovelluksen tiedot-resurssi](../application-insights/app-insights-create-new-resource.md)

![Valitse sovelluksen tiedot resurssi][3]

4) Kun olet lisännyt sovelluksen tiedot-resurssi, kyseiselle resurssille instrumentation näppäintä tallennetaan nimellä **APPINSIGHTS_INSTRUMENTATIONKEY**palvelun määritys. Voit muuttaa kunkin palvelun määritykset ja ympäristön määritys-asetusta valitsemalla toista kokoonpanoa palvelun määritys avattavasta alas ja määrittämällä instrumentation uuden tunnuksen, määrittämisessä.

![Valitse palvelun asetukset][4]

Visual Studio käyttää **APPINSIGHTS_INSTRUMENTATIONKEY** hakumäärityksen asetus määrittää diagnostiikka-tunniste tarvittavat hakemuksen tiedot resurssitiedot julkaisemisen aikana. Kokoonpanoasetus on kätevä tapa määrittäminen eri instrumentation näppäimet eri määrityksiä. Visual Studio kääntäminen asetusta ja lisätä sen diagnostiikka tunniste julkisen määritysten julkaistessasi. Voidaan yksinkertaistaa määrittämisestä PowerShellin diagnostiikka-tunniste, tulosteen Visual Studio myös paketti sisältää tarvittavat tiedot sovelluksen laitteet sisältävän julkisen määrityksen XML sisällyttää avain. Julkinen config tiedostot Extensions-kansio luodaan, ja noudata kuvion PaaSDiagnostics. <RoleName>. PubConfig.xml. Minkä tahansa mukaan PowerShell-käyttöönotoissa käyttämällä tätä mallia kunkin määritysten yhdistäminen rooli.

5) **Lähetä diagnostiikka tiedot sovelluksen havainnollistamisen** ottaminen käyttöön automaattisesti Määritä Azure diagnostiikka kaikki suorituskyvyn laskureita ja tason virhelokeja, jotka ovat parhaillaan keräämiä Azure diagnostiikka-agentti sovelluksen havainnollistamisen lähettämiseen. Jos haluat määrittää muita tietoja lähetetään sovelluksen havainnollistamisen joudut kunkin roolin *diagnostics.wadcfgx* -tiedostoa muokata manuaalisesti. Katso lisätietoja päivittää manuaalisesti kokoonpanon [Määrittäminen Azure vianmääritys ja Lähetä tiedot sovelluksen havainnollistamisen](../azure-diagnostics-configure-applicationinsights.md) .

Kun Cloud-palvelu on määritetty Azure diagnostiikka tietojen lähettäminen sovelluksen tiedot voit ottamaan se käyttöön Azure, kuten tavalliseen tapaan varmistetaan, että Azure diagnostiikka-tunniste on käytössä. Katso [julkaiseminen Visual Studiossa pilvipalveluun](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Sovelluksen tiedot Azure diagnostiikka tietojen tarkasteleminen
Azure diagnostiikan telemetriatietojen näkyy määritetty pilvipalvelussa sijaitsevaan hakemuksen tiedot resurssin.

Seuraavassa on siitä, miten eri Azure diagnostiikan lokitiedostojen tyypit kartan sovelluksen tiedot-käsitteistä:  

-  Suorituskyvyn laskureita näkyvät Mukautettu arvot-sovelluksen tiedot
-  Windowsin tapahtumalokien näkyvät jäljittää ja mukautetut tapahtumat hakemuksen tiedot
-  Sovelluksen lokit, tapahtumien seuranta-lokit ja diagnostiikka-infrastruktuurin minkä tahansa lokit näkyvät jäljitystä hakemuksen tiedot.

Voit tarkastella tietoja Azure vianmääritys sovelluksen tietoja:

- [Arvot Resurssienhallinnan](../application-insights/app-insights-metrics-explorer.md) avulla voit visualisoida haluamasi mukautettu suorituskyvyn laskureita tai erityyppisiä Windowsin tapahtumalokiin tapahtumien määrä.

![Mukautettua arvot arvot Resurssienhallinnassa][5]

- Eri Jäljityslokit Azure diagnostiikka lähettämä hakuja [haun](../application-insights/app-insights-diagnostic-search.md) avulla. Jos esimerkiksi jos haluat käyttää käsittelemättömän poikkeuksen vuoksi rooli, joka aiheuttaa rooli kaatua ja Roskakorin tiedot näkyvät *Windowsin tapahtumalokiin* *sovelluksen* -kanava. Voit tarkastella Windowsin tapahtumalokiin virheen ja koko pinon jäljitys voit löytää ongelman syy pääkansion poikkeuksen hakutoiminto.

![Etsi jäljittää][6]

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lisää sovellus havainnollistamisen SDK pilvipalveluun, että](../application-insights/app-insights-cloudservices.md) voit lähettää tietoja pyynnöt, poikkeukset, riippuvuudet ja mitä tahansa mukautettuja telemetriatietojen sovelluksestasi. Yhdistää Azure-diagnostiikka saat näkymänä sovelluksen ja järjestelmä-kaikki saman sovelluksen tietoja resurssin tiedot.  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
