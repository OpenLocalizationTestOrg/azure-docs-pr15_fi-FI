<properties
    pageTitle="Välittää sisällön asiakkaat | Microsoft Azure"
    description="Tässä artikkelissa on yleiskatsaus, millaisia toimenpiteitä kuuluu välittää sisältöä Azure Media-palvelujen kanssa."
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
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="deliver-content-to-customers"></a>Sisältö toimitetaan asiakkaat
Kun pidät streaming tai videotilauspalveluiden sisältöä asiakkaille, lähinnä aikana laadukkaita videoita eri laitteisiin verkon eri olosuhteissa.

Voit toteuttaa tämän tavoitteen seuraavasti:

- Koodata oman usean-bittitaajuus (mukautuvat bittitaajuus) videovirtaa virta. Tämä huolehtia laatua ja verkon ehdot.
- Microsoft Azure Media Services [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md) avulla voit dynaamisesti uudelleen pakkaaminen oman stream tuominen eri protokollat. Tämä huolehtia streaming eri laitteissa. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming technologies lähettämisen: HTTP Live Streaming (HLS), tasainen Streaming, MPEG-VIIVA ja HDS (varten vain Adobe Primetime/Access asiakirjamalleja).

Tässä artikkelissa on yleiskatsaus tärkeitä sisällön toimittamisen käsitteitä.

