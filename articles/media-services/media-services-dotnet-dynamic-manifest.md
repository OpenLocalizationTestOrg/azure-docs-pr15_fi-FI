<properties 
    pageTitle="Suodattimien luominen Azure Media Services .NET SDK-paketissa" 
    description="Tässä ohjeaiheessa kerrotaan, miten voit luoda suodattimia, jotta asiakkaan voi käyttää niitä stream stream tietyn osiin. Media Services luo saavuttamiseksi Valikoiva streaming dynaaminen luettelot." 
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
    ms.date="07/18/2016"
    ms.author="juliako;cenkdin"/>


#<a name="creating-filters-with-azure-media-services-net-sdk"></a>Suodattimien luominen Azure Media Services .NET SDK-paketissa

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-dynamic-manifest.md)
- [MUILLE KÄYTTÄJILLE](media-services-rest-dynamic-manifest.md)

2.11 release alkaen Media Services avulla voit määrittää oman varat suodattimet. Nämä ovat palvelimen asiakaspuolen sääntöjen, joiden avulla voit valita esimerkiksi asiakkaille: toisto vain videon (sen sijaan, että koko videon toiston)-osaan tai määrittää vain osaa ääni- ja käännöksiä, asiakkaan laitteen voit käsitellä (sijaan kaikki käännöksiä, jotka liittyvät kohteen). Oman kohteiden suodattaminen saavutetaan – **Dynaamisen luettelon**s, jotka on luotu asiakkaan pyydettäessä videovirtaa perusteella määritetty suodattimet.

Katso tarkempia tietoja suodattimet ja dynaaminen luettelon, [dynaaminen luettelot yleiskatsaus](media-services-dynamic-manifest-overview.md).

Tässä ohjeaiheessa kerrotaan, miten Media Services .NET SDK voi luoda, päivittää ja poistaa suodattimet. 


Huomautus Jos päivität suodattimen, voi kestää enintään 2 minuuttia streaming päätepisteen päivittämiseen sääntöjä. Jos sisältö on served tämän suodattimen avulla (ja välimuistiin välityspalvelimet ja CDN tallentaa), päivitetään suodatin voi aiheuttaa Playerin virheet. Suositellaan, jos haluat poistaa suodattimen päivittämisen jälkeen on. Jos tämä vaihtoehto ei ole mahdollista, kannattaa käyttää eri suodatinta. 

##<a name="types-used-to-create-filters"></a>Käytettyjä suodattimien luominen

Seuraavanlaisia käytetään, kun suodattimien luominen: 

- **IStreamingFilter**.  Tällaista perustuu seuraava REST API- [Suodatin](http://msdn.microsoft.com/library/azure/mt149056.aspx)
- **IStreamingAssetFilter**. Tällaista perustuu seuraavia REST API- [AssetFilter](http://msdn.microsoft.com/library/azure/mt149053.aspx)
- **PresentationTimeRange**. Tällaista perustuu seuraavia REST API- [PresentationTimeRange](http://msdn.microsoft.com/library/azure/mt149052.aspx)
- **FilterTrackSelectStatement** ja **IFilterTrackPropertyCondition**. Tällaisia perustuvat seuraavat REST API [FilterTrackSelect ja FilterTrackPropertyCondition](http://msdn.microsoft.com/library/azure/mt149055.aspx)


##<a name="createupdatereaddelete-global-filters"></a>Yleiset suodattimet luominen, päivittäminen ja luku/poistaminen

Seuraava koodi näkyy .NET avulla voit luoda, päivittää, lue ja poistamisesta resurssi suodattimet.
    
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();
                
    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();
    
    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);
    
    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);
    
    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();
    
    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();


##<a name="createupdatereaddelete-asset-filters"></a>Kohteiden luominen, päivittäminen ja luku/poistaminen suodattimet

Seuraava koodi näkyy .NET avulla voit luoda, päivittää, lue ja poistamisesta resurssi suodattimet.

    
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();
    
        
    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());
    
    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);
    
    filter.Update();
    
    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);
    
    // Delete
    filterUpdated.Delete();
    



##<a name="build-streaming-urls-that-use-filters"></a>Muodosta streaming suodattimilla URL-osoitteet

Lisätietoja siitä, miten voit julkaista ja pitää yhteyttä kalusto-kohdassa [Sisällön toimittaminen asiakkaille yleiskatsaus](media-services-deliver-content-overview.md).


Seuraavissa esimerkeissä suodattimien lisääminen streaming URL-osoitteet.

**MPEG-VIIVA** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Tasainen Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


**HDS**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=f4m-f4f, filter=MyFilter)


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Katso myös 

[Dynaaminen luettelot yleiskatsaus](media-services-dynamic-manifest-overview.md)
 

