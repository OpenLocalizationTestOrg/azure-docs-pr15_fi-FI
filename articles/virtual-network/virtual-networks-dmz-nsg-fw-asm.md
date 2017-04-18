<properties
   pageTitle="DMZ Esimerkki – muodosta DMZ suojaaminen sovellusten palomuuri ja NSGs | Microsoft Azure"
   description="DMZ, palomuurin ja verkon käyttöoikeusryhmät (NSG) luominen"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-2--build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs"></a>Esimerkki 2 – muodosta DMZ sovellusten palomuuri ja NSGs suojaaminen

[Palaa suojauksen reunan parhaat käytännöt sivulle][HOME]

Tässä esimerkissä luodaan Jos kyseessä on palomuuri, neljä windows-palvelimien ja verkon käyttöoikeusryhmät. Se käy läpi kunkin vaiheen tarkempaa tietoa antamaan asiaa komennot. On myös tietoliikenteen skenaarion osan antamaan perusteellisempaa vaiheittaiset ohjeet, miten liikenne etenee kerrokset DMZ linnaa kautta. -Viitteet osa on valmis koodi ja ohjepaketti luonnissa ja kokeilla eri tilanteista tässä ympäristössä. 

![Saapuvan DMZ NVA ja NSG][1]

## <a name="environment-description"></a>Ympäristön kuvaus
Tässä esimerkissä on tilauksessa, joka sisältää seuraavat:

- Kaksi cloud services: "FrontEnd001" ja "BackEnd001"
- Virtuaalinen verkon "CorpNetwork" kahden aliverkon kanssa: "FrontEnd" ja "Taustajärjestelmä"
- Yhden verkoston käyttöoikeusryhmän, jota käytetään sekä aliverkosta
- Edusta-aliverkon yhteydessä, tässä esimerkissä Barracuda NextGen on palomuuri verkon virtual laitteen
- Windows Server, joka edustaa sovelluksen verkkopalvelin ("IIS01")
- Kaksi ikkunaa palvelimissa, jotka edustavat sovelluksen takaisin lopettaa servers ("AppVM01", "AppVM02")
- Windows server, joka edustaa DNS server ("DNS01")

>[AZURE.NOTE] Tässä esimerkissä käytetään Barracuda NextGen palomuuri, vaikka monet eri verkon Virtual laitteiden voidaan käyttää tässä esimerkissä.

Viittaukset-kohdassa on PowerShell-komentosarja, joka luo useimmat edellä kuvatun ympäristön. VMs ja Virtual verkkojen vaikka Esimerkki komentosarjalla valmis on ei ole kuvattu tämän asiakirjan.
 
Voit muodostaa ympäristön seuraavasti:

  1.    Tallentaa verkon config xml-tiedoston, viittaukset-luokasta (päivitetty nimet, sijainti ja IP vastaamaan tapaukselle osoitteet)
  2.    Päivitä käyttäjän muuttujien komentosarjan vastaamaan ympäristön komentosarja on suoritettava vastaan (tilaukset, palveluiden nimet jne.)
  3.    Suorita PowerShell-komentosarja

**Huomautus**: PowerShell-komentosarjaa esittää alueen on vastattava verkon määritysten xml-tiedoston esittää alue.

Kun komentosarjan suorittaminen onnistuu voidaan toteuttaa jälkeinen komentosarjan seuraavasti:

1.  Määrittää palomuurisäännöt, tämä käsitellään jäljempänä olevaan kohtaan: palomuurisäännöt.
2.  Voit myös viittaukset-kohdassa on kaksi komentosarjojen verkkopalvelin ja app-palvelin sallimaan testaaminen oikeilla DMZ määritysten yksinkertaisen web-sovelluksen määrittäminen.

Seuraavassa osassa kerrotaan useimmat komentosarjoja verkon käyttöoikeusryhmät suhteessa väittämistä.

## <a name="network-security-groups-nsg"></a>Verkon käyttöoikeusryhmät (NSG)
Tässä esimerkissä NSG ryhmän sisäisten ja ladata sitten kuusi säännöillä. 

