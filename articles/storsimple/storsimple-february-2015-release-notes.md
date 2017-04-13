<properties 
   pageTitle="StorSimple 8000 päivityksen 0,3 julkaisutiedot | Microsoft Azure"
   description="Tässä artikkelissa uusista ominaisuuksista ja korjaukset, avoimet seurantakohteet ja käytettävissä menetelmää helmikuussa 2015 julkaistut Microsoft Azure StorSimple release (Päivitä 0,3)."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>StorSimple 8000 sarjan päivitys 0,3 julkaisutiedot - helmikuussa 2015

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot tunnistaa Avaa kriittisten StorSimple 8000 sarjan päivityksen 0,3 helmikuussa 2015 julkaistu. Niissä on myös StorSimple ohjelmiston ja laiteohjelmiston päivityksiä, tämä julkaisu sisältää luettelon. Tämä on kolmas versio, kun StorSimple 8000 sarjan Release-versiossa on tehty yleisesti saatavilla heinäkuussa 2014.
  
Tämä päivitys ei muuta laitteen versio tammikuu-päivitys. Se säilyy 6.3.9600.17312-versio. Voit vahvistaa, että päivitys on asennettu valitsemalla **Viimeksi päivitetty** päivämäärä. Jos päivämäärä on 2/10/2015 tai uudempi versio, päivitys on asennettu onnistuneesti.  

Suosittelemme, että etsiä ja käyttää saatavilla olevat päivitykset heti, kun olet asentanut StorSimple laitteen. Voit myös poistaa Automaattiset päivitykset, lataa ja asenna Microsoft tärkeät päivitykset heti, kun niitä. Lisätietoja on artikkelissa [päivittää StorSimple laitteen](storsimple-update-device.md).  

Tarkista julkaisutiedot tietoja ennen niiden käyttöönottoa päivitys StorSimple ratkaisu.  

>[AZURE.IMPORTANT]   
>
> - Asenna helmikuussa päivitys ei ole Windows PowerShell StorSimple ja StorSimple hallintapalvelu avulla.   
> - Kestää noin tunnin asentaa tämä päivitys. Jos asennat kumulatiiviset päivitykset, voi kestää noin 3 tuntia suorittamiseen.  
> - StorSimple helmikuu versio ei sisällä päivityksiä StorSimple virtual laitteeseen. Voit silti käyttää mitä tahansa Windows-päivitysten virtual laitteeseen, kuten viimeisimmät tietoturvakorjauksia, mutta et näe versio virtual laitteen muutoksen.  

Varmista, että seuraavat edellytykset täyttyvät ennen päivitystä StorSimple laitteen.  

