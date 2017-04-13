<properties
   pageTitle="ASP.NET-verkko-Ohjelmointirajapinnan tietoliikenteen palvelun | Microsoft Azure"
   description="Tietoa ottamisesta käyttöön palvelun tietoliikenne vaiheittaiset OWIN isännöinti itse luotettava palvelut-Ohjelmointirajapinnan ASP.NET-verkko-Ohjelmointirajapinnan avulla."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="get-started-service-fabric-web-api-services-with-owin-self-hosting"></a>Aloittaminen: palvelun kangasta verkko-Ohjelmointirajapinnan-palvelun OWIN itse isännöinnin

Azure palvelun kangasta siirtää käsiksi potenssiin, kun mietit, kuinka haluat palvelujen käyttäjien kanssa ja keskenään. Tässä opetusohjelmassa keskitytään käyttöönoton palvelun tietoliikenne ASP.NET-verkko-Ohjelmointirajapinnan käyttäminen Avaa Web-liittymän palvelun kangasta luotettava Services API isännöimiseen itse .NET (OWIN). Olemme delve monitasoisissa luotettavia palveluja vaihdettava viestintä API kyselyjä. Myös Käytämme verkko-Ohjelmointirajapinnan vaiheittaiset esimerkissä kerrotaan, kuinka voit määrittää mukautetun viestintä kuuntelija.


## <a name="introduction-to-web-api-in-service-fabric"></a>Verkko-Ohjelmointirajapinnan-palvelun kangasta esittely

ASP.NET-verkko-Ohjelmointirajapinnan on Suositut ja tehokas kehys etsimisen HTTP-ohjelmointirajapinnan .NET Framework päälle. Jos et ole vielä tutustunut puitteissa, katso lisätietoja [ASP.NET-verkko-Ohjelmointirajapinnan 2 käytön aloittaminen](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) .

Verkko-Ohjelmointirajapinnan-palvelun kangasta on sama ASP.NET Web Ohjelmointirajapinnan tiedät ja perinteisen Wordin kanssa. Ero on siitä, miten voit *isännän* verkko-Ohjelmointirajapinnan-sovellus. Ei käytössä Microsoft Internet Information Services (IIS). Ymmärtämään ero, nyt jakaa sen osiin kaksi osaa:

 1. Verkko-Ohjelmointirajapinnan-sovellus (mukaan lukien ohjaimet ja mallien)
 2. Host (verkkopalvelin, yleensä IIS)

Verkko-Ohjelmointirajapinnan-sovelluksen itse ei muutu. On sama asia verkko-Ohjelmointirajapinnan sovelluksista, sinun on kirjoitettu aiemman ja pitäisi onnistua siirtämällä riittää, että suurin osa sovelluksen-koodin. Mutta jos olet jo isännöivän IIS-palvelimessa, jossa host sovelluksen voi olla hieman erilainen kuin mitä olet tottunut. Ennen kuin on siirtyä isännöintipalvelu osa aloitetaan jotain hiukan näyttävämpää tuttuja: Verkko-Ohjelmointirajapinnan-sovellus.


## <a name="create-the-application"></a>Sovelluksen luominen

Aloita luomalla uuden palvelun kangasta sovelluksen Visual Studio 2015 yksittäisen tilattomien palvelu:

![Luo uusi palvelu kangasta-sovellus](media/service-fabric-reliable-services-communication-webapi/webapi-newproject.png)

Verkko-Ohjelmointirajapinnan käyttäminen tilattomien palvelun Visual Studio malli on käytettävissä. Tässä opetusohjelmassa on luominen alusta alkaen, joka johtaa mitä saat, jos olet valinnut tämän mallin verkko-Ohjelmointirajapinnan projektin.

Valitse tyhjän tilattomien palvelun projektin ja Opi luomaan verkko-Ohjelmointirajapinnan projektin alusta alkaen tai voit tilattomien palvelun verkko-Ohjelmointirajapinnan mallia ja toimimalla.  

![Luo yksittäisen tilattomien palvelu](media/service-fabric-reliable-services-communication-webapi/webapi-newproject2.png)

