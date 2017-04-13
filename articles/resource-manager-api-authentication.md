<properties 
   pageTitle="Active Directory-todennusta ja Resurssienhallinta | Microsoft Azure"
   description="Todennus Azure Resurssienhallinta API ja Active Directory sovelluksen integraation muut Azure-tilaukset kehittäjän opas."
   services="azure-resource-manager,active-directory"
   documentationCenter="na"
   authors="dushyantgill"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="dugill;tomfitz" />

# <a name="how-to-use-azure-active-directory-and-resource-manager-to-manage-a-customers-resources"></a>Asiakkaan resurssien Azure Active Directory- ja resurssien hallinnan avulla

## <a name="introduction"></a>Johdanto

Jos olet ohjelmistokehittäjä, kuka on luotava sovellus, joka hallitsee asiakkaan Azure resursseja, tässä ohjeaiheessa esitellään Azure Resurssienhallinta-ohjelmointirajapinnan todentamismenetelmä ja käyttää muita tilaukset resursseja. 

Sovelluksen voit käyttää Resurssienhallinta-ohjelmointirajapinnan eri tavoin:

1. **Käyttäjän + sovelluksen access**: sovellusten, joka käyttää resursseja kirjautunut sisään käyttäjän puolesta. Tämä menetelmä toimii sovellusten, kuten web Apps-sovellusten ja komentorivin Työkalut, joka käsitellä vain "vuorovaikutteinen" Azure resurssien hallintaa.
1. **Vain App**: sovellusten, jotka suoritetaan daemon palvelut ja ajoitetuissa. Sovelluksen tunnistetietojen myönnetään suora pääsy resurssit. Tämä menetelmä toimii sovellukset, jotka on pitkällä aikavälillä "offline-käytön" Azure.

Tämä artikkeli sisältää vaiheittaiset ohjeet sovelluksen, joka on suojattu nämä luvan lopettamista luominen. Se näyttää tietoja kunkin vaiheen REST API- tai C#. Valmis ASP.NET MVC-sovellus on saatavilla kohdassa [https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense](https://github.com/dushyantgill/VipSwapper/tree/master/CloudSense).

