<properties 
   pageTitle="Epäonnistua takaisin VMware näennäiskoneiden ja fyysinen palvelinten azuren VMware (vanha) | Microsoft Azure" 
   description="Tässä artikkelissa kerrotaan, miten voi epäonnistua takaisin VMware virtual laitteeseen, joka on ollut replikoida Azure Azure palauttaminen kanssa." 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="storage-backup-recovery" 
   ms.date="10/05/2016"
   ms.author="ruturajd@microsoft.com"/>

# <a name="fail-back-vmware-virtual-machines-and-physical-servers-from-azure-to-vmware-with-azure-site-recovery-legacy"></a>Epäonnistua takaisin VMware näennäiskoneiden ja fyysinen palvelinten azuren VMware kanssa Azure sivuston palauttaminen (vanha)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-failback-azure-to-vmware.md)
- [Azure perinteinen Portal](site-recovery-failback-azure-to-vmware-classic.md)
- [Azure perinteinen Portal (vanha)](site-recovery-failback-azure-to-vmware-classic-legacy.md)


Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten takaisin VMware näennäiskoneiden ja Windows-tai Linux fyysiset palvelimet azuren paikallisen sivustoon epäonnistuu, sen jälkeen, kun olet replikoida paikallisen-sivustosta, Azure.

>[AZURE.NOTE] Tässä artikkelissa kuvataan vanha tilanne. Vain käytettävä ohjeita tämän artikkelin Jos voit kopioida Azure [Vanha seuraavia](site-recovery-vmware-to-azure-classic-legacy.md)ohjeita. Jos käyttämällä [parannettu käyttöönoton](site-recovery-vmware-to-azure-classic-legacy.md) replikoinnin määrittäminen noudata [tämän artikkelin](site-recovery-failback-azure-to-vmware-classic.md) epäonnistuu takaisin. 


## <a name="architecture"></a>Arkkitehtuuri

Tässä kaaviossa edustaa automaattisesti ja tuntisesta skenaariota. Sininen rivit ovat automaattisesti käytettyä yhteydet. Punainen rivit ovat tuntisesta käytettyä yhteydet. Rivit, joissa nuolet Siirry Internetin välityksellä.

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## <a name="before-you-start"></a>Ennen aloittamista 

- Olisi epäonnistui VMware VMs tai fyysiset palvelimet ja niiden on oltava käynnissä Azure-tietokannassa.
- Huomaa, että olet voi vain epäonnistua takaisin VMware näennäiskoneiden ja Windows-tai Linux fyysinen palvelinten azuren näennäiskoneiden VMware paikallisen ensisijainen sivuston.  Jos olet olet kaatuvat takaisin fyysinen machine-Azure automaattisesti muuntaa sen Azure-AM ja tuntisesta VMware muuntaa sen VMware AM.

Näin tuntisesta määrittämisestä:

1. **Määritä tuntisesta osat**: sinun on vContinuum-palvelimen määrittäminen paikallisen, ja osoita määrityspalvelimen AM Azure-tietokannassa. Myös Määritä prosessin server Azure-AM kuin Lähetä tietoa takaisin paikalliseen perusmuodon kohde-palvelimeen. Voit rekisteröidä prosessin palvelimen määrityspalvelimen, käsitellään vikasietotilaa. Asennat paikallisen perusmuodon kohdepalvelimen. Jos tarvitset Windows perusmuodon kohde-palvelimeen, se on määritetty automaattisesti vContinuum asennuksen yhteydessä. Jos tarvitset Linux tarvitset voit määrittää sen manuaalisesti muussa palvelimessa.
2. **Ota suojaus ja tuntisesta**: Kun olet määrittänyt osat,-vContinuum sinun on ottaa käyttöön suojauksen epäonnistui Azure VMs päälle. VMs valmiuden tarkistaa ja suorita automaattisesti azuren paikalliseen sivustoon. Tuntisesta päätyttyä reprotect paikallisen koneet, niin, että ne alkavat replikoiminen Azure.



