<properties
   pageTitle="Käyttöönoton Hybrid-verkoston arkkitehtuuri, ja Azuren ja paikallisten VPN | Microsoft Azure"
   description="Miten toteuttamisesta suojattu sivusto verkoston arkkitehtuuri, joka ulottuu Azure virtual verkon ja paikallisen verkon yhteydessä VPN-yhteyttä käyttämällä."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN toteuttaminen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa käsitellään joukko käytännöt laajentaminen paikallisen-verkon Azure käyttämällä sivuston sivuston näennäisen yksityisverkon (VPN) päälle. Liikenne jatkuu paikalliseen verkkoon ja Azure Virtual verkon välillä (VNet) IP-VPN-tunnelin kautta. Tämä arkkitehtuuri sopii hybrid sovellusten seuraavat ominaisuudet:

- Sovelluksen osista suoritetaan paikallisen, kun taas muut suoritetaan Azure-tietokannassa.

- Liikenne välillä paikallisen laitteen ja pilveen on todennäköisesti vaalea, tai se on hyödyllisiin kaupan hieman laajennettu viive joustavuutta ja käsittelyn potenssiin pilveen.

- Laajennettu verkko on suljettu järjestelmän. Ei ole suora polku Internetistä Azure-VNet.

- Käyttäjien Muodosta yhteys paikalliseen verkkoon käyttää Azure isännöidä palveluja. Siltaa välille paikallisen verkko- ja Azure-palvelut on läpinäkyvä käyttäjille.

Esimerkkejä, jotka sopivat profiilia skenaariot:

- Liiketoiminta-sovellusten avulla voidaan organisaatiossa, jossa osa toiminnot on siirretty pilveen.

- Kehittämisen ja testaa/kurssin toiminnoista.

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Azure Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Tämä Sinikopio käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tämän arkkitehtuuri osat:

> Visio-tiedosto, joka sisältää arkkitehtuuri Tässä kaaviossa on ladattavissa [Microsoft download Centeristä][visio-download]. Tässä kaaviossa on verkossa"Hybrid - VPN-sivu.

![[0]][0]

- **Paikalliseen verkkoon.** Tämä on verkon tietokoneiden ja laitteiden, organisaation käytössä paikalliseen yksityisverkon kautta.

- **VPN-laitteen.** Tämä on laitteen tai palvelu, joka sisältää ulkoisen yhteys paikalliseen verkkoon. VPN-laitteen ehkä laite, tai se voi olla esimerkiksi reititys- ja Remote Access Service (RRAS) Windows Server 2012-ohjelmiston ratkaista.

> [AZURE.NOTE] Luettelo tuetuista VPN-laitteiden ja yhdyskäytävän Azure VPN-yhteyden muodostamisesta valitun VPN-laitteiden määrittämisestä on artikkelissa [Azure tukemat VPN-laiteluettelosta]sopiva laite ohjeet[vpn-appliance].

- **N-taso cloud-sovellus.** Tämä on ylläpidettävä Azure-sovellus. Se voi olla useita tasoa kanssa useita aliverkosta Azure kuormituksen tasoitusmääritykset kautta. Kunkin aliverkon tietoliikenne sinulta voidaan veloittaa säännöt, jotka on määritetty käyttämällä [Azure verkon käyttöoikeusryhmät (NSGs)][azure-network-security-group]. Lisätietoja on ohjeaiheessa [käytön aloittaminen Microsoft Azure suojauksen][getting-started-with-azure-security].

> [AZURE.NOTE] Tässä artikkelissa kuvataan kuin yhden kohteen cloud-sovellus. [N-taso-arkkitehtuuri Azure-] käynnissä[ implementing-a-multi-tier-architecture-on-Azure] yksityiskohtaiset tiedot.

- **[Virtuaalinen verkon (VNet)][azure-virtual-network].** Cloud-sovellus ja Azure VPN-yhdyskäytävän osia asuvat samassa VNet.

