<properties 
   pageTitle="StorSimple Virtual matriisin päivitykset julkaisutiedot | Microsoft Azure"
   description="Kuvataan kriittinen Avaa ongelmat ja ratkaisut käynnissä päivityksen 0,3 StorSimple Virtual matriisin."
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
   ms.date="09/15/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-update-03-release-notes"></a>StorSimple Virtual matriisin päivityksen 0,3 julkaisutiedot

## <a name="overview"></a>Yleiskatsaus

Seuraavat julkaisutiedot tunnistaa Avaa kriittisten ja ratkaista ongelmat Microsoft Azure StorSimple Virtual matriisin päivitykset.

Julkaisutiedot päivitetään jatkuvasti ja kriittisten virheiden edellyttävän Vaihtoehtoinen menetelmä löytyy, kun ne on lisätty. Ennen kuin otat StorSimple Virtual matriisin, Tarkista huolellisesti julkaisutiedot tietoja.

Päivitä 0,3 vastaa ohjelmiston versiota **10.0.10288.0**.

> [AZURE.NOTE] Päivitykset häiritsevä ja Käynnistä laite uudelleen. Jos i/o ovat käynnissä, laite veloitetaan käyttökatkot.


## <a name="whats-new-in-the-update-03"></a>Päivitä-0,3 uudet ominaisuudet

Päivitys 0,3 on ensisijaisesti ohjelmavirhe korjaus muodosta. Tässä versiossa on korjattu useita virheet, tuloksena on varmuuskopion virheitä aiemmilla versioilla.

## <a name="issues-fixed-in-the-update-03"></a>Päivityksen 0,3 ongelmat

Seuraavassa taulukossa on yhteenveto korjauksista tässä versiossa.

| Ei.  | Toiminto                              | Ongelma                                                                                                                                                                                                                                                                                                                           |
|------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1    | Varmuuskopioiden                                |Ongelma on nähdä missä varmuuskopioista epäonnistuu suoritettava jaetun tiedoston aiemman version. Jos tämä ongelma ilmeni varmuuskopiointityön epäonnistuu ja tärkeä ilmoitus on korotettuna ilmoittamaan käyttäjälle StorSimple hallinta-palvelusta. Tämä ongelma vaikuta osakkeet tai tietoihin pääsy tietoihin. Pääkansio syynä on tunnistetaan ja kiinteä tässä versiossa. <br></br> Korjaus ei koske taannehtivasti jaettuja resursseja, jotka ovat jo näe ongelman. Asiakkaat, jotka näkevät tämän ongelman pitäisi ensin päivityksen 0,3 ja valitse Ota yhteyttä Microsoftin Support koko järjestelmän varmuuskopiointiin voit korjata ongelman. Sen sijaan, että yhteydessä Microsoft Support, asiakkaat myös palauttaa uuden Jaa kunnossa varmuuskopiosta tarvittavien osalta.                                                                                                                                                                                 |
| 2    | iSCSI                         | Ongelma on nähdä aiemmassa versiossa, jossa asemat katoavat näkyvistä kopioitaessa tietoja StorSimple Virtual matriisin osioon. Tämä ongelma on korjattu tässä versiossa. <br></br> Korjaukset otetaan käyttöön vain vastaluotu asemat. Korjaukset eivät vaikuta takautuvasti asemat, jotka ovat jo näe ongelman. Asiakkaita kyseessä olevaan tietomääristä verkossa Azure perinteinen portaalin kautta, varmuuskopiointi asemat, ja palauttaa sitten asemat uusia.                                                               |


## <a name="known-issues-in-the-update-03"></a>Päivitä 0,3 tunnetut ongelmat

Seuraavassa taulukossa yhteenvedon StorSimple Virtual matriisin tunnetut ongelmat ja sisältää release merkille aiempien versioiden ongelmat. 


