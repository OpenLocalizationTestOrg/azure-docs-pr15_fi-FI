<properties
   pageTitle="Käynnissä Linux VMs useita alueilla suuren | Viittaus arkkitehtuuri | Microsoft Azure"
   description="Ottamisesta VMs Azure hyvän käytettävyyden ja vikasietoisuudelle useita alueilla."
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
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>Käynnissä Linux VMs suuren useita alueilla

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Käynnissä Linux VMs suuren useita alueilla](guidance-compute-multiple-datacenters-linux.md)
- [Käynnissä Windows VMs suuren useita alueilla](guidance-compute-multiple-datacenters.md)

Tässä artikkelissa on suositeltavaa joukon käytäntöjä, voit suorittaa useita Azure alueet käytettävyys ja tehokkaat tietojen palauttaminen-infrastruktuurin Linux näennäiskoneiden (VMs).

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource groups] ja perinteinen. Tässä artikkelissa käytetään Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Monille arkkitehtuuri tarjota suurempi kuin otat käyttöön vain yhden alueen käytettävyys. Jos alueellisen käyttökatkosta vaikuttaa ensisijainen alue, voit käyttää [Liikenteen hallinta] [ traffic-manager] epäonnistuu toissijainen alueen päälle. Tämä arkkitehtuuri auttaa myös, jos yksittäisiä alirakenne sovelluksen epäonnistuu.

Yleiset monilla saavuttamiseksi suuren käytettävyyden tietojen keskikohdan mukaan eri tavoilla:   
  
- Aktiivinen/passiivinen kuuma valmius kanssa. Liikenne siirtyy yhden alueen valmius muiden odottaa aikana. Toissijainen alueen VMs on kohdistettu ja käynnissä aina.

- Aktiivinen/passiivinen kylmän valmius kanssa. Sama, mutta VMs toissijainen alueen ei varata, kunnes tarvittavat automaattisesti. Tämän menetelmän kustannuksia pienempi, jos haluat suorittaa, mutta yleensä on enää alaspäin aikana epäonnistui.

- Aktiivisena. Molemmat alueet ovat aktiivisia ja pyynnöt ovat kuormitus tasataan niiden välille. Jos yksi tietokeskuksen ei ole käytettävissä, se otetaan kierto ulos.

Tämä arkkitehtuuri ohjeessa on aktiivinen/passiivinen kuuma valmius-liikenne hallinnan avulla saat automaattisesti kanssa. Huomaa, että onnistunut käyttöönotto kuuma valmius pieneen VMs, ja sitten skaalata ulos tarpeen mukaan.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa perustuu näkyvät [lisääminen luotettavuutta, N-taso-arkkitehtuuri Azure-](guidance-compute-n-tier-vm-linux.md)arkkitehtuuri. 

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on käytössä "Laske - Monisivuinen alue (Linux).

![[0]][0]

- **Ensisijaisen ja toissijaisen alueet**. Tämä arkkitehtuuri käyttää kummallakin alueella suurempi käytettävyys saavuttamiseksi. Yksi on ensisijainen alue. Tavallinen toimintojen aikana verkkoliikennettä reititetään ensisijainen alue. Mutta, joka ei ole käytettävissä, jos liikenne reititetään toissijainen alue.

- ** [Azure liikenteen hallinta] [ traffic-manager] ** reitittää pyynnöt ensisijainen alue. Jos alueen ei ole käytettävissä, liikenteen hallinta epäonnistuu toissijainen alueen yli. Lisätietoja on kohdassa [Määrittäminen liikenteen hallinta](#configuring-traffic-manager).

- **Resurssiryhmät**. Luoda erilliset [resurssiryhmien] [ resource groups] ensisijainen alue toissijainen alue- ja liikenteen hallinta. Näin voit hallita kunkin alueen yhden kokoelma resursseja joustavasti. Voi esimerkiksi käyttöön jonkin alueen tekemättä toinen alaspäin. [Linkki resurssiryhmät][resource-group-links], jotta voit suorittaa kyselyn Luettele kaikki sovelluksen resurssit.

- **VNets**. Luo erillisen VNet kunkin alueen. Varmista, että osoite välilyönnit eivät mene päällekkäin.

- **Apache Cassandra** otettu käyttöön tietojen keskikohdan mukaan Azure alueiden välillä. Eri Azure alueiden suuren Cassandra tietojen keskikohdan mukaan on otettu käyttöön. Kullakin alueella solmujen on määritetty vika Teline-aware tilaa ja Päivitä toimialueiden vikasietoisuudelle sen sisäpuolta.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten, ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="regional-pairing"></a>Alueellisten laiteparin

Azure alueittain on yhdistetty toiseen saman geography-alueella. Valitse alueet, saman alueellisen pair (esimerkiksi Itä US 2 ja US Keski). Tällöin etuja ovat:

- Jos näkyvissä on laaja käyttökatkosta, vähintään yhden alueen ulos jokaisen pair palautus on ensin.

- Suunniteltu Azure Järjestelmäpäivitykset tulevat esiin pisteparin alueiden järjestyksessä minimoi käyttökatkot mahdollista.

- Saman maantieteellisten tietojen pisimpään vaatimukset täyttävän sijaitsevat parit.

Varmista, että molemmat alueet tukevat kaikkia Azure palvelut, joita tarvitaan sovelluksen (katso [palvelut alueittain][services-by-region]). Saat lisätietoja alueellisen paria [liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR): Azure Parittainen alueiden][regional-pairs].

