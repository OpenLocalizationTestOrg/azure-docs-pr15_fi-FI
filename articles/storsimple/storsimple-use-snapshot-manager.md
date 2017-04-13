<properties 
   pageTitle="StorSimple tilannevedoksen hallinnan käyttöliittymä | Microsoft Azure"
   description="StorSimple tilannevedoksen hallinnan käyttöliittymästä ja kerrotaan, miten voit käyttää sitä varmuuskopiointityön ja varmuuskopion luettelon."
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
   ms.date="04/25/2016"
   ms.author="v-sharos" />

# <a name="storsimple-snapshot-manager-user-interface"></a>StorSimple tilannevedoksen hallinnan käyttöliittymä

## <a name="overview"></a>Yleiskatsaus

StorSimple tilannevedoksen hallinta on intuitiivisen käyttöliittymän, jonka avulla ja hallita varmuuskopiot. Tässä opetusohjelmassa esitellään käyttöliittymän ja sitten kerrotaan, miten voit käyttää eri osat. Yksityiskohtainen kuvaus StorSimple tilannevedoksen hallinta-kohdassa [Mikä on StorSimple tilannevedoksen hallinta?](storsimple-what-is-snapshot-manager.md)

### <a name="console-description"></a>Konsolin kuvaus

Voit tarkastella käyttöliittymän valitsemalla työpöydän StorSimple tilannevedoksen hallinta-kuvaketta. Konsoli-ikkunassa näkyy seuraavassa kuvassa esitetyllä tavalla.

![StorSimple tilannevedoksen hallinta-ruudut](./media/storsimple-use-snapshot-manager/HCS_SSM_gui_panes.png)

Konsoli-ikkunassa on viisi tärkeintä elementti. Valitse jokaisen osan tarkka kuvaus vastaava linkki.

