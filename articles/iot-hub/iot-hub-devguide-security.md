<properties
 pageTitle="Sovelluskehittäjän opas - IoT pääsivuston käyttöoikeuksien hallinta | Microsoft Azure"
 description="Azure IoT keskittimeen Sovelluskehittäjän opas - IoT pääsivuston käyttöoikeuksien hallinta ja tietoturvan hallintaa"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>IoT pääsivuston käyttöoikeuksien hallinta

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan suojaaminen IoT-toiminnossa asetukset. IoT toiminnossa käytetään *käyttöoikeuksia* kunkin IoT keskittimeen päätepisteet käyttöoikeus. Käyttöoikeuksien rajoittaa toimintoja perusteella IoT keskittimeen.

Tässä artikkelissa:

- Eri käyttöoikeudet, voit myöntää laitteen tai taustatietokantaan-sovelluksen, voit käyttää IoT-toiminnossa.
- Todennusprosessin ja se käyttää käyttöoikeuksien vahvistamiseen tunnukset.
- Miten laajuuden tunnistetiedot rajoittaminen tietyn resurssien käytön.
- X.509-varmenteita IoT keskittimeen tuki.
- Mukautetun laitteen todennus järjestelmiä, jotka käyttävät olemassa olevan laitteen tunnistetietojen rekistereihin tai todennusmallit.

### <a name="when-to-use"></a>Käyttäminen

Sinulla on tarvittavat käyttöoikeudet, voit käyttää kaikkia IoT keskittimeen päätepisteet. Esimerkiksi laite on sisällettävä sisältävä suojausvaltuudet sekä kaikkiin lähetettäviin se lähettää IoT keskittimeen tunnusta.

## <a name="access-control-and-permissions"></a>Käyttöoikeuksista

