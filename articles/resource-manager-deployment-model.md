<properties
   pageTitle="Resurssienhallinta ja perinteinen käyttöönoton | Microsoft Azure"
   description="Resurssien hallinnan käyttöönottomalli ja perinteinen erot kuvataan (tai palvelun hallinta) käyttöönottomalli."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/27/2016"
   ms.author="tomfitz"/>

# <a name="azure-resource-manager-vs-classic-deployment-understand-deployment-models-and-the-state-of-your-resources"></a>Azure Resurssienhallinta ja perinteinen käyttöönoton: ymmärtää käyttöönoton mallit ja resurssien tila

Tässä aiheessa kerrotaan Azure Resurssienhallinta ja perinteinen käyttöönoton tietokantamallien sekä resurssien tila sekä siitä, miksi resurssien on otettu käyttöön toisen kanssa. Resurssienhallinta ja perinteinen käyttöönoton mallien edustavat kahdella eri tavalla käyttöönotto ja Azure ratkaisujen hallinta. Voit käsitellä niitä eri API kahdet kautta ja otettu käyttöön resursseja voi olla eroja. Kaksi mallit eivät ole täysin yhteensopivia toistensa kanssa. Tässä ohjeaiheessa kerrotaan eroista.

Käyttöönotto- ja resurssien hallintaa helpottaa Microsoft suosittelee, että käytät Resurssienhallinta kaikki uudet resurssit. Jos mahdollista Microsoft suosittelee käyttöön aiemmin resurssien – Resurssienhallinta.

Jos olet uusi resurssi hallinta, haluat ehkä tarkistettava ensin määritetty [Azure resurssin hallinnassa: yleiskatsaus](azure-resource-manager/resource-group-overview.md)termejä.

## <a name="history-of-the-deployment-models"></a>Käyttöönotto-mallien historiatiedot

Azure annettu alun perin perinteinen käyttöönottomalli. Tämän mallin kullekin resurssille olivat itsenäisesti; oli ei voi ryhmitellä toisiinsa liittyvät resurssit. Sen sijaan oli manuaalisesti muodostuu ratkaisu tai sovelluksen resursseja seurata ja hallita niitä lähestymistavan muista. Ratkaisun käyttöönotto myi Luo kullekin resurssille yksitellen – perinteinen portaalin tai luoda komentosarjan, joka on otettu käyttöön kaikkien resurssien oikeassa järjestyksessä. Jos haluat poistaa ratkaisun, oli poistaminen yksitellen kullekin resurssille. Voit ei helposti käyttää ja päivittää access-ohjausobjektin käytännöt liittyvät resurssit. Lopuksi, et voi käyttää tunnisteita resurssien merkitä ne ehdot, joiden avulla voit seurata resurssien ja laskutuksen hallinta.

2014 Azure käyttöön Resurssienhallinta, joka lisätään resurssiryhmä käsite. Resurssiryhmä on resursseja, joilla on yhteiset elinkaari säilö. Resurssien hallinnan käyttöönottomalli on monia etuja:

- Voit ottaa käyttöön, hallita ja seurata ratkaisu kaikki palvelut ryhmänä sijaan käsittely nämä palvelut yksitellen.
- Voit ottaa ratkaisuja koko sen elinkaaren aikana ja resurssien otetaan käyttöön yhtenäinen tilaan luottaa toistuvasti.
- Voit käyttää kaikkien resurssien käyttöoikeuksien valvonta resurssiryhmän ja kyseiset käytännöt otetaan automaattisesti käyttöön, kun uusia resursseja resurssiryhmän lisätään.
- Voit käyttää tunnisteiden resurssien järjestäminen loogisesti kaikki tilauksen resurssit.
- JavaScript Object Notation (JSON) avulla voit määrittää infrastruktuurin ratkaisu. JSON-tiedosto on nimeltään Resurssienhallinta-malli.
- Voit määrittää resurssia, joten ne on otettu käyttöön oikeassa järjestyksessä väliset riippuvuudet.

Kun Resurssienhallinta on lisätty, kaikki resurssit on lisätty takautuvasti resurssin oletusryhmät. Jos luot resurssin – perinteinen käyttöönoton nyt, resurssi luodaan automaattisesti oletusarvon resurssin ryhmän palvelun, vaikka et ole määrittänyt resurssiryhmän käyttöönotto-palvelussa. Kuitenkin vain nykyisen resurssin ryhmän ei tarkoita, että resurssi on muunnettu Resurssienhallinta-malli. Tarkastellaan tapaa kunkin palvelun seuraavan osion kaksi käyttöönotto-malleista. 

## <a name="understand-support-for-the-models"></a>Tietoja malleista tuki 

Käyttöönottomalli-resursseissa valitsemisessa on kolme tapausta huomioon:

1. Palvelun tukee Resurssienhallinta ja sisältää vain yhden tyyppi.
2. Palvelu tukee Resurssienhallinta, mutta se on kahdenlaisia - yksi Resurssienhallinta ja yksi perinteinen. Tämä tilanne koskee vain näennäiskoneiden, tallennustilan tilit ja virtual käyttäminen.
3. Palvelu ei tue Resurssienhallinta.

Saat tietää, tukeeko palvelu Resurssienhallinta, katso [Resurssienhallinta tukee tarjoajan palveluun](resource-manager-supported-services.md).

Jos haluat käyttää palvelua ei tue Resurssienhallinta, sinun täytyy jatkaa käyttämällä perinteinen käyttöönotto.

Jos palvelun tukee Resurssienhallinta ja virtual tietokoneella **ei ole** , tallennustilan tilin tai VPN, voit käyttää Resurssienhallinta ilman mitään monimutkaisuuden.

Näennäiskoneiden, tallennustilan tilit ja virtual verkkoja, jos resurssi on luotu – perinteinen käyttöönoton, sinun täytyy jatkaa – perinteinen toiminnot toimivat. Jos virtuaalikoneen, tallennustilan tilin tai VPN luotiin Resurssienhallinta käyttöönoton kautta, sinun täytyy jatkaa Resurssienhallinta toimintojen avulla. Tunnukseen avata sekava tilauksen sisältävät resurssit luodaan Resurssienhallinta ja perinteinen käyttöönoton kautta. Tämä yhdistelmä resurssien voit luoda odottamattomia tuloksia, koska resurssit eivät tue samat toiminnot.

Joissakin tapauksissa Resurssienhallinta-komento voit hakea tietoa – perinteinen käyttöönoton luotu resurssi tai voi suorittaa järjestelmänvalvojan tehtäviä, kuten perinteinen resurssin siirtäminen toiseen resurssiryhmä. Mutta tällaisissa tapauksissa ei olisi avulla vaikutelman tyyppi tukee Resurssienhallinta-toimintoja. Oletetaan, että sinulla on resurssiryhmän, joka sisältää virtual laitteeseen, joka on luotu perinteinen käyttöönotto. Jos suoritat seuraavat Resurssienhallinta PowerShell-komennon:

    Get-AzureRmResource -ResourceGroupName ExampleGroup -ResourceType Microsoft.ClassicCompute/virtualMachines

Se palauttaa virtuaalikoneen:
    
    Name              : ExampleClassicVM
    ResourceId        : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.ClassicCompute/virtualMachines/ExampleClassicVM
    ResourceName      : ExampleClassicVM
    ResourceType      : Microsoft.ClassicCompute/virtualMachines
    ResourceGroupName : ExampleGroup
    Location          : westus
    SubscriptionId    : {guid}

Kuitenkin resurssien hallinnan cmdlet- **Get-AzureRmVM** palauttaa vain näennäiskoneiden käyttöön – Resurssienhallinta. Seuraava komento ei palauta virtuaalikoneen luotu – perinteinen käyttöönotto.

    Get-AzureRmVM -ResourceGroupName ExampleGroup

Vain resurssit luoda Resurssienhallinta tuki tunnisteiden avulla. Et voi käyttää tunnisteita perinteinen resurssit.

## <a name="resource-manager-characteristics"></a>Resurssienhallinta-ominaisuudet

Voi auttaa sinua ymmärtämään kaksi seuraavaksi tarkastellaan Resurssienhallinta tyypit ominaisuudet:

- Luoda [Azure portal](https://portal.azure.com/)avulla.

     ![Azure portal](./media/resource-manager-deployment-model/portal.png)

     Laske, varasto ja resurssien verkko-, sinulla on käyttää Resurssienhallinta tai Classic käyttöönotto. Valitse **Resurssienhallinta**.

     ![Resurssien hallinnan käyttöönotto](./media/resource-manager-deployment-model/select-resource-manager.png)

- Luotu Azure PowerShellin cmdlet-komennot Resurssienhallinta-versiolla. Nämä komennot ovat muodossa *Verbin AzureRmNoun*.

        New-AzureRmResourceGroupDeployment

- Luoda [Azure Resurssienhallinta REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) muiden toimintojen avulla.

- Luoda avulla Azure CLI komentoja **arm** -tilassa.

        azure config mode arm
        azure group deployment create 

- Resurssin laji ei ole **(perinteinen)** nimessä. Seuraava kuva esittää **tallennustilan**tiliksi tyyppi.

    ![Web App-sovelluksessa](./media/resource-manager-deployment-model/resource-manager-type.png)

## <a name="classic-deployment-characteristics"></a>Perinteinen käyttöönoton ominaisuudet

Sinun tiedettävä perinteinen käyttöönoton mallin hallinnan mallina.

Resurssit luodaan perinteinen käyttöönoton mallin jakaminen seuraavat ominaisuudet:

- Luotu – [perinteinen portal](https://manage.windowsazure.com)

     ![Perinteinen portal](./media/resource-manager-deployment-model/classic-portal.png)

     Tai Azure portaalin ja määrität **perinteinen** käyttöönoton (suorittaminen, varasto ja verkko).

     ![Perinteinen käyttöönotto](./media/resource-manager-deployment-model/select-classic.png)

- Luoda Azure PowerShellin cmdlet-komennot hallinta-version avulla. Komennon seuraavat nimet on muotoa *Verbin AzureNoun*.

        New-AzureVM 

- Luoda [Service Management REST API](https://msdn.microsoft.com/library/azure/ee460799.aspx) muiden toimintojen avulla.
- Luoda Azure CLI komennot **asm** tilassa avulla.

        azure config mode asm
        azure vm create 

- Resurssin laji sisältää **(perinteinen)** nimi. Seuraava kuva esittää tyyppi **tallennustilan**tiliksi (perinteinen).

    ![perinteinen tyyppi](./media/resource-manager-deployment-model/classic-type.png)

Voit hallita resursseja, jotka on luotu – perinteinen käyttöönoton Azure portaalin.

## <a name="changes-for-compute-network-and-storage"></a>Laske, verkon ja tallennustilaa muutokset

Seuraavassa kaaviossa näkyy suorittaminen, verkon ja tallennustilaa resursseja – Resurssienhallinta.

![Resurssienhallinta-arkkitehtuuri](./media/virtual-machines-azure-resource-manager-architecture/arm_arch3.png)

Huomaa seuraavat resurssien väliset suhteet:

- Kaikki resurssit ovat jo resurssin ryhmän.
- Virtuaalikoneen määräytyy määritettyä tallennustilan resurssi-palvelussa voit tallentaa sen levyjen blob storage (pakollinen) tietyn tallennustilan-tiliä.
- Virtuaalikoneen viittaa tiettyyn NIC, joka on määritetty resurssin verkkopalvelun (pakollinen) ja määritä Laske resurssi-palvelussa määritetyt (valinnainen) saatavuus.
- Verkkokortti viittaa virtuaalikoneen on määritetty IP-osoite (pakollinen), aliverkon virtual verkoston virtuaalikoneen (pakollinen) ja verkon käyttöoikeusryhmän (valinnainen).
- Virtuaalinen verkossa oleville aliverkon viittaa verkon käyttöoikeusryhmän (valinnainen).
- Kuormituksen tasauspalvelun esiintymän viittaa Taustajärjestelmä resurssivarantoon IP-osoitteet, jotka sisältävät NIC virtual koneen (valinnainen) ja viittaa on kuormituksen tasauspalvelun julkinen tai yksityinen IP-osoite (valinnainen).

Seuraavassa on osat ja niiden yhteydet perinteinen käyttöönottoa varten:

![perinteinen arkkitehtuuri](./media/virtual-machines-azure-resource-manager-architecture/arm_arch1.png)

Perinteinen isännöintiä virtual machine ratkaisu sisältää:

- Pakollinen pilvipalvelussa, joka toimii säilön isännöimiseen näennäiskoneiden (Laske). Verkkokortti (NIC) annetaan automaattisesti näennäiskoneiden ja Azure määrittämän IP-osoite. Lisäksi cloud-palvelu sisältää ulkoisen kuormituksen tasauspalvelun esiintymä julkiseen IP-osoite ja oletusarvon päätepisteet sallimaan Windows-pohjaisesta näennäiskoneiden remote työpöytä- ja PowerShell liikenteen ja Linux-pohjaiset näennäiskoneiden tietoliikenteen suojattu runko (SSH).
- Pakollinen tallennustilan-tili, joka tallentaa näennäiskiintolevyt virtual tietokoneelle, mukaan lukien käyttöjärjestelmän määräaikainen ja muita tietoja levyjen (tallennus).
- Valinnainen virtual verkkoon, joka toimii Lisää säilö, jossa voit luoda subnetted rakenteen ja määrittää aliverkon, jossa virtuaalikoneen on (network).

Seuraavassa taulukossa on kuvattu miten suorittaminen, verkon ja tallennustilaa resurssin tarjoajat vaikuttavat muutokset:

 Kohteen | Perinteinen | Resurssien hallinta
 ---|---|---
| Näennäiskoneiden pilvipalvelussa |  Käytössäsi on säilön asettamisen näennäiskoneiden, jotka tarvitaan käytettävyys-ympäristön ja kuormituksen tasaamisen. | Käytössäsi ei ole enää objekti vaaditaan Virtual-Machine uuden mallin luominen. |
| Virtuaalinen verkot | Virtuaalinen verkko on valinnainen virtuaalikoneen. Jos, virtual verkon ei voi ottaa käyttöön resurssien hallinta. | Virtuaalikoneen edellyttää virtual verkkoon, joka on otettu käyttöön resurssien hallinta. |
| Tallennustilan tilit | Virtuaalikoneen tarvitaan tallennustilan-tili, joka tallentaa käyttöjärjestelmille määräaikainen ja muita tietoja levyjen näennäiskiintolevyjen. | Virtuaalikoneen tarvitaan tallennustilan-tili, voit tallentaa sen levyjen Blob-objektien tallennustilaan. |
| Käytettävyys joukot | Käytettävyys ympäristö on merkitty määrittäminen saman "AvailabilitySetName" näennäiskoneiden. Vian toimialueiden enimmäismäärä on 2. | Käytettävyys on Microsoft.Compute tarjoajan näyttämiä resurssin. Näennäiskoneiden, jotka edellyttävät suuren käytettävyyden täytyy sisältyä käytettävyys määrittäminen. Vian toimialueiden enimmäismäärä on nyt 3. |
| Affiniteetti ryhmät | Affiniteetti ryhmät tarvittu Virtual verkkojen luomista varten. Kuitenkin alueellisten Virtual verkkojen myötä, joka ei tarvita enää. |Yksinkertaistaa, affiniteetti ryhmät-käsite ei ole ohjelmointirajapinnan tarjoamia kautta Azure Resurssienhallinta. |
| Kuormituksen    | Pilvipalveluun luominen on implisiittisesti kuormituksen näennäiskoneiden käyttöön. | Kuormituksen on Microsoft.Network-palvelun tarjoamista resurssi. Ensisijaisen verkon-käyttöliittymä, joka on oltava kuormitus tasataan näennäiskoneiden on viittaava kuormituksen. Kuormituksen tasoitusmääritykset voi olla sisäisiä ja ulkoisia. Kuormituksen tasauspalvelun-esiintymä viittaa Taustajärjestelmä resurssivarantoon IP-osoitteet, jotka sisältävät NIC virtual koneen (valinnainen) ja viittaa on kuormituksen tasauspalvelun julkinen tai yksityinen IP-osoite (valinnainen). [Lue lisää.](../articles/resource-groups-networking.md) |
|Virtuaalinen IP-osoite | Pilvipalveluihin tulee oletusarvon VIP (Virtual IP-osoite), kun AM lisätään pilvipalveluun. Virtuaalinen IP-osoite on implisiittisesti kuormituksen liittyvä sähköpostiosoite.    | Julkinen IP-osoite on Microsoft.Network-palvelun tarjoamista resurssi. Julkiseen IP-osoite voidaan staattiset (varattu) tai dynaaminen. Dynaaminen julkiseen IP-osoitteet voi määrittää kuormituksen tasauspalvelun. Julkiseen IP-osoitteet voidaan suojata käyttöoikeusryhmät. |
|Varattu IP-osoite|   Voit varata Azure-IP-osoite ja liittää sen pilvipalvelussa, jotta IP-osoite on muistilappuja.   | Julkinen IP-osoite voidaan luoda "Staattinen"-tilassa ja siinä on saman ominaisuuksien osoitteena"varattu IP". Staattinen julkiseen IP-osoitteet vain voi määrittää kuormituksen tasauspalvelun tällä hetkellä. |
|Julkiseen IP-osoite (PIP) AM kohden | Julkisten IP-osoitteiden myös voi liittää AM suoraan. | Julkinen IP-osoite on Microsoft.Network-palvelun tarjoamista resurssi. Julkiseen IP-osoite voidaan staattiset (varattu) tai dynaaminen. Kuitenkin vain dynaaminen julkiseen IP-osoitteet voi määrittää verkko-liittymän saat julkiseen IP kohti AM tällä hetkellä. |
|Päätepisteet| Syötteen päätepisteet tarvittavat on määritettävä Virtual tietokoneeseen avattava connectivity tietyt-porteille ylöspäin. Jokin valmis määrittämällä syötteen päätepisteet näennäiskoneiden muodostaa yleisiä tilasta. | NAT saapuvan liikenteen säännöt voidaan määrittää kuormituksen tasoitusmääritykset päästä samaan vain tietyt portit VMs muodostamisesta päätepisteet käyttöönottoon. |
|DNS-nimi| Pilvipalveluun saisit implisiittinen GUID DNS-nimi. Esimerkki: `mycoffeeshop.cloudapp.net`. | DNS-nimet ovat valinnaisten parametrien, joka voidaan määrittää resurssin julkiseen IP-osoite. Täydellinen toimialuenimi on seuraavassa muodossa - `<domainlabel>.<region>.cloudapp.azure.com`. |
|Verkon liityntäkohdat | Ensisijainen ja toissijainen verkon käyttöliittymässä ja sen ominaisuuksia on määritetty Virtual machine verkon määrittäminen. | Verkkoliittymän on Microsoft.Network tarjoajan näyttämiä resurssi. Verkko-liittymän elinkaari ei ole sidottu Virtual Machine. Se viittaa virtuaalikoneen on määritetty IP-osoite (pakollinen), aliverkon virtual verkoston virtuaalikoneen (pakollinen) ja verkon käyttöoikeusryhmän (valinnainen). |

Tietoja virtual verkostojen yhdistäminen eri käyttöönoton mallit-kohdassa [Yhdistä virtual verkkojen portaalissa eri käyttöönotto-malleista](./vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).

## <a name="migrate-from-classic-to-resource-manager"></a>Siirtää klassinen resurssien hallinta

Jos olet valmis siirtämään resurssien perinteinen käyttöönoton resurssien hallinnan käyttöönotto, lue:

1. [Tekniset perinpohjaisesti käsittelevään artikkeliin-ympäristö tuettu siirron-perinteinen Azure resurssien hallinta](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
2. [Tuetut käyttöympäristö siirto IaaS resurssit perinteinen Azure resurssien hallinta](./virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
3. [Siirtää IaaS resurssien perinteinen Azure resurssien hallinnan Azure PowerShell-toiminnolla](./virtual-machines/virtual-machines-windows-ps-migration-classic-resource-manager.md)
4. [Siirtää IaaS resurssien perinteinen Azure resurssien hallinnan avulla Azure CLI](./virtual-machines/virtual-machines-linux-cli-migration-classic-resource-manager.md)

## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

**Voinko luoda virtual kone käyttöönotto VPN tai tallennustilan tilin luotu perinteinen käyttöönoton Azure Resurssienhallinta avulla?**

Tämä ei tueta. Et voi käyttää Azure Resurssienhallinta ottamaan virtual machine virtual verkostoon, joka on luotu perinteinen käyttöönotto.

**Azure Resurssienhallinta käyttäjän kuvasta, joka on luotu Azure Service Management-ohjelmointirajapinnan käyttäminen virtual kone luominen**

Tämä ei tueta. Voit kuitenkin kopioida näennäiskiintolevytiedostoja tallennustilan tilin, joka on luotu Service Management API ja lisääminen kautta Azure Resurssienhallinta luomaan uusi tili.

**Mikä on tilauksen kiintiön vaikutus?**

Näennäiskoneiden, virtual verkkojen ja tallennustilaa tilit luotu kautta Azure Resurssienhallinta kiintiöt ovat erillään muista kiintiön. Jokaisen tilauksen saa kiintiön Luo uusi ohjelmointirajapinnan käyttäminen resursseja. Voit lukea lisää tietoja muita kiintiön [tähän](../articles/azure-subscription-service-limits.md).

**Voit jatkaa valmistelu näennäiskoneiden, virtual verkkojen ja tallennustilaa tilit Resurssienhallinta-ohjelmointirajapinnan Omat Automaattiset komentosarjat käytettäviä?**

Automaatio ja komentosarjoja, jotka olet aiemmin luonut aiemmin näennäiskoneiden virtual verkkojen luotuihin Azure hallinta-tilassa, toimivat edelleen. Kuitenkin komentosarjat on päivitettävä, jonka avulla voi luoda uuden rakenteen kautta Resurssienhallinta-tilassa samoja resursseja.

**Mistä löydän Azure Resurssienhallinta malliesimerkkien?**

Kattavaa starter malleja löytyy [Azure Resurssienhallinta pikaopas malleja](https://azure.microsoft.com/documentation/templates/).

## <a name="next-steps"></a>Seuraavat vaiheet

- Käydä läpi, joka määrittää virtuaalikoneen, tallennustilan tilin ja VPN-mallin luominen, katso [Resurssienhallinta mallin ongelmatilanteita](resource-manager-template-walkthrough.md).
- Komentojen käyttöönotto mallina, on artikkelissa [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](resource-group-template-deploy.md).