Ensimmäiseksi on hakemaan NuGet Jotkin paketit Web API. Haluat käyttää paketti on Microsoft.AspNet.WebApi.OwinSelfHost. Tämä paketti sisältää kaikki tarvittavat verkko-Ohjelmointirajapinnan-paketit ja *Host (isäntä)* -paketit. Tämä on tärkeää myöhemmin.

![Luo verkko-Ohjelmointirajapinnan NuGet Package hallinnan avulla](media/service-fabric-reliable-services-communication-webapi/webapi-nuget.png)

Kun paketit on asennettu, voit aloittaa rakennuksen verkko-Ohjelmointirajapinnan projektin perusrakenteen ulos. Jos olet käyttänyt verkko-Ohjelmointirajapinnan, projektirakenne pitäisi näyttää varmasti tutulta. Aloita lisäämällä `Controllers` hakemistosta ja valvonta on yksinkertainen arvot:

**ValuesController.cs**

```csharp
using System.Collections.Generic;
using System.Web.Http;
    
namespace WebService.Controllers
{
    public class ValuesController : ApiController
    {
        // GET api/values 
        public IEnumerable<string> Get()
        {
            return new string[] { "value1", "value2" };
        }

        // GET api/values/5 
        public string Get(int id)
        {
            return "value";
        }

        // POST api/values 
        public void Post([FromBody]string value)
        {
        }

        // PUT api/values/5 
        public void Put(int id, [FromBody]string value)
        {
        }

        // DELETE api/values/5 
        public void Delete(int id)
        {
        }
    }
}

```

Lisää seuraavaksi käynnistys-luokka on projektin pääkansio rekisteröidä reititys, formatters ja määritys-asetukset. Tämä on myös missä *isännän*, jossa myöhemmin uudelleen uusia liitetään verkko-Ohjelmointirajapinnan. 

**Startup.cs**

```csharp
using System.Web.Http;
using Owin;

namespace WebService
{
    public static class Startup
    {
        public static void ConfigureApp(IAppBuilder appBuilder)
        {
            // Configure Web API for self-host. 
            HttpConfiguration config = new HttpConfiguration();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            appBuilder.UseWebApi(config);
        }
    }
}
```

Tämä on sen sovelluksen osan. Tässä vaiheessa on olet juuri basic Web API projektin asettelun määrittämisestä. Tähän mennessä ei kannata näyttää paljon eri verkko-Ohjelmointirajapinnan projektien ehkä olet kirjoittanut aiemman tai verkko-Ohjelmointirajapinnan perusmallin. Organisaatiosi liiketoimintalogiikan oleviin ohjaimet ja mallien tavalliseen tapaan.

Nyt on tarkoitus isännöinti niin, että on todella toimii puhelimessasi

## <a name="service-hosting"></a>Palvelun isännöinnin

Palvelun kangasta palvelun suorittaa *palvelun host-prosessi*, suoritettava tiedosto, joka suoritetaan palvelun-koodin. Kirjoitettaessa palvelu käyttämällä luotettava palvelut-Ohjelmointirajapinnan palvelun projektin kääntää vain Rekisteröi palvelutyyppi ja suorittaa koodin suoritettava tiedosto. Tämä on tosi useimmissa tapauksissa, kun kirjoitat palvelun kangasta .NET-palvelu. Kun avaat Program.cs tilattomien palvelun Projectissa, näkyy:

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using Microsoft.ServiceFabric.Services.Runtime;

