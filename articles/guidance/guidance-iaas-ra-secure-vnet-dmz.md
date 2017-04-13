<properties
   pageTitle="Azure arkkitehtuuri viittaus - IaaS: Käyttöönoton DMZ Azure ja Internetin välillä | Microsoft Azure"
   description="Miten toteuttavien Azure suojatun hybrid-verkoston arkkitehtuuri Internet-yhteyden."
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

# <a name="implementing-a-dmz-between-azure-and-the-internet"></a>DMZ Azure ja toteuttaminen

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä, joka ulottuu paikallista verkkoa, joka hyväksyy Azure verkosta internet-tietoliikenteen suojatun hybrid verkoston. Viittaus-arkkitehtuuri käytössäsi on kuvattu artikkelissa [käyttöönoton DMZ Azure ja paikallisen palvelinkeskuksen]arkkitehtuuri[implementing-a-secure-hybrid-network-architecture]. On suositeltavaa, että asiakirjan lukeminen ja ymmärtäminen, jotka viittaavat arkkitehtuuri ennen tämän asiakirjan lukeminen.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Viite-arkkitehtuuri käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa. 

Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Hybrid sovellukset missä työmääriä Suorita osittain paikallisen ja osittain tekstimuodossa Azure.

- Azure infrastruktuuri, joka reitittää liikenteen paikallisen ja Internetissä.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tämän arkkitehtuuri tärkeitä osat:

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on "DMZ – julkisen-sivulla.

[! [0]][0]

- **Julkiseen IP-osoite (PIP).** Tämä on julkinen päätepisteen IP-osoite. Ulkoisten käyttäjien yhteydessä Internetiin, voit käyttää järjestelmän osoitteen kautta.

- **Verkon virtual laitteen (NVA).**  NVA on Yleinen termi, joka kuvaa AM, hyväksyminen tai hylkääminen access palomuuri, optimoiminen WAN toiminnot (mukaan lukien verkon pakkaamisen), mukautetut reititys tai verkon muita toimintoja, kuten tehtävien suorittamista.

- **Azure kuormituksen.** Kaikki pyynnöt Internetistä tämän kuormituksen läpi ja jaetaan julkisen DMZ NVAs saapuvien aliverkon.

- **Yleisen verkon saapuva aliverkon.** Tämän aliverkon hyväksyy Azure kuormituksen pyynnöt. Pyynnöt siirretään johonkin DMZ-NVAs.

- **Yleisen verkon lähtevä aliverkon.** Pyynnöt, joka on hyväksynyt NVA välittää – tämä aliverkon sisäinen kuormituksen web-tason.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="nva-recommendations"></a>NVA suositukset

Ota käyttöön joukkona NVAs peräisin Internetiin ja toinen tietoliikenteen peräisin paikallisen tietoliikenteen. Kannattaa käyttää vain yhden joukon NVAs sekä, koska tämä rakenne on verkkoliikennettä kahdet välillä ei ole suojaus ympäröivän tietoturvariskin. Se etu, voit käyttää tätä rakennetta, koska se vähentää monimutkaisuutta suojauksen tarkistussäännöt ja helpottaa selkeä mitä vastaavat kunkin saapuvan verkko-pyynnön. Esimerkiksi NVAs joukkona toteuttaa internet-liikenteen vain toisen sääntöryhmän NVAs toteuttaminen vain paikallisen tietoliikenteen aikana.

Sisällytä kerroksen 7 NVA Lopeta sovelluksen yhteydet NVA tasolla ja ylläpitää affiniteetti Taustajärjestelmä tasoa. Tämä takaa symmetrisen yhteys, jossa vastauksen tietoliikenteen taustaan tasoa palauttaa NVA kautta.  

### <a name="public-load-balancer-recommendations"></a>Julkinen kuormituksen tasauspalvelun suositukset ###

Pitämään skaalattavuus ja käytettävyys käyttöön julkinen DMZ saapuva NVAs [saatavuus] [ availability-set] ja käyttää [internet vastakkaisten kuormituksen] [ load-balancer] voit jakaa NVAs käytettävyys määrittäminen internet-pyynnöt.  

Määritä kuormituksen Hyväksy pyyntöjä vain internet-liikenne edellyttämistä porteista. Rajoita esimerkiksi saapuvien pyyntöjen porttia 80 ja saapuvia HTTPS-pyyntöjä porttiin 443.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Suunnittele infrastruktuurin selainpohjainen vastakkaisten kuormituksen saapuvien julkisen DMZ-aliverkon, valitse sen eteen. Vaikka arkkitehtuuri initally vaatii yhden NVA, se on helpompi Skaalaa useita NVAs myöhemmin Jos vastakkaisten kuormituksen internet on jo otettu käyttöön.

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Vastakkaisten kuormituksen internet vaatii julkisen DMZ kunkin NVA saapuvien aliverkon toteuttamisesta [kunto näytteenottimen][lb-probe]. Kuntotietojen näytteenottimen, lakkaa vastaamasta tämän päätepisteen pidetään ei ole käytettävissä, ja kuormituksen suora pyynnöt muiden NVAs käytettävyys samat. Huomaa, että jos kaikki NVAs eivät vastaa-sovelluksen epäonnistuu, joten on tärkeää on seuranta määritetty ilmoitusten DevOps, kun kunnossa NVA kerrat raja-arvon.

## <a name="manageability-considerations"></a>Hallittavuutta huomioon otettavia seikkoja

