<properties 
    pageTitle="Azure olevien ja palvelun Bus jonot - vertailu ja asiaan | Microsoft Azure"
    description="Analysoi erot ja yhtäläisyydet kahdentyyppisiä olevien Azure tarjoamia."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure olevien ja palvelun Bus jonot - vertailu ja asiaan

Tässä artikkelissa analysoi erot ja yhtäläisyydet olevien tarjoamia Microsoft Azure tänään kahdentyyppisiä: Azure olevien ja palvelun Bus olevien. Näiden tietojen avulla voit verrata ja kontrasti vastaaviin tekniikoita ja voi tehdä siitä päätös tietoja ratkaisun tarpeitasi parhaiten vastaava.

## <a name="introduction"></a>Johdanto

Microsoft Azure tukee kahdentyyppisiä jonon järjestelmiä: **Azure olevien** ja **Palvelun Bus olevien**.

**Azure olevien**, jotka ovat [Azure storage](https://azure.microsoft.com/services/storage/) -infrastruktuurin, ominaisuuksien yksinkertainen REST-pohjainen Get/hyllytetty/Pikanäkymä-liittymän antamisen luotettavia, pysyvä viestintä ja niiden palveluiden välillä.

**Palvelun Bus olevien** kuuluvat laajempi [Azure messaging](https://azure.microsoft.com/services/service-bus/) infrastruktuuri, joka tukee queuing sekä Julkaise ja tilaa, Web-palvelu Etäpalvelujen ja integrointi kuviot. Saat lisätietoja palvelun Bus olevien, aiheet ja tilaukset ja releitä [palvelun Bus viestiliikenteen yleiskuvaus](service-bus-messaging-overview.md).

Vaikka molemmat queuing tekniikat olemassa samanaikaisesti, Azure olevien otettiin ensin kuin oma jonon tallennustilan järjestelmä laadittuihin päälle Azure liittyviä palveluja. Palvelun Bus olevien on suunniteltu päälle laajempi "se messaging" infrastruktuuri suunniteltu integroida sovelluksia tai sovelluksen osia, jotka saattavat olla useita viestintäprotokollia tietojen sopimuksia luota toimialueet ja/tai verkko-ympäristössä.

Tässä artikkelissa verrataan kaksi jonon tekniikoita tarjoamia Azure mukaan keskustella toiminta ja soveltaminen kunkin tarjoamien ominaisuuksien eroihin. Tässä artikkelissa on myös ohjeet voi valita, mitä ominaisuuksia voi parhaiten sopivat sovellusten kehittämisen tarpeitasi.

## <a name="technology-selection-considerations"></a>Tekniikka valinnan huomioon otettavia seikkoja

Azure olevien ja palvelun Bus olevien ovat tällä hetkellä yhteyksiä Microsoft Azure-sanomajonojärjestelmä käyttöotot. Jokaisella on hieman erilainen ominaisuusjoukko, mikä tarkoittaa sitä, voit valita toisen tai käytä molempia tietyn ratkaisun tai ovat ratkaiseminen business/tekniset ongelma tarpeiden mukaan.

Mitä queuing tekniikkaa sopii annetun ratkaisun käyttötarkoitukseen määritettäessä kehittäjät että ratkaisu architects huomioon seuraavia suosituksia. Lisätietoja on artikkelissa seuraavaan osaan.

Ratkaisu architect/kehittäjän, **kannattaa harkita Azure olevien** kun:

- Sovellus on tallennettava yli 80 Gigatavua sanomia jonossa, joihin viestit on alle seitsemän päivää elinaika.

- Sovelluksen haluaa käsittelyyn sisältyy jonossa viestin edistymisen seuraaminen. Tämä on kätevä, jos viestin käsittelyn työntekijä kaatuu. Myöhemmin työntekijä, voit poistaa mihin edellisen työntekijä jäi, tee tiedot avulla.

- Tarvitset palvelimen lokitietoihin kaikkien oman olevien kohdistaa tapahtumat.

Ratkaisu architect/kehittäjän, **kannattaa harkita palvelun Bus olevien** kun:

- Ratkaisu on voitava muodostaa vastaanottaa viestejä tarvitsematta lisääminen jonossa. Palvelun Bus kanssa tämä onnistuu käyttämällä long-kysely vastaanottaa toiminto, joka tukee palvelun Bus TCP-pohjaisen-protokollia käyttäen.

- Ratkaisu edellyttää jonossa antamaan taattua ensimmäisen-in-first-out (FIFO) vyöhykkeen toimitusta.

- Haluat symmetrisen kokemus Azure- ja Windows Server (yksityinen cloud). Lisätietoja on artikkelissa [Palvelun Bus Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).

- Ratkaisu on voitava muodostaa tukevat automaattisen kaksoiskappaleiden.

- Haluat käsitellä viestejä sovellusta kuin rinnakkain pitkään suoritettavien virtaa (viestit on yhdistetty muodossa, [istunto](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -ominaisuuden käyttäminen viesti). Tämän mallin kunkin solmun vievää sovelluksen joiden kilpailee virtaa viestien sijaan. Kun stream esitetään vievää solmu-solmun voit tutkia sovelluksen stream tilan käyttäminen tapahtumien tila.

- Ratkaisu edellyttää tapahtumien toiminta ja atomisuuden lähettämisessä ja vastaanottamisessa useita viestejä-jonossa.

- Time-to-live (TTL)-ominaisuus sovelluksen kielikohtaiset kuormituksen voi ylittää 7 päivän.

- Sovelluksen käsittelee viestit, jotka voivat enintään 64 Kilotavun, mutta ei todennäköisesti lähestymistapa 256 kt rajaa.

- Voit käsitellä pyyntö antaa lähettäjien ja vastaanottajien Roolipohjainen käytön mallin olevien ja eri oikeudet ja käyttöoikeudet.

- Jonon enimmäiskoko kasvaa yli 80 Gigatavua.

- Haluat käyttää AMQP 1.0 standardien-pohjaisten tekstiviesti-protokollaa. Saat lisätietoja AMQP [Bus AMQP yleistä](./service-bus-amqp-overview.md).

- Voit kuvitella potentiaalisen siirto-jonossa perustuva pisteestä viestintä viestin exchange-kaava, joka mahdollistaa saumattomasti muita vastaanottajia (tilaajat), kukin ottavat kopio riippumaton jonossa lähetetään joko joistakin tai kaikista viesteistä. Jälkimmäinen viittaa grafiikkatiedostomuotoja myöntämä palvelun Bus n Julkaise ja tilaa-toimintoa.

- Tekstiviesti ratkaisu on voitava muodostaa "Osoitteessa useimmat-kerran" toimituksen takuun ilman, että voit luoda lisäinfrastruktuuri-osat tukevat.

- Haluat julkaista ja tarjoaman erissä viestejä.

- Tarvitset .NET Framework Windows Communication Foundation (WCF) viestintä-pino koko integrointi.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Azure olevien ja palvelun Bus olevien vertailu

Seuraavissa taulukoissa on looginen ryhmittely jonon ominaisuuksia ja avulla voit verrata yhdellä silmäyksellä, toiminnot, jotka ovat käytettävissä olevien Azure ja palvelun Bus olevien.

## <a name="foundational-capabilities"></a>Foundational ominaisuudet

Tässä osassa on Vertailtu Azure olevien ja palvelun Bus olevien myöntämä queuing keskeisiä ominaisuuksia.

|Vertailuehdot|Azure olevien|Palvelun Bus olevien|
|---|---|---|
|Takuun järjestys|**Ei** <br/><br>Lisätietoja on artikkelissa ensimmäisen muistiinpanon "Lisätietoja"-osassa.</br>|**Kyllä - ensimmäinen-In-First-Out (FIFO)**<br/><br>(käyttämällä messaging istunnot)|
|Toimituksen takuu|**Milloin ainakin kerran-**|**Milloin ainakin kerran-**<br/><br/>**Yhdellä useimmat-kertaa**|
|Atomisia toiminnon tuki|**Ei**|**Kyllä**<br/><br/>|
|Saat toiminta|**Muut estäminen**<br/><br/>(on valmis heti Jos ei ole uusi viesti ei löydy)|**Ylätunnisteosan kanssa tai ilman aikakatkaisun estäminen**<br/><br/>(tarjouksia pitkä kysely tai ["Komeetta tekniikka"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Muut estäminen**<br/><br/>(käyttämällä .NET hallittu Ohjelmointirajapinta vain)|
|Push-tyyppiseksi Ohjelmointirajapinta|**Ei**|**Kyllä**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) ja **OnMessage** istuntojen .NET API.|
|Saat tila|**Valvotun & vuokrata**|**Valvotun ja lukitseminen**<br/><br/>**Saat ja poistaminen**|
|Yksityisen käyttöoikeuden tila|**Varaus-pohjainen**|**Lukitse-pohjainen**|
|Varaus/Lukitse kesto|**30 sekuntia (oletus)**<br/><br/>**7 päivää (suurin)** (Voit uusia tai vapauttaa [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) Ohjelmointirajapinnan käyttäminen viestin varaus.)|**60 sekuntia (oletus)**<br/><br/>Voit uusia tilauksen käyttämällä [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API viestin lukitusta.|
|Varaus/Lukitse tarkkuus|**Viestin taso**<br/><br/>(viesti voi olla eri aikakatkaisuarvo, joka voi päivittää tarvittaessa sitten käsiteltäessä viestin käyttämällä [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API)|**Jonon taso**<br/><br/>(jokaisessa jonossa on käytetty kaikissa sen viestien Lukitse tarkkuuden, mutta voit uusia tilauksen käyttämällä [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API Lukitse.)|
|Erämuotoinen vastaanottaminen|**Kyllä**<br/><br/>(eksplisiittisesti määrittämällä viestin määrä noudettaessa viestit, enintään 32 viestit)|**Kyllä**<br/><br/>(käyttöönottoon implisiittisesti vanhat Nouda ominaisuus tai erikseen tapahtumien)|
|Oletusmäärä lähetys|**Ei**|**Kyllä**<br/><br/>(käyttämällä tapahtumia tai asiakkaan jonottaminen)|

### <a name="additional-information"></a>Lisätietoja

- Azure olevien viestit ovat yleensä ensimmäisen-in-first-out, mutta joskus ne voivat olla väärässä järjestyksessä; esimerkiksi kun viestin näkyvyyden aikakatkaisun kesto vanhenee (esimerkiksi tuloksena kaatuu käsiteltäessä asiakassovellus). Kun näkyvyyden aikakatkaisu vanhenee, sanoma tulee näkyviin uudelleen käyttöön toisen työntekijän jonosta sen poistamisen jonossa. Tässä vaiheessa juuri näkyvät viestin saatetaan jonossa (jotta poistettu voidaan jonosta uudelleen) jälkeen tulee sanoma, jossa on alun perin enqueued sen jälkeen.

- Jos käytössäsi on jo tallennustilan Azure-BLOB- tai taulukoita ja voit aloittaa olevien, ovat taata 99,9 % käytettävyyttä. Jos käytät palvelun Bus olevien BLOB- tai taulukoita, sinun on pienempi käytettävyys.

- Taattua FIFO kuviota, palvelun Bus olevien vaatii tekstiviesti istuntoon. Silloin, kun sovellus kaatuu käsiteltäessä **Pikanäkymä & Lukitse** -tilassa vastaanotettu viesti, jonossa vastaanottaja hyväksyy tekstiviesti-istunnon seuraavan kerran se käynnistyy epäonnistuneen viestin sen--elinaika (TTL)-piste päätyttyä.

- Azure olevien on suunniteltu tue vakio queuing vaihtoehtoja, kuten irtautus sovelluksen osia niin, että skaalattavuus ja virheet, poikkeama ladata prosessin työnkulkujen kehittämistä sekä tasaamista.

- Palvelun Bus olevien tukevat *Osoitteessa ainakin kerran -* toimituksen takuu. Lisäksi *Osoitteessa useimmat-kerran* semanttinen aiotaan lisätä istunnon tilan avulla voit tallentaa sovelluksen tilan ja tapahtumien avulla atomically vastaanottavat viestejä ja Päivitä istunnon tila.

- Azure olevien antaa yhdenmukaisia ja täsmällisiä ohjelmoinnin mallin olevien, taulukoita ja BLOB – sekä kehittäjät toimintojen ryhmiä.

- Palvelun Bus olevien tukevat paikalliset tapahtumat yhden jonon kontekstissa.

- Palvelun Bus tukemat **vastaanottaminen ja poista** tila on voi pienentää tekstiviesti toiminnon sanamäärän (ja liittyviä kustannuksia) laskettu toimituksen assurance vastaan.

- Azure olevien antaa vuokrasopimukset laajentaa viestien vuokrasopimukset. Näin työntekijät voivat ylläpitää lyhyt vuokrasopimukset viesteihin. Näin, jos työntekijä kaatuu, viesti voi nopeasti käsitellä uudelleen toiselle työntekijälle. Lisäksi työntekijä voi laajentaa varauksen viestissä, jos se on käyttänyt sitä yli varauksen nykyisen kellonajan.

- Azure olevien tarjouksen näkyvyyden aikakatkaisu, voit määrittää enqueueing yhteydessä tai dequeuing viestin. Voit lisäksi Päivitä viestin eri varauksen arvoilla suorituksen aikana ja eri arvoja päivittyä samassa jonossa olevat viestit. Palvelun Bus Lukitse aikakatkaisu määritetään jonon; metatiedot Voit kuitenkin [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) kutsumista uusia lukitus.

- Suurin aikakatkaisu estäminen vastaanotto toiminnon palvelun Bus olevien on 24 päivää. REST-pohjainen aikakatkaisu on kuitenkin 55 sekunteina suurimman arvon.

- Asiakkaan jonottaminen palvelun Bus myöntämä mahdollistaa jonon asiakkaan erän useiden viestien siirtäminen yhden lähetystoiminto. Jonottaminen on käytettävissä vain asynkroninen lähetys toimintoja.

- Ominaisuudet, kuten 200 TT enimmäismäärää Azure olevien (lisätietoja, kun tilit virtualisoi) ja rajoittamaton olevien helpompaa ihanteellinen ympäristössä SaaS palveluntarjoajien osalta.

- Azure olevien Anna luo joustavan ja performant valtuutetun access ohjausobjektin järjestelmä.

## <a name="advanced-capabilities"></a>Lisäominaisuuksia

Tässä osassa vertaa Azure olevien ja palvelun Bus olevien myöntämä lisäominaisuuksia.

|Vertailuehdot|Azure olevien|Palvelun Bus olevien|
|---|---|---|
|Ajoitetussa|**Kyllä**|**Kyllä**|
|Automaattinen toimimattoman sisältää|**Ei**|**Kyllä**|
|Kasvava jonon aika-arvo|**Kyllä**<br/><br/>(joko näkyvyyden aikakatkaisu käytönaikainen-päivitys)|**Kyllä**<br/><br/>(saatua erillinen API-funktio)|
|Poison message tuki|**Kyllä**|**Kyllä**|
|Paikallaan tapahtuva päivitys|**Kyllä**|**Kyllä**|
|Palvelinpuolen tapahtumaloki|**Kyllä**|**Ei**|
|Tallennustilan arvot|**Kyllä**<br/><br/>**Minuutti mittarit**: tarjoaa reaaliaikainen mittaukset käytettävyyden TP merkinnät-Ohjelmointirajapinnan kutsu laskelmia, virhe määrät ja useampaa reaaliaikaisesti (koostetun minuutissa ja raportoi-tuotannon vain tapahtumista muutaman minuutin kuluessa. Lisätietoja on artikkelissa [Tietoja tallennustilan Analytics arvot](https://msdn.microsoft.com/library/azure/hh343258.aspx).|**Kyllä**<br/><br/>(kyselyjen joukkona soittamalla [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx))|
|Tilan hallinta|**Ei**|**Kyllä**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx) [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Viestien automaattinen-välittäminen|**Ei**|**Kyllä**|
|Tyhjennä jono-funktio|**Kyllä**|**Ei**|
|Viestin ryhmät|**Ei**|**Kyllä**<br/><br/>(käyttämällä messaging istunnot)|
|Sovelluksen tila viestin ryhmittäin|**Ei**|**Kyllä**|
|Kaksoiskappaleiden|**Ei**|**Kyllä**<br/><br/>(määritettäviä lähettäjän reunassa)|
|WCF-integrointi|**Ei**|**Kyllä**<br/><br/>(tarjoaa ulos,-valmiilla WCF sidontojen)|
|Windowsin Palomuuri-integrointi|**Mukautettu**<br/><br/>(edellyttää mukautetun Windowsin Palomuuri-aktiviteetin luominen)|**Alkuperäisen**<br/><br/>(tarjoaa ulos,-valmiilla Windowsin Palomuuri toimintoja)|
|Viestin ryhmien selaaminen|**Ei**|**Kyllä**|
|Viestin istuntojen haetaan tunnuksen mukaan|**Ei**|**Kyllä**|

### <a name="additional-information"></a>Lisätietoja

- Molemmat queuing tekniikat käyttöön viesti voidaan ajoittaa toimittamista myöhemmin.

- Jonon automaattinen edelleenlähetystä mahdollistaa tuhansia olevien automaattinen Välitä sähköpostinsa yksittäisen jono, josta vastaanottaminen siinä käytetään viestin. Voit käyttää se saavuttamiseksi suojaus, hallita työnkulku ja eristää tallennustilan välillä jokaisen viestin publisher.

- Azure olevien tukevat viestisisältö päivitetään. Voit käyttää tätä toimintoa pysyvä tilatietoja ja lisäävät edistymispäivityksiä viestiin niin, että se voidaan käsitellä-viimeisen tunnetut tarkistuspiste sen sijaan, että aloitat tyhjästä. Palvelun Bus olevien voit ottaa saman skenaarion viestin istuntojen käyttämällä. Istunnot, joiden avulla voit tallentaa ja hakea sovelluksen käsittely-tila (käyttämällä [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) ja [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)).

- [Toimimattoman kirjain](service-bus-dead-letter-queues.md), jota tuetaan vain palvelun Bus olevien mukaan, voi olla hyötyä päätiedostoon viestit, joita ei voi käsitellä onnistuneesti vastaanottava sovellus tai viestit eivät pääse niiden kohde on vanhentunut--elinaika (TTL) ominaisuus vuoksi. TTL-arvoa määrittää, kuinka kauan viesti säilyy jonossa. Palvelun Bus kanssa viesti siirretään kutsutaan $DeadLetterQueue, kun TTL on kulunut määräten jonossa.

- "Torjuttujen" viestien etsiminen Azure olevien, kun viestin dequeuing sovelluksen tarkistaa viestin **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** -ominaisuus. Jos **DequeueCount** on tietyn kynnysarvo, sovellus siirtää viestin sovelluksen määrittämät "perille kirjain"-jonossa.

- Azure olevien avulla voit hankkia yksityiskohtainen loki kaikki tapahtumat, kohdistaa jonossa sellaisena kuin koostettu arvot. Nämä asetukset ovat käteviä virheenkorjaus ja tietoja siitä, miten sovellus käyttää Azure olevien. He ovat myös hyötyä suorituskyvyn säätö sovelluksesi ja vähentää olevien aiheutuneet.

- "Sanoma"tukema istuntojen palvelun Bus käsite mahdollistaa viestit, jotka kuuluvat tiettyjen looginen ryhmään liitettävän annetun vastaanotin, joka puolestaan luo istunnon kaltaisessa affiniteetti, viestit ja niiden vastaaviin vastaanottajia välillä. Voit ottaa tämän palvelun Bus lisäominaisuuksia määrittämällä [istunto](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) -ominaisuuden viestissä. Vastaanottajia voi sitten kuuntelemaan tiettyä istunnon-tunnus ja vastaanottaa viestejä, joilla on määritetty istunnon tunnisteen.

- Kaksinkertaisen tunnistus toimintoja tukemat palvelun Bus olevien automaattisesti poistaa kaksoiskappaleita ohjeaiheessa [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) -ominaisuuden arvon perusteella tai jonon lähetettyihin viesteihin.

## <a name="capacity-and-quotas"></a>Kapasiteetti ja kiintiöitä

Tässä osassa vertaa Azure olevien ja palvelun Bus olevien [kapasiteetin ja kiintiöt](service-bus-quotas.md) , jotka voivat käyttää näkökulmasta.

|Vertailuehdot|Azure olevien|Palvelun Bus olevien|
|---|---|---|
|Jonon enimmäiskoko|**200 TT**<br/><br/>(rajoitettu yksittäisen tallennustilaa tili)|**1 gt 80 gigatavua**<br/><br/>(määritetty luotaessa jonossa ja [ottaminen käyttöön jakaminen](service-bus-partitioning.md) – Katso "Lisätietoja"-osio)|
|Viestin enimmäiskoko|**64 KILOTAVUA**<br/><br/>(48 kt, kun käytät **Base64** koodaus)<br/><br/>Azure tukee suuria viestejä yhdistämällä olevien ja BLOB –, jolloin voit enqueue enintään 200 gt yksittäisen kohteen.|**256 Kilotavua** ja **1 Megatavu**<br/><br/>(mukaan lukien ylä- ja suurin otsikon koko leipätekstiin: 64 Kilotavua).<br/><br/>Määräytyy [palvelutaso](service-bus-premium-messaging.md).|
|Viestin TTL|**7 päivää**|**`TimeSpan.Max`**|
|Olevien enimmäismäärä|**Rajoittamaton tallennus**|**10 000**<br/><br/>(kohti palvelun nimitila voidaan lisätä)|
|Samanaikainen asiakkaiden enimmäismäärä|**Rajoittamaton tallennus**|**Rajoittamaton tallennus**<br/><br/>(100 samanaikainen yhteyden rajoitus koskee vain TCP-protokolla-pohjainen viestintä)|

### <a name="additional-information"></a>Lisätietoja

- Palvelun Bus pakottaa jonon kokorajoitukset. Jonon enimmäiskoko on määritetty luotaessa jonon, ja se voi olla arvo väliltä 1 ja 80 Gigatavua. Jos jonon kokoa-kohdan arvoksi luominen jonon saavutetaan, lisää saapuvien viestien hylätään ja poikkeuksen vastaanotetaan puheluja koodilla. Saat lisätietoja palvelun Bus kiintiön [Palvelun Bus kiintiön](service-bus-quotas.md).

- Voit luoda palvelun Bus olevien 1, 2, 3, 4 ja 5 gt koot (oletusarvo on 1 gt). Kanssa jakaminen käytössä (joka on myös oletus), palvelun Bus Luo 16 osioiden kunkin määrittämäsi Gigatavua. Sellaisenaan, jos olet luonut jono, joka on 5 gt kokoisia, 16 osiot ja jonon enimmäiskoko ei ole (5 * 16) = 80 Gigatavua. Voit tarkastella osioitua jonossa tai aiheen enimmäiskokoa katsoo sen [Azure portal][].

- Azure olevien Jos viestin sisältö ei ole XML-turvallisesti ja sitten sen on oltava **Base64** koodattu. Jos olet **Base64**-koodata viestin, käyttäjän tiedot voi olla enintään 48 kt, sen sijaan, että 64 Kilotavua.

- Palvelun Bus olevien kanssa jokaisen viestin tallennettu jonossa koostuu kahdesta osasta: ylä- ja leipäteksti on. Viestin kokonaiskoko eivät saa ylittää tason palvelun tukemat sanomien enimmäiskoon.

- Kun asiakkaat yhteydenpito palvelun Bus olevien TCP-protokollan kautta, samanaikaisia yhteyksiä yksittäisen palvelun Bus jonon enimmäismäärä on rajoitettu 100. Tämä luku jaetaan lähettäjien ja vastaanottajien välillä. Jos tämän kiintiön on saavutettu, Lisää yhteyksien seuraavien pyyntöjen hylätään ja poikkeuksen vastaanotetaan puheluja koodilla. Tämä rajoitus ei määrätä asiakassovelluksissa REST-pohjainen Ohjelmointirajapinnan käyttäminen olevien muodostamisesta.

- Jos asetat yli 10 000 olevien yksittäisen palvelun Bus nimitilan, voit Azure tukeen ja pyytää kasvaa. Skaalata yli 10 000 olevien palvelun Bus kanssa, voit myös luoda uusia nimitilan [Azure portal][].

## <a name="management-and-operations"></a>Hallinta ja toiminnot

Tässä osassa vertaa Azure olevien ja palvelun Bus olevien myöntämä hallintaominaisuudet.

|Vertailuehdot|Azure olevien|Palvelun Bus olevien|
|---|---|---|
|Protokolla|**HTTP/HTTPS KOHDALLE**|**HTTPS KOHDALLE**|
|Runtime-protokolla|**HTTP/HTTPS KOHDALLE**|**HTTPS KOHDALLE**<br/><br/>**AMQP 1.0 Standard (TCP-ja TLS)**|
|.NET hallitun Ohjelmointirajapinnan|**Kyllä**<br/><br/>(.NET hallitun tallennustilan Client API)|**Kyllä**<br/><br/>(.NET hallitun se tekstiviesti API)|
|Alkuperäisen C++|**Kyllä**|**Ei**|
|Java-Ohjelmointirajapinta|**Kyllä**|**Kyllä**|
|PHP OHJELMOINTIRAJAPINTA|**Kyllä**|**Kyllä**|
|Node.js API|**Kyllä**|**Kyllä**|
|Haluamaansa metatietojen tuki|**Kyllä**|**Ei**|
|Jonon nimeämiskäytäntö säännöt|**Enintään 63 merkkiä**<br/><br/>(Kirjaimet jonon nimi on kirjoitettava pienellä.)|**Enintään 260 merkkiä**<br/><br/>(Jonossa polkujen ja nimet ovat asennustavan.)|
|Hae jonon pituuden-funktio|**Kyllä**<br/><br/>(Likiarvo Jos viestit vanhenevat lisäksi TTL ilman poistamista.)|**Kyllä**<br/><br/>(Tarkat ja ajankohta arvot.)|
|Pikanäkymä-funktio|**Kyllä**|**Kyllä**|

### <a name="additional-information"></a>Lisätietoja

- Azure olevien tukevat satunnaisia määritteitä, jotka voidaan lisätä jonon kuvaus, nimen ja arvon tietoparin muodossa.

- Molemmat jonon tekniikat tarjouksen voit vilkaista viestin eikä sinun tarvitse lukitsemalla sen, joka voi olla hyötyä käyttöönoton jonon explorer-selaimen-työkalu.

- Palvelun Bus .NET se tekstiviesti ohjelmointirajapinnan suorituskykykertoimen kaksisuuntainen TCP-yhteyksien parannettu suorituskyky verrattuna muiden HTTP ja niiden tukemista AMQP 1.0 vakio-protokollaa.

- Azure olevien nimet voivat olla 3 63 merkkiä pitkä, voi olla pieniä kirjaimia, numeroita ja väliviivoja. Lisätietoja on artikkelissa [olevien nimeämisestä ja metatiedot](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Palvelun Bus jonon nimet voidaan 260 merkkiä ylöspäin ja joustavampi nimeämiskäytäntö säännöt. Palvelun Bus jonon nimet voivat sisältää kirjaimia, numeroita, jaksot, väliviivoja ja alaviivoja.

## <a name="authentication-and-authorization"></a>Todennus- ja

Tässä osassa käsitellään Azure olevien ja palvelun Bus olevien tukemat todennus- ja ominaisuudet.

|Vertailuehdot|Azure olevien|Palvelun Bus olevien|
|---|---|---|
|Todennus|**Symmetrisen avaimen**|**Symmetrisen avaimen**|
|Suojausmalli|Valtuutetun käyttöä Suojaussidosten tunnusten kautta.|SUOJAUSSIDOKSET|
|Tunnistetietojen toimittaja yhdistämisessä|**Ei**|**Kyllä**|

### <a name="additional-information"></a>Lisätietoja

- Jokainen pyyntö joko queuing technologies todennus on. Anonyymin käytön julkisen olevien ei tueta. Käytä [SAS](service-bus-sas-overview.md), voit käsitellä tässä skenaariossa julkaisemalla vain kirjoitus-Suojaussidokset, vain luku-Suojaussidokset tai jopa täydet Suojaussidokset.

- Azure olevien myöntämä todennus värimallin liittyy symmetrisen näppäintä, joka on hash-pohjainen viestin käyttöoikeuksien koodi (HMAC), laskettu SHA-256 algoritmin kanssa ja koodattu **Base64** merkkijonon käyttäminen. Saat lisätietoja vastaaviin protokolla [Azure-tallennustilan palveluiden todennus](https://msdn.microsoft.com/library/azure/dd179428.aspx). Palvelun Bus olevien tukevat samalla mallin symmetrisen-näppäimellä. Lisätietoja on artikkelissa [Jaettujen palvelun Bus Access allekirjoituksen todentaminen](service-bus-shared-access-signature-authentication.md).

## <a name="cost"></a>Kustannukset

Tässä osassa vertaa Azure olevien ja palvelun Bus olevien kustannukset näkökulmasta.

|Vertailuehdot|Azure olevien|Palvelun Bus olevien|
|---|---|---|
|Jonon tapahtuman kustannukset|**$0.0036**<br/><br/>(kohti 100 000 tapahtumat)|**Tavallinen taso**: **$0,05**<br/><br/>(kohti miljoonaa toimintoja)|
|Laskutettavat toiminnot|**Kaikki**|**Lähetä tai vastaanota vain**<br/><br/>(maksutonta muita toimintoja)|
|Käyttämättömänä tapahtumat|**Laskutettavat**<br/><br/>(Tyhjä jonon kyselyt lasketaan laskutettavan tapahtumana.)|**Laskutettavat**<br/><br/>(Tyhjä jonon vastaan vastaanota pidetään laskutettavan viestiin.)|
|Tallennustilan kustannukset|**$0.07**<br/><br/>(gt/kk)|**0,00**|
|Lähtevien tietojen siirtäminen kustannukset|**$0,12 - $0.19**<br/><br/>(Mukaan geography.)|**$0,12 - $0.19**<br/><br/>(Mukaan geography.)|

### <a name="additional-information"></a>Lisätietoja

- Tiedonsiirron peritään kokonaissumma jättää Azure palvelinkeskusten Internetin kautta annetun laskutusjaksolla tietojen perusteella.

- Tiedonsiirron samalla alueella sijaitseva Azure-palveluiden välillä eivät ole veloittaa maksu.

- Tämä kirjoittaminen saavuttaman saapuvien tiedonsiirron eivät ole maksu.

- Annettu tuki pitkä kysely-palvelun Bus olevien käyttämällä voi olla edullinen tilanteissa missä pieni viive toimituksen.

>[AZURE.NOTE] Kaikki kustannukset voivat muuttua. Tässä taulukossa vastaa nykyisen hinnat ja ei ole mitään erikoistarjoukset, jotka ole tällä hetkellä käytettävissä. Katso Azure hinnat uusimpia tietoja [Azure hinnoittelu](https://azure.microsoft.com/pricing/) -sivulla. Saat lisätietoja palvelun Bus hinnat [palvelun Bus hinnat](https://azure.microsoft.com/pricing/details/service-bus/).

## <a name="conclusion"></a>Tekemistä

Mukaan kasvun kaksi tekniikoista tarkempaa tietoa osaat käyttämään, mitkä jonon-tekniikan siitä päätöksentekoa ja milloin. Päätös-toiminnon käyttäminen Azure olevien tai palvelun Bus olevien määräytyy selvästi useista tekijöistä. Seikoista voivat raskaasti määräytyvät sovelluksen ja sen arkkitehtuuri tarpeitasi. Jos sovelluksen jo on käytössä Microsoft Azure core ominaisuuksia, voit halutessasi valita Azure olevien, erityisesti, jos asetat basic viestintä- ja messaging services tai tarve olevien, joka voi olla suurempi kuin 80 Gigatavun kokoisia välillä.

Koska palvelun Bus olevien tarjoaa useita lisäominaisuuksia, istunnot, tapahtumia, kuten kaksoiskappaleet automaattinen tunnistus perille kirjainten ja kestävät Julkaise ja tilaa-ominaisuuksia, he voivat olla ensisijainen valinta, jos olet muodostamassa hybrid-sovelluksen tai muussa sovelluksessa vaatiiko nämä ominaisuudet.

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavissa artikkeleissa on lisätietoja ohjeet ja Azure olevien tai palvelun Bus olevien käyttämisestä.

- [Opi käyttämään palvelua Bus olevien](service-bus-dotnet-get-started-with-queues.md)
- [Opi käyttämään jonon Tallennuspalvelu](../storage/storage-dotnet-how-to-use-queues.md)
- [Suorituskykyä on parannettu käyttäen palvelua Bus parhaita käytäntöjä se viestintä](service-bus-performance-improvements.md)
- [Olevien ja Azure palvelun Bus aiheet esittely](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [Palvelun Bus Kehittäjän ohje](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Azure jonotuspalvelu käyttäminen](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Tietoja Laskutus – kaistanleveyden, tapahtumia ja kapasiteetti Azure-tallennustilan](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure portal]: https://portal.azure.com
 
