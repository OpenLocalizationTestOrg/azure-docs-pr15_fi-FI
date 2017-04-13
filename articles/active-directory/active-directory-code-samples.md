<properties
   pageTitle="Azure Active Directory-MALLIKOODEJA | Microsoft Azure"
   description="Indeksin Azure Active Directory-MALLIKOODEJA, skenaarion mukaan järjestettyinä."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="priyamohanram"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="azure-active-directory-code-samples"></a>Azure Active Directory-MALLIKOODEJA

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Voit lisätä todennus- ja web-sovellusten ja verkko-ohjelmointirajapinnan Microsoft Azure Active Directory (Azure AD). Tässä osassa linkittää MALLIKOODEJA, jotka näyttävät, kuinka se tehdään ja koodikatkelmat, voit käyttää sovelluksia. Yksityiskohtainen luku Etsi otoksen koodisivu-me ohjeita, jotka auttavat vaatimukset, asennusta ja käyttöönottoa varten. Ja koodi on Kommentoitu voi auttaa sinua ymmärtämään kriittiset osat.

Esimerkki mistäkin basic skenaario on artikkelissa käyttöoikeuksien skenaariot varten Azure AD.

Microsoftin GitHub malleja osaltaan: [Microsoft Azure Active Directory-mallit ja asiakirjat](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="web-browser-to-web-application"></a>Web-sovelluksen Web-selaimessa

Näitä esimerkkejä näyttää, miten voit kirjoittaa web-sovelluksen, joka ohjaa käyttäjän selaimen Kirjaudu ne Azure AD.



| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| C# / .NET | [Web App-sovelluksen OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Azure AD-vuokraajan käyttäjiä todentaa OpenID yhteyden (ASP.Net OpenID yhteyden OWIN middleware) avulla.
| C# / .NET | [Web App-sovelluksen-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Usean vuokraajan käytetään OpenID yhteyden (ASP.Net OpenID yhteyden OWIN middleware) todentaa käyttäjät useita Azure AD-alihallinnat .NET MVC web-sovelluksen.
| C# / .NET | [Web App-sovelluksen WSFederation-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | Azure AD-vuokraajan-käyttäjien todentamiseen WS-Federation (ASP.Net WS Federation OWIN middleware) avulla.






## <a name="single-page-application-spa"></a>Yksittäisen sivun sovelluksen (SPA)

Tässä esimerkissä esitetään, kuinka voit kirjoittaa yhdelle sivulle-sovelluksen suojattu Azure AD.  

| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| JavaScript-, C# / .NET | [SinglePageApp DotNet](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) | JavaScript-ja Azure AD ADAL avulla voit suojata ASP.NET-verkko-Ohjelmointirajapinnan takaisin pään toteutettu AngularJS perustuva yksittäiseltä sivulta sovelluksen.


## <a name="native-application-to-web-api"></a>Verkko-Ohjelmointirajapinnan alkuperäisen sovelluksen

Nämä MALLIKOODEJA näyttää, miten verkko-ohjelmointirajapinnan, joiden suojauksessa on Azure AD Soita native client-sovellusten rakentamiseen. He käyttää [OAuth-2.0 Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)- [Azure AD todennus kirjaston (ADAL)](active-directory-authentication-libraries.md) .

| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| JavaScript | [NativeClient MultiTarget-Cordova](https://github.com/Azure-Samples/active-directory-cordova-multitarget) | Apache Cordova ADAL ‑laajennuksen avulla voit luoda Apache Cordova-sovellusta, joka soittaa verkko-Ohjelmointirajapinnan ja käyttää Azure AD todennusta varten.
| C# / .NET | [NativeClient DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) | .NET WPF sovellus, jossa puhelut verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD avulla.
| C# / .NET | [NativeClient WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Windows-kaupan sovellus, jossa puhelut verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD.
| C# / .NET | [NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) | Windows-kaupan sovellus, kutsumista usean vuokraajan verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD kanssa.
| C# / .NET | [WebAPI OnBehalfOf-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof) | Alkuperäisen asiakassovellus, joka kutsuu verkko-Ohjelmointirajapinnan, joka saa tunnuksen perusteella alkuperäisen käyttäjän puolesta ja puhelun toiseen verkko-Ohjelmointirajapinnan tunnuksen avulla.
| C# / .NET | [NativeClient WindowsPhone8.1](https://github.com/Azure-Samples/active-directory-dotnet-windowsphone-8.1) | Windows-kaupan-sovellus Windows Phone 8.1, joka kutsuu verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD mukaan.
| ObjC | [NativeClient iOS](https://github.com/Azure-Samples/active-directory-ios) | IOS-sovellus, jotka kutsut verkko-Ohjelmointirajapinnan, edellyttää Azure AD todennusta varten.
| C# / .NET | [WebAPI ManuallyValidateJwt-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation) | Native client-sovellus, joka sisältää logiikan käsittelemään JWT tunnus-verkko-Ohjelmointirajapinnan, OWIN middleware sijaan.
| C# / Xamarin | [NativeClient kuin Xamarin-Android](https://github.com/Azure-Samples/active-directory-xamarin-android) | Xamarin sidonnan, alkuperäisen Azure AD todennus kirjaston (ADAL) for Android-kirjasto.
| C# / Xamarin | [NativeClient Xamarin-iOS](https://github.com/Azure-Samples/active-directory-xamarin-ios) | Xamarin sidonnan, alkuperäisen Azure AD todennus kirjaston (ADAL) iOS.
| C# / Xamarin | [NativeClient MultiTarget-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-multitarget) | Xamarin-projekti, jossa kohteena viisi alustojen ja soittaa verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD mukaan.
| C# / .NET | [NativeClient-palvelimen-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-native-headless) | Alkuperäisen sovellus, joka suorittaa ei ole vuorovaikutteinen todennuksen ja soittaa verkko-Ohjelmointirajapinnan, joka on suojattu Azure AD mukaan.



## <a name="web-application-to-web-api"></a>Web-sovelluksen verkko-Ohjelmointirajapinnan

Nämä MALLIKOODEJA Näytä miten käyttää [OAuth 2.0 Azure AD-](https://msdn.microsoft.com/library/azure/dn645545.aspx) kutsu web-sovellusten rakentamiseen verkko-ohjelmointirajapinnan, joiden suojauksessa on Azure AD.

| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| C# / .NET | [Web App-sovelluksen-WebAPI-OpenIDConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect) | Soita web API kirjautunut sisään käyttäjän käyttöoikeuksilla.
|  C# / .NET | [Web App-sovelluksen WebAPI-OAuth2-AppIdentity-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity) | Soita web API sovelluksen käyttöoikeuksien avulla.
| C# / .NET | [Web App-sovelluksen WebAPI-OAuth2-UserIdentity-kohteeseen-Dotnet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | Lisää luvan [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) 2.0 Azure AD-web-sovellukseen, jotta se voi soittaa verkko-Ohjelmointirajapinnan.
| JavaScript | [WebAPI Nodejs](https://github.com/Azure-Samples/active-directory-node-webapi) | Määritä REST API-palvelun, joka on integroitu Azure AD API suojautumista. Sisältää Node.js palvelimen verkko-Ohjelmointirajapinnan kanssa.
| C# / .NET | [Web App-sovelluksen WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-multitenant-openidconnect) |  Usean vuokraajan MVC web-sovellus, jossa käytetään OpenID yhteyden (ASP.Net OpenID yhteyden OWIN middleware) todentaa käyttäjät Azure AD-vuokraajan. Todennus-koodin avulla voit käynnistää Graph-Ohjelmointirajapinnan.

## <a name="server-or-daemon-application-to-web-api"></a>Palvelimen tai verkko-Ohjelmointirajapinnan Daemon sovelluksesta

Nämä MALLIKOODEJA Näytä muodostaminen daemon tai palvelimen sovellus, joka hakee resurssien verkko-Ohjelmointirajapinnan käyttämällä [OAuth-2.0 Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx)- [Azure AD todennus kirjaston (ADAL)](active-directory-authentication-libraries.md) .

| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| C# / .NET | [Daemon DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon) | Console-sovellus kutsuu verkko-Ohjelmointirajapinnan. Asiakkaan tunnistetieto on salasana.
| C# / .NET | [Suodatinohjelman CertificateCredential-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential) | Konsolisovellus, jossa verkko-Ohjelmointirajapinnan soittaa. Asiakkaan tunnistetieto on varmenne.


## <a name="calling-azure-ad-graph-api"></a>Soittavan Azure AD-kaavio-Ohjelmointirajapinta

Nämä koodiesimerkki näyttää, miten Soita lukemiseen ja kirjoittamiseen directory tietojen Azure AD-kaavio-Ohjelmointirajapinnan sovellusten rakentamiseen.

| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| Java | [Web App-sovelluksen GraphAPI-Java](https://github.com/Azure-Samples/active-directory-java-graphapi-web) | Web-sovelluksen, joka käyttää Graph-Ohjelmointirajapinnan Azure Active directory-tiedot.
| PHP | [Web App-sovelluksen GraphAPI-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web) | Web-sovelluksen, joka käyttää Graph-Ohjelmointirajapinnan Azure Active directory-tiedot.
| C# / .NET | [Web App-sovelluksen GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Web-sovelluksen, joka käyttää Graph-Ohjelmointirajapinnan Azure Active directory-tiedot.
| C# / .NET | [ConsoleApp GraphAPI-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-console) | Tämä Konsolisovellus esitellään yleisiä Graph-Ohjelmointirajapinnan luku- ja kirjoitusoikeudet puhelut, ja kerrotaan, miten voit suorittaa käyttäjän käyttöoikeusmääritykset ja päivittää käyttäjän pikkukuva ja linkkejä.
| C# / .NET | [ConsoleApp-GraphAPI-DiffQuery-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-diffquery) | Käyttäjän objektien Azure AD-vuokraajan muuttuu console-sovellus, joka käyttää eroavuus kyselyn Graph-Ohjelmointirajapinnan saadaksesi säännöllisin väliajoin.
| C# / .NET | [Web App-sovelluksen-GraphAPI-DirectoryExtensions-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-directoryextensions-web) | MVC-sovellus käyttää Graph API kyselyjen yksinkertainen yrityksen organisaatiokaavion luomiseen.
| PHP | [Web App-sovelluksen-GraphAPI-DirectoryExtensions-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-directoryextensions-web) | PHP sovellus, jossa puhelut Graph-Ohjelmointirajapinnan rekisteröidä tiedostotunnistetta ja lukea, päivittää ja poistaa tunnisteen määritteen arvot.


## <a name="authorization"></a>Todennus

Nämä MALLIKOODEJA näyttää, miten Azure AD käytettävät luvan.

| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| C# / .NET | [Web App-sovelluksen GroupClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Suorita roolista käytönhallinta (RBAC) Azure Active Directory-ryhmän saatavat, joka on integroitu Azure AD-sovelluksessa.
| C# / .NET | [Web App-sovelluksen RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Suorita roolista käytönhallinta (RBAC) Azure Active Directory-sovelluksen roolit, joka on integroitu Azure AD-sovelluksessa.


## <a name="legacy-walkthroughs"></a>Aiempien versioiden vaiheittaiset ohjeet

Nämä vaiheittaiset ohjeet käyttää hieman vanha tekniikka, mutta edelleen voi olla merkitystä.

| Kielen/ympäristö | Esimerkki | Kuvaus
| ----------------- | ------ | -----------
| C# / .NET | [Roolipohjainen ja Käyttöoikeusluettelon-pohjainen todennus Microsoft Azure AD-sovelluksessa](http://go.microsoft.com/fwlink/?LinkId=331694) | Suorita Roolipohjainen käyttöoikeuksien (RBAC) ja Käyttöoikeusluettelon-pohjainen todennus, joka on integroitu Azure AD-sovelluksessa.
| C# / .NET |  [AAL - REST-palvelu Windows-kaupan sovellus - todennus](http://go.microsoft.com/fwlink/?LinkId=330605) |  Käyttäjän todennusta ominaisuuksien lisääminen Windows-kaupan sovellus [Azure AD todennus kirjaston (ADAL)](active-directory-authentication-libraries.md) (aiemmalta nimeltään AAL) Windows-kaupan Beta avulla.
| C# / .NET | [ADAL - REST-palveluun alkuperäinen App - todennuksen AAD kautta selaimessa-valintaikkunaan](http://go.microsoft.com/fwlink/?LinkId=259814) |  Käyttäjän todennusta ominaisuuksien lisääminen WPF asiakkaan [Azure AD todennus kirjaston (ADAL)](active-directory-authentication-libraries.md) avulla.
| C# / .NET | [ADAL - REST-palveluun alkuperäinen App – todentaminen ACS selaimessa-valintaikkunaan kautta](http://code.msdn.microsoft.com/AAL-Native-App-to-REST-de57f2cc) |  Käyttäjän todennusta ominaisuuksien lisääminen WPF asiakkaan [Azure AD todennus kirjaston (ADAL)](active-directory-authentication-libraries.md) ja [Käyttää ohjausobjektin palvelun 2.0 (ACS)](http://msdn.microsoft.com/library/azure/hh147631.aspx) avulla.
| C# / .NET | [ADAL - palvelinten todennus](http://go.microsoft.com/fwlink/?LinkId=259816) | [Azure AD todennus kirjaston (ADAL)](active-directory-authentication-libraries.md) avulla voit suojata Palvelukutsut palvelimen puoli prosessin MVC4 verkko-Ohjelmointirajapinnan REST-palveluun.
| C# / .NET | [Web-sovelluksen käyttäminen Azure AD-On lisääminen](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) | Määritä .NET-sovelluksen suorittamiseen web kertakirjautumisen vastaan Azure AD-yrityksen hakemistosta.
| C# / .NET | [Usean vuokraajan Web-sovellusten kanssa Azure AD kehittäminen](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) | Azure AD avulla voit lisätä kertakirjautumisen ja directory käyttömahdollisuudet yhden .NET-sovellus toimii useita organisaatioissa.
JAVA | [Java otoksen App Azure AD-kaavion Ohjelmointirajapinta](http://go.microsoft.com/fwlink/?LinkId=263969) | Azure AD-käyttää hakemiston tietoja Graph-Ohjelmointirajapinnan avulla.
PHP | [Azure AD-kaavion API PHP malli-sovellukseen](http://code.msdn.microsoft.com/PHP-Sample-App-For-Windows-228c6ddb) | Azure AD-käyttää hakemiston tietoja Graph-Ohjelmointirajapinnan avulla.
| C# / .NET | [Esimerkki sovelluksen Azure AD-kaavion Ohjelmointirajapinta](http://go.microsoft.com/fwlink/?LinkID=262648) | Azure AD-käyttää hakemiston tietoja Graph-Ohjelmointirajapinnan avulla.
| C# / .NET | [Esimerkki sovelluksen Azure AD-kaavion eroavuus kyselyn](http://go.microsoft.com/fwlink/?LinkId=275398) | Hae säännöllisiä tehtyjen muutosten Azure AD-vuokraajan Graph-Ohjelmointirajapinnan eroavuus kyselyn avulla.
| C# / .NET | [Esimerkki sovelluksen integrointi Azure AD usean vuokraajan Cloud hakeminen](http://go.microsoft.com/fwlink/?LinkId=275397) | Usean vuokraajan sovelluksen integroida Azure AD.
| C# / .NET | [Windows-kaupan sovelluksen ja REST-palvelun avulla Azure AD suojaaminen](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) | Luo yksinkertaisia verkko-Ohjelmointirajapinnan resurssi ja Azure AD käyttämällä Windows-kaupan asiakassovellus ja [Azure AD todennus kirjaston (ADAL)](active-directory-authentication-libraries.md).
| C# / .NET| [Kyselyn Azure AD kaavio Ohjelmointirajapinnan käyttäminen](https://github.com/Azure-Samples/active-directory-dotnet-graphapi-web) | Microsoft .NET-sovelluksen käyttämään Azure AD-kaavio-Ohjelmointirajapinnan käyttämään tietoja Azure AD-vuokraajan kansiosta kokoonpanon määrittäminen.

## <a name="see-also"></a>Katso myös

##### <a name="other-resources"></a>Muut resurssit

[Azure Active Directory-Sovelluskehittäjän opas](active-directory-developers-guide.md)

[Azure AD-kaavion käsitteellinen API- ja viitefunktiot](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Azure AD-kaavion API Helper kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)

