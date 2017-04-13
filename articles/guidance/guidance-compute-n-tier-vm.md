<properties
   pageTitle="Käynnissä Windows VMs N-taso-arkkitehtuuri | Viittaus arkkitehtuuri | Microsoft Azure"
   description="Miten Toteuta monitasoisten arkkitehtuuri Azure-maksetuksi erityistä huomiota käytettävyyden, tietoturva, skaalattavuus ja hallittavuuden suojaus."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
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

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>Käynnissä Windows VMs Azure-N-taso-arkkitehtuuri

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Käynnissä Linux VMs Azure-N-taso-arkkitehtuuri](guidance-compute-n-tier-vm-linux.md)
- [Käynnissä Windows VMs Azure-N-taso-arkkitehtuuri](guidance-compute-n-tier-vm.md)

Tässä artikkelissa käsitellään osoitettu käytännöt käynnissä Windows näennäiskoneiden (VMs) joukko sovellukselle, N-taso-arkkitehtuuri. Se muodostaa [käynnissä useita VMs Azure-]artikkelissa[multi-vm].

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Tässä artikkelissa käytetään Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

On N tason arkkitehtuureihin variaatioita. Suurin osa suositukset tarkoitetaan ei kannata merkitystä erot. Tässä artikkelissa oletetaan, että tyypillinen 3 tason verkkosovellukseen:

- **Web-taso.** Käsittelee HTTP-pyynnöt. Vastaukset palautetaan – tämä taso.

- **Liiketoiminnan taso.** Ottaa käyttöön liiketoimintaprosessien ja muiden toimintojen logiikan järjestelmän.

- **Tietokannan taso.** Jatkuva tietojen tallennustila, [SQL Server aina käyttöön käytettävyys ryhmien] käyttäminen[ sql-alwayson] suuren.

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on käytössä "Laske - Monisivuinen taso (Windows).

![[0]][0]

- **Käytettävyys joukot.** Luo [Saatavuus] [ azure-availability-sets] kunkin taso ja valmistella vähintään kaksi VMs-kunkin tason. Tämän menetelmän tarvitaan saavuttamiseksi käytettävyys [SLA] [ vm-sla] VMs varten.

- **Aliverkosta.** Luo erillisen aliverkon kunkin tason. Määritä käyttämällä [CIDR] merkintätapaa alue- ja aliverkon peite. 

- **Kuormituksen tasaajiksi.** Käytä [Internetiin yhteydessä oleva kuormituksen] [ load-balancer-external] jakaa saapuvan Internet-liikenne paikalliseen web-tason ja [sisäinen kuormituksen] [ load-balancer-internal] jakaa verkkoliikennettä web-tason business taso.

- **Jumpbox**. _Jumpbox_, kutsutaan myös [bastion host]on verkossa, joiden avulla järjestelmänvalvojat muodostaa yhteyden muiden VMs AM. Jumpbox on NSG, joka sallii remote tietoliikenteen vain whitelisted julkiseen IP-osoitteet. Etätyöpöytä (RDP) tietoliikenteen olisi salliminen NSG.

- **Seuranta**. Seuranta-ohjelmistoa, kuten [Nagios], [Zabbix]tai [Icinga] voi antaa sinulle vastausajan, AM käytettävyyttä ja järjestelmän yleinen kunto. Seuranta-ohjelmiston asentaminen AM, joka sijoitetaan erillinen johto aliverkon.

- **NSGs**. Käytä [verkon käyttöoikeusryhmät] [ nsg] (NSGs) rajoittaa verkkoliikennettä VNet kuluessa. Esimerkiksi kuvassa 3 tason arkkitehtuuri, tietokannan tason ei hyväksyä web-edusta-vain business taso ja hallinta-aliverkon-tietoliikenteen.

- **SQL Server aina käyttöön käytettävyys-ryhmä.** Sisältää tiedot, taso hyvin saatavuudesta ottamalla replikointi ja automaattisesti.

- **Active Directory toimialueen palveluiden (AD DS)-palvelimiin**. Active Directory-toimialueen palveluista (AD DS) kansion tiedot tallennetaan, ja niitä hallitsee käyttäjät ja toimialueet, mukaan lukien käyttäjän kirjautumistili prosesseja, todennus ja directory hakujen välisen. Active Directory-toimialueen ohjauskoneen on palvelin, jossa on käytössä AD DS: ÄÄN. Ennen Windows Server 2016 aina käyttöön käytettävyys ryhmät on liitetty toimialueeseen. Tämä johtuu siitä käytettävyys ryhmät määräytyvät Windows Server automaattisesti klusterin (WSFC)-tekniikkaa. Windows Server 2016 esitellään voi luoda automaattisesti klusterin ilman Active Directorysta. Lisätietoja on artikkelissa [Vikasietoklusterointia Windows Server 2016 uudet ominaisuudet][wsfc-whats-new]

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten, ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="vnet--subnets"></a>VNet / aliverkosta

