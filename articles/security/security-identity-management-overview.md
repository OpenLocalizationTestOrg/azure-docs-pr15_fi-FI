<properties
   pageTitle="Azure käyttäjätietojen hallinta suojauksen yleiskatsaus | Microsoft Azure"
   description=" Microsoftin tunnistetietojen ja käytön hallinta ratkaisuja Ohje IT suojata access sovellukset ja resurssit yrityksen palvelinkeskuksen ja kyselyjä pilvipalveluun, ottaminen käyttöön, kuten monimenetelmäisen todentamisen ja ehdollisen käyttöoikeuden määrittäminen kelpoisuustarkistuksen tasot. Tässä artikkelissa on yleiskatsaus tärkeä Azure suojausominaisuuksia, jotka auttavat tunnistetietojen hallinta. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="azure-identity-management-security-overview"></a>Yleistä Azure käyttäjätietojen hallinta suojauksesta

Microsoftin tunnistetietojen ja käytön hallinta ratkaisuja Ohje IT suojata access sovellukset ja resurssit yrityksen palvelinkeskuksen ja kyselyjä pilvipalveluun, ottaminen käyttöön, kuten monimenetelmäisen todentamisen ja ehdollisen käyttöoikeuden määrittäminen kelpoisuustarkistuksen tasot. Lisäsuojausasetusten raportointi, valvonnan ja ilmoitat auttaa pienentämään tietoturvariskin seurantaa epäilyttävistä toimintaa. [Azure Active Directory-Premium](../active-directory/active-directory-editions.md) tarjoaa Kirjaudu tuhansia cloud-sovellukset (SaaS) sekä käyttää web Apps-sovellusten avulla voit suorittaa-ympäristöön.

Tietoturva ja Azure Active Directory (AD) etuja mahdollisuus:

- Voit luoda ja hallita yhden tunnistetietojen kullekin käyttäjälle hybrid yrityksissä pitää käyttäjiä tai ryhmiä laitteet synkronoituina
- Yksittäisen-on pääsy sovellustesi, mukaan lukien tuhansia valmiiksi integroidut SaaS sovellukset
- Pakotetut säännöt-pohjainen Monimenetelmäisen todentamisen sekä paikalliset sovelluksen access-suojauksen käyttöön ja cloud sovellukset
- Valmistele suojatun etäkäyttöpalvelimen paikallisen web-sovellusten Azure AD-sovelluksen välityspalvelimen kautta

Tässä artikkelissa on antamaan yleiskatsaus tärkeä Azure suojausominaisuuksia, jotka auttavat tunnistetietojen hallinta. Annamme myös linkkejä artikkeleihin, joissa avulla kullekin ominaisuudelle tiedot, jotta voit lukea lisää.  

Artikkelin ohjeessa on seuraavia core Azure käyttäjätietojen hallintaominaisuuksia:

- Kertakirjautuminen
- Käänteisen välityspalvelimen
- Monimenetelmäisen todentamisen
- Suojauksen valvonta, ilmoituksia ja tietokoneen learning perustuvia raportteja
- Kuluttaja tunnistetietojen ja käytön hallinta
- Laitteen rekisteröinti
- Sellaisten jäsenyyksien hallinta
- Tunnistetiedot
- Hybrid jäsenyyksien hallinta

## <a name="single-sign-on"></a>Kertakirjautuminen

Yksittäisen Sign (SSO) tarkoittaa, että voit käyttää kaikki sovellukset ja resurssit, jotka sinun on suoritettava business, kirjautumalla sisään yksittäisen käyttäjätilin vain kerran. Kun olet kirjautunut sisään, voit käyttää kaikkia sovelluksia, sinun on ilman todentaminen edellyttää (esimerkiksi kirjoittaa salasanan) toisen kerran.

Monet organisaatiot ovat riippuvaisia ohjelmiston kuin kuten Office 365: ssä, ruutu ja Salesforce service (SaaS) sovellukset loppukäyttäjän varten. Perinteisesti IT-henkilöstön tarvitaan luominen ja päivittäminen SaaS kunkin sovelluksen käyttäjätilit yksitellen ja edellytti muista SaaS kunkin sovelluksen salasana.

