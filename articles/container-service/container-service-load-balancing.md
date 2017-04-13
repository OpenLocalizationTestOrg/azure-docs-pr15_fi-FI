<properties
   pageTitle="Lataa saldo säilöjen Azure säilö Service-klusterin | Microsoft Azure"
   description="Lataa useita säilöjen Azure säilö Service-klusterin yli saldo."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Säilöjen, mikro-palveluja, Ohjauskoneen/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Lataa saldo säilöjen Azure säilö Service-klusterin

Tässä artikkelissa tutustutaan oleva sisäinen kuormituksen luomisesta Ohjauskoneen/OS hallitun Azure säilö palvelu Marathon kg. Tämän avulla voit skaalata sovellustesi vaakasuunnassa. Se myös avulla voit hyödyntää julkisista ja yksityisistä agentti klustereiden asettamalla kuormituksen tasoitusmääritykset julkisen klusterin ja sovelluksen-säilöt yksityinen klusterin.

## <a name="prerequisites"></a>Edellytykset

[Ota Azure säilö palvelun esiintymän](container-service-deployment.md) orchestrator sisältötyyppi Ohjauskoneen/OS ja [Varmista, että asiakkaan muodostaa yhteyttä klusterin](container-service-connect.md). 

## <a name="load-balancing"></a>Kuormituksen

Olemme muodostetaan säilö palvelun klusterissa on kaksi kuormituksen tasaamisen kerrokset: 

  1. Azure kuormituksen on julkinen pikakuvakkeet (niistä, käyttäjien osumien). Tämä on annettu automaattisesti Azure säilö-palvelu ja on oletusarvoisesti käyttävät porttia 80 ja 443 8080 näyttää.
  2. Marathon kuormituksen (kg marathon) reitittää saapuvia pyyntöjä säilö esiintymät, kyseiset palvelupyynnöt. Olemme skaalata säilöt, jotka tarjoavat Microsoftin WWW-palvelun, kuten marathon kg sopeutuu dynaamisesti. Tämä kuormituksen anneta oletusarvoisesti säilö-palvelussa, mutta se on helppo asentaa.

## <a name="marathon-load-balancer"></a>Marathon kuormituksen

Marathon kuormituksen telakoit dynaamisesti itse, jonka olet ottanut säilöjen perusteella. Kannattaa myös joustavat säilön tai - agentti menetyksen, jos näin tapahtuu, Apache Mesos vain käynnistää säilö muualla ja marathon kg mukauttaa.

Asenna Marathon kuormituksen, voit käyttää joko Ohjauskoneen/käyttöjärjestelmän web-Käyttöliittymän tai komentorivin.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Asenna Marathon kg käyttämällä Ohjauskoneen/OS Web-Käyttöliittymä

  1. Valitse "Universe"
  2. Etsi "Kg Marathon"
  3. Valitse "Asenna"

![Asennuksen marathon-kg Ohjauskoneen/OS Web-liittymän kautta](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Asenna Marathon kg Ohjauskoneen/OS CLI käyttäminen

Jälkeen asentamalla Ohjauskoneen/OS CLI ja varmistaa, että voit muodostaa yhteyttä klusterin asiakkaan tietokoneesta varten suorittamalla seuraavan komennon:

```bash
dcos package install marathon-lb
```

Tämä komento asentaa automaattisesti kuormituksen julkisen tekijöiden klusterin.

## <a name="deploy-a-load-balanced-web-application"></a>Ottaa käyttöön kuormituksen saapuva verkkosovellus

Nyt, kun on marathon kg-paketti, emme voi ottaa käyttöön sovelluksen-säilö, joka on haluat ladata saldo. Tässä esimerkissä on otetaan käyttöön yksinkertainen verkkopalvelin käyttämällä asetukset ovat seuraavat:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Määritä `HAProxy_0_VHOST` ja edustajasi kuormituksen täydellinen toimialuenimi. Tämä on lomake `<acsName>agents.<region>.cloudapp.azure.com`. Jos luot säilö Service-klusterin nimellä esimerkiksi `myacs` alueen `West US`, FQDN olisi `myacsagents.westus.cloudapp.azure.com`. Voit myös poistaa etsiä hakemalla kuormituksen "agentti" nimen kanssa, kun etsit resursseja resurssiryhmän, jonka loit [Azure portal](https://portal.azure.com)säilö-palvelun kautta.
  * Määrittää servicePort porttiin > = 10 000. Tämä ilmaisee palvelu, joka suoritetaan säilössä--marathon kg käyttää tätä tunnistavan palvelut, joita se olisi saldo yli.
  * Määritä `HAPROXY_GROUP` otsikko, "ulkoinen".
  * Määritä `hostPort` 0. Tämä tarkoittaa, että Marathon satunnaisesti varata käytettävissä portin.
  * Määritä `instances` luotavan esiintymien määrän. Voit aina skaalata nämä ylös ja alas myöhemmin.

On noing se, että Marathon otetaan käyttöön yksityinen klusterin oletusarvon mukaan tämä tarkoittaa yllä käyttöönoton vain olevan käytettävissä kautta kuormituksen, joka on yleensä olemme haluavat toiminnan.

### <a name="deploy-using-the-dcos-web-ui"></a>Käyttämällä Ohjauskoneen/OS Web-Käyttöliittymän käyttöönotto

  1. Käy http://localhost/marathon Marathon sivulla (kun olet määrittänyt [SSH tunnelin](container-service-connect.md) ja sitten`Create Appliction`
  2. Valitse `New Application` valintaikkuna napsauttamalla `JSON Mode` oikeassa yläkulmassa
  3. Liitä edellä JSON editori
  4. Valitse`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Ottaa käyttöön Ohjauskoneen/OS CLI käyttäminen

Ottaa käyttöön Ohjauskoneen/OS CLI sovellusta ainoastaan kopioi edellä JSON tiedostoon, jota kutsutaan `hello-web.json`, ja suorita:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure kuormituksen

Oletusarvon mukaan Azure kuormituksen paljastaa portit 80, 8080 ja 443. Jos käytät kirjoitettujen portit (kuten on myös edellä olevassa esimerkissä) ja valitse ei ole enää sinun tarvitse tehdä mitään. Sinun pitäisi osumien oman agentti kuormituksen 's FQDN--ja aina, kun päivitetään, valitset kolme web-palvelimia pyöreän pöydän tavalla. Jos käytät eri portin, haluat lisätä pyöreän pöydän sääntö ja näytteenottimen portti, jota käytit kuormituksen. Voit tehdä tämän [Azure CLI](../xplat-cli-azure-resource-manager.md), komentoihin `azure network lb rule create` ja `azure network lb probe create`. Voit tehdä myös tämän Azure-portaalissa.


## <a name="additional-scenarios"></a>Lisää skenaarioita

Tilanne, jossa käytät eri toimialueiden voit näyttää erilaisia palveluita voi olla. Esimerkki:

mydomain1.com -> Azure LB:80 -> marathon-lb:10001 -> mycontainer1:33292  
mydomain2.com -> Azure LB:80 -> marathon-lb:10002 -> mycontainer2:22321

Tätä lisätietoa [virtual isännät](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/), johon voi liittää tietyn marathon-kg polkuihin toimialueet.

Voit vaihtoehtoisesti näyttää eri portit ja uudelleen ne takana marathon kg oikea-palveluun. Esimerkki:

Azure lb:80 -> marathon-lb:10001 -> mycontainer:233423  
Azure lb:8080 -> marathon-lb:1002 -> mycontainer2:33432


## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja käytössä [marathon kg](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/)ohjeissa Ohjauskoneen/OS.
