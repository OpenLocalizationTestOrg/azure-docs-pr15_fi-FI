<properties 
    pageTitle="Azure Media-palvelujen yleiskatsaus ja yleisiä tilanteita, joissa | Microsoft Azure" 
    description="Tässä artikkelissa on yleiskatsaus Azure Media Services" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Azure Media-palvelujen yleiskatsaus ja yleisiä tilanteita, joissa

Microsoft Azure Media Services on extensible pilvipohjaisessa ympäristössä, joka kehittäjät voivat luoda skaalattava median hallinta- ja sovelluksia. Media-palveluiden perustuu REST API, joiden avulla turvallisesti ladata, tallentaa, koodata ja pakata videon tai äänen sisältöä sekä tarvittaessa ja live eri asiakkaiden (esimerkiksi TV, PC ja mobiililaitteiden) streaming toimittamista varten.

Voit luoda täysin Media-palveluita käyttämällä lopusta loppuun-työnkulut. Voit valita myös kolmannen osapuolen osien käytettävät työnkulun joitakin osia. Esimerkiksi salata, kolmannen osapuolen encoder avulla. Lataa, suojaa, pakata, pitää Media-palveluiden avulla.

Voit valita stream sisällön live tai toimittaa sisällön pyydettäessä. Tässä ohjeaiheessa esitellään yleisiä tilanteita, joissa oman sisällön [live](media-services-overview.md#live_scenarios) tai [pyydettäessä](media-services-overview.md#vod_scenarios). Aihe on myös linkki muiden aiheissa.

## <a name="sdks-and-tools"></a>SDK: T ja työkalut

Jos haluat luoda Media Services-ratkaisuja, voit:

- [Media-palveluiden REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Jokin käytettävissä asiakkaan SDK: T:
- [Azure Media-palveluiden .NET SDK](https://github.com/Azure/azure-sdk-for-media-services)
- [Java azure SDK](https://github.com/Azure/azure-sdk-for-java)
- [Azure PHP SDK-paketissa](https://github.com/Azure/azure-sdk-for-php)
- [Azure Node.js mediapalvelut](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (Tämä on-Microsoft Node.js SDK-versio. Se yhteisön ylläpitämä ja tällä hetkellä ole AMS-ohjelmointirajapinnan 100 %: n soveltamisala).
- Olemassa olevat työkalut:
- [Azure portal](https://portal.azure.com/)
- [Azure-Media-palvelut-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) on Winforms ja C#-sovellus Windows)

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

Voit tarkastella AMS oppimispolut tähän:

- [AMS Live Streaming työnkulku](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS Streaming työnkulun pyydettäessä](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Edellytykset

Voit käyttää Azure Media Services on oltava seuraavasti:

3. Azure tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com).
2. Azure Media Services-tili. Azure-portaalissa, .NET tai REST API avulla voit luoda Azure Media Services-tilin. Lisätietoja on artikkelissa [Luo tili](media-services-portal-create-account.md).
3. (Valinnainen) Määritä kehitysympäristö. Valitse .NET tai REST API kehittäminen-ympäristössä. Lisätietoja [ympäristön määrittäminen](media-services-dotnet-how-to-use.md).

Lisäksi opettele muodostamaan [yhteyden](media-services-dotnet-connect-programmatically.md)ohjelmallisesti.
4. (Suositus) Kohdista vähintään yksi aikayksikön. On suositeltavaa varata vähintään yksi aikayksikön tuotantoympäristössä sovellukset.   Lisätietoja on artikkelissa [hallinta streaming päätepisteet](media-services-portal-manage-streaming-endpoints.md).

##<a name="concepts-and-overview"></a>Käsitteitä ja yleiskatsaus

Katso Azure Media Services käsitteitä [käsitteitä](media-services-concepts.md).

Katso toimintaohjeet sarjan, jossa esitellään tärkeimmät osat Azure Media Services, [Azure Media Services Step-by-Step opetusohjelmat](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). Sarjassa on hyvä yleiskatsaus käsitteitä ja se käyttää AMSE-työkalun osoittamaan AMS tehtävät. Huomaa, että AMSE työkalu Windows-työkalu. Tämä työkalu tukee Useimmat tehtävät, voit toteuttaa ohjelmallisesti [AMS SDK.NET](https://github.com/Azure/azure-sdk-for-media-services), [Azure SDK Java](https://github.com/Azure/azure-sdk-for-java)tai [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php).

##<a id="vod_scenarios"></a>Välittää Media tarvittaessa Azure Media-palvelujen kanssa: Yleisiä tilanteita, joissa ja tehtävät

Tässä osassa kuvataan Yleisiä tilanteita, joissa ja linkkejä aiheissa. Seuraavassa kaaviossa näkyy, että ne Media Services-ympäristön tärkeimmät osat välittää sisällön pyydettäessä. 

![VoD työnkulku](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Tallennustilan sisällön suojaaminen ja poista streaming mediatiedostojen toimita (muut kuin salatut)

1. Laadukkaat mezzanine tiedoston lataaminen sijoituksen kyselyjä.
    
    On suositeltavaa käyttää tallennuspaikka ja salauksen oman resurssi suojaamiseksi sisällön lataamisen aikana ja muut tallennustilaan osoitteessa.
 
1. Koodata säädön bittitaajuus MP4 tiedostojoukon avulla. 

    On suositeltavaa käyttää tallennuspaikka ja salauksen tulosteen kohteiden suojaamiseksi sisältöä rest-palvelussa.
    
1. Määritä resurssi toimituksen käytäntö (käyttämä Dynaaminen pakkaus). 
    
    Jos että resurssi on salattu tallennustilan, sinun **täytyy** Määritä resurssi toimituksen käytäntö. 

1. Julkaista kohteen luomalla OnDemand locator.

    Varmista, että ainakin yksi streaming varattu yksikön streaming päätepisteen, josta haluat sisällön.

1. Virta julkaista sisältöä.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Suojata sisältöä tallennustilaan, pitää dynaamisesti salattuja streaming media  

Jos dynaaminen salausta, sinun täytyy saada vähintään yhden streaming varattu yksikön streaming päätepisteen, josta haluat lähettää salatun sisältöä.

1. Laadukkaat mezzanine tiedoston lataaminen sijoituksen kyselyjä. Käytä kohteen tallennuspaikka ja salausta.
1. Koodata säädön bittitaajuus MP4 tiedostojoukon avulla. Käytä tallennuspaikka ja salauksen tulosteen resurssi.
1. Luo sisällön salausavaimen, jonka haluat salata dynaamisesti toiston aikana annetaan.
2. Sisällön avaimen luvan käytännön määrittäminen.
1. Määritä resurssi toimituksen käytäntö (käyttämä Dynaaminen pakkaus ja dynaaminen salauksen).
1. Julkaista kohteen luomalla OnDemand locator.
1. Virta julkaista sisältöä. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Yhtenäisen havainnollistamisen johdettu videoita Media-analyysin avulla 

Media Analytics on kokoelma, puhe ja vision osat, jotka helpottavat organisaatiot ja yritykset saada suoritettavia havainnollistamisen niiden videon tiedostoista. Lisätietoja on artikkelissa [Azure Media Services Analytics yleiskatsaus](media-services-analytics-overview.md).

1. Laadukkaat mezzanine tiedoston lataaminen sijoituksen kyselyjä.
2. Jokin seuraavista palveluista Media Analytics avulla voit käsitellä videot:
    
    - **Indeksointitoiminto** – [prosessin videot Azure Media indeksointitoiminto 2](media-services-process-content-with-indexer2.md)
    - **Hyperlapse** – [Hyperlapse mediatiedostojen Azure Media Hyperlapse kanssa](media-services-hyperlapse-content.md)
    - **Liikeradan tunnistaminen** – [Azure Media Analytics liikerata tunnistamista](media-services-motion-detection.md).
    - **Hymiö tunnistaminen ja hymiö tunteet** – [hymiö ja Azure Media Analytics tunteet tunnistamista](media-services-face-and-emotion-detection.md).
    - **Videon yhteenveto** – [Käytä Azure Media Video pikkukuvat Video yhteenvedon luominen](media-services-video-summarization.md)
3. Media Analytics media suorittimien tuottaa MP4-tiedostoja tai JSON-tiedostoja. Jos media-suoritin MP4-tiedoston, voit ladata tiedoston asteittain. Jos media-suoritin JSON-tiedoston, tiedosto voi ladata Azure-blob-säiliö. 


###<a name="deliver-progressive-download"></a>Nousevan lataamisen aikana 

1. Laadukkaat mezzanine tiedoston lataaminen sijoituksen kyselyjä.
1. Koodata yhteen MP4-tiedostoon.
1. Julkaista kohteen luomalla OnDemand tai SAS locator.

    Jos OnDemand locator, varmista, että ainakin yksi streaming varattu yksikön streaming päätepisteen, jonka aiot ladata asteittain sisältöä.

    Jos käytät SAS locator, sisällön ladataan Azure-blob-säiliö. Tässä tapauksessa sinun ei tarvitse on streaming varatut resurssit.
  
1. Lataa asteittain sisältö.

##<a id="live_scenarios"></a>Välittää Streaming tapahtumia Azure Media-palvelujen kanssa

Kun käsittelet Live toistamisen yleisesti sisältyy seuraavat osat:

- Kamera, jota käytetään yleislähetys tapahtuman.
- Live videon encoder, joka muuntaa signaalit kamerasta virtaa, joka on tullut live streaming-palvelu.

Voit myös useita live aika synkronoitu koodauslaitteista. Tiettyjen kriittinen live tapahtumat, että tarvittaessa erittäin suuri käytettävyys ja käyttäjäkokemuksen laatuun, on suositeltavaa käyttää aktiivinen aktiivinen tarpeettomat koodauslaitteista synchronizationto saavuttamiseksi tiedot ilman yksityiskohtien saumaton automaattisesti ajan.
- Live streaming palvelu, jonka avulla voit tehdä seuraavat toimenpiteet:

- ingest live-sisällön liittämiseen eri live protokollista (esimerkiksi RTMP tai tasainen Streaming)
- (valinnainen) koodata oman stream mukautuvat bittitaajuus stream tuominen
- oman live stream esikatselu
- tallentaminen ja tallentaa nieltäväksi sisältöä, jotta voidaan lähettää myöhemmin (videotilauspalveluiden)
- Toimita yleisiä protokollista (kuten MPEG VIIVA, tasainen, HLS, HDS) – sisältöä suoraan asiakkaille, tai voit sisältöä toimituksen verkon (CDN) edelleen.


**Microsoft Azure Media-palveluita** (AMS) mahdollistaa ingest, koodata, esikatsella, tallentaa ja pitää live streaming sisältöä.

Pitäessäsi sisältöä asiakkaille lähinnä aikana laadukkaita videon verkon eri olosuhteissa eri laitteisiin. Huolehtia laatu-ja verkko-että usean bittitaajuus (säädön bittitaajuus) videovirtaa virta koodata live koodauslaitteista avulla.  Huolehtia streaming eri laitteissa, Media Services [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md) avulla pakata oman toiseen protokollat virta dynaamisesti uudelleen. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming technologies lähettämisen: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja).

Azure Media Services, **kanavat**, **ohjelmien**ja **StreamingEndpoints** käsittelevät kaikki live streaming toimintoja myös ingest, muotoilu, DVR, tietoturva, skaalattavuus ja redundanssin.

**Kanavan** edustaa putkijohto streaming sisällön käsittelyyn. Kanavan saavat reaaliaikaisia syötetty virtaa seuraavilla tavoilla:

- Paikallisen live encoder lähettää usean bittitaajuus **RTMP** tai **Tasainen Streaming** (pirstoutuneet MP4) kanavaan, joka on määritetty **läpivienti** toimittamista varten. **Läpivientikysely** toimituksen on, kun nieltäväksi virtaa kulkevat **kanavan**s ilman mitään lisäkäsittelyä. Voit käyttää seuraavia live koodauslaitteista, joka siirtää usean bittitaajuus tasainen Streaming: alkuaine, Envivion, Cisco.  Seuraavat live koodauslaitteista tulosteen RTMP: Adobe Flash Live, Telestream Wirecast ja Tricaster transcoders.  Live encoder voit myös lähettää yhden bittitaajuus stream kanavaan, joka ei ole otettu käyttöön live koodaus, mutta, joka ei ole suositeltavaa. Kun pyydetty, Media Services toimittaa virta asiakkaille.

>[AZURE.NOTE] Läpivienti menetelmällä on eniten taloudellisia live streaming, kun teet useita tapahtumien pitkän ajanjakson aikana ja paikallisen koodauslaitteista jo sijoitetaan. Katso [hinnat](/pricing/details/media-services/) tiedot.

- Live paikallisen-encoder lähettää yhden bittitaajuus stream kanavaa, jotka on otettu käyttöön suorittamiseen live-koodaus Media Services jossakin seuraavista muodoista: RTP (MPEG-TS), RTMP tai tasainen Streaming (pirstoutuneet MP4). Kanavan suorittaa sitten live koodaus saapuvien yksittäisen bittitaajuus virta usean-bittitaajuus (mukautuvat) videovirtaa. Kun pyydetty, Media Services toimittaa virta asiakkaille.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Kanavien, joka vastaanottaa usean bittitaajuus live stream paikallisen koodauslaitteista (läpivienti) käyttäminen

Seuraavassa kaaviossa näkyvät AMS ympäristön tärkeimmät osat, jotka liittyvät **läpivienti** -työnkulussa.

![Live työnkulku][live-overview2]

Lisätietoja on artikkelissa [käsitteleminen kanavat, vastaanota usean-bittitaajuus Live virta-paikallisen koodauslaitteista](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Jotka voivat suorittaa Azure Media-palvelujen kanssa live koodaus kanavien käyttäminen

Seuraavassa kaaviossa näkyvät AMS ympäristön tärkeimmät osat, jotka liittyvät Live Streaming työnkulun, jossa kanavan on käytössä suorittamiseen live koodaus Media-palvelujen kanssa.

![Live työnkulku][live-overview1]

Lisätietoja on artikkelissa [käyttäminen kanavat, jotka ovat käytössä suorittaa Live koodaus Azure Media-palvelujen kanssa](media-services-manage-live-encoder-enabled-channels.md).


##<a name="consuming-content"></a>Muissa sisältö

Azure Media-palvelut on työkaluja, sinun on luotava dynaamisten Playerin asiakassovelluksissa useimmat ympäristöjen, mukaan lukien: iOS-laitteita, Android-laitteille, Windowsin, Windows Phone-, Xbox ja TV-ruutuihin. Seuraavassa aiheessa on linkkejä SDK: T ja Playerin kehyksiä, jotka voit kehittää oman asiakas-sovelluksia, jotka voivat käyttää streaming mediatiedostoja, kuten Media-palveluita.

[Videosoittimen sovellusten kehittäminen](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Azure CDN ottaminen käyttöön

Media-palveluiden tukee Azure CDN integrointi. Lisätietoja siitä, miten voit ottaa käyttöön Azure CDN, katso, [miten voit hallita Streaming päätepisteet Media Services-tilin](media-services-portal-manage-streaming-endpoints.md).

##<a name="scaling-a-media-services-account"></a>Media Services-tilin skaalaus

**Media-palveluiden** skaalata määrittämällä **Streaming varatut resurssit** ja **Koodaus varatut resurssit** , jotka haluat tilisi valmisteltu kanssa.

Voit myös skaalata Media Services-tilin lisäämällä tallennustilan tilit. Tallennustilan tileille on rajoitettu 500 Teratavua. Laajenna tallennustilan lisäksi oletusarvo-rajoitukset, voit liittää useita tallennustilan tilejä Media Services-tilistä.

[Tässä](media-services-portal-scale-streaming-endpoints.md) aiheessa on linkkejä aiheissa.

##<a name="support"></a>Tuki

[Azure tuki](https://azure.microsoft.com/support/options/) on Azure, kuten Media Services tukivaihtoehdoista.

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Tason palvelusopimus (SLA)

- Saat Media-palveluiden koodaus takaa 99,9 % saatavuuden REST API tapahtumat.
- Varten virtautetun median, emme onnistuneesti palvelupyynnöt 99,9 % käytettävyys taattua aiemman mediasisällön kanssa kun vähintään yhden Streaming yksikön ostetaan.
- Live kanavien takaa, että käytössä kanavien on ulkoinen yhteys vähintään 99,9 % aika.
- Sisällön suojauksen takaa, että olemme onnistuneesti täyttävät avaimen pyynnöt vähintään 99,9 % ajan.
- Varten indeksointitoiminto, emme onnistuneesti palvelupyynnöt indeksointitoiminto tehtävän käsitellään koodaus varattu aika yksikön 99,9 %.

Lisätietoja on artikkelissa [Microsoft Azure SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
