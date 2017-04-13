<properties
    pageTitle="Lisätä useita käyttäjiä muista hakemistoista tai kumppanin yritysten Azure Active Directory-esikatselu | Microsoft Azure"
    description="Kerrotaan, miten voit lisätä käyttäjiä tai muuttaa käyttäjätietoja Azure Active Directoryn, mukaan lukien ulkoisista ja Vieras käyttäjät."
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
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>

# <a name="add-users-from-other-directories-or-partner-companies-in-azure-active-directory-preview"></a>Käyttäjien lisääminen muita kansioita tai kumppaniyritykset Azure Active Directory-esikatselussa

> [AZURE.SELECTOR]
- [Azure portal](active-directory-users-create-external-azure-portal.md)
- [Azure perinteinen portal](active-directory-create-users-external.md)

Tässä artikkelissa kerrotaan, miten voit lisätä käyttäjiä muista kansioista Azure Active Directory (Azure AD) esikatselussa tai kumppaniyritykset. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Katso tietoja uusien käyttäjien lisääminen organisaation ja käyttäjät, joilla on Microsoft-tilien lisääminen [Lisää uusia käyttäjiä Azure Active Directory](active-directory-users-create-azure-portal.md). Lisätyt käyttäjät ei ole oletusarvoisesti järjestelmänvalvojan oikeudet, mutta voit määrittää roolit niihin milloin tahansa.

## <a name="add-a-user"></a>Käyttäjän lisääminen

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita tekstiruutuun **käyttäjät ja ryhmät** ja paina sitten **ENTER-näppäintä**.

    ![Avaava käyttäjien hallinta](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Valitse **käyttäjät ja ryhmät** -sivu **käyttäjät**ja valitse sitten **Lisää**.

    ![Lisää-komennon valitseminen](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Antaa **nimi** näyttönimi ja käyttäjän kirjautumisnimi **käyttäjän**nimi **käyttäjän** -sivu.

5. Kopioi tai muussa muistiin luotu käyttäjän salasana, jotta voit antaa käyttäjälle, kun tämä prosessi on valmis.

6. Voit valita **profiili** , ja Lisää käyttäjät ensin ja Sukunimi, tehtävänimike ja osaston nimi.
    
    ![Käyttäjäprofiilin avaaminen](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

    - Valitse **ryhmät** käyttäjän lisääminen yksi tai useita ryhmiä.

        ![Käyttäjän lisääminen ryhmät](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

    - Valitse **rooli organisaatiossa** , jotta voit määrittää käyttäjälle roolin **roolit** -luettelosta. Lisätietoja käyttäjän ja järjestelmänvalvojan rooleihin artikkelissa [järjestelmänvalvojan roolien määrittäminen Azure AD](active-directory-assign-admin-roles.md).

        ![Käyttäjän liittäminen rooli](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Valitse **Luo**.

8. Jakaa turvallisesti uuden käyttäjän luotu salasana, niin, että käyttäjä voi kirjautua sisään.

> [AZURE.IMPORTANT] Jos organisaatiosi käyttää useampaa kuin yhtä toimialuetta, voit perehtyä seuraavat ongelmat Kun käyttäjätilin lisääminen:
>
> - Lisää käyttäjätilejä, joiden sama käyttäjätunnus (UPN) toimialueilla **ensimmäisen** lisäämällä, esimerkiksi geoffgrisso@contoso.onmicrosoft.com, **ja sen jälkeen** geoffgrisso@contoso.com.
> - **Älä** Lisää geoffgrisso@contoso.com ennen kuin lisäät geoffgrisso@contoso.onmicrosoft.com. Tässä järjestyksessä on tärkeää ja voi olla hankalaa Kumoa.

Jos muutat käyttäjän, joiden jäsenyyden on synkronoitu paikallisen Active Directory-palvelun kanssa tietoja, et voi muuttaa käyttäjätietoja Azure perinteinen-portaalissa. Jos haluat muuttaa käyttäjätietoja, käytä paikallisen Active Directory-hallintatyökalut.


## <a name="whats-next"></a>Mitä seuraavaksi?

- [Käyttäjän lisääminen](active-directory-users-create-azure-portal.md)
- [Käyttäjän salasanan uusi Azure-portaalissa](active-directory-users-reset-password-azure-portal.md)
- [Määrittää käyttäjän roolia Azure AD-](active-directory-users-assign-role-azure-portal.md)
- [Käyttäjän työ tietojen muuttaminen](active-directory-users-work-info-azure-portal.md)
- [Käyttäjäprofiilien hallinta](active-directory-users-profile-azure-portal.md)
- [Azure AD käyttäjän poistaminen](active-directory-users-delete-user-azure-portal.md)