internal static class Program
{
    private static void Main()
    {
        try
        {
            ServiceRuntime.RegisterServiceAsync("WebServiceType",
                context => new WebService(context)).GetAwaiter().GetResult();

            ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(WebService).Name);

            // Prevents this host process from terminating so services keeps running. 
            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Jos, joka näyttää epätavallisen aloituskohdan console-sovellukseen, joka on koska se on.

Lisätietoja palvelun host-prosessi ja palvelun rekisteröinti on tämän artikkelin piiriin. Mutta on tärkeää tietää, että *palvelukoodi on käynnissä omassa prosessissa*nyt.

## <a name="self-host-web-api-with-an-owin-host"></a>Isännöidä itse OWIN isännän verkko-Ohjelmointirajapinnan

Koska, että verkko-Ohjelmointirajapinnan sovelluksen koodin nykyisessä omassa prosessissa, kuinka voit liitä se verkkopalvelimen ylöspäin? Kirjoita [OWIN](http://owin.org/). OWIN on yksinkertaisesti sopimus .NET-web-sovellusten ja web-palvelinten välillä. Perinteisesti käytettäessä (enintään MVC 5) ASP.NET web-sovelluksen on tiiviisti kytketty IIS System.Web kautta. Verkko-Ohjelmointirajapinnan kuitenkin toteuttaa OWIN, jotta voit kirjoittaa web-sovelluksen, joka on irrotettu WWW-palvelimesta, jossa se sijaitsee. Tästä syystä voit *isännöidä* OWIN verkkopalvelimeen, voit aloittaa oman aikana. Tämä sopii täydellisesti vain kuvattu palvelun kangasta isännöintipalvelu mallia.

Tässä artikkelissa käytetään Katana kuin OWIN isäntä verkko-Ohjelmointirajapinnan-sovellus. Katana on rakennettu [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx) ja Windows [HTTP palvelimen API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)Avaa lähde OWIN host toteutus.

> [AZURE.NOTE] Lisätietoja Katana, siirry [Katana sivuston](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana). Katso käyttämisestä Katana isännöimiseen itse verkko-Ohjelmointirajapinnan pikaisesti, [Käytä OWIN Self-Host ASP.NET Web API 2](http://www.asp.net/web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api).


## <a name="set-up-the-web-server"></a>Web-palvelimen määrittäminen

Luotettavan palvelut-Ohjelmointirajapinnan on communication aloituskohtaa, johon voidaan liittää viestintä pinoa, jotka sallivat käyttäjien ja asiakkaiden muodostaa yhteyden palveluun:

```csharp

protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    ...
}

```

Verkkopalvelin (ja kaikki muut viestintä pinon käytät myöhemmin, kuten WebSockets) käytettävä ICommunicationListener-liittymän voi integroida järjestelmän oikein. Syyt tähän tulee Lisää näennäinen alla kuvatulla tavalla.

Luo luokan nimi, joka sisältää ICommunicationListener OwinCommunicationListener:

**OwinCommunicationListener.cs**

```csharp
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;
using System;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        public void Abort()
        {
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
        }
    }
}
```

ICommunicationListener-liittymän sisältää kolme tapaa viestintä kuuntelija palvelun hallinta:

 - *OpenAsync*. Aloita Kuuntele pyynnöt.
 - *CloseAsync*. Lopeta pyyntöjen kuunteleminen, Lopeta tapahtumakartoitus pyynnöt ja Sammuta tilanteen.
 - *Keskeytä*. Peruuta kaikki ja Lopeta heti.

Aluksi lisätä yksityinen luokan jäseniä kuuntelutoiminto on toimivan tavalla. Nämä alustaa kautta konstruktoria ja käyttää myöhemmin, kun määrität kuunteleminen URL-osoite.

```csharp
internal class OwinCommunicationListener : ICommunicationListener
{
    private readonly ServiceEventSource eventSource;
    private readonly Action<IAppBuilder> startup;
    private readonly ServiceContext serviceContext;
    private readonly string endpointName;
    private readonly string appRoot;

    private IDisposable webApp;
    private string publishAddress;
    private string listeningAddress;

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
        : this(startup, serviceContext, eventSource, endpointName, null)
    {
    }

    public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
    {
        if (startup == null)
        {
            throw new ArgumentNullException(nameof(startup));
        }

        if (serviceContext == null)
        {
            throw new ArgumentNullException(nameof(serviceContext));
        }

        if (endpointName == null)
        {
            throw new ArgumentNullException(nameof(endpointName));
        }

        if (eventSource == null)
        {
            throw new ArgumentNullException(nameof(eventSource));
        }

        this.startup = startup;
        this.serviceContext = serviceContext;
        this.endpointName = endpointName;
        this.eventSource = eventSource;
        this.appRoot = appRoot;
    }
   

    ...

```

## <a name="implement-openasync"></a>Ota käyttöön OpenAsync

Voit määrittää WWW-palvelin, sinun on kaksi kappaletta tietojen:

 - *A URL-polku etuliite*. Tämä on valinnainen, mutta se on hyvä voit määrittää tämän nyt voit ylläpitää useita verkkopalvelut turvallisesti-sovelluksessa.
 - *Portti*.

Ennen kuin saat WWW-palvelimen portti, on tärkeää ymmärtää, että palvelun kangasta on sovelluskerroksen, joka toimii puskurin sovelluksesi ja pohjana käyttöjärjestelmille, joita se suoritetaan. Näin ollen palvelun kangasta avulla on helppo *päätepisteet* palvelujen määrittäminen. Palvelun kangasta varmistaa, että päätepisteet ovat käytettävissä palvelun käyttämään. Tällöin sinun ei tarvitse määrittää niitä itse pohjana OS-ympäristössä. Voit ylläpitää palvelun kangasta sovelluksen eri ympäristöissä helposti eikä sinun tarvitse tehdä muutoksia. (Esimerkiksi voit ylläpitää samassa sovelluksessa Azure tai oman palvelinkeskukseen.)

Määrittää HTTP-päätepisteen PackageRoot\ServiceManifest.xml:

```xml

<Resources>
    <Endpoints>
        <Endpoint Name="ServiceEndpoint" Type="Input" Protocol="http" Port="8281" />
    </Endpoints>
</Resources>

```

Tämä vaihe on tärkeää, koska palvelun host-prosessi suoritetaan rajoitettu tunnistetiedot (verkkopalvelu Windows). Tämä tarkoittaa, että palvelun voi käyttää määrittäminen HTTP-päätepisteen oma. Käyttämällä päätepisteen määritystä palvelun kangasta tietää määrittämään oikean käyttöoikeusluettelon (Käyttöoikeusluettelon), palvelun kuuntelevat URL-osoitteen. Palvelun kangasta sekä määrittämään päätepisteet vakio sijainti.


Takaisin OwinCommunicationListener.cs, voit aloittaa käyttöönoton OpenAsync. Tämä on, jossa käynnistät verkkopalvelin. Ensin päätepisteen tiedot ja luoda palvelun kuuntelevat URL-osoite. URL-osoite on erilaiset riippuen siitä, onko kuuntelua käytetään tilattomien palvelu tai tilallisten palvelu. Tilallisten palvelun kuuntelutoiminto on luotava se seuraa tilallisten palvelun jokaisen replikan yksilöllinen osoite. Tilattomien palvelut-osoitteen voi olla helpompaa. 

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
    var protocol = serviceEndpoint.Protocol;
    int port = serviceEndpoint.Port;

    if (this.serviceContext is StatefulServiceContext)
    {
        StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}{3}/{4}/{5}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/',
            statefulServiceContext.PartitionId,
            statefulServiceContext.ReplicaId,
            Guid.NewGuid());
    }
    else if (this.serviceContext is StatelessServiceContext)
    {
        this.listeningAddress = string.Format(
            CultureInfo.InvariantCulture,
            "{0}://+:{1}/{2}",
            protocol,
            port,
            string.IsNullOrWhiteSpace(this.appRoot)
                ? string.Empty
                : this.appRoot.TrimEnd('/') + '/');
    }
    else
    {
        throw new InvalidOperationException();
    }
    
    ...

```

Huomaa, että "http://+" käytetään tässä. Näin voit varmistaa, että kaikki käytettävissä olevat osoitteet, esimerkiksi localhost FQDN ja tietokoneen IP Kuuntele verkkopalvelin.

OpenAsync on yksi tärkeimmistä syitä, miksi verkkopalvelin (tai minkä tahansa viestintä-pino) on toteutettu kuin ICommunicationListener sen sijaan, että se on vain avata suoraan `RunAsync()` -palvelussa. Palautusarvo OpenAsync on käytettävä Kuuntele verkkopalvelin osoite. Kun tämä osoite palautetaan järjestelmään, se rekisteröi osoite-palvelussa. Palvelun kangasta on Ohjelmointirajapinta, jonka avulla asiakkaat ja muut palvelut pyytää osoitteen palvelun nimen mukaan. Tämä on tärkeää, koska palvelun osoitetta ei ole staattinen. Palvelujen siirretään resurssien tasaamisen ja käytettävyyden tarkoituksiin klusterin. Tämä on järjestelmä, jonka avulla asiakkaat voivat selvittää palvelun listening-osoitetta.

Liittyvän mielessä OpenAsync käynnistää verkkopalvelin ja palauttaa käytettävä Kuuntele on osoite. Huomautus seuraa "http://+", mutta ennen OpenAsync palauttaa osoite, "+" korvataan IP- tai se ei tällä hetkellä solmun täydellinen toimialuenimi. Osoite, joka palautetaan tällä menetelmällä on järjestelmän rekisteröity. Se näkyy myös asiakkaat ja muut palvelut ne pyytävät palvelun osoitetta. Asiakkaat voivat muodostaa yhteyden oikein ne on todellinen IP tai FQDN-osoitteen.

```csharp
    ...

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    try
    {
        this.eventSource.Message("Starting web server on " + this.listeningAddress);

        this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

        this.eventSource.Message("Listening on " + this.publishAddress);

        return Task.FromResult(this.publishAddress);
    }
    catch (Exception ex)
    {
        this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

        this.StopWebServer();

        throw;
    }
}

```

Huomaa, että tämä viittaa käynnistys-luokka, joka on välitetty konstruktorissa OwinCommunicationListener. Verkkopalvelin käyttää käynnistys tämä esiintymä käynnistää verkko-Ohjelmointirajapinnan-sovellus.

`ServiceEventSource.Current.Message()` Rivi tulee näkyviin diagnostiikan tapahtumat-ikkunassa myöhemmin, kun käynnistät sovelluksen Vahvista verkkopalvelin on käynnistetty.

## <a name="implement-closeasync-and-abort"></a>Ota käyttöön CloseAsync ja keskeytys

Lopuksi toteuttaa sekä CloseAsync ja Keskeytä lopettavat verkkopalvelin. Verkkopalvelin voi keskeyttää luovutaan palvelimen kahva, joka on luotu OpenAsync aikana.

```csharp
public Task CloseAsync(CancellationToken cancellationToken)
{
    this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);
            