>[AZURE.TIP] Niinkään kannattaa luoda tietyn "Salli" sääntöjen ensin ja sitten yleisen "Estä" sääntöjä viimeksi. Sääntöjen on määritetty tärkeyden mukaan määräytyy lasketaan ensin. Kun liikenne löytyy tietyn säännön koskee, arvioidaan edelleen sääntöjä ei ole. NSG sääntöjä voit käyttää joko (aliverkon näkökulmasta) saapuvat tai Lähtevät suuntaan.

Määrittämisen avulla seuraavat säännöt rakennetaan saapuvan liikenteen:

1.  Sisäiseen DNS-liikenne (portti 53) on sallittua
2.  Mikä tahansa AM RDP liikenteen (portti 3389) Internetistä sallitaan
3.  NVA (palomuuri) HTTP liikenteen (portti 80) Internetistä sallitaan
4.  Minkä tahansa liikenteen (kaikki portit)-IIS01 AppVM1 sallitaan
5.  Minkä tahansa liikenteen (kaikki portit) Internetistä kokonaan VNet (molemmat aliverkosta) on estetty
6.  Minkä tahansa liikenteen (kaikki portit) edusta-aliverkosta Taustajärjestelmä aliverkon on estetty

Sääntöjen ja sitoa kunkin aliverkon, jos HTTP-pyyntö on ollut saapuva Internetistä WWW-palvelimeen, sekä sääntöjä 3 (Salli) ja 5 (estä) sovelletaan, mutta koska sääntö 3 on suuri prioriteetti vain sen sovelletaan ja säännön 5 tule toista kyselyjä. Näin HTTP-pyyntö voivat palomuurin. Jos sama liikenne yritti saavuttaa DNS01 palvelimeen, säännön 5 (estä) on ensimmäinen sovelletaan ja liikenne eivät voi siirtää palvelimeen. Säännön 6 (estä) estää edusta-aliverkon-puhelu Taustajärjestelmä aliverkon (lukuun ottamatta sallittu liikenteen säännöt 1 – 4), tämä suojaa Taustajärjestelmä verkon tapauksessa voi saada-kompromisseja, web-sovelluksen edusta-hän on rajoitetut käyttöoikeudet Taustajärjestelmä "suojattu" verkon (vain resursseja tarjoamia AppVM01 palvelimessa).

Ei oletusarvoisesti lähtevän säännön, joka sallii Internet-liikenteen ulos. Tässä esimerkissä on olet sallimisen liikenteen ja muokkaaminen ei ole mitään lähtevän liikenteen säännöt. Lukitse liikennettä sekä suuntaan ja käyttäjän määrittämät reititys tarvitaan, tämä on tarkasteltavia eri esimerkissä voi [tärkeimpiin reunaa asiakirjan]sisältämien[HOME].

Yllä haavoittuvuuksiin NSG säännöt ovat hyvin samankaltaisia kuin [Esimerkki 1 - luoda yksinkertaisen DMZ kanssa NSGs]NSG säännöt[Example1]. Tarkista NSG niiden sääntöjen ja sen määritteet yksityiskohtaiset tarkastella asiakirjan NSG kuvauksen.

## <a name="firewall-rules"></a>Palomuurisäännöt
Hallinta asiakkaalle on asennettu tietokoneeseen, voit hallita palomuurin ja luo tarvittavat määritykset. Katso palomuurin (tai muita NVA) toimittajan hallita laitetta. Loput tässä osassa kuvataan määritys palomuuri itse toimittajien hallinta-asiakasohjelman (eli ei Azure portal tai PowerShell) kautta.

