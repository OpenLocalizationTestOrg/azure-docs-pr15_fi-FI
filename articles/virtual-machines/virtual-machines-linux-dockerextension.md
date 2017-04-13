<properties
   pageTitle="Käyttämällä Azure Docker AM-tunniste | Microsoft Azure"
   description="Opettele käyttämään Docker AM-tunniste ottamaan helposti ja turvallisesti Docker ympäristössä, jossa Azure-tietokannassa Resurssienhallinta mallien avulla."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/25/2016"
   ms.author="iainfou"/>

# <a name="create-a-docker-environment-in-azure-using-the-docker-vm-extension"></a>Luoda Docker ympäristön Azure käyttämällä Docker AM-tunniste
Docker on Suositut säilö hallinta ja imaging ympäristö, jonka avulla voit nopeasti käsitteleminen säiliöiden Linux (ja myös Windows). Azure-ovat eri tavoin voit ottaa Docker tarpeidesi mukaan. Tässä artikkelissa keskitytään Docker AM-tunniste ja Azure Resurssienhallinta mallien avulla. 

Lisätietoja eri käyttöönotto-kautta, kuten Docker Machine ja Azure säilö Servicesin seuraavissa artikkeleissa:

- Nopeasti prototyypin sovelluksen voit luoda yksittäisen Docker isäntä käyttämällä [Docker tietokoneen](./virtual-machines-linux-docker-machine.md).
- Suurempi kuin, Lisää vakaata ympäristössä voit käyttää Azure Docker AM-tunniste, joka tukee myös [Docker laatia](https://docs.docker.com/compose/overview/) yhtenäinen säilö ominaisuuksissa luomiseen. Tämän artikkelin tiedot käyttämällä Azure Docker AM-tunniste.
- Voit ottaa [Docker Swarm klusterin Azure säilö Services](../container-service/container-service-deployment.md)luonnissa tuotannon-sivuille, skaalattava ympäristöissä, joissa on Lisää työkaluja järjestäminen ja hallinta.


## <a name="azure-docker-vm-extension-overview"></a>Azure Docker AM tunniste yleiskatsaus
Azure Docker AM-tunniste asentaa ja määrittää Docker daemon, Docker asiakkaan ja Docker muodostamiseen Linux virtuaalikoneen (AM). Käyttämällä Azure Docker AM-tunniste on enemmän hallinta ja ominaisuuksia kuin yksinkertaisesti käyttämällä Docker tietokoneen tai luominen Docker host. Näiden lisäominaisuuksien, kuten [Docker laatia](https://docs.docker.com/compose/overview/), tee sopiviksi tehokkaamman kehittäjä tai tuotannon ympäristöissä Azure Docker AM-tunniste.

Azure Resurssienhallinta malleja Määritä ympäristön koko rakenne. Mallien avulla voit luoda ja määrittää resurssit, kuten Docker host VMs, varasto, Roolipohjainen Access-ohjausobjektit (RBAC) ja vianmääritys. Voit käyttää näitä malleja muita ominaisuuksissa yhdenmukaisesti. Saat lisätietoja Azure Resurssienhallinta ja malleja [resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md). 


## <a name="deploy-a-template-with-the-azure-docker-vm-extension"></a>Ottaa käyttöön mallin Azure Docker AM-tunniste
Voit luoda Ubuntu-AM, joka käyttää Azure Docker AM-laajennuksen asentaminen ja määrittäminen Docker host käytetään pikaopas mallia. Voit tarkastella tätä mallia: [yksinkertaisen käyttöönoton Ubuntu-AM Docker kanssa](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu). 

Sinun on [Uusin Azure CLI](../xplat-cli-install.md) asennettu ja kirjautunut sisään käyttämällä Resurssienhallinta-tilan seuraavasti:

```
azure config mode arm
```

Ota käyttöön mallin avulla Azure-CLI määrittäminen mallin URI. Seuraavassa esimerkissä luodaan nimeltä resurssiryhmä `myResourceGroup` - `WestUS` sijainti. Käytä omia resurssin ryhmänimen ja sijainnin seuraavasti:

```
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

Vastaa näyttöön tallennustilan tilin nimi, Anna käyttäjänimi ja salasana ja anna DNS. Tulos on seuraavassa esimerkissä:

```
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Updating resource group myResourceGroup
info:    Updated resource group myResourceGroup
info:    Supply values for the following parameters
newStorageAccountName: mystorageaccount
adminUsername: ops
adminPassword: P@ssword!
dnsNameForPublicIP: mypublicip
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK

```

Azure CLI palauttaa, näet kehotteen jälkeen vain muutaman sekunnin, mutta Docker host on edelleen käytössä luonut ja määrittänyt Azure Docker AM tunnisteen mukaan. Kestää muutaman minuutin kuluttua Viimeistele käyttöönottoa varten. Voit tarkastella tietoja Docker host tilan käyttäminen `azure vm show` komento.

Seuraavassa esimerkissä tarkistetaan nimeltä AM tilan `myDockerVM` (mallista - oletusnimen ei muuta nimi) resurssiryhmän nimeltä `myResourceGroup`. Kirjoita edellisessä vaiheessa luomasi resurssiryhmän nimi:

```bash
azure vm show -g myResourceGroup -n myDockerVM
```

Tulos `azure vm show` komento on samanlainen kuin seuraavassa esimerkissä:

```
info:    Executing command vm show
+ Looking up the VM "myDockerVM"
+ Looking up the NIC "myVMNicD"
+ Looking up the public ip "myPublicIPD"
data:    Id                              :/subscriptions/guid/resourceGroups/myresourcegroup/providers/Microsoft.Compute/virtualMachines/MyDockerVM
data:    ProvisioningState               :Succeeded
data:    Name                            :MyDockerVM
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
[...]
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-33-D3-95
data:          Provisioning State        :Succeeded
data:          Name                      :myVMNicD
data:          Location                  :westus
data:            Public IP address       :13.91.107.235
data:            FQDN                    :mypublicip.westus.cloudapp.azure.com]
data:
data:    Diagnostics Instance View:
info:    vm show command OK
```

Näet tulosteen yläreunassa `ProvisioningState` , AM. Kun näyttöön `Succeeded`, asennus on valmis, ja voit SSH AM.

Tuloksessa, loppua `FQDN` näyttää Docker host täydellinen toimialuenimi. Tämä FQDN on käyttää SSH Docker-isäntään-annettuja ohjeita.


## <a name="deploy-your-first-nginx-container"></a>Ottaa käyttöön ensimmäisen nginx-säilö
Kun asennus on valmis, SSH uuden Docker-isäntään paikallisesta tietokoneesta. Kirjoita oma käyttäjänimi ja FQDN seuraavasti:

```bash
ssh ops@mypublicip.westus.cloudapp.azure.com
```

Kun kirjautunut Docker isäntä, suorita japanin nginx säilö:

```
sudo docker run -d -p 80:80 nginx
```

Tulos on seuraavassa esimerkissä nginx kuva ladataan ja säilön aloittaminen:

```
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
efd26ecc9548: Pull complete
a3ed95caeb02: Pull complete
a48df1751a97: Pull complete
8ddc2d7beb91: Pull complete
Digest: sha256:2ca2638e55319b7bc0c7d028209ea69b1368e95b01383e66dfe7e4f43780926d
Status: Downloaded newer image for nginx:latest
b6ed109fb743a762ff21a4606dd38d3e5d35aff43fa7f12e8d4ed1d920b0cd74
```

Tarkista tila säilöjen käynnissä Docker isännän seuraavasti:

```
sudo docker ps
```

Tulos on samanlainen kuin seuraavassa esimerkissä, ilmaisee, että nginx säilö on käynnissä ja TCP-portit 80 ja 443 ja edelleenlähettämisen:

```
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                         NAMES
b6ed109fb743        nginx               "nginx -g 'daemon off"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, 443/tcp   adoring_payne
```

Nähdäksesi lisääminen säilöön-toiminto Avaa selain ja kirjoita Docker isännän toimialuenimen:

![Käynnissä olevat ngnix säilö](./media/virtual-machines-linux-dockerextension/nginxrunning.png)


## <a name="azure-docker-vm-extension-template-reference"></a>Azure Docker AM tunniste mallin viittaus
Edellisessä esimerkissä, pikaopas mallia. Voit myös ottaa Azure Docker AM-tunniste Resurssienhallinta-mallit ja. Tee Lisää seuraava teksti Resurssienhallinta mallit-määrittäminen `vmName` , että AM oikein:

```
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "[concat(variables('vmName'), '/DockerExtension'))]",
  "apiVersion": "2015-05-01-preview",
  "location": "[parameters('location')]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ],
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "DockerExtension",
    "typeHandlerVersion": "1.1",
    "autoUpgradeMinorVersion": true,
    "settings": {},
    "protectedSettings": {}
  }
}
```

Voit etsiä lisätietoja ongelmatilanteita Resurssienhallinta-mallien käyttämisestä lukemalla [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md).


## <a name="next-steps"></a>Seuraavat vaiheet
Haluat ehkä [määrittää Docker daemon porttinumeroa](https://docs.docker.com/engine/reference/commandline/dockerd/#/bind-docker-to-another-hostport-or-a-unix-socket), ymmärtää [Docker suojaus](https://docs.docker.com/engine/security/security/)tai ota käyttöön säilöjen käyttämällä [Docker kirjoittaa](https://docs.docker.com/compose/overview/). Saat lisätietoja Azure Docker AM tunnisteen itse [GitHub projektin](https://github.com/Azure/azure-docker-extension/).

Lue lisätietoja Azure muita Docker asennusvaihtoehdot:

- [Docker machinella Azure-ohjaimella](./virtual-machines-linux-docker-machine.md)  
- [Docker ja luo määrittäminen ja suorita Azure virtuaalikoneen usean säilö-ohjelman käytön aloittaminen](virtual-machines-linux-docker-compose-quickstart.md).
- [Azure säilö Service-klusterin käyttöönotto](../container-service/container-service-deployment.md)
