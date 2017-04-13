<properties
    pageTitle="Jakaa yhden Azure AD-vuokraajan Office 365: ssä ja Azure-tilauksissa | Microsoft Azure"
    description="Lue, miten voit jakaa Office 365: n Azure AD-vuokraajan ja sen käyttäjät Azure-tilauksesi ja päinvastoin"
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
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Käytä olemassa olevan Office 365-tili Azure-tilaus tai päinvastoin
Skenaario: Olet jo ole Office 365-tilausta ja Azure-tilauksen, mutta haluat käyttää aiemmin Office 365-käyttäjätilit Azure tilauksen. Voit myös ovat Azure tilaaja ja haluat saada käyttäjille Office 365-tilauksen aiemmin Azure Active Directoryn. Tässä artikkelissa kerrotaan, miten helppoa on saavuttamisessa.

> [AZURE.NOTE] Tässä artikkelissa ei koske Enterprise Agreement (EA)-asiakkaille. Jos tarvitset apua milloin tahansa tämän artikkelin, [Ota yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pääset nopeasti selvittää ongelmaa.


## <a name="quick-guidance"></a>Pika-ohjeet

- Jos olet jo Office 365-tilauksen ja haluat Azure rekisteröityminen, käytä **kirjautua sisään organisaation tiliä** -vaihtoehto. Jatka sitten Office 365-tilisi Azure rekisteröintiprosessi loppuun. Katso [tämän artikkelin toimimalla seuraavasti](#s1).

- Jos olet jo ole Azure-tilausta ja haluat tilaan Office 365: n, Kirjaudu Office 365: n Azure-tilisi kanssa. Jatka sitten rekisteröitymisen vaiheet. Kun olet suorittanut kirjautumisen, Office 365-tilauksen lisätään samaan Azure Active Directory-esiintymä, joka kuuluu Azure tilauksen. Lisätietoja on artikkelissa osan [yksityiskohtaisia ohjeita tämän artikkelin](#s2).

>[AZURE.NOTE] Hae Office 365-tilauksen tilin käyttämällä kirjautumisen on oltava Azure Active Directory vuokraajan yleisen järjestelmänvalvojan tai laskutuksen järjestelmänvalvoja hakemisto-roolin jäsen. [Opi määrittämään Azure Active Directory-roolin](#how-to-know-your-role-in-your-azure-active-directory).

Mitä tapahtuu, kun lisäät tilauksen tiliin on artikkelissa [artikkelin Taustatietoja](#background-information).

## <a name="detailed-steps"></a>Yksityiskohtaisia ohjeita
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Tapaus 1: Office 365-käyttäjät, jotka haluat ostaa Azure
Tässä tilanteessa oletetaan Kelley seinä on käyttäjä, joka on Office 365-tilauksen, ja voit tilata Azure suunnittelu. On kaksi muita aktiiviset käyttäjät-Jane ja Marianne. Kelley on tili on admin@contoso.onmicrosoft.com.

![Käyttäjän Office 365-hallintakeskukseen](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Voit rekisteröityä Azure toimimalla seuraavasti:

1. Tilaa Azure osoitteessa [Azure.com](https://azure.microsoft.com/). Valitse **Kokeile ilmaiseksi**. Valitse seuraavalla sivulla **Aloita nyt**.

    ![Kokeile Azure ilmaiseksi.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Valitse **Kirjaudu sisään organisaation tiliä**.

    ![Azure kirjautuminen.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Kirjaudu sisään Office 365-tilille. Tässä tapauksessa on Kelley käyttäjän Office 365-tilille.

    ![Kirjaudu sisään Office 365-tilille.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Täytä tiedot ja suorita rekisteröintiprosessi loppuun.

    ![Täytä tiedot ja viimeistele rekisteröityminen.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Valitse Aloita oma palvelun hallinta.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Nyt olet valmis. Azure-portaalissa pitäisi näkyä näkyvä samoille käyttäjille. Voit tarkistaa tämän toimimalla seuraavasti:

1. Valitse näytön aiemmin **hallinnoida Omat palvelu** .
2. Valitse **Selaa**ja valitse sitten **Active Directorysta**.

    ![Valitse Selaa ja valitse sitten Active Directorysta.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Valitse **käyttäjiä**.

    ![Käyttäjät-välilehti](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Kaikki käyttäjät, mukaan lukien Kelley, näkyvät oikein.

    ![Luettelo käyttäjistä](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Tapaus 2: Azure käyttäjät, jotka haluat ostaa Office 365: ssä

Tässä skenaariossa Kelley seinä on käyttäjä, jolla on Azure tilauksen tilissä admin@contoso.onmicrosoft.com. Kelley haluaa tilata Office 365: ssä ja käyttää hän on jo Azure samassa kansiossa.

>[AZURE.NOTE] Saat Office 365-tilauksen-tiliä, jota käytät kirjautumista varten on oltava Azure Active Directory vuokraajan yleisen järjestelmänvalvojan tai laskutuksen järjestelmänvalvoja hakemisto-roolin jäsen. [Lue, miten voit tietää Azure Active Directory-roolin](#how-to-know-your-role-in-your-azure-active-directory).

![Azure portaalin Tilausasetukset](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure-portaalin Active Directory-käyttäjät](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Jos haluat tilata Office 365: ssä, toimi seuraavasti:

1. Siirry [Office 365: n product-sivu](https://products.office.com/business)ja valitse sitten suunnitelma, joka sopii puolestasi.
2. Kun olet valinnut suunnitelma, seuraava sivu avautuu. Eivät täytä lomake. Valitse sivun oikeassa yläkulmassa **Kirjaudu sisään** .

    ![Office 365-kokeiluversion sivu](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Kirjaudu sisään tilin tunnistetiedot. Tässä esimerkissä on Kelley käyttäjän tiliä.

    ![Office 365-kirjautuminen](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Valitse **kokeile nyt**.

    ![Office 365: n tilauksen vahvistus.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Valitse tilauksen vastaanotto-sivulla **Jatka**.

    ![Office 365: n vastaanotto](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Nyt olet valmis. Office 365-hallintakeskuksessa pitäisi näkyä käyttäjien Contoso-hakemistosta näkyvät aktiiviset käyttäjät. Voit tarkistaa tämän toimimalla seuraavasti:

1. Avaa Office 365-hallintakeskuksessa.
2. Laajenna **käyttäjät**ja valitse sitten **Aktiiviset käyttäjät**.

    ![Office 365: n admin center käyttäjät](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Miten voit tietää tehtäväsi Azure Active Directoryssa

1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse **Selaa**ja valitse sitten **Active Directorysta**.

    ![Active Directory Azure-portaalissa](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Valitse **käyttäjiä**.

    ![Portaalin oletusarvon Azure Active Directory-käyttäjät](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Valitse käyttäjä. Tässä esimerkissä käyttäjä on Kelley seinään.

    Huomaa **ROOLI ORGANISAATIOSSA**kenttä.

    ![Azure portaalin käyttäjätiedot](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Saat lisätietoja Azure- ja Office 365-tilaukset
Office 365: n ja Azure Azure Active Directory-palvelun avulla voit hallita käyttäjiä ja tilaukset. Harkitse Azure directory eli säilö, jossa voit ryhmitellä käyttäjiä ja tilaukset. Jos haluat käyttää samaa käyttäjätili Azure- ja Office 365-tilauksia, varmista, että tilaukset luodaan samassa kansiossa. Ota huomioon seuraavat seikat:

- Tilauksen luodaan hakemiston ei tavalla ympärille.
- Käyttäjät kuuluvat hakemistojen selaaminen, ei tavalla ympärille.
- Tilauksen viedään maihin tilaus luova käyttäjä hakemistossa. Tuloksena Office 365-tilauksesi on yhdistetty saman tilin tilana Azure tilauksen käytettäessä tilin luominen Office 365-tilaukseen.

![Taustatietoja](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Lisätietoja on artikkelissa [miten Azure tilaukset liittyvät Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

>[AZURE.NOTE] Yksittäisten käyttäjien hakemiston omistamasi Azure tilaukset.

>[AZURE.NOTE] Kansion itsensä omistamasi Office 365-tilauksia. Jos hakemiston käyttäjillä on tarvittavat käyttöoikeudet, ne toimivat nämä tilaukset.

## <a name="next-steps"></a>Seuraavat vaiheet
Jos olet hankkinut Azure ja Office 365-tilauksia erikseen aiemman ja haluat käyttää Office 365-asiakasympäristön Azure tilauksesta, katso [Liitä Office 365-Alihallinta Azure-tilauksen](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Jos sinulla on edelleen kysymyksiä, [Ota yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pääset nopeasti selvittää ongelmaa.
