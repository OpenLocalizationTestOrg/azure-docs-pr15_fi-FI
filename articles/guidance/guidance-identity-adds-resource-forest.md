<properties
   pageTitle="Arkkitehtuuri-viittaus Azure - IaaS: Luominen Azure Active Directory-resurssien metsää | Microsoft Azure"
   description="Voit luoda luotetun Active Directory-toimialueen Azure-tietokannassa."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
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

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Azure Active Directory Directory Services (Lisää)-resurssin metsää luominen

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan, miten luominen, joka on erillään mutta luotettujen, Azure Active Directory-toimialueen paikallisen metsää toimialueita.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Viite-arkkitehtuuri käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

Organisaation, joka suoritetaan, Active Directory (AD) paikallisen on ehkä metsää, joka sisältää useita eri toimialueiden. Voit luoda yksittäisen toimialueiden osastoille tai suborganizations, tai uudet toimialueet on lisätty hankinta tai muiden organisaatioiden fuusion. Toimialueiden avulla voit antaa toiminta-alueet, jotka on säilytettävä erillisessä mahdollisesti tietoturvasyistä välinen eristystaso, mutta voit jakaa tietoja luomalla luottamussuhteet toimialueiden välillä.

Organisaatio, joka käyttää toimialueille voit hyödyntää Azure kunnolliselle pilveen vähintään yksi näistä toimialueista. Voit myös organisaation haluta säilyttää kaikki cloud resurssit loogisesti poikkeavat näiden vastuuseen paikallisen ja tallentaa oman hakemistossa pidetään myös pilveen toimialueen cloud resurssien tietoja.

Voit ottaa käyttöön Active Directory-Azure usealla eri tavalla kuvatulla tavalla on artikkeleissa [Laajentaminen Azure Active Directory] [ extending-ad-to-azure] ja [Toteuttaminen Azure Active Directoryn][implementing-aad]. Tämän asiakirjan ohjeessa on yksi tietty skenaario: luominen pilvipalvelussa, jotka poikkeavat minkä tahansa pidetään paikallisen toimialueen, mutta, joka voi olla paikallisen toimialueiden luottamussuhde toimialueeseen. 

Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Ylläpito objektien ja käyttäjätiedot säilytetään pilvipalveluun erottaminen suojaus.

