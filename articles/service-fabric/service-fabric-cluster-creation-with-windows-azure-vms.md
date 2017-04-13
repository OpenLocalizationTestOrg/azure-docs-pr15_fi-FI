<properties
   pageTitle="Luo erillisen klusterin Windows Azure virtuaalilaitteiksi | Microsoft Azure"
   description="Lue, miten voit luoda ja hallita Azure palvelun kangasta-klusterin Azuren näennäiskoneiden on käytössä Windows Server."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/05/2016"
   ms.author="dkshir;chackdan"/>



# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>Luo kolme solmu erillisen palvelun kangasta klusterin Azuren näennäiskoneiden on käytössä Windows Server

Tässä artikkelissa kerrotaan, kuinka voit luoda klusterin Windows-pohjaisesta Azuren näennäiskoneiden (VMs), valitse Windows Server erillisen palvelun kangasta-asennuksen avulla. Tämä on erityisen tapaus [luominen ja hallinta käytössä Windows Server-klusterin](service-fabric-cluster-creation-for-windows-server.md) VMs sijainti [Windows Server Azure virtuaalilaitteiksi](../virtual-machines/virtual-machines-windows-hero-tutorial.md), mutta luot ei [Azure pilvipohjainen palvelun kangasta klusterin](service-fabric-cluster-creation-via-portal.md). Ero on erillisen palvelun kangasta klusterin seuraavasti luoma hallitaan kokonaan itse, aikana Azure pilvipohjainen palvelun kangasta klustereiden hallitaan ja päivittää palvelun kangasta resurssin tarjoaja.


## <a name="steps-to-create-the-standalone-cluster"></a>Erillinen-klusterin luominen

1. Kirjaudu Azure-portaaliin ja luo uuden Windows Server 2012 R2 palvelinkeskuksen AM resurssiryhmä. Lue lisätietoja artikkelista [luominen Windows-AM Azure-portaalissa](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
2. Lisää muutama Lisää Windows Server 2012 R2 palvelinkeskuksen VMs saman resurssiryhmä. Varmista, että kunkin VMs on sama järjestelmänvalvojan käyttäjänimi ja salasana luotaessa. Kerran luotu näkyy kaikkien kolmen VMs virtual samassa verkostossa.
3. Liitä jokaiseen VMs ja käyttävät [Serverin hallinta, paikallisen palvelimen Raporttinäkymät-ikkunan](https://technet.microsoft.com/library/jj134147.aspx)Windowsin palomuuri käytöstä. Näin varmistat, että ‑sovelluksen verkkoliikenteen tarpeisiin viestiä laitteiden välillä. Yhdistettynä Jokaisessa koneessa, saat IP-osoitteen avaamalla komentorivi ja kirjoittamalla `ipconfig`. Voit myös napsauttaa näet valitsemalla resurssiryhmän virtual verkkoresurssin Azure-portaalissa kunkin koneen IP-osoite.
4. Muodosta yhteys VMs ja testaa, että ping kaksi VMs onnistuneesti.
5. Muodosta yhteys VMs ja [Lataa erillisen palvelun kangasta paketti Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) tietokoneeseen uuteen kansioon ja Pura paketti.
6. Avaa tiedosto *ClusterConfig.Unsecure.MultiMachine.json* Muistiossa ja muokkaa kunkin solmun koneet kolme IP-osoitteet. Muuta yläreunassa klusterinimeä ja Tallenna tiedosto.  Osittainen Esimerkki klusterin luettelo näkyy alla.

    ```
    {
        "name": "TestCluster",
        "clusterManifestVersion": "1.0.0",
        "apiVersion": "2015-01-01-alpha",
        "nodes": [
        {
            "nodeName": "vm0",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "vm1",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "vm2",
            "metadata": "Replace the localhost with valid IP address or FQDN below",
            "iPAddress": "10.7.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
    ],
    ```

7. Avaa [PowerShell ise:-ikkuna](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Siirry kansioon, johon uuttaa ladatut erillinen asennuspaketti ja tallentanut klusterin luettelon tiedoston. Suorittaa seuraavan PowerShell-komennon.

    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json -MicrosoftServiceFabricCabFilePath .\MicrosoftAzureServiceFabric.cab
    ```

8. Raportissa pitäisi näkyä PowerShell muodostaa yhteyttä kunkin tietokoneeseen ja luoda klusterin. Kun noin minuutin, voit tarkistaa, jos klusterin on toiminnassa, muodosta yhteys palvelun kangasta Explorer johonkin tietokoneen IP-osoite esimerkiksi käyttämällä `http://10.7.0.5:19080/Explorer/index.html`. Koska tämä on erillinen klusterin ansiosta suojatun sisällytetään on [otettava käyttöön Azure VMs varmenteet](service-fabric-windows-cluster-x509-security.md) tai määrittää jokin koneet [Windows Server Active Directory (AD)-ohjain Windows-todennuksen](service-fabric-windows-cluster-windows-security.md), aivan kuten mitä paikallinen Azure VMs avulla.


## <a name="next-steps"></a>Seuraavat vaiheet
- [Luo erillisen palvelun kangasta klustereiden Windows Server-tai Linux](service-fabric-deploy-anywhere.md)
- [Lisää tai poista solmujen erillisen palvelun kangasta klusteriin](service-fabric-cluster-windows-server-add-remove-nodes.md)
- [Erillinen Windows-klusterin asetukset](service-fabric-cluster-manifest.md)
- [Suojattu erillinen klusterin Windows käyttämällä Windowsin suojaus](service-fabric-windows-cluster-windows-security.md)
- [Suojattu erillinen klusterin X509 avulla Windowsiin varmenteet](service-fabric-windows-cluster-x509-security.md)