Täältä löytyvät ohjeet asiakasohjelman lataaminen ja yhteyden muodostaminen tässä esimerkissä käytetään Barracuda: [Barracuda maa-järjestelmänvalvoja](https://techlib.barracuda.com/NG61/NGAdmin)

Palomuurin lähettäminen edelleen sääntöjen on luotava. Tässä esimerkissä reitittää vain internet-liikenne-sidottujen palomuurin ja sitten WWW-palvelimeen, koska vain yksi edelleenlähetys NAT säännön tarvita. Tässä esimerkissä käytetään Barracuda NextGen palomuurin säännön olisi kohteen NAT säännön ("kesäajan NAT") välittää tämän liikenne.

Luo seuraavaa sääntöä (tai Tarkista aiemmin luotuja oletusarvon sääntöjä) lähtien Barracuda maa järjestelmänvalvojan asiakkaan raporttinäkymät-ikkuna, siirry määritys-välilehti, napsauta toiminta-asetukset-osassa sääntöjoukko. Kutsua ruudukossa "Main säännöt" Näytä aiemmin aktiivinen ja aktivointi säännöt, palomuurin. Tämä ruudukko oikeassa yläkulmassa on pieni, vihreä "+"-painiketta, valitse tämä, jos haluat luoda uuden säännön (Huomautus: palomuurin voi olla "lukittu" muutokset, jos näet painikkeen lukee "Lukita" ja et voi luoda tai muokata sääntöjä, napsauttamalla tätä painiketta "lukituksen" sääntöjoukko ja muokkausta varten). Jos haluat muokata aiemmin luotua sääntöä, valitse sääntöön, napsauta hiiren kakkospainikkeella ja valitse Muokkaa sääntöä.

Luo uusi sääntö ja Anna nimi, esimerkiksi "WebTraffic". 

Säännön kohde NAT-kuvake näyttää tältä: ![Kohde NAT-kuvake][2]

Säännön itse näyttää suunnilleen tältä:

![Palomuurisääntö][3]

Tähän jokin saapuva osoite, jota käynnit palomuurin haettavaa HTTP (80 ja 443, HTTPS portti) palomuurin "DHCP1 paikallinen IP-liittymän lähetetään ja ohjataan verkkopalvelin 10.0.1.5 IP-osoite. Koska liikenne on pian saatavilla porttiin 80 ja siirtymällä verkkopalvelin porttiin 80 on tarvittavat portti ei muutu. Kuitenkin kohdeluettelosta voi johtua 10.0.1.5:8080 Jos Microsoftin verkkopalvelin kuunteli porttiin 8080 kääntäminen näin saapuvaa porttia 80 palomuurin saapuvan porttiin 8080 WWW-palvelimessa.

Yhteystapa pitäisi myös voidaan esittää, kohde säännön Internetistä "Dynaaminen SNAT" on sopivin. 

Vaikka vain yksi sääntö on luotu on tärkeää, että sen prioriteetti on määritetty oikein. Jos kaikki palomuurin säännöt ruudukon uuden säännön on alareunassa (alla "BLOCKALL"-sääntö) se tulee aina toista kyselyjä. Varmista web tietoliikenteen juuri luomasi sääntö on suurempi kuin BLOCKALL sääntö.

Kun sääntö on luotu, sen on oltava miten palomuurin ja sitten aktivoitu, jos näin ei tehdä säännön muutokset tulevat voimaan. Push- ja aktivoinnin prosessi on kuvattu seuraavassa osassa.

## <a name="rule-activation"></a>Säännön aktivointi
Sääntöjoukko, voit lisätä tämän säännön muokattu, jossa sääntöjoukko on ladattu palomuurin ja aktivoitu.

![Palomuurin säännön aktivointi][4]

Ovat yläkulmassa hallinta-asiakasohjelman klusterin painikkeista. Muokattu säännöt lähettäminen palomuurin "Lähetä muutoksia"-painiketta ja valitse sitten "Aktivoi"-painiketta.

Palomuurin sääntöjoukko aktivointi tässä esimerkissä ympäristön muodosta on valmis. Voit myös viittaukset-kohdassa Julkaise muodosta-komentosarjat voidaan suorittaa Testaa ympäristöön tämän sovelluksen lisääminen-liikenne skenaariot alapuolella.

>[AZURE.IMPORTANT] On ehdottoman tärkeää huomaat, että ei ole valitset verkkopalvelin suoraan. Kun selain pyytää HTTP-sivun FrontEnd001.CloudApp.Net, HTTP-päätepisteen (portti 80) välittää tämän liikenne palomuurin ei verkkopalvelin. Palomuurin sitten säännön luotu edellä NAT, joka pyytää WWW-palvelimeen.

## <a name="traffic-scenarios"></a>Liikenne skenaariot

#### <a name="allowed-web-to-web-server-through-firewall"></a>(Sallittu) Web-Web-palvelin palomuurin läpi
1.  Internet käyttäjän pyynnöt HTTP sivun FrontEnd001.CloudApp.Net (Internet vastakkaisten pilvipalvelussa)
2.  Cloud palvelun siirtoja liikenne 10.0.1.4:80 porttiin 80 palomuurin paikallisen liittymään Avaa päätepiste
3.  Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 3 (Internet-palomuurin) käyttää, liikenne on sallittu, lopetus säännön käsittely
4.  Liikenne käynnit sisäinen IP-osoite palomuuri (10.0.1.4)
5.  Palomuurin edelleenlähetyssäännön Katso tämä on portti 80 liikenne, ohjaa verkkopalvelin IIS01
6.  IIS01 web tietoliikenteen listening, saa pyynnön ja aloittaa käsittelyn pyyntö
7.  IIS01 pyydetään SQL Server-AppVM01 tietoja
8.  Ei edusta aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
9.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 3 (Internet-palomuurin) ei lisää, siirry seuraavaan säännön
  4.    NSG säännön 4 (IIS01 AppVM01) käyttää, liikenne on sallittua, säännön käsittelyn lopettaminen
10. AppVM01 vastaanottaa SQL-kysely ja vastaa
11. Koska ei ole lähtevän liikenteen säännöt Taustajärjestelmä aliverkon vastauksen sallitaan
12. Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    Ei ole NSG sääntö, joka koskee saapuvien tietoliikenteen taustaan aliverkosta edusta-aliverkon, jotta ei mitään NSG sääntöjen käyttäminen
  2.    Oletusarvoinen järjestelmän säännön salliminen liikenne välillä aliverkosta Salli tämän liikenne liikenne sallitaan.
13. IIS-palvelin vastaanottaa SQL-vastauksen ja täydentää HTTP-vastauksen ja lähettää pyytäjän
14. Tämä on NAT-palomuurin istuntoa, vastaus kohde on (aluksi) palomuurin
15. Palomuurin saa vastausta verkkopalvelin ja välittää takaisin Internet-käyttäjä
16. Koska käsittelyalueella on ei lähtevän liikenteen säännöt edusta-aliverkon vastaus on sallittu ja Internet-käyttäjä saa pyydetty web-sivua.

#### <a name="allowed-rdp-to-backend"></a>(Sallittu) RDP taustaan
1.  Palvelimen järjestelmänvalvoja Internetissä pyytää RDP-istunto AppVM01, missä xxxxx RDP (varatun portin löytyy Azure-portaalissa tai PowerShell) AppVM01 satunnaisesti määritetty porttinumero BackEnd001.CloudApp.Net:xxxxx
2.  Palomuurin vain kuunteleminen FrontEnd001.CloudApp.Net osoitteen, koska siihen liittyvän ole liikenne tämä työnkulku
3.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) käyttää, liikenne on sallittu, lopetus säännön käsittely
4.  Oletusarvon säännöt otetaan käyttöön ja palaa liikenne on sallittua ei lähtevän liikenteen säännöt
5.  RDP-istunnon on otettu käyttöön
6.  AppVM01 pyytää käyttäjänimi salasana

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Sallittu) DNS-palvelimen WWW Server DNS-haku
1.  WWW-palvelimeen, IIS01, tietosyötteestä osoitteessa www.data.gov tarpeisiin, mutta tarvitsee selvittää osoitetta.
2.  Verkkokokoonpanon VNet luetteloiden DNS01 (10.0.2.4 Taustajärjestelmä aliverkon) kuin ensisijainen DNS-palvelin, IIS01 lähettää DNS-pyynnön DNS01
3.  Ei edusta aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
4.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.     NSG säännön 1 (DNS) käyttää, liikenne on sallittu, lopetus säännön käsittely
5.  DNS-palvelin vastaanottaa pyynnön
6.  DNS-palvelin ei ole välimuistiin osoite ja pyytää pääkansion DNS-palvelin Internetissä
7.  Ei ole Taustajärjestelmä aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
8.  Internet-DNS-palvelin vastaa, koska tämä istunto alkoi sisäisesti, vastaus on sallittu
9.  DNS-palvelin välimuistiin vastauksen ja vastaa alkuperäisen pyynnön takaisin IIS01
10. Ei ole Taustajärjestelmä aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
11. Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    Ei ole NSG sääntö, joka koskee saapuvien tietoliikenteen taustaan aliverkosta edusta-aliverkon, jotta ei mitään NSG sääntöjen käyttäminen
  2.    Oletusarvoinen järjestelmän säännön salliminen liikenne välillä aliverkosta antaa tämän liikenne jotta liikenne sallitaan
