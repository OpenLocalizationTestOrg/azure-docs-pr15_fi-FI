<properties
   pageTitle="Asenna StorSimple 8600-laite | Microsoft Azure"
   description="Tässä artikkelissa käsitellään purkaminen ja hyllykköä asennustapa kaapeli StorSimple 8600 laite, ennen kuin ottaminen käyttöön ja määrittää ohjelmiston."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/24/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8600-device"></a>Pura, Teline-käyttöönotto- ja StorSimple 8600 laitteen kaapeli

## <a name="overview"></a>Yleiskatsaus
Microsoft Azure-StorSimple-8600 on kaksi kehyksen laite ja koostuu ensisijaisen ja EBOD kehyksen. Tässä opetusohjelmassa kerrotaan, miten voit purkaa, Teline-asennustapa ja kaapeli StorSimple 8600 laitteisto ennen kuin voit määrittää StorSimple ohjelmiston.

## <a name="unpack-your-storsimple-8600-device"></a>Pura StorSimple 8600-laitteeseen

Seuraavissa vaiheissa olevia selkeällä ja ohjeita siitä, miten voit purkaa StorSimple 8600 tallennustilan laitteen. Laite on toimitettu yksi ensisijainen kehyksen varten ja toinen EBOD kehyksen ruutuihin. Nämä kaksi ruudut sijoitetaan sitten yksittäisen ruudun.

### <a name="prepare-to-unpack-your-device"></a>Voit purkaa laitteen valmisteleminen

Ennen kuin voit purkaa laitteen, lue seuraavat tiedot.


![Varoitus-kuvake](./media/storsimple-safety/IC740879.png)![paksu paino kuvake](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Varoitus!**

1. Varmista, että sinulla on kaksi henkilöä voi hallita laitetta leveyden ovat käsittely manuaalisesti. Täysin määritetyssä kehyksen voit arvioida enintään 32 kg (70 kg.).
1. Sijoittaa luetteloruudun tasainen, tason pinnalla.

Seuraavaksi on tehtävä seuraavat toimet purkaa laitteen.

#### <a name="to-unpack-your-device"></a>Laitteen purkaminen

1. Tarkasta ruutu ja pakkaus vaahto crushes, Leikkaa, veden vioittuneet tai ilmeisimmät vioittuneet. Jos ruutu tai pakkaus on vakavasti vioittunut, älä avaa ruutuun. Ota avulla voit arvioida, onko laite on hyvä toimintakunnossa, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) .

2. Avaa ulompi-ruutu ja sitten Poista perus- ja EBOD liitteet vastaavan kaksi ruutua. Voit nyt Pura perus- ja EBOD liitteet. Seuraavassa kuvassa on purettu näkymän jokin liitteet.

    ![Tallennustilan laitteen purkaminen](./media/storsimple-8600-hardware-installation/HCSUnpackyour4Udevice.png)

    **Tallennustilan laitteen purettu näkymä**

     Otsikko | Kuvaus
     ----- | -------------
     1     | Pakkaus-ruutu
     2     | SAS kaapeli (-apuohjelmat ja kaapeli ilmaisinalueen)
     3     | Ala vaahtoa
     4     | Laite
     5     | Ylimmät vaahtoa
     6     | Accessory valinta

3. Jälkeen purkamisen kaksi ruutua, varmista, että sinulla on:

  - 1 ensisijainen kehys (ensisijainen kehys ja EBOD kehyksen ovat kaksi eri ruutuja)
  - 1 EBOD kehys
  - 4 virtajohto, vastaaviin ruutuihin 2
  - 2 SAS kaapeli (muodosta yhteys EBOD kehyksen ensisijainen kehyksen)
  - 1 crossover Ethernet-kaapelin
  - 2 peräkkäiset console-kaapeli
  - sarja käytön 1 sarja USB-muunnin
  - 4 QSFP-,-sovittimet SFP + 10 GbE verkkoliittymät käytettäväksi
  - 2 hyllykköä asennustapa-paketit (4 puoli kulkevia käyttöönottaminen laitteisto-2 ensisijainen kehys ja EBOD kehyksen kanssa), 1 vastaaviin ruutuihin
  - Käytön aloittaminen dokumentaatio

    Jos jokin edellä mainittujen kohteista ei mennyt [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).  

