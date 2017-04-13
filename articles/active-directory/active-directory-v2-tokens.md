<properties
    pageTitle="Azure AD v2.0 suojaustunnuksen viittaus | Microsoft Azure"
    description="Tunnusten ja saatavat v2.0 päätepisteen lähettämän tyypit"
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
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="v20-token-reference"></a>v2.0 tunnuksen viittaus

V2.0 päätepisteen tietokoneesta kuuluu äänimerkki erityyppisiä suojauksen tunnusten käsittelyssä kunkin [todennus työnkulku](active-directory-v2-flows.md). Tässä asiakirjassa kuvataan muoto, suojaus-ominaisuuksien ja sisällön erityyppisiin tunnuksen.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Tunnusten tyypit

V2.0 päätepisteen tukee [OAuth 2.0 luvan protokolla](active-directory-v2-protocols.md), mitä käytetään access_tokens ja refresh_tokens.  Se tukee myös todennus- ja kirjaudu sisään kautta [OpenID muodosta](active-directory-v2-protocols.md#openid-connect-sign-in-flow), jossa esitellään tunnuksen, id_token kolmas tyyppi.  Kaikkien näiden tunnusten esitetään lukuna "haltijatunnukseen".

Haltijatunnukseen on kevyt suojaustunnus, joka antaa suojatun resurssille "haltijan"-käyttöoikeudet. Tässä mielessä "haltijan" on osapuolen, joka esittää tunnuksen. Vaikka osapuolen todentamismenetelmä on ensin Azure AD vastaanottamaan haltijatunnukseen, jos tarvittavat vaiheet ei oteta suojaaminen tunnuksen siirron ja tallennustilaa, se voidaan siepata ja käyttää tahattomia osapuoli. Kun joitakin suojauksen tunnusten on valmiita järjestelmä luvattoman estää niiden käyttämisen, haltijan-tunnukset ei ole tämä järjestelmä ja on kuljetus-suojatun kanavan, kuten transport layer security (HTTPS). Haltijatunnukseen siirretään Poista, jos jokin mies on keskimmäisen hyökkäys voidaan haittaohjelmien osapuolen hankkia tunnuksen ja käyttää sitä suojatun resurssin luvatonta pääsyä. Samojen suojauksen periaatteiden käyttää, kun tallentaminen tai tallentaminen välimuistiin haltijan-tunnukset myöhempää käyttöä varten. Aina, varmista, että sovelluksesi välittää ja tallentaa haltijan tunnusten turvallisesti. Saat lisätietoja suojausasiat-haltijan-tunnukset, [RFC 6750 vaihe 5](http://tools.ietf.org/html/rfc6750).

Monia v2.0 päätepisteen myöntämä tunnusten toteutettu Json Web tunnusten tai JWTs.  JWT on compact, URL-Osoitteen sopivaa tietojen siirtäminen kahden osapuolen välillä.  JWTs sisältämät tiedot ovat nimeltään "vaateet" ja vahvistukset tietojen haltijan ja aihe tunnuksen.  JWTs saatavat ovat JSON objekteja koodattu ja muuntaa sarjaksi tiedonsiirrossa.  Koska v2.0 päätepisteen myöntämä JWTs allekirjoitettu, mutta ei salata, voit helposti Tarkasta sisällön JWT vianmääritystä varten. Lisätietoja JWTs voidaan viitata [JWT määritykset](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens ovat kirjautumisen suojaustunnus, joka sovelluksen vastaanottaa todennuksessa [OpenID yhdistämistä](active-directory-v2-protocols.md#openid-connect-sign-in-flow)käyttämällä.  Ne esitetään [JWTs](#types-of-tokens)ja sisältää saatavat, joita voit käyttää käyttäjän kirjautuminen sovelluksen.  Voit määrittää saatavat id_token kuin sopivalla – usein niitä käytetään näyttämisen tilitiedot tai tehdä access ohjausobjektin päätöksiä-sovelluksessa.  V2.0 päätepisteen ongelmat vain yhden tyyppisiä id_token, jolla on yhtenäinen joukko saatavat riippumatta käyttäjälle, joka on kirjautunut sisään.  Tämä on ilmoittavat, että muoto ja id_tokens sisältö on sama henkilökohtainen Microsoft-Account käyttäjät ja työpaikan tai oppilaitoksen tilit.

Id_tokens on allekirjoitettu, mutta tällä hetkellä ole salattu.  Kun sovellus vastaanottaa id_token, se on voit todistaa tunnuksen aitous ja vahvista muutaman saatavat tunnuksen todistaa voimassaolon [Vahvista allekirjoitus](#validating-tokens) .  Vaateita, jotka hyväksyvät sovelluksen vaihtelevat sen mukaan, skenaarion vaatimukset, mutta on joitakin [yleisiä varaa vahvistukset](#validating-tokens) , jotka sovelluksen on suoritettava jokaisen skenaariossa.

Valitse id_tokens saatavat tarkat tiedot ovat jäljempänä, sekä otoksen id_token.  Huomaa, että id_tokens saatavat palautetaan ole missään tietyssä järjestyksessä.  Lisäksi uusi saatavat voi viedä id_tokens milloin tahansa senhetkinen - sovellus ei saa tapahtua, kun uusi vaateita, jotka on otettu käyttöön.  Alla olevassa luettelossa, joka sovelluksen luotettavasti voi tulkita tämän kirjoittaminen milloin saatavat.  Tarvittaessa myös tarkemmin löytyvät [OpenID yhteyden määritykset](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Esimerkki id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Kokeile noudattaa tarkistetaan otoksen id_token saatavat liittämällä sen [calebb.net](https://calebb.net).

#### <a name="claims-in-idtokens"></a>Id_tokens saatavat
| Nimi | Varaa | Esimerkkiarvo | Kuvaus |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Käyttäjäryhmälle | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Tunnistaa tunnuksen halutulle vastaanottajalle.  Id_tokens yleisön on sinua sovelluksen sovellustunnus varattujen sovelluksen sovelluksen rekisteröinti-portaalissa.  Sovelluksen pitäisi Vahvista arvoksi ja hylkää tunnuksen, jos se ei täsmää. |
| Myöntäjä | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Tunnistaa suojauksen suojaustunnuksen palvelun (STS), joka muodostaa ja palauttaa tunnuksen sekä Azure AD-vuokraajan, jossa todennettu käyttäjän.  Sovelluksen pitäisi Vahvista varmistaa, että tunnuksen peräisin v2.0 päätepisteen myöntäjä-ryhmän.  Se rajoittaa alihallinnat, jotka saavat sovellukseen kirjautuminen joukkoa myös vaatimus GUID-tunnus-osan avulla.  GUID-tunnus, joka ilmaisee, käyttäjä on Microsoft-tili on kuluttajille käyttäjältä `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Myöntää | `iat` | `1452285331` | Aika, jolla on annettu tunnus, edustaa kausi aika. |
| Vanhentumisajan | `exp` | `1452289231` | Aika, jossa on virheellinen, tunnuksen edustaa kausi aika.  Sovelluksen tarkastaa suojaustunnuksen elinaika vaatimus avulla.  |
| Ei ennen | `nbf` | `1452285331` |  Aika, jolloin tunnus tulee voimaan, edustaa kausi aika. Se on yleensä sama kuin liittoutumispalvelujen-aika.  Sovelluksen tarkastaa suojaustunnuksen elinaika vaatimus avulla.  |
| Versio | `ver` | `2.0` | Id_token määrittämä Azure AD versio.  V2.0 päätepisteen arvo on `2.0`. |
| Vuokraajan tunnus | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | GUID-tunnus, joka edustaa Azure AD vuokraaja joka käyttäjä on.  Työpaikan ja koulun tilien GUID-tunnus on organisaation, johon käyttäjä kuuluu pysyvä vuokraajan tunnus.  Henkilökohtaiset tilit-arvo on `9188040d-6c67-4c5b-b112-36a304b66dad`.  `profile` Laajuus vaaditaan, jotta saat vaatimus. |
| Koodin Hash | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Koodi-hash sisältyy id_tokens vain silloin, kun id_token lähetetään OAuth 2.0-todennus koodin rinnalla.  Sen avulla voidaan tarkistaa aitouden todennus-koodin.  Katso lisätietoja [OpenID yhteyden määrityksen](http://openid.net/specs/openid-connect-core-1_0.html) tämän vahvistuksen. |
| Access-tunnuksen Hash | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Access-tunnuksen hash sisältyy id_tokens vain silloin, kun id_token lähetetään OAuth 2.0-käyttöoikeustietue rinnalla.  Sen avulla voidaan tarkistaa access-tunnuksen aitous.  Katso lisätietoja [OpenID yhteyden määrityksen](http://openid.net/specs/openid-connect-core-1_0.html) tämän vahvistuksen. |
| Nonce-tiedot | `nonce` | `12345` | Nonce-arvo on strategia pienentävät tunnuksen toisto kalastelu.  Sovelluksen määrittää Nonce-arvo lupa pyynnön avulla `nonce` kyselyn parametri.  Pyyntö saadaan arvo lähetetty id_token- `nonce` varaa, muuttamattomia.  Näin sovelluksen vahvistamiseksi arvon vertaamalla sitä pyytää, joka liittää sovelluksen istunnon annetun id_token määritetyn arvon.  Sovelluksen pitäisi tehdä tämä vahvistus id_token kelpoisuuden tarkistamisen aikana. |
| Nimi | `name` | `Babe Ruth` | Ryhmän nimi sisältää ihmisten luettavissa arvo, joka määrittää tunnuksen aihe. Tämä arvo on yksilöllinen ei välttämättä, on mutable ja on suunniteltu käytetään ainoastaan näyttöä varten.  `profile` Laajuus vaaditaan, jotta saat vaatimus. |
| Sähköpostin | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Liittyvä käyttäjätili, jos sellainen on ensisijainen sähköpostiosoitteesi.  Sen arvo on mutable ja käyttäjän voivat muuttua ajan kuluessa.  `email` Laajuus vaaditaan, jotta saat vaatimus. |
| Ensisijainen käyttäjänimi | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Ensisijainen käyttäjänimi, jota käytetään esittämään v2.0 päätepisteen käyttäjän.  Se on voitu sähköpostiosoitteen, puhelinnumeron tai yleinen käyttäjänimi ilman määritetyssä muodossa.  Sen arvo on mutable ja käyttäjän voivat muuttua ajan kuluessa.  `profile` Laajuus vaaditaan, jotta saat vaatimus. |
| Aihe | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Mitkä tunnuksen vahvistukset tiedot, kuten sovelluksen käyttäjän lyhennys. Tämä arvo on muuttumaton ja ei voi määrittää uudelleen tai uudelleen, niin se voidaan suorittaa luvan tarkistaa turvallisesti, kuten tunnuksen käytettäessä resurssin. Koska aihe on aina tunnusten Azure AD-ongelmat, on suositeltavaa arvoksi käyttäminen yleisiä tarkoitus luvan järjestelmän. |
| Objektitunnus | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Objektin Azure AD-järjestelmän työpaikan tai oppilaitoksen tilin tunnus.  Tämä vaatimus ei antaa henkilökohtainen Microsoft-tilit.  `profile` Laajuus vaaditaan, jotta saat vaatimus. |


## <a name="access-tokens"></a>Access-tunnukset

Access-tunnusten v2.0 päätepisteen myöntämä ovat vain tässä vaiheessa kohdassa kerralla kulutettavia Microsoft Services.  Sovelluksia ei tarvitse suorittaa vahvistus- tai access-tunnusten tarkastus minkään tuetut skenaarioita.  Voit käsitellä access tunnusten kuin kokonaan peittävä – ne ovat vain merkkijonoja, joka sovellus voi välittää Microsoftille HTTP-pyynnöt.

Lähitulevaisuudessa v2.0 päätepisteen esittelee sovelluksen vastaanottamaan access tunnusten muiden asiakkaiden mahdollisuuden.  Sovellus on suoritettava access suojaustunnuksen kelpoisuustarkistus ja muut samoja tehtäviä tiedot päivitetään tiedot samaan aikaan.

Kun pyydät v2.0 päätepisteestä käyttöoikeustietue, v2.0 päätepisteen palauttaa myös joitakin metatietoja käyttöoikeustietue sinua sovelluksen käyttöön.  Nämä tiedot sisältävät voimassaolon ajan käyttöoikeustietue ja alueet, jossa on voimassa.  Näin suoritettava access tunnusten älykkäät välimuistiin eikä sinun tarvitse jäsentää Avaa sovelluksen käyttöoikeustietue itse.

## <a name="refresh-tokens"></a>Päivitä tunnusten

Päivitä tunnusten ovat suojauksen tunnuksia, jonka sovelluksen voit hankkia uusi käyttää tunnusten OAuth 2.0-työnkulussa.  Se sallii sovelluksen saavuttamiseksi pitkään resurssien käytön käyttäjän puolesta tarvitsematta käyttäjän toimia.

Päivitä tunnusten on usean resurssi.  Toisin sanoen vastaanotettu yksi resurssi suojaustunnuksen pyynnön päivityksen tunnusta voit voi hankkia Accessin tunnusten kokonaan eri resurssille.

Jotta he saavat päivityksen suojaustunnuksen vastauksen-sovellus on pyydettävä & myönnettävä `offline_acesss` laajuus.   Lisätietoja `offline_access` laajuus, [suostumus & käyttöalueen artikkelissa](active-directory-v2-scopes.md).

Päivitä tunnusten ovat, ja se on aina, kokonaan peittävä sovelluksen.  Ne myöntävät Azure AD-v2.0 päätepisteen ja voit vain on tarkastettu ja luokittelee v2.0 päätepiste.  Ne ovat pitkäikäiset, mutta sovellus ei voi kirjoittaa toiminta päivityksen tunnuksen kestää minkä tahansa ajan kuluessa.  Päivitä tunnusten voidaan mitätöidä minkä tahansa hetkellä eri syistä.  Saat tietää, onko päivitys-tunnuksen kelvollinen sovelluksen vain silloin, kun yrität lunastaa tekemällä suojaustunnuksen pyynnön v2.0 päätepisteelle.

Kun uusi käyttöoikeustietue, Päivitä tunnusta lunastaa (ja sovelluksen oli myönnetty `offline_access` laajuus), saat uuden päivityksen suojaustunnuksen suojaustunnuksen vastaus.  Kannattaa tallentaa äskettäin myönnetyt Päivitä-tunnuksen korvaaminen pyynnössä käytetty.  Tämä takaa, että päivityksen tunnusten voimassa niin kauan.

## <a name="validating-tokens"></a>Tunnusten tarkistaminen

Tässä vaiheessa kohdassa kerralla sovelluksia olisi on suoritettava vain suojaustunnuksen vahvistus on vahvistetaan id_tokens.  Vahvista id_token sovelluksen tulee vahvistaa id_token allekirjoitus ja id_token saatavat.

<!-- TODO: Link -->
Annamme kirjastoihin ja MALLIKOODEJA, joka näyttää, miten käsittelemään helposti suojaustunnuksen vahvistus - alla olevia tietoja vain annettu käyttäjille, jotka haluavat ymmärtää pohjana prosessi.  On myös 3 osapuolen-Avaa lähde-kirjastojen käytettävissä JWT vahvistus - on vähintään yksi vaihtoehto lähes kaikki ympäristö ja että kieli.

#### <a name="validating-the-signature"></a>Allekirjoituksen tarkistaminen
JWT, sisältää kolme osia, jotka on erotettu toisistaan `.` merkki.  Ensimmäisen segmentin kutsutaan **otsikon** **leipätekstin**toinen ja kolmas **allekirjoitus**.  Allekirjoitus-osiossa voidaan tarkistaa id_token aitouden niin, että se voi luottaa sovelluksen.

Id_Tokens on allekirjoitettu käyttämällä alan vakio julkiseen salausalgoritmit, kuten RSA 256. Otsikko id_token sisältää avain ja salauksen menetelmä, jolla kirjaudutaan tunnuksen tietoja:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

`alg` Varaa ilmaisee algoritmin, jota käytettiin Kirjaudu käyttöoikeustietue, aikaa `kid` varaa osoittaa tietyn julkisella avaimella, jota käytettiin Kirjaudu tunnuksen.

Milloin tahansa tiettynä ajankohtana v2.0 päätepisteen saattaa Kirjaudu id_token jollakin tietyt julkisen ja yksityisen avaimen paria.  V2.0 päätepisteen kiertää mahdollinen joukko näppäimet säännöllisin väliajoin, jotta sovelluksesi on kirjoitettava käsittelee tärkeimmät muutokset automaattisesti.  Voit tarkistaa päivitykset v2.0 päätepisteen käyttämä julkiset avaimet suositeltavaa kerralla korkojakso on noin 24 tuntia.

Voit hankkia tarvittavat allekirjoituksen vahvistaminen käyttämällä OpenID Yhdistä-metatietojen asiakirja sijaitsee allekirjoitetun tärkeimmät tiedot:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Kokeile tätä URL-Osoitetta selaimessa!

Metatietojen asiakirja on JSON-objekti, joka sisältää useita hyödyllisiä kappaletta tietoja, kuten eri päätepisteet suorittamiseen OpenID yhteyden todennus vaaditaan sijainti.  

Se sisältää myös `jwks_uri`, jotka antavat julkiset avaimet, jolla kirjaudutaan tunnusten määrittäminen sijainti.  JSON-tiedosto sijaitsee `jwks_uri` sisältää kaikki julkinen avaintiedoista, erityisesti hetkellä käytössä.  Sovelluksen voit käyttää `kid` väittää JWT ylätunnisteessa, kun haluat valita, mitkä julkisella avaimella tässä asiakirjassa on käytetty kirjautua tietyn tunnuksen.  Valitse se voi suorittaa oikea julkinen avain ja määritettyihin algoritmin allekirjoitusta.

Allekirjoituksen vahvistuksen on tämän artikkelin ulkopuolella – monta Avaa lähde-kirjastot ovat käytettävissä auttavat kiellä tarvittaessa.

#### <a name="validating-the-claims"></a>Saatavat vahvistaminen
Kun sovellus vastaanottaa id_token käyttäjien sisäänkirjautumisessa yhteydessä, se pitäisi myös suorittaa muutama tarkistukset saatavat vastaan id_token.  Nämä ovat, mutta eivät ole rajoitettu:

- **Käyttäjäryhmän** vaatimus - varmistaaksesi, että id_token on tarkoitettu annetaan sovelluksen.
- **Ei ennen** - ja **Päättymisaika** saatavat – voit varmistaa, että id_token ei ole vanhentunut.
- **Myöntäjän** vaatimus – voit varmistaa, että tunnus on varmasti myöntänyt sovelluksen v2.0 päätepiste.
- **Nonce-arvo** - tunnuksen toisto-hyökkäyksen rajoituksen kuin.
- ja paljon muuta...

Täydellinen luettelo varaa vahvistukset sovelluksen pitäisi tehdä viitata [OpenID yhteyden määritykset](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Näitä väitteitä odotettu arvo tiedot sisältyvät edellä [id_token-osassa](#id_tokens).


## <a name="token-lifetimes"></a>Suojaustunnuksen käyttöajan

Seuraavat suojaustunnuksen käyttöajan on tarkoitettu laskettuna ymmärrät, kuin ne helpottavat kehittäminen ja virheenkorjaus sovellukset.  Sovellus ei voi kirjoittaa jokin näistä käyttöajan säilyvät muuttumattomina toiminta - voivat ja muuttuu vaiheissa samanaikaisesti.

| Suojaustunnuksen | Elinaika | Kuvaus |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (työpaikan tai oppilaitoksen tilit) | 1 tunti | Id_Tokens ovat yleensä voimassa tunnissa.  Web-sovelluksen voit käyttää tämän saman elinaika oma istunnon ylläpito-käyttäjän (suositus) tai valita eri istunnon aikana.  Jos sovellus täytyy saadaksesi uuden id_token, se on yksinkertaisesti tehdä uuden kirjautumisen pyynnön v2.0 Määritä päätepisteen.  Jos käyttäjällä on kelvollinen selausistunnon v2.0 päätepisteen kanssa, he voivat ei tarvitse syöttää tunnistetietoja uudelleen. |
| Id_Tokens (henkilökohtaiset tilit) | 24 tunnin | Henkilökohtaiset tilit Id_Tokens ovat yleensä voimassa 24 tuntia.  Web-sovelluksen voit käyttää tämän saman elinaika oma istunnon ylläpito-käyttäjän (suositus) tai valita eri istunnon aikana.  Jos sovellus täytyy saadaksesi uuden id_token, se on yksinkertaisesti tehdä uuden kirjautumisen pyynnön v2.0 Määritä päätepisteen.  Jos käyttäjällä on kelvollinen selausistunnon v2.0 päätepisteen kanssa, he voivat ei tarvitse syöttää tunnistetietoja uudelleen. |
| Access-tunnusten (työpaikan tai oppilaitoksen tilit) | 1 tunti | Merkitty suojaustunnuksen vastaukset suojaustunnuksen metatietojen osana. |
| Access-tunnusten (henkilökohtaiset tilit) | 1 tunti | Merkitty suojaustunnuksen vastaukset suojaustunnuksen metatietojen osana.  Henkilökohtaiset tilit puolesta myönnetty Access_tokens saattaa olla määritetty eri elinaika, mutta tunnin on yleensä kirjainkokoa. |
| Päivitä tunnusten (työpaikan tai oppilaitoksen tili) | Enintään 14 päivää | Yksittäisen päivityksen tunnuksen on voimassa enintään 14 päivän ajan.  Kuitenkin päivityksen tunnuksen saattaa tulla virheellisiä hetkenä jokin muu luku syistä, jotta sovelluksesi Jatka käyttämisestä päivityksen tunnuksen, kunnes se epäonnistuu tai sovelluksen korvaa sen uuden päivityksen suojaustunnuksen ja yritä.  Päivitä-tunnuksen myös saattavat muuttua virheellisiksi, jos aikaa on kulunut alle 90 päivää, koska käyttäjä on kirjoittanut tunnistetietoja. |
| Päivitä tunnusten (henkilökohtaiset tilit) | Enintään 1 vuosi | Yksittäisen päivityksen tunnuksen kelpaa enimmäismäärä on 1 vuosi.  Kuitenkin päivityksen tunnuksen saattaa tulla virheellisiä hetkenä jokin muu luku syistä, jotta sovelluksesi Jatka Kokeile ja käytä Päivitä-tunnuksen, kunnes se epäonnistuu. |
| Luvan koodit (työpaikan tai oppilaitoksen tilit) | 10 minuutin | Luvan koodit ovat purposefully lyhytkestoinen ja pitäisi olla heti tuoteavaimella access_tokens ja refresh_tokens niiden saapuessa. |
| Luvan koodit (henkilökohtaiset tilit) | 5 minuuttia | Luvan koodit ovat purposefully lyhytkestoinen ja pitäisi olla heti tuoteavaimella access_tokens ja refresh_tokens niiden saapuessa.  Luvan koodit myöntänyt puolesta henkilökohtaiset tilit ovat myös kertaluonteista. |
