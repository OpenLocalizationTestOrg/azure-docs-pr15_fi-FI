<properties 
    pageTitle="Avanced Media Encoder Premium työnkulun opetusohjelmat" 
    description="Tämä asiakirja sisältää, Näytä lisäasetukset Media Encoder Premium työnkulun tehtävien suorittamisesta ja miten voit luoda monimutkaisia työnkulut työnkulun suunnittelun vaihe vaiheelta." 
    services="media-services" 
    documentationCenter="" 
    authors="xstof" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016"  
    ms.author="xstof;xpouyat;juliako"/>

#<a name="advanced-media-encoder-premium-workflow-tutorials"></a>Lisäasetukset Media Encoder Premium työnkulun opetusohjelmat

##<a name="overview"></a>Yleiskatsaus 

Tämä asiakirja sisältää vaihe vaiheelta, joka näyttää, miten **Työnkulun**suunnittelussa työnkulkujen mukauttaminen. Löydät todellinen työnkulun tiedostot [tähän](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/PremiumEncoderWorkflowSamples).  

##<a name="toc"></a>SISÄLLYSLUETTELO

Käsitellään seuraavia aiheita:

- [Koodaus MXF yhden bittitaajuus MP4 tuominen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)
    - [Uuden työnkulun aloittaminen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_start_new) 
    - [Syötteen Media-tiedosto](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_file_input)
    - [Tarkastavan rikasmediasisällön](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_streams)
    - [Lisää video encoder varten. MP4-tiedoston luominen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_file_generation)
    - [Äänivirtaa koodaus](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio)
    - [Äänen ja videon virtaa limittävän kyselyjä MP4-säilö](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_audio_and_fideo)
    - [Kirjoittaminen MP4-tiedosto](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_writing_mp4)
    - [Media-palveluiden kohteiden luominen kohdetiedosto](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_asset_from_output)
    - [Valmiit työnkulun paikallisesti testaaminen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_test)
- [Koodaus MXF multibitrate MP4s - kyselyjä Dynaaminen pakkaus käytössä](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)
    - [Lisäämällä yhden tai useamman muita MP4-tulostus](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_more_outputs)
    - [Tiedoston tulosteen nimien määrittäminen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_conf_output_names)
    - [Lisäämällä erillisen ääniraita](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_audio_tracks)
    - [Lisäämällä. ISM SMIL tiedosto](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging_ism_file)
- [Koodaus MXF multibitrate MP4 - parannettu Sinikopio tuominen](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4)
    - [Työnkulun yleiskatsaus parantamiseksi](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_overview)
    - [Tiedostojen nimeämiskäytännöt](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_file_naming)
    - [Osan julkaisuominaisuudet sivulle työnkulun pääkansio](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_publishing)
    - [On luotu nimet ovat riippuvaisia julkaistun ominaisuusarvoihin kohdetiedosto](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to__multibitrate_MP4_output_files)
- [Pikkukuvien lisääminen multibitrate MP4-tulostus](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)
    - [Voit lisätä pikkukuvat työnkulun yleiskatsaus](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to_multibitrate_MP4_overview)
    - [Lisäämällä JPG-koodaus](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4__with_jpg)
    - [Väri-tilaa muuntaminen käsittely](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_color_space)
    - [Kirjoittaminen pikkukuvat](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_writing_thumbnails)
    - [Työnkulun virheiden havaitseminen](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_errors)
    - [Valmiit työnkulun](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4_finish)
