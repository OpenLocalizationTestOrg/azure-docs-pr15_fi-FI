<properties 
    pageTitle="Lisäasetukset koodauksen työnkulkujen luominen työnkulun suunnittelussa | Microsoft Azure" 
    description="Lue lisää Lisäasetukset koodauksen työnkulkujen luominen työnkulun suunnittelussa." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;johndeu;anilmur"/>


#<a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Työnkulun suunnittelun avulla Lisäasetukset koodauksen työnkulkujen luominen

##<a name="overview"></a>Yleiskatsaus

**Työnkulun suunnittelun** on Windows desktop työkalu, jolla voit suunnitella ja luoda mukautettuja työnkulkuja koodaustoiminnon **Media Encoder Premium**työnkulussa.
Potenssiin työnkulun suunnittelutyökalun-työkalun avulla voit suunnitella ja luoda monimutkaisia työnkulkuja, jotka suorittavat **Media Encoder Premium**.  

Työnkulut voivat sisältää asiakkaan päätös logiikan ja haarautumista lähde-tiedoston ominaisuuksien perusteella. Työnkulkujen luominen overridable-määritys ominaisuudet ja dynaamisia arvoja, jotta myös monimutkaisia koodauksen tehtävät helposti toista ja mukauttaa pilveen.

Esimerkki työnkulut, joita voit luoda ovat seuraavat:

- Päätös perustuu työnkulkuja, jotka Tarkasta sisältölähde tarkkuutta ja koodata haluttu tulos-raidat.  Tämä on helfpul poistamalla kuluisi liikaa kappaleet, jotka luoma upscaling lähde sisällön aioit.
- Useita syötteen tiedostoja voidaan tukemaan kuvatekstit, kerrokset ja stitching yhdessä sisältöä. 

Tämän työkalun avulla voidaan myös muokata kaikkia Microsoftin [julkaistu työnkulkuja](media-services-workflow-designer.md#existing_workflows). 

>[AZURE.NOTE]Saat työnkulun suunnittelu-työkalun kopion, ota yhteyttä mepd@microsoft.com.


Kun työnkulku-tiedosto on luotu, se voi ladata amerikkalaisen nimellä ja sitten voidaan koodaustoiminnon mediatiedostojen. Lisätietoja koodata **Media Encoder Premium työnkulun** käyttämällä **.NET**-kohdassa [Advanced koodaus Media Encoder Premium työnkulussa](media-services-encode-with-premium-workflow.md).

##<a id="existing_workflows"></a>Muokkaa aiemmin työnkulkuja

Oletusarvon [julkaistu työnkulkuja](media-services-workflow-designer.md#existing_workflows) voidaan muokata suunnittelu-työkalun avulla. Voit avata oletusarvo työnkulun tiedostot [tähän](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). Kansio sisältää myös näiden tiedostojen kuvaus.

Seuraavissa videoissa näytetään, miten voit käyttää suunnittelua.

###<a name="day-1--getting-started"></a>Päivä 1 – aloittaminen

1 päivä video kuuluvat:

- Suunnittelutyökalun yleiskatsaus
- Perustiedot työnkulkujen – "Hei"
- MP4-tiedostot ja käyttää siinä Azure Media Services streaming luominen useita tulostus

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-1]

###<a name="day-2"></a>Päivä 2

Päivä 2 video kuuluvat:

- Vaihteleva lähde tiedoston skenaariot – käsittely ääni
- Työnkulut, joiden Lisäasetukset logiikka
- Kaavion vaiheet

> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-2]

###<a name="day-3"></a>Päivä-3

Päivä 3 video kuuluvat:

- Komentosarjan työnkulut tai toimintakaavioita sisältyy
- Nykyinen Encoder rajoituksia
- Q & A
 
> [AZURE.VIDEO azure-premium-encoder-workflow-designer-training-videos-day-3]


## <a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


Jos tarvitset tukea tai sinulla on kysyttävää mukautettujen työnkulkujen luominen työnkulun suunnittelutyökalun työkalussa, Lähetä sähköpostia mepd@microsoft.com.

##<a name="see-also"></a>Katso myös

[Azure Premium Encoder työnkulun suunnittelutyökalun-koulutusvideot](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)
