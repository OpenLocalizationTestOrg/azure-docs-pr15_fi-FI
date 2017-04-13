<properties
    pageTitle="Azure Active Directory ehdollisen käyttöoikeuden usein kysytyt kysymykset | Microsoft Azure"
    description="Usein kysyttyjä kysymyksiä ehdollisen käyttöoikeuden "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory ehdollisen käyttöoikeuden usein kysytyt kysymykset

## <a name="which-applications-work-with-conditional-access-policies"></a>Mitkä sovellukset suoritetaan ehdollisen käyttöoikeuden käytäntöjä?

**A:** Katso ohjeaiheen [Ehdollinen Accessin tehtävät-sovelluksia tuetaan](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Ehdollinen käytäntöjen on pakotettu B2B yhteistyön ja vierailijat

**A:** Käytännöt ovat voimassa B2B yhteistyön käyttäjille. Kuitenkin joissakin tapauksissa käyttäjä ei välttämättä voi täyttävät käytännön vaatimus, jos esimerkiksi organisaation ei tue monimenetelmäisen todentamisen. 

Käytännön tällä hetkellä ei säilytetä SharePoint vierailevia käyttäjiä varten. Vieras yhteyden määritetään SharePointissa. Vierailevien käyttäjien tilit eivät koske access-käytäntöjä todennus-palvelimessa. Vierailijan oikeudet voi hallita SharePoint-palvelussa.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>SharePoint Onlinen käytännön myös koske OneDrive for Business?

**A:** Kyllä.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Miksi en voi asettaa käytännön-asiakas-sovelluksia, kuten Wordissa ja Outlookissa?

**A:** Ehdollinen käyttöoikeuskäytäntö asettaa palvelun vaatimukset ja on käytössä, kun todennus tapahtuu kyseiseen palveluun. Käytäntöä ei ole määritetty suoraan asiakassovellus; sen sijaan sitä käytetään, kun se soittaa lisääminen. Esimerkiksi SharePoint-käytäntöä koskee asiakkaille kutsuminen SharePoint ja käytännön määrittäminen Exchange Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Koske ehdollinen käyttöoikeuskäytäntö palvelutilejä?

**A:** Ehdollinen käytäntöjen koskevat kaikkia käyttäjätilejä. Tämä sisältää käytetään palvelutilejä käyttäjätilit. Monissa tapauksissa, joka suoritetaan valvomattoman palvelutilin ei voi täyttää käytännön. Tämä on esimerkiksi tapaukselle, kun MFA tarvitaan. Tällaisissa tapauksissa palvelut tilit jätetään pois käytännön, käyttämällä ehdollisen käyttöoikeuden hallinnan käytäntöasetukset. Lisätietoja käytännön soveltaminen tähän käyttäjät.