- [Aikapohjaisten rajauksen multibitrate MP4-tulostus](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim)
    - [Työnkulun yleiskatsaus jatkaa lisäämällä rajaamisen avulla](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_start)
    - [Virta-suojauskatkaisutoteutuksen käyttäminen](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_use_stream_trimmer)
    - [Valmiit työnkulun](media-services-media-encoder-premium-workflow-tutorials.md#time_based_trim_finish)
- [Komentosarjaan määritetty osan esittely](media-services-media-encoder-premium-workflow-tutorials.md#scripting)
    - [Komentosarjan työnkulun: Hei](media-services-media-encoder-premium-workflow-tutorials.md#scripting_hello_world)
- [Kehys-pohjainen rajauksen multibitrate MP4-tulostus](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim)
    - [Sinikopio jatkaa lisäämällä rajaamisen, yleiskatsaus](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_start)
    - [Valitse ClipArt-luettelosta XML](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clip_list)
    - [ClipArt-luettelon luominen komentosarjatoimintojen osan muokkaaminen](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_modify_clip_list)
    - [Lisäämällä ClippingEnabled helppokäyttöisyys-ominaisuus](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim_clippingenabled_prop)

##<a id="MXF_to_MP4"></a>Koodaus MXF yhden bittitaajuus MP4 tuominen
 
Tätä vaiheittaista Luo yksittäinen bittitaajuus. MP4-tiedosto, jonka AAC hän koodattu ääneen. MXF näytettävä tiedosto. 

###<a id="MXF_to_MP4_start_new"></a>Uuden työnkulun aloittaminen 

Avaa työnkulun suunnittelussa ja valitse "Tiedosto"-"uusi työtila"-"Muuta Sinikopio" 

Uusi työnkulku näytetään 3 osat: 

- Ensisijainen lähdetiedosto
- Leikeluettelon XML
- Tulosteen tiedoston/resurssi  

![Uusi koodauksen työnkulku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-transcode-blueprint.png)

*Uusi koodauksen työnkulku*

###<a id="MXF_to_MP4_with_file_input"></a>Syötteen Media-tiedosto

Jos haluat hyväksyä Microsoftin input mediatiedosto, jokin alkaa Media tiedoston syötteen komponentin lisääminen. Jos haluat lisätä osan työnkulun, etsi se säilöön hakuruutuun ja suunnittelu-ruutuun haluamasi tapahtuma vetää. Toimi samoin Media tiedoston syötteen ja ensisijainen lähdetiedosto-osan Muodosta yhteys tiedostonimi syötteen PIN-tunnuksen Media tiedoston syötteen.

![Yhdistetyn mediatiedostoa syöte](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-input.png)

*Yhdistetyn mediatiedostoa syöte*

Ennen kuin on mahdollista paljon muita, on ensin on osoittamaan työnkulun suunnittelussa mitä haluamme avulla voit suunnitella Microsoftin työnkulkua, jonka mallitiedostossa. Voit tehdä suunnittelun ruudusta tausta ja Etsi oikeassa ominaisuusrivin ensisijainen lähdetiedosto-ominaisuus. Napsauta kansiokuvaketta ja valitse haluamasi tiedosto Testaa työnkulkua, jonka. Kun tämä on valmis, Media tiedoston Input-osan Tarkasta tiedosto ja täytä vastaamaan se tarkastaa tiedoston tulosteen sen PIN.

![Täytetty mediatiedostoa syöte](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-populated-media-file-input.png)

*Täytetty mediatiedostoa syöte*

Kun tämä määrittää, mitä syöte haluamme käsitteleminen, se ei kerro vielä minne koodattu tulosteen lähetetään. Samalla siitä, kuinka ensisijainen lähdetiedosto on määritetty, Määritä nyt tulosteen kansion Variable-ominaisuuden sen alapuolella.

![Määritetty ja ominaisuudet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configured-io-properties.png)

*Määritetty ja ominaisuudet*

###<a id="MXF_to_MP4_streams"></a>Tarkastavan rikasmediasisällön

Usein se haluttu on tiedettävä virta ulkoasun kuin kulkee työnkulussa. Voit tarkastaa stream milloin tahansa työnkulun napsauttamalla tulosteen tai syötteen pin-osia. Kokeile tässä tapauksessa pakkaamattomat videon tulosteen PIN-tunnuksen Microsoftin Media tiedoston syötetyistä valitsemalla. Valintaikkuna avautuu, jonka avulla voit tutkia lähtevän videon.

![Tarkistetaan pakkaamattomat videon tulosteen PIN-tunnus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-inspecting-uncompressed-video-output.png)

*Tarkistetaan pakkaamattomat videon tulosteen PIN-tunnus*

Tässä tapauksessa se kertoo us esimerkiksi, että olet käsittely 1920 x 1080-syöte 24 kehyksissä sekunnissa 4:2:2 esimerkkejä videon lähes 2 minuuttia.

###<a id="MXF_to_MP4_file_generation"></a>Lisää video encoder varten. MP4-tiedoston luominen

Huomaa, että, pakkaamattomat Video- ja useita äänen tulosteen PIN ovat käytettävissä Media-tiedosto Microsoftin Input. Jos haluat koodata saapuvan videokuvan annettava koodauksen osa - tällöin luonnissa. MP4-tiedostot.

Koodata videovirtaa, H.264, Lisää suunnittelutyökalun pinta AVC videon koodaus-osa. Tämän osan tulee Pura pakkaus-videovirtaa, syötteenä ja toimittaa AVC pakattu videovirtaa sen tulosteen PIN-tunnus.

![Yhteydettömät AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-avc-encoder.png)

*Yhteydettömät AVC Encoder*

Sen ominaisuudet määrittävät, kuinka koodaus täsmälleen tapahtuu. Katsotaanpa ikkunan toimintoja joitakin muita tärkeämpiä asetukset:

- Tulosteen leveys ja korkeus Output: nämä määrittävät koodattu videon tarkkuus. Tässä tapauksessa mennään 640 x 360 kanssa
- Kehyksen korko: Jos määritetty läpivienti se vain käytetään tietolähteen kehyksen korko, on mahdollista ohittaa vaikka. Huomaa, että framerate muuntamisessa ei liikerata-maksetaan.
- Profiilin ja taso: nämä määrittävät AVC profiilin ja taso. Kätevästi Saat lisätietoja eri tasoilla ja profiilit, valitse AVC videon koodaus-osan kysymysmerkkikuvake ja Ohje-sivu näyttää yksityiskohtaisempia tietoja eri tasoille. Tässä esimerkissä mennään Main profiiliin tasolla 3,2 (oletusarvo).
- Arvioi ohjausobjektin tila ja bittitaajuus (kbps): Tässä tapauksessa emme valitaan vakion bittitaajuus (CBR), tulosteen on 1200 kbps
- Videomuoto: Tämä on tietoja VUI (Video käytettävyydestä tietojen), joka saa kirjoittaa ne H.264 stream (puolen tiedot joita voidaan käyttää koodauksen näyttämistä varten, mutta ei ole välttämätöntä toistaa oikein):
- NTSC (tavanomainen US tai japani, käyttämällä 30 fps)
- PAL (tavanomainen Europe, 25 fps avulla)
- GOP näyttötapa: emme määritettävä kiinteä GOP koko parhaiten tarkoituksiimme kanssa 2 sekunnin avain aikavälin suljettu GOPs kanssa. Tällä varmistetaan, että yhteensopivuus Dynaaminen pakkaus Azure Media-palveluiden avulla.

Syötteen Microsoftin AVC encoder Yhdistä pakkaamattomat videon tulosteen PIN-tunnuksen Media tiedoston syötteen osan pakkaamattomat Video syötteen PIN-tunnus-AVC encoder.

![Yhdistetyn AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-avc-encoder.png)

*Yhdistetyn AVC Main encoder*

###<a id="MXF_to_MP4_audio"></a>Äänivirtaa koodaus

Tässä vaiheessa on on koodattu videon, mutta alkuperäinen pakkaamattomat äänivirtaa vielä saatava pakata. Tämän ominaisuuden perehdymme on AAC koodausta AAC Encoder (Dolby)-osa. Lisätä työnkulun.

![Yhteydettömät AVC Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-unconnected-aac-encoder.png)

*Yhteydettömät AAC encoder*

Yhteensopivuusongelma on nyt: on vain yksi pakkaamattomat ääni syötteen pin AAC Encoder samalla, kun useita todennäköisesti Media tiedoston syötteen on kaksi eri pakkaamattomat äänivirtaa käytettävissä: yksi vasen ääni kanava ja yksi oikealle. (Jos olet käsittely Sisällytä ääni, joka on 6 kanavat.) Näin ei ole mahdollista yhdistäminen suoraan AAC ääni encoder äänen Media tiedoston syötteen lähteestä. AAC-osan odottaa niin kutsutut "limitetty" äänivirtaa: yksi virta, jossa on vasemman ja oikean kanavien limitetty keskenään. Kun on tiedettävä Microsoftin lähdetiedoston media audio raidat tavoilla, valitse lähteen siirtäminen emme voi luoda näiden limitetty äänivirtaa oikein määritetyn kaiutin-toimien vasemmalle tai oikealle.

Ensin jotakin haluat luotu limitetty stream tarvittavat lähde ääni kanavat. Äänen Stream Interleaver-osan käsitellä tämä us varten. Työnkulun lisääminen ja äänen tulostaa muodosta syötteen Media-tiedosto siihen.

![Äänivirtaa Interleaver yhteydessä](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-audio-stream-interleaver.png)

*Äänivirtaa Interleaver yhteydessä*

Nyt kun limitetty äänivirtaa on, on edelleen ei, minne määrittää vasemmalle tai oikealle esittäjän toimet. Jotta voit määrittää tämän olemme voidaan hyödyntää esittäjän sijainti Assigner.

![Esittäjän sijainti-Assigner lisääminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-speaker-position-assigner.png)

*Esittäjän sijainti-Assigner lisääminen*

Määritä esittäjän sijainti Assigner käytettäväksi stereo syötteen virta Encoder esiasetus suodattimen läpi "Muokattu" ja kanavan esiasetus nimeltä "2.0 (L, R)". (Tämä määrittää kanavalle 1 vasemmalle kaiutin-sijainti ja oikean kaiuttimen-sijainti kanava 2.)

Yhdistä esittäjän sijainti Assigner tulosteen AAC Encoder syötteen. Ilmoita "2.0 (L, R)"-käyttöä varten AAC-Encoder kanavan esiasetus, niin se tietää käsitellä stereo audio syötteenä.

###<a id="MXF_to_MP4_audio_and_fideo"></a>Äänen ja videon virtaa limittävän kyselyjä MP4-säilö

Microsoftin AVC annettu koodattu videovirtaa ja tutustu AAC koodattu äänivirtaa, emme tallenna molemmat kyselyjä. MP4-säilö. Prosessi, jossa sekoittamalla eri virtaa yhdeksi kutsutaan "limittävän" (tai "muxing"). Tässä tapauksessa emme ole välilehdet äänen ja videon virtaa johdonmukaisen yhdessä. MP4-paketti. Osa, joka koordinoi tämä. MP4-säilö kutsutaan ISO-MPEG-4-multiplekseri. Lisää yksi suunnittelutyökalun pinta ja Yhdistä AVC Video Encoder sekä AAC-Encoder sen syötteiden.

![Yhdistetyn MPEG4 multiplekseri](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-mpeg4-multiplexer.png)

*Yhdistetyn MPEG4 multiplekseri*

###<a id="MXF_to_MP4_writing_mp4"></a>Kirjoittaminen MP4-tiedosto

Kirjoittaessasi kohdetiedosto tuloste-osaa käytetään. Emme voi muodostaa tämä ISO-MPEG-4-multiplekseri tulosteen niin, että sen tulos saa kirjoitettu levylle. Voit tehdä tämän muodostaa Kirjoita syötteen PIN-tunnus-tiedoston tulosteen säilö (MPEG-4) tulosteen PIN-tunnuksen.

![Yhdistetty tuloste](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connected-file-output.png)

*Yhdistetty tuloste*

Tiedoston nimi, jota käytetään määräytyy tiedosto-ominaisuus. Kun kyseisestä ominaisuudesta voi olla englanninkielisissä annetusta arvosta, todennäköisesti jokin kannattaa määrittää sen lausekkeen avulla.

Työnkulku määrittää automaattisesti tulos lausekkeen ominaisuuden nimi tiedoston, napsauta tiedostonimen vieressä (vieressä olevaa kansiokuvaketta) kohdan. Valitse "Lauseke" sitten avattavassa valikossa. Tämä tuo mukanaan Lauseke-editori. Poista ensin editorin sisällön.

![Tyhjä Lauseke-editori](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-empty-expression-editor.png)

*Tyhjä Lauseke-editori*

Lauseke-editorin avulla literaali arvo, ja yhden tai usean muuttujan. Muuttujat Aloita dollarimerkki. Kun valitset $-näppäintä, editorin näkyy käytettävissä olevien muuttujien valinnan avattava kehys. Tässä tapauksessa Käytämme directory tuloksena saatava muuttuja ja peruste lähdetiedoston nimi muuttujan yhdistelmän.

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}.MP4

![Täytetty ulos Lauseke-editori](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-expression-editor.png)

*Täytetty ulos Lauseke-editori*

>[AZURE.NOTE]Jotta voit nähdä tulostus-tiedoston koodauksen työtäsi Azure, sinun on annettava arvo lauseke-editorissa. 

Kun vahvistat lausekkeen osumalla ok, ominaisuudet-ikkuna Esikatsele, mikä arvo tiedosto-ominaisuus on liitetty tässä vaiheessa kohdassa kerralla.

![Tiedoston lausekkeen korjaa tulosteen dir](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-file-expression-resolves-output-dir.png)

*Tiedoston lausekkeen korjaa tulosteen dir*

###<a id="MXF_to_MP4_asset_from_output"></a>Media-palveluiden kohteiden luominen kohdetiedosto

On kirjoitettu MP4-kohdetiedosto, kun Microsoft on osoittaa, että tämä tiedosto kuuluu tulosteen kohteen joka media services Luo tuloksena tämä työnkulku suoritetaan. Tämän vuoksi työnkulun kangasta tulosteen tiedoston/resurssi-solmun avulla. Kaikki saapuvat tiedostot yhdeksi solmu tekee tuloksena Azure Media Services resurssi osa.

Tiedoston tulostus-osan yhdistäminen tulosteen tiedoston/resurssi-osa työnkulun loppuun.

![Valmiit työnkulun](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow.png)

*Valmiit työnkulun*

###<a id="MXF_to_MP4_test"></a>Valmiit työnkulun paikallisesti testaaminen

Voit testata työnkulkua paikallisesti osumien yläreunassa työkalurivillä toista-painiketta. Lopuksi työnkulku suoritetaan Tarkasta tulosteen luotu määritetyn tulostus-kansiossa. Näet valmiiksi MP4-tiedostoon, jonka on koodattu MXF lähde-tiedostosta.

##<a id="MXF_to_MP4_with_dyn_packaging"></a>Koodaus MXF kyselyjä MP4 - multibitrate Dynaaminen pakkaus käytössä

Tätä vaiheittaista olemme luodaan tiedostojoukon useita bittitaajuus MP4 kanssa koodattu AAC yksittäisen ääni. MXF näytettävä tiedosto.

Kun usean bittitaajuus resurssi-tulostus halutun käytettäväksi yhdessä Azure Media Services useita GOP tasattu MP4-tiedostot kaikkien eri bittitaajuus ja tarkkuus on luotava tarjoamia Dynaaminen pakkaus ominaisuuksilla. Voit tehdä [Koodaus MXF siirtäminen yhden bittitaajuus MP4](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4) -ongelmatilanteita avulla us hyvä pohjana.

![Työnkulun käynnistäminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow.png)

*Työnkulun käynnistäminen*

###<a id="MXF_to_MP4_with_dyn_packaging_more_outputs"></a>Lisäämällä yhden tai useamman muita MP4-tulostus

Tutustu tuloksena Azure Media Services resurssi MP4 kaikkien tiedostojen tukee eri bittitaajuus ja tarkkuus. Työnkulun lisääminen vähintään yksi MP4 tulosteen tiedosto.

Varmista, että kaikki tämän videon koodauslaitteista luotu samoja asetuksia on, on helpointa kaksoiskappaleet aiemmin luodusta AVC videon Encoder ja määrittää toisen yhdistelmä tarkkuutta ja bittitaajuus (lisääminen jokin 960 x 540 25 kehyksissä sekunnissa osoitteessa 2,5 Mbps). Voit kopioida aiemmin encoder, kopioi liittämällä suunnittelutyökalun pinnalla.

Microsoftin uuden AVC-osan yhdistäminen pakkaamattomat videon tulosteen PIN-tunnuksen syöttö Media-tiedosto.

![Toinen AVC encoder yhteydessä](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-avc-encoder-connected.png)

*Toinen AVC encoder yhteydessä*

Nyt mukauttaa Microsoftin uusi AVC encoder siirrettävät 960 x 540 osoitteessa 2,5 Mbps määrityskohde. (Käytä ominaisuudet "tulosteen leveys", "Tulosteen korkeus" ja "Bittitaajuus (kbps)" tätä.)

Annetut haluamme tuloksena resurssi ja Azure Media Services Dynaaminen pakkaus, streaming päätepisteen on pystyttävä luodaan MP4 tiedostojen HLS/Fragmented MP4/VIIVA-osat, jotka tasataan täsmälleen toisiinsa asiakkaat, jotka ovat eri bitrates siirtyminen Hae yksittäisen tasainen jatkuva video ja ääni kokemus tavalla. Voit varmistaa, jotka käynnistyvät, annettava varmistaa, että molemmat AVC koodauslaitteista GOP ("ryhmä kuvat") ominaisuudet sekä MP4-tiedostojen kokoa on määritetty 2 sekunnin ajan, joka voidaan tehdä:

- määrittäminen GOP koko kiinteä GOP koon ja
- Näppäintä kehykseen aikaväli kaksi sekuntia.
- määrittää myös GOP IDR ohjausobjektin suljettu GOP, jotta kaikki GOP ovat pysyvän Omat ilman riippuvuuksista

Voit tehdä tämän työnkulun helposti ymmärtää, nimetä ensimmäisen AVC encoder, jos haluat "AVC Video Encoder 640 x 360 1200kbps" ja toinen AVC encoder "AVC Video Encoder 960 x 540 2500 kbps".

Lisää nyt toinen ISO-MPEG-4-multiplekseri ja toisen tiedoston tulosteen. Uusi AVC encoder multiplekseri yhdistäminen ja varmista, että sen tulostus ohjataan kyselyjä tiedoston tulosteen. Yhdistää sitten AAC ääni encoder tulosteen uuteen multiplekseri 's syöte. Tiedoston tulosteen puolestaan sitten voidaan yhdistää tulosteen tiedostojen tai kohteiden solmun lisääminen Media-palveluiden resurssi, joka luodaan.

![Toinen Muxer ja tuloste yhteydessä](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-second-muxer-file-output-connected.png)

*Toinen Muxer ja tuloste yhteydessä*

Yhteensopivuuden Azure Media Services Dynaaminen pakkaus multiplekseri 's lohko-tilassa ja GOP määrä tai keston määrittäminen ja kohti lohkon GOPs arvoksi 1. (Tämä on oltava oletusarvo.)

![Muxer lohkossa tilat](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-muxer-chunk-modes.png)

*Muxer lohkossa tilat*

Huomautus: Voit Tämä toimenpide on toistettava kaikki muut bittitaajuus ja tarkkuus yhdistelmät haluat lisännyt resurssi tulos.

###<a id="MXF_to_MP4_with_dyn_packaging_conf_output_names"></a>Tiedoston tulosteen nimien määrittäminen

On useampi kuin yksi Yksitiedostoinen tulosteen resurssi lisätään. Tämä on tarpeen ehkä käyttää myös tiedoston nimeäminen konferenssi niin se muuttuu Poista tiedoston nimen, mitä sinun on käsittely ja varmista, että tiedostonimien kunkin tulostus-tiedostot ovat eroavat toisistaan.

Tiedoston tulosteen nimeäminen voi säätää lausekkeita suunnittelussa kautta. Avaa ominaisuusrivin jonkin tiedoston tulosteen osat ja Avaa tiedosto-ominaisuuden Lauseke-editori. Tutustu ensimmäisen kohdetiedosto on määritetty – seuraava lauseke (Katso opetusohjelma siitä [MXF yhden bittitaajuus MP4-tuloste](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4)):

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}.MP4

Tämä tarkoittaa, että Microsoftin tiedostonimi määräytyy kahden muuttujan: kirjoittaa tulostus-hakemistosta ja lähdetiedoston pääkansion nimi. Entisen on ominaisuutena-työnkulun pääkansio ja määritetään saapuvien tiedoston mukaan. Huomaa, että tulostus-kansio on käyttää paikallisen testauksessa; Tämä ominaisuus ohitetaan mukaan työnkulku-ohjelma, kun työnkulku suoritetaan pilvipohjainen media-suoritin Azure Media Services-palveluissa.
Jos haluat antaa sekä Microsoftin tiedostot yhdenmukaisia tulosteen nimeäminen, muuta nimeäminen lauseke ensimmäisen tiedoston:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

ja toinen:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_960x540_2.MP4

Varmista, että molemmat MP4 tulosteen tiedostot on luotu oikein Keskitaso testin suorittaminen.

###<a id="MXF_to_MP4_with_dyn_packaging_audio_tracks"></a>Lisäämällä erillisen ääniraita

Kun olemme tulevat näkyviin myöhemmin, kun olemme Luo .ism-tiedostoa, siirry Microsoftin MP4-tulostus-tiedostoja, on myös osalta pelkän MP4-tiedoston kuin ääniraidan, tutustu mukautuvat streaming. Voit luoda tämän tiedoston, Lisää muita muxer (ISO-MPEG-4 multiplekseri) työnkulun ja Yhdistä AAC encoder tulosteen PIN-tunnuksen sen syötteen PIN-tunnuksella seuraa 1.

![Äänen Muxer lisätty](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-added.png)

*Äänen Muxer lisätty*

Luo kolmannen tuloste osan tulosteen lähtevä virta muxer-ja määritetään tiedoston nimeäminen lauseke:
    
    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_128kbps_audio.MP4

![Äänen Muxer tuloste luominen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-audio-muxer-creating-file-output.png)

*Äänen Muxer tuloste luominen*

###<a id="MXF_to_MP4_with_dyn_packaging_ism_file"></a>Lisäämällä. ISM SMIL tiedosto

Dynaaminen pakkaus MP4 tiedostot (sekä pelkän MP4) Tutustu Media Services resurssi yhdessä käyttöä varten, saat myös annettava luettelon tiedoston (kutsutaan myös "SMIL"-tiedosto: synkronoida Multimedia integrointi kieli). Tämän tiedoston osoittaa Azure Media Services mitä MP4-tiedostot ovat käytettävissä Dynaaminen pakkaus ja mitä niihin, harkitse ääni streaming. Tyypillinen luettelon tiedosto on joukko MP4 henkilön kanssa yhden äänivirtaa näyttää seuraavanlaiselta:
    
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>
    <smil xmlns="http://www.w3.org/2001/SMIL20/Language">
      <head>
        <meta name="formats" content="mp4" />
      </head>
      <body>
        <switch>
          <video src="H264_1900kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_1300kbps_AAC_und_ch2_96kbps.mp4" />
          <video src="H264_900kbps_AAC_und_ch2_96kbps.mp4" />
          <audio src="AAC_ch2_96kbps.mp4" title="AAC_und_ch2_96kbps" />
        </switch>
      </body>
    </smil>

.Ism-tiedosto sisältää sisällä valitsin-lauseke eli viittaus yksittäisiä MP4-videotiedosto ja lisäksi myös (vähintään yksi) äänitiedosto viittaukset MP4, joka sisältää vain ääni.

Tutustu MP4 on joukko luettelon tiedostoa luotaessa voidaan toteuttaa osan nimeltä "AMS luettelon kirjoittajan" kautta. Käyttää sitä, vedä se pinta ja "Kirjoittaa valmis" tulosteen PIN yhdistäminen AMS luettelon Writer Syöte tuloste kolme osaa. Varmista, että muodostaa AMS luettelon Writer tulosteen, tuloste tiedoston tai kohteen.

Tutustu muihin tiedoston tulosteen osiin, määrittää lausekkeen .ism tiedoston tulosteen nimeä:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_manifest.ism

Tutustu valmiit työnkulun näyttää alla:

![Valmis MXF multibitrate MP4 työnkulkuun](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-mxf-to-multibitrate-mp4-workflow.png)

*Valmis MXF multibitrate MP4 työnkulkuun*

##<a id="MXF_to__multibitrate_MP4"></a>Koodaus MXF multibitrate MP4 - parannettu Sinikopio tuominen

[Edellisen työnkulun vaiheittainen kuvaus](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging) Microsoft katsomista miten yhden MXF näytettävä resurssi voidaan muuntaa tulosteen kohteiden usean bittitaajuus MP4-tiedostot, pelkän MP4-tiedosto ja tiedostojen tiedostona käytettäväksi Azure Media Services Dynaaminen pakkaus yhdessä.

Tätä vaiheittaista näkyy, miten joitakin näkökohdat voidaan parannetun ja tehdä tarkoituksenmukaista.

###<a id="MXF_to_multibitrate_MP4_overview"></a>Työnkulun yleiskatsaus parantamiseksi

![Voit parantaa Multibitrate MP4-työnkulku](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-enhance.png)

*Voit parantaa Multibitrate MP4-työnkulku*

###<a id="MXF_to__multibitrate_MP4_file_naming"></a>Tiedostojen nimeämiskäytännöt

Edellisen työnkulku on määritetty yksinkertainen lauseke luonnissa tulosteen tiedostonimet pohjana. Jotkin kaksoisarvot on kuitenkin: kaikki yksittäisten tulosteen tiedoston osien määritetty tällaisten lauseke.

Microsoftin tiedoston tulosteen osan ensimmäisen videotiedostoa on määritetty esimerkiksi seuraava lauseke:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_640x360_1.MP4

Videon toisen tulostusta, varten on lausekkeen, kuten:

    ${ROOT_outputWriteDirectory}\\${ROOT_sourceFileBaseName}_960x540_2.MP4

Eikö olisi selkeämpi vähemmän virhe voi enää ja tarkoituksenmukaista, jos on voitu poistaa joitakin kopiot ja tehdä asioita, Lisää määritettäviä sen sijaan? Kaikeksi emme voi: taulukon suunnitteluohjelmassa lauseke-ominaisuuksia voi luoda mukautettuja ominaisuuksia tämän työnkulun pääkansion yhdessä Anna helppokäyttöisyys parantamiseksi.

Oletetaan, että olemme asema tiedostonimi määritys bitrates yksittäisiä MP4-tiedostoja. Nämä bitrates tarkoituksena keskitetysti (Valitse meidän graph pääkansion), johon ne voi käyttää määrittäminen ja aseman tiedoston nimen luonti määrittäminen. Voit tehdä tämän aluksi julkaisemalla bittitaajuus-ominaisuuden sekä AVC koodauslaitteista tämän työnkulun ylimmällä, kun se ei ole käytettävissä olevat sekä pääkansio sekä AVC koodauslaitteista lähettäjä. (Vaikka näkyviin kaksi eri paikoissa, vieressä on vain yksi hakumääritystä.)

###<a id="MXF_to__multibitrate_MP4_publishing"></a>Osan julkaisuominaisuudet sivulle työnkulun pääkansio

Avaa ensimmäinen AVC encoder, siirry bittitaajuus (kbps)-ominaisuus ja avattavasta valikosta sitten Julkaise.

![Julkaiseminen bittitaajuus-ominaisuus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-bitrate-property.png)

*Julkaiseminen bittitaajuus-ominaisuus*

Määritä Julkaise tämän työnkulun graph ylimmällä Julkaise-valintaikkunan julkaistu nimi, "video1bitrate" ja lukea näyttönimi-Video 1 bittitaajuus". Määrittää mukautetun ryhmänimeä nimeltä "Streaming Bitrates" ja valitse sitten Julkaise.

![Julkaiseminen bittitaajuus-ominaisuus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-bitrate-property.png)

*Julkaisun bittitaajuus ominaisuus-valintaikkuna*

Toista sama toista AVC encoder bittitaajuus-ominaisuus ja anna sille nimi "video2bitrate" ja "Video 2 bittitaajuus" näyttönimi saman mukautetun ryhmän "Streaming Bitrates".

Jos on nyt tutkia työnkulun pääkansion ominaisuudet, emme on Microsoftin mukautetun ryhmän kaksi julkaistun ominaisuuksissa näkyvät. Molemmat kertovat ilmoitukset niiden vastaaviin AVC encoder bittitaajuus arvo.

![Valitse työnkulun pääkansion julkaistun bittitaajuus tarvikkeet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-bitrate-props-on-workflow-root.png)

Aina, kun haluamme käyttää näitä ominaisuuksia koodi tai lausekkeen, emme kiellä tältä:

- Sisäinen koodista osan oikealle pääkansion alla: node.getPropertyAsString('.. / video1bitrate ", null)
- lausekkeessa: ${ROOT_video1bitrate}
 
Suorita "Streaming Bitrates"-ryhmän japanin julkaisemalla Microsoftin ääniraidan bittitaajuus sitä sekä. AAC Encoder ominaisuuksia Etsi bittitaajuus-asetus ja valitse Julkaise avattavasta valikosta sen vieressä. Kaavio ja nimi "audio1bitrate" ylimmällä julkaiseminen ja näyttää Microsoftin mukautetun ryhmän "Streaming Bitrates" nimi "Äänen 1 bittitaajuus".

![Äänen bittitaajuus julkaisun-valintaikkuna](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publishing-dialog-for-audio-bitrate.png)

*Äänen bittitaajuus julkaisun-valintaikkuna*

![Tuloksena olevat ääni- ja tarvikkeet-pääkansio](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-resulting-video-and-audio-props-on-root.png)

*Tuloksena olevat ääni- ja tarvikkeet-pääkansio*

Huomaa, että nämä kolme muuttaminen arvot myös määrittää uudelleen ja muuttaa arvot vastaaviin osia, ne on linkitetty (ja jos julkaistu).

###<a id="MXF_to__multibitrate_MP4_output_files"></a>On luotu nimet ovat riippuvaisia julkaistun ominaisuusarvoihin kohdetiedosto

Sen sijaan, että hardcoding Microsoftin luotu tiedostonimissä on nyt muuttaa kunkin tiedoston tulosteen osat on julkaistu vain graph pääkansio bittitaajuus ominaisuudet ovat riippuvaisia Microsoftin tiedostonimi-lauseke. Tutustu ensimmäisen tuloste alkaen tiedosto-ominaisuus ja Muokkaa lausekkeen tältä:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video1bitrate}kbps.MP4

Parametrien lausekkeessa voidaan käyttää ja syötetty osumalla dollarimerkki lauseke-ikkuna-näppäinyhdistelmää. Jokin käytettävissä parametrit on Microsoftin video1bitrate-ominaisuutta, joka on aiemmin julkaistu.

![Lausekkeen parametrien käyttäminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-accessing-parameters-within-an-expression.png)

*Lausekkeen parametrien käyttäminen*

Tee sama tiedoston tulosteen sekä toinen video:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_video2bitrate}kbps.MP4

