<properties
    pageTitle="Azure AD-v2.0 päätepisteen muutoksista | Microsoft Azure"
    description="Sovelluksen mallin v2.0 julkinen esikatselu-protokollat tehtyjen muutosten kuvaus."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Tärkeitä päivityksiä v2.0 todennus protokollia
Huomiota kehittäjät! Seuraavan kahden viikon aikana emme voi tehdä muutamia päivitykset v2.0 todennus protokollista, joka tarkoittaa purkaa olet kirjoittanut esikatselu Microsoftin jaksolla sovellusten muutokset.  

## <a name="who-does-this-affect"></a>Kuka tämä vaikuttaa?
Sovellukset, jotka on kirjoitettu käyttämään v2.0 päätynyt Päätepisteen todennus

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Lisätietoja v2.0 päätepisteen löytyy [tähän](active-directory-appmodel-v2-overview.md).

Jos olet luonut sovelluksen v2.0 päätepisteen avulla voit siirtyä v2.0-protokollan koodaus, kaikkien Microsoftin OpenID muodostaa tai OAuth web middlewares käyttäminen tai muiden 3 osapuolen-kirjastojen avulla voit suorittaa todennusta, sinun olisi valmisteltava testata projektien ja tee tarvittavat muutokset.

## <a name="who-doesnt-this-affect"></a>Kuka tämä ei vaikuta?
Sovellukset, jotka on kirjoitettu tuotannon Azure AD-todennus päätepisteen vastaan

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Tämä protokolla on määritetty Kivi ja ei voi ilmenee muutoksia.

Lisäksi jos käytetään oman app **vain** todennuksella ADAL kirjastoon, ei tarvitse muokata mitään.  ADAL on käsittely ilmalta tekemäsi muutokset-sovellus.  

## <a name="what-are-the-changes"></a>Mitä muutoksia?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>X5t arvon poistaminen JWT otsikot
V2.0 päätepisteen käyttää JWT tunnusten yleisesti, jotka sisältävät haluamasi metatietoa tunnuksen parametrit ylätunnisteessa.  Jos voit toistaa sekä nykyisen JWTs otsikon, etsii suunnilleen:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Jossa "x5t" ja "Lapset" ominaisuudet tunnistaa julkisella avaimella, jota käytetään vahvistamiseen allekirjoitus tunnuksen OpenID yhteyden metatietojen päätepisteen noudettu.

Microsoft tekee tähän muutos on poistettava "x5t"-ominaisuutta.  Voi edelleen käyttää samaa järjestelmiä vahvistamiseen tunnuksia, mutta kannata vain "Lapset"-ominaisuuden arvoksi noutaa oikea julkinen avain OpenID Yhdistä-protokollan mukaisesti. 

> [AZURE.IMPORTANT] **Työ: Varmista, että sovellus ei ole riippuvainen, onko x5t-arvo.**

### <a name="removing-profileinfo"></a>Profile_info poistaminen
Aiemmin v2.0 päätepisteen on on palauttavat base64-koodattuja JSON-objektin tunnuksen vastaukset kutsutaan `profile_info`.  Kun pyydetään käyttöoikeustietue v2.0 päätepisteestä lähettämällä pyynnön:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Vastauksen näyttäisi samanlaiselta kuin seuraavassa JSON-objekti:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`profile_info` Arvon tietoja käyttäjälle, joka kirjautunut sovellukseen - niiden näyttönimi, Etunimi, Sukunimi, sähköpostiosoite, tunnus ja niin edelleen.  Pääasiassa `profile_info` käytettiin suojaustunnuksen välimuistin ja näyttämistä.

On nyt poistetaan `profile_info` arvo – mutta ei hätää, Microsoft tarjoaa nämä tiedot edelleen kehittäjille hieman eri paikkaan.  Sen sijaan, että `profile_info`, v2.0 päätepisteen palauttaa `id_token` kunkin suojaustunnuksen vastineen:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Voit toistaa ja jäsentää id_token hakemiseen saamasi profile_info samat tiedot.  Id_token on JSON Web suojaustunnuksen (JWT), sisällön OpenID yhdistää määritetyn mukaisesti.  Tällöin koodi niin pitäisi olla samankaltainen – rajoittamattoman poimimaan id_token Keskimmäinen segmentin (teksti) ja base64 toistaa käyttämään JSON objektia.

