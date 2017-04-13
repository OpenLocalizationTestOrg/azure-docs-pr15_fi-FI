<properties 
    pageTitle="Voit määrittää tietokoneen for Media Services .NET kehitys" 
    description="Lisätietoja Media-palveluita käyttämällä Media Services SDK .NET edellytyksistä. Tietoja myös Visual Studio-sovelluksen luominen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/24/2016"  
    ms.author="juliako"/>

#<a name="media-services-development-with-net"></a>Media-palveluiden kehitys .NET

[AZURE.INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

Tässä ohjeaiheessa kerrotaan, miten voit käynnistää käyttämällä .NET Media Services-sovellusten kehittämiseen.

**Azure Media Services.NET SDK** -kirjaston mahdollistaa ohjelman Media-palveluita käyttämällä .NET vastaan. Varmista entistä helpompaa kehittää kanssa .NET **Azure Media Services .NET SDK-laajennukset** -kirjasto on annettu. Tämä kirjasto sisältää tunniste menetelmien ja avustaja-toiminnot, jotka selkeyttää .NET-koodi. Sekä kirjastojen ovat käytettävissä **NuGet** ja **GitHub**kautta.


##<a name="prerequisites"></a>Edellytykset

-   Media Services-tilin, valitse uuteen tai aiemmin luotuun Azure tilauksen. Aiheessa [Media Services-tilin luominen](media-services-portal-create-account.md).
-   Käyttöjärjestelmät: Windows 10, Windows 7: ssä, Windows 2008 R2 tai Windows 8: ssa.
-   .NET framework 4.5.
-    Visual Studio 2015, Visual Studio 2013, Visual Studio 2012 tai Visual Studio 2010 SP1 (Professional, Premium, Ultimate tai Express).


##<a name="create-and-configure-a-visual-studio-project"></a>Luo ja määritä Visual Studio-projekti

Tässä osassa näytetään, miten voit luoda projektin Visual Studio ja se määritetään Media Services kehittäminen.  Tässä tapauksessa projekti on C# Windows console-sovelluksen, mutta saman määritysvaiheiden kuvassa koskevat myös muun tyyppisiä projekteja, voit luoda Media Services-sovellusten (esimerkiksi Windows-lomakkeet-sovelluksen tai ASP.NET Web-sovelluksen).

Tässä osassa esitetään, kuinka voit lisätä Media Services .NET SDK ja riippuvainen kirjastojen **NuGet** käyttämällä.

Vaihtoehtoisesti voit saada uusimman Media Services .NET SDK bittien GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) ja [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), Rakenna ratkaisu ja lisätä asiakkaan projektin viittaukset. Huomaa, että kaikki tarvittavat riippuvuudet ladataan ja luoda automaattisesti.

1. Luo uuden C# Console-sovelluksen Visual Studio 2010 SP1 tai uudempi ja versio. Kirjoita **nimi**, **sijainti**ja **ratkaisun nimeä**ja valitse sitten OK.

2. Rakenna ratkaisu.

2. **NuGet** avulla voit asentaa ja lisätä **Azure Media Services .NET SDK-laajennusten**. Asennuksessa, asentaa **Media Services.NET SDK** myös ja Lisää kaikki tarvittavat riippuvuudet.

    Varmista, että sinulla on asennettu NuGet uusin versio. Saat enemmän tietoja ja asennusohjeet [NuGet](http://nuget.codeplex.com/).

2. Napsauta ratkaisunhallinnassa projektin nimeä hiiren kakkospainikkeella ja valitse Hallitse NuGet pakettien...

    NuGet pakettien hallinta-valintaikkuna.

3. Online-valikoiman Azure MediaServices tunnisteet hakea, valitse Azure Media Services .NET SDK-laajennukset ja valitse sitten Asenna-painike.

    Projekti on muutettu ja viittauksia Media Services .NET SDK-laajennusten Media Services .NET SDK ja muiden riippuvaiset kokoonpanojen on lisätty.

4. Edistää selkeämpi kehitysympäristö, mieti, kannattaisiko ottaa NuGet paketin palauttaminen. Lisätietoja on artikkelissa [NuGet paketti palauttaa "](http://docs.nuget.org/consume/package-restore).

3. Lisää **System.Configuration** kokoonpanon viittaus. Tämän kokoonpanon sisältää System.Configuration. **ConfigurationManager** -luokka, jota käytetään tiedostojen määritys (esimerkiksi App.config).

    Lisää viitteet viittaukset hallinta-valintaikkunan Solution Explorerissa project-nimeä hiiren kakkospainikkeella. Valitse Lisää ja viittauksia.

    Hallitse viitteet-valintaikkuna tulee näyttöön.

4. .NET framework-kokoonpanot, valitse Etsi ja valitse System.Configuration kokoonpanon ja valitsemalla OK.
5. Avaa App.config-tiedosto (Lisää tiedoston projektiin, jos sitä ei lisätä oletusarvoisesti) ja *appSettings* -osan lisääminen tiedostoon.     
Määritä Azure Media Services nimi ja tilin tiliavain, kuten seuraavassa esimerkissä.

    Jos haluat etsiä nimen ja avaimen arvoja, siirry Azure-portaaliin ja valitse tilisi. Asetukset-ikkuna oikealla. Valitse asetukset-ikkunassa avaimet. Jokaisen tekstiruudun vieressä olevaa kuvaketta valitsemalla kopioi arvon järjestelmän Leikepöydälle.


        <configuration>
        ...
            <appSettings>
              <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
              <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
            </appSettings>

        </configuration>

6. Korvaa aiemmin **käyttäen** lausekkeita alussa Program.cs tiedoston seuraava koodi.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;

Tässä vaiheessa olet valmis aloittamaan Media Services-sovelluksen.    


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
