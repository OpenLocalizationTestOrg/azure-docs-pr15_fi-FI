<properties
   pageTitle="DMZ Esimerkki – muodosta DMZ suojaamaan verkkoja palomuuri, UDR ja NSG | Microsoft Azure"
   description="Luoda DMZ, jossa on palomuuri, käyttäjän määrittämä reititys (UDR) ja verkko-käyttöoikeusryhmät (NSG)"
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

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Esimerkki 3 – muodosta DMZ verkkojen palomuuri, UDR ja NSG suojaaminen

[Palaa suojauksen reunan parhaat käytännöt sivulle][HOME]

Tässä esimerkissä luodaan Jos kyseessä on palomuuri, neljä windows-palvelimien käyttäjän määrittämät reititys, IP-osoitteen välityksen tai verkon käyttöoikeusryhmät. Se käy läpi kunkin vaiheen tarkempaa tietoa antamaan asiaa komennot. On myös tietoliikenteen skenaarion osan antamaan perusteellisempaa vaiheittaiset ohjeet, miten liikenne etenee kerrokset DMZ linnaa kautta. -Viitteet osa on valmis koodi ja ohjepaketti luonnissa ja kokeilla eri tilanteista tässä ympäristössä. 

![Kaksisuuntaisen DMZ NVA, NSG ja UDR][1]

## <a name="environment-setup"></a>Ympäristön valmisteleminen
Tässä esimerkissä on tilauksessa, joka sisältää seuraavat:

- Kolme cloud services: "SecSvc001", "FrontEnd001" ja "BackEnd001"
- Virtuaalinen verkon "CorpNetwork", jossa on kolme aliverkosta: "SecNet", "FrontEnd" ja "Taustajärjestelmä"
- SecNet aliverkon yhteydessä, tässä esimerkissä on palomuuri verkon virtual laitteen
- Windows Server, joka edustaa sovelluksen verkkopalvelin ("IIS01")
- Kaksi ikkunaa palvelimissa, jotka edustavat sovelluksen takaisin lopettaa servers ("AppVM01", "AppVM02")
- Windows server, joka edustaa DNS server ("DNS01")

Viittaukset-kohdassa on PowerShell-komentosarja, joka luo useimmat edellä kuvatun ympäristön. VMs ja Virtual verkkojen vaikka Esimerkki komentosarjalla valmis on ei ole kuvattu tämän asiakirjan.

Voit muodostaa ympäristön seuraavasti:

  1.    Tallentaa verkon config xml-tiedoston, viittaukset-luokasta (päivitetty nimet, sijainti ja IP vastaamaan tapaukselle osoitteet)
  2.    Päivitä käyttäjän muuttujien komentosarjan vastaamaan ympäristön komentosarja on suoritettava vastaan (tilaukset, palveluiden nimet jne.)
  3.    Suorita PowerShell-komentosarja

**Huomautus**: PowerShell-komentosarjaa esittää alueen on vastattava verkon määritysten xml-tiedoston esittää alue.

Kun komentosarjan suorittaminen onnistuu voidaan toteuttaa jälkeinen komentosarjan seuraavasti:

1.  Määrittää palomuurisäännöt, tämä käsitellään jäljempänä olevaan kohtaan: palomuurin säännön kuvaus.
2.  Voit myös viittaukset-kohdassa on kaksi komentosarjojen verkkopalvelin ja app-palvelin sallimaan testaaminen oikeilla DMZ määritysten yksinkertaisen web-sovelluksen määrittäminen.

Kun komentosarja on suoritettu onnistuneesti palomuurin säännöt on tehtävä, tämä käsitellään kohtaan: palomuurisäännöt.

## <a name="user-defined-routing-udr"></a>Käyttäjän määrittämät reititys (UDR)
Oletusarvoisesti seuraavat järjestelmän tiet määritellään seuraavasti:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal on aina kyseisen tietyn verkon (ie se muuttuu osoitteesta VNet VNet sen mukaan, miten kukin tietyn VNet on määritetty) VNet määritetty osoite-prefix(es). Jäljellä oleva järjestelmän tiet on staattinen ja kuin yllä oletus.

Prioriteetti, kuten tiet käsitellään pisimmän etuliitteen vastine (LPM)-menetelmällä, näin tarkkaa reitin taulukon koskee annetun kohdeosoite.

Tämän vuoksi liikenne (esimerkiksi paikalliseen 10.0.2.4 DNS01-palvelin), jotka on tarkoitettu lähiverkossa (10.0.0.0/16) reititetään yli VNet perillepääsy 10.0.0.0/16 reitin vuoksi. Toisin sanoen 10.0.2.4, 10.0.0.0/16 reitin on tarkkaa reitin, vaikka 10.0.0.0/8 ja 0.0.0.0/0 myös tyylisivua, mutta koska ne ovat vähemmän tietyn muutokset eivät vaikuta tämän liikenne. Näin 10.0.2.4 tietoliikennettä tapauksessa on paikallinen VNet seuraavan pisteen ja reitittää yksinkertaisesti kohteeseen.

Jos liikenne on tarkoitettu 10.1.1.1 esimerkiksi, 10.0.0.0/16 reitin Eikö käyttää, mutta 10.0.0.0/8 olisi mahdollisimman tietyn ja liikenne olisi pudotettu ("musta holed"), koska seuraavan pisteen arvo on Null. 

Jos kohde ei koske jokin Null-etuliitteitä tai VNETLocal etuliitteitä ja valitse se noudattamalla vähintään tietyn reitittää 0.0.0.0/0 ja voidaan reitittää Internet, seuraavan pisteen ja näin ollen ulos Azure päivän internet reuna.

Jos reititys-taulukossa on kaksi samat etuliitteitä, seuraavassa on järjestyksen preference tiet "lähde-määrite perusteella:

1.  "VirtualAppliance" = käyttäjän määritetyt reitin manuaalisesti lisätty taulukkoon
2.  "VPNGateway" = dynaaminen reititys (erityisen hybrid verkkojen käytettäessä) lisäämät dynaaminen verkkoprotokollaa nämä tiet voivat muuttua ajan kuluessa kuin dynaaminen protokolla näkyvät automaattisesti muutokset peered verkossa
3.  "Oletus" = järjestelmän tiet, paikallinen VNet ja staattisia merkintöjä edellä olevan reititystaulukon mukaisesti.

>[AZURE.NOTE] Voit nyt käyttää käyttäjän määrittämät reititys (UDR) ExpressRoute ja VPN yhdyskäytävät Pakota Lähtevä ja Saapuva rajat tiloissa tietoliikenne reititetään verkon: laitteeseen virtual (NVA).

#### <a name="creating-the-local-routes"></a>Paikallinen tiet luominen

Tässä esimerkissä tarvitaan reititys taulukot, yksi edusta- ja Taustajärjestelmä aliverkosta varten. Jokaisessa taulukossa on ladattu staattinen tiet tietyn aliverkon varten. Tässä esimerkissä varten jokaisessa taulukossa on kolme tiet:

1. Paikallisen aliverkon liikenteen seuraava siirräntävälien ei ole määritetty sallimaan paikallisen aliverkon liikenteen ohittaa palomuurin
2. Virtual verkkoliikenteen kanssa seuraavan siirräntävälien lasketaan palomuuri, tämä ohittaa oletusarvon sääntö, joka mahdollistaa paikallisen VNet tietoliikenteen reitittäminen suoraan
3. Kaikki jäljellä olevat liikenne (0/0) kanssa seuraavan siirräntävälien palomuurin lasketaan

