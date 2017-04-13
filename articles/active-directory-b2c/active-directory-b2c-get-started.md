<properties
    pageTitle="Azure Active Directory-B2C: Luo Azure Active Directory-B2C vuokraajan | Microsoft Azure"
    description="Aiheen Azure Active Directory-B2C vuokraajan luomisesta"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory-B2C: Luo Azure AD B2C vuokraajan

Voit käyttää Microsoft Azure Active Directory (Azure AD) B2C noudattamalla tässä artikkelissa kuvattujen kolme vaihetta.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Vaihe 1: Rekisteröityminen Azure-tilauksen

Jos sinulla on jo Azure tilauksen, Ohita tämä vaihe. Jos et, [Azure tilauksen](../active-directory/sign-up-organization.md) rekisteröityä ja Azure AD B2C pääsy.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Vaihe 2: Luo Azure AD B2C vuokraajan

Seuraavien vaiheiden avulla voit luoda uuden Azure AD B2C vuokraajan. Tällä hetkellä B2C ominaisuuksia ei voi ottaa käyttöön aiemmin alihallinnat.

1. Kirjaudu sisään [Azure perinteinen portal](https://manage.windowsazure.com/) tilauksen järjestelmänvalvojana. Tämä on sama työpaikan tai oppilaitoksen tilille tai samalle Microsoft-tilille, jota käytit Azure rekisteröityminen.
2. Valitse **Uusi** > **sovelluksen Services** > **Active Directory** > **Directory** > **Mukautettu luominen**.

    ![Näyttökuva Luo palvelutili käynnistäminen](./media/active-directory-b2c-get-started/new-directory.png)

3. Valitse **nimi**, **Toimialuenimi** ja **maa tai alue** -vuokraajan.
4. Valitse vaihtoehto, jonka mukaan **Tämä on B2C-kansio**.
5. Napsauta valintamerkkiä, jos haluat suorittaa toiminnon.

    ![Näyttökuva valintamerkki B2C-kansion luominen](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Alihallintaan luodaan, ja ne näkyvät Active Directory-tunniste. Yleinen järjestelmänvalvoja vuokraajan myös tehdään. Voit lisätä muita Yleiset järjestelmänvalvojat halutulla tavalla.

    > [AZURE.IMPORTANT]
    Jos aiot käyttää B2C palvelutili tuotannon-sovelluksen, tutustu artikkeliin [tuotannon -](active-directory-b2c-reference-tenant-type.md)asteikon esikatselu B2C omistajien ja. Huomaa, että aiheuttavat tunnetusti ongelmia, kun poistat aiemmin B2C-vuokraajan ja luo se uudelleen sama toimialuenimi. Sinun on luotava B2C palvelutili eri toimialuenimeä.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Vaihe 3: Siirry Azure-portaalissa B2C ominaisuudet-sivu

1. Siirry vasemman reunan siirtymispalkissa Active Directory-tunniste.
2. Etsi alihallintaan kohdassa **Hakemisto** -välilehti ja valitse se.
3. Valitse **määritys** -välilehti.
4. Valitse **B2C Sivustonhallinta** -osassa **Hallitse B2C asetukset** -linkki.

    ![Näyttökuva B2C hakemisto-määritys](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Azure portaaliin B2C ominaisuudet-sivu, jossa näkyy avautuu uusi selaimen välilehti tai ikkuna.

    > [AZURE.IMPORTANT]
    Voi kestää jopa 2 – 3 minuuttia vuokraajan käytettävissä Azure-portaalissa. Yritetään seuraavasti, kun jonkin aikaa korjata ongelman. Jos et, ota yhteyttä tukeen.

6. Kiinnitä tämä sivu, että Startboard käytön helpottamiseksi. (Pin-työkalu on merkitty punaisella oikeassa yläkulmassa toiminnot-sivu.)

    ![Näyttökuva B2C ominaisuudet-sivu](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Voit hallita käyttäjät ja ryhmät, käyttäjien Omatoiminen salasanan palauttaminen määritys ja yrityksen alihallintaan [Azure perinteinen portal](https://manage.windowsazure.com/)yrityksen tunnuksen ominaisuuksia.

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Helpon pääsyn Azure-portaalissa B2C ominaisuudet-sivu

Voit löytymistä, Olemme lisänneet pikakuvakkeen, Azure-portaalissa B2C ominaisuudet-sivu.

1. Kirjautua Azure-portaaliin B2C vuokraajan yleisenä järjestelmänvalvojana. Jos olet jo kirjautunut sisään eri palvelutili, Vaihda alihallinnat (ikkunan oikeassa alakulmassa).
2. Valitse vasemmalla olevasta siirtymisrakenteen valitsemalla **Selaa** .
3. Valitse **Azure AD B2C** käyttämään B2C ominaisuudet-sivu.

    ![Näyttökuva, Selaa B2C ominaisuudet-sivu](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Opi, voit rekisteröidä sovelluksen Azure AD B2C ja Pika-aloitus-sovelluksen lukemalla [Azure Active Directory-B2C: Rekisteröi sovelluksen](active-directory-b2c-app-registration.md).
