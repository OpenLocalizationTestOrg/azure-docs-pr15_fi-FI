<properties 
    pageTitle="Azure RemoteApp - testaaminen verkon kaistanleveyden käytön havainnollistetaan Yleisiä tilanteita, jossa | Microsoft Azure"
    description="Katso, miten yleisiä käyttötilanteita, joissa avulla voit selvittää, verkon kaistanleveyden tarpeitasi Azure RemoteApp varten."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp---testing-your-network-bandwidth-usage-with-some-common-scenarios"></a>Azure RemoteApp - testaaminen verkon kaistanleveyden käytön joitakin yleisiä tilanteita, joissa kanssa

> [AZURE.IMPORTANT]
> Azure RemoteApp on on lopetettu. Lue lisätietoja [ilmoitus](https://go.microsoft.com/fwlink/?linkid=821148) .

Kun kerrottu [arvion Azure RemoteApp verkon kaistanleveyden käytön](remoteapp-bandwidth.md), paras tapa Azure RemoteApp vaikutus verkkoon on suorittaa joitakin käyttö voi selvittää testaaminen Suorita seuraavat testit määritetyn ajanjakson ja mitata tarvittavat kunkin skenaarion kaistanleveyden. Jos sinulla on ominaisuutta, voit myös mitata-paketin tappio ja verkon verkkoviive selvittääksesi, jotka luodaan järjestelmäsi verkon kuviot.

    
Kaistanleveyden käytön laskettaessa muista, että käyttö vaihtelee eri käyttäjien yrityksessäsi välillä. Esimerkiksi tekstiä lukijat ja kirjoittajat yleensä tarjoaman vähemmän kuin käyttäjät, jotka toimivat video. Saat parhaan tuloksen tutkia käyttäjän omia tarpeita ja luo mix seuraavista tilanteista, joka vastaa parhaiten yrityksesi käyttäjien. Muista [, vaikutus kaistanleveyden käytön ja käyttäjän kohdata tekijät](remoteapp-bandwidthexperience.md) - auttaa sinua tunnistamaan ihanteellinen testejä.

Lue ensin testejä, valitse että mix ja suorita sitten niitä. Alla olevan taulukon avulla voit seurata suorituskykyä.

>[AZURE.NOTE] Jos et voi tehdä omia verkko-testaaminen tai sinulla ei ole ajan kiellä, tutustu Microsoftin [perusverkko kaistanleveyden arvioiden/suosituksia](remoteapp-bandwidthguidelines.md). Oman matka voi vaihdella, niin oman testien avulla *Voit* suorittaa, jos.


## <a name="the-usage-tests"></a>Käyttö-testiä
Kunkin Suorita eri summien ajan ja testata eri Funktiot/ominaisuuksia, jotka käyttävät kaistanleveys. Muista valita testin mix parhaiten vastaa yksittäisiin yrityksen käyttäjien.
 
### <a name="executivecomplex-powerpoint---run-for-900-1000-seconds"></a>Johdon/kompleksiluvun PowerPoint - Suorita 900 1000 sekunnin ajan

Käyttäjä näkyy 45 – 50 erittäin tarkan diaan käyttämällä Microsoft Office PowerPoint koko näytön tilassa. Diojen pitäisi olla kuvia, siirtymät (ja animaatiot) ja taustojen liukuvärjäys kanssa, jotka ovat tyypillisiä yrityksesi. Käyttäjän kannattaa käyttää vähintään 20 sekuntia kunkin dian.
    
Tässä skenaariossa Luo purskeista liikenne, kun diaan Siirtyminen seuraavaan diaan esityksessä.
    
### <a name="simple-powerpoint---run-for-620-seconds"></a>Yksinkertaisen PowerPoint - Suorita ~ 620 sekunnin ajan

Käyttäjän esittää yksinkertaisen PowerPoint-tiedoston noin 30 dioista käyttämällä Microsoft Office PowerPoint koko näytön tilassa. Diat ovat enemmän tekstiä paljon kuin johdon/kompleksiluvun PowerPoint-tilanne ja taustojen ja kuvat (musta kaaviot). 
    
### <a name="internet-explorer---run-for-250-seconds"></a>Internet Explorer - Suorita noin 250 sekunnin ajan

Käyttäjä avaa verkosta Internet Explorerin avulla. Käyttäjä avaa ja vierittää tekstiä, luonnollinen kuvia sekä joitakin, joka sisältää kaavioita. Tallennettu Remote Desktop-istunnon isäntään (RD istunnon isäntä)-palvelimen paikalliselle levyasemalle verkkosivujen. MHT-tiedosto. Käyttäjä vierittää, Page Up, Page Down-ylös ja alas-näppäimiä käyttäminen erilaisten välit Vieritä kunkin avaimen/tyyppi:
    
    - Alas - näppäimiä 250 erittäin 500 ms
    - Page Up - 36 näppäinkomennot jokaisen 1000 ms
    - Alas - 75 näppäinkomennot jokaisen 100 ms
    - Page Down - 20 näppäinkomennot jokaisen 500 ms
    - Ylös - 120 näppäinkomennot jokaisen 300 ms
    
### <a name="pdf-document---simple---run-for-610-seconds"></a>PDF-tiedosto - yksinkertainen - Suorita ~ 610 sekunnin ajan
Käyttäjän lukee ja etsii PDF-tiedoston eri tavoin käyttäen Adobe Acrobatin Reader. Asiakirjan olisi koostuvat taulukoiden, yksinkertaisten kaavioiden ja useita fontteja. Asiakirja on 35 40 sivujen pitkä. Käyttäjän vierittää kaksi eri koroilla taaksepäin ja välittää neljä eri Zoomaus koon mukauttaminen (Sovita leveyden Sovita-sivulla 100 %, ja toisen sähköpostikansioon). Zoomaus varmistaa, että teksti (fonttia) hahmontaa erikokoisia. Vierittäminen on Page Up-, Page Down-ylös ja alas-näppäimiä käyttäminen erilaisten välit kunkin Vierittää alaspäin.

### <a name="pdf-document---mixed---run-for-320-seconds"></a>Asiakirjan PDF - Sekalaiset - Suorita ~ 320 sekunnin ajan
Käyttäjän lukee ja etsii PDF-tiedoston eri tavoin käyttäen Adobe Acrobatin Reader. Asiakirjan koostuu (kuten valokuvat) laadukkaita kuvia, taulukoita, yksinkertaisten kaavioiden ja useita fontteja. Käyttäjän vierittää kaksi eri koroilla taaksepäin ja välittää neljä eri Zoomaus koon mukauttaminen (Sovita leveyden Sovita-sivulla 100 %, ja toisen sähköpostikansioon). Zoomaus varmistaa, että teksti (fonttia) hahmontaa erikokoisia. Vierittäminen on Page Up-, Page Down-ylös ja alas-näppäimiä käyttäminen erilaisten välit kunkin Vierittää alaspäin.

### <a name="flash-video-playback---run-for-180-seconds"></a>Flash videon toisto - Suorita ~ 180 sekunnin ajan
Käyttäjä näkee Adobe Flash-koodattu video, upotettuna web-sivulle. Verkkosivun tallennetaan RD istunnon isäntään palvelimen paikalliselle kiintolevylle. Video toistetaan Internet Explorer upotettu Playerin mukaan.

Tässä skenaariossa emuloi rich sisällön verkkosivujen, joka sisältää multimedia käyttäjiltä. Useimmat tietojen pitäisi ruutu VOBR kautta.

### <a name="word-remote-typing---run-for-1800-seconds"></a>Wordin remote kirjoittamalla - Suorita ~ 1800 sekunnin ajan
Käyttäjä kirjoittaa asiakirjan RDP-istunnon päälle. Näppäinkomennot lähetetään-kautta RDP-istunnon asiakaspuolen Microsoft Word-asiakirjaan Etäistunto käynnissä. Kirjoittaminen korko on yhden merkin jokaisen 250 ms (yhteensä 7050 merkkiä). 

Tämä on eniten Yleisiä tilanteita, joissa knowledge työntekijän. Tässä skenaariossa Testaa Moderni työ-suoritin kirjoittamalla käyttäjän vasteaikaa. Tässä skenaariossa on merkitsevä kaistanleveyden käytön jopa pieniä muutoksia.

## <a name="tracking-the-test-results"></a>Testitulokset seuranta

Seuraavan taulukon avulla voit arvioida käyttötavoista ympäristössäsi. Jäljempänä tiedot koskee vain kuva – voi olla suojausta eroaa voit tarkkailla. 

Yksinkertaisuuden oletetaan, että kaikissa tilanteissa testataan käyttämällä samaa 1920 x 1080 kuvapisteen tarkkuutta ja TCP tiedonsiirron verkossa, jossa viive (viive) alapuolella 200 ms ja verkon synkronointivirheiden 120 ms +-merkki on noin 1 %.

Tietoja taulukko:
- **Keskiarvo-ominaisuus** on kaistanleveys, jossa käyttäjä tuottavuutta ei merkittävästi vaikuttaa, mutta ei estä ajoittaiset videon tai äänen virheistä. Järjestelmä ei voi palauttaa nopeasti dynaaminen logiikan hyödyntämistä. Voit varmistaa käyttäjäkokemuksen laatuun verkon kaistanleveyden arvioiden-yritys.
 - Kaistanleveys, jossa käyttäjät havaita kokemuksensa merkittäviä ongelmat ja niiden tuottavuutta vaikuttaa mitattavissa aikajaksojen sisältää **Noticeable ongelmat (sivunvaihto kohta)** . Tässä vaiheessa RDP-algoritmien ovat struggling ja käyttäjän kokemuksen laatu voi taata riittämätön kaistanleveys vuoksi.
 - **Suositus** on hyvä tai erinomainen takaamiseksi suositellut kaistanleveys. Se on suurempi kuin vastaavan **keskiarvo kohdata** -sarakkeen arvo yleensä yhdellä kertaa.
 - **Muistiinpanoissa huomautusten.**
 
| Testi                  | Keskimääräisen kokemus | Huomattavia ongelmia (sivunvaihto piste) | Suositeltu kaistanleveys | Huomautuksia                                                              |
|-----------------------|--------------------|---------------------------------|-------------------------------|--------------------------------------------------------------------|
| Johdon/kompleksiluvun PPT | 10 Mt/s             | 1 Mt/s                           | > 10 Mt/s, 100 Mt/s ensisijainen    | 1 Mt/s useita animaatioita menetetään                                   |
| Yksinkertainen PPT            | 5 Mt/s              | 256 kt/s                         | 10 Mt/s                        | 256 kt/s diat ladata huomattavia viive                   |
| Internet Explorerissa     | 10 Mt/s             | 1 Mt/s                           | > 10 Mt/s, 100 Mt/s ensisijainen    | Web-videot ovat epätarkalta 1 Mt/s ja katkonainen, nopea vieritys on ongelmia |
| Yksinkertainen PDF            | 1 Mt/s              | 256 kt/s                         | 5 Mt/s                         | 256 kt/s kestää jonkin aikaa sivun lataaminen                       |
| Erilaiset PDF             | 1 Mt/s             | 256 kt/s                         | 5 Mt/s                         | 256 kt/s sivun on huomattavan paljon aikaa lataaminen    |
| Flash-videon toisto  | 10 Mt/s             | 1 Mt/s                           | > 10 Mt/s, 100 Mt/s ensisijainen    | 1 Mt/s video on epätarkka ja jotkin kehykset olevat kohteet poistetaan           |
| Word-remote kirjoittaminen    | 256 kt/s            | 128 kt/s                         | 1 Mt/s                         | 256 kt/s käyttäjä voi olla näppäimen painalluksella välisen ajan             |

Arvioida kaistanleveys käyttäjää kohden, Luo mix yllä tilanteissa ja tarvittavat kaistanleveys vastaava osuus. Valitse tarvittavat tekemät suurinta numeroa. Koska käyttäjät käyttävät tuskin koskaan yksin järjestelmää, kannattaa joitakin varaa käyttäjille, jotka työskentelevät samanaikaisesti samassa verkossa.
     
## <a name="learn-more"></a>Opi lisää
- [Azure RemoteApp verkon kaistanleveyden käytön arviointi](remoteapp-bandwidth.md)

- [Azure RemoteApp - miten kaistanleveys ja laatu ilmene työn samassa paikassa?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp-kaistanleveys - yleisiä ohjeita (Jos et voi testata oman)](remoteapp-bandwidthguidelines.md)