- **[Azure VPN-yhdyskäytävän][azure-vpn-gateway].** VPN gateway-palveluun mahdollistaa paikalliseen verkkoon VNet muodostaa VPN-laitteen kautta. Lisätietoja on artikkelissa [etäyhteyden paikallisen-verkon Microsoft Azure virtual verkkoon][connect-to-an-Azure-vnet]. VPN-yhdyskäytävän sisältää seuraavat osat:

  - **VPN-yhdyskäytävä.** Tämä on resurssi, joka sisältää virtual VPN-laitteeseen VNet. On vastuussa reitittää liikenteen paikallisen verkosta VNet.

  - **Paikalliseen verkkoon kirjauduttaessa yhdyskäytävä.** Tämä on paikallisen VPN-laitteen. Tämä yhdyskäytävän reititetään verkkoliikennettä paikalliseen verkkoon cloud-sovelluksesta.

  - **Yhteys.** Yhteys on ominaisuudet, jotka määrittävät yhteystyyppi (IP) ja paikallisen VPN-laitteen jaettu avaimen salaamiseen liikenne.

  - **Yhdyskäytävän aliverkon.** VPN-yhdyskäytävän säilytetään omassa aliverkon, joka on erilaisia vaatimuksia suositukset-kohdassa alla kuvatulla tavalla.

- **Sisäinen kuormituksen.** Cloud-sovelluksen kautta sisäinen kuormituksen reititetään VPN-yhdyskäytävän verkkoliikennettä. Kuormituksen sijaitsee sovelluksen edusta aliverkon.

## <a name="recommendations"></a>Suosituksia

Azure tarjoaa useita eri resursseja ja jotta hakemisto-arkkitehtuuri voidaan resurssityypit valmisteltu monella tavalla. On annettu Azure Resurssienhallinta-mallin asentaminen viittaus-arkkitehtuuri, joka noudattaa näitä suosituksia. Jos haluat luoda oman viittaus-arkkitehtuuri seuraavien suositusten ellei sinulla ole tietyn vaatimus suositus sovi.

### <a name="vnet-and-gateway-subnet"></a>VNet ja yhdyskäytävän aliverkon

Luo Azure-VNet asettamisen osat pilveen. Azure-VNet osoitetilan on oltava mahtuu VMs ja VNet aliverkosta osoitteet. Varmista, VNet osoitetilaa on tarpeeksi tilaa kasvu, jos muita VMs todennäköisesti voi tarvittaessa. Osoitetilaa, VNet eivät saa olla päällekkäisiä paikalliseen verkkoon. Yllä olevassa kaaviossa käyttää esimerkiksi osoite tilaa 10.20.0.0/16 VNet.

Luo aliverkon nimeltä _GatewaySubnet_, jonka /27 osoite-alue. Tämän aliverkon edellyttää VPN-yhdyskäytävän ja kohdistaminen tämän aliverkon 32 osoitteet auttaa estämään käytössä työpöytäversioihin mahdollista yhdyskäytävän kokorajoitukset myöhemmin. Vältä tämä aliverkon sijoittamista keskellä osoitetilaa varten. Hyvä on osoitetilaa yhdyskäytävän aliverkon VNet osoitetilaa ylemmässä lopussa. Kaavion esimerkissä 10.20.255.224/27.  Nopea toiminto CIDR laskemiseen on seuraavanlainen:

1. Määritä muuttujan bittien 1, että yhdyskäytävän aliverkon käyttämät ja valitse muut bittiä arvoksi 0 bittien ylöspäin VNet osoitetilaa.

2. Tuloksena bittien muuntaminen desimaaliluvuksi ja ilmaiseminen-osoitteen tila etuliitteen, pituus määritetty kokoa yhdyskäytävän aliverkon.

Esimerkiksi VNet, ja IP-osoitealueita, 10.20.0.0/16, käyttämällä vaiheen #1 tulee 10.20.0b11111111.0b11100000.  Muuntaminen, desimaaliluvuksi ja ilmoittaminen se kuin osoitetilaa varten tuottaa 10.20.255.224/27

