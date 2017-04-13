<properties
 pageTitle="IoT keskittimeen laitteen hallinnan yleiskatsaus | Microsoft Azure"
 description="Tässä artikkelissa on yleiskatsaus Azure IoT toiminnossa hallinta: yrityksen laitteen elinkaari, Käynnistä, factory Palauta, laiteohjelmiston päivitys, määritys, laitteen twins, kyselyt, työt"
 services="iot-hub"
 documentationCenter=""
 authors="bzurcher"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/03/2016"
 ms.author="bzurcher"/>

# <a name="overview-of-azure-iot-hub-device-management-preview"></a>Yleistä Azure IoT keskittimeen hallinta (ennakkoversio)

## <a name="introduction"></a>Johdanto

Azure IoT-toiminnossa on ominaisuudet ja joilla laitteen ja taustatietokantaan tehokkaat IoT laitteen ratkaisujen kehittäjät laajennettavuus-malli. IoT laitteiden väliltä rajoitettu anturit ja yksittäisen tarkoitus microcontrollers tehokkaita yhdyskäytävät, joka reitittää communications ryhmien laitteiden.  Lisäksi Käytä palvelupyyntöjä ja IoT operaattorit vaatimukset vaihtelevat merkittävästi teollisuuden.  Tätä muunnelmaa huolimatta Azure IoT keskittimeen hallinta sisältää ominaisuuksia, kuvioiden ja koodi-kirjastot, monipuolisen tietyt laitteet ja loppukäyttäjät mukauttaminen.

Tärkeä osa luominen onnistui yrityksen IoT ratkaisu säätää strategia miten operaattorit käsittelee niiden sivustokokoelman laitteiden jatkuvaluonteisesta hallinnasta. IoT operaattorit edellyttävät perus- ja luotettava työkaluilla ja sovelluksilla, ota käyttöön keskittyä työnsä tärkeämpiä ominaisuuksia. Tässä artikkelissa:

- Azure IoT keskittimeen hallintatavan Laitehallinnan lyhyesti.
- Yhteiset laitteen hallinta periaatteet kuvaus.
- Laitteen elinkaari kuvaus.
- Yleiskatsaus Yleiset laitteen hallinta mallit.

## <a name="iot-device-management-principles"></a>IoT laitteen hallinta periaatteet

IoT tuo sitä ainutlaatuisia laitteen hallinta haasteita ja jokaisen yritysluokan ratkaisu on osoite seuraavien periaatteiden:

![Azure IoT keskittimeen laitteen hallinta periaatteiden kuva][img-dm_principles]

- **Skaalaa- ja automaatiofunktiot**: IoT ratkaisuja edellyttävät yksinkertainen työkaluja, joilla voit automatisoida tavallisia tehtäviä ja ottaa käyttöön suhteellisen pieni toimintojen henkilökunnan hallittavan miljoonia laitteet. Päivittäinen-operaattoreita tarpeen käsitellä laitteen toimintojen etäyhteyden välityksellä, kerralla ja vain ilmoituksen, kun ilmenee ongelmia, jotka edellyttävät niiden suora toimia.

- **Avoimuus ja yhteensopivuus**: IoT laitteen ekosysteemiin on vesivaroille monipuolisen. Hallintatyökalut on suunniteltu sopimaan monipuolisia laitteen luokkien, ympäristöjen ja protokollat. Operaattorien pystyttävä tukemaan useita erityyppisiä laitteet, tehokkaat upotetun yhden prosessin lastut, valitse tehokkaita ja täysin tietokoneissa.

- **Konteksti tilatieto**: IoT ympäristöissä ovat dynaamisia ja koskaan muuttaminen. Palvelun luotettavuutta on ensiarvoisen. Laitteen hallintatoiminnot täytyy ottaa huomioon SLA ylläpito windows, verkko-ja power, käytössä ehdot ja varmistaa, että ylläpito käyttökatkot ei tärkeät toiminnot vaikuttavat tai luoda vaarallisten ehtoja: laitteen n maantieteellinen sijainti.

