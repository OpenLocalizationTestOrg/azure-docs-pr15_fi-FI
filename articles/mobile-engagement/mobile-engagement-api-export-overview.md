<properties
    pageTitle="Mobiili välitys Vie API yleiskatsaus"
    description="Perustietoja hyödyntää oman työkalujen laitteiden käyttäjän luoma raaka tietojen vieminen"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="kpiteira"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile"
    ms.date="04/26/2016"
    ms.author="kpiteira"/>

# <a name="mobile-engagement-export-api-overview"></a>Mobiili välitys Vie API yleiskatsaus

## <a name="introduction"></a>Johdanto

Tässä asiakirjassa kerrotaan perustietoja raaka tietojen hyödyntää oman työkalujen laitteiden käyttäjän luoma.

## <a name="pre-requisites"></a>Vaatimukset

Perustietoja vieminen Mobile välitys tarvitaan:

- Voi käyttää API (katso [todennus Manuaalinen määritys](mobile-engagement-api-authentication-manual.md))-API käyttöoikeuksien määrittäminen
- REST API tai [.net SDK-paketissa](mobile-engagement-dotnet-sdk-service-api.md)
- Azure-tallennustilan tilin.

>[AZURE.NOTE] Myös kannattaa erinomainen [Microsoft Azure-tallennustilan Explorer](http://storageexplorer.com/)vähintään suunnitteluvaiheessa, kun se on helppokäyttöinen Käyttöliittymän käyttäminen Azure-tallennustilan.

## <a name="what-can-be-exported"></a>Mitä voi viedä?

Mobile välitys sen käyttäjät voivat kerätä monenlaisia tietoja ja on siksi erityyppisiä soveltuvat nämä eri tietotyyppien Vie.
On 2 vie olennaiset tyyppiä:

- Tilannevedos: käytetään yleensä tietojen vieminen, joka edustaa tilaa ja, jonka Mobile välitys ei ole historia. Tämä sisältää tunnusten tai push markkinointikampanjan palautetta tunnisteet (sovelluksen-tiedot), kuten. Näin seuraavanlaisiin Vie liittyvät ei päivämäärää.
- Historiallinen: Vie tällaista käytetään tiedoille, kuten kasvaa ajan kuluessa, kuten tapahtumia tai toimintoja.

Seuraavassa taulukossa on kuvattu täysin kaikki mahdolliset viennin:

| Vie tyyppi | Tietotyyppi | Kuvaus                                                                                                                                 |
|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| Tilannevedos    | Push-      | Luo että Vie Push Kampanjat palautetta kohti deviceid/käyttäjätunnus välein                                                              |
| Tilannevedos    | Tunniste       | Luo vienti kunkin laitteisiin liittyvät tunnisteiden (tiedot sovelluksen)                                                                       |
| Tilannevedos    | Laite    | Luo vienti useimpien laitteiden technicals (mallin, kieli ja aikavyöhyke-) tunnisteita, näkyy ensimmäistä kertaa, kuten tietoja... |
| Tilannevedos    | Suojaustunnuksen     | Luo kelvollinen tunnusten vienti                                                                                                 |
| Historiallinen  | Toiminta  | Luo-tiettynä aikajaksona kunkin laitteiden toimintoja vienti                                                           |
| Historiallinen  | Tapahtuman     | Luo-tiettynä aikajaksona kunkin laitteiden toimintoja vienti                                                           |
| Historiallinen  | Työn       | Luo kunkin laitteiden töiden tiettynä aikajaksona-vienti                                                                 |
| Historiallinen  | Virhe     | Luo-tiettynä aikajaksona kunkin laitteiden virheitä vienti                                                               |

## <a name="how-does-it-work"></a>Miten se toimii?

Vienti on pitkä suoritettavat tehtävät, joka voi tuottaa suurten tiedostojen. Tästä syystä niitä ei voi käynnistää palauttaa heti Lataa tiedosto.
Tietojen vieminen Mobile välitys, jotta voit hallita Luo **Vientityön** kautta API johon määritetään yleensä:

- Vie (tilannevedos tai historiallisten)-tyyppi
- Tietotyyppi-
- **Azure-tallennustilan säiliön** (mukaan lukien kirjoitusoikeudet kelvollinen Suojaussidokset) mihin viennin saatu kirjoittaa.

Huomaa, että voi kestää muutaman minuutin työtäsi alkamaan ja sitten sitä voi käyttää joitakin sekunteja pieni sovellusten useita tunteja useita käyttäjiä tai toiminta-sovellusten.

Kun työ on luotu, on mahdollista tarkistaa sen tilan, jos se on edelleen käytössä, tai onko se on valmis.

Kun työ on onnistui, tuloksena on datatiedosto on käytettävissä annettu tallennustilan säilö.
