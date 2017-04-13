<properties
    pageTitle="Azure AD Connect: Käytön aloittaminen käyttämällä pika-asetuksia | Microsoft Azure"
    description="Opettele lataaminen, asentaminen ja suorittaminen Azure AD Connect ohjattu asennus."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Azure AD Connect käyttämällä pika-asetuksia käytön aloittaminen
Azure AD Connect **Pika-asetuksia** käytetään, kun käytössä on yksi metsää topologian ja [salasanan synkronointi](../active-directory-aadconnectsync-implement-password-synchronization.md) todennusta varten. **Pika-asetuksia** on oletusasetus, ja sitä käytetään useimmin käyttöön-skenaario. Olet vain lyhyt muutamalla poissa siirtämisestä paikallista hakemistoa pilveen.

Ennen kuin aloitat asennuksen Azure AD Connect, varmista, että voit [ladata Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) ja valmis ennen tarvittavat ohjeet ovat kohdassa [Azure AD Connect: laitteisto ja edellytykset](../active-directory-aadconnect-prerequisites.md).

Jos Expressin asetukset eivät vastaa omaa topologian, Lisätietoja on muissa tilanteissa [liittyvä dokumentaatio](#related-documentation) .

## <a name="express-installation-of-azure-ad-connect"></a>Pika-asennus, Azure AD Connect
Voit tarkastella käytössä [videot](#videos) -osassa seuraavasti.

1. Kirjaudu paikallisen järjestelmänvalvojan haluat asentaa Azure AD Connect palvelimeen. Tee tämä haluat synkronointi-palvelin-palvelimeen.
2. Etsi ja kaksoisnapsauta **AzureADConnect.msi**.
3. Valitse aloitusnäytössä siitä käyttöoikeusehdot ruutuun ja valitse **Jatka**.  
4. Express-näytössä valitsemalla **Käytä pika-asetuksia**.  
![Tervetuloa Azure AD-muodosta](./media/active-directory-aadconnect-get-started-express/express.png)
5. Anna Muodosta yhteys näytön Azure AD-Azure AD käyttäjänimi ja salasana, Yleinen järjestelmänvalvoja. Valitse **Seuraava**.  
![Yhteyden muodostaminen Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) Jos virhesanoma ja yhteyden ilmenee ongelmia, katso [yhteysongelmien vianmääritys](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. Muodosta AD DS-näyttöön Kirjoita yrityksen järjestelmänvalvojan tilin käyttäjänimi ja salasana. Voit kirjoittaa toimialueen osa NetBios tai FQDN muodossa, eli FABRIKAM\administrator tai fabrikam.com\administrator. Valitse **Seuraava**.  
![Yhteyden muodostaminen AD DS: ÄÄN](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. [**Azure AD - kirjautuminen määritys**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) -sivulla näkyy vain, jos et ole suorittanut [Tarkista toimialueen](../active-directory-add-domain.md) [edellytykset](../active-directory-aadconnect-prerequisites.md).
![Vahvistamaton toimialueet](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Jos näet tämän sivun, tarkista jokaisen toimialueen, joka on merkitty **Ei lisätä** , ja **Ei ole vahvistettu**. Varmista, että näistä toimialueista, käytössäsi on tarkistettu Azure AD. Kun olet tarkistanut toimialueen, valitse Päivitä-symboli.
8. Siirry Ready to määrittäminen näytössä, valitse **Asenna**.
    - Voit myös määrittäminen-sivulla valmis, voit poistaa valinnan **Aloita synkronointi heti, kun määritys on valmis** -valintaruutu. Olisi Poista tämä valintaruutu, jos haluat tehdä lisämäärityksiä, kuten [suodatus](../active-directory-aadconnectsync-configure-filtering.md). Jos tämä asetus kumoaa sen valinnan, ohjattu toiminto määrittää synkronoinnin, mutta jättää käytöstä ajoituksen. Se ei toimi, ennen kuin otat sen manuaalisesti suorittamalla [Ohjattu asennus](../active-directory-aadconnectsync-installation-wizard.md).
    - Jos sinulla on Exchange paikallisen Active Directoryssa, sinun on valmisteltu vaihtoehto käyttöön [**Exchange-yhdistelmäympäristö**](https://technet.microsoft.com/library/jj200581.aspx). Tämä asetus, jos aiot käyttää Exchange-postilaatikot sekä cloud ja paikallisten samanaikaisesti.
![Haluatko määrittää Azure AD Connect](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Kun asennus on valmis, valitse **Lopeta**.
10. Kun asennus on valmis, kirjaudu ja kirjaudu sisään uudelleen, ennen kuin käytät synkronoinnin palvelun hallinta tai synkronoinnin säännön editorilla.

## <a name="videos"></a>Videot

Pika-asennus käyttämisestä video-kohdassa:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Seuraavat vaiheet
Kun olet tutustunut Azure AD Connect asennettuna, voit [Tarkista asennus ja määrittää käyttöoikeuksia](../active-directory-aadconnect-whats-next.md).

Lisätietoja näistä toiminnoista, jotka on otettu käyttöön asennuksessa: [automaattisen päivityksen](../active-directory-aadconnect-feature-automatic-upgrade.md), [Estä vahingossa poistaa](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)ja [Azure AD yhteyden kunto](../active-directory-aadconnect-health-sync.md).

Lisätietoja näiden yleisiä aiheita: [ajoitus ja käynnistämisestä Synkronoi](../active-directory-aadconnectsync-feature-scheduler.md).

Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Aiheeseen liittyvät ohjeet

Aihe |  
--------- | ---------
Azure AD Connect yleiskatsaus | [Azure Active Directory-integraation paikallisen käyttäjätietoja](../active-directory-aadconnect.md)
Asenna käyttämällä mukautettuja asetuksia | [Mukautetun asennuksen Azure AD Connect](active-directory-aadconnect-get-started-custom.md)
Dirsync-työkalun päivittäminen | [Päivittää Azure AD-synkronointityökalu (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Asennus tilit | [Lisätietoja Azure AD Connect asiakkaat ja käyttöoikeudet](active-directory-aadconnect-accounts-permissions.md)