> [AZURE.WARNING] Älä ota muiden näennäiskoneiden tai rooli esiintymät, yhdyskäytävän aliverkon. Myös Määritä NSG tämän aliverkon, kun se aiheuttaa yhdyskäytävän lakkaa toimimasta.

### <a name="virtual-network-gateway"></a>VPN-yhdyskäytävän

Varaa VPN-yhdyskäytävän julkiseen IP-osoite.

VPN-yhdyskäytävän luominen yhdyskäytävän aliverkon ja määritä se uudet julkiseen IP-osoite. Käytä yhdyskäytävän tyyppi, joka vastaa parhaiten tarpeitasi ja jotka on otettu käyttöön VPN-laitteen mukaan:

[Käytännön perustuva yhdyskäytävän] luominen[ policy-based-routing] Jos haluat määrittää tarkasti, kuinka pyynnöt reititetään. käytännön ehtoja, kuten osoitteiden etuliitteiden perusteella. Käytännön perustuva yhdyskäytävien käyttää staattinen reititys ja toimivat vain sivuston sivuston yhteydet.

[Reititys-pohjainen yhdyskäytävän] luominen[ route-based-routing] Jos muodostat yhteyden paikalliseen verkkoon RRAS, tukevat usean sivuston tai rajat-alueen yhteyksiä tai toteuta VNet VNet yhteydet (kuten tiet, käy läpi useita VNets). Reititys-pohjainen kohdeyhdyskäytävä käyttää dynaaminen reititys suora liikenne verkkojen välillä. Ne hyväksyt parempi kuin staattinen tiet verkkopolun virheet, koska ne yrittävät vaihtoehtoinen tiet. Reitti-pohjainen yhdyskäytävien myös pienentää hallinta kuormitus, koska tiet joutua ei on päivitettävä manuaalisesti, kun verkko-osoitteiden muuttaminen.

Luettelo tuetuista VPN-laitteita, tarkastella [tietoja VPN-laitteita, sivuston sivuston VPN-yhdyskäytävän yhteyksien][vpn-appliances].

> [AZURE.NOTE] Kun yhdyskäytävä on luotu, et voi muuttaa yhdyskäytävän eri ilman poistaminen ja luominen uudelleen yhdyskäytävän välillä.

Valitse Azure VPN yhdyskäytävän tuote, joka vastaa parhaiten siirtonopeuden tarpeen. Azure VPN-yhdyskäytävä on käytettävissä seuraavassa taulukossa näkyy kolme tuotteissa. Jokainen tuote on eri nopeus, [kustannukset peritään] [ azure-gateway-charges] mukaan kuinka kauan yhdyskäytävä on valmisteltu ja käytettävissä.

| TUOTE | VPN siirtonopeuden | Maks IP tunneloi|
|-----|----------------|------------------|
| Perustoiminnot | 100 Mbps | 10 |
| Vakio | 100 Mbps | 10 |
| Erinomainen suorituskyky | 200 Mbps | 30 |

> [AZURE.NOTE] Tavallinen tuote ei ole yhteensopiva Azure ExpressRoute. Voit [muuttaa olevat versiot] [ changing-SKUs] yhdyskäytävän luomisen jälkeen.

Reitityssääntöjen luominen yhdyskäytävän aliverkon sovelluksen suoraan saapuva liikenne Yhdyskäytävästä sisäinen kuormituksen salliminen pyynnöt siirtää VMs, jotka toteuttavat sovelluksen sijaan.

### <a name="on-premises-network-connection"></a>Paikallisen verkkoyhteys

Luo paikalliseen verkkoon kirjauduttaessa yhdyskäytävä. Määrittää paikallisen VPN-laite ja paikallisen verkoston osoitetilaa julkiseen IP-osoite. Huomaa, että paikallisen VPN-laitteen edellyttää julkiseen IP-osoite, jota voidaan käyttää Azure VPN-yhdyskäytävän paikalliseen verkkoon kirjauduttaessa yhdyskäytävän. VPN-laite ei löydy, takana NAT-laite.

