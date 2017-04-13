<properties 
    pageTitle="Widevine käyttöoikeuden mallin yleiskatsaus | Microsoft Azure" 
    description="Tässä ohjeaiheessa on yleiskuvaus Widevine käyttöoikeuden mallin, joka käyttää Widevine käyttöoikeuksien määrittämiseen." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Widevine käyttöoikeuden mallin yleiskatsaus

##<a name="overview"></a>Yleiskatsaus

Azure Media-palveluiden avulla voit määrittää ja pyytää Widevine käyttöoikeuksien nyt. Kun käyttäjä player yrittää Widevine suojatun sisällön toistaminen, pyyntö lähetetään käyttöoikeuden hankkiminen käyttöoikeuden toimitus-palveluun. Jos käyttöoikeus-palvelun hyväksyy pyynnön, joka lähetetään asiakkaan ja sitä voidaan käyttää salaus ja toistaa määritetyn sisällön käyttöoikeuden ongelmien vianmääritys.

Widevine käyttöoikeuden pyyntö on muotoiltu JSON-viestinä.  

Huomaa, että voit luoda tyhjän viestin arvoja vain "{}" käyttöoikeuden mallin luodaan kaikki oletusasetuksilla.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON-viesti

Nimi | Arvo | Kuvaus
---|---|---
tiedot |Base64 koodatun merkkijonon |Asiakkaan lähettämä käyttöoikeuden pyynnön. 
content_id | Base64 koodatun merkkijonon|Tunnus, jolla saada kunkin content_key_specs.track_type KeyId(s) ja sisällön avaimia.
Toimittaja |merkkijono |Käytetään Etsi sisällön näppäimet ja käytännöt. Pakollinen.
käytännön_nimi | merkkijono |Aiemmin rekisteröidyn käytännön nimi. Valinnainen
allowed_track_types | luettelo  | SD_ONLY tai SD_HD. Käyttöoikeuden sisällytetään ohjausobjektit, joiden sisällön näppäimet
content_key_specs | taulukko, JSON rakenteiden nähdä **Sisällön avain Etkö** alla | Tarkempaan martioitu ohjausobjektin palauttaa nuolinäppäimiä poistetusta sisällöstä. Sisällön avain määritys alla lisätietoja.  Vain yksi allowed_track_types ja content_key_specs voidaan määrittää. 
use_policy_overrides_exclusively | totuusarvo. TOSI tai EPÄTOSI | Käytä käytännön määritteet policy_overrides määrittämää ja jättää pois kaikki aiemmin tallennetut käytännön.
policy_overrides | JSON rakenne on artikkelissa **Käytäntö ohittaa** alla | Käytäntöasetukset käyttöoikeus.  Tapahtuma tämä resurssi on valmiiksi määritettyjä käytäntö, nämä määritettyjä arvoja käytetään. 
session_init | JSON rakenteen, katso alla **Istunnon alustaminen** | Valinnaiset tiedot siirretään käyttöoikeus.
parse_only | totuusarvo. TOSI tai EPÄTOSI | Käyttöoikeuden pyynnön jäsentää, mutta ei ole käyttöoikeus on annettu. Arvot, lomake, käyttöoikeuden pyynnön palautetaan vastauksessa.  

##<a name="content-key-specs"></a>Sisällön avaimen Etkö 

Jos käytännön luominen olemassa, ei ei tarvitse määrittää mitään arvoja sisällön avain-määritys.  Liittyvän sisällön olemassa käytännön käytetään määrittämään, kuten HDCP ja CGMS tulosteen-suojauksen.  Jos olemassa käytäntö ei ole rekisteröity Widevine käyttöoikeuspalvelimen, sisällöntuottaja Lisää arvot käyttöoikeuden kutsuun.   


Kaikki kappaleet, riippumatta siitä vaihtoehdon use_policy_overrides_exclusively on määritettävä jokaiselle content_key_specs. 


Nimi | Arvo | Kuvaus
---|---|---
content_key_specs. track_type | merkkijono | Seuraa tyyppinimi. Jos content_key_specs on määritetty käyttöoikeus-pyynnössä, varmista, että voit määrittää kaikki seurata erikseen. Voit tehdä tämän virheen aiheuttaa virheen toistamisen viimeiset 10 sekuntia. 
content_key_specs  <br/> security_level | uint32 | Määrittää asiakkaan vakauden vaatimukset toistoa varten. <br/> 1 - ohjelmisto-pohjainen whitebox crypto tarvitaan. <br/> 2 - ohjelmiston crypto ja obfuscated koodauksen tarvitaan. <br/> 3-avaimen materiaalin ja salauksen toimintojen on suoritettava laitteiston varmuuskopioidut luotettujen suorittamisen ympäristössä. <br/> 4-crypto ja koodauksen sisällöstä on suoritettava laitteiston varmuuskopioidut luotettujen suorittamisen ympäristössä.  <br/> 5 - siitä salauksen, koodauksen ja kaikki käsittely median (pakattu ja puretaan) on käsiteltävä laitteiston varmuuskopioidut luotettujen suorittamisen ympäristössä.  
content_key_specs <br/> required_output_protection.hDC | merkkijono - jokin seuraavista: HDCP_NONE, HDCP_V1, HDCP_V2 | Ilmaisee, onko HDCP edellyttävät
content_key_specs <br/>avain | Base64 <br/>koodatun merkkijonon|Raita käytettävän sisällön avain. Jos määritetty, track_type tai key_id on pakollinen.  Tämän asetuksen avulla sisältötoimittajan lisätäkseen sisällön näppäintä raita sijaan auttaa muita Widevine käyttöoikeuspalvelimen Luo tai haku-näppäintä.
content_key_specs.key_id| Base64 koodatun merkkijonon binaarinen, 16 tavua | Yksilöllinen tunnus näppäintä. 


