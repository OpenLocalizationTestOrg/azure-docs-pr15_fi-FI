<properties
   pageTitle="Lisää tai poista solmujen erillisen palvelun kangasta klusteriin | Microsoft Azure"
   description="Lue, miten voit lisätä tai poistaa Azure-palvelu kangasta-klusterin fyysiset tai virtual machine käytössä Windows Server, jotka voivat olla paikallisen tai minkä tahansa cloud solmut."
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
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Lisää tai poista solmujen erillisen palvelun kangasta klusteriin Windows Serverissä

Kun olet [luonut erillisen palvelun kangasta-klusterin Windows Server-tietokoneissa](service-fabric-cluster-creation-for-windows-server.md)tai yrityksesi tarpeisiin voivat muuttua niin, että voit joutua muuttamaan lisääminen tai poistaminen useita solmujen yhteyttä klusterin. Tässä artikkelissa on tämän tavoitteen saavuttamiseksi toimimalla seuraavasti.


## <a name="add-nodes-to-your-cluster"></a>Lisää solmujen yhteyttä klusterin

1. Valmistele AM/koneen haluat lisätä yhteyttä klusterin noudattamalla [valmisteleminen koneet täyttävät tietyt edellytykset klusterin käyttöönottoa varten](service-fabric-cluster-creation-for-windows-server.md#preparemachines) -kohdassa.
2. Mitä vian toimialueen ja Päivitä toimialueen aiot lisätä tämän AM/koneen suunnitteleminen.
3. Etätyöpöytä (RDP) AM/tietokoneessa, johon haluat lisätä klusterin kyselyjä.
4. Kopioi tai tähän AM/tietokoneeseen [Lataa palvelun kangasta Windows Server erillisversion-paketti](http://go.microsoft.com/fwlink/?LinkId=730690) ja Pura paketti.
5. Suorita Powershell järjestelmänvalvojana ja siirry sijaintiin, wsp-paketin.
6. Suorita *AddNode.ps1* Powershell kuvaava Lisää uusi solmu parametreilla. Alla olevassa esimerkissä Lisää uusi solmu nimeltä VM5, kirjoita NodeType0, IP-osoite 182.17.34.52 UD1 ja FD1. *ExistingClusterConnectionEndPoint* on yhteyden endpoint, jo aikaisemmin luodun klusterin-solmu. Tämän päätepisteen klusterin voit valita *minkä tahansa* solmun IP-osoite.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Yhteyttä klusterin solmujen poistaminen

1. Roskapostisuodattimeen määritetyn Reliablity valinnut klusterin et voi poistaa ensisijainen solmun tyyppiä ensimmäisen n (3/5/7/9) solmut
2. Keskihajonta-klusterissa RemoveNode-komennon suorittaminen ei tueta.
2. Etätyöpöytä (RDP), jonka haluat poistaa ryhmästä AM/koneen kyselyjä.
2. Kopioi tai [Lataa palvelun kangasta Windows Server erillisversion paketti](http://go.microsoft.com/fwlink/?LinkId=730690) ja Pura paketin tähän AM/tietokoneeseen.
3. Suorita Powershell järjestelmänvalvojana ja siirry sijaintiin, wsp-paketin.
4. Tee *RemoveNode.ps1* PowerShell. Alla olevassa esimerkissä poistaa nykyisen solmun klusterin. *ExistingClientConnectionEndpoint* on asiakkaan-yhteyden endpoint solmun, joka pysyy klusterin. Valitse IP-osoite ja *kaikki* **Muut solmu** päätepisteportti klusterin. **Toisen solmun** puolestaan päivittää klusterin määritykset poistetaan solmun. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Huomaa, että silloinkin, kun olet poistanut solmu, se saattaa näkyä muodossa kyselyt ja SFX että. Tämä on tunnettu virheellisten ja korjataan tulevista versiossa. 


## <a name="next-steps"></a>Seuraavat vaiheet
- [Erillinen Windows-klusterin asetukset](service-fabric-cluster-manifest.md)
- [Suojattu erillinen klusterin Windows käyttämällä Windowsin suojaus](service-fabric-windows-cluster-windows-security.md)
- [Suojattu erillinen klusterin X509 avulla Windowsiin varmenteet](service-fabric-windows-cluster-x509-security.md)
- [Windows Azure virtuaalilaitteiksi erillisen palvelun kangasta klusterin luominen](service-fabric-cluster-creation-with-windows-azure-vms.md)
