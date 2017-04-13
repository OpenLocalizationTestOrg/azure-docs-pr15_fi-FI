<properties
   pageTitle="Määritä erillinen klusterin | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Määritä erillinen tai yksityinen palvelun kangasta klusterin."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="dkshir"/>


# <a name="configuration-settings-for-standalone-windows-cluster"></a>Erillinen Windows-klusterin asetukset

Tässä artikkelissa kerrotaan, miten voit määrittää erillisen palvelun kangasta klusterin _**ClusterConfig.JSON**_ -tiedoston avulla. Voit määrittää tietoja, kuten palvelun kangasta solmut ja IP-osoitteet, erityyppiset solmujen klusterin, suojausmäärityksiä sekä kannalta vika/päivitys toimialueiden verkkotopologia erillinen klusterin tätä tiedostoa.

Kun [Lataa erillisen palvelun kangasta-paketti](service-fabric-cluster-creation-for-windows-server.md#downloadpackage), muutamia esimerkkejä ClusterConfig.JSON tiedosto ladataan työn tietokoneeseesi. Esimerkkejä, joiden *DevCluster* tallentamisessa heidän nimensä avulla voit luoda klusterin kaikki kolme solmujen samaan tietokoneeseen, kuten loogiset solmujen kanssa. Näiden ulos vähintään yksi solmu on merkitty ensisijainen solmu. Tämän klusterin on hyötyä kehitys- tai tuotantoympäristössä ja tuotannon klusteria ei ole tuettu. Esimerkkejä, joiden *MultiMachine* tallentamisessa heidän nimensä avulla voit luoda tuotannon laatu-klusterin kunkin solmun eri tietokoneeseen. Nämä klusterin ensisijainen solmujen määrän perusteella [luotettavuuden taso](#reliability).

1. *ClusterConfig.Unsecure.DevCluster.JSON* ja *ClusterConfig.Unsecure.MultiMachine.JSON* näyttää luomiseen suojaamaton testi- tai klusterin. 
    
2. *ClusterConfig.Windows.DevCluster.JSON* ja *ClusterConfig.Windows.MultiMachine.JSON* näyttää, miten voit luoda testi- tai klusterin suojattu [Windowsin suojaus](service-fabric-windows-cluster-windows-security.md).

3. *ClusterConfig.X509.DevCluster.JSON* ja *ClusterConfig.X509.MultiMachine.JSON* näyttää, miten voit luoda testi- tai klusterin, suojattu [X509 sertifikaattiin perustuva suojaus](service-fabric-windows-cluster-x509-security.md). 


Nyt on tutkii _**ClusterConfig.JSON**_ tiedoston eri osiin.

## <a name="general-cluster-configurations"></a>Yleiset Klusterimääritykset
Laaja klusterin tiettyjä määrityksiä kattaa alla JSON koodikatkelman esitetyllä tavalla.

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2015-01-01-alpha",

Voit tarkastella minkä tahansa kutsumanimi palvelun kangasta klusterin määrittämällä muuttujan **nimi** . **ClusterConfigurationVersion** on yhteyttä klusterin; versionumero suurentaa sen aina, kun päivität palvelun kangasta-klusterin. Jätä kuitenkin **apiVersion** oletusarvoon.


<a id="clusternodes"></a>
## <a name="nodes-on-the-cluster"></a>Klusterin solmut
Voit määrittää solmut palvelun kangasta-klusterissa käyttämällä koodikatkelman seuraavassa **solmut** -osassa.

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

Palvelun kangasta klusteriin on oltava ainakin 3 solmujen. Voit lisätä useita solmujen tähän osaan asetusten mukaan. Seuraavassa taulukossa kuvataan kunkin solmun asetukset.

|**Solmun määritys**|**Kuvaus**|
|-----------------------|--------------------------|
|nodeName|Voit antaa minkä tahansa kutsumanimi solmu.|
|IP-osoite|Katso tekemäsi solmun IP-osoite, avaamalla komentorivi-ikkuna ja kirjoittamalla `ipconfig`. Huomaa IPV4-osoite ja määrittää sen **IP-osoite** -muuttuja.|
|nodeTypeRef|Kukin solmu voi määrittää eri solmutyyppi. [Solmutyypit](#nodetypes) määritetään alla olevasta osiosta.|
|faultDomain|Vian toimialueiden avulla klusterin järjestelmänvalvojat voivat määrittää fyysinen solmut, joka saattaa epäonnistua vuoksi jaetun fyysinen riippuvuudet samanaikaisesti.|
|upgradeDomain|Päivityksen toimialueiden kuvaavat solmujen, palvelun kangasta päivityksistä tietoja samaan aikaan Sammuta määrä. Voit valita mitkä päivityksen toimialueet liittäminen solmut, kun ne ovat ei rajoitettu fyysinen vaatimuksia.| 


## <a name="cluster-properties"></a>Klusterin ominaisuudet

**Ominaisuudet** -osan ClusterConfig.JSON käytetään Määritä klusterin seuraavasti.

<a id="reliability"></a>
### <a name="reliability"></a>Luotettavuuden 
**ReliabilityLevel** osa määrittää järjestelmäpalvelut, jotka voidaan suorittaa klusterin ensisijainen solmut kopioiden lukumäärä. Tämä Lisää luotettavuutta näistä palveluista ja näin ollen klusterin. Voit määrittää tämän muuttujan *pronssi*, *Hopea*, *kulta* tai *Platinum* 3, 5, 7 tai 9 kopioiden näistä palveluista tarpeen mukaan. Katso alla olevassa esimerkissä.

    "reliabilityLevel": "Bronze",
    
Huomaa, että ensisijainen solmu suoritetaan yksittäistä kopiota järjestelmän palveluja, koska tarvitset vähintään 3 ensisijainen solmujen *pronssi*, *Hopea*, *kulta* , 7 ja 9 *Platinum* luotettavuutta tasoissa 5.


### <a name="diagnostics"></a>Vianmääritys
**DiagnosticsStore** -osan avulla voit määrittää parametrit käyttöön diagnostiikka- ja vianmääritys solmu- tai klusterin virheet, seuraavat koodikatkelman esitetyllä tavalla. 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**Metatietojen** mukaista klusterin diagnostiikka- ja voidaan määrittää asetusten mukaan. Muuttujia auttaa kerääminen tapahtumien seuranta jäljityslokit, kaatumisvedokset sekä suorituskyvyn laskureita. Lue lisätietoja tapahtumien seuranta Jäljityslokit [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) ja [Tapahtumien seuranta jäljitys](https://msdn.microsoft.com/library/ms751538.aspx) . Kaikki lokit, mukaan lukien [kaatua Vedostaa](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/) ja [suorituskyvyn laskureita](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) voit ohjautuu käyttämääsi laitteeseen **connectionString** -kansioon. Voit käyttää myös *AzureStorage* diagnostiikka tallentamista varten. Alla on esimerkki koodikatkelman.

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>Tietoturva 
**Suojaus** -osa on suojattu erillisen palvelun kangasta klusterin. Seuraavat koodikatkelman, jossa näkyy osa tämän osan.

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**Metatietojen** on suojattu klusterin kuvaus, ja voit määrittää asetusten mukaan. Suojaus, joka klusterin ja solmujen Toteuta tyypin selvittäminen **ClusterCredentialType** ja **ServerCredentialType** . Hän voi määrittää varmenteen perustuva suojaus joko *X509* tai *Windows* Azure Active Directory-pohjaista arvopaperille. **Suojaus** -osa loput perusteella suojauksen tyyppi. Lue lisätietoja siitä, miten Täytä **Suojaus** -kohdassa [erillinen klusterin suojaukselle varmenteet](service-fabric-windows-cluster-x509-security.md) ja [Windowsin käyttöoikeusryhmät erillinen klusterin](service-fabric-windows-cluster-windows-security.md) .


<a id="nodetypes"></a>
### <a name="node-types"></a>Solmutyypit
**NodeTypes** -osassa kuvataan solmut yhteyttä klusterin sisältävän tyyppi. Vähintään yksi solmutyyppi on määritettävä-klusterin alla koodikatkelman esitetyllä tavalla. 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**Nimi** on tietyn solmu tällaista kuvaava nimi. Luo solmu solmu tämän tyypin, määritä sen kutsumanimi **nodeTypeRef** muuttujan kyseisen solmun [edellä](#clusternodes)mainituista. Solmun mistäkin, jota käytetään yhteyden päätepisteet määritetään. Voit valita minkä tahansa näistä yhteyden päätepisteet porttinumero, kunhan ne eivät ole ristiriidassa muiden päätepisteet klusteriin. Usean solmu-klusterin ole vähintään yksi ensisijainen solmujen (eli **isPrimary** on määritetty *Tosi*), [**reliabilityLevel**](#reliability)mukaan. Lue [palvelun kangasta klusterin kapasiteetin suunnittelun huomioitavista](service-fabric-cluster-capacity.md) **nodeTypes** ja **reliabilityLevel** arvojen ja tietää, mitä ensisijainen ja perus solmu tiedostotyypit. 

#### <a name="endpoints-used-to-configure-the-node-types"></a>Voit määrittää solmu käyttämistä päätepisteistä

- *clientConnectionEndpointPort* on asiakkaan portin ottaa yhteyttä klusterin, kun API-asiakas. 
- *clusterConnectionEndpointPort* on portti, johon solmut keskenään.
- *leaseDriverEndpointPort* on klusterin varauksen ohjaimen portin voit selvittää, jos solmut on yhä käytössä. 
- *serviceConnectionEndpointPort* on yhteydenpito tietyn solmun palvelun kangasta-asiakas käyttää sovelluksia ja palveluja, jotka on otettu käyttöön, solmu portti.
- *httpGatewayEndpointPort* on palvelun kangasta Explorer klusterin muodostamisessa käytettävä portti.
- *ephemeralPorts* on [dynaaminen porteista käyttöjärjestelmän](https://support.microsoft.com/kb/929851). Palvelun kangasta käyttää osa ne sovelluksen portit ja jäljellä olevat päivitetään käyttöjärjestelmän käytettävissä. Se yhdistää tämän alueen myös aiemmin alueeseen, esitä-Käyttöjärjestelmää, jotta kaikkiin tarkoituksiin, voit käyttää alan JSON mallitiedostojen alueet. Haluat varmistaa, että alku ja loppu-portit välinen ero on vähintään 255. 
- *applicationPorts* ovat portit, jota käytetään palvelun kangasta-sovelluksilla. Nämä tiedostot on alijoukkoa *ephemeralPorts*, tarpeeksi peittää päätepisteen vaatimus sovellustesi. Palvelun kangasta Käytä näitä aina, kun uusi portit tarvittavat sekä huolehtia avaaminen palomuurin porttien varten. 
- *reverseProxyEndpointPort* on valinnainen käänteisen välityspalvelimen päätepisteen. Saat lisätietoja [Kangasta käänteinen välityspalvelin](service-fabric-reverseproxy.md) . 


### <a name="other-settings"></a>Muut asetukset
**FabricSettings** -osan avulla voit määrittää lokit ja palvelun kangasta tietojen pääkansion kansioihin. Voit mukauttaa nämä vain ensimmäisen klusterin luonnin aikana. Jäljempänä tässä osassa otoksen katkelma.

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

On suositeltavaa käyttäen FabricDataRoot ja FabricLogRoot-OS asema, kun se sisältää Lisää luotettavuutta vastaan OS kaatuu. Huomaa, että Jos mukautat vain tiedot pääkansio, sitten log pääkansio sijoitetaan tiedot pääkansio alapuolelle yhtä tasoa alemmaksi.


## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet määrittänyt erillinen klusterin asetusten mukaan ClusterConfig.JSON tiedostosta, voit ottaa yhteyttä klusterin noudattamalla artikkelin [luominen Azure palvelun kangasta-klusterin paikallisen tai pilvipalveluun](service-fabric-cluster-creation-for-windows-server.md) ja siirry sitten [kesäolympialaisten visualisointi yhteyttä klusterin kangasta Resurssienhallinnassa](service-fabric-visualizing-your-cluster.md).


