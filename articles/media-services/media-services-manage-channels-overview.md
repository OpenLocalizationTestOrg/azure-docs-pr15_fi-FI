<properties 
    pageTitle="Yleistä Live höyryssä Azure Media-palveluiden avulla | Microsoft Azure" 
    description="Tässä ohjeaiheessa on yleiskuvaus, live höyryssä Azure Media-palveluiden avulla." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Yleistä Live höyryssä Azure Media-palvelujen avulla

##<a name="overview"></a>Yleiskatsaus

Pitäessäsi streaming tapahtumia Azure Media-palvelujen kanssa yleisesti sisältyy seuraavat osat:

- Kamera, jota käytetään yleislähetys tapahtuman.
- Live videon encoder, joka muuntaa signaalit kamerasta virtaa, joka on tullut live streaming-palvelu.

    Voit myös useita live aika synkronoitu koodauslaitteista. Tiettyjen kriittinen live tapahtumat, että tarvittaessa erittäin suuri käytettävyys ja käyttäjäkokemuksen laatuun, on suositeltavaa käyttää aktiivinen aktiivinen tarpeettomat koodauslaitteista aika synkronoinnissa saavuttamiseksi tiedot ilman yksityiskohtien saumaton automaattisesti.
- Live streaming palvelu, jonka avulla voit tehdä seuraavat toimenpiteet:
    
    - ingest live-sisällön liittämiseen eri live protokollista (esimerkiksi RTMP tai tasainen Streaming)
    - (valinnainen) koodata oman stream mukautuvat bittitaajuus stream tuominen
    - oman live stream esikatselu
    - tallentaminen ja tallentaa nieltäväksi sisältöä, jotta voidaan lähettää myöhemmin (videotilauspalveluiden)
    - Toimita yleisiä protokollista (kuten MPEG VIIVA, tasainen, HLS, HDS) – sisältöä suoraan asiakkaille, tai voit sisältöä toimituksen verkon (CDN) edelleen.


**Microsoft Azure Media-palveluita** (AMS) mahdollistaa ingest, koodata, esikatsella, tallentaa ja pitää live streaming sisältöä.

Pitäessäsi sisältöä asiakkaille lähinnä aikana laadukkaita videon verkon eri olosuhteissa eri laitteisiin. Tätä koodata oman usean bittitaajuus (mukautuvat bittitaajuus) videovirtaa virta live koodauslaitteista avulla.  Huolehtia streaming eri laitteissa, Media Services [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md) avulla pakata oman toiseen protokollat virta dynaamisesti uudelleen. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming technologies lähettämisen: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja).

Azure Media Services, **kanavat**, **ohjelmien**ja **StreamingEndpoints** käsittelevät kaikki live streaming toimintoja myös ingest, muotoilu, DVR, tietoturva, skaalattavuus ja redundanssin.

**Kanavan** edustaa putkijohto streaming sisällön käsittelyyn. Kanavan saavat reaaliaikaisia syötetty virtaa seuraavilla tavoilla:

- Paikallisen live encoder lähettää usean bittitaajuus **RTMP** tai **Tasainen Streaming** (pirstoutuneet MP4) kanavaan, joka on määritetty **läpivienti** toimittamista varten. **Läpivientikysely** toimituksen on, kun nieltäväksi virtaa kulkevat **kanavan**s ilman mitään lisäkäsittelyä. Voit käyttää seuraavia live koodauslaitteista, joka siirtää usean bittitaajuus tasainen Streaming: alkuaine, Envivion, Cisco.  Seuraavat live koodauslaitteista tulosteen RTMP: Adobe Flash Media Live Encoder (FMLE), Telestream Wirecast ja Tricaster transcoders.  Live encoder voit myös lähettää yhden bittitaajuus stream kanavaan, joka ei ole otettu käyttöön live koodaus, mutta, joka ei ole suositeltavaa. Kun pyydetty, Media Services toimittaa virta asiakkaille.

    >[AZURE.NOTE] Läpivienti menetelmällä on eniten taloudellisia live streaming, kun teet useita tapahtumien pitkän ajanjakson aikana ja paikallisen koodauslaitteista jo sijoitetaan. Katso [hinnat](/pricing/details/media-services/) tiedot.
    
    