- Varmista, että laitteen molemmat ohjaimet ovat käytössä, ennen kuin voit tarkistaa päivitykset. Jos joko ohjain ei ole käynnissä, tarkistus epäonnistuu. Voit varmistaa, että ohjaimet ovat kunnossa-tilaan, siirry **ylläpito** -sivun kohdassa **Laitteiston tila** . Jos luettelossa on osat, **tarve huomiota**, ota yhteyttä Microsoft Support ennen kuin jatkat missään.
- Varmista, että kiinteä IP-osoitteet controller 0 ja 1 ohjauskoneen on reititettävä eivätkä voi muodostaa yhteys Internetiin, kun näitä käytetään ylläpidon laitteeseen päivitykset. [Testaa yhteys cmdlet-komennon](https://technet.microsoft.com/library/hh849808.aspx) avulla voit ping tunnetut osoite verkkoon, kuten Outlook.com-tilin, varmista, että ohjain on yhteys ulkopuolelta verkon ulkopuolella.
- Varmista, että portit 80 ja 443 ovat käytettävissä StorSimple laitteessasi, n lähtevän tietoliikenteen. Lisätietoja on artikkelissa [Verkko StorSimple laitteen koskevat vaatimukset](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
- Jos laitteen versio on vanhempi kuin 6.3.9600.17312 (lokakuussa 2014-päivitys), käytöstä tietojen 2 ja 3 tietojen portit, jos otettu käyttöön, ennen kuin aloitat päivityksen. Jätä tietojen 2 tai 3 tietojen portit käyttöön, kun päivitys voi aiheuttaa laitteen ohjaimen voi siirtyä palautustilaan. Huomaa, että verkkoliittymät poistetaan käytöstä, kun kaikki liittyvät asemat on otettava offline-tilassa ja i katketa päivityksen ajaksi.  
  
## <a name="whats-new-in-the-february-release"></a>Helmikuussa-version uudet ominaisuudet

Tämä päivitys sisältää korjaa tehdas Palauta ongelman, joka on tapahtunut laitteita, jotka oli päivitetty GA Vapauta lokakuussa 2014-versioon. Lisätietoja on artikkelissa [tässä versiossa on korjattu ongelmia](#issues-fixed-in-the-february-release).   

Tämä päivitys ei ole uusia ominaisuuksia tai toimintoja.  

## <a name="issues-fixed-in-the-february-release"></a>Helmikuussa versiossa korjauksista

Seuraavassa taulukossa on kuvattu, tämä päivitys korjaa ongelman.  
 
| Ei. | Toiminto | Ongelma | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|---------------------------------|-------------------------------|
| 1 | Factory palauttaminen | Yrität suorittaa factory, Palauta laitteessa, joka on alun perin oli asennettu GA-versio (versio 6.3.9600.17215), mutta on päivitetty lokakuun release (versio 6.3.9600.17312). Tehdas Palauta epäonnistuu ja laitteen muuttuu epävakaaksi. | Kyllä | Ei |


## <a name="known-issues-in-the-february-release"></a>Helmikuussa-version tunnetut ongelmat

Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.
 
| Ei. | Toiminto | Ongelma | Kommenttien tai vaihtoehtoinen menetelmä | Laitteen fyysinen koskee  | Koskee virtual laitteeseen |
|-----|---------|-------|----------------------------|-----------------------------|--------------------------|
| 1 | Factory palauttaminen | Tietyissä tilanteissa, kun suoritat factory Palauta-StorSimple laitteen voi olla jumissa ja Näytä tämä viesti: **Palauta factory on käynnissä (vaihe 8)**. Tämä tapahtuu, jos painat CTRL + C-cmdlet-komennolla ollessa käynnissä. | Jälkeen aloitetaan factory Palauta ei painamalla CTRL + C. Jos olet jo tässä tilassa, ota Microsoft Support for vaiheisiin. | Kyllä | Ei |
| 2 | Levyn quorum | Harvoin, jos 8600device EBOD-kehys-levyjen useimpia katkaistaan tuloksena on levy ole koontiresurssi sitten tallennustilan varanto on offline-tilassa. Se säilyy offline-tilassa, vaikka levyjen muodostetaan. | Tarvitset Käynnistä laite uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support for seuraavat vaiheet. | Kyllä | Ei |
| 3 | Cloud tilannevedoksen virheet | Harvoin cloud tilannevedoksen voi epäonnistua **enintään varmuuskopion enimmäismäärä on saavutettu**-virhe. Tämä tapahtuu, jos ylität 255 online kloonit samassa laitteessa, sama alkuperäisen asemasta, joka on poistettu. |  | Kyllä | Kyllä |
| 4 | Virheellinen ohjaimen tunnus | Kun ohjauskoneen korvaa suoritetaan, controller 0 saattaa näkyä muodossa ohjauskoneen 1. Aikana ohjauskoneen korvaavan kun kuva on ladattu peer-solmu ohjaimen tunnus voidaan näyttää sisältökorttina aluksi peer ohjauskoneen ID-tunnuksellasi. Harvoin ongelman myös voi tarkastella järjestelmän uudelleenkäynnistyksen jälkeen. | Käyttäjän mitään toimia ei tarvita. Tässä tilanteessa ratkaisee itse ohjauskoneen korvaaminen päätyttyä. | Kyllä | Ei |
| 5 | Laitteen kaavioiden seuranta | Laitteen seuranta-kaaviot eivät toimi, kun Basic StorSimple hallintapalvelu tai välityspalvelimen kokoonpanoa laitteen NTLM-todennus on käytössä. | Muokkaa WWW-välityspalvelimen määritykset laitteen rekisteröityjä StorSimple hallintapalvelu niin, että todennus on määritetty ei mitään. Voit tehdä tämän suorittamalla StorSimple Set-HcsWebProxy cmdlet-komennon Windows PowerShell. | Kyllä | Kyllä |
| 6 | Tallennustilan tilit | Tallennustilan-palvelun avulla voit poistaa tilin tallennustila on ei-tuettu skenaario. Tämä johtaa tilanteeseen, jossa käyttäjätietoja ei voi hakea. |  | Kyllä | Kyllä |
| 7 | Laitteen automaattisesti | Äänenvoimakkuuden säilön saman lähde-laitteesta eri kohde laitteet useita failovers ei tueta.  Yksittäisen toimimattoman laitteesta automaattisesti useilla eri laitteilla tekee aseman säilöjen epäonnistui kautta laitteen ensimmäisenä, menettävät tietojen omistajuus. Näiden automaattisesti, kun aseman näiden säilöjen näkyvät tai toimivat eri tavalla, kun niitä tarkastellaan Azure perinteinen-portaalissa. |   | Kyllä | Ei |
| 8 | Asennus | StorSimple sovittimen SharePoint-asennuksen aikana, sinun täytyy antaa laitteen IP, asenna onnistu järjestyksessä. |  | Kyllä | Ei |
| 9 | Web-välityspalvelin | Jos WWW-välityspalvelimen määritykset on määritetty protokollaksi HTTPS-device Servicen viestintä vaikuttaako ja laitteen siirtyvät offline-tilassa. Tuki pakettien luodaan myös merkittäviä resursseja laitteen aikana. | Varmista, että web välityspalvelimen URL-osoite on määritetty protokollaksi HTTP. Lisätietoja siitä, miten voit [määrittäminen web-välityspalvelimen laitteeseesi](storsimple-configure-web-proxy.md). | Kyllä | Ei |
| 10 | Web-välityspalvelin | Jos määrittäminen käyttöön web-välityspalvelimen rekisteröidyn laitteen, täytyy aktiivinen ohjauskoneen laitteen uudelleen. |  | Kyllä | Ei |
| 11 | Suuri cloud viive ja suuri i/o työmäärää | Kun StorSimple laitteen kohtaa yhdistelmä erittäin suuri cloud viiveitä (järjestyksen sekuntia) ja suuri i/o työmäärää, laite tietomääristä Siirry eivät toimi oikein vaiheeseen ja i voi epäonnistua "laite ei ole valmis"-virhe. | Tarvitset manuaalisesti uudelleen laitteen ohjaimet tai suorittaa laitteen automaattisesti, jos haluat korjata tilanteen. | Kyllä | Ei |

## <a name="physical-device-updates-in-the-february-release"></a>Laitteen fyysinen päivitykset näkyvät helmikuun vapauttaminen

Tämä päivitys korjaa factory Palauta ongelman, joka on tapahtunut laitteita, jotka oli päivitetty GA Vapauta lokakuussa 2014-versioon. Se ei sisällä muita päivityksiä StorSimple laitteeseen.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-february-release"></a>Sarja liitetty SCSI (SAS)-ohjain ja laitteisto-päivitykset näkyvät helmikuun vapauttaminen

Tässä versiossa ei sisällä päivityksiä sarja liitetty SCSI (SAS)-ohjain tai laitteisto. Ohjaimen päivitys on lokakuussa 2014 versiossa.  

## <a name="virtual-device-updates-in-the-february-release"></a>Virtuaalinen laitteen päivitykset näkyvät helmikuun vapauttaminen

Tässä versiossa ei sisällä päivityksiä virtual laitteen. Tämän asentaminen ei muutu virtual laitteen ohjelmiston version.
 