Kaikki tämän aiheen koodi on käynnissä, voit kokeilla osoitteessa [http://vipswapper.azurewebsites.net/cloudsense](http://vipswapper.azurewebsites.net/cloudsense)verkkosovellukseen. 

## <a name="what-the-web-app-does"></a>Web-sovelluksen toiminta

Web-sovelluksen:

1. Etumerkki-Azure käyttäjä.
2. Web app-käyttöoikeuksien myöntäminen resurssien hallinnan käyttäjää pyydetään.
3. Saa käyttäjän + sovelluksen käyttöoikeustietue käyttämiseen Resurssienhallinta.
4. Tunnuksen (vaiheesta 3) avulla voit soittaa Resurssienhallinta ja Määritä rooli-tilaus, jonka kautta app pitkään käsiksi tilaukseen sovelluksen palvelun lyhennyksen.
5. Saa vain sovelluksen tunnuksen.
6. Resurssien tilauksen kautta Resurssienhallinta käyttää tunnuksen (-vaihe 5).

Seuraavassa on web-sovelluksen lopusta loppuun-työnkulku.

![Resurssienhallinta todennus-työnkulku](./media/resource-manager-api-authentication/Auth-Swim-Lane.png)

Käyttäjä Anna tilauksen tunnus tilaukseen, jota haluat käyttää:

![Anna tilauksen tunnus](./media/resource-manager-api-authentication/sample-ux-1.png)

Valitse tili, jota käytetään kirjauduttaessa.

![Valitse tili](./media/resource-manager-api-authentication/sample-ux-2.png)

Anna tunnistetiedot.

![Anna tunnistetiedot](./media/resource-manager-api-authentication/sample-ux-3.png)

Azure-tilauksia käyttöoikeus sovelluksen:
 
![Käyttöoikeuksien myöntäminen](./media/resource-manager-api-authentication/sample-ux-4.png)
 
Yhdistetyn tilausten hallinta:

![Yhdistä tilaus](./media/resource-manager-api-authentication/sample-ux-7.png)


## <a name="register-application"></a>Sovelluksen rekisteröinti

Ennen kuin aloitat coding, rekisteröidä web-sovelluksen kanssa Azure Active Directory (AD). Sovelluksen rekisteröinti Luo keskitetyn tunnistetiedot, kun sovellus Azure AD. Se on perustietoja sovelluksesi, kuten OAuth Ostajantunnus vastaa URL-osoitteet ja tunnistetiedot, jotka sovellus käyttää todennusta ja käyttää Azure Resurssienhallinta API. Sovelluksen rekisteröinti tallentaa myös eri valtuutetun käyttöoikeuksista sovelluksen tarvitsemat käytettäessä Microsoft APIs käyttäjän puolesta. 

Koska sovellus käyttää muu tilaus, sinun on määritettävä usean vuokraajan sovelluksena. Välittää vahvistus on toimialueeseen liittyvä Active Directory-hakemistosta. Saat toimialueiden liittyvän Active Directory-Kirjaudu sisään [perinteinen portal](https://manage.windowsazure.com). Active Directory ja valitse sitten **Toimialueet**.

Seuraavassa esimerkissä esitetään, miten sovellus rekisteröidään Azure PowerShell-toiminnolla. Sinulla on oltava Saat komennon toimimaan PowerShellin Azure uusimpaan versioon (elokuu 2016). 

    $app = New-AzureRmADApplication -DisplayName "{app name}" -HomePage "https://{your domain}/{app name}" -IdentifierUris "https://{your domain}/{app name}" -Password "{your password}" -AvailableToOtherTenants $true
    
Kirjaudu sisään AD-sovelluksen, tarvitset tunnus ja salasana. Voit tuoda sovellustunnus, joka palautetaan, Edellinen-komennon avulla:

    $app.ApplicationId

Seuraavassa esimerkissä esitetään rekisteröimään sovelluksen Azure CLI avulla. 

    azure ad app create --name {app name} --home-page https://{your domain}/{app name} --identifier-uris https://{your domain}/{app name} --password {your password} --available true

Tulokset ovat sovelluksen tunnus, jota tarvitset todennuksen sovelluksen yhteydessä.

### <a name="optional-configuration---certificate-credential"></a>Valinnainen määritys - sertifikaatin tunnistetiedon

Azure AD tukee myös varmennetta tunnistetietojen sovellusten: itse allekirjoitetun varmenteen luominen ja säilyttää yksityinen avain julkinen avain lisääminen Azure AD-sovelluksen rekisteröinti. Todennuksen sovelluksesi lähettää pieni paketti Azure AD allekirjoittaa yksityinen avain ja Azure AD tarkistaa allekirjoitus, joka on rekisteröity julkinen avain.

Lisätietoja AD-sovelluksen luominen varmenteella on artikkelissa [Azure PowerShellin Luo pääasiallista resursseihin palvelu](resource-group-authenticate-service-principal.md#create-service-principal-with-certificate) tai [Käytä Azure CLI pääasiallista resursseihin palvelun luomiseen](resource-group-authenticate-service-principal-cli.md#create-service-principal-with-certificate).

## <a name="get-tenant-id-from-subscription-id"></a>Pyydä Tilaustunnus vuokraajan tunnus

Voit pyytää tunnuksen, jonka avulla voidaan kutsua Resurssienhallinta-sovelluksesi on tiedettävä, joka isännöi Azure tilauksen Azure AD-vuokraajan vuokraajan tunnus. Todennäköisesti että käyttäjät tietävät tilauksen niiden tunnukset, mutta ne ehkä tiedä käyttäjän vuokraajan tunnukset Active Directory. Saat vuokraajan käyttäjätunnus-pyytävät käyttäjää tilauksen tunnus. Anna tilauksen tunnus lähetettäessä pyyntö tilauksen:

    https://management.azure.com/subscriptions/{subscription-id}?api-version=2015-01-01

Kutsu epäonnistuu, koska käyttäjä on ole kirjautuneena vielä, mutta voit noutaa vuokraajan tunnus vastaus. Hakea vuokraajan tunnus vastauksen otsikon arvosta kyseisen poikkeuksen **WWW-todennusta**varten. Näet tämän toteutuksen [GetDirectoryForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L20) -menetelmää.

## <a name="get-user--app-access-token"></a>Hae käyttäjän + sovelluksen käyttöoikeustietue

Sovelluksen ohjaa käyttäjän Azure AD OAuth 2.0 Hyväksy pyyntö - todentaa käyttäjän tunnistetiedot ja palata todennus-koodin avulla. Sovellus käyttää luvan voit käyttää suojaustunnuksen resurssien hallinta. [ConnectSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/Controllers/HomeController.cs#L42) -menetelmä luo luvan pyynnön.

Tässä ohjeaiheessa esitellään todentaessaan käyttäjän REST API-pyynnöt. Voit käyttää myös helper kirjastojen suorittamaan todennusta-koodisi. Saat lisätietoja nämä kirjastot [Azure Active Directory käyttöoikeuksien kirjastoihin](./active-directory/active-directory-authentication-libraries.md). Ohjeita-integrointi jäsenyyksien hallinta-sovelluksessa on artikkelissa [Azure Active Directory-Sovelluskehittäjän opas](./active-directory/active-directory-developers-guide.md).

### <a name="auth-request-oauth-20"></a>Pyyntö auth (OAuth 2.0)

Azure AD-Hyväksy päätepisteelle myöntää Avaa tunnus Yhdistä/OAuth2.0 sallivat pyytää:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize

Merkkijono kyselyparametrit, jotka ovat käytettävissä pyyntö on artikkelissa [pyynnön todennus-koodi](./active-directory/active-directory-protocols-oauth-code.md#request-an-authorization-code) .

Seuraavassa esimerkissä esitetään, miten pyytää OAuth2.0 luvan:

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=query&response_type=code&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&domain_hint=live.com

Azure AD todentaa käyttäjän ja tarvittaessa kysyy käyttäjältä Myönnä käyttöoikeudet-sovellukseen. Se palauttaa luvan koodi sovelluksen vastaa URL-osoite. Sen mukaan joko pyydetty response_mode Azure AD lähettää tiedot kyselymerkkijonon tai Kirjaa tietoina.

    code=AAABAAAAiL****FDMZBUwZ8eCAA&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="auth-request-open-id-connect"></a>Auth pyyntö (Avaa tunnus-yhteyden)

Jos haluat ei vain pääsyn Azure Resurssienhallinta käyttäjän puolesta, mutta antaa käyttäjän sovelluksesi niiden Azure AD-tilillä kirjautuminen myös, Lähetä Avaa tunnus yhteyden sallivat pyytää. Avaa tunnus-yhteyden, jossa sovellus myös saa id_token Azure AD, sovelluksen voit käyttää käyttäjän kirjautuminen.

Merkkijono kyselyparametrit, jotka ovat käytettävissä pyyntö on kuvattu [lähettää kirjautumisen pyynnön](./active-directory/active-directory-protocols-openid-connect-code.md#send-the-sign-in-request) aihetta.

Esimerkki Avaa tunnus yhteyden pyyntö on:

     https://login.microsoftonline.com/{tenant-id}/OAuth2/Authorize?client_id=a0448380-c346-4f9f-b897-c18733de9394&response_mode=form_post&response_type=code+id_token&redirect_uri=http%3a%2f%2fwww.vipswapper.com%2fcloudsense%2fAccount%2fSignIn&resource=https%3a%2f%2fgraph.windows.net%2f&scope=openid+profile&nonce=63567Dc4MDAw&domain_hint=live.com&state=M_12tMyKaM8

Azure AD todentaa käyttäjän ja tarvittaessa kysyy käyttäjältä Myönnä käyttöoikeudet-sovellukseen. Se palauttaa luvan koodi sovelluksen vastaa URL-osoite. Sen mukaan joko pyydetty response_mode Azure AD lähettää tiedot kyselymerkkijonon tai Kirjaa tietoina.

Esimerkki Avaa tunnus yhteyden vastaus on:

    code=AAABAAAAiL*****I4rDWd7zXsH6WUjlkIEQxIAA&id_token=eyJ0eXAiOiJKV1Q*****T3GrzzSFxg&state=M_12tMyKaM8&session_state=2d16bbce-d5d1-443f-acdf-75f6b0ce8850

### <a name="token-request-oauth20-code-grant-flow"></a>Suojaustunnuksen pyyntö (OAuth2.0-koodin myöntäminen Flow)

Nyt kun sovelluksen todennus-koodi on saanut Azure AD-on aikaa, voit käyttää tunnuksen Azure Resurssienhallinta.  Kirjaaminen OAuth2.0 koodi myöntäminen suojaustunnuksen pyytää Azure AD-tunnuksen päätepisteen: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Merkkijonon kyselyparametrit, jotka ovat käytettävissä pyyntö on kuvattu [koodia luvan käyttää](./active-directory/active-directory-protocols-oauth-code.md#use-the-authorization-code-to-request-an-access-token) aihetta.

Seuraavassa esimerkissä esitetään koodin myöntäminen tunnuksen, jonka salasanan sijaan pyyntö:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Kun käsittelet varmennetta tunnistetietojen, Luo JSON Web suojaustunnuksen (JWT) ja kirjaudu (RSA SHA256) käyttämällä sovelluksen varmennetta tunnistetietojen yksityinen avain. Tunnuksen varaa tyypit näkyvät [JWT suojaustunnuksen saatavat](./active-directory/active-directory-protocols-oauth-code.md#jwt-token-claims). Katso viittaus, [Active Directory Auth kirjaston (.NET) koodin](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/blob/dev/src/ADAL.PCL.Desktop/CryptographyHelper.cs) allekirjoittamiseen asiakkaan vahvistus JWT tunnusten.

Katso lisätietoja [Avaa tunnus yhteyden määritys](http://openid.net/specs/openid-connect-core-1_0.html#ClientAuthentication) Asiakkaan todentaminen. 

Seuraavassa esimerkissä esitetään koodin myöntäminen tunnuksen, jonka varmennetta tunnistetietojen pyynnön:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1
    
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012
    
    grant_type=authorization_code&code=AAABAAAAiL9Kn2Z*****L1nVMH3Z5ESiAA&redirect_uri=http%3A%2F%2Flocalhost%3A62080%2FAccount%2FSignIn&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbG*****Y9cYo8nEjMyA

Esimerkki-vastauksen koodin myöntää tunnuksen: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039858","not_before":"1432035958","resource":"https://management.core.windows.net/","access_token":"eyJ0eXAiOiJKV1Q****M7Cw6JWtfY2lGc5A","refresh_token":"AAABAAAAiL9Kn2Z****55j-sjnyYgAA","scope":"user_impersonation","id_token":"eyJ0eXAiOiJKV*****-drP1J3P-HnHi9Rr46kGZnukEBH4dsg"}

#### <a name="handle-code-grant-token-response"></a>Käsittele koodin myöntäminen suojaustunnuksen vastaus

Onnistuneiden suojaustunnuksen vastaus sisältää (käyttäjän + app) käyttöoikeustietue Azure Resurssienhallinta. Sovellus käyttää tämä käyttöoikeustietue käyttäjän puolesta Resurssienhallinta. Access-tunnusten Azure AD myöntämä elinkaaren on tunnin. On epätodennäköistä, että web-sovelluksen täytyy uusia (käyttäjän + app) käyttöoikeustietue. Jos se on uusi käyttöoikeustietue, käytä päivityksen tunnus, jonka sovellus vastaanottaa suojaustunnuksen vastauksessa. Kirjaaminen OAuth2.0 suojaustunnuksen pyytää Azure AD-tunnuksen päätepisteen: 

    https://login.microsoftonline.com/{tenant-id}/OAuth2/Token

Päivitä pyynnön käytettäväksi parametrit on artikkelissa [access-tunnuksen päivittäminen](./active-directory/active-directory-protocols-oauth-code.md#refreshing-the-access-tokens).

Seuraavassa esimerkissä esitetään käyttämisestä päivitys suojaustunnuksen:

    POST https://login.microsoftonline.com/7fe877e6-a150-4992-bbfe-f517e304dfa0/oauth2/token HTTP/1.1

    Content-Type: application/x-www-form-urlencoded
    Content-Length: 1012

    grant_type=refresh_token&refresh_token=AAABAAAAiL9Kn2Z****55j-sjnyYgAA&client_id=a0448380-c346-4f9f-b897-c18733de9394&client_secret=olna84E8*****goScOg%3D

Päivitä tunnusten avulla voidaan saada uuden access tunnusten Azure Resurssienhallinta, ne eivät sovellu sovelluksen offline-käyttö. Päivitä tunnusten elinaika on rajoitettu ja Päivitä tunnusten on sidottu käyttäjälle. Jos käyttäjä jättää organisaation-sovelluksesta valitsemalla Päivitä tunnuksen menettää access. Tämän menetelmän ei sovellu sovellukset, joita käytetään niiden Azure resurssien ryhmiä.

## <a name="check-if-user-can-assign-access-to-subscription"></a>Tarkista, jos käyttäjä voi määrittää access-tilaukseen

Nyt sovellus on tunnuksen, voit käyttää Azure Resurssienhallinta käyttäjän puolesta. Seuraavaksi sovelluksen yhdistäminen tilaus. Kun yhteys on muodostettu, sovelluksen voit hallita näitä tilaukset, myös silloin, kun käyttäjä ei ole käytettävissä (pitkään offline-tila). 

Tilauksen kunkin muodostaa Soita [Resurssienhallinta luettelon käyttöoikeudet](https://msdn.microsoft.com/library/azure/dn906889.aspx) API määrittääksesi, onko käyttäjän käyttöoikeuksien hallinta-tilausta.

ASP.NET-MVC otoksen sovelluksen [UserCanManagerAccessForSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L44) -menetelmä toteuttaa puhelun.

Seuraavassa esimerkissä esitetään, miten pyytää tilauksen käyttäjän käyttöoikeuksista. 83cfe939-2402-4581-b761-4f59b0a041e4 on tilauksen tunnus.

    GET https://management.azure.com/subscriptions/83cfe939-2402-4581-b761-4f59b0a041e4/providers/microsoft.authorization/permissions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiLC***lwO1mM7Cw6JWtfY2lGc5A

Esimerkki vastauksen käyttäjän oikeuksien saaminen tilauksessa on:

    HTTP/1.1 200 OK

    {"value":[{"actions":["*"],"notActions":["Microsoft.Authorization/*/Write","Microsoft.Authorization/*/Delete"]},{"actions":["*/read"],"notActions":[]}]}

Käyttöoikeuksien API palauttaa useita käyttöoikeuksia. Kukin käyttöoikeus koostuu sallitut toiminnot (toiminnot) ja kielletyt toiminnot (notactions). Jos toiminto ei ole oikeutta sallitut toiminnot-luettelo ei ole tätä käyttöoikeutta notactions luettelossa, käyttäjä voi suorittaa toiminnon. **Microsoft.Authorization/RoleAssignments/Write** on, että, joka antaa käytön hallinta-toiminto. Sovellus on jäsennettävä käyttöoikeudet tämän toiminnon merkkijonon toiminnot ja kunkin käyttöoikeuden notactions regex vastinetta Tarkista tulos.

## <a name="get-app-only-access-token"></a>Hae vain sovelluksen tunnus

Tiedät nyt, jos käyttäjä määrittää access Azure-tilaukseen. Seuraavat vaiheet ovat seuraavat:

1. Määritä RBAC asianmukainen rooli sovelluksen tunnistetietojen tilauksen.
2. Vahvista access varauksen kyselyistä sovelluksen käyttöoikeuksia tilauksen tai käytettäessä resurssien hallinnan käyttäminen vain sovelluksen tunnuksen.
1. Tallentaa yhteyden sovellukset "yhdistetyn Tilaukset" tietorakenteen - toteaa tilauksen tunnus.

Oletetaan, että tarkemmin ensimmäinen vaihe. Jos haluat määrittää RBAC asianmukainen rooli sovelluksen tunnistetiedot, sinun on määritettävä:

- Sovelluksen tunnistetietojen käyttäjän Azure Active Directoryn objektitunnus
- RBAC rooli määrää sovellus edellyttää tilauksen tunnus

Kun sovellus todentaa Azure AD käyttäjältä, se luo service Pääobjektin sovelluksen kyseisen Azure AD. Azure avulla voi määrittää palvelun ansaitun vastaavan sovellusten Azure resurssien suora pääsy RBAC roolit. Tämä toiminto on täsmälleen olemme haluat tehdä. Kyselyn Azure AD-kaavio-Ohjelmointirajapinnan määrittämään palvelun lyhennys sovelluksesi kirjautunut sisään käyttäjä tunnus on Azure AD.

Vain on access-tunnuksen Azure Resurssienhallinta - tarvitset uuden käyttöoikeustietue Soita Azure AD-kaavio-Ohjelmointirajapinnan. Jokainen Azure AD-sovellus on oikeus kyselyn omassa service Pääobjektin niin vain sovelluksen käyttöoikeustietue on riittävä.

<a id="app-azure-ad-graph">
### <a name="get-app-only-access-token-for-azure-ad-graph-api"></a>Hae vain sovelluksen suojaustunnuksen Azure AD-kaavio-Ohjelmointirajapinta

Sovelluksen todennusta ja Azure AD Graph API tunnistetta, antaa asiakkaan tunnistetiedon myöntäminen OAuth2.0 työnkulku suojaustunnuksen pyyntö Azure AD suojaustunnuksen päätepisteen (**https://login.microsoftonline.com/ {directory_domain_name} / OAuth2/tunnus**).

ASP.net-MVC-sovelluksen malli [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs) -menetelmää saa vain sovelluksen access suojaustunnuksen Graph API käyttämällä Active Directory käyttöoikeuksien kirjaston .NET.

Kyselymerkkijonon ominaisuuksia, jotka ovat käytettävissä pyyntö on artikkelissa [Access-tunnuksen pyynnön](./active-directory/active-directory-protocols-oauth-service-to-service.md#request-an-access-token) .

Esimerkki-pyyntö asiakkaan tunnistetiedon myöntää tunnuksen: 

    POST https://login.microsoftonline.com/62e173e9-301e-423e-bcd4-29121ec1aa24/oauth2/token HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 187</pre>
    <pre>grant_type=client_credentials&client_id=a0448380-c346-4f9f-b897-c18733de9394&resource=https%3A%2F%2Fgraph.windows.net%2F &client_secret=olna8C*****Og%3D

Esimerkki-vastauksen asiakkaan tunnistetiedon myöntää tunnuksen: 

    HTTP/1.1 200 OK

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1432039862","not_before":"1432035962","resource":"https://graph.windows.net/","access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiJodHRwczovL2dyYXBoLndpbmRv****G5gUTV-kKorR-pg"}

### <a name="get-objectid-of-application-service-principal-in-user-azure-ad"></a>Hae käyttäjän Azure AD-sovelluksen palvelun lyhennys objektitunnus

Nyt kyselyn [ansaitun Azure AD Graph Service](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity) API määrittämään sovelluksen palvelun pääasiallista hakemistossa objektitunnus vain sovelluksen käyttöoikeustietue avulla.

ASP.net-MVC-sovelluksen malli [GetObjectIdOfServicePrincipalInOrganization](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureADGraphAPIUtil.cs#) maksutavan toteuttaa puhelun.

Seuraavassa esimerkissä esitetään, miten pyytää sovelluksen palvelun lyhennys. a0448380-c346-4f9f-b897-c18733de9394 on Asiakastunnus-sovelluksen.

    GET https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/servicePrincipals?api-version=1.5&$filter=appId%20eq%20'a0448380-c346-4f9f-b897-c18733de9394' HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJK*****-kKorR-pg

Seuraavassa esimerkissä esitetään sovelluksen palvelun pyynnön vastauksen pääasiallista 

    HTTP/1.1 200 OK

    {"odata.metadata":"https://graph.windows.net/62e173e9-301e-423e-bcd4-29121ec1aa24/$metadata#directoryObjects/Microsoft.DirectoryServices.ServicePrincipal","value":[{"odata.type":"Microsoft.DirectoryServices.ServicePrincipal","objectType":"ServicePrincipal","objectId":"9b5018d4-6951-42ed-8a92-f11ec283ccec","deletionTimestamp":null,"accountEnabled":true,"appDisplayName":"CloudSense","appId":"a0448380-c346-4f9f-b897-c18733de9394","appOwnerTenantId":"62e173e9-301e-423e-bcd4-29121ec1aa24","appRoleAssignmentRequired":false,"appRoles":[],"displayName":"CloudSense","errorUrl":null,"homepage":"http://www.vipswapper.com/cloudsense","keyCredentials":[],"logoutUrl":null,"oauth2Permissions":[{"adminConsentDescription":"Allow the application to access CloudSense on behalf of the signed-in user.","adminConsentDisplayName":"Access CloudSense","id":"b7b7338e-683a-4796-b95e-60c10380de1c","isEnabled":true,"type":"User","userConsentDescription":"Allow the application to access CloudSense on your behalf.","userConsentDisplayName":"Access CloudSense","value":"user_impersonation"}],"passwordCredentials":[],"preferredTokenSigningKeyThumbprint":null,"publisherName":"vipswapper"quot;,"replyUrls":["http://www.vipswapper.com/cloudsense","http://www.vipswapper.com","http://vipswapper.com","http://vipswapper.azurewebsites.net","http://localhost:62080"],"samlMetadataUrl":null,"servicePrincipalNames":["http://www.vipswapper.com/cloudsense","a0448380-c346-4f9f-b897-c18733de9394"],"tags":["WindowsAzureActiveDirectoryIntegratedApp"]}]}

### <a name="get-azure-rbac-role-identifier"></a>Azure RBAC rooli tunnuksen hankkiminen

Määritä tärkeimmät palvelun RBAC asianmukainen rooli, sinun on määritettävä Azure RBAC rooli tunnuksen.

Sovelluksen oikean RBAC roolia:

- Jos sovelluksen valvoo tilauksen-vain ilman, että kaikki muutokset, se vaatii vain lukijan oikeudet tilauksen. **Lukija** -roolin määrittäminen.
- Jos sovelluksen hallitsee Azure tilauksen luominen tai muokkaaminen ja poistaminen kohteiden edellyttää on osallistuja-oikeudet.
  - Hallittavan resurssin tietyn tyyppisiä määrittää resurssin kielikohtaiset avustaja-roolit (virtuaalikoneen avustaja, Virtual verkon avustaja-tallennustilan tilin avustaja jne.)
  - Voit hallita minkä tahansa resurssilaji-määrittää **osallistujan** rooli.

Sovelluksen roolimääritys näkyy käyttäjille, joten valitse vähiten tarvittavat oikeudet.

Soita [Resurssienhallinta roolimääritys API](https://msdn.microsoft.com/library/azure/dn906879.aspx) luettelon kaikki Azure RBAC roolit ja etsi sitten käytöstä löydät haluamasi roolimääritys nimelläsi tulosten päälle.

ASP.net-MVC otoksen sovelluksen [GetRoleId](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L246) -menetelmä toteuttaa puhelun.

Pyyntö seuraavassa esimerkissä esitetään Azure RBAC rooli tunnuksen hankkiminen. 09cbd307-aa71-4aca-b346-5f253e6e3ebb on tilauksen tunnus.

    GET https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV*****fY2lGc5

Vastaus on seuraavanlainen: 

    HTTP/1.1 200 OK

    {"value":[{"properties":{"roleName":"API Management Service Contributor","type":"BuiltInRole","description":"Lets you manage API Management services, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.ApiManagement/Services/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c","type":"Microsoft.Authorization/roleDefinitions","name":"312a565d-c81f-4fd8-895a-4e21e48d571c"},{"properties":{"roleName":"Application Insights Component Contributor","type":"BuiltInRole","description":"Lets you manage Application Insights components, but not access to them.","scope":"/","permissions":[{"actions":["Microsoft.Insights/components/*","Microsoft.Insights/webtests/*","Microsoft.Authorization/*/read","Microsoft.Resources/subscriptions/resources/read","Microsoft.Resources/subscriptions/resourceGroups/read","Microsoft.Resources/subscriptions/resourceGroups/resources/read","Microsoft.Resources/subscriptions/resourceGroups/deployments/*","Microsoft.Insights/alertRules/*","Microsoft.Support/*"],"notActions":[]}]},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e","type":"Microsoft.Authorization/roleDefinitions","name":"ae349356-3a1b-4a5e-921d-050484c6347e"}]}

Sinun ei tarvitse kutsua tämän Ohjelmointirajapinnan kanssa jatkuvasti. Kun olet määrittänyt roolimääritys tunnetun GUID-tunnus, voit luoda roolin määritys tunnus kuin:

    /subscriptions/{subscription_id}/providers/Microsoft.Authorization/roleDefinitions/{well-known-role-guid}

Seuraavassa on usein käytettyjä valmiit roolit tunnetun GUID:

| Rooli | GUID-tunnus |
| ----- | ------ |
| Lukija | acdd72a7-3385-48ef-bd42-f606fba81ae7
| Avustaja | b24988ac-6180-42A0-ab88-20f7382dd24c
| Virtuaalikoneen avustaja | d73bb868-a0df-4d4d-bd69-98a00b01fccb
| VPN-avustaja | b34d265f-36f7-4a0d-a4d4-e158ca92e90f
| Tallennustilan tilin avustaja | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25
| Sivuston avustaja | de139f84-1756-47ae-9be6-808fbbe84772
| Web-suunnitelma avustaja | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b
| SQL Server avustaja | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437
| SQL DB avustaja | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec

### <a name="assign-rbac-role-to-application"></a>Sovelluksen RBAC roolin määrittäminen

Sinulla on kaikki sinun täytyy määrittää RBAC asianmukainen rooli pääasiallista palveluun käyttämällä [Resurssienhallinta luominen roolimääritys](https://msdn.microsoft.com/library/azure/dn906887.aspx) API.

ASP.net-MVC otoksen sovelluksen [GrantRoleToServicePrincipalOnSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L170) -menetelmä toteuttaa puhelun.

Esimerkki pyyntö, voit määrittää RBAC rooleja sovelluksen: 

    PUT https://management.azure.com/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/microsoft.authorization/roleassignments/4f87261d-2816-465d-8311-70a27558df4c?api-version=2015-07-01 HTTP/1.1

    Authorization: Bearer eyJ0eXAiOiJKV1QiL*****FlwO1mM7Cw6JWtfY2lGc5
    Content-Type: application/json
    Content-Length: 230

    {"properties": {"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2"}}

Pyynnön käytetään seuraavat arvot:

| GUID-tunnus | Kuvaus |
| ------ | --------- |
| 09cbd307-aa71-4aca-b346-5f253e6e3ebb | Tilauksen tunnus
| c3097b31-7309-4c59-b4e3-770f8406bad2 | palvelun lyhennys sovelluksen objektitunnus
| acdd72a7-3385-48ef-bd42-f606fba81ae7 | Lukija tunnus
| 4f87261d-2816-465d-8311-70a27558df4c | Luo uusi roolimääritys uusi GUID-tunnus

Vastaus on seuraavanlainen: 

    HTTP/1.1 201 Created

    {"properties":{"roleDefinitionId":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7","principalId":"c3097b31-7309-4c59-b4e3-770f8406bad2","scope":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb"},"id":"/subscriptions/09cbd307-aa71-4aca-b346-5f253e6e3ebb/providers/Microsoft.Authorization/roleAssignments/4f87261d-2816-465d-8311-70a27558df4c","type":"Microsoft.Authorization/roleAssignments","name":"4f87261d-2816-465d-8311-70a27558df4c"}

### <a name="get-app-only-access-token-for-azure-resource-manager"></a>Käytettävissä ovat vain sovelluksen suojaustunnuksen Azure resurssien hallinta

Vahvista, että sovellus on haluamasi tilauksen on pääsy, suorittaa testin tehtävän tilaukseesi vain sovelluksen tunnuksen.

Saat vain sovelluksen access tunnuksen-osassa [Hae vain sovelluksen suojaustunnuksen Azure AD Graph API](#app-azure-ad-graph), resurssi-parametrin arvona ohjeiden: 

    https://management.core.windows.net/

ASP.NET-MVC-sovelluksen malli [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) -menetelmää saa vain sovelluksen access suojaustunnuksen Azure Resurssienhallinta käyttämällä Active Directory käyttöoikeuksien kirjaston .net.

#### <a name="get-applications-permissions-on-subscription"></a>Sovelluksen oikeuksien saaminen-tilauksessa

Voit tarkistaa, että sovelluksesi on haluamasi oikeudet Azure-tilauksessa, voivat myös soittaa [Resurssin valvontaoikeudet](https://msdn.microsoft.com/library/azure/dn906889.aspx) API. Tämän menetelmän muistuttaa siitä, miten voit määrittää onko käyttäjällä tilauksen käyttöoikeuksien hallinnan oikeudet. Tällä hetkellä Soita kuitenkin käyttöoikeudet Ohjelmointirajapinnan on vain sovelluksen tunnus, jonka olet saanut edellisessä vaiheessa.

ASP.NET-MVC otoksen sovelluksen [ServicePrincipalHasReadAccessToSubscription](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L110) -menetelmä toteuttaa tämän kutsun.

## <a name="manage-connected-subscriptions"></a>Yhdistetyn tilausten hallinta

Kun RBAC asianmukainen rooli on määritetty sovelluksen palveluun pääasiallista tilauksen, sovelluksen voit säilyttää seuranta ja hallinta käyttämällä vain sovelluksen tunnusten Azure Resurssienhallinta.

Tilauksen omistaja poistaa sovelluksen roolimääritys perinteinen portal-tai komentorivin Työkalut-sovellusta ei ole enää käyttää kyseisen tilauksen. Siinä tapauksessa pitäisi lähettää ilmoituksen käyttäjälle, joka on yhteys, jonka tilaus on asiakkaiden pääteistunnot sovelluksen ulkopuolella ja antaa heille "korjaaminen-asetus. "Korjauksen" yksinkertaisesti uudelleen loisi roolimääritys, jotka on poistettu offline-tilassa.

Vain, kun käyttäjä voi muodostaa tilaukset-sovellukseen on otettu käyttöön, sinun on sallittava käyttäjän yhteyden katkaiseminen tilaukset liian. Access hallinta näkökulmasta Katkaise roolimääritys sovelluksen palvelun lyhennys tilauksen sisältävän poistaminen tarkoittaa. Voit myös kaikki tilan tilaus-sovelluksessa voi poistaa liian. Vain käyttäjät, hallinta-käyttöoikeudet, tilauksen voivat katkaista tilaus.

ASP.net-MVC otoksen sovelluksen [RevokeRoleFromServicePrincipalOnSubscription menetelmä](https://github.com/dushyantgill/VipSwapper/blob/master/CloudSense/CloudSense/AzureResourceManagerUtil.cs#L200) toteuttaa puhelun.

Siinä - käyttäjät voivat nyt helposti yhteyden ja niiden sovelluksen Azure tilausten hallinta.

