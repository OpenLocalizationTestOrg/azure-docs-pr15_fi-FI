<properties
    pageTitle="Azure Active Directory raportointi-ilmoitukset"
    description="Käyttämisestä raportoinnin ilmoituksia epäilyttävistä kirjauduttaessa Azure Active Directory apuohjelmat."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Azure Active Directory raportointi-ilmoitukset

## <a name="what-reports-generate-email-notifications"></a>Mitä raportit-Luo sähköposti-ilmoitukset

Tällä hetkellä vain epäsäännöllinen kirjautuminen tehtävän raportin käynnistimien sähköpostin ilmoitukset.

## <a name="what-is-an-irregular-sign-in"></a>Mikä "epäsäännöllinen Kirjaudu sisään"?

Epäsäännöllinen Kirjaudu apuohjelmien ovat niitä, jotka on merkitty Microsoftin konepohjaisten oppimistekniikoiden algoritmit yhdistää erheellisiin-kirjautuminen sijainti ja laitteen "mahdotonta matka"-ehdon perusteella. Tämä voi johtua luvaton käyttäjä yrittää kirjautua tällä tilillä.

## <a name="who-receives-the-email-notifications"></a>Kuka saa sähköposti-ilmoitukset?

Sähköposti lähetetään kaikille Yleiset Järjestelmänvalvojat, joille on määritetty Active Directory-Premium-käyttöoikeus. Jotta viesti on toimitettu lähetämme se järjestelmänvalvojille vaihtoehtoinen sähköpostiosoite sekä. Järjestelmänvalvojien on sisällettävä aad-alerts-noreply@mail.windowsazure.com -hänen turvallisten lähettäjien luetteloon, jotta ne eivät jää huomaamatta sähköposti.

## <a name="how-often-are-these-emails-sent"></a>Kuinka usein ovat nämä lähetetyt sähköpostit?

Sähköposti lähetetään, jos 10 uusia epäsäännöllinen kirjautumisen aktiviteetteja esiintyvät viimeisten 30 päivän aikana tai viimeisen sähköpostin on lähetetty, koska sen mukaan, kumpi on pienempi.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Miten voin käyttää sähköpostien mainitun raportin?

Kun olet napsauttanut linkkiä, sinut ohjataan Azure perinteinen portaalin raportti-sivu. Jotta voit käyttää raportin, sinun on oltava sekä:

- Järjestelmänvalvojan tai Azure tilauksen Mää-järjestelmänvalvoja

- Yleinen järjestelmänvalvoja kansioon, ja määrittänyt Active Directory-Premium-käyttöoikeudet. Lisätietoja on artikkelissa [Azure Active Directory-versiot](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Voinko poistaa käytöstä nämä sähköpostit?

Kyllä, erheellisiin merkki-laajennukset Azure perinteinen portaalin liittyvät ilmoitukset käytöstä, valitse **Määritä**ja valitse sitten **ilmoitukset** -kohdassa **ei käytössä** .

## <a name="whats-next"></a>Mitä seuraavaksi?
- Mitä tietoja haluat tarkastella, Valvo ja käyttöraporttien on käytettävissä? Tutustu [Azure AD-suojaus ja valvonta-käyttöraportit](active-directory-view-access-usage-reports.md)
- [Azure Active Directory-Premium käytön aloittaminen](active-directory-get-started-premium.md)
- [Lisää yrityksen mukauttaminen Kirjaudu sisään ja Access-paneelin sivut](active-directory-add-company-branding.md)
