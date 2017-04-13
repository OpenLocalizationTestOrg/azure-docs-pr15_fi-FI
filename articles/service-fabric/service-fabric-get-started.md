<properties
   pageTitle="Kehitysympäristön määritys | Microsoft Azure"
   description="Asenna suorituksenaikainen, SDK ja työkalut ja luoda paikallisen kehittäminen klusterin. Kun olet suorittanut tämän asetuksen, sinun on valmis-sovelluksia."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Paikallinen ympäristö valmistellaan kehittäminen

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OS X](service-fabric-get-started-mac.md)

 Muodosta ja suorita [Azure palvelun kangasta sovellusten] [ 1] kehittäminen tietokoneeseen asentaa runtime, SDK ja työkalut. Voit myös on otettava käyttöön SDK sisältyy Windows PowerShell-komentosarjojen suorittamisen.

## <a name="prerequisites"></a>Edellytykset
### <a name="supported-operating-system-versions"></a>Tuetut käyttöjärjestelmien versiot
Seuraavien käyttöjärjestelmien tuetaan kehittämiseen:

- Windows 7: ssä
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10: ssä

>[AZURE.NOTE] Windows 7 sisältää vain Windows PowerShell 2.0 oletusarvoisesti. Palvelun kangasta PowerShellin cmdlet-komennot edellyttää PowerShell 3.0 tai uudempi versio. Voit [ladata Windows PowerShell 5.0] [ powershell5-download] Microsoft Download Centeristä.

## <a name="install-the-runtime-sdk-and-tools"></a>Asenna runtime, SDK ja työkalut

WWW-ympäristö asennusohjelma on kaksi palvelun kangasta kehittäminen määritykset:

- [Asentaa palvelun kangasta runtime, SDK ja tools for Visual Studio 2015 (edellyttää Visual Studio 2015 päivityksen 2 tai uudempi)][full-bundle-vs2015]
- [Asenna Service kangasta runtime ja vain SDK (ei ole Visual Studio työkalut)][core-sdk]

## <a name="enable-powershell-script-execution"></a>PowerShell-komentosarjojen suorittamisen käyttöön

Palvelun kangasta käyttää Windows PowerShell-komentosarjojen paikallista kehittämistä-klusterin luominen ja käyttöönotto Visual Studiossa sovelluksista. Windows estää oletusarvon mukaan nämä komentosarjojen suorittamisen. Voit ottaa ne käyttöön on muokattava PowerShell-suorittamisen käytännön. Avaa PowerShell-järjestelmänvalvojana ja kirjoita seuraava komento:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun asetus on valmis kehittäminen ympäristön luomiseen, Aloita luominen ja suorittaminen sovellukset.

- [Visual Studio ensimmäisen palvelun kangasta-sovelluksen luominen](service-fabric-create-your-first-application-in-visual-studio.md)
- [Opettele käyttöönotto ja paikallisen klusterin sovellusten hallinta](service-fabric-get-started-with-a-local-cluster.md)
- [Lisätietoja ohjelmoinnin mallien: luotettavia palveluja ja luotettava toimijoiden](service-fabric-choose-framework.md)
- [Tutustu palvelun kangasta MALLIKOODEJA-GitHub](https://aka.ms/servicefabricsamples)
- [Visualisoi yhteyttä klusterin palvelun kangasta Resurssienhallinnassa](service-fabric-visualizing-your-cluster.md)
- [Seuraa palvelun kangasta oppimispolkuun, ja katso platform laaja esittely](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Palvelun kangasta markkinointikampanjan sivu"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "JA RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "JA 2015 WebPI linkki"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI linkki"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Core SDK WebPI linkki"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
