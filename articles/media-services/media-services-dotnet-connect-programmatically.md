<properties 
    pageTitle="Yhteyden muodostaminen Media Services-tilin .NET" 
    description="Tässä ohjeaiheessa kerrotaan, miten Media Services uisng .NET yhdistäminen." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Yhteyden muodostaminen Media Services-tilin käyttämällä .NET Media Services SDK

> [AZURE.SELECTOR]
- [MUILLE KÄYTTÄJILLE](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


Tässä ohjeaiheessa kerrotaan ohjelmallisesti yhteyden muodostaminen Microsoft Azure Media Services hankkiminen, kun ovat ohjelmointi Media Services SDK .NET.


## <a name="connecting-to-media-services"></a>Yhteyden muodostaminen Media-palveluita

Muodostaa ohjelmallisesti Media-palveluihin, sinun on on aiemmin määrittäminen Azure-tili, Media-palveluita käyttävät kyseisen tilin, ja Visual Studio projektin kehittämiseen Media Services SDK sitten määrittäminen .NET. Lisätietoja on artikkelissa asennuksen kehittämisen ja Media-palveluiden SDK .NET.

Media Services-tilin määritysprosessi loppuun saada yhteyden seuraavat arvot. Käytä näitä halutaan ohjelmallisesti yhteyksiä Media-palveluita.

- Media-palveluiden tilin nimen.

- Media Services-tiliavain.

Näiden arvojen etsiminen Siirry Azure Management-portaaliin, valitse Media Service-tili ja sitten "**Hallinta NÄPPÄIMET**"-kuvakkeen päälle portal-ikkunan alareunassa. Jokaisen tekstiruudun vieressä olevaa kuvaketta valitsemalla kopioi arvon järjestelmän Leikepöydälle.


## <a name="creating-a-cloudmediacontext-instance"></a>CloudMediaContext esiintymän luominen

Ohjelmointi vastaan Media Services alkavan sinun on luotava **CloudMediaContext** esiintymän, joka edustaa konteksti. **CloudMediaContext** on tärkeää sivustokokoelmat, mukaan lukien työt, resurssien, tiedostot, käytäntöjen ja locators viittauksia.

>[AZURE.NOTE] **CloudMediaContext** luokka ei ole Turvalliset viestiketjun. Sinun on luotava uusi CloudMediaContext viestiketjun tai joukko toimintoja kohti.


CloudMediaContext on viisi konstruktori Osastollasi. On suositeltavaa käyttää konstruktoreja, jotka **MediaServicesCredentials** parametrina. Lisätietoja on artikkelissa **Uudelleen Access ohjausobjektin palvelun tunnuksia** , joka seuraa. 

Seuraavassa esimerkissä julkinen CloudMediaContext(MediaServicesCredentials credentials) muodostin:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Accessin ohjausobjektin palvelun tunnusten käyttäminen uudelleen

Tässä osassa esitellään siitä, miten voit käyttää Access Control-palvelu tunnusten CloudMediaContext konstruktoreja, jotka MediaServicesCredentials parametrina.


[Azure Active Directory käyttöoikeuksien valvonta](https://msdn.microsoft.com/library/hh147631.aspx) (tunnetaan myös nimellä Access Control-palvelu tai ACS) on pilvipohjaisia palveluja, jonka avulla saat käyttöösi-verkkosovelluksistaan helposti käyttöoikeudet ja myöntää käyttäjille. Microsoft Azure Media Services ohjaa sen käyttöoikeus, vaikka OAuth-protokolla, joka edellyttää ACS-tunnuksen. Media-palveluiden saa ACS tunnusten todennus-palvelimeen.

Kun kehittämisen on Media SDK: ssa, voit käsitellä ei tunnusten koska SDK koodi valvojat niihin puolestasi. Kuitenkin auttaa muita manage täysin ACS tunnusten SDK ohjaa tarpeettomat suojaustunnuksen pyyntöihin. Pyytää tunnusten kestää jonkin aikaa, ja siinä käytetään asiakkaan ja palvelimen resursseja. Lisäksi ACS palvelimen throttles pyynnöt, jos korko on liian suuri. Rajoitus on 30 pyynnöt sekunnissa on artikkelissa lisätietoja [ACS palvelurajoitukset](https://msdn.microsoft.com/library/gg185909.aspx) .

Voit käyttää ACS tunnusten Media Services SDK versio 3.0.0.0 alkaen. **CloudMediaContext** konstruktoreja, jotka **MediaServicesCredentials** parametrina ottaa jakamisen ACS-tunnusten useita kontekstit välillä. MediaServicesCredentials luokan kapseloi Media Services-tunnistetiedot. Jos sen päättymisaika kutsutaan ACS-tunnus on käytettävissä, voit luoda uuden MediaServicesCredentials esiintymän tunnuksen ja välittää CloudMediaContext konstruktoria. Huomaa, että Media Services SDK päivittää tunnusten automaattisesti aina, kun ne on voimassa. Kahdella uudelleen ACS tunnuksia, kuten seuraavissa esimerkeissä.

- Voit tallentaa **MediaServicesCredentials** objektin (esimerkiksi ovat staattisen luokan muuttuva). Siirtää sitten välimuistissa objektin CloudMediaContext konstruktoria. MediaServicesCredentials objekti sisältää ACS-tunnuksen, joka voi käyttää uudelleen, jos se on edelleen voimassa. Jos tunnuksen ei ole kelvollinen, se päivitetään mukaan Media-palveluiden SDK käyttämällä MediaServicesCredentials konstruktoria annetuista tunnistetiedoista.

    Huomaa, että **MediaServicesCredentials** objektin saa kelvollinen tunnus jälkeen RefreshToken kutsutaan. **CloudMediaContext** tallenteita konstruktoria **RefreshToken** -menetelmää. Jos aiot tallentaa tunnuksen arvot ulkoisen tallennustilan, varmista, että voit tarkistaa, onko TokenExpiration arvo on kelvollinen ennen tiedoston tallentamista suojaustunnuksen tiedot. Jos se ei ole kelvollinen, Soita RefreshToken ennen välimuistiin.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Voit myös tallentaa AccessToken merkkijonon ja TokenExpiration arvot. Arvot käyttää myöhemmin luoda uuden MediaServicesCredentials-objektia suojaustunnuksen välimuistiin tallennetut tiedot.  Tämä on erityisen hyödyllisiä skenaariot, jossa tunnuksen voidaan turvallisesti jakaa useita prosesseja tai tietokoneiden välillä.

    Seuraava koodikatkelmat Soita SaveTokenDataToExternalStorage, GetTokenDataFromExternalStorage ja UpdateTokenDataInExternalStorageIfNeeded menetelmiä, joita ei ole määritetty tässä esimerkissä. Voit määrittää tallentamiseen, hakeminen ja ulkoiset tallennustilan suojaustunnuksen tietojen seuraavista tavoista. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Tallennetun tunnuksen arvot avulla voit luoda MediaServicesCredentials.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Päivitä suojaustunnuksen kopio siltä varalta, että tunnus on päivittänyt Media Services SDK. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Jos käytössäsi on useita Media Services-tilejä (esimerkiksi kuormituksen jakamisen tarkoituksiin tai Geo-jakauman) voit välimuistiin MediaServicesCredentials objektien System.Collections.Concurrent.ConcurrentDictionary kerääminen (ConcurrentDictionary sivustokokoelman kuvaa, jota voi käyttää useita viestiketjuissa siirtyminen samanaikaisesti avain/arvo-pareina viestiketjun sopivaa kokoelma). Voit sitten Hae välimuistissa olevia GetOrAdd-menetelmää. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Yhteyden Media Services-tilin Pohjois kiina-alueella

Jos tilisi sijaitsee Pohjois Kiinan alueen, käytä seuraavia konstruktoria:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Esimerkki:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Tallentaminen yhteyden arvojen määrittäminen

On erittäin suositeltavaa voit tallentaa yhteyden arvoja, erityisesti luottamuksellisia arvoja, kuten tilin nimi ja salasana-määritys. On myös Suosituksena Salaa luottamukselliset määritykset tiedot. Voit salata koko määritystiedosto Windows salaaminen File System (EFS) avulla. EFS tiedostoa käyttöön tiedostoa hiiren kakkospainikkeella, valitse **Ominaisuudet**ja salauksen asetukset **Lisäasetukset** -välilehdessä Ota käyttöön. Tai voit luoda mukautetun ratkaisun salaamiseen määritystiedoston valitun osiin suojatun määrityksen avulla. Katso [salauksen määritysten tietojen avulla suojattujen kokoonpano](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Seuraava App.config-tiedosto sisältää yhteyden arvot. Arvot <appSettings> elementti on pakolliset arvot, joka on saatu Media Services-tilin määritysprosessi loppuun.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Noutaa yhteyden arvot määritykset, voit käyttää **ConfigurationManager** luokkaa ja määrittää arvot kentät koodissa:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
