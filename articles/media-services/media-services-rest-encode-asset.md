<properties 
    pageTitle="Voit salata käyttämällä Media Encoder vakio resurssi | Microsoft Azure" 
    description="Opettele käyttämään koodata Media Services mediasisältöä Media Encoder vakio. MALLIKOODEJA käyttää REST API." 
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


#<a name="how-to-encode-an-asset-using-media-encoder-standard"></a>Voit salata käyttämällä Media Encoder vakio resurssi


> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
- [MUILLE KÄYTTÄJILLE](media-services-rest-encode-asset.md)
- [Portal](media-services-portal-encode.md)

##<a name="overview"></a>Yleiskatsaus
Jos haluat pitää digitaalisen videon Internetin välityksellä on pakkaa media. Video-tiedostot ovat suuria ja saattaa olla liian suuri aikana Internetin kautta tai asiakkaiden laitteiden oikein. Prosessi, jossa pakkaaminen video ja ääni, jotta asiakkaasi voivat tarkastella mediatiedostojen koodaus on.

Koodauksen työt ovat yksi yleisimmistä käsittely-toimintoa Media Services-palveluissa. Voit luoda koodauksen töitä muunnettava mediatiedostojen koodaukset toiseen. Voit salata, voit valmiin encoder Media Services-(Media Encoder Standard). Voit myös käyttää encoder myöntämä Media Services kumppanin; kolmannen osapuolen koodauslaitteista ovat saatavilla Azure Marketplacesta. Voit määrittää koodaus tehtäviä käyttämällä valmista merkkijonot määritetty yhteyttä encoder tai käyttämällä valmista tiedostojen tietoja. Tiedostotyypit, jotka ovat käytettävissä Esiasetukset, on kohdassa [Tehtävän Esiasetukset for Media Encoder vakio](http://msdn.microsoft.com/library/mt269960).

Kunkin projektin voi olla vähintään yksi tehtävä, jonka haluat suorittaa käsittely tyypin mukaan. REST-Ohjelmointirajapinnalla avulla voit luoda työt ja niihin liittyvät tehtävät jollakin seuraavista tavoista:

- Tehtävät voidaan määritetyn tekstiin projektin kohteita, tehtäviä ja siirtyminen ‑ominaisuuksien kautta tai
- – OData eräkäsittely.


On suositeltavaa koodata aina mezzanine tiedostojen siirtäminen mukautuvat bittitaajuus MP4 määrittäminen ja muunna Määritä haluamasi [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md)-muodossa. Dynaaminen pakkaus hyödyntää, sinun täytyy saada vähintään yhden tarvittaessa streaming yksikön streaming päätepisteen, josta aiot toimituksen sisältöä. Katso lisätietoja, [miten asteikko Media Services](media-services-portal-manage-streaming-endpoints.md).

Jos tulos resurssi on salattu tallennustilan, sinun on määritettävä resurssi toimituksen käytännön. Katso lisätietoja [määrittäminen resurssi toimituksen käytännön](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE]Ennen kuin aloitat viittaava media-suorittimet, varmista, että oikea media suoritin ID-tunnuksellasi. Lisätietoja on kohdassa [Hae Media-suorittimista](media-services-rest-get-media-processor.md).

##<a name="create-a-job-with-a-single-encoding-task"></a>Luo projekti yhden koodauksen tehtävän kanssa

>[AZURE.NOTE] Kun käsittelet Media Services REST-Ohjelmointirajapinta, ottaa huomioon seuraavat asiat:
>
>Kohteiden Media-palveluita käyttäessään sinun on määritettävä HTTP-pyyntöjen tietyn otsikossa näkyvät kentät ja arvot. Lisätietoja on artikkelissa [Media Services REST API kehittäminen asetukset](media-services-rest-how-to-use.md).

>Kun https://media.windows.net muodostettu yhteys, saat 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut [yhteyden muodostaminen Media-palveluita käyttämällä REST API](media-services-rest-connect-programmatically.md)kuvattuja uusi URI-osoitteeseen.
>
>Kun käyttämällä JSON ja määrittämällä **__metadata** avainsanojen käyttäminen pyynnön (esimerkiksi viittaa linkitetyn objektin) on määritettävä **Accept** -otsikon [JSON yksityiskohtainen](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)muotoon: Hyväksy: sovelluksen/json; odata = yksityiskohtainen.

Seuraavassa esimerkissä esitetään, kuinka voit luoda ja julkaista projektin yhden tehtävän asettaminen tietyn tarkkuutta ja laatu videon koodata. Kun koodaus Media Encoder vakio kanssa, voit käyttää määritetyn tehtävän määritys Esiasetukset [tähän](http://msdn.microsoft.com/library/mt269960).

Pyyntö:

Kirjaa https://media.windows.net/API/Jobs HTTP/1.1-sisältötyypin: sovelluksen/json; odata = yksityiskohtainen Hyväksy: sovelluksen/json; odata = yksityiskohtainen DataServiceVersion: 3.0 MaxDataServiceVersion: 3.0 x-ms-versio: 2.11 luvan: haltijan <token value> 
 x-ms-asiakas-pyynnön-id: 00000000-0000-0000-0000-000000000000 Host (isäntä): media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "H264 Multiple Bitrate 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Vastaus:
    
    HTTP/1.1 201 Created

    . . . 

###<a name="set-the-output-assets-name"></a>Määritä tulostus-kohteen nimi

Seuraavassa esimerkissä esitetään määrittämisestä assetName määrite:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

##<a name="considerations"></a>Huomioon otettavia seikkoja

- TaskBody ominaisuudet käytettävä literal XML määrittää syötteen määrän tai tulosteen, jota käytetään tehtävän resurssit. Tehtävän aihe sisältää XML-rakenteen määritys XML.
- TaskBody-määrityksessä kunkin sisempi arvo <inputAsset> ja <outputAsset> on määritettävä JobInputAsset(value) tai JobOutputAsset(value).
- Tehtävän voi olla useita tulosteen resurssit. Yksi JobOutputAsset(x) voi käyttää vain kerran kuin tulos projektin tehtävästä.
- Voit määrittää syötteen resurssi tehtävän JobInputAsset tai JobOutputAsset.
- Jakson on eivät muodosta tehtävät.
- Value-parametria, jotka siirtävät JobInputAsset tai JobOutputAsset edustaa indeksiarvon sijoituksen. Todellinen varat määritetään työn kohteen määritelmän InputMediaAssets ja OutputMediaAssets siirtyminen ominaisuudet. 
- Koska Media Services perustuu OData v3, InputMediaAssets ja OutputMediaAssets siirtyminen ominaisuuden kokoelmien yksittäisiä kohteita, joihin viitataan kautta "__metadata: uri-nimi-arvo-pari.
- InputMediaAssets yhdistää yhden tai useamman varat olet luonut Media Services-palveluissa. OutputMediaAssets luodaan järjestelmä. Viittaa aiemmin resurssi.
- OutputMediaAssets voi nimetä assetName-määritteen avulla. Jos tämä määritettä ei ole ja valitse OutputMediaAsset nimi on jostakin tekstissä arvo <outputAsset> elementti on jälkiliitteellä projektin nimi-arvoa tai työn tunnus-arvon (siltä varalta, jossa Name-ominaisuutta ei ole määritetty). Esimerkiksi jos määrität assetName "Esimerkki" arvo, valitse OutputMediaAsset Name-ominaisuutta arvoksi "Esimerkki". Kuitenkin jos ei ole määrittänyt assetName arvo, mutta määrittänyt "NewJob" työn nimen, valitse OutputMediaAsset nimi olisi "JobOutputAsset (arvo) _NewJob". 


##<a name="create-a-job-with-chained-tasks"></a>Työn ketjutetut tehtävien luominen

Sovelluksen skenaarioita kehittäjät haluat luoda käsittelyn tehtävien sarjaa. Media-palveluja voit luoda ketjutetut tehtävien sarjaa. Kunkin tehtävän suorittaa eri vaiheet, ja voit käyttää eri media-suorittimista. Ketjutetut tehtävät voidaan painotaloon sijoituksen yhdestä tehtävästä toiseen, suorittaa tehtäviä Lineaarinen sarja kohteen. Kuitenkin projektin tehtäviä ei tarvitse olla peräkkäin. Kun luot Ketjutettujen tehtävän, Ketjutettujen **ITask** objektit luodaan **IJob** yhtenä objektina.

>[AZURE.NOTE] Tällä hetkellä enintään 30 tehtäviä projektia kohti. Jos haluat ketjut yli 30 tehtäviä, luoda useamman kuin yhden projektin tehtävät sisältävät.


    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


###<a name="considerations"></a>Huomioon otettavia seikkoja

Voit ottaa tehtävän ketjutus seuraavasti:

- Työn on oltava vähintään 2 tehtävät
- Asiakirjassa on oltava vähintään yksi tehtävä, jonka syötteen siirretään toisen projektin tehtävästä.

## <a name="use-odata-batch-processing"></a>Käytä OData eräkäsittely 

Seuraavassa esimerkissä esitetään, miten OData eräkäsittely avulla voit luoda projektin- ja tehtävät. Lisätietoja eräkäsittely on artikkelissa [Open Data Protocol (OData) erän käsittely](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).
 
    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net
    
    
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}
    
    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary
    
    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <token>
    x-ms-version: 2.11
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    
    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--
 


## <a name="create-a-job-using-a-jobtemplate"></a>Työn JobTemplate luominen


Useita kohteita käyttämällä yleisiä tehtäviä käsiteltäessä JobTemplates on hyötyä määrittää oletusarvon tehtävän Esiasetukset kuvatussa järjestyksessä ja niin edelleen.

Seuraavassa esimerkissä esitetään luomisesta JobTemplate TaskTemplate määritetty riveillä. TaskTemplate käyttää Media Encoder vakio MediaProcessor koodata resurssi-tiedostoa. muut MediaProcessors voi kuitenkin käyttää myös. 


    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }
 

