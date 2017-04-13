<properties
    pageTitle="Salasanojen Azure Active Directoryn hallinta | Microsoft Azure"
    description="Tietoa salasanojen Azure Active Directoryn hallinta."
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
    ms.date="08/23/2016"
    ms.author="curtand"/>

# <a name="manage-passwords-in-azure-active-directory"></a>Azure Active Directoryn salasanojen hallinta

Jos olet järjestelmänvalvoja, voit palauttaa käyttäjän salasanan Azure Active Directory (Azure AD) Azure perinteinen-portaalissa. Napsauta nimen hakemistossa ja käyttäjät-sivulla, valitse käyttäjän nimi ja valitse portaalin alareunassa **Vaihda salasana**.

Tämän aiheen loppuosassa käsitellään täydellisen luettelon salasanan hallintatoiminnot, joka tukee Azure AD, mukaan lukien:

- **Omatoiminen salasanan** vaihtaminen avulla loppukäyttäjät ja järjestelmänvalvojien muuttamaan vanhentunut tai ei ole vanhentunut salasanansa ilman kutsumista järjestelmänvalvojan tai tukipalvelusta tuki.
- **Omatoiminen** salasanan avulla loppukäyttäjät ja järjestelmänvalvojien vaihtamaan salasanansa automaattisesti ilman kutsumista järjestelmänvalvojan tai tukipalvelusta tuki. Omatoiminen salasanan vaatii Azure AD Premium tai Basic. Lisätietoja on artikkelissa [Azure Active Directory-versiot](active-directory-editions.md).
- **Järjestelmänvalvojan käynnistämä salasanan** avulla käyttäjän tai toisen järjestelmänvalvojan salasanan from Azure perinteinen portaalin järjestelmänvalvoja.
- **Salasanan hallinta käyttöraporttien** antaa järjestelmänvalvojien tiedot kohteeseen salasanan palauttaminen ja rekisteröintiä activity organisaation määrä.
- **Salasanan takaisinkirjoituksen** sallii paikallisen salasanat pilvestä hallinnointi niin kaikkia edellä skenaarioita voidaan suorittaa mukaan tai puolesta, liitetty tai salasana synkronoidut käyttäjät. Salasanan takaisinkirjoituksen edellyttää Azure AD Premium. Lisätietoja on artikkelissa [Azure Active Directory-Premium käytön aloittaminen](active-directory-get-started-premium.md).

> [AZURE.NOTE]
> Azure AD-Premium on käytettävissä Kiinan asiakkaat, jotka käyttävät Azure AD maailmaa esiintymä. Azure AD-Premium ei tällä hetkellä tueta Kiinassa 21vianetin ylläpitämä Microsoft Azure-palvelu. Saat lisätietoja yhteystiedot [Azure Active Directory-keskustelupalstalla](https://feedback.azure.com/forums/169401-azure-active-directory/)osoitteessa.

Seuraavissa linkeissä avulla voit siirtyä kiinnostavan eniten ohjeissa:

- [Yleistä: salasanojen hallinta Azure AD-](active-directory-passwords-how-it-works.md)
- [Omatoiminen salasanan Azure AD: ottaminen käyttöön, Määritä ja testaa Omatoiminen salasanan palauttaminen](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords)
- [Omatoiminen salasanan Azure AD: mukauttamisesta salasanan palauttaminen vastaamaan omia tarpeita](active-directory-passwords-customize.md)
- [Omatoiminen salasanan Azure AD: verkkosivuston käyttöönotto- ja parhaat käytännöt](active-directory-passwords-best-practices.md)
- [Salasanojen hallinta raportit Azure AD: vuokraajan salasanan toiminto tarkasteleminen](active-directory-passwords-get-insights.md)
- [Salasanan takaisinkirjoituksen: Azure AD hallittavan määrittämisestä paikallisen salasanat](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
- [Vianmääritys Azure AD salasanojen hallinta](active-directory-passwords-troubleshoot.md)
- [Usein kysytyt kysymykset Azure AD salasanojen hallinta](active-directory-passwords-faq.md)


## <a name="whats-next"></a>Mitä seuraavaksi?

- [Azure AD hallinta](active-directory-administer.md)
- [Luo tai Muokkaa käyttäjiä Azure AD](active-directory-create-users.md)
- [Azure AD-ryhmien hallinta](active-directory-manage-groups.md)
