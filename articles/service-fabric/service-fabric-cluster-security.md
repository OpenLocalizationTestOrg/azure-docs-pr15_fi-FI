<properties
   pageTitle="Suojattu palvelu kangasta klusterin | Microsoft Azure"
   description="Tässä artikkelissa kuvataan suojaustapoja palvelun kangasta klusterin ja toteuttaa tilanteisiin eri tekniikoista."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Palvelun kangasta klusterin skenaarioita

Palvelun kangasta klusteriin on omistat resurssi. Klustereiden aina voidaan suojata estää luvattomia käyttäjiä muodostamasta yhteyttä klusterin erityisesti silloin, kun se on käytössä olevat tuotannon toiminnoista. On mahdollista luoda suojaamaton klusterin, mutta tällöin niin sallii anonyymi käyttäjä, voit muodostaa yhteyden, jos se paljastaa hallinta päätepisteet julkinen Internet-yhteys. 

Tässä artikkelissa on yleiskatsaus suojaustapoja klustereiden Azure tai erillisen ja toteuttaa tilanteisiin eri tekniikoista varten. Klusterin suojaustapoja ovat seuraavat:

- Solmun solmu suojaus
- Asiakkaan solmu suojaus
- Roolipohjainen käyttöoikeuksien valvonta (RBAC)

## <a name="node-to-node-security"></a>Solmun solmu suojaus
Suojaa välisen VMs tai klusterin tietokoneissa. Näin varmistat, että vain tietokoneisiin, joilla on oikeus liittyä klusterin voivat osallistua isännöinnin sovellusten ja palveluiden klusterin.

![Solmun solmu viestintä kaavio][Node-to-Node]