- **Palvelun monta rooleja**: yksilölliset työnkulut ja prosessien IoT toimintojen roolien tuki on tärkeää. Toimintojen henkilökunnan on työskenneltävä harmoniously sisäinen IT-osastoille annetun rajoituksiin.  Ne myös etsiä pinta reaaliaikainen laitteen toimintojen tietoja supervisors ja muut johto liiketoiminta-roolien kestävän tapoja.

## <a name="iot-device-lifecycle"></a>IoT laitteen elinkaari

Ei yleensä laitteen hallinta vaiheet, jotka ovat yleisiä kaikkien yritysprojektien IoT joukko. Azure IoT on viisi IoT laitteen elinkaari vaiheita:

![Viisi Azure IoT laitteen elinkaari vaihetta: suunnitelma, valmistelu, määrittää, valvonta, Keskeytä][img-device_lifecycle]

Kunkin viisi nämä vaiheet on useita laitteen operaattori vaatimuksia, joka on täytyttävä antamaan täydellinen ratkaisu:

- **Suunnittelu**: Luo laitteen metatietojen malli, jonka avulla he voivat helposti ja tarkasti kyselyn sekä kohderyhmä joukkona hallintatoiminnot laitteiden toimijat. Laitteen sekä avulla voit tallentaa laitteen metatiedot tunnisteet ja ominaisuudet.

    *Katso myös*: [Aloita laitteen twins][lnk-twins-getstarted], [ymmärtäminen laitteen twins][lnk-twins-devguide], [sekä ominaisuuksien käyttäminen][lnk-twin-properties]

- **Säännöstä**: suojatusti valmistella IoT keskittimeen uudet laitteet ja toimijat tuttuihin heti laitteen ominaisuuksista.  IoT keskittimeen laitteen rekisterin avulla voit luoda joustavia laitteen käyttäjätietoja ja käyttäjätiedot ja suorittaa tämän toiminnon avulla työn kerralla. Luominen ja niiden ominaisuudet ja kautta laitteen ominaisuudet-laitteen sekä ehdot raportoi laitteet.

    *Katso myös*: [hallinta laitteen käyttäjätietojen][lnk-identity-registry], [laite käyttäjätietojen hallinta joukkona][lnk-bulk-identity], [sekä ominaisuuksien käyttäminen][lnk-twin-properties]

- **Määritä**: helpottaa joukkona määritysmuutoksia ja laiteohjelmiston päivitykset laitteet säilyttäen terveys- ja suojaus. Suorittaa näitä laitteen hallintatoiminnot kerralla, käyttämällä haluamasi ominaisuudet tai suoraan menetelmät ja lähetyksen työt.

    *Katso myös*: [suora menetelmillä][lnk-c2d-methods], [Käynnistä suora menetelmä laitteessa][lnk-methods-devguide], [sekä ominaisuuksien käyttäminen][lnk-twin-properties], [aikataulun ja lähetyksen työt][lnk-jobs], [Ajoita työt useilla eri laitteilla][lnk-jobs-devguide]

- **Näyttö**: Yleinen laitteen sivustokokoelman kuntotietojen, meneillään olevien toimintojen tilan seuranta ja ilmoita ongelmat, jotka voi edellyttää niiden huomiota operaattorit.  Sovellettava laitteen sekä sallimaan laitteiden raportin reaaliaikainen toiminta ehtoja ja tila päivitys-toimintoa. Tehokas Raporttinäkymät-ikkunan, tuoda suorin ongelmat käyttämällä laitteen sekä kyselyt raporttien rakentamiseen.

    *Katso myös*: [käyttämisestä sekä ominaisuudet][lnk-twin-properties], [twins ja töiden kyselykielen][lnk-query-language]

- **Retire**: korvaa tai poista käytöstä laitteiden virheen jälkeen, Päivitä kehä- tai palvelun elinkaaren lopussa.  Laitteen sekä avulla voit ylläpitää laitteen tiedot, jos fyysinen laite on korvattu tai arkistoida, jos on poistettu käytöstä. Käytä IoT keskittimeen laitteen rekisterin peruutetaan suojatusti laitteen käyttäjätietoja ja tunnistetiedot.

    *Katso myös*: [käyttämisestä sekä ominaisuudet][lnk-twin-properties], [laite käyttäjätietojen hallinta][lnk-identity-registry]

## <a name="iot-hub-device-management-patterns"></a>IoT keskittimeen laitteen hallinta kuviot