Rajaa saapuvien julkisen DMZ NVA's vastaamaan pyyntöihin hallinta aliverkon vain tekstin-ruudusta valvonnan ja hallinnan toiminnot. Kuten edellä [käyttöönoton DMZ Azure ja paikallisen palvelinkeskuksen] [ implementing-a-secure-hybrid-network-architecture] asiakirjassa, Määritä yhden verkoston reitin paikallisen tekstin-ruutuun yhdyskäytävän kautta verkosta käytön hallinta-aliverkon.

Yhdyskäytävän yhteys paikalliseen verkosta Azure ei ole käytettävissä, jos olet edelleen laskentataulukon tekstin-ruutuun ottamalla PIP, lisäät sen Siirry-ruutuun ja valitse Etäpalvelujen Internetistä.

## <a name="security-considerations"></a>Suojausasiat

Viite-arkkitehtuuri toteuttaa useita tietoturvatasot:

- Vastakkaisten kuormituksen internet ohjaa NVAs pyynnöt saapuvien julkisen DMZ aliverkon vain ja vain soveltamisen edellyttämistä porteista.

- Saapuvan ja lähtevän liikenteen julkiseen DMZ aliverkon NSG säännöt estää NVAs tartunnan estämällä pyynnöt, jotka eivät kuulu NSG säännöt.

- NAT reititys määrityskohde NVAs ohjaa pyynnöt portti 80 ja porttiin 443, web-tason kuormituksen, mutta ohittaa kaikki muut porttien pyynnöt.

Huomaa, että kirjaudut olisi kaikki pyynnöt kaikki porttiin. Valvo lokit pyynnöt, jotka eivät kuulu odotettu parametrit kuin ne voivat olla merkkinä siitä tunkeutumisen yritykset huomiota säännöllisesti.

### <a name="using-nsgs-to-blockpass-traffic-between-application-tiers"></a>Estä/pass liikenne sovelluksen tasojen välissä NSGs avulla

Kunkin web ja business-tasoa rajoittaa niiden välinen tietoliikenne NSGs avulla. Eli business tason käyttää NSG estää kaiken tietoliikenteen, joka ei peräisin web-taso ja tietojen tason käyttää NSG estää kaiken tietoliikenteen, joka ei peräisin business taso. Jos sinulla on pyyntö Laajenna NSG sääntöjä laajempi käyttää näitä tasoa, arvioida näitä vaatimuksia tietoturvauhkia vastaan. Kunkin uuden saapuvan käytävän edustaa mahdollisuuden vahingossa tai purposeful tietojen vuoto tai sovelluksen vioittuneet.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Viite-arkkitehtuuri, joka sisältää näitä suosituksia käyttöönoton on käytettävissä Github. Viite-arkkitehtuuri sisältää virtual verkon (VNet), verkko-käyttöoikeusryhmän (NSG), kuormituksen ja kaksi näennäiskoneiden (VMs).

Viite-arkkitehtuuri voi asentaa Windows-tai Linux VMs alla olevia ohjeita noudattamalla: 

1. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2FvirtualNetwork.azuredeploy.json)

2. Kun linkki on avattu Azure-portaalissa, syötä arvot, joitakin asetuksia: 
    - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-public-dmz-network-rg` tekstiruutuun.
    - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
    - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
    - Valitse avattavasta valikosta **windows** -ruutuun tai **linux** **Os tyyppi** .
    - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
    - Valitse **Osta** -painiketta.

3. Odota suorittamiseen käyttöönottoa varten.

4. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fworkload.azuredeploy.json)

5. Kun linkki on avattu Azure-portaalissa, syötä arvot, joitakin asetuksia: 
    - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-public-dmz-wl-rg` tekstiruutuun.
    - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
    - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
    - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
    - Valitse **Osta** -painiketta.

6. Odota suorittamiseen käyttöönottoa varten.

7. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-secure-vnet-dmz%2Fsecurity.azuredeploy.json)

8. Kun linkki on avattu Azure-portaalissa, syötä arvot, joitakin asetuksia: 
    - **Resurssiryhmä** nimi on jo määritetty parametri-tiedosto, joten valitsemalla **Käytä aiemmin luotua** ja kirjoita `ra-public-dmz-network-rg` tekstiruutuun.
    - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
    - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
    - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
    - Valitse **Osta** -painiketta.

9. Odota suorittamiseen käyttöönottoa varten.

10. Kaikki VMs parametritiedostot ovat koodattu järjestelmänvalvojan käyttäjänimi ja salasana ja on erittäin suositeltavaa muuttaminen voit heti molemmat. Ympäristön kunkin AM, valitse Azure-portaalissa ja valitse sitten Valitse **salasanan** **tuki + vianmääritys** -sivu. Valitse **salasanan** **tilassa** avattava luetteloruutu-ruutu ja valitse uusi **käyttäjänimi** ja **salasana**. Pysyvä **Päivitä** -painiketta.


<!-- links -->

[availability-set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[guidance-vpn-gateway]: ./guidance-hybrid-network-vpn.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-3-tier-vm.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[iptables]: https://help.ubuntu.com/community/IptablesHowTo
[lb-probe]: ../load-balancer/load-balancer-custom-probe-overview.md
[load-balancer]: ../load-balancer/load-balancer-internet-overview.md
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[ra-vpn]: ./guidance-hybrid-network-vpn.md
[ra-expressroute]: ./guidance-hybrid-network-expressroute.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vpn-failover]: ./guidance-hybrid-network-expressroute-vpn-failover.md
[0]: ./media/blueprints/hybrid-network-secure-vnet-dmz.png "Suojattu hybrid verkoston arkkitehtuuri"
