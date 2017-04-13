<properties
    pageTitle="Azure sanasto - Azure sanaston | Microsoft Azure"
    description="Azure sanasto avulla voit selvittää cloud termejä Azure-ympäristössä. Tämä lyhyt Azure sanasto on määritelmät Azure yleisiä cloud ehtojen perusteella."
    keywords="Azure sanasto, cloud terminologiaa, Azure sanasto, terminologiaa määritykset, cloud ehdot"
    services="na"
    documentationCenter="na"
    authors="MonicaRush"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="monicar"/>


# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure sanasto: sanastoa cloud termejä Azure-ympäristössä

Microsoft Azure-sanasto on lyhyt sanastoa cloud terminologiaa Azure-ympäristöön.

## <a name="find-service-definitions-and-other-cloud-terms"></a>Palvelun määritykset ja muita cloud ehtoja etsiminen

* Azure-palveluiden ja niiden sisältyy AWS määritysten vastineiden on [Microsoft Azure ja Amazon verkkopalveluihin](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

* Yleiset alan cloud ehdot artikkelissa [Cloud tietojenkäsittely ehdot](https://azure.microsoft.com/overview/cloud-computing-dictionary/).

Edellä mainitut kaksi eri viittausta kanssa Azure sanasto on lopusta loppuun-luokituksen Azure ja cloud teollisuuden.  

## <a name="azure-glossary-list"></a>Azure sanasto-luettelo


### <a name="account"></a>tilin  
Työpaikan tai koulun tai henkilökohtainen Microsoft-tili, jonka avulla voit hallita Azure tilaus ja käyttää.  
Katso myös, [miten Azure tilaukset liittyvät Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="availability-set"></a>käytettävyys määrittäminen  
Näennäiskoneiden, joita hallitaan yhdessä sovelluksen redundancy sekä luotettavuutta on kokoelma. Käytettävyys-joukon käyttäminen varmistaa, että joko suunniteltu tai suunnittelematon ylläpitotapahtuman aikana vähintään yksi virtual machine on käytettävissä.  
Katso myös [Windows näennäiskoneiden käytettävyyden hallinta](./virtual-machines/virtual-machines-windows-manage-availability.md) tai [Hallitse Linux näennäiskoneiden käytettävyys](./virtual-machines/virtual-machines-linux-manage-availability.md)


### <a name="classic-model"></a>Azure perinteinen käyttöönottomalli  
Jokin kaksi [käyttöönoton mallien](resource-manager-deployment-model.md) asentanut resurssien Azure (uusi malli on Azure Resurssienhallinta) avulla. Azure resurssien voidaan ottaa käyttöön yhden mallin tai muu, kun muiden voidaan ottaa käyttöön sekä mallit. Lisäohjeita yksittäisten Azure resurssien yksityiskohtia, jotka model(s) resurssi voidaan ottaa käyttöön kanssa.


### <a name="cli"></a>Azure käyttöliittymä (CLI)  
[Käyttöliittymä](xplat-cli-install.md) , joka voidaan Azure palveluiden hallinta Windowsin, OS x tai Linux PC-tietokoneeseen.


### <a name="powershell"></a>Azure PowerShell  
[Käyttöliittymä](powershell-install-configure.md) hallinta Azure services komentorivin kautta Windows PC-tietokoneeseen. Joidenkin palvelujen tai ominaisuudet voi hallita vain PowerShell tai CLI kautta. Ohjeet kunkin yksittäisen Azure resurssitiedot, joita model(s) resurssi voidaan ottaa käyttöön kanssa.   
Katso myös [asentaa ja määrittää PowerShellin Azure](powershell-install-configure.md)


### <a name="arm-model"></a>Azure resurssien hallinnan käyttöönottomalli  
Jokin kaksi [käyttöönoton mallien](resource-manager-deployment-model.md) asentanut Microsoft Azure (toinen osa on perinteinen käyttöönottomalli) resurssien avulla. Azure resurssien voidaan ottaa käyttöön yhden mallin tai muu, kun muiden voidaan ottaa käyttöön sekä mallit. Lisäohjeita yksittäisten Azure resurssien yksityiskohtia, jotka model(s) resurssi voidaan ottaa käyttöön kanssa.


### <a name="fault-domain"></a>Virhe toimialueen  
Kokoelma näennäiskoneiden käytettävyys-määrittäminen, joka voi epäonnistua mahdollisesti samanaikaisesti. Esimerkki on joukko Teline koneet, joilla on yhteiset power lähde- ja vaihda. Azure-käytettävyyden määrittäminen näennäiskoneiden on automaattisesti erotettu vika useita toimialueita.  
Katso myös [Windows näennäiskoneiden käytettävyyden hallinta](./virtual-machines/virtual-machines-windows-manage-availability.md) tai [Hallitse Linux näennäiskoneiden käytettävyys](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="geo"></a>GEO  
Tietoja pisimpään, joka on yleensä kahden tai useamman alueita määritetyn reunaa. Rajat voi olla tai sen lisäksi kansallisia reunat ja ovat vaikuttaneet ALV-asetuksella. Jokaisen geo on aina oltava vähintään yksi alue. Esimerkkejä geos ovat Aasian ja Tyynenmeren alueen ja japani. Kutsutaan myös *geography*.  
Katso myös [Azure alueet](best-practices-availability-paired-regions.md)


### <a name="geo-replication"></a>GEO sallittuja  
Prosessi, replikoiminen automaattisesti esimerkiksi BLOB-objektit, taulukoita ja olevien erottimien alueellisen sisällön.  
Katso myös [Aktiivinen Geo-replikoinnin Azure SQL-tietokantaan](./sql-database/sql-database-geo-replication-overview.md)


### <a name="image"></a>Kuva  
Käyttöjärjestelmän ja sovelluksen määritys, joilla voidaan luoda haluamasi määrän näennäiskoneiden sisältävän tiedoston. Azure-tietokannassa on kahdenlaisia kuvia: AM kuva ja OS kuva. AM kuva sisältää käyttöjärjestelmän ja kaikkien levyjen liitetty virtual tietokoneeseen, kun kuva on luotu. OS kuva sisältää vain on generalized käyttöjärjestelmä ei ole tietoja määrityksiä kanssa.  
Katso myös [Siirry ja valitse Windows virtuaalikoneen kuvat Azure PowerShell tai CLI](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md)


### <a name="limits"></a>rajoitukset  
Resurssien avulla voi luoda tai suorituskyvyn ensisijaisen, jotka voidaan määrä. Rajoitukset liittyy yleensä tilaukset ja palvelujen tarjouksista.  
Katso myös [Azure tilaus ja palvelun rajoitukset, kiintiöiden ja rajoitukset](azure-subscription-service-limits.md)


### <a name="load-balancer"></a>kuormituksen  
Resurssi, joka jakaa saapuvan liikenteen verkon tietokoneiden välillä. Azure-kuormituksen tasauspalvelun jakaa-liikenne paikalliseen näennäiskoneiden on määritetty kuormituksen joukon. [Kuormituksen](./load-balancer/load-balancer-overview.md) voi olla Internetiin yhteydessä oleva tai se voi olla sisäinen.  


### <a name="offer"></a>tarjouksen  
Esimerkiksi hinnat hyvitykset ja toisiinsa liittyviä termejä sovellettavan Azure-tilaukseen.  
Katso [Azure tarjouksen tiedot-sivu](https://azure.microsoft.com/support/legal/offer-details/)


### <a name="portal"></a>Portal  
Suojatun Web-portaalin käyttöönotto ja Azure palveluiden hallinta.  On kaksi portaaleihin: [Azure portal](http://portal.azure.com/) ja [perinteinen portal](http://manage.windowsazure.com/). Jotkin palvelut ovat käytettävissä sekä portaaleihin, muut käyttäjät ovat vain käytettävissä toisen. [Azure portaalin käytettävyys kaavion](https://azure.microsoft.com/features/azure-portal/availability/) luetteloita, mitkä palvelut ovat käytettävissä mihin-portaalissa.  


### <a name="region"></a>alue  
Alue, joka ei ole välisiä kansallisia reunat sisältää vähintään yhden palvelinkeskusten geo. Hinnat, alueellisen palvelut ja tarjouksen tiedostotyypit näkyvät alueen tasolla. Toisen alueen, joka voi olla enintään useita satoja Mailia piiloon muodostamiseksi alueellisen pari yleensä yhdistetty alue. Alueellisten paria käytetään järjestelmä palauttaminen ja suuren käytettävyyden skenaarioita. Kutsutaan myös yleensä *sijainti*.  
Katso myös [Azure alueet](best-practices-availability-paired-regions.md)


### <a name="resource"></a>resurssi  
Kohteen, joka on osa Azure ratkaisu. Kunkin Azure palvelun mahdollistaa erityyppisiä resurssit, kuten tietokantoja tai näennäiskoneiden käyttöönotto.   
Katso myös [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md)


### <a name="resource-group"></a>resurssiryhmä  
Säilön Resource Manageriin, joka sisältää sovelluksen liittyvät resurssit. Resurssiryhmä voi sisältää kaikki sovelluksen resurssit tai vain resurssit, jotka ovat loogisesti ryhmiteltyinä. Voit päättää, miten haluat varata resursseja resurssin ryhmiin, mitä tarkoituksenmukaisessa organisaation perusteella.  
Katso myös [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md)


### <a name="arm-template"></a>Resurssienhallinta-malli  
JSON tiedosto, joka määrittää yhden tai useamman Azure resurssin määrittämisen avulla, joka määrittää riippuvuuksia käyttöön resurssien välillä. Malliin voidaan ottaa käyttöön resurssit johdonmukaisesti ja toistuvasti.  
Katso myös [yhtä aikaa muiden kanssa Azure Resurssienhallinta-mallit](resource-group-authoring-templates.md)


### <a name="resource-provider"></a>resurssin tarjoajaan  
Palvelu, joka tuottaa resursseja, voit ottaa käyttöön ja hallita – Resurssienhallinta. Resurssin kunkin palvelun tarjoaa toimintojen käyttämisen resurssit, jotka on otettu käyttöön. Resurssin tarjoajat niitä voi käyttää Azure portaalin, PowerShellin Azure ja useita ohjelmoinnin SDK: T.  
Katso myös [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md)


### <a name="role"></a>rooli  
Keinoja käyttöoikeuksia, joka voi määrittää käyttäjät, ryhmät ja palveluja. Roolien voivat suorittaa toimintoja, kuten luominen, hallinta ja lukea Azure resursseja.  
Katso myös [RBAC: valmiit roolit](./active-directory/role-based-access-built-in-roles.md)


### <a name="sla"></a>tason palvelusopimus (SLA)  
Sopimuksen, joka kuvaa Microsoftin sitoumukset käytettävyyttä ja yhteys. Azure jokaisella palvelulla on tietyn SLA.  
Katso myös [Service Level Agreement](https://azure.microsoft.com/support/legal/sla/)


### <a name="storage-account"></a>tallennustilan tilin  
Tallennustilan-tili, jonka avulla voit käyttää Azuren tallennustilaan Azure-Blob, jonossa taulukon tai tiedosto-palveluihin. Tallennustilan-tilisi on yksilöllinen nimitilan Azuren tallennustilaan tieto-objektit.  
Katso myös [tietoja Azure-tallennustilan tilit](./storage/storage-create-storage-account.md)


### <a name="subscription"></a>tilauksen  
Asiakkaan sopimus Microsoftin, että ne voivat hankkia Azure palveluiden kanssa. Tilauksen hinnat ja toisiinsa liittyviä termejä on noudatettava valinnut tilauksen tarjous. Katso [Microsoft Online-tilauksen sopimuksen](https://azure.microsoft.com/support/legal/subscription-agreement/).  
Katso myös, [miten Azure tilaukset liittyvät Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md)


### <a name="tag"></a>tunniste  
Indeksoinnin termi, jonka avulla voit luokitella resurssien hallinta tai laskutuksen tarpeen mukaan. Voit käyttää tunnisteita, kun resurssiryhmien ja resurssien monimutkaisia kokoelman eikä sinun tarvitse visualisointi tapaa, jolla tarkoituksenmukaisessa näitä varoja. Voi esimerkiksi tunnisteen resurssit, jotka toimivat samalla rooli organisaatiossa tai saman osaston kuulut.  
Katso myös [käyttäminen tunnisteen, kun haluat järjestää Azure resurssit](resource-group-using-tags.md)


### <a name="update-domain"></a>Päivitä toimialueen  
Kokoelma näennäiskoneiden käytettävyys määrittäminen, jotka päivittyvät samanaikaisesti. Päivitä toimialueella näennäiskoneiden käynnistyvät yhdessä suunnitellun ylläpidon aikana. Azure käynnistyy useampi kuin yksi toimialue koskaan kerrallaan. Kutsutaan myös päivityksen toimialueen.  
Katso myös [Windows näennäiskoneiden käytettävyyden hallinta](./virtual-machines/virtual-machines-windows-manage-availability.md) tai [Hallitse Linux näennäiskoneiden käytettävyys](./virtual-machines/virtual-machines-linux-manage-availability.md)  


### <a name="vm"></a>virtuaalikoneen  
Fyysinen tietokoneeseen, jossa on käyttöjärjestelmä ohjelmiston käyttöönoton. Useita näennäiskoneiden voidaan suorittaa samanaikaisesti samassa laitteessa. Azure-näennäiskoneiden on käytettävissä erilaisia koot.  
Katso myös [näennäiskoneiden dokumentaatio](https://azure.microsoft.com/documentation/services/virtual-machines/)


### <a name="vm-extension"></a>virtuaalikoneen tunniste  
Resurssi, joka toteuttaa toiminnan tai ominaisuudet, jotka joko auttavat muut ohjelmat toimi tai anna voi olla vuorovaikutuksessa mahdollisuuden käynnissä tietokoneen kanssa. AM Access-tunniste avulla voi esimerkiksi Palauta tai muokata Azure virtuaalikoneen etäkäyttöpalvelimen arvot.  
Katso myös [tietoja virtuaalikoneen tunnisteet ja toiminnot (Windows)](./virtual-machines/virtual-machines-windows-extensions-features.md) tai [virtuaalikoneen tunnisteet ja ominaisuudet (Linux)](./virtual-machines/virtual-machines-linux-extensions-features.md)


### <a name="vnet"></a>VPN  
Verkon, joka sisältää Azure resurssit, jotka eristetään muista Azure alihallinnat väliset yhteydet. Se voidaan yhdistää muiden Azure virtual käyttäminen [Azure VPN-yhdyskäytävän](./vpn-gateway/vpn-gateway-about-vpngateways.md) kautta ja käyttämällä [useita vaihtoehtoja](./vpn-gateway/vpn-gateway-cross-premises-options.md)paikalliseen verkkoon. Voit hallita täysin esimerkiksi IP-osoitelohkot DNS asetukset, suojauskäytäntöjä ja reitin verkoston taulukoista.  
Katso myös [VPN-yleiskatsaus](./virtual-network/virtual-networks-overview.md)  

###<a name="see-also"></a>**Katso myös**  
- [Azure käytön aloittaminen](https://azure.microsoft.com/get-started/)
- [Cloud Resurssikeskus](https://azure.microsoft.com/resources/)  
- [Azure business-sovelluksessa](https://azure.microsoft.com/overview/business-apps-on-azure/)
- [Azure-että palvelinkeskukseen](https://azure.microsoft.com/overview/business-apps-on-azure/) 





