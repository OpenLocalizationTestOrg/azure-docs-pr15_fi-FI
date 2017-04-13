<properties
    pageTitle="Azure AD v2.0 implisiittinen työnkulku | Microsoft Azure"
    description="Rakennuksen web-sovellusten käyttäminen Azure AD v2.0 soveltaminen implisiittinen kulun yksittäiseltä sivulta sovellukset."
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

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 protokollat - kylpylät implisiittinen työnkulun käyttäminen
V2.0 päätepisteissä käyttäjät voivat kirjautua yhdelle sivulle-sovellusten kanssa sekä henkilökohtainen ja työpaikan tai oppilaitoksen tilit Microsoft.  Yhdelle sivulle ja JavaScript muista sovelluksista, jotka toimivat ensisijaisesti muutama kiinnostavat haastaa sen tullessa todentamiseen selaimen-kuvaketta:

- Suojauksen ominaisuudet nämä sovellukset poikkeavat merkittävästi perinteinen palvelinpohjaisia verkkosovellusten.
- Monta luvan palvelimet ja tunnistetietojen toimittajat eivät tue CORS pyynnöt.
- Koko sivun selaimen ohjaa muuttuvat erityisen invasiivisissa käyttäjäkokemuksen sovelluksen ulkopuolella.

Näiden sovellusten (Ajattele: AngularJS, Ember.js, React.js, jne) Azure AD tukee OAuth 2.0 implisiittinen myöntäminen kulun.  Implisiittinen työnkulku on kuvattu [OAuth 2.0-määrityksen](http://tools.ietf.org/html/rfc6749#section-4.2).  Sen tärkein etu on, että se sallii sovelluksen käyttämistä tunnusten Azure AD suorittamatta Taustajärjestelmä palvelimen tunnistetiedon exchange.  Näin sovelluksen käyttäjän kirjautuminen, ylläpitää istunnon ja hakea tunnuksia muiden sivustoon ohjelmointirajapinnan kaikki sisällä asiakkaan JavaScript-koodia.  On joitakin tärkeitä suojausasiat huomioon käytettäessä implisiittinen virtaus - erityisesti [asiakastietokoneen](http://tools.ietf.org/html/rfc6749#section-10.3) ja [käyttäjää tekeytyminen](http://tools.ietf.org/html/rfc6749#section-10.3)ympärille.

Jos haluat käyttää implisiittinen kulun sekä Azure AD käyttöoikeuksien lisääminen JavaScript-sovellukseen, on suositeltavaa käyttää Avaa lähde JavaScript kirjastoon, [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Muutamia AngularJS opetusohjelmat ovat käytettävissä [seuraavassa](active-directory-appmodel-v2-overview.md#getting-started) avulla pääset alkuun.  

Jos mieluummin käyttävät kirjastoja yksittäiseltä sivulta sovelluksen ja lähettää protokolla viestejä, yleisiä ohjeita noudattamalla.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Protocol (protokolla)-kaavio
Työnkulku koko implisiittinen kirjautuminen suunnilleen tämän näköinen – eri vaiheet on kuvattu alla.

![OpenId yhteyden kaistat](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Lähetä pyyntö-kirjautuminen

Käyttäjän kirjautuminen alun perin sovelluksen, voit lähettää [OpenID yhteyden](active-directory-v2-protocols-oidc.md) todennus-pyynnön ja Hae `id_token` v2.0 päätepisteestä:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Napsauttamalla seuraavaa linkkiä tämän pyyntö! Kun olet kirjautunut sisään, selaimen uudelleenohjataan `https://localhost/myapp/` kanssa `id_token` osoiteriville.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://Login.microsoftonline.com/common/oauth2/v2.0/Authorize...</a>

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| vuokraajan | pakollinen | `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallitut arvot ovat `common`, `organizations`, `consumers`, ja vuokraaja tunnukset.  Katso tarkemmin- [protokollan perusteet](active-directory-v2-protocols.md#endpoints). |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| response_type | pakollinen | On sisällettävä `id_token` OpenID yhteyden kirjautumista varten.  Se voi sisältää myös response_type `token`. Käyttämällä `token` tähän ansiosta sovelluksen vastaanottaa access-tunnuksen heti Hyväksy päätepisteestä eikä sinun tarvitse tehdä toisen pyynnön Hyväksy päätepiste.  Jos käytät `token` response_type, `scope` parametri on oltava alueen, joka ilmaisee, mikä resurssin antaa tunnus. |
| redirect_uri | suositellaan | Sovelluksen, joissa todennus vastaukset voidaan lähettää ja vastaanottaa sovelluksen mukaan redirect_uri.  Se on oltava täsmälleen samat jokin rekisteröidyt portaalissa, mutta sen on oltava url-koodattava redirect_uris. |
| laajuus | pakollinen | Käyttöalueen tilaa erotettu luettelo.  OpenID Connect-alue on sisällettävä `openid`, joka vastaa suostumusta Käyttöliittymän "Kirjautuminen"-käyttöoikeus.  Halutessasi voit myös kannattaa lisätä `email` tai `profile` [käyttöalueen](active-directory-v2-scopes.md) päässyt Lisää käyttäjätiedot.  Voi myös lisätä muita käyttöalueet tämän pyynnön pyydetään lupaa eri resurssien.  |
| response_mode | suositellaan | Määrittää, jota käytetään tuloksena suojaustunnuksen Edellinen lähettäminen sovelluksen.  Pitäisi olla `fragment` implisiittinen kulkuun.  |
| tila | suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa.  Voi olla merkkijonoksi sisällöstä, jotka haluat.  Satunnaisesti luotu yksilöllinen arvo käytetään yleensä [estää sivustojenvälisen pyynnön väärennös kalastelu](http://tools.ietf.org/html/rfc6749#section-10.12).  Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |
| Nonce-tiedot | pakollinen | Pyyntö sovelluksen, joka sisältyy tuloksena id_token kuin vaatimus luoma sisältyvät arvo.  Sovellus voi tarkistaa tämän arvon pienentämään tunnuksen toisto kalastelu.  Arvo on yleensä satunnaisesti yksilöllinen merkkijono, joilla voidaan tunnistaa pyynnön alkuperän.  |
| kehote | Valinnainen | Osoittaa ryhmän tyypin, jota tarvitaan käyttäjän toimia.  Tällä hetkellä vain kelvollisia arvoja ovat "kirjautuminen", "ei" ja "suostumus.  `prompt=login`pakottaa käyttäjä voi syöttää tunnistetietoja pyynnön negating single-etumerkin.  `prompt=none`on päinvastainen toiminto - varmistat, että käyttäjä ei näytetä minkä tahansa muun vuorovaikutteinen kehote.  Jos pyyntö ei voi viimeistellä äänettömästi single-etumerkin kautta, v2.0 päätepisteen palauttaa virheen.  `prompt=consent`käynnistää OAuth suostumusta-valintaikkunassa sen jälkeen, kun käyttäjä kirjautuu sisään, jossa käyttäjää pyydetään sovelluksen käyttöoikeuksia. |
| login_hint | Valinnainen | Voidaan täyttää valmiiksi sivun käyttäjän kirjautuminen käyttäjänimi/sähköpostin osoite-kenttään jos tiedät henkilön käyttäjänimi etuajassa.  Usein sovellusten käyttää parametriä uudelleen käyttöoikeuden tarkistaminen, onko sinulla jo poimittujen käyttäjänimi edellisen kirjautumisen using aikana `preferred_username` väittää. |
| domain_hint | Valinnainen | Voi olla jokin seuraavista `consumers` tai `organizations`.  Jos, se ohittaa sähköpostipohjainen etsiminen prosessin kyseisen käyttäjän käy läpi v2.0-kirjautumissivu, valitse hieman selkeyttää käyttäjäkokemus alussa.  Usein sovellusten käyttää parametriä uudelleen todennuksen aikana purkamalla `tid` väittää kohteesta id_token.  Jos `tid` väittää arvo on `9188040d-6c67-4c5b-b112-36a304b66dad`, sinun on käytettävä `domain_hint=consumers`.  Muussa tapauksessa käytä `domain_hint=organizations`. |

Tässä vaiheessa käyttäjää pyydetään syöttää tunnistetietoja ja suorittaa todennusta.  V2.0 päätepisteen myös varmistaa, että käyttäjä on hyväksynyt alla käyttöoikeudet `scope` kyselyn parametri.  Jos käyttäjä on ei sitoutunut käyttöoikeudet sitä pyytää käyttäjä suostuu tarvittavat käyttöoikeudet.  Tietoja [käyttöoikeudet, suostumusta, ja usean vuokraajan sovellusten toimitetaan tähän](active-directory-v2-scopes.md).

Kun käyttäjän todentaa ja antaa suostumus v2.0 päätepisteen palauttaa sovelluksen osoitteessa osoitettu vastauksen `redirect_uri`-parametrissa menetelmällä `response_mode` parametrin.

#### <a name="successful-response"></a>Onnistuneiden vastaus

Onnistuneiden vastauksen käyttämällä `response_mode=fragment` ja `response_type=id_token+token` näyttää seuraavalta ja luettavuutta rivinvaihdot seuraavasti:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| access_token | Jos mukana `response_type` sisältää `token`. Käyttöoikeustietue, sovellus pyytää, tässä tapauksessa Microsoft Graph.  Access-tunnuksen ei purettu tai muussa tarkastettu, se voidaan käsitellä peittävä merkkijonona. |
| token_type | Jos mukana `response_type` sisältää `token`.  On aina `Bearer`. |
| expires_in | Jos mukana `response_type` sisältää `token`.  Osoittaa tunnus on kelvollinen välimuistiin tallentaminen tarkoituksiin sekuntien määrän. |
| laajuus | Jos mukana `response_type` sisältää `token`.  Osoittaa scope(s), jolle access_token on voimassa. |
| id_token | Id_token, sovellus pyytää. Voit käyttää id_token käyttäjän henkilöllisyys ja Aloita istunto käyttäjän kanssa.  Lisätietoja id_tokens ja niiden sisältöä sisältyy [v2.0 päätepisteen tunnuksen viittaus](active-directory-v2-tokens.md).  |
| tila | Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |


#### <a name="error-response"></a>Virhe vastausta
Virhe vastaukset myös voidaan lähettää `redirect_uri` niin sovelluksen voit käsitellä niitä oikein:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |

## <a name="validate-the-idtoken"></a>Vahvista id_token
Vastaanottaminen juuri id_token ei ole riittäviä todennetaan; on Vahvista id_token allekirjoitus ja tarkista saatavat-tunnuksen lisääminen sovelluksen vaatimukset kohden.  V2.0 päätepisteen käyttää [JSON Web tunnusten (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ja julkisen avaimen salaus Kirjaudu tunnusten ja varmista, että ne ovat kelvollisia.

Voit tarkistaa `id_token` koodi, mutta yleinen käytäntö on asiakkaan Lähetä `id_token` Taustajärjestelmä palvelimeen ja vahvistusta siellä.  Kun olet vahvistettu id_token allekirjoitus, on muutamia saatavat, sinun on kirjoitettava vahvistamiseksi.  Hae lisätietoja, kuten [Vahvistaminen tunnusten](active-directory-v2-tokens.md#validating-tokens) ja [Tärkeitä tietoja tietoja kirjautuminen avain korvaamisessa](active-directory-v2-tokens.md#validating-tokens) [v2.0 suojaustunnuksen viittaus](active-directory-v2-tokens.md) .  Suosittelemme, että kaikki tarvittavat käyttö kirjaston jakaminen ja vahvistaminen tunnukset - on vähintään yksi käytettävissä useimmissa kielet ja ympäristöissä.
<!--TODO: Improve the information on this-->

Voit myös halutessasi tarkistaa muita saatavat mukaan käyttämässäsi skenaariossa.  Joitakin yleisiä vahvistukset ovat seuraavat:

- Varmistaa, että käyttäjän tai organisaation on tehnyt sovellus.
- Varmistaa, että käyttäjällä on oikea todennus ja käyttöoikeudet
- Varmistaa, että tiettyjä voimakkuuden todennus on tapahtunut, kuten monimenetelmäisen todentamisen.

Lisätietoja id_token saatavat Hae [v2.0 päätepisteen tunnuksen viittaus](active-directory-v2-tokens.md).

Kun kokonaan on vahvistettu id_token, voit aloittaa istunnon käyttäjän kanssa ja hankkia sovelluksen käyttäjän tiedot id_token saatavat avulla.  Nämä tiedot voidaan näyttää, tietueet, lupa.

## <a name="get-access-tokens"></a>Hae access tunnusten

Nyt kun olet kirjautunut yksittäiseltä sivulta sovelluksen käyttäjä, saat access tunnusten puheluja Web suojattu Azure AD, kuten [Microsoft Graph](https://graph.microsoft.io)API.  Vaikka olet saanut jo tunnuksen käyttämällä `token` response_type, voit käyttää tätä tapaa hankkimaan tunnusten lisäresursseihin ilman, että ohjaa käyttäjän kirjautua sisään uudelleen.

-OpenID Yhdistä/OAuth normaaliin merkkinä tekemällä pyynnön v2.0 `/token` päätepiste.  V2.0 päätepisteen kuitenkin tue CORS pyynnöt, joten voit hakea ja Päivitä tunnusten AJAX soittamista on kysymys.  Sen sijaan voit käyttää implisiittinen kulun piilotetut iframe-uusi tunnusten hakeminen muiden verkko-ohjelmointirajapinnan: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Kokeile kopioiminen ja liittäminen pyyntö selaimen-välilehteen alla! (Älä unohda korvaa `domain_hint` ja `login_hint` arvojen käyttäjän oikeat arvot)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| vuokraajan | pakollinen | `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallitut arvot ovat `common`, `organizations`, `consumers`, ja vuokraaja tunnukset.  Katso tarkemmin- [protokollan perusteet](active-directory-v2-protocols.md#endpoints). |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| response_type | pakollinen | On sisällettävä `id_token` OpenID yhteyden kirjautumista varten.  Se voi sisältää myös muita response_types, kuten `code`. |
| redirect_uri | suositellaan | Sovelluksen, joissa todennus vastaukset voidaan lähettää ja vastaanottaa sovelluksen mukaan redirect_uri.  Se on oltava täsmälleen samat jokin rekisteröidyt portaalissa, mutta sen on oltava url-koodattava redirect_uris. |
| laajuus | pakollinen | Käyttöalueen tilaa erotettu luettelo.  Hakeminen tunnuksia, Sisällytä kaikki [alueet](active-directory-v2-scopes.md) asetat halutut resurssi.  |
| response_mode | suositellaan | Määrittää, jota käytetään tuloksena suojaustunnuksen Edellinen lähettäminen sovelluksen.  Voi olla jokin seuraavista `query`, `form_post`, tai `fragment`.  |
| tila | suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa.  Voi olla merkkijonoksi sisällöstä, jotka haluat.  Satunnaisesti luotu yksilöllinen arvo käytetään yleensä estää sivustojenvälisen pyynnön väärennös kalastelu.  Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |
| Nonce-tiedot | pakollinen | Pyyntö sovelluksen, joka sisältyy tuloksena id_token kuin vaatimus luoma sisältyvät arvo.  Sovellus voi tarkistaa tämän arvon pienentämään tunnuksen toisto kalastelu.  Arvo on yleensä satunnaisesti yksilöllinen merkkijono, joilla voidaan tunnistaa pyynnön alkuperän.  |
| kehote | pakollinen | Päivittäminen & käytön tunnusten piilotetun iframe-kehyksen, sinun on käytettävä `prompt=none` varmistaaksesi, että iframe eikä Lopeta v2.0-kirjautumissivu ja palauttaa heti. |
| login_hint | pakollinen | Päivittäminen & käytön tunnusten piilotetut iframe-on lisättävä käyttäjän käyttäjänimi tätä Vihje voidaan erottaa useita käyttäjä voi olla tiettynä ajankohtana istuntojen välillä. Voit purkaa käyttäjänimi edellisen kirjautumisen using `preferred_username` väittää. |
| domain_hint | pakollinen | Voi olla jokin seuraavista `consumers` tai `organizations`.  Päivittäminen & käytön tunnusten piilotetun iframe-kehyksen, Sisällytä domain_hint pyynnön.  Olisi Pura `tid` väittää-Edellinen sign-in määrittää arvon, jota käytetään id_token.  Jos `tid` väittää arvo on `9188040d-6c67-4c5b-b112-36a304b66dad`, sinun on käytettävä `domain_hint=consumers`.  Muussa tapauksessa käytä `domain_hint=organizations`. |

Tukijoillemme `prompt=none` parametri, pyyntö joko onnistuu tai epäonnistuu välittömästi ja palaa sovelluksen.  Onnistuneen vastaus lähetetään sovelluksen osoitteessa osoitettu `redirect_uri`-parametrissa menetelmällä `response_mode` parametrin.

#### <a name="successful-response"></a>Onnistuneiden vastaus
Onnistuneiden vastauksen käyttämällä `response_mode=fragment` näyttää seuraavalta:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| access_token | Tunnuksen, sovellus pyytää. |
| token_type | On aina `Bearer`. |
| tila | Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Sovelluksen Tarkista, että pyynnön ja vastauksen tila-arvot ovat samat. |
| expires_in | Kuinka kauan käyttöoikeustietue on voimassa (sekunteina). |
| laajuus | Alueet käyttöoikeustietue on voimassa. |

#### <a name="error-response"></a>Virhe vastausta
Virhe vastaukset myös voidaan lähettää `redirect_uri` niin sovelluksen voit käsitellä niitä oikein.  Kyseessä on `prompt=none`, odotettu virhe on:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |

Jos näyttöön tulee seuraava virheilmoitus iframe-pyynnössä, käyttäjän on vuorovaikutteisesti Kirjaudu uudelleen hakea uuden tunnuksen.  Voit käsitellä tässä tapauksessa jostakin tapa luettelokohdat sovelluksen.

## <a name="refreshing-tokens"></a>Tunnusten päivittäminen

Molemmat `id_token`s ja `access_token`s päättyy, kun lyhyt tietyn ajan, jotta voit päivittää ne on valmisteltava sovelluksen tunnukset säännöllisin väliajoin.  Päivitä kumpaa tunnuksen, voit suorittaa piilotetun iframe-kehyksen pyynnössä yläpuolelta avulla `prompt=none` hallita Azure AD-parametri.  Jos haluat saada uuden `id_token`, varmista, että Käytä `response_type=id_token` ja `scope=openid`, sekä `nonce` parametri.


## <a name="send-a-sign-out-request"></a>Lähetä pyyntö uloskirjautuminen

OpenIdConnect `end_session_endpoint` ei tällä hetkellä tue v2.0 päätepiste. Tämä tarkoittaa sovelluksen et voi lähettää pyynnön v2.0 päätepisteen käyttäjän istunnon ja poistaa v2.0 päätepisteen asettaa evästeitä.
Kirjaudu ulos käyttäjä-sovelluksen voit vain omassa istunnon käyttäjän kanssa, ja jätä käyttäjän istunnon v2.0 päätepisteen--jäljelle.  Seuraavan kerran, kun käyttäjä yrittää kirjautua sisään, he näkevät "Valitse tili-sivulla niiden aktiivisesti kirjautunut sisään-tileillä luettelossa.
Sivun käyttäjä voi valita tilin, lopettaa istunnon v2.0 päätepisteissä uloskirjautuminen.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->