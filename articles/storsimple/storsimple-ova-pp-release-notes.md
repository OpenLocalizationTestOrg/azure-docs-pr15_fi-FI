<properties 
   pageTitle="StorSimple Virtual matriisin julkaisutiedot | Microsoft Azure"
   description="Kuvataan kriittinen Avaa ongelmat ja ratkaisut StorSimple Virtual matriisin."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/13/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-release-notes"></a>StorSimple Virtual matriisin julkaisutiedot

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot tunnistaa Avaa kriittisten Microsoft Azure StorSimple Virtual matriisin (tunnetaan myös nimellä StorSimple paikallisen virtual laitteen tai StorSimple virtual laite) maaliskuussa 2016 yleiseen käyttöön (GA)-versiossa. Tässä versiossa vastaa versio 10.0.10271.0.

Julkaisutiedot päivitetään jatkuvasti ja kriittisten virheiden edellyttävän Vaihtoehtoinen menetelmä löytyy, kun ne on lisätty. Ennen kuin otat StorSimple virtual laitteen, Tarkista huolellisesti julkaisutiedot tietoja. 

Seuraavassa taulukossa on yhteenveto tämän version tunnetut ongelmat.


| Ei. | Toiminto | Ongelma | Vaihtoehtoinen menetelmä/kommentit |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Päivitykset | Virtuaalinen laitteiden luotu preview-versio ei voi päivittää tuettuun yleiseen käyttöön. | Virtuaalinen seuraaviin laitteisiin on epäonnistui päälle julkaistujen tietojen palauttaminen (DR)-työnkulun avulla yleiseen käyttöön. |
| **2.** | Valmistellun tietojen levy | Kun tietojen DVD-levyllä tiettyjä määritetyn kokoisen on valmisteltu ja luonut vastaavan StorSimple virtual laitteen, sinun on ei laajenna tai Pienennä tietojen levy. Yritetään kiellä johtaa kaikkien paikallisen tasoa laitteen tietojen menettämisen. |   |
| **3.** | Ryhmäkäytäntö | Kun laite on toimialueeseen liittymistä, käyttämällä Ryhmäkäytäntö voi heikentää laitteen-toimintoa. | Varmista, että virtual matriisi on oma organisaatioyksikkö (OU) Active Directoryn eivätkä ole ryhmäkäytännön objekteja (Ryhmäkäytäntöobjekti) on käytetty.|
| **4.** | Paikallisen sivuston Käyttöliittymä | Jos laajennetut suojausominaisuudet on otettu käyttöön Internet Explorer (IE ESC), kuten vianmääritys tai ylläpito paikallisen sivuston Käyttöliittymä joillakin sivuilla eivät ehkä toimi oikein. Nämä sivut-painikkeita myös eivät ehkä toimi. | Poista käytöstä parannettuja suojausominaisuuksia Internet Explorerissa.|
| **5.** | Paikallisen sivuston Käyttöliittymä | Valitse Hyper-V virtual machine-Käyttöliittymän näkyvät 10 sivuston verkkoliittymät Gbps liittymät. | Tämä on heijastuksen Hyper-V. Hyper-V näkyy aina 10 Gbps virtual verkkosovittimien. |
| **6.** | Porrastettu asemat tai osakkeet | Tavualue lukitseminen sovellukset, jotka toimivat StorSimple Porrastettu tietomääristä ei tueta. Jos tavu alueen lukitus on käytössä, StorSimple tiering ei toimi. | Suositeltavat toimenpiteet ovat: <br></br>DBCS-alueen sovelluksen logiikkaa lukitseminen käytöstä.<br></br>Valitse tämän sovelluksen tietojen asettaminen paikallisesti kiinnitetty tietomääristä Porrastettu tietomääristä sijaan.<br></br>*Caveat*: Jos paikallisesti kiinnitetyt asemat ja tavu alueen lukitus on käytössä, Huomaa, että paikallisesti kiinnitetty äänenvoimakkuuden voidaan online jopa ennen kuin palautus on valmis. Tällaisissa tapauksissa Jos palautuksen on käynnissä, valitse Odota Palauta, jos haluat suorittaa. |
| **7.** | Porrastettu osakkeet | Suurten tiedostojen käsitteleminen saattaa johtaa hidas taso. | Kun käsittelet suurta tiedostoa, suosittelemme, että suurin tiedosto on pienempi kuin 3 % Jaa koosta. |
| **8.** | Käyttää osakkeet kapasiteetti | Voit nähdä jakaminen puuttuessa käytettyjen tietojen käsittelyssä Jaa. Tämä johtuu siitä osakkeet käytetyt kapasiteetti sisältää metatiedot. |   |
| **9.** | Tietojen palauttaminen | Voit tehdä vain samaa toimialuetta, lähde-laitteen tiedoston palvelimen palauttaminen. Toisen toimialueen kohde laitteeseen palauttaminen ei tueta tässä versiossa. | Tämä pantava täytäntöön myöhemmällä. |
| **10.** | Azure PowerShell | StorSimple virtual laitteet ei voi hallita tässä versiossa Azure-PowerShellin kautta. | Kaikki virtual laitteiden hallinta tulee tehdä Azure perinteinen portaalin ja paikallisen sivuston Käyttöliittymän kautta. |
| **11.** | Salasanan vaihtaminen | Virtuaalinen matriisin laitteen konsolin hyväksyy vain syötteen en-US näppäimistön muodossa. |   |
| **12.** | CHAP | Kun luonut CHAP tunnistetietoja ei voi poistaa. Lisäksi, jos muokkaat CHAP tunnistetiedot, sinun on asemat offline-tilaan ja tuo ne verkossa, jotta muutos tulee voimaan. | Nämä osoitetaan myöhemmällä. |
| **13.** | iSCSI-palvelin  | 'Käyttää tallennustilan' iSCSI-asema toissijaisille saattavat olla erilaiset StorSimple hallintapalvelu ja iSCSI-isäntä. | ISCSI-isäntä on tiedostojärjestelmän-näkymä.<br></br>Laitteen näkee lohkot varattuna, kun äänenvoimakkuuden oli enimmäiskoon.|