    this.StopWebServer();

    return Task.FromResult(true);
}

public void Abort()
{
    this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);
    
    this.StopWebServer();
}

private void StopWebServer()
{
    if (this.webApp != null)
    {
        try
        {
            this.webApp.Dispose();
        }
        catch (ObjectDisposedException)
        {
            // no-op
        }
    }
}
```

Toteutuksen jälkeisen tässä esimerkissä CloseAsync ja Keskeytä vain lopettaa verkkopalvelin. Voivat halutessaan suorittaa Lisää tilanteen koordinoidun Sammuta WWW-palvelimen CloseAsync. Sammuta voi esimerkiksi odottaa tapahtumakartoitus pyynnöt on tarkoitus valmistua palauttamista ennen.

## <a name="start-the-web-server"></a>Käynnistä verkkopalvelin

Olet nyt valmis luomaan ja palauttaa OwinCommunicationListener käynnistä verkkopalvelin esiintymä. Takaisin ohittaa Service-luokka (WebService.cs) `CreateServiceInstanceListeners()` menetelmää:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                           .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                           .Select(endpoint => endpoint.Name);

    return endpoints.Select(endpoint => new ServiceInstanceListener(
        serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
}
```

Tämä on joka verkko-Ohjelmointirajapinnan- *sovellus* ja OWIN *host* lopuksi leikkauspiste. Host (OwinCommunicationListener) annetaan esiintymä *sovellus* (verkko-Ohjelmointirajapinnan) käynnistys-luokka. Palvelun kangasta sitten hallitaan sen elinkaaren aikana. Tämä samoissa voi seurata yleensä viestintä minkä tahansa pinon kanssa.

