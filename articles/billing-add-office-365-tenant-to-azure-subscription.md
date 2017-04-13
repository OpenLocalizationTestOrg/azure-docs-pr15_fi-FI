<properties
    pageTitle="Käytä Office 365-Alihallinta Azure-tilauksen | Microsoft Azure"
    description="Opi lisää Office 365-kansio (Alihallinta) suhteen tekemään Azure-tilaukseen."
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="cjiang"/>

# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Office 365-Alihallinta liittäminen Azure tilauksen
Jos hankit Azure- ja Office 365-tilauksia erikseen aiemman ja haluat nyt voi käyttää Office 365-asiakasympäristön Azure tilauksesta, on helppo tehdä. Tässä artikkelissa kerrotaan, miten.

> [AZURE.NOTE] Tässä artikkelissa ei koske Enterprise Agreement (EA)-asiakkaille.

## <a name="quick-guidance"></a>Pika-ohjeet
Liitettävän Azure tilauksen Office 365-asiakasympäristöön Azure tilisi avulla voit lisätä Office 365-asiakasympäristöön ja liitä sitten Azure tilauksen Office 365-alihallintaan.

## <a name="detailed-steps"></a>Yksityiskohtaisia ohjeita
Tässä skenaariossa Kelley seinä on käyttäjä, jolla on Azure tilauksen tilissä kelley.wall@outlook.com. Kelley on myös Office 365-tilauksen tilissä kelley.wall@contoso.onmicrosoft.com. Nyt Kelley haluaa käyttää Office 365-alihallintaan ja Azure-tilaus.

