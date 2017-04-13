<properties
    pageTitle="Käyttäjälle määrittää järjestelmänvalvojan roolit Azure Active Directory-esikatselussa | Microsoft Azure"
    description="Kerrotaan, miten voit muuttaa järjestelmänvalvojan käyttäjätiedot Azure Active Directoryssa"
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

# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory-preview"></a>Käyttäjälle määrittää järjestelmänvalvojan roolit Azure Active Directory-esikatselussa

Tässä artikkelissa kerrotaan, miten voit määrittää järjestelmänvalvojan rooleja käyttäjän Azure Active Directory (Azure AD) esikatselussa. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Lisätietoja uusien käyttäjien lisääminen organisaation artikkelissa [Azure Active Directory Lisää uusia käyttäjiä](active-directory-users-create-azure-portal.md). Lisätyt käyttäjät ei ole oletusarvoisesti järjestelmänvalvojan oikeudet, mutta voit määrittää roolit niihin milloin tahansa.

## <a name="assign-a-role-to-a-user"></a>Määritä rooli käyttäjälle

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita tekstiruutuun **käyttäjät ja ryhmät** ja paina sitten **ENTER-näppäintä**.

    ![Avaava käyttäjien hallinta](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  Valitse **käyttäjät ja ryhmät** -sivu **kaikille käyttäjille**.

    ![Avaaminen kaikki käyttäjät-sivu](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)

4. Valitse **käyttäjät ja ryhmät - kaikki käyttäjät** -sivu-luettelosta käyttäjä.

5. Valitse valitun käyttäjän sivu **Directory rooli**ja määrittää käyttäjälle roolin **Directory rooli** -luettelosta. Lisätietoja käyttäjän ja järjestelmänvalvojan rooleihin artikkelissa [järjestelmänvalvojan roolien määrittäminen Azure AD](active-directory-assign-admin-roles.md).

      ![Käyttäjän liittäminen rooli](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Valitse **Tallenna**.


## <a name="whats-next"></a>Mitä seuraavaksi?

- [Käyttäjän lisääminen](active-directory-users-create-azure-portal.md)
- [Käyttäjän salasanan uusi Azure-portaalissa](active-directory-users-reset-password-azure-portal.md)
- [Käyttäjän työ tietojen muuttaminen](active-directory-users-work-info-azure-portal.md)
- [Käyttäjäprofiilien hallinta](active-directory-users-profile-azure-portal.md)
- [Azure AD käyttäjän poistaminen](active-directory-users-delete-user-azure-portal.md)
