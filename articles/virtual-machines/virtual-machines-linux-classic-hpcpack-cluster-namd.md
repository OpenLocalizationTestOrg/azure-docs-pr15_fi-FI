<properties
 pageTitle="Microsoftin HPC Pack-Linux VMs NAMD | Microsoft Azure"
 description="Ottaa käyttöön Microsoft HPC Pack-klusterin Azure- ja näyttää NAMD simuloinnissa charmrun useita Linux Laske solmuissa"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Suorita NAMD Microsoft HPC Pack Azure Linux Laske solmuissa

Tämän artikkelin avulla voit suorittaa Azuren näennäiskoneiden Linux tehokas tietojenkäsittely (HPC) työmäärää. Tässä kohdassa voit määrittää [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) klusterin Azure-Linux Laske solmujen kanssa ja suorita [NAMD](http://www.ks.uiuc.edu/Research/namd/) simuloinnissa, laskeminen ja Visualisoi suuri biomolecular järjestelmän rakenteen.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (Nanoscale molekyyli Dynamics näyttökielet) on suunniteltu tehokas simuloinnissa sisältävät enintään atomeja miljoonia suuri biomolecular järjestelmät rinnakkain molekyyli dynamics paketti. Esimerkkejä järjestelmät ovat viruksilta, solun rakenteet ja suuri proteiinien. NAMD Skaalaa sydämiä tyypillinen simulaatioissa satoja, ja yli 500 000 sydämiä suurin simulaatioissa.

* **Microsoft HPC Pack** sisältää ominaisuuksia, suorita suurissa HPC ja rinnakkaisia sovellusten paikalliseen tietokoneeseen tai Azuren näennäiskoneiden klustereissa. Alun perin kehitetty Windows HPC toiminnoista, HPC Pack ratkaisuksi nyt Linux sovellusten suorittaminen Linux HPC tukee Laske solmu VMs HPC Pack-klusterin käyttöön. Katso esittely [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md) .

Muita vaihtoehtoja, voit suorittaa Linux HPC työmääriä Azure kohdassa [tekniset resurssit Siirtoerän ja tehokas tietojenkäsittely](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Edellytykset

* **HPC Pack klusteri Linux ja Laske solmut** - käyttöönotto HPC Pack klusteri ja Linux Laske solmujen [Azure Resurssienhallinta-mallin](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) toista tai [Azure PowerShell-komentosarjaa](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)Azure. Katso edellytykset ja vaiheet kummassakin [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md) . Jos valitset PowerShell-komentosarjojen käyttöönottoa, katso määritysten mallitiedostossa mallitiedostojen on tämän artikkelin lopussa olevassa kohdassa. Tämä tiedosto määrittää pään Windows Server 2012 R2-solmu ja neljä kokoa suuri CentOS 6.6 Laske solmujen Azure-pohjaiset HPC Pack-klusterin. Voit mukauttaa tätä tiedostoa tarvittaessa ympäristössä.


* **NAMD ohjelmistojen ja opetusohjelma tiedostot** - ohjelmiston lataaminen NAMD Linux [NAMD](http://www.ks.uiuc.edu/Research/namd/) -sivustosta (rekisteröinti tarvitaan). Tässä artikkelissa perustuu NAMD versio 2.10 ja käyttää [Linux-x86_64 (64-bittinen Intel tai AMD Ethernet kanssa)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) -arkisto. Lataa myös [NAMD opetusohjelman tiedostot](http://www.ks.uiuc.edu/Training/Tutorials/#namd). Ladattavat .tar tiedostot ovat, ja sinun on Windows-työkalun Pura klusterin pään solmun tiedostot. Voit purkaa tiedostot, noudata tämän artikkelin ohjeita. 

* **VMD** (valinnainen) - NAMD työtäsi tulokset Lataa ja asenna molekyyli visualisoinnin ohjelma [VMD](http://www.ks.uiuc.edu/Research/vmd/) valittua tietokoneessa. Nykyistä versiota ei 1.9.2. Katso VMD lataussivusto avulla pääset alkuun.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Laske solmujen välillä keskinäisen luottamuksen määrittäminen
Rajat-solmu työn useita Linux solmuissa edellyttää solmut luotetaanko toisiinsa (tekijä: **rsh** tai **ssh**). Kun luot HPC Pack-klusterin Microsoft HPC Pack IaaS käyttöönoton komentosarja, komentosarja määrittää automaattisesti pysyvä keskinäisen luottamuksen, voit määrittää järjestelmänvalvojan tilille. Järjestelmänvalvoja käyttäjille, voit luoda klusterin toimialueen on väliaikainen keskinäisen luottamuksen kesken solmut määrittäminen työ jaetaan niille. Tuhoa yhteyden sitten, kun työ on valmis. Voit tehdä tämän kullekin käyttäjälle annettava RSA-avainparin, joka muodostaa luottamussuhde käyttää HPC Pack klusterin. Noudata näyttöön tulevia ohjeita.

### <a name="generate-an-rsa-key-pair"></a>Luo avaimen RSA-pari
On helppo luoda RSA avaimen pari, jossa on julkinen avain ja yksityinen avain suorittamalla Linux **ssh-keygen** -komennon.

1.  Kirjaudu Linux tietokoneeseen.

2.  Suorita seuraava komento:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Paina **Enter** , jos haluat käyttää oletusasetuksia, ennen kuin komento on valmis. Kirjoita salasana tähän. Kun ohjelma kysyy salasanaa, paina **Enter**-näppäintä.

    ![Luo avaimen RSA-pari][keygen]

3.  Muuta kansion ~/.ssh-kansioon. Yksityinen avain tallennetaan id_rsa ja id_rsa.pub julkinen avain.

    ![Yksityisten ja julkisten näppäimet][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Lisää avaimen pari HPC Pack-klusterin
1.  [Muodosta yhteys etätyöpöydän](virtual-machines-windows-connect-logon.md) pään solmu AM käytä toimialuetta tunnistetiedot edellyttäen kun klusterin (esimerkiksi hpc\clusteradmin) käyttöön. Voit hallita klusterin pään solmu.

2. Windows Server tavanomaisia avulla voit luoda toimialueen käyttäjätili klusterin Active Directory-toimialueen. Käytä esimerkiksi pää solmu Active Directory-käyttäjän ja tietokoneet-työkalu. Tämän artikkelin Esimerkeissä oletetaan, että luot toimialuekäyttäjä nimeltä hpcuser hpclab toimialueen (hpclab\hpcuser).

3. Lisää toimialueen käyttäjänä HPC Pack-klusterin klusterin käyttäjänä. Katso ohjeet [lisääminen tai poistaminen klusterin käyttäjät](https://technet.microsoft.com/library/ff919330.aspx).

2.  Luo tiedosto nimeltä C:\cred.xml ja kopioi se RSA tärkeimmät tiedot. Esimerkki löytyvät mallitiedostojen on tämän artikkelin lopussa.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Avaa komentorivi-ikkuna ja kirjoita seuraava komento hpclab\hpcuser tilin tunnistetiedot-tietojen määrittäminen. **Extendeddata** -parametrin avulla voit välittää C:\cred.xml luomasi tiedosto avaimen tietojen nimi.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Tämä komento on suoritettu onnistuneesti ilman tulos. Määritettyäsi käyttäjätilejä, sinun on suoritettava työt tunnistetietojen cred.xml-tiedosto tallennetaan turvalliseen paikkaan tai poista se.

5.  Jos jokin Linux-solmut luonut RSA avaimen pari, muista poistaa näppäimet, kun olet käyttänyt niitä. HPC Pack ole määritetty keskinäisen luottamuksen jos se havaitsee aiemmin luotuun id_rsa tiedostoon tai id_rsa.pub tiedosto.

>[AZURE.IMPORTANT] Ei ole suositeltavaa suoritetaan Linux työn klusterin järjestelmänvalvojana jaetun klusterissa, koska järjestelmänvalvoja esittämän työn suoritetaan pääkansion tilissä Linux-solmuissa. Järjestelmänvalvoja käyttäjän esittämän työn suoritetaan paikallisen Linux käyttäjätilin saman niminen projektin käyttäjänä. Tässä tapauksessa HPC Pack määrittää keskinäisen luottamuksen Linux kyseisen käyttäjän kaikki varattu työ solmut yli. Voit määrittää Linux käyttäjän manuaalisesti Linux-solmut ennen työn tai HPC Pack Luo käyttäjän automaattisesti työn lähetettäessä. Jos HPC Pack Luo käyttäjän, HPC Pack poistaa projektin päätyttyä. Suojauksen uhkien pienentämiseksi näppäimet poistetaan sen jälkeen, kun työ on valmis solmuissa.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Jaetun tiedoston Linux solmujen määrittäminen

Nyt SMB-tiedostoresurssin määrittäminen ja jaetun kansion käyttöön kaikissa Linux solmuissa Linux solmuissa NAMD tiedostojen yleisiä polulla. Seuraavat ovat liittämään jaetun kansion pään solmun vaiheet. Jaettuun suositellaan jaot esimerkiksi CentOS 6.6, joka ei tue tiedoston Azure-palvelu. Jos Linux-solmut tukevat Azure-tiedostoresurssin, katso, [miten voit käyttää Azure tiedostojen tallentamisesta ja Linux](../storage/storage-how-to-use-files-linux.md). Lisätietoja tiedostojen jakamisasetukset HPC Pack- [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Luo kansio pään solmu ja jaa se kaikille määrittämällä luku-/ kirjoitusoikeudet. Tässä esimerkissä \\ \\CentOS66HN\Namd on kansio, jossa CentOS66HN on isäntänimi pään solmun nimi.

2. Luo alikansion namd2 jaettuun kansioon. Luo toinen alikansion namdsample namd2.

3. Pura käyttämällä Windows-version **tar** tai toisesta Windows-apuohjelma, joka toimii .tar arkistot NAMD tiedostot-kansiossa. 
    * Pura NAMD tar arkisto, \\ \\CentOS66HN\Namd\namd2.
    
    * Pura opetusohjelman tiedostot-kohdassa \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Avaa Windows PowerShell-ikkuna ja suorittamalla seuraavat komennot liittämään Linux solmuissa jaettuun kansioon.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Ensimmäisen komennon luo kansion nimeltä /namd2 kaikissa solmuissa LinuxNodes-ryhmässä. Toinen komento liittää jaettu kansio-//CentOS66HN/Namd/namd2 dir_mode kansion päälle ja file_mode bittien arvo 777. Käyttäjätiedot pään solmun käyttäjän on oltava *käyttäjänimi* ja *salasana* -komennon.

>[AZURE.NOTE]"\`" Toisen komennon symboli on PowerShell ESC-symboli. "\`," "," (pilkku) tarkoittaa komento kuuluu.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Suorita NAMD työn Bash komentosarjan luominen

NAMD työ on *nodelist* tiedoston **charmrun** määrittämään käyttämään NAMD prosessit käynnistettäessä solmujen määrän. Voit käyttää Bash komentosarjan, joka luo nodelist-tiedoston ja suorittaa **charmrun** nodelist tälle tiedostolle. Kun lähetät sitten NAMD työn HPC klusterin hallinta, joka kutsuu komentosarjaa.

Käytä valittua tekstieditorissa, luoda Bash komentosarjan /namd2-kansio, jossa on NAMD ohjelmatiedostot ja anna sille nimi hpccharmrun.sh. Nopea siitä, joka kertoo Kopioi esimerkki hpccharmrun.sh komentosarja on tämän artikkelin lopussa ja valitse [Lähetä NAMD työn](#submit-a-namd-job).

>[AZURE.TIP] Tallenna tekstitiedostona kanssa Linux komentosarjan rivi päätteet (vain LF, CR LF). Näin varmistat, että se toimii oikein Linux-solmuissa.

Seuraavassa on tietoja bash komentosarjaa mitä. 

1.  Määritä joitakin muuttujat.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Pyydä ympäristömuuttujat solmu tiedot. $NODESCORES tallentaa Jaa sanoja $CCP_NODES_CORES luettelo. $COUNT on $NODESCORES kokoa.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    $CCP_NODES_CORES muuttujan muoto on seuraavanlainen:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Tämä muuttuja on lueteltu kokonaismäärän solmujen solmu nimet ja sydämiä kunkin solmun, joka on kohdistettu työn määrä. Esimerkiksi jos työ on 10 sydämiä suorittamiseen, $CCP_NODES_CORES arvo on samanlainen:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Jos $CCP_NODES_CORES muuttujan ei määritetä, aloita **charmrun** suoraan. (Tämä vain suoritetaan, kun suoritat tämän komentosarjan suoraan Linux-solmut.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Tai luo nodelist **charmrun**varten.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Suorita **charmrun** nodelist-tiedoston, palaa sen tilan ja poista nodelist tiedoston lopussa.

    ${CCP_NUMCPUS} on toisen pään HPC Pack-solmu määrittää ympäristömuuttuja. Tallennetaan yhteensä sydämiä kohdistettu työn määrä. Käytämme Määritä charmrun prosessien määrä.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Sulje **charmrun** Palauta tila.

    ```
    exit ${RTNSTS}
    ```



Seuraavassa on tietoja nodelist-tiedosto, joka luo komentosarja:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Esimerkki:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Lähetä NAMD työ

Nyt olet valmis lähettämään NAMD työn HPC klusterin hallinta.

1.  Muodostaa yhteyttä klusterin pää-solmu ja Käynnistä HPC klusterin hallinta.

2.  **Resurssienhallinta**varmistaa, että Linux Laske solmut ovat **Online** -tilassa. Jos ne eivät ole, valitsemalla ne ja valitse **Ota käyttöön**.

2.  Valitse **Töiden hallinta**-kohdassa **Uusi projekti**.

3.  Kirjoita nimi työlle, kuten *hpccharmrun*.

    ![Uusi HPC työ][namd_job]

4.  Valitse **Projektin tiedot** -sivulla **Työn resursseja**Valitse resurssin lajin **solmu** ja **Pienin** arvoksi 3. -työ suoritetaan on kolme Linux solmujen ja kunkin solmun on neljä sydämiä.

    ![Projektin resurssit][job_resources]

5. Valitse **Muokkaa tehtävät** vasemmassa siirtymisruudussa ja valitse sitten **Lisää** voit lisätä tehtävän työn.    


6. Valitse **Tehtävän tiedot ja i/o uudelleenohjauksen** -sivulla Määritä seuraavat arvot:

    * **Komentorivi** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Edellä olevan komentorivin on yksi komento ilman rivinvaihtoja. Se rivittää näkyvän useita rivejä, valitse **komentoriviltä käsin**.

    * **Toimi hakemisto** - /namd2

    * **Pienin** - 3

    ![Tehtävän tiedot][task_details]

    >[AZURE.NOTE] Voit määrittää käytön hakemiston tähän koska **charmrun** yrittää Siirry kunkin solmun käsitteleminen samassa kansiossa. Jos toimimasta-kansio ei ole määritetty, HPC Pack aloittaa komento satunnaisesti nimetyn kansio, joka on luotu jokin Linux-solmut. Tämä aiheuttaa muissa solmuissa seuraava virhe: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` voit välttää tämän ongelman, Määritä kansiopolku, joka voi käyttää kaikkia solmut käytön hakemiston.

5.  Valitse **OK** ja valitse sitten **Lähetä** suorittaa työn.

    Oletusarvon mukaan HPC Pack lähettää työn nykyinen kirjautunut käyttäjä-tiliksi. Valintaikkuna voi kehottaa kirjoittamaan käyttäjänimen ja salasanan, kun olet valinnut **Lähetä**.

    ![Työn tunnistetiedot][creds]

    Tietyissä tilanteissa HPC Pack muistaa käyttäjätiedot ennen Lisää ja ei näy tässä valintaikkunassa. Jos haluat tehdä HPC-paketti, Näytä se uudelleen, kirjoita komentoriville seuraava komento ja Lähetä työ.

    ```command
    hpccred delcreds
    ```

6.  Työn kestää useita minuutteja valmis.

7.  Etsi osoitteessa työn-loki \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log ja tulostus-tiedostojen \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Voit myös aloittaa VMD voit tarkastella projektin tuloksia. Kesäolympialaisten visualisointi NAMD vaiheet tulosteen (Kirjoita tässä tapauksessa ubiquitin proteiinin molekyylin-veden pallo) tiedostot ovat tämän artikkelin piiriin. Katso lisätietoja [NAMD opetusohjelma](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Työn tulokset][vmd_view]

## <a name="sample-files"></a>Mallitiedostojen

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Esimerkki XML-määritystiedoston PowerShell-komentosarjaa klusterin käyttöönotossa

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>    
```

### <a name="sample-credxml-file"></a>Cred.xml mallitiedosto

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```

### <a name="sample-hpccharmrunsh-script"></a>Esimerkki hpccharmrun.sh komentosarja

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