ja pelkän tiedoston tulostusta varten:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_${ROOT_audio1bitrate}bps_audio.MP4

Jos muutamme bittitaajuus minkään videon tai äänen tiedostot nyt vastaaviin encoder määritetään uudelleen ja bittitaajuus-pohjaisen tiedoston nimi-konferenssi noudatetaan kaikkien automaattisten.

##<a id="thumbnails_to__multibitrate_MP4"></a>Pikkukuvien lisääminen multibitrate MP4-tulostus

Lähtien työnkulun, joka luo [multibitrate MP4 MXF tulosteen syöte](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)on nyt voi esittää kyselyjä pikkukuvien lisääminen tulos.

###<a id="thumbnails_to__multibitrate_MP4_overview"></a>Voit lisätä pikkukuvat työnkulun yleiskatsaus

![Multibitrate MP4-työnkulun käynnistäminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-multibitrate-mp4-workflow-to-start-from.png)

*Multibitrate MP4-työnkulun käynnistäminen*

###<a id="thumbnails_to__multibitrate_MP4__with_jpg"></a>Lisäämällä JPG-koodaus

Tutustu pikkukuvan luonti sydän on JPG Encoder-osan, voit siirtää JPG-tiedostoja.

![JPG Encoder](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-jpg-encoder.png)

