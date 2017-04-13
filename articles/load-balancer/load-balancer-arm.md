<properties
   pageTitle="Azure Resurssienhallinta tuki kuormituksen | Microsoft Azure "
   description="PowerShellin käyttäminen kuormituksen Azure resurssien hallinta. Mallien käyttäminen kuormituksen"
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


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Azure kuormituksen Azure Resurssienhallinta-tuen käyttäminen

Azure Resurssienhallinta on ensisijainen management framework Services Azure-tietokannassa. Azure kuormituksen voi hallita Azure Resurssienhallinta-pohjainen API ja työkalujen avulla.

## <a name="concepts"></a>Käsitteitä

Resurssien hallinnan Azure kuormituksen sisältää seuraavia lapsen resursseja:

- Edusta-IP-määritys – kuormituksen tasauspalvelun voi sisältää vähintään yhden edusta IP-osoitteet, eli virtual IP-osoitteet (VIPs). Nämä IP-osoitteet yhteyshenkilönä tunkeutumisen tietoliikenteen.

- Taustatietokannan osoite resurssivarantoon – nämä ovat IP-osoitteet, jotka on liitetty kanssa virtuaalikoneen Network Interface kortin (NIC), johon kuormituksen jaetaan.

- Lataa Vastatilin säännöt – säännön ominaisuus yhdistää annetun edusta IP ja portin yhdistelmästä joukko uudelleen IP-osoitteet ja portin yhdistelmästä. Yksittäisen kuormituksen voi olla useita kuormituksen säännöt. Niiden sääntöjen on edusta IP ja portin ja taustatietokantaan IP ja portin VMs liittyvät yhdistelmä.

- Keräysputkien – keräysputkien käyttöön seurantaan AM esiintymät kunto. Jos kunto näytteenottimen epäonnistuu, AM esiintymä otetaan kierto ulos automaattisesti.

- Saapuvan liikenteen säännöt NAT – NAT sääntöjen määrittäminen – eteen juoksutus saapuvan liikenteen lopettaa IP ja jaetaan uudelleen IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Pikaopas-mallit

Azure Resurssienhallinta voit valmistella sovellustesi määritettäviä mallin avulla. Yksittäisen mallia voit ottaa palveluihin ja niiden välisten riippuvuuksien. Saman mallin avulla voit ottaa sovelluksen elinkaari jokaisen vaiheen aikana sovelluksesi toistuvasti.

Malleja voi sisältää määritykset näennäiskoneiden, Virtual verkkojen, käytettävyys joukot, verkkoliittymät (NIC), tallennustilan tilit, kuormituksen tasoitusmääritykset, verkon käyttöoikeusryhmät ja julkiseen IP-osoitteet. Mallien avulla voit luoda monimutkaisia sovelluksen kaikki. Mallitiedoston voi kuitata Versionhallinta ja yhteistyön sisällönhallinta-järjestelmään.

[Lisätietoja mallit](http://go.microsoft.com/fwlink/?LinkId=544798)

[Lisätietoja verkkoresursseja](../virtual-network/resource-groups-networking.md)

Pikaopas malleja käyttämällä Azure kuormituksen löytyy [GitHub säilöön](https://github.com/Azure/azure-quickstart-templates) isännöinnin joukko luotu yhteisön malleja.

Esimerkkejä malleista:

- [2 VMs ladata tasaustoiminto ja kuormituksen säännöt](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 VMs VNET sisäisen kuormituksen ja kuormituksen sääntöjä:](http://go.microsoft.com/fwlink/?LinkId=544800)
- [2 VMs kuormituksen tasaus- ja kg NAT sääntöjen määrittäminen](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Azure kuormituksen PowerShell tai CLI määrittäminen

Azure resurssien hallinnan cmdlet-komennot, komentoriviltä työkalut ja REST API käytön aloittaminen

- [Azure verkko cmdlet-komennot](https://msdn.microsoft.com/library/azure/mt163510.aspx) voidaan luoda kuormituksen tasauspalvelun.
- [Voit luoda kuormituksen, Azure resurssien hallinnan avulla](load-balancer-get-started-ilb-arm-ps.md)
- [Azure CLI käyttäminen Azure Resurssienhallinta](../xplat-cli-azure-resource-manager.md)
- [Kuormituksen REST API](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Seuraavat vaiheet

Voit myös [luomisesta vastakkaisten kuormituksen Internet](load-balancer-get-started-internet-arm-ps.md) - ja minkä tyyppistä [jakauman tilassa](load-balancer-distribution-mode.md) tietyn kuormituksen tasauspalvelun verkon liikenne asetusten määrittäminen.

Opettele [kuormituksen tasauspalvelun käyttämättömät TCP aikakatkaisun asetusten](load-balancer-tcp-idle-timeout.md)hallinta. Tämä on tärkeää, kun sovellus on pidä yhteydet-palvelinten kuormituksen tasauspalvelun takana toiminnassa.