Luo sivusto yhteys VPN-yhdyskäytävän ja paikallisen yhdyskäytävien. Valitse sivusto (IP) yhteyden tyyppi ja määritä jaetun avaimen. Sivusto salaus Azure VPN Gatewayn perustuu IP-protokollan jaettua näppäimellä todennusta varten. Voit määrittää avaimen, kun luot Azure VPN-yhdyskäytävän. Sinun on määritettävä käynnissä paikallisen samalla avaimella VPN-laitteen. Muiden käyttöoikeuksien menetelmien eivät ole tuettuja.

Varmista, että paikallisen reititys infrastruktuuri on määritetty välittämään pyyntöjä tarkoitettu osoitteet Azure VNet VPN-laitteeseen.

Avaa paikalliseen verkkoon cloud-sovelluksen vaatimat portit.

Testaa yhteys varmista seuraavat seikat:

- Paikallisen VPN-laitteen reitittää liikenteen oikein cloud-sovelluksen Azure VPN-yhdyskäytävän kautta.

- VNet reitittää liikenteen oikein takaisin paikalliseen verkkoon.

- Kielletyt liikenne molempiin suuntiin estetään oikein.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Voit tehostaa rajoitettu pystysuora skaalattavuus siirtämällä Basic tai vakio VPN-yhdyskäytävän tuotteissa hyvin suorituskyvyn VPN-tuote.

Harkitse VNets, joka olettaa päivittäin erittäin paljon VPN-liikenne, jakaminen tuominen eri pienempi VNets eri toiminnoista ja VPN-yhdyskäytävän määrittäminen kullekin niistä.

Voit osio VNet vaaka- tai pystysuunnassa. Osion vaakasuunnassa, voit siirtää AM joissakin tapauksissa kunkin tason uusi VNet aliverkosta. Tulos on, että kunkin VNet on sama rakenne ja toiminnot. Voit osion pystysuunnassa muuttamiseen kunkin tason toimintoja jakautuvan eri looginen alueet (kuten käsittely tilaukset, Laskutus, asiakkaan tilinhallinta ja niin edelleen). Toimi kullekin alueelle sijoitetaan sitten oma VNet.

Replikoiminen paikallisen Active Directory toimialueen ohjauskoneen VNet- ja toteuttaminen DNS VNet auttaa vähentämään tuottamat pilveen paikallisen järjestelmänvalvojan ja tietoturvaan liittyvät liikenne.

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Jos haluat, että paikalliseen verkkoon pysyy käytettävissä Azure VPN-yhdyskäytävän, ota käyttöön paikallisen VPN-yhdyskäytävän automaattisesti klusterin.

Jos organisaatiolla on useita paikallisen sivustoja, luoda [usean sivuston yhteydet] [ vpn-gateway-multi-site] vähintään yksi Azure VNets avulla. Tämän menetelmän edellyttää dynaaminen reititys (reitti-pohjainen), joten varmista, että paikallisen VPN-yhdyskäytävän tukee tätä ominaisuutta.

Katso [VPN-yhdyskäytävän SLA] [ sla-for-vpn-gateway] VPN-yhdyskäytävän SLA lisätietoja.

## <a name="manageability-considerations"></a>Hallittavuuden huomioon otettavia seikkoja

Seurata diagnostiikan paikallisen VPN-laitteiden tietoja. Tämä toimenpide määräytyy VPN-laitteen ominaisuuksia. Esimerkiksi jos reititys ja etäkäyttö-palvelu on käytössä Windows Server 2012, kannattaa ottaa käyttöön [RRAS kirjaaminen][rras-logging].

Käytä [Azure VPN-yhdyskäytävän diagnostiikka] [ gateway-diagnostic-logs] yhteysongelmat tietoja. Nämä lokeja voidaan seurata tietoja, kuten lähde- ja kohteisiin yhteyden pyynnöt, käyttämän protokollan, ja miten yhteys on muodostettu (tai miksi epäonnistui).

