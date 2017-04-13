<properties
 pageTitle="Suorita OpenFOAM HPC Pack Linux VMs | Microsoft Azure"
 description="Ottaa käyttöön Microsoft HPC Pack-klusterin Azure- ja suorita OpenFOAM työn useita Linux Laske solmuissa RDMA verkossa."
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
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Suorita OpenFoam Microsoft HPC Pack Linux RDMA klusterissa Azure-tietokannassa

Tämän artikkelin avulla voit suorittaa OpenFoam Azuren näennäiskoneiden. Tässä kohdassa voit ottaa käyttöön Microsoft HPC Pack klusterin Linux Laske solmujen kanssa Azure ja suorita [OpenFoam](http://openfoam.com/) projektin Intel MPI kanssa. Voit käyttää RDMA-yhteyttä hyödyntäviin Azure VMs Laske-solmujen niin, että Laske solmut viestiä Azure RDMA verkon kautta. Muut asetukset, voit suorittaa OpenFoam Azure kuvia täysin määritetyssä kaupallisen käytettävissä Marketplace, kuten UberCloud's [OpenFoam 2.3 CentOS 6: n](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)ja suorittamalla [Azure](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/)erän. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (Avaa kenttä-toiminto ja käsittelyä varten) on Avaa lähde laskennallinen dynamics (CFD), neste-ohjelmistopaketin, jota käytetään laajasti tekniikka ja tiede kaupallisen sekä academic organisaatioissa. Se sisältää työkaluja meshing, erityisesti snappyHexMesh parallelized mesher monimutkaisia CAD-geometries ja vanhat ja uudet käsittely. Lähes kaikki prosessit suorittaminen rinnakkain, jotta käyttäjät voivat hyödyntää tietokonelaitteiston käytettävissään.  

Microsoft HPC Pack on suurissa HPC ja rinnakkaisia sovellusten, kuten MPI-sovelluksia Microsoft Azuren näennäiskoneiden klustereiden toimintoja. HPC Pack tukee myös käynnissä Linux HPC sovellusten Linux Laske VMs käyttöön HPC Pack-klusterin solmu. Katso esittely Linux Laske solmujen käyttäminen HPC Pack [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md) .

>[AZURE.NOTE] Tässä artikkelissa kuvataan, miten suorittamiseen ja HPC Pack Linux MPI työmäärää. Se oletetaan, että jotkin kannattaa Linux järjestelmänvalvontaan ja MPI työmääriä käytössä Linux klustereiden. Jos käytät MPI ja OpenFOAM eroaa suorittaa tässä artikkelissa olevia versioita, voit joutua muuttamaan asennuksesta ja määrityksestä muutama. 

## <a name="prerequisites"></a>Edellytykset

*   **HPC Pack klusteri RDMA-yhteyttä hyödyntäviin Linux ja Laske solmut** - käyttöönotto HPC Pack-klusterin koon A8, A9, H16r tai H16rm Linux Laske solmujen käyttöön [Azure Resurssienhallinta mallia](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) tai [Azure PowerShell-komentosarjaa](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md). Katso edellytykset ja vaiheet kummassakin [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md) . Jos valitset PowerShell-komentosarjojen käyttöönottoa, katso määritysten mallitiedostossa mallitiedostojen on tämän artikkelin lopussa olevassa kohdassa. Tässä määrityksessä käyttöön pään koon A8 Windows Server 2012 R2-solmu ja 2 kokoa A8 SUSE Linux Enterprise Server 12 Laske solmujen Azure-pohjaiset HPC Pack-klusterin. Korvaa tilauksen ja palvelun nimet haluamasi arvot. 

    **Lisää tärkeää tietoa**

    *   Katso Linux RDMA verkko edellytykset Azure-tietokannassa, [H-sarjan ja Laske paljon A sarjan VMs tietoja](virtual-machines-windows-a8-a9-a10-a11-specs.md).

    *   Jos käytät Powershell-komentosarjojen käyttöönottoa, ota käyttöön kaikki Linux Laske sijaitsevien solmujen yhden pilvipalvelussa käyttämään RDMA verkkoyhteyden tilan.

    *   Jälkeen käyttöönotto Linux solmujen yhdistäminen SSH muita hallinnollisten tehtävien suorittamiseen. Etsi kunkin Linux AM SSH yhteystiedot Azure-portaalissa.  
        
*   **Intel MPI** - voidaan suorittaa OpenFOAM SLES 12 HPC Laske Azure solmujen, sinun on asennettava Intel MPI kirjaston 5 runtime [Intel.com sivuston](https://software.intel.com/en-us/intel-mpi-library/). (Intel MPI 5 on esiasennettu CentOS perustuva HPC kuvista.)  Myöhemmin Valitse tarvittaessa asentaa Intel MPI Linux Laske-solmut. Valmisteleminen tässä vaiheessa, kun olet rekisteröinyt Intel kanssa, seuraamalla linkkiä vahvistusviestin liittyvät web-sivulle. Kopioi sitten Intel MPI oikean version .tgz-tiedoston lataaminen-linkkiä. Tässä artikkelissa perustuu Intel MPI versio 5.0.3.048.

*   **OpenFOAM lähde Pack** - Linux [OpenFOAM Foundation-sivuston](http://openfoam.org/download/2-3-1-source/)OpenFOAM lähde Pack-ohjelmiston lataaminen. Tässä artikkelissa perustuu lähde Pack-versio on ladattavissa 2.3.1 OpenFOAM 2.3.1.tgz. Noudata tämän artikkelin purkaminen ja kääntää OpenFOAM Linux Laske solmuissa.

*   **EnSight** (valinnainen) - hakutuloksia OpenFOAM simuloinnissa, lataa ja asenna [EnSight](https://www.ceisoftware.com/download/) visualisointi ja analysointi-ohjelman. Käyttöoikeudet ja lataa tiedot ovat EnSight-sivustossa.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Laske solmujen välillä keskinäisen luottamuksen määrittäminen

Rajat-solmu työn useita Linux solmuissa edellyttää solmut luotetaanko toisiinsa (tekijä: **rsh** tai **ssh**). Kun luot HPC Pack-klusterin Microsoft HPC Pack IaaS käyttöönoton komentosarja, komentosarja määrittää automaattisesti pysyvä keskinäisen luottamuksen, voit määrittää järjestelmänvalvojan tilille. Järjestelmänvalvoja käyttäjille, voit luoda klusterin toimialueen on määritettävä tilapäinen keskinäisen luottamuksen kesken solmut jaettaessa työn niihin ja tuhoa suhteen, kun työ on valmis. Muodostaa luota kullekin käyttäjälle on RSA avaimen pari klusterin, jota HPC Pack käyttää luottamussuhde.

### <a name="generate-an-rsa-key-pair"></a>Luo avaimen RSA-pari

On helppo luoda RSA avaimen pari, jossa on julkinen avain ja yksityinen avain suorittamalla Linux **ssh-keygen** -komennon.

1.  Kirjaudu Linux tietokoneeseen.

2.  Suorita seuraava komento:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Paina **Enter** , jos haluat käyttää oletusasetuksia, ennen kuin komento on valmis. Kirjoita salasana tähän. Kun ohjelma kysyy salasanaa, paina **Enter**-näppäintä.

    ![Luo avaimen RSA-pari][keygen]

3.  Muuta kansion ~/.ssh-kansioon. Yksityinen avain tallennetaan id_rsa ja id_rsa.pub julkinen avain.

    ![Yksityisten ja julkisten näppäimet][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Lisää avaimen pari HPC Pack-klusterin
1.  Varmista Etätyöpöytäyhteys oman pään solmun HPC Pack järjestelmänvalvojan oikeuksin (Voit määrittää, kun olet suorittanut käyttöönoton komentosarja järjestelmänvalvojatili).

2. Windows Server tavanomaisia avulla voit luoda toimialueen käyttäjätili klusterin Active Directory-toimialueen. Käytä esimerkiksi pää solmu Active Directory-käyttäjän ja tietokoneet-työkalu. Tämän artikkelin Esimerkeissä oletetaan, että luot nimeltä hpclab\hpcuser toimialuekäyttäjä.

3.  Luo tiedosto nimeltä C:\cred.xml ja kopioi se RSA tärkeimmät tiedot. Cred.xml mallitiedosto on tämän artikkelin lopussa.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Avaa komentorivi-ikkuna ja kirjoita seuraava komento hpclab\hpcuser tilin tunnistetiedot-tietojen määrittäminen. **Extendeddata** -parametrin avulla voit välittää C:\cred.xml luomasi tiedosto avaimen tietojen nimi.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    Tämä komento on suoritettu onnistuneesti ilman tulos. Määritettyäsi käyttäjätilejä, sinun on suoritettava työt tunnistetietojen cred.xml-tiedosto tallennetaan turvalliseen paikkaan tai poista se.

5.  Jos jokin Linux-solmut luonut RSA avaimen pari, muista poistaa näppäimet, kun olet käyttänyt niitä. Jos HPC Pack löytää aiemmin luotuun id_rsa tiedostoon tai id_rsa.pub tiedoston, se ole määritetty keskinäisen luottamuksen.

>[AZURE.IMPORTANT] Ei ole suositeltavaa suoritetaan Linux työn klusterin järjestelmänvalvojana jaetun klusterissa, koska järjestelmänvalvoja esittämän työn suoritetaan pääkansion tilissä Linux-solmuissa. Kuitenkin järjestelmänvalvoja käyttäjän esittämän työn suoritetaan paikallisen Linux käyttäjätilin saman niminen projektin käyttäjänä. Tässä tapauksessa HPC Pack määrittää keskinäisen luottamuksen Linux kyseisen käyttäjän varattu työ solmujen välillä. Voit määrittää Linux käyttäjän manuaalisesti Linux-solmut ennen työn tai HPC Pack Luo käyttäjän automaattisesti työn lähetettäessä. Jos HPC Pack Luo käyttäjän, HPC Pack poistaa projektin päätyttyä. Voit pienentää tietoturvauhkia HPC Pack poistaa näppäimet projektin valmistumisen jälkeen.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Jaetun tiedoston Linux solmujen määrittäminen

Määritä vakio SMB jaetun kansion pään solmun nyt. Anna sovelluksen tiedostoja yleisiä polulla Linux-solmut, liittää jaetun kansion Linux-solmut. Jos haluat, voit käyttää toiseen tiedostojen jakamisen vaihtoehto, kuten Azure-tiedostot-Jaa - suositus skenaarioita - tai NFS-Jaa. Katso lisätietoja ja yksityiskohtaisia ohjeita [aloittaminen Linux Laske solmujen HPC Pack-klusterin Azure-tietokannassa](virtual-machines-linux-classic-hpcpack-cluster.md)tiedostonjakoa.

1.  Luo kansio pään solmu ja jaa se kaikille määrittämällä luku-/ kirjoitusoikeudet. Esimerkiksi jakaa C:\OpenFOAM pään solmu \\ \\SUSE12RDMA HN\OpenFOAM. *SUSE12RDMA HN* näin pään solmu isäntänimi.

2.  Avaa Windows PowerShell-ikkuna ja suorittamalla seuraavat komennot:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Ensimmäisen komennon luo kansion nimeltä /openfoam kaikissa solmuissa LinuxNodes-ryhmässä. Toinen komento liittää jaetun kansion //SUSE12RDMA-HN/OpenFOAM dir_mode Linux solmujen- ja file_mode bittien arvo 777. Käyttäjätiedot pään solmun käyttäjän on oltava *käyttäjänimi* ja *salasana* -komennon.

>[AZURE.NOTE]"\`" Toisen komennon symboli on PowerShell ESC-symboli. "\`," "," (pilkku) tarkoittaa komento kuuluu.

## <a name="install-mpi-and-openfoam"></a>Asenna MPI ja OpenFOAM

Voit suorittaa OpenFOAM MPI työn RDMA verkossa, haluat kääntää OpenFOAM Intel MPI kirjastojen käytöstä. 

Suorittaa useita **clusrun** komennot asentaminen Intel MPI kirjastojen (Jos ei ole jo asennettu) ja OpenFOAM ensin Linux-solmut. Käytä Jaa asennustiedostot Linux solmujen määritetty aiemmin pään solmu Jaa.

>[AZURE.IMPORTANT]Nämä asennuksen ja käännetään vaiheet ovat esimerkkejä. Sinun on joitakin tuntemus Linux järjestelmänvalvontaan, varmista, että riippuvaiset kääntäjät ja kirjastojen on asennettu oikein. Voit joutua tiettyjä ympäristömuuttujia tai muita asetuksia käyttämiesi Intel MPI ja OpenFOAM versioihin. Lisätietoja on artikkelissa [Linux oppaaseen Intel MPI kirjasto](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) ja [OpenFOAM lähde-paketti, asennus](http://openfoam.org/download/2-3-1-source/) -ympäristössä.


### <a name="install-intel-mpi"></a>Asenna Intel MPI

Tallentaa ladatut asennuspaketin Intel MPI (Tässä esimerkissä l_mpi_p_5.0.3.048.tgz)-C:\OpenFoam pään solmun niin, että Linux-solmut voit käyttää tätä tiedostoa /openfoam. Suorita **clusrun** Intel MPI kirjaston asennetaan Linux solmujen.

1.  Seuraavat komennot kopioi asennuspaketin ja Pura se /opt/intel kunkin solmun.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Asenna Intel MPI kirjaston äänettömästi, käytä silent.cfg-tiedostoa. Esimerkki löytyvät mallitiedostojen on tämän artikkelin lopussa. Lisää tämän tiedoston jaetun kansion /openfoam. Lisätietoja silent.cfg-tiedosto on artikkelissa [Linux asennuksen Guide - hiljainen asennus Intel MPI kirjasto](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Varmista, että tallennat tekstitiedosto, jossa Linux silent.cfg lähettää tiedoston rivi päätteet (vain LF, CR LF). Tällä vaiheella varmistetaan, että se toimii oikein Linux-solmuissa.

3.  Asenna Intel MPI kirjaston määritystoimintoa.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>Määritä MPI

Testaamiseen seuraavat rivit kannattaa lisätä kunkin Linux-solmut /etc/security/limits.conf:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Käynnistä Linux-solmut, kun limits.conf-tiedoston päivittämistä. Käytä esimerkiksi **clusrun** seuraava komento:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Uudelleenkäynnistämisen jälkeen varmistaa, että jaettu kansio on otettu käyttöön /openfoam nimellä.

### <a name="compile-and-install-openfoam"></a>Käännä ja asenna OpenFOAM

Tallenna OpenFOAM lähde pakettiin (OpenFOAM-2.3.1.tgz tässä esimerkissä), C:\OpenFoam ladatun asennuspaketin pään solmun, jotta Linux-solmut voit käyttää tätä tiedostoa /openfoam. Suorita **clusrun** -komennot, joilla keräämällä OpenFOAM Linux solmuissa.


1.  Luo kansio /opt/OpenFOAM kunkin Linux-solmun, kopioi tähän kansioon lähdepaketti ja Pura se.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Voit kääntää OpenFOAM Intel MPI-kirjaston kanssa, Määritä joitakin ympäristömuuttujat Intel MPI ja OpenFOAM. Muuttujien kutsutaan settings.sh bash komentosarjan avulla. Esimerkki löytyvät mallitiedostojen on tämän artikkelin lopussa. Aseta jaetun kansion /openfoam tämän tiedoston (tallennettu Linux rivin päätteet). Tämä tiedosto sisältää myös, jota käytetään myöhemmin OpenFOAM suoritetaan MPI ja OpenFOAM CRT Runtime-asetukset.

3. Asenna tarvittavat kääntää OpenFOAM riippuvaiset paketit. Sen mukaan, Linux-jakauman ensin joutua Lisää säilö. Suorita **clusrun** komennot seuraavankaltaiselta:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Valitse tarvittaessa SSH kunkin Linux-solmun voit suorittaa komennot vahvistamaan, että ne suoritetaan oikein.

4.  Suorita seuraava komento kääntää OpenFOAM. Kääntämisen prosessi kestää kauan ja luo suuria määriä vakio tulosteen lokitiedot, niin limitetty tulos näyttää **/ limitetty** -vaihtoehdon avulla.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]"\`"-Komennon symboli on PowerShell ESC-symboli. "\`&" tarkoittaa, "&" on osa komento.

## <a name="prepare-to-run-an-openfoam-job"></a>Valmistele OpenFOAM työ

Nyt voit aloittaa kutsua sloshingTank3D, joka on yksi OpenFoam kaksi Linux solmujen MPI-työ suoritetaan. 

### <a name="set-up-the-runtime-environment"></a>Runtime-ympäristön määrittäminen

Määritetään runtime-ympäristöissä MPI ja OpenFOAM Linux solmuissa, suorita seuraava komento Windows PowerShell-ikkunassa pään solmun. (Tämä komento on kelvollinen SUSE Linux ainoastaan.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Mallitietojen valmisteleminen

Käyttää määritit aiemmin voit jakaa tiedostoja (liitetty kuin /openfoam) Linux solmujen pään solmu Jaa.

1.  SSH johonkin oman Linux Laske solmujen.

2.  Suorita seuraava komento määrittämään OpenFOAM runtime-ympäristössä, jos et ole vielä tehnyt tämän.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Kopioi sloshingTank3D otosten jaettuun kansioon ja sen Siirry.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Tässä esimerkissä oletusarvon parametreja käytettäessä lähimpään kymmeneen minuuttia, jos haluat suorittaa, voi kestää niin voit muokata sen nopeuttaa joitakin parametreja. Yksinkertainen yksi vaihtoehto on aikaa vaiheen muuttujat deltaT ja writeInterval järjestelmän/controlDict tiedoston muokkaamiseen. Tähän tiedostoon tallennetaan kaikki liittyvät aika ja lukemiseen ja kirjoittamiseen ratkaisun tietojen hallinnan syöttötiedot. Voit esimerkiksi muuttaa deltaT-0,05 0.5 ja 0.5 writeInterval 0,05-arvon.

    ![Muokkaa vaihe muuttujat][step_variables]

5.  Määritä haluamasi arvot muuttujat järjestelmän/decomposeParDict-tiedostossa. Tässä esimerkissä käytetään kahta Linux solmujen kunkin 8 sydämiä, joten numberOfSubdomains arvoksi 16 ja hierarchicalCoeffs n (1 1 16), mikä tarkoittaa suorittaa OpenFOAM rinnakkain 16 prosessien kanssa. Lisätietoja on artikkelissa [OpenFOAM käyttöoppaassa: 3,4 suorittaminen sovellusten rinnakkain](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Hajota prosessit][decompose]

6.  Mallitietojen valmisteleminen sloshingTank3D-hakemistosta suorittamalla seuraavat komennot.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Pää-solmu pitäisi näkyä mallidatatiedostot kopioidaan C:\OpenFoam\sloshingTank3D. (C:\OpenFoam on pään solmu sijaitsevaan jaettuun kansioon).

    ![Datatiedostot pään solmun][data_files]

### <a name="host-file-for-mpirun"></a>Host (isäntä)-tiedoston mpirun varten

Tässä vaiheessa voit luoda joka **mpirun** -komento käyttää Host (isäntä)-tiedosto (Laske solmujen luettelo).

1.  Johonkin Linux-solmut Luo tiedosto nimeltä hostfile /openfoam, valitse niin, että tämä tiedosto tavoittaa /openfoam/hostfile kaikissa Linux solmuissa.

2.  Kirjoita tiedoston Linux-solmu nimet. Tässä esimerkissä tiedosto sisältää seuraavat nimet:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Voit luoda tämän tiedoston etsiminen C:\OpenFoam\hostfile pään solmun. Jos valitset tämän vaihtoehdon, tallenna se tekstitiedostona kanssa Linux rivi päätteet (vain LF, CR LF). Näin varmistat, että se toimii oikein Linux-solmuissa.

    **Bash komentosarja-paketti**

    Jos sinulla on useita Linux solmujen etkä halua työtäsi voidaan suorittaa vain osan, se ei ole kannattaa käyttää kiinteä Host (isäntä)-tiedostoja sen jälkeen, koska et tiedä solmut kohdistetaan työtäsi. Kirjoita tässä tapauksessa bash komentosarjan paketti, **mpirun** Host (isäntä)-tiedoston luominen automaattisesti. Voit etsiä esimerkki bash komentosarjan paketti nimeltä hpcimpirun.sh on tämän artikkelin lopussa olevassa ja Tallenna /openfoam/hpcimpirun.sh. Tämä esimerkkikomentosarja tekee seuraavat toimet:

    1.  Määrittää ympäristömuuttujat **mpirun**ja jotkin lisäys-komennon parametrit MPI työn suorittamiseen RDMA verkon kautta. Tässä tapauksessa asettaa seuraavat muuttujat:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = rajaaminen v2-ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Luo mukaan ympäristön Host (isäntä)-tiedoston muuttujan $CCP_NODES_CORES, joka määrittää HPC pään solmu kun työ on otettu käyttöön.

        Muodossa $CCP_NODES_CORES seuraa tätä mallia:

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        Jos

        * `<Number of nodes>`-varattu työ solmujen määrän.  
        
        * `<Name of node_n_...>`-kunkin solmun varattu työ nimi.
        
        * `<Cores of node_n_...>`-Sydämiä solmun kohdistettu työn määrä.

        Esimerkiksi jos työn täytyy suorittaa kaksi solmujen, $CCP_NODES_CORES muistuttaa
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Soittaa **mpirun** -komento ja liittää kaksi parametria komentorivillä.

        * `--hostfile <hostfilepath>: <hostfilepath>`-komentosarja luo Host (isäntä)-tiedoston polku

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-ympäristömuuttuja määrittää HPC Pack pään solmu, joka sisältää yhteensä sydämiä kohdistettu työn määrä. Tässä tapauksessa määrittää **mpirun**prosessien määrä.


## <a name="submit-an-openfoam-job"></a>Lähetä OpenFOAM työ

Nyt voit lähettää työn HPC klusterin hallinta. Sinun täytyy Välitä komentosarjan hpcimpirun.sh komento rivit joitakin projektitehtäviä.

1. Muodostaa yhteyttä klusterin pää-solmu ja Käynnistä HPC klusterin hallinta.

2. **Resurssienhallinta**, varmista, että Linux Laske solmut ovat **Online** -tilassa. Jos ne eivät ole, valitsemalla ne ja valitse **Ota käyttöön**.

3.  Valitse **Töiden hallinta**-kohdassa **Uusi projekti**.

4.  Kirjoita nimi työlle, kuten _sloshingTank3D_.

    ![Projektin tiedot][job_details]

5.  **Työn resursseja**Valitse "Solmu" resurssin lajin ja määritä vähintään 2. Tässä määrityksessä suoritetaan työn kaksi Linux-solmut, joista jokaisella on kahdeksan sydämiä tässä esimerkissä.

    ![Projektin resurssit][job_resources]

6. Valitse **Muokkaa tehtävät** vasemmassa siirtymisruudussa ja valitse sitten **Lisää** voit lisätä tehtävän työn. Neljä tehtävien lisääminen projektiin seuraava komento rivit ja asetukset.

    >[AZURE.NOTE]Käynnissä `source /openfoam/settings.sh` määrittää OpenFOAM ja MPI runtime-ympäristössä, jotta kunkin seuraavia tehtäviä se soittaa ennen OpenFOAM-komentoa.

    *   **Tehtävä 1**. Suorita **decomposePar** luomiseen datatiedostojen **interDyMFoam** suorittaminen rinnakkain.
    
        *   Yhden solmun varaaminen tehtävään

        *   **Komentorivi** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Toimi hakemisto** - / openfoam/sloshingTank3D
        
        Katso seuraava kuva. Voit määrittää jäljellä olevien tehtävien vastaavasti.

        ![Tehtävä 1-tiedot][task_details1]

    *   **Tehtävä 2**. Suorita **interDyMFoam** laskemiseen otosten rinnakkain.

        *   Kaksi solmujen varaaminen tehtävään

        *   **Komentorivi** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Toimi hakemisto** - / openfoam/sloshingTank3D

    *   **Tehtävä 3**. Voit yhdistää aika kansioiden kunkin processor_N_ hakemistosta määrittää yhtenäisiä **reconstructPar** suorittaminen

        *   Yhden solmun varaaminen tehtävään

        *   **Komentorivi** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Toimi hakemisto** - / openfoam/sloshingTank3D

    *   **Vaihe 4**. Suorita **foamToEnsight** rinnakkain OpenFOAM tuloksen tiedostojen muuntaminen EnSight muoto ja Aseta EnSight tiedostot hakemiston Ensight palvelupyynnön hakemistossa.

        *   Kaksi solmujen varaaminen tehtävään

        *   **Komentorivi** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Toimi hakemisto** - / openfoam/sloshingTank3D

6.  Lisää riippuvuudet tehtäviin tehtävän nousevasti.

    ![Tehtävien riippuvuudet][task_dependencies]

7.  Valitse Suorita tämä työ **Lähetä** .

    Oletusarvon mukaan HPC Pack lähettää työn nykyinen kirjautunut käyttäjä-tiliksi. Kun olet valinnut **Lähetä**, näet ehkä valintaikkuna, jossa kehotetaan lataamaan käyttäjänimeä ja salasanaa.

    ![Työn tunnistetiedot][creds]

    Tietyissä tilanteissa HPC Pack muistaa käyttäjätiedot ennen Lisää ja ei näy tässä valintaikkunassa. Jos haluat tehdä HPC-paketti, Näytä se uudelleen, kirjoita komentoriville seuraava komento ja Lähetä työ.

    ```
    hpccred delcreds
    ```

8.  Työn kestää lähimpään kymmeneen minuuttiin useita tunteja mukaan otosten määritetyt parametrit. Lämpökartta näet työn Linux-solmut-käyttöjärjestelmässä. 

    ![Lämpökartta][heat_map]

    Valitse kunkin solmun kahdeksan prosessit käynnistetään.

    ![Linux prosessit][linux_processes]

9.  Kun työ on valmis, Etsi työn tulokset C:\OpenFoam\sloshingTank3D ja lokitiedostojen osoitteessa C:\OpenFoam alaisiin kansioihin.


## <a name="view-results-in-ensight"></a>Valitse EnSight tulosten tarkasteleminen

Voit myös käyttää [EnSight](https://www.ceisoftware.com/) OpenFOAM työn tulosten analysointi ja visualisointi. Saat lisätietoja visualisointi ja EnSight animaatio, [videon opas](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Kun olet asentanut EnSight pään solmun, käynnistä se.

2.  Avaa C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Näet säiliö katseluohjelmassa.

    ![EnSight säiliö][tank]

3.  Voit luoda **Isosurface** **internalMesh**ja valitse sitten muuttujan **alpha_water**.

    ![Luo isosurface][isosurface]

4.  Värien määrittäminen **Isosurface_part** edellisessä vaiheessa luotu. Esimerkiksi määrittää sen veden sininen.

    ![Muokkaa isosurface väri][isosurface_color]

5.  Luoda **Iso-asema** **seinien** valitsemalla **seinien** **osat** -ruudussa ja valitse työkalurivin **Isosurfaces** -painiketta.

6.  Valitse-valintaikkunassa valitse **Isovolume** **tyyppi** ja määritä **Isovolume alueen** Min 0,5. Voit luoda isovolume, valitsemalla **Luo ja valittuihin osiin**.

7.  Värien määrittäminen **Iso_volume_part** edellisessä vaiheessa luotu. Esimerkiksi määrittää sen laaja veden sininen.

8.  Määrittää **seinien**värin. Määrittää sen esimerkiksi läpinäkyvä valkoinen.

9. Valitse nyt näet sen tulokset **Toista** .

    ![Säiliö tulos][tank_result]

## <a name="sample-files"></a>Mallitiedostojen

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Esimerkki XML-määritystiedoston PowerShell-komentosarjaa klusterin käyttöönotossa

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
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
### <a name="sample-silentcfg-file-to-install-mpi"></a>Asenna MPI silent.cfg mallitiedosto

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Esimerkki settings.sh komentosarja

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Esimerkki hpcimpirun.sh komentosarja

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
