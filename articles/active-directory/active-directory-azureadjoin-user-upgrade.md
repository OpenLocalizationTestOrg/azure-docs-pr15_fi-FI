<properties
    pageTitle="Määritä asetukset Azure AD kanssa Windows 10-laitteeseen | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten käyttäjät voivat liittyä Azure AD asetukset-valikon kautta."
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

# <a name="set-up-a-windows-10-device-with-azure-ad-from-settings"></a>Windows 10-laitteen kanssa Azure AD asetusten määrittäminen
Jos käytät jo Windows 7 tai Windows 8: ssa ja tietokoneeseen tai laitteeseen on päivitetty Windows 10: ssä, voit liittyä Azure Active Directory (Azure AD) asetukset-valikon kautta.

## <a name="to-join-to-azure-ad-from-the-settings-menu"></a>Liittäminen Azure AD-asetukset-valikko


1. Napsauta **Käynnistä** -valikon **asetukset** -painikkeen.
2. Valitse **asetukset**-kohdasta **järjestelmän**->**tietoja**->**liittyä Azure AD**.
<center>
![Liittyminen Azure AD-asetukset-valikosta](./media/active-directory-azureadjoin/active-directory-azureadjoin-settings.png)</center>

3. Valitse **Jatka** Azure AD liittyä viesti-ikkunassa.
<center>
![Liity Azure AD-viesti-ikkunan](./media/active-directory-azureadjoin/active-directory-azureadjoin-message.png)</center>
4. Anna kirjautumisen tunnistetiedot. Tämä kirjautuminen sisältää kaikki vaiheet, joita tarvitaan valmis todentamiseen. Jos olet ovat liitetty Alihallinta, järjestelmänvalvojasi antaa sinulle federaatio, jotka organisaatiosi nykyisessä käyttökokemuksen.
<center>
![Anna kirjautumisen tunnistetiedot](./media/active-directory-azureadjoin/active-directory-azureadjoin-sign-in.png)</center>
5. Jos organisaatio on määritetty Azure Monimenetelmäisen todentamisen liittymisen Azure AD-antaa toinen tekijä, ennen kuin jatkat.
6. **Salli tämän laitteen tuleeko** näyttöön valitsemalla **Hyväksy** .
7. Pitäisi tulla seuraava sanoma "laitteen on nyt yhdistetty organisaation Azure AD".


## <a name="additional-information"></a>Lisätietoja
* [Lisätietoja käyttötapoja Azure AD liittyminen](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Toimialueen liittyneet laitteiden yhdistäminen Azure AD for Windows 10: ssä kokemukset](active-directory-azureadjoin-devices-group-policy.md)
* [Määritä Azure AD liittyminen](active-directory-azureadjoin-setup.md)
* [Käyttäjätietojen ilman salasanan vaihtamiseen Microsoft Passport todennustapa](active-directory-azureadjoin-passport.md)