Tarkista tunnetuista ongelmista on artikkelissa [tunnetut ongelmat](media-services-deliver-content-overview.md#known-issues).

## <a name="dynamic-packaging"></a>Dynaaminen pakkaus

Voit näyttää julkaisun mukautuvat bittitaajuus MP4 tai tasainen Streaming koodattu sisällön streaming tukemat Media-palveluissa (MPEG-VIIVA, HLS, tasainen Streaming, HDS) eikä sinun tarvitse uudelleen pakkaaminen huomioon nämä streaming muodot Dynaaminen pakkaus, joka sisältää Media-palveluiden kanssa. On suositeltavaa välittää sisällön dynaamisen pakkauksen kanssa.

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata mezzanine (lähde)-tiedoston tuominen säädön bittitaajuus MP4-tiedostot tai säädön bittitaajuus tasainen Streaming-tiedostoja.
- Vähintään yhden tarvittaessa streaming yksikön streaming päätepisteen, joista voit taata aikana sisällön hakeminen Lisätietoja on artikkelissa [miten tarvittaessa streaming varatut resurssit](media-services-portal-manage-streaming-endpoints.md).

Dynaaminen pakkaus, tallentaa ja maksaa yhden Tallennusmuoto tiedostot. Media-palvelut muodosta ja yhteyshenkilönä oikea vastaus pyyntöjen perusteella.

Lisäksi estää Dynaaminen pakkaus ominaisuuksia käyttämisen tarvittaessa streaming varatut resurssit antaa sinulle erillinen juniin kapasiteetin, joka voi ostaa 200 Mbps kerrallaan. Oletusarvon mukaan tarvittaessa streaming on määritetty jaettu esiintymä-mallissa, mitkä palvelimen resurssit (esimerkiksi Laske tai juniin kapasiteetti) jaetaan muiden käyttäjien kanssa. Voit parantaa streaming tarvittaessa-siirtonopeuden ostamalla tarvittaessa streaming varatut resurssit.

Lisätietoja on artikkelissa [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md).

## <a name="filters-and-dynamic-manifests"></a>Suodattimet ja dynaaminen luettelot

Voit määrittää oman Media Services resurssien suodattimet. Nämä ovat palvelinpuolen säännöt, jotka auttavat asiakkaille esimerkiksi toistaa videon tietyn osan tai määrittää alijoukkoa ääni- ja käännöksiä, asiakkaan laitteen voit käsitellä (sijaan kaikki käännöksiä, jotka liittyvät kohteen). Suodattaminen saavutetaan kautta *dynaaminen luettelot* , jotka on luotu, kun asiakas pyytää videovirtaa perusteella vähintään yksi määritetyillä suodattimilla.

Lisätietoja on kohdassa [suodattimet ja dynaaminen luettelot](media-services-dynamic-manifest-overview.md).

## <a name="locators"></a>Locators

Voit antaa käyttäjän URL-osoite, jonka avulla voidaan käyttää tai ladata sisältöä, sinun on julkaista oman resurssi locator luomalla. Locator tarjoaa aloituskohdan käyttämään sijoituksen tiedostoille. Media-palveluiden tukee kahdentyyppisiä locators:

- OnDemandOrigin locators. Nämä käytetään mediavirtaa (kuten MPEG-VIIVA, HLS tai tasainen Streaming) tai ladata tiedostoja asteittain.
-  Jaettu käyttö allekirjoitus (SAS) URL-Osoitteen locators. Näiden avulla voit ladata mediatiedostojen paikalliseen tietokoneeseen.

*Käyttöoikeuskäytäntö* käytetään (esimerkiksi luku, kirjoittaminen ja luettelon) käyttöoikeuksien määrittämistä ja keston, jossa on jokin muu sähköpostiohjelma on käyttöoikeus, erityisesti annetaan. Huomaa, että luettelon käyttöoikeuksia (AccessPermissions.List) olisi ei käytetä OrDemandOrigin locator luomiseen.

Locators on vanhentumispäivämäärien. Azure portaalin määrittää locators vanhenemispäivämäärä 100 vuotta myöhemmin.

>[AZURE.NOTE] Jos Azure portaalin avulla voit luoda locators ennen maaliskuussa 2015, kyseiset locators on määritetty olevaksi kahden vuoden jälkeen.

Päivitä vanhenemispäivämäärä, locator käyttämällä [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) - tai [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) -ohjelmointirajapinnan. Huomaa, että kun päivität SAS locator vanhenemispäivää, URL-osoite muuttuu.

Locators ei ole tarkoitettu hallinta käyttäjäkohtainen käyttöoikeuksien hallinta. Voit tarkastella eri yksittäisten käyttäjien käyttöoikeuksia käyttämällä digitaalisten oikeuksien hallinta (DRM)-ratkaisuja. Lisätietoja on artikkelissa [Suojaaminen Media](http://msdn.microsoft.com/library/azure/dn282272.aspx).

Kun luot locator, voi olla 30 sekunnin viive tarvittavat tallennustilan ja välittäminen prosessit Azuren tallennustilaan vuoksi.


## <a name="adaptive-streaming"></a>Säätö-tietovirta

Säädön bittitaajuus technologies Salli Verkkoehdot ja valitse useita bitrates videosoittimen sovellukset. Kun verkkoliikennettä heikentää asiakkaan valita alemman bittitaajuus niin alemman videon laatu voit jatkaa toiston. Kun verkkoyhteys parantamiseksi asiakkaan voi siirtyä suurempi bittitaajuus ja parannettu videon laatu. Azure Media Services tukee seuraavia mukautuvat bittitaajuus tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG-VIIVA ja HDS.

Voit antaa käyttäjien streaming URL-osoitteet, ensin on luotava OnDemandOrigin locator. Luominen locator avulla voit polun resurssi, jossa haluat käyttää sisältöä. Kuitenkin voivat käyttää sisältöä, voit joutua muokkaamaan Tämä polku. Muodostaa streaming luettelon tiedoston täydellinen URL-osoite on KETJUTA locator polku arvo ja luettelo (filename.ism) tiedostonimi. Valitse **Liitä/luettelon** ja oikeassa muodossa (tarvittaessa) locator polku.

>[AZURE.NOTE]Voit myös käyttää sisältöä SSL-yhteyttä. Toiminto, varmista, että streaming URL-osoitteet alkavat HTTPS.

Voit vain virtauttaa SSL-yhteyden kautta, jos streaming päätepisteen, josta pidät sisältöä on luotu 10. syyskuussa 2014 jälkeen. Jos streaming URL-osoitteet perustuvat streaming päätepisteet luotu jälkeen 10. syyskuussa 2014 URL-osoite sisältää "streaming.mediaservices.windows.net." Streaming URL-osoitteet, jotka sisältävät "origin.mediaservices.windows.net" (vanha format) eivät tue SSL. Jos haluat, että voit käyttää SSL: n URL-osoite on vanha muodossa, Luo uusi streaming päätepiste. Virtauttaa sisällön SSL: n URL-osoitteet perusteella uuden streaming päätepisteen avulla.


## <a name="streaming-url-formats"></a>Streaming URL-muodot

### <a name="mpeg-dash-format"></a>MPEG-VIIVA-muoto

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)



### <a name="apple-http-live-streaming-hls-v4-format"></a>Apple HTTP Live Streaming (HLS) V4-muodossa

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

### <a name="apple-http-live-streaming-hls-v3-format"></a>Apple HTTP Live Streaming (HLS) V3-muotoa

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

### <a name="apple-http-live-streaming-hls-format-with-audio-only-filter"></a>Apple HTTP Live Streaming (HLS)-muodossa, jossa pelkän suodatin

Oletusarvon mukaan pelkän raidat sisältyvät HLS luettelo. Tämä on pakollinen matkapuhelinverkon verkkojen sertifikaatin Apple-kaupasta. Tässä tapauksessa Jos asiakas ei ole riittäviä kaistanleveyden tai on yhdistetty 2G-yhteyden kautta, toisto vaihtaa pelkän. Tämä auttaa pitämään sisältö streaming ilman puskurointi, mutta ei ole video. Joissakin tilanteissa player puskurointi ehkä ensisijainen pelkän päälle. Jos haluat poistaa pelkän seuraa, Lisää **pelkän EPÄTOSI** URL-osoitteeseen.

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-V3,Audio-Only=FALSE)

