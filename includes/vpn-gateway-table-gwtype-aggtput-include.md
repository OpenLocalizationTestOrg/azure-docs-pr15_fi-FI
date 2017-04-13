|    | **VPN-yhdyskäytävän siirtonopeuden (1)** | **VPN-yhdyskäytävän max IP tunneloi (2)** | **ExpressRoute yhdyskäytävän siirtonopeuden** | **VPN-yhdyskäytävän ja ExpressRoute olla**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Tavallinen SKU (3)(5)**              |  100 Mbps | 10                         |  500 Mbps                           | Ei   |
| **Vakio SKU (4)(5)**           |  100 Mbps | 10                         | 1 000 Mbps                           | Kyllä  |
| **Erinomainen suorituskyky SKU (4)**   | 200 Mbps  | 30                         | 2000 Mbps                           | Kyllä  |

- (1) VPN on arvio VNets saman Azure alueen välillä mitat perusteella. Se ei ole taattua siirtonopeuden paikallisen-yhteyksien Internetissä. Se on mahdollista suurin nopeus-mitta.
- (2) tunneleita määrän viitata RouteBased VPN-yhteydet. PolicyBased näennäinen Yksityisverkko tukee vain yhden sivuston sivuston VPN-tunnelin.
- (3) tavallinen tuote ei tue erityisen.
- (4) Tämä tuote ei tue PolicyBased VPN-yhteydet. Ne tuetaan vain Basic tuote.
- (5) Tämä tuote ei tue aktiivinen-aktiivinen S2S VPN-yhdyskäytävän yhteydet. Aktiivinen aktiivinen tuetaan vain korkean tuote.