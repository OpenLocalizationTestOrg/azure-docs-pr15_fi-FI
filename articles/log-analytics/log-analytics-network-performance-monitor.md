<properties
    pageTitle="Verkon suorituskyvyn valvonta-ratkaisun OMS | Microsoft Azure"
    description="Verkon suorituskyvyn valvonta voit valvoa suorituskykyä oman verkot-lähellä real-aikainen – voit auttaa tunnistamaan ja suorituskyvyn kootussa."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Verkon suorituskyvyn valvonta (esikatselu)-ratkaisun OMS

>[AZURE.NOTE] Tämä on [Esikatselu-ratkaisun](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Tässä asiakirjassa kerrotaan, miten OMS, joiden avulla voit valvoa suorituskykyä oman verkot-lähellä real-kerran kohteeseen-haku- ja Etsi verkon suorituskyvyn valvonta-ratkaisun verkon käyttöönottoa varten ja suorituskyvyn pullonkaulojen. Verkon suorituskyvyn valvonta-ratkaisuun voit valvoa tappio ja viive aliverkosta tai palvelimia verkkojen välillä. Verkon suorituskyvyn valvonta tunnistaa verkko-ongelmista, kuten liikenne blackholing, reititys virheet ja ongelmat, jotka tavalliseen verkon seuranta menetelmät eivät pysty tunnistamaan. Verkon suorituskyvyn valvonta luo ilmoituksia, ja ilmoittaa ja silloin, kun verkko-linkki tulee vika raja-arvon. Nämä raja-arvot voidaan asiat automaattisesti järjestelmä tai voit määrittää niille mukautettuja ilmoituksen sääntöjä. Verkon suorituskyvyn valvonta varmistaa verkon suorituskykyongelmia ajoissa tunnistaminen ja localizes tietyn verkon segmenttiin tai laitteeseen ongelman lähde.

Voit tunnistaa verkko-ongelmien ratkaisun Raporttinäkymät-ikkunan joka tuo näkyviin yhteenvetotietoja verkossa, kuten viimeisimmät verkon kunto tapahtumat, perusasemassa verkon linkkejä ja subnetwork linkkejä, jotka jäävät hyvin pakettien ja viive. Voit voit porautuminen verkossa-linkkiä, voit tarkastella nykyinen kunto tila subnetwork linkkejä sekä solmu solmu linkit. Voit myös tarkastella historiallisten trendi tappio ja viive on verkossa, subnetwork ja solmu solmu taso. Voit tunnistaa lyhytkestoisia verkko tarkastelemalla historiallisten trendi kaavioiden pakettien ja viive ja verkon kootussa topologian kartalla. Vuorovaikutteinen topologian kaavio avulla voit visualisoida siirräntävälien siirräntävälien verkon tiet ja selvitä ongelma. Muita ratkaisuja, kuten luomalla mukautettuja raportteja tietojen keräämistä verkon suorituskyvyn valvonta perusteella lokitiedoston etsiminen eri analytics vaatimukset avulla.

Ratkaisu käyttää synteettisiä tapahtumia ensisijaiseksi paikaksi verkon virheet vianmäärityksen avulla. Voit käyttää sitä ilman huomioon tietyn verkkolaitteen toimittajan tai mallin. Se toimii paikallisen, cloud (IaaS) ja yhdistelmäympäristöjen. Ratkaisu löytää automaattisesti verkkotopologia ja eri reitittää verkossa.

Tyypillinen verkon seuranta tuotteiden keskittyä verkon laitteen (reitittimen, valitsimet jne.) tilan seurantaan, mutta eivät tarjoa havainnollistamisen verkkoyhteys välillä kahta pistettä, jota verkon suorituskyvyn valvonta ei todellisen laadun kyselyjä.

### <a name="using-the-solution-standalone"></a>Ratkaisu erillisen käyttäminen

Jos haluat seurata niiden kriittinen työmääriä välisten yhteyksien verkon laatua, verkkojen, palvelinkeskusten tai office-sivustot ja valitse avulla verkon suorituskyvyn valvonta-ratkaisun erillisenä välinen yhteys kunnon valvonta:

- useita palvelinkeskusten tai office-sivustoja, jotka ovat yhteydessä verkkoon julkinen tai yksityinen
- Kriittinen työmääriä käynnissä olevat yrityssovellussarjan
- julkinen pilvipalveluihin, kuten Microsoft Azure tai sisältyy Amazon Web Services (AWS) ja paikallisen verkot, jos IaaS (AM) käytettävissä ja sinulla on määritetty sallimaan viestintä yhdyskäytävien paikallisen verkkojen ja cloud verkkojen välillä
- Azuren ja paikallisten verkkojen Express reitin käytettäessä

### <a name="using-the-solution-with-other-networking-tools"></a>Muut verkkotyökalut ratkaisun käyttäminen

Jos haluat seurata viivan business-sovelluksen, voit käyttää verkon suorituskyvyn valvonta-ratkaisun avustaja-ratkaisun muiden verkko-työkalut. Verkkoyhteys on hidas voi aiheuttaa hidas sovellukset ja verkon suorituskyvyn valvonta auttaa sinua, jotka aiheutuvat pohjana ongelmien sovelluksen suorituskykyongelmien vianmääritys. Koska ratkaisu ei edellytä verkkolaitteiden kaikki access-sovelluksen järjestelmänvalvojan ei tarvitse verkko ryhmä on tietoja siitä, miten verkon vaikuttavia sovellukset ovat riippuvaisia.

Myös, jos olet jo sijoittamaan muita verkon Valvontatyökalut-ratkaisun voit täydentämällä sitten Työkalut koska eniten perinteisiä verkon seuranta ratkaisuja ei ole tietoja kyselyjä lopusta loppuun-verkon suorituskyvyn mittarit, kuten tappio ja viive.  Verkon suorituskyvyn valvonta-ratkaisun avulla täyttö, väli.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Asentaminen ja määrittäminen agenttien ratkaisu

Asenna tekijöiden [Log Analytics yhteyden Windows-tietokoneiden](log-analytics-windows-agents.md) ja [Muodostaa Operations Manager Log Analytics](log-analytics-om-agents.md)basic-prosessien avulla.

>[AZURE.NOTE]
Sinun on asennettava vähintään 2 agenttien vuoksi, jotta löydät ja seurata verkon resurssit tietoja. Muussa tapauksessa ratkaisun säilyy määrittäminen-tilaan, kunnes Asenna ja määritä muut agenttien vuoksi.

### <a name="where-to-install-the-agents"></a>Minne niistä

Ennen kuin asennat agenttien vuoksi, harkitse topologian verkon ja mitä verkko-osat, joita haluat seurata. On suositeltavaa, että asennat useampi kuin yksi edustaja kunkin aliverkon, jota haluat seurata. Toisin sanoen jokaisen aliverkon, jota haluat seurata, valitse palvelimia tai VMs ja asentaa ne agentti.

Jos et ole varma verkon topologian, asenna niistä palvelimiin kriittinen työmääriä, johon haluat lisätä verkon suorituskyvyn kanssa. Voit esimerkiksi haluta seurantaan verkkopalvelimen ja SQL Server-palvelimen välinen verkkoyhteys. Tässä esimerkissä olet asentanut agentti sekä palvelimiin.

Tekijöiden seurata verkkoyhteyden (linkkien) isäntien--ei itse isäntien välillä. Jotta seurannassa verkkolinkin agenttien vuoksi täytyy asentaa molemmat päätepisteet linkkiä.

### <a name="configure-agents"></a>Käyttäjäagenttien määrittäminen

Kun olet asentanut agenttien vuoksi, sinun on Avaa palomuurin porttien tietokoneisiin varmistaa, että agenttien vuoksi viestiä. Sinun on ladattava ja suorita järjestelmänvalvojan oikeuksin PowerShell-ikkunassa [EnableRules.ps1 PowerShell-komentosarjaa](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) ilman parametreja

Komentosarja luo rekisteriavaimia vaatii verkon suorituskyvyn valvonnan ja luo Windowsin palomuurisäännöt sallimaan tekijöiden luoda TCP-yhteyksiä keskenään. Komentosarjan luoma rekisteriavaimia Määritä, kirjaudu virheenkorjaus-lokit ja lokit-tiedoston polku. Se myös määrittää TCP portin agentti viestintään. Arvot painettavat näppäimet määritetään automaattisesti komentosarjaa, jotta et olisi muuttaa painettavat näppäimet manuaalisesti.

Portti avataan oletusarvoisesti on 8084. Voit käyttää mukautettuja porttia antamalla parametrin `portNumber` komentosarjan. Kuitenkin samaa porttia käytetään kaikissa tietokoneissa missä komentosarja suoritetaan.

>[AZURE.NOTE] EnableRules.ps1 komentosarja määrittää Windowsin palomuurisäännöt vain siinä tietokoneessa, jossa komentosarja suoritetaan. Jos sinulla on verkossa on palomuuri, olisi varmistat, että se sallii verkon suorituskyvyn tarkkailu käyttää porttinumeroa tarkoitettu liikenne.


## <a name="configuring-the-solution"></a>Ratkaisun määrittäminen

Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

1. Verkon suorituskyvyn valvonta-ratkaisun hankkii tiedot tietokoneiden Windows Server 2008: n SP 1 tai uudempi tai Windows 7 SP1 tai uudempi, jotka ovat samat vaatimukset kuin Microsoft seuranta agentti (MMA).
2. Lisää verkon suorituskyvyn valvonta-ratkaisun OMS työtilan käyttämällä [Lisää Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md)kuvatut toimet.  
  ![Verkon suorituskyvyn valvonta-symboli](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. OMS-portaaliin näet **Verkon suorituskyvyn valvonta** liittyvä *ratkaisu edellyttää lisämäärityksiä*ilmoitus uusi ruutu. Tarvitset lisää verkkojen subnetworks ja solmujen, joka löytyy mukaan tekijöiden perusteella ratkaisun määrittäminen. Valitse Aloita oletusarvon verkon määrittäminen **Verkon suorituskyvyn valvonta** .  
  ![ratkaisu edellyttää lisämäärityksiä](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>Määritä ratkaisun oletusarvo-verkoston kanssa

Määritys-sivulla näet yhden verkoston nimeltä **Oletus**. Kun et ole määrittänyt verkkoja, kaikki automaattisesti havaitun aliverkosta sijoitetaan verkko.

Aina, kun luot verkkoon, voit lisätä aliverkon ja kyseisen aliverkon poistetaan oletusarvon verkosta. Jos poistat verkkoon, sen aliverkosta palautetaan oletusarvo-verkkoon.

Toisin sanoen verkko on kaikki, jotka eivät sisälly käyttäjäkohtainen verkon aliverkosta säilö. Et voi muokata tai poistaa verkko. Se pysyy aina järjestelmässä. Voit kuitenkin luoda niin monta verkkojen kuin on tarpeen.

Useimmissa tapauksissa organisaation aliverkosta järjestetään useita verkossa ja sinun on luotava ryhmitellä loogisesti oman aliverkosta yhdessä tai useassa verkossa.

### <a name="create-new-networks"></a>Luo uusia verkkoja

Verkon verkon suorituskyvyn valvonta on aliverkosta säilö. Voit luoda verkon nimistä ja lisää aliverkosta verkkoon. Esimerkiksi voit luoda *Building1* nimeltä verkon ja lisää sitten aliverkosta, tai voit luoda *DMZ* nimeltä verkon ja lisää sitten kaikki aliverkosta kuuluvat demilitarized vyöhykkeen verkoston.

#### <a name="to-create-a-new-network"></a>Voit luoda uuden verkon

1. Valitse **Lisää verkko** ja kirjoita verkkonimi ja kuvaus.
2.  Valitse vähintään yksi aliverkosta, ja valitse sitten **Lisää**.
3. Valitse Tallenna määritykset **Tallenna** .  
  ![Lisää verkko](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Tietojen koostaminen Odota

Kun olet tallentanut määrityskohde ensimmäistä kertaa, ratkaisun käynnistyy verkon paketin tappio ja viive tietojen kerääminen solmujen, johon on asennettu agenttien vuoksi. Tämä prosessi voi kestää jonkin aikaa, joskus 30 minuuttia. Tässä tilassa aikana yleiskatsaus-sivulla verkon suorituskyvyn valvonta-ruudussa näkyy viestissä kerrotaan *tietojen yhdistämisen aikana*.

![tietojen koonti käynnissä](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Kun tiedot on ladattu, näet verkon suorituskyvyn valvonta päivittää ruutu näkyy tiedot.

![Verkon suorituskyvyn valvonta-ruutu](./media/log-analytics-network-performance-monitor/npm-tile.png)

Napsauta ruutua, voit tarkastella verkon suorituskyvyn valvonta Raporttinäkymät-ikkunan.

![Verkon suorituskyvyn valvonta Raporttinäkymät-ikkunan](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Muokkaa aliverkosta valvonta-asetukset

**Subnetworks** -välilehden määritys-sivulla näkyvät kaikki aliverkosta, jossa on vähintään yksi agentti on asennettu.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Ottaa käyttöön tai poistaa tietyn subnetworks valvonta

1. Valitse tai poista **subnetwork tunnus** vieressä olevaan ruutuun ja varmistaa, että **Seuranta käyttäminen** on valittu tai valittuna, se. Voit valita tai poistaa useita aliverkosta. Kun käytöstä, subnetworks ei valvottu, kuten niistä päivitetään lopettavat ping muiden tekijöiden.
2. Valitse solmut, jonka haluat valvoa tietyn subnetwork valitsemalla subnetwork luettelosta ja siirtämällä tarvittavat solmut sisältävä lähetetä ja valvottu solmujen luetteloruutujen väliin.
Voit lisätä mukautetun **kuvaus** subnetwork, jos haluat.
3. Valitse Tallenna määritykset **Tallenna** .  
  ![Muokkaa aliverkon](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Valitse solmujen seurannassa

Kaikki solmut, joissa on asennettu agentti on lueteltu **solmut** -välilehti.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Ottaa käyttöön tai poistaa solmujen seuranta

1. Valitse tai poista solmut, jota haluat seurata tai Pysäytä seuranta.
2. Valitse **Seuranta-käyttöä**tai poistaa sen valinnan tarpeen mukaan.
3. Valitse **Tallenna**.  
  ![solmun seurannan ottaminen käyttöön](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Seurannan sääntöjen määrittäminen

Verkon suorituskyvyn valvonta luo kunto tapahtumien tietoja solmujen tai linkkejä, subnetwork tai verkossa kynnysarvo tulee vika kahdet väliset yhteydet. Nämä raja-arvot voidaan asiat automaattisesti järjestelmä tai voit määrittää ne mukautettuja ilmoitusten sääntöjä.

*Oletusarvoinen sääntö* on luotu järjestelmä ja luo kunto-tapahtuman aina, kun tappio tai kahdet verkkoja tai subnetwork välinen viive linkit ilmeisesti järjestelmä havaitsi raja-arvon. Voit valita oletusarvon säännön käytöstä ja mukautetun seurantaa sääntöjen luominen

#### <a name="to-create-custom-monitoring-rules"></a>Voit luoda mukautettuja seurantaa sääntöjä

1. Napsauta **Lisää sääntö** **Näyttö** -välilehti ja kirjoita säännön nimi ja kuvaus.
2. Valitse verkko- tai subnetwork linkit seurannassa luetteloista pari.
3. Valitse verkko, jossa ensimmäinen subnetwork/s halutut sisältyy verkon avattavasta valikosta ja valitse sitten avattavasta valikosta vastaavan subnetwork subnetwork/s.
Valitse **kaikki subnetworks** , jos haluat seurata kaikkia subnetworks verkossa-linkkiä. Valitse samalla tavalla kuin muiden subnetwork/s halutut. Ja valitsemalla **Lisää poikkeus** jättää tietyn subnetwork linkit tekemäsi valinnan seurantaa.
4. Jos et halua luoda kunto tapahtumia varten valitut kohteet, poista **Ota tämä sääntö koskee linkit terveyteen**.
5. Valitse seuranta ehdot.
Voit määrittää mukautetun raja-arvot kunto tapahtuman luontia varten kirjoittamalla raja-arvot. Aina, kun ehto arvo sen valitun kynnysarvo valitun verkon/subnetwork paria, terveys-tapahtuma luodaan.
6. Valitse Tallenna määritykset **Tallenna** .  
  ![seurannan mukautetun säännön luominen](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Tietojen kerääminen

Verkon suorituskyvyn valvonta käyttää TCP SYN-SYNACK-ACK handshake pakettien kerätä tappio ja viive tiedot ja traceroute käytetään myös topologian tiedot.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten tiedot kerätään verkon suorituskyvyn valvonta.

| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
| Windows |![Kyllä](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Kyllä](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Ei](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![Ei](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![Ei](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP istunnon kättelyt joka 5 tiedot lähetetään 3 minuutin välein |

Ratkaisu käytetään synteettisiä tapahtumia arvioida verkon kunto. OMS tekijöiden asennettu eri kohtaan toisiinsa verkon exchange TCP-paketit ja prosessin, lue kierron aika- ja paketin katoamisen, mahdollisesti. Säännöllisesti jokaiselle agentti suorittaa myös muiden tekijöiden voit hakea kaikki eri tiet, joka testataan verkon reitin jäljitys. Käytä näitä tietoja, niistä voivat päätellä verkon latenssin ja paketin tappio luvut. Testejä toistuvat viiden sekunnin välein ja tiedot yhdistetään kolmen minuutin ajan mukaan niistä ennen lataaminen OMS.

>[AZURE.NOTE] Vaikka agenttien vuoksi viestiä usein keskenään, ne eivät luo verkkoliikennettä paljon ollessa testejä. Tekijöiden luottavat vain TCP SYN-SYNACK-ACK handshake pakettien tappio ja viive--pakettien vaihdetaan tietoja määrittämiseen. Tämän prosessin aikana agenttien vuoksi keskenään vain tarvittaessa ja agentti viestintä topologian on optimoitu vähentää verkkoliikennettä.

## <a name="using-the-solution"></a>Ratkaisun avulla

Tässä osassa kerrotaan kaikki Raporttinäkymät-ikkunan funktioista ja niiden käyttämisestä.

### <a name="solution-overview-tile"></a>Ratkaisu yhteenveto-ruutu

Kun olet verkon suorituskyvyn valvonta-ratkaisun, valitse OMS sivulla ratkaisu-ruutu kuvataan lyhyesti verkon terveyden. Se näyttää rengaskaavio, jossa kunnossa ja perusasemassa subnetwork linkkien määrää. Kun valitset ruudun, Avaa ratkaisu Raporttinäkymät-ikkunan.

![Verkon suorituskyvyn valvonta-ruutu](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Verkon suorituskyvyn valvonta ratkaisu Raporttinäkymät-ikkunan

**Verkon yhteenveto** sivu näkyy sekä niiden suhteellinen koko verkostojen yhteenveto. Tämän jälkeen kokonaismäärän verkon linkkejä, aliverkon linkit ja polut näkyvät (polku kuuluu kaksi isäntää tekijöiden kanssa ja kaikkien niiden välillä siirräntävälien IP-osoitteet) järjestelmä ruudut.

**Ensimmäiset verkon kunto tapahtumat** -sivu sisältää luettelon viimeisin kunto tapahtuma-ja ilmoitusten järjestelmä-ja aika, koska tapahtuma on aktiivinen. Kuntotietojen tapahtuma tai ilmoitus luodaan aina, kun paketti katoamisen tai linkin verkko- tai subnetwork viive kynnysarvo.

**Ylhäältä perusasemassa verkon linkit** -sivu näyttää luettelon perusasemassa verkon linkeistä. Nämä ovat verkon linkkejä, joissa on vähintään yksi haitallinen tapahtuman niiden tällä hetkellä.

**Ensimmäiset Subnetwork linkit yksityiskohtien eniten** ja **Subnetwork linkin eniten viive** näiden näkyy pakettien yläreunan subnetwork linkit ja viive yläreunan subnetwork linkit. Odotusaika tai joitakin paketin tappion summa ehkä tule olettaa tiettyjen verkko-linkkejä. Näiden linkkien yläreunan kymmenen luetteloissa, mutta niitä ei merkitä perusasemassa.

**Yleiset kyselyt** -sivu sisältää hakukyselyt, joka noutaa raaka-tietojen valvominen suoraan verkkoon. Voit käyttää näitä kyselyiden pohjana omia mukautettuja raportteja varten kyselyjen luomiseen.

![Verkon suorituskyvyn valvonta Raporttinäkymät-ikkunan](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Alirakenteen syvyys

Voit valita eri linkit ratkaisu koontinäytössä voit siirtyä alaspäin tarkempaa halutut minkä tahansa alueelle. Esimerkiksi kun ilmoituksen tai näkyvät koontinäytössä perusasemassa verkossa-linkkiä, voit valita sen tutkia tarkemmin. Sinun otettava sivun, jossa näkyvät kaikki tietyn verkon linkin subnetwork linkit. Osaat subnetwork kunkin linkin tappio, viiveen ja terveyteen tilan tarkasteleminen ja nopeasti ongelman aiheuttavat Selvitä, mitä subnetwork linkkejä. Valitse **Näytä solmu linkit** , jotta saat näkyviin kaikki solmu perusasemassa aliverkon linkin. Voit sitten yksittäisiä solmu solmu linkkien tarkasteleminen ja perusasemassa solmu linkeistä.

Voit valita **Näytä topologian** tarkastelemaan siirräntävälien siirräntävälien topologian tiet lähde- ja solmujen välillä. Perusasemassa tiet tai siirräntävälien näkyvät punaisena niin, että voit nopeasti löytää tietyn verkko-osa ongelma.

![alirakenteen tiedot](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>SUUNTAUS-kaaviot

Taso, jonka jokaisessa alirakenteeseen, näet trendi tappio ja verkkolinkin viive. SUUNTAUS-kaaviot ovat käytettävissä myös Subnetwork ja solmu linkkejä. Voit muuttaa kaavion piirtää kaavion yläreunassa aika-ohjausobjektin avulla aikaväli.

Trendi pylväskaavioissa näkyvät verkkolinkin suorituskyvyn historiallisten näkökulmasta. Verkon asioita, jotka ovat lyhytkestoisia laatu ja on vaikea todellisen vain katsomalla verkon nykyisen tilan. Tämä johtuu siitä ongelmat voivat tuoda nopeasti ja katoavat, ennen kuin kaikki havaitsee vain, jos haluat myöhemmin uudelleen samanaikaisesti. Lyhytkestoisia ongelmia myös voi olla vaikeaa Järjestelmänvalvojat, koska ne ongelmat usein pinta kuin petollinen lisäykset sovellusten vasteaikaa, vaikka kaikki sovelluksen osat näkyvät toimii tietokoneessasi.

Voit tunnistaa tällaiset ongelmat helposti katsomalla kaavioon trendin-kohtaa, johon ongelma näkyy äkillinen piikin verkon latenssin tai paketin tappio.

![SUUNTAUS-kaavio](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Kohteen siirräntävälien topologian kartta

Verkon suorituskyvyn valvonta näyttää kohteen siirräntävälien topologian tiet kaksi solmujen vuorovaikutteinen topologian karttaan välillä. Voit tarkastella topologian kartan valitsemalla solmu-linkki ja valitsemalla sitten **Näytä topologian**. Voit myös tarkastella topologian kartan napsauttamalla **polut** ruutua koontinäytössä. Kun napsautat **polut** koontinäytössä, voit on solmut lähde- ja valitse vasemmalla olevasta ruudusta ja valitse sitten **Piirrä** esitystapa tiet kahden solmun välillä.

Topologian karttaan näkyy, kuinka monta tiet on kaksi solmujen – mitä tietojen pakettien kestää polkuja. Verkon suorituskyvyn pullonkaulojen on merkitty punaisella topologian kartassa. Voit etsiä viallinen verkkoyhteyden tai viallinen verkkolaitteen katsomalla punainen värillinen osat topologian kartassa.

Kun napsautat solmu tai Vie päälle topologian kartassa, näet FQDN ja IP-osoite esimerkiksi solmun ominaisuudet. Valitse kohteen, se on IP-osoite. Voit korostaa tiettyä tiet valinnan ja valitsemalla sitten tiet, jonka haluat korostaa kartassa. Voit lähentäminen tai loitontaminen topologian kartan avulla hiiren kiekkopainiketta.

Huomaa, että näkyvissä määrityksen topologian on layer 3 topologian ei sisällä tason 2 laitteet ja yhteydet.

![kohteen siirräntävälien topologian kartta](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Vian sovellus on lokalisoitu

Verkon suorituskyvyn valvonta on löydä verkon pullonkaulojen ilman verkkolaitteet muodostamisesta. Joka kerää verkosta ja lisäämällä Lisäasetukset algoritmit verkon Graphissa tietojen perusteella, verkon suorituskyvyn valvonta tekee verkko-osat, jotka todennäköisesti probabilistic arvio ongelman lähde.

Tätä tapaa kannattaa käyttää verkon pullonkaulojen milloin siirräntävälien pääsy ei ole käytettävissä, koska se ei edellytä mitään tietoja kerännyt verkko-laitteilla, kuten reitittimen tai valitsimet. Tästä on hyötyä myös silloin, kun kaksi solmujen välillä siirräntävälien eivät ole hallinnollisesta valvonnasta. Esimerkiksi siirräntävälien voi olla Internet-Palveluntarjoajan reitittimen.

### <a name="log-analytics-search"></a>Kirjaudu Analytics-haku

Kaikki tiedot, jotka ovat graafisesti näkyviä verkon suorituskyvyn valvonta Raporttinäkymät-ikkunan kautta ja alirakenteen sivuilla on saatavilla myös grafiikkatiedostomuotoja Log Analytics haku. Voit kyselyn tiedot käyttämällä Etsi-kyselykieltä ja luomalla mukautettuja raportteja viemällä tiedot Exceliin tai PowerBI. Koontinäytön **Yleiset kyselyt** -sivu on joitakin hyödyllisiä kyselyitä, jotka voit käyttää lähtökohtana omia kyselyjen ja raporttien luomiseen.

![hakukyselyt](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>Tutkia kunto-ilmoituksen pääkansio

Nyt kun olet lukenut tietoja verkon suorituskyvyn valvonta-katsotaan yksinkertainen tutkimuksen pääkansion syy kunto tapahtuman kyselyjä.

1. Valitse sivulla saat tilannevedoksen nopeasti verkon kunto tarkkailemalla **Verkon suorituskyvyn valvonta** -ruutu. Huomaa, että ulos seurataan 80 subnetworks linkkejä, 43 on perusasemassa. Tämä takaa tutkintaa varten. Napsauta ruutua, voit tarkastella ratkaisun Raporttinäkymät-ikkunan.  
  ![Verkon suorituskyvyn valvonta-ruutu](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. Esimerkki alla olevassa kuvassa huomaat, että on tällä hetkellä 4 kunto-tapahtumien ja 4 verkon linkkejä, jotka ovat perusasemassa. Päätät ongelman tutkia ja ongelma ylimmällä kieliominaisuuksien **Sharepoint-Web-** verkko-linkkiä.  
  ![Esimerkki perusasemassa verkon linkki](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. Alirakenteen-sivulla näkyvät kaikki subnetwork linkit **Sharepoint-Web** -verkko-linkkiä. Huomaat, että molemmat subnetwork linkkien, viive on pystyyn tekeminen verkkoyhteys perusasemassa raja-arvon. Voit myös tarkastella subnetwork-linkkien viive-kehitystä. Voit käyttää aika-valinnan ohjausobjektin tarvittavat aikavälin keskittyminen kaaviossa. Näet kellonaika, jolloin viive on muutettu sen Huippu. Voit hakea myöhemmin lokit ajanjaksolle tutkia ongelman. Valitse **Näytä solmun linkit** alirakenteen edelleen.  
  ![perusasemassa aliverkon linkit Esimerkki](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Kaltaisilta edelliselle sivulle tietyn subnetwork linkin alirakenteen-sivu näyttää alaspäin sen tuotteen solmu linkit. Voit suorittaa vastaavat toimet tähän samoin kuin edellisessä vaiheessa. Valitse **Näytä topologian** tarkastelemaan topologian 2 solmujen välillä.  
  ![perusasemassa solmu linkit Esimerkki](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. Kaikki polut 2 valitun solmujen välillä piirretään topologian määrityksen. Voit havainnollistaa siirräntävälien siirräntävälien topologian tiet kaksi solmujen topologian kartassa välillä. Tällä tavalla saat selkeän kuvan kaksi solmut ja mitä välillä on montako ohjaavat tietojen pakettien kestää polut. Verkon suorituskyvyn pullonkaulojen on merkitty punaisella. Voit etsiä viallinen verkkoyhteyden tai viallinen verkkolaitteen katsomalla punainen värillinen osat topologian kartassa.  
  ![perusasemassa topologian Näytä esimerkki](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. Poistetun, viiveen ja kunkin polun siirräntävälien määrä tarkistaa **Polku tiedot** -ruudussa. Tässä esimerkissä näet, että on 3 perusasemassa polut mainitut-ruudussa. Vierityspalkki avulla voit tarkastella perusasemassa polut.  Valitse jokin polut niin, että vain yhden polun topologian piirretään valintaruudut avulla. Voit hiiren kiekkopainiketta lähentäminen tai loitontaminen topologian kartan.

  Valitse kuvan alta selkeästi näet verkon tiettyyn osaan ongelmakohdat pääkansio-syy katsomalla polkujen ja punaisella siirräntävälien. Solmun topologian määrityksen valitsemalla paljastaa solmun täydellinen toimialuenimi, kuten ominaisuudet ja IP-osoite. Valitsemalla kohteen näyttää pisteen IP-osoite.  
  ![perusasemassa topologian - polku tiedot Esimerkki](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Etsi lokit](log-analytics-log-searches.md) tarkastella yksityiskohtaisia verkon suorituskyvyn tietueita.
