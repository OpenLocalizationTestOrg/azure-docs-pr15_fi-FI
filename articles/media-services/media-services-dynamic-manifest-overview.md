<properties 
    pageTitle="Suodattimet ja dynaaminen luettelot | Microsoft Azure" 
    description="Tässä ohjeaiheessa kerrotaan, miten voit luoda suodattimia, jotta asiakkaan voi käyttää niitä stream stream tietyn osiin. Media Services luo arkistoon Valikoiva streaming dynaaminen luettelot." 
    services="media-services" 
    documentationCenter="" 
    authors="cenkdin" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="cenkdin;juliako"/>

# <a name="filters-and-dynamic-manifests"></a>Suodattimet ja dynaaminen luettelot

2.11 release alkaen Media Services avulla voit määrittää oman varat suodattimet. Nämä ovat palvelimen asiakaspuolen sääntöjen, joiden avulla voit valita esimerkiksi asiakkaille: toisto vain videon (sen sijaan, että koko videon toiston)-osaan tai määrittää vain osaa ääni- ja käännöksiä, asiakkaan laitteen voit käsitellä (sijaan kaikki käännöksiä, jotka liittyvät kohteen). Oman kohteiden suodattaminen arkistoidaan – **Dynaamisen luettelon**s, jotka on luotu asiakkaan pyydettäessä videovirtaa perusteella määritetty suodattimet.

Tässä ohjeaiheessa kerrotaan Yleisiä tilanteita, joissa jossa suodattimien avulla olla erittäin hyödyllistä asiakkaidesi ja linkkejä ohjeaiheisiin, jotka kuvaavat suodattimien luomisesta ohjelmallisesti (tällä hetkellä, voit luoda suodattimia REST API vain).

##<a name="overview"></a>Yleiskatsaus

Pitäessäsi sisältöä (streaming tapahtumia tai videotilauspalveluiden) asiakkaille lähinnä aikana laadukkaita videon verkon eri olosuhteissa eri laitteisiin. Tavoitteet tätä tavoitteen toimimalla seuraavasti:

- koodata oman virta usean-bittitaajuus ([mukautuvat bittitaajuus](http://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)) videovirtaa (tämä huolehtia laatua ja verkon ehdot) ja 
- Media-palveluiden [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md) avulla voit dynaamisesti uudelleen pakkaaminen oman stream tuominen eri protokollat (tämä huolehtia streaming eri laitteissa). Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming technologies lähettämisen: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja). 

###<a name="manifest-files"></a>Luettelo tiedostoista 

Kun koodata kohteiden osalta mukautuvat bittitaajuus streaming- **luettelon** (soittoluettelo)-tiedosto luodaan (tiedosto on tekstipohjainen tai XML-pohjainen). **Luettelon** -tiedosto sisältää streaming metatietoja, kuten: tyyppi (ääni, video tai teksti) voit seurata, seuraa nimen, alkamisaika ja päättymisaika, bittitaajuus (ominaisuudet), seuraa kieli-esitys-ikkuna (kiinteä kesto liukuva ikkuna), videon pakkauksenhallinta (FourCC). Se myös ohjaa Playerin seuraava osa hakea tietoja tietojen seuraavan toistettavassa videon osat käytettävissä sekä niiden sijainti. Vaillinaiset lauseet (tai osia) ovat videon sisällön todellinen "joukkojen".


Tässä on esimerkki tiedostojen tiedoston: 

    
    <?xml version="1.0" encoding="UTF-8"?>  
    <SmoothStreamingMedia MajorVersion="2" MinorVersion="0" Duration="330187755" TimeScale="10000000">
    
    <StreamIndex Chunks="17" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="8">
    <QualityLevel Index="0" Bitrate="5860941" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931300016E360000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="1" Bitrate="4602724" FourCC="H264" MaxWidth="1920" MaxHeight="1080" CodecPrivateData="0000000167640028AC2CA501E0089F97015202020280000003008000001931100011EDC00002CD29FF8C7076850A45800000000168E9093525" />
    <QualityLevel Index="2" Bitrate="3319311" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="000000016764001FAC2CA5014016EC054808080A00000300020000030064C0800067C28000103667F8C7076850A4580000000168E9093525" />
    <QualityLevel Index="3" Bitrate="2195119" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C1000044AA0000ABA9FE31C1DA14291600000000168E9093525" />
    <QualityLevel Index="4" Bitrate="1469881" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FAC2CA503C045FBC054808080A000000300200000064C04000B71A0000E4E1FF8C7076850A4580000000168E9093525" />
    <QualityLevel Index="5" Bitrate="978815" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C08001E8480004C4B7F8C7076850A45800000000168E9093525" />
    <QualityLevel Index="6" Bitrate="638374" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EAC2CA50280BFE5C0548303032000000300200000064C080013D60000C65DFE31C1DA1429160000000168E9093525" />
    <QualityLevel Index="7" Bitrate="388851" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DAC2CA505067E7C054830303200000300020000030064C040030D40003D093F8C7076850A45800000000168E9093525" />
    
    <c t="0" d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="20000000" /><c d="9600000"/>
    </StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_128kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_128kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="125658" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    
    <StreamIndex Chunks="17" Type="audio" Url="QualityLevels({bitrate})/Fragments(AAC_und_ch2_56kbps={start time})" QualityLevels="1" Name="AAC_und_ch2_56kbps">
    <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="53655" FourCC="AACL" CodecPrivateData="1210" Channels="2" PacketSize="4" SamplingRate="44100" />
    
    <c t="0" d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201361" /><c d="20201360" /><c d="20201361" /><c d="20201360" /><c d="6965987" /></StreamIndex>
    
    </SmoothStreamingMedia>
    
###<a name="dynamic-manifests"></a>Dynaaminen luettelot

On [tilanteissa](media-services-dynamic-manifest-overview.md#scenarios) , kun asiakkaalle on joustavampi kuin oletusarvoinen kohteiden luettelon tiedoston kuvatusta. Esimerkki:

- Laitteen tietyn: toimittaa vain määritetyn käännöksiä-ja määritetty kieli kappaleet, jotka tukevat laitteella, joka on käytettävä toisto sisältö ("kuvakäännöksen suodattaminen"). 
- Pienennä luetteloa näyttämään aliraportti ClipArt-kuvan live nopeasti ("aliraportti ClipArt suodattaminen").
- TRIM ("rajaaminen video") videon alkuun.
- Muuta esitys-ikkuna (DVR), jos haluat antaa rajoitettu pituus DVR ikkunan Player-ohjelmassa ("säätää esityksen ikkuna").
 
Tämä joustavuus saavuttamiseksi Media Services on **Dynaaminen luettelot** ennalta määritetyt [suodattimet](media-services-dynamic-manifest-overview.md#filters)perusteella.  Kun olet määrittänyt suodattimet, asiakkaat voi käyttää niitä lähettää tietyn kuvakäännöksen tai aliraportti leikkeet videon. Hän määrittää suodattimet streaming URL-osoitteen. Suodattimia voi käyttää mukautuvat bittitaajuus streaming [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md)tukemat protokollat: HLS, MPEG-VIIVA, tasainen Streaming ja HDS. Esimerkki:

Suodattimen MPEG VIIVA URL-osoite

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=MyLocalFilter)

Tasainen Streaming URL-osoite suodatin

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyLocalFilter)


Katso lisätietoja siitä, miten voit toimittaa sisältöä ja luoda streaming URL-osoitteet, [toimenpide yleiskatsaus](media-services-deliver-content-overview.md).


>[AZURE.NOTE]Huomaa, että dynaaminen luettelot eivät muutu kohteen ja, että annetaan oletusarvoinen-luettelo. Asiakkaan voit pyytää stream kanssa tai ilman suodattimia. 


###<a id="filters"></a>Suodattimet 

Kohteiden suodattimien kahdenlaisia: 

- Yleiset suodattimet (voidaan käyttää minkä tahansa annetaan Azure Media Services-tilin, on tilin elinaika) ja 
- Paikallinen suodattimet (vain voi suojata, johon suodatin on liitetty luotaessa omaisuuden, on kohteen elinaika). 

Yleinen ja paikallisen suodattimen on samat ominaisuudet. Tärkein kahden välinen ero on mitä skenaarioita tyypin ilmoitus on paremmin. Yleiset suodattimet ovat yleensä sopiva laite-profiileista (kuvakäännöksen suodattaminen) jonka paikallisen suodattimia voi käyttää rajaaminen tiettyyn kohteiden.


##<a id="scenarios"></a>Yleisiä tilanteita, joissa 

Kuten on mainittu ennen pitäessäsi sisältöä (streaming tapahtumia tai videotilauspalveluiden) asiakkaille lähinnä aikana laadukkaita videon verkon eri olosuhteissa eri laitteisiin. Lisäksi, että voi on muita vaatimuksia, joihin sisältyy suodattaa oman varat ja käyttää **Dynaamisen luettelon**s. Seuraavissa osissa annetaan suodatus skenaarion lyhyt yleiskatsaus.

- Määritä ääni- ja käännöksiä, tietyt laitteet voivat käsitellä (sijaan kaikki käännöksiä, jotka liittyvät kohteen) alijoukko. 
- Vain osa (sen sijaan, että koko videon toiston) Videon toistaminen.
- Säädä DVR esitys-ikkuna.

##<a name="rendition-filtering"></a>Kuvakäännöksen suodattaminen 

Voit halutessasi koodata koodauksen profiileja (H.264 perusaikataulun H.264 hyvin AACL, AACH, Dolby Digital Plus) ja useita laatu bitrates oman resurssi. Kaikki asiakaslaitteiden ei tue, kaikki tekemäsi resurssi on profiilit ja bitrates. Esimerkiksi vanhat Android-laitteille tukee vain H.264 perusaikataulun + AACL. Lähettää suurempi bitrates laitteella, joka ei voi käyttää mitä etuja, jätteet kaistanleveyden ja laitteen laskenta. Laitteen pitää purkaa kaikki tietyn tiedot, vain, jos haluat skaalata näyttämistä varten.

Dynaaminen luettelon avulla voit luoda laite-profiileista mobile, kuten konsoli HD/SD jne ja raidat ja laatuja, johon haluat osallistua kunkin profiilin.

 
![Kuvakäännöksen suodatus-Esimerkki][renditions2]

Seuraavassa esimerkissä encoder käytettiin koodata mezzanine-kohteiden tuominen seitsemän ISO MP4s videon käännöksiä (mistä 180p 1080 p). Koodattu resurssi voi pakata dynaamisesti mihin tahansa seuraavista protokollista: HLS, tasainen, MPEG VIIVA ja HDS.  Kaavion yläreunassa näkyy HLS luettelo, jossa ei ole suodattimia annetaan (se sisältää kaikki seitsemän käännöksiä).  Näkyy vasemmassa alakulmassa, johon nimeltä "ott" suodatin on otettu käyttöön HLS luettelo. Voit poistaa kaikki bitrates alla 1Mbps, jonka tuloksena ala kaksi laatutaso parhaillaan poistaa vastauksessa määrittää "ott"-suodatin.  Oikeassa alareunassa näkyy HLS luettelo, johon nimeltä "mobiili-suodatin on otettu käyttöön. "Mobile" suodatin määrittää Poista missä tarkkuus on suurempi kuin 720p, jonka tuloksena kaksi käännöksiä 1080p käännöksiä parhaillaan poistaa.

![Kuvakäännöksen suodattaminen][renditions1]

##<a name="removing-language-tracks"></a>Poista kieliraidat

Oman kalusto voi olla esimerkiksi englanti, espanja, ranska ääni monikielisiä. Yleensä Player SDK johtajien oletuskuva ääniraidan valinnan ja käytettävissä äänen seuraa Käyttäjävalinta kohden. Se on hankalaa, voit tehdä esimerkiksi Playerin SDK: T, se vaatii erilaisia laitekohtaiset player-kehysten välillä. Myös joitakin ympäristöissä Player-ohjelmointirajapinnan on rajoitettu ja Älä sisällytä ääni valinnan ominaisuus kohtaa, johon käyttäjät eivät voi valita tai muuttaa oletusarvon ääniraidan. Kohteiden suodattimilla voit hallita ongelman luomalla suodattimia, jotka sisältävät vain halutut ääni kielet.

![Kieliraidat suodattaminen][language_filter]


##<a name="trimming-start-of-an-asset"></a>Käynnistä sijoituksen rajaaminen 

Useimmat live streaming tapahtumat-operaattoreita suorittaa joitakin testejä ennen todellinen tapahtuma. Esimerkiksi ne saattavat sisältää pöydältä tältä ennen tapahtuman alkua: "Ohjelma alkaa hetkeksi". Jos ohjelma on arkistointi, numero- ja pöydältä tiedot myös arkistoidaan ja sisällytetään esityksen. Nämä tiedot olisi kuitenkin näkyvissä ei asiakkaille. Dynaaminen luettelon, jossa voit luoda Käynnistä aika-suodatinta ja poistaa tarpeettomat tiedot luettelo.

![Käynnistä rajaaminen][trim_filter]

##<a name="creating-sub-clips-views-from-a-live-archive"></a>Luodaan aliraportti leikkeitä (näkymät) live arkistosta

Useat live tapahtumat ovat pitkiä käynnissä ja live arkisto saattavat sisältää useita tapahtumat. Live tapahtuman jälkeen päät yleisradioyhtiöille kannattaa jakaa live arkisto looginen ohjelman käynnistettäväksi kyselyjä ja Lopeta sarjaa. Valitse julkaista virtual ohjelmat erikseen ilman viestin käsittelyn live arkisto ja luomalla ei erillinen kalusto (joka aiemmin välimuistissa Vaillinaiset lauseet etu ei saavat CDNs). Esimerkkejä tällaisista virtual ohjelmista (sisäinen leikkeet) ovat neljännesvuosittain jalkapallo tai koripallon Ottelu, innings baseball- tai näkyy olympialaiset-ohjelman yksittäiset tapahtumat.

Dynaaminen luettelon, jossa voit luoda alkamis-ja päättymisajat käyttämällä suodattimia ja luoda virtuaalisen näkymiä live arkisto alkuun. 

![Subclip suodatin][subclip_filter]

Suodatettujen kohteiden:

![Hiihdon][skiing]

##<a name="adjusting-presentation-window-dvr"></a>Esitys-ikkuna (DVR) muuttaminen

Tällä hetkellä Azure Media Services on kehäviittaus arkisto, jossa keston määritetty 5 minuuttia - 25 tuntia. Tiedostojen suodattaminen voidaan avulla voit luoda juokseva DVR-ikkunan yläosassa arkisto, poistamatta media. On kohdassa yleisradioyhtiöille haluat säätää mitä siirtää live reunan ja samanaikaisesti pidä isommaksi arkistointia ikkuna rajoitettu DVR-ikkunassa skenaarioita. Lähetystoiminnan kannattaa käyttää tiedot, jotka ovat DVR Korosta leikkeet ikkunasta tai he\she kannattaa antaa eri DVR windows eri laitteissa. Esimerkiksi useimmat mobiililaitteiden ei käsittele suuri DVR windows (Voit määrittää 2 minuuttia DVR ikkunan mobiililaitteille ja 1 tunti työpöytäsovelluksiin).

![DVR-ikkuna][dvr_filter]

##<a name="adjusting-livebackoff-live-position"></a>Säätäminen LiveBackoff (live sijainti-ruutu)

Tiedostojen suodattaminen voidaan muutaman sekunnin poistaminen live ohjelmasta reaaliaikaisia reunaa. Näin yleisradioyhtiöille katsoa esityksen julkaisun esikatselu-kohta ja Luo ilmoitus kohdistimen pistettä, ennen kuin katsojille vastaanottaa stream (yleensä palautettua päältä 30 sekuntia). Lähetystoiminnan voit siirtää nämä mainokset niiden asiakkaan kehysten senhetkinen niiden vastaanotettu ja käsitellä tietoja ennen ilmoitus mahdollisuuden.

Ilmoitus-tuen lisäksi LiveBackoff voi käyttää asiakas suoran Lataa sijainnin muuttaminen niin, että kun asiakkaat alueen ryömintä viesti live reuna ja he voivat käyttää nykyistä Vaillinaiset lauseet palvelimesta sijaan 404 tai 412 HTTP-virhe.

![livebackoff_filter][livebackoff_filter]


##<a name="combining-multiple-rules-in-a-single-filter"></a>Useita sääntöjä yhden suodattimen yhdistäminen

Voit yhdistää useita suodatus sääntöjä yhden suodatinta. Esimerkkinä voit määrittää alueen säännön poistaminen live arkisto pöydältä ja suodattaa käytettävissä bitrates. Useiden suodatus sääntöjen lopputulos on sääntöjen yhdistelmä (vain leikkaus).

![useita sääntöjä][multiple-rules]

##<a name="create-filters-programmatically"></a>Luo suodattimet ohjelmallisesti

Seuraavassa aiheessa kerrotaan Media Services-objektit, jotka liittyvät suodattimet. Aihe näkyy myös ohjelmallisesti suodattimien luomisesta.  

[REST API suodattimien luominen](media-services-rest-dynamic-manifest.md).

## <a name="combining-multiple-filters-filter-composition"></a>Yhdistämällä useita suodattimia (suodatin luodaan)

Voit myös yhdistää useita suodattimia yhden URL-osoitteessa. 

Seuraavassa esitellään miksi voit yhdistää suodattimet:

1. Haluat suodattaa oman videon laatuja mobiililaitteille, kuten Android- ja iPAD (jotta voit rajoittaa videon laatuja). Jos haluat poistaa tarpeettoman laatuja, luot Yleinen suodatin, joka sopii laite-profiileista. Kuten edellä mainittiin, Yleiset suodattimet voi käyttää samaa media services-tilin ilman enempää liitos-kohdassa kaikki varat. 
2. Haluat myös rajaaminen sijoituksen alkamis- ja -aika. Tätä paikallisen suodattimen luominen ja määritä alkamis-ja päättymisaika. 
3. Haluat yhdistää molempien suodattimia (ilman on lisättävä rajaamisen suodatin, jossa vaikeuttaa suodattimen käyttö laatu suodattaminen yhdessä).

Yhdistää suodattimet, kun haluat luoda suodattimen nimet luettelon/soittoluettelon puolipisteellä URL-osoite. Oletetaan, että sinulla nimeltä *MyMobileDevice* , että suodattimet laatuja ja sinulla on toinen nimetty *MyStartTime* voit määrittää aloitusaika suodattimen. Voit yhdistää ne seuraavasti:

    http://teststreaming.streaming.mediaservices.windows.net/3d56a4d-b71d-489b-854f-1d67c0596966/64ff1f89-b430-43f8-87dd-56c87b7bd9e2.ism/Manifest(filter=MyMobileDevice;MyStartTime)

Voit yhdistää 3 suodattimet. 

Katso lisätietoja [tämän](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogin.


##<a name="know-issues-and-limitations"></a>Tiedät ongelmia ja rajoituksia

- Dynaaminen luettelo toimii GOP rajat (avain kehykset) näin ollen rajaaminen on GOP tarkkuutta. 
- Voit käyttää samaa suodattimen nimi paikalliset ja Yleiset suodattimet. Huomaa, että paikallisen suodattimen ohittavat alempana ja ohittaa yleiset suodattimet.
- Jos päivität suodattimen, se voi viedä enintään 2 minuuttia streaming päätepisteen päivittämiseen sääntöjä. Jos sisältö on served joitakin suodattimia (ja välimuistiin välityspalvelimet ja CDN tallentaa), päivitetään suodattimia voi aiheuttaa Playerin virheet. Suositellaan, jos haluat poistaa suodattimen päivittämisen jälkeen on. Jos tämä vaihtoehto ei ole mahdollista, kannattaa käyttää eri suodatinta.


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="see-also"></a>Katso myös

[Välittää sisältöä asiakkaiden yleiskatsaus](media-services-deliver-content-overview.md)

[renditions1]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/media-services-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/media-services-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/media-services-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/media-services-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/media-services-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/media-services-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/media-services-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/media-services-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/media-services-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/media-services-dynamic-manifest-overview/media-services-skiing.png
 