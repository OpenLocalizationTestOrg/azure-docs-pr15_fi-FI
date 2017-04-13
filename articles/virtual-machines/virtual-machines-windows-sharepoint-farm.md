<properties
    pageTitle="Luo SharePoint server-klustereihin | Microsoft Azure"
    description="Luoda nopeasti uuden SharePoint 2013 tai SharePoint 2016-klusterin Azure-tietokannassa."
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

# <a name="create-sharepoint-server-farms"></a>SharePoint server-klustereihin luominen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Perinteinen malli.

## <a name="sharepoint-2013-farms"></a>SharePoint 2013: n klustereihin

Microsoft Azure portaalin Marketplaceen voit nopeasti luoda valmiiksi SharePoint Server 2013: n klustereihin. Tämä säästät aikaa kun tarvitset Normaali tai suuri käytettävyys SharePoint-klusterin keskihajonta/testiympäristössä tai jos arvioit SharePoint Server 2013: n yhteiskäyttö ratkaisuksi organisaatiollesi.

> [AZURE.NOTE] **SharePoint Server-klusterin** kohteen Azure Marketplacesta Azure-portaalin on poistettu. Se on korvattu **SharePoint 2013: n ei-HA asennuksen** ja **SharePoint 2013: n HA-klusterin** kohteille.

Tavallinen SharePoint-klusterin koostuu kolmesta näennäiskoneiden Tässä määrityksessä.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Voit käyttää tätä palvelinfarmin määritys SharePointin sovellusten kehittämiseen yksinkertaistettu asetukset tai SharePoint 2013: n ensimmäistä kertaa arviointi.

Perus (kolmen palvelimen) SharePoint-klusterin luominen

1. Napsauta [tätä](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Valitse **Ota käyttöön**.
3. **SharePoint 2013: n ei-HA klusterin** -ruudussa Valitse **Luo**.
4. Määritä asetukset koskevan ilmoituksen **Luominen SharePoint 2013: n ei-HA klusterin** -ruudussa ja valitse sitten **Luo**.

Suuren käytettävyyden SharePoint-klusterin koostuu yhdeksän näennäiskoneiden Tässä määrityksessä.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Voit testata suurempi asiakkaan latautuu, suuren käytettävyyden ulkoisen SharePoint-sivuston ja SQL Server AlwaysOn käytettävyys-ryhmiä SharePoint-klusterin tämän palvelinfarmin määritys. Voit käyttää myös määritysten for SharePointin sovellusten kehittämiseen suuren käytettävyyden ympäristössä.

Suuren käytettävyyden (9-palvelin) SharePoint-klusterin luominen

1. Napsauta [tätä](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Valitse **Ota käyttöön**.
3. **SharePoint 2013: n HA-klusterin** -ruudussa Valitse **Luo**.
4. Määritä seitsemän vaiheet **Luominen SharePoint 2013: n HA-klusterin** -ruudun asetukset ja valitse sitten **Luo**.

> [AZURE.NOTE] Et voi luoda **SharePoint 2013: n ei-HA klusterin** tai **SharePoint 2013: n HA-klusterin** , Azure-kokeiluversio.

Azure portaalin Luo toinen näistä klustereihin vain pilvipalveluita virtual verkon Internetiin yhteydessä oleva web-sivuston kanssa. Yhteyttä ei ole sivuston sivuston VPN- tai ExpressRoute takaisin organisaation verkkoon.

> [AZURE.NOTE] Kun luot basic tai suuren käytettävyyden SharePoint-klustereihin Azure-portaalissa, et voi määrittää aiemmin resurssiryhmä. Voit kiertää tämän rajoituksen luoda nämä klustereihin PowerShellin Azure. Lisätietoja on artikkelissa [Luo SharePoint 2013: n keskihajonta/testi klustereissa, joissa PowerShellin Azure](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>SharePoint 2016-klustereihin

Katso [tämän artikkelin](https://technet.microsoft.com/library/mt723354.aspx) ohjeita voit luoda yhden palvelimen SharePoint Server 2016-klusterin.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>SharePoint-klustereihin hallinta

Voit hallita näitä klustereihin etätyöpöydän yhteyksien kautta palvelimiin. Lisätietoja on artikkelissa [virtuaalikoneen kirjautua](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Keskitetyn hallinnan SharePoint-sivustosta voit määrittää omat sivustot, SharePoint-sovellusten ja muita toimintoja. Lisätietoja on artikkelissa [Määrittäminen SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Seuraavat vaiheet

- Tutustu Azure infrastruktuuripalvelut muita [SharePoint-määrityksiä](https://technet.microsoft.com/library/dn635309.aspx) .
