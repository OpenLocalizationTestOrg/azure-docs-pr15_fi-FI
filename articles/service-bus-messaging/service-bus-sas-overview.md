<properties
    pageTitle="Jaettu Access allekirjoitukset yleiskatsaus | Microsoft Azure"
    description="Mikä on jaettu Access allekirjoitukset, miten ne toimivat, ja kuinka niitä käytetään solmu, PHP ja C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Jaettu käyttö allekirjoitukset

*Jaettu Access allekirjoitukset* (SAS) on ensisijainen suojauskäytäntö järjestelmä palvelun Bus, kuten tapahtuman keskittimet, se messaging (olevien ja aiheet), ja välittäminen messaging. Tässä artikkelissa käsitellään jaettu Access allekirjoitukset, miten ne toimivat ja käyttämisestä vielä ympäristö ympäristöstä riippumattomalla tavalla.

## <a name="overview-of-sas"></a>SAS yleiskatsaus

Jaetun Access-allekirjoitukset ovat SHA-256 suojatun hajautusarvot tai URI todennus-järjestelmä. SAS on hyvin tehokas tapa, jota käytetään kaikkien palvelun Bus-palvelun. Todellinen käytössä Suojaussidokset on kaksi komponenttia: *Jaettu käyttöoikeuskäytäntö* ja *Jakaa Access allekirjoitus* (usein kutsutaan *tunnus*).

Löydät jaettu Access allekirjoitukset palvelun Bus tarkempia tietoja [palvelun Bus jaettu Access allekirjoituksen todentaminen](service-bus-shared-access-signature-authentication.md).

## <a name="shared-access-policy"></a>Jaetun käyttöoikeuskäytäntö

Tietoja SAS ymmärtää tärkeintä on, että kaikki alkaa käytännön. Jokaisen käytännön voit päättää kolme kappaletta tietojen: **nimi**, **laajuuteen**ja **käyttöoikeudet**. **Nimi** on juuri tätä; alueen sisällä yksilöllinen nimi. Alue on tarpeeksi helposti: on kyseisen resurssin URI. Palvelun Bus-nimitilan alue on täydellinen toimialuenimi (FQDN), kuten `https://<yournamespace>.servicebus.windows.net/`.

Käytännön käytettävissä olevat käyttöoikeudet ovat suurelta selkeitä:

  + Lähetä
  + Kuuntele
  + Hallinta

Kun olet luonut käytännön, se on määritetty *Perusavaimen* ja *Toissijaisen avaimen*. Nämä ovat salaustavalla vahva avaimet. Et menetä ne ja paljastaa ne – ne aina on käytettävissä [Azure portal][]. Voit käyttää joko luotu näppäimet ja luo ne milloin tahansa. Kuitenkin jos uudelleen tai muuttaa käytännön perusavaimen, mitätöidään kaikki jaetut Access allekirjoitukset, se on luotu.

Kun luot palvelun Bus nimitilan, käytännön luodaan automaattisesti koko nimitilan, jota kutsutaan **RootManageSharedAccessKey**ja käytäntö on kaikki käyttöoikeudet. Älä Kirjaudu sisään kuin **ylimmällä tasolla**, jotta et käytä tämän käytännön, ellet ole erinomaisen hyvä syytä. Voit luoda uusia käytännöt portaalissa nimitilan **määritys** -välilehti. On tärkeää Huomautus yksittäinen puun palvelun Bus tason (nimitilan, jono, tapahtuma-toiminnossa jne.) voi olla vain 12 käytännöt, jotka on liitetty.

## <a name="shared-access-signature-token"></a>Jaetun Access-allekirjoitus (tunnus)

Itse käytäntö ei ole käyttöoikeustietue palvelun Bus varten. Kyseessä objekti, josta käyttöoikeustietue luodaan - käyttämällä joko ensisijaisen tai toissijaisen-näppäintä. Tunnuksen luo työstämistä huolellisesti merkkijonon seuraavanlainen:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Jossa `signature-string` on SHA-256 hash tunnuksen (**laajuus** edellisessä kohdassa kuvatulla tavalla) laajuuden CRLF, joka on lisätty ja voimassaolon ajan (sekunteina jälkeen kausi: `00:00:00 UTC` -1 tammikuussa 1970).

Hajautuksen seuraavankaltaiselta seuraavat pseudokoodiksi ja palauttaa 32 tavua.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

Hajautusalgoritmilla-arvot ovat **SharedAccessSignature** merkkijonon niin, että vastaanottaja voi laskea hash saman parametreilla, varmista, että se palauttaa saman tuloksen. URI määrittää laajuuden ja avaimen nimi tunnistaa hajautuksen laskemiseen käytettävä käytännön. Tämä on tärkeää-suojauksen kannalta. Jos haluamasi allekirjoitus ei vastaa, joka laskee vastaanottaja (Service Bus)-käyttö on estetty. Tässä vaiheessa voit varmistaa lähettäjän oli käyttää avainta ja olisi myönnettävä käytännön oikeuksiin.

## <a name="generating-a-signature-from-a-policy"></a>Allekirjoituksen luonnissa käytännöstä

Kuinka todella teet tämän koodissa? Voit tarkastella joitakin näistä.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>K & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>Jaettu Access allekirjoitusten käyttäminen (HTTP tasolla)
 
Nyt kun osaat luoda jaettu Access allekirjoituksia palvelun Bus kaikki kohteet, olet valmis suorittamaan HTTP POST:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Muista, että tämä koskee kaikki. Voit luoda SAS jonon, aiheen, tilauksen, tapahtuma-toiminto tai välitys. Jos käytät-publisher tunnistetietojen tapahtuman keskittimien, riittää, että liität `/publishers/< publisherid>`.