Seuraava vaihe on Teline-käyttöönoton laitteen.

## <a name="rack-mount-your-storsimple-8600-device"></a>Teline-käyttöönoton StorSimple 8600-laitteeseen

Noudattamalla seuraavia ohjeita noudattamalla asentaa StorSimple 8600 tallennustilan laitteen vakio 19 tuuman-Teline edessä ja takana viestit. Laitteen mukana kaksi liitteet: ensisijainen kehys ja EBOD kehyksen. Nämä molemmat on Teline-järjestelmään.

Asennus sisältää useita vaiheita, joista jokaisella on kuvattu seuraavasti.

> [AZURE.IMPORTANT]
StorSimple laitteet on oltava käyttämiseen Teline otettu käyttöön.



### <a name="site-preparation"></a>Sivuston valmistelu

Liitteet on oltava asennettuna vakio 19 tuuman Teline, jossa on edessä ja takana viestit. Seuraavien ohjeiden avulla voit valmistella Teline asennusta varten.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Sivuston valmisteleminen Teline-asennus

1. Varmista, että ensisijainen ja EBOD liitteet ovat desinfiointiaineiden turvallisesti tasainen, vakaata ja tason työmäärää pinnalla (tai vastaava).

2. Varmista, että sivuston, jos aiot määrittää on vakio AC power riippumattomaksi tietolähteeksi tai Teline Power jakauman yksikön (PDU) kanssa UPS (-laitetta).

3. Varmista, että yhden 4U (2 X 2U) paikkaa on käytettävissä Teline, jossa haluat ottaa käyttöön liitteet.

![Varoitus-kuvake](./media/storsimple-safety/IC740879.png)![paksu paino kuvake](./media/storsimple-8600-hardware-installation/HCS_HeavyWeight_Icon.png) **Varoitus!**

 Varmista, että sinulla on kaksi henkilöä voi hallita leveyden ovat käsittely laitteen asennus manuaalisesti. Täysin määritetyssä kehyksen voit arvioida enintään 32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Teline edellytykset

Liitteet käsitellään vakio 19 tuuman Teline seinäkaappi asennusta:

- Pienin syvyys-Teline kirjaa kirjaa 27.84 tuumaa
- Suurin sallittu paino 32 kg laitteen
- Suurin vastapaine 5 Pascal (0,5 mm veden mittari).

### <a name="rack-mounting-rail-kit"></a>Teline käyttöönotto rail kit

Joukko käyttöönottaminen kulkevia toimitetaan 19 tuuman Teline cab käytettäväksi. Kiskojen on testattu käsittelemään suurin kehys haluamasi leveys. Nämä kulkevia sallii myös useita liitteitä Teline tilaa menetyksiä ilman asennuksen. Asenna ensin EBOD kehys.

#### <a name="to-install-the-ebod-enclosure-on-the-rails"></a>Jos haluat asentaa kiskojen EBOD kehys

2. Suorita tätä vaihetta, vain, jos sisemmän kulkevia ei ole asennettu laitteessasi. Yleensä sisemmän kulkevia asentuvat valmiiksi. Jos kulkevia ei ole asennettu, asenna vasemmalle rail- ja oikea rail diojen kehyksen alustan puolilta. Ne liittää avulla kuuden metrisillä ruuvit kummallakin puolella. Ohjeen suunta-rail diat on merkitty **LH – eteen** ja **SK – eteen**ja loppuun, joka on kiinnitetty kehyksen taaksepäin on pää.

    ![Liittäminen kehyksen alustan rail diat](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)

    **Liittäminen rail diat kehyksen reunan**

    Otsikko | Kuvaus
    ----- | -----------
    1     | M 3 x 4 painike head ruuvit
    2     | Alustan diat

3. Liittää rail vasemman ja oikean rail laitekokonaisuuksien Teline cabinet pystysuora jäsenet. Sulkeiden on merkitty **LH**, **SK**ja **Tämä puoli ylöspäin** opastavat korjata suuntaa.

4. Etsi edessä ja takana olevan rail-kokoonpanossa rail-PIN. Laajentaa rail Teline-viestien väliin ja lisää PIN edessä ja takana Teline kirjaa pystysuora jäsenen ratkaisuja. Varmista, että rail-kokoonpanossa on taso.

5. Kaksivaiheinen annettu metrisillä ruuvit suojatun rail kokoonpano Teline pystysuora jäsenet. Käytä yhden jossa etu- ja yksi taaksepäin.

6. Toista nämä vaiheet rail-kokoonpanossa.

     ![Liittäminen Teline cabinet rail diat](./media/storsimple-8600-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Rail kokoonpanot liittäminen Teline**

     Otsikko | Kuvaus
     ----- | -----------
     1     | Jossa pienentäminen
     2     | Neliö rei eteen Teline kirjaa jossa
     3     | Postikortin etupuoli rail sijainti PIN vasemmalle
     4     | Jossa pienentäminen
     5     | Vasen taka rail sijainti PIN

### <a name="mounting-the-ebod-enclosure-in-the-rack"></a>EBOD kehys Teline-käyttöönottaminen

Käyttämällä Teline kiskojen, joka asennettiin vain suorittamalla seuraavat vaiheet Teline EBOD-kehyksen ottaminen käyttöön.

#### <a name="to-mount-the-ebod-enclosure"></a>Ota käyttöön EBOD kehys

1. Avustajan Nosta kehys ja tasassa Teline kiskojen.

2. Aseta huolellisesti kehyksen kiskojen ja sitten push se kokonaan kyselyjä Teline cabinet.

    ![Laitteen lisääminen Teline](./media/storsimple-8600-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Kehys Teline-käyttöönottaminen**

3. Poista vasen ja oikea etu laipan caps minäkin vapaa caps. Laipan caps Kohdista yksinkertaisesti ruiskun sivulle.

4. Suojatun kehyksen kyselyjä Teline asentamalla yhden annettu Phillips pää-jossa kunkin laipan vasemman ja oikean kautta.

4. Asenna laipan caps painamalla ne paikoilleen ja kohdistus ne haluamaasi kohtaan.

     ![Laipan caps asentaminen](./media/storsimple-8600-hardware-installation/HCSInstallingFlangeCaps.png)

    **Laipan caps asentaminen**

     Otsikko | Kuvaus
     ----- | -----------
     1     | Kehyksen kiskojen jossa


### <a name="mounting-the-primary-enclosure-in-the-rack"></a>Ensisijainen kehys Teline-käyttöönottaminen

Kun olet käyttöönottaminen EBOD kehys, sinun on käyttöönottoon ensisijainen kehyksen noudattamalla samoja vaiheita.

> [AZURE.NOTE]
>
> - On mahdollista, että muutaman tyhjä paikkojen Teline ensisijainen kehys ja EBOD kehyksen välillä.
> - Ensisijainen kehyksen muodostaa EBOD kehyksen annettu 2m SAS-kaapelin avulla.
> - Ei mitään rajoituksia pään EBOD yksikkö yksikkö suhteellisen sijainnin. Tämän vuoksi ensisijainen kehyksen voi asettaa yläreunan paikka ja alla EBOD kehys, tai päinvastoin.

Seuraavaksi kaapeli laitteesi power, verkon ja serial access.

## <a name="cable-your-storsimple-8600-device"></a>StorSimple 8600 laitteen kaapeli

Seuraavien kerrotaan, miten kaapeli StorSimple 8600 laitteen power, verkon ja serial yhteydet.

### <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat kaapeli laitteessa, sinun on:

- Ensisijainen kehys ja EBOD kehys-kokonaan avattaviksi
- 4 power kaapeli (2 jokaisen ensisijaisen ja EBOD kehyksen) laitteen mukana tulleita
- 2 EBOD kehyksen muodostaa ensisijainen kehyksen laitteen mukana tulleella SAS-kaapeli
- Access 2 Power jakauman yksikkö (PDU) (suositus)
- Verkkokaapelit
- Jos kaapeli
- Sarja USB-muunnin tarvittavat ohjaimella asennettuna tietokoneeseen (tarvittaessa)
- Jos 4 QSFP-,-sovittimet SFP + 10 GbE verkkoliittymät käytettäväksi
- [Tuetut laitteet 10 GbE-liittymät StorSimple laitteen](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)

### <a name="sas-and-power-cabling"></a>Suojaussidosten ja Power kaapeli

Laitteessasi on ensisijainen kehys ja EBOD kehyksen. Tämä edellyttää yksiköt kytketty yhdessä Serial liitetty SCSI (SAS)-yhteyden ja power.

Kun määrität tässä laitteessa ensimmäistä kertaa, Suorita vaiheet SAS kaapeli ensin ja tee sitten ohjeet power kaapeli.

[AZURE.INCLUDE [storsimple-cable-8600-for-SAS](../../includes/storsimple-sas-cable-8600.md)]

[AZURE.INCLUDE [storsimple-cable-8600-for-power](../../includes/storsimple-cable-8600-for-power.md)]

### <a name="network-cabling"></a>Verkkokaapeli

Laite on aktiivinen valmius-kokoonpanossa: kerrallaan, yksi ohjain moduuli on aktiivinen ja käsittelyn kaikki levy ja verkko-toiminnot aikana controller-moduulin valmiustilassa. Ohjauskoneen virheen ilmetessä valmiustilassa ohjauskoneen Aktivoi välittömästi ja jatkaa kaikki levyn ja verkko-toiminnot.

Tukemaan tarpeettomat controller-automaattisesti, sinun täytyy kaapeli laitteen verkoston mukaisesti seuraavien ohjeiden mukaisesti.

#### <a name="to-cable-for-network-connection"></a>Jos haluat verkkoyhteyden kaapeli

1. Laitteessasi on kuusi verkkoliittymät kunkin ohjaimen: neljä 1 Gbps ja kaksi 10 Gbps Ethernet portit. Lisätietoja laitteen backplane-liitäntä tietojen porttien tunnistavan seuraavassa kuvassa.

     ![Backplane-liitäntä 8600: aa laitteen](./media/storsimple-8600-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Takaisin laitteen, jossa näkyy tietoliikenteen portit**

     Otsikko   | Kuvaus
     ------- | -----------
     0,1,4,5 |  1 GbE verkon liityntäkohdat
     2,3     | 10 GbE verkon liityntäkohdat
     6       | Peräkkäiset portit



1. Katso, verkkokaapeli seuraavassa kaaviossa. (Sininen yhtenäiset viivat näkyy verkon määritys. Hyvän käytettävyyden ja suorituskyvyn tarvitaan lisämäärityksiä näkyy pisteviivoja.)

![Kaapeli 4U laitteen verkossa](./media/storsimple-8600-hardware-installation/HCSCableYour4UDeviceforNetwork.png)

**Verkon laitteeseesi kaapeli**

Otsikko | Kuvaus
----- | -----------
A    | Internet-yhteyden Lähiverkon
B    | Controller 0
C    | PCM 0
D    | Ohjaimen 1
E    | PCM 1
F    | EBOD controller 0
G    | EBOD ohjauskoneen 1
H, I  | Isännät (esimerkiksi tiedostopalvelimet)
0 – 5  | Verkon liityntäkohdat
6    | Ensisijainen kehys
7    | EBOD kehys

Kun kaapeli laitteen edellyttää vähintään määritys:


- Vähintään kaksi verkkoliittymät yhdistetty kunkin ohjaimen jokin pilvikäyttöä varten ja toinen iSCSI. TIETOJA 0 portti on otettu käyttöön ja määritetty serial konsolin laitteen kautta. TIETOJA 0, lukuun ottamatta toiseen tietojen porttiin myös varten on määritettävä palvelun Azure perinteinen-portaalissa. Tässä tapauksessa 0 tietojen yhdistäminen portin ensisijainen Lähiverkon (Verkko ja Internet-yhteyttä). Tiedot-portit voidaan yhdistää SAN/iSCSI Lähiverkon (VLAN)-osiossa verkoston tarkoitettu roolin mukaan.

- Identtiset liittymät kunkin ohjaimen yhdistetty varmistamiseksi käytettävyys, jos ohjauskoneen vikasietotila ilmenee samassa verkossa. Esimerkiksi jos haluat yhdistää tietoja 0 ja tietojen 3 johonkin ohjaimet, haluat yhdistää muiden ohjaimen vastaavat tiedot 0 ja tieto 3.

Ota huomioon hyvän käytettävyyden ja suorituskyvyn:


- Jos mahdollista, Määritä kullekin ohjaimen kahdet verkkoliittymän pilvikäyttöä (1 GbE) varten ja toinen pari iSCSI (10 GbE suositeltava) varten.

- Jos mahdollista, muodostaa verkkoliittymät kunkin ohjauskoneen kaksi eri valitsimet varmistamiseksi käytettävyys vastaan valitsin epäonnistui. Kuvassa kaksi 10 GbE verkon liittymät, tietojen 2 ja 3 tietojen lähettämät kunkin kaksi eri valitsimet yhteydessä. Saat lisätietoja viitata **verkkoliittymät** [StorSimple laitteen suuren käytettävyyden koskevat vaatimukset](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Jos käyttäminen SFP + enintään 10 GbE verkon liittymään, käyttämällä annettua QSFP-SFP + sovittimet. Jos haluat lisätietoja, siirry [10 GbE-liittymät StorSimple laitteen Tuetut laitteet](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

### <a name="serial-port-cabling"></a>Sarja-kaapeli

Seuraavien toimien avulla kaapeli sarja portti.

#### <a name="to-cable-for-serial-connection"></a>Kaapeli sarja-yhteyden avulla

1. Laitteessasi on sarja porttiin kunkin ohjauskoneen, tunnistetaan jakoavain-kuvaketta. Voit etsiä serial portit, viitata kuva, joka näyttää tiedot portit laitteen takapuolella.

2. Määritä laitteen backplane-liitäntä aktiivinen-ohjain. Vilkkuva sininen LED osoittaa, että ohjain on aktiivinen.

3. Käyttämällä annettua kaapeli (jos tarpeen, USB-sarja muuntimen kannettavassa tietokoneessa) ja Yhdistä-konsolissa tai tietokoneen (jossa on pääte-emuloinnin laitteeseen) että portti aktiivinen ohjauskoneen.

4. Asenna tietokoneeseen sarja-USB-ohjaimia (laitteen mukana toimitettu).

5. Sarja-yhteyden määrittäminen seuraavasti:
   - 115 200 siirtonopeuden
   - 8 tietojen bittien
   - Lopeta 1-bittinen
   - Ei ole välistä eroa
   - Virtaussäätimiä **ei mitään**

6. Tarkista, että yhteys toimii painamalla Enter-konsolin. Sarja konsoli-valikosta pitäisi näkyä.

> [AZURE.NOTE] **Lights-Out hallinta:** Kun laite on asennettu remote joten tai tietokoneen huoneessa, jonka käyttöoikeuksia on rajoitettu, varmista, että molemmat ohjaimet serial yhteyden aina yhteydessä serial konsolin Vaihda tai vastaavien laitteiden. Näin Luokittele kaukosäätimen ja tukea toimintojen verkon häiriöt tai odottamattomia virheitä.

Olet suorittanut kaapeli laitteesi power, verkkokäytön ja serial yhteys. Seuraavaksi voit määrittää ohjelmiston laitteessasi.

## <a name="next-steps"></a>Seuraavat vaiheet

Olet nyt valmiina [käyttöönotto](storsimple-deployment-walkthrough-u2.md)ja määrittäminen paikallista StorSimple laitteen.
