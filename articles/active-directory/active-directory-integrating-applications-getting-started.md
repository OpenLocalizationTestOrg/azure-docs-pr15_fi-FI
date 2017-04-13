<properties
   pageTitle="Azure Active Directory-integraation sovellusten käytön aloitusopas |  Microsoft Azure"
   description="Tämä artikkeli koskee hakeminen aloittaminen opas Azure Active Directory (AD) integraation paikallisen sovellukset ja cloud sovellukset."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Azure Active Directory-integraation sovellusten käytön aloitusopas
## <a name="overview"></a>Yleiskatsaus
Tässä aiheessa on tarkoitus antaa suunnitelma integroimiseen sovellukset ja Azure Active Directory (AD). Alla osioissa sisältää yksityiskohtaisempia aiheen lyhyt yhteenveto, joten voit tunnistaa hakeminen aloittaminen oppaan-osat ovat tärkeimpiä.  Noudata tarkempaa perinpohjaisesti käsittelevään artikkeliin linkit kunkin aiheen.

## <a name="before-you-begin-take-inventory"></a>Ennen kuin aloitat, tutustu varaston
Ennen kuin voit siirtyä sovellukset-integraation Azure AD, on tärkeää tietää, missä ja johon haluat siirtyä.  Voit ottaa huomioon Azure AD sovelluksen integrointi projektin on tarkoitettu seuraaviin kysymyksiin.

### <a name="application-inventory"></a>Sovelluksen varaston
- Missä ovat kaikki sovelluksesi? Kuka omistaa ne?
- Mitä todennustapaa sovellustesi vaativat?
- Mitkä sovellukset vastaanottajat
- Haluatko ottaa käyttöön uuden sovelluksen?
  - On luoda sisäiset ja ota käyttöön Azure Laske-esiintymässä?
  - Haluatko käyttää Azure-sovelluksen valikoimassa käytettävissä on?

### <a name="user-and-group-inventory"></a>Käyttäjien ja ryhmien varaston
- Jossa käyttäjätilit ovat?
 - Paikallisen Active Directory
 - Azure AD
 - Erillinen sovellustietokanta, jotka omistat sisällä
 - Unsanctioned-sovelluksissa
 - Kaikki edellä mainitut
- Mitä käyttöoikeudet ja roolimäärityksiä yksittäisille käyttäjille tällä hetkellä tarvitse? Haluat tarkastella niiden access tai Oletko varma, että käyttäjän käyttöoikeudet ja roolin varaukset ovat sopivalla tasolla nyt?
- Ovat ryhmät jo muodostettu paikallisen Active Directoryssa?
 - Miten ryhmien on järjestetty?
 - Ryhmän jäsenet?
 - Mitä oikeuksia/roolimäärityksiä ryhmät tällä hetkellä tarvitse?
- Tuleeko Puhdista käyttäjän/ryhmän tietokantojen ennen integrointi?  (Tämä on melko tärkeää kysymystä. Roskaa roskaa-.)

### <a name="access-management-inventory"></a>Accessin hallinta varaston
- Miten voit nyt hallita sovellusten käyttöoikeudet? , Joka on muutettava?  On katsoa muita tapoja hallita niiden käyttöä, kuten kanssa [RBAC](role-based-access-control-configure.md) esimerkiksi?
- Oikeudet mihinkin vastaanottajat

Ehkäpä ei ole kaikkiin näihin kysymyksiin etukäteen, mutta se ei haittaa.  Tämän oppaan avulla voit vastaa joistakin näihin kysymyksiin ja päätösten joitakin ajan tasalla.

## <a name="prerequisites"></a>Edellytykset
- Azure-tilausta ja Azure Active Directory-hakemistosta.  Jos sinulla ei vielä ole Azure tilaus, voit kokeilla Azure ilmaiseksi 30 päivän varten. [Kokeile!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Sovelluksen Azure AD-integrointi
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Cloud sovellusten etsiminen unsanctioned Cloud App etsiminen
Kuten edellä mainittiin, voi olla sovelluksia, jotka on vielä ole hallitsee organisaatiosi toistaiseksi.  Osana varaston prosessin ei löydä unsanctioned cloud-sovelluksia. Katso [etsiminen Cloud App etsiminen unsanctioned cloud-sovelluksiin](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Todennustyypit
Kaikkien sovellustesi voi olla eri todennusta. Azure AD-ja sovelluksia, jotka käyttävät SAML 2.0, WS-Federation tai OpenID yhteyden protokollat sekä salasanan kertakirjautuminen voidaan käyttää kirjautuminen varmenteet. Saat lisätietoja sovelluksen Azure AD käyttäminen todennustyypit artikkelissa [, Liitetty kertakirjautumisen Azure Active Directoryn hallinta varmenteet](active-directory-sso-certs.md) ja [salasanan mukaan kertakirjauksen](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Välityspalvelimen Azure AD-sovelluksen kanssa SSO ottaminen käyttöön
Microsoft Azure AD-sovelluksen välityspalvelimen voit kirjoittaa sovellukset tuotepakkauksen yksityinen verkoston sisällä turvallisesti, missä tahansa ja millä tahansa laitteella. Kun olet asentanut sovelluksen-välityspalvelimen yhdistimen käyttöympäristössä, se voi helposti määritetään Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Azure AD-sovellusten integraation
Seuraavissa artikkeleissa käsitellään eri tavalla sovellusten Azure AD integrointi ja esitellään joitakin.

- [Tarkistamalla, mitä Active Directoryn käyttäminen](active-directory-administer.md)
- [Azure sovelluksen valikoiman sovelluksilla](active-directory-appssoaccess-whatis.md)
- [Paikallisen ympäristön integroinnissa SaaS sovellusten opetusohjelmat luetteloon](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Sovellusten käyttöoikeuksien hallinta
Seuraavissa artikkeleissa kerrotaan access-sovellukset voivat hallita, kun ne on integroitu Azure AD-Azure AD-yhdistimien käyttämisestä ja Azure AD tavalla.

- [Käyttämällä Azure AD sovellusten käyttöoikeuksien hallinta](active-directory-managing-access-to-apps.md)
- [Azure AD-yhdistimillä automatisointi](active-directory-saas-app-provisioning.md)
- [Käyttäjien määrittäminen-sovellukseen](active-directory-applications-guiding-developers-assigning-users.md)
- [Käyttäjäryhmien määrittäminen-sovellukseen](active-directory-applications-guiding-developers-assigning-groups.md)
- [Jakaminen tilit](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Paikallisen ympäristön integroinnissa mukautetut sovellukset.
Jos uusi sovellus kirjoittamisen ja haluat auttaa kehittäjät hyödyntäminen power Azure AD, katso [Guiding kehittäjät](active-directory-applications-guiding-developers-for-lob-applications.md).

Jos haluat lisätä mukautetun sovelluksen Azure-sovelluksen valikoimaan, katso ["Tuoda Omat app" Azure AD omatoimisen SAML määrityksissä](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Katso myös

- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
