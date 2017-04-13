<properties
   pageTitle="ExpressRoute usein kysytyt kysymykset"
   description="ExpressRoute usein kysytyt kysymykset sisältää tietoja tuettu Azure-palveluja, kustannukset, tiedot ja yhteydet, SLA, tarjoajat ja sijainnit, kaistanleveyden ja lisää teknisistä tiedoista."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="expressroute-faq"></a>ExpressRoute usein kysytyt kysymykset


## <a name="what-is-expressroute"></a>Mikä on ExpressRoute?
ExpressRoute on Azure-palvelu, jolla voit luoda yksityinen yhteyksiä Microsoft palvelinkeskusten ja infrastruktuurin, joka on paikallisesti tai ulkoistaminen-tilojen välillä. ExpressRoute yhteydet ei siirry julkisen Internetin välityksellä ja tarjota suurempi suojaus, luotettavuutta ja nopeuksia alemman viiveet suurempia kuin tavallinen yhteydet kanssa Internetissä.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>Mitä hyötyä ExpressRoute ja yksityisverkkoyhteydet käytön?
ExpressRoute yhteydet ei siirry julkisen Internetin välityksellä ja tarjota suurempi suojaus, luotettavuutta ja nopeuksia ala- ja yhtenäinen viiveet suurempia kuin tavallinen yhteydet kanssa Internetissä. Joissakin tapauksissa ExpressRoute yhteyksien avulla voit siirtää tietoja välillä paikallisen laitteet ja Azure voi tuottaa kustannukset merkittäviä etuja.

### <a name="what-microsoft-cloud-services-are-supported-over-expressroute"></a>Mitä Microsoftin pilvipalveluihin tuetaan ExpressRoute kautta?
ExpressRoute tukee useimmat Microsoft Azure-palveluista, kuten tänään Office 365: ssä.  Tarkista päivitykset yleiseen käyttöön pian.

