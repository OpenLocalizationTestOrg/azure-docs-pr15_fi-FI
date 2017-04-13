|                              | **Pisteen sivuston**                                                                            | **Sivusto sivusto**                                                                                        | **ExpressRoute**                                                                                                                     |
|------------------------------|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------|
| **Azure Tuetut palvelut** | Pilvipalveluiden ja näennäiskoneiden                                                          | Pilvipalveluiden ja näennäiskoneiden                                                                     | [Services-luettelosta](../expressroute/expressroute-faqs.md#supported-services)                                                       |
| **Tyypillinen kaistanleveyksien**       | Yleensä < 100 Mbps kooste                                                               | Yleensä < 100 Mbps kooste                                                                          | 50 Mbps, 100 Mbps, 200 Mbps, 500 Mbps, 1 Gbps, 2 Gbps, 5 Gbps, 10 Gbps                                                               |
| **Tuetut protokollat**      | SSL tunnelointiprotokolla (SSTP)                                                     | IP                                                | Suora yhteys näennäislähiverkkojen NSP's VPN tekniikoita (MPLS, VPLS,...)                                                                                                    |
| **Reititys**                  | RouteBased (dynaaminen)                                                                        | Sovellus tukee PolicyBased (staattinen reititys) ja RouteBased (dynaaminen reititys VPN)                 | ERITYISEN                                                                                                                                  |
| **Yhteyden vikasietoisuudelle**    | Aktiivinen-passiivinen                                                                               | Aktiivinen-passiivinen                                                                                          | aktiivinen aktiivinen                                                                                                                        |
| **Tyypillinen Käyttötapaus**         | Prototyyppiä, keskihajonta / testata / kurssin tilanteita, joissa pilvipalveluiden ja näennäiskoneiden              | Keskihajonta / testata / kurssin skenaariot ja pienen skaalata tuotannon työmääriä pilvipalveluiden ja näennäiskoneiden | Kaikki Azure services (vahvistettu luettelo)-yritysluokan ja tehtävä kriittinen toiminnoista, varmuuskopiointi, Big datasta ja Azure DR sivustojen käytön |
| **SLA**                      | [SLA](https://azure.microsoft.com/support/legal/sla/)                                        | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                   | [SLA](https://azure.microsoft.com/support/legal/sla/)                                                                                |
| **Hinnat**                  | [Hinnat](https://azure.microsoft.com/pricing/details/vpn-gateway/)                           | [Hinnat](https://azure.microsoft.com/pricing/details/vpn-gateway/)                                      | [Hinnat](https://azure.microsoft.com/pricing/details/expressroute/)                                                                   |
| **Tekniset**  | [VPN yhdyskäytävän dokumentaatio](https://azure.microsoft.com/documentation/services/vpn-gateway/) | [VPN yhdyskäytävän dokumentaatio](https://azure.microsoft.com/documentation/services/vpn-gateway/)            | [ExpressRoute dokumentaatio](https://azure.microsoft.com/documentation/services/expressroute/)                                        |
| **USEIN KYSYTYT KYSYMYKSET**                     | [VPN-yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md)                                                    | [VPN-yhdyskäytävän usein kysytyt kysymykset](vpn-gateway-vpn-faq.md)                                                               | [ExpressRoute usein kysytyt kysymykset](../expressroute/expressroute-faqs.md)                                                                             |