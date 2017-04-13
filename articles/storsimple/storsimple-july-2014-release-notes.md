<properties 
   pageTitle="StorSimple 8000 Vapauta version julkaisutiedot | Microsoft Azure"
   description="Kuvaa uusista ominaisuuksista, avoimet seurantakohteet ja käytettävissä menetelmää heinäkuussa 2014 julkaistut Microsoft Azure StorSimple-versiossa."
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

# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>StorSimple 8000 sarjan Vapauta Version julkaisutiedot - heinäkuussa 2014 

## <a name="overview"></a>Yleiskatsaus

Seuraa julkaisutiedot tunnistaa Avaa kriittisten StorSimple 8000 sarjan heinäkuussa 2014 Microsoft Azure StorSimple yleiseen käyttöön (GA)-versiossa. Tässä versiossa vastaa versio 6.3.9600.17215.  

Ellei toisin, nämä julkaisutiedot koskevat StorSimple laitteen kaikki mallit. Julkaisutiedot päivitetään jatkuvasti; Kun kriittisten virheiden edellyttävän Vaihtoehtoinen menetelmä löytyy, ne on lisätty. Ennen kuin otat käyttöön Microsoft Azure StorSimple-ratkaisun, ota huomioon seuraavat tiedot.  

## <a name="known-issues-in-this-release"></a>Tämän version tunnetut ongelmat
Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.  
 
| Ei. | Toiminto | Ongelma | Kommenttien tai vaihtoehtoinen menetelmä | Laitteen fyysinen koskee | Koskee virtual laitteeseen |
|-----|---------|-------|----------------------------|----------------------------|---------------------------|
| 1 | Factory palauttaminen | Tietyissä tilanteissa, kun suoritat factory Palauta-StorSimple laitteen voi olla jumissa ja Näytä tämä viesti: **Palauta factory on käynnissä (vaihe 8)**. Tämä tapahtuu, jos painat CTRL + C-cmdlet-komennolla ollessa käynnissä. | Jälkeen aloitetaan factory Palauta ei painamalla CTRL + C. Jos olet jo tässä tilassa, ota Microsoft Support for vaiheisiin. | Kyllä | Ei |
| 2 | Levyn quorum | Harvoin, jos EBOD kehyksen 8600: aa laitteen levyjen useimpia katkaistaan tuloksena on levy ole koontiresurssi sitten tallennustilan varanto on offline-tilassa. Se säilyy offline-tilassa, vaikka levyjen muodostetaan. | Tarvitset Käynnistä laite uudelleen. Jos ongelma jatkuu, ota yhteyttä Microsoft Support for seuraavat vaiheet. | Kyllä | Ei |
| 3 | Cloud tilannevedoksen virheet | Harvoin cloud tilannevedoksen voi epäonnistua **enintään varmuuskopion enimmäismäärä on saavutettu**-virhe. Tämä tapahtuu, jos ylität 255 online kloonit samassa laitteessa, sama alkuperäisen asemasta, joka on poistettu. | | Kyllä | Kyllä |
| 4 | Virheellinen ohjaimen tunnus | Kun ohjauskoneen korvaa suoritetaan, controller 0 saattaa näkyä muodossa ohjauskoneen 1. Aikana ohjauskoneen korvaavan kun kuva on ladattu peer-solmu ohjaimen tunnus voidaan näyttää sisältökorttina aluksi peer ohjauskoneen ID-tunnuksellasi. Harvoin ongelman myös voi tarkastella järjestelmän uudelleenkäynnistyksen jälkeen. | Käyttäjän mitään toimia ei tarvita. Tässä tilanteessa ratkaisee itse ohjauskoneen korvaaminen päätyttyä. | Kyllä | Ei |
| 5 | Laitteen kaavioiden seuranta | Laitteen seuranta-kaaviot eivät toimi, kun Basic StorSimple hallintapalvelu tai välityspalvelimen kokoonpanoa laitteen NTLM-todennus on käytössä. | Muokkaa WWW-välityspalvelimen määritykset laitteen rekisteröityjä StorSimple hallintapalvelu niin, että todennus on määritetty ei mitään. Voit tehdä tämän suorittamalla StorSimple Set-HcsWebProxy cmdlet-komennon Windows PowerShell. | Kyllä | Kyllä |
| 6 | Tallennustilan tilit | Tallennustilan-palvelun avulla voit poistaa tilin tallennustila on ei-tuettu skenaario. Tämä johtaa tilanteeseen, jossa käyttäjätietoja ei voi hakea. | | Kyllä | Kyllä |
| 7 | Tuntisesta | Tuntisesta palauttaminen (DR) 24 tunnin kuluessa ei tueta. | | Kyllä | Ei |
| 8 | Laitteen automaattisesti | Äänenvoimakkuuden säilön saman lähde-laitteesta eri kohde laitteet useita failovers ei tueta. Yksittäisen toimimattoman laitteesta automaattisesti useilla eri laitteilla tekee aseman säilöjen epäonnistui kautta laitteen ensimmäisenä, menettävät tietojen omistajuus. Näiden automaattisesti, kun aseman näiden säilöjen näkyvät tai toimivat eri tavalla, kun niitä tarkastellaan Azure perinteinen-portaalissa. | | Kyllä | Ei |
| 9 | Asennus | StorSimple sovittimen SharePoint-asennuksen aikana, sinun täytyy laitteen IP, asennus onnistu voi säätää. | | Kyllä | Ei |
| 10 | Verkon liityntäkohdat | Verkkoliittymät tietojen 2 ja 3 tiedot on vaihtaa paikkaa ohjelmiston. | Jos haluat määrittää nämä liityntäkohdat, ota yhteyttä Microsoft Support. | Kyllä | Ei |


 
