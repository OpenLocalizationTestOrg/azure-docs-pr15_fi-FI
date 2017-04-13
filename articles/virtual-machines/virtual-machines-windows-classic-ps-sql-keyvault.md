<properties
    pageTitle="SQL Server Azure avaimen säilö integrointi määrittämistä Azure VMs (perinteinen)"
    description="Lue, miten voit automatisoida SQL Server-salausta Azure avaimen säilö käytettäväksi määrittämisen. Tässä ohjeaiheessa kerrotaan, miten Azure avaimen säilö integrointi käytettäväksi SQL Serverin perinteinen käyttöönotto-mallin luominen näennäiskoneiden."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-classic"></a>SQL Server Azure avaimen säilö integrointi määrittämistä Azure VMs (perinteinen)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-ps-sql-keyvault.md)
- [Perinteinen](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Yleiskatsaus
On useita SQL Serverin salauksen ominaisuudet, kuten [Läpinäkyvä salauksen (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sarakkeen tason salauksen (Tyhjennä)](https://msdn.microsoft.com/library/ms173744.aspx)ja [varmuuskopion salausta](https://msdn.microsoft.com/library/dn449489.aspx). Lomakkeita salaus edellyttää, että hallinta ja tallentaa käytät salauksen salausavaimet. Azure avaimen säilö (AKV)-palvelu on suunniteltu tietoturvaa ja hallinnointi painettavat näppäimet suojattuina ja erittäin käytettävissä sijainnissa. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) avulla SQL Serverin käyttämään painettavat näppäimet Azure avain säilöstä.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Jos käytössäsi on SQL Server-ympäristöön koneet, sinun on [ohjeiden voit käyttää Azure avaimen säilö paikallisen SQL Server-tietokoneesta](https://msdn.microsoft.com/library/dn198405.aspx). Mutta SQL Server-Azure VMs, voit säästää aikaa käyttämällä *Azure avaimen säilö integrointi* -toimintoa. Voit ottaa tämän ominaisuuden käyttöön muutaman Azure PowerShell cmdlet voidaan automatisoida SQL-AM käyttämään avaimen säilö tarvittavat määritykset.

Jos tämä ominaisuus on käytössä, se automaattisesti asentaa SQL Server-yhdistin, määrittää EKM tarjoaja voi käyttää Azure avaimen säilö ja luo tunnistetiedot, jotta voit käyttää omaa säilö. Jos olet tarkastellut mainittiin paikallisen ohjeista ohjeita, näet, että tätä ominaisuutta automatisoi vaiheet 2 ja 3. Tarvitset edelleen manuaalisesti riittää on luotava avaimen säilö ja avaimet. SQL-AM koko määritys on automaattinen sieltä. Kun tämä toiminto on suoritettu tätä asetusta, voit suorittaa T-SQL-lauseita Aloita salaaminen tietokantoja tai varmuuskopioiden tavalliseen tapaan.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="configure-akv-integration"></a>AKV integroinnin määrittäminen
PowerShellin avulla voit määrittää Azure avaimen säilö integrointi. Seuraavissa osissa on yleiskuvaus tarvittavat parametrit ja valitse sitten malli PowerShell-komentosarjaa.

### <a name="install-the-sql-server-iaas-extension"></a>Asenna SQL Server IaaS-laajennus

Ensin [asentaa SQL Server IaaS-tunniste](virtual-machines-windows-classic-sql-server-agent-extension.md).

### <a name="understand-the-input-parameters"></a>Tietoja syöttöparametrien
Seuraavassa taulukossa on lueteltu seuraavan osion PowerShell-komentosarjaa suorittamiseen tarvittavat parametrit.

|Parametri|Kuvaus|Esimerkki|
|---|---|---|
|**$akvURL**|**Avaimen säilö URL-osoite**|"https://contosokeyvault.vault.azure.net/"|
|**$spName**|**Palvelun täydellinen käyttäjätunnus**|"fde2b411 - 33d 5-4e11-af04eb07b669ccf2"|
|**$spSecret**|**Palvelun lyhennys salaisuus**|"9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM ="|
|**$credName**|**Tunnistetietojen nimi**: AKV integrointi Luo tunnistetiedon SQL Serverissä salliminen AM käyttää avaimen säilö. Valitse tämä tunnistetiedon nimi.|"mycred1"|
|**$vmName**|**Virtuaalikoneen nimi**: aiemmin luotu SQL-AM nimi.|"myvmname"|
|**$serviceName**|**Palvelunimi**: pilvipalvelussa nimi, joka on liitetty SQL-AM.|"mycloudservicename"|

### <a name="enable-akv-integration-with-powershell"></a>Ota käyttöön AKV PowerShell-integrointi
**Uusi AzureVMSqlServerKeyVaultCredentialConfig** cmdlet-komento luo määritysten objektin Azure avaimen säilö integrointi-toiminnolla. **Määritä AzureVMSqlServerExtension** määrittää integrointi **KeyVaultCredentialSettings** -parametria. Seuraavat vaiheet näyttää, miten voit käyttää näitä komentoja.

1. PowerShellin Azure ensin määritettävä syöttöparametrien tietyn arvoilla edellisten kohtien tässä ohjeaiheessa kuvatulla tavalla. Seuraavassa on esimerkki.

        $akvURL = "https://contosokeyvault.vault.azure.net/"
        $spName = "fde2b411-33d5-4e11-af04eb07b669ccf2"
        $spSecret = "9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM="
        $credName = "mycred1"
        $vmName = "myvmname"
        $serviceName = "mycloudservicename"
2.  Käytä seuraavaa komentosarjaa AKV-integroinnin valitseminen käyttöön ja määrittäminen.

        $secureakv =  $spSecret | ConvertTo-SecureString -AsPlainText -Force
        $akvs = New-AzureVMSqlServerKeyVaultCredentialConfig -Enable -CredentialName $credname -AzureKeyVaultUrl $akvURL -ServicePrincipalName $spName -ServicePrincipalSecret $secureakv
        Get-AzureVM -ServiceName $serviceName -Name $vmName | Set-AzureVMSqlServerExtension -KeyVaultCredentialSettings $akvs | Update-AzureVM

SQL IaaS Agent-tunniste päivittää SQL-AM uusien määritysten.

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
