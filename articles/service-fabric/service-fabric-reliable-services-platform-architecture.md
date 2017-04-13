<properties
   pageTitle="Luotettavan arkkitehtuuri | Microsoft Azure"
   description="Yleistä luotettava Service-arkkitehtuuri tilallisten ja tilattomien varten."
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Arkkitehtuuri tilallisten ja tilattomien luotettava varten.

Azure kangasta luotettavaa palvelun voi olla tilallisten tai tilattomien. Kunkin palvelutyypin suoritetaan tietyn arkkitehtuuri. Tässä artikkelissa kuvattuja nämä arkkitehtuureihin.
Artikkelissa [luotettava yleistä](service-fabric-reliable-services-introduction.md) lisätietoja tilallisten ja tilattomien välisiä eroja.

## <a name="stateful-reliable-services"></a>Tilallisten luotettavia palveluja

### <a name="architecture-of-a-stateful-service"></a>Arkkitehtuuri tilallisten palvelun
![Arkkitehtuuri kaavion tilallisten palvelun](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Tilallisten luotettavaa palvelun

Tilallisten luotettavaa palvelun voivat saada StatefulService tai StatefulServiceBase luokka. Sekä peruste kyseisten luokkien tarjoamien palvelun kangasta. Ne tarjoavat tasoisia tuki- ja otetaan tilallisten palvelun käyttöliittymä, jossa palvelun kangasta--ja osallistua palveluna palvelun kangasta klusterin kuluessa.

StatefulService johdetaan StatefulServiceBase. StatefulServiceBase on palvelujen joustavampaa, mutta vaatii palvelun kangasta sisäiset enemmän tietoja.
Saat yksityiskohtia kirjoittamisen services StatefulService ja StatefulServiceBase luokkien avulla saat lisätietoja [luotettavaa palvelun yleiskatsaus](service-fabric-reliable-services-introduction.md) ja [luotettavaa palvelun erikoishaun käyttö](service-fabric-reliable-services-advanced-usage.md) .

Elinaika ja palvelun käyttöönoton roolin hallinta sekä peruste luokat. Palvelun käyttöönoton voi ohittaa virtual menetelmiä joko perus luokan, jos palvelun käyttöönoton työn alla palvelun käyttöönoton--elinkaari näiden pisteisiin tai jos se haluaa viestintä listener objektin luominen. Huomaa, että vaikka palvelun käyttöönoton voi toteuttaa omassa viestintä listener objektin paljastaa ICommunicationListener, yllä olevassa kaaviossa viestintä kuuntelua täytäntöön palvelun kangasta--kuin palvelun käyttöönoton käyttää viestintä kuuntelija, jotka on toteutettu palvelun kangasta.

Tilallisten luotettava palvelu käyttää luotettavaa tilan Vastuuhenkilö hyödyntää luotettava sivustokokoelmat. Luotettavan sivustokokoelmat ovat paikallisten tietojen rakenteet erittäin palvelu--, joka on käytettävissä olevia, ne ovat aina käytettävissä palvelun failovers riippumatta. Kullakin virhelajilla on luotettava sivustokokoelman on toteutettu luotettava tilan tarjoajan.
Lisätietoja luotettava sivustokokoelmat on artikkelissa [luotettava sivustokokoelmien yleiskuvaus](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Luotettavan tilan hallinta ja tila-palveluntarjoajat

Luotettavan tilan hallinta on objekti, joka hallitsee luotettava tilan tarjoajat. Siinä on toiminnot, jotka luominen, poistaminen, Luetteloi ja varmista, että luotettava tila-palvelut ovat pysyviä ja käytettävissä. Luotettavan tilan tarjoajan-esiintymä edustaa pysyviä ja erittäin käytettävissä tietorakenne, kuten sanastoa tai jonon esiintymä.

Luotettavan tilan kunkin palvelun paljastaa käyttöliittymään, jota käytetään tilallisten palvelu Käsittele luotettava tila-palvelussa. Esimerkiksi IReliableDictionary käytetään liittymään luotettava sanasto, kun IReliableQueue käytetään liittymään luotettava jonossa kanssa. Kaikkien luotettava tilan palveluntarjoajien Toteuta IReliableState liittymää.

Luotettavan tilan hallinta on käyttöliittymään nimeltä IReliableStateManager, joka mahdollistaa sen tilallisten-palvelusta. Liityntäkohdat luotettava tilan tarjoajan palautetaan IReliableStateManager kautta.

Luotettavan tilan hallinta käyttää laajennuksen arkkitehtuuri niin, että uudenlaisia luotettava sivustokokoelmat voi olla kytketty oikein dynaamisesti.

Luotettavan sanasto ja luotettava jono on suunniteltu tehokas, versiotietoja eroavuus kaupan käyttöönoton yhteydessä.

### <a name="transactional-replicator"></a>Tapahtumien Monistus

Tapahtumien Monistus osa on vastuussa siitä, että kaikki replikat palvelua (eli luotettava tilan hallinta ja luotettava sivustokokoelmat tila)-palvelun tilaa ovat yhdenmukaisia. Se myös varmistaa, että siinä on samanlainen lokiin. Luotettavan tilan hallinta liittymät kanssa tapahtumien Monistus yksityinen järjestelmä, jonka kautta.

Tapahtumien Monistus käyttää protokolla vaihtamaan tilaa palvelun esiintymän muiden replikoiden kanssa niin, että kaikki replikat on ajan tasalla olevat tiedot.

Tapahtumien Monistus käyttää lokia jatkuvat tilatietoihin niin, että tilatiedot SOPIMUSKOHDASSA prosessin tai solmu kaatuu. Log-liittymän on yksityinen järjestelmä.

### <a name="log"></a>Lokitiedoston

Log-komponentti on tehokas pysyvä säilö, joka voi optimoida kirjoittaminen pyörivä tai SSD levyjä.  Kirjaudu rakenne on pysyvästä tallennuspaikasta (siis kiintolevyillä) on käynnissä olevat tilallisten palvelun solmujen paikallinen. Näin pienen viiveet suurempia ja suuri siirtonopeuden remote pysyvän säilön, joka ei ole paikallisen solmun verrattuna.

Log-komponentti käyttää useista lokitiedostoista. Tällä solmu laajuinen jaetun lokitiedoston, joka replikoita käyttää siinä voi viive pienin ja suurin nopeus projektiin tilatiedot. Oletusarvon mukaan jaettu lokin sijoitetaan palvelun kangasta solmu työn hakemiston, mutta se voidaan määrittää myös kohta toisesta sijainnista, Ihannetapauksessa levyllä varattu jaetun sisäänkirjautumista varten. Palvelun replikoissa on myös erillinen lokitiedostoon ja erillinen lokin sijoitetaan palvelun työn hakemistossa. Ei ole järjestelmä määrittämään oma log sijoittaa eri sijainnissa.

Jaetun loki on sen tilan lisätietoja siirtymä alue erillinen lokitiedosto ollessa lopullinen kohde, johon se on samanlainen. Tämän rakenteen tilatiedot on kirjoitettu ensin jaetun lokitiedostoon ja sitten purjeveneestä siirretty erillinen lokitiedostoon taustalla. Tällä tavalla jaetun lokiin kirjoitus on pienin viive- ja suurin nopeus joka sallii palvelun edetä nopeammin.

Lukee ja kirjoituksia jaetun lokiin tehdään suoraan IO Esijaetut levytilaa jaetun lokitiedoston, kautta. Anna tilaa ja erillinen lokit asemassa optimaalisen käytön, erillinen lokitiedosto luodaan lyhyet NTFS-tiedostona. Huomaa, että tämä sallii overprovisioning levytilaa ja käyttöjärjestelmän näyttää paljon levytilaa kuin käytetään käyttämällä erillinen lokitiedostot.

Loki kirjoitetaan lukuun ottamatta lokiin mahdollisimman vähän-tila-käyttöliittymä ydin tilan-ohjain. Kirjaudu tarjota suorittamalla kuin ydin tilan-ohjain suurin suorituskyvyn kaikkiin palveluihin, jotka käyttävät sitä.

Saat lisätietoja lokitiedoston [Määrittäminen tilallisten luotettavia palveluja](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Tilattomien luotettavaa palvelun

### <a name="architecture-of-a-stateless-service"></a>Arkkitehtuuri tilattomien palvelun
![Arkkitehtuuri kaavion tilattomien palvelun](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Tilattomien luotettavaa palvelun

Tilattomien palvelun käyttöotot johdettu StatelessService tai StatelessServiceBase-luokasta. StatelessServiceBase-luokan avulla joustavampi kuin StatelessService-luokka.
Elinaika ja palvelun roolin hallinta sekä peruste luokat.

Palvelun käyttöönoton voi ohittaa virtual menetelmiä joko perus luokan, jos palvelulla palvelun--elinkaari näiden pisteisiin tehtävää tai jos se haluaa viestintä listener objektin luominen. Huomaa, että palvelu voi toteuttaa omassa viestintä listener objektin paljastaa yllä olevassa kaaviossa ICommunicationListener, vaikka viestintä kuuntelua täytäntöön palvelun kangasta kuin kyseisen palvelun käyttöönoton käyttää viestintä kuuntelija, jotka on toteutettu palvelun kangasta.

Saat yksityiskohtia kirjoittamisen services StatelessService ja StatelessServiceBase luokkien avulla saat lisätietoja [luotettavaa palvelun yleiskatsaus](service-fabric-reliable-services-introduction.md) ja [luotettavaa palvelun erikoishaun käyttö](service-fabric-reliable-services-advanced-usage.md) .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja palvelun kangasta:

[Luotettavan palvelun yleiskatsaus](service-fabric-reliable-services-introduction.md)

[Pika-aloitusopas](service-fabric-reliable-services-quick-start.md)

[Luotettavan sivustokokoelmien yleiskuvaus](service-fabric-reliable-services-reliable-collections.md)

[Luotettavan palvelun erikoishaun käyttö](service-fabric-reliable-services-advanced-usage.md)

[Luotettavan palvelun määritykset](service-fabric-reliable-services-configuration.md)  