- Live paikallisen-encoder lähettää yhden bittitaajuus stream kanavaa, jotka on otettu käyttöön suorittavan live-koodaus Media Services jossakin seuraavista muodoista: RTMP tai tasainen Streaming (pirstoutuneet MP4). RTP (MPEG-TS) tuetaan myös, jos sinulla on oma yhteys Azure tietokeskuksen. Seuraavat live koodauslaitteista RTMP tulosteen kanssa tunnetusti tällaista kanavien käyttäminen: Telestream Wirecast, FMLE. Kanavan suorittaa sitten live koodaus saapuvien yksittäisen bittitaajuus virta usean-bittitaajuus (mukautuvat) videovirtaa. Kun pyydetty, Media Services toimittaa virta asiakkaille.


Media Services 2.10-versiosta lähtien kun kanava luodaan, voit määrittää, millä tavalla haluat vastaanottaa syötteen stream kanavan ja onko haluat suorittaa oman stream live koodaus kanavan. Sinulla on kaksi vaihtoehtoa:

- **Ei mitään** (läpivienti) – Määritä tämä arvo, jos aiot käyttää paikallisen live encoder joka siirtää usean bittitaajuus stream (läpivienti stream). Tässä tapauksessa saapuvien stream välitetty tulos ilman mitään koodaus. Tämä on kanavan ennen 2.10 release toimintaa.  

- **Vakio** – Valitse tämä arvo, jos haluat koodata oman yksittäisen bittitaajuus usean bittitaajuus stream live virta Media-palveluiden avulla. Tämä menetelmä on enemmän taloudellisia skaalauksen nopeasti epäsäännölliset tapahtumat. Huomioon, että on laskutuksen vaikutus live koodaustoiminnon ja muista, että jätä live koodaus kanavien "On"-tilassa maksamaan laskutuksen kulut.  On suositeltavaa, että lopetat käynnissä kanavien heti, live streaming tapahtuman päätyttyä ylimääräisiä tunnin välein lisämaksujen välttämiseksi. 

##<a name="comparison-of-channel-types"></a>Kanavan tiedostotyyppien vertailu

Seuraavassa taulukossa on oppaan avulla voit käyttää Media Services-palveluissa kanavan kahdenlaisia vertailu

Toiminto|Läpivientikysely kanava|Vakio-kanava
---|---|---
Yksittäisen bittitaajuus syöte on koodattu kyselyjä useita bitrates pilveen|Ei|Kyllä
Suurin tarkkuus lukumäärä|1080p, 8 kerrokset 60 + fps|720p, 6 kerrokset 30 fps
Syötteen protokollat|RTMP Streaming tasainen|RTMP, tasainen Streaming ja RTP
Hinta|Katso [hinnat sivu](/pricing/details/media-services/) ja valitse "Live Video"-välilehdestä|Katso [hinnat sivu](/pricing/details/media-services/) 
Suorituksen enimmäisajan|24 x 7|kahdeksan tuntia
Tuki Kivitaulut lisääminen|Ei|Kyllä
Tuki ad Signalointi|Ei|Kyllä
Läpivientikysely CEA 608/708 kuvatekstit|Kyllä|Kyllä
Mahdollisuus-syötteen osuus lyhyt ruutujen palauttaminen|Kyllä|Ei (kanavan alkaa slating 6 + ilman syöttötiedot sekunnin jälkeen)
Yhdenmukaisen syötteen GOPs tuki|Kyllä|Ei – syöte on vahvistettava 2sec GOPs
Muuttujan kehyksen korko IME-tuki|Kyllä|Ei – syöte on vahvistettava kehyksen korko.<br/>Pieniä muutoksia sallitaan esimerkiksi suuri liikerata näkymien aikana. Mutta encoder ei voi poistaa 10 kehyksiä/s.
Automaattinen-katkaisu kanavien, kun syöte syötteen menetetään.|Ei|Jos ohjelma ei ole käynnissä 12 tunnin jälkeen 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Kanavien, joka vastaanottaa usean bittitaajuus live stream paikallisen koodauslaitteista (läpivienti) käyttäminen

Seuraavassa kaaviossa näkyvät AMS ympäristön tärkeimmät osat, jotka liittyvät **läpivienti** -työnkulussa.

