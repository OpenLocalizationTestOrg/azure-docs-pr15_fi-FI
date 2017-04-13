 <properties
   pageTitle="Azure ja Linux | Microsoft Azure"
   description="Tässä artikkelissa kuvataan Linux näennäiskoneiden Azure Laske, varasto ja verkko-palvelun."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines-linux"
   authors="vlivech"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/14/2016"
   ms.author="v-livech"/>

# <a name="azure-and-linux"></a>Azure ja Linux

Microsoft Azure on integroitu julkisen valikoiman cloud services, kuten analytics-näennäiskoneiden-tietokantoja, mobiili-verkko-tallennus- ja web – erinomaisesti ratkaisujen isännöintipalvelu.  Microsoft Azure sisältää, jonka avulla voit käyttää vain maksaa skaalattava tietojenkäsittely ympäristö, kun haluat sen - eikä sinun tarvitse sijoittaa paikallisen laitteen.  Azure on valmis, kun olet skaalata ratkaisujen ja ulos jostakin näytöstä asetat vastaamisessa asiakkaiden tarpeisiin.

Jos olet tutustunut Amazon's eri toimintoja sisältyy AWS, voit tutkia Azure ja sisältyy AWS [määritelmä yhdistäminen asiakirjaan](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).


## <a name="regions"></a>Alueiden
Microsoft Azure resurssien jakautuu useita eri puolilla maailmaa alueittain.  "Alue" edustaa useita yhden maantieteellisen alueen tietojen keskikohdan mukaan.  Vuodesta 1 tammikuussa 2016, mukaan lukien: Amerikassa 2 Euroopassa Aasian ja Tyynenmeren alueen, Manner Kiinassa 2 ja 3 Intiassa 6-8.  Jos haluat kaikkien Azure alueiden kattavaan luetteloon, emme ylläpitää luettelon olemassa olevien ja juuri ilmoitettiin alueet.

