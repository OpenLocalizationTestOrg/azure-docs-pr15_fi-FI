<properties
    pageTitle="Sovelluksen Service-ympäristön määrittäminen | Microsoft Azure"
    description="Määritysten, hallinta ja sovelluksen palvelun ympäristöissä seuranta"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>


# <a name="configuring-an-app-service-environment"></a>Sovelluksen Service-ympäristön määrittäminen #

## <a name="overview"></a>Yleiskatsaus ##

Korkean tason Azure App palvelun ympäristössä kuuluu useita pääosaa:

- Laske App palvelun ympäristön nykyisessä palvelussa käynnissä olevat resurssit
- Tallennustilan
- Tietokannan
- Classic(V1) tai resurssin Manager(V2) Azure Virtual verkon (VNet) 
- Ei käytössä App palvelun ympäristön nykyisessä palvelussa aliverkon

### <a name="compute-resources"></a>Laske resurssit

Neljä resurssin jakavat käyttämällä Laske-resursseja.  Kunkin sovelluksen palvelun ympäristössä (Ietokannan) on joukko etupuoli päädyt ja kolme mahdollista työntekijä jakavat. Sinun ei tarvitse käyttää kaikkien kolmen työntekijän jakavat – Jos haluat, voit vain käyttää yhdellä tai kahdella.

Isännät-resurssin jakavat (eteen päättyy ja työntekijöiden) eivät ole käytettävissä suoraan vuokraajiin. Ei voi Remote Desktop Protocol (RDP) avulla voit muodostaa yhteyden niihin, muuttaa niiden valmistelu tai järjestelmänvalvoja ne toimivat.

Voit määrittää resurssin resurssivarantoon määrä ja koko. Sinulla Ietokannan neljä kokoa-painiketta, joka on merkitty P1 P4 kautta. Lisätietoja näiden kokojen ja niiden hinnat on artikkelissa [sovelluksen palvelun hinnat](../app-service/app-service-value-prop-what-is.md).
Määrä tai koon muuttaminen kutsutaan asteikko-toimintoa.  Vain yksi asteikko-toimintoa voi olla käynnissä kerrallaan.

**Eteen päättyy**: eteen päät ovat sovelluksia, pidetään yhteyttä Ietokannan HTTP/HTTPS-päätepisteet. Ei tee työmääriä etupuoli päättyy.

- Ietokannan alussa on kaksi P2s, joka on riittävä keskihajonta/testi toiminnoista ja alatason tuotannon toiminnoista. On suositeltavaa P3s, keskitaso, paksu tuotannon toiminnoista.
- Keskitaso, paksu tuotannon työmääriä Suosittelemme, että sinulla on vähintään neljä P3s varmistamiseksi on riittävä Suorita suunniteltu ylläpito toteutuessa eteen päättyy. Aikataulun mukainen ylläpitotoimet tuo mukanaan alaspäin yhden edusta kerrallaan. Tämä pienentää yleisesti saatavilla edusta kapasiteetin ylläpitotoimet aikana.
- Et voi lisätä uuden edusta esiintymän välittömästi. Ne voivat käyttää 2 ja 3 tuntia valmistelu.
- Saat lisätietoja asteikko tarkka määrittäminen, seurata suorittimen prosentteina, muistin prosentti ja aktiivisten pyyntöjen mittaukset edusta resurssivarantoon. Jos suorittimen ja muistin prosenttiosuudet ovat edellä 70 prosenttia, kun käyttöjärjestelmä P3s, lisätä Lisää etupuoli päättyy. Jos aktiivinen pyynnöt arvon keskiarvot 15 000 20 000 pyyntöihin kohti edusta-on lisättävä myös lisää etu päättyy. Tavoite on estää suorittimen ja muistin prosenttien alla 70 % ja aktiivisten pyyntöjen keskiarvon voit alla eteen 15 000 pyynnöt sen jälkeen, kun käytössäsi on P3s.  

**Työntekijöiden**: työntekijöiden ovat, jossa sovellus todella suorittaa. Skaalata App palvelun suunnitelmaa, joka käyttää työntekijöiden liittyvä työntekijä varannon ylöspäin.

