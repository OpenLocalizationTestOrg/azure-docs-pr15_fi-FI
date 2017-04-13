<properties
    pageTitle="Azure AD Connect: Portit | Microsoft Azure"
    description="Tämä sivu on portit, joita tarvitaan auki, jotta Azure AD Connect tekniset tiedot-sivu"
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
    ms.date="08/25/2016"
    ms.author="billmath"/>

# <a name="hybrid-identity-required-ports-and-protocols"></a>Hybrid tunnistetietojen pakollinen portit ja protokollat
Seuraava asiakirja on tekniset tiedot, anna tarvittavat portit ja protokollat hybrid identity-ratkaisun toteuttaminen varten tarvittavat tiedot. Käytä seuraavassa kuvassa ja vastaavan taulukossa.

![Mikä on Azure AD Connect](./media/active-directory-aadconnect-ports/required1.png)

## <a name="table-1---azure-ad-connect-and-on-premises-ad"></a>Taulukko 1 - Azure AD yhteyden ja paikallisen AD
Seuraavassa taulukossa kuvataan portit ja protokollat, joita tarvitaan Azure AD Connect palvelimen väliseen viestintään ja paikallisen AD.

Protokolla | Portit | Kuvaus
--------- | --------- |---------
DNS|53 (TCP/UDP)| Kohde-metsää DNS-hakuja.
Kerberos|88 (TCP/UDP)| AD-metsää Kerberos-todentamista.
MS RPC |135 (TCP/UDP)| Käytetään sen ensimmäisen määrityksen Azure AD Connect ohjatun toiminnon aikana, kun se sitoo AD-metsää.
LDAP|389 (TCP/UDP)| Tietojen tuominen AD käytetään. Tiedot salataan Kerberos-merkki & leima.
LDAP/SSL|636 (TCP/UDP)| Tietojen tuominen AD käytetään. Tiedonsiirron on allekirjoitettu ja salattu. Käyttää vain, jos käytät SSL.
RPC |49152 – 65535 (satunnainen hyvin RPC Port)(TCP/UDP)| Käyttää Azure AD Connect ensimmäisen määrityksen aikana, kun se sitoo AD-metsien. Lisätietoja on kohdassa [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017)ja [KB224196](https://support.microsoft.com/kb/224196) .

## <a name="table-2---azure-ad-connect-and-azure-ad"></a>Taulukon 2 - Azure AD-yhteyden ja Azure AD
Tässä taulukossa on kuvattu portit ja protokollat Azure AD Connect palvelimen ja Azure AD välisen varten tarvittavat.

Protokolla |Portit |Kuvaus
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Lataa kumottujen varmenteiden luettelot löytyvät (varmenteen kumottujen varmenteiden luetteloiden) Tarkista SSL-varmenteita käytetään.
HTTPS|443(TCP/UDP)| Käyttää Azure AD kanssa.

Luettelo-osoitteet ja IP osoitteet, sinun on avattava palomuurissa, katso [Office 365: n URL-osoitteet ja IP-osoitealueet](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## <a name="table-3---azure-ad-connect-and-federation-serverswap"></a>Taulukon 3 - Azure AD-yhteyden ja Federation palvelinten/WAP
Tässä taulukossa on kuvattu portit ja protokollat välisen Azure AD Connect-palvelimen nimen ja Federation/WAP palvelimia varten tarvittavat.  

Protokolla |Portit |Kuvaus
--------- | --------- |---------
HTTP|80 (TCP/UDP)| Lataa kumottujen varmenteiden luettelot löytyvät (varmenteen kumottujen varmenteiden luetteloiden) Tarkista SSL-varmenteita käytetään.
HTTPS|443(TCP/UDP)| Käyttää Azure AD kanssa.
WinRM|5985| WinRM Listener

## <a name="table-4---wap-and-federation-servers"></a>Taulukko 4 - WAP ja Liittoutumispalvelimia
Tässä taulukossa on kuvattu portit ja protokollat välisen liittoutumispalvelimia että WAP palvelimia varten tarvittavat.

Protokolla |Portit |Kuvaus
--------- | --------- |---------
HTTPS|443(TCP/UDP)| Käyttää todennusta varten.

## <a name="table-5---wap-and-users"></a>Taulukko 5 – WAP ja käyttäjille
Tässä taulukossa on kuvattu portit ja protokollat, joita tarvitaan välisen käyttäjät ja WAP-palvelimiin.

Protokolla |Portit |Kuvaus
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Käyttää laitteen todennusta varten.
TCP|49443 (TCP)| Käyttää varmenteen todentamiseen.

## <a name="table-6a--6b---azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Taulukko 6 a ja 6 b - agentti Azure AD yhteyden kunto varten (AD FS/Sync) ja Azure AD
Seuraavassa taulukossa kuvataan päätepisteet, portit ja protokollat, joita tarvitaan Azure AD yhteyden kunto tekijöiden ja Azure AD väliseen viestintään

### <a name="table-6a---ports-and-protocols-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>Taulukko 6 a - portit ja protokollat Azure AD yhteyden kunto-agentti varten (AD FS/Sync) ja Azure AD
Tässä taulukossa on kuvattu seuraavassa lähtevä portit ja protokollat Azure AD yhteyden kunto tekijöiden ja Azure AD välisen varten tarvittavat.  

Protokolla |Portit  |Kuvaus
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)| Lähtevän
Azure palvelun Bus|5671 (TCP/UDP)| Lähtevän

### <a name="6b---endpoints-for-azure-ad-connect-health-agent-for-ad-fssync-and-azure-ad"></a>6-päätepisteet Azure AD yhteyden kunto-agentti varten (AD FS/Sync) ja Azure AD b
Luettelo päätepisteet on lisätietoja [Azure AD yhteyden kunto-agentti vaatimukset-osiossa](active-directory-aadconnect-health-agent-install.md#requirements).
