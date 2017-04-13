<properties
   pageTitle="Suojatun hybrid verkko-arkkitehtuuri soveltamisesta Azure | Microsoft Azure"
   description="Voit toteuttaa suojatun hybrid verkko-arkkitehtuuri Azure-tietokannassa."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="implementing-a-dmz-between-azure-and-your-on-premises-datacenter"></a>Käyttöönoton DMZ Azure ja paikallisen palvelinkeskuksen välillä

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä, joka ulottuu paikalliseen verkkoon Azure suojatun hybrid verkoston. Viite-arkkitehtuuri toteuttaa DMZ paikalliseen verkkoon ja Azure virtual verkkoon käyttämällä käyttäjän määrittämät tiet (UDRs) välillä. Jos kyseessä on erittäin käytettävissä verkon virtual laitteiden (NVAs), jotka toteuttavat suojaus-toimintojen, kuten palomuurien ja paketin tarkastus. VNet lähtevän tietoliikenteen on voimassa tunneloidun Internet-yhteyden kautta paikalliseen verkkoon, joten se voi seurata. 

Tämä arkkitehtuuri vaatii yhteyden oman paikallisen palvelinkeskukseen, käyttämällä joko [VPN-yhdyskäytävän][ra-vpn], tai [ExpressRoute] [ ra-expressroute] yhteyden.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Viite-arkkitehtuuri käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Hybrid sovellukset missä työmääriä Suorita osittain paikallisen ja osittain tekstimuodossa Azure.

- Azure infrastruktuuri, joka reitittää liikenteen paikallisen ja Internetissä.

- Sovellusten valvonta lähtevän tietoliikenteen tarvitaan. Tämä on usein säädösten vaatimus useiden kaupallisen järjestelmien ja voit estää henkilötietojen julkistamista.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tärkeitä osia tämän arkkitehtuuri:

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on "DMZ – yksityinen-sivulla.

[! [0]][0]

- **Paikalliseen verkkoon.** Tämä on verkon tietokoneita ja otettu käyttöön organisaatiossa paikalliseen verkon kautta yksityisen yhdistettyjen laitteiden.

- **Azure virtual verkon (VNet).** VNet isännöi sovelluksen ja muita resursseja käynnissä pilveen.

- **Yhdyskäytävä.** Yhdyskäytävä on reitittimen paikallisen verkossa ja VNet välinen yhteys.

- **Verkon virtual laitteen (NVA).** NVA on Yleinen termi, joka kuvaa AM, hyväksyminen tai hylkääminen access palomuuri, optimoiminen WAN toiminnot (mukaan lukien verkon pakkaamisen), mukautetut reititys tai verkon muita toimintoja, kuten tehtävien suorittamista. 

- **Web-taso ja tietojen taso aliverkosta business taso.** Nämä ovat aliverkosta isännöinnin VMs ja palvelut, jotka toteuttavat Esimerkki 3 tason sovelluksen pilveen. Katso [N-taso-arkkitehtuuri-Azure Windows VMs käynnissä] [ ra-n-tier] lisätietoja.

- **Käyttäjän määrittämät tiet (UDR).** [Käyttäjän määrittämät tiet] [ udr-overview] Määritä IP-liikenne sisällä Azure VNets etenemisen. 

> [AZURE.NOTE] Sen mukaan, VPN-yhteyden vaatimukset voit määrittää reunan yhdyskäytävän erityisen tiet vaihtoehtona taaksepäin UDRs avulla voit toteuttaa edelleenlähetyssääntöjä, joka ohjaa liikenne paikalliseen verkkoon.

- **Hallinnan aliverkon.** Tämän aliverkon sisältää VMs, jotka toteuttavat hallinta ja valvontaan käynnissä VNet-komponentteja. 

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="rbac-recommendations"></a>RBAC suositukset

