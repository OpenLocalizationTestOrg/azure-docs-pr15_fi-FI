<properties
   pageTitle="Palvelun kangasta projektin luomista seuraavat vaiheet | Microsoft Azure"
   description="Tässä artikkelissa on linkkejä core kehittäminen tehtäviä, palvelun kangasta varten"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Palvelun kangasta sovelluksen ja seuraavat vaiheet
Azure-palvelu kangasta-sovellus on luotu. Tässä artikkelissa kuvataan Ehostus projektin ja mahdolliset seuraavia vaiheita.

## <a name="your-application"></a>Sovelluksen
Jokaisen uuden sovelluksen sisältää sovelluksen projektin. Voi olla yksi tai kaksi muita projektit-palvelun valinnut tyypin mukaan.

### <a name="the-application-project"></a>Sovelluksen projektin
Sovelluksen project sisältää seuraavat tiedot:

- Palvelut, jotka muodostavat sovelluksen viittauksia joukko.

- Kaksi julkaista profiilit (paikallinen ja pilvi), joiden avulla voit säilyttää asetusten käyttäminen eri ympäristöissä – kuten klusterin päätepiste ja suoritetaanko päivityksen ominaisuuksissa liittyvät oletusarvoisesti asetukset.

- Kaksi sovelluksen parametritiedostot (paikallinen ja pilvi), joiden avulla voit säilyttää ympäristön kielikohtaiset sovellusten määrityksiä, kuten luoda palvelun osioiden määrää.

- Käyttöönoton komentosarja, joiden avulla voit ottaa käyttöön sovelluksen komentoriviltä tai automaattinen jatkuva integrointi ja käyttöönotto-myyntijakso osana.

- Sovellusluettelo, joka kuvaa sovelluksen. Voit etsiä luettelo ApplicationPackageRoot-kansiossa.

### <a name="stateless-service"></a>Tilattomien palvelu
Kun lisäät uuden tilattomien palvelun, Visual Studio Lisää palvelun projektin ratkaisu, joka sisältää jäsenruudussa tyyppi `StatelessService`. Palvelun kasvattaa paikallista muuttujaa laskuri.

### <a name="stateful-service"></a>Tilallisten palvelu
Kun lisäät uuden tilallisten palvelun, Visual Studio Lisää palvelun projektin ratkaisu, joka sisältää jäsenruudussa tyyppi `StatefulService`. Palvelun kasvattaa laskuri-sen `RunAsync` menetelmä ja tallentaa tuloksen `ReliableDictionary`.

### <a name="actor-service"></a>Toimija-palvelu
Voit lisätä uuden luotettava toimija ja Visual Studio Lisää ratkaisu kaksi projektia: toimija-projekti ja käyttöliittymän projektista.

Toimija Projectissa asetus menetelmät ja käytön laskuri arvo, joka on luotettavasti samanlainen sisällä toimija tila. Käyttöliittymän projektin tarjoaa, muiden palvelujen avulla voit käynnistää toimija käyttöliittymän.

### <a name="stateless-web-api"></a>Tilattomien verkko-Ohjelmointirajapinnan
Tilattomien verkko-Ohjelmointirajapinnan Projectissa on basic web-palvelu, joiden avulla voit avata ulkoiset asiakkaat sovellusta. Lisätietoja rakenteinen, projektin artikkelissa [palvelun kangasta verkko-Ohjelmointirajapinnan-palvelun OWIN isännöintipalvelussa itse](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>ASP.NET-core

Palvelun kangasta SDK sisältää ASP.NET Core samat erillinen ASP.NET Core projektien käytettävissä olevat mallit: Tyhjennä, [Verkko-Ohjelmointirajapinnan][aspnet-webapi], ja [Web-sovelluksen][aspnet-webapp].

## <a name="next-steps"></a>Seuraavat vaiheet
### <a name="create-an-azure-cluster"></a>Luo Azure klusteri
Palvelun kangasta SDK on paikallisen klusterin kehittämiseen. Klusterin luominen Azure-kohdassa [palvelu kangasta klusterin Azure-portaalista on määritetty][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Kokeile käyttöönotossa Azure maksutta osapuolen klustereiden kanssa

Jos haluat kokeilla käyttöönotto ja hallinta Azure sovellusten määrittäminen oman klustereiden ilman, voit käyttää maksuton [osapuolen klusteripalvelu](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Julkaise sovelluksesi Azure
Voit julkaista sovelluksen suoraan Visual Studio Azure klusteriin. Lisätietoja on ohjeartikkelissa [julkaiseminen sovelluksesi Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Palvelun kangasta Resurssienhallinnan avulla voit visualisoida yhteyttä klusterin
Palvelun kangasta Explorer on helppo tapa visualisointi klusteri, mukaan lukien käyttöön sovellukset ja rakenteesta. Lisätietoja on artikkelissa [havainnollistetaan usein yhteyttä klusterin palvelun kangasta Explorerilla][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Version ja palvelujen päivittäminen
Palvelun kangasta mahdollistaa riippumaton versiotiedot ja riippumaton palveluiden sovelluksen päivittäminen. Lisätietoja on artikkelissa [versiotiedot ja päivittäminen palvelujen][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Määrittää jatkuva integrointi Visual Studio ryhmän palvelujen kanssa
Lisätietoja siitä, miten voit määrittää jatkuva integrointiprosessin palvelun kangasta sovelluksen on artikkelissa [Configure jatkuva integrointi Visual Studio Team Services][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
