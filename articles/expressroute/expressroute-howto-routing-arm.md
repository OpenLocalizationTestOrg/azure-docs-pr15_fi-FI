<properties
   pageTitle="Reititys ExpressRoute piiri määrittämisestä | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi vaiheet yksityinen, yleisön ja Microsoft peering ExpressRoute piiri, luominen ja. Tässä artikkelissa myös kerrotaan, miten voit tarkistaa tilan, Päivitä tai poista oman piiri peerings."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Luoda ja muokata reititys ExpressRoute piiri


> [AZURE.SELECTOR]
[Azure Portal - Resurssienhallinta](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Resurssienhallinta](expressroute-howto-routing-arm.md)
[PowerShell – perinteinen](expressroute-howto-routing-classic.md)



Tässä artikkelissa käydään läpi vaiheet ja luoda ja hallita reititys määrityskohde ExpressRoute-piiri, käyttämällä PowerShell ja Azure resurssien hallinnan käyttöönottomalli.  Seuraavien vaiheiden kuvissa olevia myös noudattamalla deprovision ExpressRoute piiri peerings ja Tarkista tila, Päivitä tai poista. 


**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Määritysten edellytykset

- Sinun on PowerShellin Azure-moduuleja uusin versio 1.0 tai uudempi. 
- Varmista, että olet tarkistanut [edellytykset](expressroute-prerequisites.md) -sivulla, [vaatimukset reititys](expressroute-routing.md) -sivulle ja [Työnkulut](expressroute-workflows.md) -sivu ennen kuin aloitat määritys.
- Sinulla on oltava aktiivinen ExpressRoute-piiri. Luo [ExpressRoute piiri](expressroute-howto-circuit-arm.md) ohjeiden mukaisesti ja olet ottanut connectivity-palveluntarjoajan, ennen kuin jatkat piiri. ExpressRoute piiri on oltava valmisteltu ja käytössä-tilaan, voivat suorittaa jäljempänä Cmdlet-komentoja.

Nämä ohjeet koskevat vain piirit luotu ojentamassa Layer 2 yritystietojen yhdistämispalvelut palveluntarjoajia. Jos käytössäsi on palveluntarjoajan tarjoaminen hallittuja Layer 3-palveluja (yleensä IPVPN, kuten MPLS), yhteys-palveluntarjoajan määrittäminen ja hallinta reititys puolestasi.

>[AZURE.IMPORTANT] Emme ole mainostaa peerings määrittämän palveluntarjoajien palvelun hallinta-portaalin kautta. Yritämme tätä ominaisuutta pian ottamisesta käyttöön. Tarkista palveluntarjoajalta ennen erityisen peerings määrittämistä.

Voit määrittää yhden, kahden tai kaikki kolme peerings (Azure yksityiset, Azure julkinen ja Microsoft), ExpressRoute piiri. Voit määrittää peerings valitset järjestyksessä. Kuitenkin varmistamalla, että suoritat kunkin peering yksi kerrallaan määritys. 

## <a name="azure-private-peering"></a>Azure yksityinen peering

Tässä osassa on ohjeita siitä, miten voit luoda, Hae, päivittäminen ja poistaminen ExpressRoute piiri Azure yksityinen peering määrityskohde. 

### <a name="to-create-azure-private-peering"></a>Voit luoda Azure yksityinen peering

1. Tuo PowerShell-moduulin ExpressRoute varten.
    
    Asenna uusimmat PowerShell-installer [PowerShell](http://www.powershellgallery.com/) -valikoimasta ja Azure Resurssienhallinta-moduulit tuominen PowerShell-istunnon jotta voit aloittaa ExpressRoute Cmdlet-komentoja. Tarvitset PowerShell Suorita järjestelmänvalvojana.

        Install-Module AzureRM

        Install-AzureRM

    Tuo kaikki AzureRM.* moduulit semanttinen palvelinversion alueen sisällä

        Import-AzureRM

    Voit tuoda yksinkertaisesti Valitse moduuli semanttinen palvelinversion alueen sisällä 
        
        Import-Module AzureRM.Network 

    Tiliisi kirjautuminen

        Login-AzureRmAccount

    Valitse luotavan ExpressRoute piiri tilaus
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Luo ExpressRoute piiri.
    
    Ohjeiden luominen [ExpressRoute piiri](expressroute-howto-circuit-arm.md) ja sen connectivity tarjoaja valmistelun yhteydessä. 

    Jos yhteys-palveluntarjoajan tarjoaa hallittuja Layer 3-palveluja, voit pyytää yhteyden palveluntarjoajan käyttöön Azure yksityinen peering puolestasi. Tässä tapauksessa sinun ei tarvitse ohjeiden seuraavien osien luettelossa. Jos yhteys-palveluntarjoajan Hallitse reititys, että piiri luomisen jälkeen, noudata seuraavia ohjeita. 

3. Tarkista ExpressRoute-piiri, jotta se on valmisteltu.

    Kuittaa ensin ExpressRoute piiri on valmisteltu ja myös käytössä. Katso alla oleva esimerkki.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Vastaus on suurin piirtein seuraavanlaisen alla olevassa esimerkissä:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Määritä Azure yksityinen peering virtapiirin varten.

    Varmista, että sinulla on seuraavat seikat, ennen kuin jatkat seuraavat vaiheet:

    - /30 aliverkon ensisijainen linkin. Tämä ei saa olla varattu virtual verkkojen minkä tahansa osoitetilan osa.
    - /30 aliverkon toissijainen linkin. Tämä ei saa olla varattu virtual verkkojen minkä tahansa osoitetilan osa.
    - Kelvollinen VLAN-tunnus muodostaa peering käyttöön. Varmista, että ei ole muita peering piirissä käyttää samaa VLAN ID-tunnuksellasi.
    - Numero peering nimellä. Voit käyttää 2-tavuinen ja 4-tavuinen LUKUINA. Voit määrittää yksityisen kuin tässä peering numero. Varmista, että käytössäsi ei ole 65515.
    - MD5 hash, jos haluat käyttää. **Tämä on valinnainen**.
    
    Voit suorittaa seuraavan cmdlet-komennon määrittäminen Azure yksityinen peering oman piiri varten.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Voit käyttää cmdlet-komennon alla, jos haluat käyttää MD5 hash.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Varmista, että määrität peering ASN asiakkaan ASN liitetään numero.

### <a name="to-view-azure-private-peering-details"></a>Voit tarkastella Azure yksityinen peering tietoja

Saa tiedot seuraavan cmdlet-komennon avulla

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Päivitä Azure yksityinen peering määritys

Voit päivittää seuraavat cmdlet-komennolla määritysten osa. Seuraavassa esimerkissä virtapiirin VLAN-tunnus päivitetään 100 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Jos haluat poistaa Azure yksityinen peering

Voit poistaa peering kokoonpanosi suorittamalla seuraavat cmdlet-komento.

>[AZURE.WARNING] Varmista, että kaikki virtual verkot ovat linkitys poistetaan ExpressRoute piiri, ennen kuin suoritat cmdlet. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure julkisen peering

Tässä osassa on ohjeita siitä, miten voit luoda, Hae, päivittää ja poistaa Azure julkinen peering määrityskohde ExpressRoute piiri.

### <a name="to-create-azure-public-peering"></a>Voit luoda Azure julkisen peering

1. Tuo PowerShell-moduulin ExpressRoute varten.
    
    Asenna uusimmat PowerShell-installer [PowerShell](http://www.powershellgallery.com/) -valikoimasta ja Azure Resurssienhallinta-moduulit tuominen PowerShell-istunnon jotta voit aloittaa ExpressRoute Cmdlet-komentoja. Tarvitset PowerShell Suorita järjestelmänvalvojana.

        Install-Module AzureRM

        Install-AzureRM

    Tuo kaikki AzureRM.* moduulit semanttinen palvelinversion alueen sisällä

        Import-AzureRM

    Voit tuoda yksinkertaisesti Valitse moduuli semanttinen palvelinversion alueen sisällä 
        
        Import-Module AzureRM.Network 

    Tiliisi kirjautuminen

        Login-AzureRmAccount

    Valitse luotavan ExpressRoute piiri tilaus
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Luo ExpressRoute piiri.
    
    Ohjeiden luominen [ExpressRoute piiri](expressroute-howto-circuit-arm.md) ja sen connectivity tarjoaja valmistelun yhteydessä. 

    Jos yhteys-palveluntarjoajan tarjoaa hallittuja Layer 3-palveluja, voit pyytää yhteyden palveluntarjoajan käyttöön Azure julkisen peering puolestasi. Tässä tapauksessa sinun ei tarvitse ohjeiden seuraavien osien luettelossa. Jos yhteys-palveluntarjoajan Hallitse reititys, että piiri luomisen jälkeen, noudata seuraavia ohjeita.

3. Tarkista ExpressRoute piiri, jotta se on valmistelun yhteydessä.

    Kuittaa ensin ExpressRoute piiri on valmisteltu ja myös käytössä. Katso alla oleva esimerkki.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Vastaus on suurin piirtein seuraavanlaisen alla olevassa esimerkissä:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Määritä Azure julkisen peering virtapiirin varten.

    Varmista, että sinulla on seuraavat tiedot, ennen kuin jatkat edelleen.

    - /30 aliverkon ensisijainen linkin. Tämä on oltava kelvollinen julkisen IPv4-etuliite.
    - /30 aliverkon toissijainen linkin. Tämä on oltava kelvollinen julkisen IPv4-etuliite.
    - Kelvollinen VLAN-tunnus muodostaa peering käyttöön. Varmista, että ei ole muita peering piirissä käyttää samaa VLAN ID-tunnuksellasi.
    - Numero peering nimellä. Voit käyttää 2-tavuinen ja 4-tavuinen LUKUINA.
    - MD5 hash, jos haluat käyttää. **Tämä on valinnainen**.
    
    Voit suorittaa seuraavan cmdlet-komennon Azure julkinen peering, että piiri määrittäminen

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Voit käyttää cmdlet-komennon alla, jos haluat käyttää MD5 hash

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Varmista, että määrität liitetään numero peering ASN ja asiakkaan ASN.

### <a name="to-view-azure-public-peering-details"></a>Voit tarkastella Azure julkisen peering tietoja

Saa tiedot seuraavan cmdlet-komennon avulla

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Päivitä Azure julkisen peering määritys

Voit päivittää seuraavat cmdlet-komennolla määritysten osa

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Virtapiirin VLAN-tunnus päivitetään kuin 200 600 edellä olevassa esimerkissä.

### <a name="to-delete-azure-public-peering"></a>Jos haluat poistaa Azure julkisen peering

Voit poistaa peering kokoonpanosi suorittamalla seuraavat cmdlet-komento

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft peering

Tässä osassa on ohjeita siitä, miten voit luoda, Hae, päivittää ja poistaa Microsoft peering määrityskohde ExpressRoute piiri. 

### <a name="to-create-microsoft-peering"></a>Voit luoda Microsoft peering

1. Tuo PowerShell-moduulin ExpressRoute varten.
    
    Asenna uusimmat PowerShell-installer [PowerShell](http://www.powershellgallery.com/) -valikoimasta ja Azure Resurssienhallinta-moduulit tuominen PowerShell-istunnon jotta voit aloittaa ExpressRoute Cmdlet-komentoja. Tarvitset PowerShell Suorita järjestelmänvalvojana.

        Install-Module AzureRM

        Install-AzureRM

    Tuo kaikki AzureRM.* moduulit semanttinen palvelinversion alueen sisällä

        Import-AzureRM

    Voit tuoda yksinkertaisesti Valitse moduuli semanttinen palvelinversion alueen sisällä 
        
        Import-Module AzureRM.Network 

    Tiliisi kirjautuminen

        Login-AzureRmAccount

    Valitse luotavan ExpressRoute piiri tilaus
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Luo ExpressRoute piiri.
    
    Ohjeiden luominen [ExpressRoute piiri](expressroute-howto-circuit-arm.md) ja sen connectivity tarjoaja valmistelun yhteydessä. 

    Jos yhteys-palveluntarjoajan tarjoaa hallittuja Layer 3-palveluja, voit pyytää yhteyden palveluntarjoajan käyttöön Azure yksityinen peering puolestasi. Tässä tapauksessa sinun ei tarvitse ohjeiden seuraavien osien luettelossa. Jos yhteys-palveluntarjoajan Hallitse reititys, että piiri luomisen jälkeen, noudata seuraavia ohjeita.

3. Tarkista ExpressRoute piiri, jotta se on valmistelun yhteydessä.

    Kuittaa ensin ExpressRoute piiri on valmisteltu ja myös käytössä. Katso alla oleva esimerkki.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Vastaus on suurin piirtein seuraavanlaisen alla olevassa esimerkissä:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Määritä Microsoft peering virtapiirin varten.

    Varmista, että sinulla on seuraavat tiedot, ennen kuin jatkat.

    - /30 aliverkon ensisijainen linkin. Tämä on oltava kelvollinen julkisen IPv4 etuliite omistamasi ja rekisteröity RIR / IRR.
    - /30 aliverkon toissijainen linkin. Tämä on oltava kelvollinen julkisen IPv4 etuliite omistamasi ja rekisteröity RIR / IRR.
    - Kelvollinen VLAN-tunnus muodostaa peering käyttöön. Varmista, että ei ole muita peering piirissä käyttää samaa VLAN ID-tunnuksellasi.
    - Numero peering nimellä. Voit käyttää 2-tavuinen ja 4-tavuinen LUKUINA.
    - Määrittämiisi etuliitteiden: sinun on määritettävä luettelon kaikki etuliitteiden aiot mainostaa erityisen istunnon päälle. Vain julkiseen IP address etuliitteiden hyväksytään. Voit lähettää pilkulla erotettuna luettelona, jos aiot lähettää etuliitteiden joukko. Seuraavia etuliitteitä on oltava rekisteröity sinulle RIR / IRR.
    - Asiakkaan ASN: Jos olet mainonta etuliitteitä ole rekisteröity peering MÄÄRÄKSI, voit määrittää liitetään numero, johon ne on rekisteröity. **Tämä on valinnainen**.
    - Reititys rekisterinimi: Voit määrittää RIR / IRR, jota vastaan, koska numero ja etuliitteiden kirjainkoko on rekisteröity.
    - MD5 hash, jos haluat käyttää. **Tämä on valinnainen.**
    
    Voit suorittaa seuraavat cmdlet-komennolla voit määrittää, että piiri peering Microsoft

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Microsoft peering tietoja

Saat tiedot seuraavan cmdlet-komennon avulla.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Jos haluat päivittää Microsoft peering määritys

Voit päivittää seuraavat cmdlet-komennolla määritysten osa.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>Voit poistaa Microsoft peering

Voit poistaa peering kokoonpanosi suorittamalla seuraavat cmdlet-komento.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraavaksi, [linkki, ExpressRoute piiri VNet](expressroute-howto-linkvnet-arm.md).

-  Saat lisätietoja ExpressRoute työnkulut [ExpressRoute työnkulkuja](expressroute-workflows.md).

-  Saat lisätietoja peering piiri [ExpressRoute piirit ja reititys toimialueet](expressroute-circuit-peerings.md).

-  Lisätietoja virtual verkkojen käyttämisestä on artikkelissa [Virtual verkon yleiskatsaus](../virtual-network/virtual-networks-overview.md).

