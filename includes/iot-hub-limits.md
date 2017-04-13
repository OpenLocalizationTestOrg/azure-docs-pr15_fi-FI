Seuraavassa taulukossa on lueteltu eri palvelutasot (S1, S2, S3, F1) liittyviä rajoituksia. Tietoja kunkin tason kunkin *yksikön* kustannus on artikkelissa [IoT keskittimeen hinnat](https://azure.microsoft.com/pricing/details/iot-hub/).

| Resurssi | S1 vakio | S2 vakio | S3 vakio | F1 vapaa |
| -------- | ----------- | ----------- | ----------- | ------- |
| Viestien/päivä | 400,000 | 6,000,000   | 300,000,000 | 8 000   |
| Enimmäiskäyttöaste | 200    | 200         | 200         | 1       |

> [AZURE.NOTE] Jos arvelet, että yli 200 yksiköt käyttäminen S1 tai S2 tai S3 taso-keskittimeen, ota yhteyttä Microsoftin tuotetukeen.

Seuraavassa taulukossa on lueteltu, jotka koskevat IoT keskittimeen resurssien rajoitukset:

| Resurssi | Raja |
| -------- | ----- |
| Suurin maksettu IoT keskittimet Azure tilausta kohti | 10 |
| Suurin sallittu vapaa IoT-keskittimet Azure tilausta kohti | 1 |
| Laitteen käyttäjätietojen enimmäismäärä<br/>  funktio palauttaa yhden puhelussa | 1 000 |
| Laitteen cloud viestien IoT keskittimeen viestin suurin säilytys | 7 päivää |
| Laitteen cloud viestin enimmäiskoon | 256 KT |
| Laitteen cloud Erän enimmäiskoko | 256 KT |
| Viestien enimmäismäärä laitteen cloud osalta | 500 |
| Cloud laitteen viestin enimmäiskoon | 64 KILOTAVUA |
| Suurin TTL cloud laitteen valitseminen | kaksi päivää |
| Cloud laitteen suurin toimituksen määrä <br/> viestit | 100 |
| Suurin toimituksen palautetta viestien määrä <br/> cloud laitteen viestin saatuaan | 100 |
| Suurin TTL palautetta viestit-kohdassa <br/> vastaus pilvessä laitteen viestiin | kaksi päivää |

> [AZURE.NOTE] Jos sinun on yli 10 maksettu IoT-keskittimet-Azure tilauksen, ota yhteyttä Microsoftin tuotetukeen.

IoT keskittimeen palvelun throttles pyynnöt, kun seuraavat kiintiö on ylitetty:

| Rajoituksen | Toiminto arvo |
| -------- | ------------- |
| Käyttäjätietojen rekisterin toiminnot <br/> (luominen, hakea, luettelon, päivittäminen ja poistaminen), <br/> yksittäisten tai bulk Tuo/Vie | 5000/min/yksikkö (S3) <br/> 100/min/yksikkö (S1 ja S2). |
| Laitteen yhteydet | 6000/s/yksikkö (S3) 120/s/yksikkö (S2), 12/s/yksikkö (S1). <br/>Vähintään 100/sec. |
| Lähettää pilvipalveluun laite | 6000/sec/yksikkö (S3) 120/sec/yksikkö (S2), 12/sec/yksikkö (S1). <br/>Vähintään 100/sec. |
| Cloud laitteen lähettää | 5000/min/yksikkö (S3) 100/min/yksikkö (S1 ja S2). |
| Cloud laite vastaanottaa | 50000/min/yksikkö (S3) 1000/min/yksikkö (S1 ja S2). |
| Lataa tiedostotoiminnot | 5000 tiedoston lataaminen ilmoitukset/min/yksikkö (S3), 100 tiedoston lataamisen ilmoitukset/min/yksikkö (S1 ja S2). <br/> 10000 SAS URI voi olla ulos Azure-tallennustilan tilin samalla kertaa.<br/> 10 SAS URI/laitteen voi olla ulos samalla kertaa. |