Voit luoda useita RBAC roolit resurssien sovelluksessa. Harkitse DevOps [mukautetun roolin] luominen[ rbac-custom-roles] hallintaoikeus infrastruktuuri-sovelluksen kanssa. Harkitse keskitetystä TIETOTEKNIIKAN järjestelmänvalvoja [Mukautettu rooli] luominen[ rbac-custom-roles] verkkoresursseja ja erillistä suojausta IT-järjestelmänvalvoja- [Mukautettu rooli] [ rbac-custom-roles] hallittavan suojatun verkkoresursseja, kuten NVAs. 

DevOps-roolin tulisi sisältää ottaa käyttöön sovelluksen osat sekä näyttö ja Käynnistä VMs käyttöoikeudet. Keskitetty IT-järjestelmänvalvojaroolin tulisi sisältää valvontaan verkkoresursseja oikeuksia. Kumpaakaan näistä roolit pitäisi olla NVA resurssien käytön, koska tämä on rajoitettava suojaus IT-järjestelmänvalvojan rooli.

### <a name="resource-group-recommendations"></a>Resurssien ryhmän suositukset

Azure resurssit, kuten VMs, VNets ja kuormituksen tasoitusmääritykset voit hallita helposti ryhmittelemällä ne yhteen resurssin ryhmiin. Voit määrittää roolit yläpuolella sitten kunkin resurssiryhmän rajoittamisesta.

On suositeltavaa luomisen seuraavasti:

- Resurssiryhmä, joka sisältää (lukuun ottamatta VMs) aliverkosta, NSGs ja yhteyden muodostaminen paikalliseen verkkoon yhdyskäytävän resurssit. Määritä tämä resurssiryhmä keskitetystä IT-järjestelmänvalvojan roolia.

- Resurssin ryhmäksi, joka sisältää VMs (mukaan lukien kuormituksen) NVAs, siirry-ruutu ja muut hallinta VMs ja yhdyskäytävän aliverkon, joka NVAs kaikki liikenne pakottaa UDR. Määritä suojaus IT-järjestelmänvalvojan rooli tämän resurssiryhmä.

- Erota sovelluksen kunkin tason resurssiryhmät, jotka sisältävät kuormituksen ja VMs. Huomaa, että tämä resurssiryhmä ei kannata sisältävät kunkin tason aliverkosta. Määritä tämä resurssiryhmä DevOps rooli.

### <a name="virtual-network-gateway-recommendations"></a>VPN yhdyskäytävän suositukset

Paikallisen liikenne välittää VNet VPN-yhdyskäytävän kautta. On suositeltavaa [Azure VPN-yhdyskäytävän] [ guidance-vpn-gateway] tai [Azure ExpressRoute yhdyskäytävän][guidance-expressroute]. 

### <a name="nva-recommendations"></a>NVA suositukset

NVAs tarjoamiseen eri verkkoliikennettä seuranta ja hallinta. Azure Marketplacesta tarjoaa useita kolmannen osapuolen fontteja NVAs, mukaan lukien:

- [Barracuda Web-sovelluksen palomuurin] [ barracuda-waf] ja [Barracuda NextGen palomuuri][barracuda-nf]

- [Koossa pysyviin verkkojen VNS3 palomuuri/reitittimen/VPN][vns3]

- [Fortinet virustorjuntapalomuurien-AM][fortinet]

- [SecureSphere Web-sovelluksen palomuuri][securesphere]

- [DenyAll Web-sovelluksen palomuuri][denyall]

- [Tarkista pisteen vSEC][checkpoint]

Jos mikään näistä kolmannen osapuolen NVAs ei vastaa tarpeitasi, voit luoda mukautetun NVA, käyttämällä VMs. Esimerkki luominen mukautetun NVAs on tämän viittaus-arkkitehtuuri, joka sisältää seuraavat ominaisuudet: DMZ:

- Liikenne reititetään käyttämällä [IP-osoitteen välityksen] [ ip-forwarding] NVA NIC.