| Ei. | Toiminto | Ongelma | Vaihtoehtoinen menetelmä/kommentit |
|-----|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **1.** | Päivitykset | Virtuaalinen laitteiden luotu preview-versio ei voi päivittää tuettuun yleiseen käyttöön. | Virtuaalinen seuraaviin laitteisiin on epäonnistui päälle julkaistujen tietojen palauttaminen (DR)-työnkulun avulla yleiseen käyttöön. |
| **2.** | Valmistellun tietojen levy | Kun tietojen DVD-levyllä tiettyjä määritetyn kokoisen on valmisteltu ja luonut vastaavan StorSimple virtual laitteen, sinun on ei laajenna tai Pienennä tietojen levy. Yritetään tehtävä tulokset menettää kaikki paikallisen tasoa laitteen tiedot. |   |
| **3.** | Ryhmäkäytäntö | Kun laite on toimialueeseen liittymistä, käyttämällä Ryhmäkäytäntö voi heikentää laitteen-toimintoa. | Varmista, että virtual matriisi on oma organisaatioyksikkö (OU) Active Directoryn eivätkä ole ryhmäkäytännön objekteja (Ryhmäkäytäntöobjekti) on käytetty.|
| **4.** | Paikallisen sivuston Käyttöliittymä | Jos laajennetut suojausominaisuudet on otettu käyttöön Internet Explorer (IE ESC), kuten vianmääritys tai ylläpito paikallisen sivuston Käyttöliittymä joillakin sivuilla eivät ehkä toimi oikein. Nämä sivut-painikkeita myös eivät ehkä toimi. | Poista käytöstä parannettuja suojausominaisuuksia Internet Explorerissa.|
| **5.** | Paikallisen sivuston Käyttöliittymä | Valitse Hyper-V virtual machine-Käyttöliittymän näkyvät 10 sivuston verkkoliittymät Gbps liittymät. | Tämä on heijastuksen Hyper-V. Hyper-V näkyy aina 10 Gbps virtual verkkosovittimien. |
| **6.** | Porrastettu asemat tai osakkeet | Tavualue lukitseminen sovellukset, jotka toimivat StorSimple Porrastettu tietomääristä ei tueta. Jos tavu alueen lukitus on otettu käyttöön, StorSimple tiering ei toimi. | Suositeltavat toimenpiteet ovat: <br></br>DBCS-alueen sovelluksen logiikkaa lukitseminen käytöstä.<br></br>Valitse tämän sovelluksen tietojen asettaminen paikallisesti kiinnitetty tietomääristä Porrastettu tietomääristä sijaan.<br></br>*Caveat*: kun paikallisesti kiinnitetyt asemat ja tavu alueen lukitus on käytössä, paikallisesti kiinnitetty äänenvoimakkuuden voi olla online jopa ennen kuin palautus on valmis. Tällaisissa tapauksissa Jos palautuksen on käynnissä, valitse Odota Palauta, jos haluat suorittaa. |
| **7.** | Porrastettu osakkeet | Suurten tiedostojen käsitteleminen saattaa johtaa hidas taso. | Kun käsittelet suurta tiedostoa, suosittelemme, että suurin tiedosto on pienempi kuin 3 % Jaa koosta. |
| **8.** | Käyttää osakkeet kapasiteetti | Voit nähdä jakaminen kulutus, jos sijaintitietoja ei ole resurssiin. Tämä johtuu siitä osakkeet käytetyt kapasiteetti sisältää metatiedot. |   |
| **9.** | Tietojen palauttaminen | Voit tehdä vain samaa toimialuetta, lähde-laitteen tiedoston palvelimen palauttaminen. Toisen toimialueen kohde laitteeseen palauttaminen ei tueta tässä versiossa. | Tämä on toteutettu uudempi versio. |
| **10.** | Azure PowerShell | StorSimple virtual laitteet ei voi hallita tässä versiossa Azure-PowerShellin kautta. | Kaikki virtual laitteiden hallinta tulee tehdä Azure perinteinen portaalin ja paikallisen sivuston Käyttöliittymän kautta. |
| **11.** | Salasanan vaihtaminen | Virtuaalinen matriisin laitteen konsolin hyväksyy vain syötteen en-US näppäimistön muodossa. |   |
| **12.** | CHAP | Kun luonut CHAP tunnistetietoja ei voi poistaa. Lisäksi voit muokata CHAP tunnistetiedot, jos haluat asemat offline-tilaan ja tuo ne verkossa, jotta muutos tulee voimaan. | Tämä ongelma on osoitettu myöhemmällä. |
| **13.** | iSCSI-palvelin  | 'Käyttää tallennustilan' iSCSI-asema toissijaisille saattavat olla erilaiset StorSimple hallintapalvelu ja iSCSI-isäntä. | ISCSI-isäntä on tiedostojärjestelmän-näkymä.<br></br>Laitteen näkee lohkot varattuna, kun äänenvoimakkuuden oli enimmäiskoon.|
| **14.** | Tiedostopalvelimeen  | Jos kansiossa ei vaihtoehtoinen tietojen Stream (MAINOSTEN) liittyy, Active Directory ei varmuuskopioida tai palauttaa palauttaminen, Kloonaa ja kohteen tason palautus kautta.| |


## <a name="next-step"></a>Seuraava vaihe

[Asenna päivitys 0,3](storsimple-ova-install-update-01.md) StorSimple Virtual matriisin.

## <a name="references"></a>Viittaukset

Etsitkö vanhempi versio-Huomautus? Mennä: 

- [StorSimple Virtual matriisin päivityksen 0,1 ja 0,2 julkaisutiedot](storsimple-ova-update-01-release-notes.md)

- [StorSimple Virtual matriisin Yleiset julkistamista muistiinpanot](storsimple-ova-pp-release-notes.md)
 

