<properties 
   pageTitle="Hallitse StorSimple-asema säilöjen | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan käyttämisestä StorSimple hallinnan palvelun aseman säilöt-sivulla voit lisätä, muokata tai poistaa aseman säilön."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-storsimple-volume-containers"></a>StorSimple hallinta-palvelun avulla voit hallita StorSimple äänenvoimakkuuden säilöt

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten StorSimple hallinta-palvelun avulla voit luoda ja hallita StorSimple äänenvoimakkuuden säilöjen.

Microsoft Azure StorSimple laitteen äänenvoimakkuuden säilön sisältää vähintään yhden asemat, jotka jakavat tallennustilan tilin, salaus ja kaistanleveyden kulutus asetukset. Laitteen voi olla useita aseman säilöjä sen asemat. 

Äänenvoimakkuuden säilön on määritteet:

- **Asemat** – Porrastettu tai paikallisesti kiinnitetty StorSimple asemista, jotka sisältyvät aseman säilö. Äänenvoimakkuuden säilön voi olla enintään 256 StorSimple asemat.

- **Salauksen** – salausavain, joka voidaan määrittää kunkin aseman säilön. Tiedot, jotka lähetetään StorSimple laitteesta pilveen salaamiseen käyttää avainta. Armeijan arvosanojen AES 256 bittinen näppäintä käytetään käyttäjä on kirjoittanut-näppäintä. Suojata tietoja, on suositeltavaa cloud tallennustilan salaus aina käyttöön.

- **Tallennustilan tilin** – tallennustilan tilin, joka on linkitetty cloud tallennustilan-palveluntarjoajan. Kaikki aseman säilön asuvat asemat jakaa tallennustilan tähän tiliin. Voit valita tallennustilan tilin olemassa oleva luettelo tai Luo uusi tili, kun luot aseman säilö ja määritä sitten tilin tunnistetiedoilla.

- **Cloud kaistanleveyden** – kaistanleveyttä-laite, kun laitteen tiedot lähetetään pilveen. Voit pakottaa kaistanleveyden ohjausobjektin määrittämällä arvo väliltä 1-1 000 Mbps, kun haluat määrittää tämän säilö. Laitteen tarjoaman kaikki kaistanleveyttä halutessasi arvoksi rajoittamaton tähän kenttään. Voit luoda ja kaistanleveyden mallin varata kaistanleveyden aikataulun mukaan.

![Äänenvoimakkuuden säilöt-sivu](./media/storsimple-manage-volume-containers/HCS_VolumeContainersPage.png)

Tämä seuraavasti kerrotaan, miten seuraavat yhteiset toiminnot StorSimple **äänenvoimakkuuden säilöt** -sivun avulla:

- Lisää äänenvoimakkuutta-säilö 
- Muokata asemaa säilö 
- Äänenvoimakkuuden säilön poistaminen 

## <a name="add-a-volume-container"></a>Lisää äänenvoimakkuutta-säilö

Seuraavien toimien Lisää äänenvoimakkuutta säilö.

[AZURE.INCLUDE [storsimple-add-volume-container](../../includes/storsimple-add-volume-container.md)]


## <a name="modify-a-volume-container"></a>Muokata asemaa säilö

Seuraavien toimien muokata asemaa säilön.

[AZURE.INCLUDE [storsimple-modify-volume-container](../../includes/storsimple-modify-volume-container.md)]


## <a name="delete-a-volume-container"></a>Äänenvoimakkuuden säilön poistaminen

Äänenvoimakkuuden säilön on sen sisältämät asemat. Voit poistaa vain, jos kaikki sen sisältävät asemat poistetaan. Seuraavien toimien aseman säilön poistaminen.

[AZURE.INCLUDE [storsimple-delete-volume-container](../../includes/storsimple-delete-volume-container.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [hallinta StorSimple asemat](storsimple-manage-volumes.md). 
- Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
