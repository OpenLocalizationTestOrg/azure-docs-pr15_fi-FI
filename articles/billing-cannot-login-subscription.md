<properties
    pageTitle="Azure-tilaukseen kirjautuminen ei onnistu | Microsoft Azure"
    description="Tässä artikkelissa käsitellään joitakin yleisten Azure tilauksen kirjautumisongelmien vianmääritys."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Kirjautuminen ei onnistu Azure tilaukseni hallintaan

Tässä artikkelissa ohjaa läpi joitakin yleisimmät menetelmistä kirjautuminen ongelmien ratkaisemiseen.

## <a name="page-hangs-in-the-loading-status"></a>Sivun jumittuu lataaminen-tila

Jos internet-selaimen sivu jumiutuu, kokeile kunkin seuraavat toimet ennen kuin voit siirtyä [Azure portal](https://portal.azure.com).

-   Päivitä sivu.
-   Eri Internet-selaimen avulla
-   Jos käytät Internet Exploreria, siirry Azure portaalin käyttämällä InPrivate-selaus-tilaa. 

    A.  Valitse **Työkalut** ![työkalupainike](./media/billing-cannot-login-subscription/Toolsbutton.png) > **turvallisuutta** > **InPrivate-selaus**.

    B.  Siirry [Azure portal](https://portal.azure.com)ja kirjaudu sitten sisään-portaaliin.

## <a name="error-message-no-subscriptions-found"></a>Virhesanoma "Tilauksia ei löydy"

Jos tiliä ei ole riittäviä käyttöoikeuksia, näkyviin saattaa **tilausta ei löytynyt** virheilmoitus. Vain tili-järjestelmänvalvoja voi käyttää [Tilin Center](https://account.windowsazure.com/)etkä palvelun järjestelmänvalvojat (SA) tai apuyhteyshenkilöiden (CA).

**Tapaus 1: Virhesanoma vastaanotetaan [Azure portal](https://portal.azure.com)**

Voit ratkaista ongelman tilin [Lisää työtovereiden järjestelmänvalvojan tai omistajan rooli](billing-add-change-azure-subscription-administrator.md) .

**Tapaus 2: Virhesanoma vastaanotetaan [Azure tilin Center](https://account.windowsazure.com/Subscriptions)**

Tarkista, onko tili, jota käytit tilin järjestelmänvalvojaan. Voit tarkistaa, kuka tilin järjestelmänvalvoja on, toimi seuraavasti:

1.  Kirjautuminen [Azure portal](https://portal.azure.com).
2.  Valitse toiminto-valikosta **tilauksen**.
3.  Valitse tilaus, jonka haluat tarkistaa, ja valitse sitten **asetukset**.
4.  Valitse **Ominaisuudet**. **Tilin hallinta** -ruudussa näkyy tilauksen tilin järjestelmänvalvojaan.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Voit automaattisesti kirjautunut sisään käyttäjänä, eri

Tämä ongelma voi ilmetä, jos käytössäsi on useita käyttäjätilin Internet-selaimessa.

Voit ratkaista ongelman, kokeile jotakin seuraavista tavoista:

-   Tyhjennä välimuisti ja poista Internet-evästeet. Valitse Internet Explorerissa **Työkalut** ![työkalupainike](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internet-asetukset** > **poistaminen**. Varmista, että väliaikaiset tiedostot, evästeet, salasana ja selaushistorian valintaruudut ovat valittuina, ja valitse sitten Poista.

-   Palauta Internet Explorer-asetukset, jos haluat palata Omat asetukset, jotka olet tehnyt. Valitse **Työkalut** ![Työkalut-painike](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internet-asetukset** > **Lisäasetukset** > **Poista Omat asetukset** -ruutu > **Palauta**.

-   Siirry Azure portaalin InPrivate-selaus-tilassa. Valitse **Työkalut** ![työkalupainike](./media/billing-cannot-login-subscription/Toolsbutton.png) > **turvallisuutta** > **InPrivate-selaus**.

## <a name="need-help-contact-support"></a>Tarvitsetko apua? Ota yhteyttä tukeen. 

Jos tarvitset apua, [Ota yhteyttä tukeen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) pääset nopeasti selvittää ongelmaa. 