Kun reititys taulukot luodaan ne on sidottu niiden aliverkosta. Edusta-aliverkon kerran ja sitoo aliverkon reititys taulukon pitäisi näyttää tältä:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Tässä esimerkissä käytetään seuraavat komennot reititystaulukon luominen ja käyttäjän määrittämät reitin lisääminen reititystaulukon sitoa sitten aliverkon (Huomautus; alkaa sanalla dollarimerkki alla olevat kohteet (esimerkiksi: $BESubnet) on tämän oppaan komentosarja käyttäjän määrittämät muuttujat):

1.  Ensin on luotava reititys perustaulukon. Tämä koodikatkelman näyttää Taustajärjestelmä aliverkon taulukon luomisen. Komentosarjan vastaavan taulukko luodaan myös edusta-aliverkon.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Kun reitti-taulukko luodaan, tietyn käyttäjän määrittämät tiet voidaan lisätä. Tässä, jotka on otettu kaikki liikenne (0.0.0.0/0) reititetty virtual laitteen (muuttujaa $VMIP [0] käytetään IP-osoite, kun virtual laitteen on luotu aiemmassa komentosarja välittää) kautta. Komentosarjan vastaavan säännön luodaan myös edusta-taulukossa.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. Yllä reitin merkintä ohittaa oletusarvon "0.0.0.0/0" reitin, mutta edelleen olemassa Oletus 10.0.0.0/16 sääntö, jonka sallia liikenteen reitittämään suoraan kohteeseen, etkä halua verkon Virtual laitteen VNet kuluessa. Oikea toiminta seuraa sääntö on lisätään.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. Tässä vaiheessa on tehtävä valinta. Edellä mainitut kaksi tiet kanssa kaikki liikenne reitittää palomuurin arvioinnin jopa liikenne yhden aliverkon sisällä. Tämä ole toivottuja, mutta sallimaan liikenne sisällä aliverkon reitittää paikallisesti ilman osallistuminen palomuurin kolmannen tarkkojen sääntö voidaan lisätä. Tätä vaihtoehtoa, toimi tavoitettavuustilat sekä kaikki destine varten paikallisen aliverkon juuri voit reitittää suoraan (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Lopuksi, kun reititys taulukko luodaan ja jossa käyttäjän määrittämät tiet, taulukko on nyt sidottava aliverkon. Komentosarjan edusta reitti-taulukko on myös sidottu edusta-aliverkon. Seuraavassa on uudelleen aliverkon sidonta-komentosarjan.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP-välityksen
Avustaja-toiminto, jolla UDR, on IP-osoitteen välityksen. Tämä on Virtual laitteen, jonka avulla saat liikenne ei ole nimenomaisesti osoitettu laitteen ja määritä edelleenlähetys ultimate perillepääsy liikenne-asetus.

Esimerkkinä, jos AppVM01 tietoliikenteen tekee pyyntö DNS01-palvelimeen, UDR reitittää tämä palomuuri. Kun IP-osoitteen välityksen käytössä, DNS01 kohde (10.0.2.4) liikenne hyväksynyt laitteen (10.0.0.4) ja siirtää sitten ultimate perillepääsy (10.0.2.4). Ilman IP-osoitteen välityksen käytössä palomuuri liikenne ei voi hyväksyä laitteen vaikka reititys-taulukossa on palomuurin kuin seuraavan kohteen. 

>[AZURE.IMPORTANT] On ehdottoman tärkeää muista ottaa yhdessä käyttäjän määrittämät reititys IP-osoitteen välityksen.

IP-osoitteen välityksen määrittäminen on yhdeksi ja voidaan toteuttaa AM luonnin aikana. Tässä esimerkissä kulkuun koodikatkelman on komentosarja loppua ja ryhmitellä ja UDR-komennot:

1.  Soita AM ‑esiintymä, jossa on virtual laitteen palomuurin tällöin ja ota käyttöön IP-osoitteen välityksen (Huomautus, punainen alkaa sanalla dollarimerkki luettelokohteen (esimerkiksi: $VMName[0]) on tämän artikkelin oppaan komentosarjan käyttäjän määrittämät muuttuja. Nolla hakasulkeisiin [0] edustaa VMs Esimerkki komentosarjan toimimaan ilman muutoksia matriisin ensimmäisen AM, ensimmäinen AM (AM 0) on oltava palomuurin):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Verkon käyttöoikeusryhmät (NSG)
Tässä esimerkissä NSG ryhmän sisäisten ja ladata sitten yksittäisen säännön. Tämän ryhmän sitten on sidottu vain edusta- ja Taustajärjestelmä aliverkosta (ei SecNet). Käyttöliittymän seuraavaa sääntöä on käännetty:

1.  Mikä tahansa liikenteen (kaikki portit) Internetistä koko VNet (kaikki aliverkosta) on estetty

Tässä esimerkissä käytetään NSGs, mutta sen tärkeimmät tarkoituksena on toissijainen kerroksen linnaa manuaalinen virheellisen määrityksen mukaan. Haluat estää kaikki saapuvan liikenteen Internetistä joko edusta- tai Taustajärjestelmä aliverkosta liikenne olisi vain flow SecNet aliverkon palomuurin läpi (ja sitten Jos tarvittaessa edusta-tai Taustajärjestelmä aliverkosta). Plus-UDR sääntöjen paikassa, liikenteestä, joka tekemällä siitä edusta- tai Taustajärjestelmä aliverkosta ohjautuu ulos palomuurin (tuotteen UDR). Palomuurin nähdä tämän kuin julkiseen työnkulku ja pudota lähtevän tietoliikenteen. Näin on kolme käyttölupiin edusta- ja Taustajärjestelmä; aliverkosta suojaaminen 1) Avaa päätepisteitä FrontEnd001 ja BackEnd001 cloud services-2) NSGs estäminen liikenne Internetistä, 3), palomuurin poistetaan julkiseen liikenne.

Yksi kiinnostavasta kohdasta koskeva verkko-käyttöoikeusryhmän tässä esimerkissä on, että se sisältää vain yhden säännön alla, joka on estää internet-liikenne koko virtual verkkoon, joka näkyy tällöin suojauksen aliverkon. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Kuitenkin NSG on vain sidottu edusta- ja Taustajärjestelmä-aliverkosta, koska sääntö ei ole käsitellään liikenteen saapuva suojauksen aliverkon. Tuloksena, vaikka NSG säännön lukee ei ole Internet-liikenne paikalliseen kaikki VNet, valitse, koska NSG koskaan sidotun suojauksen aliverkon, liikenne Toiminnonkulku suojaus-aliverkon.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Palomuurisäännöt
Palomuurin lähettäminen edelleen sääntöjen on luotava. Koska palomuuri estää tai kaikki saapuva, lähtevä edelleenlähetys ja sisäisessä VNet liikenne tarvitaan monta palomuurisäännöt. Myös kaikki saapuva liikenne osumien suojaus-palvelun julkiseen IP-osoite (eri portit), palomuurin käsittelemien. Paras käytäntö on looginen kulkee ennen kuin määrität aliverkosta kaavio ja palomuurisäännöt välttämiseksi Uudista myöhemmin. Seuraavassa kuvassa on looginen näkymän tässä esimerkissä palomuurisäännöt:
 
![Palomuurisäännöt loogisten näkymä][2]

>[AZURE.NOTE] Käyttää verkon Virtual laitteen mukaan hallinta-portit vaihtelevat. Tässä esimerkissä Barracuda NextGen palomuuri viitataan käyttäen portit 22 801 ja 807. Katso laitteen toimittajan käyttöohjeista löydät tarkan portit, joita käytetään hallinta käytössä.

