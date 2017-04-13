<properties
   pageTitle="Logiikan sovellusten esimerkkejä ja skenaarioiden | Microsoft Azure"
   description="Esimerkkejä yleisten logiikan sovellukset ja lue yleisiä tilanteita, joissa ottamisesta käyttöön"
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

# <a name="logic-apps-examples-and-common-scenarios"></a>Logiikan sovellusten esimerkkejä ja yleisiä tilanteita, joissa

Tämän asiakirjan tiedot yleisiä tilanteita, joissa ja esimerkkejä, jotka auttavat ymmärtämään joitakin tapoja, joilla voit käyttää logiikan sovellusten liiketoimintaprosessien automatisointiin. 

## <a name="custom-triggers-and-actions"></a>Mukautetun Käynnistimet ja toiminnot

Voit käynnistää logiikan-sovelluksen toisesta sovelluksesta useilla tavoilla. Seuraavassa on muutamia yleisiä esimerkkejä:

- [Luomalla mukautettuja käynnistimen tai toiminto](app-service-logic-create-api-app.md)
- [Pitkään suoritettavien toiminnot](app-service-logic-create-api-app.md)
- [HTTP-pyyntö käynnistin (JULKAISE)](app-service-logic-http-endpoint.md)
- [Webhook Käynnistimet ja toiminnot](app-service-logic-create-api-app.md)
- [Kysely Käynnistimet](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Skenaariot

- [Synkronoitu vastaus](app-service-logic-http-endpoint.md)
- [Vastaus SMS: N kanssa](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Virheenkäsittely ja kirjaaminen

- [Poikkeus ja Virheenkäsittely](app-service-logic-exception-handling.md)
- [Azure-ilmoitukset ja diagnostiikan asetusten määrittäminen](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Skenaariot

- [Käyttötapaus: Virhe ja poikkeuksen käsittely](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Käyttöönotto ja hallinta

- [Luo automaattinen käyttöönotto](app-service-logic-create-deploy-template.md)
- [Muodosta ja ota käyttöön Visual Studio sovelluksia logiikka](app-service-logic-deploy-from-vs.md)
- [Logiikan sovellusten valvonta](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Sisältötyyppien ja muunnoksia muunnokset

Logiikan sovellusten [työnkulun definition language](http://aka.ms/logicappsdocs) sisältää monia funktioita, jotta voit muuntaa ja käyttää erilaisia sisältötyyppejä.  Lisäksi moduulin tällöin toimi kaikki se voidaan säilyttää mahdollisimman tietojen kulkee työnkulussa sisältötyypit.

- Kuten sovelluksen/json ja sovelluksen/xml [tekstimuotoista/sisältötyypit käsittely](app-service-logic-content-type.md)
- [Työnkulun määritelmien luominen](app-service-logic-author-definitions.md)
- [Työnkulun definition language viittaus](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Erissä ja toistuminen

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Ennen kuin](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Ja Azure Funktiot

- [Azure Funktiot-integrointi](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Skenaariot

- [Palvelun Bus käynnistimen Azure funktioon](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>HTTP ja muut SOAP

 - [Soittavan SOAP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Microsoft lisäisit esimerkkejä ja skenaarioiden tämän asiakirjan. Kommentit-kohtaan avulla voit kertoa meille, mitkä esimerkkejä tai tilanteita, joissa haluat on täällä.