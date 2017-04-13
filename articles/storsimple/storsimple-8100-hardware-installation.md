<properties
   pageTitle="Asenna StorSimple 8100-laite | Microsoft Azure"
   description="Tässä artikkelissa käsitellään purkaminen ja hyllykköä asennustapa kaapeli StorSimple 8100 laite, ennen kuin ottaminen käyttöön ja määrittää ohjelmiston."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="unpack-rack-mount-and-cable-your-storsimple-8100-device"></a>Pura, Teline-käyttöönotto- ja StorSimple 8100 laitteen kaapeli

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure-StorSimple-8100 on yksittäinen kehys, Teline on otettu käyttöön laitteessa. Tässä opetusohjelmassa kerrotaan, miten voit purkaa, Teline-asennustapa ja kaapeli StorSimple 8100 laitteisto ennen kuin voit määrittää ja ottaa käyttöön StorSimple laite.

## <a name="unpack-your-storsimple-8100-device"></a>Pura StorSimple 8100-laitteeseen

Seuraavissa vaiheissa olevia Tyhjennä, yksityiskohtaiset ohjeet purkaa StorSimple 8100 tallennustilan laitteen. Laite on toimitettu yksittäisen ruudun.

### <a name="prepare-to-unpack-your-device"></a>Voit purkaa laitteen valmisteleminen

Ennen kuin voit purkaa laitteen, lue seuraavat tiedot.

![Varoitus-kuvake](./media/storsimple-safety/IC740879.png)![paksu paino kuvake](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Varoitus!**

1. Varmista, että sinulla on kaksi henkilöä voi hallita kehyksen leveyden ovat käsittely manuaalisesti. Täysin määritetyssä kehyksen voit arvioida enintään 32 kg (70 kg.).
1. Sijoittaa luetteloruudun tasainen, tason pinnalla.

Seuraavaksi on tehtävä seuraavat toimet purkaa laitteen.

#### <a name="to-unpack-your-device"></a>Laitteen purkaminen

1. Tarkasta ruutu ja pakkaus vaahto crushes, Leikkaa, veden vioittuneet tai ilmeisimmät vioittuneet. Jos ruutu tai pakkaus on vakavasti vioittunut, älä avaa ruutuun. Ota avulla voit arvioida, onko laite on hyvä toimintakunnossa, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) .

2. Pura ruutuun. Seuraava kuva esittää StorSimple laitteen purettu näkymän.

     ![Tallennustilan laitteen purkaminen](./media/storsimple-8100-hardware-installation/HCSUnpackyour2Udevice.png)

    **Tallennustilan laitteen purettu näkymä**

     Otsikko | Kuvaus
     ----- | -------------
     1     | Pakkaus-ruutu
     2     | Ala vaahtoa
     3     | Laite
     4     | Ylimmät vaahtoa
     5     | Accessory valinta


3. Jälkeen purkamisen ruutuun, varmista, että sinulla on:

   - 1 yksittäisen kehys-laite
   - 2 power langat
   - 1 crossover Ethernet-kaapelin
   - 2 peräkkäiset console-kaapeli
   - sarja käytön 1 sarja USB-muunnin
   - 1 merkitsee T10 ruuvimeisseli
   - 4 QSFP-,-sovittimet SFP + 10 GbE verkkoliittymät käytettäväksi
   - 1 Teline-käyttöönoton kit (2 puoli kulkevia käyttöönottaminen laitteen kanssa)
   - Käytön aloittaminen-asiakirjat

    Jos jokin edellä mainittujen kohteista ei mennyt [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md).

Seuraava vaihe on Teline-käyttöönoton laitteen.

## <a name="rack-mount-your-storsimple-8100-device"></a>Teline-käyttöönoton StorSimple 8100-laitteeseen

Noudattamalla seuraavia ohjeita noudattamalla asentaa StorSimple 8100 tallennustilan laitteen vakio 19 tuuman-Teline edessä ja takana viestit. StorSimple 8100 laitteessa on yksi ensisijainen kehys.

Asennus sisältää useita vaiheita, joista jokaisella on kuvattu seuraavasti.

> [AZURE.IMPORTANT]
StorSimple laitteet on oltava käyttämiseen Teline otettu käyttöön.

### <a name="prepare-the-site"></a>Sivuston valmisteleminen

Laite on oltava asennettuna vakio 19 tuuman Teline, jossa on edessä ja takana viestit. Seuraavien ohjeiden avulla voit valmistella Teline asennusta varten.

#### <a name="to-prepare-the-site-for-rack-installation"></a>Sivuston valmisteleminen Teline-asennus

