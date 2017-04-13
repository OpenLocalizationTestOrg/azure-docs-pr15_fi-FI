<properties 
    pageTitle="Axinom avulla voit pitää Widevine käyttöoikeuksien Azure Media Services | Microsoft Azure" 
    description="Tässä artikkelissa kuvataan, miten voit Azure Media Services (AMS) aikana muodossa, joka on dynaamisesti salattu AMS PlayReady ja Widevine DRMs. PlayReady käyttöoikeuden tulee Media Services PlayReady käyttöoikeuspalvelimen ja Widevine käyttöoikeuden toimitetaan Axinom käyttöoikeuspalvelimen mukaan." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Axinom avulla voit pitää Widevine käyttöoikeuksien Azure Media-palveluita  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Yleiskatsaus

Azure Media Services (AMS) on lisätty Google Widevine dynaaminen protection (Katso lisätietoja [Mingfei's blogi](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) ). Lisäksi Azure Media Playerin (AMP) on lisännyt Widevine-tuki (Katso lisätietoja [AMP asiakirja](http://amp.azure.net/libs/amp/latest/docs/) ). Tämä on pää saavutus-tietovirta VIIVA sisällön CENC Avainhankkeiden monivaiheisen-native-DRM (PlayReady ja Widevine) ja suojattu äänikortti MSE ja EME Moderni selaimissa.

Media Services .NET SDK versio 3.5.2 alkaen Media Services-palvelujen avulla voit määrittää Widevine käyttöoikeuden mallin ja saada Widevine käyttöoikeudet. Voit käyttää myös seuraavat AMS kumppanien avulla voit pitää Widevine käyttöoikeudet: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

Tässä artikkelissa käsitellään integroida ja testaa Widevine käyttöoikeuspalvelimen hallitsee Axinom. Tarkemmin sanottuna se peittää:  

- Usean-DRM (PlayReady ja Widevine) vastaavan käyttöoikeuden hankkiminen-URL-osoitteisiin; dynaamisen yhteistä salauksen määrittäminen
- Luodaan JWT tunnuksen täyttämiseksi palvelimen käyttöoikeusvaatimukset;
- Kehittäminen Azure Media Player-sovellus, joka käsittelee hankkimista JWT suojaustunnuksen todennusta;

Koko järjestelmän ja sisällön avaimen, avaimen tunnus avaimen lähde, JTW tunnuksen ja sen vaateita, jotka voidaan parhaiten kuvata seuraavassa kaaviossa etenemisen.

![VIIVA- ja CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Sisällön suojaus

Dynaaminen protection ja avaimen toimituksen käytännön määrittämiseen Katso Mingfei's blogi: [määrittäminen Widevine pakkaus Azure Media-palvelujen kanssa](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Voit määrittää dynaamisen CENC suojaus ja usean DRM VIIVA streaming ottaa molemmat seuraavista toimista:

1. MS Edge ja IE11, joka voi olla suojaustunnuksen käyttöoikeuksien rajoitukset PlayReady suojaus. Suojaustunnuksen rajoitettu käytäntö on liitettävä mukaan suojattu suojaustunnuksen Service (STS), Azure Active Directory, kuten annettu tunnus
1. Widevine suojaus Chrome-se edellyttää todennusta suojaustunnuksen toiseen STS myöntämä tunnuksen kanssa. 

[JWT suojaustunnuksen luonti](media-services-axinom-integration.md#jwt-token-generation) on artikkelissa miksi Azure Active Directory ei voi käyttää STS Axinom's Widevine käyttöoikeuden Server-osassa.

###<a name="considerations"></a>Huomioon otettavia seikkoja

1. Sinun on käytettävä määritetty Axinom avaimen lähde (8888000000000000000000000000000000000000) ja luotu tai valitun tunnusta avaimen toimitus-palvelun määrittäminen sisällön näppäintä luomiseen. Axinom käyttöoikeuspalvelimen antaa kaikki käyttöoikeudet, joka sisältää sisällön näppäimet saman avaimen lähde, joka on voimassa testaus ja tuotannon perusteella.
1. Widevine käyttöoikeuden hankkiminen URL-Osoitteen testikäyttöön: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP- ja HTTS sallitaan.

##<a name="azure-media-player-preparation"></a>Azure Media Playerin valmistelu

AMP v1.4.0 tukee AMS sisältöön, joka on pakattu dynaamisesti PlayReady ja Widevine DRM toiston.
Jos Widevine käyttöoikeuspalvelimen ei edellytä suojaustunnuksen todennusta, ei ole mitään muita sinun on suoritettava Testaa suojattu Widevine VIIVA-sisältöä. Esimerkki AMP ryhmän on yksinkertainen [malli](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), näet sen reunaa ja IE11 PlayReady kanssa ja Chrome Widevine kanssa.
Axinom myöntämä Widevine käyttöoikeuspalvelimen edellyttää JWT suojaustunnuksen todennusta. JWT tunnus on lähetetty käyttöoikeuden pyynnön kautta HTTP-otsikon "X-AxDRM-sanoma" kanssa. Tätä varten sinun on lisättävä seuraavat javascript isännöinnin AMP ennen kuin määrität lähteen web-sivulle:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Loput AMP koodi on vakio AMP API AMP asiakirjassa [tähän](http://amp.azure.net/libs/amp/latest/docs/).

Huomaa, että asetus mukautetun luvan otsikon yllä javascript on edelleen lyhyen aikavälin-menetelmän virallinen pitkällä aikavälillä lähestymistavan AMP on julkaistu ennen.

##<a name="jwt-token-generation"></a>JWT tunnuksen luominen

Axinom Widevine käyttöoikeuspalvelimen testikäyttöön edellyttää JWT suojaustunnuksen todennusta. Lisäksi jokin JWT tunnuksen saatavat on monimutkaisia objektityypin alkeistyyppi tietotyyppi sijaan.

Azure AD voi valitettavasti myöntää JWT tunnuksia vain primitiivityyppejä kanssa. Vastaavasti .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler ja JwtPayload) vain antaa syötteen monimutkaisia objektityyppi saatavat nimellä. Kuitenkin saatavat ovat edelleen muuntaa sarjaksi merkkijonona. Näin ollen on ei voi käyttää mitä tahansa kahden luonnissa Widevine käyttöoikeuden pyynnön JWT tunnus.


Teemu Sheehan [JWT Nuget paketti](https://www.nuget.org/packages/JWT) täyttää tarpeet niin Nuget tämän pakkauksen tarkastellaan.

Alla on luotaessa JWT tunnuksen tarpeen mukaan vaatimusten mukainen Axinom Widevine käyttöoikeuspalvelimen testikäyttöön koodi:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine käyttöoikeuspalvelimen

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Huomioon otettavia seikkoja

1.  Vaikka AMS PlayReady käyttöoikeuden toimitus-palvelu edellyttää "haltijan =" edeltävän todennus-tunnuksen Axinom Widevine käyttöoikeuspalvelimen ei käytä sitä.
2.  Kirjautuminen avain käytetään Axinom viestintä-näppäintä. Huomaa, että avain on hex merkkijono, mutta se on käsiteltävä tavua ei merkkijonon sarjana kun koodaus. Tämä saavutetaan ConvertHexStringToByteArray-menetelmällä.

##<a name="retrieving-key-id"></a>Haetaan Key-tunnus

Olet ehkä jo huomannut luonnissa JWT koodissa tunnuksen, avaimen tunnus on pakollinen. Koska JWT tunnuksen on oltava valmiina ennen ladataan AMP Playerin avaimen tunnus on noudetaan, jotta voit luoda JWT tunnuksen.

Kurssin on useita tapoja Hae pitoon avaimen tunnus. Esimerkiksi jokin voi tallentaa tunnusta ja sisällön metatietojen tietokannassa. Tai voit hakea tunnusta MPD YHDYSMERKIN (Media esityksen kuvaus)-tiedostosta. Seuraava koodi on viimeksi.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Yhteenveto

Widevine tuki Azure Media sisällönsuojaus- ja Azure Media Playerin uusimman lisäksi kanssa emme voi ottaa käyttöön streaming VIIVA + Avainhankkeiden monivaiheisen-native-DRM (PlayReady + Widevine) sekä PlayReady käyttöoikeuden palveluun AMS ja Widevine käyttöoikeuden Server-Axinom, Nykyaikainen seuraavissa selaimissa:

- Chrome
- Windows 10: n Microsoft Edge
- Internet Explorer 11 Windows 8.1 ja Windows 10: ssä
- Firefox (Työpöytä) ja Safarin Mac (ei iOS) tuetaan myös Silverlightin ja Azure Media Playerin saman URL-Osoitteen kautta

Seuraavat parametrit on käytettävä pienen ratkaisu virtuaalisten Axinom Widevine käyttöoikeuspalvelimen. Lukuun ottamatta avaimen tunnus, loput parametrit toimitetaan Axinom niiden Widevine palvelinasetukset perusteella.


Parametri|Miten sitä käytetään
---|---
Viestintä tunnusta|Varaa "com_key_id"-JWT tunnuksen arvona sisällytettävä (katso [tämän](media-services-axinom-integration.md#jwt-token-generation) osion).
Viestintä-näppäin|Käytettävä JWT tunnuksen allekirjoitetun avaimeksi (katso [tämän](media-services-axinom-integration.md#jwt-token-generation) osion).
Avaimen lähde|Luo sisällön avain jonkin tietyn sisällön kanssa käytettävä tunnusta (katso [tämän](media-services-axinom-integration.md#content-protection) osion).
Widevine käyttöoikeuden hankkiminen URL-osoite|On käytettävä resurssi toimituksen käytännön määrittäminen VIIVA streaming (katso [tämän](media-services-axinom-integration.md#content-protection) osion).
Sisällön avaimen tunnus|On oltava oikeuden viestin vaatimus JWT tunnuksen arvo sisältää (katso [tämän](media-services-axinom-integration.md#jwt-token-generation) osion). 


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Vahvistus 

Haluamme toki seuraavat henkilöt, jotka on lähettänyt kohti tämän asiakirjan luominen: Kristjan Jõgi, Axinom, Mingfei Yan ja Amit Rajput.
