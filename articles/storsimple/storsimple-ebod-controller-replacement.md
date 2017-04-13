<properties 
   pageTitle="Korvaa StorSimple EBOD ohjauskoneen | Microsoft Azure"
   description="Kerrotaan, miten voit poistaa ja korvata toisen tai molemmat EBOD ohjaimet StorSimple 8600 laitteessa."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Korvaa EBOD-ohjain StorSimple laitteen

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten voit korvata viallinen EBOD ohjauskoneen moduuli Microsoft Azure StorSimple laitteen. Jos haluat korvata EBOD-ohjain moduuli, sinun on:

- Poista viallinen EBOD-ohjain
- Asenna uusi EBOD-ohjain

Ennen kuin aloitat, ota huomioon seuraavat tiedot:

- Tyhjä EBOD moduulit on lisätty kaikki käyttämättömät paikkaa. Kehyksen ei jäähdytetään oikein, jos paikka on auki.

- EBOD-ohjain on kuuman-vaihdettavan laitepaikan ja voi poistaa tai korvata. Älä poista epäonnistui moduuli, kunnes olet korvaa. Kun käynnistät korvaava-prosessi, se on valmis 10 minuutin kuluessa.

>[AZURE.IMPORTANT] Ennen kuin yrität poistaa tai korvata StorSimple osia, varmista, että Tarkista [turvallisuutta kuvake nimeämiskäytännön](storsimple-safety.md#safety-icon-conventions) ja muut [turvallisuutta varotoimenpiteet](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Poista EBOD-ohjain

Ennen korvaamista EBOD ohjauskoneen moduulin StorSimple laitteeseen, varmista, että muut EBOD ohjauskoneen moduulin on aktiivinen ja käytössä. Seuraava menettely ja taulukossa kerrotaan, kuinka poistat EBOD controller-moduulin.

#### <a name="to-remove-an-ebod-module"></a>Jos haluat poistaa EBOD-moduuli

1. Avaa Azure perinteinen-portaali.

2. Siirry **laitteet** > **ylläpito** > **Laitteiston tila**ja varmista, että aktiivinen EBOD ohjauskoneen moduulin LED tila on vihreä ja EBOD ohjauskoneen moduulin LED on punainen.

3. Etsi EBOD ohjauskoneen moduulin laitteen takana.

4. Poista kaapeli, jotka muodostavat EBOD ohjauskoneen moduulin valvojalle ennen siihen EBOD moduulin järjestelmän ulos.

5. Pane merkille EBOD ohjauskoneen moduulia, johon on liitetty ohjaimen tarkka SAS portti. Voit edellyttää palauttaminen järjestelmän määritysten jälkeen voit korvata EBOD-moduuli. 

    >[AZURE.NOTE] Tämä on yleensä portti A, joka on merkintä **Host (isäntä)-** seuraavassa kaaviossa.

    ![EBOD backplane-liitäntä ohjain](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Kuva 1** Taustapuolta EBOD-moduuli

  	|Otsikko|Kuvaus|
  	|:----|:----------|
  	|1|Vian LED|
  	|2|Power LED|
  	|3|SAS yhdistimet|
  	|4|SAS merkkivalot|
  	|5|Peräkkäiset portit vain factory-käyttöä varten|
  	|6|Port (isäntä)|
  	|7|Portti B (isäntä ulos)|
  	|8|Portti C (vain Factory käyttö)|

## <a name="install-a-new-ebod-controller"></a>Asenna uusi EBOD-ohjain

Seuraava menettely ja taulukossa kerrotaan StorSimple laitteen EBOD-ohjain-moduulin asentaminen.

#### <a name="to-install-an-ebod-controller"></a>Asenna EBOD-ohjain

1. Tarkista vahinkoa, erityisesti käyttöliittymän yhdistimen EBOD laitteen. Älä asenna uusi EBOD ohjauskoneen, jos minkä tahansa PIN ovat taivuttaa.

2. Avaa paikalleen salpojen ja Siirrä moduulin kehys, kunnes salpojen osallistuminen.

    ![EBOD ohjauskoneen asentaminen](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Kuva 2**  EBOD controller-moduulin asentaminen

3. Sulje salvan. Kuulet olisi napsauttamalla kuin salvan alkaa.

    ![EBOD salvan vapauttaminen](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Kuva 3**  EBOD moduulin salvan sulkeminen

4. Yhdistä kaapeli. Käytä tarkka määritys, joka oli ennen korvaa. Katso seuraavat kaavio ja lisätietoja muodostaa kaapeli taulukkoa.

    ![Kaapeli 4U laitteen Power](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Kuva 4**. Kaapeli muodostaminen uudelleen

  	|Otsikko|Kuvaus|
  	|:----|:----------|
  	|1|Ensisijainen kehys|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|Controller 0|
  	|5|Ohjaimen 1|
  	|6|EBOD controller 0|
  	|7|EBOD ohjauskoneen 1|
  	|8|EBOD kehys|
  	|9|Power jakauman yksiköt|

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [StorSimple laitteiston osan korvaaminen](storsimple-hardware-component-replacement.md).