12. IIS01 saa vastausta DNS01

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(Sallittu) WWW-palvelimen access-tiedoston AppVM01
1.  IIS01 pyytää AppVM01-tiedosto
2.  Ei edusta aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
3.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 3 (Internet-palomuurin) ei lisää, siirry seuraavaan säännön
  4.    NSG säännön 4 (IIS01 AppVM01) käyttää, liikenne on sallittua, säännön käsittelyn lopettaminen
4.  AppVM01 saa pyynnön ja vastaa-tiedoston (olettaen että valtuuksia)
5.  Koska ei ole lähtevän liikenteen säännöt Taustajärjestelmä aliverkon vastauksen sallitaan
6.  Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    Ei ole NSG sääntö, joka koskee saapuvien tietoliikenteen taustaan aliverkosta edusta-aliverkon, jotta ei mitään NSG sääntöjen käyttäminen
  2.    Oletusarvoinen järjestelmän säännön salliminen liikenne välillä aliverkosta Salli tämän liikenne liikenne sallitaan.
7.  IIS-palvelin vastaanottaa tiedoston

#### <a name="denied-web-direct-to-web-server"></a>(Estetty) Web suoraan verkkopalvelin
Koska verkkopalvelimeen, IIS01 ja palomuurin on sama pilvipalvelussa ne jakaa saman julkinen aukeaman IP-osoite. Näin HTTP liikenteen ohjata palomuurin. Kun pyynnön served onnistuneesti, sitä ei voi siirtyä suoraan verkkopalvelimeen, viesti on kulkenut, suunniteltu, palomuurin läpi ensin. Katso tämän osion liikenne kulkuun ensimmäinen skenaario.

