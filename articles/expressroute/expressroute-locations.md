<properties
   pageTitle="ExpressRoute sijainnit | Microsoft Azure"
   description="Tässä artikkelissa on sijainnit yksityiskohtainen yhteenveto, jossa tarjotaan palveluja ja Azure alueiden yhdistämisestä."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute kumppanit ja peering sijainnit

Tässä artikkelissa taulukot Anna tiedot ExpressRoute yhdistämispalvelua tarjoajat, ExpressRoute soveltamisalue ExpressRoute ja ExpressRoute järjestelmän muutetaan (SIs) tuettu Microsoftin pilvipalveluihin.

## <a name="partners"></a>ExpressRoute yhdistämispalvelua tarjoajat

ExpressRoute tuetaan kaikki Azure alueiden ja sijainnit. Seuraavat kartan sisältää Azure alueiden ja ExpressRoute sijaintien luettelon. ExpressRoute sijainnit viitata ne, jossa Microsoft kollegat useita palveluntarjoajia kanssa.

![Sijainnin yhdistämismääritys][0]

Jos olet yhdistettynä vähintään yksi ExpressRoute sijainti geopoliittisten alueella, on Azure-palvelujen kaikkien alueiden geopoliittisten alueen yli. Seuraavassa taulukossa on kartan Azure alueiden ExpressRoute sijainteihin geopoliittisten alueella.

|**Geopoliittisten alue**|**Azure alueet**|**ExpressRoute sijainnit**|
|---|---|---|
|**Pohjois-Amerikka**|Yhdysvaltojen Itä, Länsi US, Yhdysvaltojen Itä 2, Yhdysvaltojen Keski, Etelä keskitetyn US, Pohjois keskitetyn Yhdysvaltain, Kanadan Keski, joten Itä|ATLANTA Chicagon Dallas, Las Vegas, Helsinki, New York, Seattle, piin laakso, Washington Ohjauskoneen, Montrealin +, Quebec kaupunki +, Toronto|
|**Etelä-Amerikka**|Brasilia Etelä|Sao Juha|
|**Europe**|Pohjois-Eurooppa, Länsi Europe UK Länsi UK Etelä|Amsterdam Dublin Lontoo, Newport(Wales) +, Pariisi|
|**Aasian**|Itä-Aasian varaaja Aasian|Hongkong, Singapore|
|**Japani**|Japanilainen Länsi ja Itä japani|Osaka, Tokiosta|
|**Australia**|Australia varaaja Australia Itä|Melbourne, Sydneyn|
|**Intia**|Intia Länsi Intia Keski-Intia Etelä|Chennai, Mumbai|



Alla olevassa taulukossa sisältää tietoja alueiden ja geopoliittisten rajat kansallisia paveikslėlis.

|**Geopoliittisten alue**|**Azure alueet**|**ExpressRoute sijainnit**|
|---|---|---|---|
|**US Government cloud**|Yhdysvaltain gov – Iowa, Yhdysvaltain gov – Virginia|Chicago-Dallas, New Yorkissa, Washington Ohjauskoneen|
|**Kiina**|Kiinan Pohjoinen, Kiinan Itä|Beijing, Shanghain kunta|
|**Saksa**|Saksa Keski, saksa Itä|Berliini, Frankfurt|


Yhteyksien geopoliittisten alueiden välillä ei tueta vakio ExpressRoute tuote. Sinun on yleinen connectivity tukemaan ExpressRoute premium-lisäosan käyttöön. Kansallinen cloud-ympäristössä yhteys ei tueta. Voit käsitellä yhteyden palveluntarjoajan Jos tällaisia tarpeen vaatiessa.


## <a name="connectivity-provider-locations"></a>Yhteyden tarjoaja sijainnit