### <a name="logical-rule-description"></a>Loogisen säännön kuvaus
Loogisen yllä olevassa kaaviossa suojaus-aliverkon ei ole näkyvissä, koska palomuuri on vain kyseisen aliverkon resurssin ja tässä kaaviossa näkyy palomuurisäännöt ja kuinka ne loogisesti Salli tai estä liikenne kulkee ja ei todellinen reititetty polkua. Myös ulkoisten porttien valittu RDP-liikenne ylittävät vaihtelivat portit (8014 – 8026) ja valittiin hieman tasassa paikallinen IP-osoite on helpompi lukea kaksi oktettia (esimerkiksi paikalliset palvelimen osoite 10.0.1.4 on liitetty ulkoisen portin 8014), minkä tahansa suurempi-ristiriitaiset portit voi kuitenkin käyttää.

Tässä esimerkissä 7 sääntötyypeistä on annettava, nämä sääntötyypeistä on kuvattu jompikumpi seuraavista:

- Ulkoiset säännöt (saapuvan liikenteen):
  1.    Palomuurin hallinta sääntö: App uudelleenohjaus tämän säännön avulla liikenteen hallinta porttien verkon virtual laitteen.
  2.    RDP säännöt (kunkin windows server): (yksi kullekin palvelimelle) neljä sääntöjen ansiosta hallinnointi kautta RDP yksittäisiin-palvelimiin. Tämä voi myös yhteen yhdeksi toisen säännön mukaan käytössä verkon Virtual laitteen ominaisuuksia.
  3.    Sovelluksen liikenteen säännöt: On kaksi sovelluksen liikenteen säännöt-edusta-WWW-tietoliikenteen ensimmäisen ja toisen uudelleen tietoliikenteen (esimerkiksi web-palvelin tietojen taso). Sääntöjen määrittäminen tulee määräytyvät (palvelinten sijoitetut) verkoston arkkitehtuuri ja liikennettä kulkee (suunnaksi, jota käytetään liikenne kulkee ja joka portit).
      - Ensimmäistä sääntöä sallii todellinen sovelluksen liikenne sovelluspalvelin saavuttamiseksi. Muita sääntöjä että suojaus, hallinta, jne., sovelluksen säännöt ovat mitä Salli ulkoisten käyttäjien tai palveluja, jotka käyttävät sovellukset. Tässä esimerkissä on yksi verkkopalvelimen porttiin 80, näin yhden palomuurin sovelluksen säännön ohjaa saapuvan liikenteen ulkoisessa IP web palvelinten sisäinen IP-osoitteeseen. Uudelleenohjatun liikenne istunnon voidaan NAT oli sisäiselle palvelimelle.
      - Toisen sovelluksen liikenne säännön on uudelleen-sääntö, joka sallii verkkopalvelin voit jutella AppVM01 server (mutta ei AppVM02) minkä tahansa portin kautta.
- Sisäisissä (sisäisessä VNet liikennettä varten)
  4.    Lähtevän säännön Internet: tämän säännön ansiosta siirtää valitun verkkojen verkon tietoliikenteen. Tämä sääntö on yleensä oletusarvon säännön jo käyttöön palomuuri, mutta ei käytössä-tilassa. Tässä esimerkissä tulisi olla käytössä tämän säännön.
  5.    DNS-sääntö: Tämän säännön avulla vain DNS (portti 53) liikenne siirtää DNS-palvelin. Tämän säännön avulla nimenomaisesti tässä ympäristössä, Frontend useimmat tietoliikenteen taustaan haluat on estetty, DNS minkä tahansa paikallisen aliverkon.
  6.    Aliverkon aliverkon säännön: Tämä sääntö ei salli mihin tahansa palvelimeen uudelleen aliverkon muodostaa yhteyden palvelimelle edusta-aliverkon (mutta et päinvastoin).
- Käynnistystila (liikenteen sääntö, joka ei vastaa mitään edellä mainituista):
  7.    Estä kaikki liikenne säännön: Tämä on aina oltava lopullinen säännön (kannalta prioriteetti) ja sellaisenaan Jos liikenne jatkuu epäonnistuu vastaamaan edellisen sääntöjen poistetaan tämän säännön perusteella. Tämä on oletusarvo säännön ja yleensä aktivoitu, muutoksia ei yleensä tarvitaan.

>[AZURE.TIP] Toinen liikenne säännön tahansa porttiin sallitaan helposti tässä esimerkissä reaali tilanne tarkkaa portin ja niitä käytetään osoitealueet pienentää tämän säännön.

<br />

>[AZURE.IMPORTANT] Kun kaikki edellä mainitut säännöt luodaan, on tärkeää tarkistettava prioriteettia niiden sääntöjen, jotta liikenne sallittu tai estetty haluamallasi tavalla. Tässä esimerkissä säännöt ovat ensisijaisuusjärjestys. On helppo lukittu ulos palomuurin vuoksi väärin järjestetyn säännöt. Vähintään varmistaa palomuurin itse johdon on aina suora suurimman prioriteetin säännön.

### <a name="rule-prerequisites"></a>Säännön edellytykset
Yksi edellyttää käynnissä palomuurin virtuaalikoneen ovat julkisia päätepisteet. Palomuurin käsittelemään liikenne tarvittavat julkisen päätepisteet on oltava avoinna. On kolmenlaisia liikenteen tässä esimerkissä; 1) hallinta-liikenne paikalliseen ohjausobjektin palomuurin ja palomuurisäännöt, 2) RDP-liikenne paikalliseen windows-palvelimien ja 3)-sovelluksen liikenteen hallinta. Nämä ovat kolme saraketta liikenne valitsemisen yläosassa on puolet loogisten näkymä palomuurin säännöt.

>[AZURE.IMPORTANT] Avaimen takeway tähän kannattaa muistaa, että **kaikki** liikenne toimitetaan palomuurin läpi. Niin etätyöpöytään IIS01 palvelimeen, vaikka se ei eteen Lopeta pilvipalvelussa ja edusta-aliverkon käyttää tätä palvelinta olemme portin 8014 palomuuri RDP täytyy ja Salli palomuurin reitittämään RDP-pyyntö sisäisesti IIS01 RDP-porttiin. Azure-portaalin "Yhdistä-painike ei toimi, koska ei ole suoraa RDP polku IIS01 (aktiivisuutta portaalin näkee). Tämä tarkoittaa kaikki yhteydet Internetistä ovat suojaus-palveluun ja portin, kuten secscv001.cloudapp.net:xxxx (viittaus yllä kaavio ulkoinen portti- ja sisäinen IP-ja portin yhdistämistä varten).

Päätepisteen voidaan avata joko AM luomisen yhteydessä tai Lähetä muodosta, esimerkiksi komentosarja valmis ja tämän koodikatkelman alla (Huomautus; minkä tahansa kohteen alkaa sanalla dollarimerkki (esimerkiksi: $VMName[$i]) on tämän artikkelin oppaan komentosarjan käyttäjän määrittämät muuttuja. "$I" hakasulkeissa, [$i] edustaa tietyn AM VMs matriisin matriisi):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Vaikka näkyvissä ei selvästi muuttujat, mutta päätepisteet käytön vuoksi seuraavien **vain** avata suojauksen pilvipalvelussa. Näin varmistetaan, että kaikki saapuva liikenne käsitellään (reitittää, NAT oli pudotettu) palomuurin mukaan.

Hallinta asiakkaalle on asennettu tietokoneeseen, voit hallita palomuurin ja luo tarvittavat määritykset. Katso palomuurin (tai muita NVA) toimittajan hallita laitetta. Tässä osassa ja seuraavan osan, palomuurin säännöt luominen loput kuvataan määritys palomuuri itse toimittajien hallinta-asiakasohjelman (eli ei Azure portal tai PowerShell) kautta.

