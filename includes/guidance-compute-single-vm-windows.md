Tässä artikkelissa käsitellään joukko osoitettu käytännöt käynnissä Windows virtual machine (AM)-Azure-huomiota skaalattavuus, käytettävyyttä, hallinta ja suojaus. 

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Azure Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Tässä artikkelissa käytetään Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Ei ole suositeltavaa yksittäisen AM käytön tuotannon toiminnoista, koska ei ole ylös / aika-tason palvelusopimus (SLA) yksittäisen VMs Azure-varten. Saat SLA-käyttöönotto [saatavuus]useita VMs[availability-set]. Lisätietoja on artikkelissa [käynnissä useita Windows VMs Azure-][multi-vm]. 

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Lisää liikkuvat osat kuin vain AM liittyy valmistelu AM Azure-tietokannassa. On suorittaminen ja tallennustilaa verkko-osat.

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on käytössä "Laske - AM yhdelle sivulle.

![[0]][0]


- **Resurssiryhmä.** [_Resurssiryhmä_] [ resource-manager-overview] on säilö, joka sisältää liittyvät resurssit. Luoda tallentamaan tämän AM resursseja resurssiryhmän.

- **VM**. Voit valmistella AM julkaistuja kuvia luettelo tai virtual kiintolevyn (Näennäiskiintolevyn)-tiedosto, joka Azure-blob-säiliö lataaminen.

- **OS levy.** OS levy on [Azuren tallennustilaan]tallennettuja Näennäiskiintolevyn[azure-storage]. Tämä tarkoittaa sitä, se toistuu myös silloin, kun host koneen siirtyy.

- **Tilapäinen levy.** Tilapäinen DVD-levyllä luodaan AM ( `D:` Windows asema). Levyn tallennetaan fyysinen asemaan host tietokoneeseen. Se on _tallennettu Azure-tallennustilan ja voivat poistua aikana käynnistyy ja muita AM elinkaari tapahtumia_ . Käytä levyä vain tilapäinen tietoja, kuten sivun tai Vaihda tiedostot.

- **Tietoja levyjen.** [Tietoja levyn] [ data-disk] on käyttää sovelluksen tiedot pysyvät Näennäiskiintolevyn. Tietoja levyjen tallennetaan Azure tallennuspaikkaa, kuten OS levy.

- **Virtuaalinen verkon (VNet) ja aliverkon.** Jokaisen AM Azure-tietokannassa on otettu käyttöön VNet, joka on jaettu edelleen aliverkosta kyselyjä.

- **Julkiseen IP-osoite.** Julkinen IP-osoite tarvita AM viestintään&mdash;esimerkiksi etätyöpöydän (RDP) kautta.

- **Verkkoliittymän (NIC)**. Verkkokortti mahdollistaa AM virtual verkoston kanssa kommunikoimiseen.

- **Verkko-käyttöoikeusryhmän (NSG)**. [NSG] [ nsg] käytetään aliverkon verkkoliikenteen Salli tai estä. Voit liittää NSG yksittäisiä NIC tai aliverkon kanssa. Jos liität sen aliverkon, NSG säännöt otetaan käyttöön, kaikki kyseisen aliverkon-VMs.
 
- **Diagnostiikka.** Diagnostiikan kirjaus on tärkeää hallinnasta ja vianmäärityksestä AM.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="vm-recommendations"></a>AM suositukset

Suosittelemme DS - ja GS-sarjan, koska nämä koneen koot tukevat [Premium tallennustilan][premium-storage]. Valitse tietokoneen näitä kokoja, ellei erityinen työmäärän, kuten tehokäytön. Lisätietoja on artikkelissa [virtuaalikoneen koot][virtual-machine-sizes]. Kun aiemmista työtehtävistäsi toiselle työntekijälle Siirry Azure ja aloita parhaiten vastaava paikallinen palvelimiin AM haluamasi kokoinen. Valitse toimenpide todellinen havainnollistamiseen, jotka koskevat suorittimen ja muistin levyn suorituskykyä i toimintoja sekunnissa (IOPS) ja koon säätäminen tarvittaessa. Myös, jos tarvitset useita NIC, on otettava huomioon jokaisen koon NIC rajoitus.  