### <a name="traffic-manager-configuration"></a>Liikenteen hallinta määritys

Ota huomioon seuraavat seikat liikenne määritettäessä käyttämässäsi skenaariossa hallinta:

- **Reititys.** Liikenteen hallinta tukee useita [Reititys algoritmit][tm-routing]. Tässä artikkelissa kuvattuja skenaarion Käytä _prioriteetti_ reititys (niin sanottuja _automaattisesti_ reititys). Kun tämä asetus, liikenteen hallinta lähettää kaikki palvelupyynnöt ensisijainen alueen, paitsi jos ensisijainen alueen tulee saada yhteyttä. Tässä vaiheessa se siirtyy automaattisesti toissijaisen alueen. Katso [määrittäminen automaattisesti reititys menetelmä][tm-configure-failover].

- **Kuntotietojen näytteenottimen.** Liikenteen hallinta käyttää HTTP (tai HTTPS) [näytteenottimen] [ tm-monitoring] seurannassa kunkin alueen käytettävyyttä. Näytteenottimen tarkistaa HTTP 200-vastauksen, URL-polku. Paras käytäntö on luoda päätepiste, joka ilmoittaa sovelluksen yleinen kunto ja käyttää tämän päätepisteen keräysputken kunto. Muussa tapauksessa näytteenottimen ehkä ilmoittaa "kunnossa" päätepisteen, vaikka sovelluksen tärkeitä osista epäonnistuvat. Lisätietoja on artikkelissa [Kunto päätepisteen seuranta kuvion][health-endpoint-monitoring-pattern].

Kun liikenteen hallinta epäonnistuu päälle, käytössäsi on tietyn ajanjakson aikana, kun asiakkaat eivät pääse sovellus, joka voi olla useita minuutteja. Kaksi tekijöiden kokonaiskesto:

- Kuntotietojen Keräysputken on tunnistettava ensisijainen tietokeskuksen on tullut saavuttamattomissa.

- DNS-palvelimet on päivitettävä välimuistiin DNS-tietueet IP-osoitetta, joka määräytyy DNS--elinaika (TTL). Oletus-TTL on 300 sekunnin (5 minuuttia), mutta voit määrittää tämän arvon, kun luot liikenteen hallinta profiilin.

Lisätietoja on artikkelissa [Tietoja liikenteen hallinta seuranta][tm-monitoring].

Jos liikenteen hallinta ei onnistu päälle, on suositeltavaa suorittamiseen manuaalinen tuntisesta sijaan kaatuvat automaattisesti takaisin. Varmista, että kaikki sovelluksen alijärjestelmien ovat kunnossa ensin. Muussa tapauksessa voit luoda tilanteeseen, jossa sovellus kääntää edestakaisin välillä tietojen keskikohdan mukaan.

Oletusarvon mukaan liikenteen hallinta siirtyy automaattisesti takaisin. Voit estää tämän pienempi prioriteetti ensisijainen alueen automaattisesti tapahtuman jälkeen manuaalisesti. Oletetaan, että ensisijainen alue on prioriteetti 1 ja toissijaisen prioriteetti 2. Automaattisesti, kun Määritä prioriteetti 3, jos haluat estää automaattisen tuntisesta ensisijainen alue. Kun olet valmis, voit siirtyä takaisin, Päivitä prioriteetti 1.

Azure CLI seuraava komento päivittää prioriteetti:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

