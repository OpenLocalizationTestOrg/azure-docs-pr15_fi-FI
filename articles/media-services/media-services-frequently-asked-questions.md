<properties 
    pageTitle="Usein kysyttyjä kysymyksiä | Microsoft Azure" 
    description="Usein kysyttyjä kysymyksiä" 
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


#<a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

##<a name="general-ams-faqs"></a>Yleinen AMS usein kysyttyjä kysymyksiä

K: miten voit skaalata indeksoinnin?

V: varatut resurssit ovat samat koodaus ja indeksointi tehtävät. Noudata ohjeita siitä, [miten voit asteikko koodaus varatut resurssit](media-services-scale-media-processing-overview.md). **Huomautus** indeksointitoiminto suorituskyvyn ei vaikuta varattu tyyppi.

Kysymys: saan on ladattu, koodattu ja julkaista videon. Mikä on syytä videon voi toistaa, kun yritän lähettää sen?

A: jokin yleisimmät syyt on vähintään yksi varattu streaming yksikön kohdistettu streaming päätepisteen, josta yrität toisto ei ole.  Noudata ohjeita siitä, [miten voit asteikko Streaming varatut resurssit](media-services-portal-scale-streaming-endpoints.md).

K: Voinko kuvanluonti live virta-tehdä?

A: kuvanluonti live virtaa käyttöön tällä hetkellä ei tarjotaan Azure Media-palveluja, joten sinun täytyy kirjoittaa valmiiksi tietokoneeseen.

K: Voinko käyttää Azure CDN Live Streaming?

A: Media Services tukee Azure CDN integrointi (Lisätietoja on Katso, [miten voit hallita Streaming päätepisteet Media Services-tilin)](media-services-portal-manage-streaming-endpoints.md).  Voit käyttää Live streaming CDN kanssa. Azure Media-palvelut on tasainen virtautetun median, HLS ja MPEG-VIIVA tulostaa. Seuraavissa muodoissa käyttää HTTP siirtää tietoja ja saada edut HTTP välimuistiin. Valitse live streaming todellinen video ja ääni tiedot jaetaan Vaillinaiset lauseet ja tämän yksittäisiä Vaillinaiset lauseet Hae välimuistiin CDN. Vain tietojen on oltava päivitetyt on luettelon tiedot. CDN päivittää säännöllisesti luettelon tiedot.

K: onko Azure Media-palveluiden tue tallentamista kuvia?

Vastaus: Jos etsit vain tallentaa JPEG tai PNG kuvia, pidä kaltaisia Azure-Blob-objektien tallennustilaan. Ei ole hyötyä sijoittamalla Media Services-tilisi paitsi, jos haluat säilyttää ne liittyvät videon tai äänen resurssit. Tai jos sinun on ehkä tarvitse käyttää kuvien kerrokset videon encoder. Media Encoder Standard tukee peitto-kuvia, videoita päälle ja mitä se on lueteltu JPEG ja PNG tuetuksi on Lisää muotoja. Lisätietoja on artikkelissa [Luominen päällekkäiset](media-services-custom-mes-presets-with-dotnet.md#overlay).

K: Miten voin kopioida varat Media Services-tililtä toiselle.

V: Voit kopioida varat Media Services-tililtä toiselle käyttämällä .NET-menetelmällä [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) tunniste käytettävissä [Azure Media Services .NET SDK-laajennusten](https://github.com/Azure/azure-sdk-for-media-services-extensions/) säilössä. Lisätietoja on artikkelissa [keskustelupalstalla säikeen](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) .

K: mitkä ovat tuetut merkit nimeämiseen tiedostot, kun käsittelet AMS?

A: Media Services käyttää IAssetFile.Name-ominaisuuden arvon luotaessa URL-osoitteet streaming sisällön (esimerkiksi http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Tästä syystä URL-koodausta ei sallita. Ominaisuuden **nimi** ei saa olla seuraavia [prosentti-koodaus-varattu merkit](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Lisäksi voi olla vain yksi '. " Saat tunnistetta.


K: miten muodostaa yhteyden muille käyttäjille?

A: kun muodostettu yhteys https://media.windows.net, saat 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut [yhteyden muodostaminen Media-palveluita käyttämällä REST API](media-services-rest-connect-programmatically.md)kuvattuja uusi URI-osoitteeseen. 


K: miten videon voit kiertää koodauksen aikana.

A: [Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) tukee kiertoa 90/180/270 kulmista mukaan. Oletusarvoisen toimintatavan on "Automaattinen", jossa se yrittää tunnistaa kierto-metatiedot saapuvien MP4/MOV tiedoston ja sen. Sisällytä seuraavat **tietolähteiden** elementti, hiirellä yhtä suorakulmion json Esiasetukset määritetyn [tähän](http://msdn.microsoft.com/library/azure/mt269960.aspx):
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
