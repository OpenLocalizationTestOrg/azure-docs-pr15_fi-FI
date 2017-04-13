<properties
    pageTitle="Azure AD v2.0 alueet, käyttöoikeudet ja suostumusta | Microsoft Azure"
    description="Azure AD v2.0 päätepisteen, kuten alueet, käyttöoikeudet ja suostumuksen luvan kuvaus."
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

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Vaikutusalueet, käyttöoikeudet ja v2.0 päätepisteen lupaa

Sovellukset, jotka Azure AD integroida noudattamalla tietyn todennus-malli, jonka avulla käyttäjät voivat määrittää, miten sovellus voi käyttää niiden tiedot.  Luvan mallia v2.0 soveltaminen on päivitetty, muuttaminen, miten sovelluksen on käsitellä Azure AD.  Tässä ohjeaiheessa kerrotaan tämän luvan mallin, kuten alueet, käyttöoikeudet ja suostumusta peruskäsitteet.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Käyttöalueen ja käyttöoikeudet

Azure AD toteuttaa [OAuth 2.0](active-directory-v2-protocols.md) -todennus-protokolla, joka on tapa, jolla sallitaan 3 osapuolen sovelluksen web isännöimä resurssien käytön käyttäjän puolesta.  Kaikki web isännöimä resurssit, joiden Azure AD integroituu on resurssitunnisteen tai **Sovelluksen tunnus URI**.  Esimerkiksi Microsoftin WWW-isännöidään resurssien muun muassa seuraavat:

- Office 365 yhdistetty sähköpostin API:`https://outlook.office.com`
- Azure AD-kaavio-Ohjelmointirajapinnan:`https://graph.windows.net`
- Microsoft Graph:`https://graph.microsoft.com`

Sama koskee 3 osapuolen resursseja, jotka on integroitu Azure AD.  Mitään näistä resursseista voit myös määrittää käyttöoikeudet, jotka voidaan jaetaan pienempi joukkojen huomioon resurssin toimintoja.  Luodaan esimerkiksi Microsoft Graph on määritetty muutaman käyttöoikeudet:

- Lue käyttäjän kalenteri
- Kirjoita käyttäjän kalenteriin
- Lähettää sähköpostia käyttäjän
- [+ Lisää](https://graph.microsoft.io)

Nämä käyttöoikeudet määrittämällä resurssi voi olla tarkasti rajattuja päättää sen tietojen ja miten se on määritetty muille.  3 osapuolen-sovellus voi pyytää näiden oikeuksien-käyttäjä - ja käyttäjä on hyväksyttävä käyttöoikeuksia, ennen kuin sovellus voi toimia heidän puolestaan.  Mukaan paloittelun resurssin toimintoja pienempi vertailun 3 osapuolten sovelluksissa on suunniteltu pyytää vain tietyt käyttöoikeudet, jotta voit suorittaa niiden maksu tarvitsemansa.  Ottaa käyttöön myös käyttäjät voivat tiedät tarkalleen, miten sovellus käyttää niiden tiedot niin, että ne ovat varmempi, että sovellus ei toimi odotusten kanssa väärinkäyttömahdollisuuksien takia.

Azure AD- ja OAuth-näiden oikeuksien kutsutaan **alueet**.  Voit myös nähdä niitä kutsutaan **oAuth2Permissions**.  Alueen esitetään Azure AD merkkijonoarvona.  Microsoft Graph-esimerkissä jatkamista, kukin käyttöoikeus laajuus-arvo on:

- Lue käyttäjän kalenterin:`Calendar.Read`
- Kirjoita käyttäjän kalenteriin:`Mail.ReadWrite`
- Lähettää sähköpostia käyttäjän:`Mail.Send`

Sovelluksen pyytää nämä käyttöoikeudet määrittämällä käyttöalueiden v2.0 päätepisteen pyynnöt seuraavalla tavalla.

## <a name="openid-connect-scopes"></a>OpenId yhteyden käyttöalueet

V2.0 soveltaminen OpenID yhteyden on muutama hyvin määritetyn alueet, jotka eivät koske mitään tiettyä resurssi - `openid`, `email`, `profile`, ja `offline_access`.

#### <a name="openid"></a>OpenId

Jos sovelluksen suorittaa kirjautumisen [OpenID yhdistämistä](active-directory-v2-protocols.md#openid-connect-sign-in-flow)käyttämällä, se on pyydettävä `openid` laajuus.  `openid` Laajuus näkyy työpaikan tili suostumusta näytön "Kirjautuminen"-käyttäjänä ja henkilökohtainen Microsoft tili suostumusta näytön "Oman profiilin tarkasteleminen ja muodosta yhteys sovelluksia ja palveluja käyttämällä Microsoft-tiliä"-käyttäjänä.  Nämä oikeudet ottaa käyttöön sovelluksen vastaanottamaan yksilöllinen käyttäjän muodossa `sub` väittää.  Se myös täyttää käyttäjän tiedot päätepisteen sovelluksen käyttöoikeus.  `openid` Laajuus voidaan myös osoitteessa v2.0 suojaustunnuksen päätepisteen hankkia id_tokens, joiden avulla voidaan suojata HTTP puhelut sovelluksen eri osien välillä.

#### <a name="email"></a>Sähköpostin

`email` Vaikutusalueen voi olla mukana pitkin `openid` laajuus ja muihin palveluihin.  Se täyttää käyttäjän ensisijaisen sähköpostiosoitteen muodossa sovelluksen käyttöoikeus `email` väittää.  `email` Vain ryhmän sisältyy tunnuksia, jos sähköpostiosoite on liitetty käyttäjätili, joka ei ole aina kirjainkokoa.  Jos `email` laajuus-sovelluksen laaditaan käsittelemään tapaukset, joissa `email` varaa ei ole tunnuksen.

#### <a name="profile"></a>Profiili

`profile` Vaikutusalueen voi olla mukana pitkin `openid` laajuus ja muihin palveluihin.  Se täyttää käyttäjän tietoja runsaasti sovelluksen käyttöoikeus.  Tämä sisältää, mutta ei ole rajoitettu käyttäjän etunimi, Sukunimi, ensisijainen käyttäjänimi, Objektitunnus ja niin edelleen.  Täydellinen luettelo käytettävissä id_tokens profiilin saatavat, tietyn käyttäjän viitata [v2.0 suojaustunnuksen viittaus](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

[ `offline_access` Laajuus](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) mahdollistaa access resurssien sovelluksen käyttäjän puolesta pitkän ajan kuluessa.  Suostumus työpaikan tili-näytön etsintäalue tulee näkyviin "Käyttää tietoja milloin tahansa"-käyttäjänä.  Henkilökohtainen Microsoft tili-suostumusta, näytön se näkyy "Käyttää omien tietojen milloin tahansa"-käyttäjänä.  Kun käyttäjä hyväksyy `offline_access` laajuus-sovellus otetaan käyttöön vastaanottamaan päivityksen tunnusten v2.0 suojaustunnuksen päätepisteestä.  Päivitä tunnusten ovat pitkäikäiset ja Salli sovelluksen hankkia uuden access tunnusten kuin vanhempia niistä päättyy.

Jos sovellus ei pyydä `offline_access` laajuus, se saa refresh_tokens.  Tämä tarkoittaa, että kun lunastaa [OAuth 2.0 luvan koodin työnkulku](active-directory-v2-protocols.md#oauth2-authorization-code-flow)authorization_code, vain saat takaisin access_token kohteesta `/token` päätepiste.  Kyseisen access_token pysyy voimassa lyhyt tietyn ajan (yleensä tunnin), mutta myöhemmin päättyy.  AT, joka ohjaa käyttäjän on ajankohta, sovelluksen takaisin `/authorize` päätepisteen noutaa uusia authorization_code.  Tämä uudelleenohjaus aikana käyttäjä voi tai ei tarvitse syöttää tunnistetietoja uudelleen tai uudelleen suostuu käyttöoikeudet sen mukaan, sovelluksen tyyppi.

Lisätietoja siitä, miten voit hakea ja käyttää Päivitä tunnuksia, [v2.0 protokolla viittaus](active-directory-v2-protocols.md)viittaa.


## <a name="requesting-individual-user-consent"></a>Yksittäisen käyttäjän lupaa pyytäminen

Luvan [OpenID muodostaa tai OAuth 2.0](active-directory-v2-protocols.md) -pyynnössä sovellus pyytää tarvitsemansa käyttämällä käyttöoikeudet `scope` kyselyn parametri.  Esimerkiksi käyttäjän kirjautuessa sovellukseen sovelluksen lähettää pyynnön seuraavanlaisia (ja rivinvaihdot luettavuuden):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

`scope` Parametri on alueet, jotka sovellus pyytää tilaa erotettu luettelo.  Yksittäisten kunkin alueen ilmaistaan alueen arvo liitettäväksi resurssin tunnus (sovelluksen tunnus URI).  Yllä pyyntö osoittaa, että sovellus on oikeus käyttäjän kalenterin lukea ja lähettää sähköpostia käyttäjän.

Sen jälkeen, kun käyttäjä syöttää tunnistetietoja, v2.0 päätepisteen tarkistaa, **käyttäjä hyväksyy**vastaavia tietueita.  Jos käyttäjä on hyväksynyt ei mitään pyydetyt oikeudet aiemman, v2.0 päätepisteen Pyydä käyttäjälle tarvittavat käyttöoikeudet.  

![Työpaikan tilin suostumusta näyttökuva](../media/active-directory-v2-flows/work_account_consent.png)

Kun käyttäjä hyväksyy käyttöoikeudet, suostumuksen kirjataan niin, että käyttäjä ei ole sallia uudelleen myöhemmin merkki-laajennukset.

## <a name="requesting-consent-for-an-entire-tenant"></a>Pyydetään lupaa koko vuokraajakohteen

Usein, kun organisaatiossa ostaa käyttöoikeuksia tai tilaus-sovellukseen, he haluavat valmistella täysin niiden työntekijöiden.  Osana tätä prosessia yrityksen järjestelmänvalvoja voi myöntää lupaa sovelluksen toimimaan minkä tahansa työntekijän puolesta.  Myöntämällä koko vuokraajakohteen suostumusta organisaation työntekijät ei ilmene suostumusta näytön sovelluksen.

Pyydä suostumusta alihallinnassa kaikille käyttäjille, jotta sovelluksen voit käyttää **järjestelmänvalvojan suostumus päätepisteen**, seuraavalla tavalla.

## <a name="admin-restricted-scopes"></a>Järjestelmänvalvojan rajoitettu käyttöalueet

Suuri alaisuudessa tietyt käyttöoikeudet Microsoft ekosysteemissä voidaan merkitä **järjestelmänvalvojan rajoitettu**.  Esimerkkejä näiden alueet:

- Luetaan organizaion directory tietoja:`Directory.Read`
- Tietojen kirjoittamista organisaation hakemistoon:`Directory.ReadWrite`
- Organisaation hakemiston luku käyttöoikeusryhmät:`Groups.Read.All`

Kun kuluttaja käyttäjä voi myöntää näiden tietojen sovelluksen käyttö, organisaation käyttäjät on rajoitettu samat yrityksen luottamuksellisten tietojen käyttöoikeuksien myöntäminen.  Jos sovellus pyytää access johonkin näiden oikeuksien organisaation käyttäjän, käyttäjä saa virhe, joka ilmaisee, että ne eivät ole suostuu oman sovelluksen käyttöoikeuksia.

Jos sovellus edellyttää nämä organisaatiot järjestelmänvalvojan rajoitettu vaikutusalueet pääsy, pyydä ne suoraan toisen **järjestelmänvalvojan suostumus päätepisteen**, jäljempänä yrityksen järjestelmänvalvoja.

Kun järjestelmänvalvoja antaa nämä oikeudet järjestelmänvalvojan suostumus päätepisteen kautta, suostumus myönnetään kaikille käyttäjille, vuokraajan, yllä olevien ohjeiden mukaisesti.

## <a name="using-the-admin-consent-endpoint"></a>Järjestelmänvalvojan suostumus päätepisteen avulla

Näiden ohjeiden sovelluksen voivat kerätä annetun vuokraajan, mukaan lukien järjestelmänvalvojan rajoitettu käyttöalueen kaikkien käyttäjien käyttöoikeuksia.  Koodi-malli, joka noudattaa alla olevia ohjeita desribed näkyviin viitata [järjestelmänvalvojan rajoitettu käyttöalueen malli](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Pyydä käyttöoikeuksia sovelluksen rekisteröinti-portaalissa

- Siirry sovelluksen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)tai [-sovelluksen luominen](active-directory-v2-app-registration.md) , jos omistajuus on vielä vahvistamatta.
- **Microsoft Graph-käyttöoikeudet** -osaan ja lisää käyttöoikeuksia, joka edellyttää sovelluksen.
- Varmista, että voit **tallentaa** sovelluksen rekisteröinti

#### <a name="recommended-sign-the-user-into-your-app"></a>(Suositus): käyttäjän kirjautua sovelluksen

Yleensä päätepisteen luominen järjestelmänvalvojan käyttävä sovellus suostumus, kun sovellus on pystyttävä muodostamaan sivun tai näkymän, joka sallii Hyväksy sovelluksen käyttöoikeuksien hallinta.  Tällä sivulla voi olla osa sovelluksen kirjautumisen työnkulku-sovelluksen asetuksissa tai oma "yhteyden" tiedonkulun.  Monissa tapauksissa se on järkevää, jos haluat näyttää tämän sovelluksen "yhteyden" Näytä vasta, kun käyttäjä on kirjautunut sisään työpaikan tai oppilaitoksen Microsoft-tili.

Käyttäjän kirjautuminen sovellukseen voit tunnistaa organziation, johon järjestelmänvalvojan kuuluu ennen heitä pyydetään Hyväksy tarvittavat käyttöoikeudet.  Kun ehdottomasti tarpeen sen avulla voit luoda nopeampaa kokemus organisaation käyttäjien.  Kirjaudu käyttäjän noudattamalla Microsoftin [v2.0 protokolla opetusohjelmat](active-directory-v2-protocols.md).

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
| vuokraajan | pakollinen | Kansion alihallintaa, johon haluat pyytää lupaa.  Voit toimittaa GUID-tunnus tai kutsumanimi-muodossa. |
| client_id | pakollinen | Rekisteröinti-portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määritetty sovelluksen sovellustunnus. |
| redirect_uri | pakollinen | Redirect_uri, mihin vastaus lähetetään, kun sovellus käsittelemään.  Se on oltava täsmälleen samat jokin portaalissa rekisteröity redirect_uris. |
| tila | suositellaan | Arvo, joka sisältyy pyynnön, joka palautetaan myös suojaustunnuksen vastauksessa.  Voi olla merkkijonoksi sisällöstä, jotka haluat.  Siinä käytetään koodata tietoa käyttäjän tilasta-sovelluksessa, ennen kuin todennus-pyyntö tapahtui, esimerkiksi sivun tai Näytä ne olisivat. |

Tässä vaiheessa Azure AD Pakota sitä, että vain vuokraajan järjestelmänvalvoja voivat kirjautua sisään pyynnön suorittamiseen.  Järjestelmänvalvojan pyydetään hyväksymään kaikki käyttöoikeudet, jotka olet pyytänyt, kun sovellus rekisteröinti-portaalissa. 

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

Kun saanut onnistuneen vastauksen järjestelmänvalvojan suostumus päätepisteestä-sovelluksen on saatu se pyytänyt käyttöoikeudet.  Voit nyt siirtää tarralle haluamasi resurssin tunnus pyytää seuraavalla tavalla.

## <a name="using-permissions"></a>Käyttöoikeuksien avulla

Sen jälkeen, kun käyttäjä suostuu sovelluksen käyttöoikeuksia, sovellus voi hankkia Accessin tunnuksia, jotka edustavat sinua sovelluksen oikeutta käyttää joidenkin kapasiteetin resurssin.  Tietyn käyttöoikeustietue voi käyttää vain yhteen resorce, mutta koodattu sisällä on jokaisen käyttöoikeuksia, sovellus, jolle on myönnetty kyseiselle resurssille.  Access-tunnuksen hankkimaan sovelluksen tehdä pyyntö v2.0 suojaustunnuksen päätepisteen:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Tuloksena käyttöoikeustietue voidaan sitten pyyntöjen resurssin – se luotettavasti kertovat resurssin sovelluksen on erisnimen käyttöoikeus tietyn tehtävän suorittamiseen.  

Saat lisätietoja OAuth 2.0-protokollan ja hankintaa access tunnusten Hae [v2.0 päätepisteen protokolla viittaus](active-directory-v2-protocols.md).
