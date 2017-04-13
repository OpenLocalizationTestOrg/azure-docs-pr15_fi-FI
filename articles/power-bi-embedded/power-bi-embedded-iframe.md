<properties
   pageTitle="Voit käyttää Power BI upotetun muiden | Microsoft Azure"
   description="Opettele käyttämään Power BI upotetun muiden kanssa "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>Voit käyttää Power BI upotetun muille käyttäjille


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>Power BI-upotettu: Mikä se on ja käyttötarkoitus
Power BI upotetun yhteenveto on kuvattu virallinen [Power BI upotetun sivustossa](https://azure.microsoft.com/services/power-bi-embedded/), mutta voit nopeasti tarkastella ennen on tuoda tiedot käyttämällä muiden kanssa.

On melko helppoa, todella. Itsenäinen Ohjelmistotoimittaja haluaa usein käyttää omia sovelluksessa [Power BI](https://powerbi.microsoft.com) dynaamisen tietojen visualisoinnit Käyttöliittymän rakenneosia.

Mutta tiedät, Power BI-raportteja tai ruutujen upottaminen web-sivulle on jo mahdollista ilman Power BI upotetun Azure-palvelu **Power BI-Ohjelmointirajapinnan**käyttämällä. Jos haluat jakaa raportteja samassa organisaatiossa, voit upottaa raporttien Azure AD-todennuksen kanssa. Kuka näkee raporttien käyttäjän täytyy omalla Azure AD-tilillä kirjautuminen. Jos haluat jakaa raportit kaikkien käyttäjien (mukaan lukien Ulkoiset käyttäjät), voit upottaa vain anonyymin käytön.

Näet, tämä yksinkertainen upottaa ratkaisu, mutta ei aivan tarpeita ISV-sovelluksen.
Useimpien sovellusten tarvitse pitää omia asiakkaat eivät välttämättä oman organisaation käyttäjien tiedot. Esimerkiksi jos pidät yrityksen A ja B yrityksen joitakin palvelun, käyttäjät yrityksen A olisi näkevät vain tietoja oman yrityksen A. Usean vuokraajan tarvitaan, toimitus.

ISV-sovellus voi myös ojentamassa omassa todennusmenetelmät, kuten Lomakepohjainen, basic auth … Tämän jälkeen upottamisen ratkaisu on Työskentele tämän aiemmin todennusmenetelmien turvallisesti. Kannattaa myös tarvittavat käyttäjät voivat käyttää näiden sovellusten ilman ylimääräisiä hankkiminen vai käyttöoikeuksien myöntämistä Power BI-tilaukseen.

 **Power BI upotetun** on suunniteltu tarkasti tällaiset ISV skenaarioita. Niin, että lyhyt esittely on pois tieltä, aloitetaan joidenkin tietojen tuominen

Voit käyttää .NET \(C#) tai Node.js SDK: ssa, voit helposti sovelluksen ja Power BI upotetun. Mutta tässä artikkelissa kerrotaan tietoja HTTP-työnkulku \(mukaan lukien AuthN) ilman SDK: T Power BI. Tietoja tämä työnkulku, voit luoda oman sovelluksen **kanssa tahansa ohjelmointikielellä**ja ymmärrät monitasoisissa Power BI upotetun keskeisimmät.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Luo Power BI työtilan kerääminen ja Hae pikanäppäin \(Provisioning)
Power BI upotetun on yksi Azure palvelut. Käyttömaksut veloitetaan vain ohjelmiston valmistaja käyttää Azure Portal \(kerran tunnissa käyttäjän istunnossa), ja käyttäjä, joka näyttää raportin ei peri tai jopa edellyttää Azure-tilausta.
Ennen kuin aloitat, tutustu sovellusten kehittämisen, emme on luotava **Power BI-työtilan sivustokokoelman** Azure-portaalissa.

Power BI upotetun kunkin työtilan on työtilan kunkin asiakkaan (Alihallinta) ja lisäämme monta työtilat työtilan valikoimien. Sama pikanäppäin käytetään työtilan valikoimien. --Tehosteen, työtilan kokoelma on for Power BI upotetun suojaus-reunaa.

![](media\power-bi-embedded-iframe\create-workspace.png)

Kun on luotu työtilan sivustokokoelman valmiiksi, kopioi pikanäppäin Azure-portaalista.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Syy valmistella työtilan sivustokokoelman ja Hae pikanäppäin REST API kautta. Lisätietoja on artikkelissa [Power BI resurssin tarjoajan API](https://msdn.microsoft.com/library/azure/mt712306.aspx).

## <a name="create-pbix-file-with-power-bi-desktop"></a>Power BI Desktopin .pbix tiedoston luominen
Seuraavaksi on luotava, tietoyhteys ja raportteja voi upottaa.
Tämän tehtävän tai ei ole ohjelmoinnin koodi. Käytämme vain Power BI Desktopin.
Tässä artikkelissa on ei käymällä läpi käyttämisestä Power BI Desktopin tiedot. Jos tarvitset apua tässä, lue [Power BI Desktopin käytön aloittaminen](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/). Esimerkissä käytetään vain [Jälleenmyynti Analysis-malli](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/).

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>Power BI-työtilan luominen

Nyt kun valmistelun kaikki tehdään, aloitetaan asiakkaan työtilan luominen työtilan sivustokokoelman REST API kautta. Seuraavat HTTP POST pyytää (muiden) luodaan uusi työtila Microsoftin aiemmin työtila-kokoelmaan. Tässä esimerkissä työtilan sivustokokoelman nimi on **mypbiapp**.
Olemme juuri määrittäminen access-näppäintä, joka on aiemmin kopioitu, **AppKey**. Se on erittäin yksinkertaisia todennus!

**HTTP-pyyntö**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-vastaus**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

Palautetut **workspaceId** käytetään myöhemmin API-kutsuja. Tutustu sovelluksen on säilytettävä tämän arvon.

## <a name="import-pbix-file-into-the-workspace"></a>Työtilan .pbix-tiedoston tuominen
Kunkin työtilan ylläpitää yhden Power BI Desktopin-tiedosto, jonka tietojoukko \(kuten tietolähde-asetukset) ja raportteja. Olemme tuoda Microsoftin .pbix tiedoston työtilaan alla olevan koodin esitetyllä tavalla. Kuten näet, että ladata .pbix tiedoston MIME multipart HTTP binaariluvuksi.

Uri fragmentti **32960a09-6366-4208-a8bb-9e0678cdbb9d** on workspaceId ja kyselyn parametri **datasetDisplayName** on tietojoukon nimi. Luotu tietojoukko pitää kaikki tiedot liittyvät .pbix palvelutiedot tiedoston siten kuin tuotuja tietoja tietolähteestä, osoittimen jne...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Tuo tämä tehtävä saattaa toimia jonkin aikaa. Kun olet valmis, tutustu sovelluksen esittää tehtävän tilan tarkistaminen Tuontitunnus. Tässä esimerkissä Tuontitunnus on **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

Seuraavassa on kysyy tila tuonti-tunnuksen avulla:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Tehtävää ei ole täydellinen, HTTP-vastaus voi tältä:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Jos tehtävä on valmis, HTTP-vastaus voi olla useita tältä:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Tietolähteen yhteys \(ja tietoja usean vuokraajan)
Kun lähes kaikki .pbix tiedostossa palvelutiedot tuodaan Microsoftin työtilan, tietolähteiden tunnistetiedot eivät ole. Tuloksena käytettäessä **DirectQuery-tilassa**upotetun raporttia ei voi näyttää oikein. Mutta käytettäessä **tuontitila**on tarkastella raportin aiemmin tuotujen tietojen avulla. Tässä tapauksessa on määritettävä seuraavien vaiheiden kautta REST-puhelut tunnistetiedon.

Ensin että saa yhdyskäytävän tietolähde. On tiedettävä tietojoukko **tunnus** on aiemmin palautettu tunnus.

**HTTP-pyyntö**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-vastaus**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

Palautetut yhdyskäytävän tunnuksen ja tietolähteen tunnus \(edellisen **gatewayId** ja palautettu tulos **tunnus** ), emme voi muuttaa tämän tietolähteen tunnistetiedon seuraavasti:

**HTTP-pyyntö**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-vastaus**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

Tuotannon että voit myös määrittää kunkin työtilaan käyttämällä REST API eri yhteysmerkkijonon. \(TS, emme erottaa tietokannan kunkin asiakkaille.)

Seuraavassa on muuttaminen yhteysmerkkijonon tietolähteen kautta muille käyttäjille.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Tai Käytämme rivin suojauksen-Power BI upotetun ja kunkin käyttäjien yhden raportin tiedot erotetaan. Olemme as a Result, valmistella kunkin asiakkaan-raportti, jossa on sama .pbix \(Käyttöliittymä, jne.) ja eri tietolähteistä.

> [AZURE.NOTE] Jos käytät **Tuo tilassa** sijaan **DirectQuery-tilassa**, ei ei voi päivittää mallien Ohjelmointirajapinnan kautta. Ja paikallisen tietolähteet – Power BI-yhdyskäytävä ei ole vielä tueta-Power BI upotetun. Kuitenkin kannattaa todella [Power BI-blogia](https://powerbi.microsoft.com/blog/) , jotta uudet seuraa ja mitä on tulossa tulevaisuudessa versioista.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Todennus ja isännöinnissä Microsoftin web-sivua (upottamisen) raportit

Edellisen REST-Ohjelmointirajapinnalla-on käyttää pikanäppäin **AppKey** itse authorization-otsikko. Koska puhelut voi käsitellä Taustajärjestelmä palvelinpuolen, on turvallista.

Mutta, kun raportti on upottaminen Microsoftin WWW-sivu, tällaisen suojaustiedot käsitellä JavaScript käyttäminen \(frontend). Valitse todennus otsikon arvo on suojattu. Tutustu pikanäppäin havaitaan haittaohjelmien käyttäjä tai haitallisen koodin, jos ne kutsuvat toiminnot tämän avaimen avulla.

Kun raportti on upottaminen Microsoftin WWW-sivu, laskettu tunnus on Käytämme pikanäppäin **AppKey**sijaan. Tutustu sovelluksen on luotava OAuth Json Web-tunnuksen \(JWT) joka koostuu saatavat ja laskettu digitaalinen allekirjoitus. Alla on esitetty OAuth-JWT on piste erotetun koodatun merkkijonon tunnusten.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Microsoft on Valmistele ensin Syöttöarvon, joka on allekirjoitettu myöhemmin. Tämä arvo on base64 url-koodattava (rfc4648)-merkkijono seuraavat json, ja ne on erotettu toisistaan piste \(.) merkin. Kerrotaan myöhemmin hankkiminen raportin tunnusta.

> [AZURE.NOTE] Jos haluamme rivin tasolle Suojaustoiminnon käyttäminen Power BI upotetun, emme on myös määritettävä **käyttäjänimi** ja **roolit** -saatavat.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Seuraavaksi on luotava HMAC base64 koodatun merkkijonon \(allekirjoitus) SHA256 algoritmin kanssa. Allekirjoitetun syötteen arvo on edellisen merkkijonon.

Viimeksi, emme täytyy yhdistää käyttämällä kauden syötteen arvo ja allekirjoituksen merkkijonon \(.) merkin. Valmiit merkkijono on sovelluksen tunnus raportin upottamista. Vaikka joku löydetyt app tunnuksen ne eivät pääse alkuperäisen pikanäppäin. Tämän sovelluksen tunnuksen päättyy nopeasti.

Seuraavassa on PHP Esimerkki varten seuraavasti:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Raportin upotat-web-sivu

Upottamiseen Microsoftin raportin syy Upota URL-osoitteen hankkiminen ja raportin **tunnus** REST-Ohjelmointirajapinnalla.

**HTTP-pyyntö**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-vastaus**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

Olemme upottaa raportin meidän web Appin edellisen sovelluksen tunnuksen.
Jos Odotamme seuraava näyte koodia, entisen osa on sama kuin edellisessä esimerkissä. Jälkimmäisessä-osassa tässä esimerkissä **embedUrl** \(edellisen tulos) IFRAME- ja sovelluksen tunnus on kirjaaminen iframe kyselyjä.

> [AZURE.NOTE] Tarvitset muutettava kuvaksi raportin tunnus-arvon. Lisäksi sisällönhallinta järjestelmään virheen vuoksi koodin otoksessa iframe-tunniste on luku literaaleina. Poista capped teksti-tunnisteen, jos kopioit ja liität otoksen koodi.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

Ja tässä on Microsoftin tulos:

![](media\power-bi-embedded-iframe\view-report.png)

Tällä hetkellä Power BI upotetun vain näyttää raportin IFRAME. Mutta seuraa [Power BI-blogi](). Tulevien parannukset voi käyttää uuden asiakaspuolen API, joka uutiskirjeistä Lähetä iframe tiedot sekä kerää tietoja. Mielenkiintoinen kohteiden!


## <a name="see-also"></a>Katso myös
- [Käyttöoikeudet ja myöntää-Power BI upotetun](power-bi-embedded-app-token-flow.md)
