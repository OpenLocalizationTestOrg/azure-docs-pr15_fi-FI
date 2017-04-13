<properties 
   pageTitle="StorSimple 8000 sarjan Update 2 julkaisutiedot | Microsoft Azure"
   description="Kuvataan uusista ominaisuuksista, ongelmista ja menetelmää StorSimple 8000 sarjan Update 2."
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

# <a name="storsimple-8000-series-update-2-release-notes"></a>StorSimple 8000 sarjan Update 2: n julkaisutiedot  

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot kuvaavat: n uusiin ominaisuuksiin ja löytää Avaa kriittisten StorSimple 8000 sarjan Update 2. Niissä on myös StorSimple-ohjelmisto, ohjaimen ja levyn laitteisto päivitykset Tämä julkaisu sisältää luettelon. 

Päivitä 2 voi suojata StorSimple laitteet, joissa julkaisu (GA) tai päivitä 0,1 päivityksen 1.2 kautta. Päivityksen 2 liittyvä laite-versio on 6.3.9600.17673.

Tarkista julkaisutiedot tietoja ennen niiden käyttöönottoa päivitys StorSimple ratkaisu.

>[AZURE.IMPORTANT]
> 
- Kestää noin 4 – 7 tuntia asentaa tämä päivitys (mukaan lukien Windows-päivitysten). 
- Päivitä 2 on ohjelmien LSI ohjaimen ja Suoritettaessa laitteisto päivitykset.
- Saat uusimmat, et ehkä näe päivitykset heti, koska olemme Tee vaiheistettu rahoittaja muutokset. Odota muutamassa päivässä ja sitten tarkistuksen päivitykset uudelleen, koska ne on jälleen käytettävissä pian.


## <a name="whats-new-in-update-2"></a>Päivitä 2: n uudet ominaisuudet

Päivitys 2 sisältää seuraavat uudet ominaisuudet.

- **Kiinnitetty paikallisesti tietomääristä** – StorSimple 8000 sarja, tietolohkot aiemmissa versioissa on tasoisen pilveen käyttö perusteella. Oli ei voi varmistaa, että lohkot pitää paikalliseen. 2-päivityksen luodessasi asema, voit määrittää aseman kuin asemaan tietojen paikallisesti kiinnitetty ja ensisijainen ei tasoisen pilveen. Paikallisesti kiinnitetty tietomääristä tilannevedosten edelleen kopioidaan varmuuskopion pilveen, niin, että pilveen voidaan käyttää tietojen mobility ja tietojen palauttamista varten. Voit lisäksi muuttaa asematyyppi (eli Muunna tasoisen paikallisesti kiinnitetty tietomääristä asemat ja muunna paikallisesti kiinnitetyt asemat tasoisen). 

- **StorSimple virtual laitteen parannuksia** – aiemmin StorSimple 8000 sarjan sijoitettu virtual laitteen tietojen palauttaminen tai kehitys/testi ratkaisuksi. Oli vain yksi mallin virtual laitteen (malli 1100). Päivitä 2: ssa kaksi virtual laitteen mallit: 

     - 8010 (niin sanottuja 1100) – ei muutu; 30 teratavua kapasiteetti ja käyttää Azure vakio tallennuspaikkaa.
     - 8020 – on 64 teratavua kapasiteetti ja käyttää Azure Premium tallennustilan suorituskyvyn parantamiseksi.

    On yksittäinen Näennäiskiintolevyn sekä virtual laitteen mallien (8010/8020). Kun käynnistät virtual laitteen, havaitsee ympäristö parametrit ja koskee oikea malliversio.

- **Verkko-parannuksia** – Update 2 sisältää seuraavat verkko parannukset:

     - Useita NIC voi ottaa käyttöön pilveen, jotta automaattisesti voi ilmetä, jos, jota Verkkokortti ei.
     - Reititys parannukset kanssa kiinteä mittaukset cloud käytössä lohkot.
     - Epäonnistuneiden resurssien automaattisesti ennen online uudelleen.
     - Palveluvirheet uusien ilmoitukset.

- **Päivittäminen parannuksia** – päivityksen 1.2 ja aiempi versio, StorSimple 8000 sarjan on päivitetty kaksi kanavien kautta: klusterointi, iSCSI, ja niin edelleen ja Microsoft Update binaaritiedostot ja laiteohjelmiston Windows Update.
    Päivitys 2 käyttää Microsoft Update, kaikki päivittää paketteja. Tämä on johtaa riittävästi aikaa korjaaminen tai tekevät failovers. 

- **Laiteohjelmiston** – seuraavat laiteohjelmiston sisältyvät:
    - LSI: lsi_sas2.sys tuoteversio 2.00.72.10
    - Vain Suoritettaessa (Kiintolevy päivityksiä): XMGG, XGEG, KZ50, F6C2 ja VR08

- **Ennakoiva tuki** – päivityksen 2 avulla Microsoft voi tuoda lisää vianmääritystiedot laitteesta. Kun toimintojen tiimimme tunnistaa laitteet, jotka on ongelmia, emme paremmin varustettu tietojen keräämiseen laitteesta ja vianmääritys. **Hyväksymällä Update 2 annat meille ennakoiva tukea**.    
 

## <a name="issues-fixed-in-update-2"></a>Päivitä 2 korjauksista

Seuraavissa taulukoissa on yhteenveto päivitykset 2 korjatut ongelmat.    

| Ei. | Toiminto | Ongelma | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|--------------------------------|--------------------------------|
| 1 | Verkon liityntäkohdat | Päivitys 1 päivityksen jälkeen StorSimple hallintapalvelu raportoitu Data2 ja Data3 portit epäonnistui, yksi ohjaimen. Tämä ongelma on korjattu. | Kyllä | Ei |
| 2 | Päivitykset | Päivitys 1 päivityksen jälkeen äänimerkillä ilmoitusten tapahtui Azure perinteinen portaalissa useilla eri laitteilla. Tämä ongelma on korjattu. | Kyllä | Ei |
| 3 | Openstack todennus | Kun Käytä Openstack cloud-palveluntarjoajan, näyttöön voi tulla virhesanoma, cloud todennus-merkkijono on liian pitkä. Tämä on vahvistettu. | Kyllä | Ei |


## <a name="known-issues-in-update-2"></a>Päivitä 2: n tunnetut ongelmat

Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.

| Ei. | Toiminto | Ongelma | Kommenttien tai vaihtoehtoinen menetelmä | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Levyn quorum | Harvoin, jos EBOD kehyksen 8600: aa laitteen levyjen useimpia katkaistaan tuloksena on levy ole koontiresurssi sitten tallennustilan resurssivarantoon voi siirtyä offline-tilassa. Se säilyy offline-tilassa, vaikka levyjen muodostetaan. | Tarvitset Käynnistä laite uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support for seuraavat vaiheet. | Kyllä | Ei |
| 2 | Virheellinen ohjaimen tunnus | Kun ohjauskoneen korvaa suoritetaan, controller 0 saattaa näkyä muodossa ohjauskoneen 1. Aikana ohjauskoneen korvaavan kun kuva on ladattu peer-solmu ohjaimen tunnus voidaan näyttää sisältökorttina aluksi peer ohjauskoneen ID-tunnuksellasi. Harvoin ongelman myös voi tarkastella järjestelmän uudelleenkäynnistyksen jälkeen. | Käyttäjän mitään toimia ei tarvita. Tässä tilanteessa ratkaisee itse ohjauskoneen korvaaminen päätyttyä. | Kyllä | Ei |
| 3 | Tallennustilan tilit | Tallennustilan-palvelun avulla voit poistaa tilin tallennustila on ei-tuettu skenaario. Tämä johtaa tilanteeseen, jossa käyttäjätietoja ei voi hakea.|  | Kyllä | Kyllä |
| 4 | Laitteen automaattisesti | Äänenvoimakkuuden säilön saman lähde-laitteesta eri kohde laitteet useita failovers ei tueta. Yksittäisen toimimattoman laitteesta automaattisesti useilla eri laitteilla tekee aseman säilöjen epäonnistui kautta laitteen ensimmäisenä, menettävät tietojen omistajuus. Näiden automaattisesti, kun aseman näiden säilöjen näkyvät tai toimivat eri tavalla, kun niitä tarkastellaan Azure perinteinen-portaalissa. | | Kyllä | Ei |
| 5 | Asennus | StorSimple sovittimen SharePoint-asennuksen aikana, sinun täytyy antaa laitteen IP, asenna onnistu järjestyksessä.    | | Kyllä | Ei |
| 6 | Web-välityspalvelin | Jos web-välityspalvelimen määritykset on määritetty protokollaksi HTTPS-device Servicen viestintä vaikuttaako ja laitteen siirtyvät offline-tilassa. Tuki pakettien luodaan myös merkittäviä resursseja laitteen aikana. | Varmista, että web välityspalvelimen URL-osoite on määritetty protokollaksi HTTP. Lisätietoja Siirry [web-välityspalvelimen määrittäminen laitteeseesi](storsimple-configure-web-proxy.md). | Kyllä | Ei |
| 7 | Web-välityspalvelin | Jos määrittäminen käyttöön web-välityspalvelimen rekisteröidyn laitteen, täytyy aktiivinen ohjauskoneen laitteen uudelleen. | | Kyllä | Ei |
| 8 | Suuri cloud viive ja suuri i/o työmäärää | Kun StorSimple laitteen kohtaa yhdistelmä erittäin suuri cloud viiveitä (järjestyksen sekuntia) ja suuri i/o työmäärää, laite tietomääristä Siirry eivät toimi oikein vaiheeseen ja i voi epäonnistua "laite ei ole valmis"-virhe. | Tarvitset manuaalisesti uudelleen laitteen ohjaimet tai suorittaa laitteen automaattisesti, jos haluat korjata tilanteen. | Kyllä | Ei |
| 9 | Azure PowerShell | Kun käytät StorSimple cmdlet-komento **Get-AzureStorSimpleStorageAccountCredential & #124; Valitse-objekti - ensin 1 - Odota** valitaksesi ensimmäistä objektia niin, että voit luoda uuden **VolumeContainer** objektin cmdlet-komento palauttaa kaikki objektit. | Rivitys-cmdlet-komennolla sulkeissa seuraavasti: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Valitse-objekti - ensin 1 - Odota** | Kyllä | Kyllä |
| 10| Siirron | Kun useita aseman säilöjen välitetään siirtoa varten, uusimman varmuuskopion EETA on päde vain ensimmäisen aseman säilö. Rinnakkaisia siirron lisäksi käynnistyy, kun ensimmäinen aseman säilö ensin 4 varmuuskopioiden siirretään. | On suositeltavaa siirtää yhden aseman säilön kerrallaan. | Kyllä | Ei |
| 11| Siirron | Palautuksen jälkeen asemat ei lisätä varmuuskopion käytännön tai virtual levy-ryhmään. | Tarvitset asemat lisääminen varmuuskopion käytännön luominen varmuuskopiot. | Kyllä | Kyllä |
| 12| Siirron | Kun siirto on valmis, 5000/7000 sarjan laitteella odotustilan ei siirretyt tiedot säilöjen. | On suositeltavaa, että poistat siirretyt tiedot säilöjen, kun siirto on valmis ja vahvistettu. | Kyllä | Ei |
| 13| Kloonaa ja DR | StorSimple laitteessa päivitys 1 ei Kloonaa tai suorittaa palauttaminen päivitystä edeltävässä 1 käyttö laitteeseen. | Sinun on päivitettävä kohdelaitteen Update 1, jotta näitä toimintoja | Kyllä | Kyllä |
| 14 | Siirron | Määritysten varmuuskopiointi-siirron saattaa epäonnistua 5000 7000 sarjan laitteessa, kun on äänenvoimakkuutta-ryhmissä, joissa ei ole liitetty asemat. | Poista kaikki tyhjä aseman ryhmät, joilla ei ole liitetty asemat ja yritä määritysten varmuuskopiointi.| Kyllä | Ei |
| 15 | Azure PowerShellin cmdlet-komennot ja paikallisesti kiinnitetty asemat | Et voi luoda paikallisesti kiinnitetty aseman kautta Azure PowerShellin cmdlet-komennot. (Mitään asemaa, voit luoda PowerShellin Azure kautta oltava tasoisen.) |Voit määrittää paikallisesti kiinnitetty tietomääristä aina käyttämällä StorSimple hallintapalvelu.| Kyllä | Ei |
| 16 |Paikallisesti kiinnitetty tietomääristä levytilaa | Jos poistat paikallisesti kiinnitetty äänenvoimakkuutta, uusia levytilaa eivät päivity välittömästi. StorSimple hallintapalvelu päivittää paikallisen tilaa noin kerran tunnissa.| Odota tuntia ennen kuin yrität luoda uuden äänenvoimakkuutta. | Kyllä | Ei |
| 17 | Paikallisesti kiinnitetty tietomääristä | Palauta työtäsi paljastaa tilapäinen tilannevedoksen varmuuskopion varmuuskopion luettelon, mutta vain palautustyön kestoa. Lisäksi se paljastaa virtual levy-ryhmä ja etuliite **tmpCollection** **Varmuuskopioinnin määrittäminen** -sivulla, mutta vain palautustyön kestoa. | Tämä ongelma voi ilmetä, jos oman palautustyön vain paikallisesti on kiinnitetty asemat tai sekä paikallisesti kiinnitetty ja Porrastettu asemat. Jos palautustyön sisältää vain Porrastettu asemat, tämä ongelma ilmene. Käyttäjän toimia ei tarvita. | Kyllä | Ei |
| 18 | Paikallisesti kiinnitetty tietomääristä | Jos peruutat palautustyön ja ohjauskoneen automaattisesti tapahtuu välittömästi jälkeenpäin palautustyön näkyy **epäonnistui** sen sijaan, että se on **Peruutettu**. Jos palautustyö epäonnistuu, ohjain-vikasietotila ilmenee välittömästi jälkeenpäin palautustyön näkyy sen sijaan, että **epäonnistui** **Peruutettu** . | Tämä ongelma voi ilmetä, jos oman palautustyön vain paikallisesti on kiinnitetty asemat tai sekä paikallisesti kiinnitetty ja Porrastettu asemat. Jos palautustyön sisältää vain Porrastettu asemat, tämä ongelma ilmene. Käyttäjän toimia ei tarvita. | Kyllä | Ei |
| 19 |Paikallisesti kiinnitetty tietomääristä | Jos peruutat palauttaminen työn tai palautuksen epäonnistuu, ja valitse controller-vikasietotila ilmenee, Lisää palautustyön näkyy **Projektit** -sivulla. | Tämä ongelma voi ilmetä, jos oman palautustyön vain paikallisesti on kiinnitetty asemat tai sekä paikallisesti kiinnitetty ja Porrastettu asemat. Jos palautustyön sisältää vain Porrastettu asemat, tämä ongelma ilmene. Käyttäjän toimia ei tarvita. | Kyllä | Ei |
| 20 |Paikallisesti kiinnitetty tietomääristä | Jos yrität Porrastettu aseman (luotu ja kloonatun päivityksen 1.2 kanssa tai vanhempi) muuntaminen paikallisesti kiinnitetty äänenvoimakkuus ja laite on loppumassa tai on cloud käyttökatkosta, clone(s) voi olla vioittunut.| Tämä ongelma ilmenee vain asemat, jotka on luotu ja kloonatun kanssa ennen Update 2-ohjelmisto. Tämä on oltava epäsäännölliset skenaario.|
| 21 | Avauksen ja vaihdon muuntaminen | Älä päivitä liitetty aseman aseman muuntoa ollessa käynnissä ACRs (tasoisen paikallisesti kiinnitetyt tai päinvastoin). Päivitys ACRs saattaa johtaa tietovirheitä. | Tarvittaessa Päivitä ennen aseman muuntoa ACRs ja soita enempää ACR päivitykset muuntaminen ollessa käynnissä. |

## <a name="controller-and-firmware-updates-in-update-2"></a>Ohjaimen ja laitteisto-päivitykset näkyvät Update 2

Tässä versiossa päivittää ohjaimen ja levyn laitteisto laitteessasi.
 
- Lisätietoja LSI laitteisto päivityksen, on Microsoft Knowledge base-artikkelissa 3121900. 
- Lisätietoja levyn laitteisto päivityksen, on Microsoft Knowledge base-artikkelissa 3121899.
 
## <a name="virtual-device-updates-in-update-2"></a>Virtuaalinen laitteen päivitykset päivityksen 2

Tämä päivitys ei voi käyttää virtual laitteeseen. Uudet virtual laitteet on luotava. 

## <a name="next-step"></a>Seuraava vaihe

Lisätietoja StorSimple laitteen [asentaa Update 2](storsimple-install-update-2.md) .
