<properties
   pageTitle="Luotettavan viestintä yleiskatsaus | Microsoft Azure"
   description="Yleistä luotettava palvelut-viestintä mallia, mukaan lukien avaaminen kuuntelijoita Services ja ratkaiseminen päätepisteet viestintä-palveluiden välillä."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="BharatNarasimman"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="how-to-use-the-reliable-services-communication-apis"></a>Luotettavia palveluja viestintä ohjelmointirajapinnan käyttäminen

Azure palvelun kangasta kuin ympäristö on kokonaan agnostic tietoja välisen palvelut. Kaikki protokollat ja pinoa on hyväksyttävä UDP HTTP. Kannattaa valita, miten palvelut kannattaa pitää yhteyttä palvelun kehittäjän. Luotettavia palveluja application framework sisältää valmiita viestintä pinoa sekä API, joiden avulla voit luoda mukautetun viestintä-osia. 

## <a name="set-up-service-communication"></a>Palvelun viestinnän määrittäminen

Luotettavan palvelut-Ohjelmointirajapinnan käyttää yksinkertaisen käyttöliittymän palvelun tietoliikenne. Avaa palvelun päätepisteen toteuta liittymän:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

Voit lisätä viestintä listener käyttöympäristösi sitten palauttamalla palvelun perustuvia class-menetelmän ohitus.

Tilattomien Services:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```

Tilallisten Services:

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

Kummassakin tapauksessa palauttaa kuuntelijoita kokoelma. Näin palvelun kuunnella useita päätepisteet, mahdollisesti käyttämällä erilaisia protokollia, useita kuuntelijoita. Esimerkiksi voi olla HTTP-kuuntelutoiminnon ja erillinen WebSocket kuuntelija. Kunkin listener saa nimi, ja tuloksena kokoelma *nimi: osoite* parit esitetään JSON objektina, kun asiakastietokone pyytää palvelun esiintymän tai osion kuunteleminen osoitteet.

Tilattomien palvelun ohituksen palauttaa ServiceInstanceListeners kokoelma. ServiceInstanceListener sisältää toiminnon, voit luoda ICommunicationListener ja antaa sille nimen. Tilallisten palveluiden ohituksen palauttaa ServiceReplicaListeners kokoelma. Tämä on hieman eri tavalla kuin tilattomien vastaavaan, koska ServiceReplicaListener voit avata ICommunicationListener toissijainen replikoiden-vaihtoehto. Paitsi voidaan käyttää useita viestintä kuuntelijoita palveluun, mutta voit myös määrittää, mitkä kuuntelijoita Hyväksy toissijaisen replikoiden pyyntöjä ja mitkä kuuntelevat vain ensisijainen replikoita.

Esimerkiksi voi olla ServiceRemotingListener, joka kestää RPC puhelut vain ensisijainen replikoita ja toinen mukautettu listener, joka vie luku pyytää toissijainen replikoiden http:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [AZURE.NOTE] Useita kuuntelijoita palvelussa luotaessa kunkin listener **on** annettava yksilöllinen nimi.

Lopuksi kuvaavat [palvelun luettelon](service-fabric-application-model.md) osassa päätepisteet-palvelua varten tarvittavat päätepisteet.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

Viestintä-listener käyttää päätepisteen sen varatut resurssit `CodePackageActivationContext` - `ServiceContext`. Kuuntelutoiminto sitten aloittaa listening pyyntöjen, kun se avataan.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```

> [AZURE.NOTE] Päätepisteen resurssit ovat yhteiset koko palvelupakettiin, ja ne kohdistetaan palvelun kangasta palvelupakettiin käynnistyessä. Useita palvelun replikoita ylläpidettävä saman ServiceHost voi jakaa samaa porttia. Tämä tarkoittaa viestintä kuuntelua olisi tue jakamista. Suositeltava tapa tekoa on communication kuuntelua käyttämään osion tunnuksen ja replikan/esiintymän tunnus, kun se luo kuunnella-osoite.

### <a name="service-address-registration"></a>Palvelun osoite rekisteröinti