Täältä löytyvät ohjeet asiakasohjelman lataaminen ja yhteyden muodostaminen tässä esimerkissä käytetään Barracuda: [Barracuda maa-järjestelmänvalvoja](https://techlib.barracuda.com/NG61/NGAdmin)

Kun lokiin palomuurin päälle, mutta ennen kuin luot palomuurisäännöt, on kaksi valmistelevat objektiluokat, jotka voit tehdä sääntöjen luominen helpottaa; Verkon ja palvelun objektit.

Tässä esimerkissä kolme nimetty verkon objektien tulisi olla määritetyn (yksi edusta-osoitteiden ja Taustajärjestelmä aliverkon, myös DNS-palvelimen IP-osoite on verkon objekti). Luo nimetty verkon; lähtien Barracuda maa järjestelmänvalvojan asiakkaan Raporttinäkymät-ikkunan Siirry määritys-välilehti, napsauta toiminta-asetukset-osassa sääntöjoukko, sitten "Verkkojen" palomuurin objektit-valikossa, valitse sitten uusi verkkojen Muokkaa-valikossa. Verkko-objekti nyt voidaan luoda, lisäämällä nimi ja etuliite:

![Luo edusta-verkko-objekti][3]
 
Tämä vaihtoehto Luo nimetty verkon edusta-aliverkon, samalla objektin luodaanko myös aliverkon Taustajärjestelmä. Nyt aliverkosta voidaan on helpompi viitata palomuurisäännöt nimen mukaan.

DNS-palvelin objektin:

![Luo DNS Server-objekti][4]

DNS-säännön edempänä asiakirjassa käytetään viitettä yksi IP-osoite.

Toinen valmistelevat objektit ovat Services objekteja. Nämä tietueet tarkoittavat RDP yhteyden portit kullekin palvelimelle. Koska RDP oletusportti on sidottu RDP-palvelun objektiin, 3389, uusia palveluita voi luoda sallimaan tietoliikenteen ulkoisten porttien (8014 8026). Uusi portit myös lisätään aiemmin RDP-palveluun, mutta helpottaa esittely, voidaan luoda yksittäisen säännön kullekin palvelimelle. Luo uusi RDP sääntö palvelimeen. lähtien Barracuda maa järjestelmänvalvojan asiakkaan Raporttinäkymät-ikkunan Siirry määritys-välilehti, valitse toiminta-asetukset-osassa sääntöjoukko, sitten Valitse "Palvelut" palomuurin objektit-valikossa, palveluluetteloon alaspäin ja valitse "RDP-palvelu. Napsauta hiiren kakkospainikkeella ja valitse Kopioi, hiiren kakkospainikkeella ja valitse Liitä. On nyt RDP Copy1 Service-objekti, jota voi muokata. RDP-Copy1 hiiren kakkospainikkeella ja valitse Muokkaa, Muokkaa-palvelun objekti ikkunan ponnahtaa jopa tässä esitetyllä tavalla:

![Oletusarvoinen RDP säännön kopio][5]

Arvoja voi muokata edustavan RDP-palvelun tiettyyn palvelimeen. Saat AppVM01 yllä oletusarvon RDP-sääntö muutettava vastaamaan uusia palvelunimi, kuvaus ja ulkoiset RDP portin kuva 8-kaaviossa (Huomautus: portit muutetaan käytössä tämän tietyn palvelimen ulkoista porttia 3389 RDP-oletusarvon, AppVM01 ulkoista porttia on 8025) muokattu palvelu on alla :

![AppVM01 sääntö][6]
 
Tämä toimenpide on toistettava luominen jäljellä olevat palvelinten; RDP palvelut AppVM02, DNS01 ja IIS01. Näiden palvelujen luominen tekee säännön luominen yksinkertaisempi ja lisää ilmeisimmät seuraavan osion.

>[AZURE.NOTE] Palomuurin RDP palvelu ei tarvita kahdesta syystä 1) ensimmäisen palomuurin AM on mukaan Linux-kuva, joten SSH käytetään porttiin 22 RDP ja 2) portin 22 sijaan AM hallintaan ja kaksi hallinta-portit sallitaan ensimmäistä sääntöä hallinta-sallimaan hallinta connectivity jäljempänä.

### <a name="firewall-rules-creation"></a>Palomuurin säännöt luominen
On kolmenlaisia palomuurisäännöt tässä esimerkissä käytetään, ne kaikki on eri kuvakkeita:

Sovelluksen uudelleenohjata sääntö: ![Sovelluksen uudelleenohjata-kuvake][7]

Kohde NAT-säännön: ![Kohde NAT-kuvake][8]

Vaiheen sääntö: ![Välittää kuvake][9]

Saat lisätietoja sääntöjen löytyy Barracuda-sivustossa.

Luo seuraavat säännöt (tai Tarkista aiemmin luotuja oletusarvon sääntöjä) lähtien Barracuda maa järjestelmänvalvojan asiakkaan raporttinäkymät-ikkuna, siirry määritys-välilehti, napsauta toiminta-asetukset-osassa sääntöjoukko. Kutsutaan ruudukossa "Main säännöt" Näytä aiemmin aktiivinen ja aktivointi säännöt, tämä palomuuri. Tämä ruudukko oikeassa yläkulmassa on pieni, vihreä "+"-painiketta, valitse tämä, jos haluat luoda uuden säännön (Huomautus: palomuurin voi olla "lukittu" muutokset, jos merkintä "Lukita"-painike on näkyvissä ja et voi luoda tai muokata sääntöjä, napsauttamalla tätä painiketta "lukituksen" sääntöjoukon ja muokkausta varten). Jos haluat muokata aiemmin luotua sääntöä, valitse sääntöön, napsauta hiiren kakkospainikkeella ja valitse Muokkaa sääntöä.

Kun säännöt luodaan ja/tai muokata, ne on miten palomuurin ja sitten aktivoitu, jos näin ei tehdä muutoksia säännön tulee voimaan. Push- ja aktivoinnin prosessi on jäljempänä tiedot sääntöjen kuvaukset.

Niiden sääntöjen tässä esimerkissä suorittamiseen tarvittava yksityiskohtia on kuvattu seuraavasti:

- **Palomuurin hallinnan säännön**: tämän sovelluksen uudelleenohjata säännön avulla liikenteen hallinta porttien verkon virtual laitteen, tässä esimerkissä Barracuda NextGen palomuuri. Hallinnan portit ovat 801, 807 ja halutessasi 22. Sisäisiä ja ulkoisia portit ovat samat (eli ei portin käännös). Tämä sääntö, asennus-Hallintoraportointi-käyttäminen, on oletusarvoinen sääntö ja oletusarvoisesti käytössä (Barracuda NextGen palomuurin versio 6.1).

    ![Palomuurin hallinnan sääntö][10]

>[AZURE.TIP] Lähde-osoitetilaa tämän säännön on mikä tahansa, jos tiedossa on hallinta IP-osoitealueet, vähentää tämän alueen myös pienentää hallinta-porteille.

