<properties 
   pageTitle="Valvoa StorSimple-laitteeseen | Microsoft Azure"
   description="Tässä artikkelissa käsitellään StorSimple hallintapalvelu avulla voit valvoa i/o suorituskyvyn, kapasiteetin, verkon suorituskykyä ja laitteen suorituskykyä."
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
   ms.date="08/16/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Valvoa StorSimple laitteen StorSimple hallinta-palvelun avulla 

## <a name="overview"></a>Yleiskatsaus

Voit käyttää StorSimple hallintapalvelu seurannassa laitteiden StorSimple-ratkaisussa. Voit luoda mukautetun kaavioiden i/o suorituskyvyn, kapasiteetin, verkon suorituskykyä ja laitteen suorituskyvyn mittarit perusteella. 

Voit tarkastella tietyn laitteen seurantatiedot Azure perinteinen portaalissa valitsemalla StorSimple hallintapalvelu. Valitse **Näyttö** -välilehti ja valitse sitten laitteet-luettelosta. **Valvonta** -sivu sisältää seuraavat tiedot.

## <a name="io-performance"></a>I/o suorituskyky 

**I/o suorituskyvyn** raidat arvot liittyvät luku määrän ja kirjoittaa toimintojen välinen joko iSCSI käynnistäjä liittymät isäntäpalvelimen ja laitteen tai laitteen ja pilveen. Tämä suorituskyky voidaan mitata määritetyn aseman, määritetyn aseman säilön tai kaikki aseman säilöt.

Alla olevassa kaaviossa näkyy i/o laitteeseen, saat kaikki tuotannon laitteen asemat käynnistäjä varten. Piirretty arvot luetaan ja kirjoittaa tavua sekunnissa, lukeminen ja kirjoittaminen IO toimintoja sekunnissa, ja lukea ja kirjoittaa viiveet suurempia.

