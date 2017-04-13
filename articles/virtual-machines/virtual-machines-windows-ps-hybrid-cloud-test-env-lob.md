<properties 
    pageTitle="LOB sovelluksen testiympäristössä | Microsoft Azure" 
    description="Verkkopohjaisia, viivan business-sovelluksen luominen cloud yhdistelmäympäristössä for IT pro tai testausta kehitystä." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Hybrid pilvestä testikäyttöön verkkopohjaisia LOB-sovelluksen määrittäminen

Tässä ohjeaiheessa opastaa luominen Simuloitu cloud yhdistelmäympäristön testikäyttöön verkkopohjaisia rivin ylläpidettävä Microsoft Azure liiketoimintasovelluksessa (LOB). Seuraavassa on tuloksena määritykset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Tässä määrityksessä sisältää seuraavat tiedot:

- Simuloitu paikalliseen verkkoon ylläpidettävä Azure (TestLab VNet).
- Paikallisen-virtual verkko ylläpidettävä Azure (TestVNET).
- VNet VNet VPN-yhteyden.
- Verkkopohjaisia LOB palvelimen, SQL server ja toissijainen toimialueen ohjauskoneen TestVNET virtual verkossa.

Tässä määrityksessä on peruste ja yleisiä aloituskohdan, josta voit:

- Kehittää ja testaa Toimialasovellusten isännöidään Internet Information Services (IIS) ja SQL Server 2014 tietokannan taustassa Azure-tietokannassa.
- Tehdä testejä, Simuloitu hybrid pilvipohjainen tehtäviä se.

On kolme tärkeintä vaiheista hybrid tämän cloud testiympäristössä määrittäminen:

1.  Määritä Simuloitu hybrid cloud-ympäristössä.
2.  Määritä SQL server-tietokoneeseen (SQL1).
3.  Määritä LOB server (LOB1).