- **RDP sääntöjä**: nämä kohde NAT säännöt ansiosta hallinnointi kautta RDP yksittäisiin-palvelimiin.
Neljä kriittinen-kentät, jotka tarvitaan Luo tämä sääntö on:
  1.    Lähde – Salli RDP missä tahansa viittaus "Kaikki" käytetään lähde-kentässä.
  2.    Palvelu – käyttää kyseistä luotu aiemmin tässä tapauksessa "AppVM01 RDP-palvelun objektia, ulkoisten porttien uudelleenohjata palvelinten paikallinen IP-osoite ja portin 3386 (RDP oletusportti). Tietyn säännöllä on AppVM01 RDP-yhteyden.
  3.    Kohteen – pitäisi olla *Paikallinen palomuuri-porttiin*, "Verkkopalvelin 1 paikallinen IP- tai eth0, jos staattinen IP-osoitteet. Järjestysluku (eth0, eth1 jne.) saattavat olla erilaiset, jos verkko-laitteen on useita paikalliset liittymät. Tämä on palomuurin lähettää-portti (voi olla sama kuin vastaanottamisessa portti)-todellinen reititetty kohde on kohdeluettelo-kenttään.
  4.    Uudelleenohjaus – tässä osassa kerrotaan virtual laitteen where uudelleenohjaaminen kädessä tämän liikenne. Yksinkertaisin uudelleenohjaus on Lisää IP-osoite ja portti (valinnainen) kohdeluettelo-kenttään. Jos käytössä on porttia ei ole saapuvien pyynnön Kohdeportti on käyttää (ie käännöstä ei ole), jos portti on määritetty portti voi myös NAT sekä IP osoite.

    ![Palomuurin RDP sääntö][11]

    Neljä RDP säännöt korkeintaan on luotava: 

  	|   Säännön nimi    | Palvelin  |   Palvelun   |  Kohdeluettelo  |
  	|----------------|---------|-------------|---------------|
  	| RDP IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] Lähde- ja kentät-aluetta alaspäin tarkentaa pienentää. Useimmat rajoitettu laajuus, joka sallii toimintoja tulisi käyttää.

- **Sovelluksen liikenteen säännöt**: on kaksi sovelluksen liikenteen säännöt-edusta-WWW-tietoliikenteen ensimmäisen ja toisen uudelleen tietoliikenteen (esimerkiksi web-palvelin tietojen taso). Sääntöjen tulee määräytyvät (palvelinten sijoitetut) verkoston arkkitehtuuri ja liikennettä kulkee (suunnaksi, jota käytetään liikenne kulkee ja joka portit).

    Web-liikenteen edusta-sääntö ensin aiheena on:

    ![Palomuurin Web sääntö][12]
 
    Kohde NAT tämän säännön avulla todellinen sovelluksen liikenne sovelluspalvelin saavuttamiseksi. Muita sääntöjä että suojaus, hallinta, jne., sovelluksen säännöt ovat mitä Salli ulkoisten käyttäjien tai palveluja, jotka käyttävät sovellukset. Tässä esimerkissä on yksi verkkopalvelimen porttiin 80, näin yhden palomuurin sovelluksen säännön ohjaa saapuvan liikenteen ulkoisessa IP web palvelinten sisäinen IP-osoitteeseen.

    **Huomautus**: porttia ei ole määritetty kohdeluettelo-kentässä, näin saapuvaa porttia 80 (tai palvelun valittuna 443) käytetään verkkopalvelin uudelleenohjaus. Jos verkkopalvelin seuraa eri porttiin, kuten portti 8080, kohdeluettelo-kenttä on voitu päivittää 10.0.1.4:8080 sekä portin uudelleenohjausta varten.

    Seuraavan sovelluksen liikenne sääntö on uudelleen-sääntö, joka sallii verkkopalvelin keskustelemaan AppVM01 server (mutta ei AppVM02) minkä tahansa palvelun kautta:

    ![Palomuurin AppVM01 sääntö][13]

    Tämän vaiheen säännön avulla minkä tahansa IIS-palvelin saavuttaa AppVM01 edusta-aliverkon (IP-osoite 10.0.2.5) mihin tahansa porttiin minkä tahansa protokollan tietojen käyttämiseen tarvitaan web-sovelluksen avulla.

    Tässä ikkunassa koodin-"\<eksplisiittinen dest\>" käytetään kohdekentän merkiksi 10.0.2.5 kohteeksi. Tämä voi johtua joko eksplisiittinen esitetyllä tai nimeltä verkon objektin (kun on tehty edellytyksistä DNS-palvelin). Tämä on palomuuri, joihin menetelmää voi käyttää järjestelmänvalvojan ylöspäin. Voit lisätä 10.0.2.5 Explict Desitnation, kaksoisnapsauta ensimmäistä tyhjää riviä, \<eksplisiittinen dest\> ja kirjoita osoite näkyviin tulevassa ikkunassa.

    Välittää tämän säännön kanssa ei ole NAT ei tarvita, sillä tämä on sisäisen tietoliikenteen, joten yhteystapa voidaan määrittää "Ei SNAT".

    **Huomautus**: tämän säännön lähde-verkko on minkä tahansa resurssin edusta-aliverkon, jos vain yksi tai verkko-palvelimiin tunnetut tietystä numerosta verkko-objekti resurssin onnistunut tarkemmin näistä tarkka IP-osoitteiden sen sijaan, että koko edusta-aliverkon.

>[AZURE.TIP] Tämä sääntö käyttää palvelun "Kaikki" sovelluksen malli on helpottaa määrittämistä ja käyttöä, tämä sallii ICMPv4 (kutsu) yksittäisen säännön. Tämä ei kuitenkaan suositella. Portit ja protokollat ("palvelut") olisi narrowed mahdollisimman pieneksi, joka sallii sovelluksen toiminto pienentää tämän reunan yli.

<br />

>[AZURE.TIP] Vaikka tämä sääntö on käytössä eksplisiittinen kohde-viittaus, johdonmukaisen lähestymistavan käytetään koko palomuurin määrityksen. On suositeltavaa, että nimettyjä verkko-objekti voi käyttää kaikkialla helpompaa luettavuutta ja supportability. Eksplisiittinen dest käytetään tässä vain, jos haluat näyttää vaihtoehtoiset viittaus-menetelmä ja ei yleensä suositella (erityisesti niiden monimutkaisia määrityksiä).

- **Internet-sääntöön lähtevä**: Tämä välittää säännön ansiosta liikenne siirtää valitun kohteen verkostojen Source minkä tahansa verkosta. Tämä sääntö on oletusarvoinen sääntö yleensä jo Barracuda NextGen palomuurin, mutta se on ei käytössä-tilassa. Tämä sääntö käyttöön napsauttamalla käyttää Aktivoi sääntö-komentoa. Voit lisätä tämän artikkelin valmistelevat osio viittausten kahden paikallisen aliverkon, jotka on luotu tämän säännön lähde-määritettä on muokattu kuvassa sääntö.

    ![Palomuurin lähtevällä säännöllä][14]

- **DNS-säännön**: Tämä välittää säännön avulla vain DNS (portti 53) liikenne siirtää DNS-palvelimeen. Tämä FrontEnd useimmat tietoliikenteen taustaan haluat estää ympäristössä tämän säännön avulla erityisesti DNS.

    ![Palomuurin DNS sääntö][15]

    **Huomautus**: tämän näytön esiin yhteystapa sisältyy. Koska tämä sääntö on sisäinen IP-osoite sisäinen IP-osoite liikennettä, ei ole NATing vaaditaan, tämä yhteystapa arvoksi "Ei SNAT" Pass tämän säännön.

- **Aliverkon aliverkon säännön**: Tämä välittää sääntö on oletusarvoinen säännön, joka on aktivoitu ja muutettava siten, että kaikki palvelimeen uudelleen aliverkon edusta-aliverkon kaikki palvelimeen. Tämä sääntö on kaikki sisäisen tietoliikenteen, joten yhteystapa voidaan määrittää ei SNAT.

    ![Palomuurin sisäisessä VNet sääntö][16]

    **Huomautus**: kaksisuuntaisen-valintaruutu ei ole kuitattu (eivätkä ne ole useimmat säännöt kuitannut), tämä on merkittäviä säännölle siten, että se on tämä sääntö "yhden suunta", yhteyden voidaan aloittaa uudelleen aliverkon edusta-verkkoon, mutta et päinvastoin. Jos kyseinen valintaruutu on valittuna, tämän säännön ottaa käyttöön kaksisuuntaisen tietoliikenne, joka on looginen Microsoftin kaaviosta ei.