Azure AD ulottuu paikallisen Active Directory pilvipalveluun, käyttäjät voivat käyttää hänen ensisijaisen organisaatiotili paitsi toimialueeseen liittyneet laitteensa kirjautuminen ja yrityksen resurssien ottaminen käyttöön, mutta myös kaikki web ja SaaS sovellusten tarvittavat työssään.

Paitsi käyttäjät ei tarvitse hallita useita ehtojoukkoja, käyttäjänimet ja salasanat-sovellusten käyttö voi olla automaattisesti valmistellun tai poistaa valmistellun mukaan organisaation ryhmät ja niiden tiloista esimerkiksi työntekijä. Azure AD esitellään suojaus ja access-hallinnon ohjausobjektit, joiden avulla voit hallita keskitetysti SaaS sovellusten käyttäjien käyttöoikeuksia.

Opi lisää:

- [Kertakirjautumisen yleiskatsaus](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/)
- [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](../active-directory/active-directory-appssoaccess-whatis.md)
- [Integroi Azure Active Directory kertakirjautumisen SaaS sovellukset](../active-directory/active-directory-sso-integrate-saas-apps.md)

## <a name="reverse-proxy"></a>Käänteisen välityspalvelimen

Välityspalvelimen Azure AD-sovelluksen avulla voit julkaista paikallisen sovellusten, kuten [SharePoint](https://support.office.com/article/What-is-SharePoint-97b915e6-651b-43b2-827d-fb25777f446f?ui=en-US&rs=en-US&ad=US) -sivustojen, [Outlook Web App](https://technet.microsoft.com/library/jj657718.aspx)ja [IIS](http://www.iis.net/)-pohjaisten sovellusten yksityinen verkoston sisällä ja tarjoaa suojatun verkon ulkopuolisille käyttäjille. Sovelluksen välityspalvelin on useita erityyppisiä paikallisen web-sovellusten kanssa, joka tukee Azure AD SaaS sovellusten tuhansia etäkäyttöpalvelimen ja kertakirjautuminen (SSO). Työntekijöiden voit kirjautua sovellukset home omia laitteissa ja todentaa tämän pilvipohjainen välityspalvelimen kautta.

Opi lisää:

- [Azure AD-sovelluksen välityspalvelimen ottaminen käyttöön](../active-directory/active-directory-application-proxy-enable.md)
- [Julkaise sovellusten välityspalvelimen Azure AD-sovelluksen avulla](../active-directory/active-directory-application-proxy-publish.md)
- [Single-Sign sovelluksen välityspalvelimen kanssa](../active-directory/active-directory-application-proxy-sso-using-kcd.md)
- [Ehdollisen käyttöoikeuden käsitteleminen](../active-directory/active-directory-application-proxy-conditional-access.md)

## <a name="multi-factor-authentication"></a>Monimenetelmäisen todentamisen
Azure monimenetelmäisen todentamisen (MFA) on todennusmenetelmän, joka edellyttää vähintään kaksi tarkistustapa käyttöä ja Lisää Kriittinen toisen kerroksen arvopaperin käyttäjän kirjautumiset ja tapahtumia. MFA auttaa suojaavat käyttämään tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana. Se toimittaa vahva todentaminen vakioverkko solualueen vahvistus asetukset – puhelun, tekstiviesti tai mobiilisovelluksessa ilmoitus- tai koodi ja 3rd osapuolen OAuth tunnukset.

Opi lisää:

- [Monimenetelmäisen todentamisen](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Mikä on Azure Monimenetelmäisen todentamisen?](../multi-factor-authentication/multi-factor-authentication.md)
- [Azure Monimenetelmäisen todentamisen toiminta](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Suojauksen valvonta, ilmoituksia ja tietokoneen learning perustuvia raportteja

Suojauksen seuranta- ja ilmoitukset- ja tietokoneen learning perustuvia raportteja, jotka Määritä epäyhtenäinen access voi auttaa suojaamaan. Voit käyttää Azure Active Directoryn käytön ja käyttöraportit ja myönnä näkyvyys eheys ja organisaation hakemistoon turvallisuus. Nämä ohjeet directory järjestelmänvalvojan paremmin selviää missä mahdollisia tietoturvauhkia saattaa ovat niin, että ne suunnittelevat riittävästi pienentämään näitä riskejä.

Azure perinteinen portaalissa raporttien luokitellaan seuraavilla tavoilla:

- Poikkeavuuksista raportit – sisältää tapahtumia, jotka on erheellisiin löydetyillä sisäänkirjautuminen. Tavoitteenamme on sinut tietoinen tällaisen toiminnan ja, joiden avulla voit tehdä siitä, onko tapahtuman epäilyttävistä määrittäminen.
- Integroitu sovellus – raporteissa kyselyjä miten cloud sovellusten käytetään organisaation tiedot. Azure Active Directory on tuhansia cloud sovellusten integrointi.
- Virheraportit – ilmaise virheet, joita voi ilmetä valmistelu ulkoisiin sovelluksiin tilit.
- Käyttäjän määrittämä raportit – näyttää laitteen ja kirjaudu tehtävätietojen tietyn käyttäjän.
- Lokeja – sisältää kaikki valvottuja tapahtumia viimeisen 24 tuntia, viimeisen seitsemän päivän tai viimeisten 30 päivän aikana, sekä ryhmän toiminnan muutokset ja salasanan palauttaminen ja rekisteröintiä tehtävän.

Opi lisää:

- [Kopioimasta raportin tarkasteleminen](../active-directory/active-directory-view-access-usage-reports.md)
- [Azure Active Directory-raportointi käytön aloittaminen](../active-directory/active-directory-reporting-getting-started.md)
- [Azure Active Directory raportointi-opas](../active-directory/active-directory-reporting-guide.md)

## <a name="consumer-identity-and-access-management"></a>Kuluttaja tunnistetietojen ja käytön hallinta

Azure Active Directory-B2C on käytettävissä, Yleinen, käyttäjätietojen hallinta-palvelu kuluttaja osoittava sovellusten, satoja käyttäjätietojen miljoonia Skaalaa. Voit integroida mobile yli ja web-käyttöjärjestelmien työpöytäsovelluksiin. Lukijoille voit kirjautua kaikkien sovellustesi mukautettavissa kokemukset kautta käyttämällä aiemmin niiden yhteisöpalvelujen tai luomalla uuden tunnistetiedot.

Aiemmin sovelluskehittäjät, jotka haluat rekisteröityä ja kirjaudu sisään kuluttajille niiden sovelluksiin kirjoittanut oman koodin. Ja ne olisi käyttänyt paikallisen tietokannan tai järjestelmien käyttäjänimet ja salasanat. Azure Active Directory-B2C on organisaation paremman tavan kuluttaja jäsenyyksien hallinta integroida suojattua ja standardit ympäristö ja suuri joukko extensible käytäntöjä sovellusten avulla.

Kun käytät Azure Active Directory-B2C, lukijoille rekisteröidä sovellusten käyttämällä niiden aiemmin sosiaalisten tilit (Facebook, Google, Amazon, Linkedinin) tai luomalla uuden tunnistetiedot (sähköpostiosoite ja salasana, tai käyttäjänimi ja salasana).

Opi lisää:

- [Azure Active Directory-B2C ominaisuudet](https://azure.microsoft.com/services/active-directory-b2c/)
- [Azure Active Directory-B2C esikatselu: Kirjaudu ylös ja kirjaudu sisään sovelluksesi käyttäjät](../active-directory-b2c/active-directory-b2c-overview.md)
- [Azure Active Directory B2C esikatselu: Tyyppisiä sovelluksia](../active-directory-b2c/active-directory-b2c-apps.md)

## <a name="device-registration"></a>Laitteen rekisteröinti

Azure AD-laitteen rekisteröinti on laitteen perustuvan [ehdollisen käyttöoikeuden](../active-directory/active-directory-conditional-access-on-premises-setup.md) skenaarioita. Kun laite on rekisteröity, Azure Active Directory laitteen rekisteröinti on laitetta, jonka jäsenyyden koon muuttamiseen tarkoitettu todennetaan laite, kun käyttäjä kirjautuu. Todennettu laitteen ja laitteen määritteiden sitten voidaan voidaan valvoa ehdollisen käyttöoikeuden käytäntöjä, joita isännöidään cloud ja paikallisen sovellusten.

Mobiililaitteen management (MDM)-ratkaisun, kuten Intuneen yhdistämällä laitteen määritteet Azure Active Directoryn päivittyvät lisätietoja laitteen. Voit luoda ehdollisen käyttösäännöt, jotka Pakota laitteilla mukaisia, tietosuojaan ja vaatimustenmukaisuuteen access.

Opi lisää:

- [Azure Active Directory laitteen rekisteröinti käytön aloittaminen](../active-directory/active-directory-conditional-access-device-registration-overview.md)
- [Paikallisen ehdollinen käytön Azure Active Directory laitteen rekisteröinti määrittäminen](../active-directory/active-directory-conditional-access-on-premises-setup.md)
- [Automaattinen laitteen rekisteröinti Azure Active Directory Windows toimialueeseen liittyneet laitteiden kanssa](../active-directory/active-directory-conditional-access-automatic-device-registration.md)

## <a name="privileged-identity-management"></a>Sellaisten jäsenyyksien hallinta
Azure Active Directory (AD) sellaisten jäsenyyksien hallinta voit hallita, hallita, ja valvoa sellaisten käyttäjätietoja ja Azure AD resursseja sekä Microsoft muihin verkkopalveluihin kuten Office 365: ssä tai Microsoft Intune-käytön.

Joskus käyttäjät käsittelevät sellaisten työvaiheita Azure- tai Office 365-resursseja tai SaaS muista sovelluksista. Tämä tarkoittaa yleensä organisaatioilla on antaa heille pysyvä sellaisten access Azure AD. Tämä on kasvava tietoturvariskin cloud isännöimä resurssien, koska organisaatiot riittävän voi seurata, mitä käyttäjät tekevät niiden järjestelmänvalvojan oikeuksilla. Lisäksi jos käyttäjätili sellaisten käytön on ongelmia, yksi sopimusrikkomusta, voi olla vaikutusta niiden cloud tietoturvan. Azure AD-luottamuksellisten jäsenyyksien hallinta auttaa ratkaisemaan riskin.

Azure AD-luottamuksellisten jäsenyyksien hallinta avulla voit:

- Tarkistaa, ketkä käyttäjät ovat Azure AD-Järjestelmänvalvojat
- Ota käyttöön tarvittaessa "-aika" järjestelmänvalvojan oikeudet Microsoft Online Services-palveluihin, kuten Office 365: ssä ja Intune
- Hae raporttien järjestelmänvalvojan access historia ja muutoksista järjestelmänvalvojan tehtävät
- Tietoja sellaisten roolin access-ilmoitusten ottaminen käyttöön

Opi lisää:

- [Azure AD-luottamuksellisten jäsenyyksien hallinta](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Roolien Azure AD-luottamuksellisten jäsenyyksien hallinta](../active-directory/active-directory-privileged-identity-management-roles.md)
- [Azure AD sellaisten jäsenyyksien hallinta: Voit lisätä tai poistaa käyttäjärooli](../active-directory/active-directory-privileged-identity-management-how-to-add-role-to-user.md)

## <a name="identity-protection"></a>Tunnistetiedot
Azure AD-tunnistetietojen suojaus on suojaus-palvelu, joka sisältää koottuja näkymän riskin tapahtumien ja mahdolliset heikkouksien, vaikuttavia organisaation käyttäjätietoja. Tunnistetiedot hyödyntää aiemmin Azure Active Directoryn poikkeavuuksista tunnistus ominaisuuksia (käytettävissä Azure AD erheellisiin käyttöraporttien) ja esitellään uusi riski tapahtuman Lisättävissä olevat tunnistaa poikkeamia reaaliajassa.

Opi lisää:

- [Azure Active Directory-tunnistetietojen suojaus](../active-directory/active-directory-identityprotection.md)
- [Kanavan 9: Azure AD ja käyttäjätiedot näkyvät: Identity suojaus esikatselu](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="hybrid-identity-management"></a>Hybrid jäsenyyksien hallinta

Käyttäjätietojen hallintatavan Microsoftin ulottuu paikallisen ja pilveen luominen yksittäisen käyttäjän tunnistustietojen todennus- ja kaikkien resurssien paikasta riippumatta.

Opi lisää:

- [Hybrid tunnistetietojen tekninen raportti](http://download.microsoft.com/download/D/B/A/DBA9E313-B833-48EE-998A-240AA799A8AB/Hybrid_Identity_White_Paper.pdf)
- [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)
- [Active Directory-Ryhmäblogi](https://blogs.technet.microsoft.com/ad/)
