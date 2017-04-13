<properties
    pageTitle="SQL Server Azure suojaustietoja | Microsoft Azure"
    description="Tässä ohjeaiheessa viittaa perinteinen käyttöönotto-mallin avulla luotu resursseja ja on yleisiä ohjeita suojaaminen SQL Serveriä Azure Virtual Machine."
    services="virtual-machines-windows"
    documentationCenter="na"
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
    ms.date="06/24/2016"
    ms.author="jroth" />

# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Azuren näennäiskoneiden SQL Server liittyviä suojaustietoja
 
Artikkelin sisältö yleinen suojausohjeita, jotka auttavat vahvistaa suojattu käyttö SQL Server-esiintymien Azure-AM. Kuitenkin varmistamiseksi paremman suojauksen SQL Serverin tietokantaan-esiintymissä Azure-tietokannassa on suositeltavaa, että otat perinteinen paikallisen suojauskäytäntöjä lisäksi suojauksen parhaiden käytäntöjen Azure.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Katso lisätietoja SQL Server-suojauskäytäntöjä [SQL Server 2008 R2: n suojauksen parhaiden käytäntöjen - toiminnallisia ja järjestelmänvalvojan tehtävät](http://download.microsoft.com/download/1/2/A/12ABE102-4427-4335-B989-5DA579A4D29D/SQL_Server_2008_R2_Security_Best_Practice_Whitepaper.docx)

Azure noudattaa eri alan asetusten ja standardit, joiden avulla voit muodostaa SQL Serveriä Virtual Machine-yhteensopiva ratkaisu. Lisätietoja Azure säädösten noudattamisen on artikkelissa [Azure Valvontakeskus](https://azure.microsoft.com/support/trust-center/).

Seuraavassa on luettelo suojausta koskevia suosituksia, jotka tulee ottaa huomioon, kun määrittäminen ja yhteyden muodostaminen SQL Server Azure-AM esiintymä.

## <a name="considerations-for-managing-accounts"></a>Huomioon otettavia seikkoja tilien hallinta:

- Luo yksilöllinen paikallisen järjestelmänvalvojatilin, nimi ei ole **järjestelmänvalvoja**.

- Käyttää kaikkien tilien monimutkaisia vahva salasana. Lisätietoja Luo vahva salasana, katso artikkeli [Vihjeitä vahvojen salasanojen luominen](http://windows.microsoft.com/en-us/windows-vista/Tips-for-creating-a-strong-password) .

- Oletusarvon mukaan Azure valitsee Windows-todennusta SQL Server Virtual Machine asennuksen aikana. Tämän vuoksi **SA** -kirjautuminen ei ole käytössä ja asennuksen on määritetty salasana. Microsoft suosittelee, että **SA** -kirjautuminen ei voi käyttää tai käytössä. Seuraavassa on vaihtoehtoisia strategioita, SQL-kirjautuminen tarvittaessa:
    - Luo SQL-tili, jolla on sysadmin jäsenyyttä.
    - Jos sinun on käytettävä **SA** -kirjautuminen, ota käyttöön kirjautuminen tai nimeä se uudelleen ja määritä uusi salasana.
    - Molemmat vaihtoehdot, jotka olivat mainittiin edellyttävät muutoksen todennustila **SQL Serverin**ja Windowsin todennustilassa. Lisätietoja on artikkelissa [Muuta palvelimen todennustila](https://msdn.microsoft.com/library/ms188670.aspx).

## <a name="considerations-for-securing-connections-to-azure-virtual-machine"></a>Azure virtuaalikoneen yhteyksien suojaamiseen huomioon otettavia seikkoja:

- Kannattaa käyttää [Azure Virtual verkoston](../virtual-network/virtual-networks-overview.md) ammattimainen näennäiskoneiden julkisen RDP portit sijaan.

- [Verkko-käyttöoikeusryhmän](../virtual-network/virtual-networks-nsg.md) (NSG) avulla voit hyväksyä tai hylätä virtuaalikoneen verkkoliikenteen. Jos haluat käyttää NSG ja päätepisteen Käyttöoikeusluettelon vielä ole paikassa, poista ensin päätepiste Käyttöoikeusluettelon. Lisätietoja hallinnasta on kohdassa [hallinta Access ohjausobjektin (käyttöoikeusluettelot) päätepisteiden PowerShell-toiminnolla](../virtual-network/virtual-networks-acl-powershell.md).

- Jos käytössäsi on päätepisteet, poistaa minkä tahansa virtuaalikoneen päätepisteet, jos et käytä niitä. Ohjeita päätepisteet käyttöoikeusluettelot käyttämisestä on artikkelissa [Hallitse päätepisteen Käyttöoikeusluettelon](../virtual-network/virtual-machines-windows-classic-setup-endpoints.md#manage-the-acl-on-an-endpoint).

- SQL Server-tietokantamoduuli Azuren näennäiskoneiden esiintymä salattua yhteyttä-asetus käyttöön. Määritä allekirjoitetun varmenteen SQL server-esiintymän. Lisätietoja on ohjeaiheessa [Käyttöön salattu yhteydet-tietokantamoduulin](https://msdn.microsoft.com/library/ms191192.aspx) ja [Yhteysmerkkijonon syntaksia](https://msdn.microsoft.com/library/ms254500.aspx).

- Jos oman näennäiskoneiden kannattaa käyttää vain tietyn verkosta, käytä Windowsin palomuuri käytön rajoittaminen tiettyihin IP-osoitteet tai verkon aliverkosta.

## <a name="next-steps"></a>Seuraavat vaiheet

Jos olet myös kiinnostunut ympärille suorituskyvyn parhaita käytäntöjä, katso [Suorituskyvyn SQL Server Azuren näennäiskoneiden parhaat käytännöt](virtual-machines-windows-sql-performance.md).

Katso muita aiheeseen liittyviä suorittamalla SQL Server Azure VMs:, [SQL Server-Azuren näennäiskoneiden yleiskatsaus](virtual-machines-windows-sql-server-iaas-overview.md).
