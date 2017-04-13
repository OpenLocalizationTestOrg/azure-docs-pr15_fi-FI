<properties
   pageTitle="Tilallisten luotettava palvelujen diagnostiikka | Microsoft Azure"
   description="Diagnostiikan tilallisen luotettavia palveluja toiminnot"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostiikan tilallisen luotettavia palveluja toiminnot
Tilallisen luotettava Services StatefulServiceBase luokan tietokoneesta kuuluu äänimerkki [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) tapahtumia, jotka voidaan korjata palvelu, anna tiedot siitä, miten suorituksen toimii ja apua vianmäärityksessä.

## <a name="eventsource-events"></a>EventSource tapahtumat
EventSource tilallisen luotettava Services StatefulServiceBase luokan nimi on "Microsoft-ServiceFabric-palvelut". Tapahtuman tästä lähteestä tapahtumat näkyvät [Diagnostiikka tapahtumat](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) -ikkunassa, kun palvelun parhaillaan [aloittaessasi Visual Studiossa](service-fabric-debugging-your-application.md).

Esimerkkejä työkalut ja toiminnot, jotka auttavat kerääminen ja/tai EventSource tapahtumien tarkasteleminen ovat [PerfView](http://www.microsoft.com/download/details.aspx?id=28567)-, [Microsoft Azure diagnostiikka](../cloud-services/cloud-services-dotnet-diagnostics.md)- ja [Microsoft TraceEvent kirjastoon](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Tapahtumat

|Tapahtuman nimi|Tapahtumatunnus|Taso|Tapahtuman kuvaus|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Tiedoksi|Lähetetty palvelun RunAsync tehtävän käynnistyksen yhteydessä|
|StatefulRunAsyncCancellation|2|Tiedoksi|Kun palvelun RunAsync tehtävän peruutetaan lähetetty|
|StatefulRunAsyncCompletion|3|Tiedoksi|Lähetetty, kun palvelua RunAsync tehtävä on valmis|
|StatefulRunAsyncSlowCancellation|4|Varoitus|Jos palvelun RunAsync tehtävän kestää liian kauan peruutus lähetetty|
|StatefulRunAsyncFailure|5|Virhe|Kun palvelun RunAsync tehtävän ilmoittaa poikkeuksen lähetetty|

## <a name="interpret-events"></a>Tapahtumien tulkitseminen

StatefulRunAsyncInvocation, StatefulRunAsyncCompletion ja StatefulRunAsyncCancellation tapahtumat ovat käteviä palvelun writer ymmärtää elinkaari palvelu--sekä ajoituksen kun palvelu alkaa, peruutettu tai valmis. Tämä voi olla hyötyä, kun virheenkorjaus palveluongelmat tai palvelun elinkaari tietoja.

Palvelun kirjoittajat maksettava huomiota StatefulRunAsyncSlowCancellation ja StatefulRunAsyncFailure tapahtumat, koska ilmoittavat-palvelussa ongelmia.

StatefulRunAsyncFailure on lähetetty aina, kun palvelua RunAsync() tehtävän ilmoittaa poikkeuksen. Yleensä poikkeuksen osoittaa virheen tai ohjelmavirhe-palvelussa. Lisäksi poikkeuksen aiheuttaa palvelun epäonnistuu, niin se siirretään eri solmu. Tämä voi olla kallista toiminto ja viivyttää pyynnöt samalla, kun palvelua siirretään. Palvelun kirjoittajat olisi poikkeuksen syy ja rajoittaa sen, jos se on mahdollista.

StatefulRunAsyncSlowCancellation on lähetetty aina, kun peruutus pyyntö RunAsync tehtävän kestää kauemmin kuin neljä sekuntia. Jos palvelun kestää liian kauan peruutus, vaikuttaa käynnistettävä nopeasti toiseen solmun palvelun mahdollisuuden. Tämä saattaa vaikuttaa palveluun yleisen käytettävyyttä.
