<properties
   pageTitle="Azure säilö palvelun säilö hallinnan REST-Ohjelmointirajapinnalla | Microsoft Azure"
   description="Säilöjen käyttöön Azure säilö palvelun Mesos-klusterin Marathon REST-Ohjelmointirajapinnalla avulla."
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

# <a name="container-management-through-the-rest-api"></a>Säilön hallinnan REST-Ohjelmointirajapinnalla

Toimialueen Ohjauskoneen/OS on ympäristö käyttöönotto ja skaalausta liitetty toiminnoista, kun abstracting pohjana olevan laitteen. Toimialueen Ohjauskoneen/OS päälle on kehys, joka hallitsee ajoitus ja Laske työmääriä suorittamista.

Vaikka kehysten ovat käytettävissä monet Suositut toiminnoista, tässä asiakirjassa kerrotaan siitä, miten voit luoda ja skaalata säilö ominaisuuksissa Marathon avulla. Ennen kuin käytät näitä esimerkkejä kautta, sinun on Ohjauskoneen/OS klusteriin, joka on määritetty Azure säilö-palvelussa. Tarvitset myös etäyhteyksien tähän klusteriin. Lisätietoja näistä kohteista on seuraavissa artikkeleissa:

- [Azure säilö Service-klusterin käyttöönotto](container-service-deployment.md)
- [Yhteyden muodostaminen Azure säilö Service-klusterin](container-service-connect.md)

Kun olet muodostanut yhteyden Azure säilö Service-klusterin, voit käyttää Ohjauskoneen/OS ja Aiheeseen liittyvät REST API http://localhost:local portin kautta. Tämän asiakirjan Esimerkeissä oletetaan, että ovat tunneling porttiin 80. Esimerkiksi Marathon päätepisteen tavoittaa `http://localhost/marathon/v2/`. Lisätietoja eri ohjelmointirajapinnan on artikkeleissa [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) ja [Chronos API](https://mesos.github.io/chronos/docs/api.html)Mesosphere ohjeissa ja [Mesos ajoituksen API](http://mesos.apache.org/documentation/latest/scheduler-http-api/)Apache ohjeissa.

## <a name="gather-information-from-dcos-and-marathon"></a>Tietojen kerääminen Ohjauskoneen/OS ja Marathon

Ennen kuin otat säilöjen Ohjauskoneen/OS klusterin, kerätä tietoja Ohjauskoneen/OS klusterin, kuten nimet ja nykyisen tilan Ohjauskoneen/OS agenttien vuoksi. Voit tehdä kyselyn `master/slaves` Ohjauskoneen/OS REST-Ohjelmointirajapinnalla päätepiste. Jos kaikki sujuu hyvin, näet luettelon Ohjauskoneen/OS tekijöiden ja useita ominaisuuksia kunkin.

```bash
curl http://localhost/mesos/master/slaves
```

Käytä nyt Marathon `/apps` päätepisteen nykyinen sovellus ominaisuuksissa Ohjauskoneen/OS-klusterin tarkistavan. Jos kyseessä on uusi klusterin, näet sovellusten tyhjän taulukon.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Docker muotoiltujen säilön käyttöönotto

Voit ottaa käyttöön Docker muotoiltujen säilöjen Marathon kautta, joka kuvaa tarkoitettu käyttöönoton JSON-tiedoston avulla. Seuraavassa esimerkissä otetaan käyttöön Nginx säilö, säilön porttia 80 toimialueen Ohjauskoneen/OS-agentti portti sidonta 80. Huomaa, että 'slave_public' 'acceptedResourceRoles'-ominaisuudeksi on määritetty. Tämä otetaan käyttöön säilö agentti julkiselle agentti asteikko määrittäminen.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Käyttöönotossa Docker muotoiltujen säilön JSON-tiedostoa tai käyttää valmista [Azure säilö-palvelun esittely](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)on annettu. Tallentaa sen käytettävissä olevaan sijaintiin. Seuraavaksi ottamaan säilö suorittamalla seuraavan komennon. Määritä JSON-tiedoston nimi.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Tulos on seuraavanlainen:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Nyt, jos testaat Marathon sovellusten, uusi sovellus näkyy tulos.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Skaalaa oman säilöt

Voit käyttää myös Marathon Ohjelmointirajapinnan skaalata tai skaalata sovelluksen käyttöönotoissa. Edellisen esimerkin käyttöön esiintymän sovelluksen. Oletetaan, että skaalata tämän sovelluksen kolme esiintymiä ulos. Voit tehdä JSON-tiedoston luominen käyttämällä JSON seuraava teksti ja tallenna se käytettävissä olevaan sijaintiin.

```json
{ "instances": 3 }
```

Suorita seuraava komento skaalata hakemuksen.

>[AZURE.NOTE] URI on http://localhost/marathon/v2/apps/ ja sovelluksen skaalata tunnus. Jos käytät Nginx otoksen, joka on annettu tässä, URI olisi http://localhost/marathon/v2/apps/nginx.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Lopuksi kyselyn Marathon päätepisteen sovellukset. Näet, että on nyt kolme Nginx säilöjä.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>PowerShellin käyttäminen tämän Harjoitus: Marathon REST API vuorovaikutus PowerShellin avulla

Voit suorittaa nämä samoja toimintoja käyttämällä Windows-järjestelmän PowerShell-komennoilla.

Voit kerätä tietoja Ohjauskoneen/OS-klusterin agentti nimet ja agentti tila, kuten varten suorittamalla seuraavan komennon.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Voit ottaa käyttöön Docker muotoiltujen säilöjen Marathon kautta, joka kuvaa tarkoitettu käyttöönoton JSON-tiedoston avulla. Seuraavassa esimerkissä otetaan käyttöön Nginx säilö, säilön porttia 80 toimialueen Ohjauskoneen/OS-agentti portti sidonta 80.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Luo JSON-tiedosto tai käyttää valmista [Azure säilö-palvelun esittely](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)on annettu. Tallentaa sen käytettävissä olevaan sijaintiin. Seuraavaksi ottamaan säilö suorittamalla seuraavan komennon. Määritä JSON-tiedoston nimi.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Voit käyttää myös Marathon Ohjelmointirajapinnan skaalata tai skaalata sovelluksen käyttöönotoissa. Edellisen esimerkin käyttöön esiintymän sovelluksen. Oletetaan, että skaalata tämän sovelluksen kolme esiintymiä ulos. Voit tehdä JSON-tiedoston luominen käyttämällä JSON seuraava teksti ja tallenna se käytettävissä olevaan sijaintiin.

```json
{ "instances": 3 }
```

Suorita seuraava komento skaalata hakemuksen.

> [AZURE.NOTE] URI on http://localhost/marathon/v2/apps/ ja sovelluksen skaalata tunnus. Jos käytössäsi on annettu Nginx otosten tähän, URI olisi http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Seuraavat vaiheet

- [Lue lisätietoja Mesos HTTP-päätepisteet]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Lue lisätietoja Marathon REST-Ohjelmointirajapinnalla]( https://mesosphere.github.io/marathon/docs/rest-api.html).
