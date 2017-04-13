<properties
   pageTitle="Asenna MongoDB Linux AM | Microsoft Azure"
   description="Lue, miten asennetaan ja määritetään MongoDB Linux virtual tietokoneeseen Azure-tietokannassa käyttämällä resurssien hallinnan käyttöönottomalli."
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
   ms.date="09/29/2016"
   ms.author="iainfou"/>

# <a name="install-and-configure-mongodb-on-a-linux-vm-in-azure"></a>Asenna ja määritä MongoDB Linux-AM Azure-tietokannassa
[MongoDB](http://www.mongodb.org) on Suositut Avaa lähde, tehokas NoSQL tietokanta. Tässä artikkelissa kerrotaan, miten asennetaan ja määritetään MongoDB-Linux-AM Azure-tietokannassa käyttämällä resurssien hallinnan käyttöönottomalli. Esimerkeissä näytetään, että tiedot siitä, miten voit:

- [Asenna ja määritä basic MongoDB esiintymän manuaalisesti](#manually-install-and-configure-mongodb-on-a-vm)
- [Luo tavallinen MongoDB-esiintymä Resurssienhallinta mallin avulla](#create-basic-mongodb-instance-on-centos-using-a-template)
- [Luoda monimutkaisia MongoDB, sharded klusterin replikaan asettaa Resurssienhallinta mallin avulla](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa edellyttää seuraavaa:

- Azure tili ([maksuton kokeiluversio](https://azure.microsoft.com/pricing/free-trial/)).
- [Azure CLI](../xplat-cli-install.md) kirjautunut sisään`azure login`
- Azure CLI *on oltava* Azure Resurssienhallinta tilan käyttäminen`azure config mode arm`


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Asentaminen ja määrittäminen MongoDB AM manuaalisesti
MongoDB [antaa asennusohjeet](https://docs.mongodb.com/manual/administration/install-on-linux/) Linux distros, mukaan lukien punainen on / CentOS, SUSE, Ubuntu ja Debian. Seuraavassa esimerkissä luodaan `CoreOS` AM säilytettävä SSH-avaimen avulla `.ssh/azure_id_rsa.pub`. Vastaa näyttöön tallennustilan tilin nimi, DNS-nimi ja järjestelmänvalvojan tunnistetietoja:

```bash
azure vm quick-create --ssh-publickey-file .ssh/azure_id_rsa.pub --image-urn CentOS
```

Kirjaudu julkiseen IP-osoitetta AM luominen edellisen vaiheen lopussa näkyvä AM:

```bash
ssh ops@40.78.23.145
```

Lisää asennustiedostojen MongoDB-luominen `yum` tietovaraston tiedostoon seuraavasti:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Avaa MongoDB repo tiedoston muokattavaksi. Lisää seuraavat rivit:

```bash
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Asenna MongoDB käyttämällä `yum` seuraavasti:

```bash
sudo yum install -y mongodb-org
```

Oletusarvon mukaan SELinuxin pakotetun CentOS kuvia, joka estää pääsyn MongoDB. Käytännön hallintatyökalut Asenna ja määritä SELinuxin sallimaan MongoDB toimimaan sen TCP oletusportti 27017 seuraavasti. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Käynnistä MongoDB-palvelu seuraavasti:

```bash
sudo service mongod start
```

Vahvista asennuksen MongoDB yhteyden muodostaminen paikalliseen `mongo` asiakkaan:

```bash
mongo
```

Testaa nyt MongoDB esiintymän joidenkin tietojen lisäämistä ja etsimällä sitten:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Halutessasi voit määrittää MongoDB käynnistymään automaattisesti järjestelmän uudelleenkäynnistyksen yhteydessä:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Luo tavallinen MongoDB esiintymän CentOS mallin avulla
Voit luoda yksittäisen CentOS AM, käyttämällä seuraavia Azure pikaopas mallin Github basic MongoDB esiintymä. Tämä malli käyttää Lisää Linux mukautettu komentosarja-laajennus `yum` juuri luomasi CentOS AM ja sitten Asenna MongoDB säilöön.

- [Tavallinen MongoDB esiintymä CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Seuraavassa esimerkissä luodaan resurssiryhmä-nimisen `myResourceGroup` - `WestUS` alue. Kirjoita oman arvot seuraavasti:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure-CLI palauttaa näyttöön tulee kehote muutamassa luominen käyttöönoton, mutta asennuksesta ja määrityksestä kestää muutaman minuutin suorittamiseen. Käyttöönoton tilan tarkistaminen `azure group deployment show myResourceGroup`, kirjoittamalla resurssiryhmän nimi vastaavasti. Odota, kunnes `ProvisioningState` näkyy ennen kuin yrität SSH AM "Onnistui".

Kun asennus on valmis, SSH, AM. AM käytön IP-osoitteen `azure vm show` komennon seuraavan esimerkin mukaisesti:

```bash
azure vm show --resource-group myResourceGroup --name myVM
```

Tuloksessa, alareunassa `Public IP address` tulee näkyviin. SSH, että AM oman AM IP-osoite:

```bash
ssh ops@138.91.149.74
```

Vahvista asennuksen MongoDB yhteyden muodostaminen paikalliseen `mongo` asiakkaan seuraavasti:

```bash
mongo
```

Testaa nyt esiintymän joidenkin tietojen lisäämistä ja etsimällä seuraavasti:

```
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Luoda monimutkaisia Sharded MongoDB-klusterin CentOS mallin avulla
Voit luoda monimutkaisia MongoDB sharded klusterin Github seuraavat Azure pikaopas mallin avulla. Tämän mallin seuraa redundancy ja suuren käytettävyyden [MongoDB sharded klusterin parhaita käytäntöjä](https://docs.mongodb.com/manual/core/sharded-cluster-components/) . Mallin Luo kaksi shards kunkin replikajoukon kolme solmujen. Yksi config server replikajoukon kolme solmujen kanssa on myös luotu, plus kaksi `mongos` reitittimen palvelinten antamaan yhdenmukaisuuden sovelluksista shards yli.

- [MongoDB Sharding klusterin-CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [AZURE.WARNING] Käyttöönotto tämän monimutkaisia MongoDB sharded klusterin edellyttää enemmän kuin 20 sydämiä, joka on tavallisesti oletusarvon core Laske tilauksen alueittain. Avaa Azure tukipyyntö niin, että core määrä.

Seuraavassa esimerkissä luodaan resurssiryhmä-nimisen `myResourceGroup` - `WestUS` alue. Kirjoita oman arvot seuraavasti:

```bash
azure group create --name myResourceGroup --location WestUS \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [AZURE.NOTE] Azure-CLI palauttaa näyttöön tulee kehote muutamassa luominen käyttöönoton, mutta asennuksesta ja määrityksestä voi kestää yli tunnin suorittamiseen. Käyttöönoton tilan tarkistaminen `azure group deployment show myResourceGroup`, resurssiryhmän nimi säätäminen vastaavasti. Odota, kunnes `ProvisioningState` näkyy 'Onnistui' ennen yhteyden muodostamista VMs.


## <a name="next-steps"></a>Seuraavat vaiheet
Näissä esimerkeissä muodostat yhteyden MongoDB esiintymän paikallisesti AM. Jos haluat muodostaa yhteyden MongoDB esiintymän toisesta AM tai verkkoon, varmista [verkon käyttöoikeusryhmän säännöt luodaan](virtual-machines-linux-nsg-quickstart.md)tarvittavat.

Lisätietoja mallien avulla luomisesta on artikkelissa [Azure resurssin hallinnassa: yleiskatsaus](../azure-resource-manager/resource-group-overview.md).

Azure Resurssienhallinta-mallien avulla mukautettu komentosarja-tunniste Lataa ja suorita komentosarjat-kohdassa VMs. Lisätietoja on artikkelissa [Azure mukautettu komentosarja-tunniste Linux näennäiskoneiden kanssa](virtual-machines-linux-extensions-customscript.md).