<properties
   pageTitle="Luotettavan toimijoiden tapahtumien | Microsoft Azure"
   description="Palvelun kangasta luotettava toimijoiden tapahtumien yleistietoja."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/30/2016"
   ms.author="amanbha"/>


# <a name="actor-events"></a>Toimija tapahtumat
Toimija tapahtumien lisäämistapaa parhaiten työmäärään ilmoitusten lähettäminen toimija asiakkaille. Toimija tapahtumat on suunniteltu toimija asiakkaan viestintään ja ei voi käyttää toimija toimija viestintään.

Seuraava koodikatkelmat Näytä toimija tapahtumien käyttämisestä sovelluksessa.

Määritä käyttöliittymä, joka kuvaa toimija julkaisema tapahtumat. Tämän liittymän on peräisin `IActorEvents` käyttöliittymän. Menetelmistä argumenttien on oltava [tiedot sopimuksen voi sarjoittaa](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). Menetelmien on palautettava void, tapahtumana ilmoitukset on yksi tapa ja parhaat työmäärään.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```

Määritellä toimija-käyttöliittymän toimija julkaisema tapahtumat.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```

Asiakaspuolen, valitse Toteuta tapahtumakäsittelijä.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

Asiakaskoneen välityspalvelimen toimijan, joka julkaisee tapahtuman luominen ja sen tapahtumien tilaaminen.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

Failovers, jos toimija voi epäonnistua päälle prosessin eri tai solmu. Toimija-välityspalvelimen hallinnoi aktiivinen tilauksia ja tilaa niitä automaattisesti uudelleen. Voit hallita uudelleen tilauksen välin kautta `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Voit peruuttaa tilauksen, käytä `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Toimija, valitse Julkaise tapahtumat yksinkertaisesti päivittyvät näyttöön reaaliajassa. Jos tapahtumaan tilaajille toimijoiden suorituksenaikainen lähettää heille ilmoituksen.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```

## <a name="next-steps"></a>Seuraavat vaiheet
 - [Toimija reentrancy](service-fabric-reliable-actors-reentrancy.md)
 - [Toimija diagnostiikka- ja suorituskyvyn seurantaa](service-fabric-reliable-actors-diagnostics.md)
 - [Toimija API oppaat](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Esimerkki koodi](https://github.com/Azure/servicefabric-samples)
