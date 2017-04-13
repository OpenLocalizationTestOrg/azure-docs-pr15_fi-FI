<properties
   pageTitle="N-taso-arkkitehtuuri Linux VMs käynnissä Azure | Microsoft Azure"
   description="Voit suorittaa Linux VMs N tason Microsoft Azure-arkkitehtuuri."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-for-an-n-tier-architecture-on-azure"></a>Käynnissä Linux VMs Azure-N-taso-arkkitehtuuri

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]


> [AZURE.SELECTOR]
- [Käynnissä Linux VMs Azure-N-taso-arkkitehtuuri](guidance-compute-n-tier-vm-linux.md)
- [Käynnissä Windows VMs Azure-N-taso-arkkitehtuuri](guidance-compute-n-tier-vm.md)

Tässä artikkelissa käsitellään osoitettu käytännöt käynnissä Linux näennäiskoneiden (VMs) joukko sovellukselle, N-taso-arkkitehtuuri. Se muodostaa [käynnissä useita VMs Azure-]artikkelissa[multi-vm].

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Tässä artikkelissa käytetään Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

On N tason arkkitehtuureihin variaatioita. Suurin osa suositukset tarkoitetaan ei kannata merkitystä erot. Tässä artikkelissa oletetaan, että tyypillinen 3 tason verkkosovellukseen:

- **Web-taso.** Käsittelee HTTP-pyynnöt. Vastaukset palautetaan – tämä taso.

- **Liiketoiminnan taso.** Ottaa käyttöön liiketoimintaprosessien ja muiden toimintojen logiikan järjestelmän.

- **Tietokannan taso.** On jatkuva tietosäilö Apache Cassandra käytön suuren käytettävyyden.

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on käytössä "Laske - Monisivuinen taso (Linux).

![[0]][0]

- **Käytettävyys joukot.** Luo [Saatavuus] [ azure-availability-sets] kunkin taso ja valmistella vähintään kaksi VMs-kunkin tason. Tämän menetelmän tarvitaan saavuttamiseksi käytettävyys [SLA] [ vm-sla] VMs varten.

- **Aliverkosta.** Luo erillisen aliverkon kunkin tason. Määritä käyttämällä [CIDR] merkintätapaa alue- ja aliverkon peite. 

- **Kuormituksen tasaajiksi.** Käytä [Internetiin yhteydessä oleva kuormituksen] [ load-balancer-external] jakaa saapuvan Internet-liikenne paikalliseen web-tason ja [sisäinen kuormituksen] [ load-balancer-internal] jakaa verkkoliikennettä web-tason business taso.

- **Jumpbox**. _Jumpbox_, kutsutaan myös [bastion host]on verkossa, joiden avulla järjestelmänvalvojat muodostaa yhteyden muiden VMs AM. Jumpbox on NSG, joka sallii remote tietoliikenteen vain whitelisted julkiseen IP-osoitteet. Suojattu runko (SSH) tietoliikenteen olisi salliminen NSG.

- **Seuranta**. Seuranta-ohjelmistoa, kuten [Nagios], [Zabbix]tai [Icinga] voi antaa sinulle vastausajan, AM käytettävyyttä ja järjestelmän yleinen kunto. Seuranta-ohjelmiston asentaminen AM, joka sijoitetaan erillinen johto aliverkon.

- **NSGs**. Käytä [verkon käyttöoikeusryhmät] [ nsg] (NSGs) rajoittaa verkkoliikennettä VNet kuluessa. Esimerkiksi kuvassa 3 tason arkkitehtuuri, valitse tietokannan tason ei hyväksy web-edusta-vain business taso ja hallinta-aliverkon-tietoliikenteen.

- **Apache Cassandra tietokannan**. Sisältää tiedot, taso hyvin saatavuudesta ottamalla replikointi ja automaattisesti.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten, ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="vnet--subnets"></a>VNet / aliverkosta

Kun olet luonut VNet, varaaminen aliverkosta, sinun on tarpeeksi osoitetilaa. Määritä VNet osoite alue- ja aliverkon rajoite käyttämällä [CIDR] -merkintätapaa. Osoite-tilaa Vakio [yksityinen IP-osoitelohkot]sisäpuolella olevat[private-ip-space], jotka ovat 10.0.0.0/8, 172.16.0.0/12 ja 192.168.0.0/16.

Valitse osoitealue, joka ei ole päällekkäinen paikallinen verkossa siltä varalta, sinun täytyy määrittää yhdyskäytävän välillä VNet ja paikallinen verkon myöhemmin. Kun olet luonut VNet, et voi muuttaa osoite-arvo.

Suunnittele aliverkosta ja suojausasetusten vaatimusten mielessä. Kaikki VMs samassa taso tai rooli olisi salliva asetus saman aliverkon, joka voi olla suojauksen reunaa. Katso lisätietoja suunnittelemisesta VNets ja aliverkosta [suunnitelma ja rakenteen Azure Virtual verkkojen][plan-network].

