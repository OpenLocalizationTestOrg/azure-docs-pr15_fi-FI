<properties
    pageTitle="Azure MFA ja AD FS | Microsoft Azure"
    description="Tämä on Azure multi-factor authentication sivu, jossa kerrotaan, miten Azure MFA ja AD FS: N käytön aloittaminen."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Azure Monimenetelmäisen todentamisen ja Active Directory Federation Services käytön aloittaminen



<center>![Cloud](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Jos organisaatiosi on liitetty paikallisen Active Directory Azure Active Directoryn avulla AD FS kanssa, on kaksi vaihtoehtoa käytettäessä Azure Monimenetelmäisen todentamisen.

- Cloud resurssien Azure Monimenetelmäisen todentamisen tai Active Directory Federation Services
- Cloud ja paikallisten resurssien Azure multi-factor Authentication-palvelimen käyttäminen

Seuraavassa taulukossa on yhteenveto välillä suojaaminen resurssien Azure Monimenetelmäisen todentamisen ja AD FS vahvistus-toiminto

|Vahvistus kokemus - Selaa-pohjainen sovellukset|Vahvistus kokemus - selaimen-pohjainen sovellukset
:------------- | :------------- | :------------- |
Azure AD resurssien Azure Monimenetelmäisen todentamisen suojaaminen |<li>Ensimmäiseksi tarkistus suoritetaan paikallisen AD FS avulla.</li> <li>Toinen vaihe on suoritettava cloud todennusta puhelimitse välitettävä menetelmä.</li>|Lopeta käyttäjät voivat käyttää sovelluksen salasanat sisään.
Azure AD-resursseja, käyttämällä Active Directory Federation Services suojaaminen |<li>Ensimmäiseksi tarkistus suoritetaan paikallisen AD FS avulla.</li><li>Toinen vaihe on tehtävä paikallisen honoring vaatimus.</li>|Lopeta käyttäjät voivat käyttää sovelluksen salasanat sisään.

Huomautukset liitetyt käyttäjät ohjelmasalasanat kanssa:

- Sovelluksen salasanat tarkistamisen avulla cloud todennusta, jolloin ne ohittaa federation. Liitetty viestintä käytetään vain aktiivisesti sovelluksen salasanan määrittäminen.
- Paikallisen asiakkaan käyttöoikeuksien valvonta-asetukset eivät ole voimassa ohjelmasalasanat mukaan.
- Menetät paikallisen sovelluksen salasanat todennus kirjaaminen-ominaisuudet.
- Tilin poistaminen käytöstä tai poistaminen saattaa kestää jopa kolme tuntia synkronointi, viivästyy sovelluksen salasanat cloud käyttäjätiedot poistaminen käytöstä tai poistaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja Azure Monimenetelmäisen todentamisen tai AD FS Azure multi-factor Authentication-palvelimen määrittämisestä on seuraavissa artikkeleissa:

- [Cloud resurssien Azure Monimenetelmäisen todentamisen ja AD FS käyttäminen](multi-factor-authentication-get-started-adfs-cloud.md)
- [Suojatun cloud ja paikallisen Azure multi-factor Authentication Server käyttäminen Windows Server 2012 R2 AD FS resurssit](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Cloud ja paikallisten resurssien AD FS 2.0 Azure multi-factor Authentication-palvelimen käyttäminen](multi-factor-authentication-get-started-adfs-adfs2.md)
