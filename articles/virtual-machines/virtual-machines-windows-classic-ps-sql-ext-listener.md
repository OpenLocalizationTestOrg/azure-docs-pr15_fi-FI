<properties
    pageTitle="Määrittää ulkoisen Listener aina käyttöön käytettävyys ryhmien | Microsoft Azure"
    description="Tässä opetusohjelmassa esitellään perusvaiheet aina käyttöön käytettävyys ryhmän Listener liittyvät pilvipalvelussa julkisen Virtual IP-osoitteen avulla ulkoisesti käytettävissä oleva Azure-tietokannassa."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-an-external-listener-for-always-on-availability-groups-in-azure"></a>Määritä ulkoisen listener, aina käyttöön käytettävyys ryhmien Azure

> [AZURE.SELECTOR]
- [Sisäinen Listener](virtual-machines-windows-classic-ps-sql-int-listener.md)
- [Ulkoinen Listener](virtual-machines-windows-classic-ps-sql-ext-listener.md)

Tässä ohjeaiheessa esitellään määrittäminen kuuntelija aina käyttöön käytettävyys-ryhmän, joita voit käyttää ulkoisesti Internetistä. Tämä on mahdollista liittämällä cloud-palvelun **Julkisen Virtual IP (VIP)** osoite kuuntelua.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Käytettävyys-ryhmän voivat sisältää replikat, jotka ovat paikallisen vain ainoastaan Azure tai kattavat paikallisen ja Azure yhdistelmäympäristö. Azure replikat voivat sijaita samalla alueella tai useita alueita käyttämällä useita virtual verkkoja (VNets). Seuraavia ohjeita oletetaan on jo [määritetty käytettävyys-ryhmä](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md) , mutta ei ole määritetty kuuntelija.

## <a name="guidelines-and-limitations-for-external-listeners"></a>Ohjeita ja ulkoiset kuuntelijoita rajoituksia

Huomaa seuraavat ohjeet tietoja käytettävyys ryhmän kuuntelua Azure-tietokannassa, kun otat cloud palvelun pallot VIP osoitteen:

- Käytettävyys ryhmän kuuntelua tuetaan Windows Server 2008 R2, Windows Server 2012: ssa ja Windows Server 2012 R2.

- Asiakassovellus on sijaittava eri pilvipalvelussa kuin näkymän, joka sisältää käytettävyys-ryhmän VMs. Azure ei tue suoraan palvelimen palauttaa asiakkaan ja palvelimen kanssa samalla pilvipalvelussa.

- Tässä artikkelissa kuvattuja Näytä oletusarvon mukaan yhden listener cloud palvelun Virtual IP (VIP) osoitteen määrittäminen. On kuitenkin mahdollista varaa ja luoda useita VIP osoitteita cloud-palveluun. Voit luoda useita kuuntelutoiminnot, jotka liittyvät kuhunkin eri VIP tämän artikkelin vaiheiden avulla. Lisätietoja siitä, miten voit luoda useita VIP osoitteita on artikkelissa [Useita VIPs pilvipalvelussa kohden](../load-balancer/load-balancer-multivip.md).

- Jos olet luomassa yhdistelmäympäristön kuuntelija, paikalliseen verkkoon on oltava lisäksi sivuston sivuston VPN-yhteyttä Azure virtual verkon julkinen Internet-yhteyden. Azure aliverkon-käytettävyys ryhmän kuuntelutoiminto ei voi käyttää vain vastaavassa pilvipalvelussa julkiseen IP-osoitteen mukaan.

- Ei voi luoda ulkoisen listener saman pilvipalvelussa, jossa sisäinen listener käyttämällä sisäisiä kuormituksen tasauspalvelun (ILB) on myös.

## <a name="determine-the-accessibility-of-the-listener"></a>Määrittää kuuntelua helppokäyttötoiminnot