- Liikenne sallitaan välitystä NVA vain, jos se on tarvittaessa. Kunkin NVA AM-viittaus-arkkitehtuuri on yksinkertainen Linux reitittimen saapuvan liikenteen saapuvat verkon käyttöliittymän *eth0*ja lähtevä liikenne sääntöjä määrittämiä mukautettuja komentosarjoja verkon käyttöliittymän *eth1*kautta lähetetty kanssa. 

- Liikenteen hallinta aliverkon reitittyy läpäise NVAs ja NVAs voidaan määrittää vain hallinta-aliverkosta. Jos liikenteen hallinta aliverkon tarvitaan reititetään kautta NVAs, ei reittiä hallinta aliverkon ja korjaa NVAs, jos ne olisi epäonnistuu.  

- Määrittää kuormituksen tasauspalvelun takana saatavuus sisältyvät VMs NVA varten. Valitse yhdyskäytävän aliverkon UDR ohjaa kuormituksen NVA pyynnöt.

Toisen suositus huomioitavia muodostaa yhteyttä kunkin NVA erityinen suojauksen tehtävän suorittamiseen useita NVAs sarjan. Tällöin kunkin suojaus-funktion avulla voidaan hallita NVA kohti välein. Esimerkiksi NVA käyttöönoton palomuuri saattaa sijoitettava sarja NVA tunnistetietojen Services-palvelun kanssa. Tarjoa hallinnan helpottamiseksi on ylimääräisiä verkon siirräntävälien, joka voi parantaa viive, joten varmista, että tämä ei vaikuta sovelluksen suorituskykyä.

### <a name="nsg-recommendations"></a>NSG suositukset

VPN-yhdyskäytävän paljastaa yhteys paikalliseen verkkoon julkiseen IP-osoite. Suosittelemme saapuvan NVA aliverkon yksityiskohtaiset säännöt haluat estää kaikki liikenne ei ole paikallisen verkosta peräisin luominen verkon käyttöoikeusryhmän (NSG).

Suosittelemme myös toteuttaa NSGs kunkin aliverkon toisen tason vastaan saapuvan liikenteen ohittaminen virheellisesti määritettyjä tai ei käytössä-NVA. Esimerkiksi viittaus-arkkitehtuuri web taso-aliverkon toteuttaa NSG säännön ohittamaan kaikki pyynnöt kuin paikalliseen verkkoon (192.168.0.0/16) tai VNet ja toisen säännön, joka ohittaa kaikki pyynnöt tekemättä porttiin 80 vastaanotetut. 

### <a name="internet-access-recommendations"></a>Internet-käytön suositukset

[Voimassa tunnelin] [ azure-forced-tunneling] kaikki lähtevät internet-liikenne paikalliseen sivusto sivusto VPN-tunnelin ja reitin yhteys Internetiin käyttämällä verkon kautta verkko-osoitteen tranlation (NAT). Tämä sekä estää vahingossa vesivuoto luottamuksellisia tietoja tallennettujen tietojen taso ja Salli tarkastus- ja kaikki lähtevän tietoliikenteen seuranta.

> [AZURE.NOTE] Internet-liikenne-web-, business- ja sovelluksen tasoa kokonaan ei estä. Jos nämä tasoa Azure PaaS services pääsemästä AM Diagnostiikan kirjaus julkiseen IP-osoitteet, lataa AM laajennukset ja muita toimintoja. Azure diagnostiikka edellyttää myös, että osien sähköpostiviestien lukeminen ja kirjoittaminen internet-riippuvaisten Azure-tallennustilan-tiliin.

Suosittelemme tarkemmin, tarkista internet-liikenteen on voimassa tunneloidun oikein. Jos käytät VPN-yhteyden [Reititys ja etäkäyttö access-palvelun] [ routing-and-remote-access-service] paikallisen palvelimessa, Käytä työkalua, kuten [Wiresharkissa] [ wireshark] tai [Microsoft viestin Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44226).

