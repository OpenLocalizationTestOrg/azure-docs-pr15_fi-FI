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

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory-B2C: WWW-kirjautuminen OpenID Connect-sovelluksessa

OpenID yhdistäminen on todennusprotokolla, laadittuihin OAuth 2.0, joita voidaan käyttää käyttäjien kirjautumalla suojatusti verkkosovellusten päälle.  Azure Active Directory (Azure AD) B2C soveltaminen OpenID yhteyden käyttämällä voit ulkoistaa kirjautumisen, kirjaudu sisään ja muut käyttäjähallinnan ilmenee web-sovellusten Azure AD. Tässä oppaassa kerrotaan, kuinka voit tehdä kielen riippumattomalla tavalla. Se kuvataan lähettäminen ja vastaanottaminen HTTP ilman Microsoftin Avaa lähde-kirjastojen avulla.

[OpenID yhteyden](http://openid.net/specs/openid-connect-core-1_0.html) laajentaa *todennus* -protokolla käyttää OAuth 2.0 *todennus* -protokollaa. Voit suorittaa kertakirjautumisen OAuth käyttämällä. Se esittelee käsite `id_token`, joka on suojaustunnus, jonka avulla voit vahvistaa käyttäjän tunnistetiedot ja hankkia perusprofiilitiedot käyttäjästä asiakasohjelmat.

Koska se ulottuu OAuth 2.0-lisäksi sovellusten hankintaa suojatusti **access_tokens**avulla. Voit käyttää access_tokens access-resursseja, joiden suojauksessa on [lupa server](active-directory-b2c-reference-protocols.md#the-basics)on. Suosittelemme OpenID yhteyden, jos luot web-sovelluksen, joka on palvelimessa ja käyttää selaimen kautta. Jos haluat lisätä jäsenyyksien hallinta matkapuhelimella tai työpöydän sovellustesi käyttämällä Azure AD B2C käytettävä [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) OpenID yhteyden sijaan.

Azure AD-B2C laajentaa vakio OpenID yhteyden protokolla enempää kuin yksinkertainen todennus- ja. Se esittelee [**käytännön parametri**](active-directory-b2c-reference-policies.md), jonka avulla voit käyttää käyttäjän kokemukset lisääminen sovelluksen – kuten OpenID yhteyden kirjautuminen, kirjaudu sisään ja Profiilin hallinta. Seuraavassa esitellään että toteuttaa kaikki nämä kokemukset web-sovellusten OpenID yhteyden ja käytännöistä avulla. Myös esitellään että verkko-ohjelmointirajapinnan käyttäminen access_tokens hankintaohjeet.

Alla olevassa esimerkissä pyyntöjen käytetään otoksen B2C hakemistoa, **fabrikamb2c.onmicrosoft.com**sekä Microsoftin otoksen sovelluksen **https://aadb2cplayground.azurewebsites.net** ja käytännöt. Maksuttomia tutustua pyynnöt itse avulla nämä arvot tai voit korvata omalla.
Lisätietoja [oman B2C vuokraajan, sovellus- ja käytännöistä](#use-your-own-b2c-directory)hankkiminen.

## <a name="send-authentication-requests"></a>Lähetä käyttöoikeuksien
Kun käyttäjä todennetaan ja suorita käytännön web Appissa, se ohjata käyttäjän `/authorize` päätepiste. Tämä on vuorovaikutteinen osa virtaus-kohtaa, johon käyttäjä todellisuudessa vievät toiminto-käytännön mukaan.

Tämä pyyntö asiakkaan osoittaa käyttöoikeudet, jotka tarvitaan hankkimaan käyttäjältä `scope` parametrin ja suorittamaan käytäntö `p` parametri. Kolme esimerkkiä ovat jäljempänä (ja rivinvaihdot luettavuuden), jokaisen eri käytännön avulla. Kokeile, sivupyynnön toiminta, yritä pyynnön liittäminen selaimessa ja suorittaa.

#### <a name="use-a-sign-in-policy"></a>Käytä käytäntöä-kirjautuminen

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Käytä kirjautumisen käytäntöä

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Käytä Muokkaa profiilia-käytäntöä

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parametri | Pakollinen? | Kuvaus |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Pakollinen | Sovelluksen ID, jolla [Azure portal](https://portal.azure.com/) myönnetyt sovelluksen. |
| response_type | Pakollinen | Vastaustyyppi, joka sisällyttää `id_token` OpenID Connect. Jos web Appissa on myös tunnusten soitettavien verkko-Ohjelmointirajapinnan, voit käyttää `code+id_token`, että olet tehnyt tässä. |
| redirect_uri | Suositellaan | Sovelluksen, joissa todennus vastaukset voidaan lähettää ja vastaanottaa sovelluksen mukaan redirect_uri. Se on oltava täsmälleen samat jokin redirect_uris, joka on rekisteröity-portaalissa lukuun ottamatta sitä, että se on oltava koodattu URL-osoite. |
| laajuus | Pakollinen | Käyttöalueen tilaa erotettu luettelo. Yhden alueen arvo osoittaa Azure AD molemmat käyttöoikeudet, jotka haetaan. `openid` Laajuus osoittaa oikeuden käyttäjän sisäänkirjautuminen ja käytön **id_tokens** (Lisää on tulossa tästä artikkelin) lomakkeen käyttäjän tietoja. `offline_access` Alue on valinnainen verkkosovelluksissa. Kertoo, että sovellus on **refresh_token** pitkäikäiset käyttää resursseja. |
| response_mode | Suositellaan | Menetelmä, jota käytetään lähettää tuloksena authorization_code sovelluksen. Voi olla jokin seuraavista 'query', 'form_post' tai "fragmentti".  'form_post' suositellaan suojausta. |
| tila | Suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa. Voi olla merkkijonoksi sisällöstä, jonka haluat. Satunnaisesti luotu yksilöllinen arvo käytetään yleensä estää sivustojenvälisen pyynnön väärennös kalastelu. Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi siirretyt-sivu. |
| Nonce-tiedot | Pakollinen | Pyynnön (luomien sovelluksen), jotka sisällytetään tuloksena id_token kuin vaatimus sisältyvät arvo. Sovellus voi tarkistaa tämän arvon pienentämään tunnuksen toisto kalastelu. Arvo on yleensä satunnaisesti yksilöllinen merkkijono, joilla voidaan tunnistaa pyynnön alkuperän. |
| p | Pakollinen | Käytännön, joka suoritetaan. Käytännön, joka on luotu B2C vuokraajan nimi on. Käytännön nimi-arvo olisi alussa on "b2c\_1\_". Lisätietoja [Extensible toimintaperiaatteet](active-directory-b2c-reference-policies.md)käytännöt. |
| kehote | Valinnainen | Käyttäjän toimia, jotka tarvitaan tyyppi. Tällä hetkellä vain kelvollinen arvo on "kirjautuminen", joka pakottaa käyttäjä voi syöttää tunnistetietoja, pyynnön. Kertakirjautumisen tulevat voimaan. |

Tässä vaiheessa käyttäjää pyydetään suorittamaan käytännön työnkulun. Tämä voi liittyä antamalla käyttäjänimi ja salasana, käyttäjän kirjautuminen sosiaalisen tunnistetiedot, kansion tai minkä tahansa muun numeron vaiheet sen mukaan, miten käytäntö määritetään rekisteröityminen.

Käyttäjän päätyttyä käytännön Azure AD palauttaa sovelluksen osoitteessa osoitettu vastauksen `redirect_uri`-menetelmällä, joka on määritetty `response_mode` parametri. Vastaus ovat täsmälleen samat kunkin edellä tapauksissa riippumaton käytännön, joka on suoritettu.

Onnistuneiden vastauksen käyttämällä `response_mode=fragment` näyttää:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| id_token | Id_token, sovellus pyytää. Voit käyttää id_token käyttäjän henkilöllisyys ja Aloita istunto käyttäjän kanssa. Lisätietoja id_tokens ja niiden sisältöä sisältyvät [Azure AD B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md). |
| koodi | Authorization_code, sovellus pyytää, jos olet käyttänyt `response_type=code+id_token`. Sovellus voi pyytää resurssin kohde access_token todennus-koodin avulla. Authorization_codes on hyvin lyhytkestoisia. Yleensä ne päättyy noin 10 minuutin kuluttua. |
| tila | Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |

Virhe vastaukset myös voidaan lähettää `redirect_uri` niin, että sovellus voit käsitellä niitä oikein:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe. |
| tila | Katso täysi kuvaus edellä olevassa taulukossa. Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |


## <a name="validate-the-idtoken"></a>Vahvista id_token
Vastaanottaminen juuri id_token ei ole tarpeeksi todennetaan--on Vahvista id_token allekirjoitus ja tarkista saatavat-tunnuksen lisääminen sovelluksen vaatimukset mukaan. Azure AD-B2C käyttää [JSON Web tunnusten (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ja julkisen avaimen salaus Kirjaudu tunnusten ja varmista, että ne ovat kelvollisia.

On monia Avaa lähde-kirjastoja, jotka ovat käytettävissä vahvistaminen JWTs, ylemmäksi kielen mukaan. On suositeltavaa tutustuminen asetuksia kuin käyttöönoton oman vahvistus-logiikkaa. Tietoja tästä on hyötyä käyttäminen käyttämisestä oikein nämä kirjastot.

Azure AD-B2C on OpenID yhteyden metatietojen päätepisteen, joka sallii sovelluksen hakeaksesi tietoja Azure AD B2C suorituksen aikana. Näitä tietoja ovat päätepisteet suojaustunnuksen sisältö ja kirjautuminen näppäimet tunnuksen. Tällä JSON metatietojen asiakirjan B2C vuokraajan käytäntöjen. Esimerkiksi metatietojen asiakirjaa varten `b2c_1_sign_in` käytännön `fabrikamb2c.onmicrosoft.com` sijaitsee:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Yksi määritys-tiedoston ominaisuuksia `jwks_uri`, joiden arvo olisi sama käytäntö:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Valitse tilaus määrittämään mitä käytäntöä käytettiin id_token allekirjoitus (ja hakeaksesi metatiedot mistä), käytettävissä on kaksi vaihtoehtoista. Ensin käytännön nimen sisältyy `acr` id_token väittää. Lisätietoja jäsentää kuin id_token saatavat Hae [Azure AD B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md). Muu vaihtoehto on koodata arvon käytäntö `state` parametrin antaa pyynnön ja toistaa sen voit selvittää, mitä käytäntöä käytettiin. Toinen tapa on täysin kelvollinen.

Kun olet hankkinut metatietojen asiakirja päätepisteestä OpenID yhteyden metatietoja, RSA 256 julkinen avain (joka on tämän päätepisteen) voit tarkistaa id_token allekirjoitus. Voi olla useita näppäimiä, jotka on lueteltu tämän päätepisteen vaiheissa on aika, kunkin tunnistaa `kid`. Otsikko id_token sisältää myös `kid` väittää, joka osoittaa, mitä painettavat näppäimet on käytetty kirjautua id_token. Hae [Azure AD B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md) lisätietoja, kuten osien [vahvistaminen tunnusten](active-directory-b2c-reference-tokens.md#validating-tokens) ja [Kirjautuminen avaimen palauttaminen tärkeitä tietoja](active-directory-b2c-reference-tokens.md#validating-tokens).
<!--TODO: Improve the information on this-->

Kun olet vahvistettu id_token allekirjoitus, on useita saatavat, jotka haluat tarkistaa, esimerkiksi:

- Sinun kannattaa tarkistaa `nonce` väittää estää tunnuksen toisto kalastelu. Arvon on oltava Kirjaudu sisään-pyynnössä määritetty.
- Sinun kannattaa tarkistaa `aud` väittää varmistaa, että id_token luotiin, kun sovellus. Arvon on oltava sovelluksesi Sovellustunnus.
- Sinun kannattaa tarkistaa `iat` ja `exp` väittää varmistaa, että id_token ei ole vanhentunut.

Saatavilla on myös useita vahvistukset, tee, kuvataan tarkemmin [OpenID yhteyden Core määritys](http://openid.net/specs/openid-connect-core-1_0.html).  Voit myös tarkistaa muita saatavat, että toimintamallin. Joitakin yleisiä vahvistukset ovat seuraavat:

- Varmistetaan, että käyttäjän tai organisaation on tehnyt sovellus.
- Varmistetaan, että käyttäjällä on oikea todennus ja käyttöoikeudet.
- Varmistetaan, että tiettyjä voimakkuuden todennus on tapahtunut, kuten Azure Monimenetelmäisen todentamisen.

Lisätietoja id_token saatavat Hae [Azure AD B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md).

Kun kokonaan on vahvistettu id_token, voit aloittaa istunnon käyttäjän kanssa ja hankkia sovelluksen käyttäjän tiedot id_token saatavat avulla. Nämä tiedot voidaan näyttää, tietueet, todennus ja niin edelleen.

## <a name="get-a-token"></a>Hae tunnus
Jos kaikki web app tavalla voit tehdä on suorittaa käytäntöjä, voit ohittaa seuraavan joitakin osia. Nämä osat ovat vain koskevat WWW-sovellukset, jotka täytyy tehdä verkko-Ohjelmointirajapinnan puhelut todennus ja Azure AD B2C ovat käytössä.

Voivat lunastaa authorization_code, joka on hankittu (käyttämällä `response_type=code+id_token`), lähettämällä haluamasi resurssin tunnus `POST` pyytää `/token` päätepiste. Tällä hetkellä vain resurssi, voit pyytää tunnus on sovelluksen oman Taustajärjestelmä web API. Tunnuksen itsellesi pyytää nimetään käytettävä sinua sovelluksen Ostajantunnus alue:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parametri | Pakollinen? | Kuvaus |
| ----------------------- | ------------------------------- | --------------------- |
| p | Pakollinen | Käytäntö, jota käytettiin hankkia todennus-koodi. Et voi käyttää eri käytäntö kehotusta. Huomautus Tämä parametri lisääminen **kyselymerkkijonon**, ei viestin tekstiosaan. |
| client_id | Pakollinen | Sovelluksen ID, jolla [Azure portal](https://portal.azure.com/) myönnetyt sovelluksen. |
| grant_type | Pakollinen | Myönnä, joiden on oltava tyypin `authorization_code` luvan koodin kulkuun. |
| laajuus | Suositellaan | Käyttöalueen tilaa erotettu luettelo. Yhden alueen arvo osoittaa Azure AD molemmat käyttöoikeudet, jotka haetaan. `openid` Laajuus osoittaa oikeuden käyttäjän sisäänkirjautuminen ja käytön **id_tokens**lomakkeen käyttäjän tietoja. Sen avulla voidaan saada tunnusten sovelluksen oman Taustajärjestelmä sivustoosi API, joka edustaa on sama kuin asiakas Sovellustunnus. `offline_access` Laajuus kertoo, että sovellus on **refresh_token** pitkäikäiset käyttää resursseja. |
| koodi | Pakollinen | Authorization_code, jotka olet hankkinut kulun ensimmäisen jalan. |
| redirect_uri | Pakollinen | Sovelluksen sait authorization_code redirect_uri. |
| client_secret | Pakollinen | Sovelluksen salainen, jotka olet luonut [Azure portaaliin](https://portal.azure.com/). Tämän sovelluksen toiminta perustuu tärkeän Palvelutietojen. Kannattaa tallentaa sen suojatusti palvelimeen. Sinun kannattaa myös huolehtia voit kiertää tämän asiakkaan salaisuus säännöllisin väliajoin. |

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
| access_token | Allekirjoitetun JWT tunnus, jonka olet pyytänyt. |
| laajuus | Alueiden tunnus on voimassa, joita voidaan käyttää välimuistiin tallentaminen tunnusten myöhempää käyttöä varten. |
| expires_in | Ajanjakso, joka access_token on voimassa (sekunteina). |
| refresh_token | OAuth 2.0-refresh_token. Sovellus käyttää tunnuksessa hankkia lisää tunnusten nykyisen tunnuksen päätyttyä.  Refresh_tokens pitkään olleet ja avulla voidaan säilyttää resurssien käytön pitkän ajan. Tarkemmin Lisätietoja on [B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md). Huomaa, että sinun on kehitetty laajuuden `offline_access` todennus-ja suojaustunnuksen pyynnöt, jotta he saavat refresh_token. |

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

## <a name="use-the-token"></a>Käytä tunnus
Nyt kun olet hankkinut onnistuneesti `access_token`, voit käyttää tunnuksen pyynnöt, että Taustajärjestelmä web ohjelmointirajapinnan sisällyttämällä sen `Authorization` otsikko:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Päivitä tunnus
Id_tokens ovat lyhytkestoisia. Sinun on päivitettävä ne, kun ne päättyy Jatka ei voi käyttää resursseja. Voit tehdä niin lähettämällä toiseen `POST` pyytää `/token` päätepiste. Tällä hetkellä tarjota `refresh_token` sijaan `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parametri | Pakollinen | Kuvaus |
| ----------------------- | ------------------------------- | -------- |
| p | Pakollinen | Käytäntö, jota käytettiin hankkimaan alkuperäinen refresh_token. Et voi käyttää eri käytäntö kehotusta. Huomautus Tämä parametri lisääminen **kyselymerkkijonon**, ei viestin tekstiosaan. |
| client_id | Pakollinen | Sovelluksen ID, jolla [Azure portal](https://portal.azure.com/) myönnetyt sovelluksen. |
| grant_type | Pakollinen | Myönnä, joiden on oltava tyypin `refresh_token` tämän luvan koodin kulun jalan varten. |
| laajuus | Suositellaan | Käyttöalueen tilaa erotettu luettelo. Yhden alueen arvo osoittaa Azure AD molemmat käyttöoikeudet, jotka haetaan. `openid` Laajuus osoittaa oikeuden käyttäjän sisäänkirjautuminen ja käytön **id_tokens**lomakkeen käyttäjän tietoja. Sen avulla voidaan saada tunnusten sovelluksen oman Taustajärjestelmä sivustoosi API, joka edustaa on sama kuin asiakas Sovellustunnus. `offline_access` Laajuus kertoo, että sovellus on **refresh_token** pitkäikäiset käyttää resursseja. |
| redirect_uri | Suositellaan | Sovelluksen sait authorization_code redirect_uri. |
| refresh_token | Pakollinen | Alkuperäinen refresh_token, jotka olet hankkinut kulun toisen jalan. Huomaa, että sinun on kehitetty laajuuden `offline_access` todennus-ja suojaustunnuksen pyynnöt, jotta he saavat refresh_token. |
| client_secret | Pakollinen | Sovelluksen salainen, jotka olet luonut [Azure portal](https://portal.azure.com/). Tämän sovelluksen toiminta perustuu tärkeän Palvelutietojen. Kannattaa tallentaa sen suojatusti palvelimeen. Sinun kannattaa myös huolehtia voit kiertää tämän asiakkaan salaisuus säännöllisin väliajoin. |

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
| access_token | Allekirjoitetun JWT tunnus, jonka olet pyytänyt. |
| laajuus | Alueiden tunnus on voimassa, joita voidaan käyttää välimuistiin tallentaminen tunnusten myöhempää käyttöä varten. |
| expires_in | Ajanjakso, joka access_token on voimassa (sekunteina). |
| refresh_token | OAuth 2.0-refresh_token. Sovellus käyttää tunnuksessa hankkia lisää tunnusten nykyisen tunnuksen päätyttyä.  Refresh_tokens pitkään olleet ja avulla voidaan säilyttää resurssien käytön pitkän ajan. Tarkemmin Lisätietoja on [B2C suojaustunnuksen viittaus](active-directory-b2c-reference-tokens.md). |

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


## <a name="send-a-sign-out-request"></a>Kirjaudu ulos Lähetyspyyntö

Kun haluat kirjautua ulos sovelluksesta käyttäjän, se ei ole tarpeeksi Poista sinua sovelluksen evästeet tai muussa end käyttäjän istunto. Käyttäjän on luotava uudelleenohjaus myös Azure AD Kirjaudu ulos. Jos et kiellä, käyttäjän ehkä, sovellus uudelleen ilman, että kirjoitat käyttöoikeutensa uudelleen. Tämä johtuu siitä, että heillä on kelvollinen yksittäisen Sign-istunnon Azure AD kanssa.

Voit uudelleenohjata riittää, että käyttäjä voi `end_session_endpoint` , joka näkyy samassa OpenID yhteyden metatietojen asiakirjassa on kuvattu aiemmissa "Vahvista id_token":

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parametri | Pakollinen? | Kuvaus |
| ----------------------- | ------------------------------- | ------------ |
| p | Pakollinen | Käytäntö, josta haluat kirjautua ulos sovelluksen käyttäjä käyttää. |
| post_logout_redirect_uri | Suositellaan | URL-osoite, käyttäjän uudelleenohjataan jälkeen onnistuneen Kirjaudu ulos. Jos se ei ole mukana, käyttäjä voi näkyvissä yleinen viestin Azure AD B2C mukaan.  |

> [AZURE.NOTE]
    Kun ohjaavat käyttäjän `end_session_endpoint` Poista joitakin käyttäjän Azure AD B2C yksittäisen Sign-tilaan, se ei Kirjaudu ulos käyttäjän sosiaalisen tunnistetietojen toimittaja (IDP) istunnon käyttäjä. Jos käyttäjä valitsee saman IDP aikana myöhemmin kirjautumisnimi, ne olla langattomilla, ilman, että kirjoitat tunnistetietoja. Jos käyttäjä haluaa B2C sovelluksen uloskirjautuminen, sitä ei välttämättä tarkoita ne haluat kirjautua ulos niiden Facebook-tilin kokonaan. Kuitenkin kyseessä paikallisten käyttäjätilien käyttäjän istunto lopetetaan oikein.

## <a name="use-your-own-b2c-tenant"></a>Käytä omia B2C vuokraajan

Jos haluat kokeilla näitä pyynnöt itsellesi, ensin Suorita nämä vaiheet ja korvaa esimerkkiarvoja yläpuolella omalla:

- [Luo B2C palvelutili](active-directory-b2c-get-started.md)ja käytä pyyntöjä alihallintaan nimeä.
- [Sovelluksen luominen](active-directory-b2c-app-registration.md) tunnus ja redirect_uri hankkimiseen. Haluat sisällyttää **web app-verkko api** sovelluksen, ja voit myös luoda **sovelluksen toiminta**.
- Voit hankkia käytännön nimet [luoda oman käytäntöjä](active-directory-b2c-reference-policies.md) .
