<properties
   pageTitle="Palvelun kangasta luotettava toimijoiden aloittaminen | Microsoft Azure"
   description="Tässä opetusohjelmassa esitellään koskevan ilmoituksen luominen ja virheenkorjaus yksinkertainen toimija-pohjaisessa palvelussa käyttämällä palvelun kangasta luotettava toimijoiden käyttöönotto."
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
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Luotettavan toimijoiden käytön aloittaminen

> [AZURE.SELECTOR]
- [C# Windows](service-fabric-reliable-actors-get-started.md)
- [Java Linux](service-fabric-reliable-actors-get-started-java.md)

Tässä artikkelissa kerrotaan Azure palvelun kangasta luotettava toimijoiden perusteet ja opastaa luominen ja virheenkorjaus käyttöönotto Visual Studiossa yksinkertaisen luotettava toimija-sovelluksen.

## <a name="installation-and-setup"></a>Asennus ja määritys
Ennen kuin aloitat, varmista, että palvelun kangasta kehitysympäristö, tietokoneen määrittäminen.
Jos haluat määrittää sen, katso lisätietoja [kehittäminen ympäristön määrittäminen](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Peruskäsitteet
Aloita luotettava toimijoiden, tarvitset vain muutaman peruskäsitteet ymmärtää:

 * **Toimija-palvelun**. Luotettavan toimijoiden on pakattu luotettava palvelut, joita voidaan ottaa käyttöön palvelun kangasta-infrastruktuuria. Toimija esiintymät aktivoidaan nimetty palvelun esiintymän.
 
 * **Toimija rekisteröinti**. Kuin luotettava Services-luotettavia toimija-palvelu on rekisteröitävä palvelun kangasta runtime. Toimija-tyyppi on rekisteröitävä toimija runtime.
 
 * **Toimija-käyttöliittymän**. Toimija-liittymän käytetään erittäin kirjoitetun julkisen liittymän toimija määrittämiseen. Luotettavan toimija mallin terminologiaa toimija-liittymän määrittää viestit, jotka toimija ymmärtämään ja prosessi. Toimija-liittymän käyttää muita toimijat ja asiakassovelluksissa "" (asynkronisesti) viestien lähettämiseen toimijan. Luotettavan toimijoiden ottaa käyttöön useita liittymät.
 
 * **ActorProxy-luokka**. Asiakassovellukset käyttää ActorProxy luokan käynnistää menetelmiä tarjoamia toimija-liittymän kautta. ActorProxy-luokka on kaksi tärkeää toimintoja:
    * Nimeä tarkkuus: on löydä toimija klusterin (Etsi solmu nykyisessä klusterin).
    * Virheen käsittely: se yrittää menetelmä ohjelmarakennekaaviossa ja ratkaista uudelleen, kun esimerkiksi epäonnistui, joka voi olla uudelleen toisaalla klusterin toimija edellyttää toimija sijainti.

Seuraavat säännöt, jotka koskevat toimija liittymät on mainitseminen:

- Toimija-liittymän menetelmiä ei voi myöskään määrittää.
- Toimija-liittymän menetelmistä ei saa sisältää ulos, ref tai valinnaisten parametrien.
- Yleinen liittymät ei tueta.

## <a name="create-a-new-project-in-visual-studio"></a>Uuden projektin luominen Visual Studiossa
Kun olet asentanut palvelun kangasta Työkalut Visual Studio, voit luoda uuden projektityypit. Uuden projektin ovat **Cloud** -luokassa **Uusi projekti** -valintaikkunan.


![Palvelun kangasta Työkalut Visual Studio - uusi projekti][1]

Valitse seuraavassa valintaikkunassa, jonka haluat luoda projektin tyyppi.

![Palvelun kangasta project-mallit][5]

HelloWorld projektin käytetään palvelun kangasta luotettava toimijoiden palvelu.

Luotuasi ratkaisun pitäisi näkyä seuraava rakenne:

![Palvelun kangasta projektirakenne][2]

## <a name="reliable-actors-basic-building-blocks"></a>Luotettavan toimijoiden basic rakenneosat

Tyypillinen luotettava toimijoiden-ratkaisun koostuu kolme projektia:

* **Sovelluksen projektin (MyActorApplication)**. Tämä on projekti, jota paketteja kaikki yhdessä palvelut käyttöönottoa varten. Se sisältää *ApplicationManifest.xml* ja PowerShell-komentosarjojen sovelluksen hallintaan.

* **Käyttöliittymän projektin (MyActor.Interfaces)**. Tämä on projekti, joka sisältää toimija liitännän määritys. MyActor.Interfaces Projectissa voit määrittää liittymät, jotka käyttävät toimijat ratkaisun. Toimija-liittymät voidaan määrittää minkä tahansa nimen projektin kuitenkin Rajapinta määrittelee toimija sopimuksen, joka on jaettu toimija käyttöönotto ja asiakkaiden kutsuminen toimija, niin yleensä kannattaa määrittää kokoonpanossa, joka on erillinen toimija käyttöönoton ja voidaan jakaa useisiin muihin projekteihin.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **Toimija-palvelun projektin (MyActor)**. Tämä on project määrittää palvelun kangasta-palvelun, joka isännöi toimija suorittaminen. Se sisältää toimija soveltaminen. Toimija-käyttöönoton on luokka, joka johdetaan perustyyppiä `Actor` ja toteuttaa liittymän, jotka on määritetty MyActor.Interfaces Projectissa. Toimijaluokka on myös pantava täytäntöön konstruktoria, joka hyväksyy `ActorService` esiintymän ja `ActorId` ja välittää niitä kantaluvun `Actor` luokka. Näin konstruktori riippuvuuden lisäämisen ympäristö riippuvuuksista.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

Toimija-palvelun rekisteröitävä palvelutyyppi palvelun kangasta suorituksen aikana. Järjestyksessä toimija-palveluun, jotta voit suorittaa toimija esiintymät toimija tyyppi on myös rekisteröitävä toimija-palvelussa. `ActorRuntime` Rekisteröintimenetelmä suorittaa toimisivat toimijat.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Jos sinulla on vain yksi toimija määrittely käynnistetään uusi projekti Visual Studiossa, rekisteröinnin sisältyy oletusarvoisesti tunnus, jolla Visual Studio Luo. Jos määrität muiden toimijoiden-palvelussa, sinun on toimija rekisteröinnin lisääminen käyttämällä:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [AZURE.TIP] Palvelun kangasta toimijoiden runtime tietokoneesta kuuluu äänimerkki, jotkin [tapahtumien ja suorituskyvyn laskureita, jotka liittyvät toimija tavoista](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). He ovat käteviä diagnostiikka- ja suorituskyvyn seurantaa.


## <a name="debugging"></a>Virheenkorjaus

Visual Studio palvelun kangasta-Työkalut tukevat virheenkorjaus paikallisessa tietokoneessa. Voit aloittaa muistin istunnon osumalla F5-näppäintä. Visual Studio muodostaa (tarvittaessa) paketit. Se myös ottaa käyttöön sovelluksen paikallisen klusterin palvelun kangasta ja liittää virheenkorjaus.

Käyttöönoton aikana voit tarkastella edistymisen **kohde** -ikkunassa.

![Palvelun kangasta muistin tulostusikkunassa][3]


## <a name="next-steps"></a>Seuraavat vaiheet
 - [Luotettavan toimijoiden käyttämisestä palvelun kangasta-ympäristössä](service-fabric-reliable-actors-platform.md)
 - [Toimija-tilan hallinta](service-fabric-reliable-actors-state-management.md)
 - [Toimijan elinkaari ja roskaa sivustokokoelman](service-fabric-reliable-actors-lifecycle.md)
 - [Toimija API oppaat](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Esimerkki koodi](https://github.com/Azure/servicefabric-samples)


<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
