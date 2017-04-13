<properties
    pageTitle="Poistaa käyttäjän tai ryhmän varauksen enterprise-sovelluksen Azure Active Directory-esikatselu | Microsoft Azure"
    description="Käyttäjän tai ryhmän access-tehtävän poistamisesta enterprise-sovelluksen Azure Active Directory"
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
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="remove-a-user-or-group-assignment-from-an-enterprise-app-in-azure-active-directory-preview"></a>Poistaa käyttäjän tai ryhmän varauksen enterprise-sovelluksen Azure Active Directory-esikatselu

On helppoa, voit poistaa käyttäjän tai ryhmän access on varattu johonkin yrityksen sovellusten Azure Active Directory (Azure AD) esikatselussa. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Sinulla on tarvittavat käyttöoikeudet hallinta enterprise-sovelluksella. Nykyisen esikatselussa sinun on oltava Yleinen järjestelmänvalvoja hakemiston.

## <a name="how-do-i-remove-a-user-or-group-assignment"></a>Kuinka voin poistaa käyttäjän tai ryhmän varauksen?

1. Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2. Valitse **Lisää palveluja**, kirjoita **Azure Active Directory** tekstiruutuun ja paina sitten **ENTER-näppäintä**.

3. Valitse *toiminta *Azure Active Directory - ** ** sivu (eli Azure AD sivu hallinnoit kansion), valitse **yrityksen sovellusten **.

    ![Avaaminen sovellusten](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Valitse **kaikki sovellukset** **yrityksen** -sivu. Näet voit hallita sovellusten luettelo.

5. Valitse sovelluksen **yrityksen sovellukset - kaikki sovellukset** -sivu.

6. Valitse **käyttäjät ja ryhmät** ***appname*** -sivu (eli valitun sovelluksen otsikko-niminen sivu).

    ![Käyttäjien tai ryhmien valitseminen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Valitse ***appname*** **-käyttäjä ja ryhmämäärityksen** sivu, valitse jokin Lisää käyttäjät tai ryhmät ja valitse sitten **Poista** -komento. Vahvista tulee näkyviin.

    ![Poista-komennon valitseminen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Näet kaikki Omat ryhmät](active-directory-groups-view-azure-portal.md)
- [Määrittää käyttäjän tai ryhmän enterprise-sovellukseen](active-directory-coreapps-assign-user-azure-portal.md)
- [Kirjaudu apuohjelmien käyttäjän enterprise-sovelluksen poistaminen käytöstä](active-directory-coreapps-disable-app-azure-portal.md)
- [Nimen tai sovelluksen yrityksen logon muuttaminen](active-directory-coreapps-change-app-logo-user-azure-portal.md)
