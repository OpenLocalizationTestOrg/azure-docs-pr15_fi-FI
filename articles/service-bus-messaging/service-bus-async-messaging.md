<properties 
    pageTitle="Palvelun Bus asynkronisen viestinvälityksen | Microsoft Azure"
    description="Palvelun Bus asynkronisen viestinvälityksen kuvaus."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" /> 
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm" />

# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynkroninen tekstiviesti kuvioiden ja suuri käytettävyys

Asynkroninen viestiliikenne voidaan toteuttaa usealla eri tavalla. Azure-palvelu Bus tukee olevien, aiheet ja tilaukset-asynchrony tallentaminen ja lähettäminen edelleen järjestelmä kautta. Normaalin (synkronoitu) käytön voit lähettää viestejä olevien ja aiheet ja vastaanottaa viestejä olevien ja tilaukset. Voit kirjoittaa sovellusten määräytyvät nämä objektit on aina käytettävissä. Kun kohde kuntotietojen muuttuu vuoksi erilaisia seuraavissa tilanteissa sinun on tapa antaa rajoitettu ominaisuuksien kohteen, jotka voivat täyttää useimmat tarpeisiin.

Sovellusten yleensä käytä asynkroninen tekstiviesti kuviot viestintä skenaarioiden määrä. Voit luoda sovelluksia, johon asiakkaiden lähettää viestejä palveluja, myös silloin, kun palvelua ei ole käynnissä. Sovellusten kyseisen kokemus tietoliikenne jonon bursts auttaa tason kuormituksen antamalla puskurin communications sijainti. Lopuksi saat yksinkertaisen mutta tehokkaan kuormituksen viestien jakelemiseen useita tietokoneissa.

Haluat säilyttää käytettävissä minkä tahansa nämä objektit, harkitse eri usealla eri tavalla, jossa nämä kohteet voi olla käytettävissä kestävät sähköpostijärjestelmän. Niinkään näemme kohteen poistuvat käytöstä sovelluksia on kirjoittaa eri seuraavasti:

- Ei voi lähettää viestejä.

- Ei voi vastaanottaa viestejä.

- Ei voi hallita kohteiden (luominen, hakea, Päivitä tai poista kohteita).

- Ota yhteyttä palvelun ei onnistu.

Kunkin nämä virheet-virheen eri tilat olemassa, jotka mahdollistavat sovelluksen Jatka tekemässä töitä rajoitettu vain joidenkin tasolla. Esimerkiksi järjestelmä, joka lähettää viestejä, mutta ne eivät saa edelleen vastaanottaa tilaukset asiakkaille, mutta ei voi käsitellä tilaukset. Tässä ohjeaiheessa kerrotaan, joita voi ilmetä ongelmia ja miten joiden lievity kyseiset ongelmat. Palvelun Bus on tietoa ongelman, joka on valita kyselyjä pienentämistavat määrä sekä tässä ohjeaiheessa kerrotaan myös näiden sisältyy ongelman pienentämistavat käyttöä koskevat säännöt.

## <a name="reliability-in-service-bus"></a>Palvelun Bus luotettavuutta

Viesti-ja yritys käsittelemään useilla tavoilla ja ei suuntaviivoja näiden ongelman pienentämistavat oikea käyttö. Selvittääksesi ohjeiden täytyy ensin ymmärtää, mitä voi epäonnistua palvelun Bus. Azure järjestelmien rakenne vuoksi kaikki nämä ongelmat yleensä voidaan lyhytkestoinen. Korkean tason siitä eri syistä näkyviin seuraavasti:

-   Rajoittimen ulkoisen järjestelmän josta palvelun Bus on riippuvainen. Säilytys- ja Laske resurssien aktiviteettien-rajoittimen tapahtuu.

-   Ongelma, josta palvelun Bus riippuvainen järjestelmän. Esimerkiksi tallennustilan tietyn osan voi ilmetä ongelmia.