Lisätietoja on artikkelissa [dynaamisen luettelon yhdistelmä tuki- ja HLS tulosteen lisäominaisuuksia](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/).


### <a name="smooth-streaming-format"></a>Tasainen virtautetun median muoto

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Esimerkki:

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

### <a id="fmp4_v20"></a>Tasainen Streaming 2.0-luettelo (aiempien versioiden luettelo)

Oletusarvon mukaan tasainen Streaming tiedostojen muoto sisältää toista tunnisteen (r-tunniste). Jotkin pelaajat eivät tue r-tunniste. Asiakkaat, joilla nämä pelaajat käyttää muodossa, joka poistaa käytöstä r-tunniste:

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=fmp4-v20)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=fmp4-v20)

### <a name="hds-for-adobe-primetimeaccess-licensees-only"></a>HDS (for Adobe PrimeTime/Access asiakirjamalleja vain)

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f)

## <a name="progressive-download"></a>Nousevan lataaminen

Nousevan Lataa avulla voit aloittaa toistamisessa, ennen kuin koko tiedosto on ladattu. Et voi ladata asteittain (.ism * ismv, isma, ismt tai ismc) tiedostoja.

Asteittain ladata sisältöä, käytä locator OnDemandOrigin tyyppi. Seuraavassa esimerkissä esitetään, joka perustuu locator OnDemandOrigin tyypin URL-osoite:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

Mikä tahansa tallennustilan salattu varat, jota haluat käyttää asteittain ladattavaksi origin-palvelusta on salauksen.

## <a name="download"></a>Lataa

Voit ladata sisältöä asiakkaan laitteeseen, sinun on luotava SAS Locator. SAS locator kautta pääset käsiksi Azuren tallennustilaan säilöön, jossa tiedosto sijaitsee. Muodostaa lataamisen URL-osoite on upottaa tiedostonimen isännän ja Suojaussidosten allekirjoituksen välillä.

