<properties
    pageTitle="Azure Active Directory-B2C: Mukautetut määritteet | Microsoft Azure"
    description="Mukautetut määritteet käyttämisestä Azure Active Directory-B2C kerää tietoja lukijoille"
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

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory-B2C: Käytä mukautettuja määritteitä kerää tietoja lukijoille

Azure Active Directory (Azure AD) B2C hakemistossa sisältyy valmis joukko tietojen (määritteitä): etunimi, Sukunimi, kaupunki, postinumero ja määritteet. Jokainen kuluttaja osoittava-sovellus on kuitenkin yksilöllinen vaatimukset, mitä määritteet hakemaan kuluttajille. Azure AD-B2C voit laajentaa määritteitä, jotka on tallennettu kuluttaja-tileille joukko. Voit luoda mukautettuja määritteitä [Azure portal](https://portal.azure.com/) ja käyttää sitä käyttäjän kirjautuminen käytännöt alla kuvatulla tavalla. Voit kuitenkin lukea ja kirjoittaa seuraavanlaisia määritteitä [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md)avulla.

> [AZURE.NOTE]
Mukautetut määritteet Käytä [Azure AD Graph API Directory rakenteen tunnisteet](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## <a name="create-a-custom-attribute"></a>Luo mukautettu määrite

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **käyttäjän määritteet**.
3. Valitse **+ Lisää** yläreunaan sivu.
4. Anna **nimi** mukautetun määritteen (esimerkiksi "ShoeSize") ja halutessasi **kuvaus**. Valitse **Luo**.

    > [AZURE.NOTE]
    Vain "Merkkijonon" **Tietotyyppi** on tällä hetkellä käytettävissä.

Mukautettu määrite on nyt saatavilla luettelosta **käyttäjän määritteet**ja ilmoittautuminen käytännöt käytettäviksi.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Mukautetun määritteen käyttäminen kirjautumisen käytäntö

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **kirjautumisen käytännöt**.
3. Valitse Avaa kirjautumisen käytäntöjen (esimerkiksi "B2C_1_SiUp"). Valitse sivu yläreunassa **Muokkaa** .
4. **Kirjautuminen määritteet** ja valitse mukautettu määrite (esimerkiksi "ShoeSize"). Valitse **OK**.
5. **Sovelluksen vaateet** ja valitse mukautettu määrite. Valitse **OK**.
6. Valitse **Tallenna** sivu yläreunassa.

Voit käyttää "Suorita"-ominaisuutta käytännön vahvistamiseksi kuluttaja-toiminto. Pitäisi nyt artikkelissa "ShoeSize" kuluttaja kirjautumisen aikana määritteet-luettelossa ja se näkyy lähetetty takaisin sovelluksen tunnus.

## <a name="notes"></a>Huomautuksia

- Kirjautuminen käytäntöjä sekä mukautetut määritteet voidaan myös kirjautuminen tai Kirjaudu sisään ja profiilin käytäntöjen muokkaaminen.
- Mukautetut määritteet tunnetut rajoitus on. Se luodaan vain ensimmäistä kertaa, sitä käytetään kaikki käytännöt ja ei, kun lisäät sen **käyttäjän määritteiden**luetteloon.
