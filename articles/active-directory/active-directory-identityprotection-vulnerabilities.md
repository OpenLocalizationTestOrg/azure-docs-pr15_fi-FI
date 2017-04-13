<properties
    pageTitle="Azure Active Directory tunnistetietojen suojauksen havaitsemien heikkouksien | Microsoft Azure"
    description="Azure Active Directory tunnistetietojen suojauksen havaitsemien heikkouksien yleiskatsaus."
    services="active-directory"
    keywords="Azure active Directoryn tunnistetietojen suojaus-cloud app etsiminen-sovellukset, suojaus, riski, riskin taso, heikkous, suojauskäytäntö hallinta"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Heikkouksien havaitsemien Azure Active Directory tunnistetietojen suojaus 

Heikkouksien ovat heikkouksien, jotka voivat hyödyntää luvaton ympäristössä. Suosittelemme varalta voit parantaa suojausta reagoimisessa organisaation osoitteen ja estää hyökkääjiä hyödyntää niitä.
<br><br>
![heikkouksien](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

Seuraavissa osissa on yleiskuvaus heikkouksien ilmoittaa tunnistetiedot.

## <a name="multi-factor-authentication-registration-not-configured"></a>Monimenetelmäisen todentamisen rekisteröinti ei ole määritetty 

Tätä avulla voit hallita organisaation Azure Monimenetelmäisen todentamisen käyttöönottoa. 

Azure monimenetelmäisen todentamisen on toisen tason arvopaperin käyttäjien todentamiseen. Se auttaa suojata tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana. Se toimittaa vahva todentaminen vakioverkko solualueen helposti vahvistus asetukset – puhelun, tekstiviesti tai mobiilisovelluksessa ilmoitus- tai koodi ja 3rd osapuolen sijasta tunnukset.

On suositeltavaa Azure Monimenetelmäisen todentamisen edellyttäminen käyttäjän kirjautumiset. Monimenetelmäisen todentamisen toistetaan roolissa riskiin perustuva ehdollisen käyttöoikeuden käytännöt tunnistetiedot kautta.

Lisätietoja on artikkelissa [Mikä Azure Monimenetelmäisen todentamisen?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Ei-hallitun cloud-sovellukset

Tätä auttaa tunnistamaan ei-hallitun cloud sovellusten organisaatiossa.
 
Nykyaikainen yritysten IT-osastoille on usein tarkistamisen kaikki organisaation käyttäjät käyttävät työssään cloud-sovellukset. On helppo nähdä, miksi järjestelmänvalvojilla on epävarma luvattomasti yrityksen tiedot, tietojen vuoto ja muita tietoturvauhkia. 

On suositeltavaa, että organisaation käyttöön Cloud App etsiminen tuttuihin ei-hallitun cloud sovellukset ja Azure Active Directoryn avulla näiden sovellusten hallinta.

Lisätietoja on artikkelissa [etsiminen ei-hallitun cloud sovellusten Cloud App Discoveryn avulla](active-directory-cloudappdiscovery-whatis.md).



##<a name="security-alerts-from-privileged-identity-management"></a>Suojausvaroitusten poistaminen sellaisten jäsenyyksien hallinta

Tätä avulla voit tutkia ja ratkaista sellaisten käyttäjätietojen organisaation ilmoituksia.  

Voit antaa käyttäjien sellaisten työvaiheita organisaatiot tarvitse myöntää eston sellaisten käyttöoikeudet Azure AD-Azure- tai Office 365-resursseja tai SaaS muista sovelluksista. Kaikkien sellaisten käyttäjien kasvaa organisaation uhanalaisten. Tätä auttaa käyttäjiä tunnistaa tarpeettomat sellaisten käytön ja suorita tarvittavat vähentämiseksi tai ne aiheuttavat riskin poistamiseksi. 

Suosittelemme, että organisaatio käyttää Azure AD sellaisten jäsenyyksien hallinta hallinta-kohtaa ja näytön etuoikeutettu käyttäjätietoja ja niiden Azure AD-resurssien käyttöoikeuksien sekä muihin Microsoft-verkkopalveluihin kuten Office 365: ssä tai Microsoft Intune.

Lisätietoja on artikkelissa [Azure AD sellaisten tunnistetietojen hallinta](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Katso myös

 - [Azure Active Directory-tunnistetietojen suojaus](active-directory-identityprotection.md)
