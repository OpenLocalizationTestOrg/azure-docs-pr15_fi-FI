<properties 
    pageTitle="Pirstoutuneet MP4 live Azure Media-palveluiden ingest määrityksen | Microsoft Azure" 
    description="Määrityksen kuvataan protokolla ja erilaisille pirstoutuneet MP4 mukaan live streaming Microsoft Azure Media Services-muodossa. Microsoft Azure Media Services sisältää live streaming palvelua, joka sallii virtauttaa tapahtumia ja lähettää sisältöä reaaliajassa asiakkaita käyttämällä Microsoft Azure cloud-ympäristössä. Tässä asiakirjassa kerrotaan myös parhaat käytännöt muodostaminen erittäin tarpeettomat ja tehokkaat live ingest järjestelmiä." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"     
    ms.author="cenkdin;juliako"/>

#<a name="azure-media-services-fragmented-mp4-live-ingest-specification"></a>Pirstoutuneet MP4 live Azure Media-palveluiden ingest määritys

Määrityksen kuvataan protokolla ja erilaisille pirstoutuneet MP4 mukaan live streaming Microsoft Azure Media Services-muodossa. Microsoft Azure Media Services sisältää live streaming palvelua, joka sallii virtauttaa tapahtumia ja lähettää sisältöä reaaliajassa asiakkaita käyttämällä Microsoft Azure cloud-ympäristössä. Tässä asiakirjassa kerrotaan myös parhaat käytännöt muodostaminen erittäin tarpeettomat ja tehokkaat live ingest järjestelmiä.


##<a name="1-conformance-notation"></a>1. merkintätapaa poikkeamisen

Avainsanat "on", "Ei saa", "REQUIRED", "On", "On NOT", "SHOULD", "Pitäisi ei", "Suositus", "Voi" ja "Valinnainen" tässä asiakirjassa on tulkitaan kuvatulla tavalla RFC 2119.

##<a name="2-service-diagram"></a>2. kaavion palvelu 

Seuraavassa kuvassa näkyy Microsoft Azure Media Services korkean tason arkkitehtuuri live streaming-palvelu:

1.  Live Encoder Vie live syötteet kautta Microsoft Azure Media Services SDK kyselyjä kanavat, jotka on luotu ja valmistelun yhteydessä.
2.  Kanavien ja-ohjelmiin sekä Streaming Microsoft Azure Media Services kahvaa päätepisteen kaikki live streaming toimintoja ingest, muotoiluja, kuten cloud DVR, tietoturva, skaalattavuus ja redundanssin.
3.  Voit myös asiakkaat voi valita käyttöön CDN kerroksen virtautetun median päätepisteen ja asiakkaan päätepisteet välillä.
4.  Asiakkaan päätepisteet stream (kuten tasainen Streaming, VIIVAN, HDS tai HLS) HTTP mukautuvat Streaming protokollia käyttäen virtautetun median päätepisteestä.

![image1][image1]


##<a name="3-bit-stream-format--iso-14496-12-fragmented-mp4"></a>3. bittinen stream muodossa – ISO 14496 12 pirstoutuneet MP4

