<properties
    pageTitle="Azure Active Directory-B2C | Microsoft Azure"
    description="Azure Active Directory-B2C myöntänyt tunnusten tyypit."
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


# <a name="azure-ad-b2c-token-reference"></a>Azure AD-B2C: Tunnuksen viittaus

Azure Active Directory (Azure AD) B2C tietokoneesta kuuluu äänimerkki suojauksen tunnusten useita erilaisia kuin käsittelee kunkin [todennus työnkulku](active-directory-b2c-apps.md). Tässä asiakirjassa kuvataan muoto, suojaus-ominaisuuksien ja sisällön erityyppisiin tunnuksen.

## <a name="types-of-tokens"></a>Tunnusten tyypit

Azure AD-B2C tukee [OAuth 2.0 luvan protokolla](active-directory-b2c-reference-protocols.md), minkä käyttäminen access-tunnusten ja Päivitä tunnukset. Se tukee myös todennus- ja kirjaudu sisään kautta [OpenID muodosta](active-directory-b2c-reference-protocols.md), jossa esitellään tunnuksen kolmas tyyppi: ID-tunnusta. Kaikkien näiden tunnusten esitetään lukuna haltijatunnukseen.

Haltijatunnukseen on kevyt suojaustunnus, joka antaa suojatun resurssille "haltijan"-käyttöoikeudet. Haltijan on osapuolen, joka esittää tunnuksen. Azure AD ensin on todentaa osapuoli, ennen kuin se voi vastaanottaa haltijatunnukseen. Mutta jos tarvittavat vaiheet ei oteta suojaaminen tunnuksen siirron ja tallennustilaa, se voidaan siepata ja käyttää tahattomia osapuoli. Jotkin suojauksen olla valmiita järjestelmä estää luvattoman käyttämästä niitä, mutta haltijan tunnusten ei ole se. Ne on kuljetus-suojatun kanavan, kuten transport layer security (HTTPS).

Jos haltijatunnukseen siirretään suojatun kanavan ulkopuolella, haittaohjelmien osapuolen käyttää mies-kohdassa-hyökkäys hankkia tunnuksen ja sen avulla suojattujen resurssin luvattomasti käyttöönsä. Samojen suojauksen periaatteiden käyttää, kun haltijan tunnusten säilytetään tai välimuistiin myöhempää käyttöä varten. Aina, varmista, että sovelluksesi välittää ja tallentaa haltijan tunnusten turvallisesti.