- **Estä kaikki liikenne säännön**: Tämä on aina oltava lopullinen säännön (kannalta prioriteetti) ja sellaisenaan Jos liikenne jatkuu ei vastaa mitään edeltävän säännöt poistetaan tämän säännön perusteella. Tämä on oletusarvo säännön ja yleensä aktivoitu, muutoksia ei yleensä tarvitaan. 

    ![Palomuuri estä sääntö][17]

>[AZURE.IMPORTANT] Kun kaikki edellä mainitut säännöt luodaan, on tärkeää tarkistettava prioriteettia niiden sääntöjen, jotta liikenne sallittu tai estetty haluamallasi tavalla. Tässä esimerkissä säännöt ovat siinä järjestyksessä, ne näkyvät pääikkunassa ruudukon välittäminen Barracuda Management-asiakas-säännöt.

## <a name="rule-activation"></a>Säännön aktivointi
Sääntöjoukko, muokattu määrityksen logiikan kaavion, jossa sääntöjoukko on ladattu palomuurin ja sitten aktivoitu.

![Palomuurin säännön aktivointi][18]
 
Ovat yläkulmassa hallinta-asiakasohjelman klusterin painikkeista. Muokattu säännöt lähettäminen palomuurin "Lähetä muutoksia"-painiketta ja valitse sitten "Aktivoi"-painiketta.
 
Palomuurin sääntöjoukko aktivointi tässä esimerkissä ympäristön muodosta on valmis.

## <a name="traffic-scenarios"></a>Liikenne skenaariot
>[AZURE.IMPORTANT] Avaimen takeway kannattaa muistaa, että **kaikki** liikenne toimitetaan palomuurin läpi. Niin etätyöpöytään IIS01 palvelimeen, vaikka se ei eteen Lopeta pilvipalvelussa ja edusta-aliverkon käyttää tätä palvelinta olemme portin 8014 palomuuri RDP täytyy ja Salli palomuurin reitittämään RDP-pyyntö sisäisesti IIS01 RDP-porttiin. Azure-portaalin "Yhdistä-painike ei toimi, koska ei ole suoraa RDP polku IIS01 (aktiivisuutta portaalin näkee). Tämä tarkoittaa suojaus-palveluun ja portin, kuten secscv001.cloudapp.net:xxxx on kaikki yhteydet Internetistä.

Näissä tilanteissa seuraavat palomuurisäännöt pitää olla sijainti:

1.  Palomuurin hallinta
2.  RDP IIS01
3.  RDP DNS01
4.  RDP AppVM01
5.  RDP AppVM02
6.  Internet-liikenteen App
7.  Sovelluksen-liikenne paikalliseen AppVM01
8.  Lähtevän Internetiin
9.  Frontend DNS01
10. Sisäisessä aliverkon liikenteen (vain edusta takaisin end)
11. Estä kaikki

Todellinen palomuurin sääntöjoukko todennäköisesti on monia muita lisäksi näitä sääntöjä, minkä tahansa määritetyn palomuurin säännöt myös on eri prioriteetti kuin tässä luettelossa olevia numeroita. Tämän luettelon ja liittyvän numerot ovat antamaan asiayhteyden vain kuuluu säännöt ja suhteellinen tärkeys ne toiseen välillä. Toisin sanoen; Todellinen palomuuri-"RDP, IIS01" saattaa säännön luku 5, mutta kunhan se on alle "Palomuurin hallinta" sääntö ja "RDP-DNS01"-sääntöä edellä tasata itse luetteloa. Luettelossa myös tuki skenaariot niitä, joten alla kuten "FW säännön 9 (DNS)". Myös niitä, neljä RDP sääntöjä muunnokset kutsutaan, "RDP sääntöjä" ollessa tietoliikenteen skenaarion itsenäisten RDP.

Peruuttaa myös verkon SharePoint-ryhmät ovat paikallaan tapahtuvan saapuvien edusta- ja Taustajärjestelmä aliverkosta internet-liikenne.

#### <a name="allowed-internet-to-web-server"></a>(Sallittu) Internet-yhteyttä verkkopalvelin
1.  Internet käyttäjän pyynnöt HTTP sivun SecSvc001.CloudApp.Net (Internet vastakkaisten pilvipalvelussa)
2.  Cloud palvelun siirtoja liikenne 10.0.0.4:80 porttiin 80 palomuurin liittymään Avaa päätepiste
3.  NSG ei ole määritetty suojaus aliverkon, niin järjestelmän NSG säännöt salli palomuuri-liikenne
4.  Liikenne käynnit sisäinen IP-osoite palomuuri (10.0.1.4)
5.  Palomuurin alkaa säännön käsittelyn:
  1.    FW säännön 1 (FW hallintoraportointi) ei lisää, siirry seuraavaan säännön
  2.    FW säännöt 2 – 5 (RDP säännöt) ei lisää, siirry seuraavaan säännön
  3.    FW säännön 6 (App: Web) käyttöön liikenne on sallittua palomuurin NAT sen 10.0.1.4 (IIS01)
6.  Edusta-aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei käytä (tämän liikenne on NAT olisi palomuuri, näin lähde-osoite on nyt palomuuri, joka on suojauksen aliverkon ja tarkastella edusta-aliverkon NSG on "paikallisen" liikenteen ja jota ei sallita), siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
7.  IIS01 web tietoliikenteen listening, saa pyynnön ja aloittaa käsittelyn pyyntö
8.  IIS01 yrittää käynnistää FTP-istuntoa AppVM01 Taustajärjestelmä aliverkon
9.  UDR reitin Frontend aliverkon tekee palomuurin seuraavan pisteen
10. Ei edusta aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
11. Palomuurin alkaa säännön käsittelyn:
  1.    FW säännön 1 (FW hallintoraportointi) ei lisää, siirry seuraavaan säännön
  2.    FW säännön 2 – 5 (RDP säännöt) ei lisää, siirry seuraavaan sääntö
  3.    FW säännön 6 (App: Web) ei voi käyttää, siirry seuraavaan säännön
  4.    FW säännön 7 (App: Taustajärjestelmä) käyttöön liikenne on sallittua, palomuurin välittää-liikenne paikalliseen 10.0.2.5 (AppVM01)
12. Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei lisää, siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
13.  AppVM01 saa pyynnön ja aloittaa istunnon ja vastaa
14. UDR reitin Taustajärjestelmä aliverkon tekee palomuurin seuraavan pisteen
15. Koska ei ole lähtevä NSG sääntöjä vastauksen sallitaan Taustajärjestelmä aliverkon
16. Koska tätä palauttaa liikenne muodostettu istunnon palomuurin välittää vastauksen verkkopalvelin (IIS01)
17. Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei lisää, siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
18. IIS-palvelin saa vastausta, viimeistelee AppVM01 tapahtuman ja täydentää rakentaminen HTTP-vastauksen, tämä HTTP-vastaus lähetetään pyytäjän
19. Koska ei ole lähtevä NSG sääntöjä vastauksen sallitaan Frontend aliverkon
20. HTTP-vastaus käynnit palomuurin ja aiemmin luotuja NAT-istunnon vastauksen, koska on hyväksynyt palomuurin
21. Palomuurin ohjaa sitten vastauksen Internet-käyttäjä
22. Koska ei ole lähtevän NSG säännöt tai UDR siirräntävälien vastauksen sallitaan Frontend aliverkon ja Internet-käyttäjä saa pyydetty web-sivua.