[Käyttöoikeuksien](#iot-hub-permissions) myöntäminen seuraavilla tavoilla:

* **Pääsivuston tason jaettu käytön käytännöt**. Jaetun käytäntöjen voit myöntää [käyttöoikeuksia](#iot-hub-permissions)yhdistelmän. Voit määrittää käytännöt [Azure portal][lnk-management-portal], tai ohjelmallisesti käyttämällä [IoT keskittimeen resurssin tarjoajaan REST API][lnk-resource-provider-apis]. Juuri luomasi IoT-toiminnossa on seuraavat oletuskäytäntöjä:

    - **iothubowner**: käytäntö, jonka kaikki käyttöoikeudet.
    - **palvelu**: käytännön ServiceConnect käyttöoikeuksia.
    - **laitteen**: käytännön DeviceConnect käyttöoikeuksia.
    - **registryRead**: käytännön RegistryRead käyttöoikeuksia.
    - **registryReadWrite**: käytännön RegistryRead ja RegistryWrite käyttöoikeuksilla.


* **Laitteen kohti suojausvaltuudet**. Kunkin IoT-toiminnossa sisältää [laitteen tunnistetietojen rekisterin][lnk-identity-registry]. Tämän rekisteriavaimen kunkin laitteen voit määrittää suojauksen tunnistetiedot, jotka vastaavat laitteen päätepisteet suodatetut **DeviceConnect** käyttöoikeuksia.

Valitse esimerkiksi tyypillinen IoT-ratkaisua:

- Laitteen hallinta-osaa käytetään *registryReadWrite* käytännön.
- Tapahtuman suoritin osa käyttää *palvelun* käytännön.
- Laitteen runtime logiikan osaa käyttää *palvelun* käytännön.
- Yksittäisten laitteiden muodostaa yhteyden IoT-toiminnossa käyttäjätiedot rekisteriin tallennettuja tunnistetietoja.

## <a name="authentication"></a>Todennus

Azure IoT keskittimeen antaa käyttöoikeudet päätepisteet vahvistamalla tunnuksen vastaan jaettuun käyttöön käytännöt ja laitteen rekisterin suojauksen käyttäjätiedot.

Suojaustunnisteiden, kuten symmetrisen näppäimet ei lähetetä koskaan ‑toiminto.

> [AZURE.NOTE] Azure IoT keskittimeen resurssi-palvelu on suojattu Azure-tilauksesi avulla kuin kaikkien palveluntarjoajien [Azure Resurssienhallinta][lnk-azure-resource-manager].

Katso lisätietoja luoda ja käyttää suojauksen tunnusten [IoT keskittimeen suojauksen tunnusten][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokollan yksityiskohtia

Kunkin tuetut protokolla, kuten MQTT, AMQP ja HTTP-siirtotapahtumat tunnusten eri tavoilla.

Kun käytät MQTT, Yhdistä-paketin on deviceId ClientId {iothubhostname} / {deviceId} käyttäjänimi-kenttään ja SAS-tunnuksen salasana-kenttään. {iothubhostname} on oltava IoT-toiminto (esimerkiksi contoso.azure-devices.net) koko CNAME-tietue.

Kun käytät [AMQP][lnk-amqp], IoT toiminto tukee [Vain SASL] [ lnk-sasl-plain] ja [AMQP saatavat-pohjainen – suojaus][lnk-cbs].

Jos käytät AMQP saatavat-perustuvan-suojauksen, standardin määrittää, miten siirtämiseen näiden tunnusten.

**Käyttäjänimi** voi olla vain SASL:

* `{policyName}@sas.root.{iothubName}`Jos käytät keskittimeen tason tunnukset.
* `{deviceId}@sas.{iothubname}`Jos käytät laitetta kohdistettu tunnukset.

Kummassakin tapauksessa salasanakenttä sisältää tunnuksen, kuvatulla tavalla [IoT keskittimeen suojauksen tunnusten][lnk-sas-tokens].

HTTP toteuttaa todennus sisällyttämällä kelvollinen tunnus **luvan** pyynnön otsikko.

#### <a name="example"></a>Esimerkki

Käyttäjänimi (DeviceId on merkitsevä):`iothubname.azure-devices.net/DeviceId`

Salasana (Luo SAS laitteen Resurssienhallinnassa):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] [Azure IoT keskittimeen SDK: T] [ lnk-sdks] tunnusten luo automaattisesti, kun yhteyden muodostamista palveluun. Joissakin tapauksissa SDK: T eivät tue kaikkia protokollia tai kaikki todennustavat.

### <a name="special-considerations-for-sasl-plain"></a>Erityistä Huomioitavaa SASL vain

Käytettäessä SASL vain AMQP ja yhteyden muodostaminen IoT-toiminnossa asiakas voi käyttää yhden tunnuksen kunkin TCP-yhteyden. Kun tunnuksen vanhenee, TCP-yhteyden katkaisee palvelun ja käynnistää Muodosta yhteys uudelleen. Toiminta, kun ei ole ongelmia sovelluksen taustatietokantaan osan on hyvin järjestelmää vahingoittavan laitteen Include sovelluksen seuraavista syistä:

*  Yhdyskäytävien muodostaa yleensä puolesta useissa laitteissa. Kun käytät SASL vain, heillä on kunkin laitteen IoT-toiminnossa muodostamisesta eri TCP-yhteyden luominen. Tässä skenaariossa huomattavasti kasvaa kulutus power ja verkko- ja kasvattaa kunkin laitteen yhteyden viiveen.
* Resurssin rajoitettu laitteiden ovat vastainen uudelleen jokaisen tunnuksen vanhentumisen jälkeen resurssien käytön.

## <a name="scope-hub-level-credentials"></a>Laajuus keskittimeen tason tunnistetiedot

Voit rajoittaa keskittimeen tason suojauskäytäntöjä luomalla tunnusten rajoitettu esiintymät. Esimerkiksi laitteen cloud viestien lähettämiseen laitteesta päätepiste on **/devices/ {deviceId} / viestien/tapahtumat**. Voit käyttää myös toiminnossa tason jaetun käyttöoikeuskäytäntö **DeviceConnect** oikeuksilla kirjautua tunnuksen, jonka resourceURI on **/devices/ {deviceId}**. Tämä menetelmä luo tunnuksen, jota voi lähettää viestejä puolesta laitteen **deviceId**käyttää vain.

Se eroaa [tapahtuman keskittimet julkaisijan käytännön][lnk-event-hubs-publisher-policy], ja sallii Toteuta mukautettu todennustavat.

## <a name="security-tokens"></a>Suojauksen tunnusten

IoT keskittimeen käyttää suojauksen tunnusten tarkistamiseen laitteet ja palvelut välttämiseksi lähettää näppäimet koneisiin. Lisäksi suojauksen tunnusten on rajoitettu voimassaoloajan ja laajuus. [Azure IoT keskittimeen SDK: T] [ lnk-sdks] luo automaattisesti niin, ettei mitään erityistä määritysten tunnukset. Joissakin tilanteissa edellyttävät kuitenkin käyttäjä voi luoda ja käyttää suojauksen tunnusten suoraan. Näitä ovat MQTT, AMQP tai HTTP-Näyttömalli käyttää suoran käytön tai tunnuksen palvelu, kuvio soveltaminen [Mukautettu laitteen todennus]esitetyllä tavalla[lnk-custom-auth].

IoT keskittimeen myös sallii laitteiden IoT keskittimeen todentamismenetelmä käyttämällä [X.509-varmenteita][lnk-x509]. 

### <a name="security-token-structure"></a>Suojauksen suojaustunnuksen rakenne
Suojauksen tunnusten avulla voit myöntää ja palveluiden tiettyjen toimintojen IoT toiminnossa laitteiden aika joka käyttöoikeudet. Voit varmistaa, että vain valtuutettujen laitteet ja -palvelut muodostaa, suojauksen tunnusten on oltava kirjautuneena jaetun access-käytäntö-näppäintä tai laitteen tunnistetiedot, käyttäjätietojen rekisterin tallennettujen symmetrisen avaimen.

Tunnuksen allekirjoitettu jaettu käyttö käytännön avaimen sallii pääsyn kaikki toiminnot, jotka liittyvät jaetun käytännön oikeudet. Toisaalta-tunnuksen, joka on allekirjoitettu laitteen jäsenyyden symmetrisen avaimen antaa vain liittyvät laitteen käyttäjätietojen **DeviceConnect** -oikeudet.

Suojauksen salasana on seuraavanlainen:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

Odotettu arvot ovat seuraavat:

| Arvo | Kuvaus |
| ----- | ----------- |
| {allekirjoituksen} | HMAC SHA256 allekirjoitus-merkkijono lomakkeen: `{URL-encoded-resourceURI} + "\n" + expiry`. **Tärkeää**: avain purkaa base64- ja käyttää avaimeksi suorittamiseen HMAC SHA256 laskenta. |
| {resourceURI} | URI (tekijä: segmentin) etuliite päätepisteet, niitä voi käyttää tunnuksessa IoT-toiminnossa (ei protocol) hostname alkaen. Esimerkiksi`myHub.azure-devices.net/devices/device1` |
| {määräajan} | UTF8 merkkijonot sekunteina kausi 00:00:00 UTC-aika-1 tammikuussa 1970 jälkeen. |
| {URL-koodatun-resourceURI} | Palvelupyynnön pienempi pieniksi kirjaimiksi esiintymät URL-koodausta |
| {käytännön nimi} | Nimi, johon tunnuksessa viittaa jaetun käyttöoikeuskäytäntö. Poissa kyseessä tunnusten viittaaminen laitteen rekisterin tunnistetiedot. |

**Huomautus etuliite**: URI etuliite on laskettu segmentin ja merkin mukaan. Esimerkiksi `/a/b` on etuliitteen `/a/b/c` etkä `/a/bc`.

Seuraavat Node.js koodikatkelman näkyy toiminto nimeltä **generateSasToken** , joka laskee tunnus-syötteiden `resourceUri, signingKey, policyName, expiresInMins`. Seuraavien osien yksityiskohtaiset tiedot siitä, miten eri syötteiden eri suojaustunnuksen Käytä tapauksissa alusta.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Haluat vertailla vastaava Python koodi, voit luoda suojaustunnus on:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Tunnuksen voimassaoloajan tarkistetaan IoT keskittimeen tietokoneissa, se on tärkeää, että ajoverkoilla, joka luo tunnuksen tietokoneen kellon mahdollisimman vähän.

### <a name="use-sas-tokens-in-a-device-client"></a>Käytä SAS tunnusten laite-asiakassovelluksessa

Voit hankkia IoT keskittimeen **DeviceConnect** oikeudet jakaa suojauksen kahdella tavalla: [symmetrisen laitteen avainta laitteen tunnistetietojen rekisteristä](#use-a-symmetric-key-in-the-identity-registry)tai [jaettuun käytännön pikanäppäin](#use-a-shared-access-policy).

Muista, että kaikki toiminnot, jotka ovat käytettävissä olevat laitteet näyttämiä päätepisteet etuliite rakenne `/devices/{deviceId}`.

> [AZURE.IMPORTANT] IoT keskittimeen todentaa tietyn laitteen ainoa tapa käyttää laitteen tunnistetietojen symmetrisen avaimen. Tapauksissa käytettäessä jaetun käyttöoikeuskäytäntö käyttämään laitteen toiminto ratkaisu on otettava huomioon varmenteiden kuin luotettu jonkin aliosan suojaustunnus osa.

Laitteen osoittava päätepisteet ovat (riippumatta protocol):

| Päätepisteen | Toiminto |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Laitteen cloud lähettämiseen. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Cloud laitteen virhesanomia. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Käytä symmetrisen avaimen tunnistetietojen rekisterin

Kun laite jäsenyyden symmetrisen avaimen avulla voit luoda tunnuksen käytännön nimi (`skn`) tunnuksen osa jätetään pois.

Esimerkiksi tunnuksen, joka on luotu voivat käyttää kaikki laitteessa on oltava seuraavat parametrit:

* resurssin URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Kirjautuminen avain: kaikki symmetrisen avain `{device id}` käyttäjätiedot
* käytäntöä ei nimi
* vanhenemisen tahansa.

Esimerkki yllä solmu-funktion käyttäminen on seuraava:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Tulos, joka antaa käyttöoikeuden device1 kaikki toiminnot on seuraava:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Luo suojatun tunnuksen, .NET-työkalulla [Laitteen Explorer]ei[lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Käyttää jaettua käyttöoikeuskäytäntö

Luotaessa tunnusta jaetun access-käytäntö käytännön nimi kentän `skn` on määritettävä käytetään käytännön nimi. Kannattaa myös edellyttää, että käytäntö antaa **DeviceConnect** -oikeudet.

Kaksi tärkeimmät skenaarioiden jaetun käytäntöjen avulla voit käyttää laitteen toiminto ovat seuraavat:

* [cloud protokolla yhdyskäytävien][lnk-endpoints],
* [tunnussanoma services] [ lnk-custom-auth] käytettävä Toteuta mukautettu tarkistus kalastelulta.

Koska jaetun käyttöoikeuskäytäntö mahdollisesti voit myöntää oikeuden muodostaa yhteyden laitteeseen, kannattaa käyttää oikean resurssin URI suojauksen tunnusten luotaessa. Tämä on erityisen tärkeää suojaustunnuksen palveluista, joihin on alueen käyttämällä resurssin URI tietyn laitteen tunnuksen. Tämä kohta on vähemmän ajankohtaisia protokolla yhdyskäytävät, kun ne ovat jo kuvakartat liikenne kaikissa laitteissa.

Esimerkkinä suojaustunnuksen palvelu valmiiksi luodun jaetun access-käytäntö **laitteen** loisi tunnusta seuraavilla parametreilla:

* resurssin URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* Kirjautuminen avain: jokin avaimet `device` käytäntö
* käytännön nimi: `device`,
* vanhenemisen tahansa.

Esimerkki yllä solmu-funktion käyttäminen on seuraava:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Tulos, joka antaa käyttöoikeuden device1 kaikki toiminnot on seuraava:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Protokolla yhdyskäytävän voi käyttää samaa tunnuksen määrittäminen yksinkertaisesti resurssin URI kaikissa laitteissa, `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>Käytä suojauksen tunnusta palvelun osat

Palvelun osia voi luoda vain suojauksen tunnusten jaetun myöntämistä tarvittavat käyttöoikeudet aiemmin kuvatulla käytäntöjen avulla.

Valitse päätepisteet tarjoamia service-Funktiot ovat seuraavat:

| Päätepisteen | Toiminto |
| ----- | ----------- |
| `{iot hub host name}/devices` | Luoda, päivittää, noutaa ja poistaa laitteen käyttäjätietoja. |
| `{iot hub host name}/messages/events` | Laitteen cloud virhesanomia. |
| `{iot hub host name}/servicebound/feedback` | Kerää palautetta cloud laitteen viestit. |
| `{iot hub host name}/devicebound` | Cloud laitteen lähettämiseen. |

Esimerkkinä service-luodaan käyttämällä valmiiksi luotuja jaetun access-käytäntö **registryRead** loisi tunnusta seuraavilla parametreilla:

* resurssin URI: `{IoT hub name}.azure-devices.net/devices`,
* Kirjautuminen avain: jokin avaimet `registryRead` käytäntö
* käytännön nimi: `registryRead`,
* vanhenemisen tahansa.

    var-päätepisteen = "myhub.azure-devices.net/devices";   var-käytännön nimi = laite.   var policyKey = "...";

    var-tunnuksen = generateSasToken (päätepisteen, policyKey, käytännön nimi, 60);

Tulos, jonka myöntää oikeuden lukea kaikki laitteen käyttäjätietoja, on seuraava:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Tuetut X.509-varmenteita

Voit käyttää minkä tahansa X.509-varmenne laitteen todentamismenetelmä IoT toiminnossa. Tämä vaihtoehto sisältää:

-   **X.509-varmenne**. Laite saattaa jo X.509-varmenne, joka on liitetty. Laitteen voi käyttää tätä varmennetta IoT keskittimeen todentamismenetelmä.

-   **X-509 itse luotu ja itse allekirjoitettua varmennetta**. Laitteen valmistajan tai sisäiset deployer voit luoda nämä varmenteet ja tallentaa vastaava yksityinen avain (ja varmenne) laitteeseen. Voit käyttää työkaluja, kuten [OpenSSL] [ lnk-openssl] ja [Windows-SelfSignedCertificate] [ lnk-selfsigned] apuohjelman tähän tarkoitukseen.

-   **CA kirjautunut X.509-varmenne**. Voit käyttää myös X.509-varmenne luodaan ja kirjautunut varmenteiden myöntäjältä (CA) antamaa tunnistaa laitteen ja todentaa laitetta, jonka IoT toiminnossa.

Laitteen voi käyttää X.509-varmenne tai suojaustunnus todennusta, mutta ei molempia.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Rekisteröi X.509-Asiakasvarmenne laitteen

[Azure IoT palvelun SDK: n C#] [ lnk-service-sdk] (versio 1.0.8+) tukee rekisteröiminen laitteella, joka käyttää X.509-Asiakasvarmenne todennusta varten. Muiden laitteiden Tuo/Vie kuten API tukevat X.509 asiakkaan varmenteet.

### <a name="c-support"></a>C\# tuki

**RegistryManager** -luokan avulla ohjelmallisesti Rekisteröi laite. Erityisesti **AddDeviceAsync** ja **UpdateDeviceAsync** menetelmiä käyttöön käyttäjä voi rekisteröidä ja päivittää laitteen rekisterissä Iot keskittimeen laitteen tunnistetiedot. Nämä kaksi menetelmää kestää **laite** -esiintymän syötteenä. **Laitteen** luokkaan kuuluvat **todennus** -ominaisuus, jota käyttäjä voi määrittää ensisijainen ja toissijainen X.509-varmenne thumbprints. Allekirjoitus edustaa SHA-1-hash, X.509-varmenne (tallennettu binaarinen DER koodausta käyttäen). Käyttäjillä on määrittää ensisijainen allekirjoitus tai toissijaisen allekirjoitus tai molempia. Jotta voit käsitellä sertifikaatin palauttaminen tilanteita, joissa tuetaan ensisijainen ja toissijainen thumbprints.

> [AZURE.NOTE] IoT keskittimeen ei vaadi tai tallentaa koko X.509-Asiakasvarmenne, vain allekirjoitus.

Tässä on esimerkki C\# koodikatkelman rekisteröidä käyttävän X.509-Asiakasvarmenne:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Käytä X.509-Asiakasvarmenne suorituksen aikana

[Azure IoT laitteen SDK .NET] [ lnk-client-sdk] (versio 1.0.11+) tukee X.509-asiakasohjelman varmenteet.

### <a name="c-support"></a>C\# tuki

Luokan **DeviceAuthenticationWithX509Certificate** tukee  **DeviceClient** -esiintymien X.509-Asiakasvarmenne luomista.

Tässä on esimerkki koodikatkelman:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Mukautetun laitteen todennus

Voit käyttää IoT keskittimeen [laitteen tunnistetietojen rekisterin] [ lnk-identity-registry] laitteen kohti suojausvaltuudet ja käyttää hallinta [tunnusten][lnk-sas-tokens]. Kuitenkin jos IoT ratkaisu on jo merkittäviä sijoitus mukautetun laitteen tunnistetietojen rekisterin ja/tai todennus värimallin, voit integroida tämän olemassa olevan infrastruktuurin IoT keskittimeen luomalla *Suojaustunnuksen palvelu*. Näin voit käyttää ratkaisu IoT muita ominaisuuksia.

Suojaustunnuksen palvelu on mukautettu pilvipalvelussa. Se käyttää IoT keskittimeen- *jaetun käyttöoikeuskäytäntö* **DeviceConnect** oikeuksilla *laitteen kohdistettu* tunnusten luontia varten. Näiden tunnusten käyttöön laitteeseen, jotta voit muodostaa yhteyden IoT-toiminnossa.

  ![Suojaustunnuksen palvelun kuvion vaiheet][img-tokenservice]

Nämä ovat suojaustunnuksen palvelun kuvion päävaihetta:

1. Luo jaettu IoT keskittimeen käyttöoikeuskäytäntö **DeviceConnect** käyttöoikeuksilla IoT-toiminnossa. Voit luoda tämän käytännön [Azure portal] [ lnk-management-portal] tai ohjelmallisesti. Suojaustunnuksen palvelun Kirjaudu luomilleen tunnusten tämän käytännön avulla.
2. Kun laite on käyttää IoT-toiminnossa, se pyytää allekirjoitetun tunnuksen suojaustunnuksen-palvelusta. Laitteen voi todentaa mukautetun laitteen tunnistetietojen rekisterin/todennus värimallin selvittäminen laitteen, suojaustunnuksen palvelua käytetään luomaan tunnuksen.
3. Suojaustunnuksen-palvelu palauttaa tunnusta. Tunnuksen luodaan käyttäen `/devices/{deviceId}` kuin `resourceURI`, ja `deviceId` on parhaillaan todennettu laite. Suojaustunnuksen palvelu käyttää jaettua käyttöoikeuskäytäntö muodostaa tunnuksen.
4. Laitteen käytetään tunnuksen suoraan IoT-toiminnossa.

> [AZURE.NOTE] Voit käyttää .NET-luokan [SharedAccessSignatureBuilder] [ lnk-dotnet-sas] tai Java-luokan [IotHubServiceSasToken] [ lnk-java-sas] luoda tunnuksen suojaustunnuksen-palvelussa.

Suojaustunnuksen palvelun määrittää tunnuksen vanhentumisen haluamallasi tavalla. Kun tunnuksen vanhenee, IoT-toiminnossa katkaisee yhteyden laite. Valitse laite on pyydettävä uuden tunnuksen suojaustunnuksen-palvelusta. Jos käytät lyhyt voimassaolon ajan, tämä suurentaa laitteen ja suojaustunnuksen palvelun kuormitus.

Laitteen muodostaa yhteyttä keskittimeen, on edelleen lisättävä se IoT keskittimeen laitteen identity-rekisterin, vaikka laite on tunnusta yhteyden ja avulla ei laitteen avainta. Sen vuoksi, voit edelleen käyttää laitteen kohti käyttöoikeuksien hallinta käyttöönoton tai käytöstäpoiston laitteen käyttäjätietojen [IoT keskittimeen tunnistetietojen rekisterin] [ lnk-identity-registry] kun laite todentaa tunnusta. Tämä lieventää riskeistä tunnusten käyttäminen pitkä määräajan kertaa.

### <a name="comparison-with-a-custom-gateway"></a>Mukautettuja yhdyskäytävän vertailu

Suojaustunnuksen palvelun kuvio on suositeltu tapaa Toteuta mukautettu tunnistetietojen rekisterin/todennus-malli IoT keskittimeen kanssa. On suositeltavaa koska IoT keskittimeen säilyy useimmista ratkaisu-liikennettä. On kuitenkin tapauksia, joissa mukautettu tarkistus-malli on niin kiinteässä yhteydessä toisiinsa käsittelyn kaikki liikenne (*mukautettuja yhdyskäytävän*) palvelu on pakollinen-protokollan kanssa. Tämä esimerkki on [Transport Layer Security (TLS) ja ennalta jaetut avaimet (PSKs)][lnk-tls-psk]. Lisätietoja on artikkelissa [protokolla yhdyskäytävän] [ lnk-protocols] aihe.

## <a name="reference-topics"></a>Viittaus aiheet:

Viittaus seuraavissa ohjeaiheissa on lisätietoja IoT-toiminnossa käytön hallinnasta.

## <a name="iot-hub-permissions"></a>IoT pääsivuston käyttöoikeuksia

Seuraavassa taulukossa on lueteltu avulla voit hallita IoT keskittimeen käyttöoikeudet.

| Käyttöoikeudet            | Huomautuksia |
| --------------------- | ----- |
| **RegistryRead**      | Laitteen tunnistetietojen rekisterin myöntää lukuoikeus. Lisätietoja [laitteen tunnistetietojen rekisterin][lnk-identity-registry]. |
| **RegistryReadWrite** | Lue palkkioita ja laitteen tunnistetietojen rekisterin kirjoitusoikeudet. Lisätietoja [laitteen tunnistetietojen rekisterin][lnk-identity-registry]. |
| **ServiceConnect**    | Antaa käyttää cloud palvelun osoittava viestintää ja seuranta päätepisteet. Esimerkiksi myöntää oikeuden taustatietokantaan pilvipalveluihin laitteen cloud virhesanomia, cloud laitteen lähettämiseen ja hakea vastaavan toimituksen vahvistus. |
| **DeviceConnect**     | Laitteen osoittava viestintä päätepisteet myöntää käyttöoikeuksia. Esimerkiksi myöntää oikeuden laitteen cloud Lähetä ja vastaanota cloud laitteen viestejä. Nämä oikeudet käyttävät laitteet. |

## <a name="additional-reference-material"></a>Lisää aineistoa

Kehittäjän opas muiden viittauksen aiheet ovat seuraavat:

- [IoT keskittimeen päätepisteet] [ lnk-endpoints] kuvataan eri päätepisteet, jotka kunkin IoT-toiminnossa paljastaa avaamiseen suorituksenaikainen ja hallintaa varten.
- [Rajoitusten ja kiintiöiden] [ lnk-quotas] kuvataan kiintiön, jotka koskevat IoT keskittimeen-palveluun ja rajoittava toiminnan toiminta palvelua käytettäessä.
- [IoT keskittimeen laite- ja palveluluettelon SDK: T] [ lnk-sdks] näyttää eri kielellä SDK: T voit Käytä tätä, kun laite ja palvelun sovelluksista, jotka toimivat IoT keskittimeen ja kehittää.
- [Kyselyn kielen twins, menetelmistä ja töiden] [ lnk-query] kuvataan kyselykieltä, voit hakea tietoja IoT-toiminnosta laitteen twins, menetelmät ja projektit.
- [IoT keskittimeen MQTT tuki] [ lnk-devguide-mqtt] on lisätietoja IoT keskittimeen tuki MQTT-protokollaa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut käytönvalvonta IoT keskittimeen, voi olla kiinnostuneita Sovelluskehittäjän opas seuraavissa ohjeaiheissa:

- [Synkronoinnin tilan ja määritykset laitteen twins avulla][lnk-devguide-device-twins]
- [Suora menetelmän laitteessa][lnk-devguide-directmethods]
- [Ajoita työt useilla eri laitteilla][lnk-devguide-jobs]

Jos haluat kokeilla osa käsitteistä, joka on kuvattu tämän artikkelin voi olla kiinnostuneita IoT keskittimeen opetusohjelmassa:

- [Azure IoT keskittimeen käytön aloittaminen][lnk-getstarted-tutorial]
- [Cloud laitteen IoT keskittimeen sisältävien viestien lähettäminen][lnk-c2d-tutorial]
- [Voit käsitellä IoT keskittimeen laitteen cloud viestit][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
