<properties 
    pageTitle="Määritä FMLE encoder lähettää yhden bittitaajuus live stream | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään Flash Media Live Encoder (FMLE) encoder yksittäisen bittitaajuus stream lähettäminen AMS, jotka on otettu käyttöön live koodaus kanavien määrittämisestä." 
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
    ms.date="10/12/2016"
    ms.author="juliako;cenkdin;anilmur"/>

#<a name="use-the-fmle-encoder-to-send-a-single-bitrate-live-stream"></a>Lähetä yksittäinen bittitaajuus live stream FMLE encoder avulla

> [AZURE.SELECTOR]
- [FMLE](media-services-configure-fmle-live-encoder.md)
- [Alkuaineina Live](media-services-configure-elemental-live-encoder.md)
- [Tricaster](media-services-configure-tricaster-live-encoder.md)
- [Wirecast](media-services-configure-wirecast-live-encoder.md)

Tässä ohjeaiheessa esitellään [Flash Media Live Encoder](http://www.adobe.com/products/flash-media-encoder.html) (FMLE) encoder yksittäisen bittitaajuus stream lähettäminen AMS, jotka on otettu käyttöön live koodaus kanavien määrittämisestä. Lisätietoja on artikkelissa [käyttäminen kanavat, jotka ovat käytössä suorittaa Live koodaus Azure Media-palvelujen kanssa](media-services-manage-live-encoder-enabled-channels.md).

Tässä opetusohjelmassa näytetään, miten voit hallita Azure Media Services (AMS) Azure Media Services Explorer (AMSE)-työkalulla. Tämä työkalu suorittaa vain Windows-tietokoneessa. Jos käytössäsi on Mac tai Linux, Azure portaalin avulla voit luoda [kanavat](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) ja [Ohjelmat](media-services-portal-creating-live-encoder-enabled-channel.md#create-and-manage-a-program).

Huomaa, että tässä opetusohjelmassa kuvataan AAC avulla. FMLE ei kuitenkaan tukee AAC oletusarvoisesti. Voit ostaa AAC koodaus, kuten MainConcept ‑laajennuksen: [AAC-lisäosa](http://www.mainconcept.com/products/plug-ins/plug-ins-for-adobe/aac-encoder-fmle.html)

##<a name="prerequisites"></a>Edellytykset

- [Azure Media Services-tilin luominen](media-services-portal-create-account.md)
- Varmista on vähintään yhden streaming yksikön kohdistettu käyttämisestä Streaming päätepiste. Lisätietoja on artikkelissa [Hallitse Streaming päätepisteet Media Services-tilin](media-services-portal-manage-streaming-endpoints.md)
- Asenna uusin versio [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) -työkalun.
- Käynnistä työkalu ja muodostaa yhteyden tiliisi AMS.

##<a name="tips"></a>Vihjeitä

- Käytä mahdollisuuksien mukaan tavallisen internet-yhteys.
- Hyvä säännön of napin-lyncserver2010 määritettäessä on kaksoisnapsauttamalla streaming bitrates. Samalla, kun tämä ei ole pakollinen vaatimus, se auttaa pienentämään verkon kuormituksen vaikutus.
- Kun ohjelmistoa käyttämällä perusteella koodauslaitteista, sulkea tarpeettomat ohjelmat.

## <a name="create-a-channel"></a>Kanavan luominen

1.  AMSE-työkalun Siirry **Live** -välilehti ja kanava-alueella Napsauta hiiren kakkospainikkeella. Valitse **Luo kanava...** Valitse valikosta.

![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle1.png)

2. Määritä kanavan nimi, kuvauskenttä on valinnainen. Kanavan asetukset-kohdassa **Vakio** Live koodaus-asetuksen arvoksi **RTMP**Input-protokollan kanssa. Voit jättää kaikki muut asetukset on.


Varmista, että **Aloita uusi kanava nyt** on valittuna.

3. Valitse **Luo kanava**.
![FMLE](./media/media-services-fmle-live-encoder/media-services-fmle2.png)

>[AZURE.NOTE] Kanavan voi kestää niin kauan kuin 20 minuutin.


Kun kanava käynnistyy voit [määrittää encoder](media-services-configure-fmle-live-encoder.md#configure_fmle_rtmp).

>[AZURE.IMPORTANT] Huomaa, että Laskutus alkaa heti, kun kanavan tiedosto on valmis. Lisätietoja on artikkelissa [kanavan hyötyä](media-services-manage-live-encoder-enabled-channels.md#states).

##<a id=configure_fmle_rtmp></a>Määritä FMLE encoder

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


###<a name="configuration-steps"></a>Määritysvaiheet

1. Siirry Flash Media Live Encoder's (FMLE) liittymän käytössä tietokoneeseen.

    Rajapinta on yksi pääsivulta asetuksia. Ota huomioi seuraavat suositellut asetukset streaming käyttämällä FMLE käytön aloittaminen.
    
    - Muoto: H.264 kehyksen korko: 30,00 
    - Syötteen kokoa: vähintään 1 280 x 720 
    - Siirtonopeuden: 5000 Kbps (voidaan säätää verkon rajoitusten mukaan)  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle3.png)

    Kun käyttämällä lomitettu lähteistä, ota valintamerkki "Deinterlace"-vaihtoehto

2. Jakoavain valitsemalla Muotoile vieressä, Lisää asetuksia:

    - Profiili: pää
    - Tason: 4.0
    - Avainkuva taajuus: 2 sekunnin ajan 
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle4.png)

3. Määritä seuraavat tärkeät ääniasetus:
    
    - Muoto: AAC 
    - Esimerkki korko: 44100 Hz
    - Bittitaajuus: 192 Kbps
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle5.png)

6. Jotta voit määrittää sen FMLE **RTMP päätepisteen**kanavan syötteen URL-Osoitteen hakeminen
    
    Siirry takaisin AMSE-työkalu ja kanavan tilan tarkistaminen. Kun tila on muuttunut **käynnistetään** **käytössä**, saat syötteen URL-osoite.
      
    Kun kanava on käynnissä, hiiren kakkospainikkeella kanavan nimi, Siirtyminen alaspäin viemällä hiiren osoitin **Kopioi syötteen URL-osoite Leikepöydälle** ja valitse sitten **Ensisijainen syötteen URL-osoite**.  
    
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle6.png)

7. Liitä tiedot tulostus-osan **FMS URL-osoite** -kenttään ja määrittää stream nimen. 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle7.png)

    Ylimääräisen redundanssi, toista nämä vaiheet ja toissijainen syötteen URL-osoite.
8. Valitse **Muodosta yhteys**.

>[AZURE.IMPORTANT]Ennen kuin valitset **Yhdistä**, sinun **on** varmistettava kanavan on valmis. 
>Varmista myös, että halua lähteviä kanavan valmiina syötteen osuus syötettä yli > 15 minuuttia.

##<a name="test-playback"></a>Testi-toisto
  
1. Siirry AMSE-työkalua ja hiiren kakkospainikkeella testataan kanava. Valitse-valikosta **toisto esikatselun** pitämällä hiiren osoitinta ja valitse **Azure Media Playerin**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle8.png)

Jos virta näkyy toisto-ohjelman, valitse encoder on määritetty oikein muodostaa AMS. 

Jos virhe on vastaanotettu, kanavan on palautettava ja encoder asetuksia on muutettu. Katso ohjeet [vianmääritys](media-services-troubleshooting-live-streaming.md) -artikkelissa.  

##<a name="create-a-program"></a>Luo ohjelma

1. Kun kanava toisto on vahvistettu, luoda ohjelman. Valitse AMSE-työkalun **Live** -välilehdessä ohjelma-alueella Napsauta hiiren kakkospainikkeella ja valitse **Luo uusi ohjelman**.  

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle9.png)

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
