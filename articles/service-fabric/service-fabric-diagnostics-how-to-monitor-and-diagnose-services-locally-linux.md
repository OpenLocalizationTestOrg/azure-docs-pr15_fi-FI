<properties
   pageTitle="Paikallisesti valvoa ja vianmäärityksen services kirjoitettu Azure palvelun kangasta | Microsoft Azure"
   description="Lue, miten ja vianmäärityksen palvelujen kirjoitettu Microsoft Azure palvelun kangasta paikallista kehittämistä koneeseen."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/24/2016"
   ms.author="subramar"/>


# <a name="monitor-and-diagnose-services-in-a-local-machine-development-setup"></a>Valvo ja vianmäärityksen services paikallisesta kehittäminen asetuksissa


> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
- [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)

Valvonta, tunnistus, ohjelmistossa ja vianmääritys Salli services mahdollisimman vähän häiriöitä käyttäjäkokemuksen, jatka. Seuranta- ja diagnostiikka ovat kriittisiä todellinen käyttöön tuotantoympäristössä. Antamista samalla mallin aikana palvelujen kehittäminen varmistaa, että diagnostiikan myyntijakso toimii, kun siirrät tuotantoympäristössä. Palvelun kangasta on helppo palvelun kehittäjille toteuttamisesta vianmääritys, joka toimii saumattomasti yksittäisen tietokoneen paikalliseen kehittäminen asetukset ja todellisen tuotannon klusterin asetukset.


## <a name="debugging-service-fabric-java-applications"></a>Palvelun kangasta Java-sovellusten virheenkorjaus

Java-sovellusten [useita kirjaaminen kehysten](http://en.wikipedia.org/wiki/Java_logging_framework) ovat käytettävissä. Koska `java.util.logging` on oletusasetus JRE, jossa käytetään [github esimerkeissä koodi](http://github.com/Azure-Samples/service-fabric-java-getting-started).  Seuraavat keskustelun kerrotaan, miten voit määrittää `java.util.logging` framework. 
 
Java.util.logging avulla voit ohjata sovelluksen lokit muistin, tuloste virtaa, tiedostot tai sockets. Kullekin nämä asetukset ovat oletusarvon käsittelytoimintoja jo antanut puitteissa. Voit luoda `app.properties` tiedoston määrittäminen sovelluksesi voit ohjata kaikki lokit paikallisen tiedoston tiedoston käsittelijä. 

Seuraavat koodikatkelman on esimerkki: 

```java 
handlers = java.util.logging.FileHandler
 
java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log             
```

Johda kansio `app.properties` tiedoston on oltava olemassa. Sen jälkeen `app.properties` tiedosto luodaan, saatat joutua muokkaamaan myös merkintä-kohdan komentosarjan `entrypoint.sh` - `<applicationfolder>/<servicePkg>/Code/` kansio-ominaisuuden määrittäminen `java.util.logging.config.file` , `app.propertes` tiedoston. Merkinnän pitäisi näyttää samalta kuin seuraavassa koodikatkelman:

```sh 
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```
 
 
Määritysten tuloksena on kerätty pyörivien tavalla lokit `/tmp/servicefabric/logs/`. **%U** ja **%g** sallivat useita tiedostoja luotaessa tiedostonimet mysfapp0.log, mysfapp1.log ja niin edelleen. Oletusarvon mukaan Jos ei ole käsittelijä on määritetty erikseen, konsolin käsittelyä on rekisteröity. Voi tarkastella lokit-kohdassa /var/log/syslog syslog.
 
Lisätietoja [github esimerkeissä koodi](http://github.com/Azure-Samples/service-fabric-java-getting-started).  



## <a name="next-steps"></a>Seuraavat vaiheet
Sama jäljitys-koodi sovelluksen lisätään toimii myös sovelluksesi Azure-klusterissa Diagnostiikka. Tutustu seuraaviin artikkeleihin, keskustella työkalujen eri valitsimet ja kuvataan, miten voit määrittää ne seuraavalla.
* [Azure diagnostiikka lokien kerääminen](service-fabric-diagnostics-how-to-setup-lad.md)
