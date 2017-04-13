<properties
    pageTitle=".NET-protokollan yleiskatsaus Azure AD | Microsoft Azure"
    description="Tässä artikkelissa käsitellään sallivat access web-sovellusten ja Azure Active Directory-ja OpenID yhteyden vuokraajan ohjelmointirajapinnan web HTTP viestien avulla."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Hyväksy OpenID yhteyden ja Azure Active Directoryn web-sovellusten käyttö

[OpenID yhteyden](http://openid.net/specs/openid-connect-core-1_0.html) on yksinkertainen tunnistetietojen kerroksen laadittuihin OAuth 2.0-protokollan päälle. OAuth 2.0 määrittää järjestelmiä hankkiminen ja käyttää **access tunnusten** suojattuja resursseja, mutta ne eivät määritä tavallisia menetelmiä, anna tunnistetiedot. OpenID yhteyden toteuttaa todennus tiedostotunnistetta OAuth 2.0-todennus prosessi kuin peruskäyttäjän muodossa tietoja tietojen `id_token` , joka vahvistaa käyttäjän sekä käyttäjän basic profiilin tietoja.

Microsoftin suositus OpenID yhteyden on, jos kokoat web-sovelluksen, joka on palvelimessa ja käyttää selaimen kautta.

## <a name="authentication-flow-using-openid-connect"></a>Todennus-työnkulku OpenID yhdistämistä käyttämällä

Yleisin kirjautumisen työnkulku sisältää seuraavat vaiheet - kullekin niistä on kuvattu alla.

![OpenId yhteyden todennus-työnkulku](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Lähetä pyyntö-kirjautuminen

Web-sovelluksen tarvitsee todennetaan, kun se on suora käyttäjän `/authorize` päätepiste. Pyyntö on ensimmäinen pidemmäksi, [OAuth 2.0 luvan koodin Flow](active-directory-protocols-oauth-code.md), joitakin tärkeitä erotettu kanssa:

- Pyynnön on sisällettävä laajuuden `openid` - `scope` parametria.
- `response_type` Parametri on sisällettävä `id_token`.
- Pyynnön on sisällettävä `nonce` parametria.

Jotta malli pyyntö näyttää tältä:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| vuokraajan | pakollinen | `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallittu arvo on vuokraajan tunnukset, kuten `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` tai `contoso.onmicrosoft.com` tai `common` vuokraajan riippumattomalla tunnusten varten |
| client_id | pakollinen | Sovelluksen kun rekisteröityjä Azure AD määritetty sovellustunnus. Voit etsiä perinteinen Azure-portaalissa. Valitse **Active Directory**, valitse kansio, sovellus napsauttamalla ja valitse **määrittäminen** |
| response_type | pakollinen | On sisällettävä `id_token` OpenID yhteyden kirjautumista varten.  Se voi sisältää myös muita response_types, kuten `code`. |
| laajuus | pakollinen | Käyttöalueen tilaa erotettu luettelo.  OpenID Connect-alue on sisällettävä `openid`, joka vastaa suostumusta Käyttöliittymän "Kirjautuminen"-käyttöoikeus.  Voi myös lisätä muita käyttöalueet tämän pyynnön pyydetään lupaa. |
| Nonce-tiedot | pakollinen | Pyyntö sovelluksen, jotka sisällytetään näkyviin luoma sisällytetä arvon `id_token` vaatimus nimellä.  Sovellus voi tarkistaa tämän arvon pienentämään tunnuksen toisto kalastelu.  Arvo on yleensä satunnaisesti ja yksilöivä merkkijono tai GUID-tunnus, joilla voidaan tunnistaa pyynnön alkuperän.  |
| redirect_uri | suositellaan | Sovelluksen, joissa todennus vastaukset voidaan lähettää ja vastaanottaa sovelluksen mukaan redirect_uri.  Se on oltava täsmälleen samat jokin rekisteröidyt portaalissa, mutta sen on oltava url-koodattava redirect_uris. |
| response_mode | suositellaan | Määrittää, jota käytetään lähettää tuloksena authorization_code sovelluksen.  Tuetut arvot ovat `form_post` varten *HTTP-lomakkeen Kirjaa* tai `fragment` *URL-Osoitteen osa*varten.  Verkkosovellusten on suositeltavaa käyttää `response_mode=form_post` tunnusten eniten suojattu siirto-sovellukseen.  
| tila | suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa.  Voi olla merkkijonoksi sisällöstä, jotka haluat.  Satunnaisesti luotu yksilöllinen arvo käytetään yleensä [estää sivustojenvälisen pyynnön väärennös kalastelu](http://tools.ietf.org/html/rfc6749#section-10.12).  Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |
| kehote | Valinnainen | Osoittaa ryhmän tyypin, jota tarvitaan käyttäjän toimia.  Tällä hetkellä vain kelvollisia arvoja ovat "kirjautuminen", "ei" ja "suostumus.  `prompt=login`pakottaa käyttäjä voi syöttää tunnistetietoja pyynnön negating single-etumerkin.  `prompt=none`on päinvastainen toiminto - varmistat, että käyttäjä ei näytetä minkä tahansa muun vuorovaikutteinen kehote.  Jos pyyntö ei voi viimeistellä äänettömästi single-etumerkin kautta, päätepisteen palauttaa virheen.  `prompt=consent`käynnistää OAuth suostumusta-valintaikkunassa sen jälkeen, kun käyttäjä kirjautuu sisään, jossa käyttäjää pyydetään sovelluksen käyttöoikeuksia. |
| login_hint | Valinnainen | Voidaan täyttää valmiiksi sivun käyttäjän kirjautuminen käyttäjänimi/sähköpostin osoite-kenttään jos tiedät henkilön käyttäjänimi etuajassa.  Usein sovellusten käyttää parametriä uudelleen käyttöoikeuden tarkistaminen, onko sinulla jo poimittujen käyttäjänimi edellisen kirjautumisen using aikana `preferred_username` väittää. |


Tässä vaiheessa käyttäjää pyydetään syöttää tunnistetietoja ja suorittaa todennusta.

### <a name="sample-response"></a>Esimerkki vastaus

Esimerkki vastauksen, kun käyttäjä on todennettu, näyttää tältä:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| id_token | `id_token` Sovelluksen pyydettyjen. Voit käyttää `id_token` käyttäjän henkilöllisyys ja Aloita istunto käyttäjän kanssa.  |
| tila | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa. Satunnaisesti luotu yksilöllinen arvo käytetään yleensä [estää sivustojenvälisen pyynnön väärennös kalastelu](http://tools.ietf.org/html/rfc6749#section-10.12).  Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |

### <a name="error-response"></a>Virhe vastausta
Virhe vastaukset myös voidaan lähettää `redirect_uri` niin sovelluksen voit käsitellä niitä oikein:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
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

## <a name="validate-the-idtoken"></a>Vahvista id_token

Vain vastaanottaminen `id_token` ei ole riittäviä todennetaan; on Vahvista allekirjoitus ja tarkista-saatavat `id_token` sinua sovelluksen vaatimukset kohden. Azure AD-päätepisteen käyttää JSON Web tunnusten (JWTs) ja julkisen avaimen salaus Kirjaudu tunnusten ja varmista, että ne ovat kelvollisia.

Voit tarkistaa `id_token` koodi, mutta yleinen käytäntö on asiakkaan Lähetä `id_token` Taustajärjestelmä palvelimeen ja vahvistusta siellä. Kun olet vahvistettu allekirjoituksen `id_token`, on muutamia saatavat, sinun on kirjoitettava vahvistamiseksi.

Voit myös halutessasi tarkistaa muita saatavat mukaan käyttämässäsi skenaariossa. Joitakin yleisiä vahvistukset ovat seuraavat:

- Varmistaa, että käyttäjän tai organisaation on tehnyt sovellus.
- Varmistaa, että käyttäjällä on oikea todennus ja käyttöoikeudet
- Varmistaa, että tiettyjä voimakkuuden todennus on tapahtunut, kuten monimenetelmäisen todentamisen.

Kun kokonaan tarkistaneet `id_token`, voit aloittaa istunnon käyttäjän kanssa ja käyttää-saatavat `id_token` voit hankkia sovelluksen käyttäjän tiedot. Nämä tiedot voidaan näyttää, tietueet, lupa. Lisätietoja suojaustunnuksen tyypit ja saatavat Lue [tuettu suojaustunnuksen ja varaa tyyppejä](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Lähetä pyyntö uloskirjautuminen

Kun haluat kirjautua ulos sovelluksesta käyttäjä, ei ole riittävästi Poista sinua sovelluksen evästeet tai muussa end käyttäjän istunto.  Käyttäjän on luotava uudelleenohjaus myös `end_session_endpoint` , kirjaudu ulos.  Jos et kiellä, käyttäjä voi uudelleen todentaa sovelluksen syöttämättä käyttöoikeutensa uudelleen, koska ne on kelvollinen yksittäisen Sign-istunnon Azure AD-päätepisteen kanssa.

Voit uudelleenohjata riittää, että käyttäjä voi `end_session_endpoint` OpenID yhteyden metatietojen asiakirjan luettelossa:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | suositellaan | URL-osoite, johon käyttäjä uudelleenohjataan jälkeen onnistuneen Kirjaudu ulos.  Jos ei ole käyttäjän näytetään yleinen viestiin.  |

## <a name="token-acquisition"></a>Tunnuksen hankkiminen

Monta web Apps-sovelluksista on paitsi Kirjaudu käyttäjälle, mutta myös käyttää verkkopalvelun OAuth-kyseisen käyttäjän puolesta. Tässä skenaariossa yhdistää OpenID yhteyden aikana samanaikaisesti hankkiminen käyttäjän todennusta varten `authorization_code` , joka voidaan saada `access_tokens` käyttämällä OAuth luvan koodin Flow.

## <a name="get-access-tokens"></a>Hae Access tunnusten

Hankkia Accessin tunnuksia, sinun on muokattava edellä kirjautumisen pyynnön:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Käyttöoikeuksien käyttöalueet sisällyttäminen pyynnön ja käyttämällä `response_type=code+id_token`, `authorize` päätepisteen varmistat, että käyttäjä on hyväksynyt alla käyttöoikeudet `scope` kyselyn parametri ja palauttaa sovelluksen todennus-koodin Exchange käyttöoikeustietue varten.

### <a name="successful-response"></a>Onnistuneiden vastaus

Onnistuneiden vastauksen käyttämällä `response_mode=form_post` näyttää seuraavalta:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| id_token | `id_token` Sovelluksen pyydettyjen. Voit käyttää `id_token` käyttäjän henkilöllisyys ja Aloita istunto käyttäjän kanssa. |
| koodi | Authorization_code, sovellus pyytää. Sovellus pyytää resurssin kohde käyttöoikeustietue todennus-koodin avulla. Authorization_codes hyvin lyhytkestoisia, yleensä ne vanhenevat noin 10 minuuttia. |
| tila | Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |

### <a name="error-response"></a>Virhe vastausta

Virhe vastaukset myös voidaan lähettää `redirect_uri` niin sovelluksen voit käsitellä niitä oikein:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |

Kuvaus virhekoodeja ja niiden suositeltu-toiminto on artikkelissa [virhekoodit luvan päätepisteen virheet](#error-codes-for-authorization-endpoint-errors).

Kun olet saanut luvan `code` ja `id_token`, voit käyttäjän kirjautuminen ja saada access tunnusten heidän puolestaan.  Kirjaudu käyttäjän, sinun on vahvistettava `id_token` täsmälleen yllä olevien ohjeiden mukaisesti. Voit hakea access tunnuksia, voit noudattaa Microsoftin [OAuth-protokollan ohjeissa](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token)kuvatut vaiheet.
