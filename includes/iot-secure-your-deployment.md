# <a name="securing-your-iot-deployment"></a>IoT käyttöönoton suojaaminen

Tämä artikkeli tarjoaa seuraavan tason yksityiskohta Azure IoT-pohjainen Internet, asiat (IoT) infrastruktuurin suojaaminen. Se on linkitetty tason käyttöönottotiedot määrittäminen ja käyttöönotto jokaisen komponentin. Se tarjoaa myös vertailuja ja valintoja eri kilpailevien menetelmien välillä.

Azure IoT-käyttöympäristön suojaus voidaan jakaa kolmeen suojausalueilla:

- **Laitteiden tietoturvaa**: IoT laitteen suojaaminen samalla, kun se on otettu käyttöön luonnosta.
- **Yhteyden suojaus**: varmistaa kaikkien IoT laitteen ja keskittimen IoT välillä siirretyt tiedot ovat luottamuksellisia ja väärentämiseltä.
- **Pilven tietoturva**: tietojen suojaa tiedot, kun se siirtyy ja tallennetaan pilven.

![Kolme turva-alueet][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Laitteen käyttöönotto ja todennus

Azure IoT Suite suojaa IoT laitteet mukaan seuraavista tavoista:

- Antamalla jokaiselle laitteelle, jota voidaan käyttää laitteen IoT keskittimen kanssa yksilöllisen käyttäjätiedon avain (suojaustunnussanomat).

- Valitse-laitteella [X.509-varmenne] [ lnk-x509] ja yksityisen avaimen keinona todentaa laitetta IoT keskittimeen. Tämä todennusmenetelmä varmistaa sen, että laitteen yksityinen avain ei ole tiedossa ulkopuolella laite milloin tahansa, tarjoaa paremman suojauksen.

Token suojausmenetelmä on jokaisen soiton tekemien laitteen IoT keskitin liittämällä jokaisen kutsun symmetrisen avaimen todennusta. X.509-pohjaisen todennuksen avulla todennus IoT-laitteen fyysinen tasolla osana TLS-yhteyden luominen. Security token perustuva menetelmä voidaan käyttää ilman X.509-todennuksen, joka on vähemmän turvallinen kuvio. Valita kahden menetelmän välillä määräytyy ensisijaisesti kuinka turvallinen laitteen todennus on oltava ja suojattua tallentamista (Jos haluat tallentaa yksityisen avaimen turvallisesti) laitteen käytettävyyden mukaan.

## <a name="iot-hub-security-tokens"></a>IoT keskitin suojaustunnussanomat

IoT keskitin käyttää suojaustunnussanomat Todenna laitteet ja palvelut verkossa avaimet lähettämistä tulisi välttää. Lisäksi suojaustunnussanomat voimassaoloajan ja soveltamisala on rajoitettu. Azure IoT keskitin SDK: T luodaan automaattisesti ilman kirjoittamista mitään erityisasetuksia tunnuksia. Joissakin tapauksissa kuitenkin vaatia, että käyttäjä voi luoda ja käyttää suojaustunnussanomat suoraan. Nämä ovat MQTT, AMQP tai HTTP-pinnat suoran käytön Suojaustunnuspalvelun kuvio täytäntöönpanoa.

Lisätietoja rakenteen Suojaustunnussanoman ja sen käyttö löytyvät seuraavista artikkeleista:

-   [Suojauksen tunnussanoma rakenne][lnk-security-tokens]
-   [SAS-tunnuksia käyttämällä laitteena][lnk-sas-tokens]

Jokainen IoT Hub on [Laitteen identiteetin rekisterin] [ lnk-identity-registry] , joka voidaan luoda laite resurssit-palvelussa jono, joka on lennolla cloud laitteen viestit kuten ja pääsy laitteen tapahtuvaa päätepisteet. IoT keskitin käyttäjätiedot rekisteriin on turvallinen varastointi laite identiteettien ja suojausavainten ratkaisua. Henkilön tai laitteen tunnistetietoja ryhmät voidaan lisätä sallittujen tai torjuttujen luettelon, käyttöön laitetta käyttämästä hallintaansa. Seuraavissa artikkeleissa antaa lisätietoja laitteen identiteetin rekisterin rakenne ja tukevat toiminnot.

[IoT keskitin tukee protokollia, kuten MQTT, AMQP ja HTTP][lnk-protocols]. Jokainen näistä protokollista käyttää suojaustunnussanomat IoT laitteen keskittimen IoT eri tavalla:

- AMQP: SASL TAVALLINEN ja AMQP väitteitä suojauksen ({policyName}@sas.root.{iothubName} osalta Hub-tason tunnuksia; {deviceId} Jos laitteen mitoitettu tunnuksia).

- MQTT: Yhdistä paketti käyttää {deviceId} {ClientId}, {IoThubhostname} / {deviceId} **käyttäjänimi** -kenttään ja SAS-tunnuksen **salasana** -kenttään.

- HTTP: Luvan pyynnön otsikossa on virheellinen tunnussanoma.

Määritä suojausvaltuudet laite ja käyttää komponenttia voidaan IoT Hub laite käyttäjätiedot rekisteriin. Kuitenkin jos IoT-ratkaisu on jo merkittäviä investointeja [mukautetun laitteen identiteetin rekisteri-ja/tai järjestelmän][lnk-custom-auth], se voidaan integroida olemassa olevan infrastruktuurin IoT keskittimen kanssa luomalla tunnuksen palvelu.

### <a name="x509-certificate-based-device-authentication"></a>X.509 todistus-pohjaisen laitteen todennus

Käytä [laitteen perustuvat X.509-varmenne] [ lnk-use-x509] ja sen liittyvän yksityisen ja julkisen avaimen parin avulla todennus on fyysinen kerros. Yksityinen avain on tallennettu laitteen turvallisesti eikä ole ulkopuolella laite havaittavaksi. X.509-varmenne sisältää laitteen, esimerkiksi laitteen tunnus ja muut organisaation tietoja. Varmenteen allekirjoituksen luodaan käyttämällä yksityistä avainta.

Valmistelun kulkua korkean tason laite:

- Liitä tunnus fyysisen laitteen – laitteen tunnistetiedot ja/tai liitetty laitteeseen laitteen käyttöönottoa tai valmistuksen aikana X.509-varmenne.

- Luo vastaava tapahtuma identiteetin IoT Hub – laitteen tunnistetiedot ja liitetyn laitteen tiedot rekisterissä IoT Hub laite.

- Rekisterin IoT Hub laite voidaan turvallisesti tallentaa X.509-varmenteen allekirjoitusta.

### <a name="root-certificate-on-device"></a>Laitteen sertifikaatti

IoT keskittimen kanssa suojattua TLS-yhteyttä muodostettaessa IoT laitteen todentaa IoT keskitin pääsertifikaattiin, joka on osa laitetta SDK: N avulla. C asiakkaan SDK varmenne sijaitsee kansiossa "\\c\\varmenteet" repo pääkansiossa. Vaikka nämä varmenteet ovat pitkäikäiset, ne voivat vanhentua edelleen tai kumota. Jos mitenkään sertifikaatti laitteen päivittämistä, laite ei ehkä myöhemmin muodostaa IoT Hub (tai pilvipalvelu). Ottaa keinot päävarmenne päivittää kun IoT-laite on otettu käyttöön tehokkaasti lieventämään tätä riskiä.

## <a name="securing-the-connection"></a>Yhteyden suojaaminen

IoT laitteen ja keskittimen IoT välillä Internet-yhteys on suojattu Transport Layer Security (TLS)-standardin avulla. Azure IoT [TLS 1.2]tukee[lnk-tls12], 1.1 TLS-Protokollaa ja TLS 1.0, tässä järjestyksessä. TLS 1.0-tuki on käytettävissä vain yhteensopivuuden. On suositeltavaa käyttää TLS 1.2, koska se sisältää tietoturvan.

Azure IoT Suite tukee seuraavia salakirjoitusjärjestelmistä tässä järjestyksessä.

| Salausohjelmisto | Pituus |
|--------------|--------|
| TLS\_ECDHE\_RSA\_kanssa\_AES\_256\_CBC\_SHA384 (0xc028) ECDH secp384r1 (eq. LA 7680 bitin RSA) | 256    |
| TLS\_ECDHE\_RSA\_kanssa\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. LA 3072 bittiä RSA) | 128    |
| TLS\_ECDHE\_RSA\_kanssa\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. LA 7680 bitin RSA)           | 256    |
| TLS\_ECDHE\_RSA\_kanssa\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. LA 3072 bittiä RSA)           | 128    |
| TLS\_RSA\_kanssa\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_RSA\_kanssa\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_RSA\_kanssa\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_RSA\_kanssa\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_RSA\_kanssa\_AES\_256\_CBC\_SHA (0x35)    | 256    |
| TLS\_RSA\_kanssa\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_RSA\_kanssa\_3DES\_muunnettua\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>Pilven suojaaminen

