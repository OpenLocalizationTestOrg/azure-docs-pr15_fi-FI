<properties
    pageTitle="Yhteyden muodostaminen SQL Server Virtual kone (Resurssienhallinta) | Microsoft Azure"
    description="Lue, miten voit muodostaa yhteyden SQL Serveriä Virtual tietokoneeseen Azure-tietokannassa. Tässä ohjeaiheessa käyttää perinteinen käyttöönottomalli. Skenaariot vaihtelevat sen mukaan, verkon määritysten ja asiakkaan sijainti."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Yhteyden muodostaminen SQL Server Virtual kone-Azure (resurssien hallinta)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-sql-connect.md)
- [Perinteinen](virtual-machines-windows-classic-sql-connect.md)

## <a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa kerrotaan, miten voit muodostaa yhteyden SQL Server-esiintymä Azure virtuaalikoneen käytössä. Se käsitellään joitakin [yleisiä connectivity skenaariot](#connection-scenarios) ja sisältää [yksityiskohtaiset ohjeet Azure-AM SQL Server-yhteyksien määrittäminen](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]perinteinen käyttöönottomalli. Tämän artikkelin perinteinen versio on artikkelissa [yhteyden, SQL Server Virtual tietokoneen Azure perinteinen](virtual-machines-windows-classic-sql-connect.md).

