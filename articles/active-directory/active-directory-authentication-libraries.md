<properties
   pageTitle="Azure Active Directory käyttöoikeuksien kirjastojen | Microsoft Azure"
   description="Azure AD todennus kirjaston (ADAL) sallii sovelluskehittäjille tarkistamiseen helposti käyttäjien cloud tai paikallisen Active Directory (AD) ja hankkia Accessin tunnusten suojaamiseen API-kutsuja."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure Active Directory käyttöoikeuksien kirjastot

Azure AD-todennus kirjaston (ADAL) avulla asiakkaan sovelluskehittäjät tarkistamiseen helposti käyttäjien cloud paikallisen Active Directory (AD) ja hankkia Accessin tunnusten suojaamiseen API-kutsuja. ADAL on monia ominaisuuksia, varmista, että todennus helpompaa kehittäjille asynkroninen tuki määritettäviä suojaustunnuksen välimuistin, joka tallentaa access tunnusten ja Päivitä tunnusten automaattisen päivityksen tunnuksen, kun access-tunniste vanhentuu ja Päivitä-tunnuksen on käytettävissä, kuten ja paljon muuta. Sisällyttämällä monimutkaisuus useimmat ADAL voit Ohje liiketoimintalogiikka niiden sovelluksen kehittäjän keskittyä ja suojatun helposti ilman, että asiantuntija suojauksen resurssit.

ADAL on käytettävissä eri ympäristöissä.

