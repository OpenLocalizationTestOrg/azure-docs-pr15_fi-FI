<properties 
    pageTitle="Media-suoritin luominen | Microsoft Azure" 
    description="Opettele luomaan media suoritin osan koodata, muunna muodossa, salaaminen ja salauksen mediasisältöä Azure Media Services. MALLIKOODEJA kirjoitetaan C# ja käyttää Media Services SDK .NET." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="how-to-get-a-media-processor-instance"></a>Toimintaohje: Hae Media-suoritin-esiintymä

> [AZURE.SELECTOR]
- [.NET](media-services-get-media-processor.md)
- [MUILLE KÄYTTÄJILLE](media-services-rest-get-media-processor.md)


##<a name="overview"></a>Yleiskatsaus

Muotoile Media Services media-suoritin on osa, joka käsittelee käsittelyä tehtävän, kuten koodaus, muunto, salaaminen ja salauksen poistaminen mediasisältöä. Voit luoda media-suoritin yleensä koodata, salaaminen ja muunna mediasisältöä muodossa tehtävän luotaessa.

Seuraavassa taulukossa on nimi ja kuvaus käytettävissä media-suoritin.

Media-suoritin nimi|Kuvaus|Lisätietoja
---|---|---
Media Encoder vakio|Tarvittaessa koodaus on vakio ominaisuuksia. |[Yleiskatsaus ja Azure-Demand Media koodauslaitteista vertailu](media-services-encode-asset.md)
Media Encoder Premium työnkulku|Voit suorittaa koodauksen tehtävät Media Encoder Premium työnkulun avulla.|[Yleiskatsaus ja Azure-Demand Media koodauslaitteista vertailu](media-services-encode-asset.md)
Azure Media indeksointitoiminto| Voit tehdä mediatiedostojen ja sisällön etsittävän sekä luoda suljettu tekstitys raidat ja avainsanat.|[Azure Media indeksointitoiminto](media-services-index-content.md)
Azure Media Hyperlapse (ennakkoversio)|Mahdollistaa tasoittaa käyttäminen videon vakiintuneiden nopeuksien ero videon "kohoumat". Myös avulla voit nopeuttaa sisällön kulutettavia ClipArt-kuvan siirtäminen.|[Azure Media Hyperlapse](media-services-hyperlapse-content.md)
Azure Media Encoder|Poisto
Tallennustilan salauksen purkaminen| Poisto|
Azure Media Packagerin|Poisto|
Azure Media-salaus|Poisto|

##<a name="get-media-processor"></a>Hae Media-suoritin

Seuraava menetelmä näyttää hankkiminen media-suoritin esiintymä. Koodin esimerkissä oletetaan, että moduulin tason muuttujan **_context** viitataan konteksti kohdassa kuvatulla tavalla käyttö [Toimintaohje: yhdistäminen ohjelmallisesti Media-palveluiden](media-services-dotnet-connect-programmatically.md).

    private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
        ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
        if (processor == null)
        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
        return processor;
    }


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun osaat hankkiminen media-suoritin esiintymän, siirry, jossa kerrotaan, kuinka koodata sijoituksen Media Encoder vakio avulla [Voit salata sijoituksen](media-services-dotnet-encode-with-media-encoder-standard.md) aihetta.


