<properties
    pageTitle="Azure Active Directory-B2C: Usein kysyttyjä kysymyksiä | Microsoft Azure"
    description="Usein kysyttyjä kysymyksiä Azure Active Directory-B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory-B2C: usein kysytyt kysymykset

Tällä sivulla vastauksia usein kysyttyihin kysymyksiin B2C Azure Active Directory (Azure AD). Säilytä tarkistaa päivitykset.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Azure AD B2C ominaisuuksien käyttäminen aiemmin luotujen, työntekijän Azure AD alihallintaympäristössä

Tällä hetkellä Azure AD B2C ominaisuuksia ei voi ottaa käyttöön aiemmin Azure AD-vuokraajan. On suositeltavaa luoda erilliset kohdevuokraajassa, jotta voit hallita lukijoille Azure AD B2C ominaisuuksien avulla.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Voinko käyttää Azure AD B2C antamaan sosiaalisen login (Facebook ja Google +) Office 365: een?

Azure AD-B2C ei voi käyttää Microsoft Office 365: n kanssa. Yleinen sitä ei voi käyttää käyttöoikeuden SaaS sovellusten (Office 365: ssä, Salesforce, työpäivä jne.). Tarjoaa tunnistetietojen ja käytön hallinta vain kuluttaja osoittava web- ja mobile-sovellusten ja ei koske työntekijöiden tai kumppanilta skenaarioita.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Mitä ovat paikalliset tilit Azure AD B2C? Miten poikkeavat ne työpaikan tai oppilaitoksen tilit Azure AD?

Azure AD-vuokraajan, jokaiselle käyttäjälle vuokraajan (paitsi käyttäjät, joilla on aiemmin Microsoft-tilit) kirjautuu sisään sähköpostiosoitteen lomakkeen `<xyz>@<tenant domain>`, jossa `<tenant domain>` on yksi tarkistetut toimialueet vuokraajan tai alkuperäisen `<...>.onmicrosoft.com` toimialueen. Tämän tilin tyyppi on työpaikan tai oppilaitoksen tiliä.

Azure AD B2C-vuokraajaan useimpien sovellusten haluat käyttäjä voi kirjautua minkä tahansa haluamaansa sähköpostiosoite (esimerkiksi joe@comcast.net, bob@gmail.com, sarah@contoso.com, tai jim@live.com). Tämän tilin tyyppi on paikallinen tili. Nykyään myös tuetaan haluamaansa käyttäjänimet (merkkijonoja, vain Normaali) kuin paikalliset tilit (esimerkiksi Esko, Teemu, Heidi tai jim). Voit valita jonkin nämä kaksi paikallisen tilityypit Azure AD B2C-palvelussa.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Mitkä sosiaalisten tunnistetietojen Toimittajat Voit tukevat nyt? Mitkä aiot tukee tulevaisuudessa?

Sovellus tukee tällä hetkellä Facebook, Google + LinkedIn tai Amazon. Lisätään muiden Suositut sosiaalisen tunnistetietojen toimittajat, perustuu asiakkaan demand tuki.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Hakemaan lisätietoja kuluttajille eri sosiaalisen tunnistetietojen toimittajat käyttöalueen määrittäminen

Ei, mutta tämä ominaisuus on käytössä sekä ohjeet. Tuettujen sosiaalisten tunnistetietojen toimittajat määrittämällä käytettävät oletusarvon käyttöalueen ovat seuraavat:

- Facebook: sähköpostin
- Google +: sähköpostin
- Microsoft-tili: openid sähköpostiprofiilin
- Amazon: profiilin
- LinkedIn: r_emailaddress r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Onko sovelluksessa voidaan suorittaa Azure varten Azure AD B2C kanssa voidaan käyttää?

Ei, voit isännöidä sovelluksesi missä tahansa (-pilvi tai paikalliseen). Azure AD B2C vuorovaikutuksessa tarvitaan ei voi lähettää ja vastaanottaa HTTP-pyyntöjen kaikkien käytettävissä päätepisteet.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Minulla on useita Azure AD B2C Alihallinnat. Miten voin hallita niitä Azure-portaalissa?