*JPG Encoder*

Microsoft ei voi kuitenkin suoraan yhteyden Microsoftin pakkaamattomat videovirtaa Media tiedoston syötteen JPG-encoder kyselyjä. Sen sijaan odottaa antaa yksittäiset kehykset. Emme voi tehdä videon kehys portti-osan kautta.

![Yhteyden muodostaminen JPG-encoder kehyksen portti](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-frame-gate-to-jpg-encoder.png)

*Yhteyden muodostaminen JPG-encoder kehyksen portti*

Kehyksen portin niin paljon sekunnin välein tai kehysten avulla välittää videokehyksen. Aikavälin ja siirtymä-ja jotka näin tapahtuu aika on määritettäviä ominaisuuksia.

![Videon kehys Portin ominaisuudet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-video-frame-gate-properties.png)

*Videon kehys Portin ominaisuudet*

Luo pikkukuvan minuutin välein määrittämällä tila aika (sekunteina) ja 60 aikaväli.

###<a id="thumbnails_to__multibitrate_MP4_color_space"></a>Väri-tilaa muuntaminen käsittely

Vaikka näyttää looginen sekä pakkaamattomat Video PIN kehyksen portin ja Media-tiedosto syötteen nyt voidaan yhdistää, on saisit varoitus, jos olemme tehdä.

