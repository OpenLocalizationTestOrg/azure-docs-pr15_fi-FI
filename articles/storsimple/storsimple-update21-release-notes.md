<properties 
   pageTitle="StorSimple 8000 sarjan päivitys 2.2 julkaisutiedot | Microsoft Azure"
   description="Kuvataan uusista ominaisuuksista, ongelmista ja menetelmää StorSimple 8000 sarjan päivitys 2.2."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-22-release-notes"></a>StorSimple 8000 sarjan päivitys 2.2 julkaisutiedot  

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot kuvaavat: n uusiin ominaisuuksiin ja löytää Avaa kriittisten StorSimple 8000 sarjan päivitys 2.2. Niissä on myös tähän julkaisuun sisältyvien StorSimple ohjelmistopäivitykset luettelo. 

Päivitä 2.2 voi suojata StorSimple laitteet, joissa julkaisu (GA) tai päivitä 0,1 päivityksen 2.1 kautta. Päivitä 2.2 liittyvä laite-versio on 6.3.9600.17708.

Tarkista julkaisutiedot tietoja ennen niiden käyttöönottoa päivitys StorSimple ratkaisu.

>[AZURE.IMPORTANT]
> 
> - Päivitä 2.2 on vain ohjelmistopäivitykset. Kestää noin 1,5 2 tuntia asentaa tämä päivitys. 

> - Jos käytössäsi on päivitys 2.1, on suositeltavaa, että asennat päivityksen 2.2 mahdollisimman pian.

> - Saat uusimmat, et ehkä näe päivitykset heti, koska olemme Tee vaiheistettu rahoittaja muutokset. Odota muutamassa päivässä ja sitten tarkistuksen päivitykset uudelleen, koska ne on jälleen käytettävissä pian.


## <a name="whats-new-in-update-22"></a>Mitä uusia ominaisuuksia päivityksen 2.2

Seuraavat avaimen parannukset on tehty päivitys 2.2.

 
- **Automaattisen tilaa regeneroitaviksi optimointi** – kun tiedot on poistettu levinneet valmistellun tietomääristä käyttämättömät tallennustilan lohkot on saadaan takaisin käyttöön. Tässä versiossa on parannettu tilaa regeneroitaviksi prosessin pilvestä, tuloksena on tulossa käyttöön nopeammin verrattuna aiempiin versioihin verrattuna käyttämätön tila.


- **Tilannevedoksen suorituskykyparannukset** – päivityksen 2.2 on parannettu aikaa käsitellä tilannevedoksen tietyissä tilanteissa, kun suuri asemat ovat käytössä ja ei ole tietoja churn rajoitustarve on pilvestä. Tilanne, jossa hyötyä tästä laajentamisesta olisi arkisto-asemat.


- **Tuen hardening paketin kerääminen** – on tehty parannuksia tuki-paketin kerännyt ja tässä versiossa on ladattu. 


- **Päivitä luotettavuuden parannuksia** – tässä versiossa on virheenkorjauksia, joka johtaa päivityksen käyttövarmuuden.

  
 

## <a name="issues-fixed-in-update-22"></a>Päivitä 2.2 korjaamista ongelmista

Seuraavissa taulukoissa on yhteenveto päivitykset 2.2 ja 2.1 korjatut ongelmat.    