1. Varmista, että laitteen päällä turvallisesti tasainen, vakaata ja tason työmäärää pinta (tai vastaava).

2. Varmista, että sivuston, jos aiot määrittää on vakio AC power riippumattomaksi tietolähteeksi tai Teline power jakauman yksikkönä (PDU), jossa on UPS (-laitetta).

3. Varmista, että yhden 2U paikkaa on käytettävissä Teline, jossa oletat käyttöönottoon laite.

![Varoitus-kuvake](./media/storsimple-safety/IC740879.png)![paksu paino kuvake](./media/storsimple-8100-hardware-installation/HCS_HeavyWeight_Icon.png) **Varoitus!**

Varmista, että sinulla on kaksi henkilöä voi hallita leveyden ovat käsittely laitteen asennus manuaalisesti. Täysin määritetyssä kehyksen voit arvioida enintään 32 kg (70 kg.).

### <a name="rack-prerequisites"></a>Teline edellytykset

8100 kehyksen on suunniteltu vakio 19 tuuman Teline seinäkaappi asennusta varten:

- Pienin syvyys-Teline kirjaa kirjaa 27.84 tuumaa.
- Suurin sallittu paino 32 kg laitteen
- 5 Pascal (0,5 mm veden mittari) suurin vastapaine.

### <a name="rack-mounting-rail-kit"></a>Teline käyttöönotto rail kit

Joukko käyttöönottaminen kulkevia annetaan 19 tuuman Teline cab käytettäväksi. Kiskojen on testattu käsittelemään suurin kehys haluamasi leveys. Nämä kulkevia sallii myös useita liitteitä Teline tilaa menettämättä asennuksen.


#### <a name="to-install-the-device-on-the-rails"></a>Jos haluat asentaa laitteen kiskojen

2. Suorita tätä vaihetta, vain, jos sisemmän kulkevia ei ole asennettu laitteessasi. Yleensä sisemmän kulkevia asentuvat valmiiksi. Jos kulkevia ei ole asennettu, asenna vasemmalle rail- ja oikea rail diojen kehyksen alustan puolilta. Ne liittää avulla kuuden metrisillä ruuvit kummallakin puolella. Ohjeen suunta-rail diat on merkitty **LH – eteen** ja **SK – eteen**ja loppuun, joka on kiinnitetty kehyksen taaksepäin on pää.<br/>

    ![Rail diojen liittäminen kehyksen alustan](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoEnclosureChassis.png)
    **kehyksen reunan Attaching sisemmän rail diat**

       Otsikko | Kuvaus
       ----- | -----------
       1     | M 3 x 4 painike head ruuvit
       2     | Alustan diat

3. Liittää vasemman ulkoliitoksen rail ja oikean ulkoliitoksen rail laitekokonaisuuksien Teline cabinet pystysuora jäsenet. Sulkeiden on merkitty **LH**, **SK**ja **Tämä puoli ylöspäin** opastavat oikeaan asentoon.

4. Etsi edessä ja takana olevan rail-kokoonpanossa rail-PIN. Laajentaa rail Teline-viestien väliin ja lisää PIN edessä ja takana Teline kirjaa pystysuora jäsenen ratkaisuja. Varmista, että rail-kokoonpanossa on taso.

5. Kaksi annettu metrisillä ruuvit avulla voit suojata rail kokoonpano Teline pystysuora jäsenet. Käytä yhden jossa etu- ja yksi taaksepäin.

6. Toista nämä vaiheet rail-kokoonpanossa.<br/>

     ![Liittäminen Teline cabinet rail diat](./media/storsimple-8100-hardware-installation/HCSAttachingRailSlidestoRackCabinet.png)

    **Ulomman rail kokoonpanot liittäminen Teline**

     Otsikko | Kuvaus
     ----- | -----------
     1     | Jossa pienentäminen
     2     | Neliö rei eteen Teline kirjaa jossa
     3     | Vasen rail etupuoli sijainti PIN
     4     | Jossa pienentäminen
     5     | Vasen rail taka sijainti PIN


### <a name="mounting-the-device-in-the-rack"></a>Laitteen Teline käyttöönottaminen

Käyttämällä Teline kiskojen, joka asennettiin vain suorittamalla seuraavat vaiheet Teline laitteen ottaminen käyttöön.

#### <a name="to-mount-the-device"></a>Laitteen ottaminen käyttöön

1. Avustajan Nosta kehys ja tasassa Teline kiskojen.

