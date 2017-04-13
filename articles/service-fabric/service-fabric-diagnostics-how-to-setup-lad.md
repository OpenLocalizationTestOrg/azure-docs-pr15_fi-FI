<properties
   pageTitle="Lokien kerääminen Linux Azure Diagnostiikan avulla | Microsoft Azure"
   description="Tässä artikkelissa käsitellään Azure diagnostiikka määrittämiseen lokit kerätään palvelun kangasta Linux-klusterin Azure käynnissä."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Lokien kerääminen Azure Diagnostiikan avulla

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Kun käytät Azure palvelun kangasta-klusterin-on hyvä lokit kerätään kaikki solmut keskitetyssä paikassa. Ottaa lokit keskitetyssä sijainnissa, on helppo analysoida ja vianmääritys, olivatpa ne sitten palvelujen, sovelluksen tai klusterin itse. Lataa ja lokien kerääminen yksi tapa on Azure diagnostiikka-tunniste, jossa Lataa lokit Azure-tallennustilan. Voit lukea tapahtumat tallennustilan ja sijoittaa ne tuotteissa, kuten [Joustavasti haun](service-fabric-diagnostic-how-to-use-elasticsearch.md) tai lokin jäsennyksen ratkaisu.

## <a name="log-sources-that-you-might-want-to-collect"></a>Log-lähteistä, jonka haluat kerätä
- **Palvelun kangasta lokit**: peräisin kautta [LTTng](http://lttng.org) ympäristö ja ladata tallennustilan-tiliisi. Lokeja voi olla toiminnallisia tapahtumia tai runtime tapahtumia, jotka platform tietokoneesta kuuluu äänimerkki. Lokitiedostot tallennetaan sijaintiin, joka määrittää klusterin luettelo. (Saat tallennustilan tilin tiedot-tunniste **AzureTableWinFabETWQueryable** hakeminen ja tarkistamalla **StoreConnectionString**.)
- **Sovelluksen tapahtumien**: oman palvelukoodi peräisin. Voit käyttää mitä tahansa kirjaaminen-ratkaisun, joka kirjoittaa tekstipohjainen lokitiedostojen – esimerkiksi LTTng. Lisätietoja on ohjeissa LTTng jäljitys sovelluksen.  


## <a name="deploy-the-diagnostics-extension"></a>Ottaa käyttöön diagnostiikka-tunniste
Ensimmäinen vaihe lokien kerääminen on ottaa käyttöön jokaisen palvelun kangasta klusterin VMs diagnostiikka-tunniste. Diagnostiikka-tunniste kerää kunkin AM-lokit ja lataa ne tallennustilan-tili, voit määrittää. Vaiheet määräytyvät käytätkö Azure portal- tai Azure Resurssienhallinta.

Käyttöön diagnostiikka-tunniste klusterin VMs osana klusterin luominen, määritä arvoksi **On** **Diagnostiikka** . Kun olet luonut klusterin, et voi muuttaa tätä asetusta portaalissa.

Määritä Linux Azure diagnostiikan (LAD) tiedostojen keräämiseen ja sijoittaa ne tallennustilan-tilillesi. Tämä toimenpide on selitetty tapaus 3 ("Lataa oman lokitiedostojen") on artikkelissa [Käyttämällä LAD ja vianmäärityksen Linux VMs](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md)nimellä. Pääsy jäljittää saa seuraavan prosessia. Voit ladata jäljittää visualizer mediaan.

Voit myös ottaa diagnostiikka-tunniste Azure resurssien hallinnan avulla. Prosessi on samanlainen kuin Windowsin ja Linux ja on kuvattu Windows [Azure diagnostiikka lokien kerääminen](service-fabric-diagnostics-how-to-setup-wad.md)varausyksiköt.

Voit myös toimintojen hallinta-ohjelmistopaketti, [Toimintojen hallinta Suite Log Analytics Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/)kuvatulla tavalla.

Kun olet tehnyt nämä määritykset, LAD-agentti valvoo määritetyn lokitiedostot. Aina, kun tiedosto lisätään uusi rivi, Luo syslog merkintä, joka lähetetään, jonka olet määrittänyt tallennustilan.


## <a name="next-steps"></a>Seuraavat vaiheet
Tarkemmin mitä tapahtumia, voit tarkastella olisi vianmääritys on artikkelissa [LTTng dokumentaation](http://lttng.org/docs) ja [Käyttää LAD](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
