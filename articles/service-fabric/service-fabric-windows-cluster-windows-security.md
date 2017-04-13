<properties
   pageTitle="Suojattu käyttämällä Windows-suojauksen Windows-käyttöjärjestelmässä klusterin | Microsoft Azure"
   description="Opettele määrittämään solmu solmu ja asiakkaan solmu tietoturvan erillinen klusterissa käyttämällä Windows-suojauksen Windows-käyttöjärjestelmässä."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="secure-a-standalone-cluster-on-windows-using-windows-security"></a>Suojattu erillinen klusterin Windows käyttämällä Windowsin suojaus

Voit estää näin luvattoman pääsyn palvelun kangasta klusteriin on suojattu, erityisesti silloin, kun se on käytössä olevat tuotannon toiminnoista. Tässä artikkelissa kerrotaan, miten voit käyttää Windowsin suojaus- *ClusterConfig.JSON* tiedoston solmu solmu ja asiakkaan solmu suojauksen määrittäminen, ja määritä suojaus vaiheeseen [Luo erillisen klusterin käytössä Windows](service-fabric-cluster-creation-for-windows-server.md)vastaa. Saat lisätietoja siitä, miten palvelun kangasta käyttää Windowsin suojaus- [klusterin skenaarioita](service-fabric-cluster-security.md).

>[AZURE.NOTE]
Suojauksen valinnan solmu solmu arvopaperille kannattaa harkita huolellisesti, koska ei ole mitään klusterin päivitystä yhden suojauksen valinta toiseen. Suojaus-valinnan muuttaminen edellyttäisi koko klusterin muodosta.

## <a name="configure-windows-security"></a>Windows-suojauksen määrittäminen
Esimerkki *ClusterConfig.Windows.JSON* kokoonpanotiedosto ladata kanssa [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) erillisversion klusterin paketti sisältää mallin määrittäminen Windowsin suojaus.  Windows-suojausasetukset on määritetty **Ominaisuudet** -osassa:

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

|**Hakumäärityksen asetus**|**Kuvaus**|
|-----------------------|--------------------------|
|ClusterCredentialType|Windowsin suojaus on käytössä *Windows* **ClusterCredentialType** -parametrin määrittämällä.|
|ServerCredentialType|Windowsin suojaus-asiakkaiden on käytössä *Windows* **ServerCredentialType** -parametrin määrittämällä. Tämä ilmaisee, että klusterin ja klusterin itse asiakkaat ovat käynnissä Active Directory-toimialueen sisällä.|
|WindowsIdentities|Sisältää klusterin ja asiakkaan käyttäjätietojen.|
|ClusterIdentity|Määrittää solmu solmu suojaus. Pilkuilla erotettu luettelo ryhmän hallittua palvelutilejä tai nimiksi.|
|ClientIdentities|Määrittää asiakkaan solmu suojaus. Asiakkaan käyttäjätilit matriisi.|
|Käyttäjätiedot|Asiakas-tunnistetiedot toimialuekäyttäjä.|
|IsAdmin|TOSI määrittää, että toimialuekäyttäjä on järjestelmänvalvojan client access-asiakkaan käytön EPÄTOSI.|

[Solmun solmu suojaus](service-fabric-cluster-security.md#node-to-node-security) on määritetty käyttämällä **ClusterIdentity**-asetus. Jotta voit luoda luottamussuhteet solmujen välillä, ne on tehtävä tietoinen toisiinsa. Tämä onnistuu kahdella eri tavalla: Määritä ryhmän hallitun palvelutilin, joka sisältää kaikki solmut klusterin tai kaikkien solmujen toimialueen solmu-käyttäjätietojen klusterin. On suositeltavaa käyttämällä [Ryhmän hallitun palvelutilin (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) -vaihtoehto, erityisesti suurempi varausyksiköt (yli 10 solmujen) tai klustereiden, jotka todennäköisesti Suurenna tai Pienennä.
Tämän menetelmän avulla voi lisätä tai poistaa gMSA-klusterin luettelo muutokset tarvitsematta solmut. Tämän menetelmän ei tarvita, jolle on myönnetty klusterin järjestelmänvalvojat käyttöoikeudet, voit lisätä ja poistaa jäseniä toimialueryhmän luomisen. Lisätietoja on artikkelissa [Ryhmän hallittuja palvelutilejä käytön aloittaminen](http://technet.microsoft.com/library/jj128431.aspx).

[Asiakkaan solmu suojaus](service-fabric-cluster-security.md#client-to-node-security) on määritetty käyttämällä **ClientIdentities**. Asiakkaan ja klusterin välinen luottamussuhde vahvistamiseksi sinun on määritettävä klusterin tietää, mitkä asiakkaan käyttäjätietoja, jotka voit luottaa. Tämä voi tehdä kahdella eri tavalla: Määritä, joka yhdistää tai määrittää toimialueen solmu käyttäjät, jotka voivat liittyä toimialueen ryhmän käyttäjät. Palvelun kangasta tukee kaksi eri Ohjausobjektityypit asiakkaille, jotka on liitetty palvelun kangasta klusterin: järjestelmänvalvoja ja käyttäjä. Käyttöoikeuksien hallinta mahdollistaa klusterin järjestelmänvalvojan käyttörajoitukset tietyntyyppisiä klusterin eri käyttäjäryhmille, klusterin parantaminen toimintoja.  Järjestelmänvalvojilla on täydet oikeudet hallintatoiminnot (mukaan lukien luku-/ kirjoitusoikeudet ominaisuudet). Käyttäjien on oletusarvoisesti vain lukuoikeudet hallintatoiminnot (esimerkiksi Kyselyominaisuudet) ja mahdollisuuden pitää korjata sovelluksia ja palveluja.

Esimerkki **Suojaus** -osa määrittää Windowsin suojaus ja määrittää, että *ServiceFabric/clusterA.contoso.com* koneet ovat osa klusterin ja *CONTOSO\usera* on järjestelmänvalvojan client access:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
        "IsAdmin": true
        }]
    }
},
```

## <a name="next-steps"></a>Seuraavat vaiheet

Kun Windowsin suojaus *ClusterConfig.JSON* -tiedostossa, jatka klusterin luontiprosessi- [Luo erillisen klusterin käytössä Windows](service-fabric-cluster-creation-for-windows-server.md).

Lisätietoja siitä, miten solmu solmu suojaus, asiakkaan solmu suojaus ja Roolipohjainen käyttöoikeuksien valvonta-kohdassa [klusterin skenaarioita](service-fabric-cluster-security.md).

Artikkelissa [etäyhteyden muodostaminen suojatun klusterin](service-fabric-connect-to-secure-cluster.md) esimerkkejä yhteyden PowerShell tai FabricClient avulla.
