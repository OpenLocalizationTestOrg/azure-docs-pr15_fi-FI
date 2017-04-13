<properties
    pageTitle="Todennus- ja Azure Mobile-sovelluksissa | Microsoft Azure"
    description="Yleistä todennusta ja käsitteellisiä viittaus / Azure Mobile-sovellusten ominaisuuksien todennus"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Todennus- ja Azure Mobile-sovelluksissa

## <a name="what-is-app-service-authentication--authorization"></a>Mikä on sovelluksen todentaminen / luvan?

> [AZURE.NOTE] Tässä ohjeaiheessa siirretään, kootut [sovelluksen todentaminen / luvan](../app-service/app-service-authentication-overview.md) ohjeaiheen, joka kattaa Web, Mobile ja API sovellusten.

Sovelluksen todentaminen / todennus on toiminto, joka sallii sovelluksen kirjautumalla käyttäjien pakollinen app taustassa koodin ilman muutoksia. Se on helppo tapa suojata sovelluksesi ja käyttäjäkohtaisten tietojen käsitteleminen.

Sovellus käyttää liitetyt käyttäjätiedot, jossa 3rd osapuolen **tunnistetietojen toimittaja** ("IDP") tallennetaan tilit ja todentaa käyttäjät, ja sovellus käyttää tätä tunnistetietojen sijaan oma. Sovelluksen palvelu tukee viisi tunnistetietojen toimittajat ruutuun ulos: _Azure Active Directory_, _Facebook_, _Google_, _Microsoft-tili_ja _Twitter_. Voit myös laajentaa tuki sovelluksia varten integroimalla toiseen tunnistetietojen toimittaja tai mukautetun identity-ratkaisuun.

Sovelluksen voidaan hyödyntää jokin muu luku tunnistetietojen näistä palveluista, jotta voit antaa käyttäjiesi siitä, miten ne kirjautuminen-vaihtoehtojen avulla.

Jos haluat aloittaa saman tien, lue jompikumpi seuraavista opetusohjelmat:

- [Käyttöoikeuksien lisääminen iOS-sovellukseen]
- [Käyttöoikeuksien lisääminen Xamarin.iOS-sovellukseen]
- [Käyttöoikeuksien lisääminen Xamarin.Android-sovellukseen]
- [Käyttöoikeuksien lisääminen Windows-sovellus]

## <a name="how-authentication-works"></a>Käyttöoikeuksien toiminta

Todentaa jollakin tunnistetietojen toimittajat, jotta sinun on tunnistetietojen toimittaja Lisätietoja sovelluksen määrittämiseen. Tunnistetietojen toimittaja sitten antaa sinulle tunnukset ja tietoja, jotka tarjoavat takaisin sovellus. Tämä on valmis luottamussuhde ja vahvista käyttäjätietojen annettu siihen palvelun sovelluksen avulla.

Nämä vaiheet on kuvattu seuraavissa artikkeleissa:

- [Voit määrittää sovelluksen käyttämään Azure Active Directory-kirjautuminen]
- [Voit määrittää sovelluksen käyttämään Facebook-kirjautuminen]
- [Kun sovellus käyttää Google-kirjautuminen määrittäminen]
- [Voit määrittää sovelluksen käyttämään Microsoft Account käyttäjätunnus]
- [Voit määrittää sovelluksen käyttämään Twitter-kirjautuminen]

Kun kaikki on määritetty taustassa, voit muokata asiakkaan Kirjaudu sisään. Käytettävissä on kaksi tapaa tähän:

- Asiakkaan SDK kirjautuminen käyttäjien Mobile-sovellusten käyttäminen yksirivinen koodin, avulla.
- Hyödyntää julkaistu annettuja käyttäjätietoja tarjoajan tunnistaa ja sitten käyttää sovelluksen palveluun SDK-paketissa.

>[AZURE.TIP] Useimpien sovellusten SDK-palveluntarjoajan pitäisi käyttäminen, saat lisää native feeling kirjautuminen-versio ja Päivitä tuki-ja muita palvelukohtaisia etuja.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Todennus ilman palveluntarjoaja SDK toiminta

Jos et halua määrittää palveluntarjoaja SDK, voit sallia Mobile-sovellusten suorittamiseen kirjautuminen. Mobile-sovellusten asiakkaan SDK avautuvat web-näkymän sähköpostikansioon tarjoajaan ja kirjautunut sisään. Joskus käytössä blogit ja näet tämän tarkoitetun "palvelimen työnkulku" tai "server ohjataan työnkulku" palvelimen keskustelupalstoilla hallinnoidaan kirjautuminen ja asiakkaan SDK saa ilmoituksen toimittaja-tunnuksen.

Aloita tämä työnkulku tarvitaan koodin käsitellään eri ympäristöissä todennus-opetusohjelmassa. Virtaus lopussa asiakkaan SDK on sovelluksen Service-tunnuksen ja tunnuksen liitetään automaattisesti kaikki pyynnöt taustaan.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Todennus palveluntarjoajalta SDK toiminta

