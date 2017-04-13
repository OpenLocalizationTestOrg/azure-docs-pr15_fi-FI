<properties
   pageTitle="ExpressRoute hyväksymisen edellytyksistä | Microsoft Azure"
   description="Tällä sivulla on luettelo vaatimuksista on täytyttävä, ennen kuin voit tilata Azure ExpressRoute piiri."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute edellytykset & tarkistusluettelo  

Tarvitset muodostaa yhteyttä Microsoftin pilvipalveluihin ExpressRoute avulla voit varmistaa, että lueteltu alla olevissa osioissa seuraavat vaatimukset täyttyvät.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure-tili

- On voimassa ja aktiivinen Microsoft Azure-tili. Tämä edellyttää ExpressRoute piiri asetukset. ExpressRoute piirit ole sisällä Azure tilaukset. Azure tilaus on tarpeen, vaikka yhteys on rajoitettu Azure Microsoftin pilvipalveluihin, kuten Office 365-palveluissa ja CRM online.
- Aktiivinen Office 365-tilaus (Jos käytät Office 365: n palveluita). Lisätietoja tämän artikkelin kohdassa [Office 365: n erityisiä vaatimuksia](#office-365-specific-requirements) .

## <a name="connectivity-provider"></a>Yhteys-palvelu
- Voit käsitellä [ExpressRoute connectivity kumppanin](expressroute-locations.md#partners) muodostaa yhteyden Microsoft cloud. Voit määrittää paikallisen verkko- ja Microsoft välinen yhteys [kolmella](expressroute-introduction.md#howtoconnect)eri tavalla. 
- Jos palveluntarjoajan ei ole ExpressRoute-yhteyksien kumppani, voit edelleen muodostaa Microsoft [cloud exchange-palvelu](expressroute-locations.md#nonpartners)pilveen.

## <a name="network-requirements"></a>Verkkovaatimukset
- **Tarpeettomien yhteys**: ei tarvita redundancy-fyysinen yhteys ja palveluntarjoajan välillä. Microsoft vaatii tarpeettomat erityisen istuntoon ja määritettävä Microsoftin reitittimen ja peering reitittimen myös silloin, kun sinulla on vain [yksi fyysinen yhteys pilveen Exchange-palvelimeen](expressroute-faqs.md#onep2plink). 
- **Reititys**: sen mukaan, kuinka haluat yhdistää Microsoft Cloud, etkä palveluntarjoajan on voit määrittää ja hallita [Reititys](expressroute-circuit-peerings.md)toimialueiden erityisen-istuntoja. Jotkin Ethernet connectivity tai cloud exchange-palveluntarjoajalta saattaa tarjota erityisen hallintaa Lisää palvelu arvo.
- **NAT**: Microsoft hyväksyy vain julkisten IP-osoitteiden – Microsoft peering. Jos käytössäsi on yksityisten IP-osoitteiden kuin paikalliseen verkkoon kirjauduttaessa sinä tai tarjoajan haluat kääntää yksityinen IP-osoitteiden julkiseen IP korjaa [NAT avulla](expressroute-nat.md).
- **QoS**: Skype for Business on useita erilaisia palveluja (kuten ääni, video-ja teksti), jotka edellyttävät eritellä QoS käsittelyä. Sinun ja palveluntarjoajan pitäisi seurata [QoS vaatimuksia](expressroute-qos.md).
- **Verkkosuojaus**: Ota huomioon [verkkosuojaus](../best-practices-network-security.md) , kun yhteyden muodostaminen Microsoft-Cloud ExpressRoute kautta.
 
## <a name="office-365"></a>Office 365-palveluun

Jos haluat ottaa käyttöön Office 365: n ExpressRoute, lue lisätietoja Office 365: n järjestelmävaatimusten seuraavat asiakirjat.


- [Office 365: n ExpressRoute yleiskatsaus](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Office 365: n kanssa ExpressRoute reititys](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365: n URL-osoitteet ja IP-osoitealueet](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Verkkosuunnittelu ja suorituskyvyn parantaminen Office 365: ssä](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Verkon kaistanleveyden laskureita ja työkaluja](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365: n integrointi paikallisten ympäristöjen kanssa](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online 
Jos haluat ottaa käyttöön CRM Online-ExpressRoute, lue lisätietoja CRM Online seuraavat asiakirjat

- [CRM URL-osoitteet](https://support.microsoft.com/kb/2655102) ja [IP-osoitealueet](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Seuraavat vaiheet

- Saat lisätietoja ExpressRoute [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md).
- Etsi tarjoajan ExpressRoute yhteys. Katso [ExpressRoute kumppanit ja peering sijainnit](expressroute-locations.md).
- Lisätietoja [Reititys](expressroute-routing.md), [NAT](expressroute-nat.md) ja [QoS](expressroute-qos.md)koskevat vaatimukset.
- Määritä ExpressRoute-yhteys.
    - [Luo ExpressRoute piiri](expressroute-howto-circuit-classic.md)
    - [Määritä reititys](expressroute-howto-routing-classic.md)
    - [Linkin VNet ExpressRoute piiri](expressroute-howto-linkvnet-classic.md)

