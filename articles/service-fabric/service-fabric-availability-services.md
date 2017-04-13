<properties
   pageTitle="Käytettävyys palvelun kangasta palvelujen | Microsoft Azure"
   description="Tässä artikkelissa kuvataan vika tunnistaminen automaattisesti ja palvelut palauttamisen"
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

# <a name="availability-of-service-fabric-services"></a>Palvelun kangasta palvelujen saatavuus
Azure palvelun kangasta services voi olla tilallisten tai kansalaisuudettomalla. Tässä artikkelissa on yleiskatsaus, miten palvelun kangasta ylläpitää käytettävyys palvelun Jos virheet.

## <a name="availability-of-service-fabric-stateless-services"></a>Palvelun kangasta tilattomien palvelujen saatavuus
Tilattomien palvelu on sovelluksen-palvelu, joka ei ole [paikallisen pysyvästi](service-fabric-concepts-state.md).

Luominen tilattomien palvelu edellyttää määrittäminen esiintymän määrä, on tilattomien palvelu, joka on oltava käynnissä klusterin esiintymien määrän. Tämä on sovelluksen logiikkaa, klusterin käynnistettäväksi kopioiden lukumäärä. Suositeltava tapa skaalaus määrittäminen tilattomien palvelu on esiintymien määrän.

Kun virheen löydetään tilattomien palvelun esiintymä, uuden esiintymän luodaan joitakin muita olevalla solmun klusterin.

## <a name="availability-of-service-fabric-stateful-services"></a>Palvelun kangasta tilallisten palvelujen saatavuus
Tilallisten palvelun on joitakin liittyy tila. Palvelun kangasta tilallisten palvelu on mallintaa replikoiden sarjana. Replikoissa on esiintymä palvelukoodi, joka sisältää kopion tila. Luku- ja kirjoitusoikeudet toimintoja suoritetaan yhdessä replikassa (jota kutsutaan ensisijainen). Maa muutokset kirjoittaa toiminnot ovat *replikoida* useita muita replikoita (eli aktiivinen secondaries). Perus- ja aktiivinen toissijainen replikoiden yhdistelmä on replikajoukon palvelun.

Voi olla vain yksi ensisijainen replikan ylläpidon lukeminen ja kirjoittaminen pyynnöt, mutta voi olla useita aktiivinen toissijainen replikoita. Määrä on aktiivinen toissijainen on määritettävä ja suurempi replikamäärälle hyväksyt samanaikainen ohjelmiston ja laitteisto suurempi määrä.

Virheen (kun ensisijainen se siirtyy) tapahtuma-ja palvelun kangasta tekee yhden aktiivisen toissijainen replikoiden uusi ensisijainen se. Tämän aktiivinen toissijainen replikan on jo päivitetyn version tila (joko *sallittuja*), ja voit jatkaa käsittelyä edelleen luku ja kirjoitus-toimintoja.

Tämän käsitteen--ensisijainen tai aktiivinen replikan toissijainen--kutsutaan replikarooli.

### <a name="replica-roles"></a>Replikan roolit
Replikan roolin käytetään parhaillaan hallinnoi replika tilan elinkaaren hallinta. Replika, jonka rooli on ensisijainen services lukea pyynnöt. Se myös palvelut kirjoittaminen pyynnöt päivittäminen tilaan ja replikoiminen muutokset-sen replikajoukon active secondaries. Aktiiviset toissijaiset rooli on Vastaanota tilan muutokset, jotka ensisijainen se on replikoida ja Päivitä valtion näkymänsä.

>[AZURE.NOTE] Ylemmän tason ohjelmoinnin malleja, kuten [luotettava toimijoiden framework](service-fabric-reliable-actors-introduction.md) abstrakti poissa replikarooli kehittäjä-käsite.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun kangasta käsitteitä on seuraavissa artikkeleissa:

- [Palvelun kangasta palvelujen skaalattavuus](service-fabric-concepts-scalability.md)

- [Palvelun kangasta services jakaminen](service-fabric-concepts-partitioning.md)

- [Määrittämisestä ja hallinnasta tila](service-fabric-concepts-state.md)
