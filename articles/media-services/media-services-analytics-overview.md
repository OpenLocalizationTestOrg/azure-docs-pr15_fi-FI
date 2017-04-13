<properties
    pageTitle="Azure Media Services-palveluiden Analytics yleiskatsaus | Microsoft Azure"
    description="Azure Media-palvelut on Azure Media Analytics, puhe ja tietokoneen Visio services yrityksen mittakaava, yhteensopivuuden, suojaus ja yleisen reach kokoelma julkisen esikatselu. Azure Media Analytics-palvelut on suunniteltu core Azure Media Services käyttöympäristön osien, joten ne ovat valmiina käsittelemään media käsittelyn tasolla päivän pohjalta. "
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/24/2016"   
    ms.author="milanga;juliako;johndeu"/>

# <a name="azure-media-services-analytics-overview"></a>Azure Media Services-palveluiden Analytics yleiskatsaus

##<a name="overview"></a>Yleiskatsaus

Lisää organisaatiot ja yritykset ovat käsittää video kuin niiden työntekijöille, osallistuminen niiden asiakkaiden ja asiakirjan liiketoimintatoimintojen ensisijainen Normaali. Cloud tietojenkäsittely on tehokas tallentamiseen, virtauttaa ja käyttää näitä suuria mediatiedostoja, mutta yritykset kasvaa niiden sisällön videokirjaston, kun ne on oltava tasaisesti tehokas tapa talteen uusia johtopäätöksiä videosta, jotta he voivat luoda paremmin kuvaavaksi mukautetut niiden käyttäjäryhmien aktiviteettien ja toteuttaa käyttävät yritykset edistynyt.

Osoite tämä Marketplacesta tarpeeseen Azure Media Services on Media Analytics-kokoelma, puhe ja vision komponentit (yrityksen mittakaava, yhteensopivuuden, suojaus ja yleisen reach), jotka helpottavat organisaatiot ja yritykset saada suoritettavia havainnollistamisen niiden videon tiedostoista. Azure Media Analytics-palvelut on suunniteltu core Azure Media Services käyttöympäristön osien, joten ne ovat valmiina käsittelemään media käsittelyn tasolla päivän pohjalta.

Azure Media-analyysin avulla kehittäjät vision ominaisuuksia videon rajoitettu tasolla käytön aloittaminen nopeasti ja Tuo tämä lisäominaisuuksia sovelluksiin. Azure Media Analytics muodostanut käytettävän yritysympäristössä asteikon, yhteensopivuuden, suojaus ja yleisen reach vaatii suurissa organisaatioissa.

Seuraavassa kaaviossa on esitetty **Media analyysin** ja muut Media Services-ympäristön tärkeimmät osat. 

![VoD työnkulku](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)

Media Analytics media suorittimien tuottaa MP4-tiedostoja tai JSON-tiedostoja. Jos media-suoritin MP4-tiedoston, voit ladata tiedoston asteittain. Jos media-suoritin JSON-tiedoston, tiedosto voi ladata Azure-blob-säiliö. 

## <a name="azure-media-analytics-services"></a>Azure Media Analytics-palvelut

- **Indeksointitoiminto** – Azure Media indeksointitoiminto avulla voit tehdä sisällön etsittävän, sekä luoda suljettu tekstitys raidat. Azure Media-palveluiden julkaistu **Azure Media indeksointitoiminto 2 Preview** nopeammin indeksoinnin ja laajempi kielten tuesta. Tuetut kieliä ovat englanti, espanja, ranska, saksa, italia, kiina, portugali ja arabia. Yksityiskohtaisia ohjeita ja esimerkkejä artikkelissa [prosessin videot Azure Media indeksointitoiminto 2](media-services-process-content-with-indexer2.md)
 
- **Hyperlapse** – Microsoft Hyperlapse on saatu 20 vuoden tietokoneen näkö Oheistiedot-palvelussa Microsoft Researchin (MSR), videon vakauttamista ja aikaa lapsing luominen nopeasti, kulutettavia, laadukkaita videoita pitkä lomakkeesta sisällön yhdistäminen. Lisäksi luodaan aika raukeaa, voit käyttää myös Hyperlapse vakaata videot luominen shaky videot tallennetaan matkapuhelimet ja videokamerat kautta. Yksityiskohtaisia ohjeita ja esimerkkejä artikkelissa [Hyperlapse mediatiedostojen Azure Media Hyperlapse kanssa](media-services-hyperlapse-content.md)
 
- **Liikerata-tunnistaminen** – tämän palvelun avulla voit tunnistaa liikerata ja taustamallit taustat video. Tämä sopii asiakkaat, jotka haluat etsiä tunnistettujen liikerata tapahtumista havaitsemien valvonta videovirtaa, kamerat valvonta. Yksityiskohtaisia ohjeita ja esimerkkejä artikkelissa [Azure Media Analytics liikerata tunnistamista](media-services-motion-detection.md).
 
- **Hymiö tunnistaminen ja hymiö tunteet** – tämän palvelun avulla voit tunnistaa ihmisten hymiöiden ja niiden tunteet, mukaan lukien onnea, sadness, muutokset yllätä, anger, contempt, jäänyt, disgust ja indifference/nolla. Tämä on useita hyödyllisiä alan sovellukset seuraavalla tavalla, yhdistäminen ja analysoiminen epätoivotuista henkilöiden tapahtuman osallistumisesta. Yksityiskohtaisia ohjeita ja esimerkkejä artikkelissa [hymiö ja Azure Media Analytics tunteet tunnistamista](media-services-face-and-emotion-detection.md).
 