Kutsua *Nimeäminen palvelun* järjestelmäpalvelu suoritetaan palvelun kangasta klustereiden. Nimeäminen-palvelu on rekisteröintipalvelu palveluista ja niiden osoitteet, jotka jokaisen esiintymän tai palvelua käyttävien Kuuntele. Kun `OpenAsync` maksutavan `ICommunicationListener` on valmis, palaa sen arvo saa rekisteröity nimeäminen-palvelussa. Tämä palautusarvo, joka saa julkaistu nimeäminen-palvelu on merkkijono, jonka arvo voi olla mikä tahansa ollenkaan. Tämä merkkijonoarvo on mitä asiakkaat näkevät, kun ne Kysy-palvelun osoite nimeäminen-palvelusta.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);
                        
    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
            
    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));
    
    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```

Palvelun kangasta on API, jonka avulla asiakkaat ja muut palvelut pyytää osoitteen palvelun nimen mukaan. Tämä on tärkeää, koska palvelun osoitetta ei ole staattinen. Palvelujen siirretään resurssien tasaamisen ja käytettävyyden tarkoituksiin klusterin. Tämä on järjestelmä, jonka avulla asiakkaat voivat selvittää palvelun listening-osoitetta.

> [AZURE.NOTE] Valmis hallintapaketteihin viestin kirjoittaminen, saat `ICommunicationListener`, katso [palvelun kangasta verkko-Ohjelmointirajapinnan-palvelun OWIN itse isännöinnin](service-fabric-reliable-services-communication-webapi.md)

## <a name="communicating-with-a-service"></a>Yhteyden pitäminen palvelu
Luotettavan palvelut-Ohjelmointirajapinnan sisältää seuraavat kirjastot, jos haluat kirjoittaa asiakkaat, jotka palvelujen yhteydessä.

### <a name="service-endpoint-resolution"></a>Palvelun päätepisteen tarkkuus
Tiedonsiirto palvelun ensimmäinen askel on ratkaista päätepisteosoite osioon tai haluat ottaa yhteyttä palvelun esiintymän. `ServicePartitionResolver` Apuohjelman luokka on basic alkuperäisten, joka auttaa asiakkaiden selvittää suorituksen palvelun päätepisteelle. Palvelun kangasta termejä määrittämistä palvelun päätepisteelle kutsutaan *palvelun päätepisteen tarkkuus*.

Muodostaa yhteyden klusterin-palveluja `ServicePartitionResolver` voidaan luoda oletusasetukset. Tämä on useimmissa tapauksissa suositellut käytön:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```

Muodostaa yhteyden eri klusterin services `ServicePartitionResolver` voidaan luoda joukon klusterin yhdyskäytävän päätepisteet. Huomaa, että yhdyskäytävän päätepisteet vain eri päätepisteet samassa klusterissa muodostamisesta. Esimerkki:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

Voit myös `ServicePartitionResolver` voidaan antaa funktion luomiseen `FabricClient` käyttämään sisäisesti: 
 
```csharp
public delegate FabricClient CreateFabricClientDelegate();
```

`FabricClient`on objekti, jota käytetään pitää yhteyttä palvelun kangasta klusterin eri hallintatoiminnot klusterin varten. Tästä on hyötyä, kun haluat määrittää tarkemmin, miten `ServicePartitionResolver` toimii yhteyttä klusterin kanssa. `FabricClient`suorittaa välimuistiin sisäisesti ja on yleensä kallista luomiseen, joten on tärkeää uudelleen `FabricClient` esiintymät mahdollisimman hyvin. 

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```

Ratkaise menetelmän käytetään hakemiseen palvelu tai palvelun osion osioitua palveluiden osoite.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```

Palvelun osoite ovat helposti käyttämällä `ServicePartitionResolver`, mutta enemmän työtä on tarpeen, jotta voidaan ratkaista osoitteen oikein. Asiakkaan on onko yhteydenotto epäonnistui lyhytkestoisia virheen vuoksi, ja voit yritettävä esiintyvien (esimerkiksi palvelun siirretty tai on tilapäisesti poissa käytöstä), tai pysyvä virhe (esimerkiksi palvelu on poistettu tai pyydetty resurssi ei enää ole). Palveluesiintymiä tai replikoita voi liikkua solmun solmun milloin tahansa useista syistä. Palvelun osoite avulla `ServicePartitionResolver` voi olla vanhentunut asiakas-koodi yrittää muodostaa ajan mukaan. Tässä tapauksessa uudelleen asiakas on selvittää uudelleen osoitetta. Tarjoaa edellisen `ResolvedServicePartition` ilmaisee, että tulkintatoiminnon tarvitsee ja yritä uudelleen sen sijaan, että vain Nouda välimuistissa osoite.

Yleensä asiakas-koodi on ei toimi kanssa `ServicePartitionResolver` suoraan. Se luodaan ja välitetään viestintä asiakkaan tehtaan luotettava Services API-kirjastossa. Tehtaan avulla tulkintatoiminnon sisäisesti voit luoda Asiakasobjektia, jonka avulla voidaan palvelujen yhteydessä.