Azure IoT keskittimen avulla [access control politiikan] määrittely[ lnk-protocols] kunkin-suojausavaimelle. Se käyttää seuraavia joukon käyttöoikeuksia myöntää pääsy IoT keskitin päätepisteet. Oikeudet rajoittaa IoT keskittimeen toimintojen perusteella.

- **RegistryRead**. Myöntää lukuoikeus rekisterin laitteen tunnistetiedot. Lisätietoja [Laitteen identiteetin rekisterin][lnk-identity-registry].

- **RegistryReadWrite**. Avustukset luku- ja kirjoitusoikeus: laitteen identiteetin rekisteriin. Lisätietoja [Laitteen identiteetin rekisterin][lnk-identity-registry].

- **ServiceConnect**. Avustusten käytön cloud service tapahtuvaa viestintä- ja päätepisteiden valvonta. Esimerkiksi se antaa oikeudet vastaanottaa viestejä laitetta pilvi, pilvi laitteeseen lähettämisessä ja hakea vastaavan toimituksen vahvistusviestejä taustapalvelimena cloud-palvelut.

- **DeviceConnect**. Sallii pääsyn laitteen tapahtuvaa tietoliikenteen päätepisteet. Esimerkiksi myöntää oikeus lähettää viestejä laitetta cloud ja vastaanottaa viestejä cloud laite. Tätä oikeutta käytetään laitteita.

