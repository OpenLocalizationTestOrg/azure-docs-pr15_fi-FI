<properties
    pageTitle="Komentosarjan toiminnon kehitys HDInsight | Microsoft Azure"
    description="Opettele mukauttamaan Hadoop klustereiden komentosarja-toiminnolla. Komentosarja-toiminnon avulla voidaan suoritetaan Hadoop klusterissa LISÄOHJELMISTOA tai muuttaa klusterissa asennetut sovellukset määritys."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="develop-script-action-scripts-for-hdinsight-windows-based-clusters"></a>Komentosarja-toiminnon kehittämiseen HDInsight Windows-pohjaisesta klustereiden varten

Opettele kirjoittamaan komentosarja-toiminnon komentosarjojen HDInsight varten. Komentosarjojen komentosarja-toiminnon avulla, lisätietoja [mukauttaminen HDInsight klustereiden komentosarja-toiminnon avulla](hdinsight-hadoop-customize-cluster.md). Saat saman artikkelin kirjoitettu Linux-pohjaiset HDInsight klustereiden, [kehittää komentosarja-toiminnon komentosarjojen Hdinsightista](hdinsight-hadoop-script-actions-linux.md).

> [AZURE.NOTE] Windows-pohjaisesta HDInsight klustereiden on tämän asiakirjan tiedot. Lisätietoja komentosarjatoiminnot käyttämisestä Windows-pohjaisesta klustereiden on artikkelissa [komentosarjan toiminnon kehitys HDInsight (Linux)](hdinsight-hadoop-script-actions-linux.md).


Komentosarja-toiminnon avulla voidaan suoritetaan Hadoop klusterissa LISÄOHJELMISTOA tai muuttaa klusterissa asennetut sovellukset määritys. Komentosarjatoiminnot ovat komentosarjoja, jotka suoritetaan klusterin solmuissa HDInsight klustereiden otetaan käyttöön ja ne suoritetaan, kun klusterin solmut loppuun HDInsight-määritys. Komentosarja-toiminnon suoritetaan järjestelmän järjestelmänvalvojan tilin käyttöoikeudet-kohdassa ja klusterisolmut täydet oikeudet. Kunkin klusterin olla mukana komentosarja, joka suoritetaan siinä järjestyksessä, jossa ne on määritetty toimintojen luettelo.