Seuraavan kahden viikon aikana, sinun tulee koodata sovelluksen noutaminen joko käyttäjätiedot `id_token` tai `profile_info`; kumpi on näkyvissä.  Sen mukaan, kun muutos on tehty, sovellus voi saumattomasti käsitellä siirtymistä `profile_info` , `id_token` keskeytyksettä.

> [AZURE.IMPORTANT] **Työ: Varmista, että sovellus ei ole riippuvainen olemassaolosta `profile_info` arvo.**

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in poistaminen
Samalla tavalla kuin `profile_info`, on myös poistetaan `id_token_expires_in` vastaukset-parametri.  Aiemmin v2.0 päätepisteen palauttavat arvon `id_token_expires_in` sekä kunkin id_token vastaus, esimerkiksi Hyväksy-vastaus:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Tai suojaustunnuksen vastaus:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`id_token_expires_in` Arvo ilmaisee, id_token pysyy voimassa sekuntien määrän.  Nyt on poistetaan `id_token_expires_in` arvo kokonaan.  Sen sijaan voi käyttää OpenID yhteyden standardiksi `nbf` ja `exp` väittää tutkia id_token voimassaolo.  Katso lisätietoja [v2.0 suojaustunnuksen viittaus](active-directory-v2-tokens.md) näitä väitteitä.

> [AZURE.IMPORTANT] **Työ: Varmista, että sovellus ei ole riippuvainen olemassaolosta `id_token_expires_in` arvo.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Muuttaminen saatavat palauttama laajuus = openid
Tämä muutos on eniten merkittäviä – itse asiassa, se vaikuttaa lähes kaikki sovellusta, joka käyttää v2.0 päätepiste.  Useita sovelluksia lähettää pyyntöjä v2.0 päätepisteen avulla `openid` laajuus, kuten:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Tänään, kun käyttäjä antaa luvalla `openid` laajuus-sovelluksen vastaanottaa käyttäjän tietoja runsaasti tuloksena id_token.  Nämä vaateita, jotka voivat sisältää nimen, ensisijainen käyttäjänimi, sähköpostiosoite, Objektitunnus ja lisää.

Tässä päivityksessä on muuttamassa tiedot, joka `openid` laajuus niille paremmin comform OpenID yhteyden määritystä, sovelluksen käyttöoikeus.  `openid` Laajuus vain sallia käyttäjien kirjautua sovelluksen ja tulee käyttäjän app kielikohtaiset tunniste `sub` väittää, id_token.  Vain id_token saatavat `openid` on myönnetty laajuus henkilökohtaisia tietoja varten.  Esimerkki id_token vaateita, jotka ovat seuraavat:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Jos haluat saada henkilökohtaisia tietoja (PII)-sovelluksen käyttäjästä, sovelluksen on Lisäkäyttöoikeuksien pyynnön käyttäjältä.  Olemme ovat esittely kahden uuden käyttöalueen tuki OpenID yhteyden – määritys `email` ja `profile` käyttöalueita –, joita voit tehdä.

- `email` Alue on hyvin helppoa – se sallii käyttäjän ensisijaisen sähköpostiosoitteen kautta sovelluksen käyttöoikeus `email` id_token väittää.  Huomaa, että `email` varaa aina eivät sisälly id_tokens – se vain sisällytetään mahdollisesti käyttäjän profiiliin.
- `profile` Laajuus niille kaikki muut perustiedot käyttäjästä – henkilön nimi, ensisijainen käyttäjänimi, sovelluksen käyttöoikeus objektin tunnus ja niin edelleen.

