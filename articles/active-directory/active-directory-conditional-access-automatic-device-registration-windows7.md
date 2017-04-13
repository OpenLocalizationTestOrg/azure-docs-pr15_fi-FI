<properties
    pageTitle="# Määritä automaattinen laitteen rekisteröinti Windows 7: n toimialueen liittyneet laitteille | Microsoft Azure"
    description="Windows 7: n toimialueen määrittämistä vaiheet liittyneet automaattisesti rekisteröidä Azure AD-laitteet. ja ohjeita, jos laitteen rekisteröinti-ohjelmistopaketin käyttöön Windows 7: n toimialueen liittyneet laitteista ohjelmiston jakauman järjestelmän kuten System Center määritysten hallinta."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="MarkVi"/>

# <a name="configure-automatic-device-registration-for-windows-7-domain-joined-devices"></a>Automaattinen laitteen rekisteröinti Windows 7: n toimialueen liittyneet laitteiden määrittäminen

IT-järjestelmänvalvoja voi määrittää automaattisesti rekisteröitymään Azure AD Windows 7: n liitetty laitteet. Voit tehdä niin laitteen rekisteröinti-ohjelmistopaketin on otettava käyttöön Windows 7: n toimialueen liittyneet laitteista ohjelmiston jakauman järjestelmän kuten System Center määritysten hallinta. Muista lukea ja viimeistele kuvatut automaattinen laitteen rekisteröinti Azure Active Directory for Windows-toimialue liittyneet laitteiden kanssa.

>[AZURE.NOTE]
 Uusimman ohjeita voit määrittää Automaattiset laitteen rekisteröinti-ohjeaiheissa [määrittäminen Windows-toimialueen automaattisen rekisteröinnin liittyneet laitteiden Azure Active Directory-hakemistosta](active-directory-conditional-access-automatic-device-registration-setup.md).

##<a name="installing-the-device-registration-software-package-on-windows-7-domain-joined-devices"></a>Laitteen rekisteröinti-ohjelmistopaketin asentaminen Windows 7: ssä toimialueen liittyneet laitteet

Laitteen rekisteröinti Windows 7 on saatavilla olevat [ladattavat MSI-paketin](https://connect.microsoft.com/site1164). Paketin on oltava asennettuna Windows 7-tietokoneissa, joihin on liitetty Active Directory-toimialueen. Paketin käyttämällä ohjelmiston jakauman järjestelmän kuten System Center määritysten hallinta kannattaa ottaa käyttöön. MSI-paketin tukee käyttämällä/quiet Hiljainen vakioasennuksen asetukset parametria.
Ohjelmistopaketti on ladattavissa [Microsoft Connect-sivustossa](https://connect.microsoft.com/site1164). Tässä voit valita ja lataa työpaikkasi liittyä Windows 7.

![](./media/active-directory-conditional-access/device-registration-process-windows7.gif)

## <a name="workplace-join-with-azure-active-directory"></a>Työpaikan liity Azure Active Directory-hakemistosta
Windows 7: n toimialueen liittyneet laitteiden laitteen rekisteröinti ei vaadi tai sisällä käyttöliittymää. Kun asennettu tietokoneeseen, kuka tahansa toimialuekäyttäjä kirjautuu-järjestelmään koneen on automaattisesti ja äänettömästi rekisteröity laitteen objektia Azure AD. Azure AD rekisteröity kaikille käyttäjille fyysinen laite on yksi laite-objekti.

Asennusohjelma luo ajoitettu tehtävä järjestelmään, joka toimii käyttäjän konteksti-käyttäjien sisäänkirjautumisessa käynnistyy. Tehtävän Rekisteröi äänettömästi käyttäjän ja Azure AD, kun käyttäjä etumerkki-laite on valmis.
Ajoitettu tehtävä löytyvät tehtävien ajoituksen kirjasto-kohdassa **Microsoft** > **Työpaikkasi liity**.
Tehtävän suorittaa ja rekisteröidä kaikista Active Directory-käyttäjät, kirjaudu sisään tietokoneeseen.
Seuraavassa kuvassa on lueteltu automaattinen laitteen rekisteröinnin vaiheittaisiin ohjeisiin siirtymistä.

![](./media/active-directory-conditional-access/automatic-device-registration-windows7.png)

1. Windows 7-asiakastietokone Active Directory-toimialueen tunnistetiedoilla kirjautuu käyttäjä (tietotyöntekijä).
1. Työpaikan liittyä ajoitettu tehtävä on suoritettu.
1. Käyttäjä todennetaan äänettömästi AD FS käyttämällä Windows-todennusta.
1. Windows 7-Käyttöjärjestelmää rekisteröidään Azure AD-käyttäjälle.
1. Laitteen objekti ja varmennetta luotu Azure AD. Objekti on user@device.
1. Työpaikan Join-varmenne on tallennettu tietokoneeseen.

## <a name="unregistering-your-windows-7-domain-joined-devices"></a>Windows 7: n toimialueen rekisteröintiä liittyneet laitteet

Voit halutessasi unregister liitetty Windows 7-laitteeseen toimimalla seuraavasti: työpaikkasi liittyä asennuksen poistaminen Windows 7: n toimialueen ohjelmistopakettia liittyneet laitteista ohjelmiston jakauman järjestelmän kuten System Center määritysten hallinta.

Avaa komentokehote, Windows 7-laitteeseen ja suorita seuraava komento laitteen rekisteröinnin poistaminen:

    %ProgramFiles%\Microsoft Workplace Join\AutoWorkplace.exe /leave

>[AZURE.NOTE]
>Tämä komento on suoritettava kontekstissa, joka on allekirjoitettu toimialueen kunkin käyttäjän tietokoneen kyselyjä.
Tapahtumienvalvonta ja Windows 7 virheitä liitetty laitteet.

Windows 7-laitteeseen Windowsin tapahtumalokiin näkyy työpaikkasi liittyminen viestejä. Löydät viestit onnistui- ja epäonnistuneista työpaikkasi liittyä tapahtumat. Tapahtumaloki löytyy, valitse sovellukset- ja palvelulokit Viewer > Microsoft työpaikkasi liity.

## <a name="additional-topics"></a>Muita ohjeita

- [Azure Active Directory-laitteen rekisteröinti yleiskatsaus](active-directory-conditional-access-device-registration-overview.md)
- [Automaattinen laitteen rekisteröinti Azure Active Directory for Windows Domain-Joined laitteiden kanssa](active-directory-conditional-access-automatic-device-registration.md)
- [Automaattinen laitteen rekisteröinti Windows 8.1 toimialueen liittyneet laitteiden määrittäminen](active-directory-conditional-access-automatic-device-registration-windows-8-1.md)
- [Automaattinen laitteen rekisteröinti Azure Active Directory for Windows 10: ssä toimialueen liittyneet laitteiden kanssa](active-directory-azureadjoin-devices-group-policy.md)