### <a name="management-subnet-recommendations"></a>Aliverkon suositukset

Hallinta-aliverkon sisältää tekstin ruutu, joka suorittaa hallinta ja seuranta toiminnot. Ottaa käyttöön seuraavat suositukset tekstin-ruutuun:
- Älä luo julkinen IP-osoite, siirry-ruutu. 
- Voit luoda yhden tekstin-ruutuun saapuvan yhdyskäytävän kautta ja toteuttavien NSG vastata vain pyynnöt sallittu reitityksestä hallinta-aliverkosta reitin.
- Rajoita suojatun hallintatehtävien suorittaminen tekstin-ruutuun.

### <a name="nva-recommendations"></a>NVA suositukset

Sisällytä kerroksen 7 NVA Lopeta sovelluksen yhteydet NVA tasolla ja ylläpitää affiniteetti Taustajärjestelmä tasoa. Tämä takaa symmetrisen yhteys, jossa vastauksen tietoliikenteen taustaan tasoa palauttaa NVA kautta.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Viite-arkkitehtuuri toteuttaa kuormituksen, ohjaavat paikallisen verkkoliikenteen NVA laitteiden resurssivarantoon. Kuten edellä kerrottiin NVA laitteita ovat VMs suoritetaan verkon liikenne Reitityssääntöjen ja otetaan huomioon [saatavuus][availability-set]. Tämän rakenteen avulla voit seurata NVAs siirtonopeuden ajan kuluessa ja lisää NVA laitteita kasvaa kuormituksen saatuaan.

Vakio SKU VPN-yhdyskäytävän tukee enintään 100 Mbps kestävä siirtonopeuden. Suuri suorituskyvyn tuote on enintään 200 Mbps. Harkitse suurempi kaistanleveyksien ExpressRoute yhdyskäytävän päivittäminen. ExpressRoute avulla enintään 10 Gbps kaistanleveyden pienempi kuin VPN-yhteyden viiveen.

> [AZURE.NOTE] [Käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN] artikkelit[ guidance-vpn-gateway] ja [toteuttaminen hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute] [ guidance-expressroute] kuvaavat ongelmat ympärillä skaalattavuus Azure yhdyskäytävää.

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Viite-arkkitehtuuri toteuttaa kuormituksen, jakaminen pyyntöihin paikallisen NVA, mikä Azure resurssivarantoon. NVA laitteita ovat VMs suoritetaan verkon liikenne Reitityssääntöjen ja otetaan huomioon [saatavuus][availability-set]. Kuormituksen kyselyt kunto-näytteenottimen toteutettu kunkin NVA ja poistaa kaikki ei vastaa NVAs olevilla säännöllisesti. 

Jos käytät Azure ExpressRoute antamaan VNet ja paikallisen verkon [määrittäminen antamaan automaattisesti VPN-yhdyskäytävän] väliset yhteydet[ guidance-vpn-failover] Jos ExpressRoute-yhteyttä ei ole käytettävissä.

Lisätietoja ylläpito VPN-ja ExpressRoute käytettävyys on artikkeleissa [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN] [ guidance-vpn-gateway] ja [toteuttaminen hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute][guidance-expressroute].

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Kaikki sovelluksen ja resurssien valvonta pitäisi tehdä hallinta aliverkon tekstin-ruutuun. Sovelluksen tarpeiden mukaan voit joutua Lisää seuranta lisäresursseja hallinta aliverkon, mutta uudelleen jokin näistä lisäresursseja kannattaa käyttää kautta tekstin-ruutuun.

Yhdyskäytävän yhteys paikalliseen verkosta Azure ei ole käytettävissä, jos olet edelleen laskentataulukon tekstin-ruutuun ottamalla PIP, lisäät sen Siirry-ruutuun ja valitse Etäpalvelujen Internetistä.