## <a name="step-1-install-vcontinuum-on-premises"></a>Vaihe 1: Asenna vContinuum paikallisen

Sinun on asennettava vContinuum palvelimen paikallinen ja osoita configuration-palvelin.

1.  [Lataa vContinuum](http://go.microsoft.com/fwlink/?linkid=526305). 
2.  Lataa [vContinuum Päivitä](http://go.microsoft.com/fwlink/?LinkID=533813) -versio.
3. Asenna uusin versio vContinuum. Valitse **aloitussivulla** **Seuraava**.
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  Ohjatun toiminnon ensimmäisellä sivulla Määritä CX palvelimen IP-osoite ja CX palvelimen portti. Valitse **Käytä HTTPS**.

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  Määritysten palvelimen IP-osoite-välilehden Etsi **Dashboard** määrityspalvelimen AM Azure-tietokannassa.
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Etsi Azure määrityspalvelimen HTTPS julkisen porttiin määrityspalvelimen AM **päätepisteet** -välilehti.

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. Määritä salasana muistiin kirjoittamasi alaspäin asentaessasi määrityspalvelimen **CS salasana tiedot** -sivu. Jos et muista sitä kuitattava sisään **C:\\Program Files (x86)\\InMage järjestelmien\\yksityinen\\connection.passphrase** määritysten palvelimessa AM.

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  Määritä **Valitse kohdesijainti** sivun kohtaa, johon haluat asentaa vContinuum palvelimeen, ja valitse **Asenna**.

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. Kun asennus on valmis, voit käynnistää vContinuum.
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## <a name="step-2-install-a-process-server-in-azure"></a>Vaihe 2: Asenna prosessin server Azure-tietokannassa 

Sinun täytyy asentaa prosessin server Azure, niin, että VMs Azure-tietokannassa voit lähettää tiedot takaisin paikalliseen perustyylin kohde palvelimeen. 

1.  Valitse Azure **Configuration Servers** -sivulla Lisää uusi prosessi-palvelin.

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  Määritä prosessin palvelimen nimi- ja nimi ja salasana virtuaalikoneen järjestelmänvalvojana. Valitse määritys-palvelin, johon haluat rekisteröidä prosessin palvelimen. Tämä on oltava samassa palvelimessa käytät suojaaminen ja epäonnistua oman näennäiskoneiden päälle.
3.  Määritä Azure verkkoon, jossa prosessin palvelimen käyttöön. Sen on oltava samassa verkossa kuin configuration-palvelin. Määritä yksilöllinen IP-osoite valittuna aliverkon ja aloita käyttöönotto.

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  Työn käynnistäminen ottamaan prosessi-palvelimeen.

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  Kun kirjaudut määrittämäsi tunnistetiedoilla Serveriin Azure prosessin palvelin on otettu käyttöön. Voit rekisteröidä prosessin palvelimen samalla tavalla paikalliseen prosessin Serveriin rekisteröity. 

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] Rekisteröity aikana tuntisesta palvelimet ei näy sivuston palauttaminen AM ominaisuudet-kohdassa. Ne näkyvät vain **palvelimet** -välilehden määrityspalvelimen, johon ne on rekisteröity. Voi kestää noin 10 – 15 minuuttia ennen kuin he prosessin palvelimen näkyy-välilehdessä.


## <a name="step-3-install-a-master-target-server-on-premises"></a>Vaihe 3: Asenna paikallisesta perusmuodon kohde-palvelimesta on

Tietolähteen näennäiskoneiden käyttöjärjestelmästä riippuen sinun on asennettava Linux tai Windows perusmuodon kohde palvelimeen paikalliseen.

### <a name="deploy-a-windows-master-target-server"></a>Windows-perusmuodon kohde Serverin käyttöönotto

Windowsin perusmuodon kohde on jo yhteen vContinuum määritykset. Kun asennat vContinuum, perusmuodon palvelimen otetaan käyttöön samaan tietokoneeseen myös ja rekisteröity määritys-palvelimeen.

1.  Aloita käyttöönoton, luoda tyhjän tietokoneen paikalliseen ESX isännän, jolle haluat palauttaa Azure VMs.

2.  Varmista, että on yhdistetty AM vähintään kaksi levyjä – käytetään käyttöjärjestelmän ja toinen käytetään säilytys-asema.

3.  Asenna käyttöjärjestelmä.

4.  Asenna vContinuum palvelimeen. Tämä suorittaa myös perusmuodon kohdepalvelimen asennuksen.

### <a name="deploy-a-linux-master-target-server"></a>Linux perusmuodon kohde-Serverin käyttöönotto

1.  Aloita käyttöönoton, luoda tyhjän tietokoneen paikalliseen ESX isännän, jolle haluat palauttaa Azure VMs.

2.  Varmista, että on yhdistetty AM vähintään kaksi levyjä – käytetään käyttöjärjestelmän ja toinen käytetään säilytys-asema.

3.  Asenna Linux-käyttöjärjestelmä. Linux perustyylin kohteen käyttöjärjestelmän Älä käytä LVM pääkansion tai säilytys tallennustilan välilyöntejä. Linux-perusmuodon kohde-palvelin on määritetty välttämiseksi LVM osioiden/levyjen etsiminen oletusarvoisesti.
4.  Osiot, voit luoda:

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  Suorita viestin alla ennen kuin aloitat asennuksen perusmuodon kohde asennusohjeita.


#### <a name="post-os-installation-steps"></a>Kirjaa OS asennusohjeita

Saat SCSI ID's kunkin SCSI kiintolevy Linux-virtual machine-käyttöön parametrin "levy. EnableUUID = TRUE "seuraavasti:

1. Sulje virtuaalikoneen.
2. AM tapahtuma vasemmassa ruudussa hiiren kakkospainikkeella > **Muokkaa asetuksia**.
3. Valitse **asetukset** -välilehti. Valitse **Lisäasetukset\>Yleiset kohteen** > **Määrityksiä**. **Parametrit** -asetus on käytettävissä vain, kun tietokoneen suljetaan.

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. Tarkistaa, onko rivi, jonka **levy. EnableUUID** on olemassa. Jos se ja on määritetty **Epätosi** Aseta sen arvoksi **True** (ei kirjainkoko on merkitsevä). Jos on olemassa ja on määritetty tosi, valitse **Peruuta** ja testaa sisällä vieras-käyttöjärjestelmän SCSI-komento, sen jälkeen, kun se käynnistetään ylöspäin. Jos ei ole **Lisää**rivi.
5. Lisää disk. EnableUUID **nimi** -sarakkeessa. Määritä sen arvoksi TRUE. Älä lisää yllä arvot sekä lainausmerkkejä.

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### <a name="download-and-install-the-additional-packages"></a>Lataa ja asenna muita paketit

Huomautus: Varmista, että järjestelmä on internet-yhteys ennen lataaminen ja asentaminen muita paketit.

\#YUM -y xfsprogs perl lsscsi rsync wget kexec-tarkistustyökalujen asentaminen

Tämä komento lataa 15 pakkauksissa CentOS 6.6 säilöstä ja asentaa ne:

viivakoodi-1.06.95-1.el6.x86\_64. jälleenmyyntihinnan

busybox 1.15.1-20.el6.x86\_64. jälleenmyyntihinnan

elfutils-kirjastoa-0.158-3.2.el6.x86\_64. jälleenmyyntihinnan

kexec-Työkalut-2.0.0-280.el6.x86\_64. jälleenmyyntihinnan

lsscsi 0.23-2.el6.x86\_64. jälleenmyyntihinnan

lzo 2.03-3.1.el6\_5.1.x86\_64. jälleenmyyntihinnan

Perl-5.10.1-136.el6\_6.1.x86\_64. jälleenmyyntihinnan

Perl-moduuli-vaihdettava-3.90-136.el6\_6.1.x86\_64. jälleenmyyntihinnan

Perl Pod-kokonaiseen-1.04-136.el6\_6.1.x86\_64. jälleenmyyntihinnan
    
Perl Pod-yksinkertainen-3.13-136.el6\_6.1.x86\_64. jälleenmyyntihinnan

Perl-kirjastoa-5.10.1-136.el6\_6.1.x86\_64. jälleenmyyntihinnan

Perl-versio-0,77-136.el6\_6.1.x86\_64. jälleenmyyntihinnan

rsync 3.0.6-12.el6.x86\_64. jälleenmyyntihinnan

snappy 1.el6.x86 kuin 1.1.0\_64. jälleenmyyntihinnan

wget 1.12-5.el6\_6.1.x86\_64. jälleenmyyntihinnan

Huomautus: Jos lähdetietokoneen käyttää Reiser tai XFS tiedostojärjestelmän pääkansion tai käynnistys-laitteen, valitse seuraavat paketit olisi voidaan ladata ja asentaa Linux perusmuodon samannimiset ennen suojaus.

\#CD-levylle /usr/local

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\#wget <http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\#jälleenmyyntihinnan - ivh kmod-reiserfs-0,0-1.el6.elrepo.x86\_64. jälleenmyyntihinnan reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64. jälleenmyyntihinnan

\#wget <http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\#jälleenmyyntihinnan - ivh xfsprogs-3.1.1-16.el6.x86\_64. jälleenmyyntihinnan

#### <a name="apply-custom-configuration-changes"></a>Mukautettu määritys muutokset

Ennen käyttöä muutokset Varmista, että olet suorittanut edellisessä osassa ja valitse seuraavasti:


1. Kopioi RHEL 6 64 yhdistetyn agentti binaarinen juuri luomasi OS.

2. Suorita tämä komento untar binaaritiedosto: **Kohdekeh - zxvf \<tiedostonimi\> **

3. Suorita tämä komento ja anna käyttöoikeudet: \# **chmod 755./ApplyCustomChanges.sh**

4. Suorita komentosarja: ** \# ./ApplyCustomChanges.sh**. Palvelimessa suoritettavat komentosarja vain kerran. Käynnistä-palvelin, kun komentosarja on suoritettu.



### <a name="install-the-linux-server"></a>Asenna Linux-palvelin


1. [Lataa](http://go.microsoft.com/fwlink/?LinkID=529757) asennustiedosto.
2. Kopioi tiedosto sftp asiakas-apuohjelmalla valittua Linux perustyyli kohde virtuaalikoneen. Vuorotellen voit kirjautua Linux perustyylin kohde virtuaalikoneen ja asennuspaketin lataaminen napsauttamalla Aiheeseen liittyvää linkkiä wget avulla
3. Kirjaudu Linux perusmuodon kohde palvelimen virtuaalikoneen käyttämällä ssh asiakkaan valittua
4. Jos yhteys on muodostettu, voit ottaa käyttöön Linux perustyylin kohde-palvelimen VPN-yhteyden kautta Azure verkkoon käyttää sisäinen IP-osoite palvelimen, josta löydät virtuaalikoneen **koontinäyttö** -välilehti ja portin 22 Linux perustyylin kohde palvelimeen käyttämällä suojattu runko yhdistäminen.
5. Jos olet muodostamassa Linux perusmuodon kohde palvelimeen julkinen internet-yhteyden kautta Linux perusmuodon kohdepalvelimen yleisen virtual IP-osoite (mistä näennäiskoneiden **koontinäyttö** -välilehti) ja julkisen päätepisteen luodaan ssh kirjautua Linux-servder.
6. Pura tiedostot gzipped Linux perustyylin kohde palvelimen installer tar arkisto suorittamalla: *"Kohdekeh – xvzf Microsoft ASR\_käyttäjien järjestelmänvalvoja\_8.2.0.0\_RHEL6 64\"* hakemistosta, joka sisältää asennustiedostoa.

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. Jos olet purkanut tiedostot eri kansioon muuttaminen hakemistoon installer joka tar sisällön arkistoida on purettu. Tämän kansion polun, suorita "sudo. / install.sh".

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. Kun sinua pyydetään valitsemaan ensisijainen rooli kohdassa **2 (perustyyli kohde)**. Jättää muut vuorovaikutteinen asennuksen asetukset oletusarvoiksi.
9. Odota, voit jatkaa asennusta ja Host Config käyttöliittymän näkyvän. Käytettävä Linux perusmuodon sarget palvelimen isännän kokoonpano-apuohjelma on komentorivi-apuohjelma. Älä muuta kokoa ssh asiakkaan apuohjelma-ikkuna. Valitse **Yleiset** -vaihtoehto ja paina ENTER-näppäintä nuolinäppäimillä.

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)


10. Kirjoita kentän **IP** (löydät sen **raporttinäkymät-ikkunan** välilehdessä määrityspalvelimen AM) määrityspalvelimen sisäinen IP-osoite ja paina ENTER-näppäintä. Kirjoita **portin** **22** ja paina ENTER-näppäintä. 
11.  Jätä **Käytä HTTPS** kuin **Kyllä**ja painamalla ENTER-näppäintä.
12.  Kirjoita salasana, joka on luotu määritys-palvelimeen. Jos käytät painovärit, muste asiakkaan Windows-tietokoneesta ssh Linux virtuaalikoneen voit VAIHTO + Insert Liitä Leikepöydän sisältö. Kopioi salasana paikallisen Leikepöydälle käyttämällä Ctrl + C ja liitä se käyttämällä VAIHTO + Insert. Paina ENTER-näppäintä.
13.  Käytä oikeaa nuolinäppäintä siirtymällä Sulje ja paina ENTER-näppäintä. Odota, viimeistele asennus.

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)

