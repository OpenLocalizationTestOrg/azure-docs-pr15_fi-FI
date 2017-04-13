<properties 
    pageTitle="StorSimple 8000 päivityksen 0,1 julkaisutiedot | Microsoft Azure"
    description="Tässä artikkelissa uusista ominaisuuksista ja korjaukset, avoimet seurantakohteet ja käytettävissä menetelmää lokakuussa 2014 Microsoft Azure StorSimple release (Päivitä 0,1)."
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
    ms.date="09/21/2016"
    ms.author="alkohli" />

# <a name="storsimple-8000-series-update-01-release-notes--october-2014"></a>StorSimple 8000 sarjan päivitys 0,1 julkaisutiedot – lokakuussa 2014  

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot tunnistaa Avaa kriittisten StorSimple 8000 sarjan päivityksen 0,1 julkaistu lokakuussa 2014. Niissä on myös StorSimple ohjelmiston ja laiteohjelmiston päivityksiä, tämä julkaisu sisältää luettelon. Tämä on ensimmäinen versio, kun StorSimple 8000 sarjan Vapauta-versio on saatavilla yleensä heinäkuussa 2014 ja vastaa versio 6.3.9600.17312.  

Suosittelemme, että etsiä ja käyttää saatavilla olevat päivitykset heti, kun olet asentanut laitteen. Voit myös poistaa Automaattiset päivitykset, lataa ja asenna Microsoft tärkeät päivitykset heti, kun niitä. Lisätietoja on artikkelissa [StorSimple laitteen](storsimple-update-device.md)päivittämisestä.  

Tarkista julkaisutiedot tietoja ennen niiden käyttöönottoa päivitykset StorSimple ratkaisu.  

>[AZURE.IMPORTANT]
> 
-   Lokakuussa-päivitysten asentaminen ei ole Windows PowerShell StorSimple ja StorSimple hallinnan avulla.  
-   Yleensä päivitykset kestää noin 3 tuntia suorittamiseen.  
-   Lokakuussa StorSimple ei sisällä päivityksiä StorSimple virtual laitteeseen. Voit edelleen käyttää tarvittavat käytettävissä Windows-päivitykset, kuten viimeisimmät tietoturvakorjauksia, mutta et näe versio virtual laitteen muutoksen.  

Varmista, että seuraavat edellytykset täyttyvät ennen päivitystä StorSimple laitteen.  