#### <a name="allowed-internet-rdp-to-backend"></a>(Sallittu) Internet RDP taustaan
1.  Palvelimen järjestelmänvalvoja Internetissä pyytää RDP-istunto AppVM01 SecSvc001.CloudApp.Net:8025, jossa 8025 on määritetty käyttäjä porttinumero "RDP-AppVM01" palomuurin säännön kautta
2.  Pilvipalvelussa sijaitsevaan välittää Avaa päätepisteen liikenne porttiin 8025 palomuuri-liittymän 10.0.0.4:8025
3.  NSG ei ole määritetty suojaus aliverkon, niin järjestelmän NSG säännöt salli palomuuri-liikenne
4.  Palomuurin alkaa säännön käsittelyn:
  1.    FW säännön 1 (FW hallintoraportointi) ei lisää, siirry seuraavaan säännön
  2.    FW säännön 2 (RDP IIS) ei lisää, siirry seuraavaan sääntö
  3.    FW säännön 3 (RDP DNS01) ei lisää, siirry seuraavaan säännön
  4.    FW säännön 4 (RDP AppVM01) käyttää, liikenne on sallittua palomuurin NAT sen 10.0.2.5:3386 (AppVM01 RDP-portti)
5.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei käytä (tämän liikenne on NAT olisi palomuuri, näin lähde-osoite on nyt palomuuri, joka on suojauksen aliverkon ja tarkastella on "paikallisen" tietoliikenteen taustaan aliverkon NSG ja jota ei sallita), siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
6.  AppVM01 RDP tietoliikenteen listening ja vastaa
7.  Oletusarvon säännöt otetaan käyttöön ja palaa liikenne on sallittua ei NSG lähtevän liikenteen säännöt
8.  UDR reitittää liikenteen palomuurin seuraavan pisteen nimellä
9.  Koska tätä palauttaa liikenne muodostettu istunnon palomuurin välittää vastauksen internet-käyttäjä
10. RDP-istunnon on otettu käyttöön
11. AppVM01 pyytää käyttäjänimi salasana

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Sallittu) DNS-palvelimen WWW Server DNS-haku
1.  WWW-palvelimeen, IIS01, tietosyötteestä osoitteessa www.data.gov tarpeisiin, mutta tarvitsee selvittää osoitetta.
2.  Verkkokokoonpanon VNet luetteloiden DNS01 (10.0.2.4 Taustajärjestelmä aliverkon) kuin ensisijainen DNS-palvelin, IIS01 lähettää DNS-pyynnön DNS01
3.  UDR reitittää liikenteen palomuurin seuraavan pisteen nimellä
4.  Lähtevän NSG sääntöjä ei on sidottu edusta-aliverkon-liikenne sallitaan
5.  Palomuurin alkaa säännön käsittelyn:
  1.    FW säännön 1 (FW hallintoraportointi) ei lisää, siirry seuraavaan säännön
  2.    FW säännön 2 – 5 (RDP säännöt) ei lisää, siirry seuraavaan sääntö
  3.    FW säännöt 6 ja 7 (sovelluksen säännöt) ei lisää, siirry seuraavaan sääntö
  4.    FW säännön 8 (Internet) ei lisää, siirry seuraavaan sääntö
  5.    FW säännön 9 (DNS) käyttää liikenne on sallittua, palomuurin välittää-liikenne paikalliseen 10.0.2.4 (DNS01)
6.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei lisää, siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
7.  DNS-palvelin vastaanottaa pyynnön
8.  DNS-palvelin ei ole välimuistiin osoite ja pyytää pääkansion DNS-palvelin Internetissä
9.  UDR reitittää liikenteen palomuurin seuraavan pisteen nimellä
10. Lähtevän NSG sääntöjä ei Taustajärjestelmä aliverkon-liikenne on sallittua.
11. Palomuurin alkaa säännön käsittelyn:
  1.    FW säännön 1 (FW hallintoraportointi) ei lisää, siirry seuraavaan säännön
  2.    FW säännön 2 – 5 (RDP säännöt) ei lisää, siirry seuraavaan sääntö
  3.    FW säännöt 6 ja 7 (sovelluksen säännöt) ei lisää, siirry seuraavaan sääntö
  4.     FW säännön 8 (Internet) käyttää, liikenne on sallittua, istunto on SNAT ulos pääkansion DNS-palvelimeen Internetissä
12. Internet-DNS-palvelin vastaa, koska tämä istunto on käynnistetty palomuurin, vastaus on hyväksynyt palomuurin
13. Tämä on muodostettu istunnon, palomuurin välittää vastauksen aloittava DNS01-palvelimeen
14. Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei lisää, siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
15. DNS-palvelin vastaanottaa ja välimuistiin vastauksen ja sitten vastaa alkuperäisen pyynnön takaisin IIS01
16. UDR reitin Taustajärjestelmä aliverkon tekee palomuurin seuraavan pisteen
17. Lähtevän NSG sääntöjä ei ole Taustajärjestelmä aliverkon, liikenne sallitaan
18. Tämä on muodostettu istunnon, palomuurin, vastaus toimitetaan palomuuri takaisin IIS-palvelin
19. Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    Ei ole NSG sääntö, joka koskee saapuvien tietoliikenteen taustaan aliverkosta edusta-aliverkon, jotta ei mitään NSG sääntöjen käyttäminen
  2.    Oletusarvoinen järjestelmän säännön salliminen liikenne välillä aliverkosta antaa tämän liikenne jotta liikenne sallitaan
20. IIS01 saa vastausta DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Sallittu) Palvelimeen edusta-palvelimeen
1.  Järjestelmänvalvojan AppVM02 RDP kautta kirjautuneet pyytää tiedoston suoraan IIS01-palvelimen kautta Windowsin Resurssienhallinnassa
2.  UDR reitin Taustajärjestelmä aliverkon tekee palomuurin seuraavan pisteen
3.  Koska ei ole lähtevä NSG sääntöjä vastauksen sallitaan Taustajärjestelmä aliverkon
4.  Palomuurin alkaa säännön käsittelyn:
  1.    FW säännön 1 (FW hallintoraportointi) ei lisää, siirry seuraavaan säännön
  2.    FW säännön 2 – 5 (RDP säännöt) ei lisää, siirry seuraavaan sääntö
  3.    FW säännöt 6 ja 7 (sovelluksen säännöt) ei lisää, siirry seuraavaan sääntö
  4.    FW säännön 8 (Internet) ei lisää, siirry seuraavaan sääntö
  5.    FW säännön 9 (DNS) ei lisää, siirry seuraavaan sääntö
  6.    FW säännön 10 (aliverkon sisäisessä) käyttää liikenne on sallittua, palomuurin välittää liikenne 10.0.1.4 (IIS01)
5.  Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei lisää, siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
6.  Jos oikea todennus- ja, IIS01 hyväksyy pyynnön ja vastaa
7.  UDR reitin Frontend aliverkon tekee palomuurin seuraavan pisteen
8.  Koska ei ole lähtevä NSG sääntöjä vastauksen sallitaan Frontend aliverkon
9.  On olemassa olevan istunnon palomuurin vastauksessa sallitaan ja palomuurin palauttaa AppVM02 vastaus
10. Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (Estä Internet) ei lisää, siirry seuraavaan säännön
  2.    Oletusarvoinen NSG sääntöjen avulla aliverkon aliverkon liikenteen-liikenne on sallittua, NSG säännön käsittelyn lopettaminen
11. AppVM02 saa vastausta

