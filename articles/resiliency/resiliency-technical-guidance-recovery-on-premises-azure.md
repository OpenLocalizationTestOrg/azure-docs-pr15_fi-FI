<properties
   pageTitle="Tekniset ohjeet: paikallisen palauttamisesta Azure | Microsoft Azure"
   description="Artikkeli tietoja ja suunnittelemisesta palautus Azure paikallisen infrastruktuuri-järjestelmät"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-on-premises-to-azure"></a>Azure vikasietoisuudelle tekniset ohjeet: paikallisen palauttamisesta Azure

Azure tarjoaa kattavaa palvelujen ottaminen käyttöön paikalliseen palvelinkeskukseen Azure tunniste suuri käytettävyys ja tietojen palauttamista varten:

* __Verkko__: näennäisen yksityisverkon kanssa suojatusti laajentaa paikallisen verkon pilveen.
* __Laske__: Jos käytössä on Hyper-V paikallisen voit "Nosta ja vaihto" aiemmin, Azuren näennäiskoneiden (VMs).
* __Tallennustilan__: StorSimple laajenee tiedostojärjestelmän Azure-tallennustilan. Azure varmuuskopion palvelun tarjoaa varmuuskopion tiedostot ja SQL-tietokantoja Azure-tallennustilan.
* __Tietokannan replikoiminen__: ryhmiin SQL Server 2014 (tai uudempi) käytettävyys, voit toteuttaa hyvin käytettävyyttä ja tietojen palauttaminen paikallisen tiedoillesi.

##<a name="networking"></a>Verkko

Azure Virtual Network avulla voit luoda loogisesti erillään osan Azure ja yhdistä se suojatusti paikalliseen palvelinkeskuksen tai yksittäisen asiakaskoneeseen IP-yhteyden avulla. Virtuaalinen verkoston kanssa voit hyödyntää skaalattava pyydettäessä infrastruktuurin Azure käyttämisessä on yhteys paikalliseen tietoja ja sovelluksia, kuten käyttöjärjestelmänä on Windows Server, keskustietokoneita ja UNIX. Saat lisätietoja [Azure verkko ohjeissa](../virtual-network/virtual-networks-overview.md) .

##<a name="compute"></a>Laske

Jos käytät Hyper-V paikallisen, voit "Nosta ja vaihto" olemassa, Azuren näennäiskoneiden ja käytössä Windows Server 2012 (tai uudempi) ilman muutosten tekeminen AM tai muuntaminen AM muotoilee palvelut. Lisätietoja on artikkelissa [levyille ja Azure-virtuaalikoneissa näennäiskiintolevyjen](../virtual-machines/virtual-machines-linux-about-disks-vhds.md).

##<a name="azure-site-recovery"></a>Azure sivuston palauttaminen

