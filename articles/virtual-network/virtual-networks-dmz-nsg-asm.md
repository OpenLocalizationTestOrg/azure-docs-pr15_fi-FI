<properties
   pageTitle="DMZ Esimerkki – luoda yksinkertaisen DMZ kanssa NSGs | Microsoft Azure"
   description="DMZ kanssa verkon käyttöoikeusryhmät (NSG) luominen"
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

# <a name="example-1--build-a-simple-dmz-with-nsgs"></a>Esimerkki 1 – muodosta yksinkertainen DMZ NSGs kanssa

[Palaa suojauksen reunan parhaat käytännöt sivulle][HOME]

Tässä esimerkissä luodaan yksinkertainen DMZ neljä windows-palvelimien ja verkon käyttöoikeusryhmät. Se käy läpi kunkin vaiheen tarkempaa tietoa antamaan asiaa komennot. On myös tietoliikenteen skenaarion osan antamaan perusteellisempaa vaiheittaiset ohjeet, miten liikenne etenee kerrokset DMZ linnaa kautta. -Viitteet osa on valmis koodi ja ohjepaketti luonnissa ja kokeilla eri tilanteista tässä ympäristössä. 

![Saapuvien DMZ NSG kanssa][1]

## <a name="environment-description"></a>Ympäristön kuvaus
Tässä esimerkissä on tilauksessa, joka sisältää seuraavat:

- Kaksi cloud services: "FrontEnd001" ja "BackEnd001"
- Virtuaalinen verkon "CorpNetwork" kahden aliverkon; kanssa "FrontEnd" ja "Taustajärjestelmä"
- Verkon käyttöoikeusryhmän, jota käytetään sekä aliverkosta
- Windows Server, joka edustaa sovelluksen verkkopalvelin ("IIS01")
- Kaksi ikkunaa palvelimissa, jotka edustavat sovelluksen takaisin lopettaa servers ("AppVM01", "AppVM02")
- Windows server, joka edustaa DNS server ("DNS01")

Viittaukset-kohdassa on PowerShell-komentosarja, joka luo useimmat edellä kuvatun ympäristön. VMs ja Virtual verkkojen vaikka Esimerkki komentosarjalla valmis on ei ole kuvattu tämän asiakirjan. 

Voit luoda ympäristöön;

  1.    Tallentaa verkon config xml-tiedoston, viittaukset-luokasta (päivitetty nimet, sijainti ja IP vastaamaan tapaukselle osoitteet)
  2.    Päivitä käyttäjän muuttujien komentosarjan vastaamaan ympäristön komentosarja on suoritettava vastaan (tilaukset, palveluiden nimet jne.)
  3.    Suorita PowerShell-komentosarja

**Huomautus**: PowerShell-komentosarjaa esittää alueen on vastattava verkon määritysten xml-tiedoston esittää alue.

Kun komentosarja suoritetaan onnistuneesti muita valinnaisia ohjeita voidaan toteuttaa, viittaukset-kohdassa on kaksi komentosarjojen verkkopalvelin ja app-palvelin sallimaan testaaminen oikeilla DMZ määritysten yksinkertaisen web-sovelluksen määrittäminen.

Seuraavissa osissa ongelman yksityiskohtainen kuvaus verkon käyttöoikeusryhmiä ja miten ne toimivat tässä esimerkissä kävelemällä PowerShell-komentosarjaa avaimen rivit kautta.

## <a name="network-security-groups-nsg"></a>Verkon käyttöoikeusryhmät (NSG)
Tässä esimerkissä NSG ryhmän sisäisten ja ladata sitten kuusi säännöillä. 

>[AZURE.TIP] Niinkään kannattaa luoda tietyn "Salli" sääntöjen ensin ja sitten yleisen "Estä" sääntöjä viimeksi. Säännöt on määritetty tärkeyden mukaan määräytyy lasketaan ensin. Kun liikenne löytyy tietyn säännön koskee, arvioidaan edelleen sääntöjä ei ole. NSG sääntöjä voit käyttää joko (aliverkon näkökulmasta) saapuvat tai Lähtevät suuntaan.

Määrittämisen avulla seuraavat säännöt rakennetaan saapuvan liikenteen:

1.  Sisäiseen DNS-liikenne (portti 53) on sallittua
2.  Mikä tahansa AM RDP liikenteen (portti 3389) Internetistä sallitaan
3.  HTTP liikenne (portti 80) Internetistä WWW-palvelimeen (IIS01) on sallittua
4.  Minkä tahansa liikenteen (kaikki portit)-IIS01 AppVM1 sallitaan
5.  Minkä tahansa liikenteen (kaikki portit) Internetistä kokonaan VNet (molemmat aliverkosta) on estetty
6.  Minkä tahansa liikenteen (kaikki portit) edusta-aliverkosta Taustajärjestelmä aliverkon on estetty

Sääntöjen ja sitoa kunkin aliverkon, jos HTTP-pyyntö on ollut saapuva Internetistä WWW-palvelimeen, sekä sääntöjä 3 (Salli) ja 5 (estä) sovelletaan, mutta koska sääntö 3 on suuri prioriteetti vain sen sovelletaan ja säännön 5 tule toista kyselyjä. Näin HTTP-pyyntö voivat verkkopalvelin. Jos sama liikenne yritti saavuttaa DNS01 palvelimeen, säännön 5 (estä) on ensimmäinen sovelletaan ja liikenne eivät voi siirtää palvelimeen. Säännön 6 (estä) estää edusta-aliverkon-puhelu Taustajärjestelmä aliverkon (lukuun ottamatta sallittu liikenteen säännöt 1 – 4), tämä suojaa Taustajärjestelmä verkon tapauksessa voi saada-kompromisseja, web-sovelluksen edusta-hän on rajoitetut käyttöoikeudet Taustajärjestelmä "suojattu" verkon (vain resursseja tarjoamia AppVM01 palvelimessa).

Ei oletusarvoisesti lähtevän säännön, joka sallii Internet-liikenteen ulos. Tässä esimerkissä on olet sallimisen liikenteen ja muokkaaminen ei ole mitään lähtevän liikenteen säännöt. Lukitse liikenne molempiin suuntiin, käyttäjän määrittämät reititys tarvitaan, tämä on tarkasteltavia "Esimerkki 3" alla.

Niiden sääntöjen kerrotaan tarkemmin seuraavasti (**Huomautus**: minkä tahansa alla alkaa sanalla dollarimerkki (esimerkiksi: $NSGName) on tämän artikkelin oppaan komentosarjan käyttäjän määrittämät muuttuja):

1. Ensin verkon käyttöoikeusryhmän rakennetaan pitoon sääntöjä:

        New-AzureNetworkSecurityGroup -Name $NSGName `
            -Location $DeploymentLocation `
            -Label "Security group for $VNetName subnets in $DeploymentLocation"
 
2.  Tässä esimerkissä ensimmäistä sääntöä sallii DNS-liikenteen kaikki sisäisen verkon DNS-palvelimeen Taustajärjestelmä aliverkon välillä. Sääntö on joitakin tärkeitä parametreja:
  - "Kirjoita" tarkoittaa mihin suuntaan liikenteen säännöllä tulevat voimaan; Tämä on näkökulmasta aliverkon tai virtuaalikoneen (sen mukaan, missä tämä NSG on sidottu). Näin Jos tyyppi on "Saapuvien" liikenne on siirtymässä aliverkon, sääntöä sovelletaan ja jätä aliverkon liikenne ei vaikuttaa tätä sääntöä.
  - "Prioriteetti" asettaa järjestyksessä, jossa liikenne vuo lasketaan. Mitä pienempi luku suuremman prioriteetti. Kun sääntö koskee tietyn liikenteen suunnan, ei ole muita säännöt käsitellään. Näin Jos prioriteetti 1 sääntö mahdollistaa tietoliikenteen, ja prioriteetti 2 sääntö voidaan estää liikenteestä, ja molemmissa koskevat liikenne sitten liikenne voivat flow (koska sääntö 1 ollut suuri prioriteetti kului tehoste ja muita sääntöjä ei on otettu käyttöön).
  - Jos tämä sääntö koskee liikenne estetyt tai sallitut tarkoittaa "Toiminto".

            Get-AzureNetworkSecurityGroup -Name $NSGName | `
                Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" `
                -Type Inbound -Priority 100 -Action Allow `
                -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
                -DestinationAddressPrefix $VMIP[4] `
                -DestinationPortRange '53' `
                -Protocol *

3.  Tämä sääntö sallii RDP-liikenne paikalliseen flow Internetistä missä tahansa VNET joko aliverkon RDP-porttiin. Tätä sääntöä käytetään kahta tietyn tyyppisten osoitteiden etuliitteiden; "VIRTUAL_NETWORK" ja "INTERNET". Tämä on sähköpostiosoite osoitteiden etuliitteiden suurempi funktioluokan helposti.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" `
            -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '3389' `
            -Protocol *

4.  Tämän säännön avulla saapuvan internet-liikenne paikalliseen osumien verkkopalvelin. Tämä ei muuta reititys toimintaa. se sallii vain IIS01 välittää tarkoitettu liikenne. Näin Jos Internet-liikenteen oli verkkopalvelin kohteena tämän säännön sallia sen ja Lopeta sääntöjen käsittely edelleen. (Osoitteessa prioriteetti säännön 140 kaikki muut saapuvan internet-liikenne on estetty). Jos olet vain käsittelyn HTTP-liikenne, tämä sääntö voi edelleen rajoitettu päästää vain kohde porttiin 80.

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable Internet to $VMName[0]" `
            -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] `
            -DestinationPortRange '*' `
            -Protocol *