> [AZURE.NOTE] Jos kohtaat seuraavan virhesanoman:
>
>     System.Management.Automation.CommandNotFoundException; ExceptionMessage : The term 'Save-HDIFile' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
> Tämä johtuu siitä helper menetelmiä kaavioon.  Katso [Helper viestintämenetelmien mukautettuja komentosarjoja](hdinsight-hadoop-script-actions.md#helper-methods-for-custom-scripts).

## <a name="sample-scripts"></a>Komentosarjamallit

Luomisen HDInsight klustereiden Windows-käyttöjärjestelmän komentosarja-toiminto on Azure PowerShell-komentosarjaa. Seuraavassa on esimerkki komentosarjan määrittäminen sivuston määritysten tiedostoja:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    param (
        [parameter(Mandatory)][string] $ConfigFileName,
        [parameter(Mandatory)][string] $Name,
        [parameter(Mandatory)][string] $Value,
        [parameter()][string] $Description
    )

    if (!$Description) {
        $Description = ""
    }

    $hdiConfigFiles = @{
        "hive-site.xml" = "$env:HIVE_HOME\conf\hive-site.xml";
        "core-site.xml" = "$env:HADOOP_HOME\etc\hadoop\core-site.xml";
        "hdfs-site.xml" = "$env:HADOOP_HOME\etc\hadoop\hdfs-site.xml";
        "mapred-site.xml" = "$env:HADOOP_HOME\etc\hadoop\mapred-site.xml";
        "yarn-site.xml" = "$env:HADOOP_HOME\etc\hadoop\yarn-site.xml"
    }

    if (!($hdiConfigFiles[$ConfigFileName])) {
        Write-HDILog "Unable to configure $ConfigFileName because it is not part of the HDI configuration files."
        return
    }

    [xml]$configFile = Get-Content $hdiConfigFiles[$ConfigFileName]

    $existingproperty = $configFile.configuration.property | where {$_.Name -eq $Name}

    if ($existingproperty) {
        $existingproperty.Value = $Value
        $existingproperty.Description = $Description
    } else {
        $newproperty = @($configFile.configuration.property)[0].Clone()
        $newproperty.Name = $Name
        $newproperty.Value = $Value
        $newproperty.Description = $Description
        $configFile.configuration.AppendChild($newproperty)
    }

    $configFile.Save($hdiConfigFiles[$ConfigFileName])

    Write-HDILog "$configFileName has been configured."

Komentosarjan kestää neljä parametria, määritys-tiedostonimi, ominaisuutta haluat muokata, arvo, jonka haluat määrittää, ja kuvaus. Esimerkki:

    hive-site.xml hive.metastore.client.socket.timeout 90

Näiden parametrien määrittäminen hive.metastore.client.socket.timeout arvon 90 rakenteen site.xml-tiedostossa.  Oletusarvo on 60 sekuntia.

Tämä esimerkki komentosarja tukikäytännöistä myös [https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1](https://hditutorialdata.blob.core.windows.net/customizecluster/editSiteConfig.ps1).

Hdinsightista on useita komentosarjoja lisäosat asennetaan HDInsight klustereiden:

Nimi | Komentosarja
----- | -----
**Ohjattu asennus** | https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/spark-Installer-v03.ps1. Katso [asentaminen ja käyttäminen tiedostojen HDInsight klustereiden][hdinsight-install-spark].
**Asenna R** | https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. [Asentaminen]ja käyttäminen R-HDInsight klustereiden[hdinsight-r-scripts].
**Asenna Solr** | https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Katso [asentaminen ja käyttäminen Solr HDInsight-klusterit](hdinsight-hadoop-solr-install.md).
- **Asenna Giraph** | https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Katso [asentaminen ja käyttäminen Giraph HDInsight-klusterit](hdinsight-hadoop-giraph-install.md).

Komentosarja-toiminnon voidaan ottaa käyttöön Azure PowerShellin Azure-portaalista tai käyttämällä HDInsight .NET SDK-paketissa.  Lisätietoja on artikkelissa [mukauttaminen HDInsight klustereiden komentosarja-toiminnon avulla][hdinsight-cluster-customize].

> [AZURE.NOTE] Komentosarjamallit toimivat vain jossa HDInsight klusterin versio 3.1 tai uudempi versio. Katso lisätietoja HDInsight-klusterin versioissa, [HDInsight-klusterin versiot](hdinsight-component-versioning.md).





## <a name="helper-methods-for-custom-scripts"></a>Mukautettujen komentosarjojen Helper menetelmät

Komentosarjan toiminnon helper menetelmät ovat apuohjelmia, jota voit käyttää mukautettuja komentosarjoja kirjoitettaessa. Nämä [https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1](https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1)määritetään ja voidaan lisätä komentosarjojesi avulla seuraavasti:

    # Download config action module from a well-known directory.
    $CONFIGACTIONURI = "https://hdiconfigactions.blob.core.windows.net/configactionmodulev05/HDInsightUtilities-v05.psm1";
    $CONFIGACTIONMODULE = "C:\apps\dist\HDInsightUtilities.psm1";
    $webclient = New-Object System.Net.WebClient;
    $webclient.DownloadFile($CONFIGACTIONURI, $CONFIGACTIONMODULE);

    # (TIP) Import config action helper method module to make writing config action easy.
    if (Test-Path ($CONFIGACTIONMODULE))
    {
        Import-Module $CONFIGACTIONMODULE;
    }
    else
    {
        Write-Output "Failed to load HDInsightUtilities module, exiting ...";
        exit;
    }

Seuraavassa on helper tapoja, jotka toimittaa tämä komentosarja:

Avustaja-menetelmällä | Kuvaus
-------------- | -----------
**Tallenna HDIFile** | Lataa tiedosto-määritetty tunnus URI (Uniform Resource) paikallisen levyllä, johon on liitetty liitetty klusterin Azure AM solmu sijaintiin.
**Laajenna HDIZippedFile** | Pura zip-tiedoston.
**Käynnistää HDICmdScript** | Komentosarjan suorittaminen cmd.exe.
**Kirjoita HDILog** | Kirjoittaa mukautettua komentosarjaa käytetään komentosarja-toiminnon tulos.
**Get-palvelut** | Saat luettelon käytettävissä services käynnissä tietokoneessa, jossa komentosarja suoritetaan.
**Get-palvelu** | Tietyn palvelun nimi syötteeksi, saat yksityiskohtaiset tiedot tietyn palvelun (palvelun nimi, käsitellä tunnus-tilaan, jne.) tietokoneessa, jossa komentosarja suoritetaan.
**Hae HDIServices** | Saat luettelon käytettävissä käynnissä tietokoneessa, jossa komentosarja suoritetaan HDInsight-palvelut.
**Hae HDIService** | Tietyn HDInsight palvelun nimellä syötteeksi, saat tarkat tiedot tietyn palvelun (palvelun nimi, käsitellä tunnus-tilaan, jne.) tietokoneessa, jossa komentosarja suoritetaan.
**Hae ServicesRunning** | Saat luettelon käytettävissä palvelut, jotka ovat käynnissä tietokoneessa, jossa komentosarja suoritetaan.
**Hae ServiceRunning** | Tarkista, onko tietyn palvelun (nimen mukaan) käynnissä tietokoneessa jossa komentosarja suoritetaan.
**Hae HDIServicesRunning** | Saat luettelon käytettävissä käynnissä tietokoneessa, jossa komentosarja suoritetaan HDInsight-palvelut.
**Hae HDIServiceRunning** | Tarkista, onko tietyn HDInsight-palvelun (nimen mukaan) käynnissä tietokoneessa jossa komentosarja suoritetaan.
**Hae HDIHadoopVersion** | Asennettu tietokoneeseen, jossa komentosarja suoritetaan Hadoop-version hankkiminen.
**Testaa IsHDIHeadNode** | Tarkista, onko tietokoneessa, jossa komentosarja suoritetaan pään solmu.
**Testaa IsActiveHDIHeadNode** | Tarkista, onko tietokoneessa, jossa komentosarja suoritetaan aktiivisen pään solmun.
**Testaa IsHDIDataNode** | Tarkista, onko tietokoneessa, jossa komentosarja suorittaa tietojen solmu.
**Muokkaa HDIConfigFile** | Muokkaa config tiedostoja rakenne-site.xml, core site.xml, hdfs site.xml, mapred site.xml tai kuitenkaan site.xml.


## <a name="best-practices-for-script-development"></a>Komentosarjan kehittäminen parhaat käytännöt

Kun kehität HDInsight-klusterin mukautettu komentosarja, käytettävissä on useita parhaita käytäntöjä, kannattaa pitää mielessä:

- Hadoop-version tarkistaminen

    Vain HDInsight versio 3.1 (Hadoop 2.4) ja yllä tuki mukautettujen osien asentaminen klusterin komentosarja-toiminnon avulla. Mukautettu komentosarja sinun on käytettävä **Get-HDIHadoopVersion** avustaja-menetelmä ennen jatkamista muiden tehtävien suorittamiseen komentosarjan Hadoop-version tarkistamisesta.


- Koonneet vakaata komentosarja-resurssit

    Käyttäjien Varmista, että kaikki komentosarjat ja muut palvelutiedot käytetty klusteriin mukautuksen säilyvät klusterin elinkaaren koko ja näiden tiedostojen versiot eivät muutu toistaminen. Nämä resurssit ovat pakollisia, jos uudelleen imaging solmujen klusterin tarvitaan. Paras käytäntö on ladattava ja arkistoida kaikki tallennustilan tilillä, joka ohjaa käyttäjän. Tämä voi olla tallennustilan oletustilin tai mitä tahansa määritettyä aikaa käyttöönoton mukautetun klusterin tallennustilan tilejä.
    Ohjattu ja R mukautettu klusterin näytteiden, jos asiakirjoissa, esimerkiksi Microsoft on tehnyt resurssit paikallisen kopion tallennustilan tilin: https://hdiconfigactions.blob.core.windows.net/.


- Varmista, että klusterin mukauttaminen komentosarja on idempotent

    On odotat HDInsight-klusterin solmut olevan uudelleen kuvalliset klusterin elinkaaren aikana. Klusterin mukauttaminen komentosarja suoritetaan aina, kun klusteriin on uudelleen kuvalliset. Tämä komentosarja on suunniteltava on idempotent siten, että yhteydessä uudelleen imaging-komentosarja varmistaa, että klusterin palautetaan samaan tilaan, jossa se oli vain, kun komentosarja suoritettiin ensimmäistä kertaa, kun klusterin on alun perin luotu mukautettu. Esimerkiksi jos mukautettu komentosarja asentaa sovelluksen D:\AppLocation sen ensin Suorita ja valitse kukin myöhemmin käynnistyksen yhteydessä uudelleen imaging-komentosarja tarkistaa, onko sovellus on D:\AppLocation sijainnissa, ennen kuin jatkat muut vaiheet komentosarja.


- Asenna mukautettujen osien tallenteen sijainti

    Klusterin ollessa uudelleen kuvalliset C:\ resurssin asema ja D:\ järjestelmäasema voi olla uudelleen muotoiltu, tuloksena on tietoja ja sovelluksia, jotka on asennettu näiden asemista menetyksiä. Tämä voi johtua myös, jos Azure virtuaalikoneen (AM)-solmu, joka on osa klusterin siirtyy ja uusi solmu korvataan. Voit asentaa osat D:\-asemassa tai klusterin C:\apps kohtaa. Muista sijainneista C:\ asemassa on varattu. Määritä sijainti, missä sovellukset tai kirjastot ovat on asennettu klusterin mukauttaminen komentosarja.


- Varmista klusterin arkkitehtuuri suuri käytettävyys

    Hdinsightissa on aktiivinen-passiivinen-arkkitehtuuri hyvän käytettävyyden, jossa yksi pään solmu on aktiivinen (jossa HDInsight-palveluita käyttävät) tila ja toisen pään solmun on valmiustilassa (mikä HDInsight-palvelut eivät ole käytössä). Solmut vaihtaa aktiivinen ja Passiivinen tila, jos HDInsight-palvelut keskeytetään. Jos komentosarja-toimintoa käytetään palveluiden asennetaan molemmat pään solmut suuren, Huomaa, että HDInsight automaattisesti järjestelmä voi epäonnistua automaattisesti käyttäjän asennettu palveluista päälle. Näin käyttäjän Asennetut palvelut HDInsight pään solmuissa, jotka on tarkoitus olla käytettävissä on on omia automaattisesti järjestelmä Jos aktiivinen-Passiivinen tila- tai on aktiivinen aktiivinen-tilassa.

    HDInsight-komentosarja-toiminnon-komento suoritetaan sekä pää solmuissa, kun pää-solmu rooli on määritetty *ClusterRoleCollection* -parametrin arvon. Niin kun suunnittelet mukautettu komentosarja, tarkista, että komentosarja on huomioon nämä asetukset. Älä suorita ilmenee ongelmia, jossa samoja palveluja asennetaan ja toinen pää solmujen aloitettu, mutta ne sinut pikaluistelukilpailuissa toistensa kanssa. Lisäksi huomioon, että tiedot menetetään alustamisen uudelleen, jotta asennettu kautta komentosarja-toiminnon on oltava joustavat tällaisten tapahtumien. Sovellukset kannattaa suunnitella helposti saatavilla, joka jaetaan useiden solmujen käyttäminen. Huomaa, että jopa 1/5 klusterin solmujen voi olla uudelleen kuvallinen samanaikaisesti.


- Määritä mukautettujen osien käyttämään Azure-Blob-säiliö

    Mukautetun osat, jotka olet asentanut klusterin solmuissa on ehkä Hadoop Distributed (HDFS)-järjestelmän tallennusvälineiden oletusasetukset. Sinun on muutettava käyttämään Azure-Blob-säiliö määritykset. Klusterin uudelleen kuvan HDFS tiedostojärjestelmän saa muotoiltu ja menetä mitään tietoja, jotka on tallennettu. Käyttämällä Azure-Blob-säiliö varmistaa sen sijaan, että tiedot säilytetään.

## <a name="common-usage-patterns"></a>Yleiset käyttötavat

Tässä osassa on ohjeet yksityiskohtaisista joitakin yleisiä käyttötavat, joita saattaa ilmetä mukautettua komentosarjaa kirjoitettaessa.

### <a name="configure-environment-variables"></a>Määritä ympäristömuuttujat

Komentosarjan toiminnon kehitystä, usein tuntuu tarvitse ympäristömuuttujat määrittäminen. Todennäköisesti tilanne on esimerkiksi kun Lataa binaarinen ulkoiselle sivustolle ja asentaa sen sitten klusterin Lisää sijainti, johon se on asennettu, 'PATH-ympäristömuuttuja. Seuraavat koodikatkelman näytetään, miten voit määrittää ympäristömuuttujat mukautettu komentosarja.

    Write-HDILog "Starting environment variable setting at: $(Get-Date)";
    [Environment]::SetEnvironmentVariable('MDS_RUNNER_CUSTOM_CLUSTER', 'true', 'Machine');

Tämä lause asettaa ympäristömuuttuja **MDS_RUNNER_CUSTOM_CLUSTER** arvon "true" ja määrittää myös tämän tietokoneen muuttujan laajuus. Joskus on tärkeää, että ympäristömuuttujat määritetään tarvittavan alueen – tietokoneen tai käyttäjän. Lisätietoja on ohjeaiheessa [tähän] [ 1] lisätietoja ympäristömuuttujat määrittämisestä.

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Mukautettujen komentosarjojen tallennuspaikkaa sijaintien käyttö

Komentosarjojen avulla voit mukauttaa klusterin on joko olla klusterin tallennustilan oletustilin tai julkinen vain luku-säilön tallennustilan-tilille. Jos komentosarja käyttää muualla resurssit nämä on kaikkien käytettävissä olevan (vähintään julkisen vain luku). Voit esimerkiksi haluta käyttää tiedostoa ja tallentaa sen SaveFile HDI-komennon avulla.

    Save-HDIFile -SrcUri 'https://somestorageaccount.blob.core.windows.net/somecontainer/some-file.jar' -DestFile 'C:\apps\dist\hadoop-2.4.0.2.1.9.0-2196\share\hadoop\mapreduce\some-file.jar'

Tässä esimerkissä sinun on varmistettava, että säilö somecontainer, jos tallennustilan tilin 'somestorageaccount' on julkisesti saatavilla. Komentosarja muussa tapauksessa palauttaa ei löydy poikkeuksen ja epäonnistuu.

### <a name="pass-parameters-to-the-add-azurermhdinsightscriptaction-cmdlet"></a>Välittää parametreja Lisää AzureRmHDInsightScriptAction cmdlet-komento

Haluat siirtää useita parametreja Lisää AzureRmHDInsightScriptAction cmdlet-komento, Muotoile sisältämään kaikki parametrit komentosarja merkkijonoarvo. Esimerkki:

    "-CertifcateUri wasbs:///abc.pfx -CertificatePassword 123456 -InstallFolderName MyFolder"

tai

    $parameters = '-Parameters "{0};{1};{2}"' -f $CertificateName,$certUriWithSasToken,$CertificatePassword


### <a name="throw-exception-for-failed-cluster-deployment"></a>Palauttaa poikkeuksen epäonnistui klusterin käyttöönottoa varten

Jos haluat saada tarkasti ilmoituksen siitä, että klusterin mukauttaminen ei onnistunut oikein, on tärkeää palauttaa poikkeuksen ja klusterin luominen epäonnistuu. Voit esimerkiksi haluta käsitellä tiedostoa, jos se on olemassa ja käsitellä kohtaa, johon tiedosto ei ole virhe kirjainkokoa. Tämä varmistaa, että komentosarja poistuu tilanteen klusterin tilan tunnetaan oikein. Seuraavat koodikatkelman avulla, miten haluat saavuttaa:

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    exit
    }

Tämä koodikatkelman Jos tiedosto ei ole valittu, se johtaa tilaan, jossa komentosarja todella poistuu tilanteen jälkeen tulostaminen virhesanoma ja klusterin saavuttaa olettaen, että se "onnistunut" valmis klusterin mukauttaminen prosessi käynnissä. Jos haluat ilmoituksen siitä, että klusterin mukauttaminen olennaisesti tarkasti ei onnistunut oikein puuttuvan tiedoston vuoksi, on tarkoituksenmukaisempia klusterin mukauttaminen vaihe epäonnistua ja palauttaa poikkeuksen. Tätä sinun on käytettävä otoksen koodikatkelman sijaan.

    If(Test-Path($SomePath)) {
        #Process file in some way
    } else {
        # File does not exist; handle error case
        # Print error message
    throw
    }


## <a name="checklist-for-deploying-a-script-action"></a>Käyttöönoton komentosarja-toiminnon tarkistusluettelo
Näin on tapahtunut valmisteltaessa ottamaan komentosarjat:

1. Sijoita tiedostoja, jotka sisältävät mukautetut komentosarjat paikkaa, jossa on käytettävissä klusterin solmut käyttöönoton aikana. Tämä voi olla mikä tahansa oletus-tai määrittää tällä hetkellä klusterin käyttöönottoa tai muita kaikkien käytettävissä olevan tallennustilan säilö tallennustilan tilejä.
2. Lisää tarkistukset komentosarjoja, varmista, että he suorittavat idempotently, jolloin komentosarja voidaan suorittaa useita kertoja, saman solmun kyselyjä.
3. **Kirjoita tulosteen** Azure PowerShell-cmdlet-komennon avulla voit tulostaa STDOUT sekä STDERR. Älä käytä **Kirjoitus-Host**.
4. Käytä väliaikaisen tiedostokansion, kuten $env: TEMP-komentosarjojen käyttää ladattu tiedosto ja sitten Puhdista ne sen jälkeen, kun komentosarjat on suoritettu.
5. Asenna vain osoitteessa D:\ tai C:\apps mukautetun ohjelmisto. Muista sijainneista c-asema ei voi käyttää, kun ne on varattu. Huomaa, että c-asema ulkopuolella C:\apps-kansion tiedostojen asentaminen saattaa johtaa asennus epäonnistuu aikana kuvat uudelleen solmun.
6. Silloin, kun OS tason asetukset tai Hadoop service määritysten tiedostot ovat muuttuneet, haluat ehkä HDInsight-palveluiden uudelleenkäynnistäminen niin, että ne Nosta OS tason asetuksia, kuten määrittäminen komentosarjat ympäristössä muuttujat.

## <a name="debug-custom-scripts"></a>Mukautettujen komentosarjojen korjaaminen

Komentosarjan virhelokeja tallennetaan, sekä muita tulostus-tallennustilan oletustilin, jonka olet määrittänyt sen luomisen osoitteessa klusterin. Lokit tallennetaan-nimisen taulukon *u < \cluster-name-fragment >< \time-stamp > setuplog*. Nämä ovat koostetun lokit, joka on kaikkien solmujen (pään solmu ja työntekijä solmujen), jossa komentosarja käytetään klusterin tietueita.
Helposti lokeista on Visual Studio HDInsight-työkalujen avulla. Asentaminen Työkalut-kohdassa [käyttäminen HDInsight Visual Studio Hadoop-Työkalut](hdinsight-hadoop-visual-studio-tools-get-started.md#install-hdinsight-tools-for-visual-studio)

**Voit tarkistaa Visual Studiossa lokiin**

1. Avaa Visual Studio.
2. Valitse **Näytä**ja valitse sitten **Palvelimen Explorer**.
3. "Azure" hiiren kakkospainikkeella, valitse Muodosta yhteys **Microsoft Azure tilaukset**ja anna tunnistetiedot.
4. Laajenna **tallennustilan**, käyttää käyttöjärjestelmän Azure-tallennustilan tilin, **taulukoita**ja kaksoisnapsauta taulukkonimi.


Voit myös remote klusterisolmut Nähdäksesi molemmat STDOUT ja mukautettujen komentosarjojen STDERR. Kukin solmu kirjautuu vain solmun yhteydessä ja **C:\HDInsightLogs\DeploymentAgent.log**kirjautuneena. Nämä lokitiedostot tallentaa kaikki tulostaa olevasta mukautettua komentosarjaa. Esimerkki log-koodikatkelman ohjattu komentosarja-toiminto on seuraavanlainen:

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand; Details : BEGIN: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Starting Spark installation at: 09/04/2014 21:46:02 Done with Spark installation at: 09/04/2014 21:46:38;

    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;
    ...

    Microsoft.Hadoop.Deployment.Engine.CustomPowershellScriptCommand;
    Details : END: Invoking powershell script https://configactions.blob.core.windows.net/sparkconfigactions/spark-installer.ps1.;
    Version : 2.1.0.0;
    ActivityId : 739e61f5-aa22-4254-aafc-9faf56fc2692;
    AzureVMName : HEADNODE0;
    IsException : False;
    ExceptionType : ;
    ExceptionMessage : ;
    InnerExceptionType : ;
    InnerExceptionMessage : ;
    Exception : ;


Tässä lokissa on tyhjä, että ohjattu komentosarja-toiminto on suoritettu nimeltä HEADNODE0 AM ja että poikkeuksia on ilmenee suorituksen aikana.

Silloin, kun suoritusvirhe ilmenee, tulos kuvaava se myös sisälsi lokitiedosto. Lokitiedostot sisältämiä tietoja voi olla apua komentosarjan ongelmien vianmäärityksessä, joita voi syntyä.


## <a name="see-also"></a>Katso myös

- [Mukauta HDInsight klustereiden komentosarja-toiminnon käyttäminen][hdinsight-cluster-customize]
- [Asentaminen ja käyttäminen ohjattu HDInsight klustereiden][hdinsight-install-spark]
- [Asentaminen ja käyttäminen HDInsight klustereiden R][hdinsight-r-scripts]
- [Asentaminen ja käyttäminen Solr HDInsight-klusterit](hdinsight-hadoop-solr-install.md).
- [Asentaminen ja käyttäminen Giraph HDInsight-klusterit](hdinsight-hadoop-giraph-install.md).

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-r-scripts]: hdinsight-hadoop-r-scripts.md
[powershell-install-configure]: install-configure-powershell.md

<!--Reference links in article-->
[1]: https://msdn.microsoft.com/library/96xafkes(v=vs.110).aspx
