<properties 
    pageTitle="Azure Gaming sovelluksen Mobile välitys käyttöönotto"
    description="Gaming app skenaarion toteuttamisesta Azure Mobile välitys" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

#<a name="implement-mobile-engagement-with-gaming-app"></a>Ota käyttöön Mobile välitys Gaming-sovelluksessa

## <a name="overview"></a>Yleiskatsaus

Gaming pysäyttämisestä on käynnistetty uuden houkutteleva mukaan role-play/strategia Ottelu sovelluksen. Ottelu on ollut 6 kuukautta hyvin alkuun. Tämän Ottelu on erittäin suuri onnistui, ja se on miljoonia lataukset säilyttäminen on erittäin suuri verrattuna Ottelu pysäyttämisestä muista sovelluksista. Vuosineljänneksen Tarkista kokouksessa sidosryhmän sopivat ne täytyy suurentaa keskimääräinen tuotto käyttäjäkohtainen (ARPU). Premium Ottelu-paketit ovat ladattavissa erikoistarjoukset. Nämä Ottelu Pack-paketit käyttäjät voivat päivittää ulkoasua ja suorituskyvyn houkutteleva rivit ja lures tai tackles löytämiseen. Paketin myynti on hyvin pieni. Näin ne päättää ensin analysoida asiakkaan kokemukset analytics-työkalulla ja sitten ohjelma niin, että myynnin käyttämällä kehittyneitä kehittää välitys Segmentointi.

Mukaan- [Azure Mobile välitys - Aloitusoppaan parhaiden käytäntöjen ja](mobile-engagement-getting-started-best-practices.md) heidän luomilleen välitys strategia.

##<a name="objectives-and-kpis"></a>Tavoitteet ja KPI: T

Ottelu täyttävät avaimen sidosryhmille. Kaikki sopivat yhteen tärkeimmät tavoite - niin, että premium paketin myynti 15 %: lla. He luovat Business suorituskykyilmaisimista (KPI) ja mittayksikön asema tämän tavoitteen

* Mikä Ottelu tasolla pakkauksissa ostetaan?
* Mikä on tuotto käyttäjää kohden, istunnossa viikossa tai kuukaudessa?
* Mitkä ovat tuttuja hankkiminen tyyppejä?

Osa 1 [Aloitusoppaan](mobile-engagement-getting-started-best-practices.md) kerrotaan, miten voit määrittää tavoitteet ja KPI: T. 

Nyt määritetty Business KPI Mobile tuotteen Manager luo välitys KPI: T, voit määrittää uuden käyttäjän trendejä ja säilytys.

* Säilytys ja yli seuraavassa väleihin käyttö: päivittäin, joka kaksi päivää, viikoittain, kuukausittain ja kolmen kuukauden välein
* Aktiivisen käyttäjän arvo
* App Storessa luokitus

IT-työryhmän suositukset-perusteella seuraavat tekniset suorituskykyilmaisimet lisättiin seuraaviin kysymyksiin:

* Mikä on käyttäjän-polku (sivu on avattu, kuinka paljon aikaa käyttäjät käyttävät sitä)
* Kaatuu tai havaittiin istunnossa virheet
* Mitä käyttöjärjestelmä Omat käyttäjät on käytössä?
* Mikä on koko näytön käyttäjille keskimäärin?
* Millaisia internet-yhteys Omat täytyykö käyttäjän?

Kunkin KPI: n Mobile tuotteen Manager määrittää tiedot, hänen on ja missä se sijaitsee hänen playbook.

## <a name="engagement-program-and-integration"></a>Välitys ohjelma ja integrointi

Ennen kuin perustaminen kehittyneet välitys-ohjelmassa, Mobile projektin johtaja saadaan päätökseen, projektin pitäisi olla laaja tietoa siitä, miten ja milloin tuotteiden vähennetään käyttäjät.

Kun kolme kuukautta Mobile projektin johtaja keräämiä riittävästi tietoja parantaakseen hänen sovelluskohtaisesta push-ilmoituksen myynti. Hän oppii:

* Ensimmäiselle tilaukselle tapahtuu yleensä tasolla 14. Ostamisesta on uusi legendaarista aseiden tapauksissa 90 %, $3.
* Käyttäjät, jotka olet tehnyt osto-tuotteen jatkaa ja tehdä muita ostaa tapauksissa 80 %.
* Käyttäjät, joilla on kulunut tason 20 käyttävät yli 10/ viikko alkaa.
* Käyttäjien yleensä ostaa premium pakettien 16, 24 ja 32 tasolla.

Tämä analyysi alkuun Mobile projektin johtaja päättää luominen tiettyyn push ilmoituksen järjestyksiä niin, että sovelluksen myynti. Hän luo kolme push sarjaa, jonka hän soittaa: Tervetuloa-ohjelman, myynti-ohjelman ja passiiviset ohjelma. Lisätietoja viitata [Playbooks](https://github.com/Azure/azure-mobile-engagement-samples/tree/master/Playbooks)
    ![][1]

<!--Image references-->

[1]: ./media/mobile-engagement-game-scenario/notification-scenario.png

<!--Link references-->
