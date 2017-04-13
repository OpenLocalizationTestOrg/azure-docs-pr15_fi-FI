<properties 
   pageTitle="StorSimple 8000 päivityksen 0,2 julkaisutiedot | Microsoft Azure"
   description="Kuvaa uusista ominaisuuksista ja korjaukset, avoimet seurantakohteet ja käytettävissä menetelmää tammikuussa 2015 Microsoft Azure StorSimple release (Päivitä 0,2)."
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
   ms.date="05/16/2016"
   ms.author="v-sharos" />


# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>StorSimple 8000 sarjan päivitys 0,2 julkaisutiedot - tammikuussa 2015

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot tunnistaa Avaa kriittisten Microsoft Azure StorSimple tammikuussa 2015-versiossa. Niissä on myös StorSimple ohjelmiston ja laiteohjelmiston päivityksiä, tämä julkaisu sisältää luettelon. Tämä on toinen versio, kun StorSimple 8000 sarjan Release-versiossa on tehty yleisesti saatavilla heinäkuussa 2014.
  
Tämä päivitys ei muutu laitteen fyysinen versio lokakuussa päivitys. Se säilyy 6.3.9600.17312-versio. Tässä versiossa on muutettu virtual laitteen kuva kuva. Tämän vuoksi kaikki uudet virtual laitteet luotu jälkeen 1/20/2015 näkyy versio 6.3.9600.17361.  

Tarkista päivityksen tammikuussa 2015 julkaisutiedot seuraavia tietoja.

> [AZURE.IMPORTANT]  
>
>- Tämä päivitys ei ole käytettävissä Windows Updaten kautta, eikä sitä voi asentaa samoin kuin muut päivitykset. Laitteen saa tämän päivityksen myös silloin, kun olet asentanut päivitykset Azure perinteinen-portaalissa. Tämä päivitys koskevat vain luotu jälkeen 20. tammikuussa 2015 virtual laitteet. 
> 
>- StorSimple tammikuu-versiossa ei sisällä päivityksiä StorSimple fyysinen laitteeseen. Voit silti käyttää mitä tahansa Windows-päivitysten virtual laitteeseen, kuten viimeisimmät tietoturvakorjauksia, mutta et näe StorSimple fyysinen laite versio muutoksen.

## <a name="whats-new-in-the-january-release"></a>Tammikuu-version uudet ominaisuudet

