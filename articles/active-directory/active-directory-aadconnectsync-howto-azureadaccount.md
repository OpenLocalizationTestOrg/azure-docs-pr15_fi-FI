<properties
    pageTitle="Azure AD Connect synkronointi: Azure AD-palvelutilin hallinnasta | Microsoft Azure"
    description="Tässä ohjeaiheessa asiakirjat palauttaminen Azure AD-palvelutilin."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, Azure AD Connect Synkronoi yhdistimen palvelutilin salasanan nollaaminen"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure AD Connect synkronointi: Azure AD-palvelutilin hallinta
Azure AD-yhdistin käytetyn palvelutilin pitäisi olla palvelun maksuton. Jos haluat nollata sen, valitse tämä artikkeli koskee voit. Jos yleisellä järjestelmänvalvojalla on mukaan virheen Palauta PowerShellin palvelutilin salasana.

## <a name="reset-the-credentials"></a>Palauta tunnistetiedot
Jos määritetty Azure AD-yhdistimen palvelutilin ei voi muodostaa yhteyttä todennusta ongelmien vuoksi Azure AD-salasanan nollaaminen.

1. Kirjaudu sisään Azure AD Connect synkronointi palvelimeen ja Käynnistä PowerShell.
2. Suorita `Add-ADSyncAADServiceAccount`.  
![PowerShell-cmdlet-komento addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD-yleisen järjestelmänvalvojan tunnistetietoja.

Tämä cmdlet-komento palauttaa palvelutilin salasanan ja päivität sen sekä Azure AD-synkronointi-ohjelma.

## <a name="known-issues-these-steps-can-solve"></a>Näin voit ratkaista tunnetut ongelmat
Tässä osassa on luettelo asiakkaat, joilla on vahvistettu tunnistetietojen luominen Azure AD-palvelutilin palautettava ilmoittaa virheitä.

-----------
Tapahtuman 6900  
Palvelimen tapahtui odottamaton virhe käsiteltäessä salasanan muuttaminen ilmoituksen:  
AADSTS70002: Virhe olevan tunnistetiedot. AADSTS50054: Todennuksessa käytetään vanha salasana.

----------
Tapahtuman 659  
Virhe noudettaessa salasanan käytännön synkronoinnin määritys. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Virhe olevan tunnistetiedot. AADSTS50054: Todennuksessa käytetään vanha salasana.

## <a name="next-steps"></a>Seuraavat vaiheet

**Yleistä aiheita**

- [Azure AD Connect synkronointi: tietoja ja mukauttaa synkronointi](active-directory-aadconnectsync-whatis.md)
- [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