- [Valikkorivi](#menu-bar) 
- [Työkalurivin](#tool-bar) 
- [Laajuus-ruutu](#scope-pane) 
- [Tulokset-ruutu](#results-pane) 
- [Asiakirjatoimet-ruutu](#actions-pane) 

Lisäksi StorSimple tilannevedoksen hallinta tukee [pikanäppäimet ja pikakuvakkeiden määrän](#keyboard-navigation-and-shortcuts).

### <a name="console-accessibility"></a>Konsolin helppokäyttötoiminnot

StorSimple tilannevedoksen hallinnan käyttöliittymä tukee Windows-käyttöjärjestelmän ja Microsoft Management Console (MMC) sekä joitakin StorSimple tilannevedoksen hallinta – pikanäppäimiä helppokäyttötoiminnoista. 

- Windowsin helppokäyttötoimintojen kuvauksen Siirry [Windowsin pikanäppäimet](https://support.microsoft.com/kb/126449). 

- Siirry MMC helppokäyttötoiminnot kuvauksen, [MMC 3.0 helppokäyttötoiminnot](https://technet.microsoft.com/library/cc766075.aspx)

- Siirry StorSimple tilannevedoksen Managerin helppokäyttötoiminnot kuvauksen, [pikanäppäimet ja pikanäppäimiä](#keyboard-navigation-and-shortcuts).

## <a name="menu-bar"></a>Valikkorivi

Valikkorivin konsoli-ikkunan yläreunassa sisältää [tiedoston](#file-menu), [toiminto](#action-menu), [Näytä](#view-menu), [Suosikit](#favorites-menu), [ikkunan](#window-menu)ja valikkojen [avulla](#help-menu) .

Valitse minkä tahansa kohteen, saat luettelon käytettävissä olevista komennoista valikon valikkorivillä. Seuraavassa esimerkissä esitetään valikkorivillä valittuna **Näytä** -valikossa.

![Näytä-valikko on valittuna](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png)

### <a name="file-menu"></a>Tiedosto-valikko

**Tiedosto** sisältää vakio Microsoft Management Console (MMC)-komentoja.

#### <a name="menu-access"></a>Valikoiden käyttö

Voit tarkastella **Tiedosto** valitsemalla Valitse valikkoriviltä **Tiedosto** . Seuraava valikko tulee näkyviin.

![StorSimple tilannevedoksen hallinta-tiedosto](./media/storsimple-use-snapshot-manager/HCS_SSM_FileMenu.png) 

#### <a name="menu-description"></a>Valikon kuvaus

Seuraavassa taulukossa on kuvattu näkyviä kohteita **Tiedosto** -valikosta.

| Valikkovaihtoehto | Kuvaus |
|:----------|:-------------|
| Uusi       | Valitse **Uusi** , jos haluat luoda uuden konsolin mukaan StorSimple tilannevedos-hallinta. |
| Avaa      | Valitse Avaa aiemmin luotu konsoli **Avaa** . |
| Tallenna      | Valitse Tallenna nykyinen konsoli **Tallenna** . |
| Tallenna nimellä   | Valitse **Tallenna nimellä** luomaan nykyisen konsolin uusi, uudelleennimetty esiintymää. **Tallenna nimellä** -vaihtoehdon avulla voit mukauttaa näkymän ja tallentaa sen myöhempää noutamista varten. Voit esimerkiksi luoda StorSimple tilannevedoksen hallinta laajennukset, jotka viittaavat tiettyihin palvelimiin. |
| Lisää tai poista laajennus | Valitse **Lisää tai poista-laajennuksen** lisääminen tai poistaminen laajennukset ja järjestää solmujen **laajuus** -ruudussa. Lisätietoja Valitse [Lisää, poista, Järjestä - laajennukset](https://technet.microsoft.com/library/cc722035.aspx)ja MMC 3.0-laajennusten. |
| Asetukset   | Valitse **asetukset** -konsolin kuvakkeelle, käyttäjän access-tila ja käyttöoikeuksien määrittäminen tai poistaminen tiedostot niin, että vapaata kiintolevytilaa. |
| Liiketoimintatiedot luettelo | Valitse liikerata numeroidun luettelon Avaa tiedosto, jonka olet avannut hiljattain. |
| Lopeta      | Valitse Sulje **Tiedosto** -valikosta **Lopeta** . |
 
### <a name="action-menu"></a>Toimintovalikko

Valitse käytettävissä olevat toiminnot käyttämällä **toiminto** -valikosta. Käytettävissä olevat kohteet määräytyvät **laajuus** -ruudussa tai **tulokset** -ruudun valinta.

#### <a name="menu-access"></a>Valikoiden käyttö

Voit tarkastella **toiminto** -valikosta jokin seuraavista:

- Napsauta hiiren kakkospainikkeella **laajuus** -ruudussa tai **tulosruudussa** .

- Valitse kohde **laajuus** -ruudussa tai **tulokset** -ruutu ja valitse sitten **toiminto** valikkorivillä. 

Jos valitset alisolmun **laajuus** -ruudussa ja hiiren kakkospainikkeella tai valitse **toiminto** -valikossa, saat seuraavan valikon esimerkiksi näkyy.
 
![StorSimple tilannevedoksen hallinta toimintovalikko](./media/storsimple-use-snapshot-manager/HCS_SSM_Action_menu.png)

**Toiminnot** -ruudussa (konsolin oikealla) on sama luettelo toiminnoista **toiminto** -valikosta. Lisäksi **toimet** -ruutu sisältää **Näytä** -valikossa asetukset, jotka, joiden avulla voit luoda mukautetun näkymän **tulosruudussa** .

![Näytä Toiminnot-ruudusta avattu](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

#### <a name="menu-description"></a>Valikon kuvaus

Seuraavassa taulukossa on luettelo aakkosjärjestyksessä StorSimple tilannevedoksen hallinnan toiminnot. 

- **Toiminto** -sarakkeessa toimintoja, joita voit suorittaa solmujen ja tulokset. 

- **Siirtyminen** -sarakkeen kerrotaan, miten voit näyttää haluamasi **toiminto** -valikosta, jonka avulla voit valita toiminnon. Jotkin toiminnot näkyvät **toiminnon** useita valikkoja. Nämä toiminnot, valitse **yksi siirtymistoimintoa** luettelomerkitty luettelo. 

- **Kuvaus** -sarakkeessa kuvataan, kuinka pystyt käyttämään kutakin **toimintovalikon** tai toiminnot ja kerrotaan kuvaus.

>[AZURE.NOTE] **Asiakirjatoimet** -ruutu ja **toiminto** valikot sisältävät lisäasetuksia, kuten **Näytä** **Omassa ikkunassa**, **päivittäminen**, **Vie luettelo**tai **Ohje**. Nämä asetukset ovat käytettävissä osana MMC ja eivät ole vain StorSimple tilannevedoksen hallinnassa. Taulukko sisältää kuvaukset seuraavista vaihtoehdoista.
 
| Toiminto  | Siirtyminen  | Kuvaus  |
|:--------|:------------|:-------------|
| Todennus | **Laitteiden** solmun ja **tulosruudussa** laitetta hiiren kakkospainikkeella. | Valitse **Tarkista** salasana, jonka määritit laitteen. |
| Kloonaa  | Laajenna **Varmuuskopiotiedoston**, laajenna **Cloud tilannevedoksia**, valitse päivätty varmuuskopiointi ja valitse sitten aseman **tulosruudussa** . | Valitse **Kloonaa** kopion cloud tilannevedoksen luominen ja se tallennetaan sijaintiin, jossa voit määrittää. |
| Laitteen määrittäminen | Napsauta hiiren kakkospainikkeella **laitteiden** solmun. | Valitse **Määritä laite** määrittämään yhteen laitteeseen tai muodostaa yhteyden Windowsin isännän useilla eri laitteilla. |
| Varmuuskopiointi-käytännön luominen | Tee jompikumpi seuraavista:<ul><li>Napsauta hiiren kakkospainikkeella **varmuuskopion käytännöt**.</li><li>Valitsemalla Laajenna **Aseman ryhmät**ja napsauta hiiren kakkospainikkeella aseman ryhmän.</li><li>Valitse Laajenna **Varmuuskopioinnin luettelo**ja napsauta hiiren kakkospainikkeella aseman ryhmän.</li></ul> | Valitse **Luo varmuuskopio käytännön** määrittäminen ajoitetun varmuuskopioinnin aseman ryhmän. |
| Avauksen ja vaihdon ryhmän luominen | Tee jompikumpi seuraavista:<ul><li>Valitse **asemat** -solmu ja napsauta hiiren kakkospainikkeella aseman **tulosruudussa** .</li><li>Napsauta hiiren kakkospainikkeella **Aseman ryhmät** -solmun.</li></ul> | Valitse **Luo aseman ryhmä** tietomääristä liittäminen aseman ryhmän. |
| Poista | Valitse solmu tai tulos (tämä kohde näkyy monta **toimintoa** valikoista ja **toimintojen** ruudut.) | Valitse poistettava solmu tai tulos, jossa valitsit **poistaminen** . Kun vahvistus-valintaikkuna tulee näyttöön, Vahvista tai Peruuta poisto. |
| Tiedot | Napsauta **laitteet** -solmu ja napsauta hiiren kakkospainikkeella laitteen **tulosruudussa** . | Valitse määritys tietojasi laitteen **tiedot** . |
| Muokkaa | Valitse **Varmuuskopiointi käytännöt**ja napsauta hiiren kakkospainikkeella käytännön **tulosruudussa** . | Valitse muutettava aseman ryhmän varmuuskopioinnin aikataulu **Muokkaa** . |
| Luettelon vieminen | Valitse haluamasi solmu tai tulos (kohdetta tulee näkyviin kaikkien **toiminnon** valikkojen ja **toimintojen** ruudut). | Valitse **Vie luettelo** tallentamaan luettelon CSV (CSV)-tiedosto. Voit tuoda tämän tiedoston laskentataulukkosovellusta analyysia varten. |
| Apua | Valitse tulos tai solmu. (Tämä kohde näkyy kaikkien **toiminnon** valikot ja **toimintojen** ruudut.) | Valitse **Ohje** -online-ohjeen avaaminen erillisessä selainikkunassa. |
| Tässä uudessa ikkunassa | Valitse haluamasi solmu tai tulos (kohdetta tulee näkyviin kaikkien **toiminnon** valikkojen ja **toimintojen** ruudut). | Valitse **Tässä uudessa ikkunassa** Avaa uuden StorSimple tilannevedoksen hallinta-ikkunassa.|
| Päivittäminen | Valitse haluamasi solmu tai tulos (kohdetta tulee näkyviin kaikkien **toiminnon** valikkojen ja **toimintojen** ruudut). | Valitse **Päivitä** päivittämään näytössä StorSimple tilannevedoksen hallinta-ikkunassa. |
| Päivitä laite | **Laitteiden** solmun ja **tulosruudussa** laitetta hiiren kakkospainikkeella. | Valitse **Päivitä laitteen** synkronointi tietyn yhdistetyn laitteen StorSimple tilannevedoksen hallinta. |
| Päivitä laitteet | Napsauta hiiren kakkospainikkeella **laitteiden** solmun. | Valitse **Päivitä laitteiden** synkronointi liitettyjen laitteiden välillä luettelon StorSimple tilannevedoksen hallinta. |
| Asemat uudelleen | Napsauta hiiren kakkospainikkeella **asemat** -solmu. | Valitse, voit päivittää luettelon asemista, joka näkyy **tulosruudussa** **asemat uudelleen** . |
| Palauttaminen | Laajenna **Varmuuskopiotiedoston**, laajenna äänenvoimakkuutta-ryhmä, laajenna **Paikallisen tilannevedoksia** tai **Cloud tilannevedoksia**ja napsauta varmuuskopiota hiiren kakkospainikkeella. | Valitse **palauttaminen** Korvaa nykyiset aseman ryhmätiedot valittu varmuuskopio tiedoilla. |
| Ota varmuuskopiointi | Tee jompikumpi seuraavista:<ul><li>Laajenna **Ryhmät äänenvoimakkuus**ja napsauta aseman ryhmän.</li><li>Laajenna **Varmuuskopioinnin luettelo**ja napsauta aseman ryhmän.</li></ul> | Valitse **Tehdä varmuuskopion** varmuuskopiointityön heti. |
| Näytä tai piilota tuonti | Napsauta hiiren kakkospainikkeella alisolmun **laajuus** -ruudussa (esimerkit **StorSimple tilannevedoksen hallinta** -solmu). | Valitse **Näytä tai piilota tuonnin** voit näyttää tai piilottaa aseman ryhmät ja liittyvän varmuuskopiot, joka on tuotu StorSimple hallinta-palvelun koontinäytössä. |

### <a name="view-menu"></a>Näytä-valikko

**Näytä** -valikon avulla voit luoda mukautetun näkymän **tulokset** -ruudun sisältöä. **Näkymä** sisältää **Lisää tai poista sarakkeita** ja **Mukauta** asetukset.

#### <a name="menu-access"></a>Valikoiden käyttö

Voit käyttää **Näytä valikkorivi tai **Asiakirjatoimet** -ruutu** .

![Näytä StorSimple tilannevedoksen hallinta](./media/storsimple-use-snapshot-manager/HCS_SSM_View_menu.png) 

#### <a name="menu-description"></a>Valikon kuvaus

Seuraavassa taulukossa on kuvattu näkyviä kohteita **Näytä** -valikon.

| Valikkovaihtoehto  | Kuvaus |
|:-----------|:-------------|
| Lisää tai Poista sarakkeet | Valitse Lisää tai poista sarakkeita **tulosruudussa** **Lisää tai Poista sarakkeet** . |
| Mukauttaminen | Valitse **Mukauta** voit näyttää tai piilottaa StorSimple tilannevedoksen hallinta console-ikkunan kohteita. |

### <a name="favorites-menu"></a>Suosikit-valikko

**Suosikit** -valikon avulla voit lisätä, poistaa ja järjestää sivun näkymät sekä tehtävät, joita käytät usein. 

#### <a name="menu-access"></a>Valikoiden käyttö

Voit käyttää **Suosikit valikkorivillä** .

![StorSimple tilannevedoksen hallinta Suosikit-valikko](./media/storsimple-use-snapshot-manager/HCS_SSM_FavoritesMenu.png)

#### <a name="menu-description"></a>Valikon kuvaus

Seuraavassa taulukossa on kuvattu **Suosikit** -valikossa näkyviä kohteita.

| Valikkovaihtoehto |  Kuvaus |
|:----------|:-------------|
| Lisää suosikkeihin | Valitse **Lisää suosikkeihin** nykyisen näkymän lisääminen suosikkiluetteloon. |
| Järjestä Suosikit | Valitse **Suosikit-kansion järjestäminen** Suosikit-kansion sisällön järjestämiseen. |

### <a name="window-menu"></a>Ikkuna-valikossa

**Ikkuna** -valikossa avulla voit lisätä ja järjestää StorSimple tilannevedoksen hallinnan konsolin windows.

#### <a name="menu-access"></a>Valikoiden käyttö

Voit käyttää valikkorivillä **ikkuna** -valikossa.

![StorSimple tilannevedoksen hallinta-ikkuna](./media/storsimple-use-snapshot-manager/HCS_SSM_WindowMenu.png)

Numeroidun luettelon, valikon alareunassa näkyy ikkunoita, jotka ovat tällä hetkellä avata. Napsauttamalla mitä tahansa tuoda ikkunan edustaa luettelo. 

#### <a name="menu-description"></a>Valikon kuvaus

Seuraavassa taulukossa on kuvattu näkyviä kohteita ikkuna-valikosta.

| Valikkovaihtoehto  | Kuvaus |
|:-----------|:-------------|
| Uudessa ikkunassa | Valitse **Uudessa ikkunassa** Avaa uuden konsoli-ikkunan (lisäksi aiemmin ikkuna). |
| Johdannaispoisto   | Valitse Avaa konsoli-ikkunoiden näyttäminen CSS-tyylin **Limittäin** . |
| Vierekkäin | Valitse **Ruutu vaakasuunnassa** , Avaa konsoli-ikkunoiden näyttäminen vierekkäin (tai ruudukon)-muodossa. |
| Järjestä kuvakkeet | Jos sinulla on useita konsolin windows Avaa ja ajoittaisia työpöydän kohdalle, pienennä ne ja valitse sitten **Järjestä kuvakkeet** Järjestä vaakasuuntainen rivi näytön alareunassa. |

### <a name="help-menu"></a>Ohje-valikko

**Ohje** -valikosta avulla voit tarkastella käytettävissä online-ohje StorSimple tilannevedoksen johtajan ja MMC: HEN. Voit myös tarkastella tietoja MMC ja StorSimple tilannevedoksen Manager ohjelmisto-versioista, jotka on jo asennettu järjestelmän tiedot. 

Voit käyttää valikkoriviltä **Ohje** -valikossa. Voit myös käyttää StorSimple tilannevedoksen hallinta ohjeaiheet poistaminen **Asiakirjatoimet** -ruutu.

![StorSimple tilannevedoksen hallinnan Ohje-valikko](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpMenu.png)

#### <a name="menu-description"></a>Valikon kuvaus

Seuraavassa taulukossa on kuvattu Ohje-valikossa näkyviä kohteita.

| Valikkovaihtoehto  | Kuvaus  |
|:-----------|:-------------|
| Ohjeita StorSimple tilannevedoksen hallinta | Valitse **StorSimple tilannevedoksen hallinta auttaa** StorSimple tilannevedoksen hallinnan Ohje avaaminen erillisessä ikkunassa. |
| Ohjeaiheet |Valitse **ohjeaiheet** MMC online-ohjeen avaaminen erillisessä ikkunassa. |
| TechCenter-sivusto | Valitse **TechCenter-sivusto** Microsoft TechNet Tech Center kotisivun avaaminen erillisessä ikkunassa. |
| Tietoja Microsoft Management Console | Valitse **Tietoja Microsoft Management Console** -järjestelmään on asennettu Microsoft Management Console-version. |
| Lisätietoja StorSimple tilannevedoksen hallinnasta | Valitse **StorSimple tilannevedoksen Managerin** version laajennus on asennettu järjestelmään. |

## <a name="tool-bar"></a>Työkalurivin

Työkalurivin alapuolella valikkoriviltä sisältää siirtymis- ja tehtävä kuvakkeet. Kunkin kuvakkeen on pikakuvakkeen tiettyyn tehtävään.

### <a name="icon-descriptions"></a>Tilakuvakkeiden kuvaukset

Seuraavassa taulukossa kuvataan kuvakkeet, jotka näkyvät-työkalurivin. 

| Kuvake  | Kuvaus  |
|:------|:-------------| 
| ![Vasen nuoli](./media/storsimple-use-snapshot-manager/HCS_SSM_LeftArrow.png) | Napsauta vasen nuoli-kuvaketta, voit palata edelliselle sivulle. |
| ![Oikea nuoli](./media/storsimple-use-snapshot-manager/HCS_SSM_RightArrow.png) | Siirry seuraavalle sivulle oikealle osoittavaa nuolta (Jos nuoli on harmaa-toiminto ei ole käytettävissä). |
| ![Ylös-kuvake](./media/storsimple-use-snapshot-manager/HCS_SSM_Up.png) | Siirry ylöspäin yhden tason verran konsolipuussa ( **laajuus** -ruutu) kuvaketta ajan. |
| ![Näytä tai piilota-konsolipuussa](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowConsoleTree.png) | Napsauttamalla Näytä tai piilota **laajuus** -ruudussa Näytä tai piilota konsolin puun-kuvaketta. |
| ![Luettelon vieminen](./media/storsimple-use-snapshot-manager/HCS_SSM_ExportListIcon.png) | Valitse luettelon vieminen CSV-tiedostoon, voit määrittää Vie luettelo-kuvake. |
| ![Ohje-kuvake](./media/storsimple-use-snapshot-manager/HCS_SSM_HelpIcon.png)  |Valitse Avaa online MMC ohjeaiheen Ohje-kuvakkeen. |
| ![Näytä tai piilota-toiminnot](./media/storsimple-use-snapshot-manager/HCS_SSM_ShowAction.png) | Valitse Näytä tai piilota **Toiminnot** ruudun-kuvaketta, voit näyttää tai piilottaa **Asiakirjatoimet** -ruutu. 
 
## <a name="scope-pane"></a>Laajuus-ruutu

**Laajuus** -ruutu on StorSimple tilannevedoksen hallinta-Käyttöliittymän vasemmanpuoleisessa ruudussa. Se sisältää console (tai solmu) puun ja on ensisijainen siirtyminen järjestelmä StorSimple tilannevedoksen hallinta. 
 
### <a name="scope-pane-structure"></a>Laajuus-ruudusta rakenne

**Laajuus** -ruutu sisältää sarjan järjestetty puurakenteeksi, valittavana olevia objekteja (solmujen). 

![Laajuus-ruutu](./media/storsimple-use-snapshot-manager/HCS_SSM_Scope_pane.png) 

- Laajenna tai kutista solmu, solmu nimen vierestä nuolta.

- Voit tarkastella tila tai solmu sisällön, valitsemalla Solmunimi. Tiedot näkyvät **tulosruudussa** . 

**Laajuus** -ruutu sisältää seuraavat solmut: 

- [Laitteiden solmun](#devices-node) 
- [Asemat-solmu](#volumes-node) 
- [Avauksen ja vaihdon ryhmät solmu](#volume-groups-node) 
- [Voit varmuuskopioida käytännöt solmu](#backup-policies-node) 
- [Voit varmuuskopioida luettelon solmu](#backup-catalog-node) 
- [Työt-solmu](#jobs-node) 

### <a name="scope-pane-tasks"></a>Laajuus-ruudussa tehtävät

Voit suorittaa toiminnon tietyn solmun **laajuus** -ruudun avulla. Valitse tehtävän, tee jompikumpi seuraavista:

- Solmun hiiren kakkospainikkeella ja valitse näkyviin tulevasta valikosta tehtävän.

- Valitse solmu ja valitse sitten valikossa **toiminto** . Valitse näyttöön tulevasta valikosta tehtävän.

- Valitse solmu ja valitse sitten **Toiminnot** -ruudussa toiminnon.

Kun valitset solmu ja nähdä tehtäväluettelon jollakin seuraavista tavoista avulla, vain toimintoja, jotka voidaan suorittaa solmun ovat näkyvissä.

### <a name="devices-node"></a>Laitteiden solmun

**Laitteet** -solmu edustaa StorSimple laitteet ja StorSimple virtual liitettyjen laitteiden StorSimple tilannevedoksen hallinta. Valitse solmu, jos haluat muodostaa yhteyden ja määrittää laitteen ja tuoda liittyvät asemat, asemat ryhmiä ja aiemmin varmuuskopiot. Useiden laitteiden voidaan yhdistää yhden isäntään.

- Laajenna solmu, valitse **laitteet**-kohdan vieressä olevaa nuolikuvaketta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **laitteiden** solmun tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Saat määritetty luettelo valitsemalla **laajuus** -ruudussa **laitteet** . **Tulokset** -ruudussa näkyy luettelon laitteista, sekä kunkin laitteen tiedot.

### <a name="volumes-node"></a>Asemat-solmu

**Asemat** -solmu edustaa asemat, jotka vastaavat, liitetty isäntä, mukaan lukien havaitsi iSCSI ja ne havaitsi laitteen läpi asemat. Solmun avulla voit tarkastella käytettävissä tietomääristä luettelo ja määrittää yksittäisen tietomääristä aseman ryhmiin.

- Laajenna solmu, valitse **tietomääristä**vieressä olevaa nuolikuvaketta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **asemat** -solmu tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Jos haluat nähdä luettelon asemista, valitse **tietomääristä** **laajuus** -ruudussa. Asemat jokaisen aseman tietoja ja luettelo tulee näkyviin **tulosruudussa** .

### <a name="volume-groups-node"></a>Avauksen ja vaihdon ryhmät solmu

Avauksen ja vaihdon ryhmät ovat tunnetaan myös nimellä yhdenmukaisuuden ryhmät. Avauksen ja vaihdon jokainen ryhmä on sovelluksen liittyvät asemat resurssivarantoon, jonka avulla voidaan varmistaa sovelluksen varmuuskopioinnin aikana. Avulla **Aseman ryhmät** -solmun määrittäminen ryhmiin ja ottaa vuorovaikutteinen varmuuskopioita tai luoda varmuuskopion aikatauluja. 

- Voit laajentaa solmu napsauttamalla **Äänenvoimakkuuden ryhmät**-kohdan vieressä olevaa nuolikuvaketta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **Aseman ryhmät** -solmun tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Äänenvoimakkuuden ryhmien luettelo näkyviin napsauttamalla **Äänenvoimakkuuden ryhmät** **laajuus** -ruudussa. Äänenvoimakkuuden ryhmät-asema kunkin ryhmän tietoja ja luettelo tulee näkyviin **tulosruudussa** .

### <a name="backup-policies-node"></a>Voit varmuuskopioida käytännöt solmu

Varmuuskopion käytännöt on paikallinen työaikatauluja ja cloud tilannevedoksia. Voit määrittää, kuinka usein varmuuskopion luodaan ja kuinka kauan varmuuskopion **Varmuuskopion käytännöt** -solmun käyttöä on säilytettävä. 

- Laajenna solmu, valitse **Varmuuskopiointi käytännöt**vieressä olevaa nuolikuvaketta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **Varmuuskopiointi käytännöt** -solmun tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Saat varmuuskopion käytännöt luettelo valitsemalla **Varmuuskopion käytännöt** **laajuus** -ruudussa. Varmuuskopion käytäntöjä kunkin käytännön tietoja ja luettelo tulee näkyviin **tulosruudussa** .

>[AZURE.NOTE] Voit säilyttää enintään 64 varmuuskopiot.


### <a name="backup-catalog-node"></a>Voit varmuuskopioida luettelon solmu

Solmun **Varmuuskopioinnin luettelo** sisältää Azure StorSimple tietomääristä paikalla ja sähköntuotannosta varmuuskopiot. Tämä solmu on järjestetty aseman ryhmän ja aseman kunkin ryhmän säilön sisältää erillisen rakenteita paikallisen tilannevedoksia ( **Paikallisen tilannevedoksen**s-solmu) ja cloud tilannevedoksia ( **Cloud tilannevedoksia** solmu). Laajennettuna, äänenvoimakkuuden kunkin ryhmän säilön on lueteltu kaikki, jotka on otettu vuorovaikutteisesti tai määritetty käytäntö onnistuu varmuuskopiot.

- Laajenna solmu, valitse vieressä **Varmuuskopioinnin luettelo**napsauttamalla nuolta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **Varmuuskopioinnin luettelo** -solmu tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Saat varmuuskopion tilannevedoksia luettelo valitsemalla **Varmuuskopiotiedoston** **laajuus** -ruudussa. Tallentaa kuvan jokaisen tilannevedoksen tietoja ja luettelo tulee näkyviin **tulosruudussa** .

### <a name="local-snapshots-node"></a>Paikallinen tilannevedoksia solmu

**Paikallisen tilannevedoksia** -solmu näyttää paikallisten tilannevedoksia määritetyn aseman ryhmän. Solmun sijaitsee **Varmuuskopioinnin luettelo** solmun **laajuus** -ruudussa. Paikallinen tilannevedoksia ajankohta aseman tiedot, jotka on tallennettu Azure StorSimple laitteeseen kopioita. Yleensä Tämäntyyppinen varmuuskopiointi voi luoda ja palauttaa nopeasti. Voit käyttää paikallisen tilannevedoksen paikallisen varmuuskopion tapaan.

- Laajenna solmu, valitse **Paikallisen tilannevedoksia**vieressä olevaa nuolikuvaketta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **Paikallisen tilannevedoksia** solmu tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Saat paikallisen tilannevedoksia luettelo valitsemalla **Paikallisen tilannevedoksia** **laajuus** -ruudussa. Tallentaa kuvan jokaisen tilannevedoksen tietoja ja luettelo tulee näkyviin **tulosruudussa** .

### <a name="cloud-snapshots-node"></a>Cloud tilannevedoksia solmu

**Cloud tilannevedoksia** -solmu näyttää cloud tilannevedoksia määritetyn aseman ryhmän. Solmun sijaitsee **Varmuuskopioinnin luettelo** solmun **laajuus** -ruudussa. Cloud tilannevedoksia ajankohta aseman tiedot, jotka on tallennettu pilvipalveluun kopioita. Cloud tilannevedoksen vastaa tilannevedoksen replikoida eri, sähköntuotannosta tallennustilan järjestelmään. Cloud tilannevedoksia ovat erityisen hyödyllisiä tietojen palauttaminen tilanteissa.

- Laajenna solmu, valitse **Cloud tilannevedoksia**vieressä olevaa nuolikuvaketta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **Cloud tilannevedoksia** solmu tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Cloud tilannevedoksia luettelo näkyviin napsauttamalla **Cloud tilannevedoksia** **laajuus** -ruudussa. Tallentaa kuvan jokaisen tilannevedoksen tietoja ja luettelo tulee näkyviin **tulosruudussa** .

### <a name="jobs-node"></a>Työt-solmu

**Työt** -solmu on suunniteltu, käynnissä ja viimeksi valmiit varmuuskopion työt tietoja. 

- Laajenna solmu, valitse **työt**-kohdan vieressä olevaa nuolikuvaketta.

- Käytettävissä olevien toimintojen valikon näkyviin napsauttamalla hiiren kakkospainikkeella **työt** -solmu tai mitä tahansa solmut, jotka näkyvät laajennettu-näkymässä hiiren kakkospainikkeella.

- Löydät luettelon ajoitetuissa, laajenna **Projektit** -solmu ja valitse sitten **ajoitettu**. **Tulokset** -ruudussa näkyy aiemmin määritetyn työt ja kunkin projektin tietoja. 

- Luettelo äskettäin valmiit työt, laajenna **Projektit** -solmu ja valitse sitten **viimeisen 24 tuntia**. Työt, jotka on tehty viimeisen 24 tunnin luettelo tulee näkyviin **tulosruudussa** . **Tulokset** -ruutu sisältää myös kunkin valmiin projektin tiedot.

- Löydät luettelon työt, jotka ovat parhaillaan käynnissä, laajenna **Projektit** -solmu ja valitse sitten **käynnissä**. Käynnissä olevat työt sekä kunkin projektin tietoja näkyy **tulosruudussa** .

## <a name="results-pane"></a>Tulokset-ruutu

**Tulokset** -ruutu on keskiruudun StorSimple tilannevedoksen hallinta käyttöliittymässä. Se sisältää luettelot ja **laajuus** -ruudussa valittu solmu yksityiskohtaiset tilatiedot.

### <a name="example"></a>Esimerkki

Seuraavassa esimerkissä näkyviin napsauttamalla **Äänenvoimakkuuden ryhmät** -solmu **laajuus** -ruudussa. **Tulokset** -ruutu näyttää luettelon äänenvoimakkuutta-ryhmissä, joissa on kunkin ryhmän tietoja.

![Tulokset-ruutu](./media/storsimple-use-snapshot-manager/HCS_SSM_Results_pane.png) 

Voit määrittää **tulokset** -ruudussa näkyvät tiedot: solmu **laajuus** -ruudussa hiiren kakkospainikkeella, valitse **Näytä**ja valitse sitten **Lisää tai Poista sarakkeet**.

## <a name="actions-pane"></a>Asiakirjatoimet-ruutu

**Asiakirjatoimet** -ruutu on oikeanpuoleisen ruudun StorSimple tilannevedoksen hallinta käyttöliittymässä. Se sisältää toiminnoista, joita voit suorittaa solmu, näkymän tai **laajuus** -ruudussa tai **tulosruudussa** valitseminen tietojen valikko. **Toiminnot** -ruudusta komennot kuin **toiminto** valikot, jotka ovat käytettävissä kohteiden **laajuus** -ruutu ja **tulokset** -ruutu. Katso kutakin kuvauksen, **toiminto** -valikosta osa-taulukko.

### <a name="examples"></a>Esimerkkejä

Seuraavassa esimerkissä **laajuus** -ruudussa, laajenna **Projektit** -solmu ja valitse **ajoitettu**. **Toiminnot** -ruudussa näkyvät käytettävissä olevat toiminnot **ajoitettu** solmun.

![Esimerkki: toiminnot ajoitetuissa](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane.png) 

Saat näkyviin lisää vaihtoehtoja, valitse **laajuus** -ruudussa Laajenna **Projektit** -solmu ja valitse **ajoitettu**ajoitetun työn **tulosruudussa** . **Asiakirjatoimet** -ruutu näyttää ajoitetun työn käytettävissä olevat toiminnot, kuten seuraavassa esimerkissä.

![Esimerkki: toiminnot työn toimintoja](./media/storsimple-use-snapshot-manager/HCS_SSM_ActionsPane_Results.png)

## <a name="keyboard-navigation-and-shortcuts"></a>Pikanäppäimet ja pikanäppäimiä

StorSimple tilannevedoksen hallinnan avulla Windows-käyttöjärjestelmän ja Microsoft Management Console (MMC) helppokäyttötoiminnot. Se myös kaikki näppäimistön siirtymistoiminnot ja pikakuvakkeita, jotka koskevat käyttämään StorSimple tilannevedoksen Manager ohjeiden mukaisesti seuraavissa osissa.
 
- [Näppäimistön pikanäppäimet](#keyboard-navigation-keys) 
- [Valikkorivin pikanäppäimet](#menu-bar-shortcut-keys) 
- [Laajuus pikanäppäimet](#scope-pane-shortcut-keys) 

### <a name="keyboard-navigation-keys"></a>Näppäimistön pikanäppäimet

Seuraavassa taulukossa on kuvattu näppäimet, joiden avulla voit siirtyä StorSimple tilannevedoksen hallinnan-käyttöliittymä. 

| Siirtyminen avain  | Toiminto  |
|:----------------|:--------| 
| ALANUOLINÄPPÄIN | Siirry pystysuunnassa seuraavaan kohteeseen-valikko tai ruudun käyttämällä ALANUOLINÄPPÄINTÄ. |
| Kirjoita | Paina Enter-näppäintä suorittaa toiminnon ja siirry seuraavaan vaiheeseen. Voit esimerkiksi painaa Enter, valitse **Seuraava**, **OK**tai **Luo**ja siirry seuraavaan vaiheeseen ohjatun toiminnon.|
| ESC | Paina Esc-näppäintä voit sulkea valikon tai Peruuta ja sulje sivu.|
| F1 | Paina F1-näppäintä, voit tarkastella ohjeaihe parhaillaan aktiivinen ikkuna.|
| F5 | Päivitä solmu F5-näppäintä. |
| F6 | Siirrä **laajuus** -ruudusta **tulosruudussa** F6-näppäintä.|
| F10 | Siirry valikkorivin F10-näppäimellä. |
| VASEN NUOLINÄPPÄIN | VASEN NUOLI-näppäinyhdistelmää avulla voit siirtää vaakasuunnassa palkin valikkovaihtoehdosta edellisen vaihtoehdon. Kun siirrät edelliseen kohteeseen valikkorivillä, edellisen kohteen toiminto (tai kontekstin)-valikko tulee näkyviin. |
| Oikeaa nuolinäppäintä | Oikeaa nuolinäppäintä avulla voit siirtää vaakasuunnassa yhden palkin vaihtoehto toiseen. Kun siirrät seuraavaan kohteeseen valikkorivillä, uuden kohteen toiminto (tai kontekstin)-valikko tulee näkyviin.
| SARKAIN-näppäintä | Siirtää seuraavaan ruutuun konsolissa tai valinnan tai tekstin nimiruutuun sivulle sarkaimella. |
| YLÄNUOLINÄPPÄIN | Ajan nuolinäppäimen avulla voit siirtää pystysuunnassa edelliseen kohteeseen-valikossa tai ruutu. |

### <a name="menu-bar-shortcut-keys"></a>Valikkorivin pikanäppäimet

Seuraavassa taulukossa on kuvattu valikkoriviltä pikanäppäimiä. Kun painat pikanäppäimet ja valikko avautuu, voit käyttää valikon pikanäppäimet (valikon alleviivattua näppäimet). Lisätietoja valikkoriviltä Siirry [valikkorivillä](#menu-bar).

| Pikakuvake | Tulos                    | Valikon pikanäppäin | Tulos          |
|:---------|:--------------------------|:------------------|:----------------|
| ALT + F    | Avaa **Tiedosto** -valikosta.  | N | Avaa uusi konsoli esiintymä.   |
|          |                           | O | Avaa **Valvontatyökalut** -sivun. |
|          |                           | S | Tallentaa StorSimple tilannevedoksen hallinta-konsolin.|
|          |                           | A | Avaa **Tallenna nimellä** -sivun. |
|          |                           | M | **Lisää tai poista laajennus** -sivu avautuu.|
|          |                           | P | Avaa **asetukset** -sivu. |
|          |                           | H | Avaa online-ohje.|
| ALT + A    | Avaa **toiminto** -valikosta.| Voin | Ottaa käyttöön ja poistaminen käytöstä Tuo haluamasi näyttötapa.|
|          |                           | W | Avaa uusi StorSimple tilannevedoksen hallinta-konsolin.|
|          |                           | F | Päivittää StorSimple tilannevedoksen hallinta-konsolin.|
|          |                           | L | **Vie luettelo** -sivu avautuu. 
|          |                           | H | Avaa online-ohje.|
| ALT + V    | Avaa **Näytä** -valikon.  | A | Avaa **Lisää tai poista sarakkeita** -sivun. |
|          |                           | U | **Mukauta näkymää** -sivu avautuu. |
| ALT + O    | Avaa **Suosikit** . | A | **Lisää suosikkeihin** -sivu avautuu. |
|          |                           | O | Avaa **Järjestä Suosikit** -sivun.|
| ALT + W    | Näyttöön avautuu **ikkuna** -valikossa.| N | Avaa toisen StorSimple tilannevedoksen hallinta-ikkunassa.|
|          |                           | C | Näyttää Avaa konsolin ikkunoiden CSS-tyylin.|
|          |                           | T | Näyttää Avaa konsolin ikkunoiden ruudukko. |
|          |                           | Voin | Järjestää kuvakkeet vaakasuuntainen rivi näytön alareunassa.|
| ALT + H    | Avaa **Ohje** -valikosta.  | H | Avaa online-ohje.|
|          |                           | T | Avaa Microsoft TechNet Tech Center-web-sivun.|
|          |                           | A | **Tietoja Microsoft Management Console** -sivu avautuu. |
 
### <a name="scope-pane-shortcut-keys"></a>Laajuus pikanäppäimet

Seuraavassa taulukossa esitetään pikakuvaketta näppäinyhdistelmät kunkin solmun **laajuus** -ruudussa. 

- [Laitteiden solmun pikanäppäimet](#devices-node-shortcut-keys)
- [Asemat solmu pikanäppäimet](#volumes-node-shortcut-keys)
- [Avauksen ja vaihdon ryhmät solmu pikanäppäimet](#volume-groups-node-shortcut-keys)
- [Voit varmuuskopioida käytännöt solmu pikanäppäimet](#backup-policies-node-shortcut-keys)
- [Voit varmuuskopioida luettelon solmu pikanäppäimet](#backup-catalog-node-shortcut-keys)
- [Töiden solmu pikanäppäimet](#jobs-node-shortcut-keys)

#### <a name="devices-node-shortcut-keys"></a>Laitteiden solmun pikanäppäimet

| Pikavalikko | Tulos                               |
|:--------------|:-------------------------------------|
| C             | **Määritä laite** -sivu avautuu. |
| D             | Päivittää laitteet ja laitteen tiedot.|
| V             | Avaa **Näytä** -valikon. |
| W             | Avaa uusi StorSimple tilannevedoksen hallinta-konsolin kohdistettu **tiedot** -solmu. |
| F             | Päivittää StorSimple tilannevedoksen hallinta-konsolin. |
| L             | **Vie luettelo** -sivu avautuu. 
| H             | Avaa online-ohje.|
 

#### <a name="volumes-node-shortcut-keys"></a>Asemat solmu pikanäppäimet

| Pikavalikko   | Tulos                              |
|:----------------|:------------------------------------|
| V               | Päivittää asemat.        |
| V (paina kaksi kertaa) | Avaa **Näytä** -valikon.            |
| W               | Avaa uusi StorSimple tilannevedoksen hallinta-konsolin kohdistettu **asemat** -solmu.|
| F               | Päivittää StorSimple tilannevedoksen hallinta-konsolin.|
| L               | **Vie luettelo** -sivu avautuu. 
| H               | Avaa online-ohje.|
 
#### <a name="volume-groups-node-shortcut-keys"></a>Avauksen ja vaihdon ryhmät solmu pikanäppäimet

| Pikavalikko   | Tulos                              |
|:----------------|:------------------------------------|
| G               | Avaa **Luo äänenvoimakkuutta-ryhmän** sivu. |
| V               | Avaa **Näytä** -valikon. |
| W               | Avaa uusi StorSimple tilannevedoksen hallinta-konsolin kohdistettu **Aseman ryhmät** -solmun.|
| F               | Päivittää StorSimple tilannevedoksen hallinta-konsolin. |
| L               | **Vie luettelo** -sivu avautuu. |
| H               | Avaa online-ohje.|

#### <a name="backup-policies-node-shortcut-keys"></a>Voit varmuuskopioida käytännöt solmu pikanäppäimet

| Pikavalikko   | Tulos                              |
|:----------------|:------------------------------------|
| B               | **Luo käytännön** -sivu avautuu. |
| V               | Avaa **Näytä** -valikon.            |
| W               | Avaa uusi StorSimple tilannevedoksen hallinta-konsolin kohdistettu **Aseman ryhmät** -solmun.|
| F               | Päivittää StorSimple tilannevedoksen hallinta-konsolin.|
| L               | **Vie luettelo **-sivu avautuu. 
| H               | Avaa online-ohje.|
 
#### <a name="backup-catalog-node-shortcut-keys"></a>Voit varmuuskopioida luettelon solmu pikanäppäimet

| Pikavalikko   | Tulos                              |
|:----------------|:------------------------------------|
| W               | Avaa uusi StorSimple tilannevedoksen hallinta-konsolin kohdistettu **Aseman ryhmät** -solmun. |
| F               | Päivittää StorSimple tilannevedoksen hallinta-konsolin. |
| H               | Avaa online-ohje.|
 
#### <a name="jobs-node-shortcut-keys"></a>Töiden solmu pikanäppäimet

| Pikavalikko   | Tulos                              |
|:----------------|:------------------------------------|
| V               | Avaa **Näytä** -valikon.            |
| W               | Avaa uusi StorSimple tilannevedoksen hallinta-konsolin kohdistettu **työt** -solmu.|
| F               | Päivittää StorSimple tilannevedoksen hallinta-konsolin.|
| L               | **Vie luettelo** -sivu avautuu.     |
| H               | Avaa online-ohje                   |
 
## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta ammattimainen StorSimple ratkaisu](storsimple-snapshot-manager-admin.md).
- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinnan yhteyden ja laitteiden hallinta](storsimple-snapshot-manager-manage-devices.md).