-   Yksittäisen alirakenne-palvelun Bus virhe. Tässä tilanteessa Laske solmun pääsevät epäyhtenäinen vaiheeseen ja käynnistettävä itse aiheuttaa se toimii saldo solmujen ladata kaikki kohteet. Tämä puolestaan aiheuttaa hidas käsittely lyhyen ajan.

-   Palvelun Bus virhe Azure palvelinkeskuksen kuluessa. Tämä on "kriittinen epäonnistui" aikana järjestelmä ei ole käytettävissä useita minuutteja tai muutaman tunnin.

> [AZURE.NOTE] Termin **tallennustilan** tarkoitetaan Azuren tallennustilaan ja SQL Azure-tietokannassa.

Palvelun Bus sisältää useita ongelman pienentämistavat ongelmaan. Seuraavissa osissa käsitellään kunkin ongelman ja niiden vastaaviin ongelman pienentämistavat.

### <a name="throttling"></a>Rajoittaminen

Palvelun Bus rajoittimen ansiosta yhteistyötä viestin korko hallinta. Kunkin yksittäisen palvelun Bus solmun ovat suosikkiraporttisi useita kohteita. Kaikkien näiden yksiköiden tekee tarpeiden kannalta suorittimen, muistin, varasto ja muut rakenteita järjestelmään. Kun jokin näistä rakenteita havaitsee käyttö, joka ylittää määritetyn raja-arvot-palvelun Bus estää tietyn pyynnön. Soittajan vastaanottaa [ServerBusyException][] ja yrittää suorittaa uudelleen 10 sekunnin kuluttua.

Rajoituksen koodi on luku virhe ja Pysäytä kaikki uudelleenyritykset viestin vähintään 10 sekunnin ajan. Koska virhe voi tapahtua laitteita asiakas-sovelluksen kautta, odotetaan kutakin itsenäisesti käynnistää uudelleen logiikan. Koodin pienentää todennäköisyys ole rajoittanut ottamalla jakaminen jonossa tai aihe.

### <a name="issue-for-an-azure-dependency"></a>Ongelman Azure-riippuvuus

Muita osia Azure Joskus voi olla palveluongelmat. Esimerkiksi palvelun Bus käyttävän järjestelmän päivitetään, kun järjestelmän Voit tilapäisesti kohdata rajoitetun ominaisuuksia. Voit kiertää seuraavanlaisiin ongelmat palvelun Bus säännöllisesti tutkii ja toteuttaa ongelman pienentämistavat. Nämä ongelman pienentämistavat puoli vaikutukset näkyvät. Tallennustilan lyhytkestoisia ongelmat käsittelemään palvelun Bus toteuttaa järjestelmän, viestin lähetys toimintoja johdonmukaisesti toimisi. Vuoksi vähentäminen laatu lähetetty viesti voi kestää 15 minuuttia näkyvän tarvittavien jonossa tai tilaus ja voidaan ryhtyä vastaanota-toimintoa. Useimpien kohteiden ei yleensä ilmene ongelman. Kuitenkin valita kohteiden määrän palvelun Bus Azure sisällä, tämä rajoitus joskus tarvitaan vain pienen osan palvelun Bus asiakkaille.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Yksittäisen alirakenne palvelun Bus virhe

On kaikissa sovelluksissa tilanteissa voi aiheuttaa palvelun Bus epäyhtenäisiä sisäinen osana. Palvelun Bus havaitsee tämän, se kerää tietoja sovelluksesta-ohjelmistossa, mitä on tapahtunut tukeen. Kun tiedot on kerätty, sovellus käynnistetään yrittää palauttaa yhdenmukaiseen tilaan. Tämä toimenpide tapahtuu varsin nopeasti ja tulokset kohteen näkyvä ei ole käytettävissä, muutaman minuutin kuluttua ylöspäin, vaikka tyypillinen alaspäin ajat ovat paljon lyhentää.

