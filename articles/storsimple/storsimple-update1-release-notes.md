<properties 
   pageTitle="StorSimple 8000 sarjan päivitys 1.2 julkaisutiedot | Microsoft Azure"
   description="Tässä kuvataan uusista ominaisuuksista, ongelmista ja menetelmää StorSimple 8000 sarjan päivitys 1.2."
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
   ms.date="08/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-8000-series-update-12-release-notes"></a>StorSimple 8000 sarjan päivitys 1.2 julkaisutiedot  

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot kuvaavat uusista ominaisuuksista ja avaa kriittisten tunnistaa StorSimple 8000 sarjan päivitys 1.2. Niissä on myös StorSimple-ohjelmisto, ohjaimen ja levyn laitteisto päivitykset tähän julkaisuun sisältyvien luettelo. 

Päivitä 1.2 voi suojata StorSimple laitteet, joissa Release (GA), 0,1 päivityksen, Päivitä 0,2 tai 0,3 Update-ohjelmisto. Päivitä 1.2 ei ole käytettävissä, jos laite toimii päivitys 1 tai Päivitä 1.1. Jos laite toimii Release (GA), ota [yhteyttä Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) avustaminen tämän päivityksen asentamisen.

Seuraavassa taulukossa on lueteltu päivitykset 1, 1.1 ja 1.2 vastaavan laitteen ohjelmistoversiot.

| Jos päivitys on... | Tämä on laite-ohjelmistoversiota. |
|---------------------|---------------------------------------|
| Päivitä 1.2          | 6.3.9600.17584                        |
| Päivitä 1.1          | 6.3.9600.17521                        |
| Päivitä 1.0          | 6.3.9600.17491                        |

Tarkista julkaisutiedot tietoja ennen niiden käyttöönottoa päivitys StorSimple ratkaisu. Lisätietoja on artikkelissa [StorSimple laitteen 1.2 päivityksen](storsimple-install-update-1.md)asentamisesta. 

>[AZURE.IMPORTANT]
> 
- Kestää noin 5 – 10 tuntia asentaa tämä päivitys (mukaan lukien Windows-päivitysten). 
- Päivityksen 1.2 on ohjelmiston, LSI ohjaimen ja levyn laitteisto päivitykset. Jos haluat asentaa, noudata ohjeita [Asenna päivitys 1.2 StorSimple laitteen](storsimple-install-update-1.md).
- Saat uusimmat, et ehkä näe päivitykset heti, koska olemme Tee vaiheistettu rahoittaja muutokset. Etsi uudelleen muutaman päivän päivityksiä kuin nämä tulee saataville pian.


## <a name="whats-new-in-update-12"></a>Mitä uusia ominaisuuksia päivityksen 1.2

Nämä ominaisuudet on ensin julkaistu päivitys 1, joka on tehty rajattu käyttäjien käytettävissä. Useimmat StorSimple käyttäjien nähdä päivityksen 1.2 versiossa seuraavat uudet ominaisuudet ja parannukset:

- **Siirron 5000 7000 sarjasta 8000 sarjan laitteet** – tässä versiossa esitellään uusi siirto-ominaisuus, jolla StorSimple 5000 7000 sarjan laitteen käyttäjät voivat siirtää tietonsa fyysinen StorSimple 8000 sarja-laitteen tai virtual laitteen. Siirto-ominaisuus on kaksi arvoa propositioita pysyisi:                                                                  

    - **Liiketoiminnan jatkuvuus**, ottamalla siirto 8000 sarjan laitteiden 5000 7000 sarjan laitteita olevat tiedot.
    - **Improved ominaisuus tarjouksia 8000 sarjan laitteiden**, kuten tehokas keskitetyn hallinnan useiden laitteiden StorSimple hallinta-palvelun kautta paremmin luokan laitteiston ja päivittää laitteisto, virtual laitteiden tai tietojen mobility ominaisuudet tulevat ohjeet.

    Lisätietoja on ohjeaiheessa [siirtymisopas](http://www.microsoft.com/download/details.aspx?id=47322) lisätietoja siitä, miten voit siirtää StorSimple 5000 7000 sarjan 8000-sarjan laitteella. 

- **Käytettävyys Azure Government-portaalissa** – StorSimple on nyt saatavilla Azure Government-portaalissa. Katso ottamisesta käyttöön [StorSimple laitteen Azure Government-portaalissa](storsimple-deployment-walkthrough-gov.md).

- **Cloud palveluntarjoajia tuki** – muut cloud palveluntarjoajat tuettu ovat Amazon S3 Amazon S3 RRS, HP ja OpenStack (beta).

- **Päivityksen uusimman tallennustilan API** – tässä versiossa StorSimple on päivitetty Azuren tallennustilaan uusimmat service API. StorSimple 8000 sarjan laitteet, joissa on käytössä päivitystä edeltävässä 1 ohjelmistoversiot (Release 0,1 0,2 ja 0,3) Azure tallennustilan Service API 2009 17 heinäkuussa vanhempia versioita. Ilmoitetulla tavalla päivitetyt [tallennustilan palvelun versioiden poistaminen ilmoitus](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx)1 elokuun 2016, jotka poistettu nämä API. On välttämätöntä käyttää ennen 1 elokuun 2016 StorSimple 8000 sarjan päivitys 1. Jos et kiellä, StorSimple laitteiden lakkaavat toimimasta oikein.

- **Tue varten vyöhykkeen tarpeettomat Storage (ZRS)** – kanssa Storage-API uusimman päivityksen StorSimple 8000 sarjan tukee vyöhykkeen tarpeettomat Storage (ZRS) lisäksi paikallisesti (LRS tarpeettomat tallennustilan) ja Geo tarpeettomat Storage (GRS). Lisätietoja on tässä [artikkelissa Azure-redundancy tallennusasetukset](../storage/storage-redundancy.md) ZRS lisätietoja.

- **Parannettu ensimmäisen käyttöönoton ja päivityksen kokemus** – tässä versiossa, asennus ja Päivitä prosesseja on parannettu. Ohjatun määritystoiminnon avulla asennuksen on entistä parempi antaa palautetta käyttäjän verkon määritys- ja palomuuriasetukset eivät ole oikein. Lisää diagnostiikan cmdlet-komennot saanut apuna verkko laitteen vianmääritys. Katso [vianmääritys käyttöönoton artikkelissa](storsimple-troubleshoot-deployment.md) lisätietoja uusi diagnostiikan Cmdlet-komentoja käytetään vianmäärityksessä.

## <a name="issues-fixed-in-update-12"></a>Päivitä 1.2 korjaamista ongelmista

Seuraavassa taulukossa on yhteenveto päivitykset 1.2, 1.1 ja 1 korjatut ongelmat.    


| Ei. | Toiminto | Ongelma | Päivityksen | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|-----------------|---------------------------------|--------------------------------|
| 1 | Windows PowerShell StorSimple | Kun käyttäjä etäyhteyden välityksellä StorSimple laitteen StorSimple Windows PowerShellin avulla ja käynnistää ohjatun määritystoiminnon, kaatumisen tapahtui tietojen 0 IP on kirjoitettu mahdollisimman pian. Tämä ohjelmavirhe on korjattu päivitys 1. | Päivitys 1 | Kyllä | Kyllä |
| 2 | Factory palauttaminen | Joissakin tapauksissa suorittaessasi factory Palauta-StorSimple laite on ollut jumissa ja näkyviin seuraava sanoma: **Palauta factory on käynnissä (vaihe 8)**. Tämä on tapahtunut Jos CTRL + C painetaan cmdlet ollessa käynnissä. Tämä ohjelmavirhe on korjattu.| Päivitys 1 | Kyllä | Ei |
| 3 | Factory palauttaminen | Kun epäonnistuneita kahden ohjauskoneen factory palauttaa, on oikeus laitteen rekisteröinnin. Tämä tuloksena ei tueta Järjestelmäkokoonpano. Update-1 virhesanoma näkyy ja rekisteröintiä on estetty laitteessa, on epäonnistunut factory palauttaa. | Päivitys 1 | Kyllä | Ei |
| 4 | Factory palauttaminen | Joissakin tapauksissa EPÄTOSI positiivinen ristiriita ilmoitukset on esitetty. Virheellinen ristiriita ilmoitusten luodaan enää päivitys 1 käyttäviin laitteisiin. | Päivitys 1 | Kyllä | Ei |
| 5 | Factory palauttaminen | Jos factory Palauta keskeytyi ennen valmistumista, laite palautus-tilassa ja etkä voi käyttää Windows PowerShellin StorSimple. Tämä ohjelmavirhe on korjattu. | Päivitys 1 | Kyllä | Ei |
| 6 | Tietojen palauttaminen | Tietojen palauttaminen (DR) ohjelmavirhe on vahvistettu, jossa DR epäonnistuu kohdelaitteen varmuuskopioiden etsinnän aikana. | Päivitys 1 | Kyllä | Kyllä |
| 7 | Merkkivalot seuranta | Joissakin tapauksissa seuranta merkkivalot käytettävien laitteen ei tarkoita oikeaa tilaa. Sininen LED on poistettu käytöstä. TIETOJEN 0 ja 1 merkkivalot tiedot on vilkkuvat jopa kun nämä liityntäkohdat ei ole määritetty. Ongelma on korjattu ja oikean tilan ilmaisimina nyt seurantaa merkkivalot.  | Päivitys 1 | Kyllä | Ei |
| 8 | Merkkivalot seuranta | Joissakin tapauksissa sen jälkeen, kun päivitys 1, sininen valo aktiivinen ohjaimen käytöstä siten, että vaikea tunnistaa active ohjauskoneen. Tämä ongelma on korjattu korjaustiedoston tässä versiossa.| Päivitä 1.2 | Kyllä | Ei |
| 9 | Verkon liityntäkohdat | Aiempien versioiden määritetty reititettävä ei Gatewayn StorSimple laitteen voi siirtyä offline-tilaan. Tässä versiossa reititys metrijärjestelmä tietojen 0 on tehty huonoiten; Vaikka muut verkon liityntäkohdat ovat cloud käytössä, kaikki cloud laitteesta liikenne reititetään vuoksi tietojen 0 kautta. | Päivitys 1 | Kyllä | Kyllä | 
| 10 | Varmuuskopioiden | Ohjelmavirhe päivitys 1, joka aiheuttaa varmuuskopioiden epäonnistuu 24 päivän jälkeen on korjattu päivityksen 1.1 korjaus-versiossa. | Päivitä 1.1 | Kyllä | Kyllä |
| 11 | Varmuuskopioiden | Cloud tilannevedoksia pienen muuta korvaukset ja huonosti tuloksena ohjelmavirhe aiemmissa versioissa. Tämä ohjelmavirhe on korjattu korjaustiedoston tässä versiossa.| Päivitä 1.2 | Kyllä | Kyllä |
| 12 | Päivitykset | Korjaustiedoston tässä versiossa on korjattu ohjelmavirhe Update-1, joka raportoidaan epäonnistui päivitys ja aiheuttaa ohjaimet voi siirtyä palautus-tilaan.| Päivitä 1.2 | Kyllä | Kyllä |


## <a name="known-issues-in-update-12"></a>Päivitä 1.2 tunnetut ongelmat

Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.

| Ei. | Toiminto | Ongelma | Kommenttien tai vaihtoehtoinen menetelmä | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Levyn quorum | Harvoin, jos EBOD kehyksen 8600: aa laitteen levyjen useimpia katkaistaan tuloksena on levy ole koontiresurssi sitten tallennustilan varanto on offline-tilassa. Se säilyy offline-tilassa, vaikka levyjen muodostetaan. | Tarvitset Käynnistä laite uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support for seuraavat vaiheet. | Kyllä | Ei |
| 2 | Virheellinen ohjaimen tunnus | Kun ohjauskoneen korvaa suoritetaan, controller 0 saattaa näkyä muodossa ohjauskoneen 1. Aikana ohjauskoneen korvaavan kun kuva on ladattu peer-solmu ohjaimen tunnus voidaan näyttää sisältökorttina aluksi peer ohjauskoneen ID-tunnuksellasi. Harvoin ongelman myös voi tarkastella järjestelmän uudelleenkäynnistyksen jälkeen. | Käyttäjän mitään toimia ei tarvita. Tässä tilanteessa ratkaisee itse ohjauskoneen korvaaminen päätyttyä. | Kyllä | Ei |
| 3 | Tallennustilan tilit | Tallennustilan-palvelun avulla voit poistaa tilin tallennustila on ei-tuettu skenaario. Tämä johtaa tilanteeseen, jossa käyttäjätietoja ei voi hakea. | Kyllä | Kyllä |
| 4 | Laitteen automaattisesti | Äänenvoimakkuuden säilön saman lähde-laitteesta eri kohde laitteet useita failovers ei tueta. Laitteen automaattisesti yhden toimimattoman laitteesta useilla eri laitteilla tekee aseman säilöjen epäonnistui kautta laitteen ensimmäisenä, menettävät tietojen omistajuus. Näiden automaattisesti, kun aseman näiden säilöjen näkyvät tai toimivat eri tavalla, kun niitä tarkastellaan Azure perinteinen-portaalissa. | | Kyllä | Ei |
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

## <a name="physical-device-updates-in-update-12"></a>Laitteen fyysinen päivitysten päivitys 1.2

Jos korjaus päivittää 1.2 käytetään fyysistä laitetta (käynnissä Update 1 versioissa), versio muuttuu 6.3.9600.17584.

## <a name="controller-and-firmware-updates-in-update-12"></a>Päivitä 1.2 ohjauskoneen ja laiteohjelmiston päivitykset

Tässä versiossa päivittää ohjaimen ja levyn laitteisto laitteessasi.
 
- Katso lisätietoja SAS ohjauskoneen päivityksestä, [Update 1: Microsoft Azure StorSimple laitteen LSI SAS-ohjainten](https://support.microsoft.com/kb/3043005). 

- Lisätietoja levyn laitteisto-ohjelmiston päivittäminen on artikkelissa [levyn laitteisto Update 1: n Microsoft Azure StorSimple laitteen](https://support.microsoft.com/kb/3063416).
 
## <a name="virtual-device-updates-in-update-12"></a>Päivitä 1.2 Virtual laitteen päivitykset

Tämä päivitys ei voi käyttää virtual laitteeseen. Uudet virtual laitteet on luotava. 

## <a name="next-steps"></a>Seuraavat vaiheet

- [Asenna päivitys 1.2 laitteessasi](storsimple-install-update-1.md).
 
