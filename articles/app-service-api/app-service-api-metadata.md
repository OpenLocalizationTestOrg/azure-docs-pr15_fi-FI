<properties
    pageTitle="Sovelluksen Service API sovellusten metatietojen API etsiminen ja koodin luontia varten | Microsoft Azure"
    description="Tietoja siitä, miten Azure App Service API-sovellusten Swagger metatietojen API etsiminen ja koodin luonti helpottamiseksi."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Sovelluksen Service API sovellusten metatietojen API etsiminen ja koodin luontia varten 

[Swagger 2.0](http://swagger.io/) API metatietojen tuki on valmiina Service API sovellukset. Sinun ei tarvitse käyttää Swagger, mutta jos käytät sitä, voit hyödyntää API-sovellusten ominaisuuksia, jotka helpottavat etsiminen ja kulutus.   

## <a name="swagger-endpoint"></a>Päätepisteen swagger

Voit määrittää päätepisteen, joka sisältää Swagger 2.0 JSON metatietojen ominaisuuden API-sovelluksen API-sovelluksen. Päätepisteen voi olla suhteessa Perusosoitteen API-sovelluksen tai absoluuttinen URL-osoite. Absoluuttinen URL-osoitteet voit osoittaa API-sovelluksen ulkopuolella. 

Monta edeltävät asiakkaat (esimerkiksi Visual Studio-koodin luominen ja PowerApps "Lisää API" tiedonkulun), URL-osoite on oltava julkinen (ei ole suojattu käyttäjän tai todentaminen). Tämä tarkoittaa, että jos App palvelun todennus on käytössä ja haluat näyttää Ohjelmointirajapinnan määritelmän mistä sovelluksen itse, sinun on käytettävä todennusta-asetus, joka sallii anonyymin liikenne oman API saavuttamiseksi. Lisätietoja on artikkelissa [todennus- ja Service API sovellukset varten](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portaalin sivu

[Azure portal](https://portal.azure.com/) päätepisteen URL-osoite voit nähdä ja muuttaa **API määritys** -sivu.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure Resurssienhallinta-ominaisuus

Voit myös määrittää [Resurssin Explorer](https://resources.azure.com/) tai [Azure Resurssienhallinta mallien](../resource-group-authoring-templates.md) avulla komentoriviltä työkaluilla, kuten [PowerShellin Azure](../powershell-install-configure.md) ja [Azure CLI](../xplat-cli-install.md)API-sovelluksen API määritelmä URL-osoite. 

**Resurssin Explorerissa**Siirry **tilaukset > {tilauksen} > resourceGroups > {resurssiryhmän} > tarjoajat > Microsoft.Web > sivustot > {sivuston} > config > web**, ja näet `apiDefinition` ominaisuus:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Esimerkki Azure Resurssienhallinta-malli, joka määrittää `apiDefinition` ominaisuutta, Avaa [azuredeploy.json tiedoston sovelluksen tehtäväluettelo-malli](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Etsi malli, joka näyttää yllä JSON-malli-osio.

### <a name="default-value"></a>Oletusarvo

Kun käytät Visual Studio API-sovelluksen luominen, API määritelmä päätepisteen määritetään automaattisesti Perusosoitteen plus API-sovelluksen `/swagger/docs/v1`. Tämä on oletusarvoinen URL-osoite, jonka avulla [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet paketin yhteyshenkilönä dynaamisesti luodun ASP.NET-verkko-Ohjelmointirajapinnan projektin Swagger metatiedot. 

## <a name="code-generation"></a>Koodin luominen

Yksi edut Swagger integroiminen Azure API-sovellukset on automaattinen koodin luominen. Luotua Luokat helpottavat kirjoittaa koodia, soittaa API-sovelluksen.

Voit luoda asiakas-koodia API-sovelluksen Visual Studiossa tai komentoriviltä. Lisätietoja siitä, miten voit luoda asiakkaan luokat Visual Studiossa ASP.NET-verkko-Ohjelmointirajapinnan projektin on artikkelissa [API-sovellusten ja ASP.NET käytön aloittaminen](app-service-api-dotnet-get-started.md#codegen). Lisätietoja tehtävän suorittaminen komentoriviltä tuetuilla kielillä näy GitHub.com [Azure/autorest](https://github.com/azure/autorest) tietovaraston Lueminut-tiedosto.
 
## <a name="next-steps"></a>Seuraavat vaiheet

Vaiheittaiset ohjeet opetusohjelmaan, joka opastaa luominen käyttöönotosta ja muissa API-sovelluksen artikkelissa [Azure App Service API-sovellusten käytön aloittaminen](app-service-api-dotnet-get-started.md).

Jos käytät Azure API hallinnan API-sovelluksilla, voit tuoda oman API API hallinta Swagger metatiedot. Lisätietoja on artikkelissa [tuominen Ohjelmointirajapinnan työvaiheita Azure API hallinnan määritys](../api-management/api-management-howto-import-api.md). 
