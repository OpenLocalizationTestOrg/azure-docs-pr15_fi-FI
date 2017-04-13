<properties
   pageTitle="Palvelun kangasta käänteisen välityspalvelimen | Microsoft Azure"
   description="Käytä palvelun kangasta käänteisen välityspalvelimen microservices ja sivuihin klusterin tietoliikenne"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Palvelun kangasta käänteisen välityspalvelimen

Kangasta käänteisen välityspalvelimen on käänteisen välityspalvelimen laadittuihin kyselyjä palvelun kangasta, joka sallii osoitteiden microservices palvelun kangasta klusterin, joka näyttää HTTP päätepisteet.

## <a name="microservices-communication-model"></a>Microservices viestintä-malli

Palvelun kangasta Microservices suorittaa AM päivän alijoukkoa klusterin tavallisesti ja voit siirtää yhden AM toiseen eri syistä. Jotta microservices päätepisteet muuttaa dynaamisesti. Tyypillinen kuvio microservice viestintä on alla, ratkaise tapahtuu

1. Ratkaise alun perin nimeäminen-palvelun kautta service-sijainti.
2. Yhdistä palveluun.
3. Epäonnistuneita syy ja ratkaise uudelleen palvelun sijainti tarvittaessa.

Tässä prosessissa on yleensä rivitys asiakkaan viestintä kirjastojen uudelleen silmukka, joka sisältää palvelun tarkkuutta ja yritä uudelleen käytännöt.
Lisätietoja tämän artikkelin kohdassa [palvelujen yhteydessä](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Viestintä SF käänteisen välityspalvelimen kautta
Palvelun kangasta käänteisen välityspalvelimen suoritetaan-klusterin solmut. Se suorittaa koko palvelun ratkaisuprosessin asiakkaan puolesta ja välittää pyyntö. Klusterin asiakkaat voivat niin luoda kirjastoja asiakkaan HTTP-viestintä voit jutella kohde-palvelu käytössä paikallisesti saman solmun SF käänteisen välityspalvelimen kautta.

![Sisäisen tietoliikenteen][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Kurottavat Microservices-klusterin ulkopuolella
Microservices ulkoinen viestintä oletusmalli on **sisältyy** , joissa kunkin palvelun oletusarvoisesti ei voi käyttää suoraan ulkoiset asiakkaat. [Azure kuormituksen](../load-balancer/load-balancer-overview.md) on verkon rajan microservices ja ulkoiset asiakkaat välille, joka suorittaa NAT ja välittää sen sisäinen **IP:port** ulkoisen pyynnöt päätepisteet. Siirrä microservice päätepisteen suoraan käytettävissä ulkoiset asiakkaat Azure kuormituksen ensin on määritettynä eteenpäin-liikenne paikalliseen kunkin palvelun klusterin käyttämä portti. Lisäksi useimmissa microservices (esp. tilallisten microservices) ei live klusterin solmuissa, ja ne siirtyvät automaattisesti, valitse solmujen välillä niin tällaisissa tapauksissa Azure kuormituksen et voi tehokkaasti määrittää replikat Kohdesolmu sijaitsevat välittäminen tietoliikennettä.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Kurottavat Microservices kautta SF käänteisen välityspalvelimen klusterin ulkopuolella

Sen sijaan, että määrittäminen yksittäisten palvelujen portit azure kuormituksen, vain SF vastakkainen välityspalvelimen portti voidaan määrittää Azure ladata tasaustoiminto. Näin saavuttamiseksi services sisällä klusterin ilman muita määrityksiä käänteisen välityspalvelimen kautta asiakkaat klusterin ulkopuolella.

![Ulkoinen viestintä][0]

>[AZURE.WARNING] Micro palvelujen määrittäminen käänteisen välityspalvelimen portin kuormituksen, tekee klusterin, joka näyttää http-päätepisteen on addressible klusterin ulkopuolella.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>URI tyylissä palvelujen kautta käänteisen välityspalvelimen osoitteet

Käänteisen välityspalvelimen käyttää URI tietyssä muodossa laji mikä service-osion saapuvan pyynnön kohdesähköpostiosoitetta:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http (s):** Käänteisen välityspalvelimen voidaan määrittää hyväksymään HTTP tai HTTPS-liikenne. Jos HTTPS-liikenne SSL-tilauksen tapahtuu käänteisen välityspalvelimen. Pyynnöt, jotka siirretään käänteisen välityspalvelimen palveluihin klusterin ovat HTTP-yhteyden kautta.
 - **Klusterin FQDN | sisäinen IP:** Ulkoiset asiakkaat käänteisen välityspalvelimen voi määrittää niin, että se on tavoitettavissa klusterin toimialue (esimerkiksi mycluster.eastus.cloudapp.azure.com) kautta. Oletusarvoisesti jokaisella solmu suoritetaan käänteisen välityspalvelimen niin sisäisen tietoliikenteen se tavoittaa localhost tai minkä tahansa sisäisen solmun IP (esimerkiksi 10.0.0.1).
 - **Portti:** Portti, jota ole määritetty käänteisen välityspalvelimen. Esimerkki: 19008.
 - **ServiceInstanceName:** Tämä on täydellinen käyttöön palvelun esiintymän nimi, jonka haluat saavuttaa palvelun "kangasta: /" värimallin. Esimerkiksi saavuttamiseksi palvelun *kangasta: / myapp/myservice/*, käytetään *myapp/myservice*.
 - **Jälkiliite polku:** Tämä on palvelu, johon haluat muodostaa yhteyden todellinen URL-polku. Esimerkiksi *myapi/arvot ja lisätä/3*
 - **PartitionKey:** Osioitu palvelun tämä on haluat saavuttaa osion laskettu osio-näppäintä. Huomaa, että kyseessä *ei ole* osion tunnus GUID-tunnus. Tämä parametri ei ole pakollinen-palveluita käyttämällä Yksiarvoinen osion värimallin.
 - **PartitionKind:** Palvelun osion malli. Tämä voi olla "Int64Range" tai "Nimi". Tämä parametri ei ole pakollinen-palveluita käyttämällä Yksiarvoinen osion värimallin.
 - **Aikakatkaisu:**  Määrittää luoma puolesta pyyntö palveluun käänteisen välityspalvelimen http-pyynnön aikakatkaisun. Tämä oletusarvo on 60 sekuntia. Tämä on valinnainen parametri.

### <a name="example-usage"></a>Esimerkki

Esimerkkinä voit palvelun **kangasta: / MyApp/MyService** avaavan HTTP-kuuntelutoiminnon seuraavaa URL-osoitetta:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Kanssa on seuraavissa resursseissa:

 - `/index.html`
 - `/api/users/<userId>`

Jos palvelua käytetään Yksiarvoinen osiointimalli, *PartitionKey* ja *PartitionKind* merkkijonon kyselyparametrit eivät ole pakollisia ja palvelun tavoittaa kuin yhdyskäytävän kautta:

 - Ulkoisesti:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Sisäisesti:`http://localhost:19008/MyApp/MyService`

Jos palvelua käytetään yhtenäisen Int64 osiointimalli, *PartitionKey* ja *PartitionKind* kyselyn merkkijono-parametrit on käytettävä palvelun osion saavuttaa:

 - Ulkoisesti:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Sisäisesti:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Saavuttaa palvelun liittyvät resurssit, sijoita resurssipolku yksinkertaisesti jälkeen palvelun URL-nimi:

 - Ulkoisesti:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Sisäisesti:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Yhdyskäytävän sitten välittää nämä pyynnöt palvelun URL-osoitteeseen:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Käsitellä portin jakamiseen palvelut

Sovelluksen yhdyskäytävän yrittää ratkaista uudelleen palvelun osoite ja yritä pyynnön, kun palvelua ei voi siirtyä. Tämä on yksi merkittäviä etuja yhdyskäytävää, koska asiakas-koodia ei tarvitse toteuttaa palvelun omassa tarkkuutta ja ratkaista silmukan.

Kun palvelu ei voi siirtyä se tarkoittaa yleensä esiintymää tai replika siirtyvät eri solmu osana sen Normaali elinkaaren aikana. Tällöin yhdyskäytävän saattaa tulla ilmaisee, että päätepisteen ei ole enää Avaa alun perin ratkaistu osoitteen verkon yhteysvirhe.

Kuitenkin replikoiden tai palvelun esiintymät, voit jakaa host-prosessi ja voi jakaa myös portti, kun ylläpitämä HTTP-pohjainen web-palvelimeen, mukaan lukien:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.NET-Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

Tässä tilanteessa on todennäköistä, että verkkopalvelin on käytettävissä host-prosessi ja pyyntöihin, mutta ratkaistu esiintymää tai replika ei ole enää käytettävissä isännän. Tässä tapauksessa yhdyskäytävän saavat HTTP 404-vastauksen verkkopalvelin. HTTP-404 tuloksena on kaksi eri merkitykset:

 1. Palvelun osoite on oikein, mutta käyttäjän pyytämä resurssia ei ole.
 2. Palvelun osoite on väärä ja käyttäjän pyytämä resurssi voi todella olemassa eri solmun.

Ensimmäinen tapauksessa tämä on tavallinen HTTP-404, jonka katsotaan käyttäjän virhe. Kuitenkin toisen tapauksessa käyttäjän pyytämän resurssi, joka ole, mutta yhdyskäytävän ei löydä sitä, koska palvelun itse on siirretty, jolloin yhdyskäytävä on oltava ratkaisemaan osoitetta uudelleen ja yritä sitten uudelleen.

Yhdyskäytävä on näin tapa erottaa kummassakin tapauksessa. Jotta voit varmistaa, että ero, tarvitaan vihjeteksti palvelimesta.

 - Oletusarvon mukaan sovelluksen yhdyskäytävän olettaa tapaus #2 ja yrittää ratkaista uudelleen ja antaa uudelleen pyynnön.
 - Osoita tapaus #1 sovelluksen yhdyskäytävään, palvelun tulee palauttaa seuraavat HTTP-vastauksen otsikon:

`X-ServiceFabric : ResourceNotFound`

HTTP-vastauksen otsikon osoittaa Normaali HTTP 404 tilannetta, jossa pyydetty resurssi ei ole ja yhdyskäytävä ei yrittää ratkaista uudelleen service-osoite.

## <a name="setup-and-configuration"></a>Asennus ja määritys
Palvelun kangasta käänteisen välityspalvelimen voi ottaa käyttöön klusterin kautta [Azure Resurssienhallinta mallia](./service-fabric-cluster-creation-via-arm.md).

Kun olet, jonka haluat ottaa käyttöön klusterin mallia (joko esimerkkimallit tai luomalla mukautettuja resurssiin hallinnan malli) käänteisen välityspalvelimen käytössä mallia suorittamalla seuraavat vaiheet.

1. Määritä portti käänteisen välityspalvelimen mallin [Parametrit-osassa](../resource-group-authoring-templates.md) .

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Määritä portti kunkin **klusterin** [Resurssin laji-osasta](../resource-group-authoring-templates.md) nodetype-objektit

    Saat apiVersion ennen 2016-09-01' portti tunnistetaan parametrin nimi ***httpApplicationGatewayEndpointPort***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Saat apiVersion henkilön käyttöön tai sen jälkeen 2016-09-01' portti tunnistetaan parametrin nimi ***reverseProxyEndpointPort***

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Osoite-käänteisen välityspalvelimen azure klusterin ulkopuolella, määritys vaiheessa 1 määritetty portti **azure kuormituksen tasauspalvelun säännöt** .

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Voit määrittää SSL-varmenteita portti käänteisen välityspalvelimen-Lisää varmenteen **klusterin** [Resurssin laji-osasta](../resource-group-authoring-templates.md) httpApplicationGatewayCertificate-ominaisuus

    Saat apiVersion ennen 2016-09-01' varmenteen tunnistetaan parametrin nimi ***httpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Saat apiVersion henkilön käyttöön tai sen jälkeen 2016-09-01' varmenteen tunnistetaan parametrin nimi ***reverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Seuraavat vaiheet
 - Esimerkki HTTP services [otoksen projektin GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount)välinen yhteys.

 - [Etäproseduurikutsu puheluiden luotettavia palveluja Etäpalvelujen](service-fabric-reliable-services-communication-remoting.md)

 - [Verkko-Ohjelmointirajapinnan, joka käyttää OWIN luotettava Services-palveluissa](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-viestintä luotettavaa-palvelujen avulla](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
