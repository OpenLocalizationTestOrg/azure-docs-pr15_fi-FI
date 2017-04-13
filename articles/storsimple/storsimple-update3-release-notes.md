<properties 
   pageTitle="StorSimple 8000 sarjan Update 3 julkaisutiedot | Microsoft Azure"
   description="Tässä kuvataan uusista ominaisuuksista, ongelmista ja menetelmää StorSimple 8000 sarjan Update 3."
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
   ms.workload="TBD"
   ms.date="10/14/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-3-release-notes"></a>StorSimple 8000 sarjan Update 3: n julkaisutiedot  

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot kuvaavat: n uusiin ominaisuuksiin ja löytää Avaa kriittisten StorSimple 8000 sarjan päivitys 3. Niissä on myös tähän julkaisuun sisältyvien StorSimple ohjelmistopäivitykset luettelo. 

Päivitä 3 voi suojata StorSimple laitteet, joissa julkaisu (GA) tai päivitä 0,1 päivityksen 2.2 kautta. Update 3 liittyvä laite-versio on 6.3.9600.17759.

Tarkista julkaisutiedot tietoja ennen niiden käyttöönottoa päivitys StorSimple ratkaisu.

>[AZURE.IMPORTANT]
> 
> - Päivitä 3 on Laiteohjelmisto, LSI ohjaimen ja laitteisto ja Storport ja Spaceport päivittää. Kestää noin 1,5 2 tuntia asentaa tämä päivitys. 

> - Saat uusimmat, et ehkä näe päivitykset heti, koska olemme Tee vaiheistettu rahoittaja muutokset. Odota muutamassa päivässä ja sitten tarkistuksen päivitykset uudelleen, koska ne on jälleen käytettävissä pian.


## <a name="whats-new-in-update-3"></a>Mitä uusia ominaisuuksia Update 3

Seuraavat avaimen parannukset ja virheenkorjauksia on tehty Update 3.

 
- **Automaattisen tilaa regeneroitaviksi muutokset** – käynnistäminen päivityksen 3, tilaa regeneroitaviksi algoritmit Suorita valmiustilassa ohjaimen järjestelmän, tuloksena on nopeampaa suorittamisen. Lisätietoja portit, joita tarvitaan toimimaan zonea viittaavat [StorSimple verkko vaatimuksia](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).

- **Suorituskykyparannukset** – Update 3 on parannettu pilveen suorituskyvyn luku-ja kirjoitusoikeudet.

