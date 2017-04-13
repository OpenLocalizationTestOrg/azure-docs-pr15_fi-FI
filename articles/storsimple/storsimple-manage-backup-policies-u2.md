<properties 
   pageTitle="StorSimple-varmuuskopioinnin käytäntöjen hallinta | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten voit StorSimple hallintapalvelu voit luoda ja hallita manuaalinen varmuuskopiointi varmuuskopion aikatauluja ja varmuuskopion säilytys."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies-update-2"></a>StorSimple hallinta-palvelun avulla voit hallita varmuuskopion käytännöt (päivitys 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten voit hallita varmuuskopion prosesseja ja StorSimple tietomääristä varmuuskopion säilytyksen StorSimple hallinnan palvelun **Varmuuskopioinnin määrittäminen** -sivulla avulla. Tässä artikkelissa myös asennuksen manuaalinen varmuuskopiointi.

Kun levyn varmuuskopioidaan, voit luoda paikallisen tilannevedoksen tai cloud tilannevedoksen. Jos varmuuskopioidaan paikallisesti kiinnitetty äänenvoimakkuutta, on suositeltavaa, että määrität cloud tilannevedoksen. Tilanne, jossa voit nopeasti suorittaa paikallisen loppumassa johtaa ottaen paikallisen tilannevedoksia paikallisesti kiinnitetty aseman tietojoukon, joka on paljon churn on suuri määrä. Jos haluat ottaa paikallisen tilannevedoksen, on suositeltavaa ottaa vähemmän päivittäisen tilannevedoksen Varmuuskopioi viimeisimmän tila säilytetään ne päivän ja poista ne.

Paikallisesti kiinnitetty aseman cloud tilannevedoksen suorittamalla voit kopioida vain muutetut tiedot pilvipalveluun, jossa se on deduplicated ja pakata. 

## <a name="the-backup-policies-page"></a>Varmuuskopiointi käytännöt-sivulla

**Varmuuskopion käytännöt** -sivulla voit varmuuskopion käytäntöjen hallinta ja ajoittaa paikallisen ja cloud tilannevedoksia. (Varmuuskopion käytännöt käytetään Määritä varmuuskopion aikatauluja ja asemat kokoelma varmuuskopion säilytyksen.) Varmuuskopion käytännöt antaa sinulle mahdollisuuden käyttää useita tietomääristä tilannevedoksen samanaikaisesti. Tämä tarkoittaa olevan varmuuskopion käytännön luoma varmuuskopioiden kaatumisen yhdenmukaisia kopiot. **Varmuuskopioinnin määrittäminen** -sivulla näkyy luettelo varmuuskopion käytäntöjä, niiden tyypit, liittyvät asemista, säily varmuuskopioiden määrän ja halutessasi ottaa käyttöön käytännöt.

**Varmuuskopion käytännöt** sivulla avulla voit suodattaa olemassa varmuuskopio käytännöt vähintään yksi seuraavista kentistä:

- **Käytännön nimi** – käytännön liittyvä nimi. Erityyppiset käytännöt ovat:

   - Ajoitettu käytännöt, jotka on luotu käyttäjän erikseen.
   - Automaattinen käytännöt, jotka luodaan oletusarvon varmuuskopioinnin aseman tämä asetus on käytössä aseman luomisen yhteydessä. Käytännöt on nimennyt sinut *aseman nimi*_Default, jossa *aseman nimi* viittaa Azure perinteinen portaalissa käyttäjän määrittämän StorSimple äänenvoimakkuuden nimi. Automaattinen käytännöt johtaa päivittäinen cloud tilannevedoksia aloittaen 22:30 laitteen aika.
   - Tuodut käytännöt, jotka on alun perin luotu StorSimple tilannevedoksen hallinta. Nämä on tunniste, joka kuvaa, josta on tuotu käytännöt StorSimple tilannevedoksen hallinta isäntä.

- **Asemat** – käytäntöön liittyvät asemat. Kaikki liittyvät varmuuskopion käytännön asemat on ryhmitelty varmuuskopioiden luotaessa.

- **Edellisen onnistuneen varmuuskopioinnin** – päivämäärän ja ajan edellisen onnistuneen varmuuskopioinnin, joka on tehty tämän käytännön avulla.

- **Seuraava varmuuskopiointi** – päivämäärä ja aika, joka voidaan aloittaa tämän käytännön seuraava ajoitettu varmuuskopiointi.

- **Aikataulut** – aikatauluja varmuuskopion käytäntöön liittyvät määrä.

Usein käytetyt toiminnot, jotka voit tehdä tämän sivun ovat seuraavat:

- Lisää varmuuskopion käytäntö 
- Lisää tai Muokkaa aikataulua 
- Poista varmuuskopioinnin käytäntö 
- Ota manuaalinen varmuuskopiointi 
- Mukautetun varmuuskopio-käytännön luominen useita asemat ja aikatauluja 

## <a name="add-a-backup-policy"></a>Lisää varmuuskopion käytäntö

Lisätä varmuuskopion käytännön automaattisesti varmuuskopioinnin. Seuraavien toimien Azure perinteinen portaalissa voit lisätä varmuuskopion käytännön StorSimple laitteeseesi. Kun olet lisännyt käytäntö, voit määrittää aikataulun (katso [Lisää aikataulun tai muokata sitä](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy-u2](../../includes/storsimple-add-backup-policy-u2.md)]

![Video käytettävissä](./media/storsimple-manage-backup-policies-u2/Video_icon.png) **Video käytettävissä**

Katso video, jossa kerrotaan, miten voit luoda paikallisen tai cloud varmuuskopion käytäntö, napsauta [tätä](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Lisää tai Muokkaa aikataulua

Voit lisätä tai muokata aikataulun, joka on liitetty olemassa varmuuskopio käytännön StorSimple laitteen. Seuraavien toimien Azure perinteinen portaalissa voit lisätä tai muokata aikataulun.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule-u2.md)]

## <a name="delete-a-backup-policy"></a>Poista varmuuskopioinnin käytäntö

Seuraavien toimien Azure perinteinen portaalin poistaa varmuuskopion käytännön StorSimple laitteen.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Ota manuaalinen varmuuskopiointi

Seuraavien toimien tarvittaessa yksittäisen aseman (manuaalinen) varmuuskopion luominen Azure perinteinen-portaalissa.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Mukautetun varmuuskopio-käytännön luominen useita asemat ja aikatauluja

Seuraavien toimien Azure perinteinen portaalissa voit luoda mukautetun varmuuskopion käytännön, jossa on useita asemat ja aikatauluja.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy-u2.md)]


## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
