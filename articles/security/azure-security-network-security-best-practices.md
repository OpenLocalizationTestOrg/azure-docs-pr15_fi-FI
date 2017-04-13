<properties
   pageTitle="Azure verkon suojauksen parhaiden käytäntöjen | Microsoft Azure"
   description="Tässä artikkelissa on joukko parhaat käytännöt verkon suojauksen Azure ominaisuuksien hallinta."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="swadhwa"
   editor="TomShinder"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/25/2016"
   ms.author="TomSh"/>

# <a name="azure-network-security-best-practices"></a>Azure verkon suojauksen parhaat käytännöt

Microsoft Azure avulla voit muodostaa yhteyden näennäiskoneiden ja laitteiden muiden verkkolaitteiden sijoittamalla Azure Virtual verkoissa. Virtuaalinen Azure-verkko on VPN-rakennetta, jonka avulla voit muodostaa VPN-liittymän kortit virtual verkon TCP/IP-pohjaisen viestinnän käytössä verkkolaitteiden välillä. Azuren näennäiskoneiden yhteydessä Virtual Azure-verkkoon on pystyttävä muodostamaan yhteys samassa verkossa Azure Virtual, eri Azure Virtual verkkoihin, Internet- tai jopa oman paikallisen-verkoissa laitteet.

Tässä artikkelissa käsittelemme Azure verkon suojauksen parhaiden käytäntöjen kokoelma. Näiden vinkkien johdettuja Microsoftin kokemusta Azure verkko ja asiakkaiden kokemukset, kuten itse.

Parhaita käytäntöjä, kerrotaan:

- Mikä on paras käytäntö
- Miksi haluat ottaa käyttöön kyseisen parhaat käytännöt
- Mitä voi olla tulos, jos et ota parhaita käytäntöjä
- Paras käytäntö mahdollista vaihtoehtoja
- Miten voit lukea käyttöön parhaat käytännöt

Artikkelissa Azure verkon suojauksen parhaiden käytäntöjen perustuu yksimielisyyttä-mielestä ja Azure platform ominaisuudet ja määrittää ominaisuus, tässä artikkelissa on kirjoitettu milloin olemassa. Lausuntoja ja -tekniikoiden muuttuvat ajan kuluessa ja sisältö päivitetään säännöllisesti muutosten mukaisiksi.

Azure verkon suojauksen parhaiden käytäntöjen tässä artikkelissa kuvatut ovat seuraavat:

- Loogisesti segmentin aliverkosta
- Hallita reititys
- Ota käyttöön pakotetun Tunneling
- Käytä Virtual verkon laitteiden
- Ota käyttöön DMZs suojauksen VYÖHYKEJAKO
- Vältä Internet-yhteys on erillinen WAN linkkejä näyttäminen
- Optimoi käytettävyyttä ja suorituskyky
- Käytä yleistä kuormituksen tasaamisen
- Poistaminen käytöstä RDP käyttöoikeuksien Azure-Virtuaalikoneissa
- Ota käyttöön Azure Tietoturvakeskus
- Laajentaa Azure oman palvelinkeskukseen


## <a name="logically-segment-subnets"></a>Loogisesti segmentin aliverkosta

