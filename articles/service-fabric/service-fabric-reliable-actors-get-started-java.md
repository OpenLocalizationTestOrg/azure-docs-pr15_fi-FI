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
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/25/2016"
   ms.author="vturecek"/>

# <a name="getting-started-with-reliable-actors"></a>Luotettavan toimijoiden käytön aloittaminen

> [AZURE.SELECTOR]
- [C# Windows](service-fabric-reliable-actors-get-started.md)
- [Java Linux](service-fabric-reliable-actors-get-started-java.md)

Tässä artikkelissa kerrotaan Azure palvelun kangasta luotettava toimijoiden perusteet ja opastaa luominen ja käyttöönotto Java-yksinkertaisen luotettava toimija-sovelluksen.

## <a name="installation-and-setup"></a>Asennus ja määritys
Ennen kuin aloitat, varmista, että sinulla on tietokoneen määrittäminen palvelun kangasta-kehitysympäristö.
Jos haluat määrittää sen Siirry [aloittaminen Mac](service-fabric-get-started-mac.md) tai [Linux aloittaminen](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Peruskäsitteet
Aloita luotettava toimijoiden, tarvitset vain muutaman peruskäsitteet ymmärtää:

 * **Toimija-palvelun**. Luotettavan toimijoiden on pakattu luotettava palvelut, joita voidaan ottaa käyttöön palvelun kangasta-infrastruktuuria. Toimija esiintymät aktivoidaan nimetty palvelun esiintymän.
 
 * **Toimija rekisteröinti**. Kuin luotettava Services-luotettavia toimija-palvelu on rekisteröitävä palvelun kangasta suorituksenaikainen. Toimija-tyyppi on rekisteröitävä toimija suorituksenaikainen.
 
 * **Toimija-käyttöliittymän**. Toimija-liittymän käytetään erittäin kirjoitetun julkisen liittymän toimija määrittämiseen. Luotettavan toimija mallia, termejä toimija-liittymän määrittää viestit, jotka toimija ymmärtämään ja prosessi. Toimija-liittymän käyttää muita toimijat ja asiakassovelluksissa "" (asynkronisesti) viestien lähettämiseen toimijan. Luotettavan toimijoiden ottaa käyttöön useita liittymät.
 
 * **ActorProxy-luokka**. Asiakassovellukset käyttää ActorProxy luokan käynnistää menetelmiä tarjoamia toimija-liittymän kautta. ActorProxy-luokka on kaksi tärkeää toimintoja:
    * Nimeä tarkkuus: on löydä toimija klusterin (Etsi solmu nykyisessä klusterin).
    * Virheen käsittely: se uudelleen menetelmä ohjelmarakennekaaviossa ja ratkaista uudelleen, kun esimerkiksi epäonnistui, joka voi olla uudelleen toisaalla klusterin toimija edellyttää toimija sijainti.

Seuraavat säännöt, jotka koskevat toimija liittymät on mainitseminen:

- Toimija-liittymän menetelmiä ei voi myöskään määrittää.
- Toimija-liittymän menetelmistä ei saa sisältää ulos, ref tai valinnaisten parametrien.
- Yleinen liittymät ei tueta.

## <a name="create-an-actor-service"></a>Luo toimija-palvelu
Aloita luomalla uusi palvelu kangasta-sovellus. Linux palvelun kangasta SDK-paketissa sisältää Yeoman luontitoiminnon antamaan rakennustelineet palvelun kangasta sovelluksen tilattomien-palvelun kanssa. Aloita suorittamalla seuraavat Yeoman komento:

```bash
$ yo azuresfjava
```

Luo **Luotettava toimija-palvelun**ohjeiden mukaisesti. Tässä opetusohjelmassa nimi "HelloWorldActorApplication"-sovellus ja toimija "HelloWorldActor." Seuraavat rakennustelineet luodaan:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Luotettavan toimijoiden basic rakenneosat

Edempänä peruskäsitteet kääntäminen basic hakukyselyn luotettava toimija-palvelun.

### <a name="actor-interface"></a>Toimija-käyttöliittymä

Tämä sisältää toimija käyttöliittymän määrityksen. Tämän liittymän määrittää toimija sopimuksen, joka on jaettu toimija käyttöönoton ja asiakkaiden kutsuminen toimija, jotta yleensä tekee kannattaa määrittää sen paikkaa, jossa on erillinen toimija käyttöönoton ja voidaan jakaa useisiin muiden palveluiden tai asiakassovelluksissa.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Toimija-palvelu 
Tämä sisältää toimija käyttöönotto ja toimijan rekisteröintikoodin. Toimija-luokan toteuttaa toimija-liittymän. Tämä on missä oman toimija onko työnsä.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Toimija rekisteröinti

Toimija-palvelun rekisteröitävä palvelutyyppi palvelun kangasta suorituksen aikana. Järjestyksessä toimija-palveluun, jotta voit suorittaa toimija esiintymät toimija tyyppi on myös rekisteröitävä toimija-palvelussa. `ActorRuntime` Rekisteröintimenetelmä suorittaa toimisivat toimijat.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {
    
    public static void main(String[] args) throws Exception {
        
        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);
            
        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testiasiakas

Tämä on yksinkertainen koe asiakassovellus voit suorittaa erikseen toimijan palvelun testaaminen palvelun kangasta-sovelluksesta. Tässä on esimerkki, jossa ActorProxy avulla voidaan aktivoida ja yhteydenpito toimija esiintymät. Se tule käyttöön palveluun.

### <a name="the-application"></a>Sovelluksen 

Lopuksi sovelluksen pakkaa toimija-palvelu ja muut palvelut, joita voit lisätä myöhemmin yhdessä käyttöönottoa varten. Se sisältää *ApplicationManifest.xml* ja Aseta haltijan toimija-palvelupakettiin varten.

## <a name="run-the-application"></a>Suorita sovellus

Yeoman rakennustelineet sisältyy voivat laatia sovelluksen ja bash komentosarjojen otetaan käyttöön ja poistaa gradle komentosarja-sovelluksen käyttöön. Sovelluksen suorittamiseen muodostaa gradle sovellukseen seuraavasti:

```bash
$ gradle
```

Tämä tuottaa palvelun kangasta sovelluspaketin, joka otetaan käyttöön palvelun kangasta Azure CLI. Install.sh komentosarja sisältää tarvittavat Azure CLI komentoja, ottamaan sovelluspaketin. Suorita install.sh-komentosarjan, joka otetaan käyttöön:

```bask
$ ./install.sh
```