| Ei | Toiminto                                    | Ongelma                                                                                                                                                                                                                                                                                        | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|----|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------|---------------------------|
| 1  | Host (isäntä)-suorituskyky                      | Aiemmassa versiossa isännän suorituskykyongelmia havaittiin paikallisesti kiinnitetty aseman luonnin aikana ja paikallisesti kiinnitetty asemaan Porrastettu aseman muunnon aikana. Tässä versiossa, ja tuloksena on aikana aseman luomista ja muuntaminen menettelyt host suorituskyvyn parantaminen korjaa ongelmat.                                                                        | Kyllä                        | Ei                        |
| 2  | Paikallisesti kiinnitetty tietomääristä                     | Harvoin järjestelmä kaatua, kun paikallisesti kiinnitetty aseman luominen. Tämä ohjelmavirhe on korjattu tässä versiossa.                                                                                                                                                               | Kyllä                        | Ei                        |
| 3  | Tiering                                    | Käytettävissä on arviointi kaatuu, kun StorSimple Cloud-laitteiden (8010 ja 8020) metatiedot tasoisen pilveen. Tämä ongelma on korjattu tässä versiossa.                                                                                                                              | Ei                         | Kyllä                       |
| 4  | Tilannevedoksen luominen                          | Oli ongelmat liittyvät tilanteita, joissa on paljon kanssa vaiheittainen tilannevedoksia luominen ja tietojen ei ole churn vähän. Tässä versiossa korjaa ongelmat.                                                                                                                 | Kyllä                        | Kyllä                       |
| 5  | Openstack todennus                   | Käytä Openstack cloud-palveluntarjoajan, kun käyttäjä suoritetaan kyselyjä epäsäännölliset ohjelmavirhe, joka todentamiseen liittyvien missä JSON-jäsentimen tuloksena kaatumisen. Tämä ohjelmavirhe on korjattu tässä versiossa.                                                                                                                              | Kyllä                        | Ei                        |
| 6  | Isännän kopio                             | Ohjelmiston aiemmissa versioissa epäsäännölliset ohjelmavirhe, joka on ODX ajoituksen liittyvät on käytetty yksi osa tietojen kopioiminen toiseen asemaan. Tämä johtaa ohjauskoneen automaattisesti ja järjestelmä voi mahdollisesti Siirry palautus-tilaan. Tämä ohjelmavirhe on korjattu tässä versiossa. | Kyllä                        | Ei       |
| 7  | Ohjattu toiminto (WMI) | Ohjelmiston aiemmissa versioissa, käytettävissä on useita esiintymiä poikkeuksen web välityspalvelimen virhe "<ManagementException> toimittajan latausvirhe". Tämä ohjelmavirhe on merkitty WMI muistivuoto ja on korjattu.                                                               | Kyllä                        | Ei                        |
| 8  | Päivitys                                     | Tiettyjen harvoin ohjelmiston aiemmissa versioissa käyttäjä sai "CisPowershellHcsscripterror" yrittäessäsi Skannaa tai asenna päivitykset. Tämä ongelma on korjattu tässä versiossa.                                                                                        | Kyllä                        | Kyllä                       |
| 9  | Tuki-paketti                            | Tässä versiossa on ollut tuki-paketin kerännyt ja ladata parannuksia.                                                                                                                                                                                                      | Kyllä                        | Kyllä                                    |


## <a name="known-issues-in-update-22"></a>Päivitä 2.2 tunnetut ongelmat

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

## <a name="controller-and-firmware-updates-in-update-22"></a>Päivitä 2.2 ohjauskoneen ja laiteohjelmiston päivitykset

Tässä versiossa on vain ohjelmiston päivitykset. Jos päivität Update 2: a vanhemmalla versiolla, sinun on asennettava ohjaimen, Storport, Spaceport, ja (joissakin tapauksissa) levyn laitteen laitteisto-päivitykset.
 
Katso lisätietoja ohjaimen, Storport, Spaceport ja levyn laitteisto-päivitysten asentaminen StorSimple laitteen [Asenna päivitys 2.2](storsimple-install-update-21.md) .

 
## <a name="virtual-device-updates-in-update-22"></a>Päivitä 2.2 Virtual laitteen päivitykset

Tämä päivitys ei voi käyttää virtual laitteeseen. Uudet virtual laitteet on luotava. 

## <a name="next-step"></a>Seuraava vaihe

Katso, [Asenna päivitys 2.2](storsimple-install-update-21.md) StorSimple laitteen.
