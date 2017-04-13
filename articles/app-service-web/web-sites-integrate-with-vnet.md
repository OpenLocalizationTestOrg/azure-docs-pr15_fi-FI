<properties 
    pageTitle="Integroi sovelluksen Azure Virtual verkossa" 
    description="Avulla voit muodostaa yhteyden Azure App Service-sovelluksen uuteen tai aiemmin luotuun Azure virtual verkkoon" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integroi ohjeartikkelista Azure Virtual verkko #

Tämän asiakirjan Azure App palvelun VPN integrointi-toiminnon kuvaus ja näyttää, miten voit määrittää sen [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)-sovellusten kanssa.  Jos olet perehtynyt Azure Virtual verkkojen (VNETs), tämä on ominaisuus, jonka avulla voit sijoittaa monia Azure resurssien-internet routeable verkossa, jossa voit hallita käyttöoikeuksia.  Nämä verkot voidaan yhdistää sitten käytössä verkkoihin paikalliseen VPN-tekniikoiden erilaisilla.  Saat lisätietoja Azure Virtual verkkojen aloittaminen seuraavat tiedot: [Azure Virtual yleistä][VNETOverview].  

Sovelluksen Azure-palvelu on kaksi.  

1. Usean vuokraajan järjestelmiin, jotka tukevat monia hinnat suunnitelmat
1. Sovelluksen palvelun ympäristössä (Ietokannan) premium ominaisuus joka ottaa käyttöön oman VNET kyselyjä.  

Tämän asiakirjan käy läpi VNET integrointi ja ei App Service-ympäristössä.  Jos haluat lisätietoja Ietokannan-ominaisuus ja Käynnistä haluamasi tiedot: [App palvelun ympäristön johdanto][ASEintro].

VNET integrointi web app-käyttö antaa virtual verkon resursseja, mutta ei yksityisen käyttöoikeuden myöntäminen web Appin virtuaalinen verkosta.  Yksityinen sivuston käyttöoikeuden on käytettävissä vain määritetty kanssa sisäinen kuormituksen tasauspalvelun (ILB) Ietokannan kanssa.  Lisätietoja ILB Ietokannan aloittaminen on artikkelissa: [luominen ja käyttäminen ILB Ietokannan][ILBASE]. 

Käytetty vaihtoehto, joissa kannattaa käyttää VNET integrointi on ottaminen käyttöön access-verkkosovelluksen tietokantaan tai suoritettavat virtual tietokoneeseen Azure virtual verkon WWW-palvelut.  VNET integrointi ei tarvitse näyttää julkisen päätepisteen sovelluksia, että AM, mutta se ei voi käyttää yksityisten-internet reititettävä osoitteiden sijaan.  

VNET Integration-toimintoa:

- edellyttää vakio- tai Premium hinnoittelua suunnitelma 
- toimivatko Classic(V1) tai resurssi Manager(V2) VNET 
- tukee TCP- ja UDP
- Web-, matkapuhelin-ja API toimii
- ottaa käyttöön sovelluksen muodostaa vain 1 VNET kerrallaan.
- mahdollistaa enintään 5 VNETs integroida--sovelluksen-palvelun suunnittelu 
- sallii saman VNET käytettävän useiden sovelluksista-sovelluksen-palvelun suunnittelu
- tukee 99,9 %-SLA VNET yhdyskäytävän riippuvuuden vuoksi

On joitakin toimintoja, jotka VNET integrointi ei tue, mukaan lukien:

- aseman käyttöönottaminen
- AD-integrointi 
- NetBios
- Yksityinen sivuston käyttöoikeudet

### <a name="getting-started"></a>Käytön aloittaminen ###
Seuraavat asiat kannattaa ottaa huomioon ennen yhteyden muodostamista web Appin virtuaalinen verkon:

- VNET integrointi toimii vain **Vakio** tai **Premium** hinnat suunnitelman sovellukset.  Jos-toiminnon käyttöön ottaminen ja skaalata App palvelun Palvelupakettisi ei tueta hinnoittelu-palvelupakettia sovelluksia menettää yhteydet VNETs, joita hän käyttää.  
- Jos virtual verkon kohde on jo luotu, sen on oltava piste sivuston VPN käyttöön dynaaminen reititys Gatewayn, ennen kuin se voidaan yhdistää sovellukselle.  Pisteen sivuston näennäisen yksityisverkon (VPN) ei voi ottaa käyttöön, jos käyttämäsi yhdyskäytävän määritetään käyttämään staattista reititys.
- VNET on oltava sovelluksen-palvelun Plan(ASP) saman tilauksen.  
- Sovellukset, jotka VNET integroida käyttämällä, joka on määritetty kyseisen VNET DNS.
- Oletusarvoisesti integrointi sovelluksia vain reitittää liikenteen kyselyjä oman VNET tiet, jotka on määritetty oman VNET perusteella.  


## <a name="enabling-vnet-integration"></a>VNET integrointi ottaminen käyttöön ##

Tämä asiakirja on koskivat pääasiassa Azure-portaalin käytön VNET integrointi.  VNET-integroinnin valitseminen käyttöön PowerShellin avulla sovelluksen kanssa, noudata ohjeita tähän: [Yhdistä virtual verkkoon PowerShell-toiminnolla sovelluksen][IntPowershell].

Voit halutessasi sovelluksen muodostaa uusi tai aiemmin luotu virtual verkkoon.  Jos luot uuden verkon osana integrointi sitten lisäksi luominen vain VNET, dynaaminen reititys yhdyskäytävä ole valmiiksi määritetyn puolestasi ja sivuston VPN, osoita otetaan käyttöön.  

>[AZURE.NOTE] Uusi VPN-integroinnin määrittäminen voi kestää useita minuutteja.  

Ota käyttöön integrointi VNET Avaa sovelluksen asetukset ja valitse sitten Verkko.  Käyttöliittymä, joka avaa on kolme verkko-vaihtoehtoa.  Tässä oppaassa on vain ohjaaminen pois VNET integrointi vaikka Hybrid yhteydet ja sovelluksen palvelun ympäristöissä käsitellään jäljempänä tässä asiakirjassa.  

Jos sovellus on ei-oikean hinnoittelua suunnitelman Käyttöliittymän helpfully käyttöön palvelupakettisi valittua suurempi hinta palvelupakettiin Skaalaa.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>VNET integrointi olemassa VNET ottaminen käyttöön###
VNET integrointi Käyttöliittymän avulla voit valita oman VNETs luettelosta.  Perinteinen VNETs ilmoittaa, että ne ovat esimerkiksi sanalla "Perinteinen" sulkeissa VNET nimen vieressä.  Luettelo on lajiteltu siten, että Resurssienhallinta VNETs luettelossa on ensimmäisenä.  Alla olevassa kuvassa näet, että vain yksi VNET on valittuna.  On useita syitä VNET harmaana, mukaan lukien:

- VNET on toiseen tilaukseen, tilisi on käyttöoikeus
- VNET ei ole käytössä sivuston piste
- VNET ei ole dynaaminen reititys yhdyskäytävän


![][2]

Voit ottaa käyttöön integrointi napsauttamalla riittää, että haluat integroida VNET.  Kun olet valinnut VNET, sovellus käynnistyy automaattisesti uudelleen, jotta muutokset tulevat voimaan.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Ota käyttöön perinteinen VNET sivuston, osoita #####
Jos yhteyttä VNET ei ole yhdyskäytävän eikä on kohdassa sivuston sinun on ensin, joka määrittää.  Voit tehdä tämän perinteinen VNET, siirry [Azure Portal] [ AzurePortal] ja tuo näkyviin Virtual Networks(classic) luettelo.  Tässä napsauttamalla verkossa haluat integroida ja valitse sitten haluamasi suuri kutsutaan VPN-yhteydet Essentials-ruudusta.  Täällä voit luoda oman sivuston VPN ja se luo yhdyskäytävä on myös osoittamalla.  Kun palaat sivuston yhdyskäytävän luominen käyttökokemuksen pisteen kautta on noin 30 minuuttia, ennen kuin se on valmis.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Osoita Resurssienhallinta VNET sivuston ottaminen käyttöön #####

Resurssienhallinta VNET yhdyskäytävän avulla ja osoita sivuston, sinun on käytettävä PowerShell suorittimessa tässä [PowerShellin virtual verkon pisteen sivuston yhteyden asetusten määrittäminen][V2VNETP2S].  Käyttöliittymän suorittamiseen tätä ominaisuutta ei ole vielä käytettävissä. 

