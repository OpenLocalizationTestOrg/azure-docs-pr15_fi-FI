<properties
 pageTitle="Excel-ja SOA HPC Pack-klusterin | Microsoft Azure"
 description="Aloita suurissa Excelissä ja SOA työmääriä käytössä HPC Pack-klusterin Azure-tietokannassa"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Aloita Excel- ja SOA työmääriä käytössä HPC Pack-klusterin Azure-tietokannassa

Tässä artikkelissa kerrotaan Microsoft HPC Pack-klusterin Azuren näennäiskoneiden käyttöön ottamisesta käyttöön Azure pikaopas-mallin tai vaihtoehtoisesti PowerShellin Azure-käyttöönoton komentosarja. Klusterin käyttää suunniteltu toimimaan Microsoft Excel- tai palvelun aloittaminen arkkitehtuuri (SOA) työmääriä HPC Pack Azure Marketplace AM kuvia. Klusterin avulla voit suorittaa yksinkertaisen Excel HPC ja SOA palveluja paikalliseen asiakastietokoneeseen. Excelin HPC-palvelut ovat purkaminen Excel-työkirjan ja Excelin käyttäjän määrittämien funktioiden tai apuohjelmissa.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Korkean tason seuraavassa kaaviossa näkyy HPC Pack-klusterin, voit luoda.

![HPC klusterin solmujen suorittaminen Excel työmääriä kanssa][scenario]

## <a name="prerequisites"></a>Edellytykset

*   **Asiakastietokone** - tarvitset Windows-pohjaisten asiakastietokone, voit lähettää klusterin otoksen Excelissä ja SOA työt. Sinun on myös Windows-tietokoneessa, suorita PowerShellin Azure klusterin käyttöönoton komentosarja (Jos valitset Tämä käyttöönottomenetelmä) ja

