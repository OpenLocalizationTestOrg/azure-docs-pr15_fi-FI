<properties
    pageTitle="Sovelluksen rekisteröinti portaalin ohjeaiheet | Microsoft Azure"
    description="Kuvaus Microsoft-sovelluksen rekisteröinnin portal useisiin ominaisuuksiin."
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

# <a name="app-registration-reference"></a>Sovelluksen rekisteröinti viittaus
Tässä tiedostossa on yhteydessä ja kuvauksia Microsoft sovelluksen rekisteröinnin Portal [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)eri ominaisuuksia.

## <a name="my-applications"></a>Omat sovellukset
Tämä luettelo sisältää kaikki sovelluksesi rekisteröity Azure AD-v2.0 päätepisteen kanssa käytettäväksi.  Nämä sovellukset voivat kirjautua sisään Microsoft-tili ja työpaikan tai oppilaitoksen tilit Azure Active Directorysta sekä oman tilin käyttäjille.  Saat lisätietoja Azure AD-v2.0 päätepisteen artikkelissa Microsoftin [v2.0 yleiskatsaus](active-directory-appmodel-v2-overview.md).  Nämä sovellukset voidaan myös voi integroida Microsoft tilin todennus päätepisteen `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK-sovellukset
Tämä luettelo sisältää kaikki sovelluksesi rekisteröity ainoastaan Microsoft-tilin kanssa käytettäväksi.  Ne eivät ole käytössä Azure Active Directory muun kanssa käytettäväksi.  Näin saat näkyviin kaikki sovellukset, jotka oli aiemmin rekisteröidyn MSA developer-portaalissa `https://account.live.com/developers/applications`.  Kaikki toiminnot, jotka olet aiemmin suorittaa `https://account.live.com/developers/applications` voidaan nyt tehdä tässä uudessa portaalissa `https://apps.dev.microsoft.com`.  Jos sinulla on muita kysymyksiä Microsoft-tili-sovellusten tietoja, ota yhteyttä.

## <a name="application-secrets"></a>Sovelluksen tietoja
Sovelluksen tietoja ovat tunnistetiedot, jotka sallivat luotettava [asiakkaan todennus](http://tools.ietf.org/html/rfc6749#section-2.3) ja Azure AD sovelluksen.  OAuth & OpenID yhteyden sovelluksen tietoja usein kutsutaan `client_secret`.  V2.0-protokollan jokin sovellus, joka vastaanottaa suojaustunnus web käytettävissä sijainnissa (käyttämällä `https` värimallin) on käytettävä sovelluksen salaisuus Azure AD yhteydessä kyseisen suojaustunnus lunastushinta selvittäminen.  Lisäksi kaikki native client kyseisen recieves tunnukset laite kielletty sovelluksen salaisuus avulla voit suorittaa asiakkaan käyttöoikeuden tarkistaminen, jotta tietoja suojaamattoman ympäristöissä tallennustilaan.

Kunkin sovelluksen voi olla kaksi hakemuksen tietoja vaiheissa samanaikaisesti.  Yllä kaksi tietoja on suoritettava säännöllisiä avaimen palauttaminen usean sovelluksen koko ympäristön ablilty.  Kun olet siirtänyt sovelluksesi uusi salaisuus kokonaisuudessaan, voit poistaa vanhan toiminta ja valmistella uuden.

Tällä hetkellä vain kahdentyyppisiä sovelluksen tietoja valitseminen sovelluksen rekisteröinti-portaalissa.  Valitsemalla **Luo uusi salasana** Luo ja Tallenna salaisuus vastaaviin tietovaraston, jota voit käyttää sovelluksen.  Valitsemalla **Luo uusi avainpari** Luo uusi julkinen/yksityinen avaimen lainausmerkeiksi, voit ladata ja käyttää asiakkaan todennusta Azure AD.

## <a name="profile"></a>Profiili
Sovelluksen rekisteröinti portaalin profiili-osassa voidaan sovelluksen kirjautumissivu mukauttamiseen.  Voit muuttaa sivun sovelluksen logoa, palvelun URL-osoite ja tietosuojatiedot kirjautuminen tällä hetkellä.  Logon on oltava läpinäkyvän 48 x 48 tai 50 x 50 kuvapiste kuvan GIF-, PNG- tai JPEG-tiedostossa, joka on 15 KB tai Pienennä.  Kokeile arvojen muuttaminen ja tarkasteleminen tuloksen etumerkin sivun!

## <a name="live-sdk-support"></a>Live SDK-tuki
Kun otat "Live SDK tuki", minkä tahansa sovelluksen tietoja, voit luoda valmisteltu suoraan Azure AD- ja Microsoft Account tietoja tallennetuista tiedoista.  Tämä sallii sovelluksesi integroida Microsoft Account service (login.live.com).  Jos haluat luoda sovelluksen käyttämällä Microsoft-Account suoraan (eikä joko Azure AD-v2.0 päätepisteen), sinun olisi varmistaa, että Live SDK-tuki on otettu käyttöön.

Live SDK tuen käytöstä varmistat, että sovelluksen toiminta on kirjoitettu vain Azure AD-tiedot tallennetaan.  Azure AD-tietojen kaupan kattaa yrityksen arvosanojen asetukset, joka mahdollistaa sen tiettyjä standardeja, kuten fisma – yhteensopivuus.  Jos otat Live SDK-tuki, sovelluksesi ei voi päästä noudattaminen joitakin näitä vaatimuksia.

Jos aiot käyttää Azure AD-v2.0 päätepistettä ainoastaan koskaan, voit poistaa turvallisesti Live SDK-tukeen.