### <a name="creating-a-pre-configured-vnet"></a>Valmiiksi määritetyt VNET luominen ###
Jos haluat luoda uuden VNET, joka on määritetty yhdyskäytävän ja pisteen sivuston verkko Käyttöliittymän App palvelulla ei voi tehdä, mutta vain Resurssienhallinta VNET.  Jos haluat luoda perinteinen VNET yhdyskäytävän ja pisteen sivuston sinun on suoritettava tämä manuaalisesti verkko käyttöliittymässä. 

Luo Resurssienhallinta-VNET VNET integrointi Käyttöliittymän kautta, riittää, että valitsemalla **Luo uusi virtuaaliverkko** ja anna:

- Virtuaalinen verkkonimi
- VPN-Osoitelohko
- Aliverkon nimi
- Aliverkon Osoitelohko
- Yhdyskäytävän Osoitelohko
- Pisteen sivuston Osoitelohko

Jos haluat muodostaa yhteyden jonkin muun verkon tämän VNET sitten Vältä valitsemalla IP-osoitetilaa, joka on päällekkäinen verkkoihin kanssa.  

>[AZURE.NOTE] Yhdyskäytävän luominen Resurssienhallinta VNET kestää noin 30 minuuttia ja VNET integroi tällä hetkellä ole sovelluksen.  Kun oman VNET on luotu Gatewayn joudut palaa sovelluksen VNET integrointi Käyttöliittymä ja valitse uusi VNET.

![][3]

Azure VNETs luodaan tavallisesti yksityisverkon osoitteet.  Oletusarvoisesti VNET integrointi ominaisuus reitittää liikenteen tarkoitettu nämä IP-osoitealueet oman VNET kyselyjä.  Yksityisen IP-osoitealueet ovat seuraavat:

- 10.0.0.0/8 - on sama kuin 10.0.0.0 - 10.255.255.255
- 172.16.0.0/12 - on sama kuin 172.16.0.0 - 172.31.255.255 
- 192.168.0.0/16 - on sama kuin 192.168.0.0 - 192.168.255.255
 
VNET osoitetilaa varten on määritettävä CIDR merkintätapaa.  Jos olet perehtynyt CIDR merkintätapaa, on menetelmän osoitelohkot IP-osoite ja kokonaisluku, joka edustaa verkkopeite argumenteille. Pikaopas, kuin huomioon, että 10.1.0.0/24 olisi 256 osoitteet ja 10.1.0.0/25 olisi 128 osoitteet.  IPv4-osoite /32 kanssa olisi vain 1 osoite.  

Jos määrität DNS-palvelimen tiedot tähän sitten, joka määritetään oman VNET.  VNET luomisen jälkeen voit muokata näitä tietoja VNET käyttäjän kokemukset.

Kun luot perinteinen VNET, VNET integrointi käyttöliittymässä, se luo VNET sovelluksen samaan resurssiryhmään. 

## <a name="how-the-system-works"></a>Järjestelmän toiminta ##
Kohdassa kattaa toiminto muodostaa pisteen sivuston VPN-tekniikka sovelluksen muodostaa yhteyttä VNET päälle.   Sovellusten Azure sovelluksen-palvelussa on usean vuokraajan järjestelmän-arkkitehtuuri joka estää valmistelu sovelluksen suoraan VNET, kuten näennäiskoneiden tehokkaasti.  Muodostamalla-kohdassa sivuston tekniikan rajoittaa vain isännöinnin sovelluksen virtuaalikoneen yhteys.  Access verkko on edelleen rajoitettu ne app isännät niin, että sovellukset voivat käyttää ainoastaan verkkoja, jotka määrität käyttäjälle.  

![][4]
 
Jos DNS-palvelin ei ole määritetty virtual verkoston sinun on käytettävä IP-osoitteet.  Käytettäessä IP-osoitteiden muista, että tätä ominaisuutta pää etuna on se, että sen avulla voit käyttää yksityinen verkossa oleville yksityiset osoitteet.  Jos määrität sovelluksen käyttämään julkiseen IP-osoitteet jokin oman VMs VNET integrointi ei ole ominaisuudella ja Internetin yhteydessä.


