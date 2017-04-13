<properties 
    pageTitle="Palvelun Bus .NET ja AMQP 1.0 | Microsoft Azure"
    description=".NET-palvelun Bus käyttäminen AMQP"
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
    ms.date="10/03/2016"
    ms.author="sethm" />

# <a name="using-service-bus-from-net-with-amqp-10"></a>.NET-palvelun Bus käyttäminen AMQP 1.0

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Palvelun Bus SDK lataaminen

AMQP 1.0 tuki on käytettävissä palvelun Bus SDK 2.1 tai uudempi versio. Voit varmistaa lataamalla palvelun Bus bittien [NuGet][]on uusin versio.

## <a name="configuring-net-applications-to-use-amqp-10"></a>Jos haluat käyttää AMQP 1.0 .NET-sovellusten määrittäminen

Oletusarvon mukaan palvelun Bus .NET asiakkaan kirjaston kommunikoi erillinen SOAP-pohjainen-protokollan avulla palvelun Bus-palvelussa. AMQP 1.0 käyttää sen sijaan, että oletusarvo-protokolla edellyttää eksplisiittinen configuration-palvelun Bus-yhteysmerkkijonon seuraavan osion ohjeiden mukaisesti. Tämä muutos kuin sovelluksen koodin säilyy muuttumattomana AMQP 1.0 käytettäessä.

Nykyisessä versiossa on joitakin API-ominaisuudet, joita ei tueta, kun käytät AMQP. Tuettuja ominaisuuksia on lueteltu jäljempänä osan [ominaisuuksia, rajoitukset, ja käytöspiirteen eroja](#unsupported-features-restrictions-and-behavioral-differences). Osa lisämäärityksiä asetuksista on eri merkitys myös AMQP käytettäessä.

### <a name="configuration-using-appconfig"></a>Käyttämällä App.config määritys

Se on hyvä sovellusten App.config määritystiedoston avulla voit tallentaa asetukset. Palvelun Bus sovellusten App.config avulla voidaan tallentaa palvelun Bus **ConnectionString** arvon asetukset. Esimerkki App.config-tiedostoa on seuraavanlainen:

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Arvo `Microsoft.ServiceBus.ConnectionString` asetus on palvelun Bus yhteysmerkkijonon, joka määrittää palvelun Bus yhteys käytetään. Muoto on seuraavanlainen:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

Jos `[namespace]` ja `SharedAccessKey` saadaan [Azure portal][]. Lisätietoja on artikkelissa [palvelun Bus olevien käytön aloittaminen][].

Kun käytät AMQP, Liitä yhteysmerkkijonon kanssa `;TransportType=Amqp`. Tämän ilmoituksen voit tehdä sen yhteys palvelun Bus käyttämällä AMQP 1.0 asiakas-kirjasto.

## <a name="message-serialization"></a>Viestin Sarjatoiminto

Oletusarvoinen protokolla käytettäessä .NET asiakkaan kirjaston Sarjatoiminto oletustoiminnon on käyttää [System.Runtime.Serialization][] onnistu [BrokeredMessage][] -esiintymän siirron asiakirjakirjaston ja palvelun Bus-palvelun välillä. Käytettäessä AMQP siirtotila asiakirjakirjaston käyttää AMQP tyyppi-järjestelmän Sarjatoiminto [se viestin][BrokeredMessage] AMQP viestiin. Tämä Sarjatoiminto mahdollistaa vastaanotettu ja vastaanottava sovellus, jonka käyttöjärjestelmä on mahdollisesti eri ympäristössä, esimerkiksi on Java-sovellus, joka käyttää JMS Ohjelmointirajapinnan palvelun Bus luokittelee viestin.

Kun muodosta [BrokeredMessage][] esiintymän, voit antaa .NET-objektin parametrina konstruktoria yhteyshenkilönä viestin tekstiosaan. Objektit, jotka voidaan määrittää AMQP primitiivityyppejä tekstissä on muuntaa sarjaksi AMQP tietojen tyyppiin. Jos objektia ei voi yhdistää suoraan sisään AMQP yksinkertaisen tyypin; Toisin sanoen sitten objektin muuntaa sarjaksi [System.Runtime.Serialization][]ja sarjoitettu tavua, sovellus määrittää mukautetun tyypin lähetetään AMQP tietojen viestin.

Helpottamiseksi yhteensopivuus-.NET asiakkaiden kanssa, käytä vain .NET tyypit, jotka voi muuntaa sarjaksi suoraan AMQP tyypit viestin tekstiosaan. Seuraavassa taulukossa kuvataan tällaisia ja vastaavan yhdistäminen AMQP tyyppi-järjestelmään.

| .NET leipätekstin objektityyppi          | Yhdistettyjen AMQP tyyppi                          | AMQP osan perusrakenne                                                                                                                                    |
|--------------------------------|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| bool                           | Totuusarvo                                   | AMQP arvo                                                                                                                                                |
| tavu                           | ubyte                                     | AMQP arvo                                                                                                                                                |
| ushort                         | ushort                                    | AMQP arvo                                                                                                                                                |
| uint                           | uint                                      | AMQP arvo                                                                                                                                                |
| ulong                          | ulong                                     | AMQP arvo                                                                                                                                                |
| sbyte                          | tavu                                      | AMQP arvo                                                                                                                                                |
| lyhyt                          | lyhyt                                     | AMQP arvo                                                                                                                                                |
| kokonaisluku                            | kokonaisluku                                       | AMQP arvo                                                                                                                                                |
| pitkä                           | pitkä                                      | AMQP arvo                                                                                                                                                |
| liukuluku                          | liukuluku                                     | AMQP arvo                                                                                                                                                |
| Kaksinkertainen                         | Kaksinkertainen                                    | AMQP arvo                                                                                                                                                |
| desimaaliluku                        | decimal128                                | AMQP arvo                                                                                                                                                |
| merkki                           | merkki                                      | AMQP arvo                                                                                                                                                |
| Päivämäärä ja aika                       | aikaleima                                 | AMQP arvo                                                                                                                                                |
| GUID-tunnus                           | UUID                                      | AMQP arvo                                                                                                                                                |
| Byte]                         | binaaritiedosto                                    | AMQP arvo                                                                                                                                                |
| merkkijono                         | merkkijono                                    | AMQP arvo                                                                                                                                                |
| System.Collections.IList       | luettelo                                      | AMQP arvo: kokoelman kohteet voi olla vain ne, jotka on määritelty tässä taulukossa.                                                             |
| System.Array                   | matriisi                                     | AMQP arvo: kokoelman kohteet voi olla vain ne, jotka on määritelty tässä taulukossa.                                                             |
| System.Collections.IDictionary | kartta                                       | AMQP arvo: kokoelman kohteet voi olla vain ne, jotka on määritelty tässä taulukossa. Huomautus: tuetaan vain merkkijono-näppäimiä.                        |
| URI                            | Merkkijono on kuvattu (katso seuraava taulukko) | AMQP arvo                                                                                                                                                |
| DateTimeOffset-arvoa                 | Pitkä on kuvattu (katso seuraava taulukko)   | AMQP arvo                                                                                                                                                |
| Aikajakson                       | Pitkä on kuvattu (katso seuraava)         | AMQP arvo                                                                                                                                                |
| Muodossa                         | binaaritiedosto                                    | AMQP tiedot (voi olla useita). Tietojen osat sisältävät Stream-objekti luetaan raaka tavua.                                                           |
| Muuhun objektiin                   | binaaritiedosto                                    | AMQP tiedot (voi olla useita). Sisältää System.Runtime.Serialization tai Sarjatoiminto, sovelluksen toimittamaan käyttävää objektia sarjoitettu binaariluvuksi. |

