<properties
   pageTitle="Azure AD-B2B yhteistyön esikatselu: toiminta | Microsoft Azure"
   description="Tässä artikkelissa kuvataan, miten Azure Active Directory-B2B yhteistyö tukee yrityksen yhteydet ottamalla liikekumppanien käyttämään yrityksen sovellustesi valikoivasti"
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

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure AD-B2B yhteistyön esikatselu: toiminta
Azure AD-B2B yhteistyö perustuu kutsu ja lunastaa malli. Antaisit haluat käsitellä, ja haluat käyttää sovellusten osapuolten sähköpostiosoitteet. Azure AD lähettää sähköposti-kutsu, jossa on linkki. Kumppanin käyttäjän jäljessä olevaa linkkiä ja pyydetään kirjautumaan sisään niiden Azure AD-tilisi tai rekisteröi sinua käyttäjäksi uusi Azure AD-tili.

1. Järjestelmänvalvojaa kutsuu kumppanin käyttäjien lataamalla [rakenteellisia .csv-tiedoston](active-directory-b2b-references-csv-file-format.md) Azure-portaalissa.
2. Portaalin lähettää kutsu sähköpostit kumppanin käyttäjille.
3. Kumppanin käyttäjät napsauttamalla sähköpostiviestissä olevaa linkkiä ja pyytää, kirjaudu sisään käyttämällä työpaikan tunnistetietoja (jos ne ovat jo Azure AD) tai Azure AD B2B yhteistyön käyttäjäksi rekisteröityminen.
4. Kumppanin käyttäjät ohjataan ne sinut on kutsuttu, joihin heillä nyt on access-sovellukseen.

## <a name="directory-operations"></a>Hakemisto-toiminnot
Kumppanin käyttäjät ovat oman Azure AD kuin Ulkoiset käyttäjät. Tämä tarkoittaa järjestelmänvalvojaa voit valmistella käyttöoikeuksia, määrittää Ryhmäjäsenyyden ja edelleen yrityksen sovellusten Azure portaalin tai käyttämällä PowerShellin Azure aivan kuin yrityksesi käyttäjien käyttöoikeus.

Aikana maksettu Azure AD tilausta (Basic tai Premium) ei ole välttämätön Azure AD B2B, joilla on maksullisen alihallinnat Azure AD-tilauksen (Basic tai Premium) tulee Lisää seuraavat edut:

 - Järjestelmänvalvojat voivat määrittää ryhmät sovellukset yksinkertaisempi kutsuttujen käyttäjien käyttöoikeuksien hallinta tarjoamalla.
 - Järjestelmänvalvojan vuokraajan mukauttaminen käytetään tuotemerkin kutsun sähköpostit ja lunastushinta-toiminto tarjoaa useita kontekstia kutsuttujen kumppanin käyttäjien.

## <a name="related-articles"></a>Aiheeseen liittyviä artikkeleita
 Hae Microsoftin Azure AD B2B yhteiskäytön artikkeleita

 - [Mikä on Azure AD B2B yhteistyö?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Laskun](active-directory-b2b-detailed-walkthrough.md)
 - [CSV-tiedoston muodon Ohje](active-directory-b2b-references-csv-file-format.md)
 - [Ulkoisen käyttäjän suojaustunnuksen muoto](active-directory-b2b-references-external-user-token-format.md)
 - [Ulkoisen käyttäjän objektin määritemuutokset](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Nykyinen esikatselu-rajoitukset](active-directory-b2b-current-preview-limitations.md)
 - [Artikkelissa indeksin Azure Active Directory-sovellusten hallinta](active-directory-apps-index.md)
