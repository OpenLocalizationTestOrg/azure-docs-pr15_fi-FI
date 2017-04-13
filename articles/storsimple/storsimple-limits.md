<properties 
   pageTitle="StorSimple järjestelmän rajoitukset | Microsoft Azure"
   description="Tässä artikkelissa kuvataan järjestelmän rajoitukset ja suositellut koot StorSimple 8000 sarjan osat ja yhteydet."
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
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="storsimple-system-limits"></a>StorSimple järjestelmän rajoitukset

## <a name="overview"></a>Yleiskatsaus

StorSimple tallennustila skaalattava ja joustavassa oman palvelinkeskukseen. On kuitenkin joitakin rajoituksia, jotka tulee ottaa huomioon, kun suunnittelu ja käyttöönotto toimivat StorSimple ratkaisu. Seuraavassa taulukossa kuvataan nämä raja-arvot ja sisältää joitakin suosituksia niin, että voit käyttää tehokkaasti StorSimple ratkaisu.

| Raja-tunniste | Raja | Kommentit |
|----------------- | ------|--------- |
| Tallennustilan tilin tunnistetiedot enimmäismäärä | 64 | |
| Äänenvoimakkuuden säilöjen enimmäismäärä | 64 | |
| Asemat enimmäismäärä | 255 | |
| Paikallisesti kiinnitetty tietomääristä enimmäismäärä | 32 | |
| Enimmäismäärä aikatauluja kaistanleveyden mallia kohden | 168 | Aikataulun tunnissa, joka päivä viikon (24 * 7). |
| Porrastettu aseman fyysinen laitteissa enimmäiskoko | 64 TT 8100 ja 8600 | 8100 ja 8600 ovat fyysisiä laitteita. |
| Porrastettu aseman Azure virtual laitteilla enimmäiskoko | 30 TT 8010 varten <br></br> 64 TT 8020 varten | 8010 ja 8020 ovat Azure virtual laitteita, jotka käyttävät vakio tallennustilan ja Premium-tallennustilan. |
| Enimmäiskoko paikallisesti kiinnitetty aseman fyysinen laitteissa | 8,5 TT 8100 varten <br></br> 22.5 TT 8600 varten | 8100 ja 8600 ovat fyysisiä laitteita. |
| ISCSI yhteyksien enimmäismäärä | 512 | |
| Enimmäismäärä laitteistokäynnistäjät iSCSI yhteydet | 512 | |
| Access-ohjausobjektin tietueiden laite enimmäismäärä | 64 | |
| Enimmäismäärä tietomääristä varmuuskopion käytännön kohden | 24 | |
| Varmuuskopioiden säily kohden (varmuuskopion käytäntö) aikataulun enimmäismäärä | 64 | |
| Enimmäismäärä aikatauluja varmuuskopion käytännön kohden | 10 | |
| Enimmäismäärä tilannevedoksia tyypin, joka voidaan tarvita kohti | 256 | Numerossa paikallisen tilannevedoksia ja cloud tilannevedoksia. |
| Enimmäismäärä tilannevedoksia, joka voi olla käytettävissä kaikissa laitteissa | 10 000 | |
| Enimmäismäärä, joka voidaan käsitellä rinnakkain varmuuskopion, palauttaminen tai Kloonaa tietomääristä | 16 |<ul><li>Jos luettelossa on yli 16 asemat, ne käsitellään peräkkäin, kun käsittely paikkojen ovat käytettävissä.</li><li>Uusi varmuuskopioita kloonatun tai palautetulle Porrastettu määrän ei toteudu, ennen kuin toiminto on valmis. Kuitenkin paikallisen aseman varmuuskopioiden sallitaan jälkeen äänenvoimakkuuden on online-tilassa.</li></ul>|
| Palauttaa ja Kloonaa Porrastettu tietomääristä ajan palauttaminen | < 2 minuuttia | <ul><li>Äänenvoimakkuuden otetaan käyttöön, palauttaminen tai Kloonaa toiminnan riippumatta Asemakoko kahden minuutin kuluessa.</li><li>Äänenvoimakkuus-suorituskyky saattaa olla alun perin hitaammin Normaali, kuten useimmat tiedot ja metatiedot silti pilveen. Suorituskyky voi lisätä kuin tietojen kulkee pilvestä StorSimple laitteeseen.</li><li>Lataa metatietojen kokonaisaika määräytyy kohdistettu Asemakoko. Metatietojen tuodaan automaattisesti laitteen 5 Teratavua kohdistettu aseman tietojen minuutit korko taustalla. Tämä kurssi voi vaikuttaa Internet-kaistanleveyden pilveen.</li><li>Palauta tai Kloonaustoiminto on valmis, kun kaikki metatiedot on laitteeseen.</li><li>Varmuuskopion toimintoja ei voi suorittaa ennen palauttamista tai Kloonaustoiminto on täysin valmis.|
| Palauttaa Palauta aika paikallisesti kiinnitetty tietomääristä | < 2 minuuttia | <ul><li>Äänenvoimakkuuden on saatavilla palautustoiminto riippumatta Asemakoko kahden minuutin kuluessa.</li><li>Äänenvoimakkuus-suorituskyky saattaa olla alun perin hitaammin Normaali, kuten useimmat tiedot ja metatiedot silti pilveen. Suorituskyky voi lisätä kuin tietojen kulkee pilvestä StorSimple laitteeseen.</li><li>Lataa metatietojen kokonaisaika määräytyy kohdistettu Asemakoko. Metatietojen tuodaan automaattisesti laitteen 5 Teratavua kohdistettu aseman tietojen minuutit korko taustalla. Tämä kurssi voi vaikuttaa Internet-kaistanleveyden pilveen.</li><li>Toisin kuin Porrastettu tietomääristä paikallisesti kiinnitetty levyasemien aseman tiedot myös ladataan paikallisesti laitteeseen. Palautus on valmis, kun kaikki aseman tiedot on tuotu laitteeseen.</li><li>Palauta toimintojen saattaa olla pitkä. Viimeistele Palauta kokonaisaika riippuu kokoa valmistellun paikallisen aseman, Internet-kaistanleveys ja laitteeseen olemassa olevia tietoja. Varmuuskopion toimintoja paikallisesti kiinnitetty aseman sallita, kun palautus on käynnissä.|
|Cloud tilannevedoksia kurssia käsittely| 15 minuutin/TT | <ul><li>Pienin aika tehdä pilveen tilannevedoksen valmiina ladattavaksi TT: n kohdistettu aseman tietojen varmuuskopioinnin kohden. </li><li> Yhteensä cloud tilannevedoksen aika lasketaan lisäämällä tilannevedoksen Lataa aika, jota vaikuta cloud Internet-kaistanleveyden tällä hetkellä.
| Asiakkaan suurin luku-/ kirjoitusoikeudet siirtonopeuden (kun Suoritettaessa taso-palvelimella) * | 920/720 Mt/s yksittäisen 10 GbE verkon käyttöliittymän | Enintään 2 x MPIO kanssa ja kaksi verkon liittymää. |
| Asiakkaan suurin luku-/ kirjoitusoikeudet siirtonopeuden (kun Kiintolevy taso-palvelimella) * | 120/250 Mt/s |
| Asiakkaan suurin luku/kirjoitus siirtonopeuden (kun cloud taso-palvelimella)* Update 3 tai uudempaa versiota** | 40/60 Mt/s, tasoisen tietomääristä<br><br>60/80 Mt/s, tasoisen tietomääristä jonka arkistointia valittuna aseman luonnin aikana | Lue siirtonopeuden määräytyy asiakkaat luodaan ja riittävästi i/o jonon pituus ylläpitämiseen. <br><br>Saavuttaa nopeus määräytyy käytetyn pohjana olevan tallennustilan tilin nopeuden. | 

& #42; Suurin nopeus kohti tyyppi on mitattu 100 prosenttiin luku ja kirjoita 100 prosenttiin skenaarioita. Todellinen nopeus voi heiketä ja i/o määräytyy sekoitetaan ja verkon ehdot.

& #42; & #42; Suorituskyvyn numerot ennen päivitystä 3 voi heiketä.

## <a name="next-steps"></a>Seuraavat vaiheet

Tarkista [StorSimple järjestelmävaatimukset](storsimple-system-requirements.md). 