>[AZURE.NOTE]Toisin kuin Media Services muihin kohteisiin on uusi GUID-tunnus määrittäminen kunkin TaskTemplate ja sijoita se taskTemplateId ja Id-ominaisuutta pyynnön tekstissä. Sisällön tunnistusmalli on seurattava tunnistaa Azure Media Services kohteiden kuvattu värimallin. Lisäksi JobTemplates ei voi päivittää. Sen sijaan sinun on luotava uusi päivitetty muutokset.
 

Jos onnistuu, funktio palauttaa seuraavan vastauksen:
    
    HTTP/1.1 201 Created
    
    . . .


Seuraavassa esimerkissä esitetään viittaava JobTemplate tunnus työn luominen:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer <token value>
    Host: media.windows.net

    
    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}
     

Jos onnistuu, funktio palauttaa seuraavan vastauksen:
    
    HTTP/1.1 201 Created
    
    . . . 



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="next-steps"></a>Seuraavat vaiheet
Nyt kun osaat luoda koodata assset työn, siirry [Siitä, miten voit tarkistaa projektin tilanne, jossa Media Services](media-services-rest-check-job-progress.md) -aihetta.


##<a name="see-also"></a>Katso myös

[Hae Media-suorittimet](media-services-rest-get-media-processor.md)
