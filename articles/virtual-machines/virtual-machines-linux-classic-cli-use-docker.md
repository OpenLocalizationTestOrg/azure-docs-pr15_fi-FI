<properties
    pageTitle="Käyttämällä Linux-Azure Docker AM-tunniste"
    description="Kuvataan Docker ja Azuren näennäiskoneiden laajennukset ja kerrotaan, miten voit luoda ohjelmallisesti näennäiskoneiden Azure, jotka ovat docker isännät käyttämällä Azure-CLI komentoriviltä."
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
    ms.date="08/29/2016"
    ms.author="rasquill"/>

# <a name="using-the-docker-vm-extension-from-the-azure-command-line-interface-azure-cli"></a>Liittymää käyttäen komentorivin azuren Docker AM-tunniste (Azure CLI)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]



Tässä ohjeaiheessa kerrotaan luomisesta AM service management (asm) tilaan-Azure CLI missä tahansa ympäristössä Docker AM-tunniste. [Docker](https://www.docker.com/) on suosituimpia virtualization tavoilla, joka käyttää vielä eristämisestä tiedot ja jaettujen resurssien tietojenkäsittely [Linux säilöjen](http://en.wikipedia.org/wiki/LXC) näennäiskoneiden sijaan. Voit käyttää Docker AM-tunniste ja [Azure Linux agentti](virtual-machines-linux-agent-user-guide.md) Docker AM, joka isännöi jokin muu luku säilöjä Azure-sovellusten luominen. Ylätason keskustelun säilöt ja niiden eduista on ohjeartikkelissa [Docker korkean tason luonnoslehtiö](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard).


##<a name="how-to-use-the-docker-vm-extension-with-azure"></a>Voit käyttää Azure Docker AM-tunniste
Jos haluat käyttää Azure Docker AM-tunniste, [Azure komentorivin Interface](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) suurempi kuin 0.8.6 versio on asennettava (katso tämän kirjoittaminen nykyinen versio on 0.10.0). Voit asentaa Mac, Linux ja Windows Azure-CLI.


Koko prosessin Docker käyttämisestä Azure on helppoa:

+ Azure-CLI ja riippuvaa asentaminen tietokoneeseen, josta haluat ohjausobjektin Azure (Windows, tämä on käynnissä virtual machine Linux jakauman)
+ Azure CLI Docker komennoilla voit luoda AM Docker host Azure
+ Paikallinen Docker komentojen avulla voit hallita Docker-säilöt-Docker-AM Azure-tietokannassa.


### <a name="install-the-azure-command-line-interface-azure-cli"></a>Asenna Azure käyttöliittymä (Azure CLI)

Asenna ja Määritä Azure-CLI, katso [asennusohjeet Azure käyttöliittymä](../xplat-cli-install.md). Vahvista asennuksen, kirjoita `azure` komentoriville seuraava komento ja näkyviin tulee Azure CLI ASCII-kuvia lyhyt hetken kuluttua, jossa luetellaan käytettävissä peruskomennot. Jos asennus toimii oikein, sinun on oltava kirjoittaa `azure help vm` ja että jotakin luettelon komennoista on "docker".

> [AZURE.NOTE] Docker on työkalut (Windows), [Docker tietokoneeseen](https://docs.docker.com/installation/windows/), jossa voit käyttää myös docker-asiakas, voit käsitellä Azure VMs kuin docker isännät luonnin automatisoida.

### <a name="connect-the-azure-cli-to-to-your-azure-account"></a>Azure CLI voit muodostaa yhteyden Azure-tili
Voit käyttää Azure-CLI ympäristösi Azure-CLI Azure tilin tunnistetiedot Liitä. Osan [Azure tilauksen yhdistämisestä](../xplat-cli-connect.md) kerrotaan, kuinka lataaminen ja **.publishsettings** -tiedoston tuominen tai Azure CLI liittäminen organisaation tunnus.

> [AZURE.NOTE] Välillä on joitakin eroja toiminnan, kun käytössä on yksi tai todennusta, joten muista lukea ymmärtää eri toimintoja asiakirja muilla tavoilla.

### <a name="install-docker-and-use-the-docker-vm-extension-for-azure"></a>Asenna Docker ja käyttää Azure Docker AM-tunniste
Noudata [asennusohjeita Docker](https://docs.docker.com/installation/#installation) Docker paikallisesti tietokoneeseen.

Voit käyttää Docker kanssa Azure Virtual Machine, AM käytettäviä Linux-kuva on asennettu [Azure Linux AM agentti](virtual-machines-linux-agent-user-guide.md) . On tällä hetkellä vain kahdenlaisia kuvia, jotka tarjoavat näin:

+ Azure-kuvavalikoiman Ubuntu kuva tai

+ Mukautetun Linux-kuva, jonka olet luonut Azure Linux AM-agentti, asentanut ja määrittänyt kanssa. Saat lisätietoja siitä, miten voit luoda mukautetun Linux AM kanssa Azure AM-agentti [Azure Linux AM agentti](virtual-machines-linux-agent-user-guide.md) .

### <a name="using-the-azure-image-gallery"></a>Azure-kuva-valikoimassa

Bash tai pääte istunnon seuraavalla komennolla Azure CLI etsiä viimeisimmän Ubuntu kuvan kirjoittamalla AM-valikoimassa

`azure vm image list | grep Ubuntu-14_04`

ja valitse jokin kuva nimet, kuten `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB`, ja seuraavalla komennolla voit luoda uuden AM, käyttämällä kyseiseen kuvaan.

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB" <username> <password>
```

Jos:

+ * &lt;AM cloudservice nimi&gt; * on nimi, josta tulee Docker säilö isäntätietokoneen Azure-tietokannassa AM

+  * &lt;käyttäjänimi&gt; * on oletusarvoinen pääkansion käyttäjää AM käyttäjänimi

+ * &lt;salasanan&gt; * on *käyttäjänimi* -tili, joka täyttää vaatimukset monimutkaisuus varten Azure salasana

> [AZURE.NOTE] Tällä hetkellä salasana on oltava vähintään 8 merkkiä, sisältävät yhden pieniksi kirjaimiksi ja yksi iso kirjain, numero ja erikoismerkin, kuten jokin seuraavista merkeistä: `!@#$%^&+=`. Ei, kauden lopussa Edellisessä lauseessa ei ole erikoismerkin.

Jos komento onnistui, katso esimerkiksi seuraavia argumentteja tarkka ja käytit mukaan seuraavasti:

![](./media/virtual-machines-linux-classic-cli-use-docker/dockercreateresults.png)

> [AZURE.NOTE] Virtual machine luominen voi kestää muutaman minuutin, mutta sen jälkeen, kun se on valmisteltu (osavaltio-arvo on `ReadyRole`) Docker daemon (Docker-palvelu) käynnistyy ja voit muodostaa yhteyden Docker säilö isäntä.

Voit testata Docker AM olet luonut Azure-tyyppi

`docker --tls -H tcp://<vm-name-you-used>.cloudapp.net:2376 info`

Jos * &lt;AM-nimi--käytit&gt; * on nimi, jota käytit puhelu virtuaalikoneen `azure vm docker create`. Raportissa pitäisi näkyä seuraava, joka ilmaisee, että Docker Host (isäntä)-AM ylös ja Azure käynnissä ja komentojen odottaa kaltaisen. 

Nyt voit yrittää muodostaa yhteyden docker asiakkaan tietoja (joitakin Docker asiakas-asetukset, kuten, Mac-kohdassa saatat joutua käyttämään `sudo`):

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

Vain, jos haluat näkyvän, että se on kaikki käytön, voit tutkia Docker tiedostotunnisteen AM:

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### <a name="docker-host-vm-authentication"></a>Docker Host AM todennus

Lisäksi luominen Docker, AM `azure vm docker create` komento luo automaattisesti myös sallimaan Docker asiakastietokone muodostaa yhteyden käyttämällä HTTPS Azuren säilö host tarvittavat varmenteet ja varmenteet tallennetaan asiakas- ja host tietokoneissa tarpeen mukaan. Valitse seuraavatkin yritykset aiemmin varmenteet uudelleen ja uusi isäntä jaettu.

Oletusarvon mukaan varmenteet sijoitetaan `~/.docker`, ja Docker määritetään toimimaan portin **2376**. Jos haluat käyttää eri portin tai kansio ja valitse jokin seuraavista voi käyttää `azure vm docker create` komentorivivalitsimet määrittäminen Docker säilö isännöidä AM käyttämään toiseen porttiin tai eri varmenteet asiakkaiden yhteyttä varten:

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

Docker daemon isännän on määritetty luoma varmenteiden käyttäminen määritetyn porttiin yhteyksien todennusta ja kuuntele `azure vm docker create` komento. Asiakkaan tietokoneeseen on oltava nämä varmenteet saat käyttöösi Docker isäntään.

> [AZURE.NOTE] Verkossa isäntä suoritetaan ilman nämä varmenteet ovat koske kuka tahansa, jolla voit muodostaa yhteyden tietokoneen. Ennen kuin muutat oletusarvoista määritykset, varmista ymmärtää riskit tietokoneilla ja sovelluksilla.

## <a name="next-steps"></a>Seuraavat vaiheet

* Olet valmis ja siirry [Docker käyttöoppaassa] Docker AM. Luo uuden portaalin Docker käyttävä AM, katso, [miten portaalissa Docker AM-tunniste].

* Azure Docker AM-tunniste tukee myös Docker luoda, joka käyttää määritettäviä YAML tiedoston developer mallintaa sovelluksen yli missä tahansa ympäristössä ja luo yhdenmukaisia käyttöönotto. Katso [Docker ja luo määrittäminen ja suorita Azure virtuaalikoneen usean säilö-ohjelman käytön aloittaminen].  

<!--Anchors-->
[Subheading 1]: #subheading-1
[Subheading 2]: #subheading-2
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

[How to use the Docker VM Extension with Azure]: #How-to-use-the-Docker-VM-Extension-with-Azure
[Virtual Machine Extensions for Linux and Windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows
[Container and Container Management Resources for Azure]: #Container-and-Container-Management-Resources-for-Azure



<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md
[Voit käyttää portaalin Docker AM-tunniste]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/

[Docker käyttöopas]: https://docs.docker.com/userguide/
 
[Docker ja luo määrittäminen ja suorita Azure virtuaalikoneen usean säilö-sovelluksen käytön aloittaminen]:virtual-machines-linux-docker-compose-quickstart.md