Tämä päivitys sisältää liittyvät siirtymistä offline-tilaan virtual laitteeseen tietomääristä korjaa. (Katso [tässä versiossa on korjattu ongelmia](#issues-fixed-in-the-january-release)).  

Päivitys ei ole uusia ominaisuuksia tai toimintoja.  

## <a name="issues-fixed-in-the-january-release"></a>Tammikuu-versiossa korjauksista

Seuraavassa taulukossa on kuvattu, tämä päivitys korjaa ongelman.

|Ei.|Toiminto|Ongelma|Laitteen fyysinen koskee|Koskee virtual laitteeseen 
|---|-------|-----|--------------------------|-------------------------
|1|Asemat siirtymistä offline-tilaan|Kun suuri cloud viiveitä jatkuvat muutaman minuutin ajan, StorSimple virtual laitteen tietomääristä go offline isännät. Korjaa kasvaa cloud viiveet suurempia siten pienentämisen tilanteissa, joka voi aiheuttaa siirryt offline-tilaan isännät tietomääristä raja-arvon.|Ei|Kyllä  

## <a name="known-issues-in-the-january-release"></a>Tammikuu-version tunnetut ongelmat

Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.
 
|Ei.|Toiminto|Ongelma|Kommenttien tai vaihtoehtoinen menetelmä|Laitteen fyysinen koskee|Koskee virtual laitteeseen 
|---|-------|-----|-------------------|--------------------------|-------------------------
|1| Factory palauttaminen|  Tietyissä tilanteissa, kun suoritat factory Palauta-StorSimple laitteen voi olla jumissa ja Näytä tämä viesti: **factory Palauta on käynnissä (vaihe 8).** Tämä tapahtuu, jos painat CTRL + C-cmdlet-komennolla ollessa käynnissä.| Jälkeen aloitetaan factory Palauta ei painamalla CTRL + C. Jos olet jo tässä tilassa, ota Microsoft Support for vaiheisiin.|Kyllä|Ei|
|2|Levyn quorum| Harvoin, jos EBOD kehyksen 8600: aa laitteen levyjen useimpia katkaistaan tuloksena on levy ole koontiresurssi sitten tallennustilan varanto on offline-tilassa. Se säilyy offline-tilassa, vaikka levyjen muodostetaan.|Tarvitset Käynnistä laite uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support for seuraavat vaiheet.|Kyllä |Ei
|3|Cloud tilannevedoksen virheet|Harvoin cloud tilannevedoksen voi epäonnistua **enintään varmuuskopion enimmäismäärä on saavutettu**-virhe. Tämä tapahtuu, jos ylität 255 online kloonit samassa laitteessa, sama alkuperäisen asemasta, joka on poistettu.||Kyllä|Kyllä
|4|Virheellinen ohjaimen tunnus|Kun ohjauskoneen korvaa suoritetaan, controller 0 saattaa näkyä muodossa ohjauskoneen 1. Aikana ohjauskoneen korvaavan kun kuva on ladattu peer-solmu ohjaimen tunnus voidaan näyttää sisältökorttina aluksi peer ohjauskoneen ID-tunnuksellasi.  Harvoin ongelman myös voi tarkastella järjestelmän uudelleenkäynnistyksen jälkeen.|Käyttäjän mitään toimia ei tarvita. Tässä tilanteessa ratkaisee itse ohjauskoneen korvaaminen päätyttyä.|Kyllä|Ei 
|5| Laitteen kaavioiden seuranta|Laitteen seuranta-kaaviot eivät toimi, kun Basic StorSimple hallintapalvelu tai välityspalvelimen kokoonpanoa laitteen NTLM-todennus on käytössä.|Muokkaa WWW-välityspalvelimen määritykset laitteen rekisteröityjä StorSimple hallintapalvelu niin, että todennus on määritetty ei mitään. Voit tehdä tämän suorittamalla StorSimple Set-HcsWebProxy cmdlet-komennon Windows PowerShell.|Kyllä|Kyllä
|6| Tallennustilan tilit|Tallennustilan-palvelun avulla voit poistaa tilin tallennustila on ei-tuettu skenaario. Tämä johtaa tilanteeseen, jossa käyttäjätietoja ei voi hakea.|| Kyllä |  Kyllä
|7|Laitteen automaattisesti| Äänenvoimakkuuden säilön saman lähde-laitteesta eri kohde laitteet useita failovers ei tueta.| Yksittäisen toimimattoman laitteesta automaattisesti useilla eri laitteilla tekee aseman säilöjen epäonnistui kautta laitteen ensimmäisenä, menettävät tietojen omistajuus. Näiden automaattisesti, kun aseman näiden säilöjen näkyvät tai toimivat eri tavalla, kun niitä tarkastellaan Azure perinteinen-portaalissa.|Kyllä|Ei
|8| Asennus|StorSimple sovittimen SharePoint-asennuksen aikana, sinun täytyy antaa laitteen IP, asenna onnistu järjestyksessä.||Kyllä|Ei
|9| Web-välityspalvelin|Jos WWW-välityspalvelimen määritykset on määritetty protokollaksi HTTPS-device Servicen viestintä vaikuttaako ja laitteen siirtyvät offline-tilassa. Tuki pakettien luodaan myös merkittäviä resursseja laitteen aikana.|Varmista, että web välityspalvelimen URL-osoite on määritetty protokollaksi HTTP. Lue lisätietoja siitä, miten voit [määrittäminen web-välityspalvelimen laitteeseesi](storsimple-configure-web-proxy.md).|Kyllä |Ei
|10|Web-välityspalvelin|  Jos määrittäminen käyttöön web-välityspalvelimen rekisteröidyn laitteen, täytyy aktiivinen ohjauskoneen laitteen uudelleen.|| Kyllä |Ei
|11|Suuri cloud viive ja suuri i/o työmäärää|Kun StorSimple laitteen kohtaa yhdistelmä erittäin suuri cloud viiveitä (järjestyksen sekuntia) ja suuri i/o työmäärää, laite tietomääristä Siirry eivät toimi oikein vaiheeseen ja i voi epäonnistua "laite ei ole valmis"-virhe.|Tarvitset manuaalisesti uudelleen laitteen ohjaimet tai suorittaa laitteen automaattisesti, jos haluat korjata tilanteen.|Kyllä|Ei

## <a name="physical-device-updates-in-the-january-release"></a>Laitteen fyysinen päivitykset tammikuussa vapauttaminen

Tämä päivitys ei sisällä muita muutoksia StorSimple laitteeseen.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-the-january-release"></a>Sarja liitetty SCSI (SAS)-ohjain ja laiteohjelmiston päivityksiä tammikuussa vapauttaminen

Tässä versiossa ei sisällä päivityksiä sarja liitetty SCSI (SAS)-ohjain tai laitteisto. Ohjaimen päivitys on lokakuussa 2014 versiossa. 

## <a name="virtual-device-updates-in-the-january-release"></a>Tammikuun Virtual laitteen päivitysten vapauttaminen

Tässä versiossa on päivitetty kuvan virtual laitteen. Kaikki luotu jälkeen 20. tammikuussa 2015 virtual laitteet näkyy versio 6.3.9600.17361.

 
