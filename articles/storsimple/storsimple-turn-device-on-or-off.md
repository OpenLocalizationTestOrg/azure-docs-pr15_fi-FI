<properties 
   pageTitle="StorSimple laitteen kytkeminen päälle tai pois | Microsoft Azure"
   description="Kerrotaan, miten voit ottaa käyttöön uuden StorSimple laitteen ja ottaminen käyttöön laitteessa, joka on Sammuta tai hävitty power käynnissä olevan laitteen käytöstä."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/23/2016"
   ms.author="alkohli" />

# <a name="turn-your-storsimple-device-on-or-off"></a>StorSimple laitteen ottaminen käyttöön ja poistaminen käytöstä 

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure StorSimple laitteen suljetaan ei tarvita osana Normaali järjestelmän toimintaa. Voi kuitenkin tarvitse ottaa käyttöön uuden laitteen tai laitteella, joka oli lopetetaan. Yleensä Sammuta tarvitaan tapauksissa, jossa on Vaihda epäonnistui laitteet, fyysisesti siirrät yksikkö tai tutustu laitteen poissa. Tässä opetusohjelmassa kuvataan tarvittavat ohjeet ottaminen käyttöön ja StorSimple laitteen eri skenaarioissa suljetaan.

Seuraavassa taulukossa luetellaan eri tilanteita, joissa ottaminen käyttöön ja StorSimple laitteen suljetaan ja aihetta käsittelevässä linkkejä.