IoT keskittimeen mahdollistaa laitteen hallinta kuviot seuraavat joukko.  [Laitteen hallinta opetusohjelmat] [ lnk-get-started] kerrotaan tarkemmin laajentamisesta nämä kuviot sopivaksi tarkka skenaarion ja kuinka voit suunnitella uuden kuviot core-mallien perusteella.

- **Käynnistä uudelleen** - taustatietokannan sovellus ilmoittaa laitteen suora menetelmä kautta, että se on aloittanut uudelleen.  Laitteen käyttää laitteen sekä raportoitu ominaisuudet laitteen uudelleenkäynnistyksen tilan päivittäminen.

    ![Azure IoT keskittimeen hallinta uudelleen kuvio-kuva][img-reboot_pattern]

- **Factory Palauta** - taustatietokannan sovellus ilmoittaa laitteen suora menetelmä kautta, että se on aloittanut factory Palauta.  Laitteen käyttää laitteen sekä raportoidun ominaisuudet päivittää tehdas Palauta tila, laite.

    ![Azure IoT keskittimeen laitteen factory kuvion kuvan palauttaminen][img-facreset_pattern]

- **Määritys** - taustatietokannan sovellus käyttää laitteen sekä haluamasi ominaisuudet laitteeseen ohjelmiston määrittämiseen.  Laitteen käyttää laitteen sekä raportoitu ominaisuudet päivitetään laitteen tilan määritys.

    ![Azure IoT keskittimeen laitteen hallinta määritysten kuvion kuva][img-config_pattern]

- **Päivitä laitteisto** - taustatietokannan sovellusta ilmoittaa laitteen suora menetelmä kautta, että se on aloittanut laitteisto-päivitys.  Laitteen käynnistää multistep prosessin Lataa laitteisto-paketti, käytä laitteisto-paketin ja lopuksi Muodosta yhteys IoT toiminto uudelleen.  Koko mult vaihetta, laite käyttää laitteen sekä raportoitu ominaisuudet päivitetään laitteen tilan ja edistymisen.

    ![Azure IoT keskittimeen laitteen laitteisto kuvion kuvien päivittäminen][img-fwupdate_pattern]

- **Raportoinnin edistymistä ja tilaa** - sovelluksen taustatietokantaan suorittaa laitteen sekä kyselyt-laitteiden raportointia tilan ja edistymisen toimintojen käytössä laitteiden välillä.

    ![Azure IoT keskittimeen hallinnan raportointi edistymistä ja tilaa kuvio-kuva][img-report_progress_pattern]

## <a name="next-steps"></a>Seuraavat vaiheet

Voit käyttää ominaisuuksia, kuvioiden ja koodi-kirjastot, joka sisältää Azure IoT keskittimeen hallinta voit luoda IoT-sovelluksia, jotka täyttävät yrityksen IoT operaattori kuluessa kunkin laitteen lifecycle-vaiheessa.

Lue lisää Azure IoT keskittimeen laitteen hallintaominaisuudet, on artikkelissa [Azure IoT keskittimeen mobiililaitteiden hallinnan käytön aloittaminen] [ lnk-get-started] opetusohjelma.

<!-- Images and links -->
[img-dm_principles]: media/iot-hub-device-management-overview/image4.png
[img-device_lifecycle]: media/iot-hub-device-management-overview/image5.png
[img-config_pattern]: media/iot-hub-device-management-overview/configuration-pattern.png
[img-facreset_pattern]: media/iot-hub-device-management-overview/facreset-pattern.png
[img-fwupdate_pattern]: media/iot-hub-device-management-overview/fwupdate-pattern.png
[img-reboot_pattern]: media/iot-hub-device-management-overview/reboot-pattern.png
[img-report_progress_pattern]: media/iot-hub-device-management-overview/report-progress-pattern.png

[lnk-twins-devguide]: iot-hub-devguide-device-twins.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-twins-getstarted]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-hub-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-query-language]: iot-hub-devguide-query-language.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-methods-devguide]: iot-hub-devguide-direct-methods.md
[lnk-jobs]: iot-hub-schedule-jobs.md
[lnk-jobs-devguide]: iot-hub-devguide-jobs.md