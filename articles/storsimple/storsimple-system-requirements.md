<properties
   pageTitle="StorSimple järjestelmävaatimukset | Microsoft Azure"
   description="Tässä artikkelissa kuvataan ohjelmiston, verkko- ja suuren käytettävyyden vaatimukset ja parhaita käytäntöjä Microsoft Azure StorSimple ratkaista."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/31/2016"
   ms.author="alkohli"/>

# <a name="storsimple-software-high-availability-and-networking-requirements"></a>StorSimple ohjelmiston ja suuren käytettävyyden verkko vaatimukset

## <a name="overview"></a>Yleiskatsaus

Tervetuloa käyttämään Microsoft Azure StorSimple. Tässä artikkelissa kuvataan tärkeitä järjestelmävaatimukset ja parhaita käytäntöjä StorSimple laitteen ja tallennustilaa-asiakkaiden laitetta. On suositeltavaa, että luet tiedot huolellisesti ennen käyttöönoton StorSimple järjestelmän ja sitten takaisin siihen viitataan tarvittaessa käyttöönotto ja myöhempiä toiminnon aikana.

Järjestelmävaatimukset ovat seuraavat:

- **Tallennustilan asiakkaiden ohjelmistovaatimukset** - kuvataan tuetut käyttöjärjestelmät ja näiden käyttöjärjestelmien lisävaatimukset.
- **Vaatimukset StorSimple laitteen verkko** -, joka on oltava käynnissä sallimaan iSCSI, cloud tai liikenteen hallinta palomuurin porttien tietoja.
- **Suuren käytettävyyden vaatimukset StorSimple** - kuvataan suuren käytettävyyden vaatimukset ja parhaita käytäntöjä StorSimple laitteen ja host tietokoneeseen. 


## <a name="software-requirements-for-storage-clients"></a>Tallennustilan asiakkaiden ohjelmistovaatimukset

Tallennustilan-asiakkaille, jotka käyttävät StorSimple laitteen ovat seuraavat ohjelmistovaatimukset.

| Tuetut käyttöjärjestelmät | Vaadittava versio | Muut vaatimukset ja huomautukset |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012 2012R2 |StorSimple iSCSI tietomääristä tuetaan vain Windows levyn seuraavanlaisia käyttöön:<ul><li>Tavallinen asema basic levyllä</li><li>Perus- ja peilattu dynaamisen levyn asemaa</li></ul>Windows Server 2012 ohuen valmistelu ja ODX ominaisuudet ovat tuettuja, jos käytössäsi on StorSimple iSCSI-asema.<br><br>StorSimple voit luoda levinneet valmisteltu ja täysin valmistellun asemat. Se ei voi luoda osittain valmistellun asemat.<br><br>Levinneet valmistellun aseman alustaminen saattaa kestää kauan. Suosittelemme poistaminen äänenvoimakkuuden ja luot sitten uuden sijaan alustaminen. Jos haluan muuttaa aseman:<ul><li>Suorita seuraava komento ennen alustaa välttämiseksi tilaa regeneroitaviksi viiveet: <br>`fsutil behavior set disabledeletenotify 1`</br></li><li>Kun muotoilu on valmis, käytä seuraavaa komentoa tilan vapauttamiseen uudelleen:<br>`fsutil behavior set disabledeletenotify 0`</br></li><li>Windows Server 2012 korjaustiedoston kuvatulla tavalla [kt 2878635](https://support.microsoft.com/kb/2870270) Windows Server-tietokoneeseen.</li></ul></li></ul></ul> Jos olet määrittämässä StorSimple tilannevedoksen hallinta tai StorSimple sovittimen SharePoint, siirry [valinnaisia osia ohjelmistovaatimukset](#software-requirements-for-optional-components).|
| VMWare ESX | 5.5 ja 6.0 | Tuettuja kanssa VMWare vSphere iSCSI-asiakas. VAAI estäminen ominaisuutta tuetaan VMware vSphere StorSimple laitteiden kanssa.
| Linux RHEL/CentOS | 5, 6 ja 7 | Tuki Linux iSCSI asiakkaiden Avaa iSCSI käynnistäjä versioilla 5, 6 ja 7. |
| Linux | SUSE Linux 11 | |
 > [AZURE.NOTE] IBM AIX ei tällä hetkellä tueta StorSimple kanssa.

## <a name="software-requirements-for-optional-components"></a>Valinnaiset osat ohjelmistovaatimukset

Seuraavat ohjelmistovaatimukset ovat valinnaisia StorSimple osia (StorSimple tilannevedoksen hallinta ja StorSimple sovittimen SharePoint).

| Osa | Host (isäntä)-ympäristössä | Muut vaatimukset ja huomautukset |
| --------------------------- | ---------------- | ------------- |
| StorSimple tilannevedoksen hallinta | Windows Server 2008R2 SP1, 2012 2012R2 | Käytä StorSimple tilannevedoksen hallinta Windows Server tarvitaan peilattu dynaamisiksi varmuuskopiointia ja palauttamista ja sovelluksen yhdenmukaisia varmuuskopiot.<br> StorSimple tilannevedoksen hallinta tuetaan vain Windows Server 2008 R2 SP1 (64-bittinen), Windowsin 2012 R2: n ja Windows Server 2012.<ul><li>Jos käytössäsi on ikkunan Server 2012, .NET 3.5 – 4.5 on asennettava, ennen kuin asennat StorSimple tilannevedoksen hallinta.</li><li>Jos käytössäsi on Windows Server 2008 R2 SP1, asenna Windows Management Framework 3.0, ennen kuin asennat StorSimple tilannevedoksen hallinta.</li></ul> |
| StorSimple sovittimen SharePoint | Windows Server 2008R2 SP1, 2012 2012R2 |<ul><li>SharePoint-sovittimen StorSimple tuetaan vain SharePoint 2010: n ja SharePoint 2013: n.</li><li>RBS vaatii SQL Server Enterprise Edition-versiolla 2008 R2 tai 2012.</li></ul>|

## <a name="networking-requirements-for-your-storsimple-device"></a>StorSimple laitteen verkko-vaatimukset

StorSimple laite on lukittu laite. Portit on kuitenkin voi avata palomuurin siten, että iSCSI, cloud ja liikenteen hallinta. Seuraavassa taulukossa on lueteltu, joka on avattava palomuurin porttien. Tässä taulukossa *-* tai *Saapuvan* viittaa siihen suuntaan, josta asiakkaan pyynnöt käyttää laitteen. *Ulos* tai *Lähtevät* viittaa siihen suuntaan, jossa StorSimple laitteen lähettää tietoja ulkoisesti-käyttöönoton jälkeen: esimerkiksi lähtevän Internetiin.

| Portti nro<sup>1,2</sup> | Sisään tai ulos | Portti laajuus | Pakollinen | Huomautuksia |
|------------------------|-----------|------------|----------|-------|
|TCP 80 (HTTP)<sup>3</sup>|  Ulos |  WAN | Ei |<ul><li>Lähtevän portin käytetään Internet-yhteys Nouda päivitykset.</li><li>Lähtevän web-välityspalvelimen on määritettävä käyttäjä.</li><li>Jos järjestelmäpäivityksiä, portin myös täytyy avata ohjaimen kiinteä IP-osoitteet.</li></ul> |
|TCP 443 (HTTPS)<sup>3</sup>| Ulos | WAN | Kyllä |<ul><li>Lähtevän portin käytetään tietojen pilveen käyttäminen.</li><li>Lähtevän web-välityspalvelimen on määritettävä käyttäjä.</li><li>Jos järjestelmäpäivityksiä, portin myös täytyy avata ohjaimen kiinteä IP-osoitteet.</li><li>Tämä portti käytetään myös sekä ohjauskoneen muistista.</li></ul>|
|UDP 53 (DNS) | Ulos | WAN | Joissakin tapauksissa Katso huomautuksia. |Tämä portti on pakollinen vain, jos käytössäsi on Internet-pohjaisia DNS-palvelimeen. |
| UDP 123 (NTP) | Ulos | WAN | Joissakin tapauksissa Katso huomautuksia. |Tämä portti on pakollinen vain, jos käytössäsi on Internet-pohjaisten NTP-palvelimeen. |
| TCP 9354 | Ulos | WAN | Kyllä |Lähtevän portin käytetään StorSimple laitteen StorSimple hallintapalvelu yhteydessä. |
| 3260 (iSCSI) | Valitse | LÄHIVERKON | Ei | Tämä portti käytetään käyttämään tietoja iSCSI päälle.|
| 5985 | Valitse | LÄHIVERKON | Ei | Saapuvan portin käytetään StorSimple tilannevedoksen hallinnan avulla viestiä StorSimple-laitteen kanssa.<br>Tämä portti käytetään myös silloin, kun etäyhteyden muodostat yhteyden Windows PowerShellin StorSimple HTTP-yhteyden kautta. |
| 5986 | Valitse | LÄHIVERKON | Ei | Tämä portti käytetään, kun etäyhteyden muodostat yhteyden Windows PowerShellin StorSimple HTTPS-protokollan välityksellä. |

<sup>1</sup> ei ole saapuvien porttien on voi avata julkisen Internetissä.

<sup>2</sup> Jos useita portit Suorita yhdyskäytävän määritykset, reititetty liikenteen tilauksen määritetään seuraavalla tavalla [portin reititys](#routing-metric)-portin reititys järjestyksen mukaan.

<sup>3</sup> kiinteä IP-osoitteet StorSimple laitteen ohjauskoneen on oltava reititettävä ja voitava muodostaa yhteys Internetiin. Kiinteä IP-osoitteiden käytetään ylläpidon laitteeseen päivitykset. Jos laitteen ohjaimia ei voi muodostaa Internetin kautta kiinteä IP-osoitteet, voit voi päivittää StorSimple laitteen.

> [AZURE.IMPORTANT] Varmista, että palomuurin ei muokata tai purkaa SSL liikenteen StorSimple laitteen ja Azure välillä.

### <a name="url-patterns-for-firewall-rules"></a>URL-kuvioiden määrittäminen palomuurisäännöt

Verkoston järjestelmänvalvojat voivat määrittää usein Lisäasetukset palomuurisäännöt URL-osoite-kuvioiden avulla ja suodattaa saapuvan ja lähtevän tietoliikenteen perusteella. Laitteen StorSimple ja StorSimple hallintapalvelu määräytyvät muiden Microsoft-sovellusten, kuten Azure palvelun Bus, Azure Active Directory käyttöoikeuksien valvonta, tallennustilan tilit ja Microsoft Update-palvelimiin. Näiden sovellusten URL-Osoitteen kuviot voidaan määrittää palomuurisäännöt. On tärkeää ymmärtää, että nämä sovellukset liittyvä URL-Osoitteen kuviot voi muuttaa. Tämä puolestaan edellyttää verkon järjestelmänvalvoja voi valvoa ja Päivitä palomuurisäännöt kuin oman StorSimple ja tarvittaessa.

On suositeltavaa, että määrität palomuurisäännöt IP-osoitteet, kiinteä liberally useimmissa tapauksissa StorSimple lähtevän tietoliikenteen. Voit kuitenkin määrittää Lisäasetukset palomuurisäännöt, joita tarvitaan Luo suojatun ympäristöissä alla olevia tietoja.

> [AZURE.NOTE] Laitteen (lähde) IP-osoitteet aina arvoksi käytössä Verkkoliittymät. Kohde IP-osoitteet on asetettava [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).

#### <a name="url-patterns-for-azure-portal"></a>Azure-portaalin URL-Osoitteen kuviot
| URL-kaava                                                      | Osan/toiminnot                                           | Laitteen IP-osoitteet                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple hallinta<br>Access Control-palvelu<br>Azure palvelun Bus| Cloud käyttävä verkon liityntäkohdat        |
|`https://*.backup.windowsazure.com`|Laitteen rekisteröinti| Vain tiedot 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Sertifikaattien kumoaminen |Cloud käyttävä verkon liityntäkohdat |
| `https://*.core.windows.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-tallennustilan asiakkaat ja seuranta | Cloud käyttävä verkon liityntäkohdat        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-palvelimet<br>                             | Kiinteä IP-osoitteet vain ohjain               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Kiinteä IP-osoitteet vain ohjain   |
| `https://*.partners.extranet.microsoft.com/*`                    | Tuki-paketti                                                  | Cloud käyttävä verkon liityntäkohdat        |

#### <a name="url-patterns-for-azure-government-portal"></a>Azure Government-portaalin URL-Osoitteen kuviot
| URL-kaava                                                      | Osan/toiminnot                                           | Laitteen IP-osoitteet                           |
|------------------------------------------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `https://*.storsimple.windowsazure.us/*`<br>`https://*.accesscontrol.usgovcloudapi.net/*`<br>`https://*.servicebus.usgovcloudapi.net/*`   | StorSimple hallinta<br>Access Control-palvelu<br>Azure palvelun Bus| Cloud käyttävä verkon liityntäkohdat        |
|`https://*.backup.windowsazure.us`|Laitteen rekisteröinti| Vain tiedot 0|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Sertifikaattien kumoaminen |Cloud käyttävä verkon liityntäkohdat |
| `https://*.core.usgovcloudapi.net/*` <br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-tallennustilan asiakkaat ja seuranta | Cloud käyttävä verkon liityntäkohdat        |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-palvelimet<br>                             | Kiinteä IP-osoitteet vain ohjain               |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |Kiinteä IP-osoitteet vain ohjain   |
| `https://*.partners.extranet.microsoft.com/*`                    | Tuki-paketti                                                  | Cloud käyttävä verkon liityntäkohdat        |

### <a name="routing-metric"></a>Reititys metrijärjestelmä

Reititys arvo on liitetty liittymät ja yhdyskäytävään, joka reitittää tiedot määritetyn verkot. Reititys metrijärjestelmä käytetään reititys protokollan mukaan laskemiseen parhaat polku annetun kohteeseen, jos se saa tietoonsa eri poluista olemassa samaan kohteeseen. Alaosassa reititys metrijärjestelmä, mitä suurempi haluamasi asetuksen.

StorSimple yhteydessä, jos useita verkkoliittymät ja yhdyskäytäville on määritetty kanavan tietoliikenteen reititys arvot toimitetaan määrittääksesi suhteellinen järjestyksessä, jossa liittymät haluat käyttää toista kyselyjä. Käyttäjä ei voi muuttaa reititys arvot. Voit kuitenkin käyttää `Get-HcsRoutingTable` cmdlet-komento, voit tulostaa StorSimple laitteen reititys taulukosta (sekä arvot). Lisätietoja Get-HcsRoutingTable cmdlet-komennon [vianmääritys StorSimple käyttöönotto](storsimple-troubleshoot-deployment.md).

Reititys metrisillä algoritmit ovat erilaiset riippuen siitä käynnissä StorSimple laitteen versio.

**Versioiden ennen päivitys 1**

Tämä vaihtoehto sisältää ohjelmistoversiot ennen päivitystä 1 GA 0,1, kuten 0,2 tai 0,3-versiossa. Reititys-arvojen mukaisesti tilaus on seuraavanlainen:

   *Määritetty viimeksi 10 GbE verkkoliittymän > muut 10 GbE verkkoliittymän > määritetty viimeksi 1 GbE verkkoliittymän > muut 1 GbE verkko-käyttöliittymä*


**Versioiden päivitys 1 ja 2 päivitys ennen aloittamista**

Tämä sisältää ohjelmistoversiot, kuten 1, 1.1 tai 1.2. Reititys-arvojen mukaisesti tilaus on päättänyt seuraavasti:

   *TIETOJA 0 > määritetty viimeksi 10 GbE verkkoliittymän > muut 10 GbE verkkoliittymän > määritetty viimeksi 1 GbE verkkoliittymän > muut 1 GbE verkko-käyttöliittymä*

   Update-1 tietojen 0 reititys metrijärjestelmä tehdään pienin; Tämän vuoksi kaikki cloud-liikenne reititetään tietojen 0. Pane merkille tämä, jos määritettynä on useampi kuin yksi cloud käyttävä verkkoliittymän StorSimple laitteen.


**Versioiden lähtien Update 2**

Reititys arvot on muutettu ja Päivitä 2 on useita verkko liittyviä parannuksia. Ongelma voidaan selittää seuraavasti.

- Arvojoukon ennalta määritetyt Verkkoliittymät.   

- Harkitse myönnetyt eri verkkoliittymät, kun käytät cloud-arvoja tai cloud ei ole käytössä, mutta määritetyn yhdyskäytävän alla Esimerkki-taulukossa. Huomautus määritetty arvot ovat vain esimerkkiarvoja.


  	| Verkkokäyttöliittymä | Cloud käytössä | Cloud-käytöstä Gatewayn |
  	|-----|---------------|---------------------------|
  	| Tietoja 0  | 1            | -                        |
  	| Tietojen 1  | 2            | 20                       |
  	| Tietoja 2  | 3            | 30                       |
  	| Tietojen 3  | 4            | 40                       |
  	| Tietojen 4  | 5            | 50                       |
  	| Tietojen 5  | 6            | 60                       |


- Siinä järjestyksessä, jossa cloud-liikenne reititetään verkkoliittymät kautta on:

    *Tietojen 0 > tietojen 1 > päivämäärä 2 > 3 tiedot > tietojen 4 > tietojen 5*

    Tämä on selitetty seuraavan esimerkin mukaan.

    Harkitse StorSimple laitetta, jonka kaksi cloud käyttävä verkon liittymää tietojen 0 ja tietojen 5. Tietojen 1 – 4 tiedot ovat cloud ei ole käytössä, mutta on määritetty yhdyskäytävän. Siinä järjestyksessä, jossa liikenne reititetään laite on:

    *Tietoja 0 (1) > tietojen 5 (6) > tietojen 1 (20) > tietojen 2 (30) > tietojen 3 (40) > tietojen 4 (50)*

    *Jos luvut ovat sulkeissa osoittavat vastaaviin reititys arvot.*

    Jos tietojen 0 epäonnistuu, cloud liikenne reitittää tiedot 5. Koska, että yhdyskäytävä on määritetty muita verkon tietojen 0 ja tietojen 5 on epäonnistuu, cloud-liikenne voi siirtyä tietojen 1 kautta.


- Jos cloud käyttävä verkkoliittymän epäonnistuu, ovat 3 uudelleenyritykset 30 sekunnin viivettä muodostaa liittymän kanssa. Jos kaikki uudelleenyritykset epäonnistuu, liikenne reititetään liittymään seuraava käytettävissä cloud käyttävä reititys taulukon mukaisesti. Jos kaikki cloud käyttävä verkon liittymät fail-laitteen epäonnistuu päälle muiden ohjauskoneen (ei ole tässä tapauksessa uudelleenkäynnistys).

- Jos näkyvissä on VIP epäonnistui iSCSI käyttävä verkko-liittymän, ole 3 uudelleenyritykset 2 sekunnin viive kanssa. Tämä ongelma on pidetty sama aiempien versioiden. Jos iSCSI verkkoliittymät kaikki epäonnistuu, ohjain automaattisesti tapahtuu (mukana uudelleen).


- Ilmoituksen myös korotettuna StorSimple laitteessasi ilmetessä VIP epäonnistui. Jos haluat lisätietoja, siirry [ilmoituksen pikaopas](storsimple-manage-alerts.md).

- Uudelleenyritykset, myös iSCSI ohittavat pilveen.

    Seuraavassa on esimerkki: A StorSimple laitteessa on kaksi verkon liittymää käytössä, tietojen 0 ja tietojen 1. Tietojen 0 on cloud käyttävä Data 1 on sekä cloud ja iSCSI on otettu käyttöön. Ei ole muita verkkoliittymät tässä laitteessa on otettu käyttöön cloud tai iSCSI.

    Jos tietojen 1 epäonnistuu, se annetaan on viimeksi iSCSI verkon-liittymän, tuloksena ohjauskoneen automaattisesti tietoja 1 muiden ohjaimen.


### <a name="networking-best-practices"></a>Verkko-parhaat käytännöt

Kirjoita edellä verkko vaatimukset parhaan mahdollisen suorituskyvyn StorSimple-ratkaisun lisäksi noudattaa seuraavien parhaiden käytäntöjen:

- Varmista, että laitteen StorSimple on erillinen 40 Mbps kaistanleveyden (tai useamman) käytettävissä koko ajan. Tämä kaistanleveyden ei voi jakaa (tai kohdistus olisi voidaan taata käyttämällä QoS käytännöt) muiden sovellusten kanssa.

- Varmista Internet-verkkoyhteys on käytettävissä aina. Arviointi tai epäluotettavista laitteilla, myös muun, ilman Internet-yhteyttä, Internet-yhteydet johtaa ei tueta määritys.

- Eristämään iSCSI ja pilvi liikenne on varattu verkkoliittymät laitteen iSCSI ja cloud käytön mukaan. Lisätietoja on artikkelissa StorSimple laitteen [Muokkaa verkkoliittymät](storsimple-modify-device-config.md#modify-network-interfaces) .

- Älä käytä verkkoliittymät linkki kooste Control Protocol (LACP)-määritys. Tämä on ei-tuettu määritys.


## <a name="high-availability-requirements-for-storsimple"></a>Suuren käytettävyyden StorSimple vaatimukset

Laitteisto-ympäristö, joka sisältyy StorSimple ratkaisu on käytettävyyden ja luotettavuuden ominaisuuksia, jotka sisältävät on käytettävissä, vikasietoinen storage-infrastruktuurin oman joten. Välillä on kuitenkin vaatimukset ja parhaita käytäntöjä, jotka sinun on oltava sähköpostistandardien varmistaa, että StorSimple käytettävyys. Ennen kuin otat StorSimple, Tarkista huolellisesti seuraavat vaatimukset ja parhaita käytäntöjä StorSimple laitteen ja yhdistetty isäntä-tietokoneissa.

Lisätietoja seuranta ja ylläpito StorSimple laitteen laitteita Valitse [Käytä StorSimple hallintapalvelu seurannassa laitteet ja tila](storsimple-monitor-hardware-status.md) ja [StorSimple laitteiston osan korvaaminen](storsimple-hardware-component-replacement.md).

### <a name="high-availability-requirements-and-procedures-for-your-storsimple-device"></a>Suuren käytettävyyden vaatimukset ja toimintojen StorSimple laitteeseesi

Tarkista huolellisesti, jotta voit varmistaa StorSimple laitteen suuren käytettävyyden seuraavat tiedot.

#### <a name="pcms"></a>PCMs

StorSimple laitteita ovat tarpeettomia, kuuman-vaihdettavan laitepaikan power ja jäähdytys moduulit (PCMs). Kunkin PCM on riittävästi kapasiteettia koko alustan-palvelun tarjoamista varten. Suuren käytettävyyden, jotta molemmat PCMs on oltava asennettuna.

- Muodostaa yhteyttä PCMs eri power lähteistä antamaan käytettävyys, jos power lähteen epäonnistuu.
- Jos PCM epäonnistuu, pyytää korvaa heti.
- Poista epäonnistui PCM vain, jos sinulla on korvaaminen ja olet valmis, asenna se.
- Älä poista sekä PCMs samanaikaisesti. PCM-moduuli sisältää varmuuskopion varauksen moduuli. Poistaminen sekä PCMs aiheuttaa Sammuta ilman varauksen suojaus ja laitteen tila ei tallenneta. Lisätietoja varauksen Siirry [Säilytä varmuuskopion varauksen moduuli](storsimple-battery-replacement.md#maintain-the-backup-battery-module).

#### <a name="controller-modules"></a>Ohjaimen moduulit

StorSimple laitteita ovat tarpeettomia, kuuman-vaihdettavan laitepaikan ohjauskoneen moduulit. Ohjaimen moduulit toimivat Aktiivinen/passiivinen tavalla. Kerrallaan yksi ohjain moduuli on aktiivinen ja tarjoaa palvelun-aikana controller-moduulia ei ole käytössä. Passiivinen ohjauskoneen moduuli on virta ja on käytettävissä, jos aktiivinen ohjauskoneen moduulin epäonnistuu, tai se on poistettu. Kunkin ohjauskoneen moduulin on riittävästi kapasiteettia koko alustan-palvelun tarjoamista varten. Sekä ohjain moduulit on oltava asennettuna suuren käytettävyyden varmistamiseksi.

- Varmista, että sekä ohjain moduulit asennetaan aina.

- Jos ohjauskoneen moduuli epäonnistuu, pyydä korvaa heti.

- Poista epäonnistui ohjauskoneen moduuli vain, jos sinulla on korvaaminen ja olet valmis, asenna se. Moduulin poistamista varten pitkän vaikuttaa ilmavirralle ja näin ollen järjestelmän jäähdytys.

- Varmista, että verkkoyhteyksien sekä ohjain moduulit, ovat samat ja yhdistetty verkko-liittymät on identtiset verkko-määritys.

- Jos ohjauskoneen moduuli epäonnistuu tai korvaaminen, tarkista, että on controller-moduulin ennen korvaamista epäonnistui ohjauskoneen moduulin aktiivinen. Varmista, että valvonta on on aktiivinen, siirry [Määritä aktiivinen ohjauskoneen laitteen](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Älä poista sekä ohjain moduulit samanaikaisesti. Ohjauskoneen automaattisesti ollessa käynnissä Sammuta valmiustilassa controller-moduuli tai poistaa sen alustan.

- Valvonta-automaattisesti, kun odota vähintään viisi minuuttia, ennen kuin poistat joko ohjauskoneen moduuli.

#### <a name="network-interfaces"></a>Verkon liityntäkohdat

StorSimple laitteen ohjauskoneen moduulit kunkin on neljä 1 Gigabit ja kaksi 10 Gigabit Ethernet Verkkoliittymät.

- Varmistamalla, että verkkoyhteyksien sekä ohjain moduulit, samanlaiset ja verkon liittymät että ohjauskoneen moduulin liittymät yhteydessä on identtiset verkko-määritys.

- Jos mahdollista, ottaa käyttöön verkkoyhteyksien koko eri valitsimia, jotta palvelun saatavuus verkko Jos laitteen.

- Irrota ainoa tai viimeisen jäljellä iSCSI käyttävä interface (ja IP-osoitteet varattu), kun käytöstä liittymän ensin ja Irrota kaapeli. Jos liittymän on irrotettu ensimmäisen kerran, se aiheuttaa active ohjain ei onnistu passiivinen ohjauskoneen päälle. Passiivinen ohjaimella on myös sen vastaavan liityntäkohdat irrotettu, jos molemmat ohjaimet uudelleen ennen kuin yhden ohjaimen selvitetään useita kertoja.

- Muodosta yhteys verkkoon vähintään kaksi tietojen liittymät kunkin ohjauskoneen moduulista.

- Jos olet ottanut kahden 10 GbE liittymät käyttöön niitä eri valitsimet yli.

- Jos mahdollista, käytä MPIO palvelimissa, jotta palvelimet hyväksyt, linkki, verkossa tai LIP-virheen.

Lisätietoja verkko laitteesi suuren käytettävyyden ja suorituskyky, siirry [StorSimple 8100 laitteen asentamisesta](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) tai [StorSimple 8600 laitteen asentaminen](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

#### <a name="ssds-and-hdds"></a>SSD ja HDDs

StorSimple laitteita ovat muun muassa tasainen tilan levyjen (SSDs) ja kiintolevyaseman levyasemien (HDDs), joka on suojattu käyttäen peilikuvana välilyöntejä. Peilattu välilyöntejä käyttäminen varmistaa, että laite on sietävät SSD tai HDDs virheen.

- Varmista, että kaikki Suoritettaessa ja Kiintolevy moduulit on asennettu.

- Jos Suoritettaessa tai Kiintolevylle ei onnistu, pyydä korvaa heti.

- Jos Suoritettaessa tai Kiintolevylle epäonnistuu tai edellyttää, että korvaava, varmista, että Suoritettaessa tai Kiintolevylle, joka edellyttää korvaavan poistaminen.

- Älä poista useita Suoritettaessa tai Kiintolevylle milloin tahansa järjestelmästä samanaikaisesti.
Vähintään 2 levyjen tietynlaista (Kiintolevy, Suoritettaessa), virheen tai peräkkäisen ylitä lyhyt aika virhe saattaa johtaa järjestelmän vioittumisen ja mahdolliset tietojen menettämistä. Jos näin tapahtuu, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) järjestelmänvalvojalta.

- Aikana korvaava-seurata asemat SSD ja HDDs **ylläpito** -sivun **Laitteiston tila** . Vihreä valintamerkki-tila tarkoittaa, että levyjen kunnossa tai OK, valitse punainen huutomerkki ilmaisee epäonnistui Suoritettaessa tai Kiintolevylle.

- On suositeltavaa, että määrität cloud tilannevedosten kaikki asemat, jotka haluat suojata, jos järjestelmä epäonnistui.

#### <a name="ebod-enclosure"></a>EBOD kehys

StorSimple laitteen mallista 8600 on laajennettu Bunch, levyjen (EBOD) kehyksen lisäksi ensisijainen kehys. EBOD sisältää EBOD ohjaimet ja kiintolevyaseman levyasemien (HDDs), joka on suojattu käyttäen peilikuvana välilyöntejä. Peilattu välilyöntejä käyttäminen varmistaa, että laite on pysty vähintään yksi HDDs virheen Hyväksy. Ensisijainen kehyksen kautta tarpeettomat SAS kaapeli on yhdistetty EBOD kehys.

- Varmista, että molemmat EBOD kehyksen ohjauskoneen moduulit SAS kaapeli sekä kaikki kiintolevyaseman levyasemien asennetaan aina.

- Jos EBOD kehyksen controller-moduulia ei onnistu, pyydä korvaa heti.

- Jos EBOD kehyksen controller-moduulin epäonnistuu, varmista, että controller-moduulin aktivoituu, ennen kuin voit korvata moduulin. Varmista, että valvonta on on aktiivinen, siirry [Määritä aktiivinen ohjauskoneen laitteen](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device).

- Aikana EBOD ohjauskoneen moduulin korvaava-jatkuvasti valvoa StorSimple hallintapalvelu komponenttia tilan käyttäminen **ylläpito** > **laitteiston tila**.

- Jos SAS-kaapelin epäonnistuu tai edellyttää, että korvaava (Microsoft Support pitäisi olla toimenpiteitä tällaisten määrittämiseksi), varmista, että poistaa vain, joka edellyttää korvaavan SAS-kaapelia.

- Ei samanaikaisesti Poista SAS sekä kaapeli milloin tahansa järjestelmästä samanaikaisesti.

### <a name="high-availability-recommendations-for-your-host-computers"></a>Host ()-isäntätietokoneet suuren käytettävyyden koskevia suosituksia

Tarkista huolellisesti näiden parhaiden käytäntöjen varmistamiseksi suuren käytettävyyden isäntien StorSimple laitteen yhteydessä.

- Määritä StorSimple [kahden solmun tiedoston palvelimen klusterin määrityksiä][1]. Yksittäisen kohtiin järjestelmävirheiden ja muodostamassa redundancy host reunassa poistamalla koko ratkaisu on erittäin käytettävissä.

- Käytä jatkuvasti käytettävissä (CA) jakaa käytettävissä Windows Server 2012 (SMB 3.0) suuren tallennustilan ohjaimet automaattisesti. Saat lisätietoja tiedoston palvelimen klustereiden ja jatkuvasti käytettävissä jaettujen resurssien määrittäminen Windows Server 2012 viitata tässä [videossa esitellään](http://channel9.msdn.com/Events/IT-Camps/IT-Camps-On-Demand-Windows-Server-2012/DEMO-Continuously-Available-File-Shares).

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lue lisää StorSimple järjestelmän rajoitukset](storsimple-limits.md).
- [Lue, miten voit ottaa StorSimple ratkaisuja](storsimple-deployment-walkthrough-u2.md).

<!--Reference links-->
[1]: https://technet.microsoft.com/library/cc731844(v=WS.10).aspx
