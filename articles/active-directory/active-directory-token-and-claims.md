 <properties
   pageTitle="Azure AD-tunnuksen viittaus | Microsoft Azure"
   description="Tietoja ja arvioidaan myöntänyt mukaan Azure Active Directory (AAD) SAML 2.0 ja JSON Web tunnusten (JWT) tunnusten saatavat opas"
   documentationCenter="na"
   authors="bryanla"
   services="active-directory"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/06/2016"
   ms.author="mbaldwin"/>

# <a name="azure-ad-token-reference"></a>Azure AD-tunnuksen viittaus

Azure Active Directory (Azure AD) tietokoneesta kuuluu äänimerkki erityyppisiä suojauksen tunnusten käsittelyssä kunkin todennus-työnkulku. Tässä asiakirjassa kuvataan muoto, suojaus-ominaisuuksien ja sisällön erityyppisiin tunnuksen.

## <a name="types-of-tokens"></a>Tunnusten tyypit

Azure AD tukee [OAuth 2.0 luvan protokolla](active-directory-protocols-oauth-code.md), mitä käytetään access_tokens ja refresh_tokens.  Se tukee myös todennus- ja kirjaudu sisään kautta [OpenID muodosta](active-directory-protocols-openid-connect-code.md), jossa esitellään tunnuksen, id_token kolmas tyyppi.  Kaikkien näiden tunnusten esitetään lukuna "haltijatunnukseen".

