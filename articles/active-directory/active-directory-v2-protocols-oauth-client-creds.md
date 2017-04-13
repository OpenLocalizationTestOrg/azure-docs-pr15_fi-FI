
<properties
    pageTitle="Azure AD v2.0 OAuth asiakkaan tunnistetiedot työnkulku | Microsoft Azure"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>v2.0 protokollat - OAuth 2.0 asiakkaan käyttäjätiedot työnkulku

[OAuth 2.0 asiakkaan käyttäjätiedot myöntää](http://tools.ietf.org/html/rfc6749#section-4.4), kutsutaan joskus myös "kahden osapuolen välisiä OAuth" voidaan käyttää web isännöidään resursseja käyttämällä sovelluksen tunnus.  Sitä käytetään tavallisesti palvelinten vuorovaikutukset, joita on suoritettava ilman käyttäjä heti precense taustalla.  Sovellusten seuraavanlaisiin viitataan usein **daemonit** tai **palvelutilejä**.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

Tavallisesti kolme "kolmen osapuolen välisiä OAuth-" asiakassovellus myönnetään käyttöoikeus resurssin tietyn käyttäjän puolesta.  Käyttöoikeus on **delegoinut** käyttäjältä-sovellukseen, yleensä [suostumus](active-directory-v2-scopes.md) prosessin aikana.  Kuitenkin asiakkaan tunnistetiedot-työnkulussa käyttöoikeudet myönnetään **suoraan** sovelluksesta.  Kun sovellus esittää resurssin tunnistetta, resurssi pakottaa sovelluksessa on lupa suorittaa jonkin toiminnon - joitakin käyttäjällä on lupa ei.

## <a name="protocol-diagram"></a>Protocol (protokolla)-kaavio
Asiakkaan tunnistetiedot kulun suunnilleen tämän näköinen – eri vaiheet on kuvattu alla.

![Asiakkaan tunnistetiedot työnkulku](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Suora saaminen 
Sovelluksen yleensä vastaanottaa suoraan todennus käyttämään resurssin kahdella tavalla: käyttöoikeusluettelo, resurssi- tai sovelluksen käyttöoikeuksia varauksen Azure AD kautta.  Useilla tavoilla muiden resurssi voi valita, määritä sen asiakkaat ja kunkin resurssin palvelimen voi valita menetelmä, jota tarkoituksenmukaisessa sen sovelluksen.  Nämä kaksi menetelmää ovat yleisimpiä Azure AD ja asiakkaiden ja resursseja, jotka haluat suorittaa asiakkaan tunnistetiedot kulun reccommended.

### <a name="access-control-lists"></a>Käyttöoikeusluettelot
Tietyn resurssin toimittaja voi Pakota sovellus tunnuksista tietää ja antaa joitakin erityisesti käyttöoikeustaso kohdeluetteloon perustuvan todennus-valintaruutu.  Kun resurssi vastaanottaa tunnusta v2.0 päätepisteen, se toistaa tunnuksen ja Pura asiakkaan tunnus- `appid` ja `iss` saatavat.  Jälkeen voit verrata, että jotkin käyttöoikeusluettelo (Käyttöoikeusluettelon) vastaan sovelluksen se ylläpitää.  Maksutavan käyttöoikeusluettelon ja rakeisuuden voivat vaihdella huomattavasti-resurssi, resurssi.

Yleisiä Käyttötapaus tällaisten käyttöoikeusluettelot on testata rönsyt web-sovelluksen tai verkko-ohjelmointirajapinnan.  Verkossa api vain myöntää alijoukkoa sen täydet oikeudet eri asiakkaille.  Jotta voit suorittaa loppuun voit tarkistaa ohjelmointirajapinnan, testiasiakas on luotu, mutta hankkii tunnusten v2.0 päätepisteestä ja lähettää ne ohjelmointirajapinnan.  Ohjelmointirajapinnan voit sitten Käyttöoikeusluettelon testi asiakkaan sovelluksen tunnus ohjelmointirajapinnan koko toimintojen täydet käyttöoikeudet.  Huomaa, että jos sinulla on luettelon palvelustasi, sinun olisi että Vahvista ei vain soittajan `appid`, mutta myös Tarkista, että `iss` tunnuksen on luotettu sekä.

Tämäntyyppisen luvassa yhteistä daemonit ja palvelutilejä, jotka tarvitsevat tietoja omistaa kuluttaja-käyttäjät, joilla on henkilökohtainen Microsoft-tilit.  Organisaatioiden omistamien tietojen on suositeltavaa hankkia tarvittavat luvan sovelluksen perimssions kautta.

### <a name="application-permissions"></a>Sovelluksen käyttöoikeudet
Sijaan käyttöoikeusluettelot ohjelmointirajapinnan voit näyttää **sovelluksen käyttöoikeudet** , voi myöntää sovellukselle.  Sovelluksen käyttöoikeus myönnetään sovelluksen organisaation järjestelmänvalvoja ja voidaan käyttää vain organisaation ja sen työntekijöiden omistamien tietojen käyttämistä varten.  Esimerkiksi Microsoft Graph paljastaa sovelluksen käyttöoikeuksiin:

- Kaikki postilaatikot viestien lukeminen
- Lukemiseen ja kirjoittamiseen sähköpostin kaikki postilaatikot
- Lähetä sähköposti käyttäjänä
- Kansion tietojen lukeminen
- [+ Lisää](https://graph.microsoft.io)

Hankkia sovelluksen nämä oikeudet, jotta voit suorittaa seuraavat vaiheet.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Pyydä käyttöoikeuksia sovelluksen rekisteröinti-portaalissa

- Siirry sovelluksen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)tai [-sovelluksen luominen](active-directory-v2-app-registration.md) , jos omistajuus on vielä vahvistamatta.  Sinun on varmistettava, että sovelluksesi on luonut vähintään yksi sovellus salaisuus.
- **Suora sovelluksen käyttöoikeudet** -osaan ja lisää käyttöoikeuksia, joka edellyttää sovelluksen.
- Varmista, että voit **tallentaa** sovelluksen rekisteröinti

#### <a name="recommended-sign-the-user-into-your-app"></a>(Suositus): käyttäjän kirjautua sovelluksen

Yleensä sovelluksen luominen, joka käyttää sovelluksen käyttöoikeuksia, kun sovellus on pystyttävä muodostamaan sivun tai näkymän, joka sallii Hyväksy sovelluksen käyttöoikeuksien hallinta.  Tällä sivulla voi olla osa sovelluksen kirjautumisen työnkulku-sovelluksen asetuksissa tai oma "yhteyden" tiedonkulun.  Monissa tapauksissa se on järkevää, jos haluat näyttää tämän sovelluksen "yhteyden" Näytä vasta, kun käyttäjä on kirjautunut sisään työpaikan tai oppilaitoksen Microsoft-tili.

Käyttäjän kirjautuminen sovellukseen voit tunnistaa organziation, johon käyttäjä kuuluu ennen heitä pyydetään Hyväksy sovelluksen käyttöoikeudet.  Kun ehdottomasti tarpeen sen avulla voit luoda nopeampaa kokemus organisaation käyttäjien.  Kirjaudu käyttäjän noudattamalla Microsoftin [v2.0 protokolla opetusohjelmat](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Pyydä käyttöoikeuksia directory-järjestelmänvalvoja

Kun olet valmis Pyydä käyttöoikeudet kuin yrityksen järjestelmänvalvoja, voit ohjata käyttäjän v2.0 **suostumus päätepisteen järjestelmänvalvoja**.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| vuokraajan | pakollinen | Kansion alihallintaa, johon haluat pyytää lupaa.  Voit toimittaa GUID-tunnus tai kutsumanimi-muodossa.  Jos et tietää, mitä käyttäjä kuuluu vuokraajan ja haluat, että he voivat kirjautua sisään minkä tahansa vuokraajan, käytä `common`. |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| redirect_uri | pakollinen | Redirect_uri, mihin vastaus lähetetään, kun sovellus käsittelemään.  Se on oltava täsmälleen samat jokin rekisteröidyt portaalissa, mutta se on oltava koodattu URL-osoite ja osia muut polku voi olla redirect_uris. |
| tila | suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa.  Voi olla merkkijonoksi sisällöstä, jotka haluat.  Siinä käytetään koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |

Tässä vaiheessa Azure AD Pakota sitä, että vain vuokraajan järjestelmänvalvoja voivat kirjautua sisään pyynnön suorittamiseen.  Järjestelmänvalvojan pyydetään hyväksymään kaikki suoran käyttöoikeudet, jotka olet pyytänyt, kun sovellus rekisteröinti-portaalissa. 

##### <a name="successful-response"></a>Onnistuneiden vastaus
Jos järjestelmävalvoja hyväksyy sovelluksen käyttöoikeuksia, on onnistunut vastaus:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| vuokraajan | Kansion alihallintaa, johon myöntää sovelluksen käyttöoikeuksia, se pyytänyt guid-muodossa. |
| tila | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa.  Voi olla merkkijonoksi sisällöstä, jotka haluat.  Siinä käytetään koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |
| admin_consent | Asetetaan `True`. |


##### <a name="error-response"></a>Virhe vastausta
Jos järjestelmänvalvoja ei hyväksy sovelluksen käyttöoikeuksia, on epäonnistunut vastaus:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| Virhe | Virhe koodin merkkijonona, joka voidaan luokitella tapahtuvien virheiden tyypit ja voidaan vastata virheitä. |
| error_description | Virhesanomasta, jotka auttavat kehittäjä tunnistaa pääkansion aiheuttaa virheen.  |

Kun olet saanut onnistuneen vastauksen valmistelu päätepisteen-sovelluksen, sovelluksen on saatu se pyytänyt suora sovelluksen käyttöoikeudet.  Voit nyt siirtää tarralle haluamasi resurssin tunnus pyytää.

## <a name="get-a-token"></a>Hae tunnus
Kun olet hankkinut tarvittavat luvan sovelluksen, voit jatkaa hankkiminen access tunnusten ohjelmointirajapinnan varten.  Pääset käyttävä tunnusta tunnistetietojen myöntäminen Lähetä viesti pyyntö `/token` v2.0 päätepisteen:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parametri | | Kuvaus |
| ----------------------- | ------------------------------- | --------------- |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| laajuus | pakollinen | Välitetty arvo `scope` pyyntö parametri on oltava resurssitunnus (sovelluksen tunnus URI) haluamasi resurssin kiinnitetty `.default` liite.  Jotta annettu Microsoft Graph kuten arvon on oltava `https://graph.microsoft.com/.default`.  Tämä arvo ilmoittaa, että kaikki suoran käyttöoikeudet, kun sovellus on määritetty, se on annettava niistä liittyvät haluamasi resurssin tunnus v2.0 päätepisteen. |
| client_secret | pakollinen | Sovelluksen salainen, jotka olet luonut rekisteröinti-portaalissa, kun sovellus. |
| grant_type | pakollinen | On oltava `client_credentials`. | 

#### <a name="successful-response"></a>Onnistuneiden vastaus
Onnistuneiden vastauksen vievät lomake:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parametri | Kuvaus |
| ----------------------- | ------------------------------- |
| access_token | Pyydetty käyttöoikeustietue. Sovelluksen voi todentaa suojatun resurssi, kuten verkko-Ohjelmointirajapinnan tunnuksessa avulla. |
| token_type | Ilmaisee tunnuksen tyyppi-arvo. Azure AD tukee on ainoa `Bearer`.  |
| expires_in | Kuinka kauan käyttöoikeustietue on voimassa (sekunteina). |

#### <a name="error-response"></a>Virhe vastausta
Virhevastauksen vievät lomakkeen:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

## <a name="use-a-token"></a>Käytä tunnus
Nyt kun olet hankkinut tunnusta, voit käyttää kyseisen tunnuksen halutaan pyynnöt resurssi.  Kun tunnuksen vanhenee, riittää, että toista pyynnön `/token` hankkia ajan tasalla käyttöoikeustietue päätepiste.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Koodiesimerkki
Esimerkki sovelluksen toteuttaa client_credentials myöntää käyttämällä järjestelmänvalvojan suostumus päätepisteen näkyviin viitata Microsoftin [v2.0 daemon koodin malli](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