Määritä kunkin aliverkon CIDR merkintätavan aliverkon osoitetilaa. Esimerkiksi "10.0.0.0/24" Luo 256 IP-osoitteiden alueen. (VMs käyttää näitä 251, viisi on varattu. Katso [Virtual verkon usein kysytyt kysymykset][vnet faq].) Varmista, että osoitealueita ole päällekkäin aliverkosta yli.

### <a name="load-balancers"></a>Kuormituksen tasaajiksi

Ulkoinen kuormituksen jakaa Internet-liikenne paikalliseen web-taso. Voit luoda tämän kuormituksen julkiseen IP-osoite. Katso [luominen Internetiin yhteydessä oleva-kuormituksen][lb-external-create].

Sisäinen kuormituksen jakaa verkkoliikennettä web-tason business taso. Jos haluat antaa tämän kuormituksen yksityinen IP-osoite, Luo edusta-IP-määritys ja liittäminen business tason aliverkon. Katso [luomisesta sisäinen kuormituksen][lb-internal-create].

### <a name="cassandra"></a>Cassandra

Suosittelemme, että [DataStax yrityksen] [ datastax] tuotannon käytössä, mutta näitä suosituksia käyttää Cassandra mikä tahansa versio. Katso lisätietoja suorittamisesta DataStax Azure [DataStax yrityksen Azure käyttöönotto-oppaassa][cassandra-in-azure]. 

Sijoita Cassandra klusterin VMs käytettävyys-joukko, varmistaaksesi, että Cassandra replikat jaetaan vika useita toimialueita ja päivittää toimialueet. Lisätietoja vian toimialueet ja Päivitä toimialueet-kohdassa [Manage näennäiskoneiden käytettävyyttä][availability-sets-manage]. 

Määritä 3 vika domains (enintään) käytettävyys joukossa. 

Määritä käytettävyys joukossa 18 päivityksen toimialueet. Näin päivityksen toimialuemäärän kuin voit edelleen jaetaan tasaisesti vika toimialueet.   

Määritä solmujen Teline-aware-tilassa. Vian toimialueiden yhdistäminen tavara `cassandra-rackdc.properties` tiedosto.

Sinun ei tarvitse kuormituksen klusterin eteen. Asiakas muodostaa suoraan klusterin solmu.

### <a name="jumpbox"></a>Jumpbox

Aseta jumpbox saman VNet kuin muiden VMs, mutta erillinen johto aliverkon.

Luo [julkinen IP-osoite] jumpbox.

Käytä pieni AM koko jumpbox, kuten Standard A1.

Määrittää WWW-taso, business taso ja tietokannan taso aliverkosta NSGs sallimaan järjestelmänvalvojan (SSH) liikenteen hallinta-aliverkosta välitystä.

Voit suojata jumpbox-luominen NSG ja käytä sitä jumpbox aliverkon. Lisää NSG-sääntö, joka sallii SSH yhteydet vain julkiseen IP-osoitteiden whitelisted joukko.

NSG voidaan liittää tai aliverkon jumpbox NIC-sivustosta. Tässä tapauksessa on suositeltavaa liittäminen NIC, joten SSH liikenne sallitaan vain jumpbox, vaikka voit lisätä muita VMs saman aliverkon.

Salli SSH access julkisesta Internetistä VMs, jotka suoritetaan sovelluksen työmäärää. Sen sijaan kaikki nämä VMs SSH käyttöoikeus on annettava jumpbox kautta. Järjestelmänvalvoja kirjautuu-järjestelmään jumpbox ja kirjaa tuominen kohteesta jumpbox muiden AM. Jumpbox mahdollistaa SSH tietoliikenteen Internetissä, mutta vain tunnetut, whitelisted IP-osoitteet.

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Sijoittaa kunkin tason tai AM rooli erillisessä käytettävyys määrittäminen. Älä sijoittaa VMs-eri tasojen käytettävyys samat. 

Tietokannan, taso-palvelussa on useita VMs ei automaattisesti kääntää erittäin käytettävissä tietokantaan. Relaatiotietokannasta sinun yleensä on saavuttamiseksi suuren käytettävyyden replikoinnin ja automaattisesti avulla.  

Jos sinun on suurempi kuin [Azure SLA varten VMs] käytettävyys[ vm-sla] tarjoaa, replikoida sovelluksen yli kummallakin alueella ja käyttää Azure liikenteen hallinta automaattisesti. Lisätietoja on artikkelissa [Käynnissä Linux VMs useita alueilla suuren][multi-dc].  

## <a name="security-considerations"></a>Suojausasiat

