<properties
    pageTitle="Videon rajaaminen | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, miten voit rajata videoiden katselusta Media Encoder vakio."
    services="media-services"
    documentationCenter=""
    authors="anilmur"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016"  
    ms.author="anilmur;juliako;"/>

# <a name="crop-videos-with-media-encoder-standard"></a>Rajaa videoiden katselusta Media Encoder vakio

Voit rajata syötettäsi video Media Encoder Vakio (MES). Rajaus on valitseminen videon suorakulmainen ikkuna ja koodaus vain kuvapistettä, keskusteluikkunasta käsin. Seuraavassa kaaviossa auttaa havainnollistaa prosessin.

![Videon rajaaminen](./media/media-services-crop-video/media-services-crop-video01.png)

Oletetaan, että sinulla syötteeksi video, jossa on tarkkuus on 1920 x 1080 pikseliä (16:9 kuvasuhde), mutta on mustia palkkeja (palvelinvirtualisointi luetteloruudut) vasemmalle ja oikealle, niin, että vain 4:3-ikkuna tai 1 440 x 1080 kuvapisteen on aktiivinen video. Voit Markkinatalousasemaa avulla muokkaaminen, musta palkit ja koodata 1 440 x 1080-alue.

Rajaus Markkinatalousasemaa on valmiiksi käsittely vaiheen, joten koodauksen esiasetus rajauksen parametrit käyttää alkuperäisen syötteen videon. Koodaus on myöhemmässä vaiheessa ja leveys ja korkeus-asetukset soveltuvat *valmiiksi käsitteleminen* videon, eikä alkuperäinen video. Kun suunnittelet oman esiasetus sinun on suoritettava seuraavat: (a) Valitse rajaa parametrien perusteella alkuperäisen syötteen videon ja (b) Valitse oman koodata rajatut videon asetukset. Jos olet eivät vastaa omaa koodata rajatut videon asetuksia, tulos ei ole haluamallasi tavalla.

[Seuraavassa](media-services-advanced-encoding-with-mes.md#encoding_with_dotnet) aiheessa näyttää luomisesta koodauksen työn Markkinatalousaseman kanssa ja määrittäminen mukautetun koodauksen tehtävän valmiiksi. 

## <a name="creating-a-custom-preset"></a>Mukautetun esiasetus luominen

Kaavion esimerkissä:

1. Alkuperäinen syöte on 1920 x 1080
1. Se on tulos 1 440 x 1080, joka näkyy nyt keskitettynä syötteen kehys, jos haluat rajata
1. Tämä tarkoittaa, että X-siirtymä (1920 – 1 440) / 2 = 240 ja Y siirtymä on nolla
1. Leveys ja korkeus rajauskehyksen on 1 440 ja 1080, tarpeen mukaan
1. Käytä vaiheessa Kysy on kolme kerrosta, ratkaisut 1 440 x 1080 960 x 720 ja 480 x 360 on tarpeen mukaan

###<a name="json-preset"></a>JSON esiasetus


    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


##<a name="restrictions-on-cropping"></a>Rajoitukset rajaus

Rajauksen ominaisuus on tarkoitettu manuaalinen. Haluat ladata syötteen videon sopivan muokkaaminen työkalu, jolla voit valita kehykset halutut, Vie kohdistin määrittämään rajauksen suorakulmio määrittämään koodauksen esiasetus, joka on optimoitu, erityisesti video-ja muille siirtymät. Tämä ominaisuus ei ole tarkoitettu käyttöön kohteita, kuten: automaattinen tunnistaminen ja musta letterbox/pillarbox syötettäsi videon reunojen poistaminen.

Seuraavat rajoitukset koskevat rajauksen ominaisuus. Jos ne eivät täyty, käytä tehtävän voi epäonnistua tai tuottaa odottamattomia tulos.

1. Muiden keskiarvot ja rajauskehyksen kokoa on sopimaan syötteen video
1. Kuten edellä mainittiin, leveyttä ja korkeutta, käytä asetuksia on rajattu videon vastata
1. Rajaus koskee videoita siepata vaakasuuntainen (eli ei sovellu videoita, ne on tallennettu smartphone pidetään pysty- tai pystysuunnassa-tilassa)
1. Toimii parhaiten asteittain kanssa neliö kuvapistettä siepatun videon

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Seuraava vaihe
 
Katso Azure Media Services oppiminen auttaa AMS hyvien ominaisuuksien tietoja.  

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