##<a name="managing-the-vnet-integrations"></a>VNET integroinnit hallinta##

Mahdollisuus muodostaa ja katkaista VNET on sovelluksen tasolla.  Toiminnot, jotka voivat vaikuttaa VNET integrointi käytettävistä useiden sovelluksista ovat ASP tasolla.  -Käyttöliittymä, joka näkyy sovelluksen tasolla saat tiedot-kohdassa VNET.  Useimmat samat tiedot näkyy myös ASP tasolla.  

![][5]

Verkon ominaisuus tila-sivulla näet, jos sovellus on yhdistetty oman VNET.  Jos VNET yhdyskäytävä on poissa käytöstä jostakin syystä sitten tämä näkyy vain ei-yhteys.  

Tiedot on nyt käytettävissä Käyttöliittymän tason VNET-integrointi on sama kuin tiedot, sinulla on ASP-sovelluksessa.  Seuraavat kohteet:

- VNET nimi - tämä linkki avaa verkon Käyttöliittymä
- Sijainti - Tämä vastaa omaa VNET sijainti.  On mahdollista integroida VNET toiseen sijaintiin.
- Varmenteen tila - on sertifikaatit suojaamiseen VPN-yhteyden sovellus ja VNET välillä.  Tämä kuvastaa testi, jotta ne on synkronoitu.
- Yhdyskäytävän tila - että yhdyskäytävien olisi alaspäin jostakin syystä sitten sovelluksen ei voi käyttää VNET resursseja.  
- VNET osoitetilaa – tämä on oman VNET IP-osoitetilaa.  
- Sivuston osoitetilaa – Valitse tämä on sivuston IP-osoitetilaa oman VNET osoittamalla.  Sovelluksen näkyy viestintä tulevaksi olevista osoite tähän IP-osoitteet.  
- Sivuston sivuston osoite-kohtaan - sivusto sivusto VPN-yhteydet avulla voit muodostaa yhteyden oman VNET käytössä paikalliseen resurssien tai muita VNETs.  Sinun on määritetty sitten IP-alueita määritetty VPN-yhteyden näkyy tässä.
- DNS-palvelimet - Jos sinulla on määritetty oman VNET sitten ne on lueteltu tässä DNS-palvelimet.
- IP-osoitteet reitittyy VNET - palvelussa on, että VNET reititys on määritetty IP-osoitteiden luettelo.  Osoitteiden näkyy tässä.  

Voit tehdä VNET integrointi sovelluksen-näkymässä vain toiminto on sovelluksen katkaiseminen VNET, se on liitetty.  Voit tehdä tämän valitsemalla Katkaise yhteys yläreunassa.  Tämä toiminto ei vaikuta oman VNET.  Myös yhdyskäytävien VNET ja sen määritys ei muutu.  Jos haluat poistaa oman VNET sitten sinun on ensin poistettava se myös yhdyskäytävien resursseista.  

Sovelluksen-palvelun suunnittelu-näkymässä on useita lisätoiminnot.  Se on käytettävissä eri tavalla kuin sovellus.  Saavuttamiseksi ASP verkko-Käyttöliittymän yksinkertaisesti Avaa ASP Käyttöliittymä ja vieritä alas.  Tällä nimeltä verkon toiminnon tilan Käyttöliittymän-elementtiä.  Sen avulla joitakin pieniä tiedot VNET integrointi ympärille.  Verkon ominaisuus tila-Käyttöliittymän valitsemalla tämä Käyttöliittymä avautuu.  Jos valitset sitten "Hallittavan napsauttamalla tätä" Voit avata Käyttöliittymä, jossa on luettelo tämän ASP VNET-Integrointien ylöspäin.

![][6]

ASP sijainti on hyvä muistaa, kun etsit on integroitu VNETs paikoissa.  Kun VNET on toiseen sijaintiin olet paljon enemmän todennäköisesti Nähdäksesi viive ongelmat.  

Integroitu VNETs on sovelluksia on integroitu tämän ASP ja kuinka monta voit määrittää, kuinka monta VNETs muistutus.  

Jos haluat nähdä kunkin VNET lisätyt tiedot, valitse kiinnostavan VNET.  Lisäksi tiedot, jotka on merkitty aiemmin näet myös luettelon tämän ASP-sovellukset, jotka käyttävät kyseistä VNET.  