Tällaisissa tapauksissa asiakassovellus Luo [System.TimeoutException][] tai [MessagingException][] poikkeuksen. Palvelun Bus sisältää rajoituksen ongelman automaattinen asiakkaan uudelleen logiikan muodossa. Kun uudelleen kauden lopussa ja viestiä ei toimitettu, voit selata muita ominaisuuksia, kuten [pisteparin nimitilan][]avulla. Pisteparin nimitilan on muita huomautukset, tässä artikkelissa käsitellään.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Palvelun Bus epäonnistumisesta sisällä Azure palvelinkeskukseen

Azure joten virheen Todennäköisin syy on epäonnistunut päivityksen asennettu palvelun Bus tai riippuvaiset järjestelmän. Kun ympäristö on kypsennetyn, tällaista virheen todennäköisyys on pienentää niiden kokoa. Palvelinkeskuksen virhe voi tapahtua myös syistä, jotka ovat seuraavat:

-   Sähköiset käyttökatkosta (power toimitus- ja luodaan power katoavat).
-   Connectivity (internet avaus asiakkaiden ja Azure välillä).

Kummassakin tapauksessa luonnollinen tai ihmisen huono aiheuttivat ongelman. Voit kiertää ongelman ja varmista, että voit lähettää viestejä edelleen, viestit lähetetään toiseen sijaintiin, kun ensisijainen sijainti on tehty kunnossa uudelleen käyttöön [pisteparin nimitilan][] . Lisätietoja on artikkelissa [turvallisuustarkoituksiin sovellusten vastaan palvelun Bus katkokset ja katastrofit parhaat käytännöt][].

## <a name="paired-namespaces"></a>Pisteparin nimitilan

[Pisteparin nimitilan][] -toiminto tukee skenaariot joka palvelun Bus kohteen tai käyttöönoton tietojen keskuksen ei ole käytettävissä. Kun tapahtuman ilmetessä harvoin hajautettu järjestelmien edelleen on valmisteltava käsittelemään Huonoin tapaus skenaarioita. Tapahtuman tapahtuu yleensä, koska joitakin elementti, josta palvelun Bus riippuvainen ilmenee lyhytkestoinen ongelma. Jos haluat säilyttää sovelluksen käytettävyys käyttökatkosta aikana, palvelun Bus käyttäjät voivat käyttää kaksi erillistä nimitilan mieluiten erillinen tietojen keskikohdan mukaan-isännöimiseen tekstiviesti niiden kohteiden. Tässä osassa loput käyttää seuraavia termejä:

-   Ensisijainen nimitilan: nimitilan, jonka sovelluksesi toimii Send ja vastaanottaa toimintoja.

-   Toissijainen nimitilan: nimitilan, joka toimii ensisijainen nimitilan varmuuskopiointi. Sovelluksen logiikkaa Käsittele tätä nimitilaa.

-   Automaattisesti väli: Hyväksy Normaali virheet, ennen kuin sovellus vaihtaa ensisijainen nimitilan toissijainen nimitilan ajan.

Parittainen nimitilan tuki *käytettävyyden lähettäminen*. Lähetä käytettävyys säilyttää voi lähettää viestejä. Lähetä käytettävyys käyttämään sovelluksesi on täytettävä seuraavat vaatimukset:

1.  Viestejä vastaanotetaan vain ensisijainen nimitilan.

2.  Tietyn jonon lähetettyjä viestejä tai aiheen ehkä saapuvat väärässä järjestyksessä.

3.  Jos sovellus käyttää istunnot, istunnon sanomat ehkä saapuvat väärässä järjestyksessä. Tämä on tauko-istunnot Normaali toimintoja. Tämä tarkoittaa, että sovelluksesi käyttää istuntojen loogisesti ryhmän viesteihin. Istunnon tila määritetään vain ensisijainen nimitilan.

4.  Istunnon sanomat ehkä saapuvat väärässä järjestyksessä. Tämä on tauko-istunnot Normaali toimintoja. Tämä tarkoittaa, että sovelluksesi käyttää istuntojen loogisesti ryhmän viesteihin.

