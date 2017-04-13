<properties
   pageTitle="Määrittää kuormituksen SQL aina | Microsoft Azure"
   description="Kuormituksen toimimaan SQL aina käyttöön ja miten voit hyödyntää powershell luo SQL-toteutus kuormituksen määrittäminen"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="configure-load-balancer-for-sql-always-on"></a>Määrittää kuormituksen SQL aina käyttöön

SQL Server AlwaysOn käytettävyys-ryhmiä voi suorittaa nyt ILB. Käytettävyys ryhmä on suuri käytettävyys ja tietojen palauttaminen SQL Server flagship ratkaisu. Käytettävyys ryhmän kuuntelua sallii sovellusten muodostaa saumattomasti ensisijainen se määrä on määrityksistä riippumatta.

Listener (DNS)-nimi on nyt yhdistetty kuormituksen IP-osoite ja Azure's kuormituksen ohjaa saapuvan liikenteen ensisijainen palvelimeen replikajoukon.

Voit käyttää SQL Server AlwaysOn (listener) päätepisteet ILB tuki. Nyt kuuntelutoiminnon helppokäyttöisyyden päättää ja kuormituksen IP-osoitetta, voit valita tietyn aliverkosta Virtual Network (VNet).

Käyttämällä ILB listener SQL Server-palvelimen päätepisteen (kuten Server = tcp:ListenerName, 1433; tietokannan = nimi) on käytettävissä vain:

- Palvelut ja VMs Virtual samassa verkostossa
- Palvelut ja VMs yhdistetyn paikallisen verkosta
- Palveluiden ja -yhdistetty VNets VMs

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Kuva 1 - SQL-AlwaysOn määritetty Internetiin yhteydessä oleva kuormituksen

## <a name="add-internal-load-balancer-to-the-service"></a>Lisää sisäinen kuormituksen-palveluun

1. Seuraavassa esimerkissä on määritetään Virtual verkkoon, joka sisältää aliverkon nimellä aliverkon-1:

        Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc

2. Lisää kuormitus tasataan päätepisteet ILB, valitse kunkin AM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –
        DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

        Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 –DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Yllä olevassa esimerkissä on 2 AM nimeltä "sqlsvc1" ja "sqlsvc2" suorittaminen pilveen palvelun "Sqlsvc". Kun olet luonut ILB "DirectServerReturn" valitsinta, kuormitus tasataan päätepisteet lisääminen sallimaan SQL määrittäminen kuuntelijoita käytettävyys ryhmien ILB.

Lisätietoja SQL AlwaysOn kohdassa [Configure sisäinen kuormituksen AlwaysOn käytettävyys ryhmän Azure-tietokannassa](../virtual-machines/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Katso myös

[Aloita selainpohjainen vastakkaisten kuormituksen tasauspalvelun asetusten määrittäminen](load-balancer-get-started-internet-arm-ps.md)

[Aloita sisäisten kuormituksen määrittäminen](load-balancer-get-started-ilb-arm-ps.md)

[Kuormituksen tasauspalvelun jakauman tilan määrittäminen](load-balancer-distribution-mode.md)

[Kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten määrittäminen](load-balancer-tcp-idle-timeout.md)