Kunkin Azure AD B2C vuokraajan on oma B2C ominaisuuksia sivu Azure-portaalissa. Katso [Azure AD B2C: Rekisteröi sovelluksesi](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) kerrotaan, miten voit siirtyä tiettyyn vuokraajan B2C ominaisuuksia sivu Azure-portaalissa. Siirtyminen Azure AD B2C kansioiden Azure-portaalissa ei pidä sivu Avaa useimmissa selaimissa B2C ominaisuudet.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Miten vahvistus sähköpostit mukauttaa (sisällön ja "-:" kentän) Azure AD B2C lähettämä?

[Yrityksen yrityksen tunnuksen ominaisuuden](../active-directory/active-directory-add-company-branding.md) avulla voit mukauttaa vahvistus sähköpostit sisältöä. Tarkemmin sanottuna nämä kaksi elementit sähköpostiviestin voi mukauttaa:

- **Ilmoituspalkin Logo**: näkyvät oikeassa alareunassa.
- **Taustaväri**: yläpuolella näkyvä.

    ![Näyttökuva mukautetun vahvistussähköposti](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Sähköpostin allekirjoitus on B2C vuokraajan nimi, jonka ilmoitit B2C vuokraajan luomisen yhteydessä. Voit muuttaa nimen seuraavia ohjeita:

- Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com/) tilauksen järjestelmänvalvojana.
- Siirry B2C alihallintaan.
- Valitse **Määritä** -välilehdessä.
- Muuta **kansion ominaisuudet** -osan **nimi** -kenttään.
- Valitse sivun alareunassa **Tallenna** .

Tällä hetkellä tällä ei voi muuttaa "Lähettäjä:" sähköposti-kenttään. Jos olet kiinnostunut tätä ominaisuutta ja mukauttaminen täysin vahvistussähköposti leipätekstiin, äänestää [Palautesivuston](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails)käyttöön.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Miten voi voin siirtää aiemmin käyttäjänimet, salasanat ja profiilit tietokantani Azure AD B2C?

Azure AD-kaavio-Ohjelmointirajapinnan avulla voit kirjoittaa siirtotyökalua käytetään. Katso lisätietoja [Graph API-malli](active-directory-b2c-devquickstarts-graph-dotnet.md) . Microsoft toimittaa eri siirron asetukset ja työkalut ulos,-valmiilla tulevaisuudessa.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Mitä Salasanakäytäntö käytetään Azure AD B2C paikalliset tilit?

