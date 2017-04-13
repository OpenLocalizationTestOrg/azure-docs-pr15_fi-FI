<properties
    pageTitle="Oman ohjelmointirajapinnan Azure API hallinta, tapahtuman keskittimet ja Runscope valvonta"
    description="Osoittaa lokin eventhub käytännön yhdistäviä Azure API hallinta, Azure tapahtuman keskittimet ja HTTP seurantaan ja valvontaan Runscope sovelluksen malli"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="monitor-your-apis-with-azure-api-management-event-hubs-and-runscope"></a>Oman ohjelmointirajapinnan Azure API hallinta, tapahtuman keskittimet ja Runscope valvonta

[API hallintapalvelun](api-management-key-concepts.md) tarjoaa useita ominaisuuksia, jotka parantavat lähetetään HTTP-Ohjelmointirajapinnan pyyntöjen käsittely. Olemassaolo pyynnöt ja vastaukset ovat kuitenkin lyhytkestoisia. Pyyntö on tehty ja se kulkee API hallintapalvelun, että Taustajärjestelmä API. Oman API käsittelee pyynnön ja vastauksen jatkuu taaksepäin API-asiakkaaksi. API hallintapalvelun pitää joitakin tärkeitä tilastotietoja näkyminen ohjelmointirajapinnan Publisher portaalin koontinäytön, mutta palkista, että tiedot eivät ole käytettävissä.

API hallintapalvelun [lokin eventhub](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub) [käytännön](api-management-howto-policies.md) avulla voit lähettää kaikki tiedot, pyynnön ja vastauksen [Azure tapahtumaa-toiminnossa](../event-hubs/event-hubs-what-is-event-hubs.md). On useita syitä, miksi kannattaa luoda tapahtumien HTTP viestit on lähetetty oman API. Esimerkkejä kirjausketju päivitykset, käyttöanalyysin, poikkeuksen ilmoitat ja 3 osapuolen integroinnit.   

Tässä artikkelissa kerrotaan, miten siepata koko HTTP-pyynnön ja vastauksen viestin, Lähetä se tapahtumaa-toiminnossa ja välittää viestin kolmannelle osapuolelle palvelun, joka tarjoaa HTTP seurantaan ja valvontaan palvelut.

## <a name="why-send-from-api-management-service"></a>Miksi lähettäminen API hallintapalvelun?
On mahdollista kirjoittaa HTTP middleware, voit liittää HTTP-Ohjelmointirajapinnan kehysten syötteen ne seurantaan ja valvontaan järjestelmien ja HTTP-pyynnöt ja vastaukset. Entä huonot puolet tämän menetelmän on HTTP-middleware on Taustajärjestelmä API integroida ja Ohjelmointirajapinnan ympäristö on vastattava. Jos määritettynä on useita API yksitellen on käyttöönotto middleware. On usein syitä, miksi Taustajärjestelmä API ei voi päivittää.

Integroi kirjaamisen infrastruktuuria Azure API Management-palvelun avulla on keskitetty ja ympäristöstä riippumaton ratkaisu. On myös skaalattava osittain vuoksi Azure API hallinta [geo replikoinnin](api-management-howto-deploy-multi-region.md) ominaisuuksia.

## <a name="why-send-to-an-azure-event-hub"></a>Miksi lähettää Azure-tapahtumaa-toiminnossa?
Onko järkevää Kysy, miksi luoda käytännön, joka on Azure tapahtuman porttiin? On monesta eri paikasta, jossa voin saatat haluta kirjautua Omat pyynnöt. Miksi ei vain lähettää pyynnöt suoraan lopullinen kohde?  Tämä on haluamasi vaihtoehto. Kuitenkin tehtäessä API-hallintapalvelun pyynnöt kirjaaminen on tarpeen ottaa huomioon, miten viestien lokiin kirjaamisen vaikuttaa suorituskykyyn Ohjelmointirajapinnan. Lataa asteittain lisäykset voi ratkaista suurentamalla käytettävissä olevat esiintymät järjestelmäosia tai hyödyntämistä geo replikoinnin. Kuitenkin lyhyt piikkarit liikenteen heikentää pyynnöt viivästyä huomattavasti, jos kirjaamisen infrastruktuuria pyynnöt Käynnistä hidastaa kuormitus.

