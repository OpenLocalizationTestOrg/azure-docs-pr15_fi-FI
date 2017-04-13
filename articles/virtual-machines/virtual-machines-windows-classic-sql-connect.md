<properties
    pageTitle="Yhteyden muodostaminen SQL Server Virtual kone (perinteinen) | Microsoft Azure"
    description="Lue, miten voit muodostaa yhteyden SQL Serveriä Virtual tietokoneeseen Azure-tietokannassa. Tässä ohjeaiheessa käyttää perinteinen käyttöönottomalli. Skenaariot vaihtelevat sen mukaan, verkon määritysten ja asiakkaan sijainti."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Yhteyden muodostaminen SQL Server Virtual kone-Azure (perinteinen käyttöönotosta)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-connect.md)
- [Perinteinen](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa kerrotaan, miten voit muodostaa yhteyden SQL Server-esiintymä Azure virtuaalikoneen käytössä. Se käsitellään joitakin [yleisiä connectivity skenaariot](#connection-scenarios) ja sisältää [yksityiskohtaiset ohjeet Azure-AM SQL Server-yhteyksien määrittäminen](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Jos käytössäsi on Resurssienhallinta VMs, ohjeaiheessa [yhteyden, SQL Server Virtual tietokoneen Azure resurssien hallinnan avulla](virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Yhteysskenaariot

Tapa, jolla asiakas muodostaa yhteyden SQL Serveriä Virtual Machine eroaa asiakkaan ja tietokoneen/verkko-määritysten sijainnin mukaan. Näissä tilanteissa ovat seuraavat:

- [Yhteyden muodostaminen SQL Server saman pilvipalvelussa](#connect-to-sql-server-in-the-same-cloud-service)
- [Yhteyden muodostaminen SQL Serverin Internetin välityksellä](#connect-to-sql-server-over-the-internet)
- [Yhteyden muodostaminen SQL Server virtual samassa verkostossa](#connect-to-sql-server-in-the-same-virtual-network)

>[AZURE.NOTE] Ennen kuin yhdistät jollakin seuraavista tavoista, sinun on noudatettava, [Voit määrittää yhteyden tämän artikkelin vaiheita](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Yhteyden muodostaminen SQL Server saman pilvipalvelussa

Useita näennäiskoneiden voi luoda saman pilvipalvelussa. Tässä skenaariossa näennäiskoneiden on artikkelissa [yhteyden näennäiskoneiden virtual verkko- tai cloud-palvelun kanssa](virtual-machines-windows-classic-connect-vms.md#connect-vms-in-a-standalone-cloud-service). Tässä skenaariossa on, kun asiakas virtual-koneessa yrittää muodostaa yhteyden SQL Serveriä virtual käyttäjäprofiilista saman pilvipalvelussa.

Tässä skenaariossa voit muodostaa yhteyden AM **nimi** (myös näkyy **Tietokonenimi** tai **isäntänimi** portaalissa). Tämä on antamasi AM luonnin aikana nimi. Esimerkiksi jos SQL AM- **mysqlvm**nimi, asiakkaan AM saman pilvipalvelussa käyttää seuraavaa yhteysmerkkijonoa ja yhteyden:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Yhteyden muodostaminen SQL Serverin Internetin välityksellä

Jos haluat muodostaa yhteyden SQL Server-tietokantamoduuli Internetin kautta, sinun on luotava virtuaalikoneen-päätepisteen saapuvan TCP-viestintään. Azure määritysten tässä vaiheessa ohjaa saapuvan TCP-portin tietoliikenteen virtuaalikoneen käytettävissä oleva porttinumeroa.

Muodostaa Internetin kautta, sinun on käytettävä AM DNS-nimen ja AM päätepisteen porttinumero (määritetty jäljempänä). Etsi DNS-nimi, siirry Azure-portaaliin ja valitse **näennäiskoneiden (perinteinen)**. Valitse virtuaalikoneen. **DNS-nimi** näkyy **Yleiskatsaus** -osan.

Esimerkkinä perinteinen virtual-koneen nimeltä **mysqlvm** **mysqlvm7777.cloudapp.net** DNS-nimen ja **57500**AM päätepisteen. Jos yhteys on määritetty oikein, seuraavat yhteysmerkkijonon avulla voi käyttää virtuaalikoneen missä tahansa Internetissä:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Vaikka näin connectivity asiakkaiden Internetin kautta, tämä ei tarkoita, että kuka tahansa muodostaa yhteyden SQL Server. Ulkopuolisten asiakkaiden on oikea käyttäjänimi ja salasana. Lisäsuojauksen Älä käytä tunnetun porttia 1433 julkisen virtuaalikoneen päätepiste. Ja jos mahdollista, lisäämällä Käyttöoikeusluettelon päätepiste rajoittaa liikenne vain asiakkaat, jolle myönnät käyttöoikeudet. Ohjeita päätepisteet käyttöoikeusluettelot käyttämisestä on artikkelissa [Hallitse päätepisteen Käyttöoikeusluettelon](virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

>[AZURE.NOTE] On tärkeää muistaa, kun käytät tätä tapaa pitää yhteyttä SQL Server Azure palvelinkeskuksen kaikkien lähtevien tietojen peritään [hinnoittelu lähtevä tiedonsiirron](https://azure.microsoft.com/pricing/details/data-transfers/)Normaali.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Yhteyden muodostaminen SQL Server virtual samassa verkostossa

[VPN](../virtual-network/virtual-networks-overview.md) mahdollistaa Lisää skenaarioita. Voit muodostaa VMs samaan virtual verkkoon, vaikka ne VMs olevia eri pilvipalveluihin. Ja [sivuston sivuston VPN-yhteyttä](../vpn-gateway/vpn-gateway-site-to-site-create.md), voit luoda hybrid-arkkitehtuuri, joka yhdistää VMs paikallisen verkkojen ja tietokoneissa.

Virtuaalinen verkkojen avulla voit liittää Azure VMs toimialueelle. Tämä on ainoa tapa käyttää Windows-todennusta SQL Server-tietokantaan. Yhteysskenaariot edellyttävät SQL-todennusta käyttäjänimet ja salasanat.

Jos aiot määrittää toimialueen käyttäjät ja Windows-todennusta, sinun ei tarvitse määrittää julkisen päätepisteen tai SQL-todennusta ja kirjautumiset tämän artikkelin vaiheiden avulla. Tässä skenaariossa voit muodostaa yhteyden SQL Server-esiintymän määrittämällä yhteysmerkkijonon SQL Server AM nimi. Seuraavassa esimerkissä oletetaan, että myös Windows-todennus on määritetty ja että käyttäjä on myönnetty access SQL Server-esiintymän.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Ohjeet määrittäminen yhteys SQL Server Azure-AM

Seuraavasti näytetään, miten voit muodostaa yhteyden SQL Server-esiintymän SQL Server Management Studiossa (SSMS) Internetin välityksellä. Kuitenkin saman vaiheet koskevat vain tekeminen SQL Server-virtuaalikoneen helppokäyttöisten sovellusten, paikallisen molemmat käynnissä ja Azure-tietokannassa.

Ennen kuin voit muodostaa yhteyden SQL Server-esiintymän toiseen AM tai internet-kuvatulla tavalla tapoja, joilla on tehtävä seuraavat toimet:

- [Luo TCP päätepiste virtuaalikoneen varten](#create-a-tcp-endpoint-for-the-virtual-machine)
- [Avaa Windowsin palomuuri TCP-portit](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server kuunnella TCP-protokollaa määrittäminen](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [SQL Server määrittää Monijärjestelmätila todennusta varten](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-todennus kirjautumiset luominen](#create-sql-server-authentication-logins)
- [Virtuaalikoneen DNS nimen määrittämiseen](#determine-the-dns-name-of-the-virtual-machine)
- [Yhteyden muodostaminen tietokantamoduuli toisesta tietokoneesta](#connect-to-the-database-engine-from-another-computer)

Seuraavassa kaaviossa on yhteenveto yhteyden polku:

![Yhteyden muodostaminen SQL Server-virtual-tietokoneeseen](../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Classic Steps](../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Jos aiot myös käyttää AlwaysOn käytettävyys ryhmiä hyvän käytettävyyden ja tietojen palauttaminen, ota huomioon käyttöönoton kuuntelija. Tietokannan asiakastietokoneet muodostavat yhteyden kuuntelua sijaan suoraan hiirellä yhtä suorakulmion SQL Server-esiintymät. Kuuntelutoiminto reitittää asiakkaiden ensisijainen se käytettävyys-ryhmässä. Lisätietoja on artikkelissa [määrittäminen ILB-listener AlwaysOn käytettävyys ryhmien Azure-tietokannassa](virtual-machines-windows-classic-ps-sql-int-listener.md).

On tärkeää haluat tarkastella kaikkia suojauksen parhaiden käytäntöjen SQL Server Azure virtuaalikoneen käytössä. Lisätietoja on artikkelissa [SQL Serverin Azuren näennäiskoneiden suojaustietoja](virtual-machines-windows-sql-security.md).

[Tutustu Oppimispolkuun](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) Azuren näennäiskoneiden SQL Server. 

Katso muita aiheeseen liittyviä suorittamalla SQL Server Azure VMs:, [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).
