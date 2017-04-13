<properties
   pageTitle="Hallitse StorSimple tietomääristä | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten voit lisätä, muokata, valvoa ja poistaa StorSimple asemat ja offline-tilaan niitä tarvittaessa."
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
   ms.date="05/11/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>StorSimple hallinta-palvelun avulla voit hallita tietomääristä

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten StorSimple hallinta-palvelun avulla voit luoda ja hallita asemista StorSimple ja StorSimple virtual laitteen.

StorSimple hallintapalvelu on perinteinen Azure-portaalin, jolla voit hallita StorSimple ratkaisu yksittäisen web-liittymän laajennus. Lisäksi hallinta asemat, voit tehdä StorSimple hallintapalvelu luominen ja StorSimple palveluiden hallinta, tarkastella ja hallita laitteita, Näytä ilmoitukset- ja tarkastella ja hallita varmuuskopion käytännöt ja varmuuskopion luettelon.

> [AZURE.NOTE] Azure StorSimple luoda levinneet valmistellun asemat. Et voi luoda valmistellun kokonaan tai osittain valmistellun Azure StorSimple järjestelmän asemat.
>
> Ohuen valmistelu on virtualization tekniikka, jossa on suurempi kuin fyysinen resurssien käytettävissä. Sen sijaan, että varaaminen riitä tallennustilan etukäteen Azure StorSimple käyttää ohuen valmistelu varata samalla tarpeeksi tilaa nykyisen ehtojen mukaan. Pilvitallennustilaa joustavasti laatu helpottaa tätä tapaa, koska Azure StorSimple voit suurentaa tai pienentää pilvitallennustilaa täyttämään muuttaminen.

## <a name="the-volumes-page"></a>Asemat-sivu

**Asemat** -sivun avulla voidaan hallita sitä, että laitteistokäynnistäjät (palvelimet) Microsoft Azure StorSimple laitteessa valmisteltu tallennustilan asemat. Se näyttää luettelon asemista StorSimple laitteen.

 ![Asemat-sivu](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Asema koostuu määritteet sarja:

- **Nimi** – kuvaava nimi, joka on oltava yksilöllinen ja auttaa tunnistamaan äänenvoimakkuutta. Tätä nimeä käytetään myös valvontaraportit, kun voit suodattaa tietyn aseman.

- **Tila** – voi olla online- tai offline-tilassa. Jos vaihto, jos offline-tilassa, ei ole näkyvissä laitteistokäynnistäjät (palvelimet), joilla on käytettävä äänenvoimakkuuden käyttöoikeus.

- **Kapasiteetti** – määrittää, kuinka suuri ääni on kuin jonka käynnistäjä (palvelin). Kapasiteetin määrittää tietoihin, jotka on tallennettu käynnistäjä (palvelin) kokonaismäärä. Asemat levinneet valmisteltu ja tietojen deduplicated. Tämä tarkoittaa sitä, että laitteesi ei valmiiksi varata fyysinen tallennustilaa sisäisesti tai pilveen määritetyn aseman kapasiteetin mukaan. Äänenvoimakkuuden kapasiteetti on varattu ja kulutettu pyydettäessä.

- **Kirjoita** – asematyyppi voi olla Porrastettu tai arkistointia (alityyppi, tasoisen)

- **Accessin** – määrittää laitteistokäynnistäjät (palvelimet), joilla on käyttöoikeus asemaan. Laitteistokäynnistäjät, jotka eivät kuulu access ohjausobjektin tietue (ACR), joka on liitetty äänenvoimakkuuden ei näy äänenvoimakkuutta.

- **Seuranta** – määrittää, onko aseman seurataan. Asema on oletusarvoisesti käytössä, kun se on luotu seuranta. Valvonta on, mutta, voidaan poistaa käytöstä äänenvoimakkuuden Kloonaa. Seurannan aseman käyttöön ohjeiden näytön aseman.

Yleisimmät aseman liittyvät tehtävät ovat seuraavat:

- Lisää aseman
- Muokata asemaa
- Aseman poistaminen
- Tilavuus offline-tilaan
- Näytön aseman

## <a name="add-a-volume"></a>Lisää aseman

Olet [luonut aseman](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) StorSimple-ratkaisun käyttöönoton aikana. Asema lisätään samalla tavalla.

### <a name="to-add-a-volume"></a>Jos haluat lisätä aseman

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Valitse aseman säilön ja käyttämään säilö liittyvät asemat vastaavalla rivillä olevaa nuolta.

3. Valitse sivun alareunassa **Lisää** . Lisää äänenvoimakkuutta ohjattu toiminto käynnistyy.

     ![Lisää äänenvoimakkuutta ohjattu perusasetukset](./media/storsimple-manage-volumes/AddVolume1.png)

4. Lisää äänenvoimakkuutta ohjatun toiminnon **Perusasetukset**-kohdassa seuraavasti:

  1. Lisää äänenvoimakkuutta **nimi** .
  2. Määritä äänenvoimakkuutta **Valmisteltu kapasiteetin** gt tai Teratavua. Resursseja on oltava vähintään 1 gt-64 TT fyysinen laitteen. Suurin kapasiteetti, joka on valmisteltu aseman StorSimple virtual laitteessa on 30 Teratavua.
  3. Valitse äänenvoimakkuutta **Käyttötyyppi** . Jos käytössäsi on Porrastettu äänenvoimakkuuden arkistointia tietojen **aseman vähemmän usein käytetyt arkistointia tietojen käyttöoikeus** -valintaruudun muuttuu äänenvoimakkuutta kopioinnin peruuttaminen-lohkon koon 512 kt. Jos et valitse tämä vaihtoehto, vastaava Porrastettu asema käyttää lohkon koko on 64 Kilotavua. Kopioinnin peruuttaminen lohkon suurentaminen avulla suorittamiseen suuri arkistointia tietojen siirto pilveen laite. (Porrastettu tietomääristä aiemmin kutsuttiin ensisijainen tietomääristä.)
  5. Napsauttamalla nuolta ![nuolikuvaketta](./media/storsimple-manage-volumes/HCS_ArrowIcon.png) **Lisäasetukset** -sivun.

        ![Lisää äänenvoimakkuutta ohjattu Lisäasetukset](./media/storsimple-manage-volumes/AddVolume2.png)

5. **Lisäasetukset**-kohdassa Lisää uusi access-ohjausobjektin tietue (ACR):

  1. Valitse access ohjausobjektin tietue (ACR) avattavasta luettelosta. Vaihtoehtoisesti voit lisätä uuden ACR. ACRs määrittävät, mitkä isännät käyttää jälkeen asemat vertaamalla host IQN lueteltu tietueen kanssa.
  2. On suositeltavaa ottaa oletusarvon varmuuskopion valitsemalla **tämän aseman oletusarvon varmuuskopion Ota käyttöön** -valintaruutu.
   3. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-volumes/HCS_CheckIcon.png) Jotta voit luoda äänenvoimakkuuden määritettyjä asetuksia.