Valvoa Azure VPN-yhdyskäytävän toiminnallisia lokit käytettävissä Azure-portaalissa valvontalokien. Erillinen lokit ovat käytettävissä paikalliseen verkkoon kirjauduttaessa yhdyskäytävän Azure yhdyskäytävien ja yhteys. Nämä tiedot voidaan seurata yhdyskäytävän tehtyjä muutoksia ja voi olla hyödyllinen, jos aiemmin toimii yhdyskäytävän lakkaa toimimasta jostain syystä.

![[2]][2]

Seurata yhteyden ja seurata yhteys epäonnistui tapahtumia. Voit käyttää seurantaan package, kuten [Nagios] [ nagios] ja nämä tiedot.

## <a name="security-considerations"></a>Suojausasiat

Luo kullekin VPN-yhdyskäytävän eri jaettu avain. Vahvan jaetun avaimen avulla vastustavat kannalta voimassa kalastelu.

> [AZURE.NOTE] Tällä hetkellä et voi käyttää ennalta jaetut avaimet Azure VPN-yhdyskäytävän Azure avaimen säilö.

Varmista, että paikallisen VPN-laitteen käyttää salausmenetelmä, joka on [yhteensopiva Azure VPN-yhdyskäytävän][vpn-appliance-ipsec]. Käytännön perustuva reititys, Azure VPN-yhdyskäytävän tukee AES256 AES128 ja 3DES salausalgoritmeista. Reititys-pohjainen yhdyskäytävien tukevat AES256 ja 3DES.

Jos paikallisen VPN-laitteen on ympäröivän verkossa, jossa on palomuuri ympäröivän verkon ja Internetin välillä, voit joutua määrittäminen [Lisää palomuurisäännöt] [ additional-firewall-rules] sallimaan sivuston sivuston VPN-yhteyttä.

Jos VNet sovelluksen lähettää tietoja Internetin, harkitse [Pakotettu tunneling] [ forced-tunneling] reitittämään kaikki Internet sidottujen liikenne paikalliseen verkkoon. Tämän menetelmän avulla voit valvoa lähtevän postin paikallisen infrastruktuuri sovelluksen tekemät pyynnöt.

> [AZURE.NOTE] Pakotetun tunneling saattaa olla vaikutusta connectivity Azure-palveluihin (tallennustilan palvelu, esimerkiksi) ja Windowsin käyttöoikeuksien hallinta.

## <a name="troubleshooting-considerations"></a>Huomioon otettavia seikkoja vianmääritys

> [AZURE.NOTE] Vieraile reititys- ja Remote Access-blogi yleistiedot [tavallisten virheiden vianmääritystapojen kuvauksia VPN-ryhmän][troubleshooting-vpn-errors].

Jos liikenne ei voi siirtää VPN-yhteyden, tarkista seuraavat asiat:

### <a name="on-premises-vpn-appliance"></a>Paikallisen VPN-laitteen:

**Paikallisen VPN-laite toimii oikein?**

Tarkista jokin virheet ja virheet VPN-laitteen luomat lokitiedostot. Sijainnin tiedot vaihtelevat oman laitteen mukaan. Esimerkiksi jos käytät RRAS Windows Server 2012, voit tehdä seuraavaa PowerShell-komentoa RRAS-palvelun virhe tapahtumatietojen näyttäminen:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Tekstien _sanoman_ -ominaisuus on virheen kuvaus. Joitakin yleisiä esimerkkejä ovat seuraavat:

- Ei voi muodostaa mahdollisesti virheellisen IP-osoitteen määritetty RRAS VPN-verkon käyttöliittymän määritysten Azure VPN-yhdyskäytävän vuoksi:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- Väärä jaetun avaimen parhaillaan määritetty RRAS VPN-verkon liittymän määritykset:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Voit myös laskea tapahtumaloki tietoja yrittää muodostaa yhteyden RRAS-palvelun kautta käyttämällä PowerShell-komentoa:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

Jos yhteyden, lokia sisältää virheitä, joka näyttää seuraavankaltaiselta:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**Onko VPN laitteen oikein reitittää liikenteen Azure VPN-yhdyskäytävän kautta?**

Käytä työkalua, kuten [PsPing] [ psping] vahvistamiseksi yhteys- ja VPN-yhdyskäytävän reitittämisen. Testaa yhteys paikalliseen tietokoneesta verkkopalvelimeen VNet sijaitsee, suorita seuraava komento (korvaa `<<web-server-address>>` WWW-palvelimen osoite):

```
PsPing -t <<web-server-address>>:80
```

Jos paikallisen tietokoneen voit reitittää liikenteen verkkopalvelimeen, pitäisi näkyä tulosteen seuraavankaltaiselta:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Jos määritetty kohde ei saa yhteyttä paikalliseen tietokoneeseen, näet viestit tältä:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**Sallittuja paikallinen palomuuri VPN-liikenne paikalliseen läpi? On avattu oikea portit?**

Tarkista, että paikallisen VPN-laitteen käyttää salausmenetelmä, joka on [yhteensopiva Azure VPN-yhdyskäytävän][vpn-appliance].

Käytännön perustuva reititys, Azure VPN-yhdyskäytävän tukee AES256 AES128 ja 3DES salausalgoritmeista. Reititys-pohjainen yhdyskäytävien tukevat AES256 ja 3DES.

### <a name="azure-vnet-gateway"></a>Azure VNet yhdyskäytävä:

Tarkastele [Azure VPN-yhdyskäytävän vianmäärityslokit] [ gateway-diagnostic-logs] mahdollisten ongelmien varalta.

**Ovat Azure VPN-yhdyskäytävän ja paikallisen VPN laitteen määritetty saman jaetun todennus-näppäimen kanssa?**

Voit tarkastella jaetun avaimen tallentamat Azure VPN-yhdyskäytävän Azure CLI-komennon avulla:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

Näytä määritetty kyseisen laitteen jaetun avaimen vastaava paikallisen VPN-laitteen-komennon avulla.

Varmista, että pidät Azure VPN-yhdyskäytävä ei ole liitetty NSG _GatewaySubnet_ aliverkon.

Voit tarkastella aliverkon muutoksen Azure CLI-komennon avulla:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Varmista ei ole tiedot-kenttä nimeltä _verkon käyttöoikeusryhmän tunnus_. Seuraavassa esimerkissä tulokset esimerkiksi _GatewaySubnet_ , joka on määritetty NSG (_VPN-yhdyskäytävä-ryhmä_). Tämä voi aiheuttaa estää yhdyskäytävän toimimasta oikein, jos on määritetty tämä NSG sääntöjä:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Valitse Azure VNet näennäiskoneiden on määritetty sallimaan tulevat ulkopuolella VNet liikenne Valitse liittyvä aliverkosta, joka sisältää näitä näennäiskoneiden NSG luomillasi säännöillä.**

Voit tarkastella kaikkia NSG sääntöjä Azure CLI-komennon avulla:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**On katkaistu Azure VPN-yhdyskäytävän?**

Seuraavat PowerShellin Azure-komennon avulla voit tarkistaa nykyisen tilan Azure VPN-yhteyden. `<<connection-name>>` Parametri on nimi, joka toimii linkkinä VPN-yhdyskäytävän ja paikallisen yhdyskäytävän Azure VPN-yhteyden.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

Seuraavat katkelmat Korosta silloin, kun yhdyskäytävä on yhdistetty (ensimmäinen esimerkki) ja irti tulos (toinen esimerkki):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Isännän AM kokoonpano, verkon kaistanleveyden käytön ja sovelluksen suorituskyvyn:

Varmista, että Vieras-käyttöjärjestelmässä käytössä aliverkon Azure-VMs palomuuri on määritetty oikein sallimaan sallitut tietoliikenteen paikallisen IP-alueita.

