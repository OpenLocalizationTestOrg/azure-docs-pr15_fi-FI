<properties
    pageTitle="Azure CDN yleiskatsaus | Microsoft Azure"
    description="Katso, mikä Azure sisällön toimituksen verkon (CDN) on ja miten sitä käytetään toimittaa suuren kaistanleveyden sisällön tallentamalla välimuistiin BLOB-objektit ja staattiseksi sisällöksi."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Yleistä Azure sisällön toimittamisen verkon (CDN)

> [AZURE.NOTE] Tässä asiakirjassa kuvataan, mitä Azure sisällön toimituksen verkon (CDN) on, sen toiminnasta ja Azure CDN jokaisen tuotteen ominaisuuksia.  Jos haluat ohittaa nämä tiedot ja siirtyä suoraan opetusohjelman CDN päätepisteen luomisesta, katso [Käyttämällä Azure CDN](cdn-create-new-endpoint.md).  Jos haluat nähdä luettelo nykyisen CDN solmu sijainneista, katso [Azure CDN POP sijainnit](cdn-pop-locations.md).

Azure sisällön toimituksen verkon (CDN) välimuistiin staattinen verkkosisällön strategisesti sijoitetaan paikoissa antamaan suurin nopeus välittää sisällön käyttäjille.  CDN on kehittäjät yleisen ratkaisun välittää suuren kaistanleveyden sisällön tallentamalla välimuistiin fyysinen solmujen sisällön kaikkialla maailmassa. 

Välimuistin sivuston vastaavien CDN käytön etuja ovat:

- Parantaa suorituskykyä ja käyttäjän kohdata käyttäjille, erityisesti silloin, kun sovelluksilla, jossa useita edestakaista tarvitaan ladata sisältöä.
- Suuri skaalaus entistä paremmin käsitellä välittömästi suuren kuormituksen aikana, kuten tuotteen julkistaminen nopeasti alkuun.
- Jakaminen pyynnöt ja tarjota reunapalvelimet sisältöä, vähemmän liikenne lähettää alkuperän.


## <a name="how-it-works"></a>Toiminta

![CDN yleiskatsaus](./media/cdn-overview/cdn-overview.png)

1. Käyttäjä (Anneli) pyytää (kutsutaan myös sijoituksen) tiedoston URL-Osoitetta käyttämällä erityistä toimialuenimeä, kuten `<endpointname>.azureedge.net`.  DNS reitittää pyynnön parhaan suorittamiseen piste-,-tavoitettavuustietojen (POP) sijainti.  Tämä on yleensä, joka on lähimpänä maantieteellisesti käyttäjän POP.

2. Jos POP-reunapalvelimet ei ole tiedoston niiden välimuistin, edge server pyyntöjä tiedoston alkuperän.  Alkuperän voi olla Azure Web App-sovelluksessa, Azure pilvipalvelussa, Azure-tallennustilan tilin tai kaikkien käytettävissä minkä tahansa verkkopalvelin.

3. Alkuperän palauttaa tiedoston reuna-palvelimeen, valinnainen kuvaava tiedoston--elinaika (TTL) HTTP-otsikot mukaan lukien.

4. Edge Serverin tallentaa tiedoston, ja palauttaa tiedoston alkuperäinen pyytäjän (Anneli).  Tiedosto on välimuistissa reunapalvelinta, kunnes TTL vanhenee.  Jos alkuperän ei määritä TTL oletusarvoista TTL on seitsemän päivän aikana.

5. Muita käyttäjiä voi pyytää, että URL-Osoitetta käyttämällä samaa tiedostoa ja voivat myös ohjautuu, että sama POP.

6. Jos tiedoston TTL ei ole vanhentunut, edge server palauttaa tiedoston välimuistista.  Tuloksena on entistä nopeampi ja suorituskykyisempi käyttökokemusta.


## <a name="azure-cdn-features"></a>Azure CDN ominaisuudet

On kolme Azure CDN tuotteiden: **Azure CDN vakio-Akamai**, **Azure CDN vakio-Verizon**ja **Azure CDN Premium-Verizon**.  Seuraavassa taulukossa on lueteltu jokaisen tuotteen kanssa käytettävissä olevat ominaisuudet.

|       | Vakio Akamai | Vakio Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Kuten [tallennuskiintiön](cdn-create-a-storage-account-with-cdn.md), [Pilvipalveluihin](cdn-cloud-service-with-cdn.md), [Web Apps -sovellusten](../app-service-web/cdn-websites-with-cdn.md)ja [Media-palveluiden](../media-services/media-services-portal-manage-streaming-endpoints.md) services helposti Azure-integrointi | **& #x 2713;** | **& #x 2713;** | **& #x 2713;**|
| [REST API](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](./cdn-app-dev-net.md), [Node.js](./cdn-app-dev-node.md)tai [PowerShellin](./cdn-manage-powershell.md)kautta hallinta. | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| HTTPS-tuki | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Kuormituksen | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Www.microsoft.com-sivustoa kohtaan](https://www.us-cert.gov/ncas/tips/ST04-015) suojaus | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| Kahden pinon IPv4 ja IPv6 | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Mukautetun toimialueen nimeä tuki](cdn-map-content-to-custom-domain.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Kyselymerkkijonon välimuistin](cdn-query-string.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [GEO suodattaminen](cdn-restrict-access-by-country.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Tyhjennä nopeasti](cdn-purge-endpoint.md) | **& #x 2713;** | **& #x 2713;** | **& #x 2713;** |
| [Kohteiden ennen lataamista](cdn-preload-endpoint.md) |  | **& #x 2713;** | **& #x 2713;** |
| [Core analytics](cdn-analyze-usage-patterns.md) |  | **& #x 2713;** | **& #x 2713;** |
| [HTTP/2-tuki](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x 2713;**  |  |  |
| [Lisäasetukset HTTP-raportit](cdn-advanced-http-reports.md) | | | **& #x 2713;** |
| [Reaaliaikainen tilasto](cdn-real-time-stats.md) | | | **& #x 2713;** |
| [Reaaliaikainen ilmoitukset](cdn-real-time-alerts.md) | | | **& #x 2713;** |
| [Mukautettavissa, säännön sisällön toimittamisen ohjelma](cdn-rules-engine.md) | | | **& #x 2713;** |
| (Joko [säännöt ohjelma](cdn-rules-engine.md)) välimuistin ja otsikko-asetukset  | | | **& #x 2713;** |
| URL-Osoitteen uudelleenohjaus/suorittamaan (joko [säännöt ohjelma](cdn-rules-engine.md)) | | | **& #x 2713;** |
| Mobiililaitteen säännöt (joko [säännöt ohjelma](cdn-rules-engine.md))  | | | **& #x 2713;** |

>[AZURE.TIP] Onko haluat nähdä Azure CDN ominaisuutta?  [Palautteen antaminen](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Seuraavat vaiheet

Aloittaminen CDN, on artikkelissa [Azure CDN avulla](./cdn-create-new-endpoint.md).

Jos olet aiemmin CDN-asiakas, voit hallita CDN-päätepisteet, tai [PowerShellin](cdn-manage-powershell.md)kautta [Microsoft Azure-portaalissa](https://portal.azure.com) .

Nähdäksesi CDN-toiminto kuittaa ulos [video Microsoftin muodosta 2016-istunnon](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Lue, miten voit automatisoida Azure CDN [.NET](./cdn-app-dev-net.md) tai [Node.js](./cdn-app-dev-node.md).

Alla on tietoja, katso [CDN hinnat](https://azure.microsoft.com/pricing/details/cdn/).