Jotka koskevat toimenpiteet on kaksi ensisijainen toiminnot.  Ensimmäinen on mahdollista lisätä tiet, jotka vaikuttavat liikenne jätä sovelluksen oman VNET kyselyjä.  Toista toiminto on mahdollisuus varmenteet ja verkkotiedot.

![][7]

**Reititys** Edellä tiet, jotka on määritetty oman VNET ovat käyttötarkoitus ohjaa lopuksi liikenne oman VNET sovelluksestasi kyselyjä.  On esimerkkejä käyttää mutta missä asiakkaiden haluat lähettää Lisää lähtevän tietoliikenteen sovelluksen VNET ja niiden tämä ominaisuus on annettu.  Mitä tapahtuu, liikenne, kun, joka on enintään kuinka asiakas määrittää niiden VNET.  

**Varmenteet** Varmenteen tila kuvastaa suoritettavan Vahvista sovelluksen-palvelu, joka on käytössä VPN-yhteyden varmenteet ovat edelleen hyviä tarkistuksen.  Kun VNET integrointi otettu käyttöön, valitse Jos näin, että VNET ensimmäisen integrointi tämän ASP-sovellusten ei tarvittavat exchange varmenteiden suojauksesta yhteyden.  Sekä varmenteet on Hae DNS-määrityksiä, tiet ja muita samanlaisia asioita, jotka kuvaavat verkossa.
Jos nämä varmenteet tai verkkotietoja on muutettu täytyy "Synkronoi verkko".  **Huomautus**: kun napsautat "Synkronointi verkon" sitten lyhyt käyttökatkosta aiheuttaa yhteyden sovelluksen ja että VNET välillä.  Kun sovellus käynnistetään ei menettäminen saattaa aiheuttaa sivustosi ei ehkä toimi oikein.  

##<a name="accessing-on-premise-resources"></a>Paikalliseen resurssien käyttäminen##

VNET Integration-toimintoa edut siksi oman VNET yhteyttä sivuston-sivusto näennäisen yksityisverkon käytössä paikalliseen verkkoon, ja valitse sovellus voi olla access käytössä paikalliseen resurssit-sovellukset.  Tämän ominaisuuden toimivat, mutta voit joutua päivittämään käytössä paikalliseen VPN-yhdyskäytävän tiet, oman sivuston IP, osoita alue.  Kun sivuston sivusto-VPN on ensin määritettävä komentosarjojen avulla määrittää sen kannattaa määrittää tiet, mukaan lukien oman sivuston VPN, osoita.  Jos lisäät kohtaan sivuston VPN-yhteyttä, oman sivuston sivusto-VPN luo sitten sinun on päivitettävä manuaalisesti tiet.  Tiedot siitä, miten voit tehdä vaihtelevat yhdyskäytävän kohden ja ole kuvattu tässä.  

>[AZURE.NOTE] Kun VNET integrointi-toiminto toimi sivuston-sivusto käyttämään paikalliseen resurssien VPN se tällä hetkellä ei toimi ExpressRoute-VPN samalla.  Tämä on TOSI, kun ja perinteinen tai Resurssienhallinta VNET.  Jos haluat käyttää resursseja ExpressRoute VPN-verkon kautta voit käyttää Ietokannan, joka suorittaa oman VNET. 

##<a name="pricing-details"></a>Hinnoittelutiedot##
On muutama hinnoittelua nuances, sinun olisi otettava huomioon, kun VNET integrointi-ominaisuudella.  Käytä tätä ominaisuutta 3 liittyvät kulut on:

- ASP hinnoittelua taso-vaatimukset
- Tietojen siirtäminen kustannukset
- VPN-yhdyskäytävän kustannukset.

Sovellukset voivat käyttää tätä toimintoa ne on vakio tai Premium App palvelun suunnitteleminen.  Näet lisätietoja näistä kustannuksia: [App palvelun hinnat][ASPricing]. 

Sivuston VPN-yhteydet tapa pisteen eivät käsitellään, sinulla on aina lähtevien tietojen kulun VNET integrointi-yhteyden kautta, vaikka VNET on sama tietokeskuksen.  Katso, kuinka kulut ovat Tutustu tähän: [Tietojen siirtäminen hinnat tiedot][DataPricing].  

