Azure kuormituksen on kerros-4 (TCP, UDP) kuormituksen. Kuormituksen tarjoaa suuren käytettävyyden jakamalla saapuvan liikenteen terveitä palveluesiintymiä cloud Services tai virtuaalisten laitteiden kuormituksen tasaustoiminto joukon kesken. Azure kuormituksen voidaan esittää myös kyseisten palveluiden useita portteja tai useita IP-osoitteita.

Voit määrittää kuormituksen, voit:

* Lataa virtuaalikoneet (VMs) saldo saapuvan Internet-tietoliikenteen. Viitataan tässä tilanteessa Kuormituksen tasauspalvelin [kuormituksen tasaus Internet-käyttöön](../articles/load-balancer/load-balancer-internet-overview.md).
* Kuorman tasapaino liikenteen välillä VMs-virtual network (VNet), VMs cloud Services välillä tai yrityksen omissa tiloissa ja verkossa rajat tiloissa, virtual VMs välillä. Viitataan tässä tilanteessa Kuormituksen tasauspalvelin [sisäisen kuormituksen (ILB)](../articles/load-balancer/load-balancer-internal-overview.md).
* Ulkoisen liikenteen eteenpäin tietyn VM-esiintymään.
