<properties 
   pageTitle="Mikä on StorSimple tilannevedoksen hallinta? | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple tilannevedoksen johtajan, sen arkkitehtuuri ja niiden ominaisuuksista."
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

# <a name="what-is-storsimple-snapshot-manager"></a>Mikä on StorSimple tilannevedoksen hallinta?

## <a name="overview"></a>Yleiskatsaus

StorSimple tilannevedoksen hallinta on Microsoft Management Console (MMC)-laajennus, joka helpottaa tietojen suojaaminen ja varmuuskopion hallinta Microsoft Azure StorSimple ympäristössä. StorSimple tilannevedoksen hallinnan avulla voit hallita tietokeskuksen ja pilvessä Microsoft Azure StorSimple tiedot yhteen integroitu tallennustilaa, ratkaisuksi näin helpottaminen backup prosessit ja pienentää kustannuksia.

Tämä yleiskatsaus esitellään StorSimple tilannevedoksen hallinta, kuvataan sen ominaisuudet ja kerrotaan sen Microsoft Azure StorSimple rooli. 

Yleisiä tietoja koko Microsoft Azure StorSimple järjestelmän, mukaan lukien StorSimple laite, StorSimple hallintapalveluun, StorSimple tilannevedoksen hallinta ja StorSimple sovittimen SharePoint-artikkelissa [StorSimple 8000 sarja: hybrid cloud tallennustilan ratkaista](storsimple-overview.md). 
 
>[AZURE.NOTE] 
>
>- StorSimple tilannevedoksen hallinnan avulla et voi hallita Microsoft Azure StorSimple Virtual matriisien (tunnetaan myös nimellä StorSimple paikallisen virtual laitteet).
>
>- Jos aiot asentaa StorSimple Update 2 StorSimple laitteen, muista ladata uusimman version StorSimple tilannevedoksen hallinta ja asenna se **ennen kuin asennat StorSimple päivityksen 2**. Uusimman version StorSimple tilannevedoksen hallinta on yhteensopiva aiempien versioiden ja kaikkien julkaistun versioiden Microsoft Azure StorSimple toimii. Jos käytössäsi on aiemmasta versiosta StorSimple tilannevedoksen hallinta, sinun on päivittää sen (sinun ei tarvitse poistaa edellisen version ennen uuden version asentamista).

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple tilannevedoksen hallinnan tarkoitus ja arkkitehtuuri

StorSimple tilannevedoksen hallinta tarjoaa keskitetyn hallinnan konsolin, joiden avulla voit luoda yhtenäisiä paikallinen ajankohta varmuuskopiot ja cloud tiedot. Voit esimerkiksi konsolin:

- Määrittää, varmuuskopioida ja poistaa asemat.
- Määritä aseman ryhmät, jos haluat varmistaa, varmuuskopioinut tiedot ovat sovelluksen yhdenmukaisia.
- Varmuuskopion käytäntöjen hallinta niin, että tiedot varmuuskopioidaan ennalta aikataulun.
- Luo paikalliseen ja cloud tilannevedoksia, joita pilveen tallennettujen ja käyttää palauttaminen.

StorSimple tilannevedoksen hallinta hakee rekisteröity isännän VSS-palvelussa sovellusten luettelossa. Valitse sovelluksen johdonmukaisia varmuuskopioiden luomiseen se tarkistaa sovelluksen käytetyt määrät ja ehdottaa aseman ryhmien määrittäminen. StorSimple tilannevedoksen hallinta käyttää aseman ryhmiin varmuuskopioita, jotka ovat yhdenmukaisia sovelluksen luomiseen. (Sovelluksen yhdenmukaisuuden olemassa, kun kaikki siihen liittyvät tiedostot ja tietokantojen synkronoidaan ja ne kuvaavat sovelluksen tiettynä ajankohtana tosi tilan.) 

StorSimple tilannevedoksen hallinta varmuuskopioiden tukena vaiheittainen tilannevedoksia, joita kerää vain muutokset edellisen varmuuskopioinnin jälkeen. Tämän vuoksi varmuuskopiot edellyttävät vähemmän tallennustilaa ja voi luoda ja palauttaa nopeasti. StorSimple tilannevedoksen hallinta käyttämällä Windows aseman varjostus kopioi Service (VSS) varmistaa tilannevedoksia siepata sovelluksen yhtenäisen tietojen. (Saat lisätietoja Siirry Windowsin tilannevedospalvelu osan integrointi.) StorSimple tilannevedoksen hallinnan avulla voit luoda varmuuskopion aikatauluja tai kestää heti varmuuskopioiden tarpeen mukaan. Jos haluat palauttaa tietoja varmuuskopion, StorSimple tilannevedoksen hallinta-toiminnon avulla voit määrittää paikallisen luettelo tai cloud tilannevedoksia. Azure StorSimple palauttaa vain tiedot, jotka tarvitaan, kun sitä tarvita, joka estää viiveitä tietojen käytettävyys-palauttaminen toimintojen aikana).

![StorSimple tilannevedoksen hallinnan arkkitehtuuri](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple tilannevedoksen hallinnan arkkitehtuuri** 

## <a name="support-for-multiple-volume-types"></a>Usean aseman käyttämisen tuki

Voit käyttää StorSimple tilannevedoksen hallinta ja asemat seuraavanlaisia varmuuskopioida: 

- **Basic tietomääristä** – basic äänenvoimakkuus on basic levyn yhden osion. 

- **Tavalliset asemat** – yksinkertainen äänenvoimakkuus on dynaaminen asema, joka sisältää yhdellä dynaamisella levyllä olevasta levytilasta. Tavallinen asema koostuu yhden alueen levyllä tai useiden alueiden tietueet, jotka on linkitetty toisiinsa saman levyn. (Voit luoda yksinkertaisen tietomääristä vain dynaamisiksi.) Tavalliset asemat eivät ole vikasietoisia.

- **Dynaamiset asemat** – dynaaminen asema, joka on luotu dynaamisen levyn asema. Dynaamisiksi tietokannan avulla voit seurata tietoja, jotka sisältyvät asemat dynaamisiksi tietokoneessa. 

- **Dynaaminen asemat peilaukseen** – dynaaminen asemat peilaus on muodostettu RAID 1-arkkitehtuuri. Samanlaisten arvopisteiden kirjoitetaan RAID 1, kahden tai useamman levyn tuottavat peilattu määrittäminen. Lukukuittauksen voidaan käsitellä sitten pyydetyt tiedot sisältävän levyn mukaan.

- **Klusterin jaetut asemat** – klusterin jaetut asemat (CSVs), jossa useita solmujen automaattisesti klusterin voivat samanaikaisesti lukea tai kirjoittaa levylle. Yhden solmun automaattisesti toisen solmun voi ilmetä nopeasti tarvitsematta asema omistajuus tai käyttöönottaminen käytöstä ja poistamalla aseman muutoksen. 

>[AZURE.IMPORTANT] CSVs ja saman tilannevedoksen ei CSVs ole yhdistelmiä. Sekoittamalla CSVs ja -tilannevedoksen ei CSVs ei tueta. 
 
StorSimple tilannevedoksen hallinnan avulla voit palauttaa koko aseman ryhmät tai Kloonaa yksittäisiä asemat ja palauttaa yksittäisiä tiedostoja.

- [Asemat ja aseman ryhmät](#volumes-and-volume-groups) 
- [Varmuuskopion tyypit ja varmuuskopioinnin määrittäminen](#backup-types-and-backup-policies) 

Lisätietoja StorSimple tilannevedoksen hallinta ominaisuuksista ja niiden käyttämisestä on artikkelissa [StorSimple tilannevedoksen hallinnan käyttöliittymä](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Asemat ja aseman ryhmät

StorSimple tilannevedoksen hallinnan avulla voit luoda asemat ja määrittää niitä aseman ryhmiin. 

StorSimple tilannevedoksen hallinta käyttää aseman ryhmät varmuuskopioita, jotka ovat yhdenmukaisia sovelluksen luomiseen. Sovelluksen yhdenmukaisuuden olemassa, kun kaikki siihen liittyvät tiedostot ja tietokantojen synkronoidaan ja ne kuvaavat sovelluksen tiettynä ajankohtana tosi tila. Äänenvoimakkuuden ryhmät (jotka ovat niin sanottuja *yhdenmukaisuuden ryhmät*) perustan varmuuskopiointi ja palauttaminen työn.

Äänenvoimakkuuden ryhmät eivät ole sama kuin aseman säilöjen. Äänenvoimakkuuden säilön sisältää vähintään yhden asemat, joka jakaa pilven-tallennustilan tilin ja muita määritteitä, kuten salaus ja kaistanleveyden kulutus. Yksittäisen aseman säilön voi olla enintään 256 levinneet valmistellun StorSimple asemat. Saat lisätietoja vaihdon säilöjen Siirry [aseman säilöjen hallinta](storsimple-manage-volume-containers.md). Äänenvoimakkuuden ryhmät ovat muotokokoelmia, joiden asemat, jotka määrität varmuuskopion toimien helpottamiseksi. Jos valitset kaksi asemat, jotka kuuluvat eri asema säilöjen, sijoittaa ne yhteen äänenvoimakkuutta-ryhmässä ja luo varmuuskopio käytännön aseman ryhmän, jokaisen aseman voi varmuuskopioida haluamasi aseman säilössä tarvittavat tallennustilan tilin avulla.

>[AZURE.NOTE] Kaikki asemat aseman ryhmän sijaittava yksittäisen cloud-palveluntarjoajalta.

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Windows-tilannevedospalvelu integrointi

StorSimple tilannevedoksen hallinta käyttää sovelluksen yhdenmukaisia tietojen Windows aseman varjostus kopioi Service (VSS). VSS helpottaa sovelluksen yhdenmukaisuuden toimittamalla VSS-aware järjestämiseen vaiheittainen tilannevedoksia luonti-sovellusten kanssa. VSS varmistaa, että sovellukset ovat tilapäisesti käytöstä tai lepotilassa, kun tilannevedoksia otetaan. 

VSS StorSimple tilannevedoksen hallinta käyttöönoton toimii SQL Server- ja Yleinen NTFS-asemat. Prosessi on seuraavanlainen: 

1. Pyytäjän, joka on yleensä tietojen hallinta ja suojaus-ratkaisuun (StorSimple tilannevedoksen hallinta kuten) tai varmuuskopio-sovelluksen, käynnistää VSS ja pyytää niiden tietojen kerääminen kohdesovelluksessa writer-ohjelmisto.

2. VSS yhteystietojen writer osan kuvaus tietojen hakemiseen. Kirjoittajan palauttaa varmuuskopioitavien tietojen kuvaus. 

3. VSS signaalit writer valmisteleminen varmuuskopiointi hakeminen. Kirjoittajan valmistelee varmuuskopiointi tiedot täyttämällä avointen tapahtumien tapahtumalokit ja niin edelleen ja ilmoittaa projektinimi

4. VSS ohjaa writer tilapäisesti lopettaa sovelluksen tietoja tallennetuista tiedoista ja varmista, että tietoja ei ole kirjoitettu äänenvoimakkuuden tilannevedos luodaan. Tämä vaihe varmistaa tietojen yhdenmukaisuuden ja kestää yli 60 sekuntia.

5. VSS ohjaa tilannevedoksen luominen Internet-palveluntarjoajaan. Tarjoajat, joka voi olla ohjelmiston tai laitteiston perustuva, hallita asemista, jotka ovat parhaillaan käynnissä ja luoda tilannevedoksia niitä pyydettäessä. Palvelu luo tilannevedoksen, ja ilmoittaa VSS, kun se on valmis.

6. VSS yhteystietojen writer ilmoittamaan sovellus, jossa i/o voit jatkaa ja vahvista, että i/o keskeyttäminen aikana tilannevedoksen luominen onnistui. 

7. Jos kopion onnistui, VSS palauttaa kopion sijainti pyytäjän. 

8. Tiedot on kirjoitettu samalla, kun tilannevedos on luotu, varmuuskopioinnin olla ristiriidassa. VSS poistaa tilannevedoksen, ja ilmoittaa pyytäjän. Pyytäjän joko varmuuskopiointia toistaminen automaattisesti tai ilmoittaa järjestelmänvalvojalle yritä uudelleen myöhemmin.

Seuraavassa kuvassa on artikkelissa.

![VSS prosessi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows-tilannevedospalvelu prosessi** 

## <a name="backup-types-and-backup-policies"></a>Varmuuskopion tyypit ja varmuuskopioinnin määrittäminen

StorSimple tilannevedoksen hallinnan avulla voit varmuuskopioida tiedot ja tallentaa sen paikallisesti ja pilveen. StorSimple tilannevedoksen hallinnan avulla voit varmuuskopioida tiedot heti tai varmuuskopion käytännön avulla voit luoda aikataulun otetaan varmuuskopioita automaattisesti. Varmuuskopion käytäntöjen avulla voit määrittää, kuinka monta tilannevedoksia säilytetään. 

### <a name="backup-types"></a>Varmuuskopion tyypit

StorSimple tilannevedoksen hallinnan avulla voit luoda varmuuskopiot seuraavanlaisia:

- **Paikallinen tilannevedoksia** – paikallisen tilannevedoksia ajankohta aseman tiedot, jotka on tallennettu StorSimple laitteeseen kopioita. Yleensä Tämäntyyppinen varmuuskopiointi voi luoda ja palauttaa nopeasti. Voit käyttää paikallisen tilannevedoksen paikallisen varmuuskopion tapaan.

- **Cloud tilannevedoksia** – Cloud tilannevedoksia ajankohta aseman tiedot, jotka on tallennettu pilvipalveluun kopioita. Cloud tilannevedoksen vastaa tilannevedoksen replikoida eri, sähköntuotannosta tallennustilan järjestelmään. Cloud tilannevedoksia ovat erityisen hyödyllisiä tietojen palauttaminen tilanteissa.

### <a name="on-demand-and-scheduled-backups"></a>Tarvittaessa ja ajoitetun varmuuskopioinnin

StorSimple tilannevedoksen hallinnan avulla voit aloittaa erikseen varmuuskopion luodaan heti tai varmuuskopion käytännön avulla voit ajoittaa toistuvan varmuuskopion toiminnot.

Varmuuskopion käytäntö on automaattinen sääntöjä, joiden avulla voit ajoittaa säännöllisen varmuuskopioinnin suunnittelu. Varmuuskopion käytännön avulla voit määrittää korkojakso ja ottaen ryhmälle määritetyn aseman tilannevedosten parametrit. Käytäntöjen avulla voit määrittää alkamis-ja vanhenemista, kellonaikojen, tiheydet ja säilytysvaatimusten mukaisesti, sekä paikallisissa ja cloud tilannevedoksia. Käytäntö otetaan käyttöön heti, kun olet määrittänyt sen. 

StorSimple tilannevedoksen hallinnan avulla voit määrittää tai määritä varmuuskopion käytännöt tarvittaessa. 

Voit määrittää kullekin varmuuskopion käytäntö, jonka luot seuraavat tiedot:

- **Nimi** – valitun varmuuskopion käytännön yksilöllinen nimi.

- **Kirjoita** – varmuuskopion käytännön; tyyppi joko paikallisen tilannevedoksen tai cloud tilannevedoksen.

- **Äänenvoimakkuuden ryhmän** – äänenvoimakkuutta-ryhmä, johon valittu varmuuskopion käytäntö on liitetty.

- **Säilytys** – säilytettävien varmuuskopioiden määrä. Valitse **kaikki** -valintaruutu, jos kaikki varmuuskopioita säilytetään, kunnes varmuuskopioita kohti enimmäismäärä on saavutettu, jolloin käytäntö tulee epäonnistua ja näyttöön virhesanoman. Vaihtoehtoisesti voit määrittää määrä (1 – 64) säilyttää varmuuskopiot.

- **Päivämäärä** – varmuuskopion käytännön luontipäivämäärä.

Lisätietoja varmuuskopioinnin käytännöt Siirry [StorSimple tilannevedoksen hallinnan käyttö voi luoda ja hallita varmuuskopion käytännöt](storsimple-snapshot-manager-manage-backup-policies.md).

### <a name="backup-job-monitoring-and-management"></a>Varmuuskopiointityön valvonnan ja hallinnan

Voit käyttää StorSimple tilannevedoksen hallinta ja tulevista, ajoitetun ja valmiit varmuuskopion töiden hallinta. Lisäksi StorSimple tilannevedoksen hallinta on enintään 64 valmiit varmuuskopiot luettelon. Luettelon avulla voit asemat tai yksittäisiä tiedostoja etsimiseen ja palauttamiseen. 

Tietoja varmuuskopion töiden seuranta Siirry [StorSimple tilannevedoksen hallinnan käyttö voi tarkastella ja hallita varmuuskopiointityön](storsimple-snapshot-manager-manage-backup-jobs.md).


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple tilannevedoksen hallinnan ammattimainen StorSimple-ratkaisun avulla](storsimple-snapshot-manager-admin.md).

- Lataa [StorSimple tilannevedoksen hallinta](https://www.microsoft.com/download/details.aspx?id=44220).