Toinen tapa välttää kiikku on tilapäisesti käytöstä päätepisteen:

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```    

Sen mukaan, automaattisesti syyn voit joutua muuttamaan redploy alueella resurssit. Ennen kaatuvat takaisin toiminnallisia valmius-testin suorittaminen Testi Tarkista kohteita, kuten:

- VMs on määritetty oikein. (Kaikki tarvittavat ohjelmistot on asennettu, IIS on käynnissä, jne.)

- Sovelluksen alijärjestelmien on kunnossa.

- Toimi testausta. (Esimerkiksi tietokannan taso on käytettävissä web-taso.)

### <a name="cassandra-deployment-across-multiple-regions"></a>Cassandra käyttöönoton useiden alueiden välillä

Cassandra tietojen keskukset ovat työmääriä jakokohtien: ryhmän liittyvät solmujen, joka määritetään yhdessä klusteri replikointi ja työmäärää eriytymistä varten.

Suosittelemme, että [DataStax yrityksen] [ datastax] tuotannon avulla. Katso lisätietoja suorittamisesta DataStax Azure [DataStax yrityksen Azure käyttöönotto-oppaassa][cassandra-in-azure]. Seuraavat Yleiset suositukset koskevat Cassandra mikä tahansa versio.

- Määritä kukin solmu julkiseen IP-osoite. Näin klustereiden käyttämällä Azure kuvaa runkoverkkoa infrastruktuuri antamisen hyvin pieni hintaan siirtonopeuden alueiden välillä.

- Suojatun solmujen haluamasi palomuurin ja NSG määritykset, sallimisen tietoliikenteen vain ja tunnetut isäntien, mukaan lukien asiakkaat ja klusterisolmujen. Huomaa, että Cassandra käyttää eri portit communication, OpsCenter, ohjattu ja niin edelleen. Portti käytön Cassandra, on artikkelissa [määrittäminen palomuurin portin][cassandra-ports].

- Käytä SSL-salausta kaikki [asiakkaan solmu] [ ssl-client-node] ja [solmu solmu] [ ssl-node-node] tietoliikenne.

- Alueella noudata [Cassandra suosituksia](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations).

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Monimutkainen N-taso-sovelluksen ei joudut ehkä replikoida toissijainen alueen koko sovellus. Sen sijaan kriittiset alijärjestelmää, joka tarvitaan tukemaan Liiketoiminnan jatkuvuus vain voi replikoida.

Liikenteen hallinta on mahdollista virhe järjestelmässä. Jos palvelun epäonnistuu, asiakkaat ei voi käyttää sovelluksen käyttökatkot aikana. Tarkista [Liikenteen hallinta SLA][tm-sla], ja määrittää liikenteen hallinnan yksin täyttääkö business-vaatimukset suuren käytettävyyden. Jos et, harkitse liikenteen hallintaratkaisu lisäämisellä tuntisesta. Jos Azure liikenteen hallinta-palvelu epäonnistuu, muuttaa CNAME-tietueet siten, että muut liikenteen hallinta-palvelun DNS. (Tämä vaihe on suoritettava manuaalisesti ja sovellus ei ole käytettävissä, kunnes DNS-muutokset välittyvät.)

Cassandra-klusterin automaattisesti muista käyttötavoista määräytyvät käyttää sovelluksen sekä määrä on käytetty yhdenmukaisuuden tasot. Yhdenmukaisuuden tasot ja Cassandra käyttö on artikkelissa [määrittäminen tietojen yhdenmukaisuuden] [ cassandra-consistency] ja [Cassandra: näkyvien solmujen ovat talked Quorum kanssa?][cassandra-consistency-usage] Cassandra tietojen käytettävyys määräytyy sovelluksen ja replikoinnin järjestelmä käyttää yhdenmukaisuuden taso. Katso Cassandra replikointi, [Tietojen replikointi NoSQL tietokantojen kuvaus][cassandra-replication].

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Kun päivität käyttöönoton, päivittää yhden alueen kerrallaan, pienenee Yleinen virhe virheellisistä määrityksistä tai virhe-sovelluksessa.

Testaa virheet järjestelmän vikasietoisuudelle. Tässä on muutamia yleisiä virheen skenaarioita Testaa:

- Sulje AM esiintymät.

- Paine resurssit, kuten suorittimen ja muistin.

- Irrota/viive-verkkoon.

- Kaatua prosessit.

- Päättyy varmenteet.

- Simuloida laitteiston virheitä.

- Toimialueen ohjauskoneen DNS-palvelun Sammuta.

Mittaa palautus ajat ja tarkista ne oman yrityksesi tarpeita vastaavan. Testaa epäonnistumisen tilat, sekä yhdistelmät.

## <a name="next-steps"></a>Seuraavat vaiheet

Sarjassa on kohdistettu täysin cloud ominaisuuksissa. Yrityksen skenaariot edellyttävät usein hybrid verkossa, yhteyden muodostaminen paikalliseen verkkoon Azure virtual verkoston kanssa. Lisätietoja näiden hybrid verkon muodostaminen on artikkelissa [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc-linux.png "Azure N tason sovellusten erittäin verkko-arkkitehtuuri"
