<properties
   pageTitle="Nykyinen esikatselu rajoitukset Azure Active Directory-B2B yhteistyö | Microsoft Azure"
   description="Azure Active Directory-B2B tukee yrityksen yhteydet ottamalla liikekumppanien käyttämään yrityksen sovellustesi valikoivasti"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure AD-B2B yhteistyön esikatselu: nykyisen esikatsella rajoitukset

- Monimenetelmäisen todentamisen (MFA) ei tue ulkoisia käyttäjiä. Esimerkiksi jos Contoso on MFA, mutta kumppanin organisaatiokaavion ei ole, valitse kumppanin organisaatiokaavion käyttäjille ei voi myöntää MFA B2B yhteistyön kautta.
- Kutsut on mahdollista vain CSV; kautta yksittäisten kutsut ja API access ei tue.
- Vain Azure AD Yleiset järjestelmänvalvojat voivat ladata .csv-tiedostoissa.
- Kutsujen kuluttaja sähköpostiosoitteet (kuten Hotmail.comia, Gmail.com tai comcast.net) ovat tällä hetkellä tueta.
- Ulkoisten käyttäjien paikalliset ei testattu sovellukset.
- Ulkoiset käyttäjät ovat ei automaattisesti Siivotaan kun todellinen käyttäjä poistetaan niiden hakemistosta.
- Jakeluluettelot kutsujen ei tueta.
- Enintään 2 000 tietuetta voi ladata CSV kautta.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
Siirry Azure AD B2B yhteistyön sekä muita artikkeleita:

- [Mikä on Azure AD B2B yhteistyön?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Toiminta](active-directory-b2b-how-it-works.md)
- [Laskun](active-directory-b2b-detailed-walkthrough.md)
- [CSV-tiedoston muodon Ohje](active-directory-b2b-references-csv-file-format.md)
- [Ulkoisen käyttäjän suojaustunnuksen muoto](active-directory-b2b-references-external-user-token-format.md)
- [Ulkoisen käyttäjän objektin määritemuutokset](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