Viimeisen kohteen on tuotteen VNET yhdyskäytävää.  Jos et tarvitse yhdyskäytävien jotain muuta esimerkiksi sivuston-sivusto VPN-yhteydet sitten maksat yhdyskäytävien tukemaan VNET integrointi-ominaisuutta.  Nämä kustannukset on tiedot: [VPN-yhdyskäytävän hinnat][VNETPricing].  

##<a name="troubleshooting"></a>Vianmääritys##

Silloin, kun ominaisuus on helppoa, joka ei tarkoita olevan käyttökokemuksen vapaa ongelma.  Olisi käytössä ilmenee ongelmia haluttua päätepisteen siellä on joitakin apuohjelmia, voit tehdä Testaa yhteys app konsolin.  On kaksi console-versiota, voit käyttää.  Yksi on Kudu-konsolin ja toinen konsolin, voit kätevästi ottaa Azure-portaalissa.  Sovelluksen käyttämistä varten Kudu console-valitsemalla Työkalut > Kudu.  Tämä on sama kuin siirtymällä [nimi]. scm.azurewebsites.net.  Kun näkyviin tulee vain Siirry virheenkorjaus console-välilehti.  Saat Azure portaalin isännöityä konsoliin Valitse sovellukset-valitsemalla Työkalut > konsolissa.  


####<a name="tools"></a>Työkalut####

Työkalut ping, nslookup ja tracert eivät toimi suojauksen rajoitusten vuoksi-konsolin kautta.  Täytä void palvelussa on kaksi erillistä Työkalut lisätty.  Jotta voit testata DNS-toimintoja lisätty nameresolver.exe-niminen työkalu.  Syntaksi on:

    nameresolver.exe hostname [optional: DNS Server]

Voit tarkistaa isäntänimet, joissa sovellus riippuu nameresolver.  Näin voit testata Jos tekemistä väärin määritetty DNS- tai ehkä pääse käyttämään DNS-palvelimeen.

Seuraava-työkalun avulla voit testata TCP yhteys isännän ja portin yhdistelmä.  Tämä työkalu kutsutaan tcpping.exe ja syntaksi on:

    tcpping.exe hostname [optional: port]

Tämä työkalu ilmoittaa, jos voit saavuttaa tietyn isännän ja portin, mutta ei toimi ICMP sisältämistä saman tehtävän perusteella ping-apuohjelma.  ICMP ping-apuohjelman kertoo, onko isäntä ylöspäin.  Tcpping ja huomaat, jos sinulla on pääsy isännän tiettyyn porttiin.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Virheenkorjaus VNET pääsy isännöidään resurssit####

On useita kohteita, jotka saattavat estää sovelluksen ottamasta tietyn isännän ja portin.  Yleensä on jonkin seuraavista toimista:

- **Tällä tavalla palomuuri**  Jos käytössäsi on palomuuri tavalla sitten valitset TCP-aikakatkaisun.  Tämä on 21 sekuntia tällöin.  Testaa yhteys tcpping-työkalun avulla.  TCP aikakatkaisu voi vuoksi lisäksi palomuurit monella eri tavalla, mutta Käynnistä-painiketta.  
- **DNS ei ole käytettävissä**  DNS-aikakatkaisu on 3 sekuntia DNS-palvelinta kohden.  Jos sinulla on 2 DNS-palvelimet, joilla on 6 sekuntia.  Toimiiko DNS nameresolver avulla.  Muista, et voi käyttää nslookup, jotka eivät käytä oman VNET on määritetty käyttämään DNS.
- **Virheellinen P2S IP-alue** Osoita sivuston IP-alueen on oltava RFC 1918 yksityinen IP-alueita (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) Jos alueen käyttää IP-osoitteet ulkopuolella, sitten asiat eivät toimi.  

Jos kohteet eivät vastaa ongelmaasi, Etsi ensimmäinen yksinkertaisempia asioita, kuten:  

- Yhdyskäytävää Näytä saattoi-portaalissa
- Varmenteet näkyy siten, että synkronointiin?
- Kuka tahansa käyttäjä, vaihtanut verkon määritysten tekemättä "Synkronointi verkon" tarvittavien ASP: T? 

