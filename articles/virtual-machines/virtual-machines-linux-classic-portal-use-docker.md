<properties
    pageTitle="Docker AM laajennuksen käytön Linux | Microsoft Azure"
    description="Tässä artikkelissa kuvataan Docker ja Azuren näennäiskoneiden laajennukset ja Azuren näennäiskoneiden, jotka ovat docker isännät Azure-CLI käyttäminen perinteinen käyttöönoton mallin luomisesta."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="05/27/2016"
    ms.author="rasquill"/>


# <a name="using-the-docker-vm-extension-with-the-azure-classic-portal"></a>Käyttämällä Docker AM tunniste Azure perinteinen-portaalissa

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


[Docker](https://www.docker.com/) on suosituimpia virtualization tavoilla, joka käyttää vielä eristämisestä tiedot ja jaettujen resurssien tietojenkäsittely [Linux säilöjen](http://en.wikipedia.org/wiki/LXC) näennäiskoneiden sijaan. Voit käyttää [Azure Linux agentti] hallitsee Docker AM tunniste Docker AM, joka isännöi jokin muu luku säilöjä Azure-sovellusten luominen.

> [AZURE.NOTE] Tässä ohjeaiheessa kerrotaan, miten voit luoda Docker AM perinteinen Azure-portaalista. Näytetään, kuinka voit luoda Docker AM komentorivillä, katso, [miten Docker AM tunniste Azure komentorivivalitsimet Interface (Azure CLI) kohteesta]. Ylätason keskustelun säilöt ja niiden eduista on ohjeartikkelissa [Docker korkean tason luonnoslehtiö](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).

## <a name="create-a-new-vm-from-the-image-gallery"></a>Luo uusi AM kuva-valikoimasta
Ensimmäiseksi on oltava Azure-AM Linux-kuvasta, joka tukee Docker AM-tunniste, käyttäen Ubuntu 14.04 l. kuvan kuva valikoimasta Esimerkki palvelimen kuvan ja Ubuntu 14.04 työpöydän asiakkaaksi. Portaalissa Valitse **+ Uusi** luomalla uuden AM ja valitse Ubuntu 14.04 l. kuvan vaihtoehdoista tai valmis kuvavalikoima vasemmassa alakulmassa alla kuvatulla tavalla.

> [AZURE.NOTE] Tällä hetkellä vain Ubuntu 14.04 l. kuvia uudempi kuin heinäkuussa 2014 tukevat Docker AM-tunniste.

![Luo uusi Ubuntu kuva](./media/virtual-machines-linux-classic-portal-use-docker/ChooseUbuntu.png)

## <a name="create-docker-certificates"></a>Luo Docker varmenteet

Jälkeen AM on luotu, varmista, että Docker on asennettu asiakastietokoneeseen. (Lisätietoja on artikkelissa [Docker asennusohjeet](https://docs.docker.com/installation/#installation).)

Docker viestintään mukaan [Käynnissä Docker kanssa https] varmenne ja -tiedostojen luominen ja sijoittaa ne **`~/.docker`** asiakastietokoneen hakemistoon.

> [AZURE.NOTE] Portaalissa Docker AM-käyttöajan pidentämiseksi tällä hetkellä tunnistetiedot, jotka ovat base64-koodattuja.

Käytä komentorivillä **`base64`** tai muuta tuttuja koodauksen työkalua base64-koodattu ohjeaiheiden luomisessa. Näin varmenne ja yksinkertainen tiedostojoukon saattaa näyttää seuraavanlaiselta:

```
 ~/.docker$ ls
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ ls
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## <a name="add-the-docker-vm-extension"></a>Lisää Docker AM-tunniste
Jos haluat lisätä Docker AM-tunniste, Etsi luomasi AM esiintymän ja **tunnisteet** kohtaan ja valitse se tulee näkyviin AM tunnisteet alla kuvatulla tavalla.
> [AZURE.NOTE] Tämä toiminto on tuettu vain esikatselu-portaalissa: https://portal.azure.com/

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickExtensions.png)
### <a name="add-an-extension"></a>Tunnisteen lisääminen
Osoita **+ Lisää** mahdollista AM-laajennukset, voit lisätä tämän AM näyttämiseen.

![](./media/virtual-machines-linux-classic-portal-use-docker/ClickAdd.png)
### <a name="select-the-docker-vm-extension"></a>Valitse Docker AM tunniste
Valitse Docker AM-tunniste, joka näyttää Docker kuvaus ja tärkeitä linkkejä, ja valitse sitten **Luo** Aloita asennus alaosassa.

![](./media/virtual-machines-linux-classic-portal-use-docker/ChooseDockerExtension.png)

![](./media/virtual-machines-linux-classic-portal-use-docker/CreateButtonFocus.png)
### <a name="add-your-certificate-and-key-files"></a>Varmenteen ja avaimen tiedostojen lisääminen:

Kirjoita lomakekentät, base64-koodattu versiot CA-varmenne, palvelimen varmenteen ja Server-näppäintä, seuraavassa kuvassa esitetyllä tavalla.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddExtensionFormFilled.png)

> [AZURE.NOTE] Huomaa, että (kuten yllä oleva kuva) 2376 täytetään oletusarvoisesti. Voit lisätä minkä tahansa päätepisteen, mutta seuraavassa vaiheessa on vastaava päätepisteen Avaa. Jos muutat oletusarvoista, varmista, että Avaa vastaavaa päätepisteen seuraavassa vaiheessa.

## <a name="add-the-docker-communication-endpoint"></a>Lisää Docker viestintäpäätepiste
Kun olet luonut resurssiryhmä tarkastelemisen oman AM liittyvät verkon suojaus-ryhmä ja **Saapuva suojaussäännöt** tarkastelemaan sääntöjä, kuten kuvassa.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddingEndpoint.png)

Valitse **+ Lisää** voit lisätä toisen säännön ja kirjoita oletusarvo-tapauksessa (Kirjoita tässä esimerkissä **Docker**) päätepisteen nimi ja 2376 'kohde portin alue". Määritä protokolla-arvo, jossa **TCP**ja valitse **OK** , jos haluat luoda säännön.

![](./media/virtual-machines-linux-classic-portal-use-docker/AddEndpointFormFilledOut.png)


## <a name="test-your-docker-client-and-azure-docker-host"></a>Testaa Docker asiakastietokoneen ja Azure Docker Host (isäntä)
Paikanna ja kopioi nimi oman AM toimialueen ja asiakastietokoneen, kirjoita komentoriville `docker --tls -H tcp://` *dockerextension* `.cloudapp.net:2376 info` (jossa *dockerextension* korvataan alitoimialueen oman AM).

Tulos näkyy tällainen:

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

Kun suoritat edellä kuvatut toimet, on nyt käytössä Azure-AM, määritetty edellyttää yhteyttä etäyhteyden muiden asiakkaiden täysin Docker isäntä.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Seuraavat vaiheet

Olet valmis ja siirry [Docker käyttöoppaassa] Docker AM. Jos haluat automatisoida luominen Docker isäntien Azure VMs komentorivillä kautta, katso lisätietoja [artikkelista Docker AM tunniste Azure komentorivivalitsimet Interface (Azure CLI) poistaminen]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Opi käyttämään Docker AM tunniste Azure komentorivivalitsimet Interface (Azure CLI) poistaminen]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux-agentti]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Docker käytön https]: http://docs.docker.com/articles/https/
[Docker käyttöopas]: https://docs.docker.com/userguide/