Tämä työmäärää edellyttää Azure tilauksen. Jos sinulla on MSDN- tai Visual Studio-tilaus, katso [kuukausittain Azure luottotietojen Visual Studio-tilaajille](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Esimerkki tuotannon LOB sovelluksen ylläpidettävä Azure-kohdassa **Yrityssovellussarjan** arkkitehtuuri Sinikopio [Microsoft-ohjelmiston arkkitehtuurikaavioita](http://msdn.microsoft.com/dn630664)ja toimintakaavioita.

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Vaihe 1: Simuloitu cloud-yhdistelmäympäristön määrittäminen

Luo [Simuloitu hybrid cloud testiympäristössä](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Koska tätä testiympäristössä ei edellytä APP1 palvelimen Corpnet aliverkon tavoitettavuustietojen, voit Sammuta se nyt.

Tämä on nykyiset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Vaihe 2: Määritä SQL server-tietokoneeseen (SQL1)

Azure-portaalista käynnistää DC2 tietokoneen tarvittaessa.

Luo seuraavaksi virtual machine SQL1 seuraavien komentojen PowerShellin Azure-komentorivin paikalliseen tietokoneeseen. Ennen käynnissä näitä komentoja, täytä muuttujien arvoihin ja poista < ja > merkit.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Azure-portaalin käyttäminen yhteyden muodostamiseen SQL1 SQL1 paikallisen järjestelmänvalvojan tilillä.

Määritä seuraavaksi Windowsin palomuurin säännöt, jotta yhteyden testaaminen ja SQL Server-liikenne. Järjestelmänvalvojan Windows PowerShellin komentoriviltä-SQL1 Suorita seuraavat komennot.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Ping-komento on johtaa neljä IP-osoite 192.168.0.4 onnistuneen vastaukset.

Lisää lisätiedot-levyn seuraavaksi SQL1-kirjain f uusi merkkiin.

1.  Valitse vasemmanpuoleisessa ruudussa palvelimen hallinnan valitsemalla **tiedosto ja tallennustilaa palvelut**ja valitse sitten **levyjen**.
2.  Valitse sisällysruudun **levyjen** -ryhmästä **levyn 2** (ja **osion** arvoksi **Tuntematon**).
3.  Valitse **tehtävät**ja valitse sitten **Uusi asema**.
4.  Valitse ennen aloittaa uuden aseman ohjatun toiminnon sivulla, valitse **Seuraava**.
5.  Valitse Valitse palvelin ja levy-sivulla **levyn 2**ja valitse sitten **Seuraava**. Kun sinulta kysytään, valitse **OK**.
6.  Valitse Määritä aseman sivun koon Valitse **Seuraava**.
7.  Tee määrittäminen aseman kirjain tai kansion sivulle Valitse **Seuraava**.
8.  Valitse Valitse file system asetukset-sivulla **Seuraava**.
9.  Valitse Vahvista asetukset-sivun **luominen**.
10. Kun olet valmis, valitse **Sulje**.

Suorita nämä komennot Windows PowerShellin komentokehotteen SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Liitä seuraavaksi SQL1 seuraavien komentojen SQL1 Windows PowerShell-kehotteessa CORP Windows Server Active Directory-toimialueen.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Toimialueen tilin tunnistetietojen **Lisää tietokoneen** -komennon pyydettäessä CORP\User1-tilin avulla.

Uudelleenkäynnistämisen jälkeen SQL1 *SQL1 paikallisen järjestelmänvalvojatilin kanssa*muodostaa Azure portaalin avulla.

Määritä seuraavaksi SQL Server-2014 käyttämään f-asema, uusien tietokantojen ja tilin käyttöoikeudet.

1.  Aloitusnäytön **SQL Server Management**ja valitse sitten **SQL Server 2014 Management Studiossa**.
2.  **Muodosta yhteys palvelimeen**ja valitse **Yhdistä**.
3.  Valitse puunäkymän objektin Explorer-ruudussa **SQL1**hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**.
4.  Valitse **Ominaisuudet** -ikkunassa **Tietokanta-asetukset**.
5.  Etsi **tietokanta oletuskansiot** ja määritä seuraavat arvot: 
    - Kirjoita polku **f:\Data** **tiedot**.
    - Kirjoita polku **f:\Log** **loki**.
    - Kirjoita polku **f:\Backup** **varmuuskopion**.
    - Huomautus: Vain uusien tietokantojen käyttää seuraavista sijainneista.
6.  Valitse **OK** , sulje ikkuna.
7.  Avaa **Suojaus**puun **Objektin Explorer** -ruudussa.
8.  **Kirjautumiset** hiiren kakkospainikkeella ja valitse sitten **New Login**.
9.  Kirjoita **kirjautumisnimi**, **CORP\User1**.
10. Valitse **Roolit** -sivulla **sysadmin**ja valitse sitten **OK**.
11. Sulje Microsoft SQL Server Management Studiossa.

Tämä on nykyiset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Vaihe 3: Määritä LOB palvelimen (LOB1)

Luo ensin virtual machine LOB1 paikalliseen tietokoneeseen PowerShellin Azure komentokehotteeseen seuraavat komennot kanssa.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Käytä seuraavaksi Azure portaalin voivat muodostaa yhteyden LOB1 LOB1 paikallisen järjestelmänvalvojatilin tunnistetiedot.

Määritä seuraavaksi Windowsin palomuuri-sääntö, joka sallii liikenteen yhteys testikäyttöön. Järjestelmänvalvojan Windows PowerShellin komentoriviltä-LOB1 Suorita seuraavat komennot.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Ping-komento on johtaa neljä IP-osoite 192.168.0.4 onnistuneen vastaukset.

Liitä seuraavaksi LOB1 CORP Active Directory-toimialueen kanssa Windows PowerShell-kehotteessa näitä komentoja.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Toimialueen tilin tunnistetietojen **Lisää tietokoneen** -komennon pyydettäessä CORP\User1-tilin avulla.

Uudelleenkäynnistämisen jälkeen Azure portaalin avulla yhteyden LOB1 CORP\User1 tili ja salasana.

Seuraavaksi LOB1 määrittäminen IIS ja testaa CLIENT1 käytöltä.

1.  Kohdassa Palvelimen hallinta, valitse **Lisää roolit ja toiminnot**.
2.  **Ennen kuin aloitat** -sivulle valitsemalla **Seuraava**.
3.  **Valitse asennustyyppi** -sivulle valitsemalla **Seuraava**.
4.  **Valitse kohdepalvelin** -sivulle valitsemalla **Seuraava**.
5.  Valitse **roolit** -sivulla **Web Server (IIS)** **roolit**-luettelossa.
6.  Kun sinulta kysytään, valitse **Lisää ominaisuuksia**ja valitse sitten **Seuraava**.
7.  **Valitse ominaisuudet** -sivulle valitsemalla **Seuraava**.
8.  Valitse **Web Server (IIS)** -sivulla **Seuraava**.
9.  **Valitse roolipalvelut** -sivulla Valitse tai poista valintaruutujen valinta, jos tarvitset testikäyttöön LOB sovelluksen Services ja valitse sitten **Seuraava**.
10. Valitse **Vahvista asennuksen asetukset** -sivun **Asenna**.
11. Odota, kunnes osien asennus on valmis, ja valitse sitten **Sulje**.
12. Azure-portaalista yhdistäminen CLIENT1-tietokone, jossa on CORP\User1 tilin tunnistetiedot, ja kirjoita Internet Explorerin.
13. Kirjoita osoiteriville **http://lob1/** ja paina sitten ENTER-näppäintä. Raportissa pitäisi näkyä oletusarvon IIS 8 web-sivu.

Tämä on nykyiset.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Tässä ympäristössä on nyt valmis verkkopohjaisia LOB1-sovelluksen käyttöönotto ja testaa Corpnet aliverkon CLIENT1 toimintoja.

## <a name="next-step"></a>Seuraava vaihe

- Lisää uuden virtual koneen [Azure portal](virtual-machines-windows-hero-tutorial.md).