Tietokoneessa käynnissä Windows Azure tai erillisen klustereiden klustereiden käyttää [Varmenteen suojaus](https://msdn.microsoft.com/library/ff649801.aspx) tai [Windowsin suojaus](https://msdn.microsoft.com/library/ff649396.aspx) Windows Server-tietokoneissa.
### <a name="node-to-node-certificate-security"></a>Solmun solmu varmenteen suojaus
Palvelun kangasta käyttää X.509-palvelimen sertifikaatteja, joita voit määrittää osana solmu tyyppi-määrityksiä, kun luot klusterin. Nämä varmenteet ovat ja kuinka voit hankkia tai luo ne pikaisesti annetaan tämän artikkelin lopussa.

Varmenteen suojausasetukset on määritetty luotaessa klusterin Azure portaalin, Azure Resurssienhallinta malleja tai erillisen JSON mallin avulla. Voit määrittää ensisijainen varmenne- ja valinnaiset toissijainen varmenne, jota käytetään varmenteen päällekirjoitusta. Voit määrittää ensisijaisen ja toissijaisen varmenteet saa olla sama kuin järjestelmänvalvojan asiakkaan ja [asiakkaan solmu suojauksen](#client-to-node-security)määrittäminen vain luku-asiakasohjelman varmenteet.

Lue lisätietoja varmenteen suojauksen määrittäminen klusterin [klusterin Azure Resurssienhallinta mallia käyttämällä määrittäminen](service-fabric-cluster-creation-via-arm.md) Azure.

Erillinen Windows Server Lue [suojatun erillinen klusterin X.509-varmenteita avulla Windowsiin](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Solmun solmu Windowsin suojaus
Erillinen Windows Server Lue [suojatun erillinen klusterin Windows käyttämällä Windowsin suojaus](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Asiakkaan solmu suojaus
Todentaa asiakkaat ja suojaa asiakkaan ja yksittäisten klusterin solmuissa välisen. Tämäntyyppinen suojaus todentaa ja suojaa asiakkaan viestintä, jolla varmistetaan, että vain valtuutettujen käyttäjien käyttöoikeus klusterin ja ottaa käyttöön klusterin sovellukset. Asiakkaiden on yksilöidä Windowsin suojaus-tunnistetietoja tai niiden varmenteen suojausvaltuudet kautta.

![Asiakkaan solmu viestintä kaavio][Client-to-Node]

Tietokoneessa käynnissä Windows Azure tai erillisen klustereiden klustereiden käyttää [Varmenteen suojaus](https://msdn.microsoft.com/library/ff649801.aspx) tai [Windowsin suojaus](https://msdn.microsoft.com/library/ff649396.aspx).

### <a name="client-to-node-certificate-security"></a>Asiakkaan solmu varmenteen suojaus
 Asiakkaan solmu varmenteen suojausasetukset on määritetty luotaessa klusterin Azure portal Resurssienhallinta malleja tai erillisen JSON mallin määrittämällä järjestelmänvalvoja-Asiakasvarmenne ja/tai käyttäjän Asiakasvarmenne kautta.  Järjestelmänvalvojan asiakastietokoneen ja käyttäjää asiakkaan varmenteet määrittämäsi saa olla sama kuin Määritä [solmu solmu](#node-to-node-security)arvopaperille ensisijainen ja toissijainen varmenteet.

Yhteyden muodostaminen järjestelmänvalvojan sertifikaatilla klusterin asiakkaat on täydet oikeudet hallintatoiminnot.  Yhteyden muodostaminen käyttämällä vain luku-käyttäjän Asiakasvarmenne klusterin asiakkaiden on vain luku käyttöoikeus hallintatoiminnot. Toisin sanoen nämä varmenteet käytetään roolin kantalukujen käyttöoikeuksia (RBAC) on kuvattu tämän artikkelin.

Lue lisätietoja varmenteen suojauksen määrittäminen klusterin [klusterin Azure Resurssienhallinta mallia käyttämällä määrittäminen](service-fabric-cluster-creation-via-arm.md) Azure.

Erillinen Windows Server Lue [suojatun erillinen klusterin X.509-varmenteita avulla Windowsiin](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Azure asiakkaan solmu Azure Active Directory (AAD) suojaus
Azure-käyttöjärjestelmässä klustereiden suojata myös käyttöoikeuksien hallinta päätepisteet Azure Active Directory (AAD) avulla. Artikkelissa on tietoja tarpeen AAD palvelutiedot luomisesta, miten täyttämiseen klusterin luonnin aikana ja yhdistämisestä näiden klustereiden jälkeenpäin [määrittäminen klusterin mukaan Azure Resurssienhallinta-mallin avulla](service-fabric-cluster-creation-via-arm.md) .

## <a name="security-recommendations"></a>Suojausta koskevia suosituksia
Azure klustereiden on suositeltavaa, että todentaa asiakkaiden ja solmu solmu suojauksen varmenteet AAD suojauksen avulla.

Varten erillinen Windows Server klustereiden, on suositeltavaa, että käytät Windowsin suojaus ryhmän kanssa hallitut tilit (GMA), jos käytössäsi on Windows Server 2012 R2 ja Active Directory. Käytä Windowsin suojaus edelleen muussa Windows-tilien kanssa.

## <a name="role-based-access-control-rbac"></a>Roolipohjainen käyttöoikeuksien valvonta (RBAC)
Käyttöoikeuksien hallinta avulla klusterin järjestelmänvalvoja voi rajoittaa käyttäjät-klusterin parantaminen eri ryhmille klusterin tiettyjen toimintojen käyttöä. Kahden eri Ohjausobjektityypit tuetaan muodostamisesta klusteriin asiakkaat: järjestelmänvalvojaksi ja liiketoiminta-rooliin.

Järjestelmänvalvojilla on täydet oikeudet hallintatoiminnot (mukaan lukien luku-/ kirjoitusoikeudet ominaisuudet). Käyttäjien on oletusarvoisesti vain lukuoikeudet hallintatoiminnot (esimerkiksi Kyselyominaisuudet) ja mahdollisuuden pitää korjata sovelluksia ja palveluja.

Voit määrittää järjestelmänvalvoja- ja käyttäjä-asiakasohjelman roolien klusterin luominen milloin antamalla eri käyttäjätiedot (varmenteet-AAD jne.) kunkin. Lisätietoja oletusarvoisen käyttöoikeusasetukset ja oletusasetusten muuttamisesta on artikkelissa [Roolipohjainen käyttöoikeuksien valvonta palvelun kangasta asiakkaille](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>X.509-varmenteita ja palvelun kangasta
X.509 digitaalisten varmenteiden käytetään yleensä asiakkaiden ja palvelinten tarkistamiseen ja viestien salaaminen ja digitaalinen allekirjoittaminen. Lisätietoja näiden varmenteiden Siirry [käyttäminen varmenteet](http://msdn.microsoft.com/library/ms731899.aspx).

Tärkeitä huomioitavia seikkoja:

- Käynnissä tuotannon työmääriä klustereissa sertifikaatit kannattaa luotu oikein määritetyn Windows Server-todistus-palvelu tai saatu hyväksytyn [Varmenteen myöntäjä (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
- Käytä koskaan minkä tahansa väliaikaisesti tai testaaminen varmenteet tuotannon, jotka on luotu työkaluilla, kuten MakeCert.exe.
- Voit käyttää itse allekirjoitettua varmennetta, mutta vain tee niin testi klustereiden ja ei tuotannon.

### <a name="server-x509-certificates"></a>Palvelimen X.509-varmenteita

Palvelimen varmenteet on ensisijainen todennustapa server (solmu) asiakkaille tai todennustapa server (solmu) (solmu)-palvelimeen. Yksi alkuperäinen tarkistukset, kun asiakas- tai solmu todentaa solmu on Tarkista Yleiset nimi Aihe-kenttään arvo. Tämä yleinen nimi tai jokin varmenteet aihe vaihtoehtoiset nimet on oltava sallittu yleisiä nimien luettelossa.

Seuraavassa artikkelissa kerrotaan, miten Luo varmenteet aihe vaihtoehtoinen nimillä (SAN): [Voit lisätä kohteen vaihtoehtoinen nimi suojattuun LDAP-sertifikaattiin](http://support.microsoft.com/kb/931351).

Subject-kenttä voi olla useita arvoja, kukin etuliite alustus osoittamaan arvon tyyppi. Useimmin alustus on kutsumanimi; "CN" esimerkiksi "CN = www.contoso.com". On myös mahdollista Subject-kenttä on tyhjä. Jos valinnainen aiheen vaihtoehtoinen nimi-kenttä lisätään, sen on oltava Yleinen nimi on varmenne ja yksi tapahtuma aiheen vaihtoehtoinen nimi. Nämä lisätään DNS-nimi-arvoina.

Varmenteen tarkoitettu varten-kenttään tulisi sisältää arvo, kuten "Palvelimen todennuksen" tai "Asiakkaan todentaminen".

### <a name="client-x509-certificates"></a>Asiakkaan X.509-varmenteita

Asiakas ei yleensä varmenteita muun toimittajan varmenteen myöntäjältä. Sen sijaan nykyisen käyttäjän sijainti Henkilökohtainen säilö sisältää yleensä asiakkaan varmenteiden päämyöntäjän, joiden tarkoitus "Asiakkaan todentaminen" tekemiä. Asiakas voi käyttää todistuksen, kun kaksisuuntainen käyttöoikeuksien tarvitaan.

>[AZURE.NOTE] Kaikki Service kangasta klusterissa hallintatoiminnot vaativat palvelimen varmenteet. Asiakkaan varmenteet ei voi käyttää hallintaa varten.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on perustietoja klusterin suojaus. Seuraavaksi [Resurssienhallinta mallin avulla Azure-klusterin luominen](service-fabric-cluster-creation-via-arm.md) tai palvelun [Azure portal](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