Kunkin tason aliverkon-viittaus-arkkitehtuuri on suojattu NSG säännöt ja voi olla tarpeen luoda säännön, joka avaa RDP-tila käyttöön Windows VMs portti 3389 tai Linux VMs SSH käyttöön portti 22. Muut hallinta ja valvontatyökaluja voi vaatia avata muita portit säännöt.

Jos käytät ExpressRoute antamaan paikallisen palvelinkeskukseen ja Azure väliset yhteydet, käytä [Azure Connectivity työkalujen (AzureCT)] [ azurect] ja yhteysongelmien vianmääritys.

> [AZURE.NOTE] Voit etsiä lisätietoja pyritään erityisesti seuranta ja hallinta VPN- ja ExpressRoute on artikkeleissa [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN] -yhteydet[ guidance-vpn-gateway] ja [toteuttaminen hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute][guidance-expressroute].

## <a name="security-considerations"></a>Suojausasiat

Viite-arkkitehtuuri toteuttaa useita tietoturvatasot: 

### <a name="routing-all-on-premises-user-requests-through-the-nva"></a>Kaikki paikallisen käyttäjäpyynnöt kautta NVA reititys

Yhdyskäytävän aliverkon-UDR estää kaikki muut kuin vastaanotti paikallisen käyttäjäpyynnöt. UDR siirtoja sallittu NVAs pyynnöt yksityinen DMZ aliverkon ja nämä pyynnöt välitetään sovellukseen, jos he saavat NVA sääntöjen mukaan. Muut tiet UDR voidaan lisätä, mutta varmista niitä ei vahingossa ohittaa NVAs tai estä järjestelmänvalvojan liikenteen hallinta aliverkon tarkoitettu.

Kuormituksen edessä NVAs toimii myös ohittaa liikenne, jotka eivät ole avoinna säännöt kuormituksen suojaus-laite. Viite-arkkitehtuuri kuormituksen-tasoitusmääritykset kuuntelevat vain pyyntöjen porttiin 80 ja HTTPS-pyyntöjä porttiin 443. Mikä tahansa lisäsääntöjä kuormituksen tasoitusmääritykset lisätään on kuvattu ja liikenne olisi seurattava varmistamiseksi suojausta ei ole ongelmia.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Estä/pass liikenne sovelluksen tasojen välissä NSGs avulla

Kunkin web ja business-tasoa rajoittaa niiden välinen tietoliikenne NSGs avulla. Eli business tason käyttää NSG estää kaiken tietoliikenteen, joka ei peräisin web-taso ja tietojen tason käyttää NSG estää kaiken tietoliikenteen, joka ei peräisin business taso. Jos sinulla on pyyntö Laajenna NSG sääntöjä laajempi käyttää näitä tasoa, arvioida näitä vaatimuksia tietoturvauhkia vastaan. Kunkin uuden saapuvan käytävän edustaa mahdollisuuden vahingossa tai purposeful tietojen vuoto tai sovelluksen vioittuneet.

### <a name="devops-access"></a>DevOps käyttö

Rajoita toiminnoista, joita DevOps suorittaa kunkin tason käyttämällä [RBAC] [ rbac] käyttöoikeuksien hallintaa. Kun käyttöoikeuksien myöntämistä käyttää [käyttöoikeuksien myöntäminen tarpeen mukaan][security-principle-of-least-privilege]. Kirjaudu sisään järjestelmänvalvojan kaikki toiminnot ja suorittaa säännöllisesti auditointeja, jotta muutoksia määritys on suunniteltu.

