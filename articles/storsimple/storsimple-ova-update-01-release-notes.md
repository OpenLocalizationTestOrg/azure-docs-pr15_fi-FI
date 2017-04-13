<properties 
   pageTitle="StorSimple Virtual matriisin päivitykset julkaisutiedot | Microsoft Azure"
   description="Kuvataan kriittinen Avaa ongelmat ja ratkaisut käynnissä 0,2 ja 0,1 päivitys StorSimple Virtual matriisin."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>StorSimple Virtual matriisin päivityksen 0,2 ja 0,1 julkaisutiedot

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot tunnistaa Avaa kriittisten ja ratkaista ongelmat Microsoft Azure StorSimple Virtual matriisin päivitykset. (Microsoft Azure StorSimple Virtual matriisi on tunnetaan myös nimellä StorSimple paikallisen virtual laitteen tai StorSimple virtual laitteen.) 

Julkaisutiedot päivitetään jatkuvasti ja kriittisten virheiden edellyttävän Vaihtoehtoinen menetelmä löytyy, kun ne on lisätty. Ennen kuin otat StorSimple virtual laitteen, Tarkista huolellisesti julkaisutiedot tietoja.

Päivitä 0,2 vastaa ohjelmiston versiota **10.0.10280.0**; Päivitys 0,1 on versio **10.0.10279.0**. Seuraavissa osissa luettelon jokaisen päivityksen muutokset. 

> [AZURE.NOTE] Päivitykset häiritsevä ja Käynnistä laite uudelleen. Jos i/o ovat käynnissä, laite maksamaan käyttökatkot.

## <a name="issues-fixed-in-the-update-02"></a>Päivityksen 0,2 ongelmat
0,2 päivityksessä on lisäksi seuraavassa taulukossa on kuvattu korjaus 0,1 Päivitä kaikki muutokset:

Toiminto                              | Ongelma                                                                                                                                                                                                                                                                                                                           |
--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
Päivitykset                                 | Edellisen version päivityksiä ei ole havaittu automaattisesti Azure perinteinen-portaalissa, niin oli paikallisen Web-Käyttöliittymän avulla voit asentaa päivitykset. Tämä ongelma on korjattu tässä versiossa. 0,2 päivityksen asentamisen jälkeen voit asentaa päivitykset Azure perinteinen-portaalissa.                       

## <a name="whats-new-in-the-update-01"></a>Päivitä 0,1 uudet ominaisuudet

Päivitä 0,1 sisältää seuraavat virheenkorjauksia ja parannukset. 

- **Improved vikasietoisuudelle varten cloud katkokset**: Tässä versiossa on useita virheenkorjauksia palauttaminen, varmuuskopiointi, palauttaminen ja jos cloud-yhteyksien häiriöt tiering ympärille. 

- **Improved palauttaa suorituskyvyn**: Tässä versiossa on virheenkorjauksia, joka on merkittävästi Leikkaa alaspäin palauttaminen työt valmistuminen-aika.

- **Automaattisen tilaa regeneroitaviksi optimointi**: kun tiedot on poistettu levinneet valmistellun asemat, käyttämättömät tallennustilan lohkot täytyy saadaan takaisin käyttöön. Tässä versiossa on parannettu tilaa regeneroitaviksi prosessin pilvestä, tuloksena on tulossa käyttöön nopeammin verrattuna aiempiin versioihin verrattuna käyttämätön tila.

- **Uusi virtual levyn kuvat**: uuden Näennäiskiintolevyn VHDX ja VMDK ovat nyt käytettävissä Azure perinteinen portaalin kautta. Voit ladata kuvien valmistelu uudet päivityksen 0,1 laitteet.

- **Töiden tila portaalissa tarkkuuden parantamiseksi**: ohjelmiston aiemmalla versiolla tilan raportointi-portaalissa ei ole hajautetun. Tämä ongelma on ratkennut tässä versiossa.

- **Toimialueen liity kohdata**: virheenkorjauksia liittyvät toimialueen liittymisestä ja laitteen nimeäminen uudelleen.


## <a name="issues-fixed-in-the-update-01"></a>Päivityksen 0,1 ongelmat

Seuraavassa taulukossa on yhteenveto korjauksista tässä versiossa.

| Ei.  | Toiminto                              | Ongelma                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | VMDK                                 | Jotkin VMware-versioissa OS levy on käytetty lyhyet ilmenneet ilmoitukset ja toiminnan tavanomainen toiminta. Tämä on vahvistettu tässä versiossa.                                                                                                                                                                                    |
| 2    | iSCSI-palvelin                         | Viimeisin versio käyttäjän on pakollinen voit määrittää kunkin käytössä verkko-liittymän StorSimple virtual laitteesi yhdyskäytävän. Tämä ongelma on muutettu tässä versiossa, niin, että käyttäjällä on vähintään yhden yhdyskäytävän varten käytössä verkkoliittymät määrittämiseen.                                                                              |
| 3    | Tuki-paketti                      | Ohjelmiston aiemmalla versiolla tukevat paketin sivustokokoelman epäonnistui, kun paketti koot on yli 1 Gigatavua. Tämä ongelma on korjattu tässä versiossa.                                                                                                                                                                               |
| 4    | Pilvikäytön                         |  Edellisen version, jos StorSimple Virtual taulukko ei ole verkkoyhteyttä ja on käynnistettävä uudelleen, paikallinen Käyttöliittymän on yhteysongelmat. Tämä ongelma on korjattu tässä versiossa.                                                                                                                            |
| 5    | Kaavioiden seuranta                    | Edellisen jälkeen laite-automaattisesti cloud kapasiteetin käyttö kaaviot näkyvät virheellisten arvojen Azure perinteinen-portaalissa. Tämä on kiinteä nykyisessä versiossa.                                                                                                                          |



## <a name="known-issues-in-the-update-01"></a>Päivitä 0,1 tunnetut ongelmat

Seuraavassa taulukossa yhteenvedon StorSimple Virtual matriisin tunnetut ongelmat ja sisältää release merkille aiempien versioiden ongelmat. **Ongelmat julkaisemisen merkille tässä versiossa on tähdellä. Lähes kaikki tässä luettelossa mainitut ongelmat on näkyvissä StorSimple Virtual matriisin GA-version.**


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
| **14.** | Tiedoston palvelimen *  | Jos kansiossa ei vaihtoehtoinen tietojen Stream (MAINOSTEN) liittyy, Active Directory ei varmuuskopioida tai palauttaa palauttaminen, Kloonaa ja kohteen tason palautus kautta.| |


## <a name="next-step"></a>Seuraava vaihe

[Asenna päivitykset](storsimple-ova-install-update-01.md) StorSimple Virtual matriisin.
