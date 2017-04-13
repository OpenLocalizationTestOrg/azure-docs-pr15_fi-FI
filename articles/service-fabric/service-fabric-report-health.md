<properties
   pageTitle="Lisää mukautettu palvelun kangasta kuntoraportit | Microsoft Azure"
   description="Tässä artikkelissa käsitellään lähettää mukautetun kuntoraportit kangasta Azure-palvelun kunto kohteisiin. Antaa suosituksia suunnitteluun ja toteutukseen laatu kuntoraportit."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="add-custom-service-fabric-health-reports"></a>Lisää mukautettu palvelun kangasta kuntoraportit
Azure palvelun kangasta esitellään suunniteltu Merkitse perusasemassa klusterin ja sovelluksen ehtoja tiettyjen kohteiden [kunto-malli](service-fabric-health-introduction.md) . Kunto-malli käyttää **kunto reporters** (järjestelmäosat ja watchdogs). Tavoitteena on helppoa ja nopeaa vianmäärityksen ja korjauksen. Palvelun kirjoittajat on mielestäsi jaettu kuntoa. Ehto, joka saattaa olla vaikutusta kunto on ilmoitettava, etenkin silloin, kun se voi auttaa merkinnän ongelmia lähellä pääkansio. Kuntotietojen tiedot säästää aikaa ja työmäärään virheenkorjaus ja tutkimuksen kerran palvelu on hyvin alkuun käytetään pilveen (yksityinen tai Azure).