Azure AD B2C Salasanakäytäntö paikallisten tilien perustuu Azure AD käytäntö. Azure AD-B2C käyttäjän kirjautuminen, ilmoittautuminen tai Kirjaudu sisään- ja salasana Palauta käytännöt käyttää "vahva" salasana voimakkuus ja ei vanhene luodut salasanat. Lue lisätietoja [Azure AD-Salasanakäytäntö](https://msdn.microsoft.com/library/azure/jj943764.aspx) .

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Siirtää kuluttaja käyttäjätietoja, jotka on tallennettu oman paikallisen Azure AD B2C Active Directoryn Azure AD Connect avulla?

Ei, Azure AD Connect ei ole suunniteltu toimimaan Azure AD B2C. Microsoft toimittaa eri siirron asetukset ja työkalut ulos,-valmiilla tulevaisuudessa.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Toimiiko Azure AD B2C kuten Microsoft Dynamics CRM järjestelmien kanssa?

Tällä hetkellä. Paikallisen ympäristön integroinnissa järjestelmät on Microsoftin ohjeet.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Onko Azure AD B2C toimivat paikallinen SharePoint 2016 ja vanhemmat versiot?

Tällä hetkellä. Azure AD-B2C ei ole SAML 1.1-tunnuksia, jotka portaaleihin ja e-kaupankäyntisovellusten sisäisten SharePoint-ympäristöön tarve tuki. Huomaa, että Azure AD B2C ei ole tarkoitettu SharePoint ulkoisen kumppanin jakaminen skenaarion; Katso [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) sijaan.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Kannattaa käyttää Azure AD B2C tai B2B ulkoisen jäsenyyksiä hallitaan?

Lue tästä artikkelista lisätietoja tarvittavat ominaisuudet soveltaminen ulkoisten käyttäjätietojen skenaariot [ulkoisten käyttäjätietojen](../active-directory/active-directory-b2b-compare-external-identities.md) tietoja.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Tietoja raportointia ja valvonta ominaisuuksia Azure AD B2C antaa? Ovatko ne samoja kuin Azure AD Premium?

Ei, Azure AD B2C ei tue raporttien samat kuin Azure AD Premium. Azure AD-B2C vapauttaminen basic raportointi ja valvonta ohjelmointirajapinnan pian.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Sivujen served mukaan Azure AD B2C Käyttöliittymän lokalisoidaan Mitä kieliä voi käyttää?

Tällä hetkellä Azure AD B2C on tarkoitettu englanniksi vain. Emme suunnittele aloittamaan lokalisoinnin ominaisuuksia mahdollisimman pian.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Omaa URL-osoitteet käyttää Omat kirjautumista ja kirjaudu sisään-sivuilla, jotka avautuvat Azure AD B2C Esiintymän voit muuttaa URL-osoite-login.microsoftonline.com login.contoso.com?

Tällä hetkellä. Tämä ominaisuus on Microsoftin ohjeet. Huomaa, että alihallintaan Azure perinteinen-portaalin **Toimialueet** -välilehdessä toimialueen vahvistamisen seuraavasti.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Miten voin poistaa Azure AD B2C alihallintaan?

Voit poistaa Azure AD B2C vuokraajan seuraavasti:

- Seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa.
- Siirry **sovellukset**, **tunnistetietojen toimittajat** ja **kaikki käytännöt** näiden ja poistaa kaikki merkinnät kullekin niistä.
- Kirjaudu nyt [Azure perinteinen portal](https://manage.windowsazure.com/) -tilauksen järjestelmänvalvojana. (Tämä on sama työpaikan tai oppilaitoksen tilille tai samalle Microsoft-tilille, jota käytit Azure rekisteröityminen.)
- Siirry vasemmalle Active Directory-tunniste ja valitse B2C alihallintaan.
- Valitse **käyttäjät** -välilehti.
- Valitse kunkin käyttäjän ottaminen käyttöön (olet kirjautunut, eli tilauksen järjestelmänvalvojan käyttäjän pois lukien). Valitse sivun alareunassa **Poista** ja valitse **Kyllä** pyydettäessä.
- Valitse **sovellukset** -välilehti.
- Valitse **sovellusten yritykseni omistaa** **Näytä** avattavan luettelon kenttään ja valitse sitten valintamerkkiä.
- Näet sovelluksen nimeltä **b2c-laajennukset-sovelluksen** ohjeita. Valitse sivun alareunassa **Poista** ja valitse **Kyllä** pyydettäessä.
- Siirry Active Directory-tunniste uudelleen ja valitse B2C alihallintaan.
- Valitse sivun alareunassa **Poista** . Viimeistele prosessi ohjeiden mukaisesti.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Mistä saan Azure AD-B2C osana yrityksen Mobility-ohjelmiston?

Ei, Azure AD B2C on ryhmävakuutussopimukset Azure-palvelu ja ei ole osa yrityksen Mobility.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Miten voin raportoida ongelmista Azure AD B2C?

Lisätietoja [tukevat Azure Active Directory-B2C tarjouspyyntöjen tiedostossa](active-directory-b2c-support.md).

## <a name="more-information"></a>Lisätietoja

Voit myös haluta ja Tarkista nykyisen [palvelurajoitukset, rajoitukset ja rajoitukset](active-directory-b2c-limitations.md).