Kun luot VNet, varata aliverkosta, sinun on tarpeeksi osoitetilaa. Määritä VNet osoite alue- ja aliverkon rajoite käyttämällä [CIDR] -merkintätapaa. Osoite-tilaa Vakio [yksityinen IP-osoitelohkot]sisäpuolella olevat[private-ip-space], jotka ovat 10.0.0.0/8, 172.16.0.0/12 ja 192.168.0.0/16.

Valitse osoitealue, joka ei ole päällekkäinen paikallinen verkossa siltä varalta, sinun täytyy määrittää yhdyskäytävän välillä VNet ja paikallinen verkon myöhemmin. Kun olet luonut VNet, et voi muuttaa osoite-arvo.

Suunnittele aliverkosta ja suojausasetusten vaatimusten mielessä. Kaikki VMs samassa taso tai rooli olisi salliva asetus saman aliverkon, joka voi olla suojauksen reunaa. Katso lisätietoja suunnittelemisesta VNets ja aliverkosta [suunnitelma ja rakenteen Azure Virtual verkkojen][plan-network].

Määritä kunkin aliverkon CIDR merkintätavan aliverkon osoitetilaa. Esimerkiksi "10.0.0.0/24" Luo 256 IP-osoitteiden alueen. (VMs käyttää näitä 251, viisi on varattu. Katso [Virtual verkon usein kysytyt kysymykset][vnet faq].) Varmista, että osoitealueita ole päällekkäin aliverkosta yli.

### <a name="network-security-groups"></a>Verkon käyttöoikeusryhmät

NSG sääntöjen avulla voit rajoittaa liikenne tasojen välissä. Esimerkiksi 3 tason arkkitehtuuri yllä, valitse web-taso ei ole yhteyttä suoraan tietokannan taso. Voit käyttää tätä tietokannan taso olisi estä saapuvan liikenteen aliverkosta web taso.  

  1. Luo NSG ja liittää sen tietokannan taso aliverkon.

  2. Lisää sääntö, joka estää kaiken saapuvan liikenteen kohteesta VNet. (Käytä `VIRTUAL_NETWORK` tunnisteen säännössä.) 

  3. Lisää sääntö, joka sallii saapuvan liikenteen business taso-aliverkosta suuren prioriteetin. Tämä sääntö ohittaa aiemman sääntö ja avulla voit jutella tietokannan tason business-taso.

  4. Lisää sääntö, joka sallii saapuvan liikenteen tietokannan taso aliverkon itse sisällä. Tämän säännön avulla välisen tietokannan taso, joka tarvita tietokannan replikoiminen ja automaattisesti VMs.

  5. Lisää sääntö, joka sallii jumpbox aliverkosta RDP-liikenne. Tämän säännön avulla järjestelmänvalvojat voivat muodostaa yhteyden Tietokantataso jumpbox.

  > [AZURE.NOTE] NSG on [oletusarvoinen säännöt] [ nsg-rules] sisällä VNet-kaikki saapuvan liikenteen tukevat. Sääntöjen ei voi poistaa, mutta voit ohittaa luomalla tärkeämpään sääntöjä.

### <a name="load-balancers"></a>Kuormituksen tasaajiksi

Ulkoinen kuormituksen jakaa Internet-liikenne paikalliseen web-taso. Voit luoda tämän kuormituksen julkiseen IP-osoite. Katso [luominen Internetiin yhteydessä oleva-kuormituksen][lb-external-create].

Sisäinen kuormituksen jakaa verkkoliikennettä web-tason business taso. Jos haluat antaa tämän kuormituksen yksityinen IP-osoite, Luo edusta-IP-määritys ja liittäminen business tason aliverkon. Katso [luomisesta sisäinen kuormituksen][lb-internal-create].

### <a name="sql-server-always-on-availability-groups"></a>SQL Server aina käytettävyys ryhmät

On suositeltavaa [Aina käyttöön käytettävyys ryhmien] [ sql-alwayson] SQL Server hyvän käytettävyyden. Aina käyttöön käytettävyys ryhmät edellyttää toimialueen ohjauskoneen. Kaikki solmut käytettävyys-ryhmässä on oltava sama AD-toimialueen.

Muut tasoa kautta [käytettävyys ryhmän listener]-tietokantayhteyden muodostamisessa[sql-alwayson-listeners]. Kuuntelutoiminto mahdollistaa SQL-asiakassovellus muodostaa yhteyden ilman tietää fyysinen SQL Server-esiintymän nimi. VMs access tietokanta on liitetty toimialueeseen. (Kirjoita tässä tapauksessa toisen tason) asiakas käyttää DNS listener virtual verkkonimi ratkaista IP-osoitteiden tuominen.

