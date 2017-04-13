<properties
    pageTitle="Mitä on tapahtunut WebJob projektin (Visual Studio Azuren tallennustilaan yhteydessä service)? | Microsoft Azure"
    description="Tässä artikkelissa kuvataan, mitä tapahtui Azure WebJob projektin jälkeen yhteyden Visual Studiossa tallennustilan tilin yhdistetyt palvelut"
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

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Mitä on tapahtunut WebJob projektin (Visual Studio Azuren tallennustilaan yhteydessä service)?

## <a name="references-added"></a>Viittauksia, jotka on lisätty

Azure-tallennustilan NuGet package on lisätty tai päivitetty Visual Studio projektin.  
Tämän paketin Lisää seuraavat .NET-viittaukset:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Azure-tallennustilan lisätty yhteysmerkkijonon
Projektin App.config-tiedostoa **AzureWebJobsStorage** ja **AzureWebJobsDashboard** tapahtumat on päivitetty valitun tallennustilan tilin yhteysmerkkijonon ja avaimen avulla.

Lisätietoja on artikkelissa [Azure WebJobs dokumentaatioresurssit](http://go.microsoft.com/fwlink/?linkid=390226).