## <a name="put-it-all-together"></a>Sijoita se ollenkaan

Tässä esimerkissä sinun ei tarvitse tehdä mitään `RunAsync()` -menetelmä niin, että ohitus yksinkertaisesti voidaan poistaa.

Lopullinen palvelun käyttöönoton pitäisi olla erittäin yksinkertaisia. Se on luotava viestintä kuuntelua vain:

```csharp
using System;
using System.Collections.Generic;
using System.Fabric;
using System.Fabric.Description;
using System.Linq;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace WebService
{
    internal sealed class WebService : StatelessService
    {
        public WebService(StatelessServiceContext context)
            : base(context)
        { }

        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
            var endpoints = Context.CodePackageActivationContext.GetEndpoints()
                                   .Where(endpoint => endpoint.Protocol == EndpointProtocol.Http || endpoint.Protocol == EndpointProtocol.Https)
                                   .Select(endpoint => endpoint.Name);

            return endpoints.Select(endpoint => new ServiceInstanceListener(
                serviceContext => new OwinCommunicationListener(Startup.ConfigureApp, serviceContext, ServiceEventSource.Current, endpoint), endpoint));
        }
    }
}
```

Koko `OwinCommunicationListener` luokan:

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Globalization;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Owin.Hosting;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Owin;

