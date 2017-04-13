<properties 
    pageTitle="Vianmäärityksen oppaan live streaming | Microsoft Azure" 
    description="Tässä ohjeaiheessa antaa ehdotuksia live streaming vianmääritys." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/12/2016"  
    ms.author="juliako"/>

#<a name="troubleshooting-guide-for-live-streaming"></a>Live streaming opas vianmääritysohjeita

Tässä ohjeaiheessa antaa ehdotuksia joitakin live streaming vianmääritys.

## <a name="issues-related-to-on-premises-encoders"></a>Paikallisen koodauslaitteista liittyvät ongelmat 

Tässä osassa antaa ehdotukset, jotka on määritetty yksittäisen bittitaajuus stream lähettäminen AMS, jotka on otettu käyttöön live koodaus kanavien paikallisen koodauslaitteista liittyvien ongelmien vianmääritys.

###<a name="problem-would-like-to-see-logs"></a>Ongelma: Haluaisit lokit 

- **Mahdollinen ongelma**: ei voi etsiä kirjaa encoder, joka voi auttaa ongelmat.
    
    - **Telestream Wirecast**: löytyy yleensä lokit kohdassa C:\Users\{käyttäjänimi} \AppData\Roaming\Wirecast\ 
    - **Alkuainemuodossa Live**: Voit etsiä on linkkejä lokit hallinta-portaalissa. Valitse **Tilasto**ja sitten **lokitiedot**. **Lokitiedostojen** sivulla näet luettelon kaikki LiveEvent kohteet; lokit Valitse vastaavat nykyisessä istunnossa. 
    - **Flash Media Live Encoder**: löydät **Log Directory...** siirtymällä **Koodaus loki** -välilehti.
    
###<a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Ongelma: Ei ole vaihtoehtoa tulostetaan asteittain muodossa

- **Mahdollinen ongelma**: käytössä encoder ei automaattisesti lomituksen. 

    **Vianmääritysohjeet**: Etsi encoder liittymää lomitusta vaihtoehto. Kun varaustiedoista lomituksen on otettu käyttöön, tarkista uudelleen asteittain tulosteen asetukset. 
 
###<a name="problem-tried-several-encoder-output-settings-and-still-unable-to-connect"></a>Ongelma: Yritti useita encoder tulosteen asetuksia ja saa yhteyttä. 

- **Mahdollinen ongelma**: Azure koodaus kanavien palautettiin ei ole oikein. 

    **Vianmääritysohjeet**: Varmista, että encoder enää valitseminen, AMS, Lopeta ja nollaa kanava. Kun suoritat uudelleen, yritä yhdistää oman encoder uusia asetuksia. Jos tämä ei vielä korjaa ongelman, voit luoda uuden kanavan kokonaan, joskus kanavien voit vioittua, kun useita epäonnistui yritykset.  

- **Mahdollinen ongelma**: GOP koon tai avaimen kehyksen asetukset eivät ole paras mahdollinen. 

    **Vianmääritysohjeet**: suositellut GOP koon tai Avainkuva väli on 2 sekunnin ajan. Jotkin koodauslaitteista Laske kehysten, tämä asetus, samalla, kun muut käyttävät sekuntia. Esimerkki: 30fps siirrettäessä GOP kokoa olisi 60 kehyksiä, jossa on sama kuin 2 sekunnin ajan.  
     
- **Mahdollinen ongelma**: suljettujen portit estävät virta. 

    **Vianmääritysohjeet**: kun streaming RTMP kautta, tarkista palomuurin tai välityspalvelimen asetukset vahvistamaan, että lähtevän portit 1935 ja 1936 ovat avoinna. Kun käytät RTP-tietovirta, varmista, että lähtevän portin 2010 on avoinna. 


###<a name="problem-when-configuring-the-encoder-to-stream-with-the-rtp-protocol-there-is-no-place-to-enter-a-host-name"></a>Ongelma: Määritettäessä encoder stream, RTP-protokollan kanssa ei ole paikka, kirjoita isäntänimi. 

- **Mahdollinen ongelma**: monta RTP koodauslaitteista eivät salli isäntänimet, ja IP-osoite on saatu.  

    **Vianmääritysohjeet**: Etsi IP-osoite, Avaa komentorivi missä tahansa tietokoneessa. Voit tehdä tämän Windowsin Avaa Suorita-avain (WIN + R) ja kirjoita "cmd" Avaa.  

    Kun komentorivi-ikkuna on avoinna, kirjoita "Ping [AMS isäntänimi]". 

    Isäntänimi voi johtaa porttinumero Azure Ingest URL-osoitteen, Dashboard seuraavan esimerkin mukaisesti: 

    RTP://test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 

    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

###<a name="problem-unable-to-playback-the-published-stream"></a>Ongelma: Toisto julkaistun virta ei onnistu.
 
- **Mahdollinen ongelma**: ei käytössä Streaming päätepistettä tai ei ole streaming yksiköiden (aikayksikön). 

    **Vianmääritysohjeet**: Siirry AMSE-työkalun "Streaming päätepisteen"-välilehti ja vahvista on yhden streaming yksikön käyttämisestä Streaming päätepiste. 
    


>[AZURE.NOTE] Jos jälkeen seuraavien vianmääritysvaiheiden avulla, voit silti ei voi onnistuneesti virtauttaa Lähetä tuki lippu Azure-portaalissa.

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
