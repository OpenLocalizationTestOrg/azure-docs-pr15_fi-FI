<properties 
    pageTitle="Lähetä yksittäinen bittitaajuus live stream Alkuainemuodossa Live-encoder määrittäminen | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään Alkuainemuodossa Live encoder yksittäisen bittitaajuus stream lähettäminen AMS, jotka on otettu käyttöön live koodaus kanavien määrittämisestä." 
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
    ms.date="10/12/2016"
    ms.author="cenkdin;anilmur;juliako"/>

#<a name="use-the-elemental-live-encoder-to-send-a-single-bitrate-live-stream"></a>Alkuainemuodossa Live encoder avulla voit lähettää yksittäisen bittitaajuus live muodossa

> [AZURE.SELECTOR]
- [Alkuaineina Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)
- [FMLE](media-services-configure-fmle-live-encoder.md)

Tässä ohjeaiheessa esitellään [Alkuainemuodossa Live](http://www.elementaltechnologies.com/products/elemental-live) encoder yksittäisen bittitaajuus stream lähettäminen AMS, jotka on otettu käyttöön live koodaus kanavien määrittämisestä.  Lisätietoja on artikkelissa [käyttäminen kanavat, jotka ovat käytössä suorittaa Live koodaus Azure Media-palvelujen kanssa](media-services-manage-live-encoder-enabled-channels.md).

Tässä opetusohjelmassa näytetään, miten voit hallita Azure Media Services (AMS) Azure Media Services Explorer (AMSE)-työkalulla. Tämä työkalu suorittaa vain Windows-tietokoneessa. Jos käytössäsi on Mac tai Linux, Azure portaalin avulla voit luoda [kanavat](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ja [Ohjelmat](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

##<a name="prerequisites"></a>Edellytykset

- On oltava kokemusta luominen tapahtumia Alkuainemuodossa Live web-liittymän avulla.
- [Azure Media Services-tilin luominen](media-services-portal-create-account.md)
- Varmista on vähintään yhden streaming yksikön kohdistettu käyttämisestä Streaming päätepiste. Lisätietoja on artikkelissa [Hallitse Streaming päätepisteet Media Services-tilin](media-services-portal-manage-streaming-endpoints.md).
- Asenna uusin versio [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) -työkalun.
- Käynnistä työkalu ja muodostaa yhteyden tiliisi AMS.

##<a name="tips"></a>Vihjeitä

- Käytä mahdollisuuksien mukaan tavallisen internet-yhteys.
- Hyvä säännön of napin-lyncserver2010 määritettäessä on kaksoisnapsauttamalla streaming bitrates. Samalla, kun tämä ei ole pakollinen vaatimus, se auttaa pienentämään verkon kuormituksen vaikutus.
- Kun ohjelmistoa käyttämällä perusteella koodauslaitteista, sulkea tarpeettomat ohjelmat.

## <a name="elemental-live-with-rtp-ingest"></a>Alkuainemuodossa Live kanssa RTP ingest

Tässä osassa näytetään, miten voit määrittää Alkuainemuodossa Live encoder, joka lähettää yhden bittitaajuus live stream RTP päälle.  Lisätietoja on artikkelissa [MPEG TS stream RTP päälle](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Kanavan luominen

1.  AMSE-työkalun Siirry **Live** -välilehti ja kanava-alueella Napsauta hiiren kakkospainikkeella. Valitse **Luo kanava...** Valitse valikosta.

![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Määritä kanavan nimi, kuvauskenttä on valinnainen. Kanavan asetukset-kohdassa **Vakio** Live koodaus-asetuksen arvoksi **RTP (MPEG-TS)**Input-protokollan kanssa. Voit jättää kaikki muut asetukset on.


Varmista, että **Aloita uusi kanava nyt** on valittuna.

3. Valitse **Luo kanava**.
![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

>[AZURE.NOTE] Kanavan voi kestää niin kauan kuin 20 minuutin.

Kun kanava käynnistyy voit [määrittää encoder](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

>[AZURE.IMPORTANT] Huomaa, että Laskutus alkaa heti, kun kanavan tiedosto on valmis. Lisätietoja on artikkelissa [kanavan hyötyä](media-services-manage-live-encoder-enabled-channels.md#states).

###<a id=configure_elemental_rtp></a>Määritä Alkuainemuodossa Live-koodaus 

Tässä opetusohjelmassa käytetään seuraavia tulostus-asetuksia. Muiden tässä osassa kuvataan tarkemmin määritysvaiheet. 

**Video**:
 
- Pakkauksenhallinnan: H.264 
- Profiili: High (tason 4.0) 
- Bittitaajuus: 5000 kbps 
- Avainkuva: kahden sekunnin (60 sekuntia) 
- Kehys korko: 30
 
**Äänen**:

- Pakkauksenhallinnan: AAC (LC) 
- Bittitaajuus: 192 kbps 
- Esimerkki korko: 44,1 kHz


####<a name="configuration-steps"></a>Määritysvaiheet

1. Siirry **Alkuainemuodossa Live** web-liittymän, ja saat **UDP/TS** streaming encoder määrittäminen. 

2. Kun uusi tapahtuma on luotu, vieritä tulostus-ryhmät ja lisää **UDP/TS** tuloste-ryhmä. 

3. Luo uusi tulosteen valitsemalla **Uusi Stream** ja valitsemalla sitten **Lisää tulosteen**.  
    
    ![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental13.png)
    
    >[AZURE.NOTE] On suositeltavaa Alkuainemuodossa tapahtuma on määritetty "Järjestelmän kelloa" avulla yhdistää kyseessä stream epäonnistui encoder aikakoodi.

4. Nyt kun tulos on luotu, valitse **Lisää muodossa**. Nyt voidaan määrittää tulostus-asetukset. 
5. "Virta 1", joka on luotu vain kohtaan, valitse vasemman reunan **Kuva** -välilehti ja laajenna **Lisäasetukset** asetukset-osassa. 

    ![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Monien käytettävissä mukauttaminen Alkuainemuodossa Live ei, seuraavat asetukset suositellaan toistamisen AMS käytön aloittaminen. 
    
    - Tarkkuus: vähintään 1 280 x 720 
    - Framerate: 30 
    - GOP kokoa: 60 kehykset 
    - Yhteenkietoutunut tila: asteittain 
    - Bittitaajuus: 5000000 bit/s (tämä voidaan säätää verkon rajoitusten mukaan) 
    

    ![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

6. Pyydä kanavan syötteen URL-osoite.
    
    Siirry takaisin AMSE-työkalu ja kanavan tilan tarkistaminen. Kun tila on muuttunut **käynnistetään** **käytössä**, saat syötteen URL-osoite.
      
    Kun kanava on käynnissä, hiiren kakkospainikkeella kanavan nimi, Siirtyminen alaspäin viemällä hiiren osoitin **Kopioi syötteen URL-osoite Leikepöydälle** ja valitse sitten **Ensisijainen syötteen URL-osoite**.  
    
    ![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
    
1. Liitä tiedot alkuaineina **Ensisijainen** kohdekentän. Kaikki muut asetukset voivat jäädä oletusarvo.
    
    ![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Toista nämä vaiheet ja toissijainen syötteen URL-osoite ylimääräisiä redundanssi luomalla erillisen "Tuloste"-välilehdestä UDP/TS Streaming.
    
7. Valitse **Luo** (Jos uusi tapahtuma on luotu) tai **päivitys** (jos olemassa tapahtuman muokkaaminen) ja jatka sitten Aloita encoder. 

>[AZURE.IMPORTANT]Ennen kuin valitset **Aloita** Alkuainemuodossa Live web-liittymän, sinun **on** varmistettava kanavan on valmis. 
>Varmista myös, että ei, kun haluat poistua kanavan ilman tapahtuma kestää kauemmin kuin > 15 minuutin Valmis-tilaan.

Kun virta on ollut käynnissä 30 sekuntia, siirtyä takaisin AMSE työkalu ja testaa toiston.  

###<a name="test-playback"></a>Testi-toisto
  
1. Siirry AMSE-työkalua ja hiiren kakkospainikkeella testataan kanava. Valitse-valikosta **toisto esikatselun** pitämällä hiiren osoitinta ja valitse **Azure Media Playerin**.  

    ![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Jos virta näkyy toisto-ohjelman, valitse encoder on määritetty oikein muodostaa AMS. 

Jos virhe on vastaanotettu, kanavan on palautettava ja encoder asetuksia on muutettu. Katso ohjeet [vianmääritys](media-services-troubleshooting-live-streaming.md) -artikkelissa.   

###<a name="create-a-program"></a>Luo ohjelma

1. Kun kanava toisto on vahvistettu, luoda ohjelman. Valitse AMSE-työkalun **Live** -välilehdessä ohjelma-alueella Napsauta hiiren kakkospainikkeella ja valitse **Luo uusi ohjelman**.  

    ![Alkuainemuodossa](./media/media-services-elemental-live-encoder/media-services-elemental9.png)

2. Ohjelman nimeä ja tarvittaessa Säädä **Arkisto ikkunan pituus** (jonka oletusarvo on 4 tuntia). Voit myös määrittää tallennussijainnin tai jätä oletusarvon mukaan.  
3. Valitse **Käynnistä ohjelma nyt** -valintaruutu.
4. Valitse **ohjelma luo**.  
  
    Huomautus: Ohjelman luominen kestää vähemmän aikaa kuin kanavan luonti.    
 
5. Kun ohjelma on käynnissä, Vahvista toisto ohjelman napsauttaminen hiiren kakkospainikkeella ja Siirtyminen **toisto ohjelmat** ja valitsemalla sitten **Azure Media Playerin**.  
6. Kun vahvistettu, napsauta ohjelma uudelleen ja valitse **Kopioi tulosteen URL-osoite Leikepöydälle** (tai noutaa tiedot **ohjelmatiedot ja asetukset** -vaihtoehto valikosta). 

Virta on nyt valmis pelaaja upotettuna tai jaettu yleisön live tarkastelua varten.  

## <a name="troubleshooting"></a>Vianmääritys

Katso ohjeet [vianmääritys](media-services-troubleshooting-live-streaming.md) -artikkelissa. 


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