namespace WebService
{
    internal class OwinCommunicationListener : ICommunicationListener
    {
        private readonly ServiceEventSource eventSource;
        private readonly Action<IAppBuilder> startup;
        private readonly ServiceContext serviceContext;
        private readonly string endpointName;
        private readonly string appRoot;

        private IDisposable webApp;
        private string publishAddress;
        private string listeningAddress;

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName)
            : this(startup, serviceContext, eventSource, endpointName, null)
        {
        }

        public OwinCommunicationListener(Action<IAppBuilder> startup, ServiceContext serviceContext, ServiceEventSource eventSource, string endpointName, string appRoot)
        {
            if (startup == null)
            {
                throw new ArgumentNullException(nameof(startup));
            }

            if (serviceContext == null)
            {
                throw new ArgumentNullException(nameof(serviceContext));
            }

            if (endpointName == null)
            {
                throw new ArgumentNullException(nameof(endpointName));
            }

            if (eventSource == null)
            {
                throw new ArgumentNullException(nameof(eventSource));
            }

            this.startup = startup;
            this.serviceContext = serviceContext;
            this.endpointName = endpointName;
            this.eventSource = eventSource;
            this.appRoot = appRoot;
        }

        public Task<string> OpenAsync(CancellationToken cancellationToken)
        {
            var serviceEndpoint = this.serviceContext.CodePackageActivationContext.GetEndpoint(this.endpointName);
            var protocol = serviceEndpoint.Protocol;
            int port = serviceEndpoint.Port;

            if (this.serviceContext is StatefulServiceContext)
            {
                StatefulServiceContext statefulServiceContext = this.serviceContext as StatefulServiceContext;

                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}{3}/{4}/{5}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/',
                    statefulServiceContext.PartitionId,
                    statefulServiceContext.ReplicaId,
                    Guid.NewGuid());
            }
            else if (this.serviceContext is StatelessServiceContext)
            {
                this.listeningAddress = string.Format(
                    CultureInfo.InvariantCulture,
                    "{0}://+:{1}/{2}",
                    protocol,
                    port,
                    string.IsNullOrWhiteSpace(this.appRoot)
                        ? string.Empty
                        : this.appRoot.TrimEnd('/') + '/');
            }
            else
            {
                throw new InvalidOperationException();
            }

            this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

            try
            {
                this.eventSource.Message("Starting web server on " + this.listeningAddress);

                this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

                this.eventSource.Message("Listening on " + this.publishAddress);

                return Task.FromResult(this.publishAddress);
            }
            catch (Exception ex)
            {
                this.eventSource.Message("Web server failed to open endpoint {0}. {1}", this.endpointName, ex.ToString());

                this.StopWebServer();

                throw;
            }
        }

        public Task CloseAsync(CancellationToken cancellationToken)
        {
            this.eventSource.Message("Closing web server on endpoint {0}", this.endpointName);

            this.StopWebServer();

            return Task.FromResult(true);
        }

        public void Abort()
        {
            this.eventSource.Message("Aborting web server on endpoint {0}", this.endpointName);

            this.StopWebServer();
        }

        private void StopWebServer()
        {
            if (this.webApp != null)
            {
                try
                {
                    this.webApp.Dispose();
                }
                catch (ObjectDisposedException)
                {
                    // no-op
                }
            }
        }
    }
}
```

Nyt kun olet lisännyt kaikki osat paikassa, projektin pitäisi näyttää samalta kuin tavallinen verkko-Ohjelmointirajapinnan-sovelluksen luotettava Services API pikakuvakkeet ja OWIN host:


![Verkko-Ohjelmointirajapinnan luotettava Services API pikakuvakkeet ja OWIN Host (isäntä)](media/service-fabric-reliable-services-communication-webapi/webapi-projectstructure.png)

## <a name="run-and-connect-through-a-web-browser"></a>Suorita ja Yhdistä selaimen kautta

Jos et ole tehnyt niin [kehittäminen ympäristön määritys](service-fabric-get-started.md).


Voit nyt muodosta ja ota käyttöön palvelun. Painamalla **F5** Visual Studiossa voivat laatia ja kehittää sovellusta. Diagnostiikan tapahtumat-ikkunassa pitäisi näkyä viesti, joka ilmaisee, että verkkopalvelin avattu http://localhost:8281 /.


![Visual Studio diagnostiikan tapahtumat-ikkuna](media/service-fabric-reliable-services-communication-webapi/webapi-diagnostics.png)

> [AZURE.NOTE] Jos portti on jo avannut toisen prosessin käytössä käyttämääsi laitteeseen, näkyviin voi tulla on virhe. Tämä ilmaisee, että kuuntelutoiminto ei voi avata. Jos näin on, kokeile eri portin ServiceManifest.xml päätepisteen-määrityksiä varten.


Kun palvelua käytetään, Avaa selain ja siirry [http://localhost:8281/api/arvot](http://localhost:8281/api/values) testaa se.

## <a name="scale-it-out"></a>Skaalata sen

Laajentaminen tilattomien verkkosovelluksissa yleensä tarkoittaa lisääminen Lisää koneet ja niiden web Apps-sovellusten määrittäminen. Palvelun kangasta tiedonsiirron ohjelma voit tehdä tämän puolestasi, aina, kun uusi solmut on lisätty klusteriin. Kun luot tilattomien palvelun esiintymät, voit määrittää luotavan esiintymien määrän. Palvelun kangasta sijoittaa klusterin solmut esiintymien määrän. Ja varmistaa ei, jos haluat luoda minkä tahansa yhden solmun useamman kuin yhden esiintymän. Voit myös opasta palvelun kangasta Luo aina erillisen jokaisen solmun esiintymä-Laske **-1** -määrittämällä. Tämä takaa, että aina, kun lisäät solmujen skaalata yhteyttä klusterin ulos, tilattomien palvelun esiintymän luodaan uusi solmuissa. Tämä arvo on palvelun esiintymän ominaisuus, joten se määritetään, kun luot palvelun esiintymän. Voit tehdä tämän PowerShellin kautta:

```powershell

New-ServiceFabricService -ApplicationName "fabric:/WebServiceApplication" -ServiceName "fabric:/WebServiceApplication/WebService" -ServiceTypeName "WebServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1

```

Voit tehdä tämän myös, määrittäessäsi oletusarvo-palvelun Visual Studio tilattomien palvelun projektin:

```xml

<DefaultServices>
  <Service Name="WebService">
    <StatelessService ServiceTypeName="WebServiceType" InstanceCount="-1">
      <SingletonPartition />
    </StatelessService>
  </Service>
</DefaultServices>

```

Katso lisätietoja siitä, miten voit luoda sovelluksen ja palvelun esiintymät, [sovelluksen käyttöönotto](service-fabric-deploy-remove-applications.md).

## <a name="next-steps"></a>Seuraavat vaiheet

[Palvelun kangasta sovelluksen virheenkorjaus Visual Studion avulla](service-fabric-debugging-your-application.md)
