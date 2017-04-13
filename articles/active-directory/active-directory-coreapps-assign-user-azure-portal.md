<properties
    pageTitle="Määrittää käyttäjän tai ryhmän enterprise-sovelluksen Azure Active Directory-esikatselu | Microsoft Azure"
    description="Määrittää käyttäjän tai ryhmän siihen Azure Active Directoryn enterprise-sovelluksen valitseminen"
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
    ms.date="10/03/2016"
    ms.author="curtand"/>

# <a name="assign-a-user-or-group-to-an-enterprise-app-in-azure-active-directory-preview"></a>Määrittää käyttäjän tai ryhmän enterprise-sovelluksen Azure Active Directory-esikatselu

On helppo määrittää käyttäjän tai ryhmän yrityksen sovellusten Azure Active Directory (Azure AD) esikatselussa. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Sinulla on tarvittavat käyttöoikeudet hallinta enterprise-sovelluksella. Nykyisen esikatselussa sinun on oltava Yleinen järjestelmänvalvoja hakemiston.

## <a name="how-do-i-assign-user-access-to-an-enterprise-app"></a>Miten käyttäjien käyttöoikeuksien määritellä enterprise-sovelluksen?

1. Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2. Valitse **Lisää palveluja**, kirjoita Azure Active Directory tekstiruutuun ja paina sitten **ENTER-näppäintä**.

3. Valitse *toiminta *Azure Active Directory - ** ** sivu (eli Azure AD sivu hallinnoit kansion), valitse **yrityksen sovellusten **.

    ![Avaaminen sovellusten](./media/active-directory-coreapps-assign-user-azure-portal/open-enterprise-apps.png)

4. Valitse **kaikki sovellukset** **yrityksen** -sivu. Näet voit hallita sovellusten luettelo.

5. Valitse sovelluksen **yrityksen sovellukset - kaikki sovellukset** -sivu.

6. Valitse **käyttäjät ja ryhmät** ***appname*** -sivu (eli valitun sovelluksen otsikko-niminen sivu).

    ![Kaikki sovellukset-komennon valitseminen](./media/active-directory-coreapps-assign-user-azure-portal/select-app-users.png)

7. Valitse ***appname*** **-käyttäjä ja ryhmämäärityksen** sivu, valitse **Lisää** -komento.

8. Valitse **Lisää tehtävä** -sivu **käyttäjät ja ryhmät**.

    ![Määrittää käyttäjän tai ryhmän-sovellukseen](./media/active-directory-coreapps-assign-user-azure-portal/assign-users.png)

9. Valitse **käyttäjät ja ryhmät** -sivu Valitse yksi tai useampi käyttäjien tai ryhmien luettelosta ja valitse sitten sivu alareunassa **Valitse** -painike.

10. Valitse **Lisää tehtävä** -sivu **rooli**. Valitse rooli, jotta voit käyttää valitut käyttäjät tai ryhmät, **Valitse rooli** , sivu- ja valitse sitten sivu alareunassa **OK** -painiketta.

11. Valitse **Lisää tehtävä** -sivu sivu alareunassa **Liitä** -painiketta. Määritetyt käyttäjät ja ryhmät on yrityksen sovelluksen valitun roolin määrittämiä käyttöoikeuksia.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Näet kaikki Omat ryhmät](active-directory-groups-view-azure-portal.md)
- [Poistaa käyttäjän tai ryhmän varauksen enterprise-sovelluksesta](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Kirjaudu apuohjelmien käyttäjän enterprise-sovelluksen poistaminen käytöstä](active-directory-coreapps-disable-app-azure-portal.md)
- [Nimen tai sovelluksen yrityksen logon muuttaminen](active-directory-coreapps-change-app-logo-user-azure-portal.md)
