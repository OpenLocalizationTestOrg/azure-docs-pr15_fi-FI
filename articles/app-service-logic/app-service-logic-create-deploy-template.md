<properties
   pageTitle="Logiikan sovelluksen käyttöönotto mallin luominen | Microsoft Azure"
   description="Lue, miten voit luoda logiikan sovelluksen käyttöönotto-malli ja käyttää sitä Vapauta hallinta"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Logiikan sovelluksen käyttöönotto mallin luominen

Kun logiikan-sovellus on luotu, haluat ehkä luoda Azure Resurssienhallinta-mallina. Tällä tavalla voit helposti ottaa Logiikan sovelluksen ympäristön tai resurssiryhmä, joita saatat joutua se. Olla esittely Resurssienhallinta mallit, tarkista on artikkeleissa [Azure Resurssienhallinta mallien luominen](../resource-group-authoring-templates.md) ja [käyttöönotto resurssien Azure Resurssienhallinta mallien avulla](../resource-group-template-deploy.md).

## <a name="logic-app-deployment-template"></a>Logiikan sovelluksen käyttöönotto-malli

Logiikan-sovellus on kolme perusosaa:

* **Logiikan sovelluksen resurssi**. Tämä sisältää tietoja esimerkiksi hinnat suunnitelma, sijainti ja työnkulun määritys.
* **Työnkulun määritys**. Tämä on mitä nähdään koodi-näkymässä. Se sisältää määritelmän kulun sekä miten moduulin kirjoittavien vaiheita. Tämä on `definition` logiikan app resurssin ominaisuus.
* **Yhteydet**. Nämä ovat eri resurssien avulla turvallisesti tallentaa metatietoja yhdistimen yhteydet, kuten yhteysmerkkijonon ja access-tunnuksen. Näitä logiikan sovelluksen viittaus `parameters` logiikan sovelluksen resurssi-osassa.

