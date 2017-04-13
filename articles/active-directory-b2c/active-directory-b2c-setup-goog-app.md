<properties
    pageTitle="Azure Active Directory-B2C: Google + määritys | Microsoft Azure"
    description="Anna kirjautumista ja kirjaudu sisään kuluttajille sovellukset, jotka on suojattu Azure Active Directory-B2C Google +-tilien kanssa."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory-B2C: Anna kirjautumista ja kirjaudu sisään Google +-tilien kanssa kuluttajille

## <a name="create-a-google-application"></a>Google +-sovelluksen luominen

Käyttää Google + tunnistetietojen toimittaja-Azure Active Directory (Azure AD) B2C, sinun on annettava oikea parametreilla ja Google +-sovelluksen luominen. Tarvitset tilin Google + toiminto. Jos sinulla ei ole, saa osoitteessa [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).

1. Siirry [Google kehittäjät konsolin](https://console.developers.google.com/) ja kirjaudu sisään Google + tilin tunnistetiedot.
2. Valitse **Luo projekti**, kirjoita **projektinimi**ja valitse sitten **Luo**.

    ![Google + – aloittaminen](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + - uusi projekti](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Valitse **API hallinta** ja valitse sitten vasemmanpuoleisesta siirtymisruudusta **tunnistetiedot** .
4. Valitse yläreunassa **OAuth suostumus näyttö** -välilehti.

    ![Google + - tunnistetiedot](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Valitse Määritä kelvollinen **sähköpostiosoite**, **tuotteen nimi**ja valitse **Tallenna**.

    ![Google + - OAuth suostumusta näyttö](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Valitse **Uusi tunnistetiedot** ja valitse sitten **OAuth Asiakastunnus**.

    ![Google + - OAuth suostumusta näyttö](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Valitse **sovelluksen tyyppi**-kohdasta **verkkosovellus**.

    ![Google + - OAuth suostumusta näyttö](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Anna sovelluksen **nimi** , kirjoita `https://login.microsoftonline.com` **alkuperistä valtuutettuja JavaScript** -kenttään ja `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` **sallittu uudelleenohjata URI** -kenttään. Korvaa **{vuokraajan}** oman vuokraajan nimi (esimerkiksi contosob2c.onmicrosoft.com). **{Vuokraajan}** -arvon kirjainkoko on merkitsevä. Valitse **Luo**.

    ![Google + – Luo Ostajantunnus](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Kopioi **Asiakastunnus** - ja **asiakkaan salaisuus**arvot. Sinun on molempien on määritettävä Google + vuokraajan tunnistetietojen toimittaja. **Asiakkaan toiminta** perustuu tärkeän tunnistetiedon.

    ![Google + - asiakasohjelman salaisuus](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Määritä Google + hyväksyttyyn tunnistetietojen toimittajaan vuokraajan

1. Seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa.
2. Valitse **tunnistetietojen toimittajat**B2C ominaisuudet-sivu.
3. Valitse **+ Lisää** yläreunaan sivu.
4. Antaa **nimi** tunnistetietojen toimittaja-määritys. Kirjoita esimerkiksi "G +".
5. Valitse **tunnistetietojen toimittaja**, valitse **Google**ja valitse **OK**.
6. Valitse **Määritä tämä tunnistetietojen toimittaja** ja kirjoita Asiakastunnus ja asiakkaan salaisuus Google +-sovelluksen aiemmin luomasi.
7. Valitse **OK** ja valitse sitten Tallenna kokoonpanosi Google + **luominen** .
