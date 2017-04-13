<properties 
   pageTitle="ExpressRoute vianmääritys Guide - ARP taulukoiden hakeminen | Microsoft Azure"
   description="Tämä sivu sisältää ohjeet ExpressRoute piiri taulukoiden ARP hakeminen"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="ganesr"/>

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>ExpressRoute vianmääritys guide - käytön ARP taulukoiden resurssien hallinnan käyttöönottomalli

> [AZURE.SELECTOR]
[PowerShell - Resurssienhallinta](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell – perinteinen](expressroute-troubleshooting-arp-classic.md)

Tässä artikkelissa käydään läpi vaiheet ExpressRoute piiri ARP taulukoiden tietoja. 

>[AZURE.IMPORTANT] Tämä asiakirja on tarkoitus auttaa vianmäärityksen ja korjauksen yksinkertaisia ongelmia. Se ei ole tarkoitettu on Microsoft-tuen korvaa. Sinun on avattava tuki lippu [Microsoft-tuen](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) kanssa, jos et voi käyttää alla kuvattuja ohjeita ongelman ratkaisemiseen.

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Osoite tarkkuus-protokolla (ARP) ja ARP taulukot
Tarkkuus ARP (Address Protocol) on määritetty [RFC 826](https://tools.ietf.org/html/rfc826)tason 2 protokolla. ARP käytetään Ethernet-osoite (MAC-osoite) ip-osoitteeseen.

ARP-taulukossa on ipv4-osoite ja MAC-osoitteen tietyn peering määritystä. ARP taulukon ExpressRoute piiri peering sisältää seuraavat tiedot kunkin liittymän (ensisijainen ja toissijainen)

1. Määritystä paikallisen reitittimen liittymän ip-osoitteen MAC-osoite
2. Määritystä ExpressRoute reitittimen interface ip-osoite MAC-osoite
3. Yhdistämismäärityksen ikä

ARP taulukoita voi auttaa Hyväksy tason 2 asetukset ja 2 yhteysongelmien vianmääritys basic kerroksen. 

Esimerkki ARP taulukko: 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Seuraavassa osassa on tietoja siitä, miten voit tarkastella tarkastella ExpressRoute reunan reitittimen ARP taulukot. 

## <a name="prerequisites-for-learning-arp-tables"></a>Edellytykset käyttöön liittyviä ARP taulukot

Varmista, että seuraavat ennen edelleen edistyminen

 - Kelvollinen ExpressRoute piiri, määritetty vähintään yksi peering. Virtapiirin on määritettävä täysin connectivity tarjoaja. Voit (tai yhteys-palveluntarjoajan) on määrittänyt vähintään yksi peerings (Azure yksityiset, Azure julkinen ja Microsoftin) tämä piiri.
 - IP-osoitealueet käytettäviä määrittäminen peerings (Azure yksityiset, Azure julkinen ja Microsoft). Tarkista ip address varauksen esimerkeissä saat tietoja siitä, miten ip-osoitteiden on yhdistetty liittymät oman reunassa ja ExpressRoute reunassa [ExpressRoute reititys vaatimukset-sivu](expressroute-routing.md) . Saat tietoja peering määritysten tarkastelemalla [ExpressRoute peering määritys-sivulla](expressroute-howto-routing-arm.md).
 - Verkko-työryhmän tiedot / connectivity tarjoajan liittymät MAC-osoitteet käyttää seuraavia IP-osoitteet.
 - Sinun on oltava uusimmat PowerShell-moduulin Azure (1.50 tai uudempi versio).

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>ExpressRoute piiri taulukoiden ARP hakeminen
Tässä osassa on ohjeet ARP taulukot kohti peering PowerShellin avulla voit tarkastella. Voit tai connectivity-palveluntarjoajan on määrittänyt peering ennen etenemistä edelleen. Kunkin piiri on kaksi polkua (ensisijainen ja toissijainen). Voit tarkistaa ARP-taulukon kunkin polun erikseen.

### <a name="arp-tables-for-azure-private-peering"></a>ARP taulukoiden Azure yksityinen peering
Seuraavan cmdlet-komennon tarjoaa ARP taulukoiden Azure yksityinen peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

Esimerkki tulos näkyy alla johonkin polut

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>ARP taulukoiden Azure julkisen peering
Seuraavan cmdlet-komennon tarjoaa ARP taulukoiden Azure julkisen peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


Esimerkki tulos näkyy alla johonkin polut

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP taulukoiden Microsoft peering
Seuraavan cmdlet-komennon tarjoaa ARP taulukoiden Microsoft peering

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


Esimerkki tulos näkyy alla johonkin polut

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Voit käyttää näitä tietoja
ARP sisällysluettelossa peering avulla voidaan määrittää Vahvista tason 2 määrittäminen ja yhteyden. Tässä osassa on yleiskatsaus ARP taulukoiden ulkoasun eri tilanteissa.

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>ARP taulukon piirin ollessa tilaan (odotettu tila)

 - ARP-taulukko on merkinnän paikallisen puolen kelvollinen IP-osoite ja MAC-osoite ja samalla tapahtuman Microsoft side. 
 - Paikallisen ip-osoitteen viimeinen oktetti on aina on pariton.
 - Microsoftin ip-osoitteen viimeinen oktetti on aina parillinen.
 - Sama MAC-osoite näkyy kaikki 3 peerings (ensisijainen / toissijainen) Microsoft-puolella. 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP taulukosta, kun paikallisen / connectivity tarjoajan puoli on ongelmia

 - Vain yksi merkintä näkyy ARP-taulukossa. Tämä näyttää yhdistämisen, MAC-osoite ja IP-osoitetta käytetään Microsoft-puolella. 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Avaa tukipyynnön connectivity tarjoajalta näiden ongelmien korjaaminen. 


### <a name="arp-table-when-microsoft-side-has-problems"></a>ARP-taulukosta, kun Microsoft puoli on ongelmia

 - Et näe peering, jos on ongelmia Microsoft-puolella näkyvät ARP-taulukon. 
 -  Avaa [Microsoft tue](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)tuki lippu. Määritä, jossa tason 2 connectivity ongelma jatkuu. 

## <a name="next-steps"></a>Seuraavat vaiheet

 - Vahvista ExpressRoute piiri Layer 3 määritykset
     - Hae reitin yhteenveto määrittämiseksi erityisen istuntoon 
     - Hae reitti-taulukko, voit selvittää, mitkä etuliitteiden ilmoittaa ExpressRoute välillä
 - Vahvista tiedonsiirto tarkastelemalla tavua / ulos
 - Avaa tuki lippu [Microsoft tue](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) Jos ongelmia ilmenee edelleen.
