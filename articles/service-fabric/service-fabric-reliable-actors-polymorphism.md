<properties
   pageTitle="Polymorfismia luotettava toimijoiden puitteissa | Microsoft Azure"
   description="Luo hierarkiat .NET ja tyypit luotettava toimijoiden puitteissa toimintoja ja API määritelmät uudelleen."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/07/2016"
   ms.author="seanmck"/>

# <a name="polymorphism-in-the-reliable-actors-framework"></a>Luotettavan toimijoiden puitteissa monimuotoisuus

Luotettavan toimijoiden framework avulla voit muodostaa toimijoiden monilla samalla tavalla, jota käyttää objektipohjaisten rakenne. Yksi näistä tavoista on polymorfismia, jonka avulla tyypit ja liittymät Lisää periytyvät generalized vanhemmat. Periytyminen luotettava toimijoiden puitteissa seuraa yleensä muutamia muita rajoituksia .NET-malli.

## <a name="interfaces"></a>Liityntäkohdat

Luotettavan toimijoiden framework edellyttää Määritä vähintään yksi liitäntä pantava täytäntöön toimija-tyypin mukaan. Tämän liittymän sillä muodostetaan välityspalvelimen luokka, joka voidaan käyttää asiakkaat oman toimijoiden kanssa kommunikoimiseen. Liittymät voit Peri muiden liittymät, kunhan jokaisen käyttöliittymässä, jotka on toteutettu toimija-tyypin mukaan ja kaikki sen vanhemmat kädessä johdettu IActor. IActor on platform määrittämät perus liittymän toimijat. Näin ollen muodoilla perinteinen polymorfismia esimerkki voi näyttää seuraavankaltaiselta:

![Rajapinta-muodon toimijoiden hierarkia][shapes-interface-hierarchy]


## <a name="types"></a>Tyypit

Voit myös luoda hierarkian toimija tyypit, joka johdetaan perus toimija-luokka, joka tarjoaa ympäristö. Kyseessä muotoja, voit joutua on kanta `Shape` tyyppi:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```

Subtypes `Shape` voit ohittaa menetelmien kantaluku.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```

Huomautus `ActorService` toimija-tyypin määrite. Tämän määritteen kertoo luotettava toimija puitteissa, että se luo automaattisesti isännöimiseen tällaista toimijoiden palvelu. Haluat ehkä luoda perustyyppiä, joka on tarkoitettu ainoastaan toimintojen jakaminen alatyypit ja käytetään aina betonin toimijoiden vahvistamiseen. Tällöin sinun on käytettävä `abstract` avainsanoja, kun haluat ilmaista, että luot koskaan toimija tyypin perusteella.


## <a name="next-steps"></a>Seuraavat vaiheet

- Katso, [kuinka luotettava toimijoiden framework hyödyntää palvelun kangasta platform](service-fabric-reliable-actors-platform.md) antamaan luotettavuutta, skaalattavuus ja yhtenäinen tila.
- Lisätietoja [toimijan elinkaari](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
