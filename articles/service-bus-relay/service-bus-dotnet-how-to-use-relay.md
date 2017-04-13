<properties
    pageTitle="Voit käyttää palvelun Bus välitys .NET | Microsoft Azure"
    description="Opettele käyttämään Azure palvelun Bus välityspalvelu muodostaa kahden eri sijainneissa ylläpidettävä sovellukset."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="sethm"/>


# <a name="how-to-use-the-azure-service-bus-relay-service"></a>Azure-palvelu Bus välityspalvelu käyttäminen

Tässä artikkelissa kerrotaan, miten palvelun Bus välitys-palvelun käyttöä varten. Mallit on kirjoitettu C# ja Windows Communication Foundation (WCF) Ohjelmointirajapinnan käyttäminen palvelun Bus kokoonpanon sisältämien tunnisteet. Saat lisätietoja palvelun Bus välitys [palvelun Bus välittäminen messaging](service-bus-relay-overview.md) yleiskatsaus.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-the-service-bus-relay"></a>Mikä on palvelun Bus välitys?

[Palvelun Bus *välitys* ](service-bus-relay-overview.md) -palvelun avulla voit luoda hybrid-sovelluksia, jotka suoritetaan Azure palvelinkeskuksen ja paikallisen oman yritysympäristössä. Palvelun Bus välitys helpottaa näin voit näyttää turvallisesti Windows Communication Foundation (WCF)-palvelut, jotka sijaitsevat ilman tarvitsee avata yhteys palomuuri tai yrityksen verkkoinfrastruktuuria tunkeutuva muutoksia edellyttävän yrityksen yritysverkkoa julkisen pilveen.

