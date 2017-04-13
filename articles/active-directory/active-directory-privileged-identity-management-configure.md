<properties
    pageTitle="Azure AD-luottamuksellisten jäsenyyksien hallinta | Microsoft Azure"
    description="Aihe, jossa kerrotaan, mikä Azure AD sellaisten jäsenyyksien hallinta on ja miten cloud tietoturvan parantaminen PIM avulla."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Azure AD-luottamuksellisten jäsenyyksien hallinta

Azure Active Directory (AD) sellaisten jäsenyyksien hallinta, jossa voit hallita, ohjausobjektin, ja käytön valvonta organisaatiossa. Tämä sisältää resurssien Azure AD- ja muihin Microsoft-verkkopalveluihin kuten Office 365: ssä tai Microsoft Intune käyttöoikeudet.  

> [AZURE.NOTE] Sellaisten jäsenyyksien hallinta on käytettävissä vain Premium P2 Azure Active Directory-versio. Lisätietoja on artikkelissa [Azure Active Directory-versiot](active-directory-editions.md).

Organisaatiot haluat pienentää henkilöille, joka käyttää suojattuun tietoihin tai resurssit, koska, joka vähentää käytön että haittaohjelmien käyttäjän. Käyttäjien on kuitenkin yhä Azure-, Office 365- tai SaaS sovelluksissa sellaisten toimintojen suorittamiseen. Organisaatiot antaa käyttäjille sellaisten Azure AD ilman seuranta, mitä käyttäjät tekevät niiden järjestelmänvalvojan oikeuksilla. Azure AD-luottamuksellisten jäsenyyksien hallinta auttaa ratkaisemaan riskin.  

Azure AD-luottamuksellisten jäsenyyksien hallinta auttaa:  

- Tarkistaa, ketkä käyttäjät ovat Azure AD-Järjestelmänvalvojat
- Ota käyttöön tarvittaessa "-aika" järjestelmänvalvojan oikeudet Microsoft Online Services-palveluihin, kuten Office 365: ssä ja Intune
- Hae raporttien järjestelmänvalvojan access historia ja muutoksista järjestelmänvalvojan tehtävät
- Tietoja sellaisten roolin access-ilmoitusten ottaminen käyttöön

Azure AD sellaisten jäsenyyksien hallinta voit hallita sisäisiä Azure AD organisaation rooleihin liittyvistä aiheista:  

- Yleinen järjestelmänvalvoja
- Laskutusjärjestelmänvalvoja
- Palvelun järjestelmänvalvoja  
- Käyttäjien järjestelmänvalvoja
- Salasanajärjestelmänvalvoja

## <a name="just-in-time-administrator-access"></a>Valitse vain aika järjestelmänvalvojan käyttöoikeudet

Perinteisesti voit määrittää käyttäjälle järjestelmänvalvojan roolin Azure perinteinen portal tai Windows PowerShellin kautta. Käyttäjästä tulee tuloksena **pysyvä järjestelmänvalvojan**rooli aina aktiivisena. Azure AD-luottamuksellisten jäsenyyksien hallinta esitellään **olevalla järjestelmänvalvojan**käsite. Olevalla järjestelmänvalvojien on oltava käyttäjät, jotka tarvitsevat sellaisten nyt ja valitse sitten, mutta ei päivittäin. Rooli ei ole käytössä, kunnes käyttäjä sen käyttöoikeudet, ne aktivointi loppuun ja ryhdy järjestelmänvalvojaksi aktiivinen ennalta ajanjakson aikana.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Ota käyttöön hakemistossa sellaisten jäsenyyksien hallinta

