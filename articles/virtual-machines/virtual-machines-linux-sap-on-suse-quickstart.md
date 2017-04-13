<properties
   pageTitle="SAP NetWeaver-Microsoft Azure SUSE Linux VMs testaaminen | Microsoft Azure"
   description="Valitse Microsoft Azure SUSE Linux VMs SAP NetWeaver testaaminen"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="hermanndms"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="09/15/2016"
   ms.author="hermannd"/>

# <a name="running-sap-netweaver-on-microsoft-azure-suse-linux-vms"></a>Käynnissä olevat Microsoft Azure SUSE Linux VMs-SAP NetWeaver


Tässä artikkelissa kuvataan eri huomioon otettavat asiat, kun käytät SAP NetWeaver Microsoft Azure SUSE Linux näennäiskoneiden (VMs). Vuodesta 19 toukokuu 2016 SAP NetWeaver virallisesti tuetaan SUSE Linux VMs Azure. Linux-versiot, SAP ydin versiot ja niin edelleen kaikki tiedot löytyvät SAP Huomautus 1928533 "SAP-sovellusten Azure: tuetut tuotteet ja Azure AM".
SAP-Linux VMs edelleen ohjeista löytyy tähän: [Käyttämällä SAP-Linux näennäiskoneiden (VMs)](virtual-machines-linux-sap-get-started.md).


Seuraavat tiedot avulla voit välttää joitakin mahdollisia käsitellään.

## <a name="suse-images-on-azure-for-running-sap"></a>Kuvien SUSE Azure SAP suorittamista varten