Jos haluat käyttää koko hallintapaketteihin valmistelu ja yhteyden, katso [valmistelu SQL Server Virtual Machine Azure](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Yhteysskenaariot

Tapa, jolla asiakas muodostaa yhteyden SQL Serveriä Virtual Machine eroaa asiakkaan ja tietokoneen/verkko-määritysten sijainnin mukaan. Näissä tilanteissa ovat seuraavat:

- [Yhteyden muodostaminen SQL Serverin Internetin välityksellä](#connect-to-sql-server-over-the-internet)
- [Yhteyden muodostaminen SQL Server virtual samassa verkostossa](#connect-to-sql-server-in-the-same-virtual-network)

### <a name="connect-to-sql-server-over-the-internet"></a>Yhteyden muodostaminen SQL Serverin Internetin välityksellä

Jos haluat muodostaa yhteyden SQL Server-tietokantamoduuli Internetistä, on pakollinen, kuten palomuurin määrittämisestä, SQL-todennuksen ottaminen käyttöön ja määrittäminen oman verkon käyttöoikeusryhmän on oltava verkon käyttöoikeusryhmän sääntö, joka sallii TCP-liikenne porttia 1433 useita vaiheita.

Jos käytät portaalin valmisteleminen SQL Server-virtuaalikoneen Kuva Resurssienhallinta, nämä vaiheet ovat valmiiksi valitessasi SQL-yhteystapa **julkisen** :

![Julkinen SQL-yhteystapa valmistelun aikana.](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Jos tämä ei ole yhtä valmistelun aikana, valitse voit määrittää manuaalisesti ja että näennäiskoneiden SQL Server seuraamalla [ohjeet ovat artikkelissa manuaalisesta yhteys](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

>[AZURE.NOTE] SQL Server Express edition virtuaalikoneen kuva ei tule automaattisesti käyttöön TCP/IP-protokollan. Express Editionin on käytettävä SQL Serverin määritysten hallinta käyttöön [manuaalisesti TCP/IP-protokollan](#configure-sql-server-to-listen-on-the-tcp-protocol) AM luomisen jälkeen.

Kun tämä on valmis, mikä tahansa asiakas internet-yhteyden muodostaa yhteyden SQL Server-esiintymän määrittämällä joko virtuaalikoneen julkiseen IP-osoite tai määritetty IP-osoite, että DNS-otsikko. Jos SQL Server-portin 1433, sinun ei tarvitse määrittää yhteysmerkkijonon.

    "Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Vaikka näin connectivity asiakkaiden Internetin kautta, tämä ei tarkoita, että kuka tahansa muodostaa yhteyden SQL Server. Ulkopuolisten asiakkaiden on oikea käyttäjänimi ja salasana. Voit välttää tunnetun porttia 1433 suojausta. Jos määritetty SQL Server kuuntelemaan porttia 1500 ja ERISNIMI palomuurin ja verkon suojaussäännöt ryhmän, voit esimerkiksi voi yhdistää liittämällä porttinumero SMTP-palvelimen nimi seuraavan esimerkin mukaisesti:

    "Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

>[AZURE.NOTE] On tärkeää muistaa, kun käytät tätä tapaa pitää yhteyttä SQL Server Azure palvelinkeskuksen kaikkien lähtevien tietojen peritään [hinnoittelu lähtevä tiedonsiirron](https://azure.microsoft.com/pricing/details/data-transfers/)Normaali.

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Yhteyden muodostaminen SQL Server virtual samassa verkostossa

[VPN](../virtual-network/virtual-networks-overview.md) mahdollistaa Lisää skenaarioita. Voit muodostaa VMs samaan virtual verkkoon, vaikka ne VMs olemassa eri resurssin ryhmissä. Ja [sivuston sivuston VPN-yhteyttä](../vpn-gateway/vpn-gateway-site-to-site-create.md), voit luoda hybrid-arkkitehtuuri, joka yhdistää VMs paikallisen verkkojen ja tietokoneissa.

Virtuaalinen verkkojen avulla voit liittää Azure VMs toimialueelle. Tämä on ainoa tapa käyttää Windows-todennusta SQL Server-tietokantaan. Yhteysskenaariot edellyttävät SQL-todennusta käyttäjänimet ja salasanat.

Jos käytät portaalin valmisteleminen SQL Server-virtuaalikoneen Kuva Resurssienhallinta, viestintä virtual verkossa ERISNIMI palomuurisäännöt ovat asetukset valitessasi SQL-yhteystapa **Yksityinen** . Jos tämä ei ole yhtä valmistelun aikana, sitten voit määrittää manuaalisesti ja että näennäiskoneiden SQL Server seuraamalla [ohjeet ovat artikkelissa määrittäminen manuaalisesti yhteys](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm). Mutta jos aiot määrittää toimialueen käyttäjät ja Windows-todennusta, sinun ei tarvitse määrittää SQL-todennusta ja kirjautumiset tämän artikkelin vaiheiden avulla. Voit myös ei tarvitse määrittää verkko-käyttöoikeusryhmän säännöt käytön Internetin välityksellä.

>[AZURE.NOTE] SQL Server Express edition virtuaalikoneen kuva ei tule automaattisesti käyttöön TCP/IP-protokollan. Express Editionin on käytettävä SQL Serverin määritysten hallinta käyttöön [manuaalisesti TCP/IP-protokollan](#configure-sql-server-to-listen-on-the-tcp-protocol) AM luomisen jälkeen.

Oletetaan, että olet määrittänyt DNS virtual verkkoon, voit muodostaa yhteyden SQL Server-esiintymän määrittämällä yhteysmerkkijonon SQL Server AM tietokonenimi. Seuraavassa esimerkissä myös oletetaan, että myös Windows-todennus on määritetty ja että käyttäjä on myönnetty access SQL Server-esiintymän.

    "Server=mysqlvm;Integrated Security=true"

Huomaa, että tässä skenaariossa voit myös määrittää AM IP-osoite.

## <a name="steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm"></a>Ohjeet määrittäminen manuaalisesti yhteys SQL Server Azure-AM

Seuraavat vaiheet kuvaavat asetukset manuaalisesti yhteys SQL Server ‑esiintymä ja valitse halutessasi Internetin välityksellä käyttämällä SQL Server Management Studiossa (SSMS). Huomaa, että monia näistä vaiheista on valmis, voit valita haluamasi SQL Server-yhteyden asetukset-portaalissa.

Ennen kuin voit muodostaa yhteyden SQL Server-esiintymän toiseen AM tai internet-kuvatulla tavalla tapoja, joilla on tehtävä seuraavat toimet:

- [Avaa Windowsin palomuuri TCP-portit](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
- [SQL Server kuunnella TCP-protokollaa määrittäminen](#configure-sql-server-to-listen-on-the-tcp-protocol)
- [SQL Server määrittää Monijärjestelmätila todennusta varten](#configure-sql-server-for-mixed-mode-authentication)
- [SQL Server-todennus kirjautumiset luominen](#create-sql-server-authentication-logins)
- [Määrittää verkko-käyttöoikeusryhmän saapuvien säännön](#configure-a-network-security-group-inbound-rule-for-the-vm)
- [Määritä DNS-selitteen julkiseen IP-osoite](#configure-a-dns-label-for-the-public-ip-address)
- [Yhteyden muodostaminen tietokantamoduuli toisesta tietokoneesta](#connect-to-the-database-engine-from-another-computer)

[AZURE.INCLUDE [Connect to SQL Server in a VM](../../includes/virtual-machines-sql-server-connection-steps.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager-nsg-rule.md)]

[AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Valmistelu connectivity seuraavasti sekä ohjeita on kohdassa [valmistelu SQL Server Virtual Machine Azure](virtual-machines-windows-portal-sql-server-provision.md).

[Tutustu Oppimispolkuun](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) Azuren näennäiskoneiden SQL Server.

Katso muita aiheeseen liittyviä suorittamalla SQL Server Azure VMs:, [SQL Server-Azuren näennäiskoneiden](virtual-machines-windows-sql-server-iaas-overview.md).