- Et voi lisätä työntekijöiden välittömästi. Ne voi kestää 2, 3 tuntia valmistelu, riippumatta siitä, kuinka monta lisätään.
- Minkä tahansa ryhmän Laske resurssin kokoa skaalaus tulevat 2-3 tuntia kohti toimialue. Ietokannan on 20 päivityksen toimialueet. Jos skaalata työntekijä resurssivarantoon 10 esiintymät Laske kokoa, se voi kestää 20 – 30 tunteja välillä.
- Jos muutat Laske resurssit, joita käytetään työntekijä resurssivarantoon kokoa, aiheuttavat työntekijä kyseiseen resurssivarantoon käynnissä olevat sovellukset kylmän käynnistyy.

Nopein tapa työntekijä resurssivarantoon, joka ei ole käynnissä sovellusten Laske resurssin koon muuttaminen on:

- Skaalaa 0 esiintymän laskeminen alaspäin. Voit poistaa varauksen kopioita kestää noin 30 minuuttia.
- Valitse uusi Laske koko ja niiden esiintymien lukumäärän. Tässä näkymässä kestää välillä 2 ja 3 tuntia.

Jos sovellukset edellyttävät Laske resurssin suurentaminen, voit ei voi hyödyntää edellisen ohjeet. Sen sijaan, että työntekijä resurssivarantoon, joka isännöi kyseisissä sovelluksissa koon muuttaminen täyttää toisen työntekijän resurssivarantoon halutun kokoinen sovellusta ja Siirrä sovelluksia kyseiseen resurssivarantoon.

- Luo tarvittavat Laske koon muut esiintymät toisen työntekijän resurssivarantoon. Tämä 2-3 tuntia suorittaminen kestää.
- Määrittää sovelluksen palvelun suunnitelmia, joka isännöi sovellukset, jotka tarvitaan juuri asennetun työntekijä resurssivarantoon suurentaminen. Tämä on nopea toiminto, joka tekee saapuville alle minuutin.  
- Skaalata ensimmäisen työntekijä resurssivarantoon alaspäin, jos sinun ei tarvitse enää käyttämättömät tilanteita. Tämä toiminto kestää noin 30 minuuttia suorittamiseen.

**Automaattisen skaalauksen poistaminen**: yksi työkaluista, joiden avulla voit hallita Laske-resurssin kulutus on automaattisen skaalauksen poistaminen. Voit käyttää automaattisen skaalauksen poistaminen varten edusta- tai työntekijä jakavat. Voit tehdä asioita, kuten kasvu resurssivarantoon kaikenlaisia kopioita aamulla ja pienentää sen illalla. Tai ehkä voit lisätä esiintymät, kun työntekijä resurssivarantoon käytettävissä olevien työntekijöiden määrä vähenee tietyn rajan alapuolelle.

Jos haluat määrittää Automaattinen skaalaus sääntöjä ympärille Laske resurssin resurssivarantoon arvot ja valitse pitää mielessä aika, joka edellyttää valmistelu. Saat lisätietoja automaattisen skaalauksen poistaminen App palvelun ympäristöissä [määrittäminen Automaattinen skaalaus App palvelun ympäristössä][ASEAutoscale].

### <a name="storage"></a>Tallennustilan

Kunkin Ietokannan on määritetty 500 gt tallennustilaa. Tähän käytetään kaikissa Ietokannan kaikissa sovelluksissa. Tämä tallennustilaa Ietokannan osan ja tällä hetkellä ei voi vaihtaa käyttämään tallennustilaa. Jos teet muutoksia VPN reititys tai suojauksen, sinun on sallittava edelleen voi käyttää Azure-tallennustilaasi tai Ietokannan ei toimi.

### <a name="database"></a>Tietokannan

Tietokanta on tiedot, joka määrittää ympäristön sekä sovellukset, jotka ovat käynnissä sen sisältämät tiedot. Tämä on liian Azure pidetään-tilauksen osana. Se ei ole jotakin, mitä sinulla on suora mahdollisuus käsitellä. Jos teet muutoksia VPN reititys tai suojauksen, sinun on sallittava edelleen access SQL Azure--tai Ietokannan ei toimi.

### <a name="network"></a>Verkon

VNet, joka on käytössä oman Ietokannan voi olla jokin, kun olet luonut Ietokannan tai aiemmin tekemäsi etuajassa. Kun luot aliverkon Ietokannan luonnin aikana, pakottaa Ietokannan olevan virtual verkon samaan resurssiryhmään. Jos tarvitset oman Ietokannan käyttämä eroaa, että VNet resurssiryhmä, sinun on luoda oman Ietokannan resurssien hallinnan mallin avulla.

