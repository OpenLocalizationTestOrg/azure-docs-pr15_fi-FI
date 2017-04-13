<properties
   pageTitle="Docker ja luo virtual koneeseen | Microsoft Azure"
   description="Luo ja Docker käyttäminen Linux näennäiskoneiden Azure-tietokannassa lyhyt esittely"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="iainfou"/>

# <a name="get-started-with-docker-and-compose-to-define-and-run-a-multi-container-application-on-an-azure-virtual-machine"></a>Docker ja luo määrittäminen ja suorita Azure virtuaalikoneen usean säilö-sovelluksen käytön aloittaminen

Aloita käyttö Docker ja [Luo](http://github.com/docker/compose) määrittäminen ja monimutkaisia sovelluksen käyttämiseen Linux virtual tietokoneeseen Azure-tietokannassa. Viestin muoto, jossa käytät Määritä sovellus, jossa useita Docker säilöjä tekstitiedosto. Valitse asettamasi sovelluksesi yksittäisen-komento, joka toimii kaikki määritetyn ympäristön käyttöönotto. 

Esimerkiksi tämän artikkelin avulla voit nopeasti määrittää WordPress blogin kanssa taustassa Ubuntu AM MariaDB SQL-tietokantaan. Voit myös määrittää monimutkaisempia sovelluksia Luo.


## <a name="step-1-set-up-a-linux-vm-as-a-docker-host"></a>Vaihe 1: Määritä Linux-AM kuin Docker isäntä

Voit luoda Linux AM ja määritä se kyseisessä kuin Docker isäntä käyttää eri Azure menettelyt ja käytettävissä olevat kuvat tai Resurssienhallinta malleja Azure Marketplacesta. Esimerkiksi näkyviin luominen nopeasti Ubuntu AM Azure Docker AM-tunniste [pikaopas mallin](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)avulla [käyttämällä ottamaan ympäristön Docker AM-tunniste](virtual-machines-linux-dockerextension.md) . 

Kun käytät Docker AM-tunniste, että AM on automaattisesti määritetty Docker host ja viestin muoto on jo asennettu. Esimerkki artikkelin avulla voit käyttää [Mac, Linux-ja Windows Azure käyttöliittymä](../xplat-cli-install.md) (Azure-CLI) Resurssienhallinta-tilassa, voit luoda AM.

Edellisen asiakirjasta basic-komento luo nimetty resurssiryhmä `myResourceGroup` - `West US` sijainti ja ottaa käyttöön AM Azure Docker AM-tunniste asennettu:

```bash
azure group create --name myResourceGroup --location "West US" \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/docker-simple-on-ubuntu/azuredeploy.json
```

## <a name="step-2-verify-that-compose-is-installed"></a>Vaihe 2: Varmista, että Käytä on asennettu

Kun asennus on valmis, SSH uusi isäntään Docker käyttämällä DNS nimeä annettu käyttöönoton aikana. Voit käyttää `azure vm show -g myDockerResourceGroup -n myDockerVM` voit tarkastella tietoja oman AM, mukaan lukien DNS-nimi.

Jos haluat tarkistaa, että Käytä on asennettu AM, suorita seuraava komento:

```bash
docker-compose --version
```

Näet tulos muistuttaa `docker-compose 1.6.2, build 4d72027`.

>[AZURE.TIP] Jos olet käyttänyt toista menetelmää voit luoda Docker host ja asentamalla Luo, on [Luo ohjeissa](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md).


## <a name="step-3-create-a-docker-composeyml-configuration-file"></a>Vaihe 3: Luo docker compose.yml kokoonpanotiedosto

Luo seuraavaksi `docker-compose.yml` tiedosto, joka on juuri määritysten tekstitiedosto, voit määrittää Docker säilöjen voidaan suorittaa AM. Tiedosto määrittää toimimaan kunkin säilön kuvan (tai se voi olla muodosta Dockerfile-) tarvitse ympäristömuuttujat ja riippuvuudet, portit ja säilöjen väliset linkit. Lisätietoja yml tiedoston syntaksista on kohdassa [Luo tiedostoviittaus](http://docs.docker.com/compose/yml/).

Luo `docker-compose.yml` tiedostoon seuraavasti:

```bash
touch docker-compose.yml
```

Lisää tietoja tiedoston tuttuja tekstieditorissa avulla. Seuraavassa esimerkissä `vi` editoria:

```bash
vi docker-compose.yml
```

Seuraavassa esimerkissä Liitä tekstitiedostosi. Tässä määrityksessä käyttää kuvien [DockerHub rekisterin](https://registry.hub.docker.com/_/wordpress/) asentamiseen WordPress (Avaa lähde blogin ja sisällön hallintajärjestelmän) ja linkitetyt taustassa MariaDB SQL-tietokantaan. Kirjoita oman `MYSQL_ROOT_PASSWORD` seuraavasti:

```bash
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 80:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
```

## <a name="step-4-start-the-containers-with-compose"></a>Vaihe 4: Alkavat säilöt viestin muoto

Samassa kansiossa kuin oman `docker-compose.yml` tiedoston, suorita seuraava komento (-käyttöympäristön mukaan voit joutua muuttamaan suorittaa `docker-compose` käyttämällä `sudo`.):

```bash
docker-compose up -d

```

Tämä komento aloittaa määritetyn Docker säilöjen `docker-compose.yml`. Kestää hetken tai kaksi tämän vaiheen suorittamiseen. Näet tulosteen samalla tavalla kuin seuraavassa esimerkissä:

```bash
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
...
```

>[AZURE.NOTE] Muista käyttää Käynnistys **-d** -vaihtoehto, niin, että säilöt käynnissä taustalla jatkuvasti.

Varmista, että säilöt ovat ylös, kirjoita `docker-compose ps`. Näyttöön tulee suunnilleen:

```bash
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:80->80
ess_1              apache2-for ...                       /tcp
```

Voit nyt muodostaa yhteyden WordPress suoraan-porttiin 80 AM. Avaa selain ja kirjoita oman AM DNS-nimi (kuten `http://myresourcegroup.westus.cloudapp.azure.com`). Pitäisi tulla näkyviin WordPress aloitusnäyttö, jossa voit Viimeistele asennus ja sovelluksen käytön aloittaminen.

![WordPress aloitusnäyttö][wordpress_start]


## <a name="next-steps"></a>Seuraavat vaiheet

* Siirry Lisää asetuksia voit määrittää Docker ja luo Docker AM [Docker AM tunniste käyttöoppaassa](https://github.com/Azure/azure-docker-extension/blob/master/README.md) . Esimerkiksi yksi vaihtoehto on (muunnettu JSON) luo yml-tiedoston tallentamisen suoraan Docker AM-tunniste määritys.
* Tutustu [Käytä komentoriviä](http://docs.docker.com/compose/reference/) ja lisää esimerkkejä ja käyttöönottoon usean säilö sovellusten [käyttöoppaassa](http://docs.docker.com/compose/) .
* Voit käyttää Azure Resurssienhallinta-mallia, joko oman oma tai yksi lähettänyt [yhteisön](https://azure.microsoft.com/documentation/templates/)Azure-AM Docker ja luo määrittänyt sovelluksen käyttöönotto. Esimerkiksi [käyttöönotto WordPress blogin kanssa Docker](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-wordpress-mysql) malli käyttää Docker ja luo WordPress käyttöönotosta MySQL-Ubuntu AM taustassa nopeasti.
* Kokeile integraation [Docker Swarm](virtual-machines-linux-docker-swarm.md) klusterin Docker kirjoittaa. Saat skenaariot [Avulla voit kirjoittaa Swarm kanssa](https://docs.docker.com/compose/swarm/) .

<!--Image references-->

[wordpress_start]: ./media/virtual-machines-linux-docker-compose-quickstart/WordPress.png
