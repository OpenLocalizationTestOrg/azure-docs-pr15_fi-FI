<properties
    pageTitle="Azure AD-toimialueen palveluista: Verkko ohjeet | Microsoft Azure"
    description="Huomioitavaa Azure Active Directory-toimialueen palveluista verkko"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="maheshu"/>

# <a name="networking-considerations-for-azure-ad-domain-services"></a>Huomioitavaa Azure AD-toimialueen palveluista verkko

## <a name="how-to-select-an-azure-virtual-network"></a>Azure virtual verkon valitseminen
Seuraavien ohjeiden avulla voit valita virtual verkon Azure AD-toimialueen palvelujen käyttämällä.

### <a name="type-of-azure-virtual-network"></a>Azure virtual verkon tyypin

- Voit ottaa Azure AD-toimialueen palveluista perinteinen Azure virtual verkossa.

- Azure AD-toimialueen palveluista **ei voi ottaa käyttöön virtual verkoissa luotu Azure Resurssienhallinta**.

- Voit muodostaa Resurssienhallinta-pohjainen virtual verkon perinteinen virtual verkkoon, jossa Azure AD-toimialuepalvelut on otettu käyttöön. Tämän jälkeen voit Resurssienhallinta-pohjainen virtual verkon Azure AD-toimialueen palveluista. Lisätietoja on kohdassa [verkkoyhteyden](active-directory-ds-networking.md#network-connectivity) .

- **Alueellisten Virtual verkkojen**: Jos aiot käyttää aiemmin virtual verkkoon, varmista, että se on alueellisen virtual verkkoon.

    - Aiempien versioiden affiniteetti ryhmät-järjestelmä käyttävät Virtual verkot ei voi käyttää Azure AD-toimialueen palveluiden kanssa.

    - Jos haluat käyttää Azure AD-toimialueen palvelut, [Voit siirtää vanhat virtual alueellisen virtual verkkoihin](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).


### <a name="azure-region-for-the-virtual-network"></a>Azure alueen virtual verkossa

- Hallitun toimialueen on otettu käyttöön sama Azure alue kuin virtual verkon Azure AD-toimialueen palvelujen voit ottaa käyttöön palvelun.

- Valitse virtual verkon Azure alueessa, joka tukee Azure AD-toimialueen palveluista.

- On tiedettävä Azure alueet, joiden Azure AD-toimialueen palveluista on käytettävissä [alueittain Azure palvelut](https://azure.microsoft.com/regions/#services/) -sivulla.


### <a name="requirements-for-the-virtual-network"></a>Virtuaalinen verkon vaatimukset

- **Azure työmääriä lähellä**: Valitse virtual verkko, joka isännöi/isännöi näennäiskoneiden, jotka tarvitsevat Azure AD-toimialueen palveluista tällä hetkellä.

- **Mukautettu/tuo-your-omia DNS-palvelimet**: että ei ole mukautettu DNS-palvelimet määritetty virtual verkko.

- **Aiemmin luotuun toimialueita, joissa on sama toimialuenimi**: Varmista, että sinulla ei ole aiemmin toimialue, joilla on sama toimialuenimi käytettävissä virtual verkossa. Oletetaan esimerkiksi, sinulla on toimialue nimeltä "contoso.com" jo käytettävissä valitun virtual verkossa. Yrität myöhemmin käyttöön Azure AD-toimialueen palveluista on sama toimialuenimi (eli "contoso.com") virtual verkon hallitun toimialue. Käytössä ilmenee virhe yritettäessä käyttöön Azure AD-toimialueen palveluista. Tämä virhe on Nimiristiriitojen toimialuenimen virtual verkossa. Tässä tapauksessa sinun käytettävä eri nimellä Azure AD-toimialueen palveluista hallitun toimialueen määrittäminen. Vaihtoehtoisesti voit valmistella varaustiedoista jo olemassa oleva toimialue ja siirry Azure AD-toimialueen palveluiden käyttöön.

> [AZURE.WARNING] Ei voi siirtää toimialuepalveluita eri virtual verkkoon, kun olet ottanut palvelua.


## <a name="network-security-groups-and-subnet-design"></a>Verkon käyttöoikeusryhmiä ja aliverkon rakenne
[Verkon suojaus ryhmän (NSG)](../virtual-network/virtual-networks-nsg.md) sisältää luettelon Käyttöoikeusluettelon (Access Control) säännöt, jotka Salli tai estä verkkoliikennettä AM-esiintymissä Virtual verkossa. NSGs voi olla aliverkosta tai yksittäiset AM esiintymät, aliverkon sisällä. Jos aliverkon liitetään NSG, kaikki kyseisen aliverkon AM esiintymät koskevat Käyttöoikeusluettelon säännöt. Lisäksi yksittäisiä AM liikenne voidaan rajoittaa edelleen liittämällä NSG suoraan, jos haluat, että AM.

![Suositeltu aliverkon rakenne](./media/active-directory-domain-services-design-guide/vnet-subnet-design.png)


### <a name="best-practices-for-choosing-a-subnet"></a>Parhaat käytännöt aliverkon valitseminen
- Ottaa käyttöön **erillinen kiinteä aliverkon** Azure virtual verkossa oleville Azure AD-toimialueen palveluista.

- NSGs eivät koske Kiinteä aliverkon hallitun toimialueen. Jos sinun on käytettävä NSGs Kiinteä aliverkon, varmistaa **Estä palvelun ja hallinnoida toimialueen portit**.

- Älä rajoita IP-osoitteiden käytettävissä Kiinteä aliverkon hallitun toimialueen määrän liian. Tämä rajoitus estää palvelun kahden toimialueen ohjauskoneen saataville hallitun toimialueen.

- **Älä ota Azure AD-toimialueen palvelut yhdyskäytävän aliverkon** virtual verkoston.


> [AZURE.WARNING] Kun liität NSG aliverkon, jossa Azure AD-toimialueen palveluiden kanssa on käytössä, Microsoftin mahdollisuus palvelun ja hallita toimialueen saa häiritä. Lisäksi keskeytyy Azure AD-vuokraajan ja hallittujen toimialueen välisen synkronoinnin. **SLA ei koske käyttöönotoissa, jossa NSG on otettu käyttöön, joka estää Azure AD-toimialueen palveluista päivittäminen ja toimialueen hallinta.**


### <a name="ports-required-for-azure-ad-domain-services"></a>Azure AD-toimialueen palveluista vaaditaan portit
Seuraavat portit tarvittavat Azure AD-toimialueen palvelut-palveluun ja ylläpitää hallitun toimialueen. Varmista, että nämä portit estetä aliverkon, jossa on käytössä hallitun toimialueen.

| Porttinumero | Tarkoitus |
|---|---|
| 443 | Azure AD-vuokraajan synkronointi |
| 3389 | Toimialueen hallinnointi |
| 5986 | Toimialueen hallinnointi |
| 636 | Hallitut toimialueen tietopalvelimet LDAP (LDAPS) |



## <a name="network-connectivity"></a>Verkkoyhteyden
Hallitut Azure AD-toimialueen palvelut-toimialueen voi olla käytössä vain yhden perinteinen virtual verkoston Azure-tietokannassa sisällä. Azure Resurssienhallinta-sovelluksella luodut Virtual verkkoja ei tueta.


### <a name="scenarios-for-connecting-azure-networks"></a>Tilanteita, joissa Azure verkostojen yhdistäminen
Yhdistä Azure virtual verkkojen käyttää hallitun toimialuetta kaikista käyttöönoton seuraavissa tilanteissa:

#### <a name="use-the-managed-domain-in-more-than-one-azure-classic-virtual-network"></a>Käyttää hallittuja toimialuetta useita Azure perinteinen virtual verkossa
Voit yhdistää muiden Azure perinteinen virtual käyttäminen Azure perinteinen virtual verkkoon joissa on käytössä Azure AD-toimialueen palveluista. Tämän VPN-yhteyden avulla voit käyttää hallitun toimialuetta oman työmääriä virtual verkoista otettu käyttöön.

![Perinteinen virtual verkkoyhteyden](./media/active-directory-domain-services-design-guide/classic-vnet-connectivity.png)

#### <a name="use-the-managed-domain-in-a-resource-manager-based-virtual-network"></a>Käyttää hallittuja toimialuetta Resurssienhallinta-pohjainen virtual verkossa
Voit muodostaa Resurssienhallinta-pohjainen virtual verkon Azure perinteinen virtual verkkoon joissa on käytössä Azure AD-toimialueen palveluista. Yhteyden avulla voit käyttää hallitun toimialuetta oman työmääriä virtual Resurssienhallinta-pohjainen-verkossa käyttöön.

![Resurssienhallinta, perinteinen virtual verkkoyhteydestä](./media/active-directory-domain-services-design-guide/classic-arm-vnet-connectivity.png)


### <a name="network-connection-options"></a>Verkko-yhteyden asetukset

- **VNet VNet yhteyksien sivusto sivusto VPN-yhteydet**: yhteyden muodostaminen toiseen virtual verkkoon (VNet VNet) virtual verkon muistuttaa yhteyden virtual verkon paikallisen sivuston sijainnissa. Yhteyksien molempien tietotyyppien käyttää VPN-yhdyskäytävän suojatun tunnelin IP/IKE.

    ![Virtuaalinen verkkoyhteyden käyttäminen VPN-yhdyskäytävän](./media/active-directory-domain-services-design-guide/vnet-connection-vpn-gateway.jpg)

    [Lisätietoja - muodostaa VPN-yhdyskäytävän virtual verkkoihin](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)


- **VNet VNet yhteyksiä VPN peering**: VPN peering on toimintoa, joka yhdistää kaksi virtual poikkeavaa Azure kuvaa runkoverkkoa verkon kautta. Kun peered, kaksi virtual verkot tulevat näkyviin yksi yhteyden kaikkiin tarkoituksiin. Niitä hallitaan edelleen erillisessä resursseina, mutta näitä virtual verkoissa näennäiskoneiden viestiä keskenään suoraan yksityisten IP-osoitteiden avulla.

    ![Virtuaalinen verkkoyhteyden käyttäminen peering](./media/active-directory-domain-services-design-guide/vnet-peering.png)

    [Lisätietoja - virtual verkon peering](../virtual-network/virtual-network-peering-overview.md)



<br>

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä

- [Azure virtual verkon peering](../virtual-network/virtual-network-peering-overview.md)

- [Perinteinen käyttöönoton mallin VNet VNet yhteyden määrittäminen](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)

- [Azure verkon käyttöoikeusryhmät](../virtual-network/virtual-networks-nsg.md)