|Käyttöympäristö|Pakettinimi|Asiakkaan ja palvelimen|Lataa|Lähdekoodin|Asiakirjat ja mallit|
|---|---|---|---|---|---|
|.NET-asiakasohjelma ja Windows-kaupan UWP Xamarin Live v2.0|Active Directory käyttöoikeuksien kirjaston (ADAL) .NET v3: lle |Asiakkaan|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[ADAL .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[Ohjeet](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET-asiakasohjelmaan Windows-kaupan Windows Phone 8.1 |.NET v2 Active Directory käyttöoikeuksien kirjasto (ADAL) |Asiakkaan|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[ADAL .NET (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[Ohjeet](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|JavaScript Active Directory käyttöoikeuksien kirjasto (ADAL)|Asiakkaan|[ADAL JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|Esimerkki: [SinglePageApp-DotNet (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|OS X-, iOS|Tavoite-C Active Directory käyttöoikeuksien kirjasto (ADAL)|Asiakkaan|[ADAL tavoite-C (CocoaPods)](http://cocoadocs.org/docsets/ADAL/)|[ADAL tavoite-C (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|Esimerkki: [NativeClient-iOS (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android-laitteeseen|Active Directory käyttöoikeuksien kirjaston (ADAL) androidille|Asiakkaan|[ADAL for Android (Keskussäilöön)](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[ADAL for Android (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-android)|Esimerkki: [NativeClient kuin Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|Node.js Active Directory käyttöoikeuksien kirjasto (ADAL)|Asiakkaan|[Node.js (npm) ADAL](https://www.npmjs.com/package/adal-node)|[ADAL Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|Esimerkki: [WebAPI-Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Microsoft Azure Active Directory Passport todennus middleware solmun|Asiakkaan|[Azure Active Directory-Passport Node.js (npm) varten](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directoryn Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Java Active Directory käyttöoikeuksien kirjasto (ADAL)|Asiakkaan|[ADAL Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[ADAL Java (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|Käyttäjätietojen protokolla tunnisteet Microsoft .NET Framework 4.5|Palvelin|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure AD tunnistetietojen mallin tunnisteet .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|JSON Web suojaustunnuksen käsittelijä Microsoft .net Framework 4.5|Palvelin|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure AD tunnistetietojen mallin tunnisteet .NET (Github)](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|OWIN middleware, joka mahdollistaa sovelluksen käyttämään Microsoftin tekniikka todennusta varten.|Palvelin|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|OWIN middleware, joka mahdollistaa sovelluksen käyttämään OpenIDConnect todennusta varten.|Palvelin|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Esimerkki: [Web App-sovelluksen-OpenIDConnecty-DotNet (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN middleware, joka mahdollistaa sovelluksen käyttämään WS Federation todennusta varten.|Palvelin|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|Esimerkki: [Web App-sovelluksen-WSFederation-DotNet (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>Skenaariot

Seuraavassa on kolme Yleisiä tilanteita, joissa jossa ADAL voidaan käyttää todennusta varten.  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>Asiakassovellus Remote resurssille käyttäjien

Tässä skenaariossa kehittäjä on asiakas, kuten WPF-sovellusta, on suojattu Azure AD, kuten verkko-Ohjelmointirajapinnan remote resurssin. Hän on Azure-tilaus, tietää, miten voit käynnistää edeltävät verkko-Ohjelmointirajapinnan ja tietää, joka käyttää verkko-Ohjelmointirajapinnan Azure AD-vuokraajan. Hän seurauksena helpottaa todentaminen Azure AD-delegoiminen täysin todennus-myös ADAL tai käsittely erikseen käyttäjän tunnistetietoja käyttämällä ADAL. ADAL tekee todennetaan, hanki käyttöoikeustietue ja Päivitä tunnuksen Azure AD ja varmista, käyttöoikeustietue avulla helppoa pyytää verkko-Ohjelmointirajapinnan.

Katso koodi-esimerkki, joka näyttää artikkelista Azure AD-todennusta- [Verkko-Ohjelmointirajapinnan Native Client WPF sovelluksen](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>Palvelin-sovelluksen Remote resurssille todennustapa

Tässä skenaariossa kehittäjä on palvelimella, joka on suojattu Azure AD, kuten verkko-Ohjelmointirajapinnan etäyhteyden resurssin sovellus. Hän on Azure-tilaus, tietävät, miten ongelma edeltävät palvelu ja tietää Azure AD-vuokraajan verkko-Ohjelmointirajapinnan käyttää. Hän seurauksena käyttää ADAL helpottaa Azure AD todentaminen käsittely erikseen sovelluksen tunnistetiedot. ADAL helpottaa sen tunnusta noutaa Azure AD käyttämällä sovelluksen asiakkaan tunnistetieto ja varmista, että tunnuksen avulla on helppo pyytää verkko-Ohjelmointirajapinnan. ADAL käsittelee myös hallinta käyttöoikeustietue elinkaaren tallentamalla sen välimuistiin ja uusiminen tarpeen mukaan. Katso koodi-esimerkki, joka näyttää tässä tilanteessa, [Verkko-Ohjelmointirajapinnan Console-sovelluksen](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>Käyttäjän puolesta palvelinsovelluksen etäyhteyden resurssin todennustapa

Tässä skenaariossa kehittäjä on palvelimella, joka on suojattu Azure AD, kuten verkko-Ohjelmointirajapinnan etäyhteyden resurssin sovellus. Pyyntö on myös tehtävä Azure AD käyttäjän puolesta. Hän on Azure-tilaus, tietää, miten voit käynnistää edeltävät verkko-Ohjelmointirajapinnan ja tietää vuokraaja palvelu käyttää Azure AD. Kun käyttäjä todennetaan WWW-sovellukseen, sovelluksen hakea todennus-koodin käyttäjän Azure AD. Web-sovelluksen voit käyttää ADAL access-tunnuksen hankkiminen ja Päivitä tunnuksen Azure AD-sovellukseen kytketty todennus-koodin ja asiakkaan tunnistetiedoilla käyttäjän puolesta. Web-sovelluksen on käytettävissään käyttöoikeustietue, kun se Soita verkko-Ohjelmointirajapinnan kunnes tunnuksen vanhenee. Kun tunnuksen vanhenee, web-sovelluksen voit käyttää ADAL uusi käyttöoikeustietue saat käyttämällä päivitys-tunnuksen, joka saatiin aiemmin.


## <a name="see-also"></a>Katso myös

[Azure Active Directory-Sovelluskehittäjän opas](active-directory-developers-guide.md)

[Azure Active directory käyttöoikeuksien tilanteita, joissa](active-directory-authentication-scenarios.md)

[Azure Active Directory-MALLIKOODEJA](active-directory-code-samples.md)