Palvelun kangasta reporters näytön tunnisti halutut ehdot. Ne raportoida edellytyksistä niiden paikallisen näkymään perustuvan. [Kuntotietojen tallentaa](service-fabric-health-introduction.md#health-store) kokoaa yhteen kaikki reporters lähettämä ovatko kohteiden yleisesti kunnossa kunto tiedot. Malli on tarkoitettu monipuolisia, joustava ja käyttöä. Äänenlaatu kuntoraportit määrittää klusterin kunto-näkymän tarkkuutta. Tunnistettujen, jotka näyttävät virheellisesti perusasemassa ongelmat voivat vaikuttaa heikentää päivitykset tai muut palvelut, jotka käyttävät kunto tiedot. Esimerkkejä näiden palvelujen ovat korjauspalvelut ja suunnattujen varoitusmenetelmien järjestelmiä. Jotkin haluat näyttää tarvitaan antamaan raportteja, joista ehdot koskevia parhaalla mahdollisella tavalla.

Voit suunnitella ja toteuttaa kunto-raportoinnin watchdogs ja järjestelmäosat on:

- Määritä ehto, he voivat olla kiinnostuneita, niin kuin se on seurattava ja niiden vaikutuksesta klusterin tai sovelluksen toimintoja. Näiden tietojen perusteella päättää kuntotietojen ja kuntotietojen Raporttiominaisuus.

- Selvitä, raportti koskee [kohteen](service-fabric-health-introduction.md#health-entities-and-hierarchy) .

- Määrittää, missä raportointi on valmis, valitse palvelun tai sisäisiä ja ulkoisia watchdog.

- Määritä tunnistetaan reporter lähteen.

- Valitse raportoinnin strategia, joko säännöllisesti tai siirtymät. Suositeltu tapaa, jolla on säännöllisesti, sellaisena kuin se vaatii yksinkertaisempi ja on virheet helposti.

- Määrittää, kuinka kauan perusasemassa ehdot-raportti kannattaa pitää kunto-kaupan ja miten se pitäisi olla valittuna. Lisätietoja päättää live raportin aikaa ja poista-vanheneminen toimintaa.

Kuten edellä, raportointi voit tehdä:

- Valvotun palvelun kangasta palvelun se.

- Sisäinen watchdogs käyttöön palvelun kangasta palveluna (esimerkiksi palvelun kangasta tilattomien palvelu, joka valvoo ehdot ja raporttien ongelmat). Watchdogs voi olla käyttöön kaikissa solmuissa tai olla affinitized valvottu-palveluun.

- Sisäinen watchdogs, suorita palvelun kangasta solmuissa mutta jotka *ole* toteutettu palvelun kangasta palvelut.

- Ulkoisen watchdogs, tutkia resurssi *ulkopuolella* palvelun kangasta klusterin (esimerkiksi seurantaa palvelun kuten Gomez).

> [AZURE.NOTE] Klusterin lisätään ulos-ruutuun lähetetty järjestelmän osien kuntoraportit. Lue lisää [käyttäminen järjestelmän kunnon](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)raportit vianmääritystä varten. Käyttäjäraportit lähetetään, joka on jo luotu järjestelmän [kunto kohteita](service-fabric-health-introduction.md#health-entities-and-hierarchy) .

Kerran kuntotietojen raportoinnin rakenne ei ole valittuna, kuntoraportit voi lähettää helposti. Voit käyttää [FabricClient](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.aspx) raportin terveyden, jos klusterin ei ole [suojattu](service-fabric-cluster-security.md) tai jos kangasta asiakas on järjestelmänvalvojan oikeudet. Tämä onnistuu käyttämällä [FabricClient.HealthManager.ReportHealth](https://msdn.microsoft.com/library/system.fabric.fabricclient.healthclient.reporthealth.aspx)Ohjelmointirajapinnan kautta, PowerShellin kautta tai muiden kautta. Määritysten nupit erän raporttien suorituskyvyn parantamiseksi.

> [AZURE.NOTE] Raportin kunto on synkronoitu ja se on vain asiakaspuolen vahvistus työt. Raportin hyväksytään kunto-asiakasohjelman kertoma tai `Partition` tai `CodePackageActivationContext` objekteja ei tarkoita, että se on kohdistettu Storessa. Se lähetetään asynkronisesti ja mahdollisesti erämuotoinen muita raportteja. Palvelimessa käsittely edelleen saattaa epäonnistua: järjestysnumero voi olla vanhentunut, johon raportti on otettava käyttöön kohde on jo poistaa, jne.

## <a name="health-client"></a>Kunto-asiakas
Kuntoraportit lähetetään kunto asiakasohjelmalla, joka sijaitsee sisällä kangasta asiakkaan kunto-kaupasta. Kunto-asiakas voidaan määrittää kanssa seuraavasti:

- **HealthReportSendInterval**: raportin lisätään asiakkaan aikaa ja se lähetetään kunto Store ajankohdan viive. Käyttää erä raportteihin jokaisen raportin siirtäminen yhden viestin lähettämisen yhden viestin sijaan. Voit parantaa suorituskykyä jonottaminen. Oletusarvo: 30 sekuntia.

- **HealthReportRetrySendInterval**: aikaväli, jolla kunto-asiakasohjelman lähettää kertynyt kunto raportoi kunto-kauppa. Oletusarvo: 30 sekuntia.

- **HealthOperationTimeout**: aikakatkaisuajan raportin viesti lähetetään kunto-kaupasta. Jos viestin aikakatkaistaan, terveys-asiakas yrittää suorittaa uudelleen sen kunnes kunto-kaupan vahvistaa raportin käsitteleminen. Oletusarvo: kahden minuutin.

> [AZURE.NOTE] Kun raportit ovat erämuotoinen, kangasta asiakas on pidettävän avoinna, vähintään HealthReportSendInterval varmistaa, että ne lähetetään. Jos viesti on kadonnut tai kunto-kaupan ei voi käyttää niitä lyhytkestoisia virheiden vuoksi, kangasta asiakas on pidettävän avoinna enää antaa sille mahdollisuuden uudelleen.

Puskurointi asiakkaan otetaan huomioon yksiselitteisyys-raportit. Esimerkiksi jos tietyn virheelliset reporter raportoi 100 raporttien sekunnissa saman kohteen saman ominaisuutta, raporttien korvataan viimeisin versio. Vain yksi tällainen raportti olemassa enintään asiakkaan jonossa. Jos jonottaminen on määritetty, terveys Store lähetettyjä raportteja määrä on vain yksi Lähetä aikavälillä. Tämä raportti on lisätty edellisen raportin, jossa kuvastaa kohteen uusimmat tilan.
Kaikki parametrit voi olla määritetty milloin `FabricClient` luodaan välittämällä [FabricClientSettings](https://msdn.microsoft.com/library/azure/system.fabric.fabricclientsettings.aspx) kunto liittyviä merkintöjä haluamasi arvot.

Seuraavat Luo kangasta asiakkaan ja määrittää, että raportit lähetetään, kun heidät lisätään. Aikakatkaisu ja virheistä, jotka voivat yritettävä uudelleenyritykset käydä 40 sekunnin välein.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

Samat parametrit voidaan määrittää luotaessa yhteyttä klusterin PowerShellin kautta. Paikallisen klusterin yhteyden alkaa seuraavasti:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

> [AZURE.NOTE] Jotta luvattoman services ei voi raportoida kunto klusterin kohteiden vastaan, palvelimen voi määrittää hyväksymään pyynnöt vain suojatun asiakkaat. `FabricClient` Raportoinnissa on on suojauksen käyttöön voivat pitää yhteyttä klusterin (esimerkiksi ja Kerberos- tai Varmenteen todentaminen). Lisätietoja [klusterin suojaus](service-fabric-cluster-security.md).

## <a name="report-from-within-low-privilege-services"></a>Raportti sisällä pienen käyttöoikeuksien palvelut
-Palvelun kangasta palvelut, joita ei ole järjestelmänvalvojan käyttöoikeudet klusterin sisällä voit raportoida kunto osallistujina nykyisen kontekstin kautta `Partition` tai `CodePackageActivationContext`.

- Tilattomien Services-palvelun nykyisen esiintymän raportoinnissa [IStatelessServicePartition.ReportInstanceHealth](https://msdn.microsoft.com/library/system.fabric.istatelessservicepartition.reportinstancehealth.aspx) avulla.

- Tilallisten palveluiden replikat raportoinnissa [IStatefulServicePartition.ReportReplicaHealth](https://msdn.microsoft.com/library/system.fabric.istatefulservicepartition.reportreplicahealth.aspx) avulla.

- [IServicePartition.ReportPartitionHealth](https://msdn.microsoft.com//library/system.fabric.iservicepartition.reportpartitionhealth.aspx) avulla voit raportoida nykyisen osiota kohteen.

- [CodePackageActivationContext.ReportApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportapplicationhealth.aspx) avulla voit raportoida nykyinen sovellus.

- Käytä [CodePackageActivationContext.ReportDeployedApplicationHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth.aspx) raportointia käyttöön nykyisen solmun nykyinen sovellus.

- Käytä [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://msdn.microsoft.com/library/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth.aspx) palvelupakettiin avoinna olevan sovelluksen käyttöön nykyisen solmun raportointia.

> [AZURE.NOTE] Sisäisesti `Partition` ja `CodePackageActivationContext` pidä kunto-asiakas, joka on määritetty oletusasetukset. Sama kuvaus [kunto](service-fabric-report-health.md#health-client) -asiakasohjelman, käytä - raporttien erämuotoinen ja lähetetty ajastimen ja jotta objektien pitäisi pidettävän voit lähettää raportin on avoinna.

## <a name="design-health-reporting"></a>Suunnittele kunto raportointi
Ensimmäinen vaihe laadukkaita raporttien luominen on määrittää ehdot, jotka voivat vaikuttaa palvelun kunto. Mikä tahansa ehto, jotka auttavat merkinnän vianmääritys service- tai klusterin, kun se käynnistyy--tai sitäkin parempi ennen ongelma tapahtuu--voi mahdollisesti tallentaa dollareina miljardeja. Mitä etuja ovat vähemmän aikaa alaspäin, vähemmän yöllä tuntia käytetyt tutkiminen ja asiakkaan tyytyväisyyttä ja liittyvien ongelmien korjaaminen.

Kun ehdot on määritetty, watchdog kirjoittajat on paras tapa etsiä tasapaino yleiskuluja ja hyödyllisyys laskemisesta. Esimerkkinä palvelu, joka toimii, jotka käyttävät joitakin väliaikaiset tiedostot jaettuun Monimutkaisten laskutoimitusten suorittaminen. Watchdog voi seurata Jaa, varmista, että tarpeeksi tilaa on käytettävissä. Onnistunut kuuntelevat tiedoston tai kansion muutokset ilmoituksia. Se raportin varoitus, jos jaettu raja-arvo on saavutettu ja ilmoittaa virheestä, jos Jaa on täynnä. Valitse varoitus, korjaa järjestelmän voi käynnistää Siivotaan vanhoja tiedostoja, valitse Jaa. Virheen, valitse korjaa järjestelmän voi siirtää toisen solmun palvelun se. Huomaa, miten ehto osavaltiot kuvataan mukaisesti terveyden: ehto, joka voidaan pitää kunnossa tilaan tai perusasemassa (varoitus tai virhe).

Kun seurannan tiedot on määritetty, watchdog writer on ottamisesta käyttöön watchdog voi selvittää. Jos ehdot voidaan määrittää palvelun, watchdog voi olla osa valvottu palvelun itse. Esimerkiksi palvelukoodi voit tarkistaa Jaa käyttö ja sitten raportin avulla paikallinen kangasta asiakkaan aina, kun se yrittää kirjoittaa tiedoston. Tämän menetelmän etuna on se raportointi on helppoa. Voit estää watchdog virheet vaikuttavat service-toimintoja, vaikka on otettava tarkkaan.

Raportointi-toimintoa valvottu palvelu ei ole aina haluamasi vaihtoehto. Palvelun watchdog ei ehkä pysty tunnistamaan ehdot. Se ei saa olla logiikan tai tietojen määrittämistä. Seurannan ehdot katseltavan voi olla suuri. Ehdot myös eivät välttämättä ole tietyn palvelun, mutta vaikuttaa sijaan vuorovaikutuksesta palvelut. Toinen vaihtoehto on sisältävät watchdogs klusterin kuin erilliset prosessit. Watchdogs seurata vain ehtoja ja raportti vaikuttamatta minkäänlaista palveluista. Esimerkiksi nämä watchdogs voi toteuttaa tilattomien services saman sovelluksessa käyttöön kaikissa solmuissa sama kuin palvelussa solmuissa nimellä.

Joskus watchdog, käynnissä klusterin vaihtoehto ei ole joko. Jos valvottu ehto on käytettävyys tai palvelun toimintoja, kun käyttäjät näkevät sen, on parasta pitää watchdogs kuin käyttäjän asiakkaat samassa paikassa. Ne, Testaa toimintojen käyttäjien kutsua niitä samalla tavalla. Voit määrittää esimerkiksi watchdog, joka sijaitsee klusterin ulkopuolella ja ongelmia pyynnöt palveluun ja kuittaa viive ja tulos oikeellisuuden. (Laskuri-palvelun, kuten 2 + 2 palauttaa 4 paljon aikaa?)

Kun watchdog tiedot on vahvistettu, voit päättää Lähdetunnus, joka yksilöi sen. Jos useita watchdogs samantyyppisiä kotiosoitteessa klusterin, joko ne on raportoitava eri kohteiden tai jos ne raportoinnissa saman kohteen, ne on varmistettava, että Lähdetunnus tai ominaisuus ei ole. Tällä tavalla niiden raportit voivat olla. Ominaisuus, kuntoraportti olisi siepata valvottu ehto. (Edellä olevassa esimerkissä ominaisuus voi olla **ShareSize**.) Jos saman ehdon raportit-ominaisuuden pitäisi näkyä dynaamisia tietoja, joka sallii päällekkäisen raportit. Esimerkiksi jos useita jaettuja resursseja on seurattava, ominaisuuden nimi voi olla **ShareSize resurssi**.

> [AZURE.NOTE] Kunto-kaupan olisi *ei* voi käyttää pitämään tilatiedot. Vain terveyteen liittyviä tietoja olisi ilmoitetaan kunto, kun nämä tiedot vaikuttaa kohteen kunto-arviointi. Kunto-kaupan ei ole suunniteltu yleinen kaupan nimellä. Se käyttää kunto arvioinnin logiikan koota kaikki tietoja kunto-tilaan. Kunto (kuten raportoinnin tila, kun OK kuntotietojen) liittymättömiä tietojen lähettäminen eivät vaikuta koostetun kuntotietojen, mutta se heikentää kunto myymälän suorituskyky.

Seuraava päätöskohta ei minkä kohteen raporttiin. Yleensä, tämä on selvää, ehdon perusteella. Parasta mahdollista rakeisuuden kanssa, sinun on valittava kohde. Jos ehdon vaikuttaa kaikkien replikoiden osioon, Ilmoita osioon, ole-palvelun. On yläkulmassa tapauksia, joissa yksi haluat näyttää tarvitaan, vaikka. Jos ehto vaikuttaa kohteen, kuten replikan, mutta halu on ehto merkitty yli replikan elinkaaren kestoa ja valitse tulisi ilmoittaa osioon. Kun se on poistettu, kaikkien raporttien liittyy poistetaan-kaupasta. Tämä tarkoittaa, että watchdog kirjoittajat on myös mietittävä käyttöajan ja raportin. Se on oltava Tyhjennä, kun raportti on puhdistettava kaupasta (esimerkiksi silloin, kun kohde virhe voi enää käyttää).

Katsotaan esimerkki, joka kokoaa yhteen voin kuvattu kohdeosoite. Harkitse palvelun kangasta-sovelluksen koostuu perusmuodon tilallisten jatkuva palvelu ja toissijainen tilattomien palvelut käyttöön kaikissa solmuissa (yhden toissijaisen palvelutyyppi mistäkin tehtävän). Perusmuodon on käsittely jono, joka sisältää komennot, joilla voidaan suorittaa secondaries. Secondaries Suorita pyynnöt ja lähetä takaisin kuittaus signaalit. Yksi ehto, joka voidaan valvoa on perustyylin käsittelyn jonon pituuden. Jos perusmuodon jono saavuttaa raja-varoituksen ilmoitetaan. Varoitus osoittaa, secondaries ei voi käsitellä Lataa. Jos jonossa saavuttaa enimmäispituus ja komennot jätetään pois, virheen ilmoitetaan, kun palvelua ei voi palauttaa. Raporttien voi olla ominaisuutta **QueueStatus**. Watchdog sijaitsee sisällä palvelu ja se lähetetään perusmuodon ensisijainen se säännöllisesti. Time to live on kaksi minuuttia ja se lähetetään säännöllisesti 30 sekunnin välein. Ensisijainen siirtyy, jos raportissa on siivotaan automaattisesti-kaupasta. Jos palvelun se on käytössä, mutta se on lukkiutunut tai muita ongelmia raportin päättyy kunto-kaupasta. Tässä tapauksessa kohteen arvioidaan virheen.

Toinen ehto, joka voidaan valvoa on tehtävä suoritusaika. Perusmuodon jakaa tehtävätyyppiä perusteella secondaries tehtävät. Rakenteen perusmuodon voitu äänestyksen järjestäminen secondaries tehtävän tilan. Se voi myös odottaa secondaries Lähetä takaisin kuittaus signaalit, kun ne on valmis. Toinen tapauksessa tarkkaan on otettava esiintyvien tilanteissa, joissa secondaries die tai viestit on menetetään. Yksi vaihtoehto on perusmuodon Lähetä ping-pyyntö samaan toissijainen, joka lähettää sen tila. Jos ei ole tila on vastaanotettu, perusmuodon katsoo epäonnistui ja siirtää tehtävän ulkopuolelle. Tämä ongelma oletetaan, että tehtävät ovat idempotent.

Valvotun ehto voidaan kääntää on varoitus, jos tehtävää ei tehdä tietyn ajan (**t1**, kuten kymmenen minuuttia). Jos tehtävä on valmis ei senhetkinen (**t2**, kuten 20 minuutin ajan), valvottu ehto voidaan kääntää virheeksi. Tähän raportointiin voidaan toteuttaa eri tavoin:

- Perusmuodon ensisijainen se raportit itse säännöllisin väliajoin. Voit määrittää yhden ominaisuuden odotetaan tehtävissä jonossa. Jos vähintään yksi tehtävä kestää kauemmin, ominaisuuden **PendingTasks** raportin tila on varoitus tai virhe tarpeen mukaan. Jos ei ole odotetaan tehtäviä tai kaikkien tehtävien suorittaminen, raportin tila on OK. Tehtävät ovat jatkuva. Jos ensisijainen, juuri ylemmän tason ensisijainen voit jatkaa raportin oikein.

- Toinen watchdog prosessi (-pilvi tai ulkoinen) tarkistaa tehtävät (-ulkopuolella, joka perustuu haluamasi tehtävä tulokset) näet, jos ne ovat valmiit. Jos ne eivät tue raja-arvot, raportin lähetetään master-palvelusta. Raportin lähetetään myös kunkin tehtävän, joka sisältää tehtävän-tunniste, kuten **PendingTask + taskId**. Raportit lähetetään vain perusasemassa hyötyä käyttöön. Määritä live muutama minuutti ja Merkitse poistetaan, kun ne vanhenevat varmistamiseksi uudelleenjärjestäminen raportit.

- Toissijaisen, joka suoritetaan tehtävän ilmoittaa, kun kestää odotettua toimii puhelimessasi. Se raportit ominaisuuden **PendingTasks**esiintymä. Raportin pinpoints palveluesiintymä, jossa on ongelmia, mutta se ei siepata tilanteeseen, jossa esiintymän kuolee. Raporttien leikkauskohdat Siivotaan sitten. Voitu ilmoittaa toissijainen-palvelusta. Jos toissijaisen suorittaa tehtävän, toissijainen esiintymän tyhjentää raportin-kaupasta. Raportin ei siepata tilanteeseen, jossa kuittaus viestin menetetään ja tehtävä ei ole valmis perusmuodon näkökulmasta.

Raportoinnin on valmis kuvattuun tapauksissa, mutta raportit ovat siepata sovelluksen kunto kun kunto arvioidaan.

## <a name="report-periodically-vs-on-transition"></a>Raportti säännöllisesti ja siirtymä
Käyttämällä kunto raportoinnin mallin watchdogs lähettää raportteja säännöllisesti tai siirtymät. Suositeltu tapaa watchdog raportointia varten on määräajoin, koska koodi on paljon yksinkertaisempi ja pienempi voi enää virheitä. Watchdogs on pyri olevan mahdollisimman yksinkertainen, voit välttää virheet, joka käynnistää virheellinen raportit. Virheellinen *perusasemassa* raporttien vaikuttaa kunto arvioinnit ja mukaisesti terveyden, mukaan lukien päivitysten skenaarioita. Virheellinen *kunnossa* raporttien piilottaa ongelmat klusterin, joka ei ole toivottuja.

Säännöllisiä raportointia varten watchdog voidaan toteuttaa ajoittaminen. Timer-takaisinsoiton watchdog voit tarkistaa tilan ja lähettää raportin nykyisen tilan perusteella. Ei ole tarpeen, katso, mitä raportti on lähetetty aiemmin tai minkä tahansa optimointi kannalta messaging. Kunto-asiakkaalla on jonottaminen logiikan apua suorituskykyä. Kunto-asiakas on käytettävissä toiminnassa, kun se yrittää suorittaa uudelleen sisäisesti kunnes raportti on Kuittaa kunto-kaupasta watchdog muodostaa uudempaan raportin kohteen, ominaisuuden ja lähde.

Valtion varovainen käsittely edellyttää raportoinut siirtymät. Watchdog valvoo Jotkin ehdot ja raportoi vain, kun ehdot muuttuvat. Tämän menetelmän upside on vähemmän raporttien tarvitaan. Entä huonot puolet siitä, että watchdog logiikan on monimutkaisia. Watchdog on säilytettävä ehdot tai raportteja, niin, että ne voidaan tarkastaa määrittämään tila muuttuu. Automaattisesti, valitse tarkkaan otettava, kun raportti on lähetetty, jotka voivat ei ole lähetetty aiemmin (jonossa, mutta ei vielä lähetetty kunto-kauppa). Järjestysnumero on oltava koskaan lisääntyvien. Jollet ole ajan tasalla olevia raportteja on hylännyt. Jos tietojen menettämisen on syntynyt harvinaisissa tapauksissa synkronoinnin voivat olla tarpeen välillä reporter tilan ja tila kunto-kaupasta.

Siirtymät raportointi on järkevää palveluiden raportoinut itse kautta `Partition` tai `CodePackageActivationContext`. Kun paikallinen objekti (replikan tai käyttöön palvelupakettiin / sovelluksen käyttöön) on poistettu, kaikkien raporttien poistetaan myös. Tämä Automaattinen puhdistus relaxes edellyttää reporter ja terveyteen kaupan välisen synkronoinnin. Jos raportti on ylemmän tason osioon tai ylätason sovellus, tarkkaan on otettava käyttöön automaattisesti vanhentuneiden raporttien kunto-kaupan välttämiseksi. Logiikan on lisättävä säilyttää oikeassa tilassa ja poista raportin kaupasta, kun ei enää tarvita.

## <a name="implement-health-reporting"></a>Toteuta kunto raportointi
Kun kohde ja raportin tietoja ei ole valittu, lähettäminen kuntoraportit voidaan toteuttaa API, PowerShell tai muille käyttäjille.

### <a name="api"></a>OHJELMOINTIRAJAPINTA
Voit raportoida Ohjelmointirajapinnan kautta, haluat luoda kuntoraportti he haluavat raportoinnissa kohteen tyyppi. Anna raportille kunto asiakkaalle. Voit myös luoda kunto-tiedot ja välittää ne raportoinnin menetelmiä korjaa `Partition` tai `CodePackageActivationContext` raportointia nykyisen kohteita.

Seuraavassa esimerkissä säännöllisiä watchdog sisällä klusterin raportit. Watchdog tarkistaa, onko ulkoinen resurssi voidaan käyttää solmu kuluessa. Resurssi tarvitsee palvelun luettelo-sovelluksessa. Jos resurssi on ei käytettävissä, muiden sovelluksessa edelleen voit toimi oikein. Tämän vuoksi raportin lähetetään käyttöön palvelu package kohteen 30 sekunnin välein.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShellin
Lähetä ja * *Lähetä ServiceFabric*entiteettityypin*HealthReport ** kuntoraportit.

Seuraavassa esimerkissä säännöllisiä raportoinut solmu suorittimen arvot. Raportit lähetetään 30 sekunnin välein, ja ne on Live kahden minuutin ajan. Jos ne voimassaolo päättyy, reporter on ongelmista on, joten solmu arvioidaan virheen. Suoritin ollessa kynnysarvo raportissa on varoitus kuntotietojen. Kun Suoritin pysyy kynnysarvo yli määritetyn ajan, se on näkyvissä virheeksi. Muussa tapauksessa reporter lähettää kuntotietojen OK.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Seuraavassa esimerkissä raportit lyhytkestoisia varoitus replikan. Se ensin saa osion tunnuksen ja replica ID-tunnus-palveluun, se on kiinnostunut. Se sitten lähettää raportin **PowershellWatcher** ominaisuutta **ResourceDependency**. Raportti on halutut vain kaksi minuuttia ja se poistetaan kaupasta automaattisesti.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>MUILLE KÄYTTÄJILLE
Lähetä muiden käyttäminen POST-pyyntöjä, siirry haluamaasi kohdetta ja terveyteen raportin kuvaus tekstissä on kuntoraportit. Esimerkiksi Katso, miten voit lähettää REST- [klusterin kuntoraportit](https://msdn.microsoft.com/library/azure/dn707640.aspx) tai [palvelun kuntoraportit](https://msdn.microsoft.com/library/azure/dn707640.aspx). Kaikki kohteet ovat tuettuja.

## <a name="next-steps"></a>Seuraavat vaiheet

Kunto-tietojen perusteella palvelun kirjoittajat ja klusterin Järjestelmänvalvojat ja ajatella tapoja tarjoaman tiedot. Esimerkiksi ne määrittää ilmoituksia kunnon tilan perusteella haluat suodattaa vakavia ongelmia, ennen kuin he vääristymiä katkokset. Järjestelmänvalvojat voit myös määrittää korjauksen systems korjaa ongelmat automaattisesti.

[Johdanto kangasta palvelun kunnon seuranta](service-fabric-health-introduction.md)

[Palvelun kangasta kuntoraporttien tarkasteleminen](service-fabric-view-entities-aggregated-health.md)

[Raportin ja tarkista palvelun kunto](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Käytä järjestelmän kuntoraportit vianmääritys](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Valvo ja vianmäärityksen paikallisesti palvelut](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Palvelun kangasta sovelluksen päivitys](service-fabric-application-upgrade.md)