5.  Tämä sääntö mahdollistaa tietoliikenteen IIS01 palvelimesta välittää AppVM01 palvelimen uudempi sääntö estää kaikki muut edusta tietoliikenteen taustaan. Voit parantaa tätä sääntöä, jos portti tunnetaan, jotka on lisättävä. Esimerkiksi jos IIS-palvelin on pallolla vain SQL Server-AppVM01-portin kohdealueen tulisi vaihtaa "*" (kaikki) ja näin salliminen pienempi saapuvien uhanalaisten-AppVM01 olisi verkkosovelluksen koskaan olla käsiin 1433 (SQL-portti).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule -Name "Enable $VMName[1] to $VMName[2]" `
            -Type Inbound -Priority 130 -Action Allow `
        -SourceAddressPrefix $VMIP[1] -SourcePortRange '*' `
        -DestinationAddressPrefix $VMIP[2] `
        -DestinationPortRange '*' `
        -Protocol *
 
6.  Tämä sääntö estää liikenne Internetistä palvelimia verkossa. Yhdessä osoitteessa prioriteetti 110 ja 120 säännön avulla vain tuleva internet-liikenne palomuurin ja RDP portit palvelimiin ja estää muiden monipuolisista. 

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $VNetName VNet from the Internet" `
            -Type Inbound -Priority 140 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK `
            -DestinationPortRange '*' `
            -Protocol *
 
