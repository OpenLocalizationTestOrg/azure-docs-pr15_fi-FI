<properties
   pageTitle="Reititys ExpressRoute-piiri PowerShellin perinteinen käyttöönoton mallin määrittämisestä | Microsoft Azure"
   description="Tässä artikkelissa käydään läpi vaiheet yksityinen, yleisön ja Microsoft peering ExpressRoute piiri, luominen ja. Tässä artikkelissa myös kerrotaan, miten voit tarkistaa tilan, Päivitä tai poista oman piiri peerings."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Luoda ja muokata reititys ExpressRoute piiri

> [AZURE.SELECTOR]
[Azure Portal - Resurssienhallinta](expressroute-howto-routing-portal-resource-manager.md)
[PowerShell - Resurssienhallinta](expressroute-howto-routing-arm.md)
[PowerShell – perinteinen](expressroute-howto-routing-classic.md)



Tässä artikkelissa käydään läpi vaiheet ja luoda ja hallita PowerShell ja perinteinen käyttöönoton mallin ExpressRoute-piiri reititys määritykset. Seuraavien vaiheiden kuvissa olevia myös noudattamalla deprovision ExpressRoute piiri peerings ja Tarkista tila, Päivitä tai poista.


**Tietoja malleista Azure käyttöönotto**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Määritysten edellytykset

