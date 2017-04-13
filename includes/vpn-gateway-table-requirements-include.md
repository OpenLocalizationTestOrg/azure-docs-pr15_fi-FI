Seuraavassa taulukossa on lueteltu PolicyBased ja RouteBased VPN yhdyskäytävien koskevat vaatimukset. Tässä taulukossa koskee Resurssienhallinta ja perinteinen käyttöönoton mallit. Perinteinen mallin PolicyBased VPN yhdyskäytävät ovat samat kuin staattinen yhdyskäytävien ja reititys-pohjainen yhdyskäytävät ovat samat kuin dynaaminen yhdyskäytävät.


|   | **PolicyBased Basic VPN-yhdyskäytävän** | **RouteBased Basic VPN-yhdyskäytävän** | **RouteBased Standard VPN-yhdyskäytävän**   | **RouteBased suuri suorituskyvyn VPN-yhdyskäytävän** |
|---|---------------------------------------|---------------------------------------|----------------------------|----------------------------------|
|    **Sivuston sivuston connectivity (S2S)**  | PolicyBased VPN-määritys        | RouteBased VPN-määritys  | RouteBased VPN-määritys     | RouteBased VPN-määritys    |
| **Pisteen sivuston connectivity (P2S**)      | Ei tueta   | Tuettu (voivat olla S2S)  | Tuettu (voivat olla S2S)  | Tuettu (voivat olla S2S) |
| **Todentamismenetelmä**                 |    Ennalta jaettu avain  | Ennalta jaettu avain S2S connectivity P2S connectivity varmenteet | Ennalta jaettu avain S2S connectivity P2S connectivity varmenteet | Ennalta jaettu avain S2S connectivity P2S connectivity varmenteet |
| **S2S yhteyksien enimmäismäärä**       | 1                              | 10                                                                    | 10                                | 30                               |
| **P2S yhteyksien enimmäismäärä**       | Ei tueta                  | 128                                                                   | 128                               | 128                              |
|**Aktiivinen reititys-tuki (erityisen)**           | Ei tueta                  | Ei tueta                                                         | Tuettu                     | Tuettu                   |
 