*   **Azure tilauksen** – Jos sinulla ei ole Azure tilauksen voit luoda [vapaa tilin](https://azure.microsoft.com/pricing/free-trial/) muutamaan minuuttiin.

*   **Sydämiä kiintiön** – voit joutua muuttamaan enimmäismäärä on saavutettu sydämiä, etenkin silloin, kun useita klusterisolmujen multicore AM koot käyttöön. Jos käytät Azure pikaopas-mallin, sydämiä kiintiön resurssien hallinta on Azure alueittain. Siinä tapauksessa voit joutua lisäämään tietyn alueen kiintiön. Katso [Azure tilauksen rajoitukset, kiintiöiden ja rajoitukset](../azure-subscription-service-limits.md). Kun haluat lisätä kiintiön, [Avaa asiakkaan online-tukipyynnön](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) maksutta.

*   **Microsoft Office-käyttöoikeus** - Jos otat Laske solmujen Marketplace HPC Pack AM kuvan käyttäminen Microsoft Excel-30 päivän kokeiluversio Microsoft Excelin Professional Plus 2013 on asennettu. Laskenta-ajan kuluttua haluat antaa Microsoft Office-käyttöoikeus, Aktivoi Excelissä voidaan suorittaa toiminnoista. Katso tämän artikkelin [Excel aktivointi](#excel-activation) . 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Vaihe 1. Määritä HPC Pack-klusterin Azure-tietokannassa

Emme esittämään voi määrittää klusterin kahdella eri tavalla: käyttämällä Azure pikaopas-malli ja Azure-portaalin; ensin ja toinen, PowerShellin Azure-käyttöönoton komentosarjan avulla.


### <a name="option-1-use-a-quickstart-template"></a>Vaihtoehto 1. Pikaopas mallin käyttäminen
Azure pikaopas-mallin avulla voit helposti ja nopeasti käyttöön HPC Pack-klusterin Azure-portaalissa. Kun avaat portaalissa mallia, saat yksinkertaisen Käyttöliittymä, johon kirjoitat yhteyttä klusterin asetukset. Seuraavien ohjeiden mukaisesti. 

>[AZURE.TIP]Voit halutessasi käyttää [Azure Marketplace-malli](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , joka luo samanlainen klusterin Excel työmääriä varten. Ohjeita vaihdella hiukan seuraavasti.

1.  Lisätietoja [mallin luominen HPC klusterin GitHub sivulla](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Jos haluat, lue malli ja lähdekoodin.

2.  Valitse **Ota Azure** aloittaaksesi käyttöönoton mallin Azure-portaalissa.

    ![Ota malli käyttöön Azure][github]

3.  Valitse-portaalissa seuraavasti Kirjoita parametrit HPC klusterin mallia.

    a. Valitse **Parametrit** -sivulla tai Muokkaa Malliparametrien arvot. (Valitse ohjeet kunkin asetuksen vieressä olevaa kuvaketta.) Otoksen arvot näkyvät seuraavassa näytössä. Tässä esimerkissä Luo *hpc01* *hpc.local* toimialueen koostuva pään solmu nimeltä klusterin ja 2 Laske solmujen. Laske-solmut luodaan HPC Pack AM-kuva, joka sisältää Microsoft Excelissä.

    ![Parametrien määrittäminen][parameters]

    >[AZURE.NOTE]Pään solmu AM luodaan automaattisesti HPC Pack 2012 R2: n Windows Server 2012 R2- [uusimmat Marketplace-kuva](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) . Tällä hetkellä kuvan perustuu HPC Pack 2012 R2: n päivitys 3.
    >
    >Laske solmu VMs luodaan valitun Laske solmu perheen uusimman kuvasta. Valitse uusimman HPC Pack Laske solmu kuvan, joka sisältää Microsoft Excelin Professional Plus 2013: n kokeiluversio **ComputeNodeWithExcel** -vaihtoehto. Ottaa käyttöön klusterin Yleiset SOA istunnot tai Excelin UDF purkaminen, valitse (ilman Exceliä) **ComputeNode** -vaihtoehto.

    b. Valitse tilaus.

    c-näppäinyhdistelmää. Resurssiryhmä klusterin, kuten *hpc01RG*luominen

    d. Valitse tiedoston sijainti resurssiryhmän, kuten keskitetyn US.

    e. Valitse **ehdot** -sivulla Tarkista ehdot. Jos hyväksyt, valitse **Osta**. Kun olet määrittänyt mallin arvot, valitse **Luo**.

4.  Kun asennus on valmis (yleensä kestää noin 30 minuuttia)-klusterin varmenne-tiedoston vieminen klusterin pään solmu. Myöhemmin voit tuoda tämän julkisten varmenteen asiakastietokoneen palvelinpuolen käyttöoikeuden suojatun HTTP sidontaa varten.

    a. Yhteyden muodostaminen pään solmu mukaan etätyöpöydän Azure-portaalista.

     ![Yhteyden muodostaminen pään solmu][connect]

    b. Käytä tavanomaisia varmenteiden hallinnan pään solmu varmenteen vieminen (sijaitseva varmenteen: \LocalMachine\My) ilman yksityinen avain. Tässä esimerkissä Vie *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Varmenteen vieminen][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Vaihtoehto 2. Käytä HPC Pack IaaS käyttöönoton komentosarja

HPC Pack IaaS käyttöönoton komentosarja kätevästi monipuolisia ottamaan HPC Pack-klusterin. Luomilleen klusterin perinteinen käyttöönotto-mallissa olisi malli käyttää Azure resurssien hallinnan käyttöönottomalli. Komentosarja on myös tilauksen joko Azure yleinen tai Azure kiina-palvelun kanssa yhteensopivat.

**Lisää edellytykset**

* **Azure PowerShell** - [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) (versio 0.8.10 tai uudempi) asiakastietokoneen.

* **HPC Pack IaaS käyttöönoton komentosarja** : Lataa ja Pura komentosarja [Microsoft Download Centeristä](https://www.microsoft.com/download/details.aspx?id=44949)uusimman version. Tarkista suorittamalla komentosarja-version `New-HPCIaaSCluster.ps1 –Version`. Tässä artikkelissa perustuu versio 4.5.0 tai uudempi komentosarja.

**Tiedoston luominen**

 HPC Pack IaaS käyttöönoton komentosarja käyttää XML-määritystiedostoa syötteeksi, joka kuvaa HPC klusterin infrastruktuurin. Ottamaan klusterin pään solmu ja 18 Laske solmujen luotaviin suorittaminen solmu-kuva, joka sisältää Microsoft Excelin korvaa arvot-ympäristössä otoksen määritys-tiedostoon. Saat lisätietoja määritystiedosto komentosarja-kansioon ja [Luo HPC-klusterin HPC Pack IaaS käyttöönoton komentosarjan](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md)Manual.rtf-tiedosto.

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Huomautuksia määritystiedoston**

* **VMName** pään solmun **täytyy** olla sama kuin **palvelun nimi**tai SOA töitä ei onnistu Suorita.

* Varmista, että määrität **EnableWebPortal** niin, että pään solmu varmenteen luodaan ja viedä.

* Tiedosto määrittää jälkeiset määritykset PowerShell-komentosarja, joka suoritetaan pään solmun PostConfig.ps1. Seuraava näyte komentosarja määrittää yhteysmerkkijonoa Azure-tallennustilan, Laske-solmu rooli poistetaan pään solmu ja tuo kaikki solmut, kun ne on otettu käyttöön. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Suorita komentosarja**

1.  Avaa PowerShell-konsolin asiakastietokoneen järjestelmänvalvojana.

2.  Vaihda kansio komentosarjakansiota (Tässä esimerkissä E:\IaaSClusterScript).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Ottaa käyttöön HPC Pack-klusterin varten suorittamalla seuraavan komennon. Tässä esimerkissä oletetaan, että määritystiedosto sijaitsee E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

HPC Pack käyttöönoton komentosarja kestää jonkin aikaa. Komentosarja tekee kuitenkaan on Vie ja lataa klusterin ja tallenna se asiakastietokoneen nykyisen käyttäjän tiedostot-kansiossa. Komentosarja luo seuraavankaltaiselta viestin. Seuraavassa vaiheessa voit tuoda sertifikaatin säilön varmennetta.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Vaihe 2. Purkaminen Excel-työkirjojen ja suorita UDF paikallisen-asiakasohjelmassa

### <a name="excel-activation"></a>Excel-aktivointi

Kun käytät ComputeNodeWithExcel AM kuvan tuotannon toiminnoista, sinun on kelvollinen Microsoft Officen käyttöoikeuden avain aktivoinnin Excelin Laske-solmut. Muussa tapauksessa Excel kokeiluversio vanhentuu 30 päivää ja suorittaminen Excel-työkirjojen epäonnistuu COMException (0x800AC472). 

Excelissä voit rearm toiseen 30 päivän ajan arviointi: Kirjaudu pää-solmu ja clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` kaikki Excel-Laske solmujen kautta HPC klusterin hallinta. Voit rearm enimmäismäärä on kaksi kertaa. Tämän jälkeen sinun on määritettävä kelvollinen Office-käyttöoikeus-näppäintä.

Office Professional Plus 2013 asennettuna AM kuva on äänenvoimakkuutta-version kanssa yleinen aseman käyttöoikeuden Key (GVLK). Voit aktivoida sen kautta Avaintenhallintapalvelu (KMS) / Active Directory-Based aktivointi (AD-BA) tai aktivointi Moniasennustunnuksella (MAK). 

    * Käyttämään KMS/AD-BA käyttää olemassa olevan KMS palvelimen tai määrittää uuden käyttämällä Microsoft Office 2013: n volyymikäyttöoikeuksien käyttöoikeuden Pack. (Voit halutessasi määrittää palvelimen pään solmun.) Aktivoi sitten kautta tai puhelimitse Avaintenhallintapalvelun isännän-näppäintä. Valitse clusrun `ospp.vbs` KMS palvelimen ja portin määrittäminen ja aktivoida Officen kaikissa Excelin Laske solmujen. 
    
    * Käyttää MAK, ensimmäinen clusrun `ospp.vbs` avaimen syöttää ja Aktivoi kaikki Excelin Laske solmujen kautta tai puhelimitse. 

>[AZURE.NOTE]Jälleenmyynti product key for Office Professional Plus 2013 ei voi käyttää AM kuvalla. Jos sinulla on kelvollinen näppäimet ja asennustietoväline kuin tässä Office Professional Plus 2013-aseman version Office- tai Excel-versioissa, voit käyttää niitä sen sijaan. Ensin aseman tämän version ja asenna käytössäsi olevan edition. Excelin Laske uudelleen solmu voi tallentaa mukautetun AM kuvana käyttäminen käyttöönoton asteikko.

### <a name="offload-excel-workbooks"></a>Excel-työkirjojen purku

Excel-työkirjan purku niin, että se toimii Azure HPC Pack-klusterin seuraavasti. Voit tehdä tämän on oltava Excel 2010 tai 2013, jotka on jo asennettu asiakastietokoneeseen.

1. Käytä jotakin vaiheessa 1 vaihtoehto, jos haluat ottaa käyttöön HPC Pack-klusterin Excel Laske solmu kuva. Hanki klusterin sertifikaattitiedosto (.cer) ja klusterin käyttäjänimi ja salasana.

2. Asiakastietokoneen Valitse varmenne: \CurrentUser\Root sertifikaattien klusterin.

3. Varmista, että Excel on asennettu. Luo Excel.exe.config-tiedoston samaan kansioon Excel.exe asiakastietokoneen seuraavat sisältöön. Tällä vaiheella varmistetaan, että HPC Pack 2012 R2 Excelin COM-apuohjelman ladataan onnistuneesti.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Määritä asiakas, voit lähettää HPC Pack-klusterin työt. Yksi vaihtoehto on koko [HPC Pack 2012 R2: n päivitys 3: n asennus](http://www.microsoft.com/download/details.aspx?id=49922) ja asentaa HPC Pack. Voit myös Lataa ja asenna [HPC Pack 2012 R2: n päivitys 3 asiakkaan apuohjelmat](https://www.microsoft.com/download/details.aspx?id=49923) ja on tarvittavat Visual C++ 2010 redistributable tietokoneen ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  Tässä esimerkissä Käytämme nimeltä ConvertiblePricing_Complete.xlsb Esimerkki Excel-työkirja. Voit ladata sen [tähän](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Kopioi työkansion, kuten D:\Excel\Run Excel-työkirja.

7.  Avaa Excel-työkirja. Valintanauhan **kehittäminen** **COM -** apuohjelmat ja varmista, että HPC Pack Excelin COM-apuohjelma on ladattu onnistuneesti.

    ![Excel-apuohjelma HPC Pack][addin]

8.  Muokkaa VBA-makrojen HPCControlMacros Excelin muuttamalla kommentoidut rivit seuraavaa komentosarjaa esitetyllä tavalla. Korvaa tarvittavat arvot-ympäristössä.

    ![Excel-makro HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Kopioi Excel-työkirjan lataaminen-kansioon, kuten D:\Excel\Upload. VBA-makrojen HPC_DependsFiles-vakio on määritetty tämä kansio.

10. Voit suorittaa työkirjan Azure-klusterin valitsemalla laskentataulukon **klusterin** -painiketta.

### <a name="run-excel-udfs"></a>Suorita Excel UDF

Suorita Excel UDF noudattamalla edellisten vaiheiden 1 – 3, jos haluat määrittää asiakastietokone. Excelin UDF varten ei tarvitse on asennettu Laske solmujen Excel-sovellus. Kun yhteyttä klusterin luominen Laske solmujen, voit valita tavallinen Laske solmu kuvan sen sijaan, että Excel Laske solmu-kuva.

>[AZURE.NOTE] Tällä 34 merkkiä Excel 2010: n ja 2013 klusterin connector-valintaikkuna. Tämän valintaikkunan avulla voit määrittää, joka suoritetaan UDF klusterin. Jos koko klusterinimi on pidempi (esimerkiksi hpcexcelhn01.southeastasia.cloudapp.azure.com), se ei mahdu-valintaikkunassa. Menetelmä on esimerkiksi *CCP_IAASHN* tietokoneen muuttujan arvona pitkä klusterinimeä. Kirjoita *CCP_IAASHN %* -valintaikkunassa klusterin pään solmu nimeksi. 

Kun klusterin on otettu, jatka Suorita valmiin otoksen tekemällä seuraavat toimet Excel UDF. Katso mukautetun Excel UDF nämä [resurssit](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) voivat laatia XLL ja ottaa käyttöön IaaS-klusterin.

1.  Avaa uusi Excel-työkirja. Valitse valintanauhan **kehittäminen** sitten **Apuohjelmat**. Valitse-valintaikkunassa **Selaa**, siirry %CCP_HOME%Bin\XLL32-kansioon ja valitse otosten ClusterUDF32.xll. Jos ClusterUDF32 ei ole asiakaskoneeseen, kopioi se pään solmu %CCP_HOME%Bin\XLL32-kansiosta.

    ![Valitse UDF][udf]

2.  Valitse **Tiedosto** > **asetukset** > **Lisäasetukset**. Tarkista **kaavoja**, **Salli käyttäjän määrittämät XLL Funktiot laskentaklusteriin suorittamiseen**. Valitse **asetukset** ja kirjoita koko klusterinimeä **klusterin pään Solmunimi**. (Mainita aiemmin tässä tekstiruudussa on rajoitettu 34 merkkiä, niin kauan klusterinimi ei ehkä sovi. Saa käyttää tietokoneen-muuttuja pitkä klusterin nimeksi.)

    ![Määritä UDF][options]

3.  Voit suorittaa UDF laskennan klusterin, arvo =XllGetComputerNameC() sisältävää solua ja painamalla Enter-näppäintä. Funktio hakee ainoastaan Laske solmun, jossa UDF käytetään nimi. Ensimmäisellä käyttökerralla varten tunnistetiedot-valintaikkunan pyytää käyttäjänimeä ja salasanaa muodostaa IaaS-klusterin.

    ![Suorita UDF][run]

    Kun useita soluja laskemiseen siis, paina Alt-VAIHTO-näppäinyhdistelmää Ctrl + F9 suorittaa laskutoimituksen kaikki solut.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Vaihe 3. Suorita SOA työmäärää paikallisen-asiakasohjelmassa

HPC Pack IaaS-klusterin Yleiset SOA sovellusten suorittamiseen ensin avulla tavoilla vaiheessa 1 käyttöön klusterin. Määritä Yleinen Laske solmu kuva siinä tapauksessa, koska et tarvitse Excelin Laske solmuissa. Noudata sitten seuraavia ohjeita.

1. Kun haettu klusterin varmennetta, tuomalla sen asiakastietokoneen varmenteen: \CurrentUser\Root-kohdassa.

2. Asenna [HPC Pack 2012 R2: n 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) ja [HPC Pack 2012 R2: n päivitys 3 asiakkaan apuohjelmat](https://www.microsoft.com/download/details.aspx?id=49923). Kyseisten työkalujen avulla voit kehittää ja suorita SOA asiakassovelluksia.

3. Lataa HelloWorldR2 [otoksen koodi](https://www.microsoft.com/download/details.aspx?id=41633). Avaa HelloWorldR2.sln Visual Studio 2010 tai 2012: ssa.

4. Luoda EchoService projektin ensin. Ottaa palvelun sitten IaaS-klusterin paikallisen klusterin käyttöön samalla tavalla. Katso lisätietoja kohdasta Readme.doc-HelloWordR2. Muokkaa ja muodosta SOA-asiakassovellusten käynnissä olevat Azure IaaS-klusterin luomiseen HellWorldR2 ja muihin projekteihin seuraavassa kohdassa kuvatulla tavalla.

### <a name="use-http-binding-with-azure-storage-queue"></a>Http-sidonta käyttäminen Azure tallennustilan jonossa

Jos haluat käyttää Http-sidonta Azure tallennustilan jonossa, tee sample code muutamia muutoksia.

* Päivitä klusterinimeä.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Voit myös käyttää oletusarvoa TransportScheme SessionStartInfo tai määrittää sen erikseen Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Käytä oletusarvoista sidonta BrokerClient.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Tai määrittää erikseen basicHttpBinding käyttämällä.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Voit myös UseAzureQueue-merkinnän määrittäminen SessionStartInfo tosi. Jos ei määritetä, sen arvoksi tulee TOSI oletusarvoisesti, kun klusterinimi on Azure toimialueliitteet ja TransportScheme on Http.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Käytä Http-sidonta ilman Azure tallennustilan jonossa

Http-sidonta ilman Azure tallennustilan jonon käyttämään määrittää UseAzureQueue merkintä SessionStartInfo FALSE.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Käytä NetTcp sidonta

Käyttämään NetTcp sidonta kokoonpano on samanlainen kuin paikallisen klusterin muodostamisesta. Sinun täytyy avata muutaman päätepisteet pään solmun AM. Jos klusterin luomiseen käytetyt HPC Pack IaaS käyttöönoton komentosarja, esimerkiksi Määritä päätepisteet Azure perinteinen-portaalissa seuraavien ohjeiden mukaisesti.


1. Lopeta AM.

2. Lisää TCP-portit 9090, 9087, 9091, 9094, istunnon Broker, Broker työntekijä ja Data services

    ![Määritä päätepisteet][endpoint]

3. Aloita AM.

SOA-asiakassovellukseen edellyttää muutoksia lukuun ottamatta muuttaminen IaaS klusterin nimen pään nimi.

## <a name="next-steps"></a>Seuraavat vaiheet

* Katso lisätietoja käytössä Excel työmääriä HPC Pack [nämä resurssit](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) .

* Katso lisätietoja käyttöönotto ja hallinta SOA services HPC Pack [Microsoft HPC Pack SOA palveluiden hallinta](https://technet.microsoft.com/library/ff919412.aspx) .

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