- [Azure alueet](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Käytettävyys
Jotta meidän 99.95 AM tason palvelusopimus oikeutettu käyttöönoton haluat ottaa käyttöön kaksi tai useampi virtuaalilaitteiksi havainnollistamiseen sisältyy saatavuus määritettynä. Näin varmistat, että VMs on useita Microsoftin tietojen keskikohdan mukaan vika toimialueita jakautuminen sekä käyttöönoton eri ylläpito Windows isännät. Koko [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) kerrotaan kokonaisuutena Azure taattua käytettävyyttä.

## <a name="azure-virtual-machines--instances"></a>Azure-Virtuaalikoneissa & esiintymät
Microsoft Azure tukee käynnissä Suositut Linux jaot annettu ja ylläpitämä osakkaiden lukumäärä määrä.  Etsii jaot, kuten punainen on Enterprise, CentOS, Debian, Ubuntu, CoreOS, RancherOS, FreeBSD Azure Marketplacesta. Olemme aktiivisesti käsitellä eri Linux yhteisöjen paremmin flavors lisääminen [Azure vahvistettava Linux Distros](virtual-machines-linux-endorsed-distros.md) -luetteloon.

Jos yhteyttä ensisijainen Linux distro värisinä ei ole tällä hetkellä ole valikoimassa, voit "tuoda Omat Linux" AM [luomalla](virtual-machines-linux-create-upload-generic.md)ja lataamisesta Linux Näennäiskiintolevyn Azure-tietokannassa.

Azure-virtuaalikoneissa avulla voit ottaa käyttöön monien tietojenkäsittely ratkaisuja joustava tavalla. Voit ottaa käyttöön miltei mistä tahansa työmäärää- ja minkä tahansa kielen lähes minkä tahansa käyttöjärjestelmä - Windows-Linux- tai mukautetun luonut jostakin kumppanien kasvavia luettelossamme. Ei vieläkään näy mitä etsit?  Älä huoli – voit myös tuoda Omat kuvat-ympäristöön.

## <a name="vm-sizes"></a>AM koot
Kun otat käyttöön AM Azure-tietokannassa, aiot Valitse AM johonkin sarjan valikoima kokoja, joka sopii havainnollistamiseen. Koko vaikuttaa myös virtuaalikoneen käsittely power, muistin ja tallennustilaa kapasiteetti. Ovat laskuttaa ajan AM on käynnissä ja muissa sen varattujen resurssien mukaan. Luettelo kaikista [näennäiskoneiden kokoisiksi](virtual-machines-linux-sizes.md).

Seuraavassa on joitakin perusohjeita valitsemiseen AM koon olevista sarjan (A, D, DS, G ja GS).

* A-sarjan VMs on Microsoftin hinnoiteltu ensimmäinen VMs Vaalea toiminnoista ja keskihajonta/testi skenaarioiden arvo. Ne ovat käytettävissä kaikilla alueilla ja muodostaa ja kaikki näennäiskoneiden käytettävissä standard resurssit.
* A-sarjan koot (A8 - A11) ovat erityisiä Laske tehostettu käyttömahdollisuudet sopii tehokas tietojenkäsittely klusterin sovellukset.
* D-sarjan VMs on suunniteltu sovelluksia, jotka vaatimus suurempi Laske power ja tilapäinen suorituskyvyn. D-sarjan VMs tilapäinen levyn avulla voit nopeampaa suorittimien ja muistin core suhde SSD asema (Suoritettaessa).
* Dv2 sarjan, on Microsoftin D-sarjan uusimman version, tehokkaampia suorittimen ominaisuudet. Dv2 sarjan Suoritin on noin 35 % D-sarjan suorittimen nopeammin. Se perustuu uusinta 2,4 GHz Intel Xeon® E5-2673 v3 suoritin (Haskell) ja Intel Turbo tehon lisäys tekniikka-2.0, voit siirtyä enintään 3.2 GHz. Dv2-sarjan on sama muistia ja määrityksiä D-arvosarjaa.
* G-sarjan VMs tarjota eniten muistia ja suorittamisesta isännät, joissa on Intel Xeon E5 V3 perhe-suorittimista.

Huomautus: DS-sarjan ja GS sarjan VMs on pääsy Premium tallennustilan – Tutustu Suoritettaessa palautettua tehokas, pieni viive tallennustila i/o tehostettu toiminnoista. Premium tallennustila on tiettyjä alueita. Lisätietoja on artikkelissa:

- [Premium tallennustila: Azure virtuaalikoneen työmääriä tehokas tallennustila](../storage/storage-premium-storage.md)

## <a name="automation"></a>Automaatio
Oikea DevOps kulttuuri saavuttamiseksi kaikki infrastruktuurin on oltava koodi.  Kun kaikki infrastruktuurin sijaitsevat koodi voi olla helposti uudelleen (Pori-palvelimet).  Azure toimii pää automaatio tooling kuten Ansible, kokki, SaltStack ja Puppet.  Azure on myös omassa sillä automatisointiin:

- [Azure-mallit](virtual-machines-linux-create-ssh-secured-vm-from-template.md)

- [Azure VMAccess](virtual-machines-linux-using-vmaccess-extension.md)

Azure on juoksevan tuki [cloud alusta](http://cloud-init.io/) , useimmat Linux-Distros, jotka tukevat sitä kautta.  Tällä hetkellä Canonical's Ubuntu VMs otetaan käyttöön cloud alusta oletusarvon mukaan käytössä.  RedHats RHEL, CentOS ja Fedora tukevat cloud alusta, mutta Azure kuvat ylläpitämä RedHat ei ole asennettu cloud-alusta.  Cloud alusta käyttämisestä RedHat perhe OS, sinun on luotava mukautetun kuvan cloud alusta asennettu kanssa.

- [Cloud alusta käyttäminen Azure Linux VMs](virtual-machines-linux-using-cloud-init.md)

## <a name="quotas"></a>Kiintiön
Kunkin Azure-tilaus on oletusarvoinen kiintiötä paikkaa, jossa on runsaasti VMs projektin käyttöönotto voi olla vaikutusta. Kohti tilauksen välein nykyisen enimmäismäärä on 20 VMs alueittain.  Kiintiötä aiheutuneet voi pyytää raja-Suurenna tuki lippu siirtämiseen.  Lisätietoja kiintiötä:

- [Azure tilauksen palvelun rajoitukset](../azure-subscription-service-limits.md)


## <a name="partners"></a>Kumppaneille

Microsoft toimii hyvin käytettävissä olevat kuvat ovat päivitetään ja Azure runtime optimoitu kumppaneita.  Lisätietoja kumppaneita Tarkista niiden Marketplacen sivuille alla.

- [Valitse Azure vahvistettava jaot Linux](virtual-machines-linux-endorsed-distros.md)

Redhat - [Azure Marketplace - RedHat yrityksen Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)

Kanoninen - [Azure Marketplace - Ubuntu palvelimen 16.04 l.](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)

Debian - [Azure Marketplace - Debian 8 "Jessie"](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)

FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)

CoreOS - [Azure Marketplace - CoreOS (vakaa)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)

RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)