![IO suorituskyky käynnistäjä laitteeseen](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Saman laitteen äänenvoimakkuuden säilöjen pilveen laitteen tiedot piirretään i/o-toimintoja. Tässä laitteessa tiedot ovat vain lineaarinen taso ja mitään on spilled pilveen. Luku-kirjoitus-työvaiheita ei ole määrä laitteesta pilveen. Siksi kaavion päät ovat 5 minuutin välein, joka vastaa korkojakso, jossa on valittuna uudelleenaktivoinnin yhteydessä laitteen ja palvelun välillä. 

![IO suorituskyvyn laitteesta pilveen](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)


Saman laitteen cloud tilannevedoksen otettiin aseman tietojen aloittaen 2:00 pm. Tämä tuloksena juoksutus laitteesta pilveen tiedot. Lukujen kirjoituksia on served pilveen tämä kesto. IO kaavio näyttää huippu vastaavan kellonajan, jolloin tilannevedoksen on otettu eri arvot. 

![Laitteen cloud cloud tilannevedoksen jälkeen IO suorituskyky](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)


## <a name="capacity-utilization"></a>Kapasiteetin käyttö 

**Kapasiteetin** seuraa, jota käytetään tietomääristä, äänenvoimakkuuden säilöjen tai laitteen tietojen tallennustilan määrää liittyvät arvot. Voit luoda raportteja kapasiteetin käyttö ensisijainen tallennustilaa, cloud-tallennustilan tai laitteen tallennustilan perusteella. Kapasiteetin käyttö voidaan mitata määritetyn aseman, määritetyn aseman säilön tai kaikki aseman säilöt.


Perus-pilvi ja tallennustilaa laitteen voidaan kuvata seuraavasti:

###<a name="primary-storage-capacity-utilization"></a>Ensisijainen tallennustilan kapasiteetin käyttö
 
Kaavioista Näytä kirjoitettu StorSimple asemat ennen tietojen deduplicated ja pakattujen tietojen määrää. Voit tarkastella ensisijainen tallennustilan käyttö kaikki asemat tai yksittäisen aseman.

Kun kaikki asemat ja jokaiseen yksittäisiä ensisijainen tallennustilan aseman kapasiteetin käyttö kaavioiden tarkasteleminen ja tällaisissa tapauksissa ensisijainen tiedot yhteen, kahden luvun välisen saattaa olla ristiriita. Kaikki asemat yhteensä ensisijainen tietoja ei voi lisätä yksittäisiä tietomääristä ensisijainen tietojen kokonaismäärä. Syynä voi olla jokin seuraavista:

- **Kaikki levyasemien tilannevedoksen tietojen**: Tämä ongelma näkyy vain, jos sinulla on käytössä Update 3 aiempi versio. Ensisijainen tiedot näkyvät kaikki asemat on ensisijainen tiedot jokaisen aseman ja tilannevedoksen tiedot. Näyttää ensisijainen tiedot määritetyn aseman vastaa vain kohdistettu aseman tietojen määrä (ja vastaavan aseman tilannevedoksen tiedot eivät sisällä).

    Tämä on esitetty myös seuraavan kaavan avulla:

    *Ensisijainen tiedot (kaikki asemat) = summa (ensisijainen tiedot (aseman i) + tilannevedoksen tietojen (aseman i) koon)*
    
    *Jos ensisijainen tiedot (aseman i) = varattu aseman i ensisijainen tiedon määrä*
 
    Jos tilannevedosten poistetaan palvelun kautta, jos poisto on valmis asynkronisesti taustalla. Voi kestää jonkin aikaa aseman tietojen koko on päivitettävä tilannevedos poistamisen jälkeen. 

    Jos käynnissä päivityksen 3 tai uudempi, äänenvoimakkuuden tiedot sisältyy ei tilannevedoksen tiedot. Ja ensisijainen käyttö lasketaan seuraavasti:

    * Ensisijainen tiedot (kaikki asemat) = summa (ensisijainen tiedot (aseman i)
    
    *Jos ensisijainen tiedot (aseman i) = varattu aseman i ensisijainen tiedon määrä*
 
- **Asemat valvontaan käytöstä kaikki asemat sisältyy**: Jos tietomääristä laitteessa, jonka seuranta on poistettu käytöstä, yksittäisiä asemat seurannan tiedot eivät ole käytettävissä kaavioissa. Kaikki asemat kaavion tiedot kuitenkin sisältää asemista, jonka seuranta on poistettu käytöstä. 
 
- **Poistaa live liittyvät varmuuskopiot mukaan kaikki asemat asemat**: Jos tilannevedoksen tiedot sisältävä asemat poistetaan, mutta liittyvän tilannevedoksia yhä ja valitse näkyviin voi tulla ristiriita.

- **Poistetut asemat mukaan kaikki asemat**: Joissakin tapauksissa vanha tietomääristä saattaa olla, vaikka ne on poistettu. Poisto vaikutus ei nähdä ja laite saattaa näkyä alemman käytettävissä oleva kapasiteetti. Sinun on otettava yhteyttä Microsoftin Support Poista asemat.

Seuraavat pylväskaavioissa näkyvät ensisijainen tallennustilan kapasiteetin käyttö StorSimple laitteen ennen ja jälkeen cloud tilannevedoksen on tehty. Tämä on vain aseman tiedoista, cloud tilannevedoksen Älä muuta ensisijainen tallennustilan. Kuten näet, kaavio näyttää tuloksena ottaen cloud tilannevedoksen ensisijainen kapasiteetin-käyttö ei ole eroa. Cloud tilannevedoksen aloitettu noin 2:00 pm kyseiseen laitteeseen.

![Cloud tilannevedoksen ennen ensisijainen kapasiteetin käyttö](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Ensisijainen kapasiteetin käyttö cloud tilannevedoksen jälkeen](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Jos käytössäsi on Päivitä 2 tai uudempi versio, voit katkaista alaspäin ensisijainen tallennustilan kapasiteetin käyttö yksittäisiä äänenvoimakkuutta, kaikki asemat, kaikki Porrastettu asemat ja kaikki paikallisesti kiinnitetty asemat alla kuvatulla tavalla. Kaikki paikallisesti kiinnitetty asemat mukaan jakamiseen avulla voit varmistaa nopeasti kuinka suuri osa paikallinen ylätasolla on käytetty.

![Kaikki paikallisesti kiinnitetty asemat ensisijainen kapasiteetin käyttö](./media/storsimple-monitor-device/localvolumes.png)


###<a name="cloud-storage-capacity-utilization"></a>Cloud tallennustilan kapasiteetin käyttö

Kaavioista ylityön käytetään pilvitallennustilaa. Tietojen deduplicated ja pakata. Tämä summa sisältää cloud tilannevedoksia, joita saattaa olla tietoja, joita ei ole mitään ensisijainen asemaa näkyvät myös ja on käytettävissä aiempien versioiden tai pakolliset säilytys tarkoituksiin. Voit verrata pääsivujen ja cloud tallennustilan kulutus luvut haluat verrata tietoja vähentäminen nopeus, vaikka määrä ei ole tarkka. Seuraavat pylväskaavioissa näkyvät cloud tallennustilan kapasiteetin käyttö StorSimple laitteen ennen ja sen jälkeen cloud tilannevedoksen on tehty. Cloud tilannevedoksen aloitettu noin 2:00 pm kyseiseen laitteeseen ja näet cloud kapasiteetin käyttö koodin yhtä aikaa lisääntyvien 5.73 Mt 4.04 gigatavua.

![Cloud kapasiteetin käyttö ennen cloud tilannevedos](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Cloud kapasiteetin käyttö cloud tilannevedoksen jälkeen](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)


###<a name="device-storage-capacity-utilization"></a>Laitteen tallennustilan kapasiteetin käyttö

Kaavioista Näytä yhteensä käyttö laitteen, jossa on enemmän kuin ensisijaisen tallennustilan käyttö, koska se sisältää Suoritettaessa lineaarinen taso. Tämä taso sisältää tiedot, jotka on olemassa myös summa laite on muita tasoa. Suoritettaessa lineaarinen tason kapasiteettia on käyttää niin, että uudet tiedot tullessa vanhat tiedot siirretään Kiintolevy tason (jolloin sitä on deduplicated ja pakattu) ja sen jälkeen pilveen.

Ajan myötä ensisijainen kapasiteetin ja laitteen kapasiteetin käyttö todennäköisesti kasvattaa yhdessä kunnes tiedot alkaa tasoisen pilveen. Tässä vaiheessa laitteen kapasiteetin käyttö todennäköisesti alkaa plateau, mutta ensisijainen kapasiteetin käyttö kasvattaa kirjoitetaan enemmän tietoja.

Seuraavat pylväskaavioissa näkyvät ensisijainen tallennustilan kapasiteetin käyttö StorSimple laitteen ennen ja jälkeen cloud tilannevedoksen on tehty. Cloud tilannevedoksen aloitettua 2:00 pm ja laitteen kapasiteetin käytön aloittaminen pienenevillä aikalohkon. Laitteen tallennustilan kapasiteetin käyttö tapahtui-11.58 gt 7.48 gigatavua. Tämä ilmaisee, että todennäköisesti lineaarinen Suoritettaessa tason pakkaamattomat tiedot on deduplicated, pakattu, siirtää Kiintolevy taso. Huomaa, että jos laite on jo paljon tietoja sekä Suoritettaessa Kiintolevy tasoa, et ehkä näe Tämä pienentää. Tässä esimerkissä laitteessa on vähän tietoja.

![Laitteen kapasiteetin käyttö ennen cloud tilannevedos](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Laitteen kapasiteetin käyttö cloud tilannevedoksen jälkeen](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)


## <a name="network-throughput"></a>Verkon suorituskykyä

**Verkon siirtonopeuden** seuraa siirretään iSCSI käynnistäjä verkkoliittymät isäntäpalvelimen ja laitteen ja laitteen ja pilveen välinen tietomäärän liittyvät arvot. Voit valvoa tämän metrijärjestelmä kunkin iSCSI verkkoliittymät laitteessasi.

Seuraavat pylväskaavioissa näkyvät verkon suorituskykyä tietojen 0 ja tietojen 4, sekä 1 verkon GbE-liittymät, laitteessasi. Tässä tapauksessa tietojen 0 on cloud käyttävä tietojen 4 on iSCSI käytössä. Näet sekä saapuvan ja lähtevän tietoliikenteen StorSimple laitteeseesi. Tasainen rivin 3:24 pm lähtien kaaviossa on syistä, että olemme kerätä tietoja vain 5 minuuttia ja ohitetaan. 

![Verkon siirtonopeuden Data4 varten](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Verkon siirtonopeuden Data4 varten](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)


## <a name="device-performance"></a>Laitteen suorituskyky 

**Laitteen suorituskyvyn** seuraa laitteen suorituskykyyn liittyvät arvot. Seuraavassa kaaviossa näkyvät suorittimen käyttö tilasto laitteen tuotannon.

![Laitteen suorittimen käyttö](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja käyttämisestä [StorSimple hallinnan palvelun laitteen Raporttinäkymät-ikkunan](storsimple-device-dashboard.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