![Värin tilaa virhe](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-input-color-space-error.png)

*Värin tilaa virhe*

Tämä johtuu siitä, mitä väri tiedot esitetään Tutustu alkuperäisen raaka pakkaamattomat videovirtaa, tutustu MXF tulevat tapaa, jolla ei ole sama kuin JPG-Encoder odottaa. Tarkemmin sanottuna niin "väri välilyöntiä" "RGB" tai "harmaasävy-odotetaan kulkevan. Tämä tarkoittaa, että videon kehys-portin saapuvien videovirtaa on pystyttävä muodostamaan muunto ensin käytetään koskeva sen väri-tilaa.

Vedä työnkulun tilaa väri - muunnin Intel ja yhdistä se Microsoftin kehyksen portti.

![Väri-tilaa muuntimen yhdistäminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-connect-color-space-convertor.png)

*Väri-tilaa muuntimen yhdistäminen*

Valitse ominaisuudet-ikkunassa BGR 24 tapahtuma esiasetus-luettelosta.

###<a id="thumbnails_to__multibitrate_MP4_writing_thumbnails"></a>Kirjoittaminen pikkukuvat

Eroaa Microsoftin MP4-video JPG Encoder-osan siirtää useita tiedostoja. Jotta voit käsitellä tämän näkymän haun JPG tiedoston Writer-osan avulla voidaan: se ottaa saapuvat JPG-pikkukuvat ja kirjoittaa ne, että jälkiliitteeksi eri luvulla tiedostonimet. (Luku, joka osoittaa yleensä sekuntia/yksiköiden määrän, jonka pikkukuva on piirretty-muodossa-.)


![Näkymän haun JPG tiedoston Writer esittely](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer.png)

*Näkymän haun JPG tiedoston Writer esittely*

Tulosteen kansiopolku-ominaisuuden määrittäminen lauseketta: ${ROOT_outputWriteDirectory} 

ja tiedostonimi etuliite ominaisuus: 

    ${ROOT_sourceFileBaseName}_thumb_

Etuliitteen määrittävät, kuinka pikkukuvan tiedostot nimetään. Ne jälkiliitteeksi luvun, joka ilmaisee virta napin paikassa.


![Näkymän haun JPG tiedoston Writer-ominaisuudet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scene-search-jpg-file-writer-properties.png)

*Näkymän haun JPG tiedoston Writer-ominaisuudet*

Yhdistä näkymän haun JPG tiedoston Writer tulosteen tiedoston/resurssi-solmu.

###<a id="thumbnails_to__multibitrate_MP4_errors"></a>Työnkulun virheiden havaitseminen

Yhdistä värin tilaa muuntimen syötteen raaka pakkaamattomat videon tulos. Nyt suorittaa työnkulun suorittaminen paikallisen testi. On hyvinkin työnkulku yhtäkkiä lopettaa suoritetaan ja määritä, punainen ääriviivalla-osa, joka havaitsi virheen:

![Värin tilaa muuntimen virhe](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error.png)

*Värin tilaa muuntimen virhe*

Napsauta oikeassa alakulmassa värin tilaa muuntimen osan syy koodauksen yrityksellä on artikkelissa epäonnistui ylälaidassa pieni punainen "E"-kuvaketta.

![Tilaa muuntimen virhevalintaikkuna](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-color-space-converter-error-dialog.png)

*Tilaa muuntimen virhevalintaikkuna*

Se muuttuu, kuten näet, että vakio värin tilaa muuntimen saapuvan väri-tilan on oltava Microsoftin pyydetty muuntaminen YUV RGB rec601. Tutustu stream ei ilmeisesti osoittavat sen rec601. (Tietueen 601 nro on standardin koodaus lomitettu analogiset videon signaalit digitaalisessa videon muodossa. Määrittää active alue, joka on 720 kirkkaus-mallit ja 360 chrominance näytteiden riviä kohden. Koodaus järjestelmän väriä käytetään nimitystä YCbCr 4:2:2.)

Voit korjata ongelman olemme ilmaista metatietoja, tutustu muodossa, joka on olet käsittely rec601 sisällön. Voit tehdä käytetään Video tietojen tyypin Updater komponentti, joka on täytyy kirjoittaa Microsoftin raaka lähde- ja väri tilaa muunto-osan välissä. Tämä tietojen tyypin updater avulla manuaalisen päivityksen videon tiettyjen tietojen tyypin ominaisuudet. Määritä se osoittamaan värin tilaa vakio, "Tietueen nro 601". Tämä aiheuttaa Video tietojen tyypin asennusohjelman merkitseminen muodossa "Tietueen nro 601" väri-tilaa, jos ei ollut ei ole vielä määritetty väri-tilaa. (Se ei ole ohittaa kaikki olemassa olevat metatiedot, ellei ohitus-valintaruutu on valittuna.)

