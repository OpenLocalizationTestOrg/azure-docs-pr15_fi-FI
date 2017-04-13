<properties
   pageTitle="ExpressRoute asiakkaan reitittimen määritys näytteiden | Microsoft Azure"
   description="Tällä sivulla on reitittimen määritys mallit-Cisco ja Juniper reitittimen."
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

# <a name="router-configuration-samples-to-setup-and-manage-nat"></a>Määritä ja hallitse NAT reitittimen määritys näytteiden

Tällä sivulla on NAT määritysten mallit-Cisco ASA ja Juniper SRX sarjan reitittimen. Nämä on tarkoitettu vain ohjeita näytteiden ja ei saa käyttää sellaisenaan. Voit käsitellä verkkoa varten tarvittavat määritykset tulee toimittaja. 

>[AZURE.IMPORTANT] Esimerkkejä, joiden tällä sivulla on tarkoitettu laskettuna ohjeita. Käsitellä toimittajan myynnin / tekniset ryhmän ja tarvittavat määritykset vastaamaan omia tarpeita tulee verkko ryhmän. Microsoft ei tue tällä sivulla luetellut käyttömahdollisuudet liittyvät ongelmat. Pyydä laitteen valmistajalta tukeen.

Azure julkinen ja Microsoftin peerings koskevat reitittimen määritys näytteiden alla. Sinun ei on määritettävä NAT Azure yksityinen peering. Tutustu [ExpressRoute peerings](expressroute-circuit-peerings.md) ja [ExpressRoute NAT vaatimukset](expressroute-nat.md) enemmän tietoja.

**Huomautus:** Tarvitset erillisen NAT IP-jakavat yhteys internet- ja ExpressRoute. Internet- ja ExpressRoute saman NAT IP-ryhmän käyttö aiheuttaa julkiseen reititys ja menettäminen.

## <a name="cisco-asa-firewalls"></a>Cisco ASA palomuurit

### <a name="pat-configuration-for-traffic-from-customer-network-to-microsoft"></a>Tietoliikenteen asiakasverkoston Microsoftille PAT määritys

    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>
    
    
    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>
    
    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>
    
    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2
    
    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-to-customer-network"></a>Microsoftin tietoliikenteen asiakasverkoston PAT määritys

#### <a name="interfaces-and-direction"></a>Liityntäkohdat ja suunta:
    Source Interface (where the traffic enters the ASA): inside
    Destination Interface (where the traffic exits the ASA): outside

#### <a name="configuration"></a>Määritys:
NAT resurssivarantoon:

    object network outbound-PAT
        host <NAT-IP>

Kohdepalvelin:

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

Asiakkaan IP-osoitteiden objektiryhmä

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>
    
    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

NAT-komennot:

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a>Juniper SRX sarjan reitittimen 

### <a name="1-create-redundant-ethernet-interfaces-for-the-cluster"></a>1. tarpeettomat Ethernet liittymät klusterin luominen

    interfaces {
        reth0 {
            description "To Internal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "To Microsoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "To Microsoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a>2. Luo kaksi Suojausvyöhykkeet

 - Salli alueen sisäisen verkon ja ulkoisen verkoston vastakkaisten reunan reitittimen Untrust-vyöhyke
 - Määritä haluamasi liittymät vyöhykkeet
 - Salli liittymät palvelut


    {vyöhykkeet {vyöhykkeellä luota {host-saapuva-liikenne {järjestelmän-palveluiden {ping; suojaus                  } {erityisen; protokollat                  liityntäkohdat}} {reth0.100;              }} vyöhykkeellä Untrust {host-saapuva-liikenne {järjestelmän-palveluiden {ping;                  } {erityisen; protokollat                  liityntäkohdat}} {reth1.100;              }          }      }  }


### <a name="3-create-security-policies-between-zones"></a>3. luoda suojauskäytäntöjä alueiden välillä
 
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a>4. NAT käytäntöjen määrittäminen
 - Luo kaksi NAT jakavat. Yksi käytetään NAT-tietoliikenteen lähtevien Microsoftin ja muiden Microsoftin asiakkaalle.
 - Vastaaviin liikenne NAT sääntöjen luominen

        security {
            nat {
                source {
                    pool SNAT-To-ExpressRoute {
                        routing-instance {
                            External-ExpressRoute;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    pool SNAT-From-ExpressRoute {
                        routing-instance {
                            Internal;
                        }
                        address {
                            <NAT-IP-address/Subnet-mask>;
                        }
                    }
                    rule-set Outbound_NAT {
                        from routing-instance Internal;
                        to routing-instance External-ExpressRoute;
                        rule SNAT-Out {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-To-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                    rule-set Inbound-NAT {
                        from routing-instance External-ExpressRoute;
                        to routing-instance Internal;
                        rule SNAT-In {
                            match {
                                source-address 0.0.0.0/0;
                            }
                            then {
                                source-nat {
                                    pool {
                                        SNAT-From-ExpressRoute;
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }


### <a name="5-configure-bgp-to-advertise-selective-prefixes-in-each-direction"></a>5. määrittäminen erityisen mainostaa Valikoiva etuliitteiden kumpaankin suuntaan

Lisätietoja näytteiden [Reititys määritysten mallit](expressroute-config-samples-routing.md) -sivulla.

### <a name="6-create-policies"></a>6. käytäntöjen luominen

    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Katso lisätietoja [ExpressRoute usein kysytyt kysymykset](expressroute-faqs.md) .