> [AZURE.SELECTOR]
[Sijaintien tarjoajan](expressroute-locations.md#connectivity-provider-locations)
[tarjoajien sijainnin mukaan](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Tuotannon Azure

| **Palveluntarjoaja**  |**Microsoft Azure** | **Office 365: n ja CRM Online** | **Sijainnit** |
|-----------------------|--------------------|----------------|---------------|
| **AARNet** | Tuettu | Tuettu | Melbourne, Sydneyn |
| **[Aryaka verkot]( http://www.aryaka.com/)** | Tuettu | Tuettu | Amsterdam, piin laakso, Singapore, Tokiosta, Washington Ohjauskoneen |
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Tuettu | Tuettu | Amsterdam, Chicagon, Dallas, Lontoo, piin laakso, Singapore, Sydneyn, Washington Ohjauskoneen |
| **[British Telecom]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | Tuettu | Tuettu | Amsterdam, Hongkong, Lontoo, piin laakso, Singapore, Sydneyn, Tokiosta, Washington Ohjauskoneen |
|**CenturyLink** | Tulossa pian | Tulossa pian| Piin laakso |
|**Kiinan Telecom Yleinen** | Tuettu | Ei tueta | Hongkong |
|**[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** | Tuettu | Tulossa pian | Dallas Montrealin + Toronto |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  Tuettu | Tuettu | Amsterdam, Dublin, Helsinki Tokiosta |
| **Comcast** | Tuettu | Tuettu | Chicago-piin laakso, Washington Ohjauskoneen |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** | Tuettu | Tuettu | Helsinki | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Tuettu | Tuettu | Amsterdam Atlanta Chicagon, Dallas, Hongkong, Lontoo, Helsinki, Melbourne, New York, Osaka, Pariisi +, Sao Juha, Seattle, piin laakso, Singapore, Sydneyn, Tokiosta, Toronto, Washington Ohjauskoneen |
| **euNetworks** |  Tuettu | Tuettu | Amsterdam |
| **GÉANT** | Tuettu | Tuettu | Amsterdam |
| **[Internet aloite japani Inc. - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |  Tuettu | Tuettu | Osaka, Tokiosta |
| **[InterCloud]( https://www.intercloud.com/)** | Tuettu | Tuettu | Amsterdam, Lontoo, Singapore, Washington Ohjauskoneen |
| **Internet-ratkaisut - pilvi yhdistäminen** | Tuettu | Tuettu | Amsterdam, Lontoo |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)**  | Tuettu | Tuettu | Amsterdam, Lontoo, Pariisi |
| **Jisc** | Tuettu | Tuettu | Lontoo | 
| **[Tason 3 viestintä]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Tuettu | Tuettu | Amsterdam Chicagon Dallas, Las Vegas +, Lontoo, Seattle, piin laakso, Washington Ohjauskoneen |
| **Megaport** | Tuettu | Tuettu | Dallas, Hongkong, Las Vegas, Helsinki, Melbourne, New York, Seattle, Singapore, Sydneyn, Washington Ohjauskoneen |
| **MTN** | Tuettu | Tuettu | Lontoo |
| **Seuraava tietojen** | Tulossa pian | Tulossa pian | Newport(Wales) + |
| **NEXTDC** | Tuettu | Tuettu | Melbourne, Sydneyn |
| **NTT viestintä** | Tuettu | Tuettu | Lontoo, Helsinki, Osaka, Tokiosta |
| **[Oranssi]( http://www.orange-business.com/en/products/business-vpn-galerie)** | Tuettu | Tuettu | Amsterdam, Hongkong, Lontoo piin laakso, Singapore, Sydneyn, Washington Ohjauskoneen |
| **Yleinen PCCW rajoitettu** | Tuettu | Tuettu | Hongkong |
| **[SingTel]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  Tuettu | Tuettu | Singapore |
| **Softbank** | Tuettu | Tuettu | Osaka, Tokiosta | 
| **[Tata viestintä](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | Tuettu | Tuettu | Amsterdam, Chennai, Hongkong Kiinan, Helsinki Mumbai piin laakso, Singapore, Washington Ohjauskoneen |
| **[TeleCity-ryhmä]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | Tuettu | Tuettu | Amsterdam, Dublin, Lontoo |
| **Telefonica** | Tuettu | Tuettu | Sao Juha |
| **Telenor** | Tuettu | Tuettu | Amsterdam, Lontoo |
| **[Telstra Corporation]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | Tuettu | Tuettu | Melbourne, Sydneyn |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | Tuettu | Tuettu | Amsterdam, Hongkong, Lontoo, piin laakso, Singapore, Sydneyn, Tokiosta, Washington Ohjauskoneen |
| **Vodafone** | Tuettu | Ei tueta | Lontoo | 
| **[Zayo-ryhmä]( http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** | Tuettu | Tuettu | Chicago-Helsinki, New Yorkissa piin laakso Toronto, Washington Ohjauskoneen |

 **+**merkitsee tulossa pian

### <a name="national-cloud-environments"></a>Kansallinen cloud-ympäristössä

#### <a name="us-government-cloud"></a>US Government cloud

| **Palveluntarjoaja**  |**Microsoft Azure** | **Office 365-palveluun** | **Sijainnit** |
|-----------------------|--------------------|----------------|---------------|
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Tuettu | Tuettu | Chicagon, Washington Ohjauskoneen |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Tuettu | Tuettu | Chicago-Dallas, New Yorkissa, Washington Ohjauskoneen |
| **[Tason 3 viestintä]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Tuettu | Tuettu | Chicagon, New Yorkissa +, Washington Ohjauskoneen |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | Tuettu | Tuettu | Chicago-Dallas + New Yorkissa, Washington Ohjauskoneen |

#### <a name="china"></a>Kiina

| **Palveluntarjoaja**  |**Microsoft Azure** | **Office 365-palveluun** | **Sijainnit** |
|-----------------------|--------------------|----------------|---------------|
| **Kiinan tietoliikennesuunnitelma** | Tuettu | Ei tueta | Beijing, Shanghain kunta|
Lisätietoja on artikkelissa [ExpressRoute Kiinassa](http://www.windowsazure.cn/home/features/expressroute/).

#### <a name="germany"></a>Saksa

| **Palveluntarjoaja**  |**Microsoft Azure** | **Office 365-palveluun** | **Sijainnit** |
|-----------------------|--------------------|----------------|---------------|
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** | Tuettu | Ei tueta | Berliini +, Frankfurt|
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Tuettu | Ei tueta | Frankfurt|
| **e shelter** | Tuettu | Ei tueta | Berliini|
| **Interxion** | Tuettu | Ei tueta | Frankfurt|

## <a name="nonpartners"></a>Yhteyksien kautta palveluntarjoajien eivät näy

Jos yhteys-palvelu ei ole luettelossa edellisten kohtien, voit luoda yhteyden.

- Tarkista yhteys palvelussa ovatko ne ovat yhteydessä jonkin yllä olevassa taulukossa vaihdot. Voit tarkistaa kerää lisätietoja exchange tarjoajan tarjoamien palvelujen linkkejä. Useita yhdistämispalvelua tarjoajat on jo muodostettu Ethernet vaihdot.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- On yhteys-palveluntarjoajan laajentaa verkon valinta peering haluamaasi kohtaan.
    - Varmista, että yhteys-palveluntarjoajan laajentaa yhteys käytettävissä tavalla, niin, että ei ole pisteet virheestä.
- Tilauksen ExpressRoute-piiri, jossa vaihdon kuin yhteyden palveluntarjoajan muodostaa yhteyden Microsoft.
    - Voit määrittää yhteyden [luominen ExpressRoute piiri](expressroute-howto-circuit-classic.md) vaiheet.

|**Yhteys-palvelu**|**Exchange**|**Sijainnit**|
|---|---|---|
|**[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)**|Equinix|Singapore|
|**Alaskan viestintä**|Equinix|Seattle|
|**[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure )**|Equinix|New Yorkin, Washington Ohjauskoneen|
|**[XO viestintä](http://www.xo.com/)**|Equinix|Piin laakso|


## <a name="expressroute-system-integrators"></a>ExpressRoute järjestelmän muutetaan

Ottaminen käyttöön yksityinen yhteys tarpeen mukaan voi olla hankalaa, verkon asteikon perusteella. Voit käsitellä jotakin auttamaan onboarding ExpressRoute seuraavassa taulukossa lueteltuja järjestelmän muutetaan.

|**Järjestelmän integraattori**|**Manner**|
|---|---|
|**[Avanade Inc.](http://www.avanade.com/)**| Aasian, Europe, USA |
|**[DotNet ratkaisut](http://www.dotnetsolutions.co.uk/)**| Europe |
|**[Equinix Professional-palvelut](http://www.equinix.com/services/consulting/)**|US|
|**[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Aasian |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | US |
|**[Projektin johtajuus](http://www.projectleadership.net/azure)** | US |

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat lisätietoja ExpressRoute [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md).
- Varmista, että kaikki edellytykset täyttyvät. Katso [ExpressRoute edellytykset](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Sijainnin yhdistämismääritys"
