<properties 
   pageTitle="Muuta StorSimple laite | Microsoft Azure"
   description="Kuvataan StorSimple laitteen tilat ja kerrotaan, miten voit muuttaa laitteen tila Windows PowerShell StorSimple avulla."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>Muuta laitteen StorSimple laitteen

Tässä artikkelissa on lyhyt kuvaus, jonka StorSimple laite toimii eri tilat. StorSimple laitteen toimivat kolmessa: Normaali, ylläpito ja palauttaminen. 

Luettuasi tämän artikkelin tiedät:

- Mikä StorSimple laitteen tila on
- Voit selvittää, mitä tilaa StorSimple laite on
- Voit muuttaa Normaali ylläpidon tila ja *päinvastoin*


Yllä hallintatehtäviä voidaan suorittaa vain StorSimple laitteesi Windows PowerShell-liittymän kautta.

## <a name="about-storsimple-device-modes"></a>Tietoja StorSimple laitteen tilat

StorSimple laite toimii Normaali, ylläpito tai palautus-tilassa. Kunkin kolmesta tilasta kuvataan lyhyesti alla.

### <a name="normal-mode"></a>Normaaliin tilaan

Tämä on määritetty täysin määritetyn StorSimple laitteen toiminnallisia normaalisti. Oletusarvon mukaan laitteessa on oltava normaalitilassa.

### <a name="maintenance-mode"></a>Ylläpidon tila

Joskus StorSimple laitteen ehkä sijoittaa ylläpito-tilaan. Tässä tilassa voit ylläpitoon laite ja asenna häiritsevä päivitykset, kuten liittyvät levyn laitteisto.

Voit sijoittaa järjestelmän StorSimple ylläpito tilaan vain Windows PowerShellin kautta. Kaikki i/o-pyynnöt on keskeytetty tässä tilassa. Pysyvä RAM-muistia (NVRAM) palveluihin tai Klusterointipalvelun myös pysäyttää. Molemmat ohjaimet käynnistyvät, kun kirjoitat tai poistu tässä tilassa. Kun suljet ylläpito-tilassa, kaikki palvelut jatkuu ja pitäisi olla kunnossa. Tämä saattaa kestää muutaman minuutin kuluttua.

>[AZURE.NOTE]**Ylläpidon tila on tuettu vain laite toimii oikein. Ei tueta laitteessa, jonka jommankumman tai molempien ohjaimet ovat ei toimi.**
</br>

### <a name="recovery-mode"></a>Palautus-tilassa

"Windowsin vikasietotila verkkotuen kanssa" voidaan kuvata palautus-tilassa. Palautus käyttää Microsoft Support team ja avulla voit suorittaa vianmäärityksen järjestelmä. Palautus tila on ensisijainen hakemiseen järjestelmälokit.

Jos järjestelmässä siirtyy palautustilaan, ota seuraavat vaiheet Microsoft Support. Siirry lisätietoja, [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE] **Et voi sijoittaa laitteen palautus-tilassa. Jos laite on virheellisessä tilassa, palautus tilassa yrittää saada laitteen vaiheeseen, jossa Microsoft Support henkilökunnan voit tarkastella sitä.**

## <a name="determine-storsimple-device-mode"></a>Määrittää StorSimple laitteen tila

#### <a name="to-determine-the-current-device-mode"></a>Määrittää nykyisen laitteen tila

1. Kirjaudu laitteen serial konsolin noudattamalla [Käytä painovärit, muste muodostaa laitteen serial konsolin](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Tarkista laitteen serial console-valikossa nauha-viesti. Tämä viesti erikseen näkyy, onko laitteen huoltoa tai palautus-tilassa. Jos viesti ei sisällä mitään tiettyjä tietoja liittyvät järjestelmä-tilaan, laite on normaalisti.

## <a name="change-the-storsimple-device-mode"></a>Muuta StorSimple laitteen tila 

Voit sijoittaa StorSimple laitteen ylläpidon tila (Normaali tilasta) suorittavat ylläpitoa tai ylläpidon tila-päivitysten asentaminen. Tekemällä seuraavat toimet tai poistu ylläpito tilasta.

> [AZURE.IMPORTANT] Ennen siirtymistä ylläpidon tila, varmista, että molemmat laitteen ohjaimet kunnossa muodostamalla yhteyden Azure perinteinen portaalissa **ylläpito** -sivun **Laitteiston tila** . Jos toinen tai molemmat ohjaimet eivät ole kunnossa, pyydä Microsoft Support seuraaviin vaiheisiin. Siirry lisätietoja, [Ota yhteyttä Microsoftin tukeen](storsimple-contact-microsoft-support.md).

#### <a name="to-enter-maintenance-mode"></a>Jos haluat kirjoittaa ylläpidon tila

1. Kirjaudu laitteen serial konsolin noudattamalla [Käytä painovärit, muste muodostaa laitteen serial konsolin](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console).

2. Valitse sarja konsoli-valikon vaihtoehto 1, **Kirjaudu sisään täydet oikeudet**. Anna kehotettaessa **laitteen järjestelmänvalvojan salasanan**. Oletus-salasana: `Password1`.

3. Kirjoita komentoriville seuraava komento 

    `Enter-HcsMaintenanceMode`

4. Näyttöön tulee varoitussanoma, joka ilmoittaa, että ylläpidon tila häiritä kaikki i/o-pyynnöt ja Server Azure perinteinen portaalin yhteys ja sinua pyydetään vahvistamaan. Kirjoita **Y** -ylläpito siirtyminen.

5. Molemmat ohjaimet käynnistetään uudelleen. Kun uudelleenkäynnistyksen on valmis, sanoma tulee näkyviin, joka ilmaisee, että laitteen ylläpito-tilassa.


#### <a name="to-exit-maintenance-mode"></a>Lopeta ylläpidon tila

1. Kirjaudu laitteen serial konsolin. Tarkista nauha viestistä ylläpito-tilassa olevan laitteen.

2. Kirjoita komentoriville seuraava komento:

    `Exit-HcsMaintenanceMode`

3. Varoitussanoma ja vahvistusviesti tulee näkyviin. Kirjoita **Y** Lopeta ylläpidon tila.

4. Molemmat ohjaimet käynnistetään uudelleen. Kun uudelleenkäynnistyksen on valmis, toinen viesti näkyy, joka ilmaisee, että laite on normaalitilassa.


## <a name="next-steps"></a>Seuraavat vaiheet

Tietoja [normaalinäkymässä ja ylläpito tilassa päivitysten](storsimple-update-device.md) StorSimple laitteen.