2. Aseta huolellisesti laitteen kiskojen ja sitten push-laitteen kokonaan kyselyjä Teline cabinet.<br/>

    ![Laitteen lisääminen Teline](./media/storsimple-8100-hardware-installation/HCSInsertingDeviceintheRack.png)

    **Laitteen Teline käyttöönottaminen**


3. Poista vasen ja oikea etu laipan caps minäkin vapaa caps. Laipan caps Kohdista yksinkertaisesti ruiskun sivulle.

5. Suojatun kehys Teline-asentamalla yhden annettu Phillips pää-jossa kunkin laipan vasemman ja oikean kautta.

4. Asenna laipan caps painamalla ne paikoilleen ja kohdistus ne paikassa.<br/>

     ![Laipan caps asentaminen](./media/storsimple-8100-hardware-installation/HCSInstallingFlangeCaps.png)

    **Laipan caps asentaminen**

     Otsikko | Kuvaus
     ----- | -----------
     1     | Kehyksen kiskojen jossa

Seuraavaksi kaapeli laitteesi power, verkon ja serial access.

## <a name="cable-your-storsimple-8100-device"></a>Kaapeli StorSimple 8100-laitteeseen

Seuraavien kerrotaan, miten kaapeli StorSimple 8100 laitteen power, verkon ja serial yhteydet.

### <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat laitteen kaapeli, sinun on:

- Tallennustilan laitteissa, kokonaan avattaviksi ja Teline otettu käyttöön.

- 2 laitteen mukana tulleita power-kaapeli

- Access 2 Power jakauman yksikkö (suositus).

- Verkkokaapelit

- Jos kaapeli

- Serial USB-muunnin tarvittavat ohjaimella asennettuna tietokoneeseen (tarvittaessa)

- Jos 4 QSFP-,-sovittimet SFP + 10 GbE verkkoliittymät käytettäväksi

- [Tuetut laitteet 10 GbE-liittymät StorSimple laitteen](storsimple-supported-hardware-for-10-gbe-network-interfaces.md)


### <a name="power-cabling"></a>Power-kaapeli

Laitteen sisältää tarpeettomat Power ja jäähdytys moduulit (PCMs). Sekä PCMs on oltava asennettuna ja eri power lähteistä, jos haluat varmistaa suuren käytettävyyden yhteydessä.

Seuraavien toimien avulla kaapeli laitteesi Power.

[AZURE.INCLUDE [storsimple-cable-8100-for-power](../../includes/storsimple-cable-8100-for-power.md)]

### <a name="network-cabling"></a>Verkkokaapeli

Laite on aktiivinen valmius-määritys: kerrallaan, yksi ohjain moduuli on aktiivinen ja käsittelyn kaikki levy ja verkko-toiminnot aikana controller-moduulin valmiustilassa. Jos valvonta on epäonnistuu, valmius ohjauskoneen aktivoidaan heti ja jatkaa levyn ja verkon toimintoja.

Tukemaan tarpeettomat controller-automaattisesti, sinun täytyy kaapeli laitteen verkon kuvatulla tavalla seuraavien ohjeiden mukaisesti.

#### <a name="to-cable-for-network-connection"></a>Jos haluat verkkoyhteyden kaapeli

1. Laitteessasi on kuusi verkkoliittymät kunkin ohjaimen: neljä 1 Gbps ja kaksi 10 Gbps Ethernet portit. Määritä eri tietojen porttien laitteen backplane-liitäntä.

    ![Backplane-liitäntä 8100 laitteen](./media/storsimple-8100-hardware-installation/HCSBackplaneof2UDevicewithPortsLabeled.jpg)

    **Takaisin laite, jossa näkyy tietoliikenteen portit**

     Otsikko   | Kuvaus
     ------- | -----------
     0,1,4,5 |  1 GbE verkon liityntäkohdat
     2,3     | 10 GbE verkon liityntäkohdat
     6       | Peräkkäiset portit

2. Katso, verkkokaapeli seuraavassa kaaviossa. (Sininen yhtenäiset viivat näkyy verkon määritys. Suuri käytettävyys ja suorituskyvyn edellyttämät lisämäärityksiä näkyy pisteviivoja.)


    ![Kaapeli 2U laitteen verkossa](./media/storsimple-8100-hardware-installation/HCSCableYour2UDeviceforNetwork.png)

    **Verkon laitteeseesi kaapeli**


  	|Otsikko | Kuvaus |
  	|----- | ----------- |
  	| A    | Internet-yhteyden Lähiverkon |
  	| B    | Controller 0 |
  	| C    | PCM 0 |
  	| D    | Ohjaimen 1 |
  	| E    | PCM 1 |
  	| F G | Isännät |
  	| 0 – 5  | Verkon liityntäkohdat |



Kun kaapeli laitteen edellyttää vähintään määritys:


- Vähintään kaksi verkkoliittymät yhdistetty kunkin ohjaimen jokin pilvikäyttöä varten ja toinen iSCSI. TIETOJA 0 portti on otettu käyttöön ja määritetty serial konsolin laitteen kautta. TIETOJA 0, lukuun ottamatta toiseen tietojen porttiin myös varten on määritettävä palvelun Azure perinteinen-portaalissa. Tässä tapauksessa 0 tietojen yhdistäminen portin ensisijainen Lähiverkon (Verkko ja Internet-yhteyttä). Tiedot-portit voidaan yhdistää SAN/iSCSI Lähiverkon (VLAN)-osiossa verkoston tarkoitettu roolin mukaan.

- Identtiset liittymät kunkin ohjaimen yhdistetty varmistamiseksi käytettävyys, jos ohjauskoneen vikasietotila ilmenee samassa verkossa. Esimerkiksi jos haluat yhdistää tietoja 0 ja tietojen 3 johonkin ohjaimet, haluat yhdistää muiden ohjaimen vastaavat tiedot 0 ja tieto 3.

Ota huomioon hyvän käytettävyyden ja suorituskyvyn:


- Jos mahdollista, Määritä kullekin ohjaimen kahdet verkkoliittymän pilvikäyttöä (1 GbE) varten ja toinen pari iSCSI (10 GbE suositeltava) varten.

- Jos mahdollista, muodostaa verkkoliittymät kunkin ohjauskoneen kaksi eri valitsimet varmistamiseksi käytettävyys vastaan valitsin epäonnistui. Kuvassa kaksi 10 GbE verkon liittymät, tietojen 2 ja 3 tietojen lähettämät kunkin kaksi eri valitsimet yhteydessä.

Saat lisätietoja viitata **verkkoliittymät** [StorSimple laitteen suuren käytettävyyden koskevat vaatimukset](storsimple-system-requirements.md#high-availability-requirements-for-storsimple).

>[AZURE.NOTE] Jos käytössäsi on SFP + enintään 10 GbE verkon liittymään, käyttämällä annettua QSFP-SFP + sovittimet. Jos haluat lisätietoja, siirry [10 GbE-liittymät StorSimple laitteen Tuetut laitteet](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).



### <a name="serial-port-cabling"></a>Sarja-kaapeli

Seuraavien toimien avulla kaapeli sarja portti.

#### <a name="to-cable-for-serial-connection"></a>Kaapeli sarja-yhteyden avulla

1. Laitteessasi on sarja porttiin kunkin ohjauskoneen, tunnistetaan jakoavain-kuvaketta. Lisätietoja on kuva, Etsi laitteen backplane-liitäntä serial porttien [kaapeli verkko](#network-cabling) -osassa.

2. Määritä laitteen backplane-liitäntä aktiivinen-ohjain. Vilkkuva sininen LED osoittaa, että ohjain on aktiivinen.

3. Annettu kaapeli (jos tarpeen, USB-sarja muuntimen kannettavassa tietokoneessa), ja Yhdistä konsolissa tai tietokoneen (jossa on pääte-emuloinnin laitteeseen) active ohjauskoneen sarja-porttiin.

4. Asenna tietokoneeseen sarja-USB-ohjaimia (laitteen mukana toimitettu).

5. Sarja-yhteyden määrittäminen seuraavasti: 115 200 siirtonopeuden, 8 tietojen bittien, Lopeta 1-bittinen, ei ole välistä eroa ja virtaussäätimiä asetuksena on ei mitään.

6. Tarkista, että yhteys toimii painamalla Enter-konsolin. Sarja konsoli-valikosta pitäisi näkyä.

>[AZURE.NOTE] **Lights-Out hallinta**: kun laite on asennettu remote joten tai tietokoneen huoneessa, jonka käyttöoikeuksia on rajoitettu, varmista, että molemmat ohjaimet serial yhteyden aina yhteydessä serial konsolin Vaihda tai vastaavien laitteiden. Näin Luokittele kaukosäätimen ja tukea toimintoja, jos verkon keskeytyksiä tai odottamattomia virheitä.

Laite on nyt kytketty power, verkkokäytön ja serial yhteys. Seuraava vaihe on ohjelmiston ja laitteen käyttöönotto.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [käyttöönotto](storsimple-deployment-walkthrough.md)ja määrittäminen paikallista StorSimple laitteen.
