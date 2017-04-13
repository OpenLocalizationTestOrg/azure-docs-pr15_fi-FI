<properties
    pageTitle="Avaimen palauttaminen kirjautumisessa Azure AD | Microsoft Azure"
    description="Tässä artikkelissa käsitellään allekirjoitetun avaimen palauttaminen parhaita käytäntöjä Azure Active Directory"
    services="active-directory"
    documentationCenter=".net"
    authors="gsacavdm"
    manager="krassk"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="gsacavdm"/>

# <a name="signing-key-rollover-in-azure-active-directory"></a>Kirjautuminen Azure Active Directoryn avaimen palauttaminen

Tässä ohjeaiheessa kerrotaan, mitä tarvitset lisätietoja julkisen näppäimet, joita käytetään Azure Active Directory (Azure AD) suojaus tunnusten kirjautua. On tärkeää muistaa, että nämä näppäimet palauttaminen säännöllisin väliajoin ja emergency voi koota heti. Kaikki sovellukset, jotka käyttävät Azure AD pitäisi ohjelmallisesti käsittelee tärkeimmät palauttaminen prosessin tai vahvistaa säännöllisiä manuaalinen palauttaminen prosessi. Jatka lukemista selvittääksesi, miten näppäimet toimivat, miten voit arvioida sovelluksen palauttaminen vaikutusta ja tai päivittää sovelluksen säännöllisiä manuaalinen palauttaminen prosessin käsittelee tärkeimmät palauttaminen tarvittaessa.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Sisäänkirjautuminen näppäimet Azure AD yleiskatsaus