Jos käyttämäsi yhdyskäytävän alas sitten noutaa sen takaisin.  Jos sertifikaattien tarkasteleminen ei ole synkronoitu siirtymällä VNET integrointi ASP-näkymään ja hit "Synkronointi verkon".  Jos epäilet, että VNET kokoonpanosi tehty muutos on ollut ja sitä ei synkronoi samalla oman ASP: T Siirry VNET integrointi ASP-näkymä ja hit "Synkronointi verkon" Just muistutukseksi, tämä aiheuttaa lyhyt käyttökatkosta VNET-yhteyden ja sovelluksia.  

Jos kaikki, jotka on kunnossa sinun on tarkastella hieman tarkemmin –:

- Onko VNET integroinnin saavuttaa saman VNET resurssien muista sovelluksista? 
- Voit siirtyä sovelluksen konsolin ja käyttää tcpping saavuttamiseksi muut resurssit-että VNET?  

Jos joko edellä mainitut ehdot täyttyvät sitten VNET integrointi on kunnossa ja ongelma on toiseen sijaintiin.  Tämä on kohtaa, johon se saa olla hankala seuraavista tavallista sivua ei ole helppo artikkelissa miksi isäntä: portti saavuta.  Muun muassa seuraavat syitä:

- Sinulla on palomuuri sovelluksen portti access estää oman sivuston IP-alueen, osoita isäntä.  Yleisön leikkaavat aliverkosta usein edellyttää.
- target host ei ole käytettävissä
- sovellus ei ole käytettävissä
- on väärä IP- tai isäntänimi
- sovelluksen seuraa eri porttiin kuin vastaa odotuksiasi.  Voit tarkistaa tämän siirtymällä sivulle isännöivien ja käyttämällä "netstat - aon" cmd seuraavasti.  Tämä näyttää prosessin tunnus Kuuntele mitä portti.  
- Verkko-käyttöoikeusryhmät on määritetty siten, että ne estää access sovelluksen isäntä ja portin oman sivuston IP-alueen, osoita

Muista, että et tiedä, mitä IP-kohdan sivuston IP-alue, jota sovellus käyttää niin, että sinun on sallittava access koko alueelta.  

Vianmäärityksen vaiheet ovat seuraavat:

- Kirjaudu sisään-että VNET toisen AM ja yritä muodostaa yhteys käyttämäsi resurssi Host (isäntä): portti sieltä.  Jotkin TCP ping-apuohjelmat on, että voit käyttää tätä varten, tai voit myös käyttää Telnetiä, jos tarvitse.  Tähän tarkoitukseen on selvittää, jos yhteys on käytettävissä tämän AM. 
- noutaa toiseen AM ja testaa access-sovellukseen, isännän ja portin konsolin-sovellukset  
####<a name="on-premise-resources"></a>Paikalliseen resursseja####
Jos yhteyttä saavuta resurssien paikalliseen, sinun on otettava ensiksi on Jos voit siirtyä resurssin oman VNET.  Jos, joka toimii seuraavat vaiheet on melko helppoa.  Kohteesta-kohdassa VNET AM haluat yritä muodostaa yhteys paikalliseen käyttöön sovelluksen.  Voit käyttää telnet tai TCP ping-apuohjelma.  Jos yhteyttä AM et voi saavuttaa käytössä paikalliseen resurssin sitten Varmista ensin, että sivusto sivusto VPN-yhteyden toimii.  Jos se toimi, tarkista merkille aiemmin käytössä paikalliseen yhdyskäytävän määritys ja tila sekä samoista asioista.  

Nyt Jos oman VNET hallinnoida AM kätevästi ottaa käyttöön paikalliseen järjestelmään, mutta sovellus ei onnistu, syynä on todennäköisesti jompikumpi seuraavista:
- oman sivuston IP-alueita on paikalliseen-yhdyskäytävän, osoita ei ole määritetty yhteyttä tiet
- Verkko-käyttöoikeusryhmät estävät access oman sivuston IP-alueen, osoita varten
- käytössä paikalliseen-palomuurit estävät tietoliikenteen kohdan sivuston IP-alue
- Käyttäjän määrittämät Route(UDR) ovat oman VNET, joka estää kohdan sivuston liikenne ottamasta käytössä paikalliseen verkkoon

