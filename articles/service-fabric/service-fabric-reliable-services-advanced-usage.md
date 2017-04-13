<properties
   pageTitle="Laajennettu luotettava palveluihin | Microsoft Azure"
   description="Lisätietoja palvelun kangasta luotettavia palveluja palvelujen joustavuutta kehittyneet käyttö."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Luotettavan palveluihin ohjelmointi mallin Lisäasetukset
Azure palvelun kangasta yksinkertaistaa kirjoittaminen ja luotettava tilattomien ja tilallisten palveluiden hallinta. Tässä oppaassa keskustella Lisäasetukset käytöt luotettavia palveluja saada lisätietoja ohjausobjekti ja joustavuutta palvelujen päälle. Ennen lukeminen tässä oppaassa tutustua [Ohjelmointi mallin luotettavia palveluja](service-fabric-reliable-services-introduction.md).

Tilallisten-ja tilattomien on kaksi ensisijainen tapahtuma pisteet Käyttäjäkoodi:

 - `RunAsync`on yleinen aloituskohtaa service-koodin.
 - `CreateServiceReplicaListeners`ja `CreateServiceInstanceListeners` on avaamista viestintä kuuntelijoita asiakkaan pyyntöihin.
 
Useimmat palvelut nämä kaksi pikakuvakkeet on riittävä. Vain harvoin tarkemmin palvelun elinkaari tarvitaan, kun muita Elinkaaritapahtumat ovat käytettävissä.

## <a name="stateless-service-instance-lifecycle"></a>Tilattomien palvelun esiintymän elinkaari

Tilattomien palvelun elinkaari on kuvattu yksinkertainen. Tilattomien palvelu vain voidaan avata, suljettu tai keskeytetty. `RunAsync`tilattomien palveluun suoritetaan, kun palvelun esiintymän avataan, ja kun palvelun esiintymän suljetaan tai keskeytetty peruutettu. 

Vaikka `RunAsync` pitäisi olla riittävän lähes kaikissa tapauksissa avoinna, Sulje ja tilattomien palvelu Keskeytä tapahtumat ovat käytettävissä myös:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync kutsutaan, kun tilattomien palvelun esiintymän on tarkoitus käyttää. Laajennettu palvelun alustus tehtävät voidaan aloittaa tällä hetkellä.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync kutsutaan siirryttäessä tilattomien palvelun esiintymän tilanteen sulkea. Näin voi käydä, kun palvelun koodi päivitetään, palvelun esiintymän siirretään vuoksi kuormituksen tasaamisen tai lyhytkestoisia virhe havaitaan. OnCloseAsync voidaan turvallisesti Sulje resursseja, Lopeta taustakäsittely, valmis tallennetaan ulkoinen tila tai sulkemaan muodostetut yhteydet.

- `void OnAbort()`
  OnAbort kutsutaan, kun tilattomien palvelun esiintymän on pakottamalla suljetaan. Tätä kutsutaan yleensä, kun pysyvä virhe havaitaan solmun tai palvelun kangasta luotettavasti ei voi hallita palvelun esiintymän elinkaari sisäinen virheiden vuoksi.

## <a name="stateful-service-replica-lifecycle"></a>Tilallisten palvelun replikan elinkaari

Tilallisten palvelun replikan elinkaari on paljon monimutkaisempi kuin tilattomien palvelun esiintymän. Lisäksi voi avata, sulkea ja keskeyttää tapahtumat, tilallisten palvelun replikan käy läpi rooli muuttuu sen elinkaaren aikana. Tilallisten palvelun replikan muuttuessa rooli `OnChangeRoleAsync` tapaus on:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync kutsutaan, kun tilallisten palvelun se muuttuu esimerkiksi rooli-ensisijaisen tai toissijaisen. Ensisijainen replikoiden annetaan kirjoitus-tila (voivat luoda ja kirjoittaa luotettava sivustokokoelmat). Toissijainen replikoiden annetaan (vain luettavissa olemassa olevan luotettavasta peräisin) lukutilaa. Useimmat työn tilallisten palvelu suoritetaan ensisijainen se. Toissijainen replikoiden voi suorittaa vain luku-kelpoisuuden, raportin luonti, mahdollisista ja muut vain luku-työt.

Tilallisten palvelun vain ensisijainen se on kirjoitusoikeus tilaan ja näin ollen on yleensä kun palvelua suorittavien todellinen työmäärä. `RunAsync` Menetelmä tilallisten palvelu suoritetaan vain, kun se tilallisten palvelu on ensisijainen. `RunAsync` Menetelmä peruutettu muuttuessa ensisijainen replikan roolin poispäin perus- ja sulje ja Keskeytä-tapahtumien aikana. 

Käyttämällä `OnChangeRoleAsync` tapahtuman voit suorittaa työn replikarooli samoin kuin roolin muutokset vastauksen mukaan.

Tilallisten palvelun tarjoaa myös saman neljä Elinkaaritapahtumat, tilattomien palveluna samalla tavalla ja käytä tapauksissa:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Seuraavat vaiheet
Monimutkaisemman aiheet, jotka liittyvät palvelun kangasta on seuraavissa artikkeleissa:

- [Tilallisten luotettava palveluiden määrittäminen](service-fabric-reliable-services-configuration.md)

- [Palvelun kangasta kunto esittely](service-fabric-health-introduction.md)

- [Järjestelmän kunto-raporttien käytön vianmääritys](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Palvelun kangasta klusterin Resurssienhallinta palveluiden määrittäminen](service-fabric-cluster-resource-manager-configure-services.md)