Määrittää SQL Server aina seuraavasti:

- Windows Server-automaattisesti klusterointi (WSFC)-klusterin ja SQL Server aina käyttöön käytettävyys ryhmän luominen Lisätietoja on kohdassa [Getting Started ryhmiin aina käyttöön käytettävyys][sql-alwayson-getting-started].

- Luo sisäinen kuormituksen staattinen yksityinen IP-osoite.

- Luo käytettävyys-ryhmän listener ja Yhdistä listener DNS-nimi sisäinen kuormituksen IP-osoite. 

- Luo kuormituksen tasauspalvelun sääntö SQL Server-kumpaan (TCP portti 1433 oletusarvoisesti). Kuormituksen tasauspalvelun sääntö on otettava _irrallisen IP_, jota kutsutaan myös suoraan palvelin palauttaa. Tällöin voit vastata asiakas, joka mahdollistaa ensisijainen se suoraan yhteyden AM.

    > [AZURE.NOTE] Irrallinen IP on käytössä, edusta porttinumero on oltava sama kuin taustatietokantaan porttinumero kuormituksen tasauspalvelun säännön.

Kun SQL Clientin yrittää muodostaa yhteyden, kuormituksen reitittää yhteyden pyynnön se, joka on nykyisen ensisijainen. Jos toinen replika siirtyy automaattisesti kuormituksen reitittää myöhemmät automaattisesti uuden ensisijainen se. Lisätietoja on artikkelissa [Configure kuormituksen SQL aina][sql-alwayson-ilb].

Aikana automaattisesti aiemmin asiakasyhteydet on suljettu. Vikasietotilaa päätyttyä uusi ensisijainen se reititetään uudet yhteydet.

Jos sovellus on huomattavasti enemmän kuin kirjoittaa lukuja, voit purku jotkin toissijainen replikaan vain luku-kyselyt. Katso [kuuntelija avulla voit muodostaa yhteyden vain luku-toissijainen replikan (vain luku-reititys)][sql-alwayson-read-only-routing].

Testata käyttöönoton pakottamalla [Manuaalinen automaattisesti][sql-alwayson-force-failover].

### <a name="jumpbox"></a>Jumpbox

Salli RDP access julkisesta Internetistä VMs, jotka suoritetaan sovelluksen työmäärää. Sen sijaan kaikki nämä VMs RDP/SSH käyttöoikeus on annettava jumpbox kautta. Järjestelmänvalvoja kirjautuu-järjestelmään jumpbox ja kirjaa tuominen kohteesta jumpbox muiden AM. Jumpbox mahdollistaa RDP tietoliikenteen Internetissä, mutta vain tunnetut, whitelisted IP-osoitteet.

Aseta jumpbox saman VNet kuin muiden VMs, mutta erillinen johto aliverkon.

Luo [julkinen IP-osoite] jumpbox.

Käytä pieni AM koko jumpbox, kuten Standard A1.

Määrittää WWW-taso, business taso ja tietokannan taso aliverkosta NSGs sallimaan järjestelmänvalvojan (RDP) liikenteen hallinta-aliverkosta välitystä.

Voit suojata jumpbox-luominen NSG ja käytä sitä jumpbox aliverkon. Lisää NSG-sääntö, joka sallii RDP yhteydet vain julkiseen IP-osoitteiden whitelisted joukko.

NSG voidaan liittää tai aliverkon jumpbox NIC-sivustosta. Tässä tapauksessa on suositeltavaa liittäminen NIC, joten RDP liikenne sallitaan vain jumpbox, vaikka voit lisätä muita VMs saman aliverkon.

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Sijoittaa kunkin tason tai AM rooli erillisessä käytettävyys määrittäminen. Älä sijoittaa VMs-eri tasojen käytettävyys samat. 

Tietokannan, taso-palvelussa on useita VMs ei automaattisesti kääntää erittäin käytettävissä tietokantaan. Relaatiotietokannasta sinun yleensä on saavuttamiseksi suuren käytettävyyden replikoinnin ja automaattisesti avulla. SQL Serverin, on suositeltavaa [Aina käyttöön käytettävyys ryhmien]käyttäminen[sql-alwayson]. 

Jos sinun on suurempi kuin [Azure SLA varten VMs] käytettävyys[ vm-sla] tarjoaa, replikoida sovelluksen yli kummallakin alueella ja käyttää Azure liikenteen hallinta automaattisesti. Lisätietoja on artikkelissa [Käynnissä Windows VMs useita alueilla suuren][multi-dc].   

## <a name="security-considerations"></a>Suojausasiat