Azure tapahtuman keskittimet on suunniteltu tunkeutumisen erittäin suuri tietomäärien, kapasiteetin käsittely tapahtumien paljon suurempi määrä kuin HTTP määrän pyytää useimmat API prosessin kanssa. Tapahtuma-toiminto toimii eräänlainen kehittyneitä puskurin API hallintapalvelun ja infrastruktuurin tallennettavia ja viestit käsitellä välillä. Näin varmistat, että API suorituskyvyn ei haittaohjelman vuoksi kirjaamisen-infrastruktuuria.  

Kun tiedot on välitetty tapahtuma-keskittimeen, se on samanlainen ja odottaa käsittely kuluttajille tapahtumaa-toiminnossa. Tapahtuma-toiminnossa ei huolellisesti, miten se käsitellään, se vain asiana varmistetaan, että viesti toimitettu onnistuneesti.     

Tapahtuman keskittimet on useita ryhmiä stream tapahtumien mahdollisuus. Näin kokonaan eri järjestelmien käsittelemien tapahtumien. Näin tukevat integrointi skenaarioita siirtämättä lisäys viiveitä API Management-palvelun API-pyynnön käsittelyä vain yhden tapahtuman tarpeiden luodaan.

## <a name="a-policy-to-send-applicationhttp-messages"></a>Käytännön sovelluksen/HTTP-yhteyden viestien lähettäminen
Tapahtuma-toiminnossa hyväksyy tapahtumatietoja yksinkertainen merkkijonona. Merkkijono sisältö on täysin itse. Voi pakata määrittäminen HTTP-pyyntö ja lähetä se tapahtuman porttiin on muokattava merkkijonon, jossa pyynnön ja vastauksen tiedot. Tilanteissa, onko aiemmin luodusta muotoilusta emme voi käyttää uudelleen ja valitse emme ehkä ole kirjoittaa oman koodin jäsennys. Alun perin voin pitää käyttämällä [OLEMME](http://www.softwareishard.com/blog/har-12-spec/) lähettämiseen HTTP-pyynnöt ja vastaukset. Kuitenkin tässä muodossa on tarkoitettu yritystietojen tallentamiseen järjestyksen pyyntöjen perusteella JSON-muodossa. Siinä oli pakollinen elementtejä, jotka lisätään tarpeettomat monimutkaisuus läpäistä HTTP-sanoman ‑toiminto skenaarion määrä.  

Vaihtoehtoinen asetus on käytettävä `application/http` mediatyyppi kuvatulla tavalla [RFC 7230](http://tools.ietf.org/html/rfc7230)HTTP-määritys. Tätä tyyppiä käyttää täsmälleen samassa muodossa, jota käytetään HTTP-viestien lähettäminen todella ‑toiminto, mutta koko viesti voidaan sijoittaa toiseen HTTP-pyyntö tekstiosaan. Tässä tapauksessa vain tässä tapauksessa tapahtuman porttiin Lähetä viestinä Microsoftin tekstiin avulla. Kätevästi, on jäsentimen, joka sijaitsee [Microsoft ASP.NET Web API 2.2 asiakkaan](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kirjastoja, joissa voi jäsentää Tämä muoto ja muuntaa alkuperäisessä `HttpRequestMessage` ja `HttpResponseMessage` objekteja.

Jos haluat luoda viestiä, voit hyödyntää C# annettava perusteella Azure API hallinta [käytännön lausekkeita](https://msdn.microsoft.com/library/azure/dn910913.aspx) . Seuraavassa on käytännön, joka lähettää HTTP pyynnön viestin Azure tapahtuman porttiin.

       <log-to-eventhub logger-id="conferencelogger" partition-id="0">
       @{
           var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                       context.Request.Method,
                                                       context.Request.Url.Path + context.Request.Url.QueryString);

           var body = context.Request.Body?.As<string>(true);
           if (body != null && body.Length > 1024)
           {
               body = body.Substring(0, 1024);
           }

           var headers = context.Request.Headers
                                  .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                  .ToArray<string>();

           var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

           return "request:"   + context.Variables["message-id"] + "\n"
                               + requestLine + headerString + "\r\n" + body;
       }
       </log-to-eventhub>

### <a name="policy-declaration"></a>Käytännön ilmoitus
Tietyn asioita nykyarvo mainitseminen tietoja käytännön lauseketta. Log eventhub käytäntö on nimeltään lokin-tunnus, joka viittaa lokin, joka on luotu API Management-palvelun nimi-määrite. Määrittäminen-toiminnossa-tapahtumalokin API Management-palvelun tiedot löytyvät asiakirjan [Kirjautuminen tapahtumien Azure tapahtuman porttiin Azure API hallinnassa](api-management-howto-log-event-hubs.md). Second-määrite on valinnainen parametri, joka ohjaa tapahtuman keskittimet, joka tallentaa viestin osio. Tapahtuman keskittimet avulla osioiden scalabilty ottaminen käyttöön ja edellyttää vähintään kaksi. Järjestetty viestien toimittamisen vain taata osion kuluessa. Jos emme ole opasta tapahtuma-toiminnossa voit sijoittaa viesti-osiossa, se käyttää pyöreän pöydän algoritmin kuormituksen jakamista varten. Kuitenkin, joka saattaa aiheuttaa joidenkin Microsoftin viestit käsitellään epäjärjestyksessä.  

### <a name="partitions"></a>Osiot
Jotta meidän viestit toimitetaan kuluttajille järjestyksessä ja hyödyntää osioiden kuormituksen jakauman-ominaisuutta, I on HTTP-pyyntö lähettämiseen yksi osio ja toinen osio HTTP vastauksen viestejä. Näin varmistat, parillinen kuormituksen jakelu ja emme voi taata, että kaikki palvelupyynnöt käytetty järjestyksessä ja kaikki vastaukset käytetty järjestyksessä. On mahdollista kulutettu ennen vastaavaa pyyntöä vastausta, mutta, joka ei ole ongelma, kun on varten hajautettuna eri järjestelmä pyytää vastauksia ja Microsoft tietää, että pyynnöt aina annettava ennen vastaukset.

### <a name="http-payloads"></a>HTTP-paketteja
Rakennuksen jälkeen `requestLine` tarkistetaan nähdäksesi, jos pyyntö teksti katkaistaan. Pyyntö tekstiin katkaistaan vain 1 024. Tämä voitaisiin lisätä, mutta yksittäisten tapahtumaa-toiminnossa viestien on oikeutettu enintään 256 kt, joten on todennäköistä, että jotkin HTTP-sanoman suorittamista näkyy yksi viesti ei mahdu. Kun teet kirjaaminen ja analysoinnin merkittäviä tietomäärän voi johtaa vain HTTP-pyyntö rivillä ja otsikot. Myös monet API pyynnöt palauttaa vain pienen laitokset ja eli tiedot arvo katkaistaan suuri laitokset tappio on suhteellisen vähän siirron, käsittely ja tallennustilaa kustannusten pitämään kaikki tekstin vähentäminen verrattuna. Tekstiin käsittelemistä koskeva lopullinen OneNoten on annettava välittää `true` , on<string>()-menetelmä koska olemme lukevat tekstin, mutta on myös haluat Taustajärjestelmä API voivat lukea tekstissä. Mukaan kulkee arvon TRUE arvoksi Tämä menetelmä aiheuttaa jotakin ellei, jotta se voidaan lukea toisen kerran. Tämä on tärkeää tietää, jos sinulla on Ohjelmointirajapinta, joita ei hyvin suurten tiedostojen lataaminen tai käyttää pitkä kysely. Tällaisissa tapauksissa olla Vältä lukeminen tekstiin ollenkaan.   

### <a name="http-headers"></a>HTTP-otsikot
HTTP-otsikot voit yksinkertaisesti siirretään päälle viestimuodon yksinkertainen avain/arvo pari muodossa. Microsoft on valinnut poistaa tiettyjä suojauksen luottamuksellisia kenttiä, voit välttää vuoda tarpeettoman tunnistetietoja. On epätodennäköistä, että API näppäimet ja muiden tunnistetietojen voidaan käyttää analytics tarkoituksiin. Jos olemme haluat käyttäjän ja tietyn tuotteen analyysin suorittamiseen osallistujat käyttävät sitten on voitu hakea, valitse `context` objekti ja lisätä sen viestiin.     
### <a name="message-metadata"></a>Viestin metatiedot
Sanoma tapahtumaa-toiminnossa lähettäminen luotaessa ensimmäisellä rivillä ei ole itse asiassa osa `application/http` viestin. Ensimmäinen rivi on metatietoja koostuva onko viestin pyyntö tai vastausviesti ja viestin tunnuksen koon muuttamiseen tarkoitettu yhdistää pyynnöt ja vastaukset. Viestin tunnuksen luodaan käyttäen toisen käytännön, joka näyttää tältä:

    <set-variable name="message-id" value="@(Guid.NewGuid())" />

Emme voi luonut pyynnön viestin, tallentaa, muuttujan ennen kuin vastaus on palautettu ja sitten yksinkertaisesti lähettää pyynnön ja vastauksen käyttäminen yhdessä viestissä. Kuitenkin pyynnön ja vastauksen lähettäminen itsenäisesti ja yhdistää kaksi viestin tunnuksen, emme saa hieman joustavammin viestin koko-ominaisuuden, jonka avulla voit hyödyntää useita osioita, kun viestin tilaus ja pyyntö tulee näkyviin Microsoftin kirjaaminen Raporttinäkymät-ikkunan nopeammin. Myös ehkä joissakin tilanteissa, jossa vastausta ei lähetetä tapahtumaa-toiminnossa mahdollisesti-Ohjelmointirajapinnan hallintapalvelun vakava pyynnön virheen takia, mutta on edelleen tietueen pyynnön.

Käytännön Lähetä vastaus HTTP viesti näyttää muistuttaa pyynnön ja siten koko käytännön määritys näyttää tältä:

      <policies>
        <inbound>
            <set-variable name="message-id" value="@(Guid.NewGuid())" />
            <log-to-eventhub logger-id="conferencelogger" partition-id="0">
              @{
                  var requestLine = string.Format("{0} {1} HTTP/1.1\r\n",
                                                              context.Request.Method,
                                                              context.Request.Url.Path + context.Request.Url.QueryString);

                  var body = context.Request.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Request.Headers
                                       .Where(h => h.Key != "Authorization" && h.Key != "Ocp-Apim-Subscription-Key")
                                       .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                       .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "request:"   + context.Variables["message-id"] + "\n"
                                      + requestLine + headerString + "\r\n" + body;
              }
          </log-to-eventhub>
        </inbound>
        <backend>
            <forward-request follow-redirects="true" />
        </backend>
        <outbound>
            <log-to-eventhub logger-id="conferencelogger" partition-id="1">
              @{
                  var statusLine = string.Format("HTTP/1.1 {0} {1}\r\n",
                                                      context.Response.StatusCode,
                                                      context.Response.StatusReason);

                  var body = context.Response.Body?.As<string>(true);
                  if (body != null && body.Length > 1024)
                  {
                      body = body.Substring(0, 1024);
                  }

                  var headers = context.Response.Headers
                                                  .Select(h => string.Format("{0}: {1}", h.Key, String.Join(", ", h.Value)))
                                                  .ToArray<string>();

                  var headerString = (headers.Any()) ? string.Join("\r\n", headers) + "\r\n" : string.Empty;

                  return "response:"  + context.Variables["message-id"] + "\n"
                                      + statusLine + headerString + "\r\n" + body;
             }
          </log-to-eventhub>
        </outbound>
      </policies>

`set-variable` Käytännön Luo arvon, joka on käytettävissä sekä `log-to-eventhub` käytännön `<inbound>` osan ja `<outbound>` osa.  

## <a name="receiving-events-from-event-hubs"></a>Tapahtumien vastaanottamisen tapahtuman keskittimet
Azure tapahtuman pääsivuston tapahtumia saapuvat [AMQP-protokollan](http://www.amqp.org/)avulla. Microsoft Service Bus ryhmän tehnyt asiakkaan kirjastojen käytettävissä helpottaa vievää tapahtumat. Tuetut kahdella eri tavalla, yksi parhaillaan *Suoraan kuluttajille* ja ja toinen `EventProcessorHost` luokka. Esimerkkejä kaksi ‑ominaisuuksia löytyy [Tapahtuman keskittimet Programming Guide](../event-hubs/event-hubs-programming-guide.md). Erot lyhyen on `Direct Consumer` avulla voit hallita ja `EventProcessorHost` onko osa putkisto tiedoista, mutta tekee tiettyjä oletuksia siitä, miten käsiteltävä tapahtumat.  

### <a name="eventprocessorhost"></a>EventProcessorHost
Tässä esimerkissä Käytämme `EventProcessorHost` yksinkertaisuuden, se voi kuitenkin ei tietyn artikkelista paras vaihtoehto. `EventProcessorHost`ei, varmistetaan, että sinun ei tarvitse huolehtia popierių ongelmat tietyn tapahtuman suoritin luokassa työtä. Kuitenkin skenaariossa on ovat vain viestin muuntaminen toiseen muotoon ja siirtämällä sitä toisen palvelun asynkroninen-menetelmällä. Ei ole tarpeen päivityksessä jaettua tilaa ja näin ollen ole riskiä popierių ongelmat. Useimmat tilanteessa `EventProcessorHost` on todennäköisesti paras valinta, ja se on varmasti helpompi vaihtoehto.     

### <a name="ieventprocessor"></a>IEventProcessor
Keskeinen käsite käytettäessä `EventProcessorHost` on luotava toteutus `IEventProcessor` käyttöliittymä, joka sisältää menetelmä `ProcessEventAsync`. Tämä menetelmä keskeisimmät on mukana:

  Tehtävän IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages) {asynkroninen

           foreach (EventData eventData in messages)
           {
               _Logger.LogInfo(string.Format("Event received from partition: {0} - {1}", context.Lease.PartitionId,eventData.PartitionKey));

               try
               {
                   var httpMessage = HttpMessage.Parse(eventData.GetBodyStream());
                   await _MessageContentProcessor.ProcessHttpMessage(httpMessage);
               }
               catch (Exception ex)
               {
                   _Logger.LogError(ex.Message);
               }
           }
            ... checkpointing code snipped ...
        }

Luettelo EventData objekteista on välitetty menetelmä ja olemme käytöstä luettelossa päälle. Kummassakin menetelmässä tavua ovat jäsentää HttpMessage objektiksi ja objektin välitetään IHttpMessageProcessor esiintymä.

### <a name="httpmessage"></a>HttpMessage
`HttpMessage` Esiintymä sisältää kolme laitteita tiedot:

      public class HttpMessage
       {
           public Guid MessageId { get; set; }
           public bool IsRequest { get; set; }
           public HttpRequestMessage HttpRequestMessage { get; set; }
           public HttpResponseMessage HttpResponseMessage { get; set; }

        ... parsing code snipped ...

      }

`HttpMessage` Esiintymä sisältää `MessageId` GUID-tunnus, joka pystyy HTTP-pyyntö muodostaa vastaavan HTTP-vastauksen ja totuusarvon, joka ilmaisee, onko objekti sisältää HttpRequestMessage ja HttpResponseMessage esiintymä. HTTP luokkia myös sisäinen avulla `System.Net.Http`, voin voitiin hyödyntää `application/http` jäsennyksen, joka sisältyy koodi `System.Net.Http.Formatting`.  

### <a name="ihttpmessageprocessor"></a>IHttpMessageProcessor
`HttpMessage` Esiintymän välitetään sitten soveltaminen `IHttpMessageProcessor` joka on luotu irrottamisen vastaanottaminen ja tulkinnassa tapahtuman Azure tapahtuma-toiminnosta ja sen suorittava käyttöliittymään.


## <a name="forwarding-the-http-message"></a>HTTP-viestin välittäminen
Tässä esimerkissä voin päättää kiinnostavat push HTTP-pyyntö päälle [Runscope](http://www.runscope.com)olisi. Runscope on pilvipohjainen-palvelu, specializes HTTP virheenkorjaus, seurantaan ja valvontaan. Heillä on vapaa taso, niin voit kokeilla helposti ja sen avulla voimme nähdä HTTP-pyyntöihin-reaaliaikainen juoksutus Microsoftin API Management-palvelun kautta.

`IHttpMessageProcessor` Käyttöönoton näyttää tältä

      public class RunscopeHttpMessageProcessor : IHttpMessageProcessor
       {
           private HttpClient _HttpClient;
           private ILogger _Logger;
           private string _BucketKey;
           public RunscopeHttpMessageProcessor(HttpClient httpClient, ILogger logger)
           {
               _HttpClient = httpClient;
               var key = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-KEY", EnvironmentVariableTarget.User);
               _HttpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("bearer", key);
               _HttpClient.BaseAddress = new Uri("https://api.runscope.com");
               _BucketKey = Environment.GetEnvironmentVariable("APIMEVENTS-RUNSCOPE-BUCKET", EnvironmentVariableTarget.User);
               _Logger = logger;
           }

           public async Task ProcessHttpMessage(HttpMessage message)
           {
               var runscopeMessage = new RunscopeMessage()
               {
                   UniqueIdentifier = message.MessageId
               };

               if (message.IsRequest)
               {
                   _Logger.LogInfo("Sending HTTP request " + message.MessageId.ToString());
                   runscopeMessage.Request = await RunscopeRequest.CreateFromAsync(message.HttpRequestMessage);
               }
               else
               {
                   _Logger.LogInfo("Sending HTTP response " + message.MessageId.ToString());
                   runscopeMessage.Response = await RunscopeResponse.CreateFromAsync(message.HttpResponseMessage);
               }

               var messagesLink = new MessagesLink() { Method = HttpMethod.Post };
               messagesLink.BucketKey = _BucketKey;
               messagesLink.RunscopeMessage = runscopeMessage;
               var runscopeResponse = await _HttpClient.SendAsync(messagesLink.CreateRequest());
               _Logger.LogDebug("Request sent to Runscope");
           }
       }

Voin voitiin hyödyntää [Runscope aiemmin asiakkaan kirjasto](http://www.nuget.org/packages/Runscope.net.hapikit/0.9.0-alpha) , joka on helppo push `HttpRequestMessage` ja `HttpResponseMessage` ylöspäin liikkeelle niiden esiintymät. Jotta voit käyttää Runscope-Ohjelmointirajapinnan on tili ja API-näppäintä. [Sovellusten luomisesta Access Runscope API](http://blog.runscope.com/posts/creating-applications-to-access-the-runscope-api) ruutukaappausvideo löytyy ohjeet käytön API-näppäintä.

## <a name="complete-sample"></a>Valmis malli
[Lähdekoodin](https://github.com/darrelmiller/ApimEventProcessor) ja testaa näytteen ovat Github. Tarvitset [Ohjelmointirajapinnan hallintapalvelun](api-management-get-started.md), [Yhdistetyt tapahtumaa-toiminnossa](api-management-howto-log-event-hubs.md)ja [Tallennustilan tilin](../storage/storage-create-storage-account.md) mallin suorittamiseen itseäsi varten.   

Malli on vain yksinkertaisia Console-sovellus, joka seuraa, tapahtumat tulevat tapahtumaa-toiminnossa muuntaa ne `HttpRequestMessage` ja `HttpResponseMessage` objektit ja välittää niitä voin Runscope Ohjelmointirajapinnan.

Seuraava animoitu kuva näet Ohjelmointirajapinnan Developer-portaalissa Console-sovellus näkyy viesti on vastaanotettu, käsittelemistä ja välittää ja sitten pyynnön ja vastauksen Runscope liikenne tarkastaminen näy tehdään pyyntö.

![Pyyntö välitetään eteenpäin Runscope esittely](./media/api-management-log-to-eventhub-sample/apim-eventhub-runscope.gif)

## <a name="summary"></a>Yhteenveto
Azure API hallintapalvelun tarjoaa ihanteellinen paikan, voit siepata HTTP-liikenne siirtyy oman API. Azure tapahtuman keskittimet on erittäin skaalattava ja edullinen ratkaisu sieppaus liikenne ja sen syöttämistä toissijainen käsittelyjärjestelmien kirjaaminen, valvonnan ja muita kehittyneitä analytics. Yhteyden muodostaminen 3 osapuolen liikenne järjestelmien, kuten Runscope on yksinkertainen muutaman dozen viivoina koodin seuranta.

## <a name="next-steps"></a>Seuraavat vaiheet
-   Lisätietoja Azure tapahtuman keskittimet
    -   [Azure tapahtuman keskittimet käytön aloittaminen](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
    -   [Vastaanottamaan viestejä EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost)
    -   [Tapahtuman keskittimet ohjelmointi opas](../event-hubs/event-hubs-programming-guide.md)
-   Lisätietoja API hallinta ja tapahtuman keskittimet integrointi
    -   [Kuinka kirjaudun tapahtumien Azure tapahtuman porttiin Azure API hallinta](api-management-howto-log-event-hubs.md)
    -   [Lokin kohteesta](https://msdn.microsoft.com/library/azure/mt592020.aspx)
    -   [lokitiedoston eventhub käytännön viitata](https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub)
    