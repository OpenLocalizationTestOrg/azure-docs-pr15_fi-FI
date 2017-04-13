<properties
    pageTitle="Azure Active Directory-B2C: Monimenetelmäisen todentamisen | Microsoft Azure"
    description="Monimenetelmäisen todentamisen ottaminen käyttöön suojattuja Azure Active Directory-B2C kuluttajien osoittava-sovelluksissa"
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

# <a name="azure-active-directory-b2c-enable-multi-factor-authentication-in-your-consumer-facing-applications"></a>Azure Active Directory-B2C: Ota käyttöön Monimenetelmäisen todentamisen kuluttaja osoittava-sovelluksissa

Azure Active Directory (Azure AD) B2C integroituu suoraan [Azure Monimenetelmäisen todentamisen](../multi-factor-authentication/multi-factor-authentication.md) niin, että voit lisätä toisen layer Security kirjautumista ja kirjaudu sisään kokemukset kuluttaja osoittava-sovelluksissa. Ja voit tehdä tämän yksittäisen rivin koodin kirjoittamatta. Sovellus tukee tällä hetkellä puhelun ja teksti viestin vahvistus. Jos olet jo luonut kirjautuminen ja kirjaudu sisään, voit ottaa Monimenetelmäisen todentamisen.

> [AZURE.NOTE]
Monimenetelmäisen todentamisen myös voidaan ottaa käyttöön, kun luot kirjautumista ja kirjaudu sisään käytäntöjä, et pelkästään muokkaamalla aiemmin luotuja käytäntöjä.

Tämän ominaisuuden avulla sovellusten käsitellä tilanteita, joissa esimerkiksi seuraavat:

- Ei edellytä Monimenetelmäisen todentamisen käyttää sovelluksen, mutta edellyttävät käyttämään toiseen. Esimerkiksi kuluttaja voit kirjautua automaattisesti vakuutuksen sovelluksen sosiaalisen tai paikallinen tilillä, mutta se on tarkistettava puhelinnumero, ennen kuin käyttäminen home Vakuutushakemuksen rekisteröity samassa kansiossa.
- Ei edellytä Monimenetelmäisen todentamisen käyttää sovelluksen yleinen, mutta se käyttää se sisällä arkaluontoisia osia tarvitse. Esimerkiksi kuluttaja voit kirjautua pankin sovellukseen sosiaalisen tai paikallinen tili ja tarkista saldo, mutta se on tarkistettava puhelinnumeron ennen kuin yrität wire siirto.

## <a name="modify-your-sign-up-policy-to-enable-multi-factor-authentication"></a>Kirjautuminen käytäntöjen käyttöön Monimenetelmäisen todentamisen muokkaaminen

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **kirjautumisen käytännöt**.
3. Valitse Avaa kirjautumisen käytäntöjen (esimerkiksi "B2C_1_SiUp").
4. **Monimenetelmäisen todentamisen** ja kääntämällä sen **tila** **on**. Valitse **OK**.
5. Valitse **Tallenna** sivu yläreunassa.

Voit käyttää "Suorita"-ominaisuutta käytännön vahvistamiseksi kuluttaja-toiminto. Tarkista seuraavat:

Kuluttajatili luodaan hakemistossa ennen multi-factor Authentication-vaihe. Vaiheesta kuluttaja on kysyä yhteystietoluetteloonsa puhelinnumero ja vahvista se. Jos tiedot on tarkistettu, puhelinnumero on liitetty kuluttajatilissä myöhempää käyttöä varten. Vaikka kuluttaja peruuttaa tai pudottaa, voit pyytää hän Tarkista puhelinnumero uudelleen aikana seuraavan kirjautuminen (ja multi-factor todennus on käytössä).

## <a name="modify-your-sign-in-policy-to-enable-multi-factor-authentication"></a>Kirjaudu sisään käytäntöjen käyttöön Monimenetelmäisen todentamisen muokkaaminen

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **Kirjaudu sisään käytännöt**.
3. Valitse Kirjaudu sisään käytäntöjen (esimerkiksi "B2C_1_SiIn"), avaa se. Valitse sivu yläreunassa **Muokkaa** .
4. **Monimenetelmäisen todentamisen** ja kääntämällä sen **tila** **on**. Valitse **OK**.
5. Valitse **Tallenna** sivu yläreunassa.

Voit käyttää "Suorita"-ominaisuutta käytännön vahvistamiseksi kuluttaja-toiminto. Tarkista seuraavat:

Kun kuluttaja kirjautuu (Käytä sosiaalisia tai paikallinen-tiliä), jos varmennettu puhelinnumero on liitetty kuluttaja-tilille, hän pyytää vahvistamaan sen. Jos puhelinnumeroa ei ole liitetty, kuluttaja pyytää jokin ja vahvista se. Onnistunut tarkistus puhelinnumero on liitetty kuluttajatilissä myöhempää käyttöä varten.

## <a name="multi-factor-authentication-on-other-policies"></a>Monimenetelmäisen todentamisen muiden käytäntöjen

Kirjautuminen ja kirjaudu sisään käytäntöjen edellä kuvatulla on myös mahdollista käyttöön multi-factor authentication-kirjautuminen tai Kirjaudu sisään käytännöt ja salasanan palauttaminen käytännöt. Se on käytettävissä pian profiilin käytäntöjen muokkaaminen.
