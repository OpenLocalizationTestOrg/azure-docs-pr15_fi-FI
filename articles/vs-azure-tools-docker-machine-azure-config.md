<properties
   pageTitle="Luo Docker isännät Azure Docker koneen kanssa | Microsoft Azure"
   description="Tässä artikkelissa kuvataan Docker tietokoneen docker isännät luominen Azure käyttö."
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="mlearned" />

# <a name="create-docker-hosts-in-azure-with-docker-machine"></a>Luo Docker isännät Azure Docker tietokoneen kanssa

[Docker](https://www.docker.com/) säilöjen edellyttää host käynnissä docker daemon AM.
Tässä ohjeaiheessa kerrotaan, miten [docker tietokoneen](https://docs.docker.com/machine/) -komennon avulla voit luoda uuden Linux VMs määritetty Docker-daemon Azure käynnissä. 

**Huomautus:** 
- *Tässä artikkelissa määräytyy docker tietokoneen versio 0.7.0 tai uudempi*
- *Windows-säilöjen tueta docker tietokoneen kautta lähitulevaisuudessa*

## <a name="create-vms-with-docker-machine"></a>Luo VMs Docker tietokoneen kanssa

Luo docker host VMs Azure-tietokannassa, `docker-machine create` -komennon avulla `azure` ohjaimen. 

Azure ohjain on tilauksen käyttämällä. Voit käyttää [Azure CLI](xplat-cli-install.md) tai [Azure Portal](https://portal.azure.com) hakemiseen Azure-tilauksen. 

**Azure-portaalissa**
- Valitse vasemmanpuoleisessa siirtymisruudussa tilaukset ja kopioi Tilaustunnus.

**Azure CLI käyttäminen**
- Kirjoita ```azure account list``` ja kopioi tilauksen tunnus.

Kirjoita `docker-machine create --driver azure` asetukset ja oletusarvot.
Näet myös lisätietoja [Docker Azure ohjaimen ohjeissa](https://docs.docker.com/machine/drivers/azure/) . 

Seuraava esimerkki perustuu oletusarvot yhteydessä, mutta se voit myös avata internet-yhteyden AM 80 porttiin. 

```
docker-machine create -d azure --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> --azure-open-port 80 mydockerhost
```

## <a name="choose-a-docker-host-with-docker-machine"></a>Valitse docker isäntä docker tietokoneen kanssa
Kun olet isäntä docker machine merkinnän, voit määrittää oletusarvon host suoritettaessa docker komennot.
##<a name="using-powershell"></a>PowerShellin avulla

```powershell
docker-machine env MyDockerHost | Invoke-Expression 
```

##<a name="using-bash"></a>Käyttämällä Bash

```bash
eval $(docker-machine env MyDockerHost)
```

Voit nyt suorittaa docker komentoja määritetyn isännän vastaan

```
docker ps
docker info
```

## <a name="run-a-container"></a>Suorita säilö

Isännällä, joka on määritetty voit suorittaa yksinkertaisen verkkopalvelimeen ja testaa, onko isäntä on määritetty oikein.
Seuraavassa on vakio nginx kuvaa, määritä se olisi kuuntelemaan porttia 80 ja, jos host AM käynnistyy, säilö on uudelleen myös (`--restart=always`). 

```bash
docker run -d -p 80:80 --restart=always nginx
```

Tulosteen pitäisi näyttää seuraavanlaiselta:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
83f52fbfa5f8: Pull complete
fa664caa1402: Pull complete
Digest: sha256:12127e07a75bda1022fbd4ea231f5527a1899aad4679e3940482db3b57383b1d
Status: Downloaded newer image for nginx:latest
25942c35d86fe43c688d0c03ad478f14cc9c16913b0e1c2971cb32eb4d0ab721
```

## <a name="test-the-container"></a>Testaa säilö

Tutkia käynnissä säilöjen käyttämällä `docker ps`:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                         NAMES
d5b78f27b335        nginx               "nginx -g 'daemon off"   5 minutes ago       Up 5 minutes        0.0.0.0:80->80/tcp, 443/tcp   goofy_mahavira
```

Ja tarkista käynnissä säilö, kirjoita `docker-machine ip <VM name>` Etsi IP-osoitteet selaimessa:

```
PS C:\> docker-machine ip MyDockerHost
191.237.46.90
```

![Käynnissä olevat ngnix säilö](./media/vs-azure-tools-docker-machine-azure-config/nginxsuccess.png)

##<a name="summary"></a>Yhteenveto
Docker tietokoneen kanssa voit helposti valmistella docker isännät Azure-tietokannassa yksittäisiä docker Host (isäntä)-vahvistukset varten.
Tuotannon isännöinnin säilöjä artikkelissa [Azure säilö-palvelu](http://aka.ms/AzureContainerService)

Visual Studio .NET Core sovellusten kehittämiseen, katso [Docker Tools for Visual Studio](http://aka.ms/DockerToolsForVS)