Seuraavassa esimerkissä esitetään, joka perustuu SAS locator URL-osoite:

    https://test001.blob.core.windows.net/asset-ca7a4c3f-9eb5-4fd8-a898-459cb17761bd/BigBuckBunny.mp4?sv=2012-02-12&se=2014-05-03T01%3A23%3A50Z&sr=c&si=7c093e7c-7dab-45b4-beb4-2bfdff764bb5&sig=msEHP90c6JHXEOtTyIWqD7xio91GtVg0UIzjdpFscHk%3D

Ottaa huomioon seuraavat asiat:

- Mikä tahansa tallennustilan salattu varat, jota haluat käyttää asteittain ladattavaksi origin-palvelusta on salauksen.
- Ladattava tiedosto, joka ei ole vielä 12 tunnin kuluessa epäonnistuu.

## <a name="streaming-endpoints"></a>Streaming päätepisteet

Streaming päätepiste edustaa streaming-palvelu, joka voi toimittaa sisältöä suoraan Playerin asiakassovellus tai edelleen sisällön toimittamisen verkkoon (CDN). Lähtevän virta streaming päätepisteen-palvelusta voi olla live stream tai videotilauspalveluiden-resurssi Media Services-tilisi. Voit myös määrittää streaming päätepisteen-palvelu kapasiteetista käsittelemään kasvava kaistanleveyden tarpeiden säätämällä streaming varatut resurssit. Vähintään yhden varattu yksikön kannattaa varata sovellusten tuotantoympäristössä. Lisätietoja on artikkelissa [miten media-palvelun](media-services-portal-manage-streaming-endpoints.md).

## <a name="known-issues"></a>Tunnetut ongelmat

### <a name="changes-to-smooth-streaming-manifest-version"></a>Muutokset tasainen Streaming luettelon versio

Ennen palvelun heinäkuussa 2016--julkaisua kun resurssien tuottamat Media Encoder vakio Media Encoder Premium työnkulun tai aiempi Azure Media Encoder on virtauttaa käyttämällä Dynaaminen pakkaus--tasainen Streaming palautettu luettelo mukainen 2.0-version. Versiosta 2.0-fragmentti kestot Älä käytä tunnisteita niin toista ("r"). Esimerkki:

<?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" n="0" />
            <c d="2000" />
            <c d="2000" />
            <c d="2000" />
        </StreamIndex>
    </SmoothStreamingMedia>

Heinäkuussa 2016-service Release luotu tasainen Streaming luettelo mukainen versioon 2.2, toista tunnisteiden fragmentti kestot. Esimerkki:

    <?xml version="1.0" encoding="UTF-8"?>
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="8000" TimeScale="1000">
        <StreamIndex Chunks="4" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="3" Subtype="" Name="video" TimeScale="1000">
            <QualityLevel Index="0" Bitrate="1000000" FourCC="AVC1" MaxWidth="640" MaxHeight="360" CodecPrivateData="00000001674D4029965201405FF2E02A100000030010000003032E0A000F42400040167F18E3050007A12000200B3F8C70ED0B16890000000168EB7352" />
            <c t="0" d="2000" r="4" />
        </StreamIndex>
    </SmoothStreamingMedia>

Joitain vanhoja tasainen Streaming asiakkaat eivät tue toista tunnisteet ja luettelo lataaminen epäonnistuu. Rajoittaa ongelman, voit käyttää aiempien versioiden tiedostojen muoto-parametrin **(Muotoile = fmp4 v20)** tai päivitä asiakkaan uusimpaan versioon, joka tukee toista tunnisteet. Lisätietoja on artikkelissa [Tasainen Streaming 2.0](media-services-deliver-content-overview.md#fmp4_v20).

## <a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Aiheeseen liittyviä ohjeita

[Päivitä Media Services locators jälkeen juoksevan tallennustilan näppäimet](media-services-roll-storage-access-keys.md)