[Azure Virtual verkkojen](https://azure.microsoft.com/documentation/services/virtual-network/) muistuttavat Lähiverkon paikallisen verkossa. Virtuaalinen Azure-verkko on, että voit luoda yksittäisen IP osoite tilaa perustuva yksityisverkko, voit sijoittaa kaikki oman [Azuren näennäiskoneiden](https://azure.microsoft.com/services/virtual-machines/). Yksityinen IP address välilyönnit käytettävissä ovat luokan A (10.0.0.0/8), luokan B (172.16.0.0/12) ja luokan C (192.168.0.0/16)-alueita.

Samalla mitä teet paikallisen, kannattaa määritetään suurempi osoitetilaa aliverkosta kyselyjä. Voit luoda oman aliverkosta [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) perusteella muut arvot periaatteet.

Reititys aliverkosta välillä tapahtuu automaattisesti, ja sinun ei tarvitse manuaalisesta reititys taulukot. Oletusasetus on kuitenkin, että luot Azure Virtual verkossa aliverkosta välillä ei ole verkon access-ohjausobjektit. Jotta voit luoda verkon access-ohjausobjektit välinen aliverkosta, sinun on objekti aliverkosta välillä.

Voit suorittaa tämän tehtävän toimea on [Verkko-käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) (NSG). NSGs on yksinkertainen tilallisten pakettien tarkastuslaitteiden, jotka käyttävät 5-monikon (esimerkiksi lähde-IP Lähdeportti, kohde-IP, Kohdeportti ja kerros 4 protocol) menetelmä luo Salli tai estä säännöt verkkoliikennettä. Voit sallia tai estää liikenteen ja yksittäisen IP-osoite, ja IP-osoitteista useita tai jopa ja sieltä pois koko aliverkosta sieltä.

NSGs käyttäminen verkon käytönhallinta välillä aliverkosta avulla voit sijoittaa resurssit, jotka kuuluvat samaan vyöhykkeellä tai oman aliverkosta rooli. Esimerkiksi ajatella yksinkertainen 3 taso-sovellus, jossa on web taso, sovellus-logiikkaa taso ja tietokannan taso. Voit lisätä näennäiskoneiden, jotka kuuluvat eri nämä tasoa omia aliverkosta kyselyjä. Valitse NSGs käyttämällä voit hallita liikenne aliverkosta välillä:

- Web-tason näennäiskoneiden vain aloittaa sovelluksen logiikan koneet yhteyksiä ja voi hyväksyä vain yhteyksiä Internetistä
- Sovelluksen logiikan näennäiskoneiden vain aloittaa tietokannan taso yhteyksiä ja voi hyväksyä vain web-tason yhteydet
- Tietokannan taso näennäiskoneiden ei voi aloittaa yhteyttä mitään omia aliverkon ulkopuolella ja voi hyväksyä vain sovelluksen logiikan tason yhteydet

Lisätietoja verkko-käyttöoikeusryhmiä ja kuinka voit niiden avulla voidaan määritetään loogisesti Azure Virtual lopettaminen, lue artikkeli [Mikä on verkon käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) (NSG).

## <a name="control-routing-behavior"></a>Hallita reititys

Kun lisäät virtual machine Azure Virtual verkossa, huomaat, että virtuaalikoneen muodostaa yhteyden muiden koneen virtual samassa Azure Virtual verkossa, vaikka muiden näennäiskoneiden on eri aliverkosta. Miksi tämä on mahdollista syytä on se, että järjestelmä tiet, jotka ovat oletusarvoisesti käytössä, joka antaa tämän tyyppisiä kokoelma. Nämä oletusarvon tiet Salli näennäiskoneiden samassa Azure Virtual verkossa aloittaa yhteyksiä keskenään ja yhteydessä Internetiin (vain Internet lähteviä yhteyksiä) varten.

Oletusarvon järjestelmässä tiet on hyötyä käyttöönottoskenaarioita, liittyy ajat, jos haluat mukauttaa käyttöönoton reititys määrityskohde. Nämä mukautukset avulla voit määrittää seuraava osoite saavuttaa tietyn kohteisiin.

On suositeltavaa, että määrität käyttäjän määrittämät tiet, kun otat käyttöön VPN suojauksen laitteen, jossa käsittelemme myöhemmin parhaita käytäntöjä.

> [AZURE.NOTE] käyttäjän määrittämät tiet eivät ole pakollisia ja oletusarvon järjestelmässä tiet ne toimivat useimmissa tapauksissa.

Voit lukea lisää käyttäjän määrittämät tiet ja määrittää niitä lukemalla artikkelin, [mitä käyttäjän määrittämät tiet, ja IP-osoitteen välityksen](../virtual-network/virtual-networks-udr-overview.md).

## <a name="enable-forced-tunneling"></a>Pakotetun Tunneling ottaminen käyttöön

Ymmärtämään pakotettu tunneling, se on hyödyllinen ymmärtää, mitä "Jaa tunneling" on.
Jaa tunneling yleisimmät Esimerkki nähdään VPN-yhteyttä. Kuvitellaan muodostaa VPN-yhteyden kohteesta hotellista room yrityksen verkkoon. Tämän avulla voit muodostaa yhteyden resurssit yrityksen verkossa ja yhteydenotot resurssien yritysverkossa käyminen läpi VPN-tunnelin.

Mitä tapahtuu, kun haluat muodostaa yhteyden Internet-resurssien? Kun jaettu tunnelointi on käytössä, kyseiset yhteydet Siirry suoraan Internetiin ja ei VPN tunnelissa. Jotkin asiantuntijat harkitse tämä riski ja suosittelee, että Jaa tunneling poistetaan käytöstä ja kaikki yhteydet, joka on tarkoitettu Internet ja tarkoitettu yritysresurssit, siirry VPN tunnelissa. Näin etuna on se, että Internet-yhteyksien pakotetaan sitten yrityksen verkkoon suojaus-laitteet, joka Eikö olla kirjainkokoa, jos VPN-tunnelin ulkopuolella Internetiin yhteydessä VPN-asiakasohjelman kautta.

Nyt oletetaan, että tuo näennäiskoneiden Azure Virtual verkossa sen takaisin. Oletusarvo-tiet Virtual Azure-verkon Salli näennäiskoneiden aloittaa Internet-liikenne. Liian voi edustaa tietoturvariskin, nämä Lähtevät yhteydet voi parantaa uhanalaisten virtual koneen ja hyödyntää hyökkääjät.
Tästä syystä suosittelemme, että otat pakotettu tunneling virtual tietokoneissa ollessasi paikallisen-yhteyksien Azure Virtual verkon ja paikallisen verkon välille. Käsittelemme cross paikallisen yhteyden jäljempänä tässä asiakirjassa Azure verkko parhaat käytännöt.

Jos sinulla ei ole paikallisen välisen yhteyden, varmista, että voit hyödyntää verkon käyttöoikeusryhmät (edellä kerrottiin) tai Azure virtual verkon estää lähteville yhteyksille Internet-yhteyttä Azuren näennäiskoneiden laitteiden security (haavoittuvuuksiin seuraava).

Lisätietoja pakotettu tunnelointiin ja miten voit ottaa käyttöön, lue artikkeli, [Määritä pakotettu Tunneling käyttämällä PowerShell ja Azure Resurssienhallinta](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md).

## <a name="use-virtual-network-appliances"></a>Käytä VPN-laitteille

Kun verkon käyttöoikeusryhmiä ja käyttäjän määrittämät reititys voidaan lisätä mitta [verkkosegmenttiä](https://en.wikipedia.org/wiki/OSI_model)verkko- ja kerrokset verkkosuojaus, ovat käyttäjästä tulee tilanteissa yhteys kohtaa, johon haluat tai on otettava käyttöön korkean tason paperipinon suojaus. Tällaisissa tilanteissa Suosittelemme VPN suojauksen laitteiden Azure kumppanien myöntämä otetaan käyttöön.

Azure verkon suojauksen laitteiden voit toimittaa merkittävästi parannettu tietoturvatasot kautta mitä tarjoaa verkon tason ohjausobjektit. Verkko-tietoturvaominaisuudet myöntämä suojaus VPN-laitteita ovat esimerkiksi seuraavat:

- Firewalling
- Tunkeutumisen tunnistus/tunkeutumisen estäminen
- Haavoittuvuus hallinta
- Sovellusten hallinta
- Verkkopohjainen poikkeavuuksista tunnistus
- Web-suodatus
- Virustentorjuntaohjelman
- Botnet suojaus

Jos asetat verkkosuojauksen ylemmän tason kuin voit hankkia verkon käyttöoikeustaso ohjausobjektien kanssa, valitse Suosittelemme, että voit tutkia ja ottaa käyttöön Azure virtual verkon suojauksen laitteita.

Lisätietoja siitä, mitä Azure virtual verkon suojauksen laitteita ovat käytettävissä ja niiden ominaisuuksista, siirry [Azure Marketplacesta](https://azure.microsoft.com/marketplace/) ja Etsi "suojaus" ja "verkkosuojaus".

##<a name="deploy-dmzs-for-security-zoning"></a>Ota käyttöön DMZs suojauksen VYÖHYKEJAKO
DMZ tai "ympäröivän verkon" on fyysinen tai looginen verkko-osiossa, joka on suunniteltu säätää kerroksen suojauksen lisääminen varat ja Internetin välillä. DMZ palveluita on paikka laitteiden erityinen verkon käytön hallinta DMZ verkkoa reunan niin, että vain haluamasi liikenne sallitaan aiempia verkon suojaus-laite ja Azure Virtual verkostoon.

DMZs on hyötyä, koska oman verkon käytön hallinta hallinta Voit sovittaa sivun koskemaan seuranta, kirjaaminen ja raportointi laitteissa Azure Virtual verkkoa reunan. Tähän tavallisesti otat WWW.microsoft.com-sivustoa kohtaan estäminen, tunkeutumisen tunnistus/tunkeutumisen estäminen järjestelmien (tunnukset/IP-Osoitteet), palomuurisäännöt ja käytäntöjä, web-suodatuksen, verkon antimalware ja lisää. Suojauksen verkkolaitteet istut Internet- ja Azure Virtual verkon välillä ja että molemmat verkoissa rajapinta.

Kun kyseessä DMZ raporttien luomisen, on monia erilaisia DMZ-rakenteita, kuten peräkkäin, Rau verkkoon, usean hallittava ja muita.

On suositeltavaa kaikki osallistujista käyttöönotoissa voit parantaa verkon suojaustasoa Azure-resursseissa DMZ käyttöönotto.

Lisätietoja DMZs ja miten voit ottaa käyttöön Azure-tietokannassa, tutustu artikkeliin [Microsoftin pilvipalveluihin ja verkkosuojaus](../best-practices-network-security.md).

## <a name="avoid-exposure-to-the-internet-with-dedicated-wan-links"></a>Vältä Internet-yhteys on erillinen WAN linkkejä näyttäminen
Monet organisaatiot valinnut Hybrid IT-reititystä. Hybrid IT-osa yrityksen tiedot-resurssit ovat Azure-muiden paikallisen olon aikana. Monissa tapauksissa jotkin osat palvelun toimii Azure muiden osien paikallisen olon aikana.

Hybrid IT-skenaario on yleensä jonkin paikallisen-yhteys. Tämä rajat paikallisen yhteyden sallii yrityksen paikallisen niiden verkkojen muodostaa Azure Virtual verkkoihin. Käytettävissä on kaksi paikallisen-yhteyksien ratkaisua:

- Sivuston sivuston VPN
- ExpressRoute

[Sivuston sivuston VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) edustaa VPN-yhteys paikalliseen verkkoon ja Virtual Azure-verkon välillä. Yhteys tapahtuu Internetin välityksellä ja mahdollistaa "tunnelin" tietoja verkko- ja Azure salattuja linkityksen. Sivuston sivuston VPN on suojattu, kehittynyt tekniikka, joka on otettu käyttöön kaiken kokoisille yritysten vuosikymmeniä. Tunnelin salaus suoritetaan [IP](https://technet.microsoft.com/library/cc786385.aspx)-tunnelitilassa.

Sivuston sivuston VPN ollessa luotettujen, luotettava ja aiemmin luotuja tekniikka liikenne tunnelin käy läpi Internetissä. Lisäksi kaistanleveys on rajoitettu suhteellisen enintään 200Mbps tietoja.

Jos asetat poikkeuksellisissa taso tai suorituskyvyn parantamiseksi paikallisen-yhteyksien, on suositeltavaa, että käytät Azure ExpressRoute paikallisen-yhteys. ExpressRoute on erillinen WAN paikallisen sijainnissa tai Exchange-palveluntarjoajan välisen linkin. Koska telco yhteyden, tietojen ei matka Internetin välityksellä ja näin ollen on eikä julkaista mahdollisia riskejä Internet-tietoliikenne.

Lisätietoja Azure ExpressRoute toiminta ja ottamisesta käyttöön, tutustu artikkeliin [ExpressRoute teknisiä tietoja](../expressroute/expressroute-introduction.md).

## <a name="optimize-uptime-and-performance"></a>Optimoi käytettävyyttä ja suorituskyky
Luottamuksellisuuden, eheyden ja käytettävyyden (CIA) sisältää kuluvan päivän eniten vaikutusvaltaista suojausmalli triad. Luottamuksellisuutta on tietoja salauksesta ja tietosuoja-eheys on varmistaa, että tiedot eivät muutu luvattoman henkilöstön ja käytettävyyden käsittelee varmistamalla, että henkilöt voivat käyttää niitä on oikeus käyttää tietoja. Virhe, jos jokin näistä alueista edustaa mahdolliset sopimusrikkomusta suojauksen.

Käytettävyys voidaan ajatella siten, että käytettävyyttä ja suorituskykyä. Palvelu ei ole käytettävissä, jos tietoja ei voi käyttää. Jos suorituskyky on heikko niin että tiedot käyttökelvoton, emme harkitse ei voi käyttää tietoja. Tämän vuoksi suojaus, näkökulmasta tarvitsemme tekemään, joista emme voi varmista, että palveluiden on paras mahdollinen käytettävyyttä ja suorituskyky
Suositut ja tehokas tapa, jolla voit parantaa käytettävyyttä ja suorituskykyä on käyttää kuormituksen tasaamisen. Keinoja niiden jakaminen verkkoliikennettä palvelun osana olevat palvelinten kuormituksen tasaamisen on. Esimerkiksi jos sinulla on edusta-WWW-palvelimien osana palvelussa, voit tehdä kuormituksen tasaamisen jakaa useita edusta-WWW-palvelimien liikenteen.

Liikenne jakelu kasvaa käytettävyys, koska web-palvelimia ei ole käytettävissä, jos kuormituksen halua liikenne lähettää tähän palvelimeen ja liikenne uudelleenohjataan palvelimet, joilla online-tilassa. Kuormituksen tasaamisen avulla suorituskykyä, koska suorittimen, verkon ja muistin katseltavan että tarjoillaan pyynnöt jaetaan kaikki kuormituksen saapuva palvelimiin.

On suositeltavaa, että voit käyttää kuormituksen tasaamisen mahdollisuuksien mukaan, ja tarvittaessa palvelujen. Olemme osoitteen tarkoituksenmukaisuutta seuraavissa osissa.
Azure Virtual Network tasolla Azure tarjoaa käyttöösi kolme ensisijaista ladata vastatilin asetukset:

- HTTP-pohjaista kuormituksen hallinta
- Ulkoiset kuormituksen
- Sisäinen kuormituksen

## <a name="http-based-load-balancing"></a>HTTP-pohjaista kuormituksen hallinta
HTTP-pohjaista kuormituksen tasaamisen asettaa päätöksiä mitä, johon haluat lähettää yhteyden ominaisuudet HTTP-protokolla. Azure on HTTP-kuormituksen, joka johtaa sovelluksen yhdyskäytävän nimi.

On suositeltavaa, että olet us Azure sovelluksen yhdyskäytävän kun:

- Sovellukset, jotka edellyttävät pyynnöt sama käyttäjä/asiakkaan istunnosta saman taustatietokantaan virtuaalikoneen saavuttamiseksi. Esimerkkejä tämä ostokset ostoskorin sovellukset ja web sähköpostipalvelimet.
- Sovellukset, jotka haluat vapauttaa web server klustereihin SSL tilauksen katseltavan-sovelluksen yhdyskäytävän [SSL purku](https://f5.com/glossary/ssl-offloading) ominaisuus hyödyntämistä.
- Sovellusten, kuten sisällön toimittamisen verkossa, jotka edellyttävät samassa pitkään suoritettavien TCP-yhteydessä reititetään tai ladata useita pyyntöjen saapuva eri taustatietokantaan-palvelimiin.

Lisätietoja Azure sovelluksen yhdyskäytävän toiminta ja miten voit käyttää sitä käyttöönoton, tutustu artikkeliin [Sovelluksen yhdyskäytävän yleiskatsaus](../application-gateway/application-gateway-introduction.md).

## <a name="external-load-balancing"></a>Ulkoinen kuormituksen
Ulkoinen kuormituksen tasaamisen tapahtuu, kun Internet-yhteyksiä ovat kuormitus tasataan Azure Virtual verkossa sijaitsevat palvelinten välillä. Azure ulkoisen kuormituksen voi antaa sinulle tätä ominaisuutta ja suosittelemme, että käytät sitä ei edellytä muistilappuja istuntoon tai SSL purku.

Toisin kuin HTTP-pohjaista kuormituksen tasaamisen, ulkoisen ladata tasaustoiminto käyttää tietoja verkko- ja kerrokset verkko verkkosegmenttiä päätösten mitä lataamaan saldo yhteys palvelimeen.

On suositeltavaa, että käytät ulkoisia kuormituksen tasaamisen aina, kun käytössäsi on [tilattomien sovellusten](http://whatis.techtarget.com/definition/stateless-app) hyväksymällä pyynnöt Internetistä.

Saat lisätietoja Azure ulkoisen kuormituksen toiminnasta ja miten voit ottaa käyttöön, varmista, lue artikkeli [aloittaminen luominen Internet vastakkaisten kuormituksen resurssien hallinta PowerShellin avulla](../load-balancer/load-balancer-get-started-internet-arm-ps.md).

## <a name="internal-load-balancing"></a>Sisäinen kuormituksen
Sisäinen kuormituksen tasaamisen muistuttaa ulkoisen kuormituksen tasaamisen ja käyttää samalla lataamaan saldo yhteydet taakse-palvelimiin. Ainoa ero on se, että kuormituksen tällöin hyväksyy näennäiskoneiden, jotka eivät ole Internet-yhteydet. Useimmissa tapauksissa yhteydet, jotka ovat hyväksyneet, kuormituksen tasaamisen aloitetaan Virtual Azure-verkko-laitteisiin.

On suositeltavaa, että käytät tilanteita, joissa tämä ominaisuus, kuten milloin haluat ladata saldo yhteydet SQL Server tai sisäinen WWW-palvelimien hyötyvät sisäinen kuormituksen.

Lisätietoja Azure sisäinen kuormituksen tasaamisen toiminta ja miten voit ottaa sen käyttöön, tutustu artikkeliin [aloittaminen luominen sisäinen kuormituksen, PowerShellin avulla](../load-balancer/load-balancer-get-started-internet-arm-ps.md#update-an-existing-load-balancer).

## <a name="use-global-load-balancing"></a>Käytä yleistä kuormituksen tasaamisen
Julkinen cloud tietojenkäsittely tekee mahdollista ottamaan yleisesti jaettu sovelluksia, jotka sijaitsevat palvelinkeskusten kaikkialla maailmassa osia. Tämä on mahdollista Microsoft Azure Azure on yleinen palvelinkeskuksen tavoitettavuuden vuoksi. Toisin kuin technologies mainittiin kuormituksen yleinen kuormituksen mahdollistaa sellaisten nopeuttavat palveluja on käytettävissä myös silloin, kun koko palvelinkeskusten saattavat olla poissa käytöstä.

Saat tämän tyyppistä yleinen kuormituksen tasaamisen Azure-tietokannassa mukaan hyödyntämistä [Azure liikenteen hallinta](https://azure.microsoft.com/documentation/services/traffic-manager/). Liikenteen hallinta tekee voi ladata saldo yhteydet palvelujen käyttäjän sijainnin perusteella.

Esimerkiksi jos käyttäjä on tehnyt EU: N palvelun pyyntö, yhteys ohjataan palvelujen sijaitsee EU-palvelinkeskukseen. Tässä osassa on liikenteen hallinta yleinen ladata Vastatilin avulla voit parantaa suorituskykyä, koska yhteyden muodostaminen lähimpään palvelinkeskukseen on suurempi kuin palvelinkeskusten, jotka ovat kaukana muodostamisesta.

Käytettävyys-puolen yleinen kuormituksen tasaamisen varmistetaan, että palvelu on käytettävissä, vaikka koko palvelinkeskuksen on nyt käytettävissä.

Esimerkiksi jos Azure palvelinkeskuksen olisi käytettävissä ympäristön syistä tai katkokset (kuten alueellisen verkon virhettä)-palvelun yhteydet on ohjattu porttiin lähin online Palvelinkeskus. Tätä yleisen kuormituksen tasaamisen on otettava hyödyntämistä DNS-käytäntöjä, joilla voit luoda liikenteen hallinta.

On suositeltavaa, että käytät liikenteen hallinta minkä tahansa cloud ratkaisu kehität, yleisesti eri aikajaksoille alue on yli useiden alueiden ja edellyttää käytettävyyttä mahdollista ylin taso.

Lisätietoja Azure liikenteen hallinta ja miten se otetaan käyttöön, lue artikkeli [Mikä on liikenteen hallinta](../traffic-manager/traffic-manager-overview.md).

## <a name="disable-rdpssh-access-to-azure-virtual-machines"></a>Poistaa käytöstä Azuren näennäiskoneiden RDP/SSH
On mahdollista saavuttaa Azuren näennäiskoneiden [Remote Desktop Protocol](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) (RDP) ja [Suojattu runko](https://en.wikipedia.org/wiki/Secure_Shell) (SSH)-protokollaa. Protokollat mahdollista hallita näennäiskoneiden remote sijainneista ja ovat vakio-palvelinkeskuksen pilvitekniikan.

Mahdollisen tietoturvaongelman käyttämällä protokollat Internetin välityksellä on, että hyökkääjät voi käyttää eri [palvelinten](https://en.wikipedia.org/wiki/Brute-force_attack) avulla saat käyttöösi, Azuren näennäiskoneiden. Kun käytetä kovin laajasti käyttöösi, ne käyttää käynnistys-kohdan virtuaalikoneen vaarantavat Azure Virtual verkossa muiden laitteiden tai hyökätä jopa verkkolaitteiden Azure ulkopuolella.

Tästä syystä suosittelemme, että poistat RDP ja SSH tutustuminen oman Azuren näennäiskoneiden Internetistä. Suora RDP- ja SSH käytön Internetistä on poistettu käytöstä, kun sinulla on muita vaihtoehtoja, voit käyttää näitä näennäiskoneiden etähallinnan:

- Pisteen sivuston VPN
- Sivuston sivuston VPN
- ExpressRoute

[Piste-,-sivusto VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md) on toisen termin asiakkaan ja palvelimen VPN-yhteyttä etäkäyttöä varten. Pisteen sivuston VPN mahdollistaa yksittäisen käyttäjän muodostaminen näennäisen Azure-verkon Internetin välityksellä. Kun piste sivuston-yhteys on muodostettu, käyttäjä voi muodostaa minkä tahansa näennäiskoneiden Azure Virtual verkkosijainnin, pisteen sivuston VPN-verkon kautta liitetyn käyttäjän RDP tai SSH avulla. Tämä oletetaan, että käyttäjän sallitaan näiden näennäiskoneiden saavuttamiseksi.

Pisteen sivuston VPN on paremmin suojattu kuin RDP- tai SSH suorat yhteydet, koska käyttäjällä on tarkistamiseen kahdesti, ennen kuin muodostat yhteyden virtual machine. Ensin käyttäjä on todennetaan (ja sallittua) kohdassa sivuston VPN-yhteyden; toinen käyttäjä on todennusta (ja sallittua) muodostaa RDP tai SSH istunto.

[Sivuston sivuston VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) muodostaa yhteyden toiseen verkkoon koko verkko Internetin välityksellä. Voit sivuston sivuston VPN muodostaa verkon paikallisen Virtual Azure-verkkoon. Jos sivusto sivusto VPN-käyttäjien käyttöön ottamiseksi paikallisen verkon voivat muodostaa yhteyden näennäiskoneiden Azure Virtual verkossa RDP- tai SSH-protokollan avulla sivuston sivuston VPN-yhteyden ja ei edellytä, että sallit RDP- tai SSH tutustuminen Internetin välityksellä.

Voit käyttää myös erillinen WAN-linkin samalla tavalla kuin sivuston sivuston VPN-toimintoja. Tärkein ero on 1. erillinen WAN-linkkiä ei käy läpi, Internet- ja 2. erillinen WAN linkit ovat yleensä useita vakaata ja performant. Azure on erillinen WAN-linkin ratkaista [ExpressRoute](https://azure.microsoft.com/documentation/services/expressroute/)muodossa.

## <a name="enable-azure-security-center"></a>Ota käyttöön Azure Tietoturvakeskus
Azure Tietoturvakeskus auttaa estämään, haku- ja uhkien vastaaminen ja on lisätty näkyvyys- ja päättää, Azure resurssien suojaus. Siinä on integroitu suojaus seuranta ja käytäntöjen hallinta Azure-tilauksissa, auttaa tunnistamaan uhkien, jotka muuten huomaamatta ja laaja valikoima security-ratkaisujen mediatyökaluja toimii.

Azure Tietoturvakeskus avulla voit optimoida ja seurata verkon suojausta:

- Tarjoaa verkon suojausta koskevia suosituksia
- Verkon suojausasetukset tilan seuranta
- Päätepisteen ja verkon tasoilla uhkien verkosta sekä varoittaa

On suositeltavaa, että otat Azure Tietoturvakeskus kaikkien Azure ominaisuuksissa.

Lisätietoja Azure Tietoturvakeskus ja kuinka se otetaan käyttöön oman käyttöönotoissa, tutustu artikkeliin [Azure Tietoturvakeskus esittely](../security-center/security-center-intro.md).

## <a name="securely-extend-your-datacenter-into-azure"></a>Laajentaa suojatusti Azure oman palvelinkeskukseen
Monta yrityksen IT organisaatiot näyttöä laajentaaksesi sen sijaan, että niiden paikallisen palvelinkeskusten kasvava pilveen kyselyjä. Tämä laajentamista edustaa tunniste on olemassa olevan IT-infrastruktuurin julkisen pilveen kyselyjä. Mukaan hyödyntämistä paikallisen-yhteyden asetukset ei käsittele Azure Virtual lopettaminen vain toisen aliverkon paikallisen verkkoinfrastruktuuria käyttöön.

On kuitenkin suunnittelun ja rakenteen ongelmat, jotka on osoitettu ensin paljon. Tämä on erityisen tärkeää verkkosuojaus-alueella. Paras tapa ymmärtää, kuinka voit esitellä tällaisessa rakenteessa on esimerkki.

Microsoft on luonut [Palvelinkeskuksen tunniste viittaus arkkitehtuurikaavio](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84#content) ja tukeminen oheismateriaalin voi auttaa sinua ymmärtämään, miltä palvelinkeskuksen laajentamisesta näyttää. Tämä on esimerkki viittaus toteutus, jonka avulla voit suunnitella suojatun enterprise-palvelinkeskuksen tunniste pilveen. On suositeltavaa, että luet suojatun ratkaisun pääosat verrata saat tämän asiakirjan.

Lisätietoja oman palvelinkeskuksen jatkuvan suojatusti Azure Näytä video [Laajentaminen Your Microsoft Azure Palvelinkeskukseen](https://www.youtube.com/watch?v=Th1oQQCb2KA).
