<properties
    pageTitle="Mitä on tapahtunut ASP.NET-5 projektin (Visual Studio yhteydessä services) | Microsoft Azuren tallennustilaan"
    description="Tässä artikkelissa kuvataan, mitä tapahtuu, kun Visual Studiossa Visual Studio ASP.NET 5 projektin Azure-tallennustilan tilin muodostamisesta yhdistetyt palvelut"
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Mitä on tapahtunut ASP.NET-5 projektin (Visual Studio Azuren tallennustilaan yhteydessä services)?

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

Lisäksi NuGet paketin **Microsoft.Framework.Configuration.Json** on lisätty.

## <a name="connection-string-for-azure-storage-added"></a>Azure-tallennustilan lisätty yhteysmerkkijonon
Projektin config.json-tiedostossa elementti on luotu valitun tallennustilan tilin yhteysmerkkijonon ja avaimen avulla.

Lisätietoja on artikkelissa [ASP.NET-5](http://www.asp.net/vnext).
