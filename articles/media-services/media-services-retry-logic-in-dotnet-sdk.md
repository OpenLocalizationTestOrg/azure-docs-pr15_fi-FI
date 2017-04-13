<properties
    pageTitle="Yritä uudelleen Media Services SDK funktioiden .NET | Microsoft Azure"
    description="Aiheen antaa Media Services SDK yleiskatsaus uudelleen logiikan .NET."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Yritä Media Services SDK funktioiden .NET varten

Lyhytkestoisia virheitä voi ilmetä, kun käsittelet Microsoft Azure-palvelut. Jos lyhytkestoisia virheen ilmetessä useimmissa tapauksissa muutaman yrityksistä, toiminto onnistuu. .NET Media-palveluiden SDK toteuttaa uudelleen logiikan käsittelemään lyhytkestoisia poikkeukset ja virheet, jotka aiheutuvat web pyynnöt liittyvät virheet suoritettaessa kyselyjä, tallentaa muutokset ja tallennustilaa toimintoja.  Oletusarvon mukaan .NET Media-palveluiden SDK suorittaa neljä uudelleenyritykset, ennen kuin järjestelmä uudelleen palauttaa poikkeuksen sovelluksen. Koodi-sovelluksessa on sitten käsiteltävä tätä poikkeusta oikein.  
  
 Seuraavassa on lyhyt ohjeena Web-pyyntö, varasto, kysely ja SaveChanges käytännöt:  
  
-   Tallennustilan käytännön käytetään blob storage toiminnot (lataukset tai resurssi tiedostojen lataaminen).  
  
-   Web-pyyntö käytännön käytetään yleisen web-pyynnöt (esimerkiksi käytön todennus-tunnuksen ja ratkaiseminen käyttäjien klusterin päätepisteen).  
  
-   Kyselyn käytännön käytetään kyselyt osallistujina REST (esimerkiksi mediaContext.Assets.Where(...)).  
  
-   SaveChanges käytännön käytetään mitään, joka muuttuu (esimerkiksi luominen kohteen päivittäminen kohteen kutsumista service-funktioon toiminnon) palvelun tiedot.  
  
 Tässä ohjeaiheessa esitellään poikkeuksen ja virhekoodit, jotka ovat käsittelemien .NET Media Services SDK uudelleen logiikan.  
  
## <a name="exception-types"></a>Poikkeus tyypit  

Seuraavassa taulukossa on kuvattu poikkeukset, .NET Media-palveluiden SDK käsittelee tai ei käsittele joitakin toimintoja, jotka saattavat aiheuttaa lyhytkestoisia virheitä.  
  
Poikkeus|WWW-pyyntö|Tallennustilan|Kyselyn|SaveChanges
----|------|----|---|---
WebException<br/>Lisätietoja on kohdassa [WebException tilakoodin](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Kyllä|Kyllä|Kyllä|Kyllä  
DataServiceClientException<br/> Lisätietoja on artikkelissa [HTTP virhekoodit tila](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ei|Kyllä|Kyllä|Kyllä
DataServiceQueryException<br/> Lisätietoja on artikkelissa [HTTP virhekoodit tila](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ei|Kyllä|Kyllä|Kyllä  
DataServiceRequestException<br/> Lisätietoja on artikkelissa [HTTP virhekoodit tila](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ei|Kyllä|Kyllä|Kyllä  
DataServiceTransportException|Ei|Ei|Kyllä|Kyllä
TimeoutException|Kyllä|Kyllä|Kyllä|Ei
SocketException|Kyllä|Kyllä|Kyllä|Kyllä  
StorageException|Ei|Kyllä|Ei|Ei 
IOException|Ei|Kyllä|Ei|Ei
  
###  <a name="WebExceptionStatus"></a>WebException tilakoodin  

Seuraavassa taulukossa on WebException virhekoodit sovelletaan uudelleen logiikan. [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) luettelointi määrittää tila.  
  
Tila|WWW-pyyntö|Tallennustilan|Kyselyn|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Kyllä|Kyllä|Kyllä|Kyllä
NameResolutionFailure|Kyllä|Kyllä|Kyllä|Kyllä  
ProxyNameResolutionFailure|Kyllä|Kyllä|Kyllä|Kyllä  
SendFailure|Kyllä|Kyllä|Kyllä|Kyllä
PipelineFailure|Kyllä|Kyllä|Kyllä|Ei  
ConnectionClosed|Kyllä|Kyllä|Kyllä|Ei  
KeepAliveFailure|Kyllä|Kyllä|Kyllä|Ei  
UnknownError|Kyllä|Kyllä|Kyllä|Ei 
ReceiveFailure|Kyllä|Kyllä|Kyllä|Ei  
RequestCanceled|Kyllä|Kyllä|Kyllä|Ei  
Aikakatkaisu|Kyllä|Kyllä|Kyllä|Ei
Protokollavirhe <br/>Valitse protokollavirhe uudelleen hallitsee HTTP tila koodin käsittely. Lisätietoja on artikkelissa [HTTP virhekoodit tila](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Kyllä|Kyllä|Kyllä|Kyllä|  
  
###  <a name="HTTPStatusCode"></a>HTTP-virhekoodit tila  

Kun kysely ja SaveChanges palauttaa DataServiceClientException, DataServiceQueryException tai DataServiceQueryException, HTTP virhekoodi palautetaan StatusCode-ominaisuus.  Seuraavassa taulukossa on virhekoodit sovelletaan uudelleen logiikan.  
  
 
Tila|WWW-pyyntö|Tallennustilan|Kyselyn|SaveChanges 
---|----|----|----|----
401|Ei|Kyllä|Ei|Ei
403|Ei|Kyllä<br/>Käsittely uudelleenyritykset pidempi odottaa kanssa.|Ei|Ei  
408|Kyllä|Kyllä|Kyllä|Kyllä
429|Kyllä|Kyllä|Kyllä|Kyllä  
500|Kyllä|Kyllä|Kyllä|Ei  
502|Kyllä|Kyllä|Kyllä|Ei  
503|Kyllä|Kyllä|Kyllä|Kyllä  
504|Kyllä|Kyllä|Kyllä|Ei  
  
Jos haluat poistaa .NET Media Services SDK todellinen soveltaminen katsaus uudelleen logiikan on artikkelissa [azure sdk-varten-media-palvelut](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