## <a name="hybrid-connections-and-app-service-environments"></a>Hybrid yhteydet ja sovelluksen palvelun ympäristöissä##
On 3 ominaisuudet, joita isännöidään VNET resurssien käytön käyttöön.  Ne:

- VNET integrointi
- Hybrid yhteydet
- Sovelluksen palvelun ympäristöissä

Hybrid yhteydet pyytää asentamaan Hybrid yhteyden Manager(HCM) nimeltä verkon välitys-agentti.  HCM on voi yhdistää Azure ja sovelluksen.  Tämä ratkaisu on erityisen hyvien remote verkosta esimerkiksi käytössä paikalliseen verkkoon tai jopa toisen cloud isännöidään verkon, koska se ei edellytä internet käytettävissä päätepisteen.  HCM toimii vain Windows ja voi olla enintään 5 käynnissä antamaan suuren käytettävyyden esiintymät.  Hybrid yhteydet tukee vain TCP vaikka ja HC päätepisteiden on vastaamaan tietyn isäntä: portti yhdistelmä.  

Sovelluksen palvelun ympäristön-ominaisuuden avulla voit suorittaa Azure-sovelluksen palvelun esiintymän oman VNET.  Näillä oikeuksilla oman VNET ilman mitään lisävaiheita sovellusten Accessin resurssit.  Jotkin sovelluksen Service-ympäristössä muita etuja on, että voit käyttää 8 core omistautunut työntekijöiden 14 Gigatavua RAM-Muistia.  Toinen etu on, että voit skaalata järjestelmän vastaamaan omia tarpeita.  Usean vuokraajan ympäristössä, jossa koko ASP on rajallinen, toisin kuin Ietokannan-käyttöoikeuksia voidaan hallita kuinka paljon resursseja haluat antaa järjestelmään.  Verkon kohdistuksen tämän asiakirjan osalta, Ietokannan, joka ei VNET integrointi sisältämistä toimea on, että ne toimivat ExpressRoute VPN-yhteyttä.  

Kun on joitakin käyttää kirjainkoon limitys-mikään näistä ominaisuus korvaa jokin muiden.  Tietää, mitä ominaisuuden käyttäminen on yhdistetty tarpeitasi ja miten haluat käyttää sitä.  Esimerkki:

- Jos olet kehittäjä ja riittää, että haluat suorittaa sivuston Azure-tietokannassa ja sen tietokannan työasemassa työpisteesi-kohdassa Käytä helpoin on Hybrid yhteydet.  
- Jos olet suurissa organisaatioissa, jotka on runsaasti haluaa web ominaisuudet julkisen cloud ja hallita niitä omia verkko- ja haluat palata App Service-ympäristössä.  
- Jos sinulla on luvun App palvelun isännöidään sovellukset ja riittää, että haluat muodostaa yhteyden oman VNET resurssien VNET integrointi on tapa siirtyä.  

Lisäksi on käytössä tapauksissa jotkin yksinkertaisuuden liittyviä ominaisuuksia.  Jos oman VNET on yhdistetty käyttämällä VNET integrointi käytössä paikalliseen verkkoon tai sovelluksen Service-ympäristössä on helppo tapa tarjoaman paikalliseen resursseja.  Toisaalta että VNET ole yhteyttä käytössä paikalliseen verkkoon useampia katseltavan määrittämään sivuston-sivusto VPN-yhteyttä ei oman VNET verrattuna HCM asentaminen.  

Lisäksi toiminnalliset erot on hinnat myös eroja.  Sovelluksen palvelun ympäristön-ominaisuus on Premium palvelun tarjoaa mutta on eniten verkon määritysten mahdollisuuksia lisäksi hyvien muita ominaisuuksia.  VNET integrointi voidaan käyttää vakio- tai Premium ASP: T ja erinomaisesti muissa suojatusti oman VNET usean vuokraajasta sovelluksen palvelun resurssit.  Hybrid yhteydet määräytyy BizTalk tili, jolla on hinnat tasot, jotka alkavat vapaa ja hae sitten asteittain kalliimpi perusteella summa, sinun on tällä hetkellä.  Jos käyttänyt monta verkkojen vaikka, ei ole kuten Hybrid yhteyksiä, joiden avulla voit käyttää myös yli 100 erillisten verkkojen resurssien ominaisuus.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
