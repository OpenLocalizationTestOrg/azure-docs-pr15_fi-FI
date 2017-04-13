<properties
   pageTitle="ExpressRoute asiakkaan reitittimen määritys näytteiden | Microsoft Azure"
   description="Tällä sivulla on reitittimen config mallit-sovellukseen Cisco ja Juniper reitittimen."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>

# <a name="router-configuration-samples-to-setup-and-manage-routing"></a>Määritä ja hallitse reititys reitittimen määritys näytteiden

Tällä sivulla on käyttöliittymässä ja reititys määritysten näytteiden Cisco IOS-XE ja Juniper MX sarjan reitittimen. Nämä on tarkoitettu vain ohjeita näytteiden ja ei saa käyttää sellaisenaan. Voit käsitellä verkkoa varten tarvittavat määritykset tulee toimittaja. 

>[AZURE.IMPORTANT] Esimerkkejä, joiden tällä sivulla on tarkoitettu laskettuna ohjeita. Käsitellä toimittajan myynnin / tekniset ryhmän ja tarvittavat määritykset vastaamaan omia tarpeita tulee verkko ryhmän. Microsoft ei tue tällä sivulla luetellut käyttömahdollisuudet liittyvät ongelmat. Pyydä laitteen valmistajalta tukeen.

Reitittimen määritys näytteiden alla koskevat kaikkia peerings. Tarkista [ExpressRoute peerings](expressroute-circuit-peerings.md) ja [ExpressRoute reititys vaatimukset](expressroute-routing.md) lisätietoja reititys.

## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE perusteella reitittimen

Tässä osassa mallit hakea mitä tahansa reitittimen IOS XE OS tuoteperheen käyttävän.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. määrittäminen liittymät ja aliraportti liityntäkohdat

Tarvitset sub-liittymän kohti välein voit muodostaa yhteyden Microsoft reitittimen peering. Sub-liittymän tunnistaa VLAN-tunnus tai pinottu kahdet VLAN tunnukset ja IP-osoite.

#### <a name="dot1q-interface-definition"></a>Dot1Q liitännän määritys

Tässä esimerkissä on osa liitännän määrityksen aliraportti liittymän, jossa on yksi VLAN. VLAN-tunnus on yksilöllinen peering kohden. IPv4-osoitteen viimeinen oktetti on aina on pariton.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

#### <a name="qinq-interface-definition"></a>QinQ liitännän määritys

Tässä esimerkissä on kaksi VLAN tunnukset aliraportti liittymän aliraportti liitännän-määritys. Ulomman VLAN-tunnus (s-tunniste), jos säilyy ennallaan peerings yli. Sisemmän VLAN-tunnus (c-tunniste) on yksilöllinen peering kohden. IPv4-osoitteen viimeinen oktetti on aina on pariton.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>
    
### <a name="2-setting-up-ebgp-sessions"></a>2. eBGP istuntojen määrittäminen

Erityisen istunnon Microsoft jokaisen peering on määritys. Alla malli mahdollistaa asennuksen erityisen istunnon Microsoft. Jos käytit sub-käyttöliittymän IPv4-osoite oli a.b.c.d, erityisen yhteisön (Microsoft) IP-osoite on a.b.c.d+1. Erityisen yhteisön IPv4-osoitteen viimeinen oktetti on aina parillinen.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. määrittäminen etuliitteiden määrittämiisi erityisen istunnon päälle

Voit määrittää, että reitittimessä mainostaa Valitse etuliitteiden Microsoftille. Voit tehdä niin käyttämällä alla malli.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. yhdistää reitin

Voit käyttää reitin karttoja ja etuliite luetteloiden suodattaminen etuliitteiden välitetty mukaan verkostoon. Voit käyttää alla otosten suoritettava tehtävä. Varmista, että sinulla on tarvittavat etuliite luetteloiden asetukset.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Juniper MX sarjan reitittimen 

Tässä osassa mallit hakea mitä tahansa Juniper MX sarjan reitittimen.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. määrittäminen liittymät ja aliraportti liityntäkohdat

#### <a name="dot1q-interface-definition"></a>Dot1Q liitännän määritys

Tässä esimerkissä on osa liitännän määrityksen aliraportti liittymän, jossa on yksi VLAN. VLAN-tunnus on yksilöllinen peering kohden. IPv4-osoitteen viimeinen oktetti on aina on pariton.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


#### <a name="qinq-interface-definition"></a>QinQ liitännän määritys

Tässä esimerkissä on kaksi VLAN tunnukset aliraportti liittymän aliraportti liitännän-määritys. Ulomman VLAN-tunnus (s-tunniste), jos säilyy ennallaan peerings yli. Sisemmän VLAN-tunnus (c-tunniste) on yksilöllinen peering kohden. IPv4-osoitteen viimeinen oktetti on aina on pariton.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. eBGP istuntojen määrittäminen

Erityisen istunnon Microsoft jokaisen peering on määritys. Alla malli mahdollistaa asennuksen erityisen istunnon Microsoft. Jos käytit sub-käyttöliittymän IPv4-osoite oli a.b.c.d, erityisen yhteisön (Microsoft) IP-osoite on a.b.c.d+1. Erityisen yhteisön IPv4-osoitteen viimeinen oktetti on aina parillinen.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a>3. määrittäminen etuliitteiden määrittämiisi erityisen istunnon päälle

Voit määrittää, että reitittimessä mainostaa Valitse etuliitteiden Microsoftille. Voit tehdä niin käyttämällä alla malli.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. yhdistää reitin

Voit käyttää reitin karttoja ja etuliite luetteloiden suodattaminen etuliitteiden välitetty mukaan verkostoon. Voit käyttää alla otosten suoritettava tehtävä. Varmista, että sinulla on tarvittavat etuliite luetteloiden asetukset.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md) .
