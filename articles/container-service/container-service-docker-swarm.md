<properties
   pageTitle="Azure säilö säilö hallinnan kanssa Docker Swarm | Microsoft Azure"
   description="Ota käyttöön säilöjen Docker Swarm Azure säilö-palvelussa"
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, säilöt-mikro-palveluja, Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-with-docker-swarm"></a>Säilön johdolle Docker Swarm

Docker Swarm on ympäristö käyttöönoton tai kontteihin pakattuja työmääriä Docker isäntien Poolisen joukkoa. Docker Swarm käytetään alkuperäisen Docker Ohjelmointirajapinnan. Työnkulun hallinta säilöt Docker Swarm on lähes samoin kuin mitä olisi yksittäisen säilö isännän. Tässä tiedostossa on yksinkertaisia esimerkkejä käyttöönotto tai kontteihin pakattuja työmääriä Azure säilö palvelun esiintymässä Docker Swarm. Katso Lisää kattavat ohjeet Docker Swarm, [Docker Swarm Docker.com käyttöön](https://docs.docker.com/swarm/).

Tässä asiakirjassa harjoitusten edellytykset:

[Swarm-klusterin luominen Azure säilö-palvelu](container-service-deployment.md)

[Yhdistä Swarm klusterin Azure säilö-palvelussa](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Uuden säilön käyttöönotto

Jos haluat luoda uuden säilön Docker Swarm, käytä `docker run` komennon (varmistetaan, että olet avannut SSH-tunnelin, kuten yllä edellytykset perustyyleissä). Tässä esimerkissä luodaan-säilön `yeasy/simple-web` kuva:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Kun säilö on luotu käyttämällä `docker ps` palauttamaan tietoja säilö. Huomaa seuraavat että, joka isännöi säilön Swarm-agentti näy:


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Voit nyt käyttää sovellus, jossa on käynnissä tässä säilössä kautta Swarm agentti kuormituksen julkinen DNS-nimi. Voit etsiä tiedot Azure-portaalissa:  


![Käy todellisia tulokset](media/real-visit.jpg)  

Oletusarvoisesti kuormituksen on portit 80, Avaa 8080 ja 443. Jos haluat muodostaa yhteyden porttiin, sinun on Avaa kyseistä porttia Azure kuormituksen Agent-ryhmän.

## <a name="deploy-multiple-containers"></a>Ottaa käyttöön useita säilöt

Kun useita säilöjen on käynnistetty, suorittamalla "docker Suorita" useita kertoja, voit käyttää `docker ps` komento nähdäksesi, joka isännöi säilöt ovat käytössä. Seuraavassa esimerkissä kolme säilöt ovat tasaisesti yli kolme Swarm käyttäjäagenttien:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Ottaa käyttöön säilöjen avulla Docker laatia

Voit laatia Docker automatisoimaan käyttöönotto ja määritys useita säilöjä. Voit tehdä varmistaa, että suojattu runko (SSH) tunnelin on luotu ja että DOCKER_HOST muuttuja on määritetty (katso yllä vaatimat).

Luo paikalliseen järjestelmään docker compose.yml-tiedosto. Käytä tätä [malli](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Suorita `docker-compose up -d` Aloita säilö ominaisuuksissa:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Lopuksi luettelon käynnissä säilöjen palautetaan. Luettelossa näkyvät säilöjen, jotka on otettu käyttöön käyttämällä Docker voit kirjoittaa:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Luonnollisesti, voit käyttää `docker-compose ps` tarkistaa vain määritelty säilöjen lisääminen `compose.yml` tiedosto.

## <a name="next-steps"></a>Seuraavat vaiheet

[Lisätietoja Docker Swarm](https://docs.docker.com/swarm/)