Voit tarkastella kaikkia näitä laitteita aiemmin logiikan sovellusten kuten [Azure resurssin Explorer](http://resources.azure.com)-työkalun avulla.

Voit luoda malleja logiikan-sovelluksen käyttäminen resurssien ryhmä-käyttöönotoissa, sinun on resurssien määrittäminen ja parameterize tarpeen mukaan. Esimerkiksi jos olet käyttöönotto kehitystä, Testaa ja tuotantoympäristössä, todennäköisesti haluat käyttää eri yhteyden merkkijonot SQL-tietokantaan kunkin ympäristössä. Vaihtoehtoisesti voit ottaa käyttöön eri tilaukset-tai resurssiryhmiä.  

## <a name="create-a-logic-app-deployment-template"></a>Logiikan sovelluksen käyttöönotto mallin luominen

Helpoin tapa on kelvollinen logiikan sovelluksen käyttöönotto-malli on [Visual Studio-työkalut funktioiden sovellukset](./app-service-logic-deploy-from-vs.md).  Visual Studio-työkalut Luo kelvollinen käyttöönottomalli, jota voi käyttää kaikissa minkä tahansa tilaus tai sijainti.

Muutamia muita työkaluja voi auttaa sinua samalla tavalla kuin logiikan sovelluksen käyttöönotto-malli. Voit luot käsin, eli käyttämällä jo käsitellyt tähän parametrien luonnissa tarvittavat resurssit. Toinen vaihtoehto on [logiikan app mallin luoja](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-moduulin käyttöä. Avaa lähde-moduulin arvioi ensin logiikan-sovellus ja mahdolliset yhteydet, että se on käytössä ja luo sitten mallia resursseja käyttöönottamiseen tarvittavat parametrit. Esimerkiksi jos logiikan-sovellusta, joka vastaanottaa viestin Azure palvelun Bus jonossa ja lisää tiedot Azure SQL-tietokantaan, työkalu säilyttää kaikki tiedonsiirron logiikan ja SQL- ja palvelun Bus yhteyden merkkijonot parameterize niin, että ne voidaan asettaa käyttöönotto.

>[AZURE.NOTE] Yhteyksien on oltava samaan resurssiryhmään logiikan-sovellus.

### <a name="install-the-logic-app-template-powershell-module"></a>Logiikan app mallin PowerShell-moduulin asentaminen

Helpoin tapa-moduulin asentaminen on [PowerShell-valikoima](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)-komennolla `Install-Module -Name LogicAppTemplate`.  

Voit myös asentaa PowerShell-moduulin manuaalisesti:

1. Lataa [logiikan app mallia luontisovellus](https://github.com/jeffhollan/LogicAppTemplateCreator/releases)uusimman version.  
1. Pura kansion PowerShell-moduulin kansioon (yleensä `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Voit käyttää mitä tahansa vuokraajan ja tilauksen käytön moduulin käyttöoikeustietue, on suositeltavaa käyttää [ARMClient](https://github.com/projectkudu/ARMClient) komentorivi-työkalu.  ARMClient tämä [blogimerkintä](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) kerrotaan tarkemmin.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Luo logiikan-sovelluksen malli PowerShell-toiminnolla

Kun PowerShell on asennettu, voit luoda mallin avulla seuraava komento:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`on Azure tilauksen tunnus. Rivin tunnuksen kautta ARMClient, Avaa access ja valitse piiput sitä kautta PowerShell-komentosarjaa ja luo mallin JSON-tiedostossa.

## <a name="add-parameters-to-a-logic-app-template"></a>Lisää parametrit logiikan-sovelluksen malli

Kun olet luonut logiikan-sovelluksen malli, voit jatkaa lisäämisestä ja muokkaamisesta, joita saatat tarvita parametrit. Esimerkiksi jos määrityksen sisältää Resurssitunnus, Azure funktion tai sisäkkäisiä työnkulun, jota aiot ottaa yksittäisen käyttöönoton, voit lisätä lisää resursseja malliin ja parameterize tunnukset tarpeen mukaan. Sama koskee viittauksia mukautetun ohjelmointirajapinnan tai Swagger päätepisteet odotat ottamaan kunkin resurssiryhmän kanssa.

## <a name="deploy-a-logic-app-template"></a>Ottaa käyttöön logiikan-sovelluksen malli

Voit ottaa mallin käyttöön käyttämällä työkaluja, kuten PowerShell, REST API, Visual Studio Vapauta hallinta ja Azure Portal mallin käyttöönottoa. Lue artikkeli tietoja Lisätietoja [käyttöönotto resurssien Azure Resurssienhallinta mallien avulla](../resource-group-template-deploy.md) . Lisäksi on suositeltavaa, että luot [parametri tiedosto](../resource-group-template-deploy.md#parameter-file) , johon parametrin arvot.

### <a name="authorize-oauth-connections"></a>Määritä OAuth yhteydet

Käyttöönoton jälkeen logiikan-sovellus toimii lopusta loppuun kelvollinen parametreilla. Kuitenkin OAuth yhteydet edelleen on oikeus luoda kelvollinen käyttöoikeustietue. Voit tehdä tämän avaamalla logiikan sovelluksen suunnittelussa ja sallimisesta yhteydet. Tai jos haluat automatisoida, komentosarjan avulla voit hyväksyminen kunkin OAuth-yhteyden. Ei Esimerkki-komentosarjan GitHub [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) projektin alle.

## <a name="visual-studio-release-management"></a>Visual Studio Vapauta hallinta

Kuten Visual Studio Vapauta hallinta-työkalun käyttäminen logiikan sovelluksen käyttöönotto-malli on käytetty vaihtoehto käyttöönottoon ja hallintaan ympäristössä. Visual Studio Team Services sisältää [Käyttöönotto Azure resurssiryhmä](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) tehtävän, voit lisätä minkä tahansa muodosta tai vapauttaa myyntijakso. Tarvitset luvan ottaa käyttöön [palvelun pääasiallista](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) ja luoda sitten release-määritys.

1. Valitse Vapauta hallinta-voit luoda uuden määrityksen, valitse **Tyhjä** aloittaaksesi tyhjä määritys.

    ![Luo uusi, tyhjä määritys][1]   

1. Valitse tämän tarvitset resursseja. Tämä on todennäköisesti on luotu manuaalisesti tai muodosta yhteydessä logiikan-sovelluksen malli.
1. Lisää **Azure resurssin ryhmän käyttöönotto** -tehtävä.
1. Määritä [service principal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)ja viittaavat mallin ja Malliparametrit-tiedostoihin.
1. Jatkaa ympäristössä, automaattinen testi tai hyväksyjien tarvittaessa release-vaiheet muodostetaan.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