| .NET-tyyppi      | Yhdistettyjen AMQP kuvattu tyyppi                                                                                              | Huomautuksia                   |
|----------------|-------------------------------------------------------------------------------------------------------------------------|-------------------------|
| URI            | `<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>`                       | Uri.AbsoluteUri         |
| DateTimeOffset-arvoa | `<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` | DateTimeOffset.UtcTicks |
| Aikajakson       | `<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> `              | TimeSpan.Ticks          |

## <a name="unsupported-features-restrictions-and-behavioral-differences"></a>Ei-tuettuja ominaisuuksia, rajoitukset ja käytöspiirteen erot

Palvelun Bus .NET-Ohjelmointirajapinnan seuraavia ominaisuuksia ei tueta tällä hetkellä, kun AMQP avulla:

-   Tapahtumat

-   Lähetä siirron kohde

Eroja myös pieni palvelun Bus .NET-Ohjelmointirajapinnan toiminnan AMQP-protokolla verrattuna käytettäessä:

-   [OperationTimeout][] -ominaisuus oteta huomioon.

-   `MessageReceiver.Receive(TimeSpan.Zero)`on toteutettu `MessageReceiver.Receive(TimeSpan.FromSeconds(10))`.

-   Viimeistellään viestien Lukitse tunnusten avulla voi tehdä vain viestin vastaanottajat, jotka alun perin vastaanotetut viestit mukaan.

## <a name="controlling-amqp-protocol-settings"></a>AMQP protokolla-asetusten hallinta

.NET-ohjelmointirajapinnan näyttää useita asetuksia hallitaan AMQP protokolla:

-   **MessageReceiver.PrefetchCount**: Ohjaa alkuperäinen luottotietojen käytetty linkin. Oletusarvo on 0.

-   **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: AMQP kehyksen enimmäiskokoa tarjota verkkoyhteydessä neuvottelun aikana ohjausobjektit Avaa kerralla. Oletusarvo on 65 536 tavun.

-   **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Jos siirrot ovat batchable, tämä arvo määrittää suurin viive dispositions lähettämistä varten. Lähettäjien ja vastaanottajien periytyvät oletusarvoisesti. Yksittäisten lähettäjä ja vastaanottaja voi ohittaa oletus, joka on 20 millisekuntia.

-   **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: määrittää, onko AMQP yhteydet on luotu SSL-yhteyttä. Oletusarvo on **Tosi**.

## <a name="next-steps"></a>Seuraavat vaiheet

Haluatko lisätietoja? Seuraavassa on seuraavissa linkeissä:

- [Palvelun Bus AMQP yleiskatsaus]
- [Palvelun Bus osioitu olevien ja aiheet AMQP 1.0 tuki]
- [Windows Server Service Bus AMQP]

  [Palvelun Bus olevien käytön aloittaminen]: service-bus-dotnet-get-started-with-queues.md
  [System.Runtime.Serialization]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
  [Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
  [OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure portal]: https://portal.azure.com
[Palvelun Bus AMQP yleiskatsaus]: service-bus-amqp-overview.md
[Palvelun Bus osioitu olevien ja ohjeita AMQP 1.0 tuki]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[Windows Server Service Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