### <a name="where-is-the-service-available"></a>Missä on palvelu?
Katso tämän sivun sijainti ja käytettävyyden: [ExpressRoute kumppanit ja sijainnit](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>Miten ExpressRoute käyttää muodostamaan yhteys Microsoft, jos käytössäni ei ole sektorin jonkin ExpressRoute carrier kumppaneiden kanssa?
Voit valita aluekohtaiset carrier ja Power View Ethernet yhteydet johonkin tuetut exchange tarjoajan sijainnit. Voit sitten vertaiskone Microsoft tarjoajan sijainnissa. Tarkista [ExpressRoute kumppanit ja sijainnit](expressroute-locations.md) , onko käytössä exchange-Kohdista-palveluntarjoajan viimeisessä osassa. Voit tilata sitten ExpressRoute-piiri muodostaa Azure palveluntarjoajan kautta.

### <a name="how-much-does-expressroute-cost"></a>Kuinka paljon ExpressRoute kustannukset?
Tarkista hintatiedot [hinnoittelua tiedot](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Jos voin maksaa ExpressRoute-piiri annetun kaistanleveyden, voin hankkia verkon palveluntarjoaja VPN-yhteyden olla ovat yhtä nopeita?
Ei. Voit hankkia VPN-yhteyden nopeudesta minkä tahansa-palveluntarjoajalta. Azure-yhteys kuitenkin rajoitettu ExpressRoute piiri kaistanleveyden, jota ostat.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-required"></a>Jos voin maksaa ExpressRoute-piiri annetun kaistanleveyden, tarvitsen mahdollisuus burst suurempia nopeuksia ylöspäin, jos tarvitaan?
Kyllä. ExpressRoute piirit on määritetty tukemaan tapaukset, joissa voit burst jopa kaksi kertaa varten ilman lisäkustannuksia hankittu kaistanleveydestä. Tarkista palveluntarjoajalta, jos ne tukevat tätä ominaisuutta.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Käytettävät samaa yksityisverkon yhteyttä VPN- ja Azure muiden kanssa samanaikaisesti
Kyllä. ExpressRoute-piiri, kerran asetukset pystyt käyttämään virtual verkon palveluja ja Azure muiden samanaikaisesti. Voit muodostaa yhteyden virtual verkkojen yksityinen peering polku ja muut palvelut julkisen peering polussa.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>ExpressRoute tarjota palvelussa palvelutasosopimusta (SLA)?
Lue lisätietoja [ExpressRoute SLA-sivu](https://azure.microsoft.com/support/legal/sla/) .

## <a name="supported-services"></a>Tuetut palvelut
Useimmat Azure services tuetaan ExpressRoute päälle.

- Yhteys näennäiskoneiden ja cloud Services-palvelut on otettu käyttöön virtual verkkoja tuetaan yksityinen peering polussa.
- Azure sivustojen tuetaan julkisen peering polussa.
- IoT keskittimeen tuetaan julkisen peering polussa.
- Office 365: n tuetaan Microsoft peering polussa.
- Kaikki muut palvelut ovat käytettävissä julkisen peering polussa. Poikkeukset ovat seuraavat:

    **Seuraavat palvelut ei tueta:**

    - CDN
    - Visual Studio Team Services kuormituksen testaaminen
    - Monimenetelmäisen todentamisen
    - Liikenteen hallinta

## <a name="data-and-connections"></a>Tietojen ja yhteydet

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>Onko rajoitusten ExpressRoute avulla voit siirtää tietojen määrään?
Emme ole määritetty rajoitukset tiedonsiirto määrää. Lisätietoja on lisätietoja kaistanleveyden korvaukset [hinnat tiedot](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>Mitä yhteysnopeus tukemat ExpressRoute?
Tuetut kaistanleveyden tarjouksia:

| 50 Mbps, 100 Mbps, 200 Mbps 500 Mbps 1Gbps, 2 Gbps, 5 Gbps 10Gbps |

### <a name="which-service-providers-are-available"></a>Mitä palveluntarjoajien on käytettävissä?
Katso [ExpressRoute kumppanit ja sijainnit](expressroute-locations.md) for palveluntarjoajia ja sijainnit.

## <a name="technical-details"></a>Teknisiä tietoja

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>Mitkä ovat tekniset vaatimukset muodostamisesta Azure paikallisen sijaintini?
Katso [ExpressRoute edellytykset sivun](expressroute-prerequisites.md) vaatimuksia.

### <a name="are-connections-to-expressroute-redundant"></a>Ovat ExpressRoute yhteydet tarpeettomat?
Kyllä. Kunkin Express reitin piiri on tarpeettomia kahdet väliset yhteydet, jotka on määritetty Suuri käytettävyys.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>Voin menetetään connectivity Jos jokin ExpressRoute linkit eivät toimi?
Menetät ei yhteyden, jos jokin cross yhteyksistä epäonnistuu. Tarpeettomien yhteys on käytettävissä tukemaan verkon kuormituksen. Voit luoda useita piirit lisäksi saada virheen toimintakykyyn peering eri sijainnissa.

### <a name="onep2plink"></a>Jos en ole yhtä osoitteessa cloud exchange ja palveluntarjoaja tarjoaa pisteestä pisteeseen-yhteys, pitääkö tilauksen kaksi fyysinen Omat paikalliseen verkkoon ja Microsoft väliset yhteydet? 
Ei, tarvitset vain yhtä fyysistä yhteyttä Jos palveluntarjoajan muodostaa kahden Ethernet virtual piirit fyysinen yhteyden kautta. Fyysinen (kuten OCR-fiber) yhteys katkaistaan tason 1 (L1) laitteen (Katso alla olevassa kuvassa). Kaksi Ethernet virtual virtapiirit on merkitty eri VLAN tunnukset, yksi, jonka ja yksi toissijaisen. VLAN näiden tunnusten on ulompi 802.1Q Ethernet otsikko. Sisemmän 802.1Q Ethernet otsikko (ei näy) on nyt yhdistetty [ExpressRoute Reititystoimialue](expressroute-circuit-peerings.md). 

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)


### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>Voit voin perusteella jokin Omat näennäislähiverkkojen Azure käyttämällä ExpressRoute?
Ei. Microsoft ei tue kerroksen 2 connectivity tunnisteet Azure kyselyjä.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Minulla on useampi kuin yksi ExpressRoute piiri tilauksen?
Kyllä. Voit valita useamman kuin yhden ExpressRoute piiri tilauksen. Erillinen piirit määrän oletusrajoitus on määritetty 10. Ota Microsoft Support niin, että rajoitus tarvittaessa.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Ovatko ExpressRoute piirit eri tarjoajien?
Kyllä. Voit määrittää ExpressRoute piirit useita palveluntarjoajia. Kunkin ExpressRoute piiri päivitetään vain yhden palveluntarjoajan.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Miten oma virtual verkostojen yhdistäminen ExpressRoute piiri
Perusvaiheet ovat Jäsennettyjen alapuolella.

- Edellyttää ExpressRoute piiri muodostaa ja ottaa sen käyttöön palveluntarjoajan on.
- Voit tai palvelu on määritettävä erityisen peering (s).
- Linkittäminen ExpressRoute piiri virtual verkkoon.

Lisätietoja on kohdassa [piiri valmistelu ja piiri Yhdysvaltojen ExpressRoute työnkulut](expressroute-workflows.md) .

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>Onko yhteys reunat ExpressRoute-piiri?
Kyllä. [ExpressRoute kumppanit ja sijainnit](expressroute-locations.md) -sivulla on yleiskatsaus connectivity rajat-ExpressRoute piiri. Yhteyden ExpressRoute piiri on rajoitettu vain yhden geopoliittisten alueen. Yhteys voidaan laajentaa ristiksi geopoliittisten alueiden ottamalla ExpressRoute premium-ominaisuus.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Voit luoda linkin useita virtual verkon ExpressRoute piiri?
Kyllä. Voit linkittää enintään 10 virtual verkkojen ExpressRoute piiri.

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Minulla on useita Azure-tilaukset, jotka sisältävät virtual verkot. Voit muodostaa virtual verkkoihin, joita yksittäisen ExpressRoute piiri erillisessä tilaaminen?
Kyllä. Voit sallia muiden enintään 10 Azure tilaukset käyttämään yksittäisen ExpressRoute piiri. Tämä rajoitus voidaan lisätä ottamalla ExpressRoute premium-ominaisuus.

Lisätietoja on artikkelissa [jakamisen ExpressRoute-piiri useita-tilauksissa](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Virtuaalinen verkkojen yhdistettyjen saman piiri toisistaan erillään?
Ei. Linkitetty saman ExpressRoute piiri virtual verkoista ovat samassa Reititystoimialue ja eivät ole toisistaan erillään reititys näkökulmasta. Jos tarvitset reitin eristystaso, tarvitset erillinen ExpressRoute virtapiiri luomiseen.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Voi olla yksi virtual verkko on useampi kuin yksi ExpressRoute piiri yhteydessä?
Kyllä. Voit linkittää yhden virtual verkoston kanssa 4 ExpressRoute piirit. Ne on tilattava 4 eri [ExpressRoute sijainnit](expressroute-locations.md).

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>Voin käyttää Internetissä: Omat ExpressRoute piirit virtuaalinen verkkojen?
Kyllä. Jos olet ei määrittämiisi tiet oletusarvo (0.0.0.0/0) tai internet reitin etuliitteiden erityisen istunnon kautta, voi linkittää ExpressRoute piiri virtual verkosta internet-yhteys.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>Voit estää internet-yhteys virtual verkkoihin ExpressRoute piirit yhteydessä?
Kyllä. Voit estää kaikki internet-yhteys näennäiskoneiden virtual verkon käyttöön, tiet oletusarvo (0.0.0.0/0) mainostaa ja reitittää, kaikki liikenne ExpressRoute piiri. Huomaa, että jos mainostaa oletusarvon tiet, emme pakottaa liikenne palveluihin tarjota julkisen peering (kuten Azure tallennustilan ja SQL DB) että paikallisen takaisin päälle. Sinun on määrittäminen yhteyttä reitittimen liikenne palaa Azure julkisen peering avulla tai Internetin välityksellä.

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Virtual verkkojen saman ExpressRoute piiri linkitetty toisiinsa puhua?
Kyllä. Näennäiskoneiden on otettu käyttöön sama ExpressRoute piiri virtual verkkojen viestiä toistensa kanssa.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>Sivusto yhteys käytetään virtual verkkojen ExpressRoute yhdessä
Kyllä. Sivuston sivuston VPN-yhteydet voivat olla ExpressRoute.

### <a name="can-i-move-a-virtual-network-from-site-to-site--point-to-site-configuration-to-use-expressroute"></a>Virtuaalinen verkon siirtää käyttämään ExpressRoute sivusto sivusto / pisteen sivuston asetuksista
Kyllä. Sinun on luotava ExpressRoute yhdyskäytävän virtual verkossa oleville. Ilmenee pieni käyttökatkot, liittyvää prosessia.

### <a name="what-do-i-need-to-connect-to-azure-storage-over-expressroute"></a>Mitä tarvitsen yhteyden muodostaminen Azuren tallennustilaan ExpressRoute kautta?
Täytyy vahvistaa ExpressRoute piiri ja määritä tiet, julkisen peering.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Mitä voin mainostaa tiet enimmäismäärään?
Kyllä. Olemme Hyväksy 4000 reitin etuliitteitä varten yksityinen peering ja 200 julkisen peering ja Microsoft peering ylöspäin. Voit kasvattaa 10 000 tiet, yksityinen peering, jos otat ExpressRoute premium-toiminnon avulla.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>Mitä rajoituksia IP-alueita voin mainostaa erityisen istunnon kautta?
Microsoft ei hyväksy yksityinen etuliitteiden (RFC1918) julkinen ja Microsoft peering erityisen-istunnossa.

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>Mitä tapahtuu, jos minulla erityisen rajoittaa?
Erityisen istunnot poistetaan. Ne palautetaan, kun etuliite-määrä siirtyy rajan alle.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>Mikä on ExpressRoute erityisen pitoon aikaa? Se voidaan säätää?
Pidon aika on 180. Pysyvät viestit lähetetään 60 sekunnin välein. Nämä ovat kiinteät asetukset, joita ei voi muuttaa Microsoft-puolella.

### <a name="after-i-advertise-the-default-route-00000-to-my-virtual-networks-i-cant-activate-windows-running-on-my-azure-vms-how-to-i-fix-this"></a>Kun voin mainostaa oletusarvon reitin (0.0.0.0/0) Omat virtual verkkoihin, aktivoimisen et voi Windows Azure-VMs käytössä. Miten voin korjaaminen tämä?
Seuraavat vaiheet auttaa Azure tunnistaa aktivointipyynnön:

1. Muodostaa ExpressRoute piiri julkisen peering.
2. Suorittaa DNS-haku ja Etsi **kms.core.windows.net** IP-osoite
3. Valitse jokin seuraavat kaksi kohdetta, niin, että Avaintenhallintapalvelulla tunnistaa aktivoinnin pyynnön tulee Azure ja tukee pyynnön.
    - Paikallisen verkossa reitittää liikenteen, joka on tarkoitettu IP-osoite (vaiheessa 2 saadun) takaisin Azure kautta julkisen peering.
    - On NSP tarjoajan karva-PIN-koodin liikenteen Azure julkisen peering kautta.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Voit muuttaa ExpressRoute piiri kaistanleveyden?
Kyllä. Voit suurentaa ExpressRoute piiri kaistanleveyden eikä sinun tarvitse tear. Sinulla on yhteys palvelussa varmistamiseksi, ei päivitetä sisällä niiden verkkojen kaistanleveyden lisäys ylikuormitustilan seuranta. Voit kuitenkin ei saa oikeuden ExpressRoute piiri kaistanleveyden vähentämiseksi. Tarvitse kaistanleveyden tarkoittaa irti revittävät alaspäin ala- ja ExpressRoute piiri vapaa.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>Miten voin muuttaa ExpressRoute piiri kaistanleveyden?
Voit päivittää kaistanleveyden ExpressRoute virtapiirin omistautunut päivityksen piiri API ja PowerShell-cmdlet-komennolla.

## <a name="expressroute-premium"></a>ExpressRoute Premium

### <a name="what-is-expressroute-premium"></a>Mikä on ExpressRoute premium?
ExpressRoute premium on kokoelma alla luetellut toiminnot.

 - Korottaa reititys taulukon raja-4000 tiet 10 000 tiet, yksityinen peering.
 - Korottaa määrä, joiden yhdistämistä tuetaan ExpressRoute virtapiirin VNets (oletusarvo on 10). Katso lisätietoja alla olevassa taulukossa.
 - Yleinen yhteys Microsoft core verkon kautta. Osaat nyt voit linkittää VNet geopoliittisten alueella ExpressRoute-piiri toisen alueen kanssa. **Esimerkki:** Voit linkittää VNet, joka on luotu piin laakso ExpressRoute-piiri Europe Länsi kansioon.
 - Office 365-palveluissa ja CRM Online-yhteys.

### <a name="how-many-vnets-can-i-link-to-an-expressroute-circuit-if-i-enabled-expressroute-premium"></a>Kuinka monta VNets voit luoda linkin ExpressRoute piiri Jos voin käytössä ExpressRoute premium?
Seuraavissa taulukoissa näkyy ExpressRoute rajoitukset ja VNets määrä ExpressRoute piiri kohden.


[AZURE.INCLUDE [expressroute-limits](../../includes/expressroute-limits.md)]


### <a name="how-do-i-enable-expressroute-premium"></a>Miten voin antaa ExpressRoute premium?
ExpressRoute premium-ominaisuuksia voi ottaa käyttöön, kun ominaisuus on käytössä, ja voit Sammuta päivittämällä piiri tila. Voit ottaa käyttöön ExpressRoute premium piiri luonnin aikana tai soittaa päivityksen varatut piiri API / PowerShell cmdlet-komento, jotta ExpressRoute premium.

### <a name="how-do-i-disable-expressroute-premium"></a>Miten voin vaimentaa ExpressRoute premium?
Voit poistaa käytöstä ExpressRoute premium soittamalla varattu päivityksen piiri API / PowerShell cmdlet-komento, sinun on varmistettava, että yhteys on skaalattu on täytettävä oletusarvon rajoituksia, ennen kuin poistat ExpressRoute premium. Olemme epäonnistuu pyynnön, jos haluat poistaa käytöstä ExpressRoute premium oman käyttö Skaalaa oletusarvon rajoissa.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Voinko valita haluan premium ominaisuus joukosta ominaisuuksia?
Ei. Sinulla voi valita ominaisuuksista, joita tarvitset. Olemme ottaminen käyttöön kaikkia ominaisuuksia, kun otat käyttöön ExpressRoute premium.

### <a name="how-much-does-expressroute-premium-cost"></a>Kuinka paljon ExpressRoute premium kustannukset?
Lisätietoja kustannusten [hinnat tiedot](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>Voin maksaa ExpressRoute Premium lisäksi vakio ExpressRoute kulut
Kyllä. ExpressRoute premium ovat voimassa tasalla ExpressRoute piiri kulujen ja kulujen vaatii yhteyden toimittaja.

## <a name="expressroute-and-office-365-services-and-crm-online"></a>ExpressRoute ja Office 365-palveluihin ja CRM Online

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services-and-crm-online"></a>Miten voin luoda ExpressRoute-piiri, voit muodostaa yhteyden Office 365-palveluissa ja CRM Online-tilassa?

1. Tarkista [ExpressRoute edellytykset](expressroute-prerequisites.md) -sivu, varmista, että ehtojen mukaan.
2. Tarkista palveluntarjoajien ja sijainnit [ExpressRoute kumppanit](expressroute-locations.md) ja sijainnit, varmista, että yhteys tarpeitasi täyttyvät.
3. Suunnittele kapasiteetin tarkastelemalla [verkkosuunnittelu ja suorituskyvyn parantaminen Office 365: ssä](http://aka.ms/tune/).
4. Noudata seuraavia ohjeita asennuksen connectivity [ExpressRoute työnkulut piiri valmistelu ja piiri Yhdysvaltojen](expressroute-workflows.md)työnkuluissa.

>[AZURE.IMPORTANT] Varmista, että olet ottanut ExpressRoute premium lisäosa yhteys Office 365-palveluissa ja CRM Online määritettäessä.

### <a name="do-i-need-to-enable-azure-public-peering-to-connect-to-office-365-services-and-crm-online"></a>Pitääkö käyttöön Azure julkisen Peering muodostaa yhteyden Office 365-palveluissa ja CRM Online-tilassa?
Ei, voit vain on otettava käyttöön Microsoft Peering. Todennus-liikenne paikalliseen Azure AD lähetetään Microsoft Peering kautta. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-crm-online"></a>Oma aiemmin ExpressRoute piirit tukee yhteys Office 365-palveluissa ja CRM Online-tilassa?
Kyllä. Olemassa olevan ExpressRoute-piiri on määritetty tukemaan muodostaa yhteys Office 365-palveluissa. Varmista, että sinulla on riittävät kapasiteetin haluat muodostaa yhteyden Office 365-palveluissa ja varmista, että olet ottanut premium-lisäosa. [Verkkosuunnittelu ja suorituskyvyn parantaminen Office 365: n](http://aka.ms/tune/) avulla voit suunnitella connectivity tarpeitasi. Katso myös [Luo ja Muokkaa ExpressRoute piiri](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>Mitä palveluja niitä voi käyttää ExpressRoute yhteyden Office 365: ssä?

Lisätietoja [Office 365: n URL-osoitteet ja IP-osoitealueet](http://aka.ms/o365endpoints) sivun ajan tasalla tueta ExpressRoute palvelut-luettelo.

### <a name="how-much-does-expressroute-for-office-365-services-and-crm-online-cost"></a>Kuinka paljon ExpressRoute Office 365-palveluissa ja CRM Online kustannukset?
Office 365-palveluihin ja CRM Online edellyttää premium-lisäosa on otettu käyttöön. [Hinnat tiedot-sivu](https://azure.microsoft.com/pricing/details/expressroute/) sisältää tietoja kustannusten ExpressRoute.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Mitä alueita ExpressRoute Office 365: n voi käyttää?
Lisätietoja saat lisätietoja luettelossa kumppanien ja sijainnit, joissa tuetaan ExpressRoute [ExpressRoute kumppanit ja sijainnit](expressroute-locations.md) .

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>Pääsenkö Office 365: n Internetin välityksellä vaikka ExpressRoute on määritetty organisaatiossani?
Kyllä. Office 365-Palvelupäätepisteet ovat tavoitettavissa Internetin kautta, vaikka ExpressRoute on määritetty verkkoa varten. Jos olet sijaintiin, jossa on määritetty muodostaa yhteyden Office 365-palvelujen avulla ExpressRoute, muodostaa yhteyden ExpressRoute kautta.

### <a name="can-dynamics-ax-online-be-accessed-over-an-expressroute-connection"></a>Dynamics AX Online voi käyttää ExpressRoute yhteyden?
Ei, sitä ei tueta.