Jos annat lähettäjä tai asiakkaan SAS-tunnuksen, hänellä ei ole avaimen suoraan ja niitä voi kumota hajautuksen hankkia sen. Sellaisenaan hallitset he voivat avata ja kuinka kauan. Tärkeää muistaa on, jos muutat perusavaimen käytännön, kaikki jaetut Access allekirjoitukset, se on luotu olla mitätöidään.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>Jaettu Access allekirjoitusten käyttäminen (AMQP tasolla)

Edellisessä osassa tuli SAS tunnuksen käyttäminen HTTP POST-pyynnön palvelun Bus tietojen lähettämistä varten. Tiedät, voit käyttää palvelun Bus Advanced viestin Queuing Protocol (AMQP), joka on käytettävä nopeuttamiseksi monissa tilanteissa ensisijainen protokolla avulla. SAS suojaustunnuksen käyttötapa ja AMQP on kuvattu asiakirjan [1.0 AMQP Claim-Based Security versio](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) , joka on luonnos työpäivän jälkeen 2013, mutta hyvin ylläpitämä Azure tänään.

Ennen kuin aloitat tietojen lähettäminen palvelun Bus, julkaisija on lähetettävä SAS tunnuksen sisällä AMQP viesti on hyvin määritetyn AMQP solmu nimeltä **$cbs** (näet sen käytettävä palvelun hankkia ja vahvista SAS tunnusten "määräten" jono nimellä). Julkaisija on määritettävä **ReplyTo** kentän sisälle AMQP sanoman. Tämä on solmu, jossa palvelun vastaukset Publisher tuloksella suojaustunnuksen kelpoisuuden (simple pyynnön ja vastauksen kuvion publisher ja palvelun välillä). Vastaa solmu luodaan "tekeminen nopeasti," puhua tietoja "dynaaminen luomisesta remote solmu" AMQP 1.0-määritysten mukaisesti. Tarkistettuaan SAS tunnuksen kelpaa julkaisijan voit eteenpäin ja tietojen lähettäminen palvelun alkaa.

Seuraavissa vaiheissa kuvataan lähettäminen SAS tunnuksen käyttämällä [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) kirjaston AMQP-protokollan kanssa. Tämä on kätevä, jos et voi käyttää virallinen palvelun Bus SDK (esimerkiksi WinRT, .net-Compact Framework, .net Micro Frameworkiin ja Mono) kehittäminen c\#. Tämä kirjasto on hyödyllisiä tietoja siitä, miten väitepohjaista suojaus toimii AMQP tasolla, kuten näyttöön tuli toiminta tasolla HTTP (HTTP POST-pyynnön ja lähetetty sisällä "Authorization"-otsikko SAS tunnus). Jos et tarvitse tällaista laaja tietoa AMQP tietoja, voit käyttää virallinen palvelun Bus SDK-paketissa .net Framework-sovellukset, joka tekee sen puolestasi.

### <a name="c35"></a>K & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

`PutCbsToken()` Menetelmä saa *yhteyttä* (AMQP yhteyden luokan esiintymän tarkoitetun [AMQP .NET Lite kirjaston](https://github.com/Azure/amqpnetlite)), joka edustaa TCP-yhteyden palveluun ja *sasToken* -parametri on lähettää SAS-tunnuksen. 

> [AZURE.NOTE] On tärkeää, että yhteys luodaan **SASL todennus järjestelmä asettaminen ulkoinen** (sekä ei oletusarvoisesti vain käyttäjänimellä ja salasanalla käytetään, kun sinun ei tarvitse lähettää SAS-tunnuksen).

Seuraavaksi julkaisijan Luo kaksi AMQP linkkiä SAS tunnuksen lähettämiseen ja vastaanottamiseen vastaa (tulos suojaustunnuksen vahvistus)-palvelusta.

AMQP viesti sisältää tietyt ominaisuudet ja enemmän tietoja kuin yksinkertaisen viestin. SAS tunnus on (joko sen konstruktori) viestin tekstiosaan. **"ReplyTo"** -ominaisuudeksi on määritetty Solmunimi vastaanottaminen vahvistus tulos (nimessä voi muuttaa, jos haluat, ja se luodaan dynaamisesti palvelun) Vastaanottaja-linkkiä. Kolme viimeisintä sovelluksen/mukautettuja ominaisuuksia käytetään palvelun ilmaisemaan, millaisia toiminto on suorittamiseen. Ohjeiden mukaan CBS luonnos määrityksen, niiden on oltava **toiminnon nimi** ("hyllytetty-tunnuksen"), **tunnuksen tyyppi** (Tässä tapauksessa "servicebus.windows.net:sastoken") ja jota tunnuksen koskee (koko kohteen) **Käyttäjäryhmän "nimi"** .

Lähettämisestä SAS tunnuksen lähettäjä-linkin julkaisija on luku vastaa vastaanottaja-linkkiä. Vastaus on yksinkertainen AMQP viesti, jossa nimeltä **"tilakoodi"** , joka voi olla samat arvot kuin HTTP-tilakoodin sovelluksen-ominaisuus. 

## <a name="next-steps"></a>Seuraavat vaiheet

Hae [palvelun Bus REST API-viittaus](https://msdn.microsoft.com/library/azure/hh780717.aspx) on lisätietoja siitä, mitä voit tehdä SAS näiden tunnusten.

Saat lisätietoja palvelun Bus todennus- [palvelun Bus todennus- ja](service-bus-authentication-and-authorization.md). 

Lisää esimerkkejä SAS C# ja Java-komentosarja on [blogikirjoituksessa](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx).

[Azure portal]: https://portal.azure.com