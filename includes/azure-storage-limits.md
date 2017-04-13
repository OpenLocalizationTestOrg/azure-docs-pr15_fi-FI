Resurssi|Oletusarvoinen enimmäismäärä
---|---
Tilauskohtaisten tilien tallennustilan määrä|200<sup>1</sup>
TT tallennustilaa tilin kohden|500 TT
Blob-objektien säilöjen, BLOB-objektit, tiedostoresurssit, taulukoita, olevien, kohteiden tai viestien määrä tallennustilan tilin enimmäismäärä|Vain rajoitus on 500 TT tallennustilaa tili
Yksittäisen blob-säilö, taulukossa tai jonon enimmäiskoko|500 TT
Max lohkot sisään block Blob-objektien määrä tai Blob-objektien liittäminen|50 000
Max eston sisään block Blob-objektien koon tai Blob-objektien liittäminen|4 MT
Max estä Blob-objektien koon tai Blob-objektien liittäminen|50 000 x 4 Megatavua (noin 195 gt) 
Sivun Blob-objektien, enimmäiskoko |1 TT:
Taulukon yksikön enimmäiskoko|1 MEGATAVU
Taulukon kohteen ominaisuuksien enimmäismäärä|252
Viestin jonossa enimmäiskoko|64 KILOTAVUA
Enimmäiskoko olevaa jaettua tiedostoresurssia.|5 TERATAVUA
Jaetun tiedoston tiedoston enimmäiskoko|1 TT:
Jaetun tiedoston tiedostojen enimmäismäärä|Vain rajoitus on 5 Teratavua kokonaiskapasiteetti jaettua tiedostoresurssia
Maks 8 kt IOPS Jaa kohden|1 000
Jaetun tiedoston tiedostojen enimmäismäärä|Vain rajoitus on 5 Teratavua kokonaiskapasiteetti jaettua tiedostoresurssia
Blob-objektien säilöjen, BLOB-objektit, tiedostoresurssit, taulukoita, olevien, kohteiden tai viestien määrä tallennustilan tilin enimmäismäärä|Vain rajoitus on 500 TT tallennustilaa tili
Tallennettujen käytäntöjen kohti säilö, jaettua tiedostoresurssia, taulukko tai jonon enimmäismäärä|5
Yhteensä pyytää korvauksen (olettaen että 1 kt objektien koon) tallennustilan tilin|Enintään 20 000 IOPS kohteiden sekunnissa tai viestit sekunnissa
Kohteen siirtonopeuden yksittäisen blob varten|Jopa 60 Mt toisen tai enintään 500 pyynnöt sekunnissa
Kohteen siirtonopeuden yhden jonon (1 kt viestit)|Enintään 2000 viestien sekunnissa
Kohteen siirtonopeuden yhden taulukon osion (1 kt yksiköt)|Enintään 2000 kohteiden sekunnissa
Yksitiedostoinen jaetun siirtonopeuden kohde|Jopa 60 Mt sekunnissa
Maks tunkeutumisen<sup>2</sup> tallennustilan tiliä (US alueet)|Jos GRS/ZRS<sup>3</sup> käytössä, saat LRS 20 Gbps 10 Gbps
Maks juniin<sup>2</sup> tallennustilan tiliä (US alueet)|Jos RA-GRS/GRS/ZRS<sup>3</sup> käytössä, saat LRS 30 Gbps 20 Gbps
Maks tunkeutumisen<sup>2</sup> tallennustilan tiliä (Euroopan ja Aasian alueilla)|Jos GRS/ZRS<sup>3</sup> käytössä, saat LRS 10 Gbps 5 Gbps
Maks juniin<sup>2</sup> tallennustilan tiliä (Euroopan ja Aasian alueilla)|Jos RA-GRS/GRS/ZRS<sup>3</sup> käytössä, saat LRS 15 Gbps 10 Gbps

<sup>1</sup> Tämä sisältää vakio- ja Premium tallennustilan tilit. Jos asetat yli 200 tallennustilan tilit, pyytävät [Azure](https://azure.microsoft.com/support/faq/)tukemalla. Azuren tallennustilaan ryhmän tarkistaa business tapaus ja voivat hyväksyä jopa 250 tallennustilan tilit. 

<sup>2</sup> *Tunkeutumisen* viittaa kaikki tiedot (pyyntöjen) on lähetetty tallennustilan-tilille. *Juniin* viittaa kaikki tiedot (vastaukset) vastaanoteta tallennustilan tilin.  

<sup>3</sup> Azure-tallennustilan Replikointiasetukset ovat:

- **RA GRS**: lukuoikeudet geo tarpeettomat tallennustilan. Jos RA GRS on käytössä, juniin kohteet toissijaisen sijainnin ovat samat kuin ensisijainen sijainti.
- **GRS**: Geo ylimääräinen. 
- **ZRS**: vyöhykkeen ylimääräinen. Käytettävissä vain estä BLOB-objektit. 
- **LRS**: paikallisesti ylimääräinen. 

