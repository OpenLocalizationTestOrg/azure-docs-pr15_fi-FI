<properties
    pageTitle="Azure Active Directory-B2C: Microsoft-tilin määrittäminen | Microsoft Azure"
    description="Anna kirjautumista ja kirjaudu sisään kuluttajille sovellukset, jotka on suojattu Azure Active Directory-B2C Microsoft-tilien kanssa."
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory-B2C: Anna kirjautumista ja kirjaudu sisään Microsoft-tilien kanssa kuluttajille

## <a name="create-a-microsoft-account-application"></a>Microsoft-tili-sovelluksen luominen

Käyttää Microsoft-tilin tunnistetiedot-tarjoajan Azure Active Directory (Azure AD) B2C, tarvitset Microsoft-tili-sovelluksen luominen ja anna oikeaa parametreilla. Tarvitset Microsoft-tiliä, voit tehdä tämän. Jos sinulla ei ole, saa osoitteessa [https://www.live.com/](https://www.live.com/).

1. [Microsoft Application rekisteröinti Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ja kirjaudu sisään Microsoft-tilin tunnistetiedot.
2. Valitse **Lisää sovellus**.

    ![Microsoft-tili - Lisää uusi sovellus](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Anna sovelluksen **nimi** ja valitse **Luo-sovellus**.

    ![Microsoft-tili - sovelluksen nimi](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Kopioi **Sovellustunnus**arvo. Sinun on se Microsoft-tilin määrittäminen kuin vuokraajan tunnistetietojen toimittaja.

    ![Microsoft-tili - tunnus](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. **Lisää** ympäristössä ja valitse **WWW**.

    ![Microsoftin tili - ympäristö Lisää](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsoft-tili – Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Kirjoita `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` **Uudelleenohjata URI** -kenttään. Korvaa **{vuokraajan}** oman vuokraajan nimi (esimerkiksi contosob2c.onmicrosoft.com).

    ![Microsoft-tili - uudelleenohjata URL-osoite](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Valitse **Luo uusi salasana** **Sovelluksen tietoja** -osassa. Kopioi näytössä uutta salasanaa. Sinun on se Microsoft-tilin määrittäminen kuin vuokraajan tunnistetietojen toimittaja. Salasana on tärkeän tunnistetiedon.

    ![Microsoft-tili - luo uusi salasana](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsoft-tili - uusi salasana](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Valitse ruutu, jossa **Live SDK tukevat** lukee **Lisäasetukset** -kohdassa. Valitse **Tallenna**.

    ![Microsoft-tili - Live SDK-tuki](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Määritä hyväksyttyyn tunnistetietojen toimittajaan vuokraajan Microsoft-tili

1. Seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa.
2. Valitse **tunnistetietojen toimittajat**B2C ominaisuudet-sivu.
3. Valitse **+ Lisää** yläreunaan sivu.
4. Antaa **nimi** tunnistetietojen toimittaja-määritys. Kirjoita esimerkiksi "MSA".
5. Valitse **tunnistetietojen toimittaja**, valitse **Microsoft-tili**ja valitse **OK**.
6. Valitse **Määritä tämä tunnistetietojen toimittaja** ja tunnus ja salasana, Microsoft-tilin sovellus, jonka loit aiemmin.
7. Valitse **OK** ja valitse sitten **Luo** Tallenna Microsoft-tilin määrittäminen.
