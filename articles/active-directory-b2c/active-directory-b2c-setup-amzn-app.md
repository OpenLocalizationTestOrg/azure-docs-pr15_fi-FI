<properties
    pageTitle="Azure Active Directory-B2C: Amazon määritys | Microsoft Azure"
    description="Anna kirjautumista ja kirjaudu sisään kuluttajille sovellukset, jotka on suojattu Azure Active Directory-B2C Amazon-tilien kanssa."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory-B2C: Anna kirjautumista ja kirjaudu sisään kuluttajille Amazon-tilien kanssa

## <a name="create-an-amazon-application"></a>Amazon-sovelluksen luominen

Käyttämään Amazon tunnistetietojen toimittaja-Azure Active Directory (Azure AD) B2C sinun on annettava oikea parametreilla ja Amazon-sovelluksen luominen. Sinulla on oltava Amazon tili toiminto. Jos sinulla ei ole, saa osoitteessa [http://www.amazon.com/](http://www.amazon.com/).

1. Siirry [Amazon Developer Centerissä](https://login.amazon.com/) ja kirjaudu sisään Amazon tilin tunnistetiedot.
2. Jos et ole vielä tehnyt **Rekisteröidy**, developer rekisteröinti ohjeiden mukaisesti ja hyväksy käytännön.
3. Valitse **Rekisteröi uusi sovellus**.

    ![Uuden sovelluksen Amazon-sivustosta](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Hakemuksen tiedot (**nimi**, **kuvaus**ja **Yksityisyyden ilmoitus URL-osoite**) ja valitse **Tallenna**.

    ![Sovelluksen tietojen uusi sovellus Amazon osoitteessa rekisteröitymisestä](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. **Web-sivuston asetukset** -osassa kopioi **Asiakastunnus** - ja **Asiakkaan salaisuus**arvot. (Sinun tarvitse näkyä tämä valitsemalla **Näytä salaisuus** .) Sinun on molempien on määritettävä Amazon vuokraajan tunnistetietojen toimittaja. Valitse kohta alareunassa **Muokkaa** . **Asiakkaan toiminta** perustuu tärkeän tunnistetiedon.

    ![Ostajantunnus ja asiakkaan salaisen tarjoaa uuden sovelluksen osoitteessa Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Kirjoita `https://login.microsoftonline.com` **Sallittu JavaScript-alkuperistä** -kenttään ja `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` **Voi palauttaa URL** -kenttään. Korvaa **{vuokraajan}** oman vuokraajan nimi (esimerkiksi contoso.onmicrosoft.com). Valitse **Tallenna**. **{Vuokraajan}** -arvon kirjainkoko on merkitsevä.

    ![Palauttaa URL- ja JavaScript-alkuperistä tarjoaa uuden sovelluksen osoitteessa Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Määritä Amazon hyväksyttyyn tunnistetietojen toimittajaan vuokraajan

1. Seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa.
2. Valitse **tunnistetietojen toimittajat**B2C ominaisuudet-sivu.
3. Valitse **+ Lisää** yläreunaan sivu.
4. Antaa **nimi** tunnistetietojen toimittaja-määritys. Kirjoita esimerkiksi "Amzn".
5. Valitse **tunnistetietojen toimittaja**, valitse **Amazon**ja valitse **OK**.
6. Valitse **Määritä tämä tunnistetietojen toimittaja** ja kirjoita Asiakastunnus ja asiakkaan salaisuus Amazon sovelluksen aiemmin luomasi.
7. Valitse **OK** ja valitse sitten **Luo** Tallenna Amazon kokoonpanosi.
