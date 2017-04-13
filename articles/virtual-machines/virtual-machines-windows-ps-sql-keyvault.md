<properties
    pageTitle="SQL Server Azure avaimen säilö integrointi määrittämistä Azure VMs (resurssien hallinta)"
    description="Lue, miten voit automatisoida SQL Server-salausta Azure avaimen säilö käytettäväksi määrittämisen. Tässä ohjeaiheessa kerrotaan, miten Azure avaimen säilö integrointi käytettäväksi SQL Server näennäiskoneiden luotiin resurssien hallinnan."
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
    ms.date="10/25/2016"
    ms.author="jroth"/>

# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-vms-resource-manager"></a>SQL Server Azure avaimen säilö integrointi määrittämistä Azure VMs (resurssien hallinta)

> [AZURE.SELECTOR]
- [Resurssien hallinta](virtual-machines-windows-ps-sql-keyvault.md)
- [Perinteinen](virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Yleiskatsaus
On useita SQL Serverin salauksen ominaisuudet, kuten [Läpinäkyvä salauksen (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sarakkeen tason salauksen (Tyhjennä)](https://msdn.microsoft.com/library/ms173744.aspx)ja [varmuuskopion salausta](https://msdn.microsoft.com/library/dn449489.aspx). Lomakkeita salaus edellyttää, että hallinta ja tallentaa käytät salauksen salausavaimet. Azure avaimen säilö (AKV)-palvelu on suunniteltu tietoturvaa ja hallinnointi painettavat näppäimet suojattuina ja erittäin käytettävissä sijainnissa. [SQL Server Connector](http://www.microsoft.com/download/details.aspx?id=45344) avulla SQL Serverin käyttämään painettavat näppäimet Azure avain säilöstä.

Jos olet SQL Serverin käytön paikallisen koneet siellä on [ohjeiden voit käyttää Azure avaimen säilö paikallisen SQL Server-tietokoneesta](https://msdn.microsoft.com/library/dn198405.aspx). Mutta SQL Server-Azure VMs, voit säästää aikaa käyttämällä *Azure avaimen säilö integrointi* -toimintoa.

Jos tämä ominaisuus on käytössä, se automaattisesti asentaa SQL Server-yhdistin, määrittää EKM tarjoaja voi käyttää Azure avaimen säilö ja luo tunnistetiedot, jotta voit käyttää omaa säilö. Jos olet tarkastellut mainittiin paikallisen ohjeista ohjeita, näet, että tätä ominaisuutta automatisoi vaiheet 2 ja 3. Tarvitset edelleen manuaalisesti riittää on luotava avaimen säilö ja avaimet. SQL-AM koko määritys on automaattinen sieltä. Kun tämä toiminto on suoritettu tätä asetusta, voit suorittaa T-SQL-lauseita Aloita salaaminen tietokantoja tai varmuuskopioiden tavalliseen tapaan.

[AZURE.INCLUDE [AKV Integration Prepare](../../includes/virtual-machines-sql-server-akv-prepare.md)]

## <a name="enabling-and-configuring-akv-integration"></a>Ottaminen käyttöön ja määrittäminen AKV-integrointi
Voit ottaa AKV integrointi valmistelun aikana tai aiemmin VMs määrittäminen.

### <a name="new-vms"></a>Uusi VMs
Jos ovat valmistelu uuden SQL Server virtual koneen kanssa Resurssienhallinta, Azure-portaalissa on vaiheen Azure avain säilöön-integroinnin valitseminen käyttöön. Azure avain säilöön-toiminto on käytettävissä vain Enterprise-, Developer- ja SQL Server laskenta-versiot.

![SQL Azure avaimen säilö integrointi](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Saat yksityiskohtaiset vaiheista valmistelu, [säännöstä SQL Server-virtual tietokoneeseen Azure-portaalissa](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Olemassa olevan VMs
Valitse aiemmin luotu SQL Server-näennäiskoneiden SQL Server-virtuaalikoneen. Valitse **asetukset** -sivu **SQL Server-määritys** -osa.

![Olemassa olevan VMs SQL AKV-integrointi](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Valitse **SQL Server-määritys** -sivu automaattinen avaimen säilö integrointi-osassa **Muokkaa** -painiketta.

![Määrittää aiemmin VMs SQL AKV integrointi](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-configuration.png)

Kun olet valmis, valitse Tallenna muutokset **SQL Server-määritys** -sivu alaosassa **OK** -painiketta.

>[AZURE.NOTE] Voit myös määrittää AKV integrointi mallin avulla. Lisätietoja [Azure pikaopas mallin Azure avain säilöön-integrointia varten](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update).

[AZURE.INCLUDE [AKV Integration Next Steps](../../includes/virtual-machines-sql-server-akv-next-steps.md)]
