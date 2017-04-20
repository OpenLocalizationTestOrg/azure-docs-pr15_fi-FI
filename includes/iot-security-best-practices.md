# <a name="internet-of-things-security-best-practices"></a>Asioita Internet-suojauksen parhaiden käytäntöjen

Secure Internet, asiat (IoT) infrastruktuuri vaatii tiukkaa security puolustautumisen strategiaa. Tämä strategia edellyttää, että suojaa tiedot solmuryhmässä, suojaa kuljetuksessa olevien tietojen eheyden julkisen Internetin kautta ja valmistella laitteita turvallisesti. Kerroksen rakentaa suurempi security assurance yleistä infrastruktuurissa.

## <a name="secure-an-iot-infrastructure"></a>IoT-infrastruktuurin suojaaminen

Tätä suojauksen puolustautumisen strategiaa voidaan kehittää ja suorittaa aktiivista osallistumista valmistus, kehitys ja IoT laitteiden ja infrastruktuurin käyttöönottoa eri toimijoiden kanssa. Seuraavassa on näiden pelaajien korkean tason kuvaus.  

- **Laitteiston valmistaja/integraattori IoT**: IoT laite otetaan käyttöön, muutetaan laitteiston kokoonpano eri valmistajien tai toimittajat tarjoavat laitteiston IoT-käyttöönoton valmistettu tai muiden toimittajien integroidaan valmistajat ovat yleensä.
- **IoT ratkaisun kehittäjä**: IoT-ratkaisun kehittämistä tehdään yleensä ratkaisun kehittäjä. Tämä kehittäjä voi osa julkaisuja ryhmän tai system integrator (SI), erikoisjulkaisuihin tähän tehtävään. IoT ratkaisun kehittäjä voi kehittää eri osien IoT-ratkaisu alusta, integroida eri osien myynnissä tai avoimen lähdekoodin tai toteuttaa ratkaisuja valmiiksi vähäinen mukautus.
- **IoT ratkaisun deployer**: IoT jälkeen ratkaisu on kehitetty, se on otettava käyttöön-kenttään. Tämä edellyttää käyttöönoton laitteiston ja laitteiden yhteenliittämisen ja laitteiden ratkaisuja tai pilven käyttöönottoa.
- **IoT ratkaisun operaattori**: jälkeen IoT ratkaisu otetaan käyttöön, se edellyttää pitkän aikavälin toimien seuranta, päivitys ja ylläpito. Tämä voidaan tehdä itse kehitetyt työryhmä, johon kuuluvat tiedot teknologia-asiantuntijoiden, laitetoimintoja ja ylläpito ryhmiä ja toimialueen asiantuntijoiden, jotka seuraavat yleistä IoT infrastruktuurin oikean toiminnan.

Tapoja, joilla on kaikista näistä pelaajien kehittäminen, käyttöönotto ja käyttö suojattu IoT infrastruktuuri auttaa parhaita käytäntöjä.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT laitteiston valmistajan/integraattori

Seuraavassa on toimintaohjeita IoT laitteistovalmistajien ja laitteita muutetaan.

- **Alueen laitteiston vähimmäisvaatimukset**: laitteiston rakenne olisi sisällyttävä vähimmäisvaatimukset laitteisto, eikä mitään enemmän toiminnan tarvittavia ominaisuuksia. Esimerkki on USB-portit ovat vain, jos se on tarpeen laitteen toiminnan. Näitä lisäominaisuuksia, Avaa laitteen, joka olisi vältettävä ei-toivottuja hyökkäystavat.
- **Varmista, että laitteisto on ehkäistävä**: rakentaa-mekanismit fyysinen käsittelyltä, kuten laitteen kannen avaamista tai poistamalla osa laitteen tunnistamiseen. Nämä pystyisivät signaalit voidaan osittain ladattu pilveen, johon voi ilmoittaa näistä tapahtumista toimijoiden tietovirtaa.
- **Muodostaa suojatun laitteiston ympärille**: Jos MTK sallii, rakentaa suojausominaisuuksia kuten suojattu ja salattu tai käynnistys-Trusted Platform Module (TPM)-pohjaisia toimintoja. Näiden ominaisuuksien ansiosta laitteet suojattuina ja suojata yleistä IoT infrastruktuuri.
- **Varmista turvallinen päivitykset**: laitteen käyttöiän aikana Firmware-päivitykset ovat väistämättömiä. Muodostetaan suojattu polkuja laitteita päivityksiä ja salauksen assurance firmware-versioita avulla laite on suojattu aikana ja sen jälkeen päivityksiä.

## <a name="iot-solution-developer"></a>IoT ratkaisun kehittäjä

IoT ratkaisujen kehittäjät voivat hyödyntää hyviä toimintatapoja ovat seuraavat:

- **Seuraa suojattujen ohjelmistojen kehittämisen menetelmät**: suojattujen ohjelmistojen kehittäminen vaatii ajattelua maasta ylös tietoturvasta, alusta loppuun, jotta aivan sen toteutus, testaus ja käyttöönotto. Ympäristöissä, kieliä ja työkaluja vaihtoehtoja kaikki palvelutoiminta tämän menettelytavan kanssa. Microsoft Security Development Lifecycle tarjoaa turvallisen ohjelmiston rakentamisen vaiheittain.
- **Valitse avoimen lähdekoodin ohjelmistojen varoen**: avoimen lähdekoodin ohjelmisto tarjoaa mahdollisuuden kehittää nopeasti ratkaisuja. Kun olet valinnut avoimen lähdekoodin ohjelmisto, harkitse kunkin osan avoimen lähdekoodin yhteisön toiminnan tason. Aktiivinen yhteisö varmistaa, että ohjelmisto on tuettu ja, että ongelmat, jotka on löydetty ja osoitettu. Peitä ja passiivinen avoimen lähdekoodin ohjelmisto ei ehkä tue vaihtoehtoisesti ja ongelmat luultavasti eivät havaitse.
- **Integroida hoito**: useita ohjelmiston suojaus niitä olemassa reunan kirjastot ja API. Toiminnot, joita ei välttämättä vaadita nykyisen käyttöönotto voi silti olla saatavilla API-tason kautta. Tietoturvan varmistamiseksi Varmista, että kaikki komponentit on integroitu suojaus niitä varten-liittymiä.      

## <a name="iot-solution-deployer"></a>IoT ratkaisu deployer

IoT ratkaisun deployers parhaat käytännöt ovat seuraavat:

- **Käyttöönotto laitteet turvallisesti**: IoT käyttöönotoissa voi vaatia laitteiston käyttöönottoa suojaamaton paikoissa kuten yleisissä tiloissa tai alueilla varoitukseksi. Tällaisissa tilanteissa varmistaa, että laitteiston käyttöönoton väärentämiseltä pakottavista säännöksistä. Jos USB- tai muita portteja laitteessa käytettävissä, varmista, että niitä käsitellään turvallisesti. Monet hyökkäystavat avulla nämä pikakuvakkeet.
- **Pitää todennusta avaimet**: käyttöönoton aikana kukin laite edellyttää laitetunnukset ja siihen liittyvät pilvi-palvelun luomia todennusta avaimet. Pidä nämä avaimet fyysisesti turvallinen jopa käyttöönoton jälkeen. Kaikki selvittämäänsä avainta voi käyttää haittaohjelmien laitteen naamioitua aiemmin luodun tiedoston.

## <a name="iot-solution-operator"></a>IoT ratkaisun operaattori

Parhaat käytännöt IoT ratkaisun toimijoita ovat seuraavat:

- **Järjestelmän ajan tasalla**: Varmista, että laitteen käyttöjärjestelmän ja kaikki ohjaimet päivitetään uusimmiksi versioiksi. Jos poistat Automaattiset päivitykset Windows (IoT tai muita SKU) 10, Microsoft pitää se ajan tasalla, tarjoaa suojatun käyttöjärjestelmän IoT laitteille. Muut käyttöjärjestelmät (kuten Linux) pitäminen ajan tasalla auttaa varmistamaan, että ne myös suojataan hyökkäyksiä vastaan.
- **Suojaa haitallista toimintaa vastaan**: Jos käyttöjärjestelmä sallii, asentaa antivirus ja antimalware uusimpia ominaisuuksia kunkin laitteen käyttöjärjestelmän. Tämä voi auttaa välttämään kaikkein ulkoisilta uhilta. Useimmat nykyaikaiset käyttöjärjestelmät uhilta suojata ottamalla tarvittavat toimenpiteet.
- **Usein valvonta**: valvonta IoT infrastruktuuri tietoturvaan liittyviin ongelmiin on avain vastattaessa suojausongelmia. Useimmissa käyttöjärjestelmissä on sisäänrakennettu tapahtumaloki, joka olisi tarkistettava säännöllisesti varmistaaksesi, että tapahtunut ei ole suojauksen kannalta turvallista. Valvontatietojen keräämiseen voidaan lähettää erillisen kaukomittaus stream kuin pilvipalvelu jossa voidaan analysoida.
- **Suojata fyysisesti infrastruktuurin IoT**: huonoin tietoturvahyökkäykset IoT infrastruktuuria vastaan käynnistetyt laitteiden käytön avulla. Yksi tärkeä turvallisuuden käytäntö on haitallinen käyttö USB-porttiin ja käyttää muita fyysisesti vastaan. Fyysistä yhteyttä, kuten USB-portti käytössä yksi avain, joka on saattanut uncovering rikkomiseen on kirjaaminen lokiin. Uudelleen Windows-10 (IoT ja muut SKU-tietoja) antaa yksityiskohtaiset tapahtumien kirjaaminen.
- **Suojaa cloud tunnistetiedot**: Cloud todentamisen tunnistetietoja käytetään toimivien IoT-käyttöönotto ja määrittäminen ovat mahdollisesti helpoin tapa päästä ja IoT-järjestelmän. Suojaa tunnistetiedot vaihtamalla salasana usein ja pidättäytyä näitä tunnistetietoja käyttämällä julkisia koneita.

IoT kytkettyjen laitteiden vaihtoehdot vaihtelevat. Jotkin laitteet saattavat käyttää yhteisen työpöydän-käyttöjärjestelmiä käyttävät tietokoneet ja jotkin laitteet saattavat käynnissä erittäin Vaalea paino käyttöjärjestelmissä. Suojauksen parhaiden käytäntöjen kuvattu aiemmin ehkä vaihtelevassa näiden laitteiden sovellettavat. Jos, on noudatettava lisäksi suojaus- ja parhaita käytäntöjä näiden laitteiden valmistajilta.

Joitakin vanhoja ja rajoitettuihin laitteita ei ehkä on suunniteltu erityisesti IoT käyttöönottoa varten. Näitä laitteita ei ehkä ole mahdollisuutta salata tietoja, muodostaa yhteyden Internetiin tai tarjota kehittyneitä valvonta. Nykyaikaisen ja suojatun kentän yhdyskäytävä voi näissä tapauksissa kootaan tiedot vanhoista laitteista ja suojausta, tarvitaan näiden laitteiden yhteyden Internetin välityksellä. Kentän yhdyskäytävä voi antaa suojattu todennus, salattujen istuntojen neuvottelu, pilven, ja monet muut suojausominaisuudet komennot vastaanottamisesta.