![Välitys käsitteitä](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Palvelun Bus välitys mahdollistaa WCF palveluita aiemmin enterprise-ympäristössä. Voit sitten delegoida Kuuntele saapuvat istunnot ja pyynnöt WCF palveluista käynnissä omassa Azure palvelun Bus-palveluun. Voit näyttää Azure sovelluksen koodin tai matkapuhelimen työntekijöiden tai ekstranet kumppanin ympäristöissä palveluista. Palvelun Bus mahdollistaa suojatusti Yhteisöpalvelut tarkasti rajattuja tasolla käyttöoikeuksien hallinta. Sen avulla voit hyödyntää sitä pilvestä ja näyttää toiminnoista ja aiemmin yritysratkaisuja tietojen tehokas ja turvallisesti.

Tässä artikkelissa kerrotaan, miten käyttää palvelun Bus välitys WCF-verkkopalveluun, tarjoamia käyttämällä TCP kanavan sidonta, joka toteuttaa Suojatun keskustelun kahden osapuolen välinen luomiseen.

## <a name="create-a-service-namespace"></a>Luo palvelun nimitila

Kun haluat käyttää palvelun Bus välitys Azure, sinun on luotava nimitila. Nimitilan tarjoaa säilöön osoitteiden palvelun Bus resurssien sovelluksessa.

Voit luoda palvelun nimitila seuraavasti:

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-the-service-bus-nuget-package"></a>Hae palvelun Bus NuGet-paketti

[Palvelun Bus NuGet paketti](https://www.nuget.org/packages/WindowsAzure.ServiceBus) on helpoin tapa saat palvelun Bus Ohjelmointirajapinnan ja määrittää kaikki palvelun Bus riippuvuudet sovelluksen. Asenna NuGet paketti projektin, toimi seuraavasti:

1.  Napsauta ratkaisunhallinnassa **viittaukset**hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**.
2.  Etsi "Palvelun Bus" ja **Microsoft Azure palvelun Bus** kohteen valitseminen. Viimeistele asennus valitsemalla **Asenna** ja valitse Sulje seuraavassa valintaikkunassa:

    ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="use-service-bus-to-expose-and-consume-a-soap-web-service-with-tcp"></a>Palvelun Bus avulla voit näyttää ja tarjoaman TCP kanssa SOAP web-palvelu

Voit näyttää aiemmin luodun WCF SOAP--verkkopalveluun ulkoisen kulutus, sinun on tehtävä muutokset palvelun sidontojen ja osoitteet. Tämä saattaa edellyttää määritys-tiedostoon tehdyt muutokset, tai se voi vaatia koodin muuttuu sen mukaan, miten olet määrittäminen ja määrittänyt WCF-palveluiden. Huomaa, että WCF sallii on useita verkko-päätepisteet saman palvelun kautta, jotta voit säilyttää aiemmin sisäinen päätepisteet samalla, kun ulkoisen käytön palvelun Bus päätepisteet lisääminen samaan aikaan.

Tässä tehtävässä luoda yksinkertaisen WCF-palvelu ja palvelun Bus kuuntelija lisääminen. Tämä Harjoitus olettaa tietyillä Visual Studio ja näin ollen ei käy läpi kaikki luominen projektin tietoja. Sen sijaan se keskitytään koodi.

Ennen aloittamista seuraavia ohjeita noudattamalla seuraavia ohjeita ympäristön määrittäminen:

1.  Visual Studion Luo console-sovellus, joka sisältää kaksi projektia, "Asiakas" ja "Service"-ratkaisuun.
2.  Lisää Microsoft Azure palvelun Bus NuGet paketti molemmat projektit. Tämän paketin Lisää kaikki tarvittavat kokoonpanon viittaukset projektit.

### <a name="how-to-create-the-service"></a>Palvelun luominen

Luo itse palvelu. Minkä tahansa WCF-palvelu sisältää vähintään kolme eri osat:

-   Sopimuksen, jossa kuvataan, mitä viestejä vaihdetaan ja mitä toimintoja voidaan kutsua määritys.
-   Mainitun sopimuksen soveltaminen.
-   Host (isäntä), joka isännöi WCF-palvelua ja paljastaa useita päätepisteet.

Tässä osassa koodiesimerkkejä osoitteen nämä osat.

Sopimuksen määrittää yhdellä kertaa `AddNumbers`, joka laskee yhteen kaksi lukua ja palauttaa tuloksen. `IProblemSolverChannel` -Liittymän avulla asiakkaan helpommin välityspalvelimen elinkaaren hallinta. Luominen liittymää parhaana käytäntönä pidetään. Se on hyvä Sijoita tämä sopimus määritelmä erilliseen tiedostoon, jotta voit viitata tiedoston "Asiakas" ja "Palvelu" projekteista, mutta voit myös kopioida koodin molemmat projektit.

```
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Paikallaan sopimuksen käyttöönoton on trivial.

```
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Määritä service-isäntä ohjelmallisesti

Sopimuksen ja käyttöönoton paikallaan voit isännöidä palvelu. Isännöinti ilmenee sisällä [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/azure/system.servicemodel.servicehost.aspx) -objekti, joka kestää varoen hallintaa palvelun esiintymät ja isännöi, jotka kuuntelevat viestien päätepisteet. Seuraava koodi määrittää palvelun Normaali paikallisen päätepisteen ja palvelun Bus päätepisteen, esitä ulkoasu-rinnakkain ja sisäisten ja ulkoisten päätepisteistä. Korvaa merkkijonon *nimitilan* nimitilan nimi ja *yourKey* Suojaussidosten avainta, jota olet hankkinut asennuksen edellisessä vaiheessa.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER to close");
Console.ReadLine();

sh.Close();
```

Esimerkissä voit luoda päätepisteet, jotka ovat samassa sopimuksen käyttöönoton. Yksi on paikallinen ja yksi on suunniteltu palvelun Bus kautta. Tärkeimmät erot ne ovat sidontojen; [NetTcpBinding](https://msdn.microsoft.com/library/azure/system.servicemodel.nettcpbinding.aspx) paikallisen yksi ja [NetTcpRelayBinding](https://msdn.microsoft.com/library/azure/microsoft.servicebus.nettcprelaybinding.aspx) palvelun Bus päätepisteen ja osoitteet. Paikallinen päätepiste on paikallinen verkko-osoitteen eri porttiin. Palvelun Bus päätepisteen on muodostetut merkkijonon päätepisteosoite `sb`, nimitilan nimi ja polku "Ratkaisimen." Tuloksena on URI `sb://[serviceNamespace].servicebus.windows.net/solver`-Palvelupäätepisteen määrittäminen palvelun Bus TCP-päätepistettä täydellinen ulkoiseen DNS nimellä. Jos lisäät kyselyjä paikkamerkkien korvaaminen koodi `Main` funktion **palvelusovelluksen,** sinun on toiminnassa palvelu. Jos haluat kuunnella yksinomaan palvelun Bus palvelun, poista paikallinen päätepiste-ilmoitus.

### <a name="configure-a-service-host-in-the-appconfig-file"></a>Määritä service-isäntä App.config-tiedostossa

Voit myös määrittää host App.config-tiedoston avulla. Seuraavassa esimerkissä isännöinnin koodin tällöin palvelun tulee näkyviin.

```
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER to close");
Console.ReadLine();
sh.Close();
```

Päätepisteen määritelmiä siirtäminen App.config-tiedostoa. NuGet-paketti on jo lisännyt määritelmät solualueen App.config-tiedostona, joka palvelun Bus vaadittu-laajennuksia. Seuraavassa esimerkissä, joka on Edellinen koodi tarkka vastine pitäisi näkyä **system.serviceModel** osan alapuolella. Koodin tässä esimerkissä oletetaan, että projektin C#-nimitilan nimi on **palvelu**.
Korvaa paikkamerkit palvelun Bus palvelun nimitila ja Suojaussidosten avain.

```
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://namespace.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Kun olet tehnyt nämä muutokset, palvelu käynnistyy, ennen kuin, mutta kanssa live päätepisteet: yksi paikallisen ja yksi listening pilveen.

### <a name="create-the-client"></a>Luo asiakkaan

#### <a name="configure-a-client-programmatically"></a>Määrittää asiakkaan ohjelmallisesti

Tarjoaman palvelun, voit luoda WCF-asiakas, joka käyttää [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) -objekti. Palvelun Bus käyttää käyttämällä SAS tunnuksen perustuva suojaus-malli. [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx) -luokka edustaa suojaustunnuksen toimittaja valmiin factory menetelmiä, jotka palauttavat jotkin tunnetun suojaustunnuksen tarjoajat. Seuraavassa esimerkissä [CreateSharedAccessSignatureTokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.createsharedaccesssignaturetokenprovider.aspx) menetelmä käsittelemiseen tarvittavat SAS tunnuksen hankkiminen. Nimen ja avaimen ovat niitä saatu portaalin edellisessä kohdassa kuvatulla tavalla.

Kopioi tai ensin pikaohje `IProblemSolver` sopimuksen asiakkaan projektiisi koodi-palvelusta.

Korvaa-koodi `Main` asiakkaan palvelun Bus nimitilan ja Suojaussidosten avain uudelleen paikkamerkkitekstin korvaaminen-menetelmää.

```
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Voit nyt luoda asiakas- ja palvelun, näyttää niitä (suorittaminen palvelun ensin) ja asiakkaan palvelun soittaa ja tulostaa **9**. Voit suorittaa asiakkaan ja palvelimen eri tietokoneissa, jopa monista verkkojen ja tiedonsiirto toimivat edelleen. Asiakas-koodin suorittamisen myös pilveen tai paikallisesti.

#### <a name="configure-a-client-in-the-appconfig-file"></a>Määrittää asiakkaan App.config-tiedostoa

Seuraava koodi näkyy määrittämisestä asiakasohjelma käyttämällä App.config-tiedostoa.

```
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Päätepisteen määritelmiä siirtäminen App.config-tiedostoa. Seuraavassa esimerkissä, joka on sama kuin luettelossa aiemmin koodi pitäisi näkyä **system.serviceModel** osan alapuolella. Tässä kohdassa kuin aiemmin palauttamalla paikkamerkit palvelun Bus nimitilan ja SAS-näppäintä.

```
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://namespace.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus välityspalvelu perusteet, noudata näitä linkkejä lisätietoja.

- [Palvelun Bus välittäminen tekstiviesti yleiskatsaus](service-bus-relay-overview.md)
- [Azure palvelun Bus arkkitehtuuri yleiskatsaus](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
- Lataa palvelun Bus näytteiden [Azure objektit][] tai artikkelissa [Yleistä palvelun Bus objektit][].

  [Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
  [Azure-objektit]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
  [Palvelun Bus näytteiden yleiskatsaus]: ../service-bus-messaging/service-bus-samples.md