##<a name="policy-overrides"></a>Käytäntö korvaa 

Nimi | Arvo | Kuvaus
---|---|---
policy_overrides. can_play | totuusarvo. TOSI tai EPÄTOSI | Ilmaisee, että toisto sisällöstä on sallittu. Oletusarvo on EPÄTOSI.
policy_overrides. can_persist | totuusarvo. TOSI tai EPÄTOSI |Ilmaisee, että käyttöoikeuden voi käyttää säilyvän offline-käyttöä varten. Oletusarvo on EPÄTOSI.
policy_overrides. can_renew | totuusarvo TOSI tai EPÄTOSI |Osoittaa, että uusiminen käyttöoikeus. Jos arvo on TOSI, käyttöoikeuden kestoa voidaan laajentaa uudelleenaktivoinnin yhteydessä. Oletusarvo on EPÄTOSI. 
policy_overrides. license_duration_seconds | Int64 | Osoittaa Aikaikkuna, jossa tietyn käyttöoikeus. Arvo 0 ilmaisee, että ei ole rajoitettu kestoa. Oletusarvo on 0. 
policy_overrides. rental_duration_seconds | Int64 | Osoittaa aika-ikkuna, kun toisto on sallittu. Arvo 0 ilmaisee, että ei ole rajoitettu kestoa. Oletusarvo on 0. 
policy_overrides. playback_duration_seconds | Int64 | Aika, kun toisto alkaa käyttöoikeuden ajan kuluessa tarkastelu-ikkuna. Arvo 0 ilmaisee, että ei ole rajoitettu kestoa. Oletusarvo on 0. 
policy_overrides. renewal_server_url |merkkijono | Kaikki tämän käyttöoikeuden uudelleenaktivoinnin yhteydessä (uusimisen) pyynnöt suunnataan määritettyyn URL-osoitteeseen. Tätä kenttää käytetään ainoastaan, jos can_renew on TOSI.
policy_overrides. renewal_delay_seconds |Int64 |Kuinka monta sekuntia license_start_time, ennen kuin uusimisen yritetään jälkeen. Tätä kenttää käytetään ainoastaan, jos can_renew on TOSI. Oletusarvo on 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Määrittää viive sekunteina seuraavien käyttöoikeuksien uusiminen pyyntöjen Jos virheen välillä. Tätä kenttää käytetään ainoastaan, jos can_renew on TOSI. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Aika, joka toisto voi jatkaa uusimisen aikana ikkuna on yritettiin, vielä epäonnistua käyttöoikeuspalvelimen Taustajärjestelmä ongelmia. Arvo 0 ilmaisee, että ei ole rajoitettu kestoa. Tätä kenttää käytetään ainoastaan, jos can_renew on TOSI.
policy_overrides. renew_with_usage | totuusarvo TOSI tai EPÄTOSI |Ilmaisee, että käyttöoikeuden lähetetään uusimista Jos käyttö aloitetaan. Tätä kenttää käytetään ainoastaan, jos can_renew on TOSI. 

##<a name="session-initialization"></a>Istunnon alustaminen

Nimi | Arvo | Kuvaus
---|---|---
provider_session_token | Base64 koodatun merkkijonon |Istunnon tunnuksessa on välitetty takaisin käyttöoikeus ja ovat olemassa myöhemmin uusintoja.  Istunnon tunnuksen ei voimassa lisäksi istuntoja. 
provider_client_token | Base64 koodatun merkkijonon | Asiakkaan tunnuksen takaisin käyttöoikeuden vastauksen lähettäminen.  Jos käyttöoikeuden-kokouspyyntö sisältää asiakas-tunnuksen, tätä arvoa oteta huomioon. Asiakas-tunnuksen voimassa lisäksi käyttöoikeuden istuntoja.
override_provider_client_token | totuusarvo. TOSI tai EPÄTOSI |Jos EPÄTOSI ja käyttöoikeuden-kokouspyyntö on asiakas-tunnuksen, käytä pyynnöstä tunnuksen, vaikka tämä rakenne on määritetty asiakas-tunnuksen.  Jos arvo on TOSI, käytä aina tämän rakenteen määritetty tunnus.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Määritä Widevine käyttöoikeuksia käyttämällä .NET-tyypit

Media-palvelut on .NET-ohjelmointirajapinnan, joiden avulla voit määrittää Widevine käyttöoikeudet. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Luokkien määritysten mukaisesti Media Services .NET SDK

Seuraavassa ovat määritelmien näitä tiedostotyyppejä.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Esimerkki

Seuraavassa esimerkissä, .NET-ohjelmointirajapinnan käyttäminen yksinkertainen Widevine käyttöoikeuden määrittäminen.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Katso myös

[Käyttää PlayReady ja/tai Widevine dynaaminen yleisiä salausta](media-services-protect-with-drm.md)