Jos haluat palauttaminen serviceksi (DRaaS), Azure on [Azure palauttaminen](https://azure.microsoft.com/services/site-recovery/). Azure sivuston palautus on täydellinen suojaus VMware, Hyper-V ja fyysinen palvelimia. Azure palauttaminen, jossa voit käyttää toisen paikallisen palvelimen tai Azure palautus-sivustoon. Lisätietoja Azure sivuston palautus on [Azure palauttaminen ohjeissa](https://azure.microsoft.com/documentation/services/site-recovery/).

##<a name="storage"></a>Tallennustilan

On useita vaihtoehtoja käyttämällä Azure varmuuskopion sivustojen paikalliset tiedot.

###<a name="storsimple"></a>StorSimple

StorSimple suojatusti ja läpinäkyvä integroi paikallisen sovellusten pilvitallennustilaa. Se on myös yksi laite, joka tarjoaa tehokas Porrastettu paikallisen ja pilvitallennustilaa, live arkistointi, pilvipohjainen tietojen suojaaminen ja palauttaminen. Lisätietoja on artikkelissa [StorSimple product-sivu](https://azure.microsoft.com/services/storsimple/).

###<a name="azure-backup"></a>Azure varmuuskopiointi

Azure varmuuskopiointi mahdollistaa cloud varmuuskopiot käyttämällä tuttuja työkaluja varmuuskopion, Windows Server 2012 (tai uudempi), Windows Server 2012 Essentials (tai uudempi) ja System Center 2012 tietojen suojauksen hallinta (tai uudempi). Kyseisten työkalujen avulla työnkulun varmuuskopion, joka on erillinen tallennuspaikan varmuuskopioita, onko paikalliseen levyasemaan tai Azure-tallennustilan hallinta. Kun tiedot varmuuskopioidaan pilveen, valtuutettujen käyttäjien helposti palauttaa mihin tahansa palvelimeen varmuuskopiot.

Lisäävän varmuuskopioinnin vain tiedostojen muutokset on siirretty pilveen. Tämä auttaa tehokkaasti käyttämällä tallennustilaa, vähentää kaistanleveyden käyttöä ja tukevat ajankohta palauttamista, tiedot on useita versioita. Voit valita myös muita ominaisuuksia, kuten tietojen säilytyskäytännöt, tietojen pakkaus ja tiedonsiirto rajoitusta. Käytä Azure varmuuskopion sijainti on ilmeisimmät etu varmuuskopioista ovat automaattisesti "käyttö etäyhteyden kautta". Näin varmentaminen ja suojaaminen paikalla varmuuskopion media ylimääräisiä vaatimukset.

Lisätietoja on artikkelissa [Azure varmuuskopiointi ominaisuudet?](../backup/backup-introduction-to-azure-backup.md) ja [Määritä Azure varmuuskopion DPM tiedoille](https://technet.microsoft.com/library/jj728752.aspx).

##<a name="database"></a>Tietokannan

Voit antaa tietojen palauttaminen ratkaista SQL Server-tietokannat hybrid IT-ympäristössä AlwaysOn käytettävyys ryhmät, tietokantapeilausta, toimitus log ja varmuuskopiointi ja palauttaminen ja Azure-Blob-säiliö. Kaikki nämä ratkaisut käyttävät-Azuren näennäiskoneiden SQL Serveriä.

AlwaysOn käytettävyys ryhmät voidaan hybrid IT-ympäristössä, jossa Tietokantareplikoita ole paikallisen sekä ja pilvessä. Seuraavassa kaaviossa näytetään.

![SQL Server AlwaysOn käytettävyys ryhmien hybrid cloud-arkkitehtuuri](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-3.png)

Tietokantapeilausta voi myös olla paikallisen palvelimia ja pilveen varmenne-asetuksissa. Seuraavassa kaaviossa on kuvattu tämän käsitteen.

![SQL Server hybrid cloud-arkkitehtuuri tietokantapeilausta](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-4.png)

Lokitiedoston toimitus voidaan paikallisen tietokannan synkronointiin Azure virtuaalikoneen SQL Server-tietokannassa.

![SQL Server-loki toimitus hybrid cloud-arkkitehtuuri](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-5.png)

Lopuksi voit varmuuskopioida paikallisen tietokannan suoraan Azure-Blob-säiliö.

![Varmuuskopioi SQL Server Azure-Blob-säiliö-hybrid cloud-arkkitehtuuri](./media/resiliency-technical-guidance-recovery-on-premises-azure/SQL_Server_Disaster_Recovery-6.png)

Lisätietoja on artikkelissa [hyvin Azuren näennäiskoneiden SQL Server käytettävyyttä ja tietojen palauttaminen](../virtual-machines/virtual-machines-windows-sql-high-availability-dr.md) ja [Varmuuskopiointi ja palauttaminen Azuren näennäiskoneiden SQL Server](../virtual-machines/virtual-machines-windows-sql-backup-recovery.md).

##<a name="checklists-for-on-premises-recovery-in-microsoft-azure"></a>Microsoft Azure paikallisen palautuksen Tarkistusluettelot

###<a name="networking"></a>Verkko

  1. Tarkista tässä asiakirjassa verkko-osa.
  2. Yhdistä turvallisesti pilvipalveluun paikallisen VPN avulla.

###<a name="compute"></a>Laske

  1. Tutustu tämän artikkelin suorittaminen-osio.
  2. Siirrä VMs Hyper-V ja Azure välillä.

###<a name="storage"></a>Tallennustilan

  1. Tutustu tämän artikkelin tallennustilan-osio.
  2. Hyödynnä StorSimple palveluita käyttämällä pilvitallennustilaa.
  3. Käyttää Azure varmuuskopiointi-palvelun avulla.

###<a name="database"></a>Tietokannan

  1. Tutustu tämän artikkelin tietokanta-osio.
  2. Voit käyttää SQL Server Azure VMs-varmuuskopio nimellä.
  3. Määritä AlwaysOn käytettävyys ryhmät.
  4. Määritä varmenteen tietokantapeilaus.
  5. Käytä log toimitus.
  6. Varmuuskopioi Azure-Blob-säiliö paikallisen tietokannan.

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on kohdistettu [Azure vikasietoisuudesta tekniset ohjeet](./resiliency-technical-guidance.md)sarjaan kuuluvan. Seuraava artikkeli sarjassa on [palautus tietovirheitä tai vahingossa](./resiliency-technical-guidance-recovery-data-corruption.md).