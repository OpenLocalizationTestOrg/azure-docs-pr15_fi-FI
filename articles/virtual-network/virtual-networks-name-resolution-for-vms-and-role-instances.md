<properties 
   pageTitle="VMs tarkkuutta roolin esiintymiä"
   description="Nimeä tarkkuus skenaariot Azure IaaS hybrid ratkaisuja eri pilvipalveluihin, Active Directoryn ja DNS-palvelimen käyttäminen "
   services="virtual-network"
   documentationCenter="na"
   authors="GarethBradshawMSFT"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="telmos" />

# <a name="name-resolution-for-vms-and-role-instances"></a>Nimenselvitys VMs ja roolin esiintymiä

Sen mukaan, miten käytät Azure isännöimiseen IaaS, PaaS ja hybrid ratkaisuja voit joutua sallimaan VMs ja roolin esiintymät, jotka luot keskenään. Tämä viestintä on mahdollista käyttämällä IP-osoitteet, mutta se on helpompi käyttää nimiä, jotka helposti mitalilla ja eivät muutu. 

Rooli esiintymät ja Azure ylläpidettävä VMs tarvittaessa selvittämään toimialuenimiä sisäisiä IP-osoitteita, he voivat käyttää toista näistä tavoista:

- [Jos Azure nimenselvitys](#azure-provided-name-resolution)

- [Nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server) (joka voi toimittaa kyselyjä antamalla Azure DNS-palvelimet) 

Voit käyttää nimenselvitys tyyppi vaihtelee sen mukaan, miten VMs ja roolin esiintymät on keskenään.

**Seuraavassa taulukossa on kuvattu skenaariot ja vastaavat nimi tarkkuus-ratkaisut:**

| **Skenaario** | **Ratkaisu** | **Jälkiliite** |
|--------------|--------------|----------|
| Nimenselvitys roolin esiintymiä tai VMs poistettavassa saman pilvipalvelussa tai VPN välillä | [Jos Azure nimenselvitys](#azure-provided-name-resolution)| isäntänimi tai täydellinen toimialuenimi |
| Nimenselvitys roolin esiintymiä tai VMs sijaitsevat eri virtual verkkojen välillä | Asiakkaan hallita DNS-palvelimet välittäminen kyselyjen ratkaisu Azure (DNS-välityspalvelimen) mukaan vnets välillä.  Katso [nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server)| Vain FQDN |
| Paikallisen tietokoneen ja palvelun nimet roolin esiintymiä tai VMs Azure tarkkuus | Asiakkaan hallita DNS-palvelimet (esimerkiksi paikalliset toimialueen ohjauskoneen, vain luku-paikallisen toimialueen ohjauskoneen tai synkronoitu käyttämällä vyöhykkeen siirrot toissijaisen DNS).  Katso [nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server)|Vain FQDN |
| Paikallinen tietokoneista Azure isäntänimet tarkkuus | Asiakkaan hallitun DNS välityspalvelinta-vastaavan vnet eteenpäin kyselyjä, välityspalvelimen välittää kyselyt Azure tarkkuutta. Katso [nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server)| Vain FQDN |
| Käänteinen DNS: n sisäisen IP-osoitteet | [Nimenselvitys omia DNS-palvelimen käyttäminen](#name-resolution-using-your-own-dns-server) | puuttuu |
| Nimenselvitys VMs tai rooli esiintymiä eri pilvipalveluihin kuin virtual verkossa sijaitsevat välillä| Ei käytettävissä. VMs ja roolin esiintymiä eri cloud Services-palveluissa välinen yhteys ei tue virtual verkon ulkopuolelta.| puuttuu |



## <a name="azure-provided-name-resolution"></a>Jos Azure nimenselvitys

Tarkkuus on julkinen DNS-nimet, sekä Azure on sisäinen nimenselvitys VMs ja roolin esiintymät, jotka asuvat samassa virtual verkossa tai pilvipalvelussa.  VMs/esiintymät pilvipalvelussa jakaa saman DNS-liite (jotta yksin hostname riittää), mutta perinteinen virtual verkkojen eri pilvipohjaisia palveluja on eri DNS-liitteet, jotta FQDN tarvitsee selvittää nimeä eri pilvipalveluihin välillä.  Virtuaalinen verkot resurssien hallinnan käyttöönottomalli, DNS-liite on yhtenäinen virtual verkon kautta (jolloin FQDN ei tarvita) ja DNS-nimet voi määrittää NIC- ja VMs. Vaikka antamalla Azure nimenselvitys ei edellytä mitään määritykset, ei ole haluamasi vaihtoehto kaikki käyttöönoton tilanteessa, tarkastelu-edellä olevassa taulukossa.

> [AZURE.NOTE] Kyseessä Internetin kautta tai työntekijä roolit voit käyttää sisäisiä IP-osoitteita roolin esiintymien Azure Service Management REST-Ohjelmointirajapinnan käyttäminen roolin nimi ja esiintymän määrän perusteella. Lisätietoja on artikkelissa [Service Management REST API-viittaus](https://msdn.microsoft.com/library/azure/ee460799.aspx).

### <a name="features-and-considerations"></a>Ominaisuudet ja huomioon otettavia seikkoja

**Seuraavat ominaisuudet:**

- Käytön helppous: Jotta voit käyttää antamalla Azure nimenselvitys ei tarvita määritysten.

- Azure toimitettuja nimi tarkkuus-palvelu on erittäin käytettävissä, jolloin sinun ei tarvitse luoda ja hallita omia DNS-palvelimien klustereiden.

- Voidaan yhdessä DNS-palvelimet sekä paikallisia että Azure isäntänimet ratkaisemiseksi.

- Nimenselvitys annetaan rooli esiintymät/VMs sisällä saman pilvipalvelussa FQDN tarvetta ilman välillä.

- Nimenselvitys annetaan virtual Resurssienhallinta-käyttöönotto-mallin ilman tarvetta FQDN käyttävät verkot VMs välillä. Perinteinen käyttöönoton mallin Virtual verkot tarvita FQDN ratkaiseminen eri pilvipalveluihin nimet. 

- Isäntänimet, jotka kuvaavat käyttöönoton, voit käyttää sen sijaan, että käyttäminen automaattisesti luodut nimet.

**Huomioon otettavia seikkoja:**

- Azure luomat DNS-liite ei voi muokata.

- Et voi rekisteröidä oman tietueet manuaalisesti.

- WINS ja NetBIOS ei tueta. (Et näe omaa VMs Resurssienhallinnassa.)

- Isäntänimet on oltava DNS-yhteensopiva (ne on käytettävä vain 0-9-a-ö ja '-', ja et voi alussa tai lopussa "-". Kohdassa RFC 3696 2.)

- DNS-kyselyn liikenne on rajoitettu kunkin AM varten. Tämä ei kannata vaikuttaa useimpien sovellusten.  Jos pyyntö rajoittimen havaitaan, varmista, että asiakaspuolen välimuisti on käytössä.  Lisätietoja on artikkelissa [mahdollisimman paljon antamalla Azure nimenselvitys](#Getting-the-most-from-Azure-provided-name-resolution).

- Vain ensimmäinen 180 cloud Services-palveluissa VMs rekisteröidään kunkin virtual verkon perinteinen käyttöönotto-mallissa. Tämä ei koske virtual verkot Resurssienhallinta käyttöönoton mallit.


### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Eniten antamalla Azure nimenselvitys hakeminen
**Asiakaspuolen välimuisti:**

Kaikki DNS-kysely on lähetetään verkon kautta.  Asiakaspuolen välimuisti auttaa vähentämään viive ja parantaa toimintakykyyn verkon blips ratkaiseminen toistuvan DNS-kyselyt paikallisesta välimuistista.  DNS-tietueet sisältävät aika-To-Live (TTL) joka sallii välimuistin tallentamiseen niin kauan tietueen ilman vaikuttavat tietueen tuoreus, joten asiakaspuolen välimuisti sopii useimmissa tapauksissa.

Oletusarvo Windows DNS-asiakas on DNS-välimuistin valmiit.  Jotkin Linux distros Älä sisällytä oletusarvoisesti välimuistiin, on suositeltavaa, että jokin voi lisätä kunkin Linux AM (sen jälkeen, kun tarkistaa, että ei ole paikallisen välimuistin jo).

Useilla eri DNS välimuistiin pakettien, kuten dnsmasq, dnsmasq asennetaan yleisimmät distros seuraavasti:

- **Ubuntu (käyttää resolvconf)**:
    - Asenna vain dnsmasq-paketti ("sudo dnsmasq piharakennus get Asenna").
- **SUSE (käyttää netconf)**:
    - Asenna dnsmasq-paketti ("sudo zypper Asenna dnsmasq") 
    - Ota käyttöön dnsmasq-palvelun ("systemctl dnsmasq.service Ota") 
    - Käynnistä dnsmasq-palvelu ("systemctl start dnsmasq.service") 
    - Muokkaa "/ jne/sysconfig/verkon/config" ja muuta NETCONFIG_DNS_FORWARDER = "", "dnsmasq"
    - päivittää resolv.conf ("netconfig päivitys"), voit määrittää välimuistin paikalliset DNS-tulkintatoiminnon
- **OpenLogic (käyttää NetworkManager)**:
    - Asenna dnsmasq-paketti ("sudo yum Asenna dnsmasq")
    - Ota käyttöön dnsmasq-palvelun ("systemctl dnsmasq.service Ota")
    - Käynnistä dnsmasq-palvelu ("systemctl start dnsmasq.service")
    - Lisää "koskevan toimialue-nimipalvelimet-127.0.0.1;", "/etc/dhclient-eth0.conf"
    - Käynnistä verkkopalvelu ("palvelu uudelleen verkon") määrittää paikallisen DNS-tulkintatoiminnon välimuisti

> [AZURE.NOTE] 'Dnsmasq' pakkaus on vain yksi käytettävissä Linux monta DNS tallentaa.  Ennen kuin käytät sitä, tarkista sen sopivuus tietyn tarpeisiin ja muut välimuistin asennetun.

**Asiakkaan uudelleenyritykset:**

DNS on ensisijaisesti UDP-protokolla.  Kun UDP-protokollan ei takaa viestin lähettämisen, yritä logiikan käsitellään itse DNS-protokolla.  Kukin DNS-asiakasohjelma (käyttöjärjestelmää) voivat toimia eri uudelleen logiikan laatijat tilaksi mukaan:

 - Windows-käyttöjärjestelmien uudelleen jälkeen 1 toinen ja sitten uudelleen, kun toinen 2, 4 ja toiseen 4 sekuntia. 
 - 5 sekunnin kuluttua oletusarvon Linux asetukset-uudelleenyritykset.  On suositeltavaa muuttaa yrittää 5 luvulla 1 sekunnin välein.  

Voit tarkistaa nykyisen asetuksia Linux AM "kate /etc/resolv.conf" ja "asetukset" olevalle riville katsaus esimerkiksi seuraavasti:

    options timeout:1 attempts:5

Resolv.conf-tiedosto on yleensä automaattisesti luodut ja ei voi muokata.  "Asetukset"-rivin lisääminen vaiheet vaihtelevat distro mukaan:

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
On useita tilanteissa nimi tarkkuus tarpeitasi saattaa Siirry lisäksi ominaisuuksia Azure, esimerkiksi kun käyttämällä Active Directory-toimialueet tai kun tarvitset DNS-ratkaisun virtual verkkojen (vnets) välillä.  Näissä tilanteissa kattamaan Azure tarjoaa mahdollisuuden voi käyttää omia DNS-palvelimet.  

DNS-palvelimet virtual verkossa oleville voi välittää DNS-kyselyt Azure on rekursiivinen resolvers ratkaisemiseksi isäntänimet virtual verkon sisällä.  Esimerkiksi toimialueen ohjauskoneen (toimialueen Ohjauskoneen) Azure käynnissä voit vastata sen toimialueen DNS-kyselyt ja kaikki muut kyselyt toimittaa Azure.  Näin näet paikallinen resurssit (joko Ohjauskoneen) ja antamalla Azure isäntänimet (joko forwarder) VMs.  Azure on rekursiivinen resolvers käyttöoikeus on annettu virtual IP 168.63.129.16 kautta.

DNS-edelleenlähetys myös muun vnet DNS-ratkaisun avulla ja avulla paikallinen-koneet antamalla Azure isäntänimet ratkaisemiseksi.  Haluat ratkaista AM hostname DNS-palvelin AM on sijaittava virtual samassa verkossa ja määritettävä Azure eteenpäin hostname kyselyjä.  DNS-liite on erilainen jokaiselle vnet, ehdollinen sääntöjen avulla voit lähettää DNS-kyselyt oikea vnet tarkkuutta.  Seuraavassa kuvassa on kaksi vnets ja suorittamalla muun vnet DNS-ratkaisun tällä menetelmällä paikallinen-verkkoon.  Esimerkki-DNS-forwarder on käytettävissä [Azure pikaopas mallivalikoimasta](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) ja [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Muun vnet DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Käytettäessä antamalla Azure nimenselvitys, sisäiseen DNS-liite (*. internal.cloudapp.net) on annettu kunkin AM DHCP: N avulla.  Näin hostname tarkkuus hostname tietueet ovat internal.cloudapp.net vyöhykkeellä.  Oman nimen tarkkuus-ratkaisun käytettäessä IDN liitteen ei ole toimitettu VMs koska se häiritsee muihin DNS arkkitehtuureihin (kuten toimialueeseen liittymistä skenaariot).  Sen sijaan annamme ei toimi paikkamerkin (reddog.microsoft.com).  

Tarvittaessa sisäiseen DNS-liite voidaan määrittää PowerShell tai Ohjelmointirajapinnan avulla:

-  Resurssien hallinnan käyttöönotto mallien virtual verkot liitteen on saatavana [verkkokortti](https://msdn.microsoft.com/library/azure/mt163668.aspx) resurssi tai [Hae AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) cmdlet-komennon kautta.    
-  Perinteinen käyttöönoton malleissa liitteen on käytettävissä, [Hae käyttöönoton API](https://msdn.microsoft.com/library/azure/ee460804.aspx) -kutsu tai kautta [Get-AzureVM-virheenkorjaus](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet-komento.


Jos soitonsiirto kyselyjen Azure ei vastaa tarpeitasi, sinun on omia DNS-ratkaisu.  DNS-ratkaisun on:

-  Anna tarvittavat hostname tarkkuus kuten [DDNS](virtual-networks-name-resolution-ddns.md)kautta.  Huomautus, jos haluat poistaa käytöstä DNS record vanhentuneiden Azure's DHCP-varauksia ovat todella pitkä ja puhdistuksen DDNS poistaa ennenaikaisesti DNS-tietueet. 
-  Anna tarvittavat rekursiivinen tarkkuutta haluat sallia ulkoisten toimialuenimien tarkkuus.
-  On käytettävissä (TCP- ja UDP-porttiin 53) se toimii asiakkaiden ja voivat käyttää internet.
-  Käyttöä vastaan voidaan suojata Internetistä pienentämään uhkien aiheuttaman ulkoisen agenttien vuoksi.

> [AZURE.NOTE] Parhaan suorituskyvyn, kun Käytä Azure VMs DNS-palvelimet, IPv6 käytöstä ja [Esiintymän tason julkiseen IP](virtual-networks-instance-level-public-ip.md) määritetään DNS-palvelimien AM.  Jos haluat käyttää Windows Server DNS-palvelimeen, [Tässä artikkelissa](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) on lisätietoja suorituskyvyn analysointi ja optimointi.


### <a name="specifying-dns-servers"></a>DNS-palvelimet määrittäminen

DNS-palvelimet käytettäessä Azure on mahdollisuus määrittää useita DNS-palvelimet VPN tai verkon (resurssien hallinta)-liittymän kohti / cloud service (perinteinen).  DNS-palvelimet cloud-palvelun ja verkko-liittymän määritetty Hae ohittaa määritettyjä virtual verkon kautta.

> [AZURE.NOTE] Verkon yhteyden ominaisuudet, kuten DNS-palvelimen IP-osoitteet, ei olisi muokata suoraan Windows VMs, koska ne voivat saada poistamiselta palvelun heal virtual verkkosovittimen saa korvata. 


Käytettäessä resurssien hallinnan käyttöönottomalli voidaan määrittää DNS-palvelimet Portal-Ohjelmointirajapinnan/mallit ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) tai PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Kun käytät perinteistä käyttöönotto-mallin, virtual verkon DNS-palvelimet voidaan määrittää Portal tai [ *Verkon määritys* -tiedostossa](https://msdn.microsoft.com/library/azure/jj157100).  DNS-palvelimet määritetään cloud Services- [ *Palvelun määritykset* ](https://msdn.microsoft.com/library/azure/ee758710) -tiedostolla tai powershellissä ([Uusi AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [AZURE.NOTE] Jos muutat DNS-asetusten virtual verkon/virtual laitteeseen, joka on jo otettu käyttöön, sinun on käynnistettävä uudelleen otetaan käyttöön muutosten kunkin tarvittavien AM.


## <a name="next-steps"></a>Seuraavat vaiheet

Resurssien hallinnan käyttöönottomalli:

- [Luo tai päivitä virtual verkossa](https://msdn.microsoft.com/library/azure/mt163661.aspx)
- [Luo tai päivitä verkkokortti](https://msdn.microsoft.com/library/azure/mt163668.aspx)
- [Uusi AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
- [Uusi AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

 
Perinteinen käyttöönoton mallia:

- [Azure määritysten malli](https://msdn.microsoft.com/library/azure/ee758710)
- [VPN-määritys rakenne](https://msdn.microsoft.com/library/azure/jj157100)
- [Määrittää Virtual verkon verkon määritys-tiedoston avulla](virtual-networks-using-network-configuration-file.md) 

