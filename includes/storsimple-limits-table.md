<!--author=alkohli last changed: 12/15/15-->

| Raja-tunniste | Raja | Kommentit |
|----------------- | ------|--------- |
| Tallennustilan tilin tunnistetiedot enimmäismäärä | 64 | |
| Äänenvoimakkuuden säilöjen enimmäismäärä | 64 | |
| Asemat enimmäismäärä | 255 | |
| Enimmäismäärä aikatauluja kaistanleveyden mallia kohden | 168 | Aikataulun tunnissa, joka päivä viikon (24 * 7). |
| Porrastettu aseman fyysinen laitteissa enimmäiskoko | 64 TT 8100 ja 8600 | 8100 ja 8600 ovat fyysisiä laitteita. |
| Porrastettu aseman Azure virtual laitteilla enimmäiskoko | 30 TT 8010 varten <br></br> 64 TT 8020 varten | 8010 ja 8020 ovat Azure virtual laitteita, jotka käyttävät vakio tallennustilan ja Premium-tallennustilan. |
| Enimmäiskoko paikallisesti kiinnitetty aseman fyysinen laitteissa | 9 TT 8100 varten <br></br> 24 TT 8600 varten | 8100 ja 8600 ovat fyysisiä laitteita. |
| ISCSI yhteyksien enimmäismäärä | 512 | |
| Enimmäismäärä laitteistokäynnistäjät iSCSI yhteydet | 512 | |
| Access-ohjausobjektin tietueiden laite enimmäismäärä | 64 | |
| Enimmäismäärä tietomääristä varmuuskopion käytännön kohden | 24 | |
| Varmuuskopioiden säily kohti varmuuskopion käytännön enimmäismäärä | 64 | |
| Enimmäismäärä aikatauluja varmuuskopion käytännön kohden | 10 | |
| Enimmäismäärä tilannevedoksia tyypin, joka voidaan tarvita kohti | 256 | Tämä vaihtoehto sisältää paikallisen tilannevedoksia ja cloud tilannevedoksia. |
| Enimmäismäärä tilannevedoksia, joka voi olla käytettävissä kaikissa laitteissa | 10 000 | |
| Enimmäismäärä, joka voidaan käsitellä rinnakkain varmuuskopion, palauttaminen tai Kloonaa tietomääristä | 16 |<ul><li>Jos määritettynä on yli 16 asemat, ne käsitellään järjestyksessä, kun käsittely paikkojen ovat käytettävissä.</li><li>Uusi varmuuskopioita kloonatun tai palautetulle Porrastettu määrän ei toteudu, ennen kuin toiminto on valmis. Kuitenkin paikallisen aseman varmuuskopioiden sallitaan jälkeen äänenvoimakkuuden on online-tilassa.</li></ul>|
| Palauttaa ja Kloonaa Porrastettu tietomääristä ajan palauttaminen | < 2 minuuttia | <ul><li>Äänenvoimakkuuden otetaan käyttöön, palauttaminen tai Kloonaa toiminnan riippumatta Asemakoko kahden minuutin kuluessa.</li><li>Äänenvoimakkuus-suorituskyky saattaa olla alun perin hitaammin Normaali, kuten useimmat tiedot ja metatiedot silti pilveen. Suorituskyky voi lisätä kuin tietojen kulkee pilvestä StorSimple laitteeseen.</li><li>Lataa metatietojen kokonaisaika määräytyy kohdistettu Asemakoko. Metatietojen tuodaan automaattisesti laitteen 5 Teratavua kohdistettu aseman tietojen minuutit korko taustalla. Tämä kurssi voi vaikuttaa Internet-kaistanleveyden pilveen.</li><li>Palauta tai Kloonaustoiminto on valmis, kun kaikki metatiedot on laitteeseen.</li><li>Varmuuskopion toimintoja ei voi suorittaa ennen palauttamista tai Kloonaustoiminto on täysin valmis.|
| Palauttaa Palauta aika paikallisesti kiinnitetty tietomääristä | < 2 minuuttia | <ul><li>Äänenvoimakkuuden on saatavilla palautustoiminto riippumatta Asemakoko kahden minuutin kuluessa.</li><li>Äänenvoimakkuus-suorituskyky saattaa olla alun perin hitaammin Normaali, kuten useimmat tiedot ja metatiedot silti pilveen. Suorituskyky voi lisätä kuin tietojen kulkee pilvestä StorSimple laitteeseen.</li><li>Lataa metatietojen kokonaisaika määräytyy kohdistettu Asemakoko. Metatietojen tuodaan automaattisesti laitteen 5 Teratavua kohdistettu aseman tietojen minuutit korko taustalla. Tämä kurssi voi vaikuttaa Internet-kaistanleveyden pilveen.</li><li>Toisin kuin Porrastettu tietomääristä kyseessä paikallisesti kiinnitetty tietomääristä aseman tiedot myös ladataan paikallisesti laitteeseen. Palautus on valmis, kun kaikki aseman tiedot on tuotu laitteeseen.</li><li>Palauta-toiminnot voivat olla pitkiä ja palauttamisen suorittamiseksi kokonaisaika riippuu kokoa valmistellun paikallisen aseman, Internet-kaistanleveys ja laitteeseen olemassa olevia tietoja. Varmuuskopion toimintoja paikallisesti kiinnitetty aseman sallita, kun palautus on käynnissä.|
| Ohut palauttaminen käytettävyys | Viimeksi automaattisesti | |
| Asiakkaan suurin luku-/ kirjoitusoikeudet siirtonopeuden (kun Suoritettaessa taso-palvelimella) * | 920/720 Mt/s yksittäisen 10GbE verkko-käyttöliittymä | Enintään 2 x MPIO kanssa ja kaksi verkon liittymää. |
| Asiakkaan suurin luku-/ kirjoitusoikeudet siirtonopeuden (kun Kiintolevy taso-palvelimella) * | 120/250 Mt/s |
| Asiakkaan suurin luku-/ kirjoitusoikeudet siirtonopeuden (kun cloud taso-palvelimella) * | 11/41 Mt/s | Lue siirtonopeuden määräytyy asiakkaat luodaan ja riittävästi i/o jonon pituus ylläpitämiseen. |

& #42; Suurin nopeus kohti tyyppi on mitattu 100 prosenttiin luku ja kirjoita 100 prosenttiin skenaarioita. Todellinen nopeus voi heiketä ja i/o määräytyy sekoitetaan ja verkon ehdot.
