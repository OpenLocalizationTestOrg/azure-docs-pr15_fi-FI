<properties
    pageTitle="Tyhjä reunan solmujen käyttäminen HDInsight | Microsoft Azure"
    description="Miten voit lisätä ampty-reuna solmu HDInsight-klusteriin, joka voidaan käyttää asiakkaan ja testaa/isäntään HDInsight-sovellukset."
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>

# <a name="use-empty-edge-nodes-in-hdinsight"></a>Tyhjä reunan solmujen käyttäminen Hdinsightiin

Opi lisää tyhjä reuna-solmu Linux-pohjaiset HDInsight-klusterin. Tyhjä reuna-solmu on Linux virtual machine saman asiakastyökalut asentanut ja määrittänyt headnodes kuin, mutta ei hadoop-palveluita. Voit käyttää klusterin, asiakas-sovellusten testaaminen ja isännöinnin asiakassovelluksissa verkkosovellusten reuna-solmu. 

Voit lisätä tyhjän reuna-solmu aiemmin HDInsight-klusterin, kun luot klusterin uuteen klusteriin. Lisätään tyhjään reuna-solmu on valmis käyttämällä Azure Resurssienhallinta mallia.  Seuraavassa esimerkissä esitetään, kuinka se tehdään mallin avulla:

    "resources": [
        {
            "name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
            "type": "Microsoft.HDInsight/clusters/applications",
            "apiVersion": "[variables('clusterApiVersion')]",
            "properties": {
                "marketPlaceIdentifier": "EmptyNode",
                "computeProfile": {
                    "roles": [{
                        "name": "edgenode",
                        "targetInstanceCount": 1,
                        "hardwareProfile": {
                            "vmSize": "[parameters('edgeNodeSize')]"
                        }
                    }]
                },
                "installScriptActions": [{
                    "name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
                    "uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
                    "roles": ["edgenode"]
                }],
                "uninstallScriptActions": [],
                "httpsEndpoints": [],
                "applicationType": "CustomApplication"
            }
        }
    ],

Otoksessa esitetyllä voit myös soittaa [komentosarja-toiminnon](hdinsight-hadoop-customize-cluster-linux.md) , joka suoritetaan lisämäärityksiä, kuten asentaminen [Apache sävyä](hdinsight-hadoop-hue-linux.md) reuna-solmu.

Luotuasi reuna-solmu voit muodostaa yhteyden käyttämällä SSH reuna-solmu ja näyttää asiakastyökalut käyttämään HDInsight Hadoop-klusterin.

## <a name="add-an-edge-node-to-an-existing-cluster"></a>Lisää reuna-solmu aiemmin luodun klusterin

