<properties 
  pageTitle="Käyttöönotto Azure yksityinen Docker oman rekisterin | Microsoft Azure"
  description="Kuvataan, miten voit käyttää Docker rekisterin isännöi säilö kuvia Azure-Blob-säiliö-palvelusta."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Azure yksityinen Docker oman rekisterin käyttöönotto

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



Tässä asiakirjassa kerrotaan mitä Docker yksityinen rekisterin on, ja näyttää Docker rekisterin 2.0-säilö kuva käyttöönotto yksityinen Docker-rekisterin Microsoft Azure käyttämällä Azure-Blob-objektien tallennustilaan.

Tämän asiakirjan oletetaan, että:

1. Osaat Docker ja saada Docker kuvat tallennetaan. (Et? [Lisätietoja Docker](https://www.docker.com))
2. Sinun on palvelimessa, johon on asennettu Docker ohjelma. (Et? [Tehdä sen nopeasti Azure.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Mikä on yksityinen Docker rekisterin?

Toimitetaan tai kontteihin pakattuja sovellustesi pilveen, jotta voit käyttää Docker säilö-kuva ja tallenna se toiseen sijaintiin, niin, että sitä voidaan käyttää itse ja muut. 

Kun säilö näköistiedoston luominen ja toimitus pilveen on helppoa, kannattaa tallentaa kuvan luotettavasti hankala. Tästä syystä Docker tarjoaa keskitetyn palvelun kutsutaan [Docker keskittimeen] [ docker-hub] tallentamiseen säilö pilveen, kuvat ja avulla voit luoda säilöjen käyttämällä milloin tahansa kuvat.

Vaikka [Docker keskittimeen] [ docker-hub] on maksullisen palvelun projektiin sovelluksen yksityiset säilö kuvat Docker suhteissa kehittäjät tarpeet ja tarjoaa Avaa lähde-toolset kuvat tallennetaan oma yksityinen Docker rekisterin palomuurin takana tai paikalliseen ilman pallolla julkinen Internet.
Koska Azure-Blob-säiliö on helppo suojatun, nopeasti voit luominen ja käyttäminen yksityinen Docker rekisterin Azure-tietokannassa, voit hallita itse.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Miksi Docker rekisterin Azure-olisi isännöintiin?

Isännöinnin Docker rekisterin-esiintymän Microsoft Azure-ja kuvien tallentaminen Azure-Blob-säiliö, sinulla on monia etuja:

**Suojaus:** Docker kuvat älä jätä Azure palvelinkeskusten, joten niitä ei toimintojen julkinen Internet samoin kuin jos käytit Docker-toiminnossa.
  
**Suorituskyvyn:** Docker kuvat tallennetaan samaan palvelinkeskuksen tai alueen sovellukset. Tämä tarkoittaa kuvat vedetään nopeammin ja lisää luotettavasti verrattuna Docker-toiminnossa.

**Luotettavuus:** Käyttämällä Microsoft Azure Blob Storage voit tehdä monta tallennustilan-ominaisuuksia, kuten suuri käytettävyys-redundanssi premium storage (SSDs) käyttö ja niin edelleen.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Määrittäminen Docker rekisterin käyttämään Azure-Blob-säiliö

(Se on suositeltavaa lukea [Docker rekisterin 2.0 asiakirjat]-[rekisterin docs] ennen jatkamista.)

Voit [määrittää] [ registry-config] Docker rekisteriä kahdella eri tavalla.
Voit:

1. Käytössä `config.yml` tiedosto. Tässä tapauksessa sinun on luotava erillinen Docker-kuvan päälle `registry` kuva.
2. Oletusarvo-kokoonpanotiedosto ympäristömuuttujat – ohittaa: saa asioiden hoitamiseksi ilman luominen ja ylläpito erillisessä Docker kuva.

Tässä ohjeaiheessa seuraa yksinkertaisuuden-vaihtoehto 2-ympäristömuuttujien käyttäminen.

Jotta voit suorittaa Docker rekisterin esiintymä joka:

* Azure-tallennustilan tilin käyttää projektiin kuvat
* Seuraa Virtual Machine porttia 5000
* on määritetty tarkistusta (ei suositella, katso alla oleva huomautus)

Sinun on suoritettava seuraavat Docker komento bash päätteen (korvaa `<storage-account>` ja `<storage-key>` tunnistetiedot kanssa):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Komennon sulkeutuu, kun näet yksityinen Docker rekisterin-esiintymää isännöivän suorittamalla säilö `docker ps` isäntä-komennon:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Suojauksen määrittäminen Docker rekisteri ei koske tässä asiakirjassa ja rekisteriä päivitetään voivat käyttää kaikki, jos ilman todennusta oletusarvoisesti, jos virtuaalikoneen päätepisteen porttiin rekisteri portin avaaminen tai kuormituksen, jos käytössäsi on yllä käyttöönotto-komento.
>
> Lue [Määrittäminen Docker rekisterin] [ registry-config] ohjeista lisätietoja suojatun rekisterin esiintymä ja kuvat.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun määrittäminen rekisteriä, on aika, siirry käyttää joidenkin Lisää. Aloita docker [rekisterin asiakirjoja]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[rekisterin asiakirjoja]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