Azure AD käytetään julkisen avaimen salaus yleisesti käytettyjen rakennettu muodostaa Salli itse ja sovelluksia, jotka käyttävät sitä. Käytännössä tämä toimii seuraavasti: Azure AD käyttää allekirjoitetun avainta, joka koostuu julkisista ja yksityisistä avaimen pari. Kun käyttäjä kirjautuu sisään Azure AD todennusta käyttävä sovellus, Azure AD Luo suojaustunnus, joka sisältää käyttäjän tietoja. Tunnuksessa on allekirjoittanut Azure AD käyttämällä sen yksityinen avain, ennen kuin se lähetetään sovelluksen. Varmista, että tunnus on voimassa ja todella peräisin Azure AD-sovellus on vahvistettava tunnuksen allekirjoituksen Azure AD, jotka sisältyvät vuokraajan [OpenID yhteyden etsiminen](http://openid.net/specs/openid-connect-discovery-1_0.html) tai SAML/WS-Fed [federation metatietojen asiakirjan](active-directory-federation-metadata.md)näyttämiä julkisen avaimen avulla.

Tietoturvasyistä Azure AD kirjautuminen avaimen rullina säännöllisin väliajoin ja, jos kyseessä emergency voi koota heti. Sovellus, jossa Azure AD integroituu laaditaan käsittelee tärkeimmät palauttaminen tapahtumasta riippumatta siitä, kuinka usein se saattaa ilmetä. Jos näin ei ole, sovellus yrittää tunnusta allekirjoituksen tarkistaminen vanhentunut-näppäimen avulla-kirjautuminen pyynnön epäonnistuu.

On aina enemmän kuin yksi kelvollinen avain käytettävissä OpenID yhteyden etsiminen asiakirjan ja federation metatietojen asiakirja. Sovelluksen pitäisi olla valmiita, voit käyttää mitä tahansa asiakirjan määritetty näppäimet, koska yksi näppäin on koottu pian, toiseen saa sen Korvaava- ja niin edelleen.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Voit arvioida, jos sovellus vaikuttaako ja mitä voit tehdä siitä

Miten sovelluksesi käsittelee tärkeimmät palauttaminen määräytyy muuttujat, kuten sovelluksen tai mitä identity-protokollan ja kirjasto on käytetty tyyppi. Seuraavissa osissa arvioida, onko yleisimpien sovellusten avaimen palauttaminen vaikuttaa ja antaa ohjeita siitä, miten voit päivittää sovelluksen tue automaattinen palauttaminen tai manuaalinen päivittäminen näppäintä.

* [Resurssien käyttäminen alkuperäisen asiakassovellukset](#nativeclient)
* [Web-sovellusten / ohjelmointirajapinnan käyttäminen resurssit](#webclient)
* [Web-sovellusten / API suojaaminen resurssit ja sisäisten Azure App Services-palvelujen avulla](#appservices)
* [Web-sovellusten / API resurssien .NET OWIN OpenID yhteyden, WS Fed tai WindowsAzureActiveDirectoryBearerAuthentication middleware suojaaminen](#owin)
* [Web-sovellusten / API resurssien .NET Core OpenID muodostaa tai JwtBearerAuthentication middleware suojaaminen](#owincore)
* [Web-sovellusten / API suojaaminen Node.js passport azure-ad-moduulin resurssit](#passport)
* [Web-sovellusten / API suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2015](#vs2015)
* [Verkkosovellusten suojaaminen resurssit ja joka on luotu käyttämällä Visual Studio 2013](#vs2013)
* [Verkko-ohjelmointirajapinnan suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2013](#vs2013_webapi)
* [Verkkosovellusten suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2012](#vs2012)
* [Verkkosovellusten suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2010 2008 o käyttämällä Windows Identity Foundation](#vs2010)
* [Web-sovellusten / API resursseja muiden kirjastojen avulla tai manuaalisesti käyttöönoton jokin seuraavista tuetuista protokollista suojaaminen](#other)

Nämä ohjeet on **ei** käytettävissä:

* Sovellusten lisätty Azure AD-sovelluksen valikoimasta (mukaan lukien mukautettu) on erilliset ohjeet tapauksen kirjautuminen avaimet. [Lisätietoja.](active-directory-sso-certs.md)
* Paikalliset sovellukset, jotka on julkaistu sovelluksen välityspalvelimen kautta ei tarvitse huolehtia kirjautuminen avaimet.

### <a name="nativeclient"></a>Resurssien käyttäminen alkuperäisen asiakassovellukset

Sovellukset, jotka ovat vain käytettäessä resurssien (ts Microsoft Graph, KeyVault, Outlook-Ohjelmointirajapinnan ja muihin Microsoft-APIs) yleensä vain tunnuksen hankkiminen ja välittää sen pitkin resurssin omistaja. Koska, että ne ovat suojaaminen ei resursseja, ei tarkasta tunnuksen ja näin ollen tarvitse varmistaaksesi, että se on oikein allekirjoitettu.

Alkuperäisen asiakassovelluksissa työpöydän tai matkapuhelimen, kuuluvat tähän luokkaan ja näin ei koske palauttaminen.

### <a name="webclient"></a>Web-sovellusten / ohjelmointirajapinnan käyttäminen resurssit

Sovellukset, jotka ovat vain käytettäessä resurssien (ts Microsoft Graph, KeyVault, Outlook-Ohjelmointirajapinnan ja muihin Microsoft-APIs) yleensä vain tunnuksen hankkiminen ja välittää sen pitkin resurssin omistaja. Koska, että ne ovat suojaaminen ei resursseja, ei tarkasta tunnuksen ja näin ollen tarvitse varmistaa sen allekirjoituksen.

Web-sovellusten ja verkko-ohjelmointirajapinnan, jotka käyttävät vain sovelluksen kulun (asiakkaan käyttäjätiedot / Asiakasvarmenne), kuuluvat tähän luokkaan ja näin ollen ei koske palauttaminen.

### <a name="appservices"></a>Web-sovellusten / API suojaaminen resurssit ja sisäisten Azure App Services-palvelujen avulla

Azure App-palvelujen todennus / todennus (EasyAuth)-toiminto on jo tarvittavat logiikan käsittelee tärkeimmät palauttaminen automaattisesti.

### <a name="owin"></a>Web-sovellusten / API resurssien .NET OWIN OpenID yhteyden, WS Fed tai WindowsAzureActiveDirectoryBearerAuthentication middleware suojaaminen

Jos sovellus on käytössä .NET OWIN OpenID yhteyden, WS Fed tai WindowsAzureActiveDirectoryBearerAuthentication middleware, se on jo tarvittavat logiikan käsittelee tärkeimmät palauttaminen automaattisesti.

Voit vahvistaa, että sovelluksesi on käytössä jokin seuraavista hakemalla jokin seuraavista katkelmat sovelluksen Startup.cs tai Startup.Auth.cs

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
    });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="owincore"></a>Web-sovellusten / API resurssien .NET Core OpenID muodostaa tai JwtBearerAuthentication middleware suojaaminen

Jos sovellus on .NET Core OWIN OpenID yhteyden tai JwtBearerAuthentication middleware, se on jo tarvittavat logiikan käsittelee tärkeimmät palauttaminen automaattisesti.

Voit vahvistaa, että sovelluksesi on käytössä jokin seuraavista hakemalla jokin seuraavista katkelmat sovelluksen Startup.cs tai Startup.Auth.cs

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
    });
```

### <a name="passport"></a>Web-sovellusten / API suojaaminen Node.js passport azure-ad-moduulin resurssit

Jos sovellus on Node.js passport ad-moduulin, se on jo tarvittavat logiikan käsittelee tärkeimmät palauttaminen automaattisesti.

Voit vahvistaa, että sovelluksen passport-ad-sovelluksen app.js seuraavat koodikatkelman hakemalla

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="vs2015"></a>Web-sovellusten / API suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2015

Jos sovellus on luotu käyttämällä web-sovelluksen malli Visual Studio 2015 ja **Muuta todennus** -valikosta valittu **Työ-ja Oppilaitostileiltä** , se on jo tarvittavat logiikan käsittelee tärkeimmät palauttaminen automaattisesti. Tämä logiikka OWIN OpenID yhteyden, middleware upotettuna hakee ja välimuistiin OpenID yhteyden etsiminen asiakirjasta näppäimet ja säännöllisesti päivittää ne.

Jos lisäsit todennus-ratkaisun manuaalisesti, sovellus ei ehkä ole tarpeen avaimen palauttaminen logiikan. Sinun on kirjoittaminen itse ja noudata ohjeita [Web-sovellusten / API muiden kirjastojen avulla tai manuaalisesti käyttöönoton jokin seuraavista tuetuista protokollista.](#other).

### <a name="vs2013"></a>Verkkosovellusten suojaaminen resurssit ja joka on luotu käyttämällä Visual Studio 2013

Jos sovellus on luotu käyttämällä web-sovelluksen malli Visual Studio 2013 ja olet valinnut **Organisaation tili** **Muuta todennus** -valikosta, se on jo tarvittavat logiikan käsittelee tärkeimmät palauttaminen automaattisesti. Tämä logiikka tallentaa organisaation yksilöllinen ja allekirjoitetun avaintiedoista projektiin liittyvän tietokannan taulukot. Löydät yhteysmerkkijono Web.config projektitiedoston tietokannan.

Jos lisäsit todennus-ratkaisun manuaalisesti, sovellus ei ehkä ole tarpeen avaimen palauttaminen logiikan. Sinun on kirjoittaminen itse ja noudata ohjeita [Web-sovellusten / API muiden kirjastojen avulla tai manuaalisesti käyttöönoton jokin seuraavista tuetuista protokollista.](#other).

Seuraavat vaiheet avulla voit tarkistaa logiikan toimii oikein, sovelluksen.

1. Visual Studio 2013: n Avaa ratkaisu ja valitse sitten oikea ikkunan **Palvelimen Explorer** -välilehdessä.
2. Laajenna **tietoyhteyksiä**, **DefaultConnection**ja sitten **taulukot**. Etsi **IssuingAuthorityKeys** taulukko, napsauta sitä hiiren kakkospainikkeella ja valitse sitten **Näytä taulukkotiedot**.
3. **IssuingAuthorityKeys** taulukossa ole vähintään yksi rivi, joka vastaa avaimen allekirjoitus-arvoa. Poista taulukon rivejä.
4. **Alihallinnat** taulukkoa hiiren kakkospainikkeella ja valitse sitten **Näytä taulukkotiedot**.
5. **Alihallinnat** taulukossa ole vähintään yksi rivi, joka vastaa yksilöivä hakemisto-vuokraajan tunnus. Poista taulukon rivejä. Jos poistat ei ja **Alihallinnat** -taulukon ja **IssuingAuthorityKeys** taulukon rivit, näkyviin tulee virhe suorituksen aikana.
6. Muodosta ja suorita sovellus. Kun olet kirjautunut sisään tiliin, voit lopettaa sovelluksen.
7. Palaa **Palvelimen Explorer** ja katsomalla **IssuingAuthorityKeys** ja **Alihallinnat** taulukon arvoissa. Huomaat, että ne on poistettu automaattisesti uudelleen federation metatietojen asiakirjasta tiedot.

### <a name="vs2013"></a>Verkko-ohjelmointirajapinnan suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2013

Jos olet luonut API-verkkosovellusta Visual Studio 2013-verkko-Ohjelmointirajapinnan mallin avulla ja valitsee sitten **Muuta käyttöoikeuksien** valikosta **Organisaation tili** , sinulla on jo tarvittavat logiikan sovelluksen.

Jos olet määrittänyt manuaalisesti todennusta, noudattamalla seuraavia ohjeita opit määrittämään verkko-Ohjelmointirajapinnan päivittää automaattisesti sen avaintiedoista.

Seuraavat koodikatkelman esitellään, miten hakee uusimmat näppäimet federation metatietojen asiakirja ja vahvista tunnuksen [JWT suojaustunnuksen käsittelijä](https://msdn.microsoft.com/library/dn205065.aspx) avulla. Koodikatkelman oletetaan, että käytettävän oma välimuistiin järjestelmä toteaa avain vahvistamiseen tulevien tunnusta Azure AD-onko kyseessä tietokannan, kokoonpanotiedosto tai muualla.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a>Verkkosovellusten suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2012

Jos sovellus on luotu Visual Studio 2012, todennäköisesti käytit käyttäjätieto- ja Accessin työkalu sovelluksen määrittämiseen. On myös todennäköistä, että käytät [Vahvistaminen myöntäjän nimi rekisterin (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). VINR on vastuussa luotettu tunnistetietojen toimittajat (Azure AD) tietoja ja vahvista tunnusten antamien käytettävät näppäimet. VINR myös on helppo päivittää automaattisesti Web.config-tiedosto tallennetaan lataamalla federation metatietojen asiakirjan uusin versio hakemistossa, liittyvät avaintiedoista tarkistaminen Jos määritystä ei ole ajan tasalla asiakirjan uusimman version kanssa ja päivitys sovellus käyttää uusi avain tarpeen mukaan.

Jos loit sovelluksen käyttämällä jotakin MALLIKOODEJA tai vaiheittainen kuvaus Microsoft toimitetussa, avaimen palauttaminen logiikan sisältyy jo projektin. Huomaat, että seuraava koodi on jo projektin. Jos sovellusta ei ole jo tämä logiikka, noudata seuraavia ohjeita voit lisätä sen ja varmista, että se toimii oikein.

1. Lisää **Ratkaisunhallinnassa**projektin **System.IdentityModel** kokoonpanon viittaus.
2. Avaa **Global.asax.cs** -tiedosto ja lisää seuraava teksti käyttämällä direktiivejä:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Lisää **Global.asax.cs** -tiedostoon seuraavasti:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Kutsua **Global.asax.cs** **Application_Start()** menetelmän **RefreshValidationSettings()** -menetelmää, kuten:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Kun olet tehnyt nämä vaiheet, sovelluksen Web.config päivitetään federation metatietojen asiakirjan, kuten uusimman näppäimet uusimmat tiedot. Tämä päivitys suoritetaan aina, kun sovelluksen sovellussarjan kierrättää IIS; Oletusarvoisesti IIS on määritetty Roskakorin sovellusten 29 tunnin välein.

Noudata seuraavia ohjeita ja tarkista, että tärkeimmät palauttaminen logiikan toimii.

1. Kun olet varmistanut, että sovelluksesi käyttää yllä olevan koodin, Avaa **seuraavan koodin korostetut** ja siirry **<issuerNameRegistry>** lohko, erityisesti Etsitkö seuraavat muutamalla:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. Valitse **<add thumbprint=””>** määrittäminen, muuttaminen allekirjoitusta korvaamalla merkin uudelleenkäyttöä. Tallenna **seuraavan koodin korostetut** .

3. Luo sovellus ja suorittamalla asennustiedosto. Kirjaudu sisään-prosessi voi suorittaa loppuun, jos sovelluksesi onnistuneesti päivittää käyttäjäavainten lataamalla oman directory federation metatietojen asiakirjasta tarvittavat tiedot. Jos sinulla on ongelmia kirjautumisessa, varmista sovelluksen muutokset ovat oikein lukemalla [Lisääminen kertakirjauksen käyttöön Web Application käyttäminen Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) -artikkeli tai lataamalla ja tarkistetaan seuraava koodi Esimerkki: [Usean vuokraajan Cloud sovelluksen Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).


### <a name="vs2010"></a>Web-sovellusten suojaaminen resursseja ja joka on luotu käyttämällä Visual Studio 2008: n tai 2010: n ja Windows Identity Foundation (WIF) v1.0 .NET 3.5 varten

Jos olet luonut WIF v1.0-sovellukseen, ei ole annettu järjestelmä päivittää automaattisesti uuden tunnuksen sovelluksen määrityksen.

- *Helpoin tapa* Käytä WIF SDK-paketissa, joka noutaa uusimmat metatietojen asiakirja ja Päivitä kokoonpanosi sisältyvät FedUtil sillä.
- Päivitä sovelluksesi .NET 4.5, joka sisältää järjestelmän nimitila sijaitsee WIF uusin versio. Voit käyttää suorittamiseen päivitykset automaattisesti sovelluksen määritys [Tarkistetaan myöntäjän nimi rekisterin (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) .
- Suorita manuaalinen palauttaminen ohjeita tämän artikkelin lopussa ohjeiden mukaisesti.

Ohjeita määritysten päivittämistä FedUtil avulla:

1. Varmista, että WIF v1.0 asennettu kehittäminen for Visual Studio 2008: n tai 2010: n SDK-paketissa. Voit [ladata sen täältä](https://www.microsoft.com/en-us/download/details.aspx?id=4451) , jos et ole vielä asentanut sitä.
2. Visual Studiossa Avaa ratkaisu ja sovellettavan projektin hiiren kakkospainikkeella ja valitse **Päivitä federation metatiedot**. Jos tämä vaihtoehto ei ole käytettävissä, FedUtil ja/tai WIF v1.0 SDK ei ole asennettu.
3. Valitse kehotettaessa **päivityksen** Aloita päivitys federation metatiedot. Jos käytössäsi on server-ympäristössä, jossa sovellus sijaitsee, voit halutessasi FedUtil's [Automaattinen metatietojen Päivitä ajoitus](https://msdn.microsoft.com/library/ee517272.aspx).
4. Valitse Suorita päivitys loppuun **Valmis** .

### <a name="other"></a>Web-sovellusten / API resursseja muiden kirjastojen avulla tai manuaalisesti käyttöönoton jokin seuraavista tuetuista protokollista suojaaminen

Jos käytössäsi on muussa kirjastossa tai toteutettu manuaalisesti jokin seuraavista tuetuista protokollista, ne on tarkistettava kirjaston tai käyttöympäristösi varmistaa, että avain haetaan OpenID yhteyden etsiminen asiakirjan tai federation metatietojen asiakirja. Voit tarkistaa tämän on koodin tai kirjaston koodin etsiä kaikki kutsut OpenID etsiminen asiakirjan tai federation metatietojen asiakirja.

Jos ne avaimen on on tallennettu jonnekin muualle tai englanninkielisissä sovelluksen, voit manuaalisesti noutaa avain ja päivitä se vastaavasti suorittaa manuaalinen palauttaminen ohjeita tämän artikkelin lopussa ohjeiden mukaisesti. **On erittäin suositeltavaa, että voit parantaa sovelluksen tukemaan automaattinen palauttaminen** käyttämällä mitä tahansa tavoista jäsentää tämän artikkelin tulevien häiriöiden välttämiseksi ja yleiskustannus, jos Azure AD kasvaa se on palauttaminen cadence tai on hätätilanteiden Luokittele-palauttaminen.

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Voit selvittää, jos se vaikuttaa sovelluksen testaaminen

Voit tarkistaa, onko sovellus tukee automaattinen avaimen palauttaminen lataamalla komentosarjat ja seuraamalla ohjeita [GitHub-tietovarasto.](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey)

## <a name="how-to-perform-a-manual-rollover-if-you-application-does-not-support-automatic-rollover"></a>Tietoja manuaalinen palauttaminen, jos olet sovellus ei tue automaattinen palauttaminen

Jos sovelluksen **tuki automaattinen palauttaminen** , sinun on muodostaa prosessin säännöllisesti valvoo allekirjoitetun Azure AD-näppäimiä ja suorittaa manuaalinen palauttaminen vastaavasti. [Tämä GitHub säilö](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) sisältää komentosarjojen ja ohjeet tähän.