#### <a name="denied-internet-direct-to-web-server"></a>(Estetty) Internet-suoraan verkkopalvelin
1.  Internet-käyttäjä yrittää käyttää verkkopalvelin IIS01, FrontEnd001.CloudApp.Net-palvelun kautta
2.  Koska päätepisteitä ei ole avoinna HTTP tietoliikenteen, tämä läpäise pilvipalvelussa ja Eikö yhteys palvelimeen
3.  Jos päätepisteet oli avattu jostain syystä, edusta-aliverkon NSG (Estä Internet) estää tämän tietoliikenteen
4.  Lopuksi edusta aliverkon UDR reitin lähettää lähtevän liikenteen IIS01 kuin seuraavan pisteen palomuurin ja palomuurin tarkastella tämä liikenteen julkiseen ja pudota lähtevän vastauksen näin on vähintään kolme riippumaton kerrokset linnaa välillä internet- ja IIS01 sen pilvipalvelussa-ei valtuuksia/asiattomia käytön kautta.

#### <a name="denied-internet-to-backend-server"></a>(Estetty) Internet-yhteyttä palvelimeen
1.  Internet-käyttäjä yrittää käyttää AppVM01 tiedoston BackEnd001.CloudApp.Net-palvelun kautta
2.  Avaa jaettu tiedostoresurssi on päätepisteitä, tämä Välitä pilvipalvelussa ja Eikö yhteys palvelimeen
3.  Jos päätepisteet oli avattu jostain syystä, NSG (Estä Internet) estää tämän liikenne
4.  Lopuksi UDR reitin lähettää kaikki lähtevän tietoliikenteen AppVM01 kuin seuraavan pisteen palomuurin ja palomuurin tarkastella tämä liikenteen julkiseen ja pudota lähtevän vastauksen näin on vähintään kolme riippumaton kerrokset linnaa välillä internet- ja AppVM01 sen pilvipalvelussa-ei valtuuksia/asiattomia käytön kautta.

#### <a name="denied-frontend-server-to-backend-server"></a>(Estetty) Palvelimeen edusta-palvelimeen
1.  Oletetaan, että IIS01 on käsiin ja on käynnissä haitallisen koodin luettava Taustajärjestelmä aliverkon palvelimet.
2.  Frontend aliverkon UDR reitin lähettää lähtevän liikenteen IIS01 palomuurin kuin seuraavan kohteen. Tämä ei ole jotakin, mitä voit muuttaa vaarantuneen AM käyttämällä.
3.  Palomuurin käsitellä liikenne, jos pyyntö ei AppVM01 tai DNS-hakuja, jotka liikenne mahdollisesti sallittava palomuurissa (vuoksi FW säännöt 7 ja 9) DNS-palvelimeen. Estää kaikki muu tietoliikenne FW säännön 11 (Estä kaikki).
4.  Jos advanced uhkien tunnistus on käytössä palomuuri (joka ei koske tässä asiakirjassa, että tietyn verkon laitteen uhkien lisäominaisuuksia toimittaja on ohjeissa), vaikka liikenteestä, joka salliman tässä asiakirjassa käsitellyt basic edelleenlähetyssääntöjä voi estettävä Jos liikenne sisältyy tunnetut allekirjoitukset tai kuviot, merkitse Lisäasetukset uhkien säännön.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Estetty) Internet-DNS-haun DNS-palvelimeen
1.  Internet-käyttäjä yrittää haku sisäinen DNS-tietue-DNS01 BackEnd001.CloudApp.Net-palvelun kautta 
2.  Koska päätepisteitä ei ole avoinna tietoliikenteen DNS-tämä läpäise pilvipalvelussa ja Eikö yhteys palvelimeen
3.  Jos päätepisteet oli avattu jostain syystä, edusta-aliverkon NSG-säännön (Estä Internet) estää tämän liikenne
4.  Lopuksi Taustajärjestelmä aliverkon UDR reitin lähettää lähtevän liikenteen DNS01 kuin seuraavan pisteen palomuurin ja palomuurin tarkastella tämä liikenteen julkiseen ja pudota lähtevän vastauksen näin on vähintään kolme riippumaton kerrokset linnaa välillä internet- ja DNS01 sen pilvipalvelussa-ei valtuuksia/asiattomia käytön kautta.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Estetty) Internet-yhteyden SQL access palomuurin läpi
1.  Internet-käyttäjä pyytää SecSvc001.CloudApp.Net (Internet vastakkaisten pilvipalvelussa) SQL-tiedot
2.  Koska päätepisteitä ei ole avoinna SQL-tämä Välitä pilvipalvelussa ja Eikö saavuttaa palomuurin
3.  Jos SQL päätepisteet oli avattu jostain syystä, palomuurin aloittaa säännön käsittelyn:
  1.    FW säännön 1 (FW hallintoraportointi) ei lisää, siirry seuraavaan säännön
  2.    FW säännöt 2 – 5 (RDP säännöt) ei lisää, siirry seuraavaan säännön
  3.    FW säännön 6 ja 7 (sovelluksen säännöt) ei lisää, siirry seuraavaan sääntö
  4.    FW säännön 8 (Internet) ei lisää, siirry seuraavaan sääntö
  5.    FW säännön 9 (DNS) ei lisää, siirry seuraavaan sääntö
  6.    FW säännön 10 (aliverkon sisäisessä) ei käyttää, siirry seuraavaan säännön
  7.    FW säännön 11 (Estä kaikki) käyttää, liikenne on estetty, Lopeta säännön käsittely


## <a name="references"></a>Viittaukset
### <a name="main-script-and-network-config"></a>Tärkeimmät komentosarja ja verkon määritys
Tallenna koko komentosarja PowerShell-komentosarjatiedosto. Verkko-määritysten tallentaminen "NetworkConf2.xml"-tiedostoon.
Muokkaa käyttäjän määrittämät muuttujat tarpeen mukaan. Suorita komentosarja sitten noudattamalla palomuurin säännön asetukset edellä.

#### <a name="full-script"></a>Koko komentosarja
Tämä komentosarja on käyttäjän määrittämät muuttujat perusteella:

1.  Yhteyden muodostaminen Azure tilauksen
2.  Luo uusi tallennustilan-tili
3.  Luo uusi VNet ja kolme aliverkosta määritysten verkon määritystiedostossa
4.  Muodosta viisi näennäiskoneiden; 1 palomuurin ja 4 windows server VMs
5.  Määritä UDR mukaan lukien:
  1.    Kahden uuden reititys-taulukon luominen
  2.    Lisää tiet taulukoihin
  3.    Taulukoiden sitoa tarvittavat aliverkosta
6.  IP-edelleenlähetyksen käyttöön NVA
7.  Määritä NSG mukaan lukien:
  1.    NSG luominen
  2.    Säännön lisääminen
  3.    Sidonta NSG tarvittavat aliverkosta

PowerShell-komentosarja on suoritettava paikallisesti edelleen internet-yhteys PC- tai palvelimen.

>[AZURE.IMPORTANT] Kun tämä komentosarja on suoritettu, voi olla varoituksia tai muita ilmoituksia, joihin pop-PowerShell. Vain virhesanomat punaisella ovat syy huolenaiheita.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
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
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
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
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
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
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
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
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
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
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
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
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Kaksisuuntaisen DMZ NVA, NSG ja UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "Palomuurisäännöt loogisten näkymä"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Luo edusta-verkko-objekti"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "Luo DNS Server-objekti"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Oletusarvoinen RDP säännön kopio"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 sääntö"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Sovelluksen Redirect-kuvake"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Kohde NAT-kuvake"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "Välittää kuvake"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Palomuurin hallinnan sääntö"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Palomuurin RDP sääntö"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Palomuurin Web sääntö"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Palomuurin AppVM01 sääntö"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Palomuurin lähtevällä säännöllä"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Palomuurin DNS sääntö"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Palomuurin sisäisessä VNet sääntö"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Palomuuri estä sääntö"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Palomuurin säännön aktivointi"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
