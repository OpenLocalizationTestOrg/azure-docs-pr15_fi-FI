<properties
   pageTitle="Resurssienhallinta Tuetut palvelut | Microsoft Azure"
   description="Tässä artikkelissa kuvataan, resurssi-palvelut, jotka tukevat Resurssienhallinta, niiden mallit ja käytettävissä API-versiot ja alueet, voit isännöidä resurssit."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="magoedte;tomfitz"/>

# <a name="resource-manager-providers-regions-api-versions-and-schemas"></a>Resurssienhallinta tarjoajien, alueet, API-versiot ja rakenteet

Azure Resurssienhallinta on uudella tavalla, voit ottaa käyttöön ja palvelut, jotka muodostavat sovellusten hallinta. Useimmissa, mutta ei kaikkiin, palvelujen tukevat Resurssienhallinta ja joidenkin palvelujen tuki Resurssienhallinta vain osittain. Tämä artikkeli sisältää luettelo tuetuista resurssin tarjoajat Azure Resurssienhallinta.

Resurssien otettaessa haluat myös mitkä alueet näiden Tukiresurssit ja resurssien käytettävissä olevat API versiot. [Tuetut alueet](#supported-regions) -osan avulla voit selvittää, mitkä alueet työtä tilauksen ja resursseja. [Tuetut API versiot](#supported-api-versions) -osiossa kerrotaan tietoja sen selvittämisestä, voit käyttää API versiot.

Mitä palveluita tuetaan Azure portaalin ja perinteinen portaalissa on ohjeartikkelissa [Azure portaalin käytettävyys kaavion](https://azure.microsoft.com/features/azure-portal/availability/). Nähdäksesi, mitkä palvelut siirtäminen tukilähteisiin, katso [Siirrä uusi resurssiryhmä ja tilauksen resurssit](resource-group-move-resources.md).

Seuraavissa taulukoissa luetellaan, mitkä Microsoft Services-palvelujen tuki käyttöönottaminen ja hallinnan Resurssienhallinta ja mitkä eivät. **Pikaopas mallit** -sarakkeessa olevaa linkkiä lähettää kyselyn Azure pikaopas malleja säilöön määritettyä resurssia palveluntarjoajasi. Pikaopas mallit lisätään ja päivitetään säännöllisesti. Tietyn palvelun linkin esiintyminen ei välttämättä tarkoita kysely palauttaa malleja säilöstä. Saatavilla on myös useita kolmannen osapuolen resurssin palvelut, jotka tukevat Resurssienhallinta. Opit miten saat näkyviin kaikki resurssin [resurssin tarjoajat ja tiedostotyypit](#resource-providers-and-types) -osassa tarjoajat.


## <a name="compute"></a>Laske

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------------------------ |-------- | ------ | ------ |
| Erän   | Kyllä     | [Erän muille käyttäjille](https://msdn.microsoft.com/library/azure/dn820158.aspx) | [2015-12-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-12-01/Microsoft.Batch.json)       |  |
|Säilö | Kyllä | [Säilön palvelun muille käyttäjille](https://msdn.microsoft.com/library/azure/mt711470.aspx) | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.ContainerService.json) | [Microsoft.ContainerService](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ContainerService%22&type=Code) |
| Dynamics elinkaari-palvelut | Kyllä  |      |        |  |
| Skaalaa joukot | Kyllä | [Asteikko määrittää muille käyttäjille](https://msdn.microsoft.com/library/azure/mt705635.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachineScaleSets](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=virtualMachineScaleSets&type=Code) | 
| Palvelun kangasta | Kyllä  | [Palvelun kangasta muille käyttäjille](https://msdn.microsoft.com/library/azure/dn707692.aspx)  |      | [Microsoft.ServiceFabric](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceFabric%22&type=Code) |
| Näennäiskoneiden | Kyllä | [AM MUILLE KÄYTTÄJILLE](https://msdn.microsoft.com/library/azure/mt163647.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) | [virtualMachines](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Compute%2Fvirtualmachines%22&type=Code) |
| Näennäiskoneiden (perinteinen) | Rajoitettu | - | - | - |
| Remote-sovellus | Ei      | -  | - | - |
| Cloud Services (perinteinen) | Rajoitettu (Katso alla oleva kuva) | - | - | - |

Näennäiskoneiden (perinteinen) viittaa resurssit, jotka on otettu käyttöön – perinteinen käyttöönottomalli sijaan kautta resurssien hallinnan käyttöönottomalli. Yleensä nämä resurssit eivät tue Resurssienhallinta toimintoja, mutta on joitakin toimintoja, jotka on otettu käyttöön. Saat lisätietoja näiden käyttöönoton mallien [tietoja resurssien hallinnan käyttöönotto- ja perinteinen käyttöönotto](resource-manager-deployment-model.md). 

Perinteinen resursseihin; kanssa voidaan käyttää pilvipalveluihin (perinteinen) kuitenkin perinteinen resurssien ei voit hyödyntää kaikkia Resurssienhallinta-ominaisuuksia ja eivät ole hyvä vaihtoehto tulevien ratkaisuja. Harkitse, käytä resursseja Microsoft.Compute, Microsoft.Storage ja Microsoft.Network nimitilan sovelluksen infrastruktuurin muuttamista.


## <a name="networking"></a>Verkko

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | -------  | -------- | ------ | ------ |
| Sovelluksen yhdyskäytävän | Kyllä | [Sovelluksen yhdyskäytävän muille käyttäjille](https://msdn.microsoft.com/library/azure/mt684939.aspx)   |        | [applicationGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FapplicationGateways%22&type=Code) |
| DNS     | Kyllä | [DNS-MUUT](https://msdn.microsoft.com/library/azure/mt163862.aspx)         | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Network.json) | [dnsZones](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FdnsZones%22&type=Code) |
| ExpressRoute | Kyllä  | [ExpressRoute muille käyttäjille](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       | [expressRouteCircuits](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FexpressRouteCircuits%22&type=Code) |
| Kuormituksen | Kyllä  | [Kuormituksen muille käyttäjille](https://msdn.microsoft.com/library/azure/mt163651.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [loadBalancers](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Floadbalancers%22&type=Code) |
| Liikenteen hallinta | Kyllä | [MUUT liikenteen hallinta](https://msdn.microsoft.com/library/azure/mt163667.aspx) | [2015-11-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-11-01/Microsoft.Network.json)  | [trafficmanagerprofiles](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Ftrafficmanagerprofiles%22&type=Code) |
| Virtuaalinen verkot | Kyllä| [Virtuaalinen verkon muille käyttäjille](https://msdn.microsoft.com/en-us/library/azure/mt163650.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) | [virtualNetworks](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworks%22&type=Code) |
| VPN-yhdyskäytävän | Kyllä | [Yhdyskäytävien muille käyttäjille](https://msdn.microsoft.com/library/azure/mt163859.aspx) |  | [virtualNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FvirtualNetworkGateways%22&type=Code) <br /> [localNetworkGateways](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2FlocalNetworkGateways%22&type=Code) <br />[yhteydet](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Network%2Fconnections%22&type=Code) |


## <a name="storage"></a>Tallennustilan

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------- | ------ |
| Tallennustilan | Kyllä  | [Tallennustilan muille käyttäjille](https://msdn.microsoft.com/library/azure/mt163683.aspx) | [Tallennustilan tilin](resource-manager-template-storage.md) | [Microsoft.Storage](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Storage%22&type=Code) |
| StorSimple | Kyllä  |         |        |  |

## <a name="databases"></a>Tietokannat

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------- | ------ |
| DocumentDB | Kyllä  | [DocumentDB muille käyttäjille](https://msdn.microsoft.com/library/azure/dn781481.aspx) | [2015 04-08](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-08/Microsoft.DocumentDB.json)  | [Microsoft.DocumentDB](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DocumentDb%22&type=Code) |
| Redis välimuisti | Kyllä |   | [2016-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-01/Microsoft.Cache.json) | [Microsoft.Cache](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cache%22&type=Code) |
| SQL-tietokantaan | Kyllä | [SQL-tietokannan muille käyttäjille](https://msdn.microsoft.com/library/azure/mt163571.aspx) | [2014-04-01-esikatselu](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) | [Microsoft.Sql](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Sql%22&type=Code) |
| SQL-tietovarasto | Kyllä |   |      |


## <a name="web--mobile"></a>Web- ja Mobile

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------ |
| API-sovellukset | Kyllä |   | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [API-sovellukset](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22kind%22%3A+%22apiApp%22&type=Code) |
| API hallinta | Kyllä  | [API hallinnan muille käyttäjille](https://msdn.microsoft.com/library/azure/dn776326.aspx) | [2016-07-07](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-07-07/Microsoft.ApiManagement.json)       | [Microsoft.ApiManagement](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ApiManagement%22&type=Code) | 
| Sisällön valvoja | Kyllä |   |   |   |
| Funktio-sovellus | Kyllä |   |   | [functionApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22functionApp%22&type=Code) |
| Logiikan sovellukset | Kyllä   | [Työnkulun palvelun REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/mt643787.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.Logic.json) | [Microsoft.Logic](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Logic%22&type=Code) |
| Mobile-sovellukset | Kyllä |  | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json)  | [mobileApp](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22mobileApp%22&type=Code)   |
| Mobiili sitoumukset | Kyllä  |  [Mobiili välitys muille käyttäjille](https://msdn.microsoft.com/library/azure/mt683754.aspx)        |        | [Microsoft.MobileEngagements](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.MobileEngagement%22&type=Code) |
| Haku | Kyllä  | [Haun REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  | [Microsoft.Search](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Search%22&type=Code) |
| Web Apps-sovelluksista | Kyllä |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) | [Microsoft.Web](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Web%22&type=Code) |

## <a name="analytics"></a>Analytics

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | -------  | -------- | ------ | ------ |
| Tietoluettelo | Kyllä  | [Tietoluettelo muille käyttäjille](https://msdn.microsoft.com/library/azure/mt267595.aspx)  | [2016-03-30](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-30/Microsoft.DataCatalog.json) |  |
| Tietoja Factory | Kyllä | [Tietoja Factory muille käyttäjille](https://msdn.microsoft.com/library/azure/dn906738.aspx) |    | [Microsoft.DataFactory](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataFactory%22&type=Code) |
| Tietoja järvi Analytics | Kyllä |   |   [2015-10-01-esikatselu](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeAnalytics](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeAnalytics%22&type=Code) |
| Järvi tietosäilö | Kyllä  | [Tietosäilö järvi muille käyttäjille](https://msdn.microsoft.com/library/azure/mt693424.aspx) | [2015-10-01-esikatselu](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01-preview/Microsoft.DataLakeAnalytics.json) | [Microsoft.DataLakeStore](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DataLakeStore%22&type=Code) |
| HDInsights | Kyllä | [HDInsights muille käyttäjille](https://msdn.microsoft.com/library/azure/mt622197.aspx) |        | [Microsoft.HDInsight](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.HDInsight%22&type=Code) |
| Koneen oppiminen | Kyllä | [Konepohjaisten Oppimistekniikoiden muille käyttäjille](https://msdn.microsoft.com/library/azure/mt767538.aspx)        | [2016-05-01-esikatselu](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-01-preview/Microsoft.MachineLearning.json) |
| Virta Analytics | Kyllä  | [Höyryn Analytics muille käyttäjille](https://msdn.microsoft.com/library/azure/dn835031.aspx)   |        |  |
| Power BI | Kyllä | [Power BI upotettu muille käyttäjille](https://msdn.microsoft.com/library/azure/mt712303.aspx) | [2016-01 – 29](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-01-29/Microsoft.PowerBI.json) |  |

## <a name="intelligence"></a>Liiketoimintatietojen

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------ |
| Kognitiiviset palvelut | Kyllä |  | [2016-02-01-esikatselu](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-01-preview/Microsoft.CognitiveServices.json) |   |

## <a name="internet-of-things"></a>Internet-toimintoihin

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------ |
| Tapahtuma-toiminnossa | Kyllä  | [Tapahtuma-toiminnossa muille käyttäjille](https://msdn.microsoft.com/library/azure/dn790674.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.EventHub.json) | [Microsoft.EventHub](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.EventHub%22&type=Code)  |
| IoTHubs | Kyllä | [IoT keskittimeen muille käyttäjille](https://msdn.microsoft.com/library/azure/mt589014.aspx) | [2016-02-03](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-02-03/Microsoft.Devices.json) | [Microsoft.Devices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Devices%22&type=Code) |
| Ilmoitus keskittimet | Kyllä | [Ilmoitus-toiminnossa muille käyttäjille](https://msdn.microsoft.com/library/azure/dn495827.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) | [Microsoft.NotificationHubs](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.NotificationHubs%22&type=Code) |

## <a name="media--cdn"></a>Media ja CDN

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------ |
| CDN | Kyllä | [CDN MUILLE KÄYTTÄJILLE](https://msdn.microsoft.com/library/azure/mt634456.aspx)  | [2016-04-02](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-04-02/Microsoft.Cdn.json) | [Microsoft.Cdn](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Cdn%22&type=Code) |
| Mediapalvelu | Kyllä | [Media-palveluiden muille käyttäjille](https://msdn.microsoft.com/library/azure/hh973617.aspx)  | [2015-10-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-01/Microsoft.Media.json) |


## <a name="hybrid-integration"></a>Hybrid integrointi

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------ |
| BizTalk palvelut | Kyllä |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |  |
| Palvelun palautus | Kyllä | [Sivuston palauttaminen muille käyttäjille](https://msdn.microsoft.com/library/azure/mt750497.aspx) | [2016-06-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-06-01/Microsoft.RecoveryServices.json) | [Microsoft.RecoveryServices](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.RecoveryServices%22&type=Code) |
| Palvelun Bus | Kyllä | [Palvelun Bus muille käyttäjille](https://msdn.microsoft.com/library/azure/mt639375.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.ServiceBus.json)       | [Microsoft.ServiceBus](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.ServiceBus%22&type=Code) |

## <a name="identity--access-management"></a>Käyttäjätietojen ja käyttöoikeushallinta 

Azure Active Directory toimii ja resurssin Manager Roolipohjainen käyttöoikeuksien valvonta tilauksen käyttöön. Lisätietoja Roolipohjainen käyttöoikeuksien valvonta ja Active Directoryn käyttämisestä on artikkelissa [Azure Roolipohjainen käyttöoikeuksien valvonta](./active-directory/role-based-access-control-configure.md).

## <a name="developer-services"></a>Kehitystyökalut-palvelut 

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------ |
| Hakemuksen tiedot | Kyllä  | [MUUT tiedot](https://msdn.microsoft.com/library/azure/dn931943.aspx)  | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) | [Microsoft.insights](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.insights%22&type=Code) |
| Bing Maps | Kyllä  |          |        |  |
| DevTest harjoituksia | Kyllä |   | [2016-05-15](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-05-15/Microsoft.DevTestLab.json) | [Microsoft.DevTestLab](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.DevTestLab%22&type=Code)  |
| Visual Studio-tili | Kyllä   |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |  |

## <a name="management-and-security"></a>Hallinta ja suojaus

| Palvelun | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------- | ------ | ------ |
| Automaatio | Kyllä | [Automaatio muille käyttäjille](https://msdn.microsoft.com/library/azure/mt662285.aspx) | [2015-10 – 31](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-10-31/Microsoft.Automation.json)       | [Microsoft.Automation](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Automation%22&type=Code) |
| Avaimen säilö | Kyllä | [Avaimen säilö muille käyttäjille](https://msdn.microsoft.com/library/azure/dn903609.aspx) | [Avaimen säilö](resource-manager-template-keyvault.md)<br />[Avaimen säilö salaisuus](resource-manager-template-keyvault-secret.md)   | [Microsoft.KeyVault](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.KeyVault%22&type=Code) |
| Toiminnalliset tiedot | Kyllä |          |        |  |
| Ajoitus | Kyllä  | [Ajoituksen muille käyttäjille](https://msdn.microsoft.com/library/azure/mt629143.aspx)     | [2016-03-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2016-03-01/Microsoft.Scheduler.json) | [Microsoft.Scheduler](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Scheduler%22&type=Code) |
| Suojauksen (ennakkoversio) | Kyllä | [Suojauksen muille käyttäjille](https://msdn.microsoft.com/library/azure/mt704034.aspx)  |  | [Microsoft.Security](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Security%22&type=Code)  |

## <a name="resource-manager"></a>Resurssien hallinta

| Toiminto | Resurssien hallinta on otettu käyttöön | REST-OHJELMOINTIRAJAPINNALLA | Rakenne | Pikaopas-mallit |
| ------- | ------- | -------------- | -------- | ------ | ------ |
| Todennus | Kyllä | [Lukitusten hallinta](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[Roolipohjainen käyttöoikeuksien valvonta](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [Resurssin lukitus](resource-manager-template-lock.md)<br />[Roolimäärityksiä](resource-manager-template-role.md)  | [Microsoft.Authorization](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Authorization%22&type=Code) |
| Resurssit | Kyllä | [Linkitetyn resurssit](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [Resurssin linkit](resource-manager-template-links.md) | [Microsoft.Resources](https://github.com/Azure/azure-quickstart-templates/search?utf8=%E2%9C%93&q=%22Microsoft.Resources%22&type=Code) |


## <a name="resource-providers-and-types"></a>Resurssin palveluntarjoajat ja tyypit

Kun otat resursseja, sinun on usein hakea tietoja resurssin tarjoajat ja tyypit. Voit noutaa tiedot REST API, PowerShellin Azure ja Azure CLI.

Toimimaan resurssi-palvelussa resurssin palveluntarjoajaan rekisteröitävä tilisi. Monet resurssin palveluntarjoajat automaattisesti rekisteröity oletusarvoisesti; kuitenkin haluat rekisteröidä manuaalisesti resurssin jotkin palveluntarjoajat. Seuraavissa esimerkeissä näyttää, miten resurssin palveluntarjoaja rekisteröinti tilan ja rekisteröintiä resurssi-palveluntarjoajan tarvittaessa.

### <a name="portal"></a>Portal

Näet helposti luettelo tuetuista resurssien tarjoajat seuraavasti:

1. Valitse **omat käyttöoikeudet**.

    ![Omat käyttöoikeudet](./media/resource-manager-supported-services/my-permissions.png)

2. Valitse **Resurssin palvelun tila**

    ![Resurssin palvelun tila](./media/resource-manager-supported-services/resource-provider-status.png)

3. Tilauksen Katso luettelo kaikista resurssin tarjoajat.

    ![luettelon resurssien tarjoajat](./media/resource-manager-supported-services/list-providers.png)

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

Saat kaikki käytettävissä olevat resurssin tarjoajat, mukaan lukien niiden sekä sijainnit, API-versiot ja rekisteröinnin tila, käytetään [kaikkien resurssien palveluntarjoajien luettelo](https://msdn.microsoft.com/library/azure/dn790524.aspx) -toimintoa. Jos haluat rekisteröidä resurssin toimittajan, katso [rekisteröidä tilauksen resurssi-palvelussa](https://msdn.microsoft.com/library/azure/dn790548.aspx).

### <a name="powershell"></a>PowerShellin

Seuraavassa esimerkissä esitetään, miten saat kaikki käytettävissä olevat resurssit-palveluista.

    Get-AzureRmResourceProvider -ListAvailable
    

Seuraavassa esimerkissä näkyy hankkiminen resurssityyppejä tietyn resurssin palveluntarjoajasi.

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes
    
Tulos on:

    ResourceTypeName                Locations                                         
    ----------------                ---------                                         
    sites/extensions                {Brazil South, East Asia, East US, Japan East...} 
    sites/slots/extensions          {Brazil South, East Asia, East US, Japan East...} .
    ...
    
Voit rekisteröidä resurssin palveluntarjoaja, anna nimitila:

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ApiManagement

### <a name="azure-cli"></a>Azure CLI

Seuraavassa esimerkissä esitetään, miten saat kaikki käytettävissä olevat resurssit-palveluista.

    azure provider list
    
Tulos on:

    info:    Executing command provider list
    + Getting registered providers
    data:    Namespace                        Registered
    data:    -------------------------------  -------------
    data:    Microsoft.ApiManagement          Unregistered
    data:    Microsoft.AppService             Registered
    data:    Microsoft.Authorization          Registered
    ...

Voit tallentaa tiedoston seuraavalla komennolla tietyn resurssin tarjoajan tiedot.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Voit rekisteröidä resurssin palveluntarjoaja, anna nimitila:

    azure provider register -n Microsoft.ServiceBus

## <a name="supported-regions"></a>Tuettujen alueiden

Kun otat resursseja, haluat yleensä Määritä resurssien alue. Resurssienhallinta tueta kaikkien alueiden, mutta käyttöönottoa resurssit eivät välttämättä tue kaikilla alueilla. Lisäksi voi olla rajoituksia tilauksen, joka estää joidenkin alueiden, jotka tukevat resurssin käytön. Näiden rajoitusten voi liittyä ALV-ongelmat koti maasi tai käytännön saatu sijoittaa käyttämään vain tiettyjä alueita tilauksen järjestelmänvalvoja. 

Katso luettelo kaikista kaikkien tuettujen alueiden kaikissa Azure-palveluissa, [Services alueittain](https://azure.microsoft.com/regions/#services) Tämän luettelon voi sisältyä kuitenkin alueiden tilauksesi tue. Voit määrittää tietyn Resurssityyppi, joka tilauksen tukee portal, REST API, PowerShell ja Azure CLI alueiden.

### <a name="portal"></a>Portal

Näet tuettujen alueiden resurssityyppi seuraavien vaiheiden läpi:

1. Valitse **Lisää palveluja** > **Resurssin Explorer**.

    ![resurssin explorer](./media/resource-manager-supported-services/select-resource-explorer.png)

2. Avaa **palvelut** -solmu.

    ![Valitse palvelut](./media/resource-manager-supported-services/select-providers.png)

3. Valitse resurssin toimittaja ja tuettujen alueiden ja API-versioiden tarkasteleminen.

    ![näkymän tarjoajaa](./media/resource-manager-supported-services/view-provider.png)

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

Saat tietää, mitkä alueet ovat käytettävissä tiettyyn resurssityyppi tilaukseesi, käytetään [kaikkien resurssien palveluntarjoajien luettelo](https://msdn.microsoft.com/library/azure/dn790524.aspx) -toimintoa. 

### <a name="powershell"></a>PowerShellin

Seuraavassa esimerkissä esitetään hankkiminen tuettujen alueiden verkkosivustojen.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
Tulos on:

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

### <a name="azure-cli"></a>Azure CLI

Seuraava esimerkki palauttaa kaikki resurssin mistäkin tuetut sijainneista.

    azure location list

Voit myös suodattaa JSON-apuohjelmalla, kuten [jq](https://stedolan.github.io/jq/)sijainti-tulokset.

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

Joka palauttaa:

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## <a name="supported-api-versions"></a>Tuetut API-versiot

Kun otat mallin käyttöön, sinun on määritettävä API-versio, jonka avulla voi luoda kullekin resurssille. REST API-toiminnot, jotka on julkaistu resurssin tarjoaja versioon vastaa API-versio. Kun resurssi-palvelun avulla uusia ominaisuuksia, versiot REST-Ohjelmointirajapinnalla uuden version. Tämän vuoksi mallin määritettyyn API-version vaikuttaa voit määrittää mallin ominaisuudet. Yleensä haluat valita API uusimman version malleja luotaessa. Mallia voit päättää, onko haluat jatkaa API aiempaa versiota käyttävän tai päivittää mallia, uusimpaan versioon, jotta voit hyödyntää uusia ominaisuuksia.

### <a name="portal"></a>Portal

Voit määrittää tuetut API-versiot samalla tavalla, voit määrittää tuetut alueet (kuten aiemmin).

### <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA

Saat tietää, mitkä API-versiot ovat saatavana resurssityypit, käytetään [kaikkien resurssien palveluntarjoajien luettelo](https://msdn.microsoft.com/library/azure/dn790524.aspx) -toimintoa. 

### <a name="powershell"></a>PowerShellin

Seuraavassa esimerkissä esitetään, miten saat käytettävissä API-versioiden tietyn resurssilaji.

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
Tulos on:
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

### <a name="azure-cli"></a>Azure CLI

Voit tallentaa tiedot (mukaan lukien käytettävissä API-versiot) resurssi-palveluntarjoajasi seuraavalla komennolla tiedostoon.

    azure provider show Microsoft.Web -vv --json > c:\temp.json

Voit avata tiedoston ja Etsi **apiVersions** -elementti

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja Resurssienhallinta-mallien luomisesta on artikkelissa [yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](resource-group-authoring-templates.md).
- Tietoja resurssien käyttöönotto-kohdassa [käyttöönotto sovelluksen Azure Resurssienhallinta-malli](resource-group-template-deploy.md).
