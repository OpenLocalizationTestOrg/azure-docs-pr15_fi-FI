<properties 
   pageTitle="DNS-nimen tarkkuusasetukset varten Linux VMs Azure-tietokannassa"
   description="Nimi tarkkuus tilanteita, joissa Linux VMs-Azure IaaS, mukaan lukien annettu DNS-palveluja, Hybrid ulkoiseen DNS- ja tuo Your oman DNS-palvelimeen."
   services="virtual-machines"
   documentationCenter="na"
   authors="RicksterCDN"
   manager="timlt"
   editor="tysonn" />
<tags 
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="rclaus" />

# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>Azure-tietokannassa VMs Linux DNS nimi-tarkkuusasetukset

Azure on DNS-nimenselvitys oletusarvoisesti kaikki VMs yhden Virtual verkoston sisällä. Olet voi ottaa käyttöön oman DNS nimi tarkkuus-ratkaisun määrittämällä oman Azure isännöityä VMs omia DNS-palvelut. Seuraavissa tilanteissa avulla voit valita, mitkä jokin toimii paremmin tarpeitasi.

- [Jos Azure nimenselvitys](#azure-provided-name-resolution)

- [Nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server) 

Voit käyttää nimenselvitys tyyppi vaihtelee sen mukaan, miten VMs ja roolin esiintymät on keskenään.

**Seuraavassa taulukossa on kuvattu skenaariot ja vastaavat nimi tarkkuus-ratkaisut:**

| **Skenaario** | **Ratkaisu** | **Jälkiliite** |
|--------------|--------------|----------|
| Nimenselvitys roolin esiintymiä tai samassa virtual verkossa sijaitsevat VMs välillä | [Jos Azure nimenselvitys](#azure-provided-name-resolution)| isäntänimi tai täydellinen toimialuenimi |
| Nimenselvitys roolin esiintymiä tai VMs sijaitsevat eri virtual verkkojen välillä | Asiakkaan hallita DNS-palvelimet välittäminen kyselyjen ratkaisu Azure (DNS-välityspalvelimen) mukaan vnets välillä.  Katso [nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server)| Vain FQDN |
| Paikallisen tietokoneen ja palvelun nimet roolin esiintymiä tai VMs Azure tarkkuus | Asiakkaan hallita DNS-palvelimet (esimerkiksi paikalliset toimialueen ohjauskoneen, paikallisen vain luku-toimialueen ohjauskoneen tai synkronoitu käyttämällä vyöhykkeen siirrot toissijaisen DNS).  Katso [nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server)|Vain FQDN |
| Paikallinen tietokoneista Azure isäntänimet tarkkuus | Asiakkaan hallitun DNS välityspalvelinta-vastaavan vnet eteenpäin kyselyjä, välityspalvelimen välittää kyselyt Azure tarkkuutta. Katso [nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server)| Vain FQDN |
| Käänteinen DNS: n sisäisen IP-osoitteet | [Nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server) | puuttuu |

## <a name="azure-provided-name-resolution"></a>Jos Azure nimenselvitys

Tarkkuus on julkinen DNS-nimet, sekä Azure on sisäinen nimenselvitys VMs ja roolin esiintymät, jotka sijaitsevat samassa virtual verkossa.  ARM-pohjainen virtual verkkojen DNS-liite on yhtenäinen virtual verkon kautta (jolloin FQDN ei tarvita) ja DNS-nimet voi määrittää NIC- ja VMs. Vaikka antamalla Azure nimenselvitys ei edellytä mitään määritykset, ei ole haluamasi vaihtoehto kaikki käyttöönoton tilanteessa, tarkastelu edellisen taulukon.

### <a name="features-and-considerations"></a>Ominaisuudet ja huomioon otettavia seikkoja

**Seuraavat ominaisuudet:**

- Käytön helppous: käyttämään antamalla Azure nimenselvitys ei tarvita määritysten.

- Azure toimitettuja nimi tarkkuus-palvelu on erittäin käytettävissä, jolloin sinun ei tarvitse luoda ja hallita omia DNS-palvelimien klustereiden.

- Voidaan sekä omia DNS-palvelimet sekä paikallisia että Azure isäntänimet ratkaisemiseksi.

- Nimenselvitys annetaan VMs virtual verkoissa ilman tarvetta FQDN välillä. 

- Isäntänimet, jotka kuvaavat käyttöönoton, voit käyttää sen sijaan, että käyttäminen automaattisesti luodut nimet.

**Huomioon otettavia seikkoja:**

- Azure luomat DNS-liite ei voi muokata.

- Et voi rekisteröidä oman tietueet manuaalisesti.

- WINS ja NetBIOS ei tueta.

- Isäntänimet on oltava DNS-yhteensopiva (ne on käytettävä vain 0-9-a-ö ja '-', ja et voi alussa tai lopussa "-". Kohdassa RFC 3696 2.)

- DNS-kyselyn liikenne on rajoitettu kunkin AM varten. Tämä ei kannata vaikuttaa useimpien sovellusten.  Jos pyyntö rajoittimen havaitaan, varmista, että asiakaspuolen välimuisti on käytössä.  Lisätietoja on artikkelissa [mahdollisimman paljon antamalla Azure nimenselvitys](#Getting-the-most-from-Azure-provided-name-resolution).


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Eniten antamalla Azure nimenselvitys hakeminen
**Asiakaspuolen välimuisti:**

Kaikki DNS-kyselyn lähetetään verkon kautta.  Asiakaspuolen välimuisti auttaa vähentämään viive ja parantaa toimintakykyyn verkon blips ratkaiseminen toistuvan DNS-kyselyt paikallisesta välimuistista.  DNS-tietueet sisältävät aika-To-Live (TTL) joka sallii välimuistin ilman vaikuttavat tietueen tuoreus niin kauan tietueen tallentamiseen.  Tästä syystä asiakaspuolen välimuisti ei sovellu useimmissa tapauksissa.

Jotkin Linux distros Älä sisällytä välimuistiin oletusarvoisesti.  On suositeltavaa lisätä sellaisen kunkin Linux AM (sen jälkeen, kun tarkistaa, että ei ole paikallisen välimuistin jo).

On useita eri DNS välimuistiin pakettien, kuten dnsmasq, dnsmasq asennetaan yleisimmät distros seuraavasti:

- **Ubuntu (käyttää resolvconf)**:
    - Asenna dnsmasq-paketti ("sudo dnsmasq piharakennus get Asenna").
- **SUSE (käyttää netconf)**:
    - Asenna dnsmasq-paketti ("sudo zypper Asenna dnsmasq") 
    - Ota käyttöön dnsmasq-palvelun ("systemctl dnsmasq.service Ota") 
    - Käynnistä dnsmasq-palvelu ("systemctl start dnsmasq.service") 
    - Muokkaa "/ jne/sysconfig/verkon/config" ja muuta NETCONFIG_DNS_FORWARDER = "", "dnsmasq"
    - päivittää resolv.conf ("netconfig päivitys"), voit määrittää välimuistin paikalliset DNS-tulkintatoiminnon
- **OpenLogic (käyttää NetworkManager)**:
    - Asenna dnsmasq-paketti ("sudo yum Asenna dnsmasq")
    - Ota käyttöön dnsmasq-palvelun ("systemctl dnsmasq.service Ota")
    - Käynnistä dnsmasq-palvelu ("systemctl dnsmasq.service Käynnistä")
    - Lisää "koskevan toimialue-nimipalvelimet-127.0.0.1;", "/etc/dhclient-eth0.conf"
    - Käynnistä verkkopalvelu ("palvelu uudelleen verkon") määrittää paikallisen DNS-tulkintatoiminnon välimuisti

> [AZURE.NOTE]: "Dnsmasq" pakkaus on vain yksi käytettävissä Linux monta DNS tallentaa.  Ennen kuin käytät sitä, tarkista sen sopivuus tietyn tarpeisiin ja muut välimuistin asennetun.

**Asiakkaan uudelleenyritykset:**

DNS on ensisijaisesti UDP-protokolla.  Kun UDP-protokollan ei takaa viestin lähettämisen, yritä logiikan käsitellään itse DNS-protokolla.  Kukin DNS-asiakasohjelma (käyttöjärjestelmää) voivat toimia eri uudelleen logiikan laatijat tilaksi mukaan:

 - Windows-käyttöjärjestelmien uudelleen jälkeen jokin toinen ja sitten uudelleen, kun toinen kahden, neljä ja toiseen neljän sekunnin. 
 - Viiden sekunnin kuluttua oletusarvon Linux asetukset-uudelleenyritykset.  Sinun on muutettava tämä yrittää viisi kertaa yhden sekunnin välein.  

Voit tarkistaa nykyisen asetuksia Linux AM "kate /etc/resolv.conf" ja "asetukset" olevalle riville katsaus esimerkiksi seuraavasti:

    options timeout:1 attempts:5

Resolv.conf-tiedosto luodaan automaattisesti ja ei voi muokata.  "Asetukset"-rivin lisääminen vaiheet vaihtelevat distro mukaan:

- **Ubuntu** (käyttää resolvconf):
    - Lisää rivin asetukset "/ etc/resolveconf/resolv.conf.d/head" 
    - Suorita resolvconf -u päivittäminen
- **SUSE** (käyttää netconf):
    - Lisää NETCONFIG_DNS_RESOLVER_OPTIONS "timeout:1 yritykset: 5" = "" parametrin tekstimuodossa "/ jne/sysconfig/verkon/config" 
    - Suorita netconfig-päivitys päivittää
- **OpenLogic** (käyttää NetworkManager):
    - Lisää "Kaiku"asetukset timeout:1 yritykset: 5"" "/ etc/NetworkManager/dispatcher.d/11-dhclient" 
    - Suorita 'palvelun verkon uudelleenkäynnistyksen' päivittäminen

## <a name="name-resolution-using-your-own-dns-server"></a>Nimenselvitys omia DNS-palvelimen käyttäminen
Useita tilanteita, jossa nimi tarkkuus tarpeitasi voi muutakin kuin ominaisuuksia edellyttäen mukaan Azure, esimerkiksi kun asetat DNS-ratkaisun virtual verkkojen (vnets) välillä.  Tässä skenaariossa kattamaan Azure tarjoaa mahdollisuuden voi käyttää omia DNS-palvelimet.  

DNS-palvelimet virtual verkossa oleville voi välittää DNS-kyselyt Azure on rekursiivinen resolvers ratkaisemiseksi isäntänimet virtual verkon sisällä.  Esimerkiksi DNS-palvelimeen Azure-tietokannassa voit vastata omassa DNS-vyöhyke tiedostojen DNS-kyselyt ja kaikki muut kyselyt toimittaa Azure.  Näin näet molemmat tapahtumat vyöhykkeen tiedostojen ja antamalla Azure isäntänimet (joko forwarder) VMs.  Azure on rekursiivinen resolvers käyttöoikeus on annettu virtual IP 168.63.129.16 kautta.

DNS-edelleenlähetys myös muun vnet DNS-ratkaisun avulla ja avulla paikallinen-koneet antamalla Azure isäntänimet ratkaisemiseksi.  Ratkaisemiseksi AM hostname DNS-palvelin AM on sijaittava virtual samassa verkostossa ja määritettävä Azure eteenpäin hostname kyselyjä.  DNS-liite on erilainen jokaiselle vnet, ehdollinen sääntöjen avulla voit lähettää DNS-kyselyt oikea vnet tarkkuutta.  Seuraavassa kuvassa on kaksi vnets ja suorittamalla muun vnet DNS-ratkaisun tällä menetelmällä paikallinen-verkossa:

![Muun vnet DNS](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Käytettäessä antamalla Azure nimenselvitys sisäiseen DNS-liite annetaan kunkin AM DHCP: N avulla.  Oman nimen tarkkuus-ratkaisun käytettäessä tämä jälkiliite ei ole toimitettu VMs koska se häiritsee muihin DNS-arkkitehtuureihin.  Voit viitata koneet FQDN mukaan tai liitteen määrittämiseksi oman VMs liitteen voidaan määrittää PowerShell tai Ohjelmointirajapinnan avulla:

-  Azure Resurssienhallinta hallitun vnets, Suffiksi [verkkokortti](https://msdn.microsoft.com/library/azure/mt163668.aspx) resurssin kautta tai voit suorittaa komennon `azure network public-ip show <resource group> <pip name>` näytettävä tietoja julkiseen IP, mukaan lukien täydellinen toimialuenimi NIC-sivustosta.    


Välittää kyselyt Azure ei vastaa tarpeitasi, jos haluat omia DNS-ratkaisu.  DNS-ratkaisun on:

-  Anna tarvittavat hostname tarkkuus kuten [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md)kautta.  Huomautus, jos haluat poistaa käytöstä DNS record vanhentuneiden Azure's DHCP-varauksia ovat todella pitkä ja puhdistuksen DDNS poistaa ennenaikaisesti DNS-tietueet. 
-  Anna tarvittavat rekursiivinen tarkkuutta haluat sallia ulkoisten toimialuenimien tarkkuus.
-  On käytettävissä (TCP- ja UDP-porttiin 53) se toimii asiakkaiden ja voivat käyttää internet.
-  Käyttöä vastaan voidaan suojata Internetistä pienentämään uhkien aiheuttaman ulkoisen agenttien vuoksi.

> [AZURE.NOTE] Parhaan suorituskyvyn, kun Käytä Azure VMs DNS-palvelimet, IPv6 käytöstä ja [Esiintymän tason julkiseen IP](../virtual-network/virtual-networks-instance-level-public-ip.md) määritetään DNS-palvelimien AM.  

