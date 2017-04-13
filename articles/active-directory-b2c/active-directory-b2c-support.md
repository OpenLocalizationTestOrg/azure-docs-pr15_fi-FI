<properties
    pageTitle="Azure Active Directory-B2C: Tukevat | Microsoft Azure"
    description="Voit siirtää tukipyyntöjä Azure Active Directory-B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-file-support-requests"></a>Azure Active Directory-B2C: Tiedoston tukipyyntöjä

Voit jättää tukipyyntöjä Azure Active Directory (Azure AD) B2C Azure-portaalissa seuraavien ohjeiden mukaisesti:

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Siirry B2C vuokraajasta toiseen alihallintaa, johon on liitetty Azure tilauksen. Tämä on yleensä työntekijän vuokraajan tai Azure-tilauksen rekisteröinnin luodaan oletusarvon vuokraajan. Lisätietoja on artikkelissa [Azure tilauksen suhteesta Azure AD](active-directory-how-subscriptions-associated-directory.md#how-an-azure-subscription-is-related-to-azure-ad).

    ![Tuki - valitsin alihallintoihin](./media/active-directory-b2c-support/support-switch-dir.png)

3. Kun olet alihallinnat, valitse **Ohje- + tuki**.

    ![Tuki - ohjeen + tuki](./media/active-directory-b2c-support/support-support.png)

4. Valitse **Uusi tukevat pyynnön**.

    ![Tuki - uusi](./media/active-directory-b2c-support/support-new.png)

5. **Perustiedot** -sivu-nämä tiedot ja valitse **Seuraava**.

    - **Ongelman tyyppi** on **tekninen**.
    - Valitse haluamasi **tilaus**.
    - **Palvelu** on **Active Directory**.
    - Valitse haluamasi **tuki suunnitelma**. Jos sinulla ei ole, voit rekisteröityä, yksi [tähän](https://azure.microsoft.com/en-us/support/plans/).

    ![Tuki - perusteet](./media/active-directory-b2c-support/support-basics.png)

6. **Ongelma** -sivu-nämä tiedot ja valitse **Seuraava**.

    - Valitse haluamasi **vakavuus** taso.
    - **Ongelmatyyppi** on **B2C**.
    - Valitse haluamasi **luokka**.
    - Kuvaile ongelmaa **tiedot** -kenttään. Anna tiedot, kuten B2C vuokraajan nimi, kuvaus ongelma, virhesanomista, korrelaatio tunnukset (jos saatavilla) ja niin edelleen.
    - Anna päivämäärä ja kellonaika (mukaan lukien aikavyöhyke), ongelma ilmeni **aikaväli** -kenttään.
    - Valitse **tiedoston lataaminen**Lataa kaikki näyttökuvat ja tiedostoja, jotka mielestäsi auttaa ratkaisemaan ongelman.

    ![Tuki - ongelma](./media/active-directory-b2c-support/support-problem.png)

7. Lisää yhteystietosi **Yhteystiedot** -sivu. Valitse **Luo**.

    ![Tuki - yhteyshenkilö](./media/active-directory-b2c-support/support-contact.png)

8. Kun olet lähettänyt tukipyynnön, voit valvoa sitä valitsemalla **Ohjeen + tuki** Startboard ja **Hallitse tuki pyytää**.

## <a name="known-issue-filing-a-support-request-in-the-context-of-a-b2c-tenant"></a>Tunnettu ongelma: siirtämiseen tukipyynnön B2C palvelutili yhteydessä

Jos vastattu vaiheessa 2 Edellä kuvattujen ja yritä luoda tukipyynnön B2C vuokraajan kontekstissa, näet seuraavan virheen.

> [AZURE.IMPORTANT]
> Ei yrittää kirjautua B2C vuokraajan Azure uusi tilaus.  

![Tuki - tilausta ei ole](./media/active-directory-b2c-support/support-no-sub.png)
