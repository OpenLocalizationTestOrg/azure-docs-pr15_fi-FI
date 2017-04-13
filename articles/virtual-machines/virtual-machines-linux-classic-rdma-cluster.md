<properties
 pageTitle="Linux RDMA klusterin MPI sovelluksia | Microsoft Azure"
 description="Luo Linux-klusterin koon H16r, H16mr, A8 tai A9 VMs käyttää Azure RDMA verkon MPI sovellukset"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Linux RDMA klusterin MPI sovelluksia määrittäminen


Opi määrittämään Linux RDMA klusterin Azure-tietokannassa ja [H - tai Laske paljon A sarjan VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md) rinnakkain viestin kulkeva Interface (MPI)-sovelluksia. Tässä artikkelissa kuvataan Linux HPC-kuva suorittaa Intel MPI klusteri valmisteleminen. Tämän jälkeen voit ottaa käyttöön kuvassa ja yksi RDMA-yhteyttä hyödyntäviin Azure AM kokoisille (tällä hetkellä H16r, H16mr, A8 tai A9) VMs klusterin. MPI-sovelluksia, jotka tekstiviestintää tehokkaasti kautta pienen viive klusterin avulla, suuri siirtonopeuden verkon perusteella remote suoraan muistin käyttö (RDMA)-tekniikkaa.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Klusterin käyttöönottoasetukset

Seuraavassa on tapoja luoda Linux RDMA klusterin kanssa tai ilman työn aikataulutus.


* **Azure CLI komentosarjat** - näytössä näkyvät jäljempänä tämän artikkelin avulla [Azure käyttöliittymä](../xplat-cli-install.md) (CLI) RDMA-yhteyttä hyödyntäviin VMs klusterin käyttöönoton komentosarja. Hallinnan tilassa CLI Luo klusterisolmut peräkkäin perinteinen käyttöönotto-mallin niin monta Laske solmujen käyttöönotto voi kestää useita minuutteja. Käyttöön RDMA verkkoyhteyden tilan, kun käytät perinteistä käyttöönotto-mallin, ota käyttöön VMs saman pilvipalvelussa.

