<properties
   pageTitle="Luotettavan toimijoiden muistiinpanot toimija Kirjoita Sarjatoiminto | Microsoft Azure"
   description="Tässä artikkelissa kuvataan basic vaatimukset voi sarjoittaa luokat, jotka voidaan määrittää palvelun kangasta luotettava toimijoiden hyötyä ja liittymät määrittäminen"
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
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="notes-on-service-fabric-reliable-actors-type-serialization"></a>Palvelun kangasta luotettava toimijoiden muistiinpanot Kirjoita Sarjatoiminto


Kaikki menetelmät, tulostyypit kummassakin menetelmässä toimija-käyttöliittymän palauttama tehtävät ja objekteja, jotka on tallennettu toimija tilan hallinta argumenttien on oltava [Tiedot sopimuksen voi sarjoittaa](https://msdn.microsoft.com/library/ms731923.aspx)... Tämä koskee myös [toimija tapahtuman liittymät](service-fabric-reliable-actors-events.md#actor-events)määritelty menetelmistä argumentit. (Toimija tapahtuman menetelmien aina palautettava void.)

## <a name="custom-data-types"></a>Mukautetun tietotyypit

Tässä esimerkissä toimija liittymästä määrittää menetelmän, joka palauttaa kutsutaan mukautetun tietotyypin `VoicemailBox`.

```csharp
public interface IVoiceMailBoxActor : IActor
{
    Task<VoicemailBox> GetMailBoxAsync();
}
```

Rajapinta on impelemented mukaan toimija, joka käyttää tilan Vastuuhenkilö tallentamiseen `VoicemailBox` objekti:

```csharp
[StatePersistence(StatePersistence.Persisted)]
public class VoiceMailBoxActor : Actor, IVoicemailBoxActor
{
    public VoiceMailBoxActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<VoicemailBox> GetMailboxAsync()
    {
        return this.StateManager.GetStateAsync<VoicemailBox>("Mailbox");
    }
}

```

Tässä esimerkissä `VoicemailBox` objekti on muuntaa sarjaksi, kun:
 - Objektin lähetetään toimija esiintymä ja soittajan välillä.
 - Objekti on tallennettu tilan hallinta, missä se on samanlainen levylle ja replikoida solmujen.
 
Luotettavan toimija framework käyttää DataContract Sarjatoiminto. Tämän vuoksi mukautetut tieto-objektit ja niiden jäsenten on mainitseminen **DataContract** ja **DataMember** -ominaisuuksilla tarpeen mukaan

```csharp
[DataContract]
public class Voicemail
{
    [DataMember]
    public Guid Id { get; set; }

    [DataMember]
    public string Message { get; set; }

    [DataMember]
    public DateTime ReceivedAt { get; set; }
}
```

```csharp
[DataContract]
public class VoicemailBox
{
    public VoicemailBox()
    {
        this.MessageList = new List<Voicemail>();
    }

    [DataMember]
    public List<Voicemail> MessageList { get; set; }

    [DataMember]
    public string Greeting { get; set; }
}
```

## <a name="next-steps"></a>Seuraavat vaiheet
 - [Toimijan elinkaari ja roskaa sivustokokoelman](service-fabric-reliable-actors-lifecycle.md)
 - [Toimija ajastimet ja muistutuksia](service-fabric-reliable-actors-timers-reminders.md)
 - [Toimija tapahtumat](service-fabric-reliable-actors-events.md)
 - [Toimija reentrancy](service-fabric-reliable-actors-reentrancy.md)
 - [Toimija monimuotoisuus ja kuviot olio-rakenne](service-fabric-reliable-actors-polymorphism.md)
 - [Toimija diagnostiikka- ja suorituskyvyn seurantaa](service-fabric-reliable-actors-diagnostics.md)
