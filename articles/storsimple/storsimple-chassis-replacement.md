<properties 
   pageTitle="Korvaa alustan StorSimple laitteessa | Microsoft Azure"
   description="Kerrotaan, miten voit poistaa ja korvata alustan StorSimple ensisijainen kehyksen tai EBOD kehys."
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

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Korvaa StorSimple laitteen alustan

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten voit poistaa ja korvata alustan StorSimple 8000 sarja-laitteessa. StorSimple 8100 malli on yksittäinen kehys-laite (yksi alustan) 8600 on kaksi kehys-laite (kaksi runkoon). 8600-mallissa on mahdollisesti kaksi alustan, joka saattaa epäonnistua laitteen: ensisijainen kehyksen tai EBOD kehyksen alustan alustan.

Kummassakin tapauksessa korvaavan alustan, joka on lähetetty Microsoft on tyhjä. Power ja jäähdytys moduulit (PCMs)-ohjain moduulit tasainen tilan levyasemien (SSDs), kiintolevyaseman levyasemien (HDDs) tai EBOD moduulit sisällytetään.

>[AZURE.IMPORTANT] Poistaminen ja korvaaminen alustan, ennen kuin [StorSimple laitteiston osan korvaava](storsimple-hardware-component-replacement.md)-arviointitietoja turvallisuus.

## <a name="remove-the-chassis"></a>Poista alustan

Seuraavien toimien Poista alustan StorSimple laitteen.

#### <a name="to-remove-a-chassis"></a>Jos haluat poistaa runkoon

1. Varmista, että StorSimple laitteen Sammuta ja katkennut power lähteistä.

2. Poista kaikki verkko- ja Suojaussidosten kaapeli tarvittaessa.

3. Poista yksikkö Teline.

4. Poista kaikki asemat ja Huomaa paikkaa, joista ne poistetaan. Lisätietoja on artikkelissa [Poista levyaseman](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. Valitse EBOD kehyksen (jos se on alustan, joka epäonnistui) Poista EBOD ohjauskoneen moduulit. Lisätietoja on artikkelissa [Poista EBOD-ohjain](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 

    Ensisijainen kehys (jos se on alustan, joka epäonnistui), valitse Poista ohjaimet, ja tarkista paikkaa, joista ne poistetaan. Lisätietoja on artikkelissa [valvonta on poistettava](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Asenna alustan

Seuraavien toimien alustan asennetaan laitteen StorSimple.

#### <a name="to-install-a-chassis"></a>Asenna runkoon

1. Ota käyttöön Teline alustan. Lisätietoja on artikkelissa [Teline-käyttöönoton StorSimple 8100 laitteen](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) tai [Teline-käyttöönoton StorSimple 8600 laitteen](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Kun alustan on otettu käyttöön Teline, asenna ohjauskoneen moduulit samassa sijainnissa, ne on aiemmin asennettu.

3. Asenna asemat samoja toimia ja paikkaa, johon ne on aiemmin asennettu.

    >[AZURE.NOTE] Suosittelemme, että Asenna SSD ensin paikkaa ja asentaa HDDs.

2. Otettu käyttöön Teline ja asennettu osista laitteeseen laitteen yhdistäminen tarvittavat power lähteet ja ottaa käyttöön laite. Lisätietoja on artikkelissa [StorSimple 8100 laitteen](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) tai [kaapelin StorSimple 8600 laitteen](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [StorSimple laitteiston osan korvaaminen](storsimple-hardware-component-replacement.md).

