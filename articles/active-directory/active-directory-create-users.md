<properties
    pageTitle="Lisää uusia käyttäjiä Azure Active Directory | Microsoft Azure"
    description="Kerrotaan, miten voit lisätä uusia käyttäjiä tai muuttaa Azure Active Directory-käyttäjätiedot."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Lisää uusia käyttäjiä tai Microsoft-tilin käyttäjille Azure Active Directory

Lisää käyttäjät voivat täyttää hakemistossa. Tässä artikkelissa kerrotaan, miten voit lisätä uusia käyttäjiä organisaation ja voit lisätä käyttäjät, joilla on Microsoft-tilit. Saat lisätietoja muista kansioista Azure Active Directoryn käyttäjien lisääminen ja käyttäjien lisääminen kumppaniyritykset [muita kansioita tai kumppaniyritykset Azure Active Directoryn Lisää käyttäjiä](active-directory-create-users-external.md). Lisätyt käyttäjät ei ole oletusarvoisesti järjestelmänvalvojan oikeudet, mutta voit määrittää roolit niihin milloin tahansa.

## <a name="add-a-user"></a>Käyttäjän lisääminen

1. Kirjautuminen [Azure perinteinen portaaliin](https://manage.windowsazure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.
2. Valitse **Active Directory**ja valitse sitten organisaation hakemiston nimi.
3. Valitse **käyttäjät** -välilehti ja valitse **Lisää käyttäjä**komentopalkista.
4. **Kerro kyseisen käyttäjän tietoja** -sivulla **käyttäjän tyyppi**Valitse jompikumpi seuraavista:

    - **Uuden käyttäjän organisaation** – Lisää uusi tili-kansiossa.
    - **Microsoft-tilillä käyttäjän** – Lisää käytössä olevan Microsoft kuluttaja-tilin kansion (esimerkiksi Outlook-tili)

5. **Käyttäjätyyppi**mukaan Kirjoita käyttäjänimi (uusi käyttäjä) tai sähköpostiosoitteen (käyttäjä, jolla on Microsoft-tili).
6. Käyttäjän **profiili** -sivulla on ensimmäinen ja viimeinen nimi, kutsumanimi ja käyttäjärooliin **roolit** -luettelosta. Lisätietoja käyttäjän ja järjestelmänvalvojan rooleihin artikkelissa [järjestelmänvalvojan roolien määrittäminen Azure AD](active-directory-assign-admin-roles.md). Määritä, onko käyttäjän **Käyttöön Monimenetelmäisen todentamisen** .
7. Valitse **Hae tilapäinen salasana** -sivulla **Luo**.

> [AZURE.IMPORTANT] Jos organisaatiosi käyttää useampaa kuin yhtä toimialuetta, voit perehtyä seuraavat ongelmat Kun käyttäjätilin lisääminen:
>
> - Lisää käyttäjätilejä, joiden sama käyttäjätunnus (UPN) toimialueilla **ensimmäisen** lisäämällä, esimerkiksi geoffgrisso@contoso.onmicrosoft.com, **ja sen jälkeen** geoffgrisso@contoso.com.
> - **Älä** Lisää geoffgrisso@contoso.com ennen kuin lisäät geoffgrisso@contoso.onmicrosoft.com. Tässä järjestyksessä on tärkeää ja voi olla hankalaa Kumoa.

## <a name="change-user-information"></a>Voit muuttaa käyttäjätietoja

Voit muuttaa minkä tahansa käyttäjän määrite lukuun ottamatta objektitunnus.

1. Avaa hakemistossa.
2. Valitse **käyttäjät** -välilehti ja valitse sitten näyttönimi käyttäjän, jota haluat muuttaa.
3. Tee haluamasi muutokset ja valitse sitten **Tallenna**.

Jos käyttäjä, jonka haluat vaihtaa on synkronoitu paikallisen Active Directory-palvelun kanssa, et voi muuttaa käyttäjätietoja kuvatulla tavalla. Jos haluat muuttaa käyttäjän, käytä paikallisen Active Directory-hallintatyökalut.

## <a name="guest-user-management-and-limitations"></a>Vieras-Käyttäjähallinta ja rajoitukset

Vieraiden tilit ovat muihin kansioihin, joita on kutsuttu hakemistossa SharePoint-tiedostojen, sovellusten tai Azure resursseihin käyttäjiltä. Hakemistossa Vieras-tili on sen pohjana UserType-määritteen arvoksi "Vierailija." Tavallinen käyttäjillä (tarkemmin sanottuna hakemistossa jäsenet) on UserType määrite "Jäsen."

Vieraiden on rajattu oikeuksien hakemistossa. Näiden oikeuksien rajoittaa vieraiden tietojen hakemistossa muiden käyttäjien mahdollisuuden. Kuitenkin vierailevat käyttäjät voivat käsitellä edelleen käyttäjät ja ryhmät käyttäjät ovat muokkaamassa resursseihin liittyviä. Vierailijat voivat:

- Muut käyttäjät ja ryhmät Azure-tilaukseen, johon ne on määritetty liittyvää näkyvissä
- Katso ryhmät, johon ne kuuluvat jäsenet
- Hakee kansioon, muut käyttäjät tietävät käyttäjän täydellinen sähköpostiosoite
- Katso vain rajattu määritteiden käyttäjien ne hakea--rajoitettu nimi, sähköpostiosoite, käyttäjätunnus (UPN) ja pikkukuva
- Tarkistetut toimialueet hakemistossa luettelo
- Suostumus sovellukset, myönnät jäsenillä on hakemistossa samat käyttöoikeudet

## <a name="set-guest-user-access-policies"></a>Vieras-käyttäjän access käytäntöjen määrittäminen

Kansion **määrittäminen** -välilehti sisältää asetukset vierailevien käyttäjien käyttöoikeuksien hallinta. Näitä asetuksia voi muuttaa vain Azure perinteinen Portalissa directory Yleinen järjestelmänvalvoja. Tällä hetkellä ei ole PowerShell tai API-menetelmää.

Avaa **Määritä** -välilehden Azure perinteinen-portaalissa, valitse **Active Directory**ja valitse sitten kansion nimi.

![Määritä Azure Active Directory-välilehti][1]

Voit muokata vaihtoehto, jos haluat hallita vierailevien käyttäjien käyttöoikeuksia.

![vierailevien käyttäjien käyttöoikeuksien hallinta asetukset][2]


## <a name="whats-next"></a>Mitä seuraavaksi?

- [Käyttäjien lisääminen muita kansioita tai kumppaniyritykset Azure Active Directoryn](active-directory-create-users-external.md)
- [Azure AD hallinta](active-directory-administer.md)
- [Azure AD salasanojen hallinta](active-directory-manage-passwords.md)
- [Azure AD-ryhmien hallinta](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
