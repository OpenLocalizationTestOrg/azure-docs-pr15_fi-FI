<properties
   pageTitle="Azure säilö palvelun johdanto | Microsoft Azure"
   description="Azure säilö-palvelun avulla on helppo luominen, määrittäminen ja hallinta klusterin näennäiskoneiden, joka on esimääritetty tai kontteihin pakattuja sovelluksia."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Azure säilö-palvelun esittely

Azure säilö-palvelun avulla yksinkertaisempi luoda ja hallita näennäiskoneiden, joka on esimääritetty tai kontteihin pakattuja sovelluksia klusterin ja määrittäminen. Tiedostossa käytetään optimoitu määritysten Suositut Avaa lähteen ajoittamisen ja tiedonsiirron työkaluja. Näin voit käyttää vanhoja taitojasi tai Piirrä suuria ja kasvava leipätekstiin yhteisön osaamisalueet, ottaa käyttöön ja hallita Microsoft Azure säilö-pohjaisten sovellusten yhteydessä.


![Azure säilö-palvelun avulla voit hallita useita isäntien Azure tai kontteihin pakattuja sovelluksia.](./media/acs-intro/acs-cluster.png)


Azure säilö-palvelun avulla voit varmistaa, että sovelluksen säilöt ovat täysin kannettavaan Docker container-muoto. Se tukee valittua Marathon sekä Ohjauskoneen/OS tai Docker Swarm myös niin, että voit skaalata säilöjen tuhansia tai jopa lähimpään kymmeneen tuhansia, nämä sovellukset.

Azure säilö-palvelun avulla voit hyödyntää yrityksen arvosanojen ominaisuuksia Azure-säilyttäen sovelluksen siirrettävyys – siirrettävyys tiedonsiirron kerrokset, kuten edelleen.

<a name="using-azure-container-service"></a>Azure säilö-palvelun avulla
-----------------------------

Tavoitteenamme Azure säilö-palvelun kanssa on säilön Isännöintiympäristö käyttämällä Avaa lähde-työkalut ja -tekniikoiden käyttävät aktiivisimmin asiakkaidemme tänään. Tämän vuoksi olemme Näytä vakio API päätepisteet varten valitut orchestrator (toimialueen Ohjauskoneen/OS tai Docker Swarm). Päätepisteen avulla voidaan hyödyntää ohjelmistoja, joka voi näiden päätepisteet puhuminen. Esimerkiksi kyseessä Docker Swarm päätepisteen, voit käyttää Docker komentorivivalitsimet interface (CLI). Toimialueen Ohjauskoneen/Käyttöjärjestelmä voit käyttää DCOS CLI.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Docker-klusterin luominen Azure säilö-palvelun avulla
-------------------------------------------------------