Haltijatunnukseen on kevyt suojaustunnus, joka antaa suojatun resurssille "haltijan"-käyttöoikeudet. Tässä mielessä "haltijan" on osapuolen, joka esittää tunnuksen. Vaikka vastaanottaminen haltijatunnukseen vaatii todennusta, Azure AD-ohjeita on otettava suojaaminen tunnuksen, voit estää kaappaamiselta tahattomia osapuoli. Koska haltijan tunnusten ei ole valmiita järjestelmä estämään luvatonta osapuolet käyttänyt niitä, ne on kuljetus suojatun kanavan, kuten transport layer security (HTTPS). Haltijatunnukseen siirretään Poista, jos jokin mies on keskimmäisen hyökkäys voidaan hankkia tunnuksen ja suojatun resurssin luvattomasti käyttöönsä. Samojen suojauksen periaatteiden käyttää, kun tallentaminen tai tallentaminen välimuistiin haltijan-tunnukset myöhempää käyttöä varten. Aina, varmista, että sovelluksesi välittää ja tallentaa haltijan tunnusten turvallisesti. Saat lisätietoja suojausasiat-haltijan-tunnukset, [RFC 6750 vaihe 5](http://tools.ietf.org/html/rfc6750).

Monia Azure AD myöntämä tunnusten toteutettu JSON Web tunnusten tai JWTs.  JWT on compact, URL-Osoitteen sopivaa tietojen siirtäminen kahden osapuolen välillä.  JWTs sisältämät tiedot ovat nimeltään "vaateet" ja vahvistukset tietojen haltijan ja aihe tunnuksen.  JWTs saatavat ovat JSON objekteja koodattu ja muuntaa sarjaksi tiedonsiirrossa.  Koska myöntämä Azure AD JWTs allekirjoitettu, mutta ei salata, voit helposti Tarkasta sisällön JWT vianmääritystä varten.  Tällöin, kuten [jwt.calebb.net](http://jwt.calebb.net)käytettävissä on useita työkaluja. Lisätietoja JWTs voidaan viitata [JWT määritykset](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens ovat kirjautumisen suojaustunnus, joka sovelluksen vastaanottaa todennuksessa [OpenID yhdistämistä](active-directory-protocols-openid-connect-code.md)käyttämällä.  Ne esitetään [JWTs](#types-of-tokens)ja sisältää saatavat, joita voit käyttää käyttäjän kirjautuminen sovelluksen.  Voit määrittää saatavat id_token kuin tuntuu sopivalta – usein niitä käytetään näyttämisen tilitiedot tai tehdä access ohjausobjektin päätöksiä-sovelluksessa.

Id_tokens on allekirjoitettu, mutta tällä hetkellä ole salattu.  Kun sovellus vastaanottaa id_token, se on voit todistaa tunnuksen aitous ja vahvista muutaman saatavat tunnuksen todistaa voimassaolon [Vahvista allekirjoitus](#validating-tokens) .  Vaateita, jotka hyväksyvät sovelluksen vaihtelevat sen mukaan, skenaarion vaatimukset, mutta on joitakin [yleisiä varaa vahvistukset](#validating-tokens) , jotka sovellus on suoritettava jokaisen skenaariossa.

Artikkelissa on tietoja seuraavassa osassa id_tokens saatavat sekä otoksen id_token.  Huomaa, että id_tokens saatavat palautetaan ole missään tietyssä järjestyksessä.  Lisäksi uusi saatavat voi viedä id_tokens milloin tahansa senhetkinen - sovellus ei saa tapahtua, kun uusi vaateita, jotka on otettu käyttöön.  Seuraava luettelo sisältää saatavat, joka sovelluksen luotettavasti voi tulkita tämän kirjoittaminen aikaa.  Tarvittaessa myös tarkemmin löytyvät [OpenID yhteyden määritykset](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Esimerkki id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.
```

> [AZURE.TIP] Kokeile noudattaa tarkistetaan otoksen id_token saatavat liittämällä sen [calebb.net](http://jwt.calebb.net).

#### <a name="claims-in-idtokens"></a>Id_tokens saatavat

| JWT varaaminen | Nimi | Kuvaus |
|-----------|------|-------------|
| `appid`| Tunnus | Tunnistaa sovellus, jossa on käytössä resurssin tunnus. Sovellus voi toimia itse tai käyttäjän puolesta. Sovellustunnus edustaa yleensä sovelluksen objektia, mutta se edustaa service Pääobjektin Azure AD. <br><br> **Esimerkki JWT arvo**: <br> `"appid":"15CB020F-3984-482A-864D-1D92265E8268"` |
| `aud`| Käyttäjäryhmälle | Tunnuksen halutulle vastaanottajalle. Sovellus, joka vastaanottaa tunnus on varmistaa, että osallistujat-arvo on oikein ja hylkää kaikki tunnusten tarkoitettu eri käyttäjäryhmälle. <br><br> **Esimerkki SAML arvo**: <br> `<AudienceRestriction>`<br>`<Audience>`<br>`https://contoso.com`<br>`</Audience>`<br>`</AudienceRestriction>` <br><br> **Esimerkki JWT arvo**: <br> `"aud":"https://contoso.com"` |
| `appidacr`| Sovelluksen käyttöoikeuksien kontekstin luokan viittaus | Ilmaisee, miten asiakas todennettu. Julkinen asiakkaan arvo on 0. Ostajantunnus ja asiakkaan salaisuus käytetään, jos arvo on 1. <br><br> **Esimerkki JWT arvo**: <br> `"appidacr": "0"`|
| `acr`| Todennus kontekstin luokan viittaus | Ilmaisee, miten aihe todennettu-sovelluksen käyttöoikeuksien kontekstin luokan viittaus vaatimus asiakkaan sijaan. "0" arvo tarkoittaa käyttäjän todennusta vastaa ISO/IEC 29115 vaatimuksia. <br><br> **Esimerkki JWT arvo**: <br> `"acr": "0"`|
| | Pikaviestien todennus | Tallentaa päivämäärän ja kellonajan, kun todennus on tapahtunut. <br><br> **Esimerkki SAML arvo**: <br> `<AuthnStatement AuthnInstant="2011-12-29T05:35:22.000Z">` |
| `amr`| Todentamismenetelmä | Näet, miten tunnuksen aihe todennettu. <br><br> **Esimerkki SAML arvo**: <br> `<AuthnContextClassRef>`<br>`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod/password`<br>`</AuthnContextClassRef>` <br><br> **Esimerkki JWT arvo**:`“amr”: ["pwd"]` |
| `given_name`| Etunimi | Sisältää ensimmäisen tai "annetaan" sen käyttäjän nimi, jotka on määritetty Azure AD-käyttäjäobjektin. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname”>`<br>`<AttributeValue>Frank<AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `"given_name": "Frank"` |
| `groups`| Ryhmät | Sisältää objektitunnukset, jotka edustavat hakijan ryhmäjäsenyyksiä. Nämä arvot ovat yksilöllisiä (Katso Objektitunnus) ja voidaan turvallisesti käyttö, kuten pakotetut todennus käyttämään resurssin hallintaan. Ryhmät-ryhmän sisältyvät ryhmät määritetään sovelluskohtaisesti-sovelluksen luettelo "groupMembershipClaims"-ominaisuuden avulla. Arvo on null jättää pois kaikki ryhmien, "SecurityGroup"-arvo sisältää vain Active Directory-käyttöoikeusryhmän jäsenyydet ja arvon "Kaikki" sisältävät käyttöoikeusryhmiä ja Office 365-jakeluluettelo. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">`<br>`<AttributeValue>07dd8a60-bf6d-4e17-8844-230b77145381</AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `“groups”: ["0e129f5b-6b0a-4944-982d-f776045632af", … ]` |
| `idp` | Tunnistetietojen toimittaja | Tietueet, jotka todennettu tunnuksen aihe tunnistetietojen toimittaja. Tämä arvo on sama kuin myöntäjä vaatimus arvo paitsi, jos käyttäjätili on eri alihallinnassa kuin myöntäjä. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name=” http://schemas.microsoft.com/identity/claims/identityprovider”>`<br>`<AttributeValue>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/<AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `"idp":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `iat` | IssuedAt | Tallentaa aika, jonka tunnus on annettu. Sitä käytetään usein mitata suojaustunnuksen tuoreus. <br><br> **Esimerkki SAML arvo**: <br> `<Assertion ID="_d5ec7a9b-8d8f-4b44-8c94-9812612142be" IssueInstant="2014-01-06T20:20:23.085Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">` <br><br> **Esimerkki JWT arvo**: <br> `"iat": 1390234181` |
| `iss` | Myöntäjä | Tunnistaa suojauksen suojaustunnuksen palvelun (STS), joka muodostaa ja palauttaa tunnuksen. Tunnuksia, joka palauttaa Azure AD-myöntäjä on sts.windows.net. GUID-tunnus myöntäjä varaa arvo on vuokraajan tunnus Azure AD-kansio. Vuokraajan tunnus on hakemiston pysyvä ja luotettava tunnus. <br><br> **Esimerkki SAML arvo**: <br> `<Issuer>https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/</Issuer>` <br><br> **Esimerkki JWT arvo**: <br>  `"iss":”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”` |
| `family_name` | Sukunimi | Sisältää viimeisen nimi, Sukunimi tai Sukunimi käyttäjän Azure AD-käyttäjäobjektin määritysten mukaisesti. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name=” http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname”>`<br>`<AttributeValue>Miller<AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `"family_name": "Miller"` |
| `unique_name`| Nimi | Sisältää ihmisten luettavissa arvo, joka määrittää tunnuksen aihe. Tämä arvo ei välttämättä voi olla yksilöivä palvelutili ja on suunniteltu käytetään ainoastaan näyttöä varten. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name=”http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name”>`<br>`<AttributeValue>frankm@contoso.com<AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `"unique_name": "frankm@contoso.com"` |
| `oid` | Objektitunnus | Sisältää Azure AD-objektin yksilöllinen. Tämä arvo on muuttumaton ja voi delegoida tai uudelleen. Objektin Azure AD-kyselyiden Objektitunnus avulla. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">`<br>`<AttributeValue>528b2ac2-aa9c-45e1-88d4-959b53bc7dd0<AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `"oid":"528b2ac2-aa9c-45e1-88d4-959b53bc7dd0"` |
| `roles` | Roolit | Vastaa kaikille sovelluksen roolit, joilla on aiheessa on myönnetty suoraan sekä epäsuorasti Ryhmäjäsenyyden avulla ja sitä voidaan käyttää voidaan valvoa Roolipohjainen käyttöoikeuksien valvonta. Sovelluksen roolit on määritetty sovelluksen kohti, välein kautta `appRoles` sovellusluettelo-ominaisuus. `value` Sovelluksen rooleille-ominaisuus on arvo, joka näkyy roolit-ryhmän. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/role">`<br>`<AttributeValue>Admin</AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `“roles”: ["Admin", … ]` |
| `scp` | Laajuus | Osoittaa asiakassovellukseen tekeytyminen käyttöoikeudet. Oletusasetusten mukaan `user_impersonation`. Suojatun resurssin omistaja voi rekisteröidä muita arvoja Azure AD. <br><br> **Esimerkki JWT arvo**: <br> `"scp": "user_impersonation"`|
| `sub` |Aihe| Näet, mitkä tunnuksen vahvistukset tiedot, kuten sovelluksen käyttäjän lyhennys. Tämä arvo on muuttumaton ja ei voi määrittää uudelleen tai uudelleen, niin se voidaan suorittaa luvan tarkistaa turvallisesti. Koska aihe on aina tunnusten Azure AD-ongelmat, on suositeltavaa arvoksi käyttäminen yleisiä tarkoitus luvan järjestelmän. <br> `SubjectConfirmation`ei ole vaatimus. Artikkelissa kuvataan, miten tunnuksen aihe on vahvistettu. `Bearer`osoittaa aihe on vahvistettu niiden käytettävissään tunnuksen. <br><br> **Esimerkki SAML arvo**: <br> `<Subject>`<br>`<NameID>S40rgb3XjhFTv6EQTETkEzcgVmToHKRkZUIsJlmLdVc</NameID>`<br>`<SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />`<br>`</Subject>` <br><br> **Esimerkki JWT arvo**: <br> `"sub":"92d0312b-26b9-4887-a338-7b00fb3c5eab"`|
| `tid` | Vuokraajan tunnus | Pysyvä, Uudelleenkäytettävä tunniste, joka määrittää hakemiston alihallintaa, johon annettu tunnus. Tätä arvoa käytetään voit vuokraajan kielikohtaiset directory resursseihin usean vuokraajan-sovelluksessa. Voit esimerkiksi tunnistaa Graph-Ohjelmointirajapinnan kutsu vuokraajan tämän arvon. <br><br> **Esimerkki SAML arvo**: <br> `<Attribute Name=”http://schemas.microsoft.com/identity/claims/tenantid”>`<br>`<AttributeValue>cbb1a5ac-f33b-45fa-9bf5-f37db0fed422<AttributeValue>` <br><br> **Esimerkki JWT arvo**: <br> `"tid":"cbb1a5ac-f33b-45fa-9bf5-f37db0fed422"`|
| `nbf`, `exp`|Suojaustunnuksen elinaika | Määrittää aikavälin, jonka tunnus on voimassa. Palvelun, joka vahvistaa tunnuksen Tarkista, että nykyinen päivämäärä on suojaustunnuksen elinkaaren, muuta se olisi hylätä tunnuksen. Palvelun saattaa sallia enintään viisi minuuttia suojaustunnuksen elinaika-alueen ulkopuolella laskemisesta eroja kellonajan ("aika jakauman.vinous-funktio") välillä Azure AD ja -palvelu. <br><br> **Esimerkki SAML arvo**: <br> `<Conditions`<br>`NotBefore="2013-03-18T21:32:51.261Z"`<br>`NotOnOrAfter="2013-03-18T22:32:51.261Z"`<br>`>` <br><br> **Esimerkki JWT arvo**: <br> `"nbf":1363289634, "exp":1363293234` |
| `upn`| Täydellinen käyttäjätunnus | Tallentaa käyttäjän täydellinen käyttäjänimi.<br><br> **Esimerkki JWT arvo**: <br> `"upn": frankm@contoso.com`|
| `ver`| Versio | Tallentaa tunnuksen versionumero. <br><br> **Esimerkki JWT arvo**: <br> `"ver": "1.0"`|

## <a name="access-tokens"></a>Access-tunnukset

Access-tunnusten ovat vain tässä vaiheessa kohdassa kerralla kulutettavia Microsoft Services.  Sovelluksia ei tarvitse suorittaa vahvistus- tai access-tunnusten tarkastus minkään tuetut skenaarioita.  Voit käsitellä access tunnusten kuin kokonaan peittävä – ne ovat vain merkkijonoja, joka sovellus voi välittää Microsoftille HTTP-pyynnöt.

Kun pyydät käyttöoikeustietue, Azure AD palauttaa myös joitakin metatietoja käyttöoikeustietue sinua sovelluksen käyttöön.  Nämä tiedot sisältävät voimassaolon ajan käyttöoikeustietue ja alueet, jossa on voimassa.  Näin suoritettava access tunnusten älykkäät välimuistiin eikä sinun tarvitse jäsentää Avaa sovelluksen käyttöoikeustietue itse.

## <a name="refresh-tokens"></a>Päivitä tunnusten

Päivitä tunnukset ovat suojauksen tunnuksia, jonka sovelluksen voit hankkia uuden access-tunnusten OAuth 2.0-työnkulussa.  Se sallii sovelluksen saavuttamiseksi pitkään resurssien käytön käyttäjän puolesta tarvitsematta käyttäjän toimia.

Päivitä tunnusten ovat usean resurssi, mikä tarkoittaa ne voivat vastaanotettu yksi resurssi suojaustunnuksen pyynnön, mutta tuoteavaimella, täysin eri resurssille tunnusten access. Voit määrittää usean resurssin `resource` parametrin kohdennettujen resurssin pyynnön.

Päivitä tunnukset ovat täysin peittävä sovelluksen. Ne ovat pitkäikäiset, mutta sovellus ei voi kirjoittaa toiminta päivityksen tunnuksen kestää minkä tahansa ajan kuluessa.  Päivitä tunnusten voidaan mitätöidä minkä tahansa hetkellä eri syistä.  Saat tietää, onko päivitys-tunnuksen kelvollinen sovelluksen vain silloin, kun yrität lunastaa tekemällä suojaustunnuksen pyynnön Azure AD-tunnuksen päätepiste.

Päivitä-tunnuksen, uusi käyttöoikeustietue lunastaa, kun saat uuden päivityksen suojaustunnuksen suojaustunnuksen vastauksessa.  Kannattaa tallentaa äskettäin myönnetyt Päivitä-tunnuksen korvaaminen pyynnössä käytetty.  Tämä takaa, että päivityksen tunnusten voimassa niin kauan.

## <a name="validating-tokens"></a>Tunnusten tarkistaminen

Tässä vaiheessa kohdassa kerralla asiakkaan sovelluksen pitäisi on suoritettava vain suojaustunnuksen vahvistus on vahvistetaan id_tokens.  Vahvista id_token sovelluksen tulee vahvistaa id_token allekirjoitus ja id_token saatavat.

Annamme kirjastojen ja MALLIKOODEJA, joka näyttää, miten käsittelemään helposti suojaustunnuksen vahvistus, jos haluat tietoja pohjana prosessi.  Liittyy myös useat kolmansien osapuolten Avaa lähde-kirjastojen käytettävissä JWT kelpoisuuden, vähintään yksi vaihtoehto lähes kaikissa ympäristön ja kieli. Saat lisätietoja Azure AD-todennus kirjastoihin ja MALLIKOODEJA [Azure AD-todennus kirjastoihin](active-directory-authentication-libraries.md).

#### <a name="validating-the-signature"></a>Allekirjoituksen tarkistaminen

JWT, sisältää kolme osia, jotka on erotettu toisistaan `.` merkki.  Ensimmäisen segmentin kutsutaan **otsikon** **leipätekstin**toinen ja kolmas **allekirjoitus**.  Allekirjoitus-osiossa voidaan tarkistaa id_token aitouden niin, että se voi luottaa sovelluksen.

Id_Tokens on allekirjoitettu käyttämällä alan vakio julkiseen salausalgoritmit, kuten RSA 256. Otsikko id_token sisältää avain ja salauksen menetelmä, jolla kirjaudutaan tunnuksen tietoja:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "kriMPdmBvx68skT8-mPAB3BseeA"
}
```

`alg` Varaa ilmaisee algoritmin, jota käytettiin Kirjaudu käyttöoikeustietue, aikaa `x5t` varaa osoittaa tietyn julkisella avaimella, jota käytettiin Kirjaudu tunnuksen.

Azure AD voi senhetkinen vaiheissa, kirjaudu id_token jollakin tietyt julkisen ja yksityisen avaimen paria. Azure AD kiertää mahdollinen joukko näppäimet säännöllisin väliajoin, jotta sovelluksesi on kirjoitettava käsittelee tärkeimmät muutokset automaattisesti.  Voit tarkistaa päivitykset Azure AD käyttämä julkiset avaimet suositeltavaa kerralla korkojakso on 24 tunnin välein.

Voit hankkia tarvittavat allekirjoituksen vahvistaminen käyttämällä OpenID Yhdistä-metatietojen asiakirja sijaitsee allekirjoitetun tärkeimmät tiedot:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [AZURE.TIP] Kokeile tätä URL-Osoitetta selaimessa!

Metatietojen asiakirja on JSON-objekti, joka sisältää useita hyödyllisiä kappaletta tietoja, kuten eri päätepisteet suorittamiseen OpenID yhteyden todennus vaaditaan sijainti.  

Se sisältää myös `jwks_uri`, jotka antavat julkiset avaimet, jolla kirjaudutaan tunnusten määrittäminen sijainti.  JSON-tiedosto sijaitsee `jwks_uri` sisältää kaikki julkinen avaintiedoista, erityisesti hetkellä käytössä.  Sovelluksen voit käyttää `kid` väittää JWT ylätunnisteessa, kun haluat valita, mitkä julkisella avaimella tässä asiakirjassa on käytetty kirjautua tietyn tunnuksen.  Valitse se voi suorittaa oikea julkinen avain ja määritettyihin algoritmin allekirjoitusta.

Allekirjoituksen vahvistuksen on tämän artikkelin ulkopuolella – monta Avaa lähde-kirjastot ovat käytettävissä auttavat kiellä tarvittaessa.

#### <a name="validating-the-claims"></a>Saatavat vahvistaminen

Kun sovellus vastaanottaa id_token käyttäjien sisäänkirjautumisessa yhteydessä, se pitäisi myös suorittaa muutama tarkistukset saatavat vastaan id_token.  Nämä ovat, mutta eivät ole rajoitettu:

  - **Käyttäjäryhmän** vaatimus - varmistaaksesi, että id_token on tarkoitettu annetaan sovelluksen.
  - **Ei ennen** - ja **Päättymisaika** saatavat – voit varmistaa, että id_token ei ole vanhentunut.
  - **Myöntäjän** vaatimus – voit varmistaa, että tunnus on varmasti myöntänyt sovelluksen Azure AD.
  - **Nonce-arvo** - tunnuksen toisto-hyökkäyksen pienentämään.
  - ja paljon muuta...

Täydellinen luettelo varaa vahvistukset sovelluksen pitäisi tehdä viitata [OpenID yhteyden määritykset](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Odotettu arvo näitä väitteitä tiedot sisältyvät [id_token osan](#id-tokens) edeltävän osan.

## <a name="sample-tokens"></a>Esimerkki tunnusten

Tässä osassa näkyy SAML ja JWT tunnuksia, joka palauttaa Azure AD-objektit. Näitä esimerkkejä, joiden avulla voit nähdä saatavat kontekstissa.
SAML-tunnus

Tämä on esimerkki tyypillisestä SAML-tunnuksen.

    <?xml version="1.0" encoding="UTF-8"?>
    <t:RequestSecurityTokenResponse xmlns:t="http://schemas.xmlsoap.org/ws/2005/02/trust">
      <t:Lifetime>
        <wsu:Created xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T05:15:47.060Z</wsu:Created>
        <wsu:Expires xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">2014-12-24T06:15:47.060Z</wsu:Expires>
      </t:Lifetime>
      <wsp:AppliesTo xmlns:wsp="http://schemas.xmlsoap.org/ws/2004/09/policy">
        <EndpointReference xmlns="http://www.w3.org/2005/08/addressing">
          <Address>https://contoso.onmicrosoft.com/MyWebApp</Address>
        </EndpointReference>
      </wsp:AppliesTo>
      <t:RequestedSecurityToken>
        <Assertion xmlns="urn:oasis:names:tc:SAML:2.0:assertion" ID="_3ef08993-846b-41de-99df-b7f3ff77671b" IssueInstant="2014-12-24T05:20:47.060Z" Version="2.0">
          <Issuer>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</Issuer>
          <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
            <ds:SignedInfo>
              <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
              <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
              <ds:Reference URI="#_3ef08993-846b-41de-99df-b7f3ff77671b">
                <ds:Transforms>
                  <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                  <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
                </ds:Transforms>
                <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <ds:DigestValue>cV1J580U1pD24hEyGuAxrbtgROVyghCqI32UkER/nDY=</ds:DigestValue>
              </ds:Reference>
            </ds:SignedInfo>
            <ds:SignatureValue>j+zPf6mti8Rq4Kyw2NU2nnu0pbJU1z5bR/zDaKaO7FCTdmjUzAvIVfF8pspVR6CbzcYM3HOAmLhuWmBkAAk6qQUBmKsw+XlmF/pB/ivJFdgZSLrtlBs1P/WBV3t04x6fRW4FcIDzh8KhctzJZfS5wGCfYw95er7WJxJi0nU41d7j5HRDidBoXgP755jQu2ZER7wOYZr6ff+ha+/Aj3UMw+8ZtC+WCJC3yyENHDAnp2RfgdElJal68enn668fk8pBDjKDGzbNBO6qBgFPaBT65YvE/tkEmrUxdWkmUKv3y7JWzUYNMD9oUlut93UTyTAIGOs5fvP9ZfK2vNeMVJW7Xg==</ds:SignatureValue>
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
              <X509Data>
                <X509Certificate>MIIDPjCCAabcAwIBAgIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAMC0xKzApBgNVBAMTImFjY291bnRzLmFjY2Vzc2NvbnRyb2wud2luZG93cy5uZXQwHhcNMTQwMTAxMDcwMDAwWhcNMTYwMTAxMDcwMDAwWjAtMSswKQYDVQQDEyJhY2NvdW50cy5hY2Nlc3Njb250cm9sLndpbmRvd3MubmV0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAkSCWg6q9iYxvJE2NIhSyOiKvqoWCO2GFipgH0sTSAs5FalHQosk9ZNTztX0ywS/AHsBeQPqYygfYVJL6/EgzVuwRk5txr9e3n1uml94fLyq/AXbwo9yAduf4dCHTP8CWR1dnDR+Qnz/4PYlWVEuuHHONOw/blbfdMjhY+C/BYM2E3pRxbohBb3x//CfueV7ddz2LYiH3wjz0QS/7kjPiNCsXcNyKQEOTkbHFi3mu0u13SQwNddhcynd/GTgWN8A+6SN1r4hzpjFKFLbZnBt77ACSiYx+IHK4Mp+NaVEi5wQtSsjQtI++XsokxRDqYLwus1I1SihgbV/STTg5enufuwIDAQABo2IwYDBeBgNVHQEEVzBVgBDLebM6bK3BjWGqIBrBNFeNoS8wLTErMCkGA1UEAxMiYWNjb3VudHMuYWNjZXNzY29udHJvbC53aW5kb3dzLm5ldIIQsRiM0jheFZhKk49YD0SK1TAJBgUrDgMCHQUAA4IBAQCJ4JApryF77EKC4zF5bUaBLQHQ1PNtA1uMDbdNVGKCmSp8M65b8h0NwlIjGGGy/unK8P6jWFdm5IlZ0YPTOgzcRZguXDPj7ajyvlVEQ2K2ICvTYiRQqrOhEhZMSSZsTKXFVwNfW6ADDkN3bvVOVbtpty+nBY5UqnI7xbcoHLZ4wYD251uj5+lo13YLnsVrmQ16NCBYq2nQFNPuNJw6t3XUbwBHXpF46aLT1/eGf/7Xx6iy8yPJX4DyrpFTutDz882RWofGEO5t4Cw+zZg70dJ/hH/ODYRMorfXEW+8uKmXMKmX2wyxMKvfiPbTy5LmAU8Jvjs2tLg4rOBcXWLAIarZ</X509Certificate>
              </X509Data>
            </KeyInfo>
          </ds:Signature>
          <Subject>
            <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">m_H3naDei2LNxUmEcWd0BZlNi_jVET1pMLR6iQSuYmo</NameID>
            <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer" />
          </Subject>
          <Conditions NotBefore="2014-12-24T05:15:47.060Z" NotOnOrAfter="2014-12-24T06:15:47.060Z">
            <AudienceRestriction>
              <Audience>https://contoso.onmicrosoft.com/MyWebApp</Audience>
            </AudienceRestriction>
          </Conditions>
          <AttributeStatement>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/objectidentifier">
              <AttributeValue>a1addde8-e4f9-4571-ad93-3059e3750d23</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/tenantid">
              <AttributeValue>b9411234-09af-49c2-b0c3-653adc1f376e</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name">
              <AttributeValue>sample.admin@contoso.onmicrosoft.com</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname">
              <AttributeValue>Admin</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname">
              <AttributeValue>Sample</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/ws/2008/06/identity/claims/groups">
              <AttributeValue>5581e43f-6096-41d4-8ffa-04e560bab39d</AttributeValue>
              <AttributeValue>07dd8a89-bf6d-4e81-8844-230b77145381</AttributeValue>
              <AttributeValue>0e129f4g-6b0a-4944-982d-f776000632af</AttributeValue>
              <AttributeValue>3ee07328-52ef-4739-a89b-109708c22fb5</AttributeValue>
              <AttributeValue>329k14b3-1851-4b94-947f-9a4dacb595f4</AttributeValue>
              <AttributeValue>6e32c650-9b0a-4491-b429-6c60d2ca9a42</AttributeValue>
              <AttributeValue>f3a169a7-9a58-4e8f-9d47-b70029v07424</AttributeValue>
              <AttributeValue>8e2c86b2-b1ad-476d-9574-544d155aa6ff</AttributeValue>
              <AttributeValue>1bf80264-ff24-4866-b22c-6212e5b9a847</AttributeValue>
              <AttributeValue>4075f9c3-072d-4c32-b542-03e6bc678f3e</AttributeValue>
              <AttributeValue>76f80527-f2cd-46f4-8c52-8jvd8bc749b1</AttributeValue>
              <AttributeValue>0ba31460-44d0-42b5-b90c-47b3fcc48e35</AttributeValue>
              <AttributeValue>edd41703-8652-4948-94a7-2d917bba7667</AttributeValue>
            </Attribute>
            <Attribute Name="http://schemas.microsoft.com/identity/claims/identityprovider">
              <AttributeValue>https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/</AttributeValue>
            </Attribute>
          </AttributeStatement>
          <AuthnStatement AuthnInstant="2014-12-23T18:51:11.000Z">
            <AuthnContext>
              <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:Password</AuthnContextClassRef>
            </AuthnContext>
          </AuthnStatement>
        </Assertion>
      </t:RequestedSecurityToken>
      <t:RequestedAttachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedAttachedReference>
      <t:RequestedUnattachedReference>
        <SecurityTokenReference xmlns="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd" xmlns:d3p1="http://docs.oasis-open.org/wss/oasis-wss-wssecurity-secext-1.1.xsd" d3p1:TokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0">
          <KeyIdentifier ValueType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLID">_3ef08993-846b-41de-99df-b7f3ff77671b</KeyIdentifier>
        </SecurityTokenReference>
      </t:RequestedUnattachedReference>
      <t:TokenType>http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV2.0</t:TokenType>
      <t:RequestType>http://schemas.xmlsoap.org/ws/2005/02/trust/Issue</t:RequestType>
      <t:KeyType>http://schemas.xmlsoap.org/ws/2005/05/identity/NoProofKey</t:KeyType>
    </t:RequestSecurityTokenResponse>

### <a name="jwt-token---user-impersonation"></a>JWT tunnus - käyttäjän tekeytyminen

Tämä on esimerkki tyypillisestä JSON web tunnus (JWT) luvan koodin grant-työnkulussa käytetään.
Lisäksi saatavat-tunnuksen sisältää versionumero **ver** ja **appidacr**-todennus kontekstin luokan-viittaus, joka ilmaisee, miten asiakas todennettu. Julkinen asiakkaan arvo on 0. Jos käytössä on Asiakastunnus tai asiakkaan salaisuus-arvo on 1.

    {
     typ: "JWT",
     alg: "RS256",
     x5t: "kriMPdmBvx68skT8-mPAB3BseeA"
    }.
    {
     aud: "https://contoso.onmicrosoft.com/scratchservice",
     iss: "https://sts.windows.net/b9411234-09af-49c2-b0c3-653adc1f376e/",
     iat: 1416968588,
     nbf: 1416968588,
     exp: 1416972488,
     ver: "1.0",
     tid: "b9411234-09af-49c2-b0c3-653adc1f376e",
     amr: [
      "pwd"
     ],
     roles: [
      "Admin"
     ],
     oid: "6526e123-0ff9-4fec-ae64-a8d5a77cf287",
     upn: "sample.user@contoso.onmicrosoft.com",
     unique_name: "sample.user@contoso.onmicrosoft.com",
     sub: "yf8C5e_VRkR1egGxJSDt5_olDFay6L5ilBA81hZhQEI",
     family_name: "User",
     given_name: "Sample",
     groups: [
      "0e129f6b-6b0a-4944-982d-f776000632af",
      "323b13b3-1851-4b94-947f-9a4dacb595f4",
      "6e32c250-9b0a-4491-b429-6c60d2ca9a42",
      "f3a161a7-9a58-4e8f-9d47-b70022a07424",
      "8d4c81b2-b1ad-476d-9574-544d155aa6ff",
      "1bf80164-ff24-4866-b19c-6212e5b9a847",
      "76f80127-f2cd-46f4-8c52-8edd8bc749b1",
      "0ba27160-44d0-42b5-b90c-47b3fcc48e35"
     ],
     appid: "b075ddef-0efa-123b-997b-de1337c29185",
     appidacr: "1",
     scp: "user_impersonation",
     acr: "1"
    }.

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä
- Lisätietoja hallinnasta suojaustunnuksen elinaika käytännön kautta Azure AD-kaavio-Ohjelmointirajapinnan ohjeaiheessa Azure AD-kaavio- [käytännön toiminnot](https://msdn.microsoft.com/library/azure/ad/graph/api/policy-operations) ja [käytännön kohteen](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference#policy-entity).
- Saat lisätietoja ja esimerkkejä, joiden hallinnasta käytännöt kautta PowerShellin cmdlet-komennot, kuten malleja, [määritettäviä suojaustunnuksen käyttöajan Azure AD](active-directory-configurable-token-lifetimes.md). 
