<properties
pageTitle="Mukauttaa Azure Active Directory (esiversio)-kirjautuminen sivun | Microsoft Azure"
description="Opettele lisäämään yrityksen Azure kirjautumissivulla mukauttaminen"
services="active-directory"
documentationCenter=""
authors="curtand"
manager="femila"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/30/2016"
ms.author="curtand"/>

# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Lisää yrityksen Azure Active Directory (esiversio)-kirjautuminen sivulle mukauttaminen

Voit Sekaannusten välttämiseksi useat yritykset haluavat Käytä yhdenmukaisen ulkoasun sivustoja ja palveluja, ne hallinta. Azure Active Directory-esikatselu on tämän ominaisuuden avulla voit muokata yrityksen logon ja mukautetut Värimallit kirjautumissivulla ulkoasua. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Kirjaudu sisään-sivu on, joka tulee näkyviin, kun kirjaudut sisään Office 365: ssä tai muita verkkopohjaisia-sovellukset, jotka käyttävät Azure AD kuin tunnistetietojen toimittaja. Voit käsitellä sivun Anna tunnistetiedot.

Jos haluat näyttää tämän sivun yrityksen brändiä, värit ja muita mukautettavia osia, lue seuraavat kuvat kahden version eroista.

Seuraava kuva näyttää ja Esimerkki Office 365-kirjautuminen sivun pöytätietokoneen **ennen** mukauttaminen:

![Office 365: n kirjautumissivulla ennen mukauttaminen](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

Seuraava kuva näyttää ja Esimerkki Office 365-kirjautuminen sivun pöytätietokoneen **jälkeen** mukauttaminen:

![Office 365: n kirjautumissivulla mukauttamisen jälkeen](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)


## <a name="customizing-the-sign-in-page"></a>Kirjaudu sisään-sivun muokkaaminen

Jos tarvitset selainpohjaisia access cloud sovellukset ja palvelut, joita organisaatiosi tilaa, voit käyttää yleensä kirjautumissivulla.

Jos olet käyttänyt muutokset sisään sivulle, voi kestää tunneiksi, jotta muutokset tulevat näkyviin.

Yrityskuvaa tukevan kirjautumissivulla näkyy vain, kun käyt palvelun, kuten https://outlook.com/**contoso**.com tai https://mail vuokraajan kielikohtaiset URL-osoite. **Contoso**. com.

Kun käyt palvelu, jolla ei ole vuokraajan tietyn URL-osoitteet (esimerkiksi: https://mail.office365.com), muiden kirjautumissivu tulee näkyviin. Tässä tapauksessa oman yrityksesi tunnuksen näkyy, kun olet kirjoittanut Käyttäjätunnus tai käyttäjän-ruutu on valittuna.

> [AZURE.NOTE]
>
- Toimialuenimi tulee näkyviin "Aktiivinen" **Toimialueet** -osassa Azure portaalin, jonka olet määrittänyt yrityksen tunnuksen. Lisätietoja on artikkelissa [Lisää mukautettuja toimialuenimiä](active-directory-domains-add-azure-portal.md).
- Kirjautumissivu mukauttaminen ei siirtää kuluttaja-Kirjaudu sisään Microsoft-sivulla. Jos olet kirjautunut sisään Microsoft-tiliä, voit nähdä yrityskuvaa tukevan käyttäjän ruudut hahmontaa Azure AD-luettelo, mutta organisaation mukauttaminen ei koske Microsoft-kirjautuminen tilisivun.

Valitse Kirjaudu sisään-sivulla **Pysy kirjautuneena** -valintaruutu valittuna käyttäjä säilyy kirjautuneena, kun ne suljettava ja avattava uudelleen selaimeen. 

   ![Pysy kirjautuneena sisään](./media/active-directory-branding-custom-signon-azure-portal/01.png)

Se ei vaikuta istunnon aikana. Voit piilottaa Azure Active Directory-kirjautumissivulla-valintaruudun valinta.
Onko valintaruutu näkyy määräytyy asetus **Pysy kirjautuneena on poistettu käytöstä**.

   ![Pysy kirjautuneena sisään](./media/active-directory-branding-custom-signon-azure-portal/02.png)


Jos haluat piilottaa valintaruudun valinta, tämä asetus **Kyllä**. 

> [AZURE.NOTE] SharePoint Onlinen ja Office 2010: n ominaisuuksia määräytyvät sen mukaan, mitä käyttäjät voivat tämän valintaruudun. Jos määrität tämän asetuksen avulla voit piilottaa, käyttäjien voi tulla ylimääräisiä ja odottamattomia pyytää kirjautumaan sisään.




**Voit lisätä yrityksen hakemistossa ulkoasua seuraavasti:**

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita tekstiruutuun **käyttäjät ja ryhmät** ja paina sitten **ENTER-näppäintä**.

    ![Avaava käyttäjien hallinta](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)

3. Valitse **käyttäjät ja ryhmät** -sivu **yrityksen mukauttaminen**.

4. Valitse **käyttäjät ja ryhmät - yrityksen mukauttaminen** sivu, valitse **Muokkaa** -komentoa.

    ![Muokkaa: Mukautettu ulkoasu](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)

5. Muokkaa elementtejä, jotka haluat mukauttaa. Kaikki osat ovat valinnaisia.

6. Valitse **Tallenna**.

Voi kestää tunneiksi, kirjaudu sisään-sivulla näkyvän mukauttaminen tekemäsi muutokset.

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisää kielikohtaiset yrityksen mukauttaminen](active-directory-branding-localize-azure-portal.md)