Voit aloittaa Azure AD sellaisten jäsenyyksien hallinta [Azure portal](https://portal.azure.com/).

>[AZURE.NOTE] Sinun on oltava Yleinen järjestelmänvalvoja organisaatiotilillä (esimerkiksi @yourdomain.com), ei ole Microsoft-tilillä (esimerkiksi @outlook.com), käyttöön Azure AD sellaisten kansion tunnistetietojen hallinta.

1. Kirjaudu [Azure portal](https://portal.azure.com/) -Yleinen järjestelmänvalvoja, hakemistossa.
2. Jos organisaatiolla on useita, valitse käyttäjänimesi Azure portaalin oikeassa yläkulmassa. Valitse kansio, jossa käytät Azure AD sellaisten tunnistetietojen hallinta.
3. Valitse **Lisää palveluja** ja Suodata-tekstiruudun avulla voit etsiä **Azure AD sellaisten tunnistetietojen hallinta**.
4. Tarkista **raporttinäkymät-ikkunan kiinnittäminen** ja valitse sitten **Luo**. Sellaisten jäsenyyksien hallinta-sovellus avautuu.

Jos ole Azure AD sellaisten käyttäjätietojen hallinta käyttäminen hakemistossa ensimmäinen henkilö, sitten [ohjatun suojaustoiminnon](active-directory-privileged-identity-management-security-wizard.md) esitellään ensimmäinen tehtävä-toiminto. Tämän jälkeen voit automaattisesti muuttuvat ensimmäisen **suojauksen järjestelmänvalvoja** - ja **sellaisten roolin järjestelmänvalvojan** hakemiston.

Vain sellaisten rooli-järjestelmänvalvoja voi hallita access muiden järjestelmänvalvojien. Voit [antaa muille käyttäjille voi hallita PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Sellaisten jäsenyyksien hallinta-koontinäyttö

Azure AD-luottamuksellisten Käyttäjätietojen hallinnan avulla raporttinäkymät-ikkuna, jonka avulla voit esimerkiksi tärkeitä tietoja:

- Ilmoitukset, joka osoittaa mahdollisuudet tietoturvan parantaminen
- Kunkin sellaisten roolissa käyttäjien lukumäärä  
- Olevalla ja pysyvä järjestelmänvalvojat määrä
- Arvioi myöhempää käyttöä varten

![PIM dashboard - näyttökuva][2]

## <a name="privileged-role-management"></a>Sellaisten Roolienhallinta

Azure AD sellaisten jäsenyyksien hallinta avulla voit hallita järjestelmänvalvojat lisäämällä tai poistamalla rooleille pysyvien tai olevalla järjestelmänvalvojat.

![PIM Lisää tai poista Järjestelmänvalvojat - näyttökuva][3]

## <a name="configure-the-role-activation-settings"></a>Rooli aktivointi asetusten määrittäminen

[Rooli asetusten](active-directory-privileged-identity-management-how-to-change-default-settings.md) avulla voit määrittää olevalla roolin aktivointi-ominaisuudet, mukaan lukien:

- Rooli aktivointi ajan
- Rooli aktivointi-ilmoitus
- Tietoja käyttäjän tarvitsee rooli-aktivointiprosessin aikana  

![PIM asetukset - järjestelmänvalvojan aktivointi - näyttökuva][4]

Huomaa, että kuvan, **Monimenetelmäisen todentamisen** painikkeet eivät ole käytettävissä. Tiettyjen, erittäin etuoikeutettu roolit, emme vaadi MFA kohonnutta suojaus.

## <a name="role-activation"></a>Rooli aktivointi  

[Aktivoi rooli](active-directory-privileged-identity-management-how-to-activate-role.md)olevalla järjestelmänvalvoja pyytää aika sidottujen "aktivointi" oikeuksia. Aktivointi voidaan pyytää **Aktivoi oma rooli** -vaihtoehdon käyttäminen Azure AD sellaisten tunnistetietojen hallinta.

Järjestelmänvalvoja, joka haluaa Aktivoi rooli on alusta Azure AD sellaisten Azure-portaalissa tunnistetietojen hallinta.

Rooli aktivoiminen ei voi mukauttaa. PIM-asetuksia voit määrittää aktivointi ja pituuden järjestelmänvalvoja tarvitsee aktivoida roolin tiedot.

![PIM järjestelmänvalvojan pyynnön roolin aktivointi - näyttökuva][5]

## <a name="review-role-activity"></a>Tarkista roolin tehtävä

Voit seurata, miten työntekijöiden ja järjestelmänvalvojien käyttävät sellaisten roolien kahdella eri tavalla. Ensimmäinen vaihtoehto on käytössä [valvonta historia](active-directory-privileged-identity-management-how-to-use-audit-log.md). Valvonta-historia kirjaa Jäljitä muutokset sellaisten roolimäärityksiä ja roolin aktivointi historia.

![PIM aktivointi historia - näyttökuva][6]

Toinen vaihtoehto on määrittäminen [access tarkistaa](active-directory-privileged-identity-management-how-to-start-security-review.md)säännöllisesti. Access näiden arvioiden maksettavan korvauksen- ja määritetty tarkistaja (kuten ryhmän esimiehen), tai työntekijät tarkastella itse. Tämä on paras tapa seurata kuka edelleen edellyttää käyttöoikeutta ja enää työntekijät.


## <a name="next-steps"></a>Seuraavat vaiheet
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