Näin voit koodi sovelluksen mahdollisimman vähän paljastaminen tavalla – voit esittää käyttäjän juuri tiedot sovelluksen edellyttää, että sen työn arvojoukon.  Jos haluat jatkaa käytön käyttää kaikkia käytettävissä olevia käyttäjätietoja, joka vastaanottaa sovelluksen parhaillaan, Sisällytä kaikki kolme alueet luvan pyynnöt:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Sovelluksen aloittaa lähettäminen `email` ja `profile` käyttöalueen välittömästi ja v2.0 päätepisteen nämä kaksi käyttöalueen Hyväksy ja aloittaa pyytävät käyttöoikeudet käyttäjille tarpeen mukaan.  Kuitenkin tulkinnassa muutos `openid` laajuus tulevat voimaan muutaman viikon ajan.

> [AZURE.IMPORTANT] **Työ: Lisää `profile` ja `email` vaikutusalueista, jos sovellus edellyttää käyttäjän tietoja.**  Huomaa, että ADAL sisältää sekä näiden oikeuksien pyyntöjä oletusarvoisesti. 

### <a name="removing-the-issuer-trailing-slash"></a>Poistetaan vinoviiva myöntäjä.
Aiemmin myöntäjä arvo, joka näkyy tunnusten v2.0 päätepisteestä noudatit lomake

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Jos GUID-tunnus on tenantId, joka on antanut tunnuksen Azure AD-vuokraajan.  Nämä muutokset myöntäjä arvo muuttuu

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

molemmat tunnusten ja OpenID yhteyden etsiminen asiakirjassa.

> [AZURE.IMPORTANT] **Työ: Varmista, että sovelluksesi hyväksyy myöntäjä arvon sekä ilman vinoviiva myöntäjä vahvistuksen aikana.**

## <a name="why-change"></a>Miksi muuttaa?
Ensisijainen motivointi sisältää nämä muutokset on vakio OpenID yhteyden määrityksen mukaiseksi.  Olemalla OpenID yhteyden yhteensopiva Toivottavasti Pienennä erot integrointi Microsoft identity-palveluiden kanssa ja alan tunnistetietojen muiden palvelujen kanssa.  Haluat ottaa käyttöön kehittäjät voivat käyttää niiden tuttuja Avaa lähde-todennus kirjastoihin muuttamatta kirjastot sopimaan Microsoft erot.

## <a name="what-can-you-do"></a>Mitä voi tehdä?
Tänään voit aloittaa tekeminen kaikki yllä olevien ohjeiden muutokset.  Sinun tulee heti:

1.  **Poistaa riippuvuuksia `x5t` otsikon parametria.**
2.  **Käsittele tilanteen siirtymistä `profile_info` , `id_token` suojaustunnuksen vastauksiin.**
3.  **Poistaa riippuvuuksia `id_token_expires_in` vastauksen parametri.**
3.  **Lisää `profile` ja `email` vaikutusalueista sovelluksen, jos sovellus täytyy käyttäjätiedot.**
4.  **Hyväksy tunnusten sekä ilman vinoviiva myöntäjä arvoja.**

Microsoftin [v2.0 protokolla asiakirjat](active-directory-v2-protocols.md) on jo päivitetty vastaamaan nämä muutokset, niin että voi käyttää viitteenä auttaa Päivitä koodia.

Jos laajuuden muutokset on muita kysymyksiä, ota vapaasti Twitter-meille saavuttaminen @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Kuinka usein protokolla muutokset tapahtuu?
Microsoft ei ennustaa, mitä tahansa muita purkaa muutoksista todennus-protokollaa.  Olemme tarkoituksella ovat niputus yhden release nämä muutokset niin, että sinun ei tarvitse käydä läpi tällaista päivitysprosessin uudelleen tahansa pian.  Toimintojen lisääminen päätynyt v2.0 Todentamispalvelu, voit hyödyntää, jatketaan, mutta muutokset on oltava lisäaineen ja ei ole olemassa koodin sivunvaihto.

Lopuksi haluamme sano Kiitos, että kokeilla asioita aikana esikatselu.  Tutustu alkuvaiheen käyttäjät kokemukset ja tiedot on erittäin hyödyllinen toiminto toistaiseksi, ja Toivottavasti jatkat lausuntoja ja ideoiden jakamiseen.

Hyvää coding!

Microsoft Identity-osasto