Salaa muut tiedot. Käytä [Azure avaimen säilö] [ azure-key-vault] voit hallita tietokannan salausavaimet. Avaimen säilö voit tallentaa salausavaimet laitteiston suojauksen moduulit (HSMs). Lisätietoja on artikkelissa [Määrittäminen Azure avaimen säilö integrointi SQL Server Azure VMs-] [ sql-keyvault] on myös suositeltavaa, voit tallentaa sovelluksen tietoja, kuten tietokannan yhteysmerkkijonoja avaimen säilö.

Lisäämällä verkon: laitteen virtual (NVA) voit luoda DMZ julkinen Internet- ja Azure virtual verkon välillä. NVA on Yleinen termi virtual laitteen sallitut toiminnot verkon liittyviä tehtäviä, esimerkiksi palomuuri, paketin tarkastus, seuranta, mukautettu reititys tai useisiin muihin toimintoihin. Lisätietoja on artikkelissa [käyttöönoton DMZ Azure ja Internetin välille][dmz].

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Kuormituksen tasoitusmääritykset Jaa verkkoliikenteen web- ja business-tasoa. AM uusia esiintymiä lisäämällä skaalata vaakasuunnassa. Huomaa, että voit skaalata web- ja business-tasoa itsenäisesti kuormituksen perusteella. Mahdollisia monimutkaisuuden aiheutuvat ei tarvitse säilyttää asiakkaan affiniteetti pienentämiseksi VMs-web-taso on oltava tilattomien. Liiketoimintalogiikan isännöinnin VMs tulisi olla tilattomien.

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Yksinkertaistaa koko järjestelmästä hallintaa käyttämällä keskitetty hallinta-työkaluja, kuten [Azure automaatio][azure-administration], [Microsoft toimintojen hallinta Suite][operations-management-suite], [kokki][chef], tai [Puppet][puppet]. Näillä työkaluilla voit yhdistää diagnostiikka- ja kuntotietojen useita VMs antamaan yleisnäkymä järjestelmä-kerätyistä tiedoista.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Viite-arkkitehtuuri, joka sisältää näitä suosituksia käyttöönoton on käytettävissä [Github][github-folder]. Viite-arkkitehtuuri sisältää web taso, business taso, tietojen taso sekä jumpbox AM ja Active Directoryn toimialueen ohjaimet.

Viite-arkkitehtuuri voidaan ottaa käyttöön noudattamalla seuraavia ohjeita: 

1. Napsauta alla olevaa painiketta.  
[! ["Käyttöön Azure"] [1]][2]

2. Kun linkki on avattu Azure-portaalissa, kirjoita seuraa: 
  - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-ntier-sql-network-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

3. Tarkista viestin käyttöönoton Azure portaalin ilmoitus on valmis.

4. Napsauta alla olevaa painiketta.  
[! ["Käyttöön Azure"] [1]][3]

5. Kun linkki on avattu Azure-portaalissa, kirjoita seuraa: 
  - **Resurssiryhmä** nimi on jo määritetty parametri-tiedosto, joten valitsemalla **Käytä aiemmin luotua** ja kirjoita `ra-ntier-sql-workload-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

6. Tarkista viestin käyttöönoton Azure portaalin ilmoitus on valmis.

7. Napsauta alla olevaa painiketta.  
[! ["Käyttöön Azure"] [1]][4]

8. Kun linkki on avattu Azure-portaalissa, kirjoita seuraa: 
  - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-ntier-sql-network-rg` tekstiruutuun.
  - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
  - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
  - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
  - Valitse **Osta** -painiketta.

9. Tarkista viestin käyttöönoton Azure portaalin ilmoitus on valmis.

10. Parametritiedostot ovat koodattu järjestelmänvalvojan käyttäjänimet ja salasanat ja se on erittäin suositeltavaa muuttaminen voit heti molempien kaikki VMs. Valitse kunkin AM Azure-portaalissa, valitse sitten Valitse **salasanan** **tuki + vianmääritys** -sivu. Valitse **salasanan** **tilassa** avattava luetteloruutu-ruutu ja valitse uusi **käyttäjänimi** ja **salasana**. Pysyvä uusi käyttäjänimi ja salasana valitsemalla **Päivitä** . 

Lisätietoja muitakin tapoja ottaa viite-arkkitehtuuri on artikkelissa [ohjeita single-AM] Lueminut-tiedosto[ github-folder] Github kansio. 

## <a name="next-steps"></a>Seuraavat vaiheet

Tavoitteet suuri käytettävyys viittaus-arkkitehtuuri, on suositeltavaa [käyttöönotto alueille][multi-dc].

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[bastion Host (isäntä)]: https://en.wikipedia.org/wiki/Bastion_host
[CIDR]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[julkinen IP-osoite]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "Käyttämällä Microsoft Azure N-taso-arkkitehtuuri"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json