Tässä on virtual verkossa, jota käytetään Ietokannan rajoituksia:
 
- Virtuaalinen verkko on oltava alueellisen virtual verkkoon.
- Täyttäviä on aliverkon 8 tai useita osoitteita, jossa Ietokannan on otettu käyttöön.
- Kun aliverkon käytetään isännöimiseen Ietokannan, aliverkon osoite solualue ei voi muuttaa. Tästä syystä suosittelemme, että aliverkon sisältää vähintään 64 osoitteet tulevaa Ietokannan-tarvetta.
- Voi olla mitään muuta aliverkon, mutta Ietokannan.

Toisin kuin isännöityä palvelu, joka sisältää Ietokannan [VPN] [ virtualnetwork] ja aliverkon käyttäjän hallinnassa.  Voit hallita virtual verkostoasi Virtual verkko-Käyttöliittymän tai PowerShellin kautta.  Ietokannan voidaan ottaa käyttöön perinteinen tai resurssien hallinnan VNet.  Portaalin ja API kokemukset ovat hieman erilaiset perinteinen ja Resurssienhallinta VNets välillä, mutta Ietokannan-toiminto on sama.

VNet, jota käytetään isännöimiseen Ietokannan voit käyttää joko yksityisten RFC1918 IP-osoitteiden tai se voi käyttää julkiseen IP-osoitteet.  Jos haluat käyttää IP-alue, joka ei kuulu RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) on luotava VNet ja aliverkon käytettävän oman Ietokannan ennen Ietokannan luomisen jälkeen.

Koska tätä ominaisuutta sijoittaa Azure App palvelun mukaan virtual verkostoon, se tarkoittaa sitä, että sovellukset, joita isännöidään oman Ietokannan voidaan nyt käyttää resursseja, jotka ovat saatavilla ExpressRoute tai sivuston sivuston näennäinen yksityisverkko (VPN) kautta suoraan. Sovelluksen Service-ympäristössä olevia sovelluksia eivät tarvitse käyttää virtual verkossa, joka isännöi App Service-ympäristössä käytettävissä olevien resurssien verkko lisäominaisuuksia. Tämä tarkoittaa, että sinun ei tarvitse käyttämällä VNET integrointi tai Hybrid kautta pääset resurssien tekstimuodossa tai liittyä virtual verkkoon. Edelleen voit sekä näiden ominaisuuksien mutta käyttää resursseja verkoissa, joka ei ole yhteydessä virtual verkkoon.

Voit esimerkiksi virtual verkkoon, joka on tilaukseesi, mutta ei ole yhdistetty olevan oman Ietokannan virtual verkon integroida VNET integrointi. Voit käyttää silti myös Hybrid yhteydet access resursseille, jotka ovat samalla tavalla kuin tavallisesti muiden verkkojen.  