**On liikenne äänenvoimakkuuden on lähes täynnä kaistanleveydestä Azure VPN-yhdyskäytävän käytettävissä?**

Sillä riippuu siitä, että VPN-laitteen käynnissä paikallisen käytettävissä tilat. Esimerkiksi jos käytät RRAS Windows Server 2012: ssa, voit tehdä suorituskyvyn valvonta seurata äänenvoimakkuuden tiedot on vastaanotettu ja välityksellä VPN-yhteyden; Käytä _RAS yhteensä_ objekti, valitse _Tavua vastaanotettu/s_ ja _Tavua lähettäminen/s_ laskureita:

![[3]][3]

Vertaile kanssa (100 Mbps Basic-ja Standard tuotteissa ja 200 Mbps for hyvin suorituskyvyn tuote) VPN-yhdyskäytävän kaistanleveyttä tulokset:

![[4]][4]

**On jokin-Azure VNet näennäiskoneiden hidas? Ovat ne ylikuormitettu, ovat riittävän niistä käsittelemään kuormituksen on kaikki kuormituksen-tasoitusmääritykset määritetty oikein?**

Tämä edellyttää, [tallentaa ja analysoiminen vianmääritystiedot][azure-vm-diagnostics]. Voit tarkastella tuloksia Azure-portaalissa, mutta monet kolmannen osapuolen työkaluja ovat myös käytettävissä, jotka tarjoavat suorituskyvyn tietoa yksityiskohtaiset tiedot.

**Puhelujen tehokkaan sovelluksen avulla cloud resurssien ominaisuudet**

Välineen sovelluksen tunnus kunkin AM määrittääksesi, onko sovellukset Tee resurssien käyttäminen käytössä. Voit käyttää työkaluja, kuten [Hakemuksen tiedot] [ application-insights] avulla näistä tehtävistä.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Jos sinulla on jo määritetty VPN-laitteen aiemmin paikalliseen infrastruktuuri, voit ottaa viite-arkkitehtuuri toimimalla seuraavasti:

1. Napsauta alla olevaa painiketta hiiren kakkospainikkeella ja valitse joko "Avaa linkki uudessa välilehdessä" tai "Avaa linkki uudessa ikkunassa":  
[![Ottaa käyttöön Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Linkki Azure-portaalissa avaamaan Odota ja valitse seuraavasti: 
    - **Resurssiryhmä** nimi on jo määritetty parametri-tiedostossa, joten valitsemalla **Luo uusi** ja kirjoita `ra-hybrid-vpn-rg` tekstiruutuun.
    - Valitse alue-ruutuun **sijainti** avattavasta luettelosta.
    - Älä muokkaa **Mallin pääkansion Uri** tai **Parametrin pääkansion Uri** -tekstiruutuja.
    - Tarkista ehdot ja valitse sitten **voin sopivat edellä mainittujen ehdot** -valintaruutu.
    - Valitse **Osta** -painiketta.

3. Odota suorittamiseen käyttöönottoa varten.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Asentaminen lisätoimialuetta ohjaimet VMs käyttäminen Azure-VNet paikallisen Active Directory-toimialueen osalta][installing-ad].

- [Ohjeet Windows Azure VMs palvelimen Active Directoryn käyttöönotosta][deploying-ad].

- [DNS-palvelimen luominen VNet][creating-dns].

- [Määrittämisestä ja hallinnasta VNet DNS][configuring-dns].

- [Paikallisen Stormshield verkon suojauksen laitteiden käyttämällä VPN-yhteyden][stormshield].

- [Käyttöönoton Hybrid-verkoston arkkitehtuuri, ja Azure ExpressRoute][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Rakenne hybrid verkon paikallisen jotka kestävät ja cloud infrastruktuurin"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Voit parantaa skaalattavuus VNet jakaminen"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Valvontalokit Azure-portaalissa"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Suorituskyvyn laskureita VPN verkkoliikennettä seurantaa varten"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Esimerkki VPN verkon suorituskyvyn graph"