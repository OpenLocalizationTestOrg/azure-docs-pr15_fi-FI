<properties
    pageTitle="Azure Active Directory-B2C: Facebook-määritys | Microsoft Azure"
    description="Anna kirjautumista ja kirjaudu sisään kuluttajille sovellukset, jotka on suojattu Azure Active Directory-B2C Facebook-tilien kanssa."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory-B2C: Anna kirjautumista ja kirjaudu sisään kuluttajille Facebook-tilin kanssa

## <a name="create-a-facebook-application"></a>Facebook-sovelluksen luominen

Käyttämään Facebook tunnistetietojen toimittaja-Azure Active Directory (Azure AD) B2C sinun on annettava oikea parametreilla ja Facebook-sovelluksen luominen. Sinun on suoritettava toiminto Facebook-tilin. Jos sinulla ei ole, saa osoitteessa [https://www.facebook.com/](https://www.facebook.com/).

1. [Sovelluskehittäjille Facebook](https://developers.facebook.com/) -sivustossa ja kirjaudu sisään Facebook-tilin tunnistetiedot.
2. Jos et ole vielä tehnyt haluat rekisteröidä Facebook-kehittäjän. Voit tehdä tämän valitsemalla (Valitse sivun oikeassa yläkulmassa) **rekisteröidä** , Hyväksy Facebook's käytännöt ja rekisteröinti vaiheiden avulla.
3. Valitse **Omat sovellukset** ja valitse sitten **Lisää uusi sovellus**. Valitse **sivuston** kuin ympäristö ja valitse sitten **Ohita ja luoda Sovellustunnus**.

    ![Facebook - uuden sovelluksen lisääminen](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook - Lisää uusi sovellus - sivusto](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook - sovelluksen tunnuksen luominen](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Lomakkeeseen **Näyttönimi**, voimassa olevaa **Yhteyshenkilön sähköposti**, sopiva **luokka**, ja sitten **Luo Sovellustunnus**. Tämä edellyttää, että Hyväksy Facebook-ympäristö käytännöt ja viimeistele online suojaus-valintaruutu.

    ![Facebook - Luo uusi sovelluksen tunnus](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. Valitse vasemmassa siirtymisruudussa **asetukset** .
6. Valitse **+ Lisää ympäristö** ja valitse sitten **sivuston**.

    ![Facebook - asetukset](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Facebook - asetukset - sivusto](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. Kirjoita [https://login.microsoftonline.com/](https://login.microsoftonline.com/) **Sivuston URL-osoite** -kenttään ja valitse sitten **Tallenna muutokset**.

    ![Facebook - sivuston URL-osoite](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Kopioi **Sovellustunnus**arvo. Valitse **Näytä** ja kopioi **App salaisuus**arvo. Sinun on molempien on määritettävä Facebook tunnistetietojen toimittaja vuokraajan. **Sovelluksen toiminta** perustuu tärkeän tunnistetiedon.

    ![Facebook - Sovellustunnus ja sovelluksen salaisuus](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Valitse **+ Lisää tuotteen** vasemmanpuoleisesta siirtymisruudusta ja valitse **Aloita** -painike **Kirjautuminen Facebook**-kohdan vieressä.

    ![Facebook - Facebook-kirjautuminen](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Kirjoita `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` **Asiakkaan OAuth-asetukset** -kohdassa **kelvollinen OAuth uudelleenohjata URI** -kenttään. Korvaa **{vuokraajan}** oman vuokraajan nimi (esimerkiksi contosob2c.onmicrosoft.com). Valitse sivun alalaidassa **Tallenna muutokset** .

    ![Facebook - OAuth uudelleenohjaus URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Jos haluat tehdä Facebook-sovelluksen käytettävissä Azure AD B2C, haluat julkaista yleiseen käyttöön. Voit tehdä tämän napsauttamalla **Sovelluksen Tarkista** vasemmanpuoleisessa siirtymisruudussa ja poistamalla Vaihda **Kyllä** sivun yläreunassa ja valitsemalla **Vahvista**.

    ![Facebook - sovelluksen julkisen](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Määritä Facebook tunnistetietojen toimittaja vuokraajan

1. Seuraavasti [B2C ominaisuudet-sivu](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Siirry Azure-portaalissa.
2. Valitse **tunnistetietojen toimittajat**B2C ominaisuudet-sivu.
3. Valitse **+ Lisää** yläreunaan sivu.
4. Antaa **nimi** tunnistetietojen toimittaja-määritys. Kirjoita esimerkiksi "FB".
5. Valitse **tunnistetietojen toimittaja**, valitse **Facebook**ja valitse **OK**.
6. Valitse **Määritä tämä tunnistetietojen toimittaja** ja kirjoita Sovellustunnus ja sovelluksen salaisuus (, Facebook-sovelluksen aiemmin luomasi) **Asiakastunnus** ja **asiakkaan salaisuus** kentät tarpeen mukaan.
7. Valitse **OK**ja valitse sitten **Luo** Tallenna Facebook-määritys.