Jos jostain syystä ei voi rekisteröidä Linux perusmuodon kohdepalvelimen määritysten palvelimeen voit tehdä niin uudelleen suorittamalla isännän kokoonpano apuohjelman /usr/local/ASR/Vx/bin/hostconfigcli (ensin tarvitset käyttöoikeuksien määrittäminen tähän kansioon suorittamalla chmod erittäin käyttäjänä).


#### <a name="validate-master-target-registration"></a>Vahvista perusmuodon kohteen rekisteröiminen

Voit tarkistaa, että perusmuodon kohdepalvelin on rekisteröity onnistuneesti Azure palauttaminen säilöön määrityspalvelimen > **Määrityspalvelimen** > **Palvelimen tiedot**.

>[AZURE.NOTE] Kun perusmuodon palvelimesta, jos saat määritysvirheet virtuaalikoneen on ehkä poistettu azuren tai päätepisteet ei ole määritetty oikein, tämä johtuu siitä perusmuodon kohde-määritysten tunnistaman Azure dndpoints kun perusmuodon kohde on otettu käyttöön Azure, vaikka ei tosi paikallisen perusmuodon kohde palvelimen paikallisen. Tämä ei vaikuta tuntisesta ja voit ohittaa nämä virheet. 



## <a name="step-4-protect-the-virtual-machines-to-the-on-premises-site"></a>Vaihe 4: Suojaa näennäiskoneiden paikallisen-sivustoon