Tässä osassa Käytä Resurssienhallinta mallin reuna-solmun lisääminen aiemmin luotuun HDInsight-klusteriin.  Resurssienhallinta-mallin löytyvät [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Resurssienhallinta-mallin puhelujen komentosarjan toiminnon komentosarjan https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh sijaitsee. Komentosarja ei tehdä mitään toimia.  Tämä on osoitettava puheluja komentosarja-toiminnon Resurssienhallinta-mallista.

**Voit lisätä aiemmin luodun klusterin tyhjä reuna-solmu**

1. Luo HDInsight-klusterin, jos jokin ei ole vielä.  Katso [Hadoop-opetusohjelma: Linux-pohjaiset Hadoop käyttämisestä HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Valitse seuraavassa kuvassa Azure kirjautuminen ja avaa Resurssienhallinta Azure-malli Azure-portaalissa. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Määritä seuraavat ominaisuudet:

    - CLUSTERNAME: Kirjoita aiemmin luodun HDInsight-klusterin nimi.
    - EDGENODESIZE: Valitse jokin AM koot.
    - EDGENODEPREFIX: Oletusarvo on **Uusi**.  Käytä oletusarvoa, reunan Solmunimi on **Uusi edgenode**.  Voit mukauttaa etuliite-portaalista. Voit mukauttaa nimen mallista.


4. Valitse **OK** , Tallenna muutokset.
5. **Resurssiryhmä**Valitse resurssiryhmä.
6. Valitse **Tarkista juridiset ehdot**ja valitse sitten **Osta**.
7. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**.

## <a name="add-an-edge-node-when-creating-a-cluster"></a>Lisää reuna-solmu klusterin luotaessa

Tässä osassa käytät luomalla HDInsight-klusterin reuna-solmu Resurssienhallinta mallia.  Resurssienhallinta-mallin löytyvät julkisen [Azure Blob storage säilö](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). Resurssienhallinta-mallin puhelujen komentosarjan toiminnon komentosarjan https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh sijaitsee. Komentosarja ei tehdä mitään toimia.  Tämä on osoitettava puheluja komentosarja-toiminnon Resurssienhallinta-mallista.

**Voit lisätä aiemmin luodun klusterin tyhjä reuna-solmu**

1. Luo HDInsight-klusterin, jos jokin ei ole vielä.  Katso [Hadoop-opetusohjelma: Linux-pohjaiset Hadoop käyttämisestä HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Valitse seuraavassa kuvassa Azure kirjautuminen ja avaa Resurssienhallinta Azure-malli Azure-portaalissa. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Määritä seuraavat ominaisuudet:
        
    - CLUSTERNAME: Kirjoita nimi uuden klusterin luominen.
    - CLUSTERLOGINUSERNAME: Kirjoita Hadoop HTTP-käyttäjänimi.  Oletusnimi on **järjestelmänvalvoja**.
    - CLUSTERLOGINPASSWORD: Määritä Hadoop HTTP käyttäjän salasana.
    - SSHUSERNAME: Kirjoita SSH käyttäjänimi. Oletusnimi on **sshuser**.
    - SSHPASSWORD: Määritä SSH käyttäjän salasana.
    - SIJAINTI: Kirjoita resurssin ryhmän nimen, klusterin ja tallennustilaa oletustilin sijainti.
    - CLUSTERTYPE: Oletusarvo on **hadoop**.
    - CLUSTERWORKERNODECOUNT: Oletusarvo on 2.
    - EDGENODESIZE: Valitse jokin AM koot.
    - EDGENODEPREFIX: Oletusarvo on **Uusi**.  Käytä oletusarvoa, reunan Solmunimi on **Uusi edgenode**.  Voit mukauttaa etuliite-portaalista. Voit mukauttaa nimen mallista.

4. Valitse **OK** , Tallenna muutokset.
5. **Resurssiryhmä**ja kirjoita uusi resurssiryhmä nimi.
6. Valitse **Tarkista juridiset ehdot**ja valitse sitten **Osta**.
7. Valitse **Kiinnitä Raporttinäkymät-ikkunan**ja valitse sitten **Luo**. 


## <a name="access-an-edge-node"></a>Access edge-solmu

Reuna-solmu ssh päätepiste on <EdgeNodeName>. <ClusterName>-ssh.azurehdinsight.net:22.  Esimerkiksi uusi-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Reuna-solmu tulee näkyviin sovelluksen Azure-portaalissa.  Portaalin avulla voit käyttää käyttämällä SSH reuna-solmu tiedot.

**Reunan solmu SSH päätepisteen vahvistamiseksi**

1. Kirjaudu [Azure portal](https://portal.azure.com).
2. Avaa HDInsight-klusterin reuna-solmu.
3. Valitse **sovellusten** klusterin-sivu. Reuna-solmu on artikkelissa.  Oletusnimi on **Uusi edgenode**.
4. Valitse reuna-solmu. On artikkelissa SSH päätepiste.

**Reuna-solmu rakenteen käyttäminen**

5. Reuna-solmu muodostaa SSH avulla.  Katso [Käytä Linux-pohjaiset Hadoop HDInsight Linux, Unix-tai OS X-ja SSH](hdinsight-hadoop-linux-use-ssh-unix.md) tai [Käytä SSH kanssa Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Kun olet muodostanut yhteyden käyttämällä SSH reuna-solmu, käytä seuraavaa komentoa rakenne-konsolin avaaminen:

        hive
7. Suorita seuraava komento rakenteen taulukot näkyviin klusterin:

        show tables;

## <a name="delete-an-edge-node"></a>Poista reuna-solmu

Voit poistaa reuna-solmu Azure-portaalista.

**Jos haluat käyttää reuna-solmu**

1. Kirjaudu [Azure portal](https://portal.azure.com).
2. Avaa HDInsight-klusterin reuna-solmu.
3. Valitse **sovellusten** klusterin-sivu. Näet on reuna solmujen luettelo.  
4. Napsauta poistettavien reuna-solmu ja valitse sitten **Poista**.
5. Vahvista valitsemalla **Kyllä** .

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on opit reuna-solmu lisäämisestä ja reuna-solmu käyttämisestä. Lisätietoja on seuraavissa artikkeleissa:

- [Asenna HDInsight sovellukset](hdinsight-apps-install-applications.md): Lue, miten voit asentaa sovelluksen oman klustereiden Hdinsightista.
- [Mukautetun HDInsight-sovellusten asentaminen](hdinsight-apps-install-custom-applications.md): käyttöönotto julkaisemattomien HDInsight sovelluksen Hdinsightista.
- [Julkaise HDInsight-sovellukset](hdinsight-apps-publish-applications.md): opit julkaisemaan mukautettujen HDInsight sovellusten Azure Marketplacesta.
- [MSDN: HDInsight-sovelluksen asentaminen](https://msdn.microsoft.com/library/mt706515.aspx): Lue, miten voit määrittää HDInsight-sovellukset.
- [Mukauta Linux-pohjaiset HDInsight klustereiden komentosarja-toiminnon käyttäminen](hdinsight-hadoop-customize-cluster-linux.md): Lue, miten voit asentaa lisää sovelluksia komentosarja-toiminnon avulla.
- [Luo Linux-pohjaiset Hadoop klusterit HDInsight Resurssienhallinta mallien avulla](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Lue, miten voit soittaa Resurssienhallinta mallien avulla voit luoda HDInsight klustereiden.

