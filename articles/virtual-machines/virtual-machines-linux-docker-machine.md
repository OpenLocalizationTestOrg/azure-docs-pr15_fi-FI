<properties
    pageTitle="Luo Docker isännät Azure Docker koneen kanssa | Microsoft Azure"
    description="Tässä artikkelissa kuvataan Docker tietokoneen docker isännät luominen Azure käyttö."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="07/22/2016"
    ms.author="rasquill"/>

# <a name="use-docker-machine-with-the-azure-driver"></a>Docker machinella Azure-ohjaimella

[Docker](https://www.docker.com/) on suosituimpia virtualization tavoilla, joka käyttää vielä eristämisestä sovelluksen tietoja ja jaettujen resurssien tietojenkäsittely Linux säilöjen näennäiskoneiden sijaan. Tässä ohjeaiheessa kerrotaan, milloin ja miten [Docker](https://docs.docker.com/machine/) machinella ( `docker-machine` komennon) voit luoda uuden Linux VMs Azure docker isäntä Linux säilöjen varten on otettu käyttöön.


## <a name="create-vms-with-docker-machine"></a>Luo VMs Docker tietokoneen kanssa

Luo docker host VMs Azure-tietokannassa, `docker-machine create` -komennon avulla `azure` ohjaimen argumentti ohjain-vaihtoehto (`-d`) ja muut argumentit. 

Seuraava esimerkki perustuu oletusarvot yhteydessä, mutta se avaa portti 80 AM Internet-yhteys Testaa nginx säilöön, tekee `ops` kirjautuminen käyttäjän SSH ja puheluita uusi AM `machine`. 

Kirjoita `docker-machine create --driver azure` asetukset ja oletusarvot; Voit myös lukea [Docker Azure ohjaimen ohjeissa](https://docs.docker.com/machine/drivers/azure/). (Huomaa, että jos sinulla on kaksivaiheinen todennus on käytössä, voit pyydetään todennuksen toinen tekijä.)

```bash
docker-machine create -d azure \
  --azure-ssh-user ops \
  --azure-subscription-id <Your AZURE_SUBSCRIPTION_ID> \
  --azure-open-port 80 \
  machine
```

Tulosteen pitäisi näyttää tältä, sen mukaan, onko sinulla Kaksivaiheinen todennus on määritetty tilissäsi.

```
Creating CA: /Users/user/.docker/machine/certs/ca.pem
Creating client certificate: /Users/user/.docker/machine/certs/cert.pem
Running pre-create checks...
(machine) Microsoft Azure: To sign in, use a web browser to open the page https://aka.ms/devicelogin. Enter the code <code> to authenticate.
(machine) Completed machine pre-create checks.
Creating machine...
(machine) Querying existing resource group.  name="machine"
(machine) Creating resource group.  name="machine" location="eastus"
(machine) Configuring availability set.  name="docker-machine"
(machine) Configuring network security group.  name="machine-firewall" location="eastus"
(machine) Querying if virtual network already exists.  name="docker-machine-vnet" location="eastus"
(machine) Configuring subnet.  name="docker-machine" vnet="docker-machine-vnet" cidr="192.168.0.0/16"
(machine) Creating public IP address.  name="machine-ip" static=false
(machine) Creating network interface.  name="machine-nic"
(machine) Creating storage account.  name="vhdsolksdjalkjlmgyg6" location="eastus"
(machine) Creating virtual machine.  name="machine" location="eastus" size="Standard_A2" username="ops" osImage="canonical:UbuntuServer:15.10:latest"
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with ubuntu(systemd)...
Installing Docker...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env machine
```

## <a name="configure-your-docker-shell"></a>Määritä docker-käyttöliittymä

Kirjoita seuraavaksi `docker-machine env <VM name>` , lue, miten voit määrittää käyttöliittymä. 

```bash
docker-machine env machine
```

Joka tulostuu tietoja ympäristöstä, joka näyttää seuraavankaltaiselta. Huomautus IP-osoite on määritetty, joka on Testaa AM.

```
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://191.237.46.90:2376"
export DOCKER_CERT_PATH="/Users/rasquill/.docker/machine/machines/machine"
export DOCKER_MACHINE_NAME="machine"
# Run this command to configure your shell:
# eval $(docker-machine env machine)
```

Voit joko suorittaa ehdotettu määritys-komennon tai voit määrittää ympäristömuuttujat itse. 

## <a name="run-a-container"></a>Suorita säilö

Nyt voit suorittaa yksinkertaisen verkkopalvelimeen Testaa onko kaikki toimii oikein. Seuraavassa on vakio nginx kuvaa, määritä se olisi kuuntelemaan porttia 80 ja, jos AM käynnistyy säilö tulee uudelleen myös (`--restart=always`). 

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

Ja tarkista käynnissä säilö, kirjoita `docker-machine ip <VM name>` Etsi IP-osoite (Jos olet unohtanut kohteesta `env` komennon):

![Käynnissä olevat ngnix säilö](./media/virtual-machines-linux-docker-machine/nginxsuccess.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Jos haluat muuttaa, voit kokeilla Azure [Docker AM tunniste](virtual-machines-linux-dockerextension.md) , voit tehdä saman toiminnon Azure-CLI tai Azure resurssien hallinnan mallien avulla. 

Katso Lisää esimerkkejä käyttäminen Docker [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Yhdistä [esittely](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/)- [Docker käsitteleminen](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) . Katso Lisää quickstarts-HealthClinic.biz esittely, [Azure Developer Työkalut Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).

