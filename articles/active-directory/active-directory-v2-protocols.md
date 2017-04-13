<properties
    pageTitle="Azure AD v2.0 protokollat | Microsoft Azure"
    description="Azure AD-v2.0 päätepisteen tukemat protokollat opas."
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

# <a name="v20-protocols---oauth-20--openid-connect"></a>v2.0 protokollat - OAuth 2.0 & OpenID yhdistäminen

V2.0 päätepisteen käyttää Azure AD identity-muodossa--palvelun alan vakio-protokollia, OpenID yhteyden ja OAuth 2.0.  Palvelun ollessa-standardin mukaisia voi olla kaksi tuotteiden protokollat pieniä eroja.  Tiedot tähän on hyödyllinen, jos haluat kirjoittaa koodin lähettämällä suoraan & käsittely pyyntöjen tai käytä 3rd osapuolen Avaa tietolähdekirjastossa kuin käyttämällä Microsoftin Avaa lähde-kirjastoihin.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Perusteet
Lähes kaikki OAuth & OpenID yhteyden työnkulut-kohdassa on neljä osapuolet Exchangen:

![OAuth 2.0 roolit](../media/active-directory-v2-flows/protocols_roles.png)

- **Todennus-palvelin** on v2.0 päätepiste.  On vastuussa varmistaa, että käyttäjän tunnistetiedot, myöntämistä ja peruutetaan resurssien käytön ja myönnä tunnuksia.  Käytetään myös nimitystä tunnistetietojen toimittaja - suojatusti käsittelee tekemistä käyttäjän tietoja, heillä on käyttöoikeudet ja luottamussuhteet osapuolten työnkulussa.
- **Resurssien omistajan** on yleensä käyttäjä.  On osapuolen, jonka omistaa tiedot, ja sallia kolmansien osapuolten käyttämään nämä tiedot tai resurssi.
- **OAuth-asiakas** on sovelluksesi merkittyä sen sovelluksen ID-tunnuksellasi.  Se on yleensä osapuoli, joka toimii loppukäyttäjän kanssa ja se pyytää tunnusten todennus-palvelimesta.  Asiakas on annettava oikeudet käyttää resurssia resurssin omistaja.
- **Resurssin palvelin** on resurssi tai tietojen sijainti.  Luottaa luvan palvelimen suojatusti todennusta ja määritä OAuth-asiakas, ja haltijan access_tokens avulla voit varmistaa, että resurssin käyttöoikeutta myönnetty.


## <a name="app-registration"></a>Sovelluksen rekisteröinti
Jokaisen sovellusta, joka käyttää v2.0 päätepisteen on rekisteröintiä [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ennen kuin se käyttää OAuth tai OpenID yhteyden avulla.  Sovelluksen rekisteröitymisen kerääminen & muutaman arvon varaaminen sovelluksen:

- **Tunnus** , joka yksilöi sovelluksen
- **Ohjaa URI** tai **Paketin tunnus** , jonka avulla voidaan ohjata vastaukset takaisin sovelluksen
- Muutamia muita skenaarion kielikohtaiset arvoja.

Lisätietoja, lue, miten voit [rekisteröidä sovelluksen](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Päätepisteet
Rekisteröitynyt sovelluksen yhteydessä Azure AD lähettämällä pyynnöt v2.0 päätepisteen:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Missä `{tenant}` voi kestää jonkin neljä eri arvoja:

| Arvo | Kuvaus |
| ----------------------- | ------------------------------- |
| `common` | Avulla käyttäjät, joilla on henkilökohtainen Microsoft-tili- ja työpaikan tai oppilaitoksen tilit Azure Active Directory kirjautumaan sovellukseen. |
| `organizations` | Vain käyttäjät, joilla työpaikan tai oppilaitoksen tili sallii Azure Active Directorysta kirjautumaan sovellukseen. |
| `consumers` | Vain käyttäjät, joilla on henkilökohtainen Microsoft-tilit (MSA), jos haluat kirjautua sovelluksen avulla. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`tai`contoso.onmicrosoft.com` | Vain käyttäjät, joilla on työpaikan tai oppilaitoksen tilit sallii tietyn Azure Active Directory-vuokraajasta kirjautumaan sovellukseen.  Voidaan joko Azure AD-vuokraajan helpossa muodossa toimialuenimi tai vuokraajan guid-tunnus.  |

Lisätietoja siitä, miten voit käsitellä nämä päätepisteet Valitse kunkin sovelluksen laji.

## <a name="tokens"></a>Tunnusten
V2.0 käyttöönoton OAuth 2.0 ja OpenID yhteyden Varmista laaja käyttö haltijan-tunnukset, mukaan lukien esitetään JWTs haltijan-tunnukset. Haltijatunnukseen on kevyt suojaustunnus, joka antaa suojatun resurssille "haltijan"-käyttöoikeudet. Tässä mielessä "haltijan" on osapuolen, joka esittää tunnuksen. Vaikka osapuolen todentamismenetelmä on ensin Azure AD vastaanottamaan haltijatunnukseen, jos tarvittavat vaiheet ei oteta suojaaminen tunnuksen siirron ja tallennustilaa, se voidaan siepata ja käyttää tahattomia osapuoli. Kun joitakin suojauksen tunnusten on valmiita järjestelmä luvattoman estää niiden käyttämisen, haltijan tunnusten ei ole tämä järjestelmä ja on kuljetus-suojatun kanavan, kuten transport layer security (HTTPS). Haltijatunnukseen siirretään Poista, jos jokin mies on keskimmäisen hyökkäys voidaan haittaohjelmien osapuolen hankkia tunnuksen ja käyttää sitä suojatun resurssin luvatonta pääsyä. Samojen suojauksen periaatteiden käyttää, kun tallentaminen tai tallentaminen välimuistiin haltijan-tunnukset myöhempää käyttöä varten. Aina, varmista, että sovelluksesi välittää ja tallentaa haltijan tunnusten turvallisesti. Saat lisätietoja suojausasiat-haltijan-tunnukset, [RFC 6750 vaihe 5](http://tools.ietf.org/html/rfc6750).

Lisätietoja tunnusten v2.0 päätepisteen käyttää erityyppisiä sisältyy [v2.0 päätepisteen tunnuksen viittaus](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protokollat

Jos haluat nähdä Esimerkki jotkin pyynnöt, Aloita mainitun-opetusohjelmat alapuolella.  Jokaisessa vastaa tiettyyn todennus tilanne.  Jos tarvitset apua, tarkistamalla, joka on oikea työnkulku, Kuittaa ulos [tyyppisiä sovelluksia, voit luoda v2.0 kanssa](active-directory-v2-flows.md).

- [Muodosta Mobile ja alkuperäisen OAuth 2.0-sovellukseen](active-directory-v2-protocols-oauth-code.md)
- [Muodosta Web Apps Avaa tunnuksella yhteyden](active-directory-v2-protocols-oidc.md)
- [Implisiittinen OAuth 2.0 mukana sovellusten yksittäisen sivun luominen](active-directory-v2-protocols-implicit.md)
- [Muodosta daemonit tai palvelimen puoli prosesseja OAuth 2.0 asiakkaan tunnuksilla Flow](active-directory-v2-protocols-oauth-client-creds.md)
- Hae tunnusten verkko-Ohjelmointirajapinnan kanssa OAuth 2.0 puolesta Flow (tulossa)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
