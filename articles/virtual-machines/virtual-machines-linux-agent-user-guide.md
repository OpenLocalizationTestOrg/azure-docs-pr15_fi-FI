<properties 
    pageTitle="Linux agentti käyttöoppaassa | Microsoft Azure" 
    description="Lue, miten asennetaan ja määritetään hallittavan virtuaalikoneen 's vuorovaikutus Azure kangasta ohjaimella agentti Linux (waagent)." 
    services="virtual-machines-linux" 
    documentationCenter="" 
    authors="szarkos" 
    manager="timlt" 
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags 
    ms.service="virtual-machines-linux" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-linux" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="szark"/>



# <a name="azure-linux-agent-user-guide"></a>Azure Linux-agentti käyttöopas

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="introduction"></a>Johdanto

Microsoft Azure Linux Agent (waagent) hallitsee Linux ja FreeBSD valmistelu ja AM vuorovaikutus Azure kangasta ohjaimella. Se on seuraavat ominaisuudet Linux ja FreeBSD IaaS ominaisuuksissa:

> [AZURE.NOTE] Katso lisätietoja Azure Linux-agentti [Lueminut-tiedosto](https://github.com/Azure/WALinuxAgent/blob/master/README.md) .

* **Kuva valmistelu**
  - Käyttäjätilin luominen
  - SSH todennustyypit määrittäminen
  - SSH julkinen avain ja avaimen
  - Isäntänimi määrittäminen
  - Isäntänimi julkaiseminen DNS-ympäristössä
  - Raportoinnin SSH host avaimen tunnisteen ympäristö
  - Resurssien Levynhallinta
  - Muotoilut ja resurssin levyn käyttöönottaminen
  - Vaihda tilan määrittäminen

* **Verkko**
  - Hallitsee tiet ympäristö DHCP-palvelimet yhteensopivuuden parantamiseen
  - Varmistaa käyttöliittymän verkkonimi vakavuus

* **Ydin**
  - Määrittää virtual NUMA (ydin käytöstä < 2.6.37)
  - Siinä käytetään Hyper-V entropia /dev/random varten
  - Määrittää SCSI aikakatkaisu ensisijaisen laitteen (joka voi olla remote)

* **Vianmääritys**
  - Konsolin uudelleenohjaus sarja portti

* **SCVMM ominaisuuksissa**
    - Tunnistaa ja bootstraps Linux VMM-agentti, kun System Center virtuaalikoneen hallinta 2012 R2-ympäristössä

* **AM tunniste**
    - Lisää osan Microsoft ja kumppanit käyttöön ohjelmiston AM Linux (IaaS) ja määritykset automaatio tekijä
    - [Https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions) AM tunniste viittaus toteuttaminen


## <a name="communication"></a>Viestintä
Tietovuo-ympäristö agentti ilmenee kaksi kanavien kautta:

* Käynnistyksen aikainen liittyvien DVD-levyn käyttöönotoissa IaaS. DVD-LEVYÄ sisältää OVF-yhteensopiva kokoonpanotiedosto, joka sisältää kaikki valmistelu tiedot kuin todellinen SSH keypairs.
* TCP-päätepistettä paljastaa REST-Ohjelmointirajapinnalla käytetyt käyttöönotto- ja topologian määrittäminen.


## <a name="requirements"></a>Vaatimukset
Seuraavien järjestelmien on testattu ja käsitellä Azure Linux-agentti tunnetusti:

> [AZURE.NOTE] Tämän luettelon voivat poiketa virallinen luettelo tuetuista järjestelmien Microsoft Azure-ympäristön seuraavassa kuvatulla tavalla: [http://support.microsoft.com/kb/2805216](http://support.microsoft.com/kb/2805216)

* CoreOS
* CentOS 6.3 +
* Punainen on Enterprise Linux 6,7 +
* Debian 7.0 +
* Ubuntu 12.04 +
* openSUSE 12.3 +
* SLES 11 SP3 +
* Oracle Linux 6.4 +

Tuetut muista järjestelmistä:

* FreeBSD 10 + (Azure Linux-agentti v2.0.10 +)

Linux-agentti määräytyy järjestelmän Jotkin paketit toimi:

* Python 2.6 +
* OpenSSL 1.0 +
* OpenSSH 5.3 +
* Tiedostojärjestelmän apuohjelmat: sfdisk, fdisk, mkfs, voitu
* Salasana-työkalut: chpasswd, sudo
* Tekstin käsittely-työkalut: sed, grep
* Verkko-työkalut: ip-reitityksen
* Ydin tuen käyttöönottaminen UDF filesystems.

## <a name="installation"></a>Asennus
Oman jakauman paketin säilöstä JÄLLEENMYYNTIHINNAN tai DEB-paketin asennuksen on ensisijainen tapa asentaminen ja päivittäminen Azure Linux-agentti. Kaikki [vahvistettava jakauman tarjoajat](virtual-machines-linux-endorsed-distros.md) integroida Azure Linux agent-paketin niiden kuvia ja säilöjen tietoihin.

Lisätietoja [Azure Linux agentti repo Github Valitse](https://github.com/Azure/WALinuxAgent) asennuksen Lisäasetukset-asetuksia, kuten lähteestä tai mukautetun kohtiin tai etuliitteiden asennuksen asiakirjoissa.


## <a name="command-line-options"></a>Komentorivivalitsimet

### <a name="flags"></a>Liput

- yksityiskohtainen: saa määritetty komento
- Pakota: Ohita vuorovaikutteinen vahvistus jotkut komennot

### <a name="commands"></a>Komennot

- Ohje: on lueteltu tuetut komennot ja merkintöjä.

- deprovision: yrittää puhdistaa järjestelmä ja ansiosta sopii toiminnon uudelleen. Tämä toiminto poistaa seuraavasti:
 * Kaikki SSH host avaimet (Jos Provisioning.RegenerateSshHostKeyPair on y, jos määritystiedostossa)
 * /Etc/resolv.conf NameServer määrittäminen
 * Pääkansio salasanan /etc/shadow (Jos Provisioning.DeleteRootPassword on y, jos määritystiedostossa)
 * Välimuistiin tallennetut DHCP-asiakasohjelman varauksia
 * Palauttaa localhost.localdomain isäntänimi


> [AZURE.WARNING] Valmistelun poistaminen takaa, että kuva on valittuna kaikki luottamuksellisia tietoja ja soveltuu uudelleenjakamista.


- deprovision + käyttäjän: suorittaa kaikki kohdassa - deprovision (edellä) ja myös poistaa viimeksi valmistellun käyttäjätili (saatu /var/lib/waagent) ja siihen tietoja. Tämä parametri on valmisteltaessa varaustiedoista kuva, joka oli aiemmin valmistelu Azure-, jotta se voidaan siepata ja käyttää uudelleen.

- versio: Näyttää version waagent

- serialconsole: määrittää GRUB Merkitse ttyS0 (ensimmäinen sarja portti) käynnistys-konsolin nimellä. Näin varmistat, että ydin bootup lokit serial porttiin lähetetyt ja saatavilla virheenkorjaus.

- Daemon: suorittaa waagent daemon hallittavan platform käsittelemisen. Argumentti on määritetty waagent waagent alusta komentosarjan.

- aloittaminen: Suorita waagent taustalla


## <a name="configuration"></a>Määritys

Määritystiedoston (/ etc/waagent.conf) ohjaa waagent toiminnot. Määritysten mallitiedosto on alla:

    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None

Asetuksia on kuvattu tarkemmin jäljempänä. Kokoonpanoasetusten määrittäminen on kolme erilaista; Totuusarvo, merkkijono tai kokonaisluku. Totuusarvo kokoonpanoasetusten määrittäminen voidaan määrittää "k" tai "e". Erityistä hakusanalla "Ei" voidaan käyttää joidenkin merkkijonon määritysten tapahtumia esitetyllä tavalla alla.

**Provisioning.Enabled:**  
Tyyppi: totuusarvo  
Oletus: y

Näin käyttäjän käyttöön ottaminen tai käytöstä agentti valmistelu toimintoja. Kelvolliset arvot ovat "k" tai "e". Jos valmistelu on poistettu käytöstä, SSH isännän ja käyttäjän näppäimet kuvan säilytetään ja valmistelu API Azure-parametrissa kokoonpanoa ohitetaan.

> [AZURE.NOTE] `Provisioning.Enabled` Parametrin oletusarvo on "o" Ubuntu Cloud kuvia, jotka käyttävät valmisteluun cloud alusta.

  
**Provisioning.DeleteRootPassword:**  
Tyyppi: totuusarvo  
Oletus: n

Jos set-/ jne/varjostus-tiedoston pääkansio salasana poistetaan valmistelun yhteydessä.


**Provisioning.RegenerateSshHostKeyPair:**  
Tyyppi: totuusarvo  
Oletus: y

Jos määrittäminen, kaikki SSH host avaimen paria (RCDSA ja dsa rsa) poistetaan kohteesta /etc/ssh/valmistelun aikana. Ja yksi tärkeimmistä ajan tasalla pari luodaan.

Salauksen tyyppi on ajan tasalla avaimen pari on määritettävä Provisioning.SshHostKeyPairType tapahtuman mukaan. Huomaa, että jotkin jaot uudelleen luoda SSH avaimen pareina puuttuu salauksen summatyyppejä SSH daemon uudelleenkäynnistyksen yhteydessä (esimerkiksi yhteydessä uudelleen).


**Provisioning.SshHostKeyPairType:**  
Tyyppi: merkkijono  
Oletus: rsa

Tämä voidaan määrittää salauksen algoritmin tyyppi, joka tue virtuaalikoneen SSH-daemon. Yleensä tuetut arvot ovat "rsa", "dsa" ja "RCDSA". Huomaa, että "putty.exe" Windows ei tue "RCDSA". Näin on, jos haluat muodostaa Linux-käyttöönoton Windows putty.exe avulla, käytä "rsa" tai "dsa".


**Provisioning.MonitorHostName:**  
Tyyppi: totuusarvo  
Oletus: y

Jos määritetty, waagent valvoo Linux virtuaalikoneen hostname muutosten ("hostname"-komento palauttamat) ja päivittää automaattisesti verkon määritysten kuvassa muuttuvat. Jotta voit siirtää nimen muuttaminen DNS-palvelimet, verkko käynnistetään virtuaalikoneen. Tämä aiheuttaa lyhyesti menetyksiä Internet-yhteys.


**Provisioning.DecodeCustomData**  
Tyyppi: totuusarvo  
Oletus: n

Jos määritetty, waagent toistaa CustomData-Base64.


**Provisioning.ExecuteCustomData**  
Tyyppi: totuusarvo  
Oletus: n

Jos määritetty, waagent suorittaa CustomData valmistelun jälkeen.


**Provisioning.PasswordCryptId**  
Merkkijonomuotoisen:  
Oletusarvo: 6

Algoritmin käyttämä crypt salasanan hash luotaessa.  
 1 - MD5  
 2 a - Blowfish  
 5 - SHA-256  
 6 - SHA 512  


**Provisioning.PasswordCryptSaltLength**  
Merkkijonomuotoisen:  
Oletusarvo: 10

Satunnainen suolaa käytetään luotaessa salasanan hash pituus.


**ResourceDisk.Format:**  
Tyyppi: totuusarvo  
Oletus: y

Jos määritetty, ympäristö toimittaman resurssin levyn muotoiltu ja käyttöön waagent Jos "ResourceDisk.Filesystem" käyttäjän pyytämä tiedostojärjestelmää on jokin muu kuin "ntfs". Yksittäinen tyyppi Linux (83)-osio on saatavana levyn. Huomaa, että tämä osio muotoillaan ei, jos se on asennettu onnistuneesti.


**ResourceDisk.Filesystem:**  
Tyyppi: merkkijono  
Oletus: ext4

Tämä määrittää resurssin levyn tiedostojärjestelmän tyyppi. Tuetut arvot vaihtelevat mukaan Linux-jakauman. Jos merkkijono on X, valitse mkfs. X esitetään Linux-kuva. SLES 11 kuvia yleensä käytettävä "ext3". FreeBSD kuvat kannattaa käyttää "ufs2".


**ResourceDisk.MountPoint:**  
Tyyppi: merkkijono  
Oletusarvo: / mnt/resurssi 

Määrittää polku, johon resurssi-levy on otettu käyttöön. Huomaa, että resurssin levy on *väliaikainen* DVD-levyllä, voidaan tyhjentää kun AM käyttömahdollisuus puretaan.


**ResourceDisk.MountOptions**  
Tyyppi: merkkijono  
Oletus: ei mitään

Määrittää levyn käyttöönoton asetukset välittämisen asennustapa -o-komentoa. Tämä on pilkuilla erotettu luettelo arvoja, ex. "nodev, nosuid". Katso lisätietoja mount(8).


**ResourceDisk.EnableSwap:**  
Tyyppi: totuusarvo  
Oletus: n

Jos määritetty, vaihda tiedoston (/ sivutustiedosto) resurssin levyn luodaan ja lisätään järjestelmän Vaihda tilaa.


**ResourceDisk.SwapSizeMB:**  
Tyyppi: kokonaisluku  
Oletus: 0

Megatavuina swap-tiedoston kokoa.


**Logs.Verbose:**  
Tyyppi: totuusarvo  
Oletus: n

Jos Määritä-loki saa on tehosti. Waagent kirjautuu /var/log/waagent.log ja hyödyntää Kierrä lokit järjestelmän logrotate-toimintoja.


**OS. EnableRDMA**  
Tyyppi: totuusarvo  
Oletus: n

Jos määritetty, agentti yrittää asentaminen ja lataaminen RDMA ydin ohjaimen, joka vastaa pohjana laitteistoon laitteisto-version.


**OS. RootDeviceScsiTimeout:**  
Tyyppi: kokonaisluku  
Oletus: 300

Tämä määrittää SCSI aikakatkaisu sekunteina OS levyn ja tietojen asemista. Jos et määritä oletukset käytetään järjestelmän.


**OS. OpensslPath:**  
Tyyppi: merkkijono  
Oletus: ei mitään

Tämän avulla voidaan määrittää vaihtoehtoisen polun binaarinen salauksen toimintojen käyttäminen openssl varten.


**HttpProxy.Host, HttpProxy.Port**  
Tyyppi: merkkijono  
Oletus: ei mitään

Jos määritetty, agentti käyttää tätä välityspalvelimen Internetiä. 


## <a name="ubuntu-cloud-images"></a>Ubuntu Cloud kuvat

Huomautus Ubuntu Cloud kuvia käyttämiseen suorittaa useita määritystehtäviä, jotka muuten hallitsee Azure Linux-agentti [cloud alusta](https://launchpad.net/ubuntu/+source/cloud-init) .  Huomaa seuraavat erot:


- **Provisioning.Enabled** oletusarvoisesti "o" Ubuntu Cloud kuvia, jotka käyttävät cloud alusta valmistelu tehtävien suorittamiseen.

- Seuraavat määritykset parametrit ei vaikuta Ubuntu Cloud kuvia, jotka cloud alusta avulla voit hallita resurssi-levyn ja vaihda tila:

 - **ResourceDisk.Format**
 - **ResourceDisk.Filesystem**
 - **ResourceDisk.MountPoint**
 - **ResourceDisk.EnableSwap**
 - **ResourceDisk.SwapSizeMB**

- Lue resurssin levyn liityntäkohtaan ja vaihda Ubuntu Cloud kuvia tilaa aikana valmistelu on seuraavissa resursseissa:

 - [Ubuntu Wiki: Määritä Vaihda osiot](http://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
 - [Azure virtuaalikoneen injisoimalla mukautetut tiedot](virtual-machines-windows-classic-inject-custom-data.md)