7.  Lopullinen sääntö estää liikenne edusta-aliverkosta Taustajärjestelmä aliverkon. Tämä on saapuvan liikenteen vain sääntö, käänteisen liikenne on sallittua (edusta ja taustasta).

        Get-AzureNetworkSecurityGroup -Name $NSGName | `
            Set-AzureNetworkSecurityRule `
            -Name "Isolate the $FESubnet subnet from the $BESubnet subnet" `
            -Type Inbound -Priority 150 -Action Deny `
            -SourceAddressPrefix $FEPrefix -SourcePortRange '*' `
            -DestinationAddressPrefix $BEPrefix `
            -DestinationPortRange '*' `
            -Protocol * 

## <a name="traffic-scenarios"></a>Liikenne skenaariot
#### <a name="allowed-web-to-web-server"></a>(*Sallittu*) Web-Web-palvelin
1.  Internet käyttäjän pyynnöt HTTP sivun FrontEnd001.CloudApp.Net (Internet vastakkaisten pilvipalvelussa)
2.  Cloud palvelun siirtoja liikenne Avaa päätepisteen porttiin 80 IIS01 kohti (WWW-palvelin)
3.  Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 3 (Internet-IIS01) käyttää, liikenne on sallittu, lopetus säännön käsittely
4.  Liikenne käynnit sisäinen IP-osoite verkkopalvelin IIS01 (10.0.1.5)
5.  IIS01 web tietoliikenteen listening, saa pyynnön ja aloittaa käsittelyn pyyntö
6.  IIS01 pyydetään SQL Server-AppVM01 tietoja
7.  Ei edusta aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
8.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 3 (Internet-palomuurin) ei lisää, siirry seuraavaan säännön
  4.    NSG säännön 4 (IIS01 AppVM01) käyttää, liikenne on sallittua, säännön käsittelyn lopettaminen
9.  AppVM01 vastaanottaa SQL-kysely ja vastaa
10. Koska ei ole lähtevän liikenteen säännöt Taustajärjestelmä aliverkon vastauksen sallitaan
11. Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    Ei ole NSG sääntö, joka koskee saapuvien tietoliikenteen taustaan aliverkosta edusta-aliverkon, jotta ei mitään NSG sääntöjen käyttäminen
  2.    Oletusarvoinen järjestelmän säännön salliminen liikenne välillä aliverkosta Salli tämän liikenne liikenne sallitaan.
12. IIS-palvelin vastaanottaa SQL-vastauksen ja täydentää HTTP-vastauksen ja lähettää pyytäjän
13. Koska käsittelyalueella on ei lähtevän liikenteen säännöt edusta-aliverkon vastaus on sallittu ja Internet-käyttäjä saa pyydetty web-sivua.

#### <a name="allowed-rdp-to-backend"></a>(*Sallittu*) RDP taustaan
1.  Palvelimen järjestelmänvalvoja Internetissä pyytää RDP-istunto AppVM01, missä xxxxx RDP (varatun portin löytyy Azure-portaalissa tai PowerShell) AppVM01 satunnaisesti määritetty porttinumero BackEnd001.CloudApp.Net:xxxxx
2.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) käyttää, liikenne on sallittu, lopetus säännön käsittely
3.  Oletusarvon säännöt otetaan käyttöön ja palaa liikenne on sallittua ei lähtevän liikenteen säännöt
4.  RDP-istunnon on otettu käyttöön
5.  AppVM01 pyytää käyttäjänimi salasana

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(*Sallittu*) DNS-palvelimen WWW Server DNS-haku
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

#### <a name="allowed-web-server-access-file-on-appvm01"></a>(*Sallittu*) WWW-palvelimen access-tiedoston AppVM01
1.  IIS01 pyytää AppVM01-tiedosto
2.  Ei edusta aliverkon lähtevän liikenteen säännöt-liikenne on sallittua.
3.  Taustajärjestelmä aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 3 (Internet-IIS01) ei lisää, siirry seuraavaan säännön
  4.    NSG säännön 4 (IIS01 AppVM01) käyttää, liikenne on sallittua, säännön käsittelyn lopettaminen
4.  AppVM01 saa pyynnön ja vastaa-tiedoston (olettaen että valtuuksia)
5.  Koska ei ole lähtevän liikenteen säännöt Taustajärjestelmä aliverkon vastauksen sallitaan
6.  Frontend aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    Ei ole NSG sääntö, joka koskee saapuvien tietoliikenteen taustaan aliverkosta edusta-aliverkon, jotta ei mitään NSG sääntöjen käyttäminen
  2.    Oletusarvoinen järjestelmän säännön salliminen liikenne välillä aliverkosta Salli tämän liikenne liikenne sallitaan.
7.  IIS-palvelin vastaanottaa tiedoston

#### <a name="denied-web-to-backend-server"></a>(*Estetty*) Sivuston palvelimeen
1.  Internet-käyttäjä yrittää käyttää AppVM01 tiedoston BackEnd001.CloudApp.Net-palvelun kautta
2.  Avaa jaettu tiedostoresurssi on päätepisteitä, tämä Välitä pilvipalvelussa ja Eikö yhteys palvelimeen
3.  Jos päätepisteet oli avattu jostain syystä, NSG säännön 5 (Internet-VNet) estää tämän tietoliikenteen

#### <a name="denied-web-dns-lookup-on-dns-server"></a>(*Estetty*) Web DNS-haun DNS-palvelimeen
1.  Internet-käyttäjä yrittää haku sisäinen DNS-tietue-DNS01 BackEnd001.CloudApp.Net-palvelun kautta
2.  Koska päätepisteitä ei ole avoinna DNS-tämä Välitä pilvipalvelussa ja Eikö yhteys palvelimeen
3.  Jos päätepisteet oli avattu jostain syystä, NSG säännön 5 (Internet-VNet) estää tämän tietoliikenteen (Huomautus: kyseisen säännön 1 (DNS) eivät päde kahdesta syystä, ensin lähdeosoite on Internetissä, tämä sääntö koskee vain paikallisen VNet lähteenä, myös tämä on Sallimissääntö, joten se koskaan estä liikenne)

#### <a name="denied-web-to-sql-access-through-firewall"></a>(*Estetty*) Palomuurin läpi SQL Access Web
1.  Internet-käyttäjä pyytää FrontEnd001.CloudApp.Net (Internet vastakkaisten pilvipalvelussa) SQL-tiedot
2.  Koska päätepisteitä ei ole avoinna SQL-tämä Välitä pilvipalvelussa ja Eikö saavuttaa palomuurin
3.  Jos päätepisteet oli avattu jostain syystä, edusta-aliverkon alkaa saapuvan liikenteen säännön käsittelyn:
  1.    NSG säännön 1 (DNS) ei lisää, siirry seuraavaan sääntö
  2.    NSG säännön 2 (RDP) ei lisää, siirry seuraavaan sääntö
  3.    NSG säännön 3 (Internet-IIS01) käyttää, liikenne on sallittu, lopetus säännön käsittely
4.  Liikenne käynnit sisäinen IP-osoite IIS01 (10.0.1.5)
5.  IIS01 ei ole kuuntele porttia 1433, joten pyynnön vastaus

## <a name="conclusion"></a>Tekemistä
Tämä on melko yksinkertainen ja suoraan eteenpäin tapa käyttöjarrujärjestelmän uudelleen aliverkon saapuvan liikenteen.

Lisää esimerkkejä sekä verkon suojauksen rajat yleiskatsaus löytyy [tähän][HOME].

## <a name="references"></a>Viittaukset
### <a name="main-script-and-network-config"></a>Tärkeimmät komentosarja ja verkon määritys
Tallenna koko komentosarja PowerShell-komentosarjatiedosto. Verkko-määritysten tallentaminen "NetworkConf1.xml"-tiedostoon.
Muokkaa käyttäjän määrittämät muuttujat tarpeen mukaan. Suorita komentosarja ja noudata edellä olevassa esimerkissä 1-osioon palomuurin säännön asennuksen ohje.

#### <a name="full-script"></a>Koko komentosarja
Tämä komentosarja on käyttäjän määrittämä; muuttujat perusteella

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
      Example of Network Security Groups in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Two new cloud services
       - Two Subnets (FrontEnd and BackEnd subnets)
       - One server on the FrontEnd Subnet
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
        $NetworkConfigFile = "C:\Scripts\NetworkConf1.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
      
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure proper NSG Rule creation later in this script:
        #       - The Web Server must be VM 0
        #       - The AppVM1 Server must be VM 1
        #       - The DNS server must be VM 3
        #
        #       Otherwise the NSG rules in the last section of this
        #       script will need to be changed to match the modified
        #       VM array numbers ($i) so the NSG Rule IP addresses
        #       are aligned to the associated VM IP addresses.
    
        # VM 0 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.5"
    
        # VM 1 - The First Application Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 2 - The Second Application Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 3 - The DNS Server
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
            New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                Remove-AzureEndpoint -Name "PowerShell" | `
                New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Note: A Remote Desktop endpoint is automatically created when each VM is created.
            $i++
        }
        # Add HTTP Endpoint for IIS01
        Get-AzureVM -ServiceName $ServiceName[0] -Name $VMName[0] | Add-AzureEndpoint -Name HTTP -Protocol tcp -LocalPort 80 -PublicPort 80 | Update-AzureVM
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
        
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rules
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internal DNS" -Type Inbound -Priority 100 -Action Allow `
            -SourceAddressPrefix VIRTUAL_NETWORK -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[3] -DestinationPortRange '53' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable RDP to $VNetName VNet" -Type Inbound -Priority 110 -Action Allow `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '3389' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable Internet to $($VMName[0])" -Type Inbound -Priority 120 -Action Allow `
            -SourceAddressPrefix Internet -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[0] -DestinationPortRange '*' `
            -Protocol *
    
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Enable $($VMName[0]) to $($VMName[1])" -Type Inbound -Priority 130 -Action Allow `
            -SourceAddressPrefix $VMIP[0] -SourcePortRange '*' `
            -DestinationAddressPrefix $VMIP[1] -DestinationPortRange '*' `
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
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
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
[1]: ./media/virtual-networks-dmz-nsg-asm/example1design.png "Saapuvien DMZ NSG kanssa"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md

