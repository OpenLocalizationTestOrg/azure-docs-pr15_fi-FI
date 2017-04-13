<properties
    pageTitle="Mitä pilvi palvelun projektin on tapahtunut? | Microsoft Azure | Visual Studio yhdistetyt palvelut"
    description="Tässä artikkelissa kuvataan, mitä tapahtuu cloud services-projekti, kun Visual Studiossa Azure-tallennustilan tilin muodostamisesta yhdistetyt palvelut"
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

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Mitä on tapahtunut cloud services projektin (Visual Studio Azuren tallennustilaan yhteydessä service)?

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

## <a name="connection-string-for-azure-storage-added"></a>Azure-tallennustilan lisätty yhteysmerkkijonon
Osat on luotu valitun tallennustilan tilin yhteysmerkkijonon ja avaimen avulla. Muutokset on tehty seuraavat tiedostot:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
