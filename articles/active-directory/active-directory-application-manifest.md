<properties
   pageTitle="Tietoja Azure Active Directory-sovellusluettelo | Microsoft Azure"
   description="Azure Active Directory-sovellusluettelo, joka edustaa Azure AD-vuokraajan sovelluksen tunnistetietojen määrittäminen ja helpottaa OAuth lupa, suostumus kokemus ja muut yksityiskohtaiset soveltamisala."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Tietoja Azure Active Directory-sovellusluettelo

Sovellukset, jotka integroida ja Azure Active Directory (AD) rekisteröitävä Azure AD-vuokraajan, tarjoavat pysyvä identity-määritys-sovelluksen. Tässä määrityksessä on kuulla suorituksen tilanteita, joissa Salli ulkoistaa ja broker todennus/luvan kautta Azure AD sovelluksen ottaminen käyttöön. Lisätietoja Azure AD-sovelluksen malli on [lisääminen, päivittäminen, ja poistamalla sovelluksen] [ ADD-UPD-RMV-APP] artikkelissa.

## <a name="updating-an-applications-identity-configuration"></a>Päivittää sovelluksen tunnistetietojen määrittäminen

Voidaan päivittää sovelluksen tunnistetietojen määritysten ominaisuudet, jotka vaihtelevat ominaisuuksia ja/tai vaikeuksiin, mukaan lukien seuraavat astetta on useita vaihtoehtoja:

- ** [Azure perinteinen portaalin] [ AZURE-CLASSIC-PORTAL] WWW-käyttöliittymän** avulla voit päivittää sovelluksen yleisimmät ominaisuudet. Tämä on nopein ja vähiten virhe voi enää tapa päivitetään sovelluksen ominaisuuksia, mutta ei anna täydet oikeudet kaikki ominaisuudet, kuten seuraava kahdella tavalla.
- Monimutkaisemman tilanteessa, jossa sinun on päivitettävä, eikä niitä julkaista Azure perinteinen portaalissa ominaisuudet voit muokata **sovelluksen luettelon**. Tämä on tämän artikkelin kohdistuksen ja kerrotaan tarkemmin seuraavan osion alkaen.
- On myös mahdollista **Kirjoita sovellus, joka käyttää [Graph API] [ GRAPH-API] ** Päivitä sovellus, jossa pyydetään eniten työmäärään. Voi olla houkutteleva vaihtoehto, mutta jos kirjoittamisen hallintaohjelmisto tai on päivitettävä palvelusovelluksen ominaisuuksien säännöllisesti automaattinen tavalla.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Sovelluksen luettelo avulla voit päivittää sovelluksen tunnistetietojen määrittäminen
Palvelun [Azure perinteinen portal][AZURE-CLASSIC-PORTAL], voit hallita sovelluksen tunnistetietojen määritys lataamalla ja tiedoston lataamisessa JSON esitys, jota kutsutaan sovellusluettelo. Ei ole todellinen tiedosto on tallennettu hakemistossa. Sovelluksen luettelo on pelkästään HTTP GET toiminto Azure AD Graph API sovelluksen oletustoimintoa ja lataus on HTTP KORJAUSTIEDOSTON sovelluksen kohteen toiminnon.

Tuloksena jotta ymmärrät muoto ja ominaisuudet sovellusluettelo, sinun on viitata Graph API- [sovelluksen kohteen] [ APPLICATION-ENTITY] ohjeissa. Esimerkkejä päivitykset, jotka voivat tehdä mutta sovelluksen luettelon Lataa:

