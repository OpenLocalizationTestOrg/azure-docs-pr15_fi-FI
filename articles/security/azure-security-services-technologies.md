<properties
   pageTitle="Azure suojauspalveluiden ja -tekniikoiden | Microsoft Azure"
   description="Tässä artikkelissa Azure suojaus-palveluiden ja -tekniikoiden curated luettelo."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="yurid"/>

# <a name="azure-security-services-and-technologies"></a>Azure suojauspalveluiden ja -tekniikoiden

Tutustu keskusteluihin nykyisten ja tulevien Azure asiakkaiden kanssa, on usein pyydetään "sinulla on kaikki suojauksen luettelo liittyviä palveluja ja -tekniikoiden, Azure uusista ominaisuuksista?"
 
Ymmärrämme, kun arvioit cloud palveluntarjoajan teknisten asetusten, kannattaa lisätä tällaisia luettelon avulla voit tarkastella alaspäin tarkempaa milloin aika on itsellesi.

Seuraavassa on Microsoftin tavoitteena on luettelon ensimmäinen työmäärään. Ajan myötä tämän luettelon muuttaminen ja suurenevat järjestyksessä, aivan kuin Azure. Luettelossa on luokiteltu ja luokkaluettelo myös kasvaa ajan kuluessa. Varmista, että tämän sivun pysyt ajan tasalla Microsoftin tietoturva-palveluiden ja -tekniikoiden säännöllisesti. 

## <a name="azure-security---general"></a>Azure suojaus - Yleiset
- [Azure Tietoturvakeskus](https://azure.microsoft.com/documentation/services/security-center/)
- [Azure avaimen säilö](https://azure.microsoft.com/documentation/services/key-vault/)
- [Azure salauksen](azure-security-disk-encryption.md)
- [Lokitiedoston Analytics](../log-analytics/log-analytics-overview.md)
- [Azure keskihajonta/testi harjoituksia](https://azure.microsoft.com/documentation/services/devtest-lab/)

## <a name="azure-storage-security"></a>Azure-tallennustilan suojaus
- [Azure-tallennustilan palvelun salaus](../storage/storage-service-encryption.md)
- [StorSimple salattu Hybrid tallennustila](https://azure.microsoft.com/documentation/services/storsimple/)
- [Azure asiakkaan salaus](../storage/storage-client-side-encryption.md)
- [Azure-tallennustilan jaettu Access allekirjoitukset](../storage/storage-dotnet-shared-access-signature-part-1.md)
- [Azure-tallennustilan tilin näppäimet](../storage/storage-create-storage-account.md)
- [Azure tiedoston jakaja SMB 3.0 salaus](../storage/storage-dotnet-how-to-use-files.md)
- [Azure-tallennustilan Analytics](https://msdn.microsoft.com/library/hh343270.aspx)

## <a name="azure-database-security"></a>Azure-tietokanta-suojaus
- [Azure SQL-palomuurin](../sql-database/sql-database-firewall-configure.md)
- [Azure SQL-solun tason salaus](https://blogs.msdn.microsoft.com/sqlsecurity/2015/05/12/recommendations-for-using-cell-level-encryption-in-azure-sql-database/)
- [Azure SQL-yhteyden salaus](../sql-database/sql-database-security-guidelines.md)
- [Azure SQL-todennus](../sql-database/sql-database-security-guidelines.md)
- [Azure SQL aina salaus](https://msdn.microsoft.com/library/mt163865.aspx)
- [Azure SQL-sarakkeen tason salaus](https://msdn.microsoft.com/library/ms179331.aspx)
- [Läpinäkyvä Azure SQL-salauksen](https://msdn.microsoft.com/library/dn948096.aspx)
- [Azure SQL-tietokanta tarkistaminen](../sql-database/sql-database-auditing-get-started.md)

## <a name="azure-identity-and-access-management"></a>Azure tunnistetietojen ja käyttöoikeushallinta
- [Azure Roolipohjainen käyttöoikeuksien valvonta](../active-directory/role-based-access-control-configure.md)
- [Azure Active Directory](../active-directory/active-directory-whatis.md)
- [Azure Active Directory-B2C](../active-directory-b2c/active-directory-b2c-get-started.md)
- [Azure Active Directory-toimialuepalveluista](https://azure.microsoft.com/documentation/services/active-directory-ds/)
- [Azure Monimenetelmäisen todentamisen](../multi-factor-authentication/multi-factor-authentication.md)

## <a name="backup-and-disaster-recovery"></a>Varmuuskopiointi ja palauttaminen
- [Azure varmuuskopiointi](https://azure.microsoft.com/documentation/services/backup/)
- [Azure sivuston palauttaminen](https://azure.microsoft.com/documentation/services/site-recovery/)

## <a name="azure-networking"></a>Azure verkko
- [Verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-nsg.md)
- [Azure VPN-yhdyskäytävän](../vpn-gateway/vpn-gateway-about-vpngateways.md)
- [Azure sovelluksen yhdyskäytävän](../application-gateway/application-gateway-introduction.md)
- [Azure kuormituksen](../load-balancer/load-balancer-overview.md)
- [Azure ExpressRoute](../expressroute/expressroute-introduction.md)
- [Azure liikenteen hallinta](../traffic-manager/traffic-manager-overview.md)
- [Azure sovelluksen välityspalvelin](../active-directory/active-directory-application-proxy-enable.md)
