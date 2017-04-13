<properties
pageTitle="Lisää kielikohtaiset yrityksen mukauttaminen Azure Active Directory (esiversio)-kirjautuminen sivulle | Microsoft Azure"
description="Opettele lisäämään kielen tietyn yrityksen kuvia ja tekstiä Azure kirjautumissivulla mukauttaminen"
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
ms.date="09/12/2016"
ms.author="curtand"/>

# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory-preview"></a>Lisää kielikohtaiset yrityksen Azure Active Directory (esiversio)-kirjautuminen sivulle mukauttaminen

Voit Sekaannusten välttämiseksi useat yritykset haluavat Käytä yhdenmukaisen ulkoasun sivustoja ja palveluja, ne hallinta. Azure Active Directory-esikatselu on tämän ominaisuuden avulla voit muokata yrityksen logon ja mukautetut Värimallit kirjautumissivulla ulkoasua. [Mikä on esikatselussa?](active-directory-preview-explainer.md) Kirjaudu sisään-sivu on, joka tulee näkyviin, kun kirjaudut sisään Office 365: ssä tai muita verkkopohjaisia-sovellukset, jotka käyttävät Azure AD kuin tunnistetietojen toimittaja. Voit käsitellä sivun Anna tunnistetiedot.

## <a name="customizing-the-sign-in-page-for-another-language"></a>Toisen kielen kirjautumissivulla mukauttaminen

Voit lisätä kielikohtaiset osat mukautettu-kirjautuminen sivulle vain, jos olet jo luonut mukautettuja sallittu kirjautumissivulla sellaisena kuvatun [Lisää mukauttaminen - kirjautuminen sivulle yrityksen](active-directory-branding-custom-signon-azure-portal.md). Voit määrittää yhden kirjautumissivu kohti directory mukautettavissa osien oletusarvot. Kun olet määrittänyt sivuelementtien oletusarvoisesti, voit määrittää useita eri kieliversiot versiot. Voit myös sekoitetaan ja vastaa eri osiin. Voit esimerkiksi seuraavaa:

- Luo oletusarvoisesti **kirjautumisen sivun kuva** , joka toimii kaikki kulttuurien ja valitse sitten Luo tietyn versiot englanniksi ja ranskaksi. Kun määrität selainikkunat johonkin näistä kahdesta kielistä, kielikohtaiset kuvan paikkaa, oletusarvo-kuvassa näkyy kaikilla kielillä.

- Määritä eri logot organisaation (esimerkiksi japanin- tai hepreankielistä versiot).

On suositeltavaa, että pidät kielen variaatiot määrän pieni, ylläpito ja suorituskyky syistä.

**Voit lisätä yrityksen hakemistossa ulkoasua seuraavasti:**

1.  Kirjautuminen [Azure portaaliin](https://portal.azure.com) tilille, jolla on yleisen järjestelmänvalvojan kansion.

2.  Valitse **Lisää palveluja**, kirjoita tekstiruutuun **käyttäjät ja ryhmät** ja paina sitten **ENTER-näppäintä**.

    ![Avaava käyttäjien hallinta](./media/active-directory-branding-localize-azure-portal/user-management.png)

3. Valitse **käyttäjät ja ryhmät** -sivu **yrityksen mukauttaminen**.

4. Valitse **käyttäjät ja ryhmät - yrityksen mukauttaminen** sivu, valitse **Lisää kieli** -komentoa.

    ![Kielikohtaiset yrityksen tunnuksen elementtien lisääminen](./media/active-directory-branding-localize-azure-portal/add-language.png)

5. Muokkaa elementtejä, jotka haluat mukauttaa. Kaikki osat ovat valinnaisia.

6. Valitse **Tallenna**.

Voi kestää tunneiksi, kirjaudu sisään-sivulla näkyvän mukauttaminen tekemäsi muutokset.

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisää yritys-kirjautuminen sivulle mukauttaminen](active-directory-branding-custom-signon-azure-portal.md)