Aloita Azure säilö-palveluun, käyttöönottoa Azure säilö Service-klusterin (Etsi Azure-säilö palvelu)-portaalin kautta käyttämällä Azure Resurssienhallinta-malli ([Docker Swarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) tai [Ohjauskoneen/OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) tai [CLI](/documentation/articles/xplat-cli-install/). Annettu pikaopas malleja voi muokata sisältämään Lisää tai lisäasetusten Azure-määrityksen. Lisätietoja Azure säilö Service-klusterin ottamisesta käyttöön on artikkelissa [Azure säilö Service-klusterin käyttöönotto](container-service-deployment.md).

<a name="deploying-an-application"></a>Sovelluksen käyttöönotto
------------------------

Azure säilö-palvelu sisältää tiedonsiirron Docker Swarm tai Ohjauskoneen/OS valinta. Sovelluksen käyttöönotto määräytyy orchestrator valinta.

### <a name="using-dcos"></a>Toimialueen Ohjauskoneen/OS käyttäminen

Toimialueen Ohjauskoneen/Käyttöjärjestelmä on jaettu käyttöjärjestelmä Apache Mesos hajautettu järjestelmien ydin perusteella. Apache Mesos osoitteessa Apache ohjelmiston Foundation pidetään ja on lueteltu joitakin [suurimmista nimet IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) käyttäjinä ja osallistujat.

![Määritetty Swarm tekijöiden ja perustyylit Azure säilö-palvelu.](media/acs-intro/dcos.png)

Toimialueen Ohjauskoneen/OS ja Apache Mesos sisältävät näyttävää ominaisuusjoukko:

-   Luotettava

-   Vikasietoinen replikoida pää- ja slaves Apache ZooKeeper käyttäminen

-   Säilöjen Docker muotoiltujen tuki

-   Alkuperäisen eristystaso Linux säilöä sisältävällä tehtävien väliin

-   Multiresource ajoitus (muistin, suorittimen, levyn ja portit)

-   Java, Python ja C++ API uusien rinnakkain sovellusten kehittämiseen

-   Sivuston Käyttöliittymän klusterin tilan tarkasteleminen

Toimialueen Ohjauskoneen/OS Azure säilö-palvelu käytössä sisältää oletusarvoisesti ajoittamisessa työmääriä Marathon tiedonsiirron-ympäristössä. Toimialueen Ohjauskoneen/OS käyttöönottoa ACS kuuluva on kuitenkin Mesosphere Universe palvelut, jotka voidaan lisätä palvelun tällaisia ohjattu, Hadoop, Cassandra ja paljon muuta.

![Toimialueen Ohjauskoneen/OS Universe Azure säilö-palvelussa](media/dcos/universe.png)

#### <a name="using-marathon"></a>Marathon käyttäminen

Marathon on klusterin laajuinen alusta ja palveluiden cgroups--tai, jos Azure säilö palvelun Docker muotoiltujen säilöjen järjestelmä. Marathon on web-Käyttöliittymä, josta voit asentaa sovellukset. Voit käyttää tätä URL-osoitteessa, joka näyttää suunnilleen `http://DNS_PREFIX.REGION.cloudapp.azure.com` jossa DNS\_ETULIITE- ja ALUEASETUSTEN molemmat määritetään käyttöönoton aikana. Voit myös lisätä omia DNS-nimi. Saat lisätietoja suorittamisesta Marathon Internet-Käyttöliittymän säilön [säilö hallinnan web-Käyttöliittymä](container-service-mesos-marathon-ui.md).

![Marathon sovellusten luettelosta](media/dcos/marathon-applications-list.png)

Voit käyttää myös REST API varten Marathon yhteydessä. Asiakas-kirjastot, jotka ovat käytettävissä kunkin työkalun määrä on. Ne kattavat useilla kielillä--ja, voit käyttää mitä tahansa kieltä HTTP-protokolla. Lisäksi monet Suositut DevOps Työkalut tukevat Marathon. Tämä tarjoaa joustavuutta toimintoja ryhmän käsittelyyn liittyvistä Azure säilö Service-klusterin. Lisätietoja käynnissä säilön Marathon REST-Ohjelmointirajapinnalla avulla on artikkelissa [säilö hallinnan REST-ohjelmointirajapinnalla](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Docker Swarm käyttäminen

Docker Swarm on Docker alkuperäisen käyttövarmuuden lisäämiseksi. Koska Docker Swarm on vakio Docker API-työkalua, joka jo yhteydessä Docker daemon käyttää Swarm läpinäkyvä skaalata useita isäntien Azure säilö-palvelusta.

![Azure säilö palvelu, jossa on määritetty käyttämään toimialueen Ohjauskoneen/OS--jumpbox, tekijöiden ja perusmuodot.](media/acs-intro/acs-swarm2.png)

Tuetut hallintatyökalut Swarm klusterin säilöt ovat, mutta ei rajoitettu seuraavia:

-   Dokku

-   Kirjoita docker CLI ja Docker

-   Krane

-   Jenkins

<a name="videos"></a>Videot
------

Azure säilö Service (101) käytön aloittaminen:  

> [AZURE.VIDEO azure-container-service-101]

Sovellusten Azure säilö-palveluun (koontiversio 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
