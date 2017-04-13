<properties
    pageTitle="Jakaminen tilille Azure AD |  Microsoft Azure"
    description="Tässä artikkelissa kuvataan, miten Azure Active Directoryn avulla organisaatiot voivat jakaa turvallisesti paikallisen sovellukset ja kuluttaja pilvipalveluihin tilit."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Tilien jakamisen Azure AD

## <a name="overview"></a>Yleiskatsaus
Joskus organisaatioiden on käytettävä yhden käyttäjänimen ja salasanan useita henkilöitä. Tämä tapahtuu yleensä kaksi tapauksissa:

- Sovellukset, jotka edellyttävät yksilöllisen sisäänkirjautuminen- ja salasana kullekin käyttäjälle käytettäessä onko paikallisen-sovelluksissa eikä kuluttaja cloud services (esimerkiksi yrityksen sosiaalisen median linkkejä).
- Kun luot usean käyttäjän ympäristössä. Joudut ehkä yhden paikallisen tilin, joka laajempia oikeudet ja avulla voidaan core asetusten hallinta ja palautus toimintoja (kuten paikallisen "Yleisen järjestelmänvalvojan" tili Office 365: n) tai Salesforce pääkansio-tili.

Perinteisesti näissä tileissä jaettava tunnistetiedot (käyttäjänimi ja salasana) jakaminen oikean henkilöille tai tallentamista jaettuun sijaintiin, johon useita luotettujen agenttien vuoksi käyttää niitä.

Perinteinen jakamisen malli on useita haitoista:

- Uusien sovellusten käyttöoikeuksien ottaminen käyttöön edellyttää, että jakaminen kaikille tunnistetiedot, jotka sen käyttöoikeudet.
- Kunkin jaetun sovelluksen voi vaatia omassa ainutlaatuisia jaetun tunnistetiedot-pakollista käyttäjien muistaa useita ehtojoukkoja ja tunnistetiedot. Kun käyttäjät on muista monta tunnistetiedot, riskien kasvaa, että ne käyttämään riskialtis käytännöt. (kuten kirjoittaminen salasanat alas).
- Ei voi tarkistaa, kenellä on oikeudet-sovellukseen.
- Ei voi tarkistaa, kuka voi *käyttää* sovelluksen.
- Kun haluat poistaa access-sovellukseen, sinun on Päivitä tunnistetiedot ja jakaa niitä uudelleen kaikille sitä tarvitseville kyseiseen sovellukseen.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory-tilin jakaminen

Azure AD mahdollistaa uuden tavan käyttämään jaettua tilit, jotka nämä haitoista jättää pois.

Azure AD-järjestelmänvalvoja määrittää, mitkä sovellukset, käyttäjä voi käyttää käyttämällä Access-ruudusta ja valitsemalla Sign parhaiten tyypin parhaiten sovelluksen. Näiden lajia *salasana-pohjainen single-etumerkin*, avulla Azure AD edustajana eräänlainen "broker" kyseisen sovelluksen Sign-prosessin aikana.

Käyttäjät kirjautuvat kerran niiden organisaation tilillä. Tämä on sama tili, jolla ne säännöllisesti käyttää työpöytää ja sähköpostin. Hän voi tutkia ja käyttää vain sellaisia sovelluksia, ne on varattu. Jaettu-tilien sovellusten luettelossa voi olla jokin muu luku jaetun tunnistetietoja. Käyttäjän ei tarvitse muistaa tai kirjoita se muistiin käyttäjät voivat käyttää eri tilit.

Jaetun asiakkaat eivät vain Suurenna valvonta ja parantaa, ne parantaa suojausta. Käyttäjät, joilla on oikeus käyttää tunnistetietoja ei näy jaetun salasana, mutta saat mieluummin oikeudet käyttää salasanan osana koordinoitu todennus-työnkulku. Lisäksi salasana joidenkin SSO-sovellusten kanssa sinulla voi olla Azure AD säännöllisesti palauttaminen (Päivitä) salasanan avulla suuria ja monimutkaisia salasanat-lisääntyvien tilin suojausta. Järjestelmänvalvoja helposti myöntää tai kumota access-sovellukseen ja tunnettiin aiemmin, kenellä on oikeudet-tilille, ja kuka käyttää sitä menneisyydessä.

Azure AD tukee tilejä, jaettu, kaikki yrityksen Mobility Suite EMS, Premium tai Basic käyttöoikeus käyttäjien salasanan yhteen kaikki välillä Kirjaudu sovellukset. Voit jakaa jokin valmiiksi integroitujen sovellusten sovelluksen valikoiman tuhansia tilit ja lisätä omia [mukautettuja SSO](active-directory-sso-integrate-saas-apps.md)-sovelluksilla salasanan todennustapa-sovelluksen.

Azure AD-ominaisuuksia, jotka mahdollistavat tilin jakamisen ovat seuraavat:

- [Salasanan kertakirjautuminen](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Salasanan yksittäisen Sign-agentti
- [Ryhmämääritys](active-directory-accessmanagement-self-service-group-management.md)
- Mukautettu salasanan sovellukset
- [Sovelluksen Raporttinäkymät-ikkunan/käyttöraportit](active-directory-passwords-get-insights.md)
- Loppukäyttäjän access portaaleihin
- [Sovelluksen välityspalvelimen](active-directory-application-proxy-get-started.md)
- [Active Directory-Marketplace](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Tilin jakaminen
Azure AD avulla voit jakaa tarvitset tilin seuraavasti:

- Lisää sovellus [app valikoima](https://azure.microsoft.com/marketplace/active-directory/) tai [mukautetun sovelluksen](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- Salasanan Single Sign-On (SSO)-sovelluksen kokoonpanon määrittäminen
- Käyttää [ryhmän perusteella varauksen](active-directory-accessmanagement-group-saasapps.md) ja antamaan jaettuun tunnistetietoa valitseminen
- Valinnainen: Jotkin sovellukset, kuten Facebookissa, Twitterissä tai LinkedIn-voit ottaa käyttöön [automaattisen Azure AD-salasanojen kaatumisen](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx) asetus

Voit suurentaa myös jaetun tilin suojausta multi-factor Authentication (MFA) (Katso lisätietoja [Azure AD suojaamisesta sovellusten](../multi-factor-authentication/multi-factor-authentication-get-started.md)) ja delegoida voi hallita, kenellä on oikeus käyttää sovelluksen [Azure AD Omatoiminen](active-directory-accessmanagement-self-service-group-management.md) ryhmän hallinta.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
- [Sovellusten ehdollinen käytön suojaaminen](active-directory-conditional-access.md)
- [Omatoiminen ryhmän hallinta/SSAA](active-directory-accessmanagement-self-service-group-management.md)
