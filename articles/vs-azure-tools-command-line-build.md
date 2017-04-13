<properties
   pageTitle="Komentorivin muodosta, Azure | Microsoft Azure"
   description="Komentorivin muodosta Azure varten"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="command-line-build-for-azure"></a>Komentorivin muodosta Azure varten

## <a name="overview"></a>Yleiskatsaus

Voit luoda paketin Azure käyttöönottoa varten suorittamalla MSBuild komentokehotteeseen. Voit määrittää ja määrittää virheenkorjaus, väliaikaisen ja tuotannon lisäksi automatisointi joitakin muodostusprosessin versiot.


## <a name="microsoft-build-engine-msbuild"></a>Microsoft muodosta ohjelma (MSBuild)

Käyttämällä Microsoft luominen Engine (MSBuild) voit luoda tuotteet muodosta kurssin ympäristössä, johon Visual Studio ei ole asennettu. MSBuild käyttää XML-tiedostomuodon extensible ja täysin tuettu Microsoft project-tiedostoja. Tässä tiedostomuodossa, voit määrittää kohteet on oltava laadittuihin ratkaisuihin alustojen ja määrityksiä.

Voit suorittaa MSBuild komentokehotteeseen ja tässä ohjeaiheessa kerrotaan, että toimintatapa sen vuoksi. Ominaisuuksilla komentokehotteeseen, voit luoda projektin tiettyjä määrityksiä. Vastaavasti voit myös määrittää kohteet, Luo MSBuild-komentoa. Saat lisätietoja komentoriviparametreja ja MSBuild [MSBuild komentoriviltä viittaus](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Asennus

Kuin Seuraavassa kuvataan, asenna ohjelmisto ja työkalut muodosta palvelimessa ennen kuin voit luoda Azure paketin MSBuild:

1. Asenna .NET Framework 4 tai sitä uudempi versio, joka sisältää MSBuild.

1. Asenna [Azure yhtä aikaa muiden kanssa Työkalut](http://go.microsoft.com/fwlink/?LinkId=394615) (Katso MicrosoftAzureAuthoringTools x64.msi tai MicrosoftAzureAuthoringTools x86.msi.

1. Asenna [.NET Azure-kirjastoja](http://go.microsoft.com/fwlink/?LinkId=394616) (Katso MicrosoftAzureLibsForNet x64.msi tai MicrosoftAzureLibs x86.msi.

1. Kopioi Microsoft.WebApplication.targets tiedosto Visual Studio asennuksesta toisessa tietokoneessa.

    Tiedosto sijaitsee directory C:\Program Files (x86) \MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 for Visual Studio 2012) ja kannattaa kopioida muodosta palvelimessa samaan kansioon.

1. Asenna [Azure Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).

    Etsi WindowsAzureTools.vs120.exe luonnissa Visual Studio 2013 projektit.

## <a name="msbuild-parameters"></a>MSBuild parametrit

Suorita MSBuild kanssa on helpointa luoda paketin `/t:Publish` vaihtoehto. Oletusarvoisesti tämä komento luo kansion pääkansion projektin, kuten ProjectDir\bin\Configuration\app.publish suhteessa\. kun luot Azure project-Luo kaksi tiedostoa, itse pakettitiedosto ja mukana kokoonpanotiedosto:

- Project.cspkg

- ServiceConfiguration.TargetProfile.cscfg

Oletusarvon mukaan kunkin Azure projektin sisältää yhden service-määritystiedosto (virheenkorjaus) paikallinen muodostaa ja toinen cloud (väliaikaisen tai tuotannon)-versiot, mutta voi lisätä tai poistaa palvelun määritykset tiedostot tarpeen mukaan. Kun luot Visual Studion paketin, sinua pyydetään mitä palvelun määritykset-tiedosto, joka sisältää paketin rinnalla. Kun luot paketin käyttämällä MSBuild-paikallisen palvelun kokoonpanotiedosto sisältyy oletusarvoisesti. Voit lisätä toisen palvelun määritykset tiedoston, Määritä `TargetProfile` ominaisuuden MSBuild-komennon (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Jos haluat käyttää vaihtoehtoinen hakemisto tallennetut paketti ja tiedostojen, Määritä polku käyttämällä `/p:PublishDir=Directory\` vaihtoehto, kuten lopussa kenoviiva erottimen.

## <a name="deployment"></a>Käyttöönotto

Kun paketti on muodostettu, käyttöön Azure. Opetusohjelmaan, joka osoittaa, että prosessi-artikkelista Azure sivuston. Lisätietoja siitä, miten voit automatisoida, on artikkelissa [Jatkuva toimitukseen pilvipalveluihin Azure-tietokannassa](./cloud-services/cloud-services-dotnet-continuous-delivery.md).