|Skenaario|Ohjeaiheista|
|:-------|:---------------|
|Uuden laitteen ottaminen käyttöön|[Uuden laitteen ottaminen käyttöön](#turn-on-a-new-device)<ul><li>[Uusi laitetta, jonka ensisijainen kehys](#new-device-with-primary-enclosure-only)</li><li>[Uusi laitetta, jonka EBOD kehys](#new-device-with-ebod-enclosure)</li></ul>|
|Ota käyttöön laitteen sulkemisen jälkeen|[Ota käyttöön laitteen sulkemisen jälkeen](#turn-on-a-device-after-shutdown)<ul><li>[Ensisijainen kehyksen laitteella](#device-with-primary-enclosure-only)</li><li>[Laitetta, jonka EBOD kehys](#device-with-ebod-enclosure)</li></ul>|
|Laitteen ottaminen käyttöön power menettämisen jälkeen|[Laitteen ottaminen käyttöön power menettämisen jälkeen](#turn-on-a-device-after-a-power-loss)<ul><li>[Ensisijainen kehyksen laitteella](#8100)</li><li>[Laitetta, jonka EBOD kehys](#8600)</li></ul>|
|Käynnistä laite jälkeen ensisijainen kehys ja EBOD yhteys katkeaa|[Laite on ensisijainen jälkeen ottaminen käyttöön ja EBOD kehyksen yhteys katkeaa](#turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost)|
|Sulje suorittaminen laite|[Käynnissä olevan laitteen poistaminen käytöstä](#turn-off-a-running-device)<ul><li>[Ensisijainen kehyksen laitteella](#8100a)</li><li>[Laitetta, jonka EBOD kehys](#8600a)</li></ul>|

## <a name="turn-on-a-new-device"></a>Uuden laitteen ottaminen käyttöön

StorSimple laitteessa ensimmäistä kertaa käyttöönottotapa sen mukaan, onko laite 8100 tai 8600-malli. 8100 on yksi ensisijainen kehys 8600 on kaksi kehyksen laitetta, jonka ensisijainen kehys ja EBOD kehyksen. Seuraavissa osissa käsitellään molempien yksityiskohtaisia ohjeita.

- [Uusi laitetta, jonka ensisijainen kehys](#new-device-with-primary-enclosure-only)

- [Uusi laitetta, jonka EBOD kehys](#new-device-with-ebod-enclosure)

### <a name="new-device-with-primary-enclosure-only"></a>Uusi laitetta, jonka ensisijainen kehys

StorSimple 8100 malli on yksittäinen kehys-laite. Laitteen sisältää tarpeettomat Power ja jäähdytys moduulit (PCMs). Sekä PCMs on oltava asennettuna ja eri power lähteistä, jos haluat varmistaa suuren käytettävyyden yhteydessä.

Seuraavien toimien avulla kaapeli laitteesi Power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

>[AZURE.NOTE]Valmis laitteen määrittäminen ja kaapeli ohjeita Siirry [StorSimple 8100 laitteen asentaminen](storsimple-8100-hardware-installation.md). Varmista, että noudattamalla ohjeita täsmälleen.

### <a name="new-device-with-ebod-enclosure"></a>Uusi laitetta, jonka EBOD kehys

StorSimple 8600-malli on ensisijainen kehys ja EBOD kehyksen. Tämä edellyttää yksiköt kytketty yhdessä Serial liitetty SCSI (SAS)-yhteyden ja power.

Kun määrität tässä laitteessa ensimmäistä kertaa, Suorita vaiheet SAS kaapeli ensin ja tee sitten ohjeet power kaapeli.

[AZURE.INCLUDE [storsimple-sas-cable-8600](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

>[AZURE.NOTE]Valmis laitteen määrittäminen ja kaapeli ohjeita Siirry [StorSimple 8600 laitteen asentaminen](storsimple-8600-hardware-installation.md). Varmista, että noudattamalla ohjeita täsmälleen.

## <a name="turn-on-a-device-after-shutdown"></a>Ota käyttöön laitteen sulkemisen jälkeen

Kytkemiseksi StorSimple laitteessa, kun se on suljettu rekisteröitymisvaiheet ovat erilaiset riippuen siitä, onko laite 8100 tai 8600-malli. 8100 on yksi ensisijainen kehys 8600 on kaksi kehyksen laitetta, jonka ensisijainen kehys ja EBOD kehyksen.

- [Ensisijainen kehyksen laitteella](#device-with-primary-enclosure-only)

- [Laitetta, jonka EBOD kehys](#device-with-ebod-enclosure)

### <a name="device-with-primary-enclosure-only"></a>Ensisijainen kehyksen laitteella

Sulkemisen jälkeen StorSimple laitetta, jonka ensisijainen kehys ja EBOD ei kehyksen ottaminen seuraavien ohjeiden avulla.

#### <a name="to-turn-on-a-device-with-a-primary-enclosure-only"></a>Voit ottaa vain ensisijainen kehyksen laitteella

1. Varmista, että potenssiin siirtyy sekä virtaa jäähdytys moduulit (PCMs) ovat asentoon ei käytössä. Jos valitsimet eivät ole asentoon ei käytössä, käännä ne asentoon ei käytössä ja odota valot Siirry.

2. Poistaa laitteeseen kääntäminen power valitsimet-sekä PCMs käytössä-asentoon. Laitteen kannattaa ottaa käyttöön.

3. Tarkista, että laite on täysin seuraavat:

    1. Molemmat PCM moduulit OK-merkkivalot ovat vihreitä.

    2. Molemmat ohjaimet tilan merkkivalot ovat tasainen vihreä.

    3. Vilkkuvaa on sininen LED johonkin ohjaimet, joka ilmaisee, että ohjain on aktiivinen.

    Jos jokin seuraavista ehdoista täyttyy ei täyty laitteesi ei ole kunnossa. Ota yhteyttä [Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).

### <a name="device-with-ebod-enclosure"></a>Laitetta, jonka EBOD kehys

Sulkemisen jälkeen StorSimple laitetta, jonka ensisijainen kehys ja EBOD kehyksen ottaminen seuraavien ohjeiden avulla. Suorittaa kunkin vaiheen järjestyksessä täsmälleen kuvatulla tavalla. Tietojen menettämisen voi aiheuttaa virheen tekemään niin.

#### <a name="to-turn-on-a-device-with-a-primary-and-an-ebod-enclosure"></a>Jos haluat poistaa perusavaimen ja EBOD kehyksen laitteella

1. Varmista, että EBOD kehyksen on yhteydessä ensisijainen kehys. Lisätietoja on kohdassa [StorSimple 8600 laitteen asentaminen](storsimple-8600-hardware-installation.md).

2. Varmista, että Power ja jäähdytys moduulit (PCMs) sekä EBOD ensisijainen liitteet ovat asentoon ei käytössä. Jos valitsimet eivät ole asentoon ei käytössä, käännä ne asentoon ei käytössä ja odota valot Siirry.

3. Ota käyttöön EBOD kehyksen ensin mukaan kääntäminen potenssiin siirtyy sekä PCMs-käytössä. PCM merkkivalot pitäisi olla vihreä. Vihreä EBOD valvonta on LED yksikkö-ilmaisee, että EBOD kehyksen on otettu käyttöön.

4. Ota käyttöön kääntäminen power valitsimet sekä PCMs edelleen kohtaan, valitse ensisijainen kehys. Koko järjestelmästä pitäisi nyt olla.

5. Varmista, että SAS merkkivalot ovat vihreänä, joka varmistaa, että EBOD kehys ja ensisijainen kehyksen välinen yhteys on hyvä.

## <a name="turn-on-a-device-after-a-power-loss"></a>Laitteen ottaminen käyttöön power menettämisen jälkeen

Power käyttökatkosta tai keskeyttäminen voit sulkea StorSimple laite. Power-käyttökatkosta voi esiintyä yhdestä power tai molempien power toimitukset. Palautus rekisteröitymisvaiheet ovat erilaiset riippuen siitä, onko laite 8100 tai 8600-malli. 8100 on yksi ensisijainen kehys 8600 on kaksi kehyksen laitetta, jonka ensisijainen kehys ja EBOD kehyksen. Tässä osassa kuvataan kunkin skenaarion palautus-ohjeet.

- [Ensisijainen kehyksen laitteella](#8100)

- [Laitteen kanssa EBOD kehys](#8600)

### <a name="device-with-primary-enclosure-only-a-name8100"></a>Ensisijainen kehyksen laitteella<a name="8100">

Järjestelmä voi jatkaa normaalisti, onko power tappio johonkin sen power tarvikkeiden. Jotta suuren käytettävyyden laitteen palauttamaan power power hankintaan mahdollisimman pian.

Jos näkyvissä on power käyttökatkosta tai valitse molemmat power tarvikkeiden keskeytyskohdasta power, järjestelmä suljetaan siisti ja valvottu tavalla. Tarvita, järjestelmä otetaan automaattisesti käyttöön.

### <a name="device-with-ebod-enclosure-a-name8600"></a>Laitteen kanssa EBOD kehys<a name="8600">

#### <a name="power-loss-on-one-power-supply"></a>Yksi power Power tappio antaa

Järjestelmä voi jatkaa normaalisti, onko power tappio johonkin sen power toimistotarvikkeita ensisijainen kehyksen tai EBOD kehys. Jotta suuren käytettävyyden laitteen, ota palauttamaan power power hankintaan mahdollisimman pian.

#### <a name="power-loss-on-both-power-supplies-on-primary-and-ebod-enclosures"></a>Perus- ja EBOD liitteet power sekä toimistotarvikkeita Power tappio

Jos näkyvissä on power käyttökatkosta tai valitse molemmat power tarvikkeiden keskeytyskohdasta power, EBOD kehyksen suljetaan heti ja ensisijainen kehyksen suljetaan siisti ja valvottu tavalla. Tarvita, laitteen käynnistyy automaattisesti.

Jos potenssiin pois päältä manuaalisesti, ota seuraavat toimet voit palauttaa power järjestelmä.

1. Ota käyttöön EBOD kehys.

2. Kun EBOD kehyksen on käytössä, ota käyttöön ensisijainen kehys.

### <a name="power-loss-on-both-power-supplies-on-ebod-enclosure"></a>Molempien power toimistotarvikkeita EBOD kehyksen Power tappio

Kun määrität oman kaapeli, sinun on varmistettava, että EBOD on aina yhteydessä yksin erillisessä PDU. Jos ensisijainen kehys ja EBOD epäonnistuvat samalla kertaa, järjestelmä palauttaa.

Jos vain EBOD kehyksen epäonnistuu sekä power tarvikkeiden, järjestelmä ei palauta automaattisesti. Tee järjestelmän ottaminen käyttöön ja palauttaa laitteen kunnossa tilan seuraavasti:

1. Jos ensisijainen kehyksen on otettu käyttöön, siirry Power ja jäähdytys moduulit (PCMs) käytöstä.

2. Odota muutama minuutti järjestelmän Sammuta.

3. Ota käyttöön EBOD kehys.

4. Kun EBOD kehyksen on käytössä, ota käyttöön ensisijainen kehys.

## <a name="turn-on-a-device-after-the-primary-and-ebod-enclosure-connection-is-lost"></a>Laite on ensisijainen jälkeen ottaminen käyttöön ja EBOD kehyksen yhteys katkeaa

Jos yhteys katkeaa valmiustilassa ohjauskoneen ja vastaavan EBOD ohjauskoneen välillä, laite työkirja toimii. Jos järjestelmä aktiivinen ohjauskoneen ja vastaavan EBOD ohjauskoneen välinen yhteys katkeaa, suoritetaan automaattisesti ja laitteen pitäisi jatkaa normaalisti.

Kun Serial liitetty SCSI (SAS) sekä kaapeli on poistettu tai EBOD kehys ja ensisijainen kehyksen välinen yhteys on asiakkaiden pääteistunnot, laite lakkaa toimimasta. Tässä vaiheessa toimimalla seuraavasti.

### <a name="to-turn-on-the-device-after-connection-is-lost"></a>Voit ottaa laitteeseen, kun yhteys katkeaa

1. Käyttää laitteen taustalle.

2. Jos välinen EBOD kehys ja ensisijainen kehyksen SAS kaapeli-yhteys katkeaa, kaikki Suojaussidokset kaistan merkkivalot EBOD kehys-on käytöstä.

3. Sammuta Power ja jäähdytys moduulit (PCMs) EBOD-kehys ja ensisijainen kehys.

4. Odota, kunnes kaikki sekä liitteet takapuolella valot käytöstä.

5. Aseta SAS kaapeli ja varmista, että on hyvä EBOD kehys ja ensisijainen kehyksen välinen yhteys.

6. Ota käyttöön EBOD kehyksen ensin mukaan sekä PCM kääntäminen vaihtaa käytössä.

7. Varmista, että EBOD kehys valitsemalla vihreä LED on käytössä on otettu käyttöön.

8. Ota perus kehys.

9. Varmistaa, että ensisijainen kehys valitsemalla ohjauskoneen vihreä LED on käytössä on otettu käyttöön.

10. Tarkista, että ensisijainen kehyksen EBOD kehyksen yhteys on hyvä valitsemalla SAS kaistan merkkivalot (neljä EBOD ohjauskoneen kohden) ovat kaikki ON.

>[AZURE.IMPORTANT] Jos SAS-kaapeli on viallinen tai EBOD kehys ja ensisijainen kehyksen välinen yhteys on hyvä ei, kun otat käyttöön järjestelmä, se siirtyy palautustilaan. Jos näin käy, ota [yhteyttä Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) .

## <a name="turn-off-a-running-device"></a>Käynnissä olevan laitteen poistaminen käytöstä

Käynnissä StorSimple laite on ehkä suljettava, jos se siirretään, poistetaan käytöstä, tai siinä on viallinen osa, joka on vaihdettava. Vaiheet ovat erilaiset riippuen siitä, onko StorSimple laite 8100 tai 8600-malli. 8100 on yksi ensisijainen kehys 8600 on kaksi kehyksen laitetta, jonka ensisijainen kehys ja EBOD kehyksen. Tässä osassa kerrotaan vaihe vaiheelta, miten käynnissä olevan laitteen Sammuta.

- [Ensisijainen kehyksen laitteella](#8100a)

- [Laitetta, jonka EBOD kehys](#8600a)

### <a name="device-with-primary-enclosure-a-name8100a"></a>Ensisijainen kehyksen laitteella<a name="8100a"> 

Sulkeutumaan laitteen siisti ja valvottu tavalla voit tehdä sen Azure perinteinen yritysportaalin kautta tai Windows PowerShellin kautta StorSimple varten. 

>[AZURE.IMPORTANT] Suljettu käynnissä olevan laitteen laitteen takapuolella power-painikkeella.
>
>Varmista ennen suljetaan laitteen, että laitteen osat ovat kunnossa. Siirry Azure perinteinen portal **laitteet** > **ylläpito** > **Laitteiston tila**ja varmista, että kaikkien osien tilan vihreä. Tämä pätee vain kunnossa Systemin. Jos järjestelmä on parhaillaan Sammuta toimintahäiriö osan, näkevät epäonnistui (punainen) tai (keltainen) tila vastaaviin osan **Laitteiston tila**heikentää.

Kun käytät Windows PowerShellin StorSimple tai Azure perinteinen portal-ohjeiden mukaisesti [StorSimple laitteen Sammuta](storsimple-manage-device-controller.md#shut-down-a-storsimple-device). 

### <a name="device-with-ebod-enclosure-a-name8600a"></a>Laitetta, jonka EBOD kehys<a name="8600a">

>[AZURE.IMPORTANT] Ennen kuin suljetaan ensisijainen kehys ja EBOD kehys, varmista, että laitteen osat ovat kunnossa. Siirry Azure perinteinen portal **laitteet** > **ylläpito** > **Laitteiston tila**ja varmista, että kaikki osat ovat kunnossa.

#### <a name="to-shut-down-a-running-device-with-ebod-enclosure"></a>Sulkeutumaan käynnissä laitetta, jonka EBOD kehys

1. Noudata kaikki ensisijainen kehys, [StorSimple laitteen Sammuta](storsimple-manage-device-controller.md#shut-down-a-storsimple-device) lueteltuja vaiheita.

2. Kun ensisijainen kehyksen suljetaan, Sammuta EBOD mukaan kääntäminen Power ja jäähdytys moduuli (PCM) valitsimet pois käytöstä.

3. Voit varmistaa, EBOD on Sammuta, tarkista, että kaikki valot EBOD kehyksen takapuolella ovat poissa käytöstä.

>[AZURE.NOTE] SAS kaapeli, joita käytetään yhteyden EBOD kehyksen ensisijainen kehyksen olisi ei poisteta ennen sen jälkeen, kun järjestelmä suljetaan.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos käytössä ilmenee ongelmia, kun ottaminen käyttöön tai StorSimple laitteen suljetaan [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) .

