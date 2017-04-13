<properties
    pageTitle="Todennus- ja Azure-sovelluksen palvelun | Microsoft Azure"
    description="Yleistä todennusta ja käsitteellisiä viittaus / luvan ominaisuus Azure App palvelun"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Todennus- ja Azure sovelluksen-palvelussa

## <a name="what-is-app-service-authentication--authorization"></a>Mikä on sovelluksen todentaminen / luvan?

Sovelluksen todentaminen / lupa on ominaisuus, joka mahdollistaa sovelluksen kirjautua käyttäjät niin, että sinun ei tarvitse muuttamisen app taustassa. Se on helppo tapa suojata sovelluksesi ja käyttäjäkohtaisten tietojen käsitteleminen.

Sovelluksen palvelu käyttää liitetyt käyttäjätiedot, jossa kolmannen osapuolen palvelun tallentaa tilit ja todentaa käyttäjät. Sovellus on riippuvainen tarjoajan tunnistetietoja niin, että sovellus ei ole itse tietojen tallentamiseen. Sovelluksen palvelu tukee viisi tunnistetietojen toimittajat ruutuun ulos: Azure Active Directory, Facebook, Google, Microsoft-Account ja Twitter. Sovelluksen avulla jokin muu luku tunnistetietojen näistä palveluista käyttävien käyttäjien siitä, miten ne sisäänkirjautuminen-vaihtoehtojen avulla. Laajenna sisäinen tuki voit integroida toiseen tunnistetietojen toimittaja tai [mukautetun identity-ratkaisun][custom-auth].

Jos haluat aloittaa saman tien-kohdassa jokin seuraavista opetusohjelmat:

- [Käyttöoikeuksien lisääminen iOS-sovellukseen] [ iOS] (tai [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]tai [Cordova])
- [Azure-sovelluksen Service API-sovellusten käyttöoikeuksien][apia-user]
- [Azure-sovelluksen palvelu – osa 2 käytön aloittaminen][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Käyttöoikeuksien toiminta sovelluksen-palvelussa

Todennetaan käyttämällä jotakin tunnistetietojen toimittajat, jotta sinun on tunnistetietojen toimittaja Lisätietoja sovelluksen määrittämiseen. Tunnistetietojen toimittaja annettava tunnukset ja tietoja, jotka sisältävät sovelluksen-palveluun. Tämä suorittaa luottamussuhde niin, että palvelun sovelluksen, voit tarkistaa käyttäjän vahvistukset, kuten käyttöoikeuksien tunnusta-tunnistetietojen toimittaja.

Kirjaudu käyttäjän käyttämällä jotakin näistä palveluista, käyttäjän on ohjataan päätepiste, joka kirjautuu palvelun käyttäjille. Jos asiakkaat käyttävät web-selaimessa, voit määrittää sovelluksen palvelun suora automaattisesti kaikki Todentamattomille käyttäjille, jotka käyttäjät kirjautuu päätepisteelle. Muussa tapauksessa sinun on ohjaamaan asiakkaita `{your App Service base URL}/.auth/login/<provider>`, jossa `<provider>` on jokin seuraavista arvoista: aad, facebook, google, Microsoftin tai twitter. Mobiili- ja API skenaariot on esitelty osien tämän artikkelin.

Käyttäjät, jotka käsitellä sovelluksesi selaimella on eväste niin, että he voivat pysyvät todennetut ne Selaa sovelluksen. Asiakkaan muuntyyppiset, kuten matkapuhelin-JSON web-tunnuksen (JWT), jossa esitetään `X-ZUMO-AUTH` ylätunnisteessa annetaan asiakkaalle. Mobile-sovellusten asiakkaan SDK: T Käsittele tätä puolestasi. Voit myös Azure Active Directory-tunnistetietojen tunnuksen tai käyttöoikeustietue suoraan mainita `Authorization` otsikko kuin [haltijatunnukseen](https://tools.ietf.org/html/rfc6750).

Sovelluksen palvelun vahvistaa kaikki evästeiden tai tunnuksen, sovelluksen ongelmien käyttäjien todentamiseen. Voit rajoittaa sovelluksen käyttöoikeuden, tämän artikkelin kohdassa [luvan](#authorization) .

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobiili todennus palveluntarjoajalta SDK

Kun kaikki on määritetty taustassa, voit muuttaa kirjautumaan sisään sovelluksen palvelun mobiilisovellusten. Käytettävissä on kaksi tapaa tähän:

- Käytä SDK-paketissa, joka tunnistaa ja sitten käyttää sovelluksen palveluun julkaisee annetun tunnistetietojen toimittaja.
- Käytä koodi Yksi tekstirivi, jotta käyttäjät kirjautumaan mobiilisovellukset asiakkaan SDK.

>[AZURE.TIP] Useimpien sovellusten käytettävä palveluntarjoaja SDK saat entistä yhdenmukaisemmat kokemus, kun käyttäjät kirjautua sisään, käyttää päivityksen tukea, sekä muita etuja, joka määrittää palvelu.

Kun palveluntarjoaja SDK, käyttäjät voivat kirjautua sisään ratkaisun, joka tiiviimmin integroitu käyttöjärjestelmille, joita sovellus on käynnissä. Näin voit myös tarjoajan tunnuksen ja asiakkaan, joka on helpompi käyttäjäkokemuksen mukauttamiseen ja kuluttavat graph ohjelmointirajapinnan joitakin käyttäjätietoja. Joskus blogit ja keskustelupalstoilla, näet tätä kutsutaan "asiakkaan kulun" tai "asiakas ohjataan työnkulku" koska asiakas-koodin kirjautuu käyttäjät ja asiakas-koodi on pääsy tarjoaja-tunnuksen.

Toimittaja-tunnuksen saadaan, kun tarvitaan vahvistus App palveluun lähetetään. Kun sovellus palvelun tarkistaa tunnuksen, sovelluksen palvelu luo uusi sovelluksen Service-tunnuksen, joka palautetaan asiakkaalle. Mobile-sovellusten asiakkaan SDK on helper tapoja hallita vaihdon ja liittää automaattisesti tunnuksen sovelluksen Taustajärjestelmä kaikki pyynnöt. Kehittäjät myös pitää tarjoajan tunnuksen viittaus halutessaan.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Mobiili todennus ilman palveluntarjoaja SDK-paketissa

Jos et halua määrittää palveluntarjoaja SDK, voit sallia Azure sovelluksen-palveluun, kirjaudu mobiilisovellukset-ominaisuus. Mobile-sovellusten asiakkaan SDK Avaa web-näkymän sähköpostikansioon tarjoajaan ja kirjaudu sisään käyttäjä. Joskus blogit ja keskustelupalstoilla, näet tätä kutsutaan "palvelimen vuota" tai "server ohjataan työnkulku" koska palvelimen hallitsee prosessi, jossa käyttäjä kirjautuu ja asiakkaan SDK saa ilmoituksen tarjoaja-tunnuksen.

Aloita tämä työnkulku koodi sisältyy eri ympäristöissä todennus-opetusohjelman. Virtaus lopussa asiakkaan SDK on sovelluksen Service-tunnuksen ja tunnuksen liitetään automaattisesti kaikki pyynnöt sovelluksen Taustajärjestelmä.

### <a name="service-to-service-authentication"></a>Palvelun todennus

Vaikka voit antaa käyttäjille sovelluksen, voit luottaa oman API Soita toiseen sovellukseen. Esimerkiksi voi olla yksi online Ohjelmointirajapinnan Soita toiseen web Appissa. Tässä skenaariossa sitä avulla tunnistetiedot palvelutilin käyttäjän tunnistetietojen sijaan tunnuksen. Palvelutili käytetään myös nimitystä Azure Active Directory-projectorin *palvelun pääasiallista* ja tarkistusta, joka käyttää tällaisen tilin käytetään myös nimitystä palvelun skenaario.

>[AZURE.IMPORTANT] Koska mobiilisovellukset suorittaa asiakkaan laitteissa, mobile sovellukset toimia _ei_ kuin Luotetut sovellukset ja Älä käytä palvelun pääasiallista työnkulun laskeminen. Sen sijaan ne käytettävä käyttäjän työnkulun, joka on tarkat aiemmin.

Palvelun tilanteessa sovelluksen palvelun voit suojata sovelluksen Azure Active Directoryn avulla. Puheluja sovellus on vain antamaan Azure Active Directory-palvelun pääasiallista luvan tunnuksen, joka saadaan antamalla asiakkaan tunnuksen ja asiakkaan salainen Azure Active Directorysta. Esimerkki tämä tilanne, jossa hyödynnetään ASP.NET-Ohjelmointirajapinnan sovellukset on selittää opetusohjelmassa [Service API-sovellusten pääasiallista tunnistus][apia-service].

Jos haluat käsitellä palvelun skenaario App Service-todennuksen avulla, voit käyttää asiakkaan varmenteet tai perustodentamista. Lisätietoja Azure asiakkaan varmenteet on artikkelissa [Siitä, miten voit määrittää TLS molemminpuoleista verkkosovelluksissa](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Lisätietoja perustodentamista ASP.NET-kohdassa [ASP.NET-verkko-Ohjelmointirajapinnan 2 todennus suodattimet](http://www.asp.net/web-api/overview/security/authentication-filters).

Palvelun tilin todennus App palvelun logiikan sovelluksesta API-sovellukseen on luettava tapaus, joka on eritelty [mukautetun API isännöimät logiikan sovellukset App-palvelun avulla](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="authorization"></a>Sovelluksen palvelussa luvan toiminta

Sinulla on täydet oikeudet, voit käyttää sovelluksen pyynnöt. Sovelluksen todentaminen / luvan voidaan määrittää jokin seuraavista ongelmista:

- Salli vain todennetut pyynnöt sovelluksen saavuttamiseksi.

    Jos selaimessa saa anonyymi pyynnön, sovelluksen palvelun ohjaa sivulle, joka on valittava, jotta käyttäjät voivat kirjautua sisään tunnistetietojen toimittaja. Jos pyynnön peräisin mobiililaitteella, HTTP _401 ei oikeuksia_ vastaus ei palautettu vastaus.

    Tällä asetuksella ei tarvitse kirjoittaa todennus koodia lainkaan sovellukseen. Jos tarvitset tarkempaan lupa, käyttäjän tietoja on saatavilla koodiin.

- Salli kaikki palvelupyynnöt saavuttaa sovelluksen, mutta Vahvista todennetut pyynnöt ja välittää todennustiedot HTTP-otsikoiden pitkin.

    Tämä vaihtoehto lykkää luvan päätöksiä sovelluksen-koodiin. Se on joustavampi käsittelyn anonyymit pyynnöt, mutta sinun tarvitse kirjoittaa koodia.

- Salli kaikki sovelluksesi saavuttamiseksi pyynnöt ja ei käsittele pyyntöjä todennustiedot.

    Tässä tapauksessa todennus / todennus-toiminto on poistettu käytöstä. Todennus- ja tehtävät ovat täysin sovelluksen koodin ylöspäin.

Edellisen toiminnan ohjaavat Azure-portaalissa **toiminto, joka suoritetaan, kun pyyntö ei ole todennettu** -vaihtoehto. Jos valitset * *lokitiedoston *nimi* **, kaikki pyynnöt on todennus.** Salli pyyntö (mitään toimia) ** lykkää luvan päätös koodi, mutta se tarjoaa edelleen todennustiedot. Jos haluat käsitellä kaikki koodi on, voit poistaa todennus / luvan ominaisuus.

## <a name="working-with-user-identities-in-your-application"></a>Käyttäjätietojen sovelluksen käyttäminen

Sovelluksen palvelun välittää joitakin käyttäjätietoja sovelluksen määräten otsikoiden avulla. Ulkoinen pyynnöt kieltää nämä otsikot ja vain esitä Jos määritetään mukaan App todentaminen / luvan. Esimerkki joidenkin otsikoiden ovat seuraavat:

* X-MS-ASIAKAS-LYHENNYS-NIMI
* X-MS-ASIAKAS-LYHENNYS-TUNNUS
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Koodi, joka on kirjoitettu kielellä tai framework saa tiedot, jotka tarvitaan nämä otsikot. ASP.NET-4.6 sovellusten **ClaimsPrincipal** määritetään automaattisesti haluamasi arvot.

Sovelluksen myös hankkia lisää käyttäjätiedot – HTTP GET `/.auth/me` sovelluksesi päätepiste. Kelvollinen tunnus, joka sisältää pyynnön palauttaa JSON-paketti, palvelu, jota käytetään tietojen, pohjana tarjoajan tunnuksen ja joitakin muita käyttäjätietoja. Mobile-sovellusten palvelimen SDK: T on helper tapaa työskennellä näiden tietojen kanssa. Lisätietoja on artikkelissa [Azure Mobile sovellusten Node.js SDK käyttämisestä](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)ja [.NET palvelimeen SDK Azure Mobile-sovellusten käyttäminen](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Asiakirjat ja muut resurssit

### <a name="identity-providers"></a>Tunnistetietojen toimittajat
Opetusohjelmassa Näytä sovelluksen palvelun eri käyttöoikeustarkistuspalvelun asetusten määrittämisestä:

- [Voit määrittää sovelluksen käyttämään Azure Active Directory-kirjautuminen][AAD]
- [Voit määrittää sovelluksen käyttämään Facebook-kirjautuminen][Facebook]
- [Kun sovellus käyttää Google-kirjautuminen määrittäminen][Google]
- [Voit määrittää sovelluksen käyttämään Microsoft Account käyttäjätunnus][MSA]
- [Voit määrittää sovelluksen käyttämään Twitter-kirjautuminen][Twitter]

Jos haluat käyttää identity-järjestelmän annettu niistä kuin tässä, voit käyttää myös [Esikatselu Mobile-sovellusten .NET-palvelimen SDK mukautettu tarkistus-tuki][custom-auth], jota voidaan käyttää web Apps-sovelluksista, mobiilisovellukset tai API-sovellukset.

### <a name="web-applications"></a>Verkkosovellusten
Opetusohjelmassa Näytä käyttöoikeuksien lisääminen web-sovelluksen:

- [Azure-sovelluksen palvelu – osa 2 käytön aloittaminen][web-getstarted]

### <a name="mobile-applications"></a>Mobile-sovellukset
Opetusohjelmassa Näytä käyttöoikeuksien lisääminen mobile asiakkaiden palvelimen ohjataan-työnkulku käyttämällä:

- [Käyttöoikeuksien lisääminen iOS-sovellukseen][iOS]
- [Lisää Android-sovelluksen-todennus] [Android]
- [Lisää sovelluksen Windows-todennus] [Windows]
- [Lisää todennus Xamarin.iOS-sovellukseen] [Xamarin.iOS]
- [Lisää todennus Xamarin.Android-sovellukseen] [Xamarin.Android]
- [Lisää todennus Xamarin.Forms-sovellukseen] [Xamarin.Forms]
- [Lisää todennus Cordova-sovellukseen] [Cordova]

Jos haluat käyttää asiakkaan ohjataan kulun Azure Active Directory, käytä on seuraavissa resursseissa:

- [IOS Active Directory käyttöoikeuksien kirjaston käyttäminen][ADAL-iOS]
- [Käytä Active Directory käyttöoikeuksien kirjaston androidille][ADAL-Android]
- [Active Directory käyttöoikeuksien kirjaston käyttäminen Windows ja Xamarin][ADAL-dotnet]

Jos haluat käyttää asiakkaan ohjataan kulun Facebook, käytä on seuraavissa resursseissa:

- [Facebook-SDK käyttäminen iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Jos haluat käyttää asiakkaan ohjataan kulun Twitteriin, käytä on seuraavissa resursseissa:

- [Twitter-kangasta käyttäminen iOS](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Jos haluat käyttää asiakkaan ohjataan kulun Google, käytä on seuraavissa resursseissa:

- [IOS-kirjautuminen SDK Google käyttäminen](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API-sovellukset
Opetusohjelmassa Näytä suojaaminen API-sovellukset:

- [Azure-sovelluksen Service API-sovellusten käyttöoikeuksien][apia-user]
- [Lainan todentaminen Azure App Service API varten][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android-laitteeseen]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
