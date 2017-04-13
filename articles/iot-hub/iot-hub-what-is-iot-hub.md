<properties
 pageTitle="Azure IoT keskittimeen yleiskatsaus | Microsoft Azure"
 description="Azure IoT keskittimeen palvelun yleiskatsaus: mikä on iot keskittimeen, laitteen yhteys, internet asioita viestintä kuvioiden ja palvelun avustaa viestintä kuvio"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Mikä on Azure IoT toiminnossa?

Tervetuloa käyttämään Azure IoT-toiminnossa. Tässä artikkelissa on yleiskatsaus Azure IoT keskittimeen ja kerrotaan, miksi tämä palvelu kannattaa käyttää toteuttamisesta asioita Internet (IoT)-ratkaisu. Azure IoT-toiminnossa on täysin hallitun palvelun luotettavan ja turvallisen kaksisuuntainen miljoonia IoT laitteet ja ratkaisu taustatietokannaksi välisten yhteyksien kautta. Azure IoT-toiminnossa:

- On luotettava laite cloud ja cloud ja laitteen messaging tasolla.
- Ottaa käyttöön suojattu tietoliikenne käyttämällä laitteen kohti suojausvaltuudet ja käyttää ohjausobjektin.
- On laaja seurantaa laitteen yhteys- ja laitteen käyttäjätietojen hallinta tapahtumat.
- Sisältää Suosituimmat kielet ja ympäristöjen laitteen kirjastoissa.

Artikkelin [IoT keskittimeen vertailu ja tapahtuman keskittimet] [ lnk-compare] kuvataan nämä kaksi palvelut eroista ja korostaa edut IoT keskittimeen käyttäminen IoT ratkaisuja.

![Azure IoT keskittimeen cloud yhdyskäytävän asioita ratkaisu Internet][img-architecture]

