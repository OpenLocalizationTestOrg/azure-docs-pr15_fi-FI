<properties
    pageTitle="Resurssivaraukset Azure access | Microsoft Azure"
    description="Tarkastella ja hallita kaikkia Roolipohjainen käyttöoikeuksien valvonta-määrityksiä käyttäjän tai ryhmän Azure-portaalissa"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Näkymä access-varauksista käyttäjien ja ryhmien Azure Public preview - portaalissa

> [AZURE.SELECTOR]
- [Käyttäjän tai ryhmän käyttöoikeuksien hallinta](role-based-access-control-manage-assignments.md)
- [Käyttöoikeuksien hallinta resurssien mukaan](role-based-access-control-configure.md)

Kanssa Roolipohjainen käytön hallinta (RBAC) Azure Active Directory-esikatselussa, voit hallita access Azure resursseja. [Mikä on esikatselussa?](active-directory-preview-explainer.md)

Varattu RBAC Access ei tarkasti rajattuja, koska voivat rajoittaa oikeuksia kahdella tavalla:

- **Laajuus:** RBAC roolimäärityksiä on rajattu tietyn tilauksen, resurssiryhmä tai resurssi. Access annettavat koottuja käyttäjä ei voi käyttää muita resursseja, valitse saman tilauksen.
- **Rooli:** Varauksen laajuuden access on suodatettujen tietojen perusteella vielä roolin mukaan. Roolien voi olla ylätason, kuten omistaja tai tietyn, kuten virtuaalikoneen reader.

Roolien voi määrittää vain tilauksen, resurssiryhmä tai resurssi, joka on tehtävän vaikutusaluetta. Mutta voit tarkastella kaikkia access-varauksista tietyn käyttäjän tai ryhmän yhdessä paikassa.

Lisätietoja [käytön roolimäärityksiä hallittavan Azure tilaus-resurssien käyttöoikeuksien](role-based-access-control-configure.md)määrittämisestä.

##  <a name="view-access-assignments"></a>Näkymä access-varauksista

Jos haluat hakea yksittäisen käyttäjän tai ryhmän access-varauksista, Käynnistä Azure Active Directoryn [Azure portal](http://portal.azure.com).

1. **Azure Active Directory**-painikkeen valitseminen Jos tämä vaihtoehto ei ole näkyvissä siirtymisruudun luettelossa, valitse **Lisää palveluja** ja vieritä sitten alaspäin kohtaan **Azure Active Directory**.
2. Valitse **käyttäjät ja ryhmät**ja valitse joko **kaikki käyttäjät** tai **Kaikki ryhmät**. Tässä esimerkissä on keskittyä yksittäisille käyttäjille.
    ![Käyttäjien ja ryhmien Azure Active Directory - näyttökuva hallinta](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Hae käyttäjän nimi tai käyttäjätunnus.
4. Valitse käyttäjä-sivu **Azure resurssit** . Kaikki kyseisen käyttäjän access tehtävät näkyvät.

### <a name="read-permissions-to-view-assignments"></a>Voit tarkastella tehtäviä lukuoikeudet

Tällä sivulla näkyvät vain access-tehtävät, jotka sinulla on oikeus lukea. Esimerkiksi on lukuoikeudet tilaukseen A ja siirry Azure resurssit-sivu, voit tarkistaa käyttäjän tehtävät. Voit nähdä hänen access varaukset tilauksen A, mutta et näe, että hän on myös access-varauksista tilauksessa B.

## <a name="delete-access-assignments"></a>Poistaa access-tehtävät

Tämä sivu-voit poistaa access-tehtävät, jotka on määritetty suoraan käyttäjää tai ryhmää. Jos access-määritys on periytynyt ylemmän tason ryhmän, joudut resurssi ja tilauksen ja hallita varauksen siellä.

1. Valitse yksi, jonka haluat poistaa kaikki access-varauksista käyttäjän tai ryhmän-luettelosta.
2. Valitse **Poista** ja valitse sitten **Kyllä** Vahvista.
    ![Poista access määritys - näyttökuva](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

- Roolipohjainen käyttöoikeuksien valvonta [Käytä roolimäärityksiä hallittavan Azure tilauksen resurssien käytön](role-based-access-control-configure.md) aloittaminen
- Katso [RBAC valmiit roolit](role-based-access-built-in-roles.md)