5.  Istunnon tila määritetään vain ensisijainen nimitilan.

6.  Ensisijainen jonossa voit tulee online-tilaan ja alkaa Hyväksy sähköpostiviestejä, ennen kuin toissijainen jonossa toimittaa kaikki viestit ensisijainen jonossa kyselyjä.

Seuraavissa osissa käsitellään API, miten API on otettu käyttöön ja näyttää otoksen koodin, joka käyttää ominaisuutta. Huomaa, että on tähän ominaisuuteen liittyvät laskutuksen vaikutukset.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>MessagingFactory.PairNamespaceAsync API

Pisteparin nimitilan-ominaisuus sisältää [Microsoft.ServiceBus.Messaging.MessagingFactory][] -luokan [PairNamespaceAsync][] -menetelmää:

```
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Kun tehtävä on valmis, nimitilan laiteparin on myös valmis perusteella toimimiseen, minkä tahansa [MessageReceiver][], [QueueClient][]tai [TopicClient][] , joka on luotu käyttämällä [MessagingFactory][] esiintymä. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions][] on erityyppisiä laiteparin, jotka ovat käytettävissä [MessagingFactory][] -objektia, perus-luokka. Tällä hetkellä vain johdetun luokan on yksi nimetty [SendAvailabilityPairedNamespaceOptions][], joka toteuttaa Lähetä käytettävyys vaatimukset. [SendAvailabilityPairedNamespaceOptions][] on konstruktorien, jotka perustuvat toisiinsa. Katsoo konstruktoria useimmat parametreilla, voivat ymmärtää muiden konstruktoreja toimintaa.

```
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Nämä parametrit on seuraavia asioita:

-   *secondaryNamespaceManager*: toissijainen nimitilan, [PairNamespaceAsync][] menetelmän avulla voit määrittää toissijaisen nimitilan valmisteltu [NamespaceManager][] esiintymä. Nimitilan hallinnan avulla nimitilan olevien luettelo hankkiminen ja varmista, että tarvittavat keskeneräisen olevien olemassa. Jos nämä olevien ei ole, suhteet on luotu. [NamespaceManager][] edellyttää mahdollisuus luoda tunnuksen **hallinta** -ryhmän kanssa.

-   *messagingFactory*: toissijainen nimitilan [MessagingFactory][] esiintymän. Voit lähettää ja vastaanottaa [EnableSyphon][] -ominaisuudeksi on määritetty **Tosi**, jos viestejä keskeneräisen olevien käytetään [MessagingFactory][] objekti.

-   *backlogQueueCount*: keskeneräisen olevien luominen määrä. Tämän arvon on oltava vähintään 1. Jos lähetät viestin keskeneräisen, satunnaisesti jokin näistä olevien valitaan. Jos määrität arvon 1, koskaan vain yksi jonon voidaan. Kun näin tapahtuu ja yksi keskeneräisen jonossa Luo virheitä, asiakkaan on ei pysty Kokeile eri keskeneräisen jono ja Lähetä viesti saattaa epäonnistua. Suosittelemme, että tämä arvo on joitakin suurempi arvo ja oletusarvon arvon 10. Voit muuttaa sen mukaan, kuinka paljon tietoja sovelluksesi lähettää päivässä ylemmäs tai alemmas arvon. Keskeneräisen kunkin jonon voivat sisältää enintään 5 gt viestejä.

