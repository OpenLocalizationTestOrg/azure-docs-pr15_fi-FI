<properties
    pageTitle="Azure Active Directory ehdollinen käyttösäännöt käyttävät sovellukset | Microsoft Azure"
    description="Ohjausobjektin ehdollisen käyttöoikeuden Azure Active Directory tarkistaa tiettyjen ehtojen todentaessaan käyttäjän ja Salli sovellusten käyttää."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/26/2016"
    ms.author="markvi"/>

# <a name="applications-that-use-conditional-access-rules-in-azure-active-directory"></a>Azure Active Directory ehdollinen käyttösäännöt käyttävät sovellukset

Ehdollinen käyttösäännöt tuetaan Azure Active Directory (Azure AD)-sovellusten yhteydessä, integroitu ennen kuin service (SaaS)-sovellukset-sovelluksia, jotka käyttävät salasanan kertakirjautuminen (SSO), liiketoiminta-sovellusten ja sovelluksia, jotka käyttävät Azure AD-sovelluksen välityspalvelimen liitetyt ohjelmiston. Yksityiskohtainen luettelo, jossa voit käyttää ehdollisia access-sovellusten on artikkelissa [palvelut käyttöön ehdollisen käytön](active-directory-conditional-access-technical-reference.md#Services-enabled-with-conditional-access). Ehdollisen käyttöoikeuden toimii sekä sähköistä tai työpöydän Nykyaikainen todentaminen käyttävien sovellusten kanssa. Tässä artikkelissa kerrotaan mobiili-ja työpöydän ehdollisten access toimii.

Voit käyttää Azure AD-kirjautuminen sivujen Nykyaikainen todentaminen käyttävistä sovelluksista. Kirjaudu sisään-sivulla käyttäjää pyydetään antamaan monimenetelmäisen todentamisen. Viesti on näkyvissä, jos käyttäjän käyttö on estetty. Nykyaikainen todentaminen vaaditaan todentamismenetelmä Azure AD-laitteen niin, että laitteen perustuva ehdollisen käyttöoikeuden käytäntöjä arvioidaan.

On tärkeää tietää, mitkä sovellukset voivat käyttää Ehdollinen käyttösäännöt sekä ohjeet, joita voit joutua suojaamiseen muiden sovellusten pikakuvakkeet tulevat.

## <a name="applications-that-use-modern-authentication"></a>Nykyaikainen todentaminen käyttävät sovellukset

> [AZURE.NOTE] Jos sinulla on ehdollisen käyttöoikeuskäytäntö Azure AD, jossa on sitä vastaavassa Office 365: ssä, määritä sekä ehdollisen käyttöoikeuden käytännöt. Tämä koskee, esimerkiksi ehdollisen käyttöoikeuden käytäntöjä Exchange Online- tai SharePoint Online.

Seuraavat sovellukset tukevat ehdollisen käyttöoikeuden Office 365: ssä ja Azure AD-yhteys service-sovelluksissa:

| Kohde-palvelu  | Käyttöympäristö  | Sovelluksen                                                  |
|--------------|-----------------|----------------------------------------------------------------|
|Office 365: n Exchange Online | Windows 10: ssä|Sähköpostin ja kalenterin/ihmiset-sovellus, Outlook 2016, Outlook 2013 (ja Nykyaikainen todentaminen), Skype for Business (ja Nykyaikainen todentaminen)|
|Office 365: n Exchange Online| Windows 8.1 tai Windows 7: ssä |Outlook 2016: ssa, Outlook 2013 (ja Nykyaikainen todentaminen), Skype for Business (ja Nykyaikainen todentaminen)|
|Office 365: n Exchange Online|iOS-Android|  Outlook mobile-sovellus|
|Office 365: n Exchange Online|Mac OS X| Outlook 2016 for monimenetelmäisen todentamisen ja sijainti. laitteen käytäntöä tuki suunniteltu tulevaisuudessa, Skype for Business-tukeen suunniteltu tulevaisuudessa|
|Office 365: n SharePoint Onlinessa|Windows 10: ssä| Office 2016-sovelluksia, yleinen Office-sovellukset, Office 2013 (ja Nykyaikainen todentaminen), OneDrive for Business-sovellus (seuraavan luonti-synkronointiohjelma tai NGSC) tukee suunniteltu myöhemmin tulevaisuudessa, SharePoint-sovelluksen tuki suunniteltu tulevaisuudessa suunniteltu Office-ryhmien tuki|
|Office 365: n SharePoint Onlinessa|Windows 8.1 tai Windows 7: ssä|Office 2016-sovelluksia, Office 2013 (ja Nykyaikainen todentaminen), OneDrive for Business-sovelluksen (Groove-synkronointiohjelma)|
|Office 365: n SharePoint Onlinessa|iOS-Android|  Office mobile-sovellukset |
|Office 365: n SharePoint Onlinessa|Mac OS X| Office 2016-sovelluksia monimenetelmäisen todentamisen ja sijainti. laitteen käytäntöä tuki suunniteltu tulevaisuudessa|
|Office 365: n Yammer|Windows 10-, iOS- ja Android-laitteeseen | Office Yammer-sovellus|
|Dynamics CRM:|Windows 10: ssä, Windows 8.1, Windows 7, iOS ja Android-laitteeseen | Dynamics CRM-sovellus|
|PowerBI-palvelu|Windows 10: ssä, Windows 8.1, Windows 7, iOS ja Android-laitteeseen | PowerBI-sovellus|
|Azure etäsovelluksen-palvelu|Windows 10: ssä, Windows 8.1, Windows 7, iOS, Android ja Mac OS X |Azure Remote-sovellus|
|Kaikki omat sovellukset app-palvelu|Android- ja iOS|Kaikki omat sovellukset app-palvelu |


## <a name="applications-that-do-not-use-modern-authentication"></a>Sovellukset, jotka eivät käytä Nykyaikainen todentaminen

Tällä hetkellä sinun on käytettävä muita menetelmiä sovellukset, jotka eivät käytä Nykyaikainen todentaminen käytön estäminen. Sovellukset, jotka eivät käytä Nykyaikainen todentaminen käyttösäännöt ei oteta mukaan ehdollisen käyttöoikeuden. Tämä on ensisijaisesti huomiota Exchange- ja SharePoint-käytön. Useimpien sovellusten aiemmissa versioissa käyttää vanhoja access-ohjausobjektin protokollia.

### <a name="control-access-in-office-365-sharepoint-online"></a>Office 365: n SharePoint Onlinessa käyttöoikeuksien hallinta
Voit poistaa vanhoja protokollat SharePoint-käytön määrittäminen SPOTenant cmdlet-komennolla. Tämä cmdlet-komennon avulla voit estää Office-asiakasohjelmat, jotka käyttävät Moderni todennusprotokollia käyttämästä SharePoint Online-resursseja.

**Esimerkkikomento**:    `Set-SPOTenant -LegacyAuthProtocolsEnabled $false`

### <a name="control-access-in-office-365-exchange-online"></a>Office 365: n Exchange Onlinessa käyttöoikeuksien hallinta

Exchange on protokollat tärkeimmät kahteen luokkaan. Tarkista seuraavat asetukset ja valitse sitten käytännön, joka on organisaatiollesi.

-   **Exchange ActiveSync**. Oletusarvon mukaan monimenetelmäisen todentamisen ja sijainnin ehdollinen käytäntöjen ei säilytetä, kun Exchange ActiveSync. Haluat suojata näiden palvelujen määrittäminen Exchange ActiveSync-käytännössä suoraan tai estämällä Exchange ActiveSync Active Directory Federation Services (AD FS) sääntöjen avulla.
-   **Vanha protokollat**. Voit estää vanha protokollat AD FS. Tämä estää pääsyn vanhempi Office-asiakasohjelmat, kuten Office 2013: n ilman nykyaikaista todennus on käytössä, ja Officen aiemmissa versioissa.


### <a name="use-ad-fs-to-block-legacy-protocol"></a>Estä vanha protokolla AD FS avulla

Seuraavassa esimerkissä sääntöjen avulla voit estää vanha protokolla AD FS tasolla. Valitse kaksi yleisiä määrityksiä.

#### <a name="option-1-allow-exchange-activesync-and-allow-legacy-apps-but-only-on-the-intranet"></a>Vaihtoehto 1: Salli Exchange ActiveSync ja sallia vanhojen sovellusten, mutta vain intranetissä

Lisäämällä seuraavat kolme säännöt käyttäisit osapuolen luota Microsoft Office 365: n Identity-ympäristössä, Exchange ActiveSync-liikenne ja selaimen ja Nykyaikainen todentaminen liikenne AD FS on käyttöoikeudet. Vanhojen sovellusten on estetty ekstranet.

##### <a name="rule-1"></a>Säännön 1

    @RuleName = “Allow all intranet traffic”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-2"></a>Sääntö 2

    @RuleName = “Allow Exchange ActiveSync ”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

##### <a name="rule-3"></a>Sääntö 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

#### <a name="option-2-allow-exchange-activesync-and-block-legacy-apps"></a>Vaihtoehto 2: Salli Exchange ActiveSync ja estää vanhojen sovellusten

Lisäämällä seuraavat kolme säännöt käyttäisit osapuolen luota Microsoft Office 365: n Identity-ympäristössä, Exchange ActiveSync-liikenne ja selaimen ja Nykyaikainen todentaminen liikenne AD FS on käyttöoikeudet. Vanhojen sovellusten torjuttavien mistä tahansa sijainnista.

##### <a name="rule-1"></a>Säännön 1

    @RuleName = “Allow all intranet traffic only for browser and modern authentication clients”
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Sääntö 2

    @RuleName = “Allow Exchange ActiveSync”
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Sääntö 3

    @RuleName = “Allow extranet browser and browser dialog traffic”
    c1:[Type == " http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");
