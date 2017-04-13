<properties
    pageTitle="Kirjaudu apuohjelmien käyttäjän enterprise-sovelluksen Azure Active Directory-esikatselu käytöstä | Microsoft Azure"
    description="Enterprise-sovelluksen poistamisesta niin, että ei ole käyttäjät voivat kirjautua sisään siihen Azure Active Directoryssa"
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
    ms.date="10/17/2016"
    ms.author="curtand"/>


# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory-preview"></a>Kirjaudu apuohjelmien käyttäjän Azure Active Directory-esikatselussa enterprise-sovelluksen poistaminen käytöstä

On helppoa enterprise-sovelluksen käytöstä niin, että ei ole käyttäjät voivat kirjautua sisään siihen Azure Active Directory (Azure AD) esikatselussa. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Sinulla on tarvittavat käyttöoikeudet hallinta enterprise-sovelluksella. Nykyisen esikatselussa sinun on oltava Yleinen järjestelmänvalvoja hakemiston.

## <a name="how-do-i-disable-user-sign-ins"></a>Miten voin vaimentaa käyttäjän kirjautumiset?

1. Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2. Valitse **Lisää palveluja**, kirjoita **Azure Active Directory** tekstiruutuun ja paina sitten **ENTER-näppäintä**.

3. Valitse *toiminta *Azure Active Directory - ** ** sivu (eli Azure AD sivu hallinnoit kansion), valitse **yrityksen sovellusten **.

    ![Avaaminen sovellusten](./media/active-directory-coreapps-disable-app-azure-portal/open-enterprise-apps.png)

4. Valitse **kaikki sovellukset** **yrityksen** -sivu. Näet luettelon voit hallita sovelluksia.

5. Valitse sovelluksen **yrityksen sovellukset - kaikki sovellukset** -sivu.

6. Valitse **Ominaisuudet** ***appname*** -sivu (eli valitun sovelluksen otsikko-niminen sivu).

    ![Kaikki sovellukset-komennon valitseminen](./media/active-directory-coreapps-disable-app-azure-portal/select-app.png)

7. Valitse ***appname*** **-Ominaisuudet** sivu, valitse **ei** , **käytössä käyttäjille kirjautumaan sisään?**.

8. Valitse **Tallenna** -komentoa.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Kaikkien ryhmien tarkasteleminen](active-directory-groups-view-azure-portal.md)
- [Määrittää käyttäjän tai ryhmän enterprise-sovellukseen](active-directory-coreapps-assign-user-azure-portal.md)
- [Poistaa käyttäjän tai ryhmän varauksen enterprise-sovelluksesta](active-directory-coreapps-remove-assignment-azure-portal.md)
- [Nimen tai sovelluksen yrityksen logon muuttaminen](active-directory-coreapps-change-app-logo-user-azure-portal.md)
