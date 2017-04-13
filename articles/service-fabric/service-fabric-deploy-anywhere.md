<properties
   pageTitle="Luo Azure palvelun kangasta klustereiden Windows Server ja Linux | Microsoft Azure"
   description="Suorita Windows Server ja Linux, mikä tarkoittaa voit voi ottaa käyttöön ja isännöidä palvelun kangasta sovelluksia missä tahansa palvelun kangasta klustereiden voit suorittaa Windows Server- tai Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Luo palvelun kangasta klustereiden Windows Server-tai Linux

Azure palvelun kangasta avulla palvelun kangasta klustereiden VMs tai Windows Server- tai Linux tietokoneita luomista varten. Tämä tarkoittaa sitä, voit ottaa käyttöön ja suorittaa palvelun kangasta sovelluksia ympäristössä, mitä tahansa kohtaa, johon sinulla on Windows Server- tai Linux-käyttöjärjestelmissä, jotka on yhdistetty joukko, voidaan paikallisen, Microsoft Azure tai minkä tahansa cloud-palvelussa.

##<a name="create-service-fabric-clusters-on-azure"></a>Luo palvelun kangasta klustereiden Azure

Klusterin luominen Azure on valmis, joko resurssin mallin mallin tai Azure portaalin kautta. Lue lisätietoja [luominen palvelun kangasta klusterin Resurssienhallinta mallin avulla](service-fabric-cluster-creation-via-arm.md) tai [luoda palvelun kangasta klusterin Azure-portaalista](service-fabric-cluster-creation-via-portal.md) .

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Tuetut käyttöjärjestelmät, varausyksiköiden Azure-

Olet voivat luoda klustereiden virtuaalilaitteiksi käyttöjärjestelmistä:

* Windows Server 2012 R2
* Windows Server 2016 (sen jälkeen, kun se on ilmoitettu kuin yleisesti saatavilla)
* Linux Ubuntu 16.04 (julkisen esikatselunäkymässä) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Luo palvelun kangasta erillinen klustereiden paikallinen tai minkä tahansa cloud-palvelussa

Palvelun kangasta sisältää Asenna-paketti, voit luoda erillisen palvelun kangasta klustereiden paikallisen tai minkä tahansa cloud-palvelun

Lisätietoja erillisen palvelun kangasta klustereiden Windows Server on määritetty Lue [Windows Server Service kangasta klusterin luominen](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Mikä tahansa cloud ominaisuuksissa ja paikallisten versioiden
Palvelun kangasta-klusterin luominen paikallisen muistuttaa minkä tahansa cloud valittua joukko VMs-klusterin luominen. Ensimmäinen vaihe vaiheelta, miten valmistelu VMs noudatettava cloud-palveluntarjoajan tai paikallisen ympäristön, jota käytät. Kun joukko VMs kanssa verkkoyhteyden käytössä välissä, sitten palvelun kangasta-paketin määrittämisen vaiheita klusterin asetusten muokkaaminen ja suorita klusterin luominen ja hallinta-komentosarjat ovat samat. Tämä varmistaa, että oman ja kokemus sekä palvelun kangasta klustereiden hallinta on voi siirtää kohteen uuden isännöintipalvelu ympäristöissä valitsemisessa.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Erillisen palvelun kangasta klustereiden luominen edut
* Voit vapaasti, voit valita minkä tahansa cloud-palvelun isännöimiseen yhteyttä klusterin.
* Palvelun kangasta sovellukset, kirjoitettu kerran, voidaan suorittaa isännöintipalvelu ympäristöissä, joissa on useita rajoitettavaa ei muutoksia.
* Palvelun kangasta sovellusten tuntemus suorittaa isännöintipalvelu ympäristöstä toiseen.
* Käynnissä ja hallintaan palvelun kangasta kokemusta klusterit suorittaa päälle ympäristöstä toiseen.
* Laaja asiakkaan saatavilla on rajoittamaton sijoittamalla ympäristön rajoitukset.
* Ylimääräisen kerroksen, luotettavuutta ja juuri katkokset suojautumista on, koska voit siirtää palveluita toiseen käyttöönotto-ympäristön keskelle tai cloud-tietopalvelu, onko pimennysaika.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Tuetut käyttöjärjestelmät erillisen klustereiden
Olet voivat luoda klustereiden VMs tai tietokoneet käyttöjärjestelmistä:

* Windows Server 2012 R2
* Windows Server 2016 (sen jälkeen, kun se on ilmoitettu kuin yleisesti saatavilla)
* Linux (tulossa)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Erillisen palvelun kangasta klusterit päällä olevien Azure palvelun kangasta varausyksiköiden edut luoda paikallisen

Palvelun kangasta klustereiden käytössä Azure antaa parempia paikallisen vaihtoehdon, joten jos sinulla ei ole erityistarpeita, jossa suoritat oman klustereiden sitten Suosittelemme, että suoritat ne Azure. Valitse Azure-annamme integrointi muihin Azure ominaisuuksia ja palveluita, jolloin toiminnot ja klusterin hallinta on entistä helpompaa ja luotettava.

* **Azure portaalissa:** Azure portaalissa on helppo luoda ja hallita klustereiden.

* **Azure Resurssienhallinta:** Käytä Azure resurssien hallinnan sallii kaikkien resurssien käyttämä klusterin yksikkönä helposti hallinnointi ja yksinkertaistaa kustannusten seuranta ja laskutukseen.
* **Palvelun kangasta klusterin Azure resurssiksi** Palvelun kangasta klusterin on yritysresurssi ARM, joten voit muovata sen, kuten tekisit KÄDESSÄ resursseihin Azure-tietokannassa.
* **Azure infrastruktuurin integrointi** Palvelun kangasta koordinaatit pohjana Azure infrastruktuuriin OS, verkon ja muut päivitykset käytettävyyden ja luotettavuuden sovellustesi parantamiseksi.  
* **Vianmääritys:** Azure, valitse annamme Azure diagnostiikka-ja Log Analytics integration.
* **Automaattinen skaalaaminen:** Annamme olevien Azure varausyksiköiden sisäistä Automaattinen skaalaus-toimintoa virtuaalikoneen asteikko-asettaa vuoksi. Paikallisen ja muut cloud-ympäristössä sinun on muodostaa oman Automaattinen skaalaaminen ominaisuus tai manuaalisesti käyttämällä API, joka paljastaa palvelun kangasta skaalauksen klustereiden asteikko.

## <a name="next-steps"></a>Seuraavat vaiheet
Luoda klusterin VMs tai tietokoneissa, joissa on käytössä Windows Server: [Windows Server Service kangasta klusterin luominen](service-fabric-cluster-creation-for-windows-server.md)

Luoda klusterin VMs tai Linux-tietokoneissa: [Palvelun kangasta Linux](service-fabric-linux-overview.md)