- Verkko-Ohjelmointirajapinnan näyttämiä **Declare käyttöoikeuksien käyttöalueet (oauth2Permissions)** [Azure Active Directory-integrointi sovelluksiin] "Paljastaa API, muut verkkosovellusten" on ohjeaiheessa[ INTEGRATING-APPLICATIONS-AAD] käyttöönoton käyttäjän tekeytyminen tietojen avulla oauth2Permissions valtuutetun käyttöoikeuksien laajuus. Kuten edellä mainittiin, sovelluksen kohteen ominaisuudet on kuvattu Graph API [kohde ja monimutkaisia tyyppi] [ APPLICATION-ENTITY] artikkelissa, mukaan lukien oauth2Permissions-ominaisuutta, joka on kokoelma tyyppi [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Sovelluksen näyttämiä Declare sovelluksen roolit (appRoles)**. Sovelluksen kohteen appRoles-ominaisuus on kokoelma tyyppi [AppRole][APPLICATION-ENTITY-APP-ROLE]. Katso [Roolipohjainen käyttöoikeuksien valvonta cloud sovellusten Azure AD-] [ RBAC-CLOUD-APPS-AZUREAD] artikkelin Esimerkki käyttöönotto.
- **Declare tunnetut asiakkaan sovellusten (knownClientApplications)**, jonka voit liittää loogisesti resource/Web API määritetty asiakas-sovellukset suostumuksen.
- **Pyytää antamaan ryhmän jäsenyyksiä väittää Azure AD** -kirjautuneena käyttäjän (groupMembershipClaims).  Tämä voi määrittää myös antaa vaateet tietoja käyttäjän kansion roolien jäsenyyksiä. Kohdassa [luvan Cloud sovelluksissa AD-ryhmien käyttäminen] [ AAD-GROUPS-FOR-AUTHORIZATION] artikkelin Esimerkki toteutuksen.
- **Salli sovelluksen tukemaan OAuth 2.0 implisiittinen myöntää** kulkee (oauth2AllowImplicitFlow). Myönnä työnkulku tämän tyyppistä käytetään upotetun JavaScript-web-sivujen tai yksittäisen sivun sovellusten (SPA). Katso lisätietoja implisiittinen käyttöoikeuksien myöntämistä koskevan [tietoja implisiittinen OAuth2 myöntää työnkulku Azure Active Directoryn][IMPLICIT-GRANT].
- **Ota käyttöön käyttäminen X509 varmenteet salausavaimen nimellä** (keyCredentials). Katso [Office 365-palvelun ja suodatinohjelman sovellusten luominen] [ O365-SERVICE-DAEMON-APPS] ja [Kehittäjän ohje auth Azure Resurssienhallinta API] [ DEV-GUIDE-TO-AUTH-WITH-ARM] toteutuksen esimerkkejä koskevat artikkelit.
- **Lisää uusi sovellus-tunnus URI** sovelluksen (identifierURIs[]). Sovelluksen tunnus URI käytetään yksilöivät sovelluksen sen Azure AD-vuokraajan (tai useita Azure AD-alihallinnoista, usean vuokraajan skenaarioissa hyväksymisen jälkeen mukautettu vahvistama kautta). Niitä käytetään, kun pyytää resurssin sovelluksen tai hankkiminen access-tunnuksen, resurssi-sovelluksen käyttöoikeuksia. Kun päivität tämä elementti, sama päivitys tehdään vastaavan palvelun lyhennys servicePrincipalNames [] kokoelma, jolla sovelluksen koti vuokraajan sijaitsevat.

Sovelluksen luettelo sisältää myös erinomainen tapa sovelluksen rekisteröinti tilan seuranta. Koska se on käytettävissä JSON-muodossa, tiedosto-esitys voidaan tarkistaa lähde-ohjausobjektiin ja sovelluksen lähde-koodin.

## <a name="step-by-step-example"></a>Suorita vaihe esimerkin mukaan
Nyt avulla, käy läpi vaiheet, tarvitse päivittää sovelluksen tunnistetietojen määritys sovellusluettelo kautta. Olemme korostaa jokin edellä olevassa esimerkissä esittää, kuinka määritellä uuden käyttöoikeuksia vaikutusalueen resurssi-sovelluksessa:

1. Siirry [Azure perinteinen portal] [ AZURE-CLASSIC-PORTAL] ja kirjaudu sisään tilille, jolla on palvelun järjestelmänvalvoja tai muiden järjestelmänvalvojan oikeudet.

2. Kun olet todennettu, alaspäin ja valitse vasemmanpuoleisessa ruudussa (1) Azure "Active Directory-tunniste sitten napsauttamalla sovelluksen missä Azure AD-vuokraajan rekisteröity (2).

    ![Valitse Azure AD-vuokraajan][SELECT-AZURE-AD-TENANT]

3. Hakemistosivun tullessa näkyviin napsauttamalla "Sovellusten" (1) löydät luettelon sovellukset vuokraajan rekisteröity-sivulla. Etsi sovellus, jonka luettelosta ja valitse sitten sitä (2).

    ![Valitse Azure AD-vuokraajan][SELECT-AZURE-AD-APP]

4. Nyt kun olet valinnut sovelluksen pääsivulta, Huomaa "Hallinta luettelon"-toiminto (1)-sivun alareunaan. Jos valitset tämän linkin, voit pyydetään lataaminen ja lataa JSON luettelon tiedosto. Valitse "Lataa luettelo" (2). Tämä noudatettava välittömästi ja lataa vahvistusikkuna kehottaa vahvistamaan valitsemalla "Lataa luettelon" (3), ja valitse joko Avaa tai Tallenna tiedosto paikallisesti (4).

    ![Hallitse luettelo, lataa vaihtoehto][MANAGE-MANIFEST-DOWNLOAD]

    ![Lataa luettelo][DOWNLOAD-MANIFEST]

5. Tässä esimerkissä on tallentanut tiedoston paikallisesti salliminen us editorin avaaminen, muutosten tekeminen JSON ja tallenna se uudelleen. Seuraavassa on esimerkiksi näyttää JSON rakenteen sisällä Visual Studio JSON-editorin. Huomautus se seuraa rakenteen [sovelluksen kohteen] [ APPLICATION-ENTITY] kuin mainittiin:

    ![Päivitä luettelon JSON][UPDATE-MANIFEST]

    Esimerkiksi jos haluamme toteuttaminen/paljastus uuden käyttöoikeustason luominen kutsua "Employees.Read.All" tämän resurssin-sovellus (API), riittää, että Lisää uusi/toisen osan oauth2Permissions-kokoelmaan ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Tapahtuma on oltava yksilöllinen ja on siksi Luo uusi yleisesti yksilöllinen tunnus (GUID) varten `"id"` ominaisuus. Tässä tapauksessa koska on määritetty `"type": "User"`, voit suostunut nämä käyttöoikeudet, jossa on rekisteröity resource/API-sovellus vuokraaja Azure AD-todennetut tilien mukaan. Asiakkaan myönnetään sovelluksen oikeutta käyttää asiakkaan puolesta. Kuvaus ja Näytä nimi merkkijonot käytetään aikana suostumus ja näyttää Azure perinteinen-portaalissa.

6. Kun olet valmis luettelo päivitetään, palaa Azure perinteinen portaalissa Azure AD sovellus-sivulle ja valitse "Hallinta luettelon" toiminto uudelleen (1). Et tällä hetkellä määrittänyt "Lataa luettelon" (2). Lataamisen tavoin sinun näyttää, mihin kohtaan uudelleen toinen valintaikkuna, jossa kehotetaan JSON-tiedoston sijainti. "Etsi tiedosto..." (3), valitse "Valitse tiedoston haluat ladata"-valintaikkunan avulla voit valita JSON-tiedosto (4) ja paina "Avaa". Kun valintaikkuna häviää näkyvistä, valitse "OK" rasti (5) ja että luettelo voi ladata.  

    ![Hallitse luettelo, lataa vaihtoehto][MANAGE-MANIFEST-UPLOAD]

    ![Lataa luettelon JSON][UPLOAD-MANIFEST]

    ![Lataa luettelon JSON - vahvistus][UPLOAD-MANIFEST-CONFIRM]

Nyt kun luettelo on tallennettu, voit tarkastella rekisteröidyn asiakkaan sovelluksen pääsy on lisätty yli uuden käyttöoikeustason. Nyt voit käyttää Azure perinteinen portal-Web-Käyttöliittymän sijaan muokkaaminen asiakassovelluksen luettelo:  

1. Ensin Siirry "Määrittäminen"-sivulle, johon haluat lisätä uuden ohjelmointirajapinnan access sovelluksen ja valitsemalla "Lisää sovellus"-painiketta.
2. Valitse näyttöön tulee vuokraajan sovellusten rekisteröity resurssi (API) luettelon kanssa. Valitse plus tai + -symbolilla resurssi-sovelluksen nimen napsauttamalla sitä.  
3. Valitse oikeassa alakulmassa valintamerkki.
4. Kun palaat määräytyy asiakkaan määrityssivulla "Lisää sovellus"-osaan, näet uusi resurssi-sovellus-luettelossa. Jos pidät osoitinta kyseisen rivin oikealla "Valtuutetun käyttöoikeudet"-osan päälle, näet näkyvät pudotusvalikosta. Napsauta luetteloa ja valitse sitten uuden käyttöoikeustason, jotta lisääminen asiakkaan pyydetty luettelon käyttöoikeuksia. Huomautus: Tämä uuden käyttöoikeustason tallennetaan asiakassovellukseen tunnistetietojen määrityksessä "requiredResourceAccess" sivustokokoelman-ominaisuudessa.

![Muiden sovellusten käyttöoikeudet][PERMS-TO-OTHER-APPS]

![Muiden sovellusten käyttöoikeudet][PERMS-SELECT-APP]

![Muiden sovellusten käyttöoikeudet][PERMS-SELECT-PERMS]

Se on. Nyt sovellusten suoritetaan käyttämällä niiden uusien käyttäjätietojen-asetuksiin.

## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja sovelluksen sovelluksen ja palvelun lyhennys objektit välinen suhde on kohdassa [sovellus ja palvelun pääasiallista objektien Azure AD-][AAD-APP-OBJECTS].
- Katso [Azure AD developer sanasto] [ AAD-DEVELOPER-GLOSSARY] core Azure Active Directory (AD) developer käsitteitä määritelmiä.

Auta meitä tarkentaminen ja muodon Microsoftin sisältöä ja antaa palautetta Käytä alla DISQUS kommentit-kohtaan.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