Jos sinulla on määritetty ExpressRoute VPN virtual verkon, sinun olisi otettava huomioon joistain reititys tarpeiden, jossa Ietokannan on. Jotkin käyttäjän määrittämä reitin (UDR)-määritykset, jotka eivät ole yhteensopivia Ietokannan on. Lisätietoja käynnissä Ietokannan virtual verkossa, ExpressRoute, jossa on [käynnissä sovelluksen Service-ympäristön virtual verkossa, jossa ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Saapuvan liikenteen suojaaminen

Kahdella ensisijainen ohjaamaan sitä, että Ietokannan saapuvan liikenteen.  Verkon suojauksen Groups(NSGs) avulla voit määrittää, mitä IP osoitteet voivat käyttää omaa Ietokannan seuraavassa kuvatulla tavalla [App palvelun ympäristössä saapuvan liikenteen hallinta](app-service-app-service-environment-control-inbound-traffic.md) ja voit myös määrittää oman Ietokannan sisäinen kuormituksen-Balancer(ILB) kanssa.  Nämä ominaisuudet voit myös käyttää yhdessä, jos haluat rajoittaa käyttämällä NSGs ILB Ietokannan.

Luodessasi Ietokannan se luo VIP oman VNet.  Kahdenlaisia VIP, sisäisiä ja ulkoisia.  Kun luot ulkoisen VIP Ietokannan sovellukset-kohdassa Ietokannan voi käyttää internet-reititettävä IP-osoitteen kautta. Kun valitset sisäinen oman Ietokannan määritetään ILB ja eivät ole suoraan internet käytettävissä.  ILB Ietokannan edellyttää edelleen ulkoisen VIP, mutta sitä käytetään vain Azure hallinta ja ylläpito käytön.  

ILB Ietokannan luonnin aikana antaa ILB Ietokannan käyttämä alitoimialueen ja hallita omia DNS: ää voit määrittää alitoimialue on.  Koska määritetty alitoimialueen nimen haluat myös käyttää HTTPS-käytön varmenteiden hallinta.  Ietokannan luonnin jälkeen sinua kehotetaan antamaan varmenne.  Saat lisätietoja luomisesta ja käyttämisestä ILB Ietokannan lukea [millä sisäinen kuormituksen, sovellus-palvelun ympäristössä][ILBASE]. 


## <a name="portal"></a>Portal

Voit hallita ja valvoa App Service-ympäristösi käyttämällä Käyttöliittymän Azure-portaalissa. Jos sinulla Ietokannan, valitse olet todennäköisesti Nähdäksesi oman sivupalkissa App Service-symboli. Tätä merkkiä käytetään esittämään App palvelun ympäristöissä Azure-portaalissa:

![Sovelluksen palvelun ympäristön symboli][1]

Avaa Käyttöliittymä, jossa on luettelo kaikista sovelluksen Service-ympäristössä, voit käyttää kuvaketta tai valitse nuolet (">" symboli), valitse sovelluksen palvelun ympäristöissä sivupalkki alareunassa. Valitsemalla jonkin seuraavista luettelossa ASEs Avaa Käyttöliittymän, jonka avulla seurata ja hallita sitä.

![Käyttöliittymän seuranta ja hallinta App Service-ympäristössä][2]

Tämä ensimmäinen sivu näyttää oman Ietokannan joitakin ominaisuuksia sekä metrisillä kaavion resurssivarannon kohden. Joitakin ominaisuuksia, jotka näkyvät **Essentials** -esto on myös hyperlinkkejä, jotka avaavat sivu, joka on liitetty sitä. Voit esimerkiksi valita Avaa virtual verkkoon, että Ietokannan suoritetaan liittyvän Käyttöliittymän **VPN** -nimi. **Sovelluksen palvelusopimusten vaihtoehdot** ja **sovellusten** kunkin Avaa terät, jotka näyttävät kohteita, jotka ovat oman Ietokannan.  

### <a name="monitoring"></a>Seuranta

Kaavioiden avulla voit nähdä erilaisia suorituskyvyn mittarit kunkin resurssivarannossa. Edustatietokannan resurssivarantoon voit valvoa keskimääräinen suorittimen ja muistin. Työntekijän jakavat voit valvoa määrä, jota käytetään ja määrä, joka on käytettävissä.

Suunnitelmien tehdä useita sovelluksen-palvelun käyttöä työntekijä resurssivarantoon olevien työntekijöiden. Työmäärää jaetaan ei samalla tavalla kuin edusta-palvelinten niin suorittimen ja muistin käyttö eivät ole kovin ehkäistä hyödyllisiä tietoja. On Lisää tärkeää seurata montako työntekijät, jotka olet käyttänyt ja ovat käytettävissä – etenkin silloin, kun hallitset järjestelmän muiden käytettäväksi.  

Voit käyttää kaikkia tietoja, joista voi seurata kaavioiden ilmoitusten määrittäminen. Tähän ilmoitusten määrittäminen toimii samalla muualla kuin sovellus-palvelussa. Voit määrittää ilmoituksen joko- **ilmoitukset** Käyttöliittymän osa tai kaikki arvot Käyttöliittymän kyselyjä siirtyminen ja valitsemalla **Lisää ilmoitus**.

![Arvot Käyttöliittymä][3]

Arvot, jotka olivat vain aiheena on sovelluksen palvelun ympäristön arvot. Saatavilla on myös arvot, jotka ovat käytettävissä sovelluksen palvelun suunnitelman tasolla. Tämä on missä suorittimen ja muistin seuranta on järkevää paljon.

Kaikki sovelluksen palvelusopimusten vaihtoehdot ovat Ietokannan erillinen App palvelusopimusten vaihtoehdot. Joka tarkoittaa, että vain sovellukset, jotka ovat käytössä kohdistettu isännät App palvelusopimus on sovellukset-sovelluksen, palvelusopimus. Sovelluksen palvelusopimus tiedot näkyviin Avaa sovelluksen palvelusopimus Ietokannan käyttöliittymässä luetteloita tai **Selaa App palvelusopimusten vaihtoehdot** (joka on lueteltu kaikki ne).   

### <a name="settings"></a>Asetukset

Sisällä Ietokannan-sivu on **asetukset** -osassa, joka sisältää useita tärkeitä ominaisuuksia:

**Asetukset** > **Ominaisuudet**: **asetukset** -sivu avautuu automaattisesti, kun voit noutaa Ietokannan-sivu. Ylimpänä on **Ominaisuudet**. Useilla tähän kohteet, jotka ovat tarpeettomia **Essentials**näkemääsi, mutta mikä on erittäin hyödyllinen on **Virtual IP-osoite**sekä **Lähtevien IP-osoitteet**.

![Ominaisuudet ja asetukset-sivu][4]

**Asetukset** > **IP-osoitteet**: Kun luot oman Ietokannan IP Secure Sockets Layer (SSL)-sovelluksen, sinun on IP SSL-osoite. Jotta voit noutaa jokin oman Ietokannan on IP SSL osoitteet, jotka se omistaa, jotka voidaan varata. Ietokannan luomisen jälkeen se on yksi IP SSL-osoite tähän tarkoitukseen, mutta voit lisätä Lisää. On varausta varten Lisää IP-SSL-osoitteet, [Sovellus-palvelun hinnat] mukaisesti[ AppServicePricing] (SSL-yhteyksien kohdassa). Lisää hinta on IP-SSL-hinta.

**Asetukset** > **Edustavarantoon** / **Työntekijä jakavat**: kaikkien näiden resurssien resurssivarantoon lavat tarjoaa mahdollisuuden saat tietoja vain kyseisen resurssivarannon lisäksi antamisen skaalata täysin kyseisen resurssivarannon ohjausobjektit.  

Kunkin resurssivarannon perus-sivu on kaavio, jossa on arvot, resurssivaranto. Aivan kuten Ietokannan-sivu-kaavioiden avulla voit kaaviossa siirryttävä ja määritä ilmoitukset haluamallasi tavalla. Sama kuin sen suorittamalla resurssivarannosta tekee asettaminen tietyn resurssivarannon Ietokannan-sivu-ilmoituksen. Sinulla on kaikki sovelluksia tai sovelluksen palvelun suunnitelmien työntekijä kannan käynnissä olevat työntekijä ryhmän **asetukset** -sivu.

![Työntekijän sovellussarjan asetukset Käyttöliittymä][5]

### <a name="portal-scale-capabilities"></a>Portaalin asteikko-ominaisuudet  

On kolme asteikko-toiminnot:

- IP-osoitteet Ietokannan-IP-SSL käyttö saatavilla olevat määrän muuttaminen.
- Laske resurssin, jota käytetään resurssivarannon koon muuttaminen.
- Laske-resursseja, joita käytetään resurssivarannon, joko manuaalisesti tai automaattisen skaalauksen poistaminen määrän muuttaminen.

-Portaalissa on ohjaamaan sitä, kuinka monta palvelimissa, jotka ovat resurssin jakavat kolmella tavalla:

- Mittakaava-toiminto-tärkeimmät Ietokannan sivu, yläreunassa. Voit tehdä useita asteikko määritysmuutoksia edusta- ja työntekijä jakavat. Ne kaikki käytetään kuin yhdellä kertaa.
- Manuaalinen asteikko-toimintoa, valitse yksittäinen resurssi resurssivarantoon **asteikko** -sivu, joka on **asetukset**-kohdassa.
- Automaattisen skaalauksen poistaminen, jossa voit määrittää yksittäisen resurssin resurssivarantoon **asteikko** -sivu.

Ietokannan sivu asteikko-toiminnon käyttöön liukusäädintä määrä ja Tallenna. Tämä Käyttöliittymä tukee myös koon muuttaminen.  

![Skaalaa Käyttöliittymä][6]

Jos haluat Manuaalinen tai Automaattinen skaalaus-ominaisuuksien käyttäminen tiettyä resurssivarannossa, valitse **asetukset** > **Edustavarantoon** / **Työntekijä jakavat** tarpeen mukaan. Avaa sitten ryhmän, jota haluat muuttaa. Valitse **asetukset** > **Mittakaava,** tai **asetukset** > **skaalata**. **Mittakaava,** sivu avulla voit hallita esiintymä määrä. **Asteikon ylöspäin** avulla voit määrittää resurssin koon.  

![Mittakaava-asetuksia Käyttöliittymä][7]

## <a name="fault-tolerance-considerations"></a>Vikasietoa huomioon otettavia seikkoja

Voit määrittää sovelluksen Service-ympäristön käyttämään enintään 55 yhteensä Laske resursseja. Kyseisten 55 Laske resurssien vain 50 voidaan host toiminnoista. Tämä on luokitellaan. On vähintään 2 edusta Laske resurssien.  Joka ei enintään 53 tukemaan työntekijä resurssivarantoon kohdistus. Jotta vikasietoa, tarvitset lisää Laske-resurssi, joka on kohdistettu seuraavien sääntöjen mukaisesti:

- Kunkin työntekijän varanto on vähintään 1 muita Laske resurssi, joka ei ole käytettävissä varataan työmäärä.
- Kun työntekijä varannon resursseja Laske määrä menee yläpuolella on tietty arvo, toinen Laske resurssi on pakollinen vikasietoa. Tämä ei ole edusta varannon kirjainkokoa.

Mikä tahansa yksittäisen työntekijän resurssivarantoon sisällä vikasietoa vaatimuksia, annetun arvon X työntekijä resurssivarantoon varattujen resurssien:

- Jos X on väliltä 2 – 20-käytettävä Laske resurssien avulla voit käyttää työmääriä määrä on X-1.
- Jos X on 21 – 40-voi käyttää Laske resurssien avulla voit käyttää työmääriä määrä on X-2.
- Jos X on väliltä 41 53, käyttää Laske resurssien avulla voit käyttää työmääriä määrä on X-3.

Pienin vaatiman tallennustilan on 2 edusta-palvelimiin ja 2 työntekijöiden.  Yllä lauseet ja sitten, seuraavassa on muutamia esimerkkejä selventää:  

- Jos sinulla on 30 työntekijöiden yhteen resurssivarantoon, 28 ne voidaan host toiminnoista.
- Jos sinulla on 2 työntekijöiden yhteen resurssivarantoon, 1 voidaan host toiminnoista.
- Jos sinulla on 20 työntekijöiden yhteen resurssivarantoon, 19 voidaan host toiminnoista.  
- Jos sinulla on 21 työntekijöiden yhteen resurssivarantoon, valitse edelleen vain 19 voidaan host toiminnoista.  

Vikasietoa kuvasuhde on tärkeä, mutta haluat ottaa huomioon, kun tietyt rajat edellä asteikkoa. Jos haluat lisätä lisää kapasiteettia siitä 20 esiintymät, siirry 22 tai sitä uudemmassa versiossa koska 21 ei lisätä minkä tahansa kapasiteetin. Sama koskee siirtymällä yli 40, missä seuraavan numero, joka lisää kapasiteetin 42.  

## <a name="deleting-an-app-service-environment"></a>Sovelluksen Service-ympäristössä poistaminen ##

Jos haluat poistaa sovelluksen Service-ympäristössä, valitse vain-toiminnolla **poistaminen** yläreunaan App palvelun ympäristön-sivu. Kun teet näin, sinua pyydetään vahvistamaan, että todella haluat tehdä tämän sovelluksen palvelun ympäristön nimi. Huomaa, että kun poistat sovelluksen Service-ympäristössä, poistat kaikki sen sisältö sekä.  

![Poista sovelluksen palvelun ympäristössä Käyttöliittymä][9]  

## <a name="getting-started"></a>Käytön aloittaminen

Aloita sovelluksen Service-ympäristössä, katso, [miten voit luoda sovelluksen palvelun ympäristön](app-service-web-how-to-create-an-app-service-environment.md).

Saat lisätietoja Azure App Service-ympäristö [Azure App palvelu](../app-service/app-service-value-prop-what-is.md).

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