![Tietojen tyypin asennusohjelman värin tilaa standardin päivittäminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-update-color-space-standard-on-data-type.png)

*Tietojen tyypin asennusohjelman värin tilaa standardin päivittäminen*

###<a id="thumbnails_to__multibitrate_MP4_finish"></a>Valmiit työnkulun

Kun sekä Microsoftin työnkulku on valmis, toisen testin vahvistuksen välittää Suorita.

![Valmiit työnkulun usean mp4 tulostukseen pikkukuvien kanssa](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-for-multi-mp4-thumbnails.png)

*Valmiit työnkulun usean mp4 tulostukseen pikkukuvien kanssa*

##<a id="time_based_trim"></a>Aikapohjaisten rajauksen multibitrate MP4-tulostus

Lähtien työnkulun, joka luo [multibitrate MP4 MXF tulosteen syöte](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)on nyt voidaan esittää kyselyjä aikaleimoja perusteella lähteen videon rajaaminen.

###<a id="time_based_trim_start"></a>Työnkulun yleiskatsaus jatkaa lisäämällä rajaamisen avulla

![Rajaaminen, jos haluat lisätä työnkulun käynnistäminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-starting-workflow-to-add-trimming.png)

*Rajaaminen, jos haluat lisätä työnkulun käynnistäminen*

###<a id="time_based_trim_use_stream_trimmer"></a>Virta-suojauskatkaisutoteutuksen käyttäminen

Stream suojauskatkaisutoteutuksen-osan avulla voit leikata alku ja loppu syötteen stream perustuvan ajoittamisen tiedot (sekunnit, minuuttia,...). Viimeistelyleikkuri ei tue kehys-pohjainen rajaamisen.

![Virta suojauskatkaisutoteutuksen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-stream-trimmer.png)

*Virta suojauskatkaisutoteutuksen*

Sen sijaan, että linkin AVC kooderit ja kaiuttimen sijainti assigner Media tiedoston syötteen suoraan, on sijoittaa ne välissä stream suojauskatkaisutoteutuksen. (Yksi videon signaalin ja yksi limitetty ääni signaalin.)

![Sijoita muodossa suojauskatkaisutoteutuksen välillä](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-put-stream-trimmer-in-between.png)

*Sijoita muodossa suojauskatkaisutoteutuksen välillä*

Määritä viimeistelyleikkuri japanin niin, että olemme vain käsittelee videon ja äänen 15 sekunnin kuluttua-videon 60 sekuntia.

Siirry videon Stream suojauskatkaisutoteutuksen ominaisuudet ja määritä alkamisaika (15 s)- ja päättymisaika (60s) ominaisuudet. Varmista, että sekä ääni- ja tutustu suojauskatkaisutoteutuksen määritetään aina samat alkamis- ja end-arvoja, on voivat julkaista ne työnkulun pääkansioon.

![Käynnistä aika ominaisuuden Stream suojauskatkaisutoteutuksen julkaiseminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-start-time-from-stream-trimmer.png)

*Käynnistä aika ominaisuuden Stream suojauskatkaisutoteutuksen julkaiseminen*

![Julkaise ominaisuusikkuna aloitusaika](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-start-time.png)

*Julkaise ominaisuusikkuna aloitusaika*

![Julkaise ominaisuusikkuna päättymisaika](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-publish-dialog-for-end-time.png)

*Julkaise ominaisuusikkuna päättymisaika*


Jos on nyt tutkia tämän työnkulun ylimmällä, molempia ominaisuuksia ovat näkyvissä ja määritettävä siististi sieltä.

![Julkaistun ominaisuudet, jotka ovat käytettävissä pääkansio](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-published-properties-available-on-root.png)

*Julkaistun ominaisuudet, jotka ovat käytettävissä pääkansio*

Nyt rajaamisen ominaisuudet avaaminen ääni suojauskatkaisutoteutuksen ja määritä alkamis-ja end lauseke, joka viittaa tämän työnkulun ylimmällä julkaistun ominaisuudet.

Äänen rajaaminen alkamisaika:

    ${ROOT_TrimmingStartTime}

ja sen päättymisaika:

    ${ROOT_TrimmingEndTime}

###<a id="time_based_trim_finish"></a>Valmiit työnkulun

![Valmiit työnkulun](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-finished-workflow-time-base-trimming.png)

*Valmiit työnkulun*


##<a id="scripting"></a>Komentosarjaan määritetty osan esittely

Komentosarjaan määritetty osia voit suorittaa haluamaansa komentosarjojen suorittamisen vaiheissa tämän työnkulun. Neljä eri komentosarjoja voi suorittaa, joissa tiettyjä ominaisuuksia ja oman työnkulun elinkaaren kohta:

- **commandScript**
- **realizeScript**
- **processInputScript**
- **lifeCycleScript**