NSG sääntöjen avulla voit rajoittaa liikenne tasojen välissä. Esimerkiksi 3 tason arkkitehtuuri yllä, valitse web-taso ei ole yhteyttä suoraan tietokannan taso. Voit käyttää tätä tietokannan taso olisi estä saapuvan liikenteen aliverkosta web taso.  

  1. Luo NSG ja liittää sen tietokannan taso aliverkon.

  2. Lisää sääntö, joka estää kaiken saapuvan liikenteen kohteesta VNet. (Käytä `VIRTUAL_NETWORK` tunnisteen säännössä.) 

  3. Lisää sääntö, joka sallii saapuvan liikenteen business taso-aliverkosta suuren prioriteetin. Tämä sääntö ohittaa aiemman sääntö ja avulla voit jutella tietokannan tason business-taso.

  4. Lisää sääntö, joka sallii saapuvan liikenteen tietokannan taso aliverkon itse sisällä. Tämän säännön avulla välisen tietokannan taso, joka tarvita tietokannan replikoiminen ja automaattisesti VMs.

  5. Lisää sääntö, joka sallii SSH liikenne jumpbox aliverkosta. Tämän säännön avulla järjestelmänvalvojat voivat muodostaa yhteyden Tietokantataso jumpbox.

  > [AZURE.NOTE] NSG on [oletusarvoinen säännöt] [ nsg-rules] sisällä VNet-kaikki saapuvan liikenteen tukevat. Sääntöjen ei voi poistaa, mutta voit ohittaa luomalla tärkeämpään sääntöjä.

Lisäämällä verkon: laitteen virtual (NVA) voit luoda DMZ julkinen Internet- ja Azure virtual verkon välillä. NVA on Yleinen termi virtual laitteen sallitut toiminnot verkon liittyviä tehtäviä, kuten palomuurin paketin tarkastus, seuranta, mukautettu reititys ja useisiin muihin toimintoihin. Lisätietoja on artikkelissa [käyttöönoton DMZ Azure ja Internetin välille][dmz].

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Kuormituksen tasoitusmääritykset Jaa verkkoliikenteen web- ja business-tasoa. Lisäämällä uusia AM esiintymiä skaalata vaakasuunnassa. Huomaa, että voit skaalata web- ja business-tasoa itsenäisesti kuormituksen perusteella. Mahdollisia monimutkaisuuden aiheutuvat ei tarvitse säilyttää asiakkaan affiniteetti pienentämiseksi VMs-web-taso on oltava tilattomien. Liiketoimintalogiikan isännöinnin VMs tulisi olla tilattomien.

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Yksinkertaistaa koko järjestelmästä hallintaa käyttämällä keskitetty hallinta-työkaluja, kuten [Azure automaatio][azure-administration], [Microsoft toimintojen hallinta Suite][operations-management-suite], [kokki][chef], tai [Puppet][puppet]. Näillä työkaluilla voit yhdistää diagnostiikka- ja kuntotietojen useita VMs antamaan yleisnäkymä järjestelmä-kerätyistä tiedoista.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Viite-arkkitehtuuri, joka sisältää näitä suosituksia käyttöönoton on käytettävissä [Github][github-folder]. Viite-arkkitehtuuri sisältää web taso, business taso ja tietojen taso.

1. Napsauta alla olevaa painiketta.  
[! ["Käyttöön Azure"] [1]][2]

2. Kun linkki on avattu Azure-portaalissa, kirjoita seuraa: 
  - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-ntier-sql-network-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

3. Tarkista viestin käyttöönoton Azure portaalin ilmoitus on valmis.

4. Parametritiedostot ovat koodattu järjestelmänvalvojan käyttäjänimet ja salasanat ja se on erittäin suositeltavaa muuttaminen voit heti molempien kaikki VMs. Valitse kunkin AM Azure-portaalissa, valitse sitten Valitse **salasanan** **tuki + vianmääritys** -sivu. Valitse **salasanan** **tilassa** avattava luetteloruutu-ruutu ja valitse uusi **käyttäjänimi** ja **salasana**. Pysyvä uusi käyttäjänimi ja salasana valitsemalla **Päivitä** .

## <a name="next-steps"></a>Seuraavat vaiheet

Tavoitteet suuri käytettävyys viittaus-arkkitehtuuri, on suositeltavaa [käyttöönotto alueille][multi-dc].

<!-- links -->

[azure-administration]: ../automation/automation-intro.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[availability-sets-manage]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[bastion Host (isäntä)]: https://en.wikipedia.org/wiki/Bastion_host
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-consistency-usage]: http://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-in-azure]: https://docs.datastax.com/en/datastax_enterprise/4.5/datastax_enterprise/install/installAzure.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[datastax]: http://www.datastax.com/products/datastax-enterprise
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters-linux.md
[multi-vm]: guidance-compute-multi-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[julkinen IP-osoite]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[0]: ./media/blueprints/compute-n-tier-linux.png "Käyttämällä Microsoft Azure N-taso-arkkitehtuuri"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier%2Fazuredeploy.json


