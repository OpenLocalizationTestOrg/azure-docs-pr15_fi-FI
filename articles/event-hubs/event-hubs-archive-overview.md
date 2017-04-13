<properties
    pageTitle="Azure tapahtuman keskittimet arkisto | Microsoft Azure"
    description="Azure tapahtuman keskittimet arkistotoimintoa yleiskatsaus."
    services="event-hubs"
    documentationCenter=""
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="darosa;sethm"/>

# <a name="azure-event-hubs-archive"></a>Azure tapahtuman keskittimet arkisto

Azure tapahtuman keskittimet arkisto avulla voit pitää automaattisesti streaming tiedot tapahtuma-keskittimet Blob storage tiliin valittua joustavuutta Määritä aika tai kokoa aikavälin sähköpostikansioon kanssa. Arkisto määrittäminen on lyhyt, ei ole järjestelmänvalvojan kustannukset suorittaa ja se sovittaa tapahtuman keskittimet- [siirtonopeuden yksiköt](event-hubs-overview.md#capacity-and-security)automaattisesti. Tapahtuman keskittimet arkisto on helpoin tapa ladata streaming tietoja Azure, ja voit keskittyä tietojen käsittely tietojen sieppaus sijaan.

Azure tapahtuman keskittimet arkisto avulla voit käsitellä reaaliaikainen ja erä putkistot-samassa muodossa. Mahdollistaa ratkaisuja, jotka voivat kasvaa ajan kuluessa tarpeitasi kanssa. Luot erä-järjestelmiin tänään silmällä kohti tulevien reaaliaikainen käsittely kanssa tai haluat lisätä tehokas kylmän polku aiemman reaaliaikainen ratkaisun, tapahtuman keskittimet arkisto on streaming helpottaa tietojen käsittelemistä.

## <a name="how-event-hubs-archive-works"></a>Tapahtuman keskittimet arkisto toiminta

Tapahtuman keskittimet on aika säilytys kestävät puskurin telemetriatietojen tunkeutumisen, eri aikajaksoille lokia samalla. Tapahtuman keskittimet skaalata on [osioitu kuluttaja-malli](event-hubs-overview.md#partition-key). Kunkin osion on tietoja riippumaton osa ja itsenäisesti kulutettu. Ajan kuluessa tiedoista ikäiset käytöstä määritettäviä säilytysaika perusteella. Tämän vuoksi annetun tapahtumaa-toiminnossa koskaan saa "liian täynnä."

Tapahtuman keskittimet arkisto avulla voit määrittää oman tilin Azure-Blob-säiliö ja säilö, joka voidaan tallentaa arkistoidut tiedot. Tämän tilin voi olla samaa alueen kuin tapahtumaa-toiminnossa tai toisen alueen tapahtuman keskittimet arkistotoimintoa joustavuutta lisääminen.

Arkistoidut tiedot on kirjoitettu [Apache Avro][] muodossa. uudelleenjärjestäminen, nopea ja binaarinen muoto, joka sisältää monipuolisia rakenteet sisäistä rakennetta. Tässä muodossa käytetään yleisesti Hadoop ekosysteemissä ja Stream Analytics ja Azure Data Factory. Lisätietoja Avro käyttämisestä on tämän artikkelin.

### <a name="archive-windowing"></a>Arkisto-Windowing

Tapahtuman keskittimet arkisto voit määrittäminen ikkunassa voit määrittää arkistointi. Tämä ikkuna on vähimmäiskoko ja ajan määrittäminen "ensimmäisen wins käytännön kanssa," mikä tarkoittaa, että ensimmäinen käynnistin havaitsi aloittaa arkisto-toiminto. Jos käytössäsi on 15 minuutin/100 Mt arkisto-ikkuna ja Lähetä 1 Mt/s-ikkunan koko käynnistää ennen aika-ikkuna. Kunkin osion arkistoidaan itsenäisesti ja kirjoittaa valmiit estä Blob-objektien arkisto, milloin nimeltä kerran, kun arkisto välin ilmeni. Nimeämiskäytännön on seuraavanlainen:

```
<Namespace>/<EventHub>/<Partition>/<YYYY>/<MM>/<DD>/<HH>/<mm>/<ss>
```

### <a name="scaling-to-throughput-units"></a>Siirtonopeuden yksikkö skaalaus

Tapahtuman keskittimet liikenne ohjataan [siirtonopeuden yksiköt](event-hubs-overview.md#capacity-and-security). Yksittäisen siirtonopeuden yksikkö avulla 1 Mt/s tai 1000 tapahtumat sekunnissa tunkeutumisen ja kahdesti, juniin määrän. Vakio tapahtuman keskittimet voidaan määrittää 1 – 20 siirtonopeuden yksiköt ja lisää voi ostaa kiintiön Suurenna [Pyydä tuki][]kautta. Lisäksi siirtonopeuden ostettujen yksiköiden käyttö on rajoitettu. Tapahtuman keskittimet arkisto kopioi tiedot suoraan tapahtuman keskittimet Sisäinen tallennusväline-ohittaminen siirtonopeuden yksikön juniin kiintiön ja että juniin tallentaminen muiden käsittely näytönlukuohjelmalla, kuten Stream Analytics tai ohjattu.

Kun määritetty, tapahtuman keskittimet arkisto käynnistyy automaattisesti heti, kun lähetät ensimmäinen tapahtuma. Se säilyy suorittaminen aina. Voit helpottaa oman edeltävät käsittelee tietävän, että prosessi toimii, tapahtuman keskittimet kirjoittaa tyhjiä tiedostoja, kun tietoja ei ole. Tämä on varmasti ennakoitavissa cadence ja merkki, joka voi syötteen erä-suorittimista.

## <a name="setting-up-event-hubs-archive"></a>Tapahtuman keskittimet arkisto määrittäminen

Tapahtuman keskittimet arkisto voidaan määrittää tapahtumaa-toiminnossa luontiaikaa portal tai Azure Resurssienhallinta. Otetaan käyttöön ainoastaan arkisto napsauttamalla **-** painiketta. Tallennustilan tilin ja säilö määrittäminen valitsemalla **säilö** -osa sivu. Koska tapahtuman keskittimet arkisto käyttää palvelun todennusta ja tallennustilaa, sinun ei tarvitse määrittää tallennustilan yhteysmerkkijonon. Resurssin valitsin valitsee resurssin URI-tallennustilan tilin automaattisesti. Jos käytät Azure Resurssienhallinta, sinun on annettava tämän URI eksplisiittisesti merkkijonona.

Oletusarvoisesti aika-ikkuna on viisi minuuttia. Pienin arvo on 1, enintään 15. Ikkunan **koko** on 10 – 500 megatavua alue.

![][1]

## <a name="adding-archive-to-an-existing-event-hub"></a>Arkisto lisääminen aiemmin luotu tapahtuma-toiminnossa

Arkistoinnin voidaan määrittää aiemmin luotu tapahtuma keskittimet, jotka ovat tapahtuman keskittimet nimitilan. Ominaisuus ei ole käytettävissä vanhat viestit tai sekoitus tyyppi nimitilan. Arkisto-aiemmin luotu tapahtuma-toiminnossa käyttöön tai arkisto-asetusten muuttaminen, valitse oman nimitilan ladata **Essentials** -sivu ja valitse sitten Valitse tapahtuma-toiminto, jonka haluat ottaa käyttöön tai muuttaa arkisto-asetusta. Valitse lopuksi Avaa sivu **Ominaisuudet** -osan seuraavassa kuvassa esitetyllä tavalla.

![][2]

Voit myös määrittää tapahtuman keskittimet arkisto Azure Resurssienhallinta malleja kautta. Katso lisätietoja [Tässä artikkelissa](event-hubs-resource-manager-namespace-event-hub-enable-archive.md).

## <a name="exploring-the-archive-and-working-with-avro"></a>Arkiston tarkasteleminen ja käsitteleminen Avro

Kun määritetty, tapahtuman keskittimet arkisto Luo tiedostoja Azure-tallennustilan tilin ja säilö annettujen määritetyt aika-ikkuna. Voit tarkastella näitä tiedostoja jokin työkalu, kuten [Azure-tallennustilan Explorer][]. Voit ladata tiedostot paikallisesti niihin.

Tapahtuman keskittimet arkisto luomat tiedostot on seuraavassa Avro rakenteessa:

![][3]

Helppo tapa tarkastella Avro tiedostoja on käyttää [Avro Työkalut][] -Apache purkkiin. Lataa tämä purkki näet tietyn Avro tiedoston rakenne suorittamalla seuraavan komennon:

```
java -jar avro-tools-1.8.1.jar getschema \<name of archive file\>
```

Tämä komento palauttaa

```
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Voit käyttää myös Avro Työkalut-tiedoston muuntaminen JSON-muoto ja tee muita käsittely.

Suoritettava monimutkaisemman käsittely Lataa ja asenna Avro ympäristön valittua. Milloin tämä kirjoittamista ei käyttöotot käytettävissä C, C++, C\#, Java, NodeJS, Perl, PHP, Python ja Ruby.

Apache Avro on valmis aloittaminen apuviivat [Java][] ja [Python][]. Voit myös lukea [Tapahtuman keskittimet arkisto käytön aloittaminen](event-hubs-archive-python.md) -artikkelissa.

## <a name="how-event-hubs-archive-is-charged"></a>Miten tapahtuman keskittimet arkisto veloitetaan

Tapahtuman keskittimet arkisto on käytön mukaan laskutettavat vastaavasti siirtonopeuden yksikkö kuin tunnin välein maksu. Kulu on suoraan siirtonopeuden nimitilan ostettujen yksiköiden määrän. Siirtonopeuden yksiköt kasvaa ja pienentynyt, tapahtuman keskittimet arkisto kasvattaa ja pienentää antamaan vastaavia suorituskykyä. Yritystietopalveluiden tapahtua metriä. Tapahtuman keskittimet arkisto maksu on 0,10 tunnissa siirtonopeuden yksikön tarjottujen 50 % alennuksen aikana esikatselu.

Tapahtuman keskittimet arkisto on todella helpoin tapa tuoda tietoja Azure. Azure tietojen järvi, Azure Data Factory ja Azure Hdinsightista avulla voit suorittaa eräkäsittely ja muut analysoinnin sähköpostikansioon käyttämällä tuttuja työkaluja ja ympäristöjen milloin tahansa tarvitset asteikko.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue lisää tapahtuman keskittimet ohjesisältöä on seuraavissa linkeissä:

- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].
- [Ulos tapahtuman käsittely tapahtuman keskittimet asteikko][] -malli.
- [Tapahtuman keskittimet yleiskatsaus][]

[Apache Avro]: http://avro.apache.org/
[tukipyyntö]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[1]: ./media/event-hubs-archive-overview/event-hubs-archive1.png
[2]: media/event-hubs-archive-overview/event-hubs-archive2.png
[Azure-tallennustilan Explorer]: http://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-archive-overview/event-hubs-archive3.png
[Avro Työkalut]: http://www-us.apache.org/dist/avro/avro-1.8.1/java/avro-tools-1.8.1.jar
[Java]: http://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: http://avro.apache.org/docs/current/gettingstartedpython.html
[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skaalaa ulos tapahtuman käsittely tapahtuman keskittimet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3