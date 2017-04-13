<properties
    pageTitle="Ryhmä on jäsen Azure Active Directory-esikatselussa ryhmien hallinta | Microsoft Azure"
    description="Ryhmät voivat sisältää Azure Active Directoryn muista ryhmistä. Näin voit näiden jäsenyyksien hallinta."
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


# <a name="manage-the-groups-your-group-is-a-member-of-in-azure-active-directory-preview"></a>Ryhmä on jäsen Azure Active Directory-esikatselussa ryhmien hallinta

Ryhmät voivat sisältää Azure Active Directory-esikatselu muista ryhmistä. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Näin voit näiden jäsenyyksien hallinta.

## <a name="how-do-i-find-the-groups-my-group-is-a-member-of"></a>Mistä löydän ryhmät oman ryhmän jäsen on?

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita tekstiruutuun **käyttäjät ja ryhmät** ja paina sitten **ENTER-näppäintä**.

  ![Avaava käyttäjien hallinta](./media/active-directory-groups-membership-azure-portal/search-user-management.png)

3.  Valitse **käyttäjät ja ryhmät** -sivu **Kaikki ryhmät**.

  ![Avaaminen ryhmät-sivu](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)

4. Valitse **käyttäjät ja ryhmät - kaikki ryhmät** -sivu.

5. Valitse * *ryhmä - *Ryhmän_nimi* -** sivu, valitse **ryhmän jäsenyyksiä **.

  ![Avaaminen ryhmän jäsenyys-sivu](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)

6. Jos haluat lisätä ryhmäsi jäsen toiseen ryhmään, valitse **ryhmä - ryhmän jäsenyys** -sivu valitsemalla **Lisää** -komento.

7. Valita ryhmän **Valitse ryhmä** -sivu ja valitse sitten sivu alareunassa **Valitse** -painiketta. Voit lisätä ryhmäsi ryhmään vain yksi kerrallaan. **Käyttäjäruudussa** suodattaa näytön vastaavat tapahtumaa, mistä tahansa osasta käyttäjä tai laitteen nimen perusteella. Ei ole yleismerkkien hyväksytään ruudussa.

  ![Lisää Ryhmäjäsenyys](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)

8. Voit poistaa ryhmän jäsen toiseen ryhmään, valitse **ryhmä - ryhmän jäsenyys** -sivu, valitse ryhmä.

9. ***Groupname*** -sivu, valitse **Poista** -komento ja vahvista valintasi tulee näkyviin.

  ![Poista jäsenyys-komento](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)

9. Kun olet määrittänyt ryhmäjäsenyyksiä ryhmän, valitse **Tallenna**.


## <a name="additional-information"></a>Lisätietoja

Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [On olemassa olevia ryhmiä](active-directory-groups-view-azure-portal.md)
* [Uuden ryhmän luominen ja jäsenten lisääminen](active-directory-groups-create-azure-portal.md)
* [Ryhmän asetusten hallinta](active-directory-groups-settings-azure-portal.md)
* [Ryhmän jäsenien hallinta](active-directory-groups-members-azure-portal.md)
* [Dynaaminen sääntöjen ryhmän käyttäjien hallinta](active-directory-groups-dynamic-membership-azure-portal.md)
