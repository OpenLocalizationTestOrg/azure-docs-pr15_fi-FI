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
| **Sijainti**  | **Palveluntarjoajien** |
|---------------|-----------------------|
| **Amsterdam** | AT & T NetBond, British Telecom, Colt, Equinix, euNetworks, Aryaka-verkoissa GÉANT, InterCloud, Solutions Internet - yhteyden Cloud Interxion-tason 3 viestinnän, oranssi, Tata viestintä-TeleCity-ryhmässä Telenor, Verizon |
| **ATLANTA** | Equinix |
| **Chennai** | Tata viestintä |
| **Chicagon** | AT & T NetBond, Comcast, Equinix-tason 3 tietoliikenne Zayo-ryhmä |
| **Dallas** | AT & T NetBond, Cologix, Equinix-tason 3 tietoliikenne Megaport |
| **Dublin** | Colt, Telecity-ryhmä |
| **Hongkong** | Yleinen British Telecom, kiina Telecom, Equinix, Megaport, oranssi, yleinen PCCW rajoitettu, Tata tietoliikenne Verizon |
| **Lontoo** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, ratkaisujen Internet - yhteyden pilveen, Interxion, Jisc-tason 3 viestinnän, MTN, NTT viestinnän, oranssi, Tata viestintä-Telecity-ryhmässä Telenor, Verizon, Vodafone |
| **Las Vegas** | Tason 3 Communications +, Megaport
| **Helsinki** | CoreSite, Equinix, Megaport, NTT, Zayo-ryhmä |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New Yorkin** | Equinix, Megaport, Zayo ryhmä |
| **Newport(Wales) +** | Seuraava luonti tietojen + |
| **Montrealin** | Cologix + |
| **Mumbai** | Tata viestintä |
| **Osaka** | Equinix Internet aloite japani Inc. - IIJ, NTT viestintä, Softbank |
| **Pariisi** | Interxion, Equinix + |
| **Sao Juha** | Equinix, Telefonica |
| **Seattle** | Equinix-tason 3 tietoliikenne Megaport |
| **Piin laakso** | Aryaka verkkojen AT & T NetBond, British Telecom, CenturyLink +, Comcast, Equinix, tason 3 viestinnän, oranssi, Tata viestintä-Verizon, Zayo-ryhmä |
| **Singapore** | Aryaka verkkojen AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, oranssi, SingTel, Tata tietoliikenne, Verizon |
| **Sydneyn** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, oranssi, Telstra Corporation, Verizon |
| **Tokiosta** | Aryaka verkkojen British Telecom, Colt, Equinix, Internet aloite japani Inc. - IIJ, NTT viestintä, Softbank-Verizon |
| **Toronto** | Cologix, Equinix, Zayo ryhmä |
| **Washingtonin Ohjauskoneen** | Aryaka verkkojen AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, tason 3 viestinnän, Megaport, oranssi, Tata viestintä-Verizon, Zayo-ryhmä |

 **+**merkitsee tulossa pian

### <a name="national-cloud-environments"></a>Kansallinen cloud-ympäristössä

#### <a name="us-government-cloud"></a>US Government cloud

| **Sijainti**  |**Palveluntarjoajien** |
|---------------|--------------------|
| **Chicagon** | AT & T NetBond Equinix-tason 3 tietoliikenne Verizon |
| **Dallas** |  Equinix, Verizon + |
| **New Yorkin** | Equinix-tason 3 Communications +,-Verizon |
| **Washingtonin Ohjauskoneen** | AT & T NetBond Equinix-tason 3 tietoliikenne Verizon |

#### <a name="china"></a>Kiina

| **Sijainti**  | **Palveluntarjoajien** |
|---------------|-----------------------|
| **Beijing** | Kiinan tietoliikennesuunnitelma |
| **Shanghain kunta** |  Kiinan tietoliikennesuunnitelma |
Lisätietoja on artikkelissa [ExpressRoute Kiinassa](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Saksa

| **Sijainti**  | **Palveluntarjoajien** |
|---------------|-----------------------|
| **Berliini** | Colt + e shelter |
| **Frankfurt** | Colt, Equinix, Interxion |

## <a name="nonpartners"></a>Yhteyksien kautta palveluntarjoajien eivät näy

Jos yhteys-palvelu ei ole luettelossa edellisten kohtien, voit luoda yhteyden.

- Tarkista yhteys palvelussa ovatko ne ovat yhteydessä jonkin yllä olevassa taulukossa vaihdot. Voit tarkistaa kerää lisätietoja exchange tarjoajan tarjoamien palvelujen linkkejä. Useita yhdistämispalvelua tarjoajat on jo muodostettu Ethernet vaihdot.

    - [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- On yhteys-palveluntarjoajan laajentaa verkon valinta peering haluamaasi kohtaan.
    - Varmista, että yhteys-palveluntarjoajan laajentaa yhteys käytettävissä tavalla, niin, että ei ole pisteet virheestä.
- Tilauksen ExpressRoute-piiri, jossa vaihdon kuin yhteyden palveluntarjoajan muodostaa yhteyden Microsoft.
    - Voit määrittää yhteyden [luominen ExpressRoute piiri](expressroute-howto-circuit-classic.md) vaiheet.

|**Sijainti**|**Exchange**|**Yhdistämispalvelua tarjoajat**|
|-------------|------------|-------------------------|
| **New Yorkin** | Equinix | Lightower |
| **Seattle** | Equinix | Alaskan viestintä |
| **Piin laakso** | Equinix | XO viestintä |
| **Singapore** | Equinix | 1CLOUDSTAR |
| **Washingtonin Ohjauskoneen** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute järjestelmän muutetaan

Yksityinen yhteys tarpeidesi käyttöönotto voi olla hankalaa, verkon asteikon perusteella. Voit käsitellä jotakin auttamaan onboarding ExpressRoute seuraavassa taulukossa lueteltuja järjestelmän muutetaan.

|**Manner**|**Järjestelmän muutetaan**|
|-------------|---------------------|
| **Aasian** | Avanade Inc. OneAs1a|
| **Europe** | Avanade Inc. Dotnet ratkaisut|
| **US** | Avanade Inc., Equinix Professional-palveluihin, Perficient tai projektin johtajuus|

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat lisätietoja ExpressRoute [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md).
- Varmista, että kaikki edellytykset täyttyvät. Katso [ExpressRoute edellytykset](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Sijainnin yhdistämismääritys"
