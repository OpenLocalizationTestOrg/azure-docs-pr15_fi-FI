<properties
    pageTitle="Linux ja Avaa lähde-Azure tietojenkäsittely | Microsoft Azure"
    description="Näyttää Linux ja Avaa lähde tietojenkäsittely artikkeleita Azure, mukaan lukien Linux peruskäyttö joitakin peruskäsitteet käynnissä tai Linux Azure ja muita tietoja tietyn Technologies-tuote ja optimointi kuvat ladataan."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="06/27/2016"
    ms.author="rasquill"/>



# <a name="linux-and-open-source-computing-on-azure"></a>Linux ja Avaa lähde-Azure tietojenkäsittely

Etsi kaikki asiakirjat, sinun täytyy luoda ja hallita näennäiskoneiden Linux-pohjaiset perinteinen käyttöönoton mallia.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="get-started"></a>Käytön aloittaminen
- [Linux Azure-esittely](virtual-machines-linux-intro-on-azure.md)
- [Usein kysyttyjä kysymyksiä tietoja Azuren näennäiskoneiden luotu perinteinen käyttöönottomalli](virtual-machines-linux-classic-faq.md)
- [Lisätietoja kuvien näennäiskoneiden](virtual-machines-linux-classic-about-images.md)
- [Ladataan Distro-kuva](virtual-machines-linux-classic-create-upload-vhd.md) (ja myös ohjeita käyttämällä [Azure-Endorsed jakauman](virtual-machines-linux-endorsed-distros.md))
- [Kirjaudu sisään Linux AM käyttämällä Azure perinteinen portal](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="set-up"></a>Määrittäminen

- [Asenna Azure käyttöliittymä (Azure CLI)](../xplat-cli-install.md)


## <a name="tutorials"></a>Opetusohjelmat

- [Asenna hehkulampun pino Linux virtual tietokoneeseen Azure-tietokannassa](virtual-machines-linux-create-lamp-stack.md)
- [Ruby Azure-AM kulkevia verkkosovelluksen](linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md)
- [Toimintaohje: Asenna Apache Qpid Proton-C AMQP ja palvelun Bus](../service-bus-messaging/service-bus-amqp-apache.md)

### <a name="databases"></a>Tietokannat
- [MySQL-Azure suorituskyvyn parantamiseksi](virtual-machines-linux-classic-optimize-mysql.md)
- [MySQL klustereiden](virtual-machines-linux-classic-mysql-cluster.md)
- [Cassandra käytön Linux Azure- ja Node.js avaaminen](virtual-machines-linux-classic-cassandra-nodejs.md)
- [MariaDbs usean pääkohteen klusterin luominen](virtual-machines-linux-classic-mariadb-mysql-cluster.md)

### <a name="hpc"></a>HPC
- [Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa käytön aloittaminen](virtual-machines-linux-classic-hpcpack-cluster.md)
- [Suorita NAMD Microsoft HPC Pack Azure Linux Laske solmuissa](virtual-machines-linux-classic-hpcpack-cluster-namd.md)
- [Linux RDMA klusterin MPI sovelluksia määrittäminen](virtual-machines-linux-classic-rdma-cluster.md)

### <a name="docker"></a>Docker
- [Liittymää käyttäen komentorivin azuren Docker AM-tunniste (Azure CLI)](virtual-machines-linux-classic-cli-use-docker.md)
- [Käyttämällä Azure-portaalista Docker AM-tunniste](virtual-machines-linux-classic-portal-use-docker.md)
- [Azure docker tietokoneen käyttämisestä](virtual-machines-linux-docker-machine.md)

### <a name="ubuntu"></a>Ubuntu
- [Toimintaohje: MySQL klustereiden](virtual-machines-linux-classic-mysql-cluster.md)
- [Toimintaohje: Node.js ja Cassandra](virtual-machines-linux-classic-cassandra-nodejs.md)

### <a name="opensuse"></a>OpenSUSE
- [Toimintaohje: asentaminen ja suorittaminen MySQL](virtual-machines-linux-classic-mysql-on-opensuse.md)

### <a name="coreos"></a>CoreOS
- [Toimintaohje: Azure CoreOS käyttäminen](https://coreos.com/os/docs/latest/booting-on-azure.html)


## <a name="planning"></a>Suunnittelu
- [Azure infrastruktuurin palvelujen käyttöönoton ohjeita](virtual-machines-linux-infrastructure-subscription-accounts-guidelines.md)
- [Linux käyttäjänimet valitseminen](virtual-machines-linux-usernames.md)
- [Määritä näennäiskoneiden perinteinen käyttöönoton mallin saatavuus määrittäminen](virtual-machines-linux-classic-configure-availability.md)
- [Azure VMs suunnitellun ylläpidon ajoittamisesta](virtual-machines-linux-planned-maintenance-schedule.md)
- [Näennäiskoneiden käytettävyyden hallinta](virtual-machines-linux-manage-availability.md)
- [Suunniteltu ylläpito Linux näennäiskoneiden Azure](virtual-machines-linux-planned-maintenance.md)


## <a name="deployment"></a>Käyttöönotto
- [Luo mukautettu virtual koneeseen, jossa käytetään Linux](virtual-machines-linux-classic-createportal.md)
- [Perusteet: sieppaus Linux AM, voit luoda malleja](virtual-machines-linux-classic-capture-image.md)
- [Tietoja ei ole vahvistanut jakelu](virtual-machines-linux-create-upload-generic.md)


## <a name="management"></a>Hallinta

- [SSH](virtual-machines-linux-mac-create-ssh-keys.md)
- [Miten vaihtamaan Linux salasanan tai SSH ominaisuudet](virtual-machines-linux-classic-reset-access.md)
- [Pääkansio käyttäminen](virtual-machines-linux-use-root-privileges.md)


## <a name="azure-resources"></a>Azure-resurssit

- [Azure Linux-agentti](virtual-machines-linux-agent-user-guide.md)
- [Azure AM tunnisteet ja toiminnot](virtual-machines-windows-extensions-features.md)
- [Injisoimalla mukautettu-tietojen tuominen AM Cloud alusta käyttäminen](virtual-machines-windows-classic-inject-custom-data.md)


## <a name="storage"></a>Tallennustilan

- [Tietoja DVD-levyllä liittäminen Linux AM](virtual-machines-linux-classic-attach-disk.md)
- [Sivuasettelusta Linux AM tietojen levyltä](virtual-machines-linux-classic-detach-disk.md)
- [RAID](virtual-machines-linux-configure-raid.md)


## <a name="networking"></a>Verkko
- [Voit määrittää päätepisteet perinteinen virtual koneeseen Azure-tietokannassa](virtual-machines-linux-classic-setup-endpoints.md)


## <a name="troubleshooting"></a>Vianmääritys
- [Koneen Azure virtual Linux-pohjaiset suojattu runko (SSH)-yhteyksien vianmääritys](virtual-machines-linux-troubleshoot-ssh-connection.md)
- [Perinteinen käyttöönoton luomisessa uuden Linux virtual koneen Azure ongelmien vianmääritys](virtual-machines-linux-classic-troubleshoot-deployment-new-vm.md)  
- [Perinteinen käyttöönoton vianmääritys uudelleenkäynnistyksen tai aiemmin luotuun Virtual Linux-Machine, Azure-tietokannassa koon muuttaminen](virtual-machines-linux-classic-restart-resize-error-troubleshooting.md) 


## <a name="reference"></a>Viittaus

- [Azure CLI komentoja Azure Service Management (asm)-tilassa.](../virtual-machines-command-line-tools.md)
- [Azure hallinnan REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/ee460799.aspx)




## <a name="general-links"></a>Yleiset linkit
Seuraavat linkit ovat Microsoft blogit, Technet-sivut ja ulkoiset sivustot sijaan Azure.com dokumentaatio kuin yllä. Azure ja Avaa lähde tietojenkäsittely maailman ovat nopea siirtäminen kohteet, se on lähes varma, että seuraavat linkit ovat vanhentunut *huolimatta* kertoma on toteuttavat parhaat jatkuvasti uudempaan aiheet lisäämään ja poistamaan vanhentuneen niistä. Jos Microsoft on vastattu, kirjoita uutiskirjeistä tietää kommenttien tai Lähetä pyyntö Microsoftin [GitHub repo](https://github.com/Azure/azure-content/)salaus puretaan.

- [ASP.NET-5-käyttöjärjestelmässä käyttämällä Docker säilöjen Linux](http://blogs.msdn.com/b/webdev/archive/2015/01/14/running-asp-net-5-applications-in-linux-containers-with-docker.aspx)
- [Ottamisesta käyttöön OpenLogic CentOS AM kuva](https://azure.microsoft.com/blog/2013/01/11/deploying-openlogic-centos-images-on-windows-azure-virtual-machines/)
- [SUSE päivityksen infrastruktuuri](https://forums.suse.com/showthread.php?5622-New-Update-Infrastructure)
- [SAP Cloud laitteen kirjaston SUSE Linux Enterprise palvelimeen](https://azure.microsoft.com/marketplace/partners/suse/suselinuxenterpriseserver11sp3forsapcloudappliance/)
- [Erittäin käytettävissä Linux pohjalta Azure 12 vaiheet](http://blogs.technet.com/b/keithmayer/archive/2014/10/03/quick-start-guide-building-highly-available-linux-servers-in-the-cloud-on-microsoft-azure.aspx)
- [Automatisoida valmistelu Linux-Azure Azure CLI, node.js, jhawk](http://blogs.technet.com/b/keithmayer/archive/2014/11/24/step-by-step-automated-provisioning-for-linux-in-the-cloud-with-microsoft-azure-xplat-cli-json-and-node-js-part-1.aspx)
- [Valitse Azure GlusterFS](http://dastouri.azurewebsites.net/gluster-on-azure-part-1/)

### <a name="freebsd"></a>FreeBSD
- [Käynnissä FreeBSD Azure-tietokannassa](https://azure.microsoft.com/blog/2014/05/22/running-freebsd-in-azure/)
- [Helppo FreeBSD käyttöönotto](http://msopentech.com/blog/2014/10/24/easy-deploy-freebsd-microsoft-azure-vm-depot/)
- [Mukautetun FreeBSD näköistiedoston käyttöönotto](http://msopentech.com/blog/2014/05/14/deploy-customize-freebsd-virtual-machine-image-microsoft-azure/)
- [Kaspersky AV Linux tiedoston palvelimen](https://azure.microsoft.com/marketplace/partners/kaspersky-lab/kav-for-lfs-kav-for-lfs/)

### <a name="nosql"></a>NoSQL

- [Avaa lähde NoSql 8 tietokantojen Azure](http://openness.microsoft.com/blog/2014/11/03/open-source-nosql-databases-microsoft-azure/)
- [Slideshare (MSOpenTech): Valitse Azure CouchDb kokemukset](http://www.slideshare.net/brianbenz/experiences-using-couchdb-inside-microsofts-azure-team)
- [CouchDB-muodossa--palvelua node.js, CORS ja Grunt](http://msopentech.com/blog/2013/12/19/tutorial-building-multi-tier-windows-azure-web-application-use-cloudants-couchdb-service-node-js-cors-grunt-2/)

- [Windows Azure Redis.txt välimuisti-palvelun Redis](http://msopentech.com/blog/2014/05/12/redis-on-windows/)
- [Julkaisu ASP.NET-istunnon tila-palvelu Redis.txt Preview-versio](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)

- [Blogi: RavenHQ on nyt käytettävissä Azure Marketplacesta](https://azure.microsoft.com/blog/2014/08/12/ravenhq-now-available-in-the-azure-store/)

### <a name="big-data"></a>Big datasta
- [Hadoop asentamisessa Azure Linux VMs](http://blogs.msdn.com/b/benjguin/archive/2013/04/05/how-to-install-hadoop-on-windows-azure-linux-virtual-machines.aspx)
- [Azure Hdinsightiin](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/)

### <a name="relational-database"></a>Relaatiotietokanta
- [Microsoft Azure MySQL suuri käytettävyys arkkitehtuurista](http://download.microsoft.com/download/6/1/C/61C0E37C-F252-4B33-9557-42B90BA3E472/MySQL_HADR_solution_in_Azure.pdf)
- [Asennuksen Postgres corosync, pg_bouncer ILB käyttäminen](https://github.com/chgeuer/postgres-azure)

### <a name="linux-high-performance-computing-hpc"></a>Linux tehokas tietojenkäsittely (HPC)

- [Pikaopas mallin: SLURM-klusterin asettamasi](https://github.com/Azure/azure-quickstart-templates/tree/master/slurm) (ja [blogimerkintä](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx))
- [Pikaopas mallin: Linux Laske solmujen HPC-klusterin luominen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

### <a name="devops-management-and-optimization"></a>Devops, hallinnasta ja optimointi

Hallinta ja optimointi on aivan laajojen devops maailman ja muuttaminen nopeasti, kannattaa harkita alapuolella pohjana olevassa luettelossa.

- [Video: Azuren näennäiskoneiden: käyttämällä kokki, Puppet ja Docker Linux VMs hallintaan](https://azure.microsoft.com/blog/2014/12/15/azure-virtual-machines-using-chef-puppet-and-docker-for-managing-linux-vms/)

- [Automaattisen Kubernetes klusterikäyttöönoton CoreOS ja kudos valmis opas](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
- [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)

- [Jenkins toissijaisen Azure laajennus](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
- [GitHub repo: Jenkins tallennustilan Azure laajennus](https://github.com/jenkinsci/windows-azure-storage-plugin)

- [Kolmannen osapuolen: Hudson toissijaisen Azure laajennus](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
- [Kolmannen osapuolen: Hudson tallennustilan Azure laajennus](https://github.com/hudson3-plugins/windows-azure-storage-plugin)

- [Video: Mikä on kokki ja kuinka se toimii?](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

- [Video: Voit käyttää Azure automaatio Linux VMs](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)

- [Blogi: Voit tehdä Powershell DSC Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
- [GitHub: Docker asiakkaan DSC](https://github.com/anweiss/DockerClientDSC)

- [Azure pakkaajan ‑laajennuksen](https://github.com/msopentech/packer-azure)
