<properties 
    pageTitle="Usean DRM ja käyttöoikeuksien valvonta CENC: viittaus suunnittelu ja käyttöönotto Azure ja Azure Media Services | Microsoft Azure" 
    description="Lisätietoja on artikkelissa käyttöoikeuksien Microsoft® tasainen Streaming asiakkaan sovittaminen Kit." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan"  
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="willzhan;kilroyh;yanmf;juliako"/>

#<a name="cenc-with-multi-drm-and-access-control-a-reference-design-and-implementation-on-azure-and-azure-media-services"></a>Usean DRM ja käyttöoikeuksien valvonta CENC: viittaus suunnittelu ja käyttöönotto Azure ja Azure Media-palveluita

##<a name="key-words"></a>Avainsanoja
 
Azure Active Directory-Azure Media Services Azure Media Playerin dynaaminen salaus käyttöoikeuden toimituksen PlayReady Widevine, FairPlay, yleisiä Encryption(CENC), usean DRM, Axinom, VIIVAN, EME, MSE, JSON Web suojaustunnuksen (JWT), saatavat, Nykyaikainen selaimissa, avaimen palauttaminen, symmetrisen avaimen, julkiseen avain, OpenID yhteyden, X509 varmenne. 

##<a name="in-this-article"></a>Tässä artikkelissa

Tässä artikkelissa käsitellään on seuraavissa artikkeleissa:

- [Johdanto](media-services-cenc-with-multidrm-access-control.md#introduction)
    - [Tässä artikkelissa yleiskatsaus](media-services-cenc-with-multidrm-access-control.md#overview-of-this-article)
- [Viite-rakenne](media-services-cenc-with-multidrm-access-control.md#a-reference-design)
- [Käyttöönotto-tekniikan rakenteen määritys](media-services-cenc-with-multidrm-access-control.md#mapping-design-to-technology-for-implementation)
- [Käyttöönotto](media-services-cenc-with-multidrm-access-control.md#implementation)
    - [Käyttöönoton ohjeita](media-services-cenc-with-multidrm-access-control.md#implementation-procedures)
    - [Toteutuksen jälkeisen joitakin kompastuskiviä](media-services-cenc-with-multidrm-access-control.md#some-gotchas-in-implementation)
- [Toteutuksen aiheisiin](media-services-cenc-with-multidrm-access-control.md#additional-topics-for-implementation)
    - [HTTP- tai HTTPS](media-services-cenc-with-multidrm-access-control.md#http-or-https)
    - [Azure Active Directory kirjautuminen avaimen palauttaminen](media-services-cenc-with-multidrm-access-control.md#azure-active-directory-signing-key-rollover)
    - [Missä on Access-tunnuksen?](media-services-cenc-with-multidrm-access-control.md#where-is-the-access-token)
    - [Live Entä Streaming?](media-services-cenc-with-multidrm-access-control.md#what-about-live-streaming)
    - [Mitä tietoja palvelimet Azure Media Services ulkopuolella?](media-services-cenc-with-multidrm-access-control.md#what-about-license-servers-outside-of-azure-media-services)
    - [Entä jos haluan käyttää mukautettuja STS?](media-services-cenc-with-multidrm-access-control.md#what-if-i-want-to-use-a-custom-sts)
- [Valmiit järjestelmän ja testi](media-services-cenc-with-multidrm-access-control.md#the-completed-system-and-test)
    - [Sisäänkirjautuminen](media-services-cenc-with-multidrm-access-control.md#user-login)
    - [Käyttämällä PlayReady salattuja Media-laajennukset](media-services-cenc-with-multidrm-access-control.md#using-encrypted-media-extensipons-for-playready)
    - [Käyttämällä EME Widevine](media-services-cenc-with-multidrm-access-control.md#using-eme-for-widevine)
    - [Ole oikeutettuja käyttäjät](media-services-cenc-with-multidrm-access-control.md#not-entitled-users)
    - [Mukautetun suojatun suojaustunnuksen palvelua](media-services-cenc-with-multidrm-access-control.md#running-custom-secure-token-service)
- [Yhteenveto](media-services-cenc-with-multidrm-access-control.md#summary)

##<a name="introduction"></a>Johdanto

Se on tunnettujen, että se on monimutkaista voit suunnitella ja luoda DRM-alirakenne OTT varten tai online-tietovirta ratkaisu. Ja jos se on yleisiä käytännöt operaattorit online videon toimittajia ulkoistaa tämän osan erityinen DRM palveluntarjoajia. Tämän asiakirjan lähinnä esittää viittaus-suunnitteluun ja toteuttamiseen lopusta loppuun DRM alirakenne OTT tai online streaming-ratkaisussa.

Tämän asiakirjan kohdennettujen lukijat ovat insinöörien työskentelyä DRM alirakenne OTT streaming/Avainhankkeiden monivaiheisen-screen ratkaisuja tai lukijat, jotka ovat kiinnostuneita DRM: n alijärjestelmää. Olettaen on, että lukijat perehtynyt vähintään yksi markkinoilla, kuten PlayReady, Widevine, FairPlay tai Adobe Access DRM-tekniikoiden kanssa.

DRM, jotka on myös sisällyttää CENC (yleisiä salaus) usean DRM. Online-tietovirta ja OTT alan pää trendi on käyttää CENC Avainhankkeiden monivaiheisen-native-DRM-eri asiakasympäristöjen, joka on VAIHTO edellisen trendin käyttämällä eri asiakasympäristöjen yhden DRM ja sen asiakkaan SDK. Käytettäessä CENC kanssa Avainhankkeiden monivaiheisen-native-DRM [Yleisiä salaus (ISO/IEC 23001 7 CENC)](http://www.iso.org/iso/home/store/catalogue_ics/catalogue_detail_ics.htm?csnumber=65271/) määrityksen kohden salattuja PlayReady ja Widevine.

Usean DRM kanssa CENC edut ovat seuraavat:

1. Pienentää kustannus koska yksi salaus käsittely käytetään kohdistamisen sen alkuperäisen DRMs; kanssa eri alustojen salaus
1. Pienentää hallinta salattuja varat, koska vain yhden kopion salattuja kalusto tarvita;
1. Jättää pois DRM asiakkaan käyttöoikeudet kustannukset, koska alkuperäisen DRM asiakas on yleensä ilmainen sen alkuperäisen ympäristössä.

Microsoft on joitakin tärkeimpiä pelaajat ja VIIVA ja CENC aktiivinen ekspressoitumattoman. Microsoft Azure Media Services on ollut tarjoaa VIIVA ja CENC tuki. Viimeisimmät ilmoitukset-kohdassa Mingfei's blogit: [julkaisu Google Widevine käyttöoikeuden toimituksen services Azure Media Services-palveluissa](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)ja [Azure Media-palveluiden Lisää Google Widevine pakkaus välittää usean DRM-muodossa](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).  

### <a name="overview-of-this-article"></a>Tässä artikkelissa yleiskatsaus

Tässä artikkelissa tavoitteen sisältää seuraavat:

1. Sisältää DRM alirakenne CENC käyttäminen usean-DRM; viittaus ulkoasua
1. Sisältää Microsoft Azure/Azure Media Services-ympäristössä; viittaus käyttöönotto
1. Tässä artikkelissa käsitellään joitakin suunnittelu ja käyttöönotto-ohjeita.

On artikkelissa "DRM usean" käsitellään seuraavia asioita:

1. Microsoft PlayReady
1. Google-Widevine
1. Apple FairPlay (ei vielä tueta Azure Media Services)

Seuraavassa taulukossa on yhteenveto alkuperäisen ympäristö/native-sovellus ja kunkin DRM tukemat selaimet.

**Asiakkaan ympäristö**|**Alkuperäisen DRM tuki**|**Selain/App**|**Streaming muodot**
----|------|----|----
**Smart televisioihin-operaattoria erilaisia, OTT erilaisia**|PlayReady pääasiassa ja/tai Widevine ja/tai muu|Linux, Opera, WebKit, muut|Eri muodoissa
**Windows 10-laitteissa (Windows-Käyttöjärjestelmää, Windows-tablettien, Windows Phone-, Xbox)**|PlayReady|MS reunan/IE11/EME<br/><br/><br/>UWP|YHDYSMERKIN (For HLS, PlayReady ei voi käyttää)<br/><br/>YHDYSMERKIN, tasainen Streaming (For HLS, PlayReady ei voi käyttää) 
**Android-laitteisiin (puhelimella tai taulutietokoneella, TV)**|Widevine|Chrome/EME|VIIVA
**iOS (iPhone, iPad), OS X-asiakkaat ja Apple TV**|FairPlay|Safari 8 +/ EME|HLS
**Laajennus: Adobe Primetime**|Primetime käyttö|Selainlaajennuksesta|HDS, HLS

Ottavat kunkin DRM käyttöönoton nykyisen tilan palvelu kannattaa yleensä toteuttaa 2 tai 3 DRMs, varmista, että päätepisteet kaikenlaisten osoite parhaalla tavalla.

Tällä sopivat palvelun logiikan monimutkaisuutta ja monimutkaisuus asiakaspuolen tason käyttäjäkokemuksen eri asiakkaiden saavuttamiseksi.

Voit tehdä valintasi, ota huomioon seuraavat asiat:

- PlayReady grafiikkatiedostomuotoja toteutettu jokaisen Windows-laitteessa joissakin Android-laitteiden ja ohjelmiston SDK: T lähes minkä tahansa ympäristössä kautta
- Widevine grafiikkatiedostomuotoja toteutettu jokaisen Android-laitteessa, Chrome ja muut laitteet
- FairPlay on käytettävissä vain iOS- ja Mac OS-asiakkaat- tai iTunes.

Jotta tyypillinen usean-DRM on seuraava:

- Vaihtoehto 1: PlayReady ja Widevine
- Vaihtoehto 2: PlayReady, Widevine ja FairPlay


## <a name="a-reference-design"></a>Viite-rakenne

Tässä osassa on esittää agnostic tekniikoista toteuta se eli viittaus-rakenne.

DRM alirakenne saattaa olla seuraavat osat:

1. Hallintaa
1. DRM pakkaus
1. DRM käyttöoikeuden toimitusta
1. Oikeuden-valintaruutu
1. Todennus ja todennus
1. Player
1. Origin/CDN

Seuraavassa kaaviossa on kuvattu korkean tason vuorovaikutuksen DRM alirakenne osien välillä.

![DRM alirakenne CENC kanssa](./media/media-services-cenc-with-multidrm-access-control/media-services-generic-drm-subsystem-with-cenc.png)
 
Sisältää kolme basic "kerrosta" rakennetta:

1. Takaisin office kerrokseen (musta) joka eikä niitä julkaista ulkoisesti.
1. Sisältävä kaikki vastakkaisten julkisen; päätepisteet kerros "DMZ" (sininen)
1. Julkinen Internet (Vaaleansininen) sisältävä kerros CDN ja pelaajat kanssa liikenteen julkiseen Internetin.
 
On oltava sisällön hallintatyökalun ohjaukseen DRM suojaus, riippumatta se on staattinen tai dynaaminen salausta. Syötteiden DRM salauksen tulee sisältää:

1. MBR videosisältöä;
1. Sisällön näppäimen.
1. Käyttöoikeuden hankkiminen URL-osoitteet.

Toiston aikana korkean tason virtaus on:

1. Käyttäjä todennetaan;
1. Käyttäjälle luodaan luvan tunnus
1. Suojattu DRM sisältö (luettelo) ladataan soittimella.
1. Soittimen lähettää käyttöoikeuden hankkiminen pyyntö palvelimet sivustoa avaimen tunnuksen ja luvan tunnuksen.

Ennen siirtymistä seuraavan aiheen pari sanaa tietoja hallintaa rakenne.

**ContentKey resurssi**|**Skenaario**
------|---------------------------
1 – 1|Yksinkertaisin kirjainkokoa. Se sisältää tarkimmat ohjausobjektin. Mutta tuloksena yleensä on suurin käyttöoikeuden toimituksen kustannus. Vähintään yksi käyttöoikeus pyyntö on tarvittavat kunkin suojatun osalta.
1 – moneen|Voit käyttää samaa sisältöä avainta useita kohteita. Esimerkiksi, kuten Tyylilaji tai alijoukko tyylilajin (tai elokuvan geenien) loogisen ryhmän kaikkien kohteiden, voit käyttää yksittäistä sisällön avainta.
Monta 1|Useita sisällön näppäimiä on tarvittavat kunkin resurssi. <br/><br/>Jos haluat dynaamisen CENC suojaaminen usean-DRM, MPEG-VIIVA ja dynaaminen AES-128 salaus HLS, sinun on esimerkiksi kaksi erillistä sisällön näppäimet-jokaisella on oma ContentKeyType. (Dynaaminen CENC suojaus käytettävän sisällön avaimen ContentKeyType.CommonEncryption tulisi olla käytetty dynaaminen AES-128 salaus käytettävän sisällön avaimen varten, ContentKeyType.EnvelopeEncryption tulisi käyttää.)<br/><br/>Toinen esimerkki on KATKOVIIVAN sisältöä, yksi sisällön avaimen avulla voidaan suojata videovirtaa ja toista sisällön näppäintä suojaaminen äänivirtaa teoriassa CENC suojaus. 
Monta – moneen-|Yllä kuvauksista yhdistelmä: sisällön näppäimet joukkona käytetään kunkin saman resurssi "ryhmä" useita kohteita.


Toinen tärkeä tekijä huomioitavia on jatkuva ja tilapäisten käyttöoikeuksia.

Miksi seuraavat seikat huomioon ovat tärkeitä? 

Heillä on suora vaikutus käyttöoikeuden toimituksen kustannukset, jos käytät julkista cloud käyttöoikeuden toimittamista varten. Seuraavassa seuraavat kaksi eri rakenne-tapausta, esitä:



1. Kuukausitilaukseen: jatkuva käyttöoikeuden ja 1 moneen sisällön avain resurssi yhdistäminen. Esimerkiksi Käytämme kaikki lapset elokuvat salauksen yksittäistä sisällön avainta. Tässä tapauksessa: 

    Yhteensä # käyttöoikeuksien pyydetty kaikki lapset elokuvat/laitteen = 1

1. Kuukausitilaukseen: jatkuva käyttöoikeus ja 1 1 muuntomalli sisältö avain ja kohteiden välillä. Tässä tapauksessa:

    Yhteensä # käyttöoikeuksien pyydetty kaikki lapset elokuvat/laitteen [seurattujen #-elokuvia] = [# istuntojen] x

Voit helposti huomaamaan, kaksi eri tyylejä johtaa hyvin eri käyttöoikeuden pyynnön kuviot näin ollen käyttöoikeuden toimituksen kustannukset, jos käyttöoikeuden toimituksen palvelun tarjoaa julkisen pilveen, kuten Azure Media Services.

## <a name="mapping-design-to-technology-for-implementation"></a>Käyttöönotto-tekniikan rakenteen määritys

Seuraavaksi on Yhdistä technologies Microsoft Azure/Azure Media Services-ympäristössä, tutustu yleinen rakenne määrittämällä, mitä tekniikkaa käytettävä kunkin rakenneosan.

Seuraavassa taulukossa on yhdistäminen:

**Rakenneosan**|**Tekniikka**
------|-------
**Player**|[Azure Media Player-ohjelmassa](https://azure.microsoft.com/services/media-services/media-player/)
**Tunnistetietojen toimittaja (IDP)**|Azure Active Directory
**Suojaaminen tunnuksen Service (STS)**|Azure Active Directory
**DRM suojaus-työnkulku**|Azure Media Services-palveluiden dynaaminen suojaus
**DRM käyttöoikeuden toimitusta**|1. azure Media Services-palveluiden käyttöoikeus toimituksen (PlayReady, Widevine, FairPlay), tai <br/>2. käyttöoikeuspalvelimen Axinom <br/>3. mukautetun PlayReady käyttöoikeuspalvelimen
**Origin**|Azure Media Services-palveluiden Streaming päätepiste
**Hallintaa**|Ei tarvita viittaus käyttöönotto
**Sisällön hallinta**|C#-konsolisovelluksen

Toisin sanoen tunnistetietojen toimittaja (IDP) ja suojattu suojaustunnuksen Service (STS) oltava Azure AD. Soittimen Käytämme [Azure Media Playerin API](http://amp.azure.net/libs/amp/latest/docs/). Azure Media Services ja Azure Media Playerin tukevan usean DRM VIIVA ja CENC.

Seuraavassa kaaviossa on esitetty yleinen rakenne ja työnkulku yllä tekniikka yhdistäminen kanssa.

![Valitse AMS CENC](./media/media-services-cenc-with-multidrm-access-control/media-services-cenc-subsystem-on-AMS-platform.png)

Määrittää dynaamisen CENC salausta, jotta sisällön hallinta-työkalu käyttää seuraavia syötteiden:

1. Avaa sisältöä;
1. Sisällön avain Key luominen ja hallinta;
1. Käyttöoikeuden hankkiminen URL-osoitteet;
1. Tietojen yhdistäminen Azure AD luettelo.

Sisällön hallinta-työkalun tulos on:

1. Määrityksen, miten käyttöoikeuden toimituksen tarkistaa JWT tunnuksen ja DRM käyttöoikeuksien määritykset, joka sisältää ContentKeyAuthorizationPolicy
1. AssetDeliveryPolicy sisältävä määritykset streaming muoto, DRM suojaus ja käyttöoikeuksien hankkiminen URL-osoitteet.

Runtimen aikana kulun on alla:

1. Käyttäjien todentamiseen yhteydessä luodaan JWT tunnuksen;
1. Vaateita, jotka sisältyvät JWT tunnus on "ryhmät" varaa sisältävä ryhmä Objektitunnus "EntitledUserGroup". Tämä vaatimus käytetään kulkeva "oikeuden valintaruudun".
1. Toisto-ohjelman lataamisen asiakkaan luettelo, CENC suojatun sisällön ja "näkee" seuraavasti:
    1. tunnusta, 
    1. sisältö on suojattu, CENC
    1. Käyttöoikeuden hankkiminen URL-osoitteet.

1. Player tekee käyttöoikeuden hankkiminen pyyntö tueta selaimessa/DRM perusteella. Käyttöoikeuden hankkiminen-pyynnössä tunnusta ja JWT tunnuksen lähetetään myös. Käyttöoikeuden toimituksen palvelun tarkistaa JWT tunnuksen ja vaateita, jotka sisältyvät ennen varmenteiden tarvittavaa käyttöoikeutta.

##<a name="implementation"></a>Käyttöönotto

###<a name="implementation-procedures"></a>Käyttöönoton ohjeita

Käyttöönoton sisältää seuraavat toimet:

1. Valmistele testi vakuutussopimukseen: koodata/paketin usean bittitaajuus testi videon pirstoutuneet MP4 Azure Media Services-palveluissa. Tämä resurssi ei ole suojattu DRM. DRM suojaus tekemän myöhemmin dynaaminen suojaus.
1. Luo avaimen tunnuksen ja sisällön avaimen (Voit myös mistä avaimen lähde). Microsoftin tarkoitukseen avaimen hallintajärjestelmän ei tarvita, sillä on käsittely vain yhtenäisiä tunnuksen ja sisällön avain pari testi varoihin;
1. Määritä usean DRM käyttöoikeuden toimituksen services, Testaa annetaan AMS API avulla. Jos käytössäsi on mukautettu palvelimet yrityksesi tai yrityksen toimittajille sijaan Azure Media Services palveluiden käyttöoikeus, voit ohittaa tämän vaiheen ja määrittää käyttöoikeuden hankkiminen URL-osoitteet määrittämiseen käyttöoikeuden toimituksen vaiheessa. AMS API tarvitaan, voit määrittää joitakin yksityiskohtaiset määritykset, kuten luvan käytännön rajoitus, käyttöoikeuden vastauksen malleja eri DRM käyttöoikeuden palveluissa jne. Tällä hetkellä Azure portaalin ei ole vielä tarjoa tarvitaan Käyttöliittymän määritysten. Voit etsiä API tason tiedot ja malli-koodin Julia Kornich asiakirjassa: [käyttämällä PlayReady ja/tai Widevine dynaaminen yleisiä salausta](media-services-protect-with-drm.md). 
1. Määritä resurssi toimituksen käytäntö, Testaa annetaan AMS API avulla. Voit etsiä API tason tiedot ja malli-koodin Julia Kornich asiakirjassa: [käyttämällä PlayReady ja/tai Widevine dynaaminen yleisiä salausta](media-services-protect-with-drm.md).
1. Luo ja Määritä Azure Active Directory-vuokraajan Azure;
1. Muutama käyttäjätilit ja ryhmien luominen Azure Active Directory-vuokraajan: sinun on luotava vähintään "EntitledUser" ryhmitellä ja Lisää käyttäjä tähän ryhmään. Tämän ryhmän käyttäjät välittää hankkimista oikeuden valintaruudun ja tämän ryhmän käyttäjät läpäissyt tarkistusta todennus epäonnistuu, ja se voi hankkia mitään. "EntitledUser"-ryhmän jäsen on pakollinen "ryhmät" vaatimus JWT tunnuksen myöntämä Azure AD. Tämä vaatimus vaatimus määritettävä määrittäminen usean DRM käyttöoikeuden toimituksen services vaihe.
1. Luo ASP.NET MVC-sovellus, joka isännöinnin videon toisto-ohjelman. ASP.NET-sovelluksen suojataan käyttöoikeuksien vastaan Azure Active Directory-vuokraajan kanssa. Oikea vaateita, jotka sisällytetään käyttöoikeuksien jälkeen saadun access-tunnukset. OpenID yhteyden API suositellaan tämän vaiheen. Sinun täytyy asentaa seuraavat NuGet paketit:
    - Asenna paketti Microsoft.Azure.ActiveDirectory.GraphClient
    - Asenna paketti Microsoft.Owin.Security.OpenIdConnect
    - Asenna paketti Microsoft.Owin.Security.Cookies
    - Asenna paketti Microsoft.Owin.Host.SystemWeb
    - Asenna paketti Microsoft.IdentityModel.Clients.ActiveDirectory
1. Luo [Azure Media Player-Ohjelmointirajapinnan](http://amp.azure.net/libs/amp/latest/docs/)käyttäminen toisto-ohjelman. [Azure Media Playerin ProtectionInfo API](http://amp.azure.net/libs/amp/latest/docs/) avulla voit määrittää, mitkä DRM-tekniikkaa käyttämisestä eri DRM-ympäristössä.
1. Testi-matriisin:

**DRM**|**Selain**|**Oikeus käyttäjän tulos**|**Yhdistelmävisualisoinnin oikeutettu käyttäjän tulos**
---|---|-----|---------
**PlayReady**|MS reunan tai IE11 Windows 10: n|Onnistunut|Epäonnistuu
**Widevine**|Windows 10: n Chrome|Onnistunut|Epäonnistuu
**FairPlay** |TBD||

George Trifonov Azure Media Services ryhmän on kirjoitettu antamisen Azure Active Directory-ASP.NET MVC player-sovelluksen määrityksen yksityiskohtaisia ohjeita blogin: [integroida Azure Media Services OWIN MVC perusteella app Azure Active Directory-hakemistosta ja rajoittaa sisällön avaimen toimittamisen JWT saatavat perusteella](http://gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

George myös on kirjoitettu blogin [JWT suojaustunnuksen](http://gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)todennus Azure Media Services-palveluissa ja dynaaminen salausta. Ja näin hänen [Azure AD-integrointi Azure Media Services avaimen toimitus-malli](https://github.com/AzureMediaServicesSamples/Key-delivery-with-AAD-integration/).

Lisätietoja Azure Active Directory:

- Löydät developer-tiedot [Azure Active Directory-kehittäjän](../active-directory/active-directory-developers-guide.md)oppaassa.
- Järjestelmänvalvojan tiedot löytyvät [Hallinta Your Azure Active Directory](../active-directory/active-directory-administer.md).

### <a name="some-gotchas-in-implementation"></a>Toteutuksen jälkeisen joitakin kompastuskiviä

Käyttöönoton on joitakin "kompastuskiviä". Hopefully "kompastuskiviä" seuraavassa luettelossa auttaa sinua vianmääritys siltä varalta, että käytössä ilmenee ongelmia.

1. **Myöntäjä** URL-Osoitteen tulisi pääty **"/"**.  

    **Yleisön** tulisi olla Playerin Sovellustunnus asiakkaan ja pitäisi myös lisätä **"/"** loppuun myöntäjän URL-osoite.

        <add key="ida:audience" value="[Application Client ID GUID]" />
        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/" /> 

    [JWT koodauksen](http://jwt.calebb.net/)näkyy **aud** ja **iss** kuin alla-JWT tunnuksen:
    
    ![1st gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-1st-gotcha.png)


2. Käyttöoikeuksien lisääminen sovelluksen AAD (-sovelluksen määrittäminen-välilehti). Tämä on tarvittavat kunkin sovelluksen (paikalliset ja otettu käyttöön versiot).

    ![2. gotcha](./media/media-services-cenc-with-multidrm-access-control/media-services-perms-to-other-apps.png)


3. Käytä oikeaa myöntäjä dynaaminen CENC suojauksen määrittäminen:

        <add key="ida:issuer" value="https://sts.windows.net/[AAD Tenant ID]/"/>
    
    Seuraavat ei toimi:
    
        <add key="ida:issuer" value="https://willzhanad.onmicrosoft.com/" />
    
    GUID-tunnus on AAD vuokraajan ID-tunnuksellasi. GUID-tunnus löytyy päätepisteet ponnahdusikkuna Azure-portaalissa.

4. Myönnä Ryhmäjäsenyyden väittää oikeudet. Varmista, että AAD sovelluksen tiedostojen tiedostossa, on seuraavat

    "groupMembershipClaims": "Kaikki", (oletusarvo on null)

5. Asetus oikea TokenType rajoitus vaatimukset luotaessa.

        objTokenRestrictionTemplate.TokenType = TokenType.JWT;

    Koska tuen JWT (AAD) lisäksi SWT (ACS) lisääminen oletusarvo TokenType on TokenType.JWT. Jos käytät SWT/ACS, sinun on määritettävä TokenType.SWT.

## <a name="additional-topics-for-implementation"></a>Toteutuksen aiheisiin
Seuraavaksi on levyn uss joitakin muita aiheet sekä suunnittelu ja käyttöönotto.

###<a name="http-or-https"></a>HTTP- tai HTTPS?

ASP.NET-MVC player-sovellus on luotu tuettava seuraavasti:

1. Käyttäjän todennusta ja Azure AD, joka on oltava kohdassa HTTPS;
1. JWT suojaustunnuksen exchange-asiakasohjelman ja Azure AD, jossa on oltava HTTPS, valitse välillä
1. DRM hankkimista, joka tarvitse olla HTTPS-kohdassa, jos käyttöoikeuden toimituksen tarjoaa Azure Media Services-asiakas. PlayReady tuotepakettia, määrätä HTTPS käyttöoikeuden toimittamista varten. Jos PlayReady käyttöoikeuspalvelimen on Azure Media Services ulkopuolella, voi käyttää HTTP tai HTTPS.

Tämän vuoksi ASP.NET player-sovelluksen käyttää HTTPS parhaiden käytäntöjen mukaisesti. Tämä tarkoittaa Azure Media Player-ohjelmassa on kohdassa HTTPS-sivulla. Kuitenkin streaming varten on mieluummin HTTP, näin ollen annettava huomioon erilaiset sisältö ongelma.

1. Selaimen ei salli yhdistelmäsisältö. Mutta laajennukset, kuten Silverlightin ja OSMF laajennuksen sujuvat ja VIIVAN Salli. Yhdistelmäsisältö on havainnut tietoturvaongelman – tämä on mahdollisuus lisätä haittaohjelmien JS, jotka voivat aiheuttaa asiakkaan tiedot vastuullasi uhkien vuoksi.  Selaimet Estä tämän oletusarvon mukaan ja tähän mennessä ainoa keino ratkaista se on käytössä (origin) palvelinpuolen, jos haluat, että kaikki toimialueet (riippumatta https tai http). Tämä ei luultavasti ole hyvä joko.
1. Olemme Vältä yhdistelmäsisältö: sekä käyttää HTTP tai molemmat https-PROTOKOLLAN avulla. Toistoa yhdistelmäsisältö silverlightSS tech edellyttää tyhjentämällä erilaiset sisällön varoitus. flashSS tech käsittelee yhdistelmäsisältö ilman eri sisällön varoitusta.
1. Jos streaming päätepiste on luotu ennen elokuussa 2014, se ei tue HTTPS. Tässä tapauksessa kirjoita luominen ja käyttäminen uusi streaming päätepiste HTTPS.

Viite-käyttöönoton DRM suojatun sisällön sekä sovellus että streaming on kohdassa HTTTPS. Avaa sisällön toisto-ohjelman ei tarvitse todennuksen tai -käyttöoikeuksia, joten sinun on käytettävä HTTP tai HTTPS liberty.

### <a name="azure-active-directory-signing-key-rollover"></a>Azure Active Directory kirjautuminen avaimen palauttaminen

Tämä on tärkeää käyttöönoton otetaan huomioon. Jos ei pidä tämä käyttöympäristösi, valmis järjestelmän myöhemmin lakkaa toimimasta kokonaan enintään 6 viikon kuluessa.

Azure AD käyttää yleinen standardi muodostaa Salli itse ja Azure AD sovellusten välillä. Tarkemmin sanottuna Azure AD käyttää allekirjoitetun avainta, joka koostuu julkisista ja yksityisistä avaimen pari. Kun Azure AD Luo suojaustunnus, joka sisältää käyttäjän tietoja, tunnuksessa on allekirjoittanut Azure AD käyttämällä sen yksityinen avain, ennen kuin se lähetetään sovelluksen. Varmista, että tunnus on voimassa ja todella peräisin Azure AD-sovellus on vahvistettava tunnuksen allekirjoituksen Azure AD, jotka sisältyvät vuokraajan federation metatietojen asiakirjan näyttämiä julkisen avaimen avulla. Tämä julkinen avain – ja josta se hakee allekirjoitetun avaimen – on käytettävä kaikki alihallinnat Azure AD-sama.

Valitse Azure AD avaimen palauttaminen yksityiskohtaiset tiedot löytyvät asiakirjan: [Kirjautuminen avaimen palauttaminen Azure AD-tärkeitä tietoja](../active-directory/active-directory-signing-key-rollover.md).

[Julkisen ja yksityisen avaimen pari](https://login.windows.net/common/discovery/keys/)välillä 

- Yksityinen avain käytetään Azure Active Directory JWT tunnuksen; luomiseen
- Julkinen avain käytetään jokin sovellus, kuten DRM käyttöoikeuden toimituksen palvelujen AMS JWT; tunnuksen vahvistamiseksi
 
Suojauksen tarkoitukseen Azure Active Directory kiertää tämän todistuksen säännöllisesti (viikon välein 6). Jos suojauksen ilmeisesti avaimen palauttaminen voi ilmetä milloin tahansa. Tämän vuoksi AMS käyttöoikeuden toimitus-palvelut on päivitettävä julkista avainta Azure AD kiertyy avaimen pari, muuten suojaustunnuksen AMS-todennus epäonnistuu, ja annetaan käyttöoikeutta ei. 

Tämä saavutetaan määrittämällä TokenRestrictionTemplate.OpenIdConnectDiscoveryDocument DRM käyttöoikeuden toimituksen services määritettäessä.

JWT suojaustunnuksen työnkulku on alla:

1.  Azure AD antaa JWT tunnuksen ja todennetun käyttäjän; nykyisen yksityinen avain
2.  Kun pelaaja näkee CENC usean DRM suojattu sisältöä, se ensin Etsi myöntämä Azure AD JWT tunnuksen.
3.  Toisto-ohjelman lähettää käyttöoikeuden hankkiminen pyyntö, jonka JWT tunnuksen AMS; käyttöoikeus toimituksen palvelut
4.  AMS käyttöoikeuden toimitus-palveluita käyttämällä nykyinen kelvollinen julkinen avain Azure AD-JWT-tunnuksen, varmista ennen käyttöoikeuksien myöntämistä.

DRM käyttöoikeuden toimituksen services aina tarkistetaan Azure AD-nykyinen kelvollinen julkinen avain. Julkinen avain Puhujan pitämä Azure AD on käytetty myöntämä Azure AD JWT-tunnuksen vahvistamisesta avain.

Entä jos avaimen palauttaminen tapahtuu, kun AAD Luo JWT-tunnuksen, mutta ennen JWT tunnuksen lähetetään pelaajat DRM käyttöoikeuden toimituksen palvelut-AMS vahvistusta varten? 

Koska näppäintä on koottu milloin tahansa, on aina useita kelvollinen julkisella avaimella käytettävissä federation metatietojen asiakirja. Azure Media-palveluiden käyttöoikeus toimituksen voit käyttää näppäimet määritetty asiakirja, koska yksi näppäin on koottu pian, toiseen saattaa olla sen Korvaava- ja niin edelleen.

### <a name="where-is-the-access-token"></a>Missä on Access-tunnuksen?

Jos tarkastelet miten verkkosovellukseen soittaa API-sovelluksen kohdassa [OAuth 2.0 asiakkaan tunnistetiedot myöntäminen sovelluksen Identity](active-directory-authentication-scenarios.md#web-application-to-web-api)-todennus-työnkulku on alla:

1.  Käyttäjä on kirjautunut Azure AD-web-sovelluksen (katso [Selaimen verkkosovellukseen](active-directory-authentication-scenarios.md#web-browser-to-web-application).
2.  Azure AD-todennus päätepisteen ohjaa käyttäjäagentin takaisin asiakassovellus todennus-koodilla. Käyttäjäagentin palauttaa luvan koodi asiakassovellukseen uudelleenohjaus URI.
3.  Web-sovelluksen on hankittava käyttöoikeustietue, niin, että voi todentaa verkko-Ohjelmointirajapinnan ja hakea haluamasi resurssi. Se on pyyntö Azure AD suojaustunnuksen päätepisteen, tunnistetieto, Asiakastunnus ja API-verkkosovelluksen tunnus URI. Sen tuottamat osoittamaan, että käyttäjä on hyväksynyt todennus-koodi.
4.  Azure AD todentaa sovellus, ja palauttaa JWT käyttöoikeustietue, jota käytetään verkko-Ohjelmointirajapinnan kutsu.
5.  Päälle HTTPS-web-sovelluksen avulla palautettujen JWT käyttöoikeustietue JWT merkkijonon "Haltijan" nimeäminen lisääminen verkko-Ohjelmointirajapinnan pyynnön Authorization-otsikko. Verkko-Ohjelmointirajapinnan sitten tarkistaa JWT-tunnuksen, ja jos vahvistus on tarkistettu, palauttaa halutun resurssi.

"Sovelluksen käyttäjätiedot" tässä työnkulussa verkko-Ohjelmointirajapinnan luottaa verkkosovelluksen todennetun käyttäjän. Tästä syystä tätä mallia kutsutaan luotettujen alirakenne. [Kaavion tällä sivulla](http://msdn.microsoft.com/library/azure/dn645542.aspx/) kerrotaan, miten luvan koodin myöntää työnkulku toimii.

Suojaustunnuksen rajoituksen kanssa hankkimista on seuraavat luotettujen alirakenne samoissa. Ja Azure Media-palveluiden käyttöoikeus toimitus-palvelu on verkko-Ohjelmointirajapinnan resurssin "Taustajärjestelmä resurssi-web-sovelluksen täytyy käyttää. Missä on niin access-tunnuksen?

Olemme ovat itse asiassa hankkiminen käyttöoikeustietue Azure AD. Onnistuneiden käyttäjän todentaminen-jälkeen luvan koodin palautetaan. Todennus-koodin sitten käytetään, ja asiakkaan tunnuksen ja sovelluksen näppäintä, käyttöoikeustietue for Exchange. Ja käyttöoikeustietue on käyttäminen "osoitin"-sovellus valitsemalla tai edustava Azure Media käyttöoikeuden toimituksen palvelut.

Tarvitsemme Rekisteröi ja Azure AD "osoitin"-sovelluksen määrittäminen noudattamalla seuraavia ohjeita:

1.  Azure AD-vuokraajan

    - Lisää sovellus (resurssi) on Sign URL: 

    https://[resource_name].azurewebsites.NET/ ja 

    - sovelluksen tunnus URL-osoite: 
    
    https://[aad_tenant_name].onmicrosoft.com/[resource_name]; 
2.  Lisää resurssin; sovelluksen uusi avain
3.  Päivitä sovellus luettelon tiedoston niin, että groupMembershipClaims-ominaisuus on seuraava arvo: "groupMembershipClaims": "Kaikki",  
4.  Lisää resurssi-sovellus, joka on lisätty edellä kuvatun vaiheen 1 toisto-ohjelman web App-sovellukseen valitsemalla Azure AD-sovelluksessa "muiden sovellusten käyttöoikeudet"--osaan. Valitse "valtuutetun käyttöoikeudet-kohdassa"Access [resource_name]"valintamerkki. Näin web Appin oikeus Accessin käyttämiseen resurssin sovelluksen tunnusten luontia varten. Pitäisi tehdä tämän sekä paikallisen ja otettu käyttöön web app-versio, jos kehität Visual Studio ja Azure web App-ohjelmalla.
    
Tämän vuoksi myöntämä Azure AD JWT tunnuksen on varmasti käyttöoikeustietue "osoitin" tämän resurssin käyttämiseen.

### <a name="what-about-live-streaming"></a>Live Entä Streaming?

Edellä Microsoftin keskustelun on ollut keskitytään tarvittaessa resurssit. Live Entä streaming?

Hyviä uutisia on, että voit käyttää täsmälleen saman suunnittelu ja käyttöönotto suojaamiseen live streaming Azure Media-palveluiden käsittelemällä resurssi liitetty "VOD resurssi-ohjelmaa.

Tarkemmin sanottuna on tunnettujen että tekemään live streaming Azure Media-palveluja, sinun on kanavan ja valitse sitten ohjelma, valitse kanavan luominen. Jos haluat luoda ohjelma, haluat luoda resurssi, joka sisältää live arkisto-ohjelman. Jotta voit antaa CENC usean DRM suojaus live sisällön, kaikki, on suoritettava, on käyttää samaa asetukset/käsittelyn kohteen kuin, jos se on "VOD resurssi", ennen kuin käynnistät ohjelman.

### <a name="what-about-license-servers-outside-of-azure-media-services"></a>Mitä tietoja palvelimet Azure Media Services ulkopuolella?

Asiakkaiden on usein sijoittaa käyttöoikeuden palvelinfarmin joko omat tietonsa keskelle tai isännöidään DRM palveluntarjoajien mukaan. Lähettäjä-Azure Media sisällön suojauksen avulla voit toimia hybrid-tilassa: sisältö isännöidään ja suojattu dynaamisesti Azure Media Services kun DRM käyttöoikeuksien toimitetaan ulkopuolella Azure Media Services-palvelimet. Tässä tapauksessa on muutokset seuraavat seikat:

1. Suojaustunnuksen suojattu palvelu on annettava tunnuksia, jotka on hyväksyttävä ja voidaan tarkistaa käyttöoikeuden palvelinklusterin. Esimerkiksi Axinom myöntämä Widevine käyttöoikeuden palvelimet vaatii tietyn JWT tunnus, joka sisältää "oikeuden viestin". Siksi sinun täytyy olla tällaisia JWT tunnuksen myöntää STS. Tekijät suorittanut tällaisten toteutus ja tietoja [Azure dokumentaatio](https://azure.microsoft.com/documentation/)Centerissä seuraavan asiakirjan: [Käyttämällä Axinom aikana Widevine käyttöoikeuksia Azure Media-palveluita](media-services-axinom-integration.md). 
1. Et enää tarvitse käyttöoikeuden toimitus-palvelun (ContentKeyAuthorizationPolicy) määrittäminen Azure Media Services-palveluissa. Sinun on suoritettava on antaa käyttöoikeuden hankkiminen URL-osoitteet (esimerkiksi PlayReady Widevine ja FairPlay) Kun määrität AssetDeliveryPolicy määrittäminen CENC usean DRM kanssa.
 
### <a name="what-if-i-want-to-use-a-custom-sts"></a>Entä jos haluan käyttää mukautettuja STS?

Voi olla useita syitä, jotka asiakas pakko käyttää mukautettuja STS-ympäristö (Secure suojaustunnuksen Service) tarjoamiseksi JWT tunnukset. Jotkin niistä ovat seuraavat:

1.  STS-ympäristö ei tue tunnistetietojen toimittaja (IDP) asiakas käyttää. Tässä tapauksessa mukautetun STS ehkä haluamasi vaihtoehto.
2.  Asiakkaan on ehkä Lisää joustavampi tai tiukempi ohjausobjektiin STS-integraation asiakkaan tilaajan järjestelmän laskutus. Esimerkiksi MVPD operaattori saattaa tarjota useita OTT tilaajan pakettien kuten premium, basic, urheilu, jne. Operaattori kannattaa vastaa saatavat tunnuksessa tilaajan-paketti, niin, että vain oikea paketin sisällön otetaan käyttöön. Tässä tapauksessa mukautetun STS-ympäristö on tarpeen mukaan joustavampi ja ohjausobjektin.

Kaksi muutokset on suoritettava mukautettujen STS käytettäessä:

1.  Käyttöoikeuden toimitus-palvelun, annetaan määritettäessä haluat määrittää mukautetun STS (enemmän tietoja alla) sen sijaan, että nykyinen avain Azure Active Directorysta yhteydenottotapa käyttämä suojausavain.
2.  JTW tunnuksen luodaan, kun se on määritetty sen sijaan, että nykyinen X509 yksityinen avain Azure Active Directory-sertifikaatti.

Suojauksen näppäinyhdistelmien kahdenlaisia:

1.  Symmetrisen avaimen: samaa avainta käytetään sekä luodaan ja tarkistetaan JWT tunnuksen;
2.  Julkiseen avain: julkisen ja yksityisen avaimen pari-X509 sertifikaattia käytetään ja salaus/luonnissa JWT tunnuksen ja julkinen avain tunnuksen vahvistamisesta yksityinen avain.

####<a name="tech-note"></a>Huomautus teknisille

Jos käytössäsi on .NET Framework ja C# as development platform X509 julkiseen suojaus-näppäimen käytettyä varmennetta on oltava vähintään 2048 pituus. Tämä on luokan .NET Framework System.IdentityModel.Tokens.X509AsymmetricSecurityKey. Muussa tapauksessa järjestelmässä Seuraava poikkeus:

IDX10630: 'System.IdentityModel.Tokens.X509AsymmetricSecurityKey' allekirjoittamiseen ei voi olla pienempi kuin '2048' bittien. 

## <a name="the-completed-system-and-test"></a>Valmiit järjestelmän ja testi

Selkeät muutamia skenaarioita valmiit lopusta loppuun-järjestelmässä niin, että lukijat voi olla basic "kuva" toiminnan ennen kirjautumista-tilin hankkiminen.

Toisto-ohjelman web-sovelluksen ja sen sisäänkirjautuminen löytyy [tähän](https://openidconnectweb.azurewebsites.net/).

Jos tarvitset on "ei-integroitu" skenaario: video varat isännöidään Azure Media-palveluja, jotka ovat joko suojaamattomia tai DRM suojattu ilman suojaustunnuksen todennus (käyttöoikeuden myöntämisestä kulloinkin voit pyytää sitä), voit kokeilla sitä ilman login (siirtymällä HTTP videon streaming onko over HTTP).

Jos tarvitset on integroitu lopusta loppuun-skenaario: video varat on dynaaminen DRM-suojauksen Azure Media-palveluiden tunnus todennusta ja JWT tunnuksen luomaa Azure AD-kohdassa, sinulla on oltava kirjautuminen.

### <a name="user-login"></a>Sisäänkirjautuminen

Jotta voit esikatsella lopusta loppuun integroitu DRM-järjestelmä-, sinun täytyy on "tili" luonut tai joita olet lisännyt. 

Minkä tilin?

Vaikka Azure alun perin käyttöoikeutta vain niiden käyttäjien Microsoft-tilin, se sallii nyt molemmissa käyttäjät pääsevät. Tämä on tehty siten, että kaikki Azure ominaisuudet luota Azure AD todennustavaksi ottaa Azure AD todentaa organisaation käyttäjät ja luomalla federation yhteyden mihin Azure AD luottaa Microsoft tilin kuluttaja tunnistetietojen järjestelmän kuluttaja käyttäjien todentamiseen. Azure AD on tuloksena voi todentaa "Vieras" Microsoft-tilit sekä "alkuperäisen" Azure AD-tilit.

Koska Azure AD luottaa Microsoft-tili (MSA) toimialueen, voit lisätä minkä tahansa tilien jossakin seuraavista toimialueiden mukautetun Azure AD vuokraaja ja tili, kirjaudu sisään käyttämällä:

**Toimialuenimi**|**Toimialueen**
-----------|----------
**Mukautettu Azure AD-vuokraajan toimialueen**|somename.onmicrosoft.com
**Yrityksen toimialueen**|Microsoft.com-sivustosta
**Microsoft-tili (MSA) toimialueen**|Outlook.com, live.com, hotmail.com

Saa yhteyttä tekijät on luotu tai lisätä tili. 

Alla on näytettävä kirjautumissivu toimialueesta tilin näyttökuvat.

**Mukautetun Azure AD-vuokraajan toimialuetilin**: Tällöin näet Mukautettu mukautetun kirjautumissivun Azure AD-vuokraajan toimialue.

![Mukautettu Azure AD-vuokraajan toimialuetili](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain1.png)

**Toimialueen Microsoft-tiliäsi älykortin**: Tällöin näet Microsoft yrityksen mukautettu kirjautumissivu kaksiosainen todentamismenetelmä IT.

![Mukautettu Azure AD-vuokraajan toimialuetili](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain2.png)

**Microsoft-tili (MSA)**: Tässä tapauksessa näet Microsoft Account kirjautumissivun kuluttajille.

![Mukautettu Azure AD-vuokraajan toimialuetili](./media/media-services-cenc-with-multidrm-access-control/media-services-ad-tenant-domain3.png)

### <a name="using-encrypted-media-extensions-for-playready"></a>Käyttämällä PlayReady salattuja Media-laajennukset

Moderni selaimen kanssa salattu Media laajennukset (EME) PlayReady tukea, kuten Internet Explorer 11 Windows 8. 1 ja ylöspäin ja Windows 10: ssä, Microsoft Edge-selaimella PlayReady ovat pohjana DRM EME varten.

![EME käytön PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready1.png)

Tumma player-alue on se, että PlayReady suojaus estää jokin tekemästä näyttökuvan suojatun videon. 

Seuraavassa kuvassa on Playerin laajennukset ja MSE/EME tuki.

![EME käytön PlayReady](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-playready2.png)

Microsoft Edge ja Internet Explorer 11 Windows 10: ssä EME avulla käynnistettäessä [PlayReady SL3000](https://www.microsoft.com/playready/features/EnhancedContentProtection.aspx/) , jotka tukevat sitä Windows 10-laitteissa. PlayReady SL3000 lukitus parannettu sisältöä kulun (4K, Tuotekatalogiotsikko, jne.) ja uusi sisältö toimituksen mallien (parannettu sisällön aikainen ikkuna).

Keskity Windows-laitteet: PlayReady on vain DRM-Windows-laitteissa (PlayReady SL3000) KV. Streaming-palvelu, voit käyttää PlayReady EME tai UWP-sovelluksen kautta ja tarjota käyttämällä PlayReady SL3000 kuin toisen DRM videon laatua. Yleensä 2K sisällön flow Chrome ja Firefox ja 4K sisältöä Microsoft Edge/IE11 tai samassa laitteessa (mukaan Palveluasetukset ja käyttöönoton) UWP-sovelluksen kautta.


#### <a name="using-eme-for-widevine"></a>Käyttämällä EME Widevine

Nykyaikainen selaimesta EME/Widevine tuki, kuten Chrome 41 + Windows 10: ssä, Windows 8.1 tai Mac OS x Yosemiten Chrome-Android 4.4.4 Google Widevine on DRM EME takana.

![Käyttämällä EME Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine1.png)

Huomaa, että Widevine ei estä jokin tekemästä siepata suojattu video.

![Käyttämällä EME Widevine](./media/media-services-cenc-with-multidrm-access-control/media-services-eme-for-widevine2.png)

### <a name="not-entitled-users"></a>Ole oikeutettuja käyttäjät

Jos käyttäjä ei ole "oikeutettuja käyttäjät"-ryhmän jäsen, käyttäjä voi välittää "oikeuden valintaruudun" ja usean DRM käyttöoikeuden palveluun kieltäytyä antaa pyydetty käyttöoikeuden alla kuvatulla tavalla. Yksityiskohtainen kuvaus on "käyttöoikeuden hankkia epäonnistui", joka on suunniteltu.

![Yhdistelmävisualisoinnin oikeutettu käyttäjät](./media/media-services-cenc-with-multidrm-access-control/media-services-unentitledusers.png)

### <a name="running-custom-secure-token-service"></a>Mukautetun suojatun suojaustunnuksen palvelua

Suorittaa mukautetun suojatun suojaustunnuksen Service (STS) skenaarion JWT tunnuksen antaa mukautetun STS symmetrisen tai julkiseen avaimen avulla. 

Kirjainkoon (käyttäminen Chrome) symmetrisen avaimen avulla:

![Mukautetun STS käynnissä](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts1.png)

Isot X509 kautta julkiseen avaimen avulla varmenne (Microsoft Moderni selaimen avulla).

![Mukautetun STS käynnissä](./media/media-services-cenc-with-multidrm-access-control/media-services-running-sts2.png)

Molemmissa tapauksissa käyttöoikeuksien pysyy muuttumattomana – Azure AD kautta. Ainoa ero on JWT tunnusten myöntämä mukautetun STS Azure AD sijaan. Dynaaminen CENC suojaus määritettäessä käyttöoikeuden toimituksen palvelun rajoitus määrittää JWT tunnuksen symmetrisen tai julkiseen-näppäintä.

## <a name="summary"></a>Yhteenveto

Tässä asiakirjassa on kerrottu CENC Avainhankkeiden monivaiheisen-native-DRM ja access-ohjausobjektin kautta suojaustunnuksen todennus: rakenteineen ja ylläpitokustannusten Azure, Azure Media Services ja Azure Media Player-ohjelmassa.

- Viittaus rakenne esitetään, joka sisältää DRM/CENC alirakenne; tarvittavat komponentit
- Viittaus käyttöönoton Azure, Azure Media Services ja Azure Media Player-ohjelmassa.
- Jotkin aiheet suoraan osallistuvan suunnittelu ja käyttöönotto myös käsitellään.


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Vahvistus 

William Zhang, Mingfei Yan, ja Roland tiedostoon frangin, Kilroy Hughes, Julia Kornich
