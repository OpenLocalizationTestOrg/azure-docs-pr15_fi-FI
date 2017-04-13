<properties
   pageTitle="Docker käyttäminen swarm Azure-aloittaminen"
   description="Luo käyttäjäryhmä, VMs Docker AM-tunniste ja swarm avulla voit luoda Docker-klusterin käsitellään."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="01/04/2016"
   ms.author="rasquill"/>

# <a name="how-to-use-docker-with-swarm"></a>Voit käyttää docker swarm

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Tässä ohjeaiheessa esitellään erittäin yksinkertaisia voi luoda swarm hallitun klusterin Azure kanssa [swarm](https://github.com/docker/swarm) [docker](https://www.docker.com/) avulla. Se luo neljä näennäiskoneiden Azure jokin edustajana swarm esimies ja kolme docker isännät klusterin osana. Kun olet valmis, voit käyttää swarm klusterin ja aloita sitten docker käyttämisestä. Lisäksi service management (asm)-tilan käyttäminen artikkelista Azure CLI-puhelut. 

> [AZURE.NOTE] Tässä aiheessa käytetään docker swarm ja Azure CLI *ilman* **docker machine** haluat näyttää, miten eri työkalujen toimivat yhdessä, mutta ovat itsenäisten. **docker tietokoneessa** on **--swarm** valitsimet, joiden avulla voit lisätä suoraan solmujen swarm **docker tietokoneen** avulla. Esimerkki [docker tietokoneen](https://github.com/docker/machine) ohjeissa. Jos **docker tietokoneen** käynnissä vastaan Azure VMs vastattu, katso [docker-koneen kanssa Azure käyttämisestä](virtual-machines-linux-docker-machine.md).

## <a name="create-docker-hosts-with-azure-virtual-machines"></a>Luo docker isännät Azuren näennäiskoneiden kanssa

Tässä ohjeaiheessa Luo neljä VMs, mutta voit määrittää haluamasi numero. Soita seuraavia toimintoja * &lt;salasanan&gt; * korvataan valitsemasi salasana.

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

Kun olet valmis sinun pitäisi nähdä Azure VMs **azure AM luettelon** avulla:

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## <a name="installing-swarm-on-the-swarm-master-vm"></a>Asennuksen swarm swarm perustyylin AM

Tässä ohjeaiheessa käyttää [säilö malli docker asennuksen swarm asiakirjat](https://github.com/docker/swarm#1---docker-image) --, mutta voit myös SSH **swarm perustyyli**. Tämän mallin **swarm** ladataan eli käynnissä swarm docker säilö. Alla on suorittaa tämän vaiheen *etäyhteyden Microsoftin kannettavan tietokoneen käyttämällä docker-* yhteyden **swarm perustyyli** AM ja kerro sen klusterin tunnus luominen-komento, **swarm luominen**. Klusteritunnus on, miten **swarm** löytää swarm-ryhmän jäsenet. (Voit myös Kloonaa säilö ja luoda itse, joka antaa täydet oikeudet ja ota käyttöön virheenkorjaus.)

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

Viimeinen rivi on; Klusteritunnus kopioi se toiseen sijaintiin, koska käyttää sitä liityttäessä solmu VMs swarm perustyyliin "swarm" Luo uudelleen. Tässä esimerkissä Klusteritunnus on **36731c17189fd8f450c395db8437befd**.

> [AZURE.NOTE] Vain, jos haluat näkyvän, poista, on käytössä paikallinen docker Microsoftin asennuksen muodostaa **swarm perustyyli** AM Azure ja opastusta **swarm perustyyli** ladata, asentaa ja suorita **luominen** -komento, joka palauttaa Microsoftin klusterin id, jolla Microsoft käyttää myöhemmin etsiminen tarkoitukseen.
<!-- -->
> Vahvista tämä suorittaa `docker -H tcp://` * &lt;hostname&gt; * ` images` luettelon säilö prosessit **swarm perustyyli** tietokoneeseen ja toinen solmu vertailun (koska olemme suoritettiin swarm edellisen komennon **--rm** valitsin, säilön poistettiin, kun se on valmis, käyttämällä **docker ps - a** ei palauta mitään).:


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $
<P />
>Jos olet tottunut käyttämään **docker**, tiedät, että muiden solmujen merkintöjä ei ole, koska ole kuvia olet ladannut ja vielä suoritettu.

## <a name="join-the-node-vms-to-our-docker-cluster"></a>Solmun VMs liittäminen meidän docker klusteri

Kunkin solmun luettelo käyttämällä Azure-CLI päätepisteen tiedot. Alla on vakiomuotoa **swarm-solmu-1** docker isännän saadakseen solmun docker portti.

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK


Käyttämällä **docker** ja `-H` Vie docker asiakkaan oman solmu AM, voit liittyä solmun välittämällä Klusteritunnus ja solmun docker portti luot swarm (viimeksi käyttämällä **--osoite**):

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

Joka näyttää hyvältä. Vahvista, että **swarm** on käytössä on Kirjoita **swarm-solmu-1** :

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

Toista klusterin muiden solmujen. Tässä tapauksessa emme vakiomuotoa **swarm-solmu-2** ja **swarm-solmu-3**.

## <a name="begin-managing-the-swarm-cluster"></a>Aloita swarm klusterin hallinta

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

ja sitten voit näyttää, että solmujen yhteyttä klusterin:

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Siirry suorittaa toimintoja oman swarm. Tarkista oletus, katso [https://github.com/docker/swarm/](https://github.com/docker/swarm/)tai mahdollisesti [videopuhelun](https://www.youtube.com/watch?v=EC25ARhZ5bI).

<!-- links -->

[docker-machine-azure]: virtual-machines-linux-docker-machine.md
 