-   *failoverInterval*: jolta hyväksyt virheet, valitse ensisijainen nimitilan ennen siirtyminen minkä tahansa yhden kohteen toissijainen nimitilan ajan. Failovers ilmetä kohteen kohteen perusteella. Yksittäisen nimitilan kohteita tallennetaan usein eri palvelun Bus sijaitsevien solmujen. Yhden kohteen epäonnistui ei tarkoita toisessa epäonnistui. Voit määrittää tämän arvon [System.TimeSpan.Zero][] , voit siirtää toissijaisen heti, kun ensimmäinen, muu kuin lyhytaikainen virhe. Virheet, joka käynnistää automaattisesti ajastin ovat kaikki [MessagingException][] jossa [IsTransient][] -ominaisuus on EPÄTOSI tai [System.TimeoutException][]. Muut poikkeukset, kuten [UnauthorizedAccessException][] eivät aiheuta automaattisesti, koska ne osoittavat, että asiakas on määritetty väärin. [ServerBusyException][] aiheuta automaattisesti koska oikea kaava on odottamaan 10 sekuntia, Lähetä viesti uudelleen.

-   *enableSyphon*: ilmaisee, että tietty laiteparin pitäisi myös syphon toissijainen nimitilan takaisin ensisijainen nimitilan sähköpostiviestejä. Yleinen-sovelluksia, jotka lähettävät viestit tulee Aseta arvoksi **false**; sovellukset, joka vastaanottaa viestejä tulee Aseta arvoksi **True**. Syy, on usein vähemmän kuin lähettäjiä viestin vastaanottajia. Vastaanottajia määrän mukaan voit käsitellä syphon tulleista yhden sovelluksen-esiintymä on. Käyttämällä useita vastaanottajia on laskutuksen vaikutuksia kunkin keskeneräisen jonon.

Käytettävä koodi ensisijainen [MessagingFactory][] esiintymän, toissijaisen [MessagingFactory][] esiintymän, toissijaisen [NamespaceManager][] esiintymän ja [SendAvailabilityPairedNamespaceOptions][] esiintymän luominen. Puhelun voi olla pelkästään seuraavasti:

```
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Kun [PairNamespaceAsync][] menetelmän palauttama tehtävä on valmis, kaikki tiedot on määritetty ja käyttövalmis. Ennen kuin tehtävä on palautettu, voivat ei olet tehnyt kaikki tarvittavat laiteparin toimimaan oikealle tausta-työstä. Et olisi seurauksena käynnistää viestien lähettäminen, kunnes tehtävän palauttaa. Jos tuontiprosessin, kuten virheelliset tunnistetiedot tai keskeneräisen, olevien luominen näiden poikkeusten voidaan ilmenee, kun tehtävä on valmis. Kun tehtävän palauttaa, varmista, että olevien on löydy tai luotu tarkastelemalla [SendAvailabilityPairedNamespaceOptions][] -esiintymässä [BacklogQueueCount][] -ominaisuus. Yllä olevaa koodia toiminnon näkyy seuraavasti:

```
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut perusteet asynkronisen viestinvälityksen palvelun Bus lukea lisätietoja [pisteparin nimitilan][].

  [ServerBusyException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.serverbusyexception.aspx
  [System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
  [MessagingException]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.aspx
  [Parhaat käytännöt lämmittämistä sovellusten vastaan palvelun Bus katkokset ja katastrofit]: service-bus-outages-disasters.md
  [Microsoft.ServiceBus.Messaging.MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [MessageReceiver]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagereceiver.aspx
  [QueueClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx
  [TopicClient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx
  [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.pairednamespaceoptions.aspx
  [MessagingFactory]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.aspx
  [SendAvailabilityPairedNamespaceOptions]:https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.aspx
  [NamespaceManager]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx
  [PairNamespaceAsync]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactory.pairnamespaceasync.aspx
  [EnableSyphon]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.enablesyphon.aspx
  [System.TimeSpan.Zero]: https://msdn.microsoft.com/library/azure/system.timespan.zero.aspx
  [IsTransient]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingexception.istransient.aspx
  [UnauthorizedAccessException]: https://msdn.microsoft.com/library/azure/system.unauthorizedaccessexception.aspx
  [BacklogQueueCount]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions.backlogqueuecount.aspx
  [pisteparin nimitilan]: service-bus-paired-namespaces.md