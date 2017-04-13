<properties
   pageTitle="Käynnissä useita VMs | Viittaus arkkitehtuuri | Microsoft Azure"
   description="Voit suorittaa useita AM esiintymiä Azure skaalattavuus, vikasietoisuudesta, hallinta ja suojaus."
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
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Useita VMs käytössä Azure skaalattavuus ja käytettävyys 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa käsitellään joukko osoitettu käytännöt useita näennäiskoneiden (AM) kopioiden parantamiseksi skaalattavuus, käytettävyyttä, hallinta ja suojaus.   

Tämä arkkitehtuuri työmäärää jaetaan AM esiintymät. On yksittäinen julkiseen IP-osoite ja Internet-liikenne jaetaan käyttämällä kuormituksen tasauspalvelun VMs. Tämä arkkitehtuuri voidaan yhden tason-sovelluksen, kuten tilattomien web Appista tai tallennustilan klusterin. Kannattaa myös N tason sovellusten rakenneosan. 

Tässä artikkelissa muodostaa [Yhden AM Azure-]suorittamisesta[single vm]. Artikkelin suositukset koskevat myös tämän arkkitehtuuri.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

VMs Azure edellyttävät tukevat verkko- ja resursseja.

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on käytössä "Laske - usean AM välilehden." 

![[0]][0]

Arkkitehtuuri on seuraavat osat: 

- **Määritä käytettävyys.** [Saatavuus] [ availability set] sisältää VMs ja tarvitaan tukemaan [Azure VMs SLA käytettävyys][vm-sla].

- **VNet**. Jokaisen AM Azure-tietokannassa on otettu käyttöön, jotka on jaettu edelleen **aliverkosta**virtual verkostoon (VNet).

- **Azure kuormituksen.** [Kuormituksen] jakaa AM esiintymissä käytettävyys määrittäminen Internet-pyynnöt. Kuormituksen sisältää joitakin liittyvät resurssit:

  - **Julkiseen IP-osoite.** Julkinen IP-osoite on tarvittavat kuormituksen vastaanottamaan Internet-liikenne.

  - **Edusta-määrityksen.** Liittää kuormituksen julkiseen IP-osoitteeseen.

  - **Taustatietokannan osoite resurssivarantoon.** Sisältää verkkoliittymät (NIC), joka vastaanottaa saapuvan liikenteen VMs.

- **Lataa tasaustoiminto säännöt.** Käyttää jakamiseen verkkoliikennettä kaikki taustatietokantaan osoite resurssivarantoon VMs kesken. 

- **NAT säännöt.** Käytetään reitti-liikenne paikalliseen tietyn AM. Esimerkiksi sallimiseksi työpöydän (RDP remote protocol) VMs voit luoda kunkin AM erillinen verkko osoite muuntaminen (NAT) säännön. 

- **Verkkoliittymät (NIC)**. Kunkin AM on NIC, muodosta yhteys verkkoon.

- **Tallennustilan.** Tallennustilan tilit pidä AM-kuvia ja muita tiedostoon liittyvät resurssit, kuten AM diagnostiikkatiedot Azure: llä.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa alla kuvattujen suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten ellei sinulla ole tietyn vaatimus suositus tue. 

### <a name="availability-set-recommendations"></a>Käytettävyys määrittää suositukset

Sinun on luotava vähintään kaksi VMs määritetty tukemaan [Azure VMs SLA käytettävyys]käytettävyys[vm-sla]. Huomaa, että Azure kuormituksen vaativat myös, että kuormituksen VMs kuuluvat käytettävyys samat.

### <a name="network-recommendations"></a>Verkon koskevia suosituksia

Kuormituksen takana VMs olisi kaikki asetettava saman aliverkon. Eivät Näytä VMs yhteys Internetiin, mutta sen sijaan avulla kunkin AM yksityinen IP-osoite. Asiakastietokoneet muodostavat yhteyden kuormituksen julkiseen IP-osoitetta.

### <a name="load-balancer-recommendations"></a>Kuormituksen tasauspalvelun suositukset

Lisää kaikki VMs käytettävyys määrittää kuormituksen taustatietokantaan osoite-ryhmään.

Määritä kuormituksen tasauspalvelun säännöt VMs suoraan verkkoliikenteen. HTTP-liikenne käyttöön Luo sääntö, joka yhdistää portti 80 edusta määrityksestä porttia 80-taustatietokannan osoite resurssivarantoon. Kun kuormituksen saa pyynnön julkisen IP-osoitteen porttiin 80, se reitittää pyynnön 80 johonkin taustatietokantaan osoite varannon NIC-porttiin.

Määritä NAT säännöt reitti-liikenne paikalliseen tietyn AM. Esimerkiksi käyttöön RDP VMs luoda erilliset NAT säännön kunkin AM. Niiden sääntöjen olisi Yhdistä eri porttinumero portti 3389 RDP oletusportti. (Esimerkiksi käyttävät porttia 50001 "VM1", "VM2" portti 50002 jne.) Määritä NIC VMs-NAT-sääntöjä. 

### <a name="storage-account-recommendations"></a>Tallennustilan tilin suositukset

Luo erillisen Azure tallennustilan tilit kunkin AM pitoon näennäiskiintolevyjen (näennäiskiintolevyjen), jotta vältät pallolla kohti toisen [(IOPS) rajoitukset] i ja-toimintojen[ vm-disk-limits] tallennustilan tileissä. 

Luoda yksi tallennustilan asiakkaan vianmäärityslokit. Tallennustilan tilin voidaan jakaa kaikki VMs.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Liittyy laajentaminen VMs Azure kahdella tavalla: 

- Kuormituksen tasauspalvelun avulla voit jakaa VMs joukko verkkoliikennettä. Skaalata ulos, Lisää VMs valmistelu ja vieminen taakse kuormituksen. 

- Käytä [virtuaalikoneen asteikko joukot][vmss]. Skaalaa-joukko sisältää samanlaisia VMs takana kuormituksen speficied määrä. AM mittakaava määrittää tuki automaattisen skaalauksen poistaminen suorituskyvyn mittarit perusteella. Kun VMs kuormitus kasvaa, Lisää VMs lisätään automaattisesti kuormituksen. 

Seuraavien osien vertaa näistä vaihtoehdoista.

### <a name="load-balancer-without-vm-scale-sets"></a>Kuormituksen ilman AM asteikko joukot

Kuormituksen tasauspalvelun tulee verkon pyynnöt ja jakaa niitä NIC taustatietokantaan osoite sarjassa. Skaalaa vaakasuunnassa, Lisää käytettävyys AM esiintymiä (tai poistaa varauksen VMs skaalata). 

Oletetaan esimerkiksi, että käytössäsi on verkkopalvelimeen. Lisää kuormituksen tasauspalvelun säännön portti 80 ja/tai porttiin 443 (for SSL: ää). Kun asiakas lähettää HTTP-pyyntö, kuormituksen valitsee käyttämällä [hajautuksessa algoritmin] taustatietokantaan IP-osoite[ load balancer hashing] , joka sisältää lähde-IP-osoite. Tällä tavoin kaikki VMs jakautuu asiakkaan pyyntöihin. 

> [AZURE.TIP] Kun lisäät uuden AM saatavuus määrittäminen, varmista, että voit luoda NIC AM ja lisää Verkkokortti kuormituksen taustatietokantaan osoite-ryhmään. Internet-liikenne ei muuten reititetty uusi AM.

Kunkin Azure tilauksella on oletusarvoinen rajoitukset paikassa, mukaan lukien enimmäismäärä VMs alueittain. Voit suurentaa rajoitus siirtämiseen tukipyynnön. Lisätietoja on artikkelissa [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset][subscription-limits].  

### <a name="vm-scale-sets"></a>AM asteikko joukot 

AM asteikko joukot avulla voit ottaa käyttöön ja hallita samanlaiset VMs joukko. Kaikki VMs määrittää sama, AM mittakaava joukot tukevat tosi Automaattinen skaalaus-ilman valmistelu valmiiksi VMs, muodosta suurissa services kohdistamisen suuri Laske big datasta ja tai kontteihin pakattuja toiminnoista on helpompaa. 

Lisätietoja AM asteikko joukot artikkelissa [Virtuaalikoneen asteikko määrittää yleiskatsaus][vmss].

Huomioon otettavia seikkoja AM asteikko joukkojen avulla:

- Jos haluat nopeasti skaalata VMs ulos tai Automaattinen skaalaus on, ota huomioon asteikko joukot. 

- Tällä hetkellä asteikko joukot eivät tue tietojen levyjä. Tietojen tallentaminen vaihtoehdot ovat Azure tiedostojen tallentamisesta, OS-asema, Temp-asema tai ulkoisen säilöön, kuten Azure-tallennustilan. 

- Kaikki AM esiintymät sisällä asteikolla määritetään automaattisesti kuuluvat käytettävyys samoja, 5 vika toimialueet ja 5 päivityksen toimialueet.

- Käytetään oletusarvon mukaan mittakaava joukot "overprovisioning", mikä tarkoittaa mittakaava määrittäminen alun perin valmistelee Lisää VMs kuin pyydät ja poistaa ylimääräiset VMs. Yleistä success nopeutta parantaa valmisteltaessa VMs. 

- On suositeltavaa enää sitten kuin 20 VMs kohti tallennustilan tilin, joka on käytössä tai ei yli 40 VMs overprovisioning kanssa overprovisioning käytöstä.  

- Voit etsiä malleja Resurssienhallinta, käyttöönottaminen asteikko määrittää [Azure pikaopas malleja][vmss-quickstart].

- On määrittäminen käyttöön asteikko joukon VMs kahdella tavalla: Mukautetun kuvan luominen tai laajennuksia avulla voit määrittää AM, kun se on valmisteltu.

    - Mukautetun kuvan rakennettu asteikko-joukon on luotava kaikki OS levyn näennäiskiintolevyjen yhden tallennustilan tilin. 

    - Mukautetun kuvalliset haluat säilyttää kuvan ajan tasalla.

    - On voi kestää kauemmin, asettamasi juuri valmistellun AM.

Muita huomioon otettavia seikkoja artikkelissa [Suunnitteleminen AM asteikko joukot, asteikko][vmss-design].

> [AZURE.TIP]  Minkä tahansa Automaattinen skaalaus-ratkaisun käytettäessä testaa se tuotannon tason työn Lataa hyvin etukäteen. 

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Käytettävyys määrittäminen mahdollistaa sovelluksen Lisää joustavat sekä suunnittelematon suunniteltu ylläpito-tapahtumat.

- _Suunniteltu ylläpito_ tapahtuu, kun Microsoft päivittää pohjana ympäristö ilmenneet joskus VMs on käynnistettävä uudelleen. Azure tekee että VMs käytettävyys määrittäminen eivät ole kaikki uudelleen samaan aikaan, vähintään yksi on käytettävissä käytössä, kun muut käyttäjät ovat uudelleen.

- _Suunnittelematon ylläpito_ tapahtuu, jos laitteen epäonnistui. Azure varmistetaan, että valmistellaan useamman kuin yhden palvelimen Teline VMs käytettävyys määrittäminen. Näin voit vähentää laitteisto, verkon katkokset, power keskeytyksiä ja niin edelleen.

Lisätietoja on kohdassa [Manage näennäiskoneiden käytettävyyttä][availability set]. Seuraavassa videossa on myös käytettävyys joukot hyvä yleiskatsaus: [Miten voin määrittää käytettävyys asteikko VMs][availability set ch9]. 

> [AZURE.WARNING]  Varmista, että eri sijainneissa käytettävissä, kun AM valmistella määrittäminen. Tällä hetkellä tällä ei voi lisätä Resurssienhallinta AM jälkeen AM on valmisteltu saatavuus.

Kuormituksen käyttää [kunto probes] seurannassa AM esiintymät käytettävyyttä. Jos näytteenottimen eivät pääse erillisen aikakatkaisuajan kuluessa, kuormituksen lopettaa liikenne lähettää kyseisen AM. Kuormituksen säilyvät tutkia, ja jos AM käytettävyys uudelleen, kuormituksen jatkaa liikenne lähettää kyseisen AM.

Seuraavassa on joitain suosituksia kuormituksen tasauspalvelun kunto keräysputkien:

- Keräysputkien testata HTTP- tai TCP. Jos VMs-Suorita HTTP-palvelimessa (IIS, nginx, Node.js sovelluksen ja niin edelleen), Luo HTTP-näytteenottimen. Luo muussa TCP-näytteenottimen.

- Määritä HTTP-näytteenotin, HTTP-päätepisteen polku. Näytteenottimen tarkistaa Tämä polku HTTP 200-vastausta. Tämä voi olla pääkansion polku ("/") tai kunnon valvonta päätepisteen, joka sisältää mukautetun logiikkaa sovelluksen kuntotietojen tarkistus. Päätepisteen on sallittava anonyymi HTTP-pyynnöt.