Haluat suojata VMs paikallisen sivuston, ennen kuin takaisin epäonnistuu.

### <a name="before-you-begin"></a>Ennen aloittamista

Azure on epäonnistui AM päälle, Lisää ylimääräinen temp asema, sivu-tiedoston. Tämä on ylimääräisiä-asema, joka ei yleensä tarvitse mukaan oman epäonnistui AM, koska se voi olla jo varattu sivun tiedoston aseman päälle. Ennen kuin aloitat näennäiskoneiden käänteisen suojaus, sinun täytyy varmistaa, että tämä asema otetaan offline-tilassa niin, että se ei suojata. Tehdä seuraavasti:

1.  Avaa Tietokoneenhallinta ja valitse tallennusvälineiden hallinta niin, että luettelo levyjen verkossa ja kiinnitetty koneeseen.
2.  Valitse kiinnitetty koneeseen tilapäinen levyn ja valitse Tuo offline-tilassa. 

### <a name="protect-the-vms"></a>Suojaa VMs

1. Azure-portaalissa Tarkista tiloista virtuaalikoneen varmistaa, että ne on epäonnistunut päälle.

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. Käynnistä vContinuum käyttämääsi laitteeseen.

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. **Uusi suojaus** ja valitse toiminto-Järjestelmälaji 