Uusi asema on nyt valmis käytettäväksi.

## <a name="modify-a-volume"></a>Muokata asemaa

Muokata asemaa, kun haluat laajentaa tai muuttaa isännät, joka käyttää äänenvoimakkuuden.

> [AZURE.IMPORTANT]
>
> - Jos muokkaat Asemakoko laitteeseen, Asemakoko on muutettava isännän paikan päällä.
> - Isännän tässä kuvatut vaiheet koskevat Windows Server 2012 (2012R2). Ohjeet Linux tai host muut käyttöjärjestelmät ovat erilaisia. Viittaavat Host (isäntä)-käyttöjärjestelmän ohjeita toiseen käyttöjärjestelmä isännän äänenvoimakkuuden muokattaessa.

### <a name="to-modify-a-volume"></a>Jos haluat muokata asemaa

1. Valitse **laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilö** -välilehti. Tällä sivulla näkyy taulukkomuodossa kaikki aseman säilöt, jotka liittyvät laitteen.

2. Valitse aseman säilön ja kaikki säilöön asemat luettelo näkyviin napsauttamalla sitä.

3. Valitse **asemat** -sivulla Valitse asema ja sitten **Muokkaa**.

4. Ohjatun Muokkaa aseman luomisen **Perusasetukset**toimi seuraavasti:

  - Muokkaa **nimi** ja **tyyppi** , jos haluat muokata arkistointia aseman Porrastettu aseman valitsemalla **Käytä tämän voimakkuutta vähemmän usein käytetyt arkistointia tietoja** -valintaruutu, jos 512 kt äänenvoimakkuutta kopioinnin peruuttaminen-lohkon koon muuttamiseen.
  - Suurentaa **kapasiteetin valmistelun yhteydessä**. **Valmisteltu kapasiteetin** vain voidaan lisätä. Asema ei voi pienentää sen luomisen jälkeen.

    > [AZURE.NOTE] Et voi muuttaa aseman säilöä sen jälkeen, kun se on määritetty aseman.

5. Valitse **Lisäasetukset**toimi seuraavasti:

  - Muokkaa ACRs, äänenvoimakkuuden on offline-tilassa. Levy on online-tilassa, jos haluat siirtää verkkosivuston offline-tilassa ensin. Lisätietoja ohjeita [otetaan tilavuus offline-tilassa](#take-a-volume-offline) ennen ACR muokkaaminen.
  - Muokkaa ACRs luetteloa, kun ääni on offline-tilassa.

    > [AZURE.NOTE] Et voi muuttaa aseman **käyttöön tämän aseman oletusarvoisesti varmuuskopio** -asetus.

6. Tallenna muutokset valitsemalla valintaruutu-kuvake ![tarkistuksen kuvake](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Azure perinteinen portaalin näkyy päivittäminen äänenvoimakkuutta-viesti. Se näkyy onnistui-sanoma, kun äänenvoimakkuuden on päivitetty.

7. Jos ovat laajentaminen asema, suorita Windows tietokoneessa seuraavat toimet:

   1. Valitse **Tietokoneenhallinta** ->**Levynhallinta**.
   2. Napsauta **Levynhallinta** ja valitse **Asemat**.
   3. Valitse levyjen-luettelosta asema, jonka olet päivittänyt hiiren kakkospainikkeella ja valitse sitten **Laajenna asema**. Laajenna asema ohjattu toiminto käynnistyy. Valitse **Seuraava**.
   4. Suorita ohjattu hyväksy oletusarvot. Kun ohjattu toiminto on valmis, äänenvoimakkuuden pitäisi näkyä parantavat kokoa.

![Video käytettävissä](./media/storsimple-manage-volumes/Video_icon.png) **Video käytettävissä**

Katso video, jossa kerrotaan, miten voit laajentaa aseman, napsauta [tätä](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Tilavuus offline-tilaan

Joudut ehkä siirtää aseman offline-tilaan, kun aiot muokata tai poistaa sen. Kun aseman on offline-tilassa, ei ole käytettävissä luku-ja kirjoitusoikeudet. Sinun on otettava offline-tilassa äänenvoimakkuuden isännän ja laitteeseen. Seuraavien toimien aseman offline-tilaan.

### <a name="to-take-a-volume-offline"></a>Tilavuus offline-tilaan

1. Varmista, että poistettavaan äänenvoimakkuuden ei ole käytössä ennen siihen offline-tilassa.

2. Ottaa isäntä äänenvoimakkuuden offline-tilassa ensimmäisen. Jättää pois kaikki riski tietovirheitä asemassa. Tietyt vaiheet viitata host käyttöjärjestelmäkohtaisia ohjeita.

3. Kun isäntä on offline-tilassa, kestää äänenvoimakkuuden laitteeseen offline-tilaan suorittamalla seuraavat vaiheet:

  1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti. **Äänenvoimakkuuden säilöt** -välilehdessä näkyvät taulukkomuodossa kaikki aseman säilöt, jotka liittyvät laitteen.
  2. Valitse aseman säilön ja kaikki säilöön asemat luettelo näkyviin napsauttamalla sitä.
  3. Valitse asema ja valitse **offline-tilaan**.
  4. Kun ohjelma pyytää vahvistusta, valitse **Kyllä**. Äänenvoimakkuuden pitäisi nyt olla offline-tilassa.

    Kun aseman on offline-tilassa, **Ota käyttöön** -vaihtoehto on käytettävissä.

> [AZURE.NOTE] **Ota Offline** -komento lähettää pyynnön laitteen äänenvoimakkuuden offline-tilaan. Jos isännät silti käyttävät äänenvoimakkuuden, tuloksena on katkennut yhteyksiä, mutta ottaen äänenvoimakkuuden offline-tilassa ei onnistu.

## <a name="delete-a-volume"></a>Aseman poistaminen

> [AZURE.IMPORTANT] Voit poistaa aseman vain, jos se on offline-tilassa.

Suorita seuraavat toimet voit poistaa aseman.

### <a name="to-delete-a-volume"></a>Voit poistaa aseman

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Valitse aseman säilö, jossa on asema, jonka haluat poistaa. Valitse aseman säilön ja **asemat** tietokantasivun.

3. Sarakemuodossa, jossa näkyvät kaikki tämän säilön liittyvät asemat. Tarkista tila määrän, jonka haluat poistaa. Jos haluat poistaa asema ei ole offline-tilassa, siirtää verkkosivuston offline-tilassa ensin noudattamalla [otetaan tilavuus offline-tilassa](#take-a-volume-offline).

4. Kun äänenvoimakkuuden on offline-tilassa, valitse sivun alareunassa **Poista** .

5. Kun ohjelma pyytää vahvistusta, valitse **Kyllä**. Äänenvoimakkuuden poistetaan ja **asemat** -sivu näyttää päivitettyjen luettelon asemista säilöön.

## <a name="monitor-a-volume"></a>Näytön aseman

Äänenvoimakkuuden seuranta voit kerätä aseman I/O-ryhmän tilastotiedot. Seuranta on käytössä oletusarvoisesti, jonka luot ensin 32 asemat. Lisää tietomääristä seuranta on poissa käytöstä oletusarvoisesti. Valvonta kloonatun asemista myös käytöstä oletusarvoisesti.

Seuraavien toimien käyttöön ottaminen tai käytöstä aseman seuranta.

### <a name="to-enable-or-disable-volume-monitoring"></a>Ottaa käyttöön tai poistaa aseman seuranta

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Valitse aseman säilö, jossa äänenvoimakkuuden sijaitsee ja valitse sitten aseman säilön ja **asemat** tietokantasivun.

3. Kaikki tässä säilössä liittyvät asemat näkyvät taulukkomuotoinen-näytössä. Valitse ja valitse äänenvoimakkuutta tai asema Kloonaa.

4. Valitse sivun alareunassa **Muokkaa**.

5. Muokata asemaa ohjatussa **Perusasetukset**-kohdassa Valitse **ottaminen käyttöön** tai **poistaa käytöstä** **Seuranta** avattavasta luettelosta.

    ![Muokata asemaa perusasetukset](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)


## <a name="next-steps"></a>Seuraavat vaiheet

- Lue, miten [Kloonaa StorSimple äänenvoimakkuutta](storsimple-clone-volume.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
