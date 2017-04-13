<properties
    pageTitle="Palvelun Bus Premium- ja Standard Messaging hinnat tasoa yleiskatsaus | Microsoft Azure"
    description="Palvelun Bus Premium ja vakio viestintä"
    services="service-bus"
    documentationCenter=".net"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/02/2016"
    ms.author="darosa;sethm"/>

# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Palvelun Bus Premium ja vakio tekstiviesti tasoa 

Palvelun Bus messaging, joka sisältää esimerkiksi olevien kohteiden messaging ja aiheiden yhdistää yrityksen messaging RTF ominaisuuksia Julkaise-tilaa semantiikkaan liittyvien cloud tasolla. Palvelun Bus messaging käytetään viestintä kuvaa runkoverkkoa monta kehittyneitä cloud ratkaisuja.

Palvelun Bus viestien *Premium* -tason käsittelee Yleiset asiakkaan pyynnöt mittakaava, suorituskykyä ja tärkeiden sovellusten käytettävyyden ympärille. Vaikka toiminto määrittää samanlaiset lähes, nämä kaksi tasoa palvelun Bus viestien on suunniteltu yhteyshenkilönä eri Käytä tapauksissa.

Seuraavassa taulukossa näkyvät korostettuina korkean tason eroja.

| Premium                               | Vakio                       |
|---------------------------------------|--------------------------------|
| Suuri nopeus                       | Muuttujan siirtonopeuden            |
| Ennakoitavissa suorituskyky               | Muuttujan viive               |
| Ennakoitavissa hinnat                   | Käytön mukaan muuttujan hinnat |
| Mahdollisuus skaalata ylä-ja kuormituksen | PUUTTUU                            |
| Viestin kokoa > 256 kt                  | Viestin koko on 256 kt          |

Resurssin eristystaso suorittimen ja muistin tasolla **Palvelun Bus Premium viestintä** on, niin, että kunkin asiakkaan työmäärää suoritetaan yksinään. Tämän resurssin säilön kutsutaan *messaging yksikkö*. Kunkin premium nimitila on varattu vähintään yhden tekstiviesti yksikön. Voit hankkia 1, 2 tai 4 tekstiviesti yksiköt kunkin palvelun Bus Premium nimitilan. Yksittäisen kuormituksen tai kohde voi olla useita tekstiviesti yksiköt ja tekstiviesti yksiköiden määrän, voi muuttaa milloin, vaikka laskutus on 24 tunnin tai päivittäin maksut. Tulos on ennakoitavissa ja toistettavien suorituskyvyn palvelun Bus-ratkaisuun.

Paitsi on tämän suorituskyvyn ennakoitavissa ja käytettävissä, mutta myös on nopeampaa. Palvelun Bus Premium messaging perustuu [Azure tapahtuman keskittimet](https://azure.microsoft.com/services/event-hubs/)käyttöön tallennustilan ohjelma. Premium messaging, Huippu suorituskyky on paljon nopeammin kuin Vakio taso.

## <a name="premium-messaging-technical-differences"></a>Premium viestit tekniset erot

Seuraavassa on muutamia erot Premium ja vakio tekstiviesti tasoa.

### <a name="partitioned-queues-and-topics"></a>Osioitu olevien ja aiheet

Osioitu olevien ja aiheet ovat tuettuja messaging Premiumin, mutta ne eivät toimi samalla tavalla kuin Vakio- ja Basic tasoa palvelun Bus viestien. Premium messaging ei käytä SQL tietosäilö ei enää on mahdollista resurssin kilpailuun liittyvien ja jaetun ympäristö. Tämän vuoksi jakaminen ei ole välttämätöntä. Lisäksi osion määrä on muutettu 16 osioita vakio messaging 2 osioihin Premiumissa. On kaksi osiota varmistaa käytettävyys ja tarkoituksenmukaisempia määrä Premium runtime-ympäristössä. Lisätietoja jakaminen on [Partitioned olevien ja -kohdissa](service-bus-partitioning.md).

### <a name="express-entities"></a>Expressin kohteiden

Koska Premium Messaging suoritetaan kokonaan erillään runtime-ympäristössä, Expressin kohteiden ei tueta Premium nimitilan. Saat lisätietoja express ominaisuudesta [Microsoft.ServiceBus.Messaging.QueueDescription.EnableExpress](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enableexpress.aspx) -ominaisuus.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun Bus viestintä on seuraavissa artikkeleissa.

- [Azure-palvelu Bus Premium messaging (blogimerkintä) esittely](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
- [Azure-palvelu Bus Premium messaging (Channel9) esittely](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
- [Palvelun Bus tekstiviesti yleiskatsaus](service-bus-messaging-overview.md)
- [Opi käyttämään palvelua Bus olevien](service-bus-dotnet-get-started-with-queues.md)
