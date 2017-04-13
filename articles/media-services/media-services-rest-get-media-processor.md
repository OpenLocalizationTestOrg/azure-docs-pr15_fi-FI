<properties 
    pageTitle="Media-suoritin luominen | Microsoft Azure" 
    description="Opettele luomaan media suoritin osan koodata, muunna muodossa, salaaminen ja salauksen mediasisältöä Azure Media Services." 
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

##<a name="get-mediaprocessor"></a>Hae MediaProcessor

>[AZURE.NOTE] Kun käsittelet Media Services REST-Ohjelmointirajapinta, ottaa huomioon seuraavat asiat:
>
>Kohteiden Media-palveluita käyttäessään sinun on määritettävä HTTP-pyyntöjen tietyn otsikossa näkyvät kentät ja arvot. Lisätietoja on artikkelissa [Media Services REST API kehittäminen asetukset](media-services-rest-how-to-use.md).

>Kun https://media.windows.net muodostettu yhteys, saat 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut [yhteyden muodostaminen Media-palveluita käyttämällä REST API](media-services-rest-connect-programmatically.md)kuvattuja uusi URI-osoitteeseen. 


Seuraava REST-kutsu näyttää, miten voit saada media-suoritin esiintymän nimi (eli tässä tapauksessa **Media Encoder vakio**). 



    
Pyyntö:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.11
    Host: media.windows.net
    
Vastaus:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun osaat hankkiminen media-suoritin esiintymän, siirry, jossa kerrotaan, kuinka koodata sijoituksen Media Encoder vakio avulla [Voit salata sijoituksen](media-services-rest-get-started.md) aihetta.