Lähetysmuodon on live streaming ingest, joka on kuvattu tämän asiakirjan perusteella [ISO-14496 – 12]. Lisätietoja [[MS-SSTR]](http://msdn.microsoft.com/library/ff469518.aspx) n videotilauspalveluiden tiedostot tarkan pirstoutuneet MP4-muodossa ja laajennukset ja live streaming nieltynä.

###<a name="live-ingest-format-definitions"></a>Ingest Live Muotoile-määritteitä 

Alla on luettelo erikoismuotoilua määritteitä, jotka koskevat live ingest tuominen Microsoft Azure Media Services:

1. 'Ftyp', LiveServerManifestBox ja 'moov'-ruudusta lähetetään sivupyynnön (HTTP POST) kanssa.  Se lähetetään virta alussa ja milloin tahansa encoder muodostaa jatkaminen virtaa ingest.  Katso kohta 6 [1] Lisätietoja.
2. [1] 3.3.2 kohdassa määrittää on valinnainen ruudun StreamManifestBox, jota kutsutaan live ingest. Microsoft Azure kuormituksen reititys logiikan vuoksi tähän ruutuun käyttö on poistettu ja valittavissa ei saa, kun ingesting tuominen Microsoft Azure Media-palvelun. Jos valintaruutu on käytettävissä, Azure Media Services äänettömästi ohittaa sen.
3. [1] 3.2.3.2 määritelty TrackFragmentExtendedHeaderBox on sijaittava jokaisen osan.
4. Version 2 TrackFragmentExtendedHeaderBox olisi voidaan käyttää Luo useita palvelinkeskusten media osia samanlaiset URL-osoitteisiin. Fragmentti index-kenttä on pakollinen rajat palvelinkeskuksen automaattisesti, indeksi-pohjainen streaming muotoilee kuten Apple HTTP Live Streaming (HLS) ja indeksi-pohjainen MPEG-VIIVA.  Usean palvelinkeskuksen automaattisesti käyttöön fragmentti indeksi on synkronoitava eri koodauslaitteista ja suurentaa välillä peräkkäiset media jokaisen osan 1 jopa monista encoder käynnistyy tai epäonnistuu.
5. [1] 3.3.6 osa määrittää kutsutaan MovieFragmentRandomAccessBox ("mfra"), joka voidaan lähettää live annosmuuntokertoimien lopussa etoksiryhmien välisiä SUHTEITA (pää-ja-muodossa) osoittamaan kanavan ruudun. Azure Media Services ingest-logiikkaa vuoksi etoksiryhmien välisiä SUHTEITA (pää-ja-muodossa) käyttö on poistettu ja ei saa erilaisille live 'mfra'-ruutuun lähetetään. Jos lähetettiin, Azure Media Services äänettömästi ohittaa sen. On suositeltavaa [Kanavan Palauta](https://msdn.microsoft.com/library/azure/dn783458.aspx#reset_channels) käyttämään nollaa ingest pisteen tila ja myös kannattaa lopettaa esityksen ja stream [Ohjelma lopettaa](https://msdn.microsoft.com/library/azure/dn783463.aspx#stop_programs) avulla.
6. MP4-fragmentti kesto on oltava vakio, pienentää asiakas-luettelot ja parantaa asiakasohjelman lataaminen heuristiikkaa toista tunnisteiden avulla.  Kesto voi lisävaluutta jotta muu kuin kokonaisluku kehyksen korvaukset hyvittää.
7. MP4-fragmentti kesto on oltava noin 2 ja 6 sekuntia.
8. MP4 fragmentti aikaleimat ja indeksit (TrackFragmentExtendedHeaderBox fragment_ absolute_ aika ja fragment_index) olisi saapuvat nousevassa järjestyksessä.  Azure Media Services on joustavat kaksoiskappaleiden osat, mutta se on erittäin rajoitettu mahdollisuus Järjestä Vaillinaiset lauseet media aikajanan mukaan.

##<a name="4-protocol-format--http"></a>4. protokolla muodossa – HTTP

ISO pirstoutuneet MP4 mukaan live ingest Microsoft Azure Media Services käyttää pitkään suoritetut HTTP POST standardi pyytää koodattu media tietojen pakattu pirstoutuneet MP4-muodossa-palveluun. HTTP jokaisen merkinnän lähettää valmis pirstoutuneet MP4 bittinen stream ("muodossa") alkaen alkaen ylätunnisteruutu ("ftyp", "Live palvelin luettelon ruutuun" ja "moov"-ruutu) ja jatkat järjestyksen osat ('moof' ja 'mdat' ruudut). Tutustu osan 9.2 in [1] HTTP POST-pyynnössä syntaksin URL-osoite. Esimerkki kirjaa URL-osoite on: 

    http://customer.channel.mediaservices.windows.net/ingest.isml/streams(720p)

###<a name="requirements"></a>Vaatimukset

Seuraavassa on yksityiskohtaiset vaatimukset:

1. Encoder pitäisi käynnistää lähetyksen lähettämällä HTTP POST-pyyntö, jonka tyhjä "tekstin" (nolla sisällön pituus) nieltynä URL-Osoitetta. Tämä voi auttaa tunnistamaan nopeasti Jos live nieltynä päätepisteen on kelvollinen ja ei mitään todennuksen tai muita ehtoja pakollinen. HTTP-protokolla kohti palvelimen voi lähettää takaisin HTTP-vastauksen, ennen kuin mukaan lukien viestin tekstiosaan koko pyyntö on vastaanotettu. Annetut live tapahtuma-ja ilman tässä vaiheessa pitkään käynnissä laatu encoder ei ehkä pysty tunnistamaan jokin virhe, ennen kuin se päättyy lähettäminen kaikki tiedot.
2. Encoder on käsiteltävä virheitä tai todennus haasteita tuloksena (1). Jos (1) onnistuu ja 200 vastaus Jatka.
3. Uusi kokouspyyntö HTTP POST alettava Encoder pirstoutuneet MP4-muodossa.  Vaillinaiset lauseet perään ylätunnisteruutu alussa on oltava paketti.  Huomautus "ftyp", "Live palvelin luettelon ruutuun" ja "moov"-ruutuun (tässä järjestyksessä) on lähetettävä sivupyynnön kanssa, vaikka koodaus on uudelleen, koska edelliseen pyyntö keskeytettiin ennen virta loppuun. 
4. Encoder on käytettävä siirtää lohkokoodausta ladataan, koska se on mahdotonta ennustaa live tapahtuman koko sisällön pituus.
5. Kun tapahtuma on päättynyt, edellisen osan lähettämisen jälkeen encoder tilanteen on lopetettava siirtää lohkokoodausta-viestin jaksossa (useimpien HTTP-asiakasohjelman pinoa käsittele sitä automaattisesti). Encoder on odotettava palvelun Palauta lopullinen vastaus-koodin ja lopeta sitten yhteys. 
6. Käytä Events() substantiivin ei Encoder saa kuvatulla tavalla 9.2 in [1] erilaisille live tuominen Microsoft Azure Media Services.
7. Jos HTTP POST-pyynnössä lopettaa tai TCP-virhe stream loppuun ennen aikakatkaistaan, koodaus on uusi viesti-pyynnön, uusi yhteys ja noudata edellä vaatimusten kanssa muita vaatimus, että encoder on lähettää edellisen kahdessa MP4-osassa, raidat virta- ja jatka ilman discontinuities media aikajanan esittely.  Kaksi MP4 osat, raidat uudelleenlähettämisestä varmistaa, että ei ole tietojen menettämisen.  Toisin sanoen jos stream sisältää ääni- ja Jäljitä ja nykyisen viestin pyynnön epäonnistuu, encoder on yhdistää ja Lähetä ääniraidan, jotka on aiemmin lähetetty onnistuneesti, kaksi osat ja kaksi osat videon seuraa, jotka on aiemmin lähetetty, jotta voit varmistaa, että ei ole tietojen menettämisen.  Koodaus on säilytettävä "eteenpäin" puskurin media-osat, jotka se lähettää, kun yhdistetään uudelleen.

##<a name="5-timescale"></a>5. aikajanan 

[[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx) on kuvattu "Aikajana" käyttö SmoothStreamingMedia (osa 2.2.2.1), StreamElement (osa 2.2.2.3), StreamFragmentElement(2.2.2.6) ja LiveSMIL (osa 2.2.7.3.1). Jos aika-asteikon arvo ei ole määritetty, käytetään oletusarvo on 10 000 000 (10 MHz). Vaikka tasainen Streaming Format-määrityksiä ei estä käyttö aikajanan muita arvoja encoder käyttöotot käyttää useimmat oletusarvo (10 MHz) luo tasainen Streaming ingest tiedot. Vuoksi [Azure Media dynaaminen pakkaaminen](media-services-dynamic-packaging-overview.md) toimintoa on suositellun käyttämään 90 kHz aikajanan video virtaa ja 44,1 tai 48.1 kHz ääni. Jos eri aikajana-arvoja käytetään eri virtaa, stream tason aika-asteikon lähetetään. Tutustu [[MS-SSTR]](https://msdn.microsoft.com/library/ff469518.aspx).     

##<a name="6-definition-of-stream"></a>6. "Muodossa" määritelmään  

"Muodossa" on toiminnan live nieltynä kirjoittaminen esityksen, käsittely streaming automaattisesti ja redundancy skenaarioita. "Muodossa" on määritetty yksi yksilöllinen pirstoutuneet MP4 bittinen-stream joka voi olla yksittäinen seuraa tai useita raidat. Koko esityksen voi sisältää vähintään yhden virtaa live encoder(s) määritysten mukaan. Seuraavissa esimerkeissä osoittavat stream(s) avulla voit laatia koko esityksen eri vaihtoehtoja.

**Esimerkki:** 

Asiakas haluaa live streaming esityksen, joka sisältää seuraavat äänen/videon bitrates luominen:

Video: 3000kbps, 1500kbps, 750kbps

Äänen – 128kbps

###<a name="option-1-all-tracks-in-one-stream"></a>Vaihtoehto 1: Kaikki raidat yhden muodossa:

Tämä vaihtoehto yksittäisen encoder Luo kaikki äänen/videon raidat ja yhdistä ne yhdeksi yhden pirstoutuneet MP4 bittinen-stream joka sitten mene yhden viestin HTTP-yhteyden kautta. Tässä esimerkissä on vain yksi profiilivirta live esitystä:

![image2][image2]

###<a name="option-2-each-track-in-a-separate-stream"></a>Vaihtoehto 2: Kunkin radan eri muodossa

Tämän asetuksen Sijoita vain yksi encoder(s) seurata kyselyjä kunkin fragmentti MP4-bittinen-muodossa ja kirjaa virtaa useita eri HTTP-yhteyden kautta. Tämän voi tehdä koodauslaitteista yhden tai usean koodauslaitteista. Live nieltynä näkökulmasta live esitystä koostuu neljä virtaa.

![image3][image3]

###<a name="option-3-bundle-audio-track-with-the-lowest-bitrate-video-track-into-one-stream"></a>Vaihtoehto 3: Yhdistä ääniraidan rataa pienin bittitaajuus videon yhden stream tuominen

Asiakas valitsee tämän vaihtoehdon, Yhdistä ääniraidan rataa pienin bittitaajuus videon yhden fragmentti MP4 bittinen stream kyselyjä ja jätä muiden kaksi videon raidat parhaillaan omassa muodossa. 


![image4][image4]


###<a name="summary"></a>Yhteenveto

Mikä on yllä ei ole täydellinen luettelo tässä esimerkissä kaikki mahdolliset nieltynä asetukset. Mahdollisimman matter, itse asiassa kaikki ryhmittely raidat kyselyjä virtaa tukee reaaliaikaista nieltynä. Asiakkaiden ja encoder toimittajien valita oman käyttöotot suunnitteluryhmät monimutkaisuus, encoder kapasiteetin ja redundancy ja automaattisesti huomioon otettavia seikkoja perusteella. Olisi kuitenkin huomattava, että useimmissa tapauksissa on vain yksi ääniraidan koko live esityksen joten on tärkeää varmistaa, joka sisältää ääniraidan ingest virta terveellisyyden. Huomiota usein johtaa ääniraidan liikkeelle omassa stream (kuten vaihtoehto 2) tai niputus pienin bittitaajuus videon seuraa (kuten vaihtoehto 3) kanssa. Myös paremmin redundancy ja vikasietoa lähettäminen saman ääniraita kaksi eri virtaa (tarpeettomat ääni raidat vaihtoehto 2) tai niputus ääniraidan vähintään kaksi pienin bittitaajuus videon raidat (vaihtoehto 3 yhteen vähintään kaksi videon virtaa äänellä) on erittäin suositeltavaa live ingest tuominen Microsoft Azure Media Services.

##<a name="7-service-failover"></a>7. palvelun automaattisesti 

Määritetty live streaming laatu hyvä automaattisesti tuki on erittäin tärkeää varmistaa, että palvelun käytettävyyttä. Microsoft Azure Media Services on suunniteltu käsitellään erilaisia virheitä, mukaan lukien verkon virheitä, palvelimen virheet, tallennustilan ongelmia. Käytettäessä yhdessä ERISNIMI automaattisesti logiikan live encoder reunasta asiakkaan voit tehostaa erittäin luotettavaa live streaming palvelu pilvestä.

Tässä osassa käsittelemme palvelun automaattisesti skenaarioita. Tässä tapauksessa virheen tapahtuu johonkin palvelun ja luettelot itse verkko-virhe. Seuraavassa on joitain suosituksia encoder käyttöönoton käsittelyyn palvelun automaattisesti:


1. Käytä TCP-yhteyden muodostamisesta 10 toisen aikakatkaisu.  Jos yrität muodostaa yhteyden kestää kauemmin kuin 10 sekuntia, Keskeytä toiminto ja yritä uudelleen. 
2. Käytä lyhyen aikakatkaisu, kun lähetät HTTP-pyyntö viestin näkyvissä.  Jos kohde MP4 fragmentti kesto on N sekuntia, käytä Lähetä aikakatkaisu N ja 2N sekuntia; Käytä esimerkiksi 6 12 sekuntia aikakatkaisu, jos MP4 fragmentti kesto on 6 sekuntia.  Jos aikakatkaisu, Palauta yhteys, avaa uuden yhteyden ja jatkaminen virtaa ingest uusi yhteys. 
3. Ylläpidä liukuvaa puskurin, joka sisältää kaksi osat raidat, jotka on kokonaan ja onnistuneesti lähettänyt palveluun.  Jos stream HTTP POST pyyntö on päättynyt tai se aikakatkaistaan ennen virta-loppuun Avaa uusi yhteys ja aloita toinen HTTP POST-pyynnön, Lähetä muodossa otsikot, Lähetä raidat kaksi osat ja jatka virta ilman keskeytyskohta media aikajanan esittely.  Tämä pienentää tietojen menettämisen mahdollisuutta.
4. On suositeltavaa encoder ei uudelleenyritykset muodostavat yhteyden tai Jatka toistamisen jälkeen TCP-virhe ilmenee määrän rajoittaminen.
5. Kun TCP-virhe:
    1. Nykyisen yhteyden on suljettava ja uusi HTTP POST-pyynnössä on luotava uusi yhteys.
    2. Uusi HTTP POST URL-osoite on oltava sama kuin alkuperäisen viestin URL-osoite.
    3. Otsikoiden stream ("ftyp", "Live palvelin luettelon ruutuun" ja "moov"-ruutu) identtiset virtauttaa otsikkoihin uusi HTTP-kirjaa-täytyy sisältää alkuperäisen viestin.
    4. Kaksi osat, jotka on lähetetty raidat on lähetetty uudelleen ja streaming jatkaa ilman keskeytyskohta media aikajanan esittely.  MP4-fragmentti aikaleimat on noustava jatkuvasti, jopa monista HTTP POST pyynnöt.
6. Encoder olisi Lopeta HTTP POST-pyynnön, jos tiedot eivät lähetetään MP4 fragmentti kesto soveltuva korvaus.  HTTP POST-pyynnön, jotka lähettävät tietoja ei voi estää Azure Media palveluiden katkaiseminen nopeasti encoder service-päivityksen yhteydessä.  Tämän vuoksi, HTTP-POST lyhyet (ad-signaali) raidat lyhyt pitäisi olla olleet lopetetaan, kun lyhyet fragmentti lähetetään.

##<a name="8-encoder-failover"></a>8. automaattisesti encoder

Toinen automaattisesti tilanne, jossa on pystyttävä lopusta loppuun live streaming toimittamista varten on Encoder automaattisesti. Tässä skenaariossa virhetilan tapahtui encoder reunassa. 

![image5][image5]


Seuraavassa on live nieltynä päätepisteestä odotuksia encoder automaattisesti tilanteissa:

1. Uuden esiintymän encoder luodaanko haluat jatkaa tietovirta, kuten yllä olevassa kaaviossa (3000 k asetuksista on katkoviiva Profiilivirta).
2. Uusi encoder on käytettävä URL-Osoitetta HTTP POST pyyntöjen, kun epäonnistuneita esiintymän.
3. Uusi encoder POST-pyynnössä täytyy sisältää sama pirstoutuneet MP4 otsikon ruudut, kun epäonnistuneita esiintymän.
4. Uusi encoder on synkronoitava oikein ja kaikki muut käynnissä koodauslaitteista live saman esityksen synkronoidun äänen/videon näytteiden tasatut fragmentti rajat luomiseen.
5. Uusi stream on oltava semanttisesti vastaavat kanssa edellisen virta ja vaihdettavissa ylä- ja fragmentti tasolla.
6. Uusi encoder yrittää pienentää tietojen menettämisen.  Fragment_absolute_time ja media-osat fragment_index olisi suurentaa kohta, jossa encoder viimeksi pysäytetty.  Fragment_absolute_time ja fragment_index olisi Suurenna jatkuva tavalla, mutta se on sallittu keskeytyskohta esitellä tarvittaessa.  Azure Media-palveluiden ohittaa Vaillinaiset lauseet se jo vastaanottanut ja käsitellä, joten voi olla parempi virhesanoma reunassa uudelleenlähettämisestä kuin haluat esitellä discontinuities media aikajanan Vaillinaiset lauseet. 

##<a name="9-encoder-redundancy"></a>9. Redundancy encoder 

Tiettyjen kriittinen live tapahtumat, että tarvittaessa myös suurempi käytettävyys ja käyttäjäkokemuksen laatuun, on suositeltavaa käyttää aktiivinen aktiivinen tarpeettomat koodauslaitteista saavuttamiseksi tiedot ilman yksityiskohtien saumaton automaattisesti.

![image6][image6]

Kuten yllä olevassa kaaviossa on kaksi ryhmän koodauslaitteista valitseminen kunkin stream kaksi kopiota samanaikaisesti live-palveluun. Näiden määritysten tuetaan, koska Microsoft Azure Media Services ei voi suodattaa pois kaksoiskappaleiden Vaillinaiset lauseet stream tunnuksen ja fragmentti aikaleiman perusteella. Tuloksena live virta ja arkisto on yksittäinen virtaa kopio, joka on paras mahdollinen koosteen kahden lähteen. Esimerkiksi hypoteettista erittäin tapauksessa nopeammin sellaisenaan (ei ole sama jokin) yhden encoder-käynnissä vaiheissa kunkin stream ajoissa, tuloksena olevat live virta-palvelusta on jatkuva ilman tietojen menettämisen. 

Tässä skenaariossa vaatimus on lähes sama kuin vaatimukset Encoder automaattisesti tapauksessa lukuun ottamatta määritettyjen koodauslaitteista käynnissä olevat ensisijainen koodauslaitteista samaan aikaan.

##<a name="10-service-redundancy"></a>10. Redundancy palvelun  

Erittäin tarpeettomat yleinen hajautuksessa se on joskus pakollinen rajat-alueen varmuuskopiointi käsittelemään alueellisen lieventämiseksi. Laajentaminen-"Encoder redundanssi" topologian, asiakkaat voivat valita on tarpeettomia palvelun käyttöönoton eri alueessa, joka on liitetty koodauslaitteista 2. omaavien. Asiakkaiden saattaa toimia myös CDN-palvelussa, voit ottaa käyttöön GTM (Yleinen liikenteen hallinta) kaksi palvelun käyttöönottoa ja haluat reitittää saumattomasti asiakkaan liikenteen eteen. Koodauslaitteista vaatimukset on sama kuin "Encoder redundanssi" tapauksessa ainoa poikkeus vaikuttavat koodauslaitteista toisen määrittäminen live toiseen ilmoittaa ingest loppupisteen merkki. Seuraavassa kuvassa näkyy nämä asetukset:

![image7][image7]

##<a name="11-special-types-of-ingestion-formats"></a>11. tietyn tyyppisten nieltynä muodot 

Tässä osassa käsitellään joitakin tietynlaisia live nieltynä muotoiluja, jotka on suunniteltu käsittelemään joitakin tietyissä skenaarioissa.

###<a name="sparse-track"></a>Lyhyet seuranta

Rich client-käyttökokemuksen streaming esityksen pitäessäsi on usein lähetys aika synkronoitu tapahtumia tai signaalit kaistan tärkeimmät media tiedoilla. Tämä esimerkki on dynaaminen live Active Directory-paikoilleen. Tämän tyyppistä tapahtuman Signalointi poikkeaa säännöllisesti äänen/videon streaming sen lyhyet laatu vuoksi. Toisin sanoen signaling tiedot yleensä ei suoriteta jatkuvasti ja väli voi olla vaikea ennustaa. Lyhyet Jäljitä käsite on suunniteltu erityisesti ingest ja Lähetä kaistan Signalointi tiedot.

Alla on suositellut käyttöönoton varten ingesting lyhyet seuraa:

1. Luo erillisen pirstoutuneet MP4 bittinen-stream joka sisältää vain lyhyet 2, äänen/videon raidat ilman.
2. Määrittää ylemmän tason Seuraa nimen "Live palvelin luettelon ruutuun" määritysten kohdassa 6 [1], "parentTrackName" parametrin avulla. Lue lisätietoja 4.2.1.2.1.2 [1]-kohtaan.
3. Kirjoita "Live palvelimen luettelon"-ruutuun manifestOutput on määritettävä "true".
4. Valita signaling tapahtuman lyhyet laatu, on suositeltavaa:
    1. Live tapahtuman alussa encoder lähettää alkuperäisen ylätunnisteruutu yksikön, joka mahdollistaa palvelun lyhyet seuraa rekisteröidään asiakas-luettelo.
    2. Encoder KANNATTAA lopettaa HTTP POST-pyynnön, kun tietoja ei lähetetä.  Pitkä käytössä HTTP-viestin, joka ei lähetä tiedot voit estää Azure Media Services katkaiseminen nopeasti encoder palvelun päivitys tai palvelimen-uudelleenkäynnistyksen yhteydessä kuin media-palvelinta estetään väliaikaisesti socket-Vastaanotto-toiminnossa. 
    3. Kun Signalointi tiedot eivät ole käytettävissä aikana encoder SHOULD Sulje HTTP POST pyytää.  Kirjaa pyynnön ollessa aktiivinen encoder Lähetä tiedot 
    4. Lyhyet Vaillinaiset lauseet lähetettäessä encoder määrittää sisällön pituus otsikon, jos se on käytettävissä.
    5. Uusi yhteys lyhyet fragmentti lähetettäessä encoder Käynnistä uuden Vaillinaiset lauseet perään ylätunnisteruutu lähettäminen. Tämä on käsittelemään silloin, kun automaattisesti on tapahtunut välillä ja uusi lyhyet yhteys muodostetaan uusi palvelimeen, joka on ole nähnyt ennen lyhyet seuraa.
    6. Lyhyet seuranta-osa on saatavana asiakkaalle kun vastaava päätaso seurata osa, joka on yhtä kuin tai suurentaa aikaleima-arvon otetaan käyttöön asiakkaalle. Esimerkiksi jos lyhyet osa on aikaleima t = 1 000, odotetaan sen jälkeen, kun asiakas näkee video (olettaen että ylätason Jäljitä nimi on video) jakaa aikaleima 1000 tai palkista, se voi ladata lyhyet fragmentti t = 1 000. Huomaa, että todellinen signaali voi hyvin käytetään eri sijaintiin esityksen aikajanan sen nimetyn tarkoitusta varten. Yllä olevassa esimerkissä on mahdollista, lyhyet fragmentti t = 1 000 on XML-tietojen lisääminen mainos, joka on joitakin sekunteja paikassa eli myöhemmin.
    7. Lyhyet Jäljitä fragmentti sisällön voi olla eri muotoilua (kuten XML tai teksti tai binaarinen, jne.) eri skenaarioissa mukaan. 


###<a name="redundant-audio-track"></a>Tarpeettomien ääniraidan

Tavallinen HTTP mukautuvat Streaming skenaario (kuten tasainen Streaming tai VIIVA) on usein kaksi ääniraidan koko esityksen. Toisin kuin videon raidat on useita laatutaso virhetilanteita valittavana asiakkaan ääniraidan voi olla yksittäisen kohdan virheen, jos muodossa, joka sisältää ääniraidan nieltynä on katkennut. 

Voit ratkaista tämän ongelman Microsoft Azure Media Services tukee reaaliaikaista nieltynä tarpeettomat ääni raidat. On, että sama ääniraidan voidaan lähettää useita kertoja eri virtaa. Vaikka palvelun vain Rekisteröi ääniraidan kerran asiakkaan luettelossa, voi käyttää tarpeettomat ääni raidat kuin varmuuskopioiden hakeminen ääni osat, jos ensisijainen ääniraidan on vaikeuksia. Jotta ingest tarpeettomat ääni raidat koodaus on:

1. Luo samaan ääniraidan useita fragmentti MP4 bittinen virtaa. Tarpeettomien ääni raidat on oltava semanttisesti vastaavat täsmälleen saman fragmentti aikaleimat kanssa ja vaihdettavissa ylä- ja fragmentti tasolla.
2. Varmistaa, että "ääni" merkintää Live palvelimen luettelon (osan 6 [1]) on oltava sama kaikki tarpeettomat ääni kappaleet.

Alla on tarpeettomia ääni raidat suositeltu käyttöönoton:

1. Lähetä kullekin yksilöllinen ääniraidan stream yksinään.  Myös lähettää tarpeettomat stream kunkin nämä ääniraidan virtaa, jossa 2. stream eroaa 1 vain HTTP POST URL-osoite-tunnisteen mukaan: {protokolla} :// {palvelimen osoitteen} / {julkaiseminen pisteen path}/Streams({identifier}).
2. Erillinen virtaa avulla voit lähettää kaksi pienin videon bitrates. Kaikkien näiden virtaa on myös oltava kunkin yksilöllinen ääniraidan kopio.  Esimerkiksi kun tukee useita kieliä, nämä virtaa pitäisi näkyä ääni raidat kullekin kielelle.
3. Erillinen server (encoder) esiintymät avulla voit salata ja lähettää mainitusta tarpeettomat virtaa (1) ja (2). 



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[image1]: ./media/media-services-fmp4-live-ingest-overview/media-services-image1.png
[image2]: ./media/media-services-fmp4-live-ingest-overview/media-services-image2.png
[image3]: ./media/media-services-fmp4-live-ingest-overview/media-services-image3.png
[image4]: ./media/media-services-fmp4-live-ingest-overview/media-services-image4.png
[image5]: ./media/media-services-fmp4-live-ingest-overview/media-services-image5.png
[image6]: ./media/media-services-fmp4-live-ingest-overview/media-services-image6.png
[image7]: ./media/media-services-fmp4-live-ingest-overview/media-services-image7.png

 