- **Videon yhteenveto** – Video yhteenvedon avulla voit luoda yhteenvetojen pitkä videoita valitsemalla kiinnostavat katkelmat automaattisesti lähde-video. Tästä on hyötyä, kun haluat lisätä pikaisesti toiminta pitkä video. Yksityiskohtaisia ohjeita ja esimerkkejä artikkelissa [Käytä Azure Media Video pikkukuvat Video yhteenvedon luominen](media-services-video-summarization.md)

- **Optisen merkkien tunnistuksen** - Azure Media Analytics OCR (optisen merkkien tunnistuksen) avulla voit muuntaa videotiedostoja sisällön tekstissä digitaalisen tekstiä voi muokata, etsittävän. Näin voit automatisoida kuvaava metatietojen purkaminen videon signaalin mediatiedostot.
 
- **Skaalattavissa hymiö redaction** - **Azure Media Redactor** on Azure-Media Analytics MP, joka tarjoaa skaalattava hymiö redaction pilveen. Hymiö redaction avulla voit muokata videon jotta pehmennystä valittujen henkilöiden sivua muodostaa. Haluat ehkä käyttää hymiö redaction palvelua julkisen turvallisuus- ja uutisia media tilanteissa. Muutaman minuutin, joka sisältää useita naamoja videomateriaali voi kestää tuntia redact manuaalisesti, mutta tämän palvelun avulla hymiö redaction prosessi edellyttää muutaman helpon vaiheen. Lisätietoja [Tässä](media-services-face-redaction.md) artikkelissa.

 
## <a name="common-scenarios"></a>Yleisiä tilanteita, joissa

Alla on muutamia skenaarioita, jossa Azure Media Analytics voi auttaa organisaatioita ja yli teollisuuden yritysten glean uusia johtopäätöksiä videosta, voit luoda mukautettuja yleisön ja työntekijän työt sekä hallita tehokkaammin erittäin paljon videosisältöä:

- **Puhelun keskittää** – parillinen Yhteisöpalvelut, asiakkaan puhelu keskikohdan mukaan helpottamiseksi edelleen asiakkaan palvelutapahtumat suuri prosentteina synnyn kanssa. Koodattu ääni tiedoista on runsaasti asiakkaat, joilla voidaan parantaa tuotteen vaiheittaisiin ja kouluttaminen myös puhelun center työntekijöiden tavoitteet asiakkaan tyytyväisyyttä analysoida tietoja. Käyttämällä Azure Media indeksointitoiminto asiakkaat voivat poimimaan tekstiä ja muodosta haun indeksin luominen ja raporttinäkymien Pura liiketoimintatietojen ympärille yleisimpiä complains, lähteen complains ja muiden tällaisten asianmukaisten tietojen.

- **Käyttäjän luoma sisällön valvonta** –-uutiset media pistorasioita poliisi osastoille monta organisaatioilla on julkinen aukeaman portaaleihin kohtaa, johon ne Hyväksy UGC tietovälineessä, kuten videot ja -kuvat. Sisällön äänenvoimakkuuden voit kasvamaan odottamattoman vuoksi. Näissä tilanteissa on lähellä mahdotonta Järjestä tehokkaita manuaalinen tarkastaminen tarkoituksenmukaisuutta sisällön. Asiakkaiden voit luottaa keskittyä sisältöön, joka sopii sisällön valvonta-palvelusta.

- **Valvonta** - ja IP-kamerat kasvu on Hajotus valvonta videosarja. Tarkistaminen manuaalisesti valvonta video on paljon resursseja ja voi enää ihmisten virhe. Azure Media Analytics on useita osia, kuten liikerata tunnistus, hymiö tunnistaminen ja Hyperlapse ansiosta tarkistaminen, hallinta ja luominen johdannaiset helpompaa.

## <a name="media-services-analytics-media-processors"></a>Media Services-palveluiden Analytics Media-suorittimet 

Tässä osassa on luettelo kaikkien Media Services Analytics Media suorittimien (MP) ja näyttää käyttämisestä .NET tai muiden saat MP-objekti.

### <a name="mp-names"></a>MP nimet


- Azure Media indeksointitoiminto 2 esikatselu
- Azure Media indeksointitoiminto
- Azure Media Hyperlapse
- Azure Media hymiö tunnistus
- Azure Media liikerata tunnistus
- Azure Media Video pikkukuvat
- Azure Media OCR

### <a name="net"></a>.NET

Seuraavan funktion jommassakummassa määritetyn MP nimet ja palaa MP-objekti.

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
            .Where(p => p.Name == mediaProcessorName)
            .ToList()
            .OrderBy(p => new Version(p.Version))
            .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }


## <a name="rest"></a>MUILLE KÄYTTÄJILLE

Pyyntö:

    GET https://media.windows.net/api/MediaProcessors()?$filter=Name%20eq%20'Azure%20Media%20OCR' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer <token>
    x-ms-version: 2.12
    Host: media.windows.net
    
Vastaus:
        
    . . .
    
    {  
       "odata.metadata":"https://media.windows.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:074c3899-d9fb-448f-9ae1-4ebcbe633056",
             "Description":"Azure Media OCR",
             "Name":"Azure Media OCR",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

##<a name="demos"></a>Esittelyt

[Azure Media Analytics esittelyt](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)

##<a name="next-steps"></a>Seuraavat vaiheet

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

[Media-palveluiden Analytics-ilmoitus](https://azure.microsoft.com/blog/introducing-azure-media-analytics/)
  

<!-- Images -->

[overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
