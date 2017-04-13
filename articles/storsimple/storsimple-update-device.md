<properties
   pageTitle="Päivitä StorSimple-laitteeseen | Microsoft Azure"
   description="Tässä artikkelissa kuvataan tavalliseen ja ylläpidon tila-päivitysten ja korjausten StorSimple päivitys-toiminnon avulla."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>Päivittää StorSimple 8000 sarjan laitteen

## <a name="overview"></a>Yleiskatsaus

StorSimple päivitykset ominaisuuksien avulla voit helposti pitää StorSimple laitteen ajan tasalla. Update-tyypin mukaan voit käyttää päivitykset laitteeseen Azure perinteinen portaalin kautta tai Windows PowerShell-liittymän kautta. Tässä opetusohjelmassa kuvataan päivityksen tyypit ja asenna kullekin niistä.

Voit käyttää kahdentyyppisiä laitteen päivitykset: 

- Tavallisen (tai normaaliin tilaan) päivitykset
- Ylläpidon tila päivitykset

Voit asentaa päivitykset säännöllisesti Azure perinteinen portal tai Windows PowerShellin; on kuitenkin ylläpidon tila-päivitysten asentaminen Windows PowerShellin avulla. 

Jokainen päivitys on kuvattu alla erikseen.

### <a name="regular-updates"></a>Päivitykset säännöllisesti

Normaali-päivitykset ovat häiritsevä päivitykset, jotka voidaan asentaa, kun laite on normaalisti. Nämä päivitykset käytetään kunkin laitteen ohjauskoneen Microsoft Update-sivuston kautta. 

> [AZURE.IMPORTANT] Päivityksen aikana saattaa ilmetä ohjauskoneen automaattisesti. Kuitenkin tämä ei vaikuta järjestelmän käytettävyyttä tai toiminnon.

- Lisätietoja siitä, miten voit asentaa päivitykset säännöllisesti Azure perinteinen portaalin kautta on artikkelissa [Asenna päivitykset säännöllisesti Azure perinteinen portal(#install-regular-updates-via-the-azure-classic-portal) kautta.

- Voit myös asentaa päivitykset säännöllisesti Windows PowerShellin kautta StorSimple. Lisätietoja on artikkelissa [Asenna päivitykset säännöllisesti Windows PowerShell StorSimple kautta](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Ylläpidon tila päivitykset

Ylläpidon tila päivityksiä häiritsevä päivitykset, kuten levyn laitteisto päivitykset. Päivitykset edellyttävät laitteeseen, jotta voit laittaa ylläpidon tila. Lisätietoja on artikkelissa [Vaihe 2: Anna ylläpidon tila](#step2). Et voi käyttää Azure perinteinen portaalin ylläpidon tila-päivitysten asentaminen. Sen sijaan sinun on käytettävä Windows PowerShellin StorSimple. 

Lisätietoja ylläpidon tila-päivitysten asentaminen on artikkelissa [Asenna ylläpidon tila päivitykset Windows PowerShell StorSimple kautta](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Ylläpidon tila päivitykset on otettava käyttöön erikseen jokaiselle valvojalle. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Asenna päivitykset säännöllisesti Azure perinteinen portaalin kautta

Voit käyttää Azure perinteinen portaalin päivitysten StorSimple laitteeseen.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Asenna päivitykset säännöllisesti Windows PowerShellin kautta StorSimple varten

Voit vaihtoehtoisesti päivitykset säännöllisesti (normaaliin tilaan) Windows PowerShell StorSimple.

> [AZURE.IMPORTANT] Vaikka voit asentaa Windows PowerShellin avulla StorSimple päivitykset säännöllisesti, suosittelemme, että asennat päivitykset säännöllisesti palvelun Azure perinteinen-portaalissa. Päivitys 1 alkaen vanhat tarkistus suoritetaan ennen päivitysten asentaminen-portaalista. Vanhat tarkistukset preempt virheet ja varmista, että tasainen. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Asenna ylläpidon tila päivitykset Windows PowerShellin kautta StorSimple

StorSimple laitteen ylläpidon tila päivitykset käyttöön Windows PowerShell StorSimple avulla. Kaikki i/o-pyynnöt on keskeytetty tässä tilassa. Pysyvä RAM-muistia (NVRAM) palveluihin tai Klusterointipalvelun myös pysäyttää. Molemmat ohjaimet on käynnistetty uudelleen, kun kirjoitat tai poistu tässä tilassa. Kun suljet tässä tilassa, kaikki palvelut jatkuu ja pitäisi olla kunnossa. (Tämä saattaa kestää muutaman minuutin kuluttua.)

Jos tarvitset ylläpidon tila päivitysten, saat ilmoituksen palvelun Azure perinteinen-portaalissa on päivitykset, jotka on oltava asennettuna. Tämä ilmoitus sisältää ohjeet päivitysten asentaminen Windows PowerShell StorSimple avulla. Kun olet päivittänyt laitteen, muuttaa laitteen säännöllisesti tilaan samalla tavalla avulla. Katso vaiheittaiset ohjeet [Vaihe 4: Lopeta ylläpidon tila](#step4).

> [AZURE.IMPORTANT] 
> 
> - Ennen kuin lisäät ylläpidon tila, varmista, että molemmat laitteen ohjaimet kunnossa valitsemalla **Laitteen tila** Azure perinteinen portaalissa **ylläpito** -sivun. Jos ohjain ei ole kunnossa, pyydä Microsoft Support seuraaviin vaiheisiin. Siirry lisätietoja, ota yhteyttä Microsoftin tukeen. 
> - Kun olet ylläpito-tilassa, sinun täytyy päivityksen ensin yhden ohjaimen ja muut ohjaimen.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Vaihe 1: Yhdistä serial konsoliin<a name="step1">

Ensin käyttää sovelluksen, kuten painovärit, muste serial konsolin. Seuraavassa kerrotaan, miten voit muodostaa serial konsolin painovärit, muste avulla.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Vaihe 2: Kirjoita ylläpidon tila<a name="step2">

Kun olet muodostanut yhteyden konsolin, määrittää, ovatko Asenna ja määritä ylläpito-tilassa ja asenna ne.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Vaihe 3: Asenna päivitykset<a name="step3">

Seuraavaksi Asenna päivitykset.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Vaihe 4: Lopeta ylläpidon tila<a name="step4">

Lopeta-ylläpito-tilassa.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Asenna Windows PowerShellin kautta korjausten StorSimple

Toisin kuin päivitykset Microsoft Azure StorSimple korjausten asennetaan jaetusta kansiosta. Päivitykset, jossa on kahdentyyppisiä korjausten: 

- Tavallinen korjausten 
- Ylläpidon tila korjausten  

Seuraavien kerrotaan, kuinka tavalliseen ja ylläpito tilassa korjausten asentaminen Windows PowerShell StorSimple avulla.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Mitä tapahtuu päivitykset, jos suoritat factory-Palauta laitteen?

Jos laite on palauttaa factory asetukset, kaikki muutokset menetetään. Factory-Palauta laite on rekisteröity ja määrittänyt, sinun on asentaminen manuaalisesti StorSimple päivitykset Azure perinteinen portal-ja/tai Windows PowerShellin kautta. Lisätietoja factory Palauta on artikkelissa [Palauta oletusarvoiksi laitteen](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [Windows PowerShellin StorSimple ammattimainen StorSimple laitteen varten](storsimple-windows-powershell-administration.md).
- Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
