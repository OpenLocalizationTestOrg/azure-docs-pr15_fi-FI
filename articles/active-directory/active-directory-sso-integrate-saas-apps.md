<properties
    pageTitle="Integroi Azure Active Directory kertakirjautumisen SaaS sovellusten |  Microsoft Azure"
    description="Ota Sign-todennusta ja valmistelu keskitetyt hallinta SaaS sovellusten Azure Active Directory-käyttäjän. Yleiskatsaus siitä, miten voit integroida Azure Active Directory SaaS sovelluksiin."
    services="active-directory"
      keywords="Integroi Azure AD SaaS sovellukset"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integroi Azure Active Directory kertakirjautumisen SaaS sovellukset  

> [AZURE.SELECTOR]
- [Azure portal](active-directory-enterprise-apps-manage-sso.md)
- [Azure perinteinen portal](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Aloita määrittäminen single Sign-sovelluksen, joka tuo organisaatioon käytät olemassa olevalle kansiolle Azure Active Directory (Azure AD). Voit käyttää Azure AD-kansio, jonka voit ladata Microsoft Azure, Office 365: ssä tai Windows Intune. Jos sinulla on kaksi tai useampi seuraavista, katso voit selvittää, mitkä niistä haluat käyttää [Azure Active Directoryn hallinta](active-directory-administer.md) .

## <a name="authentication"></a>Todennus

Sovellusten, jotka tukevat SAML 2.0-WS-Federation tai OpenID yhteyden protokollat Azure Active Directory käyttää kirjautuminen Luottamussuhteiden varmenteet. Katso lisätietoja tästä [liitetyt kertakirjautumisen varmenteet hallinta](active-directory-sso-certs.md).

Sovellusten, jotka tukevat vain HTML Lomakepohjainen Kirjautumisvirheitä aiheuttavien Azure Active Directory käyttää 'salasanan vaulting', Luottamussuhteiden. Näin organisaation käyttäjien automaattisesti allekirjoittaa SaaS sovellukseen Azure AD-käyttäjätilitiedot SaaS-sovelluksen avulla. Azure AD kerää ja tallentaa suojatusti käyttäjätilitiedot ja liittyvät salasana. Lisätietoja on artikkelissa [salasanan perustuva kertakirjautumisen](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Todennus

Valmistellun tilin avulla käyttäjä voi olla oikeus käyttää sovellusta, kun ne on todennettu kertakirjautumisen kautta. Käyttäjän valmistelu voidaan tehdä manuaalisesti tai joissakin tapauksissa, voit lisätä ja poistaa käyttäjätiedot Azure Active Directory-tehdyt muutokset perusteella SaaS-sovelluksesta. Katso lisätietoja aiemmin Azure AD-yhdistimien käyttämisestä automaattinen valmisteluun, [automaattisen käyttäjän valmistelu ja poista valmistelu SaaS sovellukset](active-directory-saas-app-provisioning.md).

Muussa tapauksessa voit manuaalisesti käyttäjätiedot lisääminen sovelluksen tai käyttää muita valmistelu ratkaisuja, jotka ovat käytettävissä olevat.

## <a name="access"></a>Access

Azure AD monin eri mukautettavissa sovellusten organisaation käyttäjille. Ei ole lukittu tietyn käyttöönotto-tai access-ratkaisuun. Voit käyttää [ratkaisu, joka sopii tarpeitasi](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Muita huomioon otettavia seikkoja sovellusten jo käytössä

Määrittäminen yksittäinen merkki käyttöön organisaatiossa käyttävä sovellus poistetaan-käyttäjätilejä, uuden sovelluksen luominen eri tavalla. Alustava vaiheet, mukaan lukien muutama: yhdistäminen käyttäjätietojen Azure AD-käyttäjätiedot-sovelluksessa ja tietoja siitä, miten käyttäjät kohdata sovellukseen kirjautuminen, kun se on integroitu.

> [AZURE.NOTE] Voit määrittää olemassa olevan sovelluksen SSO-haluat yleisen järjestelmänvalvojan oikeuksia sekä Azure AD- ja SaaS sovelluksen.

### <a name="mapping-user-accounts"></a>Yhdistäminen-käyttäjätilit

Käyttäjän tunnistetiedot on yleensä yksilöllinen tunnus, joka voi olla sähköpostiosoite tai käyttäjätunnus (UPN). Sinun on linkki (kartta) kunkin käyttäjän sovelluksen käyttäjätiedot ja niiden vastaaviin Azure AD-tunnistetiedot. Muutamalla eri tavalla sen mukaan, miten vastaanottajaksi sovelluksen käyttöoikeuksien taso.

Saat lisätietoja sovelluksen käyttäjätiedoista ja Azure AD-Käyttäjätietojen yhdistäminen on [mukauttaminen vaateita, jotka on annettu SAML-tunnuksen](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) ja [mukauttaminen määritettä yhdistämismäärityksiä valmisteluun](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Tietoja käyttäjän Kirjaudu sisään-toiminto

Kun integroit SSO sovelluksen, joka on jo käytössä, on tärkeää huomaat, että käyttäjäkokemus vaikuttaako. Kaikki sovellukset käyttäjien alkaa Azure AD-tunnistetietoja avulla voit kirjautua sisään. Se voi myös, että ne käytettävä eri portal sovelluksia.

Joidenkin sovellusten SSO voidaan toteuttaa sovelluksen kirjautumissivulla käyttöliittymän, mutta muiden sovellusten, käyttäjä saa käsitellään keskitetyn portaalin esimerkiksi ([Omat sovellukset](http://myapps.microsoft.com) tai [Office 365](http://portal.office.com/myapps)) Kirjaudu sisään. Lisätietoja erilaisia SSO ja niiden käyttäjien kokemukset- [sovellusten käyttö-ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

Toisen hyödyllinen resurssi on *Suppressing käyttäjä hyväksyy* [Guiding kehittäjät](active-directory-applications-guiding-developers-for-lob-applications.md) -artikkelissa.

## <a name="next-steps"></a>Seuraavat vaiheet


SaaS-sovellukset, jotka sovelluksen-valikoimasta löydät Azure AD tarjoaa useita [opetusohjelmat siitä, miten voit integroida SaaS sovellukset](active-directory-saas-tutorial-list.md).

Jos sovellus ei ole sovelluksen valikoimassa, voit [lisätä mukautetun sovelluksena Azure AD App-valikoimaan](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

On paljon tarkemmin kaikkien näiden ongelmien Azure.com kirjastossa alkavat [sovellusten käyttö-ja kertakirjautumisen Azure Active Directory-hakemistosta.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Katso myös

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
