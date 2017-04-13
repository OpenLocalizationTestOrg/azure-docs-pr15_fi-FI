<properties
    pageTitle="Azure AD Connect kunto versiohistoria"
    description="Tässä asiakirjassa kerrotaan versioiden Azure AD yhteyden terveys-ja mikä on sisällytetty niihin kuuluvan."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-version-release-history"></a>Azure AD Connect kunto: Release versiohistoria

Azure Active Directory-ryhmän päivittää säännöllisesti Azure AD yhteyden kunto uusista ominaisuuksista ja toiminnoista. Tässä artikkelissa esitellään versiot ja toiminnot, jotka on julkaistu.

## <a name="october-2016"></a>Lokakuussa 2016
**Update-agentti:**
- Azure AD yhteyden kunto-agentti AD FS \(2.6.408.0 versio\)
    1. Tunnistaminen IP-osoitteiden käyttöoikeuksien parannukset
    2. Ilmoituksiin liittyviä virheenkorjauksia
- Azure AD yhteyden kunto-agentti AD DS: ÄÄN (versio 2.6.408.0)
    1. Virheenkorjauksia ilmoitusasetukset.
- Azure AD yhteyden kunto-agentti julkaistu Azure AD Connect versio 1.1.281.0 Synkronoi (versio 2.6.353.0)
    1. Synkronoinnin virheraportit tarvittavat tiedot
    2. Ilmoituksiin liittyviä virheenkorjauksia

**Preview'n uudet ominaisuudet:**
- Synkronoinnin virheraporttien Azure AD-muodosta

**Uudet ominaisuudet:**
- Azure AD yhteyden kunto, AD FS - IP-osoite-kenttään on käytettävissä raportin tietoja 50 käyttäjiä virheelliset käyttäjänimellä ja salasanalla.

## <a name="july-2016"></a>Heinäkuussa 2016

**Preview'n uudet ominaisuudet:**

- [Azure AD Connect AD DS: N kunto](active-directory-aadconnect-health-adds.md).


## <a name="january-2016"></a>Tammikuussa 2016


**Update-agentti:**

- Azure AD yhteyden kunto-agentti AD FS-Liittoutumispalvelut (versio 2.6.91.1512)


**Uudet ominaisuudet:**

- [Testaa yhteys työkalu Azure AD yhteyden kunto agenttien vuoksi](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)


## <a name="november-2015"></a>Marraskuun 2015


**Uudet ominaisuudet:**

- [Roolipohjainen käyttöoikeuksien valvonta](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) -tuki


**Preview'n uudet ominaisuudet:**

- [Azure AD yhteyden kunto-synkronointia varten](active-directory-aadconnect-health-sync.md).

**Kiinteä ongelmat:**

- Virheenkorjauksia aikana agentti merkintöjen virheet.

## <a name="september-2015"></a>Syyskuussa 2015

**Uudet ominaisuudet:**

- Väärä käyttäjänimi salasana raportin AD FS: lle
- Tuki, kun haluat määrittää Todentamattomille HTTP-välityspalvelin
- Agentti määrittämiseksi Server core tuki
- AD FS ilmoitukset parannukset
- Lataa parannukset: Azure AD yhteyden kunto agentti AD FS connectivity ja tiedot.


**Kiinteä ongelmat:**

- Ohjelmavirhe korjaa käyttö havainnollistamisen AD FS virhe tyypeissä.


## <a name="june-2015"></a>Kesäkuussa 2015

**Alkuperäisessä versiossa ja AD FS AD FS ‑välityspalvelin Azure AD yhteyden kunto.**

**Uudet ominaisuudet:**

- Ilmoitusten määrittäminen seurantaa AD FS ja AD FS ‑välityspalvelin palvelinten sähköposti-ilmoitukset.
- AD FS topologian ja AD FS suorituskyvyn laskureita kuviot helposti.
- Trendi onnistuneen suojaustunnuksen pyyntöjä AD FS-palvelimiin ryhmitelty sovellukset, todennusmenetelmät, pyydä verkon sijainti jne.
- Trendien epäonnistuneen pyynnön AD FS-palvelimiin Ryhmittelyperuste-sovellukset, virhe tyypit jne.
- Yksinkertainen agentti käyttöönotto Azure AD yleisen järjestelmänvalvojan tunnistetiedoilla.  




## <a name="next-steps"></a>Seuraavat vaiheet
Lisätietoja [näytön paikallisen oman tunnistetietojen infrastruktuuri ja synkronointi services pilveen](active-directory-aadconnect-health.md).