> [AZURE.NOTE] Katso aiemmin perusteellisempaa keskusteluun IoT arkkitehtuuri [Microsoft Azure IoT viittaus arkkitehtuuri][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>IoT laitteen connectivity haasteita

IoT keskittimeen ja laitteen-kirjastojen avulla voit luotettavasti ja suojatusti yhteyden laitteiden ratkaisu taustatietokantaan haasteiden mukaiseksi. IoT laitteet:

- Ovat usein upotetun järjestelmien puuttuu ihmisten operaattori.
- Voi olla remote sijainneissa, missä fyysinen access kallista.
- Vain ehkä tavoitettavissa kautta ratkaisu takaisin loppuun.
- Saattaa on rajoitettu power ja käsittelyn resurssit.
- Saatat joutua ajoittain, hidasta tai kallista verkkoyhteyden.
- Saatat joutua käyttämään Omat, mukautetut tai toimialakohtaisia protokollia.
- Voit luoda Suositut laitteiston ja ohjelmiston ympäristöjen suuren tietojoukon avulla.

Lisäksi edellä vaatimukset minkä tahansa IoT ratkaisu on myös pitää mittakaava, suojaus ja luotettavuus. Tuloksena on joukko connectivity vaatimuksista on vaikeaa ja aikaa toteuttamisesta perinteinen tekniikoita, kuten web säilöjä ja tekstiviesti vakuutuksenvälittäjän käytettäessä.

## <a name="why-use-azure-iot-hub"></a>Mitä hyötyä Azure IoT-toiminnossa

Azure IoT keskittimeen korjaa laitteen connectivity haasteita seuraavilla tavoilla:

-   **Laitteen todennus- ja suojattu**. Voit valmistella kunkin laitteen Oma [suojauksen avaimen] [ lnk-devguide-security] , jotta se voi muodostaa IoT toiminnossa. [IoT keskittimeen tunnistetietojen rekisterin] [ lnk-devguide-identityregistry] tallentaa ratkaisun laitteen käyttäjätietoja ja avaimet. Ratkaisu taustatietokannaksi lisätä yksittäisten laitteiden Salli tai estä hallita laitteen access-luetteloihin.

-   **Laitteen yhteys toimintojen seuranta**. Saat yksityiskohtaiset toiminnon lokeja laitteen tunnistetietojen hallintatoiminnot ja laitteen connectivity tapahtumia. Tämän seurannan vain mahdollistaa IoT ratkaisu tunnistavan yhteysongelmat, kuten laitteita, yritä muodostaa väärä tunnuksilla lähettämiseen liian usein ja hylkää kaikki cloud laitteen viestejä.

-   **Laitteen kirjastojen monipuolinen**. [Azure IoT laitteen SDK: T] [ lnk-device-sdks] ovat käytettävissä ja tuettujen eri kielillä ja ympäristöjen--C monta Linux jaot, Windowsin ja reaaliaikainen käyttöjärjestelmät. Azure IoT laitteen SDK: T tukevat hallittua kieliä, kuten C#, Java ja JavaScript.

-   **IoT protokollat ja laajennettavuus**. Jos ratkaisu ei voi käyttää laitteen kirjastot, IoT keskittimeen paljastaa julkisen protokolla, jonka avulla voidaan käyttää grafiikkatiedostomuotoja MQTT v3.1.1, HTTP 1.1 tai AMQP 1.0 protokollat laitteissa. Voit myös laajentaa IoT-toiminnossa mukautettuja protokollia, tukevat:

    - Luomalla kentän yhdyskäytävän [Azure IoT yhdyskäytävän SDK] [ lnk-gateway-sdk] , joka muuntaa mukautettu protokolla jotakin kolmesta protokollista ymmärrä IoT toiminnossa. 
    - Mukauttaminen [Azure IoT protokolla yhdyskäytävän][protocol-gateway], Avaa lähde-osa, joka suoritetaan pilveen.

-   **Asteikko**. Miljoonia samanaikaisesti yhdistettyjen laitteiden ja tapahtumia sekunnissa miljoonia Skaalaa Azure IoT-toiminnossa.

Seuraavat edut ovat yleisiä monta viestintä kuviot. IoT keskittimeen avulla tällä hetkellä toteuttavien tietyn viestintä seuraavista tavoista:

-   **Tapahtuma-pohjaisen laitteen cloud nieltynä.** IoT keskittimeen luotettavasti saavat tapahtumat sekunnissa miljoonia laitteilla. Se voit käsitellä niitä kuuma polun käyttämällä tapahtuma-suoritin ohjelma. Se tallentaa ne myös kylmän PATH-komennolla analyysia varten. IoT keskittimeen säilyttää enintään seitsemän päivän ajan luotettava käsitellään takaa ja lataa päät Vaimenna tapahtumatietoja.

-   * *Luotettava cloud laitteen messaging (tai *komennot*). ** ratkaisu takaisin loppuun avulla osoitteessa ainakin kerran-toimitus-takuu sisältävien viestien lähettäminen yksittäisten laitteiden IoT toiminnossa. Viesti on yksittäinen--elinaika-asetus ja uudelleen voi pyytää kuittausta toimitus- ja vanhenemista. Nämä kuittaukset varmistaa elinkaaren cloud laitteen viestin koko näkyvyys. Voit ottaa käyttöön sitten liiketoimintalogiikan, joka sisältää toimintoja, jotka suoritetaan laitteissa.

-   **Lataa tiedostot ja välimuistiin tunnistimen tiedot pilvipalveluun.** Laitteet ladata tiedostoja käyttämällä SAS URI IoT keskittimeen puolestasi hallitsee Azure-tallennustilan. IoT toiminnossa voit luoda ilmoituksia tiedostojen saapuessa pilveen käyttöön takaisin päässä käsitellä niitä.

## <a name="gateways"></a>Yhdyskäytävät

Yhdyskäytävän IoT ratkaisussa on yleensä joko [protokolla yhdyskäytävän] [ lnk-gateway] , jotka on otettu käyttöön pilveen tai [kentän yhdyskäytävän] [ lnk-field-gateway] , jotka on otettu käyttöön paikallisesti laitteet. Protokollan yhdyskäytävän suorittaa protokolla käännös, kuten MQTT AMQP avulla. Kentän yhdyskäytävän voit suorittaa analytics reunan, aika-luottamukselliset päätösten vähentää viive, laite hallinnan tarjoamiseen, suojaus ja tietosuoja rajoitukset ja suorittaa myös protokolla käännös. Yhdyskäytävän molempien tietotyyppien toimivat laitteet ja IoT-toiminnossa välillä.

Kentän yhdyskäytävän eroaa yksinkertainen liikenne reititys laitteen (esimerkiksi osoitteeseen käännös verkkolaitteiden tai palomuurin), koska se yleensä suorittaa aktiivisen roolin hallinta access ja tietojen kulun-ratkaisu.

Ratkaisu saattaa olla protokolla ja kentän yhdyskäytävät.

## <a name="how-does-iot-hub-work"></a>Miten IoT keskittimeen toimii?

Azure IoT keskittimeen toteuttaa [palvelun avustaa viestintä] [ lnk-service-assisted-pattern] kuvion mediate laitteet ja ratkaisu takaisin päässä vuorovaikutuksesta. Palvelun avustaa viestintä lähinnä muodostaa luotettava, kaksisuuntainen viestintä polut välillä ohjausobjektin järjestelmä, kuten IoT keskittimeen ja erityiseen käyttöön laitteita, joka on otettu käyttöön epäluotettavana fyysinen tila. Kuvion vaihtelevat seuraavien periaatteiden:

- Suojauksen ohittaa muita ominaisuuksia.
- Laitteiden Hyväksy luvattoman verkkotiedot. Laitteen muodostaa kaikki yhteydet ja tiet lähtevä vain tavalla. Laitteen vastaanottamaan komennon uudelleen laite on aloitettava säännöllisesti yhteyden odotetaan komentojen käsittelemään tarkistavan.
- Laitteiden vain yhdistäminen tai vahvistaa tiet tunnetun palveluja, ne ovat peered, kuten IoT toiminnossa.
- Protokolla sovellustason on suojattu tietoliikenne polku tai laitteen ja yhdyskäytävän laite- ja palveluluettelon välillä.
- Järjestelmän tason todennus ja käyttöoikeuksien perustuvat laitteen kohti käyttäjätietoja. Niiden ansiosta access tunnistetiedot ja käyttöoikeudet lähes välittömästi voi kumota.
- Kaksisuuntaisen yhteyden laitteita, jotka muodostavat 0x1E power tai yhteyksien vuoksi koskee helpottaa pitämällä komennot ja laitteen ilmoituksia, kunnes muodostettaessa vastaanottamaan ne. IoT keskittimeen ylläpitää laitekohtaiset olevien komentojen, se lähettää.
- Sovelluksen paketin tiedoista on suojattu erikseen suojatun salataanko siirrettävät yhdyskäytävien tietyn palvelun kautta.

Mobiili alan on käyttänyt palvelun avustaa viestintä kuvion valtava tasolla toteuttamisesta push-ilmoituksen palvelut, kuten [Windows Push ilmoituksen Services][lnk-wns], [Google Cloud Messaging][lnk-google-messaging]- ja [Apple Push-ilmoituspalvelu][lnk-apple-push].

IoT keskittimeen tuetaan ExpressRoute's julkisen peering polku päälle.

## <a name="next-steps"></a>Seuraavat vaiheet

Miten Azure IoT keskittimeen mahdollistaa standardien-pohjaisten IoT Laitehallinta etäyhteyden hallintaa, määrittää ja päivittää laitteet on artikkelissa [Azure IoT keskittimeen yleiskatsaus Laitehallinnan][lnk-device-management].

Toteuttamaan asiakassovelluksissa laitteen laitteiston ympäristöjen ja käyttöjärjestelmien erilaisia voit IoT laitteen SDK: T. IoT laitteen SDK: T sisältää kirjastoja, jotka helpottavat lähettämisen telemetriatietojen IoT-toiminnosta ja vastaanota cloud laitteen komentoja. Kun SDK: T, voit valita eri verkkoprotokollien pitää yhteyttä IoT toiminnossa. Lisätietoja on artikkelissa [tietoja laitteen SDK: T][lnk-device-sdks].

Aloita kirjoittaminen lisäkoodin ja muutamia esimerkkejä käynnissä-kohdasta [IoT keskittimeen käytön aloittaminen] [ lnk-get-started] opetusohjelma.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Palvelun avustaa tietoliikenne-blogimerkinnässä Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