Saat lisätietoja suojauksesta-haltijan-tunnukset, [RFC 6750 vaihe 5](http://tools.ietf.org/html/rfc6750).

Monia tunnusten Azure AD B2C ongelmat, jotka on toteutettu JSON web tunnusten (JWTs). JWT on compact, URL-Osoitteen sopivaa tietojen siirtäminen kahden osapuolen välillä. JWTs sisältävät tietoja nimeltään vaateita koskevat rajoitukset. Nämä ovat vahvistukset haltijan tietoja ja tunnuksen aihe. JWTs saatavat ovat JSON-objekteja, jotka on koodattu ja muuntaa sarjaksi tiedonsiirrossa. Koska Azure AD B2C myöntämä JWTs allekirjoitettu, mutta se ei salata, voit helposti Tarkasta JWT korjaamisessa sen sisällön. Käytettävissä on useita työkaluja, jotka voit tehdä tämän myös [calebb.net](http://calebb.net). Lisätietoja JWTs viittaavat [JWT määritykset](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>Tunnus tunnukset

ID-tunnus on suojaustunnus, joka vastaanottaa sovelluksen Azure AD-B2C `authorize` ja `token` päätepisteet. Tunnus tunnusten esitetään [JWTs](#types-of-tokens)ja ne sisältävät saatavat, joiden avulla voit tunnistaa sovelluksesi käyttäjät. Kun tunnus tunnusten hankitaan `authorize` päätepisteen niitä käytetään usein verkkosovellusten käyttäjien kirjautuminen. Kun tunnus tunnusten hankitaan `token` päätepisteen, ne voidaan lähettää pyyntöjen välisen kaksi komponenttia saman sovelluksen tai palvelun aikana. Voit käyttää saatavat ID-tunnuksen tarpeen mukaan. Niitä käytetään yleensä tilitiedot vai access-ohjausobjektin päätösten-sovelluksessa.  

Tunnus tunnusten on allekirjoitettu, mutta niitä ei ole salattu tällä hetkellä. Kun sovellus tai API vastaanottaa ID-tunnusta, se on mikä osoittaa, että tunnuksen aito [Vahvista allekirjoitus](#token-validation) . Sovelluksen tai API on myös tarkistaa muutaman saatavat tunnuksen osoittamaan, että se on voimassa. Skenaario-vaatimukset mukaan vaateita, jotka hyväksyvät sovelluksen voi vaihdella, mutta sovelluksen on suoritettava muutamia [yleisiä varaa vahvistukset](#token-validation) jokaisen skenaariossa.

#### <a name="sample-id-token"></a>Esimerkki tunnus
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Access-tunnukset

Access-tunnuksen on myös suojaustunnus, joka vastaanottaa sovelluksen Azure AD-B2C `authorize` ja `token` päätepisteet. Access-tunnusten esitetään myös [JWTs](#types-of-tokens)ja saatavat, joiden avulla voit tunnistaa käyttäjät verkkopalveluita ja ohjelmointirajapinnan sisältää.

Access-tunnusten on allekirjoitettu, mutta ne eivät ole salattuja tällä hetkellä - ja muistuttaa tunnukset tunnus.  Access tunnusten olisi voidaan käyttää verkkopalveluita ja ohjelmointirajapinnan ja tunnistaa ja todennetaan-näistä palveluista.  Kuitenkin ne ei ole mitään näistä palveluista on lupa vahvistus.  Toisin sanoen, joka `scp` access tunnusten varaa ei rajoittaa tai muussa edustavat, tunnuksen aihe, jolle on myönnetty käyttöoikeus.

Kun oman API vastaanottaa käyttöoikeustietue, on [tarkistaa allekirjoituksen](#token-validation) mikä osoittaa, että tunnuksen aito. Oman API on myös tarkistaa muutaman vaateet tunnuksen osoittamaan, että se on voimassa. Skenaario-vaatimukset mukaan vaateita, jotka hyväksyvät sovelluksen voi vaihdella, mutta sovelluksen on suoritettava muutamia [yleisiä varaa vahvistukset](#token-validation) jokaisen skenaariossa.

### <a name="claims-in-id--access-tokens"></a>Tunnus & Access tunnusten saatavat

Kun käytät Azure AD B2C, voit hallita oman tunnusten sisällön tarkasti rajattuja. Voit määrittää [käytännöt](active-directory-b2c-reference-policies.md) lähettäminen tietyn käyttäjän tietojoukkoa saatavat, joka edellyttää sovelluksen toimintoja. Nämä vaateita, jotka voivat sisältää vakio-ominaisuudet, kuten käyttäjän `displayName` ja `emailAddress`. Ne voivat myös lisätä [mukautettuja määritteitä](active-directory-b2c-reference-custom-attr.md) , jotka voit määrittää B2C hakemistossa. Jokaisen tunnus & access tunnuksen, joka tulee näyttöön tulee tietyt tietoturvaan liittyvät vaateita koskevat rajoitukset. Sovellusten avulla näitä väitteitä todennetaan suojatusti käyttäjät ja pyynnöt.

Huomaa, että tunnus tunnusten saatavat palautetaan ei ole missään tietyssä järjestyksessä. Lisäksi uusi vaateita, jotka voidaan ottaa käyttöön-tunnuksen tunnusten milloin tahansa. Sovelluksen pitäisi ei saa tapahtua, kun uusi vaateita, jotka tuodaan. Seuraavassa on saatavat, jonka pitäisi ole myöntänyt Azure AD B2C tunnus & access tunnusten. Kaikki muut vaateet määritetään käytännöt perusteella. Kokeile noudattaa tarkistetaan otoksen tunnus saatavat liittämällä sen [calebb.net](http://calebb.net). Lisätietoja löytyy [OpenID yhteyden määritykset](http://openid.net/specs/openid-connect-core-1_0.html).

| Nimi | Varaa | Esimerkkiarvo | Kuvaus |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Käyttäjäryhmälle | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Osallistujat-ryhmän tunnistaa tunnuksen halutulle vastaanottajalle. Azure AD B2C yleisön on sinua sovelluksen Sovellustunnus varattujen sovelluksen sovelluksen rekisteröinti-portaalissa. Sovelluksen pitäisi Vahvista arvoksi ja hylkää tunnuksen, jos se ei täsmää. |
| Myöntäjä | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Tämä vaatimus tunnistaa suojauksen suojaustunnuksen palvelun (STS), joka muodostaa ja palauttaa tunnuksen. Se tunnistaa myös Azure AD-kansion, johon todennettu käyttäjän. Sovelluksen pitäisi Vahvista varmistaa, että tunnuksen peräisin v2.0 päätepisteen myöntäjä-ryhmän. |
| Myöntää | `iat` | `1438535543` | Tämä vaatimus on aika, jolla tunnuksen julkaistiin, edustaa kausi aika. |
| Vanhentumisajan | `exp` | `1438539443` | Varaa on aika, jossa on virheellinen, tunnuksen päättymisaika edustaa kausi aika. Sovelluksen tarkastaa suojaustunnuksen elinaika vaatimus avulla.  |
| Ei ennen | `nbf` | `1438535543` | Tämä vaatimus on aika, jolla tunnus on kelvollinen, esitetään kausi aika. Tämä on yleensä sama kuin tunnus on annettu aika. Sovelluksen tarkastaa suojaustunnuksen elinaika vaatimus avulla.  |
| Versio | `ver` | `1.0` | Tämä on Azure AD määritetty tunnus-versiota. |
| Koodin hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Koodi-hash sisältyy ID-tunnuksen vain, kun OAuth 2.0-todennus-koodi on annettu tunnus. Koodi-hash voidaan tarkistaa aitouden todennus-koodin. Katso lisätietoja [OpenID yhteyden määrityksen](http://openid.net/specs/openid-connect-core-1_0.html) suorittaa tämä vahvistus. |
| Access-tunnuksen hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Suojaustunnuksen access-hash sisältyy ID-tunnuksen vain, kun tunnus annetaan OAuth 2.0-käyttöoikeustietue kanssa. Suojaustunnuksen access-hash voidaan tarkistaa access-tunnuksen aitous. Katso lisätietoja [OpenID yhteyden määrityksen](http://openid.net/specs/openid-connect-core-1_0.html) suorittaa tämä vahvistus. |
| Nonce-tiedot | `nonce` | `12345` | Nonce-arvo on käytettävä pienentämään tunnuksen toisto kalastelu strategia. Sovelluksen määrittää Nonce-arvo lupa pyynnön avulla `nonce` kyselyn parametri. Pyyntö saadaan arvo lähetetty muuttamattomia `nonce` väittää, ID-tunnuksen vain. Näin sovelluksen vahvistamiseksi arvo-arvoa, se määrittää pyynnön, joka liittää sovelluksen istunnon annetun tunnus. Sovelluksen pitäisi suorittaa tämä vahvistus tunnus suojaustunnuksen vahvistuksen aikana. |
| Aihe | `sub` | `Not supported currently. Use oid claim.` | Tämä on lyhennys, mitkä tiedot, kuten sovelluksen käyttäjän tunnuksen vahvistukset. Tämä arvo on muuttumaton ja voi delegoida tai uudelleen. Sen avulla voidaan suorittaa luvan tarkistaa turvallisesti, kuten tunnuksen käytettäessä resurssin. Kuitenkin aihe-ryhmän ei ole vielä käytössä Azure AD-B2C. Kannattaa määrittää oman käytännöt sisältämään Objektitunnus `oid` ryhmätehtävän ja sen arvoa käytetään käyttäjien selvittäminen eikä käyttämällä lupaa aihe-ryhmän. |
| Todennus kontekstin luokan viittaus | `acr` | `b2c_1_sign_in` | Tämä on käytännön, jota käytettiin hankkia ID-tunnuksen nimi.  |
| Todennus-aika | `auth_time` | `1438535543` | Tämä vaatimus on aika, jolla viimeksi syötetyn käyttäjätietoja, edustaa kausi aika. |


### <a name="refresh-tokens"></a>Päivitä tunnusten

Päivitä tunnukset ovat suojauksen tunnuksia, sovelluksen avulla voit hankkia uuden tunnuksen tunnusten ja käyttää tunnusten OAuth 2.0-työnkulussa. Ne antaa resurssien pitkään pääsy käyttäjien puolesta sovelluksen tarvitsematta toimia näiden käyttäjien kanssa.

Päivityksen vastaanottamaan suojaustunnuksen suojaustunnuksen saatuaan sovelluksen on pyydettävä `offline_acesss` aluetta. Lisätietoja `offline_access` laajuus, [Azure AD B2C protokolla viittaus](active-directory-b2c-reference-protocols.md)viittaa.

Päivitä tunnusten ovat, ja se on aina, kokonaan peittävä sovelluksen. Azure AD myöntävät ja olisi tarkastaa ja luokittelee Azure AD. Ne ovat pitkäikäiset, mutta sovellus ei voi kirjoittaa ja, Päivitä-tunnuksen kestää tietyn ajan kuluessa. Päivitä tunnusten voidaan mitätöidä milloin tahansa eri syistä. Saat tietää, onko päivitys-tunnuksen kelvollinen sovelluksen vain silloin, kun yrität lunastaa tekemällä suojaustunnuksen pyynnön Azure AD.

Kun Päivitä-tunnuksen uuden tunnuksen, lunastaa (ja jos sovellus on annettu `offline_access` laajuutta), saat uuden päivityksen suojaustunnuksen suojaustunnuksen vastaus. Tallenna juuri lähetetyn päivityksen-tunnuksen. Se on korvattava olet aiemmin käyttänyt pyynnössä päivityksen tunnuksen. Näin voit varmistaa, että päivityksen tunnusten voimassa niin kauan.

## <a name="token-validation"></a>Suojaustunnuksen vahvistus

Vahvista tunnuksen sovelluksen Tarkista allekirjoitus ja tekemisestä tunnuksen.

Monta Avaa lähde-kirjastot ovat käytettävissä vahvistaminen JWTs, sen mukaan, ensisijainen kieli. On suositeltavaa, että tutkiminen näistä asetuksista sijaan toteuttaa oman vahvistus-logiikkaa. Tietojen tämän oppaan avulla voit opetella käyttämään nämä kirjastot oikein.

### <a name="validate-the-signature"></a>Vahvista allekirjoitus
JWT, sisältää kolme osia, jotka on erotettu toisistaan `.` merkki. Ensimmäinen osa on **otsikko**, toinen on **tekstissä**ja kolmas on **allekirjoitus**. Allekirjoitus-osiossa voidaan tarkistaa tunnuksen aitous niin, että se voi luottaa sovelluksen.

Azure AD-B2C tunnusten on allekirjoitettu käyttämällä yleisesti julkiseen salausalgoritmit, kuten RSA 256. Tunnuksen otsikko sisältää avain ja salauksen menetelmä, jolla kirjaudutaan tunnuksen tietoja:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

`alg` Varaa ilmaisee algoritmin, jota käytettiin Kirjaudu tunnuksen. `kid` Varaa osoittaa tietyn julkisella avaimella, jota käytettiin Kirjaudu tunnuksen.

Tietyn tahansa Azure AD saattaa Kirjaudu tunnusta jollakin tietyt julkisen ja yksityisen avaimen paria. Azure AD kiertää näppäimet mahdollinen joukko ajoittain, jotta sovelluksesi on kirjoitettava käsittelee tärkeimmät muutokset automaattisesti. Voit tarkistaa päivitykset Azure AD käyttämä julkiset avaimet suositeltavaa kerralla korkojakso on 24 tunnin välein.

Azure AD-B2C on päätepisteen metatietojen OpenID yhteyden. Näin voit noutaa tietoja Azure AD B2C suorituksen aikana. Näitä tietoja ovat päätepisteet suojaustunnuksen sisältö ja kirjautuminen näppäimet tunnuksen. B2C-hakemisto sisältää JSON metatietojen asiakirjan kunkin käytännön. Esimerkiksi metatietojen asiakirjaa varten `b2c_1_sign_in` käytännön `fabrikamb2c.onmicrosoft.com` sijaitsee:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`on käyttäjä todennetaan B2C kansio ja `b2c_1_sign_in` on käytettävä käytäntö hankkia tunnuksen. Voit määrittää mitä käytäntöä käytettiin Kirjaudu tunnus (ja hakeaksesi metatiedot mistä), käytettävissä on kaksi vaihtoehtoista. Ensin käytännön nimen sisältyy `acr` väittää tunnuksen. Voit jäsentää saatavat ulos JWT leipätekstiin base-64 koodauksen tekstiin ja poistettaessa JSON-merkkijono, joka tuottaa mukaan. `acr` Varaa päivitetään, jota käytettiin tunnuksen myöntää käytännön nimi.  Muu vaihtoehto on koodata arvon käytäntö `state` parametrin antaa pyynnön ja toistaa sitten se voit selvittää, mitä käytäntöä käytettiin. Toinen tapa on voimassa.

Metatietojen asiakirja on JSON-objekti, joka sisältää useita hyödyllisiä tietoja laitteita. Näitä ovat suorittamiseen OpenID yhteyden todennus päätepisteet sijainti. Myös niissä `jwks_uri`, joka antaa julkinen avain Määritä sijainti, käytetään kirjautua tunnukset. Sijainti on annettu tähän, mutta kannattaa noutaa sijainti dynaamisesti käyttämällä metatietojen asiakirja ja ulos jäsennyksen `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

JSON-tiedoston URL-osoitteesta sisältää kaikki julkisen avaintiedoista tietyn hetkellä käytössä. Sovelluksen voit käyttää `kid` väittää Valitse julkisella avaimella JSON-tiedostossa, jota käytetään Kirjaudu tietyn tunnuksen JWT otsikossa. Se suorittaa allekirjoitusta sitten oikea julkinen avain ja määritettyihin algoritmin avulla.

Kuvaus siitä, miten voit suorittaa allekirjoitusta on tämän artikkelin ulkopuolella. Monta Avaa lähde-kirjastojen sinuun voi auttaa sinua tätä, jos tarvitse sitä.

### <a name="validate-the-claims"></a>Vahvista saatavat
Kun sovellus tai API vastaanottaa ID-tunnusta, se pitäisi myös tehdä useita tarkistukset vastaan saatavat ID-tunnusta. Nämä ovat, mutta eivät ole rajoitettu:

- **Käyttäjäryhmän** vaatimus: tarkistaa, että ID-tunnus on tarkoitettu annetaan sovelluksen.
- **Ei ennen** - ja **Päättymisaika** -saatavat: nämä Varmista, että ID-tunnusta ei ole vanhentunut.
- **Myöntäjän** vaatimus: tarkistaa, että tunnus on myöntänyt sovelluksen Azure AD.
- **Nonce-arvo**: Tämä on strategia tunnuksen toisto hyökkäyksen rajoituksen.

Täydellisen luettelon kelpoisuus sovelluksen pitäisi tehdä Tutustu [OpenID yhteyden määritykset](https://openid.net). Näitä väitteitä odotettu arvo tiedot sisältyvät edellisen [tunnuksen-osassa](#types-of-tokens).  

## <a name="token-lifetimes"></a>Suojaustunnuksen käyttöajan

Seuraavat suojaustunnuksen käyttöajan tarkoituksena on edelleen tietosi. Niiden avulla voit kehittää ja korjata sovellukset. Huomaa, että sovelluksesi ei voi kirjoittaa odotettavissa nämä käyttöajan säilyvät muuttumattomina. Ne ja muuttaa.  Voit lukea lisää suojaustunnuksen elinaika-Azure AD B2C mukauttamisesta [tähän](active-directory-b2c-token-session-sso.md).

| Suojaustunnuksen | Elinaika | Kuvaus |
| ----------------------- | ------------------------------- | ------------ |
| Tunnus tunnukset | Tunti | Tunnus tunnusten ovat yleensä voimassa tunnissa. Web-sovelluksen avulla tämän elinaika ylläpitävät omassa istuntoja (suositus)-käyttäjien kanssa. Voit myös valita eri istunnon aikana. Jos sovellus täytyy uuden tunnuksen hankkiminen tunnuksen, se on vain uusi kirjautumisen pyyntö tehdä Azure AD. Jos käyttäjällä on kelvollinen selausistunto ja Azure AD-käyttäjä saa ei tarvitse Anna tunnistetiedot uudelleen. |
| Päivitä tunnusten | Enintään 14 päivää | Yksittäisen päivityksen tunnuksen on voimassa enintään 14 päivän ajan. Kuitenkin päivityksen tunnuksen saattaa saattavat muuttua virheellisiksi hetkenä monista syistä. Sovelluksen pitäisi edelleen yrittää käyttää Päivitä-tunnuksen, kunnes pyyntö epäonnistuu tai sovelluksen korvaa päivityksen tunnuksen uuden.  Päivitä-tunnuksen voivat muuttua virheellisiksi, jos käyttäjä viimeksi syötetyn tunnistetiedot on kulunut alle 90 päivää. |
| Luvan koodit | Viisi minuuttia | Luvan koodeilla tarkoituksella lyhytkestoinen. Ne olisi voi lunastaa heti access tunnuksia, tunnus tunnusten tai päivitä tunnusten niiden saapuessa. |
