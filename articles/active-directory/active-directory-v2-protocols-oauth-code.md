

<properties
    pageTitle="Azure AD v2.0 OAuth luvan koodi työnkulku | Microsoft Azure"
    description="Rakennuksen verkkosovellusten Azure AD-käyttöönoton OAuth 2.0 todennus-protokollan avulla."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>v2.0 protokollat - OAuth 2.0 luvan koodin työnkulku

OAuth 2.0 luvan koodin myöntäminen voidaan sovelluksissa, jotka asennetaan laitteeseen, saat käyttöösi suojatun resurssit, kuten verkko-ohjelmointirajapinnan.  Sovelluksen mallin v2.0 soveltaminen OAuth 2.0 Voit lisätä Sisäänkirjautuminen ja API käyttää sähköistä tai työpöydän sovelluksia.  Tämän oppaan kielestä riippumaton on ja kerrotaan, miten voit lähettää ja vastaanottaa HTTP viestejä ilman Microsoftin Avaa lähde-kirjastojen avulla.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

OAuth 2.0 luvan koodin työnkulku on kuvattu kohdassa [4.1 OAuth 2.0-määrityksestä](http://tools.ietf.org/html/rfc6749).  Sillä voidaan suorittaa todennus- ja sovelluksen tyypit, mukaan lukien [web Apps-sovellusten](active-directory-v2-flows.md#web-apps) ja [asenneta sovellusten](active-directory-v2-flows.md#mobile-and-native-apps)useimpia.  Ottaa käyttöön sovellusten hankintaa turvallisesti access_tokens, joiden avulla voidaan käyttää resursseja, joiden suojauksessa v2.0 päätepiste.  

## <a name="protocol-diagram"></a>Protocol (protokolla)-kaavio
Korkean tason native/mobile-sovelluksen koko todennus-työnkulku seuraavanlainen seuraavasti:

![OAuth Auth koodin työnkulku](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Pyydä todennus-koodi
Todennus-koodin työnkulku alkaa asiakas, joka ohjaa käyttäjän `/authorize` päätepiste.  Tämä pyyntö asiakkaan ilmaisee tarvitsemiaan käyttäjältä hankkimaan käyttöoikeuksia:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Napsauttamalla seuraavaa linkkiä tämän pyyntö! Kun olet kirjautunut sisään, selaimen uudelleenohjataan `https://localhost/myapp/` kanssa `code` osoiteriville.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| vuokraajan | pakollinen | `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallitut arvot ovat `common`, `organizations`, `consumers`, ja vuokraaja tunnukset.  Katso tarkemmin- [protokollan perusteet](active-directory-v2-protocols.md#endpoints). |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| response_type | pakollinen | On sisällettävä `code` luvan koodin kulkuun. |
| redirect_uri | suositellaan | Sovelluksen, joissa todennus vastaukset voidaan lähettää ja vastaanottaa sovelluksen mukaan redirect_uri.  Se on oltava täsmälleen samat jokin rekisteröidyt portaalissa, mutta sen on oltava url-koodattava redirect_uris.  Alkuperäisen ja mobile-sovellusten, sinun täytyy käyttää oletusarvon `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| laajuus | pakollinen | [Alueet](active-directory-v2-scopes.md) , jotka haluat sallia käyttäjän tilaa erotettu luettelo.  |
| response_mode | suositellaan | Määrittää, jota käytetään tuloksena suojaustunnuksen Edellinen lähettäminen sovelluksen.  Voi olla `query` tai `form_post`.  |
| tila | suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa.  Voi olla merkkijonoksi sisällöstä, jotka haluat.  Satunnaisesti luotu yksilöllinen arvo käytetään yleensä [estää sivustojenvälisen pyynnön väärennös kalastelu](http://tools.ietf.org/html/rfc6749#section-10.12).  Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |
| kehote | Valinnainen | Osoittaa ryhmän tyypin, jota tarvitaan käyttäjän toimia.  Tällä hetkellä vain kelvollisia arvoja ovat "kirjautuminen", "ei" ja "suostumus.  `prompt=login`pakottaa käyttäjä voi syöttää tunnistetietoja pyynnön negating single-etumerkin.  `prompt=none`on päinvastainen toiminto - varmistat, että käyttäjä ei näytetä minkä tahansa muun vuorovaikutteinen kehote.  Jos pyyntö ei voi viimeistellä äänettömästi single-etumerkin kautta, v2.0 päätepisteen palauttaa virheen.  `prompt=consent`käynnistää OAuth suostumusta-valintaikkunassa sen jälkeen, kun käyttäjä kirjautuu sisään, jossa käyttäjää pyydetään sovelluksen käyttöoikeuksia. |
| login_hint | Valinnainen | Voidaan täyttää valmiiksi sivun käyttäjän kirjautuminen käyttäjänimi/sähköpostin osoite-kenttään jos tiedät henkilön käyttäjänimi etuajassa.  Usein sovellusten käyttää parametriä uudelleen käyttöoikeuden tarkistaminen, onko sinulla jo poimittujen käyttäjänimi edellisen kirjautumisen using aikana `preferred_username` väittää. |
| domain_hint | Valinnainen | Voi olla jokin seuraavista `consumers` tai `organizations`.  Jos, se ohittaa sähköpostipohjainen etsiminen prosessin kyseisen käyttäjän käy läpi v2.0-kirjautumissivu, valitse johtavat hieman selkeyttää käyttökokemusta.  Usein sovellusten käyttää parametriä uudelleen todennuksen aikana purkamalla `tid` -edellisen kirjautumisnimi.  Jos `tid` väittää arvo on `9188040d-6c67-4c5b-b112-36a304b66dad`, sinun on käytettävä `domain_hint=consumers`.  Muussa tapauksessa käytä `domain_hint=organizations`. |

Tässä vaiheessa käyttäjää pyydetään syöttää tunnistetietoja ja suorittaa todennusta.  V2.0 päätepisteen myös varmistaa, että käyttäjä on hyväksynyt alla käyttöoikeudet `scope` kyselyn parametri.  Jos käyttäjä on ei sitoutunut käyttöoikeudet sitä pyytää käyttäjä suostuu tarvittavat käyttöoikeudet.  Tietoja [käyttöoikeudet, suostumusta, ja usean vuokraajan sovellusten toimitetaan tähän](active-directory-v2-scopes.md).

Kun käyttäjän todentaa ja antaa suostumus v2.0 päätepisteen palauttaa sovelluksen osoitteessa osoitettu vastauksen `redirect_uri`-parametrissa menetelmällä `response_mode` parametrin.

#### <a name="successful-response"></a>Onnistuneiden vastaus
Onnistuneiden vastauksen käyttämällä `response_mode=query` näyttää seuraavalta:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| koodi | Authorization_code, sovellus pyytää. Sovellus pyytää resurssin kohde käyttöoikeustietue todennus-koodin avulla.  Authorization_codes hyvin lyhytkestoisia, yleensä ne vanhenevat noin 10 minuuttia. |
| tila | Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |

#### <a name="error-response"></a>Virhe vastausta
Virhe vastaukset myös voidaan lähettää `redirect_uri` niin sovelluksen voit käsitellä niitä oikein:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Virhekoodit luvan päätepisteen virheet

Seuraavassa taulukossa on kuvattu voi palauttaa-virhekoodit `error` parametri virhe vastausta.

| Virhekoodi | Kuvaus | Asiakastoiminto |
|------------|-------------|---------------|
| invalid_request | Protokollavirhe, kuten tarvittavat funktiosta. | Korjaa ja Lähetä pyyntö. Tämä on kehitys virhe on yleensä pyydettyjen alkuperäinen testauksen aikana.|
| unauthorized_client | Asiakassovellus ei ole sallittua on pyydettävä lupaa-koodi. | Tämä tapahtuu yleensä, kun asiakassovellus ei ole rekisteröity Azure AD- tai ei ole lisätty käyttäjän Azure AD-vuokraajan. Sovellus voi käyttäjää ja asentamalla sovelluksen ja lisäät sen Azure AD-ohjepaketti. |
| käyttö estetty | Resurssien omistajan suostumusta estetty | Asiakassovellus voit ilmoittaa käyttäjälle, että se ei voi jatkaa, ellei käyttäjä suostuu. |
| unsupported_response_type | Todennus-palvelin ei tue vastaustyyppi pyynnön. | Korjaa ja Lähetä pyyntö. Tämä on kehitys virhe on yleensä pyydettyjen alkuperäinen testauksen aikana.|
|server_error | Palvelimen tapahtui odottamaton virhe. | Yritä pyynnön. Nämä virheet voi johtua tilapäinen ehdot. Asiakassovellus voi olla selitys käyttäjälle, että sen vastaus on viivästyy väliaikainen virhe. |
| temporarily_unavailable | Palvelin on tilapäisesti varattu ja käsitellä pyynnön. | Yritä pyynnön. Asiakassovellus voi olla selitys käyttäjälle, että sen vastaus on viivästyy tilapäinen. |
| invalid_resource |Kohteen resurssi on virheellinen, koska sitä ei ole, Azure AD Etkö löydä etsimääsi tai se ei ole määritetty oikein.| Tämä ilmaisee resurssi, jos se on olemassa, ei ole määritetty alihallintaan. Sovellus voi käyttäjää ja asentamalla sovelluksen ja lisäät sen Azure AD-ohjepaketti. |

## <a name="request-an-access-token"></a>Pyydä suojaustunnus
Nyt kun olet hankkinut authorization_code ja on myönnetty käyttöoikeus käyttäjän, voivat lunastaa `code` varten `access_token` haluamasi resurssille lähettämällä `POST` pyytää `/token` päätepisteen:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Kokeile suoritetaan pyyntö Postman! (Älä unohda korvaa `code`)     [ ![Suorittaa Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------------- |
| vuokraajan | pakollinen | `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallitut arvot ovat `common`, `organizations`, `consumers`, ja vuokraaja tunnukset.  Katso tarkemmin- [protokollan perusteet](active-directory-v2-protocols.md#endpoints). |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| grant_type | pakollinen | On oltava `authorization_code` luvan koodin kulkuun. |
| laajuus | pakollinen | Käyttöalueen tilaa erotettu luettelo.  Tämä jalan pyydetty käyttöalueet on oltava vastaaviksi tai alijoukkoa pyydetyt ensimmäisen jalan käyttöalueen.  Jos tämä pyyntö määritettyjen alueiden kattavat useita resurssin palvelimia, v2.0 päätepisteen palauttavat ensimmäisen alueen määritettyä resurssia tunnus.  Saat tarkempia tietoja virhetilanteesta käyttöalueiden viitata [käyttöoikeudet, suostumusta, ja alueet](active-directory-v2-scopes.md).  |
| koodi | pakollinen | Authorization_code, jotka olet hankkinut kulun ensimmäisen jalan.   |
| redirect_uri | pakollinen | Voit hankkia authorization_code käytetty saman redirect_uri arvo. |
| client_secret | pakollinen web Apps-sovellukset | Sovelluksen toiminta, jonka loit sovelluksen rekisteröinti-portaalissa, kun sovellus.  Se olisi ei käytetä alkuperäisen-sovellukseen, koska client_secrets ei voi tallentaa luotettavasti laitteissa.  Se tarvitaan web Apps-sovellusten ja verkko-ohjelmointirajapinnan, jotka voivat tallentaa client_secret suojatusti palvelinpuolen. |

#### <a name="successful-response"></a>Onnistuneiden vastaus
Näyttää onnistuneen suojaustunnuksen vastausta seuraavasti:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| access_token | Pyydetty käyttöoikeustietue. Sovelluksen voi todentaa suojatun resurssi, kuten verkko-Ohjelmointirajapinnan tunnuksessa avulla. |
| token_type | Ilmaisee tunnuksen tyyppi-arvo. Ainoa, joka tukee Azure AD on haltijan  |
| expires_in | Kuinka kauan käyttöoikeustietue on voimassa (sekunteina). |
| laajuus | Vaikutusalueet, joka access_token on voimassa. |
| refresh_token |  Tunnuksen OAuth 2.0-päivityksen. Sovellus käyttää tunnuksessa hankkia lisää käyttöoikeuksia tunnusten nykyisen käyttöoikeustietue päätyttyä.  Refresh_tokens ovat pitkäikäiset ja avulla voidaan säilyttää resurssien käytön pitkän ajan.  Tarkemmin Lisätietoja on [v2.0 suojaustunnuksen viittaus](active-directory-v2-tokens.md).  |
| id_token | Allekirjoittamaton JSON Web suojaustunnuksen (JWT). Sovelluksen can base64Url purkaa tätä tunnuksen käyttäjälle, joka kirjautunut tietojen osia. Sovellus voi välimuistiin arvot ja tuo ne näyttöön, mutta sitä ei kannata ne todennus-ja tietoturva rajat.  Lisätietoja id_tokens Hae [v2.0 päätepisteen tunnuksen viittaus](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Virhe vastausta
Näyttää virheen vastaukset seuraavasti:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |
| error_codes | STS tietyn virhekoodit, jotka auttavat diagnostiikka luettelo.  |
| aikaleima | Aika, jolloin virhe. |
| trace_id | Yksilöllinen tunnus, jotka auttavat diagnostiikka pyynnön.  |
| correlation_id | Yksilöllinen tunnus pyynnön, jotka auttavat diagnostiikka osien välillä. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Virhekoodit suojaustunnuksen päätepisteen virheet

| Virhekoodi | Kuvaus | Asiakastoiminto |
|------------|-------------|---------------|
| invalid_request | Protokollavirhe, kuten tarvittavat funktiosta. | Korjaa ja Lähetä pyyntö |
| invalid_grant | Todennus-koodi on virheellinen tai on vanhentunut. | Kokeile uuden pyynnön `/authorize` päätepiste |
| unauthorized_client | Todennettu asiakas ei ole oikeutta käyttää käyttöoikeuksien myöntäminen tällaista. | Tämä tapahtuu yleensä, kun asiakassovellus ei ole rekisteröity Azure AD- tai ei ole lisätty käyttäjän Azure AD-vuokraajan. Sovellus voi käyttäjää ja asentamalla sovelluksen ja lisäät sen Azure AD-ohjepaketti. |
| invalid_client | Asiakkaan todennus epäonnistui. | Asiakkaan käyttäjätiedot eivät ole kelvollisia. Voit korjata-sovelluksen järjestelmänvalvojan päivittää tunnistetiedot. |
| unsupported_grant_type | Todennus-palvelin ei tue käyttöoikeuksien myöntäminen tyyppi. | Muuttaa myöntäminen pyynnön. Virhe tällaista suoritetaan vain kehityksen aikana ja alkuperäinen testauksen aikana. |
| invalid_resource | Kohteen resurssi on virheellinen, koska sitä ei ole, Azure AD Etkö löydä etsimääsi tai se ei ole määritetty oikein. | Tämä ilmaisee resurssi, jos se on olemassa, ei ole määritetty alihallintaan. Sovellus voi käyttäjää ja asentamalla sovelluksen ja lisäät sen Azure AD-ohjepaketti. |
| interaction_required | Pyyntö edellyttää käyttäjän toimia. Esimerkiksi todennus-vaihe on pakollinen. | Yritä pyyntö, jonka samalle resurssille. |
| temporarily_unavailable | Palvelin on tilapäisesti varattu ja käsitellä pyynnön. | Yritä pyynnön. Asiakassovellus voi olla selitys käyttäjälle, että sen vastaus on viivästyy tilapäinen.|

## <a name="use-the-access-token"></a>Käytä access-tunnus
Nyt kun olet hankkinut onnistuneesti `access_token`, voit käyttää tunnuksen mukaan lukien sen verkko-ohjelmointirajapinnan pyynnöt `Authorization` otsikko:

> [AZURE.TIP] Suorita tämä pyyntö Postman! (Korvaa `Authorization` otsikon ensimmäisen)     [ ![Suorittaa Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Päivitä käyttöoikeustietue
Access_tokens on lyhyt olleet, ja ne täytyy päivittää, kun ne päättyy Jatka käytettäessä resurssien.  Voit tehdä niin lähettämällä toiseen `POST` pyytää `/token` päätepisteen, tällä kertaa antamalla `refresh_token` sijaan `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Kokeile suoritetaan pyyntö Postman! (Älä unohda korvaa `refresh_token`)     [ ![Suorittaa Postman](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | -------- |
| vuokraajan | pakollinen | `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallitut arvot ovat `common`, `organizations`, `consumers`, ja vuokraaja tunnukset.  Katso tarkemmin- [protokollan perusteet](active-directory-v2-protocols.md#endpoints). |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| grant_type | pakollinen | On oltava `refresh_token` tämän luvan koodin kulun jalan varten. |
| laajuus | pakollinen | Käyttöalueen tilaa erotettu luettelo.  Tämä jalan pyydetty käyttöalueet on oltava vastaaviksi tai alijoukkoa pyydetyt alkuperäisen authorization_code pyynnön jalan käyttöalueen.  Jos tämä pyyntö määritettyjen alueiden kattavat useita resurssin palvelimia, v2.0 päätepisteen palauttavat ensimmäisen alueen määritettyä resurssia tunnus.  Saat tarkempia tietoja virhetilanteesta käyttöalueiden viitata [käyttöoikeudet, suostumusta, ja alueet](active-directory-v2-scopes.md).  |
| refresh_token | pakollinen | Refresh_token, jotka olet hankkinut kulun toisen jalan.   |
| redirect_uri | pakollinen | Voit hankkia authorization_code käytetty saman redirect_uri arvo. |
| client_secret | pakollinen web Apps-sovellukset | Sovelluksen toiminta, jonka loit sovelluksen rekisteröinti-portaalissa, kun sovellus.  Se olisi ei käytetä alkuperäisen-sovellukseen, koska client_secrets ei voi tallentaa luotettavasti laitteissa.  Se tarvitaan web Apps-sovellusten ja verkko-ohjelmointirajapinnan, jotka voivat tallentaa client_secret suojatusti palvelinpuolen. |

#### <a name="successful-response"></a>Onnistuneiden vastaus
Näyttää onnistuneen suojaustunnuksen vastausta seuraavasti:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| access_token | Pyydetty käyttöoikeustietue. Sovelluksen voi todentaa suojatun resurssi, kuten verkko-Ohjelmointirajapinnan tunnuksessa avulla. |
| token_type | Ilmaisee tunnuksen tyyppi-arvo. Ainoa, joka tukee Azure AD on haltijan  |
| expires_in | Kuinka kauan käyttöoikeustietue on voimassa (sekunteina). |
| laajuus | Vaikutusalueet, joka access_token on voimassa. |
| refresh_token |  Uuden OAuth 2.0 suojaustunnuksen päivityksen. Vanha päivityksen tunnuksen olisi tilalle juuri luetun päivityksen tunnuksessa varmistamiseksi päivityksen tunnusten voimassa niin kauan.  |
| id_token | Allekirjoittamaton JSON Web suojaustunnuksen (JWT). Sovelluksen can base64Url purkaa tätä tunnuksen käyttäjälle, joka kirjautunut tietojen osia. Sovellus voi välimuistiin arvot ja tuo ne näyttöön, mutta sitä ei kannata ne todennus-ja tietoturva rajat.  Lisätietoja id_tokens Hae [v2.0 päätepisteen tunnuksen viittaus](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Virhe vastausta
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |
| error_codes | STS tietyn virhekoodit, jotka auttavat diagnostiikka luettelo.  |
| aikaleima | Aika, jolloin virhe. |
| trace_id | Yksilöllinen tunnus, jotka auttavat diagnostiikka pyynnön.  |
| correlation_id | Yksilöllinen tunnus pyynnön, jotka auttavat diagnostiikka osien välillä. |

Virhekoodit ja suositeltu toiminnon kuvauksen Katso [virhekoodit suojaustunnuksen päätepisteen virheet](#error-codes-for-token-endpoint-errors).
