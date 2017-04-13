<properties 
    pageTitle="Azure API hallinta käytännön viitata" 
    description="Lisätietoja käytettävissä API hallinnan käytännöt." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Azure API hallinta käytännön viitata

Tässä osassa on indeksin koskevat [API hallinta käytännön viitata][]. Tietoja lisäämällä ja määrittämällä käytäntöjä on artikkelissa [käytännöt API hallinnassa][].

Käytännön lausekkeiden voidaan määritearvojen tai tekstiarvojen kaikista API hallintakäytännöt, ellei käytäntö määrittää muuten. Joitakin käytäntöjä, kuten [Hallintavuo][] ja [Määritä muuttujan][] käytännöt perustuvat käytännön lausekkeita. Lisätietoja on artikkelissa [Advanced käytännöt][] ja [käytännön lausekkeet][]

## <a name="policy-reference-index"></a>Käytännön viitata indeksi

-   [Käytön rajoittaminen käytännöt][]
    -   [Tarkista HTTP-otsikko][] - pakottaa olemassaolo ja/tai HTTP-otsikon arvo.
    -   [Raja puhelun korko tilauksen][] - estää Ohjelmointirajapinnan käyttö piikkejä rajoittamalla puhelun korko; kohti tilauksen välein.
    -   [Raja puhelun korko avaimella](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - estää Ohjelmointirajapinnan käyttö piikkejä rajoittamalla puhelun korko; kohti avain välein.
    -   [Rajoita soittajan IP-osoitteet][] - suodattimet (avulla ja estää) puhelut-IP-osoitteet ja/tai osoitealueet.
    -   [Määrittää tilauksen käyttökiintiö][] - voit pakottaa uusia tai elinaika puhelun äänenvoimakkuuden ja/tai kaistanleveyden kiintiön, kohti tilauksen välein.
    -   [Määrittää avaimella käyttökiintiö](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - voit pakottaa uusia tai elinaika puhelun äänenvoimakkuuden ja/tai kaistanleveyden kiintiön, kohti avain välein.
    -   [Vahvista JWT][] - pakottaa olemassaolo ja JWT, joka on määritetty HTTP-otsikon tai määritetyn kyselyparametri poimittujen kelpoisuus.
-   [Lisäasetusten määrittäminen][]
    -   [Hallintavuo][] - koskee ehdollisesti käytännön lauseita totuusarvolausekkeiden [lausekkeiden][]arvioinnin tulosten perusteella.
    -   [Välitä pyynnön][] - välittää pyynnön Taustajärjestelmä-palveluun.
    -   [Tapahtuman keskittimeen log][] - lähettää viestejä [lokin](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) kohteen määrittämä viestin kohde määritetyssä muodossa.
    -   [Yritä](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - uudelleenyritykset suorittamisen oheinen käytännön väittämistä Jos ja kunnes ehto täyttyy. Suorittamisen toista määritetyin aikavälein ja ylöspäin määritettyyn uudelleen määrä.
    -   [Palauttaa vastaus](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - keskeytetään myyntijakso suorittamisen ja palauttaa määritetyn vastauksen suoraan kutsuja.
    -   [Yksisuuntaisen Lähetyspyyntö](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - lähettää pyynnön määritetyn URL-Osoitteen ilman vastausta.
    -   [Lähetä pyyntö](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) - lähettää pyynnön määritettyyn URL-osoitteeseen.
    -   [Määritä pyyntömenetelmä](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - voit vaihtaa HTTP-pyynnössä.
    -   HTTP-tilakoodin [Aseta tila](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - arvoksi määritetty arvo.
    -   [Aseta muuttuja][] - jatkuvat nimetty [kontekstin][] muuttujan myöhemmin käytön arvon.
    -   [Jäljitys](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Lisää merkkijonon tulosteen [API tarkastaminen-toiminnon](../api-management/api-management-howto-api-inspector.md) tulos.
    -   [Odota](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - odottaa oheinen lähettämispyyntö, Get-arvon välimuistin tai hallita työnkulku käytäntöjä loppuun, ennen kuin jatkat.
-   [Käyttöoikeuksien määrittäminen][]
    -   [Tarkista Basic][] - Tarkista Taustajärjestelmä-palveluun käyttämällä perustodentamista.
    -   [Tarkista Asiakasvarmenne kanssa][] - Tarkista asiakkaan varmenteiden käyttäminen Taustajärjestelmä-palvelun kanssa.
-   [Välimuistin käytännöt][] 
    -   [Pyydä välimuisti][] - suorittaa välimuistin hakeminen ja palauttaa kelvollinen välimuistissa vastaus kun niitä on saatavilla.
    -   [Säilön välimuistiin][] - tallentaa vastauksen määritetyn välimuistin ohjausobjektia-määritysten mukaisesti.
    -   [Hae välimuistista arvo](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) - Nouda avaimella välimuistissa olevan kohteen.
    -   [Välimuistin arvo store](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - kaupan avaimella välimuistin kohdetta.
    -   [Poista arvo välimuistista](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - Poista välimuistin avaimella kohdetta.
-   [Toimintojen välinen toimialueen käytännöt][] 
    -   [Toimialueiden puhelujen][] - on suunniteltu Ohjelmointirajapinnan Adobe Flash- ja Microsoft Silverlight selainpohjaisia asiakkaiden.
    -   [CORS][] - Lisää rajat origin resurssien jakaminen toiminnon tai Ohjelmointirajapinnan sallimaan toimialueiden puhelut selainpohjaisia asiakkaiden tuki (CORS).
    -   [JSONP][] - toiminnon täyttäminen (JSONP) tukee Lisää JSON tai Ohjelmointirajapinnan Salli JavaScript selainpohjaisia asiakkaiden toimialueiden puhelut.
-   [Muunnoksen määrittäminen][] 
    -   [Muuntaa JSON XML][] - muuntaa pyynnön ja vastauksen teksti JSON XML-muotoon.
    -   [JSON muuntaa XML][] - muuntaa pyynnön ja vastauksen teksti XML: stä JSON-muotoon.
    -   [Etsi ja korvaa tekstissä merkkijono][] - pyynnön ja vastauksen alimerkkijonon etsii ja korvaa sen eri alimerkkijonon.
    -   [Syöttörajoitteen URL-osoitteista sisältö][] - vastauksen kirjoittaa uudelleen (peitteitä) linkkejä teksti niin, että ne Valitse vastaava linkki yhdyskäytävän kautta.
    -   Saapuvan pyynnön Taustajärjestelmä palvelu [Taustajärjestelmä-palvelun määrittäminen][] - muuttuu.
    -   [Määritä leipäteksti][] - määrittää pyyntöjen saapuvan ja lähtevän viestin tekstiosaan.
    -   [Määritä HTTP-otsikko][] - arvo määrittää aiemmin vastauksen ja/tai pyynnön otsikossa tai lisää uuden vastauksen ja/tai pyynnön otsikon.
    -   [Määritä kyselymerkkijonoparametrin][] – Lisää, korvaa arvon tai poistaa pyytää kyselymerkkijonoparametrin.
    -   [Uuden esimerkkitekstin URL][] - muuntaa pyyntö URL-Osoitteen julkiseen muodossaan lomakkeen oletettu web-palvelu.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja käytännön rajoituksista on artikkelissa seuraavassa videossa.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Käytön rajoittaminen käytännöt]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Tarkista HTTP-otsikko]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Raja puhelun korko tilauksen]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Rajoita soittajan IP-osoitteet]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Määritä käyttökiintiö tilauksen]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Vahvista JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Lisäasetusten määrittäminen]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Hallintavuo]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Aseta muuttuja]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[lausekkeet]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[konteksti]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Kokouspyynnön lähettäminen edelleen]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Kirjaudu tapahtumaa-toiminnossa]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Käyttöoikeuksien määrittäminen]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Basic todentamismenetelmä]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Asiakasvarmenne todentamismenetelmä]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Välimuistin käytännöt]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Välimuistin noutaminen]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Välimuistiin tallentaminen]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Toimintojen välinen toimialueen käytännöt]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Salli toimialueiden puhelut]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Muunnoksen määrittäminen]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[JSON muuntaminen XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Muuntaa XML JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Etsi ja korvaa merkkijonon tekstissä]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Sisällön syöttörajoitteen URL-osoitteet]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Määritä Taustajärjestelmä palvelu]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Määritä tekstissä]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Määritä HTTP-otsikko]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Kyselymerkkijonoparametrin määrittäminen]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Uuden esimerkkitekstin URL-osoite]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Käytännöt API hallinta]: api-management-howto-policies.md
[API hallinta käytännön viitata]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Käytännön lausekkeet]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
