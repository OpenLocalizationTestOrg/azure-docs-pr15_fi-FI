<properties
 pageTitle="Suorita TÄHTI-magnesiitin + HPC Pack-Linux VMs | Microsoft Azure"
 description="Ottaa käyttöön Microsoft HPC Pack-klusterin Azure- ja suorita TÄHTI-työ magnesiitin + useita Linux Laske solmujen RDMA verkossa."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Suorita TÄHTI-magnesiitin + Microsoft HPC Pack Linux RDMA-klusterin Azure-tietokannassa
Tässä artikkelissa kerrotaan Azure ja suorita Microsoft HPC Pack klusterin ottamisesta käyttöön [CD-Adapcon TÄHTI-magnesiitin +](http://www.cd-adapco.com/products/star-ccm%C2%AE) työn useita Linux Laske solmuissa, jotka on yhdistetty toisiinsa InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack on suorittaa suurissa HPC ja rinnakkaisia sovellukset, myös MPI-sovelluksia Microsoft Azuren näennäiskoneiden klustereiden monenlaisia ominaisuuksia. HPC Pack tukee myös Linux HPC sovelluksia, jotka on otettu HPC Pack-klusterin Linux Laske-solmu VMs. Perustietoja Linux käyttämällä Laske solmujen HPC Pack on artikkelissa [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>HPC Pack-klusterin määrittäminen
Lataa HPC Pack IaaS käyttöönoton komentosarjat [Download Centeristä](https://www.microsoft.com/en-us/download/details.aspx?id=44949) ja Pura paikallisesti.

Azure PowerShell on. Jos PowerShell ei ole määritetty paikallisessa tietokoneessa, lue artikkeli [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

Milloin tämä kirjoittaminen Azure Marketplace-sivulla (joka sisältää InfiniBand ohjaimet Azure) Linux-kuvat ovat SLES 12, CentOS 6.5 ja CentOS 7.1. Tässä artikkelissa perustuu SLES 12 käyttö. Voit hakea kaikki Linux-kuvia, jotka tukevat HPC on Marketplace-suorittamalla seuraavan PowerShell-komennon:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Tulos on lueteltu sijainti, johon nämä kuvat ovat käytettävissä ja kuvan nimen (**ImageName**) käytetään myöhemmin käyttöönotto-mallin.

Ennen kuin otat klusterin, sinun on HPC Pack-käyttöönoton mallitiedoston luominen. Koska emme ole kohdistamisen pieni klusterin, pää solmu on toimialueen ohjauskoneen ja isännöi SQL-tietokannan.

Seuraavaan malliin käyttöönotto pään solmu, Luo XML-tiedosto nimeltä **MyCluster.xml**ja **SubscriptionId**, **StorageAccount**, **sijainti**, **VMName**ja **palvelun nimi** arvojen korvaaminen omasi.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

Aloita suorittamalla PowerShell-komentoa järjestelmänvalvojan oikeuksin suoritettava komentokehote pää-solmu luominen:

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

20 – 30 minuutin kuluttua pään solmu pitäisi olla valmis. Voit muodostaa siihen Azure-portaalista napsauttamalla virtuaalikoneen **Yhdistä** -kuvaketta.

Voit joutua myöhemmin DNS-forwarder korjaamiseen. Voit tehdä Käynnistä DNS-hallinta.

1.  Palvelimen nimeä DNS-hallintaan hiiren kakkospainikkeella, valitse **Ominaisuudet**ja valitse sitten **välittäjiä** -välilehti.

2.  Voit poistaa minkä tahansa välittäjiä **Muokkaa** -painiketta ja valitse sitten **OK**.

3.  Varmista, että **Käytä pääkansion vihjeitä, jos käytettävissä ei ole välittäjiä** -valintaruutu on valittuna ja valitse sitten **OK**.

## <a name="set-up-linux-compute-nodes"></a>Linux Laske solmujen määrittäminen
Voit ottaa käyttöön Linux Laske solmut saman käyttöönotto-mallin, jota käytit Luo pää solmu avulla.

Kopioi tiedosto **MyCluster.xml** paikallisessa tietokoneessa pään solmu ja Päivitä **NodeCount** -tunniste, jonka haluat ottaa käyttöön solmujen määrän (< = 20). Varmista, ettet ole riittävästi käytettävissä sydämiä Azure kiintiön, koska A9 jokaiselle esiintymälle vie 16 sydämiä-tilaukseesi. Voit käyttää A8 esiintymät (8 sydämiä) A9 sijaan, jos haluat käyttää useita VMs samassa budjetissa.

Kopioi pään solmu HPC Pack IaaS käyttöönotto-komentosarjoja.

Suorita järjestelmänvalvojan oikeuksin suoritettava komentokehote seuraavat PowerShellin Azure-komennot:

1.  Suorita **Lisää AzureAccount** muodostaa Azure-tilaukseen.

2.  Jos sinulla on useita tilauksia, suorittamalla **Get-AzureSubscription** , tee niistä luettelo.

3.  Määritä oletusarvoinen tilauksen suorittamalla **Valitse AzureSubscription - SubscriptionName xxxx-oletusarvon** komento.

4.  Suorita **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** Aloita käyttöönotto Linux Laske solmujen.

    ![Pää-solmu käyttöönotto-toiminto][hndeploy]

HPC Pack klusterin hallinta-työkalun avaaminen Muutaman minuutin kuluttua Linux Laske solmujen säännöllisesti näkyvät klusterin Laske solmujen luettelo. Perinteinen käyttöönotto-tilassa IaaS VMs luodaan peräkkäin. Niin Jos näkyvien solmujen määrän on tärkeää, käytön kaikkien käyttöön voi kestää ajan.

![Linux solmujen HPC Pack klusterin hallinta][clustermanager]

Nyt kun kaikki solmut ovat käytön klusterin, on lisäinfrastruktuuri asetukset.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Windows- ja Linux solmut Azure tiedostoresurssin määrittäminen
Azure-tiedosto-palvelun avulla voidaan tallentaa komentosarjoja, sovelluksen pakettien ja datatiedostot. Azure tiedosto sisältää CIFS ominaisuuksia päälle Azure-Blob-säiliö pysyvä kaupan nimellä. Huomaa, että tämä ei ole useimmat skaalattava ratkaisu, mutta se on helpoin ja erillinen VMs ei edellytä.

Luo Azure-tiedostoresurssin [Windows Azure-tiedostosäilön käytön aloittaminen](..\storage\storage-dotnet-how-to-use-files.md)-artikkelin ohjeiden mukaisesti.

Säilytä tallennustilan tilin nimi, kuten **saname**, Jaa tiedostonimi kuin **resurssi**ja tallennustilan tilin avain kuin **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Ota käyttöön Azure jaetun tiedostoresurssin pään solmun
Avaa järjestelmänvalvojan oikeuksin suoritettava komentokehote ja suorittamalla seuraavan komennon käyttäjätiedot tallennetaan paikallinen kone-säilö:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Liittämään Azure jaetun tiedostoresurssin Suorita:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Ota käyttöön Azure jaetun tiedostoresurssin Linux Laske solmuissa
Hyödyllinen työkalu HPC Pack mukana tulevaa on clusrun-työkalu. Tämän työkalun avulla voit Suorita sama komento samanaikaisesti Laske solmujen joukko. Tässä tapauksessa sitä käytetään käyttöön Azure jaetun tiedostoresurssin ja poistu sen uutta käynnistyy käyttöä varten.
Suorita järjestelmänvalvojan oikeuksin suoritettava komentokehote pään solmun, seuraavat komennot.

Ota hakemiston luominen

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Ota käyttöön Azure jaetun tiedostoresurssin:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Siirtää asennustapa-jakaminen:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Asenna TÄHTI-magnesiitin +
Azure AM esiintymät A8 ja A9 on InfiniBand tuki- ja RDMA ominaisuuksia. Ydin ohjaimet, jotka mahdollistavat nämä ominaisuudet ovat käytettävissä Windows Server 2012 R2, SUSE 12, CentOS 6.5 ja CentOS 7.1 kuvia Azure Marketplacesta. Microsoft MPI ja Intel MPI (julkaisu 5.x) on kaksi MPI-kirjastoja, jotka tukevat näitä ohjaimia Azure-tietokannassa.

CD-Adapcon TÄHTI-magnesiitin + Vapauta 11.x, ja myöhemmin on yhdistetty Intel MPI versio 5.x, joten Azure InfiniBand tuki sisältyy.

Hae Linux64 TÄHTI-magnesiitin +-paketti [CD-Adapcon portal](https://steve.cd-adapco.com). Tässä tapauksessa on käytetty eri tarkkuus 11.02.010 versio.

Luo pään solmu **/hpcdata** Azure tiedoston Jaa-liittymän komentosarja nimeltä **setupstarccm.sh** seuraavat sisältöä. Tämä komentosarja suoritetaan kunkin Laske solmun määrittämään TÄHTI-magnesiitin + paikallisesti.

#### <a name="sample-setupstarcmsh-script"></a>Esimerkki setupstarcm.sh komentosarja
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Nyt voit määrittää TÄHTI-magnesiitin + kaikki Linux Laske solmujen, Avaa järjestelmänvalvojan oikeuksin suoritettava komentokehote ja suorittamalla seuraavan komennon:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Komennon ollessa käynnissä voit valvoa suorittimen käyttö lämpökartta klusterin hallinnan avulla. Muutaman minuutin kuluttua kaikki solmut pitäisi olla määritetty oikein.

## <a name="run-star-ccm-jobs"></a>Suorita TÄHTI-magnesiitin + töitä
Työn ajoituksen ominaisuudet jotta voit suorittaa TÄHTI käytetään HPC Pack-magnesiitin + työt. Voit tehdä annettava joitakin komentosarjoja, joiden avulla voidaan aloittaa työn ja suorita TÄHTI tuki-magnesiitin +. Syöttötiedot säilytetään Azure jaetun tiedostoresurssin ensimmäisen yksinkertaisuuden.

Jonon TÄYTTÖKUVIONA käytetään seuraavaa PowerShell-komentosarjaa-magnesiitin + työn. Se on kolme argumenttia:

*  Mallinimi

*  Käytettävän solmujen määrän

*  Sydämiä, jota käytetään kunkin solmun määrä

Koska TÄHTI-magnesiitin + voit täyttää muistin-kaistanleveys on yleensä parempi vähemmän sydämiä Laske solmujen kohden ja Lisää uusi solmujen. Solmun kohti sydämiä tarkka määrä riippuu suoritinperhe ja interconnect nopeuden.

Solmut on varattu yksinomaan työn ja ei voi jakaa muut työt. Työn ei ole aloitettu kuin MPI-työ suoraan. **Runstarccm.sh** -liittymän komentosarja alkaa MPI-avain.

Syötteen mallin ja **runstarccm.sh** komentosarja on tallennettu **/hpcdata** Jaa, joka oli aiemmin otettu käyttöön.

Lokitiedostojen nimetään työn tunnus, ja se tallennetaan **/hpcdata jakaminen**, sekä TÄHTI-magnesiitin + tulosteen tiedostot.


#### <a name="sample-submitstarccmjobps1-script"></a>Esimerkki SubmitStarccmJob.ps1 komentosarja
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Korvaa **runner.java** ensisijainen TÄHTI-magnesiitin + Java mallin avain ja kirjaaminen koodi.

#### <a name="sample-runstarccmsh-script"></a>Esimerkki runstarccm.sh komentosarja
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

Tutustu testi on käytetty Power pyydettäessä käyttöoikeus-tunnuksen. Sinulla on kyseisen tunnuksen **$CDLMD_LICENSE_FILE** ympäristömuuttuja asettaminen **1999@flex.cd-adapco.com** ja avaimen komentorivi **- podkey** -asetus.

Jotkin alustamisen jälkeen komentosarja poimii-- **$CCP_NODES_CORES** ympäristömuuttujat, jotka HPC Pack määrittäminen--luettelon solmujen luonnissa hostfile, joka käyttää MPI-avain. Tämä hostfile sisältää Laske solmu nimet, joita käytetään työn yksi nimi riviä kohden.

Muodossa **$CCP_NODES_CORES** seuraa tätä mallia:

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Jos:

* `<Number of nodes>`on varattu työ solmujen määrän.

* `<Name of node_n_...>`on varattu työ kunkin solmun nimi.

* `<Cores of node_n_...>`on varattu työ solmun sydämiä määrä.

Sydämiä (**$NBCORES**) määrä lasketaan myös (**$NBNODES**) solmujen määrän ja sydämiä kohti solmu (taulukkona parametrin **$NBCORESPERNODE**) määrän perusteella.

MPI-asetukset, joita käytetään Intel MPI Azure-niistä ovat:

*   `-mpi intel`Jos haluat määrittää Intel MPI.

*   `-fabric UDAPL`Jos haluat käyttää Azure InfiniBand verbien.

*   `-cpubind bandwidth,v`Jos haluat optimoida kaistanleveyden MPI kanssa TÄHTI-magnesiitin +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Jotta Intel MPI Azure InfiniBand käsitteleminen ja määrittää sydämiä kohti solmu tarvittava määrä.

*   `-batch`Aloita TÄHTI-magnesiitin + erä tilassa ei ole käyttöliittymän.


Lopuksi alkavan työn, varmista, että oman solmujen on hyvin alkuun ja online-klusterin hallinta. Valitse PowerShell komentoriviltä, suorita tämä:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Lopeta solmujen
Myöhemmin sen jälkeen, kun enää tarvitse testejä, voit tehdä seuraavat HPC Pack PowerShell-komennot Lopeta ja Käynnistä solmujen:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Seuraavat vaiheet
Yrittää muiden Linux toiminnoista. Esimerkiksi seuraavissa artikkeleissa:

* [Suorita NAMD Microsoft HPC Pack Azure Linux Laske solmuissa](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Suorita OpenFOAM Microsoft HPC Pack Linux RDMA klusterissa Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