Kun olet valmistelu AM ja muita resursseja, sinun on määritettävä sijainti. Valitse yleensä lähinnä sisäisten käyttäjien tai asiakkaiden sijainti. Kuitenkin kaikki AM koot eivät ole käytettävissä kaikissa sijainneissa. Lisätietoja on artikkelissa [alueen palvelujen][services-by-region]. Luettelo käytettävissä AM koot, annetun sijainnissa, suorita Azure käyttöliittymä (CLI) seuraava komento:

```
    azure vm sizes --location <location>
```

Lisätietoja valitsemalla julkaistun AM kuva artikkelissa [Siirry ja valitse Azure virtuaalikoneen kuvien][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Levyn ja tallennustilaa suositukset

Levyn i/o parhaan suorituskyvyn, on suositeltavaa [Premium tallennustilan][premium-storage], joka tallentaa tiedot tasainen tilan asemista (SSDs). Kustannusten perustuu valmistellun levyn koko. IOPS ja siirtonopeuden riippuvat levyn koon, niin voit valmistella DVD-levyllä, ota huomioon, kun kaikki kolme tekijät (kapasiteettia, IOPS ja siirtonopeuden). 

Tallennustilan tilien tukee 1 – 20 VMs.

Lisää vähintään yksi tietojen levyjä. Kun luot uuden Näennäiskiintolevyn, on Muotoilematon. Kirjaudu sisään levyn AM.

Jos sinulla on paljon tietoja levyjä, huomioon tallennustilan tilin yhteensä i/o rajoissa. Lisätietoja on artikkelissa [Virtuaalikoneen levyn rajoitukset][vm-disk-limits].

Parhaan suorituskyvyn luoda erilliset tallennustilan tilin pitoon vianmäärityslokit. Normaali paikallisesti tarpeettomat storage (LRS)-tili on riittävä vianmäärityslokit.

Jos mahdollista, asentaa sovellukset tietojen DVD-levyllä, ei OS levy. Joidenkin vanhojen sovellusten on ehkä kuitenkin osat asennetaan c-asema. Siinä tapauksessa voit [muuttaa kokoa OS levyn] [ resize-os-disk] PowerShellin avulla.

### <a name="network-recommendations"></a>Verkon koskevia suosituksia

Julkinen IP-osoite voi olla dynaaminen tai staattinen. Oletusarvo on dynaaminen.

- Varaa [staattinen IP-osoite] [ static-ip] tarvittaessa kiinteä IP-osoitetta ei muuta &mdash; esimerkiksi, jos haluat luoda A-tietue DNS- tai on IP osoite on whitelisted.

- Voit myös luoda täydellinen toimialuenimi (FQDN) IP-osoitetta. [CNAME-tietue] Rekisteröi sitten[ cname-record] DNS-palvelimeen, joka osoittaa täydellinen toimialuenimi. Lisätietoja on kohdassa [Create täydellistä toimialuenimeä Azure-portaalissa][fqdn].

Kaikki NSGs sisältävät [oletusarvon]sääntöjoukon[nsg-default-rules], mukaan lukien säännön, joka estää kaikki Internet-liikenteen. Oletusarvoiset ei voi poistaa, mutta muita sääntöjä voit ohittaa ne. Internet-liikenne käyttöön Luo säännöt, jotka sallivat saapuvan liikenteen tietyn porttien &mdash; portti 80 esimerkiksi HTTP varten.  

RDP, Lisää NSG-sääntö, joka sallii saapuvan liikenteen TCP-porttia 3389.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Voit skaalata AM ylös tai alas muuttamalla [AM][vm-resize]. 

Skaalata vaakasuunnassa, sijoittaa kahden tai useamman VMs määrittää kuormituksen tasauspalvelun takana saatavuus. Lisätietoja on artikkelissa [käynnissä useita Windows VMs Azure-][multi-vm].

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Yllä mainittuja ei ole SLA yksittäisen AM varten. Saat SLA-asentaa useita VMs käytettävyys määrittäminen.

Oman AM voi vaikuttaa [suunniteltu ylläpito] [ planned-maintenance] tai [suunnittelematon ylläpito][manage-vm-availability]. Voit käyttää [AM uudelleen lokit] [ reboot-logs] määrittääksesi, onko AM uudelleenkäynnistyksen on aiheutunut suunniteltu ylläpito.

Näennäiskiintolevyjen varmuuskopioidaan [Azure]-tallennustilan[azure-storage], jossa on replikoida kestävyyttä ja käytettävyyttä.

Suojata tietoja vahingossa katoamiselta aikana (esimerkiksi vuoksi käyttäjä-virhe), kannattaa myös Toteuta ajankohta varmuuskopiot- [Blob-objektien tilannevedosten] käyttäminen[ blob-snapshot] tai muuta työkalua.

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

**Resurssiryhmät.** Sijoita tiiviisti kytketty resursseja, joilla on sama elinkaaren saman [resurssiryhmä][resource-manager-overview]. Resurssiryhmät mahdollistavat käyttöönotto ja seurata resurssien ryhmänä ja koota Laskutus kustannukset resurssiryhmän mukaan. Voit myös poistaa resurssien joukkona, joka on erittäin hyödyllinen testi käyttöönotoissa. Anna resurssien kuvaavat nimet. Joka on helppo etsiä tietyn resurssin ja ymmärrät sen rooli. Katso [Suositellut nimeämiskäytännöt Azure resurssien][naming conventions].

**AM Diagnostiikka.** Ota käyttöön seuranta- ja diagnostiikan asetuksia, kuten basic kunto arvot, diagnostiikka infrastruktuurin lokit ja [käynnistyksen diagnostiikka][boot-diagnostics]. Käynnistyksen diagnostiikka auttavat vianmäärityksen käynnistys epäonnistui, jos oman AM saa käynnistys tilaan. Lisätietoja on kohdassa [ottaminen käyttöön seuranta- ja diagnostiikka][enable-monitoring]. [Azure Log sivustokokoelman] [ log-collector] tunniste Azure ympäristö lokien kerääminen ja lataaminen Azure-tallennustilan.   

Seuraavat CLI-komennon avulla Diagnostiikka:

```
    azure vm enable-diag <resource-group> <vm-name>
```

**Pysäytetään AM.** Azure on "Pysäytetään" ja "Deallocated" välinen ero. Peritään kun tila on AM "pysäytetään". Kun AM Varaus poistettu ei perittävän.

Seuraavat CLI-komennon avulla voit poistaa AM varauksen:

```
    azure vm deallocate <resource-group> <vm-name>
```

**Pysäytä** -painike Azure-portaalissa myös vapauttaa AM. Kuitenkin Jos suljet – käyttöjärjestelmän AM pysäytetään kirjautunut sisään, mutta _ei_ Varaus poistettu, joten voit yhä veloitetaan.

**Poistaminen AM.** Jos poistat AM, näennäiskiintolevyt ei poisteta. Tämä tarkoittaa sitä, voit poistaa AM turvallisesti menettämättä tietoja. Voit silti veloitetaanko tallennustilan. Poista Näennäiskiintolevyn poistaa tiedoston [Blob-objektien tallennustilaan][blob-storage].

Jos haluat estää vahingossa, käytä [resurssin lukitus] [ resource-lock] lukitseminen koko resurssi ryhmän tai Lukitse yksittäisiä resurssit, kuten AM. 

## <a name="security-considerations"></a>Suojausasiat

Käytä [Azure Tietoturvakeskus] [ security-center] haluat nähdä keskitetyn Azure resurssien suojaus-tilaan. Tietoturvakeskus valvoo tietoturvariskin, kuten järjestelmän, antimalware, ja käyttöönoton suojauksen kunto täydellinen kuva. 

- Tietoturvakeskuksessa on määritetty Azure tilauskohtaisten. Ota käyttöön suojauksen tietojen kerääminen kuvatulla tavalla [Käytä Tietoturvakeskuksessa].

- Kun tietojen kerääminen on käytössä, Tietoturvakeskus tarkistaa automaattisesti kaikki VMs luotuihin kyseiseen tilaukseen.

**Korjaustiedoston hallinta.** Jos käytössä, Tietoturvakeskus tarkistaa, ovatko suojaus ja päivityksiä puuttuu. Käytä [ryhmäkäytäntöasetukset] [ group-policy] AM Automaattiset päivitykset käyttöön.

**Antimalware.** Jos otettu käyttöön, Tietoturvakeskus tarkistaa, onko antimalware ohjelmisto on asennettu. Voit myös asentaa antimalware ohjelmiston sisällä Azure portaalin Tietoturvakeskuksessa.

Käytä [Roolipohjainen käyttöoikeuksien valvonta] [ rbac] (RBAC), joiden avulla voit hallita Azure resurssien avulla voit ottaa käyttöön. RBAC avulla voit määrittää luvan roolit DevOps ryhmän jäsenille. Esimerkiksi lukija voi tarkastella Azure resursseja, mutta ei luoda, hallita, tai poista ne. Jotkin roolit ovat tietyn Azure resurssityypit. Esimerkiksi virtuaalikoneen osallistujan rooli voi uudelleen tai poistaa varauksen AM, järjestelmänvalvojan salasanan, Luo uusi AM ja niin edelleen. Muut [sisäiset RBAC roolit] [ rbac-roles] , jotka voivat olla hyödyllisiä viittaus-arkkitehtuuri mukana [DevTest harjoituksia käyttäjän] [ rbac-devtest] ja [Verkon avustaja][rbac-network]. Käyttäjä voi määrittää useita rooleja ja voit luoda mukautetun roolit paremmin tarkasti rajattuja käyttöoikeuksia.

> [AZURE.NOTE] RBAC rajoita toiminnot, jotka AM kirjautunut käyttäjä voi suorittaa. Käyttöoikeudet määräytyvät Vieras OS tilin tyyppi.   

Paikallisen järjestelmänvalvojan salasanan, suorita `vm reset-access` Azure CLI-komentoa.

```
    azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Käytä [valvontalokit] [ audit-logs] Nähdäksesi valmistelu toiminnot ja AM muita tapahtumia.

Harkitse [Azure salauksen] [ disk-encryption] Jos haluat salata OS ja tietoja-levyjä. 

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

[Käyttöönoton] [ github-folder] viittauksen arkkitehtuuri, joka osoittaa näitä parhaita käytäntöjä on käytettävissä. Viite-arkkitehtuuri sisältää virtual verkon (VNet), verkko-käyttöoikeusryhmän (NSG) ja yksittäisen virtual machine (AM).

Voit ottaa viite-arkkitehtuuri useilla tavoilla. Helpoin tapa on noudattamalla seuraavia ohjeita: 

1. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa".  
[![Ottaa käyttöön Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. Kun linkki on avattu Azure-portaalissa, syötä arvot, joitakin asetuksia: 
    - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-single-vm-rg` tekstiruutuun.
    - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
    - Älä muokkaa **Mallia pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
    - Valitse **Os tyyppi** avattavasta valikosta **windows**-ruutuun.
    - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
    - Valitse **Osta** -painiketta.

3. Odota suorittamiseen käyttöönottoa varten.

4. Parametritiedostot ovat koodattu järjestelmänvalvojan käyttäjänimi ja salasana ja on erittäin suositeltavaa muuttaminen voit heti molemmat. Valitse nimeltä AM `ra-single-vm0 `Azure-portaalissa. Valitse Valitse **salasanan** - **tuki + vianmääritys** -sivu. Valitse **salasanan** **tilassa** avattava luetteloruutu-ruutu ja valitse uusi **käyttäjänimi** ja **salasana**. Pysyvä uusi käyttäjänimi ja salasana valitsemalla **Päivitä** .

Lisätietoja muitakin tapoja ottaa viite-arkkitehtuuri on artikkelissa [ohjeita single-AM]readme-tiedoston[github-folder]] Github kansio. 

## <a name="customize-the-deployment"></a>Mukauta käyttöönotto

Jos haluat muuttaa käyttöönoton tarpeita vastaavaksi, noudata [Lueminut-tiedosto][github-folder]. 

## <a name="next-steps"></a>Seuraavat vaiheet

Jotta [SLA näennäiskoneiden] [ vm-sla] käyttöön käyttöönotto kahden tai useamman esiintymät käytettävyys määrittäminen. Lisätietoja on artikkelissa [käynnissä useita VMs Azure-][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-windows-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-windows-portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-windows-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-windows-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]: ../articles/virtual-machines/virtual-machines-windows-expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]: ../articles/virtual-machines/virtual-machines-windows-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[Käytä Tietoturvakeskus]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-windows-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[0]: ./media/guidance-blueprints/compute-single-vm.png "Yksittäisen Windows AM arkkitehtuuri Azure-tietokannassa"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