* **Azure Resurssienhallinta mallit** - resurssien hallinnan käyttöönottomalli voit käyttää myös klusterin RDMA-yhteyttä hyödyntäviin VMs, joka yhdistää RDMA verkon käyttöön. Voit [luoda oman mallin](../resource-group-authoring-templates.md), tai Tarkista [Azure pikaopas mallit](https://azure.microsoft.com/documentation/templates/) Microsoft tai yhteisön ratkaisun, ottamaan siirtämien malleille. Resurssienhallinta mallit ovat nopea ja luotettava tapa ottaa Linux-klusterin. Käyttöön RDMA verkkoyhteyden tilan, kun käytät resurssien hallinnan käyttöönottomalli, ota käyttöön-käytettävyys samat VMs.

* **HPC Pack** - Microsoft HPC Pack-klusterin luominen Azure ja lisää RDMA-yhteyttä hyödyntäviin Laske-solmut, jotka suoritetaan RDMA verkon käytön tuetut Linux-jakauman. Katso [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Esimerkki käyttöönoton ohjeita perinteinen malli

Seuraavissa vaiheissa kuvataan käyttämään Azure-CLI SUSE Linux Enterprise Server (SLES) 12 SP1 HPC AM Azure Marketplacesta käyttöönotosta, mukauttaa sitä ja luo mukautettuja AM kuva. Valitse RDMA-yhteyttä hyödyntäviin VMs klusterin käyttöönoton komentosarja kuvan avulla. 

>[AZURE.TIP]Samalla ohjeiden avulla voit ottaa klusterin RDMA-yhteyttä hyödyntäviin VMs perusteella muut tuetut HPC kuvat Azure Marketplacesta. Tietyt vaiheet voivat vaihdella hiukan mainittuja. Intel MPI on esimerkiksi sisältää ja määritetty vain joidenkin kuvien. Ja jos otat käyttöön SLES 12 HPC AM SLES 12 SP1 HPC AM sijaan, sinun on päivitettävä RDMA-ohjaimet. Lisätietoja on artikkelissa [tietoja A8 ja A9, A10, A11 Laske paljon esiintymät](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Edellytykset

*   **Asiakastietokone** - tarvitset Mac, Linux tai Windows-asiakasohjelma tietokoneen Azure yhteydessä. Seuraavassa oletetaan, että käytät Linux-asiakasohjelmaa.

*   **Azure tilauksen** – Jos ei ole tilausta voit luoda [vapaa tilin](https://azure.microsoft.com/free/) muutamaan minuuttiin. Harkitse suurempi klustereiden tukiyhteyksiä tilauksen tai Osta muita asetuksia. 

*   **AM koon käytettävyys** - tällä hetkellä seuraavia esiintymän koot ovat RDMA pystyvät: H16r, H16mr, A8 ja A9. Tarkista käytettävyys Azure alueilla [tuotteiden alueittain](https://azure.microsoft.com/regions/services/) . 

*   **Sydämiä kiintiön** – voit joutua muuttamaan sydämiä ottamaan Laske paljon VMs klusterin kiintiön Suurenna. Sinun on esimerkiksi vähintään 128 sydämiä, jos haluat ottaa käyttöön 8 A9 VMs, kuten tässä artikkelissa. Tilauksen myös saattaa rajoittaa tiettyjä AM koon perheille, mukaan lukien H-sarjan käyttöönottoa sydämiä määrä. Pyytää kiintiön Suurenna, [Avaa asiakkaan online-tukipyynnön](../azure-supportability/how-to-create-azure-support-request.md) maksutta. 

*   **Azure CLI** - [asentaa](../xplat-cli-install.md) Azure-CLI ja asiakastietokone [muodostaa yhteyden Azure tilauksen](../xplat-cli-connect.md) .


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Vaihe 1. Valmistele SLES 12 SP1 HPC-AM

Kun kirjautuminen Azure Azure-CLI kanssa `azure config list` vahvistamaan, että tulos näyttää hallinnan tilan. Jos se ei ole, Määritä tilaan suorittamalla tämän komennon:


    azure config mode asm


Kirjoita luettelon kaikki haluamasi tilaukset, sinulla on oikeus käyttää:


    azure account list

Nykyinen aktiivinen tilaus yhdistetään `Current` asettaminen `true`. Jos tämä tilaus ei ole haluamasi avulla voit luoda klusterin, määrittää haluamasi tilauksen tunnus Aktiivinen tilaus:

    azure account set <subscription-Id>

Näet Azure yleisesti saatavilla SLES 12 SP1 HPC kuvat, suorittaa komennon seuraavankaltaiselta, olettaen että shell ympäristön tukee **grep**:


    azure vm image list | grep "suse.*hpc"

Valmistele nyt RDMA-yhteyttä hyödyntäviin AM, SLES 12 SP1 HPC kuvan suorittamalla komennon seuraavankaltaiselta:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Jos

* Koon (Tässä esimerkissä A9) on yksi RDMA-yhteyttä hyödyntäviin AM koot.

* Ulkoisen SSH porttinumero (Tässä esimerkissä, joka on SSH oletusarvo 22) on mikä tahansa kelvollinen porttinumero. Sisäinen SSH porttinumero on määritetty 22.

* Azure sijainti määrittämän alueen luodaan uusi pilvipalvelussa. Määritä sijainti, jossa voit valita AM koko on käytettävissä.

* Tällä hetkellä SLES 12 SP1 kuvan nimi voi `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` tai `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` SUSE prioriteetti-tukea (muut ovat voimassa).

### <a name="step-2-customize-the-vm"></a>Vaihe 2. Mukauta AM

Kun AM päättää valmistelu SSH AM AM ulkoinen IP-osoite (tai DNS-nimi) avulla ja ulkoisen portin numeron, voit määrittää, ja mukauta sitä. Yhteystiedot Tutustu [virtuaalikoneen käynnissä Linux Kirjaudu](virtual-machines-linux-mac-create-ssh-keys.md). Suorittaa komentoja olet määrittänyt AM-käyttäjänä, ellei vaiheen suorittamiseen tarvittavan pääkansion access.

>[AZURE.IMPORTANT]Microsoft Azure ei tarjoa Linux VMs pääkansion access. Ja Myönnä järjestelmänvalvojan oikeudet, kun yhteys on luotu AM käyttäjäksi, suorita komentojen `sudo`.

* **Päivitykset** – Asenna päivitykset **zypper**avulla. Voit myös haluta ja asenna NFS apuohjelmat. 

    >[AZURE.IMPORTANT]SLES 12 SP1 HPC AM on suositeltavaa, älä käytä ydin päivitykset, jotka saattavat aiheuttaa ongelmia kanssa Linux RDMA ohjaimet.

* **Intel MPI** - asennuksen Intel-SLES 12 SP1 HPC AM MPI suorittamalla seuraavan komennon:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Lukitse muistin** - varten MPI koodit, voit lukita käytettävissä oleva muisti RDMA, lisätä tai muuttaa /etc/security/limits.conf tiedoston seuraavat asetukset. (Tarvitset pääkansion pääsy Muokkaa tiedostoa.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Testausta varten, voit myös määrittää memlock rajoittamaton. Esimerkki: `<User or group name>    hard    memlock unlimited`. Lisätietoja on artikkelissa [Parhaat tunnetut viestintämenetelmien asetus lukittu muistikoko](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **SSH näppäimet SLES VMs** – Luo SSH näppäimet, jotta voit vahvistaa Salli tilin Laske solmujen SLES klusterin, kun MPI töitä. (Jos olet asentanut CentOS perustuva HPC AM, älä noudata tämän vaiheen. Katso ohjeet artikkelin määrittämään passwordless SSH luota klusterin solmujen jälkeen voit tallentaa kuvan ja ottaa käyttöön klusterin.) 

    Suorita seuraava komento luo SSH avaimet. Kun ohjelma pyytää syötteen, painamalla Enter luomiseen näppäimet oletussijainti määrittämättä salasana.

        ssh-keygen

    Liitä julkinen avain tunnettujen julkisia näppäimet authorized_keys tiedostoon.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    ~/.Ssh kansioon Muokkaa tai luo "määritystiedosto". Anna, joita aiot käyttää Azure (Tässä esimerkissä 10.32.0.0/16) yksityisverkon IP-osoitealueita:

        host 10.32.0.*
        StrictHostKeyChecking no

    Voit myös luettelon kunkin AM yhteyttä klusterin yksityisverkon IP-osoitteen seuraavasti:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Määrittäminen `StrictHostKeyChecking no` mahdollisen tietoturvariskin voit luoda, kun IP-osoite tai aluetta ei ole määritetty.

* **Sovellusten** – Asenna tarpeen tämän AM tai suorittaa muut mukautukset, ennen kuin voit tallentaa kuvan.

### <a name="step-3-capture-the-image"></a>Vaihe 3. Sieppaa kuva

Ottaa kuvan suorittamalla seuraavan komennon ensin Linux AM. Tämä komento AM deprovisions, mutta säilyttää käyttäjätilit ja SSH näppäimet, voit määrittää.

```
sudo waagent -deprovision
```

Asiakastietokoneesta, suorita seuraavat Azure CLI komennot, ottaa kuvan. Katso, [miten voit siepata perinteinen Linux virtual machine kuvana](virtual-machines-linux-classic-capture-image.md) lisätietoja.  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Kun olet suorittanut näitä komentoja, AM kuvan kirjataan käyttöön ja AM poistetaan. On nyt valmis ottamaan klusterin mukautetun kuvan.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Vaihe 4. Klusterin kuvan käyttöönotto

Muokkaa seuraavaa Bash komentosarjaa tarvittavat arvot-ympäristön ja suorita asiakastietokoneesta. Azure käyttää VMs peräkkäin perinteinen käyttöönoton mallissa, koska se kestää muutaman minuutin 8 A9 VMs, tämä komentosarja ehdotetut otetaan käyttöön.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Huomioitavaa CentOS HPC-klusterin

Halutessasi voit määrittää yhden Azure Marketplace-sivustossa sen sijaan, että SLES 12 CentOS perustuva HPC kuvat käsittelevät perusteella klusterin noudattamalla Yleiset on edellisen osion. Huomaa seuraavat erot, kun valmistelu ja määrittää AM:

1. Intel MPI on jo asennettu AM, CentOS perustuva HPC kuvasta valmistelun yhteydessä. 

2. Muistin lukitusasetukset on jo lisätty AM /etc/security/limits.conf tiedostossa.

2. Luo SSH näppäimiä voit valmistella AM sieppauksen. On suositeltavaa, käyttäjän lomakepohjaisen todennuksen määrittäminen sen jälkeen, kun otat cluser. Luomisesta on seuraavassa osassa.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Klusterin passwordless SSH luota määrittäminen

CentOS perustuva HPC-klusterissa on kaksi tapaa Luotettavat Laske solmujen välillä: Host (isäntä)-pohjainen ja käyttäjä-pohjainen todennusta. Host (isäntä)-pohjainen todennus on tämän artikkelin laajuus ulkopuolella ja yleensä on tehtävä tunniste-komentosarjan kautta käyttöönoton aikana. Käyttäjä-pohjainen todennus on helppo Luotettavat käyttöönoton jälkeen ja luominen ja jakaminen klusterin SSH näppäimet solmujen suorittaminen edellyttää. Tämä menetelmä tunnetaan yleisesti nimellä passwordless SSH kirjautuminen ja tarvitaan, kun MPI töitä. 

Esimerkki komentosarjan lähettänyt yhteisöstä on käytettävissä [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) käyttöön helposti käyttöoikeuksien CentOS perustuva HPC klusterissa. Lataa ja käyttää tätä komentosarjaa seuraavien ohjeiden mukaisesti. Voit myös muokata tämän komentosarjan tai minkä tahansa muun tavan avulla voit määrittää passwordless SSH todennusta klusterin Laske solmujen välillä.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Suorita komentosarja, on tiedettävä aliverkon IP-osoitteiden etuliite. Pyydä etuliite suorittamalla seuraavan komennon johonkin klusterisolmut. Tulostuskohde pitäisi näyttää suunnilleen 10.1.3.5 ja etuliite on 10.1.3 osaan.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Suorita nyt kolme parametreilla komentosarja: Laske solmuissa yleisiä käyttäjänimi, kyseisen käyttäjän Laske solmut ja aliverkon etuliite, jota edellisen komennon palautti yleisiä salasana.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

Tämä komentosarja tekee seuraavat toimet:

* Luo kansio host-solmu nimeltä .ssh, jota tarvitaan passwordless kirjautuminen. 
* Luo .ssh kansioon, joka ohjaa passwordless kirjautuessasi minkä tahansa solmu kirjautuminen sallia klusterin määritystiedostoa. 
* Luo tiedostoja, jotka sisältävät solmu nimet ja solmu-klusterin solmut IP-osoitteet. Nämä tiedostot jäävät, kun komentosarja on suoritettu myöhempää käyttöä varten. 
* Luo yksityinen ja julkinen avaimen pari kunkin klusterin solmun, mukaan lukien host-solmu ja tapahtumia authorized_keys-tiedostossa.

>[AZURE.WARNING]Tämän komentosarjan suorittaminen luoda mahdollisen tietoturvariskin. Varmistaa, että ~/.ssh julkisen avaimen tiedot jaetaan ei.


## <a name="configure-intel-mpi"></a>Intel MPI määrittäminen

Azure Linux RDMA MPI sovellusten suorittamiseen haluat määrittää tietyn Intel MPI tiettyjä ympäristömuuttujia. Tässä on esimerkki Bash komentosarjan, joka määrittää sovelluksen suorittamiseen muuttujat. Muuttaa polkua mpivars.sh asennukseen Intel MPI tarvittaessa.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Host (isäntä)-tiedoston muoto on seuraava. Voit lisätä yhden rivin kunkin solmun yhteyttä klusterin. Määritä yksityisten IP-osoitteiden kohteesta VNet määritettyjä aiemmin ei DNS nimiä. Kaksi isäntien IP-osoitteita 10.32.0.1 ja 10.32.0.2 tiedosto sisältää esimerkiksi seuraavasti:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Suorita MPI basic kahden solmun klusterissa

Jos et ole vielä tehnyt niin, Määritä ympäristön Intel MPI varten. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Yksinkertainen MPI-komennon suorittaminen

Yksinkertainen MPI-komennon suorittaminen johonkin näyttämään MPI on asennettu oikein ja kommunikoida välillä on vähintään kaksi Laske solmujen Laske-solmut. Seuraava **mpirun** komento suoritetaan kaksi solmujen **hostname** -komento.

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Tulostuskohde olisi luettelon nimet, jotka voit välittää syöte solmujen `-hosts`. Esimerkiksi **mpirun** -komento, jossa on kaksi solmujen palauttaa tulosteen seuraavankaltaiselta:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Suorita MPI ensisijainen

Intel MPI seuraava komento suorittaa pingpong-ensisijainen vahvistamiseksi klusterimääritys ja RDMA verkkoyhteyden.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Kaksi solmujen kanssa toimiva-klusterissa pitäisi näkyä tulosteen seuraavankaltaiselta. Azure RDMA verkon toiminta viive tai sen alla olevia 3 mikrosekuntia viestin koko on enintään 512 tavua.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Seuraavat vaiheet

* Kokeile käyttöönotto ja Linux-klusterin sovellusten suorittaminen Linux MPI.

* Katso ohjeet [Intel MPI kirjaston asiakirjat](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) Intel MPI.

* Yritä [pikaopas mallin](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) Intel Lustre-klusterin luominen käyttämällä CentOS perustuva HPC kuva. Lisätietoja on artikkelissa tämä [blogimerkintä](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
