<properties
    pageTitle="Azure Active Directory-esikatselu ryhmän jäsenten hallinta | Microsoft Azure"
    description="Miten käyttäjille ja laitteille, jotka ovat Azure Active Directory-ryhmän jäsenet"
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


# <a name="manage-the-members-for-a-group-in-azure-active-directory-preview"></a>Azure Active Directory-esikatselu ryhmän jäsenten hallinta

Tässä artikkelissa kerrotaan, miten voit hallita Azure Active Directory (Azure AD) esikatselu ryhmän jäsenet. [Mikä on esikatselussa?](active-directory-preview-explainer.md)

## <a name="how-do-i-find-the-members-and-manage-them"></a>Miten Etsi jäsenet ja hallita niitä?

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita tekstiruutuun **käyttäjät ja ryhmät** ja paina sitten **ENTER-näppäintä**.

  ![Avaava käyttäjien hallinta](./media/active-directory-groups-members-azure-portal/search-user-management.png)

3.  Valitse **käyttäjät ja ryhmät** -sivu **Kaikki ryhmät**.

  ![Avaaminen ryhmät-sivu](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)

4. Valitse **käyttäjät ja ryhmät - kaikki ryhmät** -sivu.

5. Valitse * *ryhmä - *Ryhmän_nimi* ** sivu, valitse **jäsenet **.

  ![Avaaminen jäsenet-sivu](./media/active-directory-groups-members-azure-portal/view-group-members.png)

6. Voit lisätä jäseniä ryhmään, valitse **ryhmä - jäsenet** -sivu valitsemalla **Lisää jäseniä**.

  ![Lisää jäseniä-komento](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)

7. Valitse **jäsenet** -sivu yksi tai useampia käyttäjiä tai laitteiden Lisää ryhmään ja valitse sivu alareunassa **Valitse** -painiketta ja lisää hänet ryhmään. **Käyttäjäruudussa** suodattaa näytön vastaavat tapahtumaa, mistä tahansa osasta käyttäjä tai laitteen nimen perusteella. Ei ole yleismerkkien hyväksytään ruudussa.

8. Voit poistaa jäseniä ryhmään **ryhmän - jäsenet** -sivu valitsemalla jäsen.

9. ***Membername*** -sivu, valitse **Poista** -komento ja vahvista valintasi tulee näkyviin.

  ![Poista jäsenet-komento](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)

9. Kun olet määrittänyt ryhmän jäsenet, valitse **Tallenna**.


## <a name="additional-information"></a>Lisätietoja

Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [On olemassa olevia ryhmiä](active-directory-groups-view-azure-portal.md)
* [Uuden ryhmän luominen ja jäsenten lisääminen](active-directory-groups-create-azure-portal.md)
* [Ryhmän asetusten hallinta](active-directory-groups-settings-azure-portal.md)
* [Ryhmän jäsenyyksien hallinta](active-directory-groups-membership-azure-portal.md)
* [Dynaaminen sääntöjen ryhmän käyttäjien hallinta](active-directory-groups-dynamic-membership-azure-portal.md)