> [AZURE.NOTE] Laajemmat tietoja, esimerkkejä ja hallitsemisesta verkon arvopaperille, jonka Azure-skenaariot on artikkelissa [Microsoftin pilvipalveluihin ja verkkosuojauksen][cloud-services-network-security]. Lisätietoja suojaaminen resurssien pilveen ohjeaiheessa [käytön aloittaminen Microsoft Azure suojauksen][getting-started-with-azure-security]. Lisätietoja-osoitteiden suojaus koskee yli Azure yhdyskäytävä on yhteys on [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN] [ guidance-vpn-gateway] ja [toteuttaminen hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute][guidance-expressroute].

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Viite-arkkitehtuuri, joka sisältää näitä suosituksia käyttöönoton on käytettävissä Github. Viite-arkkitehtuuri sisältää virtual verkon (VNet), verkko-käyttöoikeusryhmän (NSG), kuormituksen ja kaksi näennäiskoneiden (VMs).

Viite-arkkitehtuuri voi asentaa Windows-tai Linux VMs alla olevia ohjeita noudattamalla: 

1. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet%2Fazuredeploy.json)

2. Kun linkki on avattu Azure-portaalissa, syötä arvot, joitakin asetuksia: 
    - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-private-dmz-rg` tekstiruutuun.
    - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
    - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
    - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
    - Valitse **Osta** -painiketta.

3. Odota suorittamiseen käyttöönottoa varten.

4. Kaikki VMs parametritiedostot ovat koodattu järjestelmänvalvojan käyttäjänimi ja salasana ja on erittäin suositeltavaa muuttaminen voit heti molemmat. Ympäristön kunkin AM, valitse Azure-portaalissa ja valitse sitten Valitse **salasanan** **tuki + vianmääritys** -sivu. Valitse **salasanan** **tilassa** avattava luetteloruutu-ruutu ja valitse uusi **käyttäjänimi** ja **salasana**. Pysyvä **Päivitä** -painiketta.

## <a name="next-steps"></a>Seuraavat vaiheet

- Opi toteuttamaan [DMZ Azure ja Internetin välillä](./guidance-iaas-ra-secure-vnet-dmz.md).
- Tietoa ottamisesta käyttöön [laajasti käytettävissä hybrid verkoston arkkitehtuuri](./guidance-hybrid-network-expressroute-vpn-failover.md).

<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-create-availability-set.md
[azurect]: https://github.com/Azure/NetworkMonitoring/tree/master/AzureCT
[azure-forced-tunneling]: https://azure.microsoft.com/en-gb/documentation/articles/vpn-gateway-forced-tunneling-rm/
[barracuda-nf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/barracuda-ng-firewall/
[barracuda-waf]: https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf/
[checkpoint]: https://azure.microsoft.com/marketplace/partners/checkpoint/check-point-r77-10/
[cloud-services-network-security]: https://azure.microsoft.com/documentation/articles/best-practices-network-security/
[denyall]: https://azure.microsoft.com/marketplace/partners/denyall/denyall-web-application-firewall/
[fortinet]: https://azure.microsoft.com/marketplace/partners/fortinet/fortinet-fortigate-singlevmfortigate-singlevm/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[guidance-expressroute]: ./guidance-hybrid-network-expressroute.md
[guidance-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[ip-forwarding]: ../virtual-network/virtual-networks-udr-overview.md#IP-forwarding
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[ra-n-tier]: ./guidance-compute-n-tier-vm.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[rbac]: ../active-directory/role-based-access-control-configure.md
[rbac-custom-roles]: ../active-directory/role-based-access-control-custom-roles.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[routing-and-remote-access-service]: https://technet.microsoft.com/library/dd469790(v=ws.11).aspx
[securesphere]: https://azure.microsoft.com/marketplace/partners/imperva/securesphere-waf-for-azr/
[security-principle-of-least-privilege]: https://msdn.microsoft.com/library/hdb58b2f(v=vs.110).aspx#Anchor_1
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vns3]: https://azure.microsoft.com/marketplace/partners/cohesive/cohesiveft-vns3-for-azure/
[wireshark]: https://www.wireshark.org/
[0]: ./media/blueprints/hybrid-network-secure-vnet.png "Suojattu hybrid verkoston arkkitehtuuri"