Ohjeissa komentosarjatoimintojen osan oleviin tarkemmin kunkin edellä mainituista. [Seuraavassa osassa](media-services-media-encoder-premium-workflow-tutorials.md#frame_based_trim) **realizeScript** komentosarja-osa käytetään muodostaa cliplist xml suoraan selaimessa, kun työnkulku alkaa. Tämä komentosarja kutsutaan osan asetukset, jotka tapahtuu vain kerran sen elinkaaren aikana.


###<a id="scripting_hello_world"></a>Komentosarjan työnkulun: Hei

Vedä komentosarjatoimintojen osan suunnittelutyökalun pinta ja nimetä sen (esimerkiksi "SetClipListXML").

![Komentosarjaan määritetty komponentin lisääminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Komentosarjaan määritetty komponentin lisääminen*

Kun tarkistat komentosarjatoimintojen-osan ominaisuuksia, eri komentosarjan tyyppejä on näkyvissä, jokainen määritettäviä eri komentosarjan.

![Komentosarjaan määritetty osan ominaisuudet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Komentosarjaan määritetty osan ominaisuudet*

Poista processInputScript ja avaa realizeScript-editori. Microsoft on nyt määritetty ja jatkaa komentosarjan.

Komentosarjojen kirjoitetaan Groovy dynaamisesti käännetty komentosarjakielen Java-ympäristö, joka säilyttää Java yhteensopivuus. Kyllä useimmat Java-koodi on kelvollinen Groovy koodi.

Kirjoita japanin yksinkertainen Hei maailma groovy komentosarjan meidän realizeScript kontekstissa. Kirjoita editorin seuraavasti:

    node.log("hello world");

Nyt suorittaa paikallisen testin suorittaminen. Kyseisen esiintymän jälkeen Tarkasta (kautta komentosarjatoimintojen komponentin järjestelmä-välilehti) lokitiedot-ominaisuutta.

![Hei maailma log tulostus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output.png)

*Hei maailma log tulostus*

Solmu-objekti on kutsua lokitiedostojen-menetelmää, viittaa Microsoftin nykyinen "solmu" tai osan emme ole komentosarjan kuluessa. Jokaisen osan on sellaisenaan voi siirtää kirjaaminen tietoja, järjestelmä-välilehden kautta. Tässä tapauksessa emme tulosteen merkkijonon literaalimerkit "Hei". Tärkeää ymmärtää näin, että tämä todista on erittäin hyödyllinen toiminto tutkittaessa, mitä komentosarja tietoja käyttämiseen todella jokaisen.

Valitse Microsoftin komentosarjat ympäristössä myös on ominaisuuksien käyttö muita osia. Kokeile seuraavaa:


    //inspect current node: 
    def nodepath = node.getNodePath(); 
    node.log("this node path: " + nodepath);
    
    //walking up to other nodes: 
    def parentnode = node.getParentNode(); 
    def parentnodepath = parentnode.getNodePath(); 
    node.log("parent node path: " + parentnodepath);
    
    //read properties from a node: 
    def sourceFileExt = parentnode.getPropertyAsString( "sourceFileExtension", null ); 
    def sourceFileName = parentnode.getPropertyAsString("sourceFileBaseName", null); 
    node.log("source file name with extension " + sourceFileExt + " is: " + sourceFileName);

Tutustu log-ikkunassa näkyvät us seuraavasti:

![Raportin avaaminen solmun polut](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-output2.png)

*Raportin avaaminen solmun polut*


##<a id="frame_based_trim"></a>Kehys-pohjainen rajauksen multibitrate MP4-tulostus

Lähtien työnkulun, joka luo [multibitrate MP4 MXF tulosteen syöte](media-services-media-encoder-premium-workflow-tutorials.md#MXF_to_MP4_with_dyn_packaging)on nyt käyttävät kyselyjä rajaaminen lähteen videon kehys laskee perusteella.

###<a id="frame_based_trim_start"></a>Sinikopio jatkaa lisäämällä rajaamisen, yleiskatsaus

![Työnkulku käynnistyy lisääminen rajaamisen avulla](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-workflow-start-adding-trimming-to.png)

*Työnkulku käynnistyy lisääminen rajaamisen avulla*

###<a id="frame_based_trim_clip_list"></a>Valitse ClipArt-luettelosta XML

Kaikki edellisen työnkulun opetusohjelmia on käytetty Media tiedoston syötteen osaa kuin Microsoftin videon lähde. Tämän tietyn skenaarion, videossa käytetään ClipArt luetteloon lähde-osan sijaan. Huomaa, että tämä tulee toimia; manuaalisten Käytä Clip luettelon lähteen vain, kun reaali syytä kiellä (tavalla kirjainkoko, jossa on tarra alla ClipArt luettelon rajaamisen ominaisuuksien käyttö).

Siirtyminen Microsoftin Clip luettelon lähteen Media tiedoston syötteen (ClipArt luetteloon lähde) komponentti vetäminen suunnitteluosaan ja Yhdistä ClipArt-luettelossa XML PIN-tunnuksen työnkulun suunnittelun ClipArt-luettelossa XML-solmu. Tämä tulee täyttää ClipArt luettelon lähteen kanssa tulosteen PIN-video Microsoftin input mukaan. Nyt yhdistää pakkaamattomat videon ja äänen-PIN-ClipArt-kuvan luettelon lähteen vastaaviin AVC koodauslaitteista ja äänen Stream Interleaver. Nyt poistaa syötteen Media-tiedosto.

![Media-tiedosto syötteen korvattu ClipArt-luettelosta lähde](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-replaced-media-file-with-clip-source.png)

*Media-tiedosto syötteen korvattu ClipArt-luettelosta lähde*

ClipArt-luettelosta lähde-osan kestää sen syötteeksi "ClipArt luettelo-XML. Testaa paikallisesti lähdetiedoston valittaessa tämä ClipArt-luettelossa xml on tiedot määräytyvät automaattisesti puolestasi.

![Tiedot määräytyvät automaattisesti ClipArt-luettelossa XML-ominaisuus](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-auto-populated-clip-list-xml-property.png)

*Tiedot määräytyvät automaattisesti ClipArt-luettelossa XML-ominaisuus*

Etkö löytänyt hieman lähempänä xml-tämä on siitä, miten se näyttää:

![Muokkaa ClipArt-luettelo-valintaikkuna](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-edit-clip-list-dialog.png)

*Muokkaa ClipArt-luettelo-valintaikkuna*

Tämä ei kuitenkaan kuvasta ClipArt-luettelossa xml: n ominaisuuksia. On yksi vaihtoehto on Lisää "Rajaaminen"-osan kohdassa sekä ääni- ja lähde-tältä:

![Poista elementti lisääminen ClipArt-luetteloon](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-adding-trim-element-to-clip-list.png)

*Poista elementti lisääminen ClipArt-luetteloon*

Jos muokkaat ClipArt luettelon xml tältä ja suorittaa paikallisen Suorita testi, näet videon oikein on rajattu videon 10 – 20 sekuntia.

Toisin kuin mitä tapahtuu, kun teet paikallisen Suorita vaikka tämä hyvin samalla cliplist xml ei ole saman tuloksen, kun käyttöön työnkulun, joka suoritetaan Azure Media Services-palveluissa. Azure Premium Encoder käynnistyessä cliplist xml luodaan aina, kun koodauksen työn annettiin lähdetiedoston uudelleen, perusteella. Tämä tarkoittaa, että kaikki muutokset, että Älä xml-valitettavasti ohittaa.

Kuittaamaan parhaillaan tietojen poiston yhteydessä koodauksen työn käynnistyessään cliplist xml on uudelleen se voidaan luoda suoraan selaimessa vain tämän työnkulun alkamisen jälkeen. Näiden mukautettuja toimintoja voidaan ottaa kautta mitä kutsutaan "Komentosarjatoimintojen osan". Lisätietoja on artikkelissa [komentosarjatoimintojen-osan esittely](media-services-media-encoder-premium-workflow-tutorials.md#scripting).


Vedä sivulle suunnittelutyökalun pinta komentosarjatoimintojen ja nimeksi "SetClipListXML".

![Komentosarjaan määritetty komponentin lisääminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-scripted-comp.png)

*Komentosarjaan määritetty komponentin lisääminen*

Kun tarkistat komentosarjatoimintojen-osan ominaisuuksia, eri komentosarjan tyyppejä on näkyvissä, jokainen määritettäviä eri komentosarjan.

![Komentosarjaan määritetty osan ominaisuudet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-scripted-comp-properties.png)

*Komentosarjaan määritetty osan ominaisuudet*


###<a id="frame_based_trim_modify_clip_list"></a>ClipArt-luettelon luominen komentosarjatoimintojen osan muokkaaminen

Ennen kuin olemme kirjoittaa uudelleen, joka on luotu työnkulun käynnistyksen aikana cliplist xml-olemme on käyttää cliplist xml-ominaisuutta ja sisältö. Emme voi tehdä tältä:

    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: " + clipListXML);

![Kun olet kirjautunut saapuvien leikeluettelon](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-incoming-clip-list-logged.png)

*Kun olet kirjautunut saapuvien leikeluettelon*

Ensin annettava voi määrittää mitä pisteestä tähän päivään, jolloin haluamme rajaa video. Voit suorittaa helposti työnkulun pienempi tekniset käyttäjälle, julkaista kaavion ylimmällä kaksi ominaisuudet. Voit tehdä tämän suunnittelutyökalun pinta napsauttamalla hiiren kakkospainikkeella ja valitse "Lisää ominaisuus":

- Ensimmäinen ominaisuus: "ClippingTimeStart" tyyppi: "AIKAKOODI"
- Toinen ominaisuus: "ClippingTimeEnd" tyyppi: "AIKAKOODI"

![Lisää näyttöleikkeen alkamisaika ominaisuusikkuna](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-start-time.png)

*Lisää näyttöleikkeen alkamisaika ominaisuusikkuna*

![Julkaistu leikkauksen aika tarvikkeet-työnkulun pääkansio](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-clip-time-props.png)

*Julkaistu leikkauksen aika tarvikkeet-työnkulun pääkansio*

Sopivan arvon sekä ominaisuuksien määrittäminen:

![Määritä näyttöleikkeen alkamis- ja loppumiskohdat ominaisuudet](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-configure-clip-start-end-prop.png)

*Määritä näyttöleikkeen alkamis- ja loppumiskohdat ominaisuudet*

Nyt-kuluessa sekä komentosarjan emme voi käyttää sekä ominaisuudet seuraavasti:

    
    // get start and end of clipping:
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    node.log("clipping start: " + clipstart);
    node.log("clipping end: " + clipend);

![Log-ikkuna, jossa näkyy alussa ja lopussa leikkeen lisääminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-show-start-end-clip.png)

*Log-ikkuna, jossa näkyy alussa ja lopussa leikkeen lisääminen*

Oletetaan, että jäsentää aikakoodi merkkijonot tarkoituksenmukaista lomakkeella, käyttämällä yksinkertaista säännöllinen lauseke:
    
    //parse the start timing: 
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1); 
    node.log("timecode start is: " + starttimecode); 
    def startframerate = startregresult.group(2); 
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend); 
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2); 
    node.log("framerate end is: " + endframerate);

![Lokitiedoston, joissa on tulosteen jäsennetyn aikakoodi](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-output-parsed-timecode.png)

*Lokitiedoston, joissa on tulosteen jäsennetyn aikakoodi*

Kätevästi nämä ohjeet on nyt muokata cliplist xml vastaamaan haluamasi kehyksen tarkkoja näyttöleikkeen elokuvan alkamis- ja päättymisajat.

![Komentosarjakoodin Poista.välit elementtien lisääminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-add-trim-elements.png)

*Komentosarjakoodin Poista.välit elementtien lisääminen*

Tämä on tehty – Normaali merkkijonotoiminnoista käytöstä. Tuloksena muokatun ClipArt-kuvan luettelon xml kirjoitetaan takaisin clipListXML-ominaisuus-työnkulun pääkansio kautta "setProperty-menetelmää. Log-ikkunassa toisen testin suorittamisen jälkeen näkyy us seuraavasti:

![Tuloksena leikeluettelon lokiin kirjaaminen](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-log-result-clip-list.png)

*Tuloksena leikeluettelon lokiin kirjaaminen*

Tee testi-asennuksen näet, miten video- ja äänivirtaa on ollut pois. Tapaan useampi kuin yksi testi-asennuksen eri arvoilla rajaamisen pisteet, huomaat, että ne ei oteta huomioon kuitenkin! Syy on, että designer toisin kuin Azure runtime Ohita cliplist xml jokaisen Suorita. Tämä tarkoittaa, että vain ensimmäisen kerran, olet määrittänyt ja pisteiden ulos aiheuttaa xml transform, kaikki muut kertaa usealle suojaa-lause (Jos (clipListXML.indexOf ("<trim>") = -1)) estää työnkulun lisäämästä toisen Poista.välit-elementin, kun kyseessä on jo jokin esitä.

Voit tehdä tämän työnkulun helposti Testaa paikallisesti, on parasta lisäämällä talon säilyttämisen lisäkoodin, jos Poista.välit elementti on jo olemassa tarkastaa. Jos näin on, Microsoft poistaa ennen jatkamista muokkaamalla xml uusilla arvoilla. Tavallinen merkkijonon yhdistämismäärityksiä sijaan on todennäköisesti suojaaminen toiminto jäsennyksen reaali xml-objektimallin kautta.

Ennen kuin emme voi lisätä koodin, vaikka emme on lisättävä Tuontilauseet useita Microsoftin komentosarjan alkuun ensin:
    
    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;

Tämän jälkeen voi lisätä tarvittavat puhdistus koodi:

    //for local testing: delete any pre-existing trim elements from the clip list xml by parsing the xml into a DOM:
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder();
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already: 
    XPath xpath = XPathFactory.newInstance().newXPath();
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements,dom,XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }

    node.log("about to delete any existing trim nodes");
     //delete the trim nodes: 
    elementsToDelete.each{ 
        e -> e.getParentNode().removeChild(e);
    }; 
    node.log("deleted any existing trim nodes");
    
    //serialize the modified clip list xml dom into a string: 
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result); 
    clipListXML = result.getWriter().toString();
    
