<properties
   pageTitle="ExpressRoute vianmäärityksen opas: taulukoiden hakeminen ARP | Microsoft Azure"
   description="Tällä sivulla on ohjeet ExpressRoute piiri taulukoiden ARP hakeminen."
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

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>ExpressRoute vianmäärityksen opas: ARP taulukoiden hakeminen perinteinen käyttöönottomalli

> [AZURE.SELECTOR]
[PowerShell - Resurssienhallinta](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell – perinteinen](expressroute-troubleshooting-arp-classic.md)

Tässä artikkelissa käydään läpi vaiheet tarkkuus ARP (Address Protocol)-taulukoissa Azure ExpressRoute piiri varten.

>[AZURE.IMPORTANT] Tämä asiakirja on tarkoitus auttaa vianmäärityksen ja korjauksen yksinkertaisia ongelmia. Se ei ole tarkoitettu on Microsoft-tuen korvaa. Jos ongelma ei ratkennut käyttämällä seuraavia ohjeita, Avaa tukipyynnön [Microsoft Azure ohjeen + tuki](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>Osoite tarkkuus-protokolla (ARP) ja ARP taulukot
ARP on tason 2-protokolla, joka on määritetty [RFC 826](https://tools.ietf.org/html/rfc826). ARP voidaan yhdistää Ethernet-osoite (MAC-osoite) IP-osoite.

ARP-taulukossa on IPv4-osoite ja MAC-osoitteen tietyn peering määritystä. ARP taulukon ExpressRoute piiri peering on kunkin liittymän (ensisijainen ja toissijainen) seuraavat tiedot:

1. Määritystä paikallisen reitittimen liittymän IP-osoitteen MAC-osoite
2. Määritystä ExpressRoute reitittimen liittymän IP-osoitteen MAC-osoite
3. Yhdistämismäärityksen ikä

ARP taulukoita voi auttaa Layer 2 määritysten tarkistaminen ja basic Layer 2-yhteysongelmien vianmääritys.

Seuraavassa on esimerkki ARP-taulukko:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Seuraavassa osassa on tietoja siitä, miten voit tarkastella ARP taulukoita, jotka ovat tarkastella ExpressRoute reunan reitittimen.

## <a name="prerequisites-for-using-arp-tables"></a>Edellytyksistä ARP taulukoiden avulla

Varmista, että sinulla seuraavasti, ennen kuin jatkat:

 - Kelvollinen ExpressRoute-piiri, joka on määritetty vähintään yksi peering. Virtapiirin on määritettävä täysin connectivity tarjoaja. Voit (tai yhteys-palveluntarjoajan) pitää määrittää vähintään yksi peerings (Azure yksityiset, Azure julkinen vai Microsoft) tämä piiri.

 - IP-osoitealueet, joita käytetään määrittäminen peerings (Azure yksityiset, Azure julkinen ja Microsoft). Tarkista IP address varauksen esimerkeissä saat tietoja siitä, miten IP-osoitteiden on yhdistetty liittymät oman aise ja ExpressRoute reunassa [ExpressRoute reititys vaatimukset-sivu](expressroute-routing.md) . Saat peering kokoonpanotietoja tarkastelemalla [ExpressRoute peering määritys-sivulla](expressroute-howto-routing-classic.md).

 - Tietoja verkko ryhmän tai connectivity tarjoajalta MAC-osoitteet liittymät, joita käytetään nämä IP-osoitteet.

 - Uusimmat Windows PowerShell-moduulin Azure (1.50 tai uudempi versio) varten.

## <a name="arp-tables-for-your-expressroute-circuit"></a>ARP taulukoiden ExpressRoute piiri
Tässä osassa on ohjeet voit tarkastella kutakin peering PowerShell-toiminnolla ARP-taulukoita. Ennen kuin jatkat, sinun tai connectivity-palveluntarjoajan on määrittäminen peering. Kunkin piiri on kaksi polkua (ensisijainen ja toissijainen). Voit tarkistaa ARP-taulukon kunkin polun erikseen.

### <a name="arp-tables-for-azure-private-peering"></a>ARP taulukoiden Azure yksityinen peering
Seuraavan cmdlet-komennon tarjoaa ARP taulukoiden Azure yksityinen peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

Seuraavassa on esimerkki tulosteen johonkin polut:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure julkisen peering ARP taulukoiden:
Seuraavan cmdlet-komennon tarjoaa ARP taulukoiden Azure julkisen peering:

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

Seuraavassa on esimerkki tulosteen johonkin polut:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


Seuraavassa on esimerkki tulosteen johonkin polut:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>ARP taulukoiden Microsoft peering
Seuraavat cmdlet-komento on ARP taulukoita varten Microsoft peering:

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


Esimerkki tulos näkyy alla johonkin polut:

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>Voit käyttää näitä tietoja
ARP sisällysluettelon peering voidaan tarkistaa Layer 2 määrittäminen ja yhteyden. Tässä osassa on yleiskatsaus eri skenaarioissa ARP taulukoiden ulkoasua.

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>ARP-taulukosta, kun piirin on toiminnassa (odotettu)-tilassa

 - ARP taulukossa on merkinnän paikallisen puolen kelvollinen IP- ja MAC-osoite ja samalla tapahtuman Microsoft side.
 - Paikallisen IP-osoitteen viimeinen oktetti on aina on pariton.
 - Microsoftin IP-osoitteen viimeinen oktetti on aina parillinen.
 - Saman MAC-osoite näkyy kaikkien kolmen peerings (ensisijainen ja toissijainen) Microsoft-puolella.


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP-taulukosta, kun se on paikallisen tai kun yhteys-toimittaja-sivu on ongelmia

 ARP-taulukossa näkyy vain yksi tapahtuma. Se näyttää yhdistämisen, MAC-osoite ja IP-osoite, jota käytetään Microsoft-puolella.

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] Jos tältä ongelma toistuu, Avaa tukipyynnön connectivity palvelussa voit ratkaista ongelman.


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>ARP-taulukosta, kun Microsoft-sivu on ongelmia

 - Et näe peering, jos on ongelmia Microsoft-puolella näkyvät ARP-taulukon.
 -  Avaa tukipyynnön [Microsoft Azure ohjeen + tuki](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Määritä Layer 2 yhteyttä ongelma jatkuu.

## <a name="next-steps"></a>Seuraavat vaiheet

 - Vahvista ExpressRoute piiri Layer 3 määritykset:
     - Pyydä reitin yhteenveto määrittää erityisen istuntojen tilan.
     - Hae reitti-taulukko, voit selvittää, mitkä etuliitteiden ilmoittaa ExpressRoute yli.
 - Vahvista tiedonsiirto tarkastelemalla tavua sisään ja ulos.
 - Avaa [Microsoft Azure ohjeen + tuki](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) -tukipyynnön, jos ongelmia ilmenee edelleen.
