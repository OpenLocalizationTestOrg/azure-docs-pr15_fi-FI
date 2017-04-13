<properties
   pageTitle="Määrittäminen ja hallinta valtion | Microsoft Azure"
   description="Suorituskykyongelman määrittäminen ja hallinta-palvelun kangasta palvelun tila"
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

# <a name="service-state"></a>Palvelun tilan
**Palvelun tilan** viittaa tiedot, jotka palvelu edellyttää toimivat. Se sisältää rakenteet ja muuttujia, palvelun lukee ja kirjoittaa toimimaan.

Harkitse yksinkertainen Laskin palvelun, kuten. Tämä palvelu on kahden luvun ja palauttaa niiden summa. Tämä on pelkästään tilattomien palvelu, jota ei ole liitetty tietoja.

Voit nyt sama Laskimen, mutta lisäksi tietojenkäsittely summa, se on myös tapa, se on laskettu viimeisen summa, joka palauttaa. Tämä palvelu on nyt tilallisen--se sisältää joitakin tilaan, jossa se kirjoittaa (kun se laskee uuden summa) ja lukujen (kun se palauttaa viimeisen laskettu).

Azure palvelun kangasta ensimmäisen palvelun nimi on tilattomien palvelu. Toisen palvelun kutsutaan tilallisten palvelu.

## <a name="storing-service-state"></a>Palvelun tilan tallentaminen
Tilan voidaan joko externalized tai sijaitsevat yhtä koodilla, joka on käsittelevistä tila. Valtion externalization tapahtuu yleensä käyttämällä ulkoisessa tietokannassa tai säilö. Laskimen tämän esimerkin Tämä voi johtua SQL-tietokanta, johon nykyisen tulos on tallennettu taulukon. Jokainen pyyntö summan laskemiseen suorittaa päivitys rivi.

Tilan voi olla myös yhteyteen koodi, joka käsittelee koodi. Tilallisten services-palvelun kangasta on suunniteltu tämän mallin avulla. Palvelun kangasta on infrastruktuuri, varmista, että tässä tilassa on erittäin käytettävissä ja jos vikasietoisia.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja palvelun kangasta käsitteitä on seuraavissa artikkeleissa:

- [Palvelun kangasta palvelujen saatavuus](service-fabric-availability-services.md)

- [Palvelun kangasta palvelujen skaalattavuus](service-fabric-concepts-scalability.md)

- [Palvelun kangasta services jakaminen](service-fabric-concepts-partitioning.md)
