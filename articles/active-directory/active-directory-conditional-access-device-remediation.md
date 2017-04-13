<properties
    pageTitle="Azure Active Directory access-ongelmien vianmääritystä | Microsoft Azure"
    description="Lue ohjeet, joita voit käyttää organisaation verkkoresursseja liittyvien ongelmien ratkaiseminen."
    services="active-directory"
    keywords="laitteen perustuva ehdollinen access-laitteen rekisteröinti käyttöön laitteen rekisteröinti, laitteen rekisteröinti ja Mobiililaitteiden"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/23/2016"
    ms.author="markvi"/>


# <a name="troubleshooting-for-azure-active-directory-access-issues"></a>Azure Active Directory access-ongelmien vianmääritys

Kun yrität käyttää organisaatiosi SharePoint Onlinen intranet ja näkyviin tulee "käyttö estetty"-virhesanoma. Mitä teet työksesi?

Tässä artikkelissa käsitellään korjaus vaiheet, joiden avulla voit käyttää organisaation verkkoresursseja liittyvien ongelmien ratkaiseminen.

Ohjeita virheiden Azure Active Directory (Azure AD) käyttöön liittyviä kysymyksiä, siirry artikkeliin, jossa käsitellään laitteen omaa ympäristöäsi kohtaan:

-   Windows-laitteessa
-   iOS-laitteessa (Tarkista takaisin pian apua Iphoneja ja iPad-sovellukset.)
-   Android-laitteessa (käy tarkistamassa tilanne pian Android apua puhelimiin ja taulutietokoneisiin.)

## <a name="access-from-a-windows-device"></a>Windows-laitteen käyttö

Jos laite toimii jompikumpi seuraavista ympäristöjen, Etsi seuraava virhesanoma, jossa näkyy, kun yrität käyttää sovelluksen tai palvelun osat:

- Windows 10: ssä
- Windows 8.1
- Windows 8: ssa
- Windows 7: ssä
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

### <a name="device-is-not-registered"></a>Laite ei ole rekisteröity

Jos sovellus on suojattu laitteen käytäntöä laitteesi ei ole rekisteröity Azure AD, näyttöön voi ilmestyä sivu, jolla näkyy jokin virheilmoituksia:

!["Sinun ei voi siirtyä sinne tästä" viestit rekisteröinti laitteille] (./media/active-directory-conditional-access-device-remediation/01.png "Skenaario")

Jos laite on toimialueeseen liittyneet muodostaminen Active Directoryyn organisaatiossa, kokeile seuraavaa:

1.  Varmista, että kirjaudut Windows käyttämällä työpaikan (vain Active Directory-tili).
2.  Yhteyden muodostaminen näennäisen yksityisverkon (VPN) tai DirectAccess yrityksen verkkoon.
3.  Kun yhteys on muodostettu, paina Windows-näppäin + L-näppäimen lukitseminen Windows-istunto.
4.  Kirjoita Windows-istunnon lukituksen työ-tilin tunnukset.
5.  Odota hetki ja yritä sitten uudelleen käyttämään sovelluksen tai palvelun.
6.  Jos näet samalle sivulle, **Lisätietoja** -linkkiä ja ota yhteyttä järjestelmänvalvojaan tiedot.

Jos laite ei ole toimialueen liittyneet ja suorittaa Windows 10: ssä, sinulla on kaksi vaihtoehtoa:

- Suorita Azure AD-liitos
- Työpaikan tai oppilaitoksen tilin lisääminen Windows

Saat tietoja siitä, kuinka nämä asetukset ovat eri, [käyttämällä Windows 10: ssä, mikä työpaikalle](active-directory-azureadjoin-windows10-devices.md).

Suorita Azure AD liittyminen, tee seuraavat toimet-ympäristön laitteen suoritetaan. (Azure AD-liitos ei ole käytettävissä Windows-puhelimet.)

**Windows 10 vuosipäivä päivitys**

1.  Avaa **Settings** -sovellukseen.
2.  Valitse **tilit** > **Access työpaikan tai oppilaitoksen**.
3.  Valitse **Muodosta yhteys**.
4.  Valitse **Liity Azure AD laitteen**.
5.  Organisaation todennusta, anna monimenetelmäisen todentamisen ja noudata ohjeita, jotka ovat näkyvissä.
6.  Kirjaudu ulos ja kirjaudu sitten sisään tililtä.
7.  Jos haluat käyttää sovellusta, yritä uudelleen.


**Windows 10 marraskuun 2015 päivitys**

1.  Avaa **Settings** -sovellukseen.
2.  Valitse **Järjestelmä** > **tietoja**.
3.  Valitse **Liity Azure AD**.
4.  Organisaation todennusta, anna monimenetelmäisen todentamisen ja noudata ohjeita, jotka ovat näkyvissä.
5.  Kirjaudu ulos ja kirjaudu sisään työpaikan (vain Azure AD-tili).
6.  Jos haluat käyttää sovellusta, yritä uudelleen.

Jos haluat lisätä käyttämällä työpaikan tai oppilaitoksen tiliä, toimi seuraavasti:

**Windows 10 vuosipäivä päivitys**

1.  Avaa **Settings** -sovellukseen.
2.  Valitse **tilit** > **Access työpaikan tai oppilaitoksen**.
3.  Valitse **Muodosta yhteys**.
4.  Organisaation todennusta, anna monimenetelmäisen todentamisen ja noudata ohjeita, jotka ovat näkyvissä.
5.  Jos haluat käyttää sovellusta, yritä uudelleen.


**Windows 10 marraskuun 2015 päivitys**

1.  Avaa **Settings** -sovellukseen.
2.  Valitse **tilit** > **tilit**.
3.  Valitse **työpaikan tai oppilaitoksen tiliä**.
4.  Organisaation todennusta, anna monimenetelmäisen todentamisen ja noudata ohjeita, jotka ovat näkyvissä.
5.  Jos haluat käyttää sovellusta, yritä uudelleen.

Jos laite ei ole toimialueen liittyneet ja suorittaa Windows 8.1, työpaikkasi liittyä ja rekisteröi Microsoft intuneen, toimi seuraavasti:

1.  Avaa **tietokoneen asetukset**.
2.  Valitse **Verkko** > **työpaikkasi**.
3.  Valitse **Liity**.
4.  Organisaation todennusta, anna monimenetelmäisen todentamisen ja noudata ohjeita, jotka ovat näkyvissä.
5.  Valitse **Ota käyttöön**.
6.  Jos haluat käyttää sovellusta, yritä uudelleen.


### <a name="browser-is-not-supported"></a>Selainta ei tueta

Olet ehkä voi muodostaa yhteyttä Jos yrität käyttää sovelluksen tai palvelun käyttämällä jotakin seuraavista selaimista:

- Chrome, Firefox tai muut selaimet kuin Microsoft Edge- tai Microsoft Internet Explorerin Windows 10: ssä tai Windows Server 2016
- Firefoxin Windows 8.1, Windows 7: ssä, Windows Server 2012 R2, Windows Server 2012: n tai Windows Server 2008 R2

Näyttöön tulee virhesivu, joka näyttää tältä:

![Viesti "Et saa on täällä" ei-tuetut selaimet] (./media/active-directory-conditional-access-device-remediation/02.png "Skenaario")

Vain korjaus on käyttää sovelluksen tukeva laite omaa ympäristöäsi selain.

## <a name="next-steps"></a>Seuraavat vaiheet

[Azure Active Directory ehdollisen käyttöoikeuden](active-directory-conditional-access.md)
