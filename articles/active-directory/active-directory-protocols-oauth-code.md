<properties
    pageTitle=".NET-protokollan yleiskatsaus Azure AD | Microsoft Azure"
    description="Tässä artikkelissa käsitellään sallivat access web-sovellusten ja Azure Active Directory ja OAuth 2.0 vuokraajan ohjelmointirajapinnan web HTTP viestien avulla."
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


# <a name="authorize-access-to-web-applications-using-oauth-20-and-azure-active-directory"></a>Hyväksy OAuth 2.0 ja Azure Active Directoryn web-sovellusten käyttö

Azure Active Directory (Azure AD) käyttää OAuth 2.0, jotta voit sallia access web-sovellusten ja web ohjelmointirajapinnan Azure AD-vuokraajan. Tämän oppaan riippumaton kieli on ja kerrotaan, miten voit lähettää ja vastaanottaa HTTP viestejä ilman Microsoftin Avaa lähde-kirjastojen avulla.

OAuth 2.0 luvan koodin työnkulku on kuvattu [osassa 4.1 OAuth 2.0-määrityksestä](https://tools.ietf.org/html/rfc6749#section-4.1) . Sillä voidaan suorittaa todennus- ja tyypit, mukaan lukien web Apps-sovellusten ja asenneta sovellukset app useimpia.

[AZURE.INCLUDE [active-directory-protocols-getting-started](../../includes/active-directory-protocols-getting-started.md)]


## <a name="oauth-20-authorization-flow"></a>OAuth 2.0 luvan työnkulku

Korkean tason koko luvan kulun sovelluksen seuraavanlainen seuraavasti:

![OAuth Auth koodin työnkulku](media/active-directory-protocols-oauth-code/active-directory-oauth-code-flow-native-app.png)


## <a name="request-an-authorization-code"></a>Pyydä todennus-koodi

Todennus-koodin työnkulku alkaa asiakas, joka ohjaa käyttäjän `/authorize` päätepiste. Tämä pyyntö asiakkaan viittaa tarvitsemiaan käyttäjältä hankkimaan käyttöoikeuksia. Saat OAuth 2.0-päätepisteet-sivulta sovelluksen Azure perinteinen Portalissa ala-laatikon **Näkymän päätepisteet** -painike.

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&resource=https%3A%2F%2Fservice.contoso.com%2F
&state=12345
```

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| vuokraajan | pakollinen | `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallittu arvo on vuokraajan tunnukset, kuten `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` tai `contoso.onmicrosoft.com` tai `common` vuokraajan riippumattomalla tunnusten varten |
| client_id | pakollinen | Sovelluksen kun rekisteröityjä Azure AD määritetty sovellustunnus. Voit etsiä Azure hallinta-portaalissa. Valitse **Active Directory**, valitse kansio, sovellus napsauttamalla ja valitse **määrittäminen** |
| response_type | pakollinen | On sisällettävä `code` luvan koodin kulkuun. |
| redirect_uri | suositellaan | Sovelluksen, joissa todennus vastaukset voidaan lähettää ja vastaanottaa sovelluksen mukaan redirect_uri.  Se on oltava täsmälleen samat jokin rekisteröidyt portaalissa, mutta sen on oltava url-koodattava redirect_uris.  Alkuperäisen ja mobile-sovellusten, sinun täytyy käyttää oletusarvon `urn:ietf:wg:oauth:2.0:oob`. |
| response_mode | suositellaan | Määrittää, jota käytetään tuloksena suojaustunnuksen Edellinen lähettäminen sovelluksen.  Voi olla `query` tai `form_post`.  |
| tila | suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa. Satunnaisesti luotu yksilöllinen arvo käytetään yleensä [estää sivustojenvälisen pyynnön väärennös kalastelu](http://tools.ietf.org/html/rfc6749#section-10.12).  Siinä käytetään myös koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |
| resurssi | Valinnainen | Verkko-Ohjelmointirajapinnan (suojatun resurssi) sovelluksen tunnus URI. Etsi verkko-Ohjelmointirajapinnan-sovelluksen tunnus URI Azure hallinta-portaalissa, valitse **Active Directory**, hakemisto, sovellus ja valitse sitten **määrittäminen**. |
| kehote | Valinnainen |  Määritä käyttäjän toimia, jotka tarvitaan tyyppi.<p> Kelvollisia arvoja: <p> *Kirjaudu sisään*: pyytää käyttäjää tarkistamiseen uudelleen. <p> *suostumus*: käyttäjän lupaa on myönnetty, mutta se on päivitettävä. Käyttäjän tulee kehotus suostumus. <p> *admin_consent*: järjestelmänvalvoja tulee kehotus suostumus organisaation kaikkien käyttäjien puolesta |
| login_hint | Valinnainen | Voidaan täyttää valmiiksi sivun käyttäjän kirjautuminen käyttäjänimi/sähköpostin osoite-kenttään jos tiedät henkilön käyttäjänimi etuajassa.  Usein sovellusten käyttää parametriä uudelleen käyttöoikeuden tarkistaminen, onko sinulla jo poimittujen käyttäjänimi edellisen kirjautumisen using aikana `preferred_username` väittää. |
| domain_hint | Valinnainen | Sisältää tietoja vuokraajan tai toimialue, jota käyttäjä käyttää kirjautuminen vihje. Vuokraajan rekisteröity toimialue domain_hint arvo. Jos vuokraajan on liitetty paikallisesta hakemistosta, AAD ohjaa määritetyn vuokraajan liittoutumispalvelimen. |

> [AZURE.NOTE] Jos käyttäjä kuuluu organisaatiossa, organisaation järjestelmänvalvoja voit hyväksyminen tai hylkääminen käyttäjän puolesta tai Salli käyttäjän suostumus. Käyttäjän on valita asetuksen, vain silloin, kun järjestelmänvalvoja sallii suostumus.

Tässä vaiheessa käyttäjää pyydetään suostumus alla käyttöoikeuksia ja syöttää tunnistetietoja `scope` kyselyn parametri. Kun käyttäjä todentaa ja antaa suostumus, Azure AD lähettää vastauksen sovelluksen `redirect_uri` osoite puhelinnumeroon.

### <a name="successful-response"></a>Onnistuneiden vastaus

Onnistuneiden vastauksen näyttää tältä:

```
GET  HTTP/1.1 302 Found
Location: http://localhost/myapp/?code= AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA&session_state=7B29111D-C220-4263-99AB-6F6E135D75EF&state=D79E5777-702E-4260-9A62-37F75FF22CCE
```

| Parametri | Kuvaus |
| -----------------------| --------------- |
| admin_consent | Arvo on TOSI, jos järjestelmänvalvoja on suostunut suostumusta pyynnön kehotteen.|
| koodi | Luvan tunnus, jolla sovellus pyytää. Sovellus pyytää resurssin kohde käyttöoikeustietue todennus-koodin avulla. |
| session_state | Yksilöivä arvo, joka määrittää nykyisen käyttäjäistunnon. Tämä arvo on GUID-tunnus, mutta peittävä arvo, joka ilman tutkimus välitetään käsitellään. |
| tila | Jos tilan parametrin pyynnön, saman arvon pitäisi näkyä vastaus. Se on hyvä sovelluksen tarkistamaan, että pyynnön ja vastauksen tila-arvot ovat samat, ennen kuin käytät vastaus. Tämä auttaa havaitsemaan [sivustojenvälisen pyytää väärennös (CSRF) kalastelu](https://tools.ietf.org/html/rfc6749#section-10.12) vastaan asiakkaan.  

### <a name="error-response"></a>Virhe vastausta

Virhe vastaukset myös voidaan lähettää `redirect_uri` niin, että sovellus voit käsitellä niitä oikein.

```
GET http://localhost:12345/?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parametri | Kuvaus |
|-----------|-------------|
| Virhe | Koodin virhearvon määritelty [OAuth 2.0 luvan Framework](http://tools.ietf.org/html/rfc6749)osan 5.2. Seuraavassa taulukossa on kuvattu virhekoodit, joka palauttaa Azure AD. |
| error_description | Virheen yksityiskohtainen kuvaus. Viestiä ei ole tarkoitus olla loppukäyttäjien friendly. |
| tila | Tila-arvo on satunnaisesti luotu-uudelleen arvo, joka lähetetään pyynnön ja vastauksen estää sivustojenvälisen pyynnön väärennös (CSRF) kalastelu määrää. |

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

## <a name="use-the-authorization-code-to-request-an-access-token"></a>Pyydä access-tunnuksen todennus-koodin avulla

Nyt kun olet hankkinut todennus-koodin ja olet saanut käyttöoikeuksia käyttäjä, voivat lunastaa access-tunnuksen haluamasi resurssiin koodin lähettämällä kirjaa pyynnön `/token` päätepisteen:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded
grant_type=authorization_code
&client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrqqf_ZT_p5uEAEJJ_nZ3UmphWygRNy2C3jJ239gV_DBnZ2syeg95Ki-374WHUP-i3yIhv5i-7KU2CEoPXwURQp6IVYMw-DjAOzn7C3JCu5wpngXmbZKtJdWmiBzHpcO2aICJPu1KvJrDLDP20chJBXzVYJtkfjviLNNW7l7Y3ydcHDsBRKZc3GuMQanmcghXPyoDg41g8XbwPudVh7uCmUponBQpIhbuffFP_tbV8SNzsPoFz9CLpBCZagJVXeqWoYMPe2dSsPiLO9Alf_YIe5zpi-zY4C3aLw5g9at35eZTfNd0gBRpR5ojkMIcZZ6IgAA
&redirect_uri=https%3A%2F%2Flocalhost%2Fmyapp%2F
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=p@ssw0rd

//NOTE: client_secret only required for web apps
```

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------------- |
| vuokraajan | pakollinen |  `{tenant}` Pyynnön polku arvo avulla voidaan hallita sitä, kuka voi kirjautua sovellus.  Sallittu arvo on vuokraajan tunnukset, kuten `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` tai `contoso.onmicrosoft.com` tai `common` vuokraajan riippumattomalla tunnusten varten |
| client_id | pakollinen | Sovelluksen kun rekisteröityjä Azure AD määritetty sovellustunnus. Voit etsiä perinteinen Azure-portaalissa. Valitse **Active Directory**, valitse kansio, sovellus napsauttamalla ja valitse **määrittäminen** |
| grant_type | pakollinen | On oltava `authorization_code` luvan koodin kulkuun. |
| koodi | pakollinen | `authorization_code` , Jotka olet hankkinut edellisessä osassa   |
| redirect_uri | pakollinen | Sama `redirect_uri` arvo, jota käytettiin hankkia `authorization_code`. |
| client_secret | pakollinen web Apps-sovellukset | Sovelluksen toiminta, jonka loit sovelluksen rekisteröinti-portaalissa, kun sovellus.  Se olisi ei käytetä alkuperäisen-sovellukseen, koska client_secrets ei voi tallentaa luotettavasti laitteissa.  Tarvitaan web Apps-sovellusten ja verkko-ohjelmointirajapinnan, jotka voivat tallentaa `client_secret` suojatusti palvelinpuolen käyttöön. |
| resurssi | Jos määritetty luvan koodi-pyynnössä, muita valinnaisia | Verkko-Ohjelmointirajapinnan (suojatun resurssi) sovelluksen tunnus URI.
Etsi sovellus tunnus-URI Azure hallinta-portaalissa, valitse **Active Directory**, hakemisto, valitse sovellus ja valitse sitten **Määritä**.

### <a name="successful-response"></a>Onnistuneiden vastaus

Azure AD palauttaa access-tunnuksen onnistuneen vastauksen yhteydessä. Pienennä verkon kutsut asiakassovellus ja niihin liittyvät viive-asiakassovellus olisi välimuistiin suojaustunnuksen elinkaaren, joka on määritetty OAuth 2.0-vastauksen access tunnusten. Määrittääksesi suojaustunnuksen elinaika jommallakummalla `expires_in` tai `expires_on` parametriarvot.

Jos verkko-Ohjelmointirajapinnan resurssin palauttaa `invalid_token` virhekoodi, tämä saattaa johtua resurssi on määrittänyt, että tunnus on vanhentunut. Jos asiakas- ja kello-ajat ovat eri (eli "aika jakauman.vinous"), resurssi kannattaa ehkä päättynyt, ennen kuin tunnuksen poistetaan välimuistiin tunnuksen. Jos näin tapahtuu, poista tunnuksen välimuistista, vaikka se on edelleen laskettu aikana.

Onnistuneiden vastauksen näyttää tältä:

```
{
  "access_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1388444763",
  "resource": "https://service.contoso.com/",
  "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA",
  "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
"id_token": " eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83ZmU4MTQ0Ny1kYTU3LTQzODUtYmVjYi02ZGU1N2YyMTQ3N2UvIiwiaWF0IjoxMzg4NDQwODYzLCJuYmYiOjEzODg0NDA4NjMsImV4cCI6MTM4ODQ0NDc2MywidmVyIjoiMS4wIiwidGlkIjoiN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlIiwib2lkIjoiNjgzODlhZTItNjJmYS00YjE4LTkxZmUtNTNkZDEwOWQ3NGY1IiwidXBuIjoiZnJhbmttQGNvbnRvc28uY29tIiwidW5pcXVlX25hbWUiOiJmcmFua21AY29udG9zby5jb20iLCJzdWIiOiJKV3ZZZENXUGhobHBTMVpzZjd5WVV4U2hVd3RVbTV5elBtd18talgzZkhZIiwiZmFtaWx5X25hbWUiOiJNaWxsZXIiLCJnaXZlbl9uYW1lIjoiRnJhbmsifQ.”
}

```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| access_token | Pyydetty käyttöoikeustietue. Sovelluksen voi todentaa suojatun resurssi, kuten verkko-Ohjelmointirajapinnan tunnuksessa avulla. |
| token_type | Ilmaisee tunnuksen tyyppi-arvo. Ainoa, joka tukee Azure AD on haltijan. Saat lisätietoja haltijan-tunnukset [OAuth2.0 luvan Framework: haltijan suojaustunnuksen käyttö (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)  |
| expires_in | Kuinka kauan käyttöoikeustietue on voimassa (sekunteina). |
| expires_on | Kun access-tunniste vanhentuu aika. Päivämäärän esitetään sekunteina-1970-01 – 01T0:0:0Z UTC-ajan päättymisaika asti. Tätä arvoa käytetään välimuistiin tunnusten elinkaaren määrittämiseen. |
| resurssi | Verkko-Ohjelmointirajapinnan (suojatun resurssi) sovelluksen tunnus URI.|
| laajuus | Tekeytymistaso käyttöoikeudet-asiakassovellukseen. Oletusasetusten mukaan `user_impersonation`. Suojatun resurssin omistaja voi rekisteröidä muita arvoja Azure AD.|
| refresh_token |  Tunnuksen OAuth 2.0-päivityksen. Sovellus käyttää tunnuksessa hankkia lisää käyttöoikeuksia tunnusten nykyisen käyttöoikeustietue päätyttyä.  Päivitä tunnusten ovat pitkäikäiset ja avulla voidaan säilyttää resurssien käytön pitkän ajan. |
| id_token | Allekirjoittamaton JSON Web suojaustunnuksen (JWT). Sovelluksen can base64Url purkaa tätä tunnuksen käyttäjälle, joka kirjautunut tietojen osia. Sovellus voi välimuistiin arvot ja tuo ne näyttöön, mutta sitä ei kannata ne todennus-ja tietoturva rajat. |

### <a name="jwt-token-claims"></a>JWT suojaustunnuksen saatavat
Arvon JWT-tunnuksen `id_token` parametri on purettu huomioon seuraavat saatavat:

```
{
 "typ": "JWT",
 "alg": "none"
}.
{
 "aud": "2d4d11a2-f814-46a7-890a-274a72a7309e",
 "iss": "https://sts.windows.net/7fe81447-da57-4385-becb-6de57f21477e/",
 "iat": 1388440863,
 "nbf": 1388440863,
 "exp": 1388444763,
 "ver": "1.0",
 "tid": "7fe81447-da57-4385-becb-6de57f21477e",
 "oid": "68389ae2-62fa-4b18-91fe-53dd109d74f5",
 "upn": "frank@contoso.com",
 "unique_name": "frank@contoso.com",
 "sub": "JWvYdCWPhhlpS1Zsf7yYUxShUwtUm5yzPmw_-jX3fHY",
 "family_name": "Miller",
 "given_name": "Frank"
}.
```

`id_token` Parametri sisältää seuraavanlaisia Varaa. Saat lisätietoja JSON web tunnusten [JWT IETF luonnos määritykset](http://go.microsoft.com/fwlink/?LinkId=392344). Lisätietoja suojaustunnuksen tyypit ja saatavat Lue [tuettu tunnuksen ja varaa tyypit](active-directory-token-and-claims.md)

| Ryhmän tyyppi | Kuvaus |
|------------|-------------|
| aud | Tunnuksen osallistujat. Kun tunnus annetaan asiakassovellus, yleisön on `client_id` asiakkaan.
| eksponentti | Vanhenemisen aika. Aika, jolloin tunnuksen vanhenee. Kelvollinen tunnuksen nykyinen päivämäärä ja kellonaika on oltava pienempi tai yhtä suuri kuin `exp` arvo. Aika esitetään sekunteina-1970 1 tammikuussa (1970-01-01T0:0:0Z) tunnus on annettu saakka UTC-aika. |
| family_name | Käyttäjän viimeksi nimi tai sukunimi. Sovellus voi näyttää tämän arvon. |
| given_name | Käyttäjän etunimi. Sovellus voi näyttää tämän arvon. |
| IAT | Myöntänyt aikaan. Aika, kun JWT on annettu. Aika esitetään sekunteina-1970 1 tammikuussa (1970-01-01T0:0:0Z) tunnus on annettu saakka UTC-aika. |
| ISS | Määrittää tunnuksen myöntäjän |
| NBF | Ei ennen aika. Aika, jolloin tunnus tulee voimaan. Kelvollinen tunnuksen nykyinen päivämäärä ja kellonaika on oltava suurempi tai yhtä suuri kuin Nbf-arvo. Aika esitetään sekunteina-1970 1 tammikuussa (1970-01-01T0:0:0Z) tunnus on annettu saakka UTC-aika. |
| OID | Käyttäjäobjektin Azure AD-tunnuksen objektin (tunnus). |
| Sub | Suojaustunnuksen aihe tunnus. Tämä on jatkuva ja pysyvä tunnus käyttäjälle, joka kuvaa tunnuksen. Käytä tätä arvoa-välimuistin logiikan. |
| TID | Vuokraajan tunnus (ID), joka on myöntänyt tunnuksen Azure AD-vuokraajan. |
| unique_name | Yksilöllinen tunnus, joka voidaan näyttää käyttäjälle. Tämä on yleensä täydellinen käyttäjätunnus (UPN). |
| täydellisen käyttäjätunnuksen | Käyttäjän pääasiallista käyttäjän nimi. |
| ver | Versio. JWT tunnuksen, yleensä 1.0 versio. |

### <a name="error-response"></a>Virhe vastausta

Suojaustunnuksen liittoutumispalvelujen päätepisteen virheet ovat HTTP-virhekoodit, koska asiakas kutsuu suojaustunnuksen liittoutumispalvelujen päätepisteen suoraan. HTTP-tilakoodin lisäksi Azure AD-tunnuksen liittoutumispalvelujen päätepisteen palauttaa objekteja, jotka kuvaavat virheen JSON tiedoston.

Esimerkki virhe vastauksen näyttää tältä:

```
{
  "error": "invalid_grant",
  "error_description": "AADSTS70002: Error validating credentials. AADSTS70008: The provided authorization code or refresh token is expired. Send a new interactive authorization request for this user and resource.\r\nTrace ID: 3939d04c-d7ba-42bf-9cb7-1e5854cdce9e\r\nCorrelation ID: a8125194-2dc8-4078-90ba-7b6592a7f231\r\nTimestamp: 2016-04-11 18:00:12Z",
  "error_codes": [
    70002,
    70008
  ],
  "timestamp": "2016-04-11 18:00:12Z",
  "trace_id": "3939d04c-d7ba-42bf-9cb7-1e5854cdce9e",
  "correlation_id": "a8125194-2dc8-4078-90ba-7b6592a7f231"
}
```
| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |
| error_codes | STS tietyn virhekoodit, jotka auttavat diagnostiikka luettelo. |
| aikaleima | Aika, jolloin virhe. |
| trace_id | Yksilöllinen tunnus, jotka auttavat diagnostiikka pyynnön.  |
| correlation_id | Yksilöllinen tunnus pyynnön, jotka auttavat diagnostiikka osien välillä.|

#### <a name="http-status-codes"></a>HTTP-tilakoodin

Seuraavassa taulukossa on lueteltu HTTP-tilan koodit, joka palauttaa suojaustunnuksen liittoutumispalvelujen päätepiste. Joissakin tapauksissa virhekoodi riittää kuvaamaan vastausta, mutta muokkaamista, sinun on mukana JSON asiakirjan jäsentää ja tarkistaa sen virhekoodi.

| HTTP-koodi | Kuvaus |
|-----------|-------------|
| 400       | Oletusarvoisen HTTP-koodi. Useimmiten ja on yleensä Vääränlainen pyynnön. Korjaa ja Lähetä pyyntö. |
| 401       | Todennus epäonnistui. Esimerkiksi pyynnön puuttuu client_secret-parametri.|
| 403       | Todennus epäonnistui. Esimerkiksi käyttäjä ei ole oikeutta käyttää resurssi. |
| 500       | Palvelu-palvelussa on tapahtunut sisäinen virhe. Yritä pyynnön. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Virhekoodit suojaustunnuksen päätepisteen virheet

| Virhekoodi | Kuvaus | Asiakastoiminto |
|------------|-------------|---------------|
| invalid_request | Protokollavirhe, kuten tarvittavat funktiosta. | Korjaa ja Lähetä pyyntö |
| invalid_grant | Todennus-koodi on virheellinen tai on vanhentunut. | Kokeile uuden pyynnön `/authorize` päätepiste |
| unauthorized_client | Todennettu asiakas ei ole oikeutta käyttää käyttöoikeuksien myöntäminen tällaista. | Tämä tapahtuu yleensä, kun asiakassovellus ei ole rekisteröity Azure AD- tai ei ole lisätty käyttäjän Azure AD-vuokraajan. Sovellus voi käyttäjää ja asentamalla sovelluksen ja lisäät sen Azure AD-ohjepaketti. |
| invalid_client | Asiakkaan todennus epäonnistui. | Asiakkaan käyttäjätiedot eivät ole kelvollisia. Jos haluat korjata, sovelluksen järjestelmänvalvojan päivittää tunnistetiedot. |
| unsupported_grant_type | Todennus-palvelin ei tue käyttöoikeuksien myöntäminen tyyppi. | Muuttaa myöntäminen pyynnön. Virhe tällaista suoritetaan vain kehityksen aikana ja alkuperäinen testauksen aikana. |
| invalid_resource | Kohteen resurssi on virheellinen, koska sitä ei ole, Azure AD Etkö löydä etsimääsi tai se ei ole määritetty oikein. | Tämä ilmaisee resurssi, jos se on olemassa, ei ole määritetty alihallintaan. Sovellus voi käyttäjää ja asentamalla sovelluksen ja lisäät sen Azure AD-ohjepaketti. |
| interaction_required | Pyyntö edellyttää käyttäjän toimia. Esimerkiksi todennus-vaihe on pakollinen. | Yritä pyyntö, jonka samalle resurssille. |
| temporarily_unavailable | Palvelin on tilapäisesti varattu ja käsitellä pyynnön. | Yritä pyynnön. Asiakassovellus voi olla selitys käyttäjälle, että sen vastaus on viivästyy tilapäinen.|

## <a name="use-the-access-token-to-access-the-resource"></a>Access-tunnuksen avulla voit käyttää resurssi

Nyt kun olet hankkinut onnistuneesti `access_token`, voit käyttää tunnuksen pyynnöt verkko-ohjelmointirajapinnan, mukaan lukien sen `Authorization` otsikko. [RFC 6750](http://www.rfc-editor.org/rfc/rfc6750.txt) määrityksen kerrotaan, miten suojatun resursseihin pyyntöjen haltijan tunnusten avulla.

### <a name="sample-request"></a>Esimerkki pyynnöstä

```
GET /data HTTP/1.1
Host: service.contoso.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ
```

### <a name="error-response"></a>Virhe vastausta

Suojatun resurssit, jotka toteuttavat RFC 6750 ongelman HTTP-tilakoodin. Jos pyyntö ei ole todennuksen tunnistetiedot tai puuttuu tunnuksen, vastaus sisältää `WWW-Authenticate` otsikko. Kun pyyntö epäonnistuu, resurssi-palvelin vastaa HTTP-tilakoodin ja virhekoodi.

Seuraavassa on esimerkki epäonnistua vastausta, kun pyyntö ei ole haltijatunnukseen:

```
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer authorization_uri="https://login.window.net/contoso.com/oauth2/authorize",  error="invalid_token",  error_description="The access token is missing.",
```

#### <a name="error-parameters"></a>Virhe parametrit

| Parametri | Kuvaus |
|-----------|-------------|
| authorization_uri | URI (fyysinen päätepisteen) todennus-palvelimeen. Tätä arvoa käytetään myös lisätietoja palvelimen käyttämistä etsiminen päätepisteen haku-näppäintä. <p><p> Asiakas on vahvistettava luvan palvelin on luotettu. Kun resurssi on suojattu Azure AD-on riittävä varmistaaksesi, että URL-osoite alkaa https://login.windows.net tai toisen hostname, joka tukee Azure AD. Vuokraajan kielikohtaiset resurssin tulee aina palauttaa vuokraajan kielikohtaiset luvan URI. |
| Virhe | Koodin virhearvon määritelty [OAuth 2.0 luvan Framework](http://tools.ietf.org/html/rfc6749)osan 5.2.|
| error_description | Virheen yksityiskohtainen kuvaus. Viestiä ei ole tarkoitus olla loppukäyttäjien friendly.|
| resurssin_tunnus | Palauttaa resurssin yksilöllinen. Asiakassovellus käyttää tunnus arvona `resource` parametri, kun se pyytää resurssin tunnus. <p><p> On erittäin tärkeää vahvistamiseksi arvoksi asiakassovellus, muuten haittaohjelmien palvelun välttämättä voi aiheuttaa **oikeudet käyttöoikeuksien** hyökkäys <p><p> Suositeltu toimintasuunnitelma estää hyökkäys on varmistamaan, että `resource_id` vastaa kantaluvun verkko-Ohjelmointirajapinnan URL-osoite, jota voi käyttää. Jos https://service.contoso.com/data käytetään, esimerkiksi `resource_id` voi olla htttps://service.contoso.com/. Asiakassovellus on hylkää `resource_id` , joka ei ala merkillä Perusosoitteen ellei vaihtoehtoinen luotettavasti vahvistamiseksi tunnus. |

#### <a name="bearer-scheme-error-codes"></a>Haltijan värimallin virhekoodit

RFC 6750 määrityksen määrittää resursseille, jotka käyttävät otsikkoa ja haltijan värimallin käyttäminen vastauksen seuraavia virheitä.

| HTTP-tilakoodi | Virhekoodi | Kuvaus | Asiakastoiminto |
|------------------|------------|-------------|---------------|
| 400 | invalid_request | Pyyntö ei ole muotoiltu. Esimerkiksi se on ehkä puuttuu parametri tai käyttämällä sama parametri kahdesti. | Korjaa virhe ja yritä uudelleen pyynnön. Virhe tällaista suoritetaan vain kehityksen aikana ja havaita alkuperäinen testauksessa. |
| 401 | invalid_token   | Access-tunnuksen on puuttuu, virheellinen tai on kumottu. Error_description-parametrin arvo on lisää yksityiskohtia. |  Pyydä uuden tunnuksen todennus-palvelimesta. Uuden tunnuksen epäonnistuu, jos on ilmennyt odottamaton virhe. Lähetä käyttäjän ja yritä uudelleen virhesanoma satunnaisia viiveitä jälkeen. |
| 403 | insufficient_scope | Access-tunnuksen ei sisällä tarvittavat resurssin tekeytyminen oikeudet. | Lähetä uusi luvan pyyntö luvan päätepiste. Jos vastaus sisältää laajuus-parametri, laajuus-arvoa käytetään resurssin pyynnön. |
| 403 | insufficient_access | Tunnuksen aihe ei ole käyttämään resurssia tarvittavat oikeudet. | Käyttäjää käyttämään eri tilillä tai pyytää oikeutta määritettyä resurssia. |

## <a name="refreshing-the-access-tokens"></a>Access-tunnusten päivittäminen

Access-tunnusten ovat lyhytkestoinen ja on päivitettävä, kun ne päättyy Jatka käyttäminen resurssit. Voit päivittää `access_token` lähettämällä toiseen `POST` pyytää `/token` päätepisteen, mutta aika-antamisen `refresh_token` sijaan `code`.

Päivitä tunnusten ei ole määritetty käyttöajan. Päivitä tunnusten käyttöajan ovat yleensä suhteellisen pitkää. Kuitenkin joissakin tapauksissa päivityksen tunnusten päättyy, on kumottu tai ei ole riittäviä oikeuksia haluamasi toiminnon. Sovellus on tulossa ja käsitellä suojaustunnuksen liittoutumispalvelujen päätepisteen oikein palauttamat virheet.

Kun saat vastauksen ja Päivitä tunnuksen virhesanoma, Päivitä nykyisen tunnuksen Hylkää ja pyytää uuden luvan koodin tai käyttää tunnuksen. Erityisesti, kun käyttämällä päivityksen tunnussanoma luvan koodin Grant-työnkulussa Jos saat vastauksen, jossa `interaction_required` tai `invalid_grant` virhekoodit, Hylkää päivityksen tunnuksen ja Pyydä uusi todennus-koodi.

Otoksen pyynnön (Voit myös käyttää **yleisiä** päätepisteen) **vuokraajan kielikohtaiset** päätepisteelle saat uusi access suojaustunnuksen käyttäen päivityksen tunnus näyttää tältä:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&grant_type=refresh_token
&resource=https%3A%2F%2Fservice.contoso.com%2F
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```
| Parametri | Kuvaus |
|-----------|-------------|
| access_token | Uusi käyttöoikeustietue, joka on pyydetty.|
| expires_in   | Tunnuksen jäljellä oleva käyttöaika sekunteina. Tyypillinen arvo 3600 (yksi tunti). |
| expires_on   | Päivämäärä ja aika, jona tunnuksen vanhenee. Päivämäärän esitetään sekunteina-1970-01 – 01T0:0:0Z UTC-ajan päättymisaika asti. |
| refresh_token | Uusi OAuth 2.0-refresh_token, jonka avulla voidaan pyytää uuden access-tunnuksia, kun yksi vastauksessa vanhenee. |
| resurssi     | Määrittää suojatun resurssi, joka käyttöoikeustietue voidaan käyttää access. |
| laajuus        | Tekeytymistaso käyttöoikeudet alkuperäisen-asiakassovellukseen. Oletus-käyttöoikeus on **user_impersonation**. Kohteen resurssin omistaja voi rekisteröidä vaihtoehtoiset arvot Azure AD. |
| token_type   | Tunnuksen tyyppi. Ainoa tuettu arvo on **haltijan**. |

### <a name="successful-response"></a>Onnistuneiden vastaus

Näyttää onnistuneen suojaustunnuksen vastausta seuraavasti:

```
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1460404526",
  "resource": "https://service.contoso.com/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1THdqcHdBSk9NOW4tQSJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0MDg2MywibmJmIjoxMzg4NDQwODYzLCJleHAiOjEzODg0NDQ3NjMsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6IjY4Mzg5YWUyLTYyZmEtNGIxOC05MWZlLTUzZGQxMDlkNzRmNSIsInVwbiI6ImZyYW5rbUBjb250b3NvLmNvbSIsInVuaXF1ZV9uYW1lIjoiZnJhbmttQGNvbnRvc28uY29tIiwic3ViIjoiZGVOcUlqOUlPRTlQV0pXYkhzZnRYdDJFYWJQVmwwQ2o4UUFtZWZSTFY5OCIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiZ2l2ZW5fbmFtZSI6IkZyYW5rIiwiYXBwaWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctODkwYS0yNzRhNzJhNzMwOWUiLCJhcHBpZGFjciI6IjAiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJhY3IiOiIxIn0.JZw8jC0gptZxVC-7l5sFkdnJgP3_tRjeQEPgUn28XctVe3QqmheLZw7QVZDPCyGycDWBaqy7FLpSekET_BftDkewRhyHk9FW_KeEz0ch2c3i08NGNDbr6XYGVayNuSesYk5Aw_p3ICRlUV1bqEwk-Jkzs9EEkQg4hbefqJS6yS1HoV_2EsEhpd_wCQpxK89WPs3hLYZETRJtG5kvCCEOvSHXmDE6eTHGTnEgsIk--UlPe275Dvou4gEAwLofhLDQbMSjnlV5VLsjimNBVcSRFShoxmQwBJR_b2011Y5IuD6St5zPnzruBbZYkGNurQK63TJPWmRd3mbJsGM0mf3CUQ",
  "refresh_token": "AwABAAAAv YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfcUl4VBbiSHZyd1NVZG5QTIOcbObu3qnLutbpadZGAxqjIbMkQ2bQS09fTrjMBtDE3D6kSMIodpCecoANon9b0LATkpitimVCrl PM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4rTfgV29ghDOHRc2B-C_hHeJaJICqjZ3mY2b_YNqmf9SoAylD1PycGCB90xzZeEDg6oBzOIPfYsbDWNf621pKo2Q3GGTHYlmNfwoc-OlrxK69hkha2CF12azM_NYhgO668yfmVCrl-NyfN3oyG4ZCWu18M9-vEou4Sq-1oMDzExgAf61noxzkNiaTecM-Ve5cq6wHqYQjfV9DOz4lbceuYCAA"
}
```

### <a name="error-response"></a>Virhe vastausta

Esimerkki virhe vastauksen näyttää tältä:

```
{
  "error": "invalid_resource",
  "error_description": "AADSTS50001: The application named https://foo.microsoft.com/mail.read was not found in the tenant named 295e01fc-0c56-4ac3-ac57-5d0ed568f872.  This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant.  You might have sent your authentication request to the wrong tenant.\r\nTrace ID: ef1f89f6-a14f-49de-9868-61bd4072f0a9\r\nCorrelation ID: b6908274-2c58-4e91-aea9-1f6b9c99347c\r\nTimestamp: 2016-04-11 18:59:01Z",
  "error_codes": [
    50001
  ],
  "timestamp": "2016-04-11 18:59:01Z",
  "trace_id": "ef1f89f6-a14f-49de-9868-61bd4072f0a9",
  "correlation_id": "b6908274-2c58-4e91-aea9-1f6b9c99347c"
}
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion syyn todennuksen virhe.  |
| error_codes | STS tietyn virhekoodit, jotka auttavat diagnostiikka luettelo. |
| aikaleima | Aika, jolloin virhe. |
| trace_id | Yksilöllinen tunnus, jotka auttavat diagnostiikka pyynnön.  |
| correlation_id | Yksilöllinen tunnus pyynnön, jotka auttavat diagnostiikka osien välillä.|

Virhekoodit ja suositeltu toiminnon kuvauksen Katso [virhekoodit suojaustunnuksen päätepisteen virheet](#error-codes-for-token-endpoint-errors).
