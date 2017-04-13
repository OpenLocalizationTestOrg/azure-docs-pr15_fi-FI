<properties
    pageTitle="Mitä tapahtui ASP.NET projektin? | Microsoft Azure | Visual Studio yhdistetyt palvelut"
    description="Tässä artikkelissa kuvataan, mitä tapahtuu, kun Azure-tallennustilan lisääminen Visual Studiossa ASP.NET projektin yhdistetyt palvelut"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Mitä on tapahtunut ASP.NET projektin (Visual Studio Azuren tallennustilaan yhteydessä service)?

## <a name="references-added"></a>Viittauksia, jotka on lisätty

Azure-tallennustilan NuGet paketin on lisätty projektiin Visual Studio.  
Tämän paketin Lisää seuraavat .NET-viittaukset:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

##<a name="connection-string-for-azure-storage-added"></a>Azure-tallennustilan lisätty yhteysmerkkijonon
Seuraavan koodin korostetut projektista elementti on luotu valitun tallennustilan tilin yhteysmerkkijonon ja avaimen avulla.

Lisätietoja on artikkelissa [ASP.NET](http://www.asp.net).
