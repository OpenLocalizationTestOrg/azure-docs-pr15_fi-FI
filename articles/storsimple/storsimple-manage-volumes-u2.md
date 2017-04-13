<properties
   pageTitle="Hallitse StorSimple-asemat (U2) | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan, miten voit lisätä, muokata, valvoa ja poistaa StorSimple asemat ja offline-tilaan niitä tarvittaessa."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/28/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>StorSimple hallinta-palvelun avulla voit hallita asemat (päivitys 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan, miten StorSimple hallinta-palvelun avulla voit luoda ja hallita asemista StorSimple ja StorSimple virtual laitteen päivityksen 2 asennettuna.

StorSimple hallintapalvelu on perinteinen Azure-portaalissa, jolla voit hallita StorSimple ratkaisu yksittäisen web-liittymän laajennus. Lisäksi hallinta asemat, voit tehdä StorSimple hallintapalvelu luominen ja StorSimple palveluiden hallinta, tarkastella ja hallita laitteita, Näytä ilmoitukset- ja tarkastella ja hallita varmuuskopion käytännöt ja varmuuskopion luettelon.

## <a name="volume-types"></a>Avauksen ja vaihdon tyypit

StorSimple asemat voi olla:

- **Kiinnitetty paikallisesti asemat**: asemat tiedot säilyvät StorSimple paikallisen laitteen aina.
- **Tiered asemat**: tietojen asemat voi spill pilveen.

Arkistointia äänenvoimakkuus on Porrastettu määrän. Kopioinnin peruuttaminen lohkon koon suurempi versio arkistointia tietomääristä käytettäviä avulla voit siirtää tietoja suurempi osia pilveen laite. 

Jos tarpeen, voit muuttaa äänenvoimakkuuden tyypin paikallinen tasoisen tai -tasoisen paikallisiin. Jos haluat lisätietoja, siirry [vaihtaminen asematyyppi](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Paikallisesti kiinnitetty tietomääristä

Paikallisesti kiinnitetty asemat ovat täysin valmistellun asemat, jotka eivät ole taso tiedot pilvipalveluun, siten varmistaa, että paikallinen takaa ensisijainen tietoja erillään cloud yhteys. Paikallisesti kiinnitetty tietomääristä ei deduplicated ja pakattu. kuitenkin paikallisesti kiinnitetty tietomääristä tilannevedosten deduplicated. 

Paikallisesti kiinnitetty tietomääristä täysin valmisteltu; sen vuoksi sinun on oltava tarpeeksi tilaa laitteen kun luot ne. Voit valmistella ylöspäin enimmäiskoko 8 TT StorSimple 8100 laitteessa ja 20 Teratavua 8600 laitteeseen paikallisesti kiinnitetty asemat. StorSimple Varaa laitteen tilannevedoksia, metatietojen ja tietojen käsittely paikallisen jäljellä olevan tilan. Voit suurentaa suurin käytettävissä oleva tila paikallisesti kiinnitetty aseman kokoa, mutta ei voi pienentää äänenvoimakkuutta, kun luonut kokoa.

Kun luot paikallisesti kiinnitetty äänenvoimakkuutta, porrastettu tietomääristä luontia varten käytettävissä olevaa tilaa on vähennetty. Vastaavasti: Jos aiemmin luodut Porrastettu asemat-tilaa varten luominen paikallisesti kiinnitetyt asemat on pienempi kuin edellä mainittujen enimmäispituus. Lisätietoja asemat on [usein kysyttyjä kysymyksiä paikallisesti kiinnitetty asemat](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Porrastettu tietomääristä

Porrastettu asemat ovat levinneet valmistellun asemat, jossa usein käytetyt tiedot ovat varmasti paikallisen laitteen ja vähemmän usein käytettyjä tietoja automaattisesti tasoisen pilveen. Ohuen valmistelu on virtualization tekniikka, jossa on suurempi kuin fyysinen resurssien käytettävissä. Sen sijaan, että varaaminen riittävästi tallennustilan etukäteen StorSimple käyttää ohuen valmistelu varata samalla tarpeeksi tilaa nykyisen ehtojen mukaan. Pilvitallennustilaa joustavasti laatu helpottaa tätä tapaa, koska StorSimple voit suurentaa tai pienentää pilvitallennustilaa täyttämään muuttaminen.

Jos käytössäsi on Porrastettu äänenvoimakkuuden arkistointia tietojen **aseman vähemmän usein käytetyt arkistointia tietojen käyttöoikeus** -valintaruudun muuttuu äänenvoimakkuutta kopioinnin peruuttaminen-lohkon koon 512 kt. Jos et valitse tämä vaihtoehto, vastaava Porrastettu asema käyttää lohkon koko on 64 Kilotavua. Kopioinnin peruuttaminen lohkon suurentaminen avulla suorittamiseen suuri arkistointia tietojen siirto pilveen laite.

>[AZURE.NOTE] StorSimple päivitystä edeltävässä 2 versiolla luotu arkistointia asemat on tuotu kuin Porrastettu arkistointia valintaruutu on valittuna.

### <a name="provisioned-capacity"></a>Valmistellun kapasiteetti

Seuraavassa taulukossa on suurin valmistellun kapasiteetin laitteen ja voimakkuutta mistäkin viittaavat. (Huomaa, että paikallisesti kiinnitetty asemat eivät ole käytettävissä virtual laitteessa.)

|             | Porrastettu aseman enimmäiskoko | Suurin paikallisesti kiinnitetyt Asemakoko |
|-------------|----------------------------|------------------------------------|
| **Fyysisten laitteiden** |       |       |
| 8100                 | 64 TT | 8 TT |
| 8600                 | 64 TT | 20 TERATAVUA |
| **Virtuaalinen laitteet**  |       |       |
| 8010                | 30 TT | PUUTTUU   |
| 8020               | 64 TT | PUUTTUU   |

## <a name="the-volumes-page"></a>Asemat-sivu

**Asemat** -sivun avulla voidaan hallita sitä, että laitteistokäynnistäjät (palvelimet) Microsoft Azure StorSimple laitteessa valmisteltu tallennustilan asemat. Se näyttää luettelon asemista StorSimple laitteen.

 ![Asemat-sivu](./media/storsimple-manage-volumes-u2/VolumePage.png)

Asema koostuu määritteet sarja:

- **Levyn nimi** – kuvaava nimi, joka on oltava yksilöllinen ja auttaa tunnistaa äänenvoimakkuutta. Tätä nimeä käytetään myös valvontaraportit, kun voit suodattaa tietyn aseman.

- **Tila** – voi olla online- tai offline-tilassa. Jos asema ei offline-tilassa, ei ole näkyvissä laitteistokäynnistäjät (palvelimet), joilla on käytettävä äänenvoimakkuuden käyttöoikeus.

- **Kapasiteetti** – määrittää tietoihin, jotka on tallennettu käynnistäjä (palvelin) kokonaismäärä. Paikallisesti kiinnitetyt asemat täysin valmisteltu ja sijaitsevat StorSimple laitteeseen. Porrastettu tietomääristä levinneet valmisteltu ja tietojen deduplicated. Levinneet valmistellun asemat, jossa laite ei valmiiksi varata fyysinen tallennustilaa sisäisesti tai pilveen määritetyn aseman kapasiteetin mukaan. Äänenvoimakkuuden kapasiteetti on varattu ja kulutettu pyydettäessä.

- **Kirjoita** – ilmaisee, onko äänenvoimakkuuden **Tiered** (oletusasetus) tai **paikallisesti kiinnitetty**.

- **Varmuuskopion** – ilmaisee onko aseman olemassa varmuuskopio oletuskäytäntö.

- **Accessin** – määrittää laitteistokäynnistäjät (palvelimet), joilla on käyttöoikeus asemaan. Laitteistokäynnistäjät, jotka eivät kuulu access ohjausobjektin tietue (ACR), joka on liitetty äänenvoimakkuuden ei näy äänenvoimakkuutta.

- **Seuranta** – määrittää, onko aseman seurataan. Asema on oletusarvoisesti käytössä, kun se on luotu seuranta. Valvonta on, mutta, voidaan poistaa käytöstä äänenvoimakkuuden Kloonaa. Seurannan aseman käyttöön ohjeiden [näytön aseman](#monitor-a-volume). 

Tässä opetusohjelmassa ohjeiden avulla voit suorittaa seuraavia tehtäviä:

- Lisää aseman 
- Muokata asemaa 
- Äänenvoimakkuus-tyypin muuttaminen
- Aseman poistaminen 
- Tilavuus offline-tilaan 
- Näytön aseman 

## <a name="add-a-volume"></a>Lisää aseman

Olet [luonut aseman](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) StorSimple-ratkaisun käyttöönoton aikana. Asema lisätään samalla tavalla.

#### <a name="to-add-a-volume"></a>Jos haluat lisätä aseman

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Äänenvoimakkuuden säilön luettelosta ja kaksoisnapsauta sitä käyttämään säilö liittyvät asemat.

3. Valitse sivun alareunassa **Lisää** . Lisää äänenvoimakkuutta ohjattu toiminto käynnistyy.

     ![Lisää äänenvoimakkuutta ohjattu perusasetukset](./media/storsimple-manage-volumes-u2/TieredVolEx.png)

4. Lisää äänenvoimakkuutta ohjatun toiminnon **Perusasetukset**-kohdassa seuraavasti:

  1. Lisää äänenvoimakkuutta **nimi** .
  2. Valitse **Käyttötyyppi** avattavasta luettelosta. Saat toiminnoista, jotka edellyttävät tiedot käytettävissä paikallisesti laitteessa koko ajan, valitse **Kiinnitetyt paikallisesti**. Kaikki muut tiedot Valitse **Tiered**. (**Tiered** on oletusarvo.)
  3. Jos olet valinnut **Tiered** vaiheessa 2, voit valita määrittäminen arkistointia aseman **aseman vähemmän usein käytetyt arkistointia tietojen käyttöoikeus** -valintaruutu.
  4. Kirjoita **Valmisteltu kapasiteetin** gt tai TT äänenvoimakkuutta. Katso laitteen ja voimakkuutta mistäkin enimmäiskoot [Provisioned kapasiteetti](#provisioned-capacity) . Tarkista, voit määrittää, kuinka paljon tallennustilaa todella käytettävissä laitteen **Käytettävissä oleva kapasiteetti** .

5. Napsauttamalla nuolta![Nuolikuvaketta](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Jos olet määrittämässä paikallisesti kiinnitetty äänenvoimakkuutta, näet seuraavan sanoman.

    ![Avauksen ja vaihdon tyyppi viestin muuttaminen](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
   
5. Napsauttamalla nuolta ![nuolikuvaketta](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)uudelleen, voit siirtyä **Lisäasetukset** -sivulle.

    ![Lisää äänenvoimakkuutta ohjattu Lisäasetukset](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>

6. **Lisäasetukset**-kohdassa Lisää uusi access-ohjausobjektin tietue (ACR):
  
  1. Valitse access ohjausobjektin tietue (ACR) avattavasta luettelosta. Vaihtoehtoisesti voit lisätä uuden ACR. ACRs määrittävät, mitkä isännät käyttää jälkeen asemat vertaamalla host IQN lueteltu tietueen kanssa. Jos et määritä ACR, näet seuraavan sanoman.

        ![Määritä ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)

  2. On suositeltavaa, että valitset **oletusarvoisesti varmuuskopio tämän aseman käyttöön** -valintaruutu.
  3. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Jotta voit luoda äänenvoimakkuuden määritettyjä asetuksia.

Uusi asema on nyt valmis käytettäväksi.

>[AZURE.NOTE] Jos Luo paikallisesti kiinnitetty asema ja luo sitten toinen paikallisesti kiinnitetty asema heti jälkeenpäin työt suoritetaan peräkkäin aseman luomisen. Äänenvoimakkuuden luominen työtä päättyminen ennen seuraavan aseman luominen työn voi alkaa.

## <a name="modify-a-volume"></a>Muokata asemaa

Muokata asemaa, kun haluat laajentaa tai muuttaa isännät, joka käyttää äänenvoimakkuuden.

> [AZURE.IMPORTANT] 
>
> - Jos muokkaat Asemakoko laitteeseen, Asemakoko on muutettava isännän paikan päällä. 
> - Isännän tässä kuvatut vaiheet koskevat Windows Server 2012 (2012R2). Ohjeet Linux tai host muut käyttöjärjestelmät ovat erilaisia. Viittaavat Host (isäntä)-käyttöjärjestelmän ohjeita toiseen käyttöjärjestelmä isännän äänenvoimakkuuden muokattaessa. 

#### <a name="to-modify-a-volume"></a>Jos haluat muokata asemaa

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Äänenvoimakkuuden säilön luettelosta ja kaksoisnapsauta sitä tarkastelemaan säilö liittyvät asemat.

3. Valitse asema ja valitse sivun alareunassa **Muokkaa**. Muokkaa aseman ohjattu toiminto käynnistyy.

4. Ohjatun Muokkaa aseman luomisen **Perusasetukset**toimi seuraavasti:

  - Muokkaa **nimi**.
  - Muuntaa **Käyttötyyppi** paikallisesti kiinnitetty, tasoisen tai -tasoisen kiinnitetyt paikallisesti (Katso lisätietoja [aseman tyypin muuttaminen](#change-the-volume-type) ).
  - Suurentaa **kapasiteetin valmistelun yhteydessä**. **Valmisteltu kapasiteetin** vain voidaan lisätä. Asema ei voi pienentää sen luomisen jälkeen.

5. Valitse **Lisäasetukset**-voit muokata ACR, äänenvoimakkuuden on offline-tilassa. Levy on online-tilassa, jos haluat siirtää verkkosivuston offline-tilassa ensin. Lisätietoja ohjeita [otetaan tilavuus offline-tilassa](#take-a-volume-offline) ennen ACR muokkaaminen.

    > [AZURE.NOTE] Et voi muuttaa aseman **käyttöön oletusarvoisesti varmuuskopio** -asetus.

6. Tallenna muutokset valitsemalla valintaruutu-kuvake ![tarkistuksen kuvake](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Azure perinteinen portaalin näkyy päivittäminen äänenvoimakkuutta-viesti. Se näkyy onnistui-sanoma, kun äänenvoimakkuuden on päivitetty.

7. Jos ovat laajentaminen asema, suorita Windows tietokoneessa seuraavat toimet:

   1. Valitse **Tietokoneenhallinta** ->**Levynhallinta**.
   2. Napsauta **Levynhallinta** ja valitse **Asemat**.
   3. Valitse levyjen-luettelosta asema, jonka olet päivittänyt hiiren kakkospainikkeella ja valitse sitten **Laajenna asema**. Laajenna asema ohjattu toiminto käynnistyy. Valitse **Seuraava**.
   4. Suorita ohjattu hyväksy oletusarvot. Kun ohjattu toiminto on valmis, äänenvoimakkuuden pitäisi näkyä parantavat kokoa.

    >[AZURE.NOTE] Jos Laajenna paikallisesti kiinnitetty äänenvoimakkuus ja laajenna sitten toiseen paikallisesti kiinnitetty aseman välittömästi jälkeenpäin aseman laajentamista työt suoritetaan peräkkäin. Äänenvoimakkuuden laajentamista työtä päättyminen ennen seuraavan aseman laajentamista työn voi alkaa.

![Video käytettävissä](./media/storsimple-manage-volumes-u2/Video_icon.png) **Video käytettävissä**

Katso video, jossa kerrotaan, miten voit laajentaa aseman, napsauta [tätä](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Äänenvoimakkuus-tyypin muuttaminen

Voit muuttaa aseman tyypin, paikallisesti kiinnitetyt tasoisen tai -paikallisesti kiinnitetty, porrastettu. Kuitenkin tämä muuntaminen ei saa olla usein käytetyt esiintymä. Syitä, joiden muuntamisen-asema, paikallisesti kiinnitetyt tasoisen ovat seuraavat:

- Paikallinen oikeudet koskevat tietojen käytettävyys ja suorituskyky
- Cloud viiveet suurempia ja cloud yhteysongelmat poistamiseksi.

Yleensä nämä ovat pieniä aiemmin luodut asemat, jota haluat käyttää usein. Paikallisesti kiinnitetty aseman täysin valmistelun yhteydessä, kun se luodaan. Jos muunnat Porrastettu aseman paikallisesti kiinnitetty asemaan StorSimple varmistaa, että sinulla on tarpeeksi tilaa laitteen ennen sen muuntamista. Jos sinulla ei ole riittävästi tilaa, näyttöön tulee virhe ja toiminto peruutetaan. 

> [AZURE.NOTE] Ennen kuin aloitat muunto-tasoisen, paikallisesti kiinnitetty, varmista, että pidät yhteyttä työmääriä tilaa vaatimuksia. 

Haluat ehkä muuttaa paikallisesti kiinnitetty äänenvoimakkuutta Porrastettu äänenvoimakkuutta, jos tarvitset lisää tilaa valmistelu muita levyjä. Kun muunnat paikallisesti kiinnitetty aseman tasoisen julkaistut kapasiteetin koon laitteen suurenee käytettävissä kapasiteetti. Yhteysongelmat estävät aseman muuntaminen paikallisen tyypistä Porrastettu tyyppi, paikallinen äänenvoimakkuuden toimia Porrastettu asemaa, ennen kuin muuntaminen on valmis. Tämä johtuu siitä tietoja voi olla spilled pilveen. Spilled tiedot säilyvät paikallisen tilaa laitteeseen, joka ei voi vapauttaa, kunnes toiminto käynnistetään uudelleen ja valmis.

>[AZURE.NOTE] Muuntaa aseman voi kestää jonkin aikaa ja et voi peruuttaa muunto, kun se käynnistyy. Äänenvoimakkuuden pysyy online-muunnon aikana ja voit tehdä varmuuskopioita, mutta ei voi laajentaa tai palauttaa äänenvoimakkuuden, kun muunnos on meneillään.  

Muunnos Porrastettu paikallisesti kiinnitetty asemaan voi heikentää laitteen suorituskykyä. Lisäksi seuraavat seikat voi parantaa muuntamista suorittamiseen kuluvaa aikaa:

- Riittämätön kaistanleveys on.

- Ei ole varmuuskopiota.

Pienennä tehosteita, kuten seikoista voi olla:

- Tarkista oman Kaistanleveyden rajoittimen käytännöt ja varmista, että oma 40 Mbps kaistanleveys on käytettävissä.
- Ajoita muunto on myöhemmin.
- Cloud kuvata ennen muuntamista.

Jos muunnat useita asemat (tukevat eri toiminnoista)-asema muuntaminen olisi priorisoida siten, että suurempi prioriteetti asemat muunnetaan ensin. Esimerkiksi pitäisi muuntaa asemat, jotka isännöivät näennäiskoneiden (VMs) tai SQL-työmääriä asemat ennen muunnat tiedoston resurssin työmääriä asemat.

#### <a name="to-change-the-volume-type"></a>Äänenvoimakkuus-tyypin muuttaminen

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Äänenvoimakkuuden säilön luettelosta ja kaksoisnapsauta sitä tarkastelemaan säilö liittyvät asemat.

3. Valitse asema ja valitse sivun alareunassa **Muokkaa**. Muokkaa aseman ohjattu toiminto käynnistyy.

4. Muuta käyttötyyppi **Perusasetukset** -sivun valitsemalla Uusi **Käyttötyyppi** avattavasta luettelosta.

    - Jos olet muuttamassa tyyppi **paikallisesti kiinnitetty**, StorSimple Tarkista, onko riittää.
    - Jos olet muuttamassa tyyppi **Tiered** ja aseman käytetään arkistointia tiedot, valitse **Käytä tätä voimakkuutta vähemmän usein käytetyt arkistointia tietoja** -valintaruutu.

        ![Arkisto-valintaruutu](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)

5. Napsauttamalla nuolta ![nuolikuvaketta](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) **Lisäasetukset** -sivun. Jos olet määrittämässä paikallisesti kiinnitetty äänenvoimakkuutta, näyttöön tulee seuraava sanoma.

    ![Avauksen ja vaihdon tyyppi viestin muuttaminen](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)

6. Napsauttamalla nuolta ![nuolikuvaketta](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) uudelleen, jos haluat jatkaa.

7. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Aloita muuntaminen. Azure portaalin näkyy päivittäminen äänenvoimakkuutta-viesti. Se näkyy onnistui-sanoma, kun äänenvoimakkuuden on päivitetty.

## <a name="take-a-volume-offline"></a>Tilavuus offline-tilaan

Joudut ehkä siirtää aseman offline-tilaan, kun aiot muokata tai poistaa sen. Kun aseman on offline-tilassa, ei ole käytettävissä luku-ja kirjoitusoikeudet. Sinun on otettava offline-tilassa äänenvoimakkuuden isännän ja laitteeseen. 

#### <a name="to-take-a-volume-offline"></a>Tilavuus offline-tilaan

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

#### <a name="to-delete-a-volume"></a>Voit poistaa aseman

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Valitse aseman säilö, jossa on asema, jonka haluat poistaa. Valitse aseman säilön ja **asemat** tietokantasivun.

3. Sarakemuodossa, jossa näkyvät kaikki tämän säilön liittyvät asemat. Tarkista tila määrän, jonka haluat poistaa. Jos haluat poistaa asema ei ole offline-tilassa, siirtää verkkosivuston offline-tilassa ensin noudattamalla [otetaan tilavuus offline-tilassa](#take-a-volume-offline).

4. Kun äänenvoimakkuuden on offline-tilassa, valitse sivun alareunassa **Poista** .

5. Kun ohjelma pyytää vahvistusta, valitse **Kyllä**. Äänenvoimakkuuden poistetaan ja **asemat** -sivu näyttää päivitettyjen luettelon asemista säilöön.

    >[AZURE.NOTE]Jos poistat paikallisesti kiinnitetty äänenvoimakkuutta, uusia levytilaa eivät päivity välittömästi. StorSimple hallintapalvelu päivittää paikallisen levytilaa säännöllisin väliajoin. Suosittelemme odottamaan hetken, ennen kuin yrität luoda uuden äänenvoimakkuutta.<br> Lisäksi jos paikallisesti kiinnitetty aseman poistaminen ja poista sitten toiseen paikallisesti kiinnitetty aseman heti jälkeenpäin työt suoritetaan peräkkäin aseman poisto. Äänenvoimakkuuden poistaminen työtä päättyminen ennen seuraavan aseman poistaminen työn voi alkaa.
 
## <a name="monitor-a-volume"></a>Näytön aseman

Äänenvoimakkuuden seuranta voit kerätä aseman I/O-ryhmän tilastotiedot. Seuranta on käytössä oletusarvoisesti, jonka luot ensin 32 asemat. Lisää tietomääristä seuranta on poissa käytöstä oletusarvoisesti. Valvonta kloonatun asemista myös käytöstä oletusarvoisesti.

Seuraavien toimien käyttöön ottaminen tai käytöstä aseman seuranta.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Ottaa käyttöön tai poistaa aseman seuranta

1. **Laitteet** -sivulla Valitse laite, kaksoisnapsauttamalla sitä ja valitse **Aseman säilöt** -välilehti.

2. Valitse aseman säilö, jossa äänenvoimakkuuden sijaitsee ja valitse sitten aseman säilön ja **asemat** tietokantasivun.

3. Kaikki tässä säilössä liittyvät asemat näkyvät taulukkomuotoinen-näytössä. Valitse ja valitse äänenvoimakkuutta tai asema Kloonaa.

4. Valitse sivun alareunassa **Muokkaa**.

5. Muokata asemaa ohjatussa **Perusasetukset**-kohdassa Valitse **ottaminen käyttöön** tai **poistaa käytöstä** **Seuranta** avattavasta luettelosta.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue, miten [Kloonaa StorSimple äänenvoimakkuutta](storsimple-clone-volume.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.

 