Bitnami - [Azure Bitnami kirjasto](https://azure.bitnami.com/)

Mesosphere - [Azure Marketplace - Mesosphere Ohjauskoneen/OS Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)

Docker - [Azure Marketplace - Azure säilö palvelun Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)

Jenkins - [Azure Marketplace - CloudBees Jenkins ympäristö](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)


## <a name="getting-setup-on-azure"></a>Azure-käytön asetukset
Kun haluat käyttää Azure tarvitset Azure-tili, asennettu Azure CLI ja kahdet SSH julkiset ja yksityiset avaimet.

## <a name="sign-up-for-an-account"></a>Tilin rekisteröityminen
Käyttämällä Azure pilveen ensimmäiseksi on rekisteröitävä Azure-tili.  Siirry [Azure tilin](https://azure.microsoft.com/pricing/free-trial/) kirjautumissivulle avulla pääset alkuun.

## <a name="install-the-cli"></a>Asenna CLI
Uusi Azure-tilisi kanssa voit aloittaa käyttämällä heti Azure-portaaliin, joka on verkkopohjainen järjestelmänvalvoja-ruutu.  Hallittavan Azure pilven kautta komentorivillä asennat `azure-cli`.  [Azure CLI ](../xplat-cli-install.md)asentaminen Mac tai Linux työasema.

## <a name="create-an-ssh-key-pair"></a>Luo avaimen SSH-pari
Nyt käytössäsi on Azure-tili, Azure web-portaaliin ja Azure-CLI.  Seuraavaksi voit luoda SSH avaimen pari, jota käytetään SSH kyselyjä Linux ilman salasanaa.  [Luo SSH näppäimet Linux- ja Mac-](virtual-machines-linux-mac-create-ssh-keys.md) salasana pienempi kirjautumiset ja paremman suojauksen ottaminen käyttöön.

## <a name="getting-started-with-linux-on-microsoft-azure"></a>Valitse Microsoft Azure Linux käytön aloittaminen
Azure-tili-asetukset, Azure CLI asennettu ja luodut SSH pikanäppäimet voit nyt jatkaa luomisesta, infrastruktuuri Azure pilveen.  Ensimmäiseksi on luotava pari VMs.

## <a name="create-a-vm-using-the-cli"></a>Luo AM, käyttämällä CLI
Linux-AM CLI avulla luodaan nopeasti käyttöön AM poistumatta pääte käsittelet.  Kaikki voit määrittää web-portaalissa on saatavana komentorivin merkinnän tai vaihda.  

- [Luo Linux-AM CLI käyttäminen](virtual-machines-linux-quick-create-cli.md)

## <a name="create-a-vm-in-the-portal"></a>Luo AM-portaalissa
Linux AM luominen Azure web-portaalissa on tapa helposti ja valitse kautta pääset käyttöönoton eri vaihtoehdoista.  Komentorivin liput tai valitsimet sijaan olet voivat tarkastella nice WWW-asettelu eri ja asetuksia.  Kaikki komentorivivalitsimet-liittymän kautta sisältyy myös portaalin.

- [Luo Linux-AM-portaalissa](virtual-machines-linux-quick-create-portal.md)

## <a name="login-using-ssh-without-a-password"></a>Kirjaudu sisään käyttämällä SSH ilman salasanaa
Azure on nyt käytössä AM ja olet valmis, kirjaudu sisään.  Kirjaudu sisään kautta SSH salasanojen on suojaamattoman ja aikaa.  SSH-näppäimellä on turvallisin tapa ja kirjautuminen nopein tapa.  Kun luot voit Linux AM portaalin tai CLI, sinulla on kaksi todennus-vaihtoehtoa.  Jos määrität salasanan SSH, Azure määrittää AM sallimaan kirjautumiset salasanat kautta.  Jos käytät projektissa SSH julkisella avaimella, Azure määrittää päästää vain kirjautumiset kautta SSH näppäimet AM sekä poistaa salasanan kirjautumiset. Voit suojata Linux AM sallimalla vain SSH avaimen kirjautumiset-käyttämällä SSH julkisen avaimen asetusta portal tai CLI AM luonnin aikana.

- [Poista käytöstä SSH salasanat Linux AM määrittämällä SSHD](virtual-machines-linux-mac-disable-ssh-password-usage.md)

## <a name="related-azure-components"></a>Azure osia

## <a name="storage"></a>Tallennustilan

- [Johdanto Microsoft Azuren tallennustilaan](../storage/storage-introduction.md)

- [Lisää käyttämällä azure-cli Linux-AM DVD-levyllä](virtual-machines-linux-add-disk.md)

- [Tietoja DVD-levyllä liittämisestä Linux-AM Azure-portaalissa](virtual-machines-linux-attach-disk-portal.md)

## <a name="networking"></a>Verkko

- [VPN-yleiskatsaus](../virtual-network/virtual-networks-overview.md)

- [Azure IP-osoitteet](../virtual-network/virtual-network-ip-addresses-overview-arm.md)

- [Avaaminen Linux AM portit Azure-tietokannassa](virtual-machines-linux-nsg-quickstart.md)

- [Luoda täysin täydellinen toimialuenimi Azure-portaalissa](virtual-machines-linux-portal-create-fqdn.md)


## <a name="containers"></a>Säilöjen

- [Näennäiskoneiden ja säilöjä Azure-tietokannassa](virtual-machines-linux-containers.md)

- [Azure säilö-palvelun esittely](../container-service/container-service-intro.md)

- [Azure säilö Service-klusterin käyttöönotto](../container-service/container-service-deployment.md)

## <a name="next-steps"></a>Seuraavat vaiheet

Valitse Azure on nyt Linux yleiskatsaus.  Seuraava vaihe on hyppäämään mukaan ja luo muutaman VMs!

- [Luo Linux AM Azure-portaalissa](virtual-machines-linux-quick-create-portal.md)

- [Luoda Linux AM Azure CLI käyttämällä](virtual-machines-linux-quick-create-cli.md)
