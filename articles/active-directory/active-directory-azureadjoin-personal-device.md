

<properties
    pageTitle="Omat laitteen liittäminen organisaation | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten käyttäjät voivat rekisteröityä yrityksen verkkoon niiden Omat Windows 10-laitteissa sekä kerrotaan käyttöönoton BYOD tilanne."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>
<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="join-a-personal-device-to-your-organization"></a>Omat laitteen liittäminen organisaation

## <a name="to-join-a-windows-10-device-to-your-organization"></a>Liittäminen organisaation Windows 10-laitteeseen

1.  Valitse **Käynnistä** -valikosta **asetukset**.
2.  Valitse **tilit**ja valitse sitten **tilin**.
3.  Valitse **Lisää työpaikan tai oppilaitoksen tilille**ja kirjoita organisaation tiliä.
4.  Organisaation Kirjaudu sisään-sivulla käyttäjänimi ja salasana ja valitse sitten **OK**.
5.  Voit pyydetään monimenetelmäisen todentamisen hankala. (Tämä todennus on määritettävä IT-järjestelmänvalvoja).
6.  Azure Active Directory (Azure AD) tarkistaa, onko laitteen edellyttää mobiililaitteiden hallinnan rekisteröinti.
7.  Windows Rekisteröi laite organisaation hakemiston Azure AD- ja liittää sen mobiililaitteiden hallinta tarvittaessa.
8.  Jos olet hallitut käyttäjien, Windows kerrotaan työpöydän automaattisen Kirjaudu sisään.
9.  Jos olet liitetyt käyttäjät, siirryt Windows-kirjautuminen näyttöön tunnistetietosi.

## <a name="additional-information"></a>Lisätietoja
* [Windows 10: n yrityksen: tapoja käyttää laitteet](active-directory-azureadjoin-windows10-devices-overview.md)
* [Cloud ominaisuuksia Windows 10-laitteisiin kautta Azure Active Directory-liittyä laajentaminen](active-directory-azureadjoin-user-upgrade.md)
* [Käyttäjätietojen ilman salasanan vaihtamiseen Microsoft Passport todennustapa](active-directory-azureadjoin-passport.md)
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