![Live työnkulku](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Lisätietoja on artikkelissa [käsitteleminen kanavat, vastaanota usean-bittitaajuus Live virta-paikallisen koodauslaitteista](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Jotka voivat suorittaa Azure Media-palvelujen kanssa live koodaus kanavien käyttäminen

Seuraavassa kaaviossa näkyvät AMS ympäristön tärkeimmät osat, jotka liittyvät Live Streaming työnkulun, jossa kanavan on käytössä suorittamiseen live koodaus Media-palvelujen kanssa.

![Live työnkulku](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Lisätietoja on artikkelissa [käyttäminen kanavat, jotka ovat käytössä suorittaa Live koodaus Azure Media-palvelujen kanssa](media-services-manage-live-encoder-enabled-channels.md).

##<a name="description-of-a-channel-and-its-related-components"></a>Kuvaus kanavan ja sen Aiheeseen liittyvät osat

###<a name="channel"></a>Kanavan

Media-palveluiden [kanavan](https://msdn.microsoft.com/library/azure/dn783458.aspx)s vastaavat streaming sisällön käsittelyyn. Kanavan tarjoaa syötteen päätepisteen (ingest URL-osoite), valitse annat live transcoder. Kanavan vastaanottaa reaaliaikaisia syötteen virtaa live transcoder ja helpottaa käytettävissä streaming vähintään yksi StreamingEndpoints kautta. Kanavien tarjoavat myös esikatselu-päätepisteen (esikatselu URL-osoite), jonka avulla esikatselu ja Vahvista oman stream ennen lisäkäsittelyä ja toimituksen.

Voit avata ingest ja Esikatsele URL-Osoitteen kanavan luodessasi. Saat nämä URL-osoitteet-kanavan ei ole aloitettu-tilassa olevan. Kun olet valmis aloittamaan valitseminen live transcoder-tietojen tuominen kanavan, kanavan on oltava käynnissä. Kun live transcoder käynnistyy ingesting tietoja, voit esikatsella oman muodossa.

Media Services-tileille voi olla useita kanavat, useita ohjelmia ja useita StreamingEndpoints. Kaistanleveys ja suojaus-tarpeiden mukaan StreamingEndpoint services voi nimettävä vähintään yksi kanavat. Mikä tahansa StreamingEndpoint voivat hyödyntää kanavan.


###<a name="program"></a>Ohjelma 

[Ohjelman](https://msdn.microsoft.com/library/azure/dn783463.aspx) avulla voit hallita julkaisu- ja live stream osia tallennustilaan. Kanavien hallinta ohjelmat. Kanavan ja ohjelma yhteyden muistuttaa perinteinen media, jossa kanavan on vakio stream sisällön ja ohjelma on määritetty kyseisen kanavan joitakin ajoitetun tapahtuma.
Voit määrittää ohjelman tallennetun sisällön säilytettävien määrittämällä **ArchiveWindowLength** -ominaisuuden tuntimäärä. Tämän arvon voi määrittää enintään 25 tuntia vähintään viiteen minuuttiin. 

ArchiveWindowLength määrittää myös ajan asiakkaiden enimmäismäärän voit haku taaksepäin nykyinen reaaliaikainen merkistä lähtien. Ohjelmat voidaan suorittaa tietyn ajanjakson päälle, mutta sisältöä, joka on välin takana ikkunan pituus jatkuvasti poistetaan. Tämän ominaisuuden arvoksi myös määrittää, kuinka kauan asiakkaan luettelot voi kasvaa.

Jokainen ohjelma on liitetty sijoituksen. Voit julkaista ohjelma on luotava locator, liitetty annetaan. Ottaa tämän locator avulla voit luoda streaming URL-osoite, voit tarjota asiakkaat.

Kanavan tukee jopa kolme samanaikaisesti käynnissä olevat ohjelmat, joten voit luoda useita arkistot samalla saapuvat muodossa. Näin voit julkaista ja arkistoida tapahtuman eri osia tarpeen mukaan. Esimerkiksi business vaatimus on arkistoon ohjelman 6 tuntia, mutta vain viimeisten 10 minuutin yleislähetys. Tämän vuoksi haluat luoda kahden samanaikaisesti käynnissä olevat ohjelmat. Yksi ohjelma on määritetty arkistoon tapahtuman 6 tuntia, mutta ohjelma ei ole julkaistu. Toinen ohjelma on määritetty arkistoida 10 minuutin ja ohjelma on julkaistu.


##<a name="billing-implications"></a>Laskutus vaikutukset

Kanavan alkaa heti, kun se on "käytössä-tilan siirtymät Ohjelmointirajapinnan laskutus.  

Seuraavassa taulukossa on esitetty, miten kanavan hyötyä yhdistäminen laskutuksen hyötyä API ja Azure-portaalissa. Huomaa, että tilat ovat hieman erilaiset API ja Portal määritetyn UX välillä Kun kanava on Ohjelmointirajapinnan kautta "Käynnissä"-tilassa tai Azure-portaalissa "Valmis" tai "Virtautetun median"-tilassa, laskutus on aktiivinen.

Lopeta laskutusta voit muokata kanavan edellyttää lopettaa kanavan kautta Ohjelmointirajapinnan tai Azure-portaalissa.
Olet vastuussa pysähtyy kanavat, kun olet tehnyt kanava. Lopeta kanavan virheen aiheuttaa jatkuvan laskutus.

>[AZURE.NOTE]Kun käsittelet vakio kanavien AMS Automaattinen katkaisu kanavan, joka on yhä tilassa "On" 12 tunnin syötteen syötteen menetetään ja ei ohjelmia ei ole käynnissä. Kuitenkin yhä laskutetaan kerran kanavan oli "käytössä-tilassa.

###<a id="states"></a>Kanavan hyötyä ja kuinka ne vastaavat laskutuksen tila 

Kanavan nykyisen tilan. Mahdolliset arvot ovat seuraavat:

- **Pysäyttää**. Tämä on kanavan alkuvaihe sen luomisen jälkeen (paitsi jos se on automaattinen käynnistys-valintaruutu on valittuna portaalin.) Ei ole Laskutus tapahtuu tässä tilassa. Tässä tilassa kanava-ominaisuuksia voi päivittää, mutta streaming ei sallita.
- **Aloitetaan**. Kanavan aloitetaan. Ei ole Laskutus tapahtuu tässä tilassa. Päivityksiä ei tai streaming sallitaan tässä tilassa aikana. Jos näyttöön tulee virhesanoma, kanavan palauttaa pysäytetty-tilaan.
- **Käynnissä**. Kanavan pystyy käsittelyn live virtaa. Se on nyt Laskutus käyttö. Voit estää muita laskutuksen kanavan on lopetettava. 
- **Pysäytetään**. Kanavan pysäytetään. Ei ole Laskutus tapahtuu lyhytkestoisia tässä tilassa. Päivityksiä ei tai streaming sallitaan tässä tilassa aikana.
- **Poistaminen**. Kanavan poistetaan. Ei ole Laskutus tapahtuu lyhytkestoisia tässä tilassa. Päivityksiä ei tai streaming sallitaan tässä tilassa aikana.

Seuraavassa taulukossa on tietoja kanavan hyötyä yhdistämisestä laskutuksen tilaan. 
 
Kanavan tila|Portaalin Käyttöliittymän ilmaisimet|On laskutuksen?
---|---|---
Aloittaminen|Aloittaminen|Ei (lyhytkestoisia tila)
Käynnissä|Valmis (ei ole käynnissä olevat ohjelmat)<br/>tai<br/>Streaming (vähintään yksi käynnissä olevan ohjelman)|Kyllä
Pysäyttäminen|Pysäyttäminen|Ei (lyhytkestoisia tila)
Pysäytetty|Pysäytetty|Ei


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Aiheeseen liittyviä ohjeita

[Pirstoutuneet MP4 Live Azure Media-palveluiden Ingest määritys](media-services-fmp4-live-ingest-overview.md)

[Kanavien, jotka voivat suorittaa Live koodaus Azure Media-palvelujen kanssa käsitteleminen](media-services-manage-live-encoder-enabled-channels.md)

[Kanavien, joka vastaanottaa usean bittitaajuus Live Stream paikallisen koodauslaitteista käsitteleminen](media-services-live-streaming-with-onprem-encoders.md)

[Kiintiöiden ja rajoitukset](media-services-quotas-and-limitations.md).  

[Media Services-palveluiden käsitteitä](media-services-concepts.md)
