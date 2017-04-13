<properties
   pageTitle="Käynnissä Windows VMs useita alueilla suuren | Viittaus arkkitehtuuri | Microsoft Azure"
   description="Ottamisesta VMs Azure hyvän käytettävyyden ja vikasietoisuudesta useita alueilla."
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

# <a name="running-windows-vms-in-multiple-regions-for-high-availability"></a>Käynnissä Windows VMs suuren useita alueilla

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [Käynnissä Linux VMs suuren useita alueilla](guidance-compute-multiple-datacenters-linux.md)
- [Käynnissä Windows VMs suuren useita alueilla](guidance-compute-multiple-datacenters.md)

Tässä artikkelissa on suositeltavaa joukon käytäntöjä, suorita Windows näennäiskoneiden (VMs) useita Azure alueilla käytettävyys ja tehokkaat tietojen palauttaminen-infrastruktuuria.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource groups] ja perinteinen. Tässä artikkelissa käytetään Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Monille arkkitehtuuri tarjota suurempi kuin otat käyttöön vain yhden alueen käytettävyys. Jos alueellisen käyttökatkosta vaikuttaa ensisijainen alue, voit käyttää [Liikenteen hallinta] [ traffic-manager] epäonnistuu toissijainen alueen päälle. Tämä arkkitehtuuri auttaa myös, jos yksittäisiä alirakenne sovelluksen epäonnistuu. 

Yleiset monilla saavuttamiseksi suuren käytettävyyden tietojen keskikohdan mukaan eri tavoilla:

- Aktiivinen/passiivinen kuuma valmius kanssa. Liikenne siirtyy yhden alueen valmius muiden odottaa aikana. Toissijainen alueen VMs on kohdistettu ja käynnissä aina.

- Aktiivinen/passiivinen kylmän valmius kanssa. Sama, mutta VMs toissijainen alueen ei varata, kunnes tarvittavat automaattisesti. Tämän menetelmän kustannuksia pienempi, jos haluat suorittaa, mutta yleensä on enää alaspäin aikana epäonnistui.

- Aktiivisena. Molemmat alueet ovat aktiivisia ja pyynnöt ovat kuormitus tasataan niiden välille. Jos yksi tietokeskuksen ei ole käytettävissä, se otetaan kierto ulos. 

Tämä arkkitehtuuri ohjeessa on aktiivinen/passiivinen kuuma valmius-liikenne hallinnan avulla saat automaattisesti kanssa. Huomaa, että onnistunut käyttöönotto kuuma valmius pieneen VMs, ja sitten skaalata ulos tarpeen mukaan.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa perustuu näkyvät [lisääminen luotettavuutta, N-taso-arkkitehtuuri Azure-](guidance-compute-n-tier-vm.md)arkkitehtuuri.

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on käytössä "Laske - alueen (Windows) monisivuinen.

[! [0]][0]

- **Ensisijaisen ja toissijaisen alueet**. Tämä arkkitehtuuri käyttää kummallakin alueella suurempi käytettävyys saavuttamiseksi. Yksi on ensisijainen alue. Tavallinen toimintojen aikana verkkoliikennettä reititetään ensisijainen alue. Mutta, joka ei ole käytettävissä, jos liikenne reititetään toissijainen alue.

