<properties
   pageTitle="Ulkoisen käyttäjän objektin määritemuutokset Azure Active Directory-B2B yhteistyön esikatselu | Microsoft Azure"
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
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure AD-B2B yhteistyö esikatselu: ulkoisen käyttäjän objektin määritemuutokset

Kunkin käyttäjän Azure Active Directoryn edustaa user-objekti. Azure AD-käyttäjäobjektin elinkaarensa aikana läpikäymiä määritemuutokset eri vaiheita B2B yhteistyön kutsu-lunastaa työnkulku. Käyttäjä-objekti-edustava, hakemistossa kumppanin käyttäjällä on määritteitä, jotka muuttaa milloin lunastaa ajan, kun kumppanin käyttäjä napsauttaa linkkiä kutsu sähköpostitse. Tarkemmin:

- **SignInName** ja **AltSecId** määritteet on täydennetty
- **Näyttönimi** -määritteen muuttuu täydellinen käyttäjätunnus (user_fabrikam.com#EXT#@contoso.com) kirjautumisen nimi(user@fabrikam.com)

Azure AD-määritteitä seuranta auttaa vianmääritys riippumatta siitä, onko kumppanin käyttäjä on tuoteavaimella niiden B2B yhteistyö kutsu.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
Siirry Azure AD B2B yhteistyö sekä muita artikkeleita:

- [Mikä on Azure AD B2B yhteistyö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Toiminta](active-directory-b2b-how-it-works.md)
- [Laskun](active-directory-b2b-detailed-walkthrough.md)
- [CSV-tiedoston muodon Ohje](active-directory-b2b-references-csv-file-format.md)
- [Ulkoisen käyttäjän suojaustunnuksen muoto](active-directory-b2b-references-external-user-token-format.md)
- [Nykyinen esikatselu-rajoitukset](active-directory-b2b-current-preview-limitations.md)
- [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
