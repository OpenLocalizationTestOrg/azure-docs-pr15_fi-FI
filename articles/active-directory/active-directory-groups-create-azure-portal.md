<properties
    pageTitle="Uuden ryhmän luominen Azure Active Directory-esikatselu | Microsoft Azure"
    description="Azure Active Directory-ryhmän luominen ja (jäsenet) käyttäjien lisääminen ryhmään"
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


# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Uuden ryhmän luominen Azure Active Directory-esikatselu

> [AZURE.SELECTOR]
- [Azure portal](active-directory-groups-create-azure-portal.md)
- [Azure perinteinen portal](active-directory-accessmanagement-manage-groups.md)
- [PowerShellin](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Tässä artikkelissa kerrotaan, miten voit luoda ja täytä Azure Active Directory (Azure AD) esikatselu uuteen ryhmään. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Ryhmän avulla voit suorittaa hallintatehtäviä, kuten käyttöoikeuksien tai käyttöoikeuksien määrittäminen yhdellä kertaa useita käyttäjiä tai laitteet.

## <a name="how-do-i-create-a-group"></a>Miten voin luoda ryhmän?

1. Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2. Valitse **Lisää palveluja**, anna **käyttäjien ja ryhmien** tekstiruutuun ja paina sitten **ENTER-näppäintä**.

  ![Avaava käyttäjien hallinta](./media/active-directory-groups-create-azure-portal/search-user-management.png)

3. Valitse **käyttäjät ja ryhmät** -sivu **Kaikki ryhmät**.

  ![Avaaminen ryhmät-sivu](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)

4. Valitse **käyttäjät ja ryhmät - kaikki ryhmät** -sivu valitsemalla **Lisää** -komento.

  ![Lisää-komennon valitseminen](./media/active-directory-groups-create-azure-portal/add-group-command.png)

5. Lisää nimi ja kuvaus ryhmän **ryhmä** -sivu.

6. Valitse jäsenet ryhmään, valitse **Vastuuhenkilö** **jäsenyyden tyyppi** -ruudussa ja valitse sitten **jäsenet**. Saat lisätietoja ryhmän jäsenyyden hallinta dynaamisesti [käyttäminen määritteet Ryhmäjäsenyyden lisäsääntöjä luomiseen](active-directory-groups-dynamic-membership-azure-portal.md).

  ![Jos haluat lisätä jäsenet](./media/active-directory-groups-create-azure-portal/select-members.png)

5. Valitse **jäsenet** -sivu yksi tai useampia käyttäjiä tai laitteiden Lisää ryhmään ja valitse sivu alareunassa **Valitse** -painiketta ja lisää hänet ryhmään. **Käyttäjäruudussa** suodattaa näytön vastaavat tapahtumaa, mistä tahansa osasta käyttäjä tai laitteen nimen perusteella. Ei ole yleismerkkejä hyväksytään ruudussa.

6. Kun olet lisännyt jäsenet-ryhmään, valitse **Luo** **ryhmä** -sivu.    

  ![Luo ryhmä vahvistus](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)




## <a name="additional-information"></a>Lisätietoja

Seuraavissa artikkeleissa on lisätietoja Azure Active Directory.

* [On olemassa olevia ryhmiä](active-directory-groups-view-azure-portal.md)
* [Ryhmän asetusten hallinta](active-directory-groups-settings-azure-portal.md)
* [Ryhmän jäsenien hallinta](active-directory-groups-members-azure-portal.md)
* [Ryhmän jäsenyyksien hallinta](active-directory-groups-membership-azure-portal.md)
* [Dynaaminen sääntöjen ryhmän käyttäjien hallinta](active-directory-groups-dynamic-membership-azure-portal.md)
