<properties
    pageTitle="Azure Active Directory-B2C | Microsoft Azure"
    description="Web-sovellusten luominen Azure Active Directory-käyttöönoton OpenID yhteyden todennus-protokollan avulla."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory-B2C: OAuth 2.0 luvan koodin työnkulku

OAuth 2.0 luvan koodin myöntäminen voidaan sovelluksissa, jotka asennetaan laitteeseen, saat käyttöösi suojatun resurssit, kuten verkko-ohjelmointirajapinnan. OAuth 2.0 Azure Active Directory (Azure AD) B2C käyttöönoton avulla voit lisätä kirjautuminen sisään ja muiden tunnistetietojen hallintatehtäviä sähköistä tai työpöydän sovelluksia. Tässä oppaassa on itsenäinen kieli. Se käsitellään lähettäminen ja vastaanottaminen HTTP ilman Microsoftin Avaa lähde-kirjastojen avulla.

<!-- TODO: Need link to libraries -->

OAuth 2.0 luvan koodin työnkulku on kuvattu [osassa 4.1 OAuth 2.0-määrityksestä](http://tools.ietf.org/html/rfc6749). Sen avulla voit suorittaa useimpia sovelluksen tyypit, mukaan lukien [web Apps-sovellusten](active-directory-b2c-apps.md#web-apps) ja [asenneta sovellusten](active-directory-b2c-apps.md#mobile-and-native-apps)todennus- ja. Ottaa käyttöön sovellusten hankintaa turvallisesti **access_tokens**, jonka avulla voidaan käyttää resursseja, joiden suojauksessa on [lupa server](active-directory-b2c-reference-protocols.md#the-basics)on.

Tässä oppaassa keskitytään OAuth 2.0 luvan koodin työnkulku--**julkinen asiakkaiden**tietyn sävy. Julkinen asiakas on minkä tahansa asiakassovellus, joka ei voi luottaa pitämään salainen salasanan eheyden suojatusti. Tämä sisältää mobiilisovellukset Työpöytäsovellukset ja melko paljon jokin sovellus, joka toimii laitteessa ja tarvitsee access_tokens. Jos haluat lisätä verkkosovellukseen jäsenyyksien hallinta käyttämällä Azure AD B2C, sinun on käytettävä [OpenID yhteyden](active-directory-b2c-reference-oidc.md) sijaan OAuth 2.0.

Azure AD-B2C laajentaa OAuth 2.0 jatkuu enempää kuin yksinkertainen todennus- ja standardin. Se esittelee [**käytännön parametri**](active-directory-b2c-reference-policies.md), jonka avulla voit käyttää OAuth 2.0 käyttäjän kokemukset lisääminen sovelluksen, kuten kirjautuminen, kirjaudu sisään ja Profiilin hallinta. Tässä Annamme ohjeet käyttämisestä OAuth 2.0 ja käytännöistä toteuttamisesta kunkin nämä kokemukset alkuperäisen-sovelluksissa. Annamme ohjeet myös verkko-ohjelmointirajapinnan käyttäminen access_tokens hankintaohjeet.

Alla olevassa esimerkissä pyyntöjen käytetään otoksen B2C hakemistoa, **fabrikamb2c.onmicrosoft.com**sekä Microsoftin sovelluksen malli ja käytännöt. Olet kokeilla pyynnöt itse, näiden arvoilla tai voit korvata omalla.
Lisätietoja [oman B2C hakemiston, sovellus, ja](#use-your-own-b2c-directory)Opi käytännöt.

## <a name="1-get-an-authorization-code"></a>1. Hae todennus-koodi
Todennus-koodin työnkulku alkaa asiakas, joka ohjaa käyttäjän `/authorize` päätepiste. Tämä on vuorovaikutteinen osa työnkulku-kohtaa, johon käyttäjä todellisuudessa vievät toiminto. Tämä pyyntö asiakkaan osoittaa käyttöoikeudet, jotka tarvitaan hankkimaan käyttäjältä `scope` parametrin ja suorittamaan käytäntö `p` parametri. Kolme esimerkkiä ovat jäljempänä (ja rivinvaihdot luettavuuden), jokaisen eri käytännön avulla.

#### <a name="use-a-sign-in-policy"></a>Kirjaudu sisään käytännön avulla

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Käytä kirjautumisen käytäntöä

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Käytä Muokkaa profiilia-käytäntöä

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parametri | Pakollinen? | Kuvaus |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Pakollinen | Sovelluksen ID, jolla [Azure portal](https://portal.azure.com) myönnetyt sovelluksen. |
| response_type | Pakollinen | Vastaustyyppi, joka sisällyttää `code` luvan koodin kulkuun. |
| redirect_uri | Pakollinen | Sovelluksen, joissa todennus vastaukset voidaan lähettää ja vastaanottaa sovelluksen mukaan redirect_uri. Se on oltava täsmälleen samat jokin redirect_uris, joka on rekisteröity-portaalissa lukuun ottamatta sitä, että se on oltava koodattu URL-osoite. |
| laajuus | Pakollinen | Käyttöalueen tilaa erotettu luettelo. Yhden alueen arvo osoittaa Azure AD molemmat käyttöoikeudet, jotka haetaan. Käyttämällä Asiakastunnus laajuuden ilmaisee, että sovelluksesi tarvitsee **käyttää tunnuksen** , joita voi käyttää omia palvelun tai verkko-Ohjelmointirajapinnan vastaan, joita edustaa sama asiakas.  `offline_access` Laajuus kertoo, että sovellus on **refresh_token** pitkäikäiset käyttää resursseja.  Voit käyttää myös `openid` laajuus **id_token** pyytämisestä Azure AD B2C. |
| response_mode | Suositellaan | Menetelmä, jota käytetään lähettää tuloksena authorization_code sovelluksen. Voi olla jokin seuraavista 'query', 'form_post' tai "fragmentti".
| tila | Suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa. Voi olla merkkijonoksi sisällöstä, jonka haluat. Satunnaisesti luotu yksilöllinen arvo käytetään yleensä estää sivustojenvälisen pyynnön väärennös kalastelu. Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun, valitse siirretyt tai suoritettava käytäntöä. |
| p | Pakollinen | Käytännön, joka suoritetaan. Käytännön, joka on luotu B2C hakemistossa nimi on. Käytännön nimi-arvo olisi alussa on "b2c\_1\_". Lisätietoja [Extensible toimintaperiaatteet](active-directory-b2c-reference-policies.md)käytännöt. |
| kehote | Valinnainen | Käyttäjän toimia, jotka tarvitaan tyyppi. Tällä hetkellä vain kelvollinen arvo on "kirjautuminen", joka pakottaa käyttäjä voi syöttää tunnistetietoja, pyynnön. Kertakirjautumisen tulevat voimaan. |

Tässä vaiheessa käyttäjää pyydetään suorittamaan käytännön työnkulun. Tämä voi liittyä antamalla käyttäjänimi ja salasana, käyttäjän kirjautuminen sosiaalisen tunnistetiedot, kansion tai minkä tahansa muun numeron vaiheet sen mukaan, miten käytäntö määritetään rekisteröityminen.

Käyttäjän päätyttyä käytännön Azure AD palauttaa sovelluksen osoitteessa osoitettu vastauksen `redirect_uri`-menetelmällä määritetyn `response_mode` parametri. Vastaus ovat täsmälleen samat kunkin edellä tapauksissa riippumaton käytännön, joka on suoritettu.

Onnistuneiden vastauksen, joka käyttää `response_mode=query` näyttää seuraavalta:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| koodi | Authorization_code, sovellus pyytää. Sovellus voi pyytää resurssin kohde access_token todennus-koodin avulla. Authorization_codes on hyvin lyhytkestoisia. Yleensä ne päättyy noin 10 minuutin kuluttua. |
| tila | Katso täysi kuvaus edellä olevassa taulukossa. Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |

Virhe vastaukset myös voidaan lähettää `redirect_uri` niin, että sovellus voit käsitellä niitä oikein:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat ohjelmistokehittäjä voi tunnistaa pääkansion syyn todennuksen virhe. |
| tila | Katso tämän osion ensimmäisen taulukon koko kuvaus. Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |


## <a name="2-get-a-token"></a>2. Hae tunnus
Nyt kun olet hankkinut authorization_code, voivat lunastaa `code` , lähettämällä haluamasi resurssin tunnus `POST` pyytää `/token` päätepiste. Azure AD B2C, voit pyytää tunnuksen, vain resurssi on sovelluksen oman Taustajärjestelmä web API. Konferenssi, jota käytetään pyytää tunnuksen itsellesi on käytettävä sinua sovelluksen Ostajantunnus alue:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parametri | Pakollinen? | Kuvaus |
| ----------------------- | ------------------------------- | --------------------- |
| p | Pakollinen | Käytäntö, jota käytettiin hankkia todennus-koodi. Et voi käyttää eri käytäntö kehotusta. Huomautus Tämä parametri lisääminen *kyselymerkkijonon*, ei viestin tekstiosaan. |
| client_id | Pakollinen | Sovelluksen ID, jolla [Azure portal](https://portal.azure.com) myönnetyt sovelluksen. |
| grant_type | Pakollinen | Myönnä, joiden on oltava tyypin `authorization_code` luvan koodin kulkuun. |
| laajuus | Reccommended | Käyttöalueen tilaa erotettu luettelo. Yhden alueen arvo osoittaa Azure AD molemmat käyttöoikeudet, jotka haetaan. Käyttämällä Asiakastunnus laajuuden ilmaisee, että sovelluksesi tarvitsee **käyttää tunnuksen** , joita voi käyttää omia palvelun tai verkko-Ohjelmointirajapinnan vastaan, joita edustaa sama asiakas.  `offline_access` Laajuus kertoo, että sovellus on **refresh_token** pitkäikäiset käyttää resursseja.  Voit käyttää myös `openid` laajuus **id_token** pyytämisestä Azure AD B2C. |
| koodi | Pakollinen | Authorization_code, jotka olet hankkinut kulun ensimmäisen jalan. |
| redirect_uri | Pakollinen | Sovelluksen sait authorization_code redirect_uri. |

Näyttää onnistuneen suojaustunnuksen vastausta seuraavasti:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| not_before | Aika, jolla tunnuksen pidetään kelvollinen kausi samanaikaisesti. |
| token_type | Tunnuksen tyyppi-arvo. Ainoa, joka tukee Azure AD on haltijan. |
| access_token | Allekirjoitetun JSON Web suojaustunnuksen (JWT) tunnus, jonka olet pyytänyt. |
| laajuus | Käyttöalueiden tunnus on voimassa, joita voidaan käyttää välimuistiin tallentaminen tunnusten myöhempää käyttöä varten. |
| expires_in | Ajanjakso, jonka tunnus on voimassa (sekunteina). |
| refresh_token | OAuth 2.0-refresh_token. Sovellus käyttää tunnuksessa hankkia lisää tunnusten nykyisen tunnuksen päätyttyä. Refresh_tokens pitkään olleet ja avulla voidaan säilyttää resurssien käytön pitkän ajan. Tarkemmin Lisätietoja on [B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md). |

Näyttää virheen vastaukset seuraavasti:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat ohjelmistokehittäjä voi tunnistaa pääkansion syyn todennuksen virhe. |

## <a name="3-use-the-token"></a>3. Käytä tunnus
Nyt kun olet hankkinut onnistuneesti `access_token`, voit käyttää tunnuksen pyynnöt, että Taustajärjestelmä web ohjelmointirajapinnan sisällyttämällä sen `Authorization` otsikko:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. Päivitä tunnus
Access & Id tunnukset ovat lyhytkestoisia. Sinun on päivitettävä ne, kun ne päättyy Jatka ei voi käyttää resursseja. Voit tehdä niin lähettämällä toiseen `POST` pyytää `/token` päätepiste. Tällä hetkellä tarjota `refresh_token` sijaan `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parametri | Pakollinen? | Kuvaus |
| ----------------------- | ------------------------------- | -------- |
| p | Pakollinen | Käytäntö, jota käytettiin hankkimaan alkuperäinen refresh_token. Et voi käyttää eri käytäntö kehotusta. Huomautus Tämä parametri lisääminen *kyselymerkkijonon*, ei viestin tekstiosaan. |
| client_id | Suositellaan | Sovelluksen ID, jolla [Azure portal](https://portal.azure.com) myönnetyt sovelluksen. |
| grant_type | Pakollinen | Myönnä, joiden on oltava tyypin `refresh_token` tämän luvan koodin kulun jalan varten. |
| laajuus | Suositellaan | Käyttöalueen tilaa erotettu luettelo. Yhden alueen arvo osoittaa Azure AD molemmat käyttöoikeudet, jotka haetaan. Käyttämällä Asiakastunnus laajuuden ilmaisee, että sovelluksesi tarvitsee **käyttää tunnuksen** , joita voi käyttää omia palvelun tai verkko-Ohjelmointirajapinnan vastaan, joita edustaa sama asiakas.  `offline_access` Laajuus kertoo, että sovellus on **refresh_token** pitkäikäiset käyttää resursseja.  Voit käyttää myös `openid` laajuus **id_token** pyytämisestä Azure AD B2C. |
| redirect_uri | Valinnainen | Sovelluksen sait authorization_code redirect_uri. |
| refresh_token | Pakollinen | Alkuperäinen refresh_token, jotka olet hankkinut kulun toisen jalan. |

Näyttää onnistuneen suojaustunnuksen vastausta seuraavasti:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| not_before | Aika, jolla tunnuksen pidetään kelvollinen kausi samanaikaisesti. |
| token_type | Tunnuksen tyyppi-arvo. Ainoa, joka tukee Azure AD on haltijan. |
| access_token | Allekirjoitetun JSON Web suojaustunnuksen (JWT) tunnus, jonka olet pyytänyt. |
| laajuus | Alueiden tunnus on voimassa, joita voidaan käyttää välimuistiin tallentaminen tunnusten myöhempää käyttöä varten. |
| expires_in | Ajanjakso, jonka tunnus on voimassa (sekunteina). |
| refresh_token | OAuth 2.0-refresh_token. Sovellus käyttää tunnuksessa hankkia lisää tunnusten nykyisen tunnuksen päätyttyä. Refresh_tokens pitkään olleet ja avulla voidaan säilyttää resurssien käytön pitkän ajan. Tarkemmin Lisätietoja on [B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md). |

Näyttää virheen vastaukset seuraavasti:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe. |

## <a name="use-your-own-b2c-directory"></a>Käytä B2C-hakemisto

Jos haluat kokeilla näitä pyynnöt itsellesi, ensin Suorita nämä vaiheet ja korvaa esimerkkiarvoja yläpuolella omalla:

- [Luo B2C-kansio](active-directory-b2c-get-started.md) ja käytä nimeä, pyyntöjä hakemistossa.
- [Sovelluksen luominen](active-directory-b2c-app-registration.md) tunnus ja redirect_uri hankkimiseen. Haluat sisällyttää **native client** sovelluksen.
- Voit hankkia käytännön nimet [luoda oman käytäntöjä](active-directory-b2c-reference-policies.md) .