#### <a name="denied-web-to-backend-server"></a>(Estetty) Sivuston palvelimeen
1.  Internet-käyttäjä yrittää käyttää AppVM01 tiedoston BackEnd001.CloudApp.Net-palvelun kautta
2.  Avaa jaettu tiedostoresurssi on päätepisteitä, tämä Välitä pilvipalvelussa ja Eikö yhteys palvelimeen
3.  Jos päätepisteet oli avattu jostain syystä, NSG säännön 5 (Internet-VNet) estää tämän tietoliikenteen

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(Estetty) Web DNS-haun DNS-palvelimeen
1.  Internet-käyttäjä yrittää haku sisäinen DNS-tietue-DNS01 BackEnd001.CloudApp.Net-palvelun kautta
2.  Koska päätepisteitä ei ole avoinna DNS-tämä Välitä pilvipalvelussa ja Eikö yhteys palvelimeen
3.  Jos päätepisteet oli avattu jostain syystä, NSG säännön 5 (Internet-VNet) estää tämän tietoliikenteen (Huomautus: kyseisen säännön 1 (DNS) eivät päde kahdesta syystä, ensin lähdeosoite on Internetissä, tämä sääntö koskee vain paikallisen VNet lähteenä, myös tämä on Sallimissääntö, joten se koskaan estä liikenne)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(Estetty) Palomuurin läpi SQL Access Web
1.  Internet-käyttäjä pyytää FrontEnd001.CloudApp.Net (Internet vastakkaisten pilvipalvelussa) SQL-tiedot
2.  Koska päätepisteitä ei ole avoinna SQL-tämä Välitä pilvipalvelussa ja Eikö saavuttaa palomuurin
3.  Jos päätepisteet oli avattu jostain syystä, edusta-aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 2 (Internet-palomuurin) käyttää, liikenne on sallittu, lopetus säännön käsittely
4.  Liikenne käynnit sisäinen IP-osoite palomuuri (10.0.1.4)
5.  Palomuuri on ei ole edelleenlähetyssääntöjä SQL ja pudottaa liikenne

## <a name="conclusion"></a>Tekemistä
Tämä on suhteellisen suoraan eteenpäin keino suojaaminen sovelluksesi, jossa on palomuuri, ja eristää kyseinen uudelleen aliverkon saapuvan liikenteen.

Lisää esimerkkejä sekä verkon suojauksen rajat yleiskatsaus löytyy [tähän][HOME].

## <a name="references"></a>Viittaukset
### <a name="main-script-and-network-config"></a>Tärkeimmät komentosarja ja verkon määritys
Tallenna koko komentosarja PowerShell-komentosarjatiedosto. Verkko-määritysten tallentaminen "NetworkConf2.xml"-tiedostoon.
Muokkaa käyttäjän määrittämät muuttujat tarpeen mukaan. Suorita komentosarja sitten noudattamalla palomuurin säännön asetukset edellä.

#### <a name="full-script"></a>Koko komentosarja
Tämä komentosarja on käyttäjän määrittämät muuttujat perusteella:

1.  Yhteyden muodostaminen Azure tilauksen
2.  Luo uusi tallennustilan-tili
3.  Luo uusi VNet ja kahden aliverkon määritysten verkon määritystiedostossa
4.  Muodosta 4 windows server VMs
5.  Määritä NSG mukaan lukien:
  - NSG luominen
  - Täyttää sääntöihin
  - Sidonta NSG tarvittavat aliverkosta

PowerShell-komentosarja on suoritettava paikallisesti edelleen internet-yhteys PC- tai palvelimen.

>[AZURE.IMPORTANT] Kun tämä komentosarja on suoritettu, voi olla varoituksia tai muita ilmoituksia, joihin pop-PowerShell. Vain virhesanomat punaisella ovat syy huolenaiheita.


    <# 
     .SYNOPSIS
      Example of DMZ and Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet (plus the NVA on the FrontEnd subnet)
       - Three Servers on the BackEnd Subnet
       - Network Security Groups to allow/deny traffic patterns as declared
      
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Security requirements are different for each use case and can be addressed in a
      myriad of ways. Please be sure that any sensitive data or applications are behind
      the appropriate layer(s) of protection. This script serves as an example of some
      of the techniques that can be used, but should not be used for all scenarios. You
      are responsible to assess your security needs and the appropriate protections
      needed, and then effectively implement those protections.
    
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       myFirewall - 10.0.1.4
       IIS01      - 10.0.1.5
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf2.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 1
        #       - The AppVM1 Server must be VM 2
        #       - The DNS server must be VM 4
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $FrontEndService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: Web traffic goes through the firewall, so we'll need to set up a HTTP endpoint.
                #       Also, the firewall will be redirecting web traffic to a new IP and Port in a
                #       forwarding rule, so the HTTP endpoint here will have the same public and local
                #       port and the firewall will do the NATing and redirection as declared in the
                #       firewall rule.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                    # Note: A Remote Desktop endpoint is automatically created when each VM is created.
                }
            $i++
        }
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[4] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[1]) to $($VMName[2])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[2] -DestinationPortRange '*' `
            -Protocol *
        
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix -DestinationPortRange '*' `
            -Protocol *
    
        # Assign the NSG to the Subnets
            Write-Host "Binding the NSG to both subnets" -ForegroundColor Cyan
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
            Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host


#### <a name="network-config-file"></a>Verkko-asetustiedosto
Tallenna xml-tiedoston päivitetty sijainti ja Lisää linkki tähän tiedostoon edellä komentosarjan $NetworkConfigFile muuttujan.
    
    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Sovelluksen komentosarjamallit
Jos haluat asentaa sovelluksen malli tälle ja DMZ muita esimerkkejä jokin on annettu seuraava linkki: [Esimerkki sovelluksen komentosarja][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-asm/example2design.png "Saapuvien DMZ NSG kanssa"
[2]: ./media/virtual-networks-dmz-nsg-fw-asm/dstnaticon.png "Kohde NAT-kuvake"
[3]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallrule.png "Palomuurisääntö"
[4]: ./media/virtual-networks-dmz-nsg-fw-asm/firewallruleactivate.png "Palomuurin säännön aktivointi"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
[Example1]: ./virtual-networks-dmz-nsg-asm.md