- ** [Azure liikenteen hallinta] [ traffic-manager] ** reitittää pyynnöt ensisijainen alue. Jos alueen ei ole käytettävissä, liikenteen hallinta epäonnistuu toissijainen alueen yli. Lisätietoja on kohdassa [Määrittäminen liikenteen hallinta](#configuring-traffic-manager).

- **Resurssiryhmät**. Luoda erilliset [resurssiryhmien] [ resource groups] ensisijainen alue toissijainen alue- ja liikenteen hallinta. Näin voit hallita kunkin alueen yhden kokoelma resursseja joustavasti. Voi esimerkiksi käyttöön jonkin alueen tekemättä toinen alaspäin. [Linkki resurssiryhmät][resource-group-links], jotta voit suorittaa kyselyn Luettele kaikki sovelluksen resurssit.

- **VNets**. Luo erillisen VNet kunkin alueen. Varmista, että osoite välilyönnit eivät mene päällekkäin. 

- **SQL Server aina käytettävyys-ryhmä**. Jos käytössäsi on SQL Server, on suositeltavaa [SQL aina käyttöön Availabilty ryhmien] [ sql-always-on] suuren. Luo yksittäisen käytettävyys-ryhmä, joka sisältää SQL Server-esiintymien molemmat alueet. Lisätietoja on kohdassa [määrittäminen SQL Server aina käyttöön käytettävyys-ryhmä](#configuring-the-sql-server-alwayson-availability-group ).

> [AZURE.NOTE] Kannattaa myös [Azure SQL-tietokanta][azure-sql-db], joka sisältää suhteellisen tietokannan pilvipalveluun nimellä. SQL-tietokantaan ei tarvitse määrittää käytettävyys-ryhmän tai hallita automaattisesti.  

- **VPN yhdyskäytävien**: [VPN-yhdyskäytävän] luominen[ vpn-gateway] kunkin VNet- ja [VNet VNet-yhteyden]määrittäminen[vnet-to-vnet], käyttöön verkkoliikennettä kahden VNets välillä. Tämä on pakollinen SQL aina käyttöön käytettävyys-ryhmän.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten, ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="regional-pairing"></a>Alueellisten laiteparin

Azure alueittain on yhdistetty toiseen saman geography-alueella. Valitse alueet, saman aluekohtaiset pair (esimerkiksi Itä US 2 ja US Keski). Tällöin etuja ovat:

- Jos näkyvissä on laaja käyttökatkosta, vähintään yhden alueen ulos jokainen palautus on ensin.

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

Oletusarvon mukaan liikenteen hallinta siirtyy automaattisesti takaisin. Voit estää tämän pienempi prioriteetti ensisijainen alueen automaattisesti tapahtuman jälkeen manuaalisesti. Oletetaan, että ensisijainen alue on prioriteetti 1 ja toissijaisen prioriteetti 2. Automaattisesti, kun Määritä prioriteetti 3, jos haluat estää automaattisen tuntisesta ensisijainen alueen. Kun olet valmis, voit siirtyä takaisin, Päivitä prioriteetti 1.

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

### <a name="sql-server-always-on-configuration"></a>SQL Server aina-määritys

SQL Server aina käyttöön käytettävyys ryhmät edellyttää toimialueen ohjauskoneen. Kaikki solmut käytettävyys-ryhmässä on oltava sama AD-toimialueen. Seuraavat asiat on SQL Server aina käyttöön käytettävyys ryhmän määrittämisestä koskevat ohjeet:

- Aseta vähintään kahden toimialueen ohjauskoneen kunkin alueen. 

- Anna kunkin toimialueen ohjauskoneen staattinen IP-osoite.

- Ottaa käyttöön VNets välisen yhteyden VNet VNet.

- Lisää kunkin VNet IP-osoitteiden luetteloon DNS server (molemmat alueelta) ohjaimet toimialueen. Voit käyttää seuraavaa CLI-komentoa. Lisätietoja-kohdassa [Hallitse DNS-palvelimet käyttämä virtual verkon (VNet)][vnet-dns].

```bat
azure network vnet set --resource-group dc01-rg --name dc01-vnet --dns-servers "10.0.0.4,10.0.0.6,172.16.0.4,172.16.0.6"
```

- Luo [Windows Server-vikasietoklustereihin] [ wsfc] (WSFC)-klusteriin, joka sisältää SQL Server-esiintymien molemmat alueet. 

- Luo SQL Server aina käyttöön käytettävyys ryhmä, joka sisältää SQL Server-esiintymien ensisijainen ja toissijainen-alueilla. Saat ohjeita [Laajentaminen aina käyttöön käytettävyys ryhmän Remote Azure palvelinkeskukseen (PowerShell)](https://blogs.msdn.microsoft.com/sqlcat/2014/09/22/extending-alwayson-availability-group-to-remote-azure-datacenter-powershell/) . 

- Valitse ensisijainen se ensisijainen alue.

- Valitse yksi tai useampi toissijainen replikoida ensisijainen alue. Määritä nämä synkronisen Vahvista käytettäväksi automaattinen automaattisesti.

- Sijoita yksi tai useampi toissijainen replikoida toissijainen alue. Määritä nämä *asynkroninen* Vahvista käytettävät suorituskyvyn parantamiseksi. (Muussa tapauksessa kaikki SQL-tapahtumat on odottaa matkassa toissijainen alueen verkossa.) 

> [AZURE.NOTE] Asynkroninen Vahvista replikat eivät tue automaattinen automaattisesti. 

Lisätietoja on artikkelissa [Azure-N tason arkkitehtuuri Windows VMs käynnissä](guidance-compute-n-tier-vm.md#SQL-AlwaysOn-Availability-Group).

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Monimutkainen N-taso-sovelluksen ei joudut ehkä replikoida toissijainen alueen koko sovellus. Sen sijaan kriittiset alijärjestelmää, joka tarvitaan tukemaan Liiketoiminnan jatkuvuus vain voi replikoida.

Liikenteen hallinta on mahdollista virhe järjestelmässä. Jos palvelun epäonnistuu, asiakkaat ei voi käyttää sovelluksen käyttökatkot aikana. Tarkista [Liikenteen hallinta SLA][tm-sla], ja määrittää tietoliikenteen hallinnan yksin täyttääkö business-vaatimukset suuren käytettävyyden. Jos et, harkitse liikenteen hallintaratkaisu lisäämisellä tuntisesta. Jos Azure liikenteen hallinta-palvelu epäonnistuu, muuttaa CNAME-tietueet siten, että muut liikenteen hallinta-palvelun DNS. (Tämä vaihe on suoritettava manuaalisesti ja sovellus ei ole käytettävissä, kunnes DNS-muutokset välittyvät.) 

SQL Server-klusterin on automaattisesti kuvauksista ottaa huomioon:

1. Kaikki ensisijainen alueen SQL-replikat epäonnistuu. Esimerkiksi Tämä voi johtua alueellisen käyttökatkosta aikana. Siinä tapauksessa voit on manuaalisesti epäonnistua ryhmästä SQL käytettävyys vaikka liikenteen hallinta epäonnistuu automaattisesti päälle edustatietokanta. [Suorittaa pakotettu manuaalinen automaattisesti SQL Server käytettävyys ryhmän](https://msdn.microsoft.com/library/ff877957.aspx), jossa kerrotaan, miten voit suorittaa pakotettu automaattisesti käyttämällä SQL Server Management Studiossa, Transact-SQL- tai PowerShell SQL Server 2016 noudattamalla. 

    > [AZURE.WARNING] Pakotetun automaattisesti, jossa on tietojen menettämisen riskiä. Kun ensisijainen alue on online-tilaan, ota tilannevedos tietokannan ja Etsi erot [tablediff] avulla.

2. Liikenteen hallinta siirtyy toissijaisen alueen, mutta ensisijainen SQL-se on edelleen käytettävissä. Edustatietokannan taso voi epäonnistua esimerkiksi vaikuttamatta SQL-VMs. Tässä tapauksessa Internet-liikenne reititetään toissijainen alue ja alueen edelleen muodostaa yhteyden SQL-se ensisijainen. Kuitenkin on parannettu viive, koska SQL-yhteydet sujuvat eri alueilla. Tässä tilanteessa suorittama manuaalinen automaattisesti seuraavasti: 

    - Siirtyy *painikkeen* Vahvista väliaikaisesti toissijainen alueen SQL sille myönnetään. Tällä varmistetaan, että vikasietotilaa aikana ei ole tietojen menettämisen.

    - Epäonnistua SQL replika päälle. 
    
    - Kun takaisin ensisijainen alue, Palauta asynkroninen Vahvista-asetus. 

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Kun päivität käyttöönoton, päivittää yhden alueen kerrallaan, pienenee Yleinen virhe virheellisistä määrityksistä tai virhe-sovelluksessa.

Testaa virheet järjestelmän vikasietoisuudesta. Tässä on muutamia yleisiä virheen skenaarioita Testaa:

- Sulje AM esiintymät.

- Paine resurssit, kuten suorittimen ja muistin.

- Irrota/viive-verkkoon.

- Kaatua prosessit.

- Päättyy varmenteet.

- Simuloida laitteiston virheitä.

- Toimialueen ohjauskoneen DNS-palvelun Sammuta.

Mittaa palautus ajat ja tarkista ne oman yrityksesi tarpeita vastaavan. Testaa virheen tilat, sekä yhdistelmät.

## <a name="next-steps"></a>Seuraavat vaiheet

Sarjassa on kohdistettu täysin cloud ominaisuuksissa. Yrityksen skenaariot edellyttävät usein hybrid verkossa, yhteyden muodostaminen paikalliseen verkkoon Azure virtual verkoston kanssa. Lisätietoja näiden hybrid verkon muodostaminen on artikkelissa [soveltamisesta Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN][hybrid-vpn].

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[azure-sql-db]: https://azure.microsoft.com/en-us/documentation/services/sql-database/
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[sql-always-on]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
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
[0]: ./media/blueprints/compute-multi-dc.png "Azure N tason sovellusten erittäin verkko-arkkitehtuuri"