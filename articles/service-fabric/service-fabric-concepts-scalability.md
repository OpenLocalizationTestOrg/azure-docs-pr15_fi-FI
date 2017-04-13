<properties
   pageTitle="Palvelun kangasta palvelujen skaalattavuus | Microsoft Azure"
   description="Tässä artikkelissa käsitellään skaalata palvelun kangasta palvelut"
   services="service-fabric"
   documentationCenter=".net"
   authors="appi101"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"
   ms.author="aprameyr"/>

# <a name="scaling-service-fabric-applications"></a>Palvelun kangasta sovellusten skaalaus
Azure palvelun kangasta on helppo luoda skaalattava sovelluksia kuormituksen tasaamisen services, osiot ja replikoiden kaikissa klusterin solmuissa. Näin suurin resurssien käyttö.

Suuri asteikko palvelun kangasta sovellusten onnistuu kahdella tavalla:

1. Osion tasolla skaalaus

2. Palvelun nimi tasolla skaalaus

## <a name="scaling-at-the-partition-level"></a>Osion tasolla skaalaus
Palvelun kangasta tukee jakaminen yksittäisten palvelujen useita pienempi osioihin. [Yleistä jakaminen](service-fabric-concepts-partitioning.md) on tietoja osioinnin mallit, joita tuetaan tyypit. Kunkin osion replikoiden leviävät klusterin solmujen välillä. Harkitse palvelu, joka käyttää ranged osiointimalli pienen näppäintä 0, 99 hyvin avaimen ja neljään osaan. Kolme solmu-klusterin ehkä sijoitettava palvelun kanssa neljä replikat, jotka käyttävät kunkin solmun resurssit, kuten kuvassa:

![Osioasettelu, jossa on kolme solmujen](./media/service-fabric-concepts-scalability/layout-three-nodes.png)

Palvelun kangasta käyttämiseksi uusien solmujen resurssit siirtämällä joitakin replikat tyhjä solmujen lisääntyvien näkyvien solmujen määrän avulla. Neljän solmujen määrän suurentamalla palvelu on nyt kolme replikoiden kukin solmu (ja eri osiot), salliminen paremmin resurssien käyttö- ja suorituskyky-käyttöjärjestelmässä.

![Osioasettelu, jossa on neljä solmujen](./media/service-fabric-concepts-scalability/layout-four-nodes.png)

## <a name="scaling-at-the-service-name-level"></a>Palvelun nimi tasolla skaalaus
Palvelun esiintymän on työnkulun esiintymään sovelluksen nimen ja palvelun tyyppi (katso [palvelun kangasta sovelluksen elinkaaren](service-fabric-application-lifecycle.md)). Palvelun luonnin aikana määrität osion menetelmä, jota käytetään (katso [jakaminen palvelun kangasta services](service-fabric-concepts-partitioning.md)).

Ensimmäisen tason skaalaus on palvelunimi. Voit luoda uusia tiedostojoukkoja palvelun, jakaminen, muuttuvat varattu vanhempia palvelun kopioita kuin eri käyttöoikeustasoja. Näin uusi palvelu kuluttajien käyttää pienempi varattu palveluesiintymiä enemmän ilmoituksia niistä sijaan.

Yksi vaihtoehto lisätä kapasiteetin sekä kasvava tai laskevan osio-arvo on luomalla uuden palvelun esiintymän uusi osio värimalli. Tämä lisää monimutkaisuus, vaikka, vievää asiakkaiden tarpeen tietää, milloin ja miten eri tavalla nimetyn-palvelun käyttöä varten.

### <a name="example-scenario-embedded-dates"></a>Esimerkkitapaus: upotettu päivämäärät
Yksi mahdollinen tilanne olisi käyttämään palvelua nimeen päivämäärätietoja. Voit esimerkiksi käyttää tiettyä nimellä kaikkien asiakkaiden, jotka liittyneet 2013: n palvelun esiintymän ja asiakkaiden, jotka 2014 liittyneet toinen nimi. Nimeämällä Tämä malli mahdollistaa lisääntyvien ohjelmallisesti nimet päivämäärän mukaan (2014 lähestyessä 2014 esiintymää voidaan luoda pyydettäessä).

Tämän menetelmän perustuu kuitenkin asiakkaita käyttämällä sovelluksen kielikohtaiset nimeämiskäytäntö tietoja, jotka on ulkopuolelle jääviä palvelun kangasta tiedot.

- *Käytetään nimeämiskäytäntöä*: 2013, kun sovellus menee live, voit luoda kutsutaan kangasta palvelu: / app/service2013. Lähellä 2013 toisen vuosineljänneksen luot toiseen palveluun, jota kutsutaan kangasta: / app/service2014. Molempia näistä palveluista ovat samassa palvelutyyppi. Tämän menetelmän asiakkaalle on käyttää logiikan muodostamiseen vuoden perusteella tarvittavat palvelunimi.

- *Haku-palveluun*: muu kaava on toissijainen haku-palvelun, joka voidaan lisätä haluamaasi näppäintä-palvelun nimi. Valitse haku-palvelun voi luoda uusi palveluesiintymiä. Haku-palvelussa ei säilytä sovelluksen tietoja, vain näkyvissä palvelujen nimet, jotka se luo tietoja. Näin ollen vuoden perusteella edellä olevassa esimerkissä asiakkaan Ota ensin tietojen käsittely tietyn vuoden palvelun nimi kieliominaisuuksien haku-palvelun ja käyttää kyseisen palvelunimi todellinen toiminnon. Ensimmäinen haku saatu voidaan säilyttää välimuistissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun kangasta käsitteitä on seuraavissa artikkeleissa:

- [Palvelun kangasta palvelujen saatavuus](service-fabric-availability-services.md)

- [Palvelun kangasta services jakaminen](service-fabric-concepts-partitioning.md)

- [Määrittämisestä ja hallinnasta tila](service-fabric-concepts-state.md)