- Varmista, että laitteen molemmat ohjaimet ovat käytössä, ennen kuin voit tarkistaa päivitykset. Jos joko ohjain ei ole käynnissä, tarkistus epäonnistuu. Voit varmistaa, että ohjaimet ovat kunnossa-tilaan, siirry **ylläpito** -sivun kohdassa **Laitteiston tila** . Jos luettelossa on osat, **tarve huomiota**, ota yhteyttä Microsoft Support ennen kuin jatkat missään.  
- Varmista, että kiinteä IP-osoitteet, sekä Controller 0 ja ohjauskoneen 1 on reititettävä eivätkä voi muodostaa yhteys Internetiin, kun näitä käytetään ylläpidon laitteeseen päivitykset. [Testaa yhteys cmdlet-komennon](https://technet.microsoft.com/library/hh849808.aspx) avulla voit ping tunnetut osoite, kuten Outlook.comin, tarkista, että ohjain on yhteys ulkopuolelta verkko verkon ulkopuolella.  
- Varmista, että tarvittavat lähtevä portit ovat käytettävissä StorSimple laitteessasi, n lähtevän tietoliikenteen. Lisätietoja on artikkelissa [Verkko StorSimple laitteen koskevat vaatimukset](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).  
- Jos laitteen versio on vanhempi kuin 6.3.9600.17312 (lokakuussa 2014-päivitys), käytöstä tietojen 2 ja 3 tietojen portit, jos otettu käyttöön, ennen kuin aloitat päivityksen. Jos jätät tietojen 2 tai 3 tietojen portit käyttöön, kun päivittämistä, se voi aiheuttaa laitteen ohjaimen voi siirtyä palautustilaan. Huomaa, että verkkoliittymät poistetaan käytöstä, kun kaikki liittyvät asemat on otettava offline-tilassa ja i/o katketa päivityksen ajaksi.  

## <a name="whats-new-in-the-october-release"></a>Mitä uusia ominaisuuksia lokakuussa

Tämä päivitys sisältää seuraavat parannukset:

- Voit nyt hallita laitteen ohjaimet StorSimple hallintapalvelu UI. Hallinnan toiminnot ovat Ota ohjaimen tai uudelleen, Sammuta. Jos haluat lisätietoja, siirry [hallinta StorSimple laitteen ohjaimet](storsimple-manage-device-controller.md).  
- Voit ajoittaa WAN kaistanleveyden kohdistus-ja-viikonpäivä-ja päivän ajan mukaan. Voit paremmin hyödyntävät WAN kaistanleveyden myöhemmin. Eri kaistanleveyden mallien valitseminen eri asema säiliöiden osalta. Jos haluat lisätietoja, siirry [StorSimple kaistanleveyden-mallien hallinta](storsimple-manage-bandwidth-templates.md).  
- Voit määrittää sähköposti-ilmoitusten ilmoittamaan, kun muutoksista Järjestelmänvalvojat ja muiden olemassa oleva vai mahdollisesti tulevat ongelmat. Jos haluat lisätietoja, siirry [ilmoitusten asetusten määrittäminen](storsimple-manage-alerts.md#configure-alert-settings).  

## <a name="issues-fixed-in-the-october-release"></a>Lokakuussa korjaa ongelmat


Seuraavassa taulukossa on yhteenveto tämän päivityksen korjatut ongelmat.  

| Ei. | Toiminto | Ongelma | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|---------------------------------|--------------------------------|
| 1 | Verkon liityntäkohdat | Valitse edellisen verkkoliittymät tieto 2 ja tieto 3 on vaihtaa paikkaa ohjelmiston. Tämä on korjattu päivitys. Poista asetukset käytöstä ja poistaminen käytöstä nämä verkon liityntäkohdat, ennen kuin asennat päivityksen. Päivityksen asentamisen jälkeen joudut määrittämään Nämä liityntäkohdat. | Kyllä | Ei |
| 2 | Tuki-paketti | Edellisessä versiossa, jos suoritit Windows PowerShellin **Vie HcsSupportPackage** cmdlet-komento noutaa Baseboard hallinta ohjauskoneen (BMC)-lokit toiminto epäonnistui, seuraavan varoituksen: "toiminto onnistui tämän ohjaimen, mutta ei voitu peer ohjaimen seuraavien virheiden vuoksi. Tarkista, onko vertaisjärjestelmä kunnossa ja onko nykyisen solmun voit muodostaa yhteyden vertaisjärjestelmä." Tämä ongelma on korjattu. | Kyllä | Ei |
| 3 | Laitteen automaattisesti | Valitse edellisen oli tietojen ristiriidan mahdollisuutta jos **Tutustu varmuuskopiointi** työ epäonnistui aikana laitteen automaattisesti. Tämä ongelma on korjattu. | Kyllä | Ei |
| 4 | Laitteen automaattisesti | Edellisen jälkeen laite-automaattisesti varmuuskopiot olivat näkyvissä mutta liittyvän aseman säilö ei ollut kohde-laitteessa. Tämä ongelma on korjattu. | Kyllä | Ei |
| 5 | Laitteen automaattisesti | Valitse edellisen oli ohjelmavirhe cloud varmuuskopioiden luettelointi, joka voi aiheuttaa tietojen ristiriidan Jos oli cloud yhteysongelmat rekisteri palauttamista toiminnon aikana. | Kyllä | Ei |
| 6 | Laitteisto-päivitys | Edellisen työn laitteen laitteisto-päivitystä ei voitu ja näyttää virheen, jotka ovat ilmoittaneet Cmdlet-komentoja ei ole tunnistettu ja päivittäminen epäonnistui tuloksena. Ohjaimen tapahtui sitten palautus-tilaan. Tämä ongelma on korjattu. | Kyllä | Ei |
| 7 | Asennus | Laite ei ole kuvallinen oikein asennuksen aikana virheet on nyt korjattu. | Kyllä | Ei |
| 8 | Factory palauttaminen | Voit nyt myös ohittaa factory Palauta laitteisto Tarkista. Tämä on muuttunut edellisen version. | Kyllä | Ei |
| 9 | Factory palauttaminen | Edellisessä versiossa, kun factory Palauta cmdlet-komento on suoritettu, laitteisto versio tarkistukset tehtiin vain laitteisto-osia. Muita laitteisto tarkistus on tehty prosessin, joka saattaa aiheuttaa Palauta epäonnistuu ensimmäisen uudelleenkäynnistyksen jälkeen. Tämä korjaus varmistaa, että kaikki laitteisto-tarkistukset tehdään, kun tehdas Palauta cmdlet suoritetaan ja ennen ensimmäisen järjestelmän uudelleen. | Kyllä | Ei |
| 10 | Tallennustilan tilin avaimen kierto | Kierrä tallennustilan tilin näppäimet nyt käyttää **Käynnistä HcsmServiceDataEncryptionKeyChange** cmdlet-komento, ja kirjoita palvelun tiedot salausavain käyttäjää. Tämä on muuttunut edellisen version, jossa palvelun tiedot salausavaimen annettiin tekstiin-parametrin. | Kyllä | Ei |
| 11 | 24 tunnin kuluessa tuntisesta | Tietojen palauttaminen aikana uudelleenjärjestäminen lähde-laitteeseen ei suoriteta puhtaasti määritetty vastaamaan, aiheuttavan tuntisesta epäonnistuu. Tämä on korjattu tässä versiossa. | Kyllä | Ei |

## <a name="known-issues-in-the-october-release"></a>Lokakuussa tunnetut ongelmat

Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.

| Ei. | Toiminto | Ongelma | Kommenttien tai vaihtoehtoinen menetelmä | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory palauttaminen | Tietyissä tilanteissa, kun suoritat factory Palauta-StorSimple laitteen voi olla jumissa ja Näytä tämä viesti: **Palauta factory on käynnissä (vaihe 8)**. Tämä tapahtuu, jos painat CTRL + C-cmdlet-komennolla ollessa käynnissä. | Jälkeen aloitetaan factory Palauta ei painamalla CTRL + C. Jos olet jo tässä tilassa, ota Microsoft Support for vaiheisiin. | Kyllä | Ei |
| 2 | Factory palauttaminen | Tee ei factory Palauta StorSimple laitteella, joka päivitetään GA lokakuussa 2014 vapauttamista. | Tämä toiminto toimii vain korjaustiedoston asennustilan. Ota yhteyttä Microsoftin Support saat tarvittavat Tämä korjaus. | Kyllä | Ei | 
| 3 | Levyn quorum | Harvoin, jos EBOD kehyksen 8600: aa laitteen levyjen useimpia katkaistaan tuloksena on levy ole koontiresurssi sitten tallennustilan varanto on offline-tilassa. Se säilyy offline-tilassa, vaikka levyjen muodostetaan. | Tarvitset Käynnistä laite uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support for seuraavat vaiheet. | Kyllä | Ei |
| 4 | Cloud tilannevedoksen virheet | Harvoin cloud tilannevedoksen voi epäonnistua **enintään varmuuskopion enimmäismäärä on saavutettu**-virhe. Tämä tapahtuu, jos ylität 255 online kloonit samassa laitteessa, sama alkuperäisen asemasta, joka on poistettu. | | Kyllä | Kyllä |
| 5 | Virheellinen ohjaimen tunnus | Kun ohjauskoneen korvaa suoritetaan, controller 0 saattaa näkyä muodossa ohjauskoneen 1. Aikana ohjauskoneen korvaavan kun kuva on ladattu peer-solmu ohjaimen tunnus voidaan näyttää sisältökorttina aluksi peer ohjauskoneen ID-tunnuksellasi. Harvoin ongelman myös voi tarkastella järjestelmän uudelleenkäynnistyksen jälkeen. |Käyttäjän mitään toimia ei tarvita. Tässä tilanteessa ratkaisee itse ohjauskoneen korvaaminen päätyttyä. | Kyllä | Ei |
| 6 | Laitteen kaavioiden seuranta | Laitteen seuranta-kaaviot eivät toimi, kun Basic StorSimple hallintapalvelu tai välityspalvelimen kokoonpanoa laitteen NTLM-todennus on käytössä. | Muokkaa WWW-välityspalvelimen määritykset laitteen rekisteröityjä StorSimple hallintapalvelu niin, että todennus on määritetty ei mitään. Voit tehdä tämän suorittamalla StorSimple Set-HcsWebProxy cmdlet-komennon Windows PowerShell. | Kyllä | Kyllä |
| 7 | Tallennustilan tilit | Tallennustilan-palvelun avulla voit poistaa tilin tallennustila on ei-tuettu skenaario. Tämä johtaa tilanteeseen, jossa käyttäjätietoja ei voi hakea. | | Kyllä | Kyllä |
| 8 | Laitteen automaattisesti | Äänenvoimakkuuden säilön saman lähde-laitteesta eri kohde laitteet useita failovers ei tueta. | Yksittäisen toimimattoman laitteesta automaattisesti useilla eri laitteilla tekee aseman säilöjen epäonnistui kautta laitteen ensimmäisenä, menettävät tietojen omistajuus. Näiden automaattisesti, kun aseman näiden säilöjen näkyvät tai toimivat eri tavalla, kun niitä tarkastellaan Azure perinteinen-portaalissa. | Kyllä | Ei |
| 9 | Asennus | StorSimple sovittimen SharePoint-asennuksen aikana, sinun täytyy antaa laitteen IP, asenna onnistu järjestyksessä.    | | Kyllä | Ei |
| 10 | Web-välityspalvelin | Jos web-välityspalvelimen määritykset on määritetty protokollaksi HTTPS-device Servicen viestintä vaikuttaako ja laitteen siirtyvät offline-tilassa. Tuki pakettien luodaan myös merkittäviä resursseja laitteen aikana. | Varmista, että web välityspalvelimen URL-osoite on määritetty protokollaksi HTTP. Lisätietoja siitä, miten voit [määrittäminen web-välityspalvelimen laitteeseesi](storsimple-configure-web-proxy.md). | Kyllä | Ei |
| 11 | Web-välityspalvelin | Jos määrittäminen käyttöön web-välityspalvelimen rekisteröidyn laitteen, täytyy aktiivinen ohjauskoneen laitteen uudelleen. | | Kyllä | Ei |
| 12 | Suuri cloud viive ja suuri i/o työmäärää | Kun StorSimple laitteen kohtaa yhdistelmä erittäin suuri cloud viiveitä (järjestyksen sekuntia) ja suuri i/o työmäärää, laite tietomääristä Siirry eivät toimi oikein vaiheeseen ja i voi epäonnistua "laite ei ole valmis"-virhe. | Tarvitset manuaalisesti uudelleen laitteen ohjaimet tai suorittaa laitteen automaattisesti, jos haluat korjata tilanteen. | Kyllä | Ei |

## <a name="physical-device-updates-in-the-october-release"></a>Laitteen fyysinen päivitysten lokakuun vapauttaminen

Kun päivitykset on otettu käyttöön fyysinen laitteeseen, versio muuttuu 6.3.9600.17312. Ellei toisin, nämä julkaisutiedot koskevat StorSimple laitteen kaikki mallit. Saat lisätietoja näiden päivitysten [lokakuussa 2014 laitteen fyysinen ohjelmiston päivittäminen Microsoft Azure StorSimple laitteen](http://support.microsoft.com/kb/2986997).  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-october-release"></a>Sarja liitetty SCSI (SAS)-ohjain ja laitteisto-päivitykset näkyvät lokakuun vapauttaminen

Tässä versiossa päivittää ohjaimen ja laitteen fyysinen SAS-ohjaimen laitteisto. Lisätietoja SAS ohjauskoneen päivittäminen on artikkelissa [lokakuussa 2014 päivittäminen Microsoft Azure StorSimple laitteen LSI SAS-ohjainten](http://support.microsoft.com/kb/2987020).   

Tässä versiossa koskee myös kumulatiivinen laitteisto-päivityksen, joka korjaa laitteen laitteistosta luotettavuutta liittyvät ongelmat. Saat lisätietoja laitteisto-päivitys [päivittää Microsoft Azure StorSimple laitteen lokakuussa 2014 laitteisto](http://support.microsoft.com/kb/2987015).  

## <a name="virtual-device-updates-in-the-october-release"></a>Lokakuun Virtual laitteen päivitysten vapauttaminen

Tässä versiossa ei sisällä päivityksiä virtual laitteen. Tämän asentaminen ei muutu virtual laitteen ohjelmiston version.
 