- Siirtyminen yksittäisiä toimialueita: paikallisen pilveen.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tärkeitä osia tämän arkkitehtuuri (*Lähennä*). Lue lisätietoja harmaana ulos-osista [käyttöönoton suojatun hybrid-verkoston arkkitehtuuri Azure-tietokannassa] [ implementing-a-secure-hybrid-network-architecture] ja [toteuttaminen suojatun hybrid verkko-arkkitehtuuri Internet-yhteyden Azure][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **Paikalliseen verkkoon.** Paikallisen verkon on oma AD metsää ja toimialueet.

- **AD-palvelimiin.** Nämä ovat toimialueen ohjaimet käyttöönoton hakemistopalvelujen (AD DS) käynnissä VMs pilveen. Seuraavissa palvelimissa ylläpitää metsää, joka sisältää vähintään yhden toimialueiden erillään ne sijaitsevat paikallisen.

- **Yksisuuntainen luottamussuhde.** Kaavion esimerkissä yksisuuntainen luottamussuhde toimialueesta paikallisen toimialueen pilveen. Tätä yhteyttä käyttäjät paikallisen toimialueen pilveen, mutta ei tavalla ympärille resurssien käytön. Tämä on yleisiä tapauksessa. Voit luoda kaksisuuntaista luottamusta, jos cloud käyttäjät tarvitsevat myös paikallisen resurssien käytön.

- **Active Directory-aliverkon.** Erillinen aliverkon isännöidään AD DS-palvelimiin. NSG säännöt suojaa AD DS-palvelimet, ja se voi tarjota palomuuri vastaan odottamattomia lähteistä.

- **Web-tason aliverkon**, **Business taso aliverkon**ja **tietojen taso aliverkon**. Nämä aliverkosta ylläpitää palvelinten ja osat, jotka suoritetaan sovellusten pilveen. Lisätietoja on artikkelissa [Käynnissä VMs, N-taso-arkkitehtuuri Azure-][running-VMs-for-an-N-tier-architecture-on-Azure]. Resurssien ja tämän aliverkon VMs sisältyvät cloud-toimialue.

- **Azure yhdyskäytävä**. Azure yhdyskäytävä on paikalliseen verkkoon ja Azure VNet välinen yhteys. Tämä voi olla [VPN-yhteyden] [ azure-vpn-gateway] tai [Azure ExpressRoute][azure-expressroute]. Lisätietoja on artikkelissa [käyttöönoton suojatun hybrid-verkoston arkkitehtuuri Azure-tietokannassa][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Suosituksia

Tässä osassa on luettelo olennainen osat, joita tarvitaan toteuttamisesta basic arkkitehtuuri perustuvia suosituksia. Näiden suositusten aiheet:

- Lisää asetuksia ja

- Luota suhteen määritys.

Voit joutua ylimääräisiä tai eri Tässä kuvattuja vaatimuksia. Voit käyttää kohteiden sisältö lähtökohtana aihetta mukauttamisesta oman järjestelmän arkkitehtuuri.

### <a name="adds-recommendations"></a>Lisää suositukset

Suosituksia yksityiskohtaisista pilveen Active Directory-viitata asiakirjan [Laajentaminen Azure Active Directoryn][extending-ad-to-azure]. Artikkelin [ohjeet käyttöönotto Windows Server Active Directory-Azuren näennäiskoneiden] [ ad-azure-guidelines] sisältää muitakin tietoja.

### <a name="trust-recommendations"></a>Luota suositukset

Paikallisen toimialueen sisältyvät eri metsää pilveen toimialueista. Paikalliset käyttäjät pilvipalvelussa todennus käyttöön pilveen toimialueet on luotettava paikallisen metsää Windows NT-toimialue. Vastaavasti jos pilveen tarjoaa kirjautuminen toimialueen ulkoisille käyttäjille, se saattaa olla tarpeen paikallisen metsää cloud toimialueen.

Voit muodostaa luottamussuhteet metsää tasolla luomalla [metsää luottamussuhteet][creating-forest-trusts], tai toimialueen luomalla [Ulkoiset luottamussuhteet]tasolla[creating-external-trusts]. Metsää tason luota Luo kaikki toimialueet kaksi metsien suhteen. Tason ulkoisen toimialuenimen-luota Luo vain kaksi määritettyä toimialuetta suhteen. Sinun on luotava vain ulkoisen toimialuenimen tason luottamussuhteet eri toimialuepuuryhmiin toimialueiden välillä.

Luottamussuhteet voi olla yksisuuntainen (yksisuuntainen) tai kaksisuuntainen (kahdenvälinen):

- Yksisuuntainen luottamussuhde avulla käyttäjät yhdessä toimialueen tai metsää (nimeltään *saapuvien* toimialueen tai metsää) toisen pidetään resurssien käytön ( *Lähtevän* toimialueen tai metsää). 

- Kaksisuuntaisen luottamuksen avulla käyttäjä toimialueen tai metsää pidetään myös toisessa resurssien käytön.

Seuraavassa taulukossa on lueteltu joitakin yksinkertaisia skenaarioita luota määritykset:

| Skenaario | Paikallisen luota | Cloud luota |
|----------|-------------------|-------------|
| Paikallisen käyttäjät tarvitsevat resurssien pilvipalvelussa, mutta ei päinvastoin | Yksisuuntainen, saapuva | Yksisuuntainen, lähtevän |
| Käyttäjät pilvipalvelussa tarvitsevat käyttöoikeudet resursseja, joka sijaitsee paikallisen, mutta ei päinvastoin | Yksisuuntainen, lähtevän | Yksisuuntainen, saapuva |
| Käyttäjien pilveen ja paikallisen edellyttää pidetään cloud ja paikallisen resurssien käytön | Kaksisuuntainen, saapuvat ja lähtevät | Kaksisuuntainen, saapuvat ja lähtevät |

## <a name="security-considerations"></a>Suojausasiat

Metsää tason luottamussuhteet ovat transitiiviverbien. Jos muodostat metsää tason luota paikallisen metsää ja metsää pilveen, tämä luota laajennetaan muiden joko metsää luodaan uusi toimialueiden. Jos käytät toimialueiden erottaminen tietoturvasyistä, kannattaa ehkä luoda luottamussuhteet vain tason toimialue. Toimialueen tason luottamussuhteet ovat muun siirtävät.

AD-kohtaisia suojausasiat on kohdassa *suojausasiat* [laajentaminen Azure Active]Directory[extending-ad-to-azure].

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Ota käyttöön vähintään kahden toimialueen ohjauskoneen kunkin toimialueen osalta. Ottaa käyttöön automaattisen replikoinnin palvelinten välillä. Luo visuaalisessa muodossa AD-palvelimet käsittely kunkin toimialueen VMs määrittäminen saatavuus. Varmista, että joukon ovat vähintään kaksi palvelimiin. 

Ota huomioon myös nimeävät yhden tai usean palvelimen kunkin toimialueen kuin [valmiustilassa operations Master] [ standby-operations-masters] siltä varalta, että yhteys palvelimeen, visuaalisessa muodossa FSMO-roolin epäonnistuu.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

AD on automaattisesti skaalattava toimialueen ohjaimet, jotka ovat osa samaa toimialuetta. Pyynnöt jakautuu kaikkien ohjainten toimialueella. Voit lisätä toisen toimialueen ohjauskoneen, ja se synkronoi automaattisesti toimialueen kanssa. Erillinen kuormituksen ohjaamaan liikenne paikalliseen ohjaimet toimialueessa ei määritetä. Varmistaa, että kaikki toimialueen ohjaimet on riittävä muistin ja tallennustilaa resurssien käsittelemään toimialueen tietokantaan. Muuttaa kaikki toimialueen ohjauskoneen VMs samankokoisiksi.

## <a name="management-and-monitoring-considerations"></a>Hallinta ja seuranta huomioon otettavia seikkoja

Lisätietoja hallintaan ja valvontaan huomioon otettavia seikkoja on kohdissa vastaavat [laajentaminen Azure Active]Directory[extending-ad-to-azure]. 

Saat lisätietoja artikkelista [Seuranta Active Directory][monitoring_ad]. Voit asentaa työkaluilla, kuten [Microsoft Systems Centerin] [ microsoft_systems_center] seurannan palvelimessa hallinta aliverkon avulla näistä tehtävistä. 

## <a name="solution-components"></a>Ratkaisun osat

Esimerkki ratkaisu komentosarja [Käyttöönotto ReferenceArchitecture.ps1][solution-script], on käytettävissä, voit toteuttaa arkkitehtuuri, joka on tässä artikkelissa kuvattuja suosituksia. Tämä komentosarja käyttämällä Azure Resurssienhallinta malleja. Joukko keskeisiä rakenneosien kukin ottavat suorittaa tietyn toiminnon, kuten VNet luomista tai määrittämistä NSG käytettävissä olevat mallit. Komentosarjan tarkoituksena on orchestrate mallin käyttöönottoa.

Mallit ovat Parametroitu parametreilla erillisessä JSON-tiedostoissa. Voit muokata näitä tiedostoja määrittäminen vastaamaan omia tarpeita käyttöönoton parametrit. Sinun ei tarvitse muuttamiseksi itse malleja. Et saa muuttaa parametritiedostot objektit rakenteet.

Mallien muokkaaminen, luominen objekteja, jotka noudattavat kuvattu [Suositellut nimeäminen nimeämiskäytännön Azure resurssien]nimeämiskäytännöt[naming-conventions].

Näyte Luo ja määrittää ympäristössä, joka sisältää nimeltä *treyresearch.com*toimialueen pilveen. Tässä ympäristössä käsittää Lisää aliverkon ja palvelimissa, DMZ, web taso, business taso ja tietojen käytön osia, VPN-yhdyskäytävän ja hallinnan taso. Näyte myös vaihtoehtoinen määritys luomiseen Simuloitu paikalliseen ympäristöön oman toimialueen *contoso.com*. Ratkaisu sisältää komentosarjan, joka muodostaa luottamussuhde nämä toimialueilla, paikallisen käyttäjät voivat käyttää objektien *treyresearch.com* toimialueen pilveen.

Seuraavissa osissa kuvaillaan elementit paikallisia, sekä cloud määrityksiä.

### <a name="on-premises-components"></a>Paikallisen osat

>[AZURE.NOTE] Komponentit eivät ole arkkitehtuuri, joka on kuvattu tämän asiakirjan ensisijaisen kohdistuksen ja annetaan vain, jos haluat antaa sinulle mahdollisuuden Testaa cloud-ympäristössä turvallisesti reaali tuotantoympäristössä sijaan. Tästä syystä tämän osan yhteenveto vain avaimen parametritiedostot. Voit muuttaa asetuksia, kuten IP-osoitteet tai VMs koot, mutta se on suositeltavaa, että jätä monet muut parametrit ei muutu.

Tässä ympäristössä käsittää AD-metsää toimialueen *contoso.com* . Toimialueen sisältää kaksi IP-osoitteita 192.168.0.4 ja 192.168.0.5 Lisää palvelimiin. Nämä kaksi palvelimet myös suorittamalla DNS-palvelusta. Paikallisen järjestelmänvalvojatilin sekä VMs kutsutaan `testuser` salasanalla `AweS0me@PW`. Lisäksi määritykset määrittää VPN-yhdyskäytävän muodostamisesta VNet pilveen. Voit muokata määritykset muokkaamalla seuraavat JSON tiedostot sijaitsevat [**Parametrit/paikallisesti käytettävät versiot**] [ on-premises-folder] kansiossa:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Tämä tiedosto määrittää paikallisen ympäristön verkko-osoitetilaa varten.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Tiedoston nimi sisältää Lisää isännöintipalveluina paikallisen VMs määrityskohde. Kaksi *Vakio-DS3-v2* VMs luodaan oletusarvon mukaan.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** ja ** [connection.parameters.json][on-premises-connection-parameters]**. Nämä tiedostot pidä Azure VPN-yhdyskäytävän VPN-yhteyden asetusten pilvipalvelussa, mukaan lukien jaetun käytettävän suojaamaan tietoliikennettä sallita ohittaa yhdyskäytävän avaimen.

Jäljellä olevat tiedostot-kansiossa sisältää paikallisen toimialueen (*contoso.com*) käyttämällä tämän infrastruktuurin luomiseen käytetyt tiedot. Asenna lisää DNS-asetukset niiden avulla ja luoda paikallisen metsää.

Ratkaisu käyttää seuraavaa komentosarjaa-nimisen [Saapuvan postin trust.ps1][incoming-trust], joka suoritetaan tietokoneessa paikallisen toimialueen:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Tämä komentosarja Lisää AD DS-palvelimien IP-osoitteiden pilvipalvelussa (katso seuraava kohta) paikalliset DNS-palveluun, ja käyttää sitten [Netdom-apuohjelmien avulla] [ netdom] komento saapuvien yksisuuntainen luottamussuhde luominen toimialueen pilvipalvelussa (*treyresearch.com*).

### <a name="cloud-components"></a>Cloud osat

Nämä osat muodostavat tämän arkkitehtuuri core. Ne infrastruktuurin *treyresearch.com* toimialueen määritys ja luottamussuhteet luo paikallisen *contoso.com* -toimialueella. [**Parametrien/azure**] [ azure-folder] kansio sisältää seuraavat parametritiedostot määrittäminen komponentit:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Tämä tiedosto määrittää rakenteen VNet VMs ja muita osia pilveen. Se sisältää asetuksia, kuten nimi, osoitetilaa, aliverkosta ja minkä tahansa DNS-palvelimet tarvittavat osoitteet. DNS-osoitteet näkyvät tässä esimerkissä viittaus paikalliset DNS-palvelimet ja oletusarvon Azure DNS-palvelimen IP-osoitteet. Muokata näitä osoitteita viittaamaan omia DNS-asetukset, jos et käytä otoksen paikallisen ympäristön seuraavasti:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Tämä tiedosto määrittää virtuaalilaitteiksi Lisää pilveen. Kokoonpanon koostuu kahdesta VMs. Järjestelmänvalvojan käyttäjänimeä ja salasanaa `virtualMachineSettings` osa ja voit halutessasi muuttaa AM kokoa vastaamaan toimialueen vaatimukset:

    Lisätietoja on artikkelissa [Laajentaminen Azure Active Directoryn][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [Lisää-Lisää-toimialue-controller.parameters.json][add-adds-domain-controller-parameters]**. Tiedoston nimi sisältää asetuksia kestävät Lisää palvelimet *treyresearch.com* toimialueen luomiseen. Tiedostossa käytetään mukautettuja laajennuksia, joka vahvistaa toimialueen ja lisätä siihen lisää-palvelimiin. Ellet luo muita Lisää servers (Tässä tapauksessa kannattaa lisätä ne `vms` matriisi), muuttaa niiden oletusarvoisen tai haluat luoda toimialueen eri nimellä, sinun ei tarvitse tehdä tämän tiedoston muokkaamiseen.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Tiedoston nimi sisältää asetukset, joilla luominen Azure VPN-yhdyskäytävän pilvipalvelussa käytettävä Muodosta yhteys paikalliseen verkkoon. Muokkaa `sharedKey` arvo `connectionsSettings` osan vastaamaan paikallisen VPN-laite. Lisätietoja on artikkelissa [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** ja ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Nämä tiedostot Määritä saapuvan (julkinen) ja lähtevien (yksityinen), jotka muodostavat DMZ suojaaminen pilveen palvelimet VMs reunassa. Saat lisätietoja elementit ja niiden määritykset [käyttöönoton DMZ Azure ja Internetin välille][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters.json][loadBalancer-biz-parameters]**, ja ** [loadBalancer data.parameters.json][loadBalancer-data-parameters]**. Parametrit-tiedostot sisältävät access web ja business-tasoa AM mukaisesti ja Määritä kuormituksen tasoitusmääritykset kunkin tason. Nämä ovat VMs, joka isännöi web Apps-sovellusten ja tietokantojen ja suorita business työmääriä organisaation. Web-taso-VMs lisätään pilveen toimialueen käyttämällä määritettyjä asetuksia ** [web-AM-toimialue-join.parameters.json] [ web-vm-domain-join-parameters] ** tiedosto.

    Kunkin tiedosto sisältää kaksi joukkoa määrityksiä. `virtualMachineSettings` Osa määrittää VMs, jotka isännöivät pilveen ADFS-palvelun. Oletusarvon mukaan komentosarja luo näistä VMs käytettävyys samat. Seuraavat osat Näytä *loadBalancer web.parameters.json* tiedoston haluamasi osat:

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    Voit muokata kokojen ja määrä kunkin tason VMs tarpeen mukaan.

    `loadBalancerSettings` Osassa on kuvaus kuormituksen näiden VMs. Kuormituksen välittää liikenteestä, joka näkyy portin 80 (HTTP) ja porttiin 443 (HTTPS) tai muu VMs. 

    >[AZURE.NOTE] Portin 80 sääntöä käytetään TCP-yhteyden sijaan HTTP. Tämä johtuu siitä web ylätasolla IIS: n asennus on määritetty tukemaan Windows-todennuksen vain. Anonyymi todentaminen on poistettu käytöstä. Yritetään *ping* portti 80 HTTP-yhteyden kautta epäonnistuu 401 (luvattoman) virhe, olisi TCP-yhteyden avulla vain havaitsee onko portti käytössä:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Tiedoston nimi sisältää tekstin valinta ja hallinta VMs määrityskohde. Vain on mahdollista saada kirjautuminen ja järjestelmänvalvojan oikeudet web, business ja tietojen tasoa VMs tekstin-ruutuun. Oletusarvon mukaan komentosarja luo yksittäinen *Standard_DS1_v2* AM, mutta voit muokata luoda suurentaa tai muita VMs, jos hallinta työmäärää on todennäköisesti merkittävä tiedosto.

Kokoonpanon käyttää myös [Lähtevän trust.ps1] [ outgoing-trust] komentosarjan luominen Yksisuuntainen lähtevä luottamus *contoso.com* -toimialueella alla:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Tämä komentosarja on samanlainen kuin *Saapuvan postin trust.ps1* komentosarja edempänä. Lisää IP-osoitteet paikallisia AD DS-palvelinten paikallisen DNS-palvelun ja käyttää sitten [Netdom-apuohjelmien avulla] [ netdom] komento lähtevän luottamussuhde luomiseen.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Ratkaisu oletetaan, että seuraavat edellytykset:

- Sinulla on aiemmin Azure tilaus, jossa voit luoda resurssiryhmiä.

- Olet ladannut ja asentanut PowerShellin Azure uusin versio. Katso [tähän] [ azure-powershell-download] ohjeita.

Voit suorittaa komentosarjan, joka ottaa ratkaisun käyttöön seuraavasti:

1. Siirrä helposti kansioon paikalliseen tietokoneeseen ja luo seuraavat alikansiot:

    - Komentosarjojen

    - Komentosarjojen ja parametrit

    - Komentosarjojen/parametrit/paikallisesti käytettävät versiot

    - Komentosarjojen/parametrit/Azure

2. Lataa [Käyttöönotto ReferenceArchitecture.ps1] [ solution-script] tiedosto komentosarjat-kansioon

3. Lataa [Parametrit/paikallisesti käytettävät versiot] sisällön[ on-premises-folder] kansio komentosarjojen/parametrit/paikallisesti käytettävät versiot kansioon:

4. Lataa [Parametrit/azure] sisällön[ azure-folder] kansio komentosarjojen/parametrit/Azure-kansioon.

5. Muokkaa komentosarjat-kansiossa käyttöönotto ReferenceArchitecture.ps1-tiedostoa ja muuta seuraavat viivoja ja määritä resurssiryhmiä, jotka on luotu tai käyttää luoma komentosarja resurssit:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. Muokkaa parametrin tiedostoja komentosarjojen/parametrit/paikallisesti käytettävät versiot ja komentosarjojen/parametrit/Azure-kansioista. Päivitä resurssin ryhmän viittaukset nämä tiedostot muuttujien käyttöönotto ReferenceArchitecture.ps1-tiedostoon liitetty resurssiryhmien nimet. Seuraavassa taulukossa on kuvattu, parametritiedostot viittaus mihin resurssiryhmä. *Ra-adfs-työmäärää-rg*, *ra-adfs-suojaus-rg*, *ra-adfs-Lisää-rg*, *ra-adfs-adfs-rg*ja *ra-adfs-välityspalvelimen-rg* resurssiryhmät käytetään vain PowerShell-komentosarjaa ja ilmene parametri-tiedostoja.

  	|Resurssiryhmä|Parametritiedostot|
  	|--------------|--------------|
  	|Ra-adtrust-paikallisesti käytettävät versiot-rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-adds.parameters.JSON<br />parameters\onpremise\virtualNetwork-adds-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|Ra-adtrust-verkko-rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-public.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-Web.parameters.JSON<br />parameters\azure\virtualMachines-adds.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-adds-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*kaksi*)

    Lisäksi määritys paikalliset ja cloud osat kuvatulla tavalla [Ratkaisun osien] [ solution-components] osa.

7. Avaa PowerShellin Azure-ikkuna, komentosarjat-kansioon ja suorita seuraava komento:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Korvaa `<subscription id>` käyttämällä Azure tilauksen.

    Saat `<location>`, Määritä Azure alue, kuten `eastus` tai `westus`.

    `<mode>` Parametri voi olla jokin seuraavista arvoista:

    - `Onpremise`, luomiseen Simuloitu paikalliseen ympäristöön.

    - `Infrastructure`, luomiseen VNet infrastruktuuri ja siirtyä ruutuun pilveen.

    - `CreateVpn`, voivat laatia Azure virtual yhdyskäytävien ja muodosta yhteys paikalliseen verkkoon.

    - `AzureADDS`, visuaalisessa muodossa Lisää palvelinten VMs muodostaminen Active Directory käyttöön näiden VMs ja luo toimialueen pilveen.

    - `WebTier`, joka luo sivuston VMs taso ja kuormituksen.

    - `Prepare`, joka suorittaa edeltävät tehtävät. **Tämä on suositeltavaa vaihtoehtoa, jos kokoat kokonaan uuden käyttöönotto ja sinulla ei ole olemassa olevan paikallisen-infrastruktuurin.**

    - `Workload`Voit luoda business ja tietojen tason VMs ja kuormituksen tasaajiksi. Nämä VMs eivät ole osana `Prepare` vaihtoehto.

    >[AZURE.NOTE] Jos käytössäsi `Prepare` vaihtoehto-komentosarjan suorittamiseen kuluvaa useita tunteja.

8.  Jos käytössäsi on esimerkki paikallisen määritykset:

    1. Muodosta yhteys tekstin-ruutuun (*ra-adtrust-hallintoraportointi-vm1* *ra-adtrust-suojaus-rg* resurssiryhmän). Kirjaudu sisään *testuser* salasanalla *AweS0me@PW*.

    2.  Avaa Siirry-ruutu ensimmäisen AM RDP istunnon *contoso.com* -toimialueella (paikallisen toimialueen). Tämä AM on IP-osoite 192.168.0.4. Käyttäjänimi on *contoso\testuser* salasanalla *AweS0me@PW*.

    3. Lataa [Saapuvan postin trust.ps1] [ incoming-trust] komentosarja ja Suorita Luo saapuva luottamus *treyresearch.com* toimialueesta.

9. Jos käytössäsi on paikallisen-infrastruktuurin:

    1. Lataa [Saapuvan postin trust.ps1] [ incoming-trust] komentosarjan.

    2. Muokkaa komentosarjaa ja korvaa arvo `$TrustedDomainName` muuttujan oman toimialueen nimi.

    3. Komentosarjan suorittaminen

10. Siirry-ruutu Muodosta yhteys ensimmäisen AM *treyresearch.com* toimialueen (pilvipalvelussa toimialue). Tämä AM on IP-osoite 10.0.4.4. Käyttäjänimi on *treyresearch\testuser* salasanalla *AweS0me@PW*.

11. Lataa [Lähtevän trust.ps1] [ outgoing-trust] komentosarja ja Suorita Luo saapuva luottamus *treyresearch.com* toimialueesta. Jos käytössäsi on oma paikallisen koneet, valitse Muokkaa komentosarja ensin. Määrittää `$TrustedDomainName` muuttujan paikallisen toimialueen nimi ja määritä tämän toimialueen AD DS-palvelimien IP-osoitteet `$TrustedDomainDnsIpAddresses` muuttuja.

12. Paikallisen-koneessa toimien artikkelissa [Tarkista luota] [ verify-a-trust] määrittääksesi, onko luottamussuhde on määritetty oikein *contoso.com* ja *treyresearch.com* toimialueiden välillä. Voit joutua odottamaan muutama minuutti, kun olet suorittanut edellisessä vaiheessa, ennen kuin voit vahvistaa luota.

Jäljellä oleva valinnaisia ohjeita noudattamalla tietoja sen selvittämisestä, onko toimialueen luottamus toimii odotetulla tavalla. Nämä vaiheet pätevät on pääsy kehittäminen tietokoneeseen asennettu Visual Studiossa.

1.  PowerShellin Azure-ikkunasta varten suorittamalla seuraavan komennon luominen web-taso, jos olet ole määrittänyt sitä aiemmin (avulla `Prepare` vaihtoehto):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Tämä komento luo web-taso ja lisää VMs *treyresearch.com* toimialueeseen.

2. ASP.NET Web-sovelluksen nimi *TreyResearchWebApp*käyttäminen Visual Studio kehittäminen-tietokoneella, luo. Käyttämällä .NET Framework 4.5.2.

3. Valitse *MVC* mallia ja muuta todennus *Windows-todennusta*. Älä luo sovelluksen-palvelun pilveen.

3. Muodosta ja testaa todennus sovelluksen käyttämiseen. Varmista, että nykyinen Windows-käyttäjänimi näyttää oikealta sivun yläreunassa valikkorivillä.

4. Sulje Internet Explorer.

5. *Ratkaisunhallinta* -ikkunassa TreyResearchWebApp projektin hiiren kakkospainikkeella, valitse *Julkaise*.

6. *Julkaise* -ikkunassa Valitse *Mukautettu*. Luo mukautettu profiilia *TreyResearchWebApp*.

7. *Yhteyden* -sivulla määrittää *Julkaise menetelmä* *Tiedostojärjestelmässä* ja määritä *TreyResearchWebApp*, helposti sijainnissa kehittäminen tietokoneeseen-nimiseen kansioon.

8. Määritä *asetukset* -sivun *määritysten* *versioon*.

9. *Esikatselun* sivulla Valitse *Julkaise*.

10. Muodosta yhteys kunkin AM-web-tason puolestaan (joko Siirry-ruutu) ja suorita seuraavat toimet. Web-sivuston IP-osoitteiden taso VMs ovat 10.0.1.4 ja 10.0.1.5. Molemmat VMs käyttäjänimi on *treyresearch\testuser* salasanalla *AweS0me@PW*:

    1. Kopioi *TreyResearchWebApp* -kansio ja sen sisältö kehittäminen tietokoneesta *C:\inetpub* -kansioon.

    2. Käytä Internet Information Services (IIS) hallinta-konsolin, siirry *Sites\Default sivuston* tietokoneeseen.

    3. Valitse *Toiminnot* -ruudusta *Perusasetukset*ja muuttaa sivuston fyysinen polku *%SystemDrive%\inetpub\TreyResearchWebApp*.

    4. Kaksoisnapsauta *Ominaisuuksia tietokantanäkymä* -ruudussa *todennusta*. Varmista, että *Windows-todennus* on käytössä ja *Anonyymi todentaminen* on poistettu käytöstä.

11. paikallisen-tietokoneesta Avaa Internet Explorer ja siirry web-sivustossa osoitteessa http://10.0.1.254 (tämä on web taso kuormituksen osoitetta).

12. Kirjoita *Windows suojaus* -valintaikkunan paikallisen toimialueen käyttäjän tunnistetiedot. Määritä käyttäjänimi, jota ei ole *treyresearch* toimialue. Jos käytössäsi on Simuloitu paikalliseen ympäristöön luo ensin käyttäjä *contoso* -toimialueen ja määrittää tämän käyttäjän tunnistetiedot.

13. Kun kotisivu tulee näkyviin, varmista, että oikea toimialue ja käyttäjänimen näkyvät valikkoriviltä oikealle sivun yläreunassa.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue [Lisää paikallisen toimialueen Azure laajentaminen] parhaat käytännöt[adds-extend-domain]

- Tutustu parhaisiin käytäntöihin [ADFS-infrastruktuuria] luomiseen[ adfs] Azure-tietokannassa.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Suojattu hybrid verkoston arkkitehtuuri ja AD toimialueille"