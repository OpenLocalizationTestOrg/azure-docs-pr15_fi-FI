<properties
   pageTitle="Valitse näennäiskoneiden klusterit MATLAB | Microsoft Azure"
   description="Microsoft Azuren näennäiskoneiden avulla voit luoda MATLAB Distributed Computing palvelimen klustereiden Suorita oman paljon suorittaminen rinnakkain MATLAB-toiminnoista"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="mscurrell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Windows"
   ms.workload="infrastructure-services"
   ms.date="05/09/2016"
   ms.author="markscu"/>

# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Luo MATLAB Distributed tietojenkäsittely palvelimen klustereiden Azure VMs 

Microsoft Azuren näennäiskoneiden avulla voit luoda yhden tai useamman MATLAB Distributed Computing palvelimen klustereiden Suorita oman paljon suorittaminen rinnakkain MATLAB-toiminnoista. MATLAB Distributed tietojenkäsittely Server-ohjelmiston asentaminen AM perus kuvana ja käyttöön ja hallita klusterin Azure pikaopas mallia tai Azure PowerShell-komentosarjaa (käytettävissä [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) avulla. Käyttöönoton jälkeen muodostaa yhteyttä klusterin Suorita oman toiminnoista. 

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>Tietoja MATLAB ja MATLAB Distributed tietojenkäsittely-palvelin 

[MATLAB](http://www.mathworks.com/products/matlab/) -ympäristö on tarkoitettu suunnitteluryhmät ja tieteellisen ongelmien ratkaisemiseen. Suurissa simulaatioita ja tietojen käsittely-tehtäviä MATLAB-käyttäjät voivat käyttää MathWorksin rinnakkain tietojenkäsittely tuotteiden nopeuttaa niiden Laske paljon työmääriä hyödyntämistä Laske klustereiden ja ruudukko-palvelut. [Rinnakkaisia tietojenkäsittely työkaluryhmän](http://www.mathworks.com/products/parallel-computing/) avulla MATLAB käyttäjät parallelize sovellukset ja hyödyntää core usean suorittimen grafiikkasuorittimet, ja laskea klustereiden. [MATLAB Distributed tietojenkäsittely palvelimen](http://www.mathworks.com/products/distriben/) avulla MATLAB käyttäjä voi käyttää useissa tietokoneissa-laskentaklusteriin. 


Azure-virtuaalikoneissa avulla voit luoda MATLAB Distributed tietojenkäsittely palvelimen klustereiden, joissa on samat järjestelmiä käytettävissä lähettää rinnakkain työtä kuin paikallisen klustereiden, kuten vuorovaikutteinen työt, Ehdota, riippumaton tehtävät ja luovuttavan tehtävät. Azure käyttäminen yhdessä MATLAB-ympäristö on monia etuja verrattuna valmistelu ja käyttämällä perinteinen paikallisen laitteen: virtuaalikoneen solualueen koot, varausyksiköiden tarvittaessa niin maksat vain Laske resurssien voit käyttöä ja voi testata asteikko mallien luominen.  

## <a name="prerequisites"></a>Edellytykset

* **Asiakastietokone** - tarvitset Windows-pohjaisesta asiakastietokone, pitää yhteyttä Azure ja MATLAB Distributed tietojenkäsittely Server-klusterin käyttöönoton jälkeen. 

* **PowerShellin azure** – Katso [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) asiakastietokoneen. 

* **Azure tilauksen** – Jos ei ole tilausta voit luoda [vapaa tilin](https://azure.microsoft.com/free/) muutamaan minuuttiin. Harkitse suurempi klustereiden tukiyhteyksiä tilauksen tai Osta muita asetuksia. 

* **Sydämiä kiintiön** - joudut ehkä ottaa käyttöön suuri klusterin tai useamman kuin yhden MATLAB Distributed tietojenkäsittely Server-klusterin core-kiintiön Suurenna. Kun haluat lisätä kiintiön, [Avaa asiakkaan online-tukipyynnön](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) maksutta. 

* **MATLAB, rinnakkain tietojenkäsittely työkaluryhmä- ja MATLAB Distributed tietojenkäsittely palvelimen käyttöoikeuksia** - komentosarjat oletetaan, että [MathWorksin isännöidään käyttöoikeuksien hallinnassa](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) käytetään kaikki käyttöoikeudet.  

* **MATLAB Distributed tietojenkäsittely Server-ohjelmiston** - asennetaan, jota käytetään perus AM kuvana klusterin VMs AM. 


## <a name="high-level-steps"></a>Korkean tason vaiheet

Jos haluat käyttää Azuren näennäiskoneiden MATLAB Distributed tietojenkäsittely palvelimen klustereiden, tarvitaan korkean tason seuraavasti. Tarkat ohjeet ovat mukana toimitettua pikaopas mallia ja komentosarjojen [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)ohjeissa.

1. **Luo perus AM kuva**  
    * Lataa ja asenna MATLAB Distributed tietojenkäsittely Server-ohjelmiston tämän AM sivulle. 

    >[AZURE.NOTE]Voi kestää muutaman tunnin, mutta sinun on vain se tehdään, kun käytät kunkin MATLAB versio.   
    
2. **Yhden tai useamman klustereiden luominen**  
    * Käytä annettu PowerShell-komentosarjaa tai pikaopas-mallin avulla voit luoda klusterin AM kuvaa.   
    * Hallitse käyttämällä annettua PowerShell-komentosarja, joka sallii luettelon, keskeyttää, jatkaa klustereiden ja poista klustereiden. 
 
## <a name="cluster-configurations"></a>Klusterimääritykset 

Tällä hetkellä klusterin luonnin komentosarjaa ja mallia, joiden avulla voit luoda yksittäisen MATLAB Distributed tietojenkäsittely palvelimen topologian. Voit halutessasi luoda yhden tai useamman muita klustereiden kunkin klusterin on eri määrä työntekijä VMs, käyttämällä AM erikokoisia ja niin edelleen. 

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB asiakkaan ja klusterin Azure-tietokannassa 

MATLAB asiakkaan solmu, MATLAB työn ajoitus-solmu ja MATLAB Distributed tietojenkäsittely palvelimen "työntekijä" solmujen on kaikki määritetty Azure VMs virtual verkossa seuraavassa kuvassa esitetyllä tavalla. 

![Klusterin topologian](./media/virtual-machines-windows-matlab-mdcs-cluster/mdcs_cluster.png)

* Klusterin käyttämään yhdistäminen asiakkaan solmun etätyöpöydän kautta. Asiakassolmu suorittaa MATLAB asiakas. 

* Asiakassolmu on jaetun tiedoston, jotka voivat käyttää kaikki työntekijät.

* MathWorksin isännöidään käyttöoikeuksien hallinnassa käytetään kaikkien MATLAB-ohjelmiston käyttöoikeussopimuksen etsii. 

* Oletusarvon mukaan yksi MATLAB Distributed tietojenkäsittely palvelimen työntekijä core kohti luodaan työntekijä VMs, mutta voit määrittää jokin muu luku. 


## <a name="use-an-azure-based-cluster"></a>Käytä Azure-pohjaiset klusterin 

Kuin muuntyyppiset MATLAB Distributed tietojenkäsittely palvelimen klustereiden, sinun on käytettävä klusterin profiilin hallinta (työasemassa AM) MATLAB-asiakasohjelmassa MATLAB aikataulutus klusterin profiilin luominen.

![Klusterin profiilin hallinta](./media/virtual-machines-windows-matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Seuraavat vaiheet

* Saat lisätietoja ottaa käyttöön ja hallita MATLAB Distributed tietojenkäsittely Server Azure varausyksiköt sisältävät mallit ja komentosarjojen [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) säilö. 

* Siirry ohjeissa MATLAB ja MATLAB Distributed tietojenkäsittely palvelimen [MathWorksin sivuston](http://www.mathworks.com/) .
