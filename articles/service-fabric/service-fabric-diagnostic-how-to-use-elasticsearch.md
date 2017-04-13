<properties
   pageTitle="Käytä Elasticsearch palvelun kangasta sovelluksen jäljitys-kaupan | Microsoft Azure"
   description="Kuvataan, miten palvelun kangasta sovellukset voivat käyttää Elasticsearch ja Kibana tallentamiseen, indeksi-ja etsiä sovelluksen jäljittää (lokit)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Käytä Elasticsearch palvelun kangasta sovelluksen jäljityksen nimellä tallentaa
## <a name="introduction"></a>Johdanto
Tässä artikkelissa kuvataan, miten [Azure palvelun kangasta](https://azure.microsoft.com/documentation/services/service-fabric/) sovellukset voivat käyttää **Elasticsearch** ja **Kibana** sovelluksen jäljitys muistiin, indeksoinnin ja haun. [Elasticsearch](https://www.elastic.co/guide/index.html) on Avaa lähde, eri aikajaksoille ja skaalattava reaaliaikainen haku- ja analytics ydin, joka on sovellu tämän tehtävän. Se voidaan asentaa Microsoft Azure-Windows- ja Linux näennäiskoneiden. Elasticsearch tehokkaasti voit käsitellä *rakenteellisia* jäljittää valmistettu käyttöä toiminnot, kuten **Tapahtuman jäljitys for Windows (tapahtumien seuranta)**.

Tapahtumien seuranta käyttämä palvelun kangasta Runtimen lähde vianmääritystiedot (tapahtumat). Palvelun kangasta sovellusten tietolähteen niiden vianmääritystiedot liian suositeltava tapa on. Samalla avulla saat korrelaatio suorituksenaikainen toimittaman ja sovelluksen toimitetaan jäljittää ja tekee vianmääritys on helppoa. Palvelun kangasta projektimallit Visual Studiossa ovat kirjaaminen API (.NET **EventSource** luokan perusteella), joka tietokoneesta kuuluu äänimerkki tapahtumien seuranta jäljittää oletusarvoisesti. Saat käyttämällä tapahtumien seuranta palvelun kangasta sovelluksen jäljityksen yleiskatsaus- [näyttö ja ohjelmistossa services paikallisesta kehittäminen asetuksissa](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Lisääminen Elasticsearch jäljittää ne on tallennetaan osoitteessa palvelun kangasta klusterisolmut reaaliajassa (kun sovellus on käynnissä) ja lähettää Elasticsearch päätepisteen. Seuraavassa on kaksi pää Jäljitä tallentamiskäytäntöjen:

+ **Prosessin jäljitys sieppaus**  
Sovelluksen tai tarkemmin palveluprosessi on vastuussa lähettää tietoja Jäljitä Store (Elasticsearch).

+ **Prosessi jäljitys sieppaus**  
Erillinen agentti sieppaus jäljittää prosessia tai prosesseja ja lähettäminen jäljitys-kaupasta.

Alla emme kerrotaan, kuinka voit määrittää Elasticsearch Azure-, keskustella ammattilaiset ja kuvakkeet sekä ottaa asetukset, ja kerrotaan, kuinka voit määrittää tietojen lähettäminen Elasticsearch palvelun kangasta palvelun.


## <a name="set-up-elasticsearch-on-azure"></a>Valitse Azure Elasticsearch määrittäminen
Azure Elasticsearch-palvelun määrittäminen eniten selkeä tapa on [**Azure Resurssienhallinta**](../azure-resource-manager/resource-group-overview.md)mallien. Täydellinen [Elasticsearch pikaopas Azure Resurssienhallinta mallia](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) ei saatavilla Azure pikaopas malleja säilöstä. Tämä malli käyttää erillisen tallennustilan tilit aikayksikön (solmujen ryhmät). Voit valmistella myös erillinen asiakkaan ja palvelimen solmujen eri määrityksiä ja yhdistetty tietojen levyjä eri määrän.

Tässä kohdassa Käytämme toiseen malliin, kutsua **ES MultiNode** [Azure diagnostiikkatyökalut säilöön](https://github.com/Azure/azure-diagnostics-tools). Tämä malli on helpompi käyttää, ja se luo suojattu HTTP perustodentamista Elasticsearch-klusterin. Ennen kuin jatkat, lataa säilö GitHub tietokoneeseesi (mukaan joko kloonaamalla säilö tai zip-tiedoston lataaminen). ES MultiNode malli sijaitsee saman niminen kansio.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Koneen Elasticsearch asennuksen komentosarjojen suorittamisen valmisteleminen
Helpoin tapa ES MultiNode mallinsa on annettu Azure PowerShell-komentosarjaa kutsutaan `CreateElasticSearchCluster`. Käytät tätä komentosarjaa, sinun täytyy asentaa PowerShell moduulit ja **openssl**-työkalua. Jälkimmäinen tarvitaan SSH-näppäintä, jonka avulla voidaan hallita Elasticsearch-klusterin etäyhteyden luomiseen.

`CreateElasticSearchCluster`komentosarja on suunniteltu helppous ES MultiNode mallin Windows-tietokoneesta. Voi käyttää mallia-Windows tietokoneeseen, mutta skenaarion on tämän artikkelin piiriin.

1. Jos et ole vielä asentanut ne jo, asenna [**PowerShellin Azure moduulit**](http://aka.ms/webpi-azps). Valitse pyydettäessä **Suorita**sitten **Asenna**. Azure PowerShell 1.3 tai uudempi versio vaaditaan.

2. [**Windowsin Git**](http://www.git-scm.com/downloads)jakautumisen sisältyy **openssl** -työkalu. Jos et ole vielä niin, asenna [Git Windows](http://www.git-scm.com/downloads) . (Asennuksen oletusasetusten ovat OK).

3. Oletetaan, että Git on asennettu, mutta sitä ei sisällytetä järjestelmäpolku, Avaa PowerShellin Microsoft Azure-ikkuna ja suorittamalla seuraavat komennot:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Korvaa `<Git installation folder>` Git sijainnin tietokoneessasi; Oletusarvo on **"C:\Program Files\Git"**. Huomautus ensimmäisen polun alkuun puolipiste-merkki.

4. Varmista, että olet kirjautuneena Azure (kautta [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet-komento) ja että olet valinnut tilaus, jota käytetään joustavasti haku-klusterin luominen. Voit tarkistaa, että oikea tilauksen valitaan käyttämällä `Get-AzureRmContext` ja `Get-AzureRmSubscription` cmdlet-komennot.

5. Jos et ole jo tehnyt niin, muuttaa nykyisen hakemiston ES MultiNode-kansioon.

### <a name="run-the-createelasticsearchcluster-script"></a>CreateElasticSearchCluster-komentosarjan suorittaminen
Ennen kuin suoritat komentosarjan, Avaa `azuredeploy-parameters.json` tiedoston ja Tarkista tai Anna arvot Komentosarjaparametrit. Seuraavat parametrit on tarkoitettu:

|Parametrin nimi           |Kuvaus|
|-----------------------  |--------------------------|
|dnsNameForLoadBalancerIP |Nimi, jota käytetään luomaan joustavasti haku-klusterin julkisesti näkyvät DNS-nimi (liittämällä Azure alueen toimialueelle määritettyä nimeä). Jos parametriarvo on "myBigCluster" ja Azure valittu alue on Länsi US, tuloksena olevat klusterin DNS-nimi on myBigCluster.westus.cloudapp.azure.com. <br /><br />Tämä nimi on myös useita palvelutiedot joustavasti haun klusterin, kuten tietojen solmu nimiä liittyvät pääkansion nimi.|
|adminUsername           |Järjestelmänvalvojan tilin nimi hallinta joustavasti haku-klusterin (vastaavan SSH avaimet on luotu automaattisesti).|
|dataNodeCount           |Joustavasti haku-klusterin näkyvien solmujen määrän. Komentosarjan nykyisen version erottele tiedot ja kyselyn solmujen; kaikki solmut toistaa molemmat roolit. Oletusarvo on 3 solmujen.|
|dataDiskSize            |Tietoja levyjen (gt) koon, joka on varattu tietojen kunkin solmun. Kukin solmu vastaanottaa 4 tietojen levyjen yksinomaan omistautunut joustavasti Search-palvelun.|
|alue                  |Azure alueen, joissa joustavasti haku-klusterin tulisi nimi.|
|esUserName              |Käyttäjä, joka on määritetty käyttämään ES klusterin (veloittaa HTTP perustodentamista) käyttäjänimi. Salasana ei ole parametrit-tiedoston osa ja annettava milloin `CreateElasticSearchCluster` komentosarja käynnistyy.|
|vmSizeDataNodes         |Azure virtuaalikoneen koon joustavasti haun klusterin solmujen. Standard_D2 oletusarvo.|

Nyt olet valmis suorittamaan komentosarja. Myöntää seuraava komento:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

Jos 

|Komentosarjan parametrin nimi    |Kuvaus|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Azure, joka sisältää kaikki joustavasti haun klusteriresursseja resurssiryhmän nimi.|
|`<azure-region>`         |Missä joustavasti haku-klusterin luodaan Azure alueen nimi.|         
|`<es-password>`          |Joustavasti haku-käyttäjän salasanan.|

>[AZURE.NOTE] Saat NullReferenceException testi AzureResourceGroup cmdlet-komento, jos olet unohtanut kirjautua Azure (`Add-AzureRmAccount`).

Jos saat virheilmoituksen komentosarjan suorittaminen ja voit määrittää, että virhe on aiheutunut väärä malli parametrin arvon, korjaa parametri-tiedosto ja Suorita komentosarja uudelleen eri resurssin ryhmänimi. Voit myös käyttää samaa resurssin ryhmän nimi ja on Puhdista vanhan lisäämällä komentosarja `-RemoveExistingResourceGroup` parametri komentosarja-kutsu.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Tulos CreateElasticSearchCluster komentosarja
Kun olet suorittanut `CreateElasticSearchCluster` komentosarjan, seuraavat tärkeimmät tiedot luodaan. Tässä esimerkissä oletetaan, että olet käyttänyt "myBigCluster" arvona `dnsNameForLoadBalancerIP` parametrin ja että alue, johon olet luonut klusterin on Länsi US.

|Palvelutietojen|Nimi, sijainti ja huomautukset|
|----------------------------------|----------------------------------|
|SSH avain etähallinta |myBigCluster.key-tiedosto (josta CreateElasticSearchCluster suoritettiin hakemistossa). <br /><br />Tämän avaimen tiedosto voidaan muodostaa järjestelmänvalvoja-solmu ja (kautta järjestelmänvalvoja-solmu)-klusterin solmut tiedot.|
|Järjestelmänvalvoja-solmu                        |myBigCluster admin.westus.cloudapp.azure.com <br /><br />Erillinen AM Elasticsearch klusterin etähallinnan--vain yksi, joka mahdollistaa ulkoisten SSH yhteydet. Se suoritetaan kuin Elasticsearch klusterisolmut kaikki samassa virtual verkossa, mutta sitä ei voi käyttää Elasticsearch palvelut.|
|Tietoja solmujen                        |myBigCluster1... *N* myBigCluster <br /><br />Tietoja solmujen käynnissä olevat Elasticsearch ja Kibana palvelut. Voit muodostaa SSH kunkin solmun kautta, mutta vain järjestelmänvalvoja-solmu kautta.|
|Elasticsearch klusteri             |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Ensisijainen päätepiste Elasticsearch klusterin (Huomautus /es liite). Se on suojattu (tunnistetiedot on määritetty esUserName/esPassword parametrit ES MultiNode mallin) HTTP perustodentamista. Klusterin on myös pää laajennus asennettuna (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) klusterin hallinnan.|
|Kibana-palvelu                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Kibana-palvelu on määritetty näyttämään Elasticsearch luodun klusterin tiedot. Se on suojattu todennus tunnistetietoja kuin klusterin itse.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>Prosessin ja prosessi jäljitys sieppaus
Johdanto, on mainittu kaksi käyttötavat, diagnostiikan tietojen keräämisen: prosessin ja prosessi. Jokaisella vahvuuksien ja heikkouksien.

**Prosessin jäljitys sieppaus** etuja ovat:

1. *Asetusten määritys ja käyttöönotto*

    * Diagnostiikan tietojen kerääminen määritys on vain osa sovelluksen-määritys. On helppo aina synkronoiminen se "" sovelluksen muilta.

    * Sovelluksen kohti tai palvelun-määritys on helposti mahdollista.

    * Yleensä prosessi jäljitys sieppaus vaatii erillisen käyttöönotto ja diagnostiikan agentti, joka on erittäin hallintatehtävän ja virheiden mahdollisia tietolähteen määrittäminen. Tietyn agent-tekniikkaa avulla usein vain yhden esiintymän agentti kohti virtuaalikoneen (solmu). Tämä tarkoittaa, että määritys sivustokokoelman diagnostiikan määritys on jaettu kaikkien sovellusten kesken ja solmun palveluita.

2. *Joustavuus*

    * Sovelluksen, voit lähettää tiedot kaikkialla, missä voit siirtyä tarvitaan, kunhan on asiakirjakirjastoon, joka tukee kohdennettujen tietojen tallennustilan-järjestelmä. Uusi poistumia voidaan lisätä vain haluamasi.

    * Monimutkaisia sieppaus, suodattaminen ja tietojen koostaminen sääntöjä voidaan ottaa käyttöön.

    * Prosessi jäljitys-sieppaus usein rajoitetut tiedot poistumia, joka tukee agentti. Jotkin tekijöiden on laajennettava.

3. *Sisäinen sovelluksen tietoja ja konteksti*

    * Diagnostiikan alirakenne sisällä sovelluspalvelu /-prosessi käynnissä voit helppo täydentäminen jäljittää tilannekohtaiset tiedoilla.

    * Prosessi vaihtoehto tiedot on lähetettävä agentti joitakin prosessien välisen tietoliikenteen järjestelmä, kuten tapahtuman jäljitys for Windowsin kautta. Se voi asettaa lisärajoituksia.

**Prosessi jäljitys sieppaus** etuja ovat:

1. *Valvoa ja kerääminen kaatumisen voi kirjoittaa*

    * Prosessin jäljitys sieppaus voi epäonnistua, jos sovellus ei käynnisty tai kaatuu. Riippumattomien agentti on paljon parempi kokeilemaan sieppaaminen tärkeitä vianmääritykseen liittyviä tietoja.<br /><br />

2. *Erääntymispvm, vakauden ja osoitettu suorituskykyä*

    * Ympäristön toimittaja (kuten Microsoft Azure diagnostiikka-agentti) kehittämä agentti on ollut tiukka testaus ja taistelu hardening.

    * Prosessin seurantatiedot sieppaus, jossa on otettava tarkkaan varmistaa, että toiminnan lähettämällä diagnostiikkatiedot sovelluksen prosessi ei häiritse sovelluksen tärkeimmät tehtävät tai esitellä ajoitus- ja suorituskyvyn ongelmia. Itsenäisesti käynnissä agentti on helposti ongelmat ja rajoittaa sen vaikutus järjestelmän esimääritettyjä.

Voi yhdistää ja hyötyä kummassakin on. Myös voi olla useita sovelluksia parhaan ratkaisun.

Tässä kohdassa Käytämme **Microsoft.Diagnostic.Listeners kirjastoon** ja lähettää tietoja palvelun kangasta sovelluksesta Elasticsearch-klusterin sieppaus prosessin jäljitys.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Lähetä diagnostiikkatiedot Elasticsearch kuuntelijoita kirjaston avulla
Microsoft.Diagnostic.Listeners kirjasto on PartyCluster otoksen palvelun kangasta sovelluksen osa. Voit käyttää sitä seuraavasti:

1. Lataa [PartyCluster otosten](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) GitHub.

2. Kopioi Microsoft.Diagnostics.Listeners ja Microsoft.Diagnostics.Listeners.Fabric-projektit (koko kansiot) PartyCluster otoksen hakemiston sovellus, joka on tarkoitettu tietojen lähettäminen Elasticsearch ratkaisu-kansioon.

3. Avaa kohde-ratkaisun hiiren kakkospainikkeella Solution Explorerissa ratkaisu-solmu ja valitse **Lisää olemassa olevaan projektiin**. Voit lisätä Microsoft.Diagnostics.Listeners projektin ratkaisu. Toista sama Microsoft.Diagnostics.Listeners.Fabric projektin.

4. Lisää projektiviittauksen palvelun projekteihin kaksi lisätyt projektit. (Kunkin palvelun, joka on tarkoitettu tietojen lähettäminen Elasticsearch tulee viitata Microsoft.Diagnostics.EventListeners ja Microsoft.Diagnostics.EventListeners.Fabric).

    ![Projektiviitteet Microsoft.Diagnostics.EventListeners ja Microsoft.Diagnostics.EventListeners.Fabric kirjastoihin][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Palvelun kangasta Yleiset julkistamista ja Microsoft.Diagnostics.Tracing Nuget pakkaus
Sovellukset, jotka on luotu palvelun kangasta Yleiset julkistamista (2.0.135, julkaissut 2016 31 maaliskuun) kohde **.NET Framework 4.5.2**. Tämä versio ei tue Azure GA julkaisemisen milloin .NET Framework uusin versio. Valitettavasti puitteissa tässä versiossa ei ole tiettyjä EventListener API Microsoft.Diagnostics.Listeners kirjaston korjattava. Koska EventSource (osa, joka perustan ohjelmointirajapinnan kirjautumisesta kangasta sovellukset) ja EventListener tiiviisti kohdalla, joka projektista, jossa käytetään Microsoft.Diagnostics.Listeners kirjaston käytettävä EventSource vaihtoehtoinen soveltaminen. Tämä toteutus tarjoaa **Microsoft.Diagnostics.Tracing Nuget-paketti**, Microsoft tekijä. Paketin on täysin yhteensopivan EventSource sisältyvät puitteissa, jotta muutoksia koodin pitäisi olla tarpeen kuin viitattua nimitilan muutokset.

Voit käyttää EventSource luokan Microsoft.Diagnostics.Tracing käyttöönoton kunkin palvelun projektin tietojen lähettäminen Elasticsearch korjattava seuraavasti:

1. Palvelun projektin hiiren kakkospainikkeella ja valitse **Nuget pakettien hallinta**.

2. Siirry nuget.org paketin lähde (jos se ei ole jo valittuna) ja Hae "**Microsoft.Diagnostics.Tracing**".

3. Asenna `Microsoft.Diagnostics.Tracing.EventSource` pakkauksen (ja sen riippuvuutta).

4. Avaa palvelu projektin **ServiceEventSource.cs** tai **ActorEventSource.cs** -tiedosto ja korvaa `using System.Diagnostics.Tracing` direktiivi tiedoston päälle `using Microsoft.Diagnostics.Tracing` direktiivin.

Näin ei ole tarpeen, kun **.NET Framework 4.6** tue Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch listener esiintymän ja määritykset
Diagnostiikkatiedot lähettämiseen Elasticsearch viimeinen vaihe on luotava esiintymän `ElasticSearchListener` ja määritä se Elasticsearch yhteyden tietoja. Kuuntelutoiminto sieppaa automaattisesti kaikki tapahtumat korotettuna määritettyjä palvelun projektin EventSource luokkia kautta. Se on oltava toiminnassa palvelun elinkaaren aikana, jotta alustus palvelukoodi on hyvä asiakirja sitten kannattaa luoda. Miten alustus koodi tilattomien palvelusta saattaa näyttää jälkeen tarvittavat muutokset (alkaen kommentit maininnut lisäykset `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch yhteyden tietoja täytyy sijoittaa eri osaan palvelun määritystiedostossa (**PackageRoot\Config\Settings.xml**). Osan nimi on vastattava välitetty arvo `FabricConfigurationProvider` konstruktoria, esimerkiksi:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Arvojen `serviceUri`, `userName` ja `password` parametrit vastaavat Elasticsearch klusterin päätepisteosoite, Elasticsearch käyttäjänimi ja salasana. `indexNamePrefix`on etuliite Elasticsearch indeksien; Microsoft.Diagnostics.Listeners kirjaston Luo uusi indeksi tietoja päivittäin.

### <a name="verification"></a>Todentaminen
Joka on tämä! Nyt palvelu suoritetaan, kun se käynnistyy lähettäminen jäljittää Elasticsearch palveluun määritykset. Voit tarkistaa tämän avaamalla Kibana Käyttöliittymän liittyvä kohde Elasticsearch-esiintymä. Tässä esimerkissä sivun osoite on http://myBigCluster.westus.cloudapp.azure.com/. Tarkista, että valittu nimi etuliitteellä indeksit `ElasticSearchListener` esiintymää on varmasti luotu ja tietoja.

![Kibana näyttäminen PartyCluster sovelluksen tapahtumat][2]

## <a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja ohjelmistossa ja palvelun kangasta palvelu seuranta](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