- **Siirtoon liittyviä parannuksia** – tässä versiossa, useita virheenkorjauksia ja parannukset oli valmis siirto-toiminnolla 5000/7000 sarjan laitteilla 8000 sarjan laitteisiin. Lisätietoja siitä, miten siirto-toiminnolla, siirry [siirron 5000/7000 sarjan laitteesta 8000 sarjan laitteeseen](https://www.microsoft.com/en-us/download/details.aspx?id=47322). 

- **Aiheeseen liittyvät korjaukset seuranta** - virheet seurantaa kaavioiden, palvelun Raporttinäkymät-ikkunan ja laitteen Raporttinäkymät-ikkunan tässä versiossa on vahvistettu.


## <a name="issues-fixed-in-update-3"></a>Päivitä 3 korjauksista

Seuraavissa taulukoissa on yhteenveto Update 3 korjatut ongelmat.    

| Ei | Toiminto                                    | Ongelma                                                                                                                                                                                                                                                                                        | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Isännän tietojen siirtäminen                      | Aiemmassa versiossa StorSimple Cloud laitteen on siirtymistä offline-tilaan isännän tietojen siirron aikana. Tämä ongelma on korjattu tässä versiossa.                                                | Ei                        | Kyllä                        |
| 2  | Paikallisesti kiinnitetty tietomääristä                     | Edellisen olivat i/o virheet, äänenvoimakkuuden muuntaminen virheet ja Tietopolkua virheiden paikallisesti kiinnitetty levyasemien liittyvät ongelmat. Ongelmaan on pääkansio aiheuttaa ja kiinteä tässä versiossa.                | Kyllä                        | Ei                       |
| 3  | Seuranta                                    | Käytettävissä on useita raportoinnin yksiköt ja seurannan sekä laitteen Raporttinäkymät-ikkunan kaavioita, jolla virheellisten tietojen näkyvät paikallisesti kiinnitetty levyasemien liittyvät ongelmat. Tässä versiossa korjaa ongelmat. | Kyllä                         | Ei                       |
| 4  | Paksu kirjoituksia i/o                          | Kun käytät StorSimple henkilöihin liittyvät paksu kirjoituksia työmääriä, käyttäjän suoritetaan kyselyjä epäsäännölliset ohjelmavirhe missä työsarja on parhaillaan tasoisen pilveen kyselyjä. Tämä ohjelmavirhe on korjattu tässä versiossa. | Kyllä                        | Kyllä                       |
| 5  | Varmuuskopiointi                                     | Kun käyttäjän noudatit remote Kloonaa varmuuskopion, ne suoritetaan cloud virheilmoituksen tiettyjä harvoin ohjelmiston aiemmissa versioissa ja toiminto olisi virhe ulos. Tässä versiossa ongelma on kiinteä ja toiminto on valmis.             | Kyllä                        | Kyllä                       |
| 6  | Varmuuskopion käytäntö                                     | Tiettyjen harvoin ohjelmiston aikaisempien versioiden oli ohjelmavirhe liittyvät varmuuskopion käytännön poisto. Tämä ongelma on korjattu tässä versiossa.       | Kyllä                        | Kyllä                       |



## <a name="known-issues-in-update-3"></a>Update 3: n tunnetut ongelmat

Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.

| Ei. | Toiminto | Ongelma | Kommenttien tai vaihtoehtoinen menetelmä | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Levyn quorum | Harvoin, jos EBOD kehyksen 8600: aa laitteen levyjen useimpia katkaistaan tuloksena on levy ole koontiresurssi sitten tallennustilan resurssivarantoon voi siirtyä offline-tilassa. Se säilyy offline-tilassa, vaikka levyjen muodostetaan. | Tarvitset Käynnistä laite uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support for seuraavat vaiheet. | Kyllä | Ei |
| 2 | Virheellinen ohjaimen tunnus | Kun ohjauskoneen korvaa suoritetaan, controller 0 saattaa näkyä muodossa ohjauskoneen 1. Aikana ohjauskoneen korvaavan kun kuva on ladattu peer-solmu ohjaimen tunnus voidaan näyttää sisältökorttina aluksi peer ohjauskoneen ID-tunnuksellasi. Harvoin ongelman myös voi tarkastella järjestelmän uudelleenkäynnistyksen jälkeen. | Käyttäjän mitään toimia ei tarvita. Tässä tilanteessa ratkaisee itse ohjauskoneen korvaaminen päätyttyä. | Kyllä | Ei |
| 3 | Tallennustilan tilit | Tallennustilan-palvelun avulla voit poistaa tilin tallennustila on ei-tuettu skenaario. Tämä johtaa tilanteeseen, jossa käyttäjätietoja ei voi hakea.|  | Kyllä | Kyllä |
| 4 | Laitteen automaattisesti | Äänenvoimakkuuden säilön saman lähde-laitteesta eri kohde laitteet useita failovers ei tueta. Yksittäisen toimimattoman laitteesta automaattisesti useilla eri laitteilla tekee aseman säilöjen epäonnistui kautta laitteen ensimmäisenä, menettävät tietojen omistajuus. Näiden automaattisesti, kun aseman näiden säilöjen näkyvät tai toimivat eri tavalla, kun niitä tarkastellaan Azure perinteinen-portaalissa. | | Kyllä | Ei |
| 5 | Asennus | StorSimple sovittimen SharePoint-asennuksen aikana, sinun täytyy antaa laitteen IP, asenna onnistu järjestyksessä.    | | Kyllä | Ei |
| 6 | Web-välityspalvelin | Jos WWW-välityspalvelimen määritykset on määritetty protokollaksi HTTPS-device Servicen viestintä vaikuttaako ja laitteen siirtyvät offline-tilassa. Tuki pakettien luodaan myös merkittäviä resursseja laitteen aikana. | Varmista, että web välityspalvelimen URL-osoite on määritetty protokollaksi HTTP. Lisätietoja Siirry [web-välityspalvelimen määrittäminen laitteeseesi](storsimple-configure-web-proxy.md). | Kyllä | Ei |
| 7 | Web-välityspalvelin | Jos määrittäminen käyttöön web-välityspalvelimen rekisteröidyn laitteen, täytyy aktiivinen ohjauskoneen laitteen uudelleen. | | Kyllä | Ei |
| 8 | Suuri cloud viive ja suuri i/o työmäärää | Kun StorSimple laitteen kohtaa yhdistelmä erittäin suuri cloud viiveitä (järjestyksen sekuntia) ja suuri i/o työmäärää, laite tietomääristä Siirry eivät toimi oikein vaiheeseen ja i voi epäonnistua "laite ei ole valmis"-virhe. | Tarvitset manuaalisesti uudelleen laitteen ohjaimet tai suorittaa laitteen automaattisesti, jos haluat korjata tilanteen. | Kyllä | Ei |
| 9 | Azure PowerShell | Kun käytät StorSimple cmdlet-komento **Get-AzureStorSimpleStorageAccountCredential & #124; Valitse-objekti - ensin 1 - Odota** valitaksesi ensimmäistä objektia niin, että voit luoda uuden **VolumeContainer** objektin cmdlet-komento palauttaa kaikki objektit. | Rivitä cmdlet sulkeissa seuraavasti: **(Get-Azure-StorSimpleStorageAccountCredential) & #124; Valitse-objekti - ensin 1 - Odota** | Kyllä | Kyllä |
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
| 20 |Paikallisesti kiinnitetty tietomääristä | Jos yrität Porrastettu aseman (luotu ja kloonatun päivityksen 1.2 kanssa tai vanhempi) muuntaminen paikallisesti kiinnitetty äänenvoimakkuus ja laite on loppumassa tai on cloud käyttökatkosta, clone(s) voi olla vioittunut.| Tämä ongelma ilmenee vain asemat, jotka on luotu ja kloonatun kanssa päivitystä edeltävässä 2.1-ohjelmiston kanssa. Tämä on oltava epäsäännölliset skenaario.|
| 21 | Avauksen ja vaihdon muuntaminen | Älä päivitä liitetty aseman aseman muuntoa ollessa käynnissä ACRs (tasoisen paikallisesti kiinnitetyt tai päinvastoin). Päivitys ACRs saattaa johtaa tietovirheitä. | Tarvittaessa Päivitä ennen aseman muuntoa ACRs ja soita enempää ACR päivitykset muuntaminen ollessa käynnissä. |
| 22 | Päivitykset | Update 3 käytettäessä Azure perinteinen portaalissa **ylläpito** -sivun näyttää seuraavan sanoman liittyvät Update 2 – "StorSimple 8000 sarjan päivitys 2 on nyt mahdollista, jos Microsoft voi kerätä lokitiedot itse laitteestasi, kun olemme tunnistaa mahdolliset ongelmat". Tämä on johtamalla heitä, kun se osoittaa, että laitteen päivitetään päivityksen 2. Kun laite on päivitetty Update 3 succeesfully, tämä viesti häviävät. | Tämä ongelma korjataan uuteen versioon. | Kyllä | Ei |


## <a name="controller-and-firmware-updates-in-update-3"></a>Ohjaimen ja laitteisto-päivitykset näkyvät Update 3

Tässä versiossa on LSI ohjaimen ja laiteohjelmiston päivitykset. Katso lisätietoja siitä, miten voit asentaa LSI ohjaimen ja laiteohjelmiston StorSimple laitteen [Asenna päivitys 3](storsimple-install-update-3.md) .

 
## <a name="virtual-device-updates-in-update-3"></a>Virtuaalinen laitteen päivitykset päivitys 3

Tämä päivitys ei voi käyttää StorSimple Cloud laitteen (tunnetaan myös nimellä virtual laite). Uudet virtual laitteet on luotava. 


## <a name="next-step"></a>Seuraava vaihe

Lisätietoja StorSimple laitteen [Asenna päivitys 3](storsimple-install-update-3.md) .
