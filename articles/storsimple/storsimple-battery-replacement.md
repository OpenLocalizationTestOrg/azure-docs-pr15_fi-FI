<properties 
   pageTitle="Korvaa varauksen StorSimple laitteessa | Microsoft Azure"
   description="Tässä artikkelissa käsitellään poistaminen, korvaaminen ja ylläpitää StorSimple laitteen varmuuskopion varauksen-moduuli."
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

# <a name="replace-the-backup-battery-module-on-your-storsimple-device"></a>Korvaa StorSimple laitteen varmuuskopion varauksen-moduuli

## <a name="overview"></a>Yleiskatsaus

Ensisijainen kehyksen Power ja jäähdytys moduuli (PCM) Microsoft Azure StorSimple laitteen on muita varauksen pack. Tämä Pack-paketti sisältää power niin, että StorSimple laitteen voit tallentaa tiedot, jos ensisijainen kehyksen AC esitysmuotoa menetyksiä. Tämän varauksen Pack-paketin kutsutaan *varmuuskopion varauksen moduuli*. Varmuuskopion varauksen moduuli on vain ensisijainen kehyksen StorSimple laitteeseen (EBOD kehyksen ei sisällä varmuuskopion varauksen moduuli). 

Tässä opetusohjelmassa kerrotaan, miten voit:

- Poista varmuuskopioinnin varauksen-moduuli 
- Uuden varmuuskopion varauksen-moduulin asentaminen
- Ylläpidä varmuuskopion varauksen-moduuli

>[AZURE.IMPORTANT] Poistaminen ja korvaaminen varmuuskopion varauksen moduuli, ennen kuin [StorSimple laitteiston osan korvaaminen johdanto](storsimple-hardware-component-replacement.md)arviointitietoja turvallisuus.

## <a name="remove-the-backup-battery-module"></a>Poista varmuuskopioinnin varauksen-moduuli

StorSimple laitteen varmuuskopion varauksen-moduuli on kentän korvattavien yksikkö. Ennen kuin se on asennettu PCM, varauksen moduuli on tallennettu alkuperäisessä. Seuraavien toimien Poista varmuuskopion varauksen.

#### <a name="to-remove-the-backup-battery-module"></a>Jos haluat poistaa varmuuskopion varauksen-moduuli

1. Siirry Azure perinteinen portal **laitteet** > **ylläpito** > **Laitteiston tila**. **Jaetut osat**, valitse Tarkista varauksen tila.

2. Määritä PCM, jossa varauksen epäonnistui. Kuvassa 1 on StorSimple laitteen taustalle.

    ![Backplane-liitäntä laitteen ensisijainen kehyksen moduulit](./media/storsimple-battery-replacement/IC740994.png)

    **Kuva 1** Takaisin ensisijainen laite, jossa PCM ja ohjauskoneen moduulit

  	|Otsikko|Kuvaus|
  	|:----|:----------|
  	|1|PCM 0|
  	|2|PCM 1|
  	|3|Controller 0|
  	|4|Ohjaimen 1|

    Kuten kuvassa 2 luvulla 3, seurannan ilmaisin LED-PCM 0, joka vastaa **Varauksen vika** , olisi toiminnassa.

    ![Backplane-liitäntä laitteen PCM seuranta ilmaisin merkkivalot.](./media/storsimple-battery-replacement/IC740992.png)

    **Kuva 2** Takaisin PCM, jossa seurantaa merkkivalot ilmaisin

  	|Otsikko|Kuvaus|
  	|:---|:-----------|
  	|1|AC power virhe|
  	|2|Tuulettimen virhe|
  	|3|Varauksen virhe|
  	|4|PCM OK|
  	|5|Toimialueen Ohjauskoneen power virhe|
  	|6|Varauksen kunnossa|

3. Jos haluat poistaa PCM epäonnistui varauksen kanssa, noudata [poistaminen PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).

