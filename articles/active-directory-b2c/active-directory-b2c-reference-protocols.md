<properties
    pageTitle="Azure Active Directory-B2C | Microsoft Azure"
    description="Miten sovellusten luominen suoraan Azure Active Directory-B2C tukemat protokollat avulla."
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

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure AD-B2C: Todennusprotokollat

Azure Active Directory (Azure AD) B2C sisältää tunnistetietojen palvelun sovellukset tukevat kaksi alan vakio protokollat: OAuth 2.0-OpenID yhteyden. Palvelu ei-standardin mukaisia, mutta kaksi tuotteiden protokollat voit kirjoitusasu.  Tämän oppaan tiedot on hyötyä, jos kirjoitat koodisi suoraan lähettämällä ja käsittelemisen pyyntöjen sijaan Avaa tietolähdekirjastossa. On suositeltavaa lukea tämän sivun, ennen kuin voit käsittelevät tietoja kunkin tietyn protokollan. Mutta jos olet jo aiemmin Azure AD B2C, voit siirtyä suoraan [protokolla viittaus apuviivat](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Perusteet
Jokaisen sovellusta, joka käyttää Azure AD B2C on oltava rekisteröity [Azure portal](https://portal.azure.com)B2C hakemistossa. Sovelluksen rekisteröitymisen kerää ja määrittää muutaman arvon sovelluksen:

- **Tunnus** , joka yksilöi sovelluksen.
- **Ohjaa URI** tai **paketti-tunniste** , jonka avulla voidaan ohjata takaisin sovelluksen vastaukset.
- Muutamia muita skenaarion kielikohtaiset arvoja. Lisätietoja on tietoja [rekisteröidä sovelluksen](active-directory-b2c-app-registration.md).

Kun olet rekisteröinyt sovelluksen, se kommunikoi Azure AD kanssa lähettämällä pyynnöt v2.0 päätepisteen:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Lähes kaikki OAuth ja OpenID Yhdistä kulkee neljä osapuolet ovat vaihtoon osallistuvan:

![OAuth 2.0 roolit](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- **Todennus-palvelin** on Azure AD v2.0 päätepiste. Se suojatusti käsittelee mitään liittyvät käyttäjätiedot ja käyttö. Se myös käsittelee luottamussuhteet vuo osapuolten välillä. On vastuussa käyttäjän tunnistetiedot, myöntämistä ja peruutetaan resurssien käytön ja myönnä tunnuksia. Käytetään myös nimitystä tunnistetietojen toimittaja.
- **Resurssien omistajan** on yleensä peruskäyttäjän. Se on osapuoli, joka omistaa tiedot, ja sen sallia kolmansien osapuolten käyttämään nämä tiedot tai resurssi.
- **OAuth-asiakas** on sovelluksen. Tunnistaa sen sovelluksen tunnus. Se on yleensä osapuolen loppukäyttäjien kanssa toimivat. Se myös pyytää tunnusten todennus-palvelimesta. Resurssien omistajan on myönnettävä asiakkaan oikeus käyttää resurssi.
- **Resurssin palvelin** on resurssi tai tietojen sijainti. Se voi luottaa luvan palvelimen suojatusti todennusta ja määritä OAuth-asiakas. Se myös käyttää haltijan access tunnusten varmistaa, että resurssin käyttöoikeutta myönnetty.

## <a name="policies"></a>Käytännöt
Azure AD B2C käytännöt ovat arguably-palvelun tärkeimmistä ominaisuuksista. Azure AD-B2C laajentaa vakio OAuth 2.0 ja OpenID yhteyden protokollat tuomalla käytännöt. Näiden avulla Azure AD-B2C paljon suurempi kuin yksinkertainen todennus- ja suorittamiseen. Käytännöt kuvaavat täysin kuluttaja tunnistetietojen kokemukset, mukaan lukien kirjautuminen, kirjaudu sisään ja profiilin muokkaaminen. Käytännöt voidaan määrittää järjestelmänvalvojan Käyttöliittymä. Hän voi suorittaa todennusta pyyntöjen määräten kysely parametrien avulla. Käytännöt eivät ole OAuth 2.0 ja OpenID yhteyden, perusominaisuudet, joten tekee saapuville ymmärtää ne aika. Lisätietoja on artikkelissa [Azure AD B2C käytäntö-komento-opas](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Tunnusten
Azure AD B2C käyttöönoton OAuth 2.0 ja OpenID yhteyden hyödyntää haltijan-tunnukset, mukaan lukien haltijan-tunnukset, jotka esitetään JSON web tunnusten (JWTs) laaja. Haltijatunnukseen on kevyt suojaustunnus, joka antaa suojatun resurssille "haltijan"-käyttöoikeudet. Haltijan on osapuolen, joka esittää tunnuksen. Azure AD ensin on todentaa osapuoli, ennen kuin se voi vastaanottaa haltijatunnukseen. Mutta jos tarvittavat vaiheet ei oteta suojaaminen tunnuksen siirron ja tallennustilaa, se voidaan siepata ja käyttää tahattomia osapuoli.

Jotkin suojauksen olla valmiita toimintoa, joka estää luvattoman niitä, mutta haltijan tunnusten ei ole se. Ne on kuljetus-suojatun kanavan, kuten transport layer security (HTTPS). Jos haltijatunnukseen siirretään suojatun kanavan ulkopuolella, haittaohjelmien osapuolen käyttää mies-kohdassa-hyökkäys hankkia tunnuksen ja sen avulla suojattujen resurssin luvattomasti käyttöönsä. Samojen suojauksen periaatteiden käyttää, kun haltijan tunnusten säilytetään tai välimuistiin myöhempää käyttöä varten. Aina, varmista, että sovelluksesi välittää ja tallentaa haltijan tunnusten turvallisesti.

Saat lisätietoja haltijan suojaustunnuksen suojausasiat, [RFC 6750 vaihe 5](http://tools.ietf.org/html/rfc6750).

Tarkemmat tiedot ovat tunnusten Azure AD B2C käyttää erityyppisiä ovat käytettävissä [Azure AD-tunnuksen viittaus](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokollat

Kun olet valmis, voit tarkastella esimerkiksi jotkin pyynnöt, voit aloittaa mainitun opetusohjelmassa. Jokaiselle vastaa tiettyyn todennus tilanne. Jos tarvitset apua määrittämiseen, mikä on itsellesi, tutustu artikkeliin [tyyppisiä sovelluksia voit luoda Azure AD B2C avulla](active-directory-b2c-apps.md).

- [Voit muodostaa sähköistä tai alkuperäisen sovellusten OAuth 2.0](active-directory-b2c-reference-oauth-code.md)
- [Web-sovellusten luominen käyttämällä OpenID yhteyden](active-directory-b2c-reference-oidc.md)
