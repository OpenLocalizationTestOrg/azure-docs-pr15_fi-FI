<properties 
   pageTitle="StorSimple laitteiston osan korvaaminen | Microsoft Azure"
   description="Tässä artikkelissa käsitellään korvaa turvallisesti PCMs, varauksen ohjauskoneen moduulit, EBOD ohjaimet, levyasemien tai alustan StorSimple laitteen."
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
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>StorSimple laitteiston osan korvaaminen

## <a name="overview"></a>Yleiskatsaus

Laitteiston osan korvaaminen opetusohjelmat kuvaavat laitteita Microsoft Azure StorSimple 8000 sarjan laitteen ja poistaa ja korvata ne tarvittavat vaiheet. Tässä artikkelissa kuvataan turvallisuus-kuvakkeet, on eräänlainen yksityiskohtaiset opetusohjelmat ja näyttää osat, jotka on vaihdettava.

>[AZURE.IMPORTANT] Ennen kuin yrität poistaa tai korvata StorSimple osia, varmista, että Tarkista [turvallisuutta kuvake nimeämiskäytännön](#safety-icon-conventions) ja muut [turvallisuutta varotoimenpiteet](storsimple-safety.md).
 
### <a name="safety-icon-conventions"></a>Turvallisuus kuvake nimeämiskäytännön

Seuraavassa taulukossa on kuvattu näissä Opetusohjelmissa käytettyjä turvallisuus-kuvaketta. Maksaa turvallisuutta nämä kuvakkeet huomiota kuitenkin poistaa ja korvata laitteen osat vaiheet.

| Kuvake | Teksti | Lisätietoja |
|:---- |:---- |:-----------|
|![Varoitus-kuvake](./media/storsimple-hardware-component-replacement/Warning.png)| **AIHEUTTAA!** | Ilmaisee, että jos ei käytetä johtaa kuoleman tai vakavia vamman vaarallisissa tilanteissa. Signaalin tämän sanan on rajoitettu eniten erittäin tilanteissa.|
|![Varoitus-kuvake](./media/storsimple-hardware-component-replacement/Warning.png)| **VAROITUS.** | Ilmaisee, joka ei käytetä, jos saattaa aiheuttaa kuoleman tai vakavia vamman vaarallisissa tilanteissa.|
|![Varoitus-kuvake](./media/storsimple-hardware-component-replacement/Caution.png)| **VAROITUS!** |Osoittaa vaarallisissa tilanteissa, jos ei käytetä, voi aiheuttaa vähäisen tai kohtalaisen vahinkoa.|
|![Ilmoitus-kuvake](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **HUOMAUTUS:** | Osoittaa pidettäviä tärkeä, mutta ei vaarallisuuden liittyvät tiedot.|
|![Sähkö sähköiskuja-kuvake](./media/storsimple-hardware-component-replacement/Electric.png) | **Sähkö sähköiskuja vaarallisuuden** | Osoittaa liitäntäpisteiden.|
|![Paksu leveys-kuvake](./media/storsimple-hardware-component-replacement/Weight.png)| **Paksu leveys**| |
|![Ei käyttäjän kalibroinnista osat-kuvake](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Käyttäjä ei ole kalibroinnista osat** | Ellei oikein koulutus ei käyttää.|
|![Lue ohjeet kuvake](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Lue ensin kaikki ohjeet**| |
|![Vihje vaarallisuuden kuvake](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Vihje vaarallisuuden**| |

### <a name="before-you-begin"></a>Ennen aloittamista

Tutustu laitteen ja turvallisuutta kuvakkeet käytetään tässä opetusohjelmassa turvallisuus-tietoja. Siirry [turvallisesti asentaminen ja toimivat StorSimple laitteen](storsimple-safety.md) tiedot. Muista tarkistaa [turvallisuutta varotoimenpiteet](storsimple-safety.md#handling-precautions) , ennen kuin voit käsitellä StorSimple laitteen. 

Ennen kuin yrität korvata osan, ota huomioon seuraavat tiedot.

![Varoitus-kuvake](./media/storsimple-hardware-component-replacement/Warning.png) ![sähkö sähköiskuja kuvake](./media/storsimple-hardware-component-replacement/Electric.png) **Varoitus!** 

- Jauhettuina itse oikein käyttämällä sähköstaattinen tai antistatic kehyskartonki käsiteltäessä moduulit ja StorSimple laitteen osat.

- Kosketa mitä tahansa piirit. Käytä määrittämisessä kahvoista ja apuviivat osat, jotka voivat olla tarjoamia piirit käsiteltäessä.

![Varoitus-kuvake](./media/storsimple-hardware-component-replacement/Warning.png) ![kuvake](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **ilmoitus:**

Kun korvaat moduulin, **Jätä koskaan tyhjä bay, kehyksen takana**. Hanki ennen kuin poistat osan korvaaminen tai tyhjä moduuli.

## <a name="hardware-component-replacement-procedures"></a>Laitteiston osan korvaaminen ohjeita

Laitteen StorSimple 8000 sarjan koostuu useita laajennuksen moduulit perus-ja/tai EBOD liitteet. 8100 on yksi ensisijainen kehys 8600 on kaksi kehyksen laitetta, jonka ensisijainen kehys ja EBOD kehyksen.

Seuraavissa taulukoissa on koottu laitteen tärkeimmät laitteita. Valitse Siirry liittyvät opetusohjelman **korvaavan toimintosarja** -sarakkeessa olevaa linkkiä.

|Osat|# Esityksen pitäminen|Laajennuksen moduulin?|Korvaavan toimintosarja
|:---------|:--------|:--------------|:---------------------|
| Alustan|1|Ei|[Korvaa StorSimple laitteen alustan](storsimple-chassis-replacement.md) |
|Ensisijainen ohjaimet|2|Kyllä| [Korvaa StorSimple laitteen ohjain moduuli](storsimple-controller-replacement.md) |
|764W Power ja jäähdytys moduulit (PCMs)|2|Kyllä| [Korvaa Power ja jäähdytys moduulin StorSimple laitteen](storsimple-power-cooling-module-replacement.md) |
|Varmuuskopion varauksen|2|Kyllä| [Korvaa StorSimple laitteen varmuuskopion varauksen-moduuli](storsimple-battery-replacement.md) |
|Levyasemien|12|Kyllä| [Korvaa StorSimple laitteen levyasemaan](storsimple-disk-drive-replacement.md) |

**Taulukko 1** Ensisijainen kehyksen laitteisto-osat

Ensisijainen kehys ja EBOD kehyksen vaihtelevat niiden i/o-moduuleissa. Lisäksi PCMs on eri wattimäärästä. Valitse ensisijainen kehyksen PCMs ovat 764 W, kaltaisia EBOD kehyksen ovat 580 W. Valitse ensisijainen kehyksen PCMs sisältää myös varmuuskopion varauksen moduuli.

|Osat|# Esityksen pitäminen|Laajennuksen moduulin?| Korvaavan toimintosarja
|:---------|:--------|:--------------|:---------------------|
|Alustan|1|Ei| [Korvaa StorSimple laitteen alustan](storsimple-chassis-replacement.md) |
|EBOD ohjaimet|2|Kyllä| [Korvaa EBOD-ohjain StorSimple laitteen](storsimple-ebod-controller-replacement.md) |
|580W Power ja jäähdytys moduulit (PCMs)|2|Kyllä| [Korvaa Power ja jäähdytys moduulin StorSimple laitteen](storsimple-power-cooling-module-replacement.md) |
|Levyasemien|12|Kyllä| [Korvaa StorSimple laitteen levyasemaan](storsimple-disk-drive-replacement.md) |

**Taulukon 2** EBOD kehyksen laitteisto-osat

Laajennuksen moduulit laitteeseen näkyvät korostettuina seuraavat etu- ja taka kaavioita. Voit käyttää näitä kaavioita, onko eri laajennuksen moduulit sijainnin korvaa tarvitaan. Postikortin etupuoli kaavio näyttää levyasemien ja taka kaavioiden EBOD kehys ja ensisijainen kehys-Näytä laajennuksen moduulit.

![Laitetta, jonka levyasemien Frontplane](./media/storsimple-hardware-component-replacement/IC741028.png)

**Kuva 1** Laitteen eteen

|Otsikko|Kuvaus|
|:----|:----------|
|0 - 11|Levyasemien (yhteensä 12)|

Ensisijainen kehys ja EBOD kehyksen on asema carrier moduulit. Alustan ovat suosikkiraporttisi 12 3,5" levyasemien järjestetty 3 x 4-muodossa.

![Backplane-liitäntä laitteen ensisijainen kehyksen moduulit](./media/storsimple-hardware-component-replacement/IC740994.png)

**Kuva 2** Taustapuolta ensisijainen kehys

|Otsikko|Kuvaus|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Controller 0|
|4|Ohjaimen 1|

![Backplane-liitäntä laitteen EBOD kehyksen laajennuksen moduulit](./media/storsimple-hardware-component-replacement/IC769599.png)

**Kuva 3** Taustapuolta EBOD kehys

|Otsikko|Kuvaus|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|EBOD Controller 0|
|4|EBOD ohjauskoneen 1|

## <a name="field-replaceable-units"></a>Kentän korvattavien yksiköt

Seuraava kenttä korvattavien yksiköt (FRU) ovat käytettävissä StorSimple laitteen:

- Alustan (mukaan lukien integroitujen toimintojen-ruutu)

- 764 W AC PCM

- 580 W AC PCM

- Kiintolevy asema carrier moduulin kanssa

- Valvonta-moduuli

- EBOD ohjauskoneen moduuli

- Varmuuskopion varauksen moduuli

- Teline rail kit käyttöönottaminen

Ota tilata korvaavan nämä yksiköt [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) .

## <a name="next-steps"></a>Seuraavat vaiheet

Tarkista kaikki [turvallisuuden tiedot](storsimple-safety.md) , ennen kuin yrität korvata StorSimple laitteisto-osan.