On kaksi tapaa saada **DeviceConnect** oikeudet [suojaustunnussanomat]IoT keskittimen kanssa[lnk-sas-tokens]: laitteen tunnistetiedot-avainta tai jaettuun käyttöön käytäntöavaimen avulla. Lisäksi on tärkeää huomata kaikkia toimintoja voi käyttää laitteita altistuu normaalia päätepisteisiin etuliite `/devices/{deviceId}`.

[Palvelun osat voivat luoda vain suojaustunnussanomat] [ lnk-service-tokens] yhteisiä käytäntöjen myönnetään tarvittavat käyttöoikeudet.

Salli Azure Active Directoryn avulla käyttäjien hallintaa Azure IoT Hub ja muut palvelut, jotka voivat olla osa ratkaisua.

Ruuansulatukseen Azure IoT keskittimen tiedot voidaan kuluttaa erilaisia palveluja, kuten Azure Stream Analytics ja Azure blob-etäsäilöpalvelun mukaan. Näiden palveluiden avulla management access. Lue lisää näistä palveluista ja käytettävissä olevat vaihtoehdot alla:

- [Azure DocumentDB][lnk-docdb]: scalable, täysin indeksoitu tietokantapalvelu, jota hallitaan laitteiden metatiedot osittain rakenteellisia tietoja, voit valmistella, kuten määritteitä kokoonpano ja ominaisuudet. DocumentDB tarjoaa korkean suorituskyvyn ja korkea-nopeus käsittelyssä, rakenteen ympäristöstä riippumattomalla tavalla indeksoinnin ja monipuolinen SQL-kyselyn liittymä.

- [Azure Stream Analytics][lnk-asa]: reaaliaikainen käsittely, jonka avulla voit nopeasti kehittää ja ottaa käyttöön edullisten analysointiratkaisu laitteet, anturit, infrastruktuurin ja sovellusten reaaliaikainen asuun paljastaa pilven stream. Täysin hallita palvelun tiedot voit skaalata mihin tahansa asemaan toimiessaan edelleen suuri siirtonopeus, pieni viive ja vikasietoisuutta.

- [Azure App palvelut][lnk-appservices]: cloud-ympäristö rakentaa tehokkaita web ja mobiili apps, jotka yhdistävät tietoja mistä tahansa; pilvi tai tiloissa. Rakentaa hoiti kannettava apps, iOS, Android ja Windows. Ohjelmisto palveluna (SaaS) ja out-of--box yhteyttä yrityssovelluksista kymmeniä pilvi-pohjaiset palvelut ja yrityssovellusten integrointi. Koodin suosikki kieli ja IDE (.NET, NodeJS, PHP, Python ja Java) rakentaa web apps ja API-liittymien nopeammin kuin koskaan.

- [Logiikan Apps][lnk-logicapps]: logiikka Apps Azure App palvelun avulla integroida IoT ratkaisun olemassa olevan liiketoiminnan linja-järjestelmiin ja automatisoida työnkulun prosessit. Logiikan Apps avulla kehittäjät voivat rakenteen työnkulkuja, jotka käynnistin käynnistää ja Suorita vaiheet — säännöt ja toimet, jotka käyttävät tehokkaita liittimet voidaan integroida yrityksen liiketoimintaprosesseja. Logiikan sovellukset tarjoavat valtavia ekosysteemiin, SaaS, pilvipohjainen, out-of--box-yhteydet ja tilojen sovelluksia.

- [Azure-blob-etäsäilöpalvelun][lnk-blob]: luotettava, energiatehokkaat cloud storage tiedoille, jotka laitteiden lähettää pilven.

## <a name="conclusion"></a>Tekemisestä

Tässä artikkelissa on yleiskatsaus toteutus tason tietojen suunnitteleminen ja käyttöönottaminen IoT-infrastruktuurin Azure IoT. Avain IoT yleisen infrastruktuurin suojaaminen on jokaisen osan suojatuksi. Käytettävissä olevat Azure IoT rakennevaihtoehtoja tarjoamaan joitakin joustavuutta ja valinnanvaraa; kuitenkin kukin vaihtoehto saattaa olla tietoturvaan vaikutuksia. On suositeltavaa, että näitä vaihtoehtoja voidaan arvioida kustannukset riskin arvioinnin kautta.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