- Näytteenottimen lähetetään [tunnetut] [ health-probe-ip] IP-osoite, 168.63.129.16. Varmista, että ei estä tietoliikenne tai pois IP palomuurin käytännöt tai verkon suojaussäännöt ryhmän (NSG).

- Käytä [näytteenottimen kunto-lokit] [ health probe log] kunto keräysputkien tila. Ottaa käyttöön jokaisen kuormituksen Azure portaali kirjautumisesta. Lokit kirjoitetaan Azure-Blob-säiliö. Lokit näyttäminen montako VMs uudelleen ei saa verkkoliikennettä vuoksi epäonnistui näytteenottimen vastaukset.

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Useita VMs on tärkeää automatisoida kannalta, jolloin ne ovat luotettavia ja toistettavien. Voit käyttää [Azure automaatio] [ azure-automation] automatisoida käyttöönoton, OS korjataan ja muihin tehtäviin. Azure automaatio on automaatio-palvelu, joka suoritetaan Azure ja joka perustuu Windows PowerShell. Esimerkki automaattisen komentosarjan ovat käytettävissä [Runbookin valikoima] TechNetissä.

## <a name="security-considerations"></a>Suojausasiat

Virtuaalinen verkkoja liikenne eristystaso rajan Azure-tietokannassa. Yksi VNet VMs et voi toimittaa suoraan eri VNet VMs. VMs samassa VNet viestiä, ellet luo [verkon käyttöoikeusryhmät] [ nsg] (NSGs), jos haluat rajoittaa liikenne. Lisätietoja on artikkelissa [Microsoftin pilvipalveluihin ja verkkosuojauksen][network-security].

Internet-liikenteen kuormituksen tasauspalvelun säännöt määritetään liikenne pääsee uudelleen. Kuitenkin kuormituksen tasauspalvelun säännöt eivät tue IP-whitelisting, joten jos haluat whitelist tiettyjä julkiseen IP-osoitteet, lisätä NSG aliverkon.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Viite-arkkitehtuuri, joka sisältää näitä suosituksia käyttöönoton on käytettävissä GitHub. Viite-arkkitehtuuri sisältää virtual verkon (VNet), verkko-käyttöoikeusryhmän (NSG), kuormituksen ja kaksi näennäiskoneiden (VMs).

Viite-arkkitehtuuri voi asentaa Windows-tai Linux VMs alla olevia ohjeita noudattamalla: 

1. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Kun linkki on avattu Azure-portaalissa, syötä arvot, joitakin asetuksia: 
    - **Resurssiryhmä** nimi on jo määritetty parametri-tiedosto, joten valitsemalla **Käytä aiemmin luotua** ja kirjoita `ra-multi-vm-rg` tekstiruutuun.
    - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
    - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
    - Valitse avattavasta valikosta **windows** -ruutuun tai **linux** **Os tyyppi** . 
    - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
    - Valitse **Osta** -painiketta.

3. Odota suorittamiseen käyttöönottoa varten.

4. Parametritiedostot ovat koodattu järjestelmänvalvojan käyttäjänimi ja salasana ja on erittäin suositeltavaa muuttaminen voit heti molemmat. Valitse nimeltä AM `ra-multi-vm1` Azure-portaalissa. Valitse Valitse **salasanan** - **tuki + vianmääritys** -sivu. Valitse **salasanan** **tilassa** avattava luetteloruutu-ruutu ja valitse uusi **käyttäjänimi** ja **salasana**. Tallenna uusi käyttäjänimi ja salasana valitsemalla **Päivitä** . Toista nimeltä AM `ra-multi-vm2`.

Lisätietoja muitakin tapoja ottaa viite-arkkitehtuuri on artikkelissa [ohjeita single-AM] Lueminut-tiedosto[ github-folder] GitHub kansio. 

## <a name="next-steps"></a>Seuraavat vaiheet

Useita VMs kuormituksen tasauspalvelun takana on rakenneosan monitasoisten arkkitehtuureihin luomiseen. Lisätietoja: [N-taso-arkkitehtuuri-Azure Windows VMs käynnissä] [ n-tier-windows] ja [N-taso-arkkitehtuuri Azure-Linux VMs käynnissä][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[kuntotietojen keräysputkien]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[kuormituksen]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Runbookin valikoima]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Usean AM-ratkaisun, joka määrittää kahden VMs ja kuormituksen tasauspalvelun saatavuus Azure arkkitehtuuri"