4.  Avaa Valitse **OS tyyppi**uudessa ikkunassa > haluat epäonnistua takaisin VMs**Hae tiedot** . **Ensisijaisen palvelimen tiedot**-tunnistaa ja valitse näennäiskoneiden, jonka haluat suojata. VMs näkyvät vCenter isäntänimen, ne olisivat ennen automaattisesti.
5.  Kun valitset virtual-tietokoneen suojaaminen (ja se on jo voinut päälle Azure) ponnahdusikkunan on kaksi merkintää virtuaalikoneen. Tämä johtuu siitä määrityspalvelimen havaitsee rekisteröity siihen näennäiskoneiden kaksi esiintymää. Sinun täytyy poistaa merkinnän paikallisen AM niin, että voit suojata oikea AM. Tunnistavan oikea Azure AM merkintä tähän Azure AM Kirjaudu ja siirry kohtaan C:\Program Files (x86) \Microsoft Azure Site Recovery\Application Data\etc. Tiedosto-drscout.conf tunnistetaan isännän ID-tunnuksellasi. Pidä tapahtuma, jonka Isäntätunnus löytyy AM vContinuum-valintaikkunassa. Poista kaikki merkinnät. Valitse oikea AM voit viitata IP-osoite. IP address alueen paikallisen on paikallisen AM.

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. Lopeta virtuaalikoneen vCenter-palvelimeen. Voit myös poistaa VMs paikallisen.
7. Määritä paikallisen MT palvelin, johon haluat suojata VMs. Voit tehdä tämän muodostaa vCenter-palvelin, johon haluat epäonnistua takaisin.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. Valitse perustyylin kohdepalvelin, johon haluat palauttaa AM host perusteella.

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. Anna kullekin näennäiskoneiden replikoinnin-vaihtoehto. Voit tehdä tämän haluat valitseminen palautus-puolen datastore, johon VMs voi palauttaa. Taulukossa on kuvattu haluat säätää kunkin AM eri vaihtoehtoja.

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **Vaihtoehto** | **Suositellut arvon asetus**
    ---|---
    Käsittele IP-osoite | Valitse prosessi-palvelin käyttöön Azure-tietokannassa
    Säilytys koko megatavuina| 
    Säilytys-arvo | 1
    Päivää/tuntia | Päivää
    Yhdenmukaisuuden väli | 1
    Valitse kohde datastore | Datastore käytettävissä palautus-sivustossa. Tietovaraston on tarpeeksi tilaa ja, jonka haluat palauttaa virtuaalikoneen ESX host käytettävissäsi.


10. Määritä ominaisuudet, jotka virtuaalikoneen hankittava jälkeen paikallisen sivuston automaattisesti. Seuraavaan taulukkoon on koottu ominaisuudet.

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

    **Ominaisuus** | **Tiedot**
    ---|---
    Verkon määrittäminen| Verkkosovittimen kunkin havaita napsauttamalla sitä ja valitse **Muuta** virtuaalikoneen tuntisesta IP-osoitteen määrittäminen. 
    Laitteiston määritykset| Määritä suorittimen ja muistin AM. Kaikki, jonka haluat suojata VMs voi suojata asetukset. Tunnistavan suorittimen ja muistin oikeat arvot viittaavat IAAS VMs roolin kokoa ja Sydämiä ja määritetty muistin määrän.
    Näyttönimi | Tuntisesta paikalliseen jälkeen voit nimetä uudelleen näennäiskoneiden sellaisina kuin ne näkyvät vCenter varastossa. Oletusnimi on virtuaalikoneen tietokoneen isäntänimi. Laji AM nimen, voit viitata suojaus-ryhmä AM-luetteloon.
    NAT-määritykset | Tiedot alla kuvatun

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### <a name="configure-nat-settings"></a>NAT-asetusten määrittäminen

1. Näennäiskoneiden suojaus käyttöön kaksi viestintä kanavien on vahvistettava. Ensimmäinen kanava on virtuaalikoneen ja prosessin palvelimen välillä. Kanavan kerää tiedot AM ja lähettää sen prosessi-palvelin, joka lähettää tiedot perusmuodon palvelimesta. Jos prosessi-palvelin ja suojattava virtuaalikoneen ovat samassa verkossa Azure virtual ei tarvitse NAT-asetusten avulla. Määritä muussa NAT-asetukset. Tarkastele Azure prosessin palvelimen julkiseen IP-osoite. 

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. Toinen kanava on prosessi-palvelimen ja perusmuodon kohde. Mahdollisuus käyttää NAT vai ei määräytyy mukaan VPN-yhteyden avulla vai viestinnästä Internetin välityksellä. Älä valitse NAt, jos käytät VPN-yhteyttä, mutta ainoastaan, jos käytössäsi on internet-yhteys.

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. Jos poistit eivät ole paikallisen näennäiskoneiden määritetyllä tavalla ja takaisin edelleen epäonnistuvat datastore sisältää vanha VMDK täytyy varmistaa, että tuntisesta AM luodaan uuteen paikkaan. Voit tehdä tämän valitsemalla **Lisäasetukset** -asetukset ja määritä **Kansion nimen asetukset**palautetaan vaihtoehtoisen kansion. Jätä muut asetukset oletusasetuksiin kanssa. Käytä jokaisessa palvelimessa kansion nimi-asetuksia.

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. Suorita valmiuden tarkistuksen varmistaa, että näennäiskoneiden on valmis suojattava takaisin paikalliseen. 

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)


5. Odota sen valmistumista. Jos kyseessä on onnistuneesti kaikki VMs voit määrittää suojauksen suunnitelman nimi. Valitse **Suojaa** alkavan.

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)


6. Voit valvoa edistymistä vContinuum.

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. **Aktivoiminen suojauksen suunnittelu** loppuu vaiheen jälkeen voit valvoa AM suojaus, sivuston palautus-portaalissa.

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. Näet tarkka tila valitsemalla AM ja edistymisen seuranta.

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## <a name="prepare-the-failback-plan"></a>Valmistele tuntisesta suunnitelma

Voit valmistella tuntisesta suunnitelman käyttämällä vContinuum, niin, että sovellus on epäonnistui takaisin paikalliseen sivustoon milloin tahansa. Palautus näistä palvelupaketeista ovat hyvin samankaltaisia kuin sivuston palauttaminen palautus-palvelupakettia.

1.  Käynnistä vContinuum ja valitse **Hallitse suunnitelmien** > **palauttaminen.** Näet luettelon kaikki suunnitelmat, joita on käytetty epäonnistuu VMs päälle. Voit palauttaa saman palvelupaketin.

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. Valitse Suojaus-suunnitelma ja kaikki sen palautettavan VMs. Kun valitset jokainen AM näet tietoja, kuten ESX palvelimesta, jolloin lähde AM levy. Valitse **Seuraava** palauttaa ohjattu alkavan, ja valitse VMs, jonka haluat palauttaa.

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. Voit palauttaa useita vaihtoehtoja mukaan, mutta on suositeltavaa käyttää **Uusimman tunnistetta** ja valitse **Hae kaikki VMs** varmistaa, että virtuaalikoneen uusimmat tiedot käytetään.
4. Suorita **valmius-valintaruudun.** Tämä tarkistaa, jos oikean parametrit on määritetty sallimaan AM palauttaminen. Jos kaikki tarkistukset onnistuvat, valitse **Seuraava** . Jos et lokista ja korjata virheet.

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  Varmista **AM määritysten** palautumisasetukset on määritetty oikein. Voit tarvittaessa muuttaa AM-asetukset.

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. Katso näennäiskoneiden palautetaan ja määritä palautus-tilaus. Huomaa, että VMs näkyvät käyttämällä tietokoneen isäntänimi. Voi olla vaikea tietokoneen isäntänimi yhdistäminen virtuaalikoneen.
Jos haluat yhdistää nimet, siirry Azure **raporttinäkymät-ikkunan** näennäiskoneiden ja tarkista AM isäntänimi.

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. Suunnitelman nimi ja valitse **Palauta myöhemmin**. On suositeltavaa palauttaa myöhemmin, koska ensimmäinen suojauksen eivät ehkä ole valmis. 
11. Valitse Tallenna suunnitelma tai käynnistää palautus, jos olet valinnut palauttaa nyt tai myöhemmin ei **Palauta** . Voit tarkistaa palautus tilaa nähdä, jos palvelupaketti on tallennettu.

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## <a name="recover-virtual-machines"></a>Palauttaa näennäiskoneiden

Kun palvelupaketti on luotu, voit palauttaa näennäiskoneiden. Ennen kuin Tarkista, näennäiskoneiden on synkronoitiin. Jos replikoinnin tila näkyy OK suojauksen on valmis ja RPO raja-arvo on saavutettu. Voit tarkistaa kunto AM ominaisuuksiin.

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

Poista käytöstä Azure-virtuaalikoneissa, ennen kuin käynnistät palautus. Näin varmistat ei ole jaettu aivot ja että käyttäjät käyttävät vain yhden sovelluksen kopion. 


1.  Käynnistä tallennetun suunnitelma. Valitse vContinuum **näytön** palvelupaketin. Tämä sisältää tiedot niistä palvelupaketeista, jotka on suoritettu.

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  Valitse suunnitelma **palauttaminen** ja **Käynnistä-painiketta**. Voit valvoa palauttaminen. Kun VMs on otettu käyttöön edelleen voit muodostaa yhteyden niihin-vCenter.

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## <a name="protect-to-azure-again-after-failback"></a>Suojaa, Azure uudelleen tuntisesta

Tuntisesta päätyttyä todennäköisesti haluat suojata näennäiskoneiden uudelleen. Tehdä seuraavasti:

1.  Tarkista, näennäiskoneiden paikallisen toimivat ja sovellukset ovat käytettävissä.
2.  Azure sivuston palautus-portaalissa näennäiskoneiden ja poista ne. Valitse tämä vaihtoehto, jos haluat poistaa käytöstä näennäiskoneiden suojaus. Näin varmistat, että VMs enää suojattu.
3.  Azure Poista epäonnistunutta Azuren näennäiskoneiden päälle
4.  Poista vanha virtuaalikoneen vSpehere käyttöön. Nämä ovat VMs, jotka olet aiemmin epäonnistui päälle Azure.
5.  Sivuston palauttaminen portaalissa suojaa VMs, joka epäonnistui viimeksi päälle. Sen jälkeen, kun ne on suojattu voit lisätä ne palautus-palvelupakettia.
 
## <a name="next-steps"></a>Seuraavat vaiheet



- [Lue lisää](site-recovery-vmware-to-azure-classic.md) replikoiminen VMware näennäiskoneiden ja fyysiset palvelimet Azure parannettu käyttöönoton avulla.

 
