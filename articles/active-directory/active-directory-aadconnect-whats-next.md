<properties
    pageTitle="Azure AD Connect: Seuraavat vaiheet ja kuinka voit hallita Azure AD Connect | Microsoft Azure"
    description="Lue, miten siirtämisestä Azure AD Connect oletusarvon määrittäminen ja toiminnallisia tehtäviä."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="next-steps-and-how-to-manage-azure-ad-connect"></a>Seuraavat vaiheet ja Azure AD Connect hallinta
Seuraavassa on Lisäasetukset toiminnallisia aiheisiin, joissa voit mukauttaa Azure Active Directory yhteyden vastaa organisaation tarpeita ja vaatimuksia.  

## <a name="add-additional-sync-administrators"></a>Lisää muita Synkronoi Järjestelmänvalvojat
Oletusarvoisesti vain käyttäjä, joka asennuksen ja paikallisen järjestelmänvalvojien saa oikeuden hallita asennettu synkronointi-ohjelma. Etsi ryhmä nimeltä ADSyncAdmins paikallisen palvelimen voi käyttää ja hallita synkronoinnin, ohjelma lisää henkilöitä ja lisää ryhmään.

## <a name="assigning-licenses-to-azure-ad-premium-and-enterprise-mobility-users"></a>Azure AD Premium- ja Enterprise-liikkuvuus käyttäjien käyttöoikeuksien määrittämisestä

Nyt kun käyttäjät on synkronoitu pilveen, sinun on määrittää ne käyttöoikeudet, jotta he voivat käyttäminen cloud-sovelluksissa, kuten Office 365: ssä.

### <a name="to-assign-an-azure-ad-premium-or-enterprise-mobility-suite-license"></a>Jos haluat määrittää Azure AD Premium- tai Enterprise Mobility ohjelmiston käyttöoikeus
--------------------------------------------------------------------------------
1. Kirjaudu sisään Azure-portaaliin järjestelmänvalvojana.
2. Valitse vasemmassa reunassa **Active Directorysta**.
3. Kaksoisnapsauta kansiota, jonka haluat ottaa käyttöön käyttäjien Active Directory-sivulla.
4. Valitse kansio-sivun yläosassa **käyttöoikeudet**.
5. Valitse Käyttöoikeudet-sivulla Active Directory-Premium tai yrityksen Mobility Suite ja valitse **Liitä**.
6. Valintaikkunassa valitse poistettavat käyttäjät, joille haluat määrittää käyttöoikeuksia ja valitse sitten Tallenna muutokset valintamerkki-kuvake.


## <a name="verifying-the-scheduled-synchronization-task"></a>Ajoitetun synkronoinnin Tehtävien tarkistaminen
Jos haluat tarkistaa synkronoinnin tilan, voit tehdä tämän valitsemalla Azure-portaalissa.

### <a name="to-verify-the-scheduled-synchronization-task"></a>Voit tarkistaa ajoitetun synkronoinnin tehtävä
--------------------------------------------------------------------------------
1. Kirjaudu sisään Azure-portaaliin järjestelmänvalvojana.
2. Valitse vasemmassa reunassa **Active Directorysta**.
3. Kaksoisnapsauta kansiota, jonka haluat ottaa käyttöön käyttäjien Active Directory-sivulla.
4. Valitse Hakemistosivun yläosassa **Yhteystietojen integrointi**.
5. Valitse integration paikallisen active directory-muistiinpanoa viimeksi synkronoi.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/verify.png)</center>

## <a name="starting-a-scheduled-synchronization-task"></a>Ajoitetun synkronoinnin tehtävän käynnistäminen
Jos haluat suorittaa synkronoinnin tehtävän voit tehdä tämän suorittamalla Azure AD Connect ohjatussa toiminnossa uudelleen.  Sinun on annettava Azure AD-tunnistetiedot.  Valitse ohjatussa **Mukauta synkronointiasetusten** tehtävä, ja valitse Seuraava ohjatussa toiminnossa. Lopussa Varmista, että **Aloita synkronointi heti, kun alkuperäinen määritys on valmis** -valintaruutu on valittuna.

<center>![Cloud](./media/active-directory-aadconnect-whats-next/startsynch.png)</center>

Lisätietoja Azure AD Connect-synkronointi: ajoitus-kohdassa [Azure AD yhteyden ajoitus](active-directory-aadconnectsync-feature-scheduler.md)


## <a name="additional-tasks-available-in-azure-ad-connect"></a>Käytettävissä olevat Azure AD Connect tehtävät
Oman alkuperäinen Azure AD Connect asennuksen jälkeen voit aina aloittaa ohjatun toiminnon uudelleen Azure AD Connect Käynnistä sivun tai työpöydän pikakuvaketta.  Huomaat, että ohjattu toiminto loppuun uudelleen on joitakin lisätoimien suorittamista lomakkeen uudet ominaisuudet.  

Seuraavassa taulukossa on yhteenveto tehtävistä ja lyhyt kuvaus, jokainen niistä.

![Liittyminen sääntö](./media/active-directory-aadconnect-whats-next/addtasks.png)


Lisää tehtävä | Kuvaus
------------- | ------------- |
Näytä valitun skenaario  |Voit tarkastella nykyistä Azure AD Connect-ratkaisua.  Tämä sisältää Yleiset asetukset, synkronoituja kansioita, synkronoinnin asetuksia.
Synkronoinnin asetusten mukauttaminen | Voit muuttaa myös muita Active Directory-puuryhmien lisääminen määritysten tai ottaminen käyttöön synkronointiasetukset käyttäjän, kuten nykyiset määritykset ryhmän, laite tai takaisinkirjoituksen salasana.
Vaiheet-tila käyttöön |  Näin voit vaiheen tiedot, jotka synkronoidaan myöhemmin, mutta mitään viedään Azure AD tai Active Directorysta.  Voit esikatsella synkronoinnin, ennen kuin ne sijaitsevat.

## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [ympäristön integroinnissa paikallisen-käyttäjätietojen Azure Active Directory-hakemistosta](active-directory-aadconnect.md).
