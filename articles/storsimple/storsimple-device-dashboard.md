<properties
   pageTitle="Käytä StorSimple hallinnan laitteen Raporttinäkymät-ikkunan | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple hallinnan palvelun laitteen Raporttinäkymät-ikkunan ja miten sitä käytetään tarkasteleminen tallennustilan arvot ja yhdistetyn laitteistokäynnistäjät ja Etsi sarjanumero ja IQN."
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
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-device-dashboard"></a>StorSimple hallinta laitteen Raporttinäkymät-ikkunan käyttäminen

## <a name="overview"></a>Yleiskatsaus

StorSimple hallinta laitteen Raporttinäkymät-ikkunan avulla voit tietyn StorSimple laitteen, toisin kuin palvelun Raporttinäkymät-ikkunan, jotka antavat tietoja kaikista laitteiden Microsoft Azure StorSimple-ratkaisun tietojen yhteenveto.

![Laitteen dashboard-sivu](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Koontinäytön sisältää seuraavat tiedot:

- **Kaavioalue** – voit tarkastella kaavioalueen Raporttinäkymät-ikkunan yläreunassa: asiaa tallennustilan-arvot. Tässä kaaviossa voit tarkastella mittaukset ensisijainen kokonaistallennustilaa (isännät laitteeseen kirjoittamat tiedot summa) ja muiden laitteiden kulutettu ajanjakson aikana yhteensä pilvitallennustilaa.

     Tässä yhteydessä *ensisijainen tallennustilan* kokonaismäärä isäntä kirjoitetut tiedot viittaa ja voidaan jakaa aseman tyypin mukaan: *ensisijainen Porrastettu tallennustilan* sisältää sekä paikallisesti tallennetut tiedot ja tietoja tasoisen pilveen; *ensisijainen paikallisesti kiinnitetyt tallennustilan* sisältää vain paikallisesti tallennetut tiedot. *Pilvitallennustilaa*on toisaalta, mittaaminen pilveen tallennettujen tietojen määrä. Tämä sisältää Porrastettu tiedot ja varmuuskopiot. Huomaa, että pilveen tallennettujen tietojen deduplicated taas pakata, ensisijainen tallennustilan osoittaa tallennustilan, ennen kuin tiedot deduplicated ja pakata. (Voit verrata nämä kaksi lukua, saat pakkaamisen korko vertailla.) Molemmat ensisijainen varten ja määrität seuranta korkojakso perustuu pilvitallennustilaa, summat. Esimerkiksi jos valitset yhden viikon korkojakso, sitten kaavio näyttää tiedot päivittäinen edellisellä viikolla.

     Voit määrittää kaavion seuraavasti:

     - Saat pilvitallennustilaa ajan kuluessa Kulutettu määrä-vaihtoehdon **CLOUD TALLENNUSTILAA käytetty** . Voit tarkastella kokonaistallennustilaa, joka on kirjoitettu isäntä, valitse **Ensisijainen TASOISEN TALLENNUSTILAN käyttää** ja **Ensisijainen PAIKALLISESTI KIINNITETYT TALLENNUSTILAN käyttää** asetukset. Kuvassa molemmat vaihtoehdot on valittu. Tämän vuoksi kaavio näyttää cloud ja ensisijainen tallennustilan tallennustilan summat. Huomaa, että kaikki ensisijainen tallennuspaikka, ennen päivitystä 2: n asentamisen edustaa **Ensisijainen TASOISEN TALLENNUSTILAA käytetty** rivi.
     - Kaavion oikeassa yläkulmassa avattavasta valikosta avulla voit määrittää 1 viikko, 1 kuukausi, 3 kuukautta tai 1 vuoden ajan kuluessa. Huomaa, että ylimmän tason kaavio päivitetään vain kerran päivässä ja näin ollen vaikuttavat edelliseen päivään summat.

     Lisätietoja on artikkelissa [StorSimple hallintapalvelu StorSimple laitteen seurannassa](storsimple-monitor-device.md).

- **Käyttö yleiskatsaus** – **käyttö yhteenveto** -alueella näet ensisijainen tallennustilan käytetään valmistellun tallennustilan ja suurin tallennustilaa laitteeseesi. Käyttö-numerot, että ne ovat käytettävissä enimmäismäärän vertaamalla näet yhdellä silmäyksellä hankkia lisää tallennustilaa tarvittaessa. Huomautus Tämä yleiskatsaus päivitetään 15 minuutin välein ja vuoksi päivityksen korkojakso eron, näkyvät eri numeroita kuin ne näytetään kaavioalueen yläpuolella, joka päivittyy päivittäin. Lisätietoja on artikkelissa [StorSimple hallintapalvelu StorSimple laitteen seurannassa](storsimple-monitor-device.md).


- **Ilmoitukset** – **ilmoitukset** on yleiskatsaus laitteeseesi ilmoituksia. Ilmoitukset on ryhmitelty vakavuus ja määrä on annettu ilmoitusten vakavuus kunkin tason numero. Ilmoitusten vakavuus napsauttaminen avaa kohdistetuissa näkymän näyttämään vain vakavuus tason laite ilmoitukset Ilmoitukset-välilehti.

- **Töiden** – **työt** -alue näyttää viimeisimmät työn tehtävä tulokset. Näin voit varmistaa voit, järjestelmä toimii odotetulla tavalla, tai se ilmoittaa, että sinun on suoritettava korjaavat toimenpiteet. Saat lisätietoja viimeksi valmiit työt valitsemalla **työt onnistui viimeisen 24 tunnin aikana**.

- Koontinäytön oikealla **quick glance** -alueella on hyödyllisiä tietoja, kuten laitteen mallista, sarjanumero, tila, kuvaus ja asemat määrä.

Voit myös määrittää automaattisesti ja tarkastella yhdistetyn laitteistokäynnistäjät laite-koontinäytössä.

Yleisiä tehtäviä, joita voi käyttää tällä sivulla ovat seuraavat:

- Tarkastele yhdistetyn laitteistokäynnistäjät

- Etsi laitteen sarjanumero

- Etsi laitteen kohde IQN

## <a name="view-connected-initiators"></a>Tarkastele yhdistetyn laitteistokäynnistäjät

Voit tarkastella iSCSI laitteistokäynnistäjät, jotka ovat yhteydessä käyttämäsi laite valitsemalla **Näytä yhdistetty laitteistokäynnistäjät** linkistä laitteen Raporttinäkymät-ikkunan **quick glance** -alueella. Tällä sivulla on taulukkomuotoinen luettelo, joka on yhdistetty laitteeseen laitteistokäynnistäjät. Voit tarkastella kunkin Käynnistäjä:

- ISCSI koko nimi (IQN) on yhdistetty lähettäjä.

- Accessin ohjausobjektin tietueen (ACR), joka sallii tämän yhdistetyn käynnistäjä nimi.

- Yhdistetyn käynnistäjä IP-osoite.

- Verkkoliittymät, käynnistäjä on liitetty tallennustilan laitteen. Nämä väliltä 0 tietojen tietojen 5.

- Kaikki asemat, yhdistetyt käynnistäjä on oikeus käyttää nykyisen ACR määritysten mukaan.

Jos tarkastella odottamattomia laitteistokäynnistäjät tämän luettelon tai ei näy odotettua niistä, tarkista ACR-määritys. Enintään 512 laitteistokäynnistäjät muodostaa laitteeseen.

## <a name="find-the-device-serial-number"></a>Etsi laitteen sarjanumero

Sinun täytyy ehkä laitteen sarjanumero, kun määrität Microsoft monipolku i/o (MPIO) laitteeseen. Seuraavien toimien Etsi laitteen sarjanumero.

#### <a name="to-find-the-device-serial-number"></a>Etsi laitteen sarjanumero

1. Siirry **laitteet** > **raporttinäkymät-ikkunan**.

2. Etsi Raporttinäkymät-ikkunan oikeanpuoleisessa ruudussa **quick glance** -alueelle.

3. Vieritä alaspäin ja Etsi järjestysluvun.

## <a name="find-the-device-target-iqn"></a>Etsi laitteen kohde IQN

Joudut ehkä laitteen kohde IQN, kun määrität StorSimple laitteen todennus Handshake Authentication Protocol (CHAP). Seuraavien toimien Etsi laitteen kohde IQN.

### <a name="to-find-the-device-target-iqn"></a>Etsi laitteen kohde IQN

1. Siirry **laitteet** > **raporttinäkymät-ikkunan**.

1. Etsi Raporttinäkymät-ikkunan oikeanpuoleisessa ruudussa **quick glance** -alueelle.

1. Vieritä alaspäin ja Etsi kohde IQN.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple hallinnan palvelun Raporttinäkymät-ikkunan](storsimple-service-dashboard.md).
- Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
