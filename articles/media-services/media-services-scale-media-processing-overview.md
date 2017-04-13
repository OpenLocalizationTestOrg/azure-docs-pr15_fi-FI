<properties
    pageTitle="Skaalaus Media käsittelyn yleiskatsaus | Microsoft Azure"
    description="Tässä artikkelissa on yleiskatsaus skaalauksen Media käsittelyn Azure Media-palvelujen kanssa."
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
    ms.date="08/29/2016"
    ms.author="juliako"/>


# <a name="scaling-media-processing-overview"></a>Skaalaus Media käsittelyn yleiskatsaus

Tämän sivun avulla opit ja miksi skaalata media käsittely. 

## <a name="overview"></a>Yleiskatsaus

Media Services-tilin on liitetty varattu yksikkö-tyyppi, joka määrittää nopeutta, jolla mediatiedostojen käsittelyn tehtävät käsitellään. Voit valita välillä varattu yksikön seuraavanlaisia: **S1**, **S2**tai **S3**. Esimerkiksi saman koodauksen suoritetaan nopeammin käytettäessä **S2** varattu yksikön tyyppi vertaa **S1** tyyppi. Lisätietoja on artikkelissa [Varattu yksikkötyypit](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Lisäksi varattu yksikkötyyppi, joka määrittää, voit määrittää tilisi valmisteleminen varatut resurssit. Valmistellun varattu yksiköiden määrän, määrittää media tehtäviin, jotka voidaan käsitellä samanaikaisesti asiakkaan määrän. Esimerkiksi jos tilillä on viisi varatut resurssit ja sitten viisi media tehtävät toimii samanaikaisesti nopeammin kuin tehtäviä käsitellään. Jäljellä olevat tehtävät odottaa jonossa ja saada noudettu käsittelyyn peräkkäin, kun käynnissä tehtävä loppuu. Jos tiliä ei ole mitään varattu yksiköt valmistelun yhteydessä, valitse tehtävät kerätään peräkkäin. Tässä tapauksessa yksi tehtävä viimeistely ja seuraavaksi yhden aloitus odotusaika riippuu resurssien käytettävyyden järjestelmässä.

## <a name="choosing-between-different-reserved-unit-types"></a>Eri varattu yksikön valitseminen

Seuraavassa taulukossa auttaa sinua valitsemaan päätös, kun valitset eri koodauksen nopeutta. Se myös tarjoaa muutamia ensisijainen tapauksissa ja antaa SAS URL-osoitteet, joiden avulla voit ladata videoita, voit tehdä omia testit:

Skenaariot|**S1**|**S2**|**S3**|
----------|------------|----------|------------
Palvelupyynnön käyttötarkoitus| Yksittäisen bittitaajuus koodaus. <br/>SD tai alapuolella ratkaisut-tiedostoja ei aikaa luottamuksellisia ja edullinen.|Yksittäisen bittitaajuus ja useita bittitaajuus koodaus.<br/>Tavallinen käyttö SD ja HD koodaus. |Yksittäisen bittitaajuus ja useita bittitaajuus koodaus.<br/>Koko HD ja 4K tarkkuus videot. Luottamuksellisia, nopeammin tulosteita koodaus-aika. 
Ensisijainen|[Input-tiedosto: 5 minuuttia pitkä 640x360p osoitteessa 29.97 kehykset/toisen](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Yksittäisen bittitaajuus MP4-tiedoston samaan tarkkuudella koodaus kestää noin 11 minuuttia.|[Kirjoita tiedoston: 5 minuuttia pitkä 1280x720p osoitteessa 29.97 kuvaa sekunnissa](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>"H264 yhden bittitaajuus 720p"-koodaus valmiiksi kestää noin 5 minuuttia.<br/><br/>Koodaus kanssa "H264 useita bittitaajuus 720p" esiasetus kestää noin 11,5 minuuttia.|[Input-tiedosto: 5 minuuttia pitkä 1920x1080p osoitteessa 29.97 kehykset/toisen](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>"H264 yhden bittitaajuus 1080p"-koodaus valmiiksi kestää noin 2.7 minuuttia.<br/><br/>Koodaus kanssa "H264 useita bittitaajuus 1080p" esiasetus kestää noin 5.7 minuuttia.

##<a name="considerations"></a>Huomioon otettavia seikkoja

>[AZURE.IMPORTANT] Huomioon otettavia seikkoja, joka on kuvattu tämän osion.  

- Kaikki media käsitteleminen, mukaan lukien indeksoinnin työt käyttämällä Azure Media indeksointitoiminto parallelizing sovellu varatut resurssit.  Kuitenkin toisin kuin koodaus Indeksointitöiden ei Hae nopeampaa nopeammin varattu yksiköillä.

- Jos jaetun resurssivarannon, eli ilman kaikki varatut resurssit sitten Käytä tehtävien saman suorituskyky on S1 RUs kanssa. On kuitenkin käytössä ei ole yläraja tehtäviä voit keskittyä jonossa-tilaan ja milloin tahansa annetun enintään yksi tehtävä on aika.

- Seuraavat tiedot keskikohdan mukaan ei tarjoa **S2** varattu yksikkötyyppi: Brasilia Etelä, Intia Länsi Intia keskitetyn tai Intia Etelä.

- Seuraavat tiedot keskikohdan mukaan ei tarjoa **S3** varattu yksikkötyyppi: Brasilia Etelä-Intia Länsi Intia keskitetyn.

- 24 tunnin määritettyjen yksikköjen suurinta numeroa käytetään laskeminen kustannukset.


##<a name="quotas-and-limitations"></a>Kiintiöiden ja rajoitukset

Katso tietoja kiintiöiden ja rajoitukset ja avaa tuki lippu [kiintiöiden ja rajoitukset](media-services-quotas-and-limitations.md).

##<a name="next-step"></a>Seuraava vaihe

Saavuttamiseksi skaalauksen media käsittely tehtävän mainitun seuraavia tekniikoita: 

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-encoding-units.md)
- [Portal](media-services-portal-scale-media-processing.md)
- [MUILLE KÄYTTÄJILLE](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