- Sinun on PowerShellin Azure-moduulit uusimman version. Voit ladata uusimman PowerShell-moduulin PowerShell-osasta [Azure-lataussivulta](https://azure.microsoft.com/downloads/). Vaiheittaiset ohjeet [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md) -sivulla ohjeiden tietokoneen käyttämään PowerShellin Azure-moduulit määrittämisestä. 
- Varmista, että olet tarkistanut [edellytykset](expressroute-prerequisites.md) -sivulla, [vaatimukset reititys](expressroute-routing.md) -sivulle ja [Työnkulut](expressroute-workflows.md) -sivu ennen kuin aloitat määritys.
- Sinulla on oltava aktiivinen ExpressRoute-piiri. Voit [luoda ExpressRoute piiri](expressroute-howto-circuit-classic.md) ohjeiden ja olet ottanut connectivity-palveluntarjoajan, ennen kuin jatkat piiri. ExpressRoute piiri on oltava valmisteltu ja käytössä-tilaan, voivat suorittaa jäljempänä Cmdlet-komentoja.

>[AZURE.IMPORTANT] Nämä ohjeet koskevat vain piirit luotu ojentamassa Layer 2 yritystietojen yhdistämispalvelut palveluntarjoajia. Jos käytössäsi on palveluntarjoajan tarjoaminen hallittuja Layer 3-palveluja (yleensä IPVPN, kuten MPLS), yhteys-palveluntarjoajan määrittäminen ja hallinta reititys puolestasi.

Voit määrittää yhden, kahden tai kaikki kolme peerings (Azure yksityiset, Azure julkinen ja Microsoft), ExpressRoute piiri. Voit määrittää peerings valitset järjestyksessä. Kuitenkin varmistamalla, että suoritat kunkin peering yksi kerrallaan määritys. 

## <a name="azure-private-peering"></a>Azure yksityinen peering

Tässä osassa on ohjeita siitä, miten voit luoda, Hae, päivittäminen ja poistaminen ExpressRoute piiri Azure yksityinen peering määrityskohde. 

### <a name="to-create-azure-private-peering"></a>Voit luoda Azure yksityinen peering

1. **Tuo PowerShell-moduulin ExpressRoute varten.**
    
    Azure ja ExpressRoute moduulit on tuominen PowerShell-istunnon jotta voit aloittaa ExpressRoute Cmdlet-komentoja. Suorita seuraavat komennot Azure ja ExpressRoute moduulit tuominen PowerShell-istunto.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Luo ExpressRoute piiri.**
    
    Ohjeiden luominen [ExpressRoute piiri](expressroute-howto-circuit-classic.md) ja sen connectivity tarjoaja valmistelun yhteydessä. Jos yhteys-palveluntarjoajan tarjoaa hallittuja Layer 3-palveluja, voit pyytää yhteyden palveluntarjoajan käyttöön Azure yksityinen peering puolestasi. Tässä tapauksessa sinun ei tarvitse ohjeiden seuraavien osien luettelossa. Jos yhteys-palveluntarjoajan Hallitse reititys, että piiri luomisen jälkeen, noudata seuraavia ohjeita. 

3. **Tarkista ExpressRoute-piiri, jotta se on valmisteltu.**

    Kuittaa ensin ExpressRoute piiri on valmisteltu ja myös käytössä. Katso alla oleva esimerkki.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Varmista, että virtapiirin näkyy Provisioned ja käytössä. Jos se ei toimi connectivity tarjoajalta saat oman piiri tarvittavat tilaan ja tila.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Määritä Azure yksityinen peering virtapiirin varten.**

    Varmista, että sinulla on seuraavat seikat, ennen kuin jatkat seuraavat vaiheet:

    - /30 aliverkon ensisijainen linkin. Tämä ei saa olla varattu virtual verkoissa minkä tahansa osoitetilan osa.
    - /30 aliverkon toissijainen linkin. Tämä ei saa olla varattu virtual verkoissa minkä tahansa osoitetilan osa.
    - Kelvollinen VLAN-tunnus muodostaa peering käyttöön. Varmista, että ei ole muita peering piirissä käyttää samaa VLAN ID-tunnuksellasi.
    - Numero peering nimellä. Voit käyttää 2-tavuinen ja 4-tavuinen LUKUINA. Voit määrittää yksityisen kuin tässä peering numero. Varmista, että käytössäsi ei ole 65515.
    - MD5 hash, jos haluat käyttää. **Tämä on valinnainen**.
    
    Voit suorittaa seuraavan cmdlet-komennon määrittäminen Azure yksityinen peering oman piiri varten.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Voit käyttää cmdlet-komennon alla, jos haluat käyttää MD5 hash.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Varmista, että määrität peering ASN asiakkaan ASN liitetään numero.

### <a name="to-view-azure-private-peering-details"></a>Voit tarkastella Azure yksityinen peering tietoja

Saa tiedot seuraavan cmdlet-komennon avulla

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Päivitä Azure yksityinen peering määritys

Voit päivittää seuraavat cmdlet-komennolla määritysten osa. Seuraavassa esimerkissä virtapiirin VLAN-tunnus päivitetään 100 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Jos haluat poistaa Azure yksityinen peering

Voit poistaa peering kokoonpanosi suorittamalla seuraavat cmdlet-komento.

>[AZURE.WARNING] Varmista, että kaikki virtual verkot ovat linkitys poistetaan ExpressRoute piiri, ennen kuin suoritat cmdlet. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure julkisen peering

Tässä osassa on ohjeita siitä, miten voit luoda, Hae, päivittää ja poistaa Azure julkisen peering määrityskohde ExpressRoute piiri.

### <a name="to-create-azure-public-peering"></a>Voit luoda Azure julkisen peering

1. **Tuo PowerShell-moduulin ExpressRoute varten.**
    
    Azure ja ExpressRoute moduuleissa on tuominen PowerShell-istunnon jotta voit aloittaa ExpressRoute Cmdlet-komentoja. Suorita seuraavat komennot Azure ja ExpressRoute moduulit tuominen PowerShell-istunto. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Luo ExpressRoute piiri**
    
    Ohjeiden luominen [ExpressRoute piiri](expressroute-howto-circuit-classic.md) ja sen connectivity tarjoaja valmistelun yhteydessä. Jos yhteys-palveluntarjoajan tarjoaa hallittuja Layer 3-palveluja, voit pyytää yhteyden palveluntarjoajan käyttöön Azure julkisen peering puolestasi. Tässä tapauksessa sinun ei tarvitse ohjeiden seuraavien osien luettelossa. Jos yhteys-palveluntarjoajan Hallitse reititys, että piiri luomisen jälkeen, noudata seuraavia ohjeita.

3. **Tarkista ExpressRoute piiri, jotta se on valmisteltu**

    Kuittaa ensin ExpressRoute piiri on valmisteltu ja myös käytössä. Katso alla oleva esimerkki.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Varmista, että virtapiirin näkyy Provisioned ja käytössä. Jos se ei toimi connectivity tarjoajalta saat oman piiri tarvittavat tilaan ja tila.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Määritä Azure julkisen peering virtapiirin varten**

    Varmista, että sinulla on seuraavat tiedot, ennen kuin jatkat edelleen.

    - /30 aliverkon ensisijainen linkin. Tämä on oltava kelvollinen julkisen IPv4-etuliite.
    - /30 aliverkon toissijainen linkin. Tämä on oltava kelvollinen julkisen IPv4-etuliite.
    - Kelvollinen VLAN-tunnus muodostaa peering käyttöön. Varmista, että ei ole muita peering piirissä käyttää samaa VLAN ID-tunnuksellasi.
    - Numero peering nimellä. Voit käyttää 2-tavuinen ja 4-tavuinen LUKUINA.
    - MD5 hash, jos haluat käyttää. **Tämä on valinnainen**.
    
    Voit suorittaa seuraavan cmdlet-komennon Azure julkisen peering, että piiri määrittäminen

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Voit käyttää cmdlet-komennon alla, jos haluat käyttää MD5 hash

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Varmista, että määrität liitetään numero peering ASN ja asiakkaan ASN.

### <a name="to-view-azure-public-peering-details"></a>Voit tarkastella Azure julkisen peering tietoja

Saa tiedot seuraavan cmdlet-komennon avulla

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Päivitä Azure julkisen peering määritys

Voit päivittää seuraavat cmdlet-komennolla määritys osa

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

Virtapiirin VLAN-tunnus päivitetään kuin 200 600 edellä olevassa esimerkissä.

### <a name="to-delete-azure-public-peering"></a>Jos haluat poistaa Azure julkisen peering

Voit poistaa peering kokoonpanosi suorittamalla seuraavat cmdlet-komento

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft peering

Tässä osassa on ohjeita siitä, miten voit luoda, Hae, päivittää ja poistaa Microsoft peering määrityskohde ExpressRoute piiri. 

### <a name="to-create-microsoft-peering"></a>Voit luoda Microsoft peering

1. **Tuo PowerShell-moduulin ExpressRoute varten.**
    
    Azure ja ExpressRoute moduulit on tuominen PowerShell-istunnon jotta voit aloittaa ExpressRoute Cmdlet-komentoja. Suorita seuraavat komennot Azure ja ExpressRoute moduulit tuominen PowerShell-istunto.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Luo ExpressRoute piiri**
    
    Ohjeiden luominen [ExpressRoute piiri](expressroute-howto-circuit-classic.md) ja sen connectivity tarjoaja valmistelun yhteydessä. Jos yhteys-palveluntarjoajan tarjoaa hallittuja Layer 3-palveluja, voit pyytää yhteyden palveluntarjoajan käyttöön Azure yksityinen peering puolestasi. Tässä tapauksessa sinun ei tarvitse ohjeiden seuraavien osien luettelossa. Jos yhteys-palveluntarjoajan Hallitse reititys, että piiri luomisen jälkeen, noudata seuraavia ohjeita.

3. **Tarkista ExpressRoute piiri, jotta se on valmisteltu**

    On ensin Tarkista, onko ExpressRoute virtapiirin Provisioned ja käytössä-tilassa.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Varmista, että virtapiirin näkyy Provisioned ja käytössä. Jos se ei toimi connectivity tarjoajalta saat oman piiri tarvittavat tilaan ja tila.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Määritä Microsoft peering virtapiirin varten**

    Varmista, että sinulla on seuraavat tiedot, ennen kuin jatkat.

    - /30 aliverkon ensisijainen linkin. Tämä on oltava kelvollinen julkisen IPv4 etuliite omistamasi ja rekisteröity RIR / IRR.
    - /30 aliverkon toissijainen linkin. Tämä on oltava kelvollinen julkisen IPv4 etuliite omistamasi ja rekisteröity RIR / IRR.
    - Kelvollinen VLAN-tunnus muodostaa peering käyttöön. Varmista, että ei ole muita peering piirissä käyttää samaa VLAN ID-tunnuksellasi.
    - Numero peering nimellä. Voit käyttää 2-tavuinen ja 4-tavuinen LUKUINA.
    - Määrittämiisi etuliitteiden: sinun on määritettävä luettelon kaikki etuliitteiden aiot mainostaa erityisen istunnon päälle. Vain julkiseen IP address etuliitteiden hyväksytään. Voit lähettää pilkulla erotettuna luettelona, jos aiot lähettää etuliitteiden joukko. Seuraavia etuliitteitä on oltava rekisteröity sinulle RIR / IRR.
    - Asiakkaan ASN: Jos olet mainonta etuliitteitä ole rekisteröity peering MÄÄRÄKSI, voit määrittää liitetään numero, johon ne on rekisteröity. **Tämä on valinnainen**.
    - Reititys rekisterinimi: Voit määrittää RIR / IRR, jota vastaan, koska numero ja etuliitteiden kirjainkoko on rekisteröity.
    - MD5 hajautuksen Jos haluat käyttää. **Tämä on valinnainen.**
    
    Voit suorittaa seuraavat cmdlet-komennolla voit määrittää, että piiri pering Microsoft

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Jos haluat tarkastella Microsoft peering

Saat tiedot seuraavan cmdlet-komennon avulla.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Jos haluat päivittää Microsoft peering määritys

Voit päivittää seuraavat cmdlet-komennolla määritysten osa.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Voit poistaa Microsoft peering

Voit poistaa peering kokoonpanosi suorittamalla seuraavat cmdlet-komento.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Seuraavat vaiheet

Seuraava [linkki VNet ExpressRoute piiri avulla](expressroute-howto-linkvnet-classic.md).


-  Saat lisätietoja työnkulkujen [ExpressRoute työnkulkuja](expressroute-workflows.md).
-  Saat lisätietoja peering piiri [ExpressRoute piirit ja reititys toimialueet](expressroute-circuit-peerings.md).

