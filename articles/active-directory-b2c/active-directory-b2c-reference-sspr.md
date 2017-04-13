<properties
    pageTitle="Azure Active Directory-B2C: Omatoiminen salasanan | Microsoft Azure"
    description="Aiheen näytetään, miten voit määrittää omatoimiseen salasanan palauttaminen lukijoille Azure Active Directory-B2C-"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory-B2C: Palauta lukijoille käyttäjien Omatoiminen salasanan määrittäminen

Käyttäjien Omatoiminen salasanan palauttaminen-toiminnon lukijoille (joka on rekisteröityi paikalliset tilit) palauttaa yhteiskäyttöä salasanansa. Tämä merkittävästi vähentää olevien tukihenkilöstöä, erityisesti, jos sovellus on miljoonia kuluttajille käyttämällä säännöllisin väliajoin. Tällä hetkellä vain tuetaan käyttäen varmennettu sähköpostiosoite palautus-menetelmää. Lisätään myöhemmin palautuksen menetelmiä (varmennettu puhelinnumero, tietoturvaan liittyvät kysymykset jne.).

> [AZURE.NOTE]
Tämä artikkeli koskee käyttäjien Omatoiminen salasanan palauttaminen käyttää kirjautumisen käytännön kontekstissa. Jos sinun on täysin mukautettavissa salasanan kutsuttu sovelluksestasi käytäntöjä, lue [artikkeli](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Oletusarvon mukaan hakemistossa ei ole käyttäjien Omatoiminen salasanan palauttaminen käytössä. Voit ottaa sen käyttöön noudattamalla seuraavia ohjeita:

1. Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com/) tilauksen järjestelmänvalvojana. Tämä on sama työpaikan tai oppilaitoksen tilille tai samalla Microsoft-tili, hakemiston luomiseen.
2. Siirry vasemman reunan siirtymispalkissa Active Directory-tunniste.
3. Etsi hakemistossa kohdassa **Hakemisto** -välilehti ja valitse se.
4. Valitse **määritys** -välilehti.
5. Vieritä näyttöä alaspäin **käyttäjän salasanan vaihtamista koskeva käytäntö** -kohtaan ja **salasanan käyttävät käyttäjät Palauta** -vaihtoehto, jos haluat **Kyllä**. Huomaa, että **Vaihtoehtoinen sähköpostiosoite** -vaihtoehto on valittuna; Jätä se ennalleen.

    ![Omatoiminen salasanan palauttaminen](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Valitse sivun alareunassa **Tallenna** . Olet valmis!

Testaa, minkä tahansa kirjautumisen käytännön, joka on paikalliset tilit Liittoutuminen "Suorittaa nyt"-toimintoa käytetään. Paikallisen tilin-kirjautuminen sivulla (kun kirjoitat sähköpostiosoitteen ja salasanan, tai käyttäjälle käyttäjänimen ja salasanan), valitse **Eikö kirjautuminen onnistu?** vahvistamiseksi kuluttaja-toiminto.

> [AZURE.NOTE]
Käyttäjien Omatoiminen salasanan palauttaminen sivut voi mukauttaa [yrityksen yrityksen tunnuksen-toiminnon](../active-directory/active-directory-add-company-branding.md)avulla.