Käynnissä SAP NetWeaver Azure, käytä vain SUSE Linux Enterprise Server SLES 12 (SPx) - Katso myös SAP Huomautus 1928533. Erityiset SUSE-kuva on Azure Marketplacesta ("SLES 11 SP3 SAP käyttö,"), mutta tämä ei ole tarkoitettu yleinen käyttö. Älä käytä tätä kuvaa, koska se on varattu [SAP Cloud laitteen kirjasto](https://cal.sap.com/) -ratkaisun.  

Kaikkien uusien testien ja Azure-asennusten käytettävä Azure Resurssienhallinta. Tarkista SUSE SLES kuvia ja versiot PowerShellin Azure tai Azure komentorivivalitsimet interface (CLI), käytä seuraavia komentoja. Sitten voit tuloksessa, esimerkiksi määrittää OS kuvan JSON mallin käyttöön uuden SUSE Linux-AM.
Nämä PowerShell-komennot ovat kelvollisia PowerShellin Azure versio 1.0.1 ja uudempi versio.

* Etsi aiemmin julkaisijat, mukaan lukien SUSE:

   ```
   PS  : Get-AzureRmVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* Etsi aiemmin tarjouksia SUSE kohteesta:

   ```
   PS  : Get-AzureRmVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```

* Etsi SUSE SLES tarjouksia:

   ```
   PS  : Get-AzureRmVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```

* Etsi SLES SKU tietyn version:

   ```
   PS  : Get-AzureRmVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```

## <a name="installing-walinuxagent-in-a-suse-vm"></a>Asentaminen WALinuxAgent SUSE AM

Kutsua WALinuxAgent agent on osa SLES kuvat Azure Marketplacesta. Lisätietoja asentamisesta manuaalisesti (esimerkiksi ladattaessa SLES OS virtual kiintolevyn (Näennäiskiintolevyn) paikallisen) on artikkelissa:

- [OpenSUSE] (http://software.opensuse.org/package/WALinuxAgent)

- [Azure] (virtual-koneet-linux-vahvistettava-distros.md)

- [SUSE] (https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/)

## <a name="sap-enhanced-monitoring"></a>SAP "parannetun seurannan"

SAP-parannetun seuranta"on pakollinen voidaan suorittaa SAP Azure. Tarkista tiedot SAP Huomaa 2191498 "SAP-Linux kanssa Azure: parannetun seuranta".

## <a name="attaching-azure-data-disks-to-an-azure-linux-vm"></a>Azure-Linux AM Azure tietojen levyjen liittäminen

Sinun tulee aina asennustapa Azure tietojen levyjen Azure-Linux AM käyttämällä laitteen ID-tunnuksellasi. Käytä yksilöllinen tunnus (UUID). Ole varovainen, kun käytät graafisia työkaluja asennustapa Azure tietojen levyjä, kuten. Tarkista /etc/fstab merkinnät.

Laitteen tunnus ongelma on, että se voi muuttua ja sitten Azure AM voivat katkaista käynnistyksen yhteydessä. Pienentämään ongelman voi lisätä nofail-parametrin /etc/fstab. Mutta varo nofail koska sovellukset voivat käyttää liityntäkohtaan ennen kuin ja voi kirjoittaa pääkansion tiedostojärjestelmän siltä varalta, että Azure ulkoiset tiedot-levy, joka ei ole otettu käyttöön käynnistyksen aikana.

Ainoa poikkeus kiinnittämiseen UUID kautta on liittäminen OS-levyn vianmääritystä, varten seuraavassa kohdassa kuvatulla tavalla.

## <a name="troubleshooting-a-suse-vm-that-isnt-accessible-anymore"></a>SUSE AM, joka ei enää voi käyttää vianmääritys

Ongelma saattaa olla tilanteet, joissa SUSE AM, valitse Azure jumittuu käynnistyksessä käsittelyssä (esimerkiksi virheeseen liittyviin käyttöönottaminen levyjä). Voit tarkistaa tämän ongelman käyttämällä käynnistyksen Diagnostiikka-toiminto Azuren näennäiskoneiden v2 Azure-portaalissa. Lisätietoja on artikkelissa [käynnistyksen diagnostiikka] (https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

Yksi tapa ongelman ratkaisemiseen on OS levyn liittäminen toisesta SUSE AM Azure-vioittunut AM. Tee tarvittavat muutokset, kuten /etc/fstab muokkaaminen tai poistaminen verkon udev säännöt seuraavan osion ohjeiden mukaisesti.

On tärkeää kuitenkaan ottaa huomioon. Useita SUSE VMs saman Azure Marketplace-kuvasta (esimerkiksi SLES 11 SP4) käyttöönotto aiheuttaa OS levylle aina käyttöön sama UUID mukaan. Tämän vuoksi käyttäminen UUID liitettävä OS-levyn eri AM, jotka on otettu käyttöön käyttämällä samaa Azure Marketplace-kuva-aiheuttaa kaksi samanlaista UUID. Tämä aiheuttaa ongelmia ja saattaa tarkoittaa, että tarkoitettu vianmääritys AM käynnistyy itse asiassa liitetty ja vioittunut OS levyltä alkuperäisessä sijaan.

Voit välttää tämän kahdella tavalla:

* Käytä eri Azure Marketplace-kuva vianmäärityksen AM (esimerkiksi SLES 11 SPx sijaan SLES 12).
* Ei liitä vioittunut OS levyn kohteesta toiseen AM käyttämällä UUID--Käytä jokin muu.

## <a name="uploading-a-suse-vm-from-on-premises-to-azure"></a>SUSE AM paikallisen from Azure lataaminen

Kuvaus vaiheita Lataa SUSE AM paikallisen from Azure-kohdassa [valmisteleminen SLES tai openSUSE virtual kone, Azure] (virtual-machines-linux-suse-create-upload-vhd.md).

Jos haluat ladata AM ilman lopussa deprovision-vaihe (esimerkiksi haluat pitää aiemmin luotua SAP-asennusta sekä isäntänimi), tarkista seuraavat seikat:

* Varmista, että OS levy on otettu käyttöön UUID ja ei on laite. Vain /etc/fstab-UUID muuttaminen ei ole tarpeeksi OS levyn. Myös Älä unohda mukauttaa käynnistyksessä YaST tai muokkaamalla /boot/grub/menu.lst.
* Jos käytät VHDX muoto SUSE OS levyn ja muuntaminen Näennäiskiintolevyn Azure ladataan, on todennäköistä, että verkkolaitteen muuttuu eth0 osoitteesta eth1. Voit välttää ongelmat, kun olet käynnistäminen Azure-on myöhemmin, muuta takaisin eth0 kuvatulla tavalla [säätämisessä eth0-kloonatun SLES 11 VMware] (https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/).

Lisäksi mikä on kuvattu artikkelissa on suositeltavaa, että poistat tämän:

   /lib/udev/rules.d/75-persistent-NET-Generator.rules

Voit myös asentaa Azure Linux agentti (waagent) avulla voit välttää ongelmia, kun useita NIC eivät ole.

## <a name="deploying-a-suse-vm-on-azure"></a>SUSE AM, valitse Azure käyttöönotto

Luo uusi SUSE VMs käyttämällä JSON mallitiedostot mallissa Azure Resurssienhallinta. JSON-mallitiedoston luomisen jälkeen AM voit ottaa käyttöön käyttämällä PowerShell vaihtoehtona CLI seuraava komento:

   ```
   azure group deployment create "<deployment name>" -g "<resource group name>" --template-file "<../../filename.json>"

   ```
Lisätietoja JSON mallitiedostot, näet [yhtä aikaa muiden kanssa Azure Resurssienhallinta mallit] (resurssi-ryhmä-yhtä aikaa muiden kanssa-templates.md) ja [Azure pikaopas mallit] (https://azure.microsoft.com/documentation/templates/).

Katso lisätietoja CLI ja Azure Resurssienhallinta [käyttäminen Azure-CLI (Mac), Linux ja Windows Azure resurssien hallinnan] (xplat-cli-azure-resurssi-manager.md).

## <a name="sap-license-and-hardware-key"></a>SAP-käyttöoikeus- ja avain

Virallinen SAP Azure-sertifikaatin uusi järjestelmä otettiin laskemiseen SAP-laitteen avain, jota käytetään SAP-käyttöoikeuden. SAP-ydin oli mukautettava, jotta voit käyttää tätä. Linux entisen SAP ydin versiot eivät kuulu koodin muutos. Tämän vuoksi kaikissa tilanteissa (esimerkiksi Azure AM koon), muuttaa SAP laitteisto-näppäintä ja SAP-käyttöoikeus on on voimassa. Tämä on ratkaistu uusimman SAP Linux ytimet. Lisätietoja Tarkista SAP Huomautus 1928533.

## <a name="suse-sapconf-package--tuned-adm"></a>SUSE sapconf package / virittää adm

SUSE on paketin nimeltä "sapconf", joka hallitsee määritetyn SAP-asetuksia. Lisätietoja tämän paketin mitä ja asentaa ja käyttää sitä, on artikkelissa [sapconf avulla valmisteleminen SUSE Linux Enterprise palvelimeen ja suorita SAP-järjestelmiin] (https://www.suse.com/communities/blog/using-sapconf-to-prepare-suse-linux-enterprise-server-to-run-sap-systems/) ja [mikä on sapconf tai käynnissä SAP-järjestelmiin SUSE Linux Enterprise palvelimen valmistelemisesta?] (http://scn.sap.com/community/linux/blog/2014/03/31/what-is-sapconf-or-how-to-prepare-a-suse-linux-enterprise-server-for-running-sap-systems).

Tällä välin on uusi työkalu, joka korvaa sapconf - virittää adm. Yksi voit etsiä lisätietoja tämän työkalun seuraavan kahden alla olevia linkkejä.

SLES ohjeista virittää adm profiilin sap-hana löytyy [tähän](https://www.suse.com/documentation/sles-for-sap-12/book_s4s/data/sec_s4s_configure_sapconf.html) 

SAP-toiminnoista ja virittää-adm - säätäminen järjestelmiä muutettavista [tähän](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/book_s4s/book_s4s.pdf) luvun 6.2


## <a name="nfs-share-in-distributed-sap-installations"></a>NFS jakaa hajautettu SAP-asennukset

Jos sinulla on jaettu asennus – esimerkiksi kohtaa, johon haluat asentaa tietokannan ja SAP-sovelluksen palvelinten erillisessä VMs – voit jakaa /sapmnt hakemiston kautta verkon tiedostojärjestelmän (NFS). Jos sinulla on ongelmia asennusohjeita kanssa, kun olet luonut /sapmnt NFS Jaa, tarkista, jos "no_root_squash" on määritetty Jaa.

## <a name="logical-volumes"></a>Loogisen tietomääristä

Aiemman jokin tarvittaessa looginen erittäin useille Azure tietojen levyille (esimerkiksi SAP-tietokanta), on suositeltavaa käyttää mdadm lvm ei ole täysin vahvistaa vielä Azure. Voit määrittää Linux RAID Azure-käyttämällä mdadm-kohdassa [Määritä ohjelmiston RAID Linux](virtual-machines-linux-configure-raid.md). Tällä välin toukokuu 2016 alkuun saavuttaman myös-lvm tuetaan täysin Azure ja voidaan käyttää vaihtoehtona mdadm. Saat lisätietoja Azure koskeva lvm [Määrittäminen LVM-Linux-AM Azure-tietokannassa](virtual-machines-linux-configure-lvm.md).


## <a name="azure-suse-repository"></a>Azure SUSE säilöön

Jos sinulla on ongelma Accessissa tavallisen Azure SUSE säilöön, voit nollata salasanasi yksinkertainen komennon. Näin voi käydä, jos Luo yksityinen OS kuva Azure alueen ja kopioida sitten kuvan toisella alueella, jossa haluat ottaa käyttöön uuden VMs, jotka perustuvat yksityinen OS kuvassa. Vain varten suorittamalla seuraavan komennon sisällä AM:

   ```
   service guestregister restart
   ```

## <a name="gnome-desktop"></a>Gnome työpöydällä

Jos haluat asentaa täydellisen SAP esittely järjestelmän sisällä yhden AM – kuten SAP-Graafisen Gnome työpöydän avulla selaimessa ja SAP-hallintakonsoli--Käytä asennetaan Azure SLES kuvat tässä Vihje:

   Saat SLES 11:

   ```
   zypper in -t pattern gnome
   ```

   Saat SLES 12:

   ```
   zypper in -t pattern gnome-basic
   ```

## <a name="sap-support-for-oracle-on-linux-in-the-cloud"></a>Oracle Linux pilveen SAP-tuki

Ei tukea rajoitus Linux Oracle-virtualisoidussa ympäristöissä. Tämä ei ole Azure-kohtaisia ohjeaiheen, mutta se on tärkeää ymmärtää. SAP: in ei tue Oracle SUSE tai julkinen Pilvipalvelun, kuten Azure on punainen. Keskustele tämän ohjeaiheen, ota yhteyttä Oracle.
