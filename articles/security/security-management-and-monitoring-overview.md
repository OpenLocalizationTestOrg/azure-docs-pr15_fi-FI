<properties
   pageTitle="Azure suojauksen hallinta ja seuranta yleiskatsaus | Microsoft Azure"
   description=" Azure on suojaus-järjestelmiä tukeen hallintaa ja Azure pilvipalveluihin ja näennäiskoneiden seuranta.  Tässä artikkelissa on yleiskatsaus nämä core security ominaisuuksia ja palveluita. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-security-management-and-monitoring-overview"></a>Azure suojauksen hallinta ja seuranta yleiskatsaus

Azure on suojaus-järjestelmiä tukeen hallintaa ja Azure pilvipalveluihin ja näennäiskoneiden seuranta. Tässä artikkelissa on yleiskatsaus nämä core security ominaisuuksia ja palveluita. Linkit toimitetaan, avulla kaikkien tiedot, jotta voit lukea lisää artikkeleita.

Microsoftin pilvipalveluihin suojaus on partnership ja jaetun vastuu asiakkaan ja Microsoftin välillä. Jaetun vastuu tarkoittaa Microsoft on vastuussa Microsoft Azure ja fyysinen turvallisuus tietonsa keskittää (käyttämällä Suojausluettelo, kuten lukitun merkin tapahtuma ovien, aitoja ja vartijoita). Lisäksi Azure on vahva tietoturvatasot cloud ohjelmiston tasolla, joka täyttää vaativia asiakkaiden tietoturva, tietosuoja ja vaatimustenmukaisuus tarpeisiin.

Omistat tiedot ja käyttäjätietoja, ne, paikallisen resurssien suojaus ja cloud osien tulosjoukko, jolle voit määrittää suojaus suojaaminen vastuu. Microsoft toimittaa turvaominaisuudet ja ominaisuuksien avulla voit suojata tietoja ja sovelluksia. Suojauksen lisääminen vastuu perustuu pilvipalvelussa tyyppi.

Seuraavassa kaaviossa on esitetty Microsoftin ja asiakkaan vastuu saldo.

![Jaetun vastuu][1]

Artikkelissa tarkempaa perinpohjaisesti käsittelevään artikkeliin suojauksen hallinta- [suojauksen hallinta Azure-tietokannassa](azure-security-management.md).

Seuraavassa on tämän artikkelin kattama core-ominaisuuksia:

- Roolipohjainen käyttöoikeuksien valvonta
- Antimalware
- Monimenetelmäisen todentamisen
- ExpressRoute
- VPN yhdyskäytävät
- Sellaisten jäsenyyksien hallinta
- Tunnistetiedot
- Tietoturvakeskus

## <a name="role-based-access-control"></a>Roolipohjainen käyttöoikeuksien valvonta

Roolipohjainen käytön hallinta (RBAC) on tarkasti rajattuja käyttöoikeuksien hallinnan Azure resurssien. Käytä RBAC, voit myöntää henkilöiden access, ne on suoritettava niiden töiden määrä.  RBAC avulla voit varmistaa, että kun jättävät organisaation ne pääse käyttämään pilvessä resursseja.

Opi lisää:

- [Active Directory-tiimin blogia-RBAC](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
- [Azure Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Antimalware

Azure voit suojata oman näennäiskoneiden haittaohjelmien tiedostoja, mainos ja muut uhkien antimalware ohjelmiston tärkeän toimittajista, kuten Microsoft, Symantec, trendi mikro, McAfee ja Kaspersky avulla.

Microsoft Antimalware on antimalware agentti PaaS roolit ja näennäiskoneiden asentamiseksi. System Center Endpoint Protection mukaan tämä ominaisuus tuo osoitettu paikallisen suojauksen tekniikkaan pilveen.

Laaja integrointi trendi on [Laaja suojauksen](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)Tarjoamme myös™ ja [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ Azure platform-tuotteita. DeepSecurity on virustentorjuntaohjelman ratkaisu ja SecureCloud salaus-ratkaisun. DeepSecurity otetaan käyttöön sisältyy VMs tunniste-malli. Portaalin Käyttöliittymä ja PowerShellin avulla voit käyttää DeepSecurity uusi VMs, jotka ovat parhaillaan kehrätty ylös tai aiemmin VMs, joka on jo otettu käyttöön.

Symantec lopuksi Protection (syys) tuetaan myös Azure. Portaalin integroimalla asiakkaiden on mahdollisuus määrittää, että ne aiot käyttää syys sisällä AM. SYYS voidaan asentaa uusi AM Azure-portaalin kautta tai aiemmin luotuun AM PowerShellin avulla voidaan asentaa.

Opi lisää:

- [Azuren näennäiskoneiden ratkaisujen Antimalware käyttöönotto](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Microsoft Azure pilvipalveluihin ja näennäiskoneiden Antimalware](../security/azure-security-antimalware.md)
- [Asentaa ja määrittää trendi mikro laaja suojauksen palveluna Windows AM](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Asentaminen ja määrittäminen Windows-AM Symantec Endpoint Protection](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Uudet Antimalware vaihtoehdot Azuren näennäiskoneiden – McAfee Endpoint Protection suojaaminen](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Monimenetelmäisen todentamisen

Azure monimenetelmäisen todentamisen (MFA) on todennusmenetelmän, joka edellyttää vähintään kaksi tarkistustapa käyttöä ja Lisää Kriittinen toisen kerroksen arvopaperin käyttäjän kirjautumiset ja tapahtumia. MFA auttaa suojaavat käyttämään tietoja ja sovelluksia käyttäjän tarve-kirjautuminen yksinkertaista kokouksen aikana. Se toimittaa vahva todentaminen vakioverkko solualueen vahvistus asetukset – puhelun, tekstiviesti tai mobiilisovelluksessa ilmoitus- tai koodi ja 3rd osapuolen sijasta tunnukset.

Opi lisää:

- [Monimenetelmäisen todentamisen](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
- [Mikä on Azure Monimenetelmäisen todentamisen?](../multi-factor-authentication/multi-factor-authentication.md)
- [Azure Monimenetelmäisen todentamisen toiminta](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute

Microsoft Azure ExpressRoute avulla voit laajentaa paikallisen lopettaminen Microsoft cloud connectivity tarjoaja helpottaa erillinen yksityisen yhteyden kautta. ExpressRoute voit muodostaa yhteyden Microsoftin pilvipalveluihin, kuten Microsoft Azure, Office 365: ssä ja CRM Online. Yhteys voi olla minkä tahansa-,-tahansa (IP-VPN)-verkko, pisteestä pisteeseen-Ethernet-verkon tai virtual rajat-yhteyden kautta rinnakkain-tilojen yhteyden palveluntarjoajaasi. ExpressRoute yhteydet ei siirry julkisen Internetin välityksellä. Näin ExpressRoute yhteydet tarjoaa useita luotettavasti, nopeampi nopeuksia, pienet viiveet suurempia ja suurempi kuin tavallinen yhteydet suojaus Internetin välityksellä.

Opi lisää:

- [ExpressRoute teknisiä tietoja](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>VPN yhdyskäytävät

VPN-yhdyskäytäviä, kutsutaan myös Azure Virtual verkon yhdyskäytävien käytetään lähettämään verkkoliikennettä virtual verkkojen ja paikallisen sijaintien välillä. Niitä käytetään myös liikenteen useita virtual verkostoja Azure (VNet VNet) välillä.  VPN yhdyskäytävät on suojattu rajat paikallisen Azure ja infrastruktuurin välinen yhteys.

Opi lisää:

- [Tietoja VPN yhdyskäytävät](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Azure verkon suojauksen yleiskatsaus](security-network-overview.md)

## <a name="privileged-identity-management"></a>Sellaisten jäsenyyksien hallinta

Joskus käyttäjät käsittelevät sellaisten työvaiheita Azure resursseja tai SaaS muissa sovelluksissa. Tämä tarkoittaa yleensä organisaatioilla on antaa pysyvä sellaisten access Azure Active Directory (Azure AD). Tämä johtuu kasvava tietoturvariskin cloud isännöimä resurssien organisaatiot riittävän voi seurata, mitä käyttäjät tekevät niiden sellaisten Accessissa.
Lisäksi, jos käyttäjätili sellaisten käytön on ongelmia, yksi sopimusrikkomusta, voi olla vaikutusta cloud tietoturvan. Azure AD-luottamuksellisten jäsenyyksien hallinta auttaa ratkaisemaan riskin pienentäminen oikeudet näyttäminen keston ja lisääntyvien näkyvyys käyttö.  

Sellaisten jäsenyyksien hallinta esitellään käsite tilapäinen järjestelmänvalvojan roolin tai "-aika" järjestelmänvalvojan oikeudet, joka on suoritettava aktivointiprosessin roolin määritetyt tarvitsevalle käyttäjälle. Aktivointi muuttuu varauksen käyttäjän rooliin Azure AD-passiiviset aktiivinen määritetyllä aikavälillä ajan, kuten kahdeksan tuntia.

Opi lisää:

- [Azure AD-luottamuksellisten jäsenyyksien hallinta](../active-directory/active-directory-privileged-identity-management-configure.md)
- [Azure AD sellaisten jäsenyyksien hallinta käytön aloittaminen](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Tunnistetiedot

Azure Active Directory (AD) tunnistetietojen suojaus on epäilyttävistä kirjautumisen toiminnot ja mahdolliset heikkouksien yrityksesi suojautua kootut näkymän. Tunnistetiedot tunnistaa epäilyttävistä toimintoja käyttäjille ja luottamuksellisten (järjestelmänvalvojat) käyttäjätietoja, kuten kannalta voimassa kalastelu signaalit perusteella vuotamaan tunnistetiedot ja kirjaudu apuohjelmien tuntemattomia sijainneista ja tartunnan laitteet.

Ilmoitukset ja suositellut korjaus antamalla tunnistetiedot auttaa pienentämään riskejä reaaliajassa. Se laskee käyttäjän riskien vakavuus ja voit määrittää riskiin perustuva käytännöt avulla automaattisesti suojaavat sovellus käyttää tulevien uhkien.

Opi lisää:

- [Azure Active Directory-tunnistetietojen suojaus](../active-directory/active-directory-identityprotection.md)
- [Kanavan 9: Azure AD ja käyttäjätiedot näkyvät: Identity suojaus esikatselu](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Tietoturvakeskus

Azure Tietoturvakeskus auttaa estämään, haku- ja uhkien vastaaminen ja on lisätty näkyvyys- ja päättää, Azure resurssien suojaus. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta Azure-tilauksissa, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

Tietoturvakeskus avulla voit optimoida ja seurata suojaus Azure resurssien mukaan:

- Voit määrittää jäsenyydelle Azure tilauksen resurssien mukaan yrityksen tarpeiden ja sovellusten tyypin tai suojaustasoa tiedot jokaisen tilauksen käyttöön.
- Azuren näennäiskoneiden tilan seuranta, verkkokäytön ja sovellukset.
- Tarjoaa Priorisoidut suojausvaroitusten, mukaan lukien ilmoitukset integroitu kumppaniratkaisuihin, sekä nopeasti tutkia tietoja ja suosituksia riskien parantaminen hyökkäys luettelo.

Opi lisää:

- [Johdanto Azure Tietoturvakeskus](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
