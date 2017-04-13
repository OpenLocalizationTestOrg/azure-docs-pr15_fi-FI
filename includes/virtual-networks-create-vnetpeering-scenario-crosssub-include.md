## <a name="peering-across-subscriptions"></a>Peering-tilauksissa

Tässä skenaariossa luodaan peering kaksi VNets kuuluvat eri tilausten välillä.

![toimintojen välinen sub-skenaario](./media/virtual-networks-create-vnetpeering-scenario-crosssub-include/figure01.PNG)

Roolipohjainen käyttöoikeuksien valvonta (RBAC) lupaa VNet peering riippuvainen. Rajat tilaukset skenaarion sinun on oikeutesi myöntäminen käyttäjille, jotka luodaan peering linkin:

> [AZURE.NOTE] Jos sama käyttäjä on oikeus kautta sekä tilaukset, voit ohittaa vaiheet 1 – 4 alla.