![Kansion näyttökuva ja Azure Active Directory-tila](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Näyttökuva, Office 365: n admin center aktiiviset käyttäjät](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Edellytykset
Suhteen toimii oikein tarvitaan seuraavat edellytykset:

- Tarvitset Azure-tilauksen palvelun järjestelmänvalvojan tunnistetietoja. Apuyhteyshenkilöiden ei voi suorittaa alijoukkoa ohjeiden mukaisesti.
- Tarvitset Office 365-alihallintaan yleisen järjestelmänvalvojan tunnistetietoja.
- Sähköpostiosoite, palvelun järjestelmänvalvoja on ei sisälly Office 365-alihallintaan.
- Sähköpostiosoite, palvelun järjestelmänvalvoja on vastaa mitä tahansa yleisen järjestelmänvalvojan Office 365-alihallintaan.
- Jos käytät parhaillaan sähköpostiosoite, joka on Microsoft-tiliin ja käytössä on organisaatiotili, tilapäisesti muuttaa palvelun järjestelmänvalvoja Azure-tilauksesi toiseen Microsoft-tiliä. Voit luoda uuden Microsoft-tilin [Microsoft-tilin kirjautumissivulla](https://signup.live.com/).


Jos haluat muuttaa palvelun järjestelmänvalvoja, toimi seuraavasti:

1. Kirjautuminen [tilinhallinta-portaalissa](https://account.windowsazure.com/subscriptions).
2. Valitse muutettava tilaus.
3. Valitse **Muokkaa tilauksen tiedot**.

    ![Azure näyttökuva Tilaustiedot "Muokkaa Tilaustiedot" korostettuna kanssa](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

4. Kirjoita **Palvelun järjestelmänvalvoja** -ruutuun sähköpostiosoite, uusi palvelu järjestelmänvalvoja.

    !["Muokkaa tilauksen-valintaikkuna](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Liitä Office 365-asiakasympäristön Azure-tilauksella
Voit liittää Office 365-asiakasympäristön Azure-tilaus, toimimalla seuraavasti:

1.  Kirjaudu [tilinhallinta-portaalin](https://account.windowsazure.com/subscriptions) palvelun järjestelmänvalvojan tunnistetietoja.
2.  Valitse vasemmanpuoleisessa ruudussa **ACTIVE DIRECTORYSTA**.

    ![Active Directory näyttökuva tapahtuma](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

    > [AZURE.NOTE] Raportissa pitäisi näkyä ei ole Office 365-alihallintaan. Jos näet sen, voit ohittaa seuraavaan vaiheeseen.

    ![Näyttökuva Azure Active Directory oletuskansio](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Lisää Office 365-asiakasympäristön Azure-tilaukseen.

    a. Valitse **Uusi** > **DIRECTORY** > **MUKAUTETUN luominen**.

    ![Kansion näyttökuva ja Azure Active Directory-mukautettu luominen](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

    b. Valitse **Lisää hakemisto** -sivulla **kansion**, valitse **Käytä kansiolla**. Valitse Valitse **olen valmis kirjauduttava nyt**ja valitse **Valmis** ![loppuun kuvake](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Näyttökuva "Käytä kansiolla"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

    c-näppäinyhdistelmää. Kun olet kirjautunut, kirjaudu sisään Office 365-asiakasympäristöön yleisen järjestelmänvalvojan tunnistetiedot.

    ![Näyttökuva, Office 365: n yleinen järjestelmänvalvoja-kirjautuminen](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

    d. Valitse **Jatka**.

    ![Näyttökuva todentaminen](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

    e. Valitse **Kirjaudu ulos**.

    ![Näyttökuva Kirjaudu ulos](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

    f. Kirjaudu [tilinhallinta-portaalin](https://account.windowsazure.com/subscriptions) palvelun järjestelmänvalvojan tunnistetietoja.

    ![Näyttökuva, Azure-kirjautuminen](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

    g. Raportissa pitäisi näkyä koontinäytön Office 365-asiakasympäristöön.

    ![Näyttökuva Raporttinäkymät-ikkunan](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. Vaihda kansio Azure-tilaukseen liittyvää.

    a. Valitse **asetukset**.

    ![Azure näyttökuva perinteinen portaalin asetuksia-kuvake](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

    b. Valitse Azure-tilauksesi ja valitse sitten **Muokkaa HAKEMISTO**.
    ![Azure näyttökuva tilauksen Muokkaa hakemisto](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

    c-näppäinyhdistelmää. Valitse **Seuraava** ![seuraava-kuvake](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

    ![Näyttökuva "Muuta liittyvän hakemisto"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

    > [AZURE.WARNING] Näyttöön tulee varoitus, että kaikki apuyhteyshenkilöiden poistetaan.

    ![Azure-Vahvista-hakemisto-määritys](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

    >[AZURE.WARNING] Lisäksi kaikki [Roolipohjainen käyttöoikeuksien valvonta (RBAC)](./active-directory/role-based-access-control-configure.md) -käyttäjät, joilla määritetyt käyttöoikeudet aiemmin resurssin ryhmien myös poistaa. Näyttöön tulee varoitus maininnat kuitenkin vain apuyhteyshenkilöiden poisto.

    ![varattu-käyttäjät-poistettu-resurssi-ryhmät](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

    d. Valitse **Valmis** ![loppuun kuvake](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Voit nyt lisätä organisaation Office 365-tilien Azure Active Directory-vuokraajan apuyhteyshenkilöiden nimellä.

    a. Valitse **JÄRJESTELMÄNVALVOJAT** -välilehti ja valitse sitten **Lisää**.

    ![Azure näyttökuva perinteinen portaalin asetusten järjestelmänvalvojat-välilehti](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

    b. Organisaation tili ja Office 365-asiakasympäristöön, Azure tilaus ja valitse sitten **Valmis** ![loppuun kuvake](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Azure näyttökuva Lisää työtovereiden järjestelmänvalvoja-valintaikkuna](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

    c-näppäinyhdistelmää. Siirry **JÄRJESTELMÄNVALVOJAT** -välilehti. Raportissa pitäisi näkyä näkyviin muiden järjestelmänvalvojana organisaatiotili.

    ![Näyttökuva järjestelmänvalvojat-välilehti](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Seuraavaksi voit testata access työtovereiden järjestelmänvalvojaan.

    a. Kirjaudu ulos tilinhallinta-portaalissa.

    b. Avaa [tilinhallinta-portaalin](https://account.windowsazure.com/subscriptions) tai [Azure portal](https://portal.azure.com/).

    c-näppäinyhdistelmää. Jos Azure kirjautumissivulla on linkkiä, **Kirjaudu sisään organisaatiotiliä**, valitse linkki. Muussa tapauksessa Ohita tämä vaihe.

    ![Azure näyttökuva kirjautumissivulle](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

    d. Kirjoita muiden järjestelmänvalvojan tunnistetiedot ja valitse sitten **Kirjaudu sisään**.

    ![Azure näyttökuva kirjautumissivulle](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Seuraavat vaiheet
Aiheeseen liittyvät skenaariot ovat seuraavat:

- Sinulla on jo Office 365-tilauksen ja olet valmis Azure-tilauksen, mutta haluat käyttää aiemmin Office 365-käyttäjätilit Azure tilauksen.
- Olet Azure tilaaja ja haluat saada käyttäjille Office 365-tilauksen aiemmin Azure Active Directory-esiintymä.

Lisätietoja näiden tehtävien suorittamisesta on artikkelissa [käyttöä aiemmin Office 365-tili-Azure-tilauksesi tai päinvastoin](billing-use-existing-office-365-account-azure-subscription.md).