Tämä koodi siirtyy yläpuolella kohtaan, jossa lisäämme Poista.välit elementtien cliplist XML-muotoon.

Tässä vaiheessa on Suorita ja muokata tämän työnkulun kuin lähes samalla tavalla kuin haluamme aikana joskus tehdyistä muutoksista aika.    

###<a id="frame_based_trim_clippingenabled_prop"></a>Lisäämällä ClippingEnabled helppokäyttöisyys-ominaisuus

Kuin ei aina haluat ehkä tehtävä rajaaminen, japanin valmis tämän työnkulun käytöstä lisäämällä helposti totuusarvo lippu, joka ilmaisee, onko haluamme rajaaminen / leikkauksen käyttöön.

Samalla tavalla kuin, Julkaise uusi ominaisuus pääkansion Microsoftin työnkulku-kohdat "ClippingEnabled" tyyppi "totuusarvo".

![Julkaistu ominaisuuden näyttöleikkeen ottaminen käyttöön](./media/media-services-media-encoder-premium-workflow-tutorials/media-services-enable-clip.png)

*Julkaistu ominaisuuden näyttöleikkeen ottaminen käyttöön*

Ja alapuolella yksinkertainen suojaa-lauseen, on tarkistaa rajaamisen tarvitaanko ja päättää, jos ClipArt-luettelossamme sellaisenaan on muokattava vai ei.

    //check if clipping is required: 
    def clippingrequired = node.getProperty("../ClippingEnabled"); 
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML); 
        node.log("no clipping required"); 
        return; 
    }


###<a id="code"></a>Valmis koodi

    import javax.xml.parsers.*; 
    import org.xml.sax.*; 
    import org.w3c.dom.*;
    import javax.xml.*;
    import javax.xml.xpath.*; 
    import javax.xml.transform.*; 
    import javax.xml.transform.stream.*; 
    import javax.xml.transform.dom.*;
    
    // get cliplist xml: 
    def clipListXML = node.getProperty("../clipListXml");
    node.log("clip list xml coming in: \n" + clipListXML);
    // get start and end of clipping: 
    def clipstart = node.getProperty("../ClippingTimeStart").toString();
    def clipend = node.getProperty("../ClippingTimeEnd").toString();
    
    //parse the start timing:
    def startregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipstart); 
    startregresult.matches(); 
    def starttimecode = startregresult.group(1);
    node.log("timecode start is: " + starttimecode);
    def startframerate = startregresult.group(2);
    node.log("framerate start is: " + startframerate);
    
    //parse the end timing: 
    def endregresult = (~/(\d\d:\d\d:\d\d:\d\d)\/(\d\d)/).matcher(clipend);
    endregresult.matches(); 
    def endtimecode = endregresult.group(1); 
    node.log("timecode end is: " + endtimecode); 
    def endframerate = endregresult.group(2);

    node.log("framerate end is: " + endframerate);
    
    //for local testing: delete any pre-existing trim elements 
    //from the clip list xml by parsing the xml into a DOM:
    
    DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
    DocumentBuilder builder=factory.newDocumentBuilder(); 
    InputSource is=new InputSource(new StringReader(clipListXML)); 
    Document dom=builder.parse(is);

    //find the trim element inside videoSource and audioSource and remove it if it exists already:
    XPath xpath = XPathFactory.newInstance().newXPath(); 
    String findAllTrimElements = "//trim"; 
    NodeList trimelems = xpath.evaluate(findAllTrimElements, dom, XPathConstants.NODESET);

    //copy trim nodes into a "to-be-deleted" collection 
    Set<Element> elementsToDelete = new HashSet<Element>(); 
    for (int i = 0; i < trimelems.getLength(); i++) { 
        Element e = (Element)trimelems.item(i); 
        elementsToDelete.add(e); 
    }
    
    node.log("about to delete any existing trim nodes");
    //delete the trim nodes:
    elementsToDelete.each{ e -> 
        e.getParentNode().removeChild(e); 
    };
    node.log("deleted any existing trim nodes");

    //serialize the modified clip list xml dom into a string:
    def transformer = TransformerFactory.newInstance().newTransformer();
    StreamResult result = new StreamResult(new StringWriter());
    DOMSource source = new DOMSource(dom);
    transformer.transform(source, result);
    clipListXML = result.getWriter().toString();

    //check if clipping is required:
    def clippingrequired = node.getProperty("../ClippingEnabled");
    node.log("clipping required: " + clippingrequired.toString()); 
    if(clippingrequired == null || clippingrequired == false) 
    {
        node.setProperty("../clipListXml",clipListXML);
        node.log("no clipping required");
        return; 
    }

    //add trim elements to cliplist xml 
    if ( clipListXML.indexOf("<trim>") == -1 ) 
    {
        //trim video 
        clipListXML = clipListXML.replace("<videoSource>","<videoSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\"" + endframerate +"\"> " + endtimecode + 
            " </outPoint>\n </trim> \n"); 
        //trim audio 
        clipListXML = clipListXML.replace("<audioSource>","<audioSource>\n <trim>\n <inPoint fps=\""+ 
            startframerate +"\">" + starttimecode + 
            "</inPoint>\n" + "<outPoint fps=\""+ endframerate +"\">" + 
            endtimecode + "</outPoint>\n </trim>\n");
        node.log( "clip list going out: \n" +clipListXML ); 
        node.setProperty("../clipListXml",clipListXML); 
    }


##<a name="also-see"></a>Katso myös 

[Esittely Premium koodaus Azure Media-palvelut](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

[Opi käyttämään Premium koodaus Azure Media-palvelut](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)

[Koodaus tarvittaessa sisältöä Azure Media-palvelun kanssa](media-services-encode-asset.md#media_encoder_premium_workflow)

[Media Encoder Premium työnkulun muotoja ja pakkauksenhallinnat](media-services-premium-workflow-encoder-formats.md)

[Työnkulun mallitiedostojen](http://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)

[Azure Media Services Explorer-työkalu](http://aka.ms/amse)

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
