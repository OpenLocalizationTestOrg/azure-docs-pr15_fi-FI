<properties
    pageTitle="Optimointi Linux-AM Azure | Microsoft Azure"
    description="Katso, varmista, että olet määrittänyt Linux-AM Azure-suorituskyvyn optimointi vihjeitä"
    keywords="Linux virtuaalikoneen, virtuaalikoneen linux ubuntu virtuaalikoneen" 
    services="virtual-machines-linux"
    documentationCenter=""
    authors="rickstercdn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="rclaus"/>

# <a name="optimize-your-linux-vm-on-azure"></a>Optimoi Linux-AM Azure-

Linux virtual machine (AM) luominen on helppo tehdä komentoriviltä tai -portaalista. Tässä opetusohjelmassa näytetään, miten voit varmistaa, että olet määrittänyt sen Microsoft Azure-ympäristössä sen suorituskyvyn parantamiseksi. Tässä ohjeaiheessa käyttää Ubuntu-palvelimen AM, mutta voit myös luoda Linux virtual tietokoneeseen käyttämällä [omia kuvia malleiksi](virtual-machines-linux-create-upload-generic.md).  

## <a name="prerequisites"></a>Edellytykset

Tässä artikkelissa oletetaan, sinulla on jo toimimasta Azure tilauksen ([ilmainen kokeiluversio liittyminen](https://azure.microsoft.com/pricing/free-trial/)) [asennettu Azure-CLI](../xplat-cli-install.md) ja on jo valmisteltu AM Azure-tilaukseen. Ennen kuin mikä tahansa Azure - kanssa harjoittelu on todentaa tilaukseen. Voit tehdä tämän ja Azure CLI kirjoittaa `azure login` vuorovaikutteinen Aloita. 

## <a name="azure-os-disk"></a>Azure OS levy

Kun luot Linux AM Azure, siinä on kaksi levyä liittyy. /dev/sda OS levytilaa, /dev/sdb on väliaikainen levytilaa.  Älä käytä tärkeimmät OS-levy (/ keskihajonta/sda) jokin muu kuin käyttöjärjestelmän sellaisena kuin se on tarkoitettu nopea AM käynnistyksen aikana ja tarjoa hyvän suorituskyvyn, että toiminnoista. Liitettävä pienempään oman AM saat pysyvä ja optimoitu tallennustilan tiedoillesi. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Koko ja suorituskyky kohteiden lisääminen levyjen 

AM koon perusteella, voit lisätä enintään 16 muita levyille A-sarjan, 32 levylle D muodostuvan sarjan ja G-sarjan 64 levyille koneen - jokainen enintään 1 TT: n kokoisia. Voit lisätä ylimääräisen levyjen tarvittaessa tilaa ja IOps vaatimukset kohden. Kunkin levyn on Premium tallennustilan 500 IOps kohteeksi suorituskyvyn vakio tallennustilan ja ylöspäin 5000 IOps levyä kohden.  Lisätietoja Premium tallennustilan levyjen viitata [Premium tallennustilan: tehokas tallennustila Azure VMs](../storage/storage-premium-storage.md)

Tavoitteet suurin IOps Premium tallennustilan levyille, jossa niiden asetukset on määritetty vain-luku"tai"Ei", sinun on poistettava käytöstä"esteistä"aikana käyttöönottaminen Linux tiedostojärjestelmän. Sinun ei tarvitse esteistä, koska Premium tallennustilan palautettua levyille kirjoituksia kestävät nämä asetukset.

- Jos käytät **reiserFS**, poista käytöstä esteistä asennustapa-vaihtoehdon "esteen = ei mitään" (esteistä on otettu käyttöön, käytä "esteen kohdistus =")
- Jos käytät **ext3/ext4**, poista käytöstä esteistä asennustapa-vaihtoehdon "esteen = 0" (esteistä on otettu käyttöön, käytä "esteen = 1")
- Jos käytät **XFS**, poista käytöstä esteistä asennustapa-vaihtoehdon "nobarrier" (käyttöönoton esteistä, käytä vaihtoehto "esteen")

## <a name="storage-account-considerations"></a>Tallennustilan tilin huomioon otettavia seikkoja

Kun luot Linux AM Azure, varmista että liität levyjen tallennustilan tileistä asuvat samassa alueella kuin oman AM varmistamiseksi lähellä toisiaan ja pienentää verkon latenssin.  Vakio tallennustilan tileille on enintään 20 k IOps ja 500 TT koon kapasiteetti.  Tämä toimii noin 40 raskaasti käytetyt levyjä, kuten OS levyn ja kaikki tiedot levyjä, voit luoda. Premium tallennustilan tilien ei ole suurin IOps rajoitettu mutta parannettavaa on 32 TT kokorajoituksen. 

Kun käsittely hyvin IOps toiminnoista ja olet valinnut vakio tallennustilan levyille, voit joutua muuttamaan levyjen jakaminen useiden tallennustilan tilit ja varmista, että valitset on ei 20 000 IOps rajoitus vakio tallennustilan tilit. Oman AM voi olla mix levyjen yli eri tallennustilan tilit ja tallennustilaa tilityypit optimaalisen kokoonpanosi saavuttamiseksi. 

## <a name="your-vm-temporary-drive"></a>AM tilapäinen levyasemasta

Oletusarvoisesti luodessasi AM, Azure voi OS-levy (/ keskihajonta/sda) ja tilapäinen DVD-levyllä (/ keskihajonta/sdb).  Kaikkia muita levyjä, voit lisätä näkyvät /dev/sdc, /dev/sdd, /dev/sde ja niin edelleen. Kaikkien tietojen tilapäinen levytilaa (/ keskihajonta/sdb) ei ole kestävät ja voit menetetään, jos tiettyjä tapahtumia, kuten AM koon, lukea, tai ylläpito pakottaa oman AM uudelleenkäynnistämistä.  Koko ja tilapäinen levyn tyypin liittyy AM koon valitsemasi käytön aikana. Tilapäiset aseman varmuuskopioidaan kyseessä jokin premium koon VMs (DS, G ja DS_V2 sarja) paikallisen Suoritettaessa muita suorituskyvyn 48 k IOps. 

## <a name="linux-swap-file"></a>Vaihda Linux-tiedosto

Jos Azure AM on Ubuntu tai CoreOS kuvan, voit käyttää CustomData cloud-config lähettämiseen cloud alusta. Jos olet [mukautetun Linux-kuva on ladattu](virtual-machines-linux-upload-vhd.md) , joka käyttää cloud alusta, myös määritettävä swap osioiden avulla cloud alusta.

Ubuntu Cloud kuvista sinun on käytettävä cloud alusta määrittäminen swap-osio. Katso lisätietoja Ubuntu wikissä seuraavalla sivulla: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Kuvien ilman cloud alusta tukea AM kuvia Azure Marketplace-tiedot käytössä on integroitu käyttöjärjestelmän AM Linux-agentti. Tämä agentti avulla AM vuorovaikutuksessa eri Azure palveluihin. Jos olet ottanut vakio kuva Azure Marketplace-sivustosta, on määritettävä oikein Linux Vaihda tiedoston asetuksia seuraavasti:

Etsi ja muokata **/etc/waagent.conf** tiedostossa on kaksi merkintää. Ne hallita erillinen Vaihda tiedoston olemassaolo ja vaihda tiedoston kokoa. Voit muokata hakua parametrit ovat `ResourceDisk.EnableSwap=N` ja`ResourceDisk.SwapSizeMB=0` 

Haluat muuttaa ne seuraavasti:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size megatavuina vastaamaan omia tarpeita} 

Kun olet tehnyt muutoksen, tarvitset waagent tai uudelleenkäynnistyksen Linux-AM muutosten mukaisiksi.  Tiedät, että muutokset on toteutettu ja vaihda-tiedosto on luotu, kun käytät `free` komento tuo tilaa. Seuraavassa esimerkissä on luotu tuloksena waagent.conf-tiedoston muokkaaminen 512 Megatavua swap tiedosto.

    admin@mylinuxvm:~$ free
                total       used       free     shared    buffers     cached
    Mem:       3525156     804168    2720988        408       8428     633192
    -/+ buffers/cache:     162548    3362608
    Swap:       524284          0     524284
    admin@mylinuxvm:~$
 
## <a name="io-scheduling-algorithm-for-premium-storage"></a>I/o Premium tallennustilan ajoituksen salausalgoritmi

Ja 2.6.18 Linux ydin, i/o ajoituksen algoritmin on muutettu määräajan, CFQ (kokonaan tiedenäyttelyn queuing algoritmin) oletusarvo. RAM i/o kuvioita on vähäinen ero CFQ ja määräpäivän suorituskyvyn erot.  Missä levyn i/o-kuvion pääosin peräkkäisiä Suoritettaessa perustuva levyjen siirrytään takaisin NOOP tai määräajan algoritmin voit tehostaa i/o suorituskyvyn parantamiseksi.

### <a name="view-the-current-io-scheduler"></a>Tarkastele nykyistä i/o-ajoitus

Kirjoita seuraava komento:  

    admin@mylinuxvm:~# cat /sys/block/sda/queue/scheduler

Näkyviin tulee tulos, joka viittaa nykyiseen päivitysyrityksen jälkeen.  

    noop [deadline] cfq

###<a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Muuta ajoitus algoritmin i/o nykyinen laite (/ keskihajonta/sda)

Käytä seuraavia komentoja:  

    azureuser@mylinuxvm:~$ sudo su -
    root@mylinuxvm:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mylinuxvm:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mylinuxvm:~# update-grub

>[AZURE.NOTE] Tämän määrittämistä varten /dev/sda yksin ei ole hyödyllisiä. Se on voi määrittää kaikki tiedot levyjä, jossa peräkkäisten i/o dominates i/o-kuvion.  

Näkyy seuraava tulos, joka ilmaisee, että grub.cfg on koottu onnistuneesti ja että oletusarvo-ajoitus on päivitetty NOOP.  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

Jakauman Redhat, perheen tarvitset vain seuraava komento:   

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="using-software-raid-to-achieve-higher-iops"></a>Käyttämällä ohjelmiston RAID saavuttamiseksi suurempi voin / Ops

Oman työmääriä vaativatko enemmän IOps kuin yhdelle levylle voidaan lisätä haluat käyttää useita levyjä ohjelmiston RAID kokoonpanoa. Koska Azure suorittaa jo levyn vikasietoisuudelle paikallisen kangasta tasolla, saat parhaan mahdollisen suorituskyvyn RAID 0 raitasarjoittamista-asetuksista.  Sinun täytyy valmistelu ja luoda uusia levyjä Azure-ympäristössä ja liittää ne Linux-AM jakaminen, muotoilu ja asemat käyttöönottaminen ennen.  Lisätietoja määrittäminen ohjelmiston RAID asetukset Linux-AM Azure-tietokannassa löytyy **[Määrittäminen ohjelmiston RAID Linux](virtual-machines-linux-configure-raid.md)** -asiakirjassa.


## <a name="next-steps"></a>Seuraavat vaiheet

Muista, kun kaikki optimointi keskustelut kanssa, sinun täytyy tehdä testejä ennen ja jälkeen muuttuessa mitata on muutoksen vaikutus.  Optimointi on vaiheittainen prosessi, jossa on erilaisia tuloksia eri tietokoneissa ympäristössä.  Mitä soveltuu yksi määritys ei ehkä toimi muiden.

Jotkin hyödyllisiä linkkejä lisäresursseihin: 

- [Premium tallennustila: Azure virtuaalikoneen työmääriä tehokas tallennustila](../storage/storage-premium-storage.md)
- [Azure Linux-agentti käyttöopas](virtual-machines-linux-agent-user-guide.md)
- [Valitse Azure Linux VMs MySQL optimoinnista](virtual-machines-linux-classic-optimize-mysql.md)
- [Määritä ohjelmiston RAID Linux](virtual-machines-linux-configure-raid.md)
