<properties
    pageTitle="Azure Active Directory-B2C: LinkedIn-määritys | Microsoft Azure"
    description="Kirjautuminen ja kirjaudu sisään tarjota kuluttajille sovellukset, jotka on suojattu Azure Active Directory-B2C LinkedIn-tilien kanssa"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory-B2C: Anna kirjautumista ja kirjaudu sisään kuluttajille LinkedIn-tilin kanssa

## <a name="create-a-linkedin-application"></a>LinkedIn-sovelluksen luominen

Käyttämään LinkedIn tunnistetietojen toimittaja-Azure Active Directory (Azure AD) B2C, sinun on annettava oikea parametreilla ja LinkedIn-sovelluksen luominen. Sinun on suoritettava toiminto LinkedIn-tilin. Jos sinulla ei ole, saa osoitteessa [https://www.linkedin.com/](https://www.linkedin.com/).

1. [LinkedIn-kehittäjille sivuston](https://www.developer.linkedin.com/) ja kirjaudu sisään LinkedIn-tilin tunnistetiedot.
2. Valitse yläreunan valikkorivillä **Omat sovellukset** ja valitse sitten **Sovelluksen luominen**.

    ![LinkedIn - uusi sovellus](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. **Luo uusi sovellus** lomakkeessa Täytä tarvittavat tiedot (**Yrityksen nimi**, **nimi**, **kuvaus**, **Sovelluksen logon URL-osoite**, **Sovelluksen käyttöä**, **Sivuston URL-osoite**, **Yrityksen sähköpostiosoite** ja **Työpuhelin**).
4. Hyväksy **LinkedIn API käyttöehdot** ja valitse **Lähetä**.

    ![LinkedIn - Rekisteröi-sovellus](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Kopioi **Asiakastunnus** - ja **Asiakkaan salaisuus**arvot. (Voit hakea muistiinpanoja kohdassa **Todentaminen avaimet**.) Sinun on molempien on määritettävä LinkedIn tunnistetietojen toimittaja vuokraajan.

    >[AZURE.NOTE] **Asiakkaan toiminta** perustuu tärkeän tunnistetiedon.

6. Kirjoita `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` (kohdassa **OAuth 2.0**) **Valtuutettuja uudelleenohjata URL** -kenttään. Korvaa **{vuokraajan}** oman vuokraajan nimi (esimerkiksi contoso.onmicrosoft.com). Valitse **Lisää**ja valitse sitten **Päivitä**. **{Vuokraajan}** -arvon kirjainkoko on merkitsevä.

    ![LinkedIn - asetukset-sovellus](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Määritä LinkedIn tunnistetietojen toimittaja vuokraajan

1. Seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa.
2. Valitse **tunnistetietojen toimittajat**B2C ominaisuudet-sivu.
3. Valitse **+ Lisää** yläreunaan sivu.
4. Antaa **nimi** tunnistetietojen toimittaja-määritys. Kirjoita esimerkiksi "LI".
5. Valitse **tunnistetietojen toimittaja**, valitse **LinkedIn**ja valitse **OK**.
6. Valitse **Määritä tämä tunnistetietojen toimittaja** ja kirjoita Asiakastunnus ja asiakkaan salaisuus aiemmin luomasi LinkedIn-sovelluksen.
7. Valitse **OK** ja valitse sitten **Luo** Tallenna LinkedIn-määritys.