### <a name="communication-clients-and-factories"></a>Viestintä-asiakkaat ja tehtaan

Viestintä factory-kirjaston toteuttaa tyypillinen vika käsittely uudelleen kaavan, joka helpottaa ratkaistu päätepisteiden yritetään yhteydet. Factory-kirjaston tarjoaa yritä samalla, kun annat virhe käsittelytoimintoja.

`ICommunicationClientFactory`määrittää perus liittymän viestintä asiakkaan factory, joka tuottaa ohjelmat, joita voit jutella palvelun kangasta-palvelun käyttöön. CommunicationClientFactory käyttöönoton määräytyy viestintä-pino käyttämä palvelun kangasta-palvelu, jos asiakas haluaa viestiä. Luotettavan palvelut-Ohjelmointirajapinnan on `CommunicationClientFactoryBase<TCommunicationClient>`. Tämä on kantaluku soveltaminen `ICommunicationClientFactory` käyttöliittymä ja suorittaa tehtäviä, jotka löytyvät viestintä pinoa. (Näihin tehtäviin kuuluu käyttämällä `ServicePartitionResolver` määrittämään Palvelupäätepisteen). Asiakkaiden Toteuta yleensä abstraktit CommunicationClientFactoryBase luokan, käsittelemään logiikka, joka on viestintä-pino.

Viestintä-asiakasohjelman vain vastaanottaa osoitteen ja käyttää sitä Yhdistä-palveluun. Asiakas voi käyttää jostakin protokolla, se haluaa.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```

Asiakkaan tehdas on käyttäjän ensisijaiset vastuualueet luominen viestintä-asiakkaille. Asiakkaille, jotka eivät säilyttää pysyvä yhteys, kuten HTTP-asiakas tehdas on vain luominen ja palauttaa asiakas. Muita protokollia, joka säilyttää pysyvä yhteys, jotkin binaarinen protokollat, kuten myös hyväksyvät factory määrittääksesi, onko yhteys tarvitsee luodaan uudelleen.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```

-Poikkeuksen käsittelytoiminnon on palvelun suorittamisesta määritettäessä toimet poikkeuksen yhteydessä. Sääntöön on jakaa **retriable** ja **muiden retriable**. 

 - **Muut retriable** poikkeukset yksinkertaisesti Hae uudelleen ilmenee takaisin soittajalle. 
 - **Retriable** poikkeukset ovat edelleen jakaa **lyhytkestoisia** ja **muu kuin lyhytaikainen**.
  - **Lyhytkestoisia** poikkeuksena ne, jotka voivat riittää, että yritettävä ilman ratkaiseminen uudelleen palvelimen päätepisteen osoitetta. Näitä ovat lyhytkestoisia verkko-ongelmista tai palvelun virhevastauksia, joita ei Määritä palvelimen päätepisteen osoitetta ei ole. 
  - **Muu kuin lyhytaikainen** poikkeukset ovat niitä, jotka edellyttävät palvelun päätepisteosoite uudelleen viesteissä. Näitä ovat poikkeukset, jotka ilmaisevat Palvelupäätepisteen ei voi siirtyä, ilmaisee, että palvelun siirtyvät eri solmu. 

`TryHandleException` Tekee päätöksen annetun poikkeuksen. Jos se **ei tiedä,** mitä päätösten tekemiseen poikkeuksen tietoja, sen tulee palauttaa **Epätosi**. Jos se **tietää,** mitä haluat tehdä päätös, se kannattaa määrittää tuloksen vastaavasti ja palauttaa **arvon TOSI**.
 
```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
### <a name="putting-it-all-together"></a>Kaikkien tietojen yhdistäminen
Ja `ICommunicationClient`, `ICommunicationClientFactory`, ja `IExceptionHandler` ympärille viestintäprotokolla `ServicePartitionClient` on rivittyy ollenkaan sekä annetaan virheen käsittely ja osion osoite tarkkuus ympyrä komponentit ympärille.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```

## <a name="next-steps"></a>Seuraavat vaiheet
 - Esimerkki HTTP services [otoksen projektin GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount)välinen yhteys.

 - [Etäproseduurikutsu puheluiden luotettavia palveluja Etäpalvelujen](service-fabric-reliable-services-communication-remoting.md)

 - [Verkko-Ohjelmointirajapinnan, joka käyttää OWIN luotettava Services-palveluissa](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-viestintä luotettavaa-palvelujen avulla](service-fabric-reliable-services-communication-wcf.md)