[AZURE.INCLUDE [ag-listener-accessibility](../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Tässä artikkelissa kerrotaan, joka käyttää **ulkoisen kuormituksen**kuuntelija luominen. Kuuntelija, joka on yksityinen virtual verkkoon halutessasi Katso tämän artikkelin ohjeaiheiden määrittämisestä [kuuntelijaa, jonka ILB](virtual-machines-windows-classic-ps-sql-int-listener.md) versiota

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Luo kuormituksen AM päätepisteet palautuksen suoraan palvelimen kanssa

Ulkoinen kuormituksen tasaamisen käyttää virtuaalisen pilvipalvelussa, joka isännöi oman VMs julkisen Virtual IP-osoite. Niin sinun ei tarvitse luoda tai määrittää kuormituksen tällöin.

[AZURE.INCLUDE [load-balanced-endpoints](../../includes/virtual-machines-ag-listener-load-balanced-endpoints.md)]

1. Kopioi alla PowerShell-komentosarjaa tekstieditorissa ja määritä muuttujien arvoihin (oletusasetusten saanut joidenkin parametrien) ympäristöön sopivaksi. Huomaa, että jos käytettävyys-ryhmän laajuinen Azure alueet, sinun on suoritettava komentosarja kerran kullekin joten pilvipalvelussa ja solmut, jotka sijaitsevat, joten.

        # Define variables
        $ServiceName = "<MyCloudService>" # the name of the cloud service that contains the availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in the same cloud service, separated by commas

        # Configure a load balanced endpoint for each node in $AGNodes, with direct server return enabled
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -Protocol "TCP" -PublicPort 1433 -LocalPort 1433 -LBSetName "ListenerEndpointLB" -ProbePort 59999 -ProbeProtocol "TCP" -DirectServerReturn $true | Update-AzureVM
        }

1. Kun olet määrittänyt muuttujat, kopioi komentosarja tekstieditorissa suorittaa kyselyjä PowerShellin Azure-istunto. Jos kehote näkyy edelleen >>, kirjoita ENTER uudelleen ja varmista, että komentosarjan suoritetaan.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Varmista, että KB2854082 on asennettu tarvittaessa

[AZURE.INCLUDE [kb2854082](../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-the-firewall-ports-in-availability-group-nodes"></a>Palomuurin porttien avaaminen käytettävyys ryhmän solmut

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-the-availability-group-listener"></a>Käytettävyys ryhmän kuuntelua luominen

[AZURE.INCLUDE [firewall](../../includes/virtual-machines-ag-listener-create-listener.md)]

1. Saat ulkoisten kuormituksen tasaamisen, sinun on hankittava pilvipalvelussa, joka sisältää oman replikoiden julkisen virtual IP-osoite. Kirjaudu sisään Azure perinteinen portaalin. Siirry pilvipalvelussa, joka sisältää käytettävyys-ryhmän AM. Avaa **raporttinäkymät-ikkunan** .

3. Huomautus **Julkisen Virtual IP (VIP)**-osoite osoite. Jos ratkaisu laajuinen VNets, toista tämä vaihe kunkin pilvipalvelussa, joka sisältää AM, joka isännöi replikan.

4. Johonkin VMs kopioi alla PowerShell-komentosarjaa tekstieditorissa ja määritä muuttujat kirjoittamasi arvot.

        # Define variables
        $ClusterNetworkName = "<ClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $CloudServiceIP = "<X.X.X.X>" # Public Virtual IP (VIP) address of your cloud service

        Import-Module FailoverClusters

        # If you are using Windows Server 2012 or higher, use the Get-Cluster Resource command. If you are using Windows Server 2008 R2, use the cluster res command. Both commands are commented out. Choose the one applicable to your environment and remove the # at the beginning of the line to convert the comment to an executable line of code.

        # Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$CloudServiceIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"OverrideAddressMatch"=1;"EnableDhcp"=0}
        # cluster res $IPResourceName /priv enabledhcp=0 overrideaddressmatch=1 address=$CloudServiceIP probeport=59999  subnetmask=255.255.255.255


1. Kerran olet määrittänyt muuttujat, avaa järjestelmänvalvojana Windows PowerShell-ikkuna, ja valitse Kopioi komentosarja tekstieditori ja liitä PowerShellin Azure-istunnon suorittaa. Jos kehote näkyy edelleen >>, kirjoita ENTER uudelleen ja varmista, että komentosarjan suoritetaan.

1. Toista tämä jokaiselle AM. Tämä komentosarja määrittää IP-osoite resurssin cloud-palvelun IP-osoite ja määrittää muut parametrit kuten näytteenottimen portti. Kun IP-osoite-resurssi on online-tilaan, valitse vastata näytteenottimen porttiin kysely päätepisteestä kuormituksen loit aiemmin tässä opetusohjelmassa.

## <a name="bring-the-listener-online"></a>Tuo kuuntelua verkossa

[AZURE.INCLUDE [Bring-Listener-Online](../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Kohteiden seuranta

[AZURE.INCLUDE [Follow-up](../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-the-availability-group-listener-within-the-same-vnet"></a>Testaa käytettävyys ryhmän kuuntelua (sisällä saman VNet)

[AZURE.INCLUDE [Test-Listener-Within-VNET](../../includes/virtual-machines-ag-listener-test.md)]

## <a name="test-the-availability-group-listener-over-the-internet"></a>Testaa käytettävyys ryhmän kuuntelua (Internetin kautta)

Jotta voit käyttää kuuntelua-virtual verkon ulkopuolella, sinun on käytettävä ulkoinen/julkisen kuormituksen tasaamisen (kuvattu tämän artikkelin) sen sijaan, että ILB, joka on vain accesible saman VNet kuluessa. Yhteysmerkkijonon Määritä cloud palvelunimi. Esimerkiksi jos haluat käyttää pilvipalvelussa nimi *mycloudservice*kanssa, sqlcmd-lause olisi seuraavasti:

    sqlcmd -S "mycloudservice.cloudapp.net,<EndpointPort>" -d "<DatabaseName>" -U "<LoginId>" -P "<Password>"  -Q "select @@servername, db_name()" -l 15

Toisin kuin edellisessä esimerkissä SQL-todennus on käytettävä, koska kutsuja ei voi käyttää windows-todennuksen Internetin välityksellä. Lisätietoja on artikkelissa [aina käyttöön käytettävyys ryhmän Azure AM: asiakkaan Connectivity skenaariot](http://blogs.msdn.com/b/sqlcat/archive/2014/02/03/alwayson-availability-group-in-windows-azure-vm-client-connectivity-scenarios.aspx). Kun SQL-todennusta, varmista, että luot saman kirjautuminen sekä replikoita. Saat lisätietoja kirjautumiset käytettävyys ryhmiin [Yhdistä kirjautumiset tai käyttämisestä sisälsi SQL-tietokannan käyttäjä Muodosta yhteys muihin replikoihin ja käytettävyyden tietokantojen yhdistäminen](http://blogs.msdn.com/b/alwaysonpro/archive/2014/02/19/how-to-map-logins-or-use-contained-sql-database-user-to-connect-to-other-replicas-and-map-to-availability-databases.aspx).

Jos aina replikat ovat eri aliverkosta, asiakkaiden on määritettävä **MultisubnetFailover = True** yhteysmerkkijonon. Tämä aiheuttaa rinnakkain yhteyksien eri aliverkosta replikoihin. Huomaa, että tässä skenaariossa rajat-alueen aina käyttöön käytettävyys ryhmän käyttöönoton.

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [Listener-Next-Steps](../../includes/virtual-machines-ag-listener-next-steps.md)]
