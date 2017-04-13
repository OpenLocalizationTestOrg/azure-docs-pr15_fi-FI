<properties
   pageTitle="Aloittaminen luotettavia palveluja | Microsoft Azure"
   description="Johdanto Microsoft Azure palvelun kangasta-sovelluksen luominen tilattomien ja tilallisten-palvelujen kanssa."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="vturecek"/>

# <a name="get-started-with-reliable-services"></a>Luotettavan palveluiden käytön aloittaminen

> [AZURE.SELECTOR]
- [C# Windows](service-fabric-reliable-services-quick-start.md)
- [Java Linux](service-fabric-reliable-services-quick-start-java.md)

Tässä artikkelissa kerrotaan Azure palvelun kangasta luotettavia palveluja perusteet ja opastaa luominen ja käyttöönotto yksinkertaisia luotettavan palvelusovelluksen kirjoitettu Java.

## <a name="installation-and-setup"></a>Asennus ja määritys
Ennen kuin aloitat, varmista, että sinulla on tietokoneen määrittäminen palvelun kangasta-kehitysympäristö.
Jos haluat määrittää sen Siirry [aloittaminen Mac](service-fabric-get-started-mac.md) tai [Linux aloittaminen](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Peruskäsitteet
Aloita luotettavia palveluja, tarvitset vain muutaman peruskäsitteet ymmärtää:

 - **Palvelutyyppi**: Tämä on palvelun käyttöönoton. Voit kirjoittaa luokka, joka on määritetty `StatelessService` ja jokin muu koodi tai käyttää sekä nimi ja versionumero, riippuvuudet.

 - **Nimetty esiintymää**: suorittamaan palvelun luot nimetyn esiintymät palvelutyyppi, paljon kuin luot luokkatyyppi objektin esiintymät. Palvelun esiintymät ovat itse asiassa yhteyttä palvelun luokan, voit kirjoittaa objektin toteutuksiin. 

 - **Isännöinti**: luot nimetyn palvelun esiintymät on suoritettava isäntä sisällä. Palvelun host on vain jos palvelun esiintymät suorittamisen prosessi.

 - **Palvelun rekisteröinti**: rekisteröinti Tuo kaikki yhteen. Palvelutyypin rekisteröitävä palvelun kangasta Runtimen avulla voit luoda sen esiintymät kangasta palvelun isännöinti, suorita.  

## <a name="create-a-stateless-service"></a>Luo tilattomien palvelu

Aloita luomalla uusi palvelu kangasta-sovellus. Linux palvelun kangasta SDK-paketissa sisältää Yeoman luontitoiminto antamaan rakennustelineet palvelun kangasta sovelluksen tilattomien-palvelun kanssa. Aloita suorittamalla seuraavat Yeoman komento:

```bash
$ yo azuresfjava
```

Luo **Luotettava tilattomien palvelun**ohjeiden mukaisesti. Tässä opetusohjelmassa nimeksi sovelluksen "HelloWorldApplication" ja "HelloWorld"-palvelun. Tulos sisältää kansioihin `HelloWorldApplication` ja `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-the-service"></a>Toteuta palvelua

Avaa **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Tähän luokkaan määrittää palvelutyypin ja voi suorittaa koodia. Service API on kaksi pikakuvakkeet koodi:

 - Avoimia aloituskohdan menetelmää, jota kutsutaan `runAsync()`, jossa voit alkaa suoritetaan toiminnoista, kuten pitkään suoritettavien Laske toiminnoista.

```java
@Override
protected CompletableFuture<?> runAsync() {
    ...
}
```

 - Viestintä aloituskohtaa, johon voidaan liittää viestintä pinoa valinta. Tämä on mistä voit aloittaa vastaanottaminen pyynnöt käyttäjät tai muista palveluista.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Keskitytään Tässä opetusohjelmassa `runAsync()` aloituskohdan menetelmää. Tämä on heti aloittaa koodin suorittamisen.

### <a name="runasync"></a>RunAsync

Platform kutsuu tätä menetelmää, kun erillisen palvelun on sijoitettu ja suorittaminen. Avaa/Sulje jakson palvelun esiintymän voi ilmetä monta kertaa elinaika-palvelun kautta kokonaisuudessaan. Näin voi käydä, mukaan lukien eri syistä:

- Järjestelmä siirtää resurssien tasaamisen palvelun kopioita.
- Koodin tapahtuvat virheet.
- Sovelluksessa tai järjestelmässä on päivitetty.
- Pohjana olevan laitteen ilmenee käyttökatkosta.

Tämä tiedonsiirron hallitaan palvelun kangasta pitämään palvelun käytettävissä ja oikein tasapuolisesti mukaan.

#### <a name="cancellation"></a>Peruuttaminen

Se on tärkeä, lähdekoodin `runAsync()` lopettaa suorittamisen, kun palvelua kangasta tiedon. `CompletableFuture` Palauttama `runAsync()` peruutetaan, kun palvelua kangasta vaatii palvelun pysäytä suoritus. Seuraavassa esimerkissä käsittelemisestä peruutus-tapahtuma 

```java
    @Override
    protected CompletableFuture<?> runAsync() {

        CompletableFuture<?> completableFuture = new CompletableFuture<>();
        ExecutorService service = Executors.newFixedThreadPool(1);
        
        Future<?> userTask = service.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try
                {
                   logger.log(Level.INFO, this.context().serviceName().toString());
                   Thread.sleep(1000);
                }
                catch (InterruptedException ex)
                {
                    logger.log(Level.INFO, this.context().serviceName().toString() + " interrupted. Exiting");
                    return;
                }
            }
         });
 
        completableFuture.handle((r, ex) -> {
            if (ex instanceof CancellationException) {
                userTask.cancel(true);
                service.shutdown();
            }
            return null;
        });
 
        return completableFuture;
   }
``` 

### <a name="service-registration"></a>Palvelun rekisteröinti

Palvelun lajien rekisteröitävä palvelun kangasta runtime. Palvelutyyppi on määritetty `ServiceManifest.xml` ja palveluluokan, joka sisältää `StatelessService`. Palvelun rekisteröinti suoritetaan prosessin Päähakusana-kohdassa. Tässä esimerkissä prosessin Päähakusana-kohta on `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    } 
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration: {0}", ex.toString());
        throw ex;
    }
}
```

## <a name="run-the-application"></a>Suorita sovellus

Yeoman rakennustelineet sisältyy voivat laatia sovelluksen ja bash komentosarjojen otetaan käyttöön ja poistaa gradle komentosarja-sovelluksen käyttöön. Sovelluksen suorittamiseen muodostaa gradle sovellukseen seuraavasti:

```bash
$ gradle
```

Tämä tuottaa palvelun kangasta sovelluspaketin, joka otetaan käyttöön palvelun kangasta Azure CLI. Install.sh komentosarja sisältää tarvittavat Azure CLI komentoja, ottamaan sovelluspaketin. Suorita install.sh-komentosarjan, joka otetaan käyttöön:

```bask
$ ./install.sh
```