4. PCM on poistettu Nosta ja tuoda ylöspäin Poista varauksen kiertokahva varauksen moduulin ylöspäin seuraavassa kuvassa esitetyllä tavalla.

    ![Varauksen poistaminen PCM](./media/storsimple-battery-replacement/IC741019.png)

    **Kuva 3** Varauksen poistaminen PCM

5. Sijoita moduulin kentän korvattavien yksikkö pakkaamista.

6. Palaa Microsoft viallinen yksikkö ERISNIMI ylläpidon ja käsittelyä.

## <a name="install-a-new-backup-battery-module"></a>Uuden varmuuskopion varauksen-moduulin asentaminen

Seuraavien toimien asentaa korvaavan varauksen moduulin PCM-StorSimple laitteen ensisijainen kehys.

#### <a name="to-install-the-battery-module"></a>Varauksen-moduulin asentaminen

1. Aseta varmuuskopion varauksen moduuli PCM ERISNIMI suuntaan.

2. Painamalla alaspäin varauksen moduulin kahvaa aivan istuin yhdistin.

3. Korvaa-ensisijainen kehyksen PCM [Korvaa Power ja jäähdytys moduulin StorSimple laitteesi](storsimple-power-cooling-module-replacement.md)ohjeiden mukaisesti.

4. Kun korvaaminen on valmis, siirry osoitteeseen **laitteet** > **ylläpito** > **Laitteiston tila** Azure perinteinen-portaalissa. Tarkista, varmista, että asennuksen onnistuneen varauksen tila. Vihreä tila ilmaisee, että varauksen on kunnossa.

## <a name="maintain-the-backup-battery-module"></a>Ylläpidä varmuuskopion varauksen-moduuli

Laitteen StorSimple varmuuskopion varauksen-moduuli sisältää power valvojalle power tappio tapahtuman aikana. Tärkeitä tietoja hallittu suljetaan ennen Tallenna StorSimple laitteen avulla. Kaksi täyteen paristoa PCMs, ja järjestelmä voit käsitellä kaksi peräkkäistä tappio tapahtumat.

Azure perinteinen portaalissa **ylläpito** -sivun **Laitteiston tila** ilmaisee, onko varauksen virheellisesti vai Lopeta elinkaaren on lähellä. Varauksen tila ilmaistaan **PCM 0 varauksen** tai **varauksen PCM 1** **Jaetut**osat. Tällä sivulla näkyy **HEIKENTYNYT** osavaltio lähellä aika ja aika, **epäonnistui** saavutetaan. 

>[AZURE.NOTE] Varauksen voit raportoida **epäonnistui** , kun se on vain veloitetaan.
 
Jos **HEIKENTYNYT** -tila näkyy, on suositeltavaa toiminnon Seuraava kurssi:

- Järjestelmä saattoi viimeisimmät tehon menetys tai paristot saattaa olla ylläpitotoimet säännöllisin väliajoin. Noudata järjestelmä 12 tuntia, ennen kuin jatkat.

    - Jos siinä on edelleen **HEIKENTYNYT** AC power ohjaimet ja käynnissä PCMs jatkuva yhteyden 12 tunnin jälkeen, varauksen on vaihdettava. Ota [yhteyttä Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) korvaavan varmuuskopion varauksen moduuli.

    - Jos tila muuttuu OK 12 tunnin jälkeen, varauksen on toiminnassa ja tarvitaan vain ylläpito maksu.

- Jos ole liitetty menetyksiä AC power ja PCM on otettu käyttöön ja yhdistetty AC power, varauksen on vaihdettava. [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md) järjestys korvaavan varmuuskopion varauksen moduuli.

>[AZURE.IMPORTANT] Luovuttaa epäonnistui varauksen kansallisia ja aluekohtaisten asetusten mukaan. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [StorSimple laitteiston osan korvaaminen](storsimple-hardware-component-replacement.md).
