<properties 
    pageTitle="Tapahtuman keskittimet usein kysyttyjä kysymyksiä | Microsoft Azure"
    description="Tapahtuman keskittimet usein kysytyt kysymykset."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Tapahtuman keskittimet usein kysytyt kysymykset

Tapahtuman keskittimet antaa suurissa saanti, pysyvyyttä ja käsittelyn suuren siirtonopeuden tietolähteistä tapahtumien tiedot ja/tai miljoonia laitteet. Kun parillinen palvelun Bus olevien ja ohjeita, tapahtuman keskittimet mahdollistaa pysyvä ohjaus- ja ominaisuuksissa [Asioita Internet (IoT)](https://azure.microsoft.com/services/iot-hub/) skenaarioita.

Tässä artikkelissa käsitellään hintatiedot ja vastauksia tapahtuman keskittimet joitakin usein kysyttyjä kysymyksiä:

## <a name="pricing-information"></a>Alla on tietoja

Valmis tietoja tapahtuman keskittimet hinnat on ohjeaiheessa [tapahtuman keskittimet hinnoittelutiedot](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Miten tapahtuman keskittimet tunkeutumisen tapahtumat lasketaan?

Kuhunkin tapahtumaan tapahtumaa-toiminnossa lähetetään laskee laskutettavan viestinä. *Tunkeutumisen tapahtuma* on määritetty yksikkönä, joka on enintään 64 Kilotavun tiedot. Tapahtuman, joka on pienempi tai yhtä suuri kuin 64 Kilotavun kokoinen pidetään laskutettava tapahtuma. Jos tapahtuma on yli 64 Kilotavua, laskutettavan tapahtumien määrä lasketaan mukaan tapahtuman koon kerrannaisten 64 kilotavua. Lähetetty tapahtumaa-toiminnossa 8 KB tapahtuma laskutetaan yhden tapahtumana, mutta tapahtuma-keskittimeen 96 kt viestiä laskutetaan kuin kaksi tapahtumaa.

Kulutettu tapahtuma-toiminnosta kuin tapahtumat sekä hallintatoiminnot ja hallita tarkistuspisteet kuten puheluita, ei lasketa mukaan laskutettavat tunkeutumisen stä mutta kertymä siirtonopeuden yksikön alennuksen ylöspäin.

## <a name="what-are-event-hubs-throughput-units"></a>Mitkä ovat tapahtuman keskittimet siirtonopeuden yksikköä?

Voit valita erikseen tapahtuman keskittimet siirtonopeuden yksiköt-Azure portaalin tai tapahtuman keskittimet resurssien hallinnan mallien avulla. Kaikki tapahtuman porttiin tapahtuman keskittimet nimitilan Käytä siirtonopeuden yksiköt ja siirtonopeuden kunkin yksikön oikeuttaa nimitilan seuraavia ominaisuuksia:

- Ylös 1 Mt sekunnissa tunkeutumisen tapahtumat (tapahtumien lähetetty tapahtuma-keskittimeen), mutta ei ole yli 1 000 tunkeutumisen tapahtumia, hallintatoiminnot tai ohjausobjektin API puhelujen sekunnissa.

- Enintään 2 Megatavun sekunnissa juniin tapahtumat (kulutettu tapahtuma-toiminnosta tapahtumien).

- 84 gt tapahtuman tallennustilaa (oletusarvo 24 tunnin säilytysaika sovellu).

Tapahtuman keskittimet siirtonopeuden yksiköt ovat laskuttaa kerran tunnissa, valitut annetun tunnin ajan yksiköt enimmäismäärä perusteella.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Miten tapahtuman keskittimet siirtonopeuden yksikön rajoitukset voimassa?

Jos yhteensä tunkeutumisen siirtonopeuden tai kaikki tapahtuman keskittimet-nimitilaa yli yhteensä tunkeutumisen tapahtuma-korko ylittää kooste siirtonopeuden yksikön korvauksia, lähettäjät rajoittanut ja virheitä, joka ilmaisee, että tunkeutumisen kiintiö on ylitetty.

Jos yhteensä juniin siirtonopeuden tai yhteensä tapahtuman juniin korko yli kaikki tapahtuman keskittimet-nimitilaa ylittää kooste siirtonopeuden yksikön korvauksia, vastaanottajia on rajoittanut ja virheitä, joka ilmaisee, että juniin kiintiö on ylitetty. Tunkeutumisen ja juniin kiintiön on pakotettu erikseen, jotta ei lähettäjän heikentää tapahtuman kulutus hidastaa eikä vastaanotin estää tapahtumien tapahtuma-keskittimeen lähettämisen.

Huomaa, että siirtonopeuden yksikön valinta riippumaton tapahtuman keskittimet osioiden määrää. Kun kunkin osion tarjoaa suurin nopeus 1 Mt toisen tunkeutumisen (jossa on enintään 1 000 tapahtumat sekunnissa) ja toinen juniin 2 Mt, on kiinteä maksutonta osioiden itse. Kulu on kaikki tapahtuman keskittimet tapahtuman keskittimet nimitilan koostetun nopeus-yksikköä. Tämän mallin avulla voit luoda tarpeeksi osioiden tukemaan odotettua suurin kuormitus niiden Systemsin niin, ettei siirtonopeuden yksikön kulut, kunnes järjestelmä tapahtuman kuormitus todella edellyttää siirtonopeuden suuremmat numerot ja eikä sinun tarvitse arkkitehtuuri järjestelmien ja rakenteen muuttaminen järjestelmän kuormitus kasvaa.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Onko valittavissa olevat siirtonopeuden yksiköiden määrän rajoitusta?

On 20 siirtonopeuden yksikköä kohden nimitilan oletusarvon kiintiön. Voit pyytää suurempi kiintiön siirtonopeuden yksiköiden ilmoittaa tuki lippu. Lisäksi 20 siirtonopeuden yksikön raja-niput ovat käytettävissä 20 – 100 siirtonopeuden mittayksikkö. Huomaa, että yli 20 siirtonopeuden yksikkö poistaa voi muuttaa siirtonopeuden yksiköiden määrän, mutta siirtämiseen tuki lippu.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Onko maksu käyttämiselle tapahtuman keskittimet tapahtumien yli 24 tuntia?

Tapahtuman keskittimet vakio taso Salli viestin säilytys kausien yli 24 tuntia, enintään 30 päivää. Jos tallennettu tapahtumien määrä ylittää tallennustilan alennuksen varten valitut siirtonopeuden yksiköiden (kohti siirtonopeuden 84 gt), koko, joka on suurempi kuin alennuksen laskutetaan julkaistun Azure Blob storage korko. Tallennustilan alennuksen siirtonopeuden kunkin yksikön kattaa kaikki tallennustilan kustannukset säilytys kausilla 24 tunnin (oletusasetus) vaikka siirtonopeuden yksikkö on käytettävä suurin tunkeutumisen alennuksen.

## <a name="what-is-the-maximum-retention-period"></a>Mikä on suurin säilytysaika?

Tapahtuman keskittimet vakio taso tukee tällä hetkellä suurin säilytysaika 7 päivää. Huomaa, että tapahtuman keskittimet ei ole tarkoitettu pysyvä tietosäilö. Yli 24 tunnin säilytys kausien on tarkoitettu skenaariot, jossa on kätevä toista tapahtuman stream kyselyjä saman järjestelmien; Jos esimerkiksi kouluttaminen tai vahvistamaan uuden tietokoneen learning mallin aiemmin luotujen tietojen.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Miten tapahtuman keskittimet tallennustilan koko lasketaan ja veloitetaan?

Kaikki tallennetut tapahtumia, kuten sisäinen katseltavan tapahtumaotsikot tai levyn tallennustilan rakenteiden kaikki tapahtuman keskittimet kokonaiskoko mitataan koko päivän. Huippu-tallennustilan koko lasketaan päivän lopussa. Päivittäisen tallennustilan korvauksen lasketaan vähimmäismäärä siirtonopeuden yksiköiden, joka on valittuna (siirtonopeuden kunkin yksikön tarjoaa 84 gt alennuksen) päivän aikana perusteella. Jos koko ylittää laskettu päivittäisen tallennustilan korvauksen, ylimääräisen tallennustilan laskutetaan käyttämällä Azure Blob storage korvaukset ( **Paikallisesti tarpeettomat tallennustilan** kurssi).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Yksittäisen AMQP-yhteyden avulla voit lähettää ja vastaanottaa tapahtuman keskittimet ja palvelun Bus olevien/aiheet

Kyllä, kunhan tapahtuman keskittimet, olevien ja aiheet ovat samaa nimitilaa. Sellaisenaan voit toteuttaa kaksisuuntaisen, se yhteyden monta laitteen, jossa on subsecond viiveet suurempia edullinen ja erittäin skaalattava tavalla.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Se yhteyden ovat voimassa tapahtuman porttiin?

Saat lähettäjät-yhteyden ovat voimassa vain, kun AMQP-protokollaa käytetään. Veloituksia ei ole yhteyttä lähettämiseen tapahtumien http:n määrän lähettäminen järjestelmien tai laitteiden riippumatta. Jos aiot käyttää AMQP (esimerkiksi tavoitteet tehokkaammin tapahtumaa streaming tai kaksisuuntaisen tietoliikenne IoT ohjaus- ja tilanteissa), katso lisätietoja siitä, mitä se yhteyden tarkoittaa ja miten käytön mukaan laskutettavat [palvelun Bus alla on tietoja](https://azure.microsoft.com/pricing/details/service-bus/) -sivu.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Mitä eroa tapahtuman keskittimet perus- ja Standard tasoa?

Tapahtuman keskittimet vakio taso sisältää ominaisuuksia, mikä on käytettävissä tapahtuman keskittimet Basic, ja jotkin kilpailuun järjestelmät. Nämä ominaisuudet ovat säilytys kausien yli 24 tuntia ja voi käyttää yhden AMQP yhteyden lähettää komentoja paljon laitteet, joissa on subsecond viiveitä sekä telemetriatietojen lähettäminen kyseiset laitteet tapahtuman keskittimet. Katso luettelo toiminnoista, [tapahtuman keskittimet hinnoittelutiedot](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="geographic-availability"></a>Maantieteelliset käytettävyys

Tapahtuman keskittimet on saatavilla seuraavilla alueilla:

|GEO|Alueiden|
|---|---|
|Yhdysvallat|Keskitetyn US, Yhdysvaltojen Itä, Yhdysvaltojen Itä 2, Etelä keskitetyn US, Länsi US|
|Europe|Pohjois-Eurooppa, Länsi Europe|
|Aasian ja Tyynenmeren alueen|Itä-Aasian varaaja Aasian|
|Japani|Japanilainen Itä Länsi japani|
|Brasilia|Brasilia Etelä|
|Australia|Australian Itä-Australia varaaja|

## <a name="support-and-sla"></a>Tuki- ja SLA

Tapahtuman keskittimet teknisestä tuesta on saatavana [yhteisön keskustelupalstoilla](https://social.msdn.microsoft.com/forums/azure/home). Laskutus ja tiedostohallinnan tuki on annettu ilmaiseksi.

Lisätietoja Microsoftin SLA on [Palvelun tason toimeenpano](https://azure.microsoft.com/support/legal/sla/) -sivulla.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja tapahtuman keskittimet on seuraavissa artikkeleissa:

- [Tapahtuman keskittimet yleiskatsaus][].
- Valmis [tapahtuman keskittimet käyttävän sovelluksen malli][].

[Tapahtuman keskittimet yleiskatsaus]: event-hubs-overview.md
[Tapahtuman keskittimet käyttävä malli-sovellus]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
