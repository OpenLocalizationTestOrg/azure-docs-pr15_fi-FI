<properties
    pageTitle="Kertakirjauksen hallinta sovellusten Azure Active Directory-esikatselussa yksittäinen | Microsoft Azure"
    description="Lue, miten voit hallita kertakirjauksen sovellusten Azure Active Directoryn avulla"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Esikatselu: Kertakirjautumisen, uusi Azure-portaalissa sovellusten hallinta

> [AZURE.SELECTOR]
- [Azure portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure perinteinen portal](active-directory-sso-integrate-saas-apps.md)

Tässä artikkelissa käsitellään [Azure portal](https://portal.azure.com) avulla voit hallita yksittäisen Sign-asetusten sovellukset, erityisesti lähimpään kokonaislukuun, joka on lisätty [Azure Active Directory (Azure AD)-sovelluksen valikoima](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Azure AD-hallintatoimintojen, kertakirjautuminen on parhaillaan julkinen esikatselu ja tässä artikkelissa kuvataan uusista ominaisuuksista sekä seuraavat tilapäinen rajoitukset, jotka ovat paikallaan vain esikatselu ajanjakson aikana. [Mikä on esikatselussa?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Uuden portaalin sovelluksia etsiminen

Vuodesta syyskuu 2016 kaikissa sovelluksissa, jotka on määritetty single Sign-kansioon, [Azure Active Directory-sovelluksen valikoiman](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) sisällä [Azure perinteinen portal](https://manage.windowsazure.com)directory järjestelmänvalvojan, nyt voi tarkastella ja hallitun Azure-portaalissa.

Nämä sovellukset löytyy **Yrityksen** osassa Azure portal-linkki, johon löytyvät [portal](https://portal.azure.com) **Lisää palvelut** -luettelossa. Sovellusten ovat sovellukset, jotka on otettu käyttöön ja organisaatiosi käyttäjät ovat käytössä.

![Yrityksen sivu][1]

Valitse **kaikkien sovellusten** luettelo kaikki sovellukset, jotka on määritetty, mukaan lukien sovellukset, jotka on lisätty valikoimasta. Resurssi-sivu, jossa raportteja voi tarkastella, sovelluksen ja erilaisia asetuksia voi hallita sovelluksen sovelluksen valitsemalla Lataa.

Voit hallita kertakirjauksen asetukset valitsemalla **kertakirjautumisen**.

![Sovelluksen resurssi-sivu][2]


##<a name="single-sign-on-modes"></a>Sign-tilassa

**Kertakirjautumisen** sivu alkaa **tila** -valikon, jonka avulla yhden Sign tila on määritetty. Käytettävissä olevat vaihtoehdot ovat:

* **SAML-pohjaisen etumerkin** - asetus on käytettävissä, jos sovellus tukee koko organisaation ulkopuolisen kertakirjautumisen Azure Active Directory-SAML 2.0-protokollan avulla.

* **Salasana-pohjainen etumerkin** - asetus on käytettävissä, jos Azure AD tukee tämän sovelluksen lomakkeiden salasana.

* **Linkitetyt sisäänkirjautuminen** - tunnettiin aiemmin nimellä "Aiemmin luotuun kertakirjautumisen"-vaihtoehto avulla järjestelmänvalvojat voivat tämän sovelluksen linkki sijoitetaan niiden käyttäjän Azure AD Access-paneelin tai Office 365: n sovellusten käynnistys.

Saat lisätietoja kolmesta tilasta [miten yksittäinen Kirjaudu sisään Azure Active Directory-työ](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>SAML-pohjaisen etumerkin

**SAML-pohjaisen etumerkin** -asetus näyttää sivu, joka on jaettu seuraaviin osiin:

###<a name="domains-and-urls"></a>Toimialueet ja URL-osoitteet
Tämä on kohtaa, johon kaikki tiedot sovelluksen toimialueen ja URL-osoitteet lisätään Azure AD-kansio. Kaikkien syötteiden tarvitse ottaa Sign työn sovelluksen näkyvät suoraan näytön kohtaan, valitse olisi kaikkien valinnainen syötteiden voidaan tarkastella valitsemalla **Näytä URL-osoite Lisäasetukset** -valintaruutu. Tuetut syötteiden täysi luettelo sisältää:

* **Merkki-URL-Osoitteen** – kohtaa, johon käyttäjä siirtyy sisäänkirjautumisongelmien tämän sovelluksen. Jos sovellus on määritetty suorittamaan palvelun tarjoajan käynnistämä yksittäinen merkki, sitten kun, jos käyttäjä siirtyy tätä URL-Osoitetta, palveluntarjoaja tekee tarvittavat uudelleenohjaus Azure AD todennusta ja käyttäjän kirjautuminen. Jos tämä kenttä täytetään, Azure AD käyttäminen Office 365: ssä ja Azure AD Access-paneelin sovellusta käynnistettäessä tätä URL-Osoitetta. Jos tämä kenttä jätetään pois ja valitse sen sijaan suorittaa tunnistetietojen toimittaja Azure AD-omaan merkki, kun sovellus käynnistetään Office 365-Azure AD Access-paneelin tai Azure AD yksittäinen Sign URL.

* **Tunniste** - tämä URI pitäisi yksilöllisesti, sovelluksen mitä yhden sisäänkirjautuminen määritetään. Tämä on arvo, joka Azure AD lähettää takaisin käyttämisen yleisön parametrina SAML-tunnuksen ja sovelluksen odotetaan tarkistamista. Tämä arvo näkyy myös sovelluksen SAML metatietoihin kohteen tunnus.

* **Vastaa URL-osoite** - vastaa URL-osoite on, jossa sovellus odottaa vastaanottamaan SAML-tunnuksen. Tätä kutsutaan myös vahvistus kuluttaja Service (ACS) URL-osoite. Sen jälkeen, kun ne on syötetty, valitse Seuraava Jatka seuraavalle sivulle. Tämä näyttö tietoja siitä, mitä varten on määritettävä, jotta se voi hyväksyä SAML-tunnuksen Azure AD-sovelluksen reunassa.

* **Välityksen tila** - välityksen tila on valinnainen parametri, jotka auttavat kertovat sovelluksen missä ohjaa käyttäjän todennusta päättymisen jälkeen. Yleensä arvo on kelvollinen URL-osoite sovelluksen, mutta jotkin sovellukset tätä kenttää käytetään eri tavalla (Lisätietoja sovelluksen kertakirjauksen ohjeissa). Mahdollisuus määrittää välityksen tila on uusi ominaisuus, joka on yksilöivä uusi Azure-portaaliin.

###<a name="user-attributes"></a>Käyttäjän määritteet
Tämä on, jossa järjestelmänvalvojat voivat tarkastella ja muokata määritteitä, jotka lähetetään SAML tunnus, jonka Azure AD-ongelmien vianmääritys sovelluksen kunkin aika käyttäjät kirjautua sisään.

Ensimmäinen preview-versio tukee vain muokattavan määrite on **Käyttäjätunnus** -määrite. Tämän määritteen arvon on kenttä, joka yksilöi kunkin käyttäjän sovelluksessa Azure AD. Esimerkiksi jos sovellus on otettu käyttöön käyttäen "sähköpostiosoite" käyttäjänimi ja yksilöllinen, valitse arvo määritettävä "user.mail"-kenttään Azure AD.

Muokkaamisen määritteitä tueta myöhemmin esikatselu.

###<a name="saml-signing-certificate"></a>SAML allekirjoitusvarmenne
Tässä osassa esitellään Azure AD allekirjoittamiseen SAML paikkamerkkejä, jotka on myönnetty aina, kun käyttäjä todentaa sovelluksen käyttämä varmenteen tietoja. Se sallii ominaisuuksien nykyinen sertifikaatti tarkastaa, mukaan lukien päättymispäivän.

Mahdollisuus palauttaminen varmenne ja muita asetuksia tueta myöhemmin preview-versio varmenteiden hallinta. Huomaa, että koko hallinta varmenteet voidaan suorittaa edelleen [Azure perinteinen portal](active-directory-sso-certs.md).

###<a name="application-configuration"></a>Sovelluksen määritys
Viimeisessä osassa on ohjeet ja/tai ohjausobjektien määritettävä itse sovelluksen käyttämään Azure Active Directory tunnistetietojen toimittaja.

**Määritä sovelluksen** esiin tulevasta valikosta on uuden sovelluksen määrittäminen tiiviin ja upotetun ohjeita. Tämä on toinen uusi ominaisuus yksilöllinen uusi Azure-portaaliin.

> [AZURE.NOTE] Esimerkki upotetun asiakirjat-kohdassa Salesforce.com-sovelluksen. Lisäsovellukset ohjeissa lisätään jatkuvasti aikana esikatselu.

![Upotetut tiedostot][3]

##<a name="password-based-sign-on"></a>Salasana-pohjainen etumerkin
Jos sovelluksen tuettu, salasana-pohjainen SSO-tilassa ja valitsemalla **Tallenna** välittömästi määrittää sen tekemään salasana-pohjainen SSO. Saat lisätietoja käyttöönotto salasana-pohjainen SSO [miten yksittäinen Kirjaudu sisään Azure Active Directory-työ](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Salasana-pohjainen etumerkin][4]


##<a name="linked-sign-on"></a>Valitse linkitetty merkki
Jos tueta sovelluksen, valitseminen linkitetyn SSO-tilassa mahdollistaa URL-osoite, josta voit ohjata uudelleen, kun käyttäjä napsauttaa sovelluksen Azure AD Access-paneelin tai Office 365: ssä. Saat lisätietoja linkitetyn SSO (aiemmalta nimeltään "aiemmin SSO") [miten yksittäinen Kirjaudu sisään Azure Active Directory-työ](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Linkitetyn Sign][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
