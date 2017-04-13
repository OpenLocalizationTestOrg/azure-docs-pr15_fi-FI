<properties 
   pageTitle="StorSimple tilannevedoksen hallinta varmuuskopion käytännöt | Microsoft Azure"
   description="Tässä artikkelissa käsitellään StorSimple tilannevedoksen hallinnan MMC-laajennuksen avulla voit luoda ja hallita varmuuskopion käytännöt, jotka ohjaavat ajoitetun varmuuskopiot."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>StorSimple tilannevedoksen hallinnan avulla voit luoda ja hallita varmuuskopioinnin määrittäminen

## <a name="overview"></a>Yleiskatsaus

Varmuuskopion käytännön Luo varmuuskopioiminen, äänenvoimakkuuden tiedot paikallisesti tai pilveen aikataulu. Kun luot varmuuskopion käytännön, voit määrittää myös säilytyskäytännön. (Enintään 64 tilannevedoksia voidaan säilyttää.) Lisätietoja varmuuskopioinnin käytäntöjä saat [Varmuuskopiointi tyypit](storsimple-what-is-snapshot-manager.md#backup-type) - [StorSimple 8000 sarja: hybrid cloud-ratkaisun](storsimple-overview.md).

Tässä opetusohjelmassa kerrotaan, miten voit:

- Varmuuskopiointi-käytännön luominen 
- Varmuuskopion käytännön muokkaaminen 
- Poista varmuuskopioinnin käytäntö 

## <a name="create-a-backup-policy"></a>Varmuuskopiointi-käytännön luominen

Seuraavien ohjeiden avulla voit luoda uuden varmuuskopion käytännön.

#### <a name="to-create-a-backup-policy"></a>Varmuuskopiointi-käytännön luominen

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa hiiren kakkospainikkeella **Varmuuskopiointi käytännöt**ja valitse **Luo varmuuskopio-käytäntö**.

    ![Varmuuskopiointi-käytännön luominen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    **Käytännön luominen** -valintaikkuna tulee näkyviin. 

    ![Luo käytäntö - Yleiset-välilehti](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Valitse **Yleiset** -välilehden suorittaa seuraavat tiedot:

   1. Kirjoita **nimi** -tekstiruutuun käytännön nimi.

   2. Kirjoita **aseman ryhmä** -muokkausruutuun käytäntöön liittyvät aseman ryhmän nimi.

   3. Valitse **Paikallisen tilannevedoksen** tai **Cloud tilannevedoksen**.

   4. Valitse tilannevedoksia, jos haluat säilyttää määrä. Jos olet valinnut **kaikki**, 64 tilannevedoksia on voimassa (enintään). 

4. Valitse **aikataulu** -välilehti.

    ![Käytännön - aikataulu-välilehti](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Valitse **aikataulu** -välilehti tee seuraavat tiedot: 

   1. Valitse **Ota käyttöön** -valintaruutu, jos haluat seuraavan varmuuskopioinnin.

   2. Valitse **asetukset**-kohdasta **kerran**, **Päivittäin**, **Viikoittain**tai **kuukausittain**. 

   3. **Käynnistä** -tekstiruutuun kalenteri-kuvaketta ja valitse alkamispäivä.

   4. **Lisäasetukset**-kohdassa voit määrittää valinnainen toista aikatauluja ja päättymispäivä.

   5. Valitse **OK**.

Kun olet luonut varmuuskopion käytännön **tulokset** -ruudussa näkyy seuraavat tiedot:

- **Nimi** – varmuuskopion käytännön nimi.

- **Kirjoita** – paikallisen tilannevedoksen tai cloud tilannevedoksen.

- **Äänenvoimakkuuden ryhmän** – käytännön liittyvän aseman ryhmän.

- **Säilytys** – tilannevedoksia säilyttää; määrä enimmäismäärä on 64.

- **Luotu** – päivämäärä, jolloin käytäntö on luotu.

- **Käytössä** – onko käytäntö tällä hetkellä käytössä: **True** osoittaa, että se on voimassa; **Epätosi** ilmaisee, että se ei ole käytössä. 

## <a name="edit-a-backup-policy"></a>Varmuuskopion käytännön muokkaaminen

Seuraavien ohjeiden avulla voit muokata olemassa olevaa varmuuskopion käytäntöä.

#### <a name="to-edit-a-backup-policy"></a>Voit muokata käytäntöä varmuuskopiointi

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. Valitse **laajuus** -ruudun **Varmuuskopiointi käytännöt** -solmun. Varmuuskopion käytännöt näkyvät **tulosruudussa** . 

3. Napsauta hiiren kakkospainikkeella, jota haluat muokata käytäntöä ja valitse sitten **Muokkaa**. 

    ![Varmuuskopion käytännön muokkaaminen](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. Kun **käytännön luominen** -ikkuna tulee näkyviin, tee haluamasi muutokset ja valitse sitten **OK**. 

## <a name="delete-a-backup-policy"></a>Poista varmuuskopioinnin käytäntö

Seuraavien ohjeiden avulla voit poistaa varmuuskopion käytännön.

#### <a name="to-delete-a-backup-policy"></a>Jos haluat poistaa varmuuskopion käytäntö

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. Valitse **laajuus** -ruudun **Varmuuskopiointi käytännöt** -solmun. Varmuuskopion käytännöt näkyvät **tulosruudussa** . 

3. Varmuuskopion käytäntö, jonka haluat poistaa hiiren kakkospainikkeella ja valitse sitten **Poista**.

4. Kun näyttöön tulee vahvistussanoma tulee näkyviin, valitse **Kyllä**.

    ![Poista varmuuskopioinnin käytännön vahvistus](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta ammattimainen StorSimple ratkaisu](storsimple-snapshot-manager-admin.md).
- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta voit tarkastella ja hallita töitä varmuuskopion](storsimple-snapshot-manager-manage-backup-jobs.md).