Palveluntarjoaja käsitteleminen SDK avulla sisään myös käsitellä tiiviimmin ympäristö OS sovellus on käynnissä. Näin voit myös tarjoajan tunnuksen ja asiakkaan, joka on helpompi käyttäjäkokemuksen mukauttamiseen ja tarjoaman graph ohjelmointirajapinnan joitakin käyttäjätietoja. Joskus blogit ja keskustelupalstoilla näet tätä kutsutaan "asiakkaan kulun" tai "asiakas ohjataan työnkulku" jälkeen koodi asiakkaan käsittelee kirjautuminen ja asiakas-koodi on pääsy toimittaja-tunnuksen.

Toimittaja-tunnuksen saadaan, kun tarvitaan vahvistus App palveluun lähetetään. Virtaus lopussa asiakkaan SDK on sovelluksen Service-tunnuksen ja tunnuksen liitetään automaattisesti kaikki pyynnöt taustaan. Kehittäjän myös pitää tarjoajan tunnuksen viittaus halutessaan.

## <a name="how-authorization-works"></a>Luvan toiminta

Sovelluksen todentaminen / luvan paljastaa toiminnon, **joka suoritetaan, kun pyyntö ei ole todennettu**useilla eri tavoilla. Ennen kuin koodisi saa annetun pyynnön, sinun on sovelluksen palvelun Tarkista pyynnön todennetaan ja, jos ei hylätä sen ja yrität on käyttäjän, kirjaudu sisään, ennen kuin yrität uudelleen.

Yksi vaihtoehto on on todentamaton pyyntöjen uudelleenohjaus johonkin tunnistetietojen toimittajat. Web-selaimessa tämä noudatetaan todella käyttäjän uudella sivulla. Liikkuva asiakas ei voi uudelleenohjata tällä tavalla, ja Todentamattomille vastaukset lähetetään HTTP _401 ei oikeuksia_ vastaus. Ottaen huomioon, asiakkaalle on ensimmäinen pyyntö on aina oltava kirjautuminen päätepisteen ja tee muita ohjelmointirajapinnan puhelut. Jos yrität kutsu toinen API ennen kirjautumisesta, asiakkaan tulee virhe.

Jos haluat on eritellympiä ohjaamiseen kautta mitä päätepisteitä edellyttää todennusta, voit myös valita "tarvittavat toimenpiteet (Salli pyynnön)" Todentamattomille pyyntöihin. Tässä tapauksessa kaikki todennus päätökset on lykätty sovelluksen-koodin. Voit myös käyttöoikeuden myöntäminen tietyille käyttäjille mukautettuja käyttöoikeuksien myöntämissääntöjä perusteella.

## <a name="documentation"></a>Ohjeet

Opetusohjelmassa Näytä käyttöoikeuksien lisääminen mobile asiakkaiden sovelluksen-palvelun avulla:

- [Käyttöoikeuksien lisääminen iOS-sovellukseen]
- [Käyttöoikeuksien lisääminen Xamarin.iOS-sovellukseen]
- [Käyttöoikeuksien lisääminen Xamarin.Android-sovellukseen]
- [Käyttöoikeuksien lisääminen Windows-sovellus]

Opetusohjelmassa näyttää, miten voit hyödyntää erilaisia käyttöoikeustarkistuspalvelun sovelluksen-palvelun määrittäminen:

- [Voit määrittää sovelluksen käyttämään Azure Active Directory-kirjautuminen]
- [Voit määrittää sovelluksen käyttämään Facebook-kirjautuminen]
- [Kun sovellus käyttää Google-kirjautuminen määrittäminen]
- [Voit määrittää sovelluksen käyttämään Microsoft Account käyttäjätunnus]
- [Voit määrittää sovelluksen käyttämään Twitter-kirjautuminen]

Jos haluat käyttää identity-järjestelmän annettu niistä kuin tässä hyödyntää [esikatsella .NET-palvelimen SDK mukautettu tarkistus-tuki](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Käyttöoikeuksien lisääminen iOS-sovellukseen]: app-service-mobile-ios-get-started-users.md
[Käyttöoikeuksien lisääminen Xamarin.iOS-sovellukseen]: app-service-mobile-xamarin-ios-get-started-users.md
[Käyttöoikeuksien lisääminen Xamarin.Android-sovellukseen]: app-service-mobile-xamarin-android-get-started-users.md
[Käyttöoikeuksien lisääminen Windows-sovellus]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Voit määrittää sovelluksen käyttämään Azure Active Directory-kirjautuminen]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Voit määrittää sovelluksen käyttämään Facebook-kirjautuminen]: app-service-mobile-how-to-configure-facebook-authentication.md
[Kun sovellus käyttää Google-kirjautuminen määrittäminen]: app-service-mobile-how-to-configure-google-authentication.md
[Voit määrittää sovelluksen käyttämään Microsoft Account käyttäjätunnus]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Voit määrittää sovelluksen käyttämään Twitter-kirjautuminen]: app-service-mobile-how-to-configure-twitter-